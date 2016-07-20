# Hauptachsentransformation

$\newcommand{\dim}{\mathop{\rm dim}\nolimits}$
$\newcommand{\Kern}{\mathop{\rm Kern}\nolimits}$
$\newcommand{\GL}{\mathop{\rm GL}\nolimits}$
$\newcommand{\diag}{\mathop{\rm diag}\nolimits}$

In diesem Kapitel wird gezeigt, dass __jede symmetrische, reelle Matrix
diagonalisierbar ist__. Wir betrachten also immer symmetrische Matrizen $A \in
\mathbb{R}$ mit der Eigenschaft:

$$A^\top = A \ in \mathbb{R}^{n \times n} .$$

Symmetrische Matrizen sind eigentlich gar nicht so selten. Z.B. sind
Kovarianzmatrizen oder Adjazenzmatrizen ungerichteter Graphen symmetrisch. Es
ist also sinnvoll, sie zu untersuchen.

Um nachzuweisen, dass jede symmetrische, relle Matrix diagonalisierbar ist,
muessen wir zuerst drei Hilfsaussagen zeigen.

## Erstes Lemma

Sei $\lambda \in \mathbb{C}$ ein Eigenwert von $A$. Wir zeigen: $\lambda$ ist
sogar immer in $\mathbb{R}$ (reell), sodass $A$ diagonalisierbar ist (wir haben
$A$ rellwertig definiert, also muss es ueber $\mathbb{R}$ in Linearfaktoren
zerfallen). Wir nehmen hierbei an, dass $A$ nur reelle Werte hat, aber behandeln
sie so, als ob sie ueber den komplexen Zahlen definiert ist. Betrachten wir
hierfuer den Eigenvektor $v \in \mathbb{C}^n$ zu $\lambda$ sowie den konjugiert
komplexen Vektor $\bar{v} \in \mathbb{C}$ (mit konjugiert komplexen
Eintraegen). Dann zeigen wir zunaechst, dass $\bar{v}$ gerade um $\bar{\lambda}$
skaliert wird:

1. $A\bar{v}$
2. $\bar{Av}$ (weil $A$ nur relle Werte enthaelt, sodass die Multiplikation mit
   $A$ die imaginaeren Teile nicht beeinflusst)
3. $\bar{\lambda v}$
4. $\bar{\lambda} \cdot \bar{v}$

Wenn $A$ seinen Eigenvektor $v$ also um $\lambda$ skaliert, so skaliert $A$ den
konjugierten Vektor $\bar{v}$ um $\bar{\lambda}$. Dann koennen wir nun die
Aussage zeigen, dass $\lambda$ rell ist:

1. $\bar{\lambda} \cdot (\bar{v}^\top v)$
2. $(\bar{v}^\top \bar{\lambda}) \cdot v$
3. $(\bar{\lambda} \bar{v})^\top \cdot v$
4. $(A \bar{v})^\top v$
5. $\bar{v}^\top A^\top v$
6. $\bar{v}^\top A v$ ($A$ ist symmetrisch)
7. $\bar{v}^\top \lambda v$
5. $\lambda \bar{v}^\top v$ ($\lambda$ ist als Skalar kommutierbar)

Insgesamt haben wir also:

$$\bar{\lambda} (\bar{v}^\top v) = \lambda (\bar{v}^\top v)$$

Diese Gleichheit $\bar{\lambda} = \lambda$ gilt genau dann, wenn $\lambda \in
\mathbb{R}$. Wir wissen nun also, wenn $\lambda$ ein Eigenwert dieser
symmetrischen Matrix ist, so muss $\lambda$ rell sein. $A$ zerfaellt also in
relle Eigenwerte, was schonmal eine Anforderung fuer Diagonalisierbarkeit ist.

Wir wissen also: __Alle Eigenwerte einer symmetrischen Matrix sind reell__.

## Zweites Lemma

Sei $U \subseteq \mathbb{R}^n$ ein Unterraum mit $U \neq \{0\}$ (nicht gleich
dem Nullraum), sodass fuer alle Vektoren $u \in U$ gilt: $Au \in U$. Das waere
also *Abgeschlossenheit ueber $A$*. Wir zeigen: *Dann enthaelt $U$ einen
Eigenvektor von $A$*.

