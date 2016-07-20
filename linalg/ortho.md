# Skalarprodukt und Orthogonalitaet

$\newcommand{\dim}{\mathop{\rm dim}\nolimits}$
$\newcommand{\Kern}{\mathop{\rm Kern}\nolimits}$

In diesem Kapitel betrachten wir das Skalarprodukt und die Eigenschaft der
Orthogonalitaet. Das Skalarprodukt ist hierbei die Summe der Produkte der
einzelnen Komponenten zweier Vektoren. Ist diese Summe gleich Null, so nennt man
die Vektoren *orthogonal*.

## Definition

Wir wollen das Skalarprodukt noch formaler betrachten. Seien $v = (x_1, ...,
x_n)^\top$ und $w = (y_1, ..., y_n)^\top$ zwei Vektoren $\in K^n$. Dann nennt
man

$$\langle v, w \rangle = \sum_{i=1}^n x_iy_i = v^\top w \in K$$

das *Skalarprodukt* von $v$ und $w$. Die Vektoren $v, w$ heissen dann senkrecht
bzw. *orthogonal* zueinander, falls $\langle v, w \rangle = 0$, also falls das
Skalarprodukt null ist.

Eine weitere Definition, die wir betrachten wollen, ist die des *orthogonalen
Komplements*. Fuer einen Unterraum $U \subseteq K^n$ ist das orthogonale
Komplement $U^\perp$ die Menge aller Vektoren im urspruenglichen Raum $K$, die
zu jedem Vektor in $U$ senkrecht stehen:

$$U^\perp = \{v \in K^n \,|\, \langle u, v \rangle = 0 \,\forall u \in U\}.$$

Haben wir einen Unterraum $U$ mit Basismatrix $B$, so finden wir sein orthogonales
Komplement als Loesungsraum des homogenen LGS $(B^\top)v = 0$. $v$ sind dann
also alle jene Vektoren, fuer welche das Skalarprodukt zwischen jedem
Basisvektor (jeder Zeile) Null ergibt. Somit gilt:

$$U^\perp = \Kern(B^\top)$$

Ein interessantes Beispiel eines komplexsertigen Vektors aus $\mathbb{C}^2$ ist
$(1, i)^\top$.  Dieser ist naemlich auf sich selbst orthogonal:

$$(1, i) (1, i)^\top = 1^2 + i^2 = 1 + (-1) = 0.$$

## Eigenschaften des Skalarprodukts

Seien $u, v, w \in K^n$ Vektoren und $a$ ein Skalar $\in K$. Damm gelten
folgende zwei Eigenschaften:

* $\langle u, v + aw \rangle = \langle u,v \rangle + a \cdot \langle u,w
\rangle$
* $\langle u + av, w \rangle = \langle u,w \rangle + a \cdot \langle v,w \rangle$

Diese beiden Eigenschaften nennt man *Bilinearitaet* und folgen aus den
Rechenregeln fuer Matrizen (Distributivgesetz und Kommutativitaet von
Skalaren). Dann noch zwei weitere Eigenschaften:

* $\langle v,w \rangle = \langle w, v \rangle$
* $(K^n)^\perp = \{0\}$

Zum Einen ist das Skalarprodukt also *kommutativ* und zum Anderen gilt, dass das
orthogonale Komplement zu einem vollstaendigen Vektorraum, also wo nach obiger
Notation $U = V$, nur den Nullvektor enthaelt. Denn der Nullvektor ist zu jedem
Vektor orthogonal, da das Skalarprodukt zwischen einem Vektor und der Null immer
die Null ist. In einem vollstaendigen Vektorraum ist es also auch nur dieser
eine Vektor.

Das Skalarprodukt ist also kommutativ. Es ist aber nicht assoziativ! Das folgt
alleine schon daraus, dass das Skalarprodukt ja ein Skalar produziert. Klammert
man also anders, erhaelt man die eine oder andere Skalierung des einen oder
anderen Vektors:

1. $\langle \langle u, v \rangle w \rangle = s_1 \cdot w$
2. $\langle u \langle v, w \rangle \rangle = u \cdot s_2$

## Laenge eines Vektors

Im Weiteren wollen wir nun nur mehr den rellen Raum betrachten, also $K =
\mathbb{R}$. Nun definieren wir den *Betrag* $|v|$ bzw. die (euklidische)
*Laenge* (auch *$L_2$ Norm* genannt) fuer einen Vektor $v \in \mathbb{R}^n$ als
die Wurzel aus der Summe der Quadrate seiner Komponenten. Diese "Summe der
Quadrate" ist aber natuerlich gerade das Skalarprodukt eines Vektors mit sich
selbst, da $\langle v, v \rangle = v^\top v$ immer jede Komponente auf der einen
Seite mit derselben Komponente (des selben Vektors) auf der anderen Seite
multipliziert und diese Werte dann addiert. D.h. wir haben also die folgende
Definition fuer $|v|$ (auch $||v||_2$ als $L_2$ Norm):

$$|v| = \sqrt{\langle v, v \rangle} = \sqrt{v^\top v} = \sqrt{\sum_i v_i^2}$$

Die Berechnung des Betrags bzw. der Laenge eines Vektors laesst sich im Uebrigen
ganz einfach durch den Satz des Pythagoras zeigen. Im zweidimensionalen Raum ist
es besonders anschaulich. Betrachtet man einen Vektor $v$, bzw. genauer die
Laenge $|v|$ des Vektors, als die Hypothenuse des rechtwinkligen Dreiecks, das
zwischen den Komponenten ($x, y$) dieses zwei-dimensionalen Vektors aufgespannt
wird, so hat man ueber den Satz des Pythagoras die Laenge gegeben durch:

$$|v|^2 = x^2 + y^2 \Rightarrow |v| = \sqrt{x^2 + y^2} = \sqrt{v^\top v}$$

Nun ist es so, dass sich der Satz des Pythagoras auch auf hoeherdimensionale
Raeume verallgemeinern laesst. Hierbei gilt dann allgemein fuer die Komponenten
$x_1, ..., x_n$ eines $n$-dimensionalen Vektors $v$, dass das Quadrat der
"Hypothenuse" in diesem Raum gleich der Summe der Quadrate der Komponenten ist
(bzw. der Differenzen der Komponenten wenn man den Vektor zwischen zwei Punkten
betrachtet), dann gilt also:

$$|v|^2 = \sum_i x_i^2 \Rightarrow |v| = \sqrt{\sum_i x_i^2} = \sqrt{v^\top v}$$

Hieraus folgt also die Formel fuer den Betrag.

#### Eigenschaften der Laenge

