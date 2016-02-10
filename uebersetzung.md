# Uebersetzung

Compiler sind ein Schritt im Uebersetzungsprozess von einer Sprache in eine
andere. Der gesamte Prozess umfasst:

1. Ein Quellprogramm.
2. Wird durch den Preprocessor modifiziert (z.B. Makro-Erweiterung).
3. Den Compiler, der entweder sogleich Objektmodule oder zuvor Assembler
   generiert.
4. Falls der Compiler Assembler generiert, einen Compiler fuer den Assembler.
5. Einen Linker/Loader, der die Objektmodule sowie statische oder gemeinsame
   Bibliotheken bindet.
6. Ein fertiges Maschinenprogramm, dass der Prozessor ausfuehren kann.

## Arten von Uebersetzern

Ein Compiler nimmt ein ganzes Programm als Input und uebersetzt es direkt, bevor
der Ausfuehrung des Programms in Maschinencode. Ein Interpreter verarbeitet erst
zur Laufzeit Zeile fuer Zeile das Programm und uebersetzt Zeile fuer Zeile jede
Instruktion in eine aequivalente Maschineninstruktion fuer den Prozessor.

### Compiler

* Takes Entire program as input.
* It takes large amount of time to analyze the source code but the overall
  execution time is comparatively faster.
