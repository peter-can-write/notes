# Beweise

Ein Beweis ist eine korrekte und vollstaendige (lueckenlose) Argumentation, aus
der sich unbestreitbar die Wahrheit einer Aussage folgern laesst. Hierbei
schuetzt uns Korrektheit davor, Fehler zu machen. Vollstaendigkeit ermoeglicht
es jedem, das Resultat zu verifizieren.

Fuer Beweise verwenden wir bestimmte Begriffe:

* *Axiome, Praemissen, Postulate, Hypothesen, Annahmen*: Aussagen, die man als
  wahr annimmt.

* *Theorem, Satz*: Eine Aussage, die aus den Axiomen folgt.

* *Beweis*: Die Argumentation, dass der Satz tatsaechlich aus den Axiomen folgt.

* *Lemma*: Ein Hilfssatz (Theorem) im Beweis eines wichtigen Theorems.

* *Korrolar*: Ein weniger bedeutendes Theorem, das leicht als Konsequenz eines
  wichtigen Teheorems bewiesen werden kann.

## Ziele

Das Ziel eines Beweises hat immer eine logische Gestalt. Diese Gestalt
suggeriert, wie man vorgehen soll. Ist die Gestalt:

* $\forall x \in A : F$, dann braucht man ein *beliebieges* $a \in A$
  und setzt $F[x\ a]$ als Ziel.

* $\exists x \in A : F$, dann genuegt ein *geeignetes* $a \in A$, fuer
  welches man $F[x\ a]$ als Ziel setzt.

* $F \rightarrow G$, dann fuege $F$ zu den Annahmen hinzu und setze $G$ als
  neues Ziel.

* $F \leftrightarrow G$, dann setze $F \rightarrow G$ und $G \rightarrow F$ als
  neue Ziele.

* $F \lor G$, dann zeige dass wenn $F$ nicht gilt, $G$ gelten muss, oder
  umgekehrt.

## Methoden

Besonders bei Beweisen der Form "wenn $F$ wahr ist, dann ist auch $G$" wahr ($F
\rightarrow G$), hat man mehrere Moeglichkeiten, den Beweis durchzufuehren:

* Direkter Beweis
* Indirekter Beweis
* Widerspruchsbeweis

### Direkter Beweis

Im direkten Beweis nehemen wir die Praemisse $F$ in unsere Annahmen, und zeigen
dann, dass $G$ wahr ist. Im Kalkuel entspricht dies der
Implikationseinfuehrungsregel:

$\frac{A, F \vdash G}{A \vdash F \rightarrow G}$

### Indirekter Beweis

Beim indirekten Beweis verwenden wir die Aequivalenz  $(F \rightarrow G) \equiv
(\neg G \rightarrow \neg F)$. Also um zu beweisen, dass $F \rightarrow G$ gilt,
nimm an, dass $\neg G$ gilt und zeige, dass daraus nur $\neg F$ folgen
kann. Wenn das gilt, gilt, dass wenn $F$ wahr ist, $G$ wahr sein muss.

$\frac{A,\neg G \vdash \neg F}{A \vdash F \rightarrow G}$

### Widerspruchsbeweis

Im Widerspruchsbeweis (reductio ad absurdum) wird eine Aussage $F$ bewiesen,
indem wir die Negationseinfuehrung und die Regel fuer $false$ verwenden. Wir
nehmen also an, dass $F$ falsch ist, nehmen $\neg F$ also in unsere
Annahmen. Dann zeigen wir, dass daraus nur $false$ folgen kann, also eine
Aussage wie auch ihre Negation (ein Widerspruch).

$\frac{A, \neg F \vdash false}{A \vdash F}$

Oder die Aequivalenz: $F \equiv \neg F \rightarrow false$

Die Regel fuer $false$ erlaubt es widerum zu sagen, dass wenn aus den Annahmen
(mit $\neg F$) $false$ folgt, dass dann eine Aussage sowie ihre Negation folgt:

$\frac{A, \neg F \vdash G \,\, A, \neg F \vdash \neg G}{A, \neg F \vdash false}$

## Vollstaendige Induktion

Das Mittel der Vollstaendigen Induktion ist eine Methode, um eine Eigenschaft
fuer alle natuerlichen Zahlen zu zeigen, also um zu zeigen:

$\forall n \in \mathbb{N}: P(n)$

wo $P$ ein Praedikat fuer die zu zeigende Eigenschaft ist.

Die Methodik der vollstaendigen Induktion folgt diesem Schema:

1. Induktionsbasis/-verankerung. Man zeigt zuerst, dass die Aussage fuer das
   kleinste Element der Menge gilt. Das bedeutet, $P(1)$ fuer $\mathbb{N}$ oder
   $P(0)$ fuer $\mathbb{N_0}$ zu zeigen. "Sei $n = 0$, dann gilt ..."

2. Induktionsschritt: Sei $n \in \mathbb{N}$ beliebig, aber fest.

   1. Annahme: Die Aussage $P$ gilt fuer $n$.

   2. Behauptung: Dann (wenn die Aussage fuer $n$ gilt) gilt die Aussage $P$
                  auch fuer $n + 1$.

   3. Beweis: Zeige die Behauptung. Hierbei muss man den Ausdruck, den man durch
              Einsetzen von $n + 1$ erhaelt, so lange "kneten", bis der Ausdruck
              fuer $n$ darin vorkommt, denn dieser ist ja schon bewiesen.

In Praedikatenlogik ausgedrueckt:

$(P(1) \land \forall n \in \mathbb{N}: P(n) \rightarrow P(n + 1)) \rightarrow
\forall n \in \mathbb{N}: P(n)$

Allgemein muss die Basis aber nicht $1$ sein, wenn man die Aussage nur fuer
Zahlen ab einem bestimmten $x$ zeigen will. Dann lautet das Prinzip der
vollstaendigen Induktion:

$(P(x) \land \forall n \in \mathbb{M}: n \geq x \land P(n) \rightarrow P(n + 1))
\rightarrow \forall n \in \mathbb{N}: n \geq x \rightarrow P(n)$

## Wohlfundierte Induktion

Das Prinzip der wohlfundierten Induktion ist eine Verallgemeinerung der
vollstaendigen Induktion. Fuer die vollstaendige Induktion hatten wir, um eine
Aussage fuer $n + 1$ zu zeigen, die Aussage nur fuer $n$ angenommen. Bei der
wohlfundierten Induktion nehmen wir die Aussage fuer *alle* $m$ an, die
bezueglich einer Relation "links" von $n$ stehen. Fuer die natuerlichen Zahlen
und der "kleiner"-Relation $<$ nehmen wir also an, die Aussage gilt fuer alle
Zahlen $m < n$, und zeigen dadurch die Aussage fuer $n$ (nicht mehr
bzw. indirekt fuer $n + 1$).

Wohlfundierte Induktion kann man nur mit *wohlfundierten* Relationen
durchfuehren. Eine *wohlfundierte* Relation ist dabei eine, wo kein Element eine
unendliche Anzahl von Vorgaengern bezueglich der Relation hat. Es gibt also
insbesondere keine Zyklen, wodurch eine wohlfundierte Relation auch stets
irreflexiv ist. Nur dadurch ist es auch moeglich, die Aussage fuer alle
Vorgaenger bezueglich der Relation anzunehmen, um die Aussage fuer ein Element
zu zeigen. Also:

1. Die Menge der Vorgaenger eines Elements $a$ ist endlich: $|\{x \, | \, (x, a)
  \in R\}| < \infty$.

2. Die Relation enthaelt keine Zyklen: $\nexists a: (a, a) \in R^+$. Diese
   Relation enthaelt z.B. einen Zyklus, weil man in von $a$ wieder zu $a$ kommt
   ueber Transitivitaet (deswegen ist $(a, a)$ in der transitiven Huelle $R^+$):
   $R = \{(a, b), (b, c), (c, a)\}$

3. Aus (3) folgt: Die Relation ist irreflexiv. Sonst muesste man, um die Aussage
  fuer $a$ zu zeigen, die Aussage fuer $a$ zeigen.

In Praedikatenlogik laesst sich die wohlfundierte Induktion fuer eine beliebige
wohlfundierte Relation $\prec$ nun so beschreiben:

$\forall a \in M: P(a) \Leftrightarrow P(a_0) \land \forall a: (\forall b \in M,
b \prec a: P(b)) \rightarrow P(a)$

