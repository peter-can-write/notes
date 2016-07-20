# Matrizen

Matrizen sind der wichtigste "Datentyp" der linearen Algebra. Im folgenden
wollen wir ihre Definition und Eigenschaften studieren.


## Definition

Sei $K$ ein Koerper, z.B. $\mathbb{N}, \mathbb{Z}$ oder $\mathbb{R}$. Seien dann
$m,n$ zwei *Dimensionen* $\in \mathbb{N}$. Eine $m \times n$ Matrix $A$ ist
formal eine Abbildung $\{1,...,m\} \times \{1,...,n\} \rightarrow K$. Wir bilden
also Paare von Indizes $(i, j)$ auf einen Wert im Koerper ab. Das Bild von
$(i,j)$ bezeichnen wir dabei mit $a_{i,j}$. Das schreiben wir so an:

$$
A =
\begin{bmatrix}
a_{1,1} & a_{1,2} ... & a_{1,n} \\
\vdots & \vdots & ... & \vdots  \\
a_{m,1} & a_{m,2} & ... & a_{m,n}
\end{bmatrix}
$$

Oder intensional: $A = (a_{i,j})_{i \leq m, j \leq n}$

Im Weiteren bezeichnen wir die Menge *aller* $m \times n$ Matrizen mit $K^{m
\times n}$. Dann sprechen wir also beispielsweise von einer Matrix $A \in K^{m
\times n}$.

Wir haben dann auch noch spezielle Namen fuer Matrizen, die in einer Dimension
nur eine Komponente haben. Naemlich nennen wir:

* $1 \times n$ Matrizen $(a_1,...,a_n) \in K^{1 \times n}$ *Zeilenvektoren*,
* $m \times 1$ Matrizen $\begin{bmatrix}a_1\\ \vdots\\a_2\end{bmatrix} \in K^{m
  \times 1}$ *Spaltenvektor*. Das schreiben wir oft auch als $(a_1, ...,
  a_n)^\top$.

Meistens betrachten wir Vektoren als Spaltenvektoren, daher nennen wir den $K^{m
\times 1}$ Raum auch einfach den $m$-dimensionalen *Standardraum* $K^m$. Daher
nennen wir Spaltenvektoren oft auch einfach *Vektoren*.

Haben wir eine Matrix $A = (a_{i,j}) \in K^{m\times n}$, dann ist $(a_{i,1},
..., a_{i,n}) \in K^{1 \times n}$ die $i$-te *Zeile* und $\begin{bmatrix}a_{1,j}
\\ \vdots \\ a_{i,n}\end{bmatrix} \in K^m$ die $j$-te Spalte.

Eine weitere Transformation einer Matrix, die wir vornehmen koennen, ist die
*Transponierung* der Matrix. Ist $A = (a_{i,j}) \in K^{m \times n}$ eine Matrix,
so ist $A^\top = (a_{j,i}) \in K^{n \times n}$ die transponierte Matrix. Sie
geht aus der urspruenglichen Matrix hervor, indem man die Zeilen und Spalten der
Matrix vertauscht.

Gilt fuer eine Matrix $A \in K^{m \times n}$, dass $m = n$, so nennen wir die
Matrix *quadratisch*. Gilt fuer eine quadratische Matrix im Weiteren, dass
$A^\top = A$, sodass $a_{i,j} = a_{j,i} \forall i,j$, dann heisst so eine Matrix
*symmetrisch*.

Ein Beispiel fuer eine Matrix in der echten Welt waere eine
*Distanzmatrix*. Sind $S_1,...,S_n$ Staedte und ist $d_{i,j}$ die Entfernung
zwischen $S_i$ und $S_j$, dann ist die Matrix $D = (d_{i,j}) \in R^{n \times n}$
die Distanzmatrix. In den meisten Anwendungen, aber nicht immer, ist diese
Matrix symmetrisch.

## Operationen

