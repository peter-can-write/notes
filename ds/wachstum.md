# Wachstum von Funktionen

Ein Software-Programm verbraucht Ressourcen in Abhaengigkeit der
Groesse der Eingabe. Diese Ressourcen koennen Zeit sein, oder sie koennen
Speicher sein.

Die Laufzeit oder der Speicherverbrauch eines Programms mit Eingabegroesse $x$
wird dabei durch eine Funktion $f(x)$ festgelegt. Diese beschreibt also die
Laufzeit oder den Speicherverbrauch des Programms, um die Eingabe der Groesse
$x$ zu verarbeiten.

Wir interessieren uns dabei dafuer, abzuschaetzen, wie die Werte $f(x)$ sich in
Abhaengigkeit von $x$ veraendern, also wie die Funktion *waechst*. Wir messen
das Wachstum einer Funktion $f$ dabei relativ zu einer anderen Funktion
$g$.

Wir sagen, dass $f_A(n)$ *langsamer waechst als* $f_B(n)$, falls *ab einem
gewissen Punkt* $f_A(n)$ immer unter $f_B(n)$ liegt.

## Klassen von Wachstum

Wir benutzen sogenannte Landau-Symbole, um eine Funktion zu klassifizieren. Mit
diesen druecken wir aus z.B., dass ein Funktions hoechstens oder mindestens so
schnell waechst wie eine andere. Eine Klasse bzw. ein Landau-Symbol beschreibt
dabei eine Menge von Funktionen, z.B. ist $O(n)$ die Menge aller Funktionen, die
bis auf einen konstanten Faktor hoechstens so schnell wachsen wie die Funktion
$n$. Wenn es eine solche Funktion $f$ gibt, dann schreibt man also $f \in O(n)$
(Betonung auf $\in$).

## $O(n)$

$O$ beschreibt die Hoechstlaufzeit (worst case) eines Programms. $f(n)$
gehoert dabei zur Menge der Funktionen $O(g(n))$ genau dann, wenn eine Konstante
$c$ existiert sodass nach einem bestimmten $n_0$ der Wert der Funktion $f$ immer
kleiner oder hoechstens gleich dem Wert der Funktion $g$ ist:

$f(n) \in O(g(n)) \Leftrightarrow \exists c \in \mathbb{R}^+: \exists n_0 \in
\mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| \leq c \cdot
g(n)$

Man beachte, dass $O(n)$ nicht unbedingt *minimal* sein muss. Eine konstante
Funktion $f(x) = 1$ schreibt man zwar meist als $f \in O(1)$, aber ebenso gilt
$f \in O(n^{1000})$, weil $f$ ja eben hoechstens so schnell waechst wie z.B. $5
\cdot n^{1000}$ (ab einem gewissen $n_0$).

"$f$ waechst bis auf einen konstanten Faktor hoechstens so schnell wie $g$"

$f \in O(g)$ schreibt man oft als $f \preceq g$.

## $o(n)$

$f(n)$ gehoert zu Menge der Funktionen $o(g(n))$ genau dann, wenn fuer alle
Konstanten $c \in \mathbb{R}^+$ ein $n_0 \in \mathbb{N}$ existiert, sodass die
Funktion $f(n)$ ab diesem $n_0$ strikt langsamer waechst als $g(n)$, bis auf den
konstanten Faktor.

$f(n) \in o(g(n)) \Leftrightarrow \forall c \in \mathbb{R}^+: \exists n_0 \in
\mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| < c \cdot g(n)$

"$f$ waechst echt/strikt langsamer als $g$"

Wenn eine Funktion strikt langsamer waechst als eine andere, waechst sie
natuerlich auch hoechtens so schnell: $f \in o(g) \rightarrow f \in O(g)$.

Man beachte hier die Reihenfolge der Quantoren: Es gibt fuer alle Konstanten ein
$n_0$ ab welchem die Funktion langsamer waechst, und nicht *ein* $n_0$ *fuer
alle Konstanten*. $n_0$ muss also nicht fuer alle Konstanten eindeutig.

