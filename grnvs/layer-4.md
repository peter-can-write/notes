# OSI Model: Layer 4

Die vierte Schicht des OSI Modells wird auch *Transportschicht* bzw. *Transport
Layer* genannt. Sie behandelt Multiplexing von Datenstömen zwischen zwei Knoten
im Netzwerk, sowie spezifiziert zwei Datenübertragungsprotokolle, *TCP* und
*UDP*. Weiters geschieht auf dieser Schicht auch *Network Address Translation*
(NAT). Das ist ein Mechanismus, wodurch private, zwischen Netzwerken
wiederverwendbare Adressen auf globale Adressen abgebildet werden.

### Multiplexing

Wir haben durch Schicht 1 (physikalische Schicht), 2 (Sicherungsschicht) und 3
(Vermittlungsschicht) nun die Möglichkeit, Daten zwischen Rechnern im Internet,
egal wo sie in der Welt physikalisch stehen, auszutauschen. Jedoch hat ein
Rechner meist nicht nur eine Anwendung laufen, sondern viele. Diese empfangen
und senden alle verschiedene Arten von Daten. Beispielsweise könnte der Rechner
einen Web-Server laufen lassen und muss also HTTP Requests und Responses senden
und empfangen können.  Gleichzeitig hat er aber auch einen Email Dienst, welcher
mit SMTP Nachrichten arbeitet. Wenn nun also ein IPv4 oder IPv6 Paket bei
unserem Rechner ankommt, dann müssen wir ja irgendwie entscheiden, an welche
Anwendung wir ein bestimmtes Paket weiterleiten. Wir müssen die SMTP Nachrichten
an den Mail Client und die HTTP Requests and den Web-Server weitergeben. Der
springende Punkt ist also, dass wir einem Paket noch detailliertere
Informationen geben müssen, um die jeweilige Verbindung bzw. den konkreten
Kommunikationskanal für eine Anwendung eindeutig zu identifizieren.

Dieses Konzept nennt sich auch *Multiplexing*. Ein Rechner selbst bzw. der Kanal
zwischen Empfänger und Sender ist ja selbst neutral gegenüber den Daten, welche
auf ihm transferiert werden. Am Empfänger müssen die Daten auf dem Kanal dann
aber auf einen von vielen Kanälen für die jeweilige Anwendung weitergeleitet
weden. Gleichzeitig muss der Sender die Daten von seinen verschiedenen
Anwendungen alle auf denselben Kommunikationskanal übertragen. Der Sender wird
dadurch also zu einem *Multiplexer* und der Empfänger zu einem *Demultiplexer*.

Um Multiplexing zu realisieren, gibt es auf der Transportschicht nun das Konzept
von *Ports*. Ein Rechner mag zwar nur eine IP Adresse haben, kann dann aber
viele Ports haben. Jeder Port ist dann sozusagen der Eingangspunkt für eine
bestimmte Art von Paket, für eine bestimmte Anwendung. Man denke beispielsweise
an die Übertragung von Briefen zwischen zwei Apartmentblöcken. Es gibt nur eine
Straße (Kommunikationskanal) zwischen diesen Blöcken. Ebenso haben diese Blöcke
nur eine (IP) Adresse. Es würde jedoch nie ein ganzer Apartmentblock an den
ganzen jeweiligen anderen Block einen Brief senden. Viel eher würde ein Mensch
in einem Apartment im Block an einen anderen Menschen in einem Apartment im
anderen Block senden. Wir können die Briefübertragung also nicht nur durch ein
$(\text{Quelladresse}, \text{Zieladresse})$ Tupel darstellen, sondern bräuchten
zusätzlich noch die Apartmentnummern innerhalb der Blöcke, um den Kanal
eindeutig zu identifizieren. Analog dazu braucht man bei Netzwerken eben noch
Ports, um die genaue Verbindung über einen geteilten Kanal erkennbar zu machen.

Im Weiteren gibt es auf der Transportschicht nun noch verschiedene
Übertragungsprotokolle. Analog kann man einen Brief auch per Luft oder Land von
$A$ nach $B$ versenden. Neben Zieladresse und -port bzw. Quelladresse und -port
brauchen wir also noch Information über die Übertragungsart bzw. das
Protokoll. Somit wird eine Verbindung auf der Transportschicht letztendlich
identifiziert durch ein 5-Tupel aus:

$$(\text{Quell-IP, Quell-Port, Ziel-IP, Ziel-Port, Protokoll})$$

Während auf der Vermittlungsschicht Daten noch in Pakete aufgeteilt wurden,
sprechen wir bei der Aufsplittung von Daten auf der Transportschicht von
*Segmenten*. Jedes Segment erhält dann einen eigenen Header, wodurch also die
Protocol Data Unit (PDU) der Transportschicht entsteht. Jedes dieser Segmente
mit Header wird dann selbst wieder an einen IP Header konkateniert und in ein IP
Paket gewickelt, dann in ein Ethernet Paket und letztendlich verschickt.

Konkret ist ein Port meist ein 16 Bit großer Wert. Das oben genannte 5-Tupel ist
auch genau das, was ein Socket im Kernel repräsentiert. Mit diesem kann dann
über einen File Deskriptor und den `read()` und `write()` Systemaufrufen Daten
versendet bzw. empfangen werden.

## Protokolle

Es gibt auf der Transportschicht zwei Protokolle: TCP, was
*verbindungsorientiert* ist und UDP, was *verbindungslos* ist. Wir werden zuerst
UDP betrachten und dann TCP. Jedenfalls bemerke man unbedingt die Parallelität
zwischen verbindungslosen und -orientierten Protokollen auf der
Transportschicht, und Paket- bzw. Leitungsvermittlung auf der
Vermittlungsschicht.

### Verbindungslose Übertragung

*Verbindungslose* Übertragungen sind so konzipiert, dass jedes versendete
 Segment unabhängig von allen anderen ist. Aus Sicht der Transportschicht sind
 die Pakete also sozusagen *zustandslos*. Daher muss zunächst mal der Header
 solcher Segmente jeweils alle Informationen enthalten, die die Verbindung
 identifizieren. Auf der Transportschicht sind das die Ports --- die Header von
 Segmenten verbindungsloser Protokolle enthalten also Quell- und Zielport.

Was wir dabei sogleich merken, ist dass wenn Segmente den Empfänger nicht
erreichen, sie einfach verloren gehen und grundsätzlich weder Empfänger noch
Sender etwas davon wissen. Man spricht hier also von *ungesicherter*
Kommunikation.

