# Mehrdimensionale Indexstrukturen

## Wieso

Gegeben also eine Menge von Menschen mit den Attributen ${\text{Alter},
\text{Geschlecht}, \text{Gehalt}}$, und eine Datenbank aller Menschen auf der
Welt ($\sim 10^{10}$). Nun moechte man also eine query nach einem
Menschen mit dem Alter $40$, dem Geschlecht $\text{maennlich}$ und dem Gehalt
$100,000$ machen. Mit B-Bauemen ginge das folgendermassen:

1. Suche ALLE Menschen ($\sim 10^{10}$) mit dem Alter $40$.
2. Suche ALLE Menschen, die maennlich sind.
3. Suche ALLE Menschen, die $100K$ verdienen.
4. Bilde den Schnitt ($\cap$) aus diesen.

Die Ineffizienz diesen Verfahrens ist eindeutig, wenn jede dieser Queries durch
einen eigenen B-Baum muss. Zum einen, weil es eben $O(3 \cdot\log_2(10^{10}))$
Laufzeit hat und zum anderen, weil die Zwischenresultate enorm sind und alle
zwischengespeichert werden muessen.

Man bedenke das Beispiel einer 2-dimensionalen Laengen-/Breitengradsuche nach
Restaurants in Garching. Mit B-Baeumen muss man zuerst alle Restaurants auf der
Welt auf den selben Breitengraden wie Garching suchen und alle Restaurants auf
der Welt auf den selben Laengengraden wie Garching, und dann den Durchschnitt
bilden, anstatt einfach die $\sim 5^2 \text{ km}$ abzusuchen. Hierfuer braucht
man also Datenstrukturen, die geeigneter fuer mehrdimensionale Suche sind.

## Arten von Queries

Welche Arten von Queries koennte man fuer Daten mit $N$ Dimensionen machen?

1. Exact-Match-Query: spezifiziert den Suchwert fuer *jede* Dimension *exakt*.
2. Partial-Match-Query: spezifiziert den Suchwert fuer *einige* der Dimensionen
   exakt (von anderen Dimensionen also *alle* gesucht).
3. Range-Query: spezifiziert ein Suchintervall $[lo, hi]$ fuer *alle* Dimension.
4. Partial-Range-Query: spezifiziert ein Suchintervall fuer *einige* Dimensionen
   (von anderen also *alle* gesucht).

## Charakterisierung Mehrdimensionaler Datenstrukturen

Mehrdimensionale Datenstrukturen koennen nach drei Kriterien charakterisiert
werden. Sie koennen sein:

1. Atomar: Beschreibbar durch ein Rechteck.
2. Vollstaendig: Die Vereinigung der Gebeite ergibt den gesamten *moeglichen*
   Datenraum (nicht nur den gespeicherten).
3. Disjunkt: Die Gebiete uberlappen nicht.

Beispiele:

Grid-File (regulaeres Gitter von Buckets): atomar, vollstaendig, disjunkt

K-D-B-Baum (K-d Baum mit B-Baum Buckets): atomar, vollstaendig, disjunkt

R-Baum: atomar

$R^+$-Baum: atomar, disjunkt

## R-Baeume

Ein R-Baum ist eine __balancierte__ mehrdimensionale Datenstruktur mit
Schluesseln in den inneren Knoten und der Wurzel, und den Daten in den
Blaettern. Dabei werden die Daten nach ihren Dimension in Regionen unterteilt.

Beispielsweise ist eine 2-dimensionale Datenstruktur schon durch zwei Punkte in
der Ebene definiert, also $(min_{d_1}, min_{d_2})$ als linker unterer Punkt und
$(max_{d_1}, max_{d_2})$ als rechter oberer Punkt.

In den inneren Knoten sind also solche Regionen definiert. Sucht man nun in
einem 3-dimensionalen R-Baum nach einem Element mit den Dimensionswerten $x, y,
z$, dann muss man jede der Regionen in den inneren Knoten durchsuchen, ob der
Punkt in der Region beinhaltet ist. Da mehrere Regionen auch ueberlappen
koennen, kann es sein, dass man mehrere Wege von der Wurzel oder von einem
inneren Knoten zu den Blaettern machen muss. Bei einem Blatt angelangt (dem
Bucket), muss man dann nach dem richtigen Element endgueltig suchen. Ist ein
Element in keiner Region enthalten, terminiert die Suche.

