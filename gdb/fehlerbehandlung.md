# Fehlerbehandlung

Die Eigenschaften *Atomicity* und *Durability* postulieren, dass eine Datenbank
eine abgeschlossene Transaktion fuer immer, bis in alle Ewigkeit, gespeichert
halten muessen; ebenso muss eine nicht abgeschlossene Transaktion nie existiert
haben, falls sie abgebrochen wird.

Natuerlich passieren in einem Datenbanksystem auch Fehler. Mit Fehlern sind hier
Systemausfaelle wie Feuer- oder Erdbeben gemeint, aber auch andere Fehler die
z.B. waehrend der Synchronisation mehrerer Benutzer auftauchen koennen.

Waehrend der Diskussion der Fehlerbehandlung interessieren uns vorallem zwei
Phasen, die durch die Ausdruecke `undo` und `redo` definiert sind. Wenn eine
Transaktion abgebrochen wird, muessen bisher geschehene Operationen rueckgaengig
gemacht werden, also `undo`-ed. Wenn aber eine abgeschlossene Transaktion schon
gespeichert ist, und das Speichermedium, in welchem sich die modifizierten
Seiten der abgeschlossenen Transakion befinden durch einen Fehler ausfaellt,
dann wollen wir diese Transaktion nach dem Absturz *wiederherstellen*, also
`redo`-en

### Fehlerklassen

Allgemein unterscheiden wir drei Fehlerklassen:

1. *Lokale* Fehler: Solche Fehler, die passieren, wenn noch nicht vollstaendig
   abgeschlossene Transaktionen gerade in Bearbeitung sind. Hierfuer benoetigen
   wier unter Umstaenden `undo`-Mechanismen. Solche lokalen Fehler treten
   meistens in Anwendungsprogrammen auf, oder wenn der Benutzer eine Transaktion
   `abort`ed. Dieser `abort` kann wie oben beschrieben auch vom System selbst
   initiiert werden. Das wichtige hierbei ist, dass die restlichen Transaktion
   sowie die Datenbasis selbst nicht beeinflusst sind. Wir nennen eine
   Wiederherstellung eines Systems nach einem solchen Fehler auch *R1-Recovery*.

2. Fehler mit *Hauptspeicherverlust*: Bei einem solchen Fehler verlieren wir
   also alle Daten, die im Hauptspeicherpuffer gespeichert waren. Je nach
   Strategie des DBMS koennen zu einem Zeitpunkt sowohl Seiten abgeschlossener
   Transaktionen noch im Hauptspeicher gelegen sein (*R2-Recovery*), sowie auch
   Seiten noch unvollstaendiger Transaktionen schon im Hintergrundspeicher sein
   (*R3-Recovery*). Wir brauchen also oft sowohl `redo` als auch `undo`
   Mechanismen.

3. Fehler mit *Hintergrundspeicherverlust*. Eine Systemkorrektur nach einem
   Hintergrundspeicherverlust nennt man *R4-Recovery*. In einem solchen Fall
   muss man also auf den Archivspeicher vertrauen.

### Speicherverwaltung

Ein DBMS muss zur Verwaltung von Transaktionen auch gewisse Strategien zur
Verwaltung der Speicherhierarchie -- Hauptspeicher, Hintergrundspeicher,
Archivspeicher -- realisieren.

#### Pufferverwaltung

So muss man sicher eine Strategie dafuer festlegen, wann man modifizierte Seiten
aus dem Puffer wieder in den Hintergrundspeicher schreibt. Hierbei unterscheiden
wir dann zwischen Puffer-Seiten von abgeschlossenen und nicht abgeschlosessenen
Transaktionen.

Fuer nicht abgeschlossene Transaktionen kennt man die *steal*
($\text{steal}$) und *no-steal* ($\neg \text{steal}$) Varianten:

* $\neg \text{ steal}$: Daten einer Seite werden erst beim Abschluss einer
  Transaktion, und nie mittendrin, vom Hauptspeicher in den Hintergrundspeicher
  geschrieben. Daten von nicht-committeten Transaktionen sind also nie im
  Hauptspeicher.

