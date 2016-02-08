# Speicher

Bei Speichern stehen Kapazitaet (wie viel kann gespeichert werden?) und
Schnelligkeit oft im Gegensatz.

Speicher haben ein Problem:

* grosse Speicher sind langsam
* kleine Speicher sind schnell

Um dieses Problem zu adressieren, gibt es in Rechnern eine *Hierarchie* an
Speichern, wo kleinere, schnellere Register naeher am Prozessor sind und
groessere, langsamerere Speicher weiter weg:

Register < Cache < Hauptspeicher < Hintergrundspeicher < Archivspeicher

## Cache

Ein Cache ist eine Ebene in der Speicherhierarchie, die naeher am Prozessor sind
als der Haupt- oder Hintergrundspeicher, aber weiter weg als Register. Die
Grundidee ist es, haeufig benoetigte Daten in kleinen, schnellen Speichern zu
halten, um sie nicht immer vom Hauptspeicher holen zu muessen.

Ein Cache enthaelt immer einen Ausschnitt (Cache Line) des Hauptspeichers und
alle Daten im Cache sind auch im Hauptspeicher enthalten. Natuerlich moechte
immer jene Daten im Cache haben, die aktuell verwendet werden.

Definition: "Ein *Cache* ist ein schneller, fuer die Answendung nicht
sichtbarer, automatisch verwalteter Pufferspeicher der einen Auschnitt eines
groesseren Speichers entahelt."

### Lokalitaet

Caches sind eine gute Idee, weil sie die *Lokalitaet* von Daten
beruecksichten. Es gibt zwei Arten von Lokalitaet:

* Zeitliche Lokalitaet: Wenn ein Programm auf eine Adresse zugreift, ist es
  wahrscheinlich dass es kurz darauf wieder auf die selbe Adresse zugreifen
  wird.

* Raeumliche Lokalitaet: Es wahrscheinlich, dass nahe Adressen oft zusammen
  gebraucht werden.

### Aufbau

Jeder Cache braucht eine *Vergleicherlogik*, die fuer eine bestimmte Adresse
bestimmt, ob das Datum, das an dieser Adresse im Hauptspeicher liegt, gerade im
Cache ist -- ein sogenannter *Cache-Hit* -- oder nicht -- ein *Cache Miss*.

Bei einem Cache-Hit kann das Datum aus dem Cache geladen werden, bei einem Miss
muss es aus dem Hauptspeicher geholt und im Cache abgelegt werden.

Ein Cache ist meist in *Zeilen* (Cache-Lines) organisiert. Die Groesse dieser
einzelnen Lines ist hierbei meist ein Vielfaches der Wortgroesse, z.B. zwischen
$32$ und $128$ Bytes. In diese Lines werden, wenn ein Datum vom Speiche geholt
werden muss, mit einem ganzen Datenblock aus dem Speicher gefuellt. Der Grund
ist, dass das Laden von groesseren Datenbloecken im Vergleich zu einzelnen Daten
(z.B. Bytes oder Wortern) sehr billig ist. Auf Grund der raeumlichen Lokalitaet
ist es auch wahrscheinlich, dass Daten in diesem Block zusammen angefragt werden
(wenn auch zeitversetzt).

*Tags* identifizieren dabei eine Cache Line bezueglich dem Ausschnitt des
 Hauptspeichers, den sie enthalten. So kann die Vergleicherlogik bestimmen, ob
 der Ausschnitt im Cache der gesuchte ist. Es gibt dann aber noch verschiedene
 Implementierungen fuer den genauen Lookup.

#### Level

Es gibt meistens mehrere Cache-Level, die sich in ihrere Naehe zum Prozessor
bzw. Groesse (also auch Schnelligkeit) unterscheiden. Oftmals gibt es drei
Level:

* Level 1 (L1) Cache: Der kleinste, naehste und schnellste.
* Level 2 (L2) Cache: Der mittlere.
* Level 3 (L3) Cache: Der groesste, am weitesten entfernteste und langsamste.

Die ersten zwei Level sind oft noch auf dem Prozessor, das letzte nicht mehr.

#### Ueberlegungen

Entwurfsueberlegung fuer Cache Implementierungen beinhalten:

##### Die Platzierungs- und Ersetzungsstrategie:

+ In welche Cache-Zeilen wird ein Speicherbock eingetragen?
+ Welcher Speicherblock muss zur Platzierung aus dem Cache entfernt werden?

##### Die Aktualisierungsstrategie

+ Bei Cache-Hit: Wann erfolgt die Aktualisierung des geaenderten Datums (wann
  wird das urspruengliche Datum im Hauptspeicher ueberschrieben)?
+ Bei Cache-Miss: Wird das Datum im Cache eingelagert, oder nicht?

Die Aktualisierungsstrategie beinhaltet auch die
*Cache-Ersetzungsstrategie*. Diese kann sein (haengt auch von der
Implementierung ab):

* Wenn eine Zeile leer ist (also invalid), schreib das Datum neue dort rein.
* Ersetze zufaellig irgendeine Cache-Zeile.
* LRU (least recently used): ersetze die Cache-Zeile, die am laengsten nicht
  mehr benutzt wurde.
* FIFO: ersetze die aelteste Cache Zeile. Einfacher zu implementieren, weil man
  sich nicht merken muss, wie alt jedes Element ist. Man kann die Daten einfach
  in dieser Reihenfolge in den Cache schreiben.

