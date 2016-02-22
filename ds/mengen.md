# Mengen

Eine Menge ist eine Struktur, die eine *ungeordnete* Sammlung von *null* oder
mehr *unterscheidbaren* (distinct) Elementen enthaelt. Die Mengentheorie
behandelt dabei Operationen auf Mengen, Beziehungen zwischen Mengen und Aussagen
ueber Mengen.

## Notation

Mengen werden allgemein in geschwungengen Klammern angegeben: $\{...\}$

Eine Menge kann *extensional* oder *intensional* beschrieben werden.

* Die *Extensionsionale Beschreibung* ist eine explizite Aufzaehlung der Elemente
  der Menge. Dies ist natuerlich nur fuer endliche Mengen moeglich. Beispiele:

  + $\{a, b, c\}$ ist die Menge, die aus drei Objekten $a, b, c$ besteht.
  + $\{1, 2, 3, 4, 5\}$ ist die Menge, die die fuenf Zahlen $1, 2, 3, 4, 5$
    enthaelt.

* In der *Intensionale Beschreibung* werden Mengen durch Eigenschaften
  beschrieben, die die Elemente der Menge erfuellen muessen, um enthalten zu
  sein. Diese Notation erlaubt unendliche Mengen. Beispiele:

  + $\{d \, | \, d \in D, M(d)\}$, wo $D$ die Menge aller in Deutschland
  lebenden ist und $M(d)$ die Eigenschaft, in Muenchen zu leben, beschreibt die
  Menge aller in Muenchen lebenden Menschen.
  + $\{d \, | \, d = 0\}$ ist die Menge aller Dinge die gleich null sind. Diese
    Menge enthaelt also nur ein Element, naemlich die null.

## Eigenschaften von Mengen

Nun einige Eigenschaften von Mengen:

* Mengen sind *ungeordnet*, die Reihenfolge von Elementen in der
  Menge ist also egal. Da jede Reihenfolge gleichgueltig ist, kann man sie auch
  selbst waehlen.
  + $\{a, b, c\} = \{b, c, a\} = \{c, a, b\}$

* Alle Elemente einer Menge sind *unterschiedlich*. Eine mehrfache Auflistung
  macht keinen Unterschied. Duplikate loesen sich also implizit auf bzw. haben
  keine Bedeutung.
  + $\{a, b, c\} = \{a, a, a, b, b, c\} = \{a, a, b, c\}$

* Eine Menge muss *nicht homogen* sein. Eine Menge kann also Zahlen, Buchstaben
  und sogar Mengen, enthalten z.B.: $\{5, A, \{a, b, c\}\}$. So gibt es
  natuerlich auch unendlich "tiefe" Mengen. Diese Eigenschaft sagt also: $1 \neq
  \{1\} \neq \{\{1\}\}$.

## Tupel

Neben den Mengen gibt es noch andere Formen der Auflistung von Elementen,
naemlich *Tupel*. Ein Tupel bezeichnet eine *geordnete* Gruppe von Elementen. Im
Gegensatz zu Mengen spielt die Reihenfolge der Elemente innerhalb der Auflistung
also eine Rolle. Auch muessen die Elemente eines Tupels nicht verschieden
sein. Tupel mit unterschiedlichen Anzahlen gleicher Elemente sind also voellig
verschieden.

Tupel werden mit runden Klammern $(...)$ angegeben. Ein Tupel von $n$ Elementen
nennt man $n$-Tupel. Hierbei gibt es noch verschiedene Namen fuer spezielle
Tupel:

* Ein $2$-Tupel wird *Paar* genannt.
* Ein $3$-Tupel wird *Tripel* genannt.
* Ein $4$-Tupel wird *Quadrupel* genannt.

Also: $(1, 2) \neq (2, 1) \neq (2, 1, 1)$

Koordinaten (x, y, z) sind beispielsweise besser als $n$-Tupel als Mengen
darstellbar, sonst waere (1, 5, 7) = (7, 5 1). Auch waere ein valides Tupel (1,
1, 1) als Menge $\{1\}$ (Quatsch).

## Venn-Diagramme

Venn-Diagramme sind eine Moeglichkeit zur Veranschaulichung von Mengen
bzw. deren Verhaeltnis. Mengen werden dabei als Kreise gezeichnet, in den
deren Elemente gemalt werden. So kann man beispielsweise veranschaulichen, dass
zwei Mengen dasselbe Element enthalten.

## Wichtige Mengen

Es gibt einige wichtige Mengen, die in der Mathematik oft verwendet werden:

* Die Menge der natuerlichen Zahlen: $\mathbb{N} = \{1, 2, ...\}$
* Die Menge der natuerlichen Zahlen, mit Null:$\mathbb{N}_0 = \{0, 1, 2, ...\}$
* Die Menge der ganzen Zahlen: $\mathbb{Z}$
* Die Menge der rationalen Zahlen: $\mathbb{Q}$
* Die Menge der irrationalen Zahlen: $\mathbb{I}$
* Die Menge der rellen Zahlen: $\mathbb{R}$
* Die Menge der komplexen Zahlen: $\mathbb{C}$
* Die Menge der ganzen Zahlen modulo $n$: $\mathbb{Z}_n$
* Die Menge aller Zahlen von $1$ bis $n$: $[n]$

### Die Leere Menge

Die leere Menge $\{\}$ oder $\emptyset$ ist eine sehr besondere Menge, da:

* Sie keine Elemente enthaelt.
* Teilmenge jeder Menge ist (man nimmt einfach keine Elemente aus der Menge)
* Somit auch Teilmenge der leeren Menge ist.

Vorsicht! Die leere Menge ist Teilmenge jeder Menge, aber sie ist nicht
*Element* jeder Menge: $\{\emptyset\} \neq \{\} = \emptyset$.

## Bezeichungen

Es gibt bestimmte Bezeichnungen und assoziierte Notation fuer Mengen:

* $x \in M$: Ein Element $x$ ist *Element von* einer Menge $M$

* $x \notin M$: Ein Element $x$ ist *kein Element von* einer Menge $M$

* $A = B$: Zwei Mengen $A, B$ sind gleich, wenn sie dieselben Elemente
  enthalten ($\forall a \in A \leftrightarrow a \in B$)

* $B \subseteq A$: Die Menge $B$ ist Teil- oder Untermenge von $A$. Das
  beruecksichtigt auch den Fall, dass $A = B$. Dann gilt auch $A \subseteq B$
  und $B \subseteq A$.

* $B \nsubseteq A$: $B \subseteq$ gilt nicht.

* $B \subset A$: $B$ ist *echte Teilmenge* von $A$, also gilt $B \subseteq A$
  und $B \neq A$.

* Zwei Mengen heissen *disjunkt* (disjoint) wenn sie keine Elemente gemeinsam
  haben.

## Operationen

Es gibt einige Operationen, die auf Mengen definiert sind:

### Kardinalitaet

Die Kardinalitaet $|A|$ einer Menge $A$ ist die Anzahl unterschiedlicher
Elemente einer Menge. Wenn gilt, dass $|A| \in \mathbb{N}_0$, ist $A$
*endlich*, ansonsten nicht.

Beispiele:

* $|\emptyset| = 0$
* $|\{1, 2, 3\}| = 3$
* $|\{a, b\}| = 2$
* $|\{\{1, 2, 3\}, \{4, 5\}\}| = 2$ (!)

Im letzten Fall muss man Acht geben, dass die Elemente einer Menge, die nur
Mengen enthaelt, wirklich die Anzahl der Mengen in der Menge ist, und nicht die
Anzahl an Elementen in den einzelnen Mengen der Menge.

### Potenzmenge

Die Potenzmenge $\mathcal{P}(M)$, auch $2^M$, einer Menge $M$ ist die *Menge
aller Teilmengen* von $M$:

$\mathcal{P}(N) = 2^M = \{X | X \subseteq M\}$

Da die leere Menge Teilmenge jeder Menge ist, ist sie in jeder Potenzmenge
enthalten. Da wir von Teilmengen, und nicht *echten Teilmengen* sprechen, ist
auch jede Menge selbst in ihrer Potenzmenge enthalten.

Beispiel: $\mathcal{P}(\{a, b\}) = \{\emptyset, \{a\}, \{b\}, \{a, b\}\}$

Die Kardinalitaet $|\mathcal{P}(M)|$ einer jeden Potenzmenge $\mathcal{P}(M)$
einer Menge $M$ ist immer $2^{|M|}$. Das folgt daraus, dass man die Elemente der
Menge als Bits eines $|M|$-Bit Wortes betrachten kann. Dann gibt es also alle
Moeglichkeiten von $000...000$ (keine Elemente enthalten; die leere Menge) bis
$111...111$ (alle Elemente enthalten; die Menge $M$ selbst) um Teilmengen zu
bilden. Ergo:

$|\mathcal{P}(M)| = |2^M| = 2^{|M|}$

Die Kardinalitaet der Potenzmenge der leeren Menge $\emptyset$ ist auch passend
$2^0 = 1$, da sie nur die leere Menge enthaelt.

