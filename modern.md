# Modernes

## Pipelining

Pipelining ist eine Methode viele Maschinenbefehle parallel auszufuehren. Hierzu
werden Befehle in mehrere Einzelschritte aufgeteilt, sodass dann bestimmte Teile
der CPU die ihnen zugewiesenen Teile erledigen koennen. Wenn ein Teilschritt
eines Befehls dann erledigt ist, kann schon der naechste Befehl in diesem
Teilschritt ausgefuehrt werden. Das ist das Grundprinzip.

Sei hier nun zuerst ein einfacher Maschinenbefehlszyklus dargestellt:

* Instruction Fetch (IF): Befehl aus Speicher ins Instruktionsregister laden.
* Instruction Decode (ID): Befehl dekodieren.
* Execute (EX): ALU-Operation ausfuehren
* Memory Access (MA): Speicherzugriff
* Write-Back (WB): Ergebnis der Operation zurueckschreiben.

Ein solcher Maschinenbefehlszyklus besteht also aus $5$ Takte. Jeder Takt kann
von einem separaten Teil der Maschine ausgefuehrt werden und somit wird pro
Befehl ein solcher Teil der Maschine nur in einem von fuenf Takten benutzt. Das
gilt es zu optimieren.

Die Grundidee ist es nun, eine $5$-stufige Befehlspipeline herzustellen. Das
bedeutet, dass wenn der erste Befehl die IF-Stufe abgeschlossen hat und in die
ID Stufe uebergeht, der naechste Befehl schon auch gleich in die IF Stufe gehen
kann. Nach $5$ Takten wuerde der erste Befehl also fertig sein, und einen Takt
spaeter schon der zweite. Somit hat man in $6$ Takten zwei Befehle fertig
gestellt. Ohne Pipelining haette das $10$ Takte gedauert. Jeden Takt wird somit
ein Befehl vervollstaendigt.

### Formal

Pipelining liegt in einer Maschine dann vor, wenn die Bearbeitung eines Objekts
in Teilschritte zerlegt und diese in einer sequentiellen Folge (die *Phasen der
Pipeline*) abgearbeitet werden kann, wobei aber die Phasen der Pipeline fuer
verschiedene Befehle __ueberlappt__.

Die *Pipeline* ist dabei die Gesamtheit der Verarbeitungseinheiten. Eine
Pipeline-*Stufe* beschreibt nun eine einzige Verarbeitungseinheit im
Pipelining-Prozess.

### Idealer Fall

Idealisiert benoetigt eine Pipeline mit $k$ Stufen genau $k$ Takte fuer die
Ausfuehrung eines Befehls. Dabei sind nach anfaenglicher Fuellung der Pipeline
(dauert einmal $k - 1$ Takte) immer $k$ Befehle gleichzeitig in der Pipeline.

Die Latenz $L$ einer Pipeline beschreibt dabei die Zeit, die ein Befehl zum
Durchlaufen der Pipeline benoetigt. Im *Idealfall* ist die Latenz bei $k$ Stufen
auch genau $k$.

Mit jedem Takt wird dann ein Befehl angefangen, und ein Befehl beendet. Es sei
aber angemerkt, dass die Laufzeit eines Befehls nicht reduziert wird.

### Realer Fall

In echt ist die Latenz $L$ aber nicht immer gleich $k$. Es
gibt naemlich gewisse Pipeline-Hemnisse. Beispielsweise dauert das Auslesen und
Laden der Pipeline-Register eine Weile. Auch muss die Stufenlaufzeit nicht immer
genau einen Takt sein, sondern kann auch weniger sein. Am Anfang dauert es auch
ein wenig, um die Pipeline zu fuellen.

Auch nehmen wir im idealisierten Modell an, dass jede Pipeline-Stufe
verschiedene Ressourcen aufnimmt. Das ist aber nicht immer der Fall.
Beispielsweise greifen instruction-fetch (IF) und memory-access (MA) beide auf
den Hauptspeicher zu. Das kann zu Pipeline-Konflikten fuehren. Konflikte und
Spruenge fuehren auch dazu, dass die Pipeline nicht immer voll gefuellt ist.

### Konflikte

Pipeline-Konflikte (Pipeline-*Hazards*) sind zum einen Situationen, die
verhindern, dass die naechste Instruktion im Befehlsstrom im zugewiesenen
Taktzyklus ausgefuehrt wird; zum Anderen Unterbrechungen des synchronen
Durchlaufs der Pipeline durch die Stufen (z.B. Ressourcen-Konflikte).

In einem solchen Fall muss die momentane Instruktion angehalten werden, und
somit auch alle anderen Instruktionen, die *vor* der Instruktion in der Pipeline
liegen. Dieses Anhalten geschieht durch einfuegen von NOP Operationen
(no-operation). Befehle, die *nach* der Instruktion in der Pipeline liegen (in
spaeteren Stufen), koennen aber weiterlaufen.

#### Datenkonflikte

Datenkonflikte sind eine weitere Klasse von Pipeline-Konflikten. Diese beziehen
sich auf Situationen, wo eine Instruktion das Ergebnis einer anderen Instruktion
in der Pipeline benoetigt, welche aber noch nicht abgeschlossen ist.

Datenkonflikte koennen beispielweise auftreten, wenn Befehle nahe aneinander im
Programm stehen und das Pipelining der Befehle zu einer Veraenderung der
Zugriffe auf ein Datum fuehrt.