###### Schreibstrategien

Dann gibt es noch zwei Formen von Schreib-Strategien, die adressiert werden
muessen:

1. Eine Durchschreibe-Strategie (Write-Through). Fuer diese Strategie wird
   grundsaetzlich der Hauptspeicher beschriebn. Dann:

   * Bei Write-Hits wird auch der Cache aktualisiert. Wenn also
     ein Datum im Hauptspeicher veraendert wird, dann auch im Cache (sonst
     einfach invalid?). Somit sind Cache und Hauptspeicher immer konsistent, man
     hat dann aber auch keinen Performance-Gewinn durch Caches beim Schreiben.
   * Bei Write-Miss: Hier kann der betroffene Block im Hauptspeicher geaendert
     und dann auch gleich in den Cache geladen werden, oder gar nicht geladen
     werden (nur im Hauptspeicher veraendert).

2. Eine Rueckschreibe-Strategie. Hier wird grundsaetzlich eher der Cache
   veraendert. Die Aktualisierung des Hauptspeichers erfolgt dann erst bei
   Verdraengung der Cache-Zeile. Hierfuer gibt es oft fuer jede Zeile ein Dirty
   Status-Bit, das das sagt, ob die Daten in der Zeile veraendert wurden, ohne
   sie auch im Hauptspeicher zu aktualisieren. Der Vorteil davon ist der
   geringere Datenvekehr, aber Cache und Hauptspeicher sind eben auch nicht mehr
   konsistent.

##### Cache-Consistency Problem

Wie stellt man sicher, dass alle Nutzer von Hauptspeicherdaten die selbe Sicht
auf die Daten haben? Was ist wenn Daten im Cache sind und ein Core schreibt auf
sie im Cache, aber laedt sie noch nicht in den Hauptspeicher. Waehrenddessen
laedt ein anderer Core die Daten, diese sind jetzt aber alt.

#### Cache Organisation

Es gibt folgende wichtige Klassen von Cache-Organisationen:

* Voll-Assoziative Caches (fully associative cache)
* Direkt-Abbildender Cache (direct mapped cache)
* Mengen-Assoziativer Cache (set-associative cache)

##### Direct-Mapped Cache

Bei direkt-abbildenden Caches ergibt sich die Cache Zeile eindeutig aus der
Adresse. Eine Hauptspeicher-Adresse kann also nur in einer eindeutigen Zeile
abgelegt werden.

Hat der Cache beispielsweise $512$ Zeilen, so z.B. koennte ein
direkt-abbildender Cache Speicheradressen $0, 512, 1024, ...$ auf die erste
Zeile abbilden, Adressen $1, 512, 1025, ...$ auf die zweite Adresse usw. Es kann
also Kollisionen zwischen Zeilen geben -- das nennt man dan Cache *Thrashing*.

Um dies zu implementeiren, wird eine Speicheradresse in drei Teile geteilt:

1. Einen *Index* in den Cache. Dieser Index gibt also die Zeile des Cache fuer
   einen Datenblock an. Mehrere Datenbloecke koennen den selben Index haben, es
   gibt also moegliche Kollisionen.
2. Einen *Tag*, der die Speicheradrese unter jenen, die in einer Zeile
   kollidieren koennen, eindeutig identifiziert. Ist der Index also z.B. das
   Resultat der Hash-Funktion in einen Separate-Chaining-Hash-Table, so ist der
   Tag dann der Key, ueber welchen in einer Chain verglichen wird. Nur ist die
   Chain hier eben nur ein einziges Element.
3. Einen *Offset*, der das gesuchte Datum innerhalb des gespeicherten
   Datenblocks (der ja ein Vielfaches der Wortgroesse gross ist) lokalisiert.

Es wird nun also z.B. nach einem Datum gesucht, dessen 32-Bit Adresse bekannt
ist. Nun sind (als Beispiel!) die oberen $17$ Bit der Tag, danach weitere $9$
Bit der Index und letztlich weitere $6$ Bit der Offset. Zuerst wird der Index
durch einen Dekoder gegeben, um die Zeile zu identifizieren. Dann wird der Tag
des Datums gegen den gespeicherten Tag (dieser wird mit dem Daten in der Zeile
gespeichet) gematched. Ist er gleich, ist der gespeicherte Datenblock der
gesuchte -- ein Cache-Hit. Das Datum kann also am Offset im Datenblock geholt
werden. Ist der Tag aber ein anderer, so muss es sein, dass der Datenblock zu
einem der anderen Adressen gehoert, die zu diesem Index mappen -- ein Cache
Miss. Der gewuenschte Datenblock muss also aus dem Speicher geladen werden und
je nach Aktualisierungsstrategie auch im Cache eingelagert (ersetzt den alten
Datenblock).

Ein Problem von Direct-Mapped Caches ist, dass die Cache-Kapazitaet nicht immer
voll ausgenutzt wird, wenn nicht alle Indizes gefuellt werden. Die
Cache-Hit-Rate koennte also noch verbessert werden.

##### Fully-Associative Cache

Bei einem Voll-Assoziativen Cache (fully associative cache) gibt es keinerlei
praedefinierte Zuordnung von Adressen zu Speicherplaetzen. Freie Zeilen des
Cachespeichers koenen daher voellig frei benutzt werden.

