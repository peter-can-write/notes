# Sortieren

Ein typisches Problem in der Informatik ist das effiziente *Sortieren* von
Elementen. Sei hierfuer $\mathcal{S} = \langle e_1, ..., e_n \rangle$ eine
Sequenz der Laenge $l = |\mathcal{S}|$, die bezueglich einer totalen Ordnung
$\leq$ sortiert werden soll. Dann liefert ein Sortieralgorithmus eine
Permutation $\mathcal{S}'$ dieser Sequenz, sodass die urspruenglichen Elemente
$e_1, ..., e_n$ bezueglich dieser Ordnung sortiert sind.

Wir wollen nun einige gaengige Sortierverfahren untersuchen und analysieren.

## SelectionSort

Der erste Sortieralgorithmus, den wir betrachten wollen, ist *Selection
Sort*. Dieser Algorithmus entspricht der folgenden Idee.

1. Fuer jeden Index $i$ im Intervall $[0, n - 1]$:
   1. Suche das kleinste Element im restlichen Feld $[i, n - 1]$
   2. Vertausche es mit dem Element an index $i$
   3. Inkrementiere $i$

In der $i$-ten Iteration wird also das $i$-kleinste Element ausgewaehlt und an
Index $i$ gegeben. Dieser Algorithmus hat zwei Invarianten. Die erste ist, dass
die Sequenz links vom Iterator bzw. momentan betrachteten Index $i$ bereits
sortiert und in ihren finalen Positionen sind. D.h. Elemente links von $i$
werden nicht mehr angeruehrt. Die zweite Invariante ist, dass kein Element
rechts vom Iterator bezueglich der Ordnun vor irgendeinem Element links vom
Iterator ist.

In der $i$-ten Iteration enthaelt das Restfeld rechts om Iterator noch $l - i$
Elemente. Somit hat dieses Restfeld initial eine Groesse von $l$, dann $l - 1$,
dann $l - 2$ und so weiter. Die Laufzeit ergibt sich dann als Summe dieser
Werte:

$$T(n) = \sum_{i=0}^n n - i = \sum_{i=0}^n i = \frac{n^2 + n}{2} \in \Theta(n^2)$$

Die Laufzeit ist also garantiert quadratisch, daher nicht nur in $O(n^2)$
sondern auch in $\Omega(n^2)$ und somit $\Theta(n^2)$. SelectionSort laeuft
also insbesondere sogar fuer bereits sortierte Sequenzen quadratisch. Im
Durchschnitt werden fuer SelectionSort $n/2$ Elemente inspiziert und
verglichen, sodass die durschnittliche Anzahl an Vergleichen $n n/2 = 1/2 n^2$
ist.

```Python
def selection_sort(array):
	for i in range(len(array)):
		minimum = i
		for j in range(i + 1, len(array)):
			if array[j] < array[minimum]:
				minimum = j
	    array[minimum], array[i] = array[i], array[minimum]
```

## InsertionSort

Der naechste elementare Sortieralgorithmus, den wir untersuchen wollen, ist
*InsertionSort*. InsertionSort befolgt konzeptuell das Schema, nach welchem
man Karten sortiert. Betrachten wir hierzu eine Hand voll Karten, die wir
sortieren wollen, sowie eine konkrete Karte in dieser Hand. Wir nehmen an, dass,
die Karten links von dieser Karte schon sortiert, aber noch nicht in ihrer
finalen Ordnung sind. Die Karten rechts von der Karte sind uns erstmal
egal. Dann sehen wir uns fuer diese Karte solange Karten links an, bis wir die
"Luecke" finden, wo die Karte reinpasst. Dast ist gerade jener Index, wo die
Karte links kleiner oder gleich der Karte ist, und die Karte rechts von der
Luecke die erste bzw. am weitesten linke Karte ist, die strikt groesser als die
betrachtete Karte ist. Genau hier fuegen wir die Karte ein. Dann wissen wir nun
fuer die naechste Karte wieder, dass die Karten links davon sortiert aber noch
nicht fixiert sind und uns die Karten rechts erstmal egal sind. Formal ergibt
sich daraus folgender Algorithmus:

1. Fuer jeden Index $i$ im Intervall $[1, n - 1]$:
   1. Speichere das Element an Index $i$.
   2. Setze $j := i$. Solange $j$ noch groesser Null ist und das Element an $j -
      1$ strikt groesser ist als das urspruengliche Element an Index $i$:
	  1. Schiebe das Element an Index $j$ um eines nach vorne, also $s[j] =
	  s[j - 1]$.
	  2. Dekrementiere $j$.
   3. Nun ist das Element an Index $j - 1$ also nicht mehr strikt groesser,
      sondern kleiner oder gleich dem gespeicherten Element, das urspruenglich
      an Index $i$ war. Auch haben wir alle Elemente, die vor $i$ waren und
      groesser als das gespeicherte Element, um einen Index nach vorne
      geschoben. Insbesondere haben wir auch das Element am momentanen Index $j$
      um Eines nach vorne geschoben, sodass es ein Duplikat vom Element an Index
      $j + 1$ ist. Nun ist die Stelle $j$ gerade der Index, wo wir das
      gespeicherte Element einfuegen.

Der schlechteste Fall fuer InsertionSort ist der, dass die Sequenz bezueglich
der Ordnung vollkommen invertiert ist. Dann muss naemlich das erste Element noch
gar nicht, das zweite Element dann um einen Index, das dritte um zwei und das
letzte Element schliesslich um ganze $n - 1$ Indizes verschoben werden. Somit
ergibt sich ueber Gauss wieder eineq quadratische Hoechstlaufzeit. Betrachten
wir aber nun die einfachste Eingabe fuer InsertionSort, wenn die Sequenz schon
sortiert ist. Dann muss ueberhaupt keine einzige Vertauschung gemacht werden,
waehrend die Sequenz durchiteriert wird. Die Laufzeit waere also linear in der
Laenge der Eingabe. Auch wenn nur wenige Vertauschungen gemacht werden muessen,
also wenn die Sequenz partiell sortiert ist, bleibt die Laufzeit linear.

Im schlimmsten Fall macht InsertionSort $1/2 n^2$ Vergleiche ($n$ mal zwischen
keinem und $n - 1$ Vergleichen, also $n$ mal durchschnittlich $(n-1)/2$
Vergleiche), wie SelectionSort. In diesem Fall waere InsertionSort aber dennoch
langsamer, weil es viel mehr Vertauschungen macht. SelectionSort vertauscht
genau $n$ mal, wohingegen InsertionSort ebenso oft vertauscht, wie es
vergleicht, also durchschnittlich $n^2/2$ Mal.

```Python
def insertion_sort(array):
	for i in range(1, len(array)):
		temp = array[i]
		j = i
		while j > 0 and array[j - 1] > temp:
			array[j] = array[j - 1]
			j -= 1
		array[j] = temp
```

## ShellSort

ShellSort ist eine interessante und effiziente Steigerung von InsertionSort,
welche eine linearithmische Hoechstlaufzeit von $O(n \log n)$ erreichen
kann. Die grundlegende Beobachtung, die Shellsort ausnutzt ist, dass wir bei
InsertionSort oftmals viele Vergleiche und Vertauschungen machen muessen. Ist
die Sequenz beispielsweise bezueglich der Ordnung invertiert ("absteigend"
sortiert), so muss da letzte Element $n - 1$ Positionen wandern. Wir fragen uns:
koennte es denn nicht mehr Positionen auf einmal wandern? Genau diese Frage
beantwortet Shellsort. Shellsort sortiert in mehreren Schritten mit variabler
Granularitaet, wobei diese Granularitaet durch einen $h$-Wert bestimmt
wird. Dieser $h$-Wert ist hierbei die Schrittbreite (engl. *stride*), um welchen
Shellsort Elemente in einer Iteration jeweils nach vorne transportiert. Fuer
InsertionSort ist die Schrittbreite immer Eins, bei ShellSort nun aber Anfangs
meist groesser als Eins. Sei $h$ beispielsweise $n/3$. Dann wuerde das oben
angesprochene Element nicht mehr $n - 1$, sondern nur mehr drei mal vertauscht
und verglichen werden, um an den Anfang der Sequenz transportiert zu werden!
Nachdem mit einem bestimmten $h$-Wert das Array wie bei InsertionSort, aber mit
geringerer Granularitaet und hoeherer Schrittbreite und Effizienz, ein ganzes
mal durchiteriert wurde, wird $h$ dann verringert. Wurde ein Element also in der
ersten Iteration in groeberen Schritten in die Naehe seiner letzendlichen
Position gebracht, so wuerde es in dieser Iteration in feineren Schritten
dorthin transportiert werden. Fuer kleinere $h$-Werte werden die Elemente also
naeher an ihren finalen Index gebracht, wobei sie aber wegen der vorherigen
groeberen Granularitaet der Schrittbreiten mit dieser feineren Granularitaet
*auch nicht mehr so weit* wandern muessen. Bis wir bei $h = 1$ sind, haben wir
dann meist nur mehr linearen Aufwand fuer eine Iteration, sprich eine konstante
Anzahl an Vertauschungen pro Element.

