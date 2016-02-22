# Aussagenlogik

Die *Logik* ist die Wissenschaft des *Schliessens*. Sie untersucht, welche
*Inferenzen* korrekt sind. Eine Inferenz ist dabei eine Aussage der Form: Wenn
$A$ wahr ist, dann ist auch $B$ wahr.

Die *Aussagenlogik* (propositional logic) besteht dabei aus einer vorgegebenen
Menge von *atomaren Aussagen* oder Aussagenvariablen, die mit Hilfe der
Operatoren (auch: *Konnektoren*, *Junktoren*) "und", "oder", "nicht" und
"wenn... dann" verbunden werden.

Die Aussagenlogik bildet dabei eine *formale Sprache*, welche definiert ist
durch eine *Syntax* und eine *Semantik*:
* Die *Syntax* der formalen Sprache legt die Regeln fest, welche Zeichenketten
  wohlgeformte Ausdruecke sind.
* Die *Semantik* legt die Bedeutung der Formeln bzw. Zeichenketten fest. Eine
  formale Semantik ordnet jeden Ausdruck einem mathematischen Objekt zu, welches
  als Bedeutung des Ausdrucks interpretiert wird. So kann die gesamte Formel
  eine Bedeutung erhalten.

Alle aussagenlogischen Formeln sind endlich!

### Syntax

Eine *formale Syntax* besteht aus einem *Vokabular* und einer Menge von
*Formationsregeln*. Das Vokabular legt fest, welche Zeichen in Ausdruecken
vorkommen duerfen. Die Formationsregeln legen fest, welche Anordnung des
Vokabulars zu wohlgeformten Ausdruecken fuehren.

#### Vokabular

Das Vokabular der Aussagenlogik umfasst:

* Wahrheitskonstanten (atomare Aussagen): $true, false$
* Aussagenvariablen (Platzhalter): $p, q, r, s, t, ...$
* Logische Operatoren (Junktoren): $\neg, \land \lor, \rightarrow$
* Hilfssymbole (Klammern): $(, )$

#### Formationsregeln

Die Formationsregeln der Aussagenlogik umfassen:

1. Regel: $true$ und $false$ sind Formeln.
2. Regel: Eine Aussagenvariable (z.B. $p$) ist eine Formel.
3. Regel: Ist $F$ eine Formel, dann ist auch $\neg F$ eine Formel.
4. Regel: Sind $F$ und $G$ Formeln, dann sind:
		  * $(F \land G)$
		  * $(F \lor  G)$
		  * $(F \rightarrow G)$
		  ebenfalls Formeln.
5. Regel: Eie Ausdruck ist nur dann wohlgeformt und eine Formel, wenn er sich
   aus den obigen Regeln zusammensetzt.

#### Bindungsregeln

Hierbei gibt es noch Bindungsregeln, die die Praezedenz von Operatoren bestimmt:
* $\neg$ bindet staerker als $\land$.
* $\land$ bindet staerker als $\lor$.
* $\lor$ bindet staerker als $\rightarrow$.

Sei $<$ definiert als eine Relation $\subseteq \{\neg, \land, \lor,
\rightarrow\}^2$ mit der Bedeutung "bindet schwaecher", dann:

$\rightarrow < \lor < \land < \neg$

Daher z.B.: $\neg p \land q = (\neg p \land q) \neq \neg(p \land q)$.

## Belegung

Eine *Belegung* $\beta$ ist eine Funktion von einer Menge von Aussagenvariablen
$V$ in die Menge der Boole'schen Werte $\{0, 1\}$. Eine Belegung ordnet jeder
Aussagenvariable einer Formel also einen atomaren Wahrheitswert $true$ oder
$false$ zu.

Sei $F$ eine Formel mit bestimmten Aussagenvariablen (z.B. $p, q, r$), dann
sagen wir, dass eine Belegung $\beta: V \rightarrow \{0, 1\}$ zur Formel $F$
*passt*, wenn alle Variablen, die in $F$ vorkommen, auch in $V$ enthalten sind.

Enthaelt $V$ nur die Aussagenvariablen von $F$, und keine weiteren Elemente,
dann ist die Belegung bezueglich $F$ *minimal*.

Beispiel: Sei $F = \neg p \land q$ eine Formel mit der Menge von
Aussagenvariablen $\{p, q\}$. Dann ist $\beta_1: \{p, q, r\} \rightarrow \{0,
1\}$ eine passende Belegung fuer $F$. $\beta_2: \{p, q\} \rightarrow \{0, 1\}$
waere eine *minimale*. $\beta_3: \{x\} \rightarrow \{0, 1\}$ ware weder passend
noch minimal.

### Projektion

Sei $\beta$ eine Belegung, die zu einer Formel $F$ passt, aber nicht minimal
ist. Dann ist $\beta_F$ die *Projektion* von $\beta$ auf $V_F$ (die Menge der
Variablen von $F$), die minimal ist. Dann gilt auch $[F](\beta) \equiv
[F](\beta_F)$.

Beispiel: $F = a \land b, V_F = \{a, b\}, \beta: \{a, b, x, 1\} \rightarrow \{0,
1\}$. Hier ist $\beta$ nicht minimal. Dann ist die Projektion $\beta$ auf $V_F$
minimal: $\beta_F: \{a, b\} \rightarrow \{0, 1\}$.

## Semantik

Die *Semantik* einer Formel $F$ ist eine Funktion $[F]$, die jeder Belegung
$\beta$, die zu $F$ __passt__, einen Wahrheitswert $[F](\beta) \in \{0, 1\}$
zuordnet.

Sei $F$ beispielsweise die Formel $p \lor q$. Dann wuerde eine *passende*
Belegung $\beta: \{p, q, x\} \rightarrow \{0, 1\}$ zum Beispiel die Abbildung:

* $p \rightarrow 0$
* $q \rightarrow 1$
* $x \rightarrow 0$

darstellen, wobei $x$ fuer $F$ irrelevant ist. Dann wuerde die Formel unter
dieser Belegung so aussehen: $0 \lor 1$. Die Semantik $[F]$ ordnet nun dieser
sowie jeder anderen zu $F$ passenden Belegung einen Wahrheitswert $\in \{0, 1\}$
zu, der besagt, ob die Formel unter der Belegung und den Regeln der
Aussagenlogik zu $false$ oder zu $true$ evaluiert.

## Wahrheitstabelle

In einer Wahrheitstabelle wird fuer eine Formel $F$ die Semantik
visualisiert. Dabei ist in jeder Zeile in den linken Spalten die jeweilige
konkrete Belegung gegeben, und in der rechten Spalte der Wahrheitswert, auf
welchen die Semantik diese Formel unter der jeweiligen Belegung abbildet. Dabei
wird immer nur eine minimale Belegung betrachtet. Meistens wird dabei jede moegliche
Belegung ueberprueft, wobei es fuer $n$ Aussagenvariablen $2^n$ Zeilen
(moegliche Belegungen) geben muss.

Manchmal ist in der Mitte zusaetzlich noch die Formel mit den eingesetzten
Bildern der Aussagenvariablen angezeigt, und ueber jedem Operator das Resultat
der Operation ($\in \{0, 1\}$). Dann ist der Wahrheitswert ueber dem auessersten
Operator genau $[F](\beta)$.

Beispiel: $F = p \land \neg q$

|$\beta$| $[F](\beta)$ |
|:------|              |
| p | q |              |
|:--|:--|:-------------|
| 0 | 0 | 0            |
| 0 | 1 | 0            |
| 1 | 0 | 1            |
| 1 | 1 | 0            |

Mit Formel:

|$\beta$|        $F$       | $[F](\beta)$ |
|:------|:-----------------|:-------------|
| p | q | $p \land \neg q$ |              |
|:--|:--|:-----------------|              |
| 0 | 0 |  0    0    1  0  |      0       |
| 0 | 1 |  0    0    0  1  |      0       |
| 1 | 0 |  1    1    1  0  |      1       |
| 1 | 1 |  1    0    0  1  |      0       |
                ^
			 Ergebnis

