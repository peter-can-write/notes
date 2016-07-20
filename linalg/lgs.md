# Lineare Gleichungssysteme

$\newcommand{\rang}{\mathop{\rm rang}\nolimits}$

Wir wollen nun lineare Gleichungssysteme betrachten, ein sehr wichtiges Gebiet
der linearen Algebra. Lineare Gleichungssysteme arbeiten mit Termen wie $ax_1 +
bx_2$. Hierbei nennt man diese Terme *linear*, weil Variablen nie miteinander
multipliziert werden (weder mit sich selbst, also keine hoehergradigen Terme
$x_i^2$, noch mit anderen Variablen, also $x_ix_j$).

## Definition

Eine Gleichung der Form $A \cdot x = b$ mit einer Matrix $A \in K^{m \times n}$
und einem Vektor $b \in K^m$ heisst *lineares Gleichungssystem* (LGS). Die
*Loesungsmenge* des LGS ist die Menge aller $x \in K^m$, die das LGS
erfuellen. Wir koennen das visualisieren, indem wir uns die Matrix $A$ als
Konkatenation von Spaltenvektoren $a_1, ..., a_n$ vorstellen, sodass also $A =
(a_1 | ... | a_n)$. Was $Ax$ dann naemlich ausdrueckt, ist $x_1a_1 + ... +
x_na_n$. D.h. wir multiplizieren die jeweilige Variable $x_i$ mit dem
entsprechenden Koeffizent jeder Zeile, also jeder Gleichung. Die Summe der so
enstandenen Zeilen ergeben die herkoemmliche Darstellung von Gleichungssystemen
als eine Kollektion von Gleichungen der Form $a_{i,1}x_1 + ... + a_{i,n}x_n =
b_i$.

Ein LGS laesst sich vollstaendig durch die erweiterte Koeffizientenmatrix $(A |
b) \in K^{m \times (n + 1)}$ beschreiben. Diese Notation steht fuer $A$ mit der
zusaetzlichen Spalte $b$. Fuer diese wird der Vektor $b$ als $n + 1$-te Spalte
an $A$ angehaengt. Ist $b$ der Nullvektor, so nennt man dieses LGS $(A | 0)$
*homogen*. Ansonsten nennt man es *inhomogen*.

### Zeilenstufenformen

Um ein LGS zu loesen, werden wir die erweiterte Koeffizientenmatrix in eine
"schoene" Form bringen, aus der man die Loesungsmenge ablesen kann. Dabei
muessen wir aufpassen, dass die Loesungsmenge nicht veraendert wird. Die
erlaubten Manipulationen heissen *elementare Zeilenoperationen*. Von diesen gibt
es drei Arten:

1. Vertauschen zweier Zeilen (Gleichungen).
2. Multiplikation einer Zeile mit einem Skalar.
3. Addieren eines Vielfachen einer Zeile zu einer anderen.

Eine Matrix $A \in K^{m \times n}$ ist in *Zeilenstufenform*, falls die
folgenden Eigenschaften gelten:

1. Wenn eine Zeile mit $k$ Nullen beginnt, so stehen unter diesen Nullen *nur
   weitere Nullen*.
2. Unter dem ersten Eintrag $\neq 0$ jeder Zeile stehen lauter Nullen (falls die
   Zeile nicht nur aus Nullen besteht).

Zusaetzlich ist $A$ in *strenger* Zeilenstufenform, wenn (1) und (2) gelten und:

3. Ueber dem ersten Eintrag $\neq 0$ jeder Zeile stehen lauter Nullen.

Die vorhin angesprochene *schoene* Form ist also die *strenge* Zeilenstufenform.

### Gauss-Algorithmus

Der sogenannte *Gaus-Algorithmus* (auch: *Gauss-Elimination*) ist ein Verfahren,
um eine Matrix in die *strenge* Zeilenstufenform zu bringen, sodass man die
Loesung des LGS, das sich durch die Zeilen ergibt, ablesen kann. Als Eingabe hat
man eine Matrix $A \in K^{m \times n}$ und als Ausgabe eine Matrix $B \in K^{m
\times n}$ die aus $A$ durch elementare Zeilenoperationen hervorgeht.

1. Setze dazu anfangs $B := A$.
2. $B$ sei bis zur $r$-ten Zeile in Zeilenstufenform, wobei auch $r = 0$
   moeglich ist
