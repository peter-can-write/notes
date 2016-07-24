# OSI Modell: Layer 1

$\newcommand{\SNR}{\mathop{\rm SNR}\nolimits}$
$\newcommand{\nrz}{\mathop{\rm nrz}\nolimits}$
$\newcommand{\rz}{\mathop{\rm rz}\nolimits}$
$\newcommand{\manc}{\mathop{\rm manc}\nolimits}$
$\newcommand{\rect}{\mathop{\rm rect}\nolimits}$

## Signale und Information

Ein *Signal* ist eine *zeitabhängige*, messbare physikalische Größe. Dabei kann
man definierten messbaren *Signaländerungen* ein *Symbol* zuordnen. Diese
Symbole repräsentieren dann Information. Beispiele für Signale wären:

* Licht (Morsezeichen)
* Spannung (Telegraphie)
* Schall (Menschliche Sprache)

### Informationstheorie

#### Informationsgehalt

Betrachten wir eine Informationsquelle (ein Signal) sowie ein konkretes Symbol,
dass von dieser Quelle emittiert wird. Nehmen wir nun an, die Quelle emittiert
nur dieses eine Symbol. Wenn wir nun dieses eine Symbol von der Quelle erhalten,
haben wir dann daraus irgendwelche interessanten Informationen gewonnen? Nein,
denn wir haben es ja erwartet, da die Quelle nur dieses Symbol emittiert. Es
sagt uns also nichts Neues, somit ist der *Informationsgehalt* dieses Symbols
gering (bzw. Null). Nehmen wir nun an, die Quelle emittiert noch ein weiteres
Symbol (Zeichen), welches aber nur sehr, sehr selten vorkommt. Wenn wir nun
dieses seltene Symbol aus der Quelle erhalten, dann muss das ein besonderes
Ereignis sein. Es enthält also mehr Information, über irgendein besonderes
Geschehnis. Insofern wäre der Informationsgehalt dieses Symbols also größer. Die
weniger seltenen Symbole würden auf "nichts Neues" schließen lassen, also
wiederum wenig Information enthalten.

Dieses Konzept kann man nun auch formalisieren. Wir betrachten hierzu die
Wahrscheinlichkeitsverteilung $p(x)$, welche angibt, wie wahrscheinlich es ist,
dass das nächste emittierte Symbol zu einem beliebigen Zeitpunkt gerade $x \in
\mathbb{X}$ ist, wo wir mit $\mathbb{X}$ die Menge aller möglichen Symbole
bezeichnen. Wir wollen nun ausdrücken, dass wenn die Wahrscheinlichkeit für ein
Symbol hoch, sein Informationgehalt niedrig ist und umgekehrt. Dafür verwenden
wir nun die Logarithmusfunktion, welche die Wahrscheinlichkeit sozusagen
invertiert. Für niedrige Werte, also kleine Wahrscheinlichkeiten $p(x)$ bei
seltenen Symbolen, wird $\log(p(x))$ ein größerer Wert sein, als bei einem
großen $p(x)$ bei häufiger auftretenden Symbolen. Der mathematische Hintergrund
hierfuer ist, dass kleine Wahrscheinlichkeiten hoehere bzw. negativere
Zehner-Potenzen sind. So ist $\log_{10}(10^{-10}) = -10$ und $\log_{10}(10^{-2})
= -2$. Da für Werte kleiner Eins der Logarithmus bei jeder Basis immer negativ
ist, negieren wir für den eigentlichen Informationsgehalt $I(x)$ eines Symbols
noch den Wert des Logarithmus: $I(x) = -\log(p(x))$. Hierbei ist also
$-\log(p(x)) = |\log(p(x))|$. Es gilt im Uebrigen, dass $\log(1/x) = -\log(x)$
fuer $x \in \mathbb{R}$.

Wir können dabei die Basis des Logarithmus prinizipell selbst wählen, wobei sich
die exakten Werte sowie Einheiten jedoch verändern würden. Wählen wir den
Logarithmus Dualis mit Basis 2, so erhalten wir den Informationsgehalt in
*Bits*. Wählen wir den natürlichen Logarithmus $\ln$, geben wir die Werte in
*Nats* an. Wir verwenden in der Vorlesung die Basis 2.

Was uns der Wert des Informationsgehalts noch sagt, ist, wieviele Bits man bei
einer idealen Quelle benötigen würde, um ein Symbol zu kodieren. Emittiert ein
Symbol immer das selbe Zeichen, so ist der Informationsgehalt des Zeichens
gleich null. Das bedeutet, dass wir prinzipiell das Symbol gar nicht erst zu
senden brauchen, weil es sowieso eindeutig ist, dass es als nächstes emittiert
würde. Emittiert eine Quelle aber viele Symbole, so würde man für häufige
Symbole schon auch Bits brauchen, um sie zu kodieren. Die seltenen Symbole
müsste man hierbei aber mit *mehr Bits* kodieren. Man stellt sich hierfür am
bestene eine unäre Kodierung der Symbole vor. Die häufigen Symbole kodiert man
mit wenigen Bits (/Strichen -> Unär), da man sie über die Quelle häufiger senden
muss. Die seltenen Symbole verteilen wir auf mehr Bits, welche teurer zu
übertragen sind.

#### Entropie

Die *Entropie* $H(\mathbb{X})$ einer Informationsquelle $Q$ beschreibt nun den
*mittleren* Informationsgehalt aller von dieser Quelle emittierten Symbole
$\mathbb{X}$. Dieser Wert wird also berechnet, indem man die Wahrscheinlichkeit
$p(x)$ jeden Symbols $x$ mit seinem Informationsgehalt $I(x)$ multipliziert, und
so über alle Symbole $x$ summiert:

$$H(\mathbb{X}) = \sum_{x \in \mathbb{X}} p(x)I(x) = -\sum_{x \in \mathbb{X}}
p(x)\log(p(x))$$

Wir berechnen also gerade den Erwartungswert $E_{x \sim p}[I]$ des
Informationsgehalts der Symbole. Der resultierende Wert, welcher ebenso in Bits
gemessen wird, beschreibt zwei Dinge:

* Die Unsicherheit bezüglich Veränderungen des Signals. Je höher die Entropie,
  desto höher ist also der gesammelte Informationsgehalt der Symbole bzw. desto
  uniformer ist auch die Wahrscheinlichkeitsverteilung $p$. Erhalten wir also
  eine hohe Entropie, bedeutet das, dass wir uns weniger sicher sein können,
  welches Symbol denn als nächstes von der Quelle emittiert wird. Ist die
  Entropie niedrig, können wir schon eher erahnen, wie die Quelle sich verhalten
  wird. Die Quelle hätte also insgesamt geringeren Informationsgehalt.
* Die durchschnittiche Anzahl an Bits, mit welchen man Symbole in einem idealen
  Code für diese Quelle kodieren müsste.

Man kann die Entropie in der Informationstheorie auch gut mit der Entropie in
der Thermodynamik beschreiben. Die Entropie eines thermodynamischen Systems
sagt aus, wieviele verschiedene Konfigurationen der Moleküle in dem System es
gibt. In einem unruhigen System, beispielsweise Gas, ist die Entropie hoch, da
die Gasmoleküle sich auf sehr viele Weisen konfigurieren können. Die
*Unsicherheit* über die nächste Konfiguration ist also hoch, ebenso wie die
Entropie einer Informationsquelle die Unsicherheit bezüglich dem als nächstes
emittierten Symbol beschreibt. In einem Kristall sind Moleküle hingegen starr.
Ihre Entropie wäre also niedrig, weil es nur eine Konfiguration des Systems
gibt.

##### Maximale Entropie

Wir fragen uns: Wie hoch kann die Entropie einer Quelle $Q$ mit Signalen
$\mathbb{X}$ maximal sein?

Zunaechst: wie hoch muessen die Wahrscheinlichkeiten sein? Intuitiv ist klar:
sie muessen alle gleich sein, die Wahrscheinlichkeitsverteilung muss also
uniform sein. Dann ist die Unsicherheit darueber, welches Symbol die Quelle als
naechstes emittiert, gerade am groessten, weil jedes der vielen Symbole gleich
wahrscheinlich ist. Mathematisch kann man es natuerlich durch Ableiten finden.

Wenn wir nun also wissen, dass die Wahrscheinlichkeit fuer eine maximale
Entropie uniform, also $1/n$ bei $n$ Signalen, sein muss, koennen wir die
Entropieformel leicht umformen:

$$H_{max}(\mathbb{X}) = -\sum_{x \in \mathbb{X}} p(x)\log(p(x)) = -\sum_{x \in
\mathbb{X}} 1/n \cdot \log(1/n) = 1/n \cdot (-(n \cdot \log(1/n))) = -\log(1/n) =
\log(n)$$

Die maximale Entropie $H_{max}$ ist also gerade immer gleich $\log(n)$, wo $n$
die Anzahl moeglicher Symbole ist. Da die Entropie ja den durchschnittichen
Informationsgehalt angibt, und dieser bei einer uniformen Wahrscheinlichkeit
jedes Symbols immer gleich ist, ist in diesem Fall die maximale Entropie auch
gleich dem Informationsgehalt jedes Symbols. Das bedeutet also, dass wenn wir
von maximale Entropie ausgehen, uns $\log(n)$ Bits genuegen, um alle unsere
Symbole zu kodieren.

