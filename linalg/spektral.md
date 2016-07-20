# Spektrale Graphentheorie

$\newcommand{\det}{\mathop{\rm det}\nolimits}$
$\newcommand{\dim}{\mathop{\rm dim}\nolimits}$

Hier wollen wir nun eine weitere Anwendung der linearen Algebra, die sogenante
*spektrale Graphentheorie* beleuchten. Die spektrale Graphentheorie untersuch
Graphen nach ihren *Spektren* (Definition unten) um zu erkennen, ob Graphen
isomorph sind oder um die Zusammenhangskomponenten von Graphen zu finden.

## Graphenmodell

Fuer die spektrale Graphentheorie wollen wir nun folgendes Modell von Graphen
benutzen: Wir betrachten Graphen $G = (V, E)$ definiert durch eine endliche,
nichtleere Menge von Knoten $V$ sowie dazugehoerigen Kanten $E$. $E$ ist hierbei
stets eine Untermenge der Menge aller moeglichen Kanten, gegeben durch:

$$E \subseteq \{\{u, v\} \,|\, u,v \in V: u \neq v\}$$

Da wir hier $u \neq v$ fordern, wollen wir also keine Schleifen in unseren
Graphen. Mit Kanten $\{u, v\}$ als Menge deuten wir an, dass der Graph
*ungerichtet* ist. Gibt es also eine Kante von $u$ nach $v$, so verbindet diese
Kante auch den Knoten $v$ mit Knoten $u$. Insgesamt kann es also maximal $n
\cdot (n - 1) = n^2 - n$ Kanten geben. Einen Graphen, der alle diese moeglichen
$n^2 - n$ Kanten besitzt, nennen wir dann *vollstaendig*.

Seien $G = (V, E)$ und $G' = (V', E')$ zwei Graphen. Dann nennt man sie
*isomorph*, wenn es eine Bijektion zwischen den Knoten gibt, sodass nach dieser
"Umbennenung" dieselben Knoten noch immer mit denselben (umbenannten) Knoten
verbunden sind. D.h. es gibt eine bijektive Abbildung $f: V \rightarrow V'$,
sodass gilt:

$$\{\{f(u), f(v)\} \,|\, \{u, v\} \in E\} = E'$$

D.h. wenn es eine Kante in $E$ gibt, die zwei Knoten $u,v$ aus $V$ verbindet,
dann muss es in einem isomorphen graphen eine Kante geben, die diese Knoten nach
der Umbennenung auch noch verbindet.

Letztlich wollen wir noch den Begriff einer *Adjazenzmatrix* definieren. Sei
hierzu $G = (V, E)$ ein Graph mit Knotenmenge $V = \{1, ..., n\}$. Dann nennt
man die Matrix $A = (g_{i,j}) \in \mathbb{R}^{n \times n}$ definiert durch

$$
g_{i,j} =
\begin{cases}
	1, \text{ falls } \{i,j\} \in E\\
	0, \text{ sonst}
\end{cases}
$$

die *Adjazenzmatrix* von $G$. Der Eintrag in Position $(i,j)$ der Adjazenzmatrix
sagt also aus, ob Knoten $i$ mit Knoten $j$ verbunden ist. Da der Graph
ungerichtet ist, ist diese Matrix dann auch symmetrisch, sodass $A = A^\top$ und
$a_{i,j} = a_{j,i}$ gilt. Ein Graph ist durch seine Adjazenzmatrix eindeutig
definiert.

## Spektrum

Die Menge der Eigenwerte der Adjazenzmatrix $A$ eines Graphen $G = (V, E)$ nennt
man nun das *Spektrum* eines Graphen. Hierbei betrachten wir das Spektrum als
Menge von Eigenwerten, wobei wir fuer die (algebraischen) Vielfachheiten jeden
Eigenwertes noch zusaetzliche Kopien in die Menge geben. Da Adjazenzmatrizen
ungerichter Graphen (wir betrachten nur solche) symmetrisch sind, wissen wir,
dass $A$ in relle Eigenwerte zerfaellt. Da es vollstaendig zerfaellt, gibt es
gerade $n$ solcher reller Eigenwerte, die aber eben nicht paarweise verschieden
sein muessen. Somit koennen wir das Spektrum eines Graphen als eine Menge von
genau $n$ rellen Zahlen angeben, wobei wir diese Eigenwerte noch nach ihrer
Groesse aufsteigend sortieren.