## Operatoren

Hier gegeben die Namen der Junktoren/Konnektoren/Operatoren der Aussagenlogik:

* $\land$: "und" bzw. *Konjunktion*.
* $\lor$: "oder" bzw. *Disjunktion*.
* $\neg$: "nicht" bzw. *Negation*.
* $\rightarrow$: "impliziert" bzw. *Implikation*.
* $\oplus$: "exklusives oder" bzw. XOR.
* $\leftrightarrow$: "genau dann, wenn" bzw. *Bidirektional*.

### Stelligkeit

Die *Aritaet* oder *Stelligkeit* einer Operation bestimmt, wieviele Operanden an
der Operation teilhaben. Hier gibt es:

* Nullstellige Operatoren: $true, false$
* Einstellige/Unaere/Monadische Operatoren: $\neg$
* Zweistellige/Binaere/Dyadische Operatoren: $\land, \lor$

### Semantiken der Operatoren

Gegeben hier nun die Semantik $[F]$ fuer bestimmte Formeln $F$. Sei $G, H$ noch
beliebige andere Formeln.

* $F = true$: Dann ist gilt fuer alle Belegungen $[true](\beta) = 1$

* $F = false$: Dann ist gilt fuer alle Belegungen $[false](\beta) = 0$

* $F = \neg G$ ist $0$, wenn $[G](\beta) = 1$, und $1$, wenn $[G](\beta) = 0$.

* $F = G \land H$ ist wahr, wenn $G$ *und* $H$ wahr sind.

* $F = G \lor H$ ist wahr, entweder wenn nur $G$ wahr ist, *oder* auch wenn nur
  $H$ wahr ist, oder auch wenn beide wahr sind (zumindest eines, nicht beide
  nicht).

* $F = G \rightarrow H$ ist falsch, wenn $G$ wahr ist und $F$ nicht. Wenn $G$
  falsch ist, ist $F$ wahr und wenn $G$ wahr ist und $H$, dann auch. Der erste
  Teil wird hierbei mit "ex falso quodlibet" (aus dem Falschen folgt das
  Beliebige) bezeichnet. Ist die *Hypothese* falsch, so kann die *Konklusion*
  beliebig sein, da man nie einen Fall findet, wo $G$ wahr ist und $H$
  falsch. "Wenn der Mond aus Kaese ist, dann bin ich cool". Diese Aussage ist
  immer wahr, weil man nie einen Gegenbeweis finden kann. Dazu muesste man
  zeigen, dass wenn der Mond aus Kaese ist, ich nicht cool bin. Aber ich finde
  nie einen solchen Fall, weil die Hypothese, dass der Mond aus Kaese ist, eben
  nicht wahr ist. $p \rightarrow q$ sagt hierbei ueberigens nichts darueber aus,
  ob $\neg p \rightarrow q$ wahr ist. Dies sind zwei voellig separate Formeln.

* $F = G \oplus H$ ist wahr, wenn maximal eine und mindestens eine, also *genau
  eine* der beiden Formeln $G, H$ wahr ist, und die andere nicht. Nur wenn die
  Wahrheitswerte der beiden Formeln also *verschieden* sind, kann die Aussage
  wahr sein.

* $F = G \leftrightarrow H$ ist wahr, wenn die Wahrheitswerte der beiden Formeln
  $G$ und $H$ *gleich* sind. Dieser Operator ist also das Gegenteil vom
  exklusiven Oder $\oplus$, und kann auch so definiert werden:
  $G \leftrightarrow H \equiv \neg(G \oplus H)$

Fuer $F \rightarrow G$ gilt noch, dass $(F \rightarrow G) \equiv (\neg G
\rightarrow \neg F)$. Wenn, wenn $F$ wahr ist, auch $G$ wahr ist, muss sein,
dass wenn $G$ falsch war, auch $F$ falsch war. Und wenn also gilt, dass wenn $G$
falsch war, auch $F$ falsch war, und $F$ ist aber wahr, dann folgt daraus, dass
$G$ wahr ist. Daraus folgt $G$ ist wahr, wenn $F$ wahr ist.

#### Modell

Zusaetzlich sei noch das Zeichen $\models$ eingefuehrt. $F \models G$ besagt,
dass $F$ ein *Modell* fuer $G$ ist. Das bedeutet, dass *wenn $F$ wahr ist*, dann
ist auch immer $G$ wahr. $F \models G$ heisst also, dass $F \rightarrow G$ immer
wahr ist (Tautologie). Wir sagen: "$G$ folgt aus $F$".

In $F \models G$ wird $F$ die Annahme genannt und $G$ die Konklusion. Hierbei
ist $F$ meist eine Folge von Konjunktionen $F_1 \land F_2 \land ... \land
F_n$. Diese sind also die *Annahmen*. Man ersetzt die Konjunktionszeichen
$\land$ dann oft durch Kommata $,$:

$F_1 \land F_2 \land ... \land F_n \models G :\equiv F_1,F_2,...,F_n \models G$

Insbesondere: gilt $F \models G$, dann: $F \equiv F \land G$.

Die rechte Seite nennt man auch die *Folgerung*.

#### Vollstaendigkeit

Eine Menge $M$ von Operatoren ist *vollstaendig*, wenn jeder moegliche Operator
egal welcher Aritaet durch die Operatoren von $M$ ausgedrueckt werden kann.

Hierbei noch: Ein Operator $OP$ mit beliebiger Stelligkeit $k \geq 0$ kann durch andere
Operatoren $OP_1, ..., OP_n$ ausgedrueckt werden, wenn es eine Formel $F$
*ueber* $OP_1, ..., OP_n$ gibt (also eine Formel, die nur diese Operatoren
enthaelt), sodass: $OP(p_1, ..., p_k) \equiv F$. Also die Operation $OP(p_1,
..., p_k)$ fuer alle $k$ Argumente von $OP$ sind durch eine Formel ausdrueckbar,
wo nur die Operatoren $OP_1, ..., OP_n$ benutzt werden.

Beispiel: $\{\neg, \lor, \land\}$ ist vollstaendig. Man kann damit zum Beispiel
$p \rightarrow q$ zu $\neg p \lor q$ umformen.

##### NEG, OR/AND

Aber: schon $\{\neg, \lor\}$ und $\{\neg, \land\}$ sind vollstaendig! Hierzu sei
gesagt, dass wenn man eine Menge $M$ von Operatoren findet, mit der man alle
Operatoren einer vollstaendigen Menge $Q$ ausdruecken kann, dann ist $M$ auch
(durch transitivitaet) vollstaendig.

Es genuegt hierfuer also zu zeigen, dass z.B. mit $\{\neg, \lor\}$ alle
Operatoren der vollstaendigen Menge $\{\neg, \lor, \land\}$ ausdrueckbar
sind. $\neg$ und $\lor$ sind hierbei natuerlich trivial, also muessen wir nur
zeigen, dass $\land$ durch $\{\neg, \lor\}$ ausdrueckbar ist:

$p \land q \equiv \neg(\neg(p \land q)) \equiv \neg(\neg p \lor \neg q)$

Symmetrisch kann das selbe auch fuer $\{\neg, \land\}$ bzw. $\lor$ beweisen,
wieder durch Doppelte Negation.

##### NAND/NOR

Letztlich: Alleine schon NAND $\{\barwedge\}$ und NOR $\{\veebar\}$ sind
vollstaendig! Hierbei ist $F \barwedge G \equiv \neg(F \land G)$ und $F \nor G
\equiv \neg(F \lor G)$.

Beweis fuer $\{nand\}$ wieder durch Herstellung der bewiesen vollstaendigen
Menge $\{\neg, \land, \lor\}$:

$\neg F \equiv \neg(F \land F) \equiv (F \barwedge F)$

$F \land G \equiv (F \barwedge G) \barwedge (F \barwedge G)$