Umgangssprachlich meint man ja beispielsweise oft, dass man fuer 256
verschiedene Werte eben 8 Bit braucht. Das ist aber theoretisch nur der Fall,
wenn jedes Symbol die selbe Auftrittswahrscheinlichkeit hat, sodass die Entropie
maximal ist. Denn nur dann ist $H(Q) = H_{max}(Q) = \log(|Q|)$

## Klassifizierung von Signalen

Wenn wir nun ein konkretes Signal $s(t)$ in Abhängigkeit des Zeitfortschritts
$t$ betrachten, müssen wir uns noch entscheiden, welchen Werten oder
Veränderungen des Signals wir welche Symbole zuordnen. Wir brauchen also eine
Abbildung vom momentanen Signalwert auf ein Symbol. Wir könnten für ein binäres
Alphabet $\mathbb{X} \in \{0, 1\}$ beispielsweise einfach die folgende Abbildung
wählen:

$$x_t = \begin{cases}0, \text{ wenn } s(t) \leq 0\\1, \text{
sonst}\end{cases}.$$

Jetzt stellen sich aber viele Fragen:

* In welchen Zeitabständen betrachten wir jeweils das Signal, um ein neues
  Symbol aufzunehmen? Theoretisch gäbe es für ein beliebig großes Intervall
  unendlich viele Symbole, da es unendlich viele Zeitpunkte $t$ in einem solchen
  kontinuierlichen Zeitintervall gibt. Da wir sicher nur endlich viele Symbole
  aufnehmen wollen (und auch speichern können), müssen wir die Zeit also
  *diskretisieren* und festlegen, wann wir *Samples* nehmen.
* Erhalten wir mehr Information, wenn wir öfter *abtasten*, also öfter Samples
  aufnehmen?
* Da bei einem Signal nicht nur die Zeit, sondern auch der Wertebereich
  (beispielsweise bei elektrischer Spannung) kontinuierlich sein kann, müssen
  wir wohl auch hier den Wertebereich diskretisieren oder abtasten. Den Begriff
  abtasten benutzt man aber nur für die Zeitachse. Für Signalwerte, also die
  Wertachse, nennen wir solches Abrunden von kontiniuerlichen Werten auf endlich
  viele, diskrete Intervalle (Bins/Buckets) *Quantisierung*.
* Auf welche Weise sollen längere Folgen von Symbolen interpretiert werden?
  Möglicherweise wollen wir nicht nur einzelne Zeichen senden, sondern
  irgendwelche Arten von *Paketen*, welche sowohl Daten als auch andere
  Informationen enthalten könnten (z.B. Steuerinformationen).
* Wie behandeln wir die Tatsache, dass Signale in der echten Welt nicht immer
  ideal, sondern oft verrauscht, gedämpft oder verzerrt sind?
* Wie wird Fehlern bei der Übertragung vorgebeugt?
* Wie wird ein Signal überhaupt erzeugt und von $A$ nach $B$ gesandt?

### Zeit- und Frequenzbereich

Nach dem *Fourier-Theorem* lassen sich alle periodischen Zeitsignale als
Überlagerungen von gewichteten Sinus- und Kosinusschwingungen unterschiedlicher
Frequenzen auffassen. Haben wir dabei ein Signal $s(t)$ in Abhaengigkeit der
Zeit $t$, so koennen wir nach Fourier dieses Signal durch eine *Fourier-Reihe*
ausdrucken:

$$s(t) = \frac{a_0}{2} + \sum_{k=1}^\infty (a_k \cos(k\omega t) + b_k \sin(k
\omega t))$$

Hierbei bezeichnet man das $k$-te Summenglied auch als die $k$-te
Harmonische. $a_k$ und $b_k$ sind dann die Gewichtungen fuer die Sinus- und
Kosinuskurven, welches nach Fourier diese $k$-te Harmonische ausmachen.  Das
konstante Glied $\frac{a_0}{2}$ wird hierbei auch *Gleichanteil* genannt, und
drueckt eine Verschiebung der Signalamplitude bezueglich der Ordinate (y-Achse)
aus. Es ist gewissermassen der Mittelwert der Funktion, zu welchem die
Schwingung immer wieder zurueckkehrt. $\omega$ drueckt in der Formel die
*Kreisfrequenz* $2\pi f$ bzw. $2\pi/T$ aus und normalisiert die Periode der
Schwingung auf das Intervall $T$. Ohne diese Kreisfrequenz $\omega$ wurde die
Schwingung eine Periode von $2\pi$ haben.

Kennen wir das Signal $s(t)$ schon, so koennen wir die Koffizienten $a_k$ und
$b_k$ des $k$-ten Gliedes berechnen. Wenn man das fuer alle $k \in \{1, ...,
\infty\}$ macht, so nennt man das eine *Fourier-Transformation*. Konkret
existieren folgende Formeln fuer das Kosinusgewicht $a_k$ und Sinusgewicht
$b_k$:

$$a_k = \frac{2}{T}\int_0^T s(t)\cos(k\omega t) \mathrm{dt} \text{ und } b_k =
\frac{2}{T}\int_0^T s(t)\sin(k\omega t) \mathrm{dt}$$

Der Gleichanteil ist hierbei $a_0/2$ und nicht $a_0$, um negative Frequenzen
unter Betracht zu ziehen. Negative Frequenzen gibt es nicht wirklich, sie sind
eher nur ein mathematisches Konstrukt das uns mathematische Berechnungen, vor
allem mit komplexen Zahlen, erleichtert. Wir addieren sie in der Praxis einfach
zu den positiven Frequenzen (klappen das Spektrum am 0-Punkt um). Deswegen ist
der Gleichanteil, symbolisch bzw. zur Veranschaulichung, $a_0/2$ und nicht
$a_0$. *Beachte also*: $a_0/2$ ist der Gleichanteil (wirklich die Verschiebung
entlang der Ordinate), $a_0$ ist lediglich der erste Koeffizient.

### Diskretisierung

Natuerliche Signale sind sowohl *zeitkontiniuerlich* (es gibt fuer jedes
Intervall ueberabzaehlbar unendlich viele Zeitpunkte $t$) sowie auch
*wertkontinuierlich* (das Signal kann in jedem Intervall des Wertebereichs
ueberabzaehlbar unendlich viele Werte aus diesem Intervall annehmen). Wir
koennen auf einem Computer aber natuerlich nur endlich viele Symbole speichern
und bearbeiten, da ein Computer nur endlich viel Speicher und auch nur endliche
Rechengenauigkeit hat.

Wir muessen Signale also diskretisieren. Im Zeitbereich nennt man das
*Abtastung* und im Wertbereich *Quantisierung*. Ist ein Signal erst einmal in
beiden Bereichen diskretisiert, so koennen wir es *digital* speichern. Wir
koennen dann also fuer jedes diskrete Zeitintervall $[t_i, t_i + 1]$ einen
diskreten Wert festhalten und in einem Datenwort fester Laenge (z.B. 16 Bits)
speichern.

#### Abtastung

Beim Abtasten legen wir ein Abtastintervall $T_a$ fest, sodass wir immer, wenn
der kontinuierliche Zeitwert $t$ ein ganzzahliges Vielfaches von $T_a$ ist, den
entsprechenden Wert des Signals $s(t)$ zu diesem Zeitpunkt erhalten. Wenn $t$
aber kein ganzzahliges Vielfaches von $T_a$ ist, so soll unsere *Abtastfunktion*
eine Null zurueckgeben. Wir bezeichnen dabei die Abtastfunktion fuer das Signal
$s(t)$ durch $\hat{s}(t)$. Sie ist wie folgt definiert:

$$\hat{s}(t) = s(t)\sum_{n=-\infty}^\infty \delta[t - nT_a]$$

Hierbei ist $\delta[t]$ der *Dirac*- oder *Delta-Impuls*, welcher genau fuer $t
= 0$ einen Wert von $1$ annimmt, und sonst immer $0$. Er ist nuetzlich, um Werte
einer kontinuierlichen Funktion nach irgendeiner Bedingung "auszuwaehlen". Das
ist auch gerade was wir hier machen wollen. Betrachten wir zum Verstaendnis
dieser Funktion einen konkreten Zeitpunkt $t$. Wir berechnen zuerst den
Signalwert $s(t)$ des kontinuierlichen Signals $s$, welches wir diskretisieren
wollen, zum diesem Zeitpunkt $t$. Dann gibt die Summe konzeptuell alle
Zeitpunkte $nT_a$ an, zu welchen wir abtasten wollen. Indem wir in die Dirac
Funktion $t - nT_a$ einsetzen, erhalten wir folgende Situation: wenn $t$ gerade
einer dieser Abtastpunkte $nT_a$ ist, dann wird $\delta[t - nT_a] = \delta[0] =
1$ sein und der Signalwert wird aufgenommen. Hierbei werden dann aber auch alle
anderen Summenglieder $0$ sein, da fuer alle $m \neq n$ gelten wird: $t - mT_a
\neq 0$. Wenn $t$ aber mit keinem dieser Abtastpunkte uebereinstimmt, dann wird
die Summe fuer alle Glieder null sein, und $s(t) \cdot 0$ wird uns also keinen
Signalwert geben. Der Koeffizient fuer $s(t)$, also die ganze Summe, ist also
eins, immer dann wenn $t = nT_a$ fuer irgendein $n \in \mathbb{Z}$, und sonst
Null. Wir probieren also fuer jeden Zeitpunkt $t$ alle moeglichen $nT_a$ von
minus unendlich bis plus unendlich aus (mathematisch ist das ja leicht
realisierbar). Wenn eines passt, wird der Wert aufgenommen. Sonst nicht.

