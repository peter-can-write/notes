# Heaps

Ein *Heap* ist eine Datenstruktur, die verwendet wird, um Daten so zu
strukturieren, dass man schnellen Zugriff auf das Element mit hoechster
Prioritaet hat. Deswegen nennt man Heaps auch *Priority Queues*.

Konkret betrachten wir bei der Diskussion von Heaps und Priority Queues eine
Menge von Elementen, die bezueglich einer Prioritaet total geordnet sind. Dabei
sollte ein Heap bzw. eine Priority Queue das folgende Interface unterstuetzen:

* `build({a, b, c, ...})`: Baut aus einer beliebig geordneten Menge eine Heap
  Struktur.
* `insert(element)`: Fuegt ein neues Element in die Heap-Struktur ein.
* `top()`: Gibt das Element mit hoechster Prioritaet zurueck.
* `pop()`: Entferne das Element mit hoechster Priotitaet und gib es
  zurueck.

Manchmal ist es auch moeglich, Heaps so zu implementieren, das man bestimmte
Knoten in der Struktur direkt addressieren kann und dann effizient Operationen
auf diesen Knoten durchfuehren kann. Beispielsweise koennte es effizient sein,
einen Knoten direkt aus einem Heap zu loeschen. Auch wollen wir einen Schluessel
bezueglich seiner Prioritaet oft erhoehen oder verringern, was ueber eine
direkte Addressierungsmoeglichkeit auch ginge. Mit einer "direkten
Adressierungsmoeglichkeit" meinen wir hier einen "Handle", ueber welchen wir den
Knoten referenzieren koennen. Dieser Handle wuerde dann beim Einfuegen
zurueckgegeben werden. Insgesamt ist folgendes optionale Interface moeglich:

* `Handle insert(element)`: Fuege das Element ein und gib ein Handle auf das
  Element zurueck.
* `erase(handle)`: Loesche das durch das Handle referenzierte Element.
* `changeKey(handle, new_priority)`: Veraendere die Prioritaet des durch
  das Handle referenzierten Elements. Haben niedrigere Elemente hoehere
  Prioritaet, nennen wir diese Methode oft auch `decreaseKey()`.
* `merge(other_heap)`: Vereinigt einen Heap mit einem anderen Heap.

## Implementierung

Die erste Idee, die wir fuer eine Implementierung einer solchen Priority Queue
haben koennten, waere ein einfaches Array. Hierbei gibt es zwei Varianten: wir
koennen das Array sortieren, oder nicht. Je nachdem ergeben sich andere
Eigenschaften.

Sortieren wir das Array, so kostet die initiale `build` Operation linearithmisch
Zeit, wenn wir einen schnellen Sortieralgorithmus verwenden. Das Auffinden des
Elements mit hoechster Prioritaet ginge dann jedoch sehr schnell, da es einfach
das erste Element waere. Die groesste Problematik ergibt sich jedoch erst durch
das Einfuegen eines neuen Elements. Mittels Binary Search koennten wir zwar in
logarithmischer Zeit die Einfuegeposition finden. Da diese Einfuegeposition dann
aber ebenso der erste Index sein koennte, wuerde ein Einfuegen an dieser
Position lineare Zeit kosten, um die anderen Elemente jeweils zu verschieben.

Halten wir das Array unsortiert, so kostet die initiale `build` Operation
natuerlich nur konstante Zeit. Da das Einfuegen eines neuen Elements immer am
Ende der Liste passieren koennte, waere Einfuegen ebenso konstant. Aber wir
erinnern uns, dass einer der Beweggruende hinter einem Heap das schnelle
Auffinden des Elements mit hoechster Prioritaet waere. Das ginge bei einer
unsortierten Sequenz nur ueber eine lineare Suche.

Alternativ koennten wir noch verkettete Listen untersuchen. Bei sortierten
Listen haetten wir leider ebenso lineare Einfugegzeit. Wir koennen naemlich
insbesondere keinen Binary Search machen, um die Einfuegeposition zu finden. Das
Einfuegen an der richtigen Position selbst waere zwar konstant, aber das hilft
uns hier nichts. Bei unsortierten Listen wuerden wir gar keinen Vorteil
gegenueber unsortierten Arrays gewinnen.

