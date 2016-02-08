# Einfuehrung in die Rechnerarchitektur -- Fragen zur Vorlesung

## 1. Aus welchem Logikgatter koennen alle anderen logischen Funktionen
   zusammengesetzt werden?

NAND und NOR. Beweis: Da sich aus OR, AND und NOT alle logischen Formeln
herstellen lassen, gilt es nur zu zeigen, dass aus NAND/NOR auch AND/OR und NOT
herleitbar sind:

__NOT__:

$\neg A \equiv \neg (A \land A) \equiv \neg (A \lor A)$

$\text{ NOT } A \equiv A \text{ NAND } A \equiv A \text{ NOR } A$

bzw. geht auch mit 1/true, also $\neg A \equiv \neg (A \land true)$

__AND__:

$A \land B \equiv (A \land B) \lor (A \land B) \equiv \neg (\neg A \lor \neg B)
\lor \neg (\neg A \lor \neg B)$ $\equiv \neg((\neg A \lor \neg B) \land (\neg A
\lor \neg B)) \equiv \neg(\neg(A \land B) \land \neg(A \land B))$

$A \text{ AND } B \equiv (A \text{ NAND } B) \text{ NAND } (A \text{ NAND } B)$

VIEL EINFACHER: $A \land B \equiv \overline{\overline{(A \land B)}} \equiv
\overline{\overline{A} \lor \overline{B}}$

also: $A \text{ AND } B \equiv NOT NOT (A AND B) \equiv NOT (NOT A \text{ OR }
NOT B) \equiv NOT A NOR NOT B$

__OR__:

Selbes wie fuer AND/NAND aber nun fuer OR/NOR:

$A \text{ OR } B \equiv (A \text{ NOR } B) \text{ NOR } (A \text{ NOR } B)$

bzw. viel einfacher: $A \text{ OR } B \equiv NOT NOT (A \text{ OR } B) \equiv
NOT (NOT A \text{ AND } NOT B) \equiv NOT A NAND NOT B$

## 2. Aus wie vielen Bit bestehen Nibble, Byte, Word, Doubleword und Quadword
   nach Intel-Nomenklatur?

Nibble: 4 Bit Byte: 8 Bit Word: 16 Bit Doubleword: 32 Bit Quadword: 64 Bit

Page-Size oft: 4kB


## 3. Was versteht man unter einem unausgerichtetem Zugriff auf den
   Hauptspeicher (engl. misaligned memory access)? Welche Konsequenzen hat ein
   solcher bei x86-CPUs?

Wenn ein Datum von $N$ Bytes an einer Adresse gespeichert wird, die kein
ganzzaehliges Vielfaches von $N$ ist.

Bei dieser Definition wird angenommen, dass $N$ eine Zweierpotenz ist, da alle
primitiven Datenttypen Zweierpotenzen sind (`char` = 1 Byte, `int` = 4 bytes,
`long` = 8 bytes etc.).

Z.B. wenn man auf einer 16-Bit Maschine arbeitet (16-Bit Wortgroesse), und nun
ein Wort an Adresse `0x1` speichert, sodass es den Speicherraum `0x1 - 0x2`
aufnimmt. Will man dieses Datum nun lesen, so nennt man den Zugriff
"unausgerichtet" bzw. "misaligned" weil das Datum der Laenge $N = 2$ Bytes
nicht auf einem Vielfachen von $N = 2$ liegt ($1$ ist kein Vielfaches von
$2$).

Der Zugriff wird dabei langsamer, weil der Prozessor dann mehr Woerter lesen
muss, um das ganze Datum zu erhalten, und dann mehere komplizierte und teuere
Shiftoperationen durchfuehren muss, um die Daten wieder auszurichten:

```
| = Wortgrenze
Die Bytes des Datums eingeklammert.

[0x0 (0x1 | 0x2) 0x3 | 0x4 0x5 | ...]
```

Der Zugriff erfordert nun lesen des Wortes `[0x0 0x1]` __sowie auch__ des Wortes
`[0x2 0x3]`. Dann muss das untere Wort erst um einen Byte nach links geshiftet
werden, das obere Wort um einen Byte nach rechts und erst dann koennen die
beiden Woerter in ein Register kombiniert werden. Weil dies eine komplizierte
und teure Operation ist, unterstuetzen es nicht alle Prozessoren.

"A memory address $a$ is said to be $n$-byte aligned, when $a$ is a multiple of
$n$-bytes (__where $n$ is a power of two__). An $n$-byte aligned address would
have $\log_2(n)$ least-significant zeros when expressed in binary."

Das letzte ist interessant. Ein $1$-byte ausgerichtetes Wort kann an jede
Addresse gelegt werden, also sind keine Bits reserviert. Ein $2$-byte
ausgerichtetes Wort kann aber z.B. nur an geraden Addressen liegen, also ist der
erste Bit immer null. Bei $4$-byte ausgerichteten sind die letzten zwei Bits
immer null, da die Adresse nie bei $4n + 1, 4n + 2, 4n + 3$ liegen kann, sondern
immer nur bei $4n + 0$. Also, bei einem $n$-byte ausgerichteten Datum sind die
letzten $\log_2(n)$ Bits null.

