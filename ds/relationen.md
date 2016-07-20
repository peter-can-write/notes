# Relationen

Sind $A_1,A_2,...,A_n$ Mengen, so ist eine *Relation* $R$ eine Teilmenge des
Kreuzprodukts dieser Mengen:

$R \subseteq \times_{i=1}^n A_i$

Ist $n = 2$, so sprechen wir von einer binaeren Relation und schreiben oft $aRb$
statt $(a, b) \in R$.

Ein Beispiel fuer eine Relation ist "kleiner" $<$ ueber den natuerlichen
Zahlen. Diese binaere Relation enthaelt alle Tupel von natuerlichen Zahlen, wo
die linke Zahl kleiner ist als die rechte: $R = \{(1, 2), (4, 5), (2, 99),
...\}$

## Grafische Darstellung

Eine Relation laesst sich schoen in einem Graphen visualisieren. Ist $R
\subseteq A \times B$ eine Relation ueber der Menge $A \times B$, so kann man
diese Relation aufzeichnen, indem man fuer jedes $a \in A$ und $b \in B$ einen
Knoten malt, und zwischen zwei Knoten genau dann eine Kante, wenn $(a, b) \in R$
ist.

```
  aRb
a-----b
|
| aRB
|
c
```

Natuerlich kann es hier auch Selbstschleifen geben.

## Eigenschaften

Man kann Relationen durch bestimmte Eigenschaften beschreiben. Eine Relation $R
\subseteq A \times A$ ist:

* *Reflexiv*: Wenn $\forall a \in A$ gilt: $(a, a) \in R$. Wenn also jedes
  Element von $A$ mit sich selbst in Relation steht (z.B. bei $=$). In Graphen
  zeichnet man hierfuer Selbstschleifen. Die Reflexivitaet wird schon verletzt,
  wenn nur ein Tupel $(a, a)$ fehlt, obwohl alle anderen da sind.

* *Symmetrisch*: Wenn $\forall a, b \in A$ gilt: $(a, b) \in R \rightarrow (b,
  a) \in R$. Wenn also $(a, b)$ in der Relation ist, dann muss es auch das
  symmetrische Gegen-Tupel $(b, a)$ (z.B. bei $=$).

* *Asymmetrisch*: Wenn $\forall a, b \in A$ gilt: $(a, b) \in R \rightarrow (b,
  a) \notin R$. Wenn also $(a, b)$ in der Relation ist, dann ist das umgekehrt
  nicht der Fall (z.B. bei $<$). __Asymmetrie schliesst Reflexivitaet aus.__

* *Antisymmetrisch*: Wenn $\forall a, b \in A$ gilt: $(a, b) \in R \land (b, a)
  \in R \rightarrow a = b$. Sind also beide Varianten $(a, b)$ und $(b, a)$ in
  der Relation, dann bedeutet das, dass $a = b$ (z.B. bei $\leq$). Aus
  Asymmetrie folgt immer auch Antisymmetrie, weil sowieso nur immer eine der
  beiden Varianten in der Relation ist. Bei Antisymmetrie ist Symmetrie also
  noch immer nicht erlaubt, Reflexivitaet aber nun schon. Die Antisymmetrie
  erweitert die Asymmetrie also sozusagen auf die Reflexivitaet.

* *Transitiv*: Wenn $\forall a, b, c \in A$ gilt: $(a, b) \in R \land (b, c) \in
  R \rightarrow (a, c) \in R$. Wenn es also ein Brueckenelement zwischen zwei
  Elementen gibt, dann muessen diese zwei Elemente auch in der Relation sein
  (z.B. bei $<$, aber nicht bei $\neq$). Eine Relation ist also transitiv, wenn
  zwei Dinge *nicht gelten*:
   + $\exists a, b: (a, b) \in R \land (b, a) \in R \land \neg((a, a) \in R \lor
     (b, b) \in R)$. Wenn es einen Kreis gibt, muss es auch zumindest eine
     Schleife geben.
   + $\exists a, b, c: (a, b) \in R \land (b, c) \in R \land (a, c) \notin R$
      Wenn zwei Elemente nur indirekt in Relation stehen.

* *Total*: Wenn $\forall a, b \in A$ gilt: Entweder ist $(a, b) \in R$ *oder*
  $(b, a) \in R$, *oder beides*. Zwei beliebige Elemente muessen also zumindest
  in eine Richtung in der Relation sein. Daher muss eine totale Relation vor
  Allem auch *immer reflexiv* sein.

