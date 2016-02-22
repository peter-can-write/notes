# Physische Organisation

Natuerlich bezieht sich ein grosser Teil der Datenorganisation von
Datenbanksystemen auf die physische Organisation -- wie die Daten wirklich
physisch gespeichert werden. Die Eigenschaften der Speichermedien und
Datenstrukturen bestimmen letztendlich die Verfuegbarkeit, Schnelligkeit und
Verlusttoleranz der Daten.

## Speichermedien

Man unterscheidet bei Speichermedien typischerweise drei verschiedene Stufen:

1. Hauptspeicher (Primaerspeicher)
2. Hintergrundspeicher (Sekundaerspeicher)
3. Archivspeicher

Hierbei unterscheiden sich diese Medien (der Listenordnung) nach absteigend in
Zugriffsgeschwindigkeit sowie aufsteigend in Speicherkapazitaet.

### Hauptspeicher

Der Hauptspeicher liegt nahe am Rechner und ist der schnellste, aber
ueblicherweise auch kleinste Speicher in der Hierarchie. Man kann auf beliebige
Adressen direkt zugreifen und letzendlich muessen alle Datenmodifikationen im
Hauptspeicher durchgefuehrt werden (oder zumindest nach den Caches darin
landen). Meist hat der Hauptspeicher auch keine Mechanismen gegen
Systemausfaelle (z.B. Stromausfall).

### Hintergrundspeicher

Der Hintergrundspeicher ist nun ein groesserer aber auch viel viel langsamerer
Speicher. Langsamer bedeutet hierbei zum einen, dass der direkte Speicherzugriff
auf ein Datum schon laenger dauert, aber dann auch, dass der Transport des
Datums vom Hintergrundspeicher in den Hauptspeicher auch wieder lange
dauert. Konkret ist das Medium hierbei meist die typische Festplatte, wie man
sie von Datenbanksystemen kennt (zylinderfoermig).

Ein solcher Plattenspeicher ist in einzelne Platten organisiert, die man auch
*Zylinder* nennt. Viele solcher Platten koennen uebereinander liegen und um eine
zentrale Achse -- die *Spindel* -- rotieren. Auf einer konkreten Platte gibt es
dann *Spuren*, also Kreise um die Achse, die die eigentlichen Daten
enthalten. Man organisiert eine Platte dann auch noch meist in Sektoren, also
Tortenstuecke. Um Daten aus Spuren zu lesen gibt es dann *Schreib-/Lesekoepfe*,
welche an *Schreib-/Lesearmen* befestigt sind. Hierbei gibt es fuer jede Platte
einen eigenen Arm und Kopf, welche also parallel lesen koennen.

Waehrend man bei Hauptspeichern noch auf beliebige Adressen zugreifen konnte,
ist die *Granularitaet* von Hintergrundspeicherzugriffen *groeber*, spricht man
muss bei einem Zugriff mehr Daten lesen, als es bei einem Primaerspeicher noetig
ist. Diese Granularitaet ist dabei immer (abstrakt) ein *Block*, was ein relativ
kleiner Adressraum ist (z.B. 512 Bytes). Datebanksysteme arbeiten dann aber
meist mit einer groesseren Basiseinheit: einer *Seite*. Eine Speicherseite
ist meist ein bis acht KiB und fasst mehrerere Bloecke (z.B. acht)
zusammen. Eine Seite liegt dabei immer sequentiell in einer Spur auf einer
Platte.

Um nun ein eine Seite zu adressieren braucht man also:

1. Zylindernummer (welche Platte)
2. Spurnummer (welcher Kreis)
3. Sektornummer (welches Tortenstueck)

Hat man diese drei "Koordinaten", kann man also auf der Platte, innerhalb des
Sektors, aus der Spur eine Seite lesen, welche viele Bloecke enthaelt. Bei einem
solchen Zugriff gibt es dann drei Schritte, die benoetigt sind, um eine solche
Seite zu erreichen:

1. Der Schreib-/Lesekopf muss auf die entsprechende Spur platziert werden. Dazu
   wird der Kopf weiter nach vor oder weiter nach hinten innerhalb seines Arms
   bewegt. Die hierfuer benoetigte Zeit nennt man *Seek Time*. Man kann hier
   ungefaehr von 5 ms ausgehen.

