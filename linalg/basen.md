# Erzeugendensysteme und Basen

Sei $S \subset V$. $S$ heisst *Erzeugendensystem* von $V$, wenn der von $S$
erzeugte Untervektorraum (bzw. die Menge aller linearen Kombinationen, bzw. der
minimale Unterraum, der $S$ enthaelt, bzw. der Schnitt aller Unterraeume, die
$S$ enthalten) $\langle S \rangle$ genau gleich $V$. D.h. $V$ ist der von $S$
aufgespannte oder erzeugte Unterraum. Ist $S$ ein linear unabhaengiges
Erzeugendensystem, so nennt man $S$ *Basis* von $V$.

Beispiele:

* $(1, 0, 0)^\top, (0, 1, 0)^\top, (0, 0, 1)^\top$ sind eine Basis des $K^3$
* $(1, 1, 0)^\top, (0, 1, 0)^\top, (0, 0, -1)^\top$ sind *auch* eine Basis des
  $K^3$ (es kann mehr als eine geben)

Allgemein ist $S = \{e_1,...,e_n\}$ mit $e_i = (0, ..., 1, ..., 0)^\top$ mit
einer $1$ in der $i$-ten Position eine Basis des $K^n$. Diese Basis nennt man
dann *Standardbasis*. Die Matrix mit den Vektoren aus $S$ ist also die
Einheitsmatrix.

Eine Moeglichkeit, schnell zu sehen, ob ein Vektor oder sogar alle Vektoren
einer Menge linear unabhaengig von den anderen sind, ist zu pruefen, ob ein
Vektor eine *eigene* Komponente. Das bedeutet, dass dieser Vektor als einziger
von allen einen Eintrag ungleich Null in dieser Zeile hat, und daher nicht als
Linearkombination der anderen dargestellt werden kann. Zum Beispiel in:

$$
\begin{bmatrix}
1 & 0 & 0
0 & 1 & 1
0 & 1 & 0
\end{bmatrix}
$$

Muessen der erste und zweite Vektor notwendigerweise linear unabhaengig sein,
weil sie in einer Zeile den einzige Eintrag ungleich Null haben. Das bedeutet,
dass man keine Koeffizienten finden kann, um diesen Eintrag aus den anderen zu
bilden.

Als Konvention hat der Nullraum $V = \{0\}$ im Uebrigen die leere Menge $S =
\emptyset$ als Basis.

## Charakterisierung von Basen

Es gibt mehrere Moeglichkeiten, eine Basis zu beschreiben. Fuer eine Teilmenge
$S \subeteq V$ sind naemlich aequivalent:

1. $S$ ist eine __Basis__ von $V$.

2. $S$ ist eine __inklusionsmaximale linear unabhaengige Menge__ ist. Aus der
   Inklusionsmaximalitaet folgt dass wenn man irgendein weiteres $v \in
   V\setminus S$ hinzufuegt, $S$ linear abhaengig werden wuerde.

3. $S$ ist ein __inklusionsminimales Erzeugendensystem__ von $V$. D.h. dass
   $\forall v \in S: S\setminus\{v\} \neq V$. Man kann also keinen Vektor aus
   dieser Menge entnehmen, sodass die resultierende Menge noch immer
   Erzeugendensystem von $V$ waere.

### Beweise

(1) $\Rightarrow$ (2):

Sei $S$ eine Basis von $V$, also insbesondere linear unabhaengig, dann gilt es
nur die Inklusionmaximalitaet zu zeigen. Sei $v \in V\setminus S$. Da $S$ ein
Erzeugendensystem (und sogar eine Basis) ist, existieren $v_1,...,v_n \in S$ und
$a_1,..,a_n \in K$ mit $v = \sum_{i=1}^n a_i v_i$. D.h. wir koennen diesen einen
Vektor $v$ als Linearkombination der anderen bilden. Wuerden wir jetzt $v$ in
$S$ dazugeben, so waere $S$ linear abhaengig und daher keine Basis mehr. $S$
muss also inklusionsmaximal sein.

(2) $\Rightarrow$ (3):