Weitere Eigenschaften:

* *Linkstotal*: Wenn jedes Element $a \in A$ zumindest einmal in der Relation
  vorkommt, sodass $\exists b \in B: aRb$. Dann gilt: $|\{b \in B \, | \, (a, b)
  \in R\}| \geq 1$. D.h. es gibt zumindest ein rechtes Element fuer $a$.

* *Rechtseindeutig*: Wenn jedes Element $a \in A$ mit hoechstens einem Element $b
  \in B$ in Relation steht: $|\{b \in B \, | \, (a, b) \in R\}| \leq 1$

Diese Eigenschaften sind wie Injektivitaet bzw. Surjektivaet fuer Funktionen,
nur eben auf das Urbild bezogen. Eine Funktion ist immer linkstotal *und*
rechtseindeutig.

Wir nennen eine Relation *homogen*, wenn Quell- und Zielmenge gleich sind ($R
\subseteq A \times A$).

### Leere Relation

Die leere Relation $\emptyset$ ist:

* symmetrisch
* asymmetrisch
* antisymmetrisch
* transitiv

wenn die Grundmenge $M \neq \emptyset$, und sonst auch noch zusaetzlich:

* reflexiv
* total

Die Leere Relation ueber der leeren Grundmenge besitzt also alle sechs
Eigenschaften.

## Aequivalenzrelationen

Eine Relation $R \subseteq M \times M$ nennt man Aequivalenzrelation, wenn sie:

* reflexiv
* transitiv
* symmetrisch

ist. Eine Aequivalenzrelation beschreibt also Elemente mit aehnlichen
Eigenschaften. Ist z.B. $m \in M$ und man moechte eine Aequivalenzrelation
bilden. Dafuer nimmt man nun alle Elemente aus $M$, die aehnlich zu $m$ sind,
und packt sie in eine *Aequivalenzklasse*. Und das macht man nun fuer alle
Elemente.

Die Teilmenge $[a] = \{b \in M \, | \, (a, b) \in R\}$ der Menge $M$ ist demnach
eine Aequivalenzklasse von $a$. $[a]$ enthaelt also alle Elemente, die zu $a$
aequivalent sind.

Die Aequivalenzklassen der Aequivalenzrelation __bilden hierbei eine Partition__
von $M$. Die Klassen sind *nichtleer*, da sie zumindest das Element $a$
enthalten. Sie sind paarweise disjunkt, weil ueber Transitivitaet jedes Element
das zu $a$ aequivalent ist, erreicht werden kann.

Eine Aequivalenzrelation ist also eine Partition des Kreuzprodukts $A \times B$
in seine Aequivalenzklassen.

In der graphischen Darstellung kommt man von jedem Element zu jedem
aequivalenten anderen Element in nur einem Schritt (mit nur einer Kante). Wir
schrumpfen also alle Zusammenhangskomponenten auf einen Graphen mit einer Senke.

## Ordnungen

### Partielle Ordnung

Eine partielle Ordnung $R \subseteq A \times B$ ist nun eine Relation, die
folgende Eigenschaften besitzt:

* reflexiv
* transitiv
* *anti*symmetrisch

Eine partielle Ordnung nennt man auch *Halbordnung*.

Beispiele:
* $=$ ist eine partielle Ordnung ueber jeder Menge. Sie ist partiell, weil fuer
  zwei ungleiche Elemente $a, b$ weder $a = b$ noch $b = a$ gilt.

* $\subseteq$ ist eine partielle Ordnung der Potenzmenge einer Menge. Sie ist
  partiell, weil wenn z.B. fuer die Menge $\{a, b\}$ die beiden Teilmengen
  $\{a\}$ und $\{b\}$ in $2^{\{a, b\}}$ sind, aber weder $\{a\} \subseteq \{b\}$
  noch $\{b\} \subseteq \{a\}$.

* $<$ ist keine Ordnung, weil es nicht mal reflexiv ist.

### Totale Ordnung

Eine *totale Ordnung* ist nun eine partielle Ordnung, bei welcher fuer zwei
beliebige Elemente der Grundmenge gilt, $aRb$ oder $bRa$. Zwei Elemente der
Grundmenge sind also immer *vergleichbar*. Eine partielle Ordnung muss also noch
nicht vollstaendig sein, und ein totale Ordnung ist nun eine, wo zwei Elemente
immer in Relation stehen (in nur eine Richtung, wenn ungleich, da
antisymmetrisch).

