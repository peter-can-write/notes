# Algebraische Strukturen

Die Algebra ist ein Teilgebiet der Mathematik, die sich mit algebraischen
Strukturen befasst. Eine algebraische Struktur ist eine Menge von Elementen und
eine Menge von Operationen, die auf der Menge definiert sind. Von diesen
Operationen sind nur gewisse Eigenschaften bekannt, die aber schon auf viele
Aussagen und Eigenschaften bezueglich der Struktur schliessen lassen.

Eine *Algebra* besteht dabei aus einer Traegermenge $S$ und einer Menge $\Phi$
von Operationen auf $S$ (der *Operatorenmenge*). Ein Operator der Stelligkeit
(*arity*) $m \in \mathbb{N}$ ist dann eine Abbildung $S^m \rightarrow
S$, also eine Abbildung von $m$-Tupeln mit Elementen aus der Traegermenge, auf
ein Element aus der Traegermenge. Operationen muessen also immer innerhalb der
Traegermenge bleiben, sodass die Algebra vollstaendig bzw. gueltig ist.

Eine Algebra mit Traegermenge $S$ und beliebigem Operator $\circ$ wird dabei als
$\langle S, \circ \rangle$ geschrieben.

Eine Algebra ist beispielsweise die Bool'sche. Die Traegermenge ist hierbei die
Menge der atomaren Wahrheitskonstanten $\{true, false\}$ und die Operatoren sind
die zweistelligen $\lor, \land$ und der einstellige Operator $\neg$.

## Operatoren

Ein Operator $\circ$ ueber einer Menge $A$ mit Aritaet $n$ ist eine Funktion:

$\circ: A^n \rightarrow A$.

Eine Menge $B \subseteq A$ heisst dabei abgeschlossen in $\circ$, wenn fuer alle
$a_1,...,a_n$ gilt:

$a_1,...,a_n \in B \Rightarrow \circ(a_1,...,a_n) \in B$.

Operatoren mit Stelligkeit $1$ heissen dabei *unaer*, mit Stelligkeit $2$
*binaer* und mit Stelligkeit $3$ ternaer. Fuer unaere Operatoren schreiben wir
statt $\circ(x)$ oft $\overset{\circ}{x}$ (z.B. Komplement) oder $\circ x$ (z.B
Negation). Bei binaeren Operatoren schreiben wir hingegen meist $a \circ b$
anstatt $\circ(a, b)$. Das nennt man dann *Infixnotation*.

### Operatortafel

Eine Algebra mit einem zweistelligen Operator laesst sich dabei eindeutig durch
eine *Operatortafel* definieren. Dabei ist in der linken oberen Ecke der Tabelle
der Operator und in der linken Spalte sowie oberen Zeile die Traegermenge
angegeben. In jedem inneren Feld steht dann das Resultat der Verknuepfung zweier
Elemente aus der Traegermenge durch den Operator:

|$\land$|$true$ |$false$|
|:------|:------|:------|
|$true$ |$true$ |$false$|
|$false$|$false$|$false$|

An der Operatortafel eines Operators erkennt man die Eigenschaften:

* Die Algebra ist abgeschlossen in $\circ$, wenn alle Elemente in der
  Tabelle in der Traegermenge sind.

* Die Algebra ist assoziativ. Hierfuer muss man alle Moglichkeiten ($|S|^3$)
  ausprobieren.

* Das neutrale Element, wenn es eines gibt. Es ist jenes, das den anderen
  Operanden erhaelt.

* Das inverse Element, wenn es eines gibt. Es ist jenes, das das neutrale
  Element produziert.

* Die Operation ist kommutativ. Das sieht man, wenn die Elemente in allen
  Diagonalen symmetrisch sind.

## Neutrales Element

Neutrale Elemente eines Operators sind jene Elemente aus der Traegermenge, die
bei der Verknuepfung mit einem anderen Element wieder das andere Element
produzieren. Beispielsweise die $0$ bei der Addition $+$ auf der Menge der
natuerlichen Zahlen.

Sei $\langle S, \circ \rangle$ eine Algebra. Ein Element $e \in S$ ist
bezueglich einer Operation $\circ$ linksneutral, wenn gilt:

$\forall a \in S: e \circ a = a$.

Wenn es also auf der linken Seite einer Operation ist, und immer das rechte
Element als Resultat hat.

Symmetrisches gilt fuer rechtsneutrale Elemente. Ist ein Element sowohl links-
als auch rechtsneutral, so ist es allgemein *neutral*. Ein neutrales Element
*erhaelt* bei einer Verknuepfung mit einem anderen Element also immer das
andere.

$\forall a \in S: e \circ a = a \circ e = a$

## Inverses Element

Ein inverses Element $x$ eines Elements $a$ der Traegermenge fuer eine Operation
$\circ$ ist jenes Element, welches bei Verknuepfung mit $a$ das neutrale Element
$e$ der Operation produziert. Es kann theoretisch ein eindeutiges inverses
Element geben, doch meist gibt es fuer jedes $a$ ein anderes inverses
Element. Bei der Addition $+$ auf der Menge der ganzen Zahlen ist das inverse
Element eines jeden Elements $a$ beispielsweise immer $-a$.

Sei $\langle S, \circ \rangle$ also eine Algebra mit einem neutralen Element $e$
und sei $a \in S$. Ein rechtsinverses Element $x \in S$ von $a$ ist eines, wo
gilt: $a \circ x = e$. Symmetrisches fuer linksinverses.

Eine links- und rechtsinverses Element nennt man dann allgemein *invers*.

$1/x$ ist invers bezueglich der Algebra $\langle \mathbb{R}\setminus \{0\},
\cdot\rangle$.

## Strukturen

Es gibt nun Bezeichnungen fuer Algebren, die bestimmte Eigenschaften besitzen:

* Halbgruppe: assoziativ
* Monoid: assoziativ und neutrales Element.
* Gruppe: assoziativ und neutrales uns inverses Element.

### Algebra

Eine Algebra $\langle S, \Phi \rangle$ nennt man eine Traegermenge $S$ mit
Operatormenge $\Phi$, wenn die Traegermenge in jedem Operator der
Operatorenmenge abgeschlossen ist, sodass gilt:

$\forall \circ \in \Phi: \circ(S) \rightarrow S$

### Halbgruppen

Eine Algebra $\langle S, \circ \rangle$ mit einem *binaeren* Operator
$\circ$ heisst Halbgruppe, falls $\circ$ __assoziativ__ ist:

$\forall a, b, c \in S: a \circ (b \circ c) = (a \circ b) \circ c$

In der Aussagenlogik ist $\rightarrow$ z.B. nicht assoziativ:

$(false \rightarrow false) \rightarrow false \equiv false$
$false \rightarrow (false \rightarrow false) \equiv true$

__Der Operator muss binaer sein__!

### Monoide

Eine Algebra $\langle S, \circ \rangle$ mit einem zweistelligen Operator $\circ$
heisst Monoid, falls $\circ$ __assoziativ__ ist und es ein __neutrales Element__
gibt.

Fuer Funktionen ist das neutrale Element die Identitaetsfunktion $Id$, also fuer
die Algebra $\langle F(U), \circ \rangle$ ($F(U)$ ist die Menge aller
Abbildungen $U \rightarrow U$)

$\langle \mathbb{N}, + \rangle$ ist beispielsweise Halbgruppe, aber kein
Monoid. Assoziativitaet ist zwar gegeben, aber es gibt kein neutrales Element
(gibt es erst bei $\mathbb{N}_0$).

### Gruppe

Eine Gruppe ist nun eine Algebra $\langle S, \circ \rangle$, die sowohl
__assoziativ__ ist (Halbgruppe), als auch ein __neutrales Element__ hat (Monoid)
aber nun auch ein __inverses Element__ zu jedem Element hat.

* $\langle \mathbb{Z}, +\rangle$ ist eine Gruppe.
* $\langle \mathbb{N}_0, +\rangle$ ist zwar Monoid, aber keine Gruppe. Es gibt
  kein $x \in \mathbb{N}$, sodass $1 + x = 0$.

Eine Gruppe hat immer nur ein einziges neutrales Element $e$, aber kann *viele*
inverse Elemente haben. Meist gibt es zu jedem Element der Traegermenge ein
anderes inverses Element.

Beispiele:

* $\langle \mathbb{Z}_n, +_n \rangle$_ ist eine Gruppe:
  + Die Addition ist assoziativ.
  + $0$ ist das neutrale Element.
  + $(n - a)$ ist das inverse Element $\forall a$.


* $\langle \mathbb{Z}_5\setminus \{0\}, \cdot_5 \rangle$ ist eine Gruppe:
  + Die Multiplikation ist assoziativ.
  + $1$ ist das neutrale Element.
  + Inverse Elemente: $(1, 1)$, $(2, 3)$, $(3, 2)$ und $(4, 4)$.


* $\langle \mathbb{Z}_4\setminus\{0\}, \cdot_4 \rangle$ ist keine Gruppe:
  + $2$ hat kein inverses Element, weil $\nexists a: 2 \cdot a \mod 4 = 1$
    (muesste $5, 9, 13, ...$ ) sein.

#### Abelsche Gruppen

Eine Gruppe, ein Monoid, oder eine Halbgruppe heisst *abelsch* oder
*kommutativ*, falls $\circ$ kommutativ ist, also gilt:

$\forall a, b \in S: a \circ b = b \circ a$

Auch gilt:

$\langle \mathbb{Z}_n, +_n \rangle$_ ist abelsch $\forall n$.

### Inneres Produkt

Das Innere Produkt $\times$ zweier Algebren $\langle S_1, \circ_1 \rangle,
\langle S_2, \circ_2 \rangle$ ist kreeirt die Algebra:

$\langle S_1 \times S_2, \circ_1 \times \circ_2 \rangle$

Die Traegermenge enthaelt also alle moeglichen Paare $(a, b)$ mit $a \in S_1, b
\in S_2$. Hierbei ist das Kreuzprodukt der Operatoren so definiert:

$(a,b)(\circ_1 \times \circ_2)(c,d) = (a \circ_1 c, b \circ_2 d)$

Also jeweils die Elemente aus der selben Gruppe werden mit demselben Operator
verknuepft.

