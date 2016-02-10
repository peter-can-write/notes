# ARM

ARM ist eine englische Technologie-Firma, die RISC Prozessoren fuer eingebettete
Systeme entwickelt. Das besondere an ARM ist, dass es keine eigenen Produkte
(Prozessoren) *herstellt* bzw. produziert, sondern nur *designed*. Es ist also
ein Design-Haus.

ARM konzipiert Prozessoren und deren Befehlssaetze sowie Kerne, und
verkauft diese Plaene via mehreren Lizenmodellen an Kunden, die diese dann in
Fabriken herstellen koennen.

### Lizenzmodelle

ARM hat fuer seine Designs zwei Lizenzmodelle:

1. Fertige Manufakturplaene fuer die Bausteine. Der Kunde muss die Plaene also
   nur mehr zu einer Fabrik bringen.

2. Kerndesigns in VHDL/Verilog. Diese Variante ermoeglicht Firmen wie Infineon,
   Texas Instruments oder Samsung die Plaene noch zu modifizieren, bevor sie sie
   herstellen. So koenen Designs z.B. noch erweitert werden.

ARM erhaelt dann eine Gebuehr pro gebautem Baustein.

### Statistiken

Einige Statistiken zu ARM:

* 1.3 Millarden (Billion) Kerne (Cores) im vierten Quartal (Q4) des Jahres 2009
  verkauft.

* 62% Prozent davon waren fuer Mobiltelefone.
  + Von diesen waren 7% fuer Anwendungsprozessoren (ARM A11, Cortex A8) fuer
    Betriebssysteme (Linux, Android, Symbian, iOS).
  + Darueber hinaus einfache Kerne fuer andere Funktionen wie GSM, WiFi, GPS,
    Bluetooth.

## Instruction Set Architecture (ISA)

Die Instruction Set Architecture (ISA) von ARM ist eine der RISC Variante. RISC
steht hierbei fuer Reduced Instruction Set Computing und steht im Gegensatz zu
CISC (Complex Instruction Set Computing), die von Intel fuer Benutzerrechner verwendet werden.

Der Unterschied zwischen RISC und CISC ist, dass es bei CISC sehr viele, oftmals
unnoetige Befehle gibt. Bei RISC gibt es hingegen nur wenige ($\sim 32$),
einfache Befehle. Der Maschinenprogrammierer muss somit also mehr Arbeit
machen. Der Vorteil von RISC ist dabei die einfache Realisierung mit wenigen
Transistoren (stromsparend) und schneller Verarbeitung.

Ein Beispiel des RISC Konzept ist, dass es keinen expliziten `INC` Befehl gibt, weil dieser Befehl ja schon mit `ADD` vom Maschinenprogrammierer realisiert werden kann.

### Befehlsformat

Die ARMv7 Architektur hat __32-Bit__ Wortgroesse und *normalerweise* ein *festes
Befehlsformat* von vier Byte. Das bedeutet, dass alle Befehle konstant mit
32-Bit im Speicher abgelegt sind. Die Intel x86 Architektur hat beispielsweise
ein variables Befehlsformat von 1 und mehr als 6 Byte.

Eine Besonderheit bei ARMv7 ist aber, dass der Prozessor in der Lage ist,
mit mehreren Befehlsformaten zu arbeiten:

* Das oben erwaehnte Standard *ARM 32-Bit* Befehlsformat.
* Einen reduzierten *Thumb 16-Bit* Befehlssatz mit 2-Byte Befehlsformat und mit
  weniger Befehlen zur Reduzierung der Code-Groesse. Besonders fuer kleine
  eingebettete Systeme wichtig.
* Jazelle: Kann Java ByteCode natively ausfuehren (in einer VM).

#### Thumb

"The Thumb instruction set is a subset of the most commonly used 32-bit ARM
instructions. Thumb instructions are each 16 bits long, and have a corresponding
32-bit ARM instruction that has the same effect on the processor model. On
execution, 16-bit Thumb instructions are transparently decompressed to full
32-bit ARM instructions in real time, without performance loss.

Thumb has all the advantages of a 32-bit core:

32-bit address space
32-bit registers
32-bit shifter, and Arithmetic Logic Unit (ALU)
32-bit memory transfer.

Thumb code is typically 65% of the size of ARM code, and provides 160% of the
performance of ARM code when running from a 16-bit memory system. Thumb,
therefore, is ideally suited to embedded applications with restricted memory
bandwidth, where code density and footprint is important."

http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.ddi0210c/CACBCAAE.html

#### Dynamisches Wechseln des Befehlformats

Da ARMv7 32-Bit Wortgroesse hat und das Standard Befehlsformat 32-Bit ist, daher
also 4-Byte aligned im Speicher liegt, sind jegliche Zugriffe auf eine
Instruktion 4-Byte aligned. Das bedeutet wiederum, dass die unteren 5 Bit immer
null sind (weil die Startadresse immer in Restklasse 0 ist). Von diesen Bits
werden zwei gewaehlt, um das Befehlsformat des Prozessors festzulegen. Man kann
also das Befehlsformat dynamisch fuer jede Instruktion veraendern, indem man die
ersten Bits entsprechend setzt. So kann man beispielsweise zeitkritischen Code
nur mit Thumb (16-Bit) ausfuehren, und den Rest mit dem ARM Befehlsformat
(32-Bit).

