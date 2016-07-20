# Vektorraeume

Ein Vektorraum (ueber $K$) ist eine Menge $V$ von *Vektoren*, mit zwei
Abbildungen $+: V \times V \rightarrow V: (v,w) \mapsto v + w$ und $\cdot: K
\times V \rightarrow V: (a, v) \mapsto a \cdot v$, wobei gilt:

1. $V$ mit $+$ ist eine abelsche Gruppe.
2. $\forall a \in K, v,w \in V: a \cdot (v + w) = a \cdot v + a \cdot w$
   (Distributivitaet eines Skalars ueber Vektoren)
3. $\forall a,b \in K, v \in V: (a + b) \cdot v = a \cdot v + b \cdot v$
   (Distributivitaet eines Vektors ueber Skalare)
4. $\forall a,b \in K, v \in V: (a \cdot b) \cdot v = a \cdot (b \cdot v)$ (Assoziativitaet)
5. $\forall v \in V: 1 \cdot v = v$ ($1$ ist das neutrale Element der Multiplikation)

Beispiele:

1. $K^{m \times n}$ ist ein $K$-Vektorraum (weil die obigen Operationen
   elementweise definiert sind)
2. Der $n$-dimensionale $K^n$ ist ein $K$-Vektorraum.
3. $K$ ist ein $K$-Vektorraum.
4. $V = \{0\}$ mit $0 + 0 = 0$ und $a0 = 0 \forall a \in K$ ist auch ein
   $K$-Vektorraum, genannt *Nullraum*.
5. $\mathbb{R}$ ist auch ein $\mathbb{Q}$-Vektorraum.
6. $K[x] = \{a_nx^n + a_{n-1}x^{n-1} + ... + a_0 \,|\, n \in mathbb{N}, a_i in
   K\}$ ist auch ein $K$-Vektorraum.
7. Sei $M$ eine Menge und $V = \text{Abb}(M, K) = \{f: M \rightarrow K \,|\, f
   \text{ ist eine Abbildung}\}$, mit $f,g \in V, a \in K$: $f + g: M
   \rightarrow K, x \mapsto f(x) + g(x)$ und $a \cdot f: M \rightarrow K,
   \mapsto a \cdot f(x)$. Das ist eine *punktweise* Definition der
   Operationen. Der Nullvektor ist die Nullfunktion $f_0(x) = 0, \forall x \in
   M$. Dann ist naemlich $(f + f_0)(x) = f(x) + f_0(x) = f(x) + 0 = f(x)$. Die
   Nullfunktion erhaelt also eine andere Funktion bei der Addition.

Man beweist, dass eine Menge ein Vektorraum ist, oder nicht, durch explizite
Kontrolle aller Vektorraumeigenschaften, also:

1. Gruppeneigenschaften von $+$ (inklusive Vorhandensein der $0$, dem Neutralen
   Element)
2. Abgeschlossenheit bezueglich Skalarmultiplikation
3. $1 \cdot v = v$

Gegenbeispiel: Sei $V$ eine abelsche Gruppe mit neutralem Element, aber $V \neq
\{0\}$, mit der Eigenschaft dass $a \cdot v = 0, \forall a \in K, \forall v \in
V$. Dies ist __kein__  Vektorraum, weil Eigenschaft (5) nicht erfuellt ist ($v
\cdot 1$ muesste wieder $v$ sein).

## Rechenregeln fuer Vektoren

Sei $V$ ein $K$-Vektorraum, $a \in K$ ein Skalar und $v \in V$ ein Vektor. Es
gelten folgende Eigenschaften:

1. $a \cdot 0 = 0$, wobei $0$ der Nullvektor ist. Beweis:
   $a \cdot 0 = a \cdot 0 + a \cdot 0 - (a \cdot 0) = a \cdot (0 + 0) - (a \cdot
   0) = a \cdot 0 - (a \cdot 0) = 0$.