Strebt $n$ gegen unendlich, und ist $f \in o(g)$, dann gilt:
$\frac{|f(n)|}{g(n)} = 0$

$f \in o(g)$ schreibt man oft als $f \prec g$.

## $\Omega(n)$

$\Omega$ ist gewissermassen das Gegenteil von $O$. Es beschreibt nicht die
Hoechstlaufzeit (worst case) eines Programms, sondern die *Mindestlaufzeit*
(best case). Eine Funktion $f(n)$ ist daher $\in \Omega(g(n))$, genau dann, wenn
es eine Konstante $c$ gibt, sodass ab einem $n_0$ die Funktion $f$ *mindestens*
so schnell waechst wie $g$.

$f(n) \in \Omega(g(n)) \Leftrightarrow \exists c \in \mathbb{R}^+ : \exists n_0
\in \mathbb{N} : \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| \geq c \cdot g(n) \geq 0$

"$f$ waechst bis auf einen konstanten Faktor mindestens so schnell wie $g$"

$f \in \Omega(g)$ schreibt man oft als $g \preceq f$

## $\omega(n)$

$\omega$ ist zu $\Omega$ wie $o$ zu $O$. $\omega$ ist also eine schaerfere
Einschraenkung auf das Wachstum der Funktion. Eine Funktion $f(n)$ ist $\in
\omega(g(n))$, wenn es fuer jede Konstante ein $n_0$ gibt, sodass die Funktion
$f(n)$ ab diesem $n_0$ strikt schneller waechst als $c \cdot g(n)$.

$f(n) \in \omega(g(n)) \Leftrightarrow \forall c \in \mathbb{R}^+ : \exists n_0
\in \mathbb{N} : \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| > c
\cdot g(n) \geq 0$

"$f$ waechst echt/strikt schneller als $g$"

Wenn eine Funktion strikt schneller waechst als eine andere, waechst sie
natuerlich auch mindestens so schnell: $f \in \omega(g) \rightarrow f \in
\Omega(g)$.

Man beachte auch hier die Reihenfolge der Quantoren: Es gibt fuer alle
Konstanten ein $n_0$ ab welchem die Funktion langsamer waechst, und nicht *ein*
$n_0$ *fuer alle Konstanten*. $n_0$ muss also nicht fuer alle Konstanten
eindeutig.

Strebt $n$ gegen unendlich, und ist $f \in \omega(g)$, dann gilt:
$\frac{|g(n)|}{f(n)} = 0$

$f \in \omega(n)$ schreibt man oft als $g \prec f$.

## $\Theta(n)$

$\Theta$ ist sozusagen die Vereinigung von $O$ und $\Omega$. Eine Funktion
$f(n)$ ist in $\Theta(g(n))$, genau dann, $g(n)$ sowohl untere als auch obere
Schranke von $f(n)$ ist, bis auf konstante Faktoren. Es existieren also zwei
Konstanten $c_1,c_2$, sodass die Funktion ab einem gewissen $n_0$ zwischen $c_1
g(n)$ liegt und $c_2 g(n)$.

$f(n) \in \Theta(g(n)) \Leftrightarrow f(n) \in O(g(n)) \land f(n) \in
\Omega(g(n))$

oder:

$f(n) \in \Theta(g(n)) \Leftrightarrow f(n) \in O(g(n)) \land g(n) \in O(f(n))$


(wenn $f(n)$ mindestens so schnell waechst wie $g(n)$, dann waechst $g(n)$
hoechstens so schnell wie $f(n)$).

oder:

$f(n) \in \Theta(g(n)) \Leftrightarrow \exists c_1,c_2 \in \mathbb{R}^+: \exists
n_0 \in \mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow c_1 \cdot
g(n) \leq |f(n)| \leq c_2 \cdot g(n)$

"f waechst bis auf konstante Faktoren genau so schnell wie g"

Oftmals sagen Informatiker ueber die Laufzeit $f(n)$ eines Algorithmus aus, dass
sie $O(g(n))$ ist, aber *asymptotically tight*. Das bedeutet dann soviel wie,
dass $f(n) \in \Theta(g(n))$ ist, also dass $f(n)$ nicht bis auf einen
konstanten Faktor hoechstens so schnell waechst wie $g(n)$, sondern dass $f(n)$
bis auf einen konstanten Faktor gleich schnell waechst wie $g(n)$.

