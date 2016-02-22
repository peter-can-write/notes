# ISA

Die Instruction Set Architecture (ISA) eines Rechners beschreibt die grundlegende
Architektur, zum Beispiel:

* Die unterstuetzten Maschinenbefehle, genauer:
  + Die Anzahl der expliziten und impliziten Operanden
  + Der Speicherort der Operanden (Hauptspeicher, Register, Konstanten)
  + Der Zielort der Operation.

* Die Adressierungmodi.
* Die Wortgroesse des Prozessors.
* Datentypen.
* Die verfuegbaren Register.
* Das Befehlsformat der Instruktionen.
* Die Ausfuehrungsmodi.
* Behandlung von Unterbrechungen (Interrupts).

Es gibt nun zwei Hauptklassen von ISAs: RISC und CISC.

## CISC

CISC steht fuer *Complex Instruction Set Computing* und ist jene Klasse von
ISAs, die ueberlicherweise eine (im Vergleich zu RISC):

* Sehr groesse Anzahl an Befehlen
* Sehr groesse Anzahl an Adressierungsmodi
* Sehr groesse Anzahl an Befehlsformaten

erlaubt.

Der Vorteil davon ist, das Programmierung auf Grund der "fetten API" angenehmer
sein kann.

Ein Nachteil kann aber auch sein, dass die ISA zu komplex scheint und viele
Funktionen unbenutzt gehen. Ebenso kann eine solche ISA langsam sein, da man
viel Zeit fuer Dekodierung braucht.

## RISC

RISC steht fuer *Reduced Instruction Set Computing* und ist bezueglich der
Komplexizitaet der ISA das genaue Gegenstueck zu CISC. Es hat:

* Viel weniger Befehle.
* Oftmals nur ein fixes Befehlsformat.
* Weniger Adressierungsmodi.

Der Vorteil davon ist die einfache, effiziente und schnelle Implementierung
solcher Rechner, sowie eine schnelle Interrupt-Behandlung auf Grund der geringen
Komplexizitaet des Rechners.

Ein Rechner mit RISC Architektur kann schwieriger zu programmieren sein, was
aber meist der Compiler vor einem verdeckt.

## Datentypen

Eine ISA legt auch die erlaubten Datentypen fuer ein Maschinenprogramm fest
(z.B. `byte`, `word`, `dword`). Somit legt sie also die Arten von Informationen
fest, die mit dem Befehlssatz *direkt* bearbeitet werden koennen.

Hierbei sei jedoch angemerkt, dass im Hautpspeicher keinerlie Typinformation
gespeichert ist. Im Hauptspeicher findet man nur untypisierte Bytefolgen. Das
Format und die Interpretation von Daten sowie deren Typen geht also allein aus
dem Verwendungskontext hervor.

Die Interpretation von Daten wird durch Befehle durchgefuehrt, die diese Daten
bearbeiten. Hoehere bzw. komplexere Datentypen, die von der Hardware nicht
unterstuetzt werden, muessen dann also auf die von der ISA unterstuetzten
elementaren Datentypen zurueckgefuehrt werden. Das wird fuer Hochsprachen wie C
oder Java in der Regel vom Compiler uebernommen. Manchmal erlaubt die ISA aber
auch Datentypen, die sie von ihrer Grundhardware nicht unterstuetzt, aber mit
Hilfe von Software Emulation oder Coprozessoren dennoch verarbeitet werden
koennen.

Die meisten heutigen Intel Architekturen erlauben folgende Datentypen:

* Byte: 8-Bit
* Word: 16-Bit
* DoubleWord: 32-Bit
* QuadWord: 64-Bit

Ein *Wort* wird allgemein als die kleinste im Speicher adressierbare und vom
Prozessor verarbeitbare Einheit angesehen. Bei einem 64-Bit MacBook ist die
Wortgroesse beispielsweise 64 Bit. Somit kann man eben *maximal* 64-Bit grosse
Datenbloecke im Speicher adressieren und verarbeiten. Die Wortgroesse legt
gleichzeitig auch die Maximalgroesse des Speichers fest, da man mit einen
Wortgroesse von $n$ Bit maximal einen Raum von $2^n$ Werten abtasten kann. Das
ist auch ein Grund, wieso es noch keine Rechner mit $128$ Bit Wortgroesse gibt:
weil es noch kein Speichersystem mit $2^{128} \approx 3.4028 \cdot 10^{38}$
Bytes gibt.

