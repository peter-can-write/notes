# Physische Optimierung

Waehrend die *logische* Optimierung die Optimierung des relational-algebraischen
Ausdrucks, der aus einer Anfrage entsteht, und das Erstellen eines QEP zum Ziel
hat, beschaeftigt sich die physische Optimierung nun mit der *Implementierung*
des QEP bzw. dessen einzelne Operationen. Es werden hier nun Implementierungen
fuer Joins, Selektionen und Duplikateliminierung vorgestellt.

## Iteratoren

Eine Variante zur Implementierung relational-algebraischer Operationen wie Joins
oder Schnitte sind *Iteratoren*. Dabei stellt ein Iterator eine einzige
Operation im Operatorbaum dar, und stellt folgendes Interface zur Verfuegung:

* Open: Initialisiert den Iterator mit seiner Eingabe (entweder Tupel oder
  andere Iteratoren). Dabei koennen bestimmte setup-Operationen durchgefuehrt
  werden, die der Iterator braucht, um andere Operationen wie `next` effizient
  auszufuehren.
* Next: Fordert wenn moeglich das naechste Element aus dem Iterator an.
* Close: Deinitialisiert den Iterator.
* Cost: Schaetzt die Kosten fuer die Operation.
* Size: Schaetzt die Groesse der Relation/dem Zwischenergebnis, auf welchem der
  Iterator iteriert.

  Es gibt fuer Iteratoren fuenf Gruppen:

  1. Selektion
  2. Binaere Zuordnung (Matching, z.B. bei Join)
  3. Gruppierung und Duplikateliminierung
  4. Projektion und Vereinigung
  5. Zwischenspeicherung

### Pipelineing

Pipelineing ist vergleichbar mit dem Konzept von Ranges in C++ oder Streams in
Java oder Generators in Python, generell also Coroutines. Anstatt eine Operation
nach der anderen sequentiell auf dem gesamten Datensatz durchzufuehren, versucht
man Operationen *lazily* bzw. *on-demand* auszufuehren, und der im Operatorbaum
naechst-hoeheren Operation eben nur das letzte, gerade eben ausgewertete Tupel
aus jeder Operation zu uebergeben. Somit koenenn die einzelnen Unteroperationen
parallel operieren und die Workload kann moeglichst parallel verteilt
werden. Sei z.B. der folgende relational-algebraische Ausdruck auszuwerten:
$\sigma_{p_1}(R) \,\theta\, \sigma_{p_2}(S)$, wo $\theta$ eine passende
Operation ist. Hier gibt es nun zwei moegliche Datenflussvarianten:

1. Selektiere alle gueltigen Tupel aus $R$.
2. Selektiere alle gueltigen Tupel aus $S$.
3. Gebe die Selektionen von $R$ und $S$ zur Vateroperation weiter.
4. Fuehre nun $\theta$ auf dem gesamten Datensatz durch.

Wobei (1) und (2) parallel oder nacheinander passieren koennen. Oder:

1. Solange weder $R$ noch $S$ erschoepft sind:
	1. Selektiere gleichzeitig das naechste Element aus $R$ und das naechste aus
       $S$.
	2. Gebe sie gleichzeitig an die Vateroperation weiter.
	3. Fuehre die Vateroperation auf diesen beiden Elementen durch und reiche
       sie nach oben.

Wobei Schritte 1/2 und 3 ebenso parallel laufen koennten.

#### Pipeline-Breaker

Pipeline-Breaker sind solche Operationen, die keine Parallelisierung erlauben,
also den gesamten Datenraum beobachten muessen. Solche Operationen sind
beispielsweise:

* Sortierung: `sort`
* Duplikateliminierung: `distinct`
* Aggregatoperationen: `min`/`max`/`sum`
* Mengendifferenz: `except`

## Selektion

Eine simple, lineare Implementierung der Selektion mit einem Iterator waere
die *brute-force* Variante, jedes Element einzeln anzusehen und das
Selektionspraedikat fuer dieses Element zu testen. Ein solcher Iterator hat
bzw. braucht also nur eine Eingabe. Dabei kann der Iterator mit oder ohne
Index arbeiten, also entweder auf einer unstrukturierten Ansammlung von
Elementen oder z.B. einem B-Baum:

a) Iterator `select_p`

* open: Oeffne die Eingabe.
* next: Fordere so lange Tupel an, bis eines das Praedikat erfuellt. Gibt es
        kein solches Tupel, terminiere.
