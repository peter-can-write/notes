# Praedikatenlogik

In der Aussagenlogik fehlt es uns an Moeglichkeiten, Aussagen ueber mehr als
eine Variable bzw. mehr als ein Objekt zu treffen. Wir koennen zum Beispiel
nicht formulieren, dass eine Aussage wahr ist, "wenn es ein $y$ gibt, sodass
alle $x \in X$ gleich $y$ sind".

Daher erweitert die Praedikatenlogik die Aussagenlogik um Vokabular und
Formationsregeln, um drei Konzepte zu realisieren:

* "fuer alle": die Aussage ist nur wahr, wenn sie fuer alle Elemente (einer
  Menge) wahr sind.

* "es existiert": Die Aussage ist nur wahr, wenn es ein $x$ mit einer bestimmten
  Eigenschaft gibt.

* "ist gleich": Die Aussage ist nur wahr, wenn das eine Element gleich dem
  anderen ist.

Weiters gibt es in der Praedikatenlogik sogenannte *Praedikate*. *Praedikate*
repraesentieren Eigenschaften oder Aussagen ueber Objekte, und evaluieren, ob
ein *konkretes* Objekt die Eigenschaft besitzt, oder nicht. Ein Praedikat ist
eine Menge von Tupeln von Individuen, die (zusammen) die Eigenschaft besitzen.

Man nehme beispielsweise den Satz "Sokrates ist sterblich". In diesem Satz ist
"Sokrates" das Subjekt und "ist sterblich" das Praedikat (keine grammatikalische
Bedeutung). Nun koennen wir ein Praedikat $P(x)$ definieren, dass fuer
Individuen bestimmt, ob sie sterblich sind, oder nicht:

$P(x) = \text{"x ist sterblich"}$.

$P(\text{Sokrates})$ wuerde also $true$ zurueckgeben.

### Aritaet

Ein Praedikat $P(x)$ das fuer *genau ein* Individum bestimmt, ob es eine
Eigenschaft hat, oder nicht, nennt man *einstellig*es Praedikat. Natuerlich gibt
es aber auch Praedikate, die mehr Argumente nehmen. Allgemein sprechen wir hier
von der *Aritaet* des Praedikats. $P(x)$ hat die Aritaet $1$.

Eine Praedikat $P(x, y)$ haette beispielsweise die Aritaet $2$ und bestimmt fuer
zwei Argumente, ob eine Eigenschaft fuer sie gilt, oder nicht. Die Eigenschaft
koennte zum Beispiel sein: "$x$ liebt $y$".

Fuer sowohl Praedikate als auch Funktionen (siehe unten), also allgemein
*Symbole*, gilt, dass man oft die Aritaet des Symbols ueber der Bezeichnung
notiert. So schreibt man ein $k$-stelliges Praedikat oft als $P^k$ und eine
$j$-stellige Funktion als $f^j$.

Wenn in einer Formel das selbe Symbol (Praedikat/Funktion), z.B. $P$, zweimal
oder oefter vorkommt, aber nicht immer mit der selben Aritaet, z.B. $P(x) \land
P(x, y)$, dann ist diese Formel __nicht gueltig__.

### Funktionen

Eine Funktion ist in der Praedikatenlogik eine Abbildung von *Individuen auf
Individuen*. Sei $f(x)$ beispielsweise eine Funktion, die einen Menschen auf
seine/ihre Mutter abbildet. Dann ist $f(\text{Ares}) = \text{Hera}$.

Auch eine Funktion kann mehrstellig sein, also eine Aritaet $> 1$ haben.

### Syntax

Hier nun die Beschreibung der formalen Syntax der Praedikatenlogik,
einschliesslich Vokabular und Formationsregeln.

#### Vokabular

Die Syntax der Praedikatenlogik ist eine Erweiterung der Syntax der
Aussagenlogik. Das bedeutet, dass alle erlaubten Variablen, Operatoren, die zwei
Bool'schen Wahrheitskonstanten sowie die Hilfssymbole der Aussagenlogik, auch in
der Syntax der Praedikatenlogik enthalten sind:

* Wahrheitskonstanten (atomare Aussagen): $true, false$
* Aussagenvariablen (Platzhalter): $p, q, r, s, t, ...$
* Logische Operatoren (Junktoren): $\neg, \land \lor, \rightarrow$
* Hilfssymbole (Klammern): $(, )$

Und so erweitert die Praedikatenlogik:

* Konstanten: $a, b, c$ (z.B. $\pi$)

* Praedikatensymbole: $P^k, Q^l, R^m, ...$
* Funktionssymbole: $f^k, g^l, h^m$

* Das Gleichheitsymbol: $=$ (!)

* __Quantoren__:
  + $\forall$: "Fuer alle"; *Allquantor*.
  + $\exists$: "Es existiert"; *Existenzquantor*.

### Formationsregeln

Auch erweitert die Praedikatenlogik die Aussagenlogik um einige
Formationsregeln. Insbesondere ist eine Variable alleine keine Formel mehr,
sondern erst ein *Term*.

1. Regel: Jede Variable und jede Wahrheitskonstante ist ein *Term*.

2. Regel: Sind $t_1,...,t_n$ Terme, dann ist $f^n(t_1,...,t_n)$ ebenfalls ein
   *Term*.

3. Regel: Sind $t_1,...,t_n$ Terme, dann ist $P^n(t_1,...,t_n)$ eine *Formel*.

4. Regel: Sind $t$ und $u$ Terme, dann ist $t = u$ eine Formel.

5. Regel: Ist $F$ eine Formel, dann ist auch $\neg F$ eine Formel.

6. Regel: Sind $F, G$ Formeln, dann sind $F \land G, F \lor G, F \rightarrow G$
   ebenfalls Formeln.

7. Regel: Ist $x$ eine Variable und $F$ eine Formel, dann sind $\forall x F$ und
   $\exists x F$ ebenfalls Formeln (z.B. $\forall x P(x)$).

Man beachte besonders, dass Variablen in der Praedikatenlogik nicht mehr fuer
Wahrheitswerte $true/false$ stehen, sondern nun fuer *Individuen* beliebigen
Typs, z.B. Menschen. Daher ist $f(x) \land P(x)$ zum Beispiel keine gueltige
Formel, weil $f(x)$ einen Term liefert, und keinen Wahrheitswert, auf welchem
die Konjunktion definiert ist.

Praedikate duerfen nicht andere Praedikate als Argumente haben, sondern nur
Variablen, Konstanten und Funktionen (Terme). Generell also duerfen sie nur auf
Termen und nicht Formeln ausgewertet werden.

Quantoren binden am staerksten. Siehe Quantorenbindung wie scopes im
programmieren. $\forall x P(x) \land Q(y)$ bindet at $P(x)$, sprich ist
aequivalent zu $(\forall x P(x)) \land Q(y)$ und nicht $\forall x(P(x) \land
Q(y))$.

#### Gueltigkeitsbereich

Der Gueltigkeitsbereich einer Variablen $x$ ist wie sein Scope in der
Programmierung. Es bestimmt also, fuer welche Teile einer Formel die Variable
$x$ eine Bedeutung hat.

Genauer definiert ist der Gueltigkeitsbereich einer Variablen $x$ fuer eine
Formel $F$ die kleinste Teilformel von $F$ der Gestalt $\forall x G$ oder
$\exists x G$, die $x$ enthaelt (benutzt). Findet man also in $F$ eine beliebige
Formel $G$, die zu einem Quantor gebunden ist, der $x$ als Variable hat, dann
ist der Gueltigkeitsbereich der Variablen eben diese Teilformel $G$. Man nennt
eine solche Variable *gebunden*. Gibt es keinen solchen Ausdruck, dann ist der
Gueltigkeitsbereich die gesamte Formel. Eine solche Variable nennt man *frei*.

Eine Formel ohne *freie* Variablen nennt man hierbei *geschlossen*.

In der (voll definierten) Formel $Q(x)$ ist die Variable $x$ frei und die Formel
nicht geschlossen, weil das $x$ nicht an einen Quantor gebunden ist.  In
$\exists x Q(x)$ ist die Variable $x$ gebunden, weil es einen Quantor gibt, der
diese Variable $x$ definiert. Die Formel ist also geschlossen.

