# OSI Modell: Layer 3

Auf Schicht 3, der *Vermittlungsschicht* bzw. dem *Network Layer*, beschaeftigen
wir uns nun schon mehr mit se Interwebs. Wir betrachten zunaechst
Vermittlungsarten, dann Adressierung im Internet und letztlich Routing.

## Vermittlungsarten

Es gibt drei verschiedene Moeglichkeiten, Daten von $A$ nach $B$ zu senden:

1. Leitungsvermittlung: Reserviere einen exklusiven Kanal fuer Vermittlung von
   $A$ nach $B$.
2. Nachrichtenvermittlung: Sende Nachrichten individuell.
3. Paketvermittlung: Teile eine Nachricht in kleinere *Pakete* auf und sende
   diese individuell, unabhaengig von den anderen.

Wir werden diese drei Arten nun naeher beleuchten. Dabei beschreiben wir die
Verfahren an Hand des folgenden Beispielpfades:

```
(s) --- (i) --- ... --- (j) --- (t)
```

Wobei $s, t$ Quell- bzw. Zielknoten sind und $i, ..., j$ Vermittlungsknoten $n$
darstellen. Dabei uebertragen wir immer eine Nachricht mit $L$ Bits ueber eine
Gesamtdauer ($s \rightarrow t$) von $T$. Die Knoten $s$ und $t$ liegen dabei $d$
Meter auseinander und alle Knoten sind aequidistant plaziert.

### Leitungsvermittlung

Bei der Leitungsvermittlung reservieren wir einen exklusiven Kanal von $s$ nach
$t$. Die Uebertragung besteht dabei aus drei Phasen:

1. Verbindungsaufbau: Hierbei tauschen $s$ und $t$ zunaechst
   Signalisierungsnachrichten aus, um die Verbindung zu initialisieren. In
   diesem Schritt wird auch schon die letztendliche Wegwahl von $s$ nach $t$
   genau ueber Knoten $i, ..., j$ getroffen.
2. Datenaustausch: Daten koennen nun ueber diesen exklusiven Kanal von $s$ nach
   $t$ und retour ausgetauscht werden. Das bedeutet, dass nur diese beiden
   Knoten diesen Kanal verwenden duerfen. Da der Kanal nur $s$ und $t$
   verbindet, kann die Adresseriung der Nachrichten waehrend der Uebertragung
   auch einfach ausfallen.
3. Verbindungsabbau: Signalisierungsnachrichten werden wieder auf beiden Seiten
   ausgetauscht, um nun das Ende der Uebertragung zu signalisieren. Danach
   duerfen also wieder neue Knoten $s'$ und $t'$ diesen Kanal (exklusiv) nutzen.

Wie hoch ist hier nun die Uebertragungszeit? Um sie zu bestimmen, nehmen wir
zunaechst an, dass:

1. Die Serialisierungszeit fuer *Signalisierungsnachrichten* vernachlaessigbar
   klein ist.
2. Die Wartezeiten fuer Nachrichten in jedem Vermittlungsknoten
   vernachlaessigbar klein sind.

Dann senden wir also zuerst eine Signalisierungsnachricht von $s$ nach $t$ sowie
retour, um die Verbindung zu starten (1). Dies kostet gerade $t_p = d/(\nu c)$
Zeit. Danach wird die eigentliche Nachricht versendet. Die serialisierten Daten
werden dabei von Knoten zu Knoten ohne Verzoegerung versendet. Also brauchen wir
nur die Serialisierungszeit der $L$ Bit bei einer Datenrate $r$ betrachten: $t_s
= L/r$. Der letzte Teil dieser Nachricht sei dabei die Signalisierung, dass die
Verbindung nun abgebrochen werden soll. Insgesamt kommt hier nun fuer die
serialisierte Nachricht und der Signalisierung noch einmal $t_p$ viel Zeit
hinzu. Letztlich "acknowledged" $t$ noch den Verbindungsabbruch mit einem
Signal, was wiederum $t_p$ Sekunden braucht, um von $t$ nach $s$ zu
kommen. Insgesamt haben wir also:

$$T_{LV} = 2t_p + t_s + 2t_p = t_s + 4t_p = \frac{L}{r} + 4\frac{d}{\nu c} .$$

Der einzige wirkliche Vorteil von Leitungsvermittlung ist, dass die Verbindung
nach Verbindungsaufbau *konstant schnell* ist. Nachrichten gehen naemlich immer
ueber den selben Weg gehen, weswegen keine aufwendigen
Vermittlungsentscheidungen zwischendrin getroffen werden muessen.

Nachteile sind jedoch:

* Ressourcenverschwendung, da die Leitung exklusiv reserviert wird (hohe
  Verzoegerung von Nachrichten anderer Knoten, die den Kanal waehrenddessen
  nicht nutzen koennen).
* Verbindungsaufbau kann komplex sein und lange dauern.

Leitungsvermittlung wird heute eher selten genutzt.

Analogie: Wir wollen ein Haus von Berlin nach Wien senden. Dafuer reservieren
wir die ganze Autobahn von Berlin nach Wien und senden unser Haus, Stein fuer
Stein, ueber diese Autobahn.

### Nachrichtenvermittlung

Bei der Nachrichtenvermittlung wird zwischen Sender und Empfaenger kein
exklusiver Kanal mehr allokiert. Stattdessen muss jede einzelne Nachricht von
$s$ nach $t$ einzeln adressiert werden und einzeln an den Empfaenger gesendet
werden. Dabei muss also auch immer fuer jede Nachricht ein neuer, womoeglich
anderer, Weg gewaehlt werden. Was wir uns aber erhoffen wuerden, ist, Zeit fuer
Verbindungsaufbau und -abbau zu sparen. Auch soll der Kanal fairer genutzt
werden.

Da ein Kanal (z.B. eine Reihe von Switches bzw. Routern) nicht mehr exklusiv
fuer Sender und Empfaenger reserviert ist, kann es auf einem Kanal also mehrere
moegliche Empfaenger geben. Insofern muss fuer jede Nachricht auch irgendwie
klargestellt werden, an welchen von diesen Empfaengern sie denn nun gehen
soll. Wir muessen die Nachricht also *adressieren*. Dafuer wird der $L$-bittigen
Nachricht ein Header der Laenge $L_H$ vorangestellt, welcher exakt diese
Adressinformationen beinhaltet. Das waere bei einem Brief beispielsweise eben
die Addressangabe des Empfaengers. Hierbei ist diese Adresse dann meist auch
global eindeutig.

Wir wollen die Uebertragungszeit einer Nachricht von $s$ nach $t$
herausfinden. Hierbei sei angemerkt, dass bei jedem Knoten auf dem Weg von $s$
nach $t$ immer die gesamte serialisierte Nachricht ankommen muss, bevor sie zum
naechsten Knoten weitergesendet werden kann. Das bedeutet dann also, dass wir
fuer $n$ Vermittlungsknoten die Nachricht $n + 1$ mal vollstaendig senden und
erhalten muessen. Dabei senden wir die Nachricht $n$ mal an einen
Vermittlungsknoten, und am Ende vom letzten Vermittlungsknoten zum Zielknoten
$t$ (deswegen $n + 1$). Da die Nachricht wie gesagt jedes mal vollstaendig
ankommen muss, dauert es immer $t_s$ Sekunden, bis die Nachricht beim vorherigen
Knoten serialisiert ist. Man beachte, dass hier neben der Nachricht natuerlich
auch noch der Header mitgesendet werden muss! Da wir aber nur die Nachricht
senden, und keine Signalisierungsnachrichten, haben wir die
Ausbreitungsverzoegerung $t_p$ nur einmal. Hierbei genuegt es, die
Ausbreitungsverzoegerung ueber die gesamte Distanz von $s$ nach $t$ zu
betrachten und nicht von jedem Knoten zu jedem (intermediaeren) Knoten, da diese
sich sowieso aus den Ausbreitungsverzoegerungen zwischen den Knoten ergeben
wuerde. Das ergibt dann fuer die Uebertragungszeit $T_{NV}$ bei der
Nachrichtenvermittlung:

$$T_{NV} = (n + 1) \cdot t_s + t_p = (n + 1) \cdot \frac{L_H + L}{r} +
\frac{d}{\nu c}$$

Da ein Kanal bei der Nachrichtenvermittlung nicht mehr exklusiv genutzt wird,
koennen also mehrere Knoten nacheinander Nachrichten ueber denselben Kanal
senden. Das entspricht dem *Zeitmultiplexing* (*Time Division Multiplex,
TDM*). Es koennen beispielsweise zwei Knoten $s_1$ und $s_2$ gleichzeitig eine
an $t$ adressierte Nachricht an einen Switch $i$ senden, der diese Nachrichten
dann *nacheinander* weitersenden wird (mit Betonung auf *nacheinander*).

Somit sind die Vorteile von Nachrichtenvermittlung zusammenzufassend:

* Flexibles *Zeitmultiplexing* von Nachrichten,
* Bessere Ausnutzung der Kanalkapazitaet (bei Leitungsvermittlung koennte der
  Kanal waehrend einer Pause gar nicht genutzt werden),
* Keine Verzoegerung durch Verbindungsaufbau beim Senden der ersten Nachricht.

Nachteile von Nachrichtenvermittlung sind:

* Nachrichten muessen fuer TDM moeglicherweise bei den Vermittlungsknoten
  gepuffert werden.
* Sind die Puffer der Vermittlungsknoten in ihrer Kapazitaet beschraenkt,
  koennen Nachrichten so verloren gehen.
* Die __ganze Nachricht__ muss $n + 1$ mal serialisiert werden!!!!!!!

### Paketvermittlung

Bei der Nachrichtenvermittlung ist das groesste Problem, dass wir die *gesamte*
Nachricht bei jedem Knoten neu serialisieren muessen. Diesem Problem koennen wir
durch *Paketvermittlung* entgegenwirken. Hierbei werden Nachrichten jeweils in
kleine Stuecke, *Pakete* genannt, aufgeteilt. Jedes Paket erhaelt dann einen
eigenen Header der Laenge $L_H$ und kann einzeln, unabhaengig von den anderen an
den Empfaenger gesendet werdet. Dieser Header enthaelt dabei nicht nur
Adressierungsinformation, sondern auch Hinweise darauf, wie die vollstaendige
Nachricht aus den einzelnen Paketen wieder reassembliert werden kann (z.B. mit
einer Nachrichten-ID). Die einzelnen Pakete muessen dabei im Allgemeinen nicht
gleich gross sein, haben aber eine minimale Groesse $p_{min}$ und maximale
Groesse $p_{max}$.

Wir interessieren uns wieder fuer die Uebertragungszeit einer $L$-bittigen
Nachricht. In diesem Szenario gehen wir davon aus, dass alle Pakete dieselbe
Groesse $p_{max}$ haben, sodass die Nachricht also ein ganzzahliges Vielfaches
von $p_{max}$ ist. Das essentielle hierbei ist, dass sobald das erste Paket der
Nachricht nach $p_{max}/r$ Sekunden serialisiert wurde und der letzte Bit nach
$t_p$ Sekunden angekommen ist, dieses beim naechsten Knoten im Pfad auch
*sofort* weitergesendet werden kann. D.h. dass wenn eine Nachricht in 1000
Paketen gesendet wird, wir zwischen dem Quellknoten $s$ und dem ersten
Vermittlungsknoten $i$ lediglich eine Verzoegerung von einem Paket haben, bevor
$i$ weitersenden kann. Bei Nachrichtenvermittlung haetten wir naemlich bei
Knoten $i$ auch noch auf die anderen 999 Pakete warten muessen, bevor wir die
1000 Pakete (bzw. die Nachricht) weitersenden haetten koennen. Das waeren fuer
*je zwei Knoten* also 999 mal mehr Paketserialisierungszeiten. So haben wir
zwischen je zwei Knoten lediglich eine Paketserialisierungszeit, die wir warten
muessen. Danach werden Pakete naemlich auch laufend weiter gesendet. Waehrend
Knoten $i$ naemlich das erste Pakete fuer $i + 1$ serialisiert, kommt bei $i$
schon das naechste Paket von $s$ an. Das heisst dann, dass wir fuer $n$
Vermittlungsknoten $n$ mal die extra Paketserialisierungszeit fuer das erste
Paket zahlen muessen. Dazu dann noch $L/p_{max}$ mal diese Zeit, fuer die erste
Serialisierung beim Quellknoten sowie letztlich natuerlich noch die
Ausbreitungsverzoegerung $t_p$. Insgesamt also:

$$T_{PV} = n \cdot \frac{p_{max} + L_H}{r} +
(\left\rceil\frac{L}{p_{max}}\right\rceil \cdot L_H + L) + \frac{d}{\nu c}$$

Durch Paketvermittlung koennen wir also nun nicht nur Nachrichten bzw. den Kanal
multiplexen, sondern Pakete. Das hat den Vorteil, dass die Aufspaltung der Zeit
(TDM) noch flexibler sein kann und dass wenn ein Paket verloren geht, auch nur
dieses erneut gesendet werden muss. Veraendert sich beispielsweise ploetzlich
die Qualitaet eines Kanals, so kann fuer das naechste kleine Paket eine andere
Entscheidung getroffen werden. Man muss nicht die Uebertragung der *gesamten*
Nachricht ueber diesen nun moeglicherweise schlechteren oder ausgelasteten Kanal
abwarten. Auch ist die Pufferung kleiner Pakete einfacher als die Pufferung
ganzer Nachrichten.

Die Nachteile von Paketvermittlung gegenueber anderen Verfahren sind:

* Jedes Paket benoetigt seinen eigenen Header, was viel Overhead bringt.
* Der Empfaenger muss Pakete wieder reassemblieren, was natuerlich zusaetzlich
  Zeit und Aufwand kostet.

Paketvermittlung wird in den meisten modernen Datennetzen und insbesondere durch
das Internet Protokoll (IP) verwendet.

## Adressierung im Internet

Die Sicherungsschicht bietet uns Moeglichkeiten, Knoten direkt zu
adressieren. Das geht natuerlich nur, wenn Knoten auch direkt verbunden
sind. Wir wollen uns nun ansehen, wie Rechner global adressiert und organisiert
werden. Dazu beschaeftigen wir uns mit *IP Adressen* und zwar Versionen 4
und 6. IP steht dabei fuer *Internet Protocol*.

Es ist vielleicht zuerst nuetzlich, zu besprechen, wieso IP Adressen ueberhaupt
notwendig sind. Wir haben doch schon durch MAC Adressen die Moeglichkeit, jeden
Rechner der Welt eindeutig zu identifizieren. Wieso brauchen wir dann noch diese
redundanten IP Adressen?

Betrachten wir hierzu das folgende Beispielnetzwerk mit vier Hosts und drei Switches:

```

01:23:45:67:89:AB A           C 11:36:45:CF:89:AB
                   \   0 1   /
                   S1---S---S2
                   /         \
74:FA:BB:01:15:80 B           D DE:AD:BE:EF:AF:FE
```

