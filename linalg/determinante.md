# Determinanten

Hier wollen wir nun Determinanten untersuchen, welche eine wichtige Operation
auf Matrizen sind und viele Anwendungen haben. Determinanten sind ueber
Permutationen definiert, also werden wir zunaechst Permutationen untersuchen.

$\newcommand{\det}{\mathop{\rm det}\nolimits}$
$\newcommand{\sgn}{\mathop{\rm sgn}\nolimits}$
$\newcommand{\dim}{\mathop{\rm dim}\nolimits}$

## Permutationen

Fuer $n \in \mathbb{N}$ ist die *symmetrische Gruppe* definiert als $S_n =
\{\sigma: \{1, ..., n\} \rightarrow \{1, ..., n\} \,|\, \sigma ist bijekitv\}$.
Die Elemente von $S_n$ heissen *Permutationen*, wobei $S_n$ also *alle*
Permutationen von $n$ Zahlen enthaelt. Eine Permutation ist also eine Abbildung
von $n$ Elementen (isomorph zu $[n]$) auf denselben Raum, also dieselben
Elemente. Sofern die Permutation nicht die Identitaet ist, wird ein Element
hierbei auf ein anderes abgebildet, wodurch sich fuer $n$ Elemente dann eine
Permutation ergeben wuerde. Wichtig hierbei ist, dass die Abbildung bijektiv
ist, also insbesondere umkehrbar.

Da es $n!$ viele Permutationen von $n$ Elementen gibt, ist $|S_n| = n!$. Die
Verknuepfung fuer die Gruppe ist die Komposition, also ist $\langle S_n, \circ
\rangle$ hier die Algebra. Fuer eine Permutation $\sigma \in S_n$ gibt es nun
noch zwei Merkmale, die man wie folgt definiert:

* $w(\sigma)$ ist die Anzahl der Paare $(i, j) \in \mathbb{N}^2$ mit $1 \leq i <
  j \leq n$ aber $\sigma(i) > \sigma(j)$. Das sind die sogenannten *Fehlstellen*
  der Permutation. In Worten: das sind jene Paare $(i, j)$, wo $i$ in der
  urspruenglichen Folge vor $j$ kam, aber in der Permutation dann nach $j$
  kommt. Sie wurden also sozusagen vertauscht.

* $\sgn(\sigma) = (-1)^{w(\sigma)}$ das Vorzeichen (Signum) von $\sigma$. Wenn
  die Anzahl der Fehlstellen gerade ist, dann ist das Signum also $1$ (positiv) und
  wenn die Anzahl der Fehlstellen ungerade ist, ist das Signum $-1$ (negativ).

### Schreibweisen

Es gibt zwei Schreibweisen fuer Permutationen: die *Matrixschreibweise* und die
*Zyklenschreibweise*.

#### Matrixschreibweise

In der Matrixschreibweise schreiben wir die Permutation $\sigma$ als eine $2
\times n$ Matrix an, wobei wir in der ersten Zeile die Elemente $a_i$ notieren
und in der zweiten Zeile dann die Bilder $\sigma(a_i)$ der Elemente
angeben. Beispielsweise sei $n = 5$, dann waere eine moegliche Permutation in
Matrixschreibweise:

$$
\pi =
\begin{bmatrix}
1 & 2 & 3 & 4 & 5 \\
4 & 5 & 2 & 1 & 3 \\
\end{bmatrix}
$$

Hierbei wird also $\sigma(1) = 4$, $\sigma(2) = 5$ und so weiter abgebildet. Die
Umkehrung der Permutation erhaelt man dann also, indem man die Zeilen der Matrix
einfach vertauscht:

$$
\pi =
\begin{bmatrix}
4 & 5 & 2 & 1 & 3 \\
1 & 2 & 3 & 4 & 5 \\
\end{bmatrix}
$$

dann kann man noch neu sortieren:

$$
\pi =
\begin{bmatrix}
1 & 2 & 3 & 4 & 5 \\
4 & 3 & 5 & 1 & 2 \\
\end{bmatrix}
$$

#### Zyklenschreibweise

In der Zyklenschreibweise notieren wir eine Permutation als eine Menge von
*Zyklen*. Ein Zyklus der Laenge $l \in \mathbb{N}$ in eienr Permutation von $n$
Elementen ist ein $l$-Tupel, fuer welches gilt, dass nach $l$ Permutationen der
$n$ Elemente jedes der Elemente im Zyklus genau einmal auf jedes andere Element
im Zyklus abgebildet wurde und dann nach der $l$-ten Permutation wieder auf sich
selbst abgebildet wird.

Ein Zyklus ist also definiert ueber eine $l$-elementigen Teilmenge $\{a_1, ...,
a_l\}$ der $n$-elementigen Urmenge $A$, sodass die Elemente dieser Teilmenge
paarweise verschieden sind. Weiters muss fuer diese Elemente gelten, dass die
Position des Elements an der Stelle $i$ nach der naechsten Permutation, gleich
der Stelle der Stelle des naechsten Elements, modulo der Laenge ist:

$$\sigma(a_i) = a_{i + 1 \mod l} \forall i \in [l]$$

Ebenso wird das Element an Stelle $i$ durch die naechste Permutation auf das
Element an Stelle $i + 1 \mod l$ abgebildet. Hat man also eine Menge an
Elementen $A = \{1, 2, 3\}$ und einen Zyklus $(1 2 3)$, so gibt es nach der
ersten Anwendung der Permutation auf $A$ einen Zyklus $(3 1 2)$. Hierbei wurde
also $1 \mapsto 2, 2 \mapsto 3$ und $3 \mapsto 1$ abgebildet.

Eine Eigenschaft von Zyklen ist, dass man sie rotieren kann, ohne den Zyklus zu
veraendern. So ist also $(1 2 3)$ der selbe Zyklus wie $(3 1 2)$. Man darf die
Elemente eines Zyklus also rotieren, aber *nicht vertauschen*:

$$(1, 2, 3) = (2, 3, 1) = (3, 2, 1) \neq (2, 1, 3) = (3, 2, 1) = (1, 3, 2)$$

Die Zyklenschreibweise ist also nicht eindeutig. Man sollte sie wenn moeglich
aber soriteren, sodass das kleinste Element ganz links steht.

Das namensgebende an einem Zyklus $\mathcal{Z}$ mit $l = |\mathcal{Z}|$ ist,
dass jedes Element an jeder Stelle nach $l$ Permutationen wieder an der
urspruenglichen Stelle stehen wird. Z.B. sei $\mathcal{Z} = (1 2 3)$, dann gilt:

$$\sigma(\sigma(\sigma(1))) = 1$$

Allgemein gilt fuer ein Element $x$ eines Zyklus der Laenge $l$ in einer
Permutation $\sigma$ dann:

$$\sigma^l(x) = x$$

Elemente, die von einer Permutation immer auf sich selbst abgebildet werden,
nennt man dann *Fixpunkte*. Sie ergeben also Zyklen der Laenge $1$, z.B. $(3)$.

Eine Permutation kann nun in Zyklenschreibweise als eien Menge von Zyklen
angegeben werden, wobei die Reihenfolge der Zyklen egal ist: Beispielsweise ist

$$\sigma = (1)(4 2)(3 6 5)$$

eine gueltige Permutation der Elemente in $[6]$. Da wir also Zyklen
untereinander vertauschen duerfen und innerhalb eines Zyklus rotieren duerfen,
gilt hier:

$$\sigma = (1)(4 2)(3 6 5) = (6 5 3)(1)(4 2)$$

Hier waere $1$ also z.B. ein Fixpunkt. Die zweielementigen Zyklen nennt man
*Transpositionen*. In Matrixschreibweise wuerde diese Permutation so aussehen:

$$
\sigma =
\begin{bmatrix}
1 & 2 & 3 & 4 & 5 & 6 \\
1 & 4 & 6 & 2 & 3 & 5 \\
\end{bmatrix}
$$

Die Umkehrung einer Permutation in Zyklenschreibweise ergibt sich daraus, dass
man die Zyklen einfach jeweils *spiegelt*. Also fuer die obige Permutation
$\sigma$ erhielte man die Umkehrung $\sigma^{-1}$ als:

$$\sigma^{-1} = (1)(2 4)(6 6 3)$$

Hier sieht man:

1. Die Fixpunkte bleiben gleich.
2. Die Umkehrung von Transpositionen (Zyklen der Laenge zwei) ergibt denselben
   Zyklus, denn hier ist $(2 4)$ nun dasselbe wie $(4 2)$ (einmal rotiert).

### Beispiele

* Die Identitaet $id \in S_n$ hat keine Fehlstellen, also ihr Signum
  $\sgn(id) = (-1)^0 = 1$

* Sei $\sigma \in S_n$ mit $\sigma(1) = 2, \sigma(2) = 1, \sigma(i) = i$
  sonst. Dann ist das Signum $\sgn(\sigma) = (-1)^1 - -1$.

* Seien $1 \leq i < j < \leq n$ und $\sigma \in S_n$ eine Permutation, und
  vertausche nur $i$ und $j$ und halte alle anderen Elemente fest,
  d.h. $\sigma(i) = j, \sigma(j) = i, \sigma(l) = l$ sonst. Dann heisst $\sigma$
  *Transposition*. Die Anzahl der Fehlstellen ist gerade $w(\sigma) = 2(j - i) -
  1$ (minus eins damit wir das Paar $(i, j)$ nicht durch $(j, i)$ doppelt
  zaehlen). Die Zahl $2(j - 1) - 1$ ist immer ungerade, eine Transposition hat
  also immer eine ungerade Anzahl an Fehlstellen und somit immer negatives
  Signum ($-1$).

