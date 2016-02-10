# Flip Flops

### Rueckkopplungselemente

Die einfachste Zustandsspeicherung ist ein OR mit Rueckkopplung, also ein OR
Schaltnetz, wo nach dem OR ein Signal wieder in das OR fuehrt. Geht einmal ein
HIGH Signal durch die andere Leitung im OR, wird dieser HIGH also nach dem OR
wieder in das OR gespeist, und da das OR nur eine HIGH Leitung von zwei braucht,
wird das andere Signal danach irrelevant und der Zustand bleibt fuer immer HIGH.

### Speicherlemente

Ein richtiges Speicherelement ist jedoch eines, wo man den gespeicherten Zustand
wieder loeschen kann. Dazu muss man an die Rueckkopplung einfach ein AND mit
einem Steuersignal $R$(eset) legen, dann hat man einen Latch. Da AND
Bauteile aber nicht so billig sind wie NOR oder NAND, wandelt man das AND in ein
NOR, in dem man die beiden Eingaenge in dieses AND negiert, und dann die
Negation rauszieht, also $A \land B \Rightarrow \neg A \land \neg B \Rightarrow
\neg (A \lor B)$. Die Ausgangswerte sind hierbei nun zwar nicht mehr die selben,
aber man kann ja selbst definieren was 0 oder 1 bedeutet.

Den urspruenglichen Dateneingang nennt man nun $S$(et) und wird zum setzen des
Bits im Speicherelement genutzt. Das neue Steuersignal zum Loeschen des Bits
nennt man $R$(eset).

#### Latch vs Flip-Flop

Ein Latch ist pegelgesteuert (level-triggered), ein Flip-Flop ist
flankengesteuert (edge-triggered). Z.B. fuer einen MS-FF: Das ganze Ding ist ein
Flip-Flop, weil es flankengesteuert ist, aber Master und Slave selbst sind
Latches, weil sie pegelgesteuert sind.

https://www.youtube.com/watch?v=m1QBxTeVaNs

#### NOR Latch

Beim NOR-Latch sind die Eingaenge $S, R$ nicht negiert. $S$et ist dabei mit
$\overline{Q}$ verbunden und $R$eset mit $Q$.

$S \rightarrow \overline{Q}$
$R \rightarrow Q$

Ist (nur) $S$(et) HIGH, wird der Bit immer gesetzt. Ist (nur) $R$(eset) gesetzt,
wird der Bit immer geloescht. Sind keine der Steuerelemente gesetzt, wird der
Bit gespeichert.

Sind aber beide gesetzt, erhaelt man einen Sonderfall der ignoriert werden muss,
da gleichzeitig setzen und resetten keinen Sinn macht (dieses Problem wird vom
JK-Latch/FF adressiert). Sind also sowohl $S$ als auch $R$ gesetzt, werden sowohl
$\overline{Q}$ als auch $Q$ $0$.

| S | R | Q | !Q |
|:--|:--|:--|:---|
| 0 | 0 | Q | !Q |
| 1 | 0 | 1 |  0 |
| 0 | 1 | 0 |  1 |
| 1 | 1 | 0 |  0 | <-- ungueltiger Fall

```
S --[OR]!-- !Q
    \___/
	/   \
R --[OR]!-- Q
```

#### NAND-Latch

Eine andere Variante des Latch ist das sogenannte *NAND-Latch*,
welches an Stelle von NOR-Bausteinen NANDs benutzt. Dazu muessen die Eingaenge
und Ausgaenge *beide negiert* werden. Also sind die Verbindungen beim NOR-Latch
$S \rightarrow \overline{Q}, R \rightarrow Q$, sind sie beim NAND-Latch:

$\overline{S} \rightarrow Q$
$\overline{R} \rightarrow \overline{Q}$