Das ein Wort in Intel Nomenklatur $16$ Bit ist, ist nur eine historische Norm.

In manchen CISC Architekturen gibt es noch andere exotischere Datentypen:

* Bit
* Nibble: 4 Bit
* Bitfeld mit variabler Bitanzahl
* String: Bytefolge mit variabler Laenge

## Speicherorganisation

Nun einige Worte zur grundlegenden Speicherorganisation einer ISA.

Bei praktisch allen Rechnern ist der Speicher byte-adressierbar. Ein Byte ist
also die im Von-Neumann Architekturkonzept festgelegte feste Groesse von Zellen
im Hauptspeicher. Dennoch gibt es einige Unterschiede bezuegliche *wie* Daten im
Speicher abgelegt werden.

### Ausrichtung

Die *Ausrichtung* ("Alignment") von Daten bezieht sich auf die Position eines
Datums im Speicher, in Abhaengigkeit von der groesse des Datums.

Wir nennen ein Datum $n$-Byte-Ausgerichtet, wenn seine Startadresse $S$ im
Speicher ein ganzes Vielfaches von $n$ ist, sodass $S \mod n = 0$. Hierbei wird
allgemein angenommen, dass Daten Groessen haben, die Zweierpotenzen sind. Ein
Beispiel waere, dass ein DoubleWord Datum (32-Bit) auf einer Adresse im
Hauptspeicher liegt, die ein Vielfaches von $32$ ist, z.B. $0, 32, 64$ oder
$96$.

Ist ein Datum nicht ausgerichtet und der Prozessor muss darauf zugreifen,
sprechen wir von einem unausgerichteten Zugriff bzw. *misaligned* oder
*unaligned* Access. Wenn nun ein Datum mit einer Groesse von $n$ Bytes also ab
einer Adresse gespeichert ist, die kein ganzes Vielfaches von $n$ ist, muss der
Prozessor ueber mehrer Huerden springen, um das Datum zu holen.

Sei z.B. die Wortgroesse des Prozessors $4$ Bytes. Nun liegt ein
unausgerichtetes Datum von $4$ Bytes an Adresse $0x1$ -- eben nicht ein
Vielfaches von $4$:

```
0x0 [ ... ]
0x1 [  O  ]
0x2 [  O  ]
0x3 [  O  ]
---------------- Wortgrenze
0x4 [  O  ]
0x5 [ ... ]
```

Wie man sieht liegt das Datum so, dass es eine Wortgrenze ueberschreitet. Diese
Grenze ist natuerlich nur imaginaer und nicht physisch sichtbar. Der Prozessor
hat also Wortgroesse $4$ und kann daher immer nur $4$ Bytes lesen, und immer nur
beginnend an Startadressen die $4$-ausgerichtet sind. Um sich also das Datum mit
den `O`s zu holen, muss es zuerst das Wort von Adresse $0x0$ bis $0x3$ holen,
und erst danach das Wort von Adresse $0x4$ bis $0x7$.

Nun muss der Prozess aber noch weitere Arbeit leisten, da man von den $8$
gelesenen Bytes ja nur die relevanten $4$ Byte will, und zwar in richtiger
Reihenfolge. Also muss der Prozessor beide Woerter an eine temporaere Stelle
speichern. Dann muss das erste Wort um einen Byte nach rechts (hier nach oben)
geschoben werden, und das zweite Wort um drei Bytes nach links (hier nach
unten). Erst dann koennen die beiden Woerter in einem Register vereinigt
werden.

Waere das Datum schon am Anfang an Adresse $0x0$ gelegen, haette man das ganze
Datum ohne weitere Verarbeitung mit nur einem Zugriff lesen koenen. Wie man
sieht, ist mit einem solchen unausgerichteten Zugriff fuer den Prozessor sehr
viel Arbeit verbunden.

Natuerlich hat man in hoehern Programmiersprachen schnell einen Datentyp, der
eigentlich keine Zweierpotenz ist, somit immer einen unausgerichteten Zugriff
erfordern wuerde:

```C++
struct Foo
{
	int x;   // 4 Bytes
	char c;  // 1 Byte
};
```

In einem solchen Fall *padded* der Compiler den Datentyp auf eine Zweierpotenz
(je nach Strategie auch manchmal auf eine bestimmte, z.B. $8$). Eine Instanz
`Foo x` wuerde in diesem Fall also zumindest auf $8$ Bytes gepadded werden. Es
laege also mit drei unnuetzen "dummy"-Bytes im Speicher.