2. $0 \cdot v = 0$, wobei $0$ ein Skalar aus dem Koerper ist. Beweis: $0 \cdot v
   = 0 \cdot v + 0 \cdot v - (0 \cdot v) = (0 + 0) \cdot v - (0 \cdot v) = 0
   \cdot v - (0 \cdot v) = 0$

3. $(-a) \cdot v = a \cdot (-v) = -(a \cdot v)$. Beweis: $-av = (-1)av = a(-1)v$
   (Skalare darf man kommutieren!)

3. $av = 0 \Rightarrow a = 0 \lor v = 0$, es muss also zumindest eines der
   beiden Null gewesen sein. Beweis:
   $v = 1 \cdot v = (a^{-1} \cdot a) \cdot v = a^{-1} \cdot (a \cdot v) = a^{-1}
   \cdot 0 = 0$.

## Untervektorraeume

Sei $V$ ein $K$-Vektorraum. Eine Teilmenge $U \subseteq V$, welche die selben
Abbildungen $+$ und $\cdot$ benutzt, heisst *Untervektorraum*, wenn gilt:

1. $U \neq \emptyset$
2. $\forall v,w \in U: v + w \in U$ (Abgeschlossenheit bezueglich Vektoraddition)
3. $\forall s \in K, v \in U: sv \in U$ (Abgeschlossenheit bezueglich
   Skalarmultiplikation)
4. Aus (3) folgt, dass die Untergruppe dasselbe neutrale Element, also die Null,
   besitzen muss (siehe oben (2)).

Einfacher: Der Vektorraum muss fuer alle Linearkombinationen seiner Elemente
abgeschlossen sein.

Jeder Untervektorraum ist ein Vektorraum.

Beispiel: Ist $V = \mathbb{R}^2$, dann ist jede *Gerade durch den Nullpukt* ein
Untervektorraum. Das heisst, jede lineare Funktion ist ein Untervektorraum des
$\mathbb{R}^2$, aber eine affine Funktion nie. Deswegen nicht, weil der
Nullvektor nicht enthalten waere. Da die Funktion ja nicht linear waere, wuerde
man den Ortsvektor bei der Addition dann auch zweimal addieren, was zu einem
komplett anderen Ort fuehren koennte. Eine affine Gerade waere also auch nicht
bezueglich der Vektoradditiona bgeschlossen.

Sei $v \in V$, dann gilt: $K \cdot v = \{av \,|\, a \in K\}$ ist ein
Untervektorraum. D.h. wenn ich den ganzen Koerper mit einem Vektor
multipliziere, erhalte ich einen Untervektorraum. Das ist naemlich genau das,
was eine Gerade ist.

Man beachte, dass die Gerade wirklich durch den Nullpunkt gehen muss. Ein
*affiner Untervektorraum* ist dann ein Untervektorraum, plus einen
(Orts-)Vektor, also $v + U$. Untervektorraeume passen also zu Loesungsmengen
homogener Gleichungssysteme (z.B. Schnitt von Ebenen im Ursprung), affine zu
Loesungsmengen inhomogener Loesungsmenge ($\mathbb{L} + v$). Insbesondere sind
Loesungsmengen linearer Gleichungssysteme immer Untervektorraeume des $K^n$, da
sie sich ja als Linearkombination freier Variablen ergeben. Die Koeffizienten
der einzelnen Basisvektoren sind Skalare, also entsprechend bezueglich der
Addition (was bei der Vektoraddition ja passiert) und Multiplikation
abgeschlossen.

Befinden wir uns um $\mathbb{R}^3$, dann sind jede Gerade und jede Ebene durch
den Nullpunkt Untervektorraeume. Dann sind natuerlich noch $\{0\}$ sowie der
Vektorraum selbst Untervektorraeume. Mehr gibt es nicht.

Sei $M$ eine Menge und $V = Abb(M, K)$ und sei $x \in M$ fest. Dann ist $U = \{f
\in V \,|\, f(x) = 0\} \subseteq V$ ein Untervektorraum. Es gilt:

1. $U \neq \emptyset$ (es gibt zumindest die Nullfunktion)
2. $(f + g)(x) = f(x) + g(x) = 0 + 0 = 0$
3. $(s \cdot f)(x) = s \cdot f(x) = 0 \cdot 0 = 0$

Hingegen wuerde $f(x) = 1$ als Eigenschaft keinen Untervektorraum bilden, denn:

* $\{f \in V\,|\, f(x) = 1\} \Rightarrow (f + g)(x) = f(x) + g(x) = 1 + 1 = 2$

Die Loesungsmenge eines homogenen LGS $Ax = 0$ mit $A \in K^{m \times n}$ ist
ein Untervektorraum von $K^n$. Denn es gilt:

* Fuer die Abgeschlossenheit bezueglich der Vektoraddition muesste gelten, dass
  die Addition zweier Loesungen $x, y$ des homogenen LGS $Av = 0$ wiederum eine
  Loesung ist, sodass also $A(x + y) = 0$.
  Beweis: $A(x + y) = Ax + Ay = 0 + 0 = 0$
* $A(K \cdot x) = K \cdot Ax = K \cdot 0 = 0$

Die Vereinigung zweier Geraden $U_1, U_2 \in \mathbb{R}^2$ durch Null ist
allerdings nicht immer ein Untervektorraum. Nehmen wir einen Vektor aus $U_1$
und einen Vektor aus $U_2$, wobei die Geraden orthogonal sind. Dann ist die
Summe dieser Vektoren auf keiner der beiden Geraden. Also waere der Unterraum
nicht abgeschlossen. Erst die Summe der beiden Unterraeume ist der kleinste
Unterraum, der beide enthaelt.

### Eigenschaften von Untervektorraeumen

* Der Schnitt $U_1 \cap U_2$ aus zwei Untervektorraeumen $U_1, U_2$ ist wieder
  ein Untervektorraum. Das ist ja eben was wir bei LGS machen.