2. Dann muss die Platte rotiert werden, um an die Startadresse des ersten Blocks
   der gesuchten Seite zu gelangen. Diese Wartezeit nennt man *Latenzzeit*. Im
   Durchschnitt muss sich hier die Platte halb drehen, was ungefaehr 3 ms
   dauert.

3. Letzendlich muessen die Daten bzw. die Bloecke innerhalb der Spur sequentiell
   gelesen werden. Diese Zeit nennt man *Lesezeit*, und haengt natuerlich von
   der zu lesenden Datenmenge ab.

#### Random vs Chained I/O

Wenn der Schreib-/Lesekopf erst einmal auf eine Spur gerichtet wird, kann sich
die Platte mit ungefaehr 10000 Umdrehungen pro Minute drehen. Offensichtlich ist
es also viel effizienter, viele Daten aus einer Spur sequentiell zu lesen, als
viele Daten aus verschiedenen Spuren an nicht-sequentiellen Stellen (also mit
Unterbrechungen dazwischen) zu lesen. Genau deswegen unterscheiden wir zwischen:

1. Chained I/O: Sequentielles Lesen aus einer einzigen Spur. Ein solcher Zugriff
   erfordert nur einmal Seek Time und Latenzzeit.

2. Random I/O: Lesen aus mehreren Spuren oder, innerhalb einer Spur, aus
   nicht-sequentiellen Sektoren. Solche Zugriffe sind natuerlich sehr, sehr
   teuer, weil man womoeglich fuer jeden Zugriff den Kopf neu positionieren
   (Seek Time) und innerhalb der Spur an die richtige Startadresse bringen
   (Latenzzeit) muss.

Nehmen wir ein Beispiel. Wir wollen 1000 Datenbloecke lesen. Sagen wir, es
dauert 300ms um diese Datenbloecke vom Plattenspeicher zum Prozessor zu
transportieren. Seien diese Bloecke dann:

* In einer einzigen Spur sequentiell gelegen. Dann hat man einmal Seek-Time
  ($\sim$ 5 ms), einmal Latenzzeit ($\sim$ 3 ms) und dann die Transportzeit von
  300ms, also insgesamt 308 ms.

* Komplett zerstreut, nicht-sequentiell in verschiedenen Spuren gelegen. Dann
  hat man fuer jeden Block Seek-Time und Latenzzeit, plus letztendlich die
  Transportzeit. Wir sprechen hier also von $1000 \cdot (3 + 5) = 8000$ ms, also
  8s.

Wie man sieht ist der Unterschied zwischen chained und random I/O gewaltig! Hier
fast zwei Groessenordnung ($10^2$).

### Archivspeicher

Als Archivspeicher werden oft Magnetbaender verwendet, welche sehr
fehlertolerant und auch vorallem sehr sehr billig sind. Auf diesen Speichern
werden also sehr grosse Datenmengen gespeichert, welche dann aber auch nur sehr
langsam gelesen bzw. geschrieben werden koennen.

## Speicherhierarchie

Wie man stehen bei der Diskussion von Speichermedien zwei Eigenschaften im
Gegensatz zu einander: Speicherkapazitaet und Zugriffsschnelligkeit. Um also
dennoch ein Maximum an Zugriffsschnelligkeit zu haben, und ein Maximum an Daten
speichern zu koennen, muessen wir die oben beschriebenen Speichermedien
kombinieren. Wir sprechen also von einer *Speicherhierarchie*.

In dieser Speicherhierarchie gibt es nun sehr kleine, schnelle Speicher sowie
auch sehr grosse, langsame Speicher. Genau sind die benutzten Speichermedien die
folgenden, wobei die Reihenfolge absteigend nach Schnelligkeit und aufsteigend
nach Kapazitaet gewaehlt wurde:

1. (Prozessor-)Register. Von diesen kann es maximal ein paar Dutzend oder
   Hundert geben, wobei sie auch nur sehr kleine Datensaetze, meist einen
   bis 8 Byte, speichern koennen. Sie sind aber relativ gesehen extrem schnell,
   mit einer Zugriffszeit von etwa einer Nanosekunde ($10^{-9}$).

