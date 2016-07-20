# Komplexe Zahlen

$\newcommand{\det}{\mathop{\rm det}\nolimits}$
$\newcommand{\adj}{\mathop{\rm adj}\nolimits}$

In diesem Kapitel wollen wir komplexe Zahlen definieren und
untersuchen. Insbesondere werden wir sehen, dass man alle besonderen
Rechenregeln von komplexen Zahlen durch die uns schon bekannten Regeln fuer
Operationen auf Matrizen erklaeren kann.

## Motivation

Wir wissen, wie man ein Polynom in seine Linearfaktoren aufspaltet, um so die
Nullstellen bzw. (im falle charakteristischer Polynome) Eigenwerte zu
finden. Dann probieren wir das nun beim boesen Polynom $x^2 + 1$. Wir erhalten:

$$x_{1,2} = \pm \sqrt{-1}$$

Arbeiten wir in $\mathbb{R}$, so hat dieses Polynom anscheinend keine (rellen)
Nullstellen. Aber in $\mathbb{C}$ zerfaellt $f$ schon in Linearfaktoren,
naemlich:

$$f = (x + 1)^2 = (x + i)(x - i)$$

mit $i$ der imaginaeren Einheit. Die Nullstellen waeren also $\pm i$. Es gilt
aber nicht nur fuer das boese Polynom, dass es unter $\mathbb{C}$ in Nullstellen
zerfaellt, sondern generell fuer alle Polynome. Diese Tatsache nennt man
__Fundamentalsatz der Algebra__. Dieser Satz lautet:

> Jedes nicht-konstante Polynom $f \in \mathbb{C}[x]$ hat zumindest eine
> Nullstelle in $\mathbb{C}$.

Es gibt noch andere Koerper wie $\mathbb{C}$, in welchen Polynome *immer*
zerfallen. Diese besonderen Koerper heissen *algebraisch abgeschlossen*. Was
sind aber nun komplexe Zahlen ueberhaupt? Wir wollen sie untersuchen.

### Definition

Wir wollen versuchen, komplexe Zahlen alleine ueber Matrizen und lineare Algebra
einzufuehren. Hierzu betrachten wir zwei Matrizen $\in \mathbb{R}^{2 \times 2}$:

* Die Einheitsmatrix $I_2 = \begin{bmatrix}1 & 0 \\ 0 & 1\end{bmatrix}$
* Die "$i$-Matrix": $i = \begin{bmatrix} 0 & 1 \\ -1 & 0\end{bmatrix}$

Wir benutzen diese beiden Matrizen nun, um die allgemein bekannte Form einer
komplexen Zahl $a + bi$ darzustellen (erstmal nicht konjugiert). Skalieren wir
naemlich die Einheitsmatrix mit einem beliebigen Skalar $a$ und die $i$-Matrix
mit einem beliebigen Skalar $b$, erhalten wir:

$$
\{a \cdot I_n + b \cdot i \,|\, a,b \in \mathbb{R}\} =
\{\begin{bmatrix}a & b \\ -b & a\end{bmatrix} \,|\, a,b \in \mathbb{R}\}
$$

Wir haben nun also eine Darstellung dieser Zahlen $a + bi$ in Matrizenform
gefunden (weil die Matrizen $I_n$ und $i$ orthogonal sind passen sie schoen in
eine Matrix rein). Somit ist diese Menge von Matrizen gerade die Menge der
komplexen Zahlen $\mathbb{C}$ (bzw. isomorph dazu). Wir werden nun zeigen, wie
man ueber diese Matrizendarstellung von komplexen Zahlen alle besonderen oder
nicht besonderen Rechenregeln fuer komplexe Zahlen herleiten kann.

Wir koennen nun auch schon gleich pruefen, ob auch hier gilt, dass $i^2 =
-1$. In diesem Fall ist jede relle Zahl $r$ wie $-1$ eine Matrix $r \cdot I_n$,
also muesste $i^2 = -I_n$ gelten. Hierzu multiplizieren wir unsere oben
definierte $i$-Matrix einfach mit sich selbst, und erhalten:

$$
i^2 =

\begin{bmatrix}
 0 & 1 \\
-1 & 0 \\
\end{bmatrix}

\cdot

\begin{bmatrix}
 0 & 1 \\
