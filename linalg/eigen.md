# Eigenwerte und Eigenvektoren

In diesem Kapitel betrachten wir Eigenvektoren und Eigenwerte sowie ihre
Anwendungen.

$\newcommand{\det}{\mathop{\rm det}\nolimits}$
$\newcommand{\dim}{\mathop{\rm dim}\nolimits}$
$\newcommand{\Kern}{\mathop{\rm Kern}\nolimits}$
$\newcommand{\Bild}{\mathop{\rm Bild}\nolimits}$
$\newcommand{\rang}{\mathop{\rm rang}\nolimits}$
$\newcommand{\deg}{\mathop{\rm deg}\nolimits}$
$\newcommand{\GL}{\mathop{\rm GL}\nolimits}$

## Definition

Sei $A \in K^{n \times n}$ eine quadratische Matrix. Dann ist $\lambda \in K$
ein *Eigenwert* von $A$, falls es $v \in K^{n} \setminus \{0\}$ gibt, mit $Av =
\lambda v$. Einen solchen Vektor $v$, welcher diese Beziehung zu $A$ hat, nennt
man dann *Eigenvektor* von $A$ zum Eigenwert $\lambda$. Das heisst also, dass
die Multiplikation mit $A$ fuer $v$ lediglich eine Streckung seiner Komponenten
ist. Der Faktor der Streckung ist dann eben gerade der Eigenwert.

Die Menge aller Vektoren, die von einer Matrix um den Faktor $\lambda$ skaliert
werden, heisst *Eigenraum* des Eigenwerts $\lambda$ und ist gegeben durch:

$$E_\lambda = \{v \in K^n \,|\, Av = \lambda v\}.$$

Der Nullvektor $v = 0$ ist in diesem Eigenraum $E_\lambda$ fuer alle $\lambda$
auch enthalten, da dann $\forall A: Av = \lambda v = 0$. Somit ist der Eigenraum
fuer $\lambda$, die nicht Eigenwerte von $A$ sind, nicht-leer. Sie enthalten
dann eben nur die Null bzw. sind der Nullraum. Die Definition ist analog fuer
lineare Abbildungen $f_A$ (bzw. ihren Darstellungsmatrizen).

### Beispiele

Die Identitaetsmatrix $I_n$ hat den einzigen Eigenwert $1$, denn $I_n \cdot v =
1 \cdot v \forall v \in V$. Somit ist fuer die Identitaetsmatrix $E_1 = V$.

Sei die Matrix $A \in R^{2 \times 2}$ gegeben durch:

$$
A =
\begin{bmatrix}
0 & 1 \\
1 & 0 \\
\end{bmatrix}
$$

Diese Matrix vertauscht einfach die Komponenten jedes 1-dimensionalen Vektors
$v$, der an sie dranmultipliziert wird. So gilt fuer alle 2-dimensionalen
Vektoren $(x, x)^\top$ mit denselben Komponenten, dass $\lambda = 1$. Denn die
Vertauschung hat ja dann keinen Effekt.

Auch alle Vektoren $(x, -x)^\top$ mit selben Komponenten aber unterschiedlichen
Vorzeichen sind Eigenvektoren dieser Matrix, jeweils mit Eigenwert $\lambda =
-1$.

### Berechnung von Eigenraeumen

Eine Beobachtung, die wir hier schon ziehen koennen ist, dass der Eigenraum zum
Eigenwert $\lambda = 1$ gegeben ist durch:

$$E_1 = \{v \in \mathbb{R}^2 \,|\, Av = v\}.$$

Fuer jeden solchen Vektor $v$ gilt, also, dass $Av = v$. Dann koenenn wir
umformen zu $Av - I_2v = 0$ und durch Herausheben dann $(A - I_2)v = 0$. Jetzt
koennen wir also mit $(A - I_2)v = 0$ ein homogenes LGS aufstellen. Der
Loesungsraum dieses LGS ist dann gerade der Eigenraum fuer $\lambda =
1$. Fuer einen Eigenwert $\lambda$ gilt also allgemein:

$$\Kern(A - \lambda \cdot I_n) = E_\lambda$$

Der Eigenraum einer Matrix zu einem Eigenwert $\lambda$ ist also gerade der Kern
der Abbildung $A - \lambda I_n$ (bzw. $f_{A - \lambda I_n}$).

Fuer das obige Beispiel erhalten wir so

$$
A - 1 \cdot I_2 =
\begin{bmatrix}
-1 &  1 \\
 1 & -1 \\
\end{bmatrix}
.$$

Hier sieht man, dass $\rang(A - I_2) = 1$, womit wir also nur einen ($n -
\rang$) Freiheitsgrad erhalten. Ebenso wissen wir dann, dass die Dimension des
Kernes (hier also der Eigenraum) $\dim(E_1) = 1$ (da $\Kern(A - I_2)$ ein
Erzeugendensystem fuer den Vektorraum $E_1$ ist). Somit haben wir fuer $E_1$
auch eine Basis $(1, 1)^\top$ (das ist der Basisvektor des Kernes, also des
Eigenraumes) oder eine normale Basis $(1, -1)^\top$ fuer den $E_{-1}$.

Eine weitere Beobachtung ist, dass $\langle (1, 1)^\top, (1, -1)^\top \rangle$
eine Basis des $\mathbb{R}^2$ ist. Wir fragen uns also: steckt hier ein
allgemeines Prinzip dahinter? Die Antwort ist: Ja! Es gibt allgemein Verfahren
die auf den obigen Beobachtungen basieren. Man weiss naemlich insbesondere, dass
Eigenraeume immer Untervektorraeume sind. Wir muessten also nur zeigen, dass die
Basn dieser Untervektorraeume auch Basen des Raumes sind.

### Berechnung von Eigenwerten

Wir wissen nun schon, wie man auf die Eigenraume einer Matrix kommt, wenn wir
die Eigenwerte schon vorliegen haben. Wir interessieren uns nun dafuer, wie
Eigenwerte genau berechnet werden. Dabei basieren wir uns auf der Beobachtung,
dass das homogene LGS $(A - \lambda I_n) x = 0$ genau dann nicht eindeutig
loesbar ist, also auch nicht-triviale Loesungen hat, wenn $\lambda$ Eigenwert
von $A$ ist. Waere das LGS naemlich eindeutig loesbar, dann gaebe es also nur
die eine, triviale Loesung fuer die Null. Der Eigenraum waere dann also der
Nullraum. Es gilt aber, dass der Eigenraum $E_\lambda$ nur fuer $\lambda$ der
Nullraum ist, die nicht Eigenwert sind.

Sei nun $A \in K^{n \times n}$ eine quadratische Matrix und $\chi_A := \det(x
I_n - A)$ ein Polynom in $K[x]$. Dann nennt man $\chi_A$ *charakteristisches
Polynom* von $A$. Hierbei hat $\chi_A$ immer Grad $n$ und hoechsten (linksten)
Koeffzienten $1$ ($1 \cdot x^n$). Die wichtige Eigenschaft dieses
charakteristischen Polynoms ist es nun, dass __die *Nullstellen* des
charakteristischen Polynoms die Eigenwerte von $A$ sind__.

Betrachten wir als Beispiel die Matrix

$$
A =
\begin{bmatrix}
0 & 1 \\
1 & 0 \\
\end{bmatrix}
\in \mathbb{R}^{2 \times 2}
$$.

Dann ist das charakteristische Polybom $\chi_A$ gegeben durch:

1. $\chi_A = \det(xI_2 - A)$
2. $\chi_A = \det\begin{bmatrix}x & -1 \\ 1 & x\end{bmatrix}$
3. $\chi_A = x^2 - 1 = (x - 1)(x + 1)$

Die Nullstellen sind nun die Eigenwerte von $A$, also nach Faktorisierung
offensichtlich $1$ und $-1$. Haetten wir das boese Polynom $x^2 + 1$, dann
haetten wir nur Eigenwerte in $\mathbb{C}$.

### Berechnung von Nullstellen von Polynomen

Sei $K[x]$ der Polynomring mit Addition und Mulitplikation von Polynomen. Wir
interessieren uns vor allem dafuer, wie die Division von Polynomen, mit Rest,
funktioniert. Durch Polynomdivisionen kommen wir naemlich zu Nullstellen, und
Nullstellen sind ja gerade Eigenwerte.