* close: Schliesse die Eingabe.

b) Iterator: `index_select_p`

* open: Oeffne die Eingabe und suche sogleich das erste gueltige
        Tupel. Z.B. suche in einem $B^+$-Baum nach dem ersten Blatt, dass die
        Bedingung erfuellt, damit `next()` folglich in konstanter Zeit laufen
        kann.
* next: Suche nach dem naechsten gueltigen Tupel und gib es zurueck,
        terminiere wenn es keines mehr gibt.
* close: Schliesse die Eingabe.

Wir gehen hierbei dabei aus, dass das Praedikat die Eigenschaften des Index
ausnutzen kann. Ist `p` eine Aequivalenz ($=$) oder eine kleiner/groesser
Relation (nur eine), dann kann ein $B^+$-Baum durch die Sortierung der Elemente
nuetzlich sein.

## Binaere Zuordnungsoperatoren

Unter die Kategorie Binaere Zuordnungsoperatoren fallen Operationen wie Join,
Differenz oder Schnitt. Jeweils wird dabei fuer ein gegebenes Tupel ein anderes
passendes Tupel gesucht. Bei Joins werden dabei bestimmte Attribute verglichen,
bei Schnitten oder Differenzen ganze Tupel.

### Nested-Loop Variante

Ein Iterator koennte dabei so implementiert sein:

`iterator nested_loop_p`
	* open: Oeffne die linke Eingabe.
	* next:
	  + Fordere das naechste Tupel aus der linken Eingabe an.
	  + Falls die rechte Eingabe geschlossen ist, oeffne sie.
	  + Fordere aus der rechten Eingabe so lange Tupel an, bis $(\text{links},
        \text{rechts})$ die Bedingung erfuellen.
	  + Sollte die rechte Eingabe ohne passenden Partner erschoepfen, rufe
        `next` erneut auf.
	  + Findet sich ein passendes Paar, fuege es zum Resultat hinzu, und
        suche ein weiteres passendes rechtes Element.
	* close: Schliesse beide Eingabequellen.

```python
result = []
left.open()

for i in left:
	if right.is_closed():
		right.open()
	for j in right:
		if predicate(i, j):
			result.append((i, j))
left.close();
right.close()
```

Auch wenn man komplexere Join-Implementierungen erlaubt, wie sie weiter unten
beschrieben werden, braucht ein DBMS auch immer eine Nested-Loop
Implementierung. Denn fuer manche Join-Praedikate koennen werder exakt-match
noch sortierte Joins helfen. Zum Beispiel bei $R \bowtie_{A \neq B} S$ kann man
weder *genau* nach einem Partner fuer jedes $A$ suchen, noch irgendeine
Sortierung ausnutzen.

#### Verfeinerung

Eine kleine Optimierung des *nested-loop*-Verfahren ist es, den Hauptspeicher
bestehend aus $m$ Seiten in zwei Mengen $X, Y$ zu partitionieren, wobei also
$|X| + |Y| = m$. Seien dann $R$ und $S$ die zu vergleichenden Relationen. Dann
fetched man zuerst $X$ Tupel aus der ersten Relation und legt sie im
Hauptspeicher ab. Dann holt man sich kontinuierlich alle Teilmengen von $S$ mit
Kardinalitaet $|Y|$ und vergleicht jedes mal jedes Element aus $X$ mit jedem aus
$Y$. Wurde die letzte Teilmenge $Y$ geholt und diese mit $X$ vollstaendig
durchiteriert, kommt die Optimierung: Beim holen des naechsten $X$ bleibt die
selbe Menge $Y$ im Hauptspeicher, und man iteriert uber $Y$ dann einfach
rueckwaerts. Dadurch spart man sich einen Fetch von $Y$ pro Fetch von $X$.

```
[X_1][X_2][X_3]
-->---->---->--

/--<----<----<--\
\-->---->---->--/
 [Y_1][Y_2][Y_3]
```

### Sortierung

Eine effizientere Methode zum Durchfuehren eines Joins ist der sogenante
*Merge-Join* oder *Sort-Merge-Join*. Dabei werden die Tupel nach den relevanten
Attributen zuerst nicht-absteigend (nondecreasing) sortiert ($O(N \log
N)$). Danach werden die beiden Relationen parallel sequentiell
abgestiegen. Seien $R, S$ nun zwei Relationen, fuer welche $R \bowtie_{R.A=S.B}
S$ durchgefuehrt werden soll, wo also $A$ ein Attribut von $R$ ist und $B$ ein
Attribut von $S$. Dann braucht man jeweils noch einen Zeiger $z_r$ auf das
momentane Element in $R$ und einen Zeiger $z_s$ auf das momentane Element in
$S$.