Da die Segmente auch alle unabhängig sind und so verschickt werden, können sie
also auch verschieden geroutet werden und insbesondere in beliebiger Reihenfolge
beim Empfänger ankommen. Gibt es also eine logische Verbindung zwischen den
Daten der Segmente, so muss man sich überlegen, wie man die Reihenfolge der
Segmente erhält. Weil Segmente also eigentlich jeweils für sich ganze
Nachrichten darstellen, nennt man verbindungslose Protokolle
*nachrichtenorientiert*. Das Gegenteil davon wäre *Stromorientierung*.

Praktisch gesehen ist verbindungslose Übertragung heutzutage Synonym mit *UDP*,
dem *User Datagram Protocol*. Nachrichten werden bei UDP dann *Datagramme*
genannt. Es ist eigentlich nur ein leichter "Wrapper" um seine Payload. Es fügt
dem IP-Paket einfach genau die minimal notwendige Information der
Transportschicht hinzu, um die Adressierung durch IP auf der Vermittlungsschicht
zu ergänzen. Die im Header enthaltenen Daten sind also genau Quellport,
Zielport, Länge von Header und Payload und letztlich noch eine Checksum. Die
Länge gibt dabei die Anzahl an Bytes von Header *und* Daten an. Die Checksumme
ist bei IPv4 optional, bei IPv6 dann aber doch notwendig. Wird sie für IPv4
nicht berechnet, wird sie einfach mit Nullen gefüllt. Wird sie schon berechnet,
wird hierfür wie bei ICMPv6 Nachrichten ein Pseudo-Header verwendet. Dieser
Pseudo-Header berechnet sich aus dem UDP Segment (Header + Payload) sowie
folgenden Informationen aus dem IP Paket:

* IP Adresse der Quelle und des Ziels
* Ein 8-Bit Feld mit nur Nullen
* Die ID des verwendeten Protokolls, also immer UDP mit ID `0x11` (17)
* Die Länge von Header und Payload

Die Vorteile von UDP sind nun wie folgt zusammenzupassen:

* UDP Segmente haben geringen Overhead, da der Header klein ist.
* Keine Verzögerung durch Verbindungsaufbau wie bei verbindungsorientierten
  Protokollen. Auch keine Verzögerung durch Retransmits.
* Gut geeignet für Anwendungen, wo es nicht so schlimm ist, wenn gelegentlich
  ein Paket verloren geht. Das sind vor allem Streaming Anwendungen wie VoIP
  oder Online-Spiele.
* Keine Beeinflussung der Daten durch Fluss- oder Staukontrolle (siehe unten)

Die Nachteile von UDP sind aber:

* Keine Zusicherung irgendeiner Form von Dienstqualität, da die Nachrichten
  ungesichert sind und die Fehlerrate somit beliebig hoch sein kann.
* Datagramme können ungeordnet ankommen, beispielsweise wenn es mehr als einen
  Pfad zu einem Ziel gibt
* Keine Flusskontrolle: der Empfänger kann überrumpelt werden.
* Keine Staukontrolle: der Sender kann das Netzwerk überlasten, wodurch die
  Verlustrate steigt, wenn Pakete in Transit verloren gehen (z.B. wenn bei einem
  Router im Puffer kein Platz mehr ist).

### Verbindungsorientierte Übertragung

Bei *verbindungsloser* Übertragung war jedes Segment eine unabhängige
Nachricht. Bei *Verbindungsorientierter* Übertragung "erstellen" wir zuerst
einen Kanal zwischen zwei Endpunkten (IP + Port). Dieser Kanal ist dann also die
dedizierte Verbindung zwischen Quell- und Zielknoten. Dieser Kanal muss dann
natürlich auch entsprechend verwaltet, also auf- und abgebaut, werden. Es gibt
also bei verbindungsorientierter Übertragung immer drei Schritte:

1. Verbindungsaufbau (*Handshake*)
2. Datenübertragung
3. Verbindungsaufbau (*Teardown*)

Während der Datenübertragung werden bei verbindungsorientierter Übertragung
einzelne Segmente, welche also nun einen Strom von Daten und nicht einzelne
Nachrichten darstellen, linear durchnumeriert. Jedes Segment erhält also eine
*Sequenznummer*. Diese erlaubt uns:

* Bestätigung erfolgreich übertragener Segmente,
* Dadurch dann die Identifikation von fehlenden Segmenten, sowie
* Erneutes Anfordern von fehlenden Segmenten und
* Zusammensetzen von Segmenten in der richtigen Reihenfolge bezüglich ihrer
  Sequenznummern

Während dem Handshake müssen sich Empfänger und Sender dann *synchronisieren*,
was dem Austausch von initialen Sequenznummern entspricht. Während der
Übertragung muss dann der *Zustand* der Verbindung gehalten werden. Mit
*Zustand* ist hierbei die aktuelle Sequenznummer sowie die bereits bestätigten
Sequenznummern gemeint.

#### Verbindungsaufbau und -abbau

Betrachten wir nun den Aufbau einer bidirektionalen Verbindung zwischen Knoten
$A$ und $B$. Knoten $A$ (o.B.d.A.) muss hierzu zuerst ein Synchronisationspaket
senden. Dieses beeinhaltet Informationen darüber, dass das Paket zum initialen
Verbindungsaufbau, also der Sychronisations, dient (`SYN` Flag). Auch wird die
initiale Sequenznummer $x$ von $A$ mitgeschickt. Dann kommt die entsprechende
Antwort auf die Synchronisation von Knoten $B$. Sie enthält wieder entsprechende
Flags, die auf die Natur des Pakets hinweisen. Dann enthält es aber noch zwei
weitere Daten:

1. Die initiale Sequenznummer $y$ der Segmente von Knoten $B$
2. Die Bestätigung der initialen Sequenznummer $x$ von $A$, sodass also das
   Segment von $A$ mit Sequenznummer $x + 1$ angefordert wird.

Nach diesen beiden Segmenten kommt noch eine letzte Antwort von $A$ für dieses
zweite Paket, das von $B$ kam. Diese Antwort enthält einzig und alleine die
Bestätigung der Sequenznummer $y$ von $B$, sodass also $y + 1$ angefordert wird.

```
A           B
|---\___    | (1) SYN, SEQ = x
|       \---|
|   ____/---| (2) SYN, SEQ = y, ACK = x + 1
|--/        |
|---\___    |
|       \---| (1) ACK = y + 1
v           v
```

Diesen drei-stufigen Verbindungsaufbau nennt man auch *3-Way Handshake*. Für den
Verbindungsabbau wird dasselbe Verfahren verwendet, wobei aber nicht
Synchronisationspakete mit der gesetzen `SYN` Flag gesendet werden, sondern
*Finish*-Pakete mit dem `FIN` Flag.

#### Datenübertragung

