# Kombinatorik

## Ziehungen

Eine einfache Anwendung der Kombinatorik ist das Ziehen von Elementen aus einer
Menge. Hierbei betrachten wir die Faelle, dass die Elemente unterschiedlich
sind, oder nicht, sowie die Faelle, dass ein Element mehrmals, oder nur einmal
gezogen werden darf.

Sei aber nun $\Sigma = \{x_1, x_2, ..., x_n\}$ ein $n$-elementiges Alphabet. Wir
interessieren uns fuer die Moeglchkeiten $k$ Elemente aus diesem Alphabet zu
ziehen.

### Mit Reihenfolge

#### Mit Reihenfolge, mit Zuruecklegen

Da man fuer jeden der $k$ Plaetze $n$ Moeglichkeiten, weil wir jedes Element
wieder zuruecklegen, und da jede Reihenfolge beruecksichtigt wird (*mit
Reihenfolge*), gibt es

__$n^k$__

solcher Ziehungen. Man kann also $n^k$ Woerter $w = a_1a_2...a_k \in \Sigma^k$
finden.

#### Mit Reihenfolge, ohne Zuruecklegen

Fuer den ersten der $k$ Plaetze gibt es $n$ Moeglichkeiten. Danach ist ein Platz
weniger frei, also gibt es nur mehr $n - 1$ Moeglichkeiten fuer den zweiten
Zug. Danach sind zwei Plaetze weniger frei, also gibt es nur mehr $n - 2$
Moeglichkeiten fuer den dritten Zug. Dies geht bis zum $k$-ten Zug, fuer welchen
es also nur mehr $n - k$ Moeglichkeiten gibt

Fuer den ersten der $k$ Plaetze gibt es $n$ Moeglichkeiten. Da wir die Elementen
icht wieder in das Alphabet zuruecklegen, haben wir fuer den zweiten der $k$
Plaetze nur mehr $n - 1$ Moeglichkeiten. Fuer den dritten Platz, dann nur mehr
$n - 2$ Moeglichkeiten. Da $n = n - 0$, geht das bis zu $n - (k - 1) = n - k +
1$ weiter. Nach der Produktregel der Kombinatorik folgt die Formel:

$\prod_{i=0}^{k - 1} (n - i) = n \cdot (n - 1) \cdot ... \cdot (n - k + 1) =
n^{\underline{k}}$

Man kann naemlich $n^{\underline{k}}$ Woerter $w = a_1a_2 ... a_k \in \Sigma^k$
wahlen, wobei fuer alle $a_i, a_j$ gilt $a_i \neq a_j$ (ohne doppelte
Zeichen). Das $i$-te Zeichen kann also aus dem Alphabet $\Sigma$, ohne allen
$i-1$ Zeichen, die vorher schon gezogen wurden, gezogen werden: also $\Sigma
\setminus \{a_1, a_2, ..., a_{i-1}\}$. Im Uebrigen kann man den Ausdruck oben
auch so schreiben, dass man von $n!$ die unteren $(n - k)!$ Zahlen wegdividiert:

$n^{\underline{k}} = \frac{n!}{(n - k)!} = \frac{n^{\underline{k}} \cdot (n -
k)!}{(n - k)!}$

Diese Anzahl nennt man die Variationen von $k$ aus $n$.

Die Fakultaet $n!$ einer Zahl $n$ ist ein Spezialfall der Variationen, wo $k =
n$. So kann man sich auch merken, dass die Reihe von $0$ bis $k - 1$ geht und
nicht bis $k$, denn dann stuende am Ende des Produkts $\cdot (n - n)$. Es sind
ja auch $k$ Zahlen da man bei $0$ anfaengt zu zaehlen.

### Ohne Reihenfolge

Wenn es keine Reihenfolge gibt, dann kommutieren die Buchstaben jeden Wortes:
$x_ix_j = x_jx_i$. So wird ein Wort der Laenge $k$ zu einem Monom vom Grad
$k$. Ein Monom kann grundsaetzlich als ein Wort ohne Reihenfolge gesehen
werden. Formal ist ein Monom eine Produktserie von Buchstaben/Variablen $x_1,
x_2, ..., x_n$, welche ausgedrueckt wird als:

$\prod_{i=1}^n x_i^{e_i}$

Wobei der *Exponent* $e_i$ des $i$-ten Buchstaben angibt, wie oft dieser
Buchstabe im Ausdruck vorkommt (einfach normale Potenz), sodass:

$\sum_{i=1}^n d_i = k$

Also expandiert man die Buchstaben (setzt man $d_i$ immer gleich 1), so hat man
$k$ Buchstaben. Die Schreibweise mit dem Exponenten ist also eine
"Komprimierung". So gilt nun:

$x_1x_2x_3x_2x_1 = x_1^2x_2^2x_3$

Alle anderen Buchstaben, z.B. $x_4$ koennen hier auch den Exponenten $0$
erhalten (kommt 0 mal vor), da $a^0 = 1 \forall a$.

Allgemein bedeutet keine Reihenfolge, dass man die Moeglichkeiten mit
Reihenfolge nimmt, und dann die Permutationen los wird. Da Moeglichkeiten immer
ein Produkt sind, bedeutet "los werden" dividieren.

#### Ohne Reihenfolge, ohne Zuruecklegen

Die Anzahl an Moeglichkeiten $k$ aus $n$ Elementen auszuwaehlen, *mit
Reihenfolge* und ohne Zuruecklegen ist wie gezeigt $n^{\underline{k}} = n \cdot
(n - 1) \cdot ... \cdot (n - k + 1)$. Unter diesen Tupeln gibt es nun fuer alle
$k$ unterscheidbaren Buchstaben auch $k!$ Permutationen. Diese sind aber, *ohne
Reihenfolge*, aequivalent zu einander. Daher dividiert man einfach diese
Moeglichkeiten weg (Gegenstueck zur Produktregel der Kombinatork):

$\frac{n^{\underline{k}}}{k!}$

Dies ist der bekannte Binomialkoeffizient:

$\frac{n^{\underline{k}}}{k!} = \frac{n!}{k!(n - k)!} = {n \choose k}$

Dieser Wert ist auch die Anzahl an $k$-Teilmengen einer $n$-elementigen Menge.

#### Ohne Reihenfolge, mit Zuruecklegen

Die Anzahl an Moeglichkeiten, aus $n$ Elementen genau $k$ auszuwaehlen, wobei
aus den $n$ auch Elemente mehrmals unter den $k$ selektierten vorkommen koennen,
und wo die Reihenfolge aber wieder egal ist, wird angegeben durch den Ausdruck:

${k + (n - 1) \choose k} = {k + (n - 1) \choose n - 1}$

##### Beispiel

Anzahl $k$-elementiger Teilmultimengen uber $X$?

Zuerst: eine Multimenge ist eine Menge, in welcher Duplikate erlaubt sind aber
die Reihenfolge egal ist, also. Sie wird mit einem Subscript $M$
dargestellt. Also:

$\{1, 2, 2, 3\}_M = \{1, 2, 3, 2\}_M \neq \{1, 2, 3\}_M$

Ueberlegung fuer $k = 3$ aus $n = 5$ ohne Reihenfolge und mit Wiederholung: Man
sucht $k = 3$ viele Elemente. Nun stelle man sich die $k$ gewaehlten Elemente
als $k$ Striche '|' dar, zusammen: '|||'. Nun muss man diese $k$ Striche nur
noch durch $n - 1$ Kommata teilen, z.B.: '|,||,,,'. Dann hat man also vom ersten
Element eins (ein Strich), vom zweiten zwei (zwei Striche) und von den uebrigen
keine (keine Striche).

Insgesamt muss so ein Wort also $k$ Elemente und $n - 1$ Kommata enthalten, also
die Laenge $k + (n - 1)$ haben. Nun gilt es einfach, aus diesen $k + (n - 1)$
Positionen entweder $k$ auszuwaehlen, dann sind die anderen Positionen fuer die
Kommata bestimmt, oder die $n - 1$ Positionen fuer die Kommata zu waehlen, dann
sind die Positionen der Elemente (Striche) bestimmt. Man sucht also nur die
Moeglichkeiten, aus den $k + (n - 1)$ moeglichen Positionen $k$ auszuwaehlen.

Durch jede Moeglichkeit von $k$ Positionen sind die $n - 1$ automatisch
bestimmt. Also gibt es ${k + (n - 1) \choose k}$ solcher moeglichen Woerter, oder
wenn man die Kommata bestimmt anstelle der Elemente, dann ${(k + n - 1) \choose
n - 1}$.

Man kann sich alternativ auch vorstellen, zuerst die $n - 1$ Kommata zu
bestimmen, und die Regionen zwischen den Kommata dann mit $k$ Strichen
aufzufuellen.