Wenn nun aber ein Datenblock in jeder beliebigen Cache-Zeile stehen kann,
bedeutet das auch, dass das man in jeder beliebigen Zeile nach dem Datenblock
bzw. dessen Adresse suchen muss. Zum einen bedeutet das nun, dass keine Bits in
der Adresse fuer den *Index* benutzt werden muessen, sondern voll auf den *Tag*
und den *Offset* aufgeteilt werden koennen.

Will man nun also bestimmen, ob das Datum mit der bestimmten Adresse im Speicher
ist, kann man nicht mehr wie beim Direct-Mapped Cache zu nur einer bestimmten
Zeile gehen, sondern muss alle Zeilen durchsuchen. Hierbei sei aber angemerkt,
dass diese Suche nicht wie in Software sequentiell ist. In Hardware koennen ja
viele elektrische Signale parallel wandern, also ist der Zeit-Overhead nicht
*sehr* gross. Dennoch gibt es doch einen Overhead (no free lunch).

Der Hauptunterschied ist, dass man nun nicht mehr einen einzigen
Tag-Komparator braucht, um den Tag des gesucheten Datums mit dem Tag des in der
Zeile gespeicherten Datums zu vergleichen, wie es beim Direct-Mapped Cache der
Fall ist. Man braucht nun fuer *jede Zeile* einen eigenen Komparator. Dies hat
die Wirkung, dass ein solcher Cache einen *viel komplexeren* Aufbau hat.

Vorteile:
+ Volle Ausnutzung der Cache-Kapazitaet

Nachteile:
- Komplexer Aufbau
- Langsamer
- Teuer

https://www.youtube.com/watch?v=YAz0qJf05ko

##### Set-Associative Cache

Direct-Mapped Caches sind sehr schnell, weil es fuer eine konkrete Adresse nur
eine eindeutige Zeile gibt. Jedoch wird bei solchen Caches die Kapazitaet oft
nicht ausgenutzt, die Hit-Rate ist also niedrig. Bei Voll-Assoziativen Caches
waren nun alle Zeilen fuer eine Adresse moeglich. Das macht die Fuellung des
Caches einfacher und erhoeht die Hit-Rate. Gleichzeitig dauert das komplette
Durchsuchen, auch wenn es parallel ist, vergleichsweise laenger und der Aufbau
ist wegen der hohen Anzahl benoetigter Komparatoren komplexer und teurer.

Set-Associative Caches (Mengen-Assoziative Caches) gehen nun eben genau den
Mittelweg zwischen Direct-Mapped und Fully-Associative. Der Cache wird nun in
verschiedene, gleichgrosse und nichtleere Mengen von Zeilen
*partitioniert*. Eine Speicheradresse ist aehnlich wie beim Direct-Mapped Cache
genau einer eindeutigen Menge zugeordnet. Wie beim Voll-Assoziativen Cache muss,
sobald die Menge fuer eine Adresse bestimmt wurde, danach der Tag der Adresse
mit dem Tag jeder der in der Menge befindlichen Adressen verglichen werden. Eine
Speicheradresse wird daher wieder dreigeteilt:

1. Set-Index: Zu welcher Menge ist die Speicheradresse (bzw. der Datenblock)
   zugeordnet?

2. Tag: Der endgueltige Identifikator fuer einen Datenblock.

3. Offset: Der Offset des gewuenschten Datums innerhalb der Cache-Zeile bzw. dem
   darin enthaltenen Datenblock.

Das Wichtige ist aber nun, dass man nicht mehr $N$ Komparatoren fuer $N$ Cache
Zeilen braucht, um parallel den Tag einer Adresse mit *allen* $N$ Adressen zu
vergleichen. Man braucht nun fuer *den gesamten Cache* nur mehr so viele
Komparatoren, wie es in jeder Menge Zeilen gibt.

Hat der Cache also $1024$ Zeilen, brauchte man beim Voll-Assoziativen Cache
$1024$ Komparator-Schaltkreise. Beim Mengen-Assoziativen Cache wuerde man diese
$1024$ Zeilen nun beispielsweise in $64$ Mengen von jeweils $16$ Zeilen
aufteilen. Zuerst wird dann der Set-Index benutzt, um sehr schnell die Menge zu
finden, in der der Datenblock sein *muss*, wenn er im Cache ist. Dann hat der
Cache noch $16$ Komparatoren, die gleichzeitig den Tag-Teil der Speicheradresse
mit den Tags der einzelnen Zeilen vergliecht. Meldet einer einen Hit, liegt der
Datenblock also im Cache. Es sei hier nochmal angemerkt dass es fuer den ganzen
Cache nur diese $16$ Komparatoren braucht, nicht fuer jede Menge $16$. Sonst
haette diese Variante ja keinen Vorteil gegenueber dem Voll-Assoziativen
Cache. Ein Mengen-Assoziativer Cache ist also weniger komplex als ein
Voll-Assozitiver bezueglich der Schaltlogik.

Fazit: Set-Assoziative Caches sind ein guter Mittelweg zwischen schnellem
Mapping aber niedriger Kapazitaetsausnutzung auf der einen Seite, und
hoher Kapazitaetsausnutzung verbudnen mit hoher Komplexizitaet auf der anderen
Seite. Vor Allem ist es moeglich, in vielen, vielen Simulationen alle moeglichen
Verhalten von Speicherzugriffen zu beobachten und solche Caches perfekt auf den
Durchschnittsfall oder manchmal auf einen bestimmten Fall zu tunen. Man kann
also die optimale Set-Groesse bestimmen.