## Binaerbaum

Wir sehen, das eine Implementierung eines Heaps ueber Arrays oder Listen nicht
maximal effizient ist. Eine weitere Idee ist es nun, einen Heap ueber einen
Binaerbaum zu konstruieren. Die Hierarchie im Baum koennte die Priorisierung
darstellen. Auch waere jegliche Traversierung von der Wurzel zu einem Blat in
logarithmischer Zeit moeglich. Insgesamt haetten wir somit konstante Zugriffzeit
auf die Wurzel, also das Element mit hoechster Prioritaet, logarithmische Zeit
um ein Element einzuordnen und daher maximal linearithmische Zeit, um einen Heap
aus einer Sequenz aufzubauen.

Genauer haetten wir zwei Anforderungen bzw. *Invarianten* fuer unseren binaeren
Heap:

1. Die *Heap-Invariante*, dass die Prioritaet eines Knotens immer groesser oder
   gleich jener seiner Kinder ist.
2. Die *Form-Invariante*, dass der Binaerbaum *fast-vollstaendig* sein muss.

Mit einem *fast-vollstaendigen* Binaerbaum meinen wir hierbei einen solchen
Binaerbaum, der in seiner untersten Ebene keine Luecken zwischen den Blaettern
hat. Ein fast-vollstaendiger Binaerbaum muss also nicht notwendigerweise maximal
viele Blaetter haben, aber wenn er Blaetter hat, dann muessen diese strikt
nebeneinander liegen.

Wozu aber diese letzte Invariante? Sie ist wichtig fuer die konkrete
Implementierung eines Heaps. Das tolle its naemlich, dass wir binaere Heaps
in einem Array speichern koennen! Das gibt natuerlich tolle Cache-Effizienz und
einfache Implementierung von Operationen. Betrachten wir hierzu den folgenden
Heap:

```
(0)       X
        /   \
(1)    C     O
      / \   / \
(2)  A   B M   K
```

Hierbei haben wir also einen Heap, der Elemente nach lexikographischer Ordnung
sortiert und Elementen mit hoeherer lexikographischer Ordnung hoehere Prioritaet
zuweist. Einen solchen Heap nennt man dann auch *MaxHeap*. Das Gegenteil dazu
waere ein *MinHeap*. In einem Array koennte man diesen Heap nun so speichern:

```
[_ X C O A B M K] (Elemente)
   0  1   2   2   (Level)
```

Die Wurzel des Baumes, also das Element mit hoechster Prioritaet, waere also in
Index Eins. Wie angedeutet also nicht in Index Null. Wie man unten sehen werden,
hat das Vorteile fuer die Implementierung. Wie koennen wir nun aber schnell auf
die Kinder oder den Elternknoten eines Knotens (sprich: eines Index in das
Array) zugreifen? Gerade hier ist die Implementierung eines Binaerbaumes ueber
ein Array sehr vorteilhaft. Sei hierfuer $0 < i < n$ der Index eines
Knoten. Dann finden wir:

* Das linke Kind an Index $2i$.
* Das rechte Kind an Index $2i + 1$.
* Den Elternknoten an Index $\lfloor i/2 \rfloor$.