Sei $S$ inklusionsmaximal und linear unbhaengig. Wir zeigen zuerst, dass $S$ ein
Erzeugendensystem ist. Sei $v \in V$. Falls $v \in S$, so gilt $v \in \langle S
\rangle$. Sei nun also $v \notin S$. Da $S$ inklusionsmaximal ist, wird $S$
linear abhaengig wenn wir $v$ zu $S$ hinzugeben. Das bedeutet, dass wir $v$ also
als Linearkombination der anderen Vektoren $\in S$ darstellen koennen. Da $v$
ein beliebieger Vektor $\in S$ (bzw. $\notin S$) war, koennen wir also
anscheinend den ganzen Vektorraum $V$ erzeugen. Somit ist $\langle S \rangle =
V$ und $S$ also ein Erzeugendensystem.

Wir wissen nun schon, dass $S$ ein Erzeugendensystem ist. Es bleibt also zu
zeigen, dass $S$ inklusionsminimal ist. D.h., dass wir keinen weiteren Vektor
aus $S$ entfernen koennen, sodass $S$ noch immer ein Erzeugendensystem waere.
Sei also nun $v \in S$ ein Vektor. Da $S$ linear unabhaengig ist, koennen wir
ihn nicht durch die uebrigen Vektoren in $S$ erzeugen. Somit waere $S \setminus
\{v\}$ also folglich kein Erzeugendensystem mehr. Damit muss $S$
inklusionsminimal gewesen sein.

Beweis von (3) $\Rightarrow$ (1):

Sei $S$ ein inklusionsminimales Erzeugendensystem. Es gilt zu zeigen, dass $S$
linear unabhaengig ist, dann haetten wir ein linear unabhaengiges
Erzeugendensystem, also eine Basis. Inklusionsminimal bedeutet, dass wir keinen
Vektor aus $S$ entnehmen koennen, sodass $S$ noch immer ein Erzeugendensystem
waere. Betrachten wir also einen beliebigen Vektor $v \in S$. Wenn wir ihn
entnehmen, ist $S \setminus \{v\}$ kein Erzeugendensystem mehr, d.h. wir koennen
$v$ (und womoeglich noch andere Vektoren, aber insbesondere $v$) nicht durch die
uebrigen Vektore in $S$ als Linearkombination ausdruecken. Da $v$ beliebig war,
heisst das also, dass wir keinen Vektor $v \in S$ durch die anderen ausdruecken
koennen. Somit ist $S$ linear unabhaengig. Wir haben also ein linear
unabhaengiges Erzeugendensystem, also eine Basis.

Klar ist mit diesen Charakterisierungen (insbesondere (c)), dass jedes $V$ mit
endlichen Erzeugendensystem auch eine Basis hat. Dazu waehlt man einfach ein
minimales (endliches) Erzeugendsystem. Fuer $V$ mit unendlichen
Erzeugendensystem gilt dieses Argument nicht, aber die Aussage (die drei Punkte)
schon.

Proposition: Jeder Vektorraum hat eine Basis. Beweis waere uber das Zorn'sche
Lemma.

Beispiel: Sei $M$ eine unendliche Menge und $V = \text{Abb}(M, K)$. Das ist ein
Vektorraum, also existiert eine Basis von $V$. Fuer $V$ ist aber keine Basis
bekannt.

## Eigenschaften von Basen

### Satz

*Lemma*: Sei $E \subseteq V$ ein endliches *Erzeugendensystem* und $U \subseteq
V$ eine *linear unabhaengige Menge*. Dann gilt $|E| \geq |U|$. Ein
Erzeugendenystem hat also immer mindestens so viele Elemente wie jede linear
unabhaengige Menge.

Beweis:

$E\setminus U$ ist endlich, wir koennen also die Aussage durch Induktion nach
$|E \setminus U|$ zeigen. Wir muessen zeigen, dass $|E \setminus U|$ nie negativ
wird.

Sei $E = \{v_1,...,v_n\}$ ein Erzeugendensystem mit paarweise verschiedenen
Vektoren und sei $v \in U \setminus E$. Weil $E$ ein Erzeugendensystem ist,
koenenn wir $v$ also als Linearkombination von Vektoren aus $E$ darstellen. Weil
$v \notin E$, muss dann aber zumindest ein Vektor aus $E$ in dieser
Linearkombination nicht auch in $U$ sein. Denn wenn alle Vektoren in dieser
Linearkombination auch in $U$ waeren, dann waere $U$ ja garnicht linear
unabhaengig. Im Weiteren muss der Koeffzient fuer diesen Vektor, der nicht in
$U$ ist, in der Linearkombination nicht Null sein, denn sonst zaehlt er ja gar
nicht (bzw. die anderen Vektoren, welche also schon in $U$ waeren, wuerden $v$
trotzdem bilden)