So erhalten wir also eine mathematische Darstellung davon, was es bedeutet, ein
Signal bezuglich der Zeit zu diskretisieren. Wir benutzen im Weiteren dann, wenn
fuer $\hat{s}(t)$ gerade gilt $t = nT_a: n \in \mathbb{Z}$ die Notation
$\hat{s}[n]$. Dann ist also $\hat{s}[n] = \hat{s}(nT_a)$.

#### Rekonstruktion

Haben wir erst einmal die Abtastwerte, so ist es unter Umstaenden moeglich, das
urspruengliche Signal wiederherzustellen -- also zu rekonstruieren. Hierbei sagt
uns das Nyquist-Shannon Abtasttheorem (Nyquist-Shannon Theorem oder einfach
Nyquist Theorem), dass wenn unser Signal auf eine Bandbreite $B$ begrenzt, ist
sodass alle Frequenzen $f$ der Sinus- und Kosninuskomponenenten kleiner gleich
$B$ sind, erst eine Abtastrate

$$f_a \geq 2B$$

eine korrekte Rekonstruktion ermoeglicht. Die Abtastrate muss also groesser als
die groesste vorkommende Frequenz im Signal sein. Analog kann man sagen, dass
die Abtastperiode oder das Abtastintervall $T_a$ zwei mal so klein sein muss,
wie die kleinste Periode im Signal. Die Intuition dahinter ist, dass wir fuer
ein Signal mindestens einen Punkt ueber dem Equilibrium und einen Punkt unter
dem Equilibrium (aber noch in der selben Periode) benoetigen, um das Signal
richtig zu rekonstruieren.

Waehlen wir eine Abtastfrequenz, die kleiner als die hoechste Frequenz im Signal
ist, so kann es vorkommen, dass wir anstatt dem urspruenglichen Signal ein
anders rekonstruieren. Dieses faelschlicherweise rekonstruierte Signal haette
also gerade die selben Abtastpunkte, wie das urspruengliche Signal (welches wir
nicht darstellen konnten). Daher spricht man bei dem falschen Signal von einem
*Alias* des urspruenglichen Signal. Deswegen sagt man, dass wenn die
Abtastfrequenz zu niedrig gewaehlt wurde, das Phaenomen des *Aliasing* auftritt,
wobei Signale eben durch ihre Aliase "ersetzt" werden.

Sehen wir uns das Frequenzspektrum an, so koennen wir die Folgen von Aliasing
auch erkennen. Hierzu sei zuerst erklaert, wie sich Abtastung auf das
Frequenzspektrum eines Signals auswirkt. Beschraenken wr unser Signal auf eine
Bandbreite von maximal $B$ Hertz, so haetten wir fuer das ursprunegliche,
kontinuierliche Signal also genau ein einziges Segemnt im Frequenzspektrum, wo
die Amplitude nicht null ist. Da es positive wie auch negative Frequenzen gibt
(zumindest im Theoretischen), ist dieses Segment eigentlich $2B$ Hertz breit
($B$ positive und $B$ negative Hertz). So hat eine Abtastung mit einer Rate von
$f_a$ Hertz zur Folge, dass dieses $2B$-Hertz breite nach links und nach rechts
ins Unendliche um jeweils $f_a$ Hertz verschoben wird. Waehlen wir nun $f_a$
kleiner als $2B$, so passiert es, dass diese Verschiebungen miteinander
*ueberlappen*. Durch Ueberlagerung der einzelnen Frequenzamplituden ergibt sich
ein neues Frequenspektrum, welches aber nun ident zu dem ist, welches wir
erhalten haetten, wenn wir den Alias des Signals korrekt abgetastet haetten.

Man erkennt aus der Analyse des Frequenzbereichs auch, wieso $f_a$ groesser
*zwei* mal der hoechsten Frequenz sein muss, und nicht groesser der hochsten
Frequenz. Wenn wir $f_a = f_{max}$ waehlen, so verschieben wir unser auf
$f_{max}$ beschraenktes Spektrum also um jeweils $f_a$ nach links und rechts. Da
wir aber auch negative Frequenzen, zumindest im Theoretischen, betrachten
muessen, wurden sich genau bei $f_{max}$ der positive Teil des Spektrums mit dem
negativen Teil des ersten nach rechts verschobenen Segments ueberlappen, und die
Amplitude waere verdoppelt.

#### Quantisierung

Unter *Quantisierung* versteht man die Diskretisierung des urspruenglich
kontiniuerlichen Wertebreichs eines Signals. Betrachtet man beispielsweise das
Sinussignal $\sin(x)$, so kann dies uaberabzaehlbar unendlich viele Werte aus
dem Intervall $[-1, 1]$ annehmen. Wir koennen aber mit unseren guten 64-Bit IEEE
754 double-precision floating-point werten aber niemals mit perfekter Praezision
jeden infinitesimal kleinen Wert aus diesem Intervall darstellen. Folglich
muessen wir uns auch hier ausdenken, wie wir diesen Wertebereich in diskrete
Stufen aufteilen.

Allgemein speichern wir Datenwoerter natuerlich mit einer bestimmten Bit-Breite
$N$. Dann koennen wir also maximal $M = 2^N$ Symbole aus einem Wertebereich
darstellen. Nun fragt es sich noch, wie wir diese Symbole auf den Wertebereich
*sinnvoll* verteilen.

Die einfachste Moeglichkeit nennt man *lineare Quantisierung*. Wenn wir einen
Wertebereich $[a, b]$ haben, dann teilen wir diesen Wertebereich einfach in $M -
1$ aequidistante Intervalle der Groesse $\Delta = \frac{b - a}{M}$, wobei die
diskreten Werte dann die $M$ Intervallgrenzen sind. Wenn wir immer zum naechsten
Quantum (Intervallgrenze) runden, ergibt sich dabei ein maximaler
*Quantisierungsfehler* $q_{max}$ von $\Delta/2$. Man beachte natuerlich, dass
dies nur fuer Werte $\in [a, b]$ gilt. Ausserhalb ist der Quantisierungsfehler
unbeschraenkt.

Ausserdem beachte man, dass man bei der linearen Quantisierung die diskreten
Werte im optimalen Fall gar nicht an die Intervallsgrenzen gibt, sondern um
$\Delta/2$ nach innen "staucht". Sonst haette man genau im Nullpunkt naemlich
einen Quantisierungsfehler von $\Delta$, da die naechsten Intervallgrenzen bei
$+\Delta$ und $-\Delta$ waeren (im Nullpunkt selbst ist kein diskreter
Wert). Dadurch, dass wir die Werte um $\Delta/2$ nach innen verschieben, ist der
Quantisierungsfehler in jedem Intervall maximal $\Delta/2$.

Als Beispiel in der echten Welt: Telefonverbindungen quantisieren nicht
gleichmaessig (linear). Eher erhalten niedrigere Amplituden mehr Intervalle als
hoehere, weil es sich auf Grund der Eigenheiten des menschlichen Hoerspektrums
lohnt, fuer niedrigere Frequenzen eine hoehre Aufloesung zu haben. Bestimmte
Wertebereiche haben also mehr Quantisierungstufen als andere.

## Uebertragungskanal

In der realen Welt wird ein Signal nicht unbeeinflusst von $A$ nach $B$
gesendet. Es kommen noch Stoerfaktoren hinzu, zum Beispiel:

* Daempfung: Die Signalamplitude kann waehrend der Uebertragung reduziert
  werden.
* Tiefpassfilterung: Hohe Frequenzen gehen verloren.
* Verzoegerung: Die Uebertragung benoetigt eine gewisse Zeit.
* Rauschen: In Form von Additive White Gaussian Noise.

Wir betrachten hierfuer ein vereinfachtes Uebetragungsmodell:

``` x --- [Kanal] --- + --- y | n ```

Hierbei ist $n$ eine Stoerquelle (eigentlich $\eta$).

### Kanalkapazitaet

Wir nehmen an, dass ein Kanal wie ein Tiefpassfilter wirkt. Das heisst,
Frequenzen ab einer bestimmten Threshold/Cutoff Frequenz $f_0$ werden
gedaempft. Man kann also gewissermassen von einer *Kanalbandbreite* $B$
sprechen. Dann gilt:

* Frequenzen unter der Bandbreite (bzw. darin) gehen ungehindert durch den
  Kanal.
* Hoehere Frequenzen werden gedaempft.
* Ab einer bestimmten Frequenz sind Signale dann vernachlaessigbar klein.

Vereinfacht nehmen wir dabei an, dass die Grenze bei $B$ scharf ist, sprich der
Filter ist perfekt. Dann werden also alle Frequenzen $f$ mit $|f| < B$
durchgelassen, und alle Frequenzen $|f| \geq B$ werden vollstaendig gesperrt,
erhalten also eine Amplitude von Null.

