# Baeume

$\newcommand{\deg}{\mathop{\rm deg}\nolimits}$
$\newcommand{\sgn}{\mathop{\rm sgn}\nolimits}$
$\newcommand{\rot}{\mathop{\rm rot}\nolimits}$

Wir kennen zu diesem Zeitpunkt schon eine tolle Datenstruktur, um Elemente
zu speichern und folglich effizient zu finden: Hashtabelle. Hashtabellen
brauchen linear viel Platz und haben erwartet konstante Suchzeit. Jedoch haben
sie auch einen inhaerenten Nachteil: sie erhalten die Ordnung von Elementen
ueberhaupt nicht. Element werden einfach dort platziert, wo sie hingehashed
werden.

Insbesondere verwehren uns Hashtabellen eine Operation, die man oftmals machen
moechte: Intervallsuchen. Man stelle sich vor, man hat eine Sequenz von Namen
von Menschen, assoziiert mit ihren Altern. Zum Einen moechte man schnell nach
dem Alter eines Menschen suchen. Dafuer wuerde sich also eine Hashtabelle
eignen. Zum Anderen wuerden wir aber oftmals gerne die Alter aller Menschen
finden, dessen Namen mit einem bestimmten Buchstaben beginnt. Bei Hashtabellen
koennte das nur ueber eine lineare Suche durch die ganze Struktur gelingen.

Ueberlegen wir uns zunaechst wieder, wie wir so eine key-value-basierte
assoziative Datenstruktur mit elementaren Datenstrukturen implementieren
koennten und uberlegen uns, wieso diese nicht ausreichen. Grundsaetzlich gibt es
hier immer zwei Varianten:

* Unsortierte Arrays oder Listes geben uns:
  - Konstante Einfuegezeit
  - Lineaer Intervallanfragen
  - Lineare Suchen
* Sortierte Arrays geben uns:
  - Lineaere Einfuegzeit
  - Logarithmische Suchen
  - Logarithmische Intervallanfragen

Das wollen wir natuerlich wieder verbessern. Ueberlegen wir uns aber zunaechst
wieder ein Interface, das unsere Suchstruktur unterstuetzen soll. Da haben wir
zunaechst die elementaren Operationen:

* `insert(key, value)`: Assoziiere den Wert mit dem gegebenen Schluessel.
* `erase(key)`: Loesche den Eintrag, der durch den Schluessel assoziiert ist.
* `get(key)`: Gib den Wert zurueck, der mit dem Schluessel assoziiert ist.
* `contains(key)`: Bestimme, ob es fuer den Schluessel einen Eintrag gibt.
* `size()`: Die Anzahl an Elementen in der Struktur.
* `is_empty()`: Ist die Struktur leer?
* `clear`: Loesche alle Knoten im Baum.

Dann gibt es noch die Operationen, die uns eine Datenstruktur, die Ordnung
erhaelt, liefern sollte:

* `build`: Erzeuge einen Baum gegeben einem sortierten Array von Schluesseln und
           assoziierten Werten.
* `minimum()`: Gib den kleinsten Schluessel zurueck.
* `maximum()`: Gib den groessten Schluessel zurueck.
* `floor(key)`: Gib den groessten Schluessel zurueck, kleiner oder gleich dem
	            gegebenen Schluessel ist.
* `ceiling(key)`: Gib den kleinsten Schluessel zurueck, der groesser oder gleich
                  dem gegebenen Schluesse ist.
* `rank(key)`: Gib die Anzahl an Elementen zurueck, die strikt kleiner als der
               Schluessel sind.
* `keyOfRank(rank)`: Gib einen Schluessel mit dem gegebenen Rang zurueck.
* `deleteMinimum()`: Loesche das kleinste Element.
* `deleteMaximum()`: Loesche das groesste Element.
* `numbeOfKeys(lowerRank, upperRank)`: Gib die Anzahl an Elementen zurueck,
  dessen Rang in diesem Intervall liegt.

Wir merken, dass wir eine wirklich grosse Anzahl an Operationen unterstuetzen
koennen. Das ist ueblicherweise moeglich, weil man nicht fuer alle ine
effiziente Implementierung garantieren kann. In diesem Fall koennen wir aber
wirklich alle effizient implementieren.

## Binary Search Tree

Als erste Datenstruktur, die das obige Interface erfuellen kann, wollen wir
*Binary Search Trees* (*BSTs*) untersuchen. Ein BST ist ein Binaerbaum, der drei
Invarianten erhaelt. Sei hierfuer $\nu$ ein Knoten im Baum und $\kappa: \nu
\rightarrow \text{key}$ die Abbildung von Knoten auf dessen respektive
Schuessel. Sei dann weiter $\nu_l$ definiert als der linke Kinderknoten von
$\nu$ und symmetrisch $\nu_r$ der rechte Kinderknoten von $\nu$. Weiter sei
$\deg(\nu)$ die Funktion, die jeden Knoten auf dessen Outdegree
abbildet. Hierbei meinen wir mit ausgehenden Kanten solche, die einen Knoten mit
dessen Kindern verbindet. Dann muss fuer jeden solchen Knoten $\nu$ zu jedem
Zeitpunkt gelten:

* Die *Suchbaum-Invariante*: Der Schluessel des linken Knoten $\nu_l$ ist nicht
  groesser als der Schluessel von $\nu$. Auch ist der Schluessel des rechten
  Knoten $\nu_l$ nicht kleiner als der Schluessel von $\nu$:
  $\kappa(\nu_l) \leq \kappa_\nu \leq \kappa(\nu_r)$

* Die *Grad-Invariante*: Jeder Knoten $\nu$ hat maximal, aber nicht
  notwendigerweise, zwei Kinder: $\deg(\nu) \leq 2$

* Die *Schluessel-Invariante*: Es gibt keine Duplikate: $\kappa$ ist injektiv.

Betrachten wir nun ledglich die `insert` Operation als Repraesentant der
grundlegenden Idee, wie wir auf einem BST operieren. Sei hierzu ein Baum $B$ mit
obigen Invarianten schon gegeben. Dann gehen wir fuer `insert` wie folgt vor:

1. Akzeptiere den einzufuegenden Eintrag, bestehend aus Schluessel $\sigma$ und
   Wert $\omega$, in einer oeffentlichen Methode. Rufe dann aber eine rekursive
   Methode auf, zu welcher wir noch den Wurzelknoten hinzugeben. Sei dann $\nu$
   der momentane Knoten und initial eben gleich der Wurzel.
   1. Ist $\nu$ `nil`, so erstelle einen neuen Knoten mit dem Eintrag und gib ihn
	  zurueck.
   2. Ist $\sigma$ kleiner als $\kappa_\nu$, so rufe die Routine rekursiv mit
	  dem $\nu_l$ auf. Setze $\nu_l$ auf den Rueckgabewert des Aufrufes und gib
	  $\nu$ zurueck.
   3. Ist stattdessen $\sigma$ groesser als $\kappa_\nu$, rufe die Routine
	  rekursiv mit $\nu_r$ auf. Gib dann zurueck, was auch immer der rekursive
	  Aufruf liefert. Setze $\nu_r$ auf den Rueckgabewert des Aufrufes und gib
	  $\nu$ zurueck.
   4. Ansonsten gilt $\sigma = \kappa_\nu$, so muss $\nu$ schon einen Wert mit
	  $\sigma$ assoziieren. Ueberschreibe jedenfalls den Wert von $\nu$ mit dem
	  neuen Wert $\omega$. Gib dann $\nu$ zurueck.
5. Akzeptiere den Rueckgabeknoten dieser rekursiven Routine als neue Wurzel. Das
   ist dann entweder die alte Wurzel, falls es vorher eine gab, oder sonst der
   erste Knoten im Baum, falls die alte Wurzel vorher `nil` war.

Interessanterweise koennen sich so bei einem BST viele verschiedene
Baumkonstruktionen ergeben. Betrachten wir zwei Extremfaelle fuer Baeume mit
Knoten aus $[7]$. Der erste Fall ist der optimale, wobei der Baum perfekt
balanciert und vollstaendig ist:

```
	 4
   /   \
  2     6
 / \   / \
1   3 5   7
```

In diesem Fall laeuft die `insert` Methode in logarithmischer Zeit, was toll
ist. Wir haetten einen solchen Baum zum Beispiel erzeugen koennen, wenn wir von
der Mitte ausgehend nach dem Binary Search Prinzip Elemente schon ideal
eingefuegt haetten. Was passiert aber, wenn wir Elemente schon in sortierter
Reihenfolge einfuegen? Nun, nach dem obigen Algorithmus wuerde jedes neue
Element immer rechts vom momentanen Maximum eingefuegt, da es groesser als alle
bisherigen Knoten waere. Dieser Baum wuerde dann also so aussehen:

```
1
 \
  2
   \
    3
	 \
	  4
	   \
	    5
		 \
		  6
		   \
		    7
```

Ist das ueberhaupt noch ein gueltiger BST? Pruefen wir die Eigenschaften:

* Suchbaum-Invariante: Gilt $\kappa(\nu_l) \leq \kappa_\nu \leq \kappa(\nu_r)
  \forall \nu$? Nun, wir haben jedenfalls niemals linke Kinder, also gilt sicher
  $\kappa(\nu_l) \leq \kappa_\nu$. Da auch jeder rechte Kinderknoten $\nu_r$
  einen groesseren Schluessel hat als $\nu$, gilt die Invariante.