In $\forall x \exists y P(x, y) \land Q(x, a, b)$ sind $x$ und $y$ gebundene
Variablen, und $a, b$ freie Variablen. Diese Formel ist also nicht geschlossen.

#### Bereinigung

Es ist normalerweise erlaubt, in der selben Formel eine Variable einmal frei und
einmal gebunden zu haben. In $P(x) \rightarrow \forall x Q(x)$ ist das $x$ bei
$P(x)$ ein anderes als das in $Q(x)$. Aber wie man schon merkt, ist eine solche
Formel schwer zu lesen und zu bearbeiten. Daher gibt es zu jeder Formel eine
aequivalente *bereinigte Formel*, welche folgende Eigenschaften hat:

1. Jeder Quantor definiert eine andere Variable.
2. Keine Variable ist sowohl gebunden als auch frei.

## Struktur

Gegeben eine Formel $\forall x P(x)$. Ist diese Aussage wahr? Wir koennen diese
Frage nicht beantworten, wenn wir nicht wissen, auf welcher Menge von Individuen
die Formel operiert, und welche Bedeutung das Symbol $P$ hat (wer sagt das es
ein Praedikat) ist? Hierfuer brauchen wir eine *Struktur* fuer die Formel.

Eine Struktur in der Praedikatenlogik ist wie eine Belegung in der
Aussagenlogik. War eine Belegung aber noch eine Abbildung von der Menge der
Variablen einer Formel in die Menge der Bool'schen Wahrheitswerte, so ist eine
Struktur nun eine komplexere Funktion, da Individuen nicht zwingen Bool'sche
Wahrheitswerte oder Zahlen sein muessen.

Allgemein besteht eine Struktur aus zwei Teilen:

1. Einer nichtleeren Menge $U_S$, genannt *Universum* (*Domaene, Grundmenge*).
2. Einer Interpretation $I_S$.

$\text{Struktur } S = (\text{Universum } U_S, \text{Interpretation } I_S)$

Das Universum umfasst also alle moeglichen Instanzen von freien und gebundenen
Variablen. Es definiert den *Wertebereich*, auf welchem eine Formel operiert.

### Interpretation

Eine Interpretation ist eine partielle Funktion, die fuer Variablen, Konstanten,
Praedikate und Funktionen der Formel konkrete Elemente des Universums
bestimmt. Hierbei wird:

* Eine freie Variable $x$ auf ein Element $x_S \in U_S$ abgebildet.

* Eine Konstante $a$ auf ein Element $a_S \in U_S$ abgebildet.

* Ein $k$-stelliges Praedikatsymbol $P$ auf eine Menge $P_S \subseteq
  U^k$ abgebildet. $P_S \subseteq U^k$ ist hierbei also eine Teilmenge der
  Menge aller moeglichen $k$-Tupel des Universums. Ein Praedikat ist also die
  Menge aller $k$-Tupel aus $U^k$, die die Eigenschaft besitzen.

* Ein $k$-stelliges Funktionssymbol $f$ auf eine Funktion $f_S: U^k \rightarrow
  U$ abbildet (Eine Funktion bildet Terme auf Terme ab).

Eine Interpretation instanziiert also abstrakte Symbole, Variablen und
Konstanten (Platzhalter) mit konkreten Elementen aus dem Universum

Wie eine Belegung in der Aussagenlogik, muss auch eine Intepretation nicht
zwingend fuer jedes Symbol ein konkretes Element des Universums definieren. Die
Interpretation kann leer sein, oder voellig andere Variablen abbilden. Deswegen
ist eine Interpretaion auch nur eine partielle Funktion.

Daher sagen wir, eine Struktur $S$ *passt* zu einer Formel, wenn die
Interpretation $I_S$:

* Fuer jedes Praedikatsymbol definiert ist.
* Fuer jedes Funktionssymbol definiert ist.
* Fuer jede Konstante definiert ist.
* Fuer jede __freie__ Variable in der Formel definiert ist.

Eine Struktur $S$ passt beispielsweise zur Formel $\forall x P(x, a, y)$, wenn
$a_S, y_S$ und $P_S$ definiert sind. $x$ muss nicht definiert sein, weil es eine
gebundene Variable ist. Es iteriert also einfach ueber alle Individuen des
Universums.