Ein Beispiel fuer eine Transposition waere die Permutation:

$$(1 2 3 4) \rightarrow (4 2 3 1)$$

Hier haben wir also die folgenden Fehlstellen:

1. $4$ ist falsch bezueglich $2, 3, 1$, weil es vor der Permutation rechts von
   diesen war. Also haben wir schonmal drei Fehlstellen.
2. Symmetrisch ist $1$ falsch bezueglich $4, 2, 3$, weil es vor der Permutation
   links von all diesen Elementen war. Also haetten wir insgesamt sechs
   Fehlstellen.
3. Jetzt haben wir aber fuer $4$ die Fehlstelle $(4, 1)$ gezeahlt, und auch fuer
   $1$ gesagt, dass $(1, 4)$ falsch ist. Das ist aber die selbe Fehlstelle, also
   ziehen wir eins ab.

Insgesamt haetten wir also $2 \cdot (4 - 1) - 1 = 2 \cdot 3 - 1 = 6 - 1 = 5$
Fehlstellen. Wie besprochen ist das Signum $(-1)^{5}$ ungerade, also $-1$.

### Eigenschaften

Eine interessante Eigenschaft ist, dass fuer zwei Permutationen $\sigma, \tau
\in S_n$ gilt:

$\sgn(\sigma \circ \tau) = \sgn(\sigma) \cdot \sgn(\tau).$

Das Signum der Komposition zweier Permutationen ist also gleich dem Produkt der
Signums der einzelnen Permutationen. Drei weitere Eigenschaften des Signums
sind:

1. Nur wenn $\sgn(\sigma) = \sgn(\tau)$ kann $\sgn(\sigma) \cdot \sgn(\tau) = 1$
   gelten, also nur das Produkt gleicher Signums (beide $+1$ oder beide $-1$)
   kann ein Signum von $+1$ ergeben.

2. Das Signum der Inversen einer Permutation ist gleich dem Signum der
   Permutation selbst. Beweis:

   1. $1 = \sgn(Id) = \sgn(\sigma \cdot \sigma^{-1})$
   2. $1 = \sgn(\sigma) \cdot \sgn(\sigma^{-1})$
   3. Aus (1) wissen wir, dass das nur dann gelten kann, wenn
	  $\sgn(\sigma) = \sgn(\sigma^{-1})$.

   Es ist auch intuitiv klar, dass wenn eine Permutation $x$ Fehlstellen hat,
   die Umkehrung die Fehlstellen wieder ruckgaengig macht. Bezueglich der
   Permutation sind diese Umkehrungen dann ja auch wieder Fehlstellen, von
   welchen es folglich wieder $x$ viele gibt.

3. Wir haben also, dass $\sgn(\sigma) \cdot \sgn(\sigma^{-1}) = 1$. Dann muss
   also $\sgn(\sigma^{-1})$ das multiplikative Inverse von $\sgn(\sigma)$
   sein. Somit haben wir insgesamt:
   $\sgn(\sigma) = \sgn(\sigma^{-1}) = \sgn^{-1}(\sigma)$.

Das Signum einer Permutation ist also gleich dem Signum des Inversen der
Permutation und gleich dem Inversen des Signums der Permutation.

## Determinanten

Sei nun $A = (a_{i,j}) \in K^{n \times n}$ eine __quadratische__ Matrix. Dann
ist die Determinante $\det(A)$ von $A$ definiert durch:

$$\det(A) = \sum_{\sigma \in S_n} \sgn(\sigma) \cdot \Pi_{i=1}^n a_{i\sigma(i)}$$

Wir gehen hier also so vor: Fuer jede Permutation $\sigma$ von $n$ (weil $n$ die
Zeilen- und Spaltendimension der Matrix ist), ermitteln wir zuerst das Signum
(also die Anzahl an Fehlstellen und dann das Signum daraus). Dann iterieren wir
ueber alle Zeilen $1 \leq i \leq n$, und fuer jede Zeile betrachten wir dann den
Eintrag $(i, \sigma(i))$. Wir erhalten fuer jede Zeile also einen solchen
Eintrag und bilden das Produkt dieser Eintraege. Dieses Produkt multiplizieren
wir dann noch mit dem Signum, sodass sich das Vorzeichen womoeglich noch
aendert. Dann machen wir das wieder fuer die naechste Permutation, und summieren
so ueber alle Permutationen.

### Beispiele

Fuer $n = 1$ ist $A = (a)$ (ein Eintrag) und $\det(A) = a$, weil $S_1 =
\{\text{Id}\}$ (fuer $n = 1$ Elemente gibt es nur eine Permutation, naemlich die
Identitaet).

Fuer $n = 2$ ist $S_n = \{\id, \sigma\}$. Fuer $(1 2)$ gibt es also nur zwei
Permutationen, einmal die Identitaet $(1 2)$ selbst und einmal eine
*Transposition* $(2 1)$. Dann haben wir also einmal Signum $\sgn(Id) = 1$ und
einmal $sgn(\sigma) = -1$, weil Transpositionen immmer eine ungerade Anzahl an
Fehlstellen und somit negatives Signum haben. Dann ergibt sich also fuer die
Determinante $\det(A)$:

1. $\sgn(Id) \cdot Pi_{i=1}^2 a_{i, Id(i)}$. Das ergibt weiter $1 \cdot (a_{1,1}
   \cdot a_{2,2})$ also einfach das Produkt der ersten Hauptdiagionalen.

2. $\sgn(\sigma) \cdot Pi_{i=1}^2 a_{i, \sigma(i)}$. Hier haben wir nun $-1
   \cdot (a_{1,2} \cdot a_{2,1})$, also das negative des Produkts der zweiten
   Hauptdiagonalen.

3. Insgesamt haben wir also $(a_{1,1} \cdot a_{2,2}) + (-(a_{1,2} + a_{2,1})) =
   a_{1,1}a_{2,2} - a_{1,2}a_{2,1}$. Also die erste Hauptdiagonale minus der
   zweiten Hauptdiagonalen.

Betrachten wir beisipelsweise die folgende Matrix:

$$
A =
\begin{bmatrix}
1 & 2\\
3 & 4\\
\end{bmatrix}
$$

Dann haben wir $\det(A) = (1 \cdot 4) - (2 \cdot 3) = -2$.

### Regel von Sarrus

Die Regel von Sarrus erlaubt eine einfache Moeglichkeit, die Determinante einer
drei-dimensionalen Matrix $A \in K^{3 \times 3}$ zu bestimmen. Fuer den
drei-dimensionalen Raum erweitert man die Matrix einfach um die ersten zwei
Spalten, und subtrahiert die drei zweiten Hauptdiagonalen von den drei ersten
Hauptdiagonalen.

Allgemein haben wir also eine Matrix

$$
\begin{bmatrix}
a_{1,1} & a_{1,2} & a_{1,3} \\
a_{2,1} & a_{2,2} & a_{2,3} \\
a_{3,1} & a_{3,2} & b_{3,3} \\
\end{bmatrix}
$$

Dann erweitern wir sie zuerst um die ersten beiden Spalten nach rechts:

$$
A =
\begin{bmatrix}
a_{1,1} & a_{1,2} & a_{1,3} & a_{1,1} & a_{1,2} \\
a_{2,1} & a_{2,2} & a_{2,3} & a_{2,1} & a_{2,2} \\
a_{3,1} & a_{3,2} & a_{3,3} & a_{3,1} & a_{3,2} \\
\end{bmatrix}
.
$$

Dann erhalten wir die Determinante wie folgt:

$$
\det(A)
= (a_{1,1}a_{2,2}a_{3,3})
+ (a_{1,2}a_{2,3}a_{3,1})
+ (a_{1,3}a_{2,1}a_{3,2})
- (a_{1,3}a_{2,2}a_{3,1})
- (a_{1,1}a_{2,3}a_{3,2})
- (a_{1,2}a_{2,1}a_{3,3})
.$$

Es sei aber angemerkt, dass diese Formel nur fuer dreidimensionale (bzw. auch
zweidimensionale) Matrizen gilt. Fuer hoeherdimesionale Matrizen muss man
explizit mit der Determinantenformel rechnen.

### Eigenschaften der Determinante

Wir koennen nun einige nuetzliche und sogar interessante Eigenschaften fuer die
Determinante betrachten. Sei hierzu $A = (a_{i,j}) \in K^{n \times n}$ eine
quadratische Matrix. Dann gelten fuer diese Matrix bestimmte Eigenschaften, die
in den naechsten Abschnitten naher betrachtet werden.

#### $\det(A) = \det(A^\top)$

Es gilt also schonmal, dass die Determinante einer Matrix $A$ gleich der
Determinante der transponierten Matrix $A^\top$ ist. Die Grundintuition ist die,
dass sich die Werte von $A$ ja nicht veraendern, wenn wir die Matrix
transponieren. D.h. wenn wir eine Permutation $\sigma$ betrachten, die fuer jede
Zeile eine Spalte $a_{i, \sigma(i)}$ selektiert, so gibt es auch eine
entsprechende Permutation $\sigma'$, die genau diese Eintrage in der
transponierten Matrix auswaehlt. Wir koennen ja beobachten, dass bei der
Determinante aus jeder Zeile ein Eintrag selektiert wird. Wichtig hierbei ist
nun auch, dass auch *aus jeder Spalte* nur ein Eintrag selekteirt wird. Eine
Permutation ist naemlich eine *Bijektion*. Gibt es eine Zeile, die auf eine
Spalte abgebildet wird, kann es keine weitere Zeile geben, die auch auf diese
Zeile abgebildet wird. Somit kann man sich intuitiv vorstellen, dass wenn die
durch die Permutation selektierten Eintraege in der Matrix hervorgehoben sind,
und wir die Matrix transponieren, wir eine neue Permutation erhalten, die ebenso
noch aus jeder Zeile (vorher Spalte) genau eine Spalte (vorher Zeile)
selektiert. Ueber die Summe aller moeglichen Permutationen erhalten wir somit
also letzendlich genau dieselbe Summe.