* Grad-Invariante: Gilt $\deg(\nu) \leq 2 \forall \nu$? Ja, da sogar $\deg(\nu)
  \leq 1$ gilt.
* Schluessel-Invariante: Ist $\kappa$ injektiv? Ja, da es keine duplizierten
  Schluessel gibt.

Er ist also gueltig. Wie schnell waere nun wohl aber eine Suche in einem solchen
Baum. Nun, nach $n$ Einfuegeoperationen waere der groesste Knoten rechts von
jedem anderen Knoten. Folglich waere die Hoechstlaufzeit $O(n)$! Wir haetten
gerade wieder eine verkettete Liste. Wir werden demnaechst Verfahren
kennenlernen, um diesem Problem entgegenzuwirken und den Baum balanciert zu
halten.

### Operationen

Im Weiteren wollen wir die anderen Operationen auch noch untersuchen.

#### `erase`

Beim Loeschen von Elementen verfolgen wir zunaechst den Standardalgorithmus, um
einen Knoten zu finden. Der einzige Unterschied ist, dass wenn wir auf einen
`nil`-Knoten treffen, wir eine Exception schmeissen muessen. Denn das bedeutet,
das das Element, das zu loeschen war, nicht im Baum ist. Interessant wird es
eher, wenn wir den Knoten gefunden haben. Um die Baumstruktur zu erhalten
muessen wir einen Knoten finden, den wir in die Stelle diesen Knotens geben
koennen, den wir loeschen wollen. Hierbei haben wir zwei Moeglichkeiten: Zum
Einen den groessten Knoten, der kleiner ist, also den *Vorgaenger*. Zum anderen
den kleinsten Knoten, der groesser ist, also den *Nachfolger*. Welchen wir
waehlen, ist voellig egal. Man koennte es sogar randomisieren (for the glory of
satan of course). Diese Art des Loeschens nennt man im Uebrigen
*Hibbard-Deletion*.

Nehmen wir an, wir waehlen immer den Nachfolger eines zu loeschenden Knoten
$\nu$. Dann finden wir den, indem wir rechts von $\nu$ starten und solange nach
links gehen, bis der Knoten kein linkes Kind mehr hat. Dann muss dieser Knoten
naemlich der lokal kleinste sein und somit der kleinste, der groesser ist als
$\nu$. Nun hat dieser Nachfolger aber moeglicherweise right baggage. Da es das
Ziel ist, den Nachfolger in die Position von $\nu$ zu geben, muessen wir diese
right baggage an den Vorgaenger von $\nu$ uebergeben. Hierzu ist es sinnvoll,
beim nach links traversieren den Vorgaenger immer mitzuspeichern. Auch gibt es
denn Fall, dass wir von $\nu_r$ aus gar nie weiter links gehen. Das merken wir
dadurch, dass wir den Vorgaenger anfangs auf `nil` setzen und dann pruefen, ob
dieser `nil` geblieben ist. Ist das der Fall, so ist das right Baggage von $\nu$
schon am richtigen Platz und es gilt ledglich, das linke Kind vom Nachfolger auf
$\nu_l$ zu routen. Nachdem wir den Nachfolger vollkommen mit den Kindern von
$\nu$ verbunden haben, duerfen wir $\nu$ vernichten. Das ergibt folgenden
Algorithmus:

1. Sei $\nu$ der momentane Knoten in einer rekursiven Methode und $\sigma$ der
Schluessel des zu loeschenden Elements. Dann:
2. Ist $\nu$ `\nil`, so gib eine Fehlermeldung aus.
3. Ist $\sigma$ kleiner als $\kappa_\nu$, so gehe nach links. Erwarte auf
$\nu_l$ dann den Rueckgabewert des rekursiven Aufrufs.
4. Ist $\sigma$ groesser als $\kappa_\nu$, so gehe nach rechts. Erwarten dann
auf $\nu_r$ den Rueckgabewert des rekursiven Aufrufs.
5. Ansonsten muss $\sigma = \kappa_\nu$ gelten und wir muessen $\nu$
loeschen. Hierzu:
	1. Ist $\nu_l$ `nil`, so loesche $\nu$ und gib einfach $\nu_r$ zurueck.
	2. Ist $\nu_r$ `nil`, so loesche $\nu$ und gib einfach $\nu_l$ zurueck.
	3. Ansonsten setze einen Vorgaenger $\rho$ zu `nil` und einen Iterator fuer
       den Nachfolger $\mu$ auf das rechte Kind $\nu_r$ von $\nu$.
	4. Solange, wie $\mu$ ein linkes Kind hat:
	   1.  Setze $\rho := \mu$
	   2. Setze $\mu := \mu_l$
	5. Ist Nun der Vorgaenger $\rho$ nicht `nil`, so setze $\rho_l$ auf $\mu_r$
       um das right Baggage vom Nachfolger loszuwerden. Setze auch nur in diesem
       Fall $\mu_r$ auf $\nu_r$.
    6. Setze (jedenfalls) $\mu_l$ auf $\nu_l$.
	7. Vernichte $\nu$ und streue Salz darueber.
    8. Gib $\mu$ als Nachfolger zurueck.

Alternativ kann man (5) auch rekursiv implementieren. Das sieht dann so aus:

1. Ist $\nu_l$ `nil`, so loesche $\nu$ und gib einfach $\nu_r$ zurueck.
2. Ist $\nu_r$ `nil`, so loesche $\nu$ und gib einfach $\nu_l$ zurueck.
3. Deklariere $\mu$ als Nachfolger.
4. Setze $\mu_r$ auf den Rueckgabewert einer rekursiven Funktion, die eine
   Referenz auf $\mu$ sowie einen Rekursionknoten $\nu'$ nimmt. Gebe hierzu
   $\nu' := \mu_r$. Die Idee ist es, so lange nach links zu rekursieren, bis wir
   den Vorgaenger gefunden haben. Also:
	   1. Hat $\nu'$ kein linkes Kind mehr, so setze $\mu := \nu'$ und gib
          $\mu_r$ zurueck.
	   2. Ansonsten koennen wir nach links rekursieren. Setze hierfuer $\nu'_l$
          auf den Rueckgabewert des rekursiven Aufrufs, zu welchem man $\nu'_l$
          als naechsten Rekursionsknoten gebe.
	   3. Berechne Hoehendifferenz und Groesse des Baumes als Backtracking neu.
	   4. Gib $\nu'$ zurueck.
7. Vernichte $\nu$ und streue Salz darueber.
8. Gib $\mu$ als Nachfolger zurueck.

#### `get` und `contains`

Das Pruefen auf Zugehoerigkeit zum Baum sowie das Finden eines Wertes
funktioniert wie `insert`, nur ohne dem `insert`. Insbesondere koennen wir diese
beiden Routinen jedoch durch eine einzige private Funktion ersetzen und auf
dessen Rueckgabewert unterschiedlich reagieren. Hierzu definieren wir eine
Methode `find`, die einen Knoten anhand seines Schluessels findet. Wir
rekursieren dabei wie immer mit Fallunterscheidung bezueglich der Ordnung des
Knotenschluessels und des gesuchten Schluessels. Finden wir den gesuchten
Knoten, so geben wir diesen einfach zurueck. Ist der momentane Knoten am Anfang
einer Rekursion `nil`, so geben wir auch `nil` zurueck.

Ausserhalb der Rekursion koennen wir dan pruefen, ob der zurueckgegebene Knoten
`null` ist, oder nicht. Bei `contains` genuegt das schon, um die Antwort zu
finden. Bei `get` koennen wir dann entweder eine Fehlermeldung ausgeben, oder
den Wert, der im Knoten gespeichert ist, zurueckgeben.

#### `clear`

Das Loeschen des ganzen Baumes geht mit einer schoenen, kurzen post-order
Rekursion:

1. Ist der momentane Knoten $\nu$ gerade `nil`, so bottom-out (kein
Rueckgabewert). 2. Rekursiere nach links. 3. Rekursiere nach rechts. 4. Loesche
$\nu$.

Somit wird in quasi-post-order oder auch "topologischer Ordnung" die
Baumstruktur Teilbaum fuer Teilbaum eliminert.

#### `build`

Es wie bei Heaps auch bei BSTs moeglich, einen Baum gegeben einer Sequenz zu
erstellen. Das geht dabei in linearer Zeit, wie beim Heap. Hierfuer muessen wir
lediglich fordern, dass die Sequenz sortiert ist. Dann rekursieren wir
"pre-order" durch die Sequenz:

1. Seien $i,j$ zwei Indizes in die Sequenz und initial $i := 0$ und $j := n$ wo
$n$ die Laenge der Sequenz ist. Die momentan betrachtete Teilsequenz sei dann
jeweils in $[i, j)$. Dann rekursiv: 2. Gilt $i == j$, so gib `nil` zurueck. 3.
Finde den Mittelpunkt $m := (i + j)/2$. 4. Erzeuge einen neuen Knoten $\nu$ fuer
den Eintrag an Index $m$. 5. Rekursiere nach links mit $j := m$ und setze den
$\nu_l$ auf den Rueckgabewert. 6. Rekursiere nach rechts mit $i := m + 1$ und
setze den $\nu_r$ auf den Rueckgabewert. 7. Gib $\nu$ zurueck.