Ein Vorteil von unausgerichteten Daten ist offensichtlich, dass sie weniger
Speicherplatz verschwenden, da man z.B. Padding nicht braucht. Aber der
Hardware-Aufwand ist eben groesser.

Die Intel IA-32 Architektur erlaubt den Zugriff auf beliebig ausgerichtete Daten im
Word-, Doubleword- und Quadword-Format. Solch ein Zugriff erfordert aber
zusaetzliche Zyklen, wenn das Datum ein Word- oder Doubleword ist, und eine
4-Byte Grenze ueberschreitet.

### Byte Reihenfolge

Es gibt zwei Moeglichkeiten, Daten im Hauptspeicher zu speichern:

* Little-Endian
* Big-Endian

Der Unterschied ist, wie *Woerter* im Speicher abgelegt werden. Gegeben zum
Beispiel das 32-Bit Wort $0xDEADBEEF$. Wir nehmen an, wir arbeiten auf einem
Rechner mit 32-Bit Wortgroesse, sodass diese Zahl eben genau ein Wort ist. Nun
muessen wir festlegen, wo das *Ende* dieser Zahl ist. Da Zahlen jeglicher Basis
immer nach links groesser werden, ist das Ende also links. Jetzt kommt eben der
Unterschied zwischen Little- und Big-Endian:

* Im Little-Endian Format werden die *kleineren* Ziffern am Ende (also links)
  abgelegt. Das Wort $0xDEADBEEF$ laege also genau verkehrt herum im Speicher,
  mit kleineren Werten an kleineren Adressen (2 Hex Ziffern sind ein Byte, also werden die Ziffern selbst nicht umgedreht, nur die Bytes):

  ```
    0    1    2    3
  [EF] [BE] [AD] [DE]
  ```

* Im Big-Endian Format werden die *groesseren* Ziffern am Ende (also links)
  abgelegt. Die Zahl wuerde also genau so, wie man sie schreibt, im Speicher
  abgelegt werden, mit groessern Werten an kleineren Adressen:

  ```
    0    1    2    3
  [DE] [AD] [BE] [EF]
  ```

Das Little-Endian Format ist typisch fuer Intel, das Big-Endian Format eher fuer
Grossrechner (IBM) oder Motorola. Grundsaetzlich hat keines der Formate einen
wirklich grossen Vorteil oder Nachteil.

Die Eigenschaft der Byte-Reihenfolge nennt man in Englisch *Endianness*.

### Register

Register sind Speicherplaetze innerhalb des Prozessors, auf die er sehr schnell
zugreifen kann. Es gibt meist nur wenige von ihnen (zwischen einem und $\sim
64$). Sie sind in der gesamten Speicherhierarchie die Speicherlemente mit der
schnellsten Zugriffszeit, weil sie eben so nahe am Prozessor sind (2-4ns).

Es gibt hierbei jedoch meist eine Unterscheidung bezueglich dem Zweck der
Register. Es gibt:

* Allzweck Register (General Purpose), fuer ganze Zahlen oder Adressen.
* Gleitkomma Register
* Spezialregister fuer besondere Befehle oder Operationen (z.B. Stack Pointer)
* Systemregister, die Programm- oder Statusinformationen speichern
  (Statusgeister, Instruktionsregister)


### Adressierungsmodi

Die Adressierungmodi einer ISA legen fest, auf welche Weise Operanden fuer
Befehle spezifiziert werden koenen. Operanden fallen dabei grundsaetzlich in
drei Klassen:

* Konstanten (*Immediates*)
* Register, die den Operandenwert enthalten.
* Speicherzugriffe, die eine Adresse fuer einen Operanden im Speicher angeben.

Diese drei Klassen koennen noch auf verschiedene Weisen vereint werden, wie es
in den weiteren Absaetzen beschrieben wird.

#### Unmittelbare Adressierung

Fuer die Methode der unmittelbaren Adressierung wird der Operand direkt mit der
Instruktion mitgegeben. Ist die Konstante zu gross um in die Instruktion zu
passen, wird sie im Hauptspeicher gleich nach der Instruktion abgelegt.

```
[Befehl | Operand]
```

Beispiel: `push 5`

#### Register Adressierung

Bei der Register Adressierung steht der Wert des Operanden in dem angegebenen
Register:

```
[Befehl | Registeradresse]
                 |
             [Operand] <-- Register
```

Beispiel: `inc eax`

#### Speicheradressierung

Bei der Speicheradressierung ist der Operand eine Speicheradresse, an dessen
Stelle im Hauptspeicher der Operand steht. Meist wird die Speicheradresse vorher
noch vom Betriebssystem etwas veraendert, wenn es eine bestimmte
Speicherverwaltung betreibt. Es gibt hier wieder mehrere Arten von
Speicheradressierung, die sehr flexible und dynamische Adressierung erlauben.

##### Direkte Adressierung

Dies ist die einfachste Art von Speicheradressierung, wo die Speicheradresse als
Konstanten im Befehl angegeben wird.

```
[Befehl | Speicheradresse]
                |
	        [Operand] <-- Speicher
 ```

 Beispiel: `inc [0x0]`

##### Registerindirekte Adressierung

Bei der Registerindirekten Adressierung wird im Befehl eine Registeradresse
angegeben, die eine Speicheradresse beinhaltet, an dessen Stelle im Speicher der
Operand liegt:

```
[Befehl | Registeradresse]
                |
	    [Speicheradresse] <-- Register
		        |
			[Operand] <-- Speicher
```

Beispiel: `inc [eax]`

###### Registerindirekte Adressierung mit Praedekrement oder Postdekrement

Hier wird vor der Registerindirekten Adressierung die Adresse im Register noch
inkrementiert oder dekrementiert. Das ist z.B. fuer Stacks oder zum Durchlaufen
von Feldern in Schleifen nuetzlich. Der Wert wird hierbei immer um die Groesse
des Datentyps in-/dekrementiert, also z.B. um vier Bytes bei einem 32-Bit
Registerwert.

```
[Befehl | Registeradresse]
                |
	    [Speicheradresse] <-- Register
				|
			  ++/--
				|
			[Operand] <-- Speicher
```

Kein Beispiel in x86 Assembly.

##### Registerindirekte Adressierung mit Displacement

Hierbei wird dem Registerwert noch ein konstanter Subtrahend oder Summand
hinzugegeben. Diese Art der Adressierung nennt man auch Basisrelative
Adressierung, oder *Based Index* in Englisch.

```
[Befehl | Registeradresse | Konstante]
                \             /
			     \_____+_____/
					   |
			   [Speicheradresse] <-- Register
					   |
				   [Operand] <-- Speicher
```

Beispiel: `inc [eax+5]`

##### Indizierte Adressierung mit Skalierungsfaktor

Bei diesem Speicheradressierungsmodus wird der Wert eines Index-Registers zum
Wert eines Basis-Registers addiert, wobei der Index noch einen konstanten
Skalierungsfaktor erhaelt. Dieser Skalierungsfaktor ist meist die Groesse des
Datums der Datenstruktur, waehrend die Basis beispielsweise die Startadresse des
Arrays ist.

```
[Befehl | Basisregister | Indexregister | Skalierungsfaktor]
               \                \                 /
		        \                \_______*_______/
				 \                       |
				  \__________+___________/
				             |
					 [Speicheradresse] <-- Register
							 |
						 [Operand] <-- Speicher
```

In x86 Assembly muss der Skalierungsfaktor $2, 4$ oder $8$ sein.

Beispiel: `inc [ebx+eax*4]`

##### Indizierte Adressierung mit Skalierungsfaktor und Displacement

Wie oben, aber mit einem zusaetzlichen konstanten Summanden.

Beispiel: `inc [ebx+eax*4+3]`

## Operandenangabe

Es gilt nun noch zu klaeren, wieviele Operanden man fuer einen Befehl angeben
muss. Das wird auch von der ISA festgelegt. Es gibt hier mehrere Moeglichkeiten,
Operanden allgemein anzugeben:

* Explizit: Hier muss der/die Operand(en) im Befehl mitgetragen werden. Das
  erfordert natuerlich zusaetzlichle Dekodierung.
* Implizit: Der Operand ist vom Befehl bzw. der ISA schon festgelegt, z.B. immer
  als das Akkumulator Register.
* Ueberdeckt: Das Ziel ist gleich der Quelle. Beispiel: `inc eax`

Aber wie viele Operanden soll ein Befehl haben?

### Nulladressform