Hier noch kurz angemerkt dass ein Set-Assoziativer Cache wirklich das
allgemeinste Konzept ist. Ein Direct-Mapped Cache mit $N$ Zeilen ist ein
Set-Assoziativer Cache mit $N$ Mengen. Ein Voll-Assoziativer Cache mit $N$
Zeilen ist ein Set-Assoziativ Cache mit nur einer Menge.

https://www.youtube.com/watch?v=mCF5XNn_xfA

##### Stream Buffer

Zusaetzlich zu den Caches gibt es meist noch einen sogenannten *Stream
Buffer*. Bei einem Cache-Miss wird hierbei im Sinne des Pre-Fetching (siehe
unten) nicht nur der gewuenschte Datenblock geladen, sondern auch im Speicher
sequentiell darauffolgende. Diese darrauffolgenden werden aber nicht im Cache
abgelegt, um diesen nicht zu verschmutzen.

Wenn aber ein Datenblock im Cache nicht gefunden wird, wird im Stream Buffer
noch nachgeguckt. Ist der Datenblock dort gefunden (Stream-Hit), kann er sehr
schnell, in einem Taktzyklus, in den Cache geladen werden. Dann wird auch schon
die naechste Instruktion parallel pre-gefetched um den Stream-Buffer zu fuellen.

Geschieht aber ein Cache-Miss und auch ein Stream-Miss, wird der Stream
geflushed und mit den nachffolgenden Datenbloecken des gerade nicht im Cache
gefundenen Datenblocks neu gefuellt.

http://web.cecs.pdx.edu/~alaa/courses/ece587/spring2012/notes/prefetching.pdf

#### Statusbits

Jede Cache Line hat zwei Statusbits:

1. Dirty Bit (manchmal Modify Bit): Ist das Datum in der Cache Line *dirty*,
   bedeutet das, dass es in der Cache Line modifiziert wurde, ohne es auch im
   Hauptspeicher zu aktualisieren.

2. Valid Bit: Der Valid Bit besagt, ob die Cache Line Daten enthaelt, oder
   nicht. Es koennen beim Start-Up des Rechners z.B. irgendwelche Garbage Werte
   dort enthalten. Dann sind also alle Cache Lines *invalid* und erst durch
   beschreiben mit Daten aus dem Hauptspeicher koennen sie *valid* gemacht
   werden. Durch die informationslosen Bytefolgen kann ja sonst nicht eruiert
   werden, ob nun wirklich Daten enthalten sind, oder nicht.

### Cache Misses

Three C's:

* Compulsory (cold) misses:
  * Erster Zugriff auf eine Adresse.
  * Wuerden auch in unendlich grossen Caches passieren.

* Capacity Misses:
  * Capacity Misses erfolgen, wenn die Daten, auf welche man zugreift, groesser
    sind als der Cache.

* Conflict Misses
  * Conflict Misses passieren bei Cache Organisationen, die direkt abbilden oder
    mengen-assoziativ sind. Hierbei hat man einen Miss, weil gerade ein anderer
    Datenblock, der auf die selbe Zeile mapped, gerade dort vorhanden ist. Sowas
    kann natuerlich nur bei Set-Assoziativen und Direct-Mapped Caches der Fall
    sein.

http://stackoverflow.com/questions/33314115/whats-the-difference-between-conflict-miss-and-capacity-miss

* Hotel Analogien:

	* Compulsory Miss: Das Hotel ist leer und der erste Gast ist noch nicht
      angekommen.
	* Capacity Miss: Im Hotel ist kein Platz mehr.
	* Conflict Miss: Zwei Gaeste haben beide das selbe Stammzimmer. Der erste
      Gast besetzt das Zimmer, jetzt hat der zweite keinen Platz mehr.

https://courses.cs.washington.edu/courses/cse378/02sp/sections/section9-2.html

## Prefetching

Prefetching ist die Idee, Daten, die momentan noch nicht vom Programm benutzt
werden, dennoch schon vorher in den Cache zu laden. Auf Grund der raeumlichen
und zeitlichen Lokalitaet ist die Wahrscheinlichkeit hoch, dass diese
Entscheidung guenstig sein kann. Durch Prefetching kann die Hit-Rate verbessert
bzw. die Miss-Rate reduziert werden.

Prefetching kann auf zwei Ebenen geschehen:

### Hardware Prefetching

Hier wird das Prefetching von der Speicherhardware durchgefuehrt. Bei einem
Miss wird nicht nur der nicht gefundene Speicherblock geladen, sondern auch
der naechste. Der gesuchte Block kommt dabei in den Cache, nachfolgende
Bloecke in den Stream Buffer.

### Software Prefetching

Im Falle des Software-Prefetching gibt es in der ISA Instruktionen, die
manuelles Prefetching zu erlauben. Der Compiler eines Programms kann dabei
Prefetching Instruktionen einfuegen, um somit auch unregelmaesse,
nicht-sequentielle Zugriffe verbessern zu koennen.

## Speicherarten

### Halbleiterspeicher

Es gibt zwei Arten von Halbleiterspeichern, die sich in ihren Kosten und
Geschwindigkeit unterscheiden:

* SRAM
* DRAM

#### SRAM

Static-Random-Access-Memory (SRAM) ist die Variante von Halbleiterspeicher, die
zwar sehr *schnell* (10-30 nsec Zugriffszeiten) sind, aber dementsprechend *viel
kosten*. Sie sind aufwendiger herzustellen, werden also hauptsaechlich in
kleinen Speichern, wie __Caches__ verwendet. SRAM Bausteine sind durch
*Flip-Flops* (MOSFET) realisiert.