#### `minimum` und `maximum`

Minimum und Maximum finden wir in einem Binaerbaum wegen seiner grundlegenden
Struktur ganz leicht. Fuer das Minimum iterieren wir von der Wurzel aus einfach
solange nach links, bis der Knoten keinen linken Knoten mehr hat. Dann muss das
der Knoten mit dem kleinsten Schluessel sein. Fuer den groessten Schluessel
gehen wir auf selbe Weise nach rechts.

#### `deleteMinimum()` und `deleteMaximum()`

Um Minimum und Maximum zu loeschen gehen wir aehnlich vor wie in `minimum` bzw.
`maximum` und auch aehnlich dazu, wie wir Hibbard-Deletion in `erase` gemacht
haben. Wir ziehen beim traversieren des Baumes nach links oder rechts einen
Vorgaengerzeiger mit, der initial `nil` ist. Haben wir das Minimum oder Maximum
gefunden, pruefen wir, ob der Vorgaenger noch `nil` ist. Wenn ja, war gerade die
Wurzel Minimum oder Maximum (wenn der BST gerade eine verkettete Liste ist).
Dann setzen wir den Wurzelzeiger im Baum einfach auf den naechsten Knoten in der
"Liste" und loeschen die Wurzel. Ansonsten haben wir also einen Vorgaenger. Wie
in `erase` ersetzen wir dann den zu loeschenden Knoten durch das entsprechende
Baggage links oder rechts von diesem Knoten.

#### `floor` und `ceiling`

Die `floor` Funktion findet das groesste Element, das kleiner oder gleich einem
bestimmten Schluessel $sigma$ ist. Die Idee hierbei ist, dass wenn wir wieder
rekursiv einen Knoten $\nu$ betrachten. Da wir einen Knoten suchen, dessen
Schluessel nicht groesser ist als $\sigma$, gehen wir sofort nach links, falls
$\kappa_\nu$ groesser als $\sigma$ ist. Ansonsten muss $\kappa_\nu$ also wie
gewollt kleiner oder gleich $\sigma$ sein. Dann gilt es ledglich, den greossten
solchen Schluessel zu finden. Sei nun $\nu$ also ein Knoten, sodass $\kappa_\nu
\leq \sigma$. Die weitere Idee ist es nun, einfach blind von $\nu$ weiter nach
rechts zu finden. Finden wir dan einen Knoten, dessen Schluessel noch groesser
als der von $\nu$ ist aber noch immmer $\leq \sigma$ ist, so ist das gut und wir
machen weiter. Kommen wir aber an einem `nil`-Knoten an, so geben wir diesen
zurueck. Innerhalb von `floor` pruefen wir dann beim Backtracken vom rekursiven
`floor` Aufruf (nach rechts), ob ein `nil` Knoten zurueckgekommen ist, oder
nicht. Falls ja, so muss $\nu$ schon der maximale Vorgaenger sein. Falls nicht,
so haben wir also einen Knoten gefunden, dessen Schluessel groesser ist als der
von $\nu$, der aber noch immer kleiner oder gleich $\nu$ ist. Also:

1. Sei $\nu$ der momentane Knoten der Rekursion und $\sigma$ der relevante
Suchschluessel. Dann: 2. Ist $\nu$ `nil`, so gib `nil` zurueck. 3. Ist
$\kappa_\nu$ groesser als $\sigma$, so gehe nach links. 4. Sonst gilt also
$\kappa_\nu \leq \sigma$. Dann: 1. Rufe die Routine rekursiv mit $\nu_r$ auf um
moeglichwerweise noch groessere Knoten zu finden, die die Eigenschaft erfuellen.
2. Pruefe, ob der Rueckgabewert von (1) nun `nil` war. Wenn ja, so gib $\nu$ als
Vorgaenger zurueck. Ansonsten gib den Rueckgabewert zurueck.

Fuer `ceiling` gehen wir symmetrisch vor. Wir suchen hierbei also den
Nachfolger, sprich den Knoten mit dem kleinsten Schluessel, der aber noch immer
groesser oder gleich $\sigma$ ist. Dann gehen wir hierbei immer sofort nach
rechts, falls $\kappa_\nu$ kleiner als $\sigma$ ist. Denn kleiner wollen wir ja
sicher nichts. Analog zu vorhin riskieren wir dann weitere Schritte nach links,
falls $\kappa_\nu \geq sigma$ gilt.

#### `rank`

Die `rank` Operation bestimmt die Anzahl an Elementen, die strikt kleiner
(alternativ: kleiner gleich) als ein Schluessel $\sigma$ ist. Hierzu befolgen
wir wieder ein rekursives Schema. Die grundlegende Idee ist, dass wenn wir bei
einem Knoten $\nu$ sind, dessen Schluessel kleiner ist als $\sigma$, dann sind
folglich auch alle Schluessel im linken Teilbaum von $\nu$ kleiner als $\sigma$.
Wir zaehlen also $\nu$ sowie die Groesse des linken Teilbaumes von $\nu$. Dann
gehen wir nach rechts und pruefen, ob wir dort noch weitere Elemente finden, die
strikt kleiner sind als $\sigma$. Ist aber dann irgendwann $\nu$ nicht kleiner,
sondern groesser klein $\sigma$, so gehen wir nach links. Kommen wir auf einen
`nil`-Knoten, so ist das der Base-Case und wir geben einen Rank von Null
zurueck.

1. Sei $\sigma$ der Schluessel, dessen Rang wir bestimmen wollen und $\nu$ der
Rekursionsknoten. 2. Ist $\nu$ `nil`, so gib Null zurueck. 3. Gilt $\kappa_\nu
\geq \sigma$, so gehe direkt nach links. 4. Sonst: 1. Setze $r := 1$, um
zumindest schon $\nu$ selbst als Knoten zu zaehlen, dessen Schluessel also nicht
groesser oder gleich (3), sondern strikt kleiner ist als $\sigma$. 2. Hat $\nu$
dann einen linken Teilbaum $\nu_l$, so addiere die volle Groesse dieses
Teilbaumes auf $r$. 3. Rekursiere nach rechts und addiere, was auch immer
zurueckkommt, auf $r$. 4. Gib $r$ zurueck.

#### `keyOfRank`

Die `keyOfRank` Operation findet einen Knoten $\nu$ bzw. dessen Schluessel
$\kappa_\nu$, sodass genau $r$ Elemente strikt kleiner sind als dieser
Schluessel. Wir koennen dabei wiederum toll Rekursion nutzen:

1. Sei $r$ der Rang, fuer welchen wir einen Knoten bestimemn wollen und desssen
Schluessel zurueckzugeben ist. Sei auch $\nu$ wieder der Rekursionsknoten. 2.
Gilt $\nu$ `nil`, gib `nil` zurueck. 3. Sei $\eta_l$ die Groesse des linken
Teilbaumes von $\nu$, falls $\nu$ einen linken Teilbaum hat, und sonst Null. 4.
Gilt $\eta_l > r$, so gibt es fuer $\nu$ also mehr als $r$ Schluessel, die
strikt kleiner sind. Rekursiere also weiter nach links. 5. Gilt $\eta_l < r$, so
suchen wir also nach einem Knoten, mit mehr Elementen, die strikt kleiner sind.
Wir muessen also rechts von $\nu$ gehen. Da $\nu_r$ aber ein unabhaengiger
Teilbaum ist, weiss dieser nichts von $\nu_l$. Daher muessen wir beim
rekursieren den Rang um genau $\eta_l + 1$ dekrementieren, da wir den linken
Teilbaum sowie auch $\nu$ schon mitgezaehlt haetten. 6. Gilt $\eta_l = r$, so
ist $\nu$ der gesuchte Knoten. Gib ihn also zurueck.

### Traversierungen

Wir wollen noch untersuchen, auf welche Weisen wir ueber die Knoten eines Baumes
iterieren koennen. Konkret gibt es hier vier Moeglichkeiten: pre-order,
in-order, post-order und level-order. Diese seien nun naeher erlaeutert.

#### In-Order

Wir erlauetern In-Order zuerst, weil sie die natuerlichste Ordnung ist. Sie gibt
die Schluessel naemlich sortiert zurueck. Die Idee hierbei ist es, rekursiv
durch den Baum zu steigen. Bei jedem Knoten $\nu$ gehen wir dann immer zuerst
nach links, da wir in dieser Sortierung ja kleinere Elemente zuerst ausgeben
wollen. Kommen wir aus der Rekursion wieder zurueck, so fuegen wir den
Schluessel $\kappa_\nu$ von $\nu$ ein, da dieser nach allen kleineren Schlueseln
nun an der Reihe ist. Danach kommen natuerlich die Schluessel, die groesser
sind, als der von $\nu$. Daher rekursieren wir danach einfach nach rechts. Ist
$\nu$ am Anfang einer Rekursion dann `nil`, so bottomen wir einfach out.

1. Sei $\nu$ der momentane Knoten in der Rekursion und $\mathbb{K}$ die Menge
der Schluessel, die wir sammeln. Dann rekursiv: 2. Ist $\nu$ `nil`, so hoere auf
zu rekursieren (`return`). 3. Rekursiere fuer $\nu_l$. 4. Fuege $\kappa_\nu$ in
$\mathbb{K}$ ein. Setze also $\mathbb{K} := \mathbb{K} \cup \{\kappa_\nu\}$ 5.
Rekursiere fuer $\nu_r$.

