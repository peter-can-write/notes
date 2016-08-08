# Graphen

$\newcommand{\deg}{\mathop{\rm deg}\nolimits}$
$\newcommand{\indeg}{\mathop{\rm indeg}\nolimits}$
$\newcommand{\outdeg}{\mathop{\rm outdeg}\nolimits}$
$\newcommand{\pop}{\mathtt{pop()}}$
$\newcommand{\back}{\mathtt{back()}}$
$\newcommand{\sort}{\mathtt{sort}}$
$\newcommand{\union}{\mathtt{union}}$
$\newcommand{\find}{\mathtt{find}}$

Die Graphentheorie und Algorithmen auf Graphen sind ein wichtiger Bestandteil
der Algorithmik. Insbesondere deswegen, weil Graphen in der echten Welt so
haeufig vorkommen. Netzwerke wie Stromnetzwerke, soziale Netzwerke oder
Strassennetzwerke findet man ueberall. Es ist insbesondere deswegen sehr
interessant, zu verstehen, wie man auf Graphen effizient operieren kann.

## Graphenmodell

In unserem Graphenmodell betrachten wir Graphen als ein Paar $G = (V, E)$,
bestehend aus einer Menge von Knoten $V$ (*Vertices*) und Kanten $E$
(*Edges*). Hierbei notieren wir ab nun die Anzahl $|V|$ an Knoten als $n$ und
die Anzahl an Kanten $|E|$ mit $m$.

Was die Elemente von $E$ sind, haengt vom jeweiligen Graphentyp ab. Grob gibt es
zwei Arten von Graphen: ungerichtete und gerichtete. Bei ungerichteten Graphen
sind Kanten immer symmetrisch. Kommt man also von Knoten $v_1$ zu Knoten $v_2$,
so auch von $v_2$ zu $v_1$. Um dies mathematisch zu repraesentieren, geben wir
Kanten als $2$-Mengen $\{v_1, v_2\}$ in die Kantenmenge $E$. Die Menge aller
moeglichen Kanten ist somit $\mathcal{E} := \{ \{v, w\} \,|\, v,w \in V\}$ und
$E$ stets $\subseteq \mathcal{E}$. Kanten in *gerichteten* Graphen sind jedoch
nicht *symmetrisch*. Sie haben also eine Richtung (daher der Name) und geht
Kante $e$ von $v_1$ zu $v_2$, so muss es nicht notwendigerweise (aber
moeglicherweise) eine Kante von $v_2$ zu $v_1$ geben. Somit notieren wir Kanten
in gerichteten Graphenmodellen nicht als Mengen, sondern Paare $(v_1,
v_2)$. Somit ist $E \subseteq V \times V$.

Im Weiteren nennen wir zwei Knoten $v,w \in V$ dann *adjazent*, wenn es eine
Kante $e$ in $E$ gibt, die sie verbindet. Gilt dies, so nennt man die Kante $e$
inzident zu den Knoten $v,w$, die $e$ verbindet. Der Grad $\deg(v)$ sagt uns
dann im Weiteren, zu wievielen Kanten ein Knoten $e$ verbunden ist. Bei
ungerichteten Graphen, ist diese Definition ausreichend. Bei gerichteten Graphen
ist es nun aber so, dass es Kanten $(x, v), x \in V$ gibt, die bei $v$ eingehen
aber auch andere Kanten $(v, x), x \in V$, die *von* $v$ ausgehen. Ein Grad
$\deg$ ist also nicht ausreichend. Deswegen spezifizieren wir durch $\indeg(v)$
den *Indegree*, also die Anzahl eingehender Kanten, sowie durch $\outdeg(v)$ den
*Outdegree*, also die Anzahl ausgehender Kanten.

Die letzte Eigenschaft, die wir unserem Graphen noch geben koennen, ist
*Gewichtung*. Wir koennen naemlich Knoten oder Kanten (meistens) einen
numerischen Wert zuweisen, der z.B. ihre Prioritaet darstellt. In einem
Strassennetzwerk waere das beispielsweise die Distanz zwischen zwei Knoten oder
die Kosten fuer eine Uebertragung. Hierfuer definieren wir eine Funktion
$\mathcal{w}: E \rightarrow \mathbb{R}$, die einer Kante $e$ ihre Kosten
bzw. ihr Gewicht $\mathcal{w}(e)$ zuweisst.

## Implementierung

Wir haben nun also eine formale, mathematische Definition von Graphen. Wie
setzen wir diese aber in einem Computer um? Es gibt hier mehrere
Antworten. Allgemein entscheiden wir auf Basis einiger Ueberlegungen
bzw. Operationen, die wir durchfuehren wollen. Moeglicherweise wollen wir
effizient

* herausfinden, ob zwei Knoten $v,w$ verbunden sind,
* herausfinden, zu welchen Knoten $v$ (ausgehend oder eingehend) verbunden ist,
* neue Knoten oder Kanten einfuegen oder loeschen.

Betrachten wir nun einige der Antworten auf die Frage, wie man Graphen effizient
in einem Computer repraesentieren kann. Sei hierfuer ein gerichteter Graph $G =
(V, E)$ gegeben, mit $V = \{1, 2, 3\}$ und $E = \{(1, 2), (1, 3), (2, 3), (3, 2)\}$.

### Kantenliste

Die erste Moeglichkeit ist es, einfach explizit eine (verkettete) Liste der
Kanten $E$ zu haben. Die Vorteile hiervon sind, dass sie optimalen
Speicherverbrauch haben, da sie nur das speichern, was notwendig ist. Auf Grund
der Eigenschaften von Listen hat das Einfuegen oder Loeschen von Knoten oder
Kanten konstante Laufzeit.

Jedoch hat eine Kantenliste, wie jede Liste, auch viele Nachteile. Da sie
keinerlei Ordnung oder Struktur hat, kann eine Suche nach Adjazenz zweier Knoten
$v,w$ nur linear in der Anzahl an Kanten erfolgen. Selbiges gilt fuer das finden
aller zu einem Knoten adjazenten Nachbarn.

### Adjazenzmatrix

Eine weitere einleuchtende Moeglichkeit, einen Graphen zu speichern, ist eine
Adjazenzmatrix. Diese kennen wir schon aus der mathematischen Definition von
Graphen. Der offensichtliche Nachteil von Adjazenzmatrizen ist, dass sie $n^2$
Eintraege haben. Vorteilhaft ist aber hingegen, dass man in konstanter Zeit auf
Nachbarschaft ueberpruefen kann. Das Loeschen und Einfuegen von Kanten verlaueft
ebenso konstant. Neue Knoten einzufuegen ist hingegen schwierig, da man sowohl
eine neue Spalte als auch einen neuen Eintrag in jeder Zeile einfuegen muss, was
also linear in $n$ laeuft. Letztlich muss das finden von Nachbarn natuerlich
auch in linearer Zeit verlaufen.

|   |$1$|$2$|$3$|
|:--|:--|:--|:--|
|$1$|   | x | x |
|$2$|   |   | x |
|$3$|   | x |   |

#### Adjazenzbitset

Eine effizientere und platzsparendere Variante einer Adjazenzmatrix waere ein
*Adjazenzbitset*, wobei wir jede Zeile einer Adjazenzmatrix durch ein Bitset mit
$n$ Bits repraesentieren, und dann die Zeilen der Matrix zu einem grossen Bitset
oder Array konkatenieren. Die Konkatenation ginge an sich auch bei normalen
Adjazenzmatrizen ohne Bitsetkodierung, wird durch diese Variante aber in ihrer
Platzsparfaehigkeit maximiert. Insbesondere ist toll, dass das Bitset
(bzw. Array) kontinuierlich und somit cache-effizient im Speicher liegt. Einen
neuen Knoten einzufuegen, wird dann jedoch etwas schwieriger.

### Adjazenzarray

Weiters ist es moeglich, Kanten kontinuerlich in einem Array zu speichern und
dann via einem separaten Array die Startindizes fuer jeden Knoten zu
speichern. Diese Variante ist natuerlich viel platzsparender als eine
Adjazenzmatrix, macht das Einfuegen von neuen Kanten aber schwieriger, da man
dann alle Startindizes rechts vom Quellknoten inkrementieren muss.

```
  [0 | 2 | 3]
  /     \   \
[2 | 3 | 3 | 2]
```

### Adjazenzlisten

Eine populaere Variante speichert fuer jeden Knoten im Graphen eine Liste oder
ein Array, welches die inzidenten Kanten bzw. adjazenten Knoten des jeweiligen
Knoten speichert. Dadurch wird das Einfuegen von neuen Knoten oder Kanten
(eventuell amortisiert) $O(1)$ (sofern das Einfuegen durch ein `push_back`
geht), die Suche nach Adjazenz zweier Knoten $O(\deg(v))$ und das Traversieren
benachbarter Knoten ebenso.

```
[1 | 2 | 3]
 |   |   |
 2   3   2
 |
 3
```

### Adjazenzhashtabellen

Natuerlich ist es auch moeglich, die Adjazenzlisten nicht als Listen, sondern
Hashtabellen zu speichern. Dadurch bleibt das Einfuegen neuer Kanten konstant,
wohingegen das Loeschen oder Finden von Kanten (also, Adjazenz) nun erwartet
konstant verlaufen kann, anstelle von linear im Grad des Knoten. Das Finden
aller Nachbarn kann wiederum linear verlaufen. Dies wird aber in der Regel
weniger effizient als eine Array-basierte Adjazenzliste sein, da das
traversieren der Ketten einer separate-chaining Hash-Tabelle relativ komplex und
cache-ineffizient ist.

### Implizite Repraesentation

Manchmal ist die Repraesentation durch die Struktur des Graphen fixiert und
braucht somit keine explizite Speicherung. Das waere beispielsweise fuer
Gittergraphen so, die durch Breiten- und Laengen Parameter $b,l$ bereits
eindeutig, auch in ihrer Konnektivitaet, definiert sind. In diesem Fall kann man
einfach mathematisch berechnen, ob zwei Knoten adjazent sind, oder
nicht. Kantengewichte koennte man dann noch separat speichern, wobei der
Platzverbrauch hierbei mit $k(l - 1) + l(k - 1)$ schon eindeutig festgelegt
waere.

### Vergleich

Welche dieser Varianten macht nun am meisten Sinn? Das haengt natuerlich wie
immer voll und ganz von der Situation ab. Eine Frage, die wir uns aber stellen
koennen, ist wie *dicht* der Graph ist. Ein *dichter* Graph ist einer, der viele
Kanten hat, also wo $|E|$ sehr nahe an $n(n - 1)$ ist (keine Schleifen). In
diesem Fall ist naemlich oftmals eine Repraesentation, die normalerweise mehr
Platz brauchen wuerde, aber effizienter gespeichert werden kann, vor einer
ueblicherweise effizienteren vorzuziehen. Auch ist wichtig, wie dynamisch oder
statisch ein Graph sein soll. Hier einige Vorschlaege:

* Dicht und statisch: Adjazenzbitset.
* Dicht und dynamisch:
  - Falls Frage nach Adjazenz wichtig ist: Adjazenzhashtabelle.
  - Falls Frage nach allen Nachbarn wichtig ist: Adjazenzliste (mit Arrays).
* Nicht dicht und statisch: Adjazenzarray.
* Nicht dicht und dynamisch: Adjazenzliste (mit Arrays).

Wir gehen hier im Uebrigen davon aus, dass man die Knoten linear von $1$ bis $n$
durchnummerieren kann. Ist das nicht der Fall, so kommen Arrays als
uebergeordnete Strukturen gar nicht in Frage, sondern nur Hashtabellen
(allgemein Suchstrukturen).

## Graphtraversierung