Der exakte Wert von $h$ ist ein noch offenes Forschungsgebiet. Eine haeufig
verwendete Sequenz fuer $h$ ist die $3x + 1$ Sequenz. Hierfuer setzt man $h$
initial auf $1$ und setzt es dann sukzessive immer um $3 \cdot h + 1$, bis $h$
groesser oder gleich $n/3$ ist. Nach einer Iteration muss man $h$ lediglich
durch drei dividieren und abrunden, um den naechsten, feineren Wert von $h$ zu
erhalten: $h' = \lfloor h/3 \rfloor$. Das ist in der Praxis insbesondere toll,
da dieses Abrunden automatisch durch Integer-Division funktioniert.

Somit ergibt sich der folgende Algorithmus fuer Shellsort mit der $3x + 1$
Sequenz:

1. Setze $h = 1$. Solange wie $h < n/3$ gilt:
   1. Setze $h = 3 \cdot h + 1$
2. Solange wie nun $h \geq 1$ ist:
   1. Fuehre InsertionSort mit dieser Schrittbreite aus. Das bedeutet:
   2. Setze $i$ initial auf $h$ und dann:
	  1. Speichere den Wert an Index $i$.
	  2. Setze $j := i$.
	  3. Solange wie $j \geq h$ und der Wert an Index $j - h$ strikt groesser
         als der gespeicherte Wert ist:
		 1. Schiebe den Wert an Index $j - h$ an den Index $j$.
		 2. Dekrementiere $j$ um $h$.
	  4. Setze nun den Wert an Index $j$ zum gespeicherten Wert.
   3. Integer-dividiere $h$ um drei: $h' = h//3$

Mit der $3x + 1$ Sequenz kann man zeigen (Knuth), dass die Laufzeit in
$O(n^{3/2})$ liegt, wobei $n^{3/2} \in o(n^2)$. Im schlimmsten Fall laeuft
Shellsort also noch immer besser als InsertionSort. Im besten Fall ist Shellsort
ein wenig schlechter als InsertionSort. InsertionSort lief im besten Fall in
linearer Zeit, also $O(n)$, weil wir nur einmal durch die Sequenz iterieren
mussten, ohne jeweils fuer ein Element irgendwelche Arbeit zu leisten. Bei
Shellsort waere dann jede Iteration, also jede Ausfuehrung von InsertionSort,
dann wohl auch linear. Aber weil wir mehr Iterationen haben, und diese von $n$
abhaengt, ergibt sich etwas anderes. Wir merken, dass wir bei der $3x + 1$
Sequenz ungefaehr $\log_3 n$ mal unseren $h$-Wert dritteln koennen, bis wir bei
einem Inkrement ($h$-Wert) von $1$ sind. Somit ergibt sich also eine Laufzeit
von $O((\log_3 n) \cdot n) = O(n \log_3 n)$.

```Python
def shell_sort(sequence):
	h = 1
	while h < len(sequence)/3:
		h = (3 * h) + 1
	while h >= 1:
		for i in range(h, len(sequence), h):
			temp = sequence[h]
			j = i
			while j >= h and sequence[j - h] > temp:
				sequence[j] = sequence[j - h]
				j -= h
			sequence[j] = temp
		h //= 3
	return sequence
```

ShellSort ist der effizienteste Algorithmus, wenn es um einen kleinen
Code-Footprint geht. Das ist beispielsweise in eingebetteten Systemen sehr
wichtig. Es benutzt naemlich auch keine Rekursion, was unter
ressourcen-eingeschraenkten Umgebungen hohen Aufwand haben kann.

## MergeSort

MergeSort ist ein Sortieralgorithmus, der auf dem Konzept von
*Divide-And-Conquer* basiert. Hierbei teilt man das Problem, also die
unsortierte Sequenz, rekursiv in kleinere Teilprobleme, also kleinere
Teilsequenzen auf. Das ist also der *Divide* Teil. Sind die Teilprobleme erstmal
klein genug, um sie leicht loesen zu koenenn, tun wir diese. Beim Sortieren ist
der einfachste Fall der, dass die Sequenz nur ein einziges Element
enthaelt. Diese kleinen Teilprobleme *Conquer*-en wir also. Nach dem Divide und
dem Conquer folgt letztlich noch ein *Combine* Schritt. Haben wir ein
(Teil-)problem erstmal in kleinere Teilprobleme aufgeteilt, und diese geloest,
kombinieren wir die Loesungen zu diesen Teilproblemen um das urspruengliche
Problem zu loesen. Fuer MergeSort heisst das nun folgendes:

1. Hat die Sequenz weniger als zwei, also nur ein oder gar kein Element, so sind
   wir schon fertig. Sonst:
2. Teile die Sequenz in zwei Haelften.
3. Rekursiere fuer die linke Haelfte.
4. Rekursiere fuer die rechte Haelfte.
5. Wir nehmen nun (induktiv) an, dass die linke und rechte Haelfte sortiert
   sind.
6. Kopiere nun diese Haelften in ein temporaeres Array. Dann sei:
   * $i$ ein Index in die urspruengliche Sequenz.
   * $j$ ein Index in die Kopie und initial Null, also am Anfang der linken Haelfte.
   * $k$ ein Index in die Kpie und initial in der Mitte, also Anfang der rechten
     Haelfte.
   1. Dann iteriere mit $i$ ueber die urspruengliche Sequenz. Es gilt nun, die
      Elemente aus der Kopie in der richtigen Reihenfolge in die urspruengliche
      Sequenz zu kopieren. Hierzu waehlen wir fuer jedes $i$ das Minimum des
      Elements an Index $j$ und Index $k$ der Kopie, also jeweils das kleinere
      momentane Element der beiden Haelften. Genauer:
	  1. Ist $k$ am Ende der rechten Haelfte oder, wenn nicht, $j$ noch nicht am
         Ende der linken Haelfte (in der Mitte der Kopie), und gleichzeitig der
         Wert an Index $j$ kleiner oder gleich dem Wert an Index $k$ in der
         Kopie, so:
		 1. Kopiere das Element von Index $j$ in der Kopie in den Index $i$.
		 2. Inkrementiere $j$.
	  2. Ansonsten ist $k$ also noch nicht am Ende der rechten Haelfte und
         entweder $j$ am Ende der linken Haelfte, oder das Element an Index $j$
         einfach groesser als der Wert bei $k$. Dann alsoL
		 1. Kopiere das Element an Index $k$ in der Kopie in den Index $i$.
		 2. Inkrementiere $k$.

Das ergibt nun folgende Implementierung von MergeSort:

```Python
def _merge_sort(sequence, first, last):
    if (last - first) < 2:
        return

    middle = first + (last - first)/2

    _merge_sort(sequence, first, middle)
    _merge_sort(sequence, middle, last)

    copy = [sequence[i] for i in range(first, last)]
	j = 0
	stop = len(copy)/2
	k = stop
	for i in range(first, last):
		if k == len(copy) or (j < stop and copy[j] <= copy[k]):
			sequence[i] = copy[j]
			j += 1
		else:
			sequence[i] = copy[k]
			k += 1

    assert is_sorted(sequence, first, last)


def merge_sort(sequence):
    _merge_sort(sequence, 0, len(sequence))
    return sequence
```

Wie steht es nun aber um die Laufzeit von MergeSort?

### Master Theorem

Das Master Theorem ist eine Methode, um die Laufzeit von rekursiven
Divide-And-Conquer Algorithmen zu bestimmen. Sie liefert uns ein einfaches
Schema um die Laufzeit eines solchen Algorithmus in eine von drei
Laufzeitklassen zu geben.

Wir betrachten hierzu ein Problem der Groesse $n = b^k$. Hierbei ist die Basis
$b$ die Groesse der Teilprobleme, in welche wir das urspruengliche Problem im
Laufe des Divide-And-Conquer Verfahren aufteilen. $k$ ist dann also der
Logarithmus von $n$ zur Basis $b$ und somit insbesondere die maximale Anzahl an
Rekursionsstufen. Denn wenn wir $n$ sukzessive immer durch $b$ Teilen, so kommen
wir nach genau $k = \log_b n$ Schritten bei Eins an.

Falls nun $k = 0 \iff n = 1$ gilt, so investieren wir immer einen konstanten
Aufwand $a$ um dieses kleinste Problem zu loesen. Falls $k \geq 1$, koennen wir
unser Problem noch weiter zerlegen. Wie beschrieben zerlegen wir unser Problem
der Groesse $n$ also in Teilprobleme der Groesse $n/b$. Bei MergeSort ist $b$
hier (meist) zwei, da wir unsere Sequenz immer halbieren. Sei nun $d$ die Anzahl
an Probleme, die wir nach der Aufteilung erhalten. Bei MergeSort ist $d = 2$ da
wir genau zwei Probleme der Groesse $n/b = n/2$ haben. Hier erhalten wir also
ebenso viele Teilprobleme, wie die Anzahl an Teilsequenzen, in welche wir die
urspruengliche Sequenz aufgeteilt haben. Es gilt hier folglich $d = b$. Nun ist
es aber nicht immer so, dass $d = b$ ist. Bei manchen Algorithmen teilen wir
unser Problem zwar in neue Probleme der Groesse $n/b$, muessen dann aber mehr
oder weniger Arbeit investieren. Zum Beispiel kann es sein, dass wir unser
Problem halbieren, also Sequenzen der Groesse $n/2$ erhalten, dann aber nur die
eine Haelfte bearbeiten und fuer sie weiterrekursieren muessen (z.B. weil links
das Maximum ist). Auch kann es aber sein, dass wir vielleicht zunaechst beide
Probleme bearbeiten muessen und sie dann aber in einem neuen Teilproblem wieder
kombinieren oder bearbeiten muessen. Jedenfalls gilt nicht notwendigerweise,
dass $d = b$.

