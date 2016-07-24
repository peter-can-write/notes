# OSI Modell: Layer 6

Es gibt viele Moeglichkeiten, Daten darzustellen. Damit Sender und Empfaenger
eines Kommunikationskanals miteinander reden koennen, muessen sie sich jedoch
bezueglich einer Darstellung der Daten einig sein ("dieselbe Sprache
sprechen"). Die *Darstellungsschicht* (Schicht 6) ist nun dafuer zustaendig, den
Kommunikationspartnern einer Verbindung eine einheitliche Interpretation von
Daten zu ermoeglichen. Genauer bietet diese Schicht folgende Dienste an:

* Die *Darstellung* von Daten (*Syntax*),
* Die *Datenstrukturen* zur Uebertragung von Daten (z.B. JSON arrays),
* Die Darstellung der Aktionen an diesen Datenstrukturen (`.append`),
* *Datentransformationen* (Umwandlungen in andere Formate).

Hierbei ist anzumerken, dass die Darstellung auf Schicht 6 (z.B. XML) nicht der
Darstellung auf Schicht 7 entsprechen muss (z.B. eine schoene HTML
Website). Insofern ist die Darstellungsschicht nur fuer die *Syntax* der
Datenformate zustaendig, nicht fuer ihre Interpretation, was der *Semantik*
entsprechen wuerde (was mit den Daten dann angefangen wird, also was mit einem
String oder Array dann z.B. gemacht wird).

## Funktionen

Die Darstellungsschicht hat zwei grundlegende Funktionen:

1. Kodierung von Daten (lower level), insbesondere:
  * Uebersetzung zwischen Zeichensaetzen und Codewoertern gemaess
    standardisierter Kodierungsvorschriften (z.B. Unicode als UTF-8)
  * Kompression von Daten (Entfernung unnoetiger Redundanz, Quellenkodierung!)
  * Verschluesselung (z.B. TLS)

2. Strukturierte Darstellung von Daten (higher level)
 * Plattformunabhaengige, einheitliche Darstellung von Daten
   (z.B. JSON/XML/HTML)
 * Uebersetzung zwischen Datenformaten
 * Serialisierung von Binaerdaten (base64 encoding,
   https://en.wikipedia.org/wiki/Base64); *Marshalling*.

Man sieht hier, dass TLS sowohl zur Sitzungsschicht (Verbindungskoordination
sowie Authentifizierung) als auch zur Darstellungsschicht (Verschluesselung
allgemein) gehoert.

## Zeichensaetze und Kodierung

Daten koennen in einer von zwei Formen vorliegen: als menschenlesbare
Textzeichen, oder als computerlesbare Binaerdaten. Wir wollen diese beiden
Varianten naeher betrachten.

### Menschenlesbare Daten

Bei menschenlesbaren Textdaten und -symbolen beschaeftigen wir uns meist mit
*Zeichensaetzen*. Ein Zeichensatz ist dabei eine Menge darstellbarer Textzeichen
(z.B. Buchstaben, Zahlen, Sonderzeichen) sowie deren Zuordnung zu *Codepoints*
-- eine eindeutige Kennzahl bzw. Identifizierung des Zeichens im Zeichensatz
(Codebuch). Die Codepoints muessen hierbei injektiv, aber nicht notwendigerweise
surjektiv vergeben sein (um Platz fuer neue Zeichen zu lassen).

Fuer jeden Zeichensatz (z.B. *Unicode*) muss es dann zumindest eine
*Kodierungsvorschrift* geben. Eine Kodierungsvorschrift legt fest, wie ein
Codepoint in binaerer Form dargestellt wird. Fuer Unicode waere das z.B. UTF-8
oder UTF-16 oder UTF-32 etc. Es kann also viele Kodierungsvorschriften fuer nur
einen Zeichensatz geben.

```
Zeichen ==Zeichensatz==> Codepoint ==Kodierungsvorschrift==> Codewort
   A                      U+0065                             01000001
```

### Binaere Daten

Binaere Daten bestehen zunaechst aus irgendwelchen atomaren Dateneinheiten,
wobei man eine solche Einheit als *Datum* bezeichnet. Daten sind heutzutage
meist Sequenzen von 8 Bit, welche man als *Oktette* oder sprachgebrauchlich auch
als Bytes bezeichnet. Was ein Datum repraesentiert (die *Semantik* der Daten)
ist vom Kontext bzw. der anwendungsspezifischen Interpretation abhaengig.

Ein Zeichensatz verknuepft also ein Zeichen (Text) mit einem Codepoint. Gaengige
Beispiele fuer Zeichensaetze sind ASCII, Unicode oder ISO-8859-1 (Latin-1). Ein
Zeichensatz kann dabei druckbare Zeichen (Buchstaben, Zahlen, Sonderzeichen)
sowie auch Steuerzeichen enthalten (\n, \t, \a).

Zur Uebertragung von Daten muessen Zeichen, bzw. die entsprechenden Codepoints,
*kodiert* werden. Hierbei unterscheidet man zwischen:

* *Fixed-Length* Codes, bei welchen alle Zeichen mit Codewoertern derselben
  Laenge kodiert werden. Beispiele hierfuer sind ASCII (8 Bit) oder UCS-2 (16
  Bit) oder UTF-32, welches Codepoints stets mit 32 Bit kodiert.
* *Variable-Length* Codes, bei denen Zeichen als Codewoerter unterschiedlicher
  Laenge kodiert werden koennen. Bei UTF-8 sind das beispielsweise 1-4 Byte, bei
  UTF-16 2-4 Byte (16-Bit *code units*).

Es sei angemerkt, dass Zeichensaetze nicht immer die dazugehoerige
Kodierungsvorschrift festlegen. ASCII (7 Bit; MSB 0) oder Latin-1 (8 Bit) legen
sie fest. Unicode legt sie nicht fest (UTF-8,-16,-32 wurden demnach also separat
entwickelt).

#### UTF-8

Das Unicode-Transformation-Format-8 (UTF-8) ist eine Kodierungsvorschrift fuer
Unicode Codepoints. Abhaengig vom jeweiligen Codepoint werden Zeichen mit 1 bis
4 Oktetten kodiert:

| Unicode Bereich    | Laenge |             Kodierung               | Nutzbits |
|:-------------------|:-------|:------------------------------------|:---------|
| U+0000 - U+007F    |   1B   | 0xxxxxxx                            |    7     |
| U+0080 - U+07FF    |   2B   | 110xxxxx 10xxxxxx                   |    11    |
| U+0800 - U+FFFF    |   3B   | 1110xxxx 10xxxxxx 10xxxxxx          |    16    |
| U+10000 - U+1FFFFF |   4B   | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |    21    |

Hierbei gibt die Notation `U+xxxx` an, dass es sich um einen Unicode Codepoint
handelt. Die Zahlen dahinter sind hierbei in hexadezimaler Notation. Die ersten
Codepoints 128 Codepoints von UTF-8 sind genau ASCII, und koennen deswegen mit
nur einem Byte kodiert werden. Hierbei wird der oberste Bit auf 0 gesetzt, wie
es der ASCII Standard verlangt. Man merkt also, dass UTF-8 rueckwaerts
kompatibel zu ASCII ist (d.h. ASCII-kodierter Text kann als UTF-8 interpretiert
werden, aber nicht immer umgekehrt). Das ist bei UTF-16 beispielsweise nicht der
Fall.

Fuer Codepoints, die hoeher als die ersten 128 liegen, gibt dann immer der erste
Byte der mit variabler Laenge kodierten Codewoerter an, wieviele Bytes fuer
dieses konkrete Codewort *insgesamt* verwendet werden. Die Anzahl (aller Bytes,
inklusive dem ersten) wird hierbei unaer kodiert und mit einer Null
terminiert. Somit enthaelt beispielsweise das erste Byte der Codepunkte zwischen
`U+0080` und `U+07FF`, welche mit zwei Bytes kodiert werden, den Praefix
`110`. Jeder weitere Byte eines Codewoertes (bzw. hier *der* weitere Byte)
beginnt dann mit einem `10` Praefix.

Eine wichtige Eigenschaft von UTF-8 ist, dass es *praefixfrei* ist. Das
bedeutet, dass kein Codewort ein Praefix eines anderen Codewortes ist, und somit
als dieses interpretiert werden koennte. Das bedeutet wiederum, dass man jedes
Codewort individuell betrachten kann und keine speziellen Marker
braucht. Insbesondere kann es so nie Verwechslungen zwischen Codewoertern
geben. Ein Beispiel fuer einen praefixfreien Code waere die Menge $\{55,
9\}$. Hierbei koennte man eine beliebige Folge dieser Codewoerter betrachten,
ohne in die Gefahr zu laufen, dass es zu einer Verwechslung kommt. In
`559999559559559` weiss man z.B. immer, ob eine Ziffer ein Codewort ist, oder
nicht. Bei $\{55, 9, 5, 9\}$ ginge das nicht, weil z.B. $5$ sowohl ein Codewort
fuer sich alleine, als auch als ein Praefix fuer $55$ und $59$ betrachtet werden
koennte (eben nicht *praefixfrei*). Aus `555955` kaeme so keine eindeutige
Interpretation.

UTF-8 ist gerade deswegen praefixfrei, weil das erste Byte, das angibt, wieviele
Bytes dieses Codewort insgesamt hat, eine unaere Kodierung verwendet (und weil
es alle Bytes zaehlt). Wuerde es beispielsweise durch einen `10` Praefix
angeben, dass das Codewort aus zwei Bytes besteht, dann wuesste man bei einem
Byte `10xxxxxx` nicht, ob es sich um den Start eines neuen Codewortes handelt,
oder um einen intermediaeren Byte. Das Problem waere, dass ein mit `10xxxxxx`
kodiertes Codewort eben ein *Praefix* eines anderen sein koennte.

## Strukturierte Darstellung

Damit Anwendungen Daten untereinander austauschen koenenn, muessen sie eine
inheitliche Syntax fuer die ausgetauschten Daten festlegen. Moeglichkeiten
hierfuer waeren:

* (gepackte) `struct`s: Daten wuerden so wie sie im Speicher vorliegen
  uebertragen. Das hat nicht nur das Problem, dass man ueber Huerden springen
  muss, um die Groesse des Structs (wegen moeglichem Compiler-Padding)
  einheitlich zu bestimmen (`__attribute__((packed))` bei GCC) und dass man
  sicherstellen muss, dass beide Parteien dieselbe Programmiersprache, denselben
  Compiler und dieselben Datenstrukturen verwenden. Ebenso muss die Endianness
  der System uebereinstimme. Das ist also sicherlich keine portable oder gute
  Idee.
* Ad-Hoc Datenformate: Formate, die spezifisch fuer die Anwendung entworfen
  werden. Hierbei muss man sich um Dokumentation, Erweiterbarkeit,
  Fehlerfreiheit, Eindeutigkeit etc. kuemmern. Es violated auch das DRY Prinzip.
* Strukturierte, populaere Serialisierungsformate wie JSON oder XML. Fuer diese
  gibt es natuerlich auch mehr Tools.

### JSON

Wir wollen die JavaScript Object Notation (JSON) etwas naeher betrachten. Es
wurde urspruenglich im ECMA-404 (ECMA ist eine Standardisierungsorganisation)
und RFC 7159 spezifiziert und ist ein sprachenunabhaengiges, menschenlesbares
Datenformat. Es definiert folgende *Datentypen*:

* `number`
* `string`
* `boolean`
* `array`
* `object`
* `null`

Zwischen Elementen kann hierbei beliebig viel Whitespace eingefuegt werden. JSON
ist vollkommen textbasiert und wird allgemein mit UTF-8 kodiert. Eine JSON Datei
ist eine ungeordnete Sammlung von Key/Value Paaren, wobei der Key immer ein
Unicode-String ist. Die Values koennen dabei von einem der oben genannten
Datentypen sein.

## Datenkompression

Kompressionsverfahren haben allgemein das Ziel, Redundanz, im
informationstheoretischen Sinn, aus Daten zu nehmen. Wir greifen hierbei also
wieder das Themengebiet der Quellenkodierung aus der physikalischen Schicht
auf. Esw ird hierbei zwischen zwei Arten von Kompressionsverfahren
unterschieden:

1. *Verlustfreie Kompression* (*lossless compression*): hierbei koennen
   komprimierte Daten verlustfrei, d.h. exakt und ohne Informationsverlust
   wiederhergestellt werden. Beispiele fuer lossless Kompressionsformate sind
   *ZIP*, *PNG* (deswegen groesser als JPG) oder *FLAC* (free lossless audio
   codec).

2. *Verlustbehaftete Kompression* (*lossy compression*): ermoeglicht es nicht,
   komprimierte Daten *exakt* wiederherzustellen. Es tritt also bei einer
   Dekomprimierung ein *Verlust* von Information auf (dieser ist meist nicht
   merkbar gross, vor allem wenn man nicht oft komprimiert). Dafuer haben solche
   Daten meist kleinere Groessen und erlaube eine variable
   Kompressionsrate. Beispiele sind *MP3*, *MPEG* oder *JPG*.

### Huffman-Code

*Huffman-Kodierung* bzw. *Huffman-Codes* sind ein gaengiges
Kompressionsverfahren. Es wird beispielsweise von TLS verwendet (vor der
Verschluesselung). Die grundlegende Idee des Huffman Codes ist es, die
informationstheoretische Entropie einer Datenquelle bzw. die
Auftrittswahrscheinlichkeiten von einzelnen Codewoertern auszunutzen. Es ist ja
beispielsweise so, dass nicht alle Buchstaben im Alphabet in der deutschen
Sprache mit derselben Haeufigkeit auftreten. Beispielsweise tritt der Buchstabe
*E* mit 17.4% am haeufigsten auf, gefolgt von *N* mit 9.8%. Eine naive Kodierung
wie ASCII wuerde diese Buchstaben allesamt mit 8 Bit kodieren. Der
Informationsgehalt dieser Codewoerter, $I(E) = -\log(0.174) \approx 1.749$
bzw. $I(N) \approx 2.323$, sagt uns aber, dass man in einem optimalen Code sogar
nur $1.749$ Bits fuer das $E$ braeuchte. Offensichtlich liegt in einem ASCII
Code (7 Bit fuer $E$) also jede Menge Redundanz vor. Der Huffman-Code will diese
rausnehmen.

Betrachten wir anhand eines Beispieles, wie man einen Huffman-Code
konstruiert. Gegeben sei das Alphabet $\mathcal{A} = \{A, B, C, D\}$ sowie
Auftrittswahrscheinlichkeiten $\Pr[X = z]$, dass das von einer Quelle ueber
diesem Alphabet als naechstes emittierte Zeichen gerade $z \in \mathcal{A}$
ist. Hierbei nehmen wir an, dass alle Zeichen unabhaengig voneinander
auftreten. Dann gebe die folgende Tabelle die Auftrittswahrscheinlichkeiten der
einzelnen Zeichen an:

| z | Pr[X = z] |
|:--|:----------|
| A |    0.05   |
| B |    0.15   |
| C |    0.30   |
| D |    0.50   |

Dann tun wir nun folgendes: solange bis wir alle Zeichen bearbeitet haben
waehlen wir jenes von den verbleibenden aus, das die *kleinste*
Auftrittswahrscheinlichkeit hat. Dieses geben wir dann in einen *Baum*, wobei
wir in den Blaettern immer die Zeichen haben werden. Je zwei Blaetter verbinden
wir dann zu einem inneren Knoten, in welchem wir die Summe der
Wahrscheinlichkeiten der einzelnen Zeichen addieren. Schritt fuer Schritt
wandern (addieren) wir so den Baum rauf, bis wir am Ende bei einer
Wahrscheinlichkeit von 1.0 sind. Das saehe dann so aus:

Schritt I: Wir nehmen das Zeichen mit der kleinsten Auftrittswahrscheinlichkeit
           und geben es in den Baum:

```
0.05 (A)
```

Schritt II: Wir nehmen das Zeichen mit der naechst kleineren Wahrscheinlichkeit
            und verbinden es mit $A$ zu einem inneren Knoten:

```
      0.20
     /     \
    /       \
0.05 (A)  0.15 (B)
```

Dann immer so weiter:


```
                 1.0
                /   \
               /     \
              /       \
            0.50       \
           /    \       \
          /      \       \
         /        \       \
      0.20         \       \
     /     \        \       \
    /       \        \       \
0.05 (A)  0.15 (B) 0.30 (C) 0.50 (D)
```

Die Idee ist nun die folgende: linke Wege repraesentieren immer eine 0 und
rechte Wege immer eine 1. Um den Code fuer ein Zeichen zu bestimmen, wandern wir
__von der Wurzel__ den Baum entlang __zum Zeichen__ (einem Blatt) und notieren
fuer jede Verbindung den entsprechenden Bit. Mit Labels saehe das so aus:

```
                 1.0
                /   \
              0/     \
              /       \
            0.50       \
           /    \       \1
         0/      \       \
         /        \1      \
      0.20         \       \
    0/     \1       \       \
    /       \        \       \
0.05 (A)  0.15 (B) 0.30 (C) 0.50 (D)
```

So ergeben sich die folgenden Codes:

| z | Code |
|:--|:-----|
| A | 000  |
| B | 001  |
| C | 01   |
| D | 1    |

Wir koennen nun zwei Sachen erkennen. Zum Einen ist diese Kodierung effizienter
als eine naive Kodierung. Mit einer naiven Kodierung meinen wir jene, die
einfach $\lceil\log_2(N)\rceil$ Bits verwendet, wobei $N$ die Anzahl an Zeichen
ist. Diese Kodierung geht naemlich davon aus, dass alle Zeichen dieselbe
Auftrittswahrscheinlichkeit haben (dass die Quellenentropie also maximal
ist). Hier koennen wir uns die Codeffizienz nun so berechnen, dass wir immer die
Auftrittswahrscheinlichkeit eines Zeichens mit der Laenge des entsprechenden
Huffman-Codewortes fuer dieses Zeichen multiplizieren (also der Erwartungswert
in der Laenge):

$$E[l_H(z)] = \sum_{z \in \mathcal{A}} \Pr[X = z] \cdot l_H(z)$$

In unserem Fall waere das also:

$$p_A \cdot l_H(A) + p_B \cdot l_H(B) + p_C \cdot l_H(C) + p_D \cdot l_H(D)$$

Das ergibt:

$$0.05 \cdot 3 + 0.15 \cdot 3 + 0.30 \cdot 2 + 0.50 \cdot 1 = 1.7$$

Wie wir sehen benoetigt dieser Code also durchschnittlich nur 1.7 Bit pro
Codewort. Das ist eine Verbesserung von:

$$1 - \frac{E[l_H(z)]}{E[l(z)]} = 1 - \frac{1.7}{2} = 15\%$$

Wir sehen also, dass der Huffman-Code die Wortlaenge reduziert hat. Genauer: er
minimiert sie sogar. Deswegen nennt man ihn auch einen *optimalen Code*. Wichtig
ist auch, dass er *praefixfrei* ist. Wir koennen oben z.B. beobachten, dass
keines der Codewoerter ein Praefix eines anderen Codewortes ist (deswegen fangen
alle Codewoerter ausser $D$ mit 0 an, damit wir $D$ mit *nur einem Bit* (!)
kodieren koennen).

Zur Erstellung eines Huffman Codes braucht die dekomprimierende Seite die
Auftrittswahrscheinlichkeiten oder einfach das Codebuch. Bei komplexeren Daten,
z.B. ganzen Woertern anstelle von einzelnen Zeichen, sind diese oft schwerer
festzulegen. Theoretisch koennen Zeichenhaeufigkeiten dynamisch bestimmt
werden. Dann braucht der Empfaenger aber eben noch das Codebuch.

### Run-Length Codes

Run-Length Encodings sind eine weitere verlustlose Kompressionsstrategie. Die
grundlegende Idee hierbei ist es, haeufige Wiederholungen einzelner Zeichen
(*runs*) so zu kodieren, dass es nur einmal vorkommt und irgendwie festzulegen,
wie oft dieses Zeichen wiederholt werden soll. Im einfachsten Fall wird vor das
betroffene Zeichen die Anzahl der Wiederholungen kodiert.  Betrachten wir
beispielsweise die Zeichenkette `AAAAAAAAAAAAAAAAAAABBC`. Diese hat, so wie sie
hier steht, die Laenge 22. Run-Length Encoding wuerde sie nun einfach als
`19A2BC` kodieren. Nun hat die Zeichenkette nur mehr eine Laenge von 6 und kann
verlustlos dekomprimiert werden. Ein weiteres, aehnilches und maechtigeres
Verfahren ist Lempel-Ziv Kodierung (z.B. LZ77
https://de.wikipedia.org/wiki/LZ77). Im Allgemeinen ist das Run-Length Encoding
Verfahren nicht optimal.