##### Andere Anwendungen

Die oben angewandte Vorstellungs- bzw. Beweistechnik "wir reservieren $k + (n -
1)$ Plaetze" kann man auch fuer andere kombinatorische Aufgaben
anwenden. Oben wollten wir aus $n$ Elementen $k$ auswaehlen. Was aber, wenn wir
$n$ Elemente allesamt auf $k$ Plaetze aufteilen wollen?

Wieder koennen wir uns vorstellen, $n$ Striche zu haben. Nun aber sind die
Kommata da, um die $k$ Plaetze zu separieren, und nicht die Striche. Wir
brauchen also Platz in unserem Wort fuer $k - 1$ Kommata. Dann verteilen wir die
$n$ Striche einfach auf die $k$ Regionen, brauchen also insgesamt $n + (k - 1)$
Positionen, aus denen wir $n$ oder $k - 1$ auswaehlen muessen, um das Wort
eindeutig zu bestimmen:

${n + (k - 1) \choose n} = {n + (k - 1) \choose k - 1}$

Dieser Wert ist somit auch die Anzahl moeglicher Partitionen einer
$n$-elementigen Menge, in $k$ Multi-Teilmengen.

Frage: Wieviele Moeglichkeiten gibt es $e$ Euro auf $k$ Kinder zu verteilen?
Antwort: Man sucht die Anzahl moeglicher Partitionen der $e$ Euro in $k$
Multi-Teilmengen, also ${e + (k - 1) \choose e}$.

### Aufgaben

"Es seien $n, k \in \mathbb{N}$. Bestimmen Sie fuer jede der folgenden Mengen
ihre Maechtigkeit":

(a) $\{(s_1, ..., s_k) \in [n]^k \, | \, s_i \neq s_j \text{ fuer } 1 \leq i < j
\leq k\}$

Gesucht ist hier die Anzahl an Elementen mit Reihenfolge und ohne Wiederholung:
$n^{\overline{k}}$

(b) $\{(s_1, ..., s_k) \in [n]^k \, | \, s_i < s_2 < ... < s_k\}$

Gesucht ist hier die Anzahl an Elementen ohne Wiederholung ($<$ ist irreflexiv)
ohne Reihenfolge. Es ist ohne Reihenfolge, denn wenn die gesuchte Sequenz keine
Reihenfolge hat, dann ist sie automatisch sortiert (es gibt eben nur eine
Reihenfolge). Also ${n \choose k}$

(c) $\{(s_1, ..., s_k) \in [n]^k \, | \, s_1 \leq s_2 \leq ... \leq s_k\}$

Man kann hier schnell sehen, dass die Anzahl an Elementen mit Wiederholung und
ohne Reihenfolge gesucht ist: ${k + n - 1 \choose k}$. Wenn man aber an die
Analogie mit den Strichen und Kommata denkt: Man hat $n$ Elemente, braucht also
$n - 1$ Kommata fuer $n$ Abschnitte in seinem Wort. Dann legt man in diese $n$
Abschnitte noch $k$ Striche, wo die Anzahl Striche in jedem Abschnitt angibt,
wie oft dieses Element gewaehlt wird.

So ein Wort braeuchte also Platz fuer $n - 1$ Kommata und $k$ Striche. Aus
diesen $k + (n - 1)$ Plaetzen muss man dann entweder die Plaetze fuer die $n -
1$ Kommata, oder fuer die $k$ Striche waehlen:

${n + k - 1 \choose k} = {n + k - 1 \choose n - 1}$

(d) $\{(s_1, ..., s_k) \in \mathbb{N}_0^k \, | \, s_1 + s_2 + ... + s_3 = n\}$

Gesucht ist die Anzahl an moeglichen geordneten $k$-Zahlpartition der Zahl $n$,
also der Anzahl an $k$-Partitionen der $n$-elementigen Multimenge von Einsen,
wobei jede der $k$-Multi-Teilmengen leer oder nichtleer sein kann. Weil jede
Teil-Multimenge auch leer sein kann, ist es eben nicht die Anzahl an geordneten
Zahlpartitionen ${n - 1 \choose k - 1}$. Besser, stellt man es sich wieder mit
Strichen und Kommata vor: Gebraucht sind $n$ Striche, wobei ein Strich fuer eine
$1$ steht, welche also jedenfalls im Wort sein muessen. Nun gilt es einfach,
diese $n$ Striche durch $k - 1$ Kommata zu teilen, fuer die $k$ Zahlen. Z.B.:
"||,||,|," steht fuer $2 + 2 + 1$ wenn $n = 5, k = 3$. Insgesamt hat ein solches
Wort also $n + k - 1$ Positionen. Aus diesen muss man nur wieder entweder die
$n$ Positionen fuer die Striche oder die $k - 1$ Positionen fuer die Kommata
waehlen. Aus dem einen ergibt sich jeweils das andere. Insgesamt: ${n + (k - 1)
\choose k - 1} = {n + (k - 1) \choose n}$

(e) $\{(s_1, ..., s_k) \in \mathbb{N}_0^k \, | \, s_1 + s_2 + ... + s_3 \leq
n\}$

Eine Moeglichkeit ist es, die Loesung aus (d) zu uebernehmen und sie fuer alle
$k \in [n]$ anzuwenden: $\sum_{i=1}^n {i + (k - 1) \choose i}$.

Eine andere Moeglichkeit: Zu den $n$ Strichen und $k - 1$ Kommata gibt man nun
noch eine moegliche Position hinzu: fuer einen Strichpunkt, links von welchem
alles fuer das jeweilige Resultat relevant ist, rechts von welchem man einfach
alles ignoriert. Wenn man jetzt die $n + k - 1$ Positionen mit einer moeglichen
Belegung von Strichen und Kommata belegt hat, gibt es $n + k - 1 + 1$ Positonen
fuer den Strichpunkt. Die Position ganz links wird nicht under Betracht
genommen, da das Resultat dann $0$ waere, aber $n \in \mathbb{N}$. Da der
Strichpunkt von ganz links bis ganz rechts "kombinatorisch wandert", wird die
Belegung fuer jedes $i \in [n]$ einmal unter Betracht gezogen. Z.B. fuer $n=5,
k=3$ (also $\leq 5$), ohne Strichpunkt: $|;|,||,| \Rightarrow n=1,
||;,||,| \Rightarrow n=2 ...$. Also:

${n + k - 1 + 1 \choose n} = {n + k \choose n} = {n + k \choose k}$.

Wir zaehlen hier den Strichpunkt zu den $k$
Kommata, um die $n$ Strichpositionen genau zu bestimmen. Die

(f) $\{(s_1, ..., s_k) \in \mathbb{N}^k \, | \, s_1 + s_2 + ... + s_3 = n\}$

Hier duerfen die $k$-Teil-Multimengen nicht mehr leer sein, also kann man wieder
mit den Zahlpartitionen arbeiten. Da $(s_1, ...., s_k)$ Tupel sind, suchen wir
geordnete $k$-Zahlpartionen von $n$: ${n - 1 \choose k - 1}$. Wir reihen $n$ als
$n$ Einsen auf. Diese sind durch $n - 1$ Plusse getrennt. Aus diesen suchen wir
$k - 1$ Plusse aus, und wandeln sie in Kommata um, um die $k$ Teilmengen zu
bestimmen.

Oder wieder mit Strichen und Kommata: die Teil-Multimengen duerfen nicht leer
sein, also nehmen wir aus den $n$ Strichen schon $k$ heraus, um spaeter jeder
der $k$ Klassen einen Strich zu geben. Nun muessen wir nur mehr die $n - k$
verbleibenden Striche durch $k - 1$ Kommata teilen. Das Wort hat also die Laenge
$(n - k) + (k - 1) = n - 1$, aus welchem wir eben die $k - 1$ Kommata oder $n -
k$ Striche waehlen.

(g) $\{(s_1, ..., s_k) \in \mathbb{N}^k \, | \, s_1 + s_2 + ... + s_3 \leq n\}$

Wir fuegen wieder den Strichpunkt hinzu. Nun gibt es also ${n - k + k - 1 + 1
\choose k} = {n \choose k}$ Moeglichkeiten.

## Beweisprinzipien

### Produktregel

Die Produktregel der Kombinatorik besagt, dass die Anzahl an Moeglichkeiten in
einem Experiment mit $k$ Stufen, gleich dem Produkt der Moeglichkeiten auf jeder
einzelnen Stufe ist. Das laesst sich schon an den fallenden Faktoriellen sehen:
"fuer das erste Element gibt es $n$ Moeglichkeiten, fuer das zweite $n - 1$,
also $n \cdot (n - 1)$". Fuer jede der $n$ Moeglichkeiten in der ersten Stufe
gibt es wiederum $n - 1$ Moeglichkeiten, deswegen $n \cdot (n - 1)$. Man sagt:
"So viele Moeglichkeiten, __und__ dann so viele, __und__ dann so viele ...".