Wir interessieren uns nun dafuer, wie man einen Graphen traversieren kann, um
irgendeine Aufgabe zu loesen. Hierfuer gibt es zwei Standardverfahren: die
Breitensuche und die Tiefensuche. Die Breitensuche traversiert einen Graphen
"level-order", also alle Nachbarn eines Knoten nacheinander. Die Tiefensuche
geht hingegen greedy und ohne bestimmte Reihenfolge durch einen Graphen. In
beiden Faellen gibt es immer einen Startknoten, von welchem aus wir die Suche
starten. Auch benoetigen wir immer eine Moeglichkeit, zu pruefen, ob wir einen
Knoten bereits besucht haben. Sind die Knoten linear durchnummeriert, eignet
sich hierfuer ein Bitset gut. Ansonsten muss man ein Hashset verwenden.

Wir wollen diese beiden Varianten nun untersuchen.

### Breitensuche

Wir haben bei der Diskussion von Baeumen bereits das Konzept von *level-order*
Traversierung kennengelernt. Hierbei haben wir von der Wurzel des Baumes
ausgehend jeweils immer alle Kinder eines Knoten auf einer Ebene besucht, bevor
wir zum naechsten Knoten und dessen Kinder gegangen sind. Die *Breitensuche*
(*Breadth-First-Search; BFS*)ist nun genau das, aber auf einem allgemeinen
Graphen. Wiederum ist heirbei die Queue die essentielle Datenstruktur, um die
Reihenfolge der Knotentraversierung zu bestimmen. Hierbei gehen wir wie folgt
vor:

1. Sei:
   * $v$ der Startknoten der Breitensuche.
   * $Q$ eine Queue, die initial nur $v$ enthaelt.
   * $S$ eine Suchstruktur, in der wir speichern, ob wir einen Knoten bereits
     besucht haben. Setze initial den Quellknoten $v$ als besucht.
   * $f(v)$ eine beliebige Taetigkeit, die einen Knoten $v$ nimmt.
2. Solange, wie $Q$ nicht leer ist:
   1. Setze (bzw. ueberschreibe) $v := Q.\pop$.
   2. Rufe $f(v)$ auf.
   3. Fuer jeden adjazenten Nachbarn $w$ von $v$:
	  1. Pruefe in $S$ ob $w$ bereits besucht wurde.
	  2. Falls ja, so ueberspringe $w$.
	  3. Falls nein, gib $w$ in die Queue $Q$ und markiere ihn in $S$ als markiert.

Es ist hier sehr wichtig, den Knoten dann zu markieren, wenn man ihn in die
Queue gibt. Markiert man ihn erst, wenn man ihn bearbeitet, so koennten
vorherige Knoten in der Queue den Knoten erneut eingefuegt haben.

Die Laufzeit von BFS ist $O(n + m)$, da wir jeden der $n$ Knoten genau einmal
besuchen und dann auch noch fuer jede der $m$ Kanten irgendeinen (konstanten)
Aufwand haben.

#### Anwendungen

Hier nun zwei Algorithmen, die auf der Breitensuche basieren.

##### Kuerzester Pfad

Das tolle an Breitensuche ist, dass es vom Ursprungsknoten ausgehend jeden
anderen Knoten im Graphen immer mit der minimalen Anzahl an Kanten besucht. Denn
da wir Level-fuer-Level durch den Graphen traversieren haben wir auch nie eine
Redundanz an Kanten, wenn wir einen anderen Knoten *das erste* Mal besuchen. Das
ergibt schon Mal einen ersten Algorithmus um den kuerzesten Pfad zwischen zwei
Knoten zu finden. Es gibt hier aber noch zwei verschieden Varianten, wie man
dies implementiert.

Seien nun $v,w$ zwei Knoten, dessen Konnektivitaet wir ueberpruefen
moechten. Dann beobachten wir, dass wenn wir von $v$ ausgehen, bei der
Breitensuche die Laenge des Pfades immer dann um genau Eins waechst, wenn wir
ein "Level" abgearbeitet haben. Bei einem Baum ist ein "Level" leicht zu
verstehen, da er nur nach unten waechst und es nie Rueckwaertskanten gibt,
sodass wir explizit von einer Ebenenstruktur sprechen koennen. Bei Graphen ist
das nicht so leicht. Nehmen wir aber an, wir gehen von $v$ aus. Dann endet das
erste "Level", was auch immer das momentan heissen mag, offensichtlich beim
letzten Nachbarn $u$ von $v$, den wir in die Queue tun. Nun koennen wir ein
Level einfach so definieren, dass es bei $u$ aufhoert und beim ersten Knoten
nach $u$ beginnt! Um den kuerzesten Pfad zwischen $v$ und $w$ also zu bestimmen,
setzen wir einen `last-of-level` Zeiger initial auf $v$. Immer dann, wenn wir
den `last-of-level` Knoten dequeuen, haben wir also ein Level abgearbeitet. Dann
ist der neue `last-of-level` Knoten gerade der letzte Knoten in der Queue, also
sozusagen das "letzte Kind". Genau dann waechst also auch die Pfadlaenge um
Eins. Somit ergibt sich der folgende Algorithmus:

1. Sei:
   * $v$ der Quellknoten.
   * $w$ der Zielknoten.
   * $\lambda$ die Pfadlange.
   * $\kappa$ der letzte Knoten eines Levels.
   * Andere Datenstrukturen der Breitensuche wie oben definiert.
2. Initialisiere den BFS. Dann setze $\lambda := 0$ und $\nu := v$.
3. Solange, wie $Q$ nicht leer ist:
   1. Setze $\nu := Q.\pop$.
   2. Falls $\nu = w$, haben wir den Zielknoten gefunden, also gib $\lambda$
      zurueck.
   4. Fuer jeden adjazenten Knoten $\mu$ von $\nu$:
	  1. Ist $\mu$ schon besucht, so ueberspringe $\mu$.
	  2. Ansonsten gib $\mu$ in die Queue $Q$ und markiere ihn in $S$ als besucht.
   5. Falls $\kappa = \nu$, setze $\kappa = Q.\back$, also dem letzten Knoten
      der Queue.
4. Falls (2) nie eintrat, sind $v$ und $w$ nicht verbunden.

Es sei aber angemerkt, dass wir hier eine wichtige, essentielle Annahme
machen. Wir nehmen an, dass die Kanten ungewichtet sind und somit allesamt
gleich teuer sind. Sonst wuerde es nicht notwendigerweise sein, dass der
kuerzeste Pfad auch der, bezueglich der Kantenkosten, billigste ist.

Um den eigentlichen Pfad zu sammeln, muessen wir uns merken, wie wir zum
Zielknoten $w$ gekommen sind. Hierfuer nehmen wir ein Array, in welchem wir uns
immer den Vorgaenger fuer jeden Knoten merken, den wir besuchen. Am Ende koennen
wir durch dieses Array rueckwaerts durchlaufen und erhalten so genau den Pfad,
wie wir vom Quell- zum Zielknotn gekommen sind:

1. Sei:
   * $v$ der Quellknoten.
   * $w$ der Zielknoten.
   * $P$ ein Array, in welchem wir uns den Vorgaenger jedes Knoten
     merken.
   * Andere Datenstrukturen der Breitensuche wie oben definiert.
2. Solange $Q$ nicht leer ist:
   1. Setze $\nu := Q.\pop$.
   2. Falls $\nu = w$, so haben wir den Zielknoten gefunden. Breche die
      Breitensuche also ab.
   3. Fuer jeden adjazenten Knoten $\mu$ von $\nu$:
	  1. Ist $\mu$ schon besucht, ueberspringe $\mu$.
	  2. Ansonsten gib $\mu$ in die Queue und markiere ihn in $S$ als besucht.
	  3. Setze nun auch $P_\mu = \nu$, also vermerke $\nu$ als Vorgaenger
         (Parent) von $\mu$.
3. Wir sammeln nun den Pfad. Sei hierzu $\rho$ der Vektor, in welchem wir den
   Pfad sammeln.
4. Setze $\rho_0 = v$.
5. Setze $\nu := w$. Dann, solange wie $\nu \neq v$ gilt:
	1. Gib $\nu$ in den Pfad.
	2. Setze $\nu := P_\nu$.
6. Gib $\rho$ zurueck.

##### Shortest Path Tree

Wir koennen wieder unter der Annahme, dass Knoten ungwichtet sind (bzw. alle
dieselben Gewichte haben), via BFS auch den *Shortest Path Tree* (*SPT*)
bestimmen. Der SPT, auch *Single Source Shortest Path Tree* (*SSSPT*) genannt,
ist dabei jener Spannbaum eines Graphen, der von einem bestimmten Wurzelknoten
aus zu jedem anderen Knoten im Graphen genau den kuerzesten Pfad aus dem
urspruenglichen Graphen erhaelt. Da wir bei der Breitensuche den kuerzesten Pfad
immer dann haben, wenn wir zum ersten Mal bei einem Knoten ankommen, koennen wir
den SPT einfach finden, indem wir zu einem anfangs leeren Knoten immer dann eine
neue Kante hinzugeben, wenn wir einen Knoten zum ersten Mal besuchen. Hierbei
verbinden wir dann immer den Knoten in der momentanen Iteration von BFS mit
seinen adjazenten Nachbarn:

1. Sei:
   * $v$ der Quellknoten.
   * $B$ der anfangs leere Shortest Path Tree, den wir finden wollen.
   * Andere Datenstrukturen der Breitensuche wie oben definiert.
2. Solange $Q$ nicht leer ist:
   1. Setze $\nu := Q.\pop$.
   3. Fuer jeden adjazenten Knoten $\mu$ von $\nu$:
	  1. Ist $\mu$ schon besucht, ueberspringe $\mu$.
	  2. Ansonsten gib $\mu$ in die Queue und markiere ihn in $S$ als besucht.
	  3. Gib nun auch die Kante $e = \{\nu, \mu\}$ in den Shortest Path Tree
         $B$.
3. Gib $B$ zurueck.

### Tiefensuche

Die *Tiefensuche* (*Depth First Search*; *DFS*) ist die zweite elementare Art
der Graphtraversierung. Tiefensuche kann man gewissermassen als "greedy"
bezeichnen, da es einfach immer so weit durch den Graphen geht, bis es an eine
Wand stoesst. Eine "Wand" ist hierbei entweder ein Knoten, den wir schon besucht
haben, oder ein Knoten, der keine Nachbarn hat. Dann geht der DFS Algorithmus
wieder zurueck vorherigen Knoten und probiert dort weiter. Der wohl wichtigste
Unterschied zwischen DFS und BFS ist, dass DFS (meist) via Rekursion und ohne
Queue implementiert wird. Eine Standardimplementierung von DFS wuerde wie folgt
aussehen:

1. Sei:
   * $v$ der Startknoten der Tiefensuche.
   * $S$ eine Datenstruktur, wo wir vermkerken koennen, dass wir einen Knoten
     schon besucht haben. Diese sei anfangs leer.
   * $f$ eine beliebige Taetigkeit fuer einen Knoten.
   * $g$ noch eine solche Taetigkeit.
2. Rufe nun die Tiefensuche mit $v$ auf. Dann:
   1. Wurde $v$ schon besucht, so gehe zurueck.
   2. Ansonsten markiere $v$ als besucht.
   3. Fuehre $f(v)$ aus.
   4. Fuer jeden adjazenten Knoten $w$ zu $v$:
	  1. Rufe die Tiefensuche rekursiv mit $w$ auf.
   5. Nachdem alle Nachbarn von $v$ abgearbeitet wurden, rufe $g(v)$ auf.

Die Laufzeit von DFS ist nach selber Begruendung wie bei BFS auch in $O(n + m)$.

#### DFS Nummerierung

