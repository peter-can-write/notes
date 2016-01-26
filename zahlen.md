# Zahlensysteme

## Binaersystem

Das Binaersystem hat nur zwei Ziffern: $0$ und $1$. Von $0$ weg hat der $n$-te
Bit also den Wert $2^n$.

### Umrechnung Dezimal => Binaer

Um eine Dezimalzahl in das Binaersystem zu ueberfuehren, sucht man sich
kontinuerlich die groesste Zweierpotenz, die noch in die Zahl passt. Man setzt
dann fuer diese Zahl $2^n$ den $n$-ten Bit im resultierenden Binaerwort. Dann
subtrahiert man diese Zahl von der Dezimalzahl und sucht sich erneut die
groesste Zweierpotenz etc. Dies geht solange, bis die Dezimalzahl $0$ ist.

Beispiel: $123_{10}$

In $123_{10}$ passt nur $64_{10} = 2^6$ als groesste Binaerzahl. Also setzt man
den sechsten Bit (von $0$ gezaehlt), und subtrahiert $64_{10}$ von $123_{10}$:
$123 - 64 = 59$. Die weiteren passenden Potenzen sind dann $32, 16, 8, 2,
1$. Daher:

$123_{10} = 0111 1011_2$

Beispiel: $123 456 789_{10}$

Wenn die Zweierpotenzen zu gross werden: auf bekannte "Anker"-Zweierpotenzen
zurueckfuehren, und diese dann multiplizieren. Z.B.: Hier ist $2^{26}$ die erste
Zweierpotenz. $2^{26} = 2^{16} \cdot 2^{10} = 65636 \cdot 1024 = 65536000 + 2
\cdot 655360 + 4 \cdot 65536$. Fuer Dezimalmultiplikation: zuerst
multiplizieren, dann Rest addieren.

Binaer: $1001001100101100000001011010010$

### Umrechnung Binaer => Dezimal

Um von Binaer auf Dezimal umzurechnen, addiert man einfach sukzessive die
vorkommenden Zweierpotenzen.

Beispiel: $1010_2$

Hier ist der dritte Bit (von $0$ weg) gesetzt, also zaehlen wir $2^3 = 8$ und
dann $2^1 = 2$, also $8 + 2 = 10$.

Beispiel: $0101 1101 1111 0111 1001 0011$

Potenzen: $2^0 + 2^1 + 2^4 + 2^7 + 2^9 + 2^{10} + 2^{12} + 2^{13} + 2^{14} +
2^{15} + 2^{16} + 2^{18} + 2^{19} + 2^{20} + 2^{22} = 6157971_{10}$

### Addition

Binaere Zahlen addiert man gleich wie Dezimale. Man schreibt sie uebereinander
und wenn die Summe zweier Zahlen die groesste Ziffer (z.B. $10$ in Dezimal)
uebersteigt, nimmt man einen Rest mit. Hier ist der Rest eben hoechstens $1$,
genau wenn man $1$ und $1$ addiert. Generell die Regeln:

1. $0 + 0 = 0$
2. $0 + 1 = 1 + 0 = 1$
3. $1 + 1 = 0, \text{ Carry} = 1$

__ Bei der Addition wird das Resultat maximal um einen Bit groesser als der
groesste Summand.__

Beispiel: $123_{10} + 456_{10} = 01111011_2 + 111001000_2$

```
 001111011 +
 111001000
----------
1001000011
```

$1001000011_2 = 512 + 64 + 2 + 1 = 579_{10}$

### Multiplikation

Eine Multiplikation ist im allgemeinen mathematischen Sinne eine Folge von
Additionen. Sind $a, b$ zwei Dezimahlzahlen, so ist $a \cdot b$ aequivalent
dazu, $a$ mit jeder Ziffer von $b$ und ihrer Ordnungsgrosse einzeln zu
multiplizieren, und die Resultate jeweils zu addieren:

$25 \cdot 13 = 25 \cdot 10 + 25 \cdot 3 = 325$

Gleiches wird also auch im Binaersystem gemacht. Um $a_2 \cdot b_2$ zu
multiplizieren, multipliziert man $a_2$ mit jedem Bit von $b_2$ einzeln. Hierbei
ist es wichtig anzumerken, dass eine Multiplikation mit einem Bit bzw. allgemein
mit einer Zweierpotenz aequivaelent zu einem Shift von $k$ Bits nach links ist,
wo $k$ die Position des Bits im Multiplikator ist.

__ Bei der Multiplikation einer $n$-stelligen Binaerzahl mit einer $m$-stelligen
kann das Resultat maximal $m + n$ viele Bits haben. __ Fuer *signed* Werte sind
es $(m - 1) + (n - 1)$, da der Sign-Bit nicht zum Wert zaehlt.

$25_{10} \cdot 13_{10} = 0001 1001_2 \cdot 0000 1101_2$

```
 00011001
 00001101 *
---------
 00011001 (Shift um 0)
 01100100 (Shift um 2)
 11001000 (Shift um 4)
--------- +
101000101
```

$101000101_2 = 325_{10}$

### Subtraktion

Subtraktion mit binaeren Zahlen kann auf zwei Weisen durchgefuehrt werden:

1. Wirkliche Subtraktion, wie im Dezimalsystem. Hierbei gibt es wieder vier
   Regeln:

   1. $0 - 0 = 0$
   2. $1 - 0 = 1$
   3. $1 - 1 = 0$
   4. $10 - 1 = 1$. Dies ist die einzige etwas "anspruchsvollere" Regel. Wenn
      man im Dezimalsystem eine groessere von einer kleineren Zahl abzieht
      (waehrend einer manuellen Subtraktion), zieht man bekanntlich $9$-en nach:
      $1000 - 1 = 0999$. $9$ ist hierbei die groesste Ziffer im
      Dezimalsystem. Somit erhaelt man eine Zahl, die am naechsten zur
      vorherigen war. Das selbe macht man nun auch im Binaersystem, nur ist eben
      $1$ die hoechste Ziffer: $10 - 0 = 01$, $10000 - 1 = 01111$ etc.