Bei der Datenübertragung interessieren wir uns vor Allem dafür, wann und auf
welche Weise Segmente quittiert, also bestätigt, werden. Die einfachste Variante
der Quittierung ist, dass einfach jede Nachricht einzeln quittiert wird und der
Sender erst auf die Bestätigung für ein Segment vom Empfänger wartet, bevor er
sein nächstes Segment wegschickt. Diese Methode ist zwar simpel, aber ebenso
ineffizient. Während nämlich ein Segment von $A$ nach $B$ propagiert, könnten
weitere Segmente nachgeschickt werden. Die *Round Trip Time* (RTT) wird also
nicht gut ausgenutzt, wodurch viel Bandbreite ungenutzt bleibt. Dieses Verfahren
nennt man auch __Stop and Wait__.

Eine effizientere und flexiblere Steigerung dieses simplen Prinzips ist es, dem
Sender ein *Fenster* (also eine Anzahl) an Segmenten mitzuteilen. Der Sender
darf alle Segmente im Fenster gleichzeitg senden, ohne auf individuelle
Bestätigungen warten zu müssen. So kann die Übertragungszeit effizienter
(dichter) genutzt werden. Bevor der Sender aber in das nächste Fenster übergeht,
muss er zuerst alle Segmente im momentanen Fenster bestätigt bekommen
haben. Außerdem kann man durch Anpassung der Fenstergröße die Übertragungsrate
auf zwei Weisen steuern:

* Da der Empfänger die maximale Fenstergröße festlegt, kann er *Flusskontrolle*
  durchführen. Er kann also je nach seiner eigenen Überlastung die
  Übertragungsrate des Senders steuern.
* Merkt der Sender, dass das Netzwerk momentan überlastet ist, kann er durch
  Modifikation der Fenstergröße *Staukontrolle* machen.

Wir betrachten im Weiteren folgendes Modell von Schiebefensterprotokollen:

* Sender und Empfänger haben denselben *Sequenznummernraum* $\mathcal{S} = \{0, 1,
  ..., N - 1\}$ mit $N := |\mathcal{S}|$
* Der Sender hat ein *Sendefenster* (Send Window) $W_s \subset \mathcal{S}$ mit
  $w_s = |W_s|$
* Der Empfänger hat ein *Empfangsfenster* (Receive Window) $W_r \subset \mathcal{S}$ mit
  $w_r = |W_r|$
* Bestätigungen sind im Weiteren immer kumulativ. Das bedeutet, dass ein `ACK
  = x + 1` nicht mehr nur das Segment mit Sequenznummer $x$ bestätigt, sondern
  alle Sequenznummern $\leq x$.
* Der Sender darf sein Fenster immer dann um eine Sequenznummer
  weiterschieben, wenn er das erste Segment in seinem momentanen Fenster
  bestätigt bekommen hat.
* Der Empfänger darf sein Fenster immer dann weiterschieben, wenn er die erste
  Sequenznummer in seinem momentanen Fenster erhalten und dieses Segment
  bestätigt hat (unabhängig davon, ob die Bestätigung eigentlich ankommt.)

Wir interessiern uns nun dafür, wie bei Schiebefensterprotokollen mit
Segmentverlusten umgegangen wird. Hierfür gibt es zwei Verfahren: *Go-Back-$N*$
und *Selective Repeat*. Wir wollen sie näher untersuchen. Jedenfalls ergibt sich
durch den Einsatz von Schiebefenstern die interessante Situation, dass
Verwechslungen zwischen Fenstern auftreten können. Es ist hierbei vor allem
wichtig zu verstehen, dass sich die Fenster beider Seiten während der
Übertragung von Segmenten eines Fensters verschieben können.

Betrachten wir beispielsweise den Fall, dass sowohl Sender und Empfänger den
gesamten Sequenznummernaum $\mathcal{S}$ als Fenster wählen. Nun darf der Sender
also $N$ Segmente gleichzeitig senden. Erhält er eine Bestätigung für das erste
Segment, würde er sein Fenster um eine Position weiterschieben dürfen. Nun
erhält der Empfänger also alle $N$ Segmente, für welche er dann anschließend
Bestätigungen wegsendet. Da er die Segmente erhalten und bestätigt hat, darf der
Empfänger sein Fenster (um 360 Grad) drehen. Er nimmt nämlich an, dass seine
Bestätigungen alle ankommen, und dieselben Sequenznummern (da sich das Fenster
im Kreis, also garnicht, bewegt hat) nun Segmente aus dem *nächsten* Fenster
designieren. Nun kann es aber vorkommen, dass alle Quittierungen in Transit
verloren. Dann erhält der Sender also keine einzige Besätigung, und darf sein
Fenster auch nicht weiterschieben. Nach einem Timeout sendet er nun alle
Segmente aus seinem unveränderten Fenster nochmals an den Empfänger. Der Sender
denkt ja, dass der Empfänger die Segmente nicht erhalten hat, weil sie nicht
bestätigt wurden. Wenn diese $N$ Segmente mit Sequenznummern gerade
$\mathcal{S}$ nun beim Empfänger ankommen, denkt dieser aber nun, dass das die
Segmente des nächsten Fensters sind. Der Empfänger hat die alten ja alle
bekommen und quittiert. Hier ist nun also eine Verwechslung aufgetreten. So
könnte aus diesem Datenstrom nun eine falsche Nachricht rekonstruiert werden.

Diese Art von Problem gibt es bei Go-Back-$N$ und Selective Repeat, wie wir
gleich sehen werden. Es gibt dann aber feste Grenzen für die Fenstergrößen,
unter welchem keine Verwechslungen auftreten können.

Es sei noch angemerkt, dass wenn Sende- und Empfangsfenster beide eins sind, man
wieder Stop and Wait erhält. Eine Steigerung von Stop and Wait erhält man erst,
wenn man das Sendefenster erhöht. Die Größe des Sendefensters ist also gerade
das essentielle, was diese beiden Methoden von Stop and Wait unterscheidet.

###### Go-Back-$N$

Die erste Möglichkeit zur Segmentverlustbehandlung ist das *Go-Back-$N$*
Verfahren. Die Idee hierbei ist es, dass wenn der Empfänger ein Segment nicht
erhält, er darauf verharrt, es zu bekommen. D.h. er akzeptiert stets nur die
nächste von ihm erwartete Sequenznummer. Segmente mit höheren Sequenznummern
werden schlicht und einfach verworfen.

Hat der Sender beispielsweise ein Sendefenster von vier Sequenznummern $\{0, 1,
2, 3\}$, dann sendet er also erstmal alle los. Nun geht das erste (1) Segment in
Transit verloren. Der Empfänger erhält also erstmal das Segment mit
Sequenznummer $0$. Dann sendet er also ein `ACK = 1` Segment zurück, da er ja
das nun erste Segment erwartet und somit das null-te bestätigt. Gleichzeitg
erhält er nun die höheren Sequenznummern $2$ und $3$. Diese verwirft der
Empfänger nun aber einfach! Er wartet nämlich noch immer auf das erste. Konkret
erhält der Empfänger diese höheren Segmente zwar, er sendet für sie aber dennoch
ein `ACK = 1` zurück.