Auf Mengen bezogen: Die Kardinalitaet des Kreuzproduktes von Mengen ist gleich
dem Produkt der Kardinalitaeten der einzelnen Mengen:

$|\times_{i=1}^n M_i| = \prod_{i=1}^n |M_i|$.

Frage: Wieviele verschiedene Anagramme gibt es fuer das Wort "PFEFFER"?  Ansatz
1: Das Alphabet ist hier also $\sigma = \{P, F, E, R\}$. Fuer den ersten
Buchstaben gibt es noch $7$ Plaetze. Fuer den zweiten Buchstaben $F$ braucht man
3 Plaetze aus den verbleibenden ("verschiedene Anagramma" -> selbe Buchstaben
duerfen nicht permutieren), also ${6 \choose 3}$. Fuer $E$ muss man zwei weitere
Plaetze aus den verbleibenden $3$ reservieren, also ${3 \choose 2}$. Fuer den
letzen Buchstaben bleibt nur mehr ein Platz. Insgesamt also, mittels
Produktregel: ${7 \choose 1} \cdot {6 \choose 3} \cdot {3 \choose 2} \cdot {1
\choose 1} = 420$ Moeglichkeiten.

Ansatz 2 (ohne Produktregel): Es gibt $7$ Buchstaben, lasse sie permutieren,
also $7!$ Moeglichkeiten. Dividiere die Permutationen von $F$ weg, also
$/3!$. Dividiere die Permutationen von $E$ weg, also $/2!$. Dann bleibt:
$\frac{7!}{3! \cdot 2!} = 420$.

### Summenregel

Hat man fuer ein Experiment mehrere mögliche Teilexperimente (die sich nicht
ueberschneiden) zur Wahl, dann addiert man die Anzahl an möglichen Ausgaengen
aller einzelnen Teilexperimente. Man sagt: "Entweder so viele Moeglichkeiten,
__oder__ so viele, __oder__ so viele ...".

Auf Mengen bezongen: Die Kardinalitaet der *disjunkten Vereinigung* von Mengen
ist gleich der Summe ihrer Kardinalitaeten: $|\biguplus_{i=1}^n M_i| =
\sum_{i=1}^n |M_i|$.

Frage: auf wieviele Weisen kann man $6$ Maedchen und $8$ Jungen auf $5$ Stuehle
       abwechselnd verteilen?

Antwort: Entweder 3 Maedchen sitzen auf den ungeraden Plaetzen $1, 3, 5$ und 2
         Jungen auf den geraden Plaetzen $2, 4$, oder umgekehrt.

1. Fall: Es gibt $6$ moegliche Maedchen fuer Platz $1$, dann $5$ verbleibende
   fuer Platz $3$, dann $4$ fuer Platz $5$. Dann gibt es $8$ moegliche Jungen
   fuer Platz $2$ und $7$ verbleibende Jungen fuer Platz $4$. Insgesamgt gibt es
   also $6 \cdot 5 \cdot 4 \cdot 8 \cdot 7$ Moeglichkeiten.

2. Hier umgekehrt, also: $8 \cdot 7 \cdot 6 \cdot 6 \cdot 5$.

Geht nur wenn die Mengen disjunkt sind! Ansonsten benoetigt man das Prinzip der
Inklusion/Exklusion bzw. die "Siebformel".

### Gleichheitsformel

Die Gleichheitsformel besagt einfach, dass wenn es fuer die Elemente einer Menge
eine Bijektion auf die Elemente einer anderen Menge gibt, dann ist die Anzahl an
Elementen der beiden Mengen gleich.

#### Prinzip des Doppelten Abzaehlens

Das Prinzip des doppelten Abzaehlens besagt, dass die Summe aller Zeilen in
einer Matrix gleich der Summe aller Spalten ist. Oftmals kann es also leichter
sein, die Reihensumme zu zaehlen als die Spaltensumme, oder umgekehrt. Wegen dem
Prinzip des doppelten Abzaehlens ist beides identisch.

Frage: In einem Tanzkurs gibt es $24$ Damen und $n$ Herren. Nach einer
Tanzstunde hat jede Dame mit genau $8$ Herren und jeder Herr mit genau $6$ Damen
getanzt. Wieviele Herren gibt es?  Antwort: Man stelle sich eine $n \cdot 24$
Matrix von $\text{Damen } \times \text{ Herren}$ vor. Die Spaltensumme ist
eindeutig $24 \cdot 8$ (es gab also $192$ Taenze). Da wegen dem Prinzip des
doppelten Abzaehlens die Spaltensumme gleich der Zeilensumme ist, ist die
Zeilensumme auch $192$. Nun dividiert man durch die Anzahl an Damen in jeder
Zeile und erhaelt die Anzahl an Herren/Zeilen: $\frac{192}{6} = 32$.

Oder einfach: $n \cdot 6 = 24 \cdot 8 \Rightarrow n = \frac{24 \cdot 8}{6}$

### Prinzip der Inklusion und Exklusion

Das Prinzip der Inklusion und Exklusion erlaubt es, die Kardinalitaet einer
nicht disjunkten Vereinigung von Mengen zu beschreiben. Mit der Summenregel
durfte man ja nur die Kardinalitaet von disjunkten Mengen beschreiben. Bei
dieser Variante berechnet man auch die Summe der Kardinalitaeten, aber zieht die
doppelt bzw. $n$-fach gezaehlten Elemente wieder ab.

Seien $A$ und $B$ zwei beliebige Mengen, dann sagt das Prinzp der Inklusion und
Exklusion, dass: $|A \cup B| = |A| + |B| - |A \cap B|$. Sei $C$ der Schnitt von
$A$ und $B$. Zaehlt man jetzt $|A| + |B|$, dann zaehlt man $|C|$ einmal in $|A|$
und *noch einmal* in $|B|$. Deswegen zieht man $|C|$ einfach noch einmal ab, um
das zweite Zaehlen ruckgaengig zu machen.

Allgemein ist also die Vereinigung $\bigcup_{i=1}^n M_i$ von $n$ Mengen gleich:

* Der Summe aller Kardinalitaeten

* *Minus* der Kardinalitaeten der Schnitte von zweien (minus der Paare)

* *Plus* der Kardinalitaeten der Schnitte von dreien (plus den Tripeln)

* *Minus* der Schnitte von vieren

* *Plus* ...

* *Minus* ...

* usw.

Fuer drei Mengen: $|A \cup B \cup C|$

1. $+ |A| + |B| + |C|$ (Einzelne)
2. $- |A \cap B| - |A \cap C| - |B \cap C|$ (Paare)
3. $+ |A \cap B \cap C|$ (Tripel)

Fuer vier Mengen: $|A \cup B \cup C \cup D|$

1. $+ |A| + |B| + |C| + |D|$ (Einzelne)
2. $- |A \cap B| - |A \cap C| - |A \cap D| + |B \cap C| + |B \cap D| + |C \cap
D|$ (Paare)
3. $+ |A \cap B \cap C| + |A \cap B \cap D| + |A \cap C \cap D| + |B \cap C \cap
D| + |A \cap B \cap D|$ (Tripel)
4. $- |A \cap B \cap C \cap D|$ (Quadrupel)

Also allgemein:

$$\left|\bigcup_{i=1}^n M_i\right| =
\sum_{i=0}^{n-1}(-1)^i\sum_{j=0}^{n/(i+1)-i}\bigcap_{k=j\cdot
i}^{[(j + 1) \cdot i] - 1} M_k$$

#### Beispiel

Frage: Wieviele durch $7$ oder $11$ teilbare natuerliche Zahlen $\leq 1000$ gibt
       es?

Antwort: Die Anzahl der Zahlen, die durch $7$ teilbar sind, plus die Anzahl an
         Zahlen, die durch $11$ teilbar sind, minus der Zahlen, die man hierbei
         zwei mal gezaehlt hat, also welche durch $7$ und $11$, somit $7 \cdot
         11 = 77$ teilbar sind. Daher: $\lfloor \frac{1000}{7} \rfloor + \lfloor
         \frac{1000}{11}\rfloor - \lfloor \frac{1000}{7 \cdot 11}\rfloor$.

__MAN SUCHT HIER DAS KGV DER ZAHLEN!!!!!!!__