In Worten nochmal, was wir zeigen wollen: Fuer jeden Unterraum des
$\mathbb{R}^n$ zeigen wir, dass wenn dieser Unterraum bezueglich der
Matrixmultiplikation __mit $A$__ abgeschlossen ist, dann muss dieser Unterraum
einen Eigevektor von $A$ enthalten.

Da nach Voraussetzung der Unterraum $U$ bezueglich der Matrixmultiplikation mit
$A$ abgeschlossen ist, duerfen wir also eine lineare Abbildung $f_A: U
\rightarrow U$ erstellen, die einen Vektor eben mit $A$ multipliziert, also $u
\mapsto Au$. Wir wissen ja sicher, dass das Bild dieser Funktion $\subseteq U$
ist und somit jeder Vektor $f_A(v)$ wieder im selben Raum $U$ sein muss. Dann
koennen wir nun eine Darstellungsmatrix zu $f_A$ bilden und mit dieser lustige
Sachen machen.

Sei dann $B = \{b_1, ..., b_k\}$ eine Basis von $U$ und bezueglich dieser Basis
dann $C = (c_{i,j}) = D_B(f_A) \in \mathbb{R}^{k \times k}$ die entsprechende
Darstellungsmatrix. Sei im Weiteren $\lambda \in \mathbb{C}$ eine Nullstelle des
charakteristischen Polynoms dieser Darstellungsmatrix $\Chi_C$, also ein
Eigenwert von $C$.

Mit einem zugehoerigen Eigenvektor $v = (x_1, ..., x_k)^\top$ von $C$ gilt also,
dass wenn wir die Matrix anwenden, also $C \cdot (x_1, ..., x_k)^\top$, dann ist
das ja genau $\lambda (x_1, ..., x_k)^\top$, weil $C = D_B(f_A)$ die Matrix $A$
auf den Eigenvektor anwendet. Wir nehmen hier vorerst an, dass $v$
komplexwertig, also in $\mathbb{C}^n$ ist. Der Vektor $v$ laesst sich weiters
natuerlich auch als Linearkombination der Basisvektoren $\in B$ darstellen, also

$$v = \sum_{i=1}^k x_i b_i \in \mathbb{C}^n .$$

Hierbei sind die Koeffizienten der Linearkombination gerade die Komponenten von
$v$, weil wir annehmen, dass $v$ schon in Koordinaten von $B$ angegeben
ist. Dieser Eigenvektor $v$ kann nicht Null sein, da nicht alle $x_i$ Null sind
(der Nullvektor ist kein Eigenvektor) und die $b_i$ als Basis linear unabhaengig
sind. Dann gibt es naemlich nur die triviale Darstellung der Null. D.h. $v$
koennte fuer diese Basisvektoren nur dann Null sein, wenn die $x_i$ Null
sind. Da das nicht zutrifft, muss $v \neq 0$ sein. Dann formen wir ein wenig um:

1. $Av = A\sum_{i=1}^k x_i b_i$
2. $\sum_{i=1}^k A x_i b_i$ (ziehen $A$ nach innen)
2. $\sum_{i=1}^k x_i f_A(b_i)$ ($f_A$ multipliziert mit $A$)
3. $\sum_{i=1}^k x_i (\sum_{j=1}^k c_{j,i} \cdot b_j)$
4. $\sum_{j=1}^k (\sum_{i=1}^k x_i c_{j,k}) b_i$ (Distributivitaet)
5. $\sum_{j=1}^k (\lambda x_j) b_j$ (Anwendung der Darstellungsmatrix)
6. $\lambda \sum_{j=1}^k x_j b_j$ (Definition von $v$ als Linearkombination)
7. $\lambda v$

Besonders die Schritte (2) $\rightarrow$ (3) und (4) $\rightarrow$ (5) sind
interessant. Im ersten Uebergang nutzen wir aus, dass $f(b_i)$ einen der
Basisvektoren durch die Abbildung schiebt. Bei der Darstellungsmatrix tun wir
das ja auch: wir bilden jeden Basisvektor ab und notieren die
Koordinaten. D.h. wir kennen das Bild von $b_i$ schon dadurch, dass wir fuer den
$i$-ten Basisvektor ueber alle $j$ Zeilen iterieren, und jeweils den Eintrag
$c_{j,i}$ in dieser Zeile (die ja nur fuer diesen Basisvektor ist) mit den
Basisvektoren multiplizieren (da wir in die selbe Basis abbilden).