-1 & 0 \\
\end{bmatrix}

=

\begin{bmatrix}
-1 &  0 \\
 0 & -1 \\
\end{bmatrix}

 = -I_2
.$$

Es gilt also hier genauso, dass $i^2 = -I_2$.

#### Koerpereigenschaften

Unsere "Matrixmenge" $\mathbb{C}$ ist offensichtlich ein Unterraum des
$\mathbb{R}^{2 \times 2}$ (da wir nur relle Werte in den Matrizen haben). Somit
ist $\mathbb{C}$ automatisch schon eine abelsche Gruppe fuer $(\mathbb{C},
+)$. Wir muessen also die Addition und Subtraktion nicht weiter beweisen.

Die Abgeschlossenheit von $\mathbb{C}$ bezueglich der Multiplikation muessen wir
aber dennoch zeigen. Betrachten wir zur weiteren Untersuchung der Rechenarten
(zur Veranschaulichung auch der Addition) zwei komplexe Zahlen (in unserer
Darstellung) $z_1 = a_1I_2 + b_1i$ und $z_2 = a_2I_2 + b_2i$. Dann:

##### Addition

Die Addition muss gelten, weil unser Matrixraum ein Unterraum des $\mathbb{R}^{2
\times 2}$ ist. Also betrachten wir die Addition $z_1 + z_2$. Hierbei addieren
wir Realteil mit Realteil, also $a_1 + a_2$ und Imaginaerteil mit Imaginaerteil,
also $b_1 + b_2$. So erhalten wir:

$$
\begin{align}
 z_1 + z_2 &= (a_1I_2 + b_1i) + (a_2I_2 + b_2i) \\
           &= (a_1 + a_2)I_2 + (b_1 + b_2)i
\end{align}
$$

Da $a,b \in \mathbb{R}$ sind, ist $a_1 + a_2$ bzw. $b_1 + b_2$ sicher wieder
jeweils eine relle Zahl, also ist $z_1 + z_2$ auch wieder eine Matrix aus
unserer Menge $\mathbb{C}$.

##### Multiplikation

Nun pruefen wir auf die Abgeschlossenheit der Menge $\mathbb{C}$ bezueglich der
Multiplikation. Wir pruefen also, ob $z_1 \cdot z_2$ wieder eine Matrix $\in
\mathbb{C}$ ergibt. Hierzu multiplizieren wir die beiden Terme einfach aus, und
sehen, was rauskommt:

$$
\begin{align}
  z_1 \cdot z_2 &= (a_1I_2 + b_1i) \cdot (a_2I_2 + b_2i) \\
                &= a_1a_2I_2^2 + (a_1b_2 + a_2b_1)I_2i + b_1b_2i^2 \\
		        &= a_1a_2 + (a_1b_2 + a_2b_1)i + b_1b_2i^2 \\
		        &= a_1a_2 + (a_1b_2 + a_2b_1i) - b_1b_2 \\
		        &= (a_1a_2 - b_1b_2)I_2 + (a_1b_2 + a_2b_1)i \\
\end{align}
$$.

Wir haben hier einmal $i^2$ zu $-I_2$ umgeformt, dessen Gueltigkeit oben schon
gezeigt wurde. Dann haben wir ausgenutzt, dass $b_1b_2$ dasselbe ist wie
$b_1b_2I_2$, und haben letztlich die Identitaetsmatrix aus diesem Ausdruck sowie
auch $a_1a_2I_2$ herausgehoben.

Es gilt im Uebrigen auch: $z_1 \cdot z_2 = z_2 \cdot z_1$, d.h. die so
definierte Multiplikation ist kommutativ. Das ist fuer einen Koerper auch
notwendig ($\langle \mathbb{C} \setminus \{0\}, \cdot \rangle$ muss eine
abelsche Gruppe sein; fuer einen Ring wuerde ein Monoid reichen). Diese Tatsche
folgt aus der Kommutativitaet der Multiplikation reeller Zahlen, worauf, wie man
sieht, die Multiplikation zweier Matrizen aus $\mathbb{C}$ hinauslaeuft.