Frage: Wieviele durch $2, 3, 5$ teilbare natuerliche Zahlen $\leq 1000$ gibt es?
Antwort: $\lfloor \frac{1000}{2}\rfloor + \lfloor \frac{1000}{3}\rfloor +
\lfloor \frac{1000}{5}\rfloor - \lfloor \frac{1000}{2 \cdot 3}\rfloor - \lfloor
\frac{1000}{2 \cdot 5}\rfloor - \lfloor \frac{1000}{3 \cdot 5}\rfloor + \lfloor
\frac{1000}{2 \cdot 3 \cdot 5}\rfloor$

Frage: Ein Kellner muss $3$ Essen zu $3$ Tischen bringen, hat aber vergessen,
welches Essen wohin kommt. Bei wievielen der $3! = 6$ moeglichen Verteilungen
kriegt keiner der Gaeste sein richtiges Essen?

Sei $A_i$ die Menge der Verteilungen, wo der $i$-te Tisch sein Essen richtig
bekommt. Dann ist $A_1 \cup A_2 \cup A_3$ die Menge aller Verteilungen, bei der
mindestens ein Tisch richtig ist. Bei $A_1$ gibt es aber auch Verteilungen, wo
auch der zweite Tisch das richtige Essen bekommt. Und bei $A_2$ gibt es
Verteilungen, wo auch der erste Tisch sein Essen bekommt. Man hat den Fall, dass
beide Essen bekommen, also zwei mal gezaehlt (einmal in $A_1$ und ein zweites
mal in $A_2$). Selbes gilt fuer $A_1$ mit Tisch 3 und noch allen anderen, wenn
es noch mehr gaebe. Man muss also die Paare von Verteilungsmengen abziehen, also
$- |A_1 \cap A_2|, |A_1 \cap A_3|, |A_2 \cap A_3|$. Wenn man $|A_1 \cap A_2|$
und $|A_1 \cap A_3|$ und $|A_2 \cap A_3|$ abzieht, hat man aber auch die Faelle
abgezogen, wo alle drei Essen bekommen. Also muessen diese wieder rein:

$$|A_1 \cup A_2 \cup A_3| = |A_1| + |A_2| + |A_3| - |A_1 \cap A_2| - |A_1 \cap
A_3| - |A_2 \cap A_3| + |A_1 \cap A_2 \cap A_3|$$

#### Klausuraufgabe

Der ASCII-Code enthaelt exkl. den Leerzeichen $94$ druckbare Zeichen, darunter die
$10$ Ziffern $Z = \{0, ..., 9\}$, die $52$ Buchstaben $B = \{a, ... , z, A, ...,
Z\}$ und die verbleibenden $32$ druckbaren Sonderzeichen $S = \{+, !, ... ,
∼\}$. Sei $\Sigma = Z \cup B \cup S$.

Wie viele Woerter $\omega \in \Sigma^7$ der Laenge $7$ gibt es, die mindestens
ein Zeichen aus jeder der drei Mengen Z, B, S enthalten?

Antwort: Generell sollte man hier nach der Anzahl an Woertern suchen, die kein
einziges der Zeichen enthalten (der "Gegenmenge"), und diese von der Grundmenge
abziehen. Also Sei $\overline{X}$ die Menge aller Woerter, die kein Zeichen aus
$X \in \{Z, B, S\}$ enthelt. Dann kann man hier das Prinzip der
Exklusion/Inklusion anwenden. Man zieht von der Grundmenge $\Sigma^7$ alle
Woerter $\in \overline{Z}, \overline{B}, \overline{S}$ ab. Dann hat man jedoch
die Woerter in $\overline{X}$ zu oft abgezogen, die in auch Woerter enthalten
die $\in$ einer anderen der Gegenmengen sind. Also muss man die Schnitte jeweils
zweier Mengen abziehen. Weil es kein Wort mit Laenge $7$ gibt, dass keinen
Buchstaben keiner der Mengen enthaelt, muss man den Fall $Z \cap B \cap S$ nicht
betrachten. Nun die Zahlen:

* $|\Sigma^7| = 94^7$ (7 mal alle moeglichen 94 Zeichen moeglich).

* $|\Sigma^7\setminus Z^7| = 84^7$

* $|\Sigma^7\setminus B^7| = 42^7$

* $|\Sigma^7\setminus S^7| = 62^7$

* $|\Sigma^7\setminus\{Z^7 \cap B^7\}| = 32^7$

* $|\Sigma^7\setminus\{Z^7 \cap S^7\}| = 52^7$

* $|\Sigma^7\setminus\{S^7 \cap B^7\}| = 10^7$

Also $94^7 - (84^7 + 42^7 + 62^7 - 32^7 - 52^7 - 10^7)$

### Schubfachprinzip

Das Schubfachprinzip besagt, dass wenn man $n$ Objekte auf $m$ Faecher verteilt
und es gibt mehr Objekte als Faecher ($n > m$), dann gibt es mindestens ein
Fach, dass mindestens zwei Elemente enthaelt.

Formal: Ist $f:X \rightarrow Y$ eine Abbildung und gilt $|X| > |Y|$, so gibt es
ein $y \in Y$ mit $|f^{-1}(y)| \geq 2$. Dann ist die Funktion also sicher nicht
mehr injektiv.

#### Allgemein

Allgemein, kann man sagen, dass wenn man $n$ Objekte auf $m$ Faecher verteilt,
es sicher ein Fach mit mindestens

$$\left\lceil\frac{n}{m}\right\rceil$$

Elementen gibt.

Formal: Ist $f: X \rightarrow Y$ eine Abbildung, so gibt es ein $y \in Y$ mit
$|f^{-1}(y)| \geq \left\lceil\frac{|X|}{|Y|}\right\rceil$.

Man muss hierbei aufrunden, weil der Rest der Division $|X|/|Y|$ Elemente sind,
die ja auch noch auf die Faecher verteilt werden muessen.

#### Beispiel

In jeder Menge von $13$ Personen gibt es zwei, die im selben Monat
Geburtstag haben. Es gibt bei so einem Beispiel nur zwei Faelle, die man sich im
Kopf vorstellen muss:

1. $12$ Personen haben in verschiedenen Monaten Geburtstag (sind maximal
   "verstreut"). Dann muss die $13.$ Person in einem Monat Geburtstag haben,
   indem auch eine andere Person Geburtstag hat.
2. $12$ Personen haben nicht alle in verschiedenen Monaten Geburtstag. Dann ist
   die Aussage auch schon bewiesen.

#### Beispiel

In jeder Menge aus $n + 1$ natuerlichen Zahlen gibt es zwei, die durch $n$
teilbar sind.

Beweis: Es gibt $n$ Restklassen fuer $n$, und $n + 1$ Zahlen. Also muessen zwei
Zahlen in der selben Restklasse liegen. Ihre Differenz ist dann durch $n$
teilbar.

#### Beispiel

Wenn es $N = 380$ Studierende gibt und das Jahr hat $52$ Wochen, dann
gibt es eine Woche in der mindestens $\lceil \frac{380}{52}\rceil =
\lceil7.31\rceil = 8$ Studierende Geburtstag haben.

#### Beispiel

Unter $6$ Personen gibt es entweder $3$ die sich alle untereinander kennen
oder $3$ die sich nicht alle untereinander kennen.

Beweis: Man nehme eine Person $P$ und 3 weitere Personen, die $A$ entweder alle
kennt, oder alle nicht kennt (solche drei muss es geben). Dann:

1. $A$ kennt alle. Dann gibt es entweder
	1. noch zwei der drei die sich kennen, dann gilt der Satz.
	2. keine zwei kennen sich, dann kennen sich dfrie drei nicht. Dann gilt der
       Satz.
2. $A$ kennt keinen. Dann gibt es entweder
	1. zwei der drei, die sich nicht kennen, dann gilt der Satz.
	2. keine zwei der drei kennen sich nicht, dann gilt der Satz.

#### Beispiel

Der Nikolaus hat ingesamt 16 verschiedene Geschenke, welche alle einen Wert
zwischen 1 und 4000 Euro haben. Zufaelligerweise haben keine zwei der 16
Geschenke denselben Wert.

Sein Problem: Er muss die Geschenke so auf zwei Kinder aufteilen, dass beide am
Ende Geschenke mit demselben Gesamtwert bekommen. Dabei muss jedes Kind
mindestens ein Geschenk bekommen, Geschenke duerfen nicht doppelt vergeben
werden, der Nikolaus darf aber auch einige Geschenke fuer sich behalten.

Zeigen Sie, dass der Nikolaus stets eine Loesung finden kann.

Beweis:

Es gibt mit Sicherheit *maximal* $16 \cdot 4000 = 64000$ verschiedene Summen,
die die $16$ Geschenke annehmen muessen, da die Werte der $16$ Geschenke alle
zwischen $1$ und $4000$ liegen ($64000$ selbst kann somit nicht erreicht
werden, also gibt es sogar weniger Summen, was uns aber bei diesem Beispiel auch
passt).