Betrachten wir nun zwei Polynome $f, g \in K[x]$ mit $g \neq 0$. Dann koennen
wir eine Polynomdivision fuer $f$ und $g$ machen. Bei dieser Division wuerden
wir ein Quotientpolynom $q$ erhalten, was dem "ganzzahligen Teiler" entspricht,
sowie einem Restpolynom $r$. Hierbei sind $q,r$ also $\in K[x]$. Dann gilt nach
der Division von $f$ durch $g$: $$f = g \cdot q + r$$ mit $\deg(r) < \deg(g)$
(der Grad des Rest is kleiner als der Grad des Divisors). Hierbei ist meistens
auch $\deg(g) < \deg(f)$, sonst ist $f$ einfach der Rest und der Quotient Null.

Nun sagen wir also, dass die Eigenwerte $\lambda \in K$ gerade die Nullstellen
von $f \neq 0$ sind. Diese finden wir, indem wir das Polynom $f$ in
Linearfaktoren bezueglich $g$ aufspalten. Alternativ betrachten wir einfach den
Fall, dass wir eine Loesung $\lambda$ kennen, und $\chi_A$ dann entsprechend
vereinfachen wollen. Hierzu fuehren wir zuerst eine Division mit Rest durch,
also nehmen wir $g = x - \lambda$ (wobei $x - \lambda$ eben ein Linearfaktor
ist) als Divisor von $f$. Dann erhalten wir $f = (x - \lambda) \cdot q + r$. Da
das Divisorpolynom $x - \lambda$ Grad 1 hat, und der Grad des Rest immer kleiner
als der Grad des Divisors ist, muss gelten $\deg(r) < 1 \Rightarrow \deg(r) =
0$. Somit waere $r$ also schon sicher einmal ein konstantes Polynom. Wir koennen
die Nullstelle $\lambda$ nun noch in die Funktion einsetzen, dann erhalten wir:

$$f(\lambda) = 0 = (\lambda - \lambda) \cdot q + r = 0 + r \Rightarrow r = 0$$

Hier haben wir $\lambda$ noch in die obige Formel fuer die Division
eingesetzt. Wir erhalten also den Term $0 = 0 + r = r$. Somit sehen wir: an
einer Nullstelle ist der Rest der Polynomdivision immer Null. Waere $g$ kein
Linearfaktor, also Teiler von $f$, so wuerden wir einen konstanten Rest $r \neq
0$ erhalten (zur Kontrolle). Daher gilt also insgesamt nun: $f = (x - \lambda)
\cdot q$. Somit ist $(x - \lambda)$ also der erste Linearfaktor von $f$, welche
man in Folge abspalten kann. Wenn wir nun noch eine weitere Nullstelle $\mu \in
K: \mu \neq \lambda$ kennen, koennen wir wieder einstzen:

$$f(\mu) = 0 = (\mu - \lambda) q(\mu)$$

Da $\mu \neq \lambda$ ist $\mu - \lambda \neq 0$. Da die ganze Summe aber Null
ist, muss $q(\mu) = 0$ gelten. Wenn $\mu$ eine Nullstelle von $f$ ist, ist es
also auch eine Nullstelle vom Quotienten $q$ und wir koennen also bei $q$
weiterarbeiten (wieder Polynomdivision). Das ganze kann man nun induktiv
wiederholen, was hoechstens $n = \deg(f)$ mal notwendig ist.

Insgesamt erhalten wir dann durch die $r$ *paarweise verschiedenen* Eigenwerte
bzw. Nullstellen $\lambda_1, ... \lambda_r$ eine Darstellung von $f$ durch ihre
Linearfaktoren:

$$f = (x - \lambda_1)^{e_1} \cdot ... \cdot ... (x - \lambda_r)^{e_r} \cdot g$$

Wenn wir beim Abspalten von Linearfaktoren eine Nullstelle $\lambda_i$ genau
$e_i$ mal fanden, dann haben wir diesen Linearfaktor eben $e_i$ mal in dem
Produkt und geben hierfuer auf den Linearfaktor $(x - \lambda)$ den Exponenten
$e_i$ hinauf. Dieser Wert $e_i \in \mathbb{N}$ nennt man dann *Vielfachheit* der
Nullstelle (genauer: *algebraische Vielfachheit*). Hinten haben wir noch ein
Polynom $g \in K[x]$ ohne Nullstellen in $K$. Da wir fuer eine Funktion mit Grad
$\deg(f) = n$ nur *maximal* (nicht genau!) $n$ Loesungen bzw. Nullstellen
bzw. Linearfaktoren haben koennen, muessen im Uebrigen auch $r \leq n$.

## Vielfachheit von Eigenwerten

Aus dem Fundamentalsatz wissen wir: ist ein Koerper $K$ algebraisch
abgeschlossen, so existieren fuer ein Polynom $n$-ten Grades sicher Nullstellen,
und zwar maximal $n$ viele (nach $n$ Polynomdivisionen haben wir ein konstantes
Polynom). Im Kontext des charakteristischen Polynoms $\chi_A$ bzw. einer
korrespondierenden Matrix $A \in K^{n \times n}$ gilt somit, dass $\chi_A$
*maximal* (nicht genau!) $n$ Nullstellen haben kann und daher $A$ *maximal* $n$
*Eigenwerte*.

Sei nun $\lambda \in K$ ein Eigenwert der Matrix $A \in K^{n \times n}$. Dann
gibt es fuer diesen Eigenwert zwei verschiedene Begriffe der *Vielfachheit*:

1. Der erste Begriff ist die *algebraische Vielfachheit* $m_a(\lambda)$ von
  $\lambda$. Diese gibt die Vielfachheit der Nullstelle $\lambda$ im
  charakteristischen Polynom $\chi_A$ an. Das ist also der Exponent $e_i$ in der
  Linearfaktorisierung $(x - \lambda_1)^e_1 \cdot ... \cdot (x - \lambda_n)^e_n$
  von $f$.

2. Die zweite Art von Vielfachheit ist die *geometrische Vielfachheit*
   $m_g(\lambda)$ von $\lambda$. Diese ist definiert als die *Dimension des
   Eigenraums* $E_\lambda$, also: $m_g(\lambda) = \dim(E_\lambda)$. Insbesondere
   muss die geometrische Vielfachheit nicht notwendigerweise gleich der
   algebraischen sein.

Die Summen von sowohl algebraischer als auch geometrischer Vielfachheiten ist
durch die Dimension des Raumes nach oben beschraenkt. Fuer algebraische
Vielfachheiten ist das so, weil wir nicht mehr als $n$ Nullstellen fuer ein
Polynom $n$-ten Grades finden koennen. Fuer geometrische Vielfachheiten ist das
so, weil wir nicht mehr als $n$ linear unabhaengige (Basis-)Vektoren aus den
Eigenraeumen waehlen koennen. Denn Eigenvektoren bzw. Basisvektoren von
Eigenraeumen sind natuerlich selbst Vektoren aus dem Raum und die maximale
Anzahl an linear unabhaengigen Vektoren ist gerade die Dimension des Raumes.

### Gleichheite von Vielfachheiten

Eine interessante Beobachtung ist, dass wenn sich die geometrischen
Vielfachheiten auf $n$ summieren, dann algebraische Vielfachheiten gleich den
Geometrischen sein muessen. Also:

$$\sum_\lambda m_g(\lambda) = n \iff m_g(\lambda) = m_a(\lambda)$$

#### Beweis

Fuer den Beweis setzen wir die geometrischen Vielfachheiten Anfangs den
Algebraischen und zeigen, dass keine andere Konfiguration moeglich ist. Es gilt
also momentan: $m_g(\lambda) = m_a(\lambda)$ fuer alle Eigenwerte
$\lambda$. Insbesondere gilt eben auch $\sum_\lambda m_g(\lambda) = \sum_\lambda
m_a(\lambda) = n$. Wir wissen auch, dass geometrische Vielfachheiten stets durch
algebraische beschraenkt sind. Wenn wir nun also eine der algebraischen
Vielfachheiten eins erhoehen wollen, so muessen wir zwingend die Vielfachheit
einer anderen um eins reduzieren. Denn sonst wuerden sie sich auf $n + 1 > n$
summieren. D.h. wir muessen also die algebraische Vielfachheit um eins
verringern, sodass diese algebraische Vielfachheit dann aber um Eines kleiner
waere als die geometrische. Wir wissen aber, dass das nicht moeglich ist!
Wiederspruch.