Wir wollen nun noch einige Eigenschaften der Laenge zweier Vektoren untersuchen.
Zum Einen gilt die beruehmte *Dreiecksungleichung*, welche sagt, dass die Laenge
der Summe von zwei Vektoren kleiner gleich der Summe der Laengen der Vektoren
ist. Also fuer $v, w \in \mathbb{R}^n$:

$$|v + w| \leq |v| + |w|$$

Das laesst sich am Einfachsten einfach so verstehen, dass der direkte Pfad
("Luftlinie") von $A$ nach $B$ ueber Vektoren $v, w$ (also wo $B = A + v + w$)
kuerzer ist als der Weg zuerst nach $A + v$ plus den Weg von $A + v$ nach $B$.

Weiters gilt auch eine Art Multiplikativitaet des Betrages. Sei $a \in K$, dann
steht fest: $|av| = |a||v|$. Der Betrag eines skalierten Vektors ist also gleich
der absoluten Skalierung mal den Betrag des Vektors. Das laesst sich intuitiv so
verstehen, dass die Quadrierung im Betrag ja normalerweise das Vorzeichen von
$a$ bzw. $a$ mal irgendeiner Komponente immer aufheben wuerde. Das stellen wir
nun so her, indem wir nur den Absolutwert von $a$ betrachten. $|v|$ hebt dann
weiterhin alle negativen Vorzeichen von Komponenten in $v$ auf. Formal:

1. $|av| = \sqrt{(av)^\top (av)}$
2. $\sqrt{\sum_i (a v_i)^2} = \sqrt{a^2 \sum_i v_i^2}$
3. $\sqrt{a^2} \cdot \sqrt{\sum_i v_i^2}$
4. $|a| \cdot |v|$

Eine weitere, einfache Eigenschaft ist, dass wenn ein Vektor ungleich Null ist,
sein Betrag auch zwingend groesser Null sein muss. Das folgt wieder daraus, dass
im Betrag, durch die Quadrierung, alle Vorzeichen aufgehoben werden. Dann gilt
ja im Weiteren, dass $\sqrt{v^\top v} = \sqrt{v_1^2 + ... + v_n^2}$ nur dann
Null ist, wenn die Summe der Quadrate Null ist. Und diese Summe von Quadraten
ist dann eben auch nur dann Null, wenn die Komponenten alle einzeln Null
sind. Gilt das nicht, muss der Betrag *positiv* sein.

Die letzte Eigenschaft ist nun die folgende (*Cauchy-Schwarz Ungleichheit*):

$$|\langle v, w \rangle| \leq |v| \cdot |w|$$

Der absolute Wert (keine Laenge!) des Skalarprodukts zweier Vektoren ist also
kleiner gleich dem Produkt der Laengen zweier Vektoren. Das laesst sich durch
die folgenden Schritte veranschaulichen:

1. Links haben wir die Summe der Quadrate der Produkte von jeweils $v_i, w_i$:
   $|v^\top w| = \sqrt{\sum_i (v_iw_i)^2}$

2. Rechts dann:
   $|v||w| = \sqrt{v_1^2 + ... + v_n^2} \cdot \sqrt{w_1^2 + ... + w_n^2}$

3.  Dann koennen wir die Wurzel zusammenziehen:
	$\sqrt{(v_1^2 + ... v_n^2) \cdot (w_1^2 + ... + w_n^2)}$.

4. Nun erhalten wir unter Anderem $v_1$ mal jedes $w_i$, zum Quadrat:
   $(v_1w_1)^2 + (v_1w_2)^2 + ... + (v_1w_n)^2$

5. Rechts haben wir also fuer jedes $v_i$ gleich $n$ solcher Produkte. Links
   hatten wir fuer jedes $v_i$ nur ein einziges Produkt, mit dem
   korrespondierenden $w_i$.

6. Man sieht also, wieso das rechte groesser sein koennte.

Es gibt dann aber einen Fall, wo $|\langle v, w \rangle| = |v||w|$, also wo
Gleichheit gilt. Das ist genau dann der Fall, wenn $v,w$ linear abhaengig
(bzw. positive Vielfache) voneinander sind. Dann gilt naemlich $w = kv$ fuer ein
Skalar $k \in \mathbb{R}$. Dann also:

1. $|\langle v, w \rangle| \leq |v||w|$
2. $|\langle, v, kv \rangle| = |v||kv|$
3. $|k \cdot \langle v, v \rangle| = |v| |k||v|$ (Bilinearitaet)
4. $|k| \cdot |\langle v, v \rangle| = |k||v||v|$ (Eigenschaft des Absolutwerts)
5. $|k||\langle v, v \rangle| = |k||v|^2$
6. $|k| \langle v, v \rangle = |k||v|^2$ (da Quadrate nie negativ)
7. Jetzt haben wir links $|k|$ mal die Summe der Quadrate von $v$ und rechts
   ebenso:
   $|k| \cdot v^\top v = |k| \cdot v^\top v$

Wir kennen im Uebrigen jetzt vier Schreibweisen fuer die Summe der Quadrate der
Komponenten desselben Vektors:

1. $\langle v, v \rangle$
2. $v^\top v$
3. $|v|^2$
4. $\sum_i v_i^2$

## Orthonormalitaet

Eine Menge von Vektoren $S = \{v_1, ..., v_k\} \subset \mathbb{R}^n$ (bzw. eine
Matrix mit solchen Spalten) heisst *Orthonormalsystem*, wenn zwei Eigenschaften
gelten:

1. Jeder Vektor in der Menge ist orthogonal zu jedem anderen Vektor darin.
2. Jeder Vektor hat *Einheitsnorm*, sodass gilt: $|v_i| = 1 \forall i$.

D.h. die Menge ist *orthogonal* (daher der *ortho-* Teil) und *normiert* (daher
der *-normal* Teil). Ein Vektor mit *Einheitsnorm* ist dabei ein Vektor, der in
einem Koordinatensystem nur eine Einheit ("ein Quadrat belegt") lang ist. Hat
man einen Vektor $v$, der nicht in Einheitsnorm ist, so erhaelt man sein
normiertes Gegenstueck $v_0$, indem man den Vektor $v$ durch seinen Betrag
dividiert. Dadurch skaliert man alle Komponenten des Vektors auf $1$ (weil quasi
jede Komponente durch sich selbst dividiert wird). Dieser zeiget eben *genau in
die selbe Richtung*, aber hat Einheitslaenge $1$.