Formal:

1. $\det(A^\top) = \sum_\sigma \sgn(\sigma) \cdot \Pi_{i=1}^n a_{\sigma(i),
   i}$. Hierbei sind die $i$ und $\sigma(i)$ also vertauscht, weil bei $A^\top$
   die Zeilen und Spalten vertauscht sind. Wir selektieren also fuer die $i$-te
   Spalte (vorher die Zeile) die $\sigma(i)$-te Zeile (vorher die entsprechende
   Spalte).

2. $\sigma_{\sigma in S_n} \sgn(\sigma) \cdot \Pi_{j=1}^1
   a_{j,\sigma^{-1}(j)}$. Allgemein: wenn wir fuer $A$ ueber die Zeilen
   iterieren und durch die Permutation immer eine Spalte auswaehlen, dann ist
   das dasselbe, wie wenn wir ueber die Spalten iterieren, und durch die
   Umkehrung der Permutation (da sie ja bijektiv ist) die Ursrpungszeile
   auswaehlen. Da wir in (1) ja eher noch ueber die Spalten iterieren und die
   Zeile auswaehlen (wir schummeln sozusagen), machen wir hier den Uebergang, um
   ueber die Zeilen zu iterieren und die entsprechende Spalte auszuwaehlen.

3. Sei nun $\tau = \sigma^{-1}$, dann erhalten wir:
   $\sum_{\tau \in S_n} \sgn(\tau^{-1}) \cdot \Pi_{j=1}^n a_{j,\tau(j)}$.
   Da wir also wieder ueber die ganze Menge $S_n$ iterieren, haben wir hier erst
   wieder alle Permutationen. Sie heissen hier nun einfach $\tau$, statt
   $\sigma$. Was hier gerade steht ist naemlich eben die Determinante von $A$,
   also $\det(A)$.


Das macht eben die Intuition klar, dass wir letzendlich ja immer noch ueber alle
Permutationen summieren.

#### Zusammenhang Permutation und Determinante

Sei:

* $A$ eine quadratische Matrix $\in K^{n \times n}$

* $\sigma \in S_n$ eine Permutation.

* $B = (b_{i,j}) \in K^{n \times n}$ mit $b_{i,j} = a_{i,\sigma(j)}$ die Matrix
  die aus der Permutation der *Spalten* von $A$ durch $\sigma$ hervorgeht.

* $C = (c_{i,j}) \in K^{n \times n}$ mit $c_{i,j} = a_{\sigma(i),j}$, die Matrix
  die aus der Permutation der *Zeilen* von $A$ durch $\sigma$ hervorgeht.

Dann gilt:

$$\det(B) = \det(C) = \sgn(\sigma) \cdot \det(A).$$

Begruendung:

Der linke Teil $\det(B) = \det(C)$ ist klar, weil $B = C^\top$. Der rechte Teil,
$\det(B) = \det(A) \cdot \sgn(\sigma)$ ist interessanter: $B$ enthaelt die
selben Zahlen wie $A$, insofern kommen wir also bei beiden Determinanten an
allen Permutationen "vorbei". $\det(B)$ unterscheidet sich also nur dadurch von
$\det(A)$, dass wir $\det(A)$ noch diesen Faktor $\sgn(\sigma)$ geben muessen,
der aus $A$ die Matrix $B$ erzeugt hatte.

Formaler Beweis:

1. $\det(B) = \sum_\tau \sgn(\tau) \Pi_{i=1}^n b_{i,\tau(i)}$

2. $\sum_\tau \sgn(\tau) \cdot \Pi_{i=1}^n a_{i,(\sigma \circ \tau)(i)}$. Wir
   haben $b_{i,j}$ ja so definiert, dass es aus der Permutation der Spalten von
   $A$ hervorgeht, sodass $b_{i,j} = a_{i,\sigma(j)}$. D.h. diese Stelle $(i,j)$
   haben wir fuer $B$ schon einmal permutiert. Jetzt permutieren wir sie in
   $b_{i,\tau(j)}$ noch einmal mit $\tau$, bilden also den Spaltenindex noch
   einmal auf ein anderes Element ab. Relativ zu $A$ hat sich diese konkrete
   Stelle $(i,j)$ jetzt also zwei mal permutiert, sodass an der Stelle
   letztendlich das Element $a_{i, (\sigma\circ\tau)(i)}$ steht.

3. Sei nun $\rho := \sigma \circ \tau$, dann haben wir:
   $\sum_\rho \sgn(\sigma^{-1}\rho) \circ \Pi_{i=1}^n a_{i,\rho(i)}$.
   Hierebei entsteht $\sigma^{-1}\rho$ daraus, dass wir aus $\rho = \sigma \circ
   \tau$ das $\sigma$ wieder rausnehmen muessen, um $\tau$ zu erhalten.

4. Jetzt wisssen wir aus der Multiplikativitaet des Signums, dass
   $\sgn(\sigma^{-1}\rho) = \sgn(\sigma^{-1})\sgn(\rho)$ gilt. Da $\sigma$ in
   der Summe nicht mehr vorkommt, koennen wir diesen Term $\sgn(\sigma^{-1})$
   dann erst mal nach draussen ziehen. Weiters wissen wir auch, dass
   $\sgn(\sigma^{-1}) = \sgn(\sigma)$. Dann erhalten wir also:
   $\sgn(\sigma) \cdot \sum_\rho \sgn(\rho) \cdot \sum_{i=1}^n a_{i,\rho(i)}$.

5. Da wir mit $\rho$ wieder alle Permutationen von $S_n$ durchiterieren, diesmal
   aber bezueglich der Matrix $A$ (und nicht mehr $B$), erhalten wir also gerade
   wieder die Determinante $\det(A)$.

6. Letztendlich haben wir also: $\sgn(\sigma) \cdot \sgn(A)$, wo $\sigma$ eben
   die Permutation ist, aus welcher $B$ von $A$ hervorgeht.

#### Gleichheit zwischen Zeilen oder Spalten

Falls in einer Matrix $A$ zwei (oder mehr) Zeilen oder Spalten uebereinstimmen,
folgt, dass ihre Determinante gleich Null ist. Hierfuer genuegt es, $\det(A) =
0$ fuer den Fall gleicher *Spalten* zu zeigen, da ja die Determinante von
$A^\top$ gleich jener von $A$ ist.

Fuer den Beweis nehmen wir uns dann ohne Beschraenkung der Allgemeinheit zwei
Spalten $j$ und $k$ aus den $n$ Spalten der quadratischen Matrix. Die Matrix sei
dann genau in diesen beiden Spalten gleich, sodass also gilt

$$a_{i,j} = a_{i,k} \forall i \in \{1, ..., n\}.$$

Das entspricht also einer Transpositionspermutation $\tau \in S_n$ definiert
durch $\tau(j) = k, \tau(k) = j, \tau(x) = x$ sonst. Sie vertauscht also in
jeder Zeile die Eintrage der beiden Spalten und laesst alle anderen Spalten
gleich. Da wir nur $j$ und $k$ "anfassen" und $j,k$ *ueberall gleich sind*, ist
$\tau$ gerade die Identitaetsfunktion. Also gilt $a_{i,l} = a_{i,\tau(l)}$.

Dann brauchen wir fuer den Beweis noch die Definition der *alternierenden
Gruppe* $A_n$. Diese alterniernde Gruppe ist einfach die Menge aller
Permutationen mit positivem Signum, also:

$$A_n = \{\sigma \in S_n \,|\, \sgn(\sigma) = 1\}$$.

Da $\tau$ oben eine Transposition war, folgt also nach Definition von
Transpositionen, dass $\sgn(\tau) = -1$. Eine Transposition kann in der
alternierenden Gruppe also sicher nicht enthalten sein. Somit koennen wir nun
die Menge *aller* Permutationen $S_n$ alleine durch $\tau$ und $A_n$
ausdruecken. $S_n$ ist dann naemlich gleich der Menge, die entsteht, wenn man
die Transposition $\tau$ einfach auf die ganze alternierende Gruppe
draufkomponiert. Dann haben wir naemlich fuer jede Permutation mit positivem
Signum auch noch die entsprechende Permutation, die aus der mit positivem Signum
hervorgeht, indem man noch die zwei Spalten $j,k$ vertauscht (da in diesen
Spalten ueber die Permutationen mal alle Spalten der urspruenglichen Matrix
auftauchen, kommt man so einmal an jeder Permutation mit negativem Signum
vorbei). Diese Menge ist dann also:

$$S_n = A_n \cup \tau \circ A_n.$$

Dann sehen wir uns einfach an, wie wir $\det(A)$ mit dieser Definition von $S_n$
fuer den Beweis passend umformen koennen:

1. $
    \det(A) =
    \sum_{\sigma \in A_n}(\sgn(\sigma)\Pi_{i=1}^n a_{i,\sgn(i)} +
    \sgn(\tau\sigma) \cdot \Pi_{i=1}^n a_{i,\tau\sigma(i)})
   $.

   Wir iterieren also gleichzeitig ueber die Permutationen $\sigma$ mit
   positivem Signum aus der alternierenden Gruppe, sowie auch ueber die
   Permuationen $\sigma \circ \tau$ aus der alternierenden Gruppe, mit $\tau$
   komponiert.

2. $
    \det(A) =
	\sum_{\sigma \in A_n}\sgn(\sigma)(\Pi_{i=1}^n a_{i,\sgn(i)} +
	\sgn(\tau) \Pi_{i=1}^n a_{i,\tau\sigma(i)})
   $.
   Was wir hier gemacht haben, ist den Ausdruck $\sgn(\tau\sigma)$ nach der
   Multiplikativitaet des Signums aufzuspalten in $\sgn(\tau)\sgn(\sigma)$. Da
   $\sgn(\sigma)$ dann sowohl vor der ersten Produktserie, also auch der zweiten
   steht (nach vorschieben), koennen wir es auch aus der Summe herausheben.

3. $
    \det(A) =
	\sum_{\sigma \in A_n}\sgn(\sigma)(\Pi_{i=1}^n a_{i,\sgn(i)} -
	\Pi_{i=1}^n a_{i,\tau\sigma(i)})
   $.

   Wir wissen, dass $\sgn(\tau) = -1$, weil $\tau$ ja eine Transposition
   ist. Also setzen wir entsprechend $-1$ ein und erhalten eine
   Subtraktion.

3. $
    \det(A) =
	\sum_{\sigma \in A_n}\sgn(\sigma)(\Pi_{i=1}^n a_{i,\sgn(i)} -
	\Pi_{i=1}^n a_{i,\sigma(i)})
   $.

   Nun wissen wir noch aus der Definition von $\tau$, dass es fuer diese
   besondere Matrix mit den vertauschen Spalten wie die Identitaet wirkt, weil
   in den vertauschen Spalten ja dieselben Eintraege stehen und andere Spalten
   nicht veraendert werden. Also gilt hier $a_{i,\tau\sigma(i)} =
   a_{i,\sigma(i)}$. Dann steht in den Klammern links und rechts vom Minus
   gerade dieselbe Produktserie. Somit wird aus der ganzen Summe eine grosse
   glorreiche Null und der Beweis ist perfekt.

So haben wir also gezeigt, dass __wenn eine Matrix zwei (oder mehr) gleiche
Spalten (oder Zeilen) hat, ihre Determinante automatisch Null ist.__

#### Multiplikativitaet der Determinante

Eine weitere interessante Eigenschaft der Determinante ist, dass sie
multiplikativ ist, also:

$$\det(A \cdot B) = \det(A) \cdot \det(B).$$

Daraus folgt insbesondere dann:

1. $1 = \det(Id)$
2. $1 = \det(A \cdot A^{-1})$ (Multiplikativitaet)
3. $1 = \det(A) \cdot \det(A^{-1})$
4. Daraus folgt: $\det(A^{-1}) = \det(A)^{-1} = \frac{1}{\det(A)}$.

Das Inverse der Determinanten einer Matrix $A$ ist also die Determinante der
inversen Matrix $A^{-1}$.

#### Additivitaet der Determinante

Die Determinante ist aber nicht additiv, also:

$\det(A + B)$ nicht unbedingt gleich $\det(A) + \det(B)$

Geeignetes Gegenbeispiel:

1. $\det(I_2 + I_2) = \det(\begin{bmatrix}2 & 0 \\ 0 & 2\end{bmatrix}) = 4$
2. $\det(I_2) + \det(I_2) = 1 + 1 = 2$

## Die vielen Aequivalenzen der linearen Algebra

Wir sind nun in der Lage, viele Konzepte der linearen Algebra zu verknuepfen und
eine Reihe von Aussagen als aequivalent bewerten. Hierzu muessen wir zuerst
Vorarbeit leisten, um zu zeigen, dass wenn eine Matrix regulaer ist, ihre
Determinante Null sein muss.

### Vorarbeit

Wir zeigen: Fuer eine quadratische Matrix $A \in K^{n \times n}$ gilt die
Aequivalenz:

$$A \text{ ist regulaer } \iff \det(A) \neq 0.$$

Wir zeigen nun beide Richtungen einzeln.

#### $\Rightarrow$

Wir zeigen: hat $A$ vollen Rang, so muss die Determinante dieser Matrix ungleich
Null sein. Wir wissen, dass $A$ nicht nur regulaer ist, sondern auch
quadratisch. Das bedeutet, dass $A$ auch eine Inverse hat (inbesondere weil $A$
regulaer ist). Weiters wissen wir:

$$\det(A^{-1}) = \det(A)^{-1} = \frac{1}{\det(A)}$$

Da $\det(A^{-1})$ sicher existiert, muss der Bruch rechts mathematisch
definiert sein. Das ist er aber nur, wenn $\det(A) \neq 0$ ist, was hier also
gelten muss.

Anders:

1. $1 = \det(Id)
2. $1 = \det(A A^{-1})$
3. $1 = \det(A) \det(A^{-1})$

Da $1$ nicht aus einem Produkt mit der Null sich ergeben kann, muss also
$\det(A^{-1}) \neq 0$ und insbesondere (was wir zeigen wollten) $\det(A) \neq 0$
sein.

#### $\Leftarrow$

Es gibt zwei Weisen, zu zeigen, dass wenn die Determinante einer Matrix ungleich
Null ist, die Matrix regulaer sein muss.

##### 1. Beweis

Dieser Beweis benutzt das unten besprochene Wissen ueber den Effekt von
elementaren Zeilenoperationen auf die Determinante einer Matrix. Auch benutzen
wir Wissen ueber das Verfahren der *Entwicklung* nach einer Zeile zur Berechnung
der Determinante, was auch weiter unten besprochen wird.

Sei $A$ nicht regulaer, dann wissen wir also, dass wir durch elementare
Zeilenoperationen zumindest eine Zeile durch eine oder viele andere ausdruecken
koennen. Insbesondere koennen wir diese redundante Zeile also zur Nullzeile
machen, indem wir passende Zeilenoperationen durchfuehren.

Weiter wissen wir, dass wir durch Addition des $s$-fachen einer Zeile auf eine
andere Zeile die Determinante der Matrix nicht veraendern. Dann koennen wir nun
also die Determinante der urspruenglichen Matrix durch diese veraenderte
berechnen. Diese (durch Zeilenoperationen) veraenderte Matrix hat nun aber eben
eine Null-Zeile. Wenn wir nach dieser entwickeln, erhalten wir fuer die
Determinante eben die Null.

##### 2. Beweis

Sei $A$ nicht regulaer. Dann gibt es also einen Vektor $v \neq 0$ mit $Av =
0$. D.h. dieser Vektor, welcher nicht der Nullvektor ist, ist eine
*nicht-triviale* eine Loesung des homogenen linearen Gleichungssystems $Ax =
0$. Da $A$ nicht regulaer ist, wissen wir, dass es einen solchen Vektor gibt.

Dann ergaenzen wir $v$ zunaechst zu einer Basis $\{v = v_1, v_2, ..., v_n\}$
(hierbei ist $v$ also gleich dem ersten Vektor $v_1$, nicht der ganzen Sequenz)
von $K^n$ und bilden die Matrix $B = (v_1 | ... | v_n) \in K^{n \times n}$ mit
diesen Basisvektoren $v_i$ als Spalten. Da $B$ nur Basisvektoren enthaelt, sind
die Spalten von $B$ also linear unabhaenging. Da der Spaltenrang gleich dem
Zeilenrang ist, folgt also dass die Matrix regulaer ist.

Da wir die Vorwaertsrichtung $\Rightarrow$ schon wissen, wissen wir also schon,
dass wenn eine Matrix regulaer ist, ihre Determinante ungleich Null sein
muss. Dann koennen wir also sagen: $\det(B) \neq 0$.

Im Weiteren betrachten wir dann das Produkt $(A \cdot B)e_1$, wobei $e_1$ der
Standardvektor $(1, 0, ..., 0)^\top$ ist. Dieser Vektor $e_1$ selektiert bei der
Multiplikation mit einer Matrix gerade die erste Spalte, da alle anderen
Komponenten auf Null gesetzt werden.

1. Versuch einer Erklaerung:

In diesem Fall selektiert er also die erste Spalte von $AB$. Nun ueberlegen wir
noch, was diese erste Spalte ist. Wenn die erste Spalte bei der Multiplikation
von Matrizen generieren, dann multiplizieren wir ja fuer jede Komponenten in der
"Output-Spalte" die ganze Zeile in der linken Matrix mit der ersten Spalte in
der rechten Matrix. Somit ist laesst sich die ausgewaehlte erste Spalte gerade
durch $Av_1$ ausdruecken!

2. Versuch einer Erklaerung:

Wir betrachten $(AB)e_1$. Ueber Assoziativitaet gilt: $(AB)e_1 = A(Be_1)$. Da
$e_1$ die erste Spalte selektiert, und diese erste Spalte der erste Basisvektor
$v_1$ ist, gilt weiter $(AB)e_1 = Av_1$.

Jedenfalls war $v_1$ hierbei der Alias fuer $v$ in der Basisergaenzung. Also
haben wir:

$$(AB)e_1 = Av_1 = Av.$$