2. Caches. Es gibt meist zwei Caches im Prozessor selbst und noch einen dritten
   Cache ausserhalb. Diese drei Caches, mit L1, L2 und L3 bezeichnet,
   unterscheiden sich auch wieder in Schnelligkeit und Kapazitaet:

   * L1 Cache: einige hunder KiB.
   * L2 Cache: einige MiB.
   * L3 Cache: mehr MiB als in L2.

   Die Zugriffszeit auf diese Caches ist noch immer im Nanosekundenbereich. Das
   haengt aber natuerlich stark vom Hersteller ab.

3. Hauptspeicher. Der Hauptspeicher enthaelt in Datenbanksystemen meist schon
   einige GiB. Die Zugriffszeit ist hier ungefaehr 100ns.

^
|
| 10^5
|
v

4. Hintergrundspeicher. Der Hintergrundspeicher, also die Platte, hat wiederum
   eine groessere Kapazitaet als der Hauptspeicher. Aber die Zugriffszeit auf
   den Hintergrundspeicher ist hier schon 10 __ms__, also 10.000.000 ns (zehn
   Millionen). Wir sprechen also zwischen dem Haupt- und Hintergrundspeicher von
   einer *Zugriffsluecke* von __fuenf Groessenordnungen ($10^5$)__!

5. Archivspeicher. Ein Zugriff auf den Archivspeicher mag schon mindestens eine
   Sekunde dauern, also eine enorm hohe Zeit. So ein Zugriff sollte
   logischerweise sehr, sehr selten noetig sein.

Visualisierung:

```
                 /\
                /  \
               /    \
              /      \
             /        \
		    / Register \
           /            \
          /    Cache     \
         /                \
        /  Hauptspeicher   \
       /                    \
	  / Hintergrundspeicher  \
     /                        \
    /      Archivspeicher      \
   /____________________________\
```

## RAID

RAID (redundant array of inexpensive/independent disks) ist eine
Speichertechnologie, die anstelle eines einzigen grossen Laufwerks fuer den
Hintergrundspeicher nun viele kleine, billigere Laufwerke parallel
betreibt. Diese preiswerten Laufwerke arbeiten durch einen entsprechenden
*RAID-Controller* nach aussen hin scheinbar (transparent) wie ein einziges
logisches Laufwerk.

Es gibt nun mehrere Moeglichkeiten, Daten auf diese vielen einzelnen Laufwerke
zu verteilen. Ziel einer jeglichen Verteilung ist es jeweils immer, die
Zugriffzeit zu erhoehen aber auch die Fehlertoleranz zu minimieren. Je nach
Anforderungen, z.B. wenn mehr Lese- oder mehr Schreibzugriffe stattfinden, ist
eine Verteilung besser als die andere. Unter den nachfolgend vorgestellten
Organisationsmodellen von RAID-Systemen gibt es also kein "bestes".

Wir sprechen bei diesen verschiedenen Organisationsmoeglichkeiten jeweils von
RAID-Leveln. Insgesamt gibt es acht RAID-Level: RAID 0 bis Raid 6, und dann noch
RAID 0+1 (oftmals: RAID 10).

### Kein RAID

Wir betrachten zuerst, wie Daten ohne RAID gespeichert wuerden. Haetten wir vier
Datenbloecke $ABCD$, dann waere die naive Variante, sie einfach sequentiell in
*eine* Festplatte zu geben:

```
|A B|
|C D|
```

Hier koennte also immer nur ein Schreib-/Lesekopf sequentiell Daten lesen.

### RAID 0

In RAID Level 0 wuerden zur Lastbalancierung die obigen vier Datenblocke $ABCD$
nun auf mehrere Festplatten aufgeteilt, sodass man die doppelte Bandbreite beim
sequentiellen Lesen der Datei erhaelt:

```
|A| |B|
|C| |D|
```