$F \lor G \equiv \neg(\neg F \land \neg G) \equiv (F \barwedge F) \barwedge (G \barwedge G)$

Symmetrisch fuer NOR.

## Gueltigkeit

### Tautologie

Eine Formel $F$ nennt man *Tautologie* oder *(allgemein)gueltig*, wenn $F$ immer
wahr ist. Eine Formel die gueltig ist ($\neq$ wahr!), ist also in jeder
moeglichen, minimalen und passenden Belegung immer wahr.

In einer Wahrheitstabelle erkennt man eine Tautologie daran, dass $[F](\beta)$
immer, also in jeder Zeile, $1$ ist.

Man schreibt fuer eine tautologische bzw. gueltige Formel $F$ auch $\models
F$. Hierbei ist $true \models F$ gemeint, wobei das $true$ implizit ist.

Beispiele:
* $p \lor \neg p$ (die sogenannte *triviale Tautologie*)
* $p \rightarrow p$
* $(p \land q) \rightarrow p$
* $p \rightarrow (q \rightarrow p)$

### Widerspruch

Gegensaetzlich zur Tautologie nennt man eine Formel $F$ nun einen *Widerspruch*,
wenn $F$ unter jeder moeglichen, passenden und minimalen Belegung $\beta$ immer
falsch ist. Also $[F](\beta) = 0 \,\forall \beta$.

In einer Wahrheitstabelle erkennt man eine Tautologie daran, dass $[F](\beta)$
immer, also in jeder Zeile, $0$ ist.

Einige Aequivalenzen:
* Ist $F$ eine Tautologie, so ist $\neg F$ ein Widerspruch.
* Ist $F$ ein Widerspruch, so ist $F \equiv false$.
* Ist $F$ ein Widerspruch, so gilt $F \models false$ weil $F \rightarrow false$
  immer wahr ist, weil $F$ immer falsch ist und ex falso quodlibet.

### Erfuellbarkeit

Eine Formel $F$ ist erfullbar, wenn es zumindest eine Belegung $\beta$ gibt, die
zu $F$ passt, und unter welcher $F$ wahr ist:

$F \text{ erfuellbar } \Leftrightarrow \exists \beta: [F](\beta) = 1$

In einer Wahrheitstabelle muss also nur eine Zeile von $[F](\beta)$ eine $1$
enthalten.

Nun koennen wir auch sagen: $F \text{ ist Widerspruch} \Leftrightarrow F
\text{ ist nicht erfuellbar}$.

Ebenso natuerlich: $F \text{ gueltig} \Rightarrow F \text{ erfuellbar}$. Aber
nicht zwingend umgekehrt!

Weiter: Die Implikation ist immer erfuellbar, da man die Hypothese immer als
falsch belegen kann.

## Aequivalenz

Zwei Formeln $F$ und $G$ sind logisch aequivalent ($F \equiv G$) genau dann,
wenn fuer jede Belegung, die zu $F$ und zu $G$ passt, gilt:

$[F](\beta) = [G](\beta)$

Wenn also $F$ und $G$ aequivalent sind, dann sind sie zwei verschiedene
Schreibweisen der selben Aussage. Man beachte aber, dass sich $\equiv$ von
$\leftrightarrow$ dadurch unterscheidet, dass $\leftrightarrow$ ein Junktor ist,
der eine Semantik hat und somit einen Wahrheitswert produzieren kann. $\equiv$
ist zu $\leftrightarrow$ wie $\models$ zu $\rightarrow$. Zwei Formeln sind also
immer genau dannn aequivalent ($F \equiv G$), wenn $F \leftrightarrow G$ eine
Tautologie ist.

Moechte man also eine Aequivalenz pruefen (dass $F \equiv G$), so kann
entweder die Wahrheitstabellen beider Formeln separat aufstellen und ueberpruefen
dass $[F](\beta) = [G](\beta) \forall \beta$, oder man ueberprueft, dass $F
\leftrightarrow G$ gueltig ist (z.B. wieder in einer Wahrheitstabelle).

Die Relation $\equiv$ ist natuerlich auch eine Aequivalenzrelation:
* reflexiv: $F \equiv F$
* transitiv: $F \equiv G, G \equiv H \rightarrow F \equiv H$
* symmetrisch: $F \equiv G \rightarrow G \equiv F$

Die Relation $\equiv$ ist auch eine *Kongruenz*, was bedeutet, dass man in einer
Formel $F$ eine Teilformel $X$ durch eine zu $X$ aequivalente Formel immer
ersetzten bzw. diese einsetzen kann.

Da $\equiv$ eine Aequivalenzrelation ist, koennn wir $\equiv$ auch verketten:
$F \equiv G \equiv ... \equiv X$

### Aequivalenzregeln

Hier nun aufgelistet einige Aequivalenzregeln fuer die Aussagenlogik, wo also
die linke Seite eine andere Schreibweise fuer die rechte ist. Sei hier $\circ$
eine beliebige Operation $\in \{\land, \lor\}$ und sei $\oplus$ jeweils der andere
Operator als das konkrete $\circ$.

* *Identitaet*: $F \land true \equiv F, F \lor false \equiv F$.

* *Dominanz*: $F \lor true \equiv true$, $F \land false \equiv false$

* *Idempotenz*: $F \circ F \equiv F$

* *Involution*: $\neg \neg F \equiv F$

* *Triviale Tautologie*: $F \lor \neg F \equiv true$

* *Triviale Kontradiktion*: $F \land \neg F \equiv false$

* *Kommutativitaet*: $F \circ G \equiv G \circ F$

* *Assoziativitaet*: $(F \circ G) \circ H \equiv F \circ (G \circ H)$

* *Distributivitaet*: $F \circ (G \oplus H) \equiv (F \circ G) \oplus (F \circ
  H)$

* *DeMorgan*: $\neg(F \circ G) \equiv \neg F \oplus \neg G$

* *Absorption*: $F \circ (F \oplus G) \equiv F$

* Folgt aus Distributivitaet: $F \land (\neg F \lor G) \equiv F \land G$
* Folgt aus Distributivitaet: $F \lor (\neg F \land G) \equiv F \lor G$

Bei der Identitatsregel erkennen wir auch, dass es fuer die Disjunktion $\lor$
ein neutrales Element (Wahrheitswert) gibt: $false$. Und auch fuer die
Konjunktion: $true$.

#### Vereinfachung

Die Implikation $\rightarrow$, das exklusive Oder $\oplus$ und das Bidirektional
$\leftrightarrow$ lassen sich noch zu einfacheren Operationen reduzieren:

* $F \oplus G \equiv (F \land \neg G) \lor (\neg F \land G)$

* $F \leftrightarrow G \equiv (F \rightarrow G) \land (G \rightarrow F)$
* $F \leftrightarrow G \equiv (F \land G) \lor (\neg F \land \neg G)$

* $F \rightarrow G \equiv \neg F \lor G$

Und wie gesagt $F \oplus G \equiv \neg(F \leftrightarrow G)$ und umgekehrt.

### Beweistechniken

* Um zu zeigen, dass eine Formel $F$ gueltig (Tautologie) ist, reduziere $F$
  mittels obiger Aequivalenzregeln soweit, dass $true$ rauskommt. Dann ist $F
  \equiv true$.

* Um zu zeigen, dass eine Formel $F$ nicht gueltig ist, finde eine Belegung
  $\beta$, sodass die Formel nicht gilt. Reduziere $F$ womoeglich vorher noch
  mittels obiger Aequivalenzregeln.

* Um zu zeigen, dass $F \equiv G$, forme $F$ zu $G$ um oder umgekehrt.

* Um zu zeigen, dass $F \not\equiv G$, finde eine Belegung, sodass die eine
  Formal wahr ist aber die andere nicht. Forme die Formeln womoeglich noch
  vorher um.

## Darstellungsweisen

Neben der Wahrheitstabelle gibt es noch zwei weitere Moeglichkeiten,
aussagenlogische Formeln darzustellen:

### Syntaxbaum