Der Uebergang von $(4)$ auf $(5)$ ist noch interessanter. Wieso ist
$\sum_{i=1}^k c_{j,i}x_i$ gerade gleich $\lambda x_j$? Hierzu ueberlegen wir
uns: Wir iterieren hier ueber die $j$-te Zeile und multiplizieren jeweils mit
der entsprechenden Komponente $x_i$ in unserem Eigenvektor $v$. Wir bilden also
das Skalarprodukt zwischen der Zeile und $v$. Das ist genau der Schritt, den wir
bei der Multiplikation $Av$ machen wuerden, da $c_{j,i}$ als Darstellungsmatrix
die Funktion $f_A$ nachbildet. D.h. wir erhalten aus $\sum_{i=1}^k c_{j,i}x_i$
gerade die $j$-te Komponente des Resultats aus $Av$.

Gut, wieso ist dieser aber nun $\lambda x_j$, wo $x_j$ also sogar die
urspruengliche Komponente in der $j$-ten Zeile ist? Nun ja, als Eigenvektor wird
$v$ von $A$ ja um den Faktor $\lambda$ skaliert. D.h. nichts anderes, als dass
jede einzelne Komponente um $\lambda$ skaliert wird. Da wir die $j$-te
Komponenten durch diese Summe bilden, erhalten wir also genau $\lambda x_j$. Das
ist ja genau die schoene Eigenschaft von Eigenvektoren und Eigenvektoren (anders
ausgedrueckt): das Skalarprodukt der $i$-ten Zeile aus $A$ mit dem ganzen Vektor
$v$ ergibt genau $\lambda$ mal diesen $i$-ten Eintrag im Vektor!

Wir haben also nun gezeigt, dass wenn $v$ ein Eigenvektor dieser
Darstellungsmatrix $C$ ist, er sogar auch ein Eigenvektor von $A$ ist. D.h. dass
$\lambda$ ist dann auch Eigenwert zu $v$ von $C$ und $A$ ist (eigentlich klar,
weil $A$ und $C$ aehnlich sind). Da $A$ symmetrisch ist, muss $\lambda$ des
Weiteren auch reell sein, also $\lambda in \mathbb{R}$.

Da die Multiplikation mit $A$ einen Vektor $\in U$ produziert, ist also $Av =
\lambda v \in U$. Da wir schon gezeigt haben, dass $\lambda$ rell ist, und da
also das Produkt $\lambda v$ rell ist (weil $U$ Unterraum von $\mathbb{R}^n$)
ist, kann $v$ nicht irrell sein. Sonst waere das ganze Produkt ja nicht in $U$
(was aber nach Voraussetzung des Raumes bzw. $Au$ sicher gilt). Auch wissen wir,
dass $v$ vorher schon in $U$ gewesen sein muss, weil wir es ja durch die
Basisvektoren $b_i \in B \subset U$ darstellen konnten und jetzt auch wissen,
dass die entsprechenden Koeffizienten rell sind.

Wir haben also nun $v$ als den gesuchten rellen Eigenvektor von $A$ mit $av \in U$.

Insgesamt haben wir also nun die Aussage, dass wenn ein Unterraum $U$, der nicht
der Nullraum $\{0\}$ ist, bezueglich der Multiplikation mit $A$ abgeschlossen
ist, dieser Raum $U$ auch sicher einen Eigenvektor von $A$ enthaelt.

__Wenn ein Unterraum bezueglich $Av$ abgeschlossen ist, so enthaelt er einen
Eigenvektor von $A$__.

## Drittes Lemma

Als drittes und letztes Lemma zeigen wir nun, dass Eigenvektoren aus
unterschiedlichen Eigenraeumen (also zu unterschiedlichen Eigenwerten) immer
*senkrecht* aufeinander stehen, *wenn* die Matrix symmetrisch ist. Haben wir
also zwei Eigenwerte $\lambda, \mu$ einer symmetrischen Matrix $A$, dann gilt
fuer alle Eigenvektoren aus den entsprechenden Eigenraeumen, also $v \in
E_\lambda$ und $w \in E_\mu$, dass ihr Skalarprodukt null sein muss: $\langle v,
w \rangle = 0$.

Fuer den Beweis zeigen wir zunaechst, dass fuer symmetrische Matrizen $A$ gilt:

$$\lambda langle v, w \rangle = \mu \langle v, w \rangle.$$

Dann wuerde naemlich gelten, dass $\lambda v^\top w - \mu v^\top w =
0$. Insbesondere muss dann fuer die obige Gleichheit $\langle v, w \rangle = 0$
sein, da wir $\lambda, \mu$ unterscheidbar gewaehlt haben. Das koennen wir
beweisen, denn:

1. $\lambda v^\top w - \mu v^\top w = 0$
2. $(v \lambda^\top)^\top w - v^\top (\mu w) = 0$
3. $(\lambda v)^\top w - v^\top (\mu w) $
4. $(Av)^\top w - v^\top (Aw)$
5. $v^\top A^\top w - v^\top A w$
6. $v^top A w - v^\top A w$ (Da $A^\top = A$)
7. $0$

__Eigenvektoren zu unterschiedlichen Eigenwerten stehen senkrecht also
zueinander, wenn $A$ symmetrisch.__

## Satz

Wir wissen jetzt also durch unsere Vorarbeit, dass wenn $A$ symmetrisch ist:

1. $A$ hat nur relle Eigenwerte.
2. Wenn $\{0\} \neq U \subset \mathbb{R}^n$ bezueglich $Av$ abgeschlossen ist,
   enthaelt $U$ einen Eigenvektor von $A$.
3. Eigenvektoren von $A$ stehen senkrecht zueinander.

Nun der eigentliche Satz zur Hauptachsentransformation:

__Fuer jede symmetrische Matrix $A \in \mathbb{R}^{n \times n}$ gibt es eine
Orthonormalbasis des $\mathbb{R}^n$, die aus Eigenvektoren besteht__.

Was dieser Satz uns eigentlich (auch) sagen wuerde, ist dass jede symmetrische
Matrix diagonalisierbar ist. Denn wenn wir fuer jede symmetrische Matrix eine
Basis fuer den ganzen Raum, also $\mathbb{R}^n$, finden koennen, dann haben wir
ja gerade die Eigenschaft, die Diagonalisierbarkeit ausmacht. Dass diese Basis
*zusaetzlich* noch orthonormal ist, macht die Aussage nur noch staerker. Es
gaebe dann also eine orthonormale Basis $S \in O_n(\mathbb{R})$ gibt, sodass
$S^{-1}AS = S^\top A S$ eine Diagonalmatrix ist. Denn durch diese Formel wissen
wir ja, dass wenn wir eine Basiswechselmatrix (oder eben einfach eine
Basismatrix, wenn wir in den Standardraum wechseln) $S$ aus Eigenvektoren haben
dann als Resultat von $S^{-1}AS$ eine Diagonalmatrix erhalten. Also zeigen wir
eigentlich, aehnlich zu einer Diagonalmatrix ist.

### Beweis

Seien $\lambda_1, ..., \lambda_r \in \mathbb{R}$ die verschiedenen Eigenwerte
von $A$. Fuer jeden Eigenwert $\lambda_i$ existiert eine Orthonormalbasis $B_i$
von $E_{\lambda_i}$. Das ist so, weil $E_{\lambda_i}$ ein Unterraum des
$\mathbb{R}^n$ (betrachte immer den Raum der Spalten bzw. der Matrix um den
Ueberraum zu bestimmen). Wir koennen ja dann eine Orthonormalbasis durch das
Gram-Schmidt Verfahren finden. Dann haben wir also fuer jeden Eigenwert
$\lambda_i$ eine solche orthonormale Basis $B_i$. Wir setzen nun $B = B_i \uplus
... \uplus B_r$.

Wir haben also eine Menge von orthonormalen Basisvektoren $B_i$, welche alle
jeweils untereinander orthonormal sind (also pro $B_i$). Da die Basisvektoren
jedes $B_i$ aber des Weiteren Eigenvektoren sind, und wir wissen, dass
Eigenvektoren zu verschiedenen Eigenraeumen immer orthogonal sind (wenn $A$, wie
hier, symmetrisch), sind also sogar alle Vektoren in $B$ orthonormal. Wir haben
also eine Menge $B$ von orthonormalen Basisvektoren. Insofern ist $B$ sicher
Basis irgendeines Unterraumes $U = \langle B \rangle$. Wir muessen also noch
zeigen, dass $U$ gerade $\mathbb{R}^n$ ist. Genauer: Unser Ziel ist es, zu
zeigen, dass wir $n$ Basisvektoren haben, also dass $k = n$. Dann haetten wir
naemlich eine Orthonormalbasis gefunden, die den ganzen $\mathbb{R}^n$
aufspannt.