Im einfachsten Fall liegen weder in $R$ noch $S$ Duplikate vor. Dann ist der
Algorithmus wie folgt:

1. Solange sowohl $z_r$ als auch $z_s$ gueltig sind: Vergleiche $*z_r$ mit
   $*z_s$. Falls gilt:
  1. Sie sind gleich, dann fuege sie zur Relation hinzu und inkrementiere beide.
  2. $*z_r$ ist kleiner. Da $S$ aufsteigend sortiert ist, waere $*z_r$ auch
      kleiner als alle Elemente in $S$ nach $z_s$. Daher inkrementiere $z_r$.
  3. $*z_r$ ist groesser. Da $R$ aufsteigend sortiert ist, waere $*z_s$ auch
      kleiner als alle Elemente in $R$ nach $z_r$. Daher inkrementiere $z_s$.

 ```
r = 0
s = 0
result = []
while r < len(R) and s < len(S):
	if r < s:
		r += 1
	elif r > s:
		s += 1
	else:
		result.append((r, s))
		r += 1
		s += 1
```

Im allgemeinen Fall koennen jedoch sowohl in $R$ als auch $S$ Duplikate
vorkommen. Im schlimmsten Fall ist $R.A = \{x\} = S.B$, sprich alle Tupel in $R$
und $S$ muessen miteinder gejoined werden -- ein Kreuzprodukt. Im Durchschnitt
werden aber nur einige Elemente/Tupel gleich sein. Dabei muss man jedoch
beachten, dass man zum ersten Element einer Aequivalentsklasse (bzw. hier einer
Gleichheitsklasse) in $S$ stets einen Zeiger behaelt. Denn hat man fuer ein
Element $r \in R$ alle Elemente $s \in S: r = s$ durch-iteriert, und stoesst
dann auf ein $r' \in R: r' = r$, dann muss man eben fuer alle $s$ die gleich $r$
waren nochmals Resultate produzieren. Sei $z_{s_0}$ ein Zeiger auf das erste
Element $\in S$ in einer Gleichheitsklasse. Dann:

1. Solange sowohl $z_r$ als auch $z_s$ gueltig sind:
   1. Falls $*z_r = *z_{s_0}$, setze $z_s$ zurueck zu $z_s$.
   2. Vergleiche $*z_r$ mit $*z_s$. Falls gilt:
	  1. Sie sind gleich, dann speichere einen Zeiger $z_{s_0}$, der auf dieses
         $*z_s$ zeigt. Dann, solange gilt $*z_r = *z_s$:
			1. Fuege die beiden zum Resultat hinzu.
			2. Inkrementiere $z_s$.
	  2. $*z_r < *z_s$, dann inkrementiere $z_r$.
	  3. $*z_r > *z_s$, dann inkrementiere $z_s$.

```
r = 0
s = 0
s_0 = 0
result = []
while r < len(R) and s < len(s):
	if r == s_0:
		s = s_0
	if r < s:
		r += 1
    elif r > s:
		s += 1
    else:
        s_0 = s
	    while r == s:
		    result.append((r, s))
		    s += 1
	    r += 1 # s already incremented
```

Die Komplexizitaet von diesem Verfahren ist im Durchschnitt also $O(N \log N)$
fuer das Sortieren und dann $O(N)$ fuer das Iterieren -- also $O(N \log N + N) =
O(N \log N)$. Im worst-case ist das Iterieren $O(N^2)$, wenn eben nur ein
einziges Element in $R.A$ und $S.B$ ist.

## Index-Join

Der Index-Join ist sehr leicht zu erklaeren. Falls wir den Join $R
\bowtie_{R.A=S.B} S$ berechnen wollen, und schon einen Index auf $R.A$ oder
$S.B$ haben, dann koennen wir diesen Index natuerlich verwenden. Vorausgesetzt,
es ist die richtige Art von Index. Die "richtige Art" haengt hier von der
Join-Bedingung ab. Bei Equi-Joins ohne Duplikaten, also z.B. wenn $S.B$ ein
Schluesselattribut ist, ist ein Hash-Index nuetzlich. Bei Joins mit Duplikaten
oder bei den Relationen $<,>,\leq,\geq$ waere ein $B^+$-Baum von Vorteil. Die
exakte Implementierung des Iterators haengt dann auch von der Art des Index
ab. Hier die Implementierung fuer einen $B^+$-Baum:

1. Oeffne linke Eingabe.
2. Solange $z_r$ gueltig ist:
   1. Hole naechstes zu $*z_r$ passendes Element $*z_s$ aus dem $S$-Index.
      2. Iteriere im Index (-> $B^+$-Baum) solange, wie $*z_r = *z_s$. Fuege
	     jeweils das passende Tupel $(*z_r, *z_s)$ zum Resultat hinzu.
	  3. Inkrementiere $z_r$.
3. Schliesse die Eingabe.

## Hash-Join

Index-Joins mit B-Baeumen sind nicht immer ideal, da B-Baeume nicht immer in den
Hauptspeicher passen. Das Anlegen von temporaeren Datenstrukturen wie
Hash-Tabellen oder Baeumen fuer die Berechnung einer Anfrage ist allgemein dann
nur sinnvoll, wenn der Index in den Hauptspeicher passt. Bei einer groesseren,
Hashtabelle geht man davon aus, dass jeder Zugriff auf ein Datum in einem
Seitenzugriff resultiert -- da Ballung bei Hash-Tabellen ausgeschlossen ist. Ist
die Hashtabelle aber klein genug, um in den Hauptspeicher zu passen, dann lohnt
es sich, ein Hash-Verfahren anzuwenden.

Die Idee des Hash-Joins besteht darin, beide Relationen mittels Hashfunktionen
in bestimmte Klassen zu partitionieren, und dies rekursiv solange, bis die
Klassen fein genug sind und (auf zufriedenstellende Weise) in den Hauptspeicher
passen. Das Resultat ist dann, dass man fuer jedes Element aus der einen
Relation nicht mehr, wie beim nested-loop, *jedes* andere Element aus der
anderen Relation vergleichen muss, sondern nur jene Elemente aus der anderen
Relation, die in der selben Partitionsklasse liegen, wobei die Partitionsklassen
fuer die beiden Relationen getrennt (auf Seiten) gespeichert sind.

### Build Phase

Die kleinere Relation wird als *Build*-Input genommen, da diese spaeter in
Hashtabellen gespeichert werden (diese Struktur ist komplexer und weniger
dynamisch, also ist es vorteilhaft die Relation mit mehr Elementen in "Bags" zu
halten und die kleinere Relation in Hash-Tabellen). Nun gibt es in diesem
Verfahren weiters eine Menge von Hashfunktionen $H = \{h_1, ..., h_n\}$, wobei
die $i$-te Hashfunktion in der $i$-ten Iteration ein Datum bzw. seinen
Schluessel auf einen Index (fuer eine Klasse) abbildet. Weiters wird das
Verfahren so angeordnet, dass von $m$ freien Hauptspeicherseiten $m - 1$ als
temporaere Klassen verwendet werden, und eine als Inputseite. Der Sinn der
temporaere Klasse ist, erst wenn die temporaere Klasse voll ist die
Zugriffsluecke zum Hauptspeicher zu ueberqueren und die Daten in den
endgueltigen Klassen zu hinterlegen.

Nun werden also seitenweise Daten aus dem Build-Input in diese Inputseite
geladen. Dann wird jedes Element in der Inputseite durch die Hashfunktion $h_1$
gegeben. $h_1(x)$ gibt dann fuer den Schluessel $x$ den Klassenindex
an. Folglich wird das Element in eine der $m - 1$ temporaeren Klassen im
Hauptspeicher gegeben. Ist eine der $m - 1$ temporaeren Klassen voll, so muss
die Zugriffsluecke also ueberwunden werden und die Daten werden in die
endgueltige Klasse im Hintergrundspeicher uebergeben. Am Ende werden dann auch
zu dem Zeitpunkt nicht volle temporaere Klassen ausgelagert.

Sind die Klassen nun noch nicht fein genug, "rekursiert" man fuer jede der
vorhin kreeirten Klasseen auf selbe Weise. Man laedt die Klasse seitenweise in
die Inputseite (eine Klasse kann ja -- zu dem Zeitpunkt noch -- mehrere Seiten
im Hintergrundspeicher belegen, wenn sie vorher mal voll wurde), und wendet nun
__die Hashfunktion $h_2$ an__, um die Klasse auf wiederum $m - 1$ kleinere
Buckets zu hashen.

