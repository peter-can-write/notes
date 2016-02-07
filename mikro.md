# Mikroprogrammierung

Die Mikroebene ist eine Ebene tiefer als die Maschinenebene (Assembler).

Bei der Mikroarchitektur befinden wir uns auf der *Register-Transfer-Ebene*
(*RTL*; "Register-Transfer-Level"). Hierbei transportieren wir Werte vom einen
Register in das andere und verknuepfen den Wert optional mit einer Operation --
einer sogenannten "Mikrooperation".

## Mikroprogrammierbare Rechner

Allgemein ist nicht jeder Rechner mikroprogrammierbar. Es gibt naemlich zwei
arten von Rechnern bzw. spezifischer Leitwerken (auf die es letztendlich
ankommt): festverdrahtete und mikroprogrammierbare.

Hierzu ist es nuetzlich, den Rechner als eine endlichen Moore-Automaten $M = (E,
A, Q, \delta, \lambda)$ zu sehen. Ein Eingang ist dabei ein Maschinenbefehl.

### Festverdrahtet

Nun ist ein festverdrahtetes Leitwerk ein solches, wo $\lambda$ und $\delta$ in
Hardware direkt umgesetzt sind. Fuer einen Maschinenbefehl als Eingang gibt es
also nur eine, feste Implementierungsstrategie, die durch geeignete Kodierung in
elektrische Signale umgewandelt werden, und somit die einzelnen Schritte zur
Durchfuehrung des Maschinenbefehls umsetzt.

$\delta$ ist also in Hardware so umgesetzt, dass es von der einen
Mikroinstruktion automatisch zur naechsten, festen springt.

### Mikroprorammierbar

Im Gegensatz zum festverdrahteten Leitwerk gibt es in einem
mikroprogrammierbaren keine feste Implementierung fuer Maschinenbefehle. Die
einzelnen Mikroinstruktionen muessen also in einem schreibbaren Speicher
abgelegt werden, und dann von der Maschine Schritt fuer Schritt gelesen werden.

Es ist also somit der Mikroprogrammierer, der $\delta$ und $\lambda$ festlegt.

### Vergleich

Festverdrahtete Leitwerke haben den Vorteil, dass sie schneller arbeiten als
mikroprogrammierte. Bei einem mikroprogrammierbaren Leitwerk ist pro Takt immer
ein Zugriff auf den Mikroprogrammspeicher notwendig, der relativ viel Zeit
kostet und damit die maximal moegliche Taktfrequenz des Prozessors
herabsetzt.

Mikroprogrammierte Leitwerke sind dafuer einfacher zu entwerfen und lassen sich
schneller an wechselnde Anforderungen anpassen, da nur der Inhalt des
Mikroprogrammspeichers geaendert werden muss, waehrend die eigentliche Hardware
unveraendert bleibt.

Da es heute jedoch sehr effiziente Werkzeuge zum Hardware-Entwurf gibt (VHDL),
werden bei modernen Prozessoren wegen des Geschwindigkeitsvorteils meist
festverdrahtete Leitwerke eingesetzt.

## Mikroprogramme

Ein Mikroprogramm besteht aus Mikroinstruktionen (jeweils 1 Takt), und jede
Mikroinstruktion besteht aus parallel ausgefuehrten, unteilbaren
Mikrooperationen.

Mikroprogramm:
* In der Mikromaschine implementiert
* Interpretiert Maschinenbefehl
* Besteht aus Mikroinstruktionen

Mikroinstruktionen (eine Zeile in einem Mikroprogramm):
* im Mikroprogrammspeicher abgelegt (spezieller Speicher)
* bestehen aus Mikrooperationen
* laesst sich gliedern in
	+ *Addressteil*: Jene Mikrooperationen bzw. Daten die noetig sind
      bzw. bestimmen, was die naechste Adresse des Mikroprogramms im
      Mikrospeicher ist. Also Branch-Adresse, Befehlszaehler,
      Adressfortschaltbefehle fuer Leitwerk (Am2910)
	+ *Steuerteil*: Jene Mikroperationen und Direktdaten (Immediates) die die
	Funktionsweise des Rechners fuer die momentane Mikroinstruktion, also einem
	Takt, festlegen.

Mikrooperationen:
* Unteilbare Steuerinformation zur Ausloesung eines Register-Transfers
* Die Mikrooperationenen einer Mikroinstruktion werden parallel (in einem Takt)
  ausgefuehrt (sind ja nur elektrische Signale).

### Kodierung

Eine Mikroinstruktion kann auf verschieden Weisen kodiert werden, um etwa
Speicher im Mikrospeicher zu sparen, oder um Zeit fuer Dekodierung zu sparen. Es
gibt hier drei Klassen von Kodierung:

#### Horizontal

Eine *horizontale* bzw. *unverschluesselte* Kodierung einer Mikroinstruktion ist
eine, bei der jedes Steuersignal einem einzigen, eindeutigen Bit
entspricht. Hat man also $30$ Operationen, so hat die Mikroinstruktion $30$ Bit
und jeder Bit hat nur eine Funktion.

Vorteile:
* Man spart sich jegliche Dekodierung, also Zeit.

Nachteile:
* Verbrauchen mehr Platz im Speicher.
* Unmoegliche oder redundante Zustaende (Kombinationen von Steuersignalen) sind
  von den Bits her erlaubt. Schliessen sich zwei Steuersignale z.B. aus (immer
  nur das eine oder das andere ist HIGH), so verschwendet man im Prinzip einen
  Bit.

#### Vertikal

Bei einer *vertikalen* bzw. *stark verschluesselten* Kodierung von
Mikroinstruktionen stehen bestimmte Kombinationen von Bits fuer bestimmte
Zustaende, also bestimmten Kombinationen von Steuersignalen. Hat man z.B. $32$
Signale von denen immer nur eines HIGH ist, so kann man diese mit $5$ Bit
kodieren, und nicht mit $32$ wie bei horizontalen. Hierbei ist jedoch ein
weiterer Baustein -- ein *Dekoder* -- noetig, der die verschluesselte Kodierung
sozusagen *horizontalisiert*.

Vorteile:
* Man kann sich sehr viel Platz sparen.
* Redundante Kombinationen sind ausgeschlossen.

Nachteile:
* Man braucht einen zusaetzlichen Dekodierungsbaustein, der Zeit und Geld
  kostet.

#### Quasi-Horizontal

Bei einer *quasi-horizontalen* bzw. *optimal verschluesselten* Kodierung mischt
man im Prinzip horizontal und vertikal. Bestimmte Bereiche der Mikroinstruktion
sind dabei kodiert, und brauchen dann auch nur fuer diesen Bereich einen eigenen
Dekoder, und andere Bereiche sind horizontal verschluesselt.

## Mikroprogrammierbarer Beispielrechner

Der mikroprogrammierbare Beispielrechner in ERA besteht hauptsaechlich aus einem
Hauptspeicher, Leitwerkaustein (Am2910), einem Rechnwerkbaustein (Am2901), einem
Carry-Lookahead Baustein und einer Wortrandlogik (Am2904).

Dieser Rechner ist 16-Bit. Daten- und Adressleitungen im Rechner sind also alle
16-Bit breit, somit auch der Daten- und Adressbus. Auch das Instruktionsregister
bzw. saemtliche Maschinenbefehle sind im Instruktionsregister sind
16-Bit. Konstanten muessen bei Maschinenbefehlen daher separat im Hauptspeicher
gespeichert werden, solche Maschinenbefehle sind also 32-Bit.