Beispiel: $\langle \mathbb{Z}_5, +_5\rangle \times \langle\mathbb{Z}_3, +_3
\rangle$.

Hier sind die Elemente des inneren Produkts alle Paare $(a,b)$ mit $a \in
\mathbb{Z}_5$ und $b \in \mathbb{Z}_3$. Dann:

$(a,b)(+_5 \times +_3)(c,d) = (a +_5 c, b +_3 d)$.

Eigenschaften des inneren Produkts:

Seien $\langle A, \circ_1 \rangle$, $\langle B, \circ_2 \rangle$ zwei Algebren.

Das innere Produkt der beiden Algebren:

* hat $|A \times B| = |A| \cdot |B|$ Elemente.

* ist assoziativ, genau dann wenn $A$ und $B$ jeweils assoziativ sind.

* hat genau dann ein neutrales Element $(e_A, e_B)$, wenn $A$ ein neutrales
  Element $e_A$ hat und $B$ ein neutrales Element $e_b$.

* hat genau dann fuer jedes Element $(a,b)$ ein Inverses $(a,b)^{-1} = (a^{-1},
  b^{-1})$, wenn $a^{-1} \in A, b^{-1} \in B$.

* ist kommutativ, genau dann wenn $A$ und $B$ jeweils assoziativ sind.

* ist nicht zwingend zyklisch, wenn $A$ und $B$ zyklisch sind (die
  Gruppenordnungen passen nicht mehr).

## Eigenschaften von Gruppen

Sei $\langle S, \circ \rangle$ eine Gruppe. Dann gilt:

1. $S$ enthaelt __genau ein neutrales__ Element $e$.

2. *Jedes* $a \in S$ hat genau ein inverses Element $a^{-1}$.

3. Involution: $\forall a \in S: a = (a^{-1})^{-1}$

4. Kuerzungsregel: $\forall a, b, c \in S$:
   * Wenn $a \circ c = b \circ c$, dann $a = b$
   * Wenn $c \circ a = c \circ b$, dann $a = b$

5. Eindeutige Loesung linearer Gleichungen: $\forall a, x, b \in S$:
   * $a \circ x = b$, dann $x = a^{-1} \circ b$
   * $x \circ a = b$, dann $x = b \circ a^{-1}$

6. Injektivitaet von $\circ$: Fuer alle $a, b, c \in S$:
   * $a \neq b \Leftrightarrow a \circ c \neq b \circ c$
   * $a \neq b \Leftrightarrow c \circ a \neq c \circ b$

7. Surjektivitaet von $\circ$: Fuer alle $a, b \in S$:
   * $\exists x \in S: a \circ x = b$
   * $\exists y \in S: y \circ a = b$

Die Kuerzungsregel besagt, dass in jeder Spalte und jeder Zeile der
Operatortafel fuer $\circ$ ein Element nur ein einziges mal vorkommen darf. Denn
wenn es ein $a \in S$ gibt, sodass $b \circ c = a$ und weiter in der Zeile auch
$b \circ d = a$, dann wuerde gelten $b \circ c = b \circ d$ und nach
Kuerzungsregel $c = d$, aber man zeichnet fuer jedes Element $\in S$ nur eine
Zeile und Spalte in der Operatortafel. Man nennt diese Regel deswegen auch
Sudokuregel.

### Beweise

1. Seien $e_1,e_2$ neutrale Elemente: $e_1 = e_1 \circ e_2 = e_2$

2. Seien $i_1,i_2$ inverse Element von $a$: $i_1 = i_1 \circ e = i_1 \circ (a
   \circ i_2) = (i_1 \circ a) \circ i_2 = e \circ i_2 = i_2$.

3. $(a^{-1})^{-1} =: b = b \circ e = b \circ (a^{-1} \circ a) = (b \circ a^{-1})
   \circ a = e \circ a = a$.

4. Unter der Annahme dass $a \circ c = b \circ c$: $b = b \circ (c \circ c^{-1})
   = (b \circ c) \circ c^{-1} = (a \circ c) \circ c^{-1} = a \circ (c \circ
   c^{-1}) = a$.

### Potenzen

Sei $\langle S, \circ \rangle$ eine Gruppe und sei $a \in S$. Wir definieren:

* $a^0 := e$

* $\forall n \geq 1: a^n := a \circ a^{n - 1} = a^{n - 1} \circ a$.

* $\forall n \geq 1: a^{-n} := (a^{-1})^n$

Achtung: $a^n$ heisst jetzt nicht mehr $n$ mal mit sich selbst *multiplizieren*,
sondern man fuehrt die konkrete Gruppenoperation *n* mal zwischen $a$ und sich
selbst aus! Zum Beispiel ist bei der Addition $a^b = a \cdot b$.

### Ordnung eines Gruppenelements

Die Ordnung eines Elements einer Gruppe ist jene Hochzahl, die man dem Element
geben muss, sodass man zum neutralen Element kommt. Also es gibt an, wie oft
man die Gruppenoperation zwischen einem Element *und sich selbst* durchfuehren
muss, sodass das Ergebnis das neutrale Element $e$ ist.

$ord(a) := \min\{n \in \mathbb{N} \, | \, a^n = e\}$

Sei $\langle S, \circ \rangle$ also eine Gruppe mit neutralem Element $e$ und
sei $a \in S$. Die Ordnung $ord(a)$ von $a$ ist die kleinste Zahl $n \in
\mathbb{N}$, sodass $a^n = e$. Falls kein solches $n$ existiert, dann ist
$ord(a) := \infty$. Man beachte dass ein $n \in \mathbb{N}$ gesucht ist, also
ohne $0$. Also kann man nicht sagen $x^0 = e \,\forall x$.

Man kann die Ordnung eines Elements in einer Operatortafel ablesen. Man bleibt
dabei einfach in der *Spalte* von $a$, und angefangen bei $a^2$, also $2$,
zaehlt man dann zu wie vielen anderen Elementen man in der Spalte gehen muss, um
zum neutralen Element (in der Spalte) zu kommen.

#### Beispiele

* In $\langle \mathbb{N}_5, \cdot_5 \rangle$ ist $ord(3) = 3$, weil $3^3 = 81$
  und $81 \mod 5 = 1$.

* Bei $\langle \mathbb{Z}, + \rangle$ ist $ord(1) = \infty$.

Hier eine Operatortafel:

|$\circ$|$e$|$x$|$y$|$z$|
|:------|:--|:--|:--|:--|
|  $e$  |$e$|$x$|$y$|$z$|
|  $x$  |$x$|$e$|$z$|$y$|
|  $y$  |$y$|$z$|$x$|$e$|
|  $z$  |$z$|$y$|$e$|$x$|

* $e = e$, daher $ord(e) = 1$

* $x \xrightarrow{\circ x} e$, daher $ord(x) = 2$

* $y \xrightarrow{\circ y} x \xrightarrow{\circ y} z \xrightarrow{\circ y} e$,
  daher $ord(x) = 4$.

* $z \xrightarrow{\circ z} x \xrightarrow{\circ z} y \xrightarrow{\circ z} e$,
  daher $ord(x) = 4$.

* Spaltentrick fuer $y$: Angefangen bei $y^2$, also zwei, zaehlen wir $3$ fuer
  $y^2 = x$, dann $4$ fuer $x \circ y = e$.

### Untergruppe

Eine Untergruppe ist eine Gruppe, dessen Traegermenge Teilmenge der Traegermenge
einer anderen Gruppe ist. Also ist $\langle S, \circ \rangle$ eine Gruppe und
$S' \subseteq S$, so heisst $\langle S', \circ \rangle$ *Untergruppe* von
$\langle S, \circ \rangle$, *wenn* sie selbst eine Gruppe ist.

Beispielsweise ist $\langle \mathbb{Z}, + \rangle$ Untergruppe von $\langle
\mathbb{Q}, + \rangle$.

Eigenschaften der Untergruppe:

* Es enthaelt das selbe neutrale Element wie die urspruengliche Gruppe.

* Der Schnitt aus zwei Untergruppen (bzw. deren Traegermenge) ist wieder eine
  Untergruppe.

* Das Erzeugnis $\langle \{a^i \,|\, i \in \mathbb{Z}\}, \circ \rangle$ von
  $a$ ist eine Untergruppe von $\langle S, \circ \rangle$, wo $a \in S$.

* $\langle S, \circ \rangle, \langle\{e\}, \circ\rangle$ sind triviale
  Untergruppen von $\langle S, \circ\rangle$.

* Ihre Kardinalitaet teilt die der Gruppe.

* Die Untergruppe mit Traegermenge $\{0\}$ ist die einzige endliche Untergruppe
  von $\langle \mathbb{Z}, + \rangle$.

* Jede Gruppe $n \cdot \mathbb{Z}, n \in \mathbb{N}$ ist eine Untergruppe der
  Gruppe $\langle \mathbb{Z}, + \rangle$. Beispiele:
  + Die Gruppe mit $\{2n | n \in \mathbb{Z}\}$ ist eine Untergruppe.
  + Die Gruppe mit $\{2n + 1 | n \in\mathbb{Z}\}$ ist keine Untergruppe.

Frage: Ist $\langle S', \circ\rangle$ Untergruppe von $\langle S, \circ
\rangle$?

* Ist die Untergruppe trivial (mit Menge $S$ oder $\{e\}$)?
* Enthaelt es das selbe neutrale Element? Ueberhaupt eins?
* Ist es das Erzeugnis von einem Element der Gruppe?
* Hat jedes Element ein inverses?
* Teilt die Untergruppenordnung die Gruppenordnung?

#### Rezept

Frage: Wie findet man alle Untergruppen eines Gruppe $\langle S, \circ \rangle$?

Methode: Bestimme zuerst fuer jedes $a \in S$ das Erzeugnis und die
Ordnung. Das ergibt schon mal die Menge aller zyklischen Untergruppen.

1. Ist $\langle S, \circ \rangle$ zyklisch, so gibt es keine weiteren
   Untergruppen ausser den zyklischen.

2. Sind alle nicht-trivialen Teiler von $|S|$ (alle ausser $1$ und $|S|$) prim,
   so gibt es auch keine weiteren Untergruppen ausser den zyklischen.

