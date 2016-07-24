# # OSI Modell: Layer 2

## Graphdarstellung

Wir stellen Netzwerke oft als Graphen dar, wobei wir eine bestimmte Notation
verwenden.

### Gerichtete Graphen

Ein gerichteter Graph stellt ein *asymmetrisches* Netzwerk dar, wobei Kanten
also nicht symmetrisch sind. Ein solcher Graph $G = (N, A)$ ist durch eine Menge
an Knoten $N$ (Nodes oder Vertices) und einer Menge an *gerichteten* Kanten $A$
(*Arcs*) definiert. Die Menge an Kanten $A$ enthaelt dabei *2-Tupel* bzw. Paare,
sodass fuer eine gerichtete Kante von $i$ nach $j$ ein Paar $(i, j) \in A$ ist.

Hierbei kann eine Kante $(i, j)$ noch *Kosten* $c_{i,j}$ haben. Das waeren
beispielsweise die Kosten, um Daten vom Knoten $i$ zum Knoten $j$ zu uebertragen.

### Ungerichtete Graphen

Ein *symmetrisches* Netzwerk laesst sich als *ungerichteter* Graph $G = (N, E)$
darstellen, wobei $N$ wieder eine Menge an Knoten ist und $E$ eine Menge an
Kanten (hier: *Edges*). Die Kanten koennen wieder Kosten haben.

### Pfade

Ein Pfad zwischen zwei Knoten $s, t \in N$ ist eine Menge $P_{s,t} = \{(s, i),
..., (j, t)\}$ gerichteter Kanten, die $s$ und $t$ miteinander
verbinden. Hierbei nennt man $s$ oft den *Source* und $t$ den *Terminal* Knoten.

Die *Kosten* $c(P)$ eines Pfades $P_{s,t}$ entsprechen dabei den Kosten der Kanten
in diesem Pfad: $c(P) = \sum_{(i,j) \in P_{s,t}} c_{i,j}$

Die *Laenge* eines Pfades entspricht der __Anzahl an Kanten__ auf dem Pfad:
$l(P_{s,t}) = |P_{s,t}|$. Diese Pfadlaenge nennt man auch *Hop Count*.

### Besondere Baumstrukturen

Ein Baum ist ein azyklischer, zusammenhaengender Graph. Wir unterscheiden dabei
zwischen zwei Arten von Baeumen, die fuer uns interessant sind:

1. *Shortest Path Tree* (SPT): Verbindet einen Wurzelknoten mit jedem anderen
   Knoten im Graphen mit minimalen Pfadkosten.
2. *Minimum Spanning Tree* (MST): Verbindet alle Knoten im Netzwerk mit
   insgesamt minimalen Kosten.

Diese Baeume sind im Allgemeinen nicht identisch.

## Verbindungen

Wir koennen eine Verbindung bezueglich mehreren Eigenschaften charakterisieren:

* *Uebertragungsrate*
* *Uebertragungsverzoegerung*
* *Uebertragungsrichtung*
* *Mehrfachzugriff* (Multiplexing)

Diese Eigenschaften wollen wir fuer einfache *Punkt-zu-Punkt* Verbindungen
charakterisieren.

### Uebertragungsrate

Die *Uebertragungsrate* $r$ einer Verbindung wird in Bit/s gemessen und sagt
aus, mit welcher Rate Daten auf eine Leitung gelegt werden koennen. Sie haengt
eng mit der *Uebertragungsverzoegerung* $t_s$, auch *Serialisierungszeit*
(*Serialization Delay* oder *Transmission Delay*) genannt, zusammen. Die Formel
ergibt sich dabei analog zum Geschwindigkeit-Weg-Zeit Verhaeltnis:

$$t_s = \frac{L}{r}$$

Hierbei ist $L$ die Anzahl an Bits, die mit der Uebertragungsrate $r$ in $t_s$
Sekunden uebertragen werden sollen. Wollen wir so also zum Beispiel 1500 Byte
mit einer Rate von 100 MBit/s (FastEthernet) senden, so erhalten wir eine
Serialisierungszeit von:

$$t_s = \frac{L}{r} = \frac{1500 \cdot 8}{100 \cdot 10^6} = 120 \mu\text{s}$$

Intuitiv kann man sich die Serialisierungszeit als die Zeit vorstellen, die man
benoetigt, um Murmeln (Daten) in ein Rohr zu stopfen. Man kann diese Murmeln
nicht alle gleichzeitg in das Rohr stopfe, insofern dauert es eine Weile, bis
alle Murmeln im Rohr ankommen. Das ist gerade die *Serialisierungszeit*.

### Ausbreitungsgeschwindigkeit

Wenn wir Daten ueber ein Kabel senden wollen, dann gibt uns die
Serialisierungszeit an, wie lange es dauert, bis die Daten allesamt auf das
Kabel "gelegt" wurden. Wie lange dauert es aber, bis die Daten durch das Kabel
gewandert sind? Dies wird von der *Ausbreitungsverzoegerung* $t_p$ (*Propagation
Delay*) bestimmt.

Im Allgemeinen propagieren elektromagnetische (EM) Wellen mit
Lichtgeschwindigkeit $c$. Das gilt aber nur, wenn das Medium Vakuum ist. In
anderen Medien, beispielsweise Kupfer oder Glas, ist die Lichtgeschwindigkeit
reduziert. Um welchen Faktor die Lichtgeschwindigkeit reduziert ist, haengt von
der *relativen Ausbreitungsgeschwindigkeit* $\nu$ ab. Die letztendliche
Geschwindigkeit, mit welcher eine EM Welle durch ein Medium propagiert, ist also
gerade $\nu c$. Haben wir also nach $t_s$ Sekunden unsere Daten serialisiert, so
gibt uns die Ausbreitungsverzoegerung $t_p$ an, wie lange es dauert, bis der
erste Bit beim Empfaenger ankommt. In Abhaengigkeit von $\nu$ und der *Distanz*
$d$ der Uebertragung berechnet sie sich also durch:

$$t_p = \frac{d}{\nu c}$$

Wenn wir jetzt also wieder an die Murmeln und das Rohr denken, so sagt und die
Ausbreitungsverzoegerung, wie lange es dauert, bis die erste Murmel am anderen
Ende des Rohres ankommt, was natuerlich von der Distanz (Laenge) des Rohres und
der Art des Rohres (Medium) ankommt. Die Serialisierungszeit sagte uns nur, wie
lange es dauert, die Murmeln in das Rohr zu stopfen.

### Uebertragungszeit

Jetzt wissen wir also, wie lange es dauert unsere Daten auf das Kabel zu legen
(Serialisierungszeit) und wie lange es dauert, bis der erste Bit dann am anderen
Ende ankommt (Ausbreitungsverzoegerung). Wie lange dauert die ganze Uebertragung
dann? Also, wann kommt der *letzte* Bit, und somit die ganze Nachricht, an?

Nun ja: Nach $t_s$ Sekunden liegt der letzte Bit auf der Leitung. Nach $t_p$
Sekunden kommt dieser dann am anderen Ende an. Folglich ist die gesamte
Uebertragungszeit $t_d$ (*delay*) gerade gleich:

$$t_d = t_s + t_p = \frac{L}{r} + \frac{d}{\nu c}$$

Man kann diese verschiedenen Zeiten in einem *Weg-Zeit-Diagramm* grafisch
veranschaulichen. Hierbei betrachtet man zwei Zeitachsen fuer zwei Knoten $i,
j$, zwischen welchen Daten gesendet werden. Die Distanz auf der "$x$"-Achse
repraesentiert dann die Distanz $d$ zwischen Knoten $i$ und $j$. Man sieht dann
naemlich auch schoen, dass es eine Weile ($t_p$) dauert, bis der erste und dann
der letzte serialisierte Bit von $i$ bei $j$ ankommt. Das wird dadurch
visualisiert, dass die "Uebertragungskante" von $i$ nach $j$ schraeg ist.

```
    i               j
    |_______________|
    |    \_____     | t_p
t_s |          \____|
    |               |
    |___            | t_s
t_p |   \_____      |
    |_________\_____|
    |               |
    v               v
```

### Speicherkapazitaet eines Kanals

Da sich Signale nur mit endlicher Geschwindigkeit ausbreiten, dauert es wie
gezeigt eine Weile ($t_p$), bis der erste Bit beim Empfaenger ankommt. In der
Zwischenzeit werden aber durch die Serialisierung der Daten neue Bits auf das
Kabel gelegt. Das geschieht eben $t_s$ Sekunden lang. Wenn man also das Kabel zu
einem konkreten Zeitpunkt waehrend der Serialiserung "einfrieren" wuerde, so
koennte man auf dem Kabel gerade propagierende Bits betrachten. Z.B. waeren nach
der halben Ausbreitungsverzoegerung $t_p$ gerade $t_p/2 \cdot r$ neue Bits auf
das Kabel gelegt (unter der Annahme, dass $t_p/2 < t_s$ ist). Denn in dieser
Zeit serialisieren wir ja mit Datenrate $r$ neue Bits. Somit koennten wir also
fast sagen, dass das Kabel eine gewisse *Speicherkapazitaet* hat. Diese
Speicherkapazitaet $C$ wird auch *Bandbreitenverzoegerungsprodukt* genannt und
ist also gegeben durch:

