# OSI Model: Layer 7

Die Anwendungsschicht ist die hoechste und letzte Schicht im Open Systems
Interconnect (OSI) Modell (se final layer, tadadada, tadadadada). Auf dieser
Ebene beschaeftigen wir uns mit vielen Protokollen, mit denen wir als
Internetnutzer oftmals direkt interagieren. Beispiele hierfuer sind:

* Domain Name System (*DNS*)
* HyperText Transfer Protokol (*HTTP*)
* File Transfer Protocol (*FTP*)
* Simple Mail Transfer Protocol (*SMTP*)
* Post Office Protocol / Internet Message Access Protocol (*POP/IMAP*)
* *Telnet*
* Secure Shell (*SSH*)

Wir wollen die meisten dieser Protokolle nun naeher betrachten.

## DNS

Das *Domain Name System* (*DNS*) ist dazu da, menschenlesbare Website
Domain-Namen in IP-Adressen umzuwandeln. Menschen ist es natuerlich (meist) viel
lieber, einfach `www.google.com` einzugeben, als die IP Adresse eines Google
Servers (`1e100.com`). Mittels DNS kann man bestimmte Knoten im Netzwerk
ansprechen, um die IP Adresse hinter einem Domain Namen zu erhalten. Denn nur
ueber IP Adressen kann man auch Daten (via IP Paketen) senden.

Das Domain Name System besteht aus drei wesentlichen Komponenten:

1. Dem *Domain Namespace*. Das ist ein hierarchisch aufgebauter Namensraum mit
   baumartiger Struktur, *aus* welchem man die Abbildung von Domain Namen auf IP
   Adressen erhalten kann.
2. *Nameservern*: Diese speichern Informationen ueber den Namensraum, wobei
   jeder Server nur einen kleinen Ausschnitt (Teilbaum) der ganzen Abbildung
   kennt.
3. *Resolver* sind Programme, die via *DNS Requests* die IP Adressen von
   Nameservern im Domain Namespace extrahieren. Will man als Nutzer einen Domain
   Namen in eine IP Adresse umwandeln, so wird man (bzw. der Browser) ueber den
   Resolver agieren.

### Namespace

Ein kleiner Ausschnitt aus dem Domain Namespace sieht in etwa so aus:

```
          ___ . ____
         /          \
        /            \
       /              \
     com              org
    /   \            /   \
   /     \          /     \
google reddit  wikipedia google
  |                |       |
 www              www     www
```

In diesem Namespace Baum nennt man jeden Knoten ein *Label*. Ein *Domain Name*
ist dann eine Sequenz solcher Labels. Hierbei unterscheiden wir noch zwischen
*Fully Qualified Domain Names* (*FQDN*) und *relativen Domain Names*. Ein FQDN
ist dabei eine Sequenz von Labels, die __von einem Knoten bis zur *Wurzel*
geht__. Man bemerke, dass das Label in der Wurzel des Namespaces ein *Punkt*
ist. Insofern ist `www.google.com` noch kein FQDN, sondern erst
`www.google.com.`. Alle anderen Domain Namen sind naemlich relativ zu einem
anderen. Beispielsweise ist `hangouts.google` relativ zum FQDN
`hangouts.google.com.` (man beachte den hinteren Punkt!). Wir wissen anhand
eines Domain Namen im Uebrigen noch nicht, ob dieser ueberhaupt valide ist und
in eine Adresse umgewandelt werden kann. So ist `asdfaspfihaeorugofadsdaf.com.`
zwar ein syntaktisch gueltiger FQDN, er kann aber nicht auf eine IP Adresse
abgebildet werden.

Hierbei befolgen Labels folgende Syntax:

* Es sind nur Buchstaben, Zahlen und Bindestriche erlaubt.
* Der Bindestrich darf hierbei nicht das erste oder letzte Zeichen sein.
* Zwischen Gross- und Kleinschreibung wird nicht unterschieden.

Fuer die ersten drei Ebenen im Namespace Baum haben wir eigene Bezeichnungen:

1. Ganz oben ist die *Root Ebene*, die nur aus dem `.` Label besteht.
2. Darunter kommt die *Top Level Domain* (*TLD*) Zone. Sie beinhaltet Labels wie
   `.net`, `.com` oder `.org`.
3. In der dritten Ebene befinden sich die *Second Level Domains* (*SLD*) wie
   `google`, `reddit` oder `facebook`.
4. (Inoffiziell) folgen nach den Second Level Domains noch die *Subdomains*,
   z.B. `plus`(`google.com.`) sowie auch `www`.

Genauer gesagt ist das Label der Root Zone nicht der `.`, sondern der *empty
string*! Der `.` ist lediglich die Trennung zwischen zwei Labels, also bei
`www.google.com.` die Trennung zwischen `www.google.com` und dem emtpy label.

### Nameserver

Der Domain Namespace kann in gewisserweise als eine grosse, verteilte Datenbank
betrachtet werden. Die einzelnen Server, auf welchen die Daten (also Labels
bzw. Domain Namen) des Namespaces gespeichert werden, nennt man hierbei
*Nameserver*. Jeder *Nameserver* kennt dabei nur einen kleinen Teil des gesamten
Raumes. Genauer kennt er nur eine *Zone*. Hierbei bezeichnen wir mit einer Zone
einen zusammenhaengenden *Teilbaum* des ganzen Baumes, welcher viele Knoten auf
vielen Ebenen *unter* sich enthalten kann. Jeder Knoten ist hierbei natuerlich
wieder ein Label.

Ein Nameserver kennt immer nur eine Zone. Man nennt diesen Server dann
*authoritativ* fuer diese Zone. Wenn er authoritativ ist, muss er alles ueber
seine Zone wissen, oder zumindest wissen, wie er an Informationen ueber seine
Zone kommt (also wie er andere Nameserver in seiner Zone anfragen kann). Es muss
aber nicht nur einen authoritativen Nameserver fuer eine Zone geben, sondern es
kann auch mehrere geben. Hierbei gibt es aber immer einen *primaeren*
authoritativen Server und alle anderen werden dann *sekundaer* genannt. Der Sinn
hinter dieser Redundanz ist es, dem primaeren Nameserver bei Ueberlast zu
unterstuetzen sowie ihn bei dessen Ausfall (temporaer) zu ersetzen. Die
Aufteilung *primaer* und *sekundaer* ist zusaetzlich auch dazu da, um
Aenderungen an den Informationen eines Nameservers zu koordinieren. Will man
einen Nameserver updaten, so fuehrt man diese Updates immer am primaeren
Nameserver einer Zone aus. Die sekundaeren sind dann lediglich dazu da bzw. dazu
befugt, Kopien der Informationen des primaeren Servers zu speichern. Auch ist es
moeglich, die Verantwortung ueber Zonen zwischen Nameservern auszutauschen. Das
nennt man dann *Zone Transfers*.