In einem Syntaxbaum wird eine aussagenlogische Formel als Baum dargestellt, wo
in den Blaettern immer Literale stehen und alle inneren Knoten Operationen
darstellen. Die Wurzel ist dann die ausserste Operation, die auch den
endgueltigen Wahrheitswert der Semantik fuer die Funktion unter einer bestimmten
Belegung bestimmt.

Sei $F = (\neg a \land b) \rightarrow (c \oplus (d \lor e))$

```
    ->
   /  \
 AND   ^
 / \   | \
!   b  c  OR
|        /  \
c	    d    e
```

### KV-Diagramme

Karnaugh-Veitch-Diagramme koennen fuer aussagenlogische Formeln gleich
aufgestellt werden, wie fuer Mengen. Fuer $n$ Variablen stellt man eine
rechteckige Tafel mit $2^n$ Feldern auf, wobei ein Feld fuer eine bestimmte
Belegung steht. In einem Feld treffen sich also negierte und nicht negierte
Variablen. Im Feld selbst steht dann ein boole'scher Wahrheitswert, der besagt,
ob die Formel unter dieser Belegung zu $0$ oder $1$ auswertet. Das ganze
Diagramm visualisiert also wie eine Wahrheitstabelle die *Semantik* der Formel.

```
     a   !a
 b [ 0 | 1 ]
!b [ 1 | 1 ]
```

## Normalformen

Da es viele Moeglichkeiten gibt, Formeln durch Aequivalenzumformungen anders
aussehen zu lassen, muss es bestimmte genormte Schreibweisen geben, um
aussagenlogische Formeln aufzuschreiben.

Hierzu einige Begriffe:

* Ein *Literal* ist eine Aussagenvariable oder die Negation davon.

* Eine *Klausel* ist eine Disjunktion von Literalen oder die Formel $false$,
  wenn es keine Literale gibt.

Definitionen von leeren Mengen:

* leere Klausel = leere Disjunktion = false (neutrales Element)
* leere Formel = leere Konjunktion = true (neutrales Element)

### KNF

Eine Formel ist in *konjunktiver Normalform* (KNF), wenn es eine Konjunktion von
Disjunktionen bzw. Klauseln ist. Dann hat diese Formel also die Form:

$\bigwedge_{i=1}^n(\bigvee_{j=1}^{m_i} L_{i,j})$

wobei $L_{i,j}$ ein Literal aus der Menge aller Literale $\{A_1,A_2,...\} \cup
\{\neg A_1, \neg A_2\}$, $i$ ueber die Klauseln iteriert und $j$ ueber die $m_i$
Literale in der $i$-ten Klausel.

Da das neutrale Element der Konjunktion $true$ ist, ist die leere KNF
aequivalent zu $true$.

Eine Formel ist in KNF, falls in allen Pfaden ihres Syntaxbaums *Konjunktionen
vor Disjunktionen vor Negationen* kommen. Also Konjunktionen sind am
aeussersten, daher am hoechsten im Syntaxbaum. Danach folgen Disjunktionen in
den Klauseln. Und erst zuletzt folgen Negationen direkt vor den Variablen.

#### Konstruktion aus einer Wertetabelle

Um eine Formel $F$ in die Konjunktive Normalform (KNF) zu bringen, stelle eine
Wahrheitstabelle auf und dann:

1. Fuer jede Zeile, fuer welche das Ergebnis $0$ ist.
2. Verbinde alle Variablen negiert oder nicht negiert (je nach Belegung) mit
   Konjunktionen.
3. Negiere nun alle diese Konjunktionen. Dadurch entsehen Disjunktionen.
4. Verbinde nun alle diese Disjunktionen durch Konjunktionen.

$F = (x \land y) \lor x$

| x | y | $[F](\beta)$ |
|:--|:--|:-------------|
| 0 | 0 |       0      | <--
| 0 | 1 |       0      | <--
| 1 | 0 |       1      |
| 1 | 1 |       1      |

Hierbei ist es wichtig zu verstehen, was die KNF intuitiv ausdrueckt. Wir wollen
ja eine Formel, die unter den richtigen Belegungen wahr ist (jene Belegungen,
die die Formel auch vorher wahr machten). Nun sagen wir bei der KNF, dass die
Formel wahr ist, wenn keiner der Faelle zutrifft, der die Formel falsch
macht. Also wenn die Konkrete Belegung $\beta$ unter welcher die Semantik $[F]$
von $F$ evaluiert wird, den Wahrheitswert $true$ produzieren soll, dann muss sie
keine der Belegungen sein, die laut Wahrheitstabelle $false$ produzieren. Also
die Formel ist wahr, "wenn die Belegung *nicht* diese ist, *und* *nicht* diese
ist, *und* *nicht* diese ist, ...". Durch die Negation kommen aus den
urspruenglichen Konjunktionen dann Disjunktionen raus.

Also fuer die Formel oben: Die Formel evaluiert zu $true$, wenn die Belegung
nicht $(\neg x \land \neg y)$ ist und nicht $(\neg x \land y)$ ist, also: $F =
\neg(\neg x \land \neg y) \land \neg(\neg x \land y)$. Nun wenden wir deMorgan
an: $(x \lor y) \land (x \lor \neg y)$. (Wir ignorieren hier dass die Formel zu
$x$ reduziert werden kann).

#### Konstruktion aus einer Formel

Eine beliebige Formel $F$ kann mit den unteren zwei Regeln in KNF gebracht
werden. Zuerst aber nochmal, im Syntaxbaum einer KNF:

* Sind alle Konjunktionen ueber Disjunktionen.
* Alle Disjunktionen ueber Negationen (also Negationen ganz unten).

1. Ziehe Negationen so weit nach innen wie moeglich:
   * $\neg \neg x$ wird zu $x$ (Involution)
   * $\neg(x \land y)$ wird zu $\neg x \lor \neg y$ (deMorgan)
   * $\neg(x \lor y)$ wird zu $\neg x \land \neg y$ (deMorgan)

   Nach diesem Schritt sind Negationen ganz unten im Syntaxbaum.

2. Distributiere Disjunktionen ueber Konjunktionen:

	* $F \lor (G \land H)$ wird zu $(F  \lor G) \land (F \lor H)$
	* $(F \land G) \lor H$ wird zu $(F \lor H) \land (G \lor H)$

	Nach diesem Schritt sind alle Konjunktionen ueber Disjunktionen im
    Syntaxbaum.

#### Mengendarstellung

Formeln in Konjunktiver Normalform koennen mit Mengen dargestellt werden. Dabei
wird eine jede Klausel (also jede Disjunktion oder $false$) durch eine Menge von
Literalen dargestellt. Diese Darstellungsform von KNF-Formeln nennt man
*Mengendarstellung*.

Beispiel: $(p \lor \neg q \lor s)$ wird als $\{p, \neg q, s\}$ dargstellt.

Eine KNF-Formel kann dann als Menge von Klauseln beschrieben werden, wobei die
einzelnen Klausen eben auch in ihrer Mengendarstellung enthalten sind. Die Menge
fuer die Formel, die die Klauseln enthaelt, wird dann *Klauselmenge* genannt.

Beispiel: $\neg p \land (p \lor \neg q \lor s)$ wird als $\{\{\neg p\}, \{p,
\neg q, s\}\}$ dargestellt.

Also eine Klauselmenge ist eine Menge von Klauseln: $\{Menge, Menge, Menge\}$,
wobei jede $Menge$ eine Klausel ist.

Ueber die leere Klausel bzw. Klauselmenge:

* Die *leere Klausel* wird als leere Menge dargestellt und steht fuer $false$,
  da $false$ der neutrale Wahrheitswert fuer die Disjunktion ist.

* Die *leere Klauselmenge* stellt hingegen die Formel $true$ dar, da $true$
  neutral gegenueber der Konjunktion ist.

Die Klauselmengendarstellung einer KNF-Formel hat den Vorteil, dass man sich
nicht um Idempotenz kuemmern muss ($A \lor A = A$), weil $A$ immer nur einmal in
einer Menge (Klausel) vorkommen kann.