Beispiel: `add r1, r1, r2` und `mul r1, r1, r2`.

Man bemerke hier, dass die Execute und Write-Back Phasen fast hintereinander
im Befehlzyklus stehen. Das bedeutet, dass `add` zuerst `r1` liesst, und danach
gleich auch `mul`. *Danach* erst wird `add` den Wert von `r1` veraendern und am
Ende zurueckschreiben. `mul` wird dann noch mit dem Alten Wert rechnen und ein
voellig falsches Ergebnis zurueckschreiben.

Um dieses Problem zu loesen, muss die Hardware einen solchen Konflikt loesen und
den ersten Befehl erst durch die Pipeline schleussen, damit der andere Befehl
dann mit den richtigen Werten arbeiten kann.

#### Steuerkonflikte

Noch eine weitere Klasse sind die *Steuerkonflikte*. Diese treten bei
Sprungbefehlen und allen anderen Befehle auf, die den Befehlszaehler explizit
veraendern.

Wahrend nun aber eine Sprungbedingung ausgefuehrt wird, was eine Zeit dauert,
kann das System schon die sogenannte Branch-Prediction (Sprungvorhersage)
durchfuehren. Das System versucht also herauszufinden, schon bevor eine
Bedingung wirklich ueberprueft wurde, ob sie gelten wird, oder nicht. Je nach
Spekulation werden dann waehrend die Bedingung echt ueberprueft wird (was
z.B. wieder Speicherzugriffe, also Zeit, erfordert) schon neue Befehl in die
Pipeline gestossen.

Wenn das System spekuliert, die Bedingung wird gelten, so ladet es Befehle vom
Sprungziel. Wenn es meint, das Programm wird nicht springen, sondern sequentiell
fortschalten, ladet es Befehle, die nach der Sprunginstruktion (also ohne
Sprung) stehen.

Nach Auswertung der Sprungbedingung koennen diese Befehle weiter durch die
Pipeline geschoben werden, wenn die Sprungvorhersage erfolgreich war. Wenn
nicht, werden die geholten Befehle verworfen.

##### Statische Sprungvorhersage

Eine Technik der Sprungvorhersage nennt man *statisch*. Das ist dann, wenn
fuer einen bestimmten Befehl immer dasselbe vorhergesagt wird.

##### Dynamische Sprungvorhersage

Dynamische Sprungvorhersage versucht, die Bedingung anhand der Vorgeschichte des
Programms zu bestimmen.

## Superskalaritaet

Ein Nachteil des gerade vorgestellten Pipeline-Konzepts ist das Befehle die
blockieren, auch Befehle blockieren, die unabhaengig vom blockierenden Befehl
sind. Die Idee des dynamischen Schedulings ist es jetzt. Diese Befehle
*gleichzeitig* auszufuehren. Hierzu ist das *Superskalarprinzip* wichtig.

Ein *superskalarer* Prozessor verfuegt im Vergleich zu einem Prozessor mit
sequentieller Pipeline ueber die $n$-fache Anzahl von Funktionseinheiten,
Datenpfaden, Dekodern usw. Somit kann man mehrere Pipelines gleichzeitig haben,
und mehrere Befehle koennen gleichzeitig in einer bestimmten Pipeline-Stufe
(z.B. IF) sein.

### Threading

Ein *Thread* ist ein eigenstaendiger Handlungsfaden innerhalb eines Prozesses,
also innerhalb eines Programms. Waehrend Prozesse ueber
inter-process-communication miteinander Daten austauschen, teilen sich Threads
saemtliche Systemressourcen mit Ausnahme der Prozessorregister.

Der Sinn ist, dass waehrend ein Thread blockiert ist, andere Threads
unproblematisch weitergefuehrt werden koennen und in anderen Funktionseinheiten,
die gerade frei sind, abgearbeitet werden koennen.

Im Prozessor braucht man nun jedoch mehrere Registersaetze inklusive
Befehlszaehler, sowie muss auch die Threadwechsel behandeln. Ansaetze fuer die
quasi-parallele Ausfuehrung von Threads sind nun unten beschrieben.

#### Cycle-by-Cyle Interleaving

In diesem Modell stehen dem Prozessor immer eine gewissen Anzahl an aktiven
Threads zur Verfuegung. Er kann dann auswaehlen, welchen er bearbeiten moechte
und dann schon den naechsten Thread zur Ausfuehrung vorbereiten. So werden die
Threads also Takt-fuer-Takt (cycle-by-cycle) "parallel" ausgefuehrt.

#### Block-Interleaving

Der Gedanke bei dieser Methode ist es, einen Thread so lange zu behandeln, bis
er einen Befehl mit langer Latenzzeit benoetigt (bis er blockiert). Dann,
waehrend er blockiert, wechselt er zum naechsten Thread.

#### Simultaenous Multithreading

Bei dieser Variante werden wirklich mehrere Threads gleichzeitig bearbeitet.
Also in jedem der Kerne des superskalaren Prozessors wird gleichzeitig ein
anderer Thread in einer bestimmten Stufe bearbeitet. Es gibt dann also mehrere
Befehlspuffer, wobei jeder Befehlspuffer den Befehlsstrom eines anderen Threads
enthaelt, welche ein Kern dann behandeln kann. Jeder Befehlsstrom hat also auch
einen eigenen Registesatz.