Oben haben wir $v$ als eine der nicht-trivialen Loesungen des homogenen LGS
gewaehlt, also gilt weiter $Av = 0$. Da $v$ nicht-trivial ist, ist $v$
insbesondere nicht der Nullvektor. Damit dieses Produkt $Av$ dennoch gleich Null
ist, muss diese selektierte erste Spalte von $AB$ nur aus Nullen besteht
bzw. der Nullvektor sein!

Wir haben also in der ersten Spalte von $AB$ immer eine Null. Die Determinante
von $AB$ wird ja dann so gebildet, dass wir ueber alle Zeile iterieren und die
entsprechende Permutation der Zeile als Eintrag in ein grosses Produkt
nehmen. Das machen wir dann fuer jede Permutation. Da jede Permutation eine
Bijektion ist, muss inbesondere jede Permutation eine Zeile immer auf die erste
Spalte abbilden. Das bedeutet dann, dass ein Eintrag in jedem Produkt
$\Pi_{i=1}^n a_{i,\sigma(i)}$ immer Null ist. Damit wird das ganze Produkt also
Null und somit ist die ganze Summe ueber alle Permutationen immer Null und
dadurch dann weiter die ganze Determinante Null (spaeter dann einfach: wir
entwickeln nach dieser Spalte). Es gilt also:

$$\det(AB) = 0.$$

Dann koennen wir die Multiplikativitaet von $AB$ ausnutzen um zu schreiben:

$$0 = \det(AB) = \det(A) \cdot \det(B)$$

Da $B$ eine Basismatrix war und somit regulaer, war ihre Determinante also
ungleich Null. Damit das Produkt $\det(A) \cdot \det(B)$ also Null wird, muss
$\det(A)$ Null sein!

Somit haben wir also gezeigt, dass wenn die Matrix nicht regulaer ist, die
Determinante immer Null sein muss:

$$A \text{ regulaer } \Rightarrow \det(A) \neq 0.$$

Umgekehrt gilt, also dass wenn die Determinante Null ist, die Matrix nicht
regulaer gewesen sein kann:

$$\det(A) = 0 \Rightarrow A { nicht regulaer }.$$

### Aequivalenzen

Hier sind nun einige Aequivalenzen aufgelistet. Die Idee ist, dass *sobald* eine
dieser Aussage gilt, auch alle anderen gelten muessen.

Fuer eine *quadratische Matrix* $A \in K^{n \times n}$ sind nun aequivalent:

* $A$ ist regulaer

* $A$ ist invertierbar ($A \in GL_n(K)$), weil wir dann naemlich mit dem Gauss
  Algorithmus sicher eine Loesung finden.

* Die Zeilen von $A$ sind linear unabhaengig (weil die Matrix regulaer ist).

* die Spalten von $A$ sind linear unabhaengig (weil die Matrix regulaer ist
  bzw. weil Zeilenrang = Spaltenrang).

* Abbildung $f_A$ ist injektiv (weil es nur eine eindeutige Loesung fuer $f(x) =
  Ax$ geben kann, und weil das homogene LGS $Ax = 0$ eindeutig durch die
  triviale Loesung $0$ loesbar ist. Der Kern von $f_A$ ist also Null, womit
  $f_A$ injektiv ist).

* Abbildung $f_A$ ist surjektiv (weil es fuer jedes $b$ eine eindeutige Loesung
  $f(x) = Ax = b$ gibt; auch: $f_A$ ist endomorph, sodass $\dim(\Urbild) =
  \dim(\Bild)$, da $f_A: K^{n \times n} \rightarrow K^{n \times n}$, sodass
  Injektivitaet gleich Surjektivitaet und Bijektivitaet ist).

* LGS $Ax = 0$ ist eindeutig loesbar (weil $A$ regulaer)

* $\kern(f_A) = \{0\}$ (weil es eben auch fuer das homogene Gleichungssystem nur
  eine eindeutige Loesung gibt, naemlich die triviale; auch weil $f_A$
  injektiv).

* $\forall b \in K^n$ ist das LGS $Ax = b$ eindeutig loesbar (weil $A$ regulaer;
  auch weil $f_A$ surjektiv).

* $\det(A) \neq 0$ (weil $A$ quadratisch und regulaer).

Das bedeutet, dass wenn wir eine quadratische Matrix haben, und nur *eine*
dieser Eigenschaften wissen, wir auch automatisch *alle anderen* Eigenschaften
gratis mitwissen.

### Korollar

Wir koennen un ein einfaches Korollar aus den obigen Aequivalenzen ziehen: Seien
$A,B \in K^{n \times n}$ zwei Matrizen, die aehnlich sin, d.h.

$$\exists S \in K^{n \times n}: B = S^{-1}AS.$$

Wir zeigen, dass dann gilt:

$$\det(A) = \det(B).$$

#### Beweis

Wir haben $B = S^{-1} A S$. Daher gilt:

1. $\det(B) = \det(S^{-1} A S)$
2. $\det(S^{-1}) \det(A) \det(S)$ ($\det(X^{-1}) = \det(X)^{-1}$)
3. $\det(S)^{-1} \det(A) \det(S)$ (Determinante sind Skalare, also darf man sie
   kommutieren)
4. $\det(A) \cdot (\det(S)^{-1}\det(S))$
5. $\det(A)$

Also gilt $\det(B) = \det(A)$

Was uns das intuitiv sagt: Haben wir eine lineare Abbildung $f: V \rightarrow V$
mit endlicher Dimension, so kann man $\det(f) = \det(D_B(f))$ fuer beliebige
Basis $B$ definieren. D.h. wir definieren die Determinante einer Abbildung
einfach als die Determinante einer beliebigen Darstellungsmatrix $D_B(f)$. Es
ist naemlich egal, welche Darstellungsmatrix wir waehlen (also zu welcher
Basis), da Darstellungsmatrizen zu verschiedenen Basen nach Definition von
Aehnlichkeit, auch immer aehnlich sind. Dann haben wir gerade gezeigt, dass
aehnliche Matrizen auch dieselbe Determinante haben.

### Entwicklung

Wir haben bisher zwei Moeglichkeiten zur Berechnung von Matrizen gesehen:

1. Wenn die Dimension der Matrix zwei oder drei ist, nutzen wir die Regel von
   Sarrus.
2. Wenn die Dimension hoeher als drei ist, nutzen wir die fundamentale
   Definition der Determinante als Iteration alle Permutationen.

Es gibt nun aber noch eine dritte Moeglichkeit, welche eine sehr interessante
und einfachere Loesung fuer (2) bietet. Insbesondere sei angemerkt, dass die
explizite Formel aus (2) alle $n!$ Permutationen fuer die $n$ Zeilen/Spalten
betrachet. Die Laufzeit fuer die Berechnung der Determinante waere also
exponentiell, was fuer einen Computer natuerlich an sich schlecht ist. Die
Loesung die wir nun betrachten werden, laesst sich viel schoener und auch
einfacher durch eine rekursive Formulierung beschreiben. Sie laesst sich auch
einfacher auf einen besseren Average Case optimieren. Der Worst Case wird
allerdings exponentiell bleiben.

Die Proposition hierfuer ist die folgende. Sei $A = (A_{i,j}) \in K^{n \times
n}$ eine quadratische Matrix mit $n \geq 2$. Dann sei fuer $i,j \in [n]$ die
Matrix $A_{i,j} \in K^{(n-1)\times(n-1)}$ jene Matrix, die aus $A$ entsteht,
__indem man die $i$-te Zeile und $j$-te Spalte weglaesst__.

Zum Beispiel sei $A$ gegeben durch:

$$
A =
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9 \\
\end{bmatrix}
.$$

Dann waere $A_{1,1}$ also die Matrix, die entsteht, indem man
aus dieser Matrix $A$ die erste Zeile und Spalte entfernt. Die resultierende
Matrix $\in K^{(3 - 1)\times (3 - 1)} = K^{2 \times 2}$ waere dann also:

$$
A_{1,1} =
\begin{bmatrix}
2 & 3 \\
5 & 6 \\
8 & 9 \\
\end{bmatrix}
.$$

Dann gilt:

1. Fuer alle Zeilenindizes $i \in \{1, ..., n\}$:
   $\det(A) = \sum_{j=1}^n (-1)^{i + j} \cdot a_{i,j} \cdot det(A_{i,j})$

2. Fuer alle Spaltenindizes $j \in \{1, ..., n\}$:
   $\det(A) = \sum_{i=1}^n (-1)^{i + j} a_{i,j} \det(A_{i,j})$.

Betrachten wir zunaechst (1): Fuer alle $i \in \{1, ..., n\}$ gilt die folgende
Aussage. Nehmen wir uns also einfach ein konkretes $i$. Dann gilt fuer dieses
$i$, dass wir die Determinante von $A$ berechnen koennen, indem wir ueber alle
*Spaltenindizes* $j \in \{1, ..., n\}$ iterieren. Fuer jeden Spaltenindex $j$
tun wir dann folgendes: Wir suchen uns das "Vorzeichen" fuer diese Iteration,
indem wir $(-1)^{i + j}$ berechnen. Dann multiplizieren wir dieses Vorzeichen
mit dem Eintrag $a_{i,j}$ an der Stelle $(i, j)$ in der Matrix. Schliesslich
__rekursieren__ wir dann noch fuer die Matrix $A_{i,j}$, die entseht, indem man
aus der momentanen Matrix $A$ die $i$-te Zeile und $j$-te Spalte
weglaesst. Dasselbe macht man dann fuer den naechsten Rekursionsschritt
$\det(A_{i,j})$ auch wieder.