https://en.wikipedia.org/wiki/Bandwidth_(computing)
https://en.wikipedia.org/wiki/Goodput

#### Rauschfreier, Binaerer Kanal

Das Nyquist Theorem sagt uns, dass wir fuer ein auf $B$ bandbegrenztes Signal
eine minimale Abtastfrequenz von $2B$ brauchen. Das bedeutet aber auch, dass
$f_a = 2B$ (unter der Annahme perfekter Interpolationsalgorithmen) auch
ausreicht, um ein solches bandbegrenztes Signal vollstaendig zu
rekonstruieren. Insofern erhalten wir also durch eine hoehere Abtastrate auch
keine neuen Informationen, da wir mit $f_a = 2B$ schon ausreichend schnell
abtasten. Anders kann man sagen, dass wir in einer bestimmten Zeiteinheit also
auch nicht mehr als $2B$ unterscheidbare und *unabhaengige* Werte aufnehmen
koenenn. Das wichtige ist hierbei *unabhaengig*. Wir koennen natuerlich mit
$100B$ fuenfzig mal mehr Samples aufnehmen. Da wir aber mit $f_A$ das Signal
schon vollkommen rekonstruieren koennen, sind diese zusaetzlichen Samples
sozusagen alle von den $2B$ abhaengig und insofern redundant. Wir brauchen
hierbei dann immer zwei Samples pro Signalaenderung, um den Zustand sowohl des
Sinus- als auch Kosinusanteils des Signals zu erfassen, da das Signal ja eine
Mischung aus beiden sein kann.

Deswegen gibt uns die Nyquist Rate $f_N = 2B$ zwei Informationen ueber einen
__binaeren__ Kanal:

* Die minimale Abtastfrequenz, um das Signal korrekt zu rekonstruieren.
* Die maximale Anzahl an unterscheidbaren und unabhaengigen Signalwerten.

#### Rauschfreier, $M$-aerer Kanal

Wir nehmen nun an, dass unsere Quelle mehr als nur zwei Symbole emittieren kann,
naemlich $M = 2^N$ viele. Dann gilt, dass die Kanalkapazitaet des Kanals gleich
$f_N \cdot H(Q)$ ist, wo $H(Q)$ die Entropie der Quelle ist. Wir wissen nun
aber, dass die Entropie einer Quelle maximal $\log(M) = N$ ist. Somit sagt uns
*Hartley's Gesetz*, dass die maximale Kanalkapazitaet, also die maximale Anzahl
an Symbolen die wir in einem Zeitintervall senden koennen, gleich

$$C_H = f_N\log(M)$$

ist. Wir wollen dies intuitiv verstehen. Wir wissen, dass wir bei einem binaeren
Kanal maximal $f_N = 2B$ Symbole ueber einen auf $B$ bandbegrenzten Kanal senden
koennen. Jetzt haben wir aber mehr als zwei moegliche Symbole
bzw. Signalzustaende. Wenn die Entropie der Quelle maximal ist, wissen wir, dass
wir genau $\log(M)$ Bits brauchen, um unsere Symbole optimal zu kodieren. Wenn
wir also $2B$ viele Symbole uebertragen koennen, koennen wir insgesamt $2B \cdot
\log(M)$ viele Bits durch diesen Kanal schleussen. Denn jedes Symbol ist ein
Zustand, und ein Zustand ist eine Konfiguration der Bits. Wenn wir also 16
Symbole haben, hat ein Signalzustand 4 Bit an Information. Deswegen haben wir
$2B$ moegliche Symbol, die je $\log(M)$ Bit an Information tragen koennen. Das
ist gerade, was uns Hartley's Gesetz ueber die maximale Kapazitaet eines Kanals
sagt.

Nun zwei weitere Beobachtungen:

1. Wenn die Entropie $H(Q)$ nicht maximal ist, bedeutet das, dass die
   Wahrscheinlichkeit der Symbole nicht uniform ist. Im Weiteren heisst das,
   dass manche Symbole eine hoehere, insbesondere manche aber eine kleinere
   Wahrscheinlichkeit, als $1/n$ haben. Diese haetten dann auch einen hoeheren
   Informationsgehaelt bzw. braeuchten mehr Bit, um kodiert zu werden. Wenn sie
   mehr Bit brauchen, koennen wir im schlimmsten Fall wohl auch weniger Symbole
   aus dieser Quelle senden. Das spiegelt sich gerade dadurch wieder, dass $f_N
   \cdot H(Q)$ dann geringer waere (weil die Entropie geringer waere
   bzw. bestimmte Symbole, mit hoeherer Gewichtgung $p(x)$ dann eben mit weniger
   Bit kodiert wuerden).
2. Wenn $M = 2$ funktioniert Hartey's Gesetz natuerlich auch: $C_H f_N\log(2) =
   f_N \cdot 1 = f_N$, wie oben beschrieben.

### Rauschen

Rauschen tritt auf, wenn das Signal durch externe Stoereinfluesse
beeintraechtigt wird. Rauschen macht insbesondere unserer Quantisierung das
Leben schwer, da Signalstufen schwieriger auseinanderzuhalten sind. So koennte
Rauschen ein Symbol ein oder zwei Quantisierungsstufen nach oben oder unten
schieben und das Signal vollkommen verfaelschen. Hierbei ist natuerlich die
Verwendung von Gray-Codes ratsam, wo nebeneinander liegende Signalstufen eine
maximale Hamming Distanz von 1 haben (unterscheiden sich in maximal einem Bit).

Wir betrachten hierbei das sogenannte *Signal to Noise Ratio* (SNR):

$$\SNR = \frac{\text{Signalleistung}}{\text{Rauschleistung}} =
\frac{P_S}{P_N} .$$

Das SNR wird dabei in Dezibel (dB) angegeben. Ein Bel ist dabei der
Zehner-Logarithmus eines Verhaeltnisses von Leistungen. Hat man also zwei
Leistungswerte (Powerwerte) $a$ und $b$ bzw. deren Verhaeltnis $a/b$, so
entspricht ein Bel dem Faktor 10 zwischen $a$ und $b$, da dann $\log_{10}(a/b) =
\log_{10}(10b/b) = \log_{10}(10) = 1$. Bzw. waeren das dann 10 Dezibel. Eine
Veraenderung um Faktor 10 der beiden Werte entspricht also 10 Dezibel bzw. einem
Bel. Also gilt dann, dass das SNR in Bel definiert ist durch: $10 \cdot
\log_{10}(P_S/P_N)$. Ist das SNR nun klein, ist das schlecht. Ist es gross, ist
das gut.

#### Beispiel

Sei die Signalleistung $P_S = 1 \text{ mW}$ und die Rauschleistung $P_N = 0.5
\text{ mW}$. Dann ist das SNR gleich

$$10 \cdot \log_{10}(1/0.5) \approx 0.3 .$$

#### Shannon-Hartley Theorem

Wenn wir nun das $\SNR$ wissen, gibt uns das Shannon-Hartley Theorem die
maximale Kanalkapazitaet $C_S$ eines rauschbehafteten, $M$-aeren Kanals durch:

$$C_S = B \log_2(1 + \SNR) = B \log_2(1 + \frac{P_S}{P_N}) .$$

Hierbei ist das $\SNR$ nicht in Dezibel gemessen, sondern einfach das
Verhaeltnis. Das $+ 1$ im Logarithmus ist die Varianz bzw. Leistung des
Rauscheinflusses $\eta$.

### Schranken der Kanalkapazitaet

Die Kapazitaet $C$ eines Kanals ist also durch zwei Faktoren beschraenkt:

1. Die Anzahl $M$ der unterscheidbaren Symbole. Wenn wir zu wenige
   unterscheidbare Symbole haben, dann kann unsere Kanalkapazitaet auch nicht
   sehr gross sein (Hartley-Theorem).
2. Das SNR. Ist das SNR zu klein (zu viel Rauschen relativ zur Signalleistung),
   muss der Abstand $\Delta$ zwischen Quantisierungsstufen moeglicherweise
   erhoeht werden. Dann koennen wir aber auch weniger Symbole senden, die
   Kanalkapazitaet waere also auch reduziert. Das erkennt man auch am
   Shannon-Hartley Theorem.

Deswegen gilt, fuer die Kanalkapazitaet:

$$C < \min(C_H, C_S) = \min(2B\log(M), B\log(1 + SNR)) \text{ bit}$$

Das Hartley Theorem mit $C_H$ begrenzt hierbei die Kanalkapazitaet anhand der
Bandbreite des Kanals, sowie insbesondere anhand der Anzahl an Signalstufen. Das
Shannon-Hartley Theorem begrenzt den Kanal dann anhand des Rauscheinflusses,
sowie wiederum der Bandbreite des Kanals. Zusammen ergeben sie also die obere
Schranke.

## Nachrichtenuebertragung

### Quellenkodierung

*Quellenkodierung* (*Source Coding*) ist der erste Schritt in unserem Modell der
Datenuebertragung. Das Ziel der Quellenkodierung ist es, Redundanz aus den zu
uebertragenden Daten zu entfernen, durch Abbildung von Bitsequenzen auf
Codewoerter. Dies sollte verlustlos sein, entspricht also einer verlustlosen
Datenkompression. Das ist aber nicht immer moeglich oder erwuenscht (da
verlustbehaftete Kompression meist weniger Speicherplatz erfordert).