Bei der Tiefensuche moechten wir oft zwei verschiedene Arten von Zahlen bei der
Exploration eines Graphen vermerken. Das sind zum einen die *DFS Zahlen* und zum
anderen die *Finish Zahlen*. Die DFS Zahlen sollen fuer jeden Knoten angeben,
wann wir diesen in der Reihenfolge aller Knoten besucht haben. Die Finish Zahlen
sollen hingegen vermerken, wann wir mit einem Knoten fertig geworden sind,
nachdem wir seine Kinder besucht haben. Diese Zahlen koennen wir wie folgt
finden:

1. Sei:
   * $v$ der Startknoten der Tiefensuche.
   * $S$ eine Datenstruktur, wo wir vermkerken koennen, dass wir einen Knoten
     schon besucht haben. Diese sei anfangs leer.
   * $D$ das Array, in welchem wir die Explorationsreihenfolge jedes Knoten
     vermerken.
   * $F$ das Array, in welch wir die Fertigstellungsreihenfolge jedes Knoten
     vermerken.
   * $d$ ein Zaehler und Index fuer $D$ und initial Null.
   * $f$ ein Zaehler und Index fuer $F$ und initial Null.
2. Rufe nun die Tiefensuche mit $v$ auf. Dann:
   1. Wurde $v$ schon besucht, so gehe zurueck.
   2. Ansonsten markiere $v$ als besucht.
   3. Setze $D_v = d$ und inkrementiere $d$.
   4. Fuer jeden adjazenten Knoten $w$ zu $v$:
	  1. Rufe die Tiefensuche rekursiv mit $w$ auf.
   5. Nachdem alle Nachbarn von $v$ abgearbeitet wurden, setze $F_v = f$ und
      inkrementiere $f$.

#### Kantentypen

Interessanterweise koennen wir auf Basis der DFS Nummerierung Kanten in einem
*gerichteten* Graphen kategorisieren. Wir beobachten hierbei vier verschiedene
Arten von Kanten:

* *Baumkanten*: Solche ausgehenden Kanten, die von einem Knoten $v$ zu einem
  bisher unbesuchten Knoten $w$ gehen. Es kann zu jedem Knoten insbesondere
  immer nur eine Baumkante geben.
* *Vorwaertskante*: Ausgehende Kanten, die zwar nicht zu einem direkt adjazenten
  Nachbarn gehen, aber zumindest noch immer zu einem Nachfahren. Mit einem
  Nachfahren bezeichnen wir hierbei einen Knoten, zu welchem es einen Pfad gibt,
  der keinen Kreis schliesst.
* *Rueckwaertskante*: Ausgehende Kanten zu einem Vorfahren. Solche Kanten
  schliessen einen Kreis, wobei andere Kanten in diesem Kreis nur Baumkanten
  oder Vorwaertskanten waren.
* *Kreuzkanten*: Sonstige ausgehende Kanten. Das sind zum Beispiel solche, die
  Baumkanten waeren, wenn es nicht schon eine andere Baumkante zum Zielknoten
  gibt.

Wir koennen nun weitere Beobachtungen fuer diese Kantenkategorieren machen. Sei
hierzu $e = (v, w)$ eine gerichtete Kante zwischen zwei Knoten $v,w$. Dann:

* Fuer Baum- und Vorwaertskanten ist die DFS Zahl des Quellknoten $v$ immer
  strikt kleiner als die von $w$, weil wir, wenn wir $v$ besucht haben, zum
  allerersten Mal im Graphen $w$ besuchen. Insbesondere gilt diese Eigenschaft
  dann fuer Kreuzkanten eben nicht. Auch gilt fuer diese zwei Kategorien, dass
  die Finish Zahl des Zielknoten $w$ immer kleiner sein muss als die des
  Quellknoten $v$, weil wir mit $v$ sicher erst nach $w$ fertig werden.
* Fuer Kreuzkanten gilt wie gesagt nicht, dass $D_v < D_w$, da es schon eine
  andere Baumkante gegeben haben muss, ueber welche wir zu $w$ gekommen sind,
  sodass $w$ schon frueher eine DFS Zahl bekommen haben muss. Da wir dann aber
  $w$ sicher schon $v$ fertig bearbeitet haben, gilt dennoch $F_v > F_w$.
* Fuer Rueckwaertskanten gelten beide Eigenschaften nicht. Bei einer
  Rueckwaertskante $e = (v, w)$ ist es naemlich nun der Fall, dass wir $w$ schon
  vor dieser Kante besucht haben. Somit wird $w$ schon eine DFS Zahl erhalten
  haben, womit $D_v < D_w$ nicht gelten kann. Ebenso haben wir, dass der
  Zielknoten $w$ hier erst nach $v$ fertig werden wird, da $w$ ja insbesondere
  darauf warten muss, dass sein Nachfahre $v$ fertig wird. Somit wird $F_v <
  F_w$ gelten, was normalerweise nicht gelten wuerde.

Insbesondere sollte uns hier auffallen, dass Rueckwaertskanten als einzige die
Eigenschaft haben, dass die Finish Zahl des Quellknoten geringer ist als die
Zielknoten. Bei Rueckwaertskanten passen die DFS Zahlen zwar auch nicht, aber
das ist ebenso fuer Kreuzkanten der Fall. Was sagt uns dies? Nun, wenn wir nach
einer DFS-Traversierung merken wuerden, dass fuer irgendeine Kante $F_v < F_w$
gilt, so muss es Rueckwaertskante gegeben haben! Was sagt uns dies wiederum?
Nun, wenn es eine Rueckwaertskante gibt, dann gibt es einen Kreis. Gilt also
fuer eine Kante $F_v < F_w$, so kann der Graph nicht kreisfrei sein! Gilt aber
fuer alle Kanten $F_v > F_w$, so ist der Graph kreisfrei. Man nennt ihn dann
eine *Directed Acyclic Graph* (*DAG*).

## Zusammenhangskomponenten

Sei $G = (V, E)$ ein ungerichteter Graph. Dann nennen wir $G$
*zusammenhaengend*, wenn es von jedem Knoten $v$ in $G$ einen Pfad zu *jedem
anderen Knoten* in $G$ gibt. Ein maximaler, zusammenhaender Teilgraph des
Graphen $G$ wird dann Zusammenhangskomponente genannt. Ist der Graph als ganzes
zusammenhaengend, so ist $G$ gerade die einzige Zusammenhangskomponente. Ist $G$
aber nicht zusammenhaengend, so muss er zumindest zwei Zusammenhangskomponenten
haben. Wieviele Zusammenhangskomponenten ein Graph $G$ hat, kann man hierbei
sehr einfach ("brute force") via DFS herausfinden:

1. Sei:
   * $S$ die Datenstruktur, wo wir vermerken, ob wir einen Knoten bereits
     besucht haben.
   * $\mathbb{Z}$ die Menge aller bisher gefundenen Komponenten und initial
     leer.
2. Dann iteriere ueber jeden Knoten $v \in V$:
   1. Falls $v$ schon besucht wurde, uebespringe $v$.
   2. Ansonsten deklariere einen neuen Behaelter $Z$ fuer die
      Zusammenhangskomponente, die zumindest $v$ enthaelt.
   3. Rufe DFS rekursiv mit $G, v, Z, S$ auf und dann:
	  1. Falls $v$ schon besucht, gehe zurueck.
	  2. Ansonsten vermerke $v$ in $S$ als besucht.
	  3. Fuege $v$ in die Zusammenhangskomponente $Z$ ein.
	  4. Rekursiere fuer alle zu $v$ adjazenten Knoten.
   4. Fuege dann $Z$ in die Menge aller Zusammenhangskomponenten $\mathbb{Z}$
      ein.
3. Gib $\mathbb{Z}$ zurueck.

Im Weiteren nennen wir einen Graphen $G = (V, E)$ dann *$k$-fach
zusammenhaengend*, falls $G$ mehr als $k$ Knoten und hat und dann gilt, dass man
immer weniger als $k$ Knoten wegnehmen kann und $G$ trotzdem zusammenhaengend
bleibt. Somit ist also jeder zusammenhaengende Graph zumindest
$1$-zusammenhaengend. Ein $2$-zusammenhaender Graph waere dann ein solcher, wo
man einen beliebigen Knoten wegnehmen kann und der Graph dennoch
zusammenhaengend bleibt. Bei einem $3$-zusammenhaengenden Graphen koennte man
sogar zwei beliebige Knoten wegnehmen, sodass der Graph zusammenhaengend
bleibt. Das grundlegende an einer $k$-fach zusammenhaengenden Komponenten eines
Graphen ist, dass es genau $k$ disjunkte (separate) Pfade zwischen jedem Knoten
und jedem anderen Knoten gibt. Deswegen kann man $k - 1$ dieser Pfade
zerstoeren, sodass es fuer die verbleibenden Knoten zumindset noch einen Pfad
geben wird.

Wir bezeichnen dann noch jene maximalen Teilgraphen eines Graphen, die
$2$-zusammenhaengend sind, als *Zweifachzusammenhangskomponenten*. Dieser Graph
ist beispielsweise zweifachzusammenhaengend:

```
  o
 / \
o---o
```

Dann definieren wir noch das Konzept eines *Artikulationsknoten*. Ein
Artikulationsknoten (*cut-vertex*) ist ein Knoten, fuer welchen sich die Anzahl
an Zusammenhangskomponenten des Graphen erhoeht, wenn man diesen Knoten aus $G$
entfernt. Dieser Knoten ist also gerade der letzte Knoten, der zwei oder mehr
Zusammenhangskomponenten verbindet:

```
o     o
|     |
o--o--o
|  ^  |
o     o
```

Ein *Block* ist dann ein solcher maximal-zusammenhaengender Teilgraph, der
*keinen* Artikulationsknoten enthaelt. Innerhalb von Bloecken kann man also auch
jeden beliebigen Knoten entfernen, ohne, dass sich die Anzahl an
Zusammenhangskomponenten innerhalb des Blocker erhoeht.

### Blocke Finden

Wir wollen nun einen Algorithmus besprechen, der die Bloecke eines Graphen
bestimmen kann. Hierfuer modifizieren wir den uns bekannten DFS Algorithmus,
sodass wir nun nicht mehr nur die DFS Zahlen fuer jeden neu besuchten Knoten
notieren, sondern auch die kleinste DFS Zahl, die von einem Knoten aus besuchen
koennen. Wir haben also weiterhin unsere Sequenz $D$, die die DFS Zahlen
sammelt. Jetzt legen wir noch eine Sequenz $L$ an. Formal ist $L_v$ dann so
definiert, dass es die minimale DFS Nummer ist, die man von $v$ aus ueber
beliebig viele (also auch keine) Baumkanten und maximal einer Rueckwaertskante
erreichen kann. Somit ist $L_v$ stets das Minimum aus der DFS Zahl $D_v$ von $v$
selbst; der kleinsten DFS Zahl $L_w$ eines Kindes $w$ von $v$ sowie der DFS Zahl
$D_w$ einer Rueckwaertskante $(v, w)$, falls es eine solche gibt.

Wir koennen nun naemlich festlegen, dass ein Knoten $v \in V$ genau dann ein
Artikulationknoten ist, wenn $v$ entweder:

1. Die Wurzel des Graphen ist und zumindest zwei Kinder hat. Dann wuerde das
   entfernen von $v$ aus dem Graphen die durch die Wurzel $v$ verbundenen
   Teilgraphen zerreissen.
2. Nicht die Wurzel des Graphen ist, aber es ein Kind $w$ von $v$ gibt, das
   keinen Knoten vor $v$ (bezueglich der DFS Zahl) erreichen kann. Dann ist also
   $L_w \geq D_v$. Da $w$ dann also keinen Knoten vor $v$ erreichen kann, trennt
   $v$ den Knoten $w$ gerade von allem vor $v$ ab. Wuerde man $v$ entfernen, so
   wuerde man insbesondere fuer den Teilgraphen, der $w$ als Wurzel hat, eine
   neue Zusammenhangskomponente des Graphen erhalten.