Sei o.B.d.A. sei $v_1 \in E\setminus U$ dieser Vektor und sein Koeffizient $a_1
\neq 0$. Wenn wir $v_1$ durch $v$ ersetzen, erhalten wir $E' = \{v, v_2, ...,
v_n\}$ und es ergibt sich $v_1 = a_1^{-1} - \sum_{i=2}^n a_1^{-1} a_i v_i \in
\langle E' \rangle$ (wir ziehen von $v$ alles was nicht $v_1$ ist wieder
ab). $E'$ ist also noch immer ein Erzeugendensystem. Nach Definition von $E'$
gilt $|E'\setminus U| = |E \setminus U| - 1$ (denn $v$ alleine ist ja kein
eigenstaendiger Vektor). Induktion liefert, dass $|E \setminus U|$ nie negativ
wird, also $U \leq |E'| = E$.

### Satz

Falls $V$ ein endliches Erzeugendensystem hat, so sind alle Basen von $V$
endlich und haben gleich viele Elemente.

Beweis:

Seien $B_1, B_2$ Basen von $V$. Als Basen sind $B_1, B_2$ also insbesondere
linear unabhaengig und auch Erzeugendensysteme. Dann stecken wir $B_1$ in die
Rolle der linear unabhaengigen Menge und $B_2$ in die Rolle des
Erzeugendensystems. Da wir nun wissen, dass jedes Erzeugendensystem mindestens
so viele Elemente haben muss, wie jede linear unabhaengige Menge, muss also
$|B_1| \leq |B_2|$. Dann vertauschen wir die Rollen ($B_1$ wird das
Erzeugendensystem und $B_2$ die linear unabhaengige Menge) und es folgt, dass
$|B_2| \leq |B_1|$. Dann muss also $|B_1| = |B_2|$ gelten.

### Dimension

Sei $V$ ein Vektorraum mit endlichem Erzeugendensystem. Die *Dimension*
$\text{dim}(V)$ von $V$ ist *die Elementzahl jeder Basis* von $V$ (bzw. einer
Basis, aber alle Basen haben ja gleich viele Elemente). Falls der Vektorraum $V$
kein endliches *Erzeugendensystem* (nicht Raum) hat, dann sagen wir, dass die
Dimension unendlich ist. Wir unterscheiden diese Begriffe durch die Sprechweise:
"endlich dimensional" bzw. "unendlich dimensional".

Beispiele:

* Der $K^n$ hat Dimension $n$ (Standardbasis).

* Die Dimension des Loesungsraums eines homogenen LGS $Ax = 0$ entspricht der
  Anzahl der Freiheitsgrade. Denn fuer jeden Freiheitsgrad bekommen wir eine
  freie Variable und damit einen weiteren Vektor in der Linearkombination des
  Erzeugendensystems. Sei also $Ax = 0$ ein homogenes LGS mit $A \in K^{m \times
  n}$. Dann gilt fuer die Loesungsmenge $\mathbb{L}$: $\text{dim}(\mathbb{L}) =
  n - \text{rang}(A)$.

## Bestimmung von Basen

Es gibt nun einen einfachen Algorithmus, um eine Basis aus einem
Erzeugendensystem zu bilden.

Input: Ein Erzeugendensystem $\langle U \rangle \subseteq K^n$ mit erzeugenden
       Vektoren $v_1, ..., v_m$.
Output: Basis fuer $\langle U \rangle$.

1. Wir bilden die Matrix $A \in K^{m \times n}$ mit den $v_i$ (transponiert) als
   Zeilen (!).

2. Bringen $A$ auf Zeilenstufenform (strenge nicht noetig).

3. Waehle dann die Zeilen, die in dieser Form nicht nur Nullen enthalten. Diese
   sind eine Basis.