#### Pre-Order und Post-Order

Pre-Order und Post-Order schliessen jetzt ganz stark an In-Order Traversierung
an. Konket ist der einzige Unterschied, *wann* bzw. *wo in der Funktion*, wir
$\kappa_\nu$ in die Schluesselmenge $\mathbb{K}$ einfuegen. Bei In-Order haben
wir das zwischen Rekursion nach links und rechts getan. Bei Pre-Order tun wir
das nun vor beiden Rekursionen. Bei Post-Order nach beiden Rekursionen. Das gibt
folgenden generischen Algoritmus:

1. Sei $\nu$ der momentane Knoten, $\mathbb{K}$ die momentane Schluesselsammlung
und $o \in \{\text{pre-order}, \text{in-order}, \text{post-order}\}$. Dann
rekursiere: 2. Ist $\nu$ `nil`, so hoere auf zu rekursieren. 3. Gilt $o =
\text{pre-order}$, so fuege jetzt $\kappa$ in $\mathbb{K}$ ein. 4. Rekursiere
fuer $\nu_l$. 5. Gilt $o = \text{in-order}$, so fuege jetzt $\kappa$ in
$\mathbb{K}$ ein. 6. Rekursiere fuer $\nu_r$. 7. Gilt $o = \text{post-order}$,
so fuege jetzt $\kappa$ in $\mathbb{K}$ ein.

Diese Einfuegeoperationen sind natuerlich exklusive, also es passiert immer nur
eine. Noch etwas Intuition, was diese Ordnungen bedeuten:

* *In-Order*, wie oben erlauetert, gibt die natuerliche Ordnung der Elemente
* wieder. *Pre-Order* gibt die Reihenfolge der Elemente da, wie sie bei der
* Traversierung durch den Baum vorkommen. *Post-Order* gibt quasi eine
* topologische Sortierung der Elemente wieder. Wir fuegen den Schluessel erst
* ein, wenn alle seine Dependencies abgearbeitet wurden. Das macht
* beispielsweise fuer Abstrakte Syntaxbaeume Sinn.

## AVL Baeume

Oben haben wir beobachtet, dass Vanilla-BSTs das enorm grosse Problem haben,
unbalanciert zu sein und dadurch sogar zu verketteten Listen ausarten koennen.
Es gibt nun eine Familie von Datenstrukturen, die BSTs erweitern um Balancierung
zu garantieren. Die gaengigsten Varianten hierbei sind *$(a,b)$-Baeume*,
*Red-Black-Trees* und *AVL-Baeume* die bekanntesten. Letztere wollen wir nun
naeher untersuchen.

Die grundlegende Innovation bei AVL-Baeumen ist jene, dass wir fuer jeden Knoten
im Baum die Hoehendifferenz der Kinder des Knoten speichern. Sei hierfuer $\nu$
ein beliebiger Knoten im Baum und $\sigma_\nu$ die Groesse des Baumes mit Wurzel
$\nu$. Dann geben wir mit $\eta_\nu$ die Differenz $\sigma_{\nu_r} -
\sigma_{\nu_l}$ an, also die Differenz in der Hoehe des linken und rechten
Teilbaumes. Unser Ziel bei AVL-Baeumen ist es nun, dass $\eta$ stets $\leq 1$
bleibt. Gilt diese Invariante erstmal, so erreichen wir dadurch, dass ein
Teilbaum hoechstens doppelt so gross werden kann, wie der jeweils andere Baum.
Dadurch bleiben insbesondere alle Operationen logarithmisch, die bei
Vanilla-BSTs linear werden konnten.

Wie erreichen wir nun aber die Balancierung des Baumes anhand der Hoehendifferenz $\eta_\nu$? Zunaechst definieren wir: ein Baum ist *balanciert*, wenn jeder Pfad von der Wurzel zu einem Blatt gleich lang und somit gleich der Hoehe des ganzen Baumes ist. Nun, betrachten wir diesen balancierten Baum:

```
	 4
   /   \
  2     6
 / \   / \
1   3 5   7
```

Da dieser Baum perfekt balanciert ist, ist die Hoehendifferenz fuer jeden Knoten
$\nu$ immer Null. Wenn wir nun aber einen neuen Schluessel einfuegen, so kann
das nicht so bleiben hawidere. Fugen wir zum Zwecke der Demonstration also den
Schluessel $19$ in den Baum ein:

```
	 4
   /   \
  2     6
 / \   / \
1   3 5   7
           \
		    19
```

War $\eta_7$ vorher noch Null, so ist es jetzt $+1$, da der rechte Teilbaum vom
Knoten mit Schluessel $7$ nun ein Uebergewicht von einem Knoten hat. Oben haben
wir aber die Invariante so festgelet, dass ein Unterschied von $\pm 1$ noch kein
Problem war. Fuegen wir nun aber noch $69$ ein:

```
	 4
   /   \
  2     6
 / \   / \
1   3 5   7
           \
		    19
			 \
			  69
```

Nun gilt ploetzlich $\eta_7 = +2$. Nach Voraussetzung ist das eine Verletzung
unserer Invariante. Wie stellen wir die Balance des Baumes nun wieder her?
Mittels *Rotationen*. In diesem Fall wuerden wir Knoten $7$ nach *links*
rotieren, was bedeutet, dass $8$ in seine Position kommt und $7$ das linke Kind
von $8$ wird:

```
	 4
   /   \
  2     6
 / \   / \
1   3 5   19
         / \
		7   69
```

Nun gilt zwar $\eta_6 = +1$, aber das ist in Ordnung. Betrachten wir noch einen
weiteren lustigen Fall, wenn wir nun $13$ einfuegen:

```
	 4
   /   \
  2     6 (+2)
 / \   / \
1   3 5   19 (-1)
         / \
		7   69
		 \
		  13
```

Wie in der Graphik angemerkt haben wir nun die interessante Situation, dass
$\eta_6 = +1$ aber $\eta_19 = -1$. Insbesondere gilt also gerade $\sgn(\eta_6)
\neq \sgn(\eta_7)$. In diesem Fall muessen wir *doppelt* rotieren. Zuerst
muessen wir das Signum der beiden Baeume gleichstellen. Hierzu rotieren wir
zunaechst Knoten $7$ nach links:

```
	 4
   /   \
  2     6 (+2)
 / \   / \
1   3 5   7 (+1)
           \
           19
	      /  \
	     13   69
```

Dann koennen wir jetzt Knoten $6$ nach links rotieren:

```
	 4
   /   \
  2     7
 / \   / \
1   3 6   19
     /   /  \
	5	13   69
```

Und die Rotation ist perfekt! Wir wollen diese beiden Rotationsoperationen nun
formalisieren.

### Einzelrotationen

Einzelrotationen sind immer dann moeglich bzw. notwendig, wenn fuer einen Knoten
$|\eta_\nu| > 1$ aber. Da wir die Balance erhalten, gilt in diesem Fall aber
auch sicher, dass $|\eta_\nu| = 2$. In diesem Fall genuegt also, lediglich
Knoten $\nu$ in die Richtung *entgegen dem Uebergewicht* zu rotieren. Hat Knoten
$\nu$ also ein Uebergewicht von $\eta_\nu = +2$, so muessen wir nach links
rotieren, weil der rechte Teilbaum von $\nu$ ein Uebergewicht haben muss. Gilt
$\eta_\nu = -2$, so rotieren wir nach rechts, weil hier der linke Teilbaum von
$\nu$ das Uebergewicht hat. Allgemein koennten wir also eine Funktion $d$
definieren, die eine Hoehendifferenz $\eta_\nu$ auf eine Richtung $\in
\{\text{links, rechts}\}$ abbildet:

$$d: \{-2, +2\} \rightarrow \{\text{links, rechts}\}: -2 \mapsto \text{rechts},
+2 \mapsto \text{links}$$

Betrachten wir nun noch den allgemeinsten aller Konfigurationen fuer eine
Einzelrotation nach links:

```
  A (+2)
 / \
B   D (+1)
   / \
  C   E
     / \
	F   G
```

Hier hat $A$ also ein Ubergewicht von $+2$ nach rechts, sodass wir $A$ sicher
rotieren muessen. Da das Signum des Ubergewichts von $D$ gleich dem von $A$ ist,
genuegt eine einzelne Rotation. Auch merken wir hier aber, dass die Knoten alle
noch weiters Baggage haben. Wir muessen also nach folgendem Algorithmus
fortfahren:

1. Wir wollen $A$ nach links rotieren.
2. Verbinde $A_r$ mit $D_l$.

```
  A
 /|
B | D
  |/ \
  C   E
     / \
	F   G
```

3. Verbinde $D_l$ mit $A$.

```
  A
 /|\
B | D
  |  \
  C   E
     / \
	F   G
```

4. Schiebe $D$ nach oben (nur visuell)

```
    _D_
   /   \
  A     E
 / \   / \
B   C F   G
```

Was fuer eine tolle Balancierung!

Wir koennen diese Rotationsoperation noch vollkommen fuer sowohl Links- als auch
Rechtsrotation verallgemeinern:

1. Sei:
   * $\nu$ der Knoten mit Hoehendifferenz $|\eta_\nu| = 2$, den wir rotieren wollen.
   * $d$ die oben definierte Abbildung von $\eta_\nu$ auf die Rotationsrichtung.
   * $\bar{d}$ der Invertierungsoperator fuer $d$, mit $\overline{\text{links}} = \text{rechts}$ und umgekehrt.
   * $\star$ ein Operator, der fuer einen Knoten den Zeiger auf diesen Knoten signifiziert. $\nu_l^\star$ wuerde also den Zeiger auf das linke Kind von $\nu$ meinen, nicht den Kinderknoten selbst.
   * $\mu := \nu_{\bar{d}}$ das Kind von $\nu$ entgegen der Rotationsrichtung, das also in die Position von $\nu$ rutschen wird.
2. Setze $\nu_{\bar{d}}^\star$ zu $\mu_d$ (oben: $A_r \rightarrow D_l$)
3. Setze $\mu_d^\star$ zu $\nu$ (oben: $D_l \rightarrow A$)
4. Gib $\mu$ zurueck.

### Doppelrotationen

Dopelrotationen sind immer dann notwendig, wenn wir eine *Zick-Zack* Situation
haben, wo das Signum $\sgn(\eta_\nu)$ der Hoehendifferenz $\eta_\nu$ eines
Knoten $\nu$ nicht mit dem Signum des uebergewichtigen Kinderknoten
uebereinstimmt. In diesem Fall muessen wir einach zunaechst das uebergewichtige
Kind so rotieren, das es im Signum der Hoehendifferenz mit $\nu$ uebereinstimmt.
Dadurch reduzieren wir es naemlich wieder exakt auf den Fall von vorhin, wo nur
mehr eine Einzelrotation notwendig war. Betrachten wir hier wieder das Problem
anhand einer einzigen Richtung:

```
 (-2)  A
      / \
(+1) D   B
    / \
   C   E
      / \
	 F   G
```

Hier hat $A$ also ein Uebergewicht von $-2$ mit negativem Signum, aber das
uebergewichtige Kind $D$ ein Uebergewicht von $+1$ mit positivem Signum. Es gilt
nun also einfach, diesen Fall auf den vorherigen zu reduzieren, sodass die
Signums uebereinstimmen. Dann bleibt naemlich nur mehr, eine Einzelrotation zu
machen. Wir rotieren also das fette Kind entgegen seinem Uebergewicht nach
links:

```
 (-2)  A
      / \
(-1) E   B
    / \
   D   G
  / \
 C	 F
```

Nun haben $A$ und $E$ zwar noch immer beide Uebergewicht, aber mit selbem
Signum. Wir rotieren also einfach $A$ mit einer Einzelrotation entgegen seinem
Uebergewicht nach rechts:

```
       _E_
      /   \
     D     A
    / \   / \
   C   F G   B
```

Nach obiger Notation koennen wir das nun auch wieder formalisieren und
generalisieren:

1. Sei:
   * $\nu$ der Knoten mit Hoehendifferenz $|\eta_\nu| = 2$, den wir rotieren wollen.
   * $d$ die oben definierte Abbildung von $\eta_\nu$ auf die Rotationsrichtung.
   * $\bar{d}$ der Invertierungsoperator fuer $d$, mit $\overline{\text{links}} = \text{rechts}$ und umgekehrt.
   * $\star$ ein Operator, der fuer einen Knoten den Zeiger auf diesen Knoten signifiziert. $\nu_l^\star$ wuerde also den Zeiger auf das linke Kind von $\nu$ meinen, nicht den Kinderknoten selbst.
   * $\mu := \nu_{\bar{d}}$ das Kind von $\nu$ entgegen der Rotationsrichtung, das also in die Position von $\nu$ rutschen wird.
   * $\rot(\nu, d)$ die Rotationsroutine fuer Einzelrotationen, die den Knoten $\nu$ in die Richtung $d \in \{\text{links, rechts}\}$ rotiert.
2. Setze $\mu := \rot(\mu, \bar{d})$.
3. Gib $\rot(\nu, d)$ zurueck.

Wir sehen also, dass sich Doppelrotationen vollkommen durch Einzelrotationen
implementieren lassen.

### Implementierung

Das wunderschoene an AVL-Baeumen (und auch RB-Baeumen) ist, dass sie quasi
*Extensions* fuer einen regulaeren BST sind. Man muss BST-Code also nie
veraendern, sondern nur etwas hinzufuegen, um die AVL-Eigenschaften zu
aktivieren. Konkret muss man folgende Schritte vornehmen, um einen BST in einen
AVL-Baum umzuwandeln:

1. Fuege Methoden oder Attribute hinzu, um die Hoehendifferenz jedes Knoten zu speichern und zu verwalten.
2. Definere eine Funktion, die man nach jeder Aenderung am BST aufrufen kann, um die Balance zu erhalten.

Die Funktion aus (2) wuerde dabei konkret wie folgt aussehen:

```
def ensure_balance(node):
	if node.height_delta < 2:
		return

	# Else we need to rotate
	direction = get_direction(node.height_delta)
	child = get_child(node, direction.opposite)

	# Reduce to single rotation case
	if sign(node.height_delta) != sign(child.height_delta):
		child = rotate(child, direction.opposite)
		set_child(node, direction.opposite, child)

   return rotate(node, direction)
```

### Listenstruktur

Am Anfang hatten wir die Beobachtung gemacht, dass Intervallanfragen mit
Hashtabellen nicht gehen und wollten eine Datenstruktur finden, die dies
ermoeglicht. BSTs und AVL-Baeume sind solche Datenstrukturen, da sie die Ordnung
von Elementen erhalten und gleichzeitg schnellen Zugriff auf die Schluessel
bieten. Dennoch koennte man denken, dass eine Iteration durch einen Baum noch
nicht vollstaendig effizient ist. Man bedenke, dass wenn man einen Baum
vollstaendig in-order traversieren will, man oftmals viele Kanten entlang laufen
muss, nur um zum naechsten Knoten in der Ordnung zu kommen. Das kann teuer sein.
Eine Idee ist es daher oftmals, eine verkettete Liste unten an den Baum zu
kleben, und den Baum selbst dann nur als Suchstruktur auf diese Liste zu
benutzen. Die assoziierten Werte selbst waeren nur in der Liste gespeichert. Die
inneren Knoten im Baum wuerden die Suche nach einem Element dann quasi nur durch
die Struktur in Richtung des Eintrags in der Liste leiten. Insbesondere koennen
wir dann aber naemlich innerhalb der verketteten Liste viel effizienter und
schneller zu naechsten Elementen in der Ordnung kommen. Somit werden
Intervallanfragen nochmals effizienter.

## $(a,b)$-Baeume

Eine weitere Datenstruktur, die Balancierung von BSTs garantiert, sind $(a,b)$-Baeume. Die Idee eines $(a,b)$-Baumes ist es nun, dass ein Knoten nicht mehr nur einen Schluessel, sondern *viele* haben kann. Genauer definieren wir zwei Parameter $a,b$, sodass *der Ausgangsgrad* bzw. die Anzahl an Kindern $\deg(\nu)$ jedes Knoten $\nu$ im Baum mindestens $a$ und maximal $b$ gross ist. Beispielsweise haette jeder Knoten in einem $(2,3)$-Baum mindestens zwei Kinder und maximal drei. D.h. jene Knoten, die zwei Kinder haetten waeren wie BST-Knoten, wohingegen jene Knoten mit drei Kindern zwei Schluessel pro Knoten speichern wuerden. Eine Besonderheit von $(a,b)$-Baeumen ist es dann, dass sie *vollstaendig und balanciert* sind.

Wie bei BSTs und AVL-Baeumen koennen wir nun wieder drei Invarianten definieren.

* *Form-Invariante*: Jeder Pfad von der Wurzel zu einem Blatt ist gleich lang und somit gleich der Tiefe des Baumes. Ein $(a,b)$-Baum ist also stets *balanciert*.

* *Grad-Invariante*: jeder Knoten im Baum, *ausser* der Wurzel, hat mindestens $a$ und maximal $b$ Kinder: $a \leq \deg(\nu) \leq b \forall \nu$.

* *Schluessel-Invariante*: die Schluessel liegen sortiert in jedem Knoten vor, sodass also $i < j \Rightarrow \kappa_i < \kappa_j$. Der Verweis links von $\kappa_i$ zeigt dann also auf die Wurzel eines Teilbaumes, der nur Schluessel kleiner $\kappa_i$ enthaelt. Der Teilbaum, auf dessen Wurzel der Verweis rechts von $\kappa_j$ zeigt, enthaelt hingegen nur Schluessel, die groesser als $\kappa_j$ und transitiv auch $\kappa_i$ sind. *Zwischen* $\kappa_i$ und $\kappa_j$ liegen dann Schluessel, die groesser als $\kappa_i$ und kleiner als $\kappa_j$ sind. Wir muessen uns nicht um gleiche Schluessel kuemmern, da Duplikate in einem $(a,b)$-Baum nicht erlaubt sind.

Wir muessen bei der Grad-Invariante eine Aussnahme fuer die Wurzel machen, da sonst schon ein leerer Baum die Invariante invalidieren wuerde. Auch muessen wir festlegen, dass $b \leq 2a - 1$ ist, da sonst bestimmte Operationen nicht moeglich sind.