Hierbei ist $(-1)^{i + j}$ eben positiv, wenn $i + j$ gerade ist, und sonst
negativ. Insbesondere alterniert das Vorzeichen fuer jeden Spaltenindex
$j$. Fangen wir beispielsweise ganz links oben an, so haben wir am Anfang den
Eintrag bei $(1, 1)$ und somit Vorzeichen $(-1)^{1 + 1} = (-1)^2 = 1$. Danach
alterniert das Vorzeichen fuer jedes $j$, da $i + j$ (mit $i$ eben konstant) als
naechstes ungerade, dann gerade, dann wieder ungerade usw. sein wird. D.h. wir
erhalten dann ein negaives Vorzeichen, dann ein positives, dann wieder ein
negatives usw.

Den Vorgang in (1) nennt man hier auch *Entwicklung nach der $i$-ten Zeile*. Der
Vorgang in (2) wird *Entwicklung nach der $j$-ten Spalte* genannt, und macht
dasselbe wie (1), nur dass es fuer einen festen Spaltenindex $j$ ueber alle
Zeilenindizes $i$ iteriert.

Das mit dem alternierenden Vorzeichen ist natuerlich immer so. Allgemein gilt
naemlich, dass wenn man in der Matrix eine Komponente irgendwohin geht (links,
rechts, oben, unten), sich das Vorzeichen immer aendert. Man geht hierbei immer
rechteckig, also niemals diagonal. $i + j$ ist hier im Uebrigen die *Manhattan
Distance*.

#### Beispiele

##### (1)

Sei die Matrix $A$ gegeben durch:

$$
A =
\begin{bmatrix}
1 & 2 & 3 \\
0 & 0 & 0 \\
4 & 5 & 6 \\
\end{bmatrix}
.$$

Entwickeln wir nach der zweiten Zeile, erhalten wir:

1. $\det(A) = \sum_{j=1}^3 (-1)^{2 + j} a_{2,j} \det(A_{2,j}) $
2. $\det(A) = \sum_{j=1}^3 (-1)^{-2+j} \cdot 0 \cdot \det(A_{2,j})$
3. $\det(A) = 0$

Wir sehen also: Besteht eine Spalte (oder eine Zeile) in einer Matrix nur aus
Nullen, so wird die Determinante immer Null sein.

##### (2)

Sei die Matrix $A$ diesmal gegeben durch:

$$
A =
\begin{bmatrix}
0 & 1 & 2 \\
3 & 4 & 5 \\
6 & 7 & 8
\end{bmatrix}
.$$

Wir sehen, dass in der oberen linken Komponente eine Null steht. Das sagt uns,
dass es eine gute Idee ist, nacher der ersten Spalte oder der ersten Zeile zu
entwicklen, weil die Iteration fuer diesen Spalten- bzw. Zeilenindex immer Null
sein wird ($a_{i,j} = 0$). Wir koennen hier also zur Demonstration einmal nach
der ersten *Spalte* entwickeln. Dann erhalten wir:

1. $\det(A) = 1 \cdot 0 \cdot A_{1,1} - 1 \cdot 3 \cdot A_{2,1} + 1 \cdot 6
   \cdot A_{3,1}$ (wie man sieht alterniert das Vorzeichen schoen)

2. $
    \det(A) =
	- 3 \cdot \det(\begin{bmatrix}1 & 2 \\ 7 & 8\end{bmatrix})
	+ 6 \cdot \det(\begin{bmatrix}1 & 2 \\ 4 & 5\end{bmatrix})
   $

3. $\det(A) = -3 \cdot (-6) + 6 \cdot (-3) = 0$

Also ist die Matrix $A$ nicht regulaer (die dritte Spalte ist zwei mal die
zweite minus der ersten).

##### (3)

Sei $A = \diag(a_1, ..., a_n)$ eine Diagonalmatrix, welche also nur auf der
Diagonalen die Werte $a_1, ..., a_n$ besitzt, und sonst in allen Komponenten nur
Nullen. Ein Beispiel fuer eine solche Matrix waere waere $\diag(6, 9)$:

$$
\diag(6, 9) =
\begin{bmatrix}
6 & 0 \\
0 & 9 \\
\end{bmatrix}
$$.

Die Determinante dieser Matrix ist gerade dem Produkt der Diagonaleintraege:

$$\det(\diag(a_1, ..., a_n)) = \Pi_{i=1}^n a_i.$$

Wir entwicklen naemlich einfach immer nach der ersten Spalte oder Zeile. Dann
sind alle Eintraege fuer eine Iteration immer Null, ausser dem ersten (in der
Ecke). Dann haben wir also z.B.

1. $\det(A) = a_{1,1} \cdot \det(A_{1,1})$
2. $\det(A) = a_{1,1} \cdot a_{2,2} \cdot \det(A_{{1,1}_{1,1}})$
3. ...

Da die Eintraege auf der Diagonalen liegen, ist das Vorzeichen immer positiv
($(-1)^{i + i} = (-1)^{2i}$ sodass die Potenz immer gerade ist).

##### (4)

Sei $A$ die *obere Dreiecksmatrix*, welche wie die Diagonalmatrix $\diag(a_1,
..., a_n)$ Werte auf den Diagonalen, aber zusaetzlich rechts von der Diagonalen
auch noch Komponenten ungleich Null hat (also in der "rechten oberen Ecke"). Ein
Beispiel waere:

$$
A =
\begin{bmatrix}
6 & 1 \\
0 & 9 \\
\end{bmatrix}
$$

(vgl. oben).

Die Determinante ist hier aber gleich wie bei der Diagonalmatrix, also $\det(A)
= \Pi_{i=1}^n a_i$. Wir koennen naemlich einfach wieder immer nach der ersten
*Spalte* entwickeln, dann haben wir nach unten hin auch immer Nullen (bei der
Diagonalmatrix war uns die Wahl zwischen Spalte und Zeile noch frei). Fuer die
*untere Dreiecksmatrix*, welche also nur Werte auf und links von der Diagonalen
hat, gilt dasselbe, wobei wir hier dann aber immer nach der ersten *Zeile*
entwickeln (sodass nach rechts hin immer nur Nullen stehen).

##### (5)

Sei $A$ eine Matrix in *Block-Dreiecksgestalt*. Eine solche Matrix $A \in K^{n
\times n}$ ist dadurch charakterisiert, dass sie aus vier disjunkten,
quadratischen *Teilbloecken* $B, C, D, 0$ besteht. So kann man die Matrix
schematisch als

$$
\begin{bmatrix}
B & 0 \\
C & D \\
\end{bmatrix}
$$

darstellen. Diese Teilbloecke sind einfach Indexintervalle, wobei sie aber
folgende Eigenschaften haben:

* $B$ ist der linke obere Block $\in K^{l \times l}$ mit $l < n$.

* $0$ ist der rechte obere Block $\in K^{l \times (n - l)}$ und besteht nur
  aus Nullen.

* $C$ ist der linke untere Block $\in K^{(n - l) \times l}$.

* $D$ ist der rechte untere Bloeck $\in K^{(n - l) \times (n - l)}$.

Wir zeigen, dass dann gilt:

$$\det(A) = \det(B) \cdot \det(D).$$

Wir koennen die Determinante dann also einfach auf ein Produkt der Determinanten
dieser beiden Teilbloecke reduzieren. Hierfuer betrachten wir vor allem die
urspruengliche Definition der Determinante ueber die Permutionen, also die
Formel

$$\det(A) = \sum_{\sigma \in S_n} \sgn(\sigma) \cdot \Pi_{i=1}^n a_{i,\sigma(i)}.$$

Nun ueberlegen wir uns folgendes: Wir haben in den Zeilen die Indizes $i = 1,
..., n$ (man stelle sich die $i$ links neben den Zeilen vor) und in den Spalten
die Indizes $j = \sigma(1), ..., \sigma(n)$ (man stelle sich die $j$ ueber den
Spalten vor). Eine Permutation $\sigma$ wuerde in diesem Fall also die $j$
permutieren. Nun denken wir ein wenig um und geben die ersten $l$ Spaltenindizes
konzeptuell links oben auf die Matrix und die anderen $n - l$ rechts unter die
Matrix, z.B.:

```
   1 2
   --------
1 |B B 0 0
2 |B B 0 0
3 |C C D D
4 |C C D D
   --------
	   3 4
```

Das soll also andeuten, wohin die jeweiligen Zeilenindizes abgebildet
werden. Hierbei deuten wir auch an, dass die oberen Zeilen nur durch die oberen
Permutationen und die unteren Zeilen nur durch die unteren Permutationen
abgebildet werden. Auch sei angemerkt, dass die Permutation unten in der
$x$-letzten Stelle natuerlich auch die $n - x$-te Zeile anspricht. Nun
betrachten wir zwei Faelle:

###### 1. Fall

Die Permutationen gehen oben genau bis $l$, also zum Ende von Block $B$, und
unten ab $l$ bis $n$, also ab Block $D$. Dann haben wir also nur die
Komponentenwerte aus den Bloecken $B$ und $D$ in dem Produkt der
Permutationsformel, und alles is OK.

###### 2. Fall

Nun wollen wir unten aber eine der Zeilen auf eine Komponente im $C$ Block
abbilden. Man kann sich das z.B. so vorstellen, dass wir die unteren
Permutationen nach links schieben. Wir haetten jetzt erstmal eine Ueberlappung
zwischen den oberen und unteren Indizes. Oben waere ein Index naemlich fuer
Block $B$ bestimmt und unten fuer Block $C$:

```
   1 2
   --------
1 |B B 0 0
2 |B B 0 0
3 |C C D D
4 |C C D D
   --------
	 4 3
```

Da eine Permutation injektiv sein muss, ist das keine gueltige Konfiguration. Um
die Ueberlappung zu entfernen, bleibt uns nur eine einzige Wahl: Wir muessen den
ueberlappenden Index oben auf einen Index ab $l$ im $0$ Block abbilden:

```
   1     2
   --------
1 |B B 0 0
2 |B B 0 0
3 |C C D D
4 |C C D D
   --------
	 4 3
```

Das Wichtige hierbei ist: sobald wir einen Eintrag aus $C$ wollen, muessen wir,
um die Permutation wieder zu "verteilen", auch einen Eintrag aus dem $0$ Block
waehlen. Fuer die Determinantenformel heisst das nun aber, dass eines der
$a_{i,\sigma(i)}$ aus dem Nullblock kommt, also Null ist. Somit waere dann also
das gesamte Produkt $\Pi_{i=1}^n a_{i,\sigma(i)}$ *gleich Null*.

Das sagt uns nun also, dass wir nur dann nicht-Null $a_{i,\sigma(i)}$ Werte
erhalten, wenn die Spaltenindizes bzw. Permutationbilder $\sigma(i)$ oben immer
im $B$ Block sind und unten immer im $D$ Block. Die ganze Determinante errechnet
sich dann also nur aus allen moeglichen Permutationen in diesen zwei
Bloecken. Alle moeglichen Permutationen (mit Signum und Produkt) ist gerade das,
was fuer den $B$ Block durch $\det(B)$ und fuer den $D$ Block durch $\det(D)$
berechnet wird. Anders gedacht bleiben einfach nur diese Permutationen in der
Summe ueber alle Permutationen $\sigma \in S_n$ erhalten (da ungleich Null).

Somit ergibt sich die ganze Determinante $\det(A)$ der Matrix in
Block-Dreiecksgestalt aus dem Produkt dieser beiden Determinanten $\det(B)$ und
$\det(A)$.  Wir multiplizieren diese beiden Determinanten, weil man zur
Rekonstruktion einer solchen Produktserie $\Pi_{i=1}^n$ die *Teilprodukte* aus
$\det(A)$ und $\det(B)$ nimmt, und durch Multiplikation sozusagen konkateniert
(denke an die Identitaetspermutation, dann haben wir links oben zuerst die obere
Haelfte der Diagonale und rechts unten dann passend die untere Haelfte der
Diagonalen). Wir spalten die Produktserie $\Pi_{i=1}^n a_{i,\sigma(i)}$ ja
eigentlich auf die beiden anderen Determinanten auf, sodass wir konzeptuell
sowas haben:

$$
\Pi_{i=1}^n a_{i,\sigma(i)} =
\Pi_{i=1}^{n/2} a_{i,\sigma(i)} \cdot \Pi_{i=n/1 + 1}^n a_{i,\sigma(i)}
.$$

##### (6)

Sei $A \in K^{n \times n}$ eine *antisymmetrische* Matrix. Eine solche Matrix
ist fast symmetrisch, aber mit dem Unterschied, dass die Eintrage links und
rechts von der Hauptdiagonalen verschiedene Vorzeichen haben. Ein Beispiel
hierfuer waere die folgende Matrix:

$$
A =
\begin{bmatrix}
 0 &  4 & 5
-4 &  0 & 3
-5 & -3 & 5
\end{bmatrix}
$$

Eine solche Matrix hat die Eigenschaft, dass sie transponiert gleich dem
negativen ist. Sei $A$ also eine antisymmetrische Matrix, dann gilt:

$$A^\top = -A$$

Wir koennen nun in einem bestimmten Fall die Determinante einer solchen
antisymmetrischen Matrix bestimmen, ohne ueberhaupt die Werte der Matrix
anzusehen. Hierzu:

1. Wir wissen: $\det(A) = \det(A^\top)$
2. Dann gilt fuer antisymemtrische Matrizen also: $\det(A) = \det(A^\top) =
   \det(-A)$
3. Nun koennen wir den Faktor $n$ linear rausizehen, und erhalten:
   $\det(A) = \det(-A) = (-1)^n\det(A)$

Sei nun $n$ gerade. Dann ist $\det(A) = \det(A)$ und wir haben nichts
gewonnen. Sei aber $n$ ungerade, dann haben wir:

$$\det(A) = -\det(A).$$

Eine solche Eigenschaft kann nur genau dann gelten, wenn die Determinante Null
ist! Es gilt also: antisymmetrische Matrizen mit ungerader Dimension haben Null
als Determinante.

Quelle: https://www.youtube.com/watch?v=aKX5_DucNq8

### Adjunkte

Sei

$$A = (a_{i,j}) \in K^{n \times n}$$

eine quadratische Matrix und

$$C = (c_{i,j}) \in K^{n \times n}$$

auch. Wenn gilt:

$$c_{i,j} = (-1)^{i + j} \det(A_{j,i}) \forall (i,j),$$

dann nennt man $C$ die *Adjunkte* bzw. *adjunkte Matrix* von $A$. Die Matrix $C$
geht also aus $A$ hervor, indem man fuer Index $i,j$ die Matrix $A$ nimmt, und
die die $j$-te Zeile und $i$-te Spalte entfernt (andersrum als bei der
Entwicklung). Fuer die Adjunkte einer Matrix $A$ gilt die folgende Eigenschaft:

$$A \cdot C = C \cdot A = \det(A).$$

Zum Einen ist $A$ ist im Produkt mit $C$ also kommutativ, und zum Anderen ist
dieses Produkt gerade gleich der Determinante von $A$. Dann gilt insbesondere
weiter:

1. $AC = \det(A)$
2. $(AC)^{-1} = \det(A)^{-1}$
3. $C^{-1} \cdot A^{-1} = \frac{1}{\det(A)}$
4. $A^{-1} = \frac{C}{\det(A)}$

Die Inverse einer Matrix $A$ ist also gleich der Adjunkten der Matrix, durch
ihre Determinante. Das gibt uns nun eine weitere Moeglichkeit, die Inverse einer
Matrix zu bestimmen. Diese Methode ist allerdings nicht effizienter als die, die
den Gauss Algorithmus nutzt.

Fuer $2 \times 2$ Matrizen ist diese Eigenschaft aber interessant und leicht
merkbar, dann gilt naemlich:

$$
\begin{bmatrix}
a & b \\
c & d \\
\end{bmatrix}^{-1}

=

\frac{1}{\det(A)}

\cdot

\begin{bmatrix}
 d & -b \\
-c &  a \\
\end{bmatrix}
$$

Somit koennen wir fuer $2 \times 2$ Matrizen relativ schnell die Inverse
finden. Die Adjunkte ergibt sich hierbei naemlich einfach daraus, dass man die
Elemente auf der ersten Hauptdiagionalen vertauscht, und das Vorzeichen jener
auf der zweiten Hauptdiagionalen wechselt.

Vor Allem aber koennen wir __mit einem Blick auf die Determinante__ der Matrix
__erkennen, ob eine Matrix eine Inverse hat__. Naemlich dann, wenn gilt:

$$\det(A) = ad - bc \neq 0.$$

### Gauss Algorithmus fuer Determinanten

Wir wollen uns nun ansehen, wie elementare Zeilenoperationen, wie sie im Gauss
Algorithmus verwendet werden, die Determinte einer Matrix jeweils
aendern. Hierfuer betrachten wir die drei Typen von Operationen im Einzelnen:

#### Typ I: Vertauschen von Zeilen

Dann *aendert die sich das Vorzeichen der Determinante*.

Das wissen wir schon, da wir vorher festgelegt haben, dass wenn eine Matrix $B$
aus einer anderen $A$ nur durch Anwendung einer Permutation auf den Zeilen (oder
Spalten) hervorgeht, sich die Determinanten genau um das Signum der Permutation
unterscheiden. Da das Vertauschen zweier Zeilen einer Transposition entspricht,
welches immmer negatives Signum hat, multiplizieren wir die Determinante der
urspruenglichen Matrix (vor dem Vertauschen der Zeilen) immer mit $-1$, wodurch
sich das Vorzeichen entsprechend aendert.

#### Typ II: Multiplikation einer Zeile mit einem Skalar $s$.

Dann wird Determinante wird mit $s$ multipliziert.

Stellen wir uns hierfuer eine Zeile vor, und entwickeln nach ihr. Dann erhalten
wir also

$$
\det(A)
= (-1)^{i + 1} \cdot a_{i,j} \cdot det(A_{i,j})
+ ...
+ (-1)^{i + n} \cdot a_{i,n} \cdot \det(A_{i,n})
.$$

Wenn wir nun diese Zeile mit $s$ multiplizieren und erneut nach ihr entwickeln,
kriegen wir:

$$
 (-1)^{i + 1} \cdot s \cdot a_{i,j} \cdot det(A_{i,j})
+ ...
+ (-1)^{i + n} \cdot s \cdot a_{i,n} \cdot \det(A_{i,n})
.$$

Dann koennen wir das $s$ also auch einfach herausheben, und erhalten eben $s$
mal das vorherige:

$$
s \cdot
(
 (-1)^{i + 1} \cdot a_{i,j} \cdot det(A_{i,j})
+ ...
+ (-1)^{i + n} \cdot a_{i,n} \cdot \det(A_{i,n})
)
=
s \cdot \det(A)
.$$