Im folgenden wollen wir die ueblichen arithmetischen Operationen auf Matrizen
untersuchen. Manche sind einfach elementweise definiert, manche haben eine
komplexere Definition.

### Summe

Die Summe von Matrizen berechnet man, indem man seine Elemente
*komponentenweise* (*elementweise*) addiert. __Diese Summe ist nur fuer Matrizen
gleicher Dimensionen definiert__.

Seien also $A = (a_{i,j}) \in K^{m \times n}$ und $B = (a_{i,j}) \in K^{m \times
n}$ zwei Matrizen. Dann bezeichnet $C = A + B = (c_{i,j}) \in K^{m \times n}$
ihre Summe. Sie ergibt sich elementweise durch:

$$c_{i,j} = a_{i,j} + b_{i,j}$$

Da diese Operation elementweise definiert ist, gelten fuer die Matrizensumme die
selben Eigenschaften, wie fuer die Addition auf Zahlen:

1. Die Assoziativitaet gilt: $A + (B + C) = (A + B) + C$
2. Die Kommutativitaet gilt: $A + B = B + A$

Ebenso wie die Summe auf Zahlen ein neutrales Element hat, gibt es auch fuer
Matrizen ein neutrales Element. Dieses ist die *Nullmatrix* $$(a_{i,j}) \in K^{m
\times n}, a_{i,j} = 0 \forall i,j$$. Man schreibt sie einfach als $0$, und sie
hat einfach immer implizit die Dimensionen der anderen Matrix. Dann:

* $A + 0 = A$
* $-A = (-a_{i,j})$, dann $-A + A = 0$

Hierbei ist $-A$ also das additive Inverse von $A$, welches ebenso aus
elementweiser Negation hervorgeht.

### Produkt

Das Produkt von Matrizen ist etwas weniger intuitiv, da es insbesondere *nicht*
elementweise definiert ist. Seien also $A = (a_{i,j}) \in K^{m \times n}$ und $B
= (b_{i,j}) \in K^{n \times l}$ zwei Matrizen. Dann ergibt sich das Produkt $C =
A \cdot B = AB$ der beiden Matrizen so, dass:

$c_{i,j} = \sum_{k=1}^n = a_{i,k} \cdot b_{k,j}$

Fuer die $i,j$-te Komponente bilden wir also das Skalarprodukt aus der $i$-ten
Zeile der linken Matrix und $j$-ten Spalte der rechten Matrix. Das bedeutet
wiederum, dass wir ueber jeden Eintrag $k$ bzw. $i,k$ in der linken Zeile
iterieren, den Eintrag mit dem entsprechenden $j$-ten Eintrag in der rechten
Spalte multiplizieren, und dann die Summe aller dieser einzelnen Produkte
bilden.

Man bemerkt hierbei, dass es also insbesondere notwendig ist, dass die linke
Matrix gleich viele Spalten hat, wie die rechte Matrix Zeilen hat. Eine weitere
Invariante ist, dass das Produkt einer $m \times n$ Matrix mit einer $n \times
l$ Matrix eine $m \times l$ Matrix ergibt (fuer jede Zeile links eine Zeile in
der Ausgabe, fuer jede Spalte rechts eine Spalte in der Ausgabe).

#### Permutationsmatrizen

Es gibt eine spezielle Klasse von Matrizen, genannt *Permutationsmatrizen*,
welche nur aus Nullen und Einsen besteht. Diese Einsen sind so geschickt
platziert, dass sie die Elemente der anderen Matrix nur umordnen, ohne sie zu
veraendern. Beispiel:

$$\begin{bmatrix}0 & 1 \\ 1 & 0\end{bmatrix} \cdot \begin{bmatrix}x_1 \\
x_2\end{bmatrix} = \begin{bmatrix}x_2 \\ x_1\end{bmatrix}$$