http://cs.stackexchange.com/questions/19141/what-is-an-asymptotically-tight-upper-bound

## Asymptotisch Gleich ($\sim$)

Zwei Funktionen $f$ und $g$ sind asymptotisch gleich, wenn gilt:

$\lim_{n\rightarrow\infty} \frac{|f(n)|}{g(n)} = 1$

Bei $\Theta(g)$ konnte die Funktion noch zwischen zwei Konstanten $c_1, c_2$ mal
$g$ liegen, hier muss es eine Konstante $c$ geben sodass $f(n) = c \cdot
g(n)$. Diese Konstante faellt im Limes in der obigen Definition weg.

Man schreibt "$f$ ist asymptotisch gleich zu $g$" oft als $f \sim g$.

https://en.wikipedia.org/wiki/Big_O_notation#Family_of_Bachmann.E2.80.93Landau_notations

## Uebersicht

Hier alle fuenf Definitionen. Diese sind wichtig, um zu beweisen, dass eine
Funktion $f$ in einer diese Klassen ist:

1. $f \in O(g) \Leftrightarrow \exists c \in \mathbb{R}^+: \exists n_0 \in
  \mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| \leq c
  \cdot g(n)$

2. $f \in \Omega(g) \Leftrightarrow \exists c \in \mathbb{R}^+: \exists n_0 \in
  \mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| \geq c
  \cdot g(n)$

3. $f \in o(g) \Leftrightarrow \forall c \in \mathbb{R}^+: \exists n_0 \in
  \mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| < c \cdot
  g(n)$

4. $f \in \omega(g) \Leftrightarrow \forall c \in \mathbb{R}^+: \exists n_0 \in
  \mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |f(n)| > c \cdot
  g(n)$

5. $f \in \Theta(g) \Leftrightarrow f \in O(g) \land f \in \Omega(g)$

Und hier alle Negationen der obigen Aussagen. Diese sind wichtig, um zu
beweisen, dass eine Funktion $f$ *nicht* in einer dieser Klassen ist.

1. $f \notin O(g) \Leftrightarrow \forall c \in \mathbb{R}^+: \forall n_0 \in
   \mathbb{N}: \exists n \in \mathbb{N}: n \geq n_0 \land |f(n)| > c \cdot g(n)$

2. $f \notin \Omega(g) \Leftrightarrow \forall c \in \mathbb{R}^+: \forall n_0
   \in \mathbb{N}: \exists n \in \mathbb{N}: n \geq n_0 \land |f(n)| < c \cdot
   g(n)$

3. $f \notin o(g) \Leftrightarrow \exists c \in \mathbb{R}^+: \forall n_0 \in
   \mathbb{N}: \exists n \in \mathbb{N}: n \geq n_0 \land |f(n)| \geq c \cdot
   g(n)$

4. $f \notin \omega(g) \Leftrightarrow \exists c \in \mathbb{R}^+: \forall n_0
   \in \mathbb{N}: \exists n \in \mathbb{N}: n \geq n_0 \land |f(n)| \leq c
   \cdot g(n)$

5. $f \notin \Theta(g) \Leftrightarrow f \notin O(g) \lor f \notin \Omega(g)$

## Abhaengigkeiten

Hier nun einige Verbindungen zwischen den Symbolen:

* $f \in o(g) \Rightarrow f \in O(g)$; wenn $f$ strikt langsamer waechst als
  $g$, waechst $f$ auch hoechstens so schnell.

* $f \in \omega(g) \Rightarrow f \in \Omega(g)$; wenn $f$ strikt schneller
  waechts als $g$, waechts $f$ auch mindestens so schnell.

* $f \in O(g) \Leftarrow f \in \Theta(g)$; (!) Wenn $f \in O(g)$ ist, bedeutet
  das nicht, dass $f$ auch mindestens so schnell waechst wie $g$. $f$ kann ja
  langsamer wachsten, dann waere $\Omega(g)$ falsch bzw. zu pessmistisch. Die
  andere Richtung folgt aus der Definition von $\Theta$.