Quellenkodierung passiert meist auf hoeheren Schichten, beispielsweise schon in
der Darstellungsschicht (6). Hierbei koennte man Daten zum Beispiel durch das
ZIP Dateiformat komprimieren.

### Kanalkodierung

Die Kanalkodierung folgt nach der Quellenkodierung. Daten werden in der realen
Welt nicht ohne Fehler uebertragen. Das *Ziel der Kanalkodierung ist es nun, den
Daten absichtlich Redundanz hinzuzufuegen*, sodass eine moeglichst grosse Anzahl
an Bitfehlern *erkannt* und *korrigiert* werden kann.

Ein Masstab fuer die Fehleranfaelligkeit von bestimmten Uebertragungsmedien ist
die *Bitfehlerwahrscheinlichkeit* $p_e$. Sie gibt an, wie hoch die
Wahrscheinlichkeit ist, dass ein gegebener, uebertragener Bit falsch sein wird.

* Bei Ethernet ueber Kupferkabel meist: $p_e \approx 10^{-8}$
* Bei WLAN charakteristisch: $p_e \approx 10^{-6}$

#### Blockcodes

Blockcodes sind eine Methode zur Fehlerbehandlung bei der
Datenuebertragung. Hierbei wird der Datenstrom (Bitfolge) in Sequenzen der
Laenge $k$ aufgeteilt und in *Bloecke* der Laenge $n > k$ gepackt. Dann werden
die uebrigen $n - k$ Bits zur Fehlererkennung und Rekonstruktion verwendet. Die
Coderate $R = \frac{k}{n}$ gibt dabei an, wieviel unsere Daten vom gesamten
Blockcode ausmachen.

##### Repetition Codes

Als Beispiel koennten wir jeden Bit, also jedes Wort der Laenge $k = 1$, auf
einen Blockcode der Laenge $n = 3$ abbilden, indem wir den einen Bit noch zwei
mal duplizieren. Bei der Dekodierung wird dieses Tripel dann auf jenen binaeren
Wert abgebildet, der am haeufigsten vorkommt. So wuerde also $0 \mapsto 000$ und
$1 \mapsto 111$ abgebildet. Dann koennen wir einen einzigen Bitfehler pro
Blockcode tolerieren. Wenn beispielsweise $000$ durch Uebertragungsfehler zu
$001$ wuerde, waere die $0$ noch immer am haeufigsten und somit der dekodierte
Bit. Erst ab *zwei* Bitfehlern waere der urspruengliche Bit verfaelscht.

Diese Art von Kanalkodierung nennt man *Repetition Codes*. Man merkt aber, dass
Repetition Codes nicht sehr effizient ist. Die Datenrate wuerde naemlich
gedrittelt werden. Interessant ist aber, dass man durch Repetition Codes Fehler
nicht nur erkennen, sondern sogar korrigieren kann.

##### 4B5B

Ein beliebter Blockcode ist der 4B5B Code, wobei alle 4 Bits auf ein neues
Datenwort mit 5 Bits abgebildet werden. Somit hat man also wiederum 4 Bits frei,
um bis zu 16 Kontrollinformationen pro Block zu senden (wenn der obere Bit 1 ist
hat man die unteren 4 Bit frei fuer Informationen). Das hat so aber eher in der
Leitungskodierung Anwendung. In der Kanalkodierung, welche sich ja lediglich mit
dem Erkennen und Korrigieren von Fehlern beschaeftigt, koennte der zusaetzliche
Bit z.B. fuer einen Parity Bit genutzt werden. Waere dieser letzte Bit gesetzt,
wuerde das aussagen, dass die vier Bit einen ungerade Kardinalitaet (Anzahl an
gesetzten Bits) haetten. Waere der Bit nicht gesetzt, heisst das, dass die vier
Bit *urspruenglich* eine gerade Kardinalitaet hatten. Kippt nun ein Bit um, so
aendert sich die Paritaet. Dann wuerde man also erkennen, dass das Parity Bit
anders ist, als die Paritaet der uebertragenen Daten. Somit wuesste man, dass
ein Fehler aufgetreten ist.

4B5B Codes in der Kanalkodierung sind also eine Massnahme zum *Erkennen* von
Bitfehlern. Korrigieren koennte man Fehler nicht, weil man nicht weiss, wieviele
Bits genau umgekippt sind (da jede ungerade Anzahl an Fehlerbits die selbe
Paritaet ergibt).

#### Praxis

In der Praxis ist am anderen Ende einer Leitung oft eine Einheit, die die
Coderate der Kanalkodierung je nach geschaetzer Bitfehlrewahrscheinlichkeit
anpasst.

## Impulsformung

Bei der *(Basisband-)Impulsformung* geht es darum, aus einem Datenstrom ein
analoges *Basisbandsignal* zu generieren. Das Basisbandsignal ist dabei jenes
Signal, welches spaeter bei der Uebertragung, z.B. bei Funk durch FM oder AM,
moduliert wird, um letztendlich abgesandt zu werden. Hierbei sendet man in
regelmaessigen Abstaenden gewichtete Sendegrundimpulse. Ein solcher Impuls
stellt dabei jeweils einen Bit oder eine Gruppe von Bits dar.

### Hintergrund

Im Weiteren wird die *Leitungskodierung* dafuer zustaendig sein, zu beschreiben,
wie Bitstroeme durch solche gewichteten Grundimpulse kodiert werden. Da, wie wir
sehen werden, die Grundimpulse in der Regel rechteckig sind, besitzen sie aber
theoretisch ein unbegrenztes Frequenzspektrum. Da es in der Praxis bei
bandbegrenzten Kanalen natuerlich nicht moeglich ist, solche unbegrenzten
Signale zu senden, muessen *Leitungscodes* noch entsprechend auf eine
Traegerwelle *moduliert* werden, sodass die Codes mit begrenzter Bandbreite
uebertragen werden koennen. Damit beschaeftigt sich folglich die *Modulation*,
welche weiter unten behandelt wird.

### Grundimpulse

Es gibt hierbei verschieden Arten von Grundimpulsen, welche jeweils ueber einem
Zeitraum von $-T/2$ bis $+T/2$ definiert sind:

* Der einfachste nennt sich *Non-Return-To-Zero* (NRZ) Impuls und ist wirklich
  nur der Rechtecksimpuls. Er kehrt innerhalb des Sendeintervalls kein einziges
  Mal zu Null zurueck, daher der Name.
* Ein aehnlicher Grundimpuls kehrt nach der halben Zeit wieder zu Null
  zurueck. Er nennt sich *Return-To-Zero* (RZ) Impuls und wird mit $\rz(t)$ notiert.