In Worten: "Um zu zeigen, dass jedes $a \in M$ die Eigenschaft $P$ besitzt,
zeige fuer jedes $a$: Wenn ein minimales Element $a_0$ die Eigenschaft besitzt,
und alle Vorgaenger $b \in M$ bezueglich der Relation $\prec$ die Eigenschaft
besitzen, dann besitzt auch $a$ die Eigenschaft".

Die Basis der wohlfundierten Induktion ist es dann, die Aussage fuer jenes
Element $a \in M$ zu zeigen, das keine Vorgaenger hat. Wo also die Menge $\{b
\in M \,|\, b \prec a\}$ die leere Menge ist. Man zeigt die Aussage also fuer
ein *minimales* Element, oder wenn die Ordnung bzw. die Relation total ist, fuer
das *kleinste* Element (in diesem Fall gilt sowieso "kleinste" $=$
"minimales"). Ist die wohlfundierte Relation die "kleiner"-Relation $<$ und $M$
die Menge der natuerlichen Zahlen, dann waere dieses kleinste Element eben $1$.

Wie ergibt sich dann die vollstaendige Induktion als Spezialfall der
wohlfundierten Induktion? Indem die Relation einfach als die Vorgaengerrelation
gewaehlt wird. Die Relation ist dann einfach $\{(n, n + 1) \, | \, n \in
\mathbb{N}\}$. Um die wohlfundierte Induktion dann durchzufuehren, nimmt man an,
die Aussage gilt fuer alle Vorgaenger eines beliebigen $n$ bezueglich der
Relation (die hier als die Vorgaengerrelation gewaehlt ist). Da ein $n$ bei
dieser Relation nur einen Vorgaenger hat, naemlich $n - 1$, nimmt man die
Aussage also nur fuer $n - 1$ an, um sie fuer $n$ zu zeigen (bzw. man nimmt sie
fuer $n$ an um sie fuer $n + 1$) zu zeigen.

## Beweisrezepte

Hier nun Standardverfahren zum mittels vollstaendiger Induktion Behauptungen
ueber natuerliche Zahlen zu beweisen.

### Summen und Produkte

Aussagen ueber Summen $\sum_{k=n_0}^n a_k$ und Produkte $\prod_{k=n_0} a_k$ kann
man oft sehr leicht mittels vollstaendiger Induktion loesen. Meist ist eine
Summe oder ein Produkt angegeben, und ein weiterer Ausdruck, zu dem die Summe
oder das Produkt gleich sein soll. Dann soll die Gleichheit fuer alle
natuerlichen Zahlen (mit/ohne Null) gezeigt werden.

Allgemein versucht man bei der vollstaendigen Induktion immer, den Ausdruck fuer
$n + 1$ auf den fuer $n$ zu reduzieren, welcher laut Induktionsannahme schon
wahr ist. Dann kann man die Summe oder das Produkt durch das Ziel ersetzen und
so den Ausdruck umformen.

Hierbei kann man normalerweise einem Standardrezept folgen. Die Aufgabe ist
dabei meistens der Form (hier fuer $\mathbb{N}$ ohne untere Schranke):

Zeige: $\sum_{k=1}^n a_k = f(n), \forall n \in \mathbb{N}$

Hierbei ist $a_k$ irgendein Ausdruck in Abhaengigkeit von $k$ und $f(n)$ die
Zielfunktion.

Dann:

I. Basis: Zeige die Aussage fuer $1$. Setze dabei $1$ in die Summe sowie
$f(n)$ ein, und forme sie zu demselben Wert um.

II. Schritt: Sei $n \in \mathbb{N}$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer $n$.

	Behauptung: Dann gilt sie auch fuer $n + 1$.

	Ziel: $\sum_{k=1}^{n + 1} a_k = f(n + 1)$

Beweis:

1. Ziehe den letzten Summanden aus der Summe raus, um somit die Summe fuer $n$
   zu erhalten, fuer welche die Aussage schon gilt:

   $\sum_{k=1}^{n + 1} a_k = \sum_{k=1}^n a_k + a_{n+1}$

2. Laut Induktionsannahme ist die Summe $\sum_{k=1}^n a_k$ gleich $f(n)$. Also
   setze $f(n)$ ein:

   $\sum_{k=1}^n a_k + a_{n+1} \xrightarrow{\text{I.A.}} f(n) + a_{n+1}$

