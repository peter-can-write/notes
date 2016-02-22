# Hashtables fuer Datenbanken

Baeume eignen sich gut, wenn sortierte Daten gefragt sind. Zum Beispiel dann,
wenn wir Bereichsanfagen machen, oder wenn der Schluessel eine Zeichenkette ist
und wir nur einen Praefix von einem gesuchten Schluessel kennen. Aber wenn es
darum geht, einen eindeutigen Schluessel so schnell wie moeglich zu finden, sind
Baeume nicht ideal. Hierfuer gibt es eine weitere Datenstruktur, die sich fuer
solche *exact-match* Anfragen viel besser eignet: Hash-Tabellen.

## Statische Hashtabellen

Im Nicht-Datenbankbereich verwenden wir Hash-Tabellen, die man hier *statisch*
nennen wuerde. Statisch, weil sie als Arrays einmal allokiert sind, und danach
fest sind. Sie koennen also nur wachsen oder schrumpfen, indem man ein neues
Array allokiert und die Daten in dieses neue Array kopiert. Wir werden sehen,
dass es hier eine dynamischere Variante gibt: das erweiterbare Hashing.

Allgemein braucht man fuer ein Hash-Verfahren eine Hash-Funktion $h(k)$, die
einen Schluessel $k$ auf einen Index $h(k)$ in eine Tabelle abbildet. Hierbei
sei gesagt, dass wenn $k$ keine Zahl ist, es eine *pre-hash* Funktion geben
muss, die $k$ zuerst in eine Ganzzahl transformiert. Die einfachste
Hash-Funktion ist dann die Methode des *modular hashing*, wobei der Schluessel
$k$ (eventuell nach pre-hasing) modulo der Tabellengroesse $M$ genommen
wird. Somit ist $h(k) = k \mod M$ immer eine Zahl zwischen $0$ und $M$, somit
also ein passender Index in die Tabelle.

Die Indices, auf welche die Hash-Funktion abbildet nennt man auch
*Buckets*. Fuer Datenbanken ist ein *Bucket* immer eine Seite (ca. 4KiB).