Hier kann man nun also schon zwei Schreib-/Lesekoepfe haben, die gleichzeitig
Datenbloecke lesen. Hier haette man also in der halben Zeit die Daten $ABCD$
gelesen. Diese Idee, Daten auf verschiedene Festplatten zu verteilen nennt man
*Striping*. Wieviele Daten pro *Stripe* gespeichert sind wird dann durch die
*Striping-Granularitaet* angegeben. Die Anzahl der benutzten Festplatten nennt
man dann die *Striping-Breite*.

Was man hier auch bemerken kann, ist dass wenn ein Datum auf ganz viele
Festplatten aufgeteilt ist, und davon nur eine einzige kaputt geht, das ganze
Datum verloren geht. Die Datensicherheit ist also relativ gering.

### RAID 1: Mirroring

Waehren RAID 0 die Zugriffzeit optimiert, optimiert RAID 1 die
Datensicherheit. Einfach genug werden hier die Daten zwar noch wie im Fall ohne
Raid sequentiell in einer Festplatte abgelegt, aber es gibt nun von jeder
Festplatte auch eine *Kopie*. Diese Kopie wird *Spiegelkopie* genannt, im
Englischen dann *mirror. Daten werden also redundant abgespeichert und man hat
somit sicher doppelten Speicherbedarf.

```
|A B| |A B|
|C D| |C D|
  1     2
```

Die Vorteile dieser Organisation sind nun aber, dass ein defekter Block nicht
verloren geht, sofern seine Spiegelkopie nicht auch verloren geht (was
natuerlich unwahrscheinlich ist). Weiterhin ist es hier auch moeglich
*Lese*zugriffe auf die beiden Kopien zu verteilen. Somit werden also die
einzelnen Festplatten weniger haeufig belastet und halten laenger. Bei
*Schreib*zugriffen muessen nun aber beide Festplatten beschrieben werden. Da
dies parallel geschieht, ist es aber nicht problematisch. Der einzige Nachteil
ist hier also der doppelte Speicherbedarf.

### RAID 0+1

In RAID 0+1 werden RAID 0 und RAID 1 einfach kombiniert. Die Daten werden also
gestriped, und dann redundant gespeichert:

```
|A| |A| |B| |B|
|C| |C| |D| |D|
 1   2   3   4
```

### RAID 2: Striping auf Bit-Ebene

Waehren din RAID 0 und RAID 0+1 Datensaetze noch in Bloecken gestriped wurden,
wird in RAID 2 (und 3) auf der Bit- oder Byte-Ebene gestriped. Es werden also
manchmal sogar die Bits eines Datums auf die einzelnen Festplatten verteilt,
sodass z.B. bei acht Festplatten parallel ein Byte gelesen werden kann. Sei
z.B. ein Datum in Binaerkodierung so gegeben: $10101101_2$. Dann wuerde es auf
vier Festplatten so gestriped:

```
|1| |0| |1| |0|
|1| |1| |0| |1|
```

Von links nach rechts und dann von oben nach unten werden also Bits
eingefuellt. Zusaetzlich dazu werden dann noch Fehlererkennungscodes
gespeichert. In Praxis wird RAID 2 nicht eingesetzt, weil meist die
RAID-Controller schon diese Fehlererkennung eingebaut haben.

Von der Zugriffszeit her ist diese Form von Organisation auch etwas anders als
die vorherigen RAID Stufen. Hier kann naemlich ein Datenblock zwar relativ
schnell parallel gelesen werden, aber der Zugriff auf einen Datenblock erfordert
dann auch alle Schreib-/Lesekoepfe. Vorher wurden also Daten als ganzes schnell
gelesen, hier werden Datenbloecke schnell gelesen. Da aber hier auch auf alle
Platten zugegriffen wird, ist die Lastbalancierung schlechter, also werden die
Platten auch statistisch schneller kaputt.