Wir koennen die Bloecke via DFS dann wie folgt bestimmen:

1. Sei:
   * $S$ eine Datenstruktur, in welcher wir vermerken, ob wir einen Knoten schon
     besucht haben.
   * $D$ das Array, das die DFS Zahlen enthaelt.
   * $L$ das Arrays, das die kleinste erreichbare DFS Zahl jedes Knoten
     enthaelt.
   * $T$ ein Stack fuer Kanten, um die Kanten eines Blockes zu bestimmen.
   * $\mathbb{B}$ die Menge aller bisher gefundenen Bloecke und initial leer.
2. Dann rekursiere fuer einen Knoten $v$:
   1. Falls $v$ schon als besucht markiert ist, gehe zurueck.
   2. Sonst gib $v$ eine DFS Zahl in $D_v$ und markiere $v$ in $S$ als besucht.
   3. Setze dann initial $L_v := D_v$, also auf die DFS Zahl von $v$ selbst.
   4. Dann, fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Sei $e := (v, w)$.
	  2. Falls $w$ bereits eine DFS Zahl hat:
		 1. Falls $w$ auch bereits eine Finish Zahl hat, so ist $(v, w)$ eine
            Vorwaerts kante und somit uninteressant.
		 2. Sonst muss $(v, w)$ eine Rueckwaertskante sein. Gib $e$ also in
            den Stack $T$. Falls $L_w$ kleiner $D_v$ ist, setze $D_v := L_w$ (da
            wir offensichtlich eine kleinere DFS-Zahl erreichen koennen).
	  3. Sonst muss $e$ eine Baumkante sein. Gib $e$ auf den Stack und
         rekursiere fuer $w$.
	  4. Falls nach der Rekursion gilt, dass $L_w < L_v$, so aktualisiere $L_v := L_w$.
      5. Gilt aber $L_w \geq D_v$, so kann $w$ keine DFS Zahl vor der von $v$
         erreichen:
		 1. Dann ist $v$ also ein Artikulationsknoten fuer den Graphen, der in
            $w$ gewurzelt ist.
		 2. Sei nun $B$ der neue Block und initial leer.
		 3. Poppe solange Kanten vom Stack und gib sie in $B$, bis auch $e$ in
            $B$ ist (einschliesslich $e$!)
		 4. Fuege $B$ in die Menge aller Bloecke $\mathbb{B}$ ein.
3. Ergaenze $\mathbb{B}$ noch um alle isolierten Knoten.

Es sei angemerkt, dass die durch diesen Algorithmus entstehenden Bloecke
lediglich eine Partition der Kanten, nicht aber der Knoten darstellt. Ein
einzelner Knoten kann in mehreren Bloecken vorkommen. Auch beobachten wir, dass
sich die Menge von Blocken $\mathbb{B}$ zusammensetzt aus:

1. Den Zweifachzusammenhangskomponenten,
2. Den Brueckenknoten, die also ueber eine Bruecke verbunden sind.
3. Den isolierten Knoten (Schritt (3)).

### Starke Zusammenhangskomponenten

Wir hatten vorhin den Begriff der Zusammenhangskomponenten fuer ungerichtete
Graphen beschrieben und diskutiert. Bei gerichteten Graphen nennt man
zusammenhaengende Teilgraphen nun *stark zusammenhaengend*. In einem gerichteten
Graphen nennt man also eine Knotenteilmenge $U \subseteq V$ genau dann *stark
zusammenhaengend*, wenn fuer alle Knoten $u,v \in U$ zumindest ein gerichteter
Pfad von $u$ nach $v$ existiert. Analog wie bei gerichteten Graphen nennt man
dann einen Teilgraphen *stark zusammenhaengend* (*strongly-connected component*;
*SCC*), wenn die Knoten des Teilgraphen stark zusammenhaengend sind und der
Teilgraph maximal ist.

Wir interessieren uns wieder dafuer, die starken Zusammenhangskomponenten eines
gerichteten Graphen zu finen. Zunaechst machen wir aber eine Beobachtung:
Vereinigt man alle Knoten von starken Zusammenhangskomponenten zu grossen
Knoten, so ergibt sich ein DAG zwischen diesen "Superknoten". Dieser Graph hat
beispielsweise zwei starke Zusammenhangskomponenten

```
o<--     -->o
|  |     |  |
|  o---->o  |
v  |     |  v
o-->     <--o
```

und kann "geschrumpft" werden zu:

```
O---->O
```

Die Idee fuer den Algorithmus, um die starken Zusammenhangskomponenten zu
finden, ist es nun, anfangs jeden einzelnen Knoten in eine eigene starke
Zusammenhangskomponente zu geben. Dann fuegen wir nach und nach Kanten aus dem
urspruenglichen Graphen ein und aktualisieren dann einfach die noch bestehenden
SCCs. Es gibt dann drei moegliche Konsequenzen fuer die SCCs des Graphen
bzw. den geschrumpften Graphen:

* Die Kante verbindet Knoten, die bereits zur selben SCC gehoeren. Dann bleiben
  die SCCs unveraendert.
* Die Kante verbindet Knoten aus zwei verschiedenen SCCs, schliesst aber keinen
  Kreis. Dann bleiben die SCCS unveraendert, da man zwar aus der einen SCC jeden
  Knoten in der anderen erreichen kann, aber das Symmetrische nicht der Fall
  ist. Im geschrumpften Graphen haetten wir aber eine neue Kante zwischen zwei
  Superknoten.
* Die Kante verbindet Knoten aus zwei verschiedenen SCCs und schliesst dann
  einen oder viele Kreise. Dann werden alle SCCs, die nun durch die Kreise
  verbunden und somit symmetrisch erreichbar sind, zu einer neuen
  Zusammenhangskomponente verbunden.

Im Weiteren beobachten wir, dass es in einem Graphen sets drei Arten von SCCs
gibt: *unentdeckte*, *offene* und *geschlossene*. *Unentdeckte* SCCs sind jene
einzelne Knoten (jeder Knoten ist anfangs eine eigene SCC), die wir noch gar
nciht betrachtet haben. Somit enthaelt der Graph initial also nur solche
unentdeckten SCCs. *Offene* SCCs sind hingegen solche, die noch mindestens einen
aktiven Knoten enthalten. Hierbei nennen wir einen Knoten aktiv bzw. offen, wenn
er zwar schon entdeckt ist und eine DFS Zahl, aber noch keine Finish Zahl
hat. Letztlich gibt es noch *geschlossene* SCCs, die nur inaktive Knoten haben,
wo also jeder Knoten eine Finish Zahl hat. Knoten in offenen oder geschlossenen
SCCs nennen wir dann ebenso offen oder geschlossen. Eine wichtige Definition ist
noch die des *Repraesentanten* einer SCCS. Wir muessen also immer einen Knoten
waehlen, der seine ganze SCC eindeutig identifiziert. Hierfuer waehlen wir
gerade jenen Knoten, den wir in der SCC als erstes besuchen. Dieser hat also
dann die kleinste DFS Zahl.

Wir koennen nun drei Invarianten fuer unseren Graphen vermerken, die also
waehrend des Algorithmus immer gelten muessen:

1. Pfade aus geschlossenen SCCs fuehren immer zu geschlossenen SCCs. Fuehrt
   naemlich ein Pfad aus einer SCC $A$ in eine aktive SCC $B$, so kann
   insbesondere jener Knoten aus $A$ der die beiden Komponenten verbindet, noch
   nicht geschlossen sein. Somit muesste auch $A$ noch aktiv sein.
2. Fuer einen beliebigen Schritt einer Tiefensuche enthaelt der aktuelle
   Rekursionsstack, also der Pfad zum momentanen Knoten, genau die
   Repraesentanten aller offenen SCCs.
3. Da die offenen Knoten im momentanen Pfad nach DFS Zahl aufsteigend sortiert
   sind und die Repraesentanten jeweils die Grenze zweier SCCs definiert, werden
   die Knoten bezueglich ihrer DFS Zahl durch die Repraesentanten in offene SCCs
   partioniert.

Invarianten (2) und (3) helfen uns nun bei unserem Algorithmus, die offenen SCCs
zu verwalten. Wir koennen naemlich den momentanen Pfad durch einen expliziten
Stack $\mathcal{N}$ darstellen, auf welchem wir die offenen Knoten aufsteigend,
bezueglich ihrer DFS Zahl sortiert, sammeln. Wenn wir dann noch einen zweiten
Stack $\mathcal{R}$ haben, der die Repraesentanten der SCCs im momentanen Pfad
enthaelt, so kennen wir durch diese Repraesentanten auch die Grenzen der SCCS
(Invariante (3)). Wir koennen unseren Algorithmus also bei der Wurzel beginnen
und dann ledglich den Wurzelknoten in sowohl $\mathcal{N}$ und $\mathcal{R}$
geben.

Der Algorithmus unterscheidet im Weiteren dann danach, wie man mit bestimmten
Kanten umgeht. Sei hierfuer $e = (v, w)$ eine Kante. Dann unterscheiden wir: Ist
$w$ ein unentdeckter (neuer) Knoten (ohne DFS Zahl), so gib $w$ auf
$\mathcal{N}$ und $\mathcal{R}$. Ist $w$ hingegen ein Knoten in einer
geschlossenen SCC, so kann es laut Invariante (1) keine Rueckwaertskante aus der
SCC von $w$ in jene von $v$ geben. Somit wuerde diese Kante $e$ auch keinen
Kreis schliessen, womit wir sie ignorieren koennen. Letztlich gibt es noch den
Fall, dass $w$ ein Knoten in einer offenen SCC ist, ungleich jener von $v$. So
muss dies eine Rueckwartskante sein, da $w$ noch nicht fertig ist. Insbesondere
gibt es also neben dieser Kante $e$ noch einen momentanen Pfad von $w$ nach $v$,
sodass $e$ einen Kreis schliesst. Dann sind also alle Knoten in der SCC von $w$
und $v$, sowie alle SCCs im Pfad von $w$ nach $v$ (auf dem Stack), durch diesen
Kreis verbunden und bilden eine gemeinsame SCC.

Neben diesen Kanten gibt es noch den Fall, dass wir mit einem Knoten fertig
werden. Falls der Knoten dann ein Repraesentant einer SCC ist, so muss nicht nur
der Knoten, sondern die ganze SCC geschlossen werden, da der Repraesentant immer
der erste Knoten einer SCC ist, den wir besuchen.

Der ganze Algorithmus laeuft dann wie folgt ab:

1. Sei:
   * $\mathcal{N}$ der Stack offener Knoten.
   * $\mathcal{R}$ der Stack offener Repraesentanten.
   * $D$ die Struktur, die die DFS Zahlen der Knoten sammelt.
   * $F$ die Struktur, die die Finish Zahlen der Knoten sammelt.
   * $Z$ die Struktur, die jedem Knoten einen ID seiner Zusammenhangskomponten
     zuteilt.
   * $z$ ein Zaehler fuer die Zusammenhangskomponenten IDs.