Allgemein besteht ein Von-Neumann Rechner bekanntlich aus drei Teilen, diese
sind auch hier zu finden:

* Hauptspeicher
* Leitwerk: Mikroleitwerk, Befehlszaehler, Mapping-PROM, Instruktionsregister
* Rechenwerk: Mikrorechenwerk, Wortrandlogik

### Leitwerk

Das Leitwerk besteht bei der MI-Maschine aus:

* Dem Instruktionsregister fuer Maschinenbefehle (Assembler)
* Dem Mapping-PROM
* Dem eigentlichen Mikroleitwerk, dass fuer die Adressfortschaltung in
  Abhaengigkeit von den Adressfortschaltbefehlen und anderen Daten im Adressteil
  der Mikroinstruktion zustaendig ist, auch in Abhaengigkeit von Bedingungen,
  die im Rechnwerk entstehen. Es enthaelt auch den __Mikrobefehlszaehler__.
* Dem Mikroprogrammspeicher, der die Mikroprogramme bzw. die einzelnen
  Mikroinstruktionen fuer jedes Mikroprogramm speichert.
* Dem Befehlszaehler fuer das __Maschinenprogramm__ (zeigt auf eine Stelle im
  Hauptspeicher).
* Einer Instruktions-PLA zuer Dekodierung von Befehlen und Steuerung des
  Adressmultiplexers.

#### Instruktionsregister

Im Instruktionsregister speichert das Leitwerk die einzelnen
*Maschinen*befehle. Jeder Maschinenbefehl besteht aus zwei Teilen:

1. Einem 8-Bit OpCode (Operation Code); belegt Bits 8 - 15.
2. Zwei Register-Operanden; belegen Bits 0 - 7

Bei der ERA-Maschine ist der oberste Bit fuer Registeroperanden immer 0. Im
Instruktionsregister waeren pro Register zwar 4 Bit Platz (= 16 ansprechbare
Register), aber der oberste wird immer null gesetzt. Daher sieht es fuer den
Maschinenprogrammierer so aus, also haette er/sie nur $2^3 = 8$ Register (man
kann mit drei Bits 0 - 2 nur Register 0 - 7 adressieren).

Der Grund dafuer ist, dass man somit 8 Register nur fuer den Mikroprogrammierer
zur Implementierun von Maschinenbefehlen reservieren kann. Diese 8 Register sind
im Rechenwerk die oberen 8, da man ja als Mikroprogrammierer nicht durch die
fixe Null im oberen Bit beschrankt ist.

Maschinenprogrammierer: 0000 - 0111 (8 Werte)
Mikroprogrammierer: 1000 - 1111 (weitere 8 Werte)

Eine Maschineninstruktion kann mit dem Befehl IFETCH geladen werden. Hierbei
wird der Befehlszaehler auf den Adressbus gegeben, die Instruktion von der
Adresse gelesen, in das Instruktionsregister gegeben und der Befehlszaehler
erhoeht.

Mit dem $\overline{IR_LD}$ Signal in Bit $1$ des Maschineninstruktionsworts kann
man das Datum (eine Instruktion) vom Datenbus in das Instruktionsregister laden.

#### Mapping-PROM

Der Mapping-PROM (programmable-read-only-memory) Baustein ist dafuer zustaendig,
den OpCode einer Maschineninstruktion auf die Startadresse des zustaendigen
Mikroprogramms im Mikroprogrammspeicher abzubilden.

Fuer die ERA-Maschine ist diese Abbildung sehr leicht: Der hexadezimal OpCode
wird einfach mit dem Hexadezimalwert $10_{16}$ multipliziert, also um eine
Stelle nach links gerueckt. Man haengt zum OpCode also einfach eine 0 an.

Ist $0xAB$ z.B. ein OpCode, wird er vom Mapping-PROM auf die Adresse $0xAB0$ im
Mikroprogrammspeicher abgebildet.

Der Mapping-PROM hat noch ein $\overline{OE}$ Signal. Dieses wird vom Leitwerk
auf LOW geschalten, wenn es den `JMAP` Adressfortschaltbefehl erhaelt. Dann darf
der Mapping-PROM einen Wert auf den Bus zum D-Eingang des Leitwerks geben.

#### Mikroleitwerk

Das Mikroleitwerk im Leitwerk des Rechners besteht aus aehnlichen Teilen wie das
Maschinenleitwerk bzw. das eigentliche Leitwerk des Rechners:

* Einem Stack
* Einem Mikrobefehlszaehler, fuer die Mikroinstrukionen.
* Einem Adressmultiplexer fuer die Adressfotschaltung.
* Einem Inkrementierer fuer den Mikrobefehlszaehler.
* Ein eigenes Register, z.B. als Zaehler fuer Schleifen.
* Dem 80-Bit breiten Mikroinstruktionsregister.

#### Maschinenbefehlszyklus

Das Leitwerk ist grundsaetzlich fuer den Maschinenbefehlszyklus
zustaendig. Dieser besteht bei jedem Von-Neumann Rechner aus drei Schritten:

1. Befehl aus dem Speicher in das Instruktionsregister laden.
2. Befehlszaehler inkrementieren.
3. Befehl dekodieren.
4. Befehl ausfuehren.

Fuer diesen Rechner, in mehr Detail:

1. Instruktionen werden aus dem Hauptspeicher im Instruktionsregister abgelegt.
2. Der Address-Abbildungs-Speicher (Mapping-PROM) bildet (mapped) dann den
   Opcode Teil der Instrutkion auf die Startadresse eines Mikroprogramms
3. Diese Startadresse liegt im Mikroprogrammspeicher. Sie ist 12-bit gross.
4. Der Mikrobefehlszaehler wird auf diese Addresse gestellt.
5. Und die erste Mikroinstruktion wird dann aus dem Mikroprogrammspeicher
   geholt, vom Mikroleitwerk, und in das Mikroinstruktionsregister gelegt.
6. Das Mikroleitwerk fuehrt den Maschinenbefehl aus.

#### Eingaenge

Es gibt folgende Eingaenge in den Am2910:

* Der $\overline{CC}$ Eingang. Das Signal in diesen Eingang kommt aus der
  Wortrandlogik, welche eine bestimmte Bedingung fuer die Flags ueberprueft und
  das Ergebnis (true oder false, also HIGH oder LOW) an diesen Eingang im
  Leitwerk weiterleitet.

* Der $\overline{CCEN}$ (Condition Code Enable) Eingang. Dieses Signal bestimmt,
  ob das Bedindungssignal am $\overline{CC}$ Eingang fuer die naechste
  Adressfortschaltung beruecksichtigt werden soll, oder nicht. Wenn nicht, wird
  der Befehl immer ausgefuehrt. Ist dieser Eingang also HIGH (`PS` Mnemo), wird
  der Befehl immer ausgefuehrt. Ist dieser Eingang LOW (`C` Mnemo), wird die
  Bedingung beruecksichtigt. Diese Bedingung macht natuerlich nur fuer bestimmte
  Befehle (z.B. bedingte Spruenge) einen Sinn.

* Der $D$ (Data) Eingang. Hier werden Adressen uebergeben, die das Leitwerk fuer
  bestimmte Befehle braucht. Z.B. wird hier die Adresse welche der Mapping-PROM
  fuer einen OpCode bestimmt, und zu welcher das Leitwerk fuer den `JMAP` Befehl
  springen soll. Oder die Verzweigungsadresse (Branch-Address) fuer einen `CJP`
  (deswegen gibt es eine Verbindung vom D Eingang zum Branch-Address Feld im
  Mikroinstruktionsfeld).