Assoziativitaet und Distributivitaet uebertragen sich ebenso aus dem
$\mathbb{R}^{2 \times 2}$ auf $\mathbb{C}$. Dann wissen wir also schon, dass
$\mathbb{C}$ ein kommutativer Ring. Um aber zu zeigen, dass $\langle \mathbb{C},
+, \cdot \rangle$ ein Koerper ist, muessen wir zeigen dass, dass $\langle
\mathbb{C}, \cdot \rangle$ eine Gruppe ist. Wir haben bisher aber nur die
Monoid-Eigenschaften gezeigt, uns fehlt naemlich noch der Beweis der Existenz
des Inversen jeden Elements $\in \mathbb{C}$.

Wir kennen aber die nette Formel fuer $2 \times 2$ Matrizen, um die Inverse zu
finden: $A^{-1} = \det(A)^{-1} \cdot C$ wo $C$ die adjunkte Matrix zu $A$
ist. Also berechnen wir die Inverse mit ihr.

Zuerst pruefen wir, ob die Matrix $A$ ueberhaupt eine Inverse hat. Das sehen wir
daran, ob $\det(A) \neq 0$, da sonst die Multiplikation mit $\det(A)^{-1}$
undefiniert waere. Wir muessen die Existenz ja nur fuer nicht-Null Elemente
zeigen, also wissen wir: $z = aI_2 + bi \neq 0$. Daraus folgt, dass, die
Determinante, fuer unsere Matrizen gegeben durch

$$\det(z) = (a \cdot a) - (-b \cdot b) = a^2 + b^2,$$

ungleich Null sein muss.

Nun setzen wir einfach ein:

$$
\begin{align}
z^{-1} &= \frac{1}{a^2 + b^2}\begin{bmatrix}a & -b \\ b & a\end{bmatrix} \\
       &= \frac{a}{a^2 + b^2}I_2 - \frac{b}{a^2 + b^2}i
\end{align}
$$

Hierbei haben wir die Matrix einfach in seine Komponenten aufgespalten und die
Summe duch diese Definiert. Wir sehen, dass wir nun wieder einen Ausdruck der
Form $x + yi$ haben. Da die Zahlen $a,b \in \mathbb{R}$, und wir festgestellt
haben, $a + bi$ ungleich Null ist, ist die Division bzw. Multiplikation fuer sie
abgeschlossen. Somit erhalten wir also wieder eine wohldefinierte Zahl $z^{-1}$
welche sich als Matrix darstellen laesst.

Hierbei war $\begin{bmatrix}a & -b \\ b & a\end{bmatrix}$ die Adjunkte unserer
Matrix, welche in diesem Fall sogar einfach die Transponierte der Matrix
ist. Somit haben wir also nun durch den Beweis die Existenz eines Inversen fuer
Zahl festgestellt. Daraus folgt: $\mathbb{C}$ ist ein Koerper.

### Rolle von $\mathbb{R}$ bezuglich des Koerpers $\mathbb{C}$

Um nun die allgemein bekannten Gesetze fuer komplexe *Zahlen* durch unsere
Matrixschreibweise zu finden, definieren wir einfach eine Abbildung $\epsilon:
\mathbb{R} \rightarrow \mathbb{C}: a \mapsto aI_2$. Diese Abbildung ist injektiv
(da nur auf die Zahlen ohne Imginaerteil abgebildet wird) und erfuellt drei
Eigenschaften:

* $\epsilon(a + b) = \epsilon(a) + \epsilon(b)$
* $\epsilon(a \cdot b) = \epsilon(a) \cdot \epsilon(b)$
* $\epsilon(1) = I_2$.

Wir haben also eine Abbildung von $aI_2$ in unserer Matrixschreibweise auf die
Zahl $a \in \mathbb{R}$. Somit koennen wir also alle $\epsilon(a) \in
\mathbb{C}$ durch $a \in \mathbb{R}$ repraesntieren. Hier haben wir gerade die
Abgeschlossenheit dieser Abbildung bezueglich Addition und Multiplikation, sowie
die Existenz des neutralen Elements gezeigt. Somit gilt also sicher:
$\mathbb{R}$ ist Unterkoerper von $\mathbb{C}$, wenn wir fuer $b$ in $aI_2 +
b_i$ einfach immer die Null waehlen.