Wir erhalten also gerade $s$ mal die vorherige Determinante. Das gilt nun also
fuer eine einzige Zeile. Wenn wir nun einen Faktor aus einer *Matrix* rausziehen
wollen, dann ist es ein wenig anders. Fuer eine $n \times n$ Matrix haben wir
dann nicht den Faktor $s$, sondern $s^n$. Denn wir ziehen ihn ja aus jeder der
*n* Zeilen einmal als multiplikativen Faktor heraus. Es gilt also:

$$\det(sA) = s^n \cdot \det(A)$$

#### Typ III: Addition des $s$-fachen einer Zeile zu einer anderen.

Dann bleibt die Determinante gleich.

Sei hierzu $E_{i,j} \in K^{n \times n}$ ueberall $0$, ausser im Eintrag $(i,
j)$, wo eine $1$ steht (quasi Standardmatrix). Die Addition des $s$-fachen einer
Zeile auf eine andere entspricht hierbei gerade

$$(I_n + s \cdot E_{i,j}) A.$$

Hierzu betrachten wir als Beispiel eine $3 \times 3$ Matrix. Wir wollen nun die
*erste Zeile fuenf mal auf die dritte* Zeile addieren. Dann generieren wir zuerst
die Matrix $5 \cdot E_{3,1}$:

$$
\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & 0 \\
5 & 0 & 0 \\
\end{bmatrix}
.$$

Jetzt addieren wir diese Matrix auf die Einheitsmatrix $I_3$ und erhalten die
Matrix

$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
5 & 0 & 1 \\
\end{bmatrix}
.$$

Nun multiplizieren wir diese Matrix mit $A$. Was passiert?  Haetten wir nur die
Einheitsmatrix, wuerde wieder $A$ rauskommen. Jetzt haben wir aber im Eintrag
$(3, 1)$ eine $5$ stehen. In der Multiplikation $(I_3 \cdot 5 \cdot E_{3,1})$
betrachten wir diese Zeile mit der $5$ gerade als Letztes. Normalerweise wuerden
wir dann die Zeile $(0, 0, 1)$ mit allen Spalten aus $A$ multiplizieren, und
somit gerade immer die unteren Komponenten aus $A$ rauspicken. Dadurch erhalten
wir die letzte Zeile eben (so die Semantik der Einheitsmatrix). Jetzt haben wir
aber $(5, 0, 1)$ in der letzten Zeile. D.h. nun, dass wenn wir diese Zeile mit
jeder Spalte in $A$ multiplizieren, wir zum ein-fachen der letzten Zeile immer
noch das fuenf-fache der ersten Zeile dazuaddieren. Das ist gerade was wir
wollten!

Nun haben wir also den Ausdruck $(I_n + s E_{i,j})A$ fuer die resultierende
Matrix. Fuer ihre Determinante gilt dann

1. $\det((I_n + s E_{i,j}) \cdot A)$ (Multiplikativitaet der Determinante)
2. $\det(I_n + s E_{i,j}) \cdot \det(A)$

Sehen wir uns also an, was $\det(I_n + s E_{i,j})$ ist. Wir geben immer genau
einen Eintrag $(i,j)$ zur Einheitsmatrix hinzu. Dieser Eintrag kann unter oder
ueber der Diagonalen (mit den Einsern der Einheitsmatrix) liegen. Sie kann nicht
auf der Diagonalen liegen, weil wir hier von der Addition eines Vielfachen einer
Zeile *auf eine andere Zeile* sprechen. __Wir haben also gerade entweder eine
obere oder eine untere Dreiecksmatrix__!

Fuer eine solche Dreiecksmatrix wissen wir, dass ihre Determinante eben gleich
dem Produkt der Diagonaleintraege ist. Da diese Eintraege bei der Einheitsmatrix
alle Eins sind, haben wir also

$$\det(I_n + s \cdot E_{i,j}) = 1.$$

Somit haben wir also insgesamt:

1. $\det((I_n + s E_{i,j}) \cdot A)$
2. $\det(I_n + s E_{i,j}) \det(A) $
3. $1 \cdot \det(A)$
4. $\det(A)$.

Daraus folgt die Aussage.

#### Hintergrund

Diese Informationen darueber, wie sich die Determinante bei den verschiedenen
elementaren Zeilenoperationen veraendert, kann man jetzt dafuer verwenden, eine
Matrix zu vereinfachen, um leichter die Determinante zu
berechnen. Beispielsweise koennten wir eine Zeile auf eine andere addieren, um
somit eine Zeile mit nur einem einzigen $1$ Eintrag zu erhalten. Dann wuerde
sich also zum Einen die Determinante der ganzen Matrix nicht aendern. Zum
Anderen haetten wir aber danach auch weniger Arbeit fuer den Rest der Matrix,
wenn wir nach dieser Zeile mit nur Nullen und einer Eins entwickeln.

Diese ganzen Eigenschaften gelten uebrigens auch fuer Spaltenoperationen, da ja
gilt: $\det(A) = \det(A^T)$.

## Anwendungen der Determinante

Seien $v_1, v_2 \in \mathbb{R}^2$ so gewaehlt, dass sie zusammen eine Ebene (ein
Parallelogram) aufspannen. Dann ist der Betrag der Determinante $|\det(v_1,
v_2)|$ der Flaechenihalt des Parallelograms. Aehnliches gilt auch fuer hoehere
Dimensionen. Die Determinante wird also allgemein fuer Volumenberechnung
polyedrischer Objekte genutzt.

Auch die Berechnung von *Eigenwerten*, wichtig fuer die Datenanalyse,
funktioniert ueber der Determinante.

In der Graphentheorie hat es auch Anwendungen. Die Determinante der
Adjazenzmatrix des vollstaendigen Graphen $K_n$ auf $n$ Knoten ist naemlich
gleich $\det(K_n) = (-1)^{n-1}(n - 1)$. Diese Informationen kann man benutzen,
um vollstaendige Subgraphen, zu entdecken. Wenn ein Teilgraph $S$ naemlich
z.B. isomorph zum $K_3$ ist, so muesste seine Determinante $(-1)^{3 - 1} \cdot
(3 - 1) = 2$. Das ist vor Allem dann nuetzlich, wenn man bei sehr grossen
Matrizen nicht einfach in die Adjanzenzmatrix blicken kann.

Ein aktuelles Forschungsgebiet ist die *Spektrale Graphentheorie*. Hier wird die
Determinante auf fuer kompliziertere Fragestellungen benutzt, z.B. zum Zaehlen
der Zusammenhangskomponenten odr zu Betimmung *dichter* Subgraphen.

## Zusammenfassung

Hier eine Zusammenfassung der Eigenschaften, die wir in diesem Kapitel gelernt haben.

### Permutationen

1. $\sgn(\sigma\tau) = \sgn(\sigma) \cdot \sgn(\tau)$
2. $\sgn(\sigma) \cdot \sgn(\sigma) = 1 \Rightarrow \sgn(\sigma) = \sgn(\tau)$
3. $\sgn(\sigma) = \sgn(\sigma^{-1}) = \sgn(\sigma)^{-1}$

### Determinante

1. $\det(A \cdot B) = \det(A) \cdot \det(B)$
2. $\det(A^{-1}) = \det(A)^{-1}$
3. $\det(A) = \det(A^\top)$
4. $\det(B) = \sgn(\sigma) \cdot \det(A)$, wenn $B$ durch $\sigma$ aus $A$
   hervorgeht.
5. Sind zwei Spalten oder Zeilen einer Matrix gleich, ist ihre Determinante $0$.
6. $A \text{ ist regulaer } \iff \det(0) \neq 0$
7. Sind $A$ und $B$ aehnlich, gilt $\det(A) = \det(B)$.
8. Sei $A = \diag(a_1, ..., a_n)$ dann gilt $\det(A) = \Pi_i a_i$
9. Sei $A$ eine obere oder untere Driecksmatrix mit Diagonale $\diag(a_1, ...,
   a_n)$ dann gilt $\det(A) = \Pi_i a_i$.
10. Sei $A$ in Block-Dreiecksgestalt mit $\begin{bmatrix}B & 0 \\ C &
    D\end{bmatrix}$. Dann gilt $\det(A) = \det(B) \cdot \det(D)$
11. Sei $A$ antisymmetrisch mit ungerader Dimension, dann ist $\det(A) = 0$.
12. Sei $A \in K^{2 \times 2}$, dann gilt $A^{-1} = \det(A)^{-1} \cdot \adj(A)$
13. Gauss Operationen:
	1. Vertauschen einer Zeile: Vorzeichen veraendert sich.
	2. Skalieren einer Zeile mit $s$: Determinante wird um $s$ skaliert.
	3. Skalieren der Matrix um $s$: Determinante wird um $s^n$ skaliert.
	4. Addieren des $s$-fachen einer Zeile auf eine andere: Determinante bleibt
       gleich.
14. Alles aus (13) auch fuer Spalten.
15. $\det(f) = \det(D_B(f))$ fuer beliebige Darstellungsmatrix $D_B(f)$.

### Aequivalenzen

Sei $A$ quadratisch, dann sind aequivalent:

* $A$ ist regulaer
* $A$ ist invertierbar
* Die Zeilen von $A$ sind linear unabhaengig
* Die Spalten von $A$ sind linear unabhaengig
* $f_A$ ist injektiv
* $f_A$ ist surjektiv
* $f_A$ ist bijektiv
* $Ax = b$ ist eindeutig loesbar
* $\kern(f_A) = \{0\}$
* $\det(A) \neq 0$
