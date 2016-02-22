# Graphentheorie

Graphen sind Diagramme mit:

* Knoten (Vertices)
* Kanten (Edges)

Hierbei ist die Topologie (Nachbarschaftsbeziehungen) wichtiger als die
Positionen von Knoten im Raum oder die Laenge von Kanten.

## Graphentypen

### Ungerichteter Graph

Ein (einfacher) ungerichteter Graph $G = (V, E)$ ist ein Graph mit Knotenmenge
$V$ und Kantenmenge $E$. Hierbei gilt, dass $E$ eine Menge von zwei-elementigen
Mengen ist. Jede dieser zwei-elementigen Mengen stellt dabei eine Kante, also
eine Verbindung zwischen zwei Knoten $v_1,v_2 \in V$ dar.

Da diese zwei-elementigen Mengen eben Mengen sind, und nicht Tupel, kann es
keine Kante zwischen einem Knoten und sich selbst geben. Es gibt also keine
*Schleifen*. Weil dann auch die Menge der Kanten selbst eine Menge ist, kann es
auch keine Kanten mehrmals geben. Es gibt also keine *Mehrfachkanten*. Diese
Eigenschaften werden erst durch einen *verallgemeinerten ungerichteten Graphen*
behandelt (siehe unten).

Beispiel: $G = (V, E)$ mit:

$V = \{v_1, v_2, v_3, v_4, v_5\}$
$E = \{\{v_2, v_3\},\{v_2, v_4\},\{v_3, v_4\},\{v_4, v_5\}\}$

Ein Graph besteht allgemein aus $|V|$ Knoten und $|E|$ Kanten. Aber wieviele
ungerichtete (einfache) Graphen kann es mit einer Knotenmenge $V$ maximal geben?
Wir bemerken hierbei, dass ein Graph $|V|$ Knoten haben kann, und diese immer
haben muss. Aber die *Kantemenge* kann variieren. Maximal kann es (in einem
*vollstaendigen Graphen*) ${|V| \choose 2}$ Kanten geben. Jede Kante kann dann
einmal enthalten sein oder, nicht. Folglich gibt es $2^{{|V| \choose 2}}$
moegliche Kantenmengen auf $|V|$ Knoten.

### Verallgemeinerter ungerichteter Graph

Ein verallgemeinerter ungerichteter Graph ist ein Graph $G = (V,E)$, dessen
Kantemenge $E$ nun eine __Multimenge__ von 2-elementigen __Multimengen__ ist. Da
die Kantemenge $E$ selbst eine Multimenge ist, kann sie Mehrfachkanten
enthalten. Da jede zwei-elementige Menge, die eine einzige Kante darstellt, nun
eine Multimenge ist, kann sie auch den selben Knoten zweimal enthalten. Also
sind auch Schleifen moeglich.

### Gerichtete Graphen

* Ein Graph $G = (V, E)$
* $E$ enthaelt nun Tupel $(v_1, v_2)$ wo $v_1$ gerichtet mit $v_2$ verbunden ist
  ($(v_1, v_2) \neq (v_2, v_1)$)
* $E \subseteq V \times V$
* $E$ selbst ist noch immer eine __Multimenge__.
* Selbstschleifen sind nun erlaubt selbst wenn $E$ keine Multimengen enthaelt.

### Hypergraph

Ein Hypergraph ist ein Graph, dessen Kanten auch mehr als zwei Knoten verbinden
koennen.

* $G = (V, E)$
* $E \subseteq 2^V$
* Die Elemente von $E$ sind *Hyperkanten* und koennen mehrere Knoten verbinden
* Z.B.: $V = \{\{v_1, v_2, v_3\}\}$ (dreifach Verbindung)

### Vollstaendige Graphen

In einem vollstaendigen Graphen, in dem alle Knoten miteinander verbunden sind,
ist die Anzahl an Kanten: $|E| = {n \choose 2} = \frac{n(n - 1)}{2}$. Man hat
also quadratisches Wachstum von Kanten. Ein vollstaendiger Graph hat nur eine
Zusammenhangskomponente.

* Knoten: $|V|$
* Kanten: ${|V| \choose 2}$

Man schreibt einen vollstaendigen Graphen ueber $|V|$ Knoten auch als ${V
\choose 2}$.

### Binaerer Hyperwuerfel

Ein binaerer Hyperwuerfel ist ein Wuerfel, dessen Knoten binaere Folgen einer
bestimmten Laenge $n$ sind. $n$ nennt man hierbei die *Dimension* des
Hyperwuerfels, und folglich ist $Q_n$ der Hyperwuerfel mit Dimension $n$.

Die Knotenmenge ist also die Menge aller Bit-Vektoren mit $n$ Stellen: $V = \{0,
1\}^n$. Zwei Knoten sind in einem solchen binaeren Hyperwuerfel genau dann ueber
eine Kante verbunden, wenn ihre Bit-Vektoren *benachbart* sind. Hier wiederum
gilt, dass zwei Bit-Vektoren genau dann benachbart sind, wenn sie sich in nur
einem Bit unterscheiden. Man sagt dann auch, dass ihr *Hamming-Abstand*, die
Zahl, die angibt, wieviele Bits zwischen zwei Bit-Vektoren verschieden sind,
genau $1$ ist. In $010$ und $111$ sind beispielsweise zwei Bits veschieden, also
ist ihr Hamming-Abstand gleich $2$. Sie waeren also nicht durch eine Kante
verbunden. Die Knoten $000$ und $001$ jedoch schon, weil sie *benachbart* sind.

Da es $2^n$ verschiedene Bit-Vektoren auf $n$ Bits gibt, hat ein
$n$-dimensionaler binaerer Hyperwuerfel auch $2^n$ Noten. So ein Hyperwuerfel
hat dann $n \cdot 2^{n - 1}$ Kanten. Das folgt daraus, dass es fuer jeden $2^n$
Kanten $n$ benachbarte Kanten gibt (jeder der $n$ Bits kann sich unterscheiden).
Zaehlt man jetzt aber $2^n \cdot n$, dann zaehlt man jede Kante zweimal, also
muss man durch zwei dividieren: $\frac{n \cdot 2^{n}}{2} = n \cdot 2^{n - 1}$.

Obige Aussage folgt auch aus dem Handshaking Theorem: Wenn wir sagen, jeder der
$2^n$ Knoten hat $n$ Kanten, zaehlen wir die Grade. Also gilt natuerlich, dass
die Summe der Grade gleich $2|E|$ ist. Somit muessen wir diese Summe also
halbieren.

* Knoten: $2^n$
* Kanten: $n \cdot 2^{n-1}$

### Bipartiter Graph

Ein bipartiter Graph ist einer, dessen Knotenmenge $V$ aus zwei verschiedenen
Mengen $V_1,V_2 \subseteq V$ besteht. Die besondere Eigenschaft des bipartiten
Graphen ist dann, dass eine Kante immer einen Knoten aus der einen Menge mit
einem Knoten aus der anderen Menge verbindet, aber niemals zwei aus der selben
Menge ($V_1$ oder $V_2$).

Allgemein ist ein multipartiter bzw. $k$-partiter Graph einer, wo man die Menge
an Kanten $V$ in $k$ Klassen unterteilen kann und es niemals eine Kante im Graph
gibt, die eine Kante aus einer Klasse mit einer anderen Kante aus der selben
Klasse verbindet. Ein solcher $k$-partiter Graph ist dann vollstaendig, wenn es
zu jedem Knoten in jeder Klasse eine Kante zu jedem anderen Knoten in jeder der
anderen Klasse gibt. Deswegen ist ein bipartierter Graph schon vollstaendig,
wenn es fuer jeden Knoten eine Kante zu jedem Knoten in der anderen Klasse
gibt. Sonderfaelle sind dann $1$-partite Graphen. Das sind dann Graphen ohne
Kanten.

Jeder Graph der $k$-partit ist, ist auch $k + 1$-partit. Denn wenn es schon
keine Kante von jedem Knoten in die selbe Menge gibt, sondern nur in die andere,
dann kann man diese andere Menge auch noch weiter partitionieren. Jeder Graph
ist daher $|V|$-partit.

## Weg, Pfad, Kreis

### Weg

Ein *Weg* der Laenge $k$ in einem Graphen $G = (V,E)$ ist eine Folge von Knoten
$(v_0, ..., v_k)$ so, dass $\{v_i, v_{i+1}\} \in E$ fuer alle $i = 0, ..., k -
1$. Man bemerke, dass in dieser Folge Knoten auch mehrmals vorkommen duerfen
(diese Eigenschaft wird erst durch *Pfade* beseitigt). Somit kann ein Weg also
auch Kreise enthalten. $\{v_0\}$ ist dabei ein Weg der Laenge $0$. Um die Laenge
eines Weges zu bestimmen, sollte man also *die Kanten zaehlen*.

### Pfad

Ein (einfacher) Pfad in einem Graphen $G$ ist ein Weg, in dem alle Knoten
(paarweise) verschieden sind. Es darf in einem Pfad also nunmehr keinen Kreis
geben, was in einem *Weg* ja noch erlaubt war.

Man bemerke, dass wenn es einen Weg gibt, es immer auch einen (nicht laengeren)
Pfad gibt. Enthaelt der Weg keinen Kreis, ist der Weg auch schon ein Pfad und
die Aussage gilt. Enthaelt der Weg aber einen Kreis, dann gibt es in der
Knotenfolge zumindest einen Knoten $v$, der zwei mal vorkommt. Man kann dann
drei Faelle unterscheiden:

1. Der Kreis ist am Anfang, sodass die Knotenfolge $(v,...,v,...,v_k)$ ist.
   Dann ist der Pfad jene Folge, die erst beim zweiten $v$ beginnt.