Summieren sich geometrische Vielfachheiten also auf $n$, so muss gelten:

$$m_a(\lambda) = m_g(\lambda)$$

Ist die Matrix im Uebrigen noch ueber ihrem eigenen Raum in Linearfaktoren
zerfallen, so muss eine Matrix, fuer welche obige Eigenschaft gilt,
diagonalisierbar sein.

### Beispiele

Sei eine Matrix $A \in \mathbb{R}^{2 \times 2}$ gegeben durch:

$$
A =
\begin{bmatrix}
0 & 1 \\
1 & 0 \\
\end{bmatrix}
$$

mit charakteristischem Polynom $\chi_A = x^2 - 1$. Dann hat $A$ Eigenwerte
bzw. $\chi_A$ Nullstellen $1$ und $-1$. Fuer beide sind die algebraische und
geometrische Vielfachheit genau $1$.

Sein nun $A \in \mathbb{R}^{2 \times 2}$ definiert als

$$
A =
\begin{bmatrix}
1 & 1 \\
0 & 1 \\
\end{bmatrix}
$$

Diese Matrix hat nun charakteristisches Polynom

$$\chi_A = \det(xI_n - A) = (x - 1)^2.$$


Eindeutig ist hier $\lambda = 1$ der einzige Eigenwert (Loesung des
Polynoms). Hier hat $\lambda$ aber nun algebraische Vielfachheit $m_a(\lambda) =
2$, was man am Exponenten in der Linearfaktorisierung ablesen kann.  Fuer die
geometrische Vielfachheit suchen wir dann die Dimension des Loesungsraums des
homogenen LGS $A - \lambdaI_2 = A - I_2 = 0$. Hierbei ist $A$ also gegeben durch

$$
A - I_2 =
\begin{bmatrix}
0 & 1 \\
0 & 0 \\
\end{bmatrix}
.$$

Wir haben nun zwei Moeglichkeiten, die Dimension des Eigenraums
bzw. Loesungsraums zu finden: einen kurzen und einen langen Weg. Im ersten Weg
bestimmen wir die Loesungen des LGS, schreiben den Loesungsraum explizit auf und
sehen uns an, wieviele Basisvektoren der Loesungsraum hat. Das ist dann die
Dimension. Jetzt ueberlegen wir uns aber, ob wir da nicht schneller hinkommen?
Wenn wir nur den Rang $r$ der Matrix wissen, dann wissen wir ja sofort, dass wir
$n - r$ viele Freiheitsgrade fuer die Loesung haben werden. Jeder Freiheitsgrad
entspricht einer freien Variable fuer den Loesungsraum bzw. dann eben *einem
Basisvektor* fuer die entsprechende Basis. Es genuegt also schon, die
Freiheitsgrade aus dem Rang auszurechnen. Wir wissen dann naemlich, dass
$m_g(\lamdba) = n - r$. Hier hat die Matrix Rang $1$, d.h. wir erhalten $n - r =
2 - 1 = 1$ Freiheitsgrade, daher einen Basisvektor und somit hat der Eigenraum
gerade Dimension Eins.

### Satz

Sei $\lambda \in K$ ein Eigenwert bzw. eine Nullstelle eines charakteristischen
Polynoms. Dann gilt:

$$1 \leq m_g(\lambda) \leq m_a(\lambda) .$$

Die geometrische Vielfachheit ist also durch die algebraische Vielfachheit nach
oben beschreankt und beide sind mindestens $1$.

#### Beweis

Es gilt sicher schonmal, dass $m_g(\lambda) \geq 1$. Denn $\lambda$ ist ein
Eigenwert, d.h. $\exists v \in K^n: Av = \lambda v$. Somit ist $E_\lambda$
sicher nicht nur der Nullraum, also $E_\lambda \neq \{0\}$. Somit kann also die
Dimension auch sicher nicht Null sein, weil sei das nur fuer den Nullraum
ist. Da $E_\lambda$ aber noch diesen weiteren Vektor enthaelt, neben dem
Nullvektor, muss dieser Eigenraum Dimension $\dim(E_\lambda) \geq 1$ haben.

Sei nun weiter $m := m_g(\lambda)$ die geometrische Vielfachheit des
Eigenwerts. Dann waehle eine Basis $\{v_1, ..., v_m\}$ des Eigenraums
$E_\lambda$. Da die Vektoren in $E_\lambda$ alle $\in K^n$ sind, ist $E_\lambda$
also Unterraum vom $K^n$ (genauer: ein Eigenraum ist Unterraum des Bildes von
$f_{A - \lambda I_n}$, also des von den Spalten von $A - \lambda I_n$
aufgespannten Raumes). Weiters ist diese Basis $\{v_1, ..., v_m\}$ also eine
linear unabhaengige Menge von Vektoren. Dann koennen wir sie also zu einer Basis
$B = \{v_1, ..., v_m, ..., v_n\}$ von $K^n$ ergaenzen, wobei die $v_1, ..., v_m$
also die Basisvektoren des Eigenraumes sind, welche wir dann um $n - m$
Basisvektoren des $K^n$ ergaenzen.

Betrachten wir nun die Matrixabbildung $f_A: v \mapsto Av$. Fuer alle $i \in
\{1, .., m\}$, also Indizes der Basisvektoren des Eigenraums (nicht der
ergaenzten Basis), gilt dann also $f_A(v_i) = Av_i = \lambda v_i$ (weil die
$v_i$ gerade die Eigenvektoren zu $\lambda$ waren). Wenn man nun die
Darstellungsmatrix $D_B(f_A)$ betrachtet, dann werden diese Vektoren aus dem
Eigenraum eben auf $\lambda v_i$ abgebildet. Somit sind ihre Koordinaten gerade
so, dass sie den Faktor $\lambda$ in der $i$-ten Spalte haben (fuer $v_i$) und
sonst ueberall Nullen (weil sie ja nur aus $\lambda$ mal diesem Basisvektor
$v_i$ und keinem anderen Basisvektor bestehen). Auch gilt, dass diese Bilder
$\lambda v_i$ sicher nicht aus den ergaenzten Basisvektoren $\{v_{m+1}, ...,
v_n\}$ bestehen. Somit haben wir also fuer die ersten $m$ Spalten in der
Darstellungsmatrix (fuer alle Basisvektoren aus dem Eigenraum) oben einen $m
\times m$ Block, der gerade $\diag(\lambda, ..., \lambda)$ ist, und darunter
noch $n - m$ Nullen fuer die Koordinaten der ergaenzten Basisvektoren, die fuer
diese Basisvektoren des Eigenraums aber keine Bedeutung spielen bzw. sicher Null
sind. Das ganze sieht ungefaehr so aus:

```
        m
l ... 0 | ...
. .   . | ...
.  .  . | ...
.   . . | ...
0 ... l | ...
--------|---- <== m
        |
   0    |  C
        |
```

$C$ ist hierbei die rechte untere Matrix $\in K^{(n - m) \times (n - m)}$. In
diesem Fall sind diese Darstellungsmatrix $D := D_B(f_A)$ sowie die
urspruengliche Matrix *aehnlich*, d.h. $\exists S \in \GL_n(K): D =
S^{-1}AS$. Sie sind aehnlich, weil $A$ ja die Darstellungsmatrix von $f_A$
bezueglich der Standardbasis ist, und $D$ nun die Darstellungsmatrix der selben
Funktion $f_A$ ist, aber eben zu dieser ergaenzten Basis $B$. Da $A$ und $D$
also Darstellungsmatrizen der selben Funktion, aber zu verschiedenen Basen sind,
sind sie notwendigerweise aehnlich (denn genau so ist Aehnlichkeit
definiert). Hierbei ist dann $S = (v_1, ..., v_n)$ ($B$ als Matrix), hat also
die $n$ Basisvektoren als Spalten. Die Basiswechselmatrix $S$ ist hier gerade
$B$, weil wir in die Standardbasis, zu welcher $A$ Darstellungsmatrix ist,
wechseln.