Hierbei koennen auch die zwei Arten von Parallelitaet angesprochen werden:
Zugriffsparallelitaet und Auftragsparallelitaet. Ein RAID System kann
*Zugriffsparallelitaet* aufweisen, wenn ein *einziger* Datenzugriff parallel
ausgefuehrt werden kann. Das ist z.B. bei Bit-Striping der Fall. Sprechen wir
aber von *Auftragsparallelitaet*, dann meinen wir damit, dass viele separate
Ein-/Ausgabe-Auftraege parallel ausgefuehrt werden koennen. Man kann also auf
viele verschiedene Daten parallel zugreifen, und dann mit keiner oder weniger
Parallelitaet auf ein einziges Datum. Block-weises Striping haette eher
Auftragsparallelitaet.

### RAID 3: Striping auf Bit-Ebene mit Paritaetsplatte

RAID 3 speichert nun zum ersten Mal auch *Paritaetsinformation* zu den
Daten. Die Idee ist folgende: Wir haben unser Datum $10101101_2$ also auf acht
Festplatten gestriped. Dann bilden wir nun ein logisches bitweises XOR dieser
Bits, und speichern das ein-bittige Resultat als Paritaetsinformation. Hierbei
wird nach der Semantik von XOR $\oplus$ dieser letzte Bit eins sein, wenn die
Anzahl an gesetzten Bits ungerade war und null sein, wenn die Anzahl an Einsen
im Datum gerade war. Fuer das Beispieldatum also: $1 \oplus 0 \oplus 1 \oplus 0
\oplus 1 \oplus 1 \oplus 0 \oplus 1 = 1$.

Jetzt haben wir also einen Paritaetsbit fuer unser Datum bestimmt, nun speichern
wir dieses Paritaetsbit bei RAID 3 in einer zusaetzlichen Festplatte, welche
also nur Paritaetsbits enthaelt. Was uns das bringt ist, dass wenn *eine*
Festplatte ausfaellt, wir den Bit, der auf dieser Festplatte gespeichert war,
wieder rekonstruieren koennen. Dazu fuehren wir einfach wieder ein bitweises XOR
zwischen den verblieben Bits des Datum und dem Paritaetsbit durch. Nach der
Semantik der XOR-Operators ergibt sich hierbei der fehlende Bit. Wir haetten
unser Datum also rekonstruiert.

Nehmen wir wieder obiges Beispiel, fuer welchen wir den Paritaetsbit $1$
ermittelt haben. Nun sagen wir, Festplatte 4 faellt aus. Das bedeutet, dass wir
(ohne RAID) den vierten Bit von links, eine $0$, verloren haetten. Dank der
Paritaetsinformation koennen wir diesen Bit nun aber wieder rekonstruieren,
indem wir alle verbliebenen sieben Bits mit dem Paritaetsbit XOR-en: $$1 \oplus
0 \oplus 1 \oplus 1 \oplus 1 \oplus 0 \oplus 1 \oplus \underline{1} = 0$$. Wie
man sieht, ist hierbei also der vermeintlich verloren Bit wieder rekonstruiert.

Man beachte aber, dass hier auch nur immer eine Festplatte aufallen darf. Sobald
eine zweite ausfaellt, funktioniert obiger Mechanismus zur Rekonstruierung nicht
mehr.

### RAID 4: Striping von Bloecken mit Paritaetsplatte

Bei RAID 4 werden Daten nun wieder *blockweise*, anstatt bit- oder byteweise
balanciert (gestriped), was eine bessere Lastbalancierung ergibt. Zusaetzlich
gibt es dann noch eine Paritaetsplatte, auf welcher nun *Paritaetsblocke*
gespeichert werden. Ein Paritaetsblock enthaelt dann also fuer jeden Bit der
Bloecke einen Paritaetsbit.

Als Beispiel sei das Datum mit den Bloecken $ABCDEFGH$ betrachtet. Gestriped
wuerde dann auf vier Platten wie folgt:

```
|A| |B| |C| |D|
|E| |F| |G| |H|
```

Wenn wir annehmen, das Daten hier alle zwei Bloecke sind (anstatt das eine
8-Block Datum), dann merkt man schon auch, dass hier die Lastbalancierung wieder
besser ist als bei RAID 3. Man muesste nur auf zwei Platten zugreifen, anstatt
auf alle.