Nach einer gewissen Anzahl an Iterationen terminiert man also. In den einzelnen
Klassen sind nun (je nach Parametern natuerlich) noch relativ viele verschiedene
Elemente, da die Tupel ja nur auf $m - 1$ Klassen aufgeteilt wurden. Hat man
z.B. die Schluessel-Multimenge $[300]$ (sei das eine Multimenge mit mindestens
den Zahlen $1...300$, die aber noch Duplikate der Zahlen enthalten kann) und die
Hashfunktion $h(x) = x/100$ wo $m - 1 = 3$, dann sind in den drei Klassen
jeweils noch mindestens $100$ Elemente enthalten. Erst in der endgueltigen
Hashtabelle, in der die Klassen im letzten Schritt dann geladen werden, wird
eine feinere Hashfunktion, z.B. $h(x) = x \mod 100$ verwendet.

Nun (oder vielleicht parallel) wird das exakt selbe fuer die groessere Relation
-- dem *Probe*-Input -- gemacht, mit den selben Hashfunktionen und selber
Iterationsanzahl. Das Resultat sind also Paare von Klasseen wobei eine
Klasse eben zum Probe- und eine zum Build-Input gehoeren. Eine Klasse ist
jeweils das Resultat der Funktionskomposition $\ocircle_{i=0}^n h_i$ von $n$
Hashfunktionen in $n$ Iterationsschritten.

### Join Phase

Jetzt kann das eigentliche Join-Verfahren ausgefuehrt werden. Es wird eine
gewisse Anzahl an Klassen (jeweils Seiten) aus dem *Build-Input* in
Hashtabellen mit der letzten Hasfunktion $h_n$ im Hauptspeicher organisiert. Dann
wird die gleiche Anzahl an Klassen aus dem Probe-Input geladen. Fuer jedes
Paar an Klassen geht man nun also durch jedes Element im Probe-Input durch
und wendet die Hashfunktion $h_n$ auf jedes Element an. Natuerlich gibt es in
den Hashtabellen sogar mit sehr feinen und gleichmaessig verteilenden
Hashfunktionen noch Kollisionen. In einer Separate-Chaining Variante wuerde man
also die jeweilige Kette durchsuchen muessen. Eine gewisse lineare Suche muss
sowieso gemacht werden, da ja Duplikate erlaubt sind und ein Join fuer jedes
Tupel alle passenden Tupel joinen muss.

Ein Hash-Join-Iterator wuerde den ganzen Setup also in seiner `open` Funktion
machen, und die Funktion `next` wuerde dann eben einen Schluessel aus dem
Probe-Input gegen den Hash-Table seiner Klasse hashen und den erste (noch
nicht benutzten) Joinpartner waehlen.


```
                 h_1     h_2    Hashtables    h_2      h_1
                  |       |         |          |        |
                  |  / [P_1.1]--->[HT_1]--[P_1.1] \   |
               _[P_1]--[P_1.2]--->[HT_2]--[P_1.2]-- [P_1]_
              /   |  \ [P_1.3]--->[HT_3]--[P_1.3] /   |   \
			 /    |       |         |        |        |    \
[Probe Input]     |       |         |        |        |     [Build Input]
             \    |  / [P_2.1]--->[HT_4]--[P_2.1] \   |    /
              \_[P_2]--[P_2.2]--->[HT_5]--[P_2.2]-- [P_2]_/
			         \ [P_2.3]--->[HT_6]--[P_2.3] /
					           ^
						     Probe
```

## Gruppierung und Duplikateliminierung

Gruppierung (`group by`) und Duplikateliminierung sind so wie Join und
Differenz/Schnitt. Gruppierung bezieht sich wie Joins nur auf eine einziges
Attribut, waehren sich Duplikateliminierung wie Schnitte und Differenzen auf
ganze Tupel beziehen.

Die Methoden zur Implementierung sind wie bei Joins und Differenzen/Schnitten:
1. Nested-Loops
2. Sort/Merge Varianten
3. Hash-Varianten

### Projektion und Vereinigung

