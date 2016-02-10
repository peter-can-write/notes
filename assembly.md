# Assembler

Assembler ist eine *Programmiersprache*, in der jede Anweisung genau einen
Maschinenbefehl erzeugt. Ein Assemblerprogramm wird durch einen *Assemblierer*
in ein *Maschinenprogramm* uebersetzt (kompiliert). Das Maschinenprogramm ist
dann eine Sammlung von Hexadezimal Codes. Im Englischen verwendet man den
Begriff *Assembly* fuer die Sprache und den Compiler.

Der Assembler selbst kann dabei eine eigene Syntax haben (NASM vs GAS), jedoch
hat ein Prozessor nur eine einzige Maschinensprache. Also muessen alle
Assemblierer letztendlich dasselbe Maschinenprogramm erzeugen (auf dem selben Rechner).

Die Programmierung in Assembler ist wegen der niedrigen Abstraktion sehr
aufwaendig, aber es gibt dennoch gute Gruende, darin zu programmieren:

1. Leistung. Assembler ist extrem hardware-getuned und somit kann man jeden
   letzten Tropfen an Rechengeschwindigkeit und Speicherbedarf rausholen. Jedoch
   muss man auch wissen was man tut. Moderne Compiler und Optimizer erzeugen
   schon extrem guten und angepassten Maschinencode, der von Menschen oftmals
   nicht erzeugt werden koennte. Oftmals schreibt man den Grossteil seiner
   Code-Base in einer schoenen Hochsprache wie C++ und geht dann nur fuer die
   Performance-kritischen Gebiete wirklich deep down low.

2. Zugriff auf Hardware. Assembler erlaubt direkten Zugriff auf die Hardware
   eines Systems und ist daher sehr gut zum Programmieren von eingebetteten
   Systemen geeignet. Auch die Programmierung von Behandlungsroutinen fuer
   Interrupts und Traps im Betriebssystem ist mit Asssembler leicht(er).

3. Geringe Code Groesse. Ein Assembler Programm hat oftmals einen viel kleineren
   Code *Footprint* als ein aequivalentes uebersetztes Programm in einer
   Hochsprache. Das ist wiederum fuer eingebettete Systeme sehr gut und
   wichtig.


Fuer x86 Assembler in ERA wird angenommen, dass die Wortgroesse 32-Bit ist und
Daten im Little-Endian Format gespeichert werden (wortweise LSB links bzw. an
niedrigster Speicheradresse). Konstanten (*Immediates*) sind daher auch 32-Bit.

## Register

Es gibt in modernen x86 Prozessoren acht 32-Bit "Allzweck" Register, von denen zwei
aber fuer Stack-Operationen reserviert sind:

'E' steht hier immer fuer "Extended".

http://www.cs.virginia.edu/~evans/cs216/guides/x86.html#register

### EAX

Historisch das "Accumulator" Register.

### EBX

Historisch das "Base Address" Register, auf 16-Bit Maschinen gedacht als
Base-Pointer fuer Table-Lookup

### ECX

Historisch das "Counter" Register, also als Loop Index gedacht.

### EDX

Historisch das "Data" Register, gedacht als 64-Bit Erweiterung von `EAX` fuer
bestimmte Operationen wie Multiplikation oder Division, bzw. zum Speichern von
Daten des Akkumulators.

### EDI

Historisch das "Destination Index" Register, gedacht als "moving-pointer" fuer
Daten aus Schleifen. Es gibt bestimmte Befehle wie `STOS` die `EDI` ideal fuer
solche Operationen machen. `STOS` schreibt beispielsweise den Wert aus dem
Akkumulator in die Adresse am Desination Index, und inkrementiert diesen
dann. Somit kann man eine solche Operation also mit nur einem Command
durchfuehren.

### ESI

Historisch das "Source Index" Register, so wie `EDI`, nur zum lesen von Werten
in Schleifen gedacht. Hat also aehnliche 2-in-1 Operationen wie STOS, nur eben
zum lesen.

### ESP

Der *reservierte* Stack-Pointer, auch heute noch genutzt fuer
Operationen im Zusammenhang mit dem Stack, wie `PUSH`, `POP`, `CALL`, `RET`.

### EBP

Der Base-Pointer wird bei Funktionsaufrufen dafuer genutzt, die Startadresse des
letzten *Stack-Frames* zu speichern (siehe Calling Conventions).

http://www.swansontec.com/sregisters.html

### Weitere Informationen

E[ABCD]X sind zwar selbst 32-Bit/4-Byte, man kann aber die unteren
(least-significant) 2 Bytes mit [ABCD]X adressieren, und von diesen den oberen
Byte mit [ABCD]H ("High") und [ABCD]L ("Low") adressieren.

```
[         | AH | AL ] <-- EAX
               ^
		       AX
```

ESI, EDI, ESP und EBP haben jeweils nur einen 16-Bit Anteil: SI, DI, SP,
BP. Aber keine (adressierbaren) 8-Bit Anteil.

Namen von Registern sind nie case-sensitive, also ist in assembly `eax` das
selbe wie `EAX`.

Gesamtblockbild:

```       < 16 Bit >

[         | AH | AL ] <-- EAX
[         | BH | BL ] <-- EBX
[         | CH | CL ] <-- ECX
[         | DH | DL ] <-- EDX
[         |   SI    ] <-- ESI
[         |   DI    ] <-- EDI
[                   ] <-- ESP (stack pointer)
[         |   BP    ] <-- EBP (base pointer)
<----- 32 Bit ------>
```

### Status Register

Jeder x86 Rechner enthaelt ein sogenanntes *Status Register*, das bestimmte Bits
mit bestimmer Bedeutung enthaelt. Diese Bits werden in Abhaengigkeit des
Ergebnisses einer arithmetischen oder logischen Operation gesetzt, und koennen
z.B. zum Ueberprufen von Bedingungen genutzt werden. Die Flags sind:

1. `CF`: Carry. Gesetzt, wenn bei einer *unsigned* Addition/Subtraktion ein Uebertrag vom
   MSB (ins nichts) passiert ist.
2. `OF`: Overflow. Gesetzt wenn man bei einer *signed* Addition zwei positive
   Zahlen addiert un das Resultat negativ ist, oder zwei negative Zahlen addiert
   und das Resultat positiv ist.
3. `ZF`: Zero. Gesetzt, wenn das Ergebnis null ist.
4. `SF`: Sign. Gesetzt, wenn das (*signed*) Ergebnis negativ ist.
5. `PF`: Parity. Gesetzt, wenn die Anzahl an gesetzten Bits im LSB (byte) gerade ist.

https://en.wikipedia.org/wiki/Parity_flagB

Man kann die Status Register nicht explizit ansprechen, sichern, lesen oder
schreiben. Ausser dem Carry Flag, mit `STC` (set carry) und `CLC` (clear carry).

#### Carry vs Overflow:

http://stackoverflow.com/questions/8496185/assembly-carry-flag-vs-overflow-flag
http://stackoverflow.com/questions/791991/about-assembly-cfcarry-and-ofoverflow-flag?rq=1
http://stackoverflow.com/questions/19301498/carry-flag-auxiliary-flag-and-overflow-flag-in-assembly

##### Carry

Das Carry bit wird gesetzt, wenn bei einer *unsigned* Addition der MSB
uebertragen wird, also wenn der MSB gaenzlich aus dem Datentyp bzw. dem
vorgesehenen Speicherraum (je nach Registerbreite) "ins nichts" uebertragen
wird. Addiert man z.B. im 8-Bit Register `AL` den Wert `0b00000001` auf
`0b11111111`, so passt der uebertragene MSB nicht mehr in den vorgesehenen
Speicherraum von 8-Bit. Also wird das Carry Flag gesetzt.

Auch wird das Carry gesetzt, wenn bei einer Subtraktion ein Borrow "aus dem
nichts" passieren muss, also ausserhalb der Breite des Datentyps bzw. wenn der
MSB also auch $0$ ist. Dies passiert immer, wenn man eine groessere Zahl von
einer kleineren subtrahiert! Z.B. bei `0000 - 00001 = 1111` wird `CF = 1` weil
man aus "dem nichts" borrow-en musste. Das Sign Bit muss dann aber nicht immer
eins sein, weil hoehere Bits den geborrowed Bit wieder loeschen koennen.

Fuer *signed* Werte hat das Carry Flag ueberhaupt keine Bedeutung.

Fuer *unsigned* Werte bedeutet ein Setzen des Carry Flags, dass bei der letzten
arithmetischen Operation ein Fehler passiert ist.

Das Carry Bit wird auch bei einem Uebertrag bei der Multiplikation gesetzt, sowie wenn der MSB bei einem logischem Schiebebefehl nach draussen geschoben wird.

##### Overflow

Das Overflow Flag wird gesetzt, wenn bei einer *signed* Addition von zwei
positiven Zahlen das Ergebnis negativ ist (wenn also der Carry Bit ins Sign Bit
uebertragen wird) *oder* wenn bei der *signed* Addition von zwei negativen
Zahlen das Ergebnis posiitv ist (wenn beim Uebertrag der Sign Bit geloescht
wird). Also allgemein, wenn sich bei einer *signed* Addition von zwei Werten mit
selben Vorzweichen das Sign Bit aendert.

Fuer *unsigned* Werte hat das Overflow Flag ueberhaupt keine Bedeutung.

Fuer *signed* Werte bedeutet ein Setzten des Overflow Flags, dass bei der
letzten Operation ein Fehler passiert ist.

##### Konklusion

Das wichtige ist, dass das Overflow Flag unabhaengig vom Carry Flag gesetzt
wird, und unabhaengig davon, ob der Wert wirklich (fuer den Programmierer)
gerade gar nicht signed ist. Ebenso wird der Carry Flag auch gesetzt, wenn bei
einer (fuer den Programmierer) signed Addition ein Uebertrag des MSB passiert
ist, wenn die Zahlen als unsigned interpretiert wuerden.