Betrachten wir beispielsweise folgenden Graphen:

```
 1 ----- 2
 |       |
 |       |
 |       |
 4 ----- 3
```

Dieser Graph $G = (V, E)$ mit $V = \{1, 2, 3, 4\}$ hat folgende Adjazenzmatrix:

$$
\begin{bmatrix}
0 & 1 & 0 & 1\\
1 & 0 & 1 & 0\\
0 & 1 & 0 & 1\\
1 & 0 & 1 & 0\\
\end{bmatrix}
$$

Das charakteristische Polynom $\chi_A = \det(xI_n - A)$ ergibt sich dann als:

$$
\chi_A = \det(
\begin{bmatrix}
x & -1 & 0 & -1 \\
-1 & x & -1 & 0 \\
0 & -1 & x & -1 \\
-1 & 0 & -1 & x \\
\end{bmatrix}
)
$$

Als Faktorisierung erhalten wir dann $x^2(x + 2)(x - 2)$. Somit sind die
Eigenwerte $\{0, -2, 2\}$ wobei der Eigenwert $0$ algebraische Vielfachheit $2$
hat. Das Spektrum des Graphen waere also $-2, 0, 0, 2$.

## Isomorphe Graphen

Wir koennen nun schon eine erste Tatsache bezueglich Graphen anhand der uns
bekannten Regeln aus der linearen Algebra zeigen. Naemlich:

__Die Spektren isomorpher Graphen stimmen ueberein.__

Seien hierzu $A = (g_{i,j}), A' = (g'_{i,j}) \in \mathbb{R}^{n \times n}$ die
Adjanzenzmatrizen zweier isomorpher Graphen $G$ und $G'$. Ohne Beschraenkung der
Allgemeinheit benutzen $G$ und $G'$ die Knotenmenge $\{1, ..., n\}$
(ueberladen). Dann wuerde Isomorphie hier bedeuten, dass es eine Permutation
$\sigma \in S_n$ gibt, sodass gilt:

$$g'_{i,j} = g_{\sigma(i), \sigma(j)}$$

Die Adjazenzmatrix $A'$ geht also aus Permutation der Spalten und Zeilen von $A$
hervor. Hierbei werden Spalten und Zeilen aber gleichzeitig bezueglich $\sigma$
permutiert, also nicht unabhaengig. Wenn wir Knoten $a$ mit Knoten $b$
vertauschen, so vertauschen wir Zeile $a$ mit Zeile $b$ ("ausgehende" Kanten)
und Spalte $a$ mit Spalte $b$ ("eingehende" Kanten).

Da wir in unseren Graphen keine Schleifen erlauben, hat die Adjazenzmatrix $A$
sowie $A'$ in der Diagonalen nur Nullen. Das heisst insbesondere, dass es keinen
zu $G$ isomorphen Graphen geben kann, der ploetzlich eine Schleife hat. Somit
ist $G'$ mit Adjanzenzmatrix $A'$ also auch Schleifenfrei. Wir wissen, dass das
charakteristische Polynom $\chi_A$ durch die Determinante $\det(xI_n - A)$
hervorgeht. das einzig bemerkenswerte an $xI_n - A$, ist dass wir in der
Diagonalen von $x$ abziehen. Ansonsten bleiben die Eintrage von $A$ bis auf ihr
Vorzeichen unveraendert. Das bedeutet, dass sich die Determinante von $A$ bei
einer Permutation nur dann aendern wuerde, wenn die Diagonaleintraege veraendert
wuerden. Denn ansonsten gibt es ja fuer jede Permutation (im Sinne der
Definition der Determinante) in der urspruenglichen Matrix $A$ auch eine
passende Permutation in der permutierten Matrix $A$, die jeweils die umbenannte
Zeile auf die umbenannte Spalte abbildet.

Da also die Diagonaleintraege von $A$ durch Permutation in $A'$ nicht veraendert
werden, bedeutet das, dass $A$ und $A'$ dieselbe Determinante und somit dasselbe
charakteristische Polynom haben. Somit sind auch die Eigenwerte von $A$
bzw. $A'$ gleich. Da die Eigenwerte das Spektrum bilden, gilt also sicher, dass
isomorphe Graphen gleiche Spektren haben.