Wir betrachten nun die Menge von $16$ Geschenken. Es gibt $2^{16} - 1 = 65535$
nichtleere Teilmengen einer $16$-elementigen Menge. Diese Teilmengen werden
jeweils auf die Menge der moeglichen Summen abgebildet. Laut Schubfachprinzip
gibt es also mindestens ein "Fach", mit $\left\lceil65536/64000\right\rceil = 2$
Elementen. Daraus folgt die Aussage.

Man beachte, dass die zwei Teilmengen nicht immer disjunkt sind. Wenn der selbe
Summand aber in beiden Teilmengen enthalten ist, dann wird auch die Summe ohne
diesen Summanden gleich sein.

## Zaehlkoeffizienten

### Binomialkoeffizient

Der Binomialkoeffizient ${n \choose k}$ gibt die Anzahl an Moeglichkeiten an,
aus $n$ Elementen $k$ ohne Wiederholung und ohne Reihenfolge auszuwaehlen. Oder:
${n \choose k}$ ist die Anzahl $k$-elementiger Teilmengen einer $n$-elementigen
Menge.

${n \choose k} = \frac{n^\underline{k}}{k!} = \frac{n!}{k!(n-k)!}$

Eine interessante Eigenschaft des Binomialkoeffizienten ist, dass er
symmetrisch bezueglich $k$ ist. Es gilt immer:

${n \choose k} = {n \choose n - k}$

So ist ${5 \choose 2}$ beispielsweise dasselbe wie ${5 \choose 3}$.

### Pascal'sche Identitaet

Die Pascal'sche Identitaet reduziert den Binomialkoeffizienten ${n \choose k}$
auf kleinere $n,k$:

$${n \choose k} = {n - 1 \choose k - 1} + {n - 1 \choose
k}$$

Begruendung: Sei $a$ ein beliebiges Element $\in [n]$. Wenn man $k$ Zahlen aus
$[n]$ zieht, gibt es zwei Faelle:

1. $a$ wird gezogen, dann gibt es fuer die verbleibenden $k - 1$ Elemente genau
   $n - 1$ Moeglichkeiten. Also ${n - 1 \choose k - 1}$

2. $a$ wird nicht gezogen, dann gibt es fuer dei verbleibenden $n - 1$ Elemente
   noch immer $k$ Moeglichkeiten. Aslo ${n - 1 \choose k}$.

Es gibt also alle Moeglichkeiten aus (1) plus alle Moeglichkeiten aus (2): ${n -
1 \choose k} + {n - 1 \choose k - 1}$.

#### Binomische Formel

Eine wichtige Anwendung des Binomialkoeffizienten ist die Binomische Formel,
welche besagt, dass fuer beliebige $n \in \mathbb{N}_0$ und beliebige $a, b$
gilt:

$$(a + b)^n = \sum_{k=0}^n {n \choose k} a^k b^{n - k}$$

Die binomische Formel ist besonders hilfreich, um Aufagben zu loesen wie:

"Finde den Koeffizenten von $x^{12}y^{13}$ in der Entwicklung von $(2x -
3y)^{25}$".

Hierbei formt man den Ausdruck nach obiger Formel in eine Summe um:

$(2x - 3y)^{25} = \sum_{k=0}^n{n \choose k}(2x)^k(-3y)^{n - k}$.

Gesucht ist $k = 12$ als Exponent fuer $x$, also entstehen die Koeffizienten
${25 \choose 12} \cdot 2^{12} \cdot (-3)^{25 - 12} = -3.395972 \cdot 10^{16}$.

##### Beispiel

Finde den Koeffizienten von $x^3y^5z^4$ in $(xyz + y + z)^6$.

Wenn wie hier mehr als $2$ Variablen im Ausdruck stehen, dann muss man zwei
einfach einklammern und zusammen als Variable sehen. Also ist in $(x + y + z)$
einfach $a = x$ und $b = (y + z)$, und dann gilt $\sum_{k=0}^n{n \choose
k}x^k(y + z)^{n - k}$.

Sei $a := xyz, b := y + z$, dann gilt:

$(a + b)^6 = \sum_{k=0}^6{6 \choose k}(xyz)^k(y + z)^{6 - k}$.

Weil $x^3$ muss, waehlen wir $k = 3$, und setzen ein:

${6 \choose 3}(xyz)^3(x + y)^{6 - 3}$

Mit $(x + y)^{6 - 3} = (x + y)^3$ haben wir wieder einen Ausdruck der Form,
welche wir in eine Summe umformen koennen:

$\sum_{k=0}^3{3 \choose k}y^k z^{3 - k}$.

Weil wir $y^5$ suchen, und aus dem ersten Schritt $y$ schon den Expontenten $3$
bekommen hat, muss hier $k = 2$ sein, damit der Exponent von $y$ gleich $5$
wird. Der Exponent von $z$ wird folglich $3 + (3 - 2) = 4$. Dann ergibt sich:

${3 \choose 2} y^2 z^{3 - 2}$

Diesen Ausdruck setzen wir oben fuer $(x + y)^3$ ein, und erhalten:

${6 \choose 3} (xyz)^3 \cdot ({3 \choose 2} y^2 z)$

Weil die Multiplikation kommutativ ist, folgt fuer den Koeffizienten fuer
$x^3y^5z^4$:

${6 \choose 3}{3 \choose 2} = 60$.

Generell koennte man fuer drei Variablen folgendes aus der binomischen Formel
schliessen: $x^ay^bz^d$ in der Entwicklung von $(\alpha x + \beta y + \gamma
z)^n$ ist gleich ${n \choose a} \alpha^a \cdot {n - a \choose b} \beta^{b}
\gamma^{n - a - b}$.

##### Summe der Binomialkoeffizienten

Ein Beweis, dass die Summe aller Binomialkoeffizienten fuer $n$, also somit die
Zeilensumme im Pascal'schen Dreieck, gleich $2^n$ ist, laesst sich auch mit der
Binomischen Formel durchfuehren:

$$\sum_{k=0}^n {n \choose k} = 2^n$$

Intutitiv kann man es sich vorstellen, dass $\sum_{k=0}^n {n \choose k}$ die
Summe aller Moeglichkeiten ist, ein Element auszuwaehlen ($1$ Bit), dann zwei
auszuwaehlen ($2$ Bits), dann drei ($3$ Bits) usw. Letztendlich kann jedes
Element in jeder dieser Teilmengen von $[n]$ enthalten sein oder nicht, also wie
bei einem Binaercode.

Anderer Beweis: $2^n = (1 + 1)^n = \sum_{k=0}^n{n \choose k}1^n 1^{n - k} =
\sum_{k=0}^n{n \choose k}$.

### Vandermonde-Identitaet

Wenn man eine Menge in zwei Mengen mit den Kardinalitaeten $n$ und $m$ teilen
kann, und daraus $k$ Elemente waehlen will, dann hat man ${n + m \choose k}$
Moeglichkeiten. Ebenso kann man sich aber denken, dass wenn man $k$ aus $n + m$
waehlen will, dann ist das aequivalent dazu, ein Element aus den $n$ zu waehlen,
und $k - 1$ aus den $m$, plus ("oder") die Moeglichkeiten, $2$ aus den $n$ zu
waehlen und $k - 2$ aus den $m$ etc. Daher:

$${n + m \choose k} = \sum_{i=0}^k {n \choose i} {m \choose k - i}$$

### Stirlingzahlen zweiter Art

Die Stirlingzahlen zweiter Art $\left\{{n \atop k}\right\}$ zweiter Art geben die
Anzahl an $k$-Partitionen (also $k$ nichtleere Klassen) einer $n$-elementigen
Menge an. Beispielsweise ist $\left\{{4 \atop 2}\right\} = 7$, weil man
$[4] = \{1, 2, 3, 4\}$ genau auf $7$ Weisen $2$-partitionieren kann,
z.B. $\{\{1\}, \{2, 3, 4\}\}$ oder $\{\{1, 2\}, \{3,
4\}\}$. $\left\{{n \atop k}\right\}$ wird auch kurz als $S_{n,k}$
angeschrieben. Man kann einige einfache Regeln aufstellen:

1. $S_{n,0} = 0$: Es gibt keine Moeglichkeit, $n$ Zahlen in $0$ nichtleere
   Klassen zu teilen.

2. $S_{n,1} = 1$: Es gibt genau eine Moeglichkeit, $n$ Zahlen in eine Klasse zu
   teilen, naemlich indem man die Menge garnicht teilt.