## Semantik

Eine Semantik $[F]$ ist, aehnlich wie in der Aussagenlogik, eine Funktion, die
jede moegliche Struktur $S$ die zu einer Formel $F$ passt auf einen
Wahrheitswert $[F](S)$, also wahr oder falsch, abbildet:

$\text{Semantik } [F]: \text{Struktur } S \rightarrow \{0, 1\}$ bzw. $[F](S) \in
\{0, 1\}$

Beispiel, fuer die Formel: $\forall x (P(x) \rightarrow \exists y Q(x, y))$ kann
es eine Struktur $S = (U, I)$ geben, wobei:

* Das Universum $U$ definiert ist als $\{0, 1, 2\}$.

Und die Interpretation $I$ definiert:

* $P = \{n \in U \, | \, n < 2\}$
* $Q = \{(n, m) \in U^2 \, | \, n \leq m\}$

Moechten nur wir ein Element eines Universums einer Struktur fuer eine Formel
definieren, koennen wir die Notation $S_{x := d}$ verwenden. Diese Struktur
$S_{x := d}$ ist die Struktur, die identisch mit $S$ ist, bis auf die Tatsache,
dass in $S_{x := d}$ $x$ als $d$ definiert ist (damit instanziiert ist; so
interpretiert wird). Dann gilt also: $x_{S_{x := d}} = d$. $S_{x := d}$ ist also
"die Struktur, in der $x$ als $d$ definiert ist".

Eine Formel $F$ ist unter bzw. *in* einer bestimmten Struktur $S$ also *wahr*
($= 1$), wenn $[F](S) = 1$.

Beispiel: fuer $F = \exists x G$ kann gesagt werden dass wenn es ein $d \in U_S$
gibt, sodass $[G](S_{x := d}) = 1$, dann ist $F = 1$, wenn aber $\forall d \in
U_S$ gilt, dass $[G](S_{x := d}) = 0$, dann $F = 0$.


Hier eine Formel, die die Menge aller Zahlen als Universum hat. Wenn man sagen
will, dass es ein $x$ gibt, dass reell ist und fuer welches $F$ gilt, muss man
$\land$ verwenden:

$$\exists x \in \mathbb{R}: F(x) \Leftrightarrow \exists x(Reell(x) \land F)$$

Wenn man aber sagen will, dass fuer alle reellen Zahlen $F$ gilt, muss man
$\rightarrow$ verwenden:

$$\forall x \in \mathbb{R}: F \Leftrightarrow \forall x(Reell(x) \rightarrow
F)$$

So gilt also, fuer alle moeglichen $x$ innerhalb und ausserhalb des Universums:

* Ist $x$ reell, so muss $F(x)$ gelten, damit die Aussage wahr ist.
* Ist $x$ nicht reell, so wird dieses Element uebersprungen, da die Aussage
  wegen "ex falso quodlibet" zu wahr evaluiert.

### Beispiel

Sei $\mathbb{N}$ die Menge aller Strukturen $S = (U_S, I_S)$ mit:

* $U_S = \mathbb{N}_0$ (Universum)
* $null_S = 0$ (Konstante)
* $nach(n) = n + 1$ (Funktion)

Dann ist das Prinzip der vollstaendigen Induktion fuer eine Eigenschaft $P$
definiert als:

$(P(null) \land \forall y(P(y) \rightarrow P(nach(y)))) \rightarrow \forall x
P(x)$

### Eigenschaften

Wie in der Aussagenlogik koennen wir Formeln $F, G, H$ wieder nach ihrer
Gueltigkeit untersuchen. Hierbei ist also:

* Eine Formel $F$ *(allgemein-)gultig*, wenn fuer jede zu $F$ passende Struktur
  $S$ gilt: $[F](S) = 1$. Eine solche Formel nennt man *Tautologie*.

* Eine Formel $F$ ein *Widerspruch*, wenn fuer jede zu $F$ passende Struktur $S$
  gilt: $[F](S) = 0$.

* Eine Formel $F$ *erfuellbar*, wenn zumindest eine passende Struktur $S$
  existiert, sodass $[F](S) = 1$.

