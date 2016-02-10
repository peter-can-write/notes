# Bool'sche Algebra

Fast alle Rechner benutzten heutzutage ein Darstellungssystem auf Basis von
binaeren Zeichen anstelle einer Darstellung als Funktion der Zeit oder des
Raums. Dies hat mehrere Vorteile:

* Keine prinzipiellen Genauigkeitsschranken.
* Sichere Speicherung (diskrete Werte gehen weniger leicht verloren).
* Billige Uebertragung und Verknuepfung.
* Einfache Arithmetik.

Die Bool'sche Algebra eignet sich gut zum Ausdruck von digitalen Schaltungen.

Allgemein ist die Bool'sche Algebra $\langle S, \neg, \land, \lor \rangle$
definiert als eine Algebra mit einer bestimmten Traegermenge $S$ und den
Operatoren:

* $\neg$: Negation
* $\land$: Konjunktion
* $\lor$: Disjunktion

Dann enthaelt die Bool'sche Algebra noch einige Axiome (siehe unten).

Fuer digitale Schaltungen ist die Traegermenge $S$ der Bool'schen Algebra nun
definiert als $\{0, 1\} := \{false, true\} := \{LOW,
HIGH\}$. Diese bestimmte Auspraegung der Bool'schen Algebra nennt man
"Schaltalgebra".

## Schreibweisen

Bei der Schaltalgebra verwendet man eine andere Notation fuer bestimmte
Operatoren bzw. Ausdruecke.

1. $A \land B$ wird als $A \cdot B$ geschrieben und verkuerzt zu $AB$.
2. $A \lor B$ wird als $A + B$ geschrieben.
3. $\neg A$ wird als $\overline(A)$ geschrieben.

## Axiome

Sei $\circ$ stellvertretend fuer alle Operatoren $\in \{\lor, \land\}$. Sei
dann $\odot$ der jeweils andere Operator $\in \{\lor, \land\}$.

1.  Involution: $\overline{\overline{F}} = F$
2.  Kommutativitaet: $A \circ B = B \circ A$
3.  Assoziativiatet: $(A \circ B) \circ C = A \circ (B \circ C)$
4.  Idempotenz: $A \circ A = A$
5.  Absorption: $A \circ (B \odot A) = A$
6.  Distributivitaet: $A \circ (B \odot C) = (A \circ B) \odot (A \circ C)$
7.  DeMorgan: $\overline{(A \circ B)} = \overline{A} \odot \overline{B}$
8.  Identitaet: $A \land true = A, A \lor false = A$
9.  Triviale Tautologie: $A \lor \overline{A} = true$
10. Trivialer Widerspruch: $A \land \overline{A} = false$
11. Neutralitaet: $A \lor \text{Trivialer Widerspruch}, A \land \text{Triviale
    Tautologie}$

Hat keinen Namen aber:

12. $A \circ (B \odot \overline{A}) = A \circ B$

Denn:
* $A \land (B \lor \overline{A}) = (A \land B) \lor (A \land \overline{A}) = (A \land B) \lor false = A \land B$
* $A \lor (B \land \overline{A}) = (A \lor B) \land (A \lor \overline{A}) = (A \lor B) \land true = A \lor B$

### Dualitaet

Bemerkenswert an den Regeln der booleschen Algebra ist eine eigenartige
Symmetrie, die als Dualitaet bezeichnet wird. Es gilt naemlich stets, dass man
die Symbole $(0, 1, \land, \lor)$ als Ganzes gegen die Symbole $(1, 0, \lor,
\land)$ austauschen kann und dennoch die Gueltigkeit aller Gesetze erhalten
bleibt! Dies ist mit dem deMorgan-Gesetz nachpruefbar.

Deswegen kann man $0$ als auch $HIGH$ definieren und $1$ als $LOW$, und dennoch
die selben Regeln anwenden. Das nennt man dann *negative Logik* (wenn die
Signale also invertiert sind).

## Schaltfunktion

Eine Funktion $\{0, 1\}^n \rightarrow \{0, 1\}$ heisst $n$-stellige
Schaltfunktion. Sie erhaelt als Argument $n$ Bits und liefert als Schaltfunktion
einen einzigen Bit.

Beispiele:

* Einstellige Schaltfunktion: $0 \mapsto 1$
* Zweistellige Schaltfunkionen: $10 \mapsto 0$
* Dreistellige Schaltfunkionen: $001 \mapsto 1$

