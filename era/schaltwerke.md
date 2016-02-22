# Schaltwerke

Ein Schaltwerk ist, einfach ausgedrueckt, ein Schaltnetz mit internem
Zustand. Es laesst sich daher sehr leicht als endlicher Automat darstellen.

Formal ist ein Schaltwerk ("sequential network") eine Funktionseinheit mit $m$
Binaereingaengen und $r$ Binaerausgaengen, wobei es zu mindestens einem
Eingangswert *mehr als einen* zugehoerigen Ausgangswert gibt. Es handelt sich
also nicht mehr um eine reine (Schalt)Funktion. Es gibt also zumindest einen
zweiten zugehoerigen Wert, welcher __von der bisherigen Folge von
Eingangswoertern abhaengt__.

Es gibt folglich eine *innere Groesse* -- genannt der *Zustand* des
Schaltwerkes -- die von der bisherigen Folge von Eingaengen abhaengt und die
zusaetzlich den momentanten Ausgang $A$ bestimmt. Dieser momentane Zustand des
Schaltwerkes ist immer einer aus einer Zustandsmenge $Q = \{q_1, q_2, ...,
q_n\}$.

## Parallelitaet

Es gibt nun einige wichtige Definitionen zur Parallelitaet im Kontext von
Schaltwerken.

### Prozess

Ein Prozess ist eine zeitlich zergliederbare Handlung, welche also nach
zeitlich nach einander folgende Schritte beinhaltet.

### Parallel

Zwei Handlungen heissen *parallel*, wenn es mindestens einen Zeitpunkt
gibt, zu welchem beide schon begonnen wurden, aber noch nicht abgeschlossen
sind.

### Asynchrone Prozesse

Zwei Prozesse $P_1, P_2$ sind asynchron, wenn man von
keiner fixen Abfolge von Schritten in diesen Prozessen sprechen kann. Man kann
also nicht sagen, dass $P_1$ nur in Schritt $i$ uebergeht, wenn $P_2$ mit
Schritt $j$ abgeschlossen ist.

__Kommunizierende asynchrone Prozesse koennen jedoch trotzdem funktional sein,
wenn der kommunizierte Wert nicht in das Ergebnis eingeht oder wenn der Wert
einer Variablen zweimal mit demselben Wert beschrieben wird.__

### Synchronisation

Die Massnahme, zwei Prozesse synchron zu machen.

### Funktional

Eine Berechnung ist funktional, wenn sie mit den selben Eingaben
reproduzierbar immer die selben Ausgaben liefert (=> deterministisch?).

Asynchrone Prozesse, die miteinander kommunizieren, sind im allgemeinen nicht
funktional, auch wenn die Prozesse selbst funktional sind. Synchronisiert man
sie aber, dann sind sie wieder funktional.

### Synchrones Schaltwerk

Ein Schaltwerk, bei dem die Rueckkopplung durch ein Taktsignal zeitgesteuert
ist, heisst *synchrones Schaltwerk*. Liegt dieses Merkmal nicht vor, ist es
*asynchron*.

Bei Schaltwerken (also endlichen Automaten) hilft ein Takt am
Verzoegerungselement $\tau$ dabei, solange mit dem Umschalten in einen neuen
Zustand zu warten, bis die Eingaenge keine Hazards mehr enthalten.

Bei einem Schaltwerk kann man sagen:

Synchron: Daten werden immer erst mit demselben Takt angenommen (sei es Flanken-
oder Pegelgesteuert).

```vhdl
architecture arch of flip_flop is
begin
	process(clk)
	begin
		if rising_edge(clk) then
			-- ...
		end if;
	end process;
end architecture;
```

Asynchron: Bestimmte Signale werden sofort ubernommen.

```vhdl
architecture arch of flip_flop is
begin
	process(clk, reset) -- Note reset auch in sensitivity list
	begin
		if reset = '0' then
			-- ..
		elsif rising_edge(clk) then
			-- ...
		end if;
	end process;
end architecture;
```

Der Sinn von Asynchronitaet ist, dass man dem Schaltwerk dann Default-Werte
geben kann.