$U$ wird von den Zeilen der Zeilenstufenform erzeugt, also insbesondere von den
Zeilen $\neq 0$. Diese erkennt man durch die Zeilenstufenform sofort als linear
unabhaengig. Daraus folgt dann auch dass $\text{dim}(A) = \text{rang}(A)$,
d.h. der Rang einer Matrix $A \in K^{m \times n}$ ist die Dimension des von den
Zeilen aufgespannten Unterraums vo $K^{1 \times n}$.

Wieso funktioniert dieser Algorithmus? Die Zeilen die Null sind, haben wir durch
elementare Zeilenoperationen offensichtlich als Linearkombination (Addition
skalarer Vielfacher) anderer Zeilen ausdruecken koennen. Jene die verbleiben
(also nicht Null sind) sind dann die Menge der linear unabhaengigen Vektoren,
die wir nicht als durch elementare Zeilenoperationen als Linearkombination
anderer Vektoren ausdruecken konnten.

## Zusammenhang von Basen und der Dimension

Seien $v_1,...,v_n \in V$ (paarweise verschieden) und $S = \{v_1,...,v_n\}$.

### Satz

(a) $S$ ist eine Basis von $V$
	$\Leftrightarrow$
(b) $\text{dim}(V) = n$ und $S$ ist linear unabhaengig
	$\Leftrightarrow$
(c) $\text{dim}(V) = n$ und $V = \langle S \rangle$.

In Worten: $S$ ist eine Basis von $V$ genau dann, wenn die Dimension des
Raumes gleich der Kardinalitaet von $S$ ist, und wenn $S$ linear unabhaengig
ist. Das ist weiters genau dann der Fall, wenn die Dimension des Raumes $n$
ist und $S$ den Raum $V$ aufspannt.

(a) $\Rightarrow$ (b, c)

Falls $S$ eine Basis ist, so ist $\text{dim}(V) = n = |S|$ und $S$ linear
unabhaengig (nach Definition der Basis und der Dimension). Es folgt also sicher
(a) $\Rightarrow$ (b). Da zusaetzlich jede Basis auch ein Erzeugendensystem ist,
gilt auch $V = \langle S \rangle$. Somit folgt auch (a) $\Rightarrow$ (c).

(b) $\Rightarrow$ (a)

Wir wissen also, dass $S$ linear unabhaengig ist. Um zu zeigen, dass $S$ auch
eine Basis ist, muesste man zeigen, dass $S$ eine *inklusionsmaximale* linear
unabhaengige Teilmenge ist. Also dass man kein Element zu $S$ hinzugeben kann,
sodass es noch immer linear unabhaengig ist.

Wir wissen $S$ hat $n$ Elemente, ebenso wie alle Basen des Raums. Nun nehmen
wir an, wir koennten noch ein $n + 1$-tes Element zu $S$ hinzugeben. Dann
haetten wir aber eine linear unabhaengige Menge mit mehr Elementen als eine
Basis, also ein Erzeugendensystem! Wiederspruch. $S$ ist also inklusionsmaximal
und somit eine Basis.

(c) $\Rightarrow$ (b)

Wir wissen, dass $S$ ein Erzeugendensystem von $V$ ist und auch, dass $S$ genau
so viele Elemente hat, wie die Dimension des Raumes. D.h. also, dass $S$ eine
Basis ist, denn es ist ja ein Erzeugendensystem mit so vielen Elementen, wie
jede Basis hat. Wenn $S$ eine Basis ist, folgt natuerlich sofort, dass $S$
linear unabhaengig ist.

### Satz

Falls $n < \text{dim}(V)$, so gilt $V \neq \langle S \rangle$.

Begruendung: Wenn $n < \text{dim}(V)$, so gibt es eine linear unabhaengige Menge
$U \subseteq V$ (z.B. eine Basis) mit $|S| < |U|$. Wir wissen aber, dass ein
Erzeugendensystem immer mindestens so viele Elemente wie jede linear
unabhaengige Menge haben muss. Daher kann $S$ auch kein Erzeugendensystem sein.

### Satz