Ueberlegen wir uns, wieso das so ist. Wir wissen, das in einem Binaerbaum eine
bestimmte Ebene immer doppelt soviele Elemente hat wie der gesamte Baum darueber
und sogar noch eins mehr Elemente. Hat der Binaerbaum beispielsweise drei
Elemente, also eine Wurzel und zwei Blaetter, und erhoehen wir diesen Baum nun
um eine Ebene, so erhaelt er vier neue Blaetter. Sei also $n$ die Anzahl an
Knoten in einem Baum und $n'$ die Anzahl Elemente, nach dem Einfuegen einer
neuen Ebene. Dann gilt $n' = 2n + 1$. Sei nun $i$ der Index eines Knoten in
unserem Heap. Nehmen wir hierbei auch an, das wir bei $1$ anfangen zu zaehlen
und $i$ gerade der letzte (rechteste) Knoten in einer Ebene ist. Dann zaehlt $i$
also gerade die Anzahl Knoten im Baum bis $i$. Wir wissen dann, dass die
naechste Ebene, die also die Kinder von $i$ enthaelt, $2i + 1$ Elemente
enthalten muss. Hierbei zaehlt $2i + 1$ also wieder die Anzahl an Knoten, aber
nun mit dieser neuen Ebene. Dieser letzte Knoten ist aber gerade auch das rechte
Kind von $i$, also dem rechtesten Knoten in der Ebene darueber. Wo finden wir
dann das linke Kind von $i$? Nun ja, einen Index links, also bei $2i$. Sei nun
$j = i - 1$ also der Knoten links vom rechtesten in der Eben von $i$. Dann gibt
uns also $2j$ das linke Kind von $j$. Das ist nun auch intuitiv klar, weil $2j =
2(i - 1) = 2i - 2$ offensichtlich gerade zwei Knoten vor dem linken Kind vo $i$
ist!

Auch wird nun klar, wieso es sinnvoll ist, bei Eins anzufangen. So ist $\lfloor
i/2 \rfloor$ fuer einen Kinderknoten naemlich entweder $\lfloor 2i/2
\rfloor = i$ oder $\lfloor (2i + 1)/2 \rfloor = i$. Ueber Integer-Division
koennen wir das dann sehr effizient implementieren. Sonst meussten wir bei der
Ermittlung des Elternknoten eine Fallunterscheidung danach machen, ob der Knoten
geraden oder ungeraden Index hat.

Letztlich ergibt sich nun auch, wieso die Form-Invariante von Heaps so wichtig
ist. Nur so koennen wir naemlich sicherstellen, das der Heap kontinuierlich in
ein Array passt.

### Invariantenerhaltung

Oben haben wir also die zwei Invarianten von Heaps genannt und untersucht. Was
passiert aber nun, wenn eine der Invarianten durch eine Operation invalidiert
wird? Das wollen wir uns ansehen.

#### Fall A

Nehmen wir an, ein beliebiges Element im Heap erhaelt ploetzlich hoehere
Prioritaet als sein Elternknoten. Das stellt eine Invalidierung der
Heap-Eigenschaft dar, da wir annehmen, das Kinder immer niedrigere Prioritaet
haben als ihre Eltern. Das Problem kann aber sehr einfach behoben werden:

1. Sei $\nu$ ein Knoten, dessen Prioritaet groesser ist, als die seines
   Elternknoten.
2. Vertausche $\nu$ mit seinem Elternknoten.
3. Ist die Invariante noch immer verletzt, gehe zu (1)

Diese Operation nennt man die `swim` oder `siftUp` Operation, da das Element
nach oben schwimmt, bis die Heap-Invariante wiederhergestellt ist. Ihre Laufzeit
ist $O(\log_2 n)$.

#### Fall B

Nehmen wir nun konvers an, das ein beliebiger Knoten im Heap eine niedrigere
Prioritaet erhaelt, als zuvor. Bezueglich den Eltern dieses Knoten ist das
egal. Der Knoten hatte vorher nach Heap-Invariante eine niedrigere
Prioritaet. Ist sie jetzt niedgriger macht das nichts. Aber: Laut
Heap-Invariante muss der Knoten insbesondere auch hoehere Prioritaet haben als
alle seine Kinder. Wenn die Prioritaet dieses Knoten also nun reduziert ist,
koennte diese Eigenschaft verletzt sein. Die Loesung hierzu ist die folgende:

1. Sei $\nu$ ein Knoten, dessen Prioritaet kleiner ist, als seine Kinder.
2. Waehle das Kind mit der hoeheren Prioritaet.
3. Vertausche den Knoten $\nu$ mit diesem Kind.
4. Hat $\nu$ noch immer niedrigere Prioritaet als seine Kinder, gehe zu (1).

Diese Operation nennt man `sink` oder `siftDown`. Sie laeuft in logarithmischer
Zeit, also $O(\log_2 n)$.

### Operationen

Ueber die eben vorgestellten `swim` (`siftUp`) und `sink` (`siftDown`)
Operationen koennen wir nun auch alle anderen oben vorgestellten Routinen
durchfuehren. Dies wird im folgenden genauer beschrieben.