3. Forme nun $f(n) = a_{n+1}$ zu $f(n + 1)$ um.

4. Male kleines Kaestchen hin: $\square$ (alternativ: schreibe "q.e.d.")

#### Beispiel:

Zeige: $\sum_{k=1}^n (2k - 1) = n^2 \, \forall n \in \mathbb{N}$

I. Basis: Sei $n = 1$, dann gilt:

	1. $\sum_{k=1}^1 (2k - 1) = 2 \cdot 1 - 1 = 1$
	2. $1^2 = 1$

II. Schritt: Sei $n \in \mathbb{N}$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer $n$.

	Behauptung: Dann gilt sie auch fuer $n + 1$.

	Ziel: $\sum_{k=1}^{n + 1} (2k - 1) = (n + 1)^2$

Beweis:

1. Ziehe den letzten Summanden aus der Summe raus:

$\sum_{k=1}^{n + 1} (2k - 1) = \sum_{k=1}^n (2k - 1) + 2(n + 1) - 1$

2. Laut Induktionsannahme ist $\sum_{k=1}^n (2k - 1) = n^2$, also setzen wir es
   ein:

$\sum_{k=1}^n (2k - 1) + 2(n + 1) - 1 = n^2 + 2(n + 1) - 1$

3. Nun formen wir den Ausdruck um, sodass er die Aussage fuer $n + 1$ erfuellt
   (siehe "Ziel"):

$n^2 + 2(n + 1) - 1 = n^2 + 2n + 2 - 1 = n^2 + 2n + 1 = (n + 1)^2$.

$\square$

### Teilbarkeitsaussagen

Weiters wird oft gefragt, ob ein bestimmter Ausdruck durch eine Zahl teilbar
ist. Eine Aussage wie $5 | 5 \cdot x^2 \, \forall n \in \mathbb{N}$ laesst sich
hierbei gut mittels Induktion loesen. In der Induktionsbasis zeigt man die
Aussage wieder fuer das kleinste Element. Im Schritt formt man den Ausdruck fuer
$n + 1$ zu dem fuer $n$ um. Wichtig ist es dann, dass wenn eine Zahl $y$ durch
eine andere $x$ teilbar ist, diese einen gemeinsamen Faktor $k$ haben:

$x | y :\iff \exists k \in \mathbb{Z}: y = k \cdot x$

Das bedeutet, dass wir den Ausdruck fuer $n$, wenn wir ihn aus dem fuer $n + 1$
rauskneten, durch $x \cdot k$ ersetzen duerfen. Dann koennen wir die gesamte
Rechnung so umformen, dass wir ein $k'$ in Abhaengigkeit von $n$ und $k$
erhalten, welches eben der Faktor fuer $n + 1$ ist.

Allgemeines Schema (heir fuer $\mathbb{N}_0$):

Zeige: $x \,|\, f(n) \forall n \in \mathbb{N}_0$.

I. Basis: Setze $0$ in $f(n)$ ein, und erhalte einen Wert, wo man meist leicht
sieht, dass er durch $x$ teilbar ist (z.B. $x$ selbst).

II. Schritt: Sei $n \in \mathbb{N}_0$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer $n$, somit auch:
	         $\exists k \in \mathbb{Z}: x = k \cdot n$

	Behauptung: Dann gilt die Aussage auch fuer $n + 1$.

	Ziel: $\exists k' \in \mathbb{Z}: f(n + 1) = k' \cdot x$

Beweis:

	1. Forme $f(n + 1)$ zu $f(n)$ um, damit die Induktionsannahme gilt. Man
       erhaelt dann also einen Ausdruck $f(n) + g(n)$ wo $g(n)$ irgendein
       Ausdruck ist, der beim Umformen von $f(n + 1)$ uebrig bleibt.

	2. Nun gilt es zu zeigen, dass $g(n)$ auch durch $x$ teilbar ist.
	   Forme $g(n)$ also zu einem Ausdruck $x \cdot h(n)$ um.

	3. Da die Induktionsannahme fuer $f(n)$ gilt, kann man $f(n)$ durch $k \cdot
       x$ ersetzen. Da man in (2) auch eine Form fuer $g(n)$ gefunden hat,
       sodass der Faktor $x$ davor steht, kann man $x$ herausheben.

	4. $k'$ ist dann der andere Faktor von $x$, der beim rausheben uebrig bleibt.