3. Falls $r = m$, so ist $B$ in Zeilenstufenform. Fuer die strenge
   Zeilenstufenform, gehe zu (8).
4. Waehle die Spalte unter der $r$-ten, die den am weitesten links stehenden
   Eintrag $\neq 0$ hat.
5. Vertausche diese Spalte mit der $(r + 1)$-ten, bringe sie also unter die
   $r$-te Zeile (Typ (1)).
6. Erzeuge unterhalb diesen $(r + 1)$-ten Eintrags lauter Nullen (Typ (2,
   3)).
7. Gehe zu (2).
8. $B$ ist nun in Zeilenstufenform. Bringe $B$ noch in die *strenge*
   Zeilenstufenform beginnend mit der letzen Zeile, durch Anwendung von
   Typ (3) Operationen.

Dieser Algorithmus kann noch fuer viele weitere Dinge verwendet werden, als nur
das Loesen von linearen Gleichungssystemen. Beispielsweise kann man hiermit
Inverse von Matrizen finden oder Eigenwerte bestimmen.

### Algorithmus zur Loesung eines LGS

Input: LGS $Ax = b$, mit $A \in K^{m \times n}, b \in K^n$
Output: Loesungsmenge $\mathbb{L}$ des LGS.

1. Bringe die erweiterte Koeffizientenmatrix $(A | b) \in K^{m \times (n + 1)}$
   in die strenge Zeilenstufenform (Gauss-Algorithmus).

2. Sei $r$, der __Rang__ der Matrix, die Anzahl an Zeilen, die einen Eintrag
   $\neq 0$ haben. Fuer $i = 1, ... ,r$ sei $j_i \in \{1, ..., n+1\}$ jene Spalte
   in Zeile $i$, in der der erste Eintrag $\neq 0$ steht.

3. Falls $j_r = n + 1$, also falls in einer Zeile im $A$ Teil nur Nullen stehen,
   und der erste Nicht-Null Eintrag erst die $r$-te Komponente in $b$ ist, so
   ist das LGS unloesbar. D.h. $\mathbb{L} = \emptyset$. Dann haben wir naemlich
   so etwas wie $0 = 4$, was immer eine falsche Aussage ist.

4. Andernfalls seien $k_1, ... , k_{n - r}$ die Spalten $\leq n$ (also von $A$),
   die nicht eines der $j_i$ sind. Das sind also jene Spalten, die in keiner
   Zeile ganz links (von allen Spalten) nicht null sind. Formal: $\{1, ..., n\}
   \setminus \{j_1, ..., j_r\} = \{k_1, ..., k_{n-r}\}$. Fuer diese $n - r$
   Spalten erhalten wir dann $n - r$ freie Variablen. Das ist so, weil wenn sie
   nie ganz links stehen, koennen wir spaeter nie die Zeile (als Gleichung) fuer
   sie aufloesen (ausser man macht dabei eine andere Variable frei). Wir
   sprechen von $n - r$ __Freiheitsgraden__. Man beachte hier, dass wir den Rang
   aus den Zeilen errechneten, also in Betracht von $m$, der Anzahl an
   Zeilen. Hier machen wir nun eine Aussage bezueglich den $n$ *Spalten*. Also
   wenn nur $r$ *Zeilen* einen Eintrag ungleich Null haben, dann sagen uns die
   $n - r$ Spalten, dass wir $n - r$ Freiheitsgrade haben. Das sind in unserer
   Loesungsmenge dann Variablen, sodass wir einen beliebigen Wert $\in K$
   einsetzen koennen, sodass das LGS noch immer geloest werden kann.

5. Die Loesungsmenge $\mathbb{L}$ ist dann gegeben durch:
   $\mathbb{L} = \{\begin{bmatrix}x_1 \\ \vdots \\ x_n\end{bmatrix}\,|\,
   x_k,...,x_{k_{n-r}} \in K \text{ beliebig }, x_{j_i} = a_{i_{j_i}}^{-1} \cdot
   (b_i - \sum_{j=1}^{n-r} a_{ik_j} \cdot x_{k_j}) \text{ fuer } i \leq r\}$