Hier werden also die exakt inversen Steuersignale generiert. So wird der Bit
nicht mehr bei $S = R = 0$ gespeichert, sondern bei $\overline{R} = \overline{S}
= 1$ (weil die negierten Signale dann ja $0$ sind). Gesetzt wird nun bei (nur)
$\overline{S} = 0$, geloescht bei $\overline{R} = 0$ und der ungueltige Zustand
ist nun $\overline{S} = \overline{R} = 0$. Der einzige Unterschied ist, dass im
ungueltigen Zustand $Q = \overline{Q} = 1$, und nicht $0$.

| S | R | Q | !Q |
|:--|:--|:--|:---|
| 1 | 1 | Q | !Q |
| 0 | 1 | 1 |  0 |
| 1 | 0 | 0 |  1 |
| 0 | 0 | 1 |  1 | <-- ungueltiger Fall

```
!S --[AND]!-- Q
     \____/
	 /    \
!R --[AND]!-- !Q
```

#### Hazards

Hazards sind in einem Schaltnetz momentan auftretende, laut Schaltfunktion
ungueltige Ausgangswerte beim Uebergang zwischen zwei gueltigen
Schaltfunktioneingaengen, aufgrund von Verzoegerungen von Signalen in
Leitungen. Hazards treten also auf, wenn eine Schaltfunktion aufgrund von
physikalischen Effekten wie der endlichen Geschwindigkeit bzw. Ausbreitung von
Signalen durch Leitungen und Schaltglieder, ungueltige Zustaende annehmen.

Dies passiert oftmals, wenn man zwischen Bitmustern wechselt, dessen
Hamming-Abstand groesser eins ist. Beispiel: Moechte man bei einem RS-Latch beliebiger Art von $RS$ zu $\overline{R}\overline{S}$ wechseln, kann es
sein, dass beim Uebergang zwischen diesen Eingangsworten momentan die Eingaenge
auf $R\overline{S}$ (oder umgekehrt) gestellt sind. Wenn sich der Ausgang dann
noch nicht umgestellt hat (fuer einen sehr kleinen Zeitraum), ist die
Schaltfunktion moeglicherweise in einem ungueltigen Zustand ($01$ sollte $Q = 0$
ergeben, aber wenn vorher $Q = 1$ war und $Q$ sich noch nicht umgestellt hat,
ist momentan bei $01$ $Q = 1$, was ungueltig ist).

Gruende fuer Hazards:
* Temperatur
* Wert der Eingangssignale
* Leitungsmaterialien
* Schaltnetzmaterializen

#### Getaktete Latches

Bisher wurden die Signale von $R$ und $S$ immer direkt in das Latch gegegen,
ohne jegliche Verzoegerung. Indem man den $R$ bzw. $S$ Eingang vorher jeweils
noch durch ein AND schleusst, das mit einem $CLK$ (clock) Signal verbunden ist,
kann man erreichen, dass die Steuersignale nur bei einem gewissen Pegel
uebernommen werden (HIGH oder LOW, meistens HIGH).

```
!S --[AND]--[AND]!-- Q
      |     \     /
CLK---|		 -----
	  |	    /     \
!R --[AND]--[AND]!-- !Q
```

Durch Taktung kann man erreichen, dass Hazards zwar __nicht ganz weggehen__,
aber abklingen, da Signale einen halben Takt Zeit haben, sich zu veraendern
(sofern der Takt so gewaehlt ist dass alle Signale mehr Zeit haben ihren
gueltigen neuen Wert anzunehmen). Zum Anderen kann man die Speicherung mit
anderen Teilen der Maschine synchronisieren.

### D-Latch/Flip-Flop

Ein D-Latch/Flip-Flop ist ein Latch/Flip-Flop mit nur einem Datenzugang D, also
keinen separaten Signal und Reset Eingang S bzw. R. Das Reset wird einfach aus
der Negation des Signals generiert, es gibt also noch ein NOT-Gatter zwischen
dem "Quasi-R" und dem CLK/Control AND-Gatter. Alle Gatter in diesem
D-Latch/Flip-Flop sind NAND Gatter, und die Ausgaenge sind Q und nicht Q.