* Die Summe aus zwei Untervektorraeumen $U_1, U_2$ ist wieder ein
  Untervektorraum. Hierbei ist $U_1 + U_2 = \{v + w \,|\, v \in U_1, w \in
  U_2\}$. Insbesondere liegt $U_1 \cup U_2$ in dieser Menge. Diesen Raum nennt
  man auch *Summenraum*.  Wieso gilt diese Eigenschaft? Es liegt sicher schon
  einmal das Null-Element drin, da $0 \in U_1, 0 \in U_2$ und somit $0 + 0 = 0
  \in U_1 + U_2$ sein wird. Seien dann $v + w, v' + w' \in U_1 + U_2, v,v' \in
  U_1, w,w' in U_2$. Dann gilt $(v + w) + (v' + w') = (v + v') + (w +
  w')$. Diese beiden Terme in Klammern sind beide Elemente aus $U_1$ bzw. $U_2$,
  fuer welche wir die Abgeschlossenheit schon wissen. Ebenso gilt $a(v + w) =
  av + aw \in U_1 + U_2$ ($av,aw$ jeweils in $U_1,U_2$).

* Ist $M \neq 0$ eine nichtleere Menge, deren Elemente Untervektorraeume von $V$
  sind, so ist der Schnitt $\bigcap_{U \in M} U$ all dieser Untervektorraeume
  wieder ein Untervektorraum. Denn:
  1. Da die $U$ alle Untervektorraeume waren, muessen sie alle den Nullvektor
     enthalten haben. Dann ist diesr also auch im Schnitt.
  2. Da jeder der Untervektorraeume abgeschlossen war, gilt auch dass die Summe
  je zwei solcher Elemente aus dem Schnitt, welche beide in jedem
  Untervektorraum sind, wieder *in jedem der Untervektorraeume* enthalten sein
  muss. Also ist diese Summe auch im Schnitt, womit dieser bezueglich der
  Vektoraddition abgeschlossen ist. Analog fuer die Skalarmultiplikation.

### Erzeugnis

Sei $V$ ein $K$-Vektorraum und $S \subseteq V$ eine Untermenge von $V$, welche
aber nicht Untervektorraum sein muss. Sei $M = \{U \subseteq V \,|\, U \text{
ist Untervektorraum von V mit } S \subseteq U\}$, also die Menge aller
Untervektorraeume von $V$, die $S$ enthalten. Dann heisst $\langle S \rangle$
definiert als __$\langle S \rangle = \bigcap_{U \in M} U$__ der von *S
aufgespannte oder erzeugte Untervektorraum* bzw. das *Erzeugnis* von $S$. Fuer
$S = \{v_1, ..., v_n\}$ endlich schreiben wir $\langle S \rangle$ als $\langle
v_1, ..., v_n \rangle$.

Nehmen wir beispielsweise einen beliebigen Vektor $v \in \mathbb{R}^2$, sodass
$S = \{v\}$. Dann koennen wir einen Untervektorraum $\in M$ bilden, der $v$
enthalet, indem wir eine Gerade durch den Ursprung und $v$ bilden. Aber
natuerlich ist auch $\mathbb{R}^2$ selbst in $M$. Also besteht $M$ aus dieser
Geraden und dem ganzen Raum. Der Schnitt ueber alle Untervektorraeume in $M$,
also der Geraden und dem Raum, ist dann wieder die Gerade. Also ist die Gerade
der von $S$ bzw. $v$ aufgespannte Untervektorraum.

Beobachtung: $\langle S \rangle$ ist der __kleinste (inklusionsminimalste)
Untervektorraum__ von $V$, __der $S$ als Teilmenge enthaelt__. Beweis: Sei $U$
der kleinste Untervektorraum, der $S$ enthaelt. Wir zeigen: $\langle S \rangle =
U$. $U$ enthaelt $S$, ist also in der Menge $M$ aller Untervektorraeume, die
geschnitten werden. Da dieser Raum der kleinste ist, koennen ihm keine weiteren
Elemente mehr weggeschnitten werden. Der Schnitt erhaelt diesen kleinsten Raum
also, sodass $\langle S \rangle = U$. Jeder Untervektorraum von $V$, der $S$
enthaelt, enthaelt also auch $\langle S \rangle$. Denn jeder Untervektorraum,
der $S$ enthaelt muss bezueglich seinen Elementen abgeschlossen sein. $\langle S
\rangle$ ist dies. Alle maechtigeren Mengen muessen dies auch sein, muessen
$\langle S \rangle$ also enthalten.

#### Beispiel

Sei $v \in V$, dann ist $\langle v \rangle = K \cdot v = \{av \,|\, a \in
K\}$. Die Begruendung ist, dass wenn $\langle v \rangle$ Untervektorraum (also
abgeschlossen) ist, auch jedes skalare Vielfache von jedem Element (also $v$)
enthalten sein muss. $K \cdot v$ ist also notwendigerweise in jedem anderen
Untervektorraum enthalten, der $v$ enthaelt.

#### Satz

Sei $V$ ein $K$-Vektorraum, $U_1, U_2$ Untervektorraeume von $V$. Sei $S = U_1
\cup U_2$, dann ist $\langle S \rangle = U_1 + U_2$. Die Summe von zwei
Untervektorraeume ist also der kleinste Untervektorraum, der die Vereinigung der
beiden Untervektorraeume enthaelt.

#### Beweis

Jedes $v \in U_1$ bzw. $w \in U_2$ liegt sicher in $U_1 + U_2$, wenn man zu
jeweils $v$ oder $w$ die Null addiert. Daher ist $U_1 + U_2$ sicher schon einmal
einer der Untervektorraeume, die in $\langle S \rangle = \bigcap{U \in M} U$ zum
Schnitt kommen. Es gilt also sicher: $\langle S \rangle \subseteq U_1 +
U_2$. Das folgt, weil aus dem Schneiden von $U_1 + U_2$ mit anderen
Vektorraeumen Elemente hoechstens entnommen werden koennen, sodass $\langle S
\rangle$ also hoechstens weniger Elemente, aber sicher nicht mehr, haben kann.

Umgekehrt sei $U \subseteq V$ ein *beliebiger* Untervektorraum mit $S \subseteq
U$, der also $S$ enthaelt. Fuer $v \in U_1, w \in U_2$ folgt $v + w \in U$, also
$U_1 + U_2 \subseteq U$. D.h. da $U$ ein Untervektorraum ist, muss er bezueglich
der Vektoraddition von Vektoren aus der Vereinigung $U_1 \cup U_2$ abgeschlossen
sein. Er muss also insbesondere mindestens so viele Elemente enthalten, wie $U_1
+ U_2$. Daher ist $U$ also sicher *Uebermenge* von $U_1 + U_2$: $U_1 + U_2
\subseteq U$. Nun haben wir diese Eigenschaft also fuer einen beliebigen
Untervektorraum $U$ gezeigt, der $S$ enthaelt. Nun, wir wissen ja, dass $\langle
S \rangle$ der minimale Unterraum ist, der $S$ enthaelt. Er enthaelt also
insbesondere $S$. Dann muss also nach diesem Teil gelten: $U_1 + U_2 \subseteq
S$.

Jetzt haben wir also $U_1 + U_2 \subseteq S$ *und* $U_1 + U_2 \superseteq
S$. Dann muss also Gleichheit herrschen und es gilt: $U_1 + U_2 = \langle S
\rangle$.

Ein intuitives Beispiel hierfuer ist, dass der kleinste Unterraum, der zwei
Geraden durch den Ursprung enthaelt, also der von diesen beiden Vektoren
aufgespannte Unterraum, die Ebene ist, die durch diese beiden Vektoren erzeugt
wird.

## Zusammenfassung

Hier zusammengefasst die wichtigsten Eigenschaften dieses Kapitels.

### Vektorraume

Sei $V$ eine Menge von Vektoren, dann ist $V$ ein Vektorraum, wenn gilt:

* $\langle V, + \rangle$ ist eine abelsche Gruppe.
* $\langle V, \cdot \rangle$ ist distributiv fuer Skalare.
* $\langle V, \cdot \rangle$ ist kommutativ fuer Skalare.
* $\langle V, \cdot \rangle$ ist assoziativ.
* $\langle V, \cdot \rangle$ hat die $1$ als neutrales Element.

### Untervektorraeume

Sei $U$ ein Untervektorraum von $V$, dann gilt:

* $U \neq \emptyset$
* $U$ ist bezueglich allen Linearkombinationen seiner Elemente abgeschlossen.
* $u + v \in U$ fuer $u,v \in U$
* $s \cdot u \in U$ fuer $s \in K, u \in U$
* $U$ besitzt dasselbe neutrale Element (dieselbe Null) wie $V$.
* Jeder Untervektorraum ist ein Vektorraum.
* Jeder Untervektorraum ist eine Gerade durch den Nullpunkt.
* Ein affiner Untervektorraum $v + U$ mit $v \in V$ ist eine affine Gerade.

Seien nun $U_1, U_2 \subseteq V$ Unterraeume und $M$ ein Menge von
Untervektorraeumen, dann gilt:

* $U_1 \cap U_2$ ist ein Untervektorraum.
* $U_1 + U_2$ ist ein Untervektorraum.
* $\bigcap_{U \in M} U$ ist ein Untervektorraum.

### Erzeugnis

* $\langle U \rangle$ ist der minimale Unterraum, der $U$ enthaelt.
* $\langle U \rangle$ ist der Schnitt aller Unterraeume, die $U$ enthalten.
* $\langle U_1 \cup U_2 \rangle = U_1 + U_2$