3. $S_{0,0} = 1$: Es gibt eine Moeglichkeit, $0$ Zahlen in $0$ Klassen zu
   teilen. Liegt irgendwo zwischen 1. und 2. Kann es auch als Definitionssache
   sehen, wie $0! = 1$.

4. $S_{n,n} = 1$: Es gibt $n$ Moeglichkeiten, $n$ Zahlen in $n$ Klassen zu
   teilen. Jede Klassen enthaelt einfach ein Element.

5. $S_{n,n-1} = {n \choose 2}$: Alle Moeglichkeiten, aus den $n$ zwei
   auszuwaehlen. Alle anderen Zahlen kommen in ihre eigene Klasse.

6. $S_{n,2} = \frac{2^n - 2}{2} = 2^{n - 1} - 1$: Man sehe die Elemente der
   Menge in einem Binaercode. Dann gibt es bekanntlich $2^n$ moegliche
   Bitstrings. Nun gibt es unter diesen aber noch die Moeglichkeiten, dass eine
   der Menge leer ist, also wenn man nur 1-en oder nur 0-en hat. Aber die
   Klassen muessen nicht leer sein, ergo $2^n - 2$. Nun haben wir alle Paare von
   Halbierungen. Jedoch gibt es hier noch den Fall, dass die selben $n/2$
   Elemente einmal in der einen Menge, und einmal in der anderen Menge sind. Die
   Partition hat aber keine Ordnung, also muss man die $2^n - 2$
   Moeglichkeiten noch durch $2$ teilen.

#### Beispiel

Wieviele Funktionen $f: A \rightarrow B$ gibt es, wo $|A| = 10$, $|B| = 11$ mit
$|f(A)| = 9$?

Antwort: Es gibt $11^{\underline{9}}$ Moeglichkeiten, aus den $B$ moeglichen
         Funktionswerten $9$ auszuwaehlen. Dann gibt es fuer jede dieser
         Moglichkeiten $S_{10,9}$ weitere Moeglichkeiten, die $10$ Elemente auf
         diese Funktionswerte zu partitionieren, also $S_{10,9} \cdot 11!/2!$

#### Berechnung

$S_{n,k}$ beschreibt nun also die Anzahl Moeglichkeiten, eine $n$-elementige
Menge in $k$ Klassen zu teilen. Aber wie berechnet man $S_{n,k}$?

Im ueblichen Fall:

$S_{n,k} = S_{n-1,k-1} + k \cdot S_{n-1,k}$ oder $\left\{{n \atop k}\right\} =
\left\{{n-1 \atop k-1}\right\} + k \cdot \left\{{n-1 \atop k}\right\}$

Genauer:

$$S_{n,k} = \begin{cases}1 \text{, wenn } n=0,k=0 \\ 0 \text{, wenn } n>0,k=0 \\
0 \text{, wenn } k>0,k>n \\ S_{n-1,k-1} + k \cdot S_{n-1,k} \text{,
sonst.}\end{cases}$$

Wie fuer die Pascal'sche Identitaet nehme man sich wieder ein Element aus der
Menge. Nun gibt es fuer dieses den Fall

1. dass es alleine in einer Klasse ist. Dann gibt es fuer die anderen $n - 1$
   Elemente nur mehr $k - 1$ Klassen.

2. dass es nicht alleine in einer Klasse ist. Dann muss man die verbleibenden
   $n - 1$ Elemente noch immer auf $k$ Klassen teilen. Da es dann $k$ moegliche
   Klassen gibt, in welches man das eine Element dann reinlegt, muss man noch
   den Faktor $k$ hinzufuegen.

Man kann die Stirlingzahlen zweiter Art in einem Pascal'schen Dreieck
aufzeichen, wenn man fuer den rechten Term (wie bei Pascal ist das Resultat
links oben plus rechts oben) noch die Spalte in der Pyramide, also den Faktor
$k$ mitnimmt.

#### Geordnete $k$-Partition einer $n$-elementigen Menge

Sollen die einzelnen Klassen unterscheidbar sein (z.B. unterscheidbare Urnen),
muss man die Moeglichkeiten noch mit den Permutationen von $k$ multiplizieren:

$k! \cdot S_{n,k}$

#### Andere Identitaet

Es gilt auch:

$$\left\{{n + 1} \atop {k + 1}\right\} = \sum_{i=k}^n {n \choose i} \left\{{i
\atop k}\right\}$$

Wir nehmen ein beliebiges Element aus den $n + 1$ und geben es in eine der $k +
1$ Klassen. Dann gibt es die Moeglichkeit, dass die anderen $n$ Elememente so
verteilt werden, dass in jeden der restlichen $k$ Klassen nur ein Element drin
ist, und die anderen $n - k$ in der Klasse vom besonderen Element. Dann gibt es
noch die Mogichkeit, dass $k + 1$ Elemente aus den $n$ anderen gewaehlt werden,
und auf die $k$ Klassen verteilt werden. Dafuer gibt es $S_{k+1,k}$
Moeglichkeiten. Die restlichen werden immer einfach in die Klasse des besonderen
Elements gewaehlt. So summiert man also ueber alle Moeglichkeiten, aus den
anderen $n$ Elementen zwischen $k$ und $n$ viele auszuwaehlen (daher der
Binomialkoeffizient), und diese dann auf $k$ Klassen zu *partitionieren* (daher
die Stirlingzahl).

### Permutationen

Eine Permutation $\pi: A \rightarrow A$ ist eine bijektive Abbildung von einer
Menge auf sich selbst. Beispielsweise ist $(b, c, a)$ eine Permutation von $(a,
b, c)$. Man kann eine Permutation auch in Matrixschreibweise angeben, wo die
Zahlen oben sind ($a_i$) und die Abbildungen ($\pi(a_i)$) unten:

$\pi = \left(\begin{matrix}1 2 3 4 5 \atop 4 5 2 1 3\end{matrix}\right)$ ist
eine Permutation, wo $1 \mapsto 4, 2 \mapsto 5$ etc.

Ein Zyklus der Laenge $l \in \mathbb{N}$ in einer Permutation von $n$
Elementen, ist ein $t$-Tupel fuer welches gilt, dass nach $l$ Permutationen der
$n$ Elemente, jedes der Elemente im Zyklus genau einmal auf jedes andere Element
im Zyklus abggebildet wurde, und dann nach der $l$-ten Permutation wieder auf
sich selbst.

Ein Zyklus ist also definiert ueber einer $t$-elementigen Teilmenge $\{a_1, ...,
a_l\} \subseteq A$ der Urmenge $A$, dessen Elemente paarweise verschieden sind
($a_i \neq a_j \, \forall i,j \in [t], i \neq j$) und fuer welche gilt, dass die
Position des Elements an der Stelle $i$ nach der naechsten Permutation, gleich
der Stelle des naechsten Elements ist, modulo der Laenge, also:

$\pi(a_i) = a_{i+1 \mod t} \forall i \in [t]$

oder $\pi((1, 2, 3)) = (2, 3, 1)$

Man kann einen Zyklus rotieren, ohne den Zyklus zu veraendern, aber nicht die
Reihenfolge der Elemente untereinander veraendern. Also

$$(1, 2, 3) = (2, 3, 1) = (3, 2, 1) \neq (2, 1, 3) = (3, 2, 1) = (1, 3, 2)$$

Die Zyklenschreibweise ist also nicht eindeutig. Man sollte sie wenn moeglich
aber sortieren, sodass das kleinste Element ganz links ist.

Man kann fuer einen Zyklus $\mathcal{Z}$ auch sagen, dass $\forall a \in
\mathbb{Z}$, mit $t = |\mathcal{Z}|$, jedes Element an jeder Stelle nach $t$
Permutationen wieder an der selben urspruenglichen Stelle stehen wird. Z.B. sei
$\mathcal{Z} = (1, 2, 3)$, dann gilt $\pi(\pi(\pi(1))) = 1$
bzw. $\pi(\pi(\pi(i))) = i$. Also allgemein:

$$\pi^t(x) = x$$

Elemente, die immer auf sich selbst abgebildet werden, heissen hierbei
*Fixpunkte* und sind also Zyklen der Laenge $1$.

Eine Permutation kann daher auch als Menge seiner Zyklen aufgeschrieben werden,
z.B.: $\pi = \{(1 5 2 8), (3), (4 6 7), (9), (10 11)\}$. Die Mengenklammern
koennen auch weggelassen werden, jedenfalls ist die Reihenfolge der Zyklen egal
und auch die Kommata in den Zyklen. Also $(1)(4 2)(3 6 5)=(6 5 3)(1)(4 2)$.

### Stirlingzahlen erster Art

Die Stirlingzahlen $\left[{n \atop k} \right]$ oder $s_{n,k}$ (kleines $s$)
erster Art geben die Anzahl an Permutationen von $n$ Elementen mit $k$ Zyklen
an. Einige einfache Definitionen:

* $s_{n,0} = 0$: Eine Permutation muss mindestens einen Zyklus haben.

* $s_{0,0} = 1$: Definition.

* $s_{n,1}=\frac{n!}{n} = (n - 1)!$: Es gibt $n!$ Permutation von $n$
  Elementen. Nun kann man eine bestimmte Permutation aber noch $n$ mal shiften,
  und jeder ist der selbe Zyklus. Also $\frac{n!}{n}$.

* $s_{n,n-1} = {n \choose 2}$: Es gibt ${n \choose 2}$ Moeglichkeiten, 2
  Elemente aus den $n$ zu waehlen, fur den ersten Zyklus. Weil bei Zyklen $(a b)
  = (b a)$, haben sie quasi keine Reihenfolge. Die restlichen Zyklen sind
  Fixpunkte der anderen $n - 2$ Elemente.

* $s_{n,n} = 1$: Nur Fixpunkte.

* $\sum_{k=1}^n s_{n,k} = n! = n \cdot s_{n,1}$.

#### Berechnung

Die Stirlingzahlen erster Art werden mit der folgenden Rekursionsformel
berechnet:

$$s_{n,k} = s_{n-1,k-1} + (n - 1) \cdot s_{n-1,k}$$

bzw. allgemeiner

$s_{n,k} = \begin{cases}0 \text{, wenn } k=0,n=0 \\ 1 \text{, wenn } k=0,n>0 \\
0 \text{, wenn } k>0,k>n \\ s_{n-1,k-1} + (n - 1) \cdot s_{n-1,k} \text{,
sonst.}\end{cases}$

Man nehme wieder ein beliebiges aber festes Element aus der Menge. Nun gibt es
die Faelle dass:

1. Das Element ein Fixpunkt ist, also alleine einen Zyklus bildet. Dann gibt es
   fuer die restlichen $n - 1$ Elemente $k - 1$ Zyklen zu fuellen. Es gibt also
   $s_{n-1,k-1}$ Moeglichkeiten fuer diesen Fall.

2. Das Element kein Fixpunkt ist. Nun gibt es also $n - 1$ Elemente, um $k$
   Zyklen zu fuellen. Anders also bei den Stirlingzahlen zweiter Art, kann man
   das Element aber nicht nur auf die $k$ Zyklen verteilen. Denn, man kann das
   Element ja nach jedem der $n-1$ Elemente in den $k$ Zyklen einfuegen, und
   erhaelt jedes mal eine voellig neue Zyklenmenge. Daher gibt es $(n - 1) \cdot
   s_{n-1,k}$ Moeglichkeiten fuer diesen Fall.

Man kann wieder eine Art Pascal'sche Pyramide fuer Stirlingzahlen erster Art
machen. Dazu muss man nun fuer den rechten Elternknoten den Faktor $n - 1$
mitnehmen, also die Nummer der letzen Zeile.

### Ungeordnete Zahlpartionen

Eine ungeordnete $k$-Zahlpartition $P_{n,k}$ einer natuerlichen Zahl $n \in
\mathbb{N}$ ist eine $k$-Multimenge $\{n_1, n_2, ..., n_k\}$ von positiven
natuerlichen Zahlen mit der Eigenschaft $n = n_1 + n_2 ... + n_k = \sum_{i=1}^k
n_i$. Anders gesagt: eine $k$-Zahlpartition ist eine $k$-Partition der
Multimenge $\{1, ..., 1\}$ mit $n$ Einsen, in $k$ nichtleere Klassen.

Z.B. hat die Zahl $5$ die Zahlpartitionen:

5. $\{1, 1, 1, 1, 1\}$
4. $\{2, 1, 1, 1\}$
3. $\{2, 2, 1\}$
2. $\{3, 1, 1\}$
1. $\{3, 2\}, \{4, 1\}$
0. $\{5\}$.

Wichtig hierbei ist, dass die Reihenfolge der Zahlen egal ist, also $\{2, 2, 1\}
= \{1, 2, 2\}$ da ja $2 + 2 + 1 = 1 + 2 + 2$.

"Bei $P_{n,k}$ sind die Elemente alle gleich (das heisst es spielt keine Rolle
welches wo landet) und die Reihenfolge der Elemente innerhalb einer Klasse ist
ebenfalls irrelevant." Also, weder die $n$ Elemente sind unterscheidbar, noch
die $k$ Klassen.

#### Berechnung

Die ungeordneten Zahlpartitionen $P_{n,k}$ lassen sich berechnen als:

$$P_{n,k} = \sum_{i=0}^k P_{n-k,i}$$

oder allgemeiner:

$P_{n,k} = \begin{cases}1 \text{, wenn }k=0,n=0  \\  0 \text{, wenn } k=0,n>0  \\  1
\text{, wenn } k>0,k>n  \\  \sum_{i=0}^k P_{n-k,i} \text{, sonst.}\end{cases}$

Die Bedingung gilt, dass jede der $k$ Klassen nicht leer sein darf. Also
entnehme man den $n$ Elementen doch schon mal $k$ Elemente und verteile jedes in
eine Klasse, damit diese Bedingung erfuellt ist. Nun gibt es noch alle
Moeglichkeiten, die verbleibenden $n - k$ Elemente auf $0$ bis $k$ Klassen zu
verteilen. Wobei man $k = 0$ nur fuer den Fall braucht, dass $n = 0$, dann ist
die Summe $1$, und die Formel funktioniert dann immer; fuer alle $n > 0$ ist
$P_{n,0}$ sowieso $0$.

Es gilt jedoch auch:

$$P_{n,k} = P_{n-k,k} + P_{n-1,k-1}$$

da gilt

$$P_{n-1,k-1} = \sum_{i=0}^{k-1} P_{(n-1) - (k-1), i} = \sum_{i=0}^{k-1}
P_{n-k,i}$$

und das eben die ersten $k - 1$ Terme der vorherigen Summe fuer $P_{n,k}$ sind.

Man kann sich denken: entweder, man will dass jede Klasse mindestens $2$
Elemente enthalten, dann braucht man $P_{n-k,k}$, denn $P_{n-k,k}$ bedeutet, es
werden zuerst $k$ Elemente auf $k$ Klassen verteilt (Mindestbedingung), und dann
rekursiert die Formel so, dass im naechsten Stack wieder erst die
Mindestbedingung erfuellt werden muss, also aus den $n - k$ werden jetzt wieder
$k$ entnommen und auf die $k$ verteilt. So hat man $2$ Elemente pro Klasse und
$n - 2k$ verbleibende Elemente. Der zweite Fall ist, dass man nicht $2$ Elemente
pro Klasse haben will. Dann nimmt man ein Element aus den $n$ raus, gibt es in
eine Klasse, und rekursiert einfach fuer die $n - 1$ Elemente, um sie auf $k -
1$ Klassen zu verteilen.

#### Fuer leere Klassen

Will man $n$ Elemente auf $k$ Klassen verteilen, von welchen manche auch leer
sein koennen, ist es auch moeglich, die ungeordneten Zahlpartitionen zu
verwenden. Dazu ueberlegt man sich, dass wenn $k - 1$ Klassen leer sind, man die
$n$ Elemente auf eine Klasse ungeordnet partitioneren muss ($P_{n,1}$), wenn
$k - 2$ leer sind, auf zwei Klassen ($P_{n,2}$) etc, also:

$$\sum_{i=1}^k P_{n,i}$$

Nach Definition der Rekursionsgleichung fuer $P_{n,k}$ folgt, dass fuer diese
Summe gilt (siehe oben):

$\sum_{i=1}^k P_{n,i} = P_{n+k,k}$

Das gilt, weil wir bei der Definition von $P_{n,k}$ als $\sum_{i=1}^k P_{n-k,k}$
gesagt, haben, "dass wir zuerst $k$ Elemente auf die $k$ Klassen verteilen, um
sie nichtleer zu machen, und die anderen $n - k$ Elemente dann beliebig auf die
$k$ Klassen partitionieren". Hier ueberspringen wir den Schritt mit "wir geben
in jede Klasse schon eines rein", und verteilen schon gleich beliebig. Die
Formel gilt trotzdem, wir haben nur die Annahme betrogen.

#### Wenn die Klassen unterscheidbar sind

Wenn die Klassen jeweils eine andere Anzahl an Elementen enthalten muessen, so
wie hier:

$$\{(x_1,x_2,...,x_k) \in \mathbb{N}^k \,|\, x_1 < x_2 < ... < x_k \land
\sum_{i=1}^k x_i = n\}$$