### DNF

Die Disjunktive Normalform (DNF) ist das Gegenstueck zur KNF. Es verbindet
Konjunktionen mit Disjunktionen. Hierbei sind also Negationen im Syntaxbaum noch
immer unter Konjunktion und Disjunktion, aber nun ist Disjunktion auch ueber
Konjunktion. In den Formeln sind die Disjunktionen also ganz aussen.

Um eine Formel $F$ in die Disjunktive Normalform (DNF) zu bringen, stelle eine
Wahrheitstabelle auf und dann:

1. Fuer jede Zeile, fuer welche das Ergebnis $1$ ist.
2. Verbinde alle Variablen mit Konjunktionen.
3. Verbinde alle diese Konjunktionen durch Disjunktionen.

$F = (x \lor y) \land x$

| x | y | $[F](\beta)$ |
|:--|:--|:-------------|
| 0 | 0 |       0      |
| 0 | 1 |       0      |
| 1 | 0 |       1      | <--
| 1 | 1 |       1      | <--

DNF dann also: $(x \land y) \lor (x \land \neg y)$

Intuitiv kann man sich hier vorstellen, dass die Formel wahr ist, wenn die
Belegung "diese ist, *oder* diese ist, *oder* diese ist, ..."

### Erfuellbarkeitsaequivalente Umformungen

Kanonische konjunktive Normalformen koennen fuer viele Variablen sehr gross
werden. Daher gibt es eine Moeglichkeit, eine Formel, die nicht in KNF ist, in
eine *erfuellbarkeitsaequivalente* andere Formel umzuformen. Dazu geht man so
vor:

Man ordnet jeder nicht atomaren Teilformel, d.h. jedem inneren Knoten im
Syntaxbaum bzw. jeder nicht-Variable, zunaechst eindeutig eien neue
aussagenlogische Variable $B_i$ zu. Dann sellt man eine neue aussagenlogische
Formel auf, welche ausdrueckt, dass:

1. Die Formel, welche der ganzen Formel zugeordnet ist, muss mit $1$ belegt
   sein. Somit bleibt die Aequivalenz zwischen der urspruenglichen und neuen
   Formel erhalten.
2. Jede Variable $B_i$ mit genau dem Wahrheitswert belegt sein muss, den die
   zugeordnete Teilformel unter genau der Belegung annimmt.

Der Sinn davon ist, dass die so generierte Formel viel kuerzer sein kann, als
eine KNF fuer eine Formel. Die Anzahl der Klauseln waechst fuer diese
Darstellungsvariante nur linear mit der Anzahl der Variablen in der
urspruenglichen Formel, waehrend eine reine KNF exponentiell waechst (in der
Anzahl der Variablen).

Sei als Beispiel $F$ die Formel $(A_1 \land A_2) \lor (A_3 \land A_4)$. Nun
haette man im Syntaxbaum als Wurzel das aussere $\lor$, und zwei innere Knoten,
die die Konjunktionen darstellen. Man ordnet der Wurzel also nun die
aussagenlogische Variable $B_1$ zu, und den beiden Teilformeln mit den
Konjunktionen $B_2$ bzw. $B_3$. Dann ist eine erfuellbarkeitsaequivalente Formel
diese:

$B_1 \land (B_2 \leftrightarrow (A_1 \land A_2)) \land (B_3 \leftrightarrow (A_3
\land A_4))$.

Wie man sieht, drueckt diese Formel also aus, dass, damit sie wahr ist, $B_1$,
also die ganze Formel $F$ wahr sein muss. Da es aber mehrere passende,
erfuellende Belegungen geben kann, wird noch ueberprueft, dass die Variablen
fuer die Teilformeln auch wirklich den Wahrheitswert der urspruenglichen
Variablen der Formel $F$ haben.

Der Sinn ist, dass diese generierte Formel wahr ist, genau dann wenn die
urspruengliche Formel wahr ist. Es besteht also eine Bidirektionalitaet zwischen
diesen Formeln. Aber diese Form ist fuer viele Variablen kuerzer. Man kann dann
also trotzdem z.B. den DPLL oder Resolutionsalgorithmus anwenden, um die Formel
zu ueberpruefen.

## Algorithmen

Es gibt zwei Algorithmen um die Erfuellbarkeit oder Nicht-Erfullbarkeit einer
aussagenlogischen Variable zu bestimmen:

* DPLL (Davis-Putnam-Logemann-Loveland)
* Resolution (Robinson)

### DPLL

Der DPLL-Algorithmus versucht, die Anzahl der zu pruefenden Belegungen fuer eine
aussagenlogische Formel zu reduzieren. Er ist dabei auf *Erfuellbarkeit*
gerichtet. Das bedeutet, dass er schnell terminiert, wenn die Formel erfuellbar
ist aber laenger braucht, um zu zeigen, dass die Formel nicht erfuellbar ist
(Widerspruch).

Der Grundgedanke des DPLL ist es, Variablen mit $true$ oder $false$ zu ersetzen
und jedesmal die Formel zu vereinfachen. Dadurch enstehen einfachere Formeln,
die rekursiv wieder vereinfacht werden. Landet man durch dieses rekursive
Ersetzen bei der Formel $true$, so hat man eine erfuellende Belegung fuer die
Formel gefunden. Landet man bei $false$, geht man eine Rekursionsstufe hoeher
(bottom-out) und versucht die naechste Variable aus (quasi DP).

Der DPLL-Algorithmus verwendet fuer eine Formel $F$ die Notation
$F[p\setminus true]$. Damit ist gemeint, das jedes Vorkommen von $p$ in $F$
durch $true$ ersetzt wird. Selbes gilt fuer $false$. Dann werden die folgenden
Regeln zur Vereinfachung verwendet:

* $F \land true \equiv F$ (Identaet)
* $F \lor false \equiv F$ (Identitaet)
* $F \lor true \equiv true$ (Dominanz)
* $F \land false \equiv false$ (Dominanz)

Dann ist der Algorithmus:

```python
def dpll(f):
	if f == 'true':
		return True
	if f == 'false':
		return False

	p = pick_variable(f)

	if dpll(f.replace(p, 'true')):
		return True

	if dpll(f.replace(p, 'false')):
		return True

	return False
```

Bezueglich der Mengendarstellung:

* Einzel-Literale $\{x\}$ die man so ersetzt, dass sie zu $true$ werden, kann
  man aus der Klauselmenge streichen.

* Die leere Klausel steht fuer $false$, also sobald man einemal eine leere
  Klausel in der Klauselmengendarstellung hat, ist die Formel nicht mehr
  erfuellbar.

* Die leere Klausel*menge* steht fuer $true$, also kommt man auf die leere
  Menge, hat man eine erfuellende Belegung gefunden. Das passiert z.B., wenn man
  am Ende nur mehr eine Klausel hat: $\{\{s\}\}$. Ersetzt man nun $s \setminus
  true$, dann erhaelt man (siehe erster Punkt) $\{\}$, also die leere Menge,
  also $true$.

#### Verbesserung

Es gibt noch eine effizientere Variante des DPLL-Algorithmus. Man waehlt hierbei
einfach nicht eine beliebige Variable $p \in V_f$, sondern sieht zuerst:

* Wenn es eine Ein-Literal-Klausel $\{p\}$ gibt, pruefe nur, ob
  $F[p \setminus true]$ erfuellbar ist, weil wenn $\beta(p) = 0$, hat man die
  leere Klausel, welche aequivalent zu $false$ ist, und in einer Konjunktion
  waere das dann sogleich $false$ fuer die ganze Formel.

* Symmetrisch: Wenn es eine Ein-Literal-Klausel $\{\neg p\}$ pruefe nur, ob
  $F[p \setminus false]$ erfuellbar ist, weil $\beta(p) = 1$ hier schon $false$
  fuer die ganze Formel produzieren wuerde.