Ein weiterer Unterschied liegt in der "Atomicity" des Zugriffs. Wir nehmen an,
es ist garantiert dass ein Zugriff auf ein Wort atomar ist, also nur ein Prozess
zu einem bestimmten Zeitpunkt dieses Wort lesen oder schreiben kann. Wenn die
Wortgroesse des Prozessors mindestens so gross ist wie der groesste primitive
Datentypen (64-bit Mac = 64 Bit long/double), dann ist ein Zugriff auf einen
Wert also immer atomar. Bei einem misaligned Access kann das eben zunichte
werden. Z.B. liest Prozess $A$ das untere Wort des Datums, das unausgerichtet
auf zwei Woertern liegt. Danach schreibt ein anderer Prozess $B$ auf die beiden
Woerter um den Wert zu aendern. Danach liest der Prozess $A$ das obere Wort. Nun
ist der von Prozess $A$ gelesene Wert weder der alte noch der neue.

__https://en.wikipedia.org/wiki/Data_structure_alignment__

http://www.alexonlinux.com/aligned-vs-unaligned-memory-access

http://stackoverflow.com/questions/381244/purpose-of-memory-alignment

## 4. Wie wird bei der Intel IA-32 Architektur der 32-Bit Wert `0xDEADBEEF` im
   Speicher abgelegt? Wie nennt man dieses Format?

Auslegung: `EF BE AD DE` Format: Little-Endian

Note das Ende ist hier links (wie bei Zahlen). `DE AD BE EF` waere Big-Endian,
weil der MSB (most-significant-byte) hier am Ende (also zahlenmaessig links)
steht. Die Auswahl ist bei Herstellern oft arbitraer. Little-endian macht
addition leichter, weil man das carry in die hoeheren Speicheradressen mitnehmen
kann.

Intel benutzt Little-Endian.  IBM benutzt Big-Endian.

## 5. Gegeben ist folgendes Stueck C-Code:

```c

struct { uint16_t x; uint32_t y[10]; } *eax;

int ebx = foo();

eax->y[ebx] = 5; 
```

Wie kann ein Compiler durch Verwendung von Displacement und Skalierung bei der
Adressierung den Zugriff effizient uebersetzen?

```asm MOV [EAX + 2 + EBX * 4], 5 ```

## 6. Wodurch unterscheidet sich die fuer RISC typische Load/Store-Architektur
   von einer CISC-Architektur wie x86?

Bei Load/Store Architekturen koennen nur die Operationen `LDR` (load register)
und `STR` (set register) auf den Hauptspeicher zugreifen, alle anderen
Operationen wie `ADD`, `SUB` oder `MOV` muessen auf Registern ausgefuehrt
werden. Bei CISC-Architekturen koennen auch andere Operationen (wie z.B. `ADD`)
auf den Hauptspeicher zugreifen.

## 7. Nennen Sie 4 arithmetische Befehle fuer Gleitkommazahlen in der FPU.

4 aus FADD, FSUB, FNEG, FABS, FMUL, FDIV, FSQRT

FPU = Floating-Point Unit

## 8. Nennen Sie die 2 Ausfuehrungsmodi, die zur Unterstuetzung von
   Mehrbenutzersystemen dienen.

Antwort: Systemmodus (engl. kernel mode), Benutzermodus (engl. user mode)

## 9. Grenzen Sie die Begriffe Assemblerprogramm und Maschinenprogramm
   voneinander ab.

Assembly ist eine Programmiersprache, die zwar hardware-nahe ist aber die der
Prozessor selbst noch nicht versteht. Sie verwendet Mnemonische Codes
(z.B. `MOV`) anstelle von den numerischen Codes, die letztendlich im
Maschinenprogramm landen. Die Assembly-Instruktionen muessen zuerst in
Maschinencode uebersetzt werden, ein Format, dass der Prozessor dan definitiv
versteht. Dieser Uebersetztungsprozess nennt sich *Assemblierung*. Ein
Assemblerprogramm ist also eine (relativ) duenne Schicht ueber dem
resultierenden Maschinenprogramm.

Assemblerprogramm:

```asm MOV EAX, 5 ```

Maschinenproramm (bei 32-Bit Instruktionsformat):

``` 01 = MOV 1 = EAX 0 = Dummy

01 10 00 05 ```

https://en.wikipedia.org/wiki/Machine_code

## 10. Nennen Sie 2 Vorteile fuer den Einsatz von Assembler im Gegensatz zu
   einer hoeheren Programmiersprache.

* Bessere Performance durch Ausnutzung Hardware-spezifischer Vorteile (der
  spezifischen Architektur).
* Geringere Groesse des generierten Maschinencodes.
* Direkter Zugriff auf Hardware (Register).

## 11. Nennen Sie 2 Nachteile fuer den Einsatz von Assembler im Gegensatz zu
   einer hoeheren Programmiersprache.

* Sehr viel mehr Code (hoehere Aufwand) um schon einfache Konstrukte zu bauen
  (e.g. Schleifen).
* Weniger portabel (als z.B. ein C Programm).

## 12. Was ist der Unterschied zwischen einer statischen und einer dynamischen
   Bibliothek?