Bei der Nulladressform werden -- wie der Name schon sagt -- gar keine Operanden
angegeben. Sie sind naemlich implizit die __obersten Elemente des Stacks__. Sind
beispielsweise zwei Werte $x, y$ auf den obersten zwei Adressen des Stacks,
wuerde der Nulladressform-Befehl `ADD` diese beiden addieren, und wieder auf den
Stack legen.

Diese Variante der Operanspezifizierung ist beispielsweise bei der JVM ueblich.

### Einadressform

Bei der Einadressform ist ein Operand immer ein spezielles Register, meistens
der Akkumulator. Das war beispielsweise im Von-Neumann Architekturkonzept so
geplant. Das Ergebnis wird hierbei wieder im selben speziellen Register
abgelegt.

Der Nachteil davon ist, dass diese Adressform sehr unflexibel ist und oftmaliges
Laden und Speichern des Akkumulators erfordert, um den gewuenschten Wert, der
moeglicherweise zuvor wo anders gespeichert ist, mit dem Akkumulator zu tauschen
oder ihn zu ueberschreiben.

In x86 Assembly ist beispielsweise `MUL` so ein Befehl.

### Zweiadressform

Die Zweiadressform ist wohl die gaengiste Variante, zumindest bei x86. Hierbei
ist der erste Operand sowohl Operand der Operation als auch Ziel fuer das
Ergebnis. Der zweite Operand ist ein anderes (oder dasselbe) Quellregister.

Meistens kann nur einer der beiden Operanden ein Speicherzugriff sein. Das nennt
man dann das *Register-Speicher-Modell*. Die Intel IA-32 ISA ist ein Beispiel
dafuer.


### Dreiadressform

In dieser flexibelsten aber auch Speicheraufwendigsten Adressform werden sowohl
die beiden Operanden der Operation sowie auch das Ziel separat angegeben. Diese
Adressform gibt es meist in zwei verschiedenen Modellen:

* Speicher-Speicher-Modell. Hierbei duerfen beide Operanden im Speicher
  stehen. Das ist heute nicht mehr gaengig, weil Befehle sehr viel Platz fuer
  die Speicheradressen brauchen und auch sehr lange dauern. Man findet dieses
  Modell eher bei alten CISC Prozessoren.

* Register-Register-Modell. In diesem Modell muessen beide Operanden in
  Registern stehen. Es gibt dann noch zusaetzlich spezielle Befehle, die
  Speicherzugriffe ermoeglichen (`LDR`, `STR`). Fast alle RISC Prozessoren
  nutzen dieses Modell.

## Befehlssatz

Weiters beschreibt eine ISA auch den Befehlssatz der Architektur. Hierbei gibt
es drei Hauptklassen von Befehlen:

1. Datentransferbefehle fuer den Hauptspeicher und Register.
   + `LOAD`
   + `STORE`
   + `MOVE`
2. Arithmetische und logische Operationen.
   + `ADD`
   + `SUB`
   + `CMP`
   + `INC`
   + `IMUL`
   + `XOR`
   + `AND`
3. Steuerung des Programmablaufs (z.B. Spruenge).
   + `JMP` (Unbedingt)
   + `JNE` (Bedingt)
   + `CALL` (Unterprogrammaufruf)

Oft noch zusaetzlich:

* Systembefehle fuer Verwaltungsaufgaben. Diese werden vom Betriebssystem
  genutzt.

* Befehle fuer das Ein-/Ausgabewerk (`OUT` bei 8085).

* Fliesskommabefehle:
  + `FADD`
  + `FSUB`
  + `FNEG`
  + `FMUL`
  + `FSQRT`!
  + Teilweise trigonometrische Funktionen ($\sin, \cos$)

### Befehlsformat

Diese Befehle werden immer mit einem *OpCode* kodiert. Das Befehlsformat kann
dann entweder einheitlich mit einer festen Groesse sein (z.B. immer 32-Bit),
oder heterogen mit variabler Laenge fuer verschiedene Befehlsklassen. Die erste
(fixe) Variante ist ueblich fuer RISC, die variable Variante ueblich fuer CISC
Architekturen. Einige Vor- und Nachteile sind hier gelistet:

* Einheitliches Befehlsformat
  + Einfach und Schnell zu kodieren.
  - Unflexibel fuer nicht-Load/Store-Architekturen.
  - Kein Platz fuer lange Konstanten (z.B. Adressen) im Befehl.