$$C = t_p \cdot r$$

Diese Kapazitaet ist also die maximale Anzahl an Bits, die sich in einer
Senderichtung gleichzeitig auf der Leitung befinden koennen. Man kann dann auch
sagen, dass ein Bit eine gewisse "Breite" hat, indem man die Distanz durch die
Kapazitaet teilt. Kann man beispielsweise 50 bit auf einem 100 Meter breiten
Kabel speichern, dann ist ein Bit also $100 / 50 = 2$ Meter lang.

### Uebertragungsrichtung

Wir unterscheiden noch hinsichtlich der Richtung einer Uebertragung zwischen
zwei Knoten $i, j$. Es gibt drei Moeglichkeiten:

1. Simplex: Unidirektional, also immer nur von $A$ nach $B$ und nie von $B$ nach
   $A$.
2. Halbduplex: Bidirektional, aber es kann immer nur in eine Richtung zu einem
   Zeitpunkt gesendet werden.
3. Vollduplex: Bidirektional, wobei gleichzeitig in beide Richtungen gesendet
   werden kann.

### Multiplexing

Mehrfachzugriff oder *Multiplexing* ist die Bezeichnung fuer die Situation, wo
mehrere Nachrichten (verschiedener Teilnehmer) *gleichzeitig* ueber einen Kanal
gesendet werden sollen. Es gibt hierbei vier verschiedene Arten von Verfahren:

1. *Zeitmultiplex* (*Time Division Multiplex*; TDM)
   * Mehrere Teilnehmer koennen auf ein Medium zugreifen, indem die *Zeit*
     aufgeteilt wird (slicing; einer nach dem Anderen).
   * Das kann deterministisch passieren, zum Beispiel wenn Nachrichten in eine
     Queue gegeben werden.
   * Oder auch nicht, beispielsweise bei einem Hub, wo mehrere Teilnehmer
     nicht-deterministisch (willkuerlich) Nachrichten senden koennen, welche
     womoeglich auch kollidieren (und verloren gehen).
2. *Frequenzmultiplex* (*Frequency Division Multiplex*; FDM)
   * Aufteilung des Kanals in unterschiedliche Frequenzbaender und Zuweisung von
     bestimmten Baendern an bestimmte Kommunikationspartner.
   * Wird bei allen Funkuebertragungen, sowie auch optischen Leitern verwendet.
3. *Raummultiplex* (*Space Division Multiplex*; SDM)
   * Verwendung mehrerer paralleler, physischer Uebertragungskanaele
     (z.B. mehrere Kabel oder Adern pro Kabel).
   * *Kanalbuendelung* bei ISDN.
   * Mehrere Antennen bei kabellosen Uebertragungen.
4. *Codemultiplex* (*Code Divison Multiplex*; CDM)
   * Verwendung orthogonaler Alphabete und Zuweisung der Alphabete an
     Kommunikationspartner.
   * Z.B. bei 4B5B Codes: das eine Signal hat den ersten Bit immer gesetzt, das
     andere Signal immer nicht.
   * Bei UMTS verwendet.

Wir haben dabei mehrere Bewertungskriterien fuer Medienzugriffsverfahren mit
mehreren Teilnehmern:

1. *Durchsatz*: Wieviele Nachrichten wir pro Zeiteinheit uebertragen koennen.
2. *Verzoegerung* einzelner Nachrichten (wenn ich eine Nachricht senden will,
   wie lange dauert es, bis ich sie senden darf).
3. *Fairness* zwischen Teilnehmern, die sich ein Medium teilen.
4. *Implementierungsaufwand* fuer Sender und Empfaenger.

Weiters kann TDM, auch *TDMA* (*time-division-multiple-access*) genannt, auf zwei
Weisen implementiert werden:

1. *Synchron* (deterministisch): Jeder Teilnehmer bekommt abwechselnd einen
   Timeslot, in welchem er senden darf. Will er in einem Timeslot nicht senden,
   so passiert eben nichts.  Man nennt dies auch *statisches* Aufteilen der Zeit
   bzw. des Kanals. Das Problem ist aber, dass Datenverkehr meist ploetzlich
   bzw. stossartig auftritt. Das bedeutet, dass ein Teilnehmer ploetzlich viel
   senden will, und danach fuer laengere Zeit nicht mehr. Bei einem statischen
   Verfahren muesste ein Teilnehmer, der sofort senden koennte, moeglicherweise
   erst alle anderen Teilnehmer abwarten, obwohl gerade gar niemand anderes (in
   ihren Timeslots) senden moechte.
2. *Asynchron*: Der Zugriff auf das Medium ist ungeregelt. Die Teilnehmer
   *konkurrieren* also quasi um das Medium. Diese Variante loest das Problem der
   hohen Verzoegerung von synchronem TDM.

### ALOHA

*ALOHA* ist ein *Random Access* (asynchrones) TDMA Verfahren, bei welchem
 Empfaenger um ein Medium *konkurrieren*. Konkret geht man bei diesem Verfahren
 von mehreren *Stationen* aus, welche allesamt an eine *Basisstation* senden
 wollen. Eine Station *sendet dabei einfach, sobald sie Daten hat*. Da alle
 Stationen auf der selben Frequenz (in der selben Bandbreite) senden, kann es
 dabei also zu Kollisionen kommen.

 Wenn es zu *keiner* Kollision kam, sendet die Basisstation an den Sender eine
 "Quitting" um zu bestaetigen, dass die Uebertragung erfolgreich war. Diese
 Bestaetigung wird dabei *auf einer anderen Frequenz* als der
 Uebertragungsfrequenz gesendet, damit es zwischen Nachrichten und
 Bestaetigungen nicht zu Kollisionen kommen kann. Das nennt man auch
 *out-of-band* Bestaetigung.

Da alle Stationen willkuerlich und vollkommen unkontrolliert senden koennen,
kann es also wie gesagt zu Kollisionen kommen. Wieviele Kollisionen es dabei
gibt, kann man mathematisch modellieren und beschreiben. Wir machen dabei
folgende Annahmen:

* Wir haben eine mittlere bis beliebig grosse Anzahl an Stationen ($N > 15$)
* Alle Stationen senden mit gleicher, unabhaengiger und im Allgemeinen geringer
  Wahrscheinlichkeit
* Alle Nachrichten sind gleich gross und brauchen also auch gleich lange
  (Sendedauer $T$)

Dann modellieren wir die Situation wie folgt:

* Ob ein bestimmter Knoten (Station) $i$ innerhalb eines Zeitintervalls
  $[t, t + T[$ sendet, oder nicht, entspricht einem Bernoulli-Experiment (binaer) mit Sendewahrscheinlichkeit $p_i$.
* Da alle Knoten mit selber Wahrscheinlichkeit $p$ senden, ist $p_i = p \forall i$.
* Da die $N$ Knoten $N$ mal jeweils unabhaengig voneinander senden, koennen wir den ganzen Sendevorgang durch eine *Binomialverteilung* modellieren.
* Der Erwartungswert dieser Binomialverteilung ist dabei gerade $\lambda = Np$
* Fuer sinnvoll grosse $N$, wie wir sie annehmen, kann man eine solche Binomialverteilung dabei durch eine Poisson-Verteilung approximieren.

Dabei betrachten wir eine Zufallsvariable $X_t$, welche beschreibt, wieviele Knoten im Intervall $[t, t + T[$ gleichzeitg senden wollen. Die Wahrscheinlichkeit, dass $X_t = k$ fuer $k$ Knoten ist durch die Poission-Verteilung dann wie folgt beschrieben:

$$\Pr[X_t = k] = \frac{\lambda^k e^{-\lambda}}{k!}$$

Die Besonderheit an einer Poisson-Verteilung im Vergleich zu einer
Bernoulli-Verteilung hierbei ist, dass wir nicht mehr $N$ unabhaengige
Teilexperimente bzw. $N$ Elemente betrachten, fuer welche wir eine
Erfolgswahrscheinlichkeit $p$ wissen. Mit der Poisson-Verteilung modelliert man
Ereignisse, die mit einem gewissen Erwartungswert $\lambda$ in einem
Zeitintervall auftreten. Insofern ist nur dieser Erwartungswert $\lambda$
relevant und so auch der einzige Parameter der Verteilung. Fuer ein
Zufallsexperiment sagen wir nun nicht mehr, dass $k$ Ereignisse (mit
Wahrscheinlichkeit $p$) eintreten und $N - k$ nicht. Wir sagen einfach, dass nur
$k$ Ereignisse eintreten, wobei die Wahrscheinlichkeit dafuer durch den
Erwartungswert parametrisiert ist. Das ist dann nuetzlich, wenn wir die
Wahrscheinlichkeit eines Erfolges nicht explizit wissen, sowie wenn die Anzahl
an betrachteten Elementen unklar oder zu gross ist. Beispielsweise kann man mit
Poission modellieren, wieviele E-Mails man pro Tag bekommt. Man beobachtet, dass
man im Schnitt 4 E-Mails taeglich erhaelt. Man kann hier nicht wirklich
beschreiben, mit welcher Wahrscheinlichkeit man eine E-Mail erhaelt oder
nicht. Auch gibt es keine Anzahl $N$ moeglicher E-Mails, somit kann man nicht
sagen, dass man $X$ E-Mails "nicht erhalten hat". Waere die Wahrscheinlichkeit
bekannt und gaebe es nur $N$ moegliche E-Mails, so koennte man mit einer
Binomialverteilung beschreiben, dass man $k$ erhaelt und $N - k$ *nicht*. Wenn
es aber kein $N$ und kein $p$ gibt, sondern nur einen Erwartungswert $\lambda$,
dann ist eine Poisson-Verteilung geeigneter.

Fuer das Modell unserer Stationen ist Poisson insbesondere interessant, weil wir
leichter sagen koennen, wieviele Stationen im Schnitt pro Intervall senden, als
eine Wahrscheinlichkeit $p$ dafuer anzugeben. Hierfuer muessen wir aber eben
annehmen, dass es ausreichend viele Stationen gibt. Hierbei sei angemerkt, dass
es dennoch besser ist, die Wahrscheinlichkeit fuer eine erfolgreiche
Uebertragung mit der Binomialverteilung zu berechnen, wenn die
Wahrscheinlichkeit schon gegeben ist. Denn die Poisson-Verteilung ist nur eine
Approximation der Binomialverteilung.

Eine beliebige Station sendet also zum Zeitpunkt $t_0$ eine Nachricht. Eine
Kollision genau dann auf, wenn eine andere Station:

* Im Intervall $[t_0, t_0 + T[$ selbst sendet, oder
* Im vorherigen Intervall $]t_0 - T, t_0[$ angefangen hat zu senden.

Das Intervall $]t_0 - T, t_0 + T[$ nennt man folglich das *kritische Intervall*. Durch die Poisson-Verteilung erhalten wir dann die Wahrscheinlichkeit $p_0$ fuer eine erfolgreiche Ubertragung durch:

1. Die Wahrscheinlichkeit, dass im vorherigen Intervall (infinitisemal nach dem Start) keiner sendet: $\Pr[X_{t_0 - T} = 0]$
2. Die Wahrscheinlichkeit, dass im momentanen Intervall genau einer sendet:
   $\Pr[X_{t_0} = 1]$

Somit:

$$
\begin{align}
	p_0 &= \Pr[X_{t_0 - T} = 0] \cdot \Pr[X_{t_0} = 1]\\
	    &= \frac{0^k e^{-\lambda}}{0!} \cdot \frac{\lambda^1 e^{-\lambda}}{1!}\\
		&= e^{-\lambda} \cdot \lambda e^{-\lambda}
		&= \lambda e^{-2\lambda}
\end{align}
$$

Hierbei sei angemerkt, dass das konkrete Zeitintervall fuer die
Poisson-Verteilung egal ist, da wir annehmen, dass die Anzahl sendender
Stationen fuer jedes Zeitintervall gleichverteilt ist.

#### Slotted ALOHA

Eine Variante von ALOHA, welche ein wenig besser funktioniert, ist *Slotted
ALOHA*. Hierbei duerfen Stationen nicht mehr zu beliebigen Zeitpunkten zu senden
beginnen, sondern nur mehr am Anfang diskreter Zeitintervalle $nT: n \in
\mathbb{N}_0$. Somit verschwindet also das hintere Intervall $]t_0 - T,
t_0[$ aus dem kritischen Bereich, da zum Zeitpunkt $t_0$ jede Uebertragung aus dem vorherigen Slot (ab $t_0 - T$) schon vollendet sein muss. Der kritische Bereich ist somit auch nur mehr $T$ lang und nicht $2T$, wodurch sich die Wahrscheinlichkeit fuer eine erfolgreiche Uebertragung auf den ersten Term von oben reduziert:

$$p_0 = \Pr[X_{t_0} = 1] = \lambda e^{-\lambda} = \frac{\lambda}{e^\lambda}$$

### CSMA

Eine einfache Verbesserung von ALOHA bzw. Slotted ALOHA (welches immer ueber
normalen ALOHA zu bevorzugen ist) ist das Prinzip *Listen before Talk*, einer
Variante von *Carrier-Sense-Multiple-Access* (*CSMA*). Hierbei hoert jede Station
das Uebertragungsmedium einfach ab, bevor es erst zu senden beginnt (falls das
Medium auch frei war).

Weiters betrachten wir verschiedene Arten von CSMA:

* *Non-persistent* CSMA:
  1. Hoere das Medium ab. Wenn es frei ist, uebertrage im naechstmoeglichen
     Slot.
  2. Wenn es nicht frei ist, warte eine feste oder zufaellige Zeitspanne
     (z.B. einen Slot) und hoere dann wieder ab (1).

* *1-persistent* CSMA:
  1. Hoere das Medium ab. Wenn es frei ist, uebertrage im naechstmoeglichen
     Intervall.
  2. Wenn es nicht frei ist, warte busy bis es frei ist (spinning).

* *$p$-persistentes* CSMA:
  1. Hoere das Medium ab. Wenn es frei ist, dann:
	 1. Uebertrage mit Wahrscheinlichkeit $p$ im naechstmoeglichen Slot, oder
	 2. Verzoegere mit Wahrscheinlichkeit $1 - p$ und hoere danach wieder ab.
  2. Wenn es nicht frei ist, warte bis es frei ist. Dann (1).

So merken:

1. non-persistent: wartet feste Zeitspanne.
2. persistent: spinned.
   $1$: sendet immer im naechstmoeglichen Slot.
   $p$: sendet mit Wahrscheinlichkeit $p$.

Es sei angemerkt, dass wir hierbei natuerlich immer von *Slotted* ALOHA
ausgehen, sonst macht das mit den Slots ja keinen Sinn.

*Abhoeren* wuerde bei CSMA im Uebrigen bedeuten, dass die Stationen sehen, ob
ihnen gerade etwas gesendet wird. Sendet eine Station, so breiten sich deren
Informationen natuerlich nicht nur zur Basisstation, sondern zu allen Stationen
aus. Eine Station weiss also, dass ein Medium besetzt ist, wenn es gerade
Nachrichten (die fuer die Basisstation gedacht sind) empfaengt.

Man beachte aber, dass CSMA Kollisionen nur weniger wahrscheinlich macht, aber
nicht vollkommen verhindern kann. Es koennen zum Beispiel mehrere Teilnehmer
gleichzeitig erkennen, dass das Medium frei ist, und gleichzeitg senden. Vor
allem muss man aber bedenken, dass es auch eine Weile dauert, bis das Medium
"besetzt" wird, wenn eine Station einemal sendet. Nehmen wir die folgende
Situation:

1. Eine Station hat Daten anliegen, waecht also auf.
2. Es hoert das Medium ab (empfange ich Nachrichten auf der
   Uebertragungsfrequenz?) und sieht, dass es frei ist.
3. Es sendet die Nachricht (mit Wahrscheinlichkeit $p$ bei $p$-persistentem
   CSMA).
4. Es dauert nun erstmal $t_p$ Sekunden, bis diese Nachricht bei anderen
   Stationen ankommt (bzw. anfaengt anzukommen), das Medium fuer sie also
   *besetzt* wird.
5. Nach $t_p/2$ Sekunden waecht eine andere Station auf und hat auch Daten
   anliegen.
6. Da nach der halben Ausbreitungsverzoegerung die Nachricht von der ersten
   Station noch nicht bei dieser Station angekommen ist, wirkt das Medium fuer
   diese zweite Station als nicht besetzt.
7. Es sendet also, und es kommt trotz CSMA zu einer Kollision.

Es sei wohlgemerkt, dass nach der Ausbreitungsverzoegerung das Medium fuer die
anderen Stationen besetzt waere. Bei CSMA ist das kritische Fenster also genau
$[t_0, t_0 + t_p]$. (Auch sei beachtet, dass $t_p$ bei unterschiedlichen
Distanzen von der sendenden Station auch fuer jede hoerende Station andere Werte
annimmt.) Auch sei angemerkt, dass diese Ueberlegungen nur fuer non-slotted
ALOHA valide sind, da bei slotted ALOHA eine Station erst zum naechsten Slot
senden wuerde, wenn das Medium gerade frei ist. Somit kaeme es nicht zu dieser
Situation. Bei slotted ALOHA gibt es nur mehr die Gefahr, dass zwei Stationen
gleichzeitig merken, dass das Medium frei ist und gleichzeitig zum naechsten
Slot senden. Hierbei hilft $p$-persistentes CSMA eben aus, da bei geringem $p$
nicht beide Stationen dann wirklich senden wuerden. Am guenstigsten scheint im
Uebrigen $0.01$-persistentes und $0.1$-persistentes CSMA zu sein.

### CSMA/CD

Wie oben beschrieben verhindert CSMA Kollisionen also nicht. Eine Kollision
wuerde von sendenden Stationen also so bemerkt, dass von der Basisstation keine
Quittierung kommt. Ein anderes Verfahren, um Kollisionen zu *erkennen*, ist die
Verwendung von *JAM* Signalen. Allgemein faellt dies unter das Gebiet von
*Collision Detection* (CD). Insbesondere erlauben JAM Signale ein schnelleres
Erkennen von Kollisionen und schnelleres Abbrechen von kollidierenden
Uebertragungen als fehlende out-of-band Quittierungen.

Wie mit JAM Signalen Kollisionen erkannt wird, sei an diesem Beispiel illustriert:

1. Eine Station $A$ sendet eine Nachricht, weil das Medium gerade frei war.
2. Noch waehrend die Nachricht zu einer anderen Station $B$ propagiert ($t_p$),
   hoert es da Medium ab.
3. Weil fuer Station $B$ das Medium noch frei ist, sendet es auch seine
   Nachricht.
4. Nach einer gewissen Zeit, bevor $B$ mit dem Senden seiner Nachricht fertig
   ist, kommt die Nachricht von $A$ endlich bei $B$ an.
5. Station $B$ erkennt also eine Kollision, und sendet ein spezielles JAM-Signal
   weg.
6. Wenn die Serialisierungszeit der Nachricht von $A$ passend gewaehlt wurde,
   erhaelt Station $A$ dieses JAM Signal noch waehrend es seine Nachricht
   sendet.
7. Beide haben die Kollision also erkannt, und probieren es je nach CSMA
   Verfahren im naechsten Zeitslot nochmal.

Besonders wichtig ist hierbei, dass die Serialisierungszeit von Nachrichten fuer
die Verwendung von JAM Signalen richtig gewaehlt werden muss. Konkret muss
gelten:

$$t_{s_{min}} = 2 t_p \iff L_{min} = \frac{2d}{\nu c} r$$

Die Serialisierungszeit muss also zwei mal so gross, wie die
Ausbreitungsverzoegerung sein. Wir ueberlegen uns, wieso. Dafuer nehmen wir
gerade den Extremfall an: Station $B$ hoert das Medium infinitesimal kurz vor
dem Zeitpunkt ab, wo die Nachricht von Station $A$ bei $B$ ankommt, und sendet
seine Nachricht, weil das Medium ja frei war. Die Nachricht von Station $A$
trifft bei $B$ also unmittelbar nach Beginn der Serialisierung der
Nachricht von $B$ ein. Sofort sendet Station $B$ ein JAM Signal weg. Nun
ueberlegen wir uns: Es hat $t_p$ Sekunden gedauert, bis der erste Bit der
Nachricht von Station $A$ bei $B$ angekommen ist und die Kollision erkannt
wurde. Es dauert dann genau $t_p$ weitere Sekunden, bis das JAM Signal in die
andere Richtung von $B$ bei $A$ ankommt. Das JAM Signal wuerde also erst nach $2
t_p$ Sekunden bei $A$ eintreffen! Wenn nun also die Serialisierungszeit der
Nachricht von Station $A$ weniger als $2 t_p$ Sekunden dauert, ignoriert Station
$A$ das JAM Signal einfach, weil es zum Zeitpunkt des Eintreffens des JAM
Signals ($2 t_p$) ja keine Nachricht mehr sendet (wieso sollte es also zu einer
Kollision gekommen sein? Anmerkung: Eine Station wird das JAM Signal, wenn es
nicht sendet, ignorieren, weil ja auch andere Stationen waehrend ihrer
Uebertragung das selbe JAM Signal senden, was fuer die Station dann keine
Bedeutung haette).

Es gibt nun aber bei Punkt (7) ein Problem: Die Stationen erkennen also eine
Kollision, und warten bis das Medium wieder frei ist und probieren dann
erneut. Wenn nach der Kollision das Medium frei ist, probieren die Stationen es
also sofort wieder. Es kaeme also schon wieder zu einer Kollision!

Die Loesung dafuer ist, nach einer Kollision eine bestimmte Zeit zu warten. Zum
Beispiel verwendet Ethernet (welches CSMA/CD verwendet) *Binary Exponential
Backoff*. Hierbei wird bei der $k$-ten Wiederholung einer Nachricht (also nach
der $k$-ten Kollision):

1. Eine __zufaellige__ Anzahl an Slotzeiten $n$ aus $\{0, 1, \min(2^{k - 1},
   1024)\}$ gewaehlt.
2. Gerade so viele Slotzeiten gewartet, bis man es wieder probiert.

Maximal probiert man es dabei 15 mal, bevor man die Nachricht verwirft. Das
entspricht also 16 Sendeversuchen. Dadurch wird die Kollisionswahrscheinlichkeit
wieder verringert.

Es ist hierbei uebrigens sehr wichtig, dass eine *zufaellige* Anzahl an
Slotzeiten gewaehlt wird. Wenn $A$ und $B$ dieselbe Slotzeit waehlen wuerden,
kaeme es halt nicht sofort, sondern nach $x$ Slotzeiten zur Kollision.

### CSMA/CA

Collision Detection (CD) hat es uns erlaubt, Kollisionen zu erkennen, noch nicht
aber, sie zu *verhindern*. Das ist das Themengebiet der *Collision Avoidance*
(CA). Am einfachsten funktioniert dies mit dem *Request-To-Send (RTS)* /
*Clear-To-Send (CTS)* Prinzip. Hierbei gibt es neben den ueblichen Stationen
noch eine Basisstation, welche sozusagen als Moderator der Kommunikation dient.

Bevor nun eine Station $A$ an eine Station $B$ eine Nachricht senden will,
sendet $A$ zuerst zur Basisstation eine Request-To-Send. Wenn die Basisstation
gerade keinen anderen RTS registriert, akzeptiert die Basisstation diesen
Request und sendet eine Clear-To-Send Nachricht. Dieser CTS geht sowohl an
Station $A$ als auch Station $B$ (in dieser CTS Nachricht muesste stehen, wem
der Kanal nun freisteht, falls beide RTS gesendet haben). Station $A$ darf dann
also senden. Station $B$ weiss dann auch, dass es zuerst eine feste,
vordefinierte Zeitspanne warten muss, bis es ueberhaupt einen RTS an die
Basisstation senden darf.

Die Vorteile davon sind, dass Kollisionen so natuerlich besser vermieden werden
koennen. Kollisionen sind aber dennoch nicht gaenzlich ausgeschlossen, z.B. wenn
Station $B$ das CTS nicht empfangen haette (wir betrachten hier vor allem
drahtlose Netzwerke) und durch einen eigenen RTS die Nachricht von $A$
zerstoert. Auch reduziert RTS/CTS die Datenrate natuerlich.

Ein konkretes Problem, dass CSMA/CA loest, ist das sogenannte *Hidden-Station*
Problem. In dieser Situation haben wir neben der Basisstation zwei Stationen $A$
und $B$, welche sich nicht sehen. Das heisst, sie sind beide gleich weit von der
Basisstation weg, aber auf unterschiedlichen Seiten. Nun kann es eben vorkommen,
dass beide Stationen zur Basisstation senden (da fuer beide das Medium auch frei
waere). Dann wuerden weder Station $A$ noch $B$ eine Kollision erkennen, da die
Nachrichten von $A$ und $B$ nicht zu der jeweils anderen Station
durchkommen. Sie koennten also z.B. kein JAM Signal senden. Dennoch wuerde es
aber spaetestens bei der Basisstation zu einer Kollision kommen. Hier sieht man
also, wo CD (Collision Detection) scheitern wuerde. Daher braucht man eben
Collision Avoidance (CA).

Anmerkungen:

* Grundsaetzlich ist CA nur das Prinzip, bei Funknetzwerken $p$-persistentes
  CSMA zu verwenden. RTS/CTS ist nur eine Erweiterung davon.
* RTS/CTS ist Bestandteil von *Virtual Carrier Sensing*, da das Medium durch ein
  CTS exklusiv fuer eine Zeitspanne reserviert wird.
* Damit RTS/CTS Nachrichten nicht so leicht verloren gehen oder verfaelscht
  werden, werden sie so robust wie moeglich kodiert. Das bedeutet auch die
  hoechste moegliche Redundanz bzw. die kleinste Datenrate.
* RTS/CTS geht auch ohne Basisstation und kann direkt durch die Geraete selbst
  gesteuert werden. Das nennt man dann *ad-hoc Modus*. Die Geraete im *ad-hoc
  Modus* nennt man dann *Service Set*.

### Token Passing

Ein weiteres Verfahren, dass zwar nicht CSMA (weder carrier sense) ist, aber
dennoch CA anbietet, ist *Token Passing*. Hierbei werden Stationen zu einem
physikalischen Ring zusammengeschlossen, sodass Stationen also direkt nur mit
ihren zwei Nachbarn kommunizieren koennen. Es zirkuliert dann immer ein *Token*
durch den Ring. Wenn eine Station Daten anliegen hat, wartet es, bis der Token
bei seiner Zirkulation bei ihm ankommt. Es nimmt den Token dann aus der
Zirkulation und reserviert also quasi den ganzen Ring exklusiv fuer sich. Andere
Knoten koennen den Ring also dann gerade nicht mehr erhalten. Dann sendet es
seine Nachrichten und gibt den Ring danach wieder fuer Zirkulation frei (oder
nach einem bestimmten Timeout).

Wenn eine Station eine Nachricht also sendet, zirkuliert diese auch wieder durch
den Ring. Der Empfaenger markiert die Nachricht dann als gelesen und sendet sie
weiter durch den Ring. Kommt die Nachricht so wieder beim Sender, gilt die
Uebertragung als erfolgreich und der Sender nimmt sie vom Netz.

Eine der Stationen im Netz agiert dabei immer als *Monitor Station*. Sie ist
fuer spezielle Aufgaben zustaendig:

* Wenn der Token verloren geht, generiert sie einen Neuen.
* Wenn mehr als ein Token zirkuliert, nimmt es die uebrigen vom Ring raus.
* Wenn Nachrichten endlos zirkulieren, beispielsweise wenn es an gar keinen
  Rechner im Netz richtig adressiert ist, nimmt es dies auch vom Ring.

Faellt diese Monitor Station aus, wird aus den verbleibenden eine neue
ausgewaehlt.

Token Passing hat den Vorteil, ueberhaupt keine kollisionsbedingten
Wiederholungen zu benoetigen, da es nie zu Kollisionen kommen kann. Die
maximale Verzoegerung von Nachrichten ist durch die feste Ringstruktur auch
deterministisch.

Die Nachteile von Token Passing sind aber auch vielfaeltig. Zum einen muss eben
eine Station spezielle Aufgaben uebernehmen. Auch ist es so, dass wenn ein
Knoten im Ring ausfaellt, die *gesamte* Kommunikation im Ring gestoert ist. Die
Uebertragungsverzoegerung ist oft auch groesser als bei CSMA, da der Sender auf
das Token erst warten muss. Auch ist es oft schwer, Stationen im Netzwerk
ueberhaupt zu einem Ring zusammenzuschliessen. Es wird daher heute fast gar
nicht mehr genutzt.

https://en.wikipedia.org/wiki/Token_ring

## Rahmen

Wir beschaeftigen uns nun mit *Rahmen*, womit wir einfach Nachrichten auf der
Sicherungsschicht meinen. Konkret sehen wir uns an, wie Rahmen gebildet werden,
wie Empfaenger in Rahmen adressiert werden und Fehler in Rahmen erkannt werden
koennen.

### Erkennung von Rahmengrenzen

Fuer die physikalische Schicht sind Nachrichten lediglich eine Folge von
Bits. Natuerlich haben Nachrichten, zumindest fuer uns, aber *Grenzen*. Diese
muss man also aus dem Bitstrom heraus irgendwie erkennen koennen.

Hierbei gibt es verschiedene Moeglichkeiten:

* *Laengenangabe* vor der Uebertragung der eigentlichen Daten,
* *Steuerzeichen*,
* *Begrenzungsfelder* und "Bit-Stopfen" oder
* *Coderegelverletzungen*.

Jedenfalls wollen wir die Rahmengrenzen so bilden, dass deswegen nicht weniger
moegliche Daten uebertragbar sind.

#### Laengenangaben

Eine Idee zur Festlegung von Rahmengrenzen waere es, vor jeder Nachricht die
Groesse der Nachricht zuerst zu senden. Wenn man also z.B. $30$ Bit an Daten
sendet, sendet man die Zahl $30$ vor den eigentlichen Daten. Dann weiss der
Empfaenger, wieviele Bits er erwarten kann. Das setzt natuerlich vorraus, dass
dieses Laengenfeld eindeutig aus dem Datenstrom zu erkennen ist (zumindest fuer
die erste Nachricht nach einer Idle-Phase).

#### Steuerzeichen

Schon in der physikalischen Schicht wurde der Begriff von *4B5B Codes*
eingefuehrt, z.B. zur Fehlererkennung bei der Kanalkodierung. Dieses Prinzip,
jeweils $k$ Bits auf $n$ Bits abzubilden um Platz fuer Steuerzeichen zu machen,
koennen wir auch zum Eingrenzen von Rahmen verwenden.

Beispielsweise bedeute ein gesetzter MSB, dass nun ein Steuerzeichen kommt. Dann
koennten wir einen Code waehlen, wobei 10001 das erste Startzeichen ist und
10010 das zweite (zwei, damit ein Fehler in den Daten nicht so leicht zu einer
faelschlichen Startsequenz fuehren kann). 10011 sei dann das erste Stopsymbol
und 10100 das zweite. So koenenn wir also den Beginn und das Ende eines Rahmens
signalisieren.

In der Praxis heissen diese beiden Start-Signale oft $J$ und $K$. Wie gesagt
gibt es *zwei* separate Startsignale $J/K$, nicht weil man zwei *braeuchte*,
sondern einfach, weil es robuster ist (z.B. wegen Synchronisation) zwei
Startsymbole zu senden. Somit kann im ersten Symbol ein Bit umkippen, ohne dass
ein falscher Rahmen erkannt wird, auch wenn das durch den Fehler entstandene
Symbol eines der J/K Symbole ist. Dadurch muss man die Hardware nicht unbedingt
praeziser machen, weil Leitungszeit eigentlich billiger ist als praezise
Hardware (wo weniger Bits umkippen).

FastEthernet (100 MBit/s) benutzt MLT-3 mit 4B5B Codes und signalisiert Start
und Ende durch Steuerzeichen.

Auch auf der Darstellungsschicht (6) werden Steuerzeichen verwendet,
beispielsweise beim ASCII-Code (`\0`, `\a`, `\n` etc.) oder in Emacs <3 (`M-x C-l`).

#### Begrenzungsfelder und Bit-Stopfen

Bei dieser Idee markieren wir den Start und das Ende einer Nachricht durch eine
bestimmte Bitfolge, beispielsweise 01110. Dann muessen wir nur noch dafuer
sorgen, dass diese Bitfolge nicht in den Daten auftritt und faelschlicherweise
das Ende einer Nachricht signalisiert. Das machen wir durch *Bit-Stuffing*. Wir
fuegen dabei (fuer dieses Beispiel) nach zwei Einsen immer eine Null ein
(011010), dann kann in den Daten die spezielle Sequenz nie vorkommen. Der
Empfaenger weiss dann, dasss wenn er zwei Einsen empfaengt und dann:

* eine 1, so handelt es sich um eine spezielle Sequenz (vorausgesetzt die 0
  hinten und vorne war da / kommt noch)
* eine 0, so kommt diese vom Bit-Stopfen und wird wieder entfernt.

Man muss hierbei auch nicht darum kuemmern, die Sequenz 011010 zu escapen. Nach
zwei Einsen wuerde naemlich eben eine 0 eingefuegt (0110010) und beim Empfaenger
wieder entfernt.

#### Coderegelverletzung

Wie schon bei der Leitungskodierung beschrieben, kann man die Tatsache
ausnutzen, dass bestimmte Kodierungsverfahren wie RZ, NRZ und Manchester
Encoding strenge Regeln fuer Signalwerte haben (z.B. niemals Null bei NRZ)
haben. Durch Verletzung dieser Regeln konnen wir den Start oder das Ende einer
Nachricht signalisieren.

Normales Ethernet (10 MBit/s) benutzt diese Methode. Es verwendet dabei
Manchester Encoding als Leitungscode und signalisiert das Ende eines Frames
durch eine Coderegelverletzung (also bei Manchester Encoding einfach Null). Der
Start wuerde auch einfach durch das erste Symbol signalisiert, da davor nur
Nullen kamen und bei Manchester Encoding auch erst der zweite Edge das Symbol (0
oder 1) signalisieren wuerde (der erste Edge bringt das Signal erst einmal auf +
1 oder -1).

### Adressierung

Wir interessieren uns nun dafuer, wie Stationen eigentlich adressiert werden
koennen. Dabei betrachen wir vorerst *Direktverbindungsnetze*, wo also jeder
Rechner mit jedem anderen Rechner direkt (durch ein Kabel) verbunden ist. Dabei
findet also keine Vermittlung (Routing) statt. Das kommt erst auf Schicht 3.

Auf der Sicherungsschicht adressieren wir Knoten im Netzwerk durch
*MAC-Adressen*. MAC steht dabei fuer *Media Access Control*. Wir haben folgende
Anforderungen an solche Adressen:

* Sie sollten Rechner in einem Direktverbindungsnetz *eindeutig identifizieren*,
* Es soll eine *Broadcast-Adresse* geben, welche *alle Knoten* im Netzwerk
  adressiert, sowie
* *Multicast-Adressen*, die bestimmte Gruppen von Knoten im Netzwerk ansprechen.

Netzwerkkarten fuer alle IEEE 802 Standards haben ab Werk (Produktion) eine
fixe, festgelegte MAC-Adresse. Diese wird meist im ROM der Netzwerkkarte
gespeichert. MAC Adressen kann man aber auch aendern. MAC-Adressen haben dabei
den folgenden Aufbau:

* Sie sind 48 Bit (6 Byte) lang.
* Sie bestehen aus einem:
  1. 24-Bit *OUI* (*Organziationally Unique Identifier*) und
  2. 24-Bit *NIC* (*Network Interface Controller* bzw. einfach Device ID).
* Der OUI referenziert dabei einen bestimmten Hersteller von Netzwerkkarten,
  beispielsweise Apple.
* Eine MAC-Adresse wird in 6 Paaren von Hexadezimalziffern angegeben, welche
  jeweils durch einen Doppelpunkt getrennt werden. Beispielsweise waere
  `de:ad:be:ef:af:fe` eine gueltige MAC-Adresse.
* Nur Einsen, also `ff:ff:ff:ff:ff:ff` bezeichnet dabei die Broadcast-Adresse.
* Der LSB des ersten Oktetts sagt, ob die Adresse *Unicast* (0) oder *Multicast*
  (1) ist.
* Der erste Bit (links vom LSB) des ersten Oktetts bestimmt, ob die MAC-Adresse
  *globally unique* (0) oder *locally administered* (1) ist. Letzteres ist
  *z.B. bei virtualisierten* Netzwerkadaptern nuetzlich. Ist dieser Bit gesetzt,
  *bedeutet dass eine locally unique Adresse.

MAC Adressen werden von den meisten IEEE 802 Protokollen verwendet
bzw. unterstuetzt, insbesondere Ethernet, WiFi und Bluetooth
(https://en.wikipedia.org/wiki/MAC_address).

### Fehlererkennung

Ein typisches L2 (z.B. Ethernet) Rahmen besteht aus den folgenden Teilen:

1. Destination MAC-Adresse,
2. Source MAC-Adresse,
3. Kontrollinformation (z.B. die Art des Pakets/der Nachricht),
4. Die eigentliche Payload (z.B. einem IP-Paket)
5. Einer *Checksum*, auch *Frame Check Sequence* (*FCS*) genannt.

Mit dem letzten Punkt, der Checksum bzw. der FCS, wollen wir uns nun
beschaeftigen. Die Checksum, zu Deutsch *Pruefsumme*, ist ein
__fehlererkennender Code__. Im Gegensatz zur Kanalkodierung wollen wir hier
Fehler nur *erkennen* und nicht *korrigieren*.

Ein konkretes Beispiel fuer eine Checksum, die z.B. bei Ethernet verwendet wird,
ist der *Cyclic Redundancy Check* (CRC). Das Ziel des CRC ist es:

* Moeglichst viele Bitfehler zu erkennen,
* Wenig Redundanz zu den Nutzdaten hinzufuegen zu muessen,
* Fehler zu erkennen, aber nicht korrigieren zu koennen.

Frage: Welche Fehler kann CRC korrigieren?
Antwort: Gar keine, CRC kann Fehler nur *erkennen*!

Der CRC wird bei Ethernet hierbei auf der ganzen PDU, also inklusive
Ether-Header berechnet.

#### Theorie

Fuer CRC benutzen wir die Eigenschaft von Bit-Sequenzen, auch Polynome
darstellen zu koennen. Ein Datenwort der Laenge $n$ Bit gibt dabei ein Polynom

$$a(x) = \sum_{i=0}^{n - 1} a_i x^i$$

vom Grad $n - 1$ an, wobei $a_i \in \{0, 1\}$ ($n$ Bit = Grad $n - 1$ weil bei
Grad $n - 1$ auch $x^0$ als konstanter Term enthalten ist). Jeder Term (jede
Potenz) kann also enthalten sein, oder nicht, je nachdem, welchen Wert der
$i$-te Bit hat. Wenn in einer Bit-Sequenz an Stelle $i$ eine Eins steht, so ist
der Term $x^i$ enthalten. Steht an Stelle $i$ eine Null im Datenwort, so ist
$x^i$ nicht enthalten (bzw. $0 \cdot x^i$ wuerde eben Null
ergeben). Beispielsweise entspricht die Bit-Sequenz $10101001_2$ dem Polynom
$a(x) = x^7 + x^5 + x^3 + x^0$. Hierbei ist $x^0$ natuerlich gleich $1$. Wie man
auch sieht ergeben 8 Bit hier ein Polynom von maximalem Grad 7.

Wir koennen nun alle Polynome vom Grad $n$ in einer Menge

$$F_q[x] = \left\{a \,|\, a(x) = \sum_{i=0}^{n-1} a_ix^i, a_i \in \{0,
1\}\right\}$$

zusammenfassen, welche genau $q = 2^n$ Elemente hat (jeder Term kann enthalten
sein, oder nicht / jeder Bit kann gesetzt sein, oder nicht). Wenn wir nun noch
die Addition und Mulitplikation auf dieser Traegermenge passend definieren,
erhalten wir einen endlichen Koerper $\langle F_q[x], +, \cdot \rangle$.

Die "passende" Addition fuer den Koerper ist dabei die Addition der
Polynomkoeffizienten, also den Bits $\in \{0, 1\}$, nach den Regeln des Galois
Field $GF(2)$, also dem additiv-multiplikativen Koerper modulo $2$. Das heisst
folgendes:

1. $0 + 0 = 0$
2. $0 + 1 = 1 + 0 = 1$
3. $1 + 1 = 2 \equiv_2 = 0$
4. $1 - 1 = 0$
5. $0 - 1 = -1 \equiv_2 1$
6. Addition ist also das selbe wie Subtraktion,
7. Und genau gleich der logischen XOR-Verknuepfung.
8. Der resultierende Grad des Polynoms / Laenge der Bitsequenz ist kleiner oder
   gleich, niemals groesser als die Grade/Laengen der Operanden.

Beispiel: (Achtung: Die Bits sind Koeffizienten von Polynomen, also kein Carry!)

```
10101110
11101010
--------
01000100
```

```
10101110 XOR
11101010
--------
01000100
```

Die "passende" Multiplikation ist etwas komplexer definiert, da die
Multiplikation zweier Bitsequenzen der Laenge $n$ maximal eine Bitsequenz der
Laenge $2n$ gibt, sodass also der Grad des resultierenden Polynoms nicht mehr
kleiner $n$ ist und das Polynom folglich auch nicht in $F_q[x]$ enthalten
waere. Wir muessen also das Produkt zweier Polynome $a(x)$ und $b(x)$ immer
modulo einem *Reduktionspolynom* $r(x)$ vom Grad $n$ rechnen, sodass der Grad
des Produkts auch kleiner $n$ bleibt. Wir multiplizieren $a(x)$ mit $b(x)$ dabei
ganz normal, fuehren dann aber eine Polynomdivision mit $r(x)$ durch. Der Rest
der Division ist dann gerade der Wert von $a(x) \cdot b(x) \mod r(x)$, welchen
wir wollen.

Waehlt man hierbei $r(x)$ als *irreduzibles* Polynom, welches sich also nicht
als Produkt zweier anderer Polynome darstellen laesst (Primzahl im
Polynombereich), erhaelt man einen endlichen Koerper. Das wollen wir aber bei
CRC nicht. Waehlen wir naemlich ein reduzibles Polynom, haeufig $p(x)(x + 1)$ wo
$p(x)$ irreduzibel ist, erhalten wir einen nicht-endlichen Koerper. Dadurch
koennen wir dann alle ungeradezahligen Fehler erkennen (1 Bit falsch, 3 Bit
falsch, ...). Endlichkeit wuerde naemlich bedeuten, dass eben bestimmte Polynome
aequivalent zu einem anderen Polynom sind, was wir nicht wollen (sonst koennten
fehlerhafte Bitsequenzen den urspruenglichen ident sein, was das ganze Konzept
von Fehlererkennung zu Nichte machen wuerde).

#### Praxis

Konkret machen wir bei CRC nun folgendes: Wir wollen also eine $n$-Bit Checksume
(Frame Check Sequence) berechnen, beispielsweise fuer die L2-Payload eines
Ethernet Frames. Die Daten bzw. die Codewoerter der Laenge $N$ Bit sind also
$\in F_q[x]: q = 2^N$. Dann waehlen wir uns ein Reduktionspolyom $r(x)$ vom Grad
$n$ ($n < N$), sodass der Rest der Division unserer Daten mit diesem
Reduktionspolynom also maximal Grad $n - 1$ hat und somit genau $n$ Bit
benoetigt ($x^0, ..., x^{n - 1}$).

Um nun die CRC-Checksum $c(x)$ unserer Daten $d(x)$ zu erhalten, multiplizieren
wir unsere Daten $d(x)$ zunaechst mit $x^n$, wo $n = \grad(r(x))$. Das ist
aequivalent dazu, die Bitsequenz $d(x)$ (also unser Datenwort), um $n$ Bit nach
links zu verschieben, da $x^n \equiv 1000...000$ mit $n$ Nullen ist. Das tun
wir, um Platz fuer die Checksum zu machen, welche ja $n$ Bit lang ist.

Dann fuehren wir die Polynomdivision $d(x) \div r(x)$ durch, wobei wir uns aber
fuer den Rest $d(x) \mod r(x)$ interessieren. Dieses Restpolynom ist naemlich
gerade unsere Checksum $c(x)$. Also $c(x) = d(x) \mod r(x)$. Es hat eben maximal
Grad $n - 1$ und benoetigt somit $n$ Bit Platz. Wir addieren diese Checksum
auf das verschobene Polynom drauf bzw. fuehren eine XOR-Operation auf der
verschobenen Bit-Sequenz mit der Rest-Bit-Sequenz durch. Die Nachricht $s(x)$,
welche wir dann senden, ergibt sich also aus $s(x) = d(x) \cdot x^n + c(x)$.

Wir senden diese Nachricht $s(x)$ also. Der Empfaenger macht dann folgendes:

1. Er kennt das selbe Reduktionspolynom $r(x)$.
2. Fuehrt dann auf den Daten wieder eine Polynomdivision durch.
3. Ist der Rest $s(x) \mod r(x) = 0$, so ist mit *hoher Wahrscheinlichkeit* kein
   Uebertragungsfehler passiert.
4. Ist der Rest ungleich Null, so ist *sicher* ein Fehler aufgetreten.

Wieso ist das so? Nachdem wir $d(x)$ mit $x^n$ multipliziert haben, bleibt bei
der Division also $c(x)$ Rest. Das bedeutet, dass bei der Polynomdivision unten
$c(x)$ stand. Da wir bei $s(x)$ aber gerade dieses Glied $c(x)$ noch
draufaddiert haben, wuerde bei einer erneuten Polynomdivision von $s(x)$ mit
$r(x)$ dieser Rest sich mit dem identen Teil aus $s(x)$ wegkuerzen, wodurch
sich 0 Rest ergibt. Wir runden das Polynom $d(x) \cdot x^n$ also quasi bis zum
naechsten Vielfachen von $r(x)$ auf.

Tritt ein Fehler bei der Uebertragung auf, wuerde also nicht Null Rest
bleiben. Ausser natuerlich in besonders unguenstigen Faellen. CRC kann bestimmte
Fehler naemlich nicht erkennen. Einen Fehler nicht zu erkennen bedeutet hierbei,
dass der Rest also trotz Veraenderung von $s(x)$ waehrend der Uebertragung Null
ist. Wir betrachten Fehler dabei immer als *Fehlermasken* der selben Laenge wie
$s(x)$, welche auf $s(x)$ draufaddiert/ge-XOR-ed werden. Die Fehler, die es
*nicht verlaesslich* erkennen kann, sind:

* Fehler, die laenger als $n$ sind.
* Fehler, die aus mehreren Bursts (isolierten Gruppen von mehr als einem Fehler-Bit)
  bestehen

Fehler, die auf Grund des Aufbaus von CRC *sicher nicht* erkannt werden koennen,
sind solche, die ein Vielfaches von $r(x)$ sind. Denn dabei wuerde der Rest
weiter gleich bleiben. Hierbei meint Vielfaches eine Mulitplikation mit einem
Term $x^k$ bzw. eine Verschiebung um $k$ Bits. $s(x)$ ist naemlich schon ein
Vielfaches von $r(x)$, also $s(x) = k(x) \cdot r(x)$ fuer ein Polynom
$k(x)$. Wenn wir nun ein weiteres Vielfaches Polynom $f(x) = k'(x) \cdot r(x)$
von $r(x)$ auf $s(x)$ draufaddieren, ist $s(x) + f(x) = (k(x) + l(x)) \cdot
r(x)$ natuerlich auch noch ein Vielfaches von $r(x)$, weswegen der Rest weiter
null waere. Eine Fehlermaske, die von CRC also sicher nie erkannt werden wuerde,
ist $r(x)$ selbst.

CRC erkennt jedoch immer:

1. Alle 1-Bit Fehler,
2. Alle isolierten 2-Bit Fehler,
3. Einige Burst-Fehler, die laenger als $N$ sind.

#### Beispiel

Wir betrachten ein Datenpolynom $d(x) = x^7 + x^5 + x^2 + 1$ von Grad $7$ sowie
ein Reduktionspolynom $r(x) = x^3 + x^2 + 1$ von Grad $n = 3$. Das entspricht
Bitsequenzen $10100101$ fuer die Daten und $1101$ fuer das
Reduktionspolynom. Die Checksum wird drei Bit lang sein. Dann:

1. Wir multiplizieren $d(x)$ mit $x^n = x^3$, verschieben die Bitsequenz also um
   3 Bit nach links und fuellen Nullen nach. Daraus ergibt sich: $10100101000$
2. Dann fuehren wir die Polynomdivision durch. Hierbei verwenden wir die
   XOR-Regeln als Addition (wobei Subtraktion = Addition im GF(2)):

```
10100101000 : 1101 = 11010101
1101 (XOR) (da die erste Ziffer im Polynom schon 1, ergibt oben erste 1)
----
0111 (da die 2. Ziffer eine 1 ist, ergibt es oben eine 1)
 1110 (schieben eine Ziffer nach, um genug Bits fuer die Maske zu haben)
 1101
 ----
 001110 (da die 2. Ziffer 0 ist, oben eine 0, und einen Bit nach schieben)
   1101 (dann geht 1 in 1 rein und es kommt oben eine 1 rein)
   ----
   001110
     1101
	 ----
	 001100
	   1101
	   ----
	   0001 = c(x)
```

Nun addieren wir unsere Checksum $c(x) = 1$ auf unser verschobenes Polynom
bzw. XOR-ren 001 auf unsere verschobene Bitsequenz (da diese unteren Bits vorher
Null waren schreiben/OR-en wir sie eigentlich einfach rein):

```
    10100101000 = d(x) * x^n
XOR 00000000001 = c(x)
    -----------
    10100101001 = s(x)
```

Der Empfaenger macht nun mit $s(x)$ wieder eine Polynomdivision:

```
10100101001 : 1101 = 11010101
1101
----
01110
 1101
 ----
 001110
   1101
   ----
   001110
     1101
	 ----
	 001101 <=== Hier ist der wichtige Unterschied
	   1101
	   ----
	   0000 <=== Kein Fehler
```

So sieht der Empfaenger also, dass kein Fehler aufgetreten ist. Nun geben wir
eine Fehlermaske `01001110001` zur Nachricht hinzu, was beispielsweise bei einer
Uebertragung (leider) passieren koennte:

```
10100101001
01001110001 XOR
-----------
11101011000 <= fehlerhafte Daten
```

Dann pruefen wir beim Empfaenger die Checksum (den ganzzahligen Teil der
Division brauchen wir in der Praxis uebrigens gar nicht zu beachten)

```
11101011000 : 1101 = 1010101
1101
----
001110
  1101
  ----
  001110
    1101
	----
	001100
	  1101
	  ----
	  0001
```

Der Rest ist nicht Null, der Empfaenger erkennt also, dass ein Fehler passiert
ist! Nun waehlen wir eine besonders unguenstige Fehlermaske, zum Beispiel $x
\cdot r(x)$, also `00000011010` (um einen Bit nach links geshiftet) und sehen,
was passiert. Zuerst bestimmen wir die fehlerhaften Daten:

```
10100101001
00000011010 XOR
-----------
10100110011 <= fehlerhafte Daten
```

Und fuehren wieder beim Empfaenger die Polynomdivision durch:

```
10100110011 : 1101
1101
----
01110
 1101
 ----
 001111
   1101
   ----
   001000
     1101
	 ----
	 01011
	  1101
	  ----
	  01101
	   1101
	   ----
	   0000 Rest
```

Wie man sieht, haben wir hier eine Fehlermaske zu den Daten hinzu getan, wobei
der Empfaenger dies gar nicht merken wuerde.

### Links

Wir wollen nun die verschiedenen Arten von Verbindungen oder *Links* auf Schicht
2 betrachten. Dabei gibt es grundsaetzlich zwei Varianten:

1. Hubs
2. Switches

Die naechsten Absaetze besprechen diese weiterfuehrend.

#### Hubs

Ein Hub ist ein enorm simpler Baustein in einem Direktverbindungsnetz, an
welchen mehrere Knoten angebunden sein koennen. Sendet ein Knoten $A$ an einen
Hub eine Nachricht, so wird diese an *jeden* anderen Knoten, der an diesem Hub
angeschlossen ist, weitergeleitet. Somit darf zu jedem Zeitpunkt auch immer nur
ein Knoten an einen Hub senden. Treffen mehr als eine Nachricht bei einem Hub
ein, kollidieren diese.

Sprechen wir also von *Kollisions-Domaenen*, so sind alle Knoten, die an einen
Hub angebunden sind, in einer Kollisions-Domaene. Dabei ist eine
Kollisions-Domaene, auch *Segment* genannt, ein Teil eines
Direktverbindungsnetzes, innerhalb welchem immer eine Kollision auftritt, sobald
gleichzeitig mehr als ein Knoten sendet.

Es gibt hier noch zwei Arten von Hubs:

* Aktive Hubs (Repeater): Diese verstaerken (amplifizieren) das Signal auf der
  physikalischen Schicht, ohne dabei die Rahmen irgendwie zu inspizieren. Sie
  sind fuer den Sender also *transparent* (er weiss nicht, dass es ihn gibt).
* Passive Hubs: Solche Hubs sind wirklich nur Sternverteiler -- man keonnte
  genauso gut die einzelnen Adern der Kabel verloeten.

Hubs werden in modernen Netzwerken aufgrund der impliziten Kollisionsdomaene
fast gar nicht mehr verwendet. Ein Switch (siehe unten) ist heutzutage billig
genug, um nur Switches und nicht Hubs zu verwenden.

#### Switch

Die zweite Art von Verbindungsknoten auf Schicht 2 sind *Switches*. Diese sind
um einiges intelligenter als Hubs. Ein Switch hat dabei verschiedene *Ports*,
auf welchen jeweils ein oder viele Hosts (wenn ein Hub angeschlossen ist)
angeschlossen sind. Der Switch hat dann das Ziel, zu lernen, auf welchen Ports
welche Hosts senden. Er wuerde dabei die MAC-Adressen der Hosts in seinen
*Switching-Table* einschreiben. Weiss der Switch erst einmal, wo alle Hosts
leben, kann er einen eingehenden Rahmen nur durch den Port weiterleiten, wo der
Destination Host laut Switching Table angemeldet ist. Dazu braucht der Switch
initial eine *Learning Phase*. Er arbeitet dabei wie folgt:

1. Kommt ein Rahmen bei einem Port an und der Switch hat die Source MAC-Adresse
   noch nicht in seine Switching Table eingetragen, so gibt er einen Eintrag
   fuer diese MAC-Adresse und den Eingangsport in den Table.
2. Dann prueft der Switch, ob die Destination MAC-Adresse in seinem Switching
   Table ist (initial ist dieser leer).
3. Wenn ja, sendet er den Rahmen nur an diesen Port. Das tut er dabei aber nur,
   wenn der Zielhost-Port nicht gleich dem Quellhost-Port ist, da das sonst
   bedeutet, dass in diesem Quell/Ziel Netzsegment irgendwo ein Hub ist und der
   Zielhost den Rahmen sowieso schon ueber diesen Hub bekommen hat (der Switch
   verwirft den Rahmen also einfach).
4. Wenn nicht, *broadcastet* er den Rahmen an alle seine Ports.

Eintraege in der Switching Table werden dabei mit einem Zeitstempel versehen und
periodisch invalidiert.

Was passiert nun, wenn mehrere Hosts zum Switch senden? Sofern diese Hosts alle
an verschiedene Ports des Switch gesendet haben, gar nichts bzw. nur Gutes! Der
Switch kann naemlich auf jedem Port ein Paket abfangen, eventuell kurz speichern
(puffern) und dann weitersenden. Switches haben daher die Eigenschaft,
Kollisiondomaenen zu *unterbrechen*. Betrachten wir beispielsweise einen Switch
mit nur zwei Ports -- auch *Bridge* genannt --- und je zwei Rechner auf beiden
Seiten, die durch je einen Hub an den Switch verbunden sind:

```
A         C
 \ 0   1 /
  H--S--H
 /       \
B         D
```

Es darf nun also auf jeder Seite, Port 0 und Port 1, gleichzeitig ein Host
senden. Der Switch wird die Pakete dann jeweils weiterleiten. Somit unterbricht
der Switch also die Kollisiondomaene in der Mitte:

```
     |
     |
     |
A    |    C
 \ 0 | 1 /
  H--S--H
 /   |   \
B    |    D
     |
     |
	 |
```

Wenn links oder rechts zwei Hosts gleichzeitig senden, z.B. $A$ und $B$, kommt
es noch immer zu einer Kollision. Wenn aber ein Host links und ein Host rechts
sendet, ist das kein Problem.

Ist pro Switchport genau ein Host angeschlossen, so spricht man von
*Microsegmentation* bzw. einem *vollstaendig geswitchem Netz*. Das ist heute der
Regelfall. Dann duerfen also je zwei Hosts gleichzeitig miteinander
kommunizieren (also, jeder mit jedem).

```
    |
  A | C
   \|/
----S----
   /|\
  B | D
    |
```

Switches koennen im Uebrigen auch dazu genutzt werden, Netzteile mit
verschiedenen Zugriffsverfahren zu verbinden! Beispielsweise kann am einen Port
CSMA/CD (Ethernet) verwendet werden und auf dem anderen Port Token Passing
(FDDI). Das ist fuer alle angeschlossenen Rechner bzw. Netzwerke transparent.

Anmerkungen:

* Switches sind wie Hubs fuer Hosts *transparent*, d.h. ein Host weiss nicht,
  dass zwischen ihm und seinem Zielhost ein Switch liegt.
* Sender- und Empfaengeradresse werden nie veraendert (nicht wie bei *Routern*).
* Ein Broadcast Paket wird an alle Ports weitergeleitet.

Es gibt auch noch zwei verschiedene Arten von Switches:

1. *Store-and-Forward*: Eingehende Rahmen werden vollstaendig empfangen und
   *deren FCS geprueft* (sowie dann gegebenfalls zurueckgesendet). Ist der
   Ausgangsport zum Zielhost belegt, koennen Rahmen gepuffert werden. Hierbei
   wird der vollstaendige Rahmen empfangen, bevor er weitergeleitet wird.
2. *Cut-Through*: Rahmen werden sofort, ohne Inspektion der FCS,
   weitergesendet. Insbesondere wird ein Rahmen schon weitergeleitet, sobald die
   Destination Address Bytes des L2-Headers (ersten 6 Byte) erhalten wurden.

https://en.wikipedia.org/wiki/Network_switch#Layer_2

Weitere Anmerkung:

In einem normalen (billigen) Switch laeuft alles out-of-the-box und man kann
auch weiter nichts konfigurieren. Man muss sich also nicht mit ihnen verbinden
koennen weil man mit ihnen auch gar nicht kommunizieren kann
(bzw. muesste). Teure high-end Switches haben ein eigenes Betriebssystem und
Management-Protokoll, sodass man sich einloggen kann (z.B. ueber SSH) um sie zu
konfigurieren (z.B. um FCS Pruefung ein/auszuschalten oder Puffer-Groessen
festzulegen). Solche Switches, auch *Managed Switches* genannt, haetten dann
auch eine eigene IP-Adresse.