Es ist egal, ob die Werte gerade signed oder unsigned sind (fuer den
Programmierer). Wenn man die Zahlen als signed interpretiert, und es passiert
ein Overflow, dann wird das OF Bit gesetzt. Wenn man die selben Zahlen nun als
unsigned interpretiert, und es passiert ein Uebertrag des MSB, dann wird auch
das Carry gesezt. Also voellig unabhaengig von einander, einfach "wenn man es
als signed/unsigned interpretiert".

Gegeben dieser Code:

```
mov al, -5  ; (1)
add al, 132 ; (2)
add al, 1   ; (3)
```

In Zeile (1) wird also der Wert $-5_{10}$ bzw. $11111011_2 = 251_{10}$ im
binaeren Zweierkomplement nach AL geschrieben.

Dann wird $132_{10} = 10000100_2$ auf diesen Wert addiert.

__Interpretiert__ man nun diese Addition als *unsigned*, dann erfolgt bei der
Addition $132_{10} + 251_{10} = 383_{10}$ ein Uebertrag des MSB ins nichts
(passt nicht mehr in 8-Bit), also wird das Carry Flag gesetzt.

__Interpretiert__ man nun diese Addition als *signed*, dann ist $132_{10} =
10000100_2$ gleich $-124$. Bei der signed Addition von $-5$ mit $-124$ kommt
also $-129$ raus. Dieser Wert passt nicht mehr in die Range von 8-Bit signed
werten ($-128$ bis $127$) bzw. der Carry Bit flipped hierbei das Sign bit, und
macht den Wert positiv ($+127$). Da also bei einer *signed* Addition von zwei
negativen Zahlen eine positive Zahl herausgekommen ist, wird das Overflow Flag
gesetzt.

Nun addiert man zu diesem Wert in Zeile (3) also noch den Wert $1$, waehrend der
Wert in `AL` also gerade $127$ ist.

Interpretiert man diese Addition nun als *unsigned*, dann ist $127 + 1 = 128$,
und dieser Wert passt perfekt in einen 8-Bit unsigned Wert. Also das Carry Flag
wird nicht gesetzt.

Interpretiert man diese Addition nun als *signed*, dann ist $127_{10} + 1_{19} =
01111111_2 + 00000001_2 = 10000000_2$. Hierbei hat sich also bei der Addition
von zwei positiven Zahlen das Sign Bit gesetzt (wegen dem Uebertrag) bzw. eine
negative Zahl ergeben, also wird das Overflow Bit gesetzt.

Ein weiteres Beispiel:

```
mov al, 100
sub al, 200
```

Zuerst unsigned: Hier sieht man gleich, dass eine groessere Zahl von einer
kleineren abgezogen wird. Also muss wird ausserhalb des Datentyps geborrowed und
die Carry Flag wird eins.

Der Sign Bit wird natuerlich auch eins, weil $100 - 200$ eine negative Zahl ist.

Dann signed: $200$ passt fuer signed nicht mehr in die positive 8-Bit Range,
also muss man sehen, welche negative Zahl es ist (binaer aufschreiben, 2-er
Komplement bilden, es ist dann -diese Zahl). Hier also $-56$. Nun ist $100 -
(-56) = 100 + 56 = 156$. Hier hat man eine Addition zweier positiver Zahlen, die
zu einer negativen Zahl fuehrt, weil $156$ passt nicht mehr in den positiven
Raum von $1$ bis $127$ fuer 8-Bit. Also wird die Overflow Flag gesetzt.

http://teaching.idallen.com/dat2343/10f/notes/040_overflow.txt

## Konstanten

Konstanten (*Immediates*) koennen mit einem `0x` Praefix *oder* mit einem
`h` Suffix im Hexadezimalsystem spezifiziert werden. Binaere Werte koennen nur
mit einem `b` Suffix markiert werden (__kein__ `0b` Praefix!).

Konstanten sind auf 32-Bit x86 Rechnern immer __32-Bit__ breit.

## Commands

Das Grundformat eines Assembler Befehls ist folgendes:

```asm
[optional]
<mandatory>

[marke:]
<opcode> <operand1>[, <operand2> [, <operand3>]] [;kommentar]
```

__Nur ein Speicherzugriff!!!__

Command Namen wie `MOV` sind sogenannte "Mnemonics" und deren Schreibweise ist
abhaengig vom Assembler. Sie stehen eigentlich nur fuer bestimmte
OpCodes. OpCodes sind Bytefolgen, die vom Prozessor uebersetzt
werden. Z.b. uebersetzt der Assembler `ADD EAX, 0x1000` in `05 00 10 00 00`

#### Typisierung

Im allgemeinen Fall kann die Groesse eines Operanden bzw. eines Datums aus der
Groesse des Registers bzw. der Assembler Instruktion geschlossen werden. Laedt
man beispielsweise ein 32-Bit Register (z.B. `EAX`) aus dem Speicher, so
schliesst der Assembler, dass man einen 4-Byte grossen Addressraum adressieren
moechte. Oder addiert man zwei Extended Register, schliest der Assembler dass es
sich um eine 32-Bit Addition handelt. In manchen Faellen ist die Groesse des
angesprochenen Datums jedoch nicht eindeutig, dann muss man es *typisieren*,
indem man einen Typ aus `[BYTE, WORD, DWORD]` vor die Konstante oder den
Speicherzugriff stellt. Dies ist z.B. immer bei 1-Address Befehlen ohne
expliziter Operandenlange der Fall:

```
NEG [20] ; error: ambiguous
NEG WORD [20]; OK

MOV EAX, [0x0] ; 32-Bit implizit
MOV EAX, DWORD [0x0] ; aber so ist es sicherer!!!!
```

Tipp: IMMER typisieren!!! Dann passieren nie bloede Fehler.

### Symbolische Adressen

Mit Marken (Labels) ist es in Assembler moeglich, Speicheradressen einen
symbolische Namen zu geben, die schliesslich vom Assembler an echte
Speicheradresse gebunden werden und vom Praeprozessor wie Makros aufgeloest
werden. Man kann hierbei an eine symbolische Adresse einen initialisierten Wert
geben (`EQU`), beliebig viele initialisierte Werte (ein Array/einen Table) geben
(`D<X>`) oder beliebig viele uninitialisierte Werte reservieren (`RES<X>`).

Wenn in einer Aufgabe "Symbolische Adresse" steht, ist damit eine Adresse
gemeint und nicht eine Konstante, also `[Wert]` und nicht `Wert` verwenden.

#### `EQU`

Mit `EQU` kann man mit einem Label fuer den Praeprozessor eine Konstante
assoziieren. Der erste Byte der Konstante kann danach im Programm mit dieser
Marke adressiert werden.

```asm
konstante1:
	EQU 123+4

adresse1:
	EQU 0x123

MOV EBX, konstante1
```

#### `D<X>`

Mit der Familie der `D<X>` Befehle kann man mehrere Konstanten in einer Art
Array oder Tabelle sequentiell (contiguously) im Speicher ablegen. `<X>` muss
dabei entweder `B` (`DB`, Byte), `W` (`DW`, Word) oder `D` (`DD`, DoubleWord)
sein. Die Konstanten werden dabei mit Kommata getrennt.

```asm
meine_tabelle:
	DB 1, 2, 3, 4, 5 ; 5 Bytes

meine_tabelle2:
	DW 0xFFF, 0xFFFF, 0xFFFFFFFF ; 3 Woerter = 12 Bytes
```

Da die Werte von `D<X>` sequentiell im Speicher landen, kann man die einzelnen
Werte auch separat deklarieren:

```asm
meine_tabelle:
	DB 1, 2, 3, 4, 5 ; 5 Bytes

; oder

meine_tabelle:
	DB 1
	DB 2
	DB 3
	DB 4
	DB 5
```

#### `RES<X>`

Waehrend `D<X>` einen Speicherbereich reserviert *und initialisiert* hat,
reserviert `RES<X>` nur, laesst den Speicherberreich also uninitialisiert
bzw. *undefiniert* (garbage values). `<X>` sollte wieder `B`yte, `W`ord oder
`D`oubleWord sein. Auch werden hier also die Werte nicht direkt angegeben,
sondern der Operand von `RES<X>` ist die Anzahl an
Bytes/Woertern/DoubleWoertern, die reserviert werden sollen.

```
meine_leere_tabelle:
	RESB 5 ; Reserviere 5 Bytes
```

### Schiebebefehle

Der einfachste Assembler Befehl ist `MOV`. `MOV` __kopiert__ den Wert von einem
Operanden zum anderen. Dabei kann einer der Operanden eine Speicherstelle sein,
aber nur eine. Die Quelle ist rechts, das Ziel ist links. Also `MOV EAX, EBX`
ist aequivalent zu `EAX = EBX` in C. Ebenso wie in C wird `EBX` nicht
verandert, nur `EAX`.

```
MOV EAX, EBX ; EAX = EBX
```

### Speicherzugriffe

Die Wortgroesse fuer ERA Maschinen ist 32-Bit/4-Byte, man kann also insgesamt
bis zu $2^{32} \text{ B} = 4 \text{ GB}$ an Speicher adressieren.

Es kann pro Command immer nur __eine Speichadresse__ angegeben werden, `MOV [1],
[2]` ist also nicht erlaubt, man muesste eine der Adressen zuerst in ein
Register laden.

Speicheradressen werden allgemein in eckigen Klammern `[]` angegeben.