Schritt fuer Schritt: Sei $x_{j_i}$ die $i$-te Variable, die einmal in einer
Zeile ganz links ungleich Null ist. Dann loesen wir nun diese Zeile nach dieser
Variable auf. Rechts, im $b$ Teil, steht die Loesung dieser Gleichung. Sei diese
in dieser Zeile $b_i$. Wir subtrahieren nun auf beiden Seiten zunaechst die
anderen Variablen mit ihren Koeffizienten. Da die Matrix in strenger
Zeilenstufenform ist, wissen wir, dass fuer alle anderen Nicht-Freiheitsgrade
der Eintrag in dieser Zeile 0 sein wird. Es bleiben also nur die Freiheitsgrade
mit ihren Koeffizienten. Das ergibt die Summe $\sum_{k=1}^{n - r}
a_{i,k}x_{i,k}$. Diese schieben wir nun auf die andere Seite. Dann haben wir
$b_i$ minus dieser Summe. Dann haben wir nur noch ein Problem: wir wollen nach
$x_{j_i}$ aufloesen. Diese Variable hat aber vielleicht noch einen Koeffizienten
$a_{i,j_i}$. Also dividieren wir auf beiden Seiten damit bzw. multiplizieren mit
dem Inversen. So ergibt sich also

$$x_{j_i} = a^{-1}_{i,j_i} \cdot (b_i - \sum_{k=1}^{n - r} a_{i,k}x_{i,k})$$

Oder anders einfach: Trage fuer die Freiheitsgrade Variablen $x_1, ..., x_n$ ein
und loese fuer die $r$ andere Variablen in Abhaengigkeit von den $n - r$
Freiheitsgraden jeweils eine Gleichung auf.

### Geometrische Anwendungen

Wir wollen zwei geometrische Anwendungen von LGS betrachten, um eine bessere
bzw. zusaetzliche Intuition dessen zu erhalten, was ein LGS ist. Es gibt zwei
Moeglichkeiten, LGS geometrisch anzuwenden:

1. Schneiden von Figuren
2. Finden von Funktionen

#### Schneiden von Figuren

LGS koennen benutzt werden, um Figuren zu schneiden. Je nach Figurentyp
entspricht das dann dem Schneiden von Geraden, Ebenen oder hoeherdimensionalen
Koerpern. Wir sprechen also allgemein vom Schneiden von $n$-dimensionalen
Figuren im $R^n$. Solch eine Figur besteht immer aus:

1. Einem $n$-dimensionalen Ortsvektor $O \in \mathbb{R}^n$.
2. Maximal $n - 1$ viele Richtungsvektoren $a \cdot (x_1, ..., x_n)^\top$.

Beispiele hierfuer waeren:

1. Geraden im zweidimensionalen Raum $\mathbb{R}^2$. Diese sind dann beschrieben
   durch einen Ortsvektor $o = (o_1, o_2)^\top$ und einem ($n - 1 = 2 - 1 = 1$)
   Richtungsvektor $r = (x, y)^\top$. So hat eine Gerade allgemein gegeben durch:
   $(x', y')^\top = ar + o = a(x, y)^\top + (o_1, o_2)^\top$ mit $a \in \mathbb{R}$
2. Geraden im dreimensionalen Raum $\mathbb{R}^3$. Diese sind dann gleich wie in
   der Ebene beschrieben, haben aber fuer jeden Vektor noch eine $z$ Komponente.
3. Ebenen im dreimensionalen Raum. Diese haben dann eben auch drei Komponenten
   pro Vektor, sind aber noch durch einen zweiten Richtungsvektor $s = (x, y)^\top$
   beschrieben. Dann hat man einen Punkt auf einer Ebene also durch:
   $(x', y')^\top = ar + bs + o = a(x, y)^\top + b(x, y)^\top + (o_1, o_2)^\top$
   mit $a,b \in \mathbb{R}$

Um zwei solche Figuren zu schneiden, wuerde man nun folgendes tun:

Sei $F_1$ eine Figur gegeben durch $o_1 + \sum_{i=1}^{k} a_{1,i}(x_{i,1}, ...,
x_{i,n})^\top$, also als Linearkombination von $k < n$ Richtungsvektoren. Sei
auch $F_2$ eine solche Figur, gegeben durch $o_2 + \sum_{j=1}^{k} a_j(x_{j,1},
..., x_{j,n})^\top$ mit eben auch $k$ Richtungsvektoren. Hierbei ist $(x_{i,1},
..., x_{1,n})^\top = x_i$ ein Vektor des $\mathbb{R}^n$, welcher also gerade der
$i$-te Richtungsvektor ist.