Der Switch $S$ haette jetzt in seiner Switching-Table bei Port 0 Eintraege fuer
Hosts $A$ und $B$ und bei Port 1 Eintraege fuer Hosts $C$ und $D$. Was ist nun
aber, wenn wir das Netzerk auf Port 1 irgendwie identifizieren wollen? Die
MAC-Adressen selbst haben ueberhaupt keine (hierarchische) Struktur, die dies
erlauben wuerden. Auch ist "Port 1" von Switch $S$ sicher keine gute oder
skalierbare Adressierungsmoeglichkeit, da sie insbesondere nicht global
eindeutig ist (Switches haben auch keine MAC Adressen). Was wir moechten ist
eine global eindeutige Identifizierung der Hosts auf Port 1, sowie dann von
Hosts innerhalb dieses Sub-Netzwerks.

Genau dafuer gibt es nun eben IP-Adressen. Sie erlauben es naemlich, Netzwerke
global eindeutig zu identifizieren und dann auch Hosts innerhalb von diesen
eindeutigen Netzwerken. Sie haben also eine *hierarchische* Struktur, die uns
die Organisation von Rechnern weltweit viel einfacher macht. Auch sei angemerkt,
dass es sehr einfach ist, seine MAC Adresse zu verfaelschen, sodass sie nicht
mehr global eindeutig ist.

### IPv4

Bei IPv4 geben wir jedem Host auf der Welt eine eindeutige *IP-Adresse*. Eine
*IP-Adresse* ist dabei eine 32-Bit lange Zahl, die aus 4 Bytes (genauer:
*Oktetten*) besteht. Diese Oktette trennt man zur Veranschaulichung mit einem
Punkt, wobei man die Byte-Werte selbst in Dezimalschreibweise angibt. Das nennt
man dann *Dotted-Decimal* Notation.

Eine IP-Adresse besteht immer aus zwei Teilen:

1. Einem Netzwerkteil, der das Netzwerk des Hosts identifiziert. Dieser ist
   immer in den oberen Bits einer Adresse.
2. Einem Hostteil, der den Host selbst innerhalb des Netzwerkes identifiziert.

Ein Beispiel fuer eine IP-Adresse waere: `192.168.1.1`. Hierbei koennten
beispielsweise die ersten drei Oktette, also `192.168.1` das Netzwerk
identifizieren und das letzte Oktett, also `.1`, den (ersten) Host innerhalb des
Netzwerkes `192.168.1`.

Um nun eine Nachricht (bzw. ein Paket, in welches eine Nachricht aufgeteilt
wurde) an einen Host mit einer bestimmten IP-Adresse zu senden, muss man diese
Nachricht an diesen Host mit seiner IP-Adresse adressieren. Der Nachricht wird
also wiederum ein Header vorangestellt, also der *IP-Header*. Der IP-Header
sowie seine Payload, also die PDU der hoeheren Schicht (Schicht 4), bilden also
die Payload eines Schicht 2 Pakets, z.B. einem Ethernet Paket.

Wir wollen einen solchen IP-Header naeher untersuchen.

#### IP-Header

IP-Header haben eine minimale Laenge von *20 Byte*. Sie enthalten neben Quell- und
Ziel-IP-Adresse noch viele weitere Informationen:

1. Bits 0-3: *Version*. Da es neben IPv4 noch IPv6 gibt, gibt dieses Feld die
   Version des verwendeten IP (Internet Protocol) an.
2. Bits 4-7: *IHL* (*IP-Header-Length*). Dieses Feld gibt in 4 Bit die Laenge
   des IP-Headers in Vielfachen von *32 Bit bzw. 4 Byte* an (ueblicherweise
   5). Das ist notwendig, das IP-Header noch optionale Felder enthalten koennen,
   dessen Anzahl variabel ist. Ein IP-Header hat somit auch eine maximale
   Laenge, naemlich $15 \cdot 4 = 60$ Byte bzw. 480 Bit.
3. Bits 8-15: *TOS* (*Type-of-Service*). Dient zur Klassifizierung
   bzw. Priorisierung von Nachrichten. Kommt es beispielsweise bei einem Router
   zum Stau, werden Nachrichten mit hoeherer Prioritaet, z.B. zeitsensitiven
   Nachrichten (Sprachuebertagung), schneller durchgelassen.
4. Bits 16-31: Gibt die Laenge des IP-Pakets, also Header plus Daten, in Bytes
   an. Somit kann ein ganzes IP-Paket also maximal 65535 Byte gross sein. Manche
   Schicht 2 bzw. Schicht 1 Protokolle haben aber eine *Maximum Transmission
   Unit* (MTU), die geringer als diese 65535 Byte sind. Beispielsweise ist die
   MTU bei FastEthernet 1500 Byte. Ist das IP-Paket groesser als diese MTU,
   wuerde sie *fragmentiert* werden, also in kleinere Pakete aufgespalten.
5. Bits 32-47: *Identification*. Ein fuer jedes IP-Paket zufaellig gewaehlter 16
   Bit Wert. Dieses wird genutzt, um Pakete einer fragmentierten Nachricht beim
   Empfaenger wieder sammeln und reassemblieren zu koennen.
6. Bits 48-50: *Flags*. Diese drei Bits stehen fuer drei Flags, naemlich:
   1. Bit 48: Reserviert und momentan noch auf 0 gesetzt.
   2. Bit 49: *Don't Fragment* (*DF*) Bit. Ist diese Flag gesetzt, darf ein
      Paket nicht fragmentiert werden. Ist das Paket dann groesser als die MTU,
      wird es einfach verworfen.
   3. Bit 50: *More Fragments* (*MF*) Bit. Dieses Bit signalisiert, ob mehr
      Fragmente folgen (= 1) oder ob dies das letzte bzw. einzige (wenn nicht
      fragmentiert wurde) Fragment ist (= 0).
7. Bits 51-66: *Fragment Offset*. Dieser 16-Bit Wert gibt die absolute Position
   der Daten in diesem Fragment bezogen auf die *unfragmentierte Nachricht* in
   ganzzahligen Vielfachen von 8 Byte an. Diese Information, zusammen mit der
   Identification und MF-Bit, ermoeglicht dann die Reassemblierung von Paketen
   beim Empfaenger.
8. Bits 67-74: *TTL* (*Time-To-Live*). Diese Zahl gibt an, wie viele Hops ein
   Paket im Netzwerk maximal machen darf. Ein *Hop* ist dabei eine Kante im
   Netzwerkgraphen, also eine L2 Direktverbindung (Ethernet, WLAN). Erhaelt ein
   Router ein IP-Paket, dekrementiert er als Erstes den TTL um 1. Ist der TTL
   dann 0, so verwirft er das Paket und sendet eine *ICMP Time-Exceeded*
   Nachricht an den Absender. Bleibt es danach positiv, sendet er es weiter. Ein
   TTL von 0 wuerde also bedeutet, dass das Paket nur an den selben Host (sich
   selbst) gesendet werden darf. Ein TTL von 1 bedeutet, dass das Paket nur an
   Hosts im selben Direktnetzwerk (Subnetz) gesendet werden darf, also
   insbesondere nie an einen Router kommen sollte. Ein TTL ist naemlich gleich
   einem Hop, also keinem Router dazwischen (nur z.B. einem odere mehreren
   Switches). Durch die TTL wird sichergestellt, das Pakete nicht endlos durch
   das Internet kreisen koennen. Der Linux Kernel setzt diesen Wert auf 64.
9. Bits 75-82: *Protocol*. Identifiziert das Protokoll auf Schicht 4, das fuer
   die Daten in der Payload genutzt wird. Gueltige Werte sind beispielsweise TCP
   (0x06) und UDP (0x08).
10. Bits 83-98: *Header Checksum*. Eine einfache, schnelle Pruefsumme, nur fuer
    den IP-Header (also nicht den Daten). Die Pruefsumme ist so konzipiert, dass
    eine Dekrementierung des TTL, welche ein Router ja durchfuehren muss, einer
    einfachen Inkrementierung dieser Pruefsumme entspricht. Somit muss also
    nicht die ganze Pruefsumme neu berechnet werden. Wie bei CRC ist nur
    Fehler*erkennung*, nicht Fehler*korrektur* moeglich.
11. Bits 99-130: *Source Address*. Die IP-Adresse des Absenders.
12. Bits 131-162: *Destination Address*. Die IP-Adresse des Empfaengers.
13. Bits 163-...: *Options* oder *Padding*. Das IP unterstuetzt eine Reihe von
    Optionen, welche als optionale Felder an den IP-Header angefuegt werden
    koennen. Eine moegliche optionale Information waere zum Beispiel ein
    Zeitstempel. Nicht alle diese Optionen sind Vielfache von 4 Byte lang. Da
    ein Paket aber auf 4 Byte "aligned" sein muss (siehe IHL), muss
    gegebenenfalls gepadded werden.

Der 49. Bit eines IP-Header ist reserviert und auf 0 gesetzt. Reservierte Bits
muessen immer auf 0 standardisiert werden. Die Alternative waere naemlich zu
sagen, dass der Wert des Bits egal ist. Wenn man sich dann aber doch eine
Bedeutung fuer dieses Bit ausdenkt, dann koennte das zu Problemen fuehren, wenn
man vorher gesagt haette, dass der Bitwert "egal" ist. Manche Dienste haetten
diesen Bit dann z.B. zufaellig auf Eins gesetzt (oder was halt in Memory lag)
oder fuer eigene Zwecke genutzt. Kriegt der Bit dann ploetzlich eine Bedeutung,
ist es schlecht, wenn der Bit bei manchen eben schon (unwissentlich) gesetzt
wird.

#### Adressaufloesung

Stellen wir uns vor, wir wissen die IP-Adresse eines Hosts. Damit kommen wir
selbst noch nicht sehr weit. Liegt der Zielhost im selben Netzwerk brauchen wir
wohl seine MAC-Adresse. Liegt er in einem anderen Netzwerk, muessen wir unsere
Nachricht zu irgendjemandem senden, der unsere Nachricht zum Netzwerk des Ziels
vermittelt.

Je nachdem, ob das Ziel nun im selben oder einem anderen Netzwerk liegt, muessen
wir also anders handeln. Wie finden wir aber ueberhaupt raus, ob wir im selben
Netzwerk sind, wie der Zielhost? Dazu koennen wir eben die hierarchische
Organisation von IP-Adressen nutzen. Wir wissen immer, wieviele Bits der
IP-Adresse das Netzwerk identifizieren, und wieviele den Host (sofern
Subnetzmaske bekannt). Daher muessen wir also nur pruefen, ob der Netzwerkteil
der Ziel-IP-Adresse gleich dem Netzwerkteil unserer eigenen IP-Adresse ist. Je
nach Resultat, gehen wir anders vor.

##### Selbes Netzwerk

Im ersten Fall, wo Quell- und Zielhost also im selben Direktnetzwerk liegen,
muessen wir von der IP-Adresse ausgehend nun irgendwie die MAC-Adresse des Ziels
herausfinden. Dazu koennen wir das *Address Resolution Protocol* (*ARP*)
verwenden. Dabei *broadcastet* der Quellhost, der die MAC-Adresse seines Ziels
wissen moechte, einen *ARP-Request* an sein lokales Netzwerk ("Who has <ip>?
Tell <my ip>). Er sendet also einen passenden ARP-Request an die
MAC-Broadcast-Adresse `ff:ff:ff:ff:ff:ff`. Jeder Switch, der diese MAC-Adresse
im Ethernet Header sieht, leitet das Paket also an alle seine Ports weiter
(ausser jenem, von welchem der Request kam), sodass auch jeder Host im
(Direkt)Netzwerk am Ende diesen Request bekommt. Liest der Zielhost dieses Paket
und erkennt, dass nach seiner MAC-Adresse gefragt wird, so sendet er einen
*ARP-Reply* ("<ip> is at <mac>") als MAC-Unicast zurueck an den Quellhost (durch
den ARP-Request waere die MAC-Adresse der Quelle ja bekannt). Der Quellhost
erhaelt somit also die Ziel-MAC-Adresse und kann seine Nachricht danach in einem
neuen Ethernet-Paket *direkt* dorthin senden.

Der genaue Aufbau eines ARP-Pakets (welcher also in der Payload eines L2 Pakets
gesendet wuerde), ist wie folgt:

1. *Hardware Type*: z.B. Ethernet.
2. *Protocol Type*: z.B. IPv4.
3. *Hardware Address Length*: die Laenge der benutzten Hardware Adressen in
   Byte, z.B. 6 fuer MAC-Adressen.
4. *Protocol Address Length*: die Laenge dre benutzten Protokoll Adressen in
   Byte, z.B. 4 fuer IPv4.
5. *Operation*: Request (0x0001) oder Reply (0x0002).
6. *Sender Hardware Address [0:31]*: Obere 32 Bit der Sender Hardware (MAC) Adresse.
7. *Sender Hardware Address [32:47]*: Untere 16 Bit der Sender Hardware (MAC) Adresse.
8. *Sender Protocol Address [0:15]*: Obere 16 Bit der Sender IP-Adresse.
9. *Sender Protocol Address [16:31]*: Untere 16 Bit der Sender IP-Adresse.
10. *Target Hardware Address [0:15]*: Obere 16 Bit der *Target* Hardware
    Address.
11. *Target Hardware Address [16:48]*: Untere 32 Bit der *Target* Hardware
    Address.
12: *Target Protocoll Address*: Die vollstaendige IP-Adresse des Ziels.

Der Host, der die MAC-Adresse eines anderen Hosts wissen moechte, wuerde die
*Operation* im ARP-Header also auf *Request* (0x0001) stellen und alle Felder,
bis auf die Target Hardware Adresse (welche er ja wissen moechte),
ausfuellen. Der Request wird dann eben an alle Hosts gebroadcastet. Der
Ziel-Host sendet dann, nachdem er diesen Request erhalten hat, einen Reply
zurueck. Dabei vertauscht er die Inhalte fuer Sender und Target, und fuellt
zusaetzlich noch das vorhin leere Target (nach Vertauschen dann Sender) Hardware
Address Feld aus. Die Operation wird dann noch auf Reply (0x0002) gesetzt und
via Unicast (unterstes Bit des ersten Oktetts der Target MAC-Adresse im Ethernet
Header auf Null gestellt) an den Absender zurueckgeschickt.

Das sieht dann beispielsweise so aus:

Request: "Who has 192.168.1.2? Tell 192.168.1.1"
Reply: "192.168.1.2 is at __04:0c:ce:e2:c8:2e__".

So weiss der Quellhost nun also, wohin er sein Paket im lokalen Netzwerk senden
muss. Er wird diesen Eintrag dann natuerlich auch noch fuer einige Zeit (5 - 10
Minuten) cachen, um nicht fuer jedes Paket erneut einen ARP-Request machen zu
muessen. Diesen Cache nennt man passend *ARP-Cache*, welcher auf Linux mit dem
`arp -a` Befehl angezeigt werden kann. Mit diesem Befehl sieht man auch, dass
eigentlich nicht nur die MAC Adresse des Ziels gespeichert wird, sondern auch
der Name des Interfaces des Zielhosts, fuer welches diese IP Adresse steht. Auch
wird versucht, den Domain Name des Zielhosts zu speichern. Komplett waere ein
ARP-Cache Eintrag also:

```
[domain name] <ip> at <mac> [on <interface>]
```