Sei $f: \{0, 1\}^n \rightarrow \{0, 1\}$ eine Schaltfunktion mit $n$
Argumenten. Dann gibt es fuer jedes $n$ genau $2^n$ verschiedene
Binaer-$n$-Tupel (Bit-Vektoren/Strings), und jede dieser Binaer-$n$-Tupel kann
auf $0$ oder auf $1$ abgebildet werden ($2$ Moeglichkeiten). Daher gibt es
$2^{2^n}$ verschiedene Schaltfunktionen mit $n$ Eingaengen.

### Einsmenge und Nullmenge

Die *Einsmenge* $f^{-1}(1)$ einer $n$-stelligen Schaltfunktion $f$ ist die Menge
aller binaeren $n$-Tupel (Eingangswoerter) fuer die die Schaltfunktion den
Funktionswert $1$ ausgibt.

Beispiele:

* Die Einsmenge zur Konjunktion ist: $\{(1, 1)\}$
* Die Einsmenge zur Disjunktion ist: $\{(0, 1), (1, 0), (1, 1)\}$

## Schaltnetz

Ein Schaltnetz ist eine *Funktionseinheit*, also ein nach Aufgabe
oder Wirkung abgrenzbares Gebilde, die eine oder mehrere Schaltfunktionen (ein
*Buendel* von Schaltfunkionen) realisiert.

Es muss nicht nur einen Ausgang haben, sondern kann $m$ Ausgaenge haben, von
denen jeder Ausgang also eine separate Schaltfunktion ist. Somit ist es
allgemein eine Funktion $\{0, 1\}^n \rightarrow \{0, 1\}^m \text{ mit } m, n \in
\mathbb{N}$.

### Gatter

Hat ein Schaltnetz nur einen Ausgang ($m = 1$), nennt man es "Gatter" (das
Schaltnetz realisiert also nur eine Schaltfunktion fuer den einen Ausgang).

Beispiele:

* Konjunktionsgatter $f: \{0, 1\}^n \mapsto \bigwedge_{i \in \{0, 1\}^n}
  i$. Sie werden in einer Schaltung mit einem Ampersand $\&$ dargestellt.

* Disjunktionsgatter: $f: \{0, 1\}^n \mapsto \bigvee_{i \in \{0, 1\}^n} i$. Sie
  werden in einer Schaltung als $\geq 1$ dargestellt.

* Negationsgatter: $f: x \mapsto \neg x, x \in \{0, 1\}$. Sie hat nur einen
  Eingang und nur einen Ausgang. In einer Schaltung wird dieses Gatter als leere
  Kiste mit einem Kreis am Ausgang dargestellt.

* XOR-Gatter: $f: x \mapsto \oplus_{i \in \{0,1\}^n} i$. Sie werden in einer Schaltung als $= 1$ dargestellt.

## Darstellungen

Jede Schaltfunktion ist darstellbar als:

* Schaltalgebraischer Ausdruck $A + B + (C \cdot D) + ...$
* Funktionstafel (Wertetabelle)
* Karnaugh-Veitch-Diagramm

### Schaltalgebraische Ausdruecke

Jede $n$-stellige Bool'sche Funktion ist eindeutig als Disjunktion von
Konjunktionen (DNF), oder als Konjunktion von Disjunktionen (KNF) darstellbar.

#### Disjunktive Normalform (DNF)

Die Disjunktive Normalform einer Bool'schen Funktion ist die Darstellungsweise
der Funktion, die nur Disjunktionen ($\lor$) von Konjunktionen ($\land$)
enthaelt.

Anders definiert ist sie eine Disjunktion von *Mintermen*. Ein Minterm ist
hierbei die Konjunktion __aller $n$__ Argumente (Eingaenge) der
Schaltfunktion. Diese Bool'schen Variablen koennen hierbei negiert sein, oder
nicht. Ein Beispiel fuer einen Minterm ist $\overline{a}bc\overline{d}$. Jeder
Minterm liefert fuer genau eine bestimmte Belegung der $n$ Argumente den Wert
$1$ bzw. $true$.

Eselsbruecke: Eine Konjunktion wird sehr schnell $0$, also minimal $\rightarrow$
Minterm.

Konkret waehlt man fuer die DNF einer Schaltfunktion immer die Einsmenge, also
man verknuepft alle jene Minterme (Eingangsworte), die den Wert $true$ bzw. $1$
liefern:

$DNF(f) = \bigvee_{min \in f^{-1}(1)} min$