* Fuer Manchester Encoding werden *Manchester*-Impulse gesandt. Diese haben die
  Form einer Square Wave, haben also zwischen $-T/2$ und $0$ den Wert $+1$ und
  im Intervall $]0, T[$ den Wert $-1$. Ein Manchester Impuls $\manc(t)$ entsteht
  aus einem RZ Impuls $\rz(t)$ wie folgt: $\manc(t) = 2\cdot \rz(t) - 1$.
* Letztlich ist der $\cos^2$ Impuls auch eine Variante, welcher also gleich der
  quadierten Kosinusfunktion in diesem Intervall ist.

### Leitungscodes

*Leitungscodes* bestimmen dabei eine Abfolge solcher Grundimpulse und gewichten sie entsprechend den Daten (z.B. $\pm 1$). Im Kontext von Leitungscodes sprechen wir bei einem Symbol dann nicht notwendigerweise von einem Datum, sondern einfach von einer *physikalisch messbaren Veranderung des Zeitsignals*. Somit bestehen bestimmte Grundimpulse (Bits) also manchmal aus mehr als nur einem Symbol. Beispielsweise der RZ Impuls, welcher zwei Veraenderungen im Signal hat, besteht also aus zwei Symbolen, obwohl er nur einen Bit kodiert. Die Bedeutung des Wortes *Symbol* ist also gewissermassen ueberladen.

Wir charakterisieren verschiedene Leitungscodes dann nach:

* Der Anzahl an Signalstufen (binaere, ternaere, $m$-aere Kodierung)
* Der Anzahl kodierter Bits pro Symbol (pro Veraenderung des Signals)
* Die Schrittgeschwindigkeit, auch *Symbolrate* oder *Baudrate* genannt. Sie wird in Bit pro Symbol, oder *Baud* angegeben.

Optional interessieren wir uns noch dafuer, ob ein Leitungscode *Taktrueckgewinnung* ermoeglicht. Taktrueckgewinnung meint, Synchronisation zwischen Sender und Empfaenger bezueglich dem Takt. Der Empfaenger muss naemlich wissen, wann und in welchen Zeitabstaenden er abtasten muss. Beispielsweise erlaubt NRZ keinerlei solche Synchronisation. RZ hingegen hat immer den Edge zurueck zu Zero, sodass man weiss, dass man in einer halben Periode als naechstes wieder abtasten muss.

Eine weitere, moegliche Eigenschaft ist *Gleichstromfreiheit*. Ist ein Leitungscode gleichstromfrei, so bedeutet das, dass er keinen Gleichanteil hat. D.h. ueber eine unendilche Signalfolge wuerde der Code keinen offensichtlichen "Offset" an Strom haben. Ist das der Leitungscode beispielsweise einfach immer $+5$ Volt, so wuerde eine unendliche Signalfolge einen Gleichstrom von $+5$ Volt erzeugen. Ist der Leitungscode allerdings um den Nullpunkt zentriert, ist das nicht so.

Letztlich ist die Bandbreite des Leitungscodes noch relevant, falls wir mehrere Signale gleichzeitig ueber denselben Kanal senden wollen (Frequency Division Multiplex).

#### Non-Return-To-Zero (NRZ)

Bei dieser Kodierung ist der Grundimpuls der einfache Rechtecksimpuls $\rect(t)$ mit Periodendauer $T$. Die Gewichte fuer den Grundimpuls sind dabei entweder $+1$ oder $-1$. $+1$ wird gewaehlt, wenn der zu sendende Bit gleich $1$ ist. Wenn der Bit $0$ ist, wird der Grundimpuls durch ein Gewicht von $-1$ eben negiert bzw. invertiert. Somit ist das Sendesignal dann definiert durch $s(t) = \sum_{n=0}^\infty d_n \rect(t - nT)$.

Dieses Signal kann man wie die Formel zur Diskretisierung eines Signals interpretieren. Zu jedem Zeitpunkt $t$ befindet sich $t$ in genau einem Impuls, sodass $\rect(t - nT)$ gleich eins ist. Das ist fuer den $n$-ten Impuls also gerade dann, wann $t \in [nT - T/2, nT + T/2]$
liegt. Dann ist $t - nT$ naemlich $\in [-0.5, +0.5]$. Der Rechtecksimpuls ist
gerade fuer dieses Intervall definiert. Fuer alle anderen $m \neq n$ liegt $t -
mT$ ausserhalb des Nicht-Null Intervalls dieses Impulses. So kann man einen
solchen Leitungscode bzw. hier einen Non-Return-To-Zero Code also als
mathematische Funktion in Abhaengigkeit der Zeit $t$ definieren, sodass die
Funktion insbesondere fuer jedes $t$ definiert ist (linkstotal).

Die Eigenschaften dieses NRZ Codes sind:

* Der Code ist binaer (nur zwei Signalstufen)
* Effizienz: 1 Bit pro Symbol (Baud-Rate = 1)
* Keine Takrueckgewinnung. D.h. wenn man eine lange Folge von Einsen oder Nullen
  hat, kann es sein, dass man einiger Zeit nicht mehr weiss, wann man abtasten
  soll. Auch: Sendet man eine lange Folge von Nullen ueber eine Funkverbindung,
  erhaelt der Empfaenger also eigentlich keine neuen Daten oder zumindest nur
  Rauschen. Dann kann die Verbindung gaenzlich verloren gehen.
* Keine Gleichstromfreiheit
* Relativ breites Frequenzpektrum (brauchen mehr Bandbreite).

#### Return-To-Zero (RZ)

Bei *Return-To-Zero* wird der entsprechende Return-To-Zero Impuls $\rz(t)$
mit Periodendauer $T$ verwendet. Dieser ist fuer das halbe Intervall $[-T/2, 0]$
gleich eins und fuer die andere Haelfte $[0, T/2[$ gleich Null. Die Gewichte sind dann wie
bei NRZ gerade $d = +1$ fuer einen gesetzten Datenbit und $d = -1$ fuer einen
geloeschten Datenbit. Das Signal ergibt sich somit durch:

$$s(t) = \sum_{n=0}^\infty d_n \rz(t - nT)$$.

```
         s(t)
          |
    |-----|
    |     |
    |     |
    |     |
    |     |------|
---------------------> t
```

Eigenschaften von RZ:

* Da das Signal drei Stufen $\in \{-1, 0, +1\}$ annehmen kann, ist der Code also ternaer* (bei NRZ hatten wir nur $\{-1, +1\}$).
* Effizienz von 1 Bit / 2 Symbolen, somit ist die Baud-Rate $1/2$. Wir haben zwei Symbole, weil wir einmal den Edge fuer das Datum haben, und einmal den Edge zurueck zu Zero.
* Taktrueckgewinnung durch Pegelwechsel!
* Keine Gleichstromfreiheit (wenn nur 1-en, dann Gleichanteil 0.5)
* Breiteres Spektrum als NRZ (abruptere Spruenge?)

#### Manchester Code

Der *Manchester*-Code besteht aus Manchester-Impulsen, welche um die halbe
Amplitude nach unten verschobene, skalierte RZ-Impulse sind. Die Gewichtung ist
wie bei RZ und NRZ, also ergibt sich das Signal durch:

$$s(t) = \sum_{n = 0}^\infty d_n \cdot \{manc(t - nT).$$

```
         s(t)
          |
    |-----|
    |     |
    |     |
    |     |
    |     |
---------------------> t
          |     |
          |     |
          |     |
          |     |
          |-----|
```

Seine Eigenschaften sind:

* Binaerer Code (zwei Signalstufen)
* Effizienz von 1 bit / 2 Symbolen (Baud Rate = $1/2$, wie RZ)
* Taktrueckgewinnung wie bei RZ moeglich
* Gleichstromfreiheit! Fuer jede Signalaenderung gibt es naemlich auch die inverse Signalaenderung. Somit ist jeder Impuls null-zentriert und es kann nie ein Offset entstehen.
* Sehr breites und langsam abklingendes Spektrum

#### Multi-Level-Transmit 3 (MLT 3)

Bei Multi-Level Transmit verwenden wir weider den Rechtecksimpuls und definieren
unser Signal durch $s(t) = \sum_{n=0}^\infty d_n \cdot \rect(t -
nT). Besonders ist jedoch, wie wir die Gewichte definieren:

$$d_n = \sin\left(\frac{\pi}{2}\sum_{k=0}^n b_k\right)$$

Fuer das $n$-te Gewichte betrachten bzw. summieren wir also alle vorherigen Bits, und gehen genau so viele Schritte jeweils um $\pi/2$ weiter. Fuer jeden neuen Bit wird die Summe um eines groesser und die Funktion geht $\pi/2$ (90 Grad) weiter. Was also passiert, ist dass wir durch die Stufen $0, \pi/2, \pi, 3\pi/2$ fuer jeden neuen gesetzten Bit quasi *durchrotieren*. Jedes Mal, wenn wir also einen neuen gesetzten Bit senden, geht unser Gewicht um $\pi/2$ Schritte weiter. D.h., dass die Gewichte also durch die Werte $\sin(0) = 0, \sin(\pi/2) = 1, \sin(\pi) = 0, \sin(3\pi/2) = -1$ gehen. Das 3 in MLT 3 kommt also davon, dass wir drei verschiedene Stufen $+1, 0, -1$ haben.

```
         s(t)
          |
    |-----|-----|
    |     |     |
    |     |     |
    |     |     |
    |     |     |
---------------------> t
          |
          |
          |
          |
          |
          |
```

Eigenschaften:

* MLT 3 benutzt einen ternaeren Code.
* Effizienz ist 1 Bit / Symbol (Baud Rate = $1$) (wobei eine Null eigentlich durch *kein Symbol*, also *keine Signalaenderung* definiert ist)
* Keine Taktrueckgewinnung, da eine lange Sequenz von gleichen Bits keine neue Signalaenderung bedeutet.
* *Schmales Spektrum*, da das Signal durch den periodischen Verlauf eine
  reduzierte Grundperiode hat.

### Kodierung zusaetzlicher Informationen

Woher wissen wir nun aber, ob auf unserem Medium wirklich Daten vorliegen, oder
ob gerade einfach nichts gesendet wird? Auch fragen wir uns, wie wir denn
ueberhaupt den Start und das Ende einer Nachricht festlegen koennen. Wir
brauchen also irgendeine Art von Kodierung, die uns gerade das erlaubt. Hierfuer
gibt es zwei Moeglichkeiten: *Coderegelverletzungen* oder *Steuerzeichen*.

#### Coderegelverletzung

Fuer diese Moeglichkeit nutzen wir die Tatsache aus, dass manche der
Leitungskodierungsverfahren strenge Regeln fuer die moeglichen Werte der
Signalpegel haben. Wir koennen also spezielle Informationen senden (quasi mit
hohem Informationsgehalt), wenn wir diese Regeln verletzen. So koennen wir also
beispielsweise signalisieren, dass eine Verbindung abgebrochen/vollendet
ist. Konkret:

* Vor Beginn einer Nachricht wird eine fest definierte Anzahl alternierender
  Bits gesendet. Diese Bits nennt man die *Praeambel* und dienen auch zur
  *Taktsynchronisation*.
* Um den Beginn der eigentlichen Nachricht zu signalisieren, wird eine bestimmte Bitsequenz festgelegt: Den *Start Frame Delimiter* (SFD). Diese Sequenz muss sich von der Praeambel auch irgendwie unterscheiden. Beispielsweise koennte man vier alternierende Bits mit *halber* Taktfrequenz senden. Diese Coderegelverletzung wuerde man dann erkennen.
* Um die Nachrichtuebertragung abzuschliessen, kehre entgegen der Regeln, zu
  Null zurueck.

Dieses Verfahren, die Regeln zu verletzen, indem man zu Null zurueckkehrt, funktioniert bei NRZ (indem man zur Null zurueckkehrt), RZ (kehre nicht zu Null zurueck) sowie Manchester Encoding (gehe auf Null). Es funktioniert jedoch nicht bei MLT3, da eine lange Folge von Nullen hierbei eine valide Nachricht signalisiert (nicht wie bei NRZ oder Manchester Encoding) und ein Signalwechsel (wie bei RZ) nicht immer notwendig ist.

Diese Methode wird bei 10 Mbit/s Ethernet verwendet.

#### Steuerzeichen

Die zweite Moeglichkeit ist es, einen speziellen Blockcode zu definieren, sodass fuer jeden uebertragenen Block ein Teil der Bits fuer Kontrollinformationen uebrig bleibt (siehe Sektion zu Blockcodes). Bei der Kanalkodierung dienten Blockcodes dazu, Fehler erkennen und sogar korrigieren zu koennen. In diesem Fall nutzen wir sie aber, um eben Steuerzeichen zu senden. Bleiben noch Codewoerter oder sogar Bits uebrig, so koennen diese dann natuerlich noch zur Fehlererkennung dienen.

Ein Beispiel ist wieder der 4B5B Code, welcher von 100 MBit/s FastEthernet verwendet wird. Die 16 moeglichen Codewoerter (wenn der erste Bit gesetzt ist) koennen fuer Start/Stop oder Idle (End of Transmission) Signale genutzt werden. Auch werden manchmal 8B10B Codes verwendet, wo wogar 768 Kontrollcodewoerter moeglich sind.

## Modulation

Die bisher besprochenen Basisbandsignale sind alle sehr abrupt und ecking, was
auf sehr viele hohe Harmonische hindeutet. Genauer hat ein Rechtecksimpuls
beispielsweise ein unendlich ausgedehntes Frequenzspektrum. Wenn wir nur dieses
eine Signal ueber unseren Kanal senden wollen, ist das auch kein Problem.

Wenn wir aber mehrere Signale gleichzeitig ueber den selben Kanal senden wollen, dann muessen wir dafuer sorgen, dass ihre Frequenzbereiche (Bandbreiten) nicht ueberlappen. Dann koennen wir naemlich zwei Signale nebenlaeufig auf demselben Medium uebertragen und am anderen Ende wieder "auseinanderziehen", indem wir eben die einzelnen Frequenzkomponenten trennen. Die Amplituden der beiden Signale wuerden dann zwar fuer das gesammelte Signal, dass aus dem Kanal ausstroemt, bzw. waehrend der Uebertragung, interferieren (additiv oder subtraktiv). Da wir die Frequenzkomponenten aber alleine betrachten koennen, erhalten wir spaeter wieder die urspruenglichen Amplituden. Dieses Konzept, mehrere Signale gleichzeitig mit verschiedenen Frequenzen ueber den selben Kanal zu senden, nennt man *Frequency Division Multiplex* (FDM).

Wir betrachten bei der Vorlesung nur Amplitudenmodulation. Der Ablauf der
Modulation ist dabei wie folgt:

1. Wir haben unser Signal, bestehend aus Grundimpulsen $g(t)$. Diese haben wie
  gesagt unendliche Bandbreite, was wir nicht wollen. Wir filtern diesen
  Grundimpuls also zunaechst mit einem Tiefpassfilter auf eine maximale Frequenz
  $f_{max}$. Die gefilterterten Impulse bezeichnen wir dabei mit $g_T(t)$, das
  resultierende Datensignal (das aus den Grundimpulsen besteht) mit $s_T(t)$. Anmerkung: Die gefilterten Impulse
  stellen eine Verzerrung des Basisbandsignals in der Zeit-Ebene dar, was aber
  nicht weiter schlimm ist (die Ecken der Rechteckskurven gehen weg), da es nur zur Modulation dient.
2. Wir nehmen dann eine *Traegerwelle* $c(t) = \sin(2\pi f_0 t)$ (carrier wave) mit
   einer bestimmten *Traegerfrequenz* $f_0$ (nur die Grundharmonische), und
   modulieren die Amplitude unseres Signals $s_T(t)$ auf die Tragerwelle $c(t)$
   auf. Wie diese Modulation aber genau passiert, wird weiter  unten besprochen.
3. Mathematisch ist unser moduliertes Signal $s(t)$ dann definiert durch: $s(t)
   = s_T(t) \cdot c(t)$. Man nennt dieses Signal auch *Bandpassignal* (anstatt
   *Basisbandsignal* fuer das unmodulierte).

Im Frequenzbereich passiert folgendes:

1. Das Signal wird durch die Filterung auf eine schmale Bandbreite $f_{max}$ begrenzt.
2. Die Modulierung verschiebt dieses schmale Basisband um genau $f_0$ Hertz (Multiplikation im Zeitbereich entspricht Faltung bzw. Verschiebung im Frequenzbereich).

Das Traegersignal besteht hierbei nur aus einer einzigen Grundharmonischen mit Frequenz $f_0$. Somit hat die Traegerwelle an sich eine winzig kleine Bandbreite (im Prinzip keine). Durch die Multiplikation dieses Traegersignals (diesem einen Sinus-Term) mit dem bandbegrenzten Datensignal $s_T(t)$ entsteht dann das modulierte Signal $s(t)$. Die Bandbreite dieses modulierten Signals ist dann gerade $f_{max}$, zentriert um $f_0$. D.h. die Tiefpassfilterung des Basisbandsignals bestimmt die letztendliche Bandbreite des Signals, das wir uebertragen. Das Traegersignal bestimmt nur, wo wir das modulierte Signal im Frequenzspektrum platzieren.

#### 4-ASK

Eine Variante der Amplitudenmodulation nennt sich *4-ASK* (*Amplitude Shift
Keying*). Hierbei kann das modulierte Signal (Bandpassignal) gerade *vier*
diskrete Stufen annehmen. Wir koennen also mit einem Symbol (einer
Signalaenderung) zwei Bits (aus einem Leitungscode) senden.

Ein Beispiel: Wir waehlen unsere vier Signalstufen mit $\mathbb{S} = \{-3/2, -1/2, +1/2, +3/2\}$. Dann bilden wir je zwei Datenbits aus unserem Datenstrom auf ein Symbol (eine Signalstufe) ab: $00 \mapsto -3/2, 01 \mapsto -1/2, 10 \mapsto +1/2, 11 \mapsto +3/2$. Wir wuerden also je zwei Bit aus einem einkommenden Leitungscode interpretieren und dann in einen Grundimpuls fuer unser neues Basisbandsignal (fuer die Modulation) mit entsprechender Gewichtung aus $\mathbb{S}$ transformieren. Das so entstehende Signal kann dann einfach mit dem Traegersignal multipliziert werden, um das modulierte Signal zu erzeugen, welches letztendlich uebertragen werden kann. Jedes Mal, wenn sich ein Gewicht eines Grundimpulses fuer das Basisbandisignal veraendert, wuerde sich auch die Amplitude des Traegersignals entsprechend der Abbildung veraendern.

Es gibt auch andere Varianten von ASK, wobei $n$-ASK also meint, dass es $n$ verschiedene Signalstufen gibt. $n$ waere hier wohl immer eine Zweierpotenz.

#### QAM

Quadratur-Amplituden-Modulation (QAM) ist eine Modulationstechnik, die
ausnutzt, dass Sinus- und Kosinusschwingungen von einander unabhhaengig sind, da
sie nicht in Phase sind. So koennen wir beispielsweise ein Signal $a \cdot
\sin(\omega t)$ nehmen und ein neues Signal $b \cdot \cos(\omega t)$ einfach
draufaddieren. Spaeter koennen wir den Sinus- und Kosinusanteil des Signals
wieder trennen und erhalten so zwei unabhaengige Informationsquellen. Somit ist
es uns moeglich, die Datenuebertragungsrate zu verdoppeln! Wir koennen naemlich
mit den Sinusanteilen $M$ Bits kodieren und mit den Kosinusanteilen noch einmal
$M$ Bits. Dieses Konzept ist aehnlich FDM, nur dass wir hier auf quasi auf
verschiedenen Phasen uebertragen und nicht auf verschiedenen Frequenzen. Der
__Kosinusanteil des Signals wird dabei als *Inphase-Anteil* bezeichnet, der
Sinusanteil als *Quadratur-Anteil*:

* __Kosinus: Inphase__
* __Sinus: Quadratur__

Ein konkretes Beispiel von QAM waere 16-QAM, wo zwei Bit durch Sinus und zwei
durch Kosinus uebertragen werden. Insgesamt hat man also vier Bit und so 16
verschiedene Signalstufen pro uebertragenem Symbol.

Wenn wir die Sinus- und Kosinusanteile nun also separat betrachten, dann koennten wir meinen, wir koennen nun mehr als $2B$, naemlich $2 \cdot 2B$ viele unterscheidbare, unabhaengige Werte uebertragen. Wir haetten also Nyquist mit seiner Nyquist Rate $f_N$ widerlegt. Das ist aber nicht der Fall. Es stellt sich naemlich heraus, dass sich durch QAM die Bandbreite verdoppelt. Die Datenrate, also die Kanalkapazitaet, ist im Vergleich zu 4-ASK verdoppelt, da die Datenrate ja neben der Bandbreite noch von den moeglichen Symbolen (Signalstufen) $M$ abhaengt ($C_H = 2B\log(M)$), welche bei QAM eben mehr sind. Aber gleichzeitig verbrauchen wir aber auch doppelt so viel Platz im Frequenzspektrum, sodass wir eben nicht mehr Signale bzw. mehr Daten uebertragen koennen, da wir noch immer durch die Bandbreite $B$ des Kanals beschraenkt sind. D.h. ja, die Datenrate verdoppelt sich, aber nein, wir koennen nicht mehr Daten uebertragen.

#### PSK

Eine weitere Modulationsvariante, *Phase Shift Keying* (PSK), veraendert nicht
die Amplitude des Signal, sondern die *Phase*. Das Prinzip ist dem ASK sehr
aehnlich. Bei 4-PSK gaebe es drei verschiedene Signalstufen:

1. 0 Grad Verschiebung
2. 90 Grad Verschiebung
3. 180 Grad Verschiebung
4. 270 Grad Verschiebung

PSK ist wie FM Modulation robust gegenueber Stoerungen in der Amplitude des
Signals, da diese die Phase (wie die Frequenz) nicht veraendern.

## Uebertragungsmedien

Wir haben nun besprochen, wie Daten kodiert und moduliert werden. Jetzt fehlt
es nur noch zu erfahren, wie diese Daten letztendlich eigentlich physisch
uebertragen werden. Hierbei unterscheiden wir zwischen:

1. Leitungsgebundener und
2. Nicht-leitungsgebundener (wireless)

Uebertragung. Sowie dann noch zwischen:

1. akustischen (Schallwellen) und
2. elektromagnetischen

Wellen. Elektromagnetische Wellen werden wir als erstes unter die Lupe nehmen.

### Elektromagnetische Wellen

Elektromagnetische (EM) Wellen bestehen aus einer *elektrischen* Komponente
$\vec{E}$ und einer *magnetischen* Komponente $\vec{B}$. Diese sind jeweils
orthogonal zueinander sowie zur Ubertragungsrichtung. EM Wellen haben dabei
folgende Eigenschaften:

* __Im Vakuum__ breiten sie sich mit Lichtgeschwindigkeit $c \approx 3 \cdot 10^8
  \text{ m/s}$ aus.
* Im Gegensatz zu Schallwellen brauchen sie kein Medium (propagieren auch im
  Vakuum).
* Innerhalb eines Mediums (z.B. Leiter, Luft, Waende) ist die
  Ausbreitungsgeschwindigkeit geringer als im Vakuum. Wir betrachten also meist
  die Geschwindigkeit $\nu c$, wo $0 < \nu < 1$ als *relative
  Ausbreitungsgeschwindigkeit* bezeichnet wird.
* Die Wellenlaenge $\lambda$ beschreibt die raeumliche Ausdehnung einer
  Wellenperiode.
* Die Frequenz $f$ ergibt sich aus der Wellenlaenge und der Lichtgeschwindigkeit
  (nach der Geschwindigkeit-Weg-Zeit Formel): $f = c/\lambda$.

Das elektromagnetische Spektrum beschreibt dabei den Wertebereich, den elektromagnetische Wellen ueblicherweise annehmen. Wir betrachten darin Frequenzen zwischen ungefaehr zwischen $10^2$ (Wechselstrom) und $10^{23}$ (Hoehenstrahlung). Fuer Datenuebertragung interessiert uns jedoch hauptsaechlich der Bereich zwischen MHz ($10^6$) und einigen Gigahertz ($10^9$).

### Koaxialkabel

Koaxialkabel sind eine bestimmte Art von Kabel, welche unter Anderem fuer
Ethernet 802.3a mit 10 MBit/s verwendet werden. Hierbei wird meist ein langer
Bus betrachtet, an welchem mehrere Teilnehmer angeschlossen sind. Ausser diesem
Gebiet sind die Anwendungsbereiche von Koaxialkabel jedoch eher gering. Sie
werden noch fuer Kabel-TV verwendet. Auch Cinch-Kabel im Audio Bereich benutzen
diese Art von Kabel.

Solche Kabel bestehen aus vier Schichten: einem Inneleiter (auch *Seele*
genannt), einem Isolator, einem Aussenleiter und letztlich einem auesseren
Mantel.

### Twisted-Pair Kabel

*Twisted-Pair* Kabel werden fuer viele heutige Netwerkanwendungen benoetigt,
 beispielsweise den meisten Ethernet Standards. Hierbei besteht ein Kabel aus
 mehreren (2 - 4) *Paaren*, bestehend aus jeweils zwei *Adern*, wobei durch ein
 Paar immer dasselbe Signal gesendet wird.

 In der einen Ader eines Paarse wird der Signalpegel jedoch immer
 invertiert. Beim Empfaenger wird das Signal dann wieder rueckinvertiert. Das
 eigentliche empfangene Signal wird dann dadurch gebildet, dass die Differenz
 (Mittelwert) der beiden Signale gebildet wird. Dies nennt man *differential
 mode*. Wenn die Signale unveraendert ankommen, dann wird der Mittelwert (nach dem Invertieren des invertierten Signals) der
 Signale wieder das urspruengliche Signal sein. Wenn nun aber Rauschen
 hinzukommt, dann wird das Invertieren sehr nuetzlich: Das Rauschen wuerde also
 sowohl beim Signal in der einen Ader als auch beim invertierten Signal additiv
 wirken. Das bedeutet, dass es beim einen Signal die Amplitude erhoeht und beim
 anderen die Amplitude um den selben Wert reduziert. Nach dem
 Mittelwert-Verfahren werden sich die Amplituden der beiden Signale dann wieder
 zurueck zum urspruenglichen Signal normalisieren. Somit hat man also das
 Rauschen entfernt.

 Neben der Invertierung ist es noch eine interessante Eigenheit, dass Paare
 immer verdrillt (*twisted*) sind. Das reduziert das gegenseitige Uebersprechen
 (*Crosstalk*) der Signale in den nebeneinanderliegenden Paaren. Genauer ist der
 Sinn davon, dass sich die Signale in den Aderpaaren weniger beeinflussen, wenn
 ihre Amplituden invers sind (Hochpunkt vom einen = Tiefpunkt vom anderen). Wenn
 sich die Adern also beeinflussen, dann soll sich das ueber Zeit ausgleichen,
 was durch Verdrillung ermoeglicht wird.

 Am Ende eines solchen Twisted-Pair Kabels werden meist RJ-45 oder RJ-11
 Steckverbinder angeschlossen, wie sie von den meisten Ethernet Kabeln bekannt
 sind.

Man unterscheidet Twisted-Pair Kabel noch nach ihrer Schirmung:

* Unshielded Twisted Pair (UTP)
* Shielded Twisted Pair (STP)
* Screened Unshielded Pair (S/UTP): Hierbei werden Kabel noch durch eine
  spezielle Aluminium-Folie umgeben.
* Screened Shielded Pair (S/STP)

Die Schirmung hat dabei Einfluss auf sowohl die Signalqualitaet (z.B. bezueglich
Crosstalk) wie auch die Flexibilitaet der Kabel (gut geschirmte Kabel sind
steifer).

Es gibt noch einen Unterschied darin, wie die Kabel innerhalb der Schirmung
konfiguriert werden. Es gibt dabei die *Straight-Through* (halbduplex) Kabel,
welche nur verschiedene Geraetetypen (z.B. PC und Hub oder Switch) verbinden
duerfen, sowie die *Cross-Over* (vollduplex) Kabel, welche auch gleiche Geraete (PC und
Router, Hub und Switch) verbinden koennen. Das hat damit zu tun, auf welchen
Pins (Adern) diese Geraete senden und auf welchen sie empfangen.

### Optische Leiter

In *optischen Leitern* werden nicht elektromagnetische Wellen, sondern Licht
uebertragen. Ein solcher Leiter besteht dabei aus einem *Kern*, in welchem das
Licht propagiert; einem Mantel sowie einer Huelle. Kern und Mantel haben dabei
einen verschiedenen Brechungsindex, sodass es innerhalb des Kerns annaehernd zu
Totalreflexion kommt. Wir unterscheiden dabei noch zwischen zwei Arten von
optischen Fasern (Leitern):

1. Single-Mode Fasern vermeiden Streuung des Lichts durch sehr geringen
   Kernduchmesser. Dadurch hat man weniger Signalverluste, jedoch ist das Kabel
   auch sehr empfindlich bezueglich Kabelbruch.
2. Multi-Mode Fasern haben einen groessern Kerndurchmesser und neigen daher eher
   zum Streuen. Sie haben also hoehere Verluste (nach aussen), sind aber weniger
   fragil.

Vorteile von optischen Leitern gegenueber elektromagnetischen sind:

1. Sehr hohe Datenraten moeglich.
2. Weite Strekcen ueberbrueckbar.
3. Kein Uebersprechen.
4. Galvanische Entkopplung von Sender und Empfaenger.
5. Energiesparender als andere Leitungen.