### Das kartesische Produkt

Das *kartesische Produkt* $\times$, kurz *Kreuzprodukt*, von zwei Mengen $A, B$
ist die Menge aller Paare $(a, b)$ wo $a \in A$ und $b \in B$, also:

$A \times B := \{(a, b) \, | \, a \in A \text{ und } b \in B\}$

Da es fuer jedes der $|A|$ Elemente in $A$ genau $|B|$ Partner in $B$ gibt, gibt
es insgesamt $|A| \cdot |B|$ Elemente in $A \times B$. Fuer endliche Mengen gilt
also immer:

$|A \times B| = |A| \cdot |B|$

Das kartesische Produkt ist nicht *kommutativ*! Das folgt daraus, dass bei
Tupeln die Reihenfolge eine Rolle spielt, und die Reihenfolge, in welcher wir
Kreuzprodukte bilden eben diese Reihenfolge bestimmt. Beispielsweise:

$\{a\} \times \{b\} = \{(a, b)\} \neq \{(b, a)\} = \{b\} \times \{a\}$

Also:

$A \times B \neq B \times A$

#### Potenz

So wie $x^n$ fuer das wiederholte Anwenden der Multiplikation zwischen $x$ und
sich selbst steht ($x \cdot x \cdot ... \cdot x$), steht nun bei Mengen
$M^n$ auch fuer das wiederholte Anwenden des kartesischen Produkts zwischen der
Menge $M$ und sich selbst:

$M^n = M \times M \times ... \times M$

Beispielsweise ist $\{a, b\}^2 = \{(a, a), (a, b), (b, a), (b, b)\}$.

Natuerlich muss es auch wie bei den Zahlen und der Multiplikation eine
Definition fuer $M^0$ geben. Wir definieren das Ergebnis von $M^0$ als die Menge
*mit dem leeren Wort* $\varepsilon$: $M^0 = \{\varepsilon\}$.

Somit koennen wir die Operation $M^n$ auch rekursiv definieren:

$$M^n = \begin{cases}\{\varepsilon\} \text{ , wenn }n = 0 \\ M \times M^{n-1}
\text{, sonst}\end{cases}$$

Weil es bei $M \times \emptyset$ kein Element in $\emptyset$ gibt, womit
irgendein Element $\in M$ ein Tupel bilden koennte, gilt:

$M \times \emptyset = \emptyset$

##### Alphabete

Die Menge $\bigcup_{n=0}^\infty = A^n$ wird mit $A^\star$ bezeichnet.

Ist z.B.  $\Sigma = \{a, b, c, ..., x, y, z\}$ das englische Alphabet, so ist
$\Sigma^\star$ die Menge aller moeglichen Woerter der englischen Sprache.

* Man nennt $A^\star$ auch allgemein eine *Sprache*.

* Ein Element aus dieser Sprache $A^\star$ nennt man dann ein *Wort*.

* Die *Konkatenation* von zwei Woertern $a_1...a_n$ und $b_1...b_m$ ist das Wort
  $a_1...a_nb_1...b_m$.

### Vereinigung

Die Vereinigung $A \cup B$ zweier Mengen $A, B$ ist die Menge aller Elemente,
die entweder in $A$, in $B$ oder in beiden enthalten sind:

$A \cup B := \{x \, | \, x \in A \lor x \in B\}$

Der Vereinigungsoperator $\cup$ aehnelt somit in seiner Semantik dem logischen
Disjunktionsoperator $\land$.

Beispiel: $\{a, b\} \cup \{1, 2\} = \{a, 2, b, 1\}$.

Man beachte hier wieder die Eigenschaft, dass Mengen keine Duplikate enthalten,
sodass von $A \cup A = A$.

Die Vereinigung ist kommutativ: $A \cup B = B \cup A$

### Schnittmenge

Die Schnittmenge $A \cap B$ zweier Mengen $A, B$ ist die Menge aller Elemente,
die sowohl in $A$, als auch in $B$ enthalten sind:

$A \cap B := \{x \, | \, x \in A \land x \in B\}$

Der Vereinigungsoperator $\cap$ aehnelt somit in seiner Semantik dem logischen
Konjunktionsoperator $\land$.

Beispiel: $\{a, b\} \cap \{b, c\} = \{b\}$, $\{1\} \cap \{2\} = \emptyset$

Der Schnittmengenoperator ist auch kommutativ: $A \cap B = B \cap A$