* Ansonsten, wenn es keine Ein-Literal-Klausel gibt, waehle wieder eine
  beliebige Variable, wie im vorherigen Algorithmus.

Hierbei kann man sich also sinnlose Rekursionsschritte sparen.

### Resolution

War der DPLL-Algorithmus noch auf Erfuellbarkeit gerichtet, ist der
Resolutions-Algorithmus nun auf *Nicht-Erfuellbarkeit* gerichet. Er kann also
schnell einen Widerspruch beweisen, braucht aber laenger, um zu zeigen, dass
eine Formel kein Widerspruch ist.

Die Idee des Resolutionsalgorithmus ist es, aus je zwei Klauseln eine logisch
erlaubte dritte Klausel zu bilden. Diese dritte Klausel kann der Klauselmenge
bzw. der Formel hinzugefuegt werden. Nach mehrmaligem Iterieren kommt man
womoeglich auf die leere Klausel, welche $false$ repraesentiert. Da in einer
Konjunktion schon ein einziges $false$ zu einem totalen $false$ fuer die ganze
Formel fuert, ist das genau das Ziel des Resolutionsalgorithmus. Findet man
jedoch keine weitere Moeglichkeit, neue Klauseln hinzuzufuegen, so ist der
Algorithmus erfuellbar.

Die zusaetzlichen Klauseln werden nach folgendem Prinzip hinzugefuegt: Sei $F$
eine KNF-Formel mit den Klauseln $(a_1 \lor ... \lor a_k \lor p)$ und $(b_1 \lor
... \lor b_k \lor \neg p)$. Man beachte hier dass $p$ einmal negiert und einmal
nicht negiert ist. Dann benutzt man die Aequivalenz $X \rightarrow Y \equiv \neg
X \lor Y$ um die Klauseln umzuformen:

* $(b_1 \lor ... \lor b_k \lor \neg p) \equiv p \rightarrow (b_1 \lor ... \lor
  b_k)$. Wenn $\neg p$, dann ist die ganze Klausel wegen Dominanz wahr, aber
  wenn $p$, dann muss zumindest eines der anderen Literale $true$ sein.

* $(a_1 \lor ... \lor a_k \lor p) \equiv \neg(a_1 \lor... \lor a_k) \rightarrow
  p$. Also, wenn $(a_1 \lor ... \lor a_k)$ $true$ ist, ist die ganze Klausel
  $true$, aber sonst muss $p$ wahr sein.

Nun haben wir also $\neg(a_1 \lor ... \lor a_k) \rightarrow p$ und $p
\rightarrow (b_1 \lor ... \lor b_k)$. Ueber Transitivaet folgt also:

$\neg(a_1 \lor ... \lor a_k) \rightarrow (b_1 \lor ... \lor b_k) \equiv (a_1
\lor ... \lor a_k) \lor (b_1 \lor ... \lor b_k)$.

Somit erhaelt man also mittels diesen aussagenlogischen Regeln die Disjunktion
aller Literale, die nicht $p$ sind. Diese Klausel darf man der Menge
hinzufuegen. Eine solche Klausel kann dann z.B. mit den Mengen $\{p\}$ und
$\{\neg p\}$ zur leeren Klausel werden, welche $false$ ist und die ganze Formel
$false$ macht.

Die entstandene Klausel nennt man *Resolvent* der beiden Ursprungsklauseln. Nun
kann man also definieren: Seien $K_1, K_2$ und $R$ Klauseln in
Mengendarstellung. Dann heisst $R$ Resolvent von $K_1$ und $K_2$, wenn es ein
Literal $L$ gibt, sodass $L \in K_1$ und $\neg L \in K_1$, und $R = (K_1
\setminus \{L\}) \cup (K_2 \setminus \{\neg L\})$. Dann gilt laut
Resolutionsverfahren:

$F \equiv F \cup \{R\}$

Die leere Klausel stellt man hierbei oft mit einem offenen Quadrat dar:
$\square$.

## Verifikation

Aussagenlogik kann benutzt werden, um Funktionen mit endlicher Urbild- und
Bildmenge zu beschreiben, was die Grundlage fuer die Verwendung von
Aussagenlogik im Entwurf von booleschen Schaltungen, in der Testfallerzeugung
und in der Analyse und Verifikation von Programmen bildet.

Sei zum Beispiel $f$ eine Funktion $\{0, 1, 2, 3\} \rightarrow \{0, 1, 2, 3\}: x
\mapsto (x + 1) \mod 4$. Diese Funktion kann mittels Aussagenlogik so
beschrieben werden:

$\phi_f := (y_0 \leftrightarrow \neg x_0) \land (y_1 \leftrightarrow (x_1
\oplus x_0))$

wobei $x_0, x_1$ die Bits vom Eingangswort $x$ sind (hier genuegen zwei Bits),
und $y_0,y_1$ die Bits vom Ausgangswort $y$ sind. In der Formel sind sie aber
aussagenlogische Variablen. Dann kann man zeigen, dass jede fuer $\phi_f$
passende Belegung genau dann wahr ist, wenn der $y$-Wert laut Funktion zu $x$
passt:

$[\phi_f](\beta) = 1 \Leftrightarrow \beta(y_0) + 2\beta(y_1) = f(\beta(x_0) +
2\beta(x_1))$.

$\beta(y_0) + 2\beta(y_1)$ ist hier gleich $y$. Man erinnere sich, dass $y_0,
y_1$ die Bits von $y$ sind. Also wenn $y$ mit $k$ Bits ausgedrueckt werden kann,
gilt $y = \sum_{i=0}^{k-1} 2^i y_i$. Somit steht oben $y = f(x)$ genau dann wenn
$[\phi_f](\beta) = 1$. Es gibt also nur ein passendes Paar an Bits $y_0,y_1$
fuer ein Eingangs-Paar $x_0,x_1$.

Somit kann man einen Algorithmus bzw. eine Funktion formal spezifizieren (eine
Funktion alleine ist noch nicht Formal genug) und ueberpruefen, ob eine
Implementierung gemaess der Spezifikation passt.

### Vorgehensweise

Um nun eine beliebige Funktion $f$ in eine formale aussagenlogische Formel
$\phi_f$ umzuformen, legt man zuerst fest, welche Eingangswerte welche
Ausgangswerte produzieren. Dies legt man in einer Tabelle fest, wobei fuer einen
Eingangs-/Ausgangswert die Bits angegeben werden.

Sei z.B. $g$ eine Funktion $\{0, 1, 2, 3\} \rightarrow \{0, 1, 2, 3\}: x \mapsto
2x \mod 4$. Dann schreibt man sich zuerst die Abbildung explizit auf:

* (x) $0 \mapsto 0$ (y)
* (x) $1 \mapsto 2$ (y)
* (x) $2 \mapsto 0$ (y)
* (x) $3 \mapsto 2$ (y)

Und legt nun eine Tabelle fuer die Bits der Werte $x, y$ an:

|$x_1$|$x_0$|$y_1$|$y_0$|
|:----|:----|:----|:----|
|  0  |  0  |  0  |  0  |
|  0  |  1  |  1  |  0  |
|  1  |  0  |  0  |  0  |
|  1  |  1  |  1  |  0  |

#### Explizit

Es gibt nun mehrere Moeglichkeiten, eine Formel $\phi_g$ zu finden. Die
einfachste ist, die Funktion explizit ueber Implikationen zu definieren. "Wenn
$x$ diesen Wert hat, dann muss $y$ diesen Wert haben, und wenn $x$ diesen
anderen Wert hat, ...":

$\phi_g := ((\neg x_1 \land \neg x_0) \rightarrow (\neg y_1 \land \neg y_0))
\land ((\neg x_1 \land x_0) \rightarrow (y_1 \land \neg y_0)) \land ...$.

Wegen der Semantik der Implikation werden Eingangswoerter, die nicht zur
Belegung passen, ignoriert (wenn $x_0 \land x_1$ in eine Klammer nicht gilt, ist
die Aussage wahr und neutral gegenueber den anderen Klammern, insbesondere
jener, wo die Bits zum Eingangswort in der Belegung passen).