2. Fuer jeden Knoten $v$, der in $Z$ noch keine Komponenten ID hat:
   1. Da wir nur fuer Baumkanten rekursieren werden, heisst das, dass wenn wir
      am Anfang dieser Funktion sind, dies entweder der Wurzelknoten einer
      Komponente ist oder die letzte Kante eine Baumkante war.
   2. Gib $v$ also auf $\mathcal{N}$,
   3. Gib $v$ auf $\mathcal{R}$,
   4. Setze $Z_v = D_v$, da $v$ nun eine neue eigenstaendige SCC startet.
   5. Dann fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Falls $w$ schon eine DFS Zahl hat, so muss $e := (v, w)$ eine
         Vorwaertskante oder Rueckwaertskante sein.
		 1. Falls $w$ schon eine Finish Zahl hat, so ist $w$ also
            geschlossen. Nach Invariante (1) kann es keinen Pfad on $w$ nach $v$
            geben, sodass $e$ einen Kreis schliessen wuerde. Ueberspringe $w$
            also.
		2. Ansonsten ist $e$ eine Rueckwaertskante und schliesst also einen
           Kreis. Dann muessen alle SCCS auf dem Pfad von $w$ zu $v$ geschlossen
           werden. Die Sortierung und Partionierung der Knoten in den Stacks
           erlaubt uns nun so vorzugehen:
		   1. Solange, wie der Repraesentant auf dem Stack $\mathcal{R}$ noch
              eine hoehere DFS Zahl hat als $w$, poppe $\mathcal{R}$.
		   2. Das geht also solange, bis wir bei der SCC von $w$ sind, sodass
              der Repraesentant der SCC von $w$ der neue Repraesentant aller
              SCCs wird, die auf diesem Kreis lagen.
	  2. Falls $w$ noch keine DFS Zahl hat, muss $e$ eine Baumkante
         sein. Rekursiere also fuer $w$.
   6. Sind wir mit dem Knoten $v$ fertig, muessen wir pruefen, ob $v$ ein
      Repraesentant ist. Dann wuerde das Schliessen von $v$ naemlich das
      Schliessen der ganzen SCC, die $v$ repraesentiert bedeuten. Falls $v$ also
      Repraesentant ist:
	  1. Gib $v$ vom Stack $\mathcal{R}$.
	  2. Solange wie $w := \mathcal{N}.\pop$ nicht $w$ ist, kommt $w$ also nach
         $v$. Setze $Z_w = z$ und inkrementiere $z$.

Da wir nichts Weiteres als DFS machen, laeuft dieser Algorithmus in $O(n + m)$
Zeit.

## Shortest Path Tree

Ein sehr bekanntes Graphenproblem ist natuerlich das Finden des kuerzesten Weges
von Knoten $A$ nach $B$. Bei ungewichteten Graphen haben wir hier schon die
Breitensuche kennengelernt, die uns den Shortest Path Tree (SPT) liefert. In der
Realitaet sind Kanten aber meist sehr wohl gewichtet, da die Gewichtungen
bzw. *Kosten* zum Beispiel Distanzen in Strassennetzwerken darstellen. Wir
wollen hier nun Algorithmen betrachten, die in gewichteten Graphen die
kuerzesten Wege finden.

### Topologische Sortierung

Der erste Algorithmus, den wir fuer das Kuerzeste Wege Problem untersuchen
wollen, ist die topologische Sortierung. Die topologische Sortierung eines
Graphen ordnet die Knoten so aufsteigend an, dass wenn man linear durch diese
Sortierung durch geht, man einen Knoten immer erst dann erreicht bzw. besucht,
wenn man zuvor alle seine Abhaengigkeiten, also Vorgaenger, besucht
hat. Betrachten wir zunaechst den Algorithmus, um die topologische Sortierung zu
bestimmen. Auf Basis dieses Algorithmus koennen wir leicht den SPT finden.

Es gibt zwei alternative Algorithmen um eine Sequenz $T$ zu finden, die jedem
Knoten $v$ seinen Rang $T_v$ in der topologischen Sortierung zuzuzweisen, sodass
man dadurch eine Permutation der Knotenmenge $V$ erstellen kann, die die
natuerliche Ordnung der Knoten bezueglich der topologischen Sortierung
wiederspiegelt. Die erste Idee ist es, explizit fuer jeden Knoten zu speichern,
wieviele eingehende Kanten er hat, also den Indegree $\indeg(v)$ bzw. die
"Abhaengigkeiten". Jedes Mal, wenn wir einen Nachbarn eines Knoten betrachten,
dekrementieren wir diese Zahl. Sinkt sie auf Null, so haben wir also alle
Vorgaenger abgearbeitet und duerfen nun den neuen Knoten bearbeiten. Das wuerde
folgenden Algorithmus liefern:

1. Sei:
   * $T$ die Struktur, die den topologischen Rang $T_v$ jedes Knoten $v$
     speichert.
   * $t$ ein Zaehler fuer den topologischen Rang und initial Null.
   * $C$ die Struktur, die die Anzahl an Vorfahren jedes Knoten speichert und
     entsprechend initialisiert ist.
2. Dann, ausgehend von einem Wurzelknoten $v$, rekursiere ueber eine
   Tiefensuchen:
   1. Da wir zuerst alle Vorgaenger von $v$ abgearbeitet haben muessen, um $v$
      ueberhaupt bearbeiten zu duerfen, muessen wir uns keine Gedanken darueber
      machen, dass wir $v$ mehrmals besuchen koennten. Denn wir duerfen ihn ja
      erst fuer die letzte Kante ueberhaupt besuchen. Wir brauchen also keinen
      Vermerk irgendwo, dass wir $v$ jetzt besuchen.
   2. Setze $T_v = t$ und inkrementiere $t$.
   3. Fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Dekrementiere dessen Abhaengigkeitszaehler $C_w$.
	  2. Falls $C_w$ nun Null ist, so duerfen wir $w$ besuchen. Rekursiere also
         fuer $w$.
	  3. Ansonsten duerfen wir $w$ noch nicht besuchen, gehe also weiter.


Die alternative Variante der topologischen Sortierung basiert auf der
Beobachung, dass die Sequenz der topologischen Raenge (Zahlen) $T$ gerade die
umgekehrte Sequenz der Finish Zahlen $F$ ist. Betrachten wir fuer den Beweis
davon einen Quellknoten $s$ und einen Zielknoten $t$. Wir bemerken, dass die
Finish Zahlen von $s$ nach $t$ gerade die umkehrten Finish Zahlen sind, wenn wir
von $t$ nach $s$ ueber denselben Pfad gehen, nur rueckwarts (und die
Kantenrichtungen etnsprechend umdrehen). Es gilt also, dass $F^{(s,t)}_v = n -
F^{(t,s)}_v$ wo $n$ die Anzahl an Knoten ist. Nun machen wir folgende tolle
Beobachtung bezueglich diesen umgekehrten Zahlen: Wir werden auf dem Weg von $t$
nach $s$ genau dann mit einem Knoten fertig, wenn wir alle seine Nachfahren
abgearbeitet haben. Genau dann erhaelt $v$ also seine Finish Zahl
$F^{(t,s)}_v$. Im Vorwaertsgraphen sind die Kanten umgekehrt. Dann erhaelt $v$
im Graphen von $s$ nach $t$ also genau dann eine Finish Zahl $F^{(t,s)}_v$, wenn
alle Vorgaenger von $v$ abgearbeitt wurden. Somit muss $F^{(t,s)}_v = T_v$
sein.

Ein alternativer Beweis basiert auf der formalen Definition der topologischen
Sortierung in einem DAG. Sei hierufer $e = (v,w)$ eine Kante. Wir wissen
bereits, dass in einem DAG dann $F_v > F_w$ und $D_v < D_w$ gelten muss. Fuer
die topologischen Zahlen gilt dann in einem DAG noch zusaetzlich $T_v <
T_w$. Der topologische Rang jedes Quellknoten muss natuerlich kleiner sein, also
der topologische Rang jeder seiner Nachfahren. Nun stellen wir die Vermutung
auf, dass die Finish Zahlen $F$ umgekehrt die topologischen Zahlen $T$ ergeben,
sodass $T_v = n - F_v$ gilt. Wir wissen bereits, dass $F_v > F_w$ gelten
muss. Dann muesste also folgen, dass $n - F_v < n - F_w$ ist, da $F_v$ eine
groessere Zahl ist, als $F_w$. Laut Annahme waere dann $T_v < T_w$, was gerade
die Bedingung fuer korrekte topologische Zahlen ist.

Der alternative Algorithmus, um die topologische Sortierung zu bestimmen, findet
also einfach die Finish Zahlen und dreht sie um.

#### Shortest Paths

Durch topologische Sortierung erhalten wir eine Permutation der Knotemenge $v$,
sodass eine sequentielle Abarbeitung der Knoten in Reihenfolge dieser
topologischen Ordnung die Abahaengigkeiten der Knoten respektiert. Wir besuchen
jeden Knoten genau erst dann, wenn wir alle Kanten zu diesem Knoten betrachtet
haben. Sagen wir nun, diese Kanten $e$ seien durch $\mathcal{W}(e)$
gewichtet. Seien dann $K_v$ die akkumulierten Kantenkosten von einem
Wurzelknoten $s$ zu diesem Knoten $v$. Sei dann $v$ ein noch unbesuchter Knoten
im Graphen. Wir koennten nun folgendes tun: jedes Mal, wenn wir eine Kante $e =
(w, v)$ nach $v$ betrachten, aktualisieren wir die akkumulierten Kosten zu
$v$. Hierbei pruefen wir, ob $K_w + \mathcal{W}(e)$ kleiner als die bisher
gespeicherten akkumulierten Kosten $K_v$ fuer $v$ sind. Falls ja, setzen wir
$K_v := K_w + \mathcal{W}(e)$. Da wir topologisch sortieren, kommen wir erst
wirklich zu $v$, wenn wir die letzte Kante $e = (w, v)$ traversieren. Das
bedeutet, dass wenn wir bei $v$ ankommen, wir schon alle moeglichen Pfade vom
Wurzelknoten $s$ zu $v$ ausprobiert haben muessen. Somit muss $v$ induktiv zu
diesem Zeitpunkt die minimalen Kosten haben, da auch alle vorherigen Knoten $w$
erst dann ueberhaupt bearbeitet worden waeren, wenn $K_w$ schon aus dem Minimum
der akkumulirten Kosten der Vorfahren von $w$ hervorgegangen waere. Somit muss
$K_v$ fuer beliebige $v$ minimal sein, wodurch der SPT folgt. Formal lautet der
Algorithmus:

1. Sei:
   * $K$ die Struktur, die die momentanen akkumulierten Kosten zu jedem Knoten
     $v$ speichert. Initialisiere $K$ lazy oder setze alle $K_v = \infty$.
   * $P$ eine Struktur, die den Vorgaenger (Parent) jedes Knoten
     speichert. Initialisiere $P_v = v$.
   * $s$ ein Wurzelknoten.

2. Setze $K_s = 0$ und finde die topologische Permutation $V_T$ bezueglich
   dieses Wurzelknoten $s$.
3. Dann iteriere ueber $V_T$. Sei $v$ der momentane Knoten, dann:
   1. Fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Sei $e = (v, w)$ die entsprechende Kante.
	  2. Falls $K_w + \mathcal{W}(e) < K_v$:
		 1. Aktualisiere $K_v := K_w + \mathcal{W}(e)$.
		 2. Setze $w$ als den Vorgaenger von $v$: $P_v = w$.
	  3. Ansonsten ignoriere die Kante.
4. Sei $S$ der Shortest Path Tree, den wir zurueckgeben wollen. Initialisiere
   diesen mit alle Knoten aus $V$, aber keinen Kanten.
5. Dann iteriere ueber $P$ (ausser dem Wurzelknoten) und fuege fuer jeden
   Eintrag $P_v$ eine Kante $(P_v, v)$ in $S$ ein.
6. Gib $S$ zurueck.

Dieser Algorithmus laeuft in $O(n + m)$ (Tiefensuche).

### Zyklische Graphen