#### DRAM

Dynamic-Random-Access-Memory (DRAM) werden nicht durch Flip-Flops, sondern
mittels *Kondensatoren* hergestellt. Kondensatoren koennen viel weniger Energie
speichern als andere Halbleiterspeicher, koennen diese Energie aber auch sehr
viel schneller abgeben. Das Hauptproblem jedoch ist, dass Kondensatoren
*Leckstroeme* haben. Das bedeutet, dass Energie kontinuierlich verloren geht und
man diese Kondensatoren periodisch (ca. alle 2ms) auffrischen muss. Auch ist das
Lesen des Bits einer DRAM-Zelle *zerstoerend*, also muss die Zelle auch gleich
nachgefrischt werden. Dadurch ist die Zykluszeit, also die Zeit zwischen
Zugriffen, laenger als die Zugriffszeit (weil noch aufgefrischt werden muss).

Kondensatoren sind im Vergleich zu Flip-Flops (SRAM) langsamer, aber auch
billiger. Sie sind gut fuer Hauptspeicher.

https://www.youtube.com/watch?v=LNci-13Bp1U

##### Aufbau

Ein DRAM-Baustein ist eine Matrix aus DRAM-Zellen. Eine jede Zelle wird also
durch eine Zeile und eine Spalte adressiert. Hierfuer wird die Adresse
aufgespalten in Zeilen- und Spaltenteil. Diese beiden Teile werden in je einem
Register gespeichert. Es gibt dann zwei Signale, das RAS (row-address-strobe)
Signal und CAS (column-address-strobe) Signal, welche die Spalten- und
Zeilenadressierung *aktivieren*. Also zuerst werden die Teile gespeichert, und
dann werden die RAS/CAS Signale gesandt, um den Speicherzugriff in der Spalte
des Spaltenregisters und der Zeile des Zeilenregisters durchzfuehren. Hierbei
wird __immer eine ganze Zeile gelesen__, und erst dann das Bit an der
selektierten Spalte rausgelesen. Es gibt dann ein Write-Enable (WE) Signal, das
dem Speicher sagt, ob nun geschrieben werden soll, oder nur gelesen.

Nachdem eine Zeile in das Zeilenregister geschrieben wurde und durch die RAS
aktiviert wurde, wird die ganze Zeile gelesen und in ein weiteres Register, den
*Leseverstaerker* geschrieben. Da die ganze Zeile nun verfuegbar ist, ist es
viel schneller, einzelne Bits der selben Zeile hintereinander zu
adressieren. Man muss nur die Spalten-adresse aendern. Braucht man hingegen eine
voellig neue Zeile, muss man wieder die ganze Zeile laden.

Cache-Zeilen sind somit also an die Laenge der Zeile angepasst.

##### DIMMs

In einem Computer werden DRAMs auf sogenannten DIMMs (dual inline memory-module)
aufgebaut. Ein solches Modul besteht aus $8$ Chips, wobei jeder Chip weiter
aufgeteilt ist. Die Speicherkapazitaet von DIMMs wird meist in GBit
angefuehrt. Der Faktor zwischen GBit und GByte (oder MBit/MByte, KBit/KByte) ist
jeweils $8$, also ein $8$ GBit sind ein Gbyte.

Eine DIMM-*Reihe* (einfach ein DIMM-Baustein) hat acht Dateleitungen zu den acht
SDRAM Chips. Jeder SDRAM Chip kann einen GBit speichern. Somit hat man insgesamt
acht GBit auf dem Speicher, was wiederum einem GByte entspricht.

Ein einzelner SDRAM-Chip speichert also einen Gbit, was $2^{30}/8 = 2^{27}$
MByte entspricht. Hierbei wird die Spalten/Zeilen-Matrix intern noch in Felder
aufgeteilt, also wird aus dem Adressteil noch die Feld-Adresse extrapoliert.

#### SDRAM

DRAM an sich ist asynchron. Daten werden also geliefert, sobad die
entsprechenden Signale ankommen. Bei der Synchronous-DRAM (SDRAM) werden nun
alle Signale mit einem festen Taktsignal kombiniert. Somit koennen Daten also
nur entsprechend einem Takt gelesen werden.

### PROM

PROM steht allgemein fuer *programmable-read-only-memory*. Es gibt hierbei noch
EPROM und EEPROM Varianten, die weiter unten erklaert sind. PROM selbst wird
durch *zerstoerbare Sicherungen* realisiert. Das bedeutet, man kann den Speicher
ein einziges Mal programmieren, wobei er nach der einmaligen *Einbrennung* der
Bits zum ROM (read-only-memory) wird. PROM unterscheidet sich zu ROM, dass bei
ROM die Daten schon bei der Manufaktur eingebaut werden. Bei PROM kann man sich
zumindest einmal einen solchen datenlosen PROM Chip kaufen, und ihn erst dann
programmieren.

http://www.batronix.com/shop/electronic/eprom-programming.html (!)
https://en.wikipedia.org/wiki/Programmable_read-only_memory

#### EPROM