Dann wollen wir also die Schnittmenge von $F_1, F_2$ finden. Das ist gerade die
Menge aller Punkte, wo $F_1 = F_2$. Wir setzen also die Figuren gleich. Dann
koennen wir alle rechten Richtungsvektoren nach links subtrahieren. Den linken
Ortsvektor subtrahieren wir noch nach rechts. Dann erhalten wir:

$\sum_{i=1}^{k} a_ix_i - \sum_{j=1}^{k} a_jx_j = o_2 - o_2$

Wir haben hier also nun ein LGS mit den Unbekannten $a_i$ und $a_j$, welche
jeweils die Skalierung fuer einen Richtungsvektor sind. Fuer Geraden im
zweidimensionalen Raum haetten wir z.B.:

1. $f: a \cdot (2, 3)^\top + (6, 8)^\top$
2. $g: b \cdot (-4, -6)^\top + (4, 5)^\top$

Dann setzen wir also $f = g$ und formen um zu:

1. $a(2, 3)^\top - b(-4, -6)^\top = (4, 5)^\top - (6, 8)^\top$
2. $a(2, 3)^\top + b(4, 6)^\top = (-2, -3)^\top$

Was wir hier haben ist also genau ein LGS mit den Spaltenvektoren $(2, 3)^\top$
und $(4, 6)^\top$ und Loesungsvektor $(-2, -3)^\top$. Die aufzuloesenden
Variablen sind $a$ und $b$ -- die Koeffizienten der jeweiligen
Richtungsvektoren. So erhalten wir also das LGS:

$$
\begin{bmatrix}
2 & 4 \\
3 & 6 \\
\end{bmatrix}

\cdot

\begin{bmatrix}
a \\ b
\end{bmatrix}

=

\begin{bmatrix}
-2 \\ -3
\end{bmatrix}
$$

Also $Ax = b$ mit $x = (a, b)^\top$. Wenn wir dieses LGS loesen, erhalten wir
genau die entsprechenden Koeffizienten fuer den Loesungsraum. Das ist dann
entweder die leere Menge, wennn die Figuren parallel zu einander sind; ein
Punkt, wenn sie sich genau schneiden und man ausreichend Figuren hat
(z.B. mindestens drei Ebenen) oder eine endliche grosse Menge, falls es freie
Variablen gibt.

Interessanterweise kann man Ebenen z.B. nur schrittweise schneiden. Im
dreidimensionalen Raum erhalten wir fuer unser LGS drei Zeilen, fuer die $x, y,
z$ Koordinaten. Wir haben aber vier Unbekannte bzw. Spalten, fuer die
Koeffizienten der vier Richtungsvektoren (zwei pro Ebenen). Drei Ebenen koennen
sich zwar schon schneiden, nur kann man durch zugabe einer weiteren Ebene nichts
erreichen. Dann haette man einfach sechs Unbekannte aber immer noch nur drei
Gleichungen. Man muesste also zuerst je zwei Ebenen schneiden um eine
Schnittgerade zu bestimmen und dann diese Geraden schneiden.

#### Finden von Funktionen

Mit LGS kann man auch Funktionen finden. Betrachten wir hierzu eine multivariate
Funktion $$f(x_1, ..., x_n)= \sum_{i=1}^n a_ix_i$$. Wir waehlen eine solche
Funktion, weil sie sich auch leicht zum bekannten Fall eines Polynoms in $x$
spezialisieren laesst, wenn $x_i = x^i$. Dann laesst sich $f(x_1, ..., x_n)$
auch einfach als $f(x)$ schreiben. Jedenfalls haben wir dann also $n$ unbekannte
Variablen. Die $a_i$ sind Konstanten. Wir brauchen also zur Loesung $n$
Gleichungen und insbesondere $n$ Loesungen der Funktion. Dann koennen wir ein
ganz einfaches Loesungssystem fuer unsere $n$ Variablen aufstellen und nach
diesen loesen.

## Ablesen der Loesungsmenge ohne Formel

Wir haben vorhin eine Formel kennengelernt, womit man die Loesung aus der
strengen Zeilenstufenform ablesen kann.

### Homogenes LGS (alle rechte Seiten null):

$$
\begin{bmatrix}
1 & 0 & 2 & 0 & 1  & | & 0 \\
0 & 1 & 2 & 0 & -1 & | & 0 \\
0 & 0 & 0 & 1 & 2  & | & 0 \\
0 & 0 & 0 & 0 & 0  & | & 0 \\
\end{bmatrix}
$$