* Eine Formel, die fuer alle Strukturen $S$ einer Menge von Strukturen
  $\mathcal{S}$ *gueltig* ist, nennt man $\mathcal{S}$-Tautologie.

* Eine Formel, die fuer alle Strukturen $S$ eine Menge von Strukturen
  $\mathcal{S}$ ein *Widerspruch* ist, nennt man $\mathcal{S}$-Widerspruch.

Fuer zwei Formeln $F, G$ koennen wir auch sagen:

* $F$ und $G$ sind *aequivalent* ($F \equiv G$), wenn fuer jede zu $F$ *und* zu
  $G$ passende Struktur $S$ gilt: $[F](S) = [G](S)$.

* $G$ folgt aus $F$, wenn gilt, dass wenn $F$ wahr ist, $G$ sicher auch immer
  wahr ist. Die Implikation $F \rightarrow G$ ist dann also *gueltig*.

## Regeln

Hier nun aufgelistet die Aequivalenzregeln fuer die Praedikatenlogik,
insbesondere fuer die Quantoren. Die Aequivalenzregeln der Aussagenlogik
gelten weiter.

### DeMorgan

1. $\neg \forall x F = \exists x \neg F$; "wenn die Formel nicht fuer alle gilt,
   gibt es zumindest eines, fuer welche die Formel nicht gilt".

2. $\neg \exists x F = \forall x \neg F$; "wenn es kein Element gibt, fuer
   welches die Formel gilt, gilt sie fuer alle Elemente nicht".

### Kommutativitaet

1. $\forall x \forall y F = \forall y \forall x F$

2. $\exists x \exists y F = \exists y \exists x F$

3. $\exists x \forall y F \models \forall y \exists x F$ (!)

Besondere Achtung hier fuer (3). Wenn es ein $x$ gibt, sodass die Formel fuer
alle $y$ gilt, dann gibt es auch fuer alle $y$ ein $x$, sodass die Formel
gilt. Aber (!): Wenn es fuer alle $y$ ein $x$ gibt, sodass die Formel gilt,
bedeutet das nicht, dass alle $x$ gleich sind. Es gibt also nicht zwingend ein
eindeutiges $x$, sodass die Formel mit diesem $x$ fuer alle $y$ gilt.

Quantoren gleichen Typs kann man also immer kommutieren.

### Distributivitaet

1. $\forall x (F \land G) = \forall x F \land \forall x G$

2. $\exists x (F \lor G) = \exists x F \lor \exists x G$

3. $\exists x (F \land G) \models \exists x F \land \exists x G$ (!)

4. $\forall x F \lor \forall x G \models \forall x (F \lor G)$ (!)

Bei (3) ist wieder nur ein Modell gegeben, und keine Aequivalenz. Wenn es ein
$x$ gibt, sodass fuer dieses konkrete $x$ sowohl $F$ als auch $G$ gilt, dann
gibt es auch fuer $F$ und $G$ separat ein gueltiges $x$ naemlich eben jenes, das
vorher fuer beide gegolten hat. Aber, wenn es ein $x$ gibt, sodass $F$ gilt, und
es ein $x$ gibt, sodass $G$ gilt, mussen diese $x$ nicht zwingend gleich sein,
also kann man keine Aequivalenz definieren.

(4) ist auch nur ein Modell. Wenn $F$ fuer alle $x$ gilt *oder* $G$ fuer alle
$x$ gilt, dann gilt fuer alle $x$ sicher $F \lor G$. Ist $\forall x G$
beispielsweise falsch, dann ist $\forall x(F \lor G)$ wahr, weil $x$ immer fuer
$F$ gilt. Wenn aber $\forall x (F \lor G)$ gilt, dann folgt nicht das
andere. Denn fuer verschiedene $x$ koennen verschiedene Praedikatsymbole wahr
sein. Also ist nicht zwingend ein Praedikatsymbol fuer alle $x$ wahr.

### Veranschauliche Aenderungen

Falls $x$ in $G$ nicht __frei__ vorkommt, sondern nur gebunden, oder gar nicht,
dann darf man die Klammern nach dem Quantor fallen lassen:

1. $\exists x (F \land G) \equiv \exists x F \land G$