2. Der Kreis ist in der Mitte, sodass die Knotenfolge
   $(v_0,...,v,...,v,...,v_k)$ ist. Dann gibt es einen Knoten $w$, bei welchem
   wir beim ersten Ankommen bei $v$ so abzweigen, dass wir wieder zu $v$
   gelangen, und beim zweiten mal so abzweigen, dass wir zum Endknoten $v_k$
   gelangen. Der Pfad ist dann jene Folge, die schon beim ersten mal so
   abzweigt, dass wir zu $v_k$ gelangen, also gar nie einen Kreis produziert.

3. Der Kreis ist am Ende, sodass die Knotenfolge $(...,v,...,v)$ ist. Dann ist
   der Pfad jene Folge, die schon beim ersten $v$ aufhoert.

### Kreis

Ein (einfacher) Kreis der Laenge $k$ in $G$ ist ein Weg der Laenge $k$, in dem
sich nach $k$ __Kanten__ ein Knoten wiederholt. Der Weg ist also eine Folge
$(v_0,...,v_k)$ wo gilt: $v_0 = v_k$.

Eine Eigenschaft von Kreisen ist, dass sie fuer $n$ Knoten immer $n$ Kanten
haben. Die Anzahl von Kanten zu Knoten waechst also linear bzw. asymptotisch
gleich.

In einem Kreis mit $n$ Knoten ist der laengste Weg zwischen zwei Knoten immer
$n/2$. Denn wenn man ueber die $n/2 + 1$-ten Kante auch noch geht, haette man in
die andere Richtung des Kreises nur $n/2 - 1$ Kanten ueberqueren muessen.

## Teilgraphen

Ein Graph $H = (V_H, E_H)$ heisst *Teilgraph* eines Graphen $G = (V_G, E_G)$
wenn die Menge seiner Knoten Teilmenge der Knoten von $G$ ist, und die Menge
seiner Kanten Teilmenge der Kanten von $G$ ist:

$V_H \subseteq V_G$ und $E_H \subseteq E_G$.

### Induzierte Teilgraphen

Ein Teilgraph heisst *induziert*, wenn jeder Knoten im Teilgraph die *selben
Kanten hat wie der selbe Knoten im Urgraph*. Das heisst, will man aus einem
Graphen einen Teilgraphen konstruieren und einen Knoten aus dem Urgraphen nicht
im Teilgraphen haben, so muss man den Knoten mitsamt __allen__ Kanten
entfernen. Man darf also nicht nur eine einzige Kante entfernen, oder gar eine
Kante hinzufuegen.

Formal ist ein induzierter Teilgraph $H = (V_H, E_H)$ eines Graphen $G = (V_G,
E_G)$ ein Graph, wo gilt:

$E_H = E_G \cap {V_H \choose 2}$.

${V_H \choose 2}$ ist hierbei die Menge an Kanten von $H$, wenn dieser
vollstaendig waere. Wenn man nun also einen Knoten aus dem Urgraphen uebernimmt
aber nicht alle dazugehoerigen Kanten, dann wird diese Kante dennoch in dieser
Menge enthalten sein und beim Schneiden mit $E_G$ erhalten bleiben. Die daraus
enstandene Menge wuerde also $\neq E_H$ sein, und der Teilgraph wuerde nicht
induziert sein. Ist der Teilgraph aber induziert, so wird fuer jeden Knoten in
$H$ die Kantenmenge gleich sein wie in $G$. Der Schnitt aus $E_G$ und ${V_H
\choose 2}$ wuerde also genau $E_H$ ergben.

## Nachbarschaften

Sei $G = (V, E)$ ein einfacher Graph. Ist dann $e = \{u,v\} \in E$ eine Kante,
dann gilt:

* Die Knoten $u$ und $v$ nennt man *adjazent*.

* Die Knoten $u$ und $v$ nennt man die *Endknoten* von der Kante $e$.

* Die Kante $e$ nennt man *inzident* zu $u$ und $v$.

Die Nachbarschaft $\Gamma(v)$ von einem Knoten $v$ ist nun die Menge der
adjazenten Knoten $u \in V$ wo $\{v, u\} \in E$. Wieviele Nachbarn, also wie
viele ausgehende(/eingehende, egal bei ungerichtetem Graph) Kanten ein konkreter
Knoten $v$ nun besitzt, wird durch den *Grad* $\deg(v)$ des Knoten $v$
angegeben. Der Grad beschreibt also genau die Kardinalitaet der
Nachbarschaftsmenge:

$\deg(v) =$ die Anzahl an adjazenten Knoten $= |\Gamma(v)|$.

### Maximal- und Minimalgrad

Ist $G = (V, E)$ ein einfacher Graph, so sind:

* $\Delta(G)$ der *Maximalgrad* von $G$: $\max\{\deg(v) \,| \,v \in V\}$.

* $\delta(G)$ der *Minimalgrad* von $G$: $\min\{\deg(v) \,|\, v \in V\}$.

### Gradfolge