Zeilen 1, 2 und 4 muessen die Bedinungen fuer die strenge Zeilenstufenform
erfuellen. Man nennt sie *Basisvariablen*. Zeilen 3 und 5 bzw. die zugehoerigen
Variablen sind *frei*, d.h. unrestringiert. Die Anzahl dieser Spalten gibt die
Freiheitsgrade der Loesungsmenge an und ist immer $n - \rang(A)$.

Wir koennen jetzt fuer die freien Variablen beliebige Werte ($a, b, c, ...$)
einsetzen und nach den nicht-freien aufloesen:

$x_1 = -2x_3 - x_5$
$x_2 = -2x_3 + x_5$
$x_3$ frei
$x_4 = -2x_5$
$x_5$ frei

Unsere Loesungsmenge ist dann $\mathbb{L} = \left\{\begin{bmatrix}-2a_3 - a_5 \\
-2a_3 + a_5 \\ a_3 \\ -2a_5 \\ a_5 \end{bmatrix} |\, a_3,a_5 \in K\right\}$.

### Fuer inhomogene LGS:

Inhomogene Gleichungssysteme koennen durch einen Trick auch aehnlich wie
homogene Systeme geloest werden, indem man sie in homogene Systeme transformiert.

Schritt 1: Bestimme Loesungesmenge $\mathbb{L}_0$ fuer das homogene System $(A | 0)$.
Schritt 2: Bestimme *einen* Vektor $x$ sodass $Ax = b$.
Schritt 3: Addiere diesen einen Vektor $x$ auf den Loesungsraum von vorhin.

Beispiel:

$$
\begin{bmatrix}
1 & 0 & 2 & 0 & 1  & | & 0 \\
0 & 1 & 2 & 0 & -1 & | & 0 \\
0 & 0 & 0 & 1 & 2  & | & 0 \\
0 & 0 & 0 & 0 & 0  & | & 0 \\
\end{bmatrix}
$$

z.B. hier: Wir waehlen $x_3 = x_5 = 0$

$x_4 + 2x_5 = 3 \Rightarrow x_4 = 3$
$x + 2x_3 - x_5 \Rightarrow x_2 = 0$
$x_1 + 2x_3 + x_5 \Rightarrow x_1 = 1$

Der Vektor ist dann $x = \begin{bmatrix}1 \\ 0 \\ 0 \\ 3 \\ 0\end{bmatrix}$.

Mathematische Formulierung, wieso das so ist:

1. $Ax = b$
2. $Ax + 0 = b$
3. $Ax + Ay = b$, wo $y$ die Loesungen des homogenen Gleichungssystems sind,
   sodass also $Ay = 0$.

Geometrische Intuition:

Betrachten wir zwei Ebenen im Raum, die sich in einer Geraden schneiden. Das
homogene LGS zum Schneiden dieser Ebenen (siehe oben fuer Konstruktion) entsteht
aus dem inhomogenen daraus, dass man die Ortsvektoren der beiden Ebenen auf den
Ursprung, also den Nullvektor, setzt. Dann loesen wir dieses LGS, bestimmen also
die Loesungsmenge. Sagen wir, die Ebenen bilden eine Schnittgerade. Dann ist der
Loesungsraum also eine Gerade. Da diese Gerade nach Konstruktion durch den
Ursprung geht, ist sie eigentlich ein Richtungsvektor. Was wir nach diesem
Verfahren dann machen, ist genau einen Punkt auf der *nicht verschobenen*
Geraden finden. Das ist dann also ein *Ortsvektor* auf der Geraden. Dann haben
wir also einen Ortsvektor (welcher ein beliebiger Punkt auf der Geraden sein
kann) sowie auch den Richtungsvektor gefunden. die nicht verschobene Gerade
bzw. der Loesungsraum des inhomogenen LGS ergibt sich also daraus, dass man den
Ortsvektor auf den Richtungsvektor, also die Loesungsmenge des homogenen LGS,
draufaddiert. Wooh!

## Varianten der Loesbarkeit

Es gibt drei Moeglichkeiten fuer die Loesbarkeit eines LGS:

1. Unloesbarkeit: $\mathbb{L} = \emptyset \Leftrightarrow j_r = n + 1$. Dann
   gilt also, dass der Rang von $A$ kleiner ist als der von $(A | b)$

