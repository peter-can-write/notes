# Kostenmodelle

Im Abschnitt zu logischer Optimierung wurden schon Heuristiken vorgestellt, die
einen Ausdruck in relationaler Algebra schon um einiges effizienter machen
koenenn, als er in seiner kanonischen Ubersetzung aus SQL noch war. Diese
Heuristiken sind aber auch nur Heuristiken, also nicht "quantifizierbar".

Ein modernes Datenbanksystem wird daher sogenannte *Kostenmodelle*
instandhalten, mit denen es einen Query-Evaluation-Plan (QEP) optimieren
kann. Ein Kostenmodell erhaelt dabei einige Informationen ueber die Anfrage und
den Zustand der Datenbank, um somit zu berechnen, welche Implementierungen fuer
eine konkrete Operation am effizientesten sind. Ein Kostenmodell wird hierfuer
als Eingabe erhalten:

1. Die Anfrage als algebraischer Ausdruck.
2. Informationen ueber die Existenz von Indices auf Relationen.
3. Informationen ueber die Ballung von Daten.
4. Kardinalitaeten der Relationen in der Datenbank.
5. Verteilungen der Attributwerte.

Hieraus generiert ein Kostenmodell dann die *Kosten* einer Operation, in einer
internen Einheit. Das DBMS kann dann die Kosten verschiedener Implementierungen
und Operationen geneinander abwaegen und versuchen, so die effizienteste
Uebersetzung des algebraischen Ausdrucks der Anfrage in die *physische Algebra*
zu erreichen.

Die *physische Algebra* bezeichnet hierbei eine neue Algebra, mit welcher ein
DBMS konkret arbeiten kann. Als Traegermenge hat diese Algebra Relationen, aber
als Operationen nun nicht mathematische Funktionen wie *Kreuzprodukt*, *Join*
oder *Selektion*, sondern wirklich Operationen wie *IndexJoin*, *Select* oder
*Sort* (vor einem *MergeJoin*). Dieser Auswertungsplan (QEP) kann dann vom DBMS
kompiliert oder interpretiert werden, z.B. in C++.

## Selektivitaeten

Im Rahmen von Kostenmodellen moechten wir bei Selektionen $\sigma_p$ sowie
natuerlichen Verbuenden $\bowtie$ und Theta-Joins $\bowtie_p$ (da diese auch
selektieren) die *Selektivitaet* dieser Operationen analysieren. Die
*Selektivitaet* $sel_p$ bezieht sich hierbei darauf, welcher Anteil der
Eingabe-Tupeln durch die Bedingung (der Selektion oder des Joins) eliminiert
wird. Je groesser die Selektivitaet, desto besser fuer uns, da wir weniger Tupel
speichern muessen. $sel$ wird heirbei auf zwei Weisen berechnet:

* Fuer Selektionen: $sel_p := \frac{\sigma_p(R)}{R}$. Welcher relative Anteil an
  Tupeln wird selektiert?

* Fuer Joins: $sel_{RS} := \frac{|R \bowtie S|}{|R \times S|}$. Welcher Anteil
  an Tupeln findet Join-Partner, relativ zum Kreuzprodukt?

Gerne haetten wir also fuer jede solche Operation im relational-algebraischen
Ausdruck diesen Wert. Dann muessen wir nur die selektivsten Operationen waehlen
und sparen uns redundante Speicherung. Natuerlich koennen wir hier aber nicht
jede dieser Operationen zuerst ausfuehren, sonst haette das ganze Kostenmodell
natuerlich keinen Sinn. Wir muessen diese Selektivitaeten also irgendwie
*abschaetzen*.

### Abschaetzungen

Einfache Abschaetzungen waeren:

* Wenn wir Schluesselattribute (also `primary key` oder `unique`) mit einer
  Konstante vergleichen, kann dabei maximal ein Tupel selektiert werden, da
  solche Schluesselattribute ja niemals Duplikate haben. Also ist $|R \bowtie S|
  = 1$. Somit wird die Selektivitaet immer $1/|R|$ sein.

* Wenn wir wieder so ein Schluesselattribut $A$ haben, und es als Join-Attribut
  bei einem Join $R \bowtie_{R.A = S.B}$ benutzten, dann ist $|R \bowtie S| \leq
  |S|$. Man ueberlege sich: Diese Attributwerte von $A$ besitzen keine
  Duplikate. Wenn also ein $A$ einen Joinpartner in $S.B$ findet, dann gibt es
  kein weiteres $A' \neq A$, sodass auch gilt $R.A' = S.B$. Somit ist die
  Funktion also injektiv und daher ist das Resultat maximal $|S|$. Wenn wir
  jetzt dieses $S$ in die Formel fuer die Selektivitaet von Joins einsetzen,
  dann folgt: $sel_{RS} = \frac{|R \bowtie S|}{|R \times S|} = \frac{|S|}{|R|
  \cdot |S|} = 1/|R|$.