__Note__: Speicherzugriffe sind teuer, also ist es besser, Daten in Registern zu
halten und erst wenn noetig Speicherzugriffe zu machen. Soll man z.B. einen Wert
berechnen und dann an eine Speicherstelle schreiben, ist es essentiell, diesen
Wert in einem Register zu berechnen und erst am Ende den Wert zum Speicher zu
schieben, als in jedem `ADD` auf die Zieladresse zuzugreifen.

Auch: __Nur 32-Bit Werte duerfen fuer Speicherzugriffe verwendet werden__!. Also
nicht: `[DL]`!!!

Es ist klueger, Speicherzugriffe wenn moeglich immer zu typisieren, sonst
vergisst man es manchmal.

#### Direct

Unter direct bzw. direkter Adressierung versteht man das zugreifen auf eine konstatne Speicheradresse. Der Operand ist also eine 32-Bit Immediate, in eckigen Klammern:

```asm
MOV EBX, [0]   ; ptr = 0x0; EBX = *ptr
```

#### Register Indirect

Eine weitere Moeglichkeit eine Speicheradresse anzugeben ist mit einem
*Register Indirect*. Hierbei kann die Adresse durch ein Register angegeben werden. Bei einem Register bestimmt der Wert, der im
Register gespeichert ist, die Adresse. Ein solches Register ist also Quasi ein
Pointer auf eine Speichadresse, und mit eckigen Klammern dereferenziert man
diesen Pointer.

```asm
MOV EBX, [EAX] ; EBX = *EAX
```

#### Based Mode

Beim Based Mode (Registerindirekt mit Displacement) wird die Adresse relativ zu
einem *Base-Pointer* (einem Register) angegeben. Der Offset ist dann eine
*Konstante*. Der Base-Pointer selbst darf keine weitere Konstante sein. Kein
Whitespace zwischen dem `+`.

```asm
MOV EBX, [EAX+4] ; EBX = *(EAX + 4)

MOV EBX, [0 + 1] ; ERROR!
```

Note: ein Label ist auch eine Konstante, also sowas geht nicht: `MOV EBX,
[label+4]`!

#### Based-Indexed Mode

Based-Indexed Mode (Indizierte Adressierung) ist wie Based Mode, aber das
Displacement ist ein weiteres Register.

```asm
MOV EBX, [EAX+EBX]
```

#### Based-Indexed Mode with Displacement

Base-Pointer + Register Offset + Konstante. Das Displacement __muss rechts
sein__.

```asn
MOV EBX, [EAX+ECX+4] ; EBX = *(EAX + ECX + 4)
```

#### Base and Scaled Index with Displacement

Weiters kann einem __Register__ (nicht Konstante!) ein Multiplikator als *Scale*
gegeben werden. Dieser Scale __muss 2, 4 oder 8 sein__! Damit kann man
beispielsweise ein Array eines bestimmten Datentyps $n$ mal ueberspringen.

```
MOV EBX, [EAX*4+array] ; EBX = array[EAX]
```

^ Hierbei ist also `array` die Startadresse eines Arrays von 4-Byte breiten
Werten (`integer`), und EAX ist ein Index. Wenn EAX z.B. gleich 3: `EBX =
array[3]`

__Scale: 2, 4 oder 8!!!!!!!!!!!!!!__

### Arithmetische Operationen

#### ADD

`ADD` Addiert einen Quellwert auf einen Zielwert. Dabei ist der erste Operand
das Ziel und der zweite Operand die Quelle. Ein Operand muss ein Register sein,
der andere eine Konstante (*Immediate*) oder eine Speicheradresse.

__Die Operanden muessen gleich breit sein!!!!!!!!!!!!!!!!!!__

```
ADD EAX, BX  ; Error
ADD EDX, CL  ; Error

ADD EAX, EBX     ; EAX += EBX
ADD EAX, 5       ; EAX += 4
ADD EAX, [0x123] ; ptr = 0x123; EAX += *ptr

ADD AX, BX ; OK
ADD CL, DH ; OK

ADD 5, 10          ; Error!
ADD [0x0], [0x123] ; Error!

```

#### SUB

Selbes Format wie `ADD`, nur subtrahiert die Quelle vom Ziel:

__Die Operanden muessen gleich breit sein!!!!!!!!!!!!!!!!!!__

```
SUB EAX, BX ; Error
SUB EDX, CL ; Error

SUB EAX, EBX ; EAX -= EBX
SUB EAX, 5   ; EAX -= 4
SUB EAX, [0x123] ; ptr = 0x123; EAX -= *ptr

SUB AX, BX ; OK
SUB CL, DH ; OK

SUB 5, 10    ; Error!
SUB [0x0], [0x123] ; Error!
```

#### NEG

`NEG` bildet das Zweierkomplement (nicht `NOT`!), also flipped die Bits und addiert
eins. Der Befehl hat nur einen Operanden, muss also in bestimmten Faellen
typisiert werden. Der Operand muss ein Register oder eine Speicheradresse sein,
keine Konstante (da der negierte Werte ja zurueckgespeichert wird).

`NEG` ist aqeuivalent zu einem `NOT` und einem `INC`:

```asm
NEG EAX

; oder

NOT EAX
INC EAX

```

```asm
MOV AL, 1 ; AL = 0b00000001

NEG AL    ; AL = 0b11111111
```

#### INC

`INC` inkrementiert einen Wert um eins. Es hat eine implizite Konstante, braucht
also (als Instruktion) weniger Speicher und war deswegen (frueher) schneller als
`ADD` mit eins.

Eine wichtige bzw. interessante Eigenschaft von `INC` ist, dass `INC` nie das
Carry Flag (CF) setzt. Es kann also gut als (zyklischer) Counter verwendet werden,
ohne dass Flags gesetzt werden. Mit der Zero Flag (ZF) kann man trotzdem auf
Overflow reagieren. __Die Overflow Flag (OF) und Sign Flag (SF) werden trotzdem
ganz normal behandelt!__

__Speicheradressen muessen typisiert werden!!!__

```asm
INC EAX ; ++EAX

INC BL  ; ++BL

INC [0x0] ; Error!

INC WORD [0x0] ; OK
```

#### DEC

`DEC` dekrementiert einen Wert um eins. Es hat auch eine implizite Konstante
und braucht daher (als Instruktion) weniger Speicher. Es setzt auch das Carry Flag nicht, Overflow und Sign Bit aber schon.


```asm
DEC EAX ; --EAX

DEC AH  ; --AH
```

#### MUL

`MUL` ist der einfachste Befehl zum *unsigned* multiplizieren von Werten. Der
Befehl hat nur einen Operanden: ein Quell*register* als Multiplikator fuer EAX.
EAX ist also implizit der andere Operand und auch das Ziel der Operation. Der
Operand muss ein Register oder eine Speicheradresse sein. __Keine
Konstante!!!___ Wichtig ist, dass die Multiplikation meist 64-Bit ist, da bei
einer Multiplikation von zwei 32-Bit Werten maximal ein 64-Bit Wert rauskommen
kann. Die unteren 32-Bit bleiben in EAX gespeichert, die oberen 32-Bit
__ueberschreiben__ ein anderes Register. Welches haengt von der Breite des Operanden ab:

* Multiplikation mit einem 32-Bit Operanden: EDX:EAX
* Multiplikation mit einem 16-Bit Operanden: __DX:AX__
* Multiplikation mit einem 8-Bit  Operanden: AH:AL

Wobei in X:Y die oberen 32/16/8 Bit immer in X landen und die unteren in Y.

Overflowen die Bits nach EDX, wird die __OF und die CF__ (Overflow und Carry
Flag) gesetzt. Ist EDX nach der Multiplikation null, werden beide Flags nicht gesetzt.

```asm
MOV EAX, 5
MOV EBX, 5

MUL EBX    ; EAX = 5 * 5, EDX = 0


MOV EAX, 0x80000000
MOV EBX, 0x2

MUL EBX    ; EAX = 0, EDX = 1


MOV AL, 10
MOV AH, 123

MOV EBX, 5

MUL EBX    ; AL = 5 * 10, AH = 0 !!!
```

#### DIV

`DIV` ist der *unsigned* Divisionsbefehl in Assembler. Es arbeitet zusammen mit
dem Data Register EDX. EAX ist hierbei wie bei `MUL` implizit. Jedoch dividiert
`DIV` nicht nur den Wert in EAX, sondern auch den Wert des Restregisters. Die
zusammengehoerenden Register haengen wieder von der Groesse des Operanden
(Divisors) ab:

* Division mit einem 32-Bit Operanden: EDX:EAX
* Division mit einem 16-Bit Operanden: __DX:AX__
* Division mit einem 8-Bit  Operanden: AH:AL

Sei X:Y das Paar an Registern in Abhaengigkeit der Operandenlaenge. Bei einer
Division mit einem Operanden wird X und Y konkateniert als der Dividend
betrachtet. Also:

* `DIV BX` = (DX:AX) / BX
* `DIV CH` = (AH:AL) / CH
* `DIV EDX` = (EDX:EAX) / EDX

Das Resultat wird hierbei wieder aufgeteilt, naemlich in den ganzzahligen Wert
des Ergebnisses und dem Rest. Der ganzzahlige Teil wird in Y (z.B. EAX)
gespeichert und der Rest in X (z.B. EDX).

Also unbedingt darauf achten, dass der X Teil leer ist, wenn man nur Y durch einen Wert dividieren will.

```asm
MOV EAX, 5
MOV EBX, 4
DIV EBX     ; EAX = 1, EDX = 1

; Achtung: EDX ist nicht null, wuerde also mitgezaehlt werden

XOR EDX, EDX

MOV EBX, 5
DIV EBX     ; EAX = 1, EDX = 0
```

Die Flags sind nach einer Division *undefiniert*.

#### IMUL