3. Fuer jeden nicht-trivialen Teiler $k$ von $|S|$, der nicht prim ist, versuche
   eine Untergruppe mit $k$ Elementen durch Ausprobieren zu finden. Die
   *Ordnungen dieser Elemente sollen alle $k$ teilen und $< k$ sein!*

Zu (3): Ist $|S| = 8$, sind die nicht-trivialen Teiler $2$ und $4$. Von denen
ist nur $4$ nicht prim, also versucht man eine Untergruppe von $4$ Elementen zu
finden, wo die Ordnung jeden Elements $4$ teilt und kleiner $4$ ist (sonst gaebe
es in der Untergruppe den Fall $ord(a) = |U| = 4$, aber wir haben die zyklischen
Gruppen ja schon gefunden).

#### Beweise

Hier nun einige Beweise zu obigen Aussagen.

##### Lemma

Lemma: Sei $G$ eine Gruppe und sei $H$ eine Untergruppe von $G$. Die neutralen
Elemente von $G$ und $H$ sind identisch.

Beweis:

1. $e_H \circ e_H = e_H = e_G \circ e_H$
2. Kuerzen: $e_H \circ e_H = e_G \circ e_H \Rightarrow e_H = e_G$

Ist $H$ also eine Untergruppe von $G$, dann haben sie das selbe neutrale Element
$e$.

##### Satz

Seien $\langle S_1, \circ \rangle$, $\langle S_2, \circ \rangle$ Untergruppen
von $\langle S, \circ \rangle$. Dann ist auch $\langle S_1 \cap S_2, \circ
\rangle$ eine Untergruppe von $\langle S, \circ \rangle$.

Beweis:

1. $S_1 \cap S_2$ muss das neutrale Element von $S$ enthalten. Da nach vorherigem
   Lemma gilt $e \in S_1$ und $e \in S_2$, gilt auch $e \in S_1 \cap S_2$.

2. $S_1 \cap S_2$ muss Teilmenge von $S$ sein. Insbesondere muss $S_1 \cap S_2$
   also die selben inversen Elemente fuer jedes $a$ enthalten, wie auch $S$. Da
   $a^{-1}$ in $S$ eindeutig ist (siehe Eigenschaft), und $S_1,S_2$ bestimmt
   Untergruppe sind, gilt $a^{-1} \in S_1 \land a^{-1} \in S_2$, also $a^{-1}
   \in S_1 \cap S_2$.

##### Satz

Sei $\langle S, \circ \rangle$ eine endliche Gruppe und $a \in S$. Dann ist

$\langle \{a^0, a^1, ..., a^{ord(a) - 1}\} = \{a^1, a^2, ..., a^{ord(a)}\}, \circ \rangle$

eine Untergruppe von $\langle S, \circ \rangle$. Die Kardinalitaet von dieser
Gruppe ist $ord(a)$. Die Gruppe ist auch zyklisch, weil es das Generator-Element
$a$ enthaelt.

Beweis:
1. Das inverse Element $a^0 = e$ ist noch immer enthalten.
2. Das neutrale Element von $a^k$ ist $a^{ord(a) - k}$.

__Einfacher Konstruktionsmechanismus von Untergruppen!!__

### Nebenklassen

Eine Nebenklasse einer Untergruppe *in* einer Gruppe ist die Gruppe, die
entsteht, wenn man jedes Element in der Untergruppe mit einem Element aus der
Gruppe *verknuepft*. Die entstandene Gruppe ist dabei nicht unbedingt wieder
eine Untergruppe.

Sei $H = \langle T, \circ \rangle$ eine Untergruppe von $G = \langle S, \circ
\rangle$ und sei $b \in S$. Dann ist

$T \circ b := \{c \circ b \, | \, c \in T\} =: H \circ b$

die *rechte Nebenklasse* von *$H$ in $G$* und

$b \circ T := \{b \circ c \, | \, c \in T\} =: b \circ H$

die *linke Nebenklasse*.

* Jede Nebenklasse von $H$ hat hierbei gleich viele Elemente wie $H$ (folgt aus
  Injektivitaet von $b \in S$).

* Der Index $ind_G(H)$ von $H$ in $G$ ist die Anzahl von *verschiedenen* linken
  Nebenklassen von $H$ in $G$.

__Die Nebenklassen von $H$ stellen eine Partition von $G$ dar.__ Das gilt sowohl
fuer die rechte als auch fuer die linke Nebenklasse.

#### Satz von Lagrange

Sei $G$ eine endliche Gruppe und $H$ eine Untergruppe von $G$. Es gilt:

1. $|H|$ teilt $|G|$.

2. Alle Nebenklassen von $H$ in $G$ haben gleich viele Elemente, naemlich $|H|$
   viele.

3. $|G| = ind_G(H) \cdot |H|$ bzw. $|G|/|H| = ind_G(H)$.

4. $ord(a)$ teilt $|G|$.

5. Ist $|G|$ prim, muessen alle Elemente ausser dem neutralen Element ($ord(1)$)
   Ordnung $|G|$ haben.

##### Beispiel

Frage: Sei $\langle [307], \circ \rangle$ eine Gruppe.

1. Wieviele Untergruppen hat die Gruppe?
2. Welche Ordnung hat $28$ in der Gruppe?

Antwort:

1. Da $307$ prim ist, und laut Lagrange die Untergruppenordnung die
   Gruppenordnung teilen muss, kann es nur die trivialen Untergruppen mit
   Traegermengen $\{e\}$ und $[307]$ geben.

2. Da die Ordnung von $28$ die Gruppenordnung teilen muss, kann die Ordnung nur
   $1$ oder $307$ sein. Es ist nicht $1$, weil nur das neutrale Element hat die
   Ordnung $1$. Also ist $ord(307)$.

### Additive Gruppen Modulo $n$

Sei $n \in \mathbb{M}$ und $\mathbb{Z}_n := \{0,...,n-1\}$ die Menge aller
moeglichen Reste einer Division durch $n$ und $+_n$ die Addition modulo $n$ mit:

$a +_n b := (a + b) \mod n$.

Dann ist $\langle \mathbb{Z}_n, +_n \rangle$ fuer alle $n \in \mathbb{Z}$ eine
Gruppe. Also __jede additive Gruppe Modulo $n$ ist eine Gruppe__.

* Assoziativ? Ja, weil Addition assoziativ ist.
* Neutrales Element? Ja, weil $\mathbb{Z}_n$ von $0$ weg geht.
* Inverses Element? Ja, immer $n - a \,\forall a$.

Man schreibt $\mathbb{Z}_n$ auch als $\mathbb{Z}/n\mathbb{Z}$.

Man bemerke, dass die Potenz in einer additiven Gruppe aequivalent zur
Multiplikation ist, da $x \cdot a = x + x + x ...$ ist. Also gilt: $a^n = a
\cdot n$ wenn die Gruppenoperation $+$ ist.

Eigenschaften:

* $|\mathbb{Z}_n| = |\{0,...,n-1\}| = n$

* Das neutrale Element ist die $0$.

* Das inverse Element von $a \in \mathbb{Z}_n$ ist $n - a$ bzw. $-a \mod n$.

* Die Gruppe ist __immer abelsch und zyklisch__.

* Zyklisch mit den Generatoren $1$ und $n - 1$, sowie manchmal Weiteren.

* Sei $a \in \mathbb{Z}_n$, dann ist $ord(a) = \frac{n}{ggT(a,n)}$.

* Alle teilerfremden Elemente haben $ord(a) = n$.

Beweis zum letzten:

Die Ordnung ist in der additiven Gruppe jene kleinste positive Zahl $o$, sodass
die Gruppenordnung $n$ die Zahl $a^o = a \cdot o$ teilt: $n | oa$. Diese Zahl wird gerade
durch das kleinste-gemeinsame Vielfache $kgV(a,n)$ ausgedrueckt. Nun gibt es die
Regel:

$kgV(a,n) = \frac{na}{ggT(a,n)}$

Da $oa = kgV(a,n)$ galt: $oa = \frac{na}{ggT(a,n)}$.

Kuerzt man nun $a$ weg, kommt man zum Schluss: $o = \frac{n}{ggT(a,n)}$.

Daher: In jeder additiven Gruppe modulo $n$ ist $ord(a) = \frac{n}{ggT(a,n)}$.

### Menge von Teilerfremden Zahlen Modulo $n$

Fuer $n \in \mathbb{N}, n \geq 2$ ist die Menge:

$\mathbb{Z}^\star_n := \{m \in \mathbb{Z} \,|\, ggT(n, m) = 1\}$

aller Zahlen aus $\mathbb{Z}_n$, die zu $n$ teilerfremd sind. Da $ggT(0, n) =
0$ ist $0$ nicht enthalten, und da $ggT(1, n) = 1$ ist $1$ schon enthalten. $n$
ist schon gar nicht in $\mathbb{Z}_n$ enthalten, weil $\mathbb{Z}_n$ die Menge
der Reste der Division mit $n$ ist.

$\mathbb{Z}_n$ enthaelt also $\{0,...,n-1\}$ und $\mathbb{Z}_n^\star$ die
teilerfremden Zahlen zu $n$ aus dieser Menge. Da die $0$ immer in
$\mathbb{Z}_n$ ist aber nie in $\mathbb{Z}_n^\star$, gilt immer:

$|\mathbb{Z}_n^\star| < |\mathbb{Z}_n|$ fuer $n \geq 1$.

Wenn $n$ prim ist, sind alle Elemente ausser der $0$ teilerfremd zu $n$, also
ist $|\mathbb{Z}^\star_n| = n - 1$.

### Multiplikative Gruppen Modulo $n$

Die Gruppe $\langle \mathbb{Z}_n^\star, \cdot_n\rangle$ nennt man die
*multiplikative Gruppe modulo $n$*. Sie ist immer eine Gruppe.

Eigenschaften:

* $|\mathbb{Z}_n^\star| = \phi(n)$.

* Das neutrale Element ist die $1$.

* Inverse Elemente bestimmt man mit dem erweiterten Euklidischen Algorithmus.

* $\langle\mathbb{Z}_n^\star, \cdot_n \rangle$ ist abelsch, aber nicht immer
  zyklisch.

* Wenn $\langle \mathbb{Z}_n^\star, \cdot_n\rangle$ zyklisch ist, gilt, dass
  sie isomorph zur additiven Gruppe modulo $\phi(n)$ ist.