Nehmen wir nun einen Vektor $w \in U^\perp$, also aus dem orthogonalen
Komplement von $U$. Wir zeigen: die Multiplikation $Aw$ unserer symmetrischen
Matrix mit diesem Vektor $w$ ist wieder $\in U^\perp$. Somit waere $U^\perp$
also bezueglich der Multiplikation mit $A$ abgeschlossen. Ist $U$ dann auch
nicht der Nullraum, so wuerden wir aus dem zweiten Lemma folgern koennen, dass
$U$ einen Eigenvektor von $A$ enthalten muss. Um das nachzuweisen, genuegt es zu
zeigen, dass $Aw$ auf alle Basisvektoren $v \in B$ senkrecht steht. Sei also $v$
ein Basisvektor aus einem unserer orthonormalen Basisraeume $B_i$, mit $i \in
\{1, ..., r\}$. Dann sollte also $Aw$ also auf $v$ senkrecht stehen, somit
$\langle v, Aw \rangle = 0$ sein. Das zeigen wir so:

1. $\langle v, Aw \rangle = v^\top Aw$
2. $v^\top A^\top w$ ($A$ symmetrisch)
3. $(Av)^\top w$
4. $(\lambda_i v)^\top w$ ($v$ war ja aus dem Eigenraum von $\lambda_i$)
5. $\lambda_i v^\top w$
6. $\lambda_i \langle v, w \rangle = 0$

Der Schritt von $A$ auf $\lambda_i$ ist wichtig, weil wir $\lambda$ als Skalar
um $v^\top$ kommutieren koennen. Da $w$ aus dem orthgonalen Komplement von $U$
gewaehlt war, sodass $w$ also senkrecht zu allen Vektoren in $U$ (insbesondere
auch den Basisvektoren dieses Raumes), ist $\langle v, w \rangle = 0$.

Wir haben also, dass das orthgonale Komplement $U^\perp$ bezueglich der
Multiplikation mit $A$ abgeschlossen ist. Wenn nun noch gelten wuerde, dass
$U^\perp \neq \{0\}$, dann wuerden wir in $U^\perp$ nach dem zweiten Lemma also
auch einen Eigenvektor $v \in \mathbb{U}^\perp$ von $A$ finden. Da $U$ von einer
Eigenvektorbasis aufgespannt wird, ist insbesondere jeder moegliche Eigenvektor
$v$ $A$ auch in $U$, weil dieses Basis jeden Eigenvektor erzeugen kann, und $U$
das gesamte Erzeugnis enthaelt. Wenn also $U$ jeden Eigenvektor enthaelt, kann
das orthgonale Komplement $U^\perp$ mit Sicherheit keinen Eigenvektor
enthalten. Somit ist $U^\perp$ also bezueglich der Mutliplikation mit $A$
abgeschlossen, aber enthaelt dennoch keinen Eigenvektor von $A$. Das kann nur
genau dann der Fall sein, wenn $U^\perp$ der Nullraum ist!

Wir haben also unsere Basisvektoren $B = \{b_1, ..., b_k\}$. Wenn wir diese
Vektoren nun in die Zeilen einer Matrix $C \in \mathbb{k \times n}$ geben, und
mit einem Vektor $x$ multiplizieren, dann multiplizieren wir ja der Reihe nach
jede Zeile in $C$ (also jeden Basisvektor in $B$) mit diesem anderen Vektor
$x$. Ist ein solches Skalarprodukt einer Zeile mit $x$ Null, so steht $x$ auf
diese Zeile bzw. diesen Basisvektor senkrecht. Erhalten wir also bei der
Multiplikation $Cx$ den Nullvektor, so muss $x$ auf alle Basisvektoren senkrecht
stehen. Betrachten wir $Cx$ als LGS, finden wir gerade so alle zu $U$
orthogonalen Vektoren (bzw. als $\Kern(C)$). Der Loesungsraum des homogenen LGS
$Cx = 0$ ist also gerade $U^\top$. Da $U^\perp = \{0\}$ folgt nun sogar, dass
das LGS eindeutig loesbar ist (nur die triviale Loesung hat). $C$ ist also
regulaer (hat vollen Rang) und muss somit *insbesondere* $n$ Zeilen haben, denn
sonst koennten wir nie $n$ Variablen (fuer die $n$ unbekannten Komponenten der
komplementaeren Vektoren) aufloesen. Wir haben mit $B$ also eine linear
unabhaengige Teilmenge des $\mathbb{R}^n$ mit $n = \dim(\mathbb{R}^n)$ vielen
Vektoren. Dann muss $B$ eine Basis des ganzen Raumes sein.