Es ist aber nun geradie Beziehung zwischen $d$ und $b$, die Laufzeitklasse
ausmacht. Betrachten wir hierzu die drei moeglichen Relationen, in welcher $d$
und $b$ stehen koennen:

* $d < b$: Hierbei teilen wir unser Problem also zuerst in $b$ Teilprobleme der
  Greose $n/b$, muessen dann aber nur in eine echte Teilmenge dieser Probleme
  Aufwand investieren. Insbesondere bedeutet das, dass der Aufwand mit
  wachsender Rekursionstiefe sogar sinkt!
* $d = b$: Hierbei haben wir also genauso viel Arbeit, wie wir Teilprobleme
  haben. Nehmen wir nun an, dass wir fuer jedes Teilproblem linearen Aufwand
  haben, haben wir also insgesamt $c \cdot b \cdot n/b = cn$ viel Aufwand. Wir
  merken, dass der Aufwand pro Teilstufe also gleich bleibt, weil wir ja,
  zusammen gesehen, immer gleich viele Elemente haben.
* $d > b$: Der Aufwand steigt mit wachsender Rekursionstiefe. Wir teilen unser
  Problem sets in $b$ weitere, kleinere Probleme, muessen dann aber irgendwie
  manche Probleme mehrmals bearbeiten oder kombinieren, sodasss wir immer mehr
  Arbeit erhalten, da wir fuer jedes zusaetzliche Teilproblem ja wieder
  rekursieren muessen, bis wir die Teilprobleme nur mehr ein Element enthalten.

Untersuchen wir nun weiter die konkrete, $i$-te Rekursionsstufe. Wir haben
unsere $n$ Elemente hierbei schon $i$ mal durch $b$ geteilt und jeweils $d$ mehr
Probleme bekommen. Somit sind in dieser Stufe die Teilprobleme nur mehr $n/b^i$
Elemente gross. Auch haben wir nun $d^i$ viele, da wir pro Stufe fuer jedes der
$d$ Probleme wieder $d$ Probleme bekommen haben. Wenn wir annehmen, dass wir
linearen Aufwand $c$ pro Teilproblem haben (das Mergen in MergeSort), ergibt
sich auf Stufe $i$ somit ein Aufwand von:

$$d^i \cdot c \frac{n}{b^i} = cn\left(\frac{d}{b}\right)^i$$

Wir haben hierbei also in Abhaengigkeit von $i$ mit Konstanten $b,c,d$ eine
geometrisch Reihe. Oben haben wir mit $a$ den konstanten Aufwand fuer jedes
minimale Teilproblem der Groesse Eins in der letzten (untersten)
Rekursiionsstufe beschrieben. Auf dieser letzten Stufe haben wir also $d^k$
solche Teilprobleme. Wir koennen nun mittels obiger Klassifizierung exakt die
Laufzeiten dieser verschiedenen Arten von Algorithmen beschreiben:

* $d = b$: Wie oben beschrieben haben wir hier auf jeder Stufe gleich viel
  Aufwand. Genauer ist der Aufwand in der letzten, $k$-ten Stufe genau $ad^k =
  ab^k = an \in \Theta(n)$. Auch haben wir gesagt, dass der Aufwand in jeder
  weiteren Stufe immer $cn$ ist, was auch in $\Theta(n)$ ist. Wir haben also
  immer linearen Aufwand in jeder Stufe. Wieviele Stufen haben wir nun? Naja,
  $k$ viele, und $k$ war gerade der Logarithmus von $n$ zur Basis $b$. Somit
  ergibt sich fuer die Laufzeit, wenn $b = d$ (sehr oft der Fall) gerade:
  $$\Theta(n \log_b n)$$

* $d < b$: Auf der letzten Stufe haben wir eben $ad^k$ Aufwand. Nach
  Voraussetzung ist $ad^k$ hier kleiner als $ab^k$, wo $b^k = n$ war. Somit muss
  also $ad^k$ in dieser letzten Stufe sicher im schlimmsten Fall linear, im
  besten Fall aber vielleicht sogar besser Laufen. Jedenfalls haben wir hier
  schonmal $O(n)$. Fuer die uebrigen Stufen ergibt sich ueber die geometrische
  Reihe:
  $$cn \sum_{i=0}^{k-1} (\frac{d}{b})^i = cn \frac{1 - (d/b)^k}{1 - d/b}$$
  Da wir oben von $1$ abziehen, muss dieser Wert sicher durch $$cn \frac{1}{1 -
  d/b}$$ nach oben beschraenkt sein. Da $d$ und $b$ konstant sind, ist die ganze
  Laufzeit somit insgesamt in $\Theta(n)$, was die letztendliche Laufzeitklasse
  bestimmt.

* $d > b$: Hierzu muessen wir zunaechst eine Beobachtung machen. Wir wissen,
  dass $n = b^k$ gilt. Dann ist $k = \log_b n$. Ueber die Logarithmusregel
  $$\log_d n = \frac{\log_b n}{\log_b d} \iff \log_b n = \log_d n \cdot \log_b
  d$$ folgt nun, dass $k = \log_b n = \log_b d \cdot \log_d n$ gilt. Dann haben
  wir im Weiteren also $d^k = d^{\log_d n \log_b d} = (d^{\log_d n})^{\log_b d}
  = n^{\log_b d}$. Dadurch ergibt sich fuer die unterste, $k$-te Stufe ein
  Aufwand von $ad^k = an^{\log_b d} \in \Theta(n^{\log_b d})$. Fuer die uebrigen
  Stufen ergibt sich ueber die geometrisch Reihe ebengleich wie oben, dass die
  Laufzeit ebens in $\Theta(n^{\log_b d})$.

Sei somit zusammenfassend ein Divide-And-Conquer Algorithmus gegeben durch eine
Rekursionsgleichung $R(n)$:

$$
R(n) =
\begin{cases}
	a, \text{ falls } n = 1,\\
	cn + d \cdot R(n/b) \text{ falls } n > 1
\end{cases}
$$

wo $a$, der Aufwand auf der untersten Stufe, $b$ der Teilungsfaktor, $d$ die
Anzahl an Teilproblemen pro Stufe und $c$ der lineare Aufwand pro Stufe ueber
der letzten jeweils positive Konstanten sind. Dann liegt die Laufzeit $T(n)$
dieses Algorithmus in:

$$
T(n) =
\begin{cases}
	\Theta(n) \text{ falls } d < b,\\
	\Theta(n \log_b n) \text{ falls } d = b,\\
	\Theta(n^{\log_b d}) \text{ fallst d > b }
\end{cases}
$$

Da alle Logarithmen gleich schnell wachsen, gilt fuer $d = b$ auch $\Theta(n
\log_2 n)$ unabhaengig vom eigentlichen Teilungsfaktor $b$.


### Verbesserungen von MergeSort

In der Praxis koennen wir einige Verbesserungen an MergeSort.

#### Selektive Sortierung

Zum Einen ist es eine gute Idee, fuer kleine Sequenzen InsertionSort zu
benutzen. InsertionSort ist der schnellste elementare Algorithmus und hat fuer
kleine Sequenzen viel weniger Overhead als die rekursive Implementierung von
MergeSort. Deswegen lohnt es sich, fuer maximal 7-10 Elemente InsertionSort zu
nutzen.

#### Nur wenn noetig Sortieren

Eine weitere Optimierung, die wir vornehmen koennen, ist es nur zu sortieren,
wenn es unbedingt notwendig ist. Betrachten wir naemlich den Fall, dass die
sortierten Teilsequenzen, die wir aus den beiden rekursiven `merge_sort`
Aufrufen erhalten, zufaelligerweise zusammen schon die gesamte sotierte Sequenz
ergeben. Sei beispielsweise `[C A B F D E]` die relevante Sequenz. Dann wuerden
wir diese Sequenz also nun in der Mitte teilen und jeweils rekursieren. Als
Ergebnis erhalten wir dann die beiden sortierten Teile `[A B C] [D E F]`. Im
Weiteren wuerde der naive MergeSort nun diese beiden Teile in ein temporaeres
Array geben und den Merge-Vorgang starten. Das Mergen kostet dabei $O(N)$ Platz
und Zeit. Wir merken aber: das ganze waere doch gar nicht noetig. Die Teile
sind, zusammen gesehen, doch schon sortiert. Und da die beiden Teile ja aus der
selben Sequenz stammen, ist die ganze Sequenz also gerade schon soriert.

Die Optimierung hier ist es also, nur dann zu mergen, wenn das groesste Element
in der linken Haelfte *strikt groesser* ist als das kleinst Element in der
rechten Haelfte. Dann, und nur dann, ist die Sequenz zusammen gesehen naemlich
noch nicht sortiert. Wir verandern also:

```Python
mergeSort(sequence, first, middle)
mergeSort(sequence, middle, last)
# ...
copy = [...]
# merge
# ...
```

zu

```Python
mergeSort(sequence, first, middle)
mergeSort(sequence, middle, last)
# ...
if sequence[middle - 1] > sequence[middle]:
	copy = [...]
    # merge
# ...
```