```
{[(1, 1) (5, 5)], [(6, 5) (10, 15)]} <-- Innere Knoten
       |                |
[Tom, Jerry, Bert]  [Frank, Susi]    <-- Daten (Buckets)
```

Hier ist also beispielsweise eine Region im Raum, wo $min_{d_1} = min_{d_2} = 1$
und $max_{d_1} = max_{d_2} = 5$. Die erste Dimension $d_1$ koennte hierbei
z.B. das Alter der Elemente (Menschen) sein und $d_2$ die Anzahl ihrer Freunde.

### Operationen

Hier nun die Erlaeuterung der Operationen auf einem anfangs leeren
2-dimensionalen R-Baum mit den Dimensionen $d_1 := \text{ Alter}$ und $d_2 :=
\text{ Gehalt}$. Sei $k = 2$ die maximale Kapazitaet der Knoten.

#### Einfuegen

Wir gehen von einem anfaenglich Datensatz von $2$ Elementen $\{A = (20, 100K), B
= (33, 2K)\}$ aus. Nun initialisieren wir die Datenstruktur, indem wir eine
Region vom Minimum der beiden Dimensionen (links unten) bis zum Maximum der
beiden Dimensionen (rechts oben) festlegen. Diese Region ist in einem inneren
Knoten und verweist auf den Bucket, der die beiden Elemente enthaelt.

```
{[(20, 2K), (33, 100K)]}
          |
	    [A, B]
```

Nun fuegen wir ein neues Element $C = (10, 17K)$ ein. Beim Einfuegen gibt es in
jedem inneren Knoten drei Moeglichkeiten:

1. Das Element faellt in eine Region.
2. Das Element faellt in mehrere Regionen, da diese ueberlappen koennnen.
3. Das Element faellt in keine Region.

__Man muss nie explizit eine neue Region einfuegen, sondern immer nur Regionen
erweitern!__ (Ausser am Anfang). Das geschieht nur, wenn ein Bucket voll wird.

Es kann sein, dass ein Element in einem inneren Knoten in eine Region faellt,
aber dann im folgenden inneren Knoten in keine Region faellt. Es kann aber auch
sein, dass ein Element schon in der Wurzel in keine Region passt. Wenn ein
Element nicht in eine Region passt, muss man eine der anderen
vergroessern. Hierbei gilt es jene Region zu waehlen, die man dabei am wenigsten
vergroessern muss, um den Index eben moeglichst fein zu halten.

Fuer $C$ stellt sich hier heraus, dass es zwar in eine Region faellt, der
einzige Bucket schon zu voll ist. Nun muss also ein Verfahren zur
Re-Partitionierung der Elemente durchgefuehrt werden mit dem Ziel, die
existierenden Elemente in zwei Regionen aufzuteilen. Es gibt dafuer mehrere
Algorithmen, wobei das Ziel immer ist, die neu entstandenen Regionen moeglichst
klein und den Index somit moeglichst praezise zu halten, da zu grosse Regionen
einen groesseren letztendlichen Suchaufwand als Resultat fuehren. Bei normalen
Kapazitaeten von ca. 100 Elementen pro Seite (Bucket) ist eine optimale
Partitionierung aber sehr schwierig.

Hier waere nun also eine moegliche Partionierung $\{\{A, C\}, \{B\}\}$, da diese
in der Ebene relativ nahe bei-einander sind. Nun sieht der Index also so aus:

```
{[(10, 2K), (20, 17K)], [(33, 100K), (33, 100K)]}
          |                         |
	    [A, C]                     [B]
```