Beispiel: $\langle \mathbb{Z}_8^\star, \cdot_8\rangle$

Die Traegermenge ist hier $\mathbb{Z}_8^\star = \{1, 3, 5, 7\}$

#### Multiplikatives Inverses

Sei $x \in \mathbb{Z}^\star_n$ beliebig. Da $\mathbb{Z}^\star_n$ nur Zahlen
enthaelt, die zu $n$ teilerfremd sind, gilt: $ggT(x, n) = 1$. Der erweiterte
euklidische Algorithmus berechnet dann $a, b \in \mathbb{Z}: ax + by = ggT(x,
n) = 1$. Es gilt also $a \cdot_n x + b \cdot_n n = 1$. Da $b \cdot_n n$
trivialerweise ein Vielfaches von $n$ ergibt, gilt $b \cdot_n n = 0$. Somit muss
$a \cdot_n x = 1$ sein. Dann gilt:

$x^{-1} = a \mod n$.

Das heisst, um das inverse Element eines Elements in einer multiplikativen
Gruppe modulo $n$ zu finden, fuehre den erwggT aus, finde somit $a$, und nimm es
modulo $n$.

#### Kardinalitaet und $\phi$-Funktion

Die Kardinalitaet $|\mathbb{Z}^\star_n|$ einer multiplikativen Gruppe modulo
$n$ wird angegeben durch die Funktion $\phi(n)$, auch genannt Euler'sche
Phi-Funktion (englisch: Totient-Function). Sie berechnet "die Anzahl an Zahlen
kleiner-gleich $n$, die zu $n$ teilerfremd sind". $n$ ist zwar nicht in der
multiplikativen Gruppe, aber da $n$ nicht teilerfremd zu $n$ ist ist die
Definition dennoch passend.

Da eine Primzahl nur durch $n$ und sich selbst teilbar ist (und die Phi-Funktion
fuer $n$ eigentlich $n + 1$ Zahlen betrachtet, naemlich $0,...,n$) gilt, dass
von den $n + 1$ betrachteten genau $n - 1$ teilerfremd zu $n$ sind, also:

$n \text{ ist prim } \Rightarrow \phi(n) = n - 1$.

Eine weitere Eigenschaft der $\phi$-Funktion ist, dass sie fuer teilerfremde
Zahlen multiplikativ ist. Das heisst, wenn $n,m \in \mathbb{N}$ und $ggT(m, n)
= 1$, dann:

$\phi(m \cdot n) = \phi(m) \cdot \phi(n)$

Wenn also $m,n$ prim sind, gilt $\phi(m \cdot n) = (m - 1) \cdot (n - 1)$.

https://en.wikipedia.org/wiki/Euler%27s_totient_function

##### Berechnung

Ist die Primfaktorenzerlegung $n = p_1^{e_1} \cdot p_2^{e_2} \cdot ... \cdot p_k^{e_k}$ von $n$ bekannt, dann gilt:

$\phi(n) = p_1^{e_1 - 1}(p_1 - 1) \cdot p_2^{e_2 - 1}(p_2 - 2) \cdot ... \cdot
p_k^{e_k - 1}(p_k - 1)$

Beispiel: $360 = 2^3 \cdot 3^2 \cdot 5$
Also:     $2^{3 - 1}(2 - 1) \cdot 3^{2 - 1}(3 - 1) \cdot 5^{1 - 1}(5 - 1)$
          $2^2(1) \cdot 3(2) \cdot 1(4) = 4 \cdot 6 \cdot 4 = 96$

Somit auch der Beweis des Satzes von oben, dass wenn $m,n$ prim sind $\phi(m
\cdot n) = (m-1)(n-1)$. Denn wenn $m,n$ prim sind, sind sie schon die
eindeutigen Primfaktoren von $m \cdot n$. Dann gilt also:

$\phi(m \cdot n) = m^{1-1}(m-1) \cdot n^{1-1}(n-1) = (m-1)(n-1)$.

#### Satz von Euler

Seien $a,n \in \mathbb{N}$ zueinander teilerfremd, dann gilt:

$a^{\phi(n)} \equiv_n 1$

Und daher:

$a^x \mod n = a^{x \mod \phi(n)} \mod n \,\forall x \in \mathbb{Z}$.

##### Beispiel

Was ist $3^{11425735486239} \mod 4$ ?

Da $ggT(3, 4) = 1$, sind $3,4$ teilerfremd. Also gilt:

$3^{11425735486239} \mod 4 = 3^{11425735486239 \mod \phi(4)} \mod 4$

$\phi(4) = 2^{2-1}(2-1) = 2$, also:

$3^{11425735486239 \mod 2} \mod 4 = 3^1 \mod 4 = 3$


##### Beweisrezept

"Beweise dass $g: \mathbb{Z}_N^\star \rightarrow \mathbb{Z}_N^\star: x \mapsto
(x^m \mod n)$ eine Permutation ist, indem du die Inverse Funktion $g^{-1}$
findest". $n$ sei prim und $m \in \mathbb{Z}$ und teilerfremd zu $n$.

Beweis: $x^m \mod n = x^{m \mod \phi(n)} \mod n$ da $m,n$ teilerfremd. Also ist
die Umkehrfunktion gerade $x^{m^{-1} \mod \phi(n)}$. Bestimmte also einfach das
Multiplikative Inverse von $m$ __modulo $\phi(n)$__.

Dann: $g^{-1}: x \mapsto x^{-m} \mod n$.

### Zyklische Gruppen

Eine Gruppe $\langle S, \circ \rangle$ heisst *zyklisch*, wenn es ein $a \in S$
gibt, sodass:

$S = \{a^i \,|\, i \in \mathbb{Z}\}$.

Das Element $a$ nennt man dann das *erzeugende Element*, *Erzeuger* oder
*Generator* der Gruppe. Man bemerke dass $i$ hier auch negative Werte annimmt,
somit fuer jedes $a^i$ auch das inverse $a^{-i}$ enthalten ist.

* Ist $a^n = e$ fuer ein $n \in \mathbb{N}$, dann gilt $S = \{a^1,...,a^n\}$,
  also die Gruppe ist endlich.

* __Die Gruppe ist endlich und zyklisch $\Leftrightarrow$ es gibt ein Element $a
  \in S: ord(a) = |S|$ (wirklich bidirektional!).__

* Wenn $|S|$ __prim__ ist, ist die Gruppe __immer zyklisch__.

* Jede __zyklische Gruppe ist kommutativ__.

* Da nur Elemente $a$ einer Gruppe mit $ord(a) = |S|$ Erzeuger der Gruppe sind,
  und das nur gilt wenn $ggT(a, |S|) = 1$, gilt auch, dass die Anzahl von
  Erzeugern einer endlichen zyklischen Gruppe gerade durch die Phi-Funktion
  $\phi(|S|)$ angegeben wird.

#### Beispiel

$\langle \mathbb{Z}, + \rangle$ ist z.B. zyklisch mit erzeugendem Element $1$.

Frage: Gibt es eine nicht-zyklische Gruppe $\langle S, \circ, \rangle$ mit $|S|$
prim?

Antwort: Nein. Die Ordnung $ord(a)$ jeden Elements $a \in S$ muss die
         Gruppenordnung $|S|$ teilen. Da $|S|$ prim ist, hat diese Zahl nur die
         Teiler $1$ und $|S|$. Nur das neutrale Element hat die Ordnung $1$,
         also gibt es $|S| - 1$ Elemente mit Ordnung $|S|$. Schon eines genuegt,
         um zu sagen: $S$ muss zyklisch sein.

### Isomorphismus

Ein *Isomorphismus* zwischen zwei Gruppen $\langle S_1, \circ_1 \rangle$ und
$\langle S_2, \circ_2 \rangle$ ist eine Bijektion $f: S_1 \rightarrow S_2$,
sodass:

$f(a \circ_1 b) = f(a) \circ_2 f(b)$ fuer alle $a, b \in S_1$.

Wir nennen zwei Grupppen, zwischen denen es einen Isomorphismus gibt
*isomorph*. Wir benutzen dann das Zeichen $\cong$ als Isomorphie-Relation
zwischen zwei Gruppen. Intuitiv heisst das, dass die beiden Gruppen die selbe
Struktur (Eigenschaften) bzw. Beziehungen zwischen ihren Elementen haben, und
nur unterschiedlich benannt sind. Man muss dann die Elemente der einen Gruppe
ueber die Bijektion nur umbenennen, und hat schon die neue Gruppe. Die Struktur
und Eigenschaften bleiben erhalten.

Sind zwei Gruppen isomorph mit Bijektion $f$, gilt:

* $ord(a) = ord(f(a))$
* $h(a^{-1}) = b^{-1}$

Seien beispielsweise $\langle A, \circ_1 \rangle, \langle B, \circ_2 \rangle$
zwei Algebren mit $A = \{a, b, c\}, B = \{r, s, t\}$ und $h: \{a,b,c\} \rightarrow
\{r,s,t\}$ eine Bijektion:

* $a \mapsto s$
* $b \mapsto r$
* $c \mapsto t$

Dann koennen wir die Operatortafel von $B$ erzeugen, indem wir die Elemente der
Operatortafel nach der Bijektion umbenennen, und die Spalten/Zeilen alphabetisch
sortieren:
2
```
|A  |a|b|c|                  | |-|s|r|t|                 |B|-|s|r|t|
|-|-|-|-|-|                  |-|-|-|-|-|                 |-|-|-|-|-|
|a|-|a|b|c| ==Umbenennung==> |s|-|s|r|t| ==Sortierung==> |r|-|t|r|s|
|b|-|b|c|a|                  |r|-|r|t|s|                 |s|-|r|s|t|
|c|-|c|a|b|                  |t|-|t|s|r|                 |t|-|s|t|r|
```

Nun haben wir $A$ exakt die Operatortafel von $B$ erhalten, nur durch
Umbenennung.

Es gibt vier Indizien, ob zwei Gruppen isomorph sind:

1. Sind die Gruppen unterschiedlich gross, kann es keinen Isomorphismus geben
  (schon keine Bijektion).

2. Sind die Ordnungen der Elemente beider Gruppen anders, also hat die eine
   Gruppe eine andere Menge von Ordnungen ihrer Elemente, dann koennen sie auch
   nicht isomorph sein.

