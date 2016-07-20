# Linearkombinationen

Seien $v_1,...,v_n$ Vektoren $\in V$, dann nennen wir

$k_1v_1 + ... + k_nv_n = \sum_{i=1}^n k_iv_i$ mit $k_i \in K$

eine *Linearkombination*. Es gibt natuerlich auch unendliche
Linearkombinationen, wobei wir im Weiteren aber implizit immer mit endlichen
Kombinationen arbeiten werden. Fuer Vektoren $u, v, w$ sind beispielsweise

* $5u + 3v - 7w$, und
* $7/2u - v + 17w$

Linearkombinationen. Betrachten wir eine Linearkombination, die die Null ergibt,
also wo gilt:

$$\sum_{i=1}^n k_iv_i = 0$$

Waehlen wir fuer alle Koeffizienten $k_i$ hier die Null, so sprechen wir von der
*trivialen Darstellung der Null*. Sind die Koeffizienten nicht alle Null, das
Ergebnis der Linearkombination aber dennoch gleich $0$, dann sprechen wir von
einer *nicht-trivialen* Darstellung der Null. Wir fragen uns nun, fuer welche
Vektoren man eine eine triviale und fuer welche man auch nicht-triviale
Darstellungen bilden kann.

Seien $v_1,...,v_n \in V$ Vektoren. Dann nennt mant diese *linear unabhaengig*,
wenn $k_1v_1 + ... + k_nv_n = 0 \Rightarrow k_1 = ... k_n = 0$, wenn es also nur
die triviale Darstellung der Null gibt (wenn die Linearkombination gleich Null
ist, impliziert dass, dass die Koeffizienten alle Null gewesen sein
muessen). Gilt dies nicht, kann man also zumindest einer der Koeffizienten
$k_1,...,k_n$ ungleich null waehlen, so heissen die Vektoren konvers *linear
abhaengig*. Das bedeutet dann, dass es eine nicht-triviale Darstellung der Null
fuer diese Vektoren gibt bzw. auch, __dass einer der Vektoren als lineare
Kombination der anderen ausgedrueckt werden kann.__

Nehmen wir zur Veranschaulichung an, dass $k_i$ (belebig gewaehlt) ungleich null
ist. Damit $\sum_i k_i v_i$ dennoch gleich null ist, muss es eine Menge von
Koeffizienten $k_a, ..., k_b$ geben, sodass diese zusammen mit ihren Vektoren
den Koeffizienten $k_i$ ausgleichen, sprich: $\sum_{j=a}^b k_j v_j = -k_iv_i$
wobei kein $v_j = v_i$. Dann laesst sich $v_i$ also als Linearkombination dieser
Vektoren ausdruecken, sodass die ganze Menge nicht linear unabhaengig ist.

Zuvor hatten wir eine lineare Abhaengigkeit zwischen Vektoren $v_1,...,v_n$
durch eine nicht-triviale Darstellung der Null beschrieben, wobei es also
Koeffizienten $k_1,...,k_n$ gab sodass $(k_1,...,k_n) \neq (0,...,0)$ und
dennoch $\sum_{i=0}^n k_1v_1 = 0$. Jetzt koennen wir eine lineare Abhaengigkeit
eben noch dadurch ausdruecken, indem wir zeigen, dass sich zumindest einer der
Vektoren in der betrachteten Menge von Vektoren als Linearkombination
ausdruecken laesst (auch wenn manche davon dann Null als Koeffizient haetten).

Eine Menge $v_1, ..., v_n$ von Vektoren ist also linear abhaengig, wenn gilt:

$$\exists i \leq n: v_i = \sum_{j \neq i} h_i v_j.$$

Hierfuer nehmen wir wieder an, dass $k_1v_1 + ... + k_nv_n = 0$ ist und $k_i
\neq 0$. Dann koennen wir alle Terme ausser den $i$-ten auf die andere Seite
bringen: $k_iv_i = -k_1v_1 - k_2v_2 - ... - k_nv_n$, sowie dann (unter der
Annahme, dass $k_i$ eben ungleich null) durch $k_i$ dividieren: $v_i = h_1v_1 +
... h_2v_2$. Hierbei definieren wir $h_j := -\frac{k_j}{k_i}$.