## Races

Ein Hazard ist in einem Schalt*netz* ein momentan auftretender, laut Bool'scher
Schaltfunktion ungueltiger Ausgangswert beim Uebergang zwischen zwei gueltigen
Schaltfunktionseingaengen, aufgrund von Verzoegerungen von Signalen bzw. deren
Ausbreitung mit endlicher Geschwindigkeit in Leitungen.

In einem Schalt*werk* nennt man so einen Hazard nun *Race*, also wenn der
Folgezustand $q'$ eines Schaltwerkes/Automaten von den Zeitbedingungen in der
Ruckkopplung $\tau$ abhaengt.

Da nicht wie beim Schaltnetz der Zustand bzw. die Ausgabe nur momentan (bis der
Uebergang vollendet ist) ungueltig ist, sondern endgueltig veraendert ist,
spricht man von einem *Critical Race*. Die "Endgueltigkeit" folgt daraus, dass
wenn bei einem Automat der Zustand sich wegen eines Critical Race einmal
veraendert, auch alle folgende Zustaende beeinflusst sind, da der Automat aus
dem neuen Zustand unter neuen Bedingungen zu neuen Folgezustaenden springen
wird.

Ein Race alleine ist noch kein Problem, wenn er z.B. genau in den Zustand
springt, der normal auch der naechste gewesen waere. Erst ein *Critical* Race
ist ein Problem.

## Flankenzaehler

Nun ein Beispiel zu einem Schaltwerk und verbunden Problemen.

Mit einem Flankenzaehler will man nur jede vierte Flanke zaehlen. Dazu verwendet
man also einen endlichen Automat, der nach zwei mal HIGH und dem zweiten mal LOW
als Ausgabe $1$ hat (= 4. Flanke), und sonst $0$.

Also: `00-->HIGH-->01-->LOW-->10-->HIGH-->11-->LOW--> Ausgabe=1`.

Nun kommen aber wieder Hazards ins Spiel: Wenn $01$ auf $10$ wechselt, gibt es
zwei Moeglichkeiten von Uebergaengen:

`01-->00-->10`

oder

`01-->11-->11`

So kann es nun sein, dass der Automat beim Uebergang von $01$ auf $10$ (eins auf
zwei) schon die Ausgabe $1$ gibt, und dann wieder auf $00$ springt. Der Automat
haette sich quasi verzaehlt. In realen Schaltungen ist es auch sehr
wahrscheinlich, dass der Uebergang von $01$ auf $01$ nicht ohne solche
Zwischenwerte geht. Hier haben wir also einen Critical Race!

### Loesungen

Es gibt zwei Massnahmen gegen Critical Races:

#### Ein-Bittige Uebergaenge

Ein Trick ist, die Zustande so zu kodieren, dass bei jedem Uebergang nur ein Bit
veraendert werden muss $\Rightarrow$ keine ungueltigen Zwischenwerte!
(Einschrittiger Code). Das geht aber nur, wenn nicht noch mehr Kanten zwischen
Zustaende sind (sodass zwischen zwei adjazenten Knoten noch immer mehrere Bits
liegen).

#### Taktung

Die beste Loesung ist jedoch: __Taktung__!

Taktung hat bei Schaltwerken (wie auch bei Flip-Flops) zwei Vorteile:

1. Hazards koennen zwar nicht vollstaendig vermieden werden, aber zumindest
   reduziert werden. Laesst man z.B. Signale nur durch, wenn das CLK-Signal HIGH
   ist, dann hat das Schaltwerk immer einen halben Takt Zeit, seine Werte
   umzustellen. Dennoch kann waehrend dem HIGH Signal noch immer ein Hazard
   passieren, wenn der CLK Takt zu schnell ist bzw. das Schaltwerk zu langsam.
   Flankengesteuerte Schaltwerke mit MS-FFs haben dabei noch einen groesseren
   Vorteil, da Veraenderungen nur bei Flanken des CLK-Signals uebernommen
   werden, also ein kleinerer Zeitraum.

2. Synchronisation mit der restlichen Maschine.