Dann: Verteile zuerst aufsteigend Einsen auf die $k$ Plaetze, also auf den
ersten $1$, auf den zweiten $2$ etc. Dann hat man (maximal) insgesamt $k(k+1)/2$
Einsen verteilt, so dass die Klassen verschieden sind. Nun verteilt man die
restlichen $n - (k(k+1)/2)$ Einsen auf beliebig viele Klassen. Weil die
Reihenfolge implizit ist, werden die restlichen Einsen so verteilt, dass die
Klassen mit den meisten Einsen die meisten restlichen Einsen bekommen. Also:

$\sum_{i=0}^k P_{n-(k(k+1)/2),i} = P_{n+k+(k(k+1)/2),i}$

Wenn die Klassen noch leer sein koennen, dann kann verteilt man am Anfang auch
eine $0$. Da die Klassen verschieden sein muessen, kann auch nur maximal diese
eine Klasse null sein. Somit hat man die Summe von $0$ bis $k - 1$, gleich
$k(k-1)/2$:

$\sum_{i=0}^k P_{n-(k(k-1)/2), i} = P_{n+k-(k(k-1)/2), i}$

### Geordnete Zahlpartitionen

Bei ungeordneten Zahlpartitionen war die Reihenfolge der einzelnen Multimengen
von Einsen bzw. die Reihenfolge der Zahlen $n_1, ..., n_k$ (wenn man die
Multimengen von Einsen als ihre Summe auffasst) innerhalb der Partition egal,
also $\{\{1, 1\}, \{1, 1\}, \{1\}\}$ war gleich $\{\{1, 1\}, \{1\}, \{1, 1\}\}$
bzw. $\{2, 2, 1\}$ war gleich $\{2, 1, 2\}$.

Man nehme aber nun die Situation, dass man $n$ Euro auf $k$ Kinder verteilen
moechte. Die Euro selbst sind ununterscheidbar, jedoch sind die $k$ Kinder alle
unterscheidbar. Daher ist eine $k$-Zahlpartition $\{2, 1\}$ fuer drei Euro bei
zwei Kindern nicht gleich $\{1, 2\}$, denn im ersten Fall bekommt Kind 1 (Hans)
$2$ Euro und im zweiten Fall ist es Kind 2 (Helga), die die zwei Euro
bekommt. Da $\text{Hans} \neq \text{Helga}$ sind dies also zwei unterschiedliche
Partitionen. Man nennt so eine Partition *geordnet*.

Es gibt leider kein Symbol fuer geordnete Zahlpartitionen, aber sei $p_{n,k}$
hier die Anzahl an geordneten $k$-Partitionen von $n$ Zahlen.

Frage: Wieviele geordnete $k$-Partitionen einer $n$-elementigen Multimenge gibt
es?  Antwort: ${n - 1 \choose k - 1}$

http://mathforum.org/library/drmath/view/68607.html

#### Berechnung

Der mathematische Ausdruck fuer die geordneten $k$-Zahlpartitionen einer
$n$-elementigen Multimenge ist:

$$p_{n,k} = {n - 1 \choose k - 1}$$

Die Berechnung ist wunderschoen auffassbar, wenn man $n$ in seine einzelnen
Einser aufteilt:

$n = 1 + 1 + 1 + 1 + 1 + 1 ... + 1$

Die Zahl $n$ kann also als $n$ Einsen geschrieben werden. Jede Eins ist von
einer anderen durch ein $+$ getrennt. Insgesamt gibt es fuer die $n$ Zahlen $n -
1$ Plusse. Nun waehle man $k - 1$ Plusse, um sie in Kommata umzuwandeln, die die
$k$ Klassen voneinander abtrennen. Dafuer gibt es genau ${n - 1 \choose k - 1}$
Moeglichkeiten.

Die Summe aller moeglichen geordneten Partitionen von $n$ Einsen ist demnach:

$\sum_{k=1}^n {n - 1\choose k - 1}$

Da man aber auch sagen kann, dass jedes der $n - 1$ Plusse einmal dabei sein
kann oder nicht (einmal eine neue Klasse abtrennt, oder nicht), gilt auch:

$\sum_{k=1}^n {n - 1\choose k - 1} = 2^{n - 1}$

#### Leere Klassen

Wenn die einzelnen Mengen leer sein koennen, dann sollte man mit ${n + k - 1
\choose k - 1}$ rechnen. Z.B.: Wieviele Terme enthaelt die Expansion von $(1 + x
+y + z)^{16}$? Antwort: So viele, wie es Moeglichkeiten gibt, aus vier Werten
die Summe $16$ zu bilden. Hier sind nicht die geordneten Zahlpartitionen
gesucht, weil Summanden auch $0$ (die Mengen also leer sein koennen). Daher: $n$
Striche, $k - 1$ Kommata: ${n + (k - 1) \choose k - 1}$.

## Goldene Tabelle

Man will eine Menge von $B$ Baellen auf $U$ Urnen verteilen. Sei $k = |B|$, $n =
|U|$. __Jeder Ball muss in eine Urne__.

Baelle gleich = Multimenge von Einsen/Strichen
Urnen gleich  = Vertauschbare Klassen in Mengen.

Surjektiv: Partitionen
Beliebig: Wenn Urnen gleich sind, Partitionen summieren.

### Baelle und Urnen unterscheidbar

#### Beliebig viele Baelle pro Urne

Fuer jede der $k$ Baelle $n$ Urnen: $n^k$

#### Hoechstens ein Ball pro Urne (injektiv)

Fuer den ersten Ball $n$ Urnen, fuer den zweiten $n - 1$ etc.:
$n^{\underline{k}}$

Geht nur, wenn $k \leq n$ (fuer jeden Ball eine Urne, fuer jede Urne maximal
einen Ball).

#### Mindestens ein Ball pro Urne (surjektiv)

*Geordnete* $k$-Partition der $n$-elementigen Menge: $k! S_{n,k}$

#### Genau ein Ball pro Urne (bijektiv)

Geht nur, wenn $n = k$, dann: $k!$

Fuer jeden Ball eine Urne, fuer jede Urne einen Ball.

### Baelle gleich, Urnen unterscheidbar

#### Beliebig viele Baelle pro Urne

$k$ Striche und $n$ Kommata: ${k + n - 1 \choose k}$

#### Hoechstens ein Ball pro Urne (injektiv)

Wieder gehen wir davon aus dass es mindestens so viele Urnen wie Baelle gibt
(jeder Ball eine Urne, jede Urne maximal ein Ball), daher: ${n \choose k}$

#### Mindestens ein Ball pro Urne (surjektiv)

Da die Baelle nicht unterscheidbar sind, ist es eine Multimenge von $k$ selben
Elementen (z.B. Striche). Von den $k - 1$ Kommata zwischen den Elementen, waehlen
wir $n - 1$ aus: ${k - 1 \choose n - 1}$

#### Genau ein Ball pro Urne (bijektiv)

Da man die Baelle nicht permutieren kann, und es einen Ball pro Urne gibt: $1$
Moeglichkeit.

### Baelle unterscheidbar, Urnen gleich

#### Beliebig viele Baelle pro Urne

Es sind nicht ganz die Stirling Zahlen 2. Art, da die Klassen auch leer sein
koenen, aber man kann schummeln, indem man summiert: Zuerst verteilt man die $k$
Baelle auf eine Urne (die anderen $n - 1$ leer), dann auf 2 etc.:

$\sum_{i=1}^n S_{k,i}$

#### Hoechstens ein Ball pro Urne (injektiv)

Da die $n$ Urnen alle gleich sind, ist es egal, wie man die Baelle verteilt: $1$

#### Mindestens ein Ball pro Urne (surjektiv)

Nun duerfen die Klassen nicht leer sein, und die Urnen haben keine Ordnung:

$S_{k,n}$

#### Genau ein Ball pro Urne (bijektiv)

Eine Moeglichkeit.

### Baelle gleich, Urnen gleich

#### Beliebig viele Baelle pro Urne

Ungeordnete Zahlpartitionen von $k$ gleichen Einsen in $n$ gleiche Klassen, wobei
manche auch leer sein koenenn: $\sum_{i=0}^nP_{k,i}$

#### Hoechstens ein Ball pro Urne (injektiv)

Eine Moeglichkeit.

#### Mindestens ein Ball pro Urne (surjektiv)

Ungeordnete Zahlpartitionen: $P_{k,n}$

#### Genau ein Ball pro Urne (bijektiv)

Eine Moeglichkeit.

## Gotchas

* Wenn man die Moglichkeiten zaehlen, soll, dass mindestens $1$ Element irgendwo
  enthalten ist, subtrahiere lieber von allen Moeglichkeiten, die Moeglichkeit,
  dass kein Element gewaehlt wird (Gegenmenge!)