Eine statische Bibliothek (.a auf UNIX, .lib auf Windows) ist eine Bibliothek,
die schon zu Compile-Time in die resultierende, ausfuehrbare Datei eines
Programms geladen wird. Es besteht also keine Abhaenigkeit von der Bibliothek
zur Laufzeit, weil jeglicher verwendeter Code schon zur Kompilierungszeit in die
Datei eingeschrieben wurde.

Eine dynamische Bibliothek (.so auf Linux, .dll auf Windows, .dylib auf OS X)
ist eine Bibliothek, zu welcher im Programm nur Referenzen gemacht werden (zu
den *Deklarationen* von Symbolen), dessen *Definitionen* aber nicht in der
ausfuehrbaren Datei landen. Erst zur Laufzeit wird die dynamische Bibliothek in
den Speicher gelanden und ein Runtime-Linker loest die Referenzen (zur Laufzeit)
auf. Deswegen muss man also die Header inkludieren (damit man Referenzen zu den
Deklarationen machen kann), waehren die in der .dylib Datei enthaltenen
Definitionen erst zur Laufzeit eingebunden werden. So koennen auch mehrere
Dateien gleichzeitig die Bibliothek verwenden, ohne jeweils den Code in der
Datei einmeisseln zu muessen. Auch kann man die dynamische Bibliothek leicht
durch eine andere mit den selben Symbolen ersetzten, ohne re-kompilieren zu
muessen. Jedoch leidet die Laufzeit ein wenig unter dem
Runtime-Binding-Overhead.

Statischen Bibliotheken:
* Keine Laufzeit Dependencies
* Hoehere Groesse der Binaries (ausfuehrbare Dateien)
* Code-Duplikation in mehreren Dateien
* Schneller als Dynamische Bibliotheken

Dynamische Bibliotheken:
* Leichter zu ersetzen.
* Laufzeit leidet ein wenig.
* Weniger Code-Duplikation.
* Geringere Binary-Groesse.

## 13. Welche Moeglichkeiten der Operanden-Adressierung gibt es bei einem Intel
   386?

1. Operand in der Instruktion (Immediate)
2. Operand in einem Register
3. Operand im Hauptspeicher
4. Operand kommt von einem I/O Port (vom E/A-Werk)

## 14. Was unterscheidet den Maschinenbefehlszyklus und die Ausfuehrung des
   eigentlichen Benutzerprogramms?

Der Maschinenbefehlzyklus (Befehl laden, dekodieren, ausfuehren) ist unabhaegig
vom eigentlichen Benutzerprogramm und ermoeglicht dieses eigentlich erst. Das
Benutzerprogramm steht im Hautpspeicher und wird vom Maschinenbefehlszyklus
abgearbeitet.

## 15. Geben sie das typische Befehlsformat an wie es in den gaengigen
   Assemblern vorkommt und erklaeren sie es kurz.

Marke (Label): Ansprungadresse Operation: Mnemotechnsiche Vereinbarung fuer
einen Opcode Operanden: Auf welchen die Operation operiert Kommentar: Zuer
Erlaeuterung

## 16. Beim Assemblieren gibt es zwei Laeufe um den Maschinencode aus dem
   Assemblerprogramm zu erstellen. Erklaeren sie kurz warum ein Lauf nicht
   ausreicht.

Weil Labels im ganzen Programm definiert werden koennen und von ueberall aus
angesprungen werden koennen. Wuerde man z.B. ein `ende:` Label am Ende des
Programms definieren, so koennte man es von einem Befehl der vorher im
Assemblerprogramm definiert wird nicht anspringen, wenn man nur einen Lauf hat,
weil der Assembler dieses Label zu dem Zeitpunkt noch nicht gesehen
haette. Deswegen muss der Assembler zuerst in einem Lauf alle Labels sammeln
(auch um Duplikate zu vermeiden) und im zweiten Lauf kann er alle
Spruenge/Referenzen zu Labels dann auch ohne Probleme aufloesen.

## 17. Was sind Unterschiede zwischen einem Compiler und einem Interpreter?

Ein Compiler nimmt ein ganzes Programm als Input und uebersetzt es direkt, bevor
der Ausfuehrung des Programms in Maschinencode. Ein Interpreter verarbeitet erst
zur Laufzeit Zeile fuer Zeile das Programm und fuehrt jede Zeile ohne
Mittelsprache direkt aus (wird direkt in eine Instruktion fuer den Prozessor
umgewandelt).

Compiler:
* Takes Entire program as input.
* It takes large amount of time to analyze the source code but the overall
  execution time is comparatively faster.