Im Raum der Zahlen gilt nun also: $\mathbb{C} = \{ a + bi \,|\, a,b \in
\mathbb{R}\}$ mit $i^2 = -1$. Fuer eine solche Zahl $z = a + bi$ spricht man
dann von:

* $z$: eine *Komplexe Zahl*
* $a$: der *Realteil*
* $b$: der *Imaginaerteil*
* $i$: die *Imaginaere Einheit*

Ausserdem gibt es zu jeder komplexen Zahl $z = a + bi$ auch noch eine passende
*konjugiert komplexe* Zahl $\bar{z} = a - bi$. Diese gibt es natuerlich auch
entsprechend als Matrix.

Wir kennen fuer die komplexen Zahlen die Regel, dass wenn wir eine komplexe Zahl
$z_1 = a + bi$ durch eine andere $z_2 = c + di$ teilen, wir den Bruch
$\frac{z_1}{z_2}$ zuerst um die konjugiert komplexe Zahl $\bar{z_2}$ erweitern
muessen, also:

$$
\begin{align}
  \frac{z_1}{z_2} &= \frac{z_1\bar{z_2}}{z_2\bar{z_2}} \\
  	              &= \frac{(a + bi)(c - di)}{(c + di)(c - di)} \\
				  &= \frac{(a + bi)(c - di)}{c^2 + d^2}
\end{align}
$$

Hierbei ist $(c + di)(c - di) = c^2 - d^2i^2$, und weil $i^2 = -1$, haben wir
dann eben $c^2 + d^2$.

Wir koennen dies ueber unsere Matrixschreibweise herleiten:

1. Zunaechst gilt natuerlich: $z_1/z_2 = z_1z_2^{-1}$. Wir multiplizieren $z_1$
   also einfach mit dem Inversen von $z_2$.

2. Das Inverse von $z_2$ finden wir nach obiger Erlaeuterung durch die Adjunkte:
   $z_2^{-1} = \frac{1}{\det(z_2)} \begin{bmatrix}c & -d \\ d & c\end{bmatrix}$.

   Da $\det(z_2) = c^2 + d^2$ und $\begin{bmatrix}c & -d \\ d & c\end{bmatrix} =
   cI_2 - di$ (Achtung: $-di$ weil wir die Adjunkte gebildet haben) koennen wir
   das auch schreiben als:

   $z_2^{-1} = \frac{c}{c^2 + d^2}I_2 - \frac{di}{c^2 + d^2}$.

3. Dann haben wir momentan also: $z_1z_2^{-1} = (aI_2 + bi)(\frac{c}{c^2 +
   d^2}I_2 - \frac{di}{c^2 + d^2})$.

4. Wir bilden nun (durch $\epsilon$) von der Matrixschreibweise in die
   Zahlenschreibweise. Dabei verschwindet $I_2$ einfach und wir koennen den
   rechten Teil auf einen gemeinsamen Nenner ziehen:

   $\frac{c}{c^2 + d^2} - \frac{-di}{c^2 + d^2} = \frac{c - di}{c^2 + d^2}$

5. Dann multiplizieren wir aus: $\frac{(a + bi)(c - di)}{c^2 + d^2}$. Die
   Aussage gilt also! Wir sehen also genau, wieso man im Zahlenraum mit
   $\bar{z_2}$ erweitern muessen.

6. Wir koenenn untere binomische Formel noch ausschreiben:
   $\frac{(a + bi)(c - di)}{(c + di)(c - di)}$

7. Dann duerfen wir $c - di$ noch weggkuerzen, und erhalten gerade unseren
   urspruenglichen Bruch: $\frac{a + bi}{c + di}$

Jetzt wissen wir also, wieso wir bei der Division $z_1/z_2$ mit der konjugiert
komplexen Zahl $\bar{z_2}$ erweitern muessen. Das folgt hier eben einfach aus
der Definition des Inversen der Matrix $z_2$.

### Betrag von Komplexen Zahlen

In den rellen Zahlen koennen wir durch $\leq$ bzw. $\geq$ eine totale Ordnung
definieren. In den komplexen Zahlen ist $\leq$ keine gueltige Relation, da man
$1 + 7i$ und $10 + 3i$ z.B. nicht richtig einordnen kann. Wir koennen aber den
*Betrag* einer komplexen Zahl nutzen, um eine totale Ordnung auf $\mathbb{C}$ zu
definieren.