EPROM steht fuer erasable-read-only-memory. Es ist wie PROM eine Variante eines
__non-volatile__ Memory Chip -- die Daten bleiben also nach ausschalten der
Stromquelle erhalten. Hierbei aber werden die Daten in Transistoren auf dem Chip
eingebrannt. Sie koennen dann wieder geloescht werden (*eraseable*-PROM), indem
sie einer sehr starken *UV-Quelle* ausgesetzt werden. Deswegen haben EPROM Chips
meist ein kleines Fenster, wo die UV-Quelle angesetzt werden kann. Nach dem
Loeschen koennen neue Daten eingebrannt werden. Es handelt sich also nicht wie
beim PROM um einen OTP (one-time-programmable) Speicher.

https://en.wikipedia.org/wiki/EPROM

#### EEPROM

EEPROM (electrically-erasable-read-only-memory) unterscheidet sich von EPROM
dadurch, dass der Loeschvorgang nicht durch eine UV-Quelle geschehen muss,
sondern auch elektrisch realisiert werden kann. Dieser Vorgang dauert zwar
laenger, aber mit einem geeigneten elektrischen Protokoll kann man den Chip nun
auch programmieren, ohne dafuer spezielle UV-Apparatur zu benoetigen.

### ROM

Letztlich gibt es noch die Variante des reinen ROM (read-only-memory). Bei
dieser primitiven Speichertechnik werden die Daten schon bei der Herstellung
eingebaut (durch elektrische Schaltungen). ROM eignet sich somit also fuer
Daten, die gewiss nicht veraendet werden muessen und schon bei der Herstellung
feststehen.

### FLASH Speicher

Flash-Speicher sind eine Erweiterung von EEPROM. EEPROM konnte nur
neu-geschrieben werden, wenn vorher alle Daten geloescht wurden. Flash-Speicher
sind aber in Seiten (pages) organisiert. Eine Seite besteht dabei aus $512$ oder
$2048$ Bytes. Es muss nun nicht zuerst der ganze Speicher geloescht werden,
sondern es kann auch seitenweise geloescht und neu beschrieben werden. Es kann
aber auch schneller gelesen werden als auf DRAM.

Ein grosser Nachteil von Flash ist die Lebenserwartung. Eine Flash-Zelle kann
ungefaehr $10.000$ bis $1.000.000$ mal geschrieben werden. Man braucht daher
Reservezellen. Flashes selbst werden durch Oxid realisiert, welche beim Loeschen
leiden.

Eine Strategie, um dieses Problem der Lebenserwartung zu umgehen ist
__Wear-Leveling__. Hierbei versucht man die Schreibzugriffe relativ
gleichmaessig ueber die Zellen zu verteilen. Dazu hat man einen weiteren Chip,
der die Daten dynamisch zu neuen Bloecken re-organisiert.

https://en.wikipedia.org/wiki/Flash_memory

## Ein-/Ausgabe

Man unterscheidet bei Ein-/Ausgabegeraeten zwischen *zeichenorientierten*
(character/stream oriented) und *blockorientierten* (block oriented) Geraeten.

https://bryansoliman.wordpress.com/2011/07/25/block-oriented-devices-vs-stream-oriented-devices/

### Zeichenorientierte Geraete

Zeichenorientierte (stream-oriented) E/A-Geraete liefern ihre Daten zeichen-,
also byte-weise. Diese Art von Eingabe ist typisch fuer Dialoggerate wie die
Tastatur, der Bildschirm, die Maus oder der Drucker. Fuer solche Gerate ist kein
Buffering notwendig, also sind Verarbeitungs- und Zugriffszeit *schneller*.

### Blockorientierte Geraete

Bei Block-orientierten Geraeten werden Daten in groesseren Einheiten,
typischerweise $256$ Byte oder einigen KByte, gelesen oder geschrieben. Diese
Form von E/A ist typisch fuer:

* Festplatten
* Floppy-Disks
* Optische Baender (CD/DVD-ROM)

### Anbindung

Die Anbindung von Ein-/Ausgabegeraten erfolgt in der Regel ueber spezialisierte
Hardware -- einem sogennanten (Device) *Controller*. Diese hat oft einen eigenen
Mikroprozessor und erhaelt Kommandos sowie Daten vom CPU. Der Controller
behandelt dann die Anbindung bzw. Steuerung des eigentlichen Geraetes ueber
dessen Interface. Er behandelt also alle Operationen und Protokolle, die
eigentlich nichts mit den Daten zu tun haben. Die Kommunikation zwischen
Controller und CPU passiert dabei ueber Interrupts.

Ein Device-Controller braucht dabei auch immer einen Device-Driver. Das ist also
die Driver Software, die man auf Windows installieren muss.

Der Zweck von Controllern ist die __Entlastung von CPUs sowie die Realisierung
von zeitkritischen Steueraufgaben__.

Ein Beispiel fuer einen E/A-Controller ist ein Ethernet-Controller. Dieser muss
die Seriell-/Parallel-Wandlung der Daten, die Sicherung der Uebertragung durch
Pruefbits, Kollisionsbehandlung sowie noch weitere Aufgaben behandeln. Diese
Aufgaben muesste sonst die CPU machen (Entlastung) und diese waere auch weiter
weg, koennte also zeitkritische Dekodierung nicht so leicht behandeln.

https://chortle.ccsu.edu/AssemblyTutorial/Chapter-04/ass04_3.html

#### Direct-Memory-Access (DMA)