* $f \in \Omega(g) \Leftarrow f \in \Theta(g)$; (!) symmetrische Argumentation
  wie oben.

* $f \in o(g) \Leftrightarrow g \in \omega(f)$: Wenn $f$ strikt langsamer
  waechst als $g$, waechst $g$ strikt langsamer als $f$.

* $f \in O(g) \Leftrightarrow g \in \Omega(f)$: Wenn $f$ hoechstens so schenll
  waechst wie $g$, waechst $g$ mindestens so schnell wie $f$.

* $f \in \Theta(g) \Leftrightarrow g \in \Theta(f)$: Wenn $f$ bis auf konstante
  Faktoren genauso schnell waechst wie $g$, dann waechst $g$ genauso schnell wie
  $f$. Anderer Beweis: $f \in Theta(g) \Rightarrow f \in O(g) \land f \in
  \Omega(g)$, dann:
  + $f \in O(g) \Rightarrow g \in \Omega(f)$
  + $f \in \Omega(g) \Rightarrow g \in O(f)$
  Also gilt: $g \in \Omega(f) \land g \in O(f) \Rightarrow g \in \Theta(f)$.

## Rechenregeln

Nun einige Eigenschaften der Landau Notation:

1. Landau Symbole sind transitiv: $f \in O(g) \land g \in O(h) \rightarrow f \in
   O(h)$.

2. Die Summe zweier Funktionen in einer Klasse ergibt eine Funktion in der
   selben Klasse, weil sich der Grad nicht veraendert: $f,g \in O(h) \rightarrow
   f + g \in O(h)$.

3. Wenn $n$ gegen unendlich strebt, sind Konstanten egal: Sei $c > 0$, dann ist
   $O(f \cdot c) = O(f + c) = O(f - c) = O(f)$.

4. Das Produkt zweier Funktionen zweier Klassen ist in der Klasse des Produkts
   der Klassen, da sich die Grade so gleich veraendern (z.B. $n^2 \cdot n = n^3$
   fuer $f$s und $g$s): $f_1 \in O(g_1), f_2 \in O(g_2) \rightarrow f_1 \cdot
   f_2 \in O(g_1 \cdot g_2)$

5. Die Summe zweier Funktionen zweier Klassen sind in der Klasse der groesseren
   Klasse: Sei $f_1 \in O(g_1), f_2 \in O(g_2)$. Dann ist $f_1 + f_2 \in O(g_1 +
   g_2)$. Wenn die Grade von $g_1,g_2$ verschieden sind, ist eine der beiden
   Funktionen in der Klasse der anderen, daher: $f_1 + f_2 \in O(\max(g_1,g_2))$
   und $f_1 + f_2 \in O(g_2)$ wenn $g_1 \in O(g_2)$.

6. Logarithmen aller Basen wachsen gleich schnell: $\log_2 n \sim \log_b n
   \,\forall b \in \mathbb{N}$

7. Moechte man zwei Funktionen der Form $\frac{1}{...}$ miteinander vergleichen,
   darf man auch nur den Nenner vergleichen:
   + $\frac{1}{f(n)} \prec \frac{1}{g(n)} \Leftrightarrow f(n) \prec g(n)$
   + $\frac{1}{f(n)} \sim \frac{1}{g(n)} \Leftrightarrow f(n) \sim g(n)$

8. Alle Definitionen der Landau-Symbole enthalten Betraggstriche. Diese sind
   nur fuer Funktionen mit $\mathbb{R}$ als Definitionsmenge (Urbild) relevant,
   also kann man sie fuer Funktionen $\mathbb{N} \rightarrow ?$ fallen lassen,
   was beim rechnen hilft.

Wenn man $O(g)$ als Term in einem arithmetischen Ausdruck stehen hat, dann
bedeutet das: "eine Funktion aus der Menge $O(g)$". Z.b., wenn steht
$O(\frac{1}{n^2})$, dann ist eine beliebige aber feste Funktion $f(n) \in
O(\frac{1}{n^2})$.