Wie man sieht, wurden die beiden Komponenten des Vektors durch die
Permutationsmatrix vertauscht. Die erste Zeile *selektiert* naemlich aus dem
Vektor die untere Komponente, und gibt sie in die obere Komponente des
Resultats. Analog selektiert die untere Zeile die obere Komponente des Vektors
und gibt sie in die untere Komponenten (weil es die zweite Zeile der Matrix ist)
des Ausgabevektors.

#### Matrix-Vektor Produkt

Ein interessantes Produkt ist das einer Matrix $A = (a_{i,j}) \in K^{m \times n}
$ mit einem Vektor $v = (x_1, ..., x_n)^\top \in K^n$. Es ergibt einen neuen
Vektor $y$, wobei die $i$-te Komponenten dieses Vektors auf folgende Weise
hervorgeht:

$$y_i = \sum_{j=1}^n a_{i,j} \cdot x_j$$

#### Matrix-Skalar Produkt

Das Produkt einer Matrix mit einem Skalar ist aequivalent mit der Multiplikation
jeder Komponente der Matrix mit diesem Skalar. Sei also $A = (a_{i,j}) \in K^{m
\times n}$ eine Matrix und $s \in K$ ein Skalar. Dann ist das Produkt $C = s
\cdot A \in K^{m \times n}$ definiert durch

$$c_{i,j} = a_{i,j} \cdot s$$.

#### Skalares Produkt

Das Produkt zweier Vektoren $v = (x_1, ... , x_n)^\top$ und $w = (y_1, ...,
y_n)^\top$ ergibt einen *Skalar*! Wir notieren es entweder als $v^\top w$ oder
$\langle v, w \rangle$. Es ergibt sich eben durch:

$$s = \sum_{i=1}^n x_iy_i$$

#### Elementweises Produkt

Das elementweise Produkt zweier Matrizen $A, B \in \mathbb{K}^{m \times n}$ mit
denselben Dimensionen nennt man auch *Haddamard* Produkt und wird mit $\circ$
notiert. Hierbei gilt also:

$$c_{i,j} = a_{i,j} \cdot b_{i,j}$$

#### Beispiel

Sei $A = (a_{i,j}) \in \mathbb{n \times n}$ die *Adjazenzmatrix* eines
Graphen. Dann gibt eine Eins in der Komponente $i,j$ also an, ob es einen
direkten Pfad (der Laenge 1) von $i$ nach $j$ gibt.

Interessant ist nun, dass $A^n$ fuer $n \in \mathbb{N}$ angibt, wieviele Pfade
es exakt der Laenge $n$ fuer jeweils $i,j$ gibt. Die Idee ist die folgende:
Betrachten wir die Komponenten $i,j$ von $A^{n + 1} = A^n \cdot A$. Diese
Komponente entsteht also aus dem Skalarprodukt der $i$-ten Zeile von $A^n$ mit
der $j$-ten Spalte von $A$. Wieso? Nun ja, wir betrachten die jeweils $i$-te
Zeile von $A^n$, und $k$-te Komponente dieser Zeile. Sie gibt an, wie viele
Pfade der Laenge $n$ es von $i$ nach $k$ gibt. Sagen wir, hier steht eine 1,
d.h. es gibt exakt einen Pfad der Laenge exakt $n$ von $i$ nach $k$. Fuer die
Komponente $i,j$ in $A^{n + 1}$ multiplizieren wir nun diese $k$-te Komponente
der Zeile aus $A^n$ mit der $k$-ten Komponente der $j$-ten Spalte von $A$. Sagen
wir, in dieser $k,j$-ten Komponente von $A$ steht eine 2. Das sagt uns, dass es
zwei Pfade der Laenge $1$ von $k$ nach $j$ gibt. Multiplizieren wir nun $i,k$
von $A^n$, also $1$, mit $k,j$ von $A$, also $2$, dann erhalten wir $2$. Wieso?
Nun ja, wir haben einen Pfad der Laenge $n$ von $i$ nach $k$ und zwei Pfade der
Laenge $1$ von $k$ nach $j$, also muessen wir *insgesamt* $1 \cdot 2 = 2$ Pfade
der Laenge $n + 1$ von $i \rightarrow k \rightarrow j$ haben.