Zur Paritaetsinformation werden nun fuer verschiedene Bloecke in einer eigenen
Festplatte Paritaetsbloecke gespeichert. Beispielsweise kann es einen
Paritaetsblock fuer $ABCD$ und einen fuer $EFGH$ geben. Diese werden dann mit
$P_{ABCD}$ und $P_{EFGH}$ angegeben.

Der Flaschenhals bildet nun aber die Paritaetsplatte, da bei jedem
*Schreibzugriff* die Paritaetsinformation fuer die Bloecke eines Paritaetsblock
neu gebildet werden muss. Wurde hierbei ein Block $A$ zu $A'$ geandert, muesste
Paritaetsblock $P_{ABCD}$ wie folgt neu berechnet werden: $$P_{ABCD}' :=
P_{ABCD} \oplus A \oplus A'$$. Der alte Wert muss also raus-ge-XOR-ed werden
(ein erneutes XOR macht ein vorheriges XOR immer rueckgaengig, so ist die
Semantik von XOR) und der neue Wert rein-ge-XOR-ed.

### RAID 5: Paritaetsbloecke in den einzelnen Platten

Hier wird nun der Paritaetsblock fuer jede Gruppe von Datenbloecken
(z.B. $P_{ABCD}$) direkt in einer Platte neben anderen Datenbloecken (nicht
unbedingt $ABCD$) gespeichert, und nicht mehr separat in einer eigenen
Paritaetsplatte. So bildet diese Paritaetsplatte keinen Flaschenhals mehr, und
man hat einen guten Ausgleich zwischen Speicherbedarf und
Leistungsfaehigkeit. In der Praxis wird diese Methode also haeufig eingesetzt.

## Datenbankpuffer

Der Hintergrundspeicher ist lediglich zum Speichern von Daten nutzbar, nicht
aber zum operieren darauf. Das bedeutet, dass man Seiten effizient zwischen
Hauptspeicher und Hintergrundspeicher verwalten muss.

Generell gibt es hierbei im Hauptspeicher sogenannte *Pufferrahmen*, worin man
eine Seite also vom Hintergrundspeicher *einlagern* kann, und eventuell wieder
*auslagern* muss. Denn wir erinnern uns, ein Hauptspeicher ist meist volatil,
also verliert seine Daten, wenn der Strom ausgesteckt wird. D.h., Daten muessen
letztendlich immer wieder zurueck in den Hintergrundspeicher geschrieben
werden.

Ausserdem gilt, dass nicht jede Seite immer im Hauptspeicher Platz hat. Wenn
also eine neue Seite aus dem Hintergrundspeicher geholt werden muss, dieser aber
voll ist, also keine freien Rahmen mehr hat, dann muss eine sich im
Hauptspeicher befindliche Seite ausgelagert werden, um Platz zu schaffen. Welche
Seite dabei ausgelagert wird, haengt von der *Erseztungsstrategie* ab. Eine
moegliche und auch effektive Strategie waere es, immer die Seite zu entfernen,
die am wenigsten Zugriffe hatte. Eine weitere waere es, immer die Seite zu
entfernen, die schon am laengsten im Puffer ist.

## Speichern von Relationen

Wir wissen bereits, wie Relationen aufgebaut sind. Eine Auspraegung einer
Relation enthaelt immer Tupel, und diese Tupel die einzelnen Attribute
bzw. deren konkrete Werte. Wie wird aber nun eine Relation physisch gespeichert?
Und vorallem, wie werden konkrete Tupel dann gefunden?

Die einfachste Antwort waere natuerlich, die Tupel einer Relation einfach
sequentiell im Haupt- bzw. Hintergrundspeicher abzulegen. Eine Suche waere dann
sequentiell. Aber diese Methode laesst viele Fragen offen. Was passiert, wenn
ein Tupel waechst, beispielsweise weil ein String, der als `varchar(100)`
deklariert wurde, von einem zu 100 Zeichen waechst? Was, wenn wir zur Vermeidung
von Fragmentierung ein Tupel innerhalb einer Seite bewegen wollen? Was, wenn auf
der Seite nach Tupelwachstum kein Platz mehr fuer ein Tupel ist?