#### DNF

Ein andere Moeglichkeit ist es, fuer die Ausgangsbits $y_0$ und $y_1$ separat
eine DNF in Abhaengigkeit der Eingangsbits zu machen. Z.B. fuer $y_1$ folgt
dann:

|$x_1$|$x_0$|$y_1$|
|:----|:----|:----|
|  0  |  0  |  0  |
|  0  |  1  |  1  |
|  1  |  0  |  0  |
|  1  |  1  |  1  |

$(\neg x_1 \land x_0) \lor (x_1 \land x_0)$

Man weiss also nun schon, dass $y_1$ wahr ist, genau dann wenn die obige DNF
gilt. Das kann man dann mit einem Bidirektional ausdruecken: $y_1
\leftrightarrow (\neg x_1 \land x_0) \lor (x_1 \land x_0)$. Man merkt aber auch,
dass diese DNF sehr kanonisch ist. Man kann $x_0$ raus-distribuieren (ziehen),
dann steht:

$x_0 \land (\neg x_1 \lor x_1) \equiv x_0 \land true \equiv x_0$.

Fuer $y_0$ gibt es keine erfuellende Belegung, also gilt sicher $\neg y_0$ fuer
die Formel. Formal ist die DNF fuer $y_0$ $false$: $y_0 \leftrightarrow false$,
aber das ist aequivalent zu $\neg y_0$.

Also kann man nun die beiden DNF konjugieren:

$(y_1 \leftrightarrow x_0) \land \neg y_0$.

#### Beobachten

Letztlich ist es noch moeglich, auch ohne DNF Beobachtungen zu machen, die einen
zu dieser Formel bringen. Man bedenke hierbei, dass $=$ in der Aussagenlogik zu
$\leftrightarrow$ uebersetzt. Generell sollte man versuchen, Muster zu
finden. Man versuche dabei solche Beobachtungen zu machen (oder andere):

* Ist ein Bit immer $1$ oder $0$? Dann kann er separat negiert oder nicht
  stehen.
* Ist ein Bit wahr, genau dann, wenn ein anderer es auch/nicht ist? Dann kann man
  diese mit $\leftrightarrow$/$\oplus$ verbinden.
* Ist ein Bit wahr, genau dann, wenn zwei andere verschieden sind? Dann kann man
  den Bit mit dem XOR der andern verbinden.

Das Ziel ist es, die Ausgangsbits *eindeutig* zu bestimmen. Findet man eine
Formel fuer $y_0$ und $y_1$, ist man fertig.

Hier wieder die obige Tabelle:

|$x_1$|$x_0$|$y_1$|$y_0$|
|:----|:----|:----|:----|
|  0  |  0  |  0  |  0  |
|  0  |  1  |  1  |  0  |
|  1  |  0  |  0  |  0  |
|  1  |  1  |  1  |  0  |

Man beobachte:

1. $y_0$ ist immer $0$, kann also separat als $\neg y_0$ stehen. Nur dann ist
   die ganze Formel $1$.
2. $y_1$ ist $1$, genau dann, wenn $x_0$ eins ist. Man kann sie also mit
   $\leftrightarrow$ verbinden.
3. Wir haben nun $y_0$ und $y_1$ eindeutig bestimmt. Bit $x_1$ ist unnoetig.

Man sollte hier nochmal ueberpruefen, ob man durch seine Beobachtungen die
funktionalen Abhaengigkeiten immer genau erreicht. Wenn ja, kann man eine Formel
aufstellen:

$\circ_g := (y_1 \leftrightarrow x_0) \land \neg y_0$.

Fertig.

## KNSAL (Kalkuel des natuerlichen Schliessens fuer Aussagenlogik)

Ein *Kalkuel* ist eine Menge von Inferenzregeln. Mit diesen Inferenzregeln kann
man Aussagen der Form:

$A_1 \land ... \land A_n \vdash F$

beweisen. Hierbei sind $A_1, ..., A_n$ eine Konjunktion von *Annahmen*, ueber
welche man versucht, die Formel $F$ *abzuleiten*. Hierbei sei angmerkt, dass
$\vdash$ nicht dasselbe wie $\models$ ist:

* $A_1,...,A_n \models F$ bedeutet: "Wenn $A_1,...,A_n$ alle wahr sind, dann
  auch $F$"

* $A_1,...,A_n \vdash F$ bedeutet: "Aus den Annahmen $A_1,...,A_n$ laesst sich
  $F$ mit den Inferenzregeln ableiten".

Wenn wir also zeigen koennen, dass wir mit den logischen Inferenzregeln aus den
Annahmen $A_1,...,A_n$ zu $F$ kommen (wenn man $F$ *ableiten* kann), dann muss
gelten, dass $A_1,...,A_n$ ein Modell fuer $F$ ist. Das bedeutet wiederum, dass
wenn $A_1,...,A_n$ alle wahr sind, dann folgt daraus, dass auch $F$ wahr ist.

Die Inferenzregeln, die wir hier anwenden, sind aber keine
Aequivalenzufmormungen. Wir nutzen die Regeln der Syntax des Kalkuels bzw. der
Aussagenlogik, um nur durch *syntaktische* Umformungen die Verbindung zwischen
den Annahmen und den Formeln zu zeigen. Wir haben keinerlei *semantische*
Informationen ueber die Aussagen (oder brauchen sie hierfuer zumindest nicht).

Dieses Kalkuel nennt man *Kalkuel des natuerlichen Schliessens* (KNS). Weil es
auch in der Praedikaten- bzw. anderen Logiken Verwendung findet, nennen wir
*Kalkuel des natuerlich Schliessens fuer Aussagenlogik* (KNSAL).

Das KNSAL hat zwei Eigenschaften, die es verwendbar machen. Es ist:

* korrekt: wenn $A \vdash F$, dann $A \models F$
* vollstaendig: wenn $A \models F$, dann $A \vdash F$

Daher gilt:
$A_1,...,A_n \models F \Leftrightarrow A_1,...,A_n \vdash F \Leftrightarrow
A_1,...,A_n \vdash F \text{ ist gueltig}$.

### Format

Um im KNSAL aus Annahmen Folgerungen abzuleiten, werden die vorhergehenden
Annahmen und Folgerungen ueber einem Strich gezeichnet, und die resultierenden
Annahmen und Folgerungen darunter.

Die Annahmen und Folgerungen ueber dem Strich nennt man hierbei die
*Praemissen*, und die resultierenden Formeln unter dem Strich die *Folgerung*
der Praemissen.

Intuitiv heisst das: "Im die Aussage unter dem Strich zu zeigen, reicht es alle
Aussagen ueber dem Strich (getrennt voneinander) zu zeigen".

Den Strich nennt man auch *Folgerungsstrich*.

Man beachte, wann Annahmen in Teilableitungen weggelassen werden duerfen: Unter
den Annahmen $A_1,...,A_n$, die durch Kommata verbunden sind, (die Menge der
Annahmen), darf eine Annahme in einem Teilbeweis auch fallen gelassen
werden. Ist in unseren Annahmen aber eine Konjunktion $A \land B$, dann gibt es
vorerst keine *syntaktische* Regel, die diese Annahmen zu $A,B$ umformbar
machen. Die Konjunktion muss also stehen bleiben! Somit darf man aus $A \land B$
nicht $A$ oder $B$ wegkuerzen.

### Inferenzregeln

Hier nun die Inferenzregeln fuer das KNSAL.

#### Annahmeregel

Die Annahmeregel besagt einfach, dass aus den Annahmen immer jede der Annahmen
folgt. Das ist so, weil wir aus $X$ immer $X$ folgern koennen. Sei $A_1,...,A_n$
die  Menge der Annahmen. Dann gilt fuer jedes $A \in A_1,...,A_n$, dass wir $A$
aus den Annahmen folgern koennen:

$\forall A \in A_1,...,A_n: \frac{}{A_1,...,A_n \vdash A}$