2. Eindeutige Loesbarkeit: $|\mathbb{L}| = 1 \Leftrightarrow r = n$ und $j_r =
   n$. Dann nennt man die Matrix *regulaer*. Eine Matrix, die nicht regulaer
   ist, nennt man dann auch *singulaer*. In diesem Fall muss gelten: $j_i = i
   \forall i \leq n$. Die strenge Zeilenstufenform hat dann die typische Form,
   nur Werte auf den Diagonalen zu haben und sonst nur nullen, bzw. rechts
   wieder beliebige Werte. Die eindeutige Loesung kann man dann auch schon
   gleich ablesen. Es gilt dann, dass $\begin{bmatrix}x_1 \\ \vdots \\
   x_n\end{bmatrix} = \begin{bmatrix}b_1/a_{1,1} \\ \vdots \\
   b_n/a_{n,n}\end{bmatrix}$, wo $b_i$ die rechte Seite und $a_{i,i}$ der
   entsprechende Koeffizient auf der Diagonalen ist. Der Rang von $A$ ist dann
   gleich dem Rang von $(A | b)$, wobei dieser Rang dann eben gleich $n$ ist.

3. Uneindeutige Loesbarkeit: $|\mathbb{L}| \geq 1 \Leftrightarrow r < n$ und
   $j_r \neq n + 1$. Die Loesung hat dann $n - r$ Freiheitsgrade. Fuer diese
   kann man dann aus unserer Zahlenmenge beliebige Werte einsetzen. Daher gilt,
   dass wenn unsere Zahlenmenge unendlich ist, in diesem Fall auch
   $|\mathbb{L}|$ unendlich ist. In der strengen Zeilenstufenform haben dann
   $m - r$ Zeilen *nur* Nullen. Es gilt dann im Uebrigen auch, dass der Rang von
   $A$ gleich dem Rang von $(A | b)$ ist, aber der Rang kleiner $n$ ist.

Definition: Sei $A \in K^{m \times n}$ und $A' \in K^{m \times n}$ durch
elementare Zeilenoperationen durch $A$ entstanden und in Zeilenstufenform. Dann
ist der Rang von $A$ die Anzahl $r = \text{rg}(A) = \rang(A)$ an Zeilen in
$A'$, die __zumindest einen Eintrag $\neq 0$ haben__.  Eine quadratische Matrix
nennt man *regulaer*, wenn ihr Rang gleich $n$ ist, also in keiner Zeile eine
null steht. Man sagt dann, dass sie *vollen* Rang hat.

$\rang(I_n) = n$, $\rang(0) = 0$

Der Rang einer Matrix $A$ ist durch die Anzahl der Zeilen und die Anzahl der
Spalten nach oben beschraenkt:

1. Entweder wir haben nur $m < n$ Zeilen, dann haben wir nicht genug Gleichungen
   um eine eindeutige Loesung zu finden, was nur dann sein koennte, wenn der
   Rang gleich $n$ ist. Wir koennen also nicht mehr Variablen aufloesen, als wir
   Gleichungen haben.
2. Oder wir haben nur $n < m$ Variablen in den Spalten bzw. mehr als genug
   Zeilen (Gleichungen) um das LGS fuer die $n$ Variablen zu loesen (es gilt ja
   dann auch $\rang(A) = n$, wobei die Matrix aber auch nicht
   quadratisch). Hierbei gaebe es wohl die Fallunterscheidung, ob die Eintrage
   in $b$ fuer die Komponenten $i > n$ Null waeren oder nicht. Sind sie null,
   dann werden wir komplette Nullzeilen erhalten und die Zeilen sind einfach
   vollkommen unnoetig. Die eindeutige Loesung nach den $n$ Variablen waere
   dennoch moeglich. Wenn diese extra Komponenten aber nicht Null sind, dann
   wird der Rang von $A$ also kleiner dem von $(A | b)$ sein.

Es gilt also: $\rang(A) \leq \min{m,n}, A \in K^{m \times n}$.

Satz (Loesbarkeit von LGS): Ein LGS $Ax = b$ ist loesbar genau dann, wenn die
Koeffizientenmatrix $A$ denselben Rang hat wie die erweiterte
Koeffizientenmatrix $(A | b)$. Wenn $A$ einen kleineren Rang haette als $b$,
dann wuerden in mindestens einer Zeile links nur Nullen stehen, aber rechts
keine Null, was gerade die Bedingung fuer keine Loesbarkeit ist.

