# Standardschaltnetze

Es gibt einige wichtige Standardschaltnetze die sehr oft verwendet werden und
dessen Struktur bekannt sein sollte. Grundsaetzlich kann man diese in drei
funktionale Gruppen unterteilen:

1. Funktionen zum Umkodieren (En-/Dekoder)
2. Funktionen zum Auswaehlen von Werten (Mux/Demux)
3. Arithmetische Funktionen ($+$, $-$, $=$)

## Umkodierende Funktionen

### Enkoder

Ein Enkoder (Verschluessler) ist ein Schaltnetz, das ein $n$-stelliges
Eingangswort auf ein *eindeutiges* $m$-stelliges Ausgangswort abbildet. Diese
Abbildung muss zwar weder injektiv noch surjektiv sein, muss jedoch die
Grundvoraussetzung einer Funktion erfuellen (dass es fuer jedes Eingangswort
ein eindeutiges Ausgangswort gibt). Hierbei ist $m$ zwar nicht zwingend, aber
meistens doch kleiner als $n$.

Am einfachsten stellt man es sich vor, dass nur ein Signal genau einen Code
(Ausganswort) produziert. Jedoch koennen natuerlich mehrere Signale (die
zusammen einen eindeutigen Zustand darstellen) auf einen Code abgebildet werden
(und nur diesen).

#### Beispiele

Eine einfache Art von Enkodierung ware z.B. eine binaere Kodierung. Ein solcher
Enkoder haette also $n = 2^m$ Eingaenge und $m$ Ausgaenge. Ein weiteres
Beispiel fuer einen Enkoder ist jener, der in einer Tastatur einen Tastendruck
(also ein Signal) in einen Zeichencode (also einer Dezimahlzahl) umwandelt.

#### Gatterebene

Auf der Gatterebene werden Enkoder so implementiert, dass jedes Ausgangssignal
(z.B. jeder Bit) des Codes durch eine Disjunktion aller Eingangssignale, die
dieses Ausgangssignal aktivieren, erzeugt wird. Beispielsweise gibt es fuer Bit
$0$ eine Disjunktion, woran alle ungeraden Zahlen (Signale) angeschlossen sind.

```

2----\
3 ----[>=1] -- Bit 1
7----/
  |
4---[>=1] -- Bit 2
```

#### VHDL

 ```vhdl
 process(eingang)
 begin
	 case eingang is
		when "1000...0" => c <= c0; -- c0 ist vordefinierter Code
		when "0100...0" => c <= c1;
		...
		when others =>
 end process;
 ```

### Dekoder

Ein Dekoder erfuellt nun die Umkehrfunktion zu einem Enkoder. In seiner
grundlegenen Beschreibung hoert es sich jedoch sehr gleich an: Es bildet ein
$n$-stelliges Eingangswort auf genau ein eindeutiges $m$-stelliges Ausgangswort
ab.

Am einfachsten stellt man sich einen Dekoder vor, dass es einen Eingangscode auf
nur ein einziges Ausgangssignal abbildet. Grundsaetzlich kann ein Eingangscode
aber wieder mehrere Ausgangssignale aktivieren, die fuer einen eindeutigen
Zustand stehen.

#### Beispiele

Ein Dekoder wuerde nun z.B. einen $n$-stelligen Binaercode auf genau ein
einziges Signal abbilden, dass diese Zahl repraesentiert. Es gaebe also $n$
Eingaenge und $m = 2^n$ Ausgaenge. Ein weiteres Beispiel fuer Dekodierung waere
der Prozess durch welchen eine IP Adresse auf einen Server abgebildet wird.

#### Gatterebene

Mit Gattern wird ein Dekoder so realisiert, dass man alle Eingaenge verschieden
negiert durch Konjunktionen schleust, mit einem oder mehreren Ausgaengen. Jeder
Minterm steht dabei also fuer genau Kombination der Eingaenge und wird auf genau
ein Signal abgebildet.

#### VHDL

```vhdl
c <= "1000" when (e = "00") else
     "0100" when (e = "01") else
	 ...
	 "1000";
```

### ROM

Mit einem Decoder und einem Encoder lassen sich sogar auch ein ROM
(read-only-memory) Speicher implementieren. Der Decoder dekodiert dabei eine
$n$-stellige Speicheradresse auf ein Datensignal, welches der Decoder wiederum
in ein Datenwort (z.B. in binaerer Darstellung) umkodiert.