`MUL` wurde urspruenglich fuer 16-Bit Maschinen implementiert, die nur 16-Bit
Register hatten. Also hatte man fuer einen zweiten 16-Bit Wert keinen Platz, und
musste den zweiten Operanden als EAX hard-coden. Spaeter, als fuer Instruktionen
mehr Platz zur Verfuegung kam, wurde `IMUL` hinzugefuegt.

`IMUL` (integer multiplication) ist flexibler, ist aber auch __signed__, es
erhaelt also das Vorzeichen. Im Grunde genommen ist es der natuerlichere
Multiplikationsoperator, da in Sprachen wie C auch erwartungsgemaess (signed)
$-16 \cdot 5 = -80$ und nicht (unsigned) $-16 \cdot 5 = 1200$.

`IMUL` ist auch viel flexibler bezueglich seiner Operanden, da es eine variable
Instruktionsgroesse hat. Die Operanden des Befehls koennen sein:

1. `REGISTER`
2. `ZIEL, REGISTER/SPEICHER/KONSTANTE`
3. `ZIEL, REGISTER/SPEICHER KONSTANTE`

Ziel und Multiplikator duerfen also beide Register oder __eine__ Speicheradresse
sein. Im ersten Fall ist EAX/AX/AL der implizite zweite Operand.

__Anders als bei `MUL` werden nicht immer die oberen Bits nach EDX
geschrieben. Die Operation ist einfach begrenzt durch die Groesse des Registers,
also wenn bei `IMUL BL, AL` das Ergebnis groesser 255 ist, overflowed der Wert
in BL. Die Overflow und Carry Flags werden dann gesetzt und die oberen Bits gehen also
verloren.__

```asm
IMUL, EBX  ; EAX *= EBX

IMUL EAX, EBX      ; EAX *= EBX
IMUL EAX, EBX, 3   ; EAX = EBX * 3
IMUL [0x3], EAX, 4 ; ptr = 0x3; *ptr = EAX * 4
```

#### IDIV

`IDIV` (integer division) ist wie `IMUL` zu `MUL`, also *signed*. Jedoch hat es
kein so flexibles Instruktionsformat, sondern dasselbe wie `DIV`, also EAX als
impliziten Dividenden (Ziel), wobei der Operand (Divisor) ein Register oder eine
Speicheradresse aber __keine Konstante__ sein kann. Der Rest wird wie bei `DIV`
aufgeteilt.

```
MOV EAX, 5
MOV EBX, 3
IDIV EBX    ; EAX /= EBX
```

### Bit Manipulation

#### NOT

`NOT` flipped die Bits wie `~` in C. Achtung: `NEG` bildet das zweier
Komplement, flipped die Bits und addiert 1. `NOT` flipped nur die Bits. Der
Operand muss ein Register oder eine Speichadresse sein.

```asm
MOV AL, 0  ; AL = 0b00000000

NOT AL     ; AL = 0b11111111
```

#### AND

`AND` fuehrt die logische Operation AND wie `&` in C durch. Die Operanden
duerfen dabei beide Register sein, oder eine Konstante oder eine
Speicheradresse.

```
MOV AL, 5  ; AL = 0b00000101

AND AL, 10 ; AL &= 0b00001010 (= 0b00001111)

AND Al, BL ; AL &= BL
```

`AND` kann dazu genutzt werden, Bits in einem Wert zu loeschen. Will man den
$n$-ten Bit eines Werts loeschen, shiftet man eine 1 $n$ mal nach links (oder
denkt sich die entsprechende Maske in Binaer/Hex aus), negiert die Maske und
`AND`-ed dann (`x &= ~(1 << n)`)

```asm
; Ziel: Unset 2. Bit in EAX

MOV EBX, 1
SHL EBX, 2
NOT EBX
AND EAX, EBX

; Oder

AND EAX, 0xFB

; Oder

AND EAX, 11110111b
```

Um einen einzigen Bit in einem Wert zu checken, kann man ihn auch mit einer
entsprechenden Maske AND-en. Ist das Ergebnis $0$, war der Bit nicht
gesetzt. Allgemein kann man so checken, ob mindestens einer von allen Bits in
der Maske gesetzt ist:

```asm
; Ziel: Ist Bit 2 in EAX gesetzt?

AND EAX, 0x4
JZ was_not_set

AND EAX, 0x7
JZ none_of_lower_3_bits_was_set
```

Will man checken, ob alle Bits einer Maske in einem Wert gesetzt sind, AND-ed
man den Wert zuerst mit der Maske, und sieht dann ob das Ergebnis *gleich der
Maske ist*.

```asm
; Ziel: Sind Bits 1-5 in EAXB gesetzt?
AND EAX, 0x3E
CMP EAX, 0x3E
JE all_bits_are_set

; else at least one bit was not set
```

Eine weitere Anwendung von AND ist eine Modulo Operation mit einer 2-er
Potenz. Man will also eine Zahl $\mod n$ berechnen wo $n = 2^k, k \in
\mathcal{N}_0$. Hierbei muss man bedenken, dass z.B. wenn $n = 4$ die unteren
zwei Bits (also $k$ in $n^k$) die Werte $0, 1, 2, 3$ annehmen koenen, also genau
die Restklassen. Dies gilt auch fuer alle anderen Zweierpotenzen. Also:

$x \mod 2^k =$ `AND x, k-Einsen`.

bzw. da $2^k - 1$ genau diese $k$ Einsen sind, gitl auch:

$x \mod 2^k =$ `AND x, (2^k)-1`

```asm
AND EAX, 11111b ; Modulo 32

AND EAX, 127 ; Modulo 128
```

#### OR

`OR` fuehrt die logische Operation OR wie `|` in C durch. Die Operanden duerfen
dabei beide Register sein, oder eine Konstante oder eine Speicheradresse.

```
MOV AL, 5  ; AL = 0b00000101

AND AL, 10 ; AL &= 0b00001010 (= 0b00001111)
```

Mit `OR` kann man Bits setzen. Um den $n$-ten Bit zu setzen shiftet man eine $1$
wieder $n$ Mal nach links und `OR`-ed dann den gewuenschten Wert.

```asm
; Ziel: Set 7. Bit in ECX

MOV EAX, 1
SHL EAX, 7
OR ECX, EAX

; Oder

OR ECX, 0x80

; Oder

OR ECX, 10000000b
```

#### XOR

`XOR` fuehrt die logische Operation XOR wie `^` in C durch. Die Operanden
duerfen dabei beide Register sein, oder eine Konstante oder eine
Speicheradresse.

`XOR` ist vorallem nuetzlich, um einen Wert auf Null zu setzen. Bei einem `MOV,
0` muss die Konstante $0$ erst aus dem Hauptspeicher geholt werden bzw. aus dem
Cache gelesen werden, und die Instruktion wird dadurch auch groesser. Wendet man
aber `XOR` auf einen Wert selbst an, muss keine Konstante gefetched werden, es
ist also schneller!

```asm
MOV AL, 5 ; AL = 0b00000101

XOR AL, 6 ; AL ^= 0b00000110 (= 0b00000011)
```

#### TEST

`TEST` ist zu `AND` wie `CMP` zu `SUB`: es bestimmt, ob im Operanden bestimmte
Bits gesetzt sind, indem es einfach ein `AND` mit der gegebenen Maske (zweiter
Operand) macht. Wird dabei die Zero Flag (ZF) gesetzt, so war keiner der Bits gesetzt.

Ebenso wie `XOR EAX, EAX` effizienter ist als `MOV EAX, 0`, weil die Konstante
die Instruktion langsamer macht, ist auch `TEST EAX, EAX` gefolgt von `JE/JZ`
eine effizientere Methode um zu checken, ob *EAX 0* ist. Sobald ein Bit in EAX
gesetzt ist, ist das Resultat von `EAX & EAX` nicht mehr null.

```asm
TEST EAX, 0x3
JNZ at_least_one_bit_was_set
```

#### SHL

`SHL` fuehrt eine *logische* left-shift Operation wie `<<` in C durch. Der linke
(Ziel-) Operand muss ein Register oder eine Speicheradresse sein, der rechte
Operand __muss eine Konstante, oder CL sein__. Ein left-shift ist eine
effiziente Weise, __eine unsigned Zahl mit $2^n$ zu multiplizieren__. Freie
Stellen (rechts) werden mit $0$ aufgefuellt.

Schiebt man einen Bit ausserhalb der Breite des Operanden, wird das Carry Bit
gesetzt (Overflow Flag wird nicht beeinflusst).

```
MOV AL, 1 ; AL = 0b00000001

SHL AL, 5 ; AL <<= 5 (= 0b00100000)


MOV EAX, 5

SHL EAX, 2 ; = 5 * 2^2 = 20


MOV CL, 3
SHL EAX, CL ; OK!

SHL EAX, ECX ; Error!
SHL EAX, BL  ; Error!
```

#### SHR

`SHR` fuehrt eine *logische* right-shift Operation wie `>>` in C durch. Der
linke (Ziel-) Operand muss ein Register oder eine Speicheradresse sein, der
rechte Operand __muss eine Konstante, oder CL sein__. Ein right-shift um einen
Bit ist eine effiziente Weise, eine __unsigned Zahl durch $2^n$ zu
dividieren__. Freie Stellen (links) werden mit $0$ aufgefuellt.

Schiebt man einen Bit ausserhalb der Breite des Operanden, wird das Carry Bit
gesetzt.

```
MOV AL, 0x80 ; AL = 0b10000000

SHR AL, 5 ; AL >>= 5 (= 0b00000100)
```

#### SAL