Sehr oft ist es hierbei nuetzlich oder notig, Koeffizienten umzuformen. Hat man
z.B. einen Ausdruck $7x + 1$, aber die Aussage gilt nur fuer $3x + 1$, dann muss
man umformen:

$7x + 1 = (4 + 3)x + 1 = 4x + 3x + 1$

Dann kann man $3x + 1$ laut Induktionsannahme ersetzen.

#### Beispiel

Zeige: $19 | 5 \cdot 2^{3n + 1} + 3^{3n + 2}\, \forall n \in \mathbb{N}_0$

I. Basis: Sei $n = 0$, dann gilt:

   $5 \cdot 2^{3 \cdot 0 + 1} + 3^{3 \cdot 0 + 2} = 5 \cdot 2 + 3^3 = 19$

   $19 | 19$.

II. Schritt: Sei $n \in \mathbb{N}_0$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer $n$, sodass auch gilt:
		     $\exists k \in \mathbb{Z}: n = k \cdot 19$

    Behauptung: Dann gilt die Aussage auch fuer $n + 1$.

	Ziel: $\exists k' \in \mathbb{Z}: 5 \cdot 2^{3(n + 1) + 1} + 3^{3(n + 1) +
    2} = k' \cdot 19$

Beweis:

	1. Multipliziere die Potenzen aus (kommutiere sie ein wenig):

	$5 \cdot 2^{3n + 1 + 3} + 3^{3n + 2 + 3}$

	2. Expandiere die Terme, um die selben Potenzen wie fuer $n$ zu erhalten:

	$5 \cdot 2^{3n + 1} \cdot 2^3 + 3^{3n + 2} \cdot 3^3$
	$8 \cdot 5 \cdot 2^{3n + 1} + 27 \cdot 3^{3n + 2}$

	3. Schreibe den Koeffizenten $27$ um, sodass man den Ausdruck fuer $n$ sogar
       mehrmals erhaelt ($8$ mal):

	$8 \cdot 5 \cdot 2^{3n + 1} + (19 + 8) \cdot 3^{3n + 2}$
	$8 \cdot (5 \cdot 2^{3n + 1} + 3^{3n + 2}) + 19 \cdot 3^{3n + 2}$

	4. Laut Induktionannahme ist der Ausdruck in Klammern durch $19$ teilbar,
       also durch $19 \cdot k$ ersetzbar. Als Nebeneffekt von (3) ist der Rest
       des Umformens auch schon eindeutig durch $19$ teilbar. Man koennte jetzt
       aufhoeren, aber es ist schoener, noch das $k'$ fuer $n + 1$ zu bestimmen:

	$8 \cdot (19 \cdot k) + 19 \cdot 3^{3n + 2}$
	$19 (8k + 3^{3n + 2})$

	5. Also gilt: $k' = 8k + 3^{3n + 2}$. $\square$.

### Rekursionsgleichungen

Rekursionsgleichungen sind solche, die durch sich selbst definiert sind. Sei $f:
\mathbb{N}_0 \rightarrow \mathbb{R}$ eine Funktion. Dann ist eine
Rekursionsgleichung vom Grad $d$ fuer $f$ eine Gleichung fuer $f(n + 1)$, die
$d$ mal $f$ mit niedrigeren Parametern als $n + 1$ aufruft. Beispiel:

$f(n + 1) = f(n) + f(n - 1)$

Der Grad dieser Rekursionsgleichung waere $2$. Eine Rekursionsgleichung vom Grad
$d$ braucht immer $d$ Anfangsbedingungen fuer die Faelle $f(0), f(1), ..., f(d -
1)$. Die Rekursionsgleichung selbst gilt dann fuer alle $n \in \mathbb{N}_0: n
\geq d$.

Manchmal moechte man dann eine Aussage ueber $f$ ueberpruefen, z.B. dass $f(n)$
gleich einer anderen Funktion $g(n)$ ist. Dann geht man folgends vor:

I. Basis: Zeige die Aussage, dass $f(n) = g(n)$, fuer die Anfangsbedingungen.