Formal gilt fuer Vektoren $v_i, v_j$ aus dieses System dann, dass das
Skalarprodukt $\langle v_i, v_j \rangle$ gleich dem *Kronecker-Delta*
$\delta_{i,j}$ ist.  Dieses Kronecker-Delta ist genau dann immer $1$, wenn $i =
j$ und sonst immer Null. So waere eine Matrix $D = (\delta_{i,j}) \in
\mathbb{R}^{n \times n}$ gerade die Identitaetsmatrix $I_n$. Denn dieses
Skalarprodukt ist fuer alle $i \neq j$ natuerlich Null, weil die Vektoren
orthogonal zueinander sind. Wenn aber dann $i = j$, so hat man mit $\langle v_i,
v_i \rangle = v_i^\top v_i$ das Quadrat des Vektors. Da die Vektoren
Einheitslaenge haben, und das Quadrat eines Vektors ja nichts anderes ist als
das Quadrat des Betrages, welcher hier ja Eins ist, ist dieses Quadrat dann
immer auch gleich Eins:

$$\langle v_i, v_i \rangle = v_i^\top v_i = |v_i|^2 = 1^2 = 1.$$

Daher ist das Skalarprodukt also gleich dem Kronecker-Delta. Symbolisch ist das
Kronecker-Delta $\delta_{i,j}$ gegeben durch:

$$
\langle v_i, v_j \rangle = \delta_{i,j} =
\begin{cases}
1, \text{ falls } i = j\\
0, \text{ sonst}
\end{cases}.$$

Haben wir nun eine Matrix $A \in \mathbb{R}^{n \times k}$, die ein
Orthonormalsystem $S$ in den Spalten hat, so gilt folglich:

$$S \text{ Orthonormalsystem } \Leftrightarrow A^\top A = I_k$$.

Die Aussage laesst sich am besten so verstehen, dass wir durch $A^\top A = A^2$
im Prinzip jede Spalte in $A$, also jedes $v_i \in S$, mit jedem Vektor in $A$
multiplizieren, wir erhalten also gerade das obige $\langle v_i, v_j \rangle$.
Dann wissen wir ja, dass dieses Skalarprodukt nur dann Eins und nicht Null ist,
wenn $i = j$, also gerade in der Diagonalen. Dadurch erhalten wir die
Einheitsmatrix, wo in den Diagonalen ja nur Einsen sind und ueberall sonst nur
Nullen.

Wenn wir $A^2$ nun nochmals mit $A$ multiplizieren, erhalten wir

$$A^3 = A^2 \cdot A = I_n \cdot A = A.$$

Das setzt sich dann fuer weitere Potenzen periodisch mit $I_n, A, I_n, A, ...$
fort. Somit koennen wir auch feststellen:

$$
A^k =
\begin{cases}
	I_n, \text{ wenn } k \text{ gerade },
	A, \text{ wenn } k \text{ ungerade }
\end{cases}
$$

Ein Beispiel fuer ein Orthonormalsystem ist im Uebrigen die Standardbasis des
$\mathbb{R}^n$. Das ist natuerlich ganz einfach daran erkennbar, dass diese
Matrix mit diesen Vektoren als Spalten ja schon die Einhetismatrix ist. Folglich
gilt auch sicher $A^\top A = I_n^\top I_n = I_n$.

Wir erkennen nun auch schon eine interessante Eigenschaft von orthonormalen
Matrizen: Da $A^\top$ die Matrix ist $B$, sodass $A \cdot B = I_n$, folgt:

$$A^\top A = I_n \Rightarrow A^\top = A^{-1}.$$

Die Inverse einer orthonormalen Matrix ist also gerade die transponierte Matrix.

### Satz

Wir zeigen: Orthonormalsysteme in $\mathbb{R}^n$ sind linear unabhaengig
(Anmerkung: ein *Orthonormalsystem* ist erstmal eine Menge von Vektoren, noch
keine Matrix).

Sei hierfuer $S = \{v_1, ..., v_k\} \subset \mathbb{R}^n$ ein Orthonormalsystem.
Nun waehlen wir passende $a_i$, sodass $a_1v_1 + ... + a_kv_k = 0$. Sind die
Vektoren linear unabhaengig, duerfe es ja nur die triviale Darstellung der Null
geben. Sprich, die $a_i$ muessten dann also Null sein. Das wollen wir
zeigen. Hierfuer betrachten wir nun einfach einen konkreten Koeffizienten $a_j$.
Dann gilt fuer diesen:

$$a_j = \sum_{i=1}^k a_i \langle v_i, v_j \rangle$$