Hierbei gibt es natuerlich wieder die unterscheidung zwischen Latch und
Flip-Flop. Ein D-Latch ist pegelgesteuert, ein D-FF ist flankengesteuert und
wird z.B. als D-Master-Slave-FF implementiert, bestehend aus zwei aneinander
geketteten D-Latches.

Einen D-Type Latch/FF nennt man __"transparent"__, denn seine Wertetabelle ist
einfach: Wenn CLOCK = 1, dann ist Q gleich dem Datum (also dem Signal an Eingang
D), wenn CLOCK = 0, dann ist der Wert gespeichert. Also Daten werden
reingelassen wenn die CLOCK HIGH ist, wobei das Reset eben automatisch gesteuert
wird, und Daten werden gespeichert wenn die CLOCK LOW ist.

Die Ausgaenge von den CLOCK-R/S NAND Gattern, die in die NAND Gatter vor Q/nicht
Q sind bleiben bei LOW CLOCK immer HIGH, sind also gegenueber den vorherigen
Daten die bei Q und nicht Q gelegen sind neutral.

https://www.youtube.com/watch?v=5ykewHgHYBI

```
D-----------[AND]!--[AND]!---Q
|            |     \     /
|      CLK---|	    -----
|            |	   /     \
\----[ ]!--[AND]!--[AND]!-- !Q
      ^
	 NOT
```

### Master-Slave Flip-Flop

Bei regulaeren Flip-Flops gibt es das Problem der sogenannten *Glitches*. Ein
Glitch passiert dann, wenn das control-signal (CLOCK) HIGH geht, kurz bevor sich
der Dateneingang (D oder S) aendert. Das hat die Folge, dass am Ausgang des
Flip-Flops kurzzeitig der eine Pegel von D ankommt und kurz danach der andere
Pegel, also eine Schwankung, die unerwuenschst ist. Hierzu gibt es die
Moeglichkeit von Master-Slave Flip-Flops. Ein solcher Aufbau ist einfach zwei
normale D oder R/S Flip-Flops hintereinander verkettet. Dabei ist der
Datenausgang vom einen Flip-Flop -- dem *Master* -- mit dem Dateneingang D vom
anderen Flip-Flop -- dem *Slave* -- verbunden. Das wichtigste aber ist, dass das
CLOCK-Signal vom Master auch in das CLOCK-Signal vom Slave eingespeist wird,
__aber negiert__. Das hat zur Folge, dass wenn das CLOCK-Signal HIGH ist, der
Master "offen" ist, also Daten annimmt, waehrend der Slave "gesperrt" ist, also
keine Daten annimmt (das CLOCK-Signal ist beim Slave naehmlich LOW). Dann, nach
einem halben Taktzyklus geht die CLOCK LOW beim Master und HIGH beim Slave. Dann
koennen die Daten die waehrenddessen beim Master "akkumuliert" wurden (sie
hatten ja einen halben Takt Zeit die Flanke zu wechseln) zum Slave wandern,
einen halben Takt lang. Da die Daten einen halben Takt Zeit haben beim Master
anzukommen bevor sie wirklich erst "interpretiert" werden im Slave, gibt es
keine Glitches. Das einzige was passieren kann, ist das ein Edge der Daten genau
ein bisschen nachdem beim Master die falling-edge von der Clock angekommen ist,
eintreffen, dann gibt es eine halben Takt lang falsche bzw. "outdated" Daten.

Da bei einem Flip-Flop sowohl $Q$ als auch $\overline{Q}$ am Ausgang anstehen,
kann man die Ausgangssignale vom Master gleich in die NANDs mit dem invertierten
CLOCK-Signal geben, da man ja nicht mehr eine Invertierung wie bei D braucht.

