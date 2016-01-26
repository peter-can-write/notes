# Zeichensaetze

## ASCII

ASCII (American Standard Code for Information Interchange) ist eine der
aeltesten und einfachsten Zeichensaetze fuer Computer. Jedes der 128 Zeichen
verbraucht genau einen Byte. Die Zeichen selbst belegen dabei die Werte $0$ bis
$127$. Hierbei steht beispielsweise Code $0$ fuer den Null-Terminator. $65$ bis
(einschliesslich) $90$ sind die Grossbuchstaben $A$ bis $Z$. Zeichen $0$ bis
$31$ sind nicht druckbare *Control Codes*.

Da die Werte nur von $0$ bis $127$ gehen, wird der obere Bit bzw. die halben
Werte des moeglichen Raumes in einem Bit gar nicht verwendet. Es entstanden dann
im Laufe der Geschichte viele Erweiterungen von ASCII, die allesamt die oberen
128 Werte anders kodierten, z.B. fuer Umlaute im Deutschen oder fuer kyrillsche
Buchstaben im slawischen Raum. Fuer den westeuropaeischen Raum ist
z.B. *Latin-1*, offiziell *ISO-8859-1*, eine beliebte Erweiterung.

## Unicode

Unicode ist eine viel komplexere und fortgeschrittenere Kodierung von Zeichen im
Vergleich zu ASCII. Unicode representiert dabei Zeichen also sogenannte
*Code-Points* in einer 16-Bit/2-Byte Darstellung. Die Werte $0$ bis $255$
entsprechen dabei Latin-1. Da es aber weit mehr als $2^{16}$ Zeichen gibt
(z.B. fuer asiatische Sprachen, auch tote Sprachen), muessen Unicode Zeichen oft
mit zwei Woertern, also insgesamt 32-Bit/4-Byte (Low/High Surrogate) kodiert und
gespeichert werden.

Der Nachteil einer reinen Unicode Kodierung ist dass man sogar fuer ASCII zwei
Bytes, also mehr Speicher, braucht. Damit hat man auch Kompatibilitaetsprobleme
mit einer ASCII Kodierung.

## UTF

UTF (Unicode Transformation Format) ist eine Erweiterung von Unicode auf 17
Ebenen zu je $2^{16} = 65535$ verschiedenen Zeichen, also abzueglich interner
Steuersymbole insgesamt $1 111 998$ verschiedene Symbole. Fuer diese Symbole
gibt es nun mehrere moegliche Kodierungen:

7-Bit:  UTF-7
8-Bit:  UTF-8
16-Bit: UTF-16
32-Bit: UTF-32

UTF-8 ist hierbei eines der beliebtesten Formate. Es speichert Unicode Punkte
mit variabler Bytezahl ab. ASCII Werte werden dabei mit nur einem einzigen Byte
gespeichert, alle weiteren mit $2$ bis $4$ Bytes.

Dies wird so kodiert, dass wenn der erste Bit in einem Unicode Wort $0$ ist,
dieses Unicode Wort als $1$ Byte lang gilt und nur ASCII Werte enthaelt. Fuer
alle Werte ausserhalb von ASCII ist der erste Bit im ersten Byte eines Unicode
Wortes gesetzt. Danach folgen $n$ weitere gesetzte Bits, die aussagen, dass $n$
weitere Bytes folgen. Die Folge von $n$ Einsen wird durch eine $0$
terminiert. Da UTF-8 maximal $4$ Byte lang ist, koennen nach dem ASCII-Bit also
insgesamt maximal $3$ weitere Bits gesetzt sein (einschliesslich dem ersten Byte
und den $3$ folgenden also insgesamt $4$). Jeder weitere Byte des UTF-8 Worts
beginnt dann mit einer $10$ Kodierung in den oberen Bits. Man hat also $21$ Bit
fuer die eigenliche Kodierung des Zeichens.


0xxxxxxxx = 0 -> ASCII
110xxxxxx 10xxxxxx = 1 -> !ASCII, 1 -> 1 weiterer Byte, 0 -> Terminierung
1110xxxxx 10xxxxxx 10xxxxxx = 2 bits gesetzt im ersten Byte -> 2 folgende bytes.
11110xxx  10xxxxxx 10xxxxxx 10xxxxxx


Da der ASCII-Bereich in UTF-8 mit nur einem Byte gespeichert wird und der
oberste Bit fuer reines ASCII keine Bedeutung hat, kann UTF-8 kodierter Text mit
nur ASCII Zeichen problemlos mit einer ASCII Kodierung geoeffnet werden. Sind in
diesem UTF-8 Text aber weitere, nicht ASCII Zeichen, werden diese als
merkwuerdige ASCII Zeichen angezeigt. Meistens oeffnet man einen Text in
Latin-1, dann kommen statt Unicode Zeichen Äs oder sonstige unerwuenschte
Zeichen.

UTF-16 speichert Werte immer mit mindestens 2, maximal 4 Bytes.

### Kodieren von Zeichen

Um Zeichen in Unicode zu kodieren, muss man erst ihren Unicode Punkt finden. Ist
dieser nicht mehr in ASCII, werden mehrere Bytes benoetigt. Dann werden die
einzelnen Bytes von rechts nach links sequentiell (die Unicode Kodierungen
beachtend) mit dem Binaerwerts des Unicode-Punkts aufgefuellt.

Ä = 0xc4 = 0b1100.0100

110(00011) 10(000100) = 0xC3 0xA4
^^          ^
||          naechster Byte
|ein folgender Bit
nicht ascii

∂ = 8706_10 = 0x2202 = 0b0010.001000.000010

1110(0010) 10(001000) 10(000010) = 0xE2 0x88 0x82
