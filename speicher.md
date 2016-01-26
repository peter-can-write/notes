# Speicher

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