* $\text{steal}$: Jede Seite ist prinzipiell ein Kandidat fuer
  die Ersetzung, falls neue Seiten eingelagert werden muessen. Also Seiten mit
  veraenderten Daten koennen schon ausgelagert werden, bevor die Transaktion
  commitet hat.

Fuer abgeschlossene Transaktionen unterscheidet man zwischen der *force*
($\text{force}$) und *no-force* ($\neg \text{force}$) Strategie:


* $\text{force$} Strategie: Nach commit einer Transaktion muessen saemtliche
  durch die Transaktion modifizierten Seiten permanent vom Hauptspeicherpuffer
  auf den Hintergrundspeicher geschrieben werden.

* $\neg \text{ force}$: geanderte Seiten von abgeschlossenen Transaktionen
  koennen noch im Hauptspeicherpuffer bleiben.

Hierbei benoetigt man fuer verschiedene Strategien verschieden Mechanismen zur
Fehlerbehandlung. Man betrachte z.B. die Variante *no-steal* und
*force*. Hierbei wuerden modifizerte Seiten noch aktiver Transaktionen nie
permanent gespeichert werden und Seiten abgeschlossener Seiten nach
Transaktionsende sofort permanent auf den Hintergrundspeicher geschrieben. Das
bedeutet, wir braeuchten schon mal keine `undo` Information um Aenderungen
abgebrochener Transaktionen rueckgaengig zu machen, da diese nie permanent
gespeichert waeren (mit *permanent* meinen wir, auf dem Hintergrundspeicher
gespeichert, da der Primaerspeicher meist volatil ist (RAM), der
Sekundaerspeicher (Festplatte) sicher nicht). Auch braeuchten wir nie `redo`
Information, da abgeschlossene Transaktionen auch sicher immer im
Hintergrundspeicher verewigt waeren.

Wuerden wir die *steal* Variante erlauben, wuerden wir es dem DBMS also
ermoeglichen, modifizierte Seiten noch nicht abgeschlossener Seiten auch schon
auf den Hintergrundspeicher -- permanent -- auszulagern. Das bedeutet, dass wenn
die Transaktion abgebrochen wird, wir diese Aenderungen auch rueckagengig machen
muessten, also `undo` Informationen benoetigen wuerden.

Fuer die *no-force* Variante braeuchten wir `redo` Information. Denn bei
*no-force* erlauben wir es dem DBMS, Seiten abgeschlossener Transaktionen noch
weiter im Puffer zu lassen, ohne sie noch permanent auszulagern. Wenn nun also
der Hauptpspeicher einen Fehler hat, dann muss diese abgeschlossene Transaktion
zum Zwecke der Durability aber wiederhergestellt werden. Daher brauchen wir also
Mechanismen, um dies zu tun: `redo`-Information.

Diese Kombination no-steal und force scheint also ideal. Wir brauchen weder
`redo` noch `undo` Informationen. Leider ist sie in echt nicht moeglich. Zum
einen muss es manchmal einfach sein, dass im Puffer kein Platz mehr ist und
modifizerte Seiten nicht abgeschlossener TAs (Transaktionen) eben auseglagert
werden muessen. Also ist `no-steal` zu restriktiv. Auch ist ein voller "Flush"
modifizerter Seiten abgeschlossener TAs eine sehr aufwendige Operation, die man
eigentlich vermeiden moechte. Also speichert man lieber `redo`
Information.

Letztlich gibt es aber noch ein sehr aussagekraeftiges und "endgueltiges"
Argument gegen $\neg \text{steal} + \text{force}$:
Mehrbenutzersynchronisation. Nehmen wir das Beispiel an, dass zwei Transaktionen
Daten auf der selben Seite bearbeiten. Dann commitet die eine Transaktion. Wegen
*force* muessten wir diese Seite nun also sofort permanent auslagern. Wegen
*no-steal* duerfen wir das aber nicht, weil die andere Transaktion noch nicht
abgeschlossen ist. Wir haben also einen Widerspruch.

### Einlagerungsstrategie