Der Zielhost kann seinen ARP-Reply auch als L2-Broadcast verschicken, sodass
alle Hosts in seiner Broadcast Domain den Reply erhalten. Abhaengig vom
Betriebssystem werden derartige unaufgeforderten ARP-Replies (*unsolicited ARP
replies*) oft auch im ARP-Cache gespeichert. Das machen beispielsweise Router
oft, sodass Hosts im Direktnetzwerk die MAC Adresse ihres Default Gateways
(insbesondere nach Aenderungen) bestenfalls immer im ARP-Cache haben.

##### Anderes Netzwerk

Wenn ein Host eine Nachricht an einen anderen Host im selben Netzwerk senden
will, und nur dessen IP-Adresse kennt, macht er also einen ARP-Request. Was ist
nun aber, wenn der Zielhost in einem anderen Netzwerk liegt? Dann kann er gar
keinen ARP-Request mehr machen, da eine Broadcastdomaene ja nur bis zum
naechsten Router geht.

Hierfuer hat jeder Host immer einen sogenannten *Default Gateway*. Das ist jener
Router, an welchen ein Host ein Paket sendet, wenn der Zielhost in einem anderen
Netzwerk liegt. Ein Host kennt dabei genauer gesagt die IP-Adresse des Default
Gateway Routers. Will ein Host also nun ein Paket an einen Host in einem anderen
Netzwerk senden, so muss der Quellhost das Paket an den Default Gateway
leiten. Kennt er dessen MAC-Adresse schon von einem vorherigen ARP-Request,
sodass die MAC-Adresse gecached wurde, kann er sein Paket direkt an den Router
senden. Ansonsten macht er mit der IP-Adresse des Routers eben vorher noch einen
ARP-Request.

Jedenfalls sendet der Quellhost dann ein Ethernet-Paket an den Router. Die
L2-Adresse (die Target MAC-Adresse im Ethernet Header) ist dabei also jene des
*Routers*. Die L3-Adresse ist aber die IP-Adresse des Target Host.

Wichtig ist hierbei, dass die MAC-Adresse, welche also nur zur Adressierung
innerhalb eines Direktverbindungsnetzes dient, beim Routing *veraendert*
wirdd. Der Router wird naemlich die Target Adresse des an ihn gerichteten
Ethernet-Headers durch eine neue ersetzen. Diese neue MAC-Adresse ist dann
entweder schon die MAC-Adresse des Zielhosts, wenn dessen Netzwerk an einem
anderen Interface des Routers haengt, oder die MAC-Adresse des Default-Gateways
des Routers selbst. Die letztere wuerde der Router eintragen, wenn der
Netzwerkteil der Target-IP-Adresse mit keinem der Netzwerke an seinen Interfaces
uebereinstimmt.

IP-Adressen, welche zur globalen End-zu-End Adresserierung zwischen mehreren
Direktverbindungsnetzen (Netzwerken), dienen, werden beim Routing aber
natuerlich __nicht__ veraendert (sofern NAT nicht beachtet wird).

#### ICMP

*ICMP* steht fuer *Internet Control Message Protocoll* und erlaubt den Austausch
von speziellen Signalnachrichten im Internet. Beispielsweise ist das notwendig,
wenn der TTL bei einem Router auf Null geht. Dann sendet der Router naemlich
eine ICMP *Time-Exceeded* Nachricht zurueck an den Absender. Andere moegliche
Signale waeren z.B.:

* *ping* (*echo request / echo reply*) um die Erreichbarkeit von Hosts zu
  ueberpruefen.
* *redirect*, um Pakete umzuleiten. Diese Nachricht wird von einem Router an
  einen Absender geschickt, wenn der Router meint, es gaebe einen besseren
  (effizienteren) Gateway als sich selbst.

Eine ICMP Nachricht wird dabei als Payload eines IP-Pakets gesendet. Es enthaelt
die folgenden Felder:

1. *Type*: Art der ICMP Nachricht (z.B. *echo request* oder *destination unreachable*)
2. *Code*: Subklassifizierung des Typs, z.B. wenn Type "destination unreachable"
   ist, dann sagt ein Code von 0, dass das Target-Netzwerk nicht erreichbar war,
   Code von 1, dass der Target-Host nicht erreichbar war etc.