Das interessante ist nun, dass das Spektrum eines Graphen also eine
*Graphinvariante* ist, wohingegen das fuer die Adjazenzmatrix eines Graphen
nicht gilt. D.h. das auch wenn die exakte Adjazenzmatrix eines Graphen durch
eine Isomorphie nicht erhalten bleibt (wegen Umbenennung), das Spektrum jedoch
schon gleich bleibt.

Hierbei ist es aber sehr wichtig anzumerken, dass diese Aussage nur in die eine
Richtung gilt!

Es gilt: Sind zwei Graphen isomorph sind, so sind ihre Spektren ident.
Es gilt __nicht__: Sind die Spektren zweier Graphen ident, so sind die Graphen
                   isomorph.

Wir wissen aber schon, dass wenn die Spektren zweier Graphen nicht ident sind,
die Graphen auch nicht isomorph sein koennen ($(A \Rightarrow B) \Rightarrow
(\neg B \Rightarrow \neg A)$). Solche Graphen, deren Spektrum identisch ist,
nennt man *isospektral*.

Es gilt also: $\text{isomorph } \Rightarrow \text{ isospektral}$
Es gilt __nicht__: $\text{isospektral } \Rightarrow \text{ isomorph}$

## Laplace Matrizen

Sie nun $G = (V, E)$ ein Graph mit Adjazenzmatrix $A = (g_{i,j}) \in
\mathbb{R}^{n \times n}$. Fuer $i = 1, ..., n$ sei dann $d_i = \sum_{i=1}^n
g_{i,j}$ die Anzahl der vom $i$-ten Knoten ausgehenden Kanten. Diese Anzahl
$d_i$ nennen wir den *Grad* des $i$-ten Knoten.

Wir bilden nun die Matrix $L = (l_{i,j}) \in \mathbb{R}^{n \times n}$ gegeben
durch:

$$
l_{i,j} =
\begin{cases}
d_i, \text{ fuer } i = j
-g_{i,j}, sonst
\end{cases}
.
$$

Diese Matrix nennt man *Laplace-Matrix* von $G$. Sie hat in der Diagonalen, also
Eintrag $l_{i,i}$ mit $i \in [n]$, jeweils den Grad des Knoten $i$ und ueberall
sonst die selben Eintrage wie die Adjazenzmatrix von $G$, aber mit negativem
Vorzeichen. Da die Adjazenzmatrix nur Einsen enthaelt, enthaelt $G$ also fuer
jede Eins in der Adjazenzmatrix eine $-1$ in der Laplace-Matrix. Das Spektrum
dieser Matrix, also die Menge ihrer Eigenwerte, nennt man *Laplace-Spektrum* von
$G$.

Eine erste Beobachtung, die wir schon feststellen koennen, ist das nicht nur
Spektren isomorpher Graphen ident sind, sondern auch Laplace-Spektren. Somit ist
also auch das Laplace-Spektrum eines Graphen eine Invariante. Wiederum gilt
aber, dass Gleichheit des Laplace-Spektrums zweier Graphen nicht deren
Isomorphie impliziert. Es gibt sogar Graphen, wo Spektrum *und* Laplace-Spektrum
gleich sind, die aber nicht isomorph sind. Eine offene Forschungsfrage ist im
Uebrigen, ob es eine Familie von Matrizen gibt, sodass man sicher sein kann,
dass ihre Spektren verschieden sind (denn auch wenn Matrizen nicht isomorph
sind, weiss man ja nicht, ob die Spektren verschieden oder gleich sind).

### Satz

Einen ersten Satz, den wir nun beweisen wollen, ist, dass *jede Laplace-Matrix
positiv semidefinit* ist. Das heisst also, dass das Laplace-Spektrum jedes
Graphen, welches ja die Eigenwerte enthaelt, nur nicht-negative Zahlen
enthaelt.

#### Beweis

Sei hierzu $A = (g_{i,j}) \in \mathbb{R}^{n \times n}$ die Adjazenzmatrix zu
einem Graphen $G$ und $L$ die entsprechende Laplace-Matrix. Fuer einen
beliebigen Vektor $v = (x_1, ..., x_n)^\top \in \mathbb{R}^n$ muessen wir nun
zeigen, dass $\langle v, Lv \rangle \geq 0$ ist. Also:

1. $\langle v, Lv \rangle$ (Bilinearitaet)
2. $\sum_{i=1}^n \sum_{j=1}^n x_i (l_{i,j} x_j)$ (Multiplizieren Komponente $i$
   vom linken Vektor $v$ mit der Summe, die Komponente $i$ in $Lv$ ergibt,
   naemlich das Skalarprodukt der $i$-ten Zeile von $L$ mit ganz $v$).
3. $\sum_{i=1}^n d_i x_ix_i - \sum_{i=1}^n\sum_{j=1, j \neq i}^n g_{i,j} x_ix_j$
   (Spalten die Summe ueber alle Komponenten auf in die Summe der Diagonalen,
   minus der Summe ueber alle anderen Eintraege; minus wegen der
   Laplace-Eigenschaft)
4. $\sum_{i=1}^n\sum_{j=1,j\neq i}^n g_{i,j} x_i^2 - \sum_{i=1}^n \sum_{j=1}^n
   g_{i,j} x_i x_j$ (der Grad $d_i$ ist gleich der Summe ueber die $i$-te Zeile)
5. $\mathop{\sum^n\sum^n}_{1 \leq i < j \leq n} g_{i,j}(x_i^2 + x_j^2 -
   2x_ix_j)$ (merkwuerdige Umformung)
6. $\mathop{\sum^n\sum^n}_{1 \leq i < j \leq n} g_{i,j} (x_i - x_j)^2$

Da wir hier nun $(x_i - x_j)^2$ haben, ist dieses Quadrat sicher groesser gleich
Null. Da im Weiteren $g_{i,j}$ als Adjazenzmatrixeintrag $\in \{0, 1\}$ liegt,
muss somit $g_{i,j}(x_i - x_j)^2$ insgesamt auch $\geq 0$ sein. Somit gilt also
$\langle v, Lv \rangle \geq 0$ und damit ist $L$ also positiv semidefinit.

### Satz

Der naechste Satz aus der spektralen Graphentheorie, den wir zeigen wollen, ist
nun, dass *die Anzahl der Zusammenhangskomponenten eines Graphen die
Vielfachheit des Eigenwerts Null im Laplace-Spektrum ist*.

Hierzu nochmals eine kurze Wiederholung, was Zusammenhangskomponenten sind. Man
nennt einen Graphen $G$ *zusammenhaengend*, wenn zwischen jedem Paar Knoten ein
Pfad existiert. Ein Graph besteht aus $k$ *Zusammenhangskomponenten*, wenn er in
$k$ inklusionsmaximale, zusammenhaengende Teilgraphen partitioniert werden kann.

#### Beweis

Fuer den Beweis untersuchen wir nun, fuer welche Vektoren $v = (x_1, ...,
x_n)^\top$ gilt, dass $\langle v, Lv \rangle = 0$. Oben hatten wir die folgende
Expansion von $\langle v, Lv \rangle$ gefunden:

$$\langle v, Lv \rangle = \mathop{\sum^n\sum^n}_{1 \leq i < j \leq n} g_{i,j}
(x_i - x_j)^2.$$

Wir sehen hier, dass diese ganze Summe nur dann Null sein kann, wenn jedes mal,
wenn $g_{i,j} = 1$ ist (kann nur $\in \{0, 1\}$ sein), auch $(x_i - x_j)^2 = 0$
gilt. Das heisst wiederum, dass immer wenn $g_{i,j} = 1$, $x_i = x_j$ gelten
muss. Nun sind $g_{i,j}$ ja die Eintraege der Adjazenzmatrix von $G$. $g_{i,j}$
ist also immer dann gleich Eins, wenn Knoten $i$ mit Knoten $j$ (direkt)
verbunden ist (wenn sie *adjazent* sind). Somit haben wir also, dass immer dann,
wenn Knoten $i$ mit Knoten $j$ direkt verbunden ist, Eintrag $x_i$ mit $x_j$
uebereinstimmen muss. Das uebertraegt sich nun transitiv auf alle Knoten in der
Zusammenhangskomponente von $i$. Sind $i$ und $j$ verbunden, muss also $x_i =
x_j$ sein. Ist Knoten $j$ dann mit Knoten $k$ verbunden, so muss also $x_j =
x_k$. Das geht dann fuer alle Knoten in der Zusammenhangskomponente von $i$ so
weiter (Zusammenhangskomponenten sind Aequivalenzklassen).