3. Sind die beiden Gruppen zyklisch, so kann man den einen Erzeuger $a$ auf den
   anderen abbilden $b$ abbilden, und dann gilt $a^n \mapsto b^n \forall n$.

4. Ist eine Gruppe endlich und zyklisch und die andere nicht, folgt aus (3)
   zusammen mit (4) dass sie nicht isomoroph sein koennen.

Bestimmen eines Isomorphismus: Die Struktur der Gruppen sind gleich. Bilde also
zuerst das neutrale Element der einen Gruppe auf das der anderen ab. Dann, bilde
Elemente auf Elemente mit der selben Ordnung ab, da diese ja gleich
bleiben. Benutze dann die Isomorphismuseigenschaft $h(x \circ_1 y) = h(x)
\circ_2 h(y)$ um die restlichen Elemente zu bestimmen.

Satz: Ist $G = \langle S, \circ \rangle$ eine __zyklische Gruppe__, dann gilt:

* Ist $S$ unendlich, dann ist $G$ isomorph zu $\langle \mathbb{Z}, + \rangle$
* Ist $S$ endlich, dann ist $G$ isomorph zu $\langle \mathbb{Z}_{|S|}, +_{|S|}\rangle$

Hat man also eine beliebige Gruppe beliebiger Zahlen von der man weiss, dass sie
zyklisch ist. Dann weiss man alles ueber die Gruppe, weil sie isomorph zur
zyklischen Gruppe mit der selben Laenge ist.

### Homomorphismus

Homorphismus ist wie Isomorphismus, nur dass die Bedingung wegfaellt, dass die
Abbildung bijektiv sein muss. Es muss fuer zwei Gruppen $\langle S_1, \circ_1
\rangle$ und $\langle S_2 \circ_2 \rangle$ nur mehr gelten:

Sei $f: S_1 \rightarrow S_2$ eine Abbildung, dann: $\forall a,b \in S_1: f(a
\circ_1 b) = f(a) \circ_2 f(b)$.

Ein Isomorphismus ist also ein bijektiver Homorphismus.

### Symmetrische Gruppe

Eine symmetrische Gruppe fuer $n$ Elemente ist die Gruppe $S_n = \langle U_n,
\circ \rangle$, wobei $\circ$ die Komposition von Abbildungen ist und $U_n$ die
Menge aller Permutationen von $[n]$.

* Eine symmetrische Gruppe enthaelt immer $|U_n| = n!$ Elemente.

* Das inverse Element zu jeder Permutation in $U_n$ ist $\pi^{-1}$, dass die
  Elemente wieder ordnet (zur Identitaet).

* Das neutrale Element zu jeder Permutation $\pi^n \in U_n$ ist die Identitaet
  $Id_S$, z.B. $(1)(2)(3)$ fuer $[3]$.

* Eine symmetrische Gruppe ist kommutativ und zyklisch fuer $n = 1, n = 2$,
  ansonsten weder kommutativ noch zyklisch.

* Die Ordnung $ord(\pi)$ einer Permutation $\pi \in S_n$ ist das kleinste
  gemeinsame Vielfache $kgV\{l_1,...,l_k\}$ der Zyklenlaengen $l_1,...,l_k$ von
  $\pi$. Z.B. in $S_9$: $ord((1,7,4,3)(2,8,6)(5,9)) = kgV{4,3,2} = 12$ ($4$ ist
  die Laenge von $(1,7,4,3)$ usw.).

## Koerper

### Ringe

Eine Algebra $\langle S, \oplus, \odot \rangle$ mit zweistelligen Operatoren
$\oplus$ und $\odot$ heisst *Ring*, falls gilt:

1. $\langle S, \oplus, \rangle$ ist eine *abelsche Gruppe*.

2. $\langle S, \odot \rangle$ ist ein *Monoid* (Assoz. + $e$)

3. Die Distributivgesetze gelten fuer $\odot$ ueber $\oplus$:
   * $a \odot (b \oplus c) = (a \odot b) \oplus (a \odot c)$
   * $(b \oplus c) \odot a = (b \odot a) \oplus (c \odot a)$

Falls $\langle S, \odot \rangle$ ein kommutatives Monoid ist, ist der Ring
kommutativ.

### Koerper

Eine Algebra $\langle S, \oplus, \odot \rangle$ mit zweistelligen Operatoren
$\oplus$ und $\odot$ heisst *Koerper*, falls:

1. $\langle S, \oplus \rangle$ eine abelsche Gruppe mit neutralem Element $0$
   ist.

2. $\langle S \setminus \{0\}, \odot \rangle$ ist eine abelsche Gruppe (man
   beachte dass die Null hier rausgenommen wird, nicht aus $S$ allgemein!).

3. Die Distributivgesetze gelten.

__Satz: Fuer alle $n \geq 2$ ist $\langle \mathbb{Z}_n, +_n, \cdot_n \rangle$
ein Koerper gdw. $n$ eine Primzahl ist.__

Jeder Koerper ist dabei auch ein Ring, aber nicht umgekehrt.

#### Beweisrezept

Ist der Koerper $\langle K, \oplus, \odot \rangle$ ein Koerper?

1. Abgeschlossenheit: Ueberpruefe ob fuer $a,b \in K$ gilt:
   * $a \oplus b \in K$
   * $a \odot b  \in K$

2. Neutrale Elemente: Finde fuer beide Operationen ein neutrales Element.
   * $a \oplus ? = a$
   * $a \odot  ? = a$

3. Inverse Elemente: Finde fuer beide Operationen ein inverses Element:
   * $a \oplus ? = e$
   * $a \odot  ? = e$

4. Ueberpruefe Kommutativitaet.

5. Uberpruefe Assoziativitaet.

#### Primitive Elemente

Man nennt ein Element *primitiv*, wenn es Erzeuger der multiplikativen Gruppe
$\langle K\setminus \{0\}, \odot \rangle$ eines Koerpers ist. Beispielsweise ist
$2$ das primitive Element des Koerpers $\langle \mathbb{Z}_3, +_3, \cdot_3
\rangle$.

#### Isomorphie

Ebenso wie bei Algebren mit nur einem Operator koennen auch Algebren mit zwei
Operatoren Isomorphie-Eigenschaften besitzen. Dabei hat man wieder *eine*
Bijektion $f: A \rightarrow B$ zwischen der Menge $A$ der einen Algebra und der
Menge $B$ der anderen Algebra, und die Isomorphie-Eigenschaft $f(a \circ_1 b) =
f(a) \circ_2 f(b)$ muss fuer beide Paare von Operatoren gelten.

Sei z.B. $\langle A, \oplus, \odot \rangle$ ein Koerper, und $\langle B, \circ,
\square \rangle$ ein anderer Koerper. Diese Koerper sind *isomorph*, wenn es
eine Bijektion $f: A \rightarrow B$ zwischen den Traegermengen gibt, sodass
gilt:

1. $f(a \oplus b) = f(a) \oplus  f(b)$
2. $f(a \odot b)  = f(a) \square f(b)$

### Unterschiede

Die wesentlichen Unterschiede zwischen Ringen und Koerpern sind:

* Alle Elemente eines Koerpers ausser der $0$ haben Inverse bezueglich $\odot$,
  die eines Ringes nur manchmal.

* $\odot$ ist bei Koerpern immer kommutativ, bei Ringen nur manchmal.

* Koerper sind immer nullteilerfrei, Ringe nur manchmal.

Ein Nullteiler ist ein $x \in S$, sodass $\exists y \in S: x \odot y = 0$. Also
ein Element, die in ihrer Zeile in der Operatortafel einen $0$-Eintrag hat.

### Eigenschaften von Strukturen

Was kann man in jeder der Strukturen: Monoid, Gruppe, Ring, Koerper, machen?

* Monoid: addieren
* Gruppen: addieren und subtrahieren
* Ringe: addieren, subtrahieren, multiplizieren
* Koerper: addieren, subtrahieren, multiplizieren, dividieren

Hierbei ist die Subtraktion $a - b$ einfach die Addition mit dem additiven
Inversen $b^{-1}$: $a - b := a + b^{-1}$.

Ebenso ist die Division eine Multiplikation mit dem Inversen:
$a/b = a \cdot b^{-1}$

### Polynomkoerper

In einem Polynomkoerper sind die Elemente des Koerpers nicht mehr Zahlen,
sondern Polynome wie $x^2 + 1$. Sei also $\langle K, +, \cdot \rangle$ ein
(kommutativer) Ring (oder Koerper). Ein Polynom ueber $K$ in der Variablen $x$
ist ein Ausdruck der Gestalt:

$\sum_{i=0}^n a_i x^i$

wobei $n \in \mathbb{N}_0$ und __$a_i \in K$__ und $a_n \neq 0$.

Der Grad des Polynoms ist $n$ und seine Koefffizienten sind $a_0,...,a_n$. Der
Polynom $p(x) = 0$ hat dabei nach Definition den Grad $-\infty$. Ein Polynom ist
dabei zunaechst aber nur ein Ausdruck, noch keine Funktion. Eine Funktion $f(x)$
kann aber in einem Polynom $p(x)$ *induziert* werden. Zwei Polynome koennen
verschieden sein, aber die selbe Funktion induzieren. Das gilt dann, wenn die
Koeffizienten modulo gerechnet werden. Dann ist z.B. in $\mathbb{Z}_2$ $f(x) =
2x^2$ die selbe Funktion wie $f(x) = 0$, aber die Polynome sind verschieden weil
die Koefffizienten verschieden sind und der Grad auch.

Zwei Polynome sind *gleich*, wenn sie die selben Koeffizienten und denselben
Grad haben.

$K[x]$ bezeichnet die Menge der Polynome ueber dem Ring $K$ in der
Variablen $x$, mit den Koeffizienten aus $K$. $K[x]\setminus p(x)$ ist dann der
Restklassenring, von $K[x]$ modulo $p(x)$, ist also darin bezueglich der
Multiplikation abgeschlossen. Multipliziert man zwei Polynome aus $K[x]$, kann
dabei ein Polynom hoeher oder gleichen Grades als $p(x)$ resultieren. Dann muss man eine
Polynomdivision mit $p(x)$ machen, um den Rest zu berechnen.