Dieses Verfahren heißt also Go-Back-$N$, weil selbst wenn der Sender alle $N$
Segmente in einem Sequenznummernaum senden würde, das erste aber verloren geht,
der Sender dennoch $N$ Segmente zurückgehen muss. Wie muss das Sendefenster bei
diesem Verfahren nun also gewählt werden, sodass nicht wie oben erläutert
Verwechlungen zwischen Fenstern auftreten? Hierzu die folgenden Beobachtungen:

* Da der Empfänger immer nur das nächste erwartete Segment akzeptiert, ist
  dieses nächste erwartete Segment auch immer die erste Sequenznummer im
  Schiebefenster des Empfängers. Der Empfänger kann ja sein Fenster dann für
  jedes erhaltene, nächste Segment auch immer gleich eins weiterschieben. Wir
  gehen also imemr nur um einen Schritt weiter. Bei selective Repeat (siehe
  unten) könnten wir auf einen Schlag auch viel mehr Schritte gehen, wenn nur
  das erste Segment des Fensters fehlt und die anderen Segmente schon gepuffert
  wurden.
* Die Gefahr, nicht zwischen Fenstern unterscheiden zu können, stammt aus der
  Überlappung von einem Fenster und dem darauffolgenden Fenster. Bei Go-Back-$N$
  ist es nun aber so, dass das nächste Fenster immer nur die allererste
  Sequenznummer akzeptiert, und nicht die darauffolgenden. Solange also nur
  nicht die Anfänge des momentanen Fensters und des nächsten Fensters
  überlappen, wird es bei Go-Back-$N$ keine Fehler geben. Dann kann man nämlich
  alle Segmente des momentanen Fensters mehrmals senden. Da keines der Segmente
  der Anfang des nächsten sein wird, wird es nie eine Verwechslung geben
  Genauer darf der Anfang des nächsten also gar keine Sequenznummer des
  momentanen Fensters sein. Es gibt aber auch nur einen Fall, wo der Anfang des
  nächsten Fensters eine Sequenznummer des momentanen Fensters ist, und in
  diesem Fall überlappen sich genau die Anfänge.
* Dieser Fall, dass sich der Anfang es nächsten Fensters mit einem Segment des
  momentanen überlappt, ergibt sich wie gesagt nur in einem Fall. Das ist genau
  dann, wenn das Fenster gerade der ganze Sequenzraum ist. Dann ist das nächste
  Fenster nämlich genau eine 360 Rotation weiter, also genau dasselbe
  Fenster. Da die Rotation 360 Grad war, überlappen sich die Anfänge. Dadurch
  gibt es dann also eine Verwechslung, falls der Anfang des vorherigen Segments
  neu gesendet werden muss (Erinnernung: es ist nur der Anfang wichtig, weil
  wenn höhere Sequenznummer wiederholt werden müssen, dann sind diese für das
  nächste Fenster sowieso uininteressant, weil es noch auf sein Anfangssegment
  wartet).
* Wir können dieses Problem, dass sich die Anfänge des momentanen und nächsten
  Fensters überlappen, ganz einfach dadurch lösen, dass wir zwischen dem
  momentanen und nächsten Fenster immer genau eine Sequenznummer Platz
  lassen. D.h. wenn das erste Fenster die Sequenznummern $0, ..., N - 2$
  okkupiert, dann setzen wir den Anfang des nächsten Fensters einfach auf
  Sequenznummer $N - 1$. Dann überlappen sich zwar $N - 2$ Sequenznummern
  zwischen diesen Fenstern (alle außer $N - 2$ und $N - 1$), aber eben nicht die
  Anfänge (an Positionen $N - 1$ und $0$).

Zusammenfassend: Bei Go-Back-$N$ muss das Sendefenster also maximal $N - 1$
Sequenznummern okkupieren, damit man den Anfang des nächsten Fensters in die
Lücke setzen kann. Dann überlappen sich zwar $N - 2$ Sequenznummern, aber die
Anfänge der Fenster nicht, was die einzige Source of Badness bei Go-Back-$N$
wäre.

Go-Back-$N$ ist einfacher zu implementieren als Selective Repeat, aber weniger
effizient. Insbesonderen interessant ist bei Go-Back-$N$ aber, dass man gar
keinen Speicherpuffer beim Empfänger braucht. Da es immer nur ein einziges
mögliches Segment gibt, das der Empfänger erwartet, muss er andere nicht
zwischenspeichern. Daher braucht er auch keinen Puffer.

###### Selective Repeat

Bei Go-Back-$N$ hatte der Empfänger immer nur das nächste erwartete Segment
akzeptiert und erhaltene Segmente mit darauffolgenden Sequenznummern einfach
verworfen. Bei Selective Repeat verwerfen wir diese höeheren Nummern nun nicht,
sondern speichern sie in einen Puffer. Fehlt eine erwartete Sequenznummer im
Fenster, so teilt der Empfänger dem Sender das durch ein passendes ACK mit. Der
Sender schickt dann aber nicht wie bei Go-Back-$N$ alle Segmente ab dem
bestätigten nochmals neu, sondern nur das mit dieser konkreten
Sequenznummer. Gehen mehrere erwartete Segmente verloren, sender der Empfänger
dem Sender eben mehrere solcher ACKs.

Wählen wir für das Empfangsfenster eine Größe von $1$, so erhalten wir einfach
wieder Go-Back-$N$. Der Empfänger könnte nämlich immer nur das nächste erwartete
Segment speichern und müsste höhere ignorieren. Es wäre aber nicht zwingend
dasselbe wie Stop and Wait, da der Sender ja wohl ein größeres Fenster haben
kann und mehr als ein Segment wegsenden kann, auch wenn alle außer dem ersten
dann ignoriert würden. Im besten Fall würde der Empfänger aber immer genau ein
Segment nach dem anderen erhalten, wodurch sich das $1$-große Fenster des
Empfängers immer um eins weiterschieben würde. Würde nun aber ein erwartetes
Segment nicht ankommen, so würden höhere ignoriert und müssten dann wie bei
Go-Back-$N$ nochmals gesendet werden.