* Generates intermediated object/machine code (synonyms).
* Programs require more memory because the entire object code has to reside in
  memory ("Generates intermediate object code which further requires linking,
  hence requires more memory")
* Program need not be compiled every time.
* It generates the error message only after scanning the whole program. Hence
  debugging is comparatively hard.
* Programming language like C, C++ use compilers.

Interpreter:
* Takes Single instruction as input.
* It takes less amount of time to analyze the source code but the overall
  execution time is slower.
* No intermediate object code is generated, hence are memory efficient.
* Programs have to be interpreted line-by-line every time they are run.
* Continues translating the program until the first error is met, in which case
  it stops. Hence debugging is easy.
* Programming language like Python, Ruby use interpreters.

http://www.programiz.com/article/difference-compiler-interpreter
http://www.c4learn.com/c-programming/compiler-vs-interpreter/

Just-In-Time (JIT) Compiler: compiles code at runtime, i.e. a hybrid of a
compiler and an interpreter.

http://stackoverflow.com/questions/95635/what-does-a-just-in-time-jit-compiler-do

## 18. Nennen Sie die Phasen eines Compilers

Frontend (Analyse):
* Lexikalische Analyse
* Syntaktische Analyse
* Semantische Analyse


Backend (Synthese):
* Zwischencode-Generierung (remember Chandler's LLVM IR (intermediate
  representation))
* Maschinenunabhaengige Optimierung
* Codegenerierung

## 19. Was steht in einem Aktivierungsblock auf dem Stack (engl. activation
   record, stack frame)?

* Aufrufparameter
* Lokale Variablen
* Zeiger zum aufrufenden Aktivierungsblock
* Ruecksprungadresse (nach der Aufrufzeile)
* Gesicherte Register

## 20. Nennen Sie die 6 funktionalen Schichten eines Rechners.

1. Benutzerprogramm-Schicht

In höherer Programmiersprache definiert: Variablen, verschiedenen, eventuell
selbstdefinierten Typs. Oftmals weitere Schichten (Betriebssystemsschicht,
Awenderschicht etc.)

2. Von-Neumann-Schicht

Definiert durch Maschinenbefehlssatz und die Objekte, auf denen dieser arbeitet:
Wörter, Adressen, Speicherzellen, Register. Durch Mikroprogrammierung
interagiert die Von-Neumann-Schicht mit der Mikroarchtektur-Schicht.


3. Mikroarchitektur-Schicht

Auch: Register-Transfer-Level (RTL), definiert durch Quell- und Zielregister und
Operationen, die auf diesen Registern operieren (sie transportieren oder
verknuepfen).

Definiert durch den Mikroinstruktionssatz und die Objekte, auf denen dieser
arbeitet: Register, Speicherzellen, Verbindungswege, verknuepfende Elemente wie
Schaltnetze, Schaltwerke.

4. Gatter-Schicht (AND/NOR Gatter)

Definiert durch elementare Boolesche Funktionen, die unter Benutzung elementarer
Schalter durch Bausteine realisiert werden.

5. Bauelemente-Schicht

Definiert durch idealisierte Bauelemente, z.B Widerstände, Kondensatoren,
Transistoren, Leitungen etc.

6. Physikalische-Schicht (Silicium, Metal-Oxid etc.)

## 21. Wie viel Bit haben die Register des 8085 Mikroprozessor?

8 Bit. Der ganze Chip ist ein 8-Bit Rechner.

## 22. Welche Groesse in Byte haben die Maschinenbefehle des 8085-Prozessors und
   wovon ist die Groesse abhaengig?

1, 2 oder 3 Byte je nach Groesse des Adressteils.

``` [opcode | addressteil | addressteil] 1 2 3 ```

## 23. Aus welchen Phasen besteht der Maschinenbefehlszyklus des
   8085-Prozessors?

1. Befehl adressieren
2. Lesezyklus im Speicher
3. Befehl einlesen
4. Befehl dekodieren und ausführen

## 24. Wie ist die Hauptspeicheranbindung bei Rechnern auf Basis des 8085
   realisiert?

16 Bit Adresse: 8 Bit unidirektionaler Adressbus + 8 Bit bidirektionaler Bus fu
er Daten und Adressen (Multiplex) (Seite 34)

Fuer das I/O-Werk sind es 8 Bit Adresses, welche mit der Zufuegung des
Datenbusses aber auch auf 16-Bit erhoeht werden koennen.

## 25. Wie viele Maschinen und Taktzyklen benoetigt der 8085 fuer
   unterschiedliche Befehle?

Wegen unterschiedlicher Befehlsformate und Zeitbedingungen besteht der
Maschinenbefehlszyklus aus einer variablen Anzahl von Maschinenzyklen:

* 1 bis 5 Maschinenzyklen mit je 3 bis 6 Taktzyklen

Maschinenbefehlszykluszeit: Ausfuehrungszeit fuer einen Maschinenbefehl
Taktzyklus: Reziprokwert der Taktfrequenz (e.g. $\frac{1}{5 \text{MHz}}$)

Note: Maschinenbefehlszyklus != Maschinenzyklus, ein Maschinenbefehlszyklus
(Ausfuehrung eines Maschinenbefehls) besteht aus mehreren Maschinenzyklen (im
Mikroprogramm). Ein Maschinenzyklus beinhaltet z.B. einen Lesezyklus oder enen
Schreibzyklus zum Hauptspeicher oder E/A-Werk.

## 26. Was macht der Befehl OUT im 8085?

Uebertraegt den Inhalt des Akkumulators zum adressierten Ausgabeport.

``` OUT 6 ; Sende Akkumulator an Port 6 des Ausgabewerkes ```