Die Gradfolge eines Graphen mit Knotenmenge $V = \{v_1,...,v_n\}$ ist das Tupel
der Grade aller Knoten: $(\deg(v_1), ..., \deg(v_n)$.

Haben alle Knoten eines Graphen denselben Grad $k$, dann nennt man diesen
Graphen $k$-regulaer. Alle Werte in der Gradfolge des Graphen waeren also
gleich.

### Handshaking Theorem

Das Handshaking Theorem gibt eine schoene, einfache und intuitive Beziehung
zwischen den Graden der Knoten eines Graphen, und der Kardinalitaet der
Kantenmenge des Graphen:

$$\sum_{v \in V} \deg(v) = 2 |E|$$

Die Richtigkeit dieses Satzes liegt darin, dass wir auf der linken Seite jede
Kante zweimal zaehlen, naemlich einmal fuer jeden Grad zweier adjazenter
Knoten. Auf der rechten Seite zaehlen wir die Kanten auch zweimal.

#### Korrolar

Ein Korrolar des Handshaking Theorems ist, dass die Anzahl an ungeraden Graden
in einem Graphen immer *gerade* sein muss. Denn aus dem Handshaking Theorem
wissen wir, dass die Summe aller Grade gerade sein muss. Da eine ungerade Zahl
mal eine ungerade Zahl wieder eine ungerade Zahl gibt, kann es einen Graphen mit
einer ungeraden Anzahl an Knoten mit ungeradem Grad *nicht geben*.

Frage: Existiert der Graph mit Gradfolge $(1,1,1,3,5,6,7,9)$?
Antwort: Nein, weil die Anzahl an ungeraden Graden ungerade ist.

"[...] in a party of people some of whom shake hands, an even number of people
must have shaken an odd number of other people's hands."
(https://en.wikipedia.org/wiki/Handshaking_lemma)

## Erreichbarkeit

Ein Knoten $u \in V$ ist erreichbar von $v \in V$, wenn es einen *Pfad* $(u,
..., v)$ in $G$ gibt. Es gibt also eine Erreichbarkeitsrelation $R_G \subseteq V
\times V$, sodass $(u, v) \in R_G$ gdw. $v$ ist erreichbar von $u$. Des Weiteren
gilt, dass $R_G$ sogar eine Aequivalenzrelation ist:

* Reflexiv: Jeder Knoten ist von sich selbst erreichbar.

* Transitiv: Gibt es einen Pfad von $u$ nach $v$, und von $v$ nach $w$, dann
             gibt es also einen Pfad von $u$ nach $w$, ueber $v$. Also koennen
             knoten auch transitiv erreichbar sein.

* Symmetrisch: Wenn der Graph ungerichtet ist, gilt, das wenn $u$ von $v$
               erreichbar ist, $v$ ueber denselben Weg auch von $u$ erreichbar
               ist.

Die Erreichbarkeitsrelation ist somit auch die reflexiv-transitive Huelle der
Kantenmenge: $R_G = E^\star$.

### Zusammenhangskomponenten

Die Aequivalenzklassen der Aequivalenzrelation "erreichbar" sind allesamt
*induzierte* Teilgraphen des Graphen $G$. Diese Teilgraphen nennt man
*(Zusammenhangs-)Komponenten* von $G$. Eine sehr wichtige Eigenschaft von
Komponenten ist:

__In einer Komponente ist jeder Knoten von jedem anderen Knoten erreichbar.__

Ein ganzer Graph $G$ heisst dann *zusammenhaengend*, wenn er nur aus *einer
einzigen (zusammenhaengenden) Komponente* besteht, sodass jeder Knoten im Graph
von *jedem anderen* Knoten im Graphen erreichbar ist.

* Zusammenhaengend $\Leftrightarrow$ $1$ Komponente
* Nicht Zusammenhaengend $\Leftrightarrow$ mehr als $1$ Komponente!

### Eigenschaft

Jeder Graph $G = (V, E)$ enthaelt mindestens $|V| - |E|$ Komponenten.

Intuition: Man stelle sich $V$ Knoten ohne Kanten vor. Dann hat man $|V| - 0 =
|V|$ Komponenten. Dann gibt man eine Kante hinzu, und man hat nun aus zwei
Komponenten eine gemacht, also hat man noch $|V| - 1$ Komponenten. Das kann man
nun maximal $|E|$ mal machen, dann hat man $|V| - |E|$ Komponenten.

#### Beweis

Mittels wohlfundierter Induktion:

Basis: Sei $|E| = 0$, dann gilt: $|V| - |E| = |V|$ Komponente.

Schritt: Sei $G = (V, E)$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer alle Teilgraphen des Graphen $G$.

	Behauptung: Dann gilt sie auch fuer $G$.

Beweis:

	1. Wir haben also einen Graphen $G = (V, E)$. Nun entfernen wir eine Kante
       und erhalten daraus einen Teilgraphen von $G$. Laut Induktionsannahme
       gilt die Aussage fuer diesn Teilgraphen, also hat der resultierende
       Graph: $|V| - (|E| - 1)$ Komponenten.

	2. Nun geben wir die Kante wieder hinzu. Wenn die Kante keine Bruecke war,
       veraendert sich die Anzahl an Komponenten im Vergleich zum Teilgraphen
       nicht. War die Kante aber eine Bruecke, so reduziert sich die Anzahl an
       Komponenten um $1$. Somit hat man *mindestens*:
	   $|V| - (|E| - 1) + 1 = |V| - |E|$
	   Komponenten, aber eben moeglicherweise mehr, wenn die Kante keine Bruecke
       war(*mindestens*).

### Eigenschaft

Fuer jeden *zusammenhaengenden* Graphen gilt: $|E| \geq |V| - 1$. Man braucht
also mindestens so viele Kanten, um einen Graphen zusammenhaengend zu malen.

Intuitiv: Die einfachste (und minimalste) Art, aus $V$ Knoten einen
zusammenhaengenden Graphen zu machen ist, sie in einer Kette aufzureihen und sie
mit Kanten zu verbinden. Dann benoetigt man genau $|V| - 1$ Kanten. Man kann
dann aber auch zusaetzliche Kanten hinzufuegen (daraus folgt $\geq |V| - 1$),
diese veraendern die Eigenschaft aber nicht.

Mathematisch: Aus $|V| - |E| = 1$ folgt $|E| \geq |V| - 1$.

Frage: Gibt es einen zusammenhaengenden Graphen mit Gradfolge
       $(3,3,2,2,2,1,1,1,1,1,1)$?

Antwort: Nein, weil $\sum_{v \in V} deg(v) = 2|E| \Rightarrow |E| =
        \frac{\sum_{v \in V} deg(v)}{2}$, also $|E| =
        \frac{3+3+2+2+2+1+1+1+1+1+1}{2} = 9$, und es gilt $|E| \geq |V| - 1$
        fuer einen zusammenhaengenden Graphen, aber $|V| = 11$ und $|E| < 10$.

### Satz von Havel-Hakimi

Der Satz von Havel-Hakimi sagt, dass es nur dann einen Graphen $G$ mit
sortierter (nicht-absteigend oder nicht-ansteigend) Gradfolge $(d_1, d_2,...,
d_n)$ *gibt*, wenn es einen Graphen $G'$ gibt mit Gradfolge:

$\{d_2 - 1, ..., d_{d_1 + 1} - 1, d_{d_1 + 2}, ..., d_n\}$ gibt.

Die Grade $d_2$ bis $d_1 + 1$ sind hierbei die obersten (nach Sortierung) $d_1$
Knoten. Der Satz faehrt hier so fort, dass er den Knoten mit hoechstem Grad
$d_1$ entfernt. Dieser war mit $d_1$ weiteren Knoten verbunden, also werden die
Grade der naechsten Knoten auch verringert. Es gibt dann also mindestens noch
diese Gradfolge.

#### Algorithmus

Der Algorithmus zu diesem Satz laesst sich so beschreiben:

1. Sei $(d_1,...,d_n)$ eine sortierte Reihenfolge.

2. Ist einer der Grade negativ, gibt es den Graphen nicht. Terminiere.

3. Wenn alle Grade null sind gibt es den Graphen. Er hat keine Kanten. Terminiere.

4. Ansonsten reduziere die Gradfolge nach obigem Schema, also ziehe von den
   ersten $d_1$ Graden jeweils $1$ ab. Gibt es weniger als $d_1$ Grade in der
   Gradfolge, gibt es den Graphen nicht. Terminiere.

5. Sortiere die resultierende Gradfolge erneut nach selber Reihenfolge wie eben.

6. Springe zu (1).

Man kann auch vorher schon einige Eigenschaften des Graphen anhand der Gradfolge
ueberpruefen. Ist die Anzahl ungerader Grade beispielsweise ungerade, gibt es
den Graphen sicher nicht.

Beispiel: Gegeben die Gradfolge $(1,1,2,3,4,4,5)$.

Im ersten Schritt reduzieren wir immer die ersten $d_1$ Grade, danach sortieren
wir.

1. $(1,1,2,3,4,4,5) \Rightarrow (1,0,1,2,3,3) \Rightarrow (0,1,1,2,3,3)$

2. $(0,1,1,2,3,3) \Rightarrow (0,1,0,1,2) \Rightarrow (0,0,1,1,2)$

3. $(0,0,1,1,2) \Rightarrow (0,0,0,0)$

Es gibt den Graphen mit dieser Gradfolge $(0,0,0,0)$ sicher. Es ist der Graph
mit vier Knoten, ohne Kanten. Also gibt es auch den Graphen mit Gradfolge
$(1,1,2,3,4,4,5)$.

https://gist.github.com/goldsborough/4ecbdb86029a8cae5f47

#### Umkehrung

Nun haben wir also einen Algorithmus, um zu bestimmen, ob es einen Graphen mit
einer bestimmten Gradfolge gibt. Wir moechten nun auch einen entsprechenden
Graphen $G = (V,E)$ zu dieser gueltigen Gradfolge bestimmen.

Der Havel-Hakimi Algorithmus kann einen solchen Graphen bestimmen. Hierbei sei
angemerkt, dass es grundsaetzlich zu einer Gradfolge sehr viele Graphen geben
kann. Aber es gibt nur einen eindeutigen, den man mit dem Havel-Hakimi-Verfahren
aus einer Gradfolge bestimmen kann.

Hierfuer fuehrt man einfach den normalen Havel-Hakimi Algorithmus aus, so wie er
oben beschrieben war. Eine wichtige Aenderung aber ist, dass man sich immer
merken muss, welche Grade zu welchen Knoten gehoeren. Man schreibt also Anfangs
zu (bzw. ueber oder unter) jedem Grad eine Knotennummer hin. Dann, wenn man
folglich die Grade reduziert und die Gradfolge neu sortiert, muss die
Knotennummer jeden Grades (reduziert oder nicht) erhalten bleiben.  Wenn man
letztendlich terminiert und eine gueltige Gradfolge gefunden hat, rekursiert man
rueckwaerts hoch. Dazu sieht man sich immer an, welche Knotennummer man als
letztes rausgenommen hat (weil es den hoechsten Grad hatte). Diesen nimmt man
als naechsten Knoten. Man verbindet diesen Knoten dann mit all den Knoten,
dessen Grad man vorher reduziert hat. Denn das sind dann genau die Kanten, die
man hinzufuegt bzw. vorher weggenommen hat. So erhaelt man letztendlich einen
eindeutigen Havel-Hakimi Graphen.

Beispiel: Gegeben die Gradfolge $(1,1,2,3,4,4,5)$.

0. Geben wir jedem Grad als Subskript seine Knotennummer:
   $(1_1,1_2,2_3,3_4,4_5,4_6,5_7)$.

1. $(1_1,1_2,2_3,3_4,4_5,4_6,5_7)$ wird zu $(1_1,1_2,1_3,2_4,3_5,3_6)$

2. $(1_1,1_2,1_3,2_4,3_5,3_6)$ wird zu $(0_3,1_1,0_2,1_4,2_5)$

3. $(0_3,1_1,0_2,1_4,2_5)$ wird zu $(0_2,0_3,0_4,0_1)$

Dann rueckwarts:

1. In (3) hatten wir Knoten $5$ mit Grad $2$ entfernt. Wir haben dabei den
   Grad der Knoten $2$ und $4$ reduziert. Also geben wir in unseren Graphen die
   Kanten $\{2,5\}$ und $\{4,5\}$.

2. In (2) hatten wir Knoten $6$ mit Grad $3$ entfernt. Wir haben dabei den Grad
   der Knoten $3,4,5$ reduziert. Also geben wir unserem Graphen die Kanten
   $\{3,6\}, \{4,6\}, \{5,6\}$.

3. In (1) hatten wir Knoten $7$ mit Grad $5$ entfernt. Wir haben dabei den Grad
   der Knoten $2,3,4,5,6$ reduziert. Also fuegen wir die Kanten $\{2,7\},
   \{3,7\}, \{4,7\}, \{5,7\}$ und $\{6,7\}$ hinzu.

Fertig.

## Weitere Eigenschaften

Fuer jeden Graphen $G = (V, E)$ gilt:

1. Es gibt eine Zusammenhangskomponente mit $\Delta(G) + 1$ Knoten, wo
   $\Delta(G)$ der *Maximalgrad* des Graphen ist. Man stelle sich dazu ein Rad
   vor, das $\Delta(G)$ Speichen (Kanten) hat. An den Enden sind $\Delta(G)$
   Knoten, und in der Mitte noch der $\Delta(G) + 1$-te Knoten, der eben Grad
   $\Delta(G)$ hat.

2. Alle Zusammenhangskomponenten haben mindestens $\delta(G) + 1$ Knoten, wo
   $\delta(G)$ der *Minimalgrad* von $G$ ist.

3. Wenn $G$ kreisfrei ist, dann ist $|E| < |V| \equiv |E| \leq |V| - 1$, denn
   sobald $|E| \geq |V|$ muss eine Kanten zwei Knoten verbinden, die schon
   verbunden sind.

4. Wenn $G$ bipartit ist, dann $|E| \leq \left(\frac{|V|}{2}\right)^2$

5. Wenn $\Delta(G) + \delta(G) + 1 \geq |V|$, ist $G$ zusammenhaengend.

6. Wenn $n$ Knoten Maximalgrad haben (Grad $|V| - 1$), dann muss jeder andere
   Knoten mindestens Grad $n$ haben.

Frage: Ist jeder Graph mit Gradfolge $(6, 4, 4, 3, 3, 2, 2)$ zusammenhaengend?

Antwort: Ja weil (1) die Summe der Grade durch zwei hier groesser $|V| - 1$
         ist. Und (2) weil gilt es gibt eine Zusammenhangskomponente mit
         $\Delta(G) + 1$ Knoten, und da $\Delta(G) + 1 = 6 + 1 = 7$, und es $7$
         Knoten gibt, muessen diese alle verbunden sein.

## Matrizen

Es gibt nun zwei Moeglichkeiten, um einen Graphen tabellarisch darzustellen.

### Adjazenzmatrix

In einer Adjazenzmatrix schreibt man oben und links die Knoten hin, es handelt
sich also um eine $|V| \times |V|$ Matrix (Knoten mal Knoten). Ein Eintrag $(i,
j)$ in der Zeile $i$ und Spalte $j$ ist dann $1$, wenn ${v_i, v_j} \in E$. Wenn
die Knoten $v_i,v_j$ aber nicht adjazent sind, ist der Eintrag $0$.

Mit diesem Modell sind Mehrfachkanten nicht darstellbar, weil es fuer zwei
Knoten nur einen Eintrag gibt. Schleifen sind jedoch schon moeglich.

Die Summe jeder Zeile bzw. jeder Spalte ist dann der Grad des Knoten in dieser
Zeile bzw. Spalte.

Bei einem ungerichteten Graphen sind die Diagonalen alle symmetrisch (von links
oben nach rechts unten und umgekehrt, fuer jede Laenge).

### Inzdenzmatrix

In einer Inzidenzmatrix schreibt man nun links die Knoten und oben die Kanten
hin. Es ist also eine $|V| \times |E|$ Matrix (Knoten mal *Kanten*). Ein Eintrag
$(i, j)$ in der Zeile $i$ und Spalte $j$ ist dann $1$, wenn $v_i$ Endknoten von
$e_j$ ist bzw. $e_j$ *inzident* zu $v_i$ ist, also wenn $\exists v_k : e_j =
\{v_i, v_k\}$.

Die Summe jeder Zeile ist hierbei weiterhin der Grad des Knoten dieser Zeile,
wohingegen die Summe jeder Spalte $2$ ist, weil jede Kante in einem (einfachen)
Graphen genau zwei Endknoten hat.

### Beispiel

* $V = [3] = \{1, 2, 3\}$
* $E = \{e_1 = \{1, 2\}, e_2 = \{2, 3\}, e_3 = \{2, 2\}\}$

Adjazenzmatrix:

|     |$v_1$|$v_2$|$v_3$|
|:----|:----|:----|:----|
|$v_1$|  0  |  1  |  0  |
|$v_2$|  1  |  1  |  1  |
|$v_3$|  0  |  1  |  0  |

Inzidenzmatrix:

|     |$e_1$|$e_2$|$e_3$|
|:----|:----|:----|:----|
|$v_1$|  1  |  0  |  0  |
|$v_2$|  1  |  1  |  1  |
|$v_3$|  0  |  1  |  0  |

### Isomorphe Graphen

Zwei Graphen $G = (V,E)$ und $G' = (V',E')$ nennt man *isomorph* ($G \cong G'$),
wenn jeder Knoten in $G$ mit allen seinen Kanten auch in $G'$ vorkommt, nur mit
anderem Namen. Die Strukturen der beiden Graphen sind also ident, die Knoten
besitzen nur andere Namen.

Formal existiert also fuer zwei isomorphe Graphen $G, G': G \cong G'$ eine
Bijektion ueber den Knotenmengen der beiden Graphen $f: V \rightarrow V'$ wo
fuer beliebige zwei Knoten $u,v$ in $G$ gilt, dass wenn sie in $G$ adjazent
sind, sie auch (mit anderen Namen) in $G'$ adjazent sind:

$\forall u,v \in V: \{u, v\} \in E \leftrightarrow \{f(u), f(v)\} \in E'$.

Das *Graphenisomorphieproblem* ist dabei ein NP-vollstaendiges Problem, was
bedeutet, dass kein polynomieller Algorithmus dafuer bekannt ist.

Isomorphie ist auch eine Aequivalenzrelation.

http://news.sciencemag.org/math/2015/11/mathematician-claims-breakthrough-complexity-theory

## Graphklassen

Nochmals zusammengefasst hier die populaersten Graphklassen.

### Pfad

Ein Graph $G = (V, E)$ mit Knotenmenge $V = \{v_0, ..., v_{n-1}\}$ mit
Kantenmenge $E = \{\{v_i, v_j\} \in {V \choose 2} \, | \, j = i + 1\}$ heisst
Pfad $P_n$. Eine solche "Kette" hat immer $|V| - 1$ Kanten. Ein Pfad ist
$0$-regulaer fuer $n=1$, $1$-regulaer fuer $n=2$ und fuer $n > 2$ nicht
regulaer.

### Kreise

Ein Graph $G = (V, E)$ mit Knotenmenge $V = \{v_0, ..., v_{n - 1}\}$ mit
Kantenmenge $E = \{\{v_i, v_j\} \in {V \choose 2} \, | \, j = (i + 1) \mod n\}$
heisst Kreis $C_n$. Ein Kreis hat immer $|V|$ Kanten. Ein Kreis ist $2$-regulaer
fuer alle $n > 2$.

### Vollstaendiger Graph

Ein Graph $G = (V, E)$ mit Knotenmenge $V = \{v_0, ..., v_{n-1}\}$ und
Kantenmenge $E = {V \choose 2}$ heisst vollstaendiger Graph $K_n$. Es ist also
jeder Knoten mit jedem anderen Knoten verbunden. Ein solcher Graph hat ${|V|
\choose 2} = \frac{n(n - 1)}{2}$ Kanten. Ein vollstaendiger Graph mit $n$-Knoten
ist $n - 1$-regulaer $\forall n \in \mathbb{N}$ (nicht $n = 0$).

### Bipartiter Graph

Ein Graph $G = (U, V, E)$ mit Knotenmenge $V = \{v_0, ..., v_{n - 1}\}$ und $U =
\{u_0, ..., u_{m - 1}\}$ mit Kantenmenge $E = \{\{v, u\} \in {{V \, \cup\, U}
\choose 2} \, | \, u \in U \text{ und } v \in V\}$ heisst (vollstaendiger)
bipartiter Graph $K_{m,n}$. Ein solcher Graph hat $m \cdot n$ Kanten. Ein
bipartiter Graph $K_{m,n}$ ist $k$-regulaer wenn $k = m = n = \frac{|V|}{2}$.

### Gittergraph

Ein Graph $G = (V, E)$ mit Knotemenge $V = \{v_{i,j} \, | \, i \in \{0, ..., n -
1\} \text{ und } j \in \{0, ..., m - 1\}\}$ mit Kantenmenge $E = \{\{v_{i,j},
v_{k,l} \, | \, |i - k| + |j - l| = 1\}\}$ heisst Gittergraph $M_{m,n}$. $|i -
k| + |j - l| = 1$ bedeutet dabei, dass der Manhattan-Abstand $1$ ist. Ein
solcher Graph hat $n$ mal $m - 1$ plus $m$ mal $n - 1$ Kanten, also $|E| =
n(m-1) + m(n-1) = 2mn - m - n$ Kanten. Ein Gittergraph ist nur $2$-regulaer,
wenn $m = n = 2$ und $1$-regulaer, wenn $m = n = 1$ und sonst nie $k$-regulaer.

### Binaerer Hyperwuerfel

Ein Graph $G = (V, E)$ mit Knotemenge $V = \{v_i \, | \, 0 \leq i \leq 2^n - 1\}$ und Kantemnenge $E = \{\{v_i, v_j\} \in {V \choose 2} \, | \, v_i \text{
und } v_j \text{ sind binaer benachbart }\}$ heisst binaerer Hyperwuerfel
$Q_n$, wobei $n \in \mathbb{N}_0$ die Anzahl an Bits angibt. Ein solcher Graph
hat $n \cdot 2^{n - 1}$ Kanten, weil jeder der $2^n$ Knoten $n$ Nachbarn hat,
man hierbei aber 2 mal zaehlt also durch $2$ dividieren muss (die $2$-er Potenz
also einfach verniedrigt). Ein Hyperwuerfel $Q_n$ ist immer $n$-regulaer.

### Kantenmengen

Frage: Wieviele Kanten kann ein ungerichteter Graph maximal haben
Antwort: ${n \choose 2} = \frac{n(n - 1)}{2}$ (ohne Zuruecklegen, ohne
         Reihenfolge)

Frage: Wieviele Kanten kann ein gerichteter Graph (ohne
       Mehrfachkanten) maximal haben?
Antwort: $n^2$, denn jetzt muss man nicht durch zwei dividieren, und
         Selbstschleifen sind erlaubt. Auch Ansatz: ${n \choose 2}$ mal zwei,
         weil es fuer jedes Paar eine Kante in die eine und eine in die andere
         Richtung gibt, plus $n$ fuer die Selbstschleifen. Dann auch: ${n
         \choose 2} = \frac{n(n - 1)}{2} \cdot 2 + n = n^2 - n + n = n^2$. (mit
         Zuruecklegen, mit Reihenfolge)

Frage: Wieviele verschiedene Kanten kann ein verallgemeinerter Graph maximal
haben?
Antwort: ${{n + k - 1} \choose k} = {{n + 2 - 1} \choose 2} =
         \frac{n(n+1)}{2}$.

Anderer Ansatz: ${n \choose 2}$ ungerichtete Kanten wie normal, plus $n$ fuer
                die Selbstschleifen: ${n \choose 2} + n = \frac{n(n - 1) +
                2n}{2} = \frac{n^2 - n + 2n}{2} = \frac{n^2 + n}{2} =
                \frac{n(n + 1)}{2}$.

Frage: Wieviele Kanten kann ein Hypergraph maximal haben?
Antwort: Man sehe die $n$ Kanten als $n$ Bits. Dann steht jeder $n$-stellige
         Bit-Vektor fuer eine Kante. Also gibt es $2^n - 1$ Kanten maximal, weil das leere Wort keine Kante sein kann (verbindet keinen Knoten, ist also keine Kante).

## Baeume

Ein Baum ist nun ein einfacher Graph, fuer welchen gilt:

1. Er ist __zusammenhaengend__ ist, hat also nur eine Komponente.

2. Er ist __kreisfrei__.

Die Knoten eines Baumes $T = (V,E)$ mit Grad $1$ nennt man dann *Blaetter* des
Baumes. Alle anderen Knoten nennt man dann *inneren Knoten*. In diesem Modell
gibt es momentan noch keine Wurzel.

In der Kantenmenge $E = \{\{u,v\} \,|\, u,v \in V\}$ sind die Blaetter alle jene
Knoten $v$, die nur einmal vorkommen (Grad $1$) und die inneren Knoten alle
jene, die mindestens zwei mal vorkommen (Grad $> 1$).

### Waelder

Wenn die Komponenten eines Graphen allesamt Baeume sind, dann nennt man den
Graphen einen *Wald* (eine Gruppe von kreisfreien, zusammenhaengenden
Baeumen). Somit kann man also sagen, dass wenn ein Graph kreisfrei ist und:

* *zusammenhaengend*, dann ist er ein *Baum*.
* *nicht zusammenhaengend*, dann ist er ein *Wald*.

Frage: Wieviele Kanten hat ein Wald mit $n$ Knoten und $k$ Komponenten?
Antwort: Wenn $k = 1$ hat man $n - 1$ Kanten. Wenn $k = 2$ hat man $n - 1 - 1
= n - 2$ Kanten. Also allgemein __$n - k$__, denn

$|E| = \sum_{i=1}^k |E_i| = \sum_{i=1}^k(|V_i| - 1) = (\sum_{i=1}^k|V_i|) - (k
\cdot (-1)) = |V| - k$

### Hoehe

Die Hoehe $h(T)$ eines Baumes $T = (V,E)$ ist die Laenge des laengsten Pfades,
__plus $1$__! Hierbei sei angemerkt, dass ein Pfad immer die Kanten zaehlt. Man
kann aber auch einfach die Knoten zaehlen, denn dann zaehlt man automatisch
immer $1$ mehr. Also:

Die Hoehe eines Baumes ist:

* Die laengste Folge von Kanten im Baum, plus $1$, oder
* Die laengste Folge von Knoten im Baum.

### Spannbaeume

Ein Teilgraph $T = (V', E')$ eines Graphen $G = (V, E)$ heisst *Spannbaum*, wenn
$T$ ein Baum ist und die selben Knoten verbindet wie $G$, also $V' = V$.

Es gibt genau $|V|^{|V| - 2}$ Spannbaeume auf $|V|$ Knoten.

### Eigenschaften

#### Mindestens zwei Blaetter

Jeder Baum $T = (V, E)$ mit $|V| \geq 2$ enthaelt __mindestens zwei
Blaetter__. Entweder hat der Baum nur zwei Knoten, oder sonst geht man so weit
wie moeglich nach links zum einen Blatt und so weit wie moeglich nach rechts
zum anderen Blatt. __Da der Graph keinen Kreis hat, kann man auch nie in einen
Kreis geraten__.

Da ein Baum entweder nur aus einem Knoten besteht und dann die Gradfolge $(0)$
hat, oder wenn $|V| \geq 2$ er __mindestens zwei Blaetter__ enthalten muss, muss
in einer Gradfolge eines Baumes mit mehr als einem Knoten mindestens zwei mal
eine $1$ stehen. Existiert also ein Baum mit Gradfolge $(2, 2, 2, 1)$? Nein.

#### Baum minus Blatt = Baum

Nimmt man von einem Baum mit $|V| \geq 2$ ein *Blatt* $v$ weg, ist der
entstandene Graph $T[V \setminus \{v\}]$ noch immer ein Baum. Der Baum muss
auch kreisfrei bleiben, weil man durch Weggnahme eines Knoten einen
kreisfreien Graphen nie einen Kreis geben kann, und ausser der einen
anhaengenden Kante kann auch sonst keine Verbindung verloren gehen, woraus der
Zusammenhang erhalten bleibt.

#### $|E| = |V| - 1$

Fuer einen Baum gilt *immer* $|E| = |V| - 1$

#### Spannbaeume

Jeder *zusammenhaengende* Graph $G = (V, E)$ enthaelt __mindestens einen
  Spannbaum__.

Satz von Cayley: In einem vollstaendigen Graphen $G = (V, E)$ ist die Anzahl von
Spannbaeumen $|V|^{|V| - 2}$ bzw. __$n^{n - 2}$__ wenn $n = |V|$.

#### $2|V| - 2$

Die Summe aller Grade in einem Baum ist $2|V| - 2$.

$\sum_{v \in V} \deg(v) = 2|E| = 2|V| - 2$, da $|E| = |V| - 1$

#### Genau ein Pfad

Wenn $T = (V, E)$ ein Baum ist, gilt, dass je __zwei__ (*beliebige*!) Knoten $u,
v \in V$ durch __genau einen Pfad__ verbunden sind. Weil:

1. Je zwei ... sind durch ... einen Pfad verbunden
   $\Rightarrow$ Zusammenhaengend.

2. Je zwei ... sind durch __genau einen__ Pfad verbunden
   $\Rightarrow$ Kreisfrei.

#### Kreisfrei, Zusammenhaengend, $|E| = |V| - 1$

Jeder Baum besitzt alle drei Eigenschaften kreisfrei, zusammenhaengend und $|V|
= |E| + 1$. Es reicht, aber nur zwei davon zu beweisen, dann folgt die dritte
automatisch. Von diesen drei Eigenschaften kann ein Graph also entweder keine,
genau eine oder alle drei besitzen, aber niemals genau zwei.

## Wurzelbaeume

Ein Wurzelbaum (*rooted tree*) ist ein Tupel $(T, v)$, bestehend aus einem Baum
$T = (V, E)$ mit einem *besonderen* Knoten $v \in V$, den man als *Wurzel*
bezeichnet (einfach so definiert).

Da es $n^{n - 2}$ Baeume mit $n$ Knoten gibt (Satz von Cayley) und in jedem
dieser Baume einer der $n$ Knoten die Wurzel sein kann, gibt es $n \cdot n^{n -
2} = n^{n - 1}$ Wurzelbaeume mit $n$ Knoten.

### Vorfahren

Die Knoten entlang eines Pfades von einem inneren Knoten $v \in V$ zur Wurzel
heissen *Vorgaenger* von $v$. Der benachbarte Voragenger von $v$ ist der
Elternknoten von $v$.

### Nachfolger

Die Knoten im Baum, fuer welche es einen Pfad zur Wurzel gibt, der $v \in V$
enthaelt, heissen *Nachfolger* von $v$.  Die zu $v$ benachbarten Nachfolger sind
die *Kinder* von $v$, auch genannt *unmittelbare Nachfolger*.

### Binaere Wurzelbaeume

Ein Binaerbaum (nicht binaerer Suchbaum!) ist ein Wurzelbaum, wo jeder Knoten
__hoechstens__ zwei Kinder hat.

Ein *vollstaendiger* (auch: *balancierter*) Binaerbaum ist ein Wurzelbaum, wo
jeder Knoten __genau__ zwei Kinder hat und alle Blaetter den selben Abstand zur
Wurzel haben (gleich der Hoehe - 1).

#### Eigenschaften

* Die Hoehe eines Binaerbaumes mit $|V|$ Knoten ist $O(\log_2 |V|)$.

* Die Anzahl an Knoten ist in einem binaeren Wurzelbaum hoechstens $2^h - 1$.

* Ein binaerer Wurzelbaum hat hoechstens $2^{h - 1}$ *Blaetter*, ein
  vollstaendiger genau so viele.

* Ein binaerer Wurzelbaum hat hoechstens $2^{h - 1} - 1$ *innere Knoten mit
  Wurzel*, ein vollstaendiger genau so viele.

### Suchbaeume

Ein Suchbaum ist ein Binaerbaum, fuer welchen gilt:

* Die Knotenmenge hat eine Ordnung.

* Die Kinder eines inneren Knoten sind unterscheidbar und beschriftet
  ("linkes"/"rechts").

* Fuer alle inneren Knoten (und Wurzel) $v$ gilt:

	+ Alle Knoten im linken Unterbaum des Knoten sind kleiner als $v$.
	+ Alle Knoten im rechten Unterbaum des Knoten sind groesser als $v$.

### Pruefer Codes

Fuer alle $n \in \mathbb{N}$ mit $n \geq 2$ gilt, dass es zu einem Baum $T =
([n], E)$ einen Pruefer-Code $c$ gibt, der ein $n - 2$ Tupel ist und den Baum
exakt identifiziert. Es gibt also eine Bijektion zwischen einem Pruefer-Code und
dem dazugehoerigen Baum: $c \mapsto T$ und $T \mapsto c$. Aus einem Pruefer-Code
der Laenge $k$ folgt hierbei ein Baum mit $k + 2$ Knoten.

Hier sind nun die Algorithmen beschrieben, um einen Baum in einen Code
umzuwandeln und umgekehrt.

#### Baum zu Code

1. Solange bis nur mehr zwei Knoten uebrig sind.

2. Suche den kleinsten Knoten (in der Ordnung) und entferne ihn und die Kante
   an der er hing.

3. Schreibe den einzigen Nachbarn dieses Blatts als naechste Pruefer-Ziffer auf.

4. Ignoriere die letzten zwei.

#### Code zu Baum

1. Bestimme die Kanten die nicht im Code vorkommen. Fuer einen Pruefer-Code mit
   $k$ Ziffern folgt ein Baum mit $n = k + 2$ Knoten, also zaehle von $1$ bis
   $n$; alle nicht im Code vorkommenden Ziffern sind die fehlenden Knoten.

2. Gehe von links nach rechts den Code durch und fuer jede Ziffer $c_i$
   wiederhole:

3. Von den von $c_i$ *verschiedenen*, *noch unmarkierten* Knoten im Baum, die
   *nicht mehr im Code vorkommen*, nimm den kleinsten, markiere ihn und verbinde
   ihn mit $c_i$ (nur den genommenen Knoten markieren, $c_i$ bleibt unmarkiert).

4. Verbinde die letzten zwei unmarkierten Knoten miteinander.

Wir markieren immer den Knoten, der mit der naechsten Pruefziffer $c_i$
verbunden wird.

Ein Pruefer-Code fuer einen Baum mit $n$ Knoten hat also $n - 2$ Ziffern. Fuer
jede der $n - 2$ Ziffern gibt es also $n$ Moeglichkeiten (jeder Knoten koennte
der Knoten sein, von dem das Blatt abgerissen wurde). Daher gibt es $n^{n - 2}$
verschiedene Pruefer-Codes. Jeder Pruefer-Code steht fuer einen eindeutigen Baum,
also gibt es $n^{n - 2}$ Baeume mit $n$ Knoten bzw. $n^{n - 2}$ Spannbaeume
eines Graphen mit $n$ Knoten. Damit wird der Satz von Cayley bewiesen.

### Beispiel

Beispiel: $T = \{[6], \{\{1, 2\}, \{\{1, 3\}, \{1, 4\}, \{1, 5\}, \{2, 6\}\}\}$

#### Baum $\Rightarrow$ Code

Die Blaetter $B$ haben Grad $1$, also hier $B = \{3, 4, 5, 6\}$.  Entferne das
kleinste Blatt und schreibe seinen einzigen Nachbarn $1$ als erste Ziffer des
Codes auf, also nun:

$B = \{4, 5, 6\}$ $E = \{\{1, 2\}, \{1, 4\}, \{1, 5\}, \{2, 6\}\}$ $c = (1)$

Nun entferne $4$:

$B = \{5, 6\}$ $E = \{\{1, 2\}, \{1, 5\}, \{2, 6\}\}$ $c = (1, 1)$

Nun entferne $5$, jetzt ist $1$ auch ein Blatt:

$B = \{1, 6\}$ $E = \{\{1, 2\}, \{2, 6\}\}$ $c = (1, 1, 1)$

Nun entferne also $1$, dann wird $2$ ein Blatt:

$B = \{2, 6\}$ $E = \{\{2, 6\}\}$ $c = (1, 1, 1, 2)$

Da nur mehr $2$ Knoten bleiben, terminiert der Algorithmus und der Pruefer-Code
zum Baum ist $c = (1, 1, 1, 2)$.

#### Code $\Rightarrow$ Baum

Zuerst muss man sich ansehen, wieviele Knoten im Baum sind. Der Pruefer-Code $c
= (1, 1, 1, 2)$ hat die Laenge $4$ und ein Pruefer-Code hat immer die Laenge
$n - 2$ wo $n$ die Anzahl an Knoten des Baumes ist, also der Baum hat die
Knotenmenge $[4 + 2] = [6] = \{1, 2, 3, 4, 5, 6\}$. Nun schreibt man sich jene
dieser Knoten auf, die nicht im Code sind, also $R = \{3, 4, 5, 6\}$. Sei auch
$M$ die Menge der markierten Knoten. Also soweit:

$c = (1, 1, 1, 2)$ $R = \{3, 4, 5, 6\}$ $E = \{\}$ $M = \{\}$ $V = \{\}$

Nun nehme die erste Ziffer $1$ aus dem Code und verbinde sie mit dem Knoten aus
$V \cup R$ mit der kleinsten Ziffer, die noch nicht markiert ist, also nicht in
$M$ ist. Also verbinde $1$ mit $3$.

$c = (1, 1, 2)$ $R = \{4, 5, 6\}$ $E = \{\{1, 3\}\}$ $M = \{3\}$ $V = \{1, 3\}$

Nun verbinde $1$ wieder mit der kleinsten Ziffer in $(V \cup R) \setminus M$,
also hier $4$. $1$ darf nicht genommen werden, weil $1$ die selbe Ziffer ist.

$c = (1, 2)$ $R = \{5, 6\}$ $E = \{\{1, 3\}, \{1, 4\}\}$ $M = \{3, 4\}$ $V =
\{1, 3, 4\}$

Nun verbinde $1$ mit $5$:

$c = (2)$ $R = \{6\}$ $E = \{\{1, 3\}, \{1, 4\}, \{1, 5\}\}$ $M = \{3, 4, 5\}$
$V = \{1, 3, 4, 5\}$

Nun verbinde $2$ mit $1$, da $1$ noch unmarkiert ist und nicht mehr im Code
vorkommt, und kleiner als $6$ ist:

$c = ()$ $R = \{6\}$ $E = \{\{1, 3\}, \{1, 4\}, \{1, 5\}, \{1, 2\}\}$ $M = \{1,
3, 4, 5\}$ $V = \{1, 2, 3, 4, 5\}$

Nun sind $6$ und $2$ die letzten beiden unmarkierten Knoten. Verbinde diese und
fertig:

$E = \{\{1, 3\}, \{1, 4\}, \{1, 5\}, \{1, 2\}, \{2, 6\}\}$ $V = \{1, 2, 3, 4, 5,
6\}$

## Euler-Touren

Eine Euler-Tour ist ein Weg durch einen Graphen, der jede Kante genau einmal
enthaelt und dessen Anfangs- und Endknoten identisch sind. Eine Euler-Tour ist
in einem Graphen moeglich wenn:

1. Er zusammenhaengend ist.
2. Jeder Knoten im Graphen geraden Grad hat.

Das folgt daraus, dass wenn alle Knoten geraden Grad haben, man gleich oft in
sie rein wie auch wieder hinaus gehen kann.

Ein Graph, in dem eine Euler-Tour moeglich ist, nennt man eulersch.

Ein Euler-Weg ist ein Weg, in dem jede Kante nur einmal vorkommt, wo aber der
Anfangs- und Endknoten nicht unbedingt die selben sein muessen. Eine Euler-Tour
bzw. ein Euler-Kreis ist also nur Sonderfall eines Euler-Weges. Ein Euler-Weg
ist nur moeglich, wenn es *maximal* zwei Knoten mit ungeraden Grad gibt, und
diese die Endknoten des Weges sind. Z.B. man nehme eine Kette. Die beiden
Endknoten des Weges haben Grad $1$, alle inneren Knoten haben Grad $2$. Hier ist
ein Euler-Weg also moeglich, aber keine Euler-Tour.

Frage: Ist jeder Graph mit Gradfolge $(4, 4, 4, 4, 4, 4, 4, 4, 4, 4)$ eulersch?
Antwort: Nein. Alle Grade sind zwar gerade, aber der Graph koennte nicht
zusammenahengend sein (z.B. zwei mal $K_5$).

### Finden von Euler-Touren

Der Algorithmus um eine *Euler-Tour* zu finden heisst Fleury's Algorithm und
lautet:

1. Stelle sicher, dass der Graph zusammenhaengend ist und alle Knoten geraden
   Grad haben.
2. Starte an einem beliebigen Knoten.
3. Gehe ueber eine beliebige Kante von diesem Knoten, ausser diese Kante ist
   eine Bruecke, also eine Kante die den Graphen in zwei Komponenten teilen
   wuerde, die beide noch unbesuchte Kanten enthalten. Markiere diese Kante und
   schreibe sie auf.
4. Terminiere, wenn es keine Kanten mehr gibt.

Fuer einen Euler-Weg:

1. Stelle sicher, dass der Graph zusammenhaengend ist und maximal zwei Knoten
   ungeraden Grad haben.
2. __Starte bei einem der beiden ungeraden Knoten__.
3. Gleich.
4. Gleich.

https://www.math.ku.edu/~jmartin/courses/math105-F11/Lectures/chapter5-part2.pdf

## Hamilton-Kreise

Ein Hamilton-Kreis in einem Graphen ist ein Kreis, der alle __Knoten__ genau
einmal enthaelt. __Dabei muessen nicht alle Kanten besucht werden!!__

Ein Graph, in dem eine Hamilton-Tour moeglich ist, heisst *hamiltonsch*.

Es gibt keine einfache Regel um zu bestimmen ob eine Hamilton-Tour moeglich ist
bzw. um diese zu finden. Dieses Problem ist NP-vollstaendig. Brute-force waere
$O(n!)$. Man kann aber sagen:

1. "Ist die Summe des Grades je zweier nicht-adjazenter Knoten mindestens $n$,
   so enthaelt ein __einfacher Graph__ $G$ einen Hamilton-Kreis (Kriterium von
   Ore). (nur $\Rightarrow$!)"

2. Ein Graph mit __mehr als zwei Knoten und einer $1$ in seiner Gradfolge kann
   kein hamiltonscher Graph sein__, denn man muss zu dem Knoten ja wieder
   zurueck, was bei mehr als zwei Knoten nur geht, wenn man den Nachbarn des
   $1$-Knoten zwei mal besucht.

3. Ist der Graph 2-faerbbar, also bipartit und somit Teilgraph des $K_{n,m}$
   (wobei die Knotemenge sogar gleich ist), dann gibt es nur einen Hamilton
   Kreis, wenn $n = m$.

Frage: Ist ein Graph mit Gradfolge $(1, 2, 3, 3, 4, 5)$ hamiltonsch?
Antwort: Nein, denn er enthaelt mehr als zwei Knoten und einen Grad $1$.

Frage: Gibt es einen hamiltonschen Graphen mit Gradfolge $(2, 4, 4, 4, 4, 4)$?
Antwort: Ja, $K_5$ (fuenf mal Grad $4$), mit einem weiteren Knoten.

## Planare Graphen

Ein Graph ist *planar*, wenn man ihn ohne Kantenueberschneidungen zeichnen
__kann__.

Ein Graph ist eben, wenn er in einer Darstellung ohne Kantenueberschneidungen
ist. Also ein planarer Graph kann als ebener Graph dargestellt werden, und jeder
ebene Graph ist natuerlich auch ein planarer Graph.

### Gebiete

Ein ebener Graph unterteilt die Ebene in eine Menge von *Gebieten* $R$ (Regions)
entlang seiner Kanten. Hierbei zaehlt auch das Gebiet ausserhalb des Graphen als
Gebiet (also $|R| \geq 1$).

Es gibt nun eine wichtige Formel, die eine eindeutige lineare Beziehung zwischen
der Anzahl von Gebieten und der Knoten- bzw. Kantenanzahl herstellt: Die
*Eulersche Polyederformel* (EPf). Fuer jeden __zusammenhaengenden, planaren__
Graphen $G = (V, E)$ gilt:

$|R| + |V| = |E| + 2$ bzw. $|R| = |V| - |E| + 2$.

Da sich bei jeder isomorphen Umstellung eines Graphen ("Umzeichnung") die Anzahl
an Knoten und Kanten des Graphen nie veraendert, folgt: Die Anzahl der Gebiete
eines ebenen Graphen bleibt in jeder planaren Darstellung gleich.

### Korrolare von $|R| + |V| = |E| + 2$

Es gibt nun eine Reihe von Korrolaren aus der Euler'schen Polyederformel, die
einige Eigenschaften von planaren Graphen feststellen, die im Weiteren unten
detaillierter erlaeutert werden:

* $|E| \leq 3|V| - 6$ fuer $|V| \geq 3$.
* Ein planarer Graph hat immer einen Knoten mit Grad $\leq 5$.
* Ein kreisfreirer Graph ist immer Planar ($|E| < |V|$).
* Es gilt: $|R| = |E| - |V| + k + 1$ fuer einen Graphen mit $k$ Komponenten.
* Der $K_5$ und $K_{3,3}$ sind nicht planar.

#### $|E| \leq 3|V| - 6$

Fuer jeden planaren Graphen $G = (V, E)$ mit $|V| \geq 3$ gilt:

$|E| \leq 3|V| - 6$

##### Beweis

Da jedes Gebiet durch mindestens drei Kanten abgegrenzt ist, und jede Kante
maximal zwei Gebiete abgrenzt, gilt dann: $3|R| \leq 2|E|$. Mit $|R| + |V| =
|E| + 2$ dann:

$3|R| \leq 2|E| \Rightarrow 3(|E| - |V| + 2) \leq 2|E| \Rightarrow 3|E| - 3|V| +
6 \leq 2|E| \Rightarrow |E| \leq 3|V| - 6$

#### Planar $\Rightarrow$ ein Knoten mit Grad $\leq 5$.

Jeder planare Graph $G = (V,E)$ hat einen Knoten mit Grad hoechstens $5$:

Graph planar $\Rightarrow$ $\exists v \in V: \deg(v) \leq 5$

##### Beweis

Wir nehmen an es gibt einen einfachen Graphen wo alle Knoten Grad $6$
haben. Dann gilt laut Handshaking-Theorem: $|V| \cdot 6 = 2|E|$, dividiert durch
zwei also $|E| = 3|V|$. Aber laut Korollar $1$ muss gelten: $|E| \leq 3|V| -
6$. Wiederspruch, es kann also keinen solchen planaren Graphen geben.

#### Kreisfrei = Planar

Jeder kreisfreie Graph muss planar sein. Ein kreisfreier Graph hat naemlich
genau ein und nur ein Gebiet, dass aeussere. Denn es kann ja nirgends einenn
Kreis geben, der ein neues Gebiet erzeugen wuerde.

#### $K_5$, $K_{3,3}$ Sind Nicht Planar

Der $K_5$ ist nicht planar. Es muesste gelten $|E| \leq 3|V| - 6$, aber $|E| =
\frac{5 \cdot 4}{2} = 10$ und $3|V| - 6 = 9$, also gilt kann dieser Graph nicht
planar sein.

Der $K_{3,3}$ ist ebenso nicht planar.

#### $|R| = |E| - |V| + k + 1$

Ein planarer Graph $G$ mit $k$ *planaren Komponenten* hat $|E| - |V| + k + 1$
Gebiete. Intuitiv kann man es sich induktiv vorstellen. der Pfad mit $|V|$
Knoten hat eine Komponente, und dann gilt die EPf $|E| - |V| + 2 = |E| - |V| + 1+ 1$. Wenn man nun eine Kante entfernt, hat man eine Komponenten mehr, also:

$(|E| - 1) - |V| + (k + 1) + 1 = |E| - |V| + k + 1$

##### Beweis

Alle Komponenten sind also planare (Teil-)Graphen. Dann besitzt die $i$-te
Komponente also $|R_i| = |E_i| - |V_i| + 2$ Gebiete nach der Euler'schen
Polyederformel. Fuer jede Komponente wird aber auch das auessere Gebiet
gezaehlt, also ziehen wir aus jeder EPf eins raus, und stellen das aussere
Gebiet als Summand $1$ nach aussen. Dann muss die Gesamtanzahl an Gebieten $|R|$
des Graphen sein:

$|R| = 1 + \sum_{i=1}^k (|E_i| - |V_i| + 2 - 1)$

Nun ziehen wir den Faktor $+ 2 - 1 = 1$ noch nach draussen:

$1 + k + \sum_{i=1}^k |E_i| - |V_i|$

Dann spalten wird die Summen in die Summe der Kantenzahlen minus der Summe der
Knotenzahlen:

$1 + k + \sum_{i=1}^k |E_i| - \sum_{i=1}^k |V_i|$

Diese Summen sind eben die Kanten- und Knotenzahlen des gesamten Graphen, und so
folgt:

$1 + k + |E| - |V| = |E| - |V| + k + 1$.

#### Beweis 2

Induktion ueber $k$.

Basis: Sei $k = 1$, dann gilt: $|R| = |E| - |V| + 1 + 1 = |E| - |V| + 2$.

Schritt: Sei $k \in \mathbb{N}$ beliebig, aber fest.

	Annahme: Die Aussage gilt fuer einen Graphen mit $k$ Komponenten.

	Behauptung: Dann gilt sie auch fuer $k + 1$.

	Ziel: $|R| = |E| - |V| + (k + 1) + 1$.

Beweis: Wir haben also einen Graphen mit $k + 1$ (planaren) Komponenten. Dann
fuegen wir eine Bruecke zwischen zwei Komponenten hinzu. Dann haben wir eine
Kante mehr, aber wichtiger: eine Komponente weniger. Nun gilt laut Annahme die
Aussage fuer einen Graphen mit $k$ Komponenten, (der hier $|E| + 1$ Kanten hat):

$|R| = (|E| + 1) - |V| + k + 1$.

Nun nehmen wir die Kante wieder weg. Dann haben wir wieder eine Kante mehr, und
weil diese Kante eine Bruecke war, auch eine Komponente mehr. Daraus folgt:

$|R| = (|E| + 1 - 1) - |V| + (k + 1) + 1 = |E| - |V| + (k + 1) + 1$.

### Satz von Kuratowski

"Ein Graph $G$ ist genau dann *nicht* planar, wenn er einen *Teilgraphen*
besitzt, der ein *Unterteilungsgraph* des $K_5$ oder $K_{3,3}$ ist".

#### Unterteilungsgraph

Ein *Unterteilungsgraph* eines Graphen $G$ ist ein Graph, der dadurch entsteht,
dass Kanten von $G$ durch Pfade ersetzt werden.

Sei $G = (V, E)$ ein ungerichteter, einfacher Graph. Dann versteht man unter der
*Unterteilung* einer Kante $e = \{u, v\} \in E$ die Ersetzung dieser Kante im
Graphen durch zwei neue Kanten $e_1 = \{u,w\}, e_2 = \{w,v\}$, mit einem neuen
Knoten $w \notin V$ dazwischen. Auf diese Weise entsteht ein neuer Graph $G' =
(V', E')$ mit der neuen Knotenmenge $V' = V \cup \{w\}$ und neuen Kantenmenge
$E' = E \setminus \{e\} \cup \{e_1, e_2\}$.

Ein Unterteilungsgraph $G'$ eines Graphen $G$ ist nun ein Graph, der aus
*null-*, ein- oder mehrmaliger Kantenunterteilung entsteht.

#### Methoden

Frage: Zeige, dass ein Graph nicht planar ist.
Methode: Finde einen Teilgraphen der ein Unterteilungsgraph des $K_5$ oder
         $K_{3,3}$ ist (oder einer der beiden ist).

Frage: Ist ein Graph planar?
Methode: Siehe, ob man Knoten oder Kanten so verschieben kann, dass der Graph
         eben ist, oder finde einen Teilgraphen, der ein Unterteilungsgraph des
         $K_5$ oder $K_{3, 3}$ ist.

#### Korollar

Ein Korrolar des Satz von Kuratowski ist, dass wenn das folgende gilt:

1. Er hat weniger als $5$ Knoten mit Grad $\geq 4$

2. Er hat weniger als $6$ Knoten mit Grad $\geq 3$.

der Graph planar ist. Aus (1) folgt, dass der Graph keinen Teilgraphen hat, der
der $K_5$ ist, und aus (2), dass er keinen Teilgraphen hat, der der $K_{3,3}$
ist. Mathematisch muss also gelten:

1. $|\{v | \deg(v) \geq 4\}| < 5$, __UND__

2. $|\{v | \deg(v) \geq 3\}| < 6$


__DIESER SATZ GEHT NUR IN EINE RICHTUNG!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!__

Wenn (1) und (2) gelten, dann ist der Graph planar. Wenn (1) oder (2) nicht
gilt, sagt das nichts darueber aus, ob der Graph planar ist, oder nicht.

### Beweisrezept

Frage: Ist ein Graph $G = (V,E)$ planar?

Methode:

	1. Wenn alle Knoten Grad $> 5$ haben, ist der Graph nicht planar.

	2. Gilt das obige Korrolar zur Anzahl an Knoten mit Grad $\geq 4$
       bzw. $\geq 3$, dann ist der Graph planar.

	3. Gilt $|E| > 3|V| - 6$, ist der Graph nicht planar.

	4. Probiere, Kanten zu strecken und planar umzuformen. Wenn es eine ebene
       Darstellung zum Graphen gibt, ist er planar.

	5. Wenn das nicht geht, vermute, dass der Graph einen Teilgraphen hat, der
       Unterteilungsgraph des $K_5$ oder $K_{3,3}$ hat. Ueberpruefe dies.

## Faerbung

Eine Knotenfaerbung (vertex coloring) eines Graphen $G = (V, E)$ mit $k$ Farben
ist eine Abbildung $c: V \rightarrow [k]$, sodass *adjazente Knoten nie die
selbe Farbe haben*, also: $c(u) \neq c(v) \forall \{u, v\} \in E$.

Die chromatische Zahl (chromatic number/index) $\chi(G)$ ist die minimale Anzahl
an Farben, die fuer eine Knotenfaerbung von $G$ benoetigt werden. Weil Graphen
immer nur einen Knoten oder gar keine haben koennen, sowie auch nicht immer
vollstaendig sind, schreibt man fuer Graphenklassen $\chi(G)$ meistens als obere
Grenze, also sodass die Anzahl an Farben $\leq \chi(G)$ ist.

### Eigenschaften

1. Fuer einen __vollstaendigen__ Graphen mit $n$ Knoten: $\chi(K_n) = n$

2. Fuer einen __bipartiten__ Graphen mit $m,n$ Knoten: $\chi(K_{m,n}) \leq 2$

3. Fuer einen __k-partiten__ Graphen $G$: $\chi(G) \leq k$

4. Fuer einen __planaren Graphen__: $\chi(G) \leq 4$ (*Vier-Farben-Satz*)

5. Fuer einen __Baum__: $\chi(G) \leq 2$

6. Fuer einen __Pfad__ mit $n \geq 2$ Knoten: $\chi(P_n) \leq 2$

7. Fuer einen __Kreis__ mit $n$ Knoten: $\chi(K_n) \leq 2$ wenn $n$ gerade,
   $\chi(K_n) \leq 3$ sonst.

8. !!!! __Allgemein: $\chi(G) \leq \Delta(G) + 1$__ !!!!

Zu (8): "Fuer einen Graphen mit Maximalgrad $\Delta(G)$ braucht man maximal
        $\Delta(G) + 1$ Farben".

#### Frage

Ist jeder Graph $G$ mit Gradfolge $(2, 2, 2, 2, 3, 3, 3, 3)$ 4-faerbbar?

Ansatz 1: Da $\Delta(G) = 3$ und gilt $\chi(G) \leq \Delta(G) + 1$, also
          $\chi(G) \leq 4$. Man braucht also maximal $4$ Farben, der Graph ist
          also $4$ faerbbar.

Ansatz 2: Man zeigt, dass $G$ planar ist, dann gilt der Vier-Farben-Satz. Ein
          Graph ist planar, wenn er weniger als $5$ Knoten mit Grad $\geq 4$ hat
          und weniger als $6$ Knoten mit Grad $\geq 3$ hat. Beides gilt hier.

### Weniger Wichtig

1. Fuer einen vollst. Gittergraphen mit $m,n$ Knoten: $\chi(M_{m,n}) = 2$
2. Fuer einen vollst. bin. Hyperwuerfel mit $n \geq 2$ bits: $\chi(Q_n) = 2$

## Matchings

Sei $G = (V, E)$ ein Graph. Dann ist ein Matching eine Teilmenge $M \subseteq E$
von __paarweise disjunkten__ Kanten. Ein beliebiger Knoten darf also nur
Endknoten von maximal einer einzigen Kante sein. Somit ist also eine injektive
Funktion gesucht (*maximal* eine Moeglichkeit pro Knoten) $f: V \rightarrow
V$. Die Funktion ist nicht bijektiv, weil nicht *jeder* Knoten ge-matched werden
muss.

Ein Matching $M$ nennt man *perfekt*, wenn jeder Knoten $v \in V$ zu einer Kante
in $M$ gehoert, also wenn die Funktion *bijektiv* ist. Dann gilt auch:

$|M| = |V|/2$

### Beispiel

Sei also $G = ([6], \{\{1, 2\}, \{1, 4\}, \{2, 4\}, \{2, 5\}, \{3, 5\}, \{3,
6\}, \{5, 6\}\})$. Dann ist:

1. $M = \{\{1, 2\}, \{2, 4\}, \{3, 5\}\}$ kein Matching, weil $\{1, 2\}$ und
$\{2, 4\}$ nicht disjunkt sind.

2. $M = \{\{1, 2\}, \{3, 5\}\}$ ein Matching, aber kein perfektes.

3. $M = \{\{1, 2\}, \{4, 5\}, \{3, 6\}\}$ ein perfektes Matching.

### Heiratssatz

Der Heiratssatz (*Hall's Marriage-Theorem*) sagt aus, dass fuer einen
__bipartiten__ Graph $G = (A, B, E)$ es genau dann ein __perfektes__ Matching
gibt, wo $|M| = |A| = |B|$, wenn gilt:

$|\Gamma(X)| \geq |X|\, \forall X \subseteq A$

Intuition: Findet man eine Teilmenge $X \subseteq A$, die insgesamt mit weniger
Knoten verbunden ist, als es Knoten in $X$ gibt, dann wird es zumindest einen
Knoten in $X$ geben, der keinen Partner findet. Es muss also zumindest gelten
dass $|B| \geq |A|$.

#### Korrolare

Es gibt folgende Korrolare des Heiratssatzes:

1. Fuer einen $k$-regulaeren bipartiten Graph existert immer ein perfektes
   Matching fuer beliebiges $k \in \mathbb{N}$. Denn in jeder Teilmenge findet
   man dann fuer jede Knoten immer $k$ Partner, wovon schon einer (bei $k = 1$)
   genuegt.

2. Existiert fuer einen bipartiten Graphen $G = (A, B, E)$ ein perfektes
   Matching, so gilt $|A| = |B|$.

3. Existiert also ein perfektes Matching, muss $|V| = |A| + |B| = 2|A|$ gerade
   sein.

### Stabile Matchings

Ein Matching $M$ fuer einen bipartiten Graphen $G = (A, B, E)$ ist stabil, wenn
es in $M$ keine zwei Paare $(a, b), (a', b')$ gibt, sodass $a$ $b'$ bevorzugt,
und $b'$ $a$. Denn sonst wuerden sich $a$ und $b'$ von ihren Partnern trennen
und ein neues Paar $(a, b')$ machen.

#### Gale-Shapley Algorithmus

Der *Gale-Shapley Algorithmus* berechnet ein stabiles Matching. Hierbei koennen
Paare gesperrt sein, oder nicht. Eine ungesperrtes Paar darf sich noch
auftrennen, um das Matching stabil(er) zu machen, ein gesperrtes Paar
nicht. Auch hat jedes $a \in A$ eine Ordnung von bevorzugten $b \in B$. Am
Anfang ist das Matching leer, dann:

1. Fuer jedes verbleibende $a$:

   1. Nehme das verbleibende $b \in B$, dass $a$ in seinem Ranking bevorzugt,
      und dass fuer $a$ noch nicht unter Betracht gezogen wurde.

   2. Ist $b$ noch nicht in einem Paar, dann mache $(a,b)$ ein Paar.

   3. Bevorzugt $b$ $a$ ueber seinen momentanen Partner $a'$, dann mache auch
      $(a, b)$ ein Paar und lege $a'$ zurueck in den Pool von verbleibenden $a$.

2. Sind alle $b$ in einem Paar, wird jedes Paar gesperrt und es hat sich ein
   perfektes Matching gefunden.

Der Algorithmus berechnet in maximal $n^2$ Schritten ein stabiles Matching, da
jedes $a$ mit jedem $b$ maximal ein Mal in Betracht gezogen wird. Er terminiert
immer mit einer stabilen Heirat, da sich sonst eine Verbesserung gefunden
haette.

```python
remainingAs = bag<A>
rankingsA   = map<A, stack<B>>
rankingsB   = map<B, map<A, int>>
matches     = map<B, pair<A,B>>

while len(remainingAs) > 0:
	a = remainingAs.pop()
	while len(rankingsA[a]) > 0: # will never happen, but just to be sure
		b = rankingsA[a].pop()
		match = matches.get(b)
		if match is None:
			matches[b] = (a, b)
			break
		elif rankingsB[b][a] > rankingsB[b][match[0]]:
			remainingAs.push(match[0]) # put back into bag
			matches[b] = (a, b)
			break

return matches.values()
```