### Load/Store Architektur

ARM hat eine sogenannte Load/Store Architektur. Das bedeutet, dass es explizite
Instruktionen fuer Speicherzugriffe gibt. Diese sind `LDR` (load register) und
`STR` (store register). Ansonsten muessen alle anderen Befehle auf Registern
arbeiten. Das hat wiederum eine reduzierte Anzahl sowie reduzierte
Komplexizitaet von Instruktionen zur Folge.

Adressierungsmodi sind hierbei auch nur sehr einfach, weil das
Instruktionsformat auch mit 32-Bit fest ist und somit keine 32-Bit
Speicheradressen beinhalten kann. Speicheradressen koennen via Registern
(register-indirekt) oder direkten Operanden relativ zum momentanen
Befehlszaehler angegeben werden.

### Bedingte Ausfuehrung

Spruenge um eine Instruktion zu ueberspringen sind sehr ineffizient, da sie
meist einen Speicherzugriff erfordern. Es waere doch viel effizienter, einen
Befehl einfach nur bei einer bestimmten Bedingung auszufuehren. Genau das ist
die Idee von *bedingter Ausfuehrung* bzw. *conditional execution*.

Anstelle von ineffizienten Spruengen um eine Instruktion zu ueberspringen, wird
ein Befehl einfach mit einer Bedingung verbinden. Der Befehl wird nur dann
ausgefuehrt, wenn die Bedingung gilt. Die oberen vier Bit einer jeden 32-Bit ARM
Instruktion sind daher fuer die Bedingung reserviert.

Moechte man z.B. eine Zahl nur inkrementieren, wenn sie gleich null ist, saehe
das in ARM so aus:

```asm
cmp r0, #0        ; compare and set flags
addeq r0, r0, #1   ; increment only if zero flag set
```

Note: es gibt kein `inc` in der ARM ISA (__RISC__)

Weiteres Beispiel, zuerst ein Stueck C Code:

```C
if (r0 == 0) ++r1;

else ++r2;
```

Nun in x86 Assembly:

```asm
cmp r0, 0
jne else
inc r1
jmp end
else:
inc r2
end:
```

Und so mit bedingter Ausfuehrung in ARM Assembly:

```asm
cmp r0, 0
addeq  r1, r1, #1
addneq r2, r2, #1
```

#### Visualisierung

Man kann Sprung-basierte Ausfuehrung mit bedingter Ausfuehrung durch folgende
Diagramme in Kontrast stellen:

Mit Spruengen:

```
o
|
o
|\ jump
o o
|/
o
```

Mit Bedingung:

```
o
|
o Bedingung? Eventuell einfach nicht ausfuehren.
|
o
```

## Register

Die ARM Architektur beinhaltet 37 Register, von welchen die $16$ Register $R0$
bis $R15$ dem Maschinenprogrammierer zur Verfuegung stehen. Von diesen $16$ sind
nun die $13$ Register $R0$ bis $R12$ Allzweckregister, und die anderen
spezielle:

* $R13$ ist der Stackpointer (ESP).
* $R14/LR$ ist das sogenannte "Link Register". Es speichert die
  Ruecksprungadresse bei Unterprogrammaufrufen (nicht auf dem Stack wie bei
  x86).
* $R15$ ist der Befehlszaehler ("program counter", PC)
* $CPSR$: Current Program Status Register (Statusregister)
* $SPSR$:   Saved Program Status Register (temporaeres Statusregister)

Bei ARM gibt es kein `CALL`/`RET`, also muss man den Befehlszahler (IP) selbst
modifizieren. Dafuer gibt es das spezielles Link-Register (LR).

## Befehlstypen

Das Befehlsformat erlaubt folgende Adressierungsmodi fuer Befehle:

* Zwei Operanden (Ziel, Quelle)
* Drei Operanden (Ziel, erste Quelle, zweite Quelle)

Hierbei kann jedem Befehl bzw. OpCode noch eine Bedingung nachgestellt
werden. Weiters kann jedem Befehl auch ein Shifter-Operand nachgestellt
werden. Dieser erlaubt es, das Ergebnis der Operation noch in der selben
Instruktion um den gegeben Wert (den Shifter-Operanden) zu logisch zu schieben.

Ein Befehl hat also folgendes Schema:

`<OpCode>[condition] Ziel [, Quelle 1 [, Quelle 2]] [Shifter-Operand]`

Zusaetzlich gibt es noch die beiden Load und Store Befehle:

* `LDR`: Load Register.
* `STR`: Store Register.

### Branch-Instruktionen

Da ARM-Instruktionen nur 32-Bit gross sind, und Speicherzugriffe nur bei Load
und Store Instruktionenen erlaubt sind, muss es anderen Moeglichkeiten geben,
ein Branch Adresse anzugeben. Diese sind:

1. Indirekte Addressierung ueber Register
2. Adresierung relativ zum Befehlszaehler ("springe zwei Adressen weiter")