#### Sortieren ohne Kopieren

Auch ist es moeglich, Overhead beim Kopieren zu sparen. Durch diese Methode wir
Laufzeit nicht gespart aber die "praktische Konstante" wird kleiner. Wir muessen
aber in Kauf nehmen, dass unser Kopf etwas gefickt wird. Konkret ist die Idee
wie folgt: Bevor wir die MergeSort Routine zum Ersten mal aufrufen, erstellen
wir bereits eine Kopie der Sequenz. Die Funktion erhaelt dann die folgende
Signatur:

`mergeSort(sequence, copy)`

Wir gehen dann wie folgt vor:

1. Wir rufen die Methode rekursiv auf und vertauschen die Sequenz mit der
   Kopie.
2. Danach mergen wir aus der Kopie in die Sequenz.

Was sich daraus ergibt ist folgende Situation:

1. In der ersten Iteration rufen wir `mergeSort(copy, sequence)` rekursiv
   auf. Dadurch werden also die beiden Haelften der *Kopie* sortiert.
2. Innerhalb der zweiten Iteration (in der ersten Rekursion) sortieren wir also
   die Kopie und nutzen die urspruengliche Sequenz als Auxiliary Array. Sagen
   wir, dass wir nun nicht weiter rekursieren muessen. Jedenfalls wird dann in
   dieser Rekursionsstufe der Methodenaufruf `mergeSort(sequence, copy)`
   gemacht, weil wir ja die Kopie weiterleiten, wobei die Kopie gerade die
   Sequenz ist. Wir nehmen also an, die Sequenz ist nach den beiden Aufrufen
   sortiert.
3. Wir mergen nun die beiden sortierten Haelften der Sequenz in die Kopie.
4. Bottom-Out und wir sind wieder in der ersten Stufe. Hier hatten wir die Kopie
   als Sequenz weitergeleitet und wissen nun also, dass die beiden Haelften der
   Kopie sortiert sind. Dann mergen wir die Kopie nun wieder in die Sequenz.
5. Fertig.

Wir merken, dass wir nie *explizit* kopieren mussten. Der Merge-Schritt bleibt
uns nicht erspart, aber das Kopieren. Somit ist also dieser eine lineare Faktor
weg (fuer das Kopieren), aber der andere (fuer das Mergen) bleibt. Die
Platzeffizienz ist auch nicht besser, weil sie noch immer linear ist. Aber: wir
muessen nicht mehr in jeder Rekursionsstufe neu eine Kopie allokieren und dann
wieder zerstoeren.

Das fuehrt nun zu folgender Implementierung:

```Python
def merge(source, destination, first, last):
    j = first
    middle = first + (last - first)//2
    k = middle
    for i in range(first, last):
        if k == last or (j < middle and source[j] <= source[k]):
            destination[i] = source[j]
            j += 1
        else:
            destination[i] = source[k]
            k += 1


def _merge_sort2(sequence, auxiliary, first, last):
    if (last - first) < 2:
        return
    middle = first + (last - first)//2
    _merge_sort2(auxiliary, sequence, first, middle)
    _merge_sort2(auxiliary, sequence, middle, last)
    if auxiliary[middle - 1] > auxiliary[middle]:
        merge(auxiliary, sequence, first, last)
    assert is_sorted(sequence, first, last)


def merge_sort2(sequence):
    auxiliary = sequence.copy()
    _merge_sort2(sequence, auxiliary, 0, len(sequence))

    return sequence
```

### Bottom-Up MergeSort

In seiner Standardimplementierung kann MergeSort als ein *top-down* Algorithmus
bezeichnet werden. Wir gehen naemlich von groesseren Teilproblemen rekursiv zu
kleineren ueber. Es gibt aber auch eine Implementierung von MergeSort, die
andersrum, also *bottom-up* geht. Sie faengt also mit kleinen Teilproblemen an
und kombiniert diese immer und immer wieder zu greosseren Teilproblemen. Es ist
also quasi top-down Mergesort, wobei wir die rekursiven Aufrufe ueberspringen
und auch immer ganze Ebenen ("level-order") abarbeiten, anstatt zuerst die linke
und dann die rechte Haelfte zu bearbeiten. Wir implementieren den Algorithmus
hierbei auf Basis des schnelleren kopie-losen Algorithmus von oben. Dadurch
ergibt sich:

1. Kopiere die Sequenz zunaechst wieder in eine Kopiesequenz.
2. Initialisiere die Bucketgroesse $b$, also die Groesse der Teilsequenzen, mit 2.
3. Solange wie die Bucketgroesse noch kleiner als die Laenge der Sequenz ist:
   1. Setze $i := 0$ und dann, solange wie $i < n$:
	  1. Setze den Mittelpunkt des momentanen "sliding" Bucket auf $m := i + b/2$.
	  2. Setze den Endpunkt des Buckets auf $j := m + b/2 = i + b$.
	  3. Falls dann der Mittlepunkt $m$ noch kleiner als $n$ ist, so haben wir
         also zumindest das Element $m$ in der rechten Teilsequenz. Dann checken
         wir zunaechst wieder, ob das Element an Index $m - 1$ groesser ist als
         das an Index $m$. Falls ja, mergen wir aus der Kopie in die Sequenz in
         den Indizes von $i$ bis $j$.
	  4. Falls nein, also falls die Sequenz entweder schon sortiert ist, oder
         der Mittelpunkt $\geq n$ ist, kopieren wir die Werte aus der Kopie
         lediglich in die Sequenz.
   2. Vertausche Sequenz und Kopie.
   3. Verdoppele die Bucketgroesse.
4. Falls die Sequenz eine ungerade Anzahl an Elementen enthielt, ist immer ein
   Element ganz rechts uebrig geblieben. Mit einer Iteration von InsertionSort
   muessen wir dieses eine Element nun noch einordnen.

Und in einer Programmiersprache dann:

```Python
def insertion_sort_on_last(sequence):
    last_value = sequence[-1]
    for i in range(len(sequence) - 1, 1, -1):
        if sequence[i - 1] <= last_value:
            break
        sequence[i] = sequence[i - 1]
    sequence[i] = last_value


def bottom_up_merge_sort(sequence):
    auxiliary = sequence[:]
    bucket = 2
    while bucket < len(sequence):
        first = last = 0
        while first < len(sequence):
            first = last
            middle = first + bucket//2
            last = first + bucket
            if middle < len(sequence):
                if auxiliary[middle - 1] > auxiliary[middle]:
                    merge(auxiliary, sequence, first, last)
                    continue
            sequence[first:last] = auxiliary[first:last]

        sequence, auxiliary = auxiliary, sequence
        bucket *= 2

    # We swapped at the end
    sequence = auxiliary

    # If the length is odd, we will have on
    # son of a bitch left on the far right
    if len(sequence) % 2 == 1:
        insertion_sort_on_last(sequence)

    return sequence
```

## QuickSort

QuickSort ist ein weiterer Divide-And-Conquer Sortieralgorithmus, der in Praxis
meist oefter gewaehlt wird als MergeSort. QuickSort basiert auf der Idee der
*Partitionierung*. Konkret gehen wir so vor: wir waehlen ein *Pivot* Element aus
der Sequenz, bezueglich welchem wir die Sequenz partitonieren
wollen. Partitionierung heisst hierbei, dass wir wollen, dass alle Elemente
links vom Pivot Element kleiner oder gleich dem Pivot sind und rechts nur
Elemente stehen, die groesser oder gleich sind. Nach dieser Partitionierung ist
das Pivot Element selbst schon in seiner finalen Position, da lediglich die
Werte, die kleiner, also links, sind, noch sortiert werden muessen aber nicht
mehr am Pivot vorbeiwandern koennen, weil es ja groesser ist und somit in der
sortierten Reihenfolge nach diesen linken Elementen folgen muss. Symmetrisches
gilt dann auch fuer die rechte Haelfte. Wir wuerden also nach der Partionierung
entsprechend dem Divide-And-Conquer Verfahren wieder fuer die linke und rechte
Haelfte rekursieren. Schritt fuer Schritt kaemen wir so zur Situation, dass wir
nur mehr ein oder zwei Elemente als Teilproblem haben. Diese wuerden dann durch
die Partionierung vollkommen sortiert. Induktiv wuerden so gerade die kleinsten
Teilprobleme geloest werden, die so gewaehlt wurden, dass sie zusammen dann das
urspuengliche Problem loesen. Das fuehrt nun zu folgende Beschreibung:

1. Hat die Sequenz weniger als zwei Elemente, sind wir fertig.
2. Sonst, waehle ein Pivot Element. Unter der Annahme uniform verteilter Werte
   ist das allererste Element eine ausreichende Wahl (vorerst). Wollen wir ein
   anderes Element als Pivot waehlen, vertauschen wir es mit dem ersten und
   fahren gleich fort.
3. Setze nun einen Iterator $i$ auf den *zweiten* Index (der erste nach dem
   Pivot) und $j$ auf den letzten validen index ($n - 1$). Dann, solange wie $i
   < j$ (auch: $i \neq j$) gilt:
   1. Gehe mit dem linken Iterator solange nach rechts, bis wir ein Element
      finden, das nicht kleiner als das Pivot ist. Hoere auch auf, wenn
      inzwischen schon $i \geq j$ gilt.
   2. Gehe mit dem rechten Iterator solange nach links, bis wir ein Element
      finden, dass kleiner oder gleich, also nicht groesser als das Pivot
      ist. Hoere ebenso auf, wenn $i \geq j$.
   3. Falls dann $i = j$ gilt, breche ab.
   4. Sonst vertausche die Elemente an Indizes $i,j$.