Dauer: 3 Maschinenzyklen und 10 Taktzyklen

## 27. Wann werden Unterbrechungswuensche (Interrupt-Anforderungen) im 8085
   abgefragt?

Im letzten Maschinenzyklus eines Befehls werden Unterbrechungswünsche
abgefragt. (Seite 42)

## 28. Welche Adressierungsarten gibt es im Instruktionssatz des 8085?

* Immediate: Operand als 2. oder 3. Byte der Instruktion (`5`)
* Implizit: Register implizit durch Operation bestimmt (z.B. ECX fuer `loop`)
* Registerdirekt: Operand steht im Register (2./3. Byte der Instruktion) (`EAX`)
* Registerindirekt: Operand steht in einer Speicheradresse, welche im Register
  des 2./3. Bytes der Insturktion steht (`[EAX]`)
* Direkt: Operand ist eine Speicheradresse (`[0x0]`)

## 29. Was sind wesentliche Eigenschaften/Neuerungen des 8086 bzw. 8088?

16-Bit Verarbeitung (Wortgroesse)

## 30. In welche zwei Teile la ̈sst sich eine Mikroinstruktion grob
   untergliedern?

1. Steuerteil
2. Adressteil

## 31. Erlaeutern Sie den Unterschied zwischen horizontalen und vertikalen
   Mikroinstruktionsformaten.

1. Horizontal: unkodiert, also jeder Bit der Mikroinstruktion ist ein
   Steuersignal. Dadurch sind jedoch auch unmoegliche oder redundante Zustaende
   (Kombinationen) moeglich.
2. Vertikal: kodiert/verschluesselt, jede Kombination von Bits steht fuer einen
   Zustand (Kombination von Steuersignalen). Ungewuenschte Kombinationen von
   Steuersignalen werden einfach nicht kodiert (es gibt keinen Code fuer
   sie). Bei diesem Format braucht man jedoch noch zusaetzlich einen Dekoder.
3. Quasi-Horizontal: Hybrid aus (1) und (2)

## 32. Nennen Sie 5 Adressfortschaltbefehle des Am2910.

Alle:

1. Bedingte Spruenge:
* CJP: conditional jump (bedingter Sprung)
* PS: pass (unbedingter Sprung)
* CONT: continue (__Mikro__-befehlszaehler einfach erhoehen)
* JMAP: jump to mapping-prom, bilde die Instruktion auf eine
  Mikroprogrammspeicheradresse ab.
* JZ: jump to adress zero, unbedingter Sprung zu Null
* JRP: Two-way jump, also wenn condition, dann dorthin, wenn nicht, dann dahin,
  nie einfach weiter im Programm
* CJV: Bedingter Sprung auf Vektor
* CJPP: Conditional Jump to Adresse auf Stack (mit eventuellem POP wenn die
  Condition gilt und gesprungen wird, sonst nicht)

2. Unterprogrammaufrufe:
* CJS: Conditional Jump Subroutine (CALL in assembly), mit RET am ende
* JSRP: Wie JRP (bedingter Zwei-Wege-Call)
* CRTN: Bedingter Ruecksprung: Wenn condition, dann zurueck zu
  Ruecksprungadresse, wenn nicht, dann weiter im Unterprogramm.

3. Schleifen:
* LDCT: Load Counter And Continue
* RPCT: Repeat Pipeline Counter, wenn Counter 0, weiter im Programm, sonst
  zurueck Adresse (Direktdatum)
* PUSH: Schreibe Datum auf Stack, inkrementiere dann Stackpointer
* RFCT: Wenn Counter 0, weiter im Programm, sonst Sprung zu Adresse auf dem
  Stack. Wenn Counter 0, auch POP (weg mit Sprungadresse).
* LOOP: Wie RFCT (Adresse auf Stack), aber beliebiege Bedingung, nicht nur
  Counter = 0.

## 33. Aus welchem Baustein außer dem Am2901 (ALU) und dem Am2904
   (Wortrandlogik) besteht die ALU der mikroprogrammierbaren Maschine? Welche
   Signale verarbeitet dieser Baustein?

Carry-Lookahead Am2902. Er verarbeitet die Carry-Generate- und Carry-
Propagate-Signale der ALUs sowie ein Carry-In-Signal und generiert daraus die
Carry- Einga ̈nge der ALUs.

## 34. Welches elektronische Bauelement eignet sich zur Speicherung eines Bits?

* Flip-Flop (SRAM -- static ram, schnell, teuer)
* Kondensator (DRAM -- dynamic ran, langsamer, billiger)

## 35. Welche Technologie wird heute vorrangig zur Herstellung von Transistoren
   in integrierten Schaltkreisen verwendet?

MOS (metal oxide semiconductor) bzw. NMOS (negative MOS), PMOS (positive MOS)
oder CMOS (complementary MOS, PMOS + NMOS)

## 36. Wie erfolgen Zuweisungen mehrerer Signale innerhalb eines Prozesses in
   VHDL?

Sie erfolgen gleichzeitig (__concurrent__ statements).

## 37. Welche Elemente sind in einer vollstaendigen Beschreibung einer
   VHDL-Komponente not- wendig?