### Spezielle Faelle

Hier einige bewiesene spezielle Faelle:

* $n! \in O(n^n)$, weil $n(n-1)(n-2)... \leq n(n)(n)$

* $2^n \in O(2^{2n})$

* $n! \in \Omega((\frac{n}{e})^n)$

* $n! \in O((n \cdot \frac{n}{e})^n)$

### Grenzwerte

Falls $\lim_{n\rightarrow \infty} \frac{|f(n)|}{g(n)}$ existiert und $g(n)$ aber
einem bestimmten $n$ positiv ist, gilt:

* $f \in o(g) \Leftrightarrow
  \lim_{n\rightarrow \infty} \frac{|f(n)|}{g(n)} = 0$

* $f \in \omega(g) \Leftrightarrow
  \lim_{n\rightarrow \infty} \frac{|f(n)|}{g(n)} = \infty$

* $f \in O(g) \Leftrightarrow
   \lim_{n\rightarrow \infty} \frac{|f(n)|}{g(n)} < \infty$

* $f \in \Omega(g) \Leftrightarrow
   \lim_{n\rightarrow \infty} \frac{|f(n)|}{g(n)} > 0$

* $f \in \Theta(g) \Leftrightarrow
   \lim_{n\rightarrow \infty} \frac{|f(n)|}{g(n)} = c, 0 < c < \infty$

### Logarithmengesetze

Weil man beim Wachstum von Funktionen oft mit Logarithmen rechnen muss, hier
einige wichtige Regeln:

1. $\log_a x = \frac{\log_b x}{\log_b a}$

2. $\log_a(n \cdot m) = \log_a n + \log_a b$

3. $\log_a(\frac{n}{m}) = \log_a n - \log_a m$

4. $\log_a n^m = m \cdot \log_a n$

5. $\log_{a^b} n = \frac{log_a n}{b}$

6. Spezialfaelle: $\log_a 1 = 0, \log_a a = 1$

7. $x = \log_b b^x = b^{\log_b x}$

Beweise:

1. Sei $c = \log_b a$, dann:
   + $b^c = a$
   + $\log_d b^c = \log_d a$
   + $c \cdot log_d b = \log_d a$
   + $c = \frac{log_d a}{\log_d b}$
   + (aus Definition oben): $\log_b a = \frac{\log_d a}{\log_d b}$

2. $\log_b(n \cdot m) = \log_b(b^{\log_b n} \cdot b^{\log_b m}) =
   \log_b(b^{\log_b n + \log_b m}) = \log_b n + \log_b m$

3. $\log_b(\frac{n}{m}) = \log_b(\frac{b^{\log_b n}}{b^{\log_b m}}) =
   \log(b^{\log_b n - \log_b m}) = \log_b n - \log_b m$

4. $\log_a n^m = \log_a (b^{\log_b n})^m = \log_a (b^{\log_b n \cdot m}) =
   \log_b n \cdot m$

5. (ueber 1.): $\log_{a^b} n = \frac{\log_a n}{\log_a a^b} = \frac{\log_a n}{b}$

## Beweistechniken

Man moechte also beweisen, dass zwei Funktionen $f, g$ in einer bestimmten
Beziehung zueinander stehen (z.B. $f \in o(g)$ oder $f \notin \Omega(g)$). Dann
sind hier die Definitionen der Landau-Symbole sowie ihrer Negationen sehr
wichtig. Zuerst sei aber gesagt, dass nicht alle Funktionen in einer Relation
mit einer anderen stehen koennen, z.B. wenn die eine Funktion nicht stetig ist
sondern ueber und unter die ander Funktion huepft.

Aber allgemein wird eine solche Defintion das folgende Schema haben:

$Q_1 c \in \mathbb{R}^+: Q_2 n_0 \in \mathbb{N}: Q_3 \in \mathbb{N}: n \geq n_0
\land |f(n)| ? c \cdot g(n)$