Eine totale Ordnung hat eine und nur eine Reihenfolge fuer die Grundmenge, daher
gibt es fuer eine Menge mit $n$ Elementen $n!$ moegliche totale Ordnungen.

In einer totalen Ordnung kann man neue Elemente also immer einordnen, in einer
partiellen Ordnung nicht unbedingt.

Ein Beispiel fuer eine partielle Ordnung ist die Teilbarkeitsrelation $|$ auf
der Menge $\{3, 4, 6\}$. Diese Relation ist nur eine partielle Ordnung, weil
fuer die Elemente $3, 4$ nicht gilt, dass entweder $3 | 4$ sodass $(3, 4) \in |$
oder $4 | 3$ sodass $(4, 3) \in |$.

Hingegen ist die Relation $\leq$ auf der Menge der natuerlichen Zahlen
$\mathbb{N}$ schon eine totale Ordnung, weil fuer alle zwei moeglichen $a, b \in
\mathbb{N}$ gilt, dass entweder $a \leq b$ oder $b \leq a$. Und wenn $a \leq b$
*und* $b \leq a$, dann muss $a = b$ gewesen sein.

### Groessenordnungen

#### Minimal und Maximal

Ein Element $x$ einer Menge $M$ ist bezuglich einer Relation $R \subseteq M
\times M$ *minimal*, wenn es *kein weiteres* Element $y \in M$ gibt, __das von
$x$ verschieden ist__, und das bezueglich $R$ kleiner ist als $x$. $x$ kommt
also niemals auf der rechten Seite eines Tupels in $R$ vor, ausser das Tupel ist
$(x, x)$.

Kommt das obige Element $x$ nie auf der linken Seite eines Tupels vor, sodass es
also kein __von $x$ verschiedenes__ $y \in M$ gibt, wo $(x, y) \in R$, dann
nennt man es *maximal*.

__Es kann fuer eine Relation mehrere maximale und mehrere minimale Elemente
geben.__

Die Verschiedenheit von $y$ in Bezug zu $x$ ist wichtig, da es sonst keine
reflexiven Tupel geben duerfte.

In Praedikatenlogik fuer eine Relation $R$ ist die Minimalitaet von $x$
definiert als: $\forall y (R(y, x) \rightarrow x = y)$

#### Kleinstes und Groesstes

Ein Element $x$ einer Menge $M$ ist bezuglich einer Relation $R \subseteq M
\times M$ nennt man *kleinstes*, wenn es bezueglich $R$ kleiner __als alle__
anderen Elemente $y \in M, y \neq x$ ist. Fuer *jedes* *andere* Element gibt es
also ein Tupel $(x, y) \in R$.

Ist das Element $x$ bezueglich $R$ groesser als __jedes__ andere Element, so
nennt man es *groesstes*. Fuer jedes andere Element $y \in M$ gibt es also ein
Tupel $(y, x) \in R$.

Es gibt fuer jede Relation hoechstens ein kleinstes und hoechstens ein groesstes
Element.

#### Unterschied

Die Begriffe *maximales* und *groesstes* Element bzw. *minimales* und
*kleinstes* Element sind fuer *totale* Ordnungen genau dasselbe. Die Begriffe
*konvergieren* fuer totale Ordnungen also, da jedes Element mit jedem anderen
irgendwie in Relation stehen __muss__.

Bei partiellen Ordnungen koennen die beiden Begriffsklassen jedoch
unterschiedliche Elemente bestimmen, da manche Paare von Elementen gar nicht in
Relation stehen, was ein kleinstes oder groesstes Element ausschliesst.

Man nehme z.B. die Teilbarkeitsrelation auf der Grundmenge $\{1, 2, 3, 4, 6, 9,
12\}$.

* Hier ist $1$ das kleinste Element, weil es fuer jede andere Zahl links in
  einem Tupel steht ($1$ teilt jede Zahl). Es ist folglich auch minimal.

* $12$ ist das einzige maximale Element, weil es keine andere Zahl teilt, also
  niemals auf der linken Seite eines Tupels ist.

* Es gibt kein groesstes Element, weil keine der Zahlen von jeder anderen
  geteilt werden kann. $36$ (das kgV) waere z.B. ein groesstes Element, wenn man
  es einfuegen wuerde. Dann waere $12$ auch nicht mehr maximal.

### Hasse-Diagramme