Soeben haben wir die Topologische Sortierung als Loesung des SPT Problems
kennengelernt. Jedoch haben wir auch die Annahme getroffen, dass der gegebene
Graph ein DAG, also insbesondere azyklisch ist. Das ist natuerlich nicht immer
der Fall. Wir wollen jetzt Algorithmen wie den von Dijkstra oder Bellman-Ford
betrachten, die auch in zyklischen Graphen kuerzeste Wege finden koennen. In der
Realitaet, beispielsweise in Strassennetzen, findet man zyklische Graphen sehr
haeufig, weswegen diese Diskussion mit Sicherheit sinnvoll ist.

#### Dijkstra

Als erstes wollen wir den beruehmten Algorithmus von Dijkstra untersuchen, der
das SPT Problem fuer zyklische Graphen loest. Wir muessen aber hier auch wieder
eine Annahme machen. Wir nehmen an, dass keine der Kantengewichte *negativ*
sind.

Die Idee dieses Algorithmus ist, eine Priority Queue (Heap) zu verwenden und
darin die Knoten sowie ihre assoziierten akkumulierten Kosten zu
speichern. Anfangs initialisieren wir diese Queue nur mit dem Startknoten und
Kosten Null. Dann iterieren wir solange, bis die Queue leer ist. In jeder
Iteration ziehen wir den Knoten mit den kleinsten akkumulierten Kosten aus der
Queue und aktualisieren wie beim auf topologischer Sortierung basierendem
Algorithmus die adjazenten akkumulierten Kosten in der Queue. Das essentielle
ist es nun, dass wenn wir einen Knoten bearbeitet haben, wir diesen niewieder
betrachten muessen. Der Grund wieso, ist gerade unsere Annahme: Kantenkosten
sind nie negativ. Was das insbesondere bedeutet, ist dass wenn wir bei einem
Knoten $v$ stehen, so koennen die Kosten von $v$ durch weitere Kanten (Kreise)
die zurueck zu $v$ fuehren, nur steigen und niemals sinken.

Sei dann $v$ also der Knoten, der momentan die kleinsten akkumulierten Kosten
vom Startknoten hat. Betrachten wir nun alle jene Knoten $w$, zu welchen $v$
ausgehende Kanten hat. Ist ein solcher Knoten $w$ momentan auch in der Queue, so
muss $w$ hoehere akkumulierte Kosten haben als $v$. Sonst haetten wir $w$ aus
der Queue genommen, nicht $v$. Da die Kosten der Kante zu $w$ nicht-negativ sind
auch jeder Pfad von $v$ zu $v$ ueber $w$ nicht-negativ sein muss, wird kein
solcher Pfad ueber einen Nachbarn $w$, der in der Queue ist, einen kuerzeren
Pfad zu $v$ liefern.

Betrachten wir dann alle Knoten $u$, zu welchen $v$ adjazent ist und die nicht
in der Queue sind. Dann gibt es zwei Faelle, wieso das so ist. Ist ein
adjazenter Knoten $u$ nicht in der Queue, weil er noch gar nicht betrachtet
wurde, so ist $u$ sicher zu keinem Knoten adjazent, der schon bearbeitet wurde
(und nach Annahme kleinere Kosten hatte als $v$). Denn sonst waere $u$ in der
Queue. Damit $u$ also in diesem Fall nicht in der Queue ist, muss dieser Knoten
zu einem der $w$ adjazent sein, die in der Queue sind. Eben haben wir
beobachtet, dass Pfad durch einen solchen Knoten $w$ einen kuerzeren Pfad zu $v$
liefert.

Es bleiben noch alle Knoten $u$, die schon in der Queue waren und schon
bearbeitet wurden. Diese muessen wir eigentlich nicht mehr betrachten. Wir haben
schon, was wir wollten: Es existiert kein Knoten, der noch nicht bearbeitet
wurde, ueber welchen ein Pfad von $v$ zurueck zu $v$ billigere Kosten liefern
wuerde, als die momentan akkumulierten Kosten von $v$. Somit koennen wir nach
Bearbeitung von $v$ sicher sein, dass wir $v$ nie wieder aktualisieren muessten
und die Aussage folgt.

Wir koennen aber noch analysieren, ob wir wirklich den kuerzesten Pfad vom
Startknoten $s$ zu $v$ gefunden haben, wenn wir $v$ fertig bearbeitet
haben. Hierfuer muessen wir ledglich jene Knoten $u$ betrachten, die schon vor
$v$ bearbeitet wurden und somit nicht mehr in der Queue sind. Genauer geneugt
es, nur die Knoten $u'$ zu betrachten, die direkte Vorfahren von $v$ sind, und
zu zeigen dass der momentane Vorgaenger $w$ von $v$ auch sicher der beste
Vorgaenger fuer $v$ ist. Nehmen wir an, unter allen verbundenen Knoten $u'$
gaebe es einen Knoten $w'$, der kuerzere Kosten zu $v$ liefern wuerde. Da wir
$w'$ schon bearbeitet haben, haetten wir dann $w'$ als Vorgaenger von $v$
gewaehlt und nicht $w$, im Widerspruch dazu, dass $w$ der Vorgaenger
ist. Induktiv ergibt sich dann, dass auch der Vorgaenger von $w$ der geeignetste
ist und somit muss nach selber Argumentation, iterativ angewendet, der ganze
Pfad von $s$ nach $v$ der kuerzest Moegliche sein.

Der Algorithmus ist also korrekt. Wir formalisieren ihn nun wie folgt:

1. Sei:
   * $Q$ die Priority Queue mit Schluesseln $(v, k)$ fuer Knoten $v$ und
     akkumulierten Kosten $k$.
   * $K$ die Struktur, die die momentanen akkumulierten Kosten $K_v$ fuer einen
     Knoten $v$ speichert.
   * $P$ die Struktur, in welcher wir den Vorgaenger $P_v$ eines Knoten $v$
     vermerken.
   * $\mathcal{W}$ die Funktion, die Kanten auf ihre Kosten abbildet.
   * $s$ der Startknoten des Algorithmus.
2. Gib $(s, 0)$ in die Queue $Q$ und setze $K_s = 0$ sowie $P_s = s$.
3. Solange, wie die $Q$ nicht leer ist:
   1. Setze $v := Q.\pop$.
   2. Fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Sei $k := K_v + \mathcal{W}(e)$ wo $e = (v, w)$.
	  2. Betrachten wir $w$ zum ersten Mal, so setze gleich $K_w = k$, $P_w =
         v$ und gib $(w, k)$ in die Queue.
	  3. Ansonsten pruefe, ob $k < K_w$:
		 1. Falls ja, setze $K_w = k$ und $P_w = v$.
		 2. Ansonsten ueberspringe $w$.
4. Sei $S$ der Shortest Path Tree, den wir zurueckgeben wollen. Initialisiere
   diesen mit alle Knoten aus $V$, aber keinen Kanten.
5. Dann iteriere ueber $P$ (ausser dem Wurzelknoten) und fuege fuer jeden
   Eintrag $P_v$ eine Kante $(P_v, v)$ in $S$ ein.
6. Gib $S$ zurueck.

##### Analyse

Die Komplexizitaet des Dijkstra Algorithmus haengt stark von der verwendeten
Priority Queue ab. Insbesondere benoetigen wir oft die `decreaseKey` Operation,
um akkumulierte Kosten zu verringern. Bei normalen Binary Heaps hat diese
Operation logarithmische Laufzeit. Es gibt aber eine andere Art von Heap,
genannt *Fibonacci Heap*, die diese Operation in amortisiert konstanter Zeit
erlaubt. Ebenso ist das Einfuegen konstant. Lediglich die `deleteMin` Operation
benoetigt dann logarithmische Zeit. Da wir im Laufe des Dijkstra Algorithmus
alle $n$ Knoten einmal in der Queue haben werden und entsprechend rausholen
muessen, ergeben sich so eine Laufzeit von $O(n \log n)$. Dazu kommen noch
konstante `decreaseKey` Operationen, die diese Laufzeit zu $O(m + n \log n)$
erhoehen. Bei normalen Binary Heaps waere dies $O((m + n) \log n)$, da die
`decreaseKey` Operarionen ebenso logarithmisch in $n$ waeren.

Es gibt aber noch eine weitere Queue, die fuer den Dijkstra Algorithmus sehr
geeignet ist. Diese Datenstruktur nennt man *Bucket Queue* und erlaubt sogar
konstante Operationen. Hierfuer muessen wir aber annehmen, dass die
Kantengewichte maximal $C$ gross sein koennen, also jeweils im Intervall
$[0, C]$ liegen. Dann wird diese Queue als ein Array von $C + 1$ Eintragen
implementiert, die die $C + 1$ Restklassen $0, ..., C$ modulo $C + 1$
darstellen. Ein Knoten mit momentaner akkumulierten Kosten $K_v$ liegt dann in
der verketteten Liste bei $K_v \mod C + 1$. Das besondere ist nun, dass alle
Knoten in einem Bucket dieser Liste die exakt selbe akkumulierten Distanz
haben. Betrachten wir hierfuer einen Knoten $v$ in Bucket $B_i := K_v \mod C +
1$. Es gibt zwei Moeglichkeiten, sodass ein Knoten $w \neq v$ auch in diesem
Bucket waere. Entweder, $K_w = K_v$, dann passt das. Oder aber, wenn nicht, so
muss $K_w \equiv_{C + 1} K_v$ sein. Minimal meusste gelten, dass $K_w = K_v +
C + 1$. Es gaebe also einen Knoten $u$ mit selben Kosten wie $v$
(notwendigerweise) sowie eine Kante $e = (u, w)$ mit Kosten $C + 1$. Wir wissen
aber, dass die Kosten jeder Kante im Intervall $[0, C]$ liegen. Damit $w$ also
diese Kosten haben kann, muss es notwendigerweise einen Knoten $x$ zwischen $u$
und $w$ geben, der Kosten im Intervall $[0, C]$ hat. Da wir von $u$ ausgehen
muss $K_x$ auch groesser $K_v$ sein. Dann haetten wir aber $x$ sicher noch nicht
bearbeitet. Somit kann $w$ sicher nicht in der Queue sein. Es sind also
tatsaechlich nur solche Knoten in einem Bucket, die die exakt selben (absoluten)
akkumulierten Kosten vom Startknoten $s$ haben.

Ueberlegen wir uns nun, wie teuer Operationen auf dieser Bucket-Queue waeren. Da
wir in jederm Eintrag der Queue Listen speichern, waere das Einfuegen eines
neuen Knoten konstant. Sofern wir einen Handle auf einen Knoten $v$ in der Queue
haben, koennen wir fuer `decreaseKey` diesen Eintrag aus dem Bucket einfach
loeschen und den Knoten mit den neuen Kosten in den anderen Bucket
einfuegen. Dies waere also auch konstant. Letztlich bleibt noch
`deleteMin`. Wenn wir einen Zeiger auf das Minimum speichern, koennen wir das
kleinste Element also schnell loeschen. Dann bleibt es nur noch, die neuen
kleinsten Kosten zu finden. Hierfuer gehen wir vom alten Bucket solange nach
rechts, bis wir einen nicht-leeren Bucket finden. Das muessen die neuen
kleinsten Kosten sein und wir geben den Zeiger auf diesen Bucket. Das waer also
in $O(C)$. Somit reduziert sich die Laufzeit des Dijkstra Algorithmus auf $O(m +
Cn)$. Fuer kleine $C$ koennen wir $C$ dann auch fallen lassen.

#### Bellman-Ford

Beim Dijkstra Algorithmus haben wir die Annahme gemacht, dass Kantenkosten nicht
negativ sind. Fuer manche Anwendungen machen negative Kosten aber durchaus
Sinn. Negative Kosten sind insbesondere deswegen schwierig, da sie *negative
Kreise* erlauben. Ein negativer Kreis ist ein Kreis in einem Graphen, dessen
Gewichtssumme negativ ist. Man koennte diesen Kreis also beliebig oft
durchlaufen, und jedes mal billigere Kosten erhalten. Somit haette jeder Knoten
in diesem Kreis sowie jeder Knoten, der von diesem Kreis aus erreichbar ist,
letztendlich unendlich-negative Kosten:

```
     _o_
    /   \
   2     -7
  /       \
 o-- -1 --o-- 0 --o
```

Der Bellman-Ford Algorithmus ist gewissermassen Brute-Force. Wir gehen einfach
$n-1$ mal alle Kanten im Graphen durch und aktualisieren die akkumulierten
Kosten jedes Knoten. Somit haetten wir am Ende von jedem Knoten aus zu jedem
anderen die minimalen akkumulierten Kosten propagiert:

```
o-- 1 --o-- -1 --o-- 5 --o
```

Fuer Pfade sowie allgemein Baeume ist das leicht zu sehen. Wir koennen uns auch
allgemein ueberlegen, dass ein Knoten $v$ von einem anderen Knoten $w$ maximal
$n - 1$ Kanten weg sein kann (bei einem Pfad ist das nur explizit so). Somit
haetten wir die Kosten von $v$ nach diesen $n - 1$ Iterationen sicher nach $w$
propagiert. Das interessante ist nun aber, dass wir nach diesen $n - 1$
Iterationen herausfinden koenenn, *ob es einen negativen Kreis* im Graphen
gibt. Iterieren wir naemlich noch ein $n$-tes Mal ueber alle Kanten und gibt es
dann noch immer moegliche Gewicht-Updates, so muss es einen negativen Kreis
geben. Denn dann muesste es im obigen Beispiel eine Kante von $w$ nach $v$ oder
einem Knoten $u$ nach $v$ (also im Pfad nach $v$) geben, sodass man diesen
Knoten $u$ noch updaten koennte. Waere dieser Kreis, der durch diese von $w$
ausgehende Kante geschlossen wird, naemlich nicht negativ, so wuerde er kein
Update der akkumulierten Kosten von $u$ veranlassen.

Um dann zu signalisieren, dass wir einen negativen Kreis gefunden haben, muessen
wir die Kosten aller Knoten, die ueber diesen negativen Kreis erreicht werden
koennen, auf $-\infty$ setzen. Hierfuer setzen wir einfach bei der letzten
($n$-ten) Iteration fuer jeden Knoten, den man noch aktualisieren koennte, eine
Tiefensuche an um die Kosten aller erreichbaren Knoten entsprechend anzupassen.

Das ergibt folgenden Algorithmus:

1. Sei:
   * $P$ die Datenstruktur, wo wir den Vorgaenger $P_v$ jedes Knoten $v$
     vermerken.
   * $K$ die Datenstruktur, die die akkumulierten Kosten $K_v$ jedes Knoten $v$
     speichert.
   * $\mathcal{W}$ die Funktion, die Kanten auf ihre Kosten abbilden.
   * $s$ der Wurzelknoten.
2. Initialisiere $P_s = s$ und $K_s = 0$.
3. Iteriere $n - 1$ mal auf folgende Weise:
   1. Fuer jede Kante $e = (v, w)$ im Graphen:
	  1. Falls $v$ noch nie betrachtet wurde, ist $K_v$ quasi noch
         unendlich. Ueberspringe also $v$ (wir muessen warten bis Distanzen zu
         $v$ propagieren).
	  2. Setze $k = K_v + \mathcal{W}(e)$.
	  3. Gilt $k > K_w$, so ueberspringe $w$.
	  4. Ansonsten setze $P_w = v$ und $K_w = k$.
4. Gehe nun in die $n$-te Iteration. Fuer jede Kante $e = (v, w)$ im Graphen:
   1. Setze $k = K_v + \mathcal{W}(e)$.
   2. Gilt $k > K_w$, so ueberspringe $w$.
   3. Kann man die Kosten $K_w$ von $w$ durch $k$ noch aktualisieren, gibt es
      einen negativen Kreis, der $w$ enthaelt. Dann:
	  1. Setze $P_w = v$ und rekursiere in einer Tiefensuche. Sei hierzu $u$ der
         Rekursionsknoten. Dann:
		 1. Gilt $K_u = -\infty$, so haben wir $u$ schon angepasst. Gehe also
            zurueck. Ansonsten:
		 2. Setze $K_u = -\infty$.
		 3. Rufe diese Funktion rekursiv fuer alle Nachbarn von $u$ auf.

Da wir $n$ mal ueber alle $m$ Kanten rekursieren, ergibt das eine Gesamtlaufzeit
von $O(m \cdot n)$. Um die genauen Zyklen zu bestimmen, koennten wir von einem
Knoten $v$ mit $K_v = -\infty$ durch die Struktur $P_v$ iterieren, bis wir
wieder bei $v$ sind. Dann haben wir den gesuchten Kreis.

Es gibt nun noch eine Optimierung des Bellman-Ford Algorithmus, der die
erwartete Laufzeit zu $O(m + n)$ verbessern kann, wobei die Hoechstlaufzeit
gleich der des urspruenglichen Algorithmus bleibt. Diese Variante implementieren
wir wie eine Breitensuche, wobei wir Knoten aber auch wieder in die Queue geben,
wenn wir sie schon einmal besucht haben:

1. Sei:
   * $Q$ eine Queue und initial mit dem Startknoten $s$ befuellt.
   * $P$ die Datenstruktur, wo wir den Vorgaenger $P_v$ jedes Knoten $v$
     vermerken.
   * $K$ die Datenstruktur, die die akkumulierten Kosten $K_v$ jedes Knoten $v$
     speichert.
   * $\mathcal{W}$ die Funktion, die Kanten auf ihre Kosten abbilden.
   * $s$ der Wurzelknoten.
   * $c$ ein Zaehler und initial Null.
2. Solange, wie die Queue $Q$ nicht leer ist und $c < n$:
   1. Setze $v := Q.\pop$
   2. Fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Setze $k = K_v + \mathcal{W}(e)$.
	  2. Gilt $k > K_w$, so ueberspringe $w$.
	  3. Ansonsten setze $P_w = v$ und $K_w = k$ und gib $w$ in die Queue.
   3. Inkrementiere $c$.
3. Fahre fort wie bei dem urspruenglichen Algorithmus.

## All Pairs Shortest Path

Der Shortest Path Tree findet von einem Wurzelknoten aus die kuerzesten Pfade zu
jedem anderen Knoten im Graphen. Was aber, wenn wir die kuerzesten Pfade von
jedem Knoten zu jedem anderen Knoten kennen wollen? Das nennt man das *All Pairs
Shortest Paths* (*APSP*) Problem.

Die erste Idee, die uns hierfuer einfallen koennte ist es, einfach $n$ Mal fuer
jeden Knoten Bellman Ford auszufuehren. Das hat aber natuerlich keine
vorteilhafte Laufzeitkomplexizitaet, sondern laeuft in $O(n \cdot n \cdot
m)$. Eine bessere Strategie waere es, den schnelleren der uns bekannten SPT
Algorithmen zu verwenden: Dijsktra. Leider funktioniert dieser aber nur fuer
Graphen ohne negative Kanten. Wenn es aber irgendwie moeglich waere, negative
Kante allesamt nicht-negativ zu machen, dann koennten wir Dijkstra anwenden.

#### Gewichtanpassung

Wir wollen nun untersuchen, wie man negative Kantenkosten aufloesen
kann. Hierbei nehmen wir an, dass der urspruengliche Graph zwar negative Kanten
haben kann, aber *keine negativen Kreise*. Die erste Moeglichkeit, die uns in
den Kopf fallen koennte, ist es, einfach auf alle Kanten $-\min_e
\mathcal{W}(e)$ auf alle Kantengewichte draufzuaddieren. Da eine Addition aber
quasi keine lineare Funktion ist, koennte das Pfade verfaelschen. Hat der
kuerzeste Pfad naemlich $k$ Knoten, so wird $k$ mal dieser Wert draufaddiert. So
koennte ein anderer Pfad, der kuerzer ist aber urspruenglich teurer war,
ploetzlich billger werden. Der Graph waere also verfaelscht.

Stattdessen definieren wir nun das Konzept des *Knotenpotentials*. Hierfuer
betrachten wir eine Funktion $\varphi: V \rightarrow \mathbb{R}$, die jeden
Knoten auf ein *Potential* abbildet. Dann definieren wir noch eine neue Funktion
$\hat{\mathcal{W}}$, die jeden Kante $e = (v, w)$ auf seine angepassten
Kantenkosten abbildet durch:

$$\hat{\mathcal{W}}(e) = \varphi(v) + \mathcal{W}(e) - \varphi(w).$$

Wir zeigen zunaechst, dass diese Funktion $\hat{\mathcal{W}}$ die Laenge der
Pfade im Graphen nicht verfaelscht. Seien hierzu $p,q$ zwei verschieden Pfade
von $v$ nach $w$ im Graphen und $\mathcal{W}(p), \mathcal{W}(q)$
bzw. $\hat{\mathcal{W}(p)}, \hat{\mathcal{W}(q)}$ die Summe aller Kantenkosten
auf diesem Pfad. Dann muss fuer jedes beliebige Potential $\varphi$ gelten:

$$\hat{\mathcal{W}(p)} < \hat{\mathcal{W}(q)} \iff \mathcal{W}(p) <
\mathcal{W}(q)$$

Sei hierfuer $p = (v_1, ..., v_k)$ ein beliebiger Pfad und $e_i$ definiert als
die $i$-te Kante $(v_i, v_{i + 1})$ in diesem Pfad. Dann gilt:

$$
\begin{align}
	\hat{\mathcal{W}}(p) &= \sum_{i=1}^{k - 1} \hat{\mathcal{W}}(e_i)\\
	                     &= \sum_{i=1}^{k - 1} (\varphi(v_i) + \mathcal{W}(e) +
						 \varphi(v_{i + 1}))\\
						 &= \varphi(v_1) + \mathcal{W}(p) - \varphi(v_k)
\end{align}
$$

Wie wir sehen bleiben in dieser Summe genau das Potential des ersten und letzten
Knoten sowie die urspruenglichen Pfadkosten uebrig. Das wichtige ist nun, dass
fuer jeden solchen Pfad $p$ zwischen $v$ und $w$ diese beiden Terme gerade
konstant sind. Somit bestimmen lediglich die urspruenglichen Kantenkosten die
Relation zweier angepasserter Pfade, wodurch keine Verfaelschung stattfinden
kann.

Nun zeigen wir, dass wenn wir das Potenial $\varphi(v)$ gerade als die Distanz
$K_{s,v}$ von irgendeinem Wurzelknoten $s$ waehlen, die angepassten Kosten
$\hat{\mathcal{W}}(e)$ fuer jede Kante $e$ nicht negativ sind. Wir nehmen
hierbei an, dass es keine negative Kreise gibt. dann gilt sicher:

$$K_{s,v} + \mathcal{W}(e) \geq K_{s,w}$$

D.h. die Kosten von $s$ nach $v$ plus die Kosten einer Kante nach $w$ muss immer
groesser gleich $K_{s,w}$ sein, da $K_{s,w}$ gerade das Minimum dieser Kanten
waehlen wuerde. Wenn wir nun umformen:

$$
\begin{align}
	K_{s,v} + \mathcal{W}(e) &\geq K_{s,w}\\
	K_{s,v} + \mathcal{W}(e) - K_{s,w} &\geq 0\\
	\varphi(v) + \mathcal{W}(e) - \varphi{w} &\geq 0\\
	\hat{\mathcal{W}}(e) &\geq 0
\end{align}
$$