Hierbei gelten die Konventionen:
* Anfragen, die 512 Byte nicht uebersteigen, werden an UDP Port 53 gestellt.
* Anfragen, die groesser als 512 Byte sind, werden an TCP Port 53 gestellt.
* Zone Transfers finden immer ueber TCP Port 53 statt.

Es gibt 13 logische Nameserver (logisch im Sinne dessen, dass das nur Reverse
Proxies sind) fuer die Root Zone (empty label). Diese kann man hier finden:
http://www.root-servers.org und haben auch den `root-servers.org` Suffix
(z.B. `a.root-servers.org`, welcher von Verisign operiert wird). Die root zone
file ist 1 MB gross (enthaelt die ganzen
TLDs).

https://en.wikipedia.org/wiki/Root_name_server
http://www.internic.net/domain/root.zone

### Zone Files

Informationen ueber eine Zone werden in *Zone Files* auf einem authoritativen
Nameserver gespeichert. Ein Zone File enthaelt hierbei eine Anzahl an *Resource
Records* (RR), welche fuer die Art von Resource Record spezifische Informationen
ueber die Zone angeben. Zone Files werden in Plain Text gespeichert. Resource
Records haben dabei das folgende (kanonische) Format in einer Zone File:

```
name ttl record-class record-type record-data
```

Hierbei sind je zwei Spalten (Attribute) durch Whitespace (Tab oder Leerzeichen)
getrennt. Auf einer Zeile darf es dann immer nur einen RR geben. Zu den
einzelnen Komponenten:

* `name`:
  - Hier gibt man den Domain Namen an, fuer welchen der RR etwas aussagen soll.
  - Laesst man ihn weg, so bezieht sich der Eintrag implizit auf den FQDN des
    vorherigen Eintrags in der Zone File.
  - Ist es kein FQDN, sondern nur ein relativer Domain Name, so ist er relativ
   zum `$ORIGIN`. Man kann naemlich am Anfang (bzw. irgendwo vor einem
   entsprechenden Eintrag) in der Datei einen solchen `$ORIGIN` Marker
   festlegen. Beispielsweise `$ORIGIN example.com.`. Dann beziehen sich also
   alle `name`s, die nicht mit einem Punkt enden (FQDNs sind), auf den
   `$ORIGIN`. `foobar` wuerde dann z.B. `foobar.example.com.` meinen. `@`
   bezeichnet dann auch immer den in der Variable `$ORIGIN` gehaltenen
   Namen. Somit kann man z.B. einen Resource Record fuer `@`, also den
   `$ORIGIN`, spezifizieren.

* `ttl`:
  - Hier kann man eine *Time to Live* Zeit in Sekunden oder, seit neueren BIND
    Versionen (das ist die Software, mit welcher DNS auf Servern implementiert
    wird), auch in anderen Zeiteinheiten angeben. Diese Zeit gibt an, wie lange
    DNS *Clients*, also insbesondere Resolver, einen Eintrag cachen duerfen. Er
    sagt also insofern nichts darueber aus, wie lange der Eintrag fuer den
    *Server* gueltig ist -- nur fuer Resolver (oder allgemein Clients, die
    direkt Anfragen an den Server machen). Alternativ kann man einen globalen
    `$TTL` Eintrag haben (das ist wie `$ORIGIN` oder `$INCLUDE` eine Direktive)
    und da einfach die Zeit angeben. Dann wird fuer jeden RR, der keinen TTL
    explizit angibt, die globale TTL als Default genommen.