#### Adressfortschaltbefehle

Die folgenden Adressfortschaltbefehle koenen dem Mikroleitwerk gegeben werden.

Allgemein ist anzumerken dass viele Befehle die es in Assembler beispielsweise
einmal mit und einmal ohne Bedingung gibt (`JMP` vs `Jcc`), fuer die
mikroprogrammierbare Maschine nur einmal, mit Bedingung gibt. Dann muss man fuer
den unbedingten Fall nur die `PS` Bedingung an das Leitwerk geben ("fuehre
unabhaengig von der Bedingung aus").

##### JMAP

Der `JMAP` Befehl ist dafuer zustaendig, den OpCode eines Maschinenbefehls im
Instruktionsregister auf die Startadresse des zustaendigen Mikroprogramms im
Mikroprogrammspeicher abzubilden. Dieser Befehl muss nach dem Fetchen (`IFETCH`)
jeden Maschinebefehls aus dem Hauptspeicher aufgerufen werden.

##### CONT

`CONT` ist sozusagen eine `nop` Instruktionen. Es inkrementeirt den
Mikrobefehlszaeheler einfach um eins. Dieser Adressfortschaltbefehl ist wohl der
am haeufigsten gebrauchte.

##### CJP

`CJP` (Conditional Jump) ist ein einfacher bedingter Sprung, wie `Jcc` in
Assembler. Ist die "Condition" erfuellt, wird jedenfalls an die Adresse im
Branch-Address Feld der Mikroinstruktion gesprungen.

Er wird in zwei Situationen benoetigt:

1. Unbedingter Sprung. Ein unbedingter Sprung ist am einfachsten durch einen
   bedingten Sprung implementiert, wobei das Signal am $\overline{CC}$ Eingang
   zum Leitwerk einfach auf `PS` (HIGH) gestellt wird. Dadurch ist die Bedingung
   "erfuellt" und das Leitwerk fuehrt den Sprung an die gewuenschte Stelle im
   Mikroprogrammspeicher (steht in Branch-Address) aus.

2. Bedingter Sprung. Hierbei wird die Bedingung am $\overline{CC}$ Eingang nicht
   ignoriert. Daher ist $\overline{CCEN}$ auch LOW (Mnemno `C`). Nur wenn die
   Bedingung (laut Wortrandlogik) erfuellt ist, wird zur Adresse im
   Branch-Addressfeld gesprungen.

##### JZ

`JZ` (Jump Zero) ist ein unbedingter Sprung auf die Adresse `0x0` im
Mikroprogrammspeicher. Das $\overline{CCEN}$ Signal wird also auf `PS` gesetzt
und im Branch-Adress Teil der Mikroinstruktion wird eine Null gespeichert.

`JZ` loescht auch zusaetlzlich den Keller. Dieser wird in ERA nicht benutzt,
also ist es egal, aber dennoch besser `CJP` mit $CCEN = PS$.

##### Weitere

Hier sind noch weitere, fuer die Mikroprogrammierung in der Klausur irrelevante
aber fuer die Fragen zur Vorlesung relevante Fortschaltbefehle:

* `JRP`: Zweiwege-Sprung. Ist die Bedingung wahr, wird zur Adresse in
  Branch-Address gesprugen, sonst zu einer Adresse die in einem der internen
  Register gespeichert ist.

* `CJS`: Wie `CALL`, aber bedingt. Somit kann man das normale `CALL`
  implementieren, wenn die Bedingung `PS` ist, und ansonsten eben nur bedingt
  springen.

* `CRTN`: Wie `RET`, aber bedingt. Somit ist auch wieder eine bedingte und
  unbedingte Form von `RET` moeglich.

Fuer Schleifenbefehle gibt es einige Adressfortschaltbefehle. Grundsaetzlich
wird ein Zaehler in das Zaehlregister geladen. Dann kann wird die
Ruecksprungadresse (zum zurueckspringen am Ende der Schleife wenn der Zaehler
noch nicht null ist) entweder in das Adressfeld der Mikroinstruktion geladen,
oder auf den internen Stack gegeben.

* `LDCT`: Laedt den Schleifenzaehler aus dem Adressfeld (Direktdaten) in das
  Zaehlregister (nicht Befehlszaehler!) des Leitwerks.

* `RPCT`: Dekrementiert den Zaehler im Zaehlregister. Springt dann an die
  Adresse im Adressfeld zurueck, wenn der Zaehler noch nicht null ist.

* `PUSH`: Fuer Schleifen benoetigt, um die Start- bzw. Ruecksprungadresse zu
  speichern. Man legt also zur Vorbereitung fuer eine Schleife die Startadresse
  der Schleife auf den Stack, und springt zu dieser mit bestimmten Befehlen am
  Ende der Schleife wieder zurueck.

* `RFCT`: Dekrementiert den Zaehler im Zaehlregister. Springt dann an die
  Adresse auf dem Stack (POPed diese), wenn der Zaehler noch nicht null ist.

* `LOOP`: Hierbei wird ohne Zaehler gearbeitet, sondern mit einer expliziten
  Bedingung. Ist die Bedingung true (haengt wieder von der Wortrandlogik und dem
  $\overline{CC}$ Eingang ab), wird zur Adresse __auf dem Stack__ gesprungen,
  ansonsten nicht.

### Hauptspeicher

Der Hauptspeicher der ERA-Maschine enthaelt 16-Bit-adressierbare Werte. Man
erreicht den Hauptspeicher ueber zwei Busse:

1. Adressbus: Auf dem Adressbuss werden die Adressen uebertragen, die adressiert
   werden sollen (durch read oder write Operationen).
2. Datenbus: *Nachdem* eine bestimmte Speicherstelle im Hauptspeicher durch die
   auf den Adressbus gelegte Speicherstelle adressiert wurde, werden auf dem
   Datenbus die eigentlichen Daten uebergeben. Bei einer READ Operation legt der
   Hauptspeicher hierbei das Datum an der adressierten Stelle auf den
   Datenbus. Bei einer WRITE Operation muss vom Mikroprogrammierer das zu
   schreibende Datum auf den Datenbus (ueber den Y-MUX) gelegt werden.

Der Hauptspeicher muss __vor__ jeder Lese- oder Schreiboperation (__1. Takt__)
noch aktiviert werden bzw. in den richtigen *Modus* gestellt werden. Hierzu gibt
es zwei Dinge:

1. Das $\overline{CS}$ Signal, welches den Hauptspeicher fuer eine Schreib- oder
   Leseoperation aktiviert. Es wird von der MI-Maschine automatisch verwaltet.

2. Das $\overline{MWE}$ (memory-write-enable) Feld im
   Mikroinstruktionsfeld. Dieses Signal ist Teil einer Mikroinstruktion. Ist
   dieser Bit gesetzt, wird gelesen (Mnemo `R`), ansonsten wird geschrieben
   (Mnemo `W`). Also W(rite) = 0, R(ead) = 1.

__Jeder Hauptspeicherzugriff erfordert zwei (2) Takte__:

1. Takt: anzusprechende Adresse auf den Adressbus geben. $\overline{MWE}$ Signal
   entsprechend setzten.
2. Takt: Daten zum Schreiben auf den Datenbus geben oder Daten beim Lesen vom
   Datenbus lesen.

Somit also das Protokoll zum *Lesen*:

1. Adresse auf den Adressbus. $\overline{MWE} = 1$ setzen.
2. Daten vom Datenbus lesen und entweder in das Instruktionsregister laden
   (`IRLD`), oder via K-MUX in das Rechenwerk senden.

Und zum *Schreiben*:

1. Adresse auf den Adressbus. $\overline{MWE} = 0$ setzen.
2. Daten entweder aus dem Befehlszaehler oder ueber den Y-MUX aus dem Rechenwerk
   auf den Datenbus geben.

### Rechenwerk (AM2901)

Das Rechenwerk ist fuer jegliche arithmetische und logische Operationen des
Rechners zustaendig. Es enthaelt so auch die 16 16-Bit Register des Rechners
(genannt RAM).

#### Bestandteile

##### Hinweis zum Aufbau

In Blockschaltbildern und in Praxis wahrend der Mikroprogrammierung ist dieses
Rechenwerk 16-Bit. Es hat also 16-Bit Register und fuehrt 16-Bit Operationen
durch.

In Wirklichkeit ist der Am2901 aber nur 4-Bit. Es hat nur 4-Bit Register und
kann nur 4-Bit Operationen durchfuehren. Um die 16-Bit zu erhalten, werden also
in Wirklichkeit 4 solcher Am2901 Bausteine seriell verknuepft. In jedem dieser
Bausteine -- genannt "Slice" -- werden 4-Bit eines Registers gespeichert und
jeweils 4 Bit einer arithmetischen Operation verarbeitet. Die Slices sind so
verbunden, dass beispielsweise bei einer Addition der Carry von Baustein zu
Baustein getragen wird.

Es gibt also vier solcher *Slices*, wobei jenes, dass die least-significant
4 Bit verarbeitet, "least-significant-slice" (LSS) genannt wird, und jenes dass
die oberen 4 Bit verarbeitet, "most-significant-slice" (MSS) genannt wird. Alle
Slices dazwischen nennt man "intermediary-slices" (IS).

Die Wortrandlogik (Am2904) muss also seine Flags koordiniert aus den einzelnen
Slices holen. Z.B. muss die Sign-Flag natuerlich aus der MSS kommen, und wenn
die ZF gesetzt werden soll, muessen alle vier Slices null sein.

Die Slices sind auch noch durch *Schiebeeinheiten* verbunden, die die Register
des RAM der einzelnen Scheiben z.B. bei Shiftoperationen verschiebt.

##### ALU

Die arithmetisch-logischen Einheit (ALU) fuehrt die eigentlichen
Rechenoperationen auf Daten durch. Sie kann nur addieren und subtrahieren, nicht
jedoch multiplizieren oder dividieren. Sie kann auch bestimmte logische
Operationen, wie XOR, AND, OR und NEG (negieren, wie NOT in Assembler).

Ihre *Quelloperanden* koennen sein:

* Register A und B aus dem RAM
* Direktdaten aus dem Hauptspeicher oder Konstanten in der Mikroinstruktion
* Eine Null
* Das Q-Register

Die ALU hat zwei Eingaenge, $R$ und $S$, welche die von der
Quelloperandenauswahl selektieren Werte fuer die naechste Operation sind.

Sie hat einen Ausgang: $F$. Ueber diesen wird das Resultat der Operation
zwischen $R$ und $S$ ausgegeben.

Nach jeder Rechenoperation in der ALU werden mehrere Statusflags generiert, die
neben dem Carry-Generate und Carry-Propagate direkt aus der ALU ausgegeben
werden:

* Das $Z$-Bit (*zero flag*) wird immer gesetzt, wenn das Ergebnis der
    ALU-Operation Null ist.
* Das $C$-Bit (*carry flag*) wird bei einem Uebertrag aus dem MSB gesetzt. Also
  dann, wenn der MSB aus den 16-Bit des Datentyps bei einer Addition
  rausgetragen wird oder dann, wenn bei einer Subtraktion ein groesserer Wert
  von einem kleineren abgezogen wird (dann muss aus dem "nichts" ein Bit
  geborrowed werden).
* Das $N$-Bit (*negative flag*). Sowie das Sign-Bit in Assembler. Also eine
  Kopie des MSB.
* Das $OVR$-Bit (*overflow flag*) wird fuer vorzeichenbehaftete Zahlen dann
  gesetzt, wenn die Addition zweier positiver Zahlen eine negative Zahl ergab,
  oder wenn die Addition von zwei negativen Zahlen eine positive Zahl gab. Also
  wenn sich der Sign-Bit bei einer Addition von zwei Zahlen mit selben
  Vorzeichen veraendert hat.

##### RAM

Das Rechenwerk besitzt 16 16-Bit Register, die als RAM bezeichnet werden (haben
nichts mit dem RAM in heutigen Rechnern zu tun).

Es hat zwei Eingaenge, $RA$ und $RB$, mit welchem parallel zwei Register
adressiert werden koennen. Diese Register werden dann ueber Datenausgang $A$
bzw. $B$ ausgegeben. Es ist auch moeglich, dass die Registeradresse an $RA$
gleicher jener an $RB$ ist, dann wird bei $A$ und $B$ jeweils das selbe Register
ausgegeben (`ADD EAX, EAX`).

Es hat auch einen weiteren Dateneingang $D$, ueber welchen Daten in ein
Register geschrieben werden koennen. Hierbei ist es so, dass das Datum am
Dateneingang __immer in die B-Adresse__ geschrieben wird. *Am Ende eines Taktes*
wird das $B$ Register also immer ueberschrieben. Somit gilt fuer jede Operation
auf den Registern $A$ und $B$: $B := A \circ B$, wo $\circ$ eine beliebiege
Verknuepfung ist.

##### QREG

Man kann zwar in der ALU keine Multiplikation oder Subtraktion mit einer
einzigen Operation durchfuehren, aber eben durch Schleifen mit Additionen und
Subtraktion. Bei einer Multiplikation von zwei $n$-Bit Zahlen entsteht ein
Ergebnis von maximal $2n$ Bits. Daher braucht man ein temporaere Register fuer
die oberen 16-Bit wenn man zwei 16-Bit Werte aus Registern oder Konstanten
multipliziert. Dieses Register ist das `QREG` (Q-Register, fuer "Quotient").

Hierbei wird von der Wortrandlogik das Carry-Bit bei jeder Teiladdition einer
Multiplikation in das `QREG` geschoben.

In der Praxis kann man das `QREG` Register auch als
temporaeres 17. Allzweck-Register verwenden. Man kann damit auch addieren und
sonstige Operationen mit anderen Registern oder Daten durchfuehren.

##### Auswahl Ausgabedaten

Dies ist der Baustein, der bestimmt, welcher Wert aus dem Rechenwerk ausgegeben
wird (`Y`-Ausgang). Der ausgegebene Wert kann sein:

1. Das Resultat der Verknuepfung der $A$ und $B$ Register, welches auch
   gleichzeitg in das $B$ Register zurueckgegeben wird. Dieses Resultat wird von
   der ALU ueber den `F` Ausgang ausgegeben, also gibt es die `RAMF` Steuerung
   an den Auswahlbaustein, der diese Daten aus `F` weiter durch seinen `Y`
   Ausgang ausgiebt.
2. Direkt der Wert des $A$-Registers. Hierfuer gibt es die `RAMA` Steuerung. Die
   Werte aus den Registern $A$ und $B$ werden noch immer durch die ALU gegeben,
   verknuepft und wieder zurueck nach $B$ geschrieben. Es wird aber eben immer
   nur der Wert des $A$-Registers ausgegeben.

##### RAM Schiebeeinheit

Dieser Baustein kann genutzt werden, um Daten vor dem Schreiben in den RAM um
*einen Bit* nach links ($\cdot 2$) oder nach rechts ($\div 2$) zu
schieben. Diese Schiebeeinheit ist sehr einfach, kann also wirklich nur um einen
einzigen Bit shiften.

Es gibt fuer dies Einheit die ALU-Befehle `RAMD` (shift "down"; $\div 2$) und
`RAMU` (shift "up"; $\cdot 2$). Somit kann man z.B. einen Wert in einem Takt mit
vier multiplizieren, ohne wirklich einen Multiplikationsoperator zu haben:

1. Addieren den Wert (aus dem Register) auf sich selbst (geht wegen der beiden
   RAM Ausgaenge, welche beide den selben Wert ausgeben koennen). Das ergibt das
   Doppelte der urspruenglichen Werts.
2. Schiebe den Wert vor dem Zurueckspeichern um einen Bit nach links mit der
   `RAMU` Operation. Somit erhaelt man das Doppelte des Doppelten, also den Wert
   mal vier.

##### Y-Schiebeeinheit

Wie die RAM-Schiebeeinheit, aber fuer das `QREG`. Hierfuer gibt es die
ALU-Zielsteuerungsbefehle `RAMQU` und `RAMQD`.

##### Quelloperandenauswahl

Der Quelloperandenauswahlbaustein erhaelt als Eingang alle moeglichen
Quelloperanden:

* Den Dateneingang $D$
* Die Registerwerte $A$ und $B$
* Die Null
* Den Wert des Q-Registers

Und selektiert je nach Quelloperandensteuerung, welche es an die ALU
weitergibt. Es hat also zwei Ausgaenge $R$ und $S$, welche eben die beiden von
den fuenf selektierten Werte sind.

##### 0

Es gibt ein Signal in die ALU rein, dass nur den Wert $0$ ausgibt. Somit kann
man ohne explizite Konstante eine Operation mit $0$ durchfuehren. Das ist
z.B. nuetzlich, um einen Wert, der in einem Register gespeichert ist, durch die
ALU ohne jegliche Veraenderung zu schleusen, um es in den Hauptspeicher zu
schreiben. Man fuehrt dann einfach eine `OR`, `XOR` oder `ADD` Operation
zwischen dem Registerwert und der $0$ durch, dann bleibt der Registerwert
unveraendert.

##### Carry Lookahead

Der Carry Lookahead Baustein Am2902 hat die Rolle nachzusehen, ob eine der vier
Bit-Slices bei der momentanen Addition eineen Uebertrag generieren wird. Der
Sinn ist, dass man bei einer binaeren Addition nicht die oberen Bits addieren
kann, bevor man nicht den Carry der unteren Bits weiss. Hat man nun also 16
Bits, muss man somit 16 Zyklen warten, da man fuer jeden Bit auf das Carry der
Addition der vorherigen zwei Bits warten muss.

Nun gibt es aber den Fall, dass die letzten beiden Bits der zu addierenden Werte
beide jeweils $1$ sind. Dann wird eine Bit-Slice *jedenfalls* einen Uebertrag
*generieren*. Daher koennte in diesem Fall diese Information sofort zur
naechsten Slice gegeben werden, und somit die Addition der beiden 4-Bit Gruppen
parallel durchgefuehrt werden. Genau deswegen hat jeder Am2901 Baustein einen
$\overline{G}$ Ausgang, fuer "Generate", der angibt, ob die letzten beiden Bits
der Werte $1$ sind. Der Carry Lookahead Baustein sieht sich dieses Signal an,
und gibt es weiter an die naechste Slice (oder die Wortrandlogik im Falle der
MSS).

Nun gibt es aber auch den Fall, dass die vorletzten beiden Bits beide eins
sind. Dann wird im letzten Bit ein Carry generiert, wenn zumindest einer der
Werte im letzten Bit eine eins hat. In diesem Fall sagt man, dass der *letzte*
Bit *propagiert* ("propagate"). Dafuer gibt es das *propagate* Signal
$\overline{P}$.

Das geht jetzt also fuer alle vier Bitstellen, wobei man also immer die Bits $i$
und $i - 1$ betrachtet und dann die jeweiligen Signale fuer $i + 2$ und $i + 1$
berechnet. Somit kann der Carry Lookeahead also schon im vorhinein berechnen, ob
es einen Carry geben wird (wenn $\overline{P} + \overline{G}$), und das an die
weiteren Bit Slices weitergeben.

https://en.wikipedia.org/wiki/Carry-lookahead_adder

#### Eingaenge

Der Am2901 hat folgende Eingaenge:

* Den Dateneingang $D$. Ueber diesen Eingang koennen Daten aus dem Hauptspeicher
  (bzw. vom Datenbus) oder Konstanten in der Mikroinstruktion als Quelloperand
  fuer die ALU selektiert werden. Von welcher Quelle genau, haengt dann vom KMUX
  ab.
* Den Registeradresseingang $A$. Ueber diesen wird festgelegt, welches Register
  im RAM ueber den A-Ausgang des RAM ausgegeben werden soll. Diese
  Registernummer kann entweder aus dem Instruktionsregister bzw. der
  Maschineninstruktion kommen, oder aus der Mikroinstruktion. Das bestimmt der
  A-MUX.
* Den Registeradresseingang $B$. Ueber diesen wird festgelegt, welches Register
  im RAM ueber den B-Ausgang des RAM ausgegeben werden soll. Diese
  Registernummer kann entweder aus dem Instruktionsregister bzw. der
  Maschineninstruktion kommen, oder aus der Mikroinstruktion. Das bestimmt der
  B-MUX.
* Neun Instruktionsbits $I_0, ..., I_8$. Diese sind die Steuerungsbefehle fuer
  die ALU Zielsteuerung, Funktionssteuerung sowie Quellsteuerung.
* Einen Carry-In Eingang $C_n$. Ueber diesen kann ein Carry-Signal von der
  Wortrandlogik in die naechste Operation der ALU eingespeist werden. Dies ist
  insofern nuetzlich, da man damit nicht nur den konstanten Quelloperanden $0$
  haben kann, sondern auch einen konstanten Quelloperanden $1$. Dazu ist einer
  der Quelloperanden einfach `Z`, und das Carry-Bit $1$ (so implementiert man
  z.B. ohne Konstante in der Mikroinstruktion den Befehl `INC`). Bestimmt wird
  dieses Bit ueber die Bits $I_{11,12}$ einer Mikroinstrukion, welche auf `CI0`
  und `CI1` gesetzt werden koenen.
* Verbindungen zu den RAM bzw. Q Schiebeeinheiten.

#### Ausgaenge

Der AM2901 hat folgende Ausgaenge:

* Den Datenausgang $Y$, welcher stets das Ergebnis der Operation in der ALU
  ausgibt (`RAMF`), oder direkt den Wert des A-Registers (`RAMA`).
* Ausgaenge fuer die vom Ergebnis der letzten Rechenoperation abhaengigen
  Statusbits.
* Verbindungen fuer die RAM bzw. Q Schiebeeinheiten.

#### Steuerung

Hier sind die Moeglichkeiten zur Steuerung des Rechenwerks beschrieben.

##### Quelloperandensteuerung

Die Quelloperandensteuerung bezieht sich darauf, welche zwei Werte fuer die
naechste arithmetische Operation in der ALU verwendet werden. Grundsaetzlich
koennen dies sein:

* Register aus dem RAM
* Das Q-Register
* Daten vom D-Eingang
* Die Null (Zero)

Hierbei gibt es die folgenden Befehle, die die drei Bits $55$ bis $57$ in jeder
Mikroinstruktion belegen:

1. `AQ`: Register A und das Q-Register
2. `AB`: Register A und Register B
3. `ZQ`: Die Null und das Q-Register
4. `ZB`: Die Null und das B-Register
5. `ZA`: Die Null und das A-Register
6. `DA`: Der Dateneingang und das A-Register
7. `DQ`: Der Dateneingang und da Q-Register
8. `DZ`: Der Dateneingang und die Null

Das wichtigste hier ist, dass es keinen `DB` Befehl gibt. Will man also den Wert
im $RB$ Teil eines Maschinenbefehls mit einer Konstante verknuepfen, muss man
das mit $RB$ adressierte Register zuerst in eins der $8$ dem Mikroprogrammierer
zur Verfuegung stehenden Register, oder dem Q-Register, schreiben. Erst danach
kann man entweder die Adresse des temporaeren Registers in das
Register-Adressfeld $A$ der Mikroinstrukionen schreiben und danach `AQ`
durchfuehren, oder eben fuer das Q-Register den Befehl `DQ`.

Der Grund fuer diese Restriktion ist, dass es nur $3$ Bits fuer diese Steuerung
gibt. Die Architekten waren einfach zu geizig mit ihren Bits.

Auch: will man mit der $0$ eine Operation durchfuehren, muss man sicherstellen,
dass der Carry Eingang der ALU unbedingt $0$ ist.

##### Funktionssteuerung

Die Funktionssteuerung der ALU bestimmt, mit welcher Operation die beiden
Quelloperanden verknuepft werden. Jedenfalls landet das Ergebnis im durch $RB$
adressierten Register.

1. `ADD`: $A + B$
2. `SUBR`: $S - R$
3. `SUBS`: $R - S$
4. `OR`: $R \lor S$
5. `AND`: $R \land S$
6. `NOTRS`: $\overline{R} \land S$
7. `EXOR`: $R \oplus S$ (XOR)
8. `EXNOR`: $\overline(R \oplus S) = R \leftrightarrow S$ (XNOR, Bidirektional)

__Wichtig__:

Fuer `SUBS` und `SUBR` gibt es eine wichtige Eigenschaft der ALU, die man
beachten muss. Eine Subtraktion $R - S$ wird grundsaetzlich nicht durch eine
echte binaere Subtraktion durchgefuehrt. Eher wird $R - S$ als $R + (-S)$
implementiert, also Addition von $R$ mit dem *Zweierkomplement* von $S$. Hierzu
wird aber zunaechst das *Einerkomplement* $\overline{S}$ von $S$ gebildet und
dann die Addition $R + \overline{S}$ durchgefuehrt. Da $\overline{S} = -S - 1$,
ist diese Addition aber noch keine vollstaendige Subtraktion: $R + \overline{S}
= R + (-S - 1)$. Deswegen muss der Mikroprogrammierer *manuell* noch das
Carry-Bit setzten (`CI1`), um somit die Addition $R + \overline{S} + 1 = R + (-S
- 1) + 1 = R - S$ zu vollenden. Anzumerken ist, dass fuer das Carry Flag bei
der Operation $R + (-S)$ genau das inverse der Carry-Flag fuer $R - S$ (bei
normaler binarere Subtraktion mit borrow) rauskommt. Daher kommt auch das
inverse Carry bei der Subtraktion raus (im Vergleich zu x86).

Gleichzeitig: Wegen der oben angefuehrten Eigenschaft kann man in nur einem
Takt vom $RB$ Register (fuer welches eine Konstante $1$ wegen dem `DB`-Problem
nicht geht) eine $1$ abziehen, indem man ohne die Carry Flag zu setzen eine $0$
abzieht:

Die ALU rechnet $RB - 0$ als $RB + \overline{0} = RB + (-0 - 1) = RB - 1$.

##### Zielsteuerung

Die Zielsteuerung bestimmt, was mit dem Resultat der letzten ALU-Operation
passiert, und welche Werte vom Rechenwerk ueber den $Y$-Ausgang ausgegeben
werden. Grundsaetzlich kann entweder das Resultat der letzten ALU-Operation vom
$F$ Ausgang der ALU ausgegeben werden, oder direkt der Wert des
$A$-Registers.

Es gibt folgende Befehle:

1. `QREG`: Gib das Ergebnis aus der ALU ($F$-Ausgang) in das
   $Q$-Register und an den $Y$-Ausgang.

2. `NOP`: Gib das Ergebnis aus der ALU aus und schreibe *nichts* zurueck in das
   $B$-Register.

3. `RAMA`: Gib den Wert des $A$-Registers aus und schreibe das Ergebnis der ALU
   ins $B$-Register.

4. `RAMF`: Gib das Ergebnis der ALU aus und schreibe es auch zurueck in
   das $B$-Register.

5. `RAMQD`: Schiebe das Ergebnis der ALU um einen Bit nach rechts ($\div
   2$) und speichere es im __$B$-Register__. Schiebe *gleichzeitig* den Wert des
   $Q$-Registers um einen Bit nach rechts (unabhaengig).

6. `RAMD`: Schiebe das Ergebnis der ALU um einen Bit nach rechts ($\div
   2$) und speichere es im __$B$-Register__.

7. `RAMQU`: Schiebe das Ergebnis der ALU um einen Bit nach links ($\cdot
   2$) und speichere es im __$B$-Register__. Schiebe *gleichzeitig* den Wert des
   $Q$-Registers um einen Bit nach links (unabhaengig).

8. `RAMU`: Schiebe das Ergebnis der ALU um einen Bit nach links ($\cdot
   2$) und speichere es im __$B$-Register__.


(1) ist nuetzlich, um Werte ohne Veraenderung durch die ALU zu
schleusen. Z.B. fuer das $DB$ Problem eine Moeglichkeite.

### Wortrandlogik (Am2904)

Der Wortrandlogikbaustein Am2904 ist in der ERA-Maschine zustaendig fuer:

1. Die Speicherung der vier Statussignale $ZF, CF, NF, OVF$ im Mikro- und
   Maschinenstatusregister. Diese Signale werden von der ALU nach jeder
   arithmetischen Operation generiert.

2. Abfragen von Bedingungen in Abhaengikeit der Statusflags in der *Testlogik*.
   Die Statusbits des Bausteins koenen so verknuepft werden, dass daraus ein
   Bedingungssignal $CT$ erzeugt wird, das dann an den $\overline{CC}$ Eingang
   des Leitwerks Am2910 z.B. fuer bedingte Spruenge, weitergegeben wird.

3. Uebertragssteuerung fuer die ALU ($C_n$ Eingang).

4. Schiebesteuerung zwischen den vier einzelnen Bit-Slices des Rechenwerks. Es
   verwaltet also das Schieben zwischen den RAM- und Q-Schiebeeinheiten der
   jeweiligen Am2901 Bausteine, die zusammen das Rechenwerk ausmachen.

#### Bestandteile

##### Statusregister

Die Wortrandlogik verwaltet zwei Statusregister:

1. Das Mikrostatusregister $\muSR$, fuer den Mikroprogrammierer.
2. Das Maschinenstatusregister $MSR$, fuer den Maschinenprogrammierer.

Diese Differenzierung ist wie jene fuer die fuer den Maschinenprogrammierer
sichtbaren $8$ Register, und die fuer den Mikroprogrammierer sichtbaren $8$
Register. Hier kann man nun also bestimmte Flags in das Mikrostatusregister
laden, um damit z.B. bedingte Spruenge im Leitwerk durchzufuehren. Gleichzeitig
muessen dabei nicht die Statusflags des Maschinenprogrammiers veraendert werden.

Mit den Signalen $\overline{CE_{\mu}}$ und $\overline{CE_M}$ kann das Laden von
Flags (siehe unten) fuer eines der Statusregister oder beide verhindert
bzw. zugelassen werden.

##### Testlogik

Die Testlogik ist innerhalb des Am2904 jener Baustein, der aus den Statusbits
des Mikro- oder Maschinenstatusregisters ein Bedingungs- bzw. *Testsignal* $CT$
generiert.

##### Uebertragssteuerung

Die Uebertragssteuerung des Am2904 bestimmt, welches Signal an den Carry-Eingang
$C_n$ der ALU im Rechenwerk (Am2901) gelangt. Es kann manuell via den
mnemotechnsichen Codes `CI1` gesetzt bzw. mit `CI0` in den Bits $I_{11,12}$ des
Maschineninstruktionsworts gestzt werden.

#### Eingaenge

Die Wortrandlogik besitzt folgende Eingaenge:

* Das Enable-Signal $\overline{CE_{\mu}}$ fuer das Mikrostatusregister.

* Das Enable-Signale $\overline{CE_M}$ fuer das Maschinenstatusregister.

* Vier Eingaenge fuer die von der ALU generierten Statusbits:
  * $I_N$ fuer die *negative-flag*.
  * $I_Z$ fuer die *zero-flag*.
  * $I_C$ fuer die *carry-flag*.
  * $I_{OVR}$ fuer die *overflow-flag*.

* Eingaenge fuer die Uebertragssteuerung der Wortrandlogik von den Bits
  $I_{11,12}$ der Maschineninstruktion.

* Sechs Eingangsbits $I_{5, 4, 3, 2, 1, 0}$ welche bestimmen, welche der
  anstehenden Statusbits $I_{N,C,Z,OVR}$ wirklich in das Mikro- oder
  Maschinenstatusregiser geladen werden, und welche Bedingung mittels den
  Statusflags ueberprueft werden soll.

#### Ausgaenge

Die Wortrandlogik hat zwei wichtige Ausgaenge:

* Den Ausgang $C_0$, der den Uebertrag fuer den $C_n$ Eingang der ALU enthaelt.

* Den $CT$ Ausgang, der das Bedingungssignal (das Ergebnis des Ueberpruefens der
  letzten Bedingung) an den $\overline{CC}$ Eingang des Leitwerks weiterleitet.

#### Befehle

Eine wesentliche Komplikation des Am2904 ist, dass verschiedene
Instruktionswoerter an den Eingaengen $I_{5,4,3,2,1,0}$ nicht nur jeweils eine
einzige Aktion ausfuehren. Die Bitkombination $001101_2$ laedt z.B. nicht nur
das Maschinenstatusregister mit den von der ALU erzeugten Statusbits, sondern
setzt auch das $N$-Bit im Mikrostatusregister ($\mu_N$) und sendet eine
`PASS`-Bedingung an das Leitwerk, wenn $\overline{\mu_C} \lor \mu_Z$
gilt. Daher sollte man darauf achten, die Enable-Signale $\overline{CE_{\mu}}$
und $\overline{CE_M}$ wirklich nur dann auf LOW (also enable) zu stellen, wenn
man die Statusregister wirklich laden moechte.

Bei der Programmierung des Am2904 ist ausserdem das Zeitverhalten zu
beruecksichtigen: Die Statusregister werden immer *am Ende* einer
Mikroinstruktion *gesetzt*, waehrend *Statusabfragen am Anfang*
erfolgen. __Zwischen dem Setzen eines Statusregisters und der Abfrage mit Hilfe
der Bedingungslogik muss also immer mindestens ein Takt vergehen.__

##### Laden von Statusbits

Die von der ALU generierten Statussignale $I_{N,C,Z,OVR}$ stehen nach jeder
Operation in der ALU an den entsprechenden Eingaengen zur Wortrandlogik an. Es
gibt nun eine ganze Menge verschiedene Bits, die man setzen kann, um alle oder
nur bestimmte Flags zu laden. In der Klausur ist es uns aber erlaubt, einfach
`LOAD` zu schreiben, um alle Eingangssignale $I_{N,C,Z,OVR}$ in die
Wortrandlogik durchzulassen. Ob die Statusflags in ein Statusregister geladen
werden, sei es das Mikro- oder das Maschinenstatusregister, bestimmt man dann
also mit den $\overline{CE_{\mu}}$ bzw. $\overline{CE_M}$ Signalen. Man setzt
ein solches Signal im selben Takt wie das `LOAD` also LOW bzw. mnemotechnisch
auf `L`, um die Statusflags in das Mikro- oder Maschinenstatusregister zu laden.

Diese Ladeoperation passiert immer *am Ende* eines Maschinentakts.

##### Ueberpruefen von Bedingungen

Nachdem man Statusflags am Ende der letzten Mikroinstruktion in die
Statusregister geladen hat, kann man dann am Anfang des naechsten Takts die
Statusflags in der Testlogik auf bestimmte Weisen verknuepfen, um ein
Bedingungssignal $CT$ zu generieren, welches das Leitwerk dann im
$\overline{CC}$ Eingang erhaelt und fuer bedingte Spruenge evaluieren kann. Wie
die Statusbits verknuepft werden und welche Bedingung dadurch ueberprueft wird,
sieht man in der folgenden Tabelle:

| Bedingung  | Status                     | Mnemo   |
|:-----------|:---------------------------|:--------|
| $A = B$    | $Z = 1$                    | Zero    |
| $A \neq B$ | $Z = 0$                    | NotZero |
| $A \geq B$ | $C = 1$                    | UGTEQ   |
| $A < B$    | $C = 0$                    | ULT     |
| $A > B$    | $C \land \overline{Z} = 1$ | UGT     |
| $A \leq B$ | $\overline{C} \lor Z = 1$  | ULTEQ   |

Hierbei ist es __sehr wichtig__, vor jeder Bedingung noch __MA oder MI__ zu
schreiben um zu bestimmen, ob die Statusflags des Mikrostatusregisters oder des
Maschinenstatusregisters nach der Bedingung ueberprueft werden. Zum Beispiel:
`MA UGTEQ` um die Bedingung $A \geq B$ fuer die Maschinenflags zu testen.

Diese Bedingungen sind nur fuer *unsigned* Werte. *signed* behandeln wir in ERA
nicht (erst im Praktikum).

Nachdem man im letzten Takt also die Satusbits geladen hat, kann man im neuen
Takt eine dieser mnemotechnischen Vereinbarungen also in den Teil der
Mikroinstruktions geben, der fuer den Am2904 voregesehen ist. Hierbei sollte man
__immer die Enable-Bits auf `H` setzen__ (also $\overline{CE_{\mu}} = H$ und
$\overline{CE_M} = H$). Dann muss man zum bedingten Springen im Leitwerk noch
das Condition-Code-Enable-Bit $\overline{CCEN}$ auf `C` setzen und den bedingten
Adressfortschaltung (`CJP`, `CJS`, `CRET`, `LOOP` etc.) auch noch eintragen.

##### Uebertragssteuerung

Da die Wortrandlogik neben den Statusregistern, der Testlogik und der
Schiebesteuerung auch noch die Uebertragssteuerung fuer die ALU uebernimmt, gibt
es noch zwei wichtige Bits fuer den Uebertrag:

1. `CI0`: Carry auf Null gesetzt (Fuer Addition mit Null in ALU essentiell!)
2. `CI1`: Carry auf Eins gesetzt (Fuer Subtraktion in ALU essentiell!)

Diese Steuersignale belegen die Bits $I_{11,12}$ des Eingangsworts.

## Mikroinstruktionsformat

Jede Mikroinstruktion ist 80-Bit breit, und besteht aus den folgenden Bits:

0: $\overline{MWE}$: Memory-Write-Enable, zum Lesen/Schreiben des
   Hauptspeichers. $W := 0, R := 1$.

1: $\overline{IR_LD}$: Instruction-Register-Load. Laedt eine Instruktion vom
   Datenbus in das Instruktionsregister. $L := 0, H := 1$.

2: $\overline{BZ_EA}$: Befehlszaehler-Enable-Adressbus. Legt den Befehlszaehler
   auf den Adressbus. $E := 0, H := 1$

3: $\overline{BZ_INC}$: Befehlszaehler-Increment. Erhoeht den Behlszaehler um
   $1$. $I := 0, H := 1$.

4: $\overline{BZ_ED}$: Befehlszaehler-Enable-Datenbus. Legt den Befehlszaehler
   auf den Datenbus. $E := 0, H := 1$

5: $\overline{BZ_LD}$: Befehlszaehler-Load. Laedt den Befehlszaehler vom
   Datenbus (z.B. fuer Spruenge). $L := 0, H := 1$

6..17: $BAR$: Branch-Address. Zum Speichern von Sprungadressen in den MPS fuer
       Spruenge, oder als Wert fuer das Zaehlregister des Leitwerks (`LDCT`).

18..21: $I_{0,1,2,3}$: Adressfortschaltbefehle des Leitwerks (z.B. `CJP`, `JRP`)

22: $\overline{CCEN}$: Condition-Code-Enable. Bestimmt, ob die Bedingung am
    $\overline{CC}$ Eingang des Leitwerks beruecksichtigt wird. $C := 0, 1 := PS$

23..28: $I_{0,1,2,3,4,5}$: Statusregisterbefehle bzw. Tests. Zur Steuerung der
        Wortrandlogik (Statusflags, Testlogik).

29: $\overline{CE_M}$: Maschinenstatusregister-Enable. Laedt die Flags an den
    Eingangen $I_{N,C,Z,OVR}$ der Wortrandlogik in das Maschinenstatusregister.

30: $\overline{CE_{\mu}}$: Mikrostatusregister-Enable. Laedt die Flags an den
    Eingangen $I_{N,C,Z,OVR}$ der Wortrandlogik in das Mikrostatusregister.

31..34: $I_{6,7,8,9}$: Schiebesteuerung der Wortrandlogik.

35..36: $I_{11,12}$: Uebertragssteuerung der Wortrandlogik. Wichtig sind nur die
        `CI0` und `CI1` Befehle (Carry $0$ bzw. $1$).

37: $\overline{DBUS}$: Legt die Daten vom $Y$-MUX (Ergebnis der ALU) auf den
    Datenbus. $DB := 0, H := 1$

38: $\overline{ABUS}$: Legt die Daten vom $Y$-MUX (Ergebnis der ALU) auf den
    Adressbuss. $AB := 0, H := 1$

39: $\overline{BSEL}$: Register-B-Select. Selektiert zwischen dem
    B-Register-Feld ($RB ADDR$) in der Mikroinstruktion und dem B-Register Teil
    des Maschinenbefehls im Instruktionsregister als $RB$-Eingang zum RAM des
    Rechenwerks. Wenn $BSEL = MR$ wird $RB ADDR$ zum Rechenwerk gegeben, sonst
    wenn $BSEL = IR$ wird $RB$ aus dem Maschinenbefehl zum Rechenwerk gegeben.

40..43: $RB \, ADDR$: Register-B-Adresse. Gibt die konstante Adresse fuer den
        $RB$-Eingang zum RAM des Rechenwerks fest, wenn $BSEL = MR$. Man traegt
        hier eines aus $r_0,...,r_{15}$ ein.

44: $\overline{ASEL}$: Register-A-Select. Selektiert zwischen dem
    A-Register-Feld in der Mikroinstruktion und dem A-Register Teil des
    Maschinenbefehls im Instruktionsregister als $RA$-Eingang zum RAM des
    Rechenwerks. Wenn $ASEL = MR$ wird $RA ADDR$ zum Rechenwerk gegeben, sonst
    wenn $ASEL = IR$ wird $RA$ aus dem Maschinenbefehl zum Rechenwerk gegeben.

45..48: $RA \, ADDR$: Register-A-Adresse. Gibt die konstante Adresse fuer den
        $RA$-Eingang zum RAM des Rechenwerks fest, wenn $ASEL = MR$. Man traegt
        hier eines aus $r_0,...,r_{15}$ ein.

49..51: $I_{6,7,8}$ ALU-Zielsteuerung. Legt fest, wohin das Ergebnis der letzten
        Operation in der ALU geschrieben wird und welcher Wert vom Rechenwerk
        ueber den $Y$-Ausgang ausgegeben wird.

52..54: $I_{3,4,5}$: ALU-Funktionssteuerung. Legt fest, mit welcher Operation
         Werte an den Eingaengen $R$ und $S$ zur ALU verknuepft werden
         (z.B. `ADD`).

55..57: $I_{0,1,2}$: ALU-Quelloperandensteuerung. Legt fest, welche Werte in die
        $R$ und $S$ Eingaenge der ALU gegeben werden ($\in \{A, B, Z, D, Q\}$).

58..73: $K_{0,...,15}$: 16-Bit Konstante. Zum Beispiel, um eine Operation mit
         einem fixen zweiten Operanden durchzufuehren.

74: $KMUX$: Mux fuer den $D$-Eingang des Rechenwerks. Legt fest, ob der Wert am
    $D$-Eingang des Rechenwerks vom Konstantenfeld ($KMUX = K$) oder vom
    Datenbus ($KMUX = D$) kommt.

75..78: Interruptsteuerung. Irrelevant fuer ERA.

79: Interupt Enable. Erlaubt Unterebrechungen. Irrelevant fuer ERA.

## Gotchas

* Bei Subtraktion Carry Bit nicht vergessen (`CI1`)
* Befehlszaehler laden/inkrementieren und jump zu IFETCH geht im selben Takt.
* Ruecksprung zu IFETCH am Enden icht vergessen.
* Nach IFETCH zeigt der BZ schon auf die naechste Instruktion/Adresse.
* Bei Spruengen Befehlszaehler schon im ersten Takt auf den Adressbus geben!
* Bei Berechnung von Takten/Speicherzugriffen auch IFETCH beachten!!
* Die Adressen sind 16-Bit beim Assemblieren, also 1 Adresse = 16 Bit (0x01,
  0x02 nicht 0x01, 0x03).
* OpCodes in der Tabelle nicht vergessen auszufuellen.
* INC/DEC geht mit Carry Flag!