$1234_{10} - 567_{10} = 0100 1101 0010_2 - 0010 0011 0111_2$

```
010011010010
001000110111 -
------------
001010011011
```

$001010011011_2 = 667_{10}$

2. Die wahrscheinlich leichtere Variante ist es, das Zweierkomplement zu bilden
   (Bits negieren und $1$ addieren) und dann eine ganz normale Addition
   bilden. Also anstatt $a - b$ rechnet man $a + (-b)$. Dann muss man das
   Resultat aber auch als *signed* interpretieren. Der letzte Bit wird hierbei
   ignoriert, falls er ueberlauft (dann ist das Ergebnis positiv).

$1234_{10} - 567_{10} = 1234_{10} + (-567_{10}) = 0100 1101 0010_2 + (~0010 0011
0111_2 + 1) = 0100 1101 0010_2 + 1101 1100 1001_2$

```
010011010010
110111001001 +
------------
001010011011
```

$001010011011_2 = 667_{10}$

### Binaere Erweiterung

Will man eine positive $n$-Bit Zahl auf eine gleiche aber breitere $m$-Bit Zahl
($m > n$) erweitern, ist es leicht: man addiert einfach $m - n$ Nullen. Negative
Zahlen werden erweitert, indem die oberen Bits eins gesetzt werden. Weil
$10000...$ mit nur Nullen ist ja der kleinste negative Wert, und jeder gesetzte
Bit ist sozusagen eine Addition zu diesem negativen Wert. Also wenn man diesen
negativen Wert erweitern will, sollten die hoeheren Bits $1$ gesetzt werden,
weil sie dadurch nicht den Wert veraendern (sie werden zu der entstandenen
groesseren Breite also wieder hinzuaddiert; dadurch der negative Wert wird
kleiner bzw. gleich dem gewuenschten Wert).

## Hexadezimal

Das Hexadezimalsystem benutzt die Ziffern $0-9A-F$, weicht also nach den $10$
Dezimalziffern auf Buchstaben aus. Da $16 = 2^4$ kann man mit einer
Hexadezimalziffer den selben Raum abdecken, wie mit $4$ Binaerziffern. Der
Vorteil davon ist, dass man Binaerzahlen $4$-weise in Hex-Ziffern umwandeln kann
bzw. umgekehrt. Somit ist Hex also sozusagen "Shorthand" fuer Binaerzahlen:

Gegeben die Binaerzahl $0101 1100 1011 1001_2$. Hier kann man nun also alle $4$
Ziffern von rechts mit einer Hexadezimalzahl ersetzen. Man ignoriert dabei
einfach alle andern Bits in der Zahl und betrachtet die $4$-Bits als eine Zahl
zwischen $0$ und $15$, also $0$ und $F$. Diese Zahl ware so also $5CB9_{16}$ im
Hexadezimalsystem.

Oftmals schreibt man Hexadezimalzahlen mit einem `0x` Praefix.

### Umrechnung Dezimal => Hexadezimal

Hierbei kann man die Dezimalzahl entweder zuerst in eine Binaerzahl nach
bekannter Methode verwandeln, und dann ganz einfach je $4$ Bits
komprimieren. Oder man subtrahiert eben den groessten Wert $k \cdot 16^n < x$ wo
$x$ die umzurechnende Zahl und $k$ ein beliebiger Koeffizent.

$123456_{10} = ?_{16}$

1. Binaere Umwandlung: $123456_{10} = 0001 1110 0010 0100 0000_2$

	* $0001_2 = 1_{16}$
	* $1110_2 = E_{16}$
	* $0010_2 = 2_{16}$
	* $0000_2 = 0_{16}$

	Also $123456_{10} = 1E20_{16}$

2. $123456 = 1 \cdot 16^3 + 14 \cdot 16^2 + 2 \cdot 16^1 + 0 = 1E20_{16}$

Wichtige Potenzen:

* $16^0 = 1$
* $16^1 = 16$
* $16^2 = 2^4 \cdot 2^4 256$
* $16^3 = 2^8 \cdot 2^4 = 4096$
* $16^4 = 2^{16} \cdot 2^4 = 1048576$

Generell immer an die Zweierpotenzen denken.

### Grundrechenarten

Die Grundrechenarten folgen dem normalen Schema wie fuer alle anderen
Zahlenbasen. Bei der Addition von zwei Ziffern $a + b$ wird immer der Wert
Modulo $16$ (bzw. Allgemein der Basis) in die naechste Spalte geschrieben, und
der Divisionswert ohne Rest wird mit in die naechste Addition getragen.

Wichtig ist es einfach, nicht zu vergessen, modulo $16$ zu rechnen, und nicht
modulo $10$. Z.b. Wenn man in einer Spalte $A + E$ rechnet und sich im Kopf "$24_{10}$"
denkt, dann ist das Resultat nicht $4$, $2$ Rest sondern $24 \mod 16 = 8$ und
$\lfloor 24/16 \rfloor = 1$ Rest.

Bei der Addition kann wieder maximal eine neue Ziffer hinzukommen.

```
 1E20
 FA3B +
-----
1185B
```

```
AE39C0FB
1CAB5DFF +
--------
Cae51efa
```

```
AE39C0FB
1CAB5DFF -
--------
918E62FC
```


## Oktal

Das Oktalsystem wird mit Ziffern $0$ bis $7$ dargestelt. Da $8 = 2^3$ kann man
mit einer Oktalziffer $3$ binaere Werte kodieren.