Wenn es nun dazu kommt, dass eine Seite vom Puffer in den Hauptspeicher
eingelagert werden muss, muessen wir auch hier noch diskutieren, wie man das am
besten macht. Wir kennen hier folgende Strategien:

* *Update-in-Place*: Bei dieser Strategie wird jeder Seite genau ein Rahmen im
  Hintergrundspeicher zugeordnet. Wird eine Seite also vom Puffer ausgelagert,
  wird sie stets genau an diese Stelle im Hintergrundspeicher geschrieben. Das
  hat natuerlich zur Folge, dass der Zustand der Seite vor Einlagerung verloren
  geht.

* *Twin-Block*: Fuer diese Strategie reservieren wir im Hintergrundspeicher fuer
  jede Seite *zwei* Rahmen. Der erste Rahmen ist immer der aktuelle, der andere
  der vorherige Zustand der Seite. Fuer jede Seite $P_i$ gibt es also im
  Hintergrundspeicher zwei Ramen $P_i^{\text{aktuell}}$ und
  $P_i^{\not\text{aktuell}}$. Wenn eine Seite dann vom Puffer ausgelagert wird,
  wird sie immer in den nicht aktuellen Rahmen geschrieben. Dann werden
  sozusagen die Negationn vor dem "$\text{aktuell}$" umgedreht. Das ganze hat
  dann den Vorteil, dass man *Before* und *After* Images immer noch hat.

* *Schattenspeicherkonzept*: Nur *geanderte* Seiten werden dupliziert. Das
   verursacht weniger Redundanz als beim Twin-Block Verfahren. Diese Strategie
   hat sich aber nicht durchgesetzt, weil die Daten beim Schattenspeicherkonzept
   nicht einheitlich auf dem Speicher gespeichert werden, was Random I/O
   verursacht.

## Logging

Die wichtigste Methode zur Behandlung von Fehlern ist *Protokollierung*
bzw. *Logging*. Logging bezieht sich einfach auf die Idee, alle Operationen
eines DBMS zu protokollieren und festzuschreiben. Das hat den Vorteil, dass man
nach einem Fehler diese Operationen erneut durchfuehren kann, fuer `redo`, sowie
auch rueckgaengig machen kann, fuer `undo`.

### Struktur von Logeintraegen

Logeintrage verfolgen allgemein eine gewisse Struktur, die alle notwendigen
Informationen beinhalten. Diese Informationen sind:

1. *LSN*: Eine *Log Sequence Number*, die einfach eine eindeutige Kennung (ID)
   fuer diesen Log-Eintrag ist. Diese Zahl muss natuerlich streng monoton
   wachsend sein, um die chronologische Folge der Operationen zu bewahren.

2. *Transaktionskennung* (TA): Eine Kennung fuer die Transaktion, um die
   Operationen vieler Transaktionen von einander zu unterscheiden.

3. *PageID*: Eine Kennung fuer die (Speicher-)Seite, auf welcher die
   Operation durchgefuehrt wurde.

4. *PrevLSN*: Die Kennung der vorhergehenden LSN dieser Transaktion. Da viele
   Transaktionen gleichzeitig aktiv sein koennen, werden die Log-Eintraege einer
   Transaktion nicht immer sequentiell gespeichert. D.h., es kann sein, dass
   Transaktion $A$ zuerst eine Operation mit LSN 1 durchfuehrt, dann Transaktion
   $B$ eine Operation mit LSN 2, und dann wieder $A$ mit LSN 3. Wenn man dann
   ruckwaerts schnell durchlaufen moechte, um, die Operationen von Transaktion
   $A$ ruckgaengig zu machen, dann kann man von LSN 3 direkt zu LSN 1 springen,
   und muss die ganze Log-Datei nicht sequentiell durchforsten.

5. *Undo*-Information: Hier werden Informationen gespeichert, die das DBMS
   benutzen kann, um die Operation, die durch diesen Log-Eintrag protokolliert
   wurde, rueckgaengig zu machen. Das ist z.B. notwendig, wenn ein Benutzer
   manuelle ein *abort* anfordert.