4. Als naechstes gilt es, das Pivot Element an die richtige Stelle, also
   zwischen kleineren und groesseren Elementen zu platzieren. Grundsaetzlich
   orientieren wir uns dabei an der Position des linken Iterators $i$. Es kann
   nun aber passiert sein, dass $i$ auf die Position von $j$ gegangen ist und
   das Element an diesem Index groesser als das Pivot ist. Dann wuerden wir beim
   Vertauschen ein Element in die erste Position, also links vom Pivot,
   befoerdern das gar nicht kleiner oder gleich dem Pivot ist. Das wollen wir
   also nicht. Ist das der Fall, dekrementieren wir $i$ also vorher noch um
   eines nach links. Dann duerfen wir vertauschen.
5. Rekursiere fuer die Teilsequenz links vom Pivot (exklusive dem Pivot!).
6. Rekursiere fuer die Teilsequenz rechts vom Pivot (exklusive dem Pivot!).

Es ist hier wichtig, das Pivot in keiner der Teilsequenzen zu inkludieren, fuer
welche wir rekursieren, da das Pivot nach der Partitionierung schon an der
richtigen Stelle ist. In Code ergibt das folgende Implementierung:

```Python
def _quick_sort(sequence, first, last):
	if (last - first) < 2:
		return
	pivot = sequence[first]
	i = first + 1
	j = last - 1
	while i != j:
		while i != j and sequence[i] < pivot:
			i += 1
		while i != j and sequence[j] > pivot:
			j -= 1
		if i == j:
			break
		sequence[i], sequence[j] = sequence[j], sequence[i]
		i += 1
	if sequence[i] > pivot:
		i -= 1
	sequence[i], sequence[first] = pivot, sequence[i]

	_quick_sort(sequence, first, i)
	_quick_sort(sequence, i + 1, last)

def quick_sort(sequence):
	_quick_sort(sequence, 0, len(sequence))
	return sequence
```

Eine alternative Implementierung in Python ist etwas schoener, aber so noch
nicht vollstaendig korrekt:

```Python
def _quick_sort2(sequence, first, last):
    if (last - first) < 2:
        return
    pivot = sequence[first]
    i = first + 1
    j = last - 1
    while True:
        while i < last and sequence[i] < pivot:
            i += 1
        while sequence[j] >= pivot:
            j -= 1
        if i == j:
            break
        swap(sequence, i, j)
    swap(sequence, j, first)

    _quick_sort(sequence, first, j)
    _quick_sort(sequence, j + 1, last)

    assert is_sorted(sequence, first, last)
```

wohingegen diese Implementierung in C++ denselben Gedanken verfolgt und korrekt
ist:

```C++
template <typename RandomAccessIterator>
void quicksort(RandomAccessIterator begin, RandomAccessIterator end) {
	if (std::distance(begin, end) < 2) return;
	auto i = std::next(begin);
	auto j = std::prev(end);
	do {
		while (i != end && *i < *begin) ++i;
		while (*j > *begin) --j;
		if (i < j) std::swap(*i, *j);
	} while (i < j);
	std::swap(*begin, *j);
	quicksort(begin, j);
	quicksort(++j, end);
}
```

aber wegen den notwendigen $i < j$ Vergleiche nur mit RandomAccessIteratoren,
also insbesondere nicht auf verketteten Listen, arbeiten kann. Die vorherige
Implementierung bleibt also optimal:

```C++
template <typename Iterator>
void quicksort(Iterator begin, Iterator end) {
	if (std::distance(begin, end) < 2) return;
	auto i = std::next(begin);
	for (auto j = std::prev(end); true; ++i) {
		while (i != j && *i < *begin) ++i;
		while (i != j && *j > *begin) --j;
		if (i == j) break;
		std::swap(*i, *j);
	}
	if (*i > *begin) --i;
	std::swap(*begin, *i);
	quicksort(begin, i);
	quicksort(++i, end);
}
```

(Man staune, dass einer der schnellsten Sortieralgorithmen Welt mit etwas mehr als
zehn Zeilen Code gelingt).

### Duplikate

Ein wichtiges Diskussionsthema bei QuickSort sind *Duplikate*. Sequenzen mit
duplizierten Elementen sind in der Realtiaet relativ haeufig
anzutreffen. Insbesondere ist es eigentlich oft sogar das Ziel der Sortierung,
gleiche Elemente zu gruppieren. Beispielsweise koennten wir die Bevoelkerung
nach dem Alter sortieren, oder die Tage nach den Sonnenstunden. Wir muessen uns
also jedenfalls damit beschaeftigen, wie unsere Sortieralgorithmen mit
Duplikaten umgehen.

Da MergeSort garantiert linearithmische Laufzeit hat, wissen wir, dass MergeSort
auch Sequenzen mit wenigen oder vielen Duplikaten korrekt und linearithmisch
sortieren wird. Bei QuickSort haengt es von einem enorm wichtigen kleinen Detail
ab: ob wir beim partionieren auch stoppen und swappen, wenn Elemente gleich
sind. Also ob wir

```
while i != j and sequence[i] < pivot:
	i += 1
```

oder

```
while i != j and sequence[i] <= pivot:
	i += 1
```

Nehmen wir an, wir waehlen die zweite Methode. Sei dann die Sequenz so gewaehlt,
dass sie nur Duplikate eines einzigen Elements enthaelt, beispielsweise
`[7, 7, 7, 7, 7]`. Dann waere also $7$ das Pivot und wir wuerden mit Iterator
$i$ solange iterieren, bis wir ein Element finden, das nicht kleiner oder
gleich, sondern groesser als $7$ ist. Da es in dieser Sequenz kein solches
Element gibt, wuerden wir also gerade bis zum Ende der Sequenz gelangen. Dann
wuerden wir das Pivot Element, also die erste $7$, ans Ende der Sequenz geben
und fuer die $n - 1$ verbleibenden Elemente rekursieren. Tun wir das $n$ Mal,
erhalten wir einen Fall, wo QuickSort qudratische Laufzeit hat.

Waehlen wir hingegen die erste Variante, so wuerden wir jeweils links mit $i$
und auch rechts mit $j$ fuer jedes Duplikat stehen bleiben und
vertauschen. Hierbei ist es wiederum essentiell, dass wir in jeder Iteration
zumindest einen der beiden Iteratoren inkrementieren
bzw. dekrementieren. Schreiben wir naemlich:

```Python
while True:
	while i != j and sequence[i] < pivot:
		i += 1
	while i != j and sequence[j] > pivot:
		j -= 1
	if i == j:
		break
	swap(sequence, i, j)
```

dann bemerken wir: sind alle Elemente gleich, so gilt `sequence[i] < pivot` und
`sequence[j] > pivot` nie. Sind $i$ und $j$ initial auch noch verschieden, so
werden $i$ und $j$ gar nie inkrementiert! Wir wuerden also in einer
Endlosschleife enden. Um dies zu beheben, sollte man bei dieser Implementierung
am Ende $i$ noch um einen Index inkrementieren.

### Laufzeit

Wir wollen nun die Laufzeit von QuickSort analysieren. Das interessante an
QuickSort ist, dass es keine *garantierte* Laufzeit hat, sondern Best Case,
Average Case und Worst Case allesamt verschieden sind. Entscheidend hierbei ist
die Wahl des Pivots, also jenem Element, bezueglich welchem wir partionieren.

Betrachten wir zunaechst den schlechtesten Fall. Einer wurde oben schon
praesentiert. Sind alle Elemente gleich und bleiben wir fuer gleiche Elemente im
Partionierschritt nicht stehen, so laeuft QuickSort in *quadratischer* Zeit. Die
Partionierung wuerde das Pivot Element naemlich entweder ganz an den Anfang oder
ganz an das Ende befoerden. Wir muessten folglich fuer $n - 1$ Elemente weiter
rekursieren, was im Schnitt $O(n(n+1)/2) = O(n^2)$ ergibt. Der haeufiger
auftretende schlechte Fall verlaueft sehr aehnlich. Wird als Pivot naemlich
immer das kleinste oder immer das groesste Element gewaehlt, passiert genau
dasselbe. Nehmen wir beispielhaft an, dass die Sequenz schon sortiert ist und
wir immer das erste Element als Pivot nehmen. Dann wuerden wir unseren Iterator
$i$ also an Stelle zwei und $j$ an $n - 1$ initialisieren. Dann iterieren wir
mit $i$ solange, bis wir ein Element finden, dass nicht kleiner als das Pivot
ist. Weil das Pivot das kleinste Element war, ist schon das zweite Element
dieses nicht kleinere. Bei $j$ hingegen iterieren wir bis wir ein Element
finden, das nicht groesser als das Pivot ist. Da das fuer alle Elemente gilt,
wuerde $j$ bis zu $i$ wandern. Da $i$ nun groesser ist als das Pivot, wuerden
wir $i$ noch dekrementieren und dann mit dem Pivot Element vertauschen. $i$
waere aber gerade an der ersten Position, sodass nichts passiert. Im Weiteren
rekursiert man dann fuer den Teil links vom Pivot, der hier leer waere. Der
rechte Teil wuerde aber noch alle $n - 1$ weiteren Elemente enthalten. In der
naechsten Rekursionsstufe wuerde wieder das erste Element "wegscrapen" und eine
quadratische Laufzeit erhalten.