`SAL` fuehrt eine *arithmetische* left-shift Operation durch. Eine arithmetische
left-shift Operation unterscheidet sich von einer logischen dadurch, dass beim
arithmetischen Shiften der __Sign Bit erhalten bleibt__. Shiftet man mit `SAL`
also um eine Stelle nach links, wird der Sign-Bit also zuerst rausgeshiftet,
dann aber wieder reingeschrieben (ueberschreibt dann den eben in die Position
des Sign-Bits geshifteten Bit). Hiermit kann man also eine __signed
Multiplikation mit $2^n$__ machen!

Schiebt man einen Bit ausserhalb der Breite des Operanden, wird das Carry Bit
gesetzt.

```ASM
MOV EAX, -5

SAL EAX, 3 ; -5 * 2^3 = -40
```

#### SAR

`SAR` fuehrt eine *arithmetische* right-shift Operation durch. Eine
arithmetische right-shift Operation unterscheidet sich von einer logischen
dadurch, dass beim arithmetischen Shiften der __Sign Bit erhalten
bleibt__. Shiftet man mit `SAR` also um eine Stelle nach right, wird der
Sign-Bit also zuerst nach innen geshiftet, dann aber wieder reingeschrieben
(ueberschreibt dann die eben in die Position des Sign-Bits geshifteten
Null). Hiermit kann man also eine __signed Division mit $2^n$__ machen!

Schiebt man einen Bit ausserhalb der Breite des Operanden, wird das Carry Bit
gesetzt.

```ASM
MOV EAX, -12

SAR EAX, 2 ; -12 / 2^2 = -3
```

#### ROL

`ROL` (rotate left) rotiert die Bits des Wertes um seine Breite. Pro Rotation um
eine Stelle wird also der MSB in die Stelle des LSB gegeben, und alle anderen
Bits um eine Stelle nach links geschoben. Diese Operation hat keine sinnvolle
arithmetische Bedeutung.

```asm
MOV EAX, 10000001b

ROL EAX, 1 ; 00000011
```

#### ROR

`ROR` (rotate right) rotiert die Bits des Wertes um seine Breite. Pro Rotation
wird also der LSB in die Position des MSB gegeben, und alle anderen Bits um eine
Position nach rechts geschoben. Diese Operation hat keine sinnvolle
arithmetische Bedeutung.

```asm
MOV EAX, 10000001b

ROR EAX, 1 ; 11000000
```

#### RCL

`RCL` (rotate left through carry) shiftet den MSB immer in das Carry Bit des
Statusregisters, und shiftet das (vorherige) Carry Bit in die Position des LSB
(also anstatt des MSB wie bei `ROL`). Diese Operation hat keine sinnvolle
arithmetische Bedeutung.

```asm
CLC ; Clear Carry Flag

MOV EAX, 10000001b ; CF = 0

RCL EAX, 1 ; EAX = 00000010, CF = 1!

RCL EAX, 1 ; EAX = 00000101, CF = 0
```

#### RCR

`RCR` (rotate right through carry) shiftet den LSB immer in das Carry Bit des
Statusregisters, und shiftet das (vorherige) Carry Bit in die Position des MSB
(also anstatt des LSB wie bei `ROR`). Diese Operation hat keine sinnvolle
arithmetische Bedeutung.

```asm
CLC ; Clear Carry Flag

MOV EAX, 10000001b ; CF = 0

RCR EAX, 1 ; EAX = 01000000, CF = 1!

RCR EAX, 1 ; EAX = 10100000, CF = 0
```

### Vergleiche

Vergleiche zwischen Werten werden in Assembler allgemein mit dem `CMP` Befehl
gemacht, welcher eine Abstraktion fuer eine Subtraktion `SUB` ist. Da `SUB` eine
arithmetische Operation ist, setzt oder cleared es bestimmte Flags im Status
Register. Bestimmte Kombinationen von diesen Flags stehen dann fuer bestimmte
Kombinationen.

Man moechte also z.B. den Wert in EAX mit dem Wert in EBX vergleichen. Dann kann
man das (low-level) mit einer Subtraktion machen: `SUB EAX, EBX`. Waren die
beiden Registerwerte nun gleich, ist das Ergebnis null. Also das Zero Flag wird
gesetzt sein. War EAX kleiner als EBX, ist das Ergebnis negativ. War EAX
groesser, ist das Ergebnis positiv etc.

Hierbei muesste man aber `EAX` veraendern, was man nicht unbedingt will. Der
Command `CMP` (compare) macht diese Subtraktion und setzt die Statusflags, aber
ohne `EAX` zu veraendern. `CMP` hat dabei als zwei Operanden entweder Register,
oder __eine Konstante__ oder __eine Speicheradresse__ und ein Register/eine
Konstante.

`CMP` funktioniert fuer negative Werte wie erwartet. Also man kann `CMP EAX, 0`
machen und dann checken, ob der Wert negativ ist (`JL`) oder nicht.

```
SUB EAX, EBX

; oder

CMP EAX, EBX     ; OK

CMP EAX, 5       ; OK
CMP 5, 4         ; error

CMP EAX, [0x0]   ; OK
CMP [0x0], [0x1] ; error (generally for ISA)
```

#### Flag Kombinationen

Die genauen Flag Kombinationen sind hier aufgelistet. Dabei wird angenommen, das
unmittelbar zuvor ein `CMP` Befehl ausgefuehrt wurde. In der *CC*-Spalte
(CC = Condition Code)
stehen die Condition Codes, wie sie dann fuer bedingte Spruenge verwendet werden
koennen. Fuer manche gibt es mehrere Aliase, z.B. damit man `JZ` (jump zero)
schreiben kann anstatt `JE` (jump equal), wenn man `CMP EAX, 0` macht.


| CC    | Erlaeuterung                         | Flags              |
|:------|:-------------------------------------|:-------------------|
| C     | Carry                                | CF = 1             |
| NC    | No Carry                             | CF = 0             |
| E/Z   | Equal / Zero                         | Z = 1              |
| NE/NZ | Not Equal / Not Zero                 | Z = 0              |
| O     | Overflow                             | OF = 1             |
| NO    | Not Overflow                         | OF = 0             |
| G/NLE | (signed) Greater / Not Less or Equal | SF = OF and ZF = 0 |
| GE/NL | (signed) Greater or Equal / Not Less | SF = OF            |
| L/NGE | (signed) Less / Not Greater or Equal | SF != OF           |
| LE/NG | (signed) Less or Equal / Not Greater | SF != OF           |

Es reicht fuer `G`/`NLE` nicht aus nur das Sign Bit zu betrachten. Es ist zwar
ausreichend, wenn man nur mit positiven Zahlen arbeitet, z.B. $5$ mit $127$
vergleicht. Aber, man nehme den Vergleich zwischen $5$ und $-127$, also `CMP 5,
-127`. $5$ ist natuerlich groesser, aber wenn man $5 - (-127) = 5 + 127 = 133$
rechnet, wird das Sign Bit gesetzt (passt nicht mehr in die signed Range), was
bei $5 - 127$ darauf hindeuten wuerde, dass $5 < 127$. Da bei einer *signed*
Interpretation des Ergebnisses aber auch ein Overflow passiert (Addition zweier
positiver Zahlen die eine negative Zahl ergibt), kann man hier sagen, dass $5 >
-127$. Umgekehrt, wuerde ein Overflow passieren, aber das Sign Bit nicht gesetzt
werden.

Flags werden immer in Abhaengigkeit des Ergebnisses der letzten Operation
veraendert (oder nicht).

Befehle, die die Flags meistens veraendern:
`ADD`, `SUB`, `MUL`, `NEG`, `CMP`, `TEST`, `AND`, `OR`, `XOR`

Befehle, die die Flags meistens *nicht* veraendern:
`MOV`, `PUSH`, `POP`, `JMP`, `CALL`, `RET`

Da Transportbefehle die Flags nicht veraendern kann es sein, dass eine Operation
die normalerweise keine Flag setzten wuerde, schon eine Flag setzt weil sie
vorher nicht von den Transportbefehlen gesetzt wurde. Z.B:

```asm
mov eax, -5 ; setzt das sign bit nicht
add eax, 0  ; setzt das sign bit jetzt!
```

### Spruenge

Der x86 Prozessor hat intern einen 32-Bit Befehlszaehler (Instruction Pointer,
IP), der auf die Adresse der gerade ausgefuehrten Instruktion im Speicher zeigt
(Von-Neumann Modell). Man kann diesen IP nicht direkt in seinem Code
manipulieren, das geht nuer ueber *Spruenge* -- Veraenderungen im sequentiellen
Ablauf.

Spruenge koennen bedingt oder unbedingt sein. Bedingte Spruenge springen nur,
wenn eine Bedingung erfuellt ist. Unbedingte Spruenge manipulieren den
Befehlszaehler immer.

#### Unbedingte Spruenge

Unbedingte Spruenge koennen mit `JMP` durchgefuehrt werden. `JMP` hat dabei als
einzigen 32-Bit Operanden entweder eine konstante Adresse, z.B. `0x123`, ein
Register (bzw. den Wert/die Adresse, der/die in diesem Register gespeichert
ist), oder ein Label.


```asm
jmp 0x123
jmp eax

label:

jmp label
```

JMP ist also equivalent zu (was aber so in Assembler nicht geht):

```asm
MOV IP, label
```

#### Bedingte Spruenge

Neben unbedingten Spruengen gibt es noch jene, die nur springen, wenn eine
bestimmte Bedingung erfuellt ist. Diese Bedingung wird immer in Verbindung mit
den *Status-Flags* ueberprueft. Die einzelnen Status-Codes stehen oben in einer
Tabelle. Bedingte Spruenge koennen nur zu Labels sein, nie zu Registern.

Wenn man nun also einen bedingten Sprung durchfuehren will, macht man zuerst
einen Vergleich mit `CMP` und dann ein `J<CC>` wo `<CC>` eben einer der oben
angefuehrten condition-codes ist.

