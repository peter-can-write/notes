# B-Baeume

Ein B-Baum ist eine Erweiterung des grundlegenen binaeren Suchbaums. Das Problem
bei solchen binaeren Suchbaeumen ist, dass sie an die seitenweise
Speicherorganisation, die Datenbanksysteme verwenden, nicht angepasst sind. In
einem binaeren Suchbaum waere es z.B. moeglich, dass der linke Verweis eines
Knotens auf einen Knoten auf einer voellig anderen Seite verweist, die
womoeglich sogar erst aus dem Hintergrundspeicher eingelagert werden muss.

Die Grundidee des B-Baumes ist es nun, die Organisation des Speichers von Seiten
auszunutzen. Ein Knoten enthaelt jetzt nicht mehr nur ein Datum mit Verweisen
nach links und rechts, sondern soviel wie auf eine Seite passen. Jeder Knoten in
einem B-Baum repraesentiert also eine *Seite*.

Auf einer solchen Seite sind nun also mehr als ein Datum gespeichert. Mit einem
"Datum" wird hierbei ein *Schluessel*, sowie ein assoziierter Wert
bezeichnet. Der *Schluessel* ist hierbei *meistens* der Primaerschluessel der
Relation, sodass damit das Tupel oder, viel eher, der TID assoziiert werden
kann. Es ist aber auch moeglich, mit dem SQL statement:

```sql
CREATE INDEX <index_name>
ON <table_name> (<column_name>)
```
Daher ist in der Diskussion von B-Baeumen mit einem *Schluessel* nicht immer der
Primaerschluessel der Relation gemeint, sondern der Schluessel der
Indexstruktur.

Ein Knoten enthaelt also wie gesagt viele solcher Schluessel und assoziierten
Daten. Fuer je zwei Schluessel $S_1,S_2$, die nebeneinander im Knoten (also auch
auf der korrespondierenden Seite) liegen, gibt es nun auch drei Verweise
$V_0,V_1,V_2$:

1. $V_0$ verweist auf einen Knoten (eine Seite) mit Eintraegen, die alle kleiner
   sind als $S_1$. "Kleiner" bezieht sich hierbei auf die Relation $<$.

2. $V_1$ verweist auf einen Knoten, dessen Eintraege alle groesser als $S_1$
   sind und kleiner als $S_2$.

3. $V_2$ verweist auf einen Knoten, sodass alle Schluessel in diesem Knoten
   groesser sind als $S_2$.

So also die Struktur des B-Baumes. Wir werden nun noch ein paar Eigenschaften
diskutieren und dann behandeln, wie Daten in einen solchen B-Baum gesucht,
eingefuegt und geloescht werden.

## Eigenschaften

Jeder B-Baum ist mit einem Grad assoziiert, der ueblicherweise durch die
Variable $k$ angegeben wird. $k$ steht dabei fuer die minimale Anzahl an
Eintraegen, die in einer Seite, also einem Knoten, enthalten sein
duerfen. Enthaelt ein Knoten weniger als so viele Eintraege, ist seine Existenz
nicht effizient, und eine Massnahme muss gezogen werden, um die Eintraege im
Baum besser zu distributieren. Hierbei ist $k$ auch immer halb so gross, wie ein
ganzer Knoten. Das bedeutet, dass ein Knoten minimal $k$, aber maximal $2 \cdot
k$ Eintraege haben kann. Wir machen bei dieser Bedingung eine Ausnahme fuer die
Wurzel, da sonst diese Eigenschaft z.B. bei einem leeren Baum ja schon verletzt
waere. Wir koennen also schon festhalten:

1. Jeder Knoten __ausser der Wurzel__ hat *mindestens* $k$ Elemente und
   *maximal* $2k$ Elemente.

2. Ein Knoten ist also zwischen $50\%$ und $100\%$ voll (minimale
   Platzverschwendung)

3. $k$ steht fuer das Minimum an Elementen.

Die Struktur eines B-Baumes ist so, dass jeder Schluessel (Eintrag) in einem
Knoten einen Verweis auf kleinere Schluessel links von sich hat, und groessere
Schluessel rechts von sich. Fuer $n$ Elemente, wo nach obiger Definition also $k
\leq n \leq 2k$, gilt also immer, dass es exakt $n + 1$ Verweise gibt. Wir
nennen diese Knoten, auf welche die Verweise zeigen, auch die *Kinder* des
Knotens, indem der Schluessel sich befindet. Wir legen also fest:

4. Fuer $n$ Elemente mit $k \leq n \leq 2k$ hat der Knoten $n + 1$ Kinder.

5. Jeder Knoten hat mindestens $k + 1$ Kinder und maximal $2k + 1$.

Wir muessen hier aber wieder eine Ausnahme fuer die Blaetter machen, da diese
natuerlich keine weiteren Kinder haben (nach Definition eines Blattes). Also
gelten obige zwei Eigenschaften fuer alle *inneren Knoten*.

Innerhalb eines Knoten sind die $k$ bis $2k$ Kinder dann immer sortiert. Somit
kann man dann innerhalb eines Knotens eine Binaersuche machen.

Eine sehr wichtige Eigenschaft von B-Baeumen ist, dass sie *balanciert*
sind. Das bedeutet, dass jeder Pfad von einem Blatt zur Wurzel gleich lang
ist. Die *Hoehe* eines Baumes gibt allgemein die Laenge des laengsten Pfades,
plus eins, an. Nach der Eigenschaft des B-Baumes ist jeder Pfad von einem Blatt
zur Wurzel also gleich der Hoehe des Baumes.

Wir koennen auch noch einige Zahlen der Knoten festlegen. Sei $h$ die Hoehe des
Baumes. Dann gilt:

1. Er enthaelt $2^h - 1$ Knoten.

2. Er enthaelt $2^{h - 1}$ Blaetter.

3. Er enthaelt $2^{h - 1} - 1$ inneren Knoten.

4. Seine Hoehe ist zwischen $\log_k N$ und $\log_{2k} N$ bei $N$ Knoten.

Noch eine Anmerkung zu einem weiteren Vorteil von B-Baeumen, neben der
seitenangepassten Struktur: Weil ein Knoten relativ viele Eintraege besitzt (bei
einer Seitengroesse von 4 KiB ca. 100 Eintraege), ist das *fan-out*, also die
Anzahl an Verweisen zu Kindern pro Knoten sehr hoch. Das hat wiederum zur Folge,
dass die Hoehe des Baumes sehr niedrig ist. Das bedeutet also, dass wir fuer
eine Suche nicht viele Seiten zu besuchen brauchen. Wir verhindern somit
vielmaliges Ein- und Auslagern von Seiten bzw. eine ganze Rundreise zum
Hintergrundspeicher.

## Einfuegen in einen B-Baum

Der Einfuegealgorithmus in einen B-Baum ist wie folgt:

1. Fuehre Suche nach Schluessel durch; diese endet. Fuege den Schluessel dort
   ein.
2. Ist der Knoten ueberfuellt, teile den Knoten in der Mitte, schiebe den Median
   in den Elternknoten und verbinde die Schluessel vom vorherigen Knoten die
   links vom Median waren mit dem linken Verweis vom Median (im Elternknoten)
   und selbes mit dem rechten Knoten (waechst nach oben wie 2-3 Tree).
3. Ist der Elternknoten ueberfuellt, wiederhole den Prozess. Gibt es keinen
   Elternknoten (i.e. ist die Wurzel ueberfuellt), so kreeire einen neuen
   Knoten. Einzige Situation wo ein B-Baum also waechst.

Bei insert muessen alle Eintraege in $O(k)$ verschoben werden.

## Deletion in einem B-Baum:

TL;DR:

Es wird ein Element geloescht in:

1. Einem Blatt. Wenn dann:
   1. Noch genug Elemente im Knoten sind, fertig.
   2. Man aus einem Geschwisterknoten etwas nehmen kann ($>k$), rotiere das
      Elternelement in den Knoten aus dem geloescht wurde, und ersetze das
      Elternelement mit seinem Successor im Geschwisterknoten.
   3. Geschwisterknoten alle $k$ Elemente haben, merge. Ziehe Elternelement in
      den Knoten und merge Geschwisterknoten dazu. Verlinke wieder im
      Elternknoten.
2. Einem inneren Knoten. Ersetze das Element mit seinem Successor und fuehre
   Loesch-Algorithmus (1) auf diesem Successor im Blatt durch.