## Auswaehlende Funktionen

### Datenwegschalter

Ein Datenwegschalter ist ein Gatter, das ein Signal nur durchlaesst, wenn ein
anders Steuersignal -- meist *Enable* genannt -- gesetzt ist. Ein
Datenwegschalter ist grundsaetzlich einfach ein AND-Gatter. Man interpretiert
die Operation aber nicht als logisches AND zwischen zwei Bool'schen Variablen,
sondern eben als das Durchschalten eines Signals in Abhaengigkeit eines
Steuersignals.

#### VHDL

Sei $e$ das Datensignal und $s$ das Steuersignal.

```vhdl
a <= e and s; -- oder,

a <= e when s = '1' else '0'; -- oder, in Prozessen

if s = '1' then a <= e; else a <= '0'; end if; -- oder

a <= '0'; -- default
if s = '1' then a <= e; end if;
```

### Datenwegmischer

Ein Datenwegmischer ist ein ODER-Gatter, in das $n$ Datenwegschalter
angeschlossen sind. Jeder einzelne Datenwegschalter (AND-Gatter) hat also ein
Datensignal und ein Steuersignal. Wir gehen davon aus, dass immer nur ein
Steuersignal ein ist. Somit kann man also kontrollieren, welches von $n$
Datensignalen durch den Datenwegmischer geschleusst wird.

#### VHDL

```vhdl
a <= (e_1 and s_1) or (e_1 and s_1) or ... (e_n and s_n);
```

### Datenwegverteiler

Ein Datenwegverteiler ist nun sozusagen das Gegenstueck zum Datenwegmischer. Es
hat nur einen Eingang, das Datensignal, und ist an $n$ Datenwegschalter
angeschlossen. Jeder einzelne Datenwegschalter hat also einen eigenes
Steuersignal. Somit kann man bestimmten, in welche Wege (Ausgaenge) das Datum am
Eingang verteilt wird.

#### VHDL

```vhdl
a_1 <= e when s_1 = '1' else '0';
a_2 <= e when s_2 = '1' else '0';
.
.
.
a_n <= e when s_n = '1' else '0';
```

### Multiplexer

Ein Multiplexer ist nun ein Datenwegmischer verbunden mit einem Dekoder. Beim
Datenwegmischer gab es noch das Problem, dass mehrere Steuersignale ein sein
konnten. Das war letztendlich egal, aber war eben auch unsinnig. Mit dem Dekoder
kann man nun nicht nur den Multiplexer steuern, sondern auch sicherstellen, dass
jedes Eingangswort (Code) auf nur ein Steuersignal abgebildet wird.

#### Beispiel

In der Elektronik sind Multiplexer nuetzlich, um zwischen Eingabequellen zu
wechseln. Hat man z.B. auf seinem Mikrokontroller nur $4$ I/O Pins, aber $8$
Eingabequellen, so hat man bei einer "horizontalen" Kodierung ein Problem. Man
kann aber $3$ Pins fuer eine drei-Bit Kodierung verwenden, und den letzten Pins
fuer ein Eingabesignal. Nun kann man mit den drei Bits durch die $8$ Pins
durchiterieren, und jedes mal die Eingabe abfragen.

Bei der mikroprogrammierbaren Maschine ist der K-MUX ein Multiplexer (er
schaltet entweder das Datum vom Datenbus oder die Konstante vom Konstantenfeld
auf den Dateneingang der ALU).

### Demultiplexer

Ein Demultiplexer ist nun ein Datenverteiler verbunden mit einem Dekoder. Es
gibt also ein Eingabesignal (das Datum) und der Dekoder bestimmt, auf welche(n)
Weg(e) dieses Datum verteilt wird.

#### Beispiel

In der Elektronik ist zwischen Multiplexern und Demultiplexern kein grosser
Unterschied, weil man bei Mikrokontrollern meist bestimmen kann, ob Pins zur
Eingabe oder zur Ausgabe (der Modus) verwendet werden sollen. Bei einem
Demultiplexer wuerde man nun nach vorherigem Beispiel nicht $8$ Eingabequellen,
sondern $8$ Ausgabeziele haben. Dann kann man mit dem $3$-Bit Code
kontrollieren, auf welchen der $8$ Ausgaenge das Datum vom letzten Pin verteilt
wird.