* Wenn wir nach $R.A$ selektieren und die Attributwerte von $A$ sind
  gleichverteilt auf $i$ "Buckets", dann wird bei der Selektion mit einer
  Konstante genau jedes $i$-te Tupel (also genau ein Bucket) selektiert.

Diese Abschaetzungen sind jedoch klarerweise nicht immer moeglich. Eine bessere
Variante ist es natuerlich, die Mittel der Statistik zu verwenden. Hierbei
interessieren wir uns vorallem um die Verteilung der Attributwerte. Es gibt dann
drei Varianten, diese abzuschaetzen:

1. Parametrisierte Funktionen.
2. Histogramme.
3. Stichproben.

#### Parametrisierte Funktionen

Fuer die Variante der parametrisierten Funktionen versucht man anhand der
Werteverteilung des Attributs die Parameter einer Funktion zu bestimmen, die die
Verteilung der Werte moeglichst genau modelliert. Passend waere hierbei
natuerlich z.B. eine Normalverteilung. Man wuerde also versuchen, die Parameter
$\mu, \sigma$ einer Normalverteilung zu bestimmen. Somit kann man dann in
Zukunft bei der Abschaetzung der Selektivitaeten eben bestimmen, wieviele
Attributwerte ungefaehr in Frage kommen wuerden.

#### Histogramme

Fuer Histogramme unterteilt man den moeglichen Wertebereich einfach in
gleich-breite Abstaende und berechnet, wieviele Werte jeweils in einen solchen
Wertebereich fallen. Hauefig sind dann aber in manchen dieser Abschnitte nur
wenige Werte und in anderen sehr viele. Deswegen gibt es neben dieser Variante,
die man auch *equi-width* Histogramme nennt, noch weitere, sogenannte
*equi-depth* oder *equi-height* Histogramme. Dabei legt man eine feste Anzahl
fest, und bestimmt dann Abschnitte, sodass in jedem Abschnitt diese Anzahl
liegt. Dann kann es sehr lange Abschnitte mit relativ wenigen Werten geben, die
erst zusammen diese Anzahl bilden. Dann gibt es aber auch sehr kurze Abschnitte,
in denen es also dichter mehr Werte gibt.

#### Stichproben

Die letzte Variante zur einfachen Bestimmung von Verteilungen ist es, einfach
eine Stichprobe der Relation zu machen. Bei den obigen zwei Varianten musste man
einmal eine Analyse machen und konnte dann fuer einige Zeit diese
verwenden. Beim Stichprobenverfahren wird jedesmal eine zufaellige Menge an
Tupeln gewaehlt und deren Verteilung representativ fuer die ganze Relation
gesehen.

### Anfordern von Statistiken

Statistiken wie parametrisierte Funktionen oder Histogramme werden nicht
automatisch generiert. Hierfuer muss man dem DBMS explizit die Anweisung geben,
die Daten zu analysieren. In Oracle ginge das z.B. so:

```sql
analyze table <Relation> compute|estimate statistics for table;
```

Hierbei wird mit `compute` eine vollkommene Analyse angefordert, und mit
`estimate` nur eine teilweise.

Auch kann man sich oft vom DBMS den Auswertungsplan (QEP) fuer eine Anfrage
anzeigen lassen. In Oracle geht das, indem man vor der eigentlichen Anfrage noch
`explain plan for` schreibt. Dann wird beschrieben, welche Operationen der
physischen Algebra angewandt werden.

## Auswertungsplaene

Ein *kostenbasierter* Optimierer, der also Kostenmodelle zur Evaluierung von
Auswertungsplaenen verwendet, wird versuchen, soviele Auswertungsplane wie
moglich zu bestimmen und den besten daraus zu waehlen. Hierbei gibt es allgemein
zwei Klassen von Auswertungsplaenen: *left-deep* und *bushy* Plaene. Die Anzahl
aller moeglichen Auswertungsplaene weachst mit der Anzahl an Relationen
hyperexponentiell, also koennen meist nicht alle moeglichen Plaene untersucht
werden. Manche DBMS beschranken sich daher auf das Untersuchen nur einer der
eben genannte beiden Klassen. Diese seien aber zunaechst naeher beschrieben.

Bei jedem dieser Planarten sind die inneren Knoten inklusive der Wurzel binaere
Operationen, also Joins oder Kreuzprodukte. Die Blaetter sind immer
*Basisrelationen*. Mit einer Basisrelation ist hierbei eine Relation gemeint,
die nicht das Zwischenergebnis einer bisherigen Operation (z.B. einem Join) ist,
sondern eine, die der Benutzer angelegt hat (z.B. *Studenten*).

### Left-Deep

Ein Left-Deep bzw. *links-tiefer* Auswertungsplan hat als rechten Operanden
immer eine Basisrelation. Alle ausser dem ersten Join werden dann als linken
Operanden also eine Zwischenrelation erhalten. Ein links-tiefer Auswertungsplan
hat meist die Form $((... (R_1 \bowtie R_2) \bowtie ...) \bowtie R_n)$. In
Baumdarstellung wuerde sie so aussehen:

```
      ...
       |
       ><
      / \
    ... R_n
    /
   ><
  /  \
R_1   R_2
```

Aus der "inline-Darstellung" kann man erkennen, dass es $n!$ solcher Plaene
geben muss.

### Bushy

Bushy bzw. *buschige* Plaene haben eine Baumstruktur, wie man sie aus der
Informatik kennen mag. Hier koennen nun also immer sowohl der linke als auch der
rechte Operand eine Basisrelation sein. Ein solcher Plan hat die folgende
typische Struktur:

```
       ...
        |
        ><
      /   \
    ...   ...
    /       \
   ><       ><
  /  \     /  \
R_1  R_2 R_3  R_4
```

Die Anzahl an Moeglichkeiten fuer $n$ Relationen einen solchen Bushy-Plan zu
bauen ist wesentlich hoeher als bei den left-deep Varianten. Bei $10$ Relationen
gab es noch $10! = 22026$ moegliche left-deep Plaene. Es gibt aber schon $1,76
\cdot 10^{10}$ moeglche Bushy-Plaene -- eine ueberwaeltigende Anzahl. Genauer
wird die Anzahl an Bushy-Plaenen fuer $n$ Relationen angegeben durch die Formel
$${{2(n-1)} \choose {n-1}}(n-1)! = (2(n-1))!/(n-1)!$$

## Dynamische Programmierung

Die Anzahl an moeglichen Plaenen fuer $n$ Relationen waechst hyperexponentiell
und schon bei $5$ Relationen muessen wir $1680$ Bushy-Plaene betrachten. Das ist
fuer noch groessere Zahlen nicht mehr moeglich. Daher wenden wir die Methode der
dynamischen Programmierung an, um den Suchraum rekursiv zu optimieren.

Eine interessante Eigenschaft ist, dass man, um einen ganzen Auswertungsbaum
(also ein Auswertungsplan in Baumdarstellung) zu optimieren, auch zuerst
Teilbaeume optimieren kann. Wollen wir $n$ Relationen joinen und die optimale
Reihenfolge dafuer berechnen, genuegt es dabei den optimalen linken sowie
rechten Teilbaum des letzten (aeusserten) Joins zu berechnen und dann den Join
dieser Teilbaeume zu optimieren.

Bei jedem Algorithmus basierend auf dynamischer Programmierung benoetigen wir
neben der Bedingung, dass man die Optimierung des ganzen Problems auf die
Vereinigung der optimalen Loesungen von Sub-Problemen reduzieren kann, auch
immer etwas, das wir "raten". Hier wuerden wir also immer "raten", welche
Permutation die Relationen haben und wo wir den Baum teilen.

Klassisch arbeitet die dynamische Programmierung *top-down*, also vom grossen
Problem zu kleineren hin. Fuer den Auswertungsplan wollen wir aber eher
*bottom-up* arbeiten. Das bedeutet, wir optimieren zuerst die kleine Probleme
und beauen so schrittweise die komplete Loesung auf.

Um nun mit DP einen QEP zu berechnen stellen wir eine Tabelle auf, in der ganz
unten das Einlesen der Relationen in den Hauptspeicher berechnet wird und in den
hoeheren Zeilen dann die Baeume schritt fuer schritt aufgezeichnet werden. Die
Knoten dieser Baeume sind dabei immer optimale Ergebnisse niedrigerer
Stufen.

Fuer das Einlesen gibt es allgemein drei Moeglichkeiten:

1. `scan(R)`: Einlesen der Daten ohne weitere Eigenschaften.
2. `iscan(R)`: Wenn schon es schon einen Index auf diese Relation gibt, dann
               lesen wir diesen Index ein.
3. `iseek_p(R)`: Wenn es schon einen Index auf die Relation gibt und wir eine
                 Bedingung $p$ fuer die Tupel haben, dann koennen wir auch
                 gleich nur diese Tupel selektieren.

Diese Plaene nennt man die *Zugriffsplaene*. Danach berechnen wir die optimale
Sequenz von Joins zwischen den Relationen, also die *Join-Plaene*. Hierfuer
haben wir die folgenden Operationen zur Auswahl:

1. `nested-loop-join(R,S)`, oder kurz `nl-join`.
2. `sort-merge-join(R,S)`, oder kurz `merge-join`, wobei aber vorher noch eine
   explizite `sort` Operation gewesen sein muss.
3. `index-join(R,S)`, wenn schon ein Index auf zumindest einer der beiden
   Relationen liegt.
4. `hash-join(R,S)`.

Am Ende muessen wir dann noch die Teilplane vereinen. Die Tabelle hat dann immer
drei Spalten. Ganz links geben wir an, welche Relationen wir in dieser Zeile
betrachten. In der Mitte zeichnen wir den eigentlichen (bisherigen) QEP und ganz
rechts geben wir die Kosten bis hierhin an.