Falls $n > \text{dim}(V)$, so ist $S$ linear abhaengig.  Begruendung: Falls $n >
\text{dim}(V)$, so gibt es eine Basis $B \superseteq V$ mit $|B| < |S|$. Wenn
eine Menge von Vektoren mehr Elemente hat als eine Basis, so muss diese Menge
linear abhaengig sein. Denn, wie oben: Jedes Erzeugendensystem hat immer
mindestens so viele Elemente wie jede linear unabhaengige Menge von Vektoren,
also kann es keine linear unabhaengige Menge mit mehr Elementen als eine
(bzw. jede) Basis geben.

## Basisergaenzung

Sei $V$ ein endlich-dimensionaler Vektorraum und $S \subseteq V$ linear
unabhaengig. Dann existiert eine Basis $B$ dieses Vektorraums mit $S \subseteq
B$. Dann heisst $B$ *Basisergaenzung* von $S$. Das gilt auch fuer unendliche
Dimensionen.

Beweis: Jede linear unabhaengige Menge in $V$ hat hoechstens $\text{dim}(V)$
Elemente. Aus allen linear unabhaengigen Mengen $M$ mit $S \subseteq M$ waehlen
wir eine aus, die maximal viele Elemente hat. Das ist dann eine maximal linear
unabhaengige Menge, also eine Basis. Finden wir keine solche Menge $M$, die $S$
enthaelt, so ist $S$ selbst schon eine inklusionsmaximale linear unabhaengige
Menge, also eine Basis.

Ausgehend von einer linear unabhaengigen Menge $S$ kann man so eine
Basisergaenzung durchfuehren, indem man "greedy" immer weitere linear
unabhaengige Vektoren der Menge zu den bisherigen in $S$ gibt.

Beispiel:

$v_1 = (1, 1)^\top \in \mathbb{R}^2$ ist als einzelner Vektor linear
unabhaengig. $v_1$ laesst sich nub durch jeden zweiten Vektor, der nicht
kollinear zu $v_1$ ist, zu einer Basis ergaenzen. Sprich: wir finden einen
Vektor $v_2$, der nicht linear abhaengig von $v_1$ ist (also nicht auf der
selben Gerade liegt), und bilden die Menge $\{v_1, v_2\}$. Da die Kardinalitaet
einer Basis natuerlich durch die Dimension des Raumes beschraenkt ist, muss
$\{v_1, v_2\}$ auch schon eine Basis sein.

### Allgemeine Moeglichkeit fuer Basisergaenzung

Seien $S = \{v_1, ..., v_s\} \in K^n$ mit $s < n$ linear unabhaengig.

Dann ergaenzen wir $S$ einfach um die Identitaetsmatrix $I_n$ und bilden so die
Matrix $A = (S | I_n)$. Die Spalten dieser Matrix $A$ sind ein
Erzeugendensystem, denn die Spalten von $I_n$ sind ja bereits die
Standardbasis. Nun haben wir also ein Erzeugendensystem, dass aber nicht
unbedingt linear unabhaengig bzw. eine Basis ist. Das Ziel ist es also, von den
Vektoren der Identitaetsmatrix ein paar wegzunehmen, sodass die verbleibende
Menge linear unabhaengig ist. Wir ergaenzen also unsere linear unabhaengige
Menge $S$ um ein paar Standardvektoren.

Waehle also eine Teilmenge von $n - s$ Vektoren (hier ist mit $n$ die Dimension
des Raumes gemeint) von $I_n$ aus. Diese Vektoren muessen linear unabhaengig
sein zu $v_1, ..., v_s$ (wegen der Einheitsmatrix ja einfach). Wir nehmen die
anderen $n$ Vektoren der Einheitsmatrix weg, da wir sie ja als Linearkombination
durch die anderen Vektoren ausdruecken koennen.

Hierfuer koennen wir wieder die Zeilenstufenform mit $A^T$ bilden und
die Eintraege nicht null nehmen. Man muss hierbei jedoch darauf achten, Zeilen
nicht zu vertauschen, damit man nicht Nullen in den $v_1$ bis $v_s$
"weg-kombiniert". Man will nur aus der Identitaetsmatrix Vektoren (in der
Zeilenstufenform dann Zeilen) wegnehmen.

### Beobachtung fuer endliche Dimensionen

Nun noch zwei Beobachtungen fuer endlich-dimensionale Raeume:

Sei $U \subseteq V$ ein Untervektorraum des Vektorraumes $V$. Dann gilt sicher:

1. $\text{dim}(U) \leq \text{dim}(V)$
2. $U \neq V \Rightarrow \text{dim}(U) < \text{dim}(V)$ oder aequivalent: Wenn
   die Dimension eines Untervektorraumes $U$ gleich der Dimension des
   urspruenglichen Raumes $V$ ist, dann muss $U$ sogar gleich $V$ gewesen sein
   (gilt fuer endliche Raeume).

#### Beweis

Jede linear unabhaengige Untermenge $S$ von $U$ kann zu einer Basis $B$ von
*$V$* ergaenzt werden. Dann gilt also immer $|S| \leq |B| = \text{dim}(V)$. Die
Basis von $U$ waere natuerlich auch so eine linear unabhaengige Menge, also
koennte sie entsprechend auch auf *maximal $\text{dim}(V)$* viele Elemente
erweitert werden.

Anders: $U$ kann keine Basis mit mehr Vektoren benoetigen als $V$, denn die
Basis von $V$ mit $\dim(V)$ Vektoren erzeugt schon den ganzen Raum $V$, somit
auch jeden Unterraum $U$. Wenn $n$ Basivektoren also fuer $V$ genuegen, so
muessen sie auch fuer $U$ genuegen. $U$ kann also hoechstens weniger
Basisvektoren benoetigen. Diese findet man eben z.B., indem man einen einzigen
Vektor aus $U$ nimmt und ihn vor eine Einheitsmatrix klebt und dann
Zeilenoperationen durchfuehrt, um den Vektor zu ergaenzen (sieh Algorithmus
oben).

Es gibt also eine Basis $S$ von $U$ mit dieser Eigenschaft, dass $|S| \leq
\text{dim}(V)$ (weil wir sie zu einer Basis von $V$ ergaenzen koennen). Wenn
jetzt also gilt, dass $|S| = \text{dim}(U) = \text{dim}(V)$, dann muss also $|S|
= |B|$ (wo $B$ eben die ergaenzte Basis ist) und dann koennen sowohl $S$ als
auch $B$ denselben Raum $U$ bzw. $V$ erzeugen. Es wuerde also gelten:

$$U = \langle S \rangle = \langle B \rangle = V$$

Anders: wenn wir eine inklusionmaximale linear unabhaengige Teilmenge aus $U$
haben, dann sind diese Vektoren ja auch in $V$. Dann ist diese Teilmenge also
auch in $V$ inklusionsmaximal und linear unabhaengig. Nach Defintion von Basen
muss das also eine Basis sein, fuer sowohl $U$ als auch $V$.

## Zusammenfassung

Hier die Zusammenfassung der wichtigsten Eigenschaften dieses Kapitels. Sei
hierzu $E$ ein Erzeugendensystem und $B$ eine Basis, sowie $V$ der entsprechende
Vektorraum. Dann gilt:

* $E$ ist Erzeugendensystem, wenn $\langle E \rangle = V$
* $B$ ist Basis, wenn $B$ linear unabhaengig und Erzeugendensystem
* Ist $B$ Basis und $v \in V$, gilt: $v = \sum_i a_i b_i$ fuer geeignete $a_i$
* Es sind aequivalent:
  1. $B$ ist Basis.
  2. $B$ ist inklusionsmaximal und linear unabhaengig.
  3. $B$ ist inklusionsminal und Erzeugendensystem.
* $|B| = \dim(V)$
* Sei $U$ eine linear unabhaengige Menge, dann gilt: $|E| \geq |U|$
* Sei $U$ eine linear unabhaengige Menge mit $|U| = \dim(V)$, dann ist $U$ Basis
  von $V$.
* Sei $U$ Unterraum von $V$, dann gilt:
  1. $\dim(U) \leq \dim(V)$
  2. $\dim(U) = \dim(V) \iff U = V$
* Alle Basen haben gleich viele Elemente, naemlich $\dim(V)$
* $\dim(K^n) = n$
* Sei

* Bilde aus $E$ eine Basis durch elementare Zeilenoperationen mit $E$ in den
  Zeilen einer Matrix.
* Sei $S$ eine linear unabhaengige Menge, dann ergaenze sie durch $(S | I_N)$ in
  Zeilenstufenform zu einer Basis.