Bei der mikroprogrammierbaren Maschine ist der Y-MUX ein Demultiplexer (er
verteilt das Datum vom F-Ausgang der ALU entweder auf den Daten- oder
Adressbus).

#### VHDL

3-auf-8 Demultiplexer ($3$-Bit Code, $8$ Datenwege)

```vhdl
process(c)
begin
	a <= x"00"; -- hex! = 8 x 0 Bits
	case c is
		when "000" => a(0) <= e;
		when "001" => a(1) <= e;
		when "010" => a(2) <= e;
		.
		.
		.
		when "111" => a(7) <= e;
		when others =>
	end case;
end process;
```

## Arithmetische Funktionen

### Vergleiche

Vergleiche von zwei Signalen $a = b$ koennen ganz einfach durch ein XNOR (not
XOR) Gatter realisiert werden. $\neg(A \oplus B)$ ist naemlich aequivalent zu $a
\leftrightarrow b$ und das ist eben die Gleichheit. Also ein XNOR Gatter hat die
Einsmenge $\{(0, 0), (1, 1)\}$.

Moechte man nun Zahlen mit mehreren Bits miteinander vergleichen (auf
Gleichheit), so muss man einfach jeden einzelnen Bit (bzw. jede Stelle in der
binaeren Zahl) fuer alle Zahlen in ein XNOR Gatter schliessen, und die Resultate
aller XNOR Gatter durch ein AND Gatter schleusen.

```
2-Bit a = b

a_0--\
      [=1]!--\
b_0--/	      \
               [&]---(a=b)
a_1--\        /
      [=1]!--/
b_1--/
```

### Addierer

Addierer koennen in einer Schaltung so implementiert werden, dass man pro Bit
einen Baustein hat, der einen Carry-In Eingang und ein einen Carry-Out Ausgang
hat. In jedem solchem Baustein wird dann ein Bit addiert, indem man den einen
Eingangsbit mit dem anderen Eingangsbit XOR-ed, und das Resultat noch mit dem
Carry-In Bit XOR-ed (Reihenfolge wegen Assoziativitaet und Kommutativitaet
egal).

Seien also $a,b$ die Eingangsbits und $y$ der resultierende Bit des Resultats an
dieser Stelle, und $c_{in, out}$ das Carry-In bzw. Carry-Out Signal, dann gelten
folgende Gleichungen:

$y = (a \oplus b) \oplus c_{in}$
$c_{out} = ab + c_{in}(a + b)$

Fuer die vielen Bits einer mehr-bittigen Addition kann man dann einfach den hier
beschriebenen Baustein kaskadieren, also man verbindet das Carry-Out des einen
Bausteins mit dem Carry-In des anderen.

#### VHDL

In VHDL kann man Addierer ganz einfach mit dem eingebauten `unsigned`
bzw. `signed` Datentyp realisieren. Diese erlauben naemlich arithmetische
Operationen und Vergleiche, wohingegen `std_logic_vector` diese nicht
unterstuetzt.

Ziel und Quelloperanden muessen jedoch die selbe Bitbreite haben. Wenn nicht,
muss man den kleineren mit $0$-en auffuellen. Das geht mit dem
Konkatenierungsoperator `&`:

`vektor_10Bit <= ("00" & vektor_8Bit) + ("0000000" & vektor_3Bit);`

Konstanten koenen dabei in Binaer, Dezimal oder Hexadezimal angegeben werden:

```vhdl
signal a: unsigned(x downto y);

a <= b + "101";
a <= b + 49;
a <= b + x"42";
```

Aber Achtung, man kann nur Vektoren zuweisen (bei Dezimahlzahlen ist die Breite
noch undefiniert):

```vhdl
a <= 40; -- Falsch!

a <= "101000" -- So!
```

### Subtraktion

Die Subtraktion ist im Wesentlichen analog zur Addition. Sie bildet auch die
Grundlage fuer jegliche Groessenvergleiche ($<, >, \leq, \geq$).

Alternativ kann man in einer Schaltung auch das Einerkomplement vom Subtrahenden
bilden, und das Carry-In auf `1` stellen, so wie es die mikroprogrammierbare
Maschine macht.