Da bei der "physischen Algebra" (SQL) keine Duplikateliminierung bei Projektion
und Vereinigung durchgefuehrt wird, ist eine Projektion nichts anderes als ein
linearer Lauf ueber die Daten wobei jedes Tupel auf die zu projezierenden
Attribute reduziert wird. D.h. der Iterator geht einfach ueber die Tupel drueber
und gibt beim naechsten Call zu `next` die gewuenschten Attribute des momentanen
Tupels wieder. Bei der Vereinigung (da nicht auf Duplikateliminierung geachtet
werden muss), gibt man einfach zuerst die Tupel der einen Menge und dann der
anderen aus.

## Zwischenspeicherung

In manchen Faellen kann es sein, dass es effizienter ist, Ergebnisse
zwischenzuspeichern, als sie voll pipelined zu berechnen. Gegeben z.B. folgende
Anfrage:

```
   join
   /  \
 join  C
 /  \
A    B
```

In einer nested-loop Implementierung:

```python
for i in C:
	for j in join(A, B):
		if joinable(i, j):
			result.append((i, j))
```

Hier wuerde fuer jedes $i$ der Join $A \bowtie B$ neu berechnet werden
muessen. Also ist es natuerlich effizienter, diesen Join zwischenzuspeichern --
in einen Bucket.

Ein Bucket ist also ein "Auffangbecken" im Hintergrundspeicher, auf die andere
Iteratoren zugreifen koennen.

Eine weitere Anwendungsmoeglichkeit von Buckets sind Anfragen, in welchen
bestimmte Ausdruecke mehrmals vorkommen. Diese kann man also einmal berechnen
und in einen Bucket speichern, worauf die Iteratoren (der Operationen, in
welchen der Ausdruck eben mehrmals vorkam) dann schliesslich zugreifen koennen.


```
     ?
    / \
   /   \
  ?     ?
 / \   / \
A   X X   B
```

Hier ist der Ausdruck $X$ also mehrmals enthalten (wuerde hier jedes Mal einem
`scan` entsprechen). Man koennte ihn also nur einmal auswerten, in einen Bucket
speichern, und mehrmals verwenden, dann sieht der Graph so aus:

```
     ?
    / \
   /   \
  ?     ?
 / \   / \
A    X    B
```

Hier wird $X$ also nur einmal ausgewertet.

Ebenso kann es nuetzlich sein, diese Zwischenergebnisse weiter zu bearbeiten,
z.B. indem man sie sortiert oder in einen Index, sei es ein B-Baum oder eine
Hash-Tabelle, gibt.

## Externes Sortieren

Relationen enthalten ueblicherweise mehr Tupel, als im Hauptspeicher Platz
ist. Daher sind Sortierverfahren wie Quicksort, welche den gesamten Datenraum
sehen muessen, nicht anwendbar. Jedoch eignen sich Varianten von Mergesort
hervorragend dafuer.

Die Grundidee hinter Mergesort liegt darin, die Menge an Daten in sortierte
Stuecke -- hier sogenante *Laeufe* (engl. *runs*) -- aufzuteilen, und diese dann
mit einem *Merge*-Verfahren rekursiv in ein einziges sortiertes Stueck zu
reduzieren. Es gibt bei diesem Mergesort im Vergleich zum herkoemmlichen
Mergesort einige Unterschiede.

Zum Ersten werden die Daten nicht vollkommen rekursiv auf nur zwei Elemente
reduziert. Eher werden die Daten in Stuecke einer bestimmten Minimalgroesse
aufgeteilt, und diese einzelnen Stuecke werden dann in einem Hauptspeichersort
wie Quicksort in Ordnung gebracht. Diese minimalen Lauefe werden *Level-0
Laeufe* genannt und werden in temporaeren Relationen auf dem Plattenspeicher
zurueckgespeichert.

Stehen dem gesamten Verfahren $m$ Seiten zur Verfuegung, wird der gesamte
Datenraum (der Relation) bestehend aus $M$ Seiten also in $M/m$ Stuecke
aufgeteilt, und diese einzelnen Laeufe, jeweils aus $m$ Seiten bestehend, werden
dann mit Quicksort (zum Beispiel) sortiert.

Dann beginnt das eigentliche Mischen bzw. *Mergen*. Allgemein versucht man die
Anzahl an Stufen, also "Levels", bzw. ultimativ die Anzahl an Merges zu
minimieren, also sollte man pro Merge die groesstmoegliche Anzahl an Laeufen
auswaehlen. Fuer jeden Merge stehen also noch immer die selben $m$ Seiten im
Hauptspeicher zur Verfuegung. Von diesen duerfen nun $m - 1$ Seiten fuer den
Input aus den Laeufen verwendet werden, und die letzte Seite ist das
Auffangbecken fuer die Ausgabe, welche das Resultat des Merges beinhaltet und
beim Vollwerden in den Plattenspeicher schickt.

