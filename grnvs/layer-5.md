# OSI Model: Layer 5

Der hauptsaechliche Dienst, den die Sitzungsschicht bringt, sind *Sitzungen*
(Sessions). Sessions auf Schicht 5 haben aehnliche moegliche Eigenschaften wie
Verbindungen auf Schicht 4 (Transportschicht), aber eben auf einer hoeheren
Ebene. Insbesondere koennen Sitzungen auf Schicht 5 *mehrere* Verbindungen
unterer Schichten haben. Ein Beispiel dafuer ist das FTP Protokoll. Dieses ist
als Anwendung eigentlich auf Schicht 7, aber es hat auch Sitzungen zwischen
einem Server und einem Client. Waehrend einer solchen Sitzung werden dann *zwei*
TCP Verbindungen aufgebaut und erhalten. Eine als Kontrollkanal, der andere als
Datenkanal. Insofern gibt es also *eine Verbindung (Sitzung)* auf Schicht 5, die
mehrere Verbindungen auf Schicht 4 beinhalten kann.

Es gibt hierbei zwei grundlegende Arten von Sitzungen:

1. *Verbindungsorientierte* Sitzungen sind solche, wo die Verbindung zwischen
   Empfaenger und Sender ueber mehrere Datenuebertragungen, aber auch ueber
   mehrere Verbindungen niedrigerer Schichten gehen kann. Beispielsweise kann es
   in einer solchen verbindungsorientierten Sitzung mehrere TCP Verbindungen
   geben, die nacheinander auf- und wieder abgebaut werden. Wie auf der
   Transportschicht gibt es auch hier das Konzept von *Verbindungsaufbau*,
   *Datentransfer* und *Verbindungsabbau*.

2. *Verbindungslose* Sitzungen haben keinen Zustand. Beispielsweise kann ein
   HTTP Request (*ohne Cookies*) eine verbindunglose Sitzung sein, wo also nur
   eine einmalige Verbindung auf der Transportschicht initiiert wird, aber nach
   diesem Request sofort wieder abgebaut wird. Wuerde die Verbindung auch noch
   fuer den Reply offen gehalten werden, so waere es
   verbindungsorientiert. Wichtig hierbei ist wiederum, dass es auch fuer
   verbindungslose Sitzungen auf Schicht 5 verbindungsorientierte Verbindungen
   auf Schicht 4 geben kann. Auch wenn der HTTP Request alleine steht, kann er
   eine verbindungsorientierte TCP Verbindung nutzen, sodass Pakete gesichert
   uebertragen werden. So kann auch eine E-Mail ueber eine verbindungslose
   Sitzung einmalig gesendet werden, wobei verbindungsorientierte
   Transportprotokolle wie TCP verwendet werden.

Wichtig ist auch, dass eine Sitzung nicht nur zwei Teilnehmer haben muss,
sondern *auch mehr* haben kann. Das laesst sich leicht durch eine
Video-Konferenz mit vielen Teilnehmern verstehen. Das ist beispielsweise eine
Eigenschaft, wo es fuer die Transportschicht keine Parallele gibt.

Verbindungsorientierte Sitzungen koennen im Weiteren folgende Dienste anbieten:

* *Aufbau* und *Abbau* von Sessions,
* Normaler und beschleunigter Datentransfer (*Expedited Data Transfer*,
  z.B. fuer Interrupts),
* *Token-Management* zur Koordination der Teilnehmer (wenn es mehr als zwei gibt),
* Synchronisation und Resynchronisation,
* *Fehlermeldungen* und Aktivitaetsmanagement,
* Erhaltung und Wiederaufnahme von Sessions nach Verbindungsabbruechen (welche
  eine Sitzung ja nicht unterbrechen sollten).

### Transport Layer Security (TLS)

Transport Layer Security (TLS) ist ein Schicht 5/6 Protokoll, dass zur
Verschluesselung von Daten dient. Es ist der Nachfolger von SSL (Secure Sockets
Layer). Es bildet beispielsweise die *Grundlage fuer HTTP__S__* und bietet unter
anderem:

* Authentifizierung: Die beiden Parteien geben ihre Identitaet via Public-Key
  Kryptographieverfahren einander bekannt, um sich zu authentifizieren. Dann
  werden Algorithmus und Schluessel ueber einen privaten Kanal
  ausgetauscht. Insofern ist TLS kein *Verschluesselungsverfahren* sondern nur
  ein *Verschluesselungsprotokoll*, das z.B. AES als Verschluesselungsverfahren
  verwendet. Hierbei wird also sichergestellt, das Verbindungen ueberhaupt erst
  zwischen Teilnehmern erstellt werden, wo das erlaubt ist.
* Integritaetsschutz: Es wird sichergestellt, dass Daten *ohne Veraenderung*
  zwischen Empfaenger und Sender ausgetauscht werden. Das ist nun also eine
  Sicherheit, die angeboten wird, nachdem die Teilnehmer authentifiziert
  wurden.
* Verschluesselung: stellt sicher, dass Daten nicht von Augen gesehen werden
  koennen, die es nicht sehen sollen.

Eine TLS verschluesselte Sitzung kann dann natuerlich wieder viele Verbindungen
der Transportschicht beinhalten. Die Verschlueseslung selbst faellt dabei eher
in den Bereich der Darstellungsschicht (Schicht 6).