Entity (Beschreibung des Bauteils mit Input/Output Ports) und Architecture
(Beschreibung der Signale die in das / aus dem Bauteil gehen).

## 38. Wozu dient eine Entity in VHDL?

Zur Beschreibung der Ein-/Ausgaenge (Schnittstellen) einer Komponente

## 39. Was passiert in folgendem VHDL-Code mit den Signalen a und b? Welche
   Werte haben a und b nachdem der Prozess einmal durchlaufen wurde, wenn diese
   zuvor mit a := 0 und b := 1 belegt waren?

```vhdl process(clk) begin if rising_edge(clk) then a <= b; b <= a; end if; end
process; ```

`a` wird 1 haben und `b` wird 0 haben. Die Signale werden gleichzeitig
(concurrently) ausgefuehrt, also nicht wie z.B. in C wo `a` nach der ersten
Instruktion den Wert von `b` haette und `b <= a` dann wie `b <= b` ware.

Das ganze passiert bei einem rising-edge des Signals `clk`.

## 40. Was ist die Aufgabe des Mapping-PROM?

Es bildet eine Maschineninstruktion aus dem Maschineinstruktionregister auf die
erste Zeile (Speicheradresse) des passenden Mikroprogramms im
Mikroprogrammspeicher ab.

## 41. Welches Signal liegt am F-Ausgang der ALU des Rechenwerks an?

Das Ergebnis der letzten Rechenoperation.

## 42. Wie viele Register besitzt der Am2901? Was ist bei ihrer Adressierung zu
   beachten?

Der Am2901 besitzt insgesamt 16 16-Bit Register, wobei der Maschinenprogrammier
aber nur die unteren 8 (0 - 7) adressieren kann, weil der MSB des Registers in
der Maschineninstruktion immer auf 0 gestetzt ist. Das heisst, der
Maschinenprogrammierer kann die oberen 8 Register (8 - 15) nicht
adressieren. Diese koennen vom Mikroprogrammier zur Implementierung der
Maschinenbefehle genutzt werden.

## 43. Ist es moeglich mit der MI-Maschine den Assemblerbefehl `ADD r1,r1` als
   Mikroprogramm zu implementieren? Welche Eigenschaft der Registereinheit des
   Am2901 wird hierbei genutzt?

Ja es ist moeglich. Die Eigenschaft ist, dass man bei der Registereinheit zwei
mal das selbe Register adressieren kann, welches separat aus den beiden
Datenausgaengen $A$ und $B$ ausgegeben wird.

## 44. Welche Rolle spielt das Am2902(Carry Lookahead)

Die Volladdierer sind in der Reihe geschaltet und deshalb braucht der
n-te Addierer den Carry Bit von dem vorherigen und das Am2902 versucht
noch am Anfang der Addition zu erraten ob die Carry bits gesetzt sind
oder nicht. 

Noch dazu: er verarbeitet Carry-Generate- und Carry-Propagate-Signale
der ALUs sowie ein Carry-In-Signal und generiert daraus die Carry
Eingaenge der ALUs.

## 45. Wieso laesst die Quellensteuerung des Am2901 die Kombination DB
nicht zu?

Da wir nur drei Bits zur Auswahl der Quellen zur Verfuegung haben,
gibt es nur 8 moegliche Kombinationen und sie sind alle schon
ausgenutzt. Und es lohnt sich nicht noch ein Bit einzufuehren.

## 46. Kann der Am2901 die Funktion gdw. nativ berechnen?

Ja, mit XNOR. (Alle anderen Funktionen werden wir auf den
Merkblaettern sehen koennen, so chill)


## 47. Welche Statusregister besitzt Am2904 (Wortrandlogik)?

Er besitzt zwei: Mikrostatus- und Maschinenstatusregister. Der erste
ist fuer den Mikroprogrammierer gedacht, um komplexere Mikroprogramme
zu schreiben und der andere wird quasi dem Assemblerprogrammierer exportiert.


## 48. Welche Bits des Mikrinstruktionsformats sind fuer die Absadresse?

BAR: MI_6..17 (koennte man aus dem Merkblatt ablesen)

## 49. Zeichnen sie einen 2:1 Multiplexer. (AND, OR, NOT)

You're the master in drawing with the help of the keyboard, I leave it
to you. Fck this shit!

## 50. Was versteht man unter einem Hazard in einer Schaltung?

Ein kurzzeitig falscher Wert einer boolschen Funktion, der bei der
Aenderung der Eingaegen dadurch entsteht, dass der Strom sich mit einer
endlichen Geschwindigkeit ausbreitet und deshalb koennen durch
unterschiedliche Leitungslaengen unerwartete Verzoegerungen entstehen.

## 51. Unterschied zwischen einem Schaltnetz und Schaltwerk?

Ein Schaltnetz ist ein Buendel von Funktionen und die Ausgabe haengt
nur von den Eingaengen ab. Und Schaltwerke haben auf der anderen Hand
einen inneren Zustand und die Ausgabe haengt auch von ihm ab.

+ *Schaltnetz* - eine Funktionseinheit, ein nach Aufgabe oder Wirkung
  abgrenzbares Gebilde, die eine oder mehrere Schaltfunktionen
  realisiert.
  