#### Operationen

Seien zwei Polynome $a(x) = \sum_{i=0}^n a_i x^i$ und $b(x) = \sum_{i=0}^n b_i
x^i$ gegeben.

#### Summe

Die Summe von $a(x), b(x)$ wird gebildet, indem man einfach alle Koeffizienten
fuer einen Term eines bestimmten Grades addiert: $\sum_{i=0}^n (a_i + b_i)
x^i$. Hierbei muss der Grad der Summe kleiner-gleich dem Maximum der beiden
Polynomgrade sein (er kann sich durch Subtraktion reduzieren).

#### Differenz

Die Differenz von $a(x), b(x)$ wird gebildet, indem man einfach alle
Koeffizienten subtrahiert: $\sum_{i=0}^n (a_i - b_i) x^i$

#### Produkt

Das Produkt von $a(x), b(x)$ wird gebildet, indem man die beiden Seiten
ausmultipliziert und anschliessend sortiert und zusammenfast. Hierbei kann aus
Polynomen von Grad $m$ bzw. $n$ maximal $m + n$ sein (wegen der modularen
Arithmetik kann er auch weniger sein). Dann gilt allgemein:

$\sum_{i=0}^{m+n}\sum_{j=0}^i a_j b_{i-j} x^i$

Man kann sich also, um schneller zu sein, zuerst ansehen, wo ueberall ein Term $x^i$ eines bestimmten Grades $i$ zusammenkommt, und dann gleich die
Koeffizienten multiplizieren und zusammenzaehlen:

$a(x) = x^2 + 3x + 5$, $b(x) = 4x + 2$:

1. $x^3$ bei $x^2 \cdot 4x$, also: $(1 \cdot 4)x^3$
2. $x^2$ bei $x^2 \cdot 2$, $3x \cdot 4x$: $(1 \cdot 2 + 3 \cdot 4)x^2$
3. $x^1$ bei $3x \cdot 2$, $5 \cdot 4x$: $(3 \cdot 2 + 5 \cdot 4)x^1$
4. $x^0$ bei $5 \cdot 2$: $5 \cdot 2$

Also $4x^3 + 14x^2 + 26x + 10$

#### Division

Die Division wird gebildet, indem eine Polynomdivision durchfuehrt. Dies ist nur
moeglich, wenn der Leitkoeffizient (Koeffizient des Terms hoechsten Grades)
*invertierbar* ist (ein inverses hat). Dann gibt es zu je zwei Polynomen $a(x),
b(x)$ *eindeutig* bestimmte Polynome $q(x)$ (Quotient) und $r(x)$ (Rest), sodass
$a(x) = q(x) \cdot b(x) + r(x)$. Dann gilt auch entweder:

* $r = 0$ (kein Rest), oder
* $grad(r) < grad(b)$.

Modulo einer Zahl ist die Polynomdivision ein wenig anders. Aber nur insofern,
dass alle Koeffizienten modulo $n$ genommen werden. D.h., man kann eine negative
Zahl $a$ als $-a$ darstellen, oder $n - a$. Auch muss man manchmal andere
Faktoren im Quotient nehmen. Will man z.B. $3x^2$ durch $2x$ dividieren, koennte
man in $\mathbb{R}$ den Quotienten $3/2x$ nehmen. Aber wenn wir in einem
Koerper mit $\mathbb{Z}$ rechnen, sind Brueche nicht erlaubt. Also modulo $5$
waere $3x^2 \div 2x$ beispielsweise $4x$, weil $2x \cdot 4x = 8x^2$, und $8 \mod
5 = 3$.

#### Allgemein

Man beachte vor Allem, dass wir in $\mathbb{Z}_n$ *modulo* rechnen. Wenn wir
also in $\mathbb{Z}_n$ zwei Polynome aus $\mathbb{Z}_n[x]$ addieren, wie $4x +
2x$ in $\mathbb{Z}_6$, dann kommt $0 \cdot x = 0$ raus.

Also nochmal, die Grade:

* Summe: $grad(a(x) + b(x)) \leq max\{grad(a(x)), grad(b(x))\}$
* Produkt: $grad(a(x) \cdot b(x)) \leq grad(a(x)) + grad(b(x))$

Fuer Polyonme auf Koerpern gilt hier $=$ und nicht $\leq$.

#### Teilbarkeit

Ein Polynom $a(x)$ teilt einen anderen Polynom $b(x)$, wenn es einen
Quotient-Polynom $q(x)$ gibt, sodass $b(x) = q(x) \cdot a(x)$. Wenn es also
keinen Rest bei der Division gibt.

#### Restklassen

Ebenso ist $a(x)$ zu $b(x)$ kongruent modulo $\pi(x)$, wenn die Differenz
$a(x) - b(x)$ durch $\pi(x)$ teilbar ist. Dann gilt also $a(x) \equiv b(x) \mod
\pi(x)$. Dann haben $a(x)/\pi(x)$ und $b(x)/\pi(x)$ den selben Rest.

Beispiel: $(x+1) \cdot x \mod (x^2 + x) = (x^2 + x) \mod (x^2 + x) = 0$.

Die moeglichen Reste der Division eines Polynoms aus $K[x]$, wo $K =
\mathbb{Z}_n$, durch einen Polynom $\pi(x)$ sind die Polynome mit Koeffizienten
aus $K$, mit Graden kleiner als der von $\pi(x)$ (Restklassen vom Grad, so wie
bei Zahlen). Den kommutativen Ring $\langle K[x]_\pi, +_\pi, \cdot_\pi\rangle$
nennt man dann *Restklassenring* $K$ modulo $\pi$. Hierbei ist dieser Ring genau
dann auch ein Koerper, wenn $p(x)$ irreduzibel ist (so wie der additiv-multiplikative Koerper modulo $n$ nur dann ein Koerper ist, wenn $n$ prim ist).

Da es fuer jeden der $\deg(\pi)$ Terme $x^0,x^1,...,x^{\deg(\pi) - 1}$ $|K|$
Moeglichkeiten fuer Koeffizenten gibt, gilt fuer den Ring $K[x]_\pi$:

$|K[x]_\pi| = |K|^{\deg(\pi)}$

Somit, wenn (wie meist) $K = \mathbb{Z}_n$:

$|\mathbb{Z}_n[x]_\pi| = n^{\deg(\pi)}$

Den Restklassenring $K$ modulo dem Polynom $\pi$ schreibt man auch $K/\pi$.

##### Kongruenz

Die Kongruenzrelation $\equiv$ teilt dabei die Menge $K[x]$ der Polynome in der
Variablen $x$ des Rings $K$ in Aequivalenzklassen bezueglich einem Polynom
$\pi$:

$K[x]_\pi := \{f(x) \in K[x] \,|\, grad(f) < grad(\pi)\}$

Wenn $K$ endlich ist, ist auch $K[x]_\pi$ endlich. Es gilt dann:

* $f(x) +_\pi g(x) := (f(x) + g(x)) \mod \pi$.
* $f(x) \cdot_\pi g(x) := (f(x) \cdot g(x)) \mod \pi$.

##### Beispiel

Sei $K = \mathbb{Z}_3$ und $\pi(x) = x^2 + 1$. Dann ist $\langle
\mathbb{Z}_3[x]_\pi, +_\pi, \cdot_\pi\rangle$ der *Restklassenring* modulo
$\pi$. Die Restpolynome sind nun alle mit Grad $0,...,grad(\pi(x)) - 1$ also
hier $0$ oder $1$. Und die Koeffizienten alle aus $\mathbb{Z}_3 =
\{0,1,2\}$. Man beachte, dass wenn der Grad $a$ ist, man noch alle
Moeglichkeiten von $x^{a-1},x^{a-2},...,x^{a - a}$ zu $x^a$ addiert, in Betracht
ziehen muss:

1. Grad $0$: $0, 1, 2$ bzw. $0 \cdot_3 x^0, 1 \cdot_3 x^0$
2. Grad $1$: $x, 2x, x + 1, x + 2, 2x + 1, 2x + 2$

Frage: Wieviele Polynome aus dem Restklassenring $\mathbb{Z}_3[x]_\pi$ gibt es
hier nun?

Antwort: $3^{\deg(\pi)} = 3^2 = 9$.

##### Beispiel

Frage: Sei $q = 2x^2 + x$ ein Polynom ueber $\mathbb{Z}_3$ und $R = \langle
\mathbb{Z}_3[x]_\pi, +_q, \cdot_q\rangle$ ein Restklassenring. Fuelle diesen
Ausschnitt der Operatortafel aus:

|$\cdot_q$|$x+1$|$x+2$|
|:--------|:----|:----|
|  $2x$   |     |     |
| $2x + 1$|     |     |

Antwort:

1. $(2x)(x+1) = 2x^2 + 2x$, $2x^2 + 2x \div q = x$ Rest.

2. $(2x)(x+2) = 2x^2 + 4x$, $2x^2 + 4x \div q = 3x$ Rest, $3x \mod 3 = 0$ Rest.

3. $(2x+1)(x+1) = 2x^2+2x+x+1 = 2x^2+1 \mod 3$, $2x^2+1 \div q = 1$ Rest.

4. $(2x+1)(x+2) = 2x^2+4x+x+2 = 2x^2+2x+2 \mod 3$, $2x^2+2x+2 \div q = x + 2$
    Rest.

Wie man sieht, muss man also immer darauf achten, dass die Koeffizienten modulo
$n$ von $\mathbb{Z}_n$ (hier $3$) betrachtet werden, und die Polynome als
Ganzes immer modulo $\pi$. Deswegen machen wir nach jeder Multiplikation, wenn
der Grad des Produkts groesser oder gleich dem von $\pi$ ist, eine
Polynomdivision.

#### Faktorisierung

Wie auch Zahlen haben Polynome $p \in K[x]$ eindeutige Faktoren $p_1,...,p_n \in
K[x]$, sodass gilt: $\Pi_{i=1}^n p_i = p$. Dabei gilt auch:

$x_0$ ist Nullstelle von $p$ gdw. ($\Leftrightarrow$) $(x-x_0)$ ein Faktor von
$p$.

Wie berechnet man nun die Nullstellen, also Faktoren eines Polynoms?

