# Rechnerarchitektur - 16.10.2015

### Operatoren

* `AND`
* `OR`
* `NOT`
* `NOR`
* `NAND`
* `XOR`
* `XNOR`
* `SLL` (shift left  logically)
* `SRL` (shift right logically)
* `SLA` (shift left  arithmetically)
* `SRA` (shift right arithmetically)
* `ROL` (rotate-left)
* `ROR` (rotate-right)
* `=` equality (like `==`, not assignment)
* `/=` inequality (like `!=`)
* Conditions: `>`, `<`, `<=`, `>=`, `and`, `or`, `xor`, `nand`, `nor`, `xnor`
* __Modulo: `mod` (nicht `%`!!!!)__
* Abs: `abs`
* Standard arithmetic operators: `+`, `-`, `/`, `*`, `**` (power)
* Konkatenierungsoperator fuer Vektoren: `&`
* Mehrere Faelle fuer Case-Statements: `|`

Gross/Kleinschreibung egal. Operatoren haben keine Praezedenz (obwohl in der
Bool'schen Algebra $\land$ vor $\lor$ kommt).

### Datentypen

Konstanten in Anfuehrungszeichen: '0', '1'

* `std_logic`, hat z.B. noch Werte fuer Kurzschluss oder Undefined.

* `std_logic_vector`: Vektor von `std_logic`.

* `unsigned`: Vektor, der noch als vorzeichenlose Zahl eine Bedeutung hat.

* `signed`: Vektor, der noch als vorzeichenbehaftete Zahl eine Bedeutung hat.

* Konstanter Vektor: Mit doppelten Anfuehrungszeichen: `x <= "1010"` (hex: `x <=
  x"A"`)

* Subscript Operator mit `()`, nicht `[]`: `ein_bit <= vektor1(5)`.
  * Das geht auch fuer mehrere Bits: `vektor(3 downto 0)` adressiert die unteren
    $4$ Bits.

#### std_logic

`boolean` ist ein primitiver Datentyp, der die Werte $0$ oder $1$ annehmen
kann. Man nehme aber den folgenden Code:

```vhdl
x <= 1;
x <= 0;
-- Welchen Wert sollte x jetzt sinnvollerweise haben?
-- Achtung: Die Zuweisungen erfolgen VHDL-typisch gleichzeitig!
```

Fuer solche Faelle gibt es in VHDL einen besonderen Datentyp `std_logic`. Dieser
kann neben den normalen Werten $0$ und $1$ auch noch anderen spezielle Werte
annehmen. Es hat also intern eine Matrix, die verschiedene Kombinationen von
Werte zu einem Resultat aufloest. Z.B. gibt es:

* `X`: Kurzschluss. Dieser wuerde oben angenommen werden (gleichzetig $0$ und
  $1$).
* `U`: Undefined, der Wert von `std_logic`, wenn noch kein Signal zugewiesen
  wurde.
* Man kann einem `std_logic` Defaultwerte geben, indem man `H` ("Weak HIGH")
  oder `L` ("Weak LOW") zuweist. Wird kein weiteres Signal zugewiesen, wird der
  Wert des Eingangs/Ausgags als dieser Defaultwert angenommen. Wird jedoch
  spaeter ein anderes Signal zugewiesen, dann hat dieses neue/spaetere Signal
  Praezedenz (deswegen ist das andere "weak")!
* `Z`: Deaktiviert. So kann man z.B. erreichen, dass ein Ausgang von den
  Eingangssignalen unbeeinflusst bleibt. Z.B. fuer Tri-state Busse wichtig. Also
  ein Zustand, der von jedem anderen immer ueberschrieben werden kann.

Fuer logische Operationen (arithmetische sind nicht erlaubt) hat der Datentyp
intern eine Resolutionstabelle, die besagt, dass z.B. `1 and X` wieder `X`,
oder dass `1 or 0` gleich `0`.

Wichtig jedoch ist, in Ausdruecken zwischen `std_logic` und `boolean` zu
unterscheiden. Logische Operationen zwischen `std_logic` Instanzen ergeben
wieder `std_logic`, aber diese evaluieren nicht implizit zu Boolean. Man muss
also z.B. in Bedingungen (`if`-clauses) erst mit einem Ausdruck wie `x = '1'`
eine Boolean erschaffen:

```vhdl
signal a, b: std_logic;

a and b -- std_logic
a = '1' -- boolean
(a = '1') and (b = '0') -- boolean
```

Man kann Booleans dann auch nicht zu `std_logic` zuweisen!

`a <= (b = `1`) -- Error!`

Das ist auch bei Vergleichen von `signed`/`unsigned` wichtig:

```vhdl
signal c: unsigned(1 downto 0);

a <= (c > 5); -- Error!

a <= '1' when (c > 5) else '0'; -- SO!
```

Auch in `if`-clauses:

```vhdl
if a then -- Error!
-- ...
end if;

if a = '1' then -- SO!
-- ...
end if;
```

#### `unsigned` vs `std_logic_vector`

`unsigned` hat den Vorteil, dass man damit arithmetische Operationen
durchfuehren kann (z.B. `z = z + 1`), sowie auch arithmetische Vergleiche
(z.B. `z > 1`). Man kann fuer  `unsigned` in der `when <condition>` Klausel
jedoch keine Vergleiche mit Dezimalzahlen durchfuehren. Auch kann man ihnen nie
__Dezimahlzahlen__ zuweisen!!!!! Muessen also immer Bit-Vektoren sein.

`std_logic` dagegen kann keine dieser arithmetischen Operationen/Vergleiche, hat
aber den Vorteil der verschiedenen Zustaende (nicht nur $true$/$false$).

Fuehr Zaehler oder Zustaende sollte man daher `unsigned` verwenden. Wenn man
viele `std_logic` in einem Vektor zusammenfassen moechte, ist `std_logic_vector`
besser (bzw. die einzig richtige Variante).

## Vektorinitialisierung:

```
vector1(7 downto 0) -- 8 Bit breit, Bit 7 MSB (little-endian)
vector1(0 to 7) -- 8 Bit breit, Bit 0 MSB (big-endian)
```

Man kann in Realitaet, in Hardware, nie *genau 10ns* warten. Dafuer gibt es zu viele andere Faktoren:
* Temperatur
* Betriebsspannung
* Varianzen beim Prozessor

Asssignment bei Variablen: `a := b`
Signalzuweisung: `a <= b after 10 ns` # nicht kleiner-gleich, sondern wie Pfeil

## Entity

^ Nach 10ns soll a den Wert von b haben.

```vhdl
entity and_gatter is
	port (e1, e2: in std_logic   -- comment
		  ; x:      out std_logic);
end and_gatter;
```

* `entity`: So wie `class`, beliebig oft instanziierbares Bauteil
* `e1, e2`: Eingangsport(s)
* `in`: Der Modus, also "Eingang"
* `std_logic`: Datentyp, aehnlich boolean
* `x`: Ausgansport(s)
* `out`: Ausgangsmodus

`in/out` sind also "Modi".

__Wichtig__: die Liste von Ports ist ;-separiert, also besser so schreiben:

```vhdl
port (
		  a, b: in std_logic
		; c: in std_logic
		; d: out std_logic
		; x: in std_logic
);
```

als

```vhdl
port (
		  a, b: in std_logic;
		c: in std_logic;
		d: out std_logic;
		x: in std_logic
);
```

Damit man nicht in Versuchung kommt, die `;` ans Ende einer Deklaration zu geben, sondern wirklich als Listenseparator fuer neue Elemente sieht.

## Process

```vhdl
process(<signal>) -- sensitivity list ist <signal>, alles optional
-- variable declarations
begin
	if rising_edge(signal) then
		a <= b -- for later: a should be the signal b was
		b <= a -- for later: b should be the signal a was
	end if;
end process;
```

# Compiler error, think about children in africa!!!

They could have eaten that semicolon..

* `process`: Wird aufgerufen wenn sich ein Signal aus der sensitivity-list
  aendert.
* `sensitivity list`: Parameter von Prozess. Wann soll der Prozess aufgerufen
  werden? Ist optional!
* `a <= b; b <= a`: Lazy evaluation, es wird sich fuer spaeter gemerkt, dass
  wenn der Prozess fertig ist, dann soll das ausgefuehrt werden (ist also kein
  bug)
* Es werden wirklich bis zum end-statement, also `end process`, alle
  Signalzuweisungen gesammelt, und erst dann werden die Signale
  zugewiesen. Werden in einem Prozess einem Signal also mehrere Signale
  zugewiesen, wird nur das letzte uebernommen.
* Letztendlich ist ein Process dann selbst aber ein concurrent-statement, denn
  die gesammelten Assignments werden letztendlich wie ein einziges
  concurrent-statement umgesetzt in einem Simulation "tick" durchgefuehrt.
* Nur Signale die in der sensitivity-list aufgefuehrt sind, werden auch
  potentiell veraendert, also unbedingt alle Signale dort auflisten!

The flow is like this: there are "delta-cyles" in VHDL, units of execution
(units of zero simulated time). At the start of each delta-cycle, the
concurrent-statements are evaluated simultaneously. If any of those signals were
in the sensitivity-list of a process, the call to that process is
scheduled. After all concurrent-statements have been processed and still within
the same delta-cycle (no "physical time" has passed yet), the processes are
evaluated in sequential order, i.e. each assignment in the process is evaluated
and *scheduled for the start of the next delta-cycle*. Then, when all processes
have been evaluated and their results have been scheduled, the next delta-cycle
is initiated ("time moves on"). At the start of this new delta-cycle, the
results of the processes from the last delta-cycle (that were *scheduled*), are
now executed simultaneously again, resulting in new process-calls etc.

http://www.sigasi.com/content/vhdls-crown-jewel
https://en.m.wikibooks.org/wiki/Programmable_Logic/VHDL_Processes

`... when ... else` ist nur in concurrent-statements erlaubt, nicht in Prozessen.

Weitere Konstrukte:

### if-then-else

```vhdl
IF condition THEN
      sequence_of_statements
ELSIF condition THEN
	sequence_of_statements -- no separate END IF
ELSE
	sequence_of_statements -- no separate END IF
END IF;
```

Bedingungen werden auch mit `and` verknuepft:
`if (x = '1') and (y = "101") then`

### case-when

Wie `switch-case`:

```vhdl
CASE expression IS
      WHEN constant_value => <statements>
	  WHEN OTHERS => <statements>
END CASE;
```

Mit einem Bar kann man den selben Code fuer mehrere Faelle haben (wie leere
cases ohne break in `C++`): ``when a | b => ...`

`WHEN OTHERS =>` ist hierbei `default:`.

### for-loop

```vhdl
[<loop_label>:] -- optional
FOR <variable_name> IN <first_number> TO/DOWNTO <last_number> LOOP
      sequence_of_statements
END LOOP;
```

Loops from the first number (up)TO or DOWNTO the last_number, wie bei Vektorinitialisierung.

Note the __LOOP__ at the end.

### while-loop

```vhdl
[<loop_label>:] -- optional
WHILE condition LOOP
      sequence_of_statements
END LOOP;
```

if-then-else, case-when, for/while-loop nur in Prozessen

Miniprozess / Concurrent-statement: Prozess ohne formale Struktur, bzw. einfach "inline" Prozess:

`a <= c OR d; -- let a be (c OR d)``

a <= c when d="101" else f; -- a = (d == "101") ? c : f;

### WAIT

```vhdl
WAIT UNTIL condition;
WAIT FOR duration unit;
```

Stops the simulation for the simulated duration (does not affect the actual execution time). Only allowed in processes with an empty sensitivity-list.

In concurrent-statements kann diese duration auch gleich am Ende der Expression folgen:

```vhdl
a <= b when <condition> else c after 1 s;
```

## Instantiation

Instantiation of an entity:

```vhdl
<instance_name>:
	<entity> port map (<port_parameters>)
```

e.g.:

```vhdl
mein_and_gatter:
	and_gatter port map (eingang1, eingang2, signal_4711);
```

Parameter sind also wie fuer Konstruktor.

## Attribute

Attribute von Signale:
* a'last: Der Wert den das Signal vor der Aenderung hatte
* a'delayed(2 ns): Der Wert den das Signal vor 2ns hatte
* a'event: true wenn sich das Signal veraendert hat (__wichtig__)
* a'quiet(3 ns): true wenn sich das Signal in den letzten 3ns nicht geaendert hat
* a'range: first index, last index of bit vector (nicht immer 0 - N)
* a'length: laenge vom vektor

You can check for a rising edge on a signal `s` like so:

`s'event and s='1'`

That is how it used to be done and works most times. However, `std_logic` has states other than `0` and `1`, such as weak-HIGH `H` or weak-LOW `L` or Undefined `U`. Imagine you have a component on default-HIGH (pull-up), i.e. its value is `H` (weak so that it can be override by any strong value without short circuit) and the set it to strong HIGH (`1`). Now that will cause an event so `s'event` will be TRUE for that signal, and the value of the signal will be one so `s=1` will be TRUE, however it wasn't a rising edge because it was HIGH before too (just weakly). Therefore, you should also check if `s'last_value='0'`. This is what the `rising_edge()` function does.

http://stackoverflow.com/questions/15205202/clkevent-vs-rising-edge

## Architecture

* Implementierung einer Entity (code)
* Mehrere Versionen moeglich
* Contains:
	+ Component declarations (optional)
	+ Signalzuweisung
	+ Concurrent-statements (inline-prozesse)
	+ Prozesse
	+ Instanziierungen

```vhdl
architecture <name> of <entity> is
	-- signale
	signal <signal_name>: <data_type> [:=];
	<component declarations>
begin
	<concurrent_statements>

	<INSTANCE_NAME>: <component_name> port map(<port_parameters>);
	<INSTANCE_NAME>: entity <package>.<entity_name> port map(<port_parameters>);

	process(<signal>)
	begin
		<statements>
	end process;
end architecture;
```

Mit `:=` kann man Signalen einen initialen Wert geben: `signal a: std_logic :=
'0';`

### Instantiation

So the deal is, components are basically abstract units without definition inside an architecture that can be "concretized" via entities:

A component tells the compiler "there's going to be something with these sorts of pins on it called this at some point, but don't worry for now". It sort of defines a "socket". You can go on to describe what "wires up" to that "socket" etc.

An entity is something specific with a name and a set of pins, which the compiler can then "plug in" to that "socket" (and hence be connected to the "wires").

Now, there are two ways to use an entity in an architecture:
1. Declare a component and instantiate it with an entity
2. Instantiate an entity directly (recommended)

First method would look like this:

```vhdl
architecture my_arch of my_entity is
	component my_component port(<same ports as entity>); end component;
begin
	my_instance: my_component port map(<parameters>); -- component instantiation
end architecture;
```

Second method, which is recommended because you don't need the extra "blueprint":

```vhdl
architecture my_arch of my_entity is
begin
	-- note the extra "entity"
	-- also have to specify the package, "work" means default/current package
	my_instance: entity work.my_entity port map(<parameters>); -- better!
end architecture;
```

http://www.sigasi.com/content/four-and-half-ways-write-vhdl-instantiations
http://electronics.stackexchange.com/questions/16692/vhdl-component-vs-entity
http://web.engr.oregonstate.edu/~traylor/ece474/vhdl_lectures/component_instantiaton.pdf

### Port parameters

The other important thing about port parameters is that you can give them in default oder, i.e. the ports you pass to the port map must have the same mode and type for each argument, then it looks like a constructor i.e. `port map(s1, s2, s3)` etc. But if you want to pass them in arbitrary order, you can also map them using `port_of_entiy => signal_you_want_to_map`. So a port map could look like this:

```vhdl
entity bla is
begin
	port(e: in std_logic; a: out std_logic);
end entity;

begin architecture bla_arch of bla is
	signal s1, s2: std_logic;
begin
	my_entity: entity work.bla port map(s1, s2);
	-- or
	my_entity2: entity work.bla port map(
			e => s1
			a => s2
	);
end architecture;
```

http://vhdlguru.blogspot.de/2010/03/entity-instantiation-easy-way-of-port.html

## Packages/libraries:

```vhdl
package <package_name> is
	component <name>
		-- same stuff as for entity
	end components;
end <package_name>;
```

Usage: `use work.<package_name>.all;`

__`work` is the current package.__

## Generics: wie template-parameter. Beispiel:

```vhdl
entity register is
generic (width: integer := 8)
port (
		logic_in: in std_logic;
		data_in:  in unsigned(width - 1 downto 0);
		data_out: out unsigned(width - 1 downto 0)
	 );
end register;
```

* `width`: a generic (template) parameter, *defaults to integer with 8 bits*
* `integer := 8`: an integer with 8 bits
* unsigned: bit-vector with bits (x downto y), meaning from x to y (e.g. 7 - 0)

Example instantiation:

```vhdl
REG16: register generic map(16) -- 16 bit register
	 		    port    map(eingang1, eingang2, ausgang);
```

```vhdl
REG8: register port map(eingang1, eingang2, ausgang); -- width defaults to 8
```

Because of the default parameters, you can add the generics later.

`generate` generiert Komponenten:

```vhdl
<generate_loop_name>
for <index> in <start> to <end> generate
<label>: <entity_name> port map(<port_parameters>)
end generate <generate_loop_name>;
```

## Structural vs Behavioral Modelling

Behavioral Modelling is what you do when you define an architecture for an
entity and describe how its signals change -- you write the algorithm or
perform the logical operations that assign the proper signals to the output
ports, given the input ports.

Structural Modelling is how you describe the interaction between components
within the entity you are modelling, i.e. you map the ports between your
NAND/NOR gates and expect them to perform the operations (the logic/algorithms)
that you describe in the behavioral modelling.

## Fucking compiling the stupid fucking thing

TL;DR: `ghdl -i *.vhdl; ghdl -m entity; ./entity --wave=out.ghw; gtkwave out.ghw`

First load the fucking vhdl into the fucking work cache:

`ghdl -i *.vhdl`

Then analyze the fucking unit (i.e. entity) you want:

`ghdl -a my_stupid_entity`

Then elaborate the stupid shit:

`ghdl -e my_stupid_entity`

That creates an executable file under linux (run `-r my_stupid_entity` on OS X/Windows first).

You can also analyze and elaborate the fucking thing at once:

`ghdl -m my_stupid_entity`

Same result. Then you can simulate the stupid shit fucking simulation via `-r` or simply `./my_stupid_entity`. Pass the `--wave=my_wave_file.ghw` flag to create a fucking wave file. Then run

`gtkwave my_wave_file.ghw`

and have a great fine look at your shit.

Also clean the stupid mess you made:

`for i in $(ls); do if [ ${i%*.} != vhdl ]; then rm $i; fi; done`

http://www.fpgarelated.com/showarticle/20.php
http://stackoverflow.com/questions/17069939/how-do-i-compile-and-run-a-vhdl-program-on-mac

## Gotchas

* Modulo ist `mod`, nicht `%`
* Immer `when others =>`!!
* Wenn nicht explizit gesagt ist, dass ein Uebergang asynchron ist, immer
  synchron annehmen, ist leichter!
* Fuer viele Signalbits Vektor verwenden und ausserhalb des Prozesses dann
  einzeln zuweisen.