Dann koennen wir diese Formel $D = S^{-1}AS$ umformen zu $A = SDS^{-1}$. Weiter
folgt, dass $\chi_A = \det(xI_n - A) = \det(x I_n - SDS^{-1})$. Nun koennen wir
den Ausdruck $xI_n - SDS^{-1}$ noch zu $S(xI_n - D)S^{-1}$ umformen. Denn das
ergibt $SxI_nS^{-1} - SDS^{-1}$. Die Identitaetsmatrix $I_n$ hat dabei die
Eigenschaft, dass die Multiplikation mit ihr kommutativ ist. Das skalieren mit
$x$ aendert diese Eigenschaft nicht, also ist $S(xI_n)S^{-1} =
(xI_n)SS^{-1}$. $SS^{-1}$ hebt sich dabei dann natuerlich wieder auf, sodass wir
am Ende wieder nur $xI_n$ stehen haben bzw. in der Klammer dann $xI_n -
SDS^{-1}$. Die Umformung ist also valide.

Jetzt koennen wir die Multiplikativitaet der Determinante ausnutzen, um zu
schreiben:

$$\det(S(xI_n - D)S^{-1}) = \det(S) \det(xI_n - D) \det(S^{-1}) .$$

Dann wissen wir auch, dass $\det(S^{-1}) = \det(S)^{-1}$. Da wir hier nur mehr
*Zahlen* multiplizieren, duerfen wir die Determinanten hier einfach vertauschen
bzw. $\det(S)$ dann eben wegstreichen. Dann haben wir also nur mehr $\det(xI_n -
D)$. Wir haben also gezeigt, dass gilt:

$$\chi_A = \det(xI_n - A) = \det(xI_n - D) = \chi_D .$$

Das waere eigentlich auch schon daraus gefolgt, dass aehnliche Matrizen
dieselben charakteristichen Polynome haben, was wiederum daraus folgt, dass
aehnliche Matrizen dieselbe Determinante haben.

Wegen der speziellen Form von $\chi_D$ mit den $\lambda$s auf der Diagonalen der
ersten $m$ Zeilen und Spalten wissen wir nun aber, dass $(x - \lambda)$ als
erste $m$ Linearfaktoren hat und der Rest sich aus der Determinante von
$\det(xI_n - C) = \chi_C$ ergibt ($D$ ist ja auch eine Matrix in
Block-Dreiecksgestalt). Also gilt $\chi_D = (x - \lambda)^m \cdot
\chi_C$. $\chi_C$ kann hierbei auch nicht das Nullpolynom sein. Das heisst nun,
dass $\chi_A = \chi_D$ mindestens $m$ viele Nullstellen hat. Somit ist
$m_a(\lambda)$ also sicher gleich gross wie $m_g(\lambda)$. Wenn $\chi_A$ noch
weitere Nullstellen bzw. Loesungen fuer $\lambda$ haette, dann koennte die
algebraische Vielfachheit aber groesser sein als die geometrische. Somit gilt
also insgesamt:

$$m_a(\lambda) \geq m_g(\lambda) \geq 1$$

Zusammenfassung des Beweises:
1. Wir nehmen eine Basis mit $\dim(E_\lambda)$ vielen Basisvektoren.
2. Wir ergaenzen sie zu einer Basis $B$ des $K^n$.
3. Wir bilden die Darstellungsmatrix $D := D_B(f)$.
4. Wir erhalten aus der interessanten Gestalt dieser Darstellungsmatrix, dass
   $\chi_D = \chi_A$.
5. Das wissen wir aber schon, weil $A$ und $D$ aehnlich sind.
6. Wir sehen, dass $\chi_D$ den Eigenwert von $A$ mindestens $\dim$ mal im
   charakteristischen Polynom hat.
7. Wir erkennen, dass $\chi_D$ nur mehr, aber nicht weniger Nullstellen an der
   Stelle $\lambda$ haben kann.
8. Wir schliessen daraus, dass $m_a(\lambda) \geq m_g(\lambda)$ gilt.

## Diagonalisierbarkeit

Wir wollen nun die Eigenschaft der *Diagonlaisierbarkeit* einer Matrix
untersuchen. Eine quadratische Matrix $A \in K^{n \times n}$ heisst
*diagonalisierbar*, falls es eine Basis des $K^n$ bestehend aus Eigenvektoren
von $A$ gibt. In anderen Worten: $A$ ist aehnlich zu einer Diagonalmatrix. Wir
werden zuerst die formalen Definition und Anmerkungen aus der Vorlesung
bearbeiten, und dann eine Intuition fuer diesen letzen Satz suchen.

### Beispiele

Zunaechst zwei Beispiele fuer diagonalisierbare und nicht diagonalisierbare
Matrizen.

Sei hiefuer die Matrix $A \in \mathbb{R}^{2 \times 2}$ zunaechst gegeben durch:

$$
A =
\begin{bmatrix}
0 & 1 \\
1 & 0 \\
\end{bmatrix}
.$$

Diese Matrix hat Eigenwerte $1, -1$ bzw. Eigenraeume

1. $E_1 = \langle (1, 1)^\top \rangle$, und
2. $E_2 = \langle (1, -1)^\top \rangle$

Sie ist diagonalisierbar, weil $\{(1, 1)^\top, (1, -1)^\top\}$ eine Basis des
$K^2$, ist die also aus Eigenvektoren.

Sei dann $A$ anders definiert als

$$
A =
\begin{bmatrix}
1 & 1 \\
0 & 1 \\
\end{bmatrix}
.$$

$A$ hat nun ein charakteristisches Polynom $\chi_A$ mit Nullstellen
bzw. Eigenwerten $1$ (2 mal) und Eigenraum $E_1 = \langle (1, 0)^\top
\rangle$. Diese Matrix ist nicht diagonalisierbar, denn es fehlen anscheinend
Eigenwerte. Wir haben erst einen Basisvektor, der den Eigenraum $E_1$
aufspannt. Fuer den $K^2$ brauchen wir aber sicher einen zweiten Eigenvektor.

### Bedingungen fuer Diagonalisierbarkeit

Wir koennen sogar genau festlegen, wann eine Matrix diagonalisierbar ist. Eine
quadratische Matrix $A \in K^{n \times n}$ ist genau dann diagonalisierbar, wenn
beide der folgenden Bedingungen erfuellt sind:

1. Das charakteristische Polynom $\chi_A$ zerfaellt in Linearfaktoren unter dem
   Raum der Matrix.

2. Fuer alle Eigenwerte $\lambda_i$ gilt $m_g(\lambda_i) = m_a(\lambda_i)$.
   Algebraische und geometrische Vielfachheiten sind also gleich.

Bei (1) ist mit "unter dem Raum der Matrix" gemeint, dass wenn $A \in
\mathbb{R}^{m \times n}$ (also aus dem reellen Raum ist), die Loesungen rell
sein muessen. Ist die Matrix komplexwertig, duerfen die Eigenwerte auch
komplexwertig sein.

#### Beweis

Wir beweisen diese Aussagen in mehreren Schritten. Zunaechst betrachten wir ein
Lemma, und dann den Beweis der eigentlichen Aussage.

##### Lemma

Es seien $\lambda_1, ..., \lambda_r \in K$ paarweise verschiedene Eigenwerte
einer Matrix $A \in K^{n \times n}$. Weiter seien $v_i \in E_{\lambda_i} \forall
i$ mit $v_1 + ... + v_r = 0$ entsprechende Eigenvektoren. Wir zeigen:

Dann muessen alle diese Vektoren $v_i$ gerade die Nullvektoren $v_i = 0$ aus den
jeweiligen Eigenraeumen sein.

Wir zeigen also, dass Eigenvektoren aus verschiedenen Eigenraeumen, die nicht
der Nullvektor sind, immer linear unabhaengig sind. Bzw. dass wenn eine Summe
von Eigenvektoren Null ergibt, die einzelnen Vektoren Null gewesen sein
muessen. Hierbei die wichtige Anmerkung, dass wenn $v$ Eigenvektor ist, auch
alle skalare Vielfache $av \forall a \in \mathbb{R}$ davon, Eigenvektoren im
selben Eigenraum sind. Denn es gilt:

$$A(av) = a(Av)= a(\lambda v) = \lambda(av).$$

Ein gestrecketer Eigenvektor wird intuitiv noch immer um denselben *Faktor*
$\lambda$ gestreckt. Auch ist ein Eigenraum ja ein Untervektorraum des $K^n$,
muss also folglich bezueglich der Linearkombination seiner Elemente
abgeschlossen sein. Auch hat jeder Eigenraum eine Basis, womit wir nicht nur $v$
sondern $av \forall a \in K$ generieren koenenn. Die Aussage ist also recht
offensichtlich wahr.

