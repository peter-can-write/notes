# Endliche Automaten

Endliche Automaten (Finite-State-Machine, *FSM*) ist ein theoretisches Konstrukt
dass im Schaltungsentwurf ein passendes Modell fuer eine Schaltung sein kann.

Ein Endlicher Automat $M = (E, A, Q, \delta, \lambda)$ ist definiert durch

$E = \text{ Eingabemenge}$
$A = \text{ Ausgabemenge}$
$Q = \text{ Zustandsmenge}$
$\delta = \text{ Zustandsuebergangsfunktion} \delta: E \times Q \rightarrow E$
$\lambda = \text{ Ausgabefunktion} \lambda$

$\delta$ nimmt also die neueste Eingabe $E$ in den Automaten, den momentanen
Zustand $q$ und bildet diese auf den neuen Zustand $q'$ ab.

Somit ist $q_{t_i} = \delta(e_{t_{i - 1}}, q_{t_{i - 1}})$. Der Zustand
$q_{t_i}$ zum Zeitpunkt $t_i$ haengt davon, was $\delta$ aus dem vorherigen
Eingang $e_{t_{i-1}}$ und dem vorherigen Zustand $q_{t_{i - 1}}$ produziert.

Es gibt dann zwei Typen von Automaten, die sich in der Definition ihrer
Ausgabefunktion $\lambda$ unterscheiden.

## Moore Automaten

Bei *Moore* Automaten haengt die Ausgabe $a \in A$ __nur vom momentaten
Zustand__ ab -- die Eingabe ist irrelvant:

$\lambda: Q \rightarrow A$

## Mealy Automaten

Bei *Mealy* Automaten haengt die Ausgabe $a \in A$ sowohl vom momentanen Zustand
$q \in Q$, als auch vom momentanen Eingang $e \in E$ ab, also:

$\lambda: E \times Q \rightarrow A$

$\delta$ und $\lambda$ haben somit die selbe "Signatur".

Bei Mealy Automaten besteht dass Problem, dass wenn man mehrere Mealy-Automaten
verkettet, endlose Rueckkopplungsschleifen entstehen koennen. Ist z.B. der
Ausgang von Automat A an den Eingang von Automat B geschlossen, und der Ausgang
von Automat B an den Eingang von Automat A. Kommt jetzt einmal eine Eingabe $E$,
dann wird dies eine Endlosschleife starten, die kontinuierlich den Zustand sowie
die Ausgabe (und daher wieder den Zustand) der einzelnen Automaten veraendert.

## Satz

Zu jedem Moore Automaten gibt es einen aequivalenten Mealy Automaten, und
umgekehrt. Der Moore Automat muss eben richtig verdrahtet sein. Also:

$\forall e \in E \exists q \in Q: \exists \lambda_{\text{Mealy}},
\lambda_{\text{Moore}}: \lambda_{\text{Mealy}}(q, e) = \lambda_{\text{Moore}}$

## Darstellungsmoeglichkeiten

Man kann endliche Automaten auf zwei Weisen darstellen:

### Automatengraphen (Bubble-Diagram)

Hierbei werden die einzelnen Zustaende $q \in Q$ jeweils durch Knoten ("Blasen")
in einem Graphen dargestellt. Dann gibt es noch gerichtete Kanten zwischen den
Knoten, die die Uebergaenge zwischen Zustaenden visualisieren. Ueber jeder Kante
steht also der Eingang $e$, der zusammen mit dem Ausgangsknoten (Zustand) zum
neuen Zustand fuehrt.

Eine Unterscheidung zwischen Mealy und Moore Automaten ist dann, wie man die
Ausgabe visualisiert. Bei Moore Autmaten ist die Ausgabe nur vom Zustand
abhaengig, also wird die Ausgabe gleich neben den Knoten geschrieben. Beim Mealy
Automaten ist auch noch die Eingabe wichtig, also wird die Ausgabe neben die
Eingabe ueber den Kanten geschrieben. `q--(e, a)-->q'` bedeutet beim Mealy
Automaten dann, dass gegeben den Eingang $e$, $\delta(q, e) = q'$ und $\lambda(q,
e) = a$.

Sei z.B. $M = (E, A, Z, \delta, \lambda)$ mit $E = \{e_1, e_2\}$, $A = \{a_1,
a_2\}$, $Q = \{q_1, q_2\}$, $\delta$ so definiert, dass sie nur von $q_1$ nach
$q_2$ und umgekehrt springt, wenn $q$ und $e$ das selbe Subskript haben.

#### Moore Automaten

```
               a_1             a_2
	-->e_2<-- [q_1]--->e_1-->-[q_2] -->e_1<--
	              \<--<e_2---/

-->e<-- = selbstschleife
```

Wie man sieht sind die Ausgaenge beim Moore Automaten direkt an die Zustaende
gebunden. Man sieht auch, dass unter bestimmten Zustaenden (bei Mealy Automaten
auch bestimmten Eingaengen) der Zustand sich nicht aendert, gesehen an den
Selbstschleifen.

#### Mealy Automaten

Bei Mealy Automaten sind die Ausgaenge an die Kanten, also an den
Ausgangszustand *und die Eingabe* gebunden:

```
  -->(e_2, a_2)<-- [q_1]--->(e_1, a_1)-->-[q_2] -->(e_1, a_1)<--
	                  \<---<(e_2, a_2)----/

-->(e, a)<-- = selbstschleife
```

Man sieht hier auch, dass die Ausgaenge $A$ an in Abhaengigkeit vom momentanen
Zustand *und* von der Eingabe produziert werden.


### Automatentafeln

Automatentafeln visualisieren die Uebergaenge und Ausgaenge durch Tabellen an
Stelle von Graphen.

#### Mealy Automaten

Fuer Mealy Automaten sind in der linken Spalte die Zustandsmenge $Q$ (eine
Zeile pro Zustand) und in der obersten Zeile die Eingabemenge $E$ gelistet. In
den Schnittpunkten aus $E$ un $Q$ sieht man dann jeweils das Tupel $(q', a)$,
welches anzeigt, zu welchem neuen Zustand $q'$ der Automat gegeben die momentane
Eingabe $e$ und den momentanen Zustand $q$ uebergeht, und welche Ausgabe der
Automat liefert.

| Q\E |    e_1   |    e_2   |
| :-- | :------- | :------- |
| q_1 | q_2, a_1 | q_1, a_2 |
| q_2 | q_2, a_1 | q_1, a_2 |


#### Moore Automaten

Bei Moore Automaten sind die Ausgaenge an die Zustaende gebunden, also gibt man
den Ausgaengen einfach ein Spalte, wo der Ausgang in Zeile $i$ an den Zustand in
dieser Zeile gebunden ist. Die Schnittpunkte aus $Q$ und $E$ geben somit also
nur mehr den resultierenden Zustand an.

|  Q  |  A  | e_1 | e_2 |
| :-- | :-- | :-- | :-- |
| q_1 | a_1 | q_2 | q_1 |
| q_2 | a_2 | q_2 | q_1 |

## Reale Schaltungen

In realen Schaltungen muss also nur die Uebergangsfunktion $\delta$ und die
Ausgabefunktion $\lambda$ implementiert werden. Jedoch kommt noch eine weitere
Komponente hinzu: Ein Verzoegerungselement $\tau$. Dieses dient dazu, die Zeit
zu simulieren, die der Automat braucht, um vom einen Zustand in den naechsten zu
wechseln -- die *Ruckkopplungszeit*. Alle weiteren Wege und Bausteine im Automat
brauchen im theoretischen Modell also keine Zeit (sind *zeitlos*).