Beispiele:

1. Ein einzelner Vektor $v$ ist linear unabhaengig genau dann, wenn er nicht der
   Nullvektor ist. Denn sonst gaebe es fuer alle Koeffizienten $k$ eine
   nicht-triviale Darstellung der Null (und somit eine lineare Abhaengigkeit).

2. Zwei Vektoren $u,v$ sind dann linear abhaengig, wenn sie skalare Vielfache
   voneinander sind.

3. $\{(1, 2)^\top, (1, 1)^\top, (7, 7)^\top\}$ ist linear abhaengig, denn
   z.B. $v_2 = 1/7 \cdot v_3 + 0 \cdot v_1$.

### Koeffizientenregel

Seien $v_1,...,v_n$ linear unabhaengige Vektoren. Dann sind ihere Koeffizienten
fuer eine konkrete rechte Seite einer Gleichung eindeutig bestimmt, also:

$k_1v_1 + ... k_nv_n = h_1v_1 + ... h_nv_n \Rightarrow k_i = h_i \forall i \leq
n$.

Beweis:

1. $\sum_{i=1}^n k_iv_i = \sum_{i=1}^n h_iv_i$
2. $\sum_{i=1}^n (k_i - h_i) v_i = 0$
3. Da $v_1,...,v_n$ linear unabhaengig sind, muessen nun $(k_i - h_i) = 0$
   bzw. $k_i = h_i$ sein. Sonst gaebe es eine nicht-triviale Darstellung der
   Null mit Koeffzient $c_i = k_i - h_i$.

## Erzeugnis

Sei $S \subseteq V$. Dann ist $\langle S \rangle$ der von $S$ erzeugte
Untervektorraum, also der inklusionsminimalste Untervektorraum von $V$, der $S$
enthaelt. Wir zeigen, dass $\langle S \rangle$ genau die Menge aller
Linearkombinationen von $S$ ist, d.h. $\langle S \rangle = \{v \in V \,|\, v
\text{ ist Linearkombination von } S\}$ (die Menge aller Vektoren die man als
Linearkombination der Vektoren in $S$ darstellen kann). Fuer $v_1,...,v_n \in V$
bzw. $S = \{v_1,..,v_n\}$ hat man dann:

$$\langle v_1, ..., v_n \rangle = \left\{\sum_{i=1}^n a_iv_i \,|\, a_i \in
K\right\}$$.

Beweis: Sei $W \subseteq V$ die Menge aller Linearkombinationen von $S$. Es gilt
$0 \in W$ und zudem sind die Summe und das skalare Vielfache von
Linearkombinationen wieder Linearkombinationen. Daher ist nach Definition eines
Untervektorraums $W$ also ein Untervektorraum von $V$ (komprimiert kann man die
Vektorraumeigenschaft ja so ausdruecken, dass der Untervektorraum bezueglich
allen Linearkombinationen abgeschlossen sein muss). Da $\langle S \rangle$ der
Schnitt aller Untervektorraeume ist, die $S$ enthalten, und $W$ den Raum $S$
sicher enthaelt (einfache Linearkombination mit nur einem Koeffizient 1), gilt
also $\langle S \rangle \subseteq W$.

Umgekehrt sei $U \subseteq V$ ein *beliebiger* Unterraum mit $S \subseteq
U$. Fuer $v_1,...,v_n \in S$ und $a_1,...,a_n \in K$ sind dann alle $v_i \in U$,
und nach den Eigenschaften eines Unterraums auch alle Linearkombinationen
$\sum_{i=1}^n a_iv_i$ in diesem Unterraum $U$. Daher ist $W$ (die Menge aller
Linearkombinationen von $S$) sicher Untermenge von $U$. Nun haben wir diese
Eigenschaft also fuer einen *beliebigen* Unterraum gezeigt, der $S$
enthaelt. Nun, $\langle S \rangle$ ist auch so ein Unterraum, also muss $\langle
S \rangle \supersetq W$ sein.

Jetzt haben wir also sowohl $\langle S \rangle \subseteq W$, wie auch $\langle S
\rangle \superseteq W$. Also muss Gleichheit herrschen und es gilt: $\langle S
\rangle = W$