Und wenn durch 1.3 ein innerer Knoten $< k$ wird, dann:

1. Wenn im Geschwisterknoten des nun ungueltigen Knoten genug Elemente sind,
   rotiere das Geschwisterelement wie bei RB-Tree (Geschwisterelement kommt in
   den Elternknoten, Elternelement in den inneren knoten, und erhaelt
   als Kinderknoten den Kinderknoten des (alten) Geschwisterelements).
2. Wenn der Geschwisterknoten $k$ Elemente enthaelt, merge Elternelement mit dem
   ungueltigen Knoten und dem Geschwisterknoten und verlinke wieder mit dem
   Elternknoten.

Deletion wird in echten B-Bauemen (echten Datenbanksystemen) oft gar nicht
behandelt, weil es so selten vorkommt. Oder wenn schon behandelt, dann wartet
man einfach bis ein Knoten leer ist und loescht ihn.

### Deletion in einem Leaf:

1. Fall: Deletion eines Keys in einem Leaf so, dass danach noch die mindestens
   $k$ erforderlichen Keys in diesem Leaf enthalten sind, der Baum also noch
   vollstaendig korrekt ist.

k = 2 (mindestens 2 keys pro node)

```
[*|k|*|k|*|k|*]
       ^ delete
```

Einfach loeschen (noch immer $\geq k$ Keys):

```
[*|k|*|k|*]
```

2. Fall: Deletion eines Keys in einem Leaf so, dass danach $< k$ Keys verbleiben
   wuerden, der Knoten die B-Baum Invarianten also verletzt. Hier gibt es nun
   zwei "Unterfaelle":

(a) Der linke oder rechte Schwesterknoten vom Knoten in welchem geloescht werden
soll enthaelt $> k$ Keys, sodass man einen aus diesem entnehmen koennte, um die
Invarianten wiederherzustellen. Sind mit $k = 2$ im rechten Schwesterknoten
z.B. 3 Keys enthalten, kann man den kleinsten davon in die Position des
Vaterkeys geben und den Vaterkey als groessten Key in den Knoten geben, wo
geloescht werden soll.

```
         [*|c|*|x|*]
         /     \
[*|a|*|b|*] [*|d|*|e|*|f|*]
       ^
```

Man will hier also $b$ loeschen. Die Moeglichkeit bestuende hier, $c$ (Vater) in
die Position von $b$ zu geben, wenn man $c$ dann durch ein passendes Element
(von der Ordnung im Vaterknoten, also $< x$ bzw. $\geq c$) ersetzten koennte. Da
$d$ groesser $c$ ist und kleiner $x$, und der rechte Knoten mehr als $k$ Kinder
enthaelt (sodass er bei Entnahme mindestens noch $k$ enthalten wuerde), kann man
also $d$ bei $c$ einsetzen und $c$ in den linken Knoten geben:

```
         [*|d|*|x|*]
         /     \
[*|a|*|c|*] [*|e|*|f|*]
```

Selbes haette mit einem linken Schwesterknoten gemacht werden koennen.

(b) Keiner der Schwesterknoten hat genuegend Elemente, um eines zu entfernen wie
in (a). Dann kann man den Knoten, in welchem geloescht wird, und einen
Schwesterknoten zusammen mit dem Vaterkey einfach mergen. Denn es gilt ja, dass
Knoten zwischen $k$ und $2k$ Keys enthalten muessen. In den zwei Schwesterknoten
sind in dieser Situation also jeweils $k$ enthalten. Merged man die Knoten, hat
man einen Knoten mit $2k$ Keys. Nun loescht man den zu loeschenden Key ($- 1$)
und setzt den Vaterknoten noch ein $+ 1$ und man hat eben exakt $2k$ Keys.


```
         [*|c|*]
         /     \
[*|a|*|b|*] [*|d|*|e|*]
                   ^
```

Man kann vom linken Schwesterknoten also kein Element mehr entfernen
$\Rightarrow$ merge:

```
[*|a|*|b|*|c|*|d|*]
```

### Deletion in einem inneren Knoten

Hierbei muss man einfach Hibbard-Deletion durchfuehren:

1. Suche das kleinste Element das groesser ist (ceiling) oder das groesste
   Element das kleiner ist (floor).

2. Ersetze das zu loeschende Element im inneren Knoten durch den Successor.

3. Fuhre den Loesch-Algorithmus fuer den Successor durch. Z.B. wenn dieser
   innere Knoten ueber den Blaettern ist und das Blatt nach dem Loeschen des
   Successors $< k$ Elemente hat, muss man womoeglich mit dem Successor, der
   jetzt im inneren Knoten ist, mergen (was man auch gleich sehen kann).

#### Wenn ein innerer Knoten durch merge $< k$ wird

Wenn ein innerer Knoten $k$ Schluessel enthaelt und zwei Verweise links
und rechts auf Kinderknoten mit ebenso jeweils nur $k$ Schluessel,
und man loescht dann einen der Kinderelemente (oder wenn man den inneren Knoten
loescht, als Resultat von Hibbard-Deletion), dann ist es enorm wichtig nach den
folgenden drei Schritten vorzugehen:

1. Element aus dem Kinderknoten entfernen.

2. __Elternelement zuerst in den geloeschten Knoten ziehen__. Der innere Knote
   wird dabei $< k$ und __soll so werden!__.

3. Kinderknoten nun zu einem validen Knoten zusammenziehen.

Der Punkt ist, dass man jetzt einen Knoten $< k$ als inneren Knoten hat, und
kann/muss somit das Problem um eine Stufe nach oben ziehen. Oben kann man sich nun
ansehen ob man:

a) *mergen* muss. Dazu zieht man das Elternelement des inneren Knoten in den
   inneren Knoten, zieht dann den inneren Knoten mit seinem Geschwisterknoten
   zusammen und verlinkt den resultierenden Knoten wieder in den Elternknoten.

b) *rotieren* muss. Das passiert dann wie beim Red-Black-Tree. Also man will das
   kleinste Element des Geschwisterknoten in den Elternknoten ziehen und das
   Elternelement in den inneren Knoten. Dabei muss man jedoch "the left baggage"
   des Geschwisterelemennts noch rechts an das Elternelement im inneren Knoten
   knuepfen.

$k = 1$

```
    [10]
    /   \
  [4]   [15]
 /   \    |   \
[1]  [7]  [12] [20]
```

Loesche nun $7$, dann:

1) Loesche das Element

```
    [10]
    /   \
  [4]   [15]
 /   \    |   \
[1]  []  [12] [20]
```

2) Ziehe das Elternelement in den geloeschten Kinderknoten:

```
    [10]
    /   \
  [ ]   [15]
 /   \    |   \
[1]  [4]  [12] [20]
```

Nun ist der Elternknoten leer. Haette man jetzt gleich schnell-schnell gemerged
kaeme man womoeglich in die Situation, dass `[1 4]` auf der zweiten Ebene
waere. Dann wuerde man sich fragen, wie man nun rotieren soll um die Balance
wiederherzustellen. Dadurch, dass man den Elternknoten leer werden laesst, sieht
man schoen, dass man sich nur um diesen kuemmern muss.

3) Zusammenziehen

```
    [10]
    /   \
  [ ]   [15]
   |     |   \
[1 4]  [12] [20]
```

Man sieht hier also, das man aus dem Geschwisterknoten des nun leeren Knoten
nichts rotieren kann, also muss man mergen. Dazu wieder Schritt fuer Schritt:
Zuerst das Elternelement runterziehen, dann verknuepfen:

```
    [10 15]
   /   |   \
[1 4] [12] [20]
```

Fall b waere gewesen, wenn der Baum in 3) so ausgesehen haette:

```
       [10]
      /     \
   [ ]    [15 23]
   |       /   \  \
[1 4]  [12 13] [20] [24]
```

Hier kann man nicht mergen, also rotiert man wie bei der Links-Rotation beim
RB-Tree:


```
           [15]
       /         \
   [10]           [23]
   |   \          /  \
[1 4]  [12 13] [20] [24]
```

## Komplexizitaet

Wir moechten nun die Komplexizitaet der Operationen betrachten. Zunaechst die
der Suche. Wir analysieren die Schritte der Suche:

1. Wir gehen maximal $\log_k N$ Schritte nach unten.