Wie oben beschrieben ist es oft moeglich, eine verkettete Liste unter eine Suchstruktur zu geben. Wir wollen diesen Ansatz bei diesem Baum auch waehlen. Im Bereich von Datenbanken nennt man einen $(a,b)$-Baum ohne Liste in den Blaettern uebrigens auch $B$-Baum und mit Liste dann $B^+$-Baum.

### Beispiel

Betrachten wir zunaechst ein Beispiel eines $(a,b)$-Baumes, indem wir $a$ auf $3$ festlegen. Nach Voraussetzung muss $b$ dann mindestens $2a - 1$ sein. Wir setzen $b$ also auf $2 \cdot 3 - 1 = 5$. Das bedeutet nun also, dass jeder Knoten ausser der Wurzel:

* *Mindestens* $a = 3$ Kinder und somit $a - 1 = 2$ Schluessel haben muss.
* *Maximal* $b = 5$ Kinder und somit $b - 1 = 4$ Schluessel haben muss.

Ein Knoten wird dann so strukturiert bzw. implementiert, dass jeder der $a - 1$ Knoten einen Verweis nach links hat. Meist haben wir dann noch einen Sentinel-Knoten ganz rechts, der den linken, letzten Verweis behaelt. Die Schluessel selbst liegen dann also sortiert in jedem Knoten vor. Somit kann man fuer eine Suche in $O(\log_2 k)$ Zeit einen Schluessel in einem Knoten ueber Binary Search finden. Ein gueltiger $(3,5)$-Baum waere somit:

```
         ________[10]_________
	    /                     \
  [2 | 3 | 5]       [12 | 27 | 30 | 33]
  /   /   / \        /   /    /    /  \
[2 | 3 | 5 | 10]->[12 | 27 | 30 | 33 | x]
```

Wir koennen hier folgende Eigenschaften beobachten:

* Die Wurzel hat Grad zwei, was also unter $3$ liegt und daher eigentlich die
  Grad-Invariante invalidieren wuerde. Da es sich aber um die Wurzel handelt,
  machen wir eine Ausnahme.
* Das linke Kind der Wurzel hat Ausgangsgrad vier, was in Ordnung ist, da $3
  \leq 4 \leq 5$.
* Das rechte Kind der Wurzel hat Ausgangsgrad fuenf, was auch noch in Ordnung
  ist.
* Unten haben wir die verkettete Liste, die dieselben Schluessel enthaelt, wie
  der Baum. Hierbei ist es nochmals wichtig sich zu erinnern, dass bei einer
  solchen Listenstruktur der Baum selbst nur die Suche zur Liste hinleitet aber
  die Werte der Schluessel selbst nicht speichert.
* Am Ende der Liste haben wir ein Sentinel-Element.
* Die inneren Knoten verweisen fuer ihre eigenen Schluessel immer nach links.

Man kann sich nun auch schon denken, wie wir nach einem Element suchen
koennten. Sagen wir beispielsweise, wir moechten den Wert finden, der mit dem
Schluessel $27$ assoziiert ist. Da $27$ groesser als $10$ ist, gehen wir
zunaechst nach rechts. Innerhalb vom rechten Kinderknoten der Wurzel haben wir
dann mehr als ein Element. Wir machen also eine binaere Suchen nach der $27$ und
finden diesen Eintrag. Nach Konvention liegt das Element dann links von der $27$
in der Liste. Wir finden diesen Eintrag also und geben den assoziierten Wert zurueck.

### Analyse

Da wir nun schon eine grundlegende Idee davon haben, wie ein solcher
$(a,b)$-Baum aussieht, koennen wir seine Eigenschaften analysieren.

#### Komplexizitaet

Zunaechst ist es aufschlussreich zu sehen, dass die Laufzeit eines
$(a,b)$-Baumes die eines balancierten BSTs nicht uebersteigt, obwohl er soviel
komplexer erscheint. Man ueberlege sich, dass man, um ein Element zu finden,
maximal ganz ans Ende des Baumes muss. Im schlimmsten Fall ist der Baum dann
auch minimal ausgelastet, sodass wir in jedem Knoten wirklich nur $a - 1$
Schluessel haben. In der Wurzel selbst ist das Minimum dan nur ein Schluessel.

Fuer eine vernuenftige Analyse merken wir nun, dass es sinnvoll ist, die beiden
Kinderbaeume der Wurzel separat zu betrachten, da diese symmetrische
Eigenschaften haben und die Wurzel etwas speziell ist. In jedem der beiden
Teilbaeume haben wir jeweils $a - 1$ Schluessel pro Knoten mit Aritaet
bzw. *fan-out* $a$. Auch nehmen wir an, dass die Schluessel perfekt auf die
beiden Baeume verteilt sind, sodass wir also links und auch rechts jeweils
$\lfloor n/2 \rfloor$ Elemente haben. Das Abrunden kommt daher, da wir die
Wurzel nicht zaehlen. Dann muss die Hoehe in jedem der beiden Teilbaeume also
natuerlich $\log_a \lfloor n/2 \rfloor$ sein. Da wir zwei solcher Teilbaeume
haben, nehmen wir diesen Wert noch mal zwei. Dann muessen wir nur noch die
Wurzel beruecksichtigen und formen um:

$$
\begin{align}
	T(n) &= 2 \cdot (\log_a \lfloor n/2 \rfloor) + 1\\
	     &\leq 2 \cdot \log_a n/2 + 1\\
		 &= 2(\log_a n - \log_a 2) + 1\\
		 &= 2\log_a n - \log_a 4 + 1\\
		 &= 2\log_a n - O(1)\\
		 &\in O(\log_a n) = O(\log n)
\end{align}
$$

Im schlimmsten Fall wird eine Suche also logarithmisch in der Anzahl an
Elementen laufen. Da alle Logarithmen gleich schnell laufen, ist die Basis des
Logarithmus auch egal. Es ist in diesem Fall auch egal, ob wir die untere Liste
bei den $n$ mitzaehlen, oder nicht.

#### Hoehe

Fuer die Hoehe des $(a,b)$-Baumes nehmen wir nun explizit an, dass wir unten
eine verketette Liste erhalten. Dann wissen wir naemlich auch, dass wir $n$
Blaetter haben. Wir muessen hierbei aber noch den Sentinelknoten am Ende
mitzaehlen, sodass wir $n + 1$ Knoten haben. Dann wissen wir auch noch, dass die
Wurzel im schlimmsten Fall nur einen Schluessel hat und alle inneren Knoten nur
minimal mit $a$ Schluesseln bestetzt sind. Fangen wir mit dem ersten Knoten an,
so haben wir also in der ersten Ebene zwei Knoten. Ist $t$ dann die Hoehe des
Baumes in Kanten, so bleiben nach dieser ersten Kante noch $t - 1$ weitere
Kanten, wobei die Anzahl Blaetter jedesmal also um Faktor $a$ waechst. Somit
ergibt sich folgende Beziehung:

$$n + 1 \geq 2a^{t - 1}$$

Hierbei ist $n + 1$ moeglicherweise auch greosser als der Ausdruck rechts, weil
wir annehmen, dass die inneren Knoten nur minimal ausgefuellt sind. Dann koennen
wir umformen zu:

$$t \leq 1 + \log_a \frac{n + 1}{2}$$

Da wir auch wissen, dass $t$ eine ganze Zahl ist, koennen bzw. muessen wir den
Logarithmus noch abrunden:

$$t \leq 1 + \lfloor \log_a \frac{n + 1}{2} \rfloor$$

### Operationen

Wir koennen uns nun ansehen, wie man Elemente in eine $(a,b)$-Baum einfuegt,
daraus loescht, darin findet und andere Operationen durchfuehrt.

#### `get`

Um einen Wert, assoziiert mit einem Schluessel, zu finden, traversieren wir den
Baum wie oben schon vorgestellt von oben nach unten. Da die Schluessel in den
Knoten und im ganzen Baum total geordnet sind, koennen wir wie bei BSTs nach
links oder rechts gehen. Hier gibt es jedoch noch zwei Unterschiede. Zum Einen
gibt es eben moeglicherweise mehr als einen Schluessel pro Knoten. Daher koennen
wir nicht in konstanter Zeit einfach nach links oder rechts gehen, sondern
muessen durch eine binaere Suche zuerst den lower-bound (ceiling) des
Schluessels innerhalb des Knoten finden. Finden wir direkt den Schluessel im
Knoten, so gehen wir nach Konvention nach links. Formal als Algorithmus:

1. Sei $\kappa$ der gesuchte Schluessel und $\nu$ der momentane Knoten in der
   Rekursion. Dann rekursiv:
2. Sind wir bei einem Blatt, so pruefe: Ist der Schluessel in diesem Knoten
   $\kappa$?
   1. Falls ja, gibt den Wert im Listenknoten zurueck.
   2. Falls nein, gib eine Fehlermeldung aus.
3. Finde durch eine binaere Suche den kleinsten Schluessel, der groesser oder
   gleich $\kappa$ ist.
4. Gehe nach links von diesem Schluessel.

Im Unterschied zu BSTs ist es hier sogar gar nicht noetig, eine
Fallunterscheidung nach der Beziehung zwischen $\kappa$ und dem durch binaere
Suche gefundenen Schluessel zu machen. Da wir explizit schon nach dem
lower-bound suchen, ist $\kappa$ implizit kleiner oder gleich diesem
Schluessel. Wir koennen also immer links gehen.