#### `insert`

Das Einfuegen eines neuen Elements in einen Heap geht nun wie folgt:

1. Gib den Knoten ans Ende des Heaps.
2. Fuehre `swim` auf dem Knoten auf, um ihn so weit nach oben in den Heap zu
   schieben, wie noetig.
3. Gibt den Index des Knoten im Array zurueck.

Diese Operation kostet $O(\log_2 n)$ amortisierte Zeit, um das Element nach oben
zu `swim`-en. Die Amortisierung ist notwendig, weil wir mit dynamisch wachsenden
und schrumpfenden Arrays arbeiten.

#### `pop`

Diese Operation gibt das Element mit hoechster Prioritaet zurueck und loescht es
auch gleichzeitig aus der Struktur. Wir koennen sie nun so implementieren:

1. Vertausche den ersten Knoten mit dem letzten Element im Heap.
2. Dekrementiere die Groesse des Heaps, sodass das Element mit hoechster
   Prioritaet, das nun also am Ende des Arrays liegt, nicht mehr im Heap drin
   ist.
3. Fuehre `sink` auf dem ersten Element aus.

Durch `sink` ergibt sich eine logarithmische Laufzeit fuer diese Operation. Da
wir aber mit dynamischen Arrays arbeiten, ist diese Laufzeit nur die amortisierte.

#### `erase`

Erase laesst sich einfach dadurch implementieren, das wir dem zu loeschenden
Element unendliche Prioritaet geben. Dann koennen wir es an die Oberflaechse
schwimmen lassen. Letztlich rufen wir einfach `pop`!

1. Sei $\nu$ der Knoten mit Index $i$, der zu loeschen ist.
2. Setze die Prioritaet von $\nu$ auf Unendlich.
3. Schwimme $\nu$ ganz nach oben.
4. Rufe `pop`.

#### `changeKey`

Das Ziel dieser Operation ist es, ein durch seinen Arrayindex adressiertes
Element zu veraendern. Wir muessen hierbei ledglich sicherstellen, dass danach
die Heap-Invariante erhalten bleibt. Die Implementierung lautet also:

1. Sei $\nu$ der Knoten, der durch Index $i$ referenziert ist und $\kappa$ der
   neue Wert fuer den Knoten $\nu$.
2. Setze den Wert von $\nu$ auf $\kappa$.
3. Eribt der neue Werte eine hoehere Prioritaet als der vorherige, muessen wir
   uns nur darum kuemmern, dass $\nu$ moeglicherweise weiter rauf muss. Wir
   rufen also `swim` auf dem Knoten auf.
4. Andernfalls muss die Prioritaet gesunken (oder gleich geblieben) sein. Wir
   rufen also `sink` auf. War die Prioritaet tatsaechlich gleich, ist das egal,
   da `sink` "idemptotent" ist.

#### `build`

Die `build` Operation ist dafuer zustaendig, aus einer Sequenz von Elementen
einen Heap zubauen. Wir haben hierfuer zwei Moeglichkeiten. Die naive Variante
fuegt einfach mittels `insert` jedes einzelne Element in den anfangs leeren
Heap. Da eine `insert` Operation logarithmisch ist, kostet diese Operation $O(n
\log_2 n)$ Zeit. Die viel bessere Variante nutzt aber folgende Beobachtung: die
rechte Haelfte der Sequenz waeren im Heap gerade die Blattknoten. Wenn wir also
in der Mitte anfangen, nach links gehen und sukzessive `sink` auf jedem Knoten
aufrufen, so stellen wir Ebene fuer Ebene im Baum die Heap-Eigenschaft
her. Insbesondere ist toll, dass man zeigen kann, das diese `sink`-basierte
Variante in *linearer* Zeit laeuft! Das ist also eine tolle Steigerung. Der
Algorithmus lautet also:

1. Setze $i = \lfloor n/2 \rfloor$ (in Praxis einfach `n//2`) und iteriere
   bis $i = 0$ (nicht $1$!). Faengt man nicht bei Eins an zu zaehlen, kann man
   auch bei $\lfloor n/2 \rfloor - 1$ anfangen.