Wir haben also nun eine Basis des $\mathbb{R}^n$. Wir wussten bereits, dass die
Basis orthonormal ist. Wir haben also insgesamt gezeigt, dass man fuer jede
symmetrische Matrix $A \in \mathbb{R}^{n \times n}$ eine Orthonormalbasis des
$\mathbb{R}^n$ findet. $\qed$

Anders ausgedrueckt finden wir durch den Hauptachsentransformationssatz eine
orthonormale Basiswechselmatrix $S$ (die Basis aus orthonormalen Eigenvektoren),
sodass $S^\top A S = \diag(\lambda_1, ..., \lambda_n)$, da $\diag(\lambda_1,
..., \lambda_n)$ ja immer die Diagonalisierung einer Matrix ist.

## Beispiel

Sei die symmetrische Matrix $A \in \mathbb{R}^{3 \times 3}$ gegeben durch:

$$
A =
\begin{bmatrix}
2 & 1 & 1 \\
1 & 2 & 1 \\
1 & 1 & 2
\end{bmatrix}
$$

Wir suchen eine Diagonalisierung dieser Matrix. Nach dem
Hauptachsentransformationssatz wissen wir, dass es fuer jede symmetrische Matrix
$A$ eine Orthonormalbasis (bestehend aus Eigenvektoren von $A$) des Raumes, also
$\mathbb{R}^3$, gibt. Haben wir diese gefunden, koennen wir ueber $S^{-1}AS =
S^{\top}AS$ ($S^{-1} = S^\top$, da $S$ orthonormal) die Diagonalisierung finden!

Natuerlich koennen wir zunaechst die Diagonalisierung von $A$ einfach ueber die
Eigenwerte finden. Denn die resultierende Diagonalmatrix hat ja die Eigenwerte
in der Diagonalen. Wir bestimmen also einfach das charakteristische Polynom
$\chi_A$ und erhalten dann eine Linearfaktorisierung $(x - 1)^2(x - 4)$. Dann
hat man also Eigenwerte $1$ und $4$ mit algebraischen Vielfachheiten $m_a(1) =
2$ und $\m_a(4) = 1$. Somit ist $A$ also aehnlich zur Diagonalmatrix $\diag(1,
1, 4)$.

Wir suchen jetzt aber noch die orthogonale Basiswechselmatrix
(Transformationsmatrix) bzw. die Orthonormalbasis des $\mathbb{R}^3$. Diese
bestimmen wir also, indem wir die Basisvektoren der Eigenraeume der Matrix $A$
bestimmen, orthogonalisieren und vereinigen.

Wir loesen also zuerst das homogene LGS $(A - 1 \cdot I_3)x = 0$ um die
Basisvektoren des Eigenraumes von $E_1$ zu finden. Diese sind hier $\langle (1,
0, -1)^\top, (1, -1, 0)^\top \rangle$, welche aber noch nicht orthonormal
sind. Wir wenden also das Gram-Schmidt Verfahren an, welches jedes
Erzeugendensystem eines Unterraumes des $\mathbb{R}^n$ in eine Orthonormalbasis
konvertiert.

Hierfuer nehmen wir den ersten Vektor $(1, 0, -1)^\top$ und normieren ihn
einfach, da er ja der erste Orthonormalvektor ist. Dann erhalten wir fuer $u_1 =
\frac{1}{\sqrt{2}}(1, 0, -1)^\top$. Die anderen werden wir dann bezueglich
dieses Vektors "orthonormalisieren". Sei hierzu $v$ der andere Eigenvektor $(1,
-1, 0)^\top$:

1. $w_2 = v - \sum_{i=1}^m \langle v, u_i \rangle u_i$
2. $w_2 = v - \langle v, \frac{1}{\sqrt{2}}(1, 0, -1)^\top \rangle (1, 0, -1)^\top$
3. $w_2 = v - \frac{1}{\sqrt{2}}(1, 0, -1)^\top$
4. $w_2 = \frac{1}{2}(1, -2, 1)^\top$