Somit folgt auch, dass $|A \cup B| = |A| + |B| - |A \cap B|$. Bei $|A| + |B|$
werden gemeinsame Elemente naemlich einmal fuer $A$ und einmal fuer $B$, also
insgesamt einmal zu viel gezaehlt. Deswegen zieht man die Schnittmenge wieder
ab. Dieses Prinzip nennt man das *Prinzip der Inklusion und Exklusion*.

### Differenzmenge

Die Differenzmenge $A \setminus B$ ist die Menge aller Elemente der Menge $A$,
die nicht auch in $B$ enthalten sind:

$A \setminus B := \{x | x \in A \land x \notin B\}$

Dabei ist $A \setminus B = A \cap \overline{B}$.

Die Differenz ist keine kommutative Operation! $A \setminus B \neq B \setminus
A$, z.B.: fuer $\{a, b\} \setminus \{a, b, c\}$:

* $\{a, b, c\} \setminus \{a, b\} = \{c\}$, aber
* $\{a, b\} \setminus \{a, b, c\} = \emptyset$.

Generell $A \setminus B = \emptyset$ wenn $A \subseteq B$.

Um zu beweisen dass $A \neq B$ ist es oft gut, zu zeigen, dass $|A \setminus B|
\geq 0$.

### Komplementmenge

Sei $U$ eine Grundmenge (auch: *Universum*), wobei gilt, dass alle fuer eine
Situation betrachteten Mengen $M$ Teilmengen dieses Universums sind. Dann ist
die *Komplementmenge* $\overline{M}$ ("M quer") die Menge aller Elemente, die in
$U$ enthalten sind, aber nicht in $M$:

Gegeben ein Universum $U$ mit einer Menge $M \subseteq U$:
$\overline{M} := U \setminus M$

### Symmetrische Differenz

Die *Symmetrische Differenz* $A \Delta B$ zwischen zwei Mengen $A, B$ ist die
Menge aller Elemente, die nur in einer der Mengen enthalten ist. Sozusagen ein
"Mengen-XOR".

$A \Delta B := (A \setminus B) \cup (B \setminus A)$

### Disjunkte Vereinigung

Eine *Disjunkte Vereinigung* $A \uplus B$ zwischen zwei Mengen $A \cap B$ ist eine
herkoemmliche Vereinigung, wobei angenommen wird, dass $A$ und $B$ disjunkt.

### Partition

Eine *Partition* einer Menge $A$ ist eine Zerlegung von $A$ in *paarweise
disjunkte* (jeweils zwei Teilmengen sind disjunkt), *nichtleere* Teilmengen
$A_1, ..., A_n$, sodass gilt:

$A = \biguplus_{i=1}^n A_i$

Die einzelnen Teilmengen werden hierbei auch *Klassen* genannt.

Beispiel: $\{\{1, 2\}, \{3, 4, 5\}, \{6\}\}$ ist eine von vielen moeglichen
Partitionen der Menge $[6]$.

### Gesetze

Fuer beliebige Mengen $A, B, C$ gelten die folgenden Identitaeten:

* Idempotenz: $A \cup A = A \cap A = A$
* Kommutativitaet: $A \cup B = B \cup A$, auch fuer $\cap$.
* Assoziativitaet: $(A \cup B) \cup C = A \cup (B \cup C)$, auch fuer $\cap$.
* Distributivitaet: $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$, auch fuer
  umgekehrte Operatoren.
* DeMorgan: Wie oben angemerkt besteht eine gewisse Aehnlichkeit zwischen $\cap
  $ und $\land$ sowie $\cup$ und $\lor$. Daher kann man auch die DeMorgan'schen
  Gesetze anwenden:
	  1. $\overline{A \cap B} = \overline{A} \cup \overline{B}$
	  2. $\overline{A \cup B} = \overline{A} \cap \overline{B}$

Die Mengendifferenz $\setminus$ ist weder kommutativ, assoziativ, noch
distributiv.

Beispiele:

* $A \setminus (B \cup C) = (A \setminus B) \cup (A \setminus C)$ gilt nicht.
* $A \setminus (B \cup C) = (A \setminus B) \cap (A \setminus C)$ gilt.
* $A \setminus (B \setminus C) = (A \setminus B) \setminus C$ gilt nicht.
* $A \setminus (B \setminus C) = (A \setminus B) \cup (A \cap C)$ gilt.
* $A \setminus (B \setminus C) = (A \setminus B) \cup (A \cup C)$ gilt nicht.

## Gotchas

* Beim Vergleichen von Formeln: Am Ende immer mit echten Mengen probieren, um
  zu zeigen dass die Formeln wirklich nicht aequivalent sind oder um zu pruefen,
  dass wirklich links und rechts dasselbe rauskommt.