3. *Checksum*: Eine Checksum fuer die ICMP Nachricht mit Payload
   (https://tools.ietf.org/html/rfc1071).
4. 32 Bit die vom Typ der ICMP Nachricht abhaengen.
5. Weitere variable Daten.

##### `ping`

Ein Beispiel von ICMP Nachrichten sind *Echo Request* und *Echo Reply*
Nachrichten. Hierbei kann ein Host einem anderen Host einen Echo Request senden
(ihn `ping`en). Erhaelt der Zielhost diese Nachricht, sendet er einen Echo
Reply zurueck. Kommt der Request nicht beim Host an, sendet irgendein Router
eine TTL Exceeded Nachricht zurueck.

Die Echo Request Nachricht wuerde dabei so aussehen:

1. Type: 0x08
2. Code: 0x00
3. Checksum
4. *Identifier*: Ein "grober" Identifier (z.B. fuer einen Stream von Requests;
   ein `traceroute`)
5. *Sequence Number*: Ein "feiner" Identifier (z.B. fuer einen konkreten Request
   in einem Stream; ein request in einem `traceroute`)

Linux Systeme geben zum Beispiel jedem `ping` Prozess einen eindeutigen
*Identifier*, und dann jedem Request ausgehend aus einem bestimmten `ping`
Prozess eine aufsteigende Sequence Number.

##### `traceroute`

Was man mit ICMP Echo Request/Reply Nachrichten zum Beispiel machen kann, ist
den `traceroute` Befehl zu implementieren. Dieser berichtet, welche Hops ein
Paket von Host $A$ zu Host $B$ macht. Er wird wie folgt implementiert:

1. Host $A$ will den Weg zu Host $B$ wissen.
2. Er sendet dabei Echo Request Nachrichten mit aufsteigender TTL. Initial ist
   dieser 1 (Hop).
3. Fuer jeden TTL Wert kommt das Paket maximal genau so viele Hops weiter. Wird
   der TTL bei einem Router Null, so sendet dieser Router dem Sender eine ICMP
   Time Exceeded Nachricht zurueck. Dann weiss der Sender also, dass nach so
   vielen Hops also dieser Router (mit dieser IP Adresse) kommt.
4. Dann erhoeht er die TTL fuer den naechsten Echo Request, sodass die Nachricht
   also einen Router weiter kommt. Das geht so lange, bis das Paket beim Ziel
   ankommt. Dann kommt eben der Echo Reply zurueck.

So kann Host $A$ also genau nachvollziehen, welchen Weg ein Paket zu $B$ gehen
wuerde.

#### IP-Zuteilung

Jeder Host wo gibt hat also eine global eindeutige IP-Adresse. Aber woher
bekommt er diese ueberhaupt? Es gibt zwei Moeglichkeiten:

1. Ein Netzwerkadministrator verteilt die Adressen statisch, per Hand (aus dem
   Pool von Adressen den ihn sein ISP fuer sein Subnetz gegeben hat)
2. Dynamisch, von einem *DHCP-Server*.

Vor allem die zweite Variante ist interessant. DHCP steht fuer *Dynamic Host
Configuration Protocol* und erlaubt es, in einem Netzwerk dynamisch IP-Adressen
zu verteilen. Moechte ein Host (Client) bei DHCP eine IP-Adresse haben, passiert
folgendes:

1. Er broadcastet einen *DHCP-Discover* Paket (L2-Broadcast), um einen
   DHCP-Server zu finden.
2. Ein (oder viele) DHCP-Server erhaelt diesen Rahmen und sendet einen
   *DHCP-Offer* mit einer moeglichen IP-Adresse zurueck an den Client.
3. Passt die Adresse dem Client, sendet er dem DHCP-Server wiederum einen
   *DHCP-Request* um zu signalisieren, dass er die Adresse haben moechte. Das
   ist so, weil es mehrere DHCP-Server in einem Netzwerk geben kann. Dann
   haetten auch mehrere Server den Discover Rahmen erhalten und auch mehrere
   einen Offer gesendet. Der Client wird aber nur einen Offer
   akzeptieren. Dafuer spezifiziert er in einem Teil des Request Rahmens einen
   ID fuer den Server, dessen Adresse er akzeptiert hat. Der Request, welcher
   dann eben wiederum gebroadcastet wird, kommt dann bei allen Servern an. Diese
   koennen dann pruefen, ob der ID im Request ihr eigener ist. Wenn ja, dann hat
   der Client also den Offer von diesem Server akzeptiert. Wenn nein, dann war
   es wohl ein anderer Server. Server, dessen Offer nicht akzeptiert wurden,
   geben ihre angebotenen Adressen dann eben wieder in ihren Addresspool.
4. Der DHCP-Server antwortet dann mit einem *DHCP-ACK*, um zu signalisieren,
   dass er die angeforderte IP-Adresse fuer den Client freigibt, oder einen
   *DHCP-NACK*, wenn er die Adresse (doch) nicht hergeben will (z.B. weil
   zwischendurch ein anderer Host die Adresse schon genommen hat).

Eine IP-Adresse, die ein Client von einem DHCP-Server erhaelt, wird auch *Lease*
bezeichnet. Ein Lease ist immer auf einen bestimmten Zeitraum beschraenkt,
welcher dem Client im DHCP-Offer bekanntgegben wird. Nach der Haelfte dieses
Zeitraums muss ein Client also vom (selben) DHCP-Server eine neue Adresse
anfordern. Dann kann der Lease entweder erneuert werden, oder ein neuer Lease
(eine neue IP Adresse) vergeben werden. Eine weitere interessante Eigenschaft
bei DHCP ist es, dass, besonders in kleinen Netzwerken, oftmals *Router* die
Aufgaben eines DHCP-Servers uebernehmen.

Oben wurde schon angemerkt, dass es auch mehrere DHCP-Server in einem Netzwerk
geben kann. Diese muessen dann aber entweder disjunkte Adressraeume verwalten,
oder, wenn nicht, ihren gemeinsamen Adressraum auf irgendwelche Art und Weise
synchronisieren ("Ich habe gerade diese Adresse vergeben; tu sie auch aus deinem
Adresspool").

Ein DHCP-Paket (L3) hat immer dieselbe Struktur. Interessant dabei sind drei
Felder:

* *Client-IP-Address*: Falls der Client noch keine IP-Adresse hat, dann sind
  hier nur Nullen. Wenn er schon eine hat, kann er sie hier reinschreiben,
  z.B. fuer Erneuerung (er wuerde fuer ein Renewal einen Request zu seinem
  DHCP-Server direkt unicasten, nicht zuerst einen Discover broadcasten.)
* *Your-IP-Address*: Wenn das DHCP-Paket ein Offer oder ein ACK ist, kommt hier
  die vergebene IP-Adresse rein.
* *Server-IP-Adress*: Wird vom Server bei Offern und ACKs entsprechend
  ausgefuellt (damit der Client dann zu diesem Server fuer ein Renewal unicasten
  kann)

Neben der IP Adresse und der Subnetzmaske liefern DHCP Server dann noch oft
weitere Informationen, zum Beispiel:

* DNS-Resolver zur Aufloesung von Domain-Namen
* Hostname (FQDN)
* Statische Routen, insbesondere den Default-Gateway
* MTU im Direktnetzwerk
* NTP Server zur Zeitsynchronisation

#### Adressklassen

Wie schon besprochen sind bestimmte Anteile einer IP-Adresse immer fuer das
Netzwerk designiert, und andere fuer Hosts. Wo genau die Trennlinie zwischen
Netzwerk- und Hostteil ist, haengt von der *Adressklasse* der IP-Adresse ab. Es
gibt naemlich folgende Adressklassen:

| Klasse | 1. Oktett | Anteile | #Netze | #Hosts |
|:-------|:----------|:--------|:-------|:-------|
|   A    | 0xxxxxxx  | N.H.H.H |  128   |  2^24  |
|   B    | 10xxxxxx  | N.N.H.H |  2^14  |  2^16  |
|   C    | 110xxxxx  | N.N.N.H |  2^21  |  2^8   |
|   D    | 1110xxxx  |        Multicast          |
|   E    | 1111xxxx  |        reserviert         |

Im Jahre 1981 war diese Aufteilung mit Sicherheit logisch. Zum Einen konnte sich
niemand vorstellen, dass man mit 32 Bit niemals auskommen wuerde. Zum Anderen
dachte man dass es deswegen OK waere, bestimmten grossen Organisationen wie
Apple, MIT oder AT&T so gigantische Adressbereiche zu geben.

Heutzutage merkt man aber zwei Sachen:

1. 32 Bit reichen nicht aus. Der letzte IPv4 Adressblock wurde am 3.2.2011
   vergeben.
2. Die Aufteilung in solche Adressklassen ist generell vollkommen ineffizient.

Deswegen wurde 1998 auch IPv6 eingefuehrt, womit wir uns spaeter beschaeftigen
werden.

#### Subnetting

Man hat schnell gemerkt, dass Adressklassen (*Classful Routing*) nicht effizient
oder effektiv sind. Man brauchte einen neue Moeglichkeit, Netzwerke zu
identifizieren und zu strukturieren. Hierfuer hat man sich das Konzept von
*Subnetzen* (*Classless Routing*) ausgedacht.

Ein Subnetz ist im Prinzip einfach nur ein Netzwerk. So erhaelt nun jedes
Interface nicht nur eine IP-Adresse, sondern auch eine 32-Bit lange
*Subnetzmaske*. Eine Subnetzmaske ist eine logische Maske, welche eine Adresse
in einen Netzanteil und einen Hostanteil teilt (so wie man es vorher durch die
obersten Bits eines Oktetts beim classful routing gemacht haette). Eine logische
1 an einer Bitstelle sagt dabei aus, dass diese Bitstelle in einer IP-Adresse
zum Netzanteil gehoert, und eine 0 dass diese Stelle im Hostanteil steht. Diese
logischen Einsen muessen dabei aneinander grenzend (contiguous) sein (man kann
also Netzanteil und Hostanteil nicht mehrmals mischen).

Will man nun konkret von einer IP-Adresse den Netzteil haben, so fuehrt man
einfach eine logische AND-Verknuepfung zwischen der IP-Adresse und der
Subnetzmaske durch. Z.B.:

```
11000000.10101000.00000000.10110010 (192.168.0.127)
11111111.11111111.11111111.00000000 (255.255.255.0)
-----------------------------------
11000000.10101000.00000000.00000000 (192.168.0.0)
```

Man sieht also hier an der Subnetzmaske, dass dieses Netzwerk 24 Netzwerkbits
und 8 Hostbits hat. Diese aus der AND-Verknuepfung resultierende Adresse, welche
in den Host Bits nur Nullen hat, nennt man die *Netzadresse* dieses
Subnetzes. Sie identifiziert nie einen konkreten Host, sondern immer das ganze
Netzwerk an sich (symbolisch). Setzt man nun alle Host Bits auf 1, erhaelt man
die (L3) *Broadcastadresse* des Subnetzes. Fuer das obige Beispeilsubnetz waere
die Broadcastadresse:

```
11000000.10101000.00000000.11111111 (192.168.0.255)
```

Es ist hierbei wichtig anzumerken, dass diese Broadcast Adresse auf Schicht 3
gilt und insofern anders als die L2-Broadcast (MAC) Adresse ist. So koennen
beispielsweise Hosts in einem Subnetz in verschiedenen Direktnetzwerken liegen
(mit mehreren Routern dazwischen). Jeder Router unterbricht dabei eine
Broadcast-Domain, sodass L2-Broadcasts nur innerhalb einer solchen Domain
gesendet wuerden. Aber ein L3 Paket, das an die L3 Broadcast Adresse gerichtet
ist, wuerde jeden Host in einem IP-Subnetz addressieren, unabhaengig von ihrer
physischen Position und ihrer Zugehoerigkeit zu einem Direktnetzwerk.

Weiss man nun also, dass ein Subnetz $N$ Netzwerkbits und $H$ Hostbits hat, so
weiss man auch, dass man in diesem Netzwerk (maximal) $2^H - 2$ Hosts
adressieren kann. Die minus zwei sind wegen der Netz- und Broadcastadresse.

Man fasst eine Subnetzmaske im Uebrigen oftmals durch die *Praefix-Notation*
zusammen. Hierbei schreibt man nicht die volle Subnetzmaske aus, sondern notiert
einfach neben einer IP-Adresse, nach einem Schraegstrich, die Anzahl an
Netzwerkbits in der Subnetzmaske. Oben hatten wir beispielsweise 24
Netzwerkbits. Dann wuerden wir die IP-Adresse als *192.168.0.255/24* notieren.

#### Supernetting

Wir koennen also einen groesseren Adressraum durch Subnetting in kleinere,
logisch gruppierte Netzwerke aufteilen. Das gleiche koennen wir natuerlich auch
in die andere Richtung machen und mehrere, kleine Netzwerke zu einem groesseren
zusammenzufassen (und dann als solches addressieren). Dieses Prinzip nennt man
*Supernetting*. Man kann zwei Subnetze aber nur dann supernetten, wenn sie sich
*nur im letzen Netzwerkbit* ihrer IP Adresse unterscheiden. Dann kann man diese
beiden Subnetze mit $N$ Netzwerkbits naemlich eindeutig durch ihr
uebergeordnetes Netz mit $N - 1$ Netzwerk identifizieren.

Nehmen wir beispielsweise diese beiden Netzadressen:

```
11000000.10101000.00000010.00000000 / 24
11000000.10101000.00000011.00000000 / 24
```

Sie unterscheiden sich also nur im letzen Netzwerkbit (Subnetzmaske ist
255.255.255.0). Das bedeutet, wir koennen diese beiden Subnetze supernetten und
durch die folgende /23 Adresse eindeutig identifizieren:

```
11000000.10101000.00000010.00000000 / 23
```

Da der ISP uns die beiden Subnetzraeume gegeben hat, hat er uns ja im Prinzip
dieses Supernet gegeben. Also gehoert dieses /23 Supernet auch sicher uns. Falls
uns das andere /24 Netz nicht gehoert, duerfen wir das natuerlich nicht machen.

#### Besondere Adressbereiche

Es gibt einige Adressbereiche in IPv4, die eine besondere Bedeutung haben,
naemlich:

* 0.0.0.0/8: *Hosts in diesem Netzwerk* (die *unspezifizierte Adresse*)
  - Wird als Quell-Adresse bei DHCP verwendet, wenn der DHCP-Client noch keine
    IP-Adresse hat.
  - Bedeutet bei Serveranwendungen: "jede Adresse die auf meinem Host verfuegbar
    ist"
  - Adressen aus diesem Bereich werden nicht geroutet (nicht von Routern
    weitergeleitet).
* 127.0.0.0/8: *Loopback-Adressen*
  - Identifiziert den lokalen Rechner (__localhost__): 127.0.0.1 (127.0.0.0 ist
    die Netzadresse, keine Host-Adresse!)
  - Wird nicht geroutet.
  - Solche Pakete werden nie gesendet, sondern einfach __gelooped__
    (*loopback*). Sprich, es wird sozusagen vom "Send-Buffer" einfach in den
    "Receive-Buffer" am selben Rechner gelegt.
* 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16: *Private Adressbereiche*
  - Adressen hieraus kann es viele, viele Male in privaten Netzwerken auf der
    Welt geben (vor allem wegen NAT).
  - Adressen innerhalb dieser Bereiche duerfen nur zwischen privaten Netzwerken
    geroutet werden.
  - Duerfen nicht in oeffentliche Netze geroutet werden (wenn der Router keinen
    Eintrag fuer das Netzwerk hat, darf er es nicht zu seinem Gateway into se
    WWW senden)
* 255.255.255.255/32: *Global Broadcastadresse*
  - Identifiziert alle Hosts im momentanen Netzwerk. Es ist sozusagen die
    Broadcastadresse von 0.0.0.0/8.
  - Wird von Linux automatisch auf die L2 (Ethernet) Broadcastadresse gesendet.
  - Wird nicht geroutet.

### IPv6

IPv4 wird auf Grund seiner Adressknappheit langsam aber sicher obsolet. Auch ist
es von seiner Header Struktur einfach nicht effizient genug. Deswegen wurde 1995
als Nachfolger von IPv4 ein neues Internet Protokoll, IPv6, vorgeschlagen. Es
wurde dann 1998 auch standardisiert und ist seitdem im Einstatz.

Neuerungen bei IPv6 gegenueber IPv4 sind:

* Vergroesserung des Addressraums von $2^{32}$ auf $2^{128}$ Adressen. Das
  entspricht ca. $6.67 \cdot 10^{24}$ Adressen pro Quadratmeter auf der Erde.
* Vereinfachung des Headerformats (ergibt effizientere Verarbeitung auf Routern)
* Aenderungen bei der IP-Fragmentierung
* Stateless Address Autoconfigruation (SLAAC) mittels ICMPv6
* ...

Wir werden nun die Eigenheiten von IPv6 detaillierter betrachten.

#### Adressformat

IPv6 Adressen sind 128 Bit lang, was 16 Bytes entspricht. In Dotted-Decimal
Notation, wie sie bei IPv4 benutzt wird, waeren also 16 Gruppen zur
Repraesentation noetig. Das ist natuerlich nicht praktisch. Deswegen notiert man
IPv6 Adressen zu je 16 Bit in Hexadezimal. Jede Gruppe von 2 Byte bzw. 4 Hex
Ziffern wird dabei durch einen Doppelpunkt von seinen Nachbarn separiert. Eine
Beispieladresse waere `2001:0db8:0000:0000:0001:0000:0000:0001`.

Solche IPv6 Adressen sind dabei in drei Teile aufgespalten:

1. Oktette 0 bis 5 (48 Bit; 3 Gruppen): Praefix
2. Oktette 6 bis 7 (16 Bit): Subnet Identifier
3. Oktette 8 bis 15 (64 Bit): Interface Identifier

Es gibt dabei noch einige Konventionen zur Notation. Wir betrachten dazu die
obige Beispieladresse.

1. Fuehrende Nullen in Bloecken (je 16 Bit) konnen immer weggelassen werden:
  ```
  2001:0db8:0000:0000:0001:0000:0000:0001 => wird zu =>
  2001:db8:0:0:1:0:0:1
  ```
2. *Hoechstens eine* Gruppe konsekutiver Bloecke von nur Nullen darf durch einen
  leeren Block abgekuerzt werden:
  ```
  2001:db8:0:0:1:0:0:1 => wird zu =>
  2001:db8::1:0:0:1
  ```
3. Gibt es mehrere Moeglichkeiten fuer (2), so waehlt man die laengste Sequenz
   von Nullbloecken. Wenn es mehrere gleich lange Bloecke gibt, so waehlt man
   den ersten. Wenn man naemlich mehr als eine solche Gruppe von Bloecken
   abkuerzt, weiss man nicht mehr, wie lange die einzelnen Gruppen waren.
   Oben waere also falsch gewesen:
   ```
   X 2001:db8:0:0:1::1
   X 2001:db8::1::1
   ```
4. Ein einzelner 0 Block darf *nicht* mit :: abgekuerzt werden:
   ```
   2001:db8:0:1:1:1:1:1 => darf nicht werden zu =>
   2001:db8::1:1:1:1:1
   ```

Die Loopbackadresse in IPv6 besteht aus nur Nullen und hinten einer Eins:

```
0000:0000:0000:0000:0000:0000:0000:0001/128
```

Man kann sie also wie folgt abkuerzen:

```
::1/128
```

Die ganze Adresse ist hierbei der Praefix.

#### Headerformat

Eine der Neuheiten bei IPv6 gegenueber IPv4 ist ein neues Headerformat. Es ist
viel einfacher als bei IPv4 und besteht aus:

1. Bits 0-3: *Version*. Gibt wie bei IPv4 Headern die IP Version an (dieses Feld
   ist das Einzige, welches bei IPv4 und IPv6 gleich ist,
   notwendigerweise). Gueltige Werte sind 4 oder 6.
2. Bits 4-11: *Traffic Class*. Aequivalent zum Type-Of-Service (TOS) Feld bei
   IPv4. Es priorisiert Pakete (*Quality-of-Service*; QoS)
3. Bits 12-31: *Flow Label*. Ein Identifikator fuer einen *Flow*. Das war
   urspruenglich gedacht, um Echtzeitanwendungen hoehere Prioritaet zu
   geben. Heute dient es dazu, Routern zu sagen, dass Pakete aus dem selben Flow
   gleich behandelt werden sollen. D.h. sie sollen ueber den selben Pfad gesendet
   werden.
4. Bits 21-36: *Payload Length*. Die Laenge der Payload des Pakets, in Bytes.
5. Bits 37-45: *Next Header*. Gibt den Typ des naechsten (inneren) Headers an,
   der am Ende des IPv6 Headers folgt. Das waere bei L4-Payloads zum Beispiel
   TCP oder UDP, oder aber auch ICMPv6.
6. Bits 46-53: *Hop Limit*. Das Selbe wie TTL bei IPv4.
7. Bits 54-181: *Source Address*. Die 128 Bit lange IPv6 IP-Adresse des Senders.
8. Bits 182-309: *Destination Address*. Die 128 Bit lange IPv6 IP-Adresse des Empfaengers.

##### Extension Header

Was neben TCP/UDP oder ICMP noch im *Next Header* Feld eines IPv6 Headers stehen
kann, ist *Extension Header*. Eine IPv6 Extension Header ist ein Zusatz zum
urspruenglichen Header.

Es gibt verschiedene Arten von Extension Header, welche durch den konkreten Wert
im Next Header Field des IPv6 Headers bestimmt ist, z.B.:

* Fragment (44): Information bezueglich Fragmentierung von IPv6 Paketen.
* Hop-By-Hop (0): Informationen, die von jedem Hop auf einem Pfad inspiziert
  werden muessen.
* Destination Options (60): Informationen, die nur vom Target inspiziert werden
  sollen.

Deswegen haengt die eigentliche "Payload" eines Extension Headers vom konkreten
Typ ab. Abstrakt besteht ein Extension Header aber aus:

1. *Next Header*: Den Typ des naechsten (inneren) Header. Das kann selbst wieder
   ein Extension Header, oder aber der eigentliche L4 PDU Header sein.
2. *Header Extension Length*: Laenge dieses Headers. Dessen Sinn ist zweifaltig.
    Erstens, dient es natuerlich dazu, die Laenge der Extension Payload zu
    bestimmen, da diese variabel ist bzw. von der Art des Headers
    abhaengt. Zweitens dient es aber auch dazu, dass wenn ein Knoten nicht
    weiss, wie es mit einer bestimmten Art von Extension Header umgehen soll, es
    diesen auch ueberspringen kann. Waere nur der Extension Header Typ
    angegeben (im Next Header Feld des IPv6 Headers), muesste der Knoten wissen,
    wie lange so ein Extension Feld von diesem Typ ist.
3. *Daten*: Die Payload des Extension Headers, welche vom Typ der Extension
   abhaengt.

Ein Beispiel fuer einen Extension Header waere der, der fuer Fragmentierung
genutzt wird. Schicht-2 Protokolle wie Ethernet haben naemlich immer eine
MTU. Ueberschreitet die Groesse der Payload eines IP-Pakets diese MTU-Groesse,
muss das Paket in kleinere Pakete fragmentiert werden. Hierfuer gab es bei IPv4
den Fragment Offset und das More-Fragments Bit/Flag. Bei IPv6 gibt es hierfuer
einen eigenen Extension Header. Die Payload dieses Extension Headers besteht
aus:

1. Bits 16-28: *Fragment Offset*
2. Bits 29-30: Einem reservierten Bereich von 2 Bit.
3. Bit 31: Einem *More-Fragments* (*MF*) Bit.
4. Bit 32-63: *Identification*

Diese Teile sollten von IPv4 Fragmentierung noch bekannt sein. Es sei aber
angemerkt: __Fragmentierung passiert bei IPv6 nur beim Sender__! Der Sender muss
also wissen, was die MTU auf dem Weg von sich selbst bis zum Sender ist. Diese
Information kann ihm beispielsweise durch einen DHCP-Server (oder Router)
mitgeteilt werden.

#### Besondere Adressen

Wie bei IPv4 gibt es wieder bestimmte *spezielle* Adressen:

* ::1/128 -- *Loopback Adresse*
  - Adressiert den lokalen Rechner.
  - Werden an den selben Rechner "zurueckgeloopt" (verlassen ihn also nie).
  - Wird nicht geroutet.
  - Analog zu 127.0.0.1/8 bei IPv4.
  - Man bemerke dass die ganze Adresse der Praefix ist.
* ::/128 - *Nicht-spezifizierte Adresse*
  - Wird nicht geroutet.
  - Analog zu 0.0.0.0/8 bei IPv4.
* fe80::/10 - *Link-Local Adressen*
  - Jedes IPv6 Interface hat eine Link-Local Adresse.
  - Sie wird aus dem Interface-Identifier (MAC Adresse) generiert.
  - Hat nur innerhalb des lokalen Links (Direktnetzwerk) Gueltigkeit.
  - Kann es in verschiedenen Netzwerken mehrmals geben (nicht global eindeutig)
  - Wird daher nicht geroutet.
  - Aehnlich zu privaten Adressen bei IPv4 (aber einzeln vergeben).
* fc00:/7 - *Unique-Local Unicast Adressen*
  - Global eindeutige Adressen, die nur privat geroutet werden duerfen.
  - Aehnlicher zu privaten Adressen bei IPv4 (das sind hier wirklich ganze Bloecke).
* ff00:/8 *Multicast-Adressen*
  - Adressen, die eine bestimmte Gruppe von Hosts adressieren.
  - Werden geroutet.

#### Multicast

Ueberfliegen wir noch einmal, welche Arten von Adressierung es gibt:

1. Unicast
   * Pakete (Rahmen), die an ein *einzelnes Ziel* adressiert sind.
   * Alle anderen Knoten im Netzwerk verwerfen solche Rahmen bzw. leiten sie
     lediglich zum Ziel (Router).
2. Broadcast
   * Pakete, die an alle Stationen im Netzwerk adressiert sind.
   * Hierfuer gibt es spezielle Broadcastadressen.
   * Auf Schicht 3 sind diese meist auf das lokale Netzsegment (Direktnetzwerk)
     beschraenkt.
3. Multicast
   * Pakete, die an eine *bestimmte Gruppe von Knoten* adressiert sind.
   * Passiert mittels spezieller Multicast-Adressen (mit ff00 Praefix).
   * Auf Schicht 2 werden Multicasts haeufig wie Broadcasts behandelt.
   * Auf Schicht 3 gibt es spezielle Protokolle, die Multicast-Adressierung auch
     ueber das lokale Netzsegment hinaus ermoeglichen.
4. Anycast
   * Pakete, die an eine beliebige Station einer bestimmten Gruppe adressiert
     sind.
   * Zum Beispiel alle DNS oder alle DHCP Server.

Wie entscheidet ein Host aber, ob ein Multicast Paket an ihn adressiert ist? Die
Antwort ist, dass es bestimmte, vordefinierte *Multicast Grupppen* gibt. Jeder
Host ist immer in zumindest einer, aber moeglicherweise auch vielen Multicast
Gruppen. Erhaelt ein Host ein IPv6 Multicast Paket, muss er nur pruefen, ob die
Multicast Gruppe, an welche das Paket adressiert ist (die IPv6 Adresse des
Pakets) eine der Gruppen ist, in welcher sich der Host befindet. Ist der Host
nicht in dieser Gruppe, braucht er es auch gar nicht weiter zu bearbeiten
(z.B. auf L3). Die folgenden Adressen sind Beispiele fuer Multicast-Gruppen:

* ff02::1 - *All Nodes*
  - Adressiert alle Knoten auf dem lokalen Link.
  - Man bemerke: Es gibt in IPv6 keine explizite Broadcast Adresse
    (__insbesondere ist die hoechste Adresse in einem Subnetz eine valide IPv6
    Adresse!__), sondern nur diese spezielle Art von Multicast.
  - Jeder Host, der IPv6 spricht, ist in dieser Gruppe. Das ist also die oben
    angesprochene Gruppe, in welcher mit Sicherheit jeder Host ist.
* ff02::2 - *All Routers*
  - Adressiert alle Router auf dem lokalen Link.
* ff02::1:2 - *All DHCP-Agents*
  - Adressiert alle DHCP-Server auf dem lokalen Link.
* ff02::1:ff00:0/104 - *Solicited-Node Adress*
  - Wird im Neighbor Discovery Protocol (NDP) verwendet, welches zur
    Adressaufloesung (wie ARP) verwendet wird.
  - Wird aus einer normalen IPv6 Adresse gebildet, indem man die unteren 24 Bit
    (6 Ziffern / 3 Byte) auf den Prefix ff02::1:ff00:0 addiert.

Wie man sieht, fangen diese bestimmten Multicast Adressen alle mit `ff02`
an. Ist das Zufall? Nein. Wie oben beschrieben haben alle IPv6 Multicast
Adressen den Prefix `ff` (bzw. `ff00::/8`). Was konkret danach kommt, bestimmmt
den *Scope* des Multicast. Das Scope kann dabei sein:

* `ff02::`: *Link-Local*: Adressen im selben Direktnetzwerk (LAN).
* `ff05::`: *Site-Local*: Adressen, z.B. im selben Gebaeude (wobei es mehrere
  Links bzw. Subnetze in diesem Gebaeude gaebe)
* `ff08::`: *Global*

Die obigen Multicast Adressen sind also alle *link-local*.

Es gibt nun des Weiteren noch spezielle Protokolle fuer Multicast. Bei IPv4
hiess dies *Internet Group Management Protocol* (*IGMP*) und bei IPv6 heisst es
nun *Multicast Listener Discovery* (*MLD*). Mit diesen Protokollen arbeiten
Router, um Multicasts ueber Netzwerke hinweg routen zu koennen. Bei MLD haelt
jeder Router dabei eine Tabelle von Hosts auf seinem Interface (Netzwerk), und
den jeweiligen Multicast Gruppen, in welchen der Host ist. Der Router kann dann
eingehende Multicast Nachrichten entsprechend filtern. Wird ein Host nun Teil
einer neuen Multicast Gruppe, z.B. wenn er zum ersten Mal von einem DHCP-Server
eine IPv6 Adresse erhaelt und so automatisch Teil der *all nodes* Multicast
Gruppe wird, so informiert er den Router dabei durch einen *MLD Report*
(`exclude Host A from the filter for this multicast group`). Das selbe tut er
auch, wenn er nicht mehr Teil der Gruppe wird. MLD ist uebrigens Teil von
ICMPv6.

Zusaetzlich dazu gibt es dann noch das Konzept des *MLD Snooping* (oder IGMP
Snooping). Hierbei hoeren Switches im Netzwerk die MLD Nachrichten ab, um zu
erfahren, auf welchen Ports bestimmte Multicast Gruppenmitglieder present oder
nicht sind. Ein Switch muss dann Multicast Pakete nur an jene Ports senden, wo
der Switch weiss, dass es dort einen Host in dieser Gruppe gibt. Fuer *all
nodes* waere das natuerlich jeder Port. Da bei MLD ueber MLD Reports immer
gemeldet wird, wenn ein Host Teil einer neuen Gruppe wird, kann der Switch aber
auch fuer andere Gruppen immer genau wissen, ob auf einem Port Mitglieder dieser
Gruppe sind, oder nicht.

#### Solicited Node Address

Es seien noch einige Details zu *Solicited Node Adressen* (*SNA*)
angemerkt. Zuerst wollen wir uns ansehen, wann und wie eine solche Adresse
generiert wird. Grundsaetzlich erhaelt eine Host fuer jegliche IPv6 Adresse,
egal ob link-local oder global, immer eine entsprechende Solicited Node
Address. Geben wir einem Host beispielsweise die globale IPv6 Adresse
`2001:0db8:1ee7:2ea2:0921:2e11:d2c6:938b`, so erhaelt er automatisch auch eine
Solicited Node Adresse. Diese wird erzeugt, inde man die letzten drei Byte (24
Bit; 6 Ziffern) der urspruenglichen IP Adresse an den 104-Bit Praefix
`ff02::1:ff00:0` hinten anhaengt. Also:

```
2001:0db8:1ee7:2ea2:0921:2e11:d2c6:938b
                                ^^^^^^^     Relevant
ff02:0000:0000:0000:0000:0001:ff00:0000  (ff02::1:ff00:0)
---------------------------------------
ff02:0000:0000:0000:0000:0001:ffc6:938b
=>
ff02::1:ffc6:938b (Solicited Node Address)
```

Wir haben also wie man hier sieht, `ff02::1:ffc6:938b` als Solicited Node Adress
erhalten. Was bedeutet das aber, genau? Bzw. was macht man mit dieser Adresse?

Das wichtige hier ist, dass diese SNA keine Unicast, sondern eine Multicast
Adresse ist. Sie identifiziert also eine *Multicast* Gruppe (im link-local
`ff02::` scope). Sobald ein Host also eine IP-Adresse bekommt, erhaelt er auch
diese SNA und faellt in dies Multicast Gruppe.

Jetzt kommt das allerwichtigste: Bei Neighbor Discovery will ein Host nun also
die L2-Adresse zur IP Adresse `2001:0db8:1ee7:2ea2:0921:2e11:d2c6:938b`
finden. Bei IPv4 haetten wir jetzt einen ARP Request an *alle Hosts im Netzwerk
gebroadcastet*. Das heisst, dass jeder Rechner, jeder Router, jeder Switch,
jeder DHCP-Server etc. im lokalen Link diesen Request erhalten, entpacken und,
bis auf einen Rechner, dann verwerfen muss. Bei IPv6 muss der Quellhost sein
Neighbor Discovery Paket (genauer: Neighbor Soliciation; siehe unten) aber nur
an diese SNA Multicast Adresse senden, die genau fuer diesen Zweck konzipiert
wurde. Im einfachen Fall, wo Switches kein MLD Snooping machen, erhaelt dann
zwar jeder Host das Paket. Sie muessen aber einfach schauen, ob sie in dieser
Multicast Gruppe sind. Wenn nicht, brauchen sie das Paket nach L2/L3 aber nicht
weiter verarbeiten (also die Neighbor Soliciation nicht weiter betrachten). Das
spart viel Arbeit. Wenn die Switches dann durch MLD Sooping sogar noch wissen,
an welchen Ports welche Multicast Pakete erwartet werden, kommt der ganze
Traffic dann sogar gar nicht erst zu allen Hosts.

Es ist vielleicht noch nicht ganz klar, wieso mehrere Hosts in der selben SNA
Multicast Gruppe enthalten sein koennen. Das ist aber vor Allem bei link-local
Adressen denkbar, welche ja aus der MAC-Adresse gebildet werden. Es koennte
beispielsweise zwei Hosts nebeneinander in einem Subnetz geben, die folgende MAC
Adressen haben:

```
de:ad:be:ef:af:fe (A)
de:ad:00:ef:af:fe (B)
```

Diese wurden passend so gewaehlt, dass ihre letzten 24 Bit, also ihre Device
Identifier (NIC) gleich sind. Z.B. koennte (A) ein Computer von Apple sein und
(B) ein Router von Cisco, sodass ihre oberen 24 Bit (OUI) verschieden
waeren. Nun bilden wir via SLAAC ihre link-local IPv6 Adressen:

```
fe80::dcad:beff:efef:affe (A)
fe80::dcad:00ff:efef:affe (B)
```

Man bemerke hier:

1. Wir haben `fe:ef:` zwischen dem OUI und dem Device Identifier eingefuegt.
2. Wir haben das zweit-linkste Bit (global/local) im linksten Byte des OUI
   geflipped (`de` => `dc`)

Jetzt haben wir also unsere link-local Adressen. IPv6 erfordert nun die
Generierung von SNA Multicast Adressgruppen, damit wir ueber Neighbor Discovery
die Hosts an diesen IP-Adresse durch Multicasts erreichen koennen. Wir bilden
also die SNA Gruppen, indem wir die unteren 24 Bit der IPv6 Adressen an den
Praefix `ff02::1:ff00:0/104` knuepfen:

```
ff02::1:ff00:0             +
fe80::dcad:beff:efef:affe
                  ^^^^^^^
-------------------------
ff02::1:ffef:affe        (A)


ff02::1:ff00:0             +
fe80::dcad:beff:efef:affe
                  ^^^^^^^
-------------------------
ff02::1:ffef:affe        (B)
```

Wie man sieht haben beide Hosts die selbe SNA Multicast Adresse fuer diesen
lokalen Link. Sendet ein Host also ein Neighbor Discovery Paket an diese SNA, um
die MAC Adresse zu einer der beiden link-local Adressen zu erhalten, werden
diese beiden Hosts es sicher bekommen und verarbeiten. Sie muessen dann also
auch als einzige Hosts (unter der Annahme dass es nicht noch mehr Hosts in
dieser SNA Gruppe gibt) das Neighbor Discovery Paket, also den Neighbor
Soliciation Request, inspizieren. Gibt es noch 10,000 weitere Hosts im lokalen
Link, sind das 9,998 Hosts weniger. In der Neighbor Soliciation steht dann die
genaue 128-Bit Target Adresse drin, welche natuerlich nur entweder (A) oder (B)
gehoeren wird. Nur einer der beiden Hosts wuerde im Weiteren dann einen Neighbor
Advertisement senden.

#### Multicast Abbildung

Um ein IPv6 Multicast Paket zu senden, muss auch auf Schicht 2 eine
entsprechende Multicast Adresse gebildet werden. Hierfuer gibt es einen
speziellen MAC-Adressraum, der nur fuer IPv6 Multicast reserviert wurde. Bei
diesem Adressraum sind die ersten zwei (der insgesamt sechs) Oktette
`33:33`. Wir erinnern uns, was das bedeutet:

1. Der unterste Bit des ersten Oktetts ist 1 $\rightarrow$ *Multicast* (und nicht
   Unicast)
2. Der zwei-unterste Bit des ersten Oktetts ist 1 $\rightarrow$ *locally
   administered* (nicht globally unique)

D.h. wenn ein Switch diese spezielle, locally administerd Multicastadresse
bekommt, wird er sofort wissen, dass es sich um einen Multicast handelt. Woher
aber weiss man aber, was die Multicastgruppe ist? Wir haben oben ja verschiedene
Multicastgruppen bei IPv6 eingefuehrt, z.B. *All Nodes* (`ff02::1`) oder *All
DHCP-Agents* (`ff02::2:1`). Fuer jede dieser IPv6 (Schicht 3) Adressen gibt es
auch eine passende MAC/Ethernet (Schicht 2) Adresse. Diese wird gebildet, indem
man zu dem `33:33` Praefix der IPv6 Multicast MAC-Adresse noch die unteren vier
Oktette (32 Bit) der IPv6 Multicastadresse hinzugibt.

Moechte man also einen Multicast an alle Router im lokalen Link senden, so
wuerde man diese an die `ff02::2` IPv6 Multicast Adresse senden wollen -- auf
Schicht 3. Auf Schicht 2 wuerde man die entsprechende MAC-Multicast Adresse dann
so bilden:

1. Man nimmt den `33:33` Prefix fuer IPv6 Multicast MAC Adressen.
2. Und gibt die unteren 32 Bit (4 Oktette; zwei Gruppen) der IPv6 Multicast
   Adresse hinzu.

Das ergibt dann also `33:33:0:0:0:2` fuer den All-Routers Multicast.

##### Hypothese

Wieso werden die unteren 32-Bit der IPv6 Adresse auf die MAC-Adresse abgebildet?
Dazu zwei Beobachtungen:

* Da wir hier von Level 2 Multicasts sprechen, sprechen wir auch nur von
  Link-Local Multicasts auf Level 3. Insofern kommt auch nur der `ff02` Praefix
  fuer die MAC-Multicast Adressen in Frage (heisst auch, dass wir ihn weglassen
  koennen).
* Wir erlauben dann eben bis zu $2^{32}$ verschiedene solche Link-Local
  Multicasts mit Praefix `ff02`. Deswegen die unteren vier Oktette der IPv6
  Multicast Adresse. Dann muessten die 80 Bit zwischen `ff02` und diesen unteren
  32 Bit also eben immer Null sein. Scheint auch so zu sein:
  https://en.wikipedia.org/wiki/Multicast_address.

#### SLAAC

IPv6 hat ein Feature, welches ermoeglicht, dass sich Hosts eigene Link-Local
IPv6 Adressen aus ihrer MAC Adresse generieren koennen. Dieses Feature heisst
*stateless address auto-configuration* (*SLAAC*). Man nennt sie *stateless*,
weil diese Adressen nicht von einem DHCP-Server vergeben werden. Eine solche
durch SLAAC generierte Adresse sieht dabei wie folgt aus:

1. Der Praefix ist `fe80::/10`, also eben fuer Link-Local Adressen (2 Oktette).
2. Danach kommen 54 Oktette nur Nullen fuer den Subnet Identifier (brauchen wir
   hier nicht, da Link Local Adressen sowieso nur fuer das lokale Subnetz/Link
   gueltig sind).
3. Danach kommt die MAC Adresse ins Spiel:
   1. Zuerst kommen die 24 Bit des *OUI* (Organizationally Unique
      Identifier).
   2. Dann kommen die 16 Bit `ff:fe`, um die 48 Bit MAC Adresse auf die vollen
      64 Bit des IPv6 Interface Identifiers zu padden.
   3. Und letztlich die restlichen 24 Bit der MAC Adresse, also der Device
      Identifier (*NIC*).

Hierbei ist zu beachten, dass beim OUI Teil der zweite Bit von rechts, also die
locally administered / globally unique Flag, *invertiert* wird. Wenn bei der MAC
Adresse dieser Bit also Null ist (globally unique), dann muss fuer die
entsprechende link-local Adresse dieser Bit auf Eins gesetzt werden (um das
selbe zu meinen). Das hat den Sinn, dass man, wenn man Link-Local Adressen
manuell verteilt, diesen Bit nicht explizit setzen muss um einen locally
administered link-local Adresse zu signalisieren. Wuerde man sonst naemlich die
MAC Adresse aus einer (vermeintlich) mit SLAAC konfigurierten Link-Local Adresse
rauslesen, so wuerde das auf eine globally-unique MAC Adresse hinweisen, wobei
wir das aber gar nicht so wollten. Als Loesung muesste man also bei manuell
vergebenen IPv6 Adressen selbst das locally-administered Bit setzen, was die
Adressen weniger uebersichtlich machen wuerde, da dieses Bit ziemlich weit links
ist. Wie oft es aber vorkommt, dass man MAC Adressen aus Link-Local Adressen
rausliest, sei dahingestellt.

Es ist im Weiteren sogar auch moeglich, *globale* Adressen automatisch zu
konfigurieren. Hierbei muss das Geraet noch die Praefixe seines Subnetzes
wissen, welche er durch das Neighbor Discovery Protocol von Routern im Netzwerk
in Form von unsolicited Router Advertisements erfahren kann. Solche globalen
Adressen mit eingebetteter MAC Adresse sind aber nicht unbedingt sicher, da man
so einen Host immer verfolgen kann. Hierzu gibt es IPv6 Privacy Extensions.

#### ICMPv6

Es gibt auch bei IPv6 wieder ICMP Nachrichten. ICMPv6 Header sind aehnlich wie
jene bei IPv4 aufgebaut:

1. Bits 0-7: *Type*. Die Art der ICMP Nachricht.
2. Bits 8-15: *Code*. Praezisiert den Typ der Nachricht, z.B. ob es sich um
   einen Echo *Request* oder Echo *Reply* handelt, wenn der Typ *Echo*
   ist. Anmerkung: Bei ICMPv4 waren das noch zwei verschiedene *Typen*.
3. Bits 16-31: *Checksum* Eine einfache Checksum fuer das ganze ICMPv6 Paket,
   inklusive Payload.
4. Bits 32-: *Payload*. Eine Nachrichtenpayload variabler Groesse (keine
   bestimmte Padding Vorschrift). Sie wird durch den Type bestimmt.

Fuer die Checksum wird ein sogenannter *Pseudo-IPv6-Header* verwendet. Hierbei
wird die Checkusumme ueber einen IPv6-Header berechnet, welche aber nur drei
Felder enthaelt:

* Next Header.
* Source IPv6 Address.
* Destination IPv6 Address.

Diese Felder sind naemlich gerade jene, welche sich bei der Uebertragung durch
se Interwebs nicht verandern. Dadurch kann man zwar eine Checksumme auf dieser
hoeheren Schicht haben (neben der FCS auf Schicht 2 und der Kanalkodierung auf
Schicht 1), was nochmals hoehere Sicherheit bringt, da kein
Fehlererkennungsverfahren optimal ist. Man muss aber Router auch nicht unnoetig
belasten. Wuerde man in diesen Pseudo Header beispielsweise noch den Hop Limit
einfuegen, so muesste man natuerlich nach jedem Hop die Checksum neu berechnen.

#### NDP

Das *Neighbor Discovery Protocol* (NDP) ist Bestandteil von ICMPv6 und bietet
eine Reihe von Funktionalitaeten an, wofuer man bei IPv4 teilweise eigene
Protokolle brauchte (z.B. ARP). Genauer gesagt definiert NDP *fuenf* ICMPv6
Paket Typen, naemlich:

1. Neighbor Solicitation (Type 135): Wie ARP-Requests.
1. Neighbor Advertisement (Type 135): Wie ARP-Replies.
3. Router Solicitation (Type 133): Nachfrage nach einem Router.
4. Router Advertisement (Type 134): Ich bin ein Router, hallo.
5. Redirect (Type 137): Ein Router teilt dem Quellhost mit, dass es einen
   besseren Gateway fuer ein Paket gibt, als sich selbst.

Diese fuenf Nachrichtenarten werden bei NDP fuer verschieden Zwecke genutzt:

* *Adressaufloesung* (NDP ersetzt ARP): Durch Neighbor Solicitations und
  Advertisements.
* *Router Discovery*: Durch Router Solicitations und
  Advertisements.
* Fuer SLAAC
* *Neighbor Unreachability Detection* (NUD). Funktioniert auch ueber Neighbor
  Solicitations.
* *Duplicate Address Detection*: Ein Knoten kann ueberpruefen, ob seine IPv6
  Adresse im Link schon benutzt wird (wie mit ARP bei IPv4). Das ist
  z.B. notwendig, wenn man sie sich durch SLAAC selbst generiert hat.
* *Parameter Discovery*: Hosts koennen Parameter fuer einen Pfad finden,
  z.B. die MTU oder den Hop Limit eines Routers auf dem lokalen Link. Bei IPv6
  muessen Hosts ja selbst, bevor dem Senden, ihre Nachricht
  fragmentieren. Router machen das bei IPv6 nicht mehr. Wenn die MTU fuer das
  gewaehlte Interface eines Routers kleiner ist als ein eingehendes IPv6 Paket,
  verwirft dieser das Paket und sendet eine ICMPv6 *Packet Too Big* (Type 2)
  Nachricht zuruck an den Quellhost. Was ein Host bei IPv6 also macht, ist
  anfangs anzunehmen, dass die MTU fuer den gesamten Pfad gleich der MTU auf dem
  lokalen Link ist, welche es eben durch diese Parameter Discovery herausfinden
  kann (oder durch DHCP). Kommt das Paket dann zu einem Router auf diesem Pfad,
  dessen MTU kleiner ist als das Paket, sendet es eben eine solche *Packet Too
  Big*-Nachricht zurueck an den Host. In dieser ist dabei auch die MTU dieses
  Routers enthalten. Der Quellhost kann sein Paket dann also entsprechend dieser
  MTU fragmentieren und probiert dann die Fragmente nochmals wegzusenden. Das
  macht er so lange, bis die Fragmente klein genug sind.

Betrachten wir konkret die Nutzung von NDP fuer Adressaufloesung, was bei IPv4
noch mit ARP ging. Die Situation ist wie bei ARP die folgende: Ein Host kennt
die IP (v6) Adresse eines Hosts auf dem lokalen Link, benoetigt aber nun noch
seine MAC (Schicht 2) Adresse, um ihm eine Nachricht zu senden.

Als Erstes wuerde dieser Host, der die MAC Adresse seines *Neighbors* finden
will, eine *Neighbor Solicitation*. Diese Neighbor Soliciation (welche also eine
ICMPv6 Nachricht ist) besteht aus den folgenden Teilen:

1. Dem ICMPv6 Header, bestehen aus Type (135 fuer Solicitations) und Code (0)
   sowie Pseudo-Header Checksum.
2. 4 Byte nur Nullen, als Padding.
3. Die 128 Bit IPv6 Adresse des Interface, dessen MAC Adresse man finden
   moechte. Erst ueber diese Adresse weiss ein Host in der Solicited Node
   Multicast Adressgruppe, ob wirklich nach seiner MAC Adresse gefragt ist, oder
   jener eines anderen Hosts in der selben Multicast Gruppe. D.h. das ist
   wirklich die global eindeutige IPv6 Adresse des Hosts.
4. Einer NDP *Option*. NDP unterstuetzt naemlich auch wieder verschiedene
   Optionen, welche aufgebaut sind aus:
   1. *Type* (1 = Source Link Layer Address)
   2. *Length* (hier 1) in __Vielfachen von 8 Byte__ fuer __die
      ganze Option__. Das passt bei Solicications z.B. eben genau: einen Byte
      fuer den Type, einen Byte fuer diese Length und dann 6 Bytes fuer:
  3. *Source Link Address*: Die MAC-Adresse des Quellhosts (der dieses NDP Paket
     absendet).

Diese Solicitation wuerde dann also an alle Hosts in der Multicast Gruppe der
Solicited Node Adresse fuer die gewuenschte Adresse (die in der Solicitation
drin) gemulticastet werden. Diese Solicited Node Address Multicast Adresse wird
dann auch auf die entsprechende MAC Multicast Adresse mit `33:33` Praefix
abgebildet. Wenn Switches kein MLD Snooping machen und nicht wissen, an welchen
Ports welche Multicast-Gruppenmitglieder sind, artet das dann auch
gewissermassen zu einem Broadcast aus. Ein Host der so ein Solication IPv6 Paket
wuerde dann also:

1. Pruefen, ob er Mitglied der Solicited Node Multicast Gruppe ist, an welche
   das Paket adressiert ist. Wenn der Switch MLD Snooping macht, waeren das
   gerade alle Hosts die in dieser SNA Gruppe sicher drin sind. Ansonsten sind
   das alle Hosts im lokalen Link. Das kann hierbei im Uebrigen schon auf
   Schicht 2 ueber die Multicast-Adresse geschehen.
2. Wenn der Host nicht in der Multicast Gruppe ist, verwirft er das Paket.
3. Wenn er schon drin ist, entpackt er das IPv6 Paket um an die ICMPv6 Nachricht
   bzw. die Neighbor Soliciation in der Payload des Pakets zu kommen. Dann
   prueft er genau ob die gewuenschte 128 Bit Adresse ihm gehoert, oder nicht.

In Retour kommt vom passenden Host dann ein *Neighbor Advertisement*
Paket. Dieses Paket ist dann *Unicast* an den Quellhost. Es ist ein wenig anders
aufgebaut als die Soliciation:

1. ICMPv6 Header:
   1. Type (136 fuer Advertisement)
   2. Code (0)
   3. Pseudo-Header Checksum
2. Drei Flag-Bits:
   1. Router-Flag (R): gestetzt, wenn der antwortende Knoten ein Router ist.
   2. Solicited-Flag (S): gibt an, ob dieses Advertisement eine Antwort auf eine
      Solication ist, oder ob es sich um ein *unsolicited Advertisement*
      handelt. Das machen zum Beispiel Router, um ihre MAC Adressen
      unaufgefordert bekannt zu machen.
   3. Override-Flag (O): gesetzt, wenn das Advertisement einen Eintrag im Cache
      des Ziels aktualisieren soll. Z.B. bei unsolicited Advertisements von
      Routern sinnvoll.
3. Target Adress: selbe IPv6 Adresse (des Targets), die auch schon in der
   Solicitation geschickt wurde. Sie bleibt also gleich.
4. Die NDP Option fuer die gesuchte MAC Adresse:
   1. Type: 2 ( = Target Link Layer Adress)
   2. Length: 1
   3. Target Link Adress: Die gesuchte MAC Adresse.

## Routing

Wir haben nun in genuegender Laenge uns damit beschaeftigt, wie Pakete innerhalb
von Direktnetzwerken zirkuliert werden. Nun interessieren wir uns aber noch
dafuer, wie wir ein Paket auch an den Nordpol senden koennen, wo wir
hochstwahrscheinlich keinen Direktlink per Ethernet haben. Dafuer muessen wir
uns mit *Routing* von Nachrichten auseinandersetzen. Hier gibt es zwei
Varianten:

* Statisches Routing
* Dynamisches Routing

### Routing Tabellen

Bei Routing sprechen wir allgemein immer von *Routing Tabellen*. Wenn wir ein
Paket an einen Router senden, entscheidet er anhand dieser Tabellen, wohin er
das Paket denn *routen* soll. Bei statischem Routing legen wir diese Tabelle per
Hand fest. Bei dynamischem Routing werden wir ueber Protokolle sprechen, die das
Aufstellen und Aktualisieren von diesen Tabellen automatisch machen.

Eine Routing Tabelle hat standardmaessig folgende Spalten:

1. Die *Netzadresse* eines dem Router bekannten Netzwerkes.
2. Die *Laenge des Praefixes*, also des Netzanteils einer IP-Adresse in diesem
   Netzwerk. Bei IPv4 bestimmt das die Subnetzmaske.
3. Den *Next-Hop* vom Router aus in Richtung dieses Netzwerkes. Diesen Next-Hop
   nennt man auch *Gateway*.
4. Das *Hardware-Interface* des Routers, ueber welches man zum Gateway gelangt
   (z.B. `eth0`).
5. Eine Metrik fuer die *Kosten* bis zum Ziel.

Nehmen wir diese Tabelle als Beispiel:

|  Destination  | Praefix |    Gateway     | Costs | Interface |
|:--------------|:--------|:---------------|:------|:----------|
| 192.168.255.8 |    30   | 0.0.0.0        |   0   |    eth2   |
| 192.168.255.0 |    29   | 0.0.0.0        |   0   |    eth1   |
| 192.168.0.0   |    24   | 0.0.0.0        |   0   |    eth0   |
| 172.16.1.0    |    24   | 192.168.255.3  |   1   |    eth1   |
| 172.16.0.0    |    23   | 192.168.255.3  |   1   |    eth1   |
| 0.0.0.0       |    0    | 192.168.255.10 |   0   |    eth2   |

Wir koennen hier viele interessante Dinge beobachten:

1. Unter *Destination* haben wir immer die *Netzadresse* eines Netzwerkes.
2. Ist das Netzwerk direkt von diesem Router aus erreichbar, liegt es also an
   einem Interface des Routers, so ist der Gateway 0.0.0.0. Diese Adresse
   identifiziert das lokale Direktnetzwerk (local link). Das bedeutet, dass der
   Router ein Paket, dass in ein Netzwerk mit diesem eingetragenen Gateway gehen
   soll, direkt per L2 Unicast senden kann.
3. Liegt das Netzwerk nicht direkt am Router angeschlossen, so ist der Gateway
   ein anderer Router.

Und noch zwei weitere Eigenschaften, die uns zum Thema des *longest prefix
matching* fuehren und die eigentliche Vorgehensweise beim Routing erklaeren:

1. Die Adressen sind nach absteigender Praefix-Laenge sortiert.
2. Die letzte Netzwerkadresse in der Destination-Spalte ist 0.0.0.0/0.

#### Longest Prefix Matching

Ein Router erhaelt nun also ein Paket. Wie routet er es? Er geht so vor:

1. Er betrachtet die Eintraege in der Tabelle in absteigender Reihenfolge,
   laengste Praefixe also immr zuerst.
2. Er berechnet dann fuer die Zieladresse des Pakets die Netzadresse, indem er
   eine logische AND-Operation der Zieladresse mit der durch die Praefixlaenge
   angegebene Subnetzmaske durchfuehrt.
3. Die erste Netztadresse, die so genau matched, wird genommen. Das ist also die
   Adresse, mit dem *longest matching prefix*.
4. Das Paket wird dann an den Next Hop (Gateway) gesendet. Ist dieser 0.0.0.0,
   so sieht er in seinem ARP-Cache nach um die MAC-Adresse des Hosts zu finden
   (muss eventuell einen ARP-Request machen) und sendet es direkt ueber den
   lokalen Link. Der Interface Eintrag sagt dem Router, ueber welches seiner
   Interfaces er den Next Hop dann erreichen kann, also von wo er das Paket
   weitersenden soll. Ist der Gateway nicht diese Default-Adresse, sendet er es
   also an den Next Hop, welcher ebenso ein Router ist (und bei sich fuer seine
   Routing Tabelle dasselbe macht). Hierbei muss zum Weitersenden des IP Pakets
   an den naechsten Gateway natuerlich womoeglich auch ein ARP Request gemacht
   werden.
5. Passt keine der nicht-`0.0.0.0` Destination Netzwerkadressen, passt am Ende
   sicher die `0.0.0.0` Adresse. Dies hat naemlich Praefix Null und eine Verundung
   mit nur Nullen ergibt auch sicher diese Nur-Nullen Adresse. Der Next Hop fuer
   diese Default-Route wird dann auch *Default Gateway* genannt.

Betrachten wir ein Beispiel. Der Router mit der obigen Tabelle erhaelt nun ein
Paket, dass an die IPv4 Adresse `172.16.1.23` adressiert ist. Dann geht der
Router so vor:

1. Er versucht die Adresse gegen den ersten Eintrag in der Tabelle zu
   matchen. Dazu ver-UND-et er diese eingehende Adresse mit der Subnetzmaske,
   die 30 Einsen (Praefixlaenge) und 2 Nullen hat, also `255.255.255.252`.
   ```
   10101100.00010000.00000001.00010111 (172.16.1.23)
   11111111.11111111.11111111.11111100 (255.255.255.252)
   -----------------------------------
   10101100.00010000.00000001.00010100 (172.16.1.20)
   ```
   Diese Adresse passt nicht zur Destination Adresse in der Tabelle
   (`192.168.255.8`). Also gehen wir eins weiter.

2. Wir probieren es mit dem zweiten Eintrag in der Tabelle:
   ```
   10101100.00010000.00000001.00010111 (172.16.1.23)
   11111111.11111111.11111111.11111000 (255.255.255.248)
   -----------------------------------
   10101100.00010000.00000001.00010000 (172.16.1.16)
   ```
   Passt wieder nicht zu `192.168.255.0`.

3. Probieren es mit dem dritten Eintrag:
   ```
   10101100.00010000.00000001.00010111 (172.16.1.23)
   11111111.11111111.11111111.00000000 (255.255.255.0)
   -----------------------------------
   10101100.00010000.00000001.00000000 (172.16.1.0)
   ```
   Passt auch nicht zu `192.168.0.0`.

4. Wir probieren weiter, nun mit dem vierten Eintrag:
   ```
   10101100.00010000.00000001.00010111 (172.16.1.23)
   11111111.11111111.11111111.00000000 (255.255.255.0)
   -----------------------------------
   10101100.00010000.00000001.00000000 (172.16.1.0)
   ```
   Ein Match! Diese Netzadresse passt zu der Destination Adresse in Zeile 4.

Wir haben nun also die Zeile fuer das Destination Netzwerk erkannt. Jetzt leiten
wir das Paket also an den in dieser Zeile spezifizierten Gateway weiter. In
diesem Fall waere das der Router (eigentlich allgemein ein Host)
`192.168.255.3`. Auch wissen wir, dass wir diesen naechsten Router ueber
Interface `eth1` erreichen koennen.

Wuerden wir die Adresse `123.123.123.123 ` routen wollen, so waere der einzige
Match in der letzten Zeile:

```
1111011.1111011.1111011.1111011 (123.123.123.123)
0000000.0000000.0000000.0000000 (0.0.0.0)
-------------------------------
0000000.0000000.0000000.0000000 (0.0.0.0)
```

Matched also (natuerlich) mit der Netzadresse in der letzten Zeile. Dann wuerden
wir dieses Paket an unseren *Default Gateway* weitersenden. Das geht so lange,
bis ein Router fuer die Adresse gar keine Moeglichkeit mehr hat. Das ist
z.B. bei Routern ganz ganz weit oben in der Hierarchie des Internet, wo es gar
keine Default Gateways mehr gibt. Diesen Raum nennt man deswegen auch *Default
Free Zone*. Solche Router muessen also immer weiter wissen.

### Dynamisches Routing

Bisher haben wir noch nicht besprochen, wie Eintraege in unsere Routing Tabelle
ueberhaupt reinkommen. Die einfachste Variante waere natuerlich, sie manuell zu
konfigurieren. Das wird ab mehreren tausend Eintraegen sehr lustig. Viel eher
wollen wir, dass unsere Routing Tabellen *dynamisch* eingerichtet und
aktualisiert wreden. Hierzu gibt es verschiedene Protokolle, welche wir uns nun
ansehen wollen. Jedenfalls ist hierbei immer die Kommunikation von Routing
Tabellen und Informationen zwischen Routern ein integraler Bestandteil, sowie
auch die Methoden, durch welche kuerzeste Pfade gefunden werden.

#### Routing Protokolle

Es gibt zwei verschiedene, grundlegende Arten von Routing Protokollen:
Distanz-Vektor Protokolle und Link-State Protokolle. Diese haben folgende
Eigenschaften:

1. Distanz-Vektor Protokolle:
   * Hier kennen Router nur die Richtung (Vektor; der Gateway) und Kosten (Distanz)
     zu einem Ziel (Destination).
   * Router haben keine Information ueber die Netztopologie, sehen also nicht
     ueber ihre unmittelbaren Nachbarn hinaus.
   * Router tauschen untereinander lediglich __akkumulierte Kosten__ aus.
   * Lassen sich verteilt implementieren.
   * Finden kuerzeste Wege mittels dem Algorithmus von *Bellman-Ford*, welcher
     nur Informationen ueber die unmittelbaren Knoten des Nachbars benoetigt.
2. Link-State Protokolle:
   * Router informieren einander nicht ueber die akkumulierten Kosten, sondern
     nur ueber die Nachbarschaftsbeziehungen jeden Knotens. D.h., zu welchen
     Knoten sie verbunden sind und wie hoch die Kosten zu diesen Knoten sind.
   * Jeder Router erhaelt also von allen anderen Routern (Knoten) deren
     Nachbarschaftsbeziehungen (Connectivity Information). Somit kennt der
     Router also letztendlich die *gesamte Netztopologie*.
   * Anders gesagt: Jeder Router erfaehrt uber den *State* jeden *Links* im
     Netzwerk.
   * Aus dieser errechnet sich dann jeder Router selbst den kuerzesten Pfad
     zu einem Ziel durch das gesamte Netzwerk.
   * Das steht im Gegensatz zu Distanz-Vektor Protokollen, wo man davon ausgeht,
     dass der naechste Router schon den besten Pfad zum Ziel kennt.
   * Lassen sich nicht verteilt implementieren.
   * Verwendet den *Dijkstra Algorithmus* um kuerzeste Wege zu finden.

Der grundlegende Unterschied zwischen Distanz-Vektor und Link-State Protokollen
ist, dass Knoten bei Distanz-Vektor Protokollen davon ausgehen, dass
Nachbarknoten den besten Pfad zu einem Ziel schon kennen. Jeder Knoten hat also
sozusagen die akkumulierten Informationen ueber einen Pfad bis zu diesem
Knoten. Erhaelt ein Knoten von seinem Nachbarn seine akkumulierten Kosten zu
einem Ziel, vertrauen wir ihm, dass diese Pfade die bestmoeglichen sind und
inkrementieren den Hop Count einfach um einen Hop. Bei Link-State Protokollen
kann man sagen, dass Knoten einenander nicht so sehr vertrauen. Ein Knoten
vertraut seinem Nachbarn nicht, dass er wirklich den besten Pfad zu einem Ziel
kennt. Deswegen will er nicht die *akkumulierten Kosten* (Hop Counts) eines
Routers zu einem Ziel haben (Routing Table), sondern einfach nur dessen
Nachbarschaftsbeziehungen (Kanten und *direkte Kosten*). Kennt der Knoten
erstmal die gesamte Netztopologie, rechnet er sich *selbst* den kuerzesten Pfad
durch das *gesamte Netzwerk* zu jedem Ziel aus. Deswegen kann man Link-State
Protokolle auch nicht verteilt implementieren, da ja jeder Knoten das gesamte
Netzwerk kennen moechte (beim Dijkstra Algorithmus: deswegen hat jeder Knoten
einen Heap fuer das gesamte Netzwerk).

https://keepingitclassless.net/2011/10/link-state-vs-distance-vector-the-lowdown/

#### Bellman-Ford

Der Algorithmus von Bellman-Ford laesst sich wie folgt implementieren:

* Wir haben ein *Parent-Array* `parent`, welches immer den Vorgaenger eines Knoten im
  Graphen enthaelt.
* Wir speichern immer die Kosten von der Wurzel, also jenem Knoten, wo wir
  anfangen, in einem Kosten- bzw. Distanzarray `distance`.
* Ein vordefiniertes Array `costs` mit den Kosten `costs[i, j]` jeder Kante $(i, j)$.
* Wir haben also einen Wurzelknoten $s$.

Dann liest sich der Algorithmus in Pseudo-Code wie folgt:

```
# Initialisierung
for n in Nodes:
	parent[n] = -1
	distance[n] = 0 if n == s else infinity

T = {s} # Menge erreichbarer Knoten

while distance[n] has changed for some n in N:
	S = {} # Updated nodes
	for i in T:
		for j in adjacent_to(i):
			if distance[i] + costs[i, j] < distance[j]:
				distance[j] = distance[i] + costs[i, j]
				parent[j] = i
				S.put(j)
	T = S

```

Praktisch gesehen machen wir im Prinzip Breadth-First-Search. Noch weitere
konkrete Implementierungsdetails:

* Der Graph sei intern durch eine Adjazenzliste dargestellt.
* Kanten seien durch Destination, Kosten und eindeutigem ID charakterisiert.
* Knoten seien durch Vorgaenger, akummulierte Distanz und eindeutigem ID
  charakterisiert.
* Wir markieren Kanten, nicht Knoten. Wir koennen Knoten naemlich mehrmals
  betrachten, um i
* Wir enqueuen dennoch Knoten.

```
struct Node {
	var distance,
	var parent,
	var id
}

struct Edge {
	var cost,
	var destination,
	var id
}

function bellman-ford(graph, root):
	visited = LookupTable()
	queue = Queue()
    queue.enqueue(root.id)

	while not queue.is_empty():
		node = queue.dequeue()
		for edge in graph.adjacent(node):
			if not visited.contains(edge.id):
				visited.put(edge.id)
				if node.distance + edge.cost < edge.destination.distance:
            		queue.enqueue(edge.destination.id)
					edge.destination.distance = node.distance + edge.cost
					edge.destination.parent = node
```

Wichtig hierbei ist, dass jeder Knoten nur seine unmittelbaren Nachbarn zu
kennen braucht bzw. die Kosten zu ihnen.

#### Dijkstra

Beim Dijkstra Algorithmus, welcher fuer Link-State Protokolle verwendet wird,
haben wir den folgenden Setup:

* Wir haben wieder ein Parent Array `parent` und akkumulierte Distanz Array
  `distance` sowie Kantenkosten Array `costs`.
* Nun haben wir noch eine Priority Queue (Heap), in welchem immer der billigste
  Knoten im Netzwerk steht. Dieser soll hier eine Art Dictionary Semantik haben,
  sodass (Knoten, Kosten)-Paare gespeichert werden koennen. Dieses Paar soll
  durch den Knoten identifiziert sein, wobei der Heap nach den Kosten geordnet
  ist (MinHeap).

```
for n in Nodes:
	parent[n] = -1
	distance[n] = 0 if n == s else infinity
	queue.put((n, distance[n]))

T = {} # Menge der bearbeiteten Knote (visited)

while T != N:
	i = queue.popMin()
	T.put(i)
	for j in adjacent_to(i):
		if distance[i] + costs[i, j] < distance[j]:
			queue.decreaseKey(j, distance[i] + costs[i, j])
			parent[j] = i
```

#### Vergleich

Wir wollen nun noch diese beiden Algorithmen bezueglich ihrer Eigenschaften
vergleichen:

1. Bellman-Ford:
   * Im $n$-ten Durchlauf betrachten wir gerade alle Pfade Laenge hoechstens
     $n$.
   * Keine komplexen Datenstrukturen notwendig.
   * Verteilte Implementierung moeglich.
   * Laufzeit in $O(|N| \cdot |E|)$
2. Dijkstra:
   * Es werden immer Pfade ueber den im jeweiligen Schritt am guenstisten Knoten
     gesucht.
   * Wurde ein Knoten abgearbeitet, so ist garantiert, dass der kuerzeste Pfad
     zu diesem Knoten gefunden ist.
   * Ressourcenintensiver als der Algorithmus von Bellman-Ford, da komplexe
     Datenstruktur (Priority Queue).
   * Wegen dem ersten Punkt muss jeder Knoten die vollstaendige Netztopologie
     kennen und die selbe grosse Priority Queue im Speicher halten. Laesst sich
     also schlechter verteilen.
   * Asymptotisch bessere Laufzeit: $O(|E| + |N| \cdot |N|)$

#### RIP

*RIP* steht fuer *Routing Information Protocol* und ist ein sehr einfaches,
altes Routing Protokoll, das bereits 1988 eingefuehrt wurde. Es dient dazu,
Routing Tabellen auszutauschen. Es hat nur ein einzige Metrik fuer Verbindungen:
den Hop Count (Anzahl an Routern zum Ziel). Das waere also so, als ob man allen
Kanten im Graphen die selben Kosten geben wuerde. Diese eine Metrik ist
natuerlich nicht sehr realistisch, da beispielsweise eine optische Leitung viel
schneller ist als eine alte Kupferleitung.

Die Funktionsweise ist bei RIP wie folgt:

1. Router senden in regelmaessigen Abstaenden (Standardwert: 30 Sekunden) den
   Inhalt ihrer Routingtabelle an die spezielle IPv4 Multicast Adresse
   `224.0.0.9`.
2. Alle Geraete in dieser Multicast-Gruppe (Router) akzeptieren das
   Update.
3. Sie inkrementieren alle Kosten in der erhaltenen Tabelle (um den Hop vom
   Quellrouter der Tabelle zu intergrieren). Gilt fuer einen Eintrag in dieser
   Tabelle dann, dass:
   * Es eine fuer diesen Router noch unbekannte Route ist, so wird sie sofort in
     die eigene Routingtabelle integriert.
   * Es eine Route ist, die der Router schon kennt, aber mit niedrigeren Kosten
     (nach dem Inkrementieren!), so uebernimmt er diese neue Route.
   * Es eine Route zu einem Ziel ist, wo der Router der seine Tabelle sendet
     gerade der Gateway ist, wird die Route entsprechend aktualisiert. Also wenn
     Router $A$ Router $B$ seine Tabelle sendet, mit einem Eintrag nach Router
     $C$ wo $A$ bei $B$ Gateway ist, dann aktualisiert $B$ seinen Eintrag nach
     den neuen Informationen von $A$, da die Informationen von $B$ fuer dieses
     Ziel $C$ ja nur relativ zu $A$ sind.
   * Andernfalls wird die vorhandene Route beibehalten und dieser Eintrag
     uebersprungen.
4. Bleiben *fuenf* aufeinanderfolgende Updates (also 2:30 Minuten bei 30
   Sekunden) von einem bestimmten Router aus, so gilt der Router als tot. Andere
   Router, die dies detektieren, schmeissen dann alle Eintreage mit diesem nun
   toten Router als Next Hop aus ihrer Tabelle raus.
5. RIP hat eine vordefiniertes Hop Count Limit von 15. Weiter entfernte Ziele
   sind ueber RIP also gar nicht erreichbar.

Betrachten wir wieder ein Beispiel. Wir nehmen hierfuer ein Netzwerk mit Routern
$A, B, C$. Anfangs sind ihre Tabellen leer, mit Ausnahme eines einzigen
Eintrags: der Route vom Router zu sich selbst.

```
    |Dst|GW|Cost|
    |---|--|----|
    | A |xx|  0 |
	      A
	    /   \
	   /     \
	  /       \
	 /         \
	/           \
   /             \
  B ------------- C
|Dst|GW|Cost|   |Dst|GW|Cost|
|---|--|----|   |---|--|----|
| B |xx|  0 |   | C |xx|  0 |
```

Dann sendet beispielsweise A zuerst seine Routing Tabelle an $B$ und
$C$. Dadurch erhalten $B$ und $C$ zum ersten Mal Routen fuer $A$:

```
    |Dst|GW|Cost|
    |---|--|----|
    | A |xx|  0 |
	      A
	    /   \
	   /     \
	  /       \
	 /         \
	/           \
   /             \
  B ------------- C
|Dst|GW|Cost|   |Dst|GW|Cost|
|---|--|----|   |---|--|----|
| B |xx|  0 |   | C |xx|  0 |
| A |xx|  1 |   | A |xx|  1 |
```

Der Gateway war in diesem Fall eben wieder das lokale Netzwerk, weil alle
Rechner hier direkt miteinander angeschlossen sind. Nun betrachten wir noch, was
passiert, wenn ein Router einen Eintrag nicht akzeptiert. Das waere der Fall,
wenn nun $B$ seinen Table multicastet. Dann wuerden $A$ und $B$ den Eintrag fuer
$B$ zum Ersten mal erhalten und folglich sofort integrieren. Aber: $C$ hat schon
einen Eintrag fuer $A$. Wenn $C$ nun also den Hop Count aller von $B$ erhaltenen
Eintraege inkrementiert und so eine Route fuer $A$ mit Kosten $2$ erhaelt,
obwohl $C$ eine Route zu $A$ mit Kosten $1$ kennt, dann wird $C$ diesen Entrag
nicht in seine Tabelle uebernehmen.

```
    |Dst|GW|Cost|
    |---|--|----|
    | A |xx|  0 |
	| B |xx|  1 |
	      A
	    /   \
	   /     \
	  /       \
	 /         \
	/           \
   /             \
  B ------------- C
|Dst|GW|Cost|   |Dst|GW|Cost|
|---|--|----|   |---|--|----|
| B |xx|  0 |   | C |xx|  0 |
| A |xx|  1 |   | A |xx|  1 |
                | B |xx|  1 |
```

#### Probleme

Wir wollen zwei Probleme betrachten, die es bei RIP bzw. Distanz-Vektor
Protokollen allgemein, geben kann.

##### Langsame Updates

Das erste Problem ist, dass es bei diesen rundenweisen, synchronen Updates oft
sehr lange dauern kann, bis Updates zu jedem Router durchsickern. Da das
maximale Hop Count Limit bei RIP 15 ist und standardweise alle 30 Sekunden ein
Update von einem Router weggesendet wird, kann es also bis zu $15 \cdot 30s =
7.5 min$ dauern, bis ein Update vom einen Ende des Pfades zum anderen kommt:


```
    30s      30s      30s      30s
(s) ---> (i) ---> ... ---> (j) ---> (t) 7,5 min!
```

Eine einfache Loesung hierfuer sind sogenannte *Triggered Updates*. Die Idee
dafuer ist einfach, dass ein Router neue Aenderungen an seiner Tabelle sofort
weitersagt. So fuehrt ein einziges Update von einem Router also zu einer "Welle"
von Updates im ganzen Netzwerk. Das hat den grossen Nachteil, dass das Netzwerk
in dieser Zeit sehr stark belastet sein kann (denkbar, wenn Routing Tabellen
mehrere hundert tausend Eintreage haben und es viele hundert Router in einem
Netzwerk geben kann).

Man spricht bei der Zeit, bis alle Knoten im Netz eine einheitliche Sicht auf
das Netzwerk haben, auch von der *Konvergenzzeit*. Sind also alle Router im
Netzwerk nach einer veraenderten Route "stabil", haben sie *Konvergenz*
erreicht. Bei RIP ist die Konvergenzzeit eben sehr hoch.

##### Count to Infinity

Betrachten wir fuer dieses Problem das folgende Netzwerk (mit eingezeichneten
Kantenkosten):

```
      (C)
     /   \
  1 /     \ 100
   /       \
  /         \
(A) ------- (B)
       1
```

Nun faellt der Link von $A$ nach $C$ aus:

```
      (C)
     /   \
  1 X     \ 100
   X       \
  /         \
(A) ------- (B)
       1
```

Host $A$ hatte vorher einen Eintrag nach $C$ in seiner Tabelle. Da es nach fuenf
fehlenden Updates merkt, dass der Host tot ist, loescht $A$ dann $C$ aus seiner
Routing Tabelle. $B$ hatte zuvor auch einen Eintrag fuer Host $C$ in seinem
Routing Table, naemlich mit einem Hop ueber Host $A$. Nun nehmen wir an, dass
$B$ seine Routing Tabelle nach dem RIP Protokoll multicastet (da die Reihenfolge
von Updates ja unspezifiziert ist). Es sendet dabei als Distanz-Vektor Protokoll
nur den Vektor, also die Adresse von seinen Eintraegen, sowie dessen Distanz
(Kosten). Hier sei der Fokus darauf, dass eben nicht der Gateway mitgesendet
wird. $A$ erhaelt nun also den (2-spaltigen) Table von $B$ und sieht den Eintrag
fuer den Pfad nach $C$. Da $A$ nach den fuenf fehlenden Updates keinen Eintrag
fuer $C$ mehr hat, nimmt es diesen Eintrag in seine Tabelle und inkrementiert
den Hop Count von 1 auf 2. Etwas spaeter sendet $A$ seine Tabelle nach $B$. Da
$B$ den Router $A$ als Gateway zu $C$ eingetragen hat, muss $B$ seine Route nach
$C$ entsprechend dem Eintrag von $A$ aktualisieren. D.h. $B$ inkrementiert
seinen Hop Count nach $C$ ueber $A$ um eins. Dann sendet $B$ seine Tabelle
wieder nach $A$, und $A$ sieht das sein Gateway nun einen Pfad der Laenge $2$ zu
$C$ hat, also muss $A$ seinen Hop Count nach $C$ ueber $B$ auf drei
inkrementieren, dann sendet $A$ seine Routing Tabelle zu $B$ ...

Es ist also ein Zyklus zwischen $A$ und $B$ entstanden. Die Hop Counts wuerden
so lange upgedatet, bis der Hop Count (nach RIP) das Maximum von $15$
ueberschreitet. Danach, mit einem Hop Count von $16$, gilt der Knoten sowohl von
$A$ als auch $B$ unerreichbar. Dieses Problem bei Distance-Vektor Protokollen
wird deswegen auch *Count to Infinity* genannt. Es sei aber angmerkt, dass
"Count to Infinity" nur ein fancy Name fuer einen Routing Loop ist. Das Problem
oben war ja, dass es eine Schleife zwischen $A$ und $B$ gab, weil sich diese
Knoten gegenseitig als Gateways eingetragen hatten.

Es gibt hierfuer drei Loesungen:

1. Split Horizon
   * "Sende dem Nachbarn, von dem du die Route zu $X$ gelernt hast, keine Route
     zu $X$"
   * "Sende deinem Gateway keine Routen ueber ihn selbst"
   * $B$ haette $A$ also nach dem Ausfallen von $C$ erst gar nicht eine Route zu
     $C$ geschickt, weil die Route von $B$ ueber $A$ ging.
   * Wenn es aber noch mehr Knoten im Netzwerk gibt, loest Split Horizon das
     Problem auch nicht. Wichtig hierbei ist insbesondere, dass es nicht
     spezifiziert ist, in welcher Reihenfolge Knoten ihre Tabellen
     austauschen. So koennten also dennoch ungueltige Routen verteilt werden.
2. Poison Reverse
   * "Sende dem Nachbarn, von dem du die Route zu $X$ gelernt hast, nur Routen
     zu $X$ mit unendlicher Metrik"
   * *Unendliche Metrik* bedeutet bei RIP also Metrik von 16.
   * Aehnlicher Ansatz wie Split Horizon, insbesondere da unendliche Metrik
     mehr oder minder dasselbe wie gar kein Eintrag ist.
   * Loest das Problem also insbesondere bei mehreren involvierten Knoten nicht.
3. Path Vector
   * Sende bei Updates nicht nur Ziel (Vektor) und Kosten (Distanz), sondern
     auch den vollstaendigen Pfad, ueber den das Ziel erreicht wird.
   * Ein Router prueft dann vor Installation einer Route in seiner Tabelle, ob
     er wohl nicht selbst in dieser Route drin ist.
   * Somit werden Zyklen im Graph also vermieden.

#### Echte Routing Protokolle

Wir haben schon RIP als ein echtes Routing Protokoll kennengelernt. Es wird aber
heute fast nirgends mehr benutzt, weil es schon sehr alt ist und viele
Schwaechen hat (z.B. Count to Infinity). Wir wollen uns noch andere Beispiele
fuer echte Protokolle ansehen:

* Distanz-Vektor Protokolle
  - *RIP* (*Routing Information Protocol*): Sehr einfaches Protokoll mit Hop
    Count als einzige Metrik. Geeignet fuer eine geringe Anzahl von Netzen.
  - *IGRP* (*Interior Gateway Routing Protocol*): Proprietaeres Routing
    Protokoll von Cisco, welches komplexere Metriken unterstuetzt als RIP.
  - *EIGRIP* (*Enhanced Interior Gateway Routing Protocol*): Verbesserte Form
    von IGRP, mit verbesserten Konvergenzeigenschaften.
* Link-State Protokolle
  - *OSPF* (*Open Shortest Path First*): Industriestandard fuer mittlere bis
    grosse Netzwerke.
  - *IS-IS* (*Intermediate System to Intermediate System*): Seltener
    eingesetztes, leistungsfaehiges Protokoll, welches unabhaengig von IP ist,
    da es sein eigenes L3-Protokoll mitbringt. Es wird insbesondere fuer grosse
    ISP Netzwerke genutzt. https://en.wikipedia.org/wiki/IS-IS

IGRP hat beispielsweise fuenf verschiedene Metriken:

* Hop count,
* Bandwidth,
* Delay,
* Load,
* Reliability.

Es benutzt einen maximalen Hop Count von 100 und multicastet Routing Tabellen
alle 90 Sekunden.

https://en.wikipedia.org/wiki/Interior_Gateway_Routing_Protocol

### Autonome Systeme

Die oben vorgestellten Protokolle betrachten alle Pfade im Netz objektiv nach
ihren Metriken (Hop Count, Bandbreite, Delay etc.). Sie bieten aber keine
Moeglichkeit, Routen direkt zu beeinflussen. In Realitaet wollen wir aber
oftmals genau das machen. Uns interessieren dann zum Beispiel:

* Geldkosten, um ein Paket ueber eine bestimmte Leitung zu senden.
* Die Identitaet der Netze, ueber welche Datenverkehr (Traffic) weitergeleitet
  werden soll. Zum Beispiel in welchen Laendern diese Netze sind.
* Infrastrukturentscheidungen (z.B. Belastung einzelner Router)

Routingentscheidungen auf Basis solcher nicht-objektiver Kriterien bezeichnet
man auch als *Policy-Based Routing*.

Policy-Based Routing ist vor allem im Kontext von *Autonomen Systemen*
relevant. Ein Autonomes System (AS) bezeichnet ein Netzwerk oder eine Menge von
Netzwerken, die unter einheitlicher administrativer Kontrolle stehen (z.B. durch
die deutsche Telekom). Ein AS wird durch einen 16 Bit Identifier, der
sogenannten AS-Nummer, identifiziert. Dieser wird von der IANA vergeben.

Bei den oben angefuehrten Beispielen fuer Routing Protokollen wurde auch das
Interior Gateway Routing Protocol (IGRP) angesprochen. Weswegen heisst es
*Interior* GRP? Bzw. was ist das Gegenteil von Interior im Kontext von Routing?
Die Antwort ist:

* Innerhalb eines autonomen Systems werden *Interior Gateway Protocols* (IGPs)
  wie RIP, OSPF oder EIGRP eingesetzt.
* Zum Austausch von Routen zwischen autonomen Systemen wird ein *Exterior
  Gateway Protocol* (EGP) verwendet. Das einzige in der Praxis genutzte EGP ist
  das *Border Gateway Protocol* (BGP).

Autonome Systeme sind meist kommerzielle Gruppen, wie AT&T oder
Vodafone. Hierbei gibt es dann noch drei Schichten, auch *Tiers*, genannt. Tier
3 AS sind die groessten, monolithischten Internet Service Provider (ISPs) wie
AT&T. Auf Tier 2 sind kleinere AS (Comcast, Tele2) und auf Tier 3 wiederum.

Damit Knoten im einen AS mit Knoten in einem anderen AS kommunizieren koennen,
muessen AS irgendwie miteinander verbunden werden. Hierbei gibt es zwei
Moeglichkeiten:

1. *Peering Verbindungen*: Vereinbarungen zwischen AS, Traffic untereinander zum
   gegenseitigen Nutzen beider Systeme kostenlos auszutauschen. Solche
   Verbindungen sind meist horizontal.
2. *Customer-Provider* (*C2P*) Verbindungen: Kommerzielle Vertraege zwischen
   einem Customer-AS und einem Provider-AS, damit der Customer seinen Traffic in
   das Netz des Providers leiten kann und umgekehrt. Solche C2P Verbindungen
   werden meist zwischen niedrigeren Tiers als Customer und hoeheren Tiers als
   Provider geschlossen. Die Provider werden deswegen oft auch *Upstream
   Provider* genannt. Man kann sich diese Verbindungen meist vertikal
   vorstellen.

Peerings werden meist bei *Internet Exchange Points* (*IXP*s) eingerichtet. Hier
schliessen sich mehrere ISPs zusammen um Traffic untereinander kostenlos
austauschen zu koennen. Der groesste IXP der Welt ist der DE-CIX in Frankfurt.

Die Border-Router eines AS (welche also ein EGP wie BGP einsetzen wuerden)
announcen Netzadressen und Praefixe an seine Peerings und
Upstream-Provider. Wenn ein Tier 2 AS mit seinem Traffic nicht weiter weiss,
leitet er es an seinen Default Gateway weiter, welcher ein Router eines
Upstream-Providers sein wird. Kommt es in eine Tier 1 AS und kann auch dort
nicht verarbeitet werden, so wird das Paket verworfen. Tier 1 AS muessen
naemlich *immer* wissen, wo sie Traffic hinsenden koennen, da sie keine Default
Gateways mehr haben (welche bei einem niedrigeren Tier eben Router in hoeheren
Tiers waeren). Deswegen nennt man Tier 1 auch die *Default Free Zone*.