Hierbei sei angemerkt, dass die Annahme auf der rechten Seite von $\vdash$
*syntaktisch* gleich der selben Annahme auf der linken Seite sein
muss. Semantisch aequivalent (durch Aequivalenzumformungen ableitbar) ist nicht
ausreichend!

Das bedeutet auch, dass wenn rechts nur ein Teil der Formeln steht, die
Annahmeregel noch nicht anwendbar ist. Denn $F \land G$ ist zwar semantisch
aequivalent zu $F$, aber syntaktisch gesehen nicht dieselbe Zeichenkette. Man
muss erst noch durch Konjunktionsbeseitigung die anderen Annahmen mitnehmen: "um
zu zeigen, das aus den Annahmen $F$ folgt, reicht es zu zeigen, dass aus den
Annahmen $F \land G$ folgt." Wenn $F \land G$ eben die Menge der Annahmen ist,
dann darf man die Annahmeregel zur Festellung der Gueltigkeit verwenden.

#### Ausgeschlossenes Drittes

Diese Regel besagt, dass aus den Annahmen entweder die Folgerung $F$, oder ihre
Negierung folgen muss. Eines von beiden muss ja immer ableitbar sein:

$\frac{}{A_1,...,A_n \vdash F \lor \neg F}$

#### Regel fuer $false$

Wenn aus einer Konjunktion von Annahmen der Wahrheitswert $false$ gefolgert
werden soll, dann bedeutet das, dass man aus den Annahmen einen *Widerspruch*
erzeugen kann. Ein Widerspruch bedeutet, dass man aus den Annahmen eine
Folgerung, *sowie auch* die Negation der Folgerung ableiten kann (Kurzschluss):

$\frac{A_1,...,A_n \vdash F \, \, A_1,...,A_n \vdash \neg F}{A_1,...,A_n \vdash
false}$

Hierbei muss wirklich die Wahrheitskonstante $false$ rechts stehen und nicht
eine Aussage, aus der ein Widerspruch folgt (z.B. trivialer Widerspruch $X \land
\neg X$).

Intuition fuer $X \vdash false$: "Wenn $X$ wahr ist, dann ist false wahr" ist
ein Widerspruch.

#### Regel fuer $true$

Die Aussage $X \rightarrow true$ bzw. $X \models true$ist immer wahr. Ist $X$
$false$, so ist die Aussage wahr. Ist $X$ $true$, so ist die Aussage auch
wahr. Daher kann man immer schreiben:

$\frac{}{A_1,...,A_n \vdash true}$

#### Konjunktionseinfuehrung ($+\land$)

Um zu zeigen, dass man aus einer Menge von Annahmen $A_1,...,A_n$ zwei Aussagen
$A, B$ folgern kann ($A \land B$), reicht es zu zeigen, dass man aus den
Annahmen die beiden Aussagen $A, B$ getrennt folgern kann:

$\frac{A_1,...,A_n \vdash F \,\, A_1,...,A_n \vdash G}{A_1,...,A_n \vdash (F
\land G)}$

#### Konjunktionsbeseitigung ($-\land$)

Um zu zeigen, dass man aus einer Menge von Annahmen $A_1,...,A_n$ eine Formel
$F$ ableiten kann, geht es natuerlich auch, zu zeigen, dass man aus den Annahmen
die Formel *und* eine beliebige andere Formel ableiten kann:

$\frac{A_1,...,A_n \vdash (F \land G)}{A_1,...,A_n \vdash F}$

Diese Regel ist besonders hilfreich, wenn man eine Formel aus den Annahmen
bereits auf der rechten Seite hat, aber noch die anderen Formeln aus den
Annahmen braucht, um die Annahmeregel zu verwenden.

#### Disjunktionseinfuehrung ($+\lor$)

Da die Disjunktion $\lor$ schon bei einem zu $true$ aequivalenten Wert auf einer
Seite ganz zu $true$ auswertet, reicht es, um eine Disjunktion $F \lor G$ aus
einer Menge von Annahmen $A_1,...,A_n$ abzuleiten, nur eine der beiden Formeln
$F$ oder $G$ abzuleiten:

$\frac{A_1,...,A_n \vdash F}{A_1,...,A_n \vdash (F \lor G)}$

#### Disjunktionsbeseitigung ($-\lor$)

Wenn man aus den Annahmen $F \lor G$ ableiten kann, und aus den Annahmen mit $F$
eine Formel $H$ ableiten kann, und aus den Annahmen mit $G$ auch $H$ ableiten
kann, dann kann man aus den Annahmen in jedem Fall auch $H$ ableiten:

$\frac{A_1,...,A_n \vdash (F \lor G) \,\, A_1,...,A_n,F \vdash H \,\,
A_1,...,A_n,G \vdash H}{A_1,...,A_n \vdash H}$

Hierbei sei angemerkt, dass wirklich nur $F$ oder $G$ wahr sein muss. Ist $F$
nicht wahr aber $G$ schon, dann wertet $A_1,...,A_n,F$ automatisch zu $true$ aus
und wir ignorieren den Fall. Es wertet automatisch dazu aus, weil "ex falso
quodlibet" gilt. Wir sagen ja "*wenn* die Annahmen und $F$ wahr sind, dann
...". Wenn die Annahmen nicht wahr sind, ist die Aussage eben immer wahr
(bzw. egal).

#### Implikationseinfuehrung ($+ \rightarrow$)

Um zu zeigen, dass wenn die Annahmen $A_1,...,A_n$ alle wahr sind, und wenn $F$
wahr ist, dann $G$ wahr ist, genuegt es zu zeigen, dass wenn $A_1,...,A_n,F$
wahr ist, also "die Annahmen wahr sind und $F$ wahr ist", dann auch $G$ wahr
ist. Es sind wirklich nur zwei verschiedene Schreibweisen, was man an der
Uebersetzung in die deutsche Sprache merkt.

$\frac{A_1,...,A_n,F \vdash G}{A_1,...,A_n \vdash (F \rightarrow G)}$

#### Implikationsbeseitigung ($-\rightarrow$)

Die Implikationsbeseitigung (lat.: *Modus Ponens*) besagt, dass wenn man zeigen
moechte, dass aus den Annahmen $A_1,...,A_n$ eine Formel $G$ folgt, dann genuegt
es zu zeigen, dass wenn aus den Annahmen eine andere Formel $F$ folgt, und dass
wenn die Annahmen wahr sind, und $F$ wahr ist, dann immer auch $G$ wahr ist:

$\frac{A_1,...,A_n \vdash F \,\, A_1,...,A_n \vdash F \rightarrow G}{A_1,...,A_n
\vdash G}$

Hierbei darf $G$ eine beliebige Formel sein.

#### Negationseinfuehrung ($+ \neg$)

Um zu zeigen, dass sich aus einer Menge von Annahmen $A_1,...,A_n$ die Negation
$\neg F$ einer Formel $F$ ableiten laesst, genuegt es zu zeigen, dass wenn man
$F$ (*nicht negiert!*) mit in die Annahmen nimmt, aus den Annahmen immer ein
Widerspruch folgt. Der Widerspruch ist hierbei wieder $false$ auf der rechten
Seite.

#### Negationsbeseitigung $(-\neg)$

Um zu zeigen, das aus den Annahmen $A_1,...,A_n$ eine Formel $F$ folgt, reicht
es, zu zeigen, dass wenn man $\neg F$ mit in die Annahmen nimmt, sofort ein
Widerspruch folgt ($\neg F$).

Intuitiv: "Wenn die Annahmen wahr sind, dann ist $F$ wahr. Wenn also die
Annahmen alle wahr sind, und $\neg F$ auch, dann ist das ein Widerspruch, weil
aus den Annahmen immer $F$ folgt".

Note: Diese Regel ist exakt dieselbe wie die Negationseinfuehrung, aber hier ist
$F$ nicht negiert, und wird negiert in die Annahmen genommen, und oben
umgekehrt.