Wie haetten wir dieses Problem loesen koennen? Nun, wie gesagt, ist die Wahl des
Pivots entscheidend. Fragen wir uns zunaechst, welches Element denn *ideal* als
Pivot waere. Die Antwort ist klar: der Median. Dann waeren unsere Teilprobleme
in etwa gleich gross und man kaeme so schnell wie moeglich auf die unterste
Rekursionsstufe. Wir koennten also versuchen, den Median zu finden. Hierfuer
gibt es den QuickSelect Algorithmus, welcher weiter unten besprochen wird. Auch
koennten wir probieren, den Median nur zu approximieren und dennoch Laufzeit zu
sparen. Ebenso koennten wir Randomisierung einfuehren und jedes Mal eine neue
zufaellige Position als Pivot waehlen. Letztlich koennten wir uns das auch
sparen und ganz am Anfang die Sequenz einfach random-shufflen. Wir werden uns
diese Moeglichkeiten naeher ansehen.

Waehlen wir das Pivot jedenfalls guenstig, so erreicht QuickSort
erwartungsgemaess eine linearithmische Laufzeit von $2n \ln n \approx 1.39 n
\log_2 n$. QuickSort macht hierbei ungefaehr 39\% mehr Vergleiche als MergeSort,
ist in Praxis aber guenstiger, weil er in-place ist und Daten weniger oft
verschieben muss.

### Pivot Wahl

Sehen wir uns an, welche Moeglichkeiten es gibt, den Pivot so zu waehlen, das
wir eine linearithmische Laufzeit garantieren koennen.

#### Random Shuffle

Die einfachste Moeglichkeit, sicherzustellen, das QuickSort fast nie schlecht
ist, ist es, die Sequenz vor der Sortierung zufaellig zu permutieren. Betrachten
wir naemlich eine Sequenz von nur 10 Elementen. Dann gibt es also $10! =
3,628,800$ viele Permutationen. Von diesen ist nur eine die boese Permutation,
die die Sequenz bereits sortiert. Die Wahrscheinlichkeit hierfuer ist $27.6
\cdot 10^{-8}$ also unfassbar gering. Insbesondere diese Wahrscheinlichkeit bei
einer steigenden Anzahl von Elementen immer winziger bis es wahrscheinlicher
ist, das der Rechner noch waehrend dem Sortieren von einem Blitz getroffen wird.

Zur Permutierung verwenden wir den Knuth-Random-Shuffle. Dieser funktioniert wie
folgt:

1. Fuer jeden Index $i$ in der Sequenz:
   1. Waehle einen zufaelligen zweiten Index $j$ aus dem Intervall $[0, i]$.
   2. Vertausche die Werte an Index $i$ und $j$.
   3. Inkrementiere $i$.

Hierbei ist es sehr wichtig, dass man den zweiten Index nicht aus allen $n$
moeglichen Indizes waehlt, sondern wirklich nur bis $i$. Ueberlegen wir uns,
wieso. Wuerden wir fuer jeden der $n$ Indizes jeden der $n$ Indizes erlauben,
wuerden wir $n^n$ Moeglichkeiten erhalten. Wir wissen aber, dass es nur $n! \in
o(n^n)$ moegliche Permutationen gibt. Somit muessen unter diesen $n^n$ manche
oefter auftreten als andere. Dann waehlen wir aber nicht mehr *uniform* aus
allen Moeglichkeiten aus, was den Algorithmus zerstoert.

```Python
def random_shuffle(sequence):
	for i in range(len(sequence)):
		j = random.randrange(i)
		sequence[i], sequence[j] = sequence[j], sequence[i]
```

Dann implementiert man QuickSort also so:

```Python
def quick_sort(sequence):
	random_shuffle(sequence)
	_quick_sort(sequence, 0, len(sequence))
	return sequence
```

#### Random Pivot

Nach aehnlicher Idee koennen wir auch einfach "lazy" einen Random Pivot
bestimmen. D.h. in jeder Iteration waehlen wir den Pivot nicht als das erste
Element, sondern von einem zufaelligen Index. Dann vertauschen wir das Element
an diesem zufaelligen Index zunaechst in die erste Position und fahren dann wie
ueblich fort:

```Python
def partition(sequence, first, last):
    if (last - first) < 2:
        return first

    random_index = random.randrange(first, last)
    swap(sequence, first, random_index)

    pivot = sequence[first]
    i = first + 1
    j = last - 1

    while True:
        while i != j and sequence[i] < pivot:
            i += 1
        while i != j and sequence[j] > pivot:
            j -= 1
        if i == j:
            break
        swap(sequence, i, j)
        i += 1
    if sequence[i] > pivot:
        i -= 1
    swap(sequence, first, i)

    return i
```

oder

```C++
template <typename Iterator>
Iterator random_partition(Iterator begin, Iterator end) {
	using Distribution = std::uniform_int_distribution<std::size_t>;

	static std::mt19937 random_generator(std::random_device{}());

	if (std::distance(begin, end) < 2) return begin;

	auto distance = std::distance(begin, end);
	auto distribution = Distribution(0, distance - 1);
	auto index = distribution(random_generator);
	auto random_iterator = begin;
	std::advance(random_iterator, index);

	std::swap(*begin, *random_iterator);

	auto i = std::next(begin);
	for (auto j = std::prev(end); true; ++i) {
		while (i != j && *i < *begin) ++i;
		while (i != j && *j > *begin) --j;
		if (i == j) break;
		std::swap(*i, *j);
	}
	if (*i > *begin) --i;
	std::swap(*begin, *i);

	return i;
}
```

#### Median

Wie oben angesprochen waere es theoretisch ideal, wenn unser Pivot stets der
Median waere. Es gibt hier nun zwei Moeglichkeiten dies zu tun. Die eine
Moeglichkeit findet den Median exakt und in erwartet linearer, aber hoechstens
linearithmischer, Zeit. Die andere Moeglichkeit approximiert den Median
ledliglich. Die exakte Methode verwendet den QuickSelect Algorithmus und ist
unten beschrieben. Die inexakte Methode selektiert drei zufaellige Elemente und
nimmt den Median dieser drei Werte. Das ist eine Approximation des echten Median
und kann sogar 10% Verbesserung mit nur konstantem Aufwand bringen. Hierzu
fahren wir so fort:

1. Am Anfang jeder Iteration von QuickSort, waehle drei zufaellige (!) Elemente
   aus der Sequenz. Die Randomisierung hat denselben Sinn wie oben.
2. Waehle aus diesen drei Werten den Median.
3. Waehle diesen Median als Pivot, also vertausche ihn mit dem ersten Element
   der Teilsequenz und fahre normal mit QuickSort fort.

```Python
def median_of_three(sequence, first, last):
    a = first + random.randrange(last - first)
    b = first + random.randrange(last - first)
    c = first + random.randrange(last - first)

    if a < b:
        if b < c:
            return b
        elif a < c:
            return c
        else:
            return a
    elif a < c:
        return a
    elif b < c:
        return c
    else:
        return b
```

### QuickSelect

QuickSelect ist an und fuer sich ein eigener Algorithmus, der aber zur
Pivot-Selektion genutzt werden kann. Genauer dient er dazu, das $k$-te Element
aus einer Sequenz zu selektieren. Das $k$ bezieht sich hierbei auf die Position
in der sortierten Sequenz. Interessant ist aber, das die Sequenz hierfuer eben
noch nicht sortiert sein muss, sondern QuickSelect es auch ohne schafft, dieses
Element zu finden. Die Idee dieses Algorithmus ist wie folgt:

1. Solange, bis wir das $k$-te Element sicher gefunden haben:
   1. Waehle einen Pivot.
   2. Partioniere wie bei QuickSort bezueglich diesem Element.
   3. Swappe das Pivot wieder in die Luecke der Partionierung.
   4. Ist die Position des Pivots nun $k$, sind wir fertig ist.
   5. Ist die Position aber groesser, also rechts, von $k$, so fahre rekursiv
      nach links fort.
   6. Ist die Position kleiner, also links, von $k$, so fahre rekursiv nach
      rechts fort.

Dieser Algorithmust hat erwartet lineare Laufzeit. Im schlimmsten Fall ist er
einfach QuickSort und somit linearithmisch oder sogar quadratisch. Wenn wir also
mit QuickSelect den Median aus unserer Sequenz als Pivot fuer QuickSort waehlen
ist die erwartete Hoechstlaufzeit von QuickSort $O(n \log_2 n)$. Im schlimmsten
Fall laeuft QuickSelect aber quadratisch. Da wir dann aber sicher den Median
waehlen, haben wir sicher nur $\log_2 n$ Stufen. Das ergibt dann jedenfalls
$O(n^2 \log_2 n)$ als Hoechstlaufzeit.

```Python
def quick_select(sequence, k):
    if (len(sequence) < k):
        return

    k = int(k)
    first = 0
    last = len(sequence)

    while True:
        pivot = partition(sequence, first, last)
        if k < pivot:
            last = pivot
        elif k > pivot:
            first = pivot + 1
        else:
            return pivot
```

