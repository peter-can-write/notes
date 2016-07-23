# Lineare Abbildungen

$\newcommand{\dim}{\mathop{\rm dim}\nolimits}$
$\newcommand{\Kern}{\mathop{\rm Kern}\nolimits}$
$\newcommand{\Bild}{\mathop{\rm Bild}\nolimits}$
$\newcommand{\Id}{\mathop{\rm Id}\nolimits}$

Seien $V, W$ Vektorraeume, $K$ ein Koerper.

Eine *Abbildung* $f: V \rightarrow W$ heisst linear, falls gelten:

1. Fuer alle $v, v' \in V$: $f(v + v') = f(v) + f(v')$ (bzgl. Vektoraddition)
2. Fuer alle $v \in V, a \in K: f(a \cdot v) = a \cdot f(v)$ (bzgl. Skalarmultiplikation)

Eine erste Beobachtung, die wir aus (2) schliessen koennen, ist dass eine
lineare Abbildung den Nullvektor von $V$ auf den Nullvektor von $W$
abbildet. Beweis: Sei $v$ beliebig und $a = 0$. Dann gilt nach
Linearitaetskriterium $f(a \cdot v) = f(0) = a \cdot f(v) = 0 \cdot f(v) = 0.$. Ist $f$ linear, so kann man die Anwendung von $f$ auf einen Vektor durch eine Matrixmultiplikation nachahmen. Diese Matrix nennt man dann *Darstellungsmatrix* und wird weiter unten besprochen.

Beispiele:

1. $A \in K^{m \times n}$. Dann ist $f_A: K^n \rightarrow K^m, v \mapsto Av$
   eine lineare Abbildung. Man beachte: $K^n \rightarrow K^m$. Der Eingabevektor
   $x$ muss gleich viele Komponenten haben wie $A$ Spalten hat, und der
   Ausgabevektor hat Dimensionen $m \cdot 1 = m$ (eine Komponente pro Zeile von
   $A$). Allgemein bemerkt man, dass wenn die Abbildung nur mit irgendeiner
   Konstante (sei es ein Skalar oder eine Matrix) multipliziert, die Abbildung
   linear wird. Erst wenn wir etwas im Bild dazuaddieren, wird die Abbildung
   affin und dann nicht mehr linear.

2. Die Nullabbildung $V \rightarrow W, v \mapsto 0$ ist linear.

3. Sei $f: \mathbb{R}^2 \rightarrow \mathbb{R}^2$.
   Dann sind lineare Abbildungen z.B.:
   * Drehung um den Nullpunkt
   * Streckung mit Nullpunkt als Zentrum
   * Spiegelung an einer durch Null gehenden Geraden
   Aber nicht:
   * Drehungen um Punkte $\neq 0$
   * Verschiebungen (Translationen, also $f(v) = v + (1, 1)^T$),
     z.B. Verschiebung einer Datenwolke um ihren Mittelwert (zur Null).

4. Sei $V = \mathbb{R}[x]$ der Polynomring in $\mathbb{R}$: Dann ist $f: V
   \rightarrow V, f \rightarrow f'$, also die Ableitungsabbildung, eine lineare
   Abbildung, denn $(a \cdot f)' = a \cdot f'$ (Konstantenregel) sowie $(f + g)'
   = f' + g'$ (Summenregel) (!)

5. Fuer einen Vektorraum $V = K^n$ und einem Index $i \in \{1,...,n\}$ ist
   $\pi_i: V \rightarrow K, (x_1, ..., x_n)^\top \mapsto x_i$ linear und heisst
   $i$-tes Koordinatenfunktional. Es selektiert also immer die $i$-te Komponente
   aus einem Vektor.

6. Sei $M$ eine Menge, $x_1,...,x_n \in M$ fest. Dann ist $f: V =
   \text{Abb}(M,K) \rightarrow K^n, f \mapsto (f(x_1), ..., f(x_n))^\top$
   linear. Diese Funktion bildet eine Funktion also auf eine neue Funktion ab,
   die die Funktion elementweise anwendet. Also `np.vectorize`.

7. Sind $\phi, \psi: V \rightarrow W$ linear, so auch $\phi + \psi: V
   \rightarrow W, v \mapsto \phi(v) + \psi(v)$ und fuer ein $a \in K$ auch $a
   \cdot \phi: V \rightarrow W, v \mapsto a \cdot \phi(v)$. Die Menge aller
   linearen Abbildungen sind also selbst ein abgeschlossener Vektorraum.

8. Sind $\phi: V \rightarrow W$ und $\si: W \rightarrow U$ linear, so ist auch
   die Komposition $\psi \circ \phi: V \rightarrow U, (\psi \circ \phi)(v) =
   \psi(\phi(v))$ linear.

### Kern, Bild und Unterraeume

Sei $f: V \rightarrow W$ eine lineare Abbildung. Dann nennt man die Menge jener
Vektoren $v$ in $V$, fuer welche $f(v) = 0$, den *Kern* von $f$. Also:

$$\Kern(f) = \{v \in V \,|\, f(v) = 0\} \subseteq V.$$

Auch nennt man die Menge aller Werte $\in W$, auf welche $f$ zumindest einmal
abbildet, das *Bild* von $f$:

$$\Bild(f) = f(V) = \{f(v) \,|\, v \in V\} \subseteq W.$$

#### Eigenschaften

Sei also $f: V \rightarrow W$ eine lineare Abbildung. Dann gilt:

1. Der Kern von $f$ ist ein Unterraum von $V$
2. Das Bild von $f$ ist ein Unterraum von $W$
3. Ist $f$ injektiv, so ist der Kern nur $\{0\}$. Achtung: $\Leftrightarrow$!

##### Beweis (1)

Wir zeigen die Aussage durch explizite Kontrolle der Unterraumeigenschaften:

1. Der Kern darf nicht die leere Menge sein. Da eine lineare Abbildung zumindest
   immer die Null auf die Null abbildet, ist der Kern sicher nicht leer.

2. Der Kern muss bezueglich der Vektoraddition abgeschlossen sein. Nehmen wir
   also $a,b \in \Kern(f)$. Dann sollte gelten, dass $a + b$ wieder im
   Kern von $f$ ist. Das gilt, weil $f$ linear ist:
   $f(a + b) = f(a) + f(b) = 0 + 0 = 0$

2. Der Kern muss bezueglich der Skalarmultiplikation abgeschlossen sein. Nehmen
   wir also wieder einen Vektor $a$ aus dem Kern und ein Skalar $s \in K$. Dann
   folgt wieder aus der Linearitaet von $f$:
   $f(s \cdot a) = s \cdot f(a) = s \cdot 0 = 0$

##### Beweis (2)

Wir zeigen die Aussage wieder durch explizite Kontrolle der
Unterraumeigenschaften:

1. Das Bild darf nicht die leere Menge sein. Da wir schon gezeigt haben, dass
   zumindest der Kern nicht leer ist, folgt die Aussage.

2. Das Bild muss bezueglich der Vektoraddition abgeschlossen sein. Nehmen wir
   also $a,b \in \Bild(f)$. Dann sollte gelten, dass $a + b$ wieder im
   Bild von $f$ ist. Seien hierzu $x, y$ die Urbilder von $a, b$, also $f(x) =
   a, f(y) = b$. Dann gilt:
   $f(x + y) = f(x) + f(y) = (a + b)$. Somit ist also $a + b$ wieder im Bild.

2. Das Bild muss bezueglich der Skalarmultiplikation abgeschlossen sein. Nehmen
   wir also wieder einen Vektor $a$ aus dem Bild und ein Skalar $s \in K$. Sei
   auch $f(x) = a$. Dann folgt wieder aus der Linearitaet von $f$: $f(s \cdot x)
   = s \cdot f(x) = (s \cdot a)$. Somit ist also auch $s \cdot a$ wieder im Bild.

##### Beweis (3)

$\Rightarrow$:

Zu zeigen: Wenn $f$ injektiv ist, dann enthaelt der Kern nur die Null.

Wir wissen, dass bei einer linearen Abbildung die Null auf die Null abgebildet
wird. Insofern ist die Null schon einmal im Kern. Da die Funktion injektiv ist,
kann es auch kein weiteres Element geben, dass auf die Null abgebildet wird und
somit im Kern ist.

$\Leftarrow$:

Zu zeigen: Wenn der Kern nur die Null enthaelt, dann ist $f$ injektiv.

Seien $v, v' in V$ zwei Vektoren, die auf dasselbe abbilden: $f(v) = f(v')$. Die
Abbildung $f$ waere dann injektiv, wenn nun gelten wuerde, dass $v = v'$ sein
muss. Ueber Linearitaet gilt jedenfalls:

1. $f(v) = f(v')$
2. $f(v) - f(v') = 0$
3. $f(v - v') = 0$

Anscheinend ist $v - v'$ also im $\Kern(f)$. Nun hatten wir aber
angenommen, dass der Kern nur die Null enthaelt. Es muss also gelten, dass $v -
v' = 0$. Somit ist also $v = v'$ und das ist gerade die Eigenschaft, die
aussagt, dass $f$ injektiv ist.

#### Beispiele

Sei $A \in K^{m \times n}$. Dann ist die "Matrizenabbildung" $f_A: x \mapsto Ax$
ja eine lineare Abbildung. Nun gilt: Der Kern $\Kern(f_A)$ dieser
Abbildung ist die Loesungsmenge des homogenen Gleichungssystem $Ax = 0$. Das
sind dann naemlich alle moeglichen Vektoren $x$, sodass $Ax = 0$. Insbesondere
gilt, dass die Funktion $f_A$ genau dann injektiv ist, wenn das homogene LGS
$f_A(x) = 0$ nur die triviale Loesung besitzt. Denn dann ist der Kern der
Funktion eben gleich Null.

Wir betrachten $V = \mathbb{R}[x]$ und eine lineare Abbildung $f: V \rightarrow
V, f \mapsto f'$. Dann ist:

1. Der Kern von $f$ ist die Menge aller konstanten Polynome.
2. Das Bild ist wieder $V$ selbst (jedes Polynom ist Ableitung einer anderen
   Funktion, der Stammfunktion).

## Koordinaten

Sei $V$ ein $K$-Vektorraum mit $\dim(V) = n < \infty$ und sei $B = \{v_1,
..., v_2\}$ eine Basis von $V$. Dann kann man eine lineare Abbildung

$$f: K^n \rightarrow V: (a_1, ..., a_n)^\top \rightarrow \sum_{i=1}^n a_iv_i$$

definieren, die also einen $n$-dimensionalen Vektor mit Werten aus $K$ auf das
Element $v$ in $V$ abbildet, dass als Linearkombination der Koeffizienten $a_i$
dieses Vektors fuer entsprechende Basisvektoren $v_i \in B$ entsteht.

Die lineare Unabhaengigkeit der Basisvektoren aus $B$ ergibt, dass der Kern von
$f$ genau die Null ist ($\Kern(f) = \{0\}$). Denn fuer eine linear
unabhaengige Menge von Vektoren gibt es nur die triviale Darstellung der
Null. Es gibt also nur einen Vektor, der auf das Nullelement in $V$ abgebildet
wird, und dass ist der Nullvektor in $K^n$. Insofern ist also $\Kern(f) =
\{0\}$. Wir wissen, dass eine Funktion genau dann injektiv ist, wenn das gilt.

Weiters ist $B$ ein Erzeugendenssystem (sogar eine Basis), daher gibt es fuer
jedes Element $v \in V$ einen Vektor von Koeffizienten, sodass $\sum_{i=1}^n
a_iv_i = v$. Daher bildet also $f$ sicher auf jedes Element $\in V$ ab und $B$
ist also surjektiv.

Die Funktion ist also injektiv und surjektiv, somit bijektiv. Eine Bijektion
nennt man auch *Isomorphismus*. Eine lineare Abbildung $f: V \rightarrow W$
heisst also *Isomorphismus*, wenn sie bijektiv ist. Dann ist naemlich auch die
Umkehrabbildung $f^{-1}: W \rightarrow V$ ein Isomorphismus bzw. bijektiv. Wenn
es einen Isomorphismus fuer $V \rightarrow W$ gibt, dann nennt man $V, W$
*isomorph* und schreibt $V \cong W$.

Die Umkehrabbildung $f^{-1}$ zu obigem $f$ assoziiert zu jedem $v \in V$ seinen
Koordinatenvektor bezueglich Basis $B$, d.h. den __eindeutigen__ Vektor (weil
$f$ und $f^{-1}$ bijektiv), der genau die Koeffizienten der Linearkombination
der Vektoren in $B$ enthaelt.

Aus diesem Isomorphismus folgt der Satz, dass fuer jeden $n$-dimensionalen
Vektorraum $V$ mit $n = \dim(V) < \infty$ gilt, dass dieser isomorph zum
$K^n$. Dann gilt also: $$V \cong K^n$$

Der Punkt ist, dass man beim Arbeiten mit endlich-dimensionalen Vetktorraeumen
man immer "in $K^n$" denken kann.

Beispiel:

Sei $V$ der Vektorraum aller Polynome mit Grad hoechstens zwei. Dann haben wir
also Polynome der Form $a_1 + a_2x + a_3x^2$. Nun koennen wir einen
Isomorphismus dieses Raumes zum $K^3$ bilden, wobei wir als Koordinatenvektoren
immer den drei-dimensionalen Vektor $(a_1, a_2, a_3)^\top$ betrachten. Hierbei
sind naemlich die "Basisvektoren" des Polynomrings $(1, x, x^2)$ sodass die
Dimension des Raumes gleich drei ist. Also gilt, dass dieser Raum $\cong K^3$
ist.

## Dimensionanssatz fuer Lineare Abbildungen

Satz: Sei $f: V \rightarrow W$ linear. Dann gilt:

$$\dim(V) = \dim(\Kern(V)) + \dim(\Bild(V))$$.

### Beweis

Beweis (fuer den endlichen Fall, die Aussage gilt aber allgemein):

Sei $\{w_1, ..., w_n\}$ eine Basis des Bildes von von $f$ und $\{v_1, ...,
v_n\}$ eine Basis des Kernes von $f$. O.B.d.A. koennen wir $v_1', ..., v_n' \in
V$ waehlen mit $f(v_i') = w_i$. $v_i'$ ist also das Urbild von $w_i$. Wir
zeigen, dass $B = \{v_1, ..., v_m, v_1', ..., v_n'\}$ eine Basis von $V$
ist. Man bemerke: der Beweis ist sogar konstruktiv. Wir koennen nicht nur
zeigen, dass die Dimension von $V$ sich durch die Dimension des Kernes und
Bildes finden laesst. Sondern, wir finden sogar die Vektoren aus dem Kern und
(indirekt) aus dem Bild, um eine Basis fuer $V$ zu bilden.

#### Lineare Unabhaengigkeit von $B$

Wir wollen die folgende Aussage (*) zeigen: $a_1v_1 + ... + a_m v_m + b_1 v_1' +
... + b_n v_n' = 0 \Rightarrow a_1 = ... a_n = b_1 = ... b_m 0$, also dass diese
Menge von Vektoren $\{v_1, ..., v_n, v_1', ..., v_m'\}$ linear unabhaengig ist,
weil es nur die triviale Darstellung der Null fuer ihre Vektoren gibt. Das waere
der erste Schritt, um zu zeigen, dass diese Basis $B$ aus Kernbasisvektoren und
Urbildbasisvektoren eine Basis von $B$ ist.

Wir zeigen zunaechst, dass wenn $\sum_{i=1}^n a_iv_i + \sum_{i=1}^m b_i v_i' =
0$, mit Sicherheit alle $b_i = 0$ sind. Dazu wenden wir einen Trick an: Wir
wissen, dass $0 = f(0)$, da $f$ linear unabhaengig ist. Also koennen wir $0$
schreiben als: $0 = f(0) = f(\sum_{i=1}^n a_iv_i + \sum_{i=1}^m b_i v_i') =
\sum_{i=1}^n a_i f(v_i) + \sum_{i=1}^m b_i f(v_i')$. Hierbei wenden wir das
Linearitaetskriterium der Addition sowie der Skalarmultiplikation an, sodass wir
aus jeweils $f(\alpha_i\beta_i + \alpha_j\beta_j) = f(\alpha_i\beta_i) +
f(\alpha_j\beta_j) = \alpha_i f(\beta_i) + \alpha_j f(\beta_j)$ umspalten (fuer
je zwei solcher Additionsglieder).

Nun haben wir rechts also eine Summe uber $f(v_i')$. Wir haben $v_i'$ gerade so
gewaehlt, dass es das Urbild von $w_i$ ist, also: $v_i' = f^{-1}(w_i)$ bzw. $w_i
= f(v_i')$. Somit ersetzen wir nun die $f(v_i')$ durch $w_i$ und erhalten:
$\sum_{i=1}^n a_i f(v_i) + \sum_{i=1}^m b_i w_i = 0$. Dann zwei Dinge:

1. Da die $v_i$ gerade aus dem Kern von $f$ waren, gilt fuer alle $v_i: f(v_i)
   = 0$. Somit ist die linke Summe schon sicher null.
2. Es bleibt $\sum_{i=1}^m b_i w_i = 0$. Da $w_i$ aus der Basis des Bildes
   gewaehlt wurde, sind die $w_i$ insbesondere linear unabhaengig. Somit gibt es
   nur die triviale Darstellung der $0$ und daher sind auch alle $b_i = 0$.

Wir haben nun also durch diesen Trick schonmal rausgefunden, dass die $b_i$ alle
Null sind. Somit reduziert sich die zu zeigende Aussage (*) auf: $\sum_{i=1}^m
a_iv_i = 0$. Wir muessen nur mehr zeigen, dass auch die $a_i$ alle null sind. Da
wir die $v_i$ aus der Basis des Kernes gewaehlt haben, muessen auch alle $a_i =
0$ sein.

Es sind also sowohl alle $a_i$ als auch alle $b_i$ gleich null. Es existiert
also nur die triviale Darstellung der Null fuer $B$. Somit ist $B$ schon einmal
linear unabhaengig.

#### $B$ ist ein Erzeugendensystem

Um zu zeigen, dass $B$ eine Basis ist, muessen wir weiter noch zeigen, dass es
ein Erzeugendensystem ist. Wir waehlen dazu einen Vektor $v \in V$ beliebig. Da
$f(v)$ im Bild von $f$ ist, kann man $f(v)$ auch als Linerkombination der Basis
des Bildes schreiben: $f(v) = \sum_{i=1}^n b_i w_i$ mit $b_i \in K$. Wir waehlen
nun geeignet $\tilde{v} = v - \sum_{i=1}^n b_i v_i'$.

Wenn wir nun naemlich $f$ auf $\tilde{v}$ anwenden, dann erhalten wir links und
rechts denselben Ausdruck. Zunaechst haben wir $f(\tilde{v}) = f(v) -
\sum_{i=1}^n b_i f(v_i)$ (wobei wir fuer das rechte Glied wieder das
Linearitaetskriterium anwenden). Dann ist $f(v)$ eben der obige Ausdruck (die
Linearkombination), und $f(v_i) = w_i \forall i$. Somit steht am Ende da:

$$f(\tilde{v}) = \sum_{i=1}^n b_i w_i - \sum_{i=1}^n b_i w_i = 0.$$

Somit ist also $\tilde{v}$ im Kern von $f$. Wenn $\tilde{v}$ im Kern ist,
koennen wir es also als Linearkombination der Basisvektoren des Kernes
ausdruecken. Es existieren also Koeffizienten $a_1, ..., a_m \in K$, sodass
$\tilde{v} = a_1v_1 + ... + a_m v_m$. Somit haben wir also nach obiger
Definition von $\tilde{v}$ einen neuen Ausdruck fuer $v$:

1. $v = \tilde{v} + \sum_{i=1}^n a_i v_i'$
2. $v = \sum_{i=1}^m a_i v_i + \sum_{i=1}^n b_i v_i'$.

Wir haben nun also gezeigt, dass wir *jeden* Vektor $v \in V$ (wir haben $v$ ja
beliebig gewaehlt) durch Definition eines geeigneten $\tilde{v}$ als
Linearkombination der Basen des Kernes sowie des Bildes definieren
koennen. Somit ist also diese Menge $B = \{v_1, ..., v_m, v_1', ..., v_n'\}$ ein
Erzeugendenssystem von $V$.

Insgesamt haben wir also ein linear unabhaengiges Erzeugendenssystem, also eine
Basis von $V$. Insofern gilt also:

$\dim(V) = |B| = \dim(\Kern(f)) + \dim(\Bild(f))$

### Korrolar

Sei $A \in K^{m \times n}$ und $f_A: K^n \rightarrow K^m, v \mapsto A \cdot v$
die entsprechende Matrizenabbildung. Fuer diese Abbildung ist $f_A^{-1}(b)$ die
Loesungsmenge von $Ax = b$. Dann beobachten wir:

*Der Kern $\Kern(f_A)$ hat die Dimension $n - \text{rang}(A)$ (Anzahl an
Freiheitsgraden).*

Der Kern dieser Funktion enthaelt also die Loesungen des homogenen linearen
Gleichungssystems $Ax = 0$. Fuer diese Menge gibt es zwei Moeglichkeiten:

1. Sie enthaelt nur den Nullvektor. Dann ist $\Kern(f_A) = \{0\}$. Nach
   Definition ist die Dimension des Nullraums die leere Menge, also stimmt es
   dass der Kern dann Dimension $n - \text{rang}(A)$. Hierbei ist
   $\text{rang}(A)$ ja $n$, weil das LGS eine eindeutige Loesung hat ($A$ ist
   regulaer).

2. Sie enthaelt unendlich viele Loesungen. Dann ist die Kardinalitaet der Basis
   dieser Loesungsmenge genau gleich der Anzahl an Freiheitsgraden. Fuer jeden
   Freiheitsgrad erhalten wir einen neuen Basisvektor. Es gilt also wieder, dass
   die Dimension gleich $n - \text{rang}(A)$ ist.

Betrachten wir jetzt wieder die Aussage, dass fuer eine Abbildung $f: V
\rightarrow W$ die Dimension des Vektorraums $\dim(V)$ gleich
$\dim(\Kern(f)) + \dim(\Bild(f))$ ist. Wir haben fuer
$f_A$ also, wo $V = K^n$: $\dim(K^n) = \dim(\Kern(f_A)) +
\dim(\Bild(f_A))$ ist. Wir wissen, dass $\dim(K^n) = n$. Also
schreiben wir $n = \dim(\Kern(f_A)) + \dim(\Bild(f_A)).$

Oben haben wir festgestellt, dass die Dimension des Kernes
$\dim(\Kern(f_A))$ gleich $n - \text{rang}(A)$ ist. Nun konnen wir
also einsetzen:

1. $\dim(K^n) = \dim(\Kern(f_A)) + \dim(\Bild(f_A))$
2. $n = n - \text{rang}(A) + \dim(\Bild(f_A))$
3. $\text{rang}(A) = \dim(\Bild(f_A))$.

Wir bemerken weiter, dass das Bild von $f_A$ die Menge aller "rechten Seiten",
bzw. genauer, *die Menge aller Linearkombinationen der Spalten von $A$ ist: $Ax
= b \Rightarrow x_1 \cdot (a_{1,1}, ..., a_{n,1})^\top + ... + x_n \cdot
(a_{n,1}, ..., a_{n,n})^\top = (b_1, ..., b_n)^\top$

Somit bildet das Bild von $f_A$ also alle moeglichen $b$. Daher sind die Spalten
der Matrix $A$ also ein *Erzeugendensystem* vom von den Spalten aufgespannten
Vektorraum. Das Bild bzw. diese Menge aller linearen Kombinationen der Spalten
sind also gerade das Erzeugnis der Spalten.

Daher der Satz:

__Der Rang einer Matrix $A \in K^{m \times n}$ ist die Dimension des von den
Spalten aufgespannten Untervektorraums von $K^m$.__

Nun betrachten wir wieder die obige Beobachtung, dass $\text{rang}(A) =
\dim(\Bild(f_A))$. Genauer betrachten wir zwei Faelle:

1. Die Matrix hat vollen Rang (ist regulaer), dann gilt also $\text{rang}(A) =
   n$. Somit also auch $\dim(\Bild(f_A)) = n$, wobei $n$ gerade die
   Kardinalitaet der Spaltenmenge, also unseres Erzeugendensystems ist. Was hier
   gerade steht, ist dass wenn die Matrix vollen Rang hat, die Spalten eine
   *Basis* des aufgespannten Untervektorraums von $K^m$ sind. Vor allem haben
   wir, wenn die Matrix quadratisch ist, eine Basis des $K^n = K^m$
   gefunden. Zum Beispiel wenn die Matrix die Identitaetsmatrix ist, haben wir
   den ganzen $K^n$ als Bild.

2. Die Matrix hat nicht vollen Rang, sodass also $\text{rang}(A) < n$. Dann
   heisst das drei Dinge:
   1. Das homogene LGS $Ax = 0$ hat mehr als eine Loesung.
   2. Die Zeilen sind nicht linear unabhaengig. Genauer: es gibt: $n -
      \text{rang}(A)$ linear abhaengige Zeilen (Freiheitsgrade).
   3. Da $\text{rang}(A) = \dim(\Bild(f_A)) < n$, gelten also drei
      Sachen:
	  1. Die Spalten sind auch nicht linear unabhaengig.
	  2. Daher sind die Spalten keine Basis, sondern nur ein Erzeugendensystem
         des aufgespannten Untervektorraums von $K^m$. Das gilt aber auch nur,
         wenn der Rang zwar nicht $n$, aber zumindest $\geq m$ ist. Wenn $m < n$
         haben wir also, dass wenn der Rang gleich $m$ ist, die Spalten
         ein Erzeugendensystem feur den $K^m$ sind und auch eine Basis
         enthalten. Wenn der Rang aber auch hier nicht $m$ ist, dann haben wir
         nicht mal ein Erzeugendensystem fuer den $K^m$.
	  3. Es sind ebenso $n - \text{rang}(A) = n - \dim(\Bild(f_A))$
         Spalten linear abhaengig.

Also gilt:

1. Der Spaltenrang (Dimension des Bildes) ist immer gleich dem Zeilenrang (Rang
   der Matrix).

2. Die Anzahl der linear unabhaengigen Zeilen entspricht also der Anzahl der
   linear unabhaengigen Spalten.

Man beachte wann die Spalten einen Untervektorraum von $K^m$ aufspannen, und
wann den ganzen $K^m$. Wenn:

1. $m > n$: Dann koennen die Spalten sicher nur ein Untervektorraum vom $K^m$
   aufspannen (nicht den ganzen $K^m$), weil wir nur $n < m$ Spalten haben. Der
   $K^m$ hat aber eine Basis von $m$ Vektoren. Welchen Untervektorraum sie
   aufspannen, haengt wieder vom Rang der Zeilen (bzw. Spalten) ab.
2. $m = n$: Dann kommt es darauf an, was der Rang der Matrix ist. Ist
   $\text{rang}(A) = n = m$, dann sind sowohl die Zeilen (!) als auch die
   Spalten eine Basis des $K^n = K^m$. Ist $\text{rang}(A) < n$, dann spannen
   die Spalten und Zeilen nur einen Untervektorraum des $K^n$ auf. Bringt man
   die Matrix in Zeilenstufenform, findet man die Kardinalitaet der Basis dieses
   aufgespannten Raumes.
3. $m < n$: Dann koennen die Spalten den $K^m$ aufspannen. Ob sie es tun, haengt
   wie immer vom Rang ab.

### Korrolar

Ein Korrolar vom obigen ist das folgende: Sei $f: V \rightarrow W$ eine lineare
Abbildung, mit der Eigenschaft, dass $\dim(V) = \dim(W) < \infty$,
dass also sowohl Urbildraum als auch Bildraum die selbe Dimension haben
(z.B. wenn die Funktion endomorph ist, also von und in denselben Raum
abbildet). Dann gilt:

$f$ ist isomorph $\Leftrightarrow$ $f$ ist injektiv $\Leftrightarrow$ $f$ ist
surjektiv.

Bijektiv, injektiv und surjektiv waeren also das Selbe. Fuer den Beweis, dass
$f$ isomorph ist, genuegt es zu zeigen, dass $f$ injektiv und surjektiv ist. Wir
gehen zuerst in die eine Richtung: $f \text{ injektiv } \Rightarrow f \text{
surjektiv }$:

1. Wenn $f$ injektiv ist, dann ist sein Kern also der Nullraum: $\Kern(f)
   = \{0\}$, dessen Dimension bekanntlich null ist: $\dim(\Kern(f))
   = 0$.
2. Weiters wissen wir aus dem Dimensionssatz, dass $\dim(\text{Bild(f)}) =
   \dim(V) - \dim(\text{Kern(f)})$. Da wir annehmen, dass
   $\dim(V) = \dim(W)$, koennen wir auch schreiben:
   $\dim(\text{Bild(f)}) = \dim(W) -
   \dim(\text{Kern(f)})$. Weiters wissen wir ja, dass der Kern die
   Dimension Null hat. Also haben wir: $\dim(\text{Bild(f)}) =
   \dim(W)$.
3. $f$ ist also genau dann injektiv, wenn $\dim(\Bild(f)) =
   \dim(W)$.
4. Das ist __genau dann__ ($\Leftrightarrow$) der Fall, wenn $\Bild(f) =
   W$. Das folgt zum einen aus der Definition des Bildes, aber zum andern auch
   unserer allgemeinen Beobachtung, dass wenn $U \subseteq V$, wo hier $U$ das
   Bild und $V$ der "Bildraum" $W$ ist, und wenn $\dim(U) =
   \dim(V)$, dann muss $U = V$. Denn wenn wir eine inklusionsmaximale
   linear unabhaengige Teilmenge aus $U$ haben, dann sind diese Vektoren auch in
   $V$. Und somit haben wir eine Basis in $V$.
5. Das ist wiederum genau dann der Fall, wenn $f$ surjektiv ist.

Liest man diesen Beweis rueckwarts, erhaelt man auch die andere Richtung
(surjektiv $\Rightarrow$ injektiv).

Wir koennen daher fuer eine Matrix $A \in K^{n \times n}$ und dann die Abbildung
$f_A$ folgendes sagen:

1. Ist der Rang der Matrix gleich $n$ (ist die Matrix regulaer), dann gibt es
   also nur die triviale Loesung des homogenen Gleichungssystems $f_A(x) = Ax =
   0$ und damit ist der Kern der Funktion der Nullraum.
2. Wenn der Kern der Nullraum ist, ist die Funktion injektiv.
3. Dann ist sie also surjektiv.
4. Daher ist sie ein Isomorphismus.

Hierbei ist wichtig, dass die Matrix quadratisch ist. Denn dann bildet sie
Vektoren aus dem $K^n$, die ja gleich viele Komponenten haben muessen, wie $A$
Spalten hat, auf Vektoren im $K^n$ ab, da wir ja immer eine $n \times 1 = n$
Matrix bzw. Vektor erhalten. Allgemein gilt ja, dass $f_A$ fuer $A \in K^{m
\times n}$ eine Abbildung von Vektoren im $K^n$ auf Vektoren im $K^n$ ist, also
$f_A: K^n \rightarrow K^m$.

## Inverse einer Matrix

Es gibt einen Algorithmus fuer das Invertieren einer Matrix, den wir hier
betrachten wollen. Invertieren heisst allgemein, dass wir fuer eine Matrix $A$
die inverse Matrix $A^{-1}$ finden wollen.

Input: $A \in K^{n \times n}$ (__quadratische Matrix__)
Output: $B \in K^{n \times n}$, sodass $A \cdot B = I_n$ wie auch $BA = I_n$.

1. Bilde die Matrix $(A | I_n) \in K^{n \times 2n}$
2. Fuehre $(A | I_n)$ in die *strenge* Zeilenstufenform (im Teil von A) ueber,
   sodass in jeder Zeile $\neq 0$ der erste Eintrag __eine 1__ ist.
3. Falls die Zeilenstufenform die Form $(I_n | B)$ mit $B \in K^{n \times n}$,
   dann ist $B$ unsere inverse Matrix. Sonst ist der Rang $\text{rang}(A) < n$
   und dann *gibt es keine inverse Matrix* mit $A \cdot B = I_n$.

Man merkt also zwei Sachen:

1. Nicht jede Matrix ist invertierbar. Der obige Algorithmus sagt uns aber
   eindeutig, ob eine Matrix invertierbar ist, oder nicht (und ist gleichzeitig
   konstruktiv).
2. Das Matrixprodukt ist im Allgemeinen nicht kommutativ aber fuer Matrizen und
   ihre Inverse: $AA^{-1} = A^{-1}A = I_n$. Beweis:
   $B(AB)A = B \cdot I_n \cdot A = BA$. Gleichzeitig koennen wir aber wegen
   Assoziativitaet auch anders klammern: $B(AB)A = (BA)(BA)$. Somit muss also
   gelten, dass $(BA)(BA) = BA$. Das kann nur genau dann sein, wenn $BA = I_n$.

Wieso ist dier Algorithmus korrekt? Die angegebene Umformung loest *gleichzeitg*
alle LGS $Ax = e_i$ wo $e_i$ der $i$-te Standardbasisvektor ist, also die $i$-te
Spalte der Identitaetsmatrix. Im Fall $(I_n | B)$ war die Matrix $A$ also
regulaer und man hat die eindeutige Loesbarkeit. Die Spalten von $B$ sind dann
jeweils die Loesungsvektoren.

Wir loesen also gleichzeitig LGS fuer jeden Standardvektor. D.h. wir loesen
parallel $Ax = e_1, Ax = e_2, Ax = e_3, ..., Ax = e_n$ und finden also die
eindeutigen Vektoren (wenn $A$ also regulaer), die jeweils eine Loesung fuer
einen solchen Standardvektor sind. Wenn wir diese Loesungen in einer Matrix
zusammenfassen, haben wir die Inverse. Wenn wir naemlich beobachten, was wir bei
einer Matrixmultiplikation $AB$ genau machen, bemerken wir, dass wir fuer jede
Spalte in $B$ eine neue Ausgabespalte erhalten. Diese entsteht gerade dadurch,
dass wir jede Zeile in $A$ mit dieser Spalte multiplizieren. Das ist also
wiederum genau so, als ob wir $Ax$ rechnen wuerden, fuer jede Spalte $x$. Da
jede Spalte fuer den entsprechenden Standardvektor loest, erhalten wir so bei
der Multiplikation alo die Standardvektoren und letztendlich die
Identitaetsmatrix. Das ist genau dass, was wir wollten: eine Matrix $B$, sodass
die Multiplikation mit $A$ gleich der Identitaetsmatrix ist.

Bemerkung: $A^{n \times n}$ ist also genau dann invertierbar, wenn $A$ regulaer
ist (nur dann gibt es diese eindeutige Loesbarkeit fuer die
Standardvektoren). Bei der Invertierbarkeit ist also immer quadratisch
notwendig.

Zur Probe dann natuerlich einfach $A \cdot B$ rechnen und sehen, ob die
Einheitsmatrix rauskommt.

## Lineare Fortsetzung

Satz: Sei $B = \{v_1, ..., v_n\}$ eine Basis von $V$.

(a) Eine lineare Abbildung $f: V \rightarrow W$ ist durch die Bilder der
    Basisvektoren $v_i$ eindeutig bestimmt. D.h. es gibt nur eine Abbildung, die
    eine Basis auf ein bestimmtes Bild abbildet. Ist $g: V \rightarrow W$ eine
    weitere Abbildung mit demselben Basisbild, sodass also alle Basisvektoren
    auf dasselbe abgebildet werden ($f(v_i) = g(v_i) \forall i$), so muss $f =
    g$.

(b) Seien $w_1, ..., w_n \in W$ beliebig. Dann gibt es eine Abbildung $f: V
	\rightarrow W$ mit $f(v_i) = w_i \forall i$.

Interpretation: Man kann eine lineare Abbildung eindeutig definieren durch
Angabe der Bilder der Basisvektoren. Daraus ergibt sich die Information worauf
alle Elemente der Definitionsbereichs abbilden. Das nennt man das Prinzip der
linearen Fortsetztung.

### Beweis fuer (a)

Betrachten wir zwei lineare Abbildungen $f, g: V \rightarrow W$. Dann wollen wir
zeigen, dass wenn die Abbildungen die Basisvektoren alle jeweils auf dasselbe
abbilden, die Abbildungen $f, g$ gleich sein muessen.

Es gelte also, dass $f(v_i) = g(v_i) \forall i$ wo $v_i$ die Basisvektoren
sind. Dann sei $v \in V$ ein beliebieger (Nicht-Basis-)Vektor. Dieser laesst
sich als Linearkombination der Basisvektoren darstellen, sodass $v =
\sum_{i=1}^n a_iv_i$. Dann koennen wir die Linearitaet der Abbildungen
ausnutzen:

1. $f(v) = f(\sum_{i=1}^n a_iv_i)$ (Linearitaet)
2. $\sum_{i=1}^n a_i f(v_i)$ (Annahme)
3. $\sum_{i=1}^n a_i g(v_i)$
4. $g(\sum_{i=1}^n a_i v_i) = g(v)$

$f$ und $g$ bilden also nicht nur alle Basisvektoren, sondern sogar alle
Vektoren auf dasselbe ab. Folglich muessen $f$ und $g$ dieselbe Funktion sein.

### Beweis fuer (b)

Wir wollen also fuer beliebige Vektoren $w_1, ..., w_n$ im Bild $W$ zeigen, dass
es eine Abbildun gibt, die die Basisvektoren von $V$ auf genau diese Vektoren
abbildet.

Definiere hierfuer eine Abbildung $f: V \rightarrow W$ so, dass es einen Vektor
$v$ gibt, sodass $v = \sum_{i=1}^n a_i v_i$. Dann sei die $f(v_i) = w_i$ und wir
erhalten: $f(v) = \sum_{i=1}^n a_i f(v_i) = \sum_{i=1}^n a_i w_i$. Da wir jeden
Vektor $v$ *eindeutig* durch die Basisvektoren $v_i$ darstellen koennen, ist
diese Funktion sicher wohldefiniert. Die Linearitaet kann man dann explizit
nachkontrollieren.

## Darstellungsmatrizen

Sei $K$ ein Koerper, $V, W$ zwei endlich-dimensionale Vektorraeume, $B = \{v_1,
..., v_n\}$ eine Basis von $V$ und $C = \{w_1, ..., w_m\}$ eine Basis von
$W$. __Die Reihenfolge der Basen wird jetzt wichtig__. Wir sollten also eher
Tupel von Basisvektoren betrachten, anstatt Mengen (im Kopf; tun wir nicht, weil
notationell bloed).

Sei $f: V \rightarrow W$ eine lineare Abbildung. Fuer alle Indizes $j \in {1,
..., n}$ fuer Basisvektoren $v_j$ aus $B$ kann man ihr Bild ausdruecken als:

$$f(v_j) = \sum_{i=1}^m a_{i,j} w_i \text{ mit } a_{i,j} \in K.$$

Wir bilden also jeden Basisvektor $v_j$ auf einen Vektor in $W$ ab. Da das also
ein Vektor in $W$ ist, koennen wir ihn als Linearkombination der $m$
Basisvektoren $w_i$ von $W$ darstellen. Wenn wir die entsprechenden Koeffzienten
$a_{i,j}$ fuer den $j$-ten Basisvektor aus $V$ und den $i$-ten Koeffzienten der
Linearkombination in einen Vektor packen, dann haben wir geraden den
Koordinatenvektor des Bildes von $v_j$ bezueglich der Basis $C$.

Wenn wir beispielsweise den Vektor $v_1 = (a_1, a_2) \in V$ auf einen Vektor
$(b_1, b_2, b_3, b_4) \in W$ abbilden, so koennen wir diesen Bildvektor also
durch die Koeffizienten der Linearkombination mit den Basisvektoren aus $C$
ausdruecken. Dass waeren dann also vier Koeffzienten, die wir in diesem Kontext
*Koordinaten* nenen.

Wir erhalten also fuer jeden der $n$ Basisvektoren $v_j$ einen solchen
Koordinatenvektor. Dann koenenn wir diese in einer Matrix $A = (a_{i,j}) \in
K^{m \times n}$ zusammenfassen. Diese haette also fuer jeden der $n$
Basisvektorn aus $V$ einen $m$-dimensionalen Koordinatenvektor fuer die
Linearkombination der Basisvektoren von $C$. Diese Matrix $A$ nennt man nun
*Darstellungsmatrix*. Man notiert sie als $D_{B,C}(f) \in \mathbb{m \times n}$
bezueglich Basen $B, C$. Hierbei bilden wir also Basisvektoren aus dem linken
Subskript auf Vektoren im rechten Subskript ab. Wenn $V = W$, dann betrachtet
man nur eine Basis $B$ und schreibt $D_B(f)$.

Da wir oben gezeigt habe, dass jede Menge von Vektoren $w_1, ..., w_n \in W$
eine lineare Abbildung definiert, die die Basisvektoren aus $V$ auf diese $w_i$
abbildet, koennen wir auch sagen, dass jede beliebige Matrix auch eine
Darstellungsmatrix irgendeiner Abbildung ist. Dabei sind die Spalten der
Vektoren also einfach die Koordinaten der $w_i$ bezueglich der Basis $W$. Es ist
gerade hier sehr wichtig, dass man die Spalten nicht vertauscht.

Eine weitere Eigenschaft ist, dass wenn wir in die Standardbasis eines Raumes
abbilden, wir hinterher die Koordinaten nicht explizit finden muessen. Denn wenn
wir abbilden, geben wir die Komponenten des Vektors sowieso schon zur
Standardbasis an. Also sind die Komponenten dann auch schon die Koordinaten.

### Zusammenhang von linearen Abbildungen und Matrizen

Satz: Sei $V = K^n, W = K^m$ mit Standardbasen $B, C$ und sei $f: V \rightarrow
W$ eine lineare Abbildung. Dann gilt, dass $f = f_{D_{B,C}}$. D.h. die Abbildung
eines Vektors durch $f$ ist aequivalent zur Multiplikation mit der
Darstellungsmatrix. Beweis: explizites Nachrechnen:

1. Sei $v \in V$.
2. $v = \sum_{j=1}^n x_je_j$ (Linearkombination in $B$)
3. $f(v) = \sum_{j=1}^n x_j f(e_j)$
4. $f(e_j) = \sum_{i=1}^m a_{i,j} e_i$
5. $f(v) = \sum_{j=1}^n x_j (\sum_{i=1}^m a_{i,j} e_i)$. Nun refactoren wir das
   Summieren ueber die Zeilen (also die Linearkombinationen von $C$) nach
   draussen. Gleichzeitig ziehen wir die Multiplikation von $x_j$ nach innen,
   sodass wir also fuer die Linearkombination von $C$ haetten: $\sum_{i=1}^m
   x_j a_{i,j} e_i$
6. $\sum_{i=1}^m (\sum_{j=1}^n x_j a_{i,j}) e_i$
7. Das ist nun genau $Av$.

Ueberlegen wir uns, was wir bei $D_{B,C} \cdot x$ ueberhaupt machen. Sei dazu $A
:= D_{B,C}$. Wenn wir das expandieren, multiplizieren wir die Komponenten von
$x$ ja mit den Spalten, erhalten also:

$$x_1a_1 + x_2a_2 + ... + x_na_n$$,

wo $a_i$ die Spalten von $A$ sind. Was wir hier eigentlich tun, ist keine
Linearkombination von den Koordinaten $x_i$ mit den Basisvektoren zu bilden,
sondern eben mit den Koordinaten der Bilder der Basisvektoren. Es ist also
aehnlich zur Linearkombination $x = \sum_{i=1}^n x_ib_i$ wo $b_i$ die
Basisvektoren aus $B$ sind. Aber bei $D_{B,C} \cdot x$ addieren wir einfach die
Koordinaten der Bilder dieser Basisvektoren zusammen. Daraus ergibt sich dann
der Bildvektor fuer $x$.

### Komposition von Abbildungen und Matrizen

Seien $U, V, W$ endlich-dimensionale $K$-Vektorraeume mit Basen $A, B, C$. Seien
dann weiters $f: U \rightarrow V, g: V \rightarrow W$ lineare Abbildungen. Dann
gilt:

$$D_{A,C}(g \circ f) = D_{B,C}(g) \cdot D_{A,B}(f)$$

Man beachte, dass so wie die Evaluation von Funktionen bei der Komposition von
rechts nach links geht, sodass $(f \circ g)(x) = f(g(x))$, geht hier die
Auflistung der Darstellungsmatrizen auch von rechts nach links. Es macht dann
aber auch Sinn, wenn man einen Vektor einsetzt:

1. $(g \circ f)(x) = g(f(x))$
2. $D_{A,C}(g \circ f) \cdot x = (D_{B,C}(g) \cdot D_{A,B}(f)) \cdot x$. Wegen
   Assoziativitaet duerfen wir nun anders klammern.
3. $D_{B,C}(g) \cdot (D_{A,B}(f) \cdot x)$. Nun haben wir also das erste Produkt,
   was gerade der Abbildung von $U$ (zur Basis $A$) nach $V$ (zur Basis $B$)
   entspricht. Wir erhalten als Ausgabe einen Wert $x'$ zur Basis $B$.
4. $D_{B,C}(g) \cdot x$ ist nun die zweite Funktionsanwendung, welche den Vektor
   zur Basis $B$ auf einen finalen Vektor zur Basis $C$ abbildet. Das ist dann
   das Endergebnis.

#### Beispiel

Betrachten wir zwei lineare Abbildungen

1. $f: \mathbb{R}^2 \rightarrow \mathbb{R}: (x_1, x_2)^\top \mapsto 3x_1$
2. $g: \mathbb{R} \rightarrow \mathbb{R}^3: x \mapsto (x, x, x)^\top$

Sowie die Basen:

* Fuer $U$: $A := I_2$
* Fuer $V$: $B := (1)$
* Fuer $W$: $C := I_3$

Dann wollen wir die Verbindung zwischen $g \circ f: \mathbb{R}^3 \rightarrow
\mathbb{R}^2$ und $D_{A,C}(g \circ f)$untersuchen.

Hierzu finden wir zunaechst die Darstellungsmatrizen, indem wir die
Basisvektoren jeweils durch die jeweilige Abbildung durchschieben und die
Koordinaten der Bilder notieren. So erhalten wir:

* $D_{A,B}(f) = (3 0) \in \mathbb{R}^{1 \times 2}$
* $D_{B,C}(g) = (1, 1, 1)^\top \in \mathbb{R}^{3 \times 1}$

Allgemein sei angemerkt, dass wenn wir von einem $n$-dimensionalen Raum in einen
$m$-dimensionalen abbilden, die Darstellungsmatrix $\in K^{m \times n}$ sein
wird, weil wir $n$ Spalten bzw. Komponenten brauchen und $m$ Koordinaten im
Ausgabevektor erhalten.

Dann muesste also $D_{A,C}(g \circ f) = D_{B,C}(g) \cdot D_{A,B}(f)$ sein. Das
waere dann:

$$
\begin{bmatrix}
1 \\
1 \\
1 \\
\end{bmatrix}

\cdot

\begin{bmatrix}
3 & 0
\end{bmatrix}

=

\begin{bmatrix}
3 & 0 \\
3 & 0 \\
3 & 0 \\
\end{bmatrix}
$$

Man beachte, dass $D_{A,C}(g \circ f) \in \mathbb{R}^{3 \times 2}$. Nun koennen
wir pruefen, ob $(g \circ f)(x)$ tatsaechlich gleich $D_{A,C}(g \circ f) \cdot
x$ ist. Sei hierzu $x = (3, 2)^\top$. Dann einmal fuer die direkte Abbildung:

1. $(g \circ f)(x) = g(f(x)$. Nun selektiert $f(x)$ dreimal die erste
   Komponenten von $x$, also erhalten wir $f(x) = 9$. Dann:
2. $g(9) = (9, 9, 9)^\top$

Und nun mit der Matrix:

$$
\begin{bmatrix}
3 & 0 \\
3 & 0 \\
3 & 0 \\
\end{bmatrix}

\cdot

\begin{bmatrix}
3 & 2
\end{bmatrix}

=

\begin{bmatrix}
9 \\
9 \\
9 \\
\end{bmatrix}
$$

Wir selektieren also jeweils $3$ mal die erste Komponente und addieren dann
nichts von der zweiten dazu. Es scheint also zu funktionieren.

### Eigenschaften

Seien $A \in \mathbb{K}^{l \times n}$, $B \in \mathbb{K}^{m \times n}$ zwei
Matrizen und $f_A: K^m \rightarrow K^l, f_B: K^n \rightarrow K^m$ die
zuegehoreigen Matrizenabbildungen. Dann gilt

$$f_A \circ f_B = f_{AB}.$$

Das folgt daraus, dass die Darstellungsmatrizen von $f_A, f_B$ gerade $A,B$
selbst sind und wir wissen ja, dass $f_A \circ f_B$ gleich der Multiplikation
der Darstellungsmatrizen ist, also $A \cdot B$. Das ist dann also die
Darstellungsmatrix fuer die resultierende Funktion, eben $f_{A \cdot B}$.

Betrachten wir im Weiteren nur noch $A$. Sagen wir dann, dass $A \in K^{n \times
n}$ invertierbar  (und daher notwendigerweise quadratisch) ist. So folgt:

$$f_{A^{-1}} \circ f_A = f_{A^{-1} \cdot A} = f_{I_n} = Id_{K^n}$$

Also muss die Abbildung $f_{A^{-1}}$, dessen Darstellungsmatrix das Inverse von
$A$ ist, die Umkehrabbildung $f^{-1}_A$ von $f_A$ sein. Man beachte aber, dass
$A^{-1}$ nur bezueglich der Standardbasis die Darstellungsmatrix von
$f^{-1}_A$. Es sei allemein angemerkt, dass $A$ auch *nur bezueglich der
Standardbasis* ueberhaupt Darstellungsmatrix von $f_A$ ist. Das finden der
Darstellungsmatrix bei einer Matrizenabbildung wie $f_A$ ist ja aequivalent zur
Multiplikation der Basis, aus welcher wir abbilden, mit der Matrix $A$. Dann
muss man die resultierenden Spalten noch eventuell in die Koordinaten der
Zielbasis umformen. Nur wenn die Quellbasis nun die Standardbasis ist, als
Matrix also die Identitaetsmatrix ist, erhaelt diese Multiplikation dann die
Matrix $A$. Und nur wenn die Zielbasis dann auch die Standardbasis ist, muessen
wir das Ergebnis, also $A$, nicht weiter in andere Koordinaten umformen. Somit
bleibt also nur fuer die Standardbasis die Matrix $A$ selbst die
Darstellunsgmatrix. Insofern ist auch nur bezueglich der Standardbasis dann
$A^{-1}$ die Darstellungsmatrix von $f^{-1}_A = f_{A^{-1}}$.

Aus diesen Eigenschaften koennen wir nun die Eindeutigkeit der inversen Matrizen
folgern. Wir wussten das zwar vorhin schon, weil das loesen der
Gleichungssysteme nach der Einheitsmatrix eindeutig ist, aber jetzt koennen wir
das auch auf andere Weise folgern. Denn da $f_A$ invertierbar ist, ist es
eindeutig bijektiv. Wir wissen naemlich:

$f^{-1}_A \circ f_A = Id_{K^n} = f_A \circ f^{-1}_A$

Da wir die Darstellungsmatrizen dieser Funktionen kennen, folgt hieraus:

$$A^{-1} \cdot A = I_n = A \cdot A^{-1}$$

#### General Linear Group

Die invertierbaren Matrizen erfuellen nach obiger Bemerkung und den explizit
besprochenen Operationen auf Matrizen als Menge alle Eigenschaften einer
Gruppe. Diese Gruppe nennt man *general linear Group*:

$$GL_n(K) = \{A \in K^{n \times n} \,|\, A \text{ invertierbar }\}$$

Wir koennen ja nun die Eigenschaften ueberpruefen:

1. Abgeschlossenheit (Algebra): $A^{-1}B^{-1} = (BA)^{-1}$, also wieder
   invertierbar.
2. Assoziativitaet (Halbgruppe): Gilt fuer Matrizen allgemein.
3. Neutrales Element (Monoid): Identiaetsmatrix.
4. Inverses Element (Gruppe): Die (invertierte invertierte) Matrix: $A$

## Basiswechsel

Die Basen von Vektorraeumen koennen sich unterscheiden und manche Basen sind
fuer die Darstellung von Daten (Menge von Vektoren) geeigneter als andere. Was
passiert mit der Darstellungsmatrix einer linearen Abbildung $V \rightarrow V$
bei einem Basisewechsel?

Seien hierzu $B = \{v_1, ..., v_n\}$ und $B' = \{v_1', ..., v_n'\}$ zwei Basen
von $V$.

Da $B$ eine Basis ist, kann man also alle Elemente von $B'$ ueber $B$
ausdruecken (und auch umgekehrt). Fuer einen Basisvektor $v_j'$ aus $B'$
erhalten wir so:

$$v_j' = \sum_{i=1}^n a_{i,j} \cdot v_i$$ mit $a_{i,j} \in K$.

Wenn wir die Koeffizienten $a_{i,j}$ in einen Spaltenvektor geben, erhalten wir
so (wie fuer die Darstellungsmatrix) eine solche Spalte pro Basisvektor aus
$B'$. Geben wir all diese Spalten in eine Matrix

$S = S_{B,B'} = (a_{i,j}) \in K^{n \times n}$

so nennt man diese Matrix *Basiswechselmatrix* oder
*Transformationsmatrix*. Diese kann man benutzen, um den Uebergang von $B'$ nach
$B$ zu beschreiben. Die Spalten von $S$ sind die Koordinatenvektoren von $B'$
__geschrieben in der ersten Basis $B$__.

Das ganze geht natuerlich auch umgekehrt. $B'$ ist eine Basis von $V$, also kann
man die Elemente von $B$ als Linearkombination von $B'$ ausdruecken. Sei hierzu
$v_j$ ein Basisvektor aus $B$, dann haben wir:

$$v_j = \sum_{i=1}^n b_{i,j}v_i'$$ mit $b_{i,j} \in K$.

Diese Matrix $T = (b_{i,j}) \in K^{n \times n}$ ist nun die zugehoerige
(umgekehrte) Basiswechselmatrix. Betrachten wir nun einmal ein solches
Basiselement $v_j \in B$, und wie es sich darstellen laesst:

1. $v_j = \sum_{i=1}^n b_{i,j}v_i'$ ($v_j$ laesst sich durch $B'$ darstellen)
2. $v_j = \sum_{i=1}^n b_{i,j}(\sum_{i=1}^n a_{k,i} v_k)$ (Jedes $v_i'$ laesst
sich durch $B$ darstellen)
3. $\sum_{i=1}^n(\sum_{i=1}^n a_{k,i} b_{i,j})v_k$ (verschieben der Summen und Terme)

Wir haben also so ein Element zur Basis $B$. Dann koennen wir dieses also als
Linaerkombination seiner eigenen Basis darstellen (wobei nur $v_k$ fuer $v_k$
ungleich Null sein wird). Insbesondere koenenn wir jedes $v_j$ aber als
Koordinaten von $B'$ angeben. Dazu hatten wir $T$ und deswegen haben wir hier
$b_{i,j}$, also fuer die Vektoren $v_i'$. Nun koennen wir jeden Basisvektor von
$T$ in Koordinaten von $B$ angeben. Hierzu hatten wir $S$ und deswegen
multiplizieren wir $b_{i,j}$ mit $a_{k,i}$. Das ist dann naemlich die Summe
ueber alle Koordinaten jedes Basisvektors aus $B'$ (durch welche wir $v_j$)
ausdruecken, bezueglich dem $k$-ten Basisvektor aus $B$. Das ergibt dann
naemlich die Summe ueber die $k$-te Zeile von $S$ (alle Koordinaten der
verschiedenen Basisvektoren aus $B'$ bezueglich $v_k$) mal die $j$-te Spalte
(die Koordinaten von $v_j$ bezueglich $B'$, also bezueglich der $v_i'$, ueber
deseen Koordinaten wir in $S$ summieren). Somit ist dieses Skalarprodukt der
Zeile in $S$ mal die entsprechende Zeile in $T$ gerade der Eintrag in $S \cdot
T$.

Wir haben also, gefunden, dass wir durch Elemente von $ST$ ein Element $v_j$ aus
$B$ erhalten. Somit muss also $S \cdot T = I_n$ sein. Daraus folgt eine
intuitive Eigenschaft:

$T = S^{-1}$

bzw. genauer:

$S_{B,B'} = S_{B',B}^{-1}$

D.h. das Inverse einer Basiswechselmatrix ergibt eine neue Basiswechselmatrix,
aber einfach in die andere Richtung. Auch erkennen wir so, dass *jede*
invertierbare Matrix $S \in GL_n(K)$ einen Basiswechselbeschreibt.

Eine gewisser Basiswechsel, der wie fuer Darstellungsmatrizen einfach ist, ist
jener in die Standardbasis. Wir haben also einen beliebigen Vektorraum $V$ und
eine Basis $B$, *aus* welcher wir *in* den Standardraum wechseln wollen. Dann
ist es nun naemlich so, dass wenn wir jeden Basisvektor aus $B$ durch die
Zielbasis, also die Standardbasis, ausdruecken wollen, wir schon fertig
sind. Denn wenn wir eine Basis angeben, dann geben wir sie ja schon zur
Standardbasis an. Wenn wir schreiben, dass $(1, 1)^\top, (0, -1)^\top$ eine
Basis des $\mathbb{R}^2$ ist, dann muessen wir ja sagen, zu welcher Basis ein
Vektor der neuen Basis $(1, 1)^\top$ gemeint ist. Wenn die Basis $(5, 10)^\top,
(100, -1)^\top$ ist, dann waere $(1, 1)^\top$ ja eigentlich gerade $1 \cdot (5,
10)^\top + 1 \cdot (100, -1)^\top$. Aber implizit meinen wir, wenn wir eine
Basis angeben, immer, dass ihre Vektoren bezueglich der Standardbasis
sind. Natuerlich meinen wir mit der Basis $(1, 1)^\top, (0, -1)^\top$, dass der
eine Vektor einen Kaule nach rechts geht und einen nach oben. Und der andere
geht nur einen Kaule nach oben. Jedenfalls ist es also so, dass wir Basen schon
in Koordinaten der Standardbasis angeben. Somit haben wir nichts zu tun, wenn
wir in die Standardbasis wechseln wollen.

Eine weitere wichtige Beobachtung ist, dass wir Basiswechselmatrizen eigentlich
schon aus der Diskussion der Darstellungsmatrizen kennen. Denn was tun wir denn,
um eine Basiswechselmatrix $S_{B,C}$ zu finden? Wir stellen jeden Basisvektor
aus $C$ durch die entsprechenden Koordinaten von $B$ dar. Das ist ja gerade so,
als ob wir die Basisvektoren von $C$ durch die Identitaetsabbildung schieben,
und dann das Bild (also die Basisvektoren von $C$ selbst) durch die Koordinaten
von $B$ darstellen. Somit gilt:

$$S_{B,C} = D_{C,B}(Id)$$

Hier auch eine Anmerkung zur Notation:

1. Mit $D_{X, Y}$ meinen wir die Darstellungsmatrix von $X$ nach $Y$.
2. Mit $S_{X, Y}$ meinen wir die Basiswechselmatrix von $Y$ nach $X$ (weil wir
   Vektoren von rechts ranmultiplizieren).

Deswegen werden die Basen in obiger Gleichheit vertauscht.

### Anwendung

Wir haben nun also einen Vektor $v$ aus einem Vektorraum $V$, sowie zwei Basen
$B, B'$ dieses Vektorraums. Dann koenenn wir $v$ nun auf zwei Weisen in $V$
darstellen, naemlich einmal durch seine Koordinaten $a_B$ bezueglich $B$ und
einmal durch seine Koordinaten $a_{B'}$ bezueglich $B'$. Hierbei sind $a_B$ und
$a_{B'}$ die Koordinatenvektoren, sodass die Multiplikation von der Matrix $B$
bzw. $B'$ also genau die jeweilige Spalte von $B$ mit der entsprechenden
Koordinate in $a_B$ bzw. $a_{B'}$ darstellt ($B \cdot a_B$ ist die
Linearkombination, die $v$ ergibt). Wir koennen $v$ also darstellen in:

1. Der Basis $B$: $v = B \cdot a_B$
2. Der Basis $B'$: $v = B \cdot a_{B'}$.

Wir fragen uns nun: wenn wir $v$ in der einen Basis $B$ haben, wie kommen wir
dann zu $v$ in der anderen Basis $B'$? Hierfuer formen wir um:

1. Wir haben oben: $v = B a_B$ und $v = B' a_{B'}$.
2. Dann setzen wir gleich: $B a_B = B' a_{B'}$
3. Und tauschen um: $a_B' = (B'^{-1} \cdot B) a_B$

Genau durch diese Multiplikation kommen wir zu den Koordinaten $a_B$. Was heisst
das im Uebrigen? Wenn wir unseren Vektor in Koordinaten zu $B$ haben, also als
Vektor $a_B$ (was meistens der Fall sein wird), dann fuehrt die Multiplikation
also zum Vektor $a_{B'}$ zur Basis  $B'$. Somit muss $B'^{-1} \cdot B$ also
gerade die Basiswechselmatrix $S_{B',B}$ sein!

Allgemein seien also $B, C$ zwei Basen eines Vektorraums $V$. Dann gilt:

$$S_{B,C} = B^{-1} \cdot C$$

Wir erkennen hier auch die obige Eigenschaft, dass wenn wir in die Standardbasis
wechseln, wir keine Arbeit haben. Denn dann haetten wir:

1. $S_{I_n,C}$
2. $I_n^{-1} \cdot C$
3. $I_n \cdot C$
4. $C$

Deswegen also: wenn wir eine Basis $B$ in die Standardbasis wechseln wollen,
dann ist $B$ selbst die Basiswechselmatrix.

### Beispiel

Betrachten wir den Raum $\mathbb{R}^2$, zwei Basen $B = \{(1, 0)\top, (0,
1)^\top\}$ und $B' = \{(1, 1)^\top, (0, -1)^\top\}$ dieses Raums. Die erste
Basis ist hierbei also die Standardbasis. Dann nehmen wir noch einen Vektor $v =
(2, 3)^\top$ aus $V$ zur Standardbasis. Wir wollen nun den Basiswechsel von $B'$
nach $B$ untersuchen (Achtung: in der Vorlesung steht $B$ nach $B'$, aber das
ist nur eine unintuitive Sprechweise fuer $S_{B,B'}$, also fuer uns $B'$ nach
$B$). Wir wissen also:

$$S_{B,B'} = B^{-1} B'$$

Da hier aber $B = I_n$ die Standardbasis ist, haben wir eigentlich:

$$S_{B,B'} = B^{-1} B' = I_n^{-1} B' = B'$$

Das heisst, unsere Basiswechselmatrix ist $B'$ selbst, weil wir eben in die
Standardbasis wechseln. Der Basiswechsel in die andere Richtung, also von der
Standardbasis $B$ in die Basis $B'$ waere ein wenig spannender:

$$S_{B',B} = B'^{-1} \cdot B = B'^{-1} \cdot I_n = B'^{-1}$$

Wir finden also einfach die Inverse von $B'$. Da $B'$ hier eine $2 \times 2$
Matrix ist, gibt es die Formel:

$$B'^{-1} = \frac{1}{\text{det}(B')} \cdot \text{Adj}(B')$$

Das waere hier:

$$B'^{-1} = \frac{1}{1} \cdot \begin{bmatrix}1 & 0 \\ -1 & 1\end{bmatrix}$$

Das ergibt dann also:

$$S_{B',B} = B'^{-1} = \begin(bmatrix)1 & 0 \\ -1 & 1\end{bmatrix}$$

Wenn wir nun einen Vektor $v = (3, 2)^\top$ zur Basis $B$ (Standardbasis)
betrachten, erhalten wir sein Gegenueber zur Basis $B'$ dadurch, dass wir $v$
von rechts an $S_{B,B'} = B^{-1}$ ranmultiplizieren:

$$v' = S_{B,B'} \cdot v = (3, -1)^\top$$

Um nun von diesem Vektor $v'$ zur Basis $B'$ zurueck zum Vektor bezueglich $B$
zu kommen, multiplizieren wir $v'$ mit der anderen Basiswechselmatrix, welche ja
$B'$ selbst war:

$$v = S_{B',B} \cdot v' = B' \cdot v = (3, 2)^\top.$$

So funktioniert das Ganze mit Basisewechselmatrizen also.

### Vektorrouting

Seien $B, B'$ Basen eines endlich-dimensionalen Vektorraum $V$ und $C, C'$
Basen eines endlich-dimensionalen Vektorraums $W$. Dann gilt fuer eine lineare
Abbildung $f: V \rightarrow W$:

$$D_{B',C'}(f) = S_{C,C'}^{-1} D_{B,C}(f) S_{B,B'}$$.

Wir wollen betrachten, was das genau bedeutet. Wir suchen also die
Darstellungsmatrix $D_{B',C'}(f)$, also einen Weg von $B'$ nach $C'$ durch
unsere Funktion $f$. Konkret: fuer einen Vektor $v'_{B'}$ ($v'$ zur Basis $B'$)
suchen wir sein Bild $w'_{C'}$ zur Basis $C'$. Wir haben diese Matrix aber nicht
gegeben, sondern nur drei Informationsstucke:

1. Wir wissen, wie wir von $B'$ nach $B$ kommmen: $S_{B,B'}$.
2. Wir wissen, wie wir von $B$ nach $C$ kommen: $D_{B,C}(f)$.
3. Wir wissen, wie wir von $C'$ nach $C$ kommen: $S_{C,C'}$.

Graphisch:

```
(B) === D_{B,C} ==> (C)
 ^                   ^
 |                   |
 | S_{B,B'}          | S_{C,C'}
 |                   |
(B') == gesucht ==> (C')
```

Jetzt nehmen wir uns einen Vektor $v'_{B'}$, welchen wir auf $w'_{C'}$ abbilden
wollen. Den direct hop kennen wir nicht. Also routen wir den Vektor ueber seinen
Gateway nach $B$, denn wir kennen den Weg von $B'$ nach $B$ durch die
Basiswechselmatrix $S_{B,B'}$. Dann wissen wir fuer $B$, wie wir nach $C$
abbilden koennen. Das sagt uns naemlich gerade die Darstellungsmatrix
$D_{B,C}(f)$. bzw. lax $D_{B,C}$ Also gehen wir den Weg weiter ueber die Kante
$D_{B,C}$. Dann wissen wir letztlich auch noch, wie wir von $C$ nach $C'$
kommen, naemlich durch $S_{C,C'}^{-1}$. Man beachte: Wir kannten anfangs nur
$S_{C,C'}$. Aber dann kennen wir also auch automatisch $S_{C',C} =
S_{C,C'}^{-1}$ (das haben wir durch Koeffizientenvergleich vorhin gezeigt).

Die Beziehung, die wir hier haben ist uebrigens die folgende: die
Darstellungsmatrizen stellen die selbe Funktion dar, nur zu verschiedenen
Basen. Basiswechsel erlauben also einfach eine schoenere oder einfachere
Darstellung von Abbildungen bzw. __Daten__, auch wenn wir die Abbildung fuer die
schoenere Basis nicht explizit kennen.

Wir koennen diese Verwandlung auch schoen auf ein Koordinatensystem
uebertragen. Hierbei waeren $B,B'$ zwei verschiedene "x Achsen" und $C,C'$ zwei
verschiedene "y Achsen". Das heisst, wir haetten also eine Funktion $f:
K^\dim(B) \rightarrow K^\dim(C)$ (z.B. $(\text{hoehe, breite}) \rightarrow
(\text{alter, hautfarbe, wohnort})$). Dann wollen wir also $f(v_{B'})_{C'}$,
also das Bild zur Basis $C'$ von $v$ zur Basis $B'$ herausfinden. Wir kennen
aber nur die Abbildung eines Vektors $v$ zur Basis $B$, auf ein Bild zur Basis
$C$ Dann:

1. Zuerst gehen wir von $B'$ zu $B$, mittels $S_{B,B'}$, erhalten also $v_B$.
2. Dann bilden wir mit der uns bekannten $D_{B,C}$ den Vektor $v_B$ auf sein
   Bild $f(v_B)_C$ ab.
3. Zuletzt gehen wir von $C$ zu $C'$ durch $S_{C,C'}^{-1}$, und erhalten also
   $f(v_B)_{C'}$.
4. Das so gefundene Bild ist gerade jenes, das wir wollten.

```
     C      C'     B
     ^      ^      ^
      \ (3) |<----/------
       \--->|(2) /       |
        \<--|---/<--     | (4)
         \  |  /   |     |
          \ | /    | (1) |
           \|/     |     |
--------------------------> B'
           /|\
		  / | \
		 /  |  \
		/	|   \
	   /	|    \
	  /		|     \
     v		v     v
```

(1): $S_{B,B'}$
(2): $D_{B,C}$
(3): $S_{C,C'}^{-1}$
(4): $D_{B',C'}$ (Ergebnis)

Hierbei waeren die Basen im Uebrigen alle eindimensional, also nur die Geraden.

#### Aequivalenz

Zwei Matrizen in den Raeumen $A,B \in K^{m \times n}$ heissen *aequivalent*,
falls es invertierbare Matrizen $S \in GL_n(K)$ und $T \in GL_m(K)$ gibt mit der
Eigenschaft:

$$B = T^{-1} \cdot A \cdot S$$

Um diese Formel zu verstehen, koennen wir sie ganz einfach auf die obige
Erlaeutung zu "Vektorrouting" uebersetzen. Wir wissen naemlich: jede lineare
Abbildung hat eine entsprechende Darstellungsmatrix aber auch __jede Matrix
entspricht der Darstellungsmatrix einer linearen Abbildung__. Diese ist
natuerlich schon mal $f_A$, auch wenn es noch eine andere Form dafuer geben
kann, z.B. $x \mapsto 3x$. Jedenfalls ist die Abbildung eindeutig bestimmt,
d.h. es gibt nur eine, mit dieser Darstellungsmatrix. Somit koennen wir $A$ und
$B$ in der obigen Formel einfach als Darstellungsmatrizen interpretieren. Dann
interpretieren wir $T,S$ einfach noch als Basiswechselmatrizen und wir haben
exakt die obige Formel reproduziert!

```
  B         =  T^{-1}       *   A      *    S
  |               |             |           |
  |               |             |           |
  v               v             v           v
  D_{B',C'} = S_{C,C'}^{-1} * D_{B,C} * S_{B,B'}
```

Wie koennen wir den Aequivalenzbegriff also *intuitiv* verstehen? So: Sind zwei
Matrizen *aequivalent*, so sind sie also die *Darstellungsmatrix* der *selben
Funktion*, aber *zu verschiedenen Basen*!

Darstellungsmatrizen der selben Funktion in verschiedenen Basen sind
insbesondere also immer aequivalent (bzw. auch aehnlich), da die Matrizen dann
einfach "explizit" anstelle von "implizit" (durch oben beschreibene Verbindung
von Matrizen und Funktionen) Darstellungsmatrizen sind.

#### Aehnlichkeit

Zwei quadratische Matrizen $A, B$ aus dem $K^{n \times n}$ heissen *aehnlich*
falls es *eine* invertierbare Matrix $S \in GL_n(K)$ gibt, sodass:

$$B = S^{-1} \cdot A \cdot S$$.

Hierbei ist die Situation nun also so, dass wir nur eine Matrix $S$ haben, nicht
zwei. Das sagt uns, dass wir hier nur einen Basiswechsel zwischen zwei Basen
$B,B'$ betrachten muessen. $A$ waere dann also die Darstellungsmatrix zur einen
Basis und $B$ die Darstellungsmatrix zur anderen Basis. Der springende Punkt ist
aber, dass wir (bzw. diese Darstellungsmatrizen) eben immer auf die selbe Basis
desselben Raumes abbilden.So erhalten wir:

```
   B      =  S^{-1}       * A   *      S
   |            |           |          |
   |            |           |          |
   v            v           v          v
  D_{B'} = S_{B,B'}^{-1} * D_B  *    S_{B,B'}
```

Intuitiv koennen wir *Aehnlichkeit* wieder so verstehen, dass $A$ und $B$ die
Darstellungsmatrizen der selben Funktion sind, aber zu verschiedenen
Basen. Hierbei bildet die Funktion nun aber immer in die selbe Basis ab, nur
eben jeweils in eine andere. Wir haetten also eine Funktion $f: V \rightarrow V$
(endomorph), wobei wir einmal die Darstellunsgmatrix zur Basis $B$ dieses Raumes
$V$ haben, also $D_B$, und daraus die Darstellunsgmatrix zur Basis $B'$ dieses
Raumes $V$ erhalten, also $D_B'$. Die Funktion muss endomorph sein, also der
Bildraum muss gleich dem Urbildraum sein. Denn ein Raum ist durch seine Basis
eindeutig bestimmt. Haben zwei Raeume namelich dieselbe Basis, so spannt diese
Basis ja denselben Raum auf. Also muessen die beiden Raeume derselbe sein.

Auch gilt hier wieder dass Darstellungsmatrizen $D_X(f)$ und $D_Y(f)$, einmal
zur Basis $X$ von $V$ und einmal zur Basis $Y$ von $V$, der selben Funktion $f$
immer aehnlich zueinander sind. Hierbei ist die oben beschriebene Eigenschaft
einfach explizit.

## Zusammenfassung

Hier eine Zusammenfassung der wichtigsten Eigenschaften, die in diesem Kapitel
besprochen wurden.

### Lineare Abbildungen

Sei $f: V \rightarrow W$ eine lineare Abbildung, dann gilt:

* $f(v + w) = f(v) + f(w)$
* $f(av) = a f(v)$
* $f(0) = 0$
* $\Kern(f) = \{ v \in V \,|\, f(v) = 0\}$
* $\Bild(f) = \{ w \in W \,|\, \exists v \in V: f(v) = w\}$
* Jeder $k$-dimensionale Vektorraum ist isomorph zum $K^n$
* Sei $K$ die Basis von $\Kern(f)$ und $U$ die Basis von $f^{-1}(\Bild(f))$ (die
  Urbilder der Basis des Bildes), dann ist $K \uplus U$ eine Basis von $f$
* $\dim(V) = \dim(\Kern(f)) + \dim(\Bild(f))$ fuer $f: V \rightarrow V$
* Fuer $f: V \rightarrow W$ mit $\dim(V) = \dim(W)$ gilt:
  $\text{surjektiv } \iff \text{ injektiv } \iff { bijektiv }$
* $\Kern(f_A) = \dim(V) - \dim(\Bild(f_A)) = n - \rang(A)$
* Spaltenrang ($\dim(\Bild(f))$) ist gleich Zeilenrang $\rang(A)$
* Die Anzahl linear unabhaengiger Zeilen gleicht jener der Spalten
* Man kann $f$ als (Darstellungs-)Matrix darstellen.

### Inverse

Sei $A$ eine Matrix, dann gilt:

* Finde $A^{-1}$ durch $(A | I_n)$ in Zeilenstufenform.
* $A^{-1} A = A A^{-1}$
* $A^{-1}$ existiert nur, wenn $A$ quadratisch.
* $GL_n(K)$ ist die Menge aller invertierbaren $n \times n$ Matrizen ueber $K$.

### Darstellungsmatrizen

Sei $f: V \rightarrow W$ eine lineare Abbildung, dann gilt:

* $f$ ist durch das Bild der Basis von $V$ eindeutig bestimmt.
* Jede Menge von $n$ Vektoren in $W$ tritt als Bild der Basisvektoren von $V$
  fuer eine lineare Abbildung auf.
* Finde $D_{B,C}$ durch Abbildung von $B$ durch $f$ und Darstellung des Bildes
  zur Basis $C$.
* Sei $D_{B,C}$ die Darstellungsmatrix von $f$, dann gilt: $f(v) = D_{B,C}v$
* $D_{B,C} \cdot D_{C,E} = D_{B,E}$

Sei nun $A \in K^{m \times n}$ und $f: K^n \rightarrow K^m: v \mapsto Av$, dann:

* $f_A \circ f_B = f_{AB}$
* $f_A^{-1} = f_{A^{-1}}$
* $f_A \circ f_{A^{-1}} = \Id$
* $A$ ist Darstellungsmatrix von $f_A$ zur Standardbasis:
  $A = D_{I_n,I_n}(f_A)$
* $A^{-1}$ ist Darstellungsmatrix von $f_A^{-1}$ zur Standardbasis:
  $A^{-1} = D_{I_n,I_n}(f_A^{-1})$
* $D_{B,I_n} = A \cdot B$
* $D_{B,C} = A \cdot B$ zur Basis $C$

### Basiswechsel

Sei $A, D$ Matrizen und $B, C$ Basen, dann gilt:

* $S_{B,C} = D_{C,B}(\Id)$
* $S_{B,C} = B^{-1} C$
* $S_{B,C}^{-1} = S_{C,B}$
* Aehnliche Matrizen haben dieselbe Determinante.
* Aehnliche Matrizen haben dasselbe charakteristische Polynom.
* Die Basiswechselmatrix $S_{I_n, B}$ ist $B$ selbst.
* Sei auch $E$ eine Basis, dann gilt: $S_{B,C} \cdot S_{C,E} = S_{B,E}$
* $Bx = Cy$ fuer Koordinaten $x,y$ von $v$ zur Basis $B,C$

### Aehnlichkeit

Seien $A,B$ zwei Matrizen, dann gilt:

* Sind $A,B$ aehnlich, so sind sie Darstellungsmatrizen derselben Funktion zu
  verschiedener Basis.
* $B = T^{-1} A S$ mit $S \in \GL_n(\mathbb{R})$ wenn $A,B$ aequivalent
* $B = S^{-1} A S$ mit $S \in \GL_n(\mathbb{R})$ wenn $A,B$ aehnlich
* $A = S B S^{-1}$ mit $S \in \GL_n(\mathbb{R})$ wenn $A,B$ aehnlich