Ein wichtiger Unterschied zu dem, wie man in Hochsprachen wie `C` eine Bedingung
schreibt (`if`-clause), ist dass man bei Assembler nicht die positive Bedingung
beschreibt, und dann etwas ausfuehrt. Sondern, man negiert die gewollte
Bedingung, und springt in diesem Fall (wenn also die Negation des gewollten
eintritt) zum `else` bzw. einfach "zum Ende vom Scope". Wenn die Bedingung, die
man testen will (`if (condition)`) also *nicht* wahr ist, dann ueberspringt man
den folgenden Code ("im Scope" des `if`), und sonst geht der Kontrollfluss
weiter.

##### if

Frage: Wie sieht der unten stehende Code in Assembler aus?

```C
if (x == 0) x = 1;

++x;
```

Antwort (Sei $x := EAX$):

```asm
cmp EAX, 0
jnz end     ; Negation fuehrt zum jump!
mov EAX, 1
end:
INC EAX
```

Oder:

```C
if (x <= y) ++x;
```

(Sei $x := EAX, y := EBX$)

```asm
cmp EAX, EBX
jg end
inc EAX
end:
```

##### if-else

Fuer ein else macht man einfach ein eigenes "Scope" bzw. Label vor dem `end`
label:

```C
if(x == 10) x = 0;

else ++x;
```

(Sei $x := EAX$)

```asm
cmp EAX, 10

jne else    ; Negation der Bedingung

mov EAX, 0
jmp end     ; Muessen dann else ueberspringen

else:
inc EAX     ; Fliesst dann weiter zu end:

end:
```

https://en.wikibooks.org/wiki/X86_Disassembly/Branches

### Schleifen

Schleifen sind mit bedingten Spruengen ganz einfach auszufuehren und man sieht,
wie wenig Aufwand hinter einer Schleife wirklich steckt.

#### Do-While

Die einfachste Art von Schleife ist in Assembler die `do-while` Schleife. Der
Grund ist, dass zuerst der Code kommt, und dann am Ende nur die Bedingung
ueberprueft werden muss, ob man zurueck zum Start des Codes springt, oder
einfach zur naechsten Instruktion geht.

```
do ++x;
while (x < 10);
```

($x := EAx$)

```asm

start:
inc EAX

cmp EAX, 10
jl start
```

#### While

Eine `while` Schleife unterscheidet sich von einer `do-while` Schleife nur
dadurch, dass die Schleifenbedingung schon am Anfang, vor dem ersten Ausfuehren
des Codes ueberprueft wird:

```C
while (x < 10) ++x;
```

```asm
cmp EAX, 10
jnl end

start:
inc eax
cmp eax, 10
jl start

end:
```

Was sehr interessant ist: de-assembliert man diese Schleife nun, so erhalt man
den folgenden `C` Code:

```C
if (eax < 10) // Bedingung vom Assembler wieder negiert
{
	do ++x;

	while (x < 10);
}
```

Eine andere intuitive Moeglichkeit fuer eine `while` Schleife ist, die Bedingung
an den Start des Labels zu geben, und am Ende des Codes einfach wieder zur
Bedingung zurueck zu springen:

```asm

start:
cmp EAX, 10
jnl end

inc EAX

jmp start

end:

```

Oder man gibt die Bedingung in ein eigenes Label ans Ende:

```asm

jmp condition

start:
inc EAX

condition:
cmp EAX, 10
jl start

```

Exakt gleich viele Zeilen Code. Der Unterschied ist wirklich nur, ob wir den
Start zur Bedingung durchfallen lassen, oder den Code. Die erste Variante ist
vom eigentlichen Fluss in C natuerlicher (setup, condition check, code,
increment, condition check, if not met: end).

#### For

Eine `for` Schleife laesst sich ganz leicht in eine `while` Schleife zerlegen:

```C
for (x = 0; x < 10; ++x) { ... }

// gleich wie

x = 0

while (x < 10)
{
	...
	++x
}

// bzw. allgemein

for(setup; condition; increment) { code }

// oder

setup
while (condition)
{
	code
	increment
}
```

Also, C zu Assembler:

```C
y = 5;

for (i = 0; i < 10; ++x)
{
	y += x;
}
```

($x := EAX, y := EBX$)

```asm
mov EBX, 5

; setup
mov ECX, 0

condition:
cmp ECX, 10
jnl end

; code
add EBX, EAX

; increment
inc ECX
jmp condition

end:
```

Oder (wieder exakt gleich viele Zeilen Code)

```asm
mov EBX, 5

; setup
mov EAX, 0
jmp condition

code:
add EBX, EAX

; increment
inc EAX

; condition
cmp EAX, 10
jl code
```

https://en.wikibooks.org/wiki/X86_Disassembly/Loops

#### LOOP

`LOOP` ist ein spezieller, auf das CX (*Counter*) Register ausgerichteter
Befehl, der sehr gut fuer Schleifen geeignet ist. `LOOP` dekrementiert zuerst
den Wert in CX, und springt dann zum gegebenen Label, wenn CX nicht null
ist. Der einzige Operand des Commands ist das Label, zu welchem es springen
soll.

Um also $n$ mal zu loopen, muss man in ECX oder CX $n$ schreiben, und ruft dann
am Ende seines Blockes `LOOP`. Schreibt man in ECX $0$ oder einen negativen Wert, geht der Loop die ganze negative Range durch.

```asm
mov ecx, n

label:
; code
loop label
```

Aequivalent zu:

```asm
mov ecx, n

label:
; code
dec ecx
jnz label
```

bzw. als for Loop:

```asm
xor ecx, ecx

label:
cmp ecx, n
jnl end

; code

inc ecx
jmp label

end:
```

### Stack

Ein Stack ist allgemein eine LIFO (Last-In, First-Out) Datenstruktur, die eine
leichte, "selbstorganisierende" Datenorganisation erlaubt. Benutzer des Stacks
muessen sich nicht um Adressen kuemmern, es gibt nur "oben".

In x86 waechst der Stack *nach unten*, also von hoeheren Adressen zu niedrigeren
Adressen hin. Der ESP (Extended Stack Pointer) __zeigt immer auf den obersten
(aktuellen) Wert__ des Stacks (falls es einen obersten Wert gibt, ansonsten ist
der ESP ungueltig). Am Anfang zeigt der ESP also auf die hoechste Adresse,
welche initial ungueltig ist, weil eben noch kein Wert auf dem Stack
liegt. Belegt der Stack also 128 Byte, zeigt der ESP ganz am Anfang (bzw. wenn
man alles vom Stack runterloescht) auf Adresse $7F_{16} = 128_{10}$. Da
Speicheradressen $0$-indiziert sind, also nur von $0$ bis $127$ gehen, ist der
ESP anfangs also ungueltig, man darf also keinen Wert zu diesem Zeitpunkt lesen.

Ein Grund, wieso der Stack nach unten waechst, koennte sein, dass dadurch im
Little-Endian Format Daten leichter abzulegen sind. Denn so kann man ein Datum
vom Stack Pointer aufwaerts ohne Probleme im Little-Endian Format ablegen.
Wuerde der Stack von niedrigeren zu hoeheren Adressen wachsen, wuerde der ESP
immer an der oberen Byte-Grenze eines Wortes stehen. Ein Datum muesste dann also
rueckwaerts vom ESP abwaerts in den Stack geschrieben werden.

Der Stack erlaubt zwei (und nur zwei) Operationen, um den ESP sowie die Werte
auf dem Stack zu manipulieren: `PUSH` und `POP`.

__Es ist essentiell genau so oft zu `PUSH`en wie man `POP`ed und umgekehrt!__

Vorallem in Unterprogrammen ist es nuetzlich, Register auf dem Stack zu
sichern. Z.B. will man in einem Unterprogramm EAX nuetzen, dern Caller aber
nicht veraergern, dass seine Daten nach dem Unterprogramm weg sind, so `PUSH`ed
man EAX am Anfang des Unterprogramms auf den Stack ("sichern") und `POP`ed es am
Ende wieder nach EAX. Dann ist EAX relativ zum vorherigen Programm unveraendert.

#### PUSH

`PUSH` ist der Befehl, um neue Werte auf den Stack zu schreiben. Er nimmt nur
einen Operanden, naemlich das/die Register/Konstante, das/die auf den Stack
gelegt werden soll. Der Operand muss 16-Bit/2-Byte oder 32-Bit/4-Byte breit
sein. Die Groesse wird aus dem Operanden geschlossen (z.B. EAX = 4 Byte, AX = 2
Byte), oder kann z.B. fuer Konstanten auch durch Typisieren festgelegt werden.

Es koennen nur 16/32-Bit Adressen geschrieben werden, und nicht 8-Bit, weil
sonst alignment Probleme passieren koennten. Schreibt man z.B. einen Byte auf
den Stack, sind danach alle weiteren 2/4-Byte werde misaligned.

In x86 sind Konstanten normalerweise immer 32-Bit, aber `PUSH` interpretiert
eine Konstante by default als 16-Bit. Man muss also mit `dword` typisieren, wenn
man eine 32-Bit Konstante will.

```
PUSH EAX ; push 4-bytes of EAX onto stack

PUSH 123 ; push 123 as 2-byte constant onto stack

PUSH DWORD 123 ; push 123 as 4-byte constant onto stack

PUSH AL ; Error! Must be 16 or 32-Bit
```

Eine `PUSH` Operation ist aequivalent (und kann auch so ausgefuehrt werden)
dazu, den ESP um 2/4 Bytes zu __dekrementieren__ (der Stack waechst von hoeheren
zu niedriegern Adressen hin) und den Operanden dann an die von ESP gezeigte
Adresse zu schreiben.