* `record class`:
  - Gibt den *Namespace* des RR an. Das ist heutzutage immer `IN`, also das
    Internet. DNS gibt es naemlich schon seit den 1980ern, wo man noch an
    anderen Namespaces neben dem Internet, z.B. dem CHAOSNet
    (https://en.wikipedia.org/wiki/Chaosnet), gearbeitet hat.

* `record type`:
  - Record types sind neben dem FQDN der wichtigste Eintrag. Es gibt an, welche
    Art von Information mit dem FQDN assoziiert werden soll.
  - *SOA* (*Start of Authority*) records enthalten Metainformationen ueber die
	Zone, fuer die ein Nameserver authoritativ ist (jeder Nameserver ist fuer
	irgendeine Zone authoritativ). Sie sind insbesondere fuer sekundaere
	Nameserver interessant.
  - *NS* Records geben den *FQDN* eines Nameservers an, welche auch in anderen
	Zonen liegen koennen. Nach [RFC 1034](https://tools.ietf.org/html/rfc1034)
	muss jeder Nameserver mindestens zwei NS Eintraege fuer seine Zone (dem
	Domain Namen, fuer welchen er authoritativ ist) haben, die nicht zur selben
	IP Adresse resolvieren. Der erste Eintrag kann dabei der Nameserver selbst
	sein, der zweite muss (zumindest beim primaeren Nameserver) auf einen
	anderen (*sekundaeren*) Nameserver zeigen. Das verlangt die IANA bzw. der
	RFC so, einfach um die Reliability von Websiten (bzw. Domains) im Internet
	zu erhoehen.
  - *A* Records: assoziieren einen FQDN aus der Zone, fuer welche der Nameserver
	authoritativ ist, mit einer bekannten *IPv4* Adresse.
  - *AAAA* (*quad-A*) Records: wie *A* records, aber fuer *IPv6* Adressen.
  - *CNAME* (*canonical name*) Records sagen aus, dass der FQDN nur ein Alias
    ist, und dass sein kanoonischer (echter) Name der angegebene ist (in
    `data`). Der Resolver soll also bitteschoen eine neue Anfrage fuer den
    kanonischen Namen machen.
  - *MX* Records: geben den FQDN eines *Mailservers* (Mail Transfer Agent; MTA)
    fuer eine Domain an.
  - *TXT* Records: assoziieren einen FQDN mit einem String (Text).
  - *PTR* Records: diese werden fuer *Reverse-DNS* verwendet, wobei *IP Adressen
    auf Domain Namen* abgebildet werden. Ein PTR Record speichert also fuer eine
    IP Adresse einen entsprechenden Domain Namen. Solche Eintrage haben
    eigentlich nur die `in-addr.arpa` server.

#### Start of Authority (SOA) Records

*Jeder Nameserver* muss einen Start of Authority (SOA) Eintrag haben. Dieser
enthaelt wichtige Metainformationen zur Zone File, die vor allem fuer sekundaere
(Slave) Nameserver wichtig sind. Ein SOA Record enthaelt:

1. Alles was auch jeder andere Eintrag braucht: `FQDN` des Servers; TTL, falls
   kein globaler gesetzt oder gewuenscht ist; record class (`IN`) und record
   type `SOA`. Erst `data` ist interessant.
2. `MNAME`: Primary (*Master*) *Name Server*. Das ist der FQDN des Primary (Master)
   Name Server fuer diese Zone. Es darf immer nur einen authoritativen Master
   Name Server. Im Falle des Master Servers ist das der FQDN des Servers selbst
   (bzw. der Zone, fuer welche er authoritativ ist). Fuer sekundaere (Slave)
   Server steht hier aber der FQDN des Master Servers.
3. `RNAME`: *Responsible Person*. Hier steht die Email Adresse des
   Administrators fuer diese Zone. Die Email Adresse muss hierbei das Format
   `username.domain.` haben, wobei man normal `username@domain` schreiben
   wuerde.
4. `SERIAL`: Gibt einen monoton ansteigenden Veraenderungscount der Zone File
   an. Jedes Mal wenn man am Primary Server was aendert, sollte die Serial
   inkrementiert bzw. erhoeht werden. Sekundaere Server werden dann ab und an
   (siehe `REFRESH` unten) beim Master nachfragen, ob er denn Veraenderungen
   hat, die die sekundaeren Server auch haben sollten. Das erkennen sie daran,
   dass ihr eigener `SERIAL` geringer als der `SERIAL` des Masters waere. Dann
   wuerden sie also ein *Zone Transfer Update* initialisieren. Oftmals gibt man
   diesen Wert auch als Datum `YYYYMMDDNN` an, wo `NN` noch ein Count fuer den
   Tag ist, falls die Zone File oefter als einmal an einem Tag geandert wird.
5. `REFRESH`: Sagt aus, in welchem Zeitintervall sekundaere Nameserver bei ihrem
   Master nach Updates sehen sollen. Dieser Wert sollte nicht zu klein sein, um
   den Primary Server nicht zu ueberschwemmen. Er sollte aber auch nicht zu
   gross sein, um schnell Konvergenz zu erreichen. Typische Werte liegen
   zwischen einem Tag und 30 Minuten.
6. `RETRY`: Wie lange Slave Server nach einer gescheiterten Update-Anfrage
   warten sollen, bevor sie eine neue Anfrage an den Master machen. Es kann
   naemlich zum Beispiel sein, dass der Master gerade einfach zu beschaeftigt
   war (oder tot). Das ganze waere also, nachdem die `REFRESH` Zeit verstrichen
   ist. Typische Werte liegen zwischen einer Stunde und 10 Minuten.
7. `EXPIRE`: Wie lange es dauern soll, bevor ein sekundaerer Nameserver sich
   selbst invalidieren soll, wenn er keine Updates von seinem Master erhalten
   kann. Das waere beispielsweise der Fall, wenn der Master Server gerade down
   ist. Es ist wichtig, diesen Wert relativ hoch zu halten, damit eine Website
   auch noch laenger erreichbar ist, wenn der Primary Server stirbt. Typische
   Werte sind zwischen vier Wochen und einer Woche.
8. `MINIMUM`: Dieser Wert hat viele Interpretationen. Die Offizielle ist aber
   (nach neuestem RFC), dass dieser Wert angibt, wie lange *negative* Antworten
   dieses Servers von Resolvern gecached werden sollen. Also falls ein Resolver
   eine IP Adresse fuer eine Domain haben moechte, fuer welche der Nameserver
   authoritativ ist und somit die Antwort wissen muss, *falls es ein bekannter
   bzw. existenter FQDN ist*, der Nameserver aber keine Antwort weiss
   (bzw. seine delegierten Server), dann soll der Resolver sich so lange
   speichern, dass es diesen Eintrag *nicht gibt*. Laut RFC 2308 ist der
   maximale Wert hierfuer drei Stunden.

http://www.peerwisdom.org/2013/05/15/dns-understanding-the-soa-record/
http://www.zytrax.com/books/dns/ch8/soa.html

#### Quest for "How to Internet": Registrars

Was macht ein Registar? Wie funktioniert das, dass mein Namecheap Registrar
`.me` Adressen (Montenegro) speichern darf. Antwort: Der Registrar darf Domains
aus der `.me` Zone verkaufen, muss dann aber an die montenegrinische
Internetbehoerde Geld zahlen. Die montenegrinischen Server werden dann fuer den
`goldsborough.me.` einen `NS` Eintrag auf einen Nameserver von Namecheap
speichern, sodass jeglicher Traffic auf meine Website dorthin geforwarded wird.

http://stackoverflow.com/questions/2235607/how-does-domain-registration-work/2235644#2235644

### Resolver

Resolver sind Knoten im Netzwerk, die fuer einen Client Informationen aus dem
DNS extrahieren. Da das DNS eine Art verteilte Datenbank ist und kein einzelner
Nameserver alle Zonen kennt, sind in der Regel mehrere Anfragen vom Resolver
notwendig.

Moechte ein Client einen Domain Namen zu einer IP Adresse aufloesen, so macht
der Client eine *rekursive Anfrage*. Das bedeutet, dass er die volle,
letzendliche Antwort will, also die IP Adresse. Kann der Resolver die
Information nicht genau bestimmen, sondern gar nicht (der Domain Name ist
ungueltig) oder nur teilweise (nur den naechstbesten Nameserver), so soll der
Resolver eine Fehlermeldung an den Client (Computer) zurueckgeben.

Um die rekursive Anfrage zu erfuellen macht der Resolver selbst *iterative*
Anfragen an authoritative Nameserver. *Iterativ* meint, dass wenn der Resolver
einen solchen DNS Request macht, es auch OK ist, wenn er nur teilweise eine
Antwort erhaelt. Beispielsweise wenn der Resolver einen authoritativen
Nameserver der `com.` Zone anfragt, um auf `hangouts.google.com..` zu kommen,
und der `com.` Server ihm nur den DNS Nameserver von `google.com.`
zurueckgibt. Dann ist der Resolver mit dieser unvollstaendigen Antwort (im Sinne
dessen, dass es noch nicht die gewuenschte aufgeloeste IP Adresse ist) erstmal
zufrieden, und fragt dann *iterativ* den erhaltenen Nameserver von `google.com.`
an. Dieser weiss dann hoffentlich die IP Adresse von `hangouts.google.com` oder
einen Nameserver, den man diesbezueglich weiter *iterativ* anfragen kann.

Sobald der Resolver die Adresse hat, kann er sie *cachen* und dan den Client
zurueckgeben. Bei der naechsten Anfrage des Clients nach `hangouts.google.com.`
kann der Resolver die Antwort sogleich, ohne weitere Anfragen an Nameserver,
zurueckgeben, sofern die `TTL` des Eintrags noch gueltig (groesser 0) ist. Diese
TTL weiss der Resolver aus dem Resource Record, den er letztendlich vom
Nameserver erhalten hat, der fuer `hangouts.google.com.` authoritativ war.

Damit ein Resolver weiss, wo er ueberhaupt mit seinen Anfragen anfangen soll,
hat er sogenannte *Root Hints*. Das ist eine statische Liste der 13 *logischen*
Root Nameserver. Diese Server sind logisch, weil sich dahinter eigentlich
hunderte, per *anycast* erreichbare, Server befinden. Sie sind also nur *Reverse
Proxies*. Diese werden von verschiedenen Institutionen, z.B. der ICANN, NASA,
U.S. Army und anderen betrieben. Es gibt genau 13 Server, damit man alle 13
Adressen (32-Bit IP Adressen) in ein Datagramm unter 512 Byte geben kann. Das
ist naemlich was von der MTU nach allen weiteren Daten fuer eine solche
Nachricht uebrig bleibt (oder fuer eine 512 Byte UDP Nachricht?). Denn da diese
Informationen oft angefragt werden, ist es ratsam, IP Fragmentierung zu
vermeiden.

Damit ein Client im Uebrigen weiss, was sein Resolver ist, muss er einen
entsprechenden Verweis auf einen Resolver in einer lokalen Datei `resolver.conf`
speichern. Oftmals haben Router Resolverfunktionen. Man kann aber auch den
Google Resolver bei `8.8.8.8` als Resolver benutzen (damit Google seinen Traffic
tracken kann). Oft benutzt man auch einen Resolver seines ISPs. Meist
funktioniert das dann so, dass man via DHCP mit jedem IP Lease auch einen
entsprechenden Resolver zugeordnet bekommt.

https://technet.microsoft.com/en-us/library/cc961401.aspx

### Reverse DNS

Via dem DNS kann man Domain Namen zu IP Adressen umwandeln. Es ist aber auch
moeglich, den umgekehrten Weg zu gehen und IP Adressen auf Domain Namen zu
mappen. Das nennt man dann *Reverse DNS* und wird durch *PTR* (Pointer) Records
in Zone Files ermoeglicht. Hierfuer gibt es zwei eigene Zonen im DNS:

* `in-addr.arpa` fuer IPv4
* `ip6.arpa` fuer IPv6

Der `in-addr.arpa` Baum ist ein 4-stufiger Trie mit Fanout fuer je einem Oktett,
also 8 der 32 IPv4 Adressbits, bzw. 256 Kindern pro Knoten. Fuer IPv6 gibt es
einen aehnlichen Baum, wobei auch nach 4 Bit Grenzen (einer Hex Ziffer)
unterschieden wird. Man hat dann also 32 Ebenen anstelle von nur vier.

Man kann sich die entsprechende Zone File hier ansehen:
https://www.internic.net/domain/in-addr.arpa

Man sieht in dieser Zone File, dass der Nameserver `in-addr.arpa.` eigentlich
nur 256 `NS` Eintraege hat, fuer je ein Klasse A Netzwerk. Z.B. gibt es einen
Eintrag

`175.in-addr.arpa.  86400  IN  NS  ns1.apnic.net.`

Hier steht also, dass Resolver ihre Reverse-Lookups fuer `175.xxxx.xxxx.xxxx`
Adressen beim `ns1.apnic.net` Nameserver machen sollen. Dieser haette dann wohl
weitere `NS` Eintraege und irgendwann hat ein Server dann letzendlich die `PTR`
Eintraege, die eine IP Adresse mit einem FQDN assoziieren.

### Beispiel

Ich greife auf `goldsborough.me` zu, was passiert? (Hypothese)

1. Die Anfrage geht ueber UDP Port 53 an meinen Resolver, den ich z.B. via DHCP
   bekommen habe.
2. Der Resolver macht eine erste Anfrage bei einem der 13 logischen Root
   Nameservern (`[a-m].root-servers.org`). Einer der Root Server antwortet ihm
   mit dem FQDN des Nameservers der montenigrinischen Internet-Behoerde.
3. Der Namecheap Registrar einen Vertrag mit dieser Internet-Behoerde
   geschlossen, sodass der Registar `.me` Domains verkaufen kann. Verwalten muss
   diese Domains aber Namecheap. In der Zone File der authoritativen `.me.`
   Nameserver steht also ein `NS` Resource Record fuer `goldsborough.me.`, wo
   der FQDN eines Nameservers von Namecheap steht.
4. Da `goldsborough.me` nur ein Alias fuer `goldsborough.github.io` ist, hat der
   Namecheap NS lediglich einen `CNAME` Eintrag fuer meine Domain.
5. Erst der NS von `github.io.` hat einen `A` und womoeglich `AAAA` Eintrag fuer
   `goldsborough` (relativ zur `$ORIGIN github.io.` Direktive).

### Das Internet

https://www.internic.net/domain/

## URLs

Mit FQDN koennen wir Rechner global ansprechen bzw. deren Schicht 3 (IP)
Adressen herausfinden. Es gibt uns aber noch nicht die Moeglichkeit, eine
bestimmte Resource, z.B. eine HTML Datei, zu adressieren. Auch gibt der FQDN
alleine noch nicht an, auf welche Weise, also ueber welches Schicht 7 Protokoll,
der FQDN bzw. dann die Ressource geholt werden soll. Hierfuer nutzen wir
*Uniform Resource Locator*s (*URL*s). Diese haben die allgemeine Form:

```
<protocol>://[<username>[:<password]@]<fqdn>[:<port>][/<path>][?<query>][#<fragment>]
```

Hier haben wir also:

* *protocol*: das verwendete Anwendungsprotokoll: HTTP, FTP oder SMTP.
* *username[:password]@* ermoeglicht die optionale Angabe eines Benutzernamens
  und Kennworts. Das mit dem Kennwort ist natuerlich eine schlechte Idee, weil
  das vollkommen unverschluesselt uebertragen werden wuerde.
* *fqdn*: der vollqualifizierte Domain Name, ueblicherweise ohne `root` Punkt,
  der das Ziel auf Schicht 3 identifiziert.
* *port*: optional der Port, auf welchem ueber dem jeweiligen Protokoll mit dem
  Client kommuniziert werden soll. Per Default ist das immer der
  *well-known port* des Protokolls (z.B. Port 80 bei HTTP). Diesen muss der
  Browser (bzw. die Anwendung) selbst wissen.
* */path* ermoeglicht die Angabe eines Pfads auf dem Zielrechner relativ zur
  Wurzel seiner Verzeichnisstruktur.
* *?query* erlaubt die Uebergabe von Key-Value Paaren der Form
  `key=value`. Mehrere solcher Angaben koennen dabei via `&` konkateniert
  werden.
* *#fragment* macht es moeglich, einzelne Abschnitte einer Datei zu
  referenzieren.

## HTTP

Das beliebteste Protokoll auf Schicht 7, das zur Uebertragung von Daten zwischen
Servern und Clients genutzt wird, ist das *HyperText Transfer Protokoll*
(*HTTP*). Es definiert einen Request/Reply Mechanismus und definiert, welche
Anfragen ein Client stellen darf und wie ein Server darauf zu reagieren hat. Mit
einem HTTP Kommando (z.b. `GET`, `PUT`, `POST`) kann *hoechstens ein Objekt*,
z.B. eine Textdatei oder eine Grafik, uebertragen werden. HTTP ist hierbei ein
vollkommen *textbasiertes* Protokoll, d.h. alle Anfragen werden in ASCII
kodiert. Eingehende HTTP Verbindungen hierbei werden auf dem well-known Port TCP
*80* erwartet.

Der heutige HTTP Standard ist in Version 2.0 und wurde 2015 auf Basis eines
Protokolls von Google <3 veroeffentlicht. Der allererste Standard, HTML 1.0,
hatte noch ein grosses Problem. Es schlug naemlich vor, dass nach jedem
Anfrage/Abfrage Paar die assoziierte Schicht 4 (TCP) Verbindung wieder abgebaut
werden soll. Das machte bei der Geburt des Internets noch Sinn, als Websiten
noch nicht viel Content hatten. Heutzutage waere es aber absolut katastrophal,
fuer jedes kleine Element auf einer Website (z.B. jedes kleine Bild) einen TCP
Verbindungsaufbau und -abbau haben zu muessen. Deswegen wuerde mit HTTP 1.1
festgelegt, dass TCP Verbindungen auch laenger offen sein duerfen.

Es gibt grundsaetzlich zwei Arten von HTTP Nachrichten: Requests und
Responses. Requests gehen dabei vom Client zum Server und haben immer eine
*Method*. Die Method gibt die Aktion an, die sich der Client vom Server
wuenscht. Das koennte z.B. eine `GET` Aktion fuer eine Ressource auf der Website
sein. Allgemein gibt es folgende moegliche HTTP Methoden:

* `GET`: Requested eine Ressource auf dem Server. Diese Anfrage sollte keine
  Nebeneffekte haben.
* `HEAD`: Wir eine `GET` Anfrage, wobei aber nur der Header zurueckgegeben
  wird. Das ist nuetzlich, um Metainformationen zu erhalten, ohne die ganze
  Payload transportieren zu muessen.
* `POST`: Erlaubt es dem Client, dem Server Daten zu uebergeben, die dann meist
  auf diesem gespeichert werden sollen.
* `PUT`: Der Client moechte eine Ressource (URI) mit neuen Daten updaten oder
  die entsprechende Ressource erstellen, falls es sie noch nicht gibt.
* `DELETE`: Loescht die entsprechende Ressource auf dem Server.
* `TRACE`: Echoed den Request, damit der Client sehen kann, welche
  Veraenderungen an der HTTP Nachricht Server auf dem Weg dorthin machen.
* `OPTIONS`: Sagt dem Client, welche Arten von Anfragen er machen darf.

Der *Pfad* des URLs wird hierbei besonders wichtig, da er die angefragte Datei
genau beschreiben kann. Auch die Query Parameter sind fuer bestimmte APIs
notwendig. Ein HTTP Request hat auch immer einen *HTTP Header*, welcher weitere
Informationen enthaelt, naemlich:

* Den FQDN des angefragten Hosts,
* Zeichensatz und Encoding, in dem die Antwort erwartet wird (Accept-Charset)
* Von wo eine Anfrage kam (z.B. bei Weiterleitung). Wird dann *Referer* (falsch
  geschrieben) genannt.
* Den *User-Agent*, was die verwendete Client-Software sein soll.

HTTP *Responses* sagen dann zunaechst als aller Erstes immer aus, was der
*Status* der Nachricht ist. Dieser Status besteht aus einem numerischen Code
(dem Status Code, z.B. 404) sowie ein Text zur Angabe von Fehlern. Moegliche
Status Codes sind hierbei die folgenden, wobei die erste der drei Ziffern jedes
Codes immer die Klasse angibt:

* 1xx: *Informational* Codes. Sagen, dass der Request erhalten wurde und die
  Verbindung weiterschreiten kann. Beispiele:
  - 101: Switching Protocols
* 2xx: *Success*. Diese bestaetigen eine erfolgreiche Uebertragung. Beispiele:
  - 200: OK
  - 201: Created (after POST)
* 3xx: *Redirection*. Bedeutet, dass weitere Aktionen vom Client benoetigt
  werden um den Request vollkommen abzuschliessen. Beispiele:
  - 301: Moved Permanently (die angefragte Ressource ist nun woanders)
  - 302: Redirect (`location` gibt neuen URL an, wo der Client anfragen soll)
  - 305: Use Proxy (benutze bitte den (reverse) Proxy, der im `location` Header steht)
* 4xx: *Client Error*. Die Anfrage war auf Grund eines Fehlers vom Client nicht
  verarbeitbar. Das kann Syntaxgruende, aber auch semantische Gruende
  haben. Beispiele:
  - 404: Page Not Found
  - 400: Bad Request (Server hat die Anfrage einfach nicht verstanden)
  - 403: Forbidden
  - 418: I'm a Teapot
* 5xx: *Server Error*. Die Anfrage koennte auf Grund eines Fehlers beim Server
  nicht korrekt beantwortet oder verarbeitet werden. Beispiele:
  - 500: Internal Error (Unerwarteter Fehler beim Server)
  - 502: Bad Gateway (Server bekam eine falsche Antwort von einem anderen
    Server)
  - 503: Service Unavailable (temporarily down)

Dann haben HTTP Responses, wie Requests, auch Header die weitere Optionen
beeinhalten koennen. Das ganze wird dann via CRLF (Carriage Return Line Feed;
`\r\n`) von den eigentlichen Nutzdaten abgetrennt.

### Verschluesselung

HTTP selbst kennt keine Verschluesselungsmechanismen fuer uebertragene
Daten. Man kann aber zwischen Transport- und Anwendungsschicht (insbesondere
Darstellungsschicht) Verschluesselungsprotokolle nutzen.

Beispielsweise gibt es HTTP__S__, welches das TLS (Transport Layer Security) auf
der Sitzungs-/Darstellungsschicht (5/6) nutzt. Bei HTTPS wird der ganze HTTP
Request, inklusive Header und Daten (Payload) ver- und entschluesselt. Einzig
die Website (im Sinne ihrer IP Adresse in der Transportschicht PDU) sowie Port
können nicht geschuetzt werden, einfach weil TLS auf einer höheren Ebene
arbeitet. Als Loesung gibt es beispielsweise IPSec
(https://en.wikipedia.org/wiki/IPsec), welches schon auf Schicht 3
verschluesselt, und somit auch die gesamte IP Payload, inklusive Destination und
Source IP, schuetzt.

Fuer HTTPS wird TCP Port 443 verwendet anstelle von TCP Port 80, zur
Unterscheidung von HTTP.

https://en.wikipedia.org/wiki/HTTPS

## Proxy

Proxies sind Rechner, die zwischen einem Client und einem Server stehen, auf
welchen der Client zugreifen will. Dann hat der Client also keine Ende-zu-Ende
Verbindung zum Server, sondern nur indirekt ueber den Proxy. Anstelle den
Server direkt zu kontaktieren, wird der Client seine *HTTP* Anfragen zunaechst
an den Proxy senden. Dieser verwendet dann oftmals andere Well-Known
Portnummern, z.B. 3128 anstelle von 80.

Sobald der Proxy dann eine Anfrage von einem Client erhaelt, baut er selbst eine
Verbindung zum Zielserver auf und stellt die Anfrage des Clients. Der Server
selbst denkt dann, dass er mit dem Proxy kommuniziert und nicht mit dem
Client. Der Proxy wuerde die entsprechende Antwort dann wieder zum Client
senden.

Eine wichtige Aufgabe von Proxies ist im Weiteren, die Anfragen der Clients dann
zu *cachen*. Das heisst, dass ein Proxy die Antwort auf eine Anfrage eines
Clients fuer einen gewissen Zeitraum speichert, sodass darauffolgende,
identische Anfragen (auch von anderen Clients) dann nicht mehr zum Server gehen
muessen. Der Proxy kann einfach direkt die im Cache liegende Antwort
zurueckgeben (z.B. ein Bild, das mittels HTTP `GET` angefragt wurde).

Proxies koennen auch transparent arbeiten und Netzwerkpakete filtern. Das ist in
Schulen oder Firmennetzen haeufig anzutrefen, um die Produktivitaet von
Mitarbeitern zu maximieren, indem man sie nicht auf Spieleseiten laesst. Hierbei
muss ein Administrator den Proxy aber manuell einrichten (siehe unten).

Eine weitere Eigenschaft, die mit Proxies assoziiert ist, ist *Anonymitaet*. Da
der Server den Client, der ueber einen Proxy mit dem Server kommuniziert, nicht
sieht, ist dessen Identitaet nicht sichtbar fuer den Server. Der Client ist also
gewissermassen anonym. Das ist beispielsweise das Konzept hinter TOR (The Onion
Router). Requests werden durch mehrere, global verteilte Proxies geroutet, damit
am Ende die Identitaet des urspruenglichen Clients geschuetzt ist (durch
Verzwiebelung der Anfrage, also zwiebelaehnliche Verdeckung).

Proxies, wie sie bisher beschrieben wurden, sind eigentlich nur eine bestimme
Art von Proxy. Man nennt sie *Forward Proxies*. Neben *Forward Proxies* gibt es
auch noch *Reverse Proxies*. Der hauptsaechliche Unterschied hierbei liegt
darin, wer den Proxy konfiguriert:

1. Forward Proxies:
   * Muessen vom Client konfiguriert werden.
   * Dienen manchmal dazu, Traffic zu kontrollieren. Beispielsweise koennte eine
     Schule jeglichen Traffic, der nicht durch ihren Proxy geht, einfach
     unterbinden (durch eine Firewall an ausgehenden Routern). D.h., dass alle
     Requests notwendigerweise durch den Proxy gehen muessen (das wuerden
     Netzwerkadministratoren auf allen PCs in der Schule so
     konfigurieren). D.h. wiederum, dass man auf dem Proxy dann alle Requests
     inspizieren und pruefen kann, ob man den Request ueberhaupt zulassen
     soll. Geht der Request beispielsweise auf `playboy.com`, so wuerde man ihn
     verhindern und das HTML fuer eine schulinterne Fehlerwebsite
     zurueckgeben. Andere Requests koennte man dann einfach durchlaufen lassen.
   * Manchmal dienen sie auch dazu, das obere gerade zu umgehen. Wenn ich
     beispielsweise dennoch `playboy.com` aufrufen moechte, koennte ich mich mit
     einem externen Proxy Server `definitely-not-a-proxy-for-playboy.com`
     verbinden, der meine Requests dann einfach zu `playboy.com` weiter
     routet. Der obige ("Firewall") Proxy wuerde dann ja nicht `playboy.com`
     sehen, sondern nur die Adresse dieses Forward Proxies. Insofern wuerde er
     den Traffic nicht unterbinden.
   * Insbesondere bleibt der Client hinter dem Proxy verborgen. Der Server sieht
     nur den Proxy. Hierdurch kann der Client also eine gewisse Anonymitaet
     erreichen.

2. Reverse Proxies:
   * Werden vom *Server* konfiguriert (Server-Side Konzept).
   * Ein Server kann einstellen, dass Requests, die eigentlich an ihn gerichtet
     sind durch einen *Reverse Proxy* gehen muss (der Domain Name bzw. die IP
     Adresse wuerde wohl den Proxy, nicht den Server identifizieren).
   * Das koennte z.B. den Sinn haben, Traffic fuer eine beliebte Website zu
     load-balancen. Das ist gerade was Content Delivery Networks (CDNs) wie
     CloudFront oder Akamai machen. Sie bieten den Dienst an, global Reverse
     Proxies aufzustellen, sodass Requests fuer, z.B. `apple.com`, an den
     geographisch naechsten Proxy geroutet werden. Dieser erfuellt den Request
     dann, indem er entweder eine gecachede Antwort zurueckgibt, oder selbst
     einen Request an den Server von Apple im Hintergrund stellt.
   * Reverse Proxies sind auch dazu nuetzlich, Verschluesselung uber TSL einfach
     zu "poolen". Sagen wir, der Reverse Proxy hat 10 Server im
     Hintergrund. Anstelle die 10 Server mit TSL auszustatten (was entsprechende
     Zertifikate benoetigit), stattet man einfach den einen Reverse Proxy mit
     TLS aus und hat somit jegliche Client-Server Kommunikation bis zum Reverse
     Proxy geschuetzt. Dieser Reverse Proxy waere dann wohl im selben lokalen
     Netzwerk wie die Server, also muesste man sich um den restlichen Weg nicht
     mehr kuemmern.

Das wichtige ist, dass Forward Proxies als Zwischenstationen von Client-Requests
auf den Server dienen, wohingegen Reverse Proxies als Zwischenstationen von
Server-Responses fuer den Client dienen. Insbesondere sieht der Client bei
Reverse-Proxies den Server nicht und weiss gar nicht, dass dieser
existiert. Fuer den Client ist der Reverse-Proxy der Server. Nur der
Reverse-Proxy weiss, dass er die Requests an die eigentlichen Server im
Hintergrund weiterleiten muss (und Sachen dann cached).

Forward Proxy:
```
A ---|
     |
B --- Proxy --> Server
     |
C ---|
```

Reverse Proxy:
```
                    Server A
                  /
                 /
Client --> Proxy -- Server B
                 \
				  \ Server C
```

https://www.quora.com/Whats-the-difference-between-a-reverse-proxy-and-forward-proxy

## SMTP

Das *Simple Mail Transfer Protocol* (*SMTP*) ist das gaengigste Protokoll zum
Versenden von E-Mails im Internet. Es ist ein textbasiertes Protokoll und hat
drei grundlegende Aktoren:

1. *Mail User Agents* (*MUAs*), welche E-Mail versenden und empfangen wollen.
2. *Mail Transfer Agents* (*MTAs*), welche E-Mail durch das Internet routen. MTA
   sind hierbei ein Synonym fuer "Mailserver". Solche Server benutzen oft Postfix
   (http://www.postfix.org) als Software. Sie vermitteln Mail zwischen MUAs
   (z.B. Apple Mail) und MDAs (z.B. `imap.gmail.com`).
3. *Mail Delivery Agents* (*MDA*), welche E-Mails fuer den Empfaenger in
   *Mailboxen* speichern. Sie erhalten also E-Mails von MTAs und platzieren sie
   in eine Mailbox fuer den User, sodass dieser dann ueber POP oder IMAP darauf
   zugreifen kann.

```
Sender ==> MUA ==SMTP==> MTA ==SMTP==> ... ==SMTP==> MTA
	   ==SMTP==> MDA ==POP/IMAP==> MUA ==> Empfaenger
```

https://en.wikipedia.org/wiki/Message_transfer_agent
http://ccm.net/contents/116-how-email-works-mta-mda-muaA

Es gibt wie oben bemerkt also zwei Protokolle, ueber welche man auf E-Mails
zugreifen kann, die ein MDA speichert:

1. Das *Post Office Protocol* (*POP*): POP ist momentan in Version 3 und ein
   vergleichsweise viel simpleres Protokoll als IMAP. Es dient mehr oder weniger
   nur dazu, E-Mail in der Mailbox des Mail-Servers zwischenzuspeichern, bis
   *ein* Client die Nachricht downloaded. Per Default wird die Nachricht dann
   vom Mail-Server geloescht. Das ist insofern bloed, wenn mehr als ein Client,
   also mehr als ein Geraet, auf die selbe Mailbox zugreifen moechte. Das waere
   beispielsweise so, wenn man von seinem Laptop und von seinem Smartphone
   zugreifen will. Offensichtlich ist das heutzutage der Regelfall. Hierfuer
   gibt es dann noch die Option, dass die Mail auf dem Server bleiben kann. Aber
   das wichtige ist, dass es ueberhaupt *keine Synchronisation zwischen dem
   Server und einzelnen Geraeten* gibt. Wenn man auf einem Geraet eine Mail
   downloaded, dann ist das auf anderen Geraeten noch nicht so. Wenn man sie auf
   einem Geraet loescht, ist *sie noch nicht auf dem Server geloescht* und auch
   sicher nicht auf anderen Geraeten. Auch hat POP kein Konzept von Flags fuer
   "read", "replied to", "deleted" oder "important".

2. Das *Internet Message Access Protocol* (*IMAP*) ist viel flexibler und
   komplexer als POP. Insbesondere hat es viel staerkere Synchronisation
   zwischen Server und Client. Clients cachen Nachrichten nur. Somit sind
   Mail-Clients (MUAs) als eher Interfaces zu diesen Servern. Jegliche
   Veraenderungen werden auf dem Server durchgefuehrt. Das bedeutet dann aber
   auch, dass diese Aenderungen auf allen Clients der Mailbox, also heutzutage
   auf allen Geraeten, reflektiert sind. Loesche ich meine E-Mail auf meinem
   Smartphone, so wird sie auch auf dem Server geloescht und das sehe ich dann
   auch auf meinem Laptop. Auch hat IMAP ein Konzept von Flags ("read", "replied
   to").

http://www.pop2imap.com

Um einen Mailserver (MTA) im DNS zu registrieren, muss man in Zone Files entsprechende
*MX* Resource Records eintragen, diese sehen beispielsweise so aus:

```
tum.de. 3600 IN MX 100 postrelay2.lrz.de.
```

1. Hier steht links der FQDN, fuer dessen Domain der Mailserver zustaendig ist.
2. Der TTL fuer DNS Clients
3. Die Record Class `IN`
4. `MX` als Record Type.
5. Dann folgt die Resource Data, bestehend aus:
   1. Einer *Priority* fuer den Mailserver. Wenn man mehrere hat, kann man diese
      nach Priority sortieren, sodass Mailserver mit hoeherer Priority zuerst
      angefragt werden (hierbei bedeuten kleinere Werte hoehere Priority).
   2. Dem FQDN des Mailservers.

Eine E-Mail geht also von MTA zu MTA, wobei jeder MTA selbst wieder MX-Eintraege
hat. So kann eine E-Mail also von `gmail.com` zu `goldsborough.me` Adressen
geroutet werden.

## FTP

Ein weiteres Schicht 7 Protokoll ist das *File Transfer Protocol* (*FTP*). Es
wird genutzt, um Binaerdaten, also Dateien, zwischen Server und Client zu
uebertragen. FTP verwendet hierbei Well-Known Port TCP 21.

Eine Eigenheit von FTP ist, dass es *zwei* getrennte TCP-Verbindungen fuer eine
Verbindung nutzt:

1. Die erste Verbindung ist der *Kontrollkanal*, zur Uebermittlung von Befehlen
   und Statuscodes zwischen Client und Server. Hier wird beispielsweise
   festgelegt, ueber welche Ports Daten ausgetauscht werden sollen. Generell
   erfolgt ueber diesen Kanal der Verbindungsaufbau und -abbau. Auch werden
   Fehlermeldungen ueber diesen Kanal gesendet. Da dieser Kanal ueber mehrere
   Datenuebertragungen hinweg erhalten bleibt, spricht man von FTP als ein
   *stateful* Protokoll. Ueber diesen Kanal muessen auch Benutzername und
   Passwort, welche bei FTP immer benoetigt werden, ausgetauscht werden. Falls
   ein Server auch ohne Benutzername und Passwort Zugriffe erlaubt (wenn auch
   mit restriktiven Permissions), dann gibt es oft noch einen Benutzernamen
   `anonymous` oder `ftp`, der kein (bzw. leeres) Passwort benoetigt.

2. Die zweite Verbindung ist der *Datenkanal*, welcher fuer die Uebertragung der
   eigentlichen Daten genutzt wird.

Es gibt im Weiteren zwei Modi, in welchen FTP ausgefuehrt werden kann, *active*
und *passive* Mode:

* In beiden Faellen baut der Client den Kontrollkanal zum Server auf TCP 21 auf.
* Im *active mode* koordiniert der Client den Datenkanal. Er uebermittelt dem
  Server dann durch das `PORT`-Kommando ueber den Kontrollkanal eine Portnummer
  und IP Adresse mit, auf welcher sich der *Server* dann mit dem *Client*
  verbindet. Der Server `connect`ed sich dann also ueber TCP Quellport 20 auf
  den angegebenen Port und Adresse des Clients, welcher dort `listen()`ed. Ueber
  diesen Kanal werden Daten dann ausgetauscht.
* Im *passive mode* sendet der Client das Kommando `PASV` ueber den
  Kontrollkanal und erhaelt vom Server eine IP Adresse und zugehoeriegen Port,
  ueber welchen der Client dann eine neue Verbindung *zum Server* aufbaut.

Wieso gibt es diese zwei Modi? Betrachten wir, welche Probleme es geben kann,
wenn sich der Client im *active mode* mit dem Server verbinden will:

1. Der Client ist hinter NAT und hat lokale (private) Adresse `10.0.0.7`.
2. Der Client sendet `PORT 10,0,0,7,123,64` um dem Server mitzuteilen, dass der
   Client nun auf Port `31,552` und IP `10.0.0.7` `listen`ed(). Der Client
   kriegt vom NAT ja nichts mit. Wie man sieht, werden diese Daten byteweise in
   Dezimal und Komma-getrennt uebertragen. Deswegen ist der Port $123 * 256 + 64
   = 31,552$.
3. Beim Router wird die IP Adresse dieses Pakets von der privaten Adresse
   `10.0.0.7` auf eine global eindeutige Adresse des Routers geaendert.
3. Der Server erhaelt die Port Information und versucht nun, sich mit dem Client
   an Adresse `10.0.0.7` und Port `31,552` zu verbinden.
4. Er scheitert, weil die IP Adresse im `PORT` Kommand natuerlich nicht geandert
   wurde, und noch immer die private Adresse `10.0.0.7` enthielt. Als private
   Adresse wird sie bzw. das Paket natuerlich nicht geroutet.

In diesem Fall erkennt man, dass Passive Mode notwendig waere, damit der Server
dem Client eine Verbindungsmoeglichkeit anbietet und nicht andersrum. Waere der
Server hinter NAT, wuerde natuerlich wiederum nur Active Mode
funktionieren. Sind beide hinter NAT, hat man ein Problem. Ausser der Server
richtet dann explizit einen Forwarding Eintrag fuer die Kontrollkanaladresse
ein. Beim Programmieren wuerde man dann fuer jeden `socket()` Aufruf sowieso
einen eigenen Socket fuer jede eingehende Verbindung erhalten, insofern wuerde
das gut funktionieren. Ein Socket ist ja durch das Fuenf-Tupel identifiziert,
also wuerde jeder Socket eine dedizierte Verbindung mit einem Client ueber
denselben Port bedeuten.