Wie man sieht, ist diese Methode keine gute Loesung. Es gibt aber die Weisheit,
dass "every problem in software engineering can be solved by an extra level of
indirection". Nun, diese Weisheit gilt wohl auch fuer Hardware. Wir machen also
folgendes: Auf dem Hintergrundspeicher werden Daten mit Tupel-Identifikatoren
(TIDs) identifiziert. Diese bestehen aus der Seitennummer und einem
Indentifikator fuer ein Tupel in der Seite.

Wichtig hierbei ist, dass dieser Identifikator keine Speicheradresse ist,
sondern einfach eine Zahl (ein ID) die dem Tupel zugewiesen wird. Die Seite
verwaltet dann eine interne Suchtabelle zwischen diesen Zahlen zu
Speicheradressen.

Hierdurch werden alle oben angesprochenen Probleme geloest. Am trivialsten ist
das Problem des Seiten-internen Umbewegens: Da der TID nur eine logische
(virtuelle) und keine physische Adresse ist, ist es dem TID vollkommen egal, wo
das Tupel innerhalb der Seite lebt. Wenn ein Tupel also umziehen muss, muss die
Seite einfach ihren Zeiger fuer den ID-Teil (im Gegensatz zum Seiten-Teil) auf
die physische Speicheradresse modifizieren.

Was aber, wenn ein Tupel auf einer Seite keinen Platz mehr hat? Dann wird obige
Weisheit nochmals angewandt: eine weitere "level of indirection". Es wird dann
einfach an der Stelle des Tupels innerhalb der Seite, in der kein Platz mehr
ist, ein *Forward* eingerichtet. Ein Forward ist einfach ein weiterer TID. Will
man also anhand des (noch immer gleichen) TID des Tupels das Tupel aufrufen,
wird die erste Seite zuerst diesen Forward adressieren. Wenn es sieht, dass dies
ein Forward ist und kein echtes Datum, wird das echte Tupel also von der
korrespondierenden Seite geholt, wo es "hin-geForwarded" wurde.

Hierbei sei aber angemerkt, dass dieses Forwarding sich auch auf genau diese
Situation beschraenkt. Wenn das Tupel auf seiner zweiten Seite auch keinen Platz
mehr hat, dann wird der nicht noch ein zweiter Forward eingefuegt. Viel mehr
wird der alte Forward auf der ersten Seite geaendert, sodass er dann auf diese
neue (dritte) Seite verweist, und nicht mehr auf die zweite (wo auch kein Platz
mehr war).

Diese Forwards koenenn Flaschenhaelse fuer die Performance sein, da sie immer
einen weiteren Seitenzugriff erfordern. Daher wird in allen Platten etwas Platz,
z.B. 10%, freigelassen. D.h. wenn nur mehr 10% frei waeren, wuerden keine neuen
Tupel mehr eingefuegt. Somit haetten die existierenden Tupel zumindest etwas
Platz, um zu wachsen, bevor ein Forward eingefuegt werden muss.

Letztlich sei noch gesagt, dass eine weitere Eigenschaft dieser
Speicherorganisation ist, dass Tupel nie ueber Seitengrenzen hinweggehen. Es
kann also nie sein, dass die halben Attribute eines Tupels auf der einen Seite
und die anderen halben auf einer anderen Seite gespeichert werden. Das gaebe
naemlich Probleme mit der Geschwindigkeit, der Fehlertoleranz sowie auch der
Mehrbenutzersynchronisation.

## Ballung logisch verwandter Datensaetze

Das Konzept der Ballung (Clustering) ist die Idee, Daten die oft miteinander
angefragt werden, nahe bei einander (im Hintergrundspeicher/Hauptspeicherpuffer)
zu speichern. Ballung verwendet also die *raeumliche Lokalitaet* von Daten zu
seinem Vorteil. Man nehme z.B. die folgende Anfrage:

```sql
select *
from R r
where r.A = x;
```

Nun waere eine moegliche (schlechte) Aufteilung aller Tupel $t$ fuer welche gilt
$t.A = x$, dass sie weit verstreut auf mehreren Seiten im Hintergrundspeicher
liegen:

```
[a, b, c, ...] [d, e, f, ...]
[g, h, i, ...] [j, k, l, ...]
        ...
```