2. Wir machen in jedem der $\log_k N$ Schritte eine binaere Suche durch die $k$
   Elemente, haben also eine Komplexizitaet von $\log_2 k$.

Wir koennen also eine Komplexizitaet von $\log_k N \cdot \log_2 k$
bestimmen. Nun gilt aber das Logarithmusgesetz $\log_b x = \frac{\log_a
x}{\log_a b}$ fuer beliebige Basen $a,b \in \mathbb{N}$. Somit koennen wir
$\log_k N$ umformen:

$$\log_k N = \frac{\log_2 N}{\log_2 k}$$

Setzen wir das wieder in unsere Formel ein, kuerzt sich der Nenner diesen
Bruchs:

$\frac{\log_2 N}{\log_2 k} \cdot \log_2 k = \log_2 N$

Die Komplexizitaet der Suche in einem B-Baum ist also exakt gleich wie in einem
Binaerbaum. Daraus folgt eben die Beobachtung, dass ein B-Baum die Effizienz der
Suche weder erhoeht noch verniedrigt. Sein Vorteil ist einfach die
seitenangepasste Struktur und die niedrige Tiefe, die seltene Zugriffe auf den
Hintergrundspeicher implizieren.

Fuer Einfuegen und Loeschen muessen wir im schlimmsten Fall in jedem Knoten alle
Eintraege verschieben. Somit erhalten wir eine Komplexizitaet von $O(\log_k N
\cdot k)$.

## $B^+$-Baum

Ein $B^+$-Baum ist eine Erweiterung des B-Baumes. Die Grundidee ist es hier,
dass Daten nur in den Blaettern gespeichert werden. Die inneren Knoten enthalten
hingegen nur die Schluessel, welche eine Suche also nur durch den Baum zu einem
Blatte leiten sollen. Das besondere an den Blaetterknoten ist dann, dass sie wie
verkettete Listen durch zusaetzliche Zeiger verbunden sind. Somit sind
$B^+$-Baueme ideal fuer *Bereichsanfragen* wie:

```sql
select *
from R r
where r.A between x and y;
```

Ein $B^+$-Baum ist nun nicht mehr nur durch einen Grad $k$ definiert, sondern
durch ein Paar an Graden $(k, k^\star)$. Hierbei steht $k$ dann fuer die
minimale Anzahl an Eintraegen in einem inneren Knoten, wo also nur Schluessel
udn Verweise, nicht Daten gespeichert sind. $k^\star$ ist dann die minimale
Anzahl an Eintraegen in einem Blatt. Wie bei einem B-Baum ist das Maximum der
Anzahl an Eintragen in einem $B^+$-Baum also $2k$ bzw. $2k^\star$.

### Einfuegen in einem $B^+$-Baum

Sei ein $B^+$-Baum mit Graden $(2,2)$ gegeben, der momentan nur einen Knoten
enthaelt. Dieser Knoten ist anfangs sowohl Wurzel- als auch Blattknoten:

```
[*|a|*|b|*|c|*|d|*]
```

Fuegt man nun einen neuen Schluessel $e$ in diesen Baum ein, so wird der Knoten
zu voll, muss also wie bei einem B-Baum gesplittet werden. Man erhaelt also zwei
Knoten und einen freien Median $c$, der bei einem *B-Baum* nun in den Elternknoten
(oder hier in einen neu kreeirten) Knoten eingefuegt werden wuerde. Bei einem
$B^+$-Baum sind Daten aber nur in den Blaettern, das heisst der Median muss
auch in den Blaettern bleiben.

__Nach Konvention bleibt der Median im linken Knoten. Stoesst man bei der Suche
auf einen inneren Knoten mit dem exakten Schluessel, muss man also nach links
gehen__.  Die gesplitteten Blaetter werden auch noch mit Pointern
verbunden (linked-list). Resultat:

```
          [*|c|*]
		  /     \
[*|a|*|b|*|c|*]--->[*|d|*|e|*]
```

Sucht man nun nach $c$, so wird man diesen Schluesel zwar schon in einem inneren
Knoten (bzw. hier der Wurzel) finden, man muss dem Baum jedoch trotzdem folgen,
da Daten nur in den Blaettern enthalten sind. Der Median selbst wird beim
Splitten im linken Knoten gespeichert, kommt man bei der Suche also in einem
inneren Knoten auf den Schluessel selbst, so muss man nach links gehen.