+ *Schaltwerk* - eine Funktionseinheit mit $m$ Eingaengen und $r$
  Ausgaengen, wobei es zu mindestens einem Eingangswert mehr als einen
  zugehoerigen Ausgangswert gibt.

## 52. Wie ist es moeglich fuer komm. Prozesse funkt. zu sein?

Wenn die kommunizierte Werte nicht in das Ergebnis kommen oder der
lokale Wert mit demselben Wert ueberschrieben wird.

## 53. Einschrittige Zustandsueberagaenge?

Der Hammingabstand zwischen den Zustanden 1 ist, also sie die
Zustandsnummern unterscheiden sich in nur einem Bit. Wenn die so
kodiert werden, wie es oben beschrieben wurde, dann koennen die
Hazards vermieden werden. (bei 10 -> 01 koennte ein Hazard entstehen,
z.B. 10 -> 00 -> 01)

## 54. Raeumliche Lokalitaet?

Wenn gemeinsam genutzte Daten im Speicher raeumlich beieinander
liegen, sind Optimierungen im Cache und beim Transfer der Daten ueber
den Bus zum Prozessor moeglich.

+ *Raeumliche Lokalitaet*
  * Die Daten, die neben einander in dem
	Speicher liegen, werden oft gemeinsam genutzt.
  * Wenn ein Programm auf die Adresse x zugreift ist es warscheinlich,
    dass es auch auf die Adressen in der naehe von x zugreift.
	
+ *Zeitliche Lokalitaet*
  * Wenn ein Programm auf die Adresse x zugreift, ist es
    warscheinlich, dass es bald wieder darauf zugreifen wird.
	
## 55. Unterschiede der drei Cache-Organisationsformen?

+ *Direkt* - jede Adresse im HS wird auf eine bestimmte Zeile im Cache
  abgebildet, deshalb kann oft nicht der ganze Cachespeicher
  ausgenutzt werden.
  
+ *Vollassoziativ* - keine Zuordnung der Adresse einer Zeile im Cache,
  schwerer zu implementieren, der ganze Cache genutzt.
  
+ *Mengenassoziativ* - Zuordnung der Adresse in eine Menge, innerhalb
  der Menge vollassoziativ.
  
## 56. Was ist der Index in der Adresse eines Datenzugriffs?

Die Zeile im Cache.

* *<Tag><Index><Offset>*

## 57. Wie wird die Verwendung des virtuellen Speichers in Hardware
unterstuetzt?

Durch die Verwendung eines speziellen Cachespeichers TLB (Translation
Lookahead Buffer), in dem haefig verwendete paare aus virtueller und
physischer Adresser gespeichert werden.

Zur Wiederholung: 
* MMU Register speichert die Adresse die page directory
* Page Directory speichert die Verweise auf die Page Table, also die
  Umsetzungstabellen

MMU Register wird immer neugeladen, beim Wechsel des Programms.

In der Tabellen wird nur die Anfangsadresse zugewiesen und der Offset
bleibt gleich,

Es koennen fuer solche Umsetzung Segmente oder Pages verwendet
werden. Segmente haben keine feste groesse (z.B. Code-, Daten-,
Kellersegment). Und bei Paging bleibt die groesse konstant.

MMU prueft auch die Zugriffsrechte.

## 58. Beispiele, wo Caches eingesetzt werden?

L1, L2, L3, TLB, I/O Geraeten

## 59. Kann jede Schaltfunktion durch Boolsche-Normalformen
augedrueckt werden?

Ja. //Ich weiss es nicht, was man hier noch sonst hinschreiben kann.

Jede n-Stellige boolsche Funktion ist eindeutig in DNF und KNF
darstellbar.

*Schaltfunktion* - ein Funktion der Form {0,1}^n -> {0,1}

Noch etwas zum Thema boolsche Algebra:
Die Einsmenge einer n-Stelligen Funktion ist die Menge aller
n-Tupel(Eingangwoerter), fuer die die Schaltfunktion den Wert 1
ausgibt.

## 60. In welcher Normalform wird ein Minterm verwendet?

Minterme werden in DNF verwendet(z.B. $abc+\overline{a,b},c$)

Ein *Minterm* ist die Konjunktion aller n Argumente einer
Schaltfunktion. Die Argumente koennen negiert werden und jeder Minterm
liefert nur genau fuer eine Belegung den Wahrheitswert 1.

Ein *Maxterm* ist genau das umgekehrte.

## 61. Liefert eine Schaltfunktion bei gleichen Eingaben immer die
gleichen Ausgaben?

Ja, die Ausgaben haengen nur von der Eingabe ab; eine Schlatfunktion
hat keinen internen Zustand, der die Ausgabe irgendwie beeinfluessen
koennte.

## 62. Laesst sich mit einer Schlatfunktion ein Speicher realisieren?

Nein, keinen internen Zustand. Einen ROM kann man doch mit einem
Encoder und Decoder realisieren.

## 63. Was ist eine Verschmelzungsmenge?

Ist die Menge aller n-stelligen Minterme, die sich durch
Distributivitaets- und Neutralitaetsregeln zu einem k-Stelligen Monom
umformen laesst. (abc+not(a)bc=bc)