```C++
template <typename Iterator>
Iterator quick_select(Iterator begin, Iterator end, std::size_t k) {
	if (std::distance(begin, end) < k) return end;

	auto first = begin;
	auto last = end;
	while (true) {
		auto pivot = random_partition(begin, end);
		auto position = std::distance(begin, pivot);
		if (k < position) {
			last = pivot;
		} else if (k > position) {
			first = ++pivot;
			// Because the indices are relative
			k -= position + 1;
		} else {
			return pivot;
		}
	}
}
```

oder rekursiv:

```Python
def _recursive_quick_select(sequence, first, last, k):
    pivot = partition(sequence, first, last)

    if k < pivot:
        return _recursive_quick_select(sequence, first, pivot, k)
    elif k > pivot:
        return _recursive_quick_select(sequence, pivot + 1, last, k)
    else:
        return pivot


def recursive_quick_select(sequence, k):
    if (k >= len(sequence) or k < 0):
        return
    return _recursive_quick_select(sequence, 0, len(sequence), int(k))
```

### QuickSort + InsertionSort

Wie bei MergeSort ist es wieder oft eine gute Idee, bei wenigen Elementen
InsertionSort zu verwenden anstatt QuickSort. Der Cutoff kann hier mit ungefaehr
10 Elementen gewaehlt werden, sodass man also nur bei mehr als 10 Elementen
wirklich QuickSort macht. Auch ist es eine Idee, eine oder zwei Ebenen hoeher
als normal mit dem rekursieren aufzuhoeren und dann am Ende noch einmal
InsertionSort zu machen. Dann hat man eine konstante Anzahl an Invertierungen am
Ende, die man mit InsertionSort mit weniger Aufwand in linearer Zeit beheben
kann.

```Python

def _better_quick_sort(sequence, first, last, cutoff):
	if ((last - first) <= cutoff):
		return
	# Regular QuickSort

def better_quick_sort(sequence, cutoff=4):
	_better_quick_sort(sequence, 0, len(sequence), cutoff)
	return insertion_sort(sequence)
```

### Three-Way Partitioning

Vorhin hatten wir die Thematik von Duplikaten besprochen und festgestellt, dass
in der Praxis Duplikate oft vorkommen koennen. Manchmal haben wir sogar nur $k$
verschiedene Werte, wobei also $k << n$. In diesem Fall waere doch folgende Idee
gut: Partioniere so, dass alle Elemente, die gleich dem Pivot sind, in der Mitte
aggregiert sind. Dann ist also nicht nur das Pivot, sondern sogar alle diese
Elemente in ihrer finalen Position. Dann muessen wir nur mehr fuer alle Elemente
links sowie rechts von diesem Segment ("equal range") rekursieren. Fuer eine
sehr hohe Anzahl von praktischen Anwendungen ist QuickSort dann nicht nur
linearithmisch, sonder sogar linear!

Formal verwenden wir hierfuer einen *three-way-partioning* Algorithm. Dieser
wurde von Edsger Dijkstra entwickelt, der dabei die niederlaendische Flagge
sortieren wollte. Genauer haben wir drei verschiedene Arten von Elementen: rote
, blaue und weisse. Wir wollen nun die niederlaendische Flagge bilden, also alle
weissen in der Mitte haben, alle roten links und alle blauen Elemente rechts
haben. Hierfuer gehen wir so vor:

0. Wir nehmen an, das das erste Element weiss ist und waehlen es als Pivot
   Element.
1. Setze $i$ auf den zweiten Index der Sequenz. Dies wird unser hauptsaechlicher
   "Iterator" sein.
2. Setze $j$ auf den letzten gueltigen Index der Sequenz. Dieser Index zeigt
   immer auf den Platz, wo wir ein blaues Element hingeben, das wir mit $i$
   finden.
3. Setze $k$ auf den zweiten Index der Sequenz. Analog ist $k$ immer der Index,
   wo wir rote Elemente hingeben, die wir mit $i$ finden.
4. Dann, solange bis $i$ kleiner als $j$ ist, betrachte das Element an Index
   $i$. Gilt
   1. Dass es rot ist, so vertausche diese rote Element an Index $i$ mit dem
      Element an Index $k$. Inkrementiere dann sowohl $i$ als auch $k$.
   2. Dass es blau ist, so vertausche das Element mit dem an Index $j$ und
      dekrementiere nur $j$. Wir inkrementieren $i$ nicht, weil wir nicht
      wissen, was wir von $j$ erhalten haben (womoeglich ein blaues oder rotes
      Element).
   3. Sonst ist das Element weiss, inkrementiere also nur $i$. Insbesondere sind
      anfangs $i$ und $k$ also solange gleich, bis $i$ und $k$ an ein weisses
      Element kommen, sodass nur mehr $i$ inkrementiert wird. Deswegen duerfen
      wir dann auch $k$ inkrementieren, wenn wir Elemente bei $i$ und $k$
      vertauschen, weil wir von $k$ dann immer ein weisses Element erhalten.
5. Ist am Ende dann $j$ weiss, inkrementiere $j$ um einen Index, um es das erste
   Element der blauen Teilsequenz zu machen.
6. Vertausche das weisse Pivot mit dem Element an Index $k - 1$, da $k$ immer
   auf das erste weisse Element zeigt.
7. Da nun auch $k - 1$ weiss ist (wegen dem Pivot), ist $k - 1$ also der Start
   der weissen und Ende der roten Sequenz. Auch ist $j$ der Start der blauen und
   Ende der weissen Sequenz.
8. Gibt $k - 1$ und $j$ zurueck.

Das ergibt folgende QuickSort Implementierung:

```Python
def three_way_partition(sequence, first, last):
    pivot_index = random.randrange(first, last)
    swap(sequence, first, pivot_index)

    pivot = sequence[first]
    i = first + 1
    k = i
    j = last - 1
    while i <= j:
        current = sequence[i]
        if current < pivot:
            swap(sequence, k, i)
            k += 1
            i += 1
        elif current > pivot:
            swap(sequence, i, j)
            j -= 1
        else:
            i += 1
    if sequence[j] == pivot:
        j += 1
    swap(sequence, first, k - 1)

    return k - 1, j


def _three_way_quick_sort(sequence, first, last):
    if (last - first) < 2:
        return
    left_end, right_start = three_way_partition(sequence, first, last)
    _three_way_quick_sort(sequence, first, left_end)
    _three_way_quick_sort(sequence, right_start, last)


def three_way_quick_sort(sequence):
    _three_way_quick_sort(sequence, 0, len(sequence))
    return sequence
```

## Stabilitaet

Eine Eigenschaft von Sortieralgorithmen, die uns oft interessiert, ist
*Stabilitaet*. Wir nennen einen Sortieralgorithmus *stabil*, wenn er die
relative Reihenfolge gleicher Elemente nicht veraendert. Betrachten wir zum
Beispiel die Sequenz `[A B C B]`. Wenn wir diese Sequenz nun lexikographisch
sortieren, erhalten wir `[A B B C]`. Bezueglich dieser Ordnung sind die `B`s
also gleich. Notieren wir nun aber das erste `B` mit `B1` und das zweite mit
`B2`. Dann sind sowohl `[A B1 B2 C]` als auch `[A B2 B1 C]` korrekte
Permutationen fuer eine Sortierung. Aber nur im ersten Fall ist die relative
Reihenfolge der `B`s erhalten geblieben, da in der zweiten Sortierung `B1` mit
`B2` vertauscht wurde. Der erste Sortieralgorithmus waere also *stabil*, der
zweite nicht.

Zwei allgemeine Indizien fuer Stabilitaet eines Sortieralgorithmus sind
die Vertauschusoperationen und die verwendeten Vergleichsoperatoren. Faustregeln
hierbei sind:

1. Werden Elemente ueber weite Strecken hinweg vertauscht, so ist der
   Algorithmus meist nicht stabil.
2. Werden Elemente auch vertauscht, wenn sie gleich sind, ist der Algorithmus
   meist nicht stabil. Die Frage ist also, ob Elemente auch vertauscht werden,
   wenn $x \geq y$ (nicht stabil) gilt, oder nur $x > y$ (stabil).

Nun koennen wir die uns bekannten Algorithmen bezueglich der Eigenschaft der
Stabilitaet einordnen:

* QuickSort: nicht stabil, da Elemente ueber weite Strecken vertauscht werden.
* SelectionSort: nicht stabil, da wir womoeglich ein gleiches Element an einem
  anderen vorbeiswappen, wenn das momentane Minimum nach dem anderen gleichen
  liegt.
* InsertionSort: stabil, sofern wir aufhoeren, wenn das vorherige Element
  kleiner oder gleich und nicht nur kleiner ist. Man merkt aber auch, das
  Elemente nicht ueber weite Strecken, sondern nur lokal bzw. sequentiell
  vertauscht werden.
* ShellSort: nicht stabil, da ueber weite Strecken vertauscht wird.
* MergeSort: stabil, sofern wir beim Mergen immer das linke Element waehlen,
  wenn die Elemente auf beiden Seiten gleich sind.
* HeapSort: (siehe unten) nicht stabil, da wir ueber weite Strecken
  vertauschen.

## HeapSort

HeapSort ist ein weiterer Sortieralgorithmus, der in manchen Faellen gut
geeignet ist. Er hat naemlich garantiert linearithmische Laufzeit (wie
MergeSort) und ist in-place (wie QuickSort). Wir gehen hierbei so vor:

1. Fuehre die lineare Build-Heap Operation auf der Sequenz auf um einen Max-Heap
   aus der Sequenz zu machen.
2. Setze $i = n - 1$ auf den letzen gueltigen Index.
3. Dann, solange wie $i > 0$ gilt:
   1. Vertausche das Maximum (an Index 1) mit dem Element an Index $i$.
   2. Dekrementiere $i$ und verkuerze die Laenge der Sequenz somit.
   3. Fuehre die `sink()` Operation auf dem neuen Element durch um die
      Heap-Eigenschaft wiederherzustellen.

```Python
def sink(sequence, index, length):
    while True:
        left_child = (2 * index) + 1
        if left_child >= length:
            return
        right_child = 2 * index + 2
        greater_child = left_child
        if right_child < length:
            if sequence[right_child] > sequence[left_child]:
                greater_child = right_child
        if sequence[greater_child] > sequence[index]:
            swap(sequence, greater_child, index)
            index = greater_child
            continue
        return


def heap_sort(sequence):
    if (len(sequence) < 2):
        return sequence

    for i in range(len(sequence)//2, -1, -1):
        sink(sequence, i, len(sequence))

    for i in range(len(sequence) - 1, 0, -1):
        swap(sequence, 0, i)
        sink(sequence, 0, i)

    return sequence
```

```C++
template <typename RandomAccessIterator>
void sink(RandomAccessIterator begin,
					RandomAccessIterator iterator,
					RandomAccessIterator end) {
	while (true) {
		auto index = std::distance(begin, iterator);
		auto left_child = begin + ((2 * index) + 1);

		if (left_child >= end) return;

		auto larger_child = left_child;
		auto right_child = std::next(left_child);
		if (right_child < end && *right_child > *left_child) {
			larger_child = right_child;
		}

		if (*larger_child > *iterator) {
			std::swap(*larger_child, *iterator);
			iterator = larger_child;
		} else {
			return;
		}
	}
}

template <typename RandomAccessIterator>
void heapsort(RandomAccessIterator begin, RandomAccessIterator end) {
	auto distance = std::distance(begin, end);
	if (distance < 2) return;

	for (auto iterator = begin + distance / 2; iterator != begin; --iterator) {
		sink(begin, iterator, end);
	}

	for (auto last = std::prev(end); last != begin; --last) {
		std::swap(*begin, *last);
		sink(begin, begin, last);
	}
}
```

In Praxis wird HeapSort aber meist nicht verwendet, weil er sehr schlechte
Cache-Lokalitaet hat (da Kinder weit weg von ihren Elternknoten sein koennen).


## BucketSort

Normalerweise versuchen wir Sortieralgorithmen zu entwickeln, die fuer alle
moeglichen Eingaben das korrekte Ergebnis liefern. Hierzu lassen wir sogar
beliebige Kompartoren und Ordnungen zu. Manchmal ist das aber nicht optimal. So
kann es sein, dass wir schon irgendwelche Annahmen ueber unsere Daten treffen
koennen. Dann ist es moeglich, durch diese Annahmen, viel effizienter zu
sortieren.

Betrachten wir beispielsweise den Fall, dass wir einen String sortieren
moechten, der nur aus Buchstaben besteht. Wir koennen diesen String nun auf zwei
Weisen sortieren. Entweder wir betrachten ihn einfach als eine Sequenz von
Elementen, definieren die lexikographische Ordnung $\leq$ auf dieser Sequenz und
schieben sie in einen unserer Sortieralgorithmen. Aber geht das nicht besser?
Ueberlegen wir uns, was wir ueber diese Sequenz wissen. Wir wissen, dass er nur
aus 26 verschiedenen Zeichen besteht. Ginge es denn dann nicht auch, einfach zu
zaehlen, wie oft jeder Buchstabe vorkommt? Wuessten wir erstmal, dass wir 8 mal
den Buchstaben $A$ und 7 mal den Buchstaben $C$ haben, so koennten wir diese
Sequenz einfach durch Iteration ueber diese Zahlen wiederherstellen. Das
Sortieren selbst waere ein einfaches Zaehlen der Buchstaben im String. Das geht
in linearer Zeit!

```Python
def string_bucket_sort(string):
    string = string.lower()

    allowed = "abcdefghijklmnopqrstuvwxzy"
    assert all(i in allowed for i in string)

    buckets = [0 for i in range(26)]

    for character in string:
        buckets[ord(character) - ord('a')] += 1

    sorted_sequence = [(chr(i + ord('a'))) * buckets[i] for i in range(26)]

    return ''.join(sorted_sequence)
```

Bei den Buchstaben haben wir nun aber zwei Annahmen gemacht. Zum Einen, dass die
Sequenz nur diese 26 Buchstaben enthaelt. Zum Anderen aber auch, dass wir die
urspruenglichen Elemente nicht mehr brauchen. Das geht bei Zeichen oder
Zahlen. Was ist aber mit arbitraeren Objekten? Wenn wir beispielsweise wissen,
das wir nur zwei verschieden Arten von Objekten haben, dann waere BucketSort
eben gut. Wir koennten die urspruenglichen Objekte dann aber nicht einfach
zaehlen, wir wuerden sie erhalten wollen. Deswegen muss man in diesem
Allgemeinen Fall Listen der Objekte erhalten. Wir merken dann also auch, dass
BucketSort linear viel Platz brauchen wuerde.

Da wir die Elemente auch in der Reihenfolge durchiterieren und dann auch
konkatenieren, wie sie in der Sequenz auftauchen, ist BucketSort stabil. Das
setzt jedoch voraus, das wir Elemente *hinten* an die jeweiligen Buckets
anhaengen.

Die Laufzeit ist auch leicht festzustellen. Wir iterieren zuerst linear durch
die Liste, was $\Theta(n)$ ergibt. Dann haben wir noch Aufwand linear in der
Anzahl an Buckets, um sie zu konkatenieren. Das ergibt insgesamt eine Laufzeit
von:

$$\Theta(n + K)$$

Wir merken also auch, dass BucketSort nur dann Sinn macht, wenn $K \in o(n \log
n)$, da wir sonst auch einen der anderen Sortieralgorithmen verwenden koennten.

```Python
def general_bucket_sort(sequence, key, number_of_keys):
    buckets = [[] for _ in range(number_of_keys)]
    for element in sequence:
        buckets[key(element)].append(element)

    return [j for i in buckets for j in i]
```

## RadixSort

RadixSort ist nun ein weiterer Sortieralgorithmus, der stark auf BucketSort
basiert. Vorhin hatten wir nun schon einen String anhand der einzelnen Zeichen
sortieren koennen. Was nun aber, wenn wir ganze Strings sortieren moechten? Das
geht auch! Wir sortieren hierbei zunaechst nach dem letzten Zeichen. Das ist
sozusagen die *feinste* Sortierung. Danach sortieren wir nach dem vorletzten
Zeichen. Wir erhalten dadurch eine groebere Sortierung als vorher. Hierbei ist
es nun essentielle, das BucketSort stabil ist. Denn hier gilt nun, das wenn
Elemente bezueglich der vorletzten Ziffer gleich sind, die relative Reihenfolger
von vorher, also gerade die Sortierung nach dem letzten Zeichen, erhalten
bleibt. Somit bleibt die feinere Sortierung von vorher also erhalten. Sukzessive
sortieren wir dann nach immer groeberen "Indizes im Woerterbuch", bis wir
leztendlich beim ersten Zeichen sortieren. Hierbei wird also schon sortiert,
aber die relative Reihenfolge der einzelnen Aequivalenzklassen bleibt erhalten.

Formal betrachten wir hierzu unsere Schluessel mit einer $K$-adischen
Darstellung. Das bedeutet, das jeder Schluessel aus "Ziffern" besteht, die
jeweils in der Menge $\{0, ..., K - 1\}$ sind. Somit ist jeder Schluessl also
ein Wert aus $\{0, ..., K^d - 1\}$. Haben unsere Schluessel dann $d$ Ziffern, so
sortieren wir einfach Schritt fuer Schritt via BucketSort nach einem weiteren
Zeichen. Hierbei gehen wir durch die Ziffern aber rueckwaerts, damit immer
groeber sortiert wird. Sind nicht alle Schluessel $d$ Ziffern lang, padden wir
die kleineren Schluessel auf $d$ Ziffern mit Nullen (Im Woerterbuch ist "apfel"
vor "apfelbaum").

```Python
def radix_sort(sequence, length, key, number_of_keys):
    for position in range(length):
        current_key = lambda e: key(e[length - position - 1])
        sequence = general_bucket_sort(sequence, current_key, number_of_keys)

    return sequence

def string_radix_sort(strings):
    max_length = max(len(string) for string in strings)
    # Pad the strings to the maximum length
    for i, string in enumerate(strings):
        if len(string) < max_length:
            strings[i] = string + '\0' * (max_length - len(string))
    # The nil character should always compare smaller
    key = lambda c: ord(c) - ord('a') + 1 if c != '\0' else 0
    # the nil character + 26 characters
    sorted_strings = radix_sort(strings, max_length, key, 27)

    # Get rid of the nil characters (they are only padding)
    return [i.replace('\0', '') for i in sorted_strings]
```