Interessant ist auch, dass in der Diagonalen von $A^2$ die Grade des $i$-ten
Knotens stehen. Das sind naemlich alle Pfade der Laenge zwei von $i$ nach $i$,
welche sich daraus ergeben, dass wir zu jedem adjazenten Knoten einmal hingehen,
und dann wieder zurueck.

#### Eigenschaften

Die erste Eigenschaft, die wir zu Matrizen sagen koennen, ist, dass $\langle
K^{m \times n}, +\rangle$ eine abelsche Gruppe ist. Das folgt ganz einfach aus
der elementweisen Definition der Addition.

Seien im Weiteren $A,B \in K^{m \times n}$ zwei Matrizen und $s,s' \in K$
Skalare, dann gilt:

* $s \cdot (A + B) = s \cdot A + s \cdot B$.

Skalare distribuieren also ueber Matrizen. Aehnlich gilt:

* $(s + s') \cdot A = s \cdot A + s' \cdot A$ (Matrizen distribuieren ueber Skalare)
* $s \cdot (s' \cdot A) = s \cdot s' + s \cdot A$ (Skalare distribuieren wo geht)
* $1 \cdot A = A$ (die Eins ist das neutrale Element der Skalarmultiplikation)

Seien $A,B,C$ Matrizen passender Zeilen- und Spaltenzahlen, dann gelten folgende
Eigenschaften:

* Assoziativitaet: $(A \cdot B) \cdot C = A \cdot (B \cdot C)$
* Distributivitaet von $\cdot$ ueber $+$: $A \cdot (B + C) = A \cdot B + A \cdot
  C$ bzw. $(A + B) \cdot C = A \cdot C + B \cdot C$
* Fuer Einheitsmatrizen $I_n = (a_{i,j}) \in K^{n \times n}, a_{i,j}$ gilt, dass
  sie den anderen Operanden erhaelt: $I_n \cdot A = A, B \dot I_n = B$. Sie sind
  also das neutrale Element der Matrixmultiplikation.

Viele dieser Eigenschaften lassen sich aus den Koerpereigenschafen herleiten.

Allerdings: __Das Produkt von Matrizen ist *nicht* kommutativ__! Im Allgemeinen
ist $A \cdot B \neq B \cdot A$. Fuer wohldefiniertes $A \cdot B$ muss $B \cdot
A$ nicht definiert sei. Aber auch fuer quadratische Matrizen hat man hier keine
sichere Gleichheit. Ein weiterer Fall, wo beide Operationen definiert aber nicht
zwingend gleich sind ($AB$ und $BA$), ist wenn $A \in K^{m \times n}$ und $B \in
K^{n \times m}$. $AB$ ist dann eine $m \times m$ Matrix und $BA$ eine $n \times
n$ Matrix.

## Zusammenfassung

Seien $A \in K^{m \times n}$ und $B \in K^{n \times k}$ zwei Matrizen, dann
gilt:

* $A + B$ ergibt sich elementweise. Hierfuer muss $m = k$.
* $A \circ B$ ergibt sich elementweise (Hadamard Produkt). Hierfuer muss $m = k$.
* $s \cdot A$ ergibt sich elementweise.
* $A \cdot B$ ergibt sich durch Skalarprodukt jeder Zeile von $A$ mit jeder
  Spalte von $B$.
* $A \cdot B \in K^{m \times k}$
* $A$ muss gleich viele Spalten haben wie $B$ Zeilen hat.
* $A^\top$ ergibt sich durch Vertauschen von Zeilen mit Spalten.
* Gilt $A = A^\top$, so nennt man $A$ symmetrisch.
* $Av \in K^m$ fuer Vektor $v \in K^n$