#### `insert`

Beim Einfuegen muessen wir etwas mehr Acht geben. Zuerst koennen wir wieder wie
bei `get` nach unten traversieren. Wir gehen nun aber nicht ganz nach unten zu
einem Blatt, sondern muessen auf der vorletzten Ebene, also den inneren Knoten
ueber den Blaettern, aufhoeren. Dann fuegen wir den Knoten an dem Platz ein, den
wir durch binaere Suche gefunden haben. War der Schluessel schon im Baum, so
aktualisieren wir den Wert des Schluessels natuerlich nur. Ansonsten machen wir
dann einen neuen Eintrag in der Liste.

Nun kann es aber sein, dass durch unsere Modifikation die Anzahl Knoten in
diesem unteren inneren Knoten von $b - 1$ auf $b$ geht, sodass wir nun $b + 1$
Kinderknoten haben. Das invalidiert unsere Grad-Invariante und wir muessen
reagieren. Die Idee ist es nun, diesen Knoten einfach aufzuspalten. Haben wir
nun also $b$ Schluessel, so teilen wir diesen Knoten in zwei Knoten auf, die
jeweils $b/2$ Schluessel beinhalten. $b/2$ waere also insbesondere $< b$ und
$\geq a - 1$. Was machen wir nun aber mit diesen beiden Knoten? Die Idee ist es
nun weiter, den Median der $b$ Schluessel als neue Wurzel dieser beiden
Teilknoten in einen neuen Knoten zu geben. Dann koennen wir naemlich den
Medianknoten in den Knoten ueber der untersten Ebene mergen. Sei als Beispiel
ein $(2,3)$-Baum gegeben, dessen unterster linker Knoten nach dem Insert von `C`
uebervoll wird:

```
(1) Anfangs:

       __[F]__
      /       \
  [A | B]   [X | Y]
  /  /   \    \  \
[A | B | F]->[X | Y]

(2) Einfuegen:


       __[F]__
      /       \
  [A | B | C] [X | Y]
  /  /   /  \    \  \
[A | B | C | F]->[X | Y]

(3) Split:


       _[B | F]_
      /    |    \
   [A]    [C] [X | Y]
  /  \   /  \    \  \
[A | B | C | F]->[X | Y]
```

Was waere nun aber gewesen, wenn die Wurzel nun auch voll geworden waere? Nun,
dann setzt man einfach rekursiv bis oben fort. Insbesondere haetten wir im Falle
einer vollen Wurzel aber auch den einzigen Fall, wo ein $(a,b)$-Baum jemals
waechst!

#### `erase`

Das Loeschen in $(a,b)$-Baeumen ist die wohl komplexeste Aufgabe. Gerade wegen
dieser enorm komplexen Methode verwendet man in Praxis eher AVL-Baeume oder
RB-Baeume, da insbesondere die Letzteren eine viel simplere rotationsbasierte
Implementierung von $(2,3)$-Baeumen sind.

Wir betrachten zunaechst das Loeschen von Elementen in einem Blattknoten und
sehen dann, dass sich das Loeschen in inneren Knoten sehr leicht auf das
Loeschen in Blaettern reduzieren laesst.

##### Blattknoten

Genau genommen gibt es drei verschiedene Faelle fuer das Loeschen von Elementen
in Blaettern. Der erste Fall ist der guenstigste: wir muessen nach dem Loeschen
gar nichts machen. Das ist genau dann so, wenn die Anzahl der Knoten vor dem
Loeschen noch $\geq a$ ($a - 1$ ist die untere Schranke) war und danach noch
$\geq a - 1$. Loeschen wir beispielsweise im unteren Baum `B`, so ist das ganz
einfach:

```
(1) Anfangs:

       __[F]__
      /       \
  [A | B]   [X | Y]
  /  /   \    \  \
[A | B | F]->[X | Y]

(2) Loesche B:

       [F]
      /   \
  [A]   [X | Y]
  / \    \  \
[A | F]->[X | Y]
```

Wir koennen wieder dir $(a,b)$-Baum Invarianten pruefen:

1. *Form-Invariante*: Ist der Baum balanciert? Ja, ist er (nach Konstruktion).
2. *Grad-Invariante*: Gilt fuer jeden Knoten $\nu$, dass $a \leq \deg(\nu) \leq
   b$, also hier $2 \leq \deg(\nu) \leq 3$. Ja, das gilt.
3. *Schluessel-Invariante*: Sind die Schluessel in jedem Knoten aufsteigend
   sortiert? Ja, sind sie.

Dann betrachten wir den zweiten Fall, wobei nach dem Loeschen die
*Grad-Invariante* verletzt ist. Dann haben wir also in dem Knoten $\nu$, wo wir
geloescht haben, danach zu wenig Schluessel uebrig. Dann gibt es hier den
Unterfall, dass wir aber in einem der Geschwisterknoten von $\nu$ ausreichend
Elemente haben, dass wir womoeglich von diesen welche stehlen koennten. Das tun
wir auch, mittels einer *Rotation*. Hierbei geben wir das Elternelement von
$\nu$ (dessen Zeiger auf $\nu$ zeigt) in den Knoten $\nu$, um zunaechst die
Grad-Invariante wieder zu erfuellen. Dann geben wir anstelle dieses
Elternelements ein Element aus einem der Geschwisterknoten in die Position des
Elternelements. Denn einer der Geschwisterknoten muss ja ausreichend viele haben
und wegen der Schluessel-Invariante muss die totale Ordnung auf den Schluesseln
danach auch noch erhalten bleiben. Loeschen wir als Beispiel hierfuer
anschliessend zu obigem Beispiel den Schluessel `A` aus dem Baum. Dann koennen
wir, weil der rechte Geschwisterknoten zwei Elemente hat, wobei also $2 \geq a -
1 = 1$, rotieren. Hierzu geben wir das Elternelement `F` in den Knoten von `A`
und dann das linkste Element des Geschwisterknoten in die Position des
Elternelements. Man beachte natuerlich noch die Anpassung der Pointer (baggage).

```
(1) Anfangs:

       [F]
      /   \
  [A]   [X | Y]
  / \    \  \
[A | F]->[X | Y]

(2) Loesche A:

      [F]
     /   \
   []  [X | Y]
  /    /   /
[F]->[X | Y]

(3) Rotiere F in den verletzten Knoten:


      [ ]
     /   \
   [F]  [X | Y]
  /    /   /
[F]->[X | Y]

(4) Rotiere X in die Position des Elternelements:

      [X]
     /   \
   [F]  [Y]
  / \    |
[F | X] [Y]
```

Da hier nun alle Knoten zwei Kinder haben, gilt die Grad-Invariante
wieder. Dasselbe haetten wir auch mit einem linken Geschwisterknoten machen
koennen, wenn es einen gegeben haette.

Betrachten wir nun aber den letzten Fall, wo nach dem Loeschen in $\nu$ keine
Rotation moeglich ist. Das ist also immer genau dann, wenn die Geschwisterknoten
eines verletzten Knoten (der eine Baum-Invariante verletzt) minimal saturiert
sind, also die Anzahl ihrer Schluessel genau $a - 1$ (das Minimum) ist. Dann
ueberlegen wir uns aber folgendes: Nach dem Loeschen eines Elements in $\nu$ ist
$\nu$ also verletzt. Dann hat $\nu$ momentan $a - 2$ Knoten, obwohl ein Minimum
von $a - 1$ gefordert ist. Dann wissen wir gerade auch, dass die
Geschwisterknoten $a - 1$ Schluessel haben. Wenn wir also $\nu$ mit seinem
Geschwisterknoten mergen wuerden, dann haetten wir $a - 2 + a - 1 = 2a - 3$
Schluessel in einem Knoten. Dann muessten wir aber irgendwie das Elternelement
im Elternknoten loswerden. Wir koennten es theoretisch zwischen $\nu$ und den
Geschwisterknoten geben. Wegen der totalen Ordnung waere dies korrekt und das
Problem geloest. Dann haetten wir also $2a - 3 + 1 = 2a - 2$ Schluessel. Passen
die noch in einen Knoten. Nun, oben hatten wir die Voraussetzung genannt, dass
$b \geq 2a - 1$ sein muss. Da $b$ in Kanten gemessen ist, haben wir Platz fuer
mindestens $b - 1 = 2a - 2$ Schluessel. Also genau passend! Jetzt wissen wir
auch, wieso wir diese Voraussetzung fuer $b$ haben.

Diese Operation, die wir `merge` nennen, koennen wir uns nun an einem Beispiel
ansehen. Wir loeschen hierfuer aus obigem Baum das Element `Y`. Dann hat dieser
Knoten, in welchem `Y` war, kein Element mehr, wobei mit $a = 2$ das Minimum ein
Element ist. Auch merken wir, dass der Geschwisterknoten gerade dieses Minimum
an Schluesseln hat. Wir koennen also nicht rotieren! Deswegen mergen wir. Das
sieht dann so aus:

```
(1) Anfangs:

      [X]
     /   \
   [F]  []
  / \    |
[F | X] []

(2) Gib das Elternelement `X` in den invalidierten Knoten:

      []
     /   \
   [F]  [X]
  / \    |
[F | X] []

(3) Merge die Geschwisterknoten:

   []
  / \
[F | X]
 |   |
[F | X]
```

