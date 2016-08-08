# Hashing

$\newcommand{\deg}{\mathop{\rm deg}\nolimits}$
$\newcommand{\indeg}{\mathop{\rm indeg}\nolimits}$
$\newcommand{\outdeg}{\mathop{\rm outdeg}\nolimits}$

In diesem Kapitel beschaeftigen wir uns mit *Hashing* und im Weiteren dann mit
*Hashtabellen*. Eine Hashtabelle ist eine *assoziative* Datenstruktur. Ein
solche Datenstruktur nennt man auch *Woerterbuch* (*dictionary*). Eine
assoziative Datenstruktur speichert eine Menge von Elementen, welche ueber einen
*eindeutigen* Schluessel identifiziert werden.

## Hashfunktionen

Eine Hashfunktion $h: \mathbb{K} \rightarrow {0, ..., m - 1}$ bildet eine Menge
$\mathbb{K}$ von Schluesseln auf eine Zahl $\in {0, ..., m - 1}$ ab, wobei $m$
die Anzahl Elemente einer *Hashtabelle* $T$ ist. Die Hashtabelle $T$ ist hierbei
einfach ein Array. Die Kardinalitaet $|\mathbb{K}|$ der Schluesselmenge
bezeichnen wir mit $N$. Wir bilden also $N$ Schluessel auf $m$ *Slots* oder
*Buckets* in der Hashtabelle ab. Zu einem bestimmten, konkreten Zeitpunkt $t$
befinden sich dabei $n$ Elemente in der Hashtabelle, welche also ueber $n$ der
insgesamt $N$ Schluessel eindeutig identifiziert werden.

Hashfunktionen liefern und arbeiten meist auf Zahlen. In der Praxis sind aber
nicht alle Objekte, die wir hashen moechten, notwendigerweise
Zahlen. Beispielsweise will man oft auch Strings oder andere Objekte
hashen. Hierfuer verwendet man dann *Pre-Hash* Funktionen, welch ein Objekt erst
in eine Zahl umwandeln. In Java verwendet man hierfuer zum Beispiel die
`hashCode()` Funktion und in Python `hash()`.

### Eigenschaften

Wir haben folgende Anforderungen an eine Hashfunktion:

* Schneller Zugriff (*Zeiteffizienz*)
* Platzsparend (*Speichereffizienz*), bestenfalls also eine surjektive Abbildung
  von Schluesseln auf Buckests.
* *Gute Streuung*: moeglichst breite und uniforme Verteilung von Schluesseln auf
  Slots. Zusammen mit obigem Punkt ergibt sich also, dass wir eine bijektive
  Abbildung anstreben (aber sie wahrscheinlich nicht bekommen werden).

Im Idealfall (einer bijektiven Abbildung) befindet sich der Wert eines
Schluessels genau in $T[h(key)]$. In diesem Fall waere die Laufzeit von Suche,
Einfuegen sowie Loeschen konstant (unabhaengig von der Anzahl an Schluesseln in
der Tabelle).

In der Praxis ist eine perfekte Zuordnung zwischen den gespeicherten Schluesseln
un den Adressen der Tabelle nur bei einer statischen Tabelle moeglich (wobei die
maximale Anzahl an Elementen also schon vorher fixiert werden muss). Ebenso
gilt, dass die Abbildung $h$ meist nicht surjektiv ist, sodass es auch leere
(ungenutzte) Eintraege geben kann. Dann ist $h$ in der Realitaet oft auch nicht
injektiv, sodass bestimmmte Schluessel also auf den selben Eintrag abgebildet
werden. Das nennt man dann eine *Kollision*.

Wir merken, dass wenn man die Theorie verlaesst, nicht alle, wenn ueberhaupt
irgendwelche, der von uns erwuenschten Eigenschaften erreichbar sind. Insofern
muss man bei der Konstruktion einer Hashfunktion also Kompromisse
machen. Verschiedene gaengige Hashalgorithmen machen hierbei andere
Kompromisse. Beispiele sind:

* `sha-1`, `sha-3`, `md5`: Kryptographische Hashfunktionen, mit den
  Eigenschaften, dass sie schnell zu berechnen sind; es unglaublich schwer ist,
  den gehashten Wert vom Hashwert zu rekonstruieren; kleine Veraenderungen
  grosse Unterschiede im Hashwert verursachen (*avalanching effect*) sowie die
  Anzahl an Kollision fuer alle praktischen Nutzen winzig ist.
* `bcrypt`: ist eine *sehr langsame* Hashfunktion. Das macht brute force
  Attacken weniger machbar.