Fuegt man nun in einem Blatt wieder zu viele Elemente ein, wird der Median
rekursiv in den Elternknoten geschoben (hier nur ein Schritt):

```
+ e, + f, + g, +h

          [*|c|*]
		  /     \
[*|a|*|b|*|c|*] -> [*|d|*|e|*|f|*|g|*|h|*] <-- Zu voll!

Resultat:

           ____ [*|c|*|e|*]_____
          /          |          \
[*|a|*|b|*|c|*] -> [*|d|*|e|*] -> [*|f|*|g|*|h|*]
```

Die Blaetter wuerden hier noch zusaetzliche Daten, also das ganze Tupel oder den
TID enthalten.

### Deletion in einem $B^+$-Baum

1. Fall: Es sind nach dem Loeschen noch mehr als $k^\star}$ Elemente in einem
   Blatt enthalten. Dann ist es leicht, man loescht das Element einfach und wenn
   es der Schluessel im Elternknoten war (also der ganz links), dann ersetzt man
   diesen einfach durch den naechst kleineren Schluessel:

```
- f
           ____ [*|c|*|f|*]_____
          /          |          \
[*|a|*|b|*|c|*] -> [*|d|*|e|*|f|*] -> [*|g|*|h|*]

Resultat:

          ____ [*|c|*|e|*]_____
         /          |          \
[*|a|*|b|*] -> [*|c|*|e|*] -> [*|g|*|h|*]
```

2. Fall: Es sind nach dem Loeschen weniger als $k^\star$ Elemente in einem Blatt
   enthalten. Dann:

(a): In einem Schwesterknoten sind mehr als $k^\star$ Elemente enthalten. Dann
     schiebt man den groessten im linken Knoten oder kleinsten im rechten Knoten
     in den nun zu kleinen Knoten (wo geloescht wurde). __Auf neuen Parent
     achten!__

```
- e
           ____ [*|c|*|e|*]_____
          /          |          \
[*|a|*|b|*|c|*] -> [*|d|*|e|*] -> [*|x|*|y|*]

Resultat:

          ____ [*|b|*|x|*]_____
         /          |          \
[*|a|*|b|*] -> [*|c|*|d|*] -> [*|x|*|y|*]
```

(b) Es sind in keinem Schwesterknoten genuegend Elemente ($> k^\star$). Dann
    muss man den Knoten in dem geloescht wurde mit einem Schwesterknoten mergen
    (wie bei B-Baum). Der Schluessel im Elternknoten kann mitsamt rechten
    Pointer dann geloescht werden. Eventuell muss im Elternknoten dann wieder
    balanciert werden (hier nicht weil Wurzel kann weniger als $k$ Elemente
    enthalten)

```
- e
           ____ [*|c|*|e|*]_____
          /          |          \
[*|b|*|c|*] -> [*|d|*|e|*] -> [*|x|*|y|*]

Resultat:

           [*|c|*]
         /        \
[*|b|*|c|*] -> [*|d|*|x|*|y|*]
```

## Praefix $B^+$-Baum

Ein Praefix $B^+$ Baum ist eine Optimierungsvariante des $B^+$-Baumes, wobei
nicht ganze Schluessel (strings) als Referenzschluessel in den inneren Knoten
gewaehlt werden, sondern nur Praefixe davon. Z.B. werden anstatt ganzen Namen
"Mueller", "Bauhaus", "LKW" in den Knoten nur Praefixe, also am einfachsten nur
Buchstaben in den inneren Knoten gespeichert. __Dadurch spart man Platz__.

```
	              [Mauer] <-- 5 Bytes
	             /       \
[Apfel, Baum, Mauer]->[Riegel, Schloss, Xylophon]
```

Wenn doch auch ginge:

```
	                [M] <-- 1 Byte
	             /       \
[Apfel, Baum, Mauer]->[Riegel, Schloss, Xylophon]
```

Bei einem Overflow muss man eben den ersten Buchstaben des Medians der Werte
nehmen (lexikographisch sortiert). Haben alle Woerter den selben Buchstaben,
erweitert man den Praefix um den Median des zweiten Buchstaben.