Was oben also eigentlich steht (ueber ein paar Ecken), ist dass eine
Linearkombination von Eigenvektoren nur dann Null sein kann, wenn die
Koeffizienten Null sind. Also dass es nur die triviale Darstellung der Null fuer
Eigenvektoren zu verschiedenen Eigenwerten gibt.

Diesen Satz wollen wir nun also beweisen. Hierfuer induzieren wir ueber die
Anzahl der verschiedenen Eigenwerte $r$:

###### Induktionsanfang

$r = 1 \Rightarrow v_1 = 0$ (nur der Nullvektor ist der Nullvektor).

###### Induktionsschritt

Wir gehen nun von $r - 1$ auf $r$, fuer alle $r \geq 2$.

Betrachten wir hierzu die Summe ueber alle gewaehlten Eigenvektoren, skaliert um
ihren Eigenwert:

1. $\sum_{i=1}^r \lambda_i v_i$ (*Eigen*schaft; AHAHAHA)
2. $\sum_{i=1}^r A v_i$ (Ziehen $A$ aus der Summe heraus)
3. $A (\sum_{i=1}^r v_i)$ (Voraussetzung, dass die Summe der Eigenvektoren Null ergibt)
4. $A \cdot 0 = 0$

Wir summieren also ueber alle $\lambda_i v_i$, was eben genau der entsprechenden
Skalierung der Vektoren $v_i$ durch die Matrix $A$ entspricht (nach Definition
von Eigenwerten). Dann koennen wir die Matrix $A$ also herausheben und erhalten
die Summe ueber alle Eigenwerte. Diese Summe ist nach Voraussetzung Null
(Anmerkung: wir nehmen das ja an und wollen dann zeigen, dass die $v_i$ alle
Null sein muessen).

Andererseits gilt aber auch:

$$\sum_{i=1}^r \lambda_1 v_i = \lambda_1(\sum_{i=1}^r v_i) = 0$$.

Wir koennen also auch alle $v_i$ mit dem ersten Eigenwert multiplizieren und
erhalten dadurch (wieder ueber die Voraussetzung) die Null. Es gibt also zwei
Wahlmoeglichkeiten fuer Koeffizienten, sodass sich die $v_i$ (mal diesen
Koeffizienten) auf Null aufaddieren: $\lambda_1$ und $\lambda_i$. Jetzt koennen
wir diese beiden Summen also gleichsetzen und erhalten:

1. $\sum_{i=1}^r \lambda_i v_i = \sum_{i=1}^r \lambda_1 v_i$
2. $\sum_{i=1}^r \lambda_i v_i - \sum_{i=1}^r \lambda_1 v_i = 0$
3. $\sum_{i=1}^r (\lambda_i - \lambda_1) v_i = 0$ ($\lambda_1 - \lambda_1 = 0$
   fuer $i = 1$)
4. $\sum_{i=2}^r (\lambda_i - \lambda_1) v_i = 0$

Es gilt hier noch zusaetzlich fuer alle $i$, dass $(\lambda_i - \lambda_1) v_i
\in E_{\lambda_i}$. Der Grund hierfuer ist (wie oben besprochen), dass allgemein
fuer einen Eigenvektor gilt, dass ein Vielfaches dieses Vektors sicher auch
wieder im selben Eigenraum ist.

Nun haben wir also fuer alle $i \in \{2, ..., r\}$ die Eigenvektoren
$(\lambda_i - \lambda_1)v_i$. Diese sind weiterhin paarweise verschieden und
haben also (wie oben gerade gezeigt) auch die Eigenschaft, dass sie auf Null
aufaddieren. Insbesondere haben wir also nur mehr $r - 1$ solcher Vektoren und
nicht mehr $r$. Nun greift also die Induktionsvoraussetzung sodass wir mit
Sicherheit davon ausgehen koennen, dass diese $(\lambda_i - \lambda_1)v_i = 0$
fuer alle $i \in \{2, ..., r\}$. Wir wissen also, dass diese skalierten Vektoren
$(\lamda_i - \lambda_1)v_1 = 0$, muessen aber noch zeigen, dass das daran liegt,
dass $v_i = 0$ war.

Da wir die $\lambda_i$ paarweise verschieden gewaehlt haben, gilt also, dass
$\lambda_i \neq \lambda_1 \forall i$. Dann gilt auch $\lambda_i - \lambda_1 \neq
0$. Damit also alle $(\lambda_i - \lambda_1)v_i = 0$ sein koennen, muessen also
alle $v_i$ Null sein. Jetzt haben wir also, dass die $v_i$ Null sind.

Wir wissen weiters, dass $v_1 + v_2 + ... + v_n = 0$ und damit $v_1 = -(v_2 +
v_3 + ... v_n)$. Hier gilt nun also $v_1 = -(0 + 0 + ... + 0) = 0$ und wir haben
gezeigt, dass die Vektoren alle Null sein muessen (einfacher: damit $v_1 + 0 =
0$ gilt, muss $v_1 = 0$ sein).

Da wir gerade auch gezeigt haben, dass wenn ein Vektor in einem Eigenraum ist,
auch alle Vielfachen dieses Vektors im Eigenraum sind, laesst sich $v_1 +
... v_n = 0 \Rightarrow v_i = 0$ erweitern, um zu sagen, dass $\sum_{i=1}^n a_i
v_i = 0 \Rightarrow v_i = 0$. Es gibt also nur die triviale Darstellung der Null
fuer diese Basisvektoren. Somit gilt also:

__Eigenvektoren zu verschiedenen Eigenwerten sind immer linear unabhaengig__.

##### Eigentlicher Beweis

Nun zum eigentlichen Beweis der Diagonalisierbarkeit auf Basis der
Eigenschaften, dass das charakteristische Polynom in Linearfaktoren ueber dem
Raum der Matrix zerfaellt, und algebraische Vielfachheiten aller Eigenwerte
gleich den geometrischen Vielfachheiten sind.

###### $\Leftarrow$

Zunaechst zeigen wir, dass aus der Diagonalisierbarkeit die beiden Eigenschaften
folgen (also $\Leftarrow$).

Wenn $A$ diagonalisierbar ist, bedeutet das, dass $A$ aehnlich zu einer
Diagonalmatrix $D = \diag(a_1, ..., a_n)$ ist. Nun gilt folgende essentielle
Eigenschaft:

__Aehnliche Matrizen haben dieselben charakteristischen Polynome (somit auch die
selben Eigenwerte und Eigenvektoren)__.

(Das folgt aus der Gleihheit der Determinanten aehnlicher Matrizen). Dann gilt
also $\chi_A = \chi_D = \Pi_{i=1}^n (x - a_i)$ (in Linearfaktordarstellung). Da
$D$ nur Werte $a_i$ in der Diagonalen hat, die aus demselben Raum wie die Matrix
kommen (folgt z.B. aus Aehnlichkeit, dass sie Darstellungsmatrizen fuer
denselben Raum sein muessen), und die $a_i$ also die Loesungen des
charakteristischen Polynoms sind, zerfaellt also $\chi_D$ bzw. (was wir zeigen
wollten) $chi_A$ in Linearfaktoren ueber dem Raum der Matrix. Wir haben also die
erste Eigenschaft. Inbesondere bedeutet dass, dass __diagonalisierbare, relle
Matrizen nur relle Eigenwerte haben__. Bzw. allgemein sind die Eigenwerte einer
diagonalisierbaren Matrix aus demselben Raum.

Da $\chi_D = \Pi_{i=1}^n (x - a_i)$ und $\chi_A = \Pi_{i=1}^n (x - \lambda_i)$,
muss gelten, dass jedes $a_j$ mit einem Eigenwert $\lambda_i$ uebereinstimmen
(wobei aber nicht zwingend $j = i$ gelten muss). Fuer $i = \{1, ..., r\}$ sei
$e_i$ nun die Anzahl der Indizes $j$ mit $a_j = \lambda_i$. Dann ist also die
algebraische Vielfachheit $m_a(\lambda_i)$ von diesem Eigenwert $\lambda_i$, zu
welchem $a_j$ korrespondiert, genau gleich $e_i$.