Eine bessere Verteilung waere jedoch eine solche:

```
[a, b, c, d, e, f, g, h, i, j, k, l] [...]
[...] [...]
    ...
```

Sind $\{a, ..., l\}$ die gesuchten Tupel, dann muss man im ersten Fall vier
Seiten aus dem Hintergrundspeicher ueber die Zugriffsluecke von $10^5$ in den
Hintergrundpuffer laden, und dann eben vier Plaetze in diesem Puffer
verbrauchen. Im zweiten Fall muss man nur eine Seite laden und speichern, was viel
effizienter ist. Das ist also das Ziel von *Ballung*.

### Realisierung

Man kann Ballung durch zwei verschiedene Indices implementieren. Ein Index ist
dabei der *Cluster-Index* bzw. *Primaer-Index* und der andere der
*Sekundaer-Index*. Der Primaer-Index ist ein Index auf die Seiten, die die
Kategorien/Ballungen, also z.B. alle Tupel $t: t.X = 1$, alle wo $t.X = 2$ etc.,
enthalten. In ihm wuerde man also nach allen Elementen einer Klasse suchen
(wobei man beachten muss, dass es mehrere Ballungsseiten geben kann). Der
Sekundaerindex ist hingegen ein Index auf die Tupel selbst, wobei ein Tupel
bzw. sein Primaerschlussel als Value einen oder mehrere TID(s) auf die Seite(n)
im Primaer-Index hat, die die Datensaetze des gesuchten Tupels wirklich
enthalten.

``` __________
   /          \
  /            \
 /   Primaer    \
/                \
[x=a]->[x=b]->[x=c] <--- Geballte Seiten
 ^ ^   _^_____/
 | |  / |
[| |][| |][...] <--- TIDs
\             /
 \ Sekundaer /
  \         /
   \_______/
```

Beim Spalten von Knoten im Primaer-Index kann es Probleme geben, da man dann
entweder fuer die alten Elemente TID-Forwards in den Knoten hinterlassen,
oder den Sekundaer-Index re-organisieren (die Verweise veraendern) muss.

## Row-Store vs. Column-Store

Eine Entwicklung in neueren Datenbanksystemen ist die Unterscheidung zwischen
Row-Store und Column-Store Datenbanken. Row-Store ist die Weise, auf welche
Relationen traditionell gespeichert werden: zeilen-, also tupelweise. D.h.,
Tupel werden immer gemeinsam, sequentiell auf einer (und nur einer) Seite
gespeichert.

Betrachten wir, welche Eigenschaften wir aus einer solchen Organisation
gewinnen:

1. Wenn wir auf Tupel als Ganzes zugreifen wollen, ist diese Organisation sehr
   gut. Wir koennen ein Tupel immer mit einem einzigen sequentiellen Zugriff
   abrufen.

2. Wenn wir aber oftmals sehr komplexe Zugriffe mit vielen Relationen aber nur
   wenigen Attributen und auch vielen Aggregationen machen wollen, dann versagt
   diese Organisation klaeglich. Man nehme sich als Beispiel eine Relation mit
   100 Attributen und einer Million Tupeln. Moechten wir nun eine Aggregation
   ueber einem Tupel machen, z.B. fuer statistische Werte den Durchschnitt
   diesen Werts analysieren, so lade wir $10^6 \cdot 99 = 99.000.000$ unnoetige
   Tupel in den Speicher. Das Problem ist klar.

Row-store ist also vorallem fuer *viele, kleine* Transaktionen mit ganzen Tupeln
nuetzlich. Ein DBMS das auf solche Transaktionen getrimmt ist, nennt man ein
on-line-*transaction*-processing (OLTP) DBSM. Durch (2) wird klar, dass eine
column-store Architektur sehr nuetzlich ist, wenn man eher wenige aber sehr
komplexe, viele Relationen und relativ wenige Attribute ueberspannende Anfragen
macht. DBMS die solche Anfragen (Transaktionen) gut koennen, nennt man ein
on-line-*analytical*-processing (OLAP) DBMS. Im heutigen Zeitalter von Big-Data
ist column-store sehr beliebt.