```
PUSH EAX

; oder

SUB ESP, 4      ; ESP -= 4
MOV [ESP], EAX  ; *ESP = EAX
```

#### POP

Eine `POP` Operation macht das Gegenteil von `PUSH`: es liest einen Wert vom
Stapel und inkrementiert den `ESP`. Der einzige Operand von `POP` ist die/das
Speicheradresse/Register, in welches der Befehl den Wert am Stack schreiben
soll. Es koennen wieder nur 16-Bit oder 32-Bit Adressen gelesen werden.

```
POP EAX   ; EAX = *ESP; ESP += 4

POP [EAX] ; *EAX = *ESP; ESP += 4

POP [0x0] ; default WORD, also 2 bytes

POP DWORD [0x0]; 4 bytes
```

PUSH war aequivalent dazu, den ESP um 2/4 zu verringern, und den Operanden nach
ESP zu schreiben. POP ist nun aequivalent dazu, den Wert zu lesen, und dann den
ESP *nach oben* um 2/4 Bytes zu __inkrementieren__. (2/4 haengt wieder vom
Operanden ab).

```
POP EAX

; oder

MOV EAX, [ESP] ; *EAX = *ESP
ADD ESP, 4     ; ESP += 4
```

Will man einen Wert auf dem Stack "wegwerfen" (discarden), kann man den ESP
einfach um die jeweilige Breite des Werts (16/32 Bit) verringern:

```
PUSH EAX
PUSH EBX
PUSH ECX

ADD ESP, 12 ; fuck 'em
```

http://eli.thegreenplace.net/2011/02/04/where-the-top-of-the-stack-is-on-x86/
http://stackoverflow.com/questions/2586591/why-is-it-not-possible-to-push-a-byte-onto-a-stack-on-pentium-ia-32

### Unterprogramme

Unterprogramme sind vom Konzept her vergleichbar mit Funktionen, sind aber aufs
wesentliche reduziert: Ein Label. Ein Unterprogramm wird also durch ein Label
(Marke) irgendwo im Programm definiert. Man kann dann unter diesem Label
bestimmten Code auslagern.

Es gibt dann spezielle Sprungbefehle, die es einem erlauben, den Kontrolfluss
von oder zu Unterprogrammen zu lenken. Diese Befehle, `CALL` und `RET`, sind
aehnlich wie `PUSH` und `POP` und machen vom Stack auch Gebrauch.

#### `CALL`

`CALL` wird benutzt, um den Kontrollfluss des Programms *zu* einem Unterprogramm
zu lenken. Der einzige Operand ist gleich wie fuer `JMP` eine konstante Adresse
wie `0x123`, ein Register wie `EAX`, oder ein Label. CALL fuehrt dabei zwei
Operationen durch:

1. Es legt die 32-Bit Ruecksprungadresse^1 auf den Stack (der ESP zeigt dann auf
   diese).

2. Dann macht es einen `JMP` zum Operanden.

^1: Die Ruecksprungadresse ist die Adresse unmittelbar nach dem `CALL`.

```asm
MOV EAX, 0
...
CALL label

XOR EBX, 5 ; <--- Ruecksprungadresse (bei dieser Instruktion)
```

#### `RET`

`RET` zu `CALL` ist wie `POP` zu `PUSH`, waehrend `CALL` die Ruecksprungadresse
auf den Stack legt und zum Operanden springt, `POP`ed `RET` die
Ruecksprungadresse in den Befehlszaehler (intern).

```
foo:
mov eax, 5
ret         ; POP IP (pop into instruction pointer, not really possible)

call foo    ; POP next_line; JMP foo
```

#### Calling Conventions

Um das Schreiben von Unterprogrammen zu normieren und das Verteilen und
Modifizieren von Code zwischen Programmierern zu erleichtern, gibt es normierte
Verfahren, wie man eine Funktion aufrufen und letztlich von dieser
zurueckspringen soll. Da es in Assembler keine expliziten Parameter oder
Rueckgabewerte gibt, muss man festlegen, wie Parameter uebergeben werden, welche
Register modifiziert werden duerfen und welche nicht und wie Werte
zurueckgegeben werden. Diese *normierten* Verfahren nennt man *Calling
Conventions*.

Die Calling Convention in C ist sehr stark Stack-basiert, benutzt also die
Befehle `push`, `pop`, `call` und `ret`. Register werden auf dem Stack
gesichert, Parameter werden auf dem Stack uebergeben und auch lokale Variablen
residieren im Stack ("allocate on the stack").

Calling conventions werden in zwei Teile aufgeteilt: *Caller* Conventions und
*Callee* Conventions. Also jene Regeln, die der Aufrufer einer Funktion befolgen
muss und jene Regeln, die die aufgerufene Funktion (bzw. ihr Schreiber) befolgen
muss.

##### Callee Conventions

Um ein Unterprogramm aufzurufen, sollte ein *Aufrufer* (*Callee*):

1. Die als *callee-saved* designierten Register EAX, ECX und EDX sichern, da es
   einer Funktion erlaubt ist, diese Register ohne vorheriges Sichern zu
   benutzen und zu modifizieren.
2. Funktionsparameter auf den Stack `push`en. Hierbei sollten die Parameter in
   umgekehrer Reihenfolge gepusht werden, sodass der erste Parameter (einer
   Signatur, z.B. `x` in `foo(int x, int y, int z)`) an der *niedrigsten*
   Adresse im Stack steht (der Stack waechst zu niederigen Adressen hin). Diese
   Art der Parameteruebergabe nennt sich *cdecl*, fuer "C Declaration", und hat
   den Vorteil, dass die Parameterzahl dynamisch sein kann. Der erste Parameter
   wird vom Base Pointer aus naemlich immer acht Bytes (Ruecksprungadresse
   dazwischen) darueber sein, und dann kann man variabel den Stack hoch
   gehen. Umgekehrt wuesste man nicht, wo der erste Parameter ist, wenn man
   nicht weiss, wieviele es gibt.
3. Um die Funktion zu rufen, benutze `CALL`. Diese Instruktion legt die
   Ruecksprungadresse auf den Stack, und springt dann zum Unterprogramm.

Nachdem das Unterprogramm terminiert (nach `RET`) kann der Aufrufer davon
ausgehen, dass der __Rueckgabewert in EAX liegt__.

Um den vorherigen Stand der Maschine wiederherzustellen, sollte der Aufrufer
nach dem Terminieren des Unterprogramms:

1. Die Parameter vom Stack nehmen. Dies geht am einfachsten mit einer
   Addition zu `ESP`.
2. Die *callee-saved* Register EAX, ECX, EDX, welche die Funktion modifizieren
   durfte, wieder vom Stack nehmen (falls sie Anfangs gepushed wurden).

Beispiel, fuer eine Funktion mit der C Signatur: `int foo(int x, int y)`:

```asm
; don't want to lose our data!
push eax
push ecx
push edx

push dword [0x123] ; last parameter y  (32-Bit value)
push dword 55      ; first parameter x (32-Bit value)

call foo     ; puts return address on stack and jumps to foo

; return value is in EAX

; remove parameters (2 * 4 Bytes)
add esp, 8

; restore registers
pop edx
pop ecx
pop eax
```

##### Caller Conventions

Ein Unterprogramm sollte, bevor es mit seinem eigentlichen Code anfaengt:

1. Den momentanen EBP auf den Stack pushen. Der EBP wird immer als Zeiger auf
   den Aktivierungsblock, also den *Stack Frame* der Funktion benutzt. Er wird
   fuer die Funktion genutzt werden, jedoch wird er momentan von der aufrufenden
   Funktion genutzt. Also muss man ihn sichern.
2. Den EBP auf den ESP zeigen lassen. Der EBP wird folglich als Basis fuer
   sowohl Parameter- als auch lokale Variablenzugriff verwendet -- eben als
   Base-Pointer. Also wird der EBP nun auf seine -- fuer den Funktionsaufruf --
   endgueltige Position gelegt. Diese Position ist immer die Adresse des
   vorherigen Base-Pointers, der in (1) auf den Stack gesichert wurde. Der ESP
   wird danach weiter zum navigieren auf dem Stack verwendet.
3. Lokale Variablen allokieren. Diese Allokation (*on the stack*) wird so
   durchgefuehrt, dass man den ESP einfach um die Groesse aller Variablen in
   Bytes dekrementiert (Stack waechst nach unten). Z.B., hat man zwei 4-Byte
   lokale Variablen, wird hier `sub esp, 8` gemacht.
4. Speichere die *callee-saved* Register EBX, EDI und ESI. Diese Register
   duerfen laut Calling Convention vom Unterprogramm nicht modifiziert werden,
   muessen also von der Funktion gesichert werden, wenn man sie benutzen will.

Der Base-Pointer wird nun zum adressieren sowohl der Parameter als auch der
lokalen Variablen benutzt:

* Er zeigt immer auf den alten EBP.
* Bei EBP+4 liegt die Ruecksprungadresse.
* Bei EBP+8 liegt der erste (4-Byte) Parameter, bei EBP+12 der zweite etc.
* Bei EBP-4 liegt die erste (4-Byte) lokale Variable, bei EBP-8 die zweite etc.

```
[   Last Parameter       ]
[          ...           ]
[   First Parameter      ]
[   Return Address       ]
[   Old EBP              ] <-- EBP
[   First Local Variable ]
[          ...           ]
[   Last Local Variable  ]
[          EBX           ]
[          ESI           ]
[          EDI           ] <-- ESP (if no other values pushed yet)
```

Nachdem dieser *Prologue* der Funktion ausgefuehrt wurde, darf der
Funktionskoerper ausgefuehrt werden. Danach folgt der (relativ symmetrische)
*Epilogue*.

Um die Funktion zu verlassen, muss der Callee:

1. Den Rueckgabewert in EAX lassen.
2. Die *callee-saved* Register EBX, ESI, EDI wiederherstellen.
3. Die lokalen Variablen deallokieren. Dies ginge, indem man die Gesamtgroesse
   wieder zum ESP addiert. Jedoch ist es sicherer, den ESP einfach wieder auf
   den EBP zeigen zu lassen: `MOV ESP, EBP`
4. Stelle den alten EBP (der Base Pointer der aufrufenden Funktion) wieder her:
   `PUSH EBP`
5. Verlasse die Funktion mit `RET`.

Beispiel Callee Conventions fuer die Funktion:

```C
int add_three(int x, int y, int z)
{
	int a;

	a = z;
	a += y;

	x += a;

	return x;
}
```

```asm
add_three:

; secure and setup EBP
push ebp
mov ebp, esp

; allocate one 4-byte local variable on the stack
sub esp, 4

; secure registers
push edi
push esi

mov eax, [ebp+8]  ; put first parameter into return register
mov esi, [ebp+12] ; grab second parameter
mov edi, [ebp+16] ; grab third parameter

mov [ebp-4], edi  ; move the third parameter into the local variable
add [ebp-4], esi  ; add second parameter to local variable
add eax, [ebp-4]  ; add local variable to return value

; restore registers
pop esi
pop edi

; discard local variable
add esp, 4

; restore old ebp
pop ebp

; return to caller
ret
```

(Bottom:) http://www.cs.virginia.edu/~evans/cs216/guides/x86.html#registers
https://en.wikibooks.org/wiki/X86_Disassembly/Functions_and_Stack_Frames

## Makros

Eine Alternative zu Unterprogrammen sind Makros. Wie in C oder C++ sind Makros
selten eine gute Wahl, da sie den Code, den sie implementieren, bei jedem Aufruf
duplizieren. Fuer kleine Codesequenzen kann es aber effizienter sein, da ein
Unterprogramm bedingte Spruenge und Fetchen von Adressen umfasst. Ein Makro ist
wirklich nur eine 1-zu-1 Kopie des Codes an die Aufrufstelle und hat daher
keien Overhead fuer Calls.

Die Erweiterung der Makros uebernimmt der Assemblierer (wie der Praeprozessor in
C/C++). Die Ersetzung von Parametern ist hierbei wirklich textuell.

Fuer NASM sieht das Schema von Makros so aus:

```asm
%macro <macro_name> <number_of_parameters>
<OpCode> %param_number
%endmacro
```

Parameter gibt man also enumeriert mit `%x` an (z.B. `%1` fuer den ersten
Parameter).

Beispiel:

```asm
%macro swap 2
push %1
mov %1, %2
pop %2
%endmacro
```

### Problem der doppelten Marken

Ein Problem das bei Makros auftreten kann ist das "der doppelten Marken"
(Labels). Deklariert man innerhalb eines Makros naemlich ein Label, wird dieses
bei jeder Verwendung des Makros neu deklariert. Ein Label darf aber nur einmal
deklariert werden.

Als Loesung kann man die Marke auch als Parameter uebergeben. Im Microsoft
Assembler kann man Marken auch als `LOCAL` deklarieren.

## Assemblierungsvorgang

Eine interessante Eigenschaft an Assemblierern ist, dass sie den Code immer
zweimal durchlaufen muessen. Der Hauptgrund ist, dass Labels und Symbole in
Assemblercode ueberall definiert sein koenen, unabhaenging von deren
Verwendung. So kann man zum Beispiel ein Label anspringen, welches erst spaeter
im Code deklariert ist. Gaebe es nur einen Lauf des Assemblierers, wuesste
dieser dann an der Stelle des Sprungs nicht, ob es das Label wirklich gibt.

Im ersten Lauf des Assembliererns sammelt der Assemblierer also zuerst Marken
(Symbole) in einer *Symboltabelle* und erweitert Makros. Die Symboltabelle
enthaelt dabei ein Mapping zwischen Symbolnamen (Labelnamen) und den
assoziierten Werten, also z.B. bei `equ` die Konstante oder bei normalen Marken
die Adresse im Programm. Weiters werden noch andere Informationen wie die Laenge
des Datenfelder oder die Zugriffsrechte fuer jedes Symbol gespeichert.

Erst im zweiten kompletten Lauf wird der Code dann mit den vollstaendigen
Informationen ueber Marken generiert. Bei der Generierung des Codes werden
mnemotechnische Befehlnamen durch die entsprechenden Hexadezimal OpCodes
ersetzt. Der Code selbst ist dabei aber noch nicht der endgueltige
Maschinencode, sondern Objekt-Code (`.o`).

### Fehlerbehandlung

Waehrend der Code Generierung wird nun aber noch auf Fehlererkennung und
Behandlung geachtet. Moegliche Fehler sind beispielsweise:

* OpCode nicht zulaessig.
* Ungueltige Operandenzahl fuer einen Befehl.
* Falsche Verwendung von Registern.

Tritt ein Fehler auf, wird eine Fehlermeldung ausgegeben und der
Kompilierprozess unterbrochen.

### Linker

Der Assemblierer generiert aus einer Assembler-Datei (z.B. `.asm/.S`) ein
Objektmodul mit der Dateiendung `.o`. Eine solche Objektdatei enthaelt zum
Grossteil Maschinen-Code, aber auch Symbolnamen bzw. Referenzen zu Symbolen, von
welchen manche moeglicherweise gar nicht in der gerade kompilierten Datei
definiert waren.

Wenn alle Assembler-Dateien des Programms zu Objektmodulen assembliert
(kompiliert) wurden, uebernimmt der sogenannte *Linker* das Verbinden (eben
*linken*) der Objekt-Dateien zu einem vollstaendigen ausfuehrbaren Programm
(executable).

Der Linker loest dabei Referenzen zu Symbolen zwischen den Objektmodulen
auf. Hat z.B. ein Befehl in der einen Assembler-Datei einen Sprung zu einem
Label gemacht, das in einer anderen (nun kompilierten) Assembler-Datei war, war
vorher in der Assembler-Datei mit dem Sprungbefehl nur eine Referenz zu diesem
Label. Der Linker kann diese Referenz nun durch die wirkliche Adresse hinter der
Marke austauschen. Diese Adresse erhaelt der Linker aus der Symboltabelle, die
fuer jedes Objektmodul generiert wurde.

Eine weitere Aufgabe des Linkers ist es, Anfangsspeicheradressen fuer jedes
Objektmodul zu bestimmen. Er erstellt dafuer eine Tabelle mit den Modulen und
deren Laengen. Dann addiert er die gewaehlte Anfangsadresse auf alle
Speicherreferenzen im Modul (z.B fuer `db`). Auch aktualisiert er die Adressen
von Prozeduraufrufen.

Waehrend des Link-Prozesses koenen noch Fehler bezuglich Symbolen auftreten:

* Fehler bezueglich nicht gefundenen bzw. nicht aufloesbaren Symbolen (undefined
  Symbol)
* Fehler bezueglich mehrmals definierter Symbole (duplicate symbol)

http://stackoverflow.com/questions/7718299/whats-an-object-file-in-c

#### Struktur eines Objektmoduls

Ein Objektmodul enthaelt die folgenden Informationen:

1. Identifikations-Informationen wie Name, Laenge, Startadresse und Teilbereiche
   des Moduls.

2. Einsprungpunktabelle (entry point table), in welcher exportierte Symbole
   gespeichert werden, um diese schnell zu lokalisieren (fuer andere Dateien).

3. Externe Referenztabelle (cross reference table): Eine Tabelle mit externen
   Referenzen und deren Benutzungen im Code, um sie schnell aufloesen zu
   koennen.

4. Maschinenbefehle: Der assemblierte Code mit Konstanten sowie noch
   unaufgeloesten Symbolreferenzen.

5. Relokations-Tabelle (relocation table): Enthaelt Informationen zu
   Speicheradressen, die im Maschinencode verwendet werden und noch durch die
   wirklichen Adressen, die im Laufe des Symbol-Resolutions-Verfahren des
   Linkers gefunden werden, ersetzt werden muessen. Auch die Startadressen der
   Module spielen eine Rolle.

6. Ende des Moduls.

https://en.wikipedia.org/wiki/Relocation_%28computing%29

## Gotchas

Extrem wichtige, gerne vergessene Dinge.

* Nur ein Speicherzugriff pro Befehl.
* Nur 32-Bit Register fuer Speicherzugriffe.
* Register sichern.
* RET nie vergessen.
* __Indizes scalen! 2-Byte Elemente: `* 2`!__
* Register loeschen bevor man ihre 16/8 Byte Anteile beschreibt!
* Speicherzugriffe typisieren! (`mov dword [0x0], 1`)
* Den Rest Teil bei Divisionen vorher loeschen!!!
* Zwischen Aufrufen von Prozeduren und Anspringen von Marken unterscheiden!
* `CMP` ist unnoetig nach einer arithmetischen Operation!
* Werte vom Speicher werden rueckwaerts ins Register gelegt!
* Shiften geht variabel nur mit CL.
* Am besten einfach alle Register die man nicht veraendern darf pushen/poppen.
* Keine Konstante fuer MUL (Nur Register oder Speicherzugriff)! Zuerst in ein Register. Oder IMUL!
* Bei `IMUL` gibt es keine hoeheren Bits, die Operation ist begrenzt durch die Groesse der Operandenregister (overflowed einfach).
* `PUSH <Konstante>` pushed 16-Bit Konstante (obwohl Konstanten in x86 32-Bit sind)! Also fuer 32-Bit: `PUSH dword <Konstante>`
* `CALL` und `JMP` koennen zu 32/16/8-Bit Registern springen.
