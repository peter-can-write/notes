# Floating-Point Zahlen

## Festkomma

Bei Festkommazahlen ist die Anzahl an Vorkomma- bzw. Nachkommabits konstant,
z.B. hat eine Zahl im Format $8.8$ immer konstant 8 Vorkommabits und 8
Nachkommabits. Mit einer solchen Zahl haette man z.B. einen Wertebereich von
$2^0 - 2^8 - 1 = 0 - 255$ im Vorkommabereich (8 Bits) und $\frac{1}{2^8} -
\frac{2^8 - 1}{2^8} = \frac{0}{256} - \frac{255}{256}$ im Nachkommabereich.

### Umwandlung

Sowohl der Vorkomma als auch der Nachkommabereich koennen direkt aus dem
Dezimalsystem in das Binaersystem, und umgekehrt, umgewandelt werden. Beispiel:

Format: $4.3$

$12.375_{10} = 1100.011_2$

Um den Nachkommabereich umzuwandeln, kontinuierlich kleinere negative
Zweier-Potenzen abziehen: $0.375 = 0 \cdot 2^{-1} + 1 \cdot 2^{-2} + 1 \cdot
2^{-3}$

### Addition

Bei der Addition muss man auf keine besonderen Regeln achten. Addiert man zwei
Festkommazahlen, kann im Nachkommabereich ein Ubertrag passieren, aber dieser
geht direkt als Ubertrag in den Vorkommabereich ueber (wenn der Nachkommawert
$\geq 1$ wird). Da bei einer Addition von zwei binaeren Zahlen mit $n$ bzw. $m$
Bits maximal ein Bit mehr als $\max(a, b)$ entstehen kann, muss man notfalls den
MSB ignorieren. Daher immer die Position fuer das Komma von rechts zaehlen.

Sei das Format $8.8$

$a = 123.78125_{10} = 01111011.11001000_2$
$b = 5.125_{10} = 00000101.00100000_2$

$a + b = 128.90625$

```
01111011.11001000
                  +
00000101.00100000
-----------------
10000000.11101000
```

$10000000.11101000_2 = 128.90625$

### Subtraktion

Selbes gilt fuer Subtraktion, eben gleich wie bei binaeren Zahlen ohne Komma.

### Multiplikation

Bei der Multiplikation wandelt man wieder Vorkomma und Nachkommabereiche beider
Zahlen in Binaer um und "vergisst" den Punkt einfach. Man fuehrt die
Multiplikation also einfach wie mit normalen binaeren Zahlen aus. Am Ende muss
man den Festkommapunkt an die Stelle $a + b$ setzten wo $a$ die Nachkommastellen
von der ersten Zahl sind und $b$ jene von der zweiten, also z.B. an
die 16. Stelle (von rechts) wenn man zwei Zahlen im Format $8.8$ multipliziert.

Um zu rechnen kann man die Festkommazahl also einfach um $8$ Bits nach links
schieben, somit also mit $256$ multiplizieren. Danach muss man die Zahl jedoch
wieder korrigieren.

Format: $7.4$

$123.675_{10} \cdot 7.1875_{10}$

$123.675_{10} = 1100111.1010_2$
$7.1875_{10} =  0000111.0011_2$

```
        11001111010
    	     +
        00001110011
-------------------
        11001111010
       110011110100
    110011110100000
   1100111101000000
  11001111010000000
===================
1011101000.11001110
```

Komma an die $4 + 4 = 8$-te Stelle.

Frage: Wie multipliziert man mit $\pi$ im $8.8$ Format?  Antwort: Per Hand: $\pi
\cdot 2^8$ rechnen, dann normale Multiplikation, dann wie oben das Komma wieder
verschieben. Beim Programmieren passiert dieses "nach links schieben" einfach
dadurch, dass man das Komma vergisst.

## Gleitkomma

Eine Gleitkommazahl $W$ hat generell die folgende Form:

$W = s \cdot \frac{M}{D} \cdot B^{e - b}$, mit

$s = \text{Vorzeichen (Sign)} \in \{-1, +1\}$,
$M = \text{Mantisse} \in \mathbb{N_0}$
$D = \text{Divisor} \in \mathbb{N}$
$B = \text{Basis} \in \mathbb{N}\setminus{\{1\}}$
$e = \text{Exponent} \in \mathbb{N_0}$
$b = \text{Verschiebung (Bias)} \in \mathbb{N_0}$

$\frac{M}{D}$ ist Zusammen der "Signifikant".

### Single-Precision

Der IEEE (international association of electrical and electronic engineers) legt
den IEEE-Standard 754 fuer einen 32-Bit *single-precision* Gleitkommadatentyp
fest mit:

$B = 2$
$M = 24 \text{ Bit}$
$D = 2^{23}$
$e = 8 \text{ Bit}$
$b = 127$

#### Eigenschaften

* Die Basis ist $2$, damit man damit schnell in einem Computer rechnen kann.
* Jedoch kann man mit (negativen) Zweier-Potenzen nicht alle Werte darstellen,
  z.B. $0.1$ kann nur als $0.100000001$ dargestellt werden.
* Es gibt auch Basis-$10$ ("decimal") Floating-Point Zahlen, mit welchen man
  alle Zahlen *exakt* darstellen kann. Diese sind jedoch langsamer. Im
  Finanzbereich aber eben oftmals erwuenscht.

* Der Bias im Exponenten bewirkt, dass man keinen Sign-Bit fuer den Exponenten
  braucht.