2. Fuer jedes $i$, ruefe `sink` auf.

### Ueberlegungen

Hier noch zwei kurze Ueberlegeungen zu Binary Heaps. Zum Einen machen wir die
Beobachtung, das wir die Implementierung eines Binaerbaums duch ein Array auch
auf binaere Suchbaeume ausweiten koennten. Das wuerde gut funktionieren, wenn
wir nur Einfuegeoperationen und Suchen machen. Denn ein Array hat immer erhoehte
Cache-Effizienz. Das einzige Problem ergibt sich durch das Loeschen eines
beliebigen Elements, da diese bei einem binaeren Suchbaum dann immer linear
waere. Das gilt auch fuer die Wurzel, da man `sink` und `swim` auch nicht
implementieren koennte.

In diesem Fall haben wir unseren Heap durch einen *binaeren Heap*
implementiert. Wir koennten aber auch eine hoehere Aritaet waehlen. Dadurch
wuerden sich natuerlich Traversierung des Baumes von der Wurzel zu einem Blatt
verbessern, wobei `sink` etwas schwieriger zu implementieren waere. Wir machen
hier naemlich die folgende Beobachtung: Sei $\alpha$ die Aritaet des Heaps und
$i$ ein Knotenindex, dann ist $\alpha i$ das *vorletzte Kind* des Knoten
$i$. Das skaliert auch fuer $\alpha = 2$, da das "linke" Kind gerade der
vorletzte Knoten waere.

```
	     1
	  /  |   \
	2	 3       4
   /     |        \
5 6 7  8 9 10  11 12 13
```

Hier waere $\alpha = 3$ und $\alpha \cdot 2 = 6$ der mittlere, also vorletzte
Knoten. Aehnliches gilt fuer hoeher Aritaet.

## Binomial-Heaps

Eine weitere Implementierung von Heaps, sogenannte *Binomial-Heaps*, verwendet
nicht Arrays, sondern explizite Baumstrukturen. Binomial-Heaps bestehen aus
*Binomialbaumen*, welche eine faszinierende Datenstruktur sind. Wir wollen sie
und ihre Implementierung zunaechst verstehen und folgich Binomial-Heaps
betrachten.

### Binomial Baeume

Binomialbaeume sind wie Binaere Heaps in dem Sinn, dass sie auch die
Heap-Invariante erhalten. Sie haben nun aber eine andere
Form-Invariante. Genauer definieren wir fuer einen Binomialbaum einen Rang
$r$. Ein Binomialbaum mit Rang $r = 0$ besteht aus exakt einem Knoten. Der
Binomialbaum mit Rang $r = 1$ geht nun so aus dem mit Rang Null hervor, dass man
zwei solcher Rang-Null Binomialbaeume bei ihrer Wurzel verbindet. Jeder weitere
Binomialbaum vom Rang $r$ geht dann auf gleiche Weise aus zwei Binomial Baeumen
vom Rang $r - 1$ hervor.

```
r = 0
 o

r = 1
 o
 |
 o

r = 2
  o
 /|
o o
|
o

r = 3

	__o
   / /|
  o o o
 /| |
o o o
|
o
```

Da wir nicht nur die Form-Invariante, sondern auch die Heap-Invariante erhalten
wollen, haengen wir hierbei immer die Wurzel mit kleinerer Prioritaet an die
Wurzel mit hoeherer Prioritaet an. Somit bleibt induktiv immer der Knoten mit
hoeherer Prioritaet an der Wurzel des ganzen Binomialbaumes und jeden
Teilbaumes.

Wir wissen nun also, dass ein Binomialbaum vom Rang $r + 1$ aus zwei Baeumen vom
Rang $r$ hervorgeht. Interessant ist nun auch, dass wenn wir die Wurzel eines
Binomialbaumes vom Rang $r$ entfernen, dieser in gerade $r$ Binomialbaeume vom
Rang $r - 1, ..., 0$ zerfaellt:


```
r = 3

	__o
   / /|
  o o o
 /| |
o o o
|
o

== -Wurzel ==>

  o o o
 /| |
o o o
|
o
 2  1 0
```