Diesen Vektor normieren wir noch und erhalten

$$u_2 = \frac{1}{\sqrt{6}}(1, -2, 1)^\top .$$

Aehnlich erhalten wir so fuer $E_4$ den Loesungsraum $\langle (1, 1, 1)^\top
\rangle$ mit $u_3 = \frac{1}{\sqrt{3}}(1, 1, 1)^\top$.

Wenn wir nun diese drei Vektoren $u_1, u_2, u_3$ in die Spalten einer Matrix
geben, erhalten wir genau die gesuchte Matrix $S$. Mit dieser kommen wir dann
ueber $S^{-1}AS$ wieder auf $\diag(1, 1, 4)$.

Man beachte, dass der Hauptachsentransformationssatz nur fuer den rellen Raum
funktioniert. Fuer den komplexen Raum, also Matrizen $A \in \mathbb{C}^{m \times
n}$, gilt der Satz nicht mehr.

## Definitheit

Hat man eine symmetrische Matrix $A \in \mathbb{R}^{n \times n}$, so nennt man
diese:

* *positiv definit*, falls $A$ nur positive Eigenwerte hat.
* *positiv semidefinit*, falls $A$ nur nicht-negative Eigenwerte hat.
* *negativ definit*, falls $A$ nur negative Eigenwerte hat.
* *negativ semidefinit*, falls $A$ nur nicht-positive Eigenwerte hat.
* *indefinit*, falls $A$ sowohl positive als auch negative Eigenwerte hat.

Weiters wissen wir fuer eine symmetrische Matrix, dass diese genau dann positiv
definit, wenn fuer alle Vektoren $v \in \mathbb{R}^n\setminus \{0\}$ (alle
ausser dem Nullvektor) gilt, dass $\langle v, Av \rangle$ positiv ist. Aehnlich
muss fuer negativ semidefinite symmetrische Matrizen dann $\langle v, Av \rangle
\leq 0$ sein und analog fuer die anderen Varianten. Das moechten wir beweisen.

### Beweis

Die Matrix $A$ ist symmetrisch. Dann wissen wir also nach dem
Hauptachsentransformationssatz, dass es eine orthonormale Matrix $S \in
O_n(\mathbb{R})$ gibt, sodass $S^\top A S = \diag(\lambda_1, ..., \lambda_n) =
D$ ist. $S$ ist dann eben eine orthonormale Basis des Raumes $\mathbb{R}^n$, die
aus Eigenvektoren von $A$ besteht. Weil $S$ bzw. $S^{-1} = S^\top$ (da $S$
orthonormal) invertierbar ist ($S \in \GL_n(\mathbb{R})$), und $S$ auch
quadratisch ist, folgt aus den vielen Aequivalenzen der linearen Algebra, dass
die Abbildung $f_S$ und insbesondere $f_{S}^{-1} = f_{S^{-1}} = f_{S^\top}$
bijektiv ist.

Dann ist diese Abbildung $f_{S^\top}$ also auch injektiv und ihr Kern somit nur
die Null. Dann gilt also fuer jeden Vektor $v \in \mathbb{R}^n \setminus \{0\}$,
der also nicht der Nullvektor ist, dass $S^\top v \neq 0$, da eben nur $v = 0$
im Kern ist, also auf die Null abgebildet wird. Da die Funktion durch
Bijektivitaet im Weiteren auch surjektiv ist, tritt auch jeder Vektor aus
$\mathbb{R}^n\setminus \{0\}$ als das Ergebnis von $S^\top v$ fuer geeignetes
$v$ auf (die inverse Funktion, also $f_S$, findet diesen Vektor genau).

Sei im Weiteren $v$ ein Vektor und $w = (x_1, ..., x_n)^\top$ das Bild von $v$
bezueglich $S^\top$, also $w = S^\top w$. Dann gelten folgende Schritte:

1. $\langle v, Av \rangle$
2. $v^\top Av$
3. $v^\top (SDS^\top) v$ (Umformung von $D = S^\top A S$)
4. $(v^\top S) D (S^\top v)$ (Assoziativitaet)
5. $(S^\top v)^\top D (S^\top v)$
6. $w^\top D w$
7. $w^\top (\lambda_1 x_1, ..., \lambda_n x_n)^\top$
8. $\sum_{i=1}^n \lambda_i x_i^2$