Daten muessen vom Hauptspeicher an die (Controller der) E/A-Gerate uebertragen
werden. Hierbei muesste normalerweise der CPU die Daten fetchen und zu den
Controllern uebergeben. Die Idee eines DMA-Controllers ist es nun, eine eigene
Einheit neben der CPU zu haben, die diese Speicherzugriffe fuer die CPU
erledigt. Die CPU muss an den DMA-Controller also nur die Anfangsadresse und die
Laenge des Datenblocks weiterleiten, und diese kann dann parallel zur
Ausfuehrung der CPU diese Daten holen und an den Controller uebergeben. Somit
wird die CPU weiter entlastet.

Diese DMA-Faehigkeit kann dabei direkt in den Controller des E/A-Gerates
eingebaut sein, oder es gibt einen separaten Baustein, der diese Aufgabe
uebernimmt.

### Hintergrundspeicher

Ein *Hintergrund*speicher steht separat zum Hauptspeicher, und ist dazu gedacht,
groessere Datenmengen dauerhaft zu speichern, beispielsweise zur Archivierung
von Daten. Er ist sehr viel groesser als der Hauptspeicher, aber dementsprechend
auch sehr viel langsamer.

Hierzu wird meist eine der folgenden Methoden zur Realisierung angewandt:

* *magnetische* Aufzeichnung:
  + Festplatten (HDD)
  + Magnetbaender (alte VHS Kasetten)
  + Disketten (Floppy)

* *optische* Aufzeichnung:
  + CD-ROM/R/RW
  + DVD
  + Blu-Ray (verwendet blauen Laser; schneller)

#### Magnetische Aufzeichnung

Bei magnetischen Aufzeichnungsverfahren werden Daten auf magnetisierbaren
Metallen gespeichert. Ein Bit wird hierbei durch die Magnetisierungs*richtung*
eines bestimmten Teiles der Schicht gespeichert. Diese Sued/Nord bzw. Nord/Sued
Polung kann dann durch elektrische Induktion zum Lesen eines bestimmten Bits
verwendet werden.

Ein Problem ist die zeitliche Synchronisation der Bits. Eine lange Folge von
identischen Bits ist schwer auseinanderzuhalten, wenn die zeitliche
Synchronisation nicht perfekt ist (das Problem das Manchester-Encoding
loest). Man muss also speziell codieren (sodass unabhaengig von den Bits die
Magnetisierung haeufig genug wechselt) und auch Pruefbits speichern.

##### Plattenspeicher

Eine Art von Festplattenspeicher der konventionelle Plattenspeicher wie man ihn
aus dem Datenbankbereich kennt. Hierbei sind die Daten auf magnetisierten,
runden Spuren angeordnet. Ein Schreib-Lesekopf kann dann die Daten lesen,
waehren die Platte rotiert.

## Virtueller Speicher

Ein Betriebssystem muss die Mehrprozess- und Mehrbenutzerfaehigkeit eines
Rechners ermoeglichen. Es muss also zu einem Zeitpunkt mehrere Prozesse mehrere
Benutzer (quasi)-gleichzeitig abarbeiten. Weiters muss das Betriebssystem auch
Programme laden und fuer diese Speicher reservieren (Code und Daten). Auch ist
das Betriebssystem dafuer zustaendig, eine effiziente Nutzung des Speichers zu
gewaehrleisten. Ein Prozess braucht nicht immer den gesamten moglichen Speicher,
und oftmals brauchen alle Prozesse zusammen auch mehr Speicher, als im
Hauptspeicher verfuegbar ist. Deswegen muss das Betriebssystem nicht benoetigte
Teile des Adressraums auf dem Hintergrundspeicher auslagern, um so Platz zu
schaffen.

Das Prinzip der *virtuellen Adressierung* ist eine Methode, um all diese
Aufgaben zu ermoeglichen. Das Grundkonzept hierbei ist, dass Programme mit
*logischen* (also *virtuellen*) Daten-Adressen arbeiten, welche nicht unbedingt
den echten Adressen der Daten im Hauptspeicher entsprechen. Nach den Vorgaben
des Betriebssystems wird eine logische Adresse erst durch die
*Memory-Management-Unit* (MMU) bei jedem Speicherzugriff in die echte, physische
Adresse umgesetzt. Jeder Prozess in einem Betriebssystem besitzt also seinen
eigenen virtuellen Adressraum und wenn ein Prozess ein Datum adressiert,
uebersetzt die MMU diese Adresse in die wirkliche Adresse.

Eine weitere Aufgabe der MMU ist die Ueberprufung der Zugriffsrechte eines
Prozesses fuer ein Datum. hat ein Prozess nicht die Rechte, auf ein Datum
zuzugreifen, erzeugt die MMU eine *Ausnahme*.

### Umsetzungstechniken

Die Umsetzung der logischen in die physischen Adressen durch die MMU erfolgt
ueber bestimmte Tabellen. Diese Tabellen enthalten natuerlich nicht fuer jedes
Byte eine Ubersetzung. Sonst muesste man den halben Speicher nur fuer diese
Tabelle opfern. Man uebersetzt also nicht byte-weise, sondern in groesseren
Speicherbloecken. Mit einem *Speicherblock* kann hierbei entweder ein *Segment*
oder eine *Seite* gemeint sein (siehe unten). Es werden also nur die Block-Teile
(oberen Bits) einer jeden Adresse uebersetzt, waehrend der Offset in diesen
Block (auf das genaue Datum) gleich bleibt.

