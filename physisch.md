# Physikalische Schicht

Auf der Bauelemente-Schicht behandeln wir Transistoren und andere elektronische
Bauelemente. Auf der physikalischen Schicht behandeln wir, wie diese Bauelemente
physikalisch hergestellt werden.

Auf der Bauelemente-Schicht werden $0V$ meist als LOW und $~ 5V$ als HIGH
definiert. Die Stromstaerke kann aber als beliebiger positiver Wert gewaehlt
werden.

## Halbleiter

Halbleiter bestehen auf der untersten Schicht aus Silizium Atomen ($Si$), die
man gitterfoermig auf einem *Waver* als *Kristall* anordnen kann. Jedes Silizum
Atom enthaelt dabei vier Elektronen. Bei sehr tiefen Temperaturen kann Silizum
keinen Strom leiten. Steigt jedoch die Temperatur, geraten die Atome in
Schwingung und Elektronen koennen sich von den Atomen loesen und zu einem Strom
fuehren.

Loest sich ein Elektron von einem Silizium Atom, hinterlaesst es ein positiv
geladenes *Loch*. Da dieses Loesen jedoch nicht schnell genug passiert,
*verunreinigt* man den Silizium Kristall mit anderen Atomen, z.B. Arsen oder
Phosphor. Diese Verunreinigung nennt man *Dotierung*. Diese anderen Atome
enthalten fuenf Elektronen, von welchen vier zur Bindung im Gitter benoetigt
werden, das letzte sich aber leicht loest. Wenn es sich loest, hinterlaesst es
ein positiv geladenes Arsen-/Phosphor-Ion.

Wenn sich ein Elektron von einem Atom loest, und in ein vorher entstandenes Loch
eines anderen Atoms faellt, nennt man das *Rekombination*.

Enthaelt ein Halbleiter-Kristall durch Verunreinigung (*Dotierung*) einen
Ueberschuss an negativen Teilchen, nennt man ihn *n-dotiert*. Der Loecherstrom
ist dann vernachlaessigbar, also sagt man, es gibt nur den
Elektronenstrom. Daher n-dotiert = Elektronenueberschuss. Einen n-dotierten
Halbleiter nennt man *n-Halbleiter*.

Verunreinigt man einen Kristall mit Indium oder Bor, dessen Atome nur drei
Elektronen enthalten, muss das Indium/Bor von den Silizium Atomen Elektronen
stehlen. Dabei entstehen mehr Loecher. Setzt man einen solchen Silizium-Indium
Kristall unter Spannung, wandern freie Elektronen zum Plus-Pol. Wenn sich ein
Elektron loest und zum Plus-Pol wandert, kann man sagen, dass die Loecher
gleichzeitig zum Minuspol wandern. Physiker haben sich geeinigt, dass die
Loecher wandern, und nicht die Elektronen. Also spricht man hier von einem
Loecherstrom.

Weil die Loecher positiv sind, nennt man einen Halbleiter mit Loecherstrom
*p-dotiert*. Einen p-dotierten Hableiter nennt man *p-Hableiter*. p-Halbleiter
haben einen Elektronenmangel.

## Transistoren

Mit Halbleitern kann man Dioden realisieren, welche Strom nur in eine Richtung
fliessen lassen. Mit diesen kann man wiederum Transistoren entwickeln, welche
Strom zwischen zwei Kanaelen nur fliessen lassen, wenn ein Eingangsstrom da ist,
oder nicht. Sie sind also perfekt als *Schalter* geeignet. Hierbei gibt es noch
eine Unterscheidung zwischen Bipolar-Junction-Transistoren (BJT) und
Field-Effekt-Transistoren (FET). BJTs sind Strom-gesteuert (current), FETs sind
Spannung-gesteuert (voltage). FETs werden in digitalen Schaltkreisen eher
verwendet. Im Weiteren ist mit "Transistor" die FET-Variante gemeint.

https://www.quora.com/What-are-the-pros-and-cons-of-BJT-versus-FET-transistor

### Aufbau

Transistoren sind aufgebaut durch:

* Einen $D$rain Kanal (Ausgang).
* Einen $S$ource Kanal (Eingang).
* Ein $G$ate, das den Stromfluss zwischen $S$ und $D$ steuert.

Zwischen dem Drain und dem Source fliesst der *Kanal*.

Es gibt hierbei zwei Arten dieser Transistoren:

1. NPN: Welche, die leiten, wenn das Eingangssignal HIGH ($P$) ist.
2. PNP: Welche, die leiten, wenn das Eingangssignal LOW ($N$) ist.

Typ (1) nennt man auch Arbeitskontakt-Typ, Typ (2) auch Ruhekontakt-Typ.

Strom fliesst also von $S$ (Source) nach $D$ (Drain), wenn Strom am $G$ (Gate)
anliegt (NPN), oder nicht (PNP). Der Strom am Gate ist dabei nicht binaer,
sondern steigt von 0 zu *einer als HIGH definierten Stromstaerke* (meist $5V$).

### Herstellung

Zur Herstellung von Transistoren wird heutzutage meist eine Mischung aus
Metall-Oxid und Silizium verwendet. Oftmals kommt noch Carbon (Kohlenstoff)
hinzu.

MOS = metal-oxide-semiconductor
CMOS = complementary-metal-oxide-semiconductor

Hierfuer werden Silizium-Kristalle mittels dem Czochralski-Prozess
gezuechtet. Dann werden auf *Wavern* (Siliziumplatten) viele Transistoren
gebaut.

### Wirkung

Durch positive Spannung am Gate werden Elektronen aus dem Silizium-Substrat an
deren Oberfaleche gezogen. Zwischen Source und Drain ensteht dadurch eine
schmale leitende Schicht, durch welchen Strom fliessen kann -- der "Schalter"
ist also offen.

Wird die Spannung am Gate wieder entfernt, verschwindet der leitfaehige Kanal
und der Transistor wird wieder nichtleitend -- der "Schalter" ist also
geschlossen.

## Integrierte Schaltkreise

Integrierte Schaltkreise ("integrated circuits"; IC) sind Bausteine mit bis zu
$10^{10}$ Transistoren. Sie sind aufgebaut auf Siliziumplaettchen von circa
$1\times1\, cm^2$.