* `djb2`: eine sehr schnelle Hashfunktion
  (http://www.cse.yorku.ca/~oz/hash.html)

## Hashing mit Chaining

Beim Hashing mit Chaining betrachten wir zunaechst eine Hashtabelle mit $m$
Buckets, in welche wir unsere $n$ Elemente platzieren wollen. Bei dieser
konkreten Variante geht nun von jedem Bucket eine verkettete Liste aus, welche
wir als *Chain* bezeichnen. Um ein Element einzufuegen, wird also zuerst ueber
eine Hashfunktion der Index der Liste des Elements in der Hashtabelle bestimmt,
und dann das Element in diese Liste eingefuegt. Kollidieren zwei Schluessel, so
werden sie in dieselbe Liste eingegliedert. Um ein Element zu suchen, muss
zuerst wieder ueber die Hashfunktion der richtige Bucket gefunden werden und
dann die Liste entlang iteriert werden.

Da also jedes der $n$ Elemente in seiner Liste einen Knoten beansprucht, hat
eine solche Hashtabelle eine Platzkomplexizitaet von $O(m + n)$ -- $m$ fuer das
Array und $n$ fuer die jeweiligen Knoten. Im schlimmsten Fall (worst-case)
hashen hierbei alle Elemente in den selben Bucket und formen somit eine
verkettete Liste der Laenge $n$, womit alle Operationen wieder *linear* in der
Anzahl an Elementen waeren.

Normalerweise wuerde man die Insert Operation so implementieren, dass die
verkettete Liste des Buckets, gegen welchen das einzufuegenden Element gehashed
wird, zuerst nach Duplikaten durchsucht wird. Gibt es kein solches Duplikat,
kann das Element ganz vorne in die Liste eingefuegt werden (eine einzeln
verkettete Liste wuerde genuegen). Alternativ kann es uns aber auch egal sein,
ob die Liste Duplikate enthaelt oder nicht. Lieber haetten wir garantiert
konstante Einfuegezeit. Dann koennen wir beim Einfeugen das Element auch direkt,
ohne Inspektion der entsprechenden Liste, vorne an die Liste splicen. Dann waere
die Insert Operation unbeachtete vom momentanen Zustand der Hashtabelle stets
$\in O(1)$.

Oftmals interessieren wir uns aber nur fuer die *erwartete* oder
*durchschnittliche* Laufzeit. Unter der Annahme, dass ein Schluessel mit einer
uniformen Wahrscheinlichkeit von $1/m$ auf einen der $m$ Buckets gehasht wird,
ergibt sich aus dem Schubfachprinzip, dass eine Liste (im Durchschnitt) eine
Laenge von $n/m$ haben muss. Daher ist die *erwartete* Laufzeit also $O(1 +
n/m)$.

### Beweis

Wir koennen nun formal beweisen, dass die Laenge einer Liste bei einer uniform
hashenden Funktion (*uniform hashing assumption*) wirklich $n/m$ ist, wobei $m$
die Groesse der Hashtabelle ist, und $n$ die Anzahl an Elementen in der
Hashtabelle. Wir betrachten hierzu einen beliebigen aber festen Index $i$ in der
Hashtabelle sowie die daran angeschlossene verkettete Liste. Dann ergibt sie die
Laufzeit fuer eine Suche in dieser Liste aus einem konstanten Aufwand $O(1)$
fuer das Hashen, sowie dem Erwartungswert $\mathbb{E}$ der Laufzeit fuer die
Suche. Sei dann $L$ die Zufallsvariable, die die Laenge der Liste an Position
$i$ beschreibt und seien weiter $L_e \in \{0, 1\}$ binaere Zufallsvariablen, die
den Wert Eins annehmen, wenn das $e$-te der $n$ Elemente in dieser Liste
ist. Dann ergibt sie die Laenge $L$ der Liste aus der Summe ueber alle $L_e$:

$$L = \sum_e L_e$$

Dann koennen wir nun die erwartete Listenlaenge bestimmen durch:

$$
\begin{align}
	\mathbb{E}[L] &= \mathbb{E}[\sum_e L_e]\\
	              &= \sum_e \mathbb{E}[L_e]\\
				  &= \sum_e \Pr[L_e = 1]\\
				  &= \sum_e \frac{1}{m}\\
				  &= n \cdot \frac{1}{m}
\end{align}
$$

Es folgt also tatsaechlich, dass die Laufzeit fuer eine Suche in einer
verketteten Liste gerade

$$O(1 + \mathbb{E}(L)) = O(1 + n/m)$$

ist. Diesen Wert $n/m$ nennt man auch *Load Factor* $\alpha$. Da die Laufzeit
fuer eine Suche vom Load Factor $\alpha = n/m$ abhaengt, lohnt es sich zu
analysieren, wie man $m$ und $n$ waehlen sollte:

* Ist $m$ zu gross, werden wir nur schwer surjektiv abbilden und somit relativ
  viel Platz verschwenden (viele leere Ketten).
* Ist $m$ zu klein, werden die Ketten zu lang. Somit wird die Laufzeit, wenn
  auch konstant, natuerlich groesser. In der Praxis muss man sich naemlich auch
  darum Gedanken machen, dass das Iterieren durch eine verkettete Liste sehr
  cache-ineffizient ist.

In der Praxist ist $\alpha = 5$ oft eine gute Wahl und ergibt eine
unproblematische konstante Laufzeit.

## $c$-universelle Hashfunktionen

Wir wollen im Weiteren nun das Konzept von *$c$-universellen Hashfunktionen*
beleuchten. Sei hierzu $c$ eine positive Konstante $\in \mathbb{R}^+$ und
$\mathbb{H}$ eine Familie (Menge) von Hashfunktionen $h: \mathbb{K} \rightarrow
\{0, ..., m - 1\}$. Auch seien $x,y$ zwei verschiedene Schluessel. Dann nennt
man diese Familie $\mathbb{H}$ von Hashfunktionen *$c$-universell*, wenn gilt:

$$\frac{|\{ h \in \mathbb{H} | h(x) = h(y) }|\}{|\mathbb{H}|} \leq \frac{c}{m}$$

Das bedeutet also, dass das Verhaeltnis bzw. die relative Anzahl an
Hashfunktionen in dieser Familie $\mathbb{H}$, fuer welche diese zwei Schluessel
$x \neq y$ kollidieren, durch $c/m$ nach oben beschraenkt ist. Aus einer
frequentistischen (Laplace) Sicht betrachtet bedeutet das also, dass wenn wir
eine *zufaellige* Hashfunktion $h$ aus dieser Familie $\mathbb{H}$ auswaehlen,
die Wahrscheinlichkeit, dass wir gerade eine Funktion ziehen, bei welcher $x$
und $y$ kollidieren, maximal $c/m$ ist (guenstig durch moeglich).

Wir wissen also, dass die Wahrscheinlichkeit $c/m$ ist, dass wir eine
Hashfunktion aus $\mathbb{H}$ ziehen, sodass $x$ und $y$ kollidieren. Das sagt
uns gerade, dass die Wahrscheinlichkeit, dass zwei Schluessel bei einer
beliebigen $c$-universellen Hashfunktion kollidieren, eben genau $c/m$ ist. Denn
genau mit dieser Wahrscheinlichkeit, waehlen wir eine der Funktionen aus dieser
Familie, bei welcher $x$ und $y$ kollidieren. Somit haben wir fuer eine
$c$-universelle Funktion $h \in \mathbb{H}$:

$$\forall x, y \in \mathbb{K}, x \neq y: \Pr[h(x) = h(y)] \leq \frac{c}{m}$$.

Hierbei nennen wir $1$-universelle Hashfunktionen, bei denen die
Kollisionswahrscheinlichkeit also uniform $1/m$ ist, auch einfach *universell*
oder *uniform-hashend*.

Wir koennen uns also intuitiv vorstellen, dass Hashfunktionen aus
$c$-universellen Familien mit $c > 1$ inherent eine groessere
Kollisionswahrscheinlichkeit aufweisen. Wenn $c = 2$, ist die
Wahrscheinlichkeit, dass die ersten zwei Schluessel einer Hashtabellen
kollidieren nicht mehr $1/m$ (uniforme Wahrscheinlichkeit dass der zweite
Schluessel genau in den selben Bucket hasht), sondern auf Grund der natuerlichen
Eigenschaften der Hashfunktion einfach schon $2/m$. Eine solche Hashfunktion
haette also eine Art *Bias*. Auch kann man es sich so vorstellen, dass eine
$c$-universelle Hashfunktion immer $c$ mal probieren darf, eine Kollision zu
erzeugen. D.h. wenn man einen Schluessel $x$ betrachtet, der schon in der
Hashtabelle ist, und $y$ ein weiterer Schluessel, dann ergaebe sich ja eine
Kollision, wenn $h(x) = h(y)$. Sagen wir, die Wahrscheinlichkeit dafuer ist
zunaechst uniform, also $1/m$. Eine $c$-universelle Hashfunktion darf nun aber
$c$ mal probieren, schlecht zu sein und eine Kollision zu erzeugen. Denn dann
haetten wir $c$ mal eine Wahrscheinlichkeit von $1/m$, welche bei einer
Disjunktion also addiert wuerden ($1/m$ oder $1/m$ oder ...).

Vorher hatten wir den Ausdruck $\mathbb{E}[L] = \sum_e \Pr[L_e = 1]$ fuer den
Erwartungswert der Laenge einer Liste und somit fuer die Laufzeit einer
Suche. Dann hatten wir oben angenommen, dass die Wahrscheinlichkeit, dass ein
Element in der Liste an Index $i$ ist, gerade $1/m$ ist. Nun wissen wir aber
auch, dass das nur fuer ($1$-)universelle Hashfunktionen gilt. Allgemein haben
wir also:

$$\mathbb{E}[L] = \sum_e \Pr[L_e = 1] = \sum_e \frac{c}{m} = n \frac{n}{m}$$

Somit ergibt sich fuer die Laufzeit einer Suche in einer Hashtabelle mit einer
$c$-universellen Hashfunktion entsprechend auch: $O(1 + c \cdot n/m)$. Somit
ergibt sich auch eine neue Interpretation der Laufzeit einer
Hashfunktion. Vorher konnten wir $O(1 + n/m)$ so deuten, dass wir einen
konstanten Aufwand fuer das Hashen haben und dann wegen dem Schubfachprinzip
$n/m$ Elemente in einer Liste liegen muessen. Anders gedeutet ergab sich dieser
Wert ja eigentlich aus $n \cdot 1/m$, weil jedes der $n$ Elemente eine
Wahrscheinlichkeit von $1/m$ hatte, in diese Liste zu hashen. Nun haben die $n$
Elemente also eine Wahrscheinlichkeit von jeweils $c/m$. Wenn wir das aber
wieder anders deuten, haben wir, dass $c$ ein "Faktor fuer das Schubfachprinzip"
ist. Eine $c$-universelle Hashfunktion hat also $c$ mal so viele Elemente in
einer Liste, wie das Schubfachprinzip fuer eine uniform-hashende (universelle)
Hashfunktion schliessen lassen wuerde.

Es sei angemerkt, dass es fuer beliebige $m$ *stets* eine $2$-universelle
Familie von Hashfunktionen gibt.

Haben wir nun eine Familie von Hashfunktionen gegeben, und wollen $c$ bestimmen,
so tun wir folgendes:

1. Wir schreiben in den Zeilen einer Tabelle alle Paare von Schluesseln auf.
2. In den Spalten alle Funktionen.
3. Dann kreuzen wir alle Eintreage in der Tabelle an, wo es eine Kollision gibt.
4. Wir notieren die maximale Anzahl an Kollisionen fuer ein Paar, also die
   maximale Anzahl an Kreuzen in einer Zeile.
5. Das ist die Anzahl an Hashfunktionen mit Kollisionen, die wir benutzen, um
   $c$ zu bestimmen.
6. Dann wissen wir:

$$c \geq \max_{x \neq y} \frac{|\{h \in H \,|\, h(x) = h(y)\}|}{|H|} \cdot m$$

### Vektorhashfunktion

Wir wollen ein Beispiel fuer eine $c$-universelle Familie von Hashfunktionen
betrachten. Hierfuer betrachten wir die Schluessel in ihrere
Binaerdarstellung. Sei dann $m$ die Tabellengreosse und prim. Dann ist
$\mathbb{Z}_m$ bekanntlich ein endlicher Koerper. Sei dann weiter $w :=
\left\lfloor \log_2 m\right\rfloor$ die Anzahl an Bits, die man benoetigt, um
Indizes in die Hashtabelle binaer zu kodieren. Die Idee ist es dann, die
Schluessel, die wir in die Hashtabelle einfuegen wollen und moeglicherweise mehr
als $w$ Bits haben, in $k = \left\lceil \frac{\log_2 b}{w}\right\rceil$
Bitsequenzen zu je $w$ Bits aufzuteilen, wo $b$ die Anzahl Bits der Schluessel
sei. Da also jeder dieser Teile aus $w$ Bits besteht, koennen wir diese
Sequenzen als Zahlen aus dem Intervall $[0, ..., 2^w - 1]$ interpretieren. Der
Schluessel selbst kann dann also als $k$-Tupel oder $k$-dimensionaler Vektor
solcher Teile behandelt werden. Auch bemerke man, dass jeder Teil nun mit ebenso
vielen Bits kodiert wird, wie ein Index in die Tabelle. Insofern *ist* also
jeder Teil ein separater Index in die Tabelle.

Fuer das Verfahren definiert man dann im Weiteren noch einen *Hashvektor*
$\mathbf{h}$, welcher jeweils Koeffizienten fuer die Teile in einem
Schluesselvektor $\mathbf{k}$ enthaelt. Jeder Wert $h_i$ in diesem Hashvektor
ist dabei ein zufaelliger Wert aus $\{0, ..., m - 1\}$. Somit ergibt sich dann
aus dem Skalarprodukt $\mathbf{h}^\top \mathbf{k} = \langle \mathbf{h},
\mathbf{k} \rangle$ ein reller Wert. Sowohl die Koeffizienten $h_i$ im
Hashvektor also auch die Schluesselteile $k_i$ im Schluesselvektor sind mit $w$
Bit kodiert und im Intervall $\{0, ..., m - 1\}$. Somit ergebn sich aus der
Multiplikation je zweier Komponenten also maximal $2w$-bit breite Werte. Die
Summe erhaelt diese Anzahl an Bits. Wir merken also, dass das Skalarprodukt
alleine noch nicht ausreicht, um einen Index in die Tabelle zu formen.

Der letze Schritt, der nun auch die letzendliche Hashfunktion
$h_\mathbf{h}(\mathbf{k})$ bildet, ergibt sich dann erst durch:

$$h_\mathbf{h}(\mathbf{k}) = \mathbf{h}^\top \mathbf{k} \mod m$$

Wenn nun $m$ prim war, dann gilt: eine solche Hashfunktion ist
$1$-universell. Somit haben wir also fuer die Familie $\mathbf{H} = \{
h_\mathbf{h} \,|\, \mathbf{h} \in \{0, ..., m - 1\}^k\}$ eine *universelle*
Familie von Hashfunktionen.

Es sei aber angemerkt, dass diese Hashfunktion in der Praxis einige Probleme
haette. Zum Einen ist es nur sehr schwer zu realisieren, dass eine *dynamische*
Hashtabelle eine Primzahl als Groesse hat. Auch ist das Skalarprodukt eine sehr
teure Operation, die eben zu einer `for`-Schleife ueber die Vektorkomponenten
ausartet (oder einer SIMD-Vektorinstruktion).

Letztlich sei nun noch eine Intuition gegeben, wieso diese Hashfunktion "gut",
also universell ist: durch das Aufteilen der Bits, Multiplizieren mit
Hashvektorkomponenten und letztlicher Addition werden die Bits der Werte sehr
stark durcheinandergemischt. Das ergibt eine sehr gute Variation der Hashwerte.

### Hashfunktionen fuer Integer

Es gibt einige bekannte universelle Familien von Hashfunktionen, welche
besonders guenstige Eigenschaften haben und in der Praxis oft verwendet
werden. Wir wollen diese untersuchen.

#### $ax + b \mod p \mod m$

Eine Variante ist die folgende Konstruktion. Waehle:

* $a,b$ als zufaellige Ganzzahlen
* $p$ eine zufaellige Primzahl $\geq m$, wo $m$ die Groesse unseres Universums
  $U = \{0, ..., m - 1\}$ ist.

Dann ist die folgende Hashfunktion

$$h_{a,b}(x) = ((ax + b) \mod p) \mod m$$

*universell*.

#### Beweis

Wir wollen dies formal beweisen. Seien hierzu $x,y$ zwei Schluessel und $a,b,p$
wie oben beschrieben passend fuer eine Hashfunktion $h := h_{a,b}(x)$
gewaehlt. Dann gilt:

$$
\begin{align}
	h(x) &= h(y)\\
	((ax + b) \mod p) \mod m &= ((ay + b) \mod p) \mod m\\
	ax + b &\equiv ay + b + im \mod p
\end{align}
$$

da wenn $ax + b$ und $ay + b$ modulo $m$ gleich sind, sie sich also um ein
Vielfaches $i$ von $m$ unterscheiden. Hierbei ist $i$ nun im Intervall
$[0, (p-1)/m]$. Die obere Schranke gilt, weil wir jedes moegliche $i$ als Bruch
ueber $m$ darstellen koennen, wenn wir es mit $m$ erweitern:

$$i = \frac{i'}{m}$$

fuer geeignetes $i'$. Dann haben wir also $im = \frac{i'm}{m} = i'$. Es bleibt
also nur $i'$. Dann erkennen wir auch, dass $ay + b + i' \mod p = (ay + b) \mod
p + i' \mod p$ gilt. Somit gilt, dass sobald $i' \geq p$, wir wieder eine Zahl
aus dem obigen Intervall erhalten. Insbesonder ist nun $'i = p - 1$ die obere
Schranke fuer $i'$, weil die nachste Ganzzahl $i' + 1 = p$ ja eben wieder die
Null ergibt.

Wenn nun, wie angenomen, $x \neq y$, dann ist auch $x - y \neq 0$ und hat somit
im Restklassenring modulo $p$ ein multiplikatives Inverses. Dann koennen wir
also vereinfachen:

$$
\begin{align}
	ax + b &\equiv ay + b + im \mod p \\
	ax + b - ay - b &\equiv im \mod p \\
	a(x - y) &\equiv im \mod p \\
	a &\equiv im \cdot (x - y)^{-1} \mod p
\end{align}
$$

Da nun $a$ also in einer Restklasse von $p$ ist (da wir modulo $p$ rechnen),
gibt es fuer $a$ theoretisch $p$ viele moegliche Werte. Wir wollen aber $a \neq
0$, da die Hashfunktion sonst keinen Sinn macht. Also bleiben fuer $a$ gerade
$p - 1$ Moeglichkeiten. Nun haben wir, wie oben bestimmt, $(p - 1)/m$ viele
Moeglichkeiten, ein $i$ zu bestimmen, dass fuer diese Gleichung passt. Hierbei
nehmen wir an, dass $(p - 1)/m$ eine Ganzzahl ist. Wenn wir nun annehmen, dass
$a$ uniform gewaehlt wird, dann liegt $a$ auch uniform in einer Restklasse von
$p$. D.h. die Wahrscheinlichkeit fuer eine Kolllision, also dass $x,y$ fuer
einen konkreten Wert von $a$ gerade passend gewaehlt sind, ist eben $1/(p -
1)$. Da es nun aber noch $(p - 1)/m$ weitere Moeglichkeiten fuer diese
Gleichheit gibt, ist die Wahrscheinlichkeit fuer eine Kollision eben genau so
viel mal hoeher. Die Kollisionswahrscheinlichkeit ergibt somit:

$$\frac{(p - 1)/m}{p - 1} = \frac{p - 1}{m(p - 1)} = \frac{1}{m}$$

Diese Hashfamilie ist also universell. Sie wird im Uebrigen auch von Linear
Congruental Engines (LCG) verwendet. Eine einfachere Variante verzichtet auf die
Addition mit $b$, ist aber nur $2$-universell:

$$h_a(x) = (ax \mod p) \mod m$$

#### Multiply-Shift Schemes

Eben wurde die $(ax + b \mod p) \mod m$ Hashfunktion vorgestellt, welche
universell ist. Wie man sieht, enthaelt sie aber zwei Modulo Operationen, welche
in der Realitaet relativ teuer sein koennen. Deswegen gibt es Hashfunktionen,
welche auf dieser obigen basieren, aber billiger zu implementieren sind.

Die erste solche Funktion, die wir untersuchen wollen, basiert auf der
einfacheren oben vorgestellten Variante $(ax \mod p) \mod m$. Hierfuer waehlt
man:

* Die Tabellengroesse $m$ als Zweierpotenz: $m = 2^M$ (in der Praxis machbar)
* $w$ die Wortgroesse, also z.B. `sizeof(int)`
* $a$ als zuefaellige Zahl $< 2^w$, die also in ein Wort mit $w$ Bits
  passt. Auch gelten folgende Constraints:
  1. $a$ muss positiv sein.
  2. $a$ muss *ungerade* sein
  3. $a$ sollte nicht nahe einer Zweierpotenz sein (also moeglichst viele Bits
     gesetzt haben, da mit $a$ multipliziert wird)

Um nun einen Hashwert $h_a(x)$ zu berechnen, multipliziert man $a$ mit $x$
modulo $2^w$ und behaelt nur die oberen $M$ Bit (die Anzahl bits um einen Index
in die Tabelle zu kodieren):

$$h_a(x) = (ax \mod 2^w) \div 2^{w - M}$$

Diese Form hat nun zwei tolle Vorteile fuer die praktische Implementierung,
naemlich:

1. Da $w$ die Wortgroesse, also die Groesse des Datentyps in Bit, ist,
   entspricht eine Modulo Operation mit $2^w$ einem *Cast* auf den Datentyp
   (weil $2^w$ gerade nur den Bit gesetzt hat, der nach der Wortgroesse kommt,
   sodass die Modulooperation nur diese erhaelt)
2. Die Division mit der Zweierpotenz $2^{w - M}$ entspricht einem Shift um $w -
   M$ Bits, um gerade die oberen $M$ Bit ganz nach rechts zu holen.

Somit waere die Implementierung in `C` fuer 32-Bit Integer:

```
h(x) = ((unsigned int) a * x) >> (w - M);
```

wobei `w = 8 * sizeof(x)` oder hier eben genau `32`. Fuer eine Hashtabelle mit
$256$ Eintragen haetten wir also:

```
h(x) = ((unsigned int) a * x) >> 24;
```

wobei man diesen Wert beim resizen immer schoen cachen koennte. Wir koennen uns
auch ueberzeugen, dass wir hier nichts Neues erfunden haben, sondern immer noch
das obige (einfache) Schema verfolgen:

1. $2^w$ ist offensichtlich groesser als die Tabellengroesse $m$. Diese Zahl ist
   zwar nicht prim, das hat aber im Weiteren keinen Einfluss (siehe Wikipedia).
2. Da wir gerade $M$ bit herunterholen, ist dieser Wert in einer Restklasse von
   $m$, also so, als ob wir (mit den oberen Bits) $\mod m$ gerechnet haetten.

Diese Hashfunktion ist aber wieder nur $2$-universell. Waehlen wir noch eine
weitere nicht-negative Konstante $b < 2^{w - M}$ und addieren diese auf $ax$, so
erhalten wir eine *universelle* Hashfunktion mit:

$$h_{a,b}(x) = ((ax + b) \mod 2^w) \div 2^{w - M}$$

bzw.:

```
h(x) = ((unsigned int) a * x + b) >> (w - M);
```

hierbei muss $b$ nicht wie $a$ notwendigerweise unerade sein, sollte aber wieder
moeglichst weit von einer Zweierpotenz entfernt sein, also viele Bits gesetzt
haben, um moeglichst viel Schaden anzurichten. Die Addition wuerde ueber Carries
dann eben noch weiter durchmischen. Wie man sieht ist da nun eben auch die oben
angefuehrte universelle Hashfunktion.

https://en.wikipedia.org/wiki/Universal_hashing#Constructions

### Hashfunktionen fuer Strings

Hashfunktionen fuer Strings betrachten allgemein die einzelnen Zeichen, also
Bytes und berechnen daraus den Hashwert. Sei hierfuer $\mathbf{s} = (c_0, ...,
c_l)$ ein String der Laenge $l$, als Vektor seiner Zeichen repraesentiert. Diese
Laenge $l$ muss hierbei nicht bekannt oder statisch sein. Dann sind unverselle
Hashfunktionen fuer diese Strings parametrisiert durch eine grosse Primzahl $p
\geq \max\{u, m\}$, wobei $u$ so gewaehlt ist, dass jedes Zeichen $x_i \in
[u]$. Meistens bedeutet das, dass $u = 127$ wenn die Zeichen in ASCII kodiert
sind. Man muss also die Primzahl $p$ groesser als die Kardinalitaet des
Zeichensatzes, und groesser als die Laenge der Hashtabelle waehlen. Dann sei
noch $\mathcal{s}$ ein *Seed*-Wert. Wir haben dann eine universelle Hashfunktion
mit:

$$h_{\mathcal{s}} = \left(\sum_{i=0}^l x_i \mathcal{s}^i \right) \mod p$$

Wir koennen dann in der Praxis die Horner-Methode benutzen, um diese Polynom zu
berechnen. Sei hierzu $h_i$ der momentane Hashwert und initial gleich dem
*hoechsten Koeffizenten* $x_l$, dann kann man ein solches Polynom "rekursiv"
berechnen durch:

$$h_{i + 1} = (h_i \cdot \mathcal{s}) + x_i$$

Das ist im Prinzip einfach ein iteratives "Herausheben" der unabhaengigen
Variable, so als ob man

$$27x^3 + 4x^2 + 7x - 13$$

umschreiben wuerde zu

$$((27x + 4)x + 7)x - 13$$

Hierbei ist waere also $27$ der initiale Wert und somit der innerste Koeffzient,
sowie dann $4, 7, 13$ die weiteren $x_1,x_2,x_3$ und dann eben die Zeichen des
Strings, wenn wir diese betrachten. Nun kann man diese obige Formel auch sehr
effizient und schoen wie folgt in Code implementieren:

```C
uint32_t hash(const char* string, int seed, int prime) {
	uint32_t hash = initial_value;
	for (char c = *string; c != '\0'; c = *(++string)) {
		hash = ((hash * seed) + c) % prime;
	}
	return hash;
}
```

Diese Implementierung nennt man dann eine *Multiplikative Hashfunktion*. In der
Praxis kann man die Modulo-Operation dann oftmals durch einfachen
Integer-Overflow vermeiden. Hier sind nun einige gaengige Parametrisierungen
dieser Funktion:

| Implementation              | Initial Value | Seed |
| djb2                        |      5381     | 33   |
| K&R                         |         0     | 31   |
| java.lang.String.hashCode() |         0     | 31   |

was dann z.B. die folgende `djb2` Hashfunktion ergibt:

```
uint32_t djb2(const char* string, int seed) {
	uint32_t hash = 5381;
	char c;
	while (c = *string++) {
		// ((x << 5) + x) = x * 33
		// mod 2^32 = Overflow
		hash = ((hash << 5) + hash) + c;
	}
	return hash;
}
```

__Es sei aber angemerkt__, dass dies eigentlich nur eine Pre-Hashfunktion
ist. Denn wir wollen ja einen Index in unsere Tabelle, also einen Wert $\in \{0,
..., m - 1\}$.

#### Beweis der Universalitaet

Um zu zeigen, dass diese Stringhashfunktionen universell sind, beobachten wir:
Hashen zwei String(vektoren) $\mathbf{s} = (c_1, ..., c_l), \mathbf{s}' = (c'_1,
..., c'_l)$ auf denselben Wert, so sind also ihre (wie oben definierten)
Polynome gleich:

$$(\sum_{i=1}^l c_i \mathcal{s}^i) \mod p = (\sum_{i=1}^l c'_i \mathcal{s}^i)
\mod p = $$

Zieht man nun das rechte Polynom vom linken ab, ergibt sich:

$$(\sum_{i=1}^l (c_i - c'_i) \mathcal{s}^i) \mod p = 0$$

Dieses Polynom hat also offensichtlich fuer den Seed-Wert $\mathcal{s}$ eine
Nullstelle. Wir wissen nach dem Fundamentalsatz der Algebra, dass es maximal $l$
viele solcher Nullstellen geben kann, weil der Grad der Funktion eben $l$
ist. Auch wissen wir, dass $p$ genau $p$ Restklassen $\{0, ..., p - 1\}$. Somit
ist die Kollisionswahrscheinlichkeit modulo $p$, also die Wahrscheinlichkeit,
dass zwei Werte modulo $p$ gleich sind, wenn sie zufaellig gewaehlt sind,
allgemein $1/p$. Da es hier um die Strings geht duerfen wir annehmen, dass sie
zufaellig sind. Nun haben wir also gefunden, dass es maximal $l$ viele solcher
Seed Werte gibt, sodass dass Polynom Null wird, die Polynome der zwei Strings
also gleich sind. Somit ist Kollisionswahrscheinlichkeit gerade $l/p$.

Das gilt jetzt aber erstmal fuer diese Pre-Hashfunktion. Wir brauchen dann ja im
Weiteren noch eine Funktion, die diesen Stringhash auf eineen Tabellenindex
abbildet. Sei hierfuer $h'$ eine solche, *universelle* Hashfunktion, dann ist
die Kollisionswahrscheinlichkeit insgesamt also $1/m + l/p$, weil wir entweder
in der ersten *oder* in der zweiten Funktion eine Kollision haben koennen, oder
in beiden. Es gilt nun eben, dass wenn $p >> l$, der Bruch $l/p \approx 0$
ist. Dann und nur dann ist eine solche Polynomialhashfunktion also universell
und auch nur dann wenn $h'$ universell ist. Deswegen aber: $p$ sehr gross
waehlen! In der Praxis waehlen wir $p$ sowieso als $2^w$, wo $w$ die
Datentypgroesse ist. Also meist $2^32$ (da wir mit Overflow arbeiten ist dies
Zahl also in der Restklasse einer Primzahl groesser als $2^2$). Diese Zahl ist
wohl sicher groesser als jede Stringlaenge $l$.

## Open-Addressing

Neben dem Hashing mit Chaining gibt es noch eine weitere beliebte Form von
dynamischen Hashtabellen, welche man *offen addressiert* nennt. Im einfachsten
Falle verwenden wir hier die *linear probing* Variante. Moechte man ein Element
in die Hashtabelle einfuegen und es kommt zu einer Kollision, so bewegt man sich
so lange nach rechts (modulo der Tabellengroesse), bis ein freier Platz gefunden
wird. Sucht man nach diesem Element, geht man wieder so lange vom urspruenglich
gehashten Bucket nach rechts, bis man das gewuenschte Element findet. Ist das
Element nicht enthalten, so kommt man auf dem Weg dahin irgendwann zu einem
leeren Eintrag in der Tabelle.

Der Vorteil einer solchen Open-Adressing Variante ist die __erhoehte
Cache-Effizenz__. Da Elemente kontinuierlich im Speicher liegen, erhaelt man
durch prefetching im Prozessor eine hoehere Cache-Hit-Rate als beim Hashing mit
Chaining, wo Listenknoten theoretisch ueberall im Speicher liegen koennten.

Ein relativ grosses Problem bei offener Adressierung ist das Loeschen von
Elementen. Moechte man ein Element $x$ aus der Tabelle entfernen, so kann man
seinen Eintrag in der Tabelle nicht einfach leer machen. Der Grund ist, dass ein
anderes Element $y$, dass danach eingefuegt wurde, moeglicherweise waehrend des
linear probing bei diesem Eintrag vorbeigekommen ist. Da der Eintrag da noch
blockiert war, wurde weiter sondiert und das Element $y$ in einem Eintrag weiter
rechts (modulo) eingefuegt. Loescht man das uebersprungene Element $x$ nun, und
sucht dann nach $y$, so wird man an einem leeren Eintrag vorbeikommen, was
darauf hindeutet, dass $y$ nicht in der Tabelle ist. Die Tabelle waere also
vollkommen invalidiert. Es gibt hierfuer drei moegliche Loesungen:

1. Man verbietet Loeschen grundsaetzlich. Das ist in der Praxis natuerlich keine
   realistische Loesung.
2. Man verwendet Platzhalter, sogenannte *Tombstones*, um zu signifizieren, dass
   Plaetze zwar fuer erneutes Einfuegen frei sind, bei Suchen aber so behandelt
   werden sollen, als ob hier ein Element waere (wobei der Vergleich der
   Schluessel stets zu `false` evaluieren wuerde).
3. Man rehashed alle Elemente rechts von dem Elemente, das man loeschen moechte,
   bis zur naechsten Luecke.

### Sondieren

Allgemein ist die Hashfunktion bei einem offen-addressierten Verfahren so
definiert, dass sie nicht nur den zu hashenden Schluessel nimmt, sondern auch
den Index $i$, der angibt, wie lange schon sondiert wird. Es gilt also dass der
naechste Eintrag fuer ein Element $k$ nach $i$ Versuchen durch $h(k, i)$ gegeben
ist. Innerhalb von $h(k, i)$ wird dann meist (zumindest) eine weitere
Hashfunktion $h'(k)$ verwendet, die die eigentliche Hasharbeit leistet. Es gibt
nun verschiedene Verfahren, wie das oben besprochene lineare Sondieren, wie man
$h(k, i)$ in Abhaengigkeit von $i$ fortschreiten laesst.

#### Lineares Sondieren

Im einfachsten Fall nehmen wir $h(k, i) = h'(k) + i \mod m$. Es wird also einfach
modulo der Tabellengroesse $m$ nach rechts sondiert.

Ein Problem, dass hier auftauchen kann, ist das sogenannte *primary clustering*,
wobei sich "Haeufchen" von Elementen in der Tabelle bilden. Formal tritt primary
clustering auf, wenn zwei Schluessel $k_1, k_2, h'(k_1) \neq h'(k_2)$ ab einem
bestimmten Index die selbe Sondiersequenz haben: $\exists i_1, i_2: \forall j:
h(k_1, i_1 + j) = h(k_2, i_2 + j)$. Hier kann man auch sagen, dass $\exists j:
i_1 = i_2 + j$.

Das groesste Problem bei primary Clustering ist, dass die Cluster *immer
groesser* werden. Sei naemlich $x$ die Groesse eines Clusters. Unter der Annahme
uniformen Hashings ist die Wahrscheinlichkeit, dass ein neues Element diesen
Cluster vergroessert, genau $(x + 1)/m$. Wird naemlich gegen irgendeines der $x$
Elemente gehashed, so wird das Element nach dem linearen Sondieren ans Ende des
Clusters geklebt. Somit also $x/m$. Das letzte $1/m$ ergibt sich dann noch, dass
der Cluster auch waechst, wenn man direkt an das Ende des Clusters
hashed. Besonders schmerzhaft ist es dabei, wenn man genau in die Luecke zweier
Cluster hashed und diese dann verschmelzt.

Damit lineares Sondieren gut funktioniert, sollte die Tabellengroesse $m$ viel
groesser als $n$ gewaehlt werden. In der Praxis ist $2n$ eine gute Wahl. Hierbei
ist mit $n$ die maximale zu erwartende Anzahl an Elementen gemeint (oder bei
einer dynamischen Tabelle der Schwellenwert, nach welchem man resized). Donald
Knuth hat dann gezeigt, dass wenn $m = 2n$ die durchschnittlich zu erwartende
Sondierzeit $3/2$ betraegt. Man muss also im Schnitt nur $1.5$ mal sondieren!
Fuer ein volles Array ist die Laufzeit hingegen:

$$\approx = \sqrt{\frac{\pi m}{8}} = \text{ Brainfuck }$$

#### Quadratic Probing

Beim *Quadratic Probing* waehlen wir zwei relle Konstanten $c_1, c_2$ und
definieren die Hashfunktion dann durch:

$$h(k, i) = h'(k) + c_1 i + c_2 i^2 \mod m .$$

Wir hashen also zunaechst gegen einen Index $h'(k)$ und sondieren dann linear
mit $c_1$ und quadratisch mit $c_2$. Alternativ ist auch die folgende Definition
moeglich:

$$h(k, i) = h'(k) - (-1)^i \cdot (\left\lceil frac{i}{2} \right\rceil)^2$$

Hierbei springt $h(k, i)$ jeweils immer links und rechts von $h'(k)$. Das ganze
passiert immer symmetrisch, da wir immer zwei Werte haben, die gleich $\lceil
i/2 \rceil$ sind. Einmal den ganzzahlen Wert $x$ und einmal $x - 1/2$. Hierbei
ist dann aber fuer diesen gleichen Wert einmal das Vorzeichen positiv und einmal
negativ.

Man bemerke, dass Quadratic Probing noch immer Clustering verursachen kann. Wenn
$h(k_1) = h(k_2)$ ist die Sondiersequenz naemlich gleich, da $c_1, c_2$ konstant
sind. Jedoch gilt dies nicht mehr fuer Schluessel die nicht denselben Hashwert
haben. Bei quadratischen Funktionen, die einen verschiedenen Abstand vom
Ursprung haben (was $h(k)$ ja ist) ist es nun naemlich nicht mehr so, dass sie
ab einem bestimmten $i$ fuer alle nachfolgenden $j > i$ auch gleich sein
werden. Quadratic Probing loest das Clustering Problem jedenfalls nicht, es
macht es nur weniger schlimm.

#### Double Hashing

Eine dritte Variante ist das sogenannte *double hashing*, wobei wir zwei
Hashfunktionen $h_1(k), h_2(k)$ wahlen, und $h(k, i)$ dann so definieren:

$$h(k, i) = h_1(k) + i \cdot h_2(k) .$$

Um sicherzustellen, dass wir durch diese Funktion alle moeglichen Eintraege in
der Tabelle irgendwann erreichen, muessen wir eine der folgenden beiden
Einschraenkungen befolgen:

1. Wir waehlen $m$ prim, dann muss $h_2$ eine Zahl kleiner als $m$ zurueckgeben.
2. Wir waehlen $m$ als *Zweierpotenz* und lassen $h_2$ auf eine ungerade Zahl
   abbilden.

Jedenfalls muss $h_2(k)$ teilefremd zu $m$ sein. Die zweite Variante ist
natuerlich in der Praxis viel sinnvoller, da man meist mit verdoppelnden Arrays
arbeitet. Dann kann man $h_2$ durch eine andere Hashfunktion $h'$ definieren:

$$
h_2(k) =
begin{cases}
	h'(k), \text{ wenn } h'(k) \text{ ungerade }\\
	h'(k) + , \text{ wenn } h'(k) \text{ gerade }
\end{cases}
$$

oder in einer Programmiersprache:

```
value = h(k)
return value % 2 ? value : value + 1
```

Es sei angemerkt, dass durch double hashing primaere oder sekundaere Haefung
nicht ausgeschlossen wird. Gilt fuer zwei Schlussel $k_1, k_2$, dass $h_1(k_1)
\neq h_1(k_2)$, aber $h_2(k_1) = h_2(k_2)$, so koennen wir wieder primaere
Haeufung erhalten (nicht immer, haengt von den konkreten Werten von $h_1(k_1),
h_1(k_2)$ ab). Sekundaere Haeufung gibt es immer dann, wenn $h_1(k_1) = h_2(k_2)
\land h_2(k_1) = h_2(k_2)$. Hierbei gilt aber eben, dass die Wahrscheinlichkeit
dass es eine Kollision bei *beiden* Hashfunktionen gibt, sehr gering ist
(z.B. $1/m^2$ bei universellen Hashfunktionen).

## Perfektes Hashing

Wir haben bisher Hashverfahren betrachtet, die eine konstante Laufzeit fuer
Suchoperationen haben. Diese Laufzeit ist aber nur die *erwartete*, also
durchschnittliche Laufzeit. Es ist aber auch moeglich, eine Hashtabelle so zu
konstruieren, dass sie konstante Laufzeit im *schlimmsten* Fall, also *worst
case* hat. Das nennt man dann *perfektes* Hashing. Dies ist aber nur dann
moeglich, wenn wir eine *statische* Menge $\mathbb{K}$ von Schluesseln haben,
deren Kardinalitaet also konstant ist.

Hierzu betrachten wir also ein solche Menge $\mathbb{K} = \{k_1, ..., k_n\}$. Im
weiteren sei $\mathbb{H}_m$ eine $c$-universelle Familie von Hashfunktionen auf
$m$ Elemente. Auch notieren wir durch $C(h)$ die Anzahl an Kollisionen einer
Hashfunktion $h \in \mathbb{H}_m$. Diese Anzahl $C(h)$ berechnet sich aus:

$$C(h) = \sum_{i=1}^m l_i \cdot (l_i - 1)$$

Hierbei ist $l_i$ die Liste der jeweils $i$-ten Liste. Denn fuer jede Liste mit
$l$ Elementen gibt es fuer eine Kollision zuerst $l$ Moeglichkeiten, den ersten
Kollisionspartner zu waehlen und dann nur mehr $l - 1$ Moeglichkeiten fuer den
zweiten Kollisionspartner.

Eine erste Beobachtung, die wir aufstellen koennen, ist das der Erwartungswert
$\mathbb{E}[C(h)]$ fuer die Anzahl an Kollisionen durch $cn(n-1)/m$ nach oben
beschraenkt ist. Das ist intuitiv verstaendlich, weil wir $n$ Schluessel
haben. Fuer jeden der $n$ Schluessel gibt es $n - 1$ moegliche
Kollisionspartner, welche jeweils mit einer Wahrscheinlichkeit von $c/m$
kollidieren. Somit ergibt sich fuer den Erwartungswert der Kollisionen fuer
einen der $n$ Schluessel gerade $(n-1) c/m$ und fuer alle $n$ Schluessel dann
$n(n-1)c/m$. Auch Formal:

Sei $X_{i,j}(h)$ die Zufallsvariable, die den Wert $1$ annimmt, wenn Schluessel
$k_i, k_j$ mit $i \neq j$ kollidieren ($h(k_i) = h(k_j)$). Dann ergibt sich der
Erwartungswert in der Anzahl an Kollisionen aus der Sume ueber alle $X_{i,j}$:

$$
\begin{align}
	\mathbb{E}[C] &= \mathbb{E}\left[ \sum_{i \neq j} X_{i,j} \right]\\
		          &= \sum_{i \neq j} \mathbb{E}[X_{i,j}]\\
				  &= \sum_{i \neq j} c/m\\
				  &\leq n(n - 1) c/m
\end{align}
$$

Was folgern wir daraus? Nun, wuerden wir $m$ quadratisch in der Anzahl an
Elementen $n$ waehlen, also z.B. $m = n(n - 1)$, so waere der Erwartungswert
fuer die Anzahl an Kollisionen konstant, da $\mathbb{E}[C] = n(n - 1) c/m = c$
waere.

Fuer die weitere Analyse muessen wir nun die sogenannte *Markov-Ungleichung*
verwenden. Sei hierzu $X$ eine Zufallsvariable. Dann sagt die
Markov-Ungleichung, dass die Wahrscheinlichkeit, dass $X \geq k$ ist, durch
$\mathbb{E}[X]/k$ nach oben beschraenkt ist. Sei hierfuer nun $E$ eines der
Ereignisse, das durch $X$ beschrieben wird. Dann sei $I_E$ die Indikatovariable
(binaere Zufallsvariable) fuer das Auftreten dieses Ereignisses. Sei dann noch
$a$ ein Wert der Zufallsvariable. Mit der Notation $I_{X \geq a}$ druecken wir
das Ereignis auf, dass $X$ einen Wert groesser oder gleich $a$ annimmt. Es gilt
also:

$$
I_{X \geq a} =
\begin{cases}
	1, \text{ wenn } X \geq a,\\
	0 \text { wenn } X < a
\end{cases}
$$

Nun koennen wir sagen, dass $aI_{X \geq a} \leq X$ sein muss. Denn wenn $X < a$
ist, so ist $I_{X \geq a} = 0$ und somit $0 \leq X$. Ist aber $X \geq a$, so ist
ergibt sich $aI_{X \geq a} = a \leq X$ weil $X \geq a \iff a \leq X$. Nun
duerfen wir auf beiden Seiten den Erwartungswert nehmen, weil dieser monoton
ansteigend ist. Dadurch ergibt sich:

$$
\begin{align}
	\mathbb{E}[aI_{X \geq a}] &\leq \mathbb{E}[X]\\
	a \mathbb{E}{I_{X \geq a}} &\leq \mathbb{E}[X]\\
	a \Pr[X \geq a] &\leq \mathbb{E}[X]\\
	\Pr[X \geq a] &\leq \frac{\mathbb{E}[X]}{a}
\end{align}
$$

wodurch die Aussage also folgt. Setzen wir nun $k' := k \cdot \mathbb{E}[X]$,
dann haben wir:

$$
\begin{align}
	\Pr[X \geq k'] &\leq \frac{\mathbb{E}[X]}{k'}\\
    \Pr[X \geq k \cdot \mathbb{E}[X]] &\leq \frac{\mathbb{E}[X]}{k \cdot
	\mathbb{E}[X]}\\
	\Pr[X \geq k \cdot \mathbb{E}[X]] &\leq \frac{1}{k}
\end{align}
$$

Was gewinnen wir nun daraus? Nun, wir koennen jetzt folgendes Lemma beweisen:
Fuer mindestens die Haelfte der Funktionen $h \in \mathbb{H}_m$ gilt:

$$C(h) \leq 2cn(n - 1)/m .$$

Wir wissen naemlich: der Erwartungswert fuer Anzahl an Kollisionen ist allgemein
durch $cn(n-1)/m$ nach oben beschraenkt. Auch folgt nun aus der
Markov-Ungleichung:

$$\Pr[C(h) \geq 2cn(n-1)/m] \leq \Pr[C(h) \geq 2 \cdot \mathbb{E}[C(h)]] \leq
\frac{1}{2}$$

Somit haben wir also, dass fuer hoechstens die Haelfte aller Funktionen (weil
die Wahrscheinlichkeit $\leq 1/2$) ist, die Anzahl an Kollisionen $\geq
2cn(n-1)/m$ ist. Folglich ist das Gegenteil fuer mindestens die Haelfte wahr.

Wenn wir nun fuer deise halben Hashfunktionen $m \geq cn(n-1) + 1$ waehlen, dann
erhalten wir:

$$C(h) \leq \frac{2cn(n-1)}{cn(n-1) + 1}.$$

Dieser Wert ist sicher $< 2$. Es gilt also, dass fuer die Haelfte der
Hashfunktionen $h \in \mathbb{H}_m$ die Anzahl an Kollisionen $C(h) < 2$ ist. Da
Kollisionen symmetrisch sind, muss $C(h)$ immer gerade sein. Die einzige Zahl,
die kleiner als Zwei und gerade ist, ist Null. Es folgt also bei einer solchen
Konfiguration direkt, dass $C(h) = 0$. Es gaebe also gar keine Kollisionen. Wir
koennen nun also einfach zufaellig Hashfunktionen aus unserer Familie
$\mathbb{H}_m$ waehlen und pruefen, ob es eine Kollision gibt. Wenn ja, waehlen
wir einfach eine weitere Hashfunktion aus der Familie. Nach durschnittlich zwei
Versuchen sollten wir eine erfolgreich sein, da die obige Eigenschaft fuer
mindestens die Haelfte der Funktionen gilt.

### Statisches Woerterbuch

Wir koennen unser Wissen ueber perfektes Hashing nun nutzen, um ein perfektes
statisches Woerterbuch zu kreeiren. Am einfachsten koennten wir natuerlich
einfach eine quadratische Tabellegroesse waehlen, wie oben beschrieben. In der
Praxis ist das aber nicht so geschmeidig. Unser Ziel ist, so nah wie moeglich an
einer linearen Groesse zu bleiben.

Dieses Ziel koennen wir erreichen, indem wir die Schluessel *zweistufig*
abbilden. In der ersten Stufe werden Schluessel mit einer Hashfunktion, die nur
wenige Kollisionen erzeugen soll, auf Buckets abgebildet. In der zweiten Stufe
bilden wir dann mit einer perfekten Hashfunktion innerhalb dieses Buckets
ab. Jeder Bucket wird dann also quadratisch viel Platz benoetigen. Hierfuer
betrachten wir Buckets $\mathbb{B}_l^h$, wo $h$ die Hashfunktion der ersten Stufe ist und
$l$ der Index des Buckets in der ersten Stufe. $\mathbb{B}_l^h \subseteq
\mathbb{K}$ ist also eine Menge, die gerade Elemente enthaelt, die in den
$l$-ten Bucket abgebildet werden. Im weiteren sei dann $b_l^h$ die Kardinalitaet
des Buckets $\mathbb{B}_l^h$.

Innerhalb eines Buckets haben wir bekanntlich $b_l^h (b_l^h - 1)$
Kollisionen. Somit ergibt sich die Gesamtanzahl $C(h)$ von Kollisionen in der
Hashtabelle aus der Summe dieser Zahlen:

$$C(h) = \sum_{l = 0}^{m - 1} b_l^h (b_l^h - 1)$$

In der ersten Stufe wollen wir nur linear viel Speicher verwenden. Wir suchen
also eine Kontante $\alpha$, sodass die Tabellengroesse $\alpha n$ ist. Hierfuer
waehlen wir nun eine Hashfunktion $h \in \mathbb{H}_{\alpha n}$, fuer welche
gilt:

$$C(h) \leq \frac{2 cn (n - 1)}{\alpha n} \leq \frac{2cn}{\alpha}$$

Wir wissen, dass diese Eigenschaft mindestens fuer die Haelfte der
Hashfunktionen einer beliebigen Familie gilt. Man erwartet also nur maximal zwei
Versuche, um eine solche Funktion zu finden.

In der zweiten Stufe wollen wir nun fuer jeden Bucket quadratisch viel Platz
verwenden, um keine Kollisionen zu erhalten. Hierzu setzen wir die Groesse $m_l$
des $l$-ten Buckets also gleich $cb_l^h(b_l^h - 1) + 1$, wobei wir oben gezeigt
haben, dass es dann keine Kollisionen geben kann, sofern wir die Hashfunktion
$h_l$ passend aus $H_{m_l}$ waehlen. D.h. wir legen $m_l$ so fest und suchen so
lange eine Hashfunktion, bis sie keine Kollisionen erzeugt. Das sollte wieder
fuer die Haelfte aller Funktionen in $H_{m_l}$ gelten.

Wenn wir nun diese Tabellen $B_l^h$ hintereinanderreihen, erhalten wir eine
Hashtabelle der Groesse $\sum_l m_l$. Hierbei beginnt die $l$-te Tabelle an
Index $\sum_{i=0}^l m_i$, sodass wir diesen Index in einem separaten Array
einfach speichern koennen. Dann definieren wir unsere Hashfunktion $\mathcal{h}$
fuer diese Tabelle durch:

$$\mathcal{h}(k) = s[h(k)] + h_l(h(k))$$

Hierbei ist $h(k)$ also die Abbildung erster Stufe und $s$ das Array, das die
Startindizes der Tabellen speichert. $h_l$ ist dann die gewaehlte Hashfunktion
zweiter Stufe, die letzendlich in den Bucket abbildet.

Es bleibt nur noch, die Konstante $\alpha$ zu waehlen, um unsere Tabellengroesse
festzulegen. Man kann herausfinden (ohne Beweis), dass $\alpha$ mit $2\sqrt{2}c$
eine gute Wahl ist. Dann ergibt sich also eine Tabellengreosse von $\alpha n =
2\sqrt{2}cn$, was aber linear in $n$ ist. Wir haben also eine *perfekte*
(kollisionsfreie!) Hashtabelle mit linear viel Platz. Ist $c = 1$, betrachten
wir also nur universelle Familien von Hashfunktionen, ergibt sich somit eine
Tabellengroesse von nur $\lceil 2\sqrt{2}\rceil n = 3n$! Wir muessen lediglich
geeignete Hashfunktionen finden. Wir finden aber immer solche Hashfunktionen,
weil es fuer jedes $m$ universelle Familien gibt ($(ax + b) \mod p \mod
m$). Dieser Algorithmus ist also eine Instanz eines *Las-Vegas* Algorithmus. Ein
solcher Algorithmus liefert immer das korrekte Ergebnis, wobei die genaue Zeit
dafuer unbekannt ist. Es gambled also nur mit der Zeit, nicht mit der
Korrektheit. Das steht im Kontrast zu einem Monte-Carlo Algorithmus, der
deterministische Zeit benoetigt aber nicht notwendigerweise das korrekte
Ergebnis liefert.