6. *Redo*-Information: Analog zur Undo-Information wird hier festgelegt, wie man
   die Operation wiederholen kann, wenn man z.B. nach einem Systemfehler eine
   abgeschlossene aber noch nicht permanent gespeicherte Transaktion
   wiederherstellen muss. Diese Information ist also exakt die Operation die
   durchgefuehrt wurde.

### Logische vs. Physische Protokillierung

Betrachten wir die Operation $a += 5$, wo $a$ ein Datum auf Seite $P_1$, von
Transaktion $T_1$ sei. Sagen wir es wurden vorher $69$ Operationen nur von $T_1$
durchgefuehrt, also soviele Log-Eintraege generiert. Dann wuerde der zur
Operation $a += 5$ passende neue Log-Eintrag so aussehen:

|  LSN |  TA | PageID |   Undo   |   Redo   | PrevLSN |
|:-----|:----|:-------|:---------|:---------|:--------|
| $70$ |$T_1$| $P_1$  | $a -= 5$ | $a += 5$ |   $69$  |

Hier sieht man also die Struktur eines Log-Eintrags und auch, dass die
Redo-Information eben genau die Operation beschreibt, und die Undo-Information
genau die inverse Operation. Ist also $f$ die Funktion der Operation, so
speichert man als Undo-Information $f^{-1}$. Aber betrachten wir nun als
Beispiel die Operation $f: x \mapsto x^2$. Dann haben wir hier ein Problem, denn
diese Funktion ist nicht injektiv und daher ist das Urbild auch nicht eindeutig,
da sowohl $x$ also auch $-x$ in Frage kaemen: $f^{-1}: x \mapsto
\pm\sqrt{x}$. Auch bedenke man allgemein, dass nicht jede Funktion invertierbar
ist und auch, dass manche Funktionen sehr, sehr komplex sind und ebenso sehr,
sehr komplexe Umkehrfunktionen benoetigen wuerden.

Daher unterscheiden wir zwischen *logischer* und *physischer*
Protokollierung. Die *logische* Protokollierung haben wir bisher betrachtet, wo
Aenderungen als Funktionen angegeben werden. Bei *physischer* Protokollierung
speichern wir also Redo- und Undo-Information nicht die Funktion selbst, sondern
nur den Zustand des Datums vor und nach der Operation. Den physischen Zustand
des Datums vor der Operation nennen wir dann das *Before*-Image, und den Zustand
nach der Operation das *After*-Image.

Die *Undo*-Operation wurde also die Datenbasis einfach auf das Before-Image
zuruecksetzen. Ein *Redo* wuerde die Datenbasis vom Before-Image auf das
After-Image wiederherstellen.

Wenn es nun zur Fehlerbehandlung kommt, muessen wir fuer eine konkrete Seite im
Hintergrundspeicher natuerlich wissen, welcher Zustand sich hier gerade
gespeichert findet. Daher speichern wir auf jeder Seite in einem reservierten
Adressraum die LSN des letzten Log-Eintrags, der die Seite modifizert hat. Also
stellt eine modifizierte Seite genau das After-Image des Log-Eintrags dar,
dessen LSN gespeichert ist. Wenn wir nun also im Laufe der Fehlerbehandlung eine
Seite mit der LSN eines Log-Eintrags vergleichen, dann koennen wir sagen:

* Wenn der LSN des Log-Eintrags kleiner ist als der, der auf der Seite
  gespeichert ist, dann sind in der Seite schon neuere Daten gespeichert (also
  das After-Image eines Log-Eintrags mit hoeherer LSN).

* Wenn der LSN des Log-Eintrags groesser oder ist, als der gespeicherte, dann
  befindet sich auf der Seite noch das Before-Image diese Log-Eintrags, oder
  eines mit niedrigerer LSN.

### Speichern der Log-Information