* Variables Befehlsformat
  + Extrem flexibel
  + Erlaubt Orthogonalitaet: Alle Adressierungsarten fuer alle moeglichen
    Operanden erlaubt (z.B. Unmittelbare Adressierung)
  - Dekodierung der Befehle sehr aufwendig
  - Probleme mit Speicherverwaltung

### Betriebssystem-Unterstuetzung

Rechner sind heutzutage meist Mehrbenutzersysteme. Also sollten
Benutzerprogramme nicht direkt auf gemeinsame Hardwareressourcen und
Sytemkonfigurationen zugreifen duerfen. Das erfordert wiederum Koordination
durch eine externe Macht: das Betriebssystem. Die technische Basis fuer eine
Betriebssystem sind die *Ausfuehrungsmodi* einer ISA.

Es gibt grundsaetzlich zwei Ausfuehrungsmodi:

* Den *Systemmodus*, der vollen Zugriff auf alle Rechnerkomponenten hat. Dieser
  Modus ist fuer das Betriebssystem gedacht.

* Den *Benutzermodus*, welcher nur eingeschraenkten Zugriff auf das System
  hat. Speicherzugriffe werden vorher durch das Betriebssystem abgebildet. Es
  hat auch keine priviligierten Befehle um Zugriffe auf die Ein-/Ausgabe zu
  machen oder auf Konfigurationsregister.

Die Ausfuehrung einer unzulaessigen Operation fuehrt zu einer *Ausnahme*
(*Exception*). Eine unzulaessige Operation kann sein:

* Der Versuch, einen priviligierten Befehl im Benutzermodus auszufuehren.
* Ein arithmetischer Fehler (z.B. Division durch Null)
* Fehler beim Speicherzugriff (Hardwarefehler, ungueltige Adresse, *fehlende
  Zugriffsberechtigung*)

### Ausnahmebehandlung

Passiert eine Ausnahme, wird der gerade ausgefuehrte Befehl unterbrochen. Dann
wird der Prozessorzustand, vor Allem der Befehlszaehler, gesichert. Die
Programmausfuehrung wird dann an einer *vordefinierten* Adresse im *Systemmodus*
fortgesetzt, wo die Ausnahme behandelt wird. Die Behandlung kann z.B. sein, den
Prozess, indem es die Ausnahme gab, abzubrechen. Wenn das Betriebssystem die
Ausnahme behandeln konnte, z.B. wenn es bei einem Seitenfehler die passende
Seite aus dem Hintergrundspeicher fetched, wird das Programm normal
fortgefahren.

Die *Exceptionvectortabelle* speichert dabei die Adressen, zu welchen das
Programm fuer bestimmte Ausnahmen springen soll.

### Interrupts

Interrupts (Unterbrechungen) erlauben es dem Prozessor auf __externe,
asynchrone__ Ereignisse zu reagieren. Die externe Quelle (z.B. E/A-Geraet)
sendet dabei ein spezielles Signal, eine sogenannte *Unterbrechungsanforderung*
(interrupt request), an den Prozessor. Dieses Signal wird *am Ende eines
Befehlszyklus* vom Prozessor abgefragt und wenn es ein solches Signal gibt, muss
der Prozessor die Unterbrechung (bald) bearbeiten.

Um das zu tun, sichert der Prozessor zuerst den Programmstatus und schaltet dann
in den Systemmodus. Dort springt er dann an eine vordefinierte Adresse im
Betriebssystem, wo die Unterbrechungsbehandlungsroutine (interrupt handler)
liegt.

Interrupts sind vor Allem fuer Ein- und Ausgabegerate wie Tastaturen, Maeuse
oder bestimmte Sensoren notwendig.

### Moduswechsel

Natuerlich muss es auch moeglich sein, zwischen dem priviligierten Systemmodus
und dem Benutzermodus zu wechseln:

* Vom System- in den Benutzermodus. Dies wird mit einem priviligierten Befehl
  realisiert.

* Wechsel vom Benutzer- in den Systemmodus. Dieser Wechsel geht nur fuer einen
  Befehl und wird meist *System Call* (`syscall`) oder *Trap* genannt. Dabei
  wird die Ruecksprungadresse gesichert und das System schaltet in den
  priviligierten Modus. Dort wird der Befehl dann ausgefuehrt und mit einem
  Rueckkehrbefehl wird wieder in den Benutzermodus zurueckgeschalten. Dieser
  System-Call-Befehl loest also einfach eine spezielle Ausnahme aus.