Wenn es nun eine solche Uebersetzungstabelle fuer Speicherbloecke gibt, kann das
Betriebssystem jedem Prozess einen kontinuierlichen (contiguous) Speicher zur
Verfuegung stellen, wobei ein Block aus diesem Adressraum an *beliebiger* Stelle
im Hauptspeicher stehen kann.

Auch kann das Betriebssysteme Speicherbloecke wenn noetig auf den
Hintergrundspeicher auslagern. Dann wird die Adressabbildung fuer diesen Block
in der Umsetzungstabelle geloescht. Will ein Prozess nun doch auf diese Seite
zugreifen, wird ein page-error erzeugt (Seitenfehler). Dann muss der
Speicherblock geladen werden, die Umsetzungstabelle aktualisiert werden und der
Prozess fortgefuehrt werden.

https://www.quora.com/What-is-the-difference-between-paging-and-segment-in-memory-management

#### Segementierung

Bei der Segmentierung wird er Adressraum eines jeden Prozesses in logisch
zusammengehoerige Bloecke, sogennante *Segemente*, gegliedert. Beispielsweise
kann es Code-, Daten- und Kellersegmente geben.

Es gibt dann also eine Tabelle, welche die virtuellen Segmente in die physischen
uebersetzt. Diese Tabelle enthaelt fuer jedes virtuelle Segment einen Eintrag,
der folgende Informationen beinhaltet:

* Die physische Anfangsadresse des Segments.
* Die Laenge des Segments.
* Die Zugriffsrechte.

Die Zugriffsrechte beinhalten hierbei das read/write-execute Byte sowie die
Privilegstufe, welche angibt, ob die Daten nur im Systemmodus, oder auch im
Benutzermodus der CPU adressiert werden koennen.

Will man nun auf ein Segment zugreifen, wird der Segmentindex (aus den oberen
Bits einer Adresse) gegen die Tabelle gemapped, und die Anfangsadresse
geholt. Diese wird dann auf den Offset dazuaddiert, um somit die physische
Adrese zu geben. Die Laenge des Segements ist dazu da, zu ueberpruefen, ob der
Offset nicht ausserhalb der Laenge liegt.

Die meisten moderenen Betriebssystem verwenden anstatt Segmentierung jedoch eher
Paging.

https://www.quora.com/Paging-versus-segmentation-why-are-they-two-separate-methods-and-why-would-I-chose-one-over-the-other

#### Paging

Neben der Segementierung gibt es noch die Moeglichkeit der *seitenbasierten*
Speicherverwaltung, dem sogenannten *Paging*. Hierbei wird der virtuelle
Adressraum in Bloecke gleicher Groesse aufgeteilt, wobei eine solcher Block dann
als Seite (page) bezeichnet wird. Eine typische Seitengroesse ist $4\, KiB$.

Eine Seite (page) im virtuellen Adressraum entspricht dann einer Kachel (page
frame) im physischen Adressraum (auf dem Hauptspeicher). Wieder gibt es hier
eine Umsetzungstabelle, welche fuer jede Seite die:

* physische Adresse der zugehoerigen Kachel (falls nicht ausgelagert)
* Zugriffsrechte

enthaelt.

Wenn paging als Virtualisierungstechnik genutzt wird, gibt es noch ein
spezielles Register in der CPU, welche die Adresse des page-table (der
Umsetzungstabelle) fuer den momentanen Prozess enthaelt. Dieser page-table liegt
im Hauptspeicher liegenden *page-directory*, neben den page-tables fuer alle
anderen Prozesse. Wird ein Prozess gewechselt, wird die Adresse des
page-table fuer diesen Prozess in das spezielle Register geladen.

##### Translation-Lookaside-Buffer (TLB)

Um kontinuierlich viele Zugriffe auf das im Hauptspeicher liegende
page-table (die Umsetzungstabelle zwischen Seiten/pages und Kacheln/page
frames) zu vermeiden, gibt es einen speziellen Cache, den sogenannten
Translation-Lookaside-Buffer (TLB). Dieser wird fuer jeden Prozess am Anfang
geflushed. Dann, wenn eine Seite einmal im page-table gesucht wird, wird die
virtuelle Seitenadresse als Tag fuer den Cache verwendet, und die dazugehoerige
physische Kacheladresse dort gespeichert. Somit kann man beim naechsten mal die
Kacheladresse schon gleich durch diesen Cache gewinnen, ohne vorher noch
zusaetzlich zum Hauptspeicher gehen zu muessen.

Translation-Lookaside-Buffers sind meist voll- oder mengenassoziativ und halten
typischerweise $32$ bis $512$ Eintraege. Auch gibt es oft getrennte TLBs fuer
Daten und Code.

https://www.quora.com/What-is-the-difference-between-paging-and-segment-in-memory-management

##### Seitengroesse

Die Seitengroesse wird meist mit $4\, KiB$ gewaehlt, manchmal aber auch
groesser. Wann ist was besser?

* Gruende fuer groessere Seiten:
  + Kleinere Seitentabellen.
  + Ein-Auslagerung von groesseren Seiten ist effizienter.
  + TLB wird oefter und effizienter genutzt (mehr Daten pro Seite = mehr Hits
    fuer eine Seite im Cache).

* Gruende fuer kleinere Seiten:
  + Weniger Fragmentierung (effizientere/flexiblere Speichernutzung)
  + Aufwand zum Star von kleinen Prozessen ist niedrieger (brauchen nicht so
    einen grossen Addressraum)