Der Betrag $|z|$ einer komplexen Zahl $z$ ist definiert als:

$$|z| = \sqrt{z \bar{z}} = \sqrt{a^2 + b^2} \in \mathbb{R}_{\geq 0}$$

Diese Definition laesst sich auch geometrisch interpretieren. Betrachten wir
hier fuer eine komplexe Zahl $z = a + bi$ die Zahlen $a,b$ als Komponenten eines
Vektors $(a, b)^\top$ in einem Gausschen Koordinatensystem. Dabei beschreibt $a$
also die Richtung des Vektors auf der realen Achse und $b$ die Richtung auf der
imaginaeren Achse. Dann ist der Betrag der komplexen Zahl $z$ eben einfach der
Betrag (die Laenge) dieses Vektors, welcher ja bekanntlich als die Wurzel aus
der Summe der Quadrate der Komponenten besteht. Sieht man $z$ also als Vektor,
haben wir einfach $|z| = ||z||_2$.

Wir haben nun also die Verbindung $|z| = \sqrt{z \bar{z}}$. Dann ist also
$z\bar{z} = |z|^2$. So koennen wir das multiplikative Inverse (im Zahlenraum)
schoen definieren als:

$$z^{-1} = \frac{\bar{z}}{|z|^2}$$

Das multiplikative Inverse $z^{-1}$ einer komplexen Zahl $z \neq 0$ ist also
gleich dem Konjugierten $\bar{z}$ der Zahl, durch das Quadrat der Laenge der
komplexen Zahl. Betrachten wir noch die Definition der Inversen im Matrixraum:

$$z^{-1} = \frac{\adj(z)}{\det(z)}$$

Wir sehen also:

* Fuer unsere $2 \times 2$ Matrizen $\in \mathbb{C}$, die komplexe Zahlen
  darstellen, ist die Adjunkte der Matrix gerade das konjugiert Komplexe der
  Zahl. Das folgt, weil die Adjunkte fuer unsere Matrizen die Realteile ja nicht
  veraendert (Vertauschung von $a$ mit $a$), wobei die Imaginaerteile $b$
  bzw. $-b$ ihr Vorzeichen veraendern. So gilt also, dass $\adj(z) = \bar{z}$.

* Die Determinante einer solchen Matrix ist gleich dem Quadrat des Betrags der
  komplexen Zahl. Denn wir haben dann $\det(z) = a^2 - (-b \cdot b) = a^2 +
  b^2$.

#### Eigenschaften des Betrags

Wir wollen noch einige Eigenschaften des Betrags einer komplexen Zahl nennen:

* $|z| = 0 \iff z = 0$, sonst waere $|z|^2 = a^2 + b^2 > 0$

* $|z_1 \cdot z_2| = |z_1| \cdot |z_2|$. Das folgt aus der Multiplikativitaet
  der komplexen Zahlen (explizites Nachrechnen).

* $|z| = |\bar{z}|$, weil das Vorzeichen beim Quadrieren sowieso verloren geht.

* $|z_1 + z_2| \leq |z_1| + |z_2|$ (Dreiecksungleichung)

## Zusammenfassung

Hier die in diesem Kapitel besprochenen Eigenschaften.

1. $z = a + bi$ mit:
	* $z$: Komplexe Zahl
	* $a$: Realteil
	* $b$: Imaginaerteil
	* $i$: Imaginaere Einheit
2. $\bar(a + bi) = a - bi$
3. $|z| = \sqrt{z \cdot \bar{z}} = \sqrt{a^2 + b^2}$
4. $|z_1 z_2| = |z_1| \cdot |z_2|$
5. $\frac{z_1}{z_2} = \frac{z_1\bar{z_2}}{z_2\bar{z_2}}$
6. $(a + bi) + (c + di) = (a + c) + (b + d)i$
7. $(a + bi)(c + di) = (ac - bd) + (ad + bc)i$
8. $z^{-1} = \frac{\bar{z}}{|z|^2}$
9. $|z_1 + z_2| \leq |z_1| + |z_2|$
10. $|z| = 0 \iff z = 0$
11. $|z| \geq 0 \iff a \geq 0 \lor b \geq 0$