Im Schritt von (6) auf (7) multiplizieren wir die Diagonalmatrix $D$ mit dem
Vektor $w$. Da $D$ eine Diagonalmatrix ist, ist die Multiplikation mit ihr eine
Skalierung jeder einzelnen Komponente. Sie ist ja wie die Identitaetsmatrix,
wobei aber nicht mit $1$ sondern jeweils (weil $D = \diag(\lambda_1, ...,
\lambda_n)$) mit dem Eigenvektor $\lambda_i$ in der $i$-ten Zeile
bzw. Komponente. Von (7) auf (8) bilden wir dann nochmals das Skalarprodukt von
$w$ mit diesem komponentenweise skalierten Vektor. Hierbei multiplizieren
jeweils $x_i$ mit $\lambda_i x_i$ und erhalten dadurch jeweils $\lambda_i
x_i^2$. Da es sich um ein Skalarprodukt handelt, haben wir dann noch die Summe
daraus.

Da wir nun $x_i$ im Quadrat haben und es somit immer positiv sein muss,
bestimmen also alleine die Eigenwerte $\lambda_i$ das Vorzeichen der Summe (das
positive Vorzeichen von $x_i$ ist sozusagen neutral gegenueber dem Vorzeichen
einer Multiplikation). Daraus folgen dann also die verschiedenen Faelle. Ist die
Matrix $A$ z.B. positiv semidefinit, sodass ihre Eigenwerte alle groesser gleich
Null sind. dann muss hier also eben auch $\langle v, Av \rangle = \sum_{i=1}^n
\lambda_i x_i$ groesser gleich Null sein. Wir haben also hiermit gezeigt, dass
sich die Eigenschaft der Definitheit sich alleine aus $\langle v, Av \rangle$
rauslesen laesst.

Was wir hieraus haben ist eine einfache Moeglichkeit, ohne die genauen
Eigenwerte zu bestimmen, schon das Vorzeichen der Eigenwerte zu erhalten. Wenn
ich also wissen will, ob all Eigenwerte positiv sind, muss ich nur $\langle v,
Av \rangle$ mit einem *beliebigen* Vektor ausrechnen. Ein beliebiger Vektor
genuegt wegen der "genau dann wenn" Beziehung. (Eigentlich wuesste ich zumindest
dass er positiv semidefinit ist, noch nicht ganz positiv, ich muesste also noch
pruefen ob ich einen Vektor $v \neq 0$ finde, sodass $\langle v, Av \rangle =
0$, sodass $v$ also orthogonal zu $Av$ ist).

## Zusammenfassung

Hier die in diesem Kapitel besprochenen Eigenschaften

### Symmetrische Matrizen

Sei $A$ eine symmetrische Matrix $\in \mathbb{R}^{n \times n}$, dann gilt:

* $A$ zerfaellt in relle Eigenwerte.
* Eigenvektoren von $A$ zu verschiedenen Eigenwerten sind orthogonal.
* Ist ein Unterraum $U \neq \{0\}$ bezueglich Multiplikation mit $A$
  abgeschlossen, so enthaelt $U$ einen Eigenvektor von $A$.

### Hauptachsentransformationssatz

* Es gibt eine Orthonormalbasis des $\mathbb{R}^n$ mit Eigenvektoren von $A$.
* $A$ ist transformierbar in eine orthonormale Diagonalmatrix.

### Definitheit

Sei $A$ eine symmetrische Matrix $\in \mathbb{R}^{n \times n}$ und $\lambda$ ein
*beliebiger* Eigenwert von $A$, dann nennt man $A$:

* *positiv definit*,      falls $\lambda > 0$
* *positiv semi-definit*, falls $\lambda \geq 0$
* *negativ semi-definit*, falls $\lambda \leq 0$
* *negativ definit*,      falls $\lambda < 0$
* *indefinit*,            wenn $\lambda > 0$ und $\lambda < 0$ moeglich

Sei $\circ$ eine Relation aus $\{>, \geq, \leq, <\}$ und $v \neq 0$ ein
beliebiger Vektor, dann gilt:

$$\lambda \circ 0 \iff \langle v, Av \rangle \circ 0$$