Wir merken hier nun, dass die Wurzel leer geworden ist. Das bedeutet nun
natuerlich, dass wir die Wurzel loeschen koennen und eine neue Wurzel
bekommen. Insbesondere ist dies wiederum die einzige Situation, wo ein
$(a,b)$-Baum schrumpft! Der letztendliche Baum sieht also so aus:

```
[F | X]
 |   |
[F | X]
```

##### Innere Knoten

Wie funktioniert nun das Loeschen in einem inneren Knoten eines $(a,b)$-Baumes?
Die Idee ist die folgende: wir finden wie bei Hibbard-Deletion den Vorgaenger
oder Nachfolger des Schluessels $\kappa$, den wir loeschen wollen. Dann
vertauschen wir den zu loeschenden Schluessel mit dem Nachfolger oder
Vorgaenger. Da Nachfolger oder Vorgaenger jeweils immer in einem Blatt waeren,
ist $\kappa$ nun in einem Blatt. Das Loeschen des Schluessels funktioniert nun
also exakt ident wie oben beschrieben!

Es sei aber noch angemerkt, dass es enorm wichtig ist, bei der `merge` Operation
algorithmisch exakt vorzugehen. Die Reihenfolge sollte so sein:

1. Sei:
   * $\nu$ der Knoten, dessen Grad-Invariante verletzt ist.
   * $\rho$ das Elternelement von $\nu$, dessen linker Zeiger $\rho_l^\star$
     also auf $\nu$ zeigt.
   * $\mu$ der Geschwisterknoten von $\nu$, mit welchem wir mergen.
2. Ziehe $\rho$ in den Knoten von $\nu$.
3. Vereinige $\nu$ mit $\mu$.
4. Loesche den Eintrag vo $\rho$ im Elternknoten.
5. Wird dadurch die Grad-Invariante vom Elternknoten invalidiert, fahre rekursiv
   fort.

Sehen wir uns an, wieso das so wichtig ist. Sei hierzu der unten abgebildete
$(2,3)$-Baum gegeben (ohne verkettete Liste). Wir loeschen darin das Element
`5`:

```
(1) Anfangs:

     _[10]__
    /       \
  [4]      [15]
 /   \     /  \
[1]  [5] [12] [20]

(2) Loesche 5:

     _[10]__
    /       \
  [4]      [15]
 /   \     /  \
[1]  []  [12] [20]
```

Da der Geschwisterknoten dieses Knoten genau zwei Schluessel hat, koennen wir
nicht rotieren, sondern muessen mergen. Jetzt sind wir dumm und denken uns
schnell: mergen volle gas bam oida!!

```
       [10]
      /   \
  [1 | 4]  [15]
           /  \
         [12] [20]
```

Eh, wtf? Was nun? Wir haben die Form-Invariante des Baumes verletzt! Waeren wir
algorithmisch vorgegangen, haetten wir keinen Fehler gemacht:

```
(1) Loesche 5:

     _[10]__
    /       \
  [4]      [15]
 /   \     /  \
[1]  []  [12] [20]

(2) Ziehe das Elternelement in den invalidierten Knoten:

     _[10]__
    /       \
  [ ]      [15]
 /   \     /  \
[1]  [4] [12] [20]

(3) Vereinige die Geschwisterknoten:

     _[10]__
    /       \
  [ ]      [15]
   |       /  \
[1 | 4] [12] [20]

(4) Blatt ist OK, bottom-out nach oben. Innerer Knoten ist invalidiert!
(5) Rotation nicht moeglich, also merge.
(6) Ziehe das Elternelement in den invalidierten Knoten:

      __[ ]__
     /       \
   [10]     [15]
   /        /  \
[1 | 4]  [12] [20]

(7) Vereinige die Geschwisterknoten:

      [ ]
       |
   [10 | 15]
   /    \   \
[1 | 4]  [12] [20]

(8) Loesche die Wurzel, da leer:

   [10 | 15]
   /    \   \
[1 | 4]  [12] [20]

(9) Profit.
```

#### `build`

Wie bei BSTs gibt es auch fuer $(a,b)$-Baeume einen `build`-Algorithmus, der aus
einer sortierten Sequenz einen $(a,b)$ Baum baut. Wir baeuen hierbei den Baum
ohne verkettete Liste, da diese immer noch in linearer Zeit nachtraegelich
konstruiert werden kann. Die Idee ist die folgende: Haben wir eine Sequenz von
$n$ Elementen, so suchen wir aus diesen $a - 1$ Pivots aus, die also dann genau
$a$ Teilsequenzen von einander abtrennen. Diese $a - 1$ Pivots geben wir als
minimal erlaubte Anzahl an Schluesseln in den Wurzelknoten. Dann rekursieren wir
jeweils fuer die $a$ Segmente und erzeugen daraus neue $(a,b)$-Baeume, die wir
entsprechend an die $a - 1$ Pivots anknuepfen. Kommen wir dabei rekursiv an eine
Sequenz mit $< b$ Elementen, so koennen wir diese dann auch gleich in einen
Blattknoten geben und zurueckgeben. Somit werden also alle inneren Knoten
inklusive der Wurzel genau $a - 1$ Elementen enthalten und die Blaetter zwischen
$a - 1$ und $b - 1$. Formal:

1. Sei $\mathbb{S}$ eine sortierte Sequenz von Zahlen, dann rekursiere:
2. Ist $|S| < b$, so gib $S$ in einen Knoten $\nu$ und gib ihn zurueck.
3. Ist $|S| = b$, so ist das ein grauenhafter Edge-Case. Selektiere das Element
   bei $b/2$ als einziges Wurzelelement mit linkem und rechten Knoten
   entsprechend links und rechts von Index $b/2$.
4. Ist $|S| > b$, so sei $\nu := \emptyset$ ein anfangs leerer Knoten. Dann
   iteriere fuer jedes $i \in \{1, ..., a - 1\}$:
   1. Waehle den Pivot-Index $p := \lfloor \frac{i \cdot |S|}{a} \rfloor$.
   2. Waehle $S_p$ als $i - 1$-ten Schluessel fuer $\nu$.
   3. Setze den linken Zeiger dieses Elements auf den Rueckgabewert des
      rekursiven Aufrufs fuer die Sequenz $S_{i'}, ..., S_{i - 1}$, wo $i'$ das
      vorherige $i$ ist und initial gleich dem ersten Index der Sequenz.
5. Rekursiere noch einmal fuer den rechten Zeiger vom letzten Schluessel in
   $\nu$ und der Teilsequenz $S_{a - 1}, ..., S_n$.
6. Gib $\nu$ zurueck.

#### `concatenate`

Eine Operation, die man effizient auf $(a,b)$-Baeumen durchfuehren kann, ist die
Konkatenation. Seien hierfuer $T_1, T_2$ zwei Baeume. Gilt nun, dass alle
Schluessel in $T_2$ strikt groesser als alle in $T_1$ sind, so koennen wir den
kleineren der beiden an den anderen heften. Sagen wir als Beispiel, dass $T_1 <
T_2$ (fuer alle Schluessel), dann koennen wir sie also konkatenieren. Sei dann
$T_2$ auch der kleinere Baum. Dann finden wir den aeussersten inneren Knoten,
der auf der selben Ebene wie die Wurzel von $T_2$ ist. Nun koennen wir sie auf
dieser Ebene mergen. Das einzige Problem ist, dass man Konflikte fuer die
inneren Knoten erhalten kann. Hat naemlich die Wurzel von $T_2$ ein linkes Kind
und der entsprechende Knoten in $T_1$ ein rechtes Kind, dann wissen wir nicht,
welches wir als mittleres Kind im konkatenierten Baum waehlen sollen. Die
Loesung hierfuer waere, rekursiv ganz nach unten zu gehen, bis wir diesen
Konflikt nur mehr in den Blaettern haben. Dann koennen wir diese Knoten mergen
und somit rekursiv den Reissverschluss schliessen. Fuer jede Ebene muss man
dann aber noch sicherstellen, dass die Schlueselzahl die Grad-Invariante nicht
verletzt.

```
(1) Anfangs:

T1:

      __[5]_
     /      \
   [1]      [7]
  /  \     /  \
[0]  [2] [6] [8]

T2:
    [150]
   /     \
[100]  [200]

(2) Konflikt:

      __[5]_
     /      \
   [1]      [7]      [150]
  /  \     /  \  ?  /     \
[0]  [2] [6]  [8] [100]  [200]

(3) Reissverschluss:

      ____[5]____
     /           \
   [1]      [  7 | 150  ]
  /  \     /     |       \
[0]  [2] [6]  [8 | 100]  [200]
```

#### `split`

Es gibt nun auch die Moglichkeit, einen $(a,b)$-Baum aufszuspalten. Sei hierfuer
$\kappa$ irgendein Schluessel im Baum. Dann gibt es also einen Pfad von der
Wurzel zum enstprechenden Listenblatt, das $\kappa$ enthaelt. Die Idee ist es
nun, den Baum entlang dieses Pfades einfach in linke und rechte Teilbaeume
aufzuspalten. Dadurch ergeben sich neue $(a,b)$-Baeume, denen jeweils ein Zeiger
fehlt. Somit koenenn wir sie auch enstprechend links und rechts zu einem
grossen $(a,b)$-Baum konkatenieren.