Nun werden also maximal $m - 1$ der $M/m$ Laeufe ausgewaehlt (Erinnerung: Ein
Lauf besteht aus $m$ Seiten und ist ueber diese hinweg sortiert). Aus jedem der
$m - 1$ Laeufen wird die Seite mit den kleinsten Werten in den Hauptspeicher
geladen. Nun nimmt man aus jedem der $m - 1$ Seiten im Hauptspeicher jeweils das
kleinste Element, und gibt diese in eine Heap (der Groesse $m - 1$). Der Vorteil
des Heaps ist es, dass das kleinste der $m - 1$ Elemente, welches also als
naechstes in die Ausgabe soll, in logarithmischer Zeit ($\log_2 m - 1$) von
der Spitze des Heaps geholt werden kann (konstant um es zu holen, logarithmisch
um dann das naechst kleinere Element an die Spitze zu geben). Dieses kleinste
Element wird dann in die Ausgabe gegeben. Nun ist es wichtig, sogleich das
naechst kleinere Element aus der Seite zu holen, von welcher das eben genommene
(kleinste) Element stammte. Dies wird nun solange wiederholt, bis eine der $m -
1$ Seiten im Hauptspeicher leer ist. Dann wird die naechste Seite aus dem
jeweiligen Lauf vom Plattenspeicher geladen. Und dies geht dann insgesamt
solange, bis alle Seiten aller Laeufe verarbeitet wurden.

Exkurs: Heap vs. Array: Fuer Array muss man in $O(N)$ die Elemente laden, dann
in $O(N \log N)$ sortieren, dann in $O(N)$ iterieren. Fuer Heap muss man in $O(N
\log N)$ laden/sortieren, und in $O(N \log N)$ iterieren (pop = $\log N$). Somit
fuer Array: $O(N + N \log N + N)$ und fuer Heap: $O(2 \cdot N \log N + N)$. Man
koennte also meinen das Array waere besser. Aber, man muss ja Elemente immer
nachfuellen, und ein neues Element in ein sortieres Array zu geben ist $O(\log N
+ N)$ waehrend es fuer einen Heap $O(\log N)$ ist. Also wenn man fuer jede
Iteration die Kosten des Replacements betrachtet, dann ist iterieren fuer
Arrays nicht $O(N)$ sondern $(N^2)$ bzw. $O(N(\log N + N)) = O(N \log N +
N^2)$, waehrend es fuer die Heap noch immer $O(N \log N)$ ist (replace_top
koennte sein: swap minimum mit naechstem Element, then sink the new one =
$O(\log N)$).

### Vergroesserung der Laeufe

Man moechte die Anzahl an Merges reduzieren, und daher die "Hoehe" des
Merge-Verfahrens. Eine wichtige Moeglichkeit dazu ist es natuerlich, die Level-0
Lauefe so gross wie moeglich zu machen. Hat man nur zwei Laeufe, die jeweils die
halbe Relation in sortierter Ordnung beinhalten, dann muss man nur einmal
mergen. Hierbei sind die Kosten aller Merges auf einem Level insgesamt
natuerlich linear. Hat man 32 kleine Laufe, muss man erst 4 mal auf vier Levels
linear mergen, um auf zwei Laufe zu kommen -- also vier mal (linear!)
oefter. Deswegen will man die *initialen* Level-0 Laufe also in ihrer Groesse
maximieren. Hierbei zur Erinnerung, dass man nicht einfach mehr Elemente am
Anfang sortieren kann, es haben ja nicht mehr als $m$ Seiten im Speicher Platz.

Ein Algorithmus dafuer, die Initialgroesse zu vergroessern, ist der sogenannte
Replacement-Selection-Algorithmus, eine Variante von Heap-Sort. Hierbei werden
zuerst die Elemente der $m - 1$ erlaubten Seiten in eine Min-Heap gegeben, also
quasi sortiert. Nun werden aber nicht gleich alle $m - 1$ Seiten wieder sortiert
in den Hauptspeicher zurueckgespeichert, und die naechsten $m$ geholt. Sondern,
der Algorithmus ist dann wie folgt:

1. Entnehme das kleinste Element der $m - 1$ Seiten von der Min-Heap.
2. Gib es in die Ausgabeseite.
3. Nehme das naechste Element in der Eingabe Queue (es werden also
   *seitenweise* neue Elemente aus dem Plattenspeicher geholt).
   3.1 Ist das Element kleiner als alle bisher in die Ausgabe geschriebenen
       Elemente, dann gib es ans Ende der Heap oder in eine andere Liste
	   -- "sperre" es jedenfalls fuer den restlichen Lauf.
   3.12 Ist das Element groesser oder gleich dem groessten bisher ausgegebenen
        Element, dann fuege es in die Heap ein, sodass es fuer die naechsten Werte
        des Laufes in Frage kommt.
4. Wiederhole solange, bis alle Plaetze auf den $m - 1$ Seiten gesperrt sind.

Nach dieser ersten Iteration des Algorithmus hat man (moeglicherweise) einen
sortierten Lauf, der viel laenger ist, als die urspruengliche Anzahl an
Elementen auf den $m - 1$ sortierten Seiten, da manche Elemente aus der Eingabe
zwischendurch eingefuegt wurden. Weiters liegt der naechste Lauf nun auch schon
passend in einer Heap auf den $m - 1$ Seiten im Hauptspeicher, fuer die naechste
Iteration des Algorithmus.

```
Ausgabe              | Speicher             | Eingabe
---------------------|----------------------|---------------------
                     |  10   20   30   40   | 25 73 16 26 33 50 31 40
		          10 |  20   25   30   40   | 73 16 26 33 50 31 40
	           10 20 |  25   30   40   73   | 16 26 33 50 31 40
            10 20 25 | (16)  30   40   73   | 26 33 50 31 40
         10 20 25 30 | (16) (26)  40   73   | 33 50 31 40
      10 20 25 30 40 | (16) (26) (33)  73   | 90 31 40
   10 20 25 30 40 73 | (16) (26) (33)  90   | 31 40
10 20 25 30 40 73 90 | (16) (26) (31) (33)  | 40
```

Hier sind die eingeklammerten Eintraege gesperrt. Wie man sieht ist der
entstandene Lauf in der Ausgabe drei Elemente groesser als der Lauf der
entstanden waere, haette man nach dem Sortieren sofort die Elemente als fertigen
Lauf in den Plattenspeicher geschrieben. Nach Knuth fuehrt Replacement-Selection
im Durchschnitt zu einem doppelt-grossen Lauf. Auch sieht man, dass der
restliche Lauf schon sortiert ist (bzw. in einer Heap ist).

Spekulation: Eine moegliche Implementierung waere es, gesperrte Elemente ans
Ende der Heap (also des Arrays) zu geben, um nur ein Array allokieren zu
muessen. Dann koennte man zwei Heaps gleichzeitig im selben Array habe, vorne
die fuer den momentanen Lauf, und hinten die mit den gesperrten Elementen fuer
den naechsten Lauf. Andernfalls koennte man es auch gleich in eine neue Heap
fuer den zweiten Lauf geben, aber dann braucht man zwei Heaps, also zwei
statische Arrays mit Platz fuer $m - 1$ Seiten, was ja nicht geht.

Somit kann man erreichen, dass die Level-0 Laufe groesser sind und man somit
weniger Levels im Merge-Verfahren hat.

### Komplexizitaet des Externen Sortierens

Grundsaetzlich sollte der Algorithmus noch immer linearithmisch sein. Das
Quasi-HeapSort Verfahren fuer die Level-0 Runs ist noch immer $O(N \log
N)$. Jedoch hat es den Vorteil, dass die Runs laenger werden koenne, als bei
normalem Quicksort. HeapSort wird ja allgemein nicht im Alltag angewandt, da es
sehr schlechte Locality of Reference, also zu viele Cache-Misses, hat, wenn man
so wild um $2k + 1$ Indices zum Kind spring oder $k/2$ zum Parent. Gleichzeitig
ist es fuer Datenbankensysteme hier aber besser, laengere Runs zu haben, da das
Mergen jeden Runs viele Zugriffe auf den Plattenspeicher bedeuten. Man tauscht
also die schlechten Seiten von HeapSort fuer quasi-gratis laengere Runs, welche
ultimativ in weniger Merges/Levels resultieren.

Im Durschnitt sind die generierten Runs 2x groesser als urspruenglich, man spart
sich also einen Merge-Pass.