Wir überlegen uns wieder, wie groß nun die maximale Größe des Sendefensters sein
darf. Es sei wieder angemerkt, dass das Empfangsfenster in dieser Diskussion
keine Rolle spielt. Überlegen wir uns zunächst, ob $N - 1$ wieder eine passende
obere Schranke sein könnte. Dann würden sich das momentane und nächste
Sendefenster also wie oben besprochen in genau $N - 2$ stellen überlappen (nur
nicht im letzten des momentanen und im ersten des nächsten). Bei Go-Back-$N$ war
diese Überlappung kein Problem, da der Empfänger für das nächste Segment sowieso
nur die erste Sequenznummer akzeptieren würde, wo keine Überlappung war. Nun ist
es aber so, dass der Empfänger höhere Segmente für das nächste Fenster __sehr
wohl__ akzeptiert. Er puffert ja diese Segmente bei Selective Repeat. D.h. wir
haben mit dieser Überlappung von $N - 2$ Segmenten ein Problem. Konkret kann man
hier erkennen, dass eine Unterscheidung vom momentanen und nächsten Fenster nur
dann möglich ist, wenn es __überhaupt keine Überlappung__ gibt. Denn jegliche
Überlappung würde bedeutetn, dass wenn dieses Segment im momentanen Fenster neu
gesendet werden muss, der Empfänger aber schon auf die Segmente des nächsten
Fensters wartet (weil er alle Segmente erhalten hat, die ACKs aber alle in
Transit krepiert sind), er dieses Segment für das nächste Fenster puffern würde,
und nicht als erneutes Senden eines alten Segments erkennen würde.

Wir wollen also keinerlei Üeberlappung zwischen zwei konsekutiven Fenstern. Das
erreichen wir im günstigsten Fall genau dadurch, dass wir die Sendefenstergröße
auf $\lfloor N/2 \rfloor$ setzen. Dann würde das momentane Fenster nämlich die
eine Hälfte des Sequenznummernaums okkupieren und das nächste Fenster dann immer
die zweite Hälfte. Es gäbe also gar keine Überlappung. Natürlich kann man die
Fenstergrößen auch kleiner wählen, aber dies ist die obere Grenze.

Bei Selective Repeat benötigt der Empfänger einen Puffer, was bei Go-Back-$N$
noch nicht der Fall war.

#### TCP

Wir wollen nun *TCP*, das *Transmission Control Protocol*, als ein Beispiel
eines verbindungsorientierten Protokolls untersuchen. Rund 90% des
Internetverkehrs funktioniert über TCP. Es bietet abgesicherte (im Sinne von,
Segmente kommen durch Neu-Übermittlung sicher irgendwann an) sowie
stromorientierte (anstatt nachrichtenorientierte) Übertragung mittels Sliding
Window und Selective Repeat. Bei TCP werden aber nicht ganze Segmente, sondern
immer einzelne Bytes bestätigt. D.h. wenn ein Segment $b$ Bytes enthält, wird
der nächste `ACK` den Offset $b$ gegenüber dem letzten ACK haben.

Ein TCP Segment hat natürlich einen TCP Header. Währen der UDP Header wirklich
nur minimal klein war, enthält ein TCP Header nun schon viel mehr
Infromationen. Er ist minimal 20 Byte groß (wie ein IP Header) besteht also aus:

* Einem *Source Port* (wie bei UDP)
* Einem *Destination Port* (wie bei UDP)
* Der *Sequence Number*
* Der *Acknowledgement Number* (`ACK`)
* Einem (Data) *Offset*, der angibt, an welchem Offset die Daten im TCP Segment
  anfangen. Ein TCP Header kann nämlich noch *Optionen* enthalten. Somit gibt
  dieser Wert also eigentlich an, wie groß der Header insgesamt ist.