Es ist also moeglich, jeden beliebgien Graphen mit negativen Kanten aber ohne
negative Kreise in einen Graphen mit nicht negativen Kanten umzuformen, wenn man
das Knotenpotential $\varphi(v)$ jedes Knoten als die Distanz von einem fixen
Wurzelknoten $s$ waehlt und die neuen Kantenkosten $\hat{\mathcal{W}}(e)$ als
$\varphi(v) + \mathcal{W}(e) - \varphi(w)$ waehlt.

#### Johnson Algorithmus

Der Johnson Algorithmus nutzt die obige Gewichtanpassung, um das APSP Problem zu
loesen. Die Idee ist es, einen virtuellen Startknoten $s$ zum Graphen
hinzuzufuegen und jeden Knoten im Graphen mit $s$ zu Kosten Null zu
verbinden. Dann definieren wir wie oben das Knotenpotential $\varphi(v) =
K_{s,w}$. Es sei angemerkt, dass dann nicht fuer jeden Knoten $K_{s,w} = 0$
gelten wird, weil es ja negative Kanten innerhalb der Knoten geben kann, die
$K_{s,v}$ nach unten ziehen. Die erste Stufe dieses Algorithmus findet also mit
Bellman-Ford die Distanzen $K_{s,v}$ jedes Knoten $v$ von $s$ aus.

Mit $\hat{\mathcal{W}}$ hat man dann keine negativen Kantenkosten im Graphen
mehr. Wir koennen nun also Dijkstra verwenden, um von jedem Knoten aus den SPT
zu berechnen. Dann muessen wir nur noch die Kantenkosten zurueckrechnen. Wir
wissen, dass fuer jede Kante $e = (v, w)$ gilt:

$$\hat{K}_{v,w} = \hat{\mathcal{W}}(e) = \varphi(v) + \mathcal{W}(e) -
\varphi(w) .$$.

Wobei $K_{v,w} = \mathcal{W}(e)$. Somit gilt nach Umformung:

$$K_{v,w} = \mathcal{W}(e) = \varphi(w) + \mathcal{W}(e) -
\varphi(v) .$$.

Wir koennen dann also in einer $n \times n$ Matrix die kuerzesten Pfade von
jedem Knoten zu jedem anderen Knoten vermerken. Die Laufzeit hiervon ist $O(mn +
n^2 n \log n)$. Bei wenigen Knoten und vielen Kanten ist dieser Algorithmus also
besser als der simple $n$-fache Bellman-Ford.

## Minimum Spanning Tree

Neben dem Shortest Path Tree (SPT) gibt es noch eine zweite Art von Baum, den
wir aus einem Graphen oft induzieren wollen: *Minimum Spanning Trees*
(*MST*). Das sind jene Baeume, die alle Knoten im Graphen verbinden, sodass die
Summe aller Kantenkosten minimal ist. Dieser Baum wuerde also von einem
beliebigen Knoten aus immer den kleinsten Pfad zu jedem anderen Knoten
ermoeglichen. Hierbei sei aber angemerkt, dass dieser Pfad selbst nicht der
kleinste sein muss, den wir aus einem SPT erfahren haetten. Die besondere
Eigenschaft des MST ist es einfach, dass wir von einem *beliebigen* Knoten
ausgehen koennen und dennoch (quasi "im Mittel") die niedrigsten Kosten
erhalten.

Wir betrachten bei diesen Problemen im Allgemeinen ungerichtete Graphen.

#### Kruskal

Der Algorithmus von Kruskal ist der erste den wir fuer das MST Problem
betrachten wollen. Seine Idee ist simpel: sortiere die Kanten aufsteigend. Nehme
dann den Graphen anfangs ohne Kanten und fuege nach und nach die kleinsten
Kanten in diesen Graphen ein, die noch neue Zusammenhangskomponenten bilden. Tue
dies solange, bis man den gesamten Graphen verbunden hat (er zusammenhaengend
ist). Da man immer die kleinste Kante gewaehlt hat, um zwei Komponenten zu
verbinden, muessen die Kanten im ganzen graphen insgesamt am kleinsten
sein. Das ergibt folgenden Algorithmus:

1. Sei:
   * $M$ der momentane Spannbaum, der initial alle Knoten von $G$ enthaelt aber
     keine Kanten.
2. Setze $S := \sort(E)$ als die sortierten Kanten.
3. Iteriere aufsteigend ueber jede Kante $e = (v, w) \in S$:
   1. Falls $u,v$ bereits in einer Zusammenhangskomponente sind, so ueberspringe
      $e$.
   2. Ansonsten vereinige die Zusammenhangskomponenten $Z_v$ und $U_w$ von $v$
      und $w$ und gibt $e$ in den MST $M$.
4. Gib $M$ zurueck.

Alternativ kann man auch mit dem vollstaendigen Graphen beginnen, die Kanten
sortieren und dann absteigend alle Kanten loeschen, die keine Bruecken
sind. Somit geht diese Variante sogar In-Place.

Wir merken, dass dieser Algorithmus grundsaetzlich sehr einfach ist. Seine
Laufzeit haengt aber insbesondere sehr stark davon ab, wie schnell wir
Zusammenhangskomponenten vereinigen koennen. Das ist aber ein klassisches
*Union-Find* Problem. Wir betrachten bei diesem Problem disjunkte Mengen von
Elementen, die jeweils durch einen Repraesentanten (Parent) identifiziert
sind. Dann wollen wir durch die `union` Operation zwei solcher Mengen vereinigen
koennen (Zusammenahngskomponenten verschmelzen) und via `find` die
Zugehoerigkeit eines Elements (Knoten) zu seiner Menge (Zusammenhangskomponente)
bestimmen.

Diese Idee kann man ganz einfach mit einer Sequenz $R$ implementieren, die die
Repraesentanten $R_v$ jedes Knoten $v$ speichert. Hierbei setzen wir initial
$R_v = v \forall v$. Im Weiteren wird sich der Repraesentant eines Knoten
natuerlich (eventuell) veraendern. Dann implementieren wir die `find` Operation
einfach, indem wir von $R_v$ ausgehend solange rekursiv durch $R$ iterieren, bis
wir bei einem Element sind, das sich selbst als Repraesentant hat. Dast ist dann
der Reprasentant von $v$, den wir zuruckgeben koennen. Das essentielle ist nun
jedoch, dass prinzipell jede zusaetzliche Iteration um diesen Repraesentanten zu
finden unnoetig ist. Am besten waere es, wennn $R_v$ direkt auf den
Repraesentanten zeigen wuerde. Deswegen koennen wir, nachdem wir den
Repraesentanten gefunden haben, $R_v$ direkt auf diesen zeigen lassen:

1. Sei $v$ der Knoten, fuer welchen wir einen Repraesentanten finden wollen.
2. Setze $v' := v$. Dann, solange wie $R_{v'} \neq v'$:
   1. Setze $v' = R_{v'}$.
3. Setze $R_v = v'$.
4. Gib $v' = R_v$ zurueck.

Die Union Operation koennen wir ebenso einfach implementieren. Seien hierzu
$v,w$ zwei knoten. Grundsaetzlich genuegt es, die Repraesentanten $R_v, R_w$
beider Knoten zu finden und auf Gleichheit zu pruefen. Falls sie nicht gleich
sind, setze $R_v := R_w$. Das ist die naive Variante. Wir haben aber schon
bermerkt, dass die Laufzeit von `find` und somit von `union` von der Hoehe jedes
Baumes abhaengt (der implizit in $R$ lebt). Somit ist es sinnvoll, bei der
Vereinigung zweier Komponenten den kleineren an den hoeheren zu heften. Sonst
wuerde man den hoeheren Baum wiederum um eine Kante hoeher machen. Eine
Erweiterung von `union` speichert daher in einer Struktur $H$ die Hoehe jedes
Baumes und entscheidet auf Basis der Hoehe, ob $R_v := R_w$ oder $R_w := R_v$
gesetzt wird. Waren die Hoehen der beiden Baeume gleich, so muss man die Hoehe
jenes Baumes, an welchen man den anderen angebunden hat, noch vergroessern. Da
wir Komponenten nie trennen, muss man die Hoehe auch nirgends mehr
dekrementieren. Das ergibt folgende Implementierung fuer `find`:

1. Sei:
   * $R$ die Struktur, die den Repraesentant $R_v$ jedes Knoten speichert.
   * $H$ die Struktur, die die Hoehe $H_v$ jedes Baumes speichert, dessen Wurzel
     der Knoten $v$ ist.
   * $v,w$ zwei Knoten.
2. Gilt $R_v = R_w$, so gibt es nicht zu tun, gehe also zurueck.
3. Ansonsten, falls $H_v < H_w$, setze $R_v := R_w$.
4. Falls $H_v > H_w$, setze $R_w := R_v$.
5. Ansonsten gilt $H_v = H_w$. Dann ist uns die Wahl mehr oder weniger
   frei. Setze also einfach $R_v := R_w$ und inkrementiere $H_w$.

Das ergibt nun den folgenden Algorithmus von Kruskal:

1. Sei:
   * $M$ der momentane Spannbaum, der initial alle Knoten von $G$ enthaelt aber
     keine Kanten.
2. Setze $S := \sort(E)$ als die sortierten Kanten.
3. Iteriere aufsteigend ueber jede Kante $e = (v, w) \in S$:
   1. Falls $\find(v) = \find(w)$, ueberspringe diese Kante.
   2. Ansonsten rufe $\union(v, w)$ und gib $e$ in den MST $M$.
4. Gib $M$ zurueck.

Man kann zeigen, das `find` und `union` mehr oder weniger konstant sind. Somit
ergibt sich die Laufzeit des Algorithmus von Kruskal rein aus dem Sortieren von
Kanten, also $O(m \log m)$.

#### Prim

Ein weiterer Algorithmus fuer das MST Problem ist der *Algorithmus von
Prim*. Dieser funktioniert sehr aehnlich wie jener von Dijkstra. Anstelle der
akkumulierten Kosten speichern wir jetzt aber fuer jeden Knoten $v$ in der Queue
die Kosten der kleinsten Kante nach $v$. Wenn wir dann den Knoten aus der Queue
nehmen, auf welchen die momentan kleinste Kante zeigt, aktualisieren wir die
Kosten der Kanten aller adjazenten Knoten entsprechend. Somit bleibt wie bei
Dijkstra die Laufzeit bei $O(m + n \log n)$. Der Algorithmus lautet dann wie
folgt:

1. Sei:
   * $Q$ die Priority Queue mit Schluesseln $(v, k)$ fuer Knoten $v$ und
     kleinsten Kantenkosten $k$.
   * $K$ die Struktur, die die kleinsten momentanen Kantenkosten $K_v$ fuer
     einen Knoten $v$ speichert.
   * $P$ eine Struktur, in welchem wir uns den Vorgaenger jedes Knoten merken.
   * $M$ der momentane Spannbaum, der initial alle Knoten von $G$ enthaelt aber
     keine Kanten.
   * $\mathcal{W}$ die Funktion, die Kanten auf ihre Kosten abbildet.
   * $s$ ein beliebiger Startknoten fuer den Algorithmus.
2. Gib $(s, 0)$ in die Queue $Q$ und setze $P_s = s$.
3. Solange, wie die $Q$ nicht leer ist:
   1. Setze $v := Q.\pop$.
   2. Gib die Kante $(P_v, v)$ in den Spannbaum $M$.
   2. Fuer jeden zu $v$ adjazenten Knoten $w$:
	  1. Sei $\mathcal{W}(e)$ wo $e = (v, w)$.
	  2. Betrachten wir $w$ zum ersten Mal, so setze $P_w := v$, $K_w :=
         \mathcal{W}(e)$ und gib $(w, \mathcal{W}(e))$ in die Queue.
	  3. Ansonsten pruefe, ob $k < K_w$:
		 1. Falls ja, setze $K_w = k$ und $P_w = v$.
		 2. Ansonsten ueberspringe $w$.
4. Gib $M$ zurueck.