Woher kommt nun aber der Name eines Binomialbaumes? Die Antwort ist: ein
Binomialbaum hat auf Level $k \in {0, ..., r}$ gerade ${r \choose k}$ Knoten!
Um das zu verstehen, muessen wir uns ansehen, wie eine Ebene eines neuen
Binomialbaumes vom Rang $r$ aus den entsprechenden Ebenen der Baeume vom Rang $r
- 1$ hervorgeht. Da wir die Heap-Invariante erhalten wollen, verbinden wir ja
die kleinere Wurzel des einen Baumes mit der groesseren Wurzel des anderen
Baumes. Diese beiden Wurzeln sind dann also verbunden und somit insbesondere um
eine Stufe verschoben. Die kleinere Wurzel ist ja dann auf derselben Ebene wie
die Kinder der groesseren Wurzel. Auch wissen wir jetzt, dass sich die Anzahl an
Knoten auf einer Ebene $k$ aus dem Binomialkoeffizient ${r \choose k}$
ergibt. Sei nun $k$ eine Ebene im Baum mit der groesseren Wurzel. Wegen der
Verschiebung bezueglich dem Baum mit kleinerer Wurzel kommt diese Ebene $k$
neben die $k - 1$-te dieses anderen Baumes. Somit ergibt sich die Anzahl an
Knoten nach der Konkatenierung aus:

$${r - 1 \choose k - 1} + {r - 1 \choose k}$$

Nun ist es so, dass diese Summe gerade die Pascal'sche Identitaet des
Binomialkoeffizienten ${r \choose k}$ ist! Es gilt also:

$${r \choose k} = {r - 1 \choose k - 1} + {r - 1 \choose k}$$

Induktiv folgt also die Aussage. Auch ergeben sich so weitere interessante
Eigenschaften:

* Da die Summe der Binomialkoeffizienten ${n \choose i} \forall 0 \leq i \leq n$
  gerade $2^n$ ist, hat ein Binomialbaum vom Rang $r$ gerade $2^r$ Knoten.
* Da ${r \choose 1} = 1$ hat die Wurzel eines Binomialbaumes gerade $r$ Kinder.
* Der *Grad* eines jeden Knoten in einem Binomialbaum ist maximal $r$.
* Da $k \in \{0, ..., r - 1\}$ hat ein Binomialbaum vom Rang $r$ eine Hoehe von
  $r$ in Kanten oder $r + 1$ in Knoten.

### Bimomial Heaps

Da wir nun das Konzept von Binomialbaeumen kennen, koennen wir uns
Binomial-Heaps ansehen. Ein Binomial-Heap wird als eine *verkettete Liste* von
Binomialbaeumen implementiert. Hierbei haben wir pro Rang maximal einen solchen
Binomialbaum. Das Element mit hoechster Prioritaet ist dann die Wurzel eines der
Baeume, auf welchen wir einen Zeiger haben. Sehen wir uns nun an, wie
Operationen auf einem solchen Baum implementiert werden.

#### `merge`

Wir untersuchen zuerst die `merge` Operation, da viele andere auf ihr
basieren. Wie oben angemerkt haben wir fuer jeden Rang maximal einen einzigen
Binomialbaum. Das ist so, weil zwei Binomialbaeume vom selben Rang $r$ einen
Binomialbaum vom Rang $r + 1$ erzeugen. Betrachten wir nun also die verkettete
Liste eines Binomial-Heaps als ein Bitstring, mit Einsen dort wo wir einen
Binomialbaum haben und Nullen wo nicht, so ergibt sich das Mergen zweier
Binomial-Heaps eben wie Binaeraddition! Denn wir haben fuer das Mergen zweier
Baeume bzw. die Addition zweier "Bits" folgende Moeglichkeiten:

* $0 + 0 = 0$: wenn wir keine Baeume fuer einen gewissen Rang haben, erhalten
  wir auch keinen solchen Baum.
* $1 + 0 = 0 + 1 = 1$: wenn nur einer der Heaps einen Baum mit Rang $r$ hat, so
  wird es folglich auch nur diesen Baum mit Rang $r$ geben.