Wir moechten nun diskutieren, wie die Log-Eintraege gespeichert
werden. Grundsaetzlich sei hierfuer gesagt, dass die Behandlung von Logs einen
eigenen Teil der Speicherhierarchie bzw. -verwaltung darstellen. Eine besonders
guenstige Eigenschaft von Logs ist, dass sie streng monoton in ihrer LSN
fortschreiten und daher sequentiell gespeichert werden koennen. Das ist von
enormen Vorteil, da Plattenspeicher sehr, sehr gut fuer "Sequential I/O"
geeignet sind. Der Schreib-/Lesekopf muss dafuer nur ab und an die Spur
wechseln, kann sonst aber durchgehend die Platte beschreiben. Es gibt also zwar
*Seek-Time* (Positionierung des Kopfes) aber keinerlei *Latenzzeit* (Rotieren
der Platte auf die Startadresse des Zielraums). Daher moechtn wir auch fuer Logs
und Daten separate Platten benutzen, da Platten fuer Daten doch auch Random I/O
machen muessen.

Neben dem Datenbankpuffer gibt es im Hauptspeicher dann auch noch einen
Log-Puffer. Der Log-Puffer ist dann sowohl mit einer Log-Datei als auch mit
einem Log-Archiv verbunden. Auf dem Log-Archiv werden alle jemals generierten
Log-Eintraege, sowie auch die neuesten, gespeichert. Auf der Log-Datei werden
parallel nur die neuesten Log-Eintraege gespeichert. Das hat den Vorteil, dass
man aus der Log-Datei schneller die neueren Eintraege lesen kann, um z.B. lokale
Fehler schnell zu beheben. Gleichzeitig kann man sich bei schwereren Fehlern
auch vollstaendig auf das Archiv (Magnetbaender) verlassen, da sie auch die
neueren Eintrage speichern muessen. Das lesen aus dem Log-Archiv ist aber
natuerlich langsamer, als aus der Log-Datei.

Man muss hierbei beachten, dass viele DBMS ein Feature haben, dass sich
*Wrap-Around* nennt. Wenn die Log-Datei voll ist, werden hierbei einfach die
aeltesten Eintrage quasi zirkulaer ueberschrieben. Diese Feature sollte man
ausschalten. Wenn die Log-Datei dann voll ist, muss man einfach warten, bis die
Datei auf das Archiv ausgelagert ist.

Der Log-Buffer im Hauptspeicher selbst ist dabei zirkulaer. Das bedeutet, dass
man sozusagen zwei sich bewegende Zeiger hat, die um das Ende des Buffers
wrappen. Beim einen Zeiger werden neue Eintraege geschrieben und beim anderen
werden sie an Archiv und Datei ausgelesen.

### WAL Prinzip

Das Write-Ahead-Log (WAL) Prinzip ist einfach die Bedingung, dass Log-Eintraege
immer zuerst geschrieben werden muessen, bevor eine bestimmte Aktion
durchgefuehrt wird. Die "Aktion" kann hierbei ein Commit einer Transaktion oder
das Auslagern einer modifizerten Seite aus dem Hauptspeicherpuffer in den
Hintergrundspeicher sein.

Fuer einen Commit bedeutet dass, das auch wirklich erst der endgueltige
Abschluss der Transaktion folgen kann, wenn die Log-Eintraege ausgeschrieben
wurden (in die Log-Datei und das Log-Archiv). Wenn man den `commit`-Befehl
auessert, aber dann noch vor dem erfolgreichen Schreiben aller Log-Eintraege ein
Systemabsturz erfolgt, dann ist das so, also ob der Systemabsturz vor dem Commit
passiert war. Also die Transaktion waere noch immer ein Loser. Ansonsten koennte
man die Transaktion nicht wiederherstellen, wenn sie vorher schon als
abgeschlossen galt. Fuer den Log-Writer, also der Verwaltng des Log-Schreibens,
bedeutet dass, das der zuletzt geschrieben LSN groesser gleich dem LSN der
letzten Operation der Transaktion sein muss.

Fuer eine Seitenauslagerung bedeutet das, dass man zuerst wissen muss, wie man
die zu speichernden Aenderungen rueckgangig machen kann (`undo`), bevor man sie
wirklich permanent schreibt.

### Wiederherstellung nach einem Fehler