1. Fuer lineare Funktionen indem man die Gleichung loest.

2. Fuer quadratische Funktionen, Loesungsformel oder siehe (3).

3. Fuer hoehere Funktionen, finde eine Nullstelle $x_0$ durch Probieren, und
   fuehre dann eine Polynomdivision mit $(x - x_0)$ durch. Mache diese so lange,
   bis die Funktion Grad $2$ hat und man sie in die Loesungsformel speissen
   kann, oder bis sie linear ist und man die Gleichung loesen kann.

Die Loesungsformel ist dabei: $x_1,x_2 = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$.

Wenn man mit Modulo rechnet, muss man eben wieder auf die Koeffizienten achten.

#### Irreduzibilitaet

Ein *irreduzibler* Polynom ist einer, durch den man nicht restlos
polynom-dividieren kann. Der Rest ist nie $0$, sondern immer irgendein
Restpolynom $r(x)$. Ein irreduzibler Polynom ist also so wie eine Primzahl in
der Domaene der Zahlen.

Formal: Sei $K$ ein Koerper. Ein Polynom $p \in K[x]$ heisst reduzibel, wenn
zwei Polynome $p_1,p_2 \in K[x]$ existieren mit __$\deg(p_1), \deg(p_2) \geq
1$__ und $p = p_1 \cdot p_2$. Wenn zwei solche Polynome nicht existieren, nennt
man den Polynom $p$ *irreduzibel*.

Aus der Definition folgt, dass erst quadratische Polynome reduzibel sein
koennen. Denn der Grad der Polynomfaktoren muss $\geq 1$ sein ($x - x_0$ ...),
und somit sind Polynome $a_0$ (also nur Koeffizienten) oder $a_0 \cdot x_0$
nicht reduzibel. Fuer lineare Terme muesste naemlich einer der beiden Polynome
Grad $0$ haben bzw. $1$ sein.

Weiters folgt, dass ein Polynom vom Grad $2$ oder $3$ genau dann irreduzibel
ist, wenn es *keine Nullstelle* hat. Denn bei Graden $2$ und $3$ gilt noch,
dass, wenn man einen solchen Polynom aufspalten will in zwei Polynome $p_1,p_2$,
zumindest einer der Polynome Grad $1$ haben muss. Wir wissen aber, dass es einen
solchen linearen Faktor nur geben kann, wenn er die Form $x - x_0$ hat, wo $x_0$
eine Nullstelle ist. Weil es keine Nullstelle gibt, kann es keinen solchen
Faktor geben, und der Polynom ist also irreduzibel.

Gleichzeitig kann ein Polynom hoeheren Grades reduzibel sein, obwohl es keine
Nullstellen hat. Denn ab Grad $4$ kann man den Polynom in zwei Polynome
aufspalten, wo keiner der beiden mehr Grad $1$ haben muss, also linear sein muss
und die Form $x - x_0$ haben muss. Man kann den Polynom z.B. in zwei
quadratische Polynome aufspalten. Diese Polynomfaktoren muessen keine bestimmte
Form mehr haben, also ist es egal, dass der Polynom keine Nullstellen hat.

Beispiel: $p = x^4 + 2x^2 + 1 = (x^2 + 1)(x^2 + 1)$ ist reduzibel, obwohl $p$
          keine Nullstellen hat.

Hingegen: $p = x^3 + 5x^2 - 7$ = (\text{Grad 2})(\text{Grad 1})$. Weil es keine
          Nullstelle gibt, gibt es keinen Polynom vom Grad $1$.

Somit zusammenfassend, sei $p \in K[x]$ ein Polynom:

* $\deg(p) \leq 1 \Rightarrow$ der Polynom $p$ ist immer irreduzibel.

* $2 \leq \deg(p) \leq 3 \Rightarrow$ der Polynom $p$ ist *irreduzibel* genau
  dann wenn ($\Leftrightarrow$) der Polynom *keine Nullstelle* hat.

* $\deg(p) \geq 4 \Rightarrow$ der Polynom kann reduzibel sein, auch wenn er
  keine Nullstelle hat. Aber wenn er irreduzibel ist, hat er keine (nicht
  unbedingt umgekehrt!).

## Modulare Arithmetik

Seien $a \in \mathbb{Z}, b \in \mathbb{N}$, dann ist die Operation $a \mod n$
definiert als:

$a \mod n := a - \lfloor \frac{a}{n} \rfloor \cdot n$

Der Wert von $a \mod n$ ist hierbei der Rest der Division $a/n$. Diesen erhaelt
man also, indem man den ganzzahligen Teil $\lfloor a/n \rfloor \cdot n$ der
Division von $a$ abzieht.

Sei $m \in \mathbb{N}$. Zwei Zahlen $x, y \in \mathbb{Z}$ sind kongruent
modulo $m$, genau dann wenn:

* Sie in der selben Restklasse modulo $m$ sind (bei der Division beider Zahlen
  mit $m$ kommt der selbe Rest raus).

* Die Differenz $(x - y)$ durch $m$ teilbar sind.

* Es $k \in \mathbb{Z}$ gibt, mit $x = y + k \cdot m$

Die Kongruenz modulo $m$ ist eine Aequivalenzrelation auf $\mathbb{Z}$. Sie
wird $x \equiv_m y$ oder $x \equiv y \mod m$ geschrieben.

Vor Allem bei grossen Zahlen ist die Definition von $a \mod n$ als $a - \lfloor
a/n \rfloor \cdot n$ hilfreich. Z.B. ist $437 \mod 7 = 437 - \lfloor
437/7\rfloor \cdot 7 = 437 - 62 \cdot 7 = 3$.

### Notation

Es gibt nun einige Schreibweisen fuer die modulare Arithmetik:

1. $(x \mod y)$ ist der Rest der Division $x / y$.

2. $x +_m y$ ist eine Abkuerzung fuer $(x + y) \mod m$

3. $x \cdot_m y$ ist eine Abkuerzung fuer $(x \cdot y) \mod m$.

4. $\mathbb{Z}_m$ ist die Menge $\{0, 1, ..., m - 1\}$, wo $m \geq 2$. Hierbei
   nennt man $m$ den *Index* der Menge.

Man beachte, dass Modulo-Zahlen auch negativ sein koennen. Dann sind sie relativ
zu Zahl, zu der Modulo gerechnet wird. Also z.B. ist die Restklasse $-1$ in
$\mod n$ das selbe wie $n - 1$.

Beispiele:

1. $(5 \mod 3) = 2$

2. $5 +_3 6 = 2$, weil $5 + 6 = 11$ und $11 \mod 3 = 2$.

3. $7 \cdot_5 2 = 4$, weil $7 \cdot 2 = 14$ und $14 \mod 5 = 4$.

4. $\mathbb{Z}_4 = \{0, 1, 2, 3\}$

### Axiome

Fuer beliebige $a,b,n \in \mathbb{N}$ gilt:

1. $(a + b) \mod n = ((a \mod n) + (b \mod n)) \mod n$

2. $(a \cdot b) \mod n = ((a \mod n) \cdot (b \mod n)) \mod n$

3. $(a \cdot b) \mod (a \cdot n) = a (b \mod n)$


4. Aus (2): $x^y \mod n = (x \mod n)^y \mod n$, weil $x^y = x \cdot x ...$

#### Beispiele

* $2^{168} \mod 3 = (2^2)^{84} \mod 3 = (4 \mod 3)^{84} \mod 3 = 1^{84} \mod 3 =
  1$

* $(10^{85} + 5^{63} + 12^{47}) \mod 3$:
  + $10 \mod 3 = 1$
  + $5^{63} \mod 3 = 5 \cdot (5^2)^{31} \mod 3 = 5 \cdot 1^{31} = 5 \cdot 1 = 5$
  + $12^{47} \mod 3 = (12 \mod 3)^{47} \mod 3 = 0^{47} \mod 3 = 0$
  + Also: $1 + 5 + 0 \mod 3= 6 \mod 3 = 0$.

## Groesster Gemeinsamer Teiler

Der Groesste Gemeinsame Teiler (ggT) zweier Zahlen $x, y \in \mathbb{N}$ ist
die groesste natuerliche Zahl, die sowohl $x$ als auch $y$ teilt. Dabei gilt
dass $ggT(x, y) = 1$ gdw. $x$ und $y$ teilerfremd sind.

### Euklidischer Algorithmus

Es gibt einen einfachen Algorithmus, um das ggT zweier Zahlen zu
berechnen. Hierbei nutzen wir die folgende Definition des $ggT$ aus. Seien $x, y
\in \mathbb{Z}$ mit $x \leq y$:

1. $ggT(x, y) = x$, wenn $y \mod x = 0$
2. $ggT(x, y) = ggT(y \mod x, x)$, sonst.

Hierbei sei angemerkt, dass $y \mod x < x$.

Beweis:

1. Wenn $x$ teilt $x$ und $x$ teilt $y$ dann ist $x$ ggT weil es keine Zahl
   groesser $x$ sein kann.

2. Allgemein gilt $y = (y \mod x) + \lfloor y/x \rfloor \cdot x$. Hierbei ist $y
   \mod x$ der Rest der Division $\lfloor y/x \rfloor$ und der andere Term das
   ganzzahlige Ergebnis von $y/x$. Wenn es also ein $z \in \mathbb{N}$ gibt,
   sodass $z|x \land z|y$, dann gilt, weil $z|x|\lfloor y/x \rfloor \cdot x$,
   dass $z|x \land z|y \mod x$. $z$ teilt $\lfloor y/x \rfloor \cdot x$, weil
   $z$ $x$ teilt. Also muss man nur noch zeigen, dass $z$ auch $y \mod x$ teilt
   (ein Summe ist durch eine Zahl teilbar, wenn beide Summanden durch die Zahl
   teilbar sind).

Also gibt es den folgenden Algorithmus, gennant *Euklidischer Algorithmus*:

```
def ggT(x, y):
	if y % x == 0:
		return x
	return ggT(y % x, x)
```

Man muss hier auch nicht darauf achten, dass $x \leq y$, weil die Argumente
vertauscht werden.

#### Fuer Menschen

1. Variante: Man malt sich eine Tabelle mit zwei Spalten fuer die Zahlen $x$
und $y$. In die erste Zeile schreibt man $x$ und $y$ gleich rein. Dann gilt
fuer jede neue Zeile:

* $x_{neu} = y$
* $y_{neu} = x \mod y$ (der Rest).

Terminiere, wenn $y$ in einer Zeile $0$ ist. Dann ist das $x$ aus der Spalte der
$ggT$.

Beispiel: $x = 76, y = 28$

|$x$|$y$|
|:--|:--|
|76 |28 |
|28 |20 |
|20 | 8 |
| 8 | 4 |
| 4 | 0 |

Der ggT von $(76, 28)$ ist also $4$. Man kann auch schon aufhoeren, wenn man
sieht, dass $y \mod x = 0$.

2. Variante: Man schreibt $x, y$ in eine einzige Spalte uebereinander, den Rest
   dann immer darunter. Dann ist von den untersten zwei Werten immer der oberste
   $x$ und der unterste gerade $y$.

Beispiel: $x = 129, y = 96$

|$r$| <-- "Rest"
|:--|
|129|
| 96|
| 33|
| 30|
|  3|
|  0|

Also $ggT(129, 96) = 3$.

3. Variante: Das geht dann sogar in einer Zeile:

Beispiel: $x = 53, y = 46$: $r_i = 53, 46, 7, 4, 3, 1, 0$.

Fertig, $ggt(53, 46) = 1$

### Erweiterter Euklidischer Algorithmus

Es gibt neben dem Eukldischen Algorithmus noch eine Erweiterung, die zwei
Koeffizienten fuer die Argumente liefert, die den groessten gemeinsamen Teiler
bilden.

Satz: Seien $x,y \in \mathbb{N}$. Es gibt $a, b \in \mathbb{Z}$ sodass:

$ggT(x, y) = ax + by$

Diese $a, b$ muessen nicht eindeutig sein.

#### Beweis

Induktion ueber $\max\{x, y\}$

Basis: $\max\{x, y\} = 1$, dann $x = y = 1$ (natuerliche Zahlen) und $ggT(x,y) =
       1 = 1 \cdot x + 0 \cdot y$

Schritt: $\max{x,y} > 1$.

	Annahme: Die Aussage gilt fuer $ggT(x,y \mod x)$.

	Behauptung: Dann gilt die Aussage auch fuer $ggT(x,y)$.

Beweis:

	1. $y \mod x = 0$, dann $ggT(x,y) = 1 \cdot x + 0 \cdot y$.

	2. $y \mod x > 0$. Dann gilt $ggT(x, y) = ggT(y \mod x, x)$. Nach
       Induktionsannahme gibt es ein $a',b' \in \mathbb{Z}$ mit
	   $ggT(y \mod x, x) = a'(y\mod x) + b'x$.

	3. Da $y = y \mod x + \lfloor y/x \rfloor \cdot x$, gilt:

		$a'(y\mod x) + b'x = a'(y - \lfloor y/x \rfloor \cdot x) + b'x$
		$= a'y - a'lfloor y/x \rfloor \cdot x + b'x$
		$= (b' - lfloor y/x rfloor \cdot a')x + a'y$

	Wir haben also $a,b$ fuer $x,y$ gefunden.

Das fuehrt wiederum zu folgendem Algorithmus:

```python
def erwggT(x,y):
	if y % x == 0:
		return (1, 0)
	a_prime, b_prime = erwggT(y % x, x)
	return ((b_prime - a_prime * (y//x)), a_prime)
```

bzw. mit ggT:

```python
def ggT(x, y):
	if y % x == 0:
		return (x, 1, 0)
    g, a, b = ggT(y%x, x)
    return g, (b - (y//x) * a), a
```

#### Fuer Menschen

Als Mensch fuehrt man den Algorithmus am besten aus, indem man eine Tabelle
aufstellt. Die Spalten dieser Tabelle sind dann $x$, $y$, der Quotient $\lfloor
y/x \rfloor := q$, sowie die Koeffizienten $a$ und $b$:

|$x$|$y$|$q$|$a$|$b$|
|:--|:--|:--|:--|:--|

Dann seien $x, y$ konkrete Werte $\in \mathbb{N}$. Man geht dann so vor:

1. Schreibe $x$ und $y$ in die entsprechenden Spalten der ersten Zeile. Dann,
   fuehre den einfachen euklidschen Algorithmus aus, also:

   1. Trage in $q$ das ganzzahlige Ergebnis der Division von $y/x$ ein.
   2. Trage in der naechsten Zeile fuer $x$ den Rest von $y/x$, also $y \mod x$
      ein, und fuer $y$ den alten Wert von $x$.
   3. Wenn nun $x$ $y$ teilt, terminiere. Sonst gehe zu (1).

   Der groesste gemeinsame Teiler steht am Ende ganz unten in der Spalte von
   $x$.

2. Nun berechnet man die Koeffizienten. Dazu geht man so vor:

	1. Trage in der letzten Zeile fuer $a$ (Koeffizient von $x$) eine $1$ ein,
       fuer $b$ eine $0$. Dann:
	2. Uebertrage $a$ in die Spalte von $b$ in der oberen Zeile.
	3. Berechne das neue $a$ (der obere Zeile) als $b - q \cdot a$.
	4. Wenn man in der ersten Zeile angelangt ist, terminiere, ansonsten zurueck
       zu (2).

Somit hat man sowohl den ggT als auch die Koeffizienten gefunden. In Schritt 2
muss in jeder Zeile $(b - q \cdot a)x + ay = ggT$ gelten.

##### Beispiel

Seien $x = 78, y = 99$. Zuerst der einfache Algorithmus:

|$x$|$y$|$q$|$a$|$b$|
|:--|:--|:--|:--|:--|
|78 |99 | 1 |   |   | # 78 geht 1 mal in 99, 21 Rest
|21 |78 | 3 |   |   | # 21 geht 3 mal in 78, 15 Rest
|15 |21 | 1 |   |   | # 15 geht 1 mal in 21,  6 Rest
| 6 |15 | 2 |   |   | #  6 geht 2 mal in 15,  3 Rest
|(3)|6  | 2 |   |   | #  3 geht 2 mal in  6,  0 Rest, fertig
| 0 |3  | - |   |   |

Der ggT ist also $3$. Nun trage fuer $3$ eine $1$ ein und fuer $6$ eine $0$, da
$3 = 3 \cdot 1 + 0 \cdot 6$.

|$x$|$y$|$q$|$a$|$b$|
|:--|:--|:--|:--|:--|
|78 |99 | 1 |   |   |
|21 |78 | 3 |   |   |
|15 |21 | 1 |   |   |
| 6 |15 | 2 |   |   |
| 3 |6  | 2 | 1 | 0 |
| 0 |3  | - | 0 | 1 |

Nun zu Schritt 2 (von unten nach oben):

|$x$|$y$|$q$|$a$ |$b$  |
|:--|:--|:--|:---|:----|
|78 |99 | 1 |(14)|(-11)|
|21 |78 | 3 |-11 | 3   | -11 nach b, a = 3 - 1 * -11
|15 |21 | 1 | 3  |-2   | 3 nach b, a = -2 - 3 * 3
| 6 |15 | 2 |-2  | 1   | -2 nach b, a = 1 - 1 * -2
|(3)|6  | 2 | 1  | 0   | 1 nach b, a = 0 - 2 * 1

Also hat man nun den ggT mit $3$ gefunden, und die Koeffizienten fuer $x$ und
$y$, sodass:

$ggT(78, 99) = 14 \cdot 78 + (-11) \cdot 99 = 3$

## Permutationen

Eine Permutation $\pi$ ist eine *bijektive* Abbildung $A \rightarrow A$ von
einer Menge $A$ auf sich selbst. Bildet $\pi$ jedes Element auf sich selbst ab,
nennt man die Permutation *Identitaetsabbildung* $id_A$. Auch gilt, da die
Abbildung bijektiv ist, dass jede Permutation auch eine Umkehrfunktion
$\pi^{-1}$ besitzt, wo gilt:

$\forall x, y \in A: \pi(x) = y \Leftrightarrow \pi^{-1}(y) = x$

Sei also z.B. die Bijektion $\pi$ in Zyklenschreibweise $(1,2,3)(4,5)(6)$
gegeben. Hier wird $1 \mapsto 2, 2 \mapsto 3, 3 \mapsto 1, 4 \mapsto 5, ...$
abgebildet. Die Umkehrfunktion bildet dann einfach jedes Element (bzw. an jedem
Index) wieder zurueck auf das vorherige. Fuer die Zyklenschreibweise spiegelt
man dafuer die Zyklen. Zyklen mit ein oder zwei Elementen bleiben gleich. Somit:

$\pi^{-1}: (3,2,1)(5,4)(6)$, bzw. nach Sortierung: $(1,3,2)(4,5)(6)$.

Da eine Permutation also eine Funktion ist, kann man auch die Komposition
$\circ$ von Permutationen $\pi_1,\pi_2$ bilden:

$\pi_1(x) \circ \pi_2(x) = \pi_1(\pi_2(x))$

Man beachte hier aber vor Allem die Evaluierungsreihenfolge: ein Element muss
zuerst in der rechten Permutation ausgewertet werden, und erst danach in der
linken, nicht umgekehrt. In Zyklenschreibweise beispielsweise folgende
Komposition:

$(6,3,4)(2,5,1) \circ (6,2,4,3,1,5) = (3,2,6,5)(4)(1)$

Man geht hierfuer einfach fuer jedes Element in der rechten Permutation einen
transitiven Pfad der Laenge $2$. Z.B. $6 \mapsto 2$ und in der linken
Permutation $2 \mapsto 5$, also in der neuen Permutation $6 \mapsto 5$. Und dann
macht man fuer diese Abbildung einen neuen Zyklus. Und wenn ein Element an
diesen Zyklus "ankettet", schreibt man ihn noch in die selben Klammern. Sonst
macht man einen neuen Zyklus (neue Klammern) auf. Ebenso ist es strategisch
geschickt, immer fuer das Bild des letzten Elements das neue Bild zu suchen
(also wenn $a \mapsto b$, suche als naechstes das Bild von $b$).

## Gotchas

* Permutation = Bijektion
* Um das multiplikative Inverse von $a$ mit dem EEA zu berechnen, muss $ggT(a,n)
  = 1$ sein!