* Reserved: Momentan auf Null gesetzter Bereich.
* Dann sechs Flags:
  - *URG* (urgent): Ist dieses Bit gesetzt, dann heißt das, dass das Segment
    *urgent data* enthält. Es gibt dann noch ein *Urgent Pointer* Feld im Header
  (siehe unten). Dann sind die Daten im Segment ab dem ersten Byte bis zum
  Urgent Pointer also die wichtigen Daten. Diese sollen dann sofort an höhere
  Schichten weitergeleitet werden. Die restlichen Daten werden dann ganz normal
  verarbeitet. Im Linux-Kernel kann ein Prozess z.B. das `SIGURG` Signal
  erhalten. Dann weiß er, dass auf einem Socket nun urgent Daten anstehen.
 - *ACK* (acknowledgement): Erst durch dieses Flag wird das Acknowledgement
   Number Feld "aktiv". D.h. wenn dieser Bit gesetzt ist, dann handelt es sich
   im Ack. Number Field um eine Bestätigungsnummer. Ansonsten wird das Feld
   ignoriert. Eine "reine Bestätigung" wie wir sie oben betrachtet haben, hätte
   also keine Daten und dieses Bit gesetzt. In der Praxis ist es aber möglich,
   dass der Empfänger (bzw. eben Knoten $B$) dem Sender Daten auch senden
   möchte. Dann kann er in einem Segment gleichzeitig durch die Ack. Number das
   letzte Segment von Knoten $A$ bestätigen, und seine Daten an $A$
   senden. Dieses Konzept nennt man *piggy-backing*.
 - *PSH* (push): Ist dieses Flag gesetzt, werden die Daten im Segment nicht beim
   Sender und Empfänger nicht gepuffert. Normalerweise würde der TCP Stack des
   Senders Daten zuerst ein wenig puffern, bevor er ein Segment ins Netzwerk
   wegschickt und Traffic macht. So wird nicht jeder einzelne Byte, den man in
   einen Socket reinschickt, sofort als ein IP Paket mit einem Byte Nutzdaten
   weggeschickt. Viel eher wartet man, bis der Puffer voll ist oder man
   zumindest genügend Daten hat, und schickt dann ein Segment weg. Beim
   Empfänger ist das genau so. Der TCP Stack wird die Anwendung nicht für jeden
   einzelnen Byte stören. Auch hier würden Daten zuerst gepuffert. Hat man aber
   eine interaktive Anwendung, die Daten sofort möchte, dann will man diese
   Puffer nicht. Beispielsweise bei Telnet möchte man lieber, dass jeder Byte
   (also jedes Text-Zeichen) sofort an die Anwendung weitergeleitet wird. Dann
   setzt man also die push flag. Bei einem Socket gibt es hierfür die
   `TCP_NODELAY` Sockopt, welche den Nagle Algorithmus für Buffering deaktiviert
   (https://en.wikipedia.org/wiki/Nagle%27s_algorithm).
   Der Unterschied zwischen urgent und push Flag ist nun der, dass selbst wenn
   ein Segment urgent data enthält, diese dennoch gepuffert werden
   könnten. Meistens würde man also urgent und push Flag für maximal schnellen
   Durchsatz gleichzeitig senden.
 - *RST* (reset): Dient dem Abbruch einer TCP-Verbindung ohne ordnungsgemäßen
   Verbindungsabbau.
 - *SYN* (synchronization): Ist diese Flag gesetzt, handelt es sich bei dem
   Segment um eines, welches zum Verbindungsaufbau dient. Ein gesetztes SYN-Flag
   inkrementiert Sequenz- und Bestätigungsnummer um 1, auch wenn keine Nutzdaten
   geschickt werden. Das SYN Paket hat nämlich selbst auch eine Sequenznummer.
 - *FIN* (finish): Das Gegenstück zu *SYN*, dient also zum
   Verbindungsabbau. Inkrementiert ebenso beide Nummern im Header.
* *Receive Window*: Die vom Empfänger mitgeteilte Größe des Empfangsfensters
  $W_r$ in Byte. Da die Größe des Sendefensters nur maximal so groß wie das
  Fenster vom Empfänger ist, bestimmt dieser Wert also indirekt auch die
  Sendefenstergröße.
* *Checksum*: Eine weitere Checksum, die wie bei UDP wieder einen Pseudo-Header
  verwendet.
* *Urgent Pointer*: Gibt das Ende der urgent Daten im Segment an. Anfangen tun
  die urgent Daten, sofern das Urgent Flag gesetzt ist, immer beim ersten Byte.
* *Zusätzliche Optionen*, z.B. für Window Scaling. Denn das Window Feld ist nur
  16 Bit groß, man könnte also ein maximales Sendefenster von $2^16 - 1$ Byte
  angeben. Wenn man nun mehr Byte möchte, dann kann man diese Zahl einfach
  skalieren. Der Wert in diesem Feld würde also nicht mehr Byte, sondern dann
  vielleicht zwei oder mehr Byte repräsentieren.

http://stackoverflow.com/questions/9153566/difference-between-push-and-urgent-flags-in-tcp
http://www.tcpipguide.com/free/t_TCPPriorityDataTransferUrgentFunction-2.htm

#### MSS

Bei TCP ist immer eine *Maximum Segment Size* (MSS) bekannt. Sie gibt die
maximale Größe der Nutzdaten eines TCP Segments (also ohne TCP
Header). Vergleichsweise gibt die MTU auf Schicht zwei die maximale Größe eines
Ethernet Headers (zum Beispiel) an. In Praxis wird die MSS so gewählt, dass
keine IP-Fragmentierung geschehen muss. Die MTU (minus TCP Header) ist dann also
sozusagen eine obere Schranke für die MSS. Bei FastEthernet ist die MTU
beispielsweise 1500 Byte. Minus 20 Byte für IP Header und 20 Byte für TCP Header
bleiben also 1460 Byte für die MSS.

Die MSS Größe kann im Weiteren dann zum Zwecke der Fluss- und Staukontrolle
angepasst werden. Auch sei angemerkt, dass man auf Schicht 4 die MSS nicht immer
genau so wählen kann, dass keine Fragmentierung passiert. Insbesondere weiß man
auf Schicht 4 nicht, welches Schicht 3 oder 2 Protokoll überhaupt angewandt
wird. Selbst wenn man weiß, dass Ethernet und IP genutzt wird, weiß man dann
auch noch nicht, ob der IP Header vielleicht noch Optionen hat.

#### Flusskontrolle

Da der Empfänger dem Sender die Größe des Empfangsfensters mitteilen kann, kann
der Empfänger auf Über- oder Unterlast auf seiner Seite dynamisch reagieren. Das
nennt man dann *Flusskontrolle*. Da weiter bei TCP nicht Segmente, sondern immer
Byte quittiert werden, bestimmt die Größe des Empfangsfensters also die maximale
Anzahl an Bytes, die der Sender in einem Mal lossenden kann. Insofern bestimmt
die Größe des Empfangsfensters die MSS (als ein Faktor, siehe unten). Das ist
also die maximale Anzahl an Bytes, die der Sender ohne Abwarten einer
Bestätigung wegsenden kann.

Durch Herabsetzen dieses Fensters kann der Empfänger beispielsweise auf einen
droheden Overflow seines Empfangspuffers reagieren, wenn er von einem Sender
schneller Daten erhält, als er sie verarbeiten kann.

#### Staukontrolle

Flusskontrolle reagiert auf Überlast beim Empfänger. *Staukontrolle* ist nun
dazu da, Überlast im Netzwerk, also auf dem gesamten Übertragungspfad, zu
vermeiden. Diese Kontrolle wird nun nicht mehr vom Empfänger über das Receive
Window getätigt, sondern vom Sender, wenn er merkt, dass seine Segmente nicht
mehr so oft bestätigt werden.

Hierfür wird beim Sender nun noch ein *Staufenster*, also *Congestion Window*,
$W_c$ eingeführt. Der Sender versucht dieses Fenster maximal groß und
insbesondere maximal nahe an dem Receive Window zu halten. Daher vergrößert er
das Fenster schrittweise, solange Daten verlustfrei übertragen werden. Treten
Verluste auf (kommen keine Quittierungen mehr), so verringert der Sender das
Fenster wieder.

Das Sendefenster, was also gerade die MSS ist, wird also nicht nur durch das
Empfangfenster nach oben begrenzt, sondern auch durch das Congestion
Window. Somit gilt also:

$$\text{MSS} = W_s = \min{W_c, W_r}$$

Es gibt bei TCP grundsätzlich zwei *Phasen* der Staukontrolle:

1. *Slow-Start*: Hierbei wird Congestion Window bei jedem bestätigten Segment um
   eine MSS vergrößert. Im Fall das das Receive Window größer als das Congestion
   Window ist, gilt also $\text{MSS} = W_s = W_c$. Dann entspricht die Addition
   eiber MSS gerade der Verdoppelung des Congestion Windows. Das führt dann also
   zu exponentiellem Wachstum gemäß $\text{MSS} \cdot 2^i$, wobei $i$ die Anzahl
   bisher bestätigter Segmente ist. Es sei aber angemerkt, dass wenn das
   Empfangsfenster kleiner ist als das Congestion Window, die MSS gleich dem
   Empfangsfenster ist. Dann ist die Addition der MSS keine Verdoppelung
   mehr. In diesem Fall müsste Staukontrolle als ganzes aber auch abgebrochen
   werden müssen. Denn wenn die MSS gleich dem kleineren Empfangfenster ist,
   dann kommen wohl alle Segmente durch. Wenn man gerade Slow Start macht, würde
   man also glauben, das Congestion Window (was eigentlich gerade gar keinen
   interessiert, weil das Empfangsfenster kleiner ist) passt so und kann weiter
   erhöht werden. Dann hätte man also quasi ein Count-To-Infinity Problem.
   Kommt es hierbei jedenfalls dann langsam zu Überlast im Netzwerk, wird mit
   Phase (2) begonnen.
2. *Congestion-Avoidance*: Wird bei Slow-Start nun erkannt, dass es zu einer
   Überlast gekommen ist, wird mit Congestion Avoidance begonenn. Hierbei wird
   das Congestion Window zunächst reduziert (entweder auf die Hälfte oder auf
   die ursprüngliche, kleinste MSS Größe). Dann wird das Congestion Window für
   jedes bestätigte Segment nicht um eine ganze MSS, sondern um $MSS/W_c$
   erhoeht (= 1 ?????). Das Wachstum ist also linear.

Da frühestens nach einer RTT (bei wiederholtem Senden aber manchmal erst nach
mehreren RTTs) eine Bestätigung für ein Segment beim Sender ankommt, ist die
Zeiteinheit auf unserer $x$-Achse in RTTs.

Wann genau mit Slow-Start aufgehört und mit Congestion Avoidance angefangen
wird, und wie der Übergang zu Congestion Avoidance geschieht, ist durch die
konkrete TCP Implementierung bzw. Version bestimmt. Es gibt nämlich mehrere
Versionen von TCP, z.B. TCP Tahoe, Reno, New Reno oder Cubic. Der Linux Kernel
verwendet momentan TCP Cubic, welches das Congestion Window als eine kubische
Funktion mit schnellerem Wachstum modelliert
(http://www4.ncsu.edu/~rhee/export/bitcp/cubic-paper.pdf). Wir behandeln in der
Vorlesung TCP Reno. Es geht auf folgende Weise von Slow Start zu Congestion
Avoidance über:

1. Wenn es drei duplizierte ACKs erhält, also nach dreifach fehlgeschlagenenen
   Senden, reduziert es die Congestion Window Größe auf die Hälfte der
   momentanen Größe. Dann beginne mit der (bei TCP Reno) linearen
   Stauvermeidungsphase.
2. Wenn gar keine ACKs mehr bekommt (also nach einem *Timeout*), dann ist das
   Netzwerk anscheinend vollkommen im Eimer. Dann setzt es also einen
   *Schwellenwert* $W_c/2$ fest. Dann reduziert es das Fenster auf die
   ursprüngliche MSS Größe. Dann beginnt es mit einem neuen Slow-Start, bis es
   den Schwellenwert erreicht, worauf es mit Congestion Avoidance beginnt. Es
   geht also erstmal komplett vom Gas, um das Netzwerk ausruhen zu lassen. Dann
   geht es mit Slow Start schnell auf den Schwellenwert.

#### Anmerkungen

Obwohl TCP eine gesicherte Verbindung ist, dient es nicht der Kompensation eines
unzuverlässigen Physical oder Data Link Layer (Schichten 2,3). Also man kann,
nur weil man mit TCP sicher davon ausgehen kann, dass Pakete irgendwann einmal
durch Retransmits ankommen, nicht einfach auf Fehlerkorrekturmechanismen
verzichten. TCP interpretiert Paketverlust nämlich stets als eine
Überlastsituation im Netzwerk. Dann wird die Datenrate also entsprechend
gedrossel. Wenn der Paketverlust aber auf Grund von Bitfehlern geschieht, dann
ist das eine unnötige Reduktion des Durchsatzes. In Praxis ist TCP bereits mit
1% Paketverlust, der nicht Überlast zurückzuführen ist, überfordert.

## NAT

Network Adress Translation (NAT) ist eine Methode, durch welche lokal, aber
nicht global, eindeutige Adressen auf global eindeutige abgebildet werden
können. Genauer erlaubt NAT eine Übersetzung von *privaten Adressen*,
beispielsweise dem 10.0.0.0/8 Adressraum, auf *öffentliche Adressen*. Die Hosts
innerhalb eines Netzwerkes können dann über ihre privaten Adressen miteinander
kommunizieren. Hierbei kann es in zwei verschiedenen privaten Netzwerken
durchaus diese privaten Adressen mehrmals geben. Um mit Hosts in anderen
Netzwerken zu kommunizieren, werden ihre privaten Adressen dann in global
eindeutige übersetzt.

Formal bezeichnet NAT Techniken, welche es ermöglichen, $N$ private (global
nicht eindeutige) Adressen auf $M$ global eindeutige Adressen
abzubilden. Hierbei wird ferner wie folgt zwischen NAT-Typen unterschieden:

* Ist $N \geq M$, kann die Übersetzung statisch geschehen. Dann hat jeder Knoten
  eine private und öffentliche Adresse. Insofern ist dieser Fall nicht wirklich
  interessant. Er ist aber mit link-local Adressen und öffentlichen IPv6
  Adressen vergleichbar.
* Ist $N > M$, wird eine öffentliche Adresse von mehreren Rechnern gleichzeitg
  verwendet. Eine eindeutige Untesrcheidung kann dann mittels
  *Port-Multiplexing* geschehen. Häufig ist $M$ sogar gleich $1$, z.B. bei
  privaten Haushalten.

### Private Adressen

Zunächst wollen wir wiederholen, was private Adressen überhaupt sind. Private
IP-Adressn sind *spezielle Adressebereiche*, die zur privaten Nutzung ohne
vorherige Registeriung freigegeben sind. Sie können in unterschiedlichen
Netzwerken gleichzeitig genutzt werden und werden nicht geroutet. Die private
Adressbereiche sind:

* 10.0.0.0/8
* 172.16.0.0/18
* 169.254.0.0/16
* 192.168.0.0/16

Der Addressbereich 169.254.0.0/16 wird des Weiteren zur automatischen
Adressvergabe (*Automatic Private IP Adressing*) genutzt. Startet ein Computer
ohne statisch vergebene IP Adresse, so versucht dieser zunächst, einen DHCP
Server (durch eine DHCP Discovery) zu erreichen. Findet sich kein DHCP Server,
generiert das Betriebssystem eine zufällig gewählte Adresse aus diesem
Adressblock. Dann broadcastet es einen ARP-Request, um zu prüfen, ob diese
Adresse schon ein anderer Rechner im Direktnetzwerk hat. Wenn ja, generiert es
eine neue. Das passiert so lange, bis kein ARP Reply mehr kommt.

### NAT im Detail

Im Allgemeinen übernehmen Router die NAT Funktion in einem Netzwerk. Der Router
hat dann eine *NAT-Tabelle*, in welcher er im einfachsten Fall vier Spalten
hält:

1. Die private Queladresse eines Rechners.
2. Der private Quellport eines Rechners.
3. Der globale Port des Routers.
4. Das verwendete Protokoll.

Hierbei nehmen wir an, dass der Router nur eine globale Adresse hat ($M =
1$). Will ein Rechner im lokalen Link nun ein Paket an eine globale Adresse
außerhalb des Netzwerkes senden, so sendet er es an den Router (seinen
Gateway). Wenn das das erste Paket von diesem Rechner auf diesem Port ist,
erstellt der Rechner einen neuen Eintrag in der Tabelle. Er vermerkt darin dann
zunächst private Adresse und Port des Quellrechners. Dann gilt es, einen
globalen Port des Routers für diese $(\text{Quelladresse, Quellport})$
Kombination zu finden. Hierzu prüft der Router zunächst, ob der Quellport schon
als globaler Port vergeben wurde. Wenn nicht, übernimmt es den private Quellport
einfach als globalen Port und gibt ihn in die Tabelle. Ist der Port schon
vergeben, generiert es einfach einen zufälligen Port. Würde also ein weiterer
lokaler Rechner auf diesem selben Port kommunizieren wollen, würde er eben einen
solchen zufällig gewählten globalen Port zugewiesen bekommen.

Nachdem der Router diesen Eintrag generiert hat (oder nicht, falls der Eintrag
schon existierte), geschieht die eigentliche Adressübersetzung. Der Router
ersetzt nun die Quell-IP Adresse des privaten Rechners im Paket, durch die
globale Adresse des Routers. Ebenso wird im UDP/TCP Segment der private Port
durch den zugewiesenen globalen Port ersetzt. Dann schickt der Router das Paket
into se world wide webs.

Wenn nun aus se world wide webs eine Antwort auf dieses Paket kommen soll, wird
dies also nicht an den Rechner adressiert sein, sondern an den *Router*. Die
Zieladresse wird also die globale Adresse des Routers sein und ebenso wird der
Port der gewählte globale Port des Routers sein. Dann muss der Router also die
Adressen zurückübersetzten. Hierzu sieht der Router in seiner Tabelle unter dem
globalen Port nach. Dort findet er private Adresse und Port des entsprechenden
lokalen Rechners, durch welche er das IP Paket modifiziert. Dann schickt er das
Paket in den lokalen Link an den privaten Rechner.

Während des ganzen Prozesses wissen also weder der lokale Quellrechner noch der
Zielrechner, dass sie eigentlich mit dem Router kommunizieren. Der lokale
Rechner sendet das Paket ja einfach an seinen Gateway, im Glauben, dass dieser
das Paket einfach weiterleiten wird. Der Zielrechner hingegen glaubt, dass er
eigentlich mit dem Router kommuniziert. Er weiß vom lokalen Rechner gar nichts!
Wenn zwei lokale Rechner mit demselben Zielrechner kommunizieren, glaubt der
Zielrechner, dass der Router einfach zwei Verbindungen aufgebaut hat.

### NAT Typen

Die Art von NAT, die wir gerade besprochen haben, nennt sich *Full Cone NAT* und
ist eigentlich die einfachste Art von NAT. Andere Arten von NAT unterscheiden
sich in den Informationen die sie in eine Tabelle aufnehmen, sowie in ihrer
Restriktivität. Router könnten nämlich zusätzlich noch weitere Informationen in
ihre Tabelle aufnehmen, z.B.:

* Ziel-IP
* Ziel-Port
* Die globale IP des Routers, falls er mehrere hat.

Im Weiteren ist die Restriktivität der Übersetzung also ein
Unterscheidungsfaktor. Was meinen wir damit? Nun ja, Full Cone NAT hat folgende
Eigenschaften:

* Bei eingehenden Verbindungen findet keine Prüfung der Absender-IP oder des
  Absender-Ports statt. Der Router speichert auch keinerlei solche Informationen
  in seiner Tabelle.
* Existiert einmal ein Eintrag für einen Rechner in der NAT-Tabelle, so kann
  jeder externer Host mit diesem privaten Rechner über den entsprechenden Port
  kommunizieren.

Das machen andere Arten von NAT anders:

* *Restricted NAT* leitet nur dann ein Paket an einen internen Host weiter, wenn
  der interne Host zuvor schon ein Paket an die Quell-IP-Adresse gesendet
  hat. Der Port wird dabei nicht betrachtet.
* *Port-Restricted NAT* erhöht die Restriktivität der Weiterleitung noch
  dadurch, dass nur Pakete weitergeleitet werden, wo der interne Host schon ein
  Paket an diese Quell-IP-Adresse und *an diesen Quell-Port* des eingehenden
  Pakets gesendet hat.

http://www.think-like-a-computer.com/2011/09/16/types-of-nat/

Im Weiteren ist es so, dass ein Router einen Eintrag in seiner NAT Tabelle nach
einiger Zeit wieder löscht, um den Port wieder freizugeben. Denn ansonsten ist
das Maximum an Verbindungen eben $2^16$ pro globaler IP Adresse. Oft entfernt
ein Router einen Eintrag auch dann sofort, wenn er einen TCP Verbindungsabbau
erkennt.

Man kann Einträge in NAT Tabellen auch manuell erstellen, um z.B. einen Server
hinter einem NAT-Router auf einem globalen Port erreichbar zu machen. Das nennt
man dann *Port Forwarding*. Sonst müsste der Server zuerst von dem Port, wo er
eigentlich Daten erhalten möchte (z.B. HTTP Port 80), ein Paket an irgendeinen
Rechner senden, damit ein Eintrag generiert wird. Adress- und
Port-Restriktivität würde dann natürlich auch Probleme machen.

### NAT ohne Schicht 4

Natürlich kann man nur Ports in eine Tabelle eintragen, wenn es Ports
gibt. Nicht alle IP Payloads sind ja TCP oder UDP Segmente. Zum Beispiel könnte
ein IP Paket eine ICMP Nachricht enthalten. Dies haben keinen Port. Was tun? Bei
Echo Requests und Replies ist das weniger problematisch, da diese eine
*Identifier* Nummer in ihrem Header haben. Diese wird dann einfach an Stelle
des Ports in die Tabelle eingetragen.

### NAT bei IPv6

NAT kann auch bei IPv6 angewendet werden. Das ist weniger häufig nötig, da man
bei IPv6 das Problem der Adressknappheit nicht hat. IPv6 hat aber ebenso das
Konzept von privaten Adressen. Diese heißen bei IPv6 dann
*Unique-Local-Unicast-Adressen* und haben den Präfix `fc00::/7`. Die
NAT-Übersetzung benutzt bei IPv6-NAT aber keinerlei Merkmale aus Schicht 4. Es
werden nämlich einfach Präfixe übersetzt. Z.B. werden alle lokalen Adressen mit
Präfix `fd01:0203:0405::/48` auf den globalen Präfix `2001:db8:0001::/48`
abgebildet. Erhält ein Router also ein Paket, dass an eine Adresse mit diesem
globalen Präfix adressiert ist, ersetzt er einfach den Präfix. Anders als bei
IPv4 passiert bei IPv6 NAT also nie Port Multiplexing. Wir haben ja für $N$
lokale Adressen ebenso viele globale Adressen bei dieser
Präfixtauschmethode. Insofern lösen wir das Problem der Adressknappheit
nicht. Das ist bei IPv6 aber eben auch nicht nötig.