Das folgt daraus, dass wir ja ein Orthonormalsystem haben. D.h. das
Skalarprodukt $\langle v_i, v_j \rangle$ ist gleich dem Kronecker-Delta und
genau dann $1$ (!), wenn $v_i = v_j$ (weil die Vektoren ja normiert sind.
Ansonsten habn wir die Null. D.h. die Summe reduziert sich auf

$$a_j = a_j \langle v_j, v_j \rangle = a_j \cdot |v_j|^2 = a_j \cdot 1$$.

Nun koennen wir umformen:

1. $a_j = \sum_{i=1}^k a_i \langle v_i, v_j \rangle$ (Bilinearitaet)
2. $a_j = \langle v_j, \sum_{i=1}^k a_iv_i \rangle$ (nach Voraussetzung)
3. $a_j = \langle v_j, 0 \rangle$
4. $a_j = 0$

Nun haben wir also fuer alle $j$ eben genau, dass $a_j$ Null sein muss. Also
haben wir nur die triviale Darstellung der Null und die Vektoren muessen linear
unabhaengig sein.

#### Korollar

Sei $U \subseteq \mathbb{R}^n$ und $S = \{v_1, ..., v_k\}$ ein Orthonormalsystem
mit $k$ genau der Dimension des Unterraums $U$, also $k = \dim(U)$. Dann haben
wir also (gerade gezeigt) eine linear unabhaengige Teilmenge des Raums mit
Kardinalitaet $\dim(U)$ und somit also eine *Basis*. Diese Menge $S$ wird dann
auch *Orthonormalbasis* genannt.

Die Standardbasis ist ein Beispiel fuer eine Orthonormalbasis.

## Gram-Schmidt Algorithmus

Wir interessieren uns dafuer, ob jeder Unterraum denn eine Orthonormalbasis
hat. Also eine Basis, die aus Vektoren besteht, die normiert sind und senkrecht
aufeinander stehen. Tatsaechlich gibt es immer eine solche Basis! Wir finden sie
durch das *Gram-Schmidtsche Orthogonalisierungsverfahren*, einem Algorithmus.

Dieser Algorithmus ist wie folgt beschrieben:

* Input: Ein Unterraum $U$ sowie ein *Erzeugendensystem* (nicht notwendigerweise
  Basis!) $\langle v_1, ..., v_k \rangle \subseteq \mathbb{R}^n = U$.
* Output: Orthonormalbasis $\{u_1, ..., u_m\}$ von $U$. Fuer diese Basis gilt
  dann also, dass $\dim(U) = m \leq k$.

1. Setze $m = 0$ ($m$ ist die eben die momentane Anzahl an gesammelten
   orthonormalen Vektoren)
2. Fuer $i = 1, ..., k$:
  1. Setze $w_i := v_i - \sum_{j=1}^m \langle u_j, v_i \rangle u_j$
  2. Falls $w_i \neq 0$, setze $m := m + 1$ und $u_m = \frac{w_i}{|w_i|}$

Was wir hier machen ist die Orthogonalprojektion des Vektors auf den bestehenden
Vektorraum mit Orthonormalbasis $u_1, ..., u_n$ zu bilden und daraus den
naechsten orthogonalen Vektor zu konstruieren. Hierzu nun viel mehr
Informationen.

### Projektionen

Wir wollen zunaechst betrachten, was eine Projektion ueberhaupt ist, um dann
erst Orthogonalprojektionen zu besprechen und den Gram-Schmidt Algorithmus zu
beweisen und insbesondere zu verstehen.

Eine Projektion ist eine *endomorphe, idempotente lineare Abbildung*. Sei
also $P$ eine lineare Abbildung, dann muss fuer sie gelten:

1. Sie bildet in den selben Raum ab, d.h. sie ist eine Funktion $P: V \rightarrow
  V$. Diese Eigenschaft nennt man *Endomorphie*.

2. Sie bildet Elemente aus ihrem Bild immer auf sich selbst ab. D.h. fuer ihr
  Bild ist eine Projektion immer gerade die Identitaet, sodass $P(v) = v \forall
  v \in P(V)$. Diese Eigenschaft nennt man *Idempotenz*. Die Idempotenz hat
  insbesondere die Folge, dass die Potenz einer Projektion (als mehrfache
  Komposition) gleich der Projektion ist, also: $P^2 = P \circ P = P$, weil das
  Bild, $P(v)$, von $P$ dann ja wieder auf sich selbst, also $P(v)$ abgebildet
  wird. Die Funktion ist also quasi "re-entrant".

Ein Beispiel fuer eine Projektion im zwei-dimensionalen Raum ist die folgende
Darstellungsmatrix fuer eine Funktion $f(v)$:

$$
\begin{bmatrix}
1 & 0 \\
0 & 0
\end{bmatrix}
$$

Sie projeziert jeden Vektor $v \in \mathbb{R}^2$ auf die $x$-Achse, also: $f:
(x, y)^\top \mapsto (x, 0)^\top$. Sie hat alle Eigenschaften einer Projektion,
denn:

1. Sie ist linear, also bildet $f(v + w)$ auf $f(v) + f(w)$ und
   $f(av)$ auf $a \cdot f(v)$ ab.
2. Sie bildet vom Raum $\mathbb{R}^2$ in denselben Raum ab (endomorph).
3. Sie ist idempotent, da man nach dem ersten Mal die $y$-Komponente schon
   weggenommen hat und man danach nichts mehr wegnehmen kann.

### Orthogonalprojektionen

Eine Orthogonalprojektion ist nun eine Projektion, die einen Punkt $P$ auf einen
anderen Punkt $P'$ in einem Vektorraum $V$ so abbildet, dass die
Verbindungslinie $\bar{PP'}$ gerade senkrecht, also orthogonal, zu allen
Vektoren, also insbesondere zu den Basen, dieses Vektorraums steht. Wir
betrachten dieses Konzept nun zunaechst in der euklidischen Ebene, dann im Raum
und letztendlich koennen wir eine Verallgemeinerung finden, die direkt zum
Gram-Schmidtschen Orthogonalisierungsverfahren fuehrt.

#### Euklidische Ebene

In der euklidischen Ebene ist eine Orthogonalprojektion $P_g(x)$ die Abbildung
eines Punkts $P$ auf eine Gerade $g$ (ein Untervektorraum mit einem Basisvektor,
dem Richtungsvektor), sodass die Verbindungslinie $\bar{PP'}$ zwischen $P$ und
dem auf $g$ liegenden Abbild $P'$ von $P$ einen rechten Winkel mit der Gerade
bildet. Wir koennen die Eigenschaften einer Orthogonalprojektion in der Ebene
also wie folgt ausdruecken:

1. Der Punkt $P'$ liegt auf der Geraden $g$: $P' \in g$
2. Die Verbindungslinie $\bar{PP'}$ ist orthogonal zu $g$.

Das koennen wir dann mathematisch "uebersetzen". Hierzu bezeichnen wir nun $x$
als den Vektor zu $P$, sodass $x = (x_1, x_2) = \vec{0P}$. Weiters sei $u$ der
Richtungsvektor der Geraden $g$. Wir nehmen hierbei an, dass $g$ *linear und
nicht affin* ist, also dass $g$ durch den Nullpunkt geht! Haben wir den
Projektionspunkt $P'$, so koennen wir die Verbindungslinie (bzw. den
*Verbindungsvektor*) dann durch $P' - P = P_g(x) - x$ angeben. Dann haben wir:

1. $P_g(x) = \lambda u$: Die Projektion bildet $x$ auf die Gerade $g$ ab. Dieser
   resultierende Punkt ist also ein Vielfaches $\lambda$ vom Richtungsvektor
   $u$.
2. $\langle P_g(x) - x, u \rangle = 0$: Die Verbindungslinie ist orthogonal zu
   $g$.

Dann setzen wir nun einfach die erste Aussage in die zweite ein und erhalten:

1. $\langle P_g(x) - x, u \rangle = 0$
2. $\langle \lambda u - x, u \rangle = 0$
3. $\langle \lambda u, u \rangle - \langle x, u \rangle = 0$
4. $\lambda (u^\top u) = x^\top u$

Insbesondere haben wir jetzt also (5):

$$\lambda = \frac{x^\top u}{u^\top u}$$

Das schoene ist, dass wir nun einen nur von $x$ und $u$ abhaengigen Ausdruck
fuer $P_g(x)$ gefunden haben:

$$P_g(x) = \lambda u = \frac{x^\top u}{|u|^2} u$$

Gilt des Weiteren, dass der Richtungsvektor $u$ der Geraden der Einheitsvektor
ist, dann ist sein Betrag $1$ und sein Quadrat auch. Dann haben wir also:

$$P_g(x) = (x^\top u) u$$

#### Euklidischer Raum

Im eukdlischen Raum, also $\mathbb{R}^3$, haben wir nun eine aehnliche
Situation. Im dreidimensionalen bilden Orthogonalprojektionen Punkte aber nicht
mehr auf Geraden, sondern auf Ebenen ab (Untervektorraeume mit zwei
Basisvektoren). Wir betrachten also nun zwei Vektoren $u, v$ die eine Ebene $E$
aufspannen. Dann haben wir also:

1. Die Orthogonalprojektion $P_E(x)$ bildet $x$ auf die Ebene $E$ ab: $P' \in E$
2. Die Verbindungslinie $\bar{PP'}$ ist nun orthogonal zur Ebene, also
   $\bar{PP'} \perp E$. Insbesondere ist diese Linie also orthogonal zu beiden
   Vektoren $u, v$ im Einzelnen.

Dann uebersetzen wir wieder ins Mathematische:

1. Wenn der Projektionspunkt $P_E(x)$ auf $E$ liegt, koennen wir ihn also als
   (Linear-)Kombination (!) des $\lambda$-fachen des ersten Basisvektors $u$ mit
   dem $\mu$-fachen des zweiten Basisvektors $v$ ausdruecken:
   $P_E(x) = \lambda u + \mu v$.
2. $\langle P_E(x) - x, u \rangle = 0$: Die Verbindungslinie ist orthogonal zum
   ersten Basisvektor.
3. $\langle P_E(x) - x, v \rangle = 0$: Die Verbindungslinie ist orthogonal zum
   zweiten Basisvektor.

Wir haben also einen Punkt $P$ im dreidimensionalen und suchen den
Projektionspunkt $P'$, sodass die Verbindungslinie $\bar{PP'}$ zu beiden
Vektoren senkrecht steht. Wir koennen uns also eine Linie vorstellen, die vom
Punkt $P$ in die Ebene geht, und dann orthogonal zu $u$ und $v$ steht. Wo liegt
dieser Projektionspunkt nun aber? Die Antwort ist: er liegt im Projektionspunkt
des Vektors in der einen Achse, verschoben um den Projektionspunkt auf der
anderen Achse. Wir koennen uns also vorstellen, dass dieser Punkt $P$ bzw. sein
Ortsvektor $x$ zuerst orthogonal zu $u$ gemacht wird, und der resultierende
Verbindungsvektor danach orthogonal zu $v$.

Das ist also die eine Moeglichkeit, die Verbindungslinie zu finden, die
orthogonal auf die ganze Ebene steht: Wir bilden zuerst die Verbindungslinie von
$P$ auf die Achse $u$, indem wir den Projektionspunkt $P_u(x)$ finden und
$P_u(x) - x$ bilden. Diese Linie ist mit Sicherheit orthogonal zu $u$, weil wir
ja die Normalprojektion bilden. Dieser Punkt hat nun eine orthogonale
Komponenten fuer $u$, aber noch immer die selbe Komponente fuer die andere Achse
$uv$, da diese durch die Projektion nicht beeinflusst wurde (!). Dann bilden wir
nun noch von $P_(x) - x$ die Projektion auf $v$ und nehmen wieder die
Verbindungslinie auf den Projektionspunkt. Dann haben wir also die
$v$-Komponente auch orthogonal gemacht.

Die zweite Moeglichkeit, wie wir uns das vorstellen koennen ist, indem wir das
nicht sequentiell wie oben machen, sondern einfach kombinieren. Wir projezieren
$P$ zuerst auf $u$. Dann projezieren wir $P$ unabhaengig auf $v$. Dann haben wir
also ein Vielfaches von $u$ als Projektionspunkt auf $u$ und ein Vielfaches von
$v$ als Projektionspunkt auf $v$. Genauer haben wir zwei Richtungsvektoren
$\lambda u$ und $\lambda v$. Wenn wir diese zu $\lambda u + \mu v$ addieren,
erhalten wir einen Punkt auf der Ebene $E$. Das ist gerade der Projektionspunkt
auf $E$. Nun berechnen wir einfach von $P$ auf diesen Punkt die
Verbindungslinie. Diese ist dann genau orthogonal zur Ebene und insbesondere zu
den beiden Geraden $u, v$.

Mathematisch ist der Unterschied einfach in der Interpretation der folgenden
Summe (vgl. Gram-Schmidt):

$$P_E(x) = x - P_u(x) - P_v(x)$$

Im ersten Fall haben wir das so interpretiert:

$$P_E(x) = (x - P_u(x)) - P_v(x)$$

Eigentlich haben wir es uns als $(x - P_u(x)) - P_v(x - P_u(x))$ vorgestellt,
aber da es nur um die $v$-Komponente geht, und diese von $P_u$ nicht beeinflusst
wurde, sind die beiden Projektionspunkte dieselben. Genauer:

1. Wir finden die Verbindungslinie von $P$ auf $u$: $P_u(x) - x$
2. Dann die Verbindungslinie vom Ende der Verbingslinie (also das Ende des
   Richtungsvektors, im Raum), der nun orthogonal auf $u$ steht, zu $v$. Dabei
   bilden wir aber dann genau die Projektion auf $E$, weil die resultierende
   Verbindungslinie orthgonal zu beiden Achsen wird:
   $P_v(P_u(x) - x) - x = P_E(x) - x$

Im zweiten Fall haben wir die Summe so interpretiert:

$$P_E(x) = x - (P_u(x) + P_v(x))$$

Diese zweite Schreib- und Denkweise ist insbesondere interssant. Wir koenenn
naemlich nun die konkrete Funktion $P_E(x)$ durch Kombination der konkreten
Funktionen $P_u(x)$ und $P_v(x)$ bilden. Wir wissen auch schon, wie wir diese
finden. Wir wissen ja wieder, dass z.B. $P_u(x)$ ein Vielfaches $\lambda v$ von
$v$ ist und $\langle P_u(x) - x, u \rangle = 0$ ist. Dann setzen wir wieder ein
und erhalten denselben Ausdruck wie in der Ebene. Das ganze geht dann auch fuer
$u$. Letztendlich erhalten wir den gesuchten, auf die Ebene und beide Achsen
orthogonalen, Vektor $\bar{PP'}$ durch:

$$\bar{PP'} = x - (\frac{x^\top u}{|u|^2} u + \frac{x^\top v}{|v|^2} v)$$

#### Hoehere Dimensionen

In hoeheren Dimensionen gelten die exakt selben Prinzipen. Der Raum war schon
ein gutes Beispiel dafuer, wie das mit mehr als einem Basisvektor
funktioniert. Fuer noch mehr Vektoren geht das Aehnlich. wir betrachten nun aber
die folgenden Umstaende:

Wir haben eine Orthgonalprojektion auf einen Untervektorraum $U$ eines
Vektorraums $V$ gegeben durch eine lineare Abbildung $P_U: V \rightarrow V$,
sodass gilt:

1. $P_U(v) \in U$: $P_U(v)$ ist eine Projektion und bildet in den
   Untervektorraum ab. Hierbei sei angemerkt, dass ein Untervektorraum ja nichts
   anderes ist als eine Verallgemeinerung der Ebene und des $\mathbb{R}^3$. Wir
   haben nun einfach mehr Basisvektoren. Diese spannen dann eben eine Art hoeher
   dimensionale Ebene auf, z.B. einen auch einen Raum. Wenn $P_U(v)$ also
   abbildet, dann geschieht das eben in den von den Basisvektoren aufgespannten
   Raum. Das ist dann also eine Gerade, eine Ebene, ein Raum etc.
2. $\langle P_U(v) - v, u \rangle = 0$ fuer alle $u \in U$.

Wir uebersetzen wieder in die Mathematik. Der Untervektorraum $U$ hat natuerlich
eine Basis $B = \{u_1, ..., u_k\}$ mit Dimension $k$. Insofern kann man fuer
jeden Vektor $u \in U$ als eindeutige Darstellung eine Linearkombination der
Basisvektoren waehlen, sodass $u = \sum_{i=1}^k c_i u_i$. Des Weiteren gilt nun,
da jeder Vektor in $U$ eben als Linearkombination der Basisvektoren darstellbar
ist, es vollkommen ausreicht, die Orthogonalitaet des Projektionspunktes
gegenueber den Basisvektoren zu zeigen. Ist der Punkt $P_U(v)$ orthgonal zu den
Basisvektoren, so muss er auch orthogonal zu jedem Vektor in diesem Raum sein,
da diese ja aus diesen Basisvektoren bestehen. Somit erhalten wir:

1. $P_U(v) = \sum_{i=1}^n c_i u_i \in U$
2. $\langle P_U(v) - v, u_j \rangle = 0$ fuer $j = 1, ..., k$

Dann setzen wir (1) wieder in (2) ein, und erhalten ueber die Bilinearitaet:

1. $\langle \sum_{i=1}^n c_i u_i - v, u_j \rangle = 0$ (Distributivitaet)
2. $\langle \sum_{i=1}^n c_i u_i u_j \rangle - \langle v, u_j \rangle = 0$
3. $\sum_{i=1}^n c_i \rangle u_i, u_j \rangle - \langle v, u_j \rangle = 0$
4. $\sum_{i=1}^n c_i \rangle u_i, u_j \rangle = \langle v, u_j \rangle$

Diese letzte Gleichung (4) haben wir nun also fuer alle $j = 1, ..., k$. Genauer
haben wir also ein lineares Gleichungssystem mit $k$ Gleichungen und $k$
Unbekannten $c_1, ..., c_k$. Nur um das ganze in Kontext zu setzen: Vorher
hatten wir hier $c_1 := \lambda$ und $c_2 := \mu$ und rechts dann $x^\top u$
bzw. $x^\top v$. Wir koennten nun also diese LGS loesen, wobei ja die skalaren
Produkte $\langle u_i, u_j \rangle$ konkrete Vektoren und somit reelle Werte
sind (als Skalarprodukt). Das sind ja dann die Koeffizienten des LGS. Dieses
koennten wir dann nach den $c_i$ loesen, sodass wir die Koeffizienten fuer die
Linearkombination erhalten, die den Punkt $P_U(x)$ produziert, sodass die
Verbindungslinie $\bar{P_U(x) P}$ gerade orthogonal auf jeden Basisvektor $v_j$
steht.

Im Weiteren wollen wir nun aber nur den Fall betrachten, dass $B$ eine
Orthogonalbasis von $U$ ist, sodass $\langle u_i, u_j \rangle \neq 0
\Leftrightarrow i = j$. Dann haben wir nun die Situation, dass das skalare
Produkt $\langle u_i, u_j \rangle$ in der Summe genau ein einziges Mal nicht
Null ist, naemlich wenn $i = j$. Dann reduziert sich die ganze Summe also auf

$$c_j \langle u_j, u_j \rangle = \langle v, u_j \rangle .$$

Wir erhalten dann:

1. $c_i = \frac{\langle v, u_j \rangle}{\langle u_i, u_i \rangle}$
2. $c_i = \frac{v^\top u_j}{|u_i|^2}$.

Wir haben also unsere Koeffizienten fuer diesen Spezialfall gefunden. Somit
koennen wir auch die Projektionsfunktion eindeutig definieren:

$$P_U(v) = \sum_{i=1}^k c_i u_i = \sum_{i=1}^k \frac{v^\top u_i}{|u_i|^2}u_i.$$

Ist die Basis $B$ sogar eine Ortho*normal* basis, dann ist $\langle u_i, u_i
\rangle = |u_i|^2 = 1^2 = 1$ und $\langle u_i, u_j \rangle = 0$ wenn $i \neq j$
(das galt aber schon fuer eine orthogonale Basis). Genauer ist $u_i^\top u_j$ ja
dann gleich dem Kronecker Delta $\delta_{i,j}$. Dann haben wir die folgende
schoene Darstellung fuer die Orthogonalprojektion, die vor Allem fuer das
Gram-Schmidtsche Orthogonalisierungsverfahren relevant ist:

$$P_U(v) = \sum_{i=1}^k \langle v, u_i \rangle \cdot u_i$$

Den auf den ganzen Raum $U$ orthogonalen Vektor, also die Verbindungslinie von
$v$ zu $P_U(v)$, erhalten wir dann wieder ueber: $P_U(v) - v$ bzw. $v - P_U(v)$.

Eine Eigenschaft, die wir noch schoen ueber Bilinearitaet zeigen koennen, ist
die Idempotenz von $P_U(v)$ im Sinne dessen, dass es Elemente in seinem Bild auf
sich selbst abbildet. Betrachten wir hierfuer einen Vektor $v \in U$. Dieser ist
also schon im Bild der Projektionsfunktion (wir bilden ja aus $V$, in $U$
bzw. $V$ ab). Daher koennen wir ihn also als Linearkombination der Basisvektoren
von $B$ ausdruecken und erhalten:

$$v = \sum_{i=1}^n c_i u_i$$

Dann setzen wir diesen Vektor $v$ einfach einmal in $P_U$ ein (wir nehmen an die
Basis ist orthonormal):

1. $P_U(v) = \sum_{i=1}^k \langle \sum_{j=1}^k c_j u_j, u_i\rangle u_i$
2. $P_U(v) = \sum_{i=1}^k \sum_{j=1}^k c_j \langle u_j, u_i\rangle u_i$
3. $P_U(v) = \sum_{i=1}^k c_i u_i$
4. $P_U(v) = v$

Von (1) auf (2) haben wir die Bilinaeritaet angewandt und von (2) auf (3) die
Tatsache, dass die Basis orthonormal ist. Dann ist ja $\langle u_i, u_j \rangle
= \delta_{i,j}$ und somit genau ein einziges Mal *Eins*. Das ist genau, wenn $i
= j$. D.h. wir erhalten fuer jedes $i$ genau $c_i \cdot u_i$. Das gab uns dann
eben genau wieder die Linearkombination, durch welche wir $v$ ja ausgedrueckt
hatten.

### Gram Schmidt

Wir koennen nach diesem Exkurs das Gram-Schmidt Verfahren viel besser
verstehen. Wir wissen nun naemlich, dass es eigentlich ueberhaupt gar nichts
besonderes macht. Druecken wir den Algorithmus mit unserem neuen Wissen neu aus:

Input: Ein Untervektorraum $U$ sowie ein Erzeugendensystem $\langle v_1, ...,
v_k \rangle$ von $U$.
Output: Eine Orthonormalbasis $\langle u_1, ..., u_m \rangle$ von $U$.

1. Setze den Count von gefundenen Orthornormalbasisvektoren $m$ initial auf $0$.
2. Dann, fuer jeden Vektor $v_i$:
   1. Bilde die Orthogonalprojektion von $v_i$ auf die momentanen
      Orthogonalbasisvektoren (auf den bisherigen Unterraum) und erhalte dann
      durch Subtraktion von $v_i$ die Verbindungslinie $w_i$, welche dann nach
      Definition der Orthogonalprojektion eben orthognal auf die bisherigen
      Vektoren im Untervektorraum steht.
   2. Aus (1) erhalten wir neue Ortho*gonal*basen. Wir wollen aber
      Ortho*normal*basen. Also normiere $w_i$ indem man es durch seine Laenge
      dividiert und erhalte so den naechsten Orthonormalbasivektor $u_m =
      w_i/|w_i|$ (wobei also $u_m = w_{i_0}$).
   3. Setze $m := m + 1$ und zurueck zu (2).

Wir bilden aus unseren $v_i$ einfach Schritt fuer Schritt neue Vektoren durch
Orthogonalprojektionen auf den bisherigen Raum, normieren diese und erhalten so
Orthonormalbasisvektoren. Gram Schmidt findet also einfach iterativ
Normalprojektionen und nimmt die Verbindungslinien als neue Orthonormalbasen
auf. Fertig.

Es gibt aber bei (2) drei Faelle zu beachten, die wir unterschiedlich
interpretieren und behandeln muessen:

1. $v$ steht bereits senkrecht auf den Raum. Wir betrachten Vektoren ja immer
   so, dass sie vom Ursprung ausgehen (als Ortsvektoren). D.h. der Vektor geht
   durch den Ursprung und ist dort orthogonal zu allen anderen Vektoren. Dann
   ist die Orthogonalprojektion, also der Punkt auf der Gerade, Ebene etc.,
   gerade der Ursprung, also die Null. Dann haben wir also $v - \sum_{i=1}^k 0 =
   v$. Die orthogonale Verbindungslinie zum Ursprung ist ja dann auch gerade $v$
   selbst, weil $v$ ja schon orthogonal ist. D.h. in diesem Fall nehmen wir $v$
   so in unsere Orthonormalbasis, wie er ist, weil er ja genau das ist, was wir
   wollen.
2. $v$ ist bereits im Raum (auf der Geraden, in der Ebene etc.). Er laesst sich
   also als Linaerkombination der Basisvektoren des (momentanen) Unterraumes
   ausdruecken. Dann bildet die Projektion, als idempotente Funktion, diesen
   Punkt gerade auf sich selbst ab (wie oben auch bewiesen). Dann ist $v -
   P_U(v) = v - v = 0$. Solche redundanten Vektoren, wo $w_i = 0$, nehmen wir
   natuerlich nicht in unsere Orthonormalbasis auf.
3. Fuer alle anderen Vektoren $v$ gelten die Standardregeln, also dass $w_i =
   v - P_U(v)$.

Wir koennen nun noch beweisen, dass ein neuer Vektor $w_i$, so wie wir ihn
waehlen, auch sicher orthogonal zu jedem beliebigen Vektor $u_l$ ist, den wir
bereits eingesammelt haben:

1. $\langle u_l, w_i \rangle$ muesste dann Null sein.
2. $\langle u_l, v - \sum_{j=1}^m (v^\top u_j) u_j \rangle$ (nach Gram-Schmidt)
3. $\langle u_l, v \rangle - \langle u_l, \sum_{j=1}^m \langle v^\top u_j
   \rangle u_j \rangle$ (Distribuieren $u_l$)
4. $\langle u_l, v \rangle - \sum_{j=1}^m u_l \langle v^\top u_j \rangle u_j$
   (Ziehen die Summe nach Bilinearitaet raus)
5. $\langle u_l, v \rangle - \sum_{j=1}^m \langle v^\top u_j \rangle \langle
   u_l, u_j \rangle$ (Das Skalarprodukt ist ein Skalar, also kommutativ)
6. $\langle u_l, v \rangle - \langle u_l, v \rangle$ ($\langle u_l, u_j
   \rangle = \delta_{i,j}$)

Was haben wir nun aus dem ganzen? Nun: Da jeder Vektorraum ein Erzeugendensystem
hat, und wir nun wissen wie wir aus einem beliebigen Erzeugendensystem eine
Orthonormalbasis bilden koennen, gilt also, dass

__jeder Unterraum des $\mathbb{R}^n$ eine Orthonormalbasis hat__.

## Orthogonale Matrizen

Wir haben bisher Orthonormalbasen, also Mengen von Vektoren betrachtet. Wenn wir
nun eine solche Menge nehmen und sie in die Spalten einer (quadratischen) Matrix
$A \in \mathbb{R}^{n \times n}$ geben, dann nennt man diese Matrix $A$
*orthogonal*. Achtung: eine *orthogonale* Matrix hat *orthonormale* Spalten. Es
gibt insofern keine orthonormale Matrizen, man nennt sie naemlich dann einfach
nur orthogonal.

Eine orthogonale Matrix hat die besondere und hinreichende Eigenschaft, dass gilt

$$A^\top A = I_n.$$

Wenn wir also diese Orthonormalvektoren in den Spalten nehmen und sie alle
transponieren und mit $A$ multiplizieren, dann multiplizieren wir jeden
Orthonormalvektor mit jedem anderen Orthonormalvektor. Was man daraus erhaelt
ist eine *Gram Matrix*, welche an Position $(i, j)$ das Skalarprodukt der
Vektoren von $A$ und $A^\top$ in diesen Positionen enthaelt, also hier eben
$\langle v_{i, j}, v_{j, i} \rangle$. Wenn die Matrix nun *orthogonale* Vektoren
enthaelt, dann ist diese Gram Matrix eine *Diagonalmatrix*, mit dem Quadrat der
Laenge jeden Vektors in der Diagonalen. Wegen der Orthonormalitaet der Vektoren
gilt nun weiter, dass ihr Skalarprodukt gleich dem Kronecker-Delta ist, welches
genau dann Eins und nicht Null ist, wenn $i = j$. Sprich: diese Matrix hat in
den Diagonalen nur Einsen. Also haben wir die Einheitsmatrix.

Wichtig hierbei ist, dass *jede orthogonale Matrix* $A \in \mathbb{R}^{n \times
n}$ eine Basis des $\mathbb{R}^n$ bildet, mit den Spalten der Matrix als
Basisvektoren. Denn wir haben vorhin gezeigt, dass orthogonale Vektoren immer
linear unabhaengig sind. Wenn wir nun also $n = \dim(\mathbb{R}^n)$ viele
solcher linear unabhaengigen Vektoren haben, ist diese Menge inklusionsmaximal
und somit eine Basis des $\mathbb{R}^n$.

Aus der Eigenschaft $A^\top A = I_n$ folgt natuerlich auch gleich, dass $A^\top
= A^{-1}$!. Dann definieren wir noch:

1. Die Menge aller orthogonalen Matrizen $O_n(\mathbb{R}) = \{A \in
   \mathbb{R}^{n \times n} \,|\, A^\top A = I_n\}$ (man bemerke, dass $A^\top A
   = I_n$ wirklich eine fuer orthogonale Matrizen eindeutige Eigenschaft
   ist). $O_n(\mathbb{R})$ nennt man die *orthogonale Gruppe*.

2. $SO_n(\mathbb{R}) = O_n(\mathbb{R}) \cap SL_n(\mathbb{R})$ nennt man die
   *spezielle orthogonale Gruppe*. Die spezielle lineare Gruppe
   $SL_n(\mathbb{R})$ hat dabei die Eigenschaft, dass ihre Determinante immer
   Eins ist.

Wir koennen fuer die Menge aller orthogonalen Matrizen die Gruppenstruktur mit
dem Matrixprodukt beispielsweise schon schoen anhand der Abgeschlossenheit
zeigen. Dann muesste also gelten, dass fuer zwei orthogonale Matrizen $A, B$ ihr
Produkt wiederum orthogonal ist. Das gilt, denn:

$$
\begin{align}
	(AB)\top \cdot (AB) &= (B^\top A^\top) (A B) \\
		                &= B^\top (A^\top A) B   \\
						&= B^\top (I_n) B        \\
						&= B^\top B              \\
						&= I_n
\end{align}
$$

Eine interessante Eigenschaft von orthogonalen Matrizen ist, __dass sie
laengenerhaltend sind__. Sei hierzu $A \in O_n(\mathbb{R})$ eine orthogonale
Matrix und $v \in \mathbb{R}^n$ ein Vektor, dann gilt:

$$
\begin{align}
	|Av| &= \sqrt{\langle Av, Av, \rangle}
	     &= \sqrt{(Av)^\top (Av)}
		 &= \sqrt{((v^\top A^\top) (Av))}
		 &= \sqrt{v^\top (A^\top A) v}
		 &= \sqrt{v^\top I_n v}
		 &= \sqrt{v^\top v}
		 &= |v|
\end{align}
$$

Wie man sieht hat die Multiplikation von $v$ mit der Matrix $A$ zwar
(womoeglich) den Vektor $v$ veraendert, aber eben *nicht seine Laenge*. Solche
Abbildungen wie $f_A$, die laengenerhaltend sind, nennt man
*Isometrien*.

Orthogonale Matrizen erhalten aber nicht nur Laengen erhalten, sondern allgemein
*Skalarprodukte*:

1. $\langle Av, Aw \rangle$
2. $(Av)^\top Aw$
3. $v^\top A^\top A w$
4. $v^\top w$ (Eigenschaft von Orthogonalmatrizen)
5. $\langle v, w \rangle$

## Zusammenfassung

Hier die wichtigsten Eigenschaften, die in diesem Kapitel besprochen wurden.

### Skalarprodukt

Seien $v,w$ zwei Vektoren. Dann gilt:

* $\langle v, w \rangle = v^\top w = \sum_i v_i w_i$
* $\langle u, v + aw \rangle = \langle u,v \rangle + a \langle u, w \rangle$
* $\langle v, w \rangle = \langle w, v \rangle$
* $(K^n)^\perp = \{0\}$

### Laenge (Betrag)

* $|v| = \sqrt{\langle v, v \rangle} = \sqrt{v^\top v}$
* $|v|^2 = \langle v, v \rangle = v^\top v$
* $|av| = |a||v|$
* $|v + w| \leq |v| + |w|$ (Dreiecksungleichung)
* $| \langle v, w \rangle | \leq |v||w|$ (Cauchy-Schwarz)
* $|v| = 0 \iff v = 0$
* $v_0 = v/|v|$ (Normierter Vektor; Einheitsvektor)

### Orthogonalitaet

Sei $A \in \mathbb{R}^{n \times n}$ eine orthogonale Matrix (und dessen Spalten
entsprechend ein Orthonormalsystem). Dann gilt:

* $\langle v_i, v_j \rangle = \delta_{i,j}$ (fuer Spalten $v_i, v_j$)
* $A^\top = A^{-1}$
* $|v_i| = 1$
* $A^k = I_n$, wenn $k$ gerade, sonst $A$
* $|Av| = |v|$
* $\langle Av, Aw \rangle = \langle v, w \rangle$
* Die Spalten von $A$ sind linear unabhaengig
* $A$ ist eine Basis des $\mathbb{R}^n$
* Gram-Schmidt Update:
  1. $w_i = v_i - \sum_j^m (u_j^\top v) u_j$
  2. $u_i = w_i/|w_i|$
* Jeder Unterraum des $\mathbb{R}^n$ hat eine Orthnormalbasis (via Gram-Schmidt)
* Jede orthogonale Matrix $A$ ist invertierbar, da $A^{-1} = A^\top$
* $U^\perp$ ist $\Kern(B^\top)$ mit Basis $B$ von $U$