2. $\exists x (F \lor G) \equiv \exists x F \lor G$

3. $\forall x (F \land G) \equiv \forall x F \land G$

4. $\forall x (F \lor G) \equiv \forall x F \lor G$

$x$ darf in $G$ nicht frei sein, weil nach den Regeln der Gueltigkeitsbereiche
$x$ in $G(x)$ sonst an den Quantor gebunden waere (was ja vorher nicht galt): $\exists x F(x) \land G(x)$.

__Man darf das nur mit den ganz rechten Quantoren!!__ (eventuell kommutieren)

## Kalkuel

Es gibt auch ein Kalkuel fuer die Praedikatenlogik. Alle Inferenzregel der
Aussagenlogik gelten auch hier, aber es gelten noch vier weitere Regeln fuer die
Einfuehrung und Beseitigung von Quantoren.

Aber zuerst: Sei $F$ eine Formel und $a$ eine Konstante. Dann bezeichnen wir mit
$F[x/a]$ die Formel, die man erhaelt, indem man alle __FREIEN__ Vorkommnisse von
$x$ in $F$ durch $a$ ersetzt. Also zum Beispiel:

* $F_1 = \forall y Q(x, y)$; $F_1[x/a] = \forall y Q(a, y)$
* $F_2 = P(x) \land \forall x Q(x)$; $F_2[x/a] = P(a) \land \forall x Q(x)$
* $F_3 = \forall x P(x)$; $F_3[x/a] = \forall x P(x)$

Dann die Inferenzegeln:

#### Allquantoreinfuehrung ($+ \forall$)

Fuer jede Menge von Annahmen $A$, Variable $x$, Formel $F$ und fuer jede
Konstante $a$, die __weder in $A$ noch in $F$ vorkommt__, gilt:

$\frac{A \vdash F[x/a]}{A \vdash \forall x F}$

Um zu zeigen, dass unter den Annahmen $A$ die Formel $\forall x F$ gilt, genuegt
es zu zeigen, dass die Formel fuer beliebige, aber feste $a$ gilt. Dadurch kann
man die Formel mit einem Allquantor in eine rein aussagenlogische Formel
umformen. Die Formel ist dann noch immer fuer alle $x$ gueltig, weil $a$
beliebig ist.

#### Allquantorbeseitigung ($- \forall$)

Nun genau das Gegenteil. Fuer jede Menge von Annahmen $A$, Variable $x$, Formel
$F$ und fuer jede Konstante $a$, gilt:

$\frac{A \vdash \forall x F}{A \vdash F[x/a]}$

Um zu zeigen, dass $F$ fuer beliebige aber feste $a$ gilt, zeige, dass $F$ fuer
alle $x$ gilt. Oder von oben nach unten: Wenn $\forall x F$ gilt, dann gilt auch
$F[x/a]$ fuer beliebiges $a$.

#### Existenzquantoreinfuehrung ($+ \exists$)

Fuer jede Menge von Annahmen $A$, Variable $x$, Formel $F$ und jede Konstante
$k$, gilt:

$\frac{A \vdash F[x/k]}{A \vdash \exists x F}$

Um zu zeigen, dass es ein $x$ gibt, fuer welches die Formel $F$ gilt, genuegt es
ein konstantes, konkretes (nicht beliebiges, wie $a$ oben!) $k$ zu finden,
sodass unter den Annahmen die Formel erfuellt ist.

#### Existenzquantorbeseitigung ($- \exists$)

Fuer jede Menge von Annahmen $A$, Variable $x$, Formel $F$ und fuer jede
Konstante $k$, die __weder in $A$ noch in $F$ vorkommt__, gilt:

$\frac{A \vdash \exists x F \, \, A, F[x/k] \vdash G}{A \vdash G}$

Wenn es fuer die Formel $F$ ein $x$ gibt, das die Formel erfuellt, und wenn ich
diese mit $x$ eingesetzte, also erfuellte (wahre), Formel in meine Annahmen
nehme, und daraus $G$ folgt, dann folgt aus meinen Annahmen $A$ immer $G$, weil
$F[x/k]$ immer wahr ist (weil wir annehmen es gibt ein $x$ fuer dass $[F](S) =
1$).