Somit kann man sagen, dass die Funktion $f$ den Funktionswert $true$ ausgibt,
wenn das Eingangswort "so ist, oder so ist, oder so ist, oder so ist, ..."

Gegeben eine Funktionstafel (Wertetabelle) bildet man die DNF also dadurch, dass
man sich alle Eingangswoerter raussucht, die den Wert $true$ bzw. $1$ liefern.
Dann verknuepft man die einzelnen Eingangsbits mit Konjunktionen zu einem
Minterm, und verknuepft dann alle Minterme mit Disjunktionen.

#### Konjunktive Normalform (KNF)

Gegensaetzlich zur DNF ist die KNF jene Darstellung einer Bool'schen Funktion,
die nur Konjunktionen ($\land$) von Disjunktionen ($\lor$) enthaelt.

Ein *Maxterm* ist nun die Disjunktion __aller $n$__ Argumente (Eingaenge) zur
Schaltfunktion. Wieder koennen die Bool'schen Variablen negiert sein, oder
nicht. War $\overline{a}bc\overline{d}$ vorher ein Minterm, so ist nun also $a +
\overline{b} + \overline{c} + d$ ein Beispiel fuer einen Maxterm. Jeder Maxterm
liefet fuer genau eine Belegung den Funktionswert $0$ bzw. $false$.

Eselsbruecke: Eine Disjunktion mit $+$ dargestellt wird sehr schell zu einer
"grossen Zahl" $\rightarrow Maxterm$.

Die Grundidee der KNF ist es zu sagen, dass eine Bool'sche Funktion genau dann
den Wert $true$ liefert, wenn das konkrete Eingangswort keines der
Eingangswoerter ist, die den Wert $false$ liefern. Man nehme beispielsweise die
zweistellige Bool'sche Funktion $\oplus$ (XOR). Diese liefert genau dann den
Funktionswert $false$, wenn beide Eingaenge gleich sind ($00 \lor 11$). Also
liefert diese Funktion fuer ein Eingangswort $xy$ genau dann den Wert $true$,
wenn gilt $\overline{(\overline{x} \land \overline{y})} \land \overline{(x \land
y)}$, also wenn es nicht $00$ ist und nicht $11$. Mittels DeMorgan wird nun jeder
negierte Minterm zu einem Maxterm. Genau dadurch entsteht die KNF.

Intuitiv bzw. konzeptuell vermittelt die KNF also fuer eine Funktion, dass sie
$true$ ausgibt, wenn "nicht das gilt, und nicht das, und nicht das, ...". Die
Negationen fuehren dann eben zu Maxtermen.

Formal ist die KNF nun also definiert als:

$KNF(f): \bigwedge_{min \in f^{-1}(0)} \overline{min}$

Also die Konjunktion aller negierten Minterme aus der Nullmenge. Wobei dann $\overline{min} = max$.

#### Umwandlungen zwischen KNF und DNF

Es ist bewiesen, dass es fuer jede KNF eine aqeuivalente DNF und umgekehrt
gibt. Eine Moeglichkeit, von der einen Darstellung zur anderen zu kommen ist die
Distributivitaet:

DNF: $ab + cd = (ab + c) (ab + d) = ((a + c) (b + c)) ((a + d) (b + d)) = (a +
c) (b + c) (a + d) (b + d)$

KNF: $(a + c) (b + c) (a + d) (b + d) = (a + cd) (b + cd) = ab + cd$

Schon bei drei Variablen kann eine Wahrheitstabelle oder ein KV-Diagramm aber schon schneller sein. Vor Allem, da man bei einer DNF schon die Zeilen der Wahrheitstabelle aufgelistet hat, die $1$ ergeben. Bei der KNF muss man die Maxterme zuerst negieren, und die entstandenen Minterme sind dann die Nullmenge der Formel.

#### Umwandlung zu NAND/NOR Form

NOR/NAND sind oft guenstiger herzustellen als andere Bausteine. Daher kann es
Sinn machen, eine DNF/KNF in NOR/NAND Form zu bringen. Eine DNF kann durch
doppelte Negation in eine NAND Form gebracht werden und eine KNF durch doppelte
Negation in NOR Form.

$DNF \Rightarrow NAND: ab + cd \equiv \overline{\overline{ab + cd}}$

Wegen Involution ist diese Umformung korrekt. Jetzt zieht man die innere
Negation einfach mit deMorgan ueber alle Terme und erhaelt so eine NAND Form!

$\overline{\overline{ab} \cdot \overline{cd}})$

Hier ist jede einzelne innere Konjunktion ein NAND-Gatter und die auessere
Konjunktion ist auch ein NAND-Gatter.