*Monom* - (konjunktives) ist die Konjunktion k-Variablen einer
n-stelligen Schaltfunktion.

## 64. Wie funktioniert ein KV-Diagramm fuer 4 vars?

Drawing.. not my thing. Fck this shit.

Hierbei werden 2^n Felder fuer n Variablen verwendet, um jeden
moeglichen Minterm darzustellen.

## 65. Welche Ziele werden bei der Minimierung von Schaltnetzen
verfolgt?

* Weniger Schaltglieder
* Weniger Eingaenge und Leitungen
* Weniger Stufen
* Kuerzere Leitungen
* Billigere Schaltglieder

## 66. Welches Schaltnetz hat genau einen Ausgang(MUX/DEMUX)?

MUX //nope I'm not gonna draw it

## 67. Wie kann man ein ROM bauen?

n -> Decoder -> 1 aus 2^n -> Encoder -> m

## 68. Wodurch blahblah Gatter sparen bein nem Funkbuendel?

Gemeinsame Nutzung von Mintermen, die von mehreren Funktion ueberdeckt
werden.

## 69. Was eignet sich besser fuer die Umformung in NOR-Gattern?
(konjunktives oder disjunktives Polynom)

Konjunktives Polynom - ist doch trivial

## 70. Mealy- und Moore-Automaten

Beim Moore die Ausgabe nur vom inneren Zustand, beim Mealy noch von
der Eingaengen. Bei der Zusammenschaltung von mehreren Mealy Automaten
kann eine endlosschleife passieren.

## 71. Prozess

Zeitlich zergliederbare Handlung

## 72. Warum Synchrone Schaltwerke?

Um die Funktionalitaet des Computers sicherzustellen, Hazards zu
vermeiden, verschiedene Elemente miteinander zu synchronisieren.

## 73. Warum ein Hazard in nem And-Gatter kein Gegenbeispiel fuer die
Funktionalitaet?

Weil nach kuerzer Zeit wieder der korrekte Wert einstellt.

## 74. Welche Zahlen benachbart?

trivial

## 75. Welchen Vorteil bringen sie?

Vermeidung von "critical races"

*Critical Race* - Abhaengigkeit des Folgezustands eines Rechenweks von
Zeitbedingungen im Rueckkopplungsweg und das ist endgueltig.

## 76. Was wird durch die Taktung erreicht?

Hazards koennen abklingen bevor die Daten weiterverarbeitet werden und
Synchrnonization des Schaltwerks mit anderen Teilen der Maschine.

## 77. Welche Aussagen sind korrekt ueber D-FlipFlops?

kein Lust

## 78. Warum wird der Pegel, der am Eingang eines MS-Flipflops liegt
##nicht sofort in den Ausgang uebernommen?

Weil eines der beiden FlipFlops immer blockiert. (einer wird von dem
low-Pegel aktiviert und der andere bei dem umgekehrten)

## 79. Was bringt das?

Die Synchronisation kann auf den Takt erreicht werden, also
Flankengesteuert.

## 80. Zeitliche Lokalitaet

## 81. Prefetching

Die Daten werden in den Cache geladen noch bevor sie benoetigt werden.

Hardware und Software Prefetching

Bei Hardware prefetching wird meisten der naechste auszufuehrende
Befehl in den Stream Buffer geladen und beim Software Prefetching
setzt der Compiler explizit Befehle rein, um die entsprechenden Daten
im Voraus zu fetchen und das kann auch von dem Programmierer
kontrolliert werden.

## 82. Geschiechte der Computern in 2 seconds?

Prozessoren sind deutlich schneller geworden und die
Speichergeschwindigkeit hinkt deutlich hinterher.

## 83. Transparenz von Caches?

Caches werden automatisch von der Hardware gesteuert.

## 84. Vorteile vom virtuellen Speicher in Kombination mit Paging?

Transparente Auslagerung von unbenutzten Speicherseiten auf HSpeicher,
Speicherschutz, verminderte Fragmentierung, beim Linkervorgang stehen
virtuelle Adressen von Symbolen bereits fest.

## 85. TLB? Wieso oft vollassoziativ?

Wird benutzt, um die Uebersetzungstabellen von virtuellen Adressen auf
die Physische zu speichern und wird bei jedem Speicherzugriff
benoetigt, also ist sehr wichtig, dass er sehr effizient implementiert
wird und voll ausgenutzt wird, deshalb ist die vollasoziative
Implementierungen genau das richtige fuers TLB.

## 86. Warum setzt man Controller mit IO ein?

Zur Entlastung der CPU und Realisierung zeitkritischer Aufgaben.

## 87. Cache misses
* Compulsory miss
* Capacty miss
* Conflict miss

Die Erklaerungen zu allen drei sind trivial.

## 88. Welche Eigenschaften stehen bei Speicherhierarchie im
##Gegensatz?

Kapazitaet und Geschwindigeit.

## 89. Verfahren fuer den HSpeicher?

* Magnetisches - Festplatte
* Optisches - CD-ROM, CD-RW, DVD
* Elektrisches - Flash/SSD


