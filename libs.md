# Bibliotheken

Bibliotheken (Libraries) stellen eine Moeglichkeit dar, Code fuer schelle
Einbindung in andere Projekte zu verpacken. Sie koennen auch Programmteile
zusammenfassen, um den Bindevorgang zu beschleunigen. Es gibt drei Klassen von
Bibliotheken:

* Statische Bibliotheken
* Dynamische Bibliotheken
* Gemeinsame Bibliotheken

## Statische Bibliotheken

Eine statische Bibliothek (*static library*; `.a` auf UNIX, `.lib` auf Windows)
ist eine Bibliothek, die schon zu Compile-Time in die resultierende,
ausfuehrbare Datei eines Programms geladen wird. Es besteht also keine
Abhaengigkeit von der Bibliothek zur Laufzeit, weil jeglicher verwendeter Code
schon zur Kompilierungszeit in die Datei eingeschrieben wurde.

Vorteile:
+ Keine Laufzeit Dependencies
+ Schnelleres Laden

Nachteile:
- Hoehere Groesse der Binaries (ausfuehrbare Dateien)
- Code-Duplikation in mehreren Dateien


## Gemeinsame Bibliotheken

Gemeinsame Bibliotheken (*shared library*) sind eine allgemeine Klasse von
Bibliotheken, wobei der Bibliotheks-Code nicht statisch in die Datei kompiliert
und somit eine *monolitische* ausfuehrbare Datei erstellt wird, sondern erst
beim Laden (vor dem Ausfuehren) oder sogar erst zur Laufzeit die Referenzen
bzw. Symbole im Code aufgeloest werden.

Das Programm macht also nur Referenzen zu Symbolen (zu den *Deklarationen*). Die
Symbol-Resolution hierbei erfolgt durch einen speziellen Lader durch das
Betriebssystem. Dieser bindet die Deklarationen dann zu den assoziierten
*Definitionen*. Allgemein erfolgt die Adressbindung hier beim Laden des
Programms, wobei aber Dynamische Bibliotheken sogar erst zur Laufzeit binden.

Mit gemeinsamen Bibliotheken koennen auch mehrere Dateien gleichzeitig die
Bibliothek verwenden, ohne jeweils den Code in der Datei einmeisseln zu
muessen. Auch kann man die Bibliothek leicht durch eine andere mit
den selben Symbolen ersetzten, ohne re-kompilieren zu muessen.

Vorteile:
+ Leichter zu ersetzen.
+ Weniger Code-Duplikation (spart Hauptspeicherplatz).
+ Geringere Binary-Groesse.

Nachteile:
- Eventuell Versionskonflikte.
- Kann auf dem System fehlen.

## Dynamische Bibliotheken

Eine dynamische Bibliothek (*dynamic library*; `.so` auf Linux, `.dll` auf
Windows, `.dylib` auf OS X) ist eine Art von gemeinsamer Bibliothek. Hier wird
erst zur Laufzeit die dynamische Bibliothek in den Speicher gelanden und ein
Runtime-Linker loest die Referenzen (zur Laufzeit) auf. (Deswegen muss man also
in C die Header inkludieren, damit man Referenzen zu den Deklarationen machen
kann).

Die in der `.dylib` Datei enthaltenen Definitionen werden also erst zur Laufzeit
eingebunden.  Das kann Vorteile bringen, da man nur den Code ladet, den man
braucht. Jedoch leidet die Laufzeit ein wenig unter dem Overhead der
Laufzeitbindung.

Vorteile:
+ Leichter zu ersetzen.
+ Weniger Code-Duplikation (spart Hauptspeicherplatz).
+ Geringere Binary-Groesse.
+ Lazy Laden -- "on-demand".

Nachteile:
- Laufzeit leidet ein wenig.
- Eventuell Versionskonflikte.
- Kann auf dem System fehlen.