* $1 + 1 = 0$ mit Carry: wenn beide Heaps einen Baum vom Rang $r$ haben, ergibt
  das einen Baum vom Rang $r + 1$. Die Baeume vom Rang $r$ verschwinden also ($=
  0$), wir haben aber einen Carry-Bit. Wie bei normaler Binaeraddition kann
  dieses Carry-Bit auch weiterpropagieren.

Sagen wir als Beispiel, dass wir einen Binomial-Heap mit Binomialbaeumen mit
Raengen $7, 5, 2, 0$, sowie einen weiteren mit Raengen $5, 3, 2$ haben. Dann
koennen wir diese Listen als 8-Bit Bitsequenzen darstellen und einfache
Binaeraddition durchfuehren:

```
10100101
00101100
--------
11010001
```

Wir erhalten also einen neuen Heap mit Baeumen vom Rang $7, 6, 4, 0$. Wie wir
sehen hatten wir zwei Baeume vom Rang Zwei. Diese haben einen neuen Baum vom
Rang Drei ergeben. Da es aber auch schon einen Baum mit Rang Drei gab, ergibt
der Merge bzw. die "Addition" also wiederum einen Baum vom Rang Vier. Das ergibt
folgenden Algorithmus:

1. Seien $A,B$ die beiden Binomial-Heaps, die es zu mergen gilt. Dann, fuer
   jeden Binomialbaum in Heap $B$:
   2. Sei $r$ der Rang dieses Baumes aus $B$.
   3. Siehe, ob Heap $A$ ebenso einen Baum mit Rang $r$ hat.
	  1. Falls nicht, fuege den Baum in $A$ ein und fertig.
	  2. Fall schon:
		 1. Verbinde sie zu einem Baum vom Rang $r + 1$.
		 2. Falls der vorherige Baum das Element mit hoechster Prioritaet
            enthielt, aktualisiere den Zeiger auf den neu entstandenen Baum.
		3. Gehe zu (3).

#### `insert`

Die Einfuegeoperation basiert nun stark auf der `merge`-Routine. Wir erinnern
uns, das ein Binomialbaum vom Rang $r = 0$ gerade $2^r = 2^0 = 1$ Knoten hat. Um
ein neues Element in einen Binomial-Heap einzufuegen, muessen wir also lediglich
einen Binomial-Heap mit einem einzigen Baum vom Rang Null erzeugen und in die
`merge`-Operation geben. Als Handles geben wir hierbei natuerlich nicht mehr
Indizes, sondern Knotenzeiger (oder explizite Handle-Objekte) zurueck.

#### `pop`

Da wir einen Zeiger auf den Baum haben, der das Element mit hoechster Prioritaet
enthaelt, finden wir diesen Baum in konstanter Zeit. Dann koennen wir die `pop`
Operation auf diesem Baum durchfuehren. Daraus erhalten wir zum Einen das
gesuchte Element mit hoechster Prioritaet und zum Anderen eben auch Baeume mit
Rang $r-1, ..., 0$ wo $r$ der vorherige Rang des Baumes war. Diese koennen wir
folglich wieder durch `merge` in die verkettete Liste einfuegen.

Leider gibt es hier nun keine sehr schnelle Moeglichkeit, das neue Element mit
hoechster Prioritaet zu finden. Wir muessen also in logarithmischer Zeit die
Wurzeln aller Baeume in der Liste inspizieren.

#### `erase`

Wie beim Array-Heap koennen wir das Loeschen von Elementen so realisieren, dass
wir dem Element maximale Prioritaet geben und es nach oben `swim`-en und dann
`pop` aufrufen. Dadurch erhalten wir wieder Teilbaeume des urspruenglichen
Binomialbaumes, welche wir in den Heap neu reinmergen koennen.

#### `changeKey`

Funktioniert wie bei Array-Heaps. Man veraendert also den Wert des Elements, das
durch einen Handle adressiert wird. Dann ruft man entweder `swim` oder `sink`
auf. Hatte das Element vorher hoechste Prioritaet im Baum und wurde dieser
reduziert, muss aus den Wurzeln der Baeume danach wieder das neue Element mit
hoechster Prioritaet selektiert werden.