Der Sprungbefehl heisst bei ARM $B[condition]$. Es gibt aber noch einen weiteren
Befehl: `BL`. So wie `CALL` wird dieser Befehl fuer Unterprogrammaufrufe
verwendet. Anders als bei `CALL` wird die Ruecksprungadresse aber nicht auf den
Stack geleget, sondern in das *Link Register* ($R14$).

Der Ruecksprungbefehl (von der Funktion her wie `RET`) ist bei ARM einfach
`MOV`. Moechte man zur Ruecksprungadresse zurueckspringen, schreibt man
`MOV PC, LR`, also "move link register into program counter". Im Link Register
war die Ruecksprungadresse gespeichert, also wird der Befehlszaehler (PC) auf
diese Adresse gesetzt.

### Datenverarbeitende Instruktionen (Add/Sub)

Der ARM Befehlssatz beinhaltet alle bekannten arithmetischen, logischen und
Vergleichsoperationen mit Ausnahme von Multiplikation und Division.

Weil es in der ARM ISA kein `NOP` (no operation) gibt, muss man den Befehl `MOV
R0, R0` benutzen.

Diese Befehle haben das folgende Schema:
`<OpCode>[condition] Ziel, Quelle [, zweite Quelle]`

Beispiele:
* `add r1, r2, r3 ; r1 = r2 + r3`
* `addeq r2, r4, r5 ; when zero flag set, r2 = r4 + r5`
* `mov pc, r14 ; ruecksprung, lr would also work instead of r14`
* `mov r0, r0 ; nop`
* `tst r0, #5 ; test r0, 5`
* `teq r0, #5 ; equality test without modifying c or v flags`
* `cmp r0, #5 ; checks equality and modifies all flags (like cmp in x86)`

### Load/Store-Instruktionen

Da ARM eine Load/Store Architektur hat, duerfen alle Operationen (e.g. ADD/MOV)
ausser zwei nur Register oder Konstanten als Operanden haben, nicht aber
Speicherzugriffe. Fuer die Speicherzugriffe gibt es extra Befehle um Daten aus
dem Speicher in Register zu laden (`LDR`) oder Register in den Speicher zu
schreiben (`STR`).

Diese Befehle haben das folgende Schema:
`<LDR|STR>[B][condition] Register, Address`

Die Adresse muss wie bei Branch-Adressen relativ oder ueber Register uebergeben
werden, da die Instruktion nur 32-Bit lang ist, ebenso wie die Wortgroesse bei
x86.

Normalerweise fetched `LDR/STR` immer wortweise (4 Byte), aber mit dem `B`
Zusatz zum OpCode (`LDRB/STRB`) kann man einzelne Bytes fetchen.

Fuer diese Befehle gibt es nun viele Adressierungsmoeglichkeiten, z.B. `LDR R6,
[R1, 4]` = `R6 = *(R1 + 4)`

### Koprozessor-Instruktionen

Der Kauefer von ARM-Prozessoren kann sich beim Entwurf noch Zusatzbausteine
entwickeln lassen. Daher gibt es Koprozessor-Instruktionen, welche Interaktionen
mit anderen Prozessoren ermoeglichen.

## Stromverbrauch

Ein grosser Pluspunkt von ARM-Prozessoren ist, dass sie enorm stromsparend
sind. Der ARM Cortex-A9 verbraucht beispielsweise 250mW = 1,6 Watt bei 1Ghz. Das
ist ein Grund, wieso sie so beliebt fuer eingebettete Systeme sind.

### Stromspartechniken

* Verringerung unnoetiger Befehlsladeoperationen durch __Sprungvorhersage__.
* Verringerung der Cache Flushes (Weiterleitung an naechstes Level in der
  Speicherhierarchie) durch physische Adressen.
* __Reduktion der Speicherzugriffe durch Caches__.
* Kurze Schleifen mit weniger als 64-Byte ohne Zugriff auf Cache.
* __Verschiedene Spannungsbereiche__ (RAM, Prozessor Logik, FPU etc.)
* __Verschiedene Power Modes__ (Full Run, Standby, Sthutdown) in denen die
  Stromversorgung oder das Taktsignal fuer bestimmte Bereiche abgeschaltet
  werden koennen.

## Synchronisation

Synchronisation von Multikernen wird bei Intel x86 durch bestimmte *atomic*
instructions wie `cmpxchg` erreicht. Dieser Befehl schreibt einen Wert (zweiter
Operand) an eine Speicherstelle (erster Operand), wenn er noch der
spezifizierte ist (AL/AX/EAX je nach Operandenlaenge).

https://courses.engr.illinois.edu/ece390/archive/spr2002/books/labmanual/inst-ref-cmpxchg.html

ARM hat eine etwas andere Synchronisationsstrategie. Es gibt dafuer zwei
besondere Befehle:

* `LL` (load-linked): Laedt die Speicherstelle und beobachtet sie.
* `SR` (store): Speichert einen Wert nur an die Speicherstelle, wenn sie sich
  nicht veraendert hat (wenn sie vorher mit `LL` unter Beobachtung gestellt
  wurde).

Somit kann Atomicity *emuliert* werden.