Nun zeigen wir noch, dass die geometrische Vielfachheit von $\lambda_i$ auch
genau gleich $e_i$ ist. Diese Tatsache entnehmen wir daraus, dass $D$ eine
Diagonalmatrix hat. Haben wir naemlich einmal einen Eigenwert $\lambda_i$
bzw. eine in $D$ korrespondierende Loesung $a$, dann berechnen wir ja
bekanntlich den Eigenraum als die Loesungsmenge von $(D - a_i I_n)x = 0$. In
diesem Fall sind wir aber nur an der Dimension dieses Raums interessiert, was
gerade gleich der Anzahl an Freiheitsgraden der Matrix $D - a_i I_n$ ist. Wir
haben mit $D$ also eine Diagonalmatrix. Wenn wir jetzt $a_i I_n$ von dieser
abziehen, erhalten wir genau in den Stellen, wo $D$ eben die Werte $a_i$ hatte,
eine Null. Dann waere also wegen der Diagonalmatrix eben die ganze Zeile
Null. In allen anderen Stellen der Diagonale erhalten wir $a_j - a_i \neq 0$,
weil $a_j \neq a_i$. Wenn wir nun diese Matrix in die strenge Zeilenstufenform
bringen, haetten wir die Nullzeilen ja unten. Das sagt uns, dass wir genau so
viele, also $e_i$ viele, Freiheitsgrade habe. Damit ist also auch die Dimension
(Kardinalitaet der Basis) des Eigenraums genau $e_i$ und insgesamt dann
$m_a(\lambda) = m_g(\lambda)$. Damit ist also auch die zweite Eigenschaft
gezeigt.

###### $\Rightarrow$

Umgekehrt gelten jetzt also auf Grund der Diagonalisierbarkeit der Matrix diese
beiden Eigenschaften (1) und (2). Jetzt wollen wir noch zeigen, dass wenn diese
beiden Eigenschaften gelten, die Matrix diagonalisierbar ist. Also den
umgekehrten Weg ($\Rightarrow$).

Fuer $i \in [r]$ ($r$ viele Eigenwerte) sei nun $B_i$ eine Basis des Eigenraums
$E_{\lambda_i}$. Wir setzten dann $B = B_1 \uplus ... \uplus B_r$, also gleich
der (disjunkten!) Vereinigung von den $B_i$. Es ist klar, dass $B$ aus
Eigenvektoren besteht und mit dem Lemma von oben wissen wir, dass $B$ linear
unabhaengig ist (deswegen disjunkte Vereinigung). Zudem gilt:

Da die $B_i$ alle linear unabhaengig sind, haben wir also $|B| = \sum_{i=1}^r
|B_i|$. Die Dimension der Basis $B_i$ entspricht dabei gerade der geometrischen
Vielfachheit $m_g(\lambda_i)$ des Eigenwertes $\lambda_i$. Somit gilt also
weiter $|B| = \sum_{i=1}^r m_g(\lambda_i)$. Ueber die zweite Eigenschaft fuer
diagonalisierbare Matrizen wissen wir nun, dass $m_g(\lambda_i) =
m_a(\lambda_i)$. Also gilt weiter $|B| = \sum_{i=1}^r m_a(\lambda_i) =
\deg(\chi_A) = n$. Es gilt also, dass $|B| = n$. Da die Eigenvektoren in $B$
alle linear unabhaengig sind und wir genau $n$ viele haben, was die Dimension
des Raumes $K^n$ ist, muss $B$ also eine Basis des $K^n$ sein! Das ist gerade
die Bedingung dafuer, wann man eine Matrix $A$ *diagonalisierbar nennt*: Man
kann eine Basis des ganzen Raumes bilden, die aus Eigenvektoren besteht.

#### Intuition

Intuitiv bedeutet dass, dass die Matrix $A$ alle Vektoren im Raum $K^n$ auf
irgendwelche Weise skaliert. Denn $A$ skaliert ja seine Eigenvektoren. Jetzt
wissen wir fuer eine diagonalisierbare Matrix, dass die Basen der Eigenraeume
eine Basis des ganzes Raumes ergeben. Das bedeutet, dass wir jeden Vektor im
Raum durch eine Linearkombination von Eigenvektoren von $A$ darstellen koennen,
welche $A$ ja beeinflusst (jeweils um ihren Eigenwert skaliert). Formal: haben
wir einen Vektor $v$, so kann man ihn als Linearkombination der Eigenvektoren
$v_i \in B$ einer Basis aus Eigenvektoren darstellen:

$$v = \sum_{i=1}^n a_i v_i.$$.

Nun koennen wir ueberpruefen, wie $v$ durch eine Multiplikation mit $A$
beeinflusst wird:

$$
\begin{align}
	Av &= A (\sum_{i=1}^n a_i v_i) \\
	   &= \sum_{i=1}^n a_i (A v_i) \\
	   &= \sum_{i=1}^n a_i \lambda_i v_i
\end{align}
$$

$A$ streckt die Basiskomponenten von $v$ also anscheinend jeweils um den
entsprechenden Eigenwert. Somit kann $A$ also jeden Vektor $v$ im Raum
beeinflussen bzw. skalieren. Dann koennten wir ja auch direkt die Matrix $A$ auf
eine Matrix abbilden, welche angibt, wie man die Komponenten von $v$ *jeweils
direkt* skalieren muss, um die Skalierung der Eigenvektorenkomponenten
nachzubilden. Wie wuerde so eine Matrix aussehen, die jede Komponente um einen
bestimmten Faktor skaliert? Das waere genau eine Diagonalmatrix (so wie die
Identitaetsmatrix, aber mit Skalierungen in der Diagonalen)! Wir koennten also
direkt eine Matrix bauen, die die Skalierungsfaktoren in der Diagonalen hat. Das
waere also die diagonalisierte Variante von $A$! Deswegen sagt man eben, dass
$A$ dann aehnlich zu einer Diagonalmatrix ist, weil sie ja die selbe Funktion
darstellen (die selbe Skalierung), nur zu verschiedenen Basen (einmal
Eigenwerte, einmal die Basis, in welcher $v$ angegben ist; nicht
notwendigerweise die Standardbasis).

Frage: Haben wir maximal Rang viele Eigenwerte?

Antwort:

Jein. Eine Matrix muss ja regulaer sein, damit wir ueberhaupt Eigenwerte finden
koennen. Sonst ist die Determinante Null, funktionert also mit dem
charakteristischen Polynom nicht. D.h. das ginge dann sowieso nicht. Es stimmt
aber insofern schon, dass ja dann die $K^{\rang \times \rang}$ Matrix, die aus
der singulaeren Matrix $A$ mit $\rang(A) < n$ hervorgeht, indem man nur die
linaer unabhaengigen Spalten bzw. Zeilen selektiert, schon regulaer waere. Da
der Rang von $A$ dann die Dimension dieser Matrix waere, haetten wir fuer sie
dann maximal $\rang$ viele Eigenwerte. Also jein.

#### Mathematische Bedeutung

Wenn eine Matrix diagonalisierbar ist, ist sie also zum Einen aehnlich zu einer
Diagonalmatrix und zum Anderen ist es dann moeglich, eine Basis des ganzen
Raumes zu bilden, die aus Eigenvektoren der Matrix besteht. Was bedeutet das
fuer uns mathematisch? Wenn $A$ aehnlich zu einer Diagonalmatrix $D$ ist, so
ist es also moeglich, eine invertierbare Matrix $S \in GL_n(K)$ zu finden,
sodass gilt:

$$D = S^{-1} A S.$$

Hierbei war die Interpretation (bzw. Definition) von Aehnlichkeit ja jene, dass
wenn $A$ und $D$ aehnlich sind, sie Darstellungsmatrizen der selben linearen
Abbildung, aber zu verschiedenen Basen sind. Die Matrix $S$ hatten wir ja dann
als Basiswechselmatrix von der Basis von $D$ in die Basis von $A$
interpretiert. Dann koennen wir naemlich einen Vektor $v$ zur Basis von $D$
zuerst in die Basis von $A$ wechseln, dann via $A$ das Abbild von $v$ finden,
und dann das Abbild uebr den inversen Basiswechsel aus der Basis von $A$ wieder
in die Basis von $D$ zu bringen. D.h. $S$ ist also eine Basiswechselmatrix.