Ist $Z_k$ also die $k$-te Zusammenhangskomponente von $G$, so gibt es eine Zahl
$\alpha_k \in \mathbb{R}$, sodass $x_i = \alpha_k$ fuer alle Knoten $x_i \in
Z_k$. Wir waehlen also fuer jede Zusammenhangskomponente eine Zahl $\alpha_k$
und setzen alle Werte des Vektors $v$ auf diesen Wert. Fuer jeden Vektor, den
wir auf diese Weise erzeugen koennen, wird $\langle v, Lv \rangle = 0$ gelten.

Anders betrachtet erzeugen also $k$ Variablen bzw. Koeffizienten einen jeden
solchen Vektor $v$. Der Vektor $v$ ergibt sich dann ja als Linearkombination von
$\sum_{i=1}^k \alpha_i b_i$, wo $b_i$ ein Vektor ist, mit einer Eins in jeder
Komponente $x_i$, wo $x_i \in Z_k$ und Null ueberall sonst. Diese $b_i$ sind
also eben Basisvektoren von $v$ und die $\alpha_i$ die Koeffizienten dieser
Basisvektoren. Da wir fuer jede Zusammenhangskomponente also einen solchen
Basisvektor haben, ist die Anzahl der Basisvektoren gleich der Anzahl an
Zusammenhangskomponenten unseres Graphen $G$. Sei nun

$$E_0 = \{ v \in \mathbb{R}^n \,|\, \langle v, Lv \rangle = 0\}$$

der Raum aller dieser Vektoren, sodass $\langle v, LV \rangle = 0$. Dann nennen
wir die Anzahl an Basisvektoren dieses Raumes ja die *Dimension*
$\dim(E_0)$. Somit haben wir also:

$$\dim(E_0) = \text{ Anzahl der Zusammenhangskomponenten }$$

Es bleibt also nur noch zu zeigen, dass diese Dimension $\dim(E_0)$ die
Vielfachheit des Eigenwerts $0$ von $L$ ist. Hierfuer betrachten wir eine
*Orthonormalbasis* $\{v_1, ..., v_n\}$ aus Eigenvektoren von $L$, die den ganzen
Raum $\mathbb{R}^n$ aufspannen. Da die Laplace-Matrix $L$ symmetrisch ist,
wissen wir, dass es eine solche Orthonormalbasis gibt. Wenn nun also die Anzahl
der Zusammenhangskomponenten gleich der Vielfachheit des Eigenwerts $0$ ist (das
wollen wir zeigen), so muss es zumindest ein Vorkommnis dieses Eigenwertes
geben. Allgemein nehmen wir nun an, dass es $1 \leq l \leq n$ viele Vorkommnisse
des Eigenwerts $0$ gibt. Es gelte also o.B.d.A., dass $\lambda_1 = ... =
\lambda_l = 0$. Da wir vorhin gezeigt haben, dass Laplace-Matrizen positiv
semi-definit sind, muessen also alle weiteren Eigenwerte $\lambda_i$ mit $i > l$
dann *positiv* sein.

Betrachten wir nun weiter einen Vektor $v$. Wir wissen, dass wir $v$ zur obigen
Orthonormalbasis darstellen koennen, also gilt $v = \sum_{i=1}^n y_i v_i \in
\mathbb{R}^n$ fuer geeignete Koeffizienten $y_i$. Sei dann $y$ der Vektor, der
diese Koordinaten $y_i$ enthaelt. Das ist dann also $v$ in dieser
Orthonormalbasis.

Betrachten wir dann weiter $\langle v, Lv \rangle$. Wir koennen nun $v$ einfach
in die Orthonormalbasis wechseln, denn das ist ja dann der selbe Vektor, also
muessen selbe Eigenschaften gelten. Insbesondere ist $Lv$ dann naemlich viel
einfacher zu schreiben. Denn $L$ ist als symmetrische Matrix ja diagonalisierbar
(deswegen die Orthonormalbasis) und somit aehnlich zu einer Diagonalmatrix $D$
mit den Eigenwerten in der Diagonalen. Durch die Aehnlichkeit ist die Abbildung
$Lv$ dann eben aequivalent zur Abbildung $Dy$. Da $D$ eine Diagonalmatrix ist,
ist diese Abbilidng im Weiteren gleich einer komponentenweisen Skalierung von
$y$, wobei jede Komponente $y_i$ um den jeweiligen Eigenwert $\lambda_i$
skaliert wird. Somit gilt:

$$\langle v, Lv \rangle = \langle y, Dy \rangle$$

Die $i$-te Komponente von $Dy$ ist also $\lambda_i y_i$. Das Skalarprodukt
$\langle y, Dy \rangle$ multipliziert dann diese $i$-te Komponente noch jeweils
mit $y_i$ und ergibt somit fuer jede Komponente $y_i \lambda_i y_i = \lambda_i
y_i^2$. Das Skalarprodukt summiert dann noch ueber alle Eintraege. Dadurch
ergibt sich:

$$\langle v, Lv \rangle = \langle y, Dy \rangle = \sum_{i=1}^n \lambda_i y_i^2$$

Ueberlegen wir uns nun, wann genau diese Summe Null ist. Wir wissen, dass die
ersten $l$ Eigenwerte Null sind. Somit sind die Werte $y_1, ..., y_l$ schon mal
egal. Die uebrigen Eigenwerte $\lambda_{l + 1}, ..., \lambda_n$ haben wir dann
definitiv groesser Null, also insbesondere ungleich Null gewaehlt. Dann kann
diese ganze Summe also nur genau dann Null sein, wenn alle $y_{l+1}, ..., y_n$
Null sind. Da sie naemlich quadriert werden, koennen sich auch nicht zwei $y_i$
aufheben. Also muessen sie alle Null sein, damit $\langle v, Lv \rangle = 0$
gilt. Wenn also nun die oberen $n - l$ Komponenten Null sind, so verbleiben nur
die ersten $l$ Komponenten, um den Vektor zu definieren. Die ersten $l$
Komponenten entsprechen $l$ Koordinaten der Orthonormalbasis und somit $l$
Basisvektoren. Somit wissen wir also, dass es genau $l$ Basisvektoren von $E_0$
gibt, die ja den Raum der Werte $v$, sodass $\langle v, Lv \rangle = 0$,
aufspannen.

Also ist die Dimension $\dim(E_0)$ dieses Raumes genau $l$. $l$ war im Weiteren
aber gerade auch die von uns bestimmte Vielfachheit des Eigenwerts Null. Und
oben haben wir herausgefunden, dass $\dim(E_0)$ gleich der Anzahl an
Zusammenhangskomponenten eines Graphen ist. Somit muss also transitiv die Anzahl
an Zusammenhangskomponenten gerade gleich der Vielfachheit des Eigenwerts Null
im Laplace-Spektrum sein. Das ist gerade das, was wir zeigen wollten. Also haben
wir:

__Die Anzahl an Zusammenhangskomponenten eines Graphen ist gleich der
Vielfachheit des Eigenwerts Null im Laplace-Spektrum.__

## Zusammenfassung

Hier nun die wichtigsten Eigenschaften, die in diesem Kapitel vorgestellt
wurden. Sei dafuer $G = (V, E)$ ein Graph und $A$ dessen Adjazenzmatrix. Dann
sei noch $L$ die Laplace-Matrix von $L$ und letzlich $G' = (V', E')$ ein
weiterer Graph.

* Das Spektrum von $G$ ist $\{\lambda_1, ..., \lambda_n\}$.
* Das Spektrum ist eine Graphinvariante, die Adjazenzmatrix nicht.
* Ist $G$ isomorph zu $G'$, so sind ihre Spektren gleich.
* Ist $G$ isomorph zu $G'$, so sind ihre Laplace-Spektren gleich.
* Sind Spektrum oder Laplace-Spektrum oder beide Spektren zweier Graphen gleich,
  sagt noch nicht, dass $G$ isomorph zu $G'$ sein muss.
* $L$ hat den Grad der Knoten in der Diagonalen und $-A$ ueberall sonst.
* $L$ ist positiv semidefinit.
* Die Anzahl der Zusammenhangskomponenten von $G$ ist die Vielfachheit des
  Eigenwertes Null im Laplace-Spektrum.