Fuer partielle Ordnungen gibt es bestimmte Diagramme, um diese zu visualisieren:
*Hasse-Diagramme*. Hierbei zeichnet man einen ungerichteten Graphen fuer
die Relation, ohne:

* Reflexive Tupel. Also keine Selbstschleifen.
* Transitive Tupel. Also keine unnoetigen Verbindungen, die durch transitive
  Pfade schon dargestellt sind.

Sei $\subseteq$ beispielsweise die Relation "Teilmenge von" ueber
$\mathbb{P}(M)^2$. Diese Relation enthaelt also Tupel mit der leeren Menge und
jeder moeglichen Teilmenge von $M$, sowie generell Tupel zwischen allen
Teilmengen von $M$, wo die eine noch Teilmenge der anderen ist. Sei $M = \{a,
b\}$, dann:

```
  {a,b}
  /   \
{a}   {b}
  \  /
   {}
```

Wie man sieht, sind die transitiven Tupel nicht gezeichnet. $(\emptyset, \{a,
b\})$ ist zwar in der Relation, aber es gibt schon andere Wege dorthin.

Um zu entscheiden ob ein Element im Hassediagram nicht drin ist, weil es
transitiv ist, siehe also, ob fuer das relevante Tupel $(a, b)$ auch die beiden
Tupel $(a, x)$ und $(x, b)$ gibt, dann waere $(a, b)$ nicht drin.

## Bild und Urbild

Das *Bild* einer Relation $R \subseteq A \times A$ ist die Menge aller Elemente
$b$ von $A$, mit welchen ein $a \in A$ in Relation steht. Es ist sozusagen also
die Menge aller "rechten" Elemente:

$\text{Bild } := \{b \in A \, | \, \exists a: (a, b) \in R\}$

Das *Urbild* der selben Relation ist dann die Menge aller $a$, mit welchen ein
$b \in A$ in Relation steht. Es ist sozusagen die Menge aller "linken" Elemente:

$\text{Urbild } := \{a \in A \, | \, \exists b: (a, b) \in R\}$

## Umkehrrelation

Die Inverse bzw. die Umkehrrelation $R^{-1}$ zu einer Relation $R \subseteq A
\times B$ enthaelt alle Elemente, die auch $R$ enthaelt, aber mit den Elementen
der Tupel in ihrer Reihenfolge vertauscht:

$R^{-1} = \{(b, a) \in B \times A \, | \, (a, b) \in R\}$

## Komplementrelation

Die Komplementrelation $\overline{R}$ zu einer Relation $R \subseteq A \times B$
ist die Menge aller Tupel aus $A \times B$, die nicht in $R$ enthalten sind:

$\overline{R} = \{(a, b) \, | \, a \in A, b \in b, (a, b) \notin R\}$

## Relationenprodukt

Sind $R \subseteq A \times B$ und $T \subseteq B \times C$ binaere Relationen,
dann ist das Relationenprodukt $R \circ T$ die Menge aller Tupel $(a, c)$, wo
es ein Brueckenelement $b$ in den Relationen gibt:

$R \circ T := \{(a, c) \in A \times C \, | \, \exists b:  (a, b) \in R \land (b,
c) \in T\}$

Das Produkt $R \circ T$ aus zwei Relationen schreibt man oft auch kurz $RT$ (wie
bei der Multiplikation von Zahlen).

$\circ$ ist distributiv ueber $\cup$, aber nicht ueber $\cap$.

Das Relationenprodukt nennt man auch *Komposition* von Relationen.

### Potenzen

Wie bei Potenzen kann man das Relationenprodukt auch mehrmals durchfuehren und
schreibt es dann als Hochzahl ueber die Relation:

$R^{n + 1} = R^n \circ R$ fuer $n \in \mathbb{N}_0$

Hierbei ist $R^0$ definiert als die Identitat $Id$, welche fuer eine Relation
$R \subseteq A \times A$ alle reflexiven Tupel $(a, a), a \in A$ enthaelt:

$R^0 = \{(a, a) \, | \, a \in A\}$

Der Sinn ist, dass man somit auch $R^1$ als $R^0 \cdot R$ definieren kann
(wodurch mittels Rekursion dann auch alle anderen Potenzen definiert sind), da
beim Relationenprodukt mit allen reflexiven Tupeln genau die selben Tupel
rauskommen. Ist z.B. $(a, b) \in R$, dann ist $\{(a, b)\} \circ \{(b, b)\} =
\{(a, b)\}$.