Was heisst das nun fuer unseren Fall, wo wir eine Basis des Raumes kennen, die
aus Eigenvektoren von $A$ besteht? Hierzu nochmal zur Wiederholung: da wir aus
den Eigenvektoren von $A$ eine Basis des Raumes bilden koennen, kann $A$ eben
jeden Vektor $v$ aus diesem Raum beeinflussen. Hierzu sei $v$ einfach *zu dieser
Eigenvektorbasis gegeben*, dann skaliert die Multiplikation mit $A$ eben jede
Komponente um den entsprechenden Eigenwert. Das heisst nun also fuer die obige
Formel, dass wir sie wie folgt interpretieren koennen:

* $A$ ist die Darstellungsmatrix einer linearen Transformation eines Vektors $v$
  zu einer Basis $B$ (nicht notwendigerweise die Standardbasis).
* $D$ ist die Darstellungsmatrix dieser selben Transformation *zur
  Eigenvektorbasis* $E$.
* $S$ ist die Basiswechselmatrix $S_{B,E}$.

Wir haben nun also einen Vektor $v$. Wir wissen, dass wenn $v$ zur Basis $B$
gegeben ist, $A$ bzw. $f_A$ eine lineare Transformation des Vektors $v$
beschreibt. Wir wissen nun aber auch, dass $A$ diagonalisierbar ist, also dass
es eine Diagonalmatrix $D$ mit Eigenwerten von $A$ in der Diagonalen gibt, zu
welcher $A$ aehnlich ist. Denn $D$ ist ja die Darstellungsmatrix dieser *selben*
linearen Transformation zur Eigenvektorbasis. D.h. wenn wir $v$ zur
Eigenvektorbasis gegeben haben, dann ist die Multiplikation von $v$ mit $D$ die
selbe Transformation wie $f_A$ es war, als $v$ zur Basis $B$ gegeben war. Wir
koennen nun auch exakt erkennen, wieso das so ist: Wenn $v$ zur Eigenvektorbasis
gegeben ist, so sind die Koordinaten von $v$ ja fuer die Eigenvektoren von $A$,
die in der Basis $E$ enthalten sind. Da diese Basisvektoren eben Eigenvektoren
von $A$ sind, welche von $A$ jeweils um ihren entsprechenden Eigenwert skaliert
wuerden, koennen wir diese lineare Abbildung nachbilden, indem wir eine
Diagonalmatrix angeben, die schon direkt die Eigenvektoren entsprechend
skaliert. Gerade deswegen hat die Diagonalmatrix die Eigenwerte in der
Diagonalen!

Wir interessieren uns also nun dafuer, wie ein Vektor $v$ von der Abbildung $A$
transformiert wird. Die Multiplikation mit $A$ ergibt diese Transformation aber
nur zur Basis von $A$, also die Basis $B$. Wir muessen unseren Vektor $v$ also
zuerst in die Basis von $A$ wechseln, dann multiplizieren, und dann wieder
zurueck in die Eigenvektorbasis $E$ wechseln. Das geht ja wegen Aehnlichkeit
gerade ueber die Formel $S^{-1} A S$. Wenn wir nun diesen Vektor $v$ von rechts
dranmultiplizieren, passiert folgendes:

1. $v$ wird durch $S$ aus der Eigenvektorbasis $E$ in die Basis $B$ von $A$
   gewechselt.
2. Die Multiplikation mit $A$ ergibt dann die lineare Transformation von $v$ zur
   Basis $B$.
3. $S^{-1}$ wechselt $v$ dann aus der Basis $B$ wieder zurueck in die
   Eigenvektorbasis $B$.
4. Dadurch ergibt sich das Abbild von $v$ bezueglich dieser Transformation, in
   der Eigenvektorbasis $E$.

Der Sinn ist, dass nun eben $D = S^{-1} A S$. D.h. diese ganzen Schritte sind
aequivalent zur Multiplikation von $v$ direkt mit $D$. Der Grund, weshalb, ist
nun eben dass $D$ gerade die passenden Skalierungen jeder Eigenvektorkomponente
angibt. Wir haben also mit $D$ (hoechstwahrscheinlich) eine viel schoenere
Darstellungsmatrix, als $A$.

In den meisten Faellen ist diese andere Basis $B$ die Standardbasis. Nun ist es
ja so, dass wenn man eine Basiswechselmatrix von einer Basis $X$ in die
Standardbasis bildet, $X$ selbst gerade die Basiswechselmatrix ist. Das ist so,
weil wir Basen ja schon in Standardbasiskoordinaten angeben, sodass wir nichts
weiter machen muessen. Das heisst nun folgendes: Wenn $B$ die Standardbasis ist,
so ist die Eigenvektorbasis $E$ selbst die Basiswechselmatrix. D.h. diese
Eigenvektorbasis, die den Raum aufspannen kann, ist dann gerade $S$. Ist also
nach $S$ gefragt, so muss man einfach die Eigenvektoren in die Spalten
einfuellen. Als Ergebnis erhaelt man dann die Diagonalmatrix $D$, mit den
Eigenwerten in der Diagonalen.

## Trigonalisierbarkeit

Wir wissen jetzt, dass Matrizen, die in Eigenwerte ueber ihrem Raum zerfallen
und wo algebraische Vielfachheiten von Eigenwerten gleich geometrischen sind,
diagonalierbar sind. Es kann aber eben sein, dass selbst ueber einem algebraisch
abgeschlossenem Koerper (uber welchem es immer Nullstellen eines Polynoms gibt),
Matrizen nicht diagonalisierbar ist. Denn es muss ja noch die zweite Eigenschaft
gelten, welche auch fuer algebraisch abgeschlossene Koerper nicht gegeben sein
kann.

Betrachten wir beispielsweise die komplexwertige Matrix $A \in \mathbb{C}^{2
\times 2}$ gegebn durch:

$$
A =
\begin{bmatrix}
1 & 1 \\
0 & 1 \\
\end{bmatrix}
$$

Diese Matrix hat nur den Eigenwert $1$ mit algebraischer Vielfachheit zwei, aber
geometrischer Vielfachheit Eins, sodass also $2 = m_a(1) \neq m_g(1) = 1$. Das
gilt, *obwohl* die Matrix aus dem algebraisch abgeschlossenen Koerper
$\mathbb{C}$ kommt.

Es gilt aber nun eine andere Aussage fuer algebraisch abgeschlossene
Koerper. Sei $K$ algebraisch abgeschlossen und $A \in K^{n \times n}$ eine
quadratische Matrix. Dann ist __$A$ aehnlich zu einer oberen Dreiecksmatrix__.

Es gibt dann also eine obere Dreiecksmatrix $U$ (upper), sodass es eine
invertierbare Matrix $S \in \GL_n(K)$ gibt, sodass gilt:

$$S^{-1}AS = U(\lambda_1, ..., \lambda_n).$$

Hierbei sei $U(\lambda_1, ..., \lambda_n)$ die obere Dreiecksmatrix, die
$\lambda_1, ..., \lambda_n$ in der Diagonalen hat, und oben rechts noch
*irgendetwas anderes*. Da $A$ aehnlich zu dieser Matrix ist, hat es also
dieselbe Determinante und somit dasselbe charakteristische Polynom. Da $U$ die
Eigenwerte von $A$ in der Diagonalen hat (und rechts davon irgendwas) gilt
insbesondere:

$$\chi_A = \Pi_{i=1}^n(x - \lambda_i)$$

### Beweis

Betrachten wir nun den Beweis dieser Aussage, dass jede Matrix ueber einem
algebraisch abgeschlossenen Koerper aehnlich zu einer oberen Dreiecksmatrix mit
den Eigenwerten in der Diagonale ist. Hierzu induzieren wir ueber die Dimension
$n$ der Matrix:

Beweis durch Induktion nach $n$:

#### Induktionsbasis

Fuer $n = 1$ muss $A = (\lambda_1)$, damit wir nur diese Nullstelle haben,
bzw. den Faktor $x - \lambda_1$. $A$ selbst ist auch schon eine obere
Dreiecksmatrix, und $A$ ist aehnlich zu sich selbst, also gilt die Aussage.

#### Induktionsschritt

Sei nun $n \geq 2$. Nach Voraussetzung hat das charakteristische Polynom
$\chi_A$ zumindest eine Nullstelle $\lambda_1$, also ist $\lambda_1$ ein
Eigenwert. Dann nehmen wir nun einen Eigenvektor $v_1$ zum Eigenwert $\lambda_1$
und ergaenzen ihn zu einer Basis $\{v_1, ..., v_n\}$ des Raumes $K^n$.