Zusammnfassend also fuer quadratische Matrizen (alles andere sind Sonderfaelle):

1. Unloesbar: $rang(A) < rang(A|b)$
2. Eindeutig loesbar: $rang(A) = rang(A|b) = n$
3. Uneindeutig loesbar: $rang(A) = rang(A|b) < n$

### Loesbarkeit fuer Homogene Gleichungssysteme

Fuer ein homogenes Gleichungssystem der Form $Ax = 0$ gibt es nur *zwei*
Varianten der Loesbarkeit:

1. Wenn der Rang der Matrix voll ist, also $\rang(A) = n$, dann hat das
   Gleichungssystem nur eine eindeutige Loesung. Diese eindeutige Loesung ist
   dann der *Nullvektor*. Das nennt man die *triviale Loesung* eines homogenen
   LGS. Weil natuerlich fuer jede Matrix $A$ gilt, dass $A \cdot 0 = 0$. Man
   beachte, dass wenn es rechts nur Nullen gibt, man auch nie etwas in dieser
   Spalte veraendern kann. Insofern kann es auch nur diese eindeutige, triviale
   Loesung geben.

2. Wenn der Rang der Matrix nicht voll ist, also $\rang(A) < n$, dann hat
   ein homogenes LGS unendlich viele Loesungen mit $n - \rang(A)$
   Freiheitsgraden. Die daraus entstehenden Loesungen nennt man die
   *nicht-trivialen* Loesungen, von welchen es immer unendlich viele gibt, wenn
   es sie gibt.

Ein homogenes LGS kann also nie keine Loesung haben, da es immer die triviale
Loesung gibt. Denn kommen wir nie in die schlechte Situation, dass
$\rang(A) < \rang(A|b)$, was bei einem inhomegenen LGS auf *keine
Loesung* hindeutet. Hat man naemlich in der Zeilenstufenform von $A$ in manchen
Zeilen nur Nullen, so werden diese Zeilen in $(A | b)$ auch weiterhin nur Nullen
haben, weil $b$ ja als Nullvektor nur Nullen enthaelt. Somit gibt es nie keine
Loesung.

Man beachte auch, dass $\rang(A)$ dann immer gleich $\rang(A|b)$ ist (wobei dann
eindeutige Loesbarkeit gilt, wenn $\rang(A) = n$).

## Zusammenfassung

Hier die wichtigsten Erkenntnisse aus diesem Kapitel. Sei hierfuer $A \in K^{m
\times n}$ eine Matrix. Dann gilt:

### LGS allgemein

* $A$ ist in Zeilenstufenform, wenn fuer jede Zeile $z$ gilt:
  1. Faengt $z$ mit $k$ Nullen an, muessen unter diesen Nullen nur Nullen
     folgen.
  2. Ist $j_z$ der erste Eintrag ungleich Null, so muessen unter dieser Spalte
     nur Nullen folgen.

* $A$ ist in *strenger* Zeilenstufenform, wenn $A$ in Zeilenstufenform und fuer
  jede Zeile gilt:
  3. Ist $j_z$ der Erste Eintrag ungleich Null, duerfen ueber diesem Eintrag nur
     Nullen stehen.
* $\rang(A)$ ist durch Zeilen- und Spaltenzahl beschraenkt: $\rang(A) \leq \min\{m,n\}$
* $A$ ist regulaer, wenn $\rang(A) = n$
* $A$ ist singulaer, wenn $\rang(A) < n$
* $Ax = b$ ist:
  1. Eindeutig loesbar, wenn $\rang(A) = n$.
  2. Uneindeutig loesbar, wenn $\rang(A) < n$ und $\rang(A) = \rang(A | b)$
  2. Nicht loesbar, wenn $\rang(A) < \rang(A | b)$
* Wenn $Ax = b$ loesbar, ist $n - \rang(A)$ die Anzahl an Freiheitsgraden.
* Der Loesungsraum von $Ax = b$ ist jener von $Ax = 0$ plus einem Loesungsvektor
  von $Ax = b$

### Homogene LGS

* $Ax = 0$ ist immer loesbar.
* $\rang(A) = \rang(A | b) = \rang(A | 0)$
* Ist $A$ eindeutig loesbar, so gibt es nur die triviale Loesung $0$.
* Ist $A$ uneindeutig loesbar, so gibt es $n - \rang(A)$ Freiheitsgrade.