Da die Funktion $h(k)$ allgemein nicht injektiv ist, kann es passieren, dass
zwei unterscheidbare Schluessel $k, k': k \neq k'$ auf den selben Index $h(k) =
h(k')$ abgebildet werden. In einem solchen Fall nennt man $k,k'$ Synonyme und
die Situation als ganzes eine *Kollision*. Dann brauchen wir auch immer eine
*Kollisionsresolutionsstrategie*. Hiervon sind die beiden beliebtesten
*Separate-Chaining* und *Open-Adressing*. Fuer die erste Methode wird von jedem
Index in der Tabelle aus eine Linked-List gesponnen. Fuer die zweite Methode
wird bei einer Kollision solange nach rechts iteriert (modulo der Groesse der
Tabelle), bis sich ein freier Index findet, und genau so auch gesucht.

Wenn eine solche statische Tabelle zu voll wird, entweder weil die Ketten zu
lange wird oder die Tabelle zu wenige freie Plaetze hat, muss neu allokiert
werden. Dazu allokiert man also eine neue Tabelle mit (z.B.) doppelt so viel
Platz und muss dann in linearer Zeit, also $O(N)$, *rehashen*. Re-hashen bezieht
sich auf den Prozess, fuer jedes Element einen neuen Index zu berechnen. Da sich
die Groesse geaendert hat, wird der Wert modulo der Tabellengroesse manchmal
auch anders sein. Eine weitere Moeglichkeit ist es, die Tabelle gar nicht zu
vergroessern und nur eine andere Hashfunktion zu waehlen. Das ist natuerlich nur
bei Separate-Chaining Varianten moeglich (bei Open-Adressing ist halt voll wenn
voll ist).

Die Vorteile von einer solchen Hash-Tabelle sind nun, dass auf ein Datum in in
konstanter Zeit, also $O(1)$, zugegriffen werden kann und in amortisiert
konstanter Zeit geloescht und eingefuegt wird. Man beachte dass diese
Laufzweiten durchschnittlich sind, also average bzw. expected case.

## Erweiterbare Hashtabellen

Statische Hashtabellen, wenn sie auch tolle Vorteile haben moegen, sind nicht
immer ideal fuer den Datenbankbereich. Sie haben naemlich auch Nachteile:

* Die Hashfunktion enorm ausschlaggebend fuer die Qualitaet des Hashverfahrens.
* Die Neuallokation mit Rehashing ist sehr teuer und bei einem 24h Betrieb der
  Datenbank oft nicht moeglich.

Das Ziel der erweiterbaren Hashtabellen ist es vorallem, die Notwendigkeit des
Rehashing zu minimieren, da dies immer lineare Zeit kostet.

Die grundlegende Idee des erweiterbaren Hashing ist nun folgendes: Der Hashwert
$h(k)$ soll nicht mehr direkt einen Bucket (Seite) in der Hash-Tabelle
indizieren, sondern sein Wert wird in seiner Binaerdarstellung betrachtet. Es
wird dem Bitmuster des Hashwertes in einem binaeren Entscheidungsbaum (Trie)
gefolgt, an dessen Ende der Bucket liegt.

Hierbei wird aber immer nur ein Praefix der binaeren Zahl betrachtet, je nachdem
wieviele Entscheidungsebenen es in diesem Baum gibt, was wiederum davon abhaengt
wieviele Bits man braucht um genug Verzweigungen fuer alle Buckets zu
haben. Werden z.b. nur zwei Bits betrachtet, so gibt es $2 \cdot 2 = 4$
moegliche Werte. Fuer jeden Bit braucht man eine Stufe, um eine neue binaere
Entscheidung zu treffen.

Indem man nur einen Praefix des ganzen Binaercodes betrachtet, vermeidet man
unnoetig viele leere Buckets. Erlauben wir in unseren Buckets (*Seiten*)
z.B. zwei Elemente und moechten drei Elemente in der Tabelle speichern, dann
waere es Verschwendung nach allen Bits zu unterscheiden. Dann haette man
naemlich $2^{32}$ Buckets und nur drei waeren voll. Viel eher moechten wir nur
anhand des ersten Bits unterscheiden, dann koennen wir die drei Elemente schoen
auf zwei Buckets aufteilen. Spaeter ist die Idee dann, dass wenn ein Bucket voll
wird, wir nach mehr Bits unterscheiden um ein "feineres Sieb" zu erhalten. Dazu
aber bald mehr.

Hier zunaechst eine Visualisierung des eben beschriebenen Konzepts, wobei nach
zwei Bits unterschieden wird:

```
h(x) = (11) 0110
h(y) = (00) 1010

         .
	 0 /   \ 1
	  .     .
   0 /       \ 1
   [y]       [x]
```

Die Anzahl an Bits, nach denen hierbei unterschieden wird (oben zwei) nennen wir
dann die *globale Tiefe* und kuerzen es mit $gT$ ab.

Hier muss man jedoch beim Einfuegen eines neuen Elements, dass nicht in eine
bereits existierende Seite passt, einen neuen Knoten bzw. eine neue Seite
erstellen (wenn man am Ende ist). So ist ein Trie auch definiert. Um diese teure
dynamische Allokation zu vermeiden, hat man einfach schon alle Knoten vorher,
wobei manche "Dummy"-Knoten sind, sprich auf denselben Bucket zeigen wie andere
Knoten.

Zeigen z.B. zwei Knoten auf einen Bucket, dann heisst das, dass der letzte Bit
im Praefix nicht beachtet wird, sondern alle Elemente die $gT - 1$ Bits gleich
haben dort reingelotst werden (der $gT$-te Bit kann also 0 oder 1 sein). Die
Anzahl an Bits nach denen unterschieden wird, bis ein Element in einem Bucket
landet, nennt man *lokale Tiefe* $lT$. Insgesamt gibt es also $gT$ Bits. Von
diesen werden $lT$ Bits betrachtet. Somit gibt es $gT - lT$ Bits, die wir nicht
betrachten. "Nicht betrachten" bedeutet hierbei, dass sie beliebige Werte
annehmen koennen, da sie immer im selben Bucket landen. Fuer $gT - lT$ Bits gibt
es bekanntlich $2^{gT - lT}$ moegliche Belegungen, somit gibt es auch
genausoviele Verweise auf einen konkreten Bucket.

Da nun also alle "Bit-Pfade" von vorneherein verzweigt sind, kann man den ganzen
Baum auch einfach wegoptimieren und ein normales Array anlegen. Ist die globale
Tiefe z.B. gleich 3, so legt man einfach ein Array von $2^3 = 8$ Elementen
an. Der Hashwert kann dann als Index in dieses Array benutzt werden. Dieses
Array nennt man dann *Directory*.

### Suche

Gegeben nun also ein zu suchender Schluessel $k$. Dann berechnet man einfach
seinen Hashwert $h(k) \in \{0, 1\}^\star$ und fetched sich die relevanten oberen
Bits als Index, z.B.:

```python
offset = bitwidth - global_depth # data-type width offset
mask = ((1 << global_depth) - 1) << offset # shift over mask bits
index = (h(key) & mask) >> offset # mask off bits and shift into range
```

Und mit diesem kann man nun einfach das Array indizieren. Die lokale Tiefe ist
hierbei irrelvant, da am Ende einfach ein Pointer auf einen Bucket zeigt (auch
wenn dieser nicht unique ist wenn lokale Tiefe < globale Tiefe) und in diesen
wird das Element reingegeben. So hat man keinen Trie-overhead.

### Einfuegen

Wir wollen eineen neuen Wert einfuegen und suchen nach obiger Methode den Bucket
und geben den Wert rein. Wird nun aber der Bucket zu voll, dann:

1. Fall: die lokale Tiefe des Buckets ist weniger als die globale Tiefe (sie
   kann niemals groesser sein), dann splittet man den Bucket einfach und erhoeht
   die lokale Tiefe um eins, muss also (nur!) die Elemente in diesem Bucket
   rehashen.

```
gT = 2
h(k1) = 00
h(k2) = 01

[00] -- [k1|k2] lT = 1
[01] --/
[10] -- [k3] lT = 2
[11] -- [k4] lT = 2

+ k5, h(k5) = 01 => Overflow

[00] -- [k1]    lT = 2
[01] -- [k2|k5] lT = 2
[10] -- [k3]    lT = 2
[11] -- [k4]    lT = 2
```

Hier wurde der erste Bucket also zu voll. Weil noch ein Pointer auf diesen
Bucket zeigte ($\Rightarrow lT < gT$) konnte man also einfach *dynamisch* einen
neuen Bucket erstellen und die Keys die in dem urspruenglichen Bucket drin waren
quasi "lokal" rehashen. $k_2$ passt nach dem Rehashing z.B. besser in den neuen
Bucket. __Die anderen Buckets blieben unveraendert.__ Genau darin liegt der
Vorteil des erweiterbaren Hashings gegenueber dem statischen.

2. Fall: Die lokale Tiefe des Buckets ist gleich der globalen Tiefe, sodass der
   Bucket also mit den verfuegbaren Bits nicht mehr gesplittet werden kann. So
   muss sich die globale Tiefe erhoehen, das Array wird also verdoppelt. Dann
   kann der Bucket gesplittet und rehashed werden, da nun ein Bit mehr zur
   Differenzierung von Elementen zur Verfuegung steht. __Wichtig__: Die lokale
   Tiefe der __anderen__ Buckets bleibt gleich und __es muss nicht rehashed
   werden__. Es kommt zwar ein Bit dazu, jedoch muss dieser nicht
   beruecksichtigt werden und die neu entstandenen Array Indizes zeigen nun
   einfach auf diese Buckets (mehr Dummy Pointer).

```
gT = 2

[00] -- [k1|k2] lT = 2
[01] -- [k5]    lT = 2
[10] -- [k3]    lT = 2
[11] -- [k4]    lT = 2

Schritt 1: gT' = 3

[000] -- [k1|k2] lT = 2
[001] /
[010] -- [k5]    lT = 2
[011]/
[100] -- [k3]    lT = 2
[101] /
[110] -- [k4]    lT = 2
[111] /

Schritt 2: + k6, h(k6) = 001 => Overflow

[000] -- [k1|k2] lT = 3 # rehashing nur hier
[001] -- [k6]    lT = 3 # rehashing nur hier
[010] -- [k5] lT = 2
[011]/
[100] -- [k3] lT = 2
[101] /
[110] -- [k4] lT = 2
[111] /
```

Man sollte hier vorallem systematisch vorgehen und zuerst die globale Tiefe um
eins erhoehen und die Dummy Pointer erstmal verbinden. Erst *danach* sollte man
die eigentliche Einfuegeoperation durchfuehren, dann reduziert sich diese ganze
Operation naemlich auf:

1. Verdopple die Groesse und verbinde die Dummy Pointer.
2. Mache das exakt selbe wie im 1. Fall (allokiere neuen Bucket usw.)

### Deletion

Dazu Definition eines Nachbarbucket: Ein Bucket ist "Nachbar" eines anderen,
wenn sie die selbe lokale Tiefe haben und wenn sie Bits alle ausser dem LSB (des
Praefixes) gemeinsam haben, i.e. $lT - 1$ Bits sind gleich, dann ist beim einen
Bucket die $0$ und beim anderen die $1$.

```
^ = Alle bis auf den letzten Bit gleich (lT - 1)

[00] Nachbar von 01
[01] Nachbar von 10
 ^
[10] Nachbar von 11
[11] Nachbar von 10
 ^
```

Es wird ein Schluessel also aus einem Bucket geloescht. Dann:

1. Fall: Der Nachbarbucket ist zu voll fuer einen Merge. Dann geschieht eben
   nichts (ausser der Bucket wird leer, dann wird er deallokiert, der Zeiger auf
   den Nachbarbucket gelegt und die lokale Tiefe von diesem dekrementiert.)

```
gT = 2

[00] -- [k1|k2] lT = 2
[01] -- [k5|k7] lT = 2 # Nachbar von 00, kein Platz
[10] -- [k3] lT = 2
[11] -- [k4] lT = 2

- k2

[00] -- [k1] lT = 1
[01] -- [k5|k7] lT = 2
[10] -- [k3] lT = 2
[11] -- [k4] lT = 2
```

2. Fall: Der Nachbarbucket hat genug Platz, dass die verbleibenden Elemente des
   Buckets, wo geloescht wurde, in diesen Nachbarbucket "ge-merged" werden
   koenen. Der alte Bucket wird dann de-allokiert und die lokale Tiefe
   dekrementiert.

```
gT = 2

[00] -- [k1|k2] lT = 2
[01] -- [k5] lT = 2 # Nachbar von 00, hat Platz
[10] -- [k3] lT = 2
[11] -- [k4] lT = 2

- k2

[00] \
[01] -- [k1|k5] lT = 1 (!)
[10] -- [k3] lT = 2
[11] -- [k4] lT = 2
```

Ist die lokale Tiefe aller Buckets kleiner als die globale Tiefe, so kann die
globale Tiefe um eins verringert werden, das Array also in seiner Groesse
halbiert werden (Re-allokation).