$KNF \Rightarrow NOR: (a+b) \cdot (c+d) \equiv \overline{\overline{(a+b) \cdot
(c+d)}} \equiv \overline{\overline{a+b} + \overline{c+d}}$

Hier hat man nun also eine KNF in eine Form mit drei NOR Gattern umgewandelt.

#### Kanonische Formen

Man nennt eine KNF/DNF "kanonisch", wenn in jedem Minterm (DNF) bzw. in jedem
Maxterm (KNF) jede einzelne Variable vorkommt.

Beispiel einer kanonischen DNF fuer die Variablen $a, b, c$:
$abc + \overline{a}bc + a\overline{b}\overline{c}$

Beispiel einer nicht-kanonischen KNF fuer die Variablen $a, b, c$:
$(a + b) \cdot c$

## Minimierung

Eine KNF/DNF in ihrer kanonischen Auspraegung ist nicht immer die minimale. Es
kann sein, dass man unter bestimmten Regeln bzw. Axiomen Vereinfachungen
durchfuehren kann, die die Anzahl an Termen der Formel reduziert. Dies hat
wiederum zur Folge, dass weniger Gatter zur Implementierung der Schaltfunktion
gebraucht werden. Man moechte also:

* Weniger Schaltglieder ($\land, \lor, \neg$)
* Weniger Eingaenge und Leitungen
* Weniger Stufen (Gatter, die auf die Ergebnisse anderer Gatter warten muessen)
* Kuerzere Leitungen
* Billigere Schaltglieder (z.B. NOR statt NAND)

Der Grundgedanke der Minimierung ist dass Bool'sche Formeln der Form $xyz +
xy\overline{z}$ durch Distributivitaet und trivialer Tautologie
bzw. Neutralitaet umgeformt werden koennen:

$xyz + xy\overline{z} = xy \cdot (z + \overline{z}) = xy \cdot (true) = xy$

Hier haette man z.B. eine Schaltung mit zwei Stufen und drei Gattern (zwei
Konjunktionen geschalten in eine Disjunktion) in eine Schaltung mit nur einer
einzigen Stufe und nur einem Gatter, einer Konjunktion, umgeformt.

Diese Minimierungstechnik auf Basis der Distributivitaet und Neutralitaet nennt
man "Verschmelzung".

Bemerkung: Algebraische Vereinfachungen koennen aequivalente Schaltungen zu
Schaltungen in kanonischer DNF ergeben, aber einfacher, heisst oft langsamer,
wenn man mehr Stufen hat. Beispiel: $abcd + abef$. Hier braucht man drei Gatter
in zwei Stufen. Hebt man nun $ab$ heraus, erhaelt man mit $ab \cdot (cd + ef)$
zwar einen einfachereren algebraischen Ausdruck, aber Nun braucht man fuenf
Gatter und drei Stufen. Mehr Stufen bedeuten, dass mehr Gatter auf die Ergebnisse anderer Gatter warten muessen. Es geht also das Element der Parallelitaet verloren.

### Definitionen

#### Monom

Ein (konjunktives) $k$-stelliges Monom ist die Konjunktion aus $k$ Variablen
einer $n$-stelligen Funktion. Also ist ein Minterm ein $n$-stelliges Monom. Ein
Monom muss aber eben nicht alle $n$ Variablen (Eingaenge) enthalten.

#### Polynom

(Disjunktives) Polynom: Die Disjunktion von (konjunktiven) Monomen.

#### Benachbart

Allgemein heissen Binaer-Tupel gleicher Kardinalitaet benachbart, wenn sie sich
nur in einem Bit unterschieden. Man sagt auch, dass ihr *Hamming-Abstand* gleich
$1$ ist. Der Hamming-Abstand zaehlt dabei die Anzahl sich unterscheidender Bits
zwischen zwei Zahlen (maximal $n$).

Nun wird fuer die Bool'sche Algebra der Begriff "benachbart" auch auf Monome
ausgedehnt. Zwei Monome sind also benachbart, wenn sie sich nur dadurch
unterscheiden, dass die Negation einer bestimmten, konkreten Variable zwischen
den beiden Monomen anders ist.

Beispiel: $x_1x_2$ ist benachbart zu $x_1\overline{x_2}$

#### Verschmelzungsmenge

Die Verschmelzungsmenge ist nun die Menge aller $n$-stelligen Minterme, die sich
durch Distributivitaet und Neutralitaet ($abc + ab\overline{c} = ab$) zu einem
$k$-stelligen Monom umformen laesst, wo $k < n$.