Da $v_1$ ein Eigenvektor zum Eigenwert $\lambda_1$ ist, und diese gesuchte obere
Dreiecksmatrix $U$ die Eigenwerte in der Diagonalen hat, muss $U = S^{-1}AS$
eine Matrix sein, mit $\lambda_1$ in der linken oberen Ecke, darunter (in der
ersten Spalte) nur Nullen, rechts davon (erste Zeile) irgendetwas, und diagonal
davon eine Matrix $B \in K^{(n-1) \times (n-1)}$:

```
    1
l_1 | . . . . .
 0  |---------- <== 1
 0  |
 0  |    B
 0  |
    n --------- <== n
```

Nach Induktion gibt es nun sicher eine invertierbare Matrix $T \in
\GL_{n-1}(K)$, sodass $T^{-1} B T = U(\lambda_2, ..., \lambda_n)$. Ueber
Induktion wissen wir wirklich mit Sicherheit, dass $T^{-1} B T$ gleich dieser
Matrix ist. $B$ hatten wir jetzt aus der oben gegebenen Matrix rausgezogen. Nun
koennen wir das folgende machen: wir betrachten $T^{-1} B T \Rightarrow T'^{-1}
(S^{-1} A S) T'$, wobei $T'$ gleich $T$ ist, aber mit Nullen rechts und oben
gepadded und einer $1$ im allerersten Eintrag:

```
1 0 . . . 0
0 ---------
. |
. |   T
. |
0 |
```

$T'{-1}$ geht dann eben aus dem Inversen dieser Matrix hervor. Der Sinn dieser
Matrix ist, dass wenn wir sie mit der oben gegebenen ersten oberen
Dreiecksmatrix $U$, mit dem Eigenwert $\lambda_1$ links oben und rechts unten
$B$, multiplizieren, so selektieren wir erstmal diesen Eigenwert (wieder) fuer
die erste Komponenten, und erhalten ansonsten die Multiplikation $T^{-1} B T$
fuer die restliche Matrix (wir selektieren eigentlich nicht nur $\lambda_1$ aus
der ersten Komponente, sonder die ganze erste Zeile, sodass sie gleich bleibt
bzw. uebernommen wird). Nach Induktion wissen wir eben, dass $T{-1} B T$ die
obere Dreiecksmatrix mit den uebrigen Eigenwerten $\lambda_2, ..., \lambda_n$
ergibt. Somit erhalten wir gerade eine obere Dreiecksmatrix:

```
l_1 ? ? ? ?
0 l_2 ? ? ?
.   .
.    .
.     .
0 . .  l_n
```

Zu eben dieser Matrix ist jede Matrix $A$ aus einem algebraisch abgeschlossenem
Koerper aehnlich. Wir haben bei dieser Aussage nur benutzt, dass das
charakteristische Polynom bei einem algebraisch abgeschlossenem Koerper immer
zerfaellt. Es gilt aber allgemeiner, dass jede quadratische Matrix, deren
charakteristisches Polynom zerfaellt (also nicht nur ueber einem algebraisch
abgeschlossenem Koerper), aehnlich zu einer Dreiecksmatrix ist. Eine
Diagonalmatrix ist ja auch eine obere Dreiecksmatrix, also gilt diese Aussage
beispielsweise fuer jede diagonalisierbare Matrix, die ueber ihrem Raum
zerfaellt, welcher eben nicht notwendigerweise algebraisch abgeschlossen sein
muss. In dieser Darstellung ist die Reihenfolge der $\lambda_i$ im Uebrigen
beliebig (die Determinante bleibt ja gleich dem Produkt ueber alle Eigenwerte,
und die Multiplikation ist hier kommutativ).

Der Sinn von dieser Tatsache ist, dass man nicht jede Matrix in die schoenste
Form, einer Diagonalmatrix, bringen kann. Das ist die schoenste Form, weil man
dann genau sieht, um welchen Faktor jede Komponente eines Vektors bei der
Multiplikation skaliert wird. Auch ist ihre Determinante gleich dem Produkt
ueber die Diagonale. Das geht aber eben nicht fuer jede Matrix. Man *kann* aber
jede Matrix (ueber einem algebraisch abgeschlossenem Koerper) sicher in diese
"fast-schoene" Form einer oberen Dreiecksmatrix bringen. Sie hat zum Beispiel
schon die schoene Eigenschaft, dass ihre Determinante leicht zu berechnen ist
(eben auch Produkt der Diagonaleintraege, bzw. Eigenwerte).

Diese Eigenschaft der Aehnlichkeit zu einer oberen Dreiecksmatrix nennt man
*Trigonalisierbarkeit*.

https://de.wikipedia.org/wiki/Trigonalisierung

### Jordan-Normalform

Es gilt sogar noch eine staerkere Aussage, welche wir aber nicht beweisen: Jede
quadratische Matrix, deren charakteristisches Polynom in Linearfaktoren
zerfaellt, ist aehnlich zu einer sogenannten *Jordan-Matrix*. Eine Jordan-Matrix
ist eine Diagonalmatrix von Bloecken, also nicht nur einzelnen Elementen,
sondern quadratischen Bloecken. Jeder einzelne Block ist dabei eine
*Bidiagonalmatrix* mit einem (pro Block immer demselben) Eigenwert in der
Diagonalen, und nur Einsen in der kleineren Diagoneln ueber bzw. rechts von der
Hauptdiagonalen:

```
l_i 1 . . . 0
 l_i 1 . . .
   .  . . .
    .  . .
 0	 .  1
	 l_i
```

Zu jedem Eigenwert gibt es dann $m_g(\lambda)$ viele solcher Jordanbloecke,
wobei die Summe der Dimensionen dieser Jordanbloecke fuer einen Eigenwert dann
der algebraischen Vielfachheit $m_a(\lambda)$ entspricht. Das besondere an
dieser Aussage ist einfach, dass wir nicht nur wissen, dass jede Matrix die in
Linearfaktoren zerfaellt aehnlich zu einer oberen/unteren Dreiecksmatrix ist,
sondern sogar wissen, dass diese obere Dreiecksmatrix in Jordan-Normalform ist
bzw. man sie in diese bringen kan..

https://de.wikipedia.org/wiki/Jordansche_Normalform

## Zusammenfassung

Hier die in diesem Kapitel besprochenen Eigenschaften.

 1. Ist $v$ Eigenvektor von $A$ zu Eigenwert $\lambda$, gilt $Av = \lambda v$
 2. Das charakteristische Polynom ergibt sich aus $\chi_A = \det(xI_n - A)$
 3. Eigenraeume findet man durch: $E_\lambda = Kern(A - \lambda I_n)$
 4. $m_a(\lambda)$ (algebraisch) ist die Vielfachheit der Nullstelle $\lambda$
 5. $m_g(\lambda) = \dim(E_\lambda)$
 6. $1 \leq m_g(\lambda) \leq m_g(\lambda)$
 7. $E_\lambda = \{0\}$ gilt nur, wenn $\lambda$ nicht Eigenwert ist
 8. $\dim(E_\lambda)$ ist gleich den Freheitsgraden von $A - \lambda I_n$
 9. Aehnliche Matrizen haben dasselbe charakteristische Polynom.
10. Eigenwerte zu verschiedenen Eigenraeumen sind linear unabhaengig.
11. Ist $v$ Eigenvektor zu Eigenwert $\lambda$, gilt das auch fuer Vielfachen $av$
12. Eine Matrix $A$ ist genau dann diagonalisierbar, wenn gilt:
     1. $A$ zerfaellt in Eigenwerte aus demselben Raum wie $A$
     2. $m_a(\lambda) = m_g(\lambda) \forall \lambda$
     3. Die Basisvektoren der Eigenraeume von $A$ bilden eine Basis des $K^n$.
13. Ist eine Matrix $A$ diagonalisierbar, so ist $A$ aehnlich zu einer
    Diagonalmatrix $\diag(\lambda_1, ..., \lambda_r)$.
14. $\sum_\lambda m_g(\lambda) = \sum_\lambda m_a(\lambda) = n$
15. Jede Matrix, dessen charakteristisches Polynom in Linearfaktoren zerfaellt,
    ist aehnlich zu einer oberen Dreiecksmatrix, mit Eigenwerten in der
    Diagonalen (und diese kann in Jordan-Normalform gebracht werden).