Durch das ganze wird der FF von einem Level-Triggered FF zu einem edge-triggered
FF, da die Signale ja nur bei Flanken vom einen FF zum naechsten (also Master ->
Slave oder Slave -> naechster Master) wandern duerfen.

Daten werden also immer nur bei fallender Flanke uebernommen! (der Slave zaehlt
ja, weil sein Ausgang ist der Ausgang des ganzen MS-FF)

Auch hat ein MS-FF zur Folge, dass Schaltwerke synchronisiert sind, also immer
nur zur steigenden Flanke Daten einlesen, und immer nur zur fallenden Flanke
Daten ausgeben -- und waehrenddessen keine neuen Daten einnehmen.

Moegliche Anwendung von MS-FFs: Shift-registers/Schieberegister. Verkettet man
$N$ MS-FFs, enthaelt vom Eingangssignal aus der $i$-te MS-FF das Signal um $i$
Takte verzoegert, wobei $i = 0$ fuer den ganz ersten FF.

### JK-Flip-Flop

Der JK-FF ist eine Moeglichkeit, das vierte Eingangswort $R = S = 1$ sinnvoll zu
nutzen: zum togglen. $J$ steht fuer "Jump" und $K$ fuer "Kill". Dabei hat man
noch bevor die Signale $R$ und $S$ an die NANDs/NORs oder an die ANDs mit der
CLK kommen zwei weitere AND-Gatter. In ein so ein AND-Gatter kommt also das $J$
Signal, und als zweiter Eingang die Rueckkopplung von $\neg Q$. Der Punkt ist,
dass $J = \text{ "Jump"}$ nur durchgelassen wird, wenn $\neg Q$ gilt, also dass
man nur SET-en kann wenn der gespeicherte Wert auch $0$/LOW ist -- sonst bleibt
der gespeicherte Wert sowieso $1$. Ein selbes AND-Gatter gibt es dann auch fuer
das Kill-Signal $K$ und der Rueckkopplung von $Q$, sodass man nur KILL-en kann,
wenn $K = 1$ und $Q = 1$ -- ansonsten bleibt $Q$ sowieso 0. Also wenn nur SET,
dann immer $1$ und wenn nur KILL, dann immer $0$, auch wenn die Signale nicht
durchkommen.

Da in einem anfangs stabilen System nur entweder $Q$ oder $\neg Q$ $1$ sind,
koennen somit niemals gleichzeitig KILL *und* JUMP Signale durch die ANDs
gelangen. Was aber passiert wenn $K = J = 1$ ist, dass wenn $Q = 1$, KILL
durchgelassen wird und wenn $\neg Q = 1$, JUMP durchgelassen wird. Dann wird der
Zustand also getogglet (was auch nicht immer gut ist => race-around).

| J | K | Q  | !Q |
|:--|:--|:---|:---|
| 0 | 0 | Q  | !Q |
| 1 | 0 | 1  | 0  |
| 0 | 1 | 0  | 1  |
| 1 | 1 | !Q | Q  |

JK-FFs haben folgende Gleichung: $Q_{next} = J\overline{Q} + \overline{K}Q$. Der Zustand $Q_{next}$ ist also $1$, wenn ein Jump-Signal kommt und der Zustand $Q$ momentan $0$ ist, oder wenn kein Kill Signal kommt und der Zustand momentan $1$ ist.

JK-FFs koennen nur mit NORs implementiert werden.

Ein Problem bei JK-FFs ist der sogenannte *Race-Around* Zustand. Wenn $J = K =
CLK = 1$, togglet der Wert kontinuierlich -- er ist also sehr instabil. Mit
einem MS-FF kann man dies umgehen, da der Wert dann immer nur zur fallenden
Flanke des Takes uebernommen wird, und das Signal somit immer stabil ist (keine
Glitches).

https://www.quora.com/Why-should-we-use-master-slave-flip-flops