Es ist anzumerken, dass die Verschmelzungsmenge nur die Menge der Minterme ist,
und nicht das Resultat der Minimierung! Fuer $abc + ab\overline{c}$ waere also
die Verschmelzungsmenge $\{abc, ab\overline{c}\}$, nicht $ab$.

Allgemein gilt: $2^k$ $n$-stellige *verschiedene* Monome sind genau dann zu
einem $n-k$-stelligen Monom verschmelzbar, wenn sie sich in $k$ Stellen
unterscheiden.

Besser ausgedrueckt: findet man $2^k$ *verschiedene* Monome die sich in $n -
k$ Stellen uebereinstimmen, dann muss es fuer die $k$ anderen Stellen alle
Moeglichkeiten geben, und die Variablen koennen eliminiert werden.

Beispiel: $k = 2, n = 3$

$a(bc), a(b\overline{c}), a(\overline{b}c), a(\overline{b}\overline{c})$

Diese $2^2$ Monome der Variablen $a, b, c$ unterscheiden sich in genau
$k$-Stellen. Da sie alle verschieden sind und es $2^k$ davon gibt, muss jede
Moeglichkeit fuer diese Variablen einmal vorkommen. Somit haengt der
Funktionswert in jedem Fall von den anderen $n - k = 3 - 2 = 1$ Variablen ab,
hier naemlich genau $a$.

#### Primimplikant

Ein Primimplikant ist ein minimales Monom, das sich nicht weiter nach oben
genannten Regeln minimieren laesst.

#### Karnaugh-Veitch-Diagramme

Karnaugh-Veitch (KV) Diagramme sind eine praktische Darstellung einer
Schaltfunkion. Hierbei werden fuer $n$ Variablen in $2^n$ Feldern alle
moeglichen Minterme bzw. Eingangswoerter dargestellt. In jedem einzelnen
Feld steht dann eine $1$ oder eine $0$ als Resultat der Funktion fuer das
jeweilige Eingangswort/Minterm.

Fuer $n = 2$ ($2$ Variablen):

```
     b     !b
a  [ ab  | a!b  ]
!a [ !ab | !a!b ]
```

Wie man sieht steht ueber den Spalten bzw. Zeilen die jeweilige Variable, und in
den Feldern ("Schnittpunkten") die Konjunktionen mit den anderen Variablen.

Eine interessante bzw. wichtige Eigenschaft von KV-Diagrammen ist, dass sie die
Nachbarschaft erhalten. Benachbarte Felder beinhalten immer benachbarte
Minterme. Wichtig ist, dass diese Benachbarung auch ueber die Grenzen "wrapped",
also sozusagen hinter dem Diagramm ueberlauft. So ist ein ganz linkes Feld einer
Zeile immer mit dem ganz Rechten benachbart und ein ganz oberes einer Spalte
immer mit dem ganz unteren.

Moechte man nun von einem KV-Diagramm fuer $n$ Variablen auf eines fuer $n + 1$
Variablen kommen, so muss man das ganze Diagramm einfach nach unten oder nach
rechts spiegeln. Wichtig ist, dass alle moeglichen $2^n$ Schnitte ein eigenes
Feld haben und das Felder weiterhin benachbart bleiben.

Fuer $n = 3$

```
      b      !b     b!  b
a  [ abc  | a!bc  | a!b!c  | a!b!c  ]
!a [ !abc | !a!bc | !a!b!c | !ab!c  ]
      c       c       c!       c!
	              ^
	              Spiegelungsachse von 2 auf 3
```

Fuer $n = 4$:

```
      b      !b     b!  b
a  [ abcd   | a!bcd   | a!b!cd   | a!b!cd   ] d
!a [ !abcd  | !a!bcd  | !a!b!cd  | !ab!cd   ] d
!a [ !abc!d | !a!bc!d | !a!b!c!d | !ab!c!d  ] !d
a  [ abc!d  | a!bc!d  | a!b!cd   | ab!c!d   ] !d
      c       c       c!       c!
```

Damit die verschiedenen Schnittmoeglichkeiten erhalten bleiben, sollte auf
gegenueberliegenden Seiten die ein Seite das Schema $xxx...!x!x!x...$ verfolgen
und die andere Seite das Schema $yyy...!y!y!y...!y!y!y...yyy$.

#### Minimierung via KV-Diagrammen

