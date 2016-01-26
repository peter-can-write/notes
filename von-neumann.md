# Von-Neumann Architekturkonzept

Das Architekturkonzept der meisten Rechner heutzutage basiert auf Ueberlegungen
des amerikanischen Wissenschaftlers John von Neumann.

## Aufbau

In diesem Architekturkonzept besteht der Rechner aus vier verschiedenen Werken:

1. Einem *Haupt- bzw. Arbeitsspeicher*, indem *sowohl* Programme *als auch*
   Daten gespeichert werden. Dazu zaehlen heute auch Caches.

2. Einem *Leitwerk*, das Programme, die im Hauptspeicher gespeichert sind,
   dekodiert bzw. interpretiert und ausfuehrt.

3. Einem *Rechenwerk*, dass saemtliche arithmetische und logische Operationen
   durchfuehrt. Eventuell kann man zu diesem auch die Grafikkarte (GPU) zaehlen.

4. Einem *Ein-/Ausgabewerk*, dass mit Peripheriegeraeten (Bildschirm, Maus,
   externen Speichermedien) ueber verschiedenen Bussen (PCI, SATA, Serieller
   Anschluss) kommuniziert.

Das Leitwerk beeinhaltet hierbei noch (nach kanonischem Konzept) drei
verschieden Register:

* Ein *Befehlszaehler-Register*, dass die Adresse der momentan (oder als
  naechstes) ausgefuehrten Instruktion im Hauptspeicher enthaelt.

* Ein *Instruktions-Register*, dass die momentan ausgefuehrte, vorher vom
  Hauptspeicher geholte, und nun dekodierte Instruktion enthaelt.

* Ein *Status-Register*, dass bestimmte Bits zur Bestimmung des Programmstatus
  bzw. zur Bestimmung bestimmter Bedingungen in Abhaengigkeit der letzten
  arithmetischen Operation im Rechenwerk, beinhaltet.

Das Rechenwerk enthaelt auch ein Register, naemlich das sogeannnte
*Akkumulator-Register*, auf welchem saemtliche arithmetischen oder sonstigen
Operationen des Rechenwerks ausgefuehrt werden sollten.

## Prinzipien

Des Weiteren hat John von Neumann einige Prinzipien festgelegt:

* Die Struktur des Rechners soll unabhaengig vom bearbeiteten Programm sein. Der
  Rechner soll also fuer alle moeglichen Zwecke dienen, und nicht nur fuer eine
  bestimmte Kategorie von Berechnung gebaut sein.

* Programme und Daten liegen im selben Speicher und koennen durch die Maschine
  veraendert werden. So sind z.B. auch selbstmodifizierende Programme erlaubt.

* Der Hauptspeicher ist in Zellen *gleicher Groesse* geteilt, die durch
  *fortlaufende* Nummern angesprochen werden. Diese Nummern nennt man dann
  *Adressen*.

* Ein Programm besteht aus einer Folge von Befehlen (Instruktionen), die
  nacheinander, also *sequentiell*, ausgefuehrt werden.

* Von dieser sequentiellen Fortschaltung kann durch bedingte oder unbedingte
  Sprunbefehle abgewichen werden, die die Programmfortsetuzng aus einer anderen
  Zelle bewirken. Beidngte Spruenge sind hierbei von gespeicherten Werten
  (z.B. dem Statusregister) abhaengig.

* Die Maschine benutzt Binaercodes, Zahlen werden also dual dargestellt.

## Unterschiede Heute

Heutige Rechner weisen einige Unterschiede zum Von-Neumann Konzept auf:

* Leit- und Rechenwerk sind im Prozessor zusammengefasst
* Speicher sind in eine Speicherhierarchie aufgespalten (Register, Cache,
  Hauptspeicher, Hintergrundspeicher, Archivspeicher).
* Indirekter Anschluss der E/A-Geraete uber device controller.
* Mehrere Rechenwerke im Zentralprozessor.
* Mehrere Zentralprozessoren pro Rechner in Multiprozessorsystemen.
* Mehrere Zentralprozessoren pro Chip in Multikernrechnern.
* Befehlsausfuehrung ist durch Pipelining und Superskalaritaet heutzutage nicht
  mehr streng seriell.
* Auch Interrupts verletzten das Prinzip der Seriellitaet der Instruktionen.
* Rechner haben immer mehr als nur ein Akkumulator Register.

Unter *Superskalaritaet* bzw. dem *Superskalaprinzip* versteht man die
Eigenschaft eines Prozessors, mehrere Befehle aus einem Befehlsstrom
gleichzeitig mit mehreren parallel arbeitenden Funktionseinheiten zu
verarbeiten. (https://de.wikipedia.org/wiki/Superskalarität)

Speicher bestehen auch heute noch aus Zellen fester Wortlaenge, was aber auch zu
Kritik fuehrt. So waehre es beispielsweise einfacher Konsistenz und Sicherheit
zu ueberwachen, wenn durch die Groesse der Zellen Datentypinformationen
*mitgespeichert* (kodiert) werden, quasy "strongly-typed memory".

Man hat auch gemerkt, dass gerade die Verbindung zwischen Leit- bzw. Rechnewerk
und dem Hauptspeicher der Haupt-Flaschenhals in der Von-Neumann Architektur
ist. Deswegen gibt es eben Caches bzw. generell eine Speicherhierarchie. Dieses
Problem wird auch *Von-Neumann-Flaschenhals* genannt.

### Schichten

Bei Rechnern unterscheidet man heute zwischen sechs Schichten:

1. Benutzerprogramm-Schicht

In höherer Programmiersprache definiert: Variablen, verschiedenen, eventuell
selbstdefinierten Typs. Oftmals weitere Schichten (Betriebssystemsschicht,
Awenderschicht etc.)

2. Von-Neumann-Schicht

Definiert durch Maschinenbefehlssatz und die Objekte, auf denen dieser arbeitet:
Wörter, Adressen, Speicherzellen, Register. Durch Mikroprogrammierung
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

Definiert durch idealisierte Bauelemente, z.B Widerstände, Kondensatoren,
Transistoren, Leitungen etc.

6. Physikalische-Schicht (Silicium, Metal-Oxid etc.)