Intuitiv enthaelt $R^n$ alle Abkuerzungen fuer Wege der Laenge genau $n$ in der
graphischen Darstellung von $R$. Hat man z.B. einen transitiven Weg $a
\rightarrow b \rightarrow c$ der Laenge zwei, also die Tupel $(a, b), (b, c)$ in
der Relation, dann wird der Weg bei jeder Potenz $> 1$ um eins verkuerzt. Also
in $R^2$ waere der Weg schon $a \rightarrow c$ bzw. das Tupel $(a, c)$.

Beispiel: Ist $Kind$ die Relation $\{(k, v) \, | \, k \text{ ist Kind von }
v\}$, dann ist $Kind^2$ die Enkel-Relation mit den Tupeln $(k, v')$, wo es
Tupel $(k, v)$ und $(v, v')$ gab.

## Huellen

Die reflexive, symmetrische oder transitive Huelle einer Relation $R$ ist jene
minimalste Relation $R'$, die Uebermenge von $R$ ist und reflexiv, symmetrisch
oder transitiv (einzeln) ist.

Die transitive Huelle einer Relation $R$ bezeichnet man ueblicherweise mit
$R^+$, die reflexiv-transitive mit $R^\star$.

Beispiel: Ist $R = \{(x, y) \, | \, y = x + 1\}$ die Nachfolgerrelation auf den
natuerlichen Zahlen, dann ist $R^+ = <$ und $R^\star = \leq$.

### Reflexive Huelle

Um die reflexive Huelle $R^{refl}$ einer Relation zu bauen, gibt man zur
Relation einfach alle reflexiven Tupel hinzu. Somit ist $R^{refl} = R \cup
Id_M$, also die Relation vereinigt mit der Identitaetsrelation.

### Symmetrische Huelle

Die symmetrische Huelle $R^{sym}$ einer Relation $R$ wird gebildet, indem man
$R$ einfach mit seiner Umkehrrelation $R^{-1}$ vereint:

$R^{sym} = R \cup R^{-1}$

Somit hat man immer beide Richtungen $(a, b)$ sowie auch $(b, a)$ fuer beliebige
$a, b$ der Grundmenge.

### Transitive Huelle

Die transitive Huelle $R^+$ einer Relation $R \subseteq A \times A$ wird
gebildet, indem man $R$ so oft mit sich selbst komposiert, bis alle transitiven
Tupel enthalten sind. Das muss man so oft machen, wie es Elemente in der
Grundmenge gibt, minus eins:

$R^+ = \bigcup_{i=1}^{n-1} R^i$

wo $n = |A|$.

### Reflexiv-Transitive Huelle

Die reflexiv-transitive Huelle $R^\star$ einer Relation $R \subseteq A \times A$
ist gleich der transitiven Huelle $R^+$, vereinigt mit der reflexiven Huelle,
oder einfach der Identitaet $Id_A$. Da $R^0 := Id_A$ gilt:

$R^\star = Id_A \cup R^+ = \bigcup_{i=0}^{n - 1} R^i$

## Funktionen

Wir nennen eine Relation $R \subseteq A \times B$ eine *Funktion von $A$ nach
$B$* bzw. eine *Abbildung von $A$ auf $B$*, wenn es fuer alle $a \in A$ genau
ein einziges eindeutiges Element $b \in B$ gibt, sodass $aRb$ bzw. $(a, b) \in
R$. Eine Funktion ist immer linkstotal und rechtseindeutig, es bildet ein $a \in
A$ also auf mindestens ein und maximal ein Element ab -- also genau eines.

Ist $f$ eine Funktion (also Relation) von $A$ nach $B$, dann schreiben wir:
$f: A \rightarrow B$

Fuer jedes $a \in A$ bezeichnet dann $f(a)$ das einzige Element $\in B$, sodass
$(a, f(a)) \in f$.

Eine $n$-stellige Operation ueber der Menge $A$ ist eine Funktion von $A^n$ nach
$A$.

### Funktionsoperatoren

Ein beliebiger Operator $\bullet$ ueber einer Menge $B$ kann auch auf Funktionen
$f: A \rightarrow B$ erweitert werden. Sind $f, g: A \rightarrow B$ zwei
Funktionen, dann ist:

$(f \bullet g): A \rightarrow B: a \mapsto f(a) \bullet g(a)$.

Ist der Operator beispielsweise die Addition $+$ oder die Multiplikation
$\cdot$ ueber der Menge der rellen Zahlen $\mathbb{R}$, dann gilt fuer zwei
Funktionen $f, g: \mathbb{R} \rightarrow \mathbb{R}$:

$(f + g): \mathbb{R} + \mathbb{R}: x \mapsto f(x) + g(x)$
$(f \cdot g): \mathbb{R} \cdot \mathbb{R}: x \mapsto f(x) \cdot g(x)$

### Funktionskomposition

Die Operation $\circ$ bildet aus zwei Funktionen $g: A \rightarrow B, f: B
\rightarrow C$ eine neue Funktion, wobei $g$ auf das Resultat von $f$ angewandt
wird. Man sagt auch, das Resultat von $g$ wird *auf* $f$ evaluiert.

Dann ist $(f \circ g)$ eine Funktion von $A \rightarrow C$, definiert durch:

$(f \circ g): A \rightarrow C: x \mapsto f(g(x))$.

Das Ergebnis von $g$ ist fuer ein $x \in A$ ein Element $b \in B$. Da die
Funktion $f$ von $B$ nach $C$ abbildet, wird das $b$ also weiter auf ein Element
in $C$ abgebildet. Es wird sozusagen eine Kante von $A$ nach $C$ gezeichnet,
weil es Kanten von $A$ nach $B$ und von $B$ nach $C$ gibt. $B$ ist also aehnlich
wie bei der Diskussion von Transitivitaet eine "Brueckenmenge".

Der Operator $\circ$ ist hierbei nicht kommutativ. $f \circ g \neq g \circ f$.

Beispiel:
$f: \{1, 2\} \rightarrow \{10, 20\}: a \mapsto 10 \cdot a$
$g: \{10, 20\} \rightarrow \{9, 19\}: b \mapsto b - 1$

Dann ist $(g \circ f)(1) = 9$, weil $f(1) = 10$ und $g(f(1)) = g(10) = 9$.

### Bild und Urbild

#### Bild

Bei einer Relation $R \subseteq A \times B$ war das Bild allgemein definiert als
die Menge aller $b \in B: \exists a \in A: (a, b) \in R$. Da eine Funktion eine
Relation ist, wo es fuer ein $a \in A$ genau ein $b \in B$ gibt, sodass $aRb$,
gilt auch, dass das Bild $\{b\}$ von $a$ nur ein Element enthaelt. Daher nennt
man schon $f(a) = b$ das Bild von $a$ unter $f$.

Das kann nun auch noch fuer eine ganze Menge von Elementen erweitert werden. Ist
$X \subseteq A$, dann ist das Bild von $X$ unter $f$ die Menge aller Bilder
der Elemente in $X$:

$f(X) = \{f(x) \, | \, x \in X\}$

#### Urbild

Das Urbild $f^{-1}(b)$ eines Elements $b \in B$ unter einer Funktion $f: A
\rightarrow B$ ist definiert als die Menge aller Elemente $a \in A$, die auf $b$
abgebildet werden. Somit ist es auch die Menge aller Elemente, die $b$ in ihrem
*Bild* haben.

$f^{-1}(b) = \{a \in A \, | \, f(a) = b\}$

Gleichfalls gilt fuer eine Menge $X \subseteq B$:

$f(X) = \{a \in A \,|\, f(a) \in X\} = \bigcup_{b \in X} f^{-1}(b)$

### Eigenschaften

Funktionen koennen gewisse Eigenschaften besitzen, die ihre Abbildung
betrifft. Eine Funktion $f: A \rightarrow B$ ist:

* *injektiv*, wenn auf ein $b \in B$ maximal ein $a \in A$ abgebildet wird, wenn
  also alle Elemente $a \in A$ *unterschiedliche Bilder* haben. Dann gilt auch:
  $\forall b \in B: |f^{-1}(b)| \leq 1$

* *surjektiv*, wenn auf jedes Element $b \in B$ *zumindest ein* $a \in A$
  abgebildet wird, also das Bild von mindestens einem Element von $A$ ist. Dann
  gilt auch:
  $\forall b \in B: |f^{-1}(b)| \geq 1$

* *bijektiv*, wenn auf jedes Element $b \in B$ *genau ein einziges* $a \in A$
  abgebildet wird. Eine Funktion, die bijektiv ist, ist also *sowohl bijektiv,
  als auch injektiv*. Das Urbild eines Elements von $B$ enthaelt daher
  mindestens ein und hoechstens ein Element, daher genau eines. Somit:
  $\forall b \in B: |f^{-1}(b)| \leq 1 \land |f^{-1}(b)| \geq 1 \leftrightarrow
  |f^{-1}(b)| = 1$.

Eine bijektive Funktion nennt man auch *invertierbar*, da es fuer jede bijektive
Funktion $f: A \rightarrow B$ eine eindeutige inverse Funktion $f^{-1}: B
\rightarrow A$ gibt, wobei $f^{-1} \circ f = Id_A$ (Identitaetsfunktion).

Wenn eine Funktion injektiv ist, gilt $f(x) = f(y) \rightarrow x = y$.

### Abzaehlbarkeit

Eine beliebige Menge $S$ ist *abzaehlbar*, wenn $|S| \leq
|\mathbb{N}_0|$. Andernfalls ist $S$ *ueberabzaehlbar*. Der Hauptgedanke ist,
das wenn man die Elemente einer Zahl sequentiell auflisten kann, dann ist diese
Menge *abzaehlbar*. Kann man dies nicht, ist sie *ueberabzaehlbar*.

Eine Menge $S$ ist also abzaehlbar, wenn man eine Bijektion von der Menge in die
Menge der natuerlichen Zahlen mit $0$ machen kann. Das geht, weil wir wissen,
das man die Menge der natuerlichen Zahlen auflisten kann. Beispiel fuer die
Menge der ganzen Zahlen:

Wir bilden jedes Element $z \in \mathbb{Z}$, wenn es positiv ist, auf eine
gerade natuerliche Zahl ab, und wenn es negativ ist, auf eine ungerade Zahl:

$f(x) = \begin{cases}2x \text{ wenn } i \geq 0\\ -2x -1 \text{ sonst
}\end{cases}$

Somit ist diese Menge zwar unendlich, aber man kann die Elemente darin
abzaehlen.

Mit der Menge der reellen Zahlen geht das nicht. Zum Beispiel kann man die
Zahlen im Intervall $[0, 1]$ nicht sequentiell auflisten, weil die Menge
beliebig tief ist.

## Kardinalitaeten

Einige Aussagen zu Kardinalitaeten im Bezug auf Relationen:

* Es gibt $2^{n^2}$ moegliche Relationen auf einer Menge mit $n$ Elementen,
  bzw. allgmein $2^{|A| \cdot |B|}$ auf zwei Mengen $A, B$ (also wo $R \subseteq
  A \times B$). Beweis: Es gibt $|A \times B| = |A| \cdot |B|$ im Kreuzprodukt,
  welches alle moeglichen Tupel enthaelt. Nun enthaelt die Potenzmenge dieser
  Menge alle moeglichen Relationen. Bekanntlich ist die Kardinalitaet einer
  Potenzmenge $2^{|M|}$.

* Die Anzahl der *reflexiven Relationen* laesst sich ganz einfach aus obigem
  berechnen. Man nimmt aus $A \times A$ einfach alle $|A|$ reflexiven Tupel raus
  (naemlich die in $Id_A$) und berechnet die Kardinalitaet der restlichen:
  $2^{\mathcal{P}(A \setminus Id_A)} = 2^{|A|^2 - |A|} = 2^{|A|(|A| - 1)}$. Also
  kurz, wenn $n = |A|$: $2^{n(n-1)}$

* Die Anzahl der *symmetrischen Relationen* berechnet sich so: Sei $|A| = n$,
  dann gibt es $n^2$ Elemente im Kreuzprodukt, von denen $n$ die reflexiven
  sind und $n(n-1)$ die symmetrischen Paare $(a, b), (b, a)$ mit $a, b \in A, a
  \neq b$. Nun betrachten wird die beiden separat. Von den reflexiven koennen
  wir beliebig waehlen, also $2^n$. Von den Paaren wollen wir immer nur eines,
  da das andere dann implizit drin sein muss. Also haben wir $n(n+1)/2$ Paare,
  also haben wir fuer sie $2^{n(n-1)/2}$ Moeglichkeiten. Also:
  $2^n \cdot 2^{n(n-1)/2} = 2^{n(n+1)/2}$.

* Die Anzahl der *asymmetrischen Relationen*. Die reflexiven lassen wir
  natuerlich weg. Dann haben wir also $n(n-1)/2$ Paare. Nun fuer jedes Paar das
  eine enthalten sein, oder das andere, oder keines (aber nie beide). Also drei
  Moeglichkeiten: $3^{n(n-1)/2}$

* Die Anzahl der *antisymmetrischen Relationen* beinhaltet nun wieder alle
  Moeglichkeiten fuer reflexive Tupel. Ansonsten haben wir fuer jedes Paar noch
  immer die drei Moeglichkeiten das erste zu waehlen, das zweite oder keines:
  $2^n \cdot 3^{n(n-1)/2}$

* Die Anzahl der *totalen Relationen* berechnet sich gleich wie die Anzahl der
  *asymmetrischen*. Die reflexiven Tupel muessen enthalten sein, also haben wir
  hier keine Wahlmoeglichkeiten. Fuer alle weiteren Paare muss nun das eine, das
  andere oder beide enthalten sein: $3^{n(n-1)/2}$

* Da die Stirling Zahlen zweiter Art die Anzahl $k$-Partitionen einer
  $n$-elementigen Menge angeben, gilt dass die Anzahl moeglicher
  Aequivalenzrelation ist: $\sum_{i=0}^n S_{n,k}$.

Frage: Wie viele Relationen $\bullet$ ueber einer $n$-elementigen Menge gibt es,
wo $a \bullet b \rightarrow a < b$?
Antwort: Wie immer faengt man mit dem Kreuzprodukt an, also mit $n^2$
Elementen. Keine solche Relation kann reflexiv sein, also sind wir wieder bei
den $n(n-1)/2$ Paaren. Anders als dem Fall der asymmetrischen Relation haben wir
hier aber eine geforderte Ordnung der Elemente der Tupel, also kommt immmer nur
eines der Elemente in Frage. Also haben wir nicht $3^{n(n-1)/2}$ Moeglichkeiten,
wie bei der asymetrischen (wo das eine oder ander Tupel erlaubt war), sondern
nur $2^{n(n-1)/2}$ Moeglichkeiten.

## Beweistechniken

* Um zu zeigen, dass $A \subseteq B$ reicht es zu zeigen, dass jedes Element
  $\in A$ auch in $B$ enthalten ist!

## Beweise

Gegeben sind $f: A \rightarrow B$ und $g: B \rightarrow C$


Zeigen Sie: Sind $f$ und $g$ injektiv, so ist auch $g \circ f$ injektiv.

Beweis: Da $f$ auf $B$ abbildet, ist $f(a) \in B$ fuer beliebige $a \in A$. Sei
nun $b := f(a)$. Dann koennen wir $b$ in $g \circ f = g(f(a))$ einsetzen und
erhalten $g(b)$. Wir wissen, dass $g(b), b \in B$ injektiv ist, also ist auch $g
\circ f$ injektiv.

Kuerzer: Es gelte $g(f(a)) = g(f(b))$. Da $g$ injektiv ist, folgt $f(a) = f
(b)$. Da $f$ injektiv ist, folgt $a = b$. Also ist auch $g \circ f$ injektiv.


Zeigen Sie: Ist $g \circ f$ surjektiv, so ist auch $g$ surjektiv.

Beweis: Sei $a$ ein beliebiges Element $\in A$. Da $f$ von $A$ nach $B$
abbildet, gilt $f(a) = b$, wo $b \in B$. Also kann man $(g \circ f)(a)$
schreiben als $g(f(a)) = g(b)$. Da $g(f(a))$ surjektiv war und $b = f(a)$ ist
also auch $g(a)$ surjektiv.

Kuerzer: Waehle ein beliebiges $c \in C$. Da $g \circ f$ surjektiv ist, gibt es
ein $a$ sodass $g(f(a)) = c$. Dann gibt es ein $b$ sodass $g(b) = c$, naemlich
wenn $b = f(a)$. Also hat jedes $c \in C$ offensichtlich ein Urbild, daher ist
$g$ surjektiv.


Zeigen Sie: $f(X\setminus Y) \supseteq f(X)\setminus f(Y) \forall X,Y
\subseteq A$.

Beweis: Um zu zeigen, dass $f(X)\setminus f(Y) \subseteq f(X\setminus Y)$
genuegt es, zu zeigen, dass jedes Element in der linken Menge auch in der
rechten ist. Also sei $x \in X$ sodass $f(x) \in f(X)\setminus f(Y)$. Dann ist
$f(x) \in X$, aber nicht $\in Y$. Daher ist auch $x \in X$ und $\notin Y$. Dann
ist $x \in X \setminus Y$ und somit ist $f(x) \in f(X \setminus Y)$.