Nun fuegen wir wieder ein Element in Bucket von $\{A, C\}$ um zu zeigen was
passiert, wenn ein innerer Knoten overflowed. Sei $D = (15, 20K)$ also das neue
Element. Es passt in keine Region, aber die von $\{A, C\}$ muss am wenigsten
vergroessert werden. Wir vergroessern diese Region also, wobei das Maximum des
Gehalts von $17K$ auf $20K$ waechst (die Region waechst also in die Hoehe).

```
{[(10, 2K), (20, 20K)], [(33, 100K), (33, 100K)]}
          |                         |
	    [A, C, D]                  [B]
```


Nun muessen die Elemente $\{A, C, D\}$ neu partitionert werden.  Da $D$ vom
Alter gleich weit distanziert von $A = (20, 2K)$ wie von $C = (10, 17K)$ ist,
diskriminieren wir nach dem Gehalt, wonach $D$ naeher bei $C$ ist. Eine neue
Partionierung ergibt also den folgenden Baum:

```
{[(10, 17K), (15, 20K)], [(33, 100K), (33, 100K)], [(20, 2K), (20, 2K)]}
          |                         |                       |
	    [C, D]                     [B]                     [A]
```

Da $k = 2$ gewaehlt wurde overflowed der innere Knoten. Nun werden die Regionen
ebenso wie die Punkte (Elemente) vorher partitioniert. Man versucht also die
Regionen so gut wie moeglich durch umfassende Regionen zu beschreiben. Dabei
sollten die umfassenden Regionen wieder moeglichst klein sein. Die Region von
$\{C, D\}$ ist naeher bei der von $A$, also gruppieren wir diese.

Nun wird die Region von $\{C, D \}$ und $A$ also durch eine umfassende,
minimale Region zusammengefasst, wieder durch zwei Punkte vom Minimum der
Dimension zum Maximum der Dimensionen: $[(10, 2K), (20, 20K)]$. $B$ bleibt in
einer Klasse nur mit sich selbst. Man muss jetzt also eine umfassende Region von
$B$ erstellen, die eben genau die Region von $B$ ist. Dies ist in diesem Fall
unvorteilhaft, aber ein R-Baum ist eben __balanciert__! Die neuen Regionen
werden in den Elternknoten des inneren Knoten geschoben. Da der innere Knoten
hier aber die Wurzel ist, muss ein neuer Elternknoten allokiert werden:

```
             {[(10, 2K), (20, 20K)], [(33, 100K), (33, 100K)}
                       /                                 \
{[(15, 17K), (10, 20K)], [(20, 2K), (20, 2K)]}      {[(33, 100K), (33, 100K)]}
          |                         |                       |
	    [C, D]                     [A]                     [B]
```

Wie bei B-Baeumen wachsen R-Baeume also immer nur nach oben.

### Suche

Man will nun also eine Bereichsanfrage nach
$[(10, 10K), (50, 200K)$ durch den R-Baum machen:

```
             {[(10, 2K), (20, 20K)], [(33, 100K), (33, 100K)}
                       /                                 \
{[(15, 20K), (10, 17K)], [(20, 2K), (20, 2K)]}      {[(33, 100K), (33, 100K)]}
          |                         |                       |
	    [C, D]                     [A]                     [B]
```

In diesem Fall ueberlappt das Anfragefenster mehrere Regionen, also wird man
mehrere Wege gehen muessen -- ein unguenstiger Fall. Dennoch:

1. Fuer jede Region im Knoten evaluiere: __uberlappt__ die Region mit dem
   Anfragefenster?
   + Wenn Ja, dann rekursiere.
   + Wenn Nein, dann gehe weiter.
2. Gelangt man an ein Blatt, muss man auch hier linear durchsuchen. Da Regionen
   sich ueberlappen koennen, kann es sein dass man beim Rekursieren durch die
   eine "false-positive" Region zu einem Bucket kommt, in welchem gar kein
   Element im Bucket wirklich im Anfragefenster voll enthalten ist (die Region
   muss ja nur ueberlappen). Gilt natuerlich besonders wenn man eine
   Punktanfrage macht.

Anfragen fuer Elemente, also Punkte, sind analog.

http://stackoverflow.com/questions/4326332/could-anyone-tell-me-whats-the-difference-between-kd-tree-and-r-tree/11109467