II. Schritt: Sei $n \in \mathbb{N}_0, n \geq d - 1$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer $n, n-1, ...,n - (d - 1)$. Man muss hier diese
	         Werte annehmen, weil die Rekursionsgleichung fuer $n + 1$ die
			 Funktion fuer diese Parameter aufruft.

	Behauptung: Dann gilt die Aussage auch fuer $n + 1$.

	Ziel: $f(n + 1) = g(n + 1)$ (Schreibe $g(n + 1)$ aus, $f(n + 1)$ nur als
	      Funktionsaufruf)

Beweis:

	1. Ersetze in der Rekursionsgleichung fuer $n + 1$ die Funktionsaufrufe fuer
       $n,n-1,...,n-d+1$ durch die laut Induktionsannahme gueltigen Werte von
	   $g(n),g(n-1),...,g(n-d+1)$.

	2. Forme den resultierenden Ausdruck zu $g(n + 1)$ um.

#### Beispiel

Zeige: Sei $f: \mathbb{N}_0 \rightarrow \mathbb{N}_0$ eine Funktion mit den
       Anfangsbedingungen $f(0) = 1, f(1) = 3, f(2) = 5$ und der
	   Rekursionsgleichung
	   $f(n + 1) = 3 \cdot f(n) - 3 \cdot f(n - 1) + f(n - 2)$
	   fuer $n \geq 2$. Dann gilt $\forall n \in \mathbb{N}_0: f(n) = 2n + 1$.

Der Grad dieser Rekursionsgleichung ist $3$.

I. Basis: Zeige, dass die zu zeigende Funktion fuer $f(0), f(1), f(2)$ die laut
          Anfangsbedingungen richtigen Werte liefert:

		  1. $f(0) = 2 \cdot 0 + 1 = 1$
		  2. $f(1) = 2 \cdot 1 + 1 = 3$
		  3. $f(2) = 2 \cdot 2 + 1 = 5$

II. Schritt: Sei $n \in \mathbb{N}, n \geq 2$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer $n, n - 1, n - 2$.

	Behauptung: Dann gilt die Aussage auch fuer $n + 1$.

	Ziel: $f(n + 1) = 2(n + 1) + 1 = 2n + 3$

Beweis:

	1. Setze die laut Induktionsannahme gueltigen Werte fuer $n, n - 1, n - 2$
       ein:

	   $3f(n) - 3f(n - 1) + f(n - 2) = 3(2n + 1) - 3(2(n-1) + 1) + 2(n-2) + 1$

	2. Multipliziere aus und forme um:

	   $6n + 3 - (6n - 6 + 3) + 2n - 4 + 1$
	   $6n - 6n + 2n + 3 - 3 + 6 - 4 + 1$
	   $2n + 3$

	3. $\square$.

### Wohlfundierte Induktion

Das Beweisrezept fuer Wohlfundierte Induktion unterscheidet sich von der
Vorgehensweise ein wenig von den Rezepten fuer vollstaendige Induktion. Bei der
wohlfundierten Induktion nehmen wir die Aussage fuer alle Vorgaenger von $n$ an,
und zeigen sie dann fuer $n$ (nicht fuer $n + 1$). Also hier ein Schema:

Sei $R$ eine wohlfundierte Relation (mit endlicher Anzahl an Vorgaengern fuer
jedes Element) auf den natuerlichen Zahlen, z.B. $\{(n + 5, n) | n \in
\mathbb{N}\}$. Zeige die Aussage $P$ fuer alle natuerlichen Zahlen.

Basis: Zeige die Aussage $P$ zuerst fuer alle $n$, die bezueglich $R$ keine
       Vorgaenger haben, also die minimalen Elemente.

Schritt: Sei $n \in \mathbb{N}$	beliebig, aber fest.

	Annahme: Die Aussage gilt fuer alle Vorgaenger von $n$ bezueglich $R$:
	         $m \in \mathbb{M}: mRn$ (die Aussage gilt noch nicht fuer $n$, wie
	         bei der vollstaendigen Induktion!)

	Behauptung: Dann gilt sie auch fuer __$n$__ (nicht $n + 1$).

Beweis: Fuehre die Aussage fuer $n$	auf die Aussage fuer $k \leq n$ zurueck.

## Gotchas

* Wirklich aufpassen, dass man die Aussage fuer alle Faelle zeigt. Also wenn man
  eine Fallunterscheidung hat (z.B. bei rekursiven Gleichungen), fuer beide
  Faelle im Schritt beweisen, nicht nur in der Basis.