Ein KV-Diagramm enthaelt in den Feldern Minterme, also Konjunktionen. Das ganze
KV-Diagramm stellt dabei eine Disjunktion dieser Minterme dar. Eine DNF laesst
sich also bilden, indem man nur Minterme waehlt, die eine $1$ als Funktionswert
haben.

Hierbei bietet es sich an, groessere benachbarte Gebiete, die also fuer eine
Variable mehr als nur einen Wahrheitswert belegen, zu verschmelzen. Solch ein
Gebiet (dass auch ueber die Grenzen "wrapped") ist dann die Verschmelzungsmenge,
und wenn man $2^k$ $n$-stellige Minterme (Felder) in einer Verschmelzungsmenge
findet, die sich in $n - k$ nicht unterscheiden, kann man sie nach obiger Regel
zu einem $n - k$-stelligen Monom minimieren.

Findet man also z.B. zwei $3$-stellige Minterme (Felder) wo eine Variable einmal
negiert und einmal nicht negiert vorkommt, kann man daraus ein Monom mit den $2$
konstanten Variablen machen.

Hierbei gilt es die Groesse dieser Verschmelzungmengen zu maximieren, da
groessere Verschmelzungsmengen mehr Moeglichkeiten von Variablen abdecken. Eine
Verschmelzungsmenge sollte jedoch __als Groesse immer eine Zweierpotenz__
haben. Wenn nicht, vereinfacht sich der Ausdruck auf einen solchen.

Man kann auch sagen, man sucht die minimale Anzahl an Variablen, die die
Verschmelzungsmenge eindeutig innerhalb des Diagramms lokalisieren, quasi wie
Koordinaten.

Auch ist zu beachten, dass Felder nicht nur einmal verwendet werden
muessen/koenen. Hat man z.B. drei Felder $A, B, C$, die $1$ sind, so kann man
zuerst aus $AB$ eine Konjunktion machen und dann aus $BC$. Hauptsache ist, die
Felder sind moeglichst gross (= weniger Variablen).

##### Beispiel

````
     c  !c   !c  c
!d [ 1 | 1 | 1 | 1 ]  a
 d [ 1 | 1 | 0 | 0 ]  a
 d [ 1 | 0 | 1 | 1 ] !a
!d [ 0 | 0 | 1 | 1 ] !a
     b   b  !b   !b
````

Hier sei nun zuerst angemerkt, dass eine aus einem KV-Diagramm resultierende
DNF nicht eindeutig ist. Sie haengt einfach davon ab, wie man die Felder
waehlt.

Als erstes koennte man hier die beiden Quadrate $[(abcd) (ab!c!d)]$ und
$[(!a!b!d!c) (!a!bcd)]$ verschmelzen. Dann bleiben die Felder $(a!b!c!d),
(a!bc!d), (!abcd)$. Hierbei kann man fuer die ersten zwei die ganze erste Zeile
verschmelzen. Man sieht also, man kann Felder wieder verwenden und man sollte
immer die groesstmoegliche Verschmelzung waehlen (__dessen Groesse eine
Zweierpotenz ist__). Fuer das letzte Feld kann man die mittleren Felder der ganz
linke Spalte nehmen.

Nun also:

1. Quadrat: Es enthaelt $2^2 = 4$ Variablen $a,b,c,d$, von denen sich $2$ nicht
   unterscheiden. Also muss es fuer die anderen zwei Variablen alle $2^2$
   Moeglichkeiten geben, und man kann sie eliminieren. Wir erhalten also einen
   $4 - 2 = 2$-stelligen Monom $ab$.

2. Quadrat: Nach selber Logik erhaelt man hier $\overline{a}\overline{b}$.

1. Zeile: Ganz $a$ und ganz $!d$, also: $a!d$

Linke Spalte ohne unteres Feld: $b$ geschnitten $c$, geschnitten $d$: $bcd$

Nun kann man diese einzelnen Primimplikanten in einer minmalen DNF durch
Disjunktionen vereinen:

$ab + \overline{a}\overline{b} + a\overline{d} + bcd$

## Gotchas

* Wenn nach der DNF/KNF gefragt ist, ist die kanonische gemeint! Nicht
  algebraisch vereinfachen wenn nicht gefragt!!
* Beim algebraischen Vereinfachen von DNF kann es auch hilfreich sein, einen
  Term zu verdoppeln, damit man ihn mit einem anderen verschmelzen kann.
* Zuerst sehen, welche Variable am oeftesten vorkommt, nicht einfach die erste
  nehmen, die oft vorkomm!