* Mit den 8-Bit ist der Exponent somit einfach nicht mehr im Intervall
  $[0, 2^8 - 1] = [0, 255]$, sondern $[-2^7 + 1, +2^7] = [-127, +128]$
* Dies bewirkt auch, dass Floating-Point Zahlen korrekt sortiert und verglichen
  werden koennen, sogar wenn sie als integer interpretiert werden.
* Manche Werte des Exponenten sind jedoch reserviert.
	* Sind im Exponenten alle Bits $1$ und:
		* Im Signifikant alle Bits $0$, steht die Zahl fuer plus/minus
          unendlich. Unendlich ist das Resultat einer Zahl, die:
			* die Range des Exponenten ueberschreitet (passiert automatisch wenn
              sich der Exponent dann um 1 erhoeht)
			* durch Division mit 0 resultiert
		* Im Signifikant __nicht__ alle Bits $0$, steht die Zahl fuer `NaN`, das
          Resultat von:
			* Einer Multiplikation von $0$ mit Unendlich
			* Jeglicher Rechenoperation mit Unendlich
			* Anwendungsspezifische Werte (kann das Programm festlegen?)
		* Vergleiche mit `NaN` muessen immer falsch sein, sogar bit-gleiche
          Vergleiche von `NaN` mit `NaN`
	* Sind im Exponenten alle Bits $0$, koennen damit bestimmte "Subnormale"
      Zahlen dargestellt werden (besonders kleine).
* Daher verlfauft $e$ fuer Zahlen wirklich zwischen $[1 - 127, 254 - 127] =
  [-126, +127]$ ($0$ und $255$ sind reserviert)

* Der MSB der Mantisse ist immer 1. Das nennt man *normalisiert*.
* Der Sinn ist, dass man den Signifikant immer zwischen $1$ und $2$ halten kann,
da man jede Zahl zwischen $0$ und $1$ mit einem (um $1$) niedrigeren Exponenten
darstellen kann.  Wichtig hierbei: man arbeitet mit 2-er Potenzen. Wie stellt
man also $0.5$ dar? Eben $1.0 \cdot 2^{-1}$. Wie $5$? $1.25 \cdot 2^2$
* Die Zahl bleibt zwischen $1$ und $2$, weil fuer den Signifikant 24-Bit
  festgelegt sind. Ist der 23. Bit (von 0, links weg) also immer 1, ist die
  Range nicht mehr $[0, 2^{24} - 1]$, sondern $[2^23, 2^24 - 1]$.

* Da die Mantisse normalisiert ist, der $24.$ Bit also immer $1$ ist, kann er
  auch weggelassen werden.
* Es ist nun in der 32-Bit Zahl Platz fuer alles:
	* 1-Bit: Sign
	* 23-Bit: Signifikant
	* 8-Bit: Exponent
* Wobei zuerst der Sign-Bit, dann der Exponent und dann der Signifikant
  gespeichert werden.

* $+0$ und $-0$ sind beide verschieden als Floating-Point Zahlen darstellbar,
  muessen jedoch laut Standard immer zueinander gleich sein.

* Werte:
	* Groesster: $M = 2^{24} - 1, e = 254 - 127 = 127 \Rightarrow +1 \cdot
      \frac{2^{24} - 1}{2^23} \cdot 2^{127} = +1 \frac{16777215}{8388608} \cdot
      2^{127} = 3.402823466 \cdot 10^{38}$
	* Kleinster: $-1 \cdot \frac{2^{24} - 1}{2^23} \cdot 2^{127} = -1
      \frac{16777215}{8388608} \cdot 2^{127} = -3.402823466 \cdot 10^{38}$$
	* Kleinste Schrittweite: $\frac{2^{23}}{2^{23}} \cdot 2^{1 - 127} =
	 \frac{2^{23}}{2^{23}} \cdot 2^{-126}$

### Addition

Zur Addition zweier Fliesskommazahlen muessen zunaechst die Exponenten beider
Zah- len angepasst werden. Das geht, in dem die absolut kleinere Zahl unter
Beibehaltung des Wert denormalisiert wird. Das heisst, die Mantisse wird um n
Stellen nach rechts geschoben und der Exponent um n erhoeht. Dabei wird n so
gewaehlt, dass der neue Exponent dem der groesseren Zahl
entspricht. Anschließend werden beide Mantissen addiert und das Ergebnis
normalisiert.  Bei Addition kann durch die Anpassung des Exponenten der
Mantissen-Anteil in der Genauigkeit stark reduziert oder gar ausgeloescht
werden.

Aufwendig!

### Multiplikation

Die Multiplikation zweier Fließkommazahlen multipliziert die Mantissen und
addiert die Exponenten, eine Anpassung der Exponenten ist nicht
notwendig. Anschließend wird das Ergebnis noch normalisiert.

Schnell!

### Double-Precision

$B = 2$
$M = 53 \text{ Bit}$
$D = 2^{23}$
$e = 11 \text{ Bit}$
$b = 1023$

$e \text{ fuer nicht-reservierte Werte } \in [1 - 1023, 2^{11} - 1023] =
[-1022, +1023]$

| Festkomma                 | Gleitkomma            |
| :------------------------ | :-------------------- |
| Einfache Grundrechenarten | Aufwendige Addition   |
| Konstante Aufloesung      | Dynamische Aufloesung |
| Kein Rundungsfehler       | Rundungsfehler        |
| Geringe Dynamik           | Grosser Wertebereich  |
| Kein Universalformat      | Universell anwendbar  |