Wir haben nun die ganze Infrastruktur rund um die Fehlerbehandlung aufuehrlich
behandelt. Was passiert aber genau, wenn nun wirklich ein Fehler passiert?

Hierfuer unterscheiden wir zunaechst zwei verschiedene Arten von Transaktionen,
die waehrend der Fehlerbehandlung veschieden behandelt werden:

1. *Winner*-Transaktionen sind solche, die zum Zeitpunkt des Fehlers schon
   abgeschlossen waren. Es hat also vor dem Absturz ein `commit` gegeben. Wir
   muessen dafuer sorgen, dass diese Transaktionen unbedingt wiederhergestellt
   werden.

2. *Loser*-Transaktionen sind dann solche, die zum Zeitpunkt des Fehlers noch
   nicht abgeschlossen waren. Nach dem Prinzip der Atomicity muessen wir unsere
   Datenbasis so veraendern, dass es die Transaktion scheinbar *nie* gegeben
   hat.

Wir gehen dann nach einem Fehler konkret so vor:

1. Analyse der Log-Eintraege: Wir bestimmen, welche Transaktionen zum Zeitpunkt
   des Fehlers schon abgeschlossen waren -- die Winner -- sowie welche es noch
   nicht waren -- die Loser.

2. Wiederholung der Historie: Wir nutzen zuerst die Redo-Information der
   Log-Eintraege aus, um die Datenbank so nahe wie moeglich an den Zeitpunkt des
   Fehlers zu bringen. Wir wiederholen also alle Operationen, fuer die es
   gespeicherte Log-Eintraege bis zum Zeitpunkt des Absturzes gibt.

3. Undo der Loser: Erst nachdem wir die Datenbasis wiederhergestellt haben,
   nutzen wir wieder die Log-Eintrage, um Loser-Transaktionen vollstaendig
   zurueckzusetzen.

#### Redo

Um nun die Datenbasis nach einem Fehler wiederherzustellen, nutzen wir die
Redo-Information der Log-Eintraege. Wir lesen also vom ersten LSN der aeltesten
vor dem Fehler noch laufenden Transaktion alle Log-Eintraege einfach sequentiell
durch. Fuer jeden Log-Eintrage lagern wir dann die protokollierte Seite in den
Hauptspeicher-Puffer.

Wir pruefen dann, ob der gespeicherte LSN auf der Seite (dessen After-Image die
Seite gerade darstellt) groesser oder gleich dem LSN des betrachteten
Log-Eintrags ist. Falls ja, dann haben ist diese protokollierte Operation also
schon in der Seite enthalten und wir koennen diesen Log-Eintrag
ueberspringen.

Falls nicht, dann muessen wir die protokollierte Redo-Operation
durchfuehren. Gleichzeitig aendern wir dann auch den gespeicherten LSN auf der
Seite zum LSN diesen Log-Eintrags. Das ist sehr wichtig. Wenn die Datenbank
naemlich waehrend der Wiederherstellung wieder abstuerzen wuerde, dann muesste
festgelegt sein, dass wir diese Redo-Operation schon durchgefuehrt haben. Das
ist fuer die *Idempotenz* der Wiederherstellung essentiell.

#### Undo


CLR = Compensation Log Record

Ein CLR wird bei einem Undo eingefuegt. Es enthaelt nur die Redo information
(die Aktion, das vorher das gesamte undo war), keine undo Information. Die
Redo-Information ist dann so, als ob es eine ganz normale Operation waere.

0. + 25
1. + 50
2. + 10
3. Undo 2 = -10

Dann beim Redo: + 25, + 50, +10, -10

Fail-Over Zeit: Zeit, um System wiederherzustellen. Paar Minuten heute.

Sicherungspunkt: HS-Puffer -> Platte

Master-Slave Variante: Hier gibt es zwei Plattenspeicher. Der LogWriter schreibt
Daten auf den einen Plattenspeicher (wie normal), aber die Log-Entries werden
*gesniffed* (*log-sniffing*), wobei die redo-Information dazu verwendet wird,
die Aktionen auf einem zweiten Plattenspeicher nachzubilden.