### Beispiele

Sei $V = \mathbb{R}^3$, dann gilt:

1. Der minimale Untervektorraum $\left\langle \begin{bmatrix}1 \\ 0 \\
   0\end{bmatrix}, \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}\right\rangle$ ist
   nach obigem Satz also gleich der Menge aller Linearkombinationen dieser
   beiden Vektoren $\left\{a_1\begin{bmatrix}1 \\ 0 \\ 0\end{bmatrix},
   \begin{bmatrix}0 \\ 1 \\ 0 \end{bmatrix} \,|\, a_1,a_2 \in \mathbb{R}\right\}
   = \left\{\begin{bmatrix}a_1 \\ a_2 \\ 0\end{bmatrix} \,|\, a_1, a_2 \in
   \mathbb{R}\}\right$.

2. $\left\langle \begin{bmatrix} 1\\ 0 \\ 0 \end{bmatrix}, \begin{bmatrix}0 \\ 1
   \\ 0\end{bmatrix}\right\rangle = \left\langle\begin{bmatrix}2 \\ 0 \\ e
   0\end{bmatrix},\begin{bmatrix}1 \\ 1\\ 0\end{bmatrix}\right\rangle$ (diese
   Vektoren haben denselben minimalen Unterraum).

3. Weil $\begin{bmatrix}1 \\ 1 \\ 0\end{bmatrix}, \begin{bmatrix}-2 \\ -2 \\
   0\end{bmatrix}$ linear abhaengig sind, ist der eine Vektor jeweils im
   minimalen Unterraum vom anderen enthalten. Denn dieser Unterraum ist ja wie
   wir wissen die Menge aller Linearkombinationen. Daher gilt
   $\left\langle\begin{bmatrix}1 \\ 1 \\ 0\end{bmatrix} \right\rangle =
   \left\langle\begin{bmatrix}-2 \\ -2 \\ 0\end{bmatrix} \right\rangle$

4. Sagen wir, wir haben $\mathbb{L} = \{(a, -2a, b, 0)^\top | a,b \in K\}$ als
   Loesungsmenge eines LGS gefunden. Dann ist dieser Vektor $(a, -2a, b,
   0)^\top$ gleichbedeutend einer Summe $a \cdot (1, -2, 0, 0)^\top + b \cdot
   (0, 0, 1, 0)$. $a,b \in K$ sind beliebig gewaehlte Skalare, insofern gibt
   diese Linearkombination an, dass der Loesungsraum durch $\langle (1, -2, 0,
   0)^\top, (0, 0, 1, 0)^\top \rangle$ erzeugt wird.

Wir koennen so auch den gesamten $\mathbb{K}^3$ als Erzeugnis bzw. Menge aller
Linearkombinationen $\langle (1, 0, 0)^\top, (0, 1, 0)^\top, (0, 0, 1)^\top
\rangle$ der drei "Standardvektoren" bzw. "Einheitsvektoren" gesehen werden.

### Satz zu Zeilenoperationen

Satz: Sei $B \in K^{m \times n}$ aus $A \in K^{m \times n}$ durch elementare
Zeilenoperationen hervorgegangen. Dann erzeugen die Zeilen von $A$ denselben
Unterraum von $K^{1 \times n}$ wie die Zeilen von $B$. Das kann man dadurch
verstehen, dass eine Linearkombination fuer weitere Linearkombinationen seiner
Elemente immer abgeschlossen ist. Deswegen wird die Loesungsmenge von linearen
Gleichungssystemen bei elementaren Zeilenoperationen auch nicht veraendert,
weswegen wir diese im Gauss Algorithmus anwenden duerfen.
Beispiel:

Betrachten wir die Zeilenvektoren $x_1, x_2, x_3 \in \mathbb{R}^3$. Hierbei ist
$\langle x_1, x_2, x_3 \rangle = ax_1 + bx_2 + cx_3$ fuer alle $a, b, c
\mathbb{R}$. Betrachten wir nun also die Zeilenoperation, die zur zweiten Zeile
$x_2$, drei mal die erste Zeile $x_1$ dazuaddiert. Dann erhalten wir also:

1. $ax_1 + b(x_2 + 3x_1) + cx_3$
2. $ax_1 + bx_2 + 3bx_1 + cx_3$
3. $(a + 3b)x_1 + bx_2 + cx_3$
4. Sei dann einfach $a' = a + 3b$, dann haben wir:
5. $a'x_1 + bx_2 + cx_3$ fuer alle moeglichen $a', b, c \in \mathbb{R}$

### Test nach Linearer Unabhaengigkeit

Fuer Vektoren $v_1,...,v_n \in K^m$ gibt es einen eindeutigen Test nach linearer
Unabhaengigkeit:

Bilde die Matrix $A$ mit den $v_i$ als Spalten. Dann sind $v_1, ...v,v_n$ genau
dann linear unabhaengig, wenn $\text{rang}(A) = n$. Die $v_i$ sind naemlich
genau dann linear unabhaengig, wenn das homogene LGS $Ax = 0$ als einzige
Loesung den Nullvektor hat. Das ist genau dann der Fall, wenn $\text{rang}(A) =
n$, da ein homogenes LGS, dass eindeutig loesbar ist, nur die triviale Loesung
hat. Wenn die Menge linear abhaengig war, dann ist $\text{rang}(A) < n$. Wenn
wir nur den Nullvektor als eindeutige Loesung haben, bedeutet das insbesondere,
dass es nur die triviale Darstellung dieser Vektoren feur die Null gibt. Wir
wissen dann naemlich, dass nur der Nullvektor $0$ das LGS $Ax = 0$ loest, sodass
also gilt:

$$A0 = 0$$

Diese Multiplikation $A0$ expandiert bekanntlich zu:

$$0v_1 + 0v_2 + ... + 0v_n = 0$$

Wo $v_i$ die Spalten von $A$ sind. Wir haben also genau die Eigenschaft der
linearen Unabhaengigkeit: Es gibt nur eine eindeutige Darstellung der Null,
naemlich die triviale.

Es gilt allgemein fuer ein LGS, dass $\text{rang}(A) \leq \min\{m, n\}$ sein
muss. Daraus folgt, dass es im $K^m$ maximal $m$ linear unabhaengige Vektoren
geben kann. Intuition: Wir bilden die Vektoren $\in K^m$ auf die *Zeilen* eines
LGS ab (transponieren die Matrix mit den Vektoren in den Spalten). Insofern hat
diese Matrix also $m$ Spalten. Wenn wir jetzt mehr als $m$ Vektoren nehmen, dann
haben wir mehr als $m$ Zeilen. Nach Definition ist der Rang einer Matrix durch
die Anzahl an Spalten (und auch Zeilen) beschraenkt, also kann der Rang dieser
Matrix maximal $m$ sein. In der Zeilenstufenform werden somit alle weiteren
Zeilen (Vektoren) $> m$ zu Nullen. Das bedeutet, dass wir sie durch elementare
Zeilenoperationen als lineare Kombination anderer Vektoren dargestellt
haben. Somit erkennt man also, dass man im $K^m$ nie mehr als $m$ linear
unabhaengige Vektoren haben kann.

## Zusammenfassung

Hier die wichtigsten Erkenntnisse dieses Kapitels. Seien hierzu $v_1, ..., v_n
\in V$ Vektoren. Dann gilt:

* Gilt $\sum_i a_i v_i = 0 \Rightarrow a_i = 0$, so sind die $v_i$ linear
  unabhaengig (triviale Darstellung der Null)
* Gilt $\sum_i a_i v_i = 0$ und $\exists a_i \neq 0$, so $\exists v_i: v_i =
  -(\sum_{j \neq i} a_i)$
* Der Nullvektor ist immer linear abhaengig (von sich selbst).
* Die Koeffizienten $a_i$ fuer $v = \sum_i v_i$ sind eindeutig.
* $\langle U \rangle$ ist die Menge aller Linearkombinationen mit $u \in U$.
* Geht $B$ aus $A$ durch elementare Zeilenoperationen hervor, so ist $\langle A
  \rangle = \langle B \rangle$ fuer die Zeilen von $A$ und $B$.
* $v_1, ..., v_n$ sind genau dann linear unabhaengig, wenn $A = (v_1 | ... |
  v_n)$ regulaer ist.