* Generates intermediate object/machine code (synonyms).
* Programs require more memory because the entire object code has to reside in
  memory ("Generates intermediate object code which further requires linking,
  hence requires more memory")
* Program need not be compiled every time.
* It generates the error message only after scanning the whole program. Hence
  debugging is comparatively hard.
* Can optimize globally, looking beyond single instructions.
* Programming language like C, C++ use compilers.

### Interpreter

* Takes Single Instructions as input.
* It takes less amount of time to analyze the source code but the overall
  execution time is slower.
* No intermediate object code is generated, hence are memory efficient.
* Programs have to be interpreted line-by-line every time they are run.
* Continues translating the program until the first error is met, in which case
  it stops. Can combine error message with source code. Easier debugging.
* Cannot optimize outside of local context.
* Programming language like Python, Ruby use interpreters.

http://www.programiz.com/article/difference-compiler-interpreter
http://www.c4learn.com/c-programming/compiler-vs-interpreter/

### Hybrid Compiler

Ein Hybrid Compiler, auch Just-In-Time (JIT) Compiler genannt, ist eine Mischung
aus Compiler und Interpreter. Hiebei kompiliert zuerst ein normaler Compiler den
Code in eine Zwischenrepresentation (`javac` -> bytecode), welcher schliesslich
auf einer virtuellen Maschine (JVM) interpretiert werden koennte (`java -Xint`
*interpretiert* den bytecode).

Ein JIT-Compiler hat jetzt aber noch die zusaetzliche Funktionalitaet, dass es
den bytecode nicht nur interpretieren kann, sondern auch zusaetzlich einen Count
haelt darueber, wie oft eine gewisse Instruktion nun schon interpretiert
wurde. Nach einem gewissen Limit fuer diesen Count kompiliert der Compiler den
Code in Maschinencode, sodass er fuer nachfolgende Aufrufe nicht mehr
interpretiert werden muss (also Zeile fuer Zeile in Maschineninstruktionen
umgewandelt wird), sondern schon direkt als Maschinencode an den Prozessor
gesandt werden kann. Diesen Count muss es nicht immer geben, manchmal wird der
Code auch gleich beim ersten Mal kompiliert. Jedenfalls wird er erst zur
Laufzeit unmittelbar vor dem Gebrauch in Maschinencode umgewandelt und nicht
schon vorher, wie es ein Assemblierer oder C-Compiler tun wuerde.

Durch dieses JIT Verhalten kann der Compiler auch zur Ausfuehrungszeit
Optimisierungen z.B. auf Basis der Haufigkeit eines Funktionsaufrufs
durchfuehren. So kann er beispielsweise eine Funktion inlinen, wenn sie haufig
aufgerufen wird.

Ein weiterer Vorteil eines JIT-Compilers ist, dass der durch den ersten Compiler
generierte Zwischencode sehr portabel ist. Man muss nur auf jeder Zielplattform
die Virtuelle Maschine realisieren.

http://stackoverflow.com/questions/95635/what-does-a-just-in-time-jit-compiler-do

## Compiler

Der Vorgang eines Compilers umfasst zwei Phasen: Frontend und Backend. In der
Frontend Phase wird Code in einer bestimmten Sprache in einen Zwischencode
(intermediate representation) uebersetzt. In der Backend-Phase wird der
Zwischencode dann in Zielmaschinencode uebersetzt. Mehrere Sprachen koennen ein
Frontend fuer das selbe Backend haben. *clang* ist beispielsweise das C++ und
Objective-C Frontend fuer den *LLVM* Backend. Genauer:

* Frontend:
  + Lexikalische Analyse: Ein Bytestream wird in eine Folge von Tokens
    uebersetzt. Weiters wird eine Symboltabelle angelegt.
  + Syntaktsiche Analyse: Der Token-Stream wird in einen Syntaxbaum
    uebersetzt. Hierbei werden syntaktische Fehler ueberprueft,
    also die Anordnung der Tokens.
  + Semantische Analyse: Hier wird der Token-Stream auf semantische
    Eigenschaften wie korrekte Typisierung analysiert. Auch IDs werden gesammelt
    und es wird sichergestellt, dass z.B. IDs bei der Nutzung schon deklariert
    sind. Das Resultat ist ein annotierter Syntaxbaum.
  + Zwischencode-Generierung: Hierbei wird der Code in eine Representation
    gebracht, die zwischen dem High-Level und Maschinen-Code steht und vom
    Backend weiter verwendet werden kann.

Zur Optimierung arbeitet der Compiler oft mit sogenannten
*Basisbloecken*. Hierzu wird der Code in Dreiadresscode
strukturiert. Dreiadresscodes sind Instruktionen der Form `destination =
operand1 <operation> operand2`. Ein Basisblock ist dann die maximale Folge von
Dreiadressbefehlen, die zwei Bedingungen erfuellen:

1. Single-Entry: Der Basisblock kann nur durch die erste Instruktion betreten
   werden.
2. Single-Exit: Der Basisblock kann nur durch die letzte Instruktion verlassen
   werden.

Mit diesen Basisbloecken kann man z.B. toten von lebendigen Variablen
unterscheiden.

* Backend:
  + Maschinenunabhaengige Optimierung: Der Zwischencode wird veraendert, sodass
    der Zielcode schneller, kuerzer und energieeffzienter ist.
  + Code-Generierung: Aus dem optimierten Zwischencode wird direkt Maschinencode    generiert. Variablen erhalten dabei Speicheradressen
    und Register werden zugewiesen.
  + Maschinenabhaengige Optimierung: Hierbei werden Optimierungen des
    Maschinencodes durchgefuehrt, um letztendlich den Zielmaschinencode zu
    generieren.

### Andere Anwendungen

Andere Anwendungen vom Compilern sind:

* Binary Translation: Maschinencode wird zwischen Plattformen wie x86 und Sparc
  uebersetzt.
* Hardware-Synthese: Hardware wird in Software beschrieben und fuer
  Manufakturplaene kompiliert.
* Interpreter fuer Datenbankanfragen (SQL)
* Interpreter fuer PostScript (Druckersprache)

http://www.tutorialspoint.com/compiler_design/compiler_design_phases_of_compiler.htmA

## Speicherorganisation

Jedem Programm stehen mehrere Speicherraeume zur Verfuegung:

* Code: Programminstruktionen
* Static: Statische Programmvariablen.
* Heap: Dynamische Datenstrukturen.
* Stack: Fuer Prozeduraufrufe.

Diese werden von der Laufzeitumgebung verwaltet (runtime-environment).

### Statische Speicherzuteilung

Die statische Speicherzuteilung umfasst jene Daten, die schon zur
Uebersetzungszeit feststehen und welche waehrend der gesamten Laufzeit
exisitieren. Diese Speicherzuteilung erfolgt vom Compiler, welcher auch das
Padding von Daten zur Sicherstellung von Alignment-Eigenschaften uebernimmt
(Datum von $n$ Bytes sollte auf einer Startadresse von $x \mod n = 0$; Daten
sollten auch Zweierpotenzgroessen haben).

### Dynamische Speicherzuteilung

Dynamische Speicherzuteilung ist jene Speicherzuteilung, die nicht durch den
Compiler erfolgen kann. Sie muss also zur Laufzeit erfolgen. Hierfuer gibt es
nun noch zwei Moeglichkeiten:

1. Allokation auf dem Stack
2. Allokation auf dem Heap

#### Verwaltung des Stacks

Der Stack (Stapel; Keller) dient zuer Verwaltung von Funktionsaufrufen. Fuer
jeden Funktionsaufruf wird hierbei je nach Konvention (*Calling-Convention*) ein
bestimmter Block -- der sogenannte *Aktivierungsblock* -- mit Programmstatus auf
den Stack gegeben. Fuer die C Calling-Convention umfasst dieser meist (in dieser
Reihenfolge):

1. Funktionsparameter
2. Die Ruecksprungadresse
3. Den Zeiger auf den vorherigen Aktivierungsblock
4. Lokale Variablen.
5. Gesicherte Register.

In manchen Faellen wird auch noch der Rueckgabewert vom Stack genommen.

#### Verwaltung des Heaps

Daten auf dem Heap (auch: "free store") werden vom Programm in speziellen
Datenrauemen allokiert und muessen vom Programm auch selber wieder de-allokiert
werden.

##### Allokation

Zur Allokation muss das Laufzeitsystem einen passenden freien Speicherbereich
finden. Hierzu verwaltet sie eine Liste von freien Bereichen -- die *free
list*. Wird ein neues Datum allokiert, wird sein zugewiesener Datenraum aus der
free list entfernt.

##### Freigabe

Bei der Freigabe kann der dem Datum zugewiesene Datenbereich wieder in die free
list gegeben werden. Hierbei koennen zur Verhinderung von Speicherfragmentierung
noch benachbarte Gebiete verschmolzen werden.