wo $Q_1,Q_2,Q_3$ Quantoren sind. Dann muss man fuer die Variable jeden
Existenquantors unter $Q_1,Q_2,Q_3$ einen konkreten Wert finden, der die
Ungleichung fuer die Funktionen erfuellt. Nach den Regeln der Praedikatenlogik
steht eine jede solche Variable in Abhaengigkeit aller Variablen von
Allquantoren links vom Existenzquantor. Z.B. in $\forall x \exists y$ steht $y$
in Abhaengigkeit der Variable $x$ des Allquantors links. Wenn wir also einen
Wert finden wollen, der die Formel erfuellt, muss die Variable des
Existenzquantors einen Wert haben, der von den Variablen der Allquantoren
abhaengt. Das ist so, weil wenn die Variable des Allquantors in der Formel
auftritt, wir keinen konkreten Wert fuer diese Variable festlegen koennen, da
die Variable des Allquantors ja beliebig ist (nach der Semantik des
Allquantors).

### Beispiel 1

Zeige, dass $6n^2 \in o(2n^3)$.

Dann nehme man zuerst die Definition von $\in o$:

$f \in o(g) \Leftrightarrow \forall c \in \mathbb{R}^+: \exists n_0 \in
\mathbb{N}: \forall n \in \mathbb{N}: n \geq n_0 \rightarrow |6n^2| < c \cdot
2n^3$.

Dann sucht man zuerst den Wert fuer $n$, der die Ungleichung erfuellt. Wir
nehmen an, dass die Defintionsmenge $\mathbb{N}$ ist, also koennen wir die
Betraggstriche weglassen.

1. $|6n^2| < c \cdot 2n^3$
2. $6n^2 < c2n^3$, dividiere durch $n^2$
3. $6 < \frac{c2n^3}{n^2}$, kuerze
4. $6 < c2n$, dividiere durch $2c$
5. $\frac{3}{c} < n$, erweitere auf $\geq$
6. $\frac{3}{c} + 1 \leq n$ bzw.
7. $n \geq \frac{3}{c} + 1$

Nun haben wir also die Bedingung fuer $n$ gefunden, sodass die Funktionen die
gewollte Beziehung haben. Als naechstes suchen wir ein $n_0 \in \mathbb{N}$
(neben einem *Existenzquantor*), das in Abhaengigkeit von allen Variablen von
Allquantoren links steht, und das die Ungleichung oben erfuellt. In diesem Fall
koennen wir trivialerweise $n_0 := \frac{3}{c} + 1$. Man muss hier jedoch
beachten, dass unsere Wertemenge nur natuerliche Zahlen enthaelt, wobei
$\frac{3}{c}$ nicht immer natuerlich ist. Also muessen wir noch __aufrunden__,
dann ist der Wert auch noch immer sicher groesser. Dann haben wir also ein
$n_0$, das die ganze Formel wahr macht: $n_0 := \lceil\frac{3}{c} + 1\rceil$.

### Beispiel 2

Zeige, dass $2^n \notin \omega(n^n)$.

Zuerst die Defintion von $\notin \omega$:

$f \notin \omega(g) \Leftrightarrow \exists c \in \mathbb{R}: \forall n_0 \in
\mathbb{N}: \exists n \in \mathbb{N}: n \geq n_0 \land |2^n| \leq c \cdot n^n$

Nun loesen wir zuerst wieder die Ungleichung, um zu sehen, fuer welche $n$ die
Aussage gilt:

1. $|2^n| \leq c \cdot n^n$
2. $2^n \leq cn^n$, dividiere durch $c$
3. $\frac{2^n}{c} \leq n^n$, nehme die $n$-te Wurzel
4. $\sqrt[n]{\frac{2^n}{c}} \leq n$, kuerze Potenzen
5. $\frac{2}{\sqrt[n]{c}} \leq n$

Bei dieser Definition ist nun die Konstante $c$ neben einem Existenzquantor. Da
sie ganz links ist, haengt sie von keiner Allquantor-Variable ab, und wir
koennen sie frei definieren. Hierbei wollen wir sie natuerlich so definieren,
dass die obige Ungleichung leichter wird. hier z.B. indem wir $c := 1$
definieren:

$\frac{2}{\sqrt[n]{1}} \leq n \Rightarrow 2 \leq n \Rightarrow n \geq 2$

Nun muessen wir, um das $n$ zu bestimmen, es in Abhaengigkeit von $n_0$ bringen,
weil es bei einem Allquantor links steht. Weiters muss fuer $n$ die obige
Ungleichung gelten. Hier, wie oft auch, ist es dabei nuetzlich, die
Maximumsfunktion $\max$ zu benutzen:

$n := \lceil \max\{n_0, 2\} \rceil$

Dadurch gilt in jedem Fall dass die beiden Bedingungen erfuellt sind. Eine
andere Moeglickeit waere es, die beiden zu addieren.

## Komplexizitaetsklassen

Hier nun aufgelistet einige bekannte bzw. wichtige Komplexizitaetsklassen in
$O$-Notation. Fuer jede Klasse gilt, dass Funktionen in ihr auch hoechstens so
schnell waechsen wie die Funktionen der darauffolgenden Klasse (z.B. $\forall f:
f \in O(1) \rightarrow f \in O(n)$). Mit $\log$ ist hier immer $\log_2$
gemeint.

1. Konstante Funktionen: $O(1)$
2. Logarithmische Funktionen: $O(\log n)$
3. Poly-Logarithmische Funktioen: $O(\log^k n)$
4. Wurzel Funktionen: $O(\sqrt{n})$
5. Lineare Funktionen: $O(n)$
6. Linearithmische Funktionen: $O(n \log n)$
7. Quadratische Funktionen: $O(n^2)$
8. Kubische Funktionen: $O(n^3)$
9. Polynomielle Funktionen: $\bigcup_{k \geq 1} O(n^k)$
10. Exponentielle Funktionen: $k^n$
11. $n!$-Funktionen: $n!$
12. $n^n$-Funktionen: $n^n$

Ein Algorithmus, der in $O(\sqrt{n})$ laeuft, ist beispielsweise ein
primality-test.

Ein Algorithmus, der in $O(n^n)$ laeuft, waere das $n$-fache Kartesische Produkt
einer Menge mit sich selbst: $M^n, |M| = n$.

Es gibt noch weitere spezielle Klassen, die auch Namen haben. Sei $T_A$ die
Laufzeit eines Algorithmus $A$, dann sagt man, dass $A$ die Laufzeit des
Algorithmus:

* *polynomiell* ist, falls:
  $T_A \in P := U_{k\in\mathbb{N}} O(n^k)$

* *quasi-polynomiell* ist, falls:
  $T_A \in QP := U_{k \in \mathbb{N}} 2^{O(\log^k n)}$
  Eine Funktion $\in QP$ waechst schneller als alle polynomiellen Funktionen,
  aber langsamer als alle exponentiellen.

* *super-polynomiell* ist, falls:
  $T_A \in SP := \cap_{k \in \mathbb{N}} \omega(n^k)$
  Eine Funktion $\in SP$ waechts schneller als alle polynomiellen
  Funktionen. Hier verwendet man $\cap$, weil alle Funktionen
  $\in \omega(n^{k+1})$ auch $\in \omega(n^k)$ sind.

* *exponentiell* ist, falls:
  $T_A \in EXPTIME := \cup_{k \in \mathbb{N}} O(2^{n^k})$

* *schwach-subexponentiell* ist, falls:
  $T_A \in SUBEPT := 2^{o(n)}$
  Eine Funktion $\in SUBEPT$ ist auch in $SP$, aber waechst schneller als eine
  $\in QP$.

Bei den Klassen, wo das Landau-Symbol die Potenz von $2$ ist, muss so
vorgegangen werden: Um z.B. zu zeigen, dass eine Funktion $f \in 2^{o(n)}$ ist,
schreibe $f$ als $2^{\log f} = f$, und siehe dann, ob $\log f \in o(n)$ ist.

Hier gilt dann: $P \subseteq QP \subseteq SP \subseteq SUBEPT \subseteq EXPTIME$

## Gotchas

* Bei Beweisen das Aufrunden nicht vergessen.
