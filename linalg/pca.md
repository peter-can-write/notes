# Hauptkomponentenanalyse (Principal Components Analysis)

$\newcommand{\Var}{\mathop{\rm Var}\nolimits}$
$\newcommand{\cov}{\mathop{\rm cov}\nolimits}$
$\newcommand{\GL}{\mathop{\rm GL}\nolimits}$

In diesem Kapitel wollen wir uns einer Anwendung der linearen Algebra widmen:
der Hauptkomponentenanalyse (engl. *Principal Components Analysis*; PCA).

## Hintergrund

Die Hauptkomponentenanalyse ist eine Methode aus der Datenanalyse, dessen Ziel
es grundsätzlich ist, eine geeignete Basis zur Darstellung einer gegebenen
Datenmenge zu finden.

Des Weiteren ist PCA aber insbesondere auch eine Technik zur Dimensionreduktion.
Es kann nämlich vorkommen, dass unsere Daten sehr hochdimensional sind, also
durch viele Attribute (Features) dargestellt sind. Meist finden wir dann aber
auch heraus (bzw. PCA findet heraus), dass nur wenige dieser Dimensionen
wirklich betrachtenswert sind. Mit "betrachtenswert" sind solche Attribute
gemeint, die uns viele Informationen über die Eigenheiten eines Datenpunktes
liefern. Indirekt meinen wir hiermit, dass die Varianz dieses Attributs hoch
ist. Dann kann sich ein Datenpunkt nämlich dadurch auszeichnen, dass er sehr
verschieden von den anderen Datenpunkten bezüglich dieser Dimension ist. Hat ein
Attribut nur wenig Varianz, so sind also der Großteil unserer Daten in dieser
Dimension gleich. Insofern ist der Informationsgehalt des Wertes eines
Datenpunktes in dieser Dimension gering, da viele weitere Punkte auch (sehr)
ähnliche Werte in dieser Dimension haben.

Beispielsweise könnten wir bei einer Umfrage 50 Attribute über Menschen sammeln.
Attribute könnten das Alter, das Gewicht, Haarfarbe und weitere relle oder
diskrete Eigenschaften sein. Jedes dieser Attribute ist dann eine Achse in einem
Koordinatensystem. Durch PCA könnten wir dann herausfinden, dass eigentlich nur
3 der 50 Attribute eine hohe Varianz haben. Wenn nämlich alle Menschen blond
sind (geringe Varianz), so sagt uns die Eigenschaft der "Blondheit" nichts
besonderes über einen Datenpunkt (einen Menschen) aus. Wir würden dieses
Attribut, also diese Dimension bzw. diese Achse im Koordinatensystem, entfernen
wollen. Um die Daten dann am besten zu repräsentieren, würden wir diese drei
besten Dimensionen (mit höchster Varianz) als Basis unserer Datenmenge wählen
und die Datenwolke direkt in den Ursprung (Mittelwert) dieser Basis schieben.
Letztlich wünschen wir uns noch, dass die Basisvektoren unserer Datenmenge
orthogonal (und im Weiteren auch orthonormal) sind. Intuitiv sind dann alle
Achsen vollkommen verschieden und unabhängig von einander, sodass wir keine
Redundanz in unseren Daten haben.

Zusammenfassend ist die Hauptkomponentenanalyse also eine Technik dafür, sowohl
einen Datensatz in seiner Dimension (und insofern im benötigten Speicherplatz)
zu reduzieren, als auch eine *Orthonormalbasis* zu finden, die den Datensatz am
besten repräsentiert.

## Algorithmus

Als Eingabe betrachten wir für die Hauptkomponentenanalyse eine Matrix
$\mathbf{X} \in \mathbb{R}^{m \times n}$, welche in den *Spalten* $n$
Datenpunkte (Vektoren) hält. Jeder dieser $n$ Datenpunkte hat wiederum $m$
Attribute oder Features, sodass also jede Spalte dieser Matrix $\mathbf{X}$
$m$-dimensional ist.

Diese Spaltenvektoren sind zur Standardbasis $\mathbf{B}$ dargestellt, weil das
natürlich die häufigste Basis ist, zu welcher Attribute gemessen werden. Messen
wir beispielsweise Alter und Größe von Menschen, so meinen wir mit einem Alter
von $20$ eben $20 \cdot 1 \text{ Jahr}$ und mit einer Größe von $180$ dann eben
$180 \cdot 1 \text{ cm}$. Insofern wäre jeder Datenpunkt in unserer Matrix
$\mathbf{X}$ ein zwei-dimensionaler Vektor $\mathbf{v} = (\text{Alter,
Größe})^\top$, dargestellt zur Standardbasis $\begin{bmatrix}1 & 0 \\ 0 &
1\end{bmatrix}$.

Als Ausgabe wünschen wir uns von diesem Algorithmus eine Basis $\mathbf{C} \in
\mathbb{R}^{k \times n}$, wobei $k \leq m$ und meist $k < m$ (dadurch die
Dimensionsreduktion). Diese Basis $\mathbf{C}$ soll die Daten besser
repräsentieren, also insbesondere besser an die Eigenschaften der konkreten
Daten angepasst sein. Die Repräsentation der Daten durch $\mathbf{C}$ wird also
wohl höchstwahrscheinlich die Varianz der Daten ausnutzen. Durch ein geeignete
Transformation werden wir unseren Datensatz $\mathbf{X}$ dann in eine neue
Repräsentation $\mathbf{Y}$ zu dieser besseren Basis $\mathbf{C}$ überführen.

Eine Einschränkung, die wir an die gesuchte Basis $\mathbf{C}$ stellen, ist dass
sie lediglich durch lineare Transformationen aus der ursprünglichen Basis
$\mathbf{B}$ hervorgehen kann. Letztendlich soll es also eine lineare Abbildung

$$f: \mathbb{R}^n \rightarrow \mathbb{R}^k: \mathbf{x} \mapsto \mathbf{S_{C,B}} \cdot
\mathbf{x}$$

geben, die einen Datenvektor $\mathbf{x}$ über eine lineare Transformation
(sprich: Matrixmultiplikation) mit $\mathbf{S_{C,B}}$ aus der Basis $\mathbf{B}$
in die Basis $\mathbf{C}$ überführen soll.

Im Weiteren soll diese Abbildung $f$ dann auch bijektiv sein, sodass wir eine
inverse Funktion $g(f(\mathbf{x}))$ definieren können, die einen "Code"-Vektor
$\mathbf{c} = f(\mathbf{x})$ wieder *so gut wie möglich* zurück auf den
Eingabevektor $\mathbf{x} \approx g(\mathbf{c})$ abbildet. Hierbei ist
anzumerken, dass das Bild von $g(\mathbf{c})$ nicht notwendigerweise gleich dem
Vektor $\mathbf{x}$ sein muss, da wir durch die Hauptkomponentenanalyse ja
insbesondere Dimensionen verlieren werden (bzw. wollen). Sind diese Attribute
eines Datenpunktes $\mathbf{x}$ verloren, so kann man sie unmöglich
wiederherstellen. Definieren wir also eine *Rekonstruktionsfunktion*
$r(\mathbf{x}) = (g \circ f)(\mathbf{x})$, so ist eine Eigenschaft der
Hauptkomponentenanalyse:

$$r(\mathbf{x}) \approx \mathbf{x}$$

### Grundidee

In den folgenden Absätzen wollen wir nun die Mechanik und das Werkzeug der
Hauptkomponentenanalyse weiter untersuchen. Zunächst sei aber ganz knapp die
Grundidee der Hauptkomponentenanalyse zusammengefasst.

Für die Hauptkomponentenanalyse bilden wir zunächst die Kovarianzmatrix
$\mathbf{C_X}$ unserer Datenmatrix $\mathbf{X}$ um eine kompakte Repräsentation
der Beziehungen verschiedener Attribute unserer Daten zu erhalten. Dann finden
wir eine Diagonalisierung $\mathbf{C_Y}$ dieser Kovarianzmatrix $\mathbf{C_X}$.
Als Diagonalmatrix wird diese diagonalisierte Kovarianzmatrix $\mathbf{C_Y}$ nur
Nullen jenseits der Diagonalen haben, sodass also zur Basis dieser
diagonalisierten Kovarianzmatrix die einzelnen Attribute unserer Daten
unabhängig sind. Die Eigenvektoren von $\mathbf{C_X}$ geben uns dann nach dem
Prinzip der Diagonalisierung die Basisvektoren der Diagonalmatrix
$\mathbf{D_X}$. Das sind die besten Basisvektoren, die wir für unsere Daten
finden konnten. Somit haben wir also die gesuchte optimale Basis
$\mathbf{C}$. Die Diagonaleinträge von $\mathbf{C_X}$, in sortierter
Reihenfolge, geben uns dann noch die Varianzen der Attribute zu dieser neuen
Basis $\mathbf{C}$. Wenn wir sehen, das nur ein paar wenige dieser Attribute
hohe Varianz aufweisen, dann nehmen wir uns einfach diese paar wenigen raus und
reduzieren den Raum bzw. den Datensatz auf weniger Dimensionen.

### Kovarianzmatrix

Als Erstes bilden wir für die Hauptkomponentenanalyse also die Kovarianzmatrix
$\mathbf{C_X}$ unserer Daten $\mathbf{X}$. Hierzu zunächst ein kurzer Exkurs
über die Definition von Varianz und Kovarianz allgemein.

#### Varianz

Sei $X$ eine diskrete Zufallsvariable mit Ergebnisraum (Menge möglicher Werte
von $X$) $\Omega$, dann beschreibt die Varianz $\Var(X)$ wie sehr die konkreten,
möglichen Werte der Zufallsvariable vom Mittelwert $\mu = \mathbf{E}[X]$ im
Schnitt abweichen. Für ein Zufallsexperiment $\omega$ mit $n$ Ergebnissen, also
$n$ Ausprägungen $x_i$ von $X$, ergibt sich die Varianz $\Var(X)$ von $X$ dann
durch:

$$\Var(X) = \mathbb{E}[(X - \mu)^2] = \sum_{i=1}^n p_i \cdot (x_i - \mu)^2.$$

Haben alle Werte von $X$ dieselbe Wahrscheinlichkeit, so kann die Varianz als
Mittelwert von $(X - \mu)^2$ geschrieben werden:

$$\Var(X) = \frac{1}{n}\sum_{i=1}^n (x_i - \mu)^2.$$

Die Größe (engl. *Magnitude*) von $\Var(X)$ sagt uns dann, *wie sehr* die Werte
von $X$ vom Mittelwert $\mu$ abweichen. Ist die Varianz sehr hoch, so sind die
Daten breit gestreut. Ist sie gering, sind die Daten kompakt und insbesondere
sehr aehnlich verteilt. Ist sie Null, so sind alle Daten gleich.

Die Einheit der Varianz ist schwer zu interpretieren, da die Varianz die
*Quadrate* der Zufallswerte summiert. Beschreibt die Zufallsvariable also eine
Länge in Metern, so ist die Varianz in Quadratmetern. Die *Standardabweichung*
$\sigma(X)$ nimmt zusätzlich noch die Wurzel aus der Varianz, wodurch die
Einheit der Daten erhalten bleibt:

$$\Var(X) = \sigma^2(X)$$

Ist die Standardabweichung also beispielsweise $3$, so sagt uns das, dass die
Daten im Schnitt drei Meter vom Mittelwert der Länge abweichen.

##### Für Zufallsvektoren

Ist $\mathbf{X}$ ein *Zufallsvektor*, so betrachten wir jede Komponente von
$\mathbf{X}$ separat als Zufallsvariable. Die Varianz eines Zufallsvektors
ergibt sich also einfach kompnentenweise:

$$\Var(\mathbf{X})_j = \frac{1}{n} \sum_{i=1}^n (x_i - \mu_j)^2.$$

#### Kovarianz

Die Kovarianz ist nun ein weiteres statistisches Maß, welches die Varianz
zwischen zwei *verschiedenen* Zufallsvariablen $X, Y$ beschreibt. Untersuchen
wir also ein Zufallsexperiment, wo für jeden von $n$ Datenpunkten sowohl ein
Wert von $X$ als auch ein Wert von $Y$ generiert wird, so ergibt sich die
Kovarianz $\cov(X, Y)$ durch folgende Formel:

$$\cov(X, Y) = \mathbb{E}[(X - \mu)(Y - \gamma)].$$

Hierbei sind $\mu = \mathbb{E}[X]$ sowie $\gamma = \mathbb{E}[Y]$ die
Mittelwerte von $X$ bzw. $Y$ fur das konkrete Zufallsexperiment. Da der
Erwartungswert $\mathbb{E}[X]$ eine Art Linearkombination der Werte von $X$ mit
den Wahrscheinlichkeiten von $X$ ist, ist der Erwartungswert insbesondere eine
lineare Abbildung. Somit können wir den obigen Ausdruck noch vereinfachen:

$$
\begin{align}
	\cov(X, Y) &= \mathbb{E}[(X - \mathbb{E}[X])(Y - \mathbb{E}[Y])]\\
		       &= \mathbb{E}[XY - X\mathbb{E}[Y] - \mathbb{E}[X]Y + \mathbb{E}[X]\mathbb{E}[Y]]\\
			   &= \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y] - \mathbb{E}[X]\mathbb{E}[Y] +
			   \mathbb{E}[X]\mathbb{E}[Y]\\
			   &= \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y]
\end{align}
$$

Hierbei wurde noch ausgenutzt, dass $\mathbb{E}[\mathbb{E}[X]] = \mathbb{E}[X]$,
da $\mathbb{E}[X]$ schon Skalar ist und der Erwartungswert in einem einzigen
Wert natürlich der Wert selbst ist. Sind die Werte von $X$ und $Y$ schon um den
Mittelwert zentriert, sodass $\mathbb{E}[X] = \mathbb{E}[Y] = 0$, gilt somit
insbesondere:

$$\cov(X, Y) = \mathbb{E}[XY]$$

Sind also $X$ und $Y$ um den Mittelwert zentriert, so hängt die Kovarianz linear
vom Produkt $X \cdot Y$ ab. Der Wert $\cov(X, Y)$ der Kovarianz ist nun wie
folgt zu interpretieren:

* Ist $\cov(X, Y)$ positiv, so sind $X$ und $Y$ meist sehr gleich. Ist $X$
  positiv, so is ist $Y$ meist auch positiv und ist $X$ negativ, so ist $Y$
  meist auch negativ. $X$ und $Y$ sind also gewissermassen abhängig von
  einander. Man sagt dann auch, dass $X$ und $Y$ *korrelieren*.
* Ist $\cov(X, Y)$ negativ, so sind $X$ und $Y$ ebenso abhängig voneinander,
  aber indirekt. Wenn der Wert von $X$ positiv ist, ist $Y$ meist negativ und
  umgekehrt. $X$ und $Y$ würden also noch immer korrelieren.
* Ist $\cov(X, Y) = 0$, so besteht keine Korrelation zwischen $X$ und $Y$. Wen
  $X$ positiv ist, so beeinflusst das den Wert und das Vorzeichen von $Y$ nicht.
  Im Schnitt heben sich die Werte dann auf. Die Zufallsvariablen sind also
  vollkommen *unabhängig*. Man sagt dann, dass $X$ und $Y$ nicht *korrelieren*.

Gerade Letzteres sieht man in obiger Formel schön: Sind $X$ und $Y$ nämlich
unabhängig, so ist $\mathbb{E}[XY] = \mathbb{E}[X] \cdot \mathbb{E}[Y]$. Dann
ergibt $\mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y]$ also Null.

Für diskrete Zufallsvariablen $X$ und $Y$ ergibt sich die Kovarianz $\cov(X, Y)$
rechnerisch dann durch folgenden Ausdruck:

$$\cov(X, Y) = \frac{1}{n}\sum_{i=1}^n (x_i - \mu)(y_i - \gamma)$$

#### Kovarianzmatrix

Betrachten wir nun *Zufallsvektoren* $\mathbf{X}$ und $\mathbf{Y}$, wobei also
jede Komponente der beiden Vektoren jeweils für sich zufallsverteilt
ist. Rechnerisch ergibt sich die Kovarianz $\cov(\mathbf{X}, \mathbf{Y})$ dann
durch

$$
\begin{align}
	\cov(\mathbf{X, Y}) &=
	\mathbb{E}[(\mathbf{X} - \mathbb{E}[\mathbf{X}])(\mathbf{Y} -
	\mathbb{E}[\mathbf{Y}])^\top] \\
	                    &= \mathbb{E}[\mathbf{X} \mathbf{Y}^\top] -
						\mathbb{E}[\mathbf{X}]\mathbb{E}[\mathbf{Y}]^\top
\end{align}
$$

Sind $\mathbf{X, Y}$ im Weiteren dann $0$-zentriert, erhalten wir:

$$\cov(\mathbf{X, Y}) = \mathbb{E}[\mathbf{X} \mathbf{Y}^\top]$$

Und sind alle Zufallsvariablen in $\mathbf{X, Y}$ gleichverteilt, bleibt:

$$\cov(\mathbf{X, Y}) = \frac{1}{n}\mathbf{X} \mathbf{Y}^\top$$

Betrachten wir nun ein konkretes Zufallsexperiment. Dann sind die
Zufallsvariablen $\mathbf{X, Y}$ hier gleichbedeutend mit jeweils $n$
Ausprägungen dieser Vektoren $\mathbf{X, Y}$, also behandeln wir $\mathbf{X, Y}$
im Weiteren als Matrix, mit den Ausprägungen, also konkreten Datenvektoren, in
den Spalten von $\mathbf{X}$ bzw. $\mathbf{Y}$. Die Kovarianz ergibt sich dann
als Matrixprodukt von $\mathbf{X}$ mit $\mathbf{Y}^\top$.

Wir können uns dieses Produkt veranschaulichen, wenn wir annehmen, dass $n = 1$,
sodass $\mathbf{X}$ nur einen Spaltenvektor enthält und $\mathbf{Y}^\top$ nur
einen Zeilenvektor (wegen der Transponierung). Das Produkt $\mathbf{XY^\top}$
sähe dann so aus:

$$
\begin{bmatrix}
 x_1\\
 x_2\\
 \vdots\\
 x_n
\end{bmatrix}
\cdot
\begin{bmatrix}
y_1 & y_2 & \dots & y_n
\end{bmatrix}
$$

und hätte die Semantik, dass jede Komponente $x_i$, also der Wert jeder
Zufallsvariable in $\mathbf{X}$ mit jeder Komponente $y_i$ in $\mathbf{Y}$
multipliziert wird. Da wir annehmen, dass $\mathbf{X}$ und $\mathbf{y}$
null-zentriert sind, also Mittelwert $0$ haben, ergibt dieses Produkt $x_iy_i$
jeweils gerade die Kovarianz von $\mathbf{X}_i$ mit $\mathbf{Y}_i$. Das ist
hierbei aber nur so, weil $n = 1$, sodass wir für jede Zufallsvariable
$\mathbf{X}_i$ und $\mathbf{Y}_i$ auch nur diese eine Komponente jeweils
haben. Dann ist die Kovarianz $\cov(\mathbf{X}_i, \mathbf{Y}_i)$ naemlich
gegeben durch:

$$
\begin{align}
\cov(\mathbf{X}_i, \mathbf{Y}_i) &= \frac{1}{1} \sum_{1}^1 (x_i -
\mathbb{E}[X])(y_i - \mathbb{E}[Y]) \\
                                 &= (x_i - 0)(y_i - 0) \\
								 &= x_iy_i
\end{align}
$$

Die Matrix $\cov(\mathbf{X, Y})$ sieht dann also so aus:

$$
\cov(\mathbf{X}, \mathbf{Y}) =
\begin{bmatrix}
\cov(\mathbf{X}_1, \mathbf{Y}_1) & \cov(\mathbf{X}_1, \mathbf{Y}_2) & \dots &
\cov(\mathbf{X}_1, \mathbf{Y}_n) \\
\cov(\mathbf{X}_2, \mathbf{Y}_1) & \cov(\mathbf{X}_2, \mathbf{Y}_2) & \dots &
\cov(\mathbf{X}_2, \mathbf{Y}_n) \\
\vdots & \vdots & \ddots & \vdots \\
\cov(\mathbf{X}_n, \mathbf{Y}_1) & \cov(\mathbf{X}_n, \mathbf{Y}_2) & \dots &
\cov(\mathbf{X}_n, \mathbf{Y}_n) \\
\end{bmatrix}
$$

Was sich im Fall eines einzigen Datenpunktes reduziert auf:

$$
\cov(\mathbf{X}, \mathbf{Y}) =
\begin{bmatrix}
x_1y_1 & x_1y_2 & \dots  & x_1y_n\\
x_2y_1 & x_2y_2 & \dots  & x_2y_n\\
\vdots & \vdots & \ddots & \vdots\\
x_ny_1 & x_ny_2 & \dots  & x_ny_n
\end{bmatrix}
$$

Haben wir mehr als einen Datenpunkt pro Variable, also gilt $n > 1$, so artet
$\mathbf{XY}$ zum Skalarprodukt jeder Zeile von $\mathbf{X}$ mit jeder Spalte
von $\mathbf{Y}$ aus. Der einzige Unterschied zu vorhin ist, dass wir nun viele
Ausprägungen für jede Zufallsvariable $\mathbf{X}_i$ und $\mathbf{Y}_i$
haben. Somit ergibt sich die Kovarianz $\cov(\mathbf{X}_i, \mathbf{Y}_j)$ eben
durch obige Formel:

$$\cov(\mathbf{X}_i, \mathbf{Y}_i) = \frac{1}{n}\sum_{k=1}^n x_{i,k}y_{k,j} =
\frac{1}{n}\langle \mathbf{X}_i, \mathbf{Y}_i \rangle$$

Die Komponente $(i,j)$ der Kovarianzmatrix $\cov(\mathbf{X}_i, \mathbf{Y}_j)$
ergibt sich also aus dem normierten Skalarprodukt $\mathbf{X}_i
\mathbf{Y}_j^\top$ (Anmerkung: $\mathbf{X}_i$ sind hier Zeilenvektoren, denn es
ist ja die $i$-te Komponente jedes Spaltenvektors $\in \mathbf{X}$).

Interessant ist im Übrigen folgende Eigenschaft der Kovarianz(matrix):

$$\cov(\mathbf{X}, \mathbf{Y}) = \cov(\mathbf{Y}, \mathbf{X})^\top$$

##### $\mathbf{X} = \mathbf{Y}$

Der nächste Schritt, welcher für die Hauptkomponentenanalyse wichtig ist, ist es
nun, nur mehr einen Zufallsvektor $\mathbf{X}$ zu betrachten. Wir können auch
hier die Kovarianz untersuchen, aber nun eben zwischen den Zufallsvariablen von
$\mathbf{X}$ mit jeweils allen anderen in $\mathbf{X}$. Vorher hatten wir ja nur
die Kovarianz der Werte in $\mathbf{X}$ mit jenen in $\mathbf{Y}$ berechnet. Auf
ähnliche Weise wie vorhin erhalten wir nun so die folgende Matrix:

$$
\cov(\mathbf{X}, \mathbf{X}) =
\begin{bmatrix}
\cov(\mathbf{X}_1, \mathbf{X}_1) & \cov(\mathbf{X}_1, \mathbf{X}_2) & \dots &
\cov(\mathbf{X}_1, \mathbf{X}_n) \\
\cov(\mathbf{X}_2, \mathbf{X}_1) & \cov(\mathbf{X}_2, \mathbf{X}_2) & \dots &
\cov(\mathbf{X}_2, \mathbf{X}_n) \\
\vdots & \vdots & \ddots & \vdots \\
\cov(\mathbf{X}_n, \mathbf{X}_1) & \cov(\mathbf{X}_n, \mathbf{X}_2) & \dots &
\cov(\mathbf{X}_n, \mathbf{X}_n) \\
\end{bmatrix}
$$

Das ist nun die Kovarianzmatrix, mit welcher wir in der Hauptkomponentenanalyse
arbeiten werden. Eine wichtige Eigenschaft dieser Matrix ist, dass sie in der
Diagonalen jeweils die Varianz von $\mathbf{X}_i$ enthält, denn es gilt:

$$\cov(\mathbf{X}_i, \mathbf{X}_i) = \mathbb{E}[\mathbf{X}_i \mathbf{X}_i^\top]
= \Var(\mathbf{X}_i)$$

Hierbei wieder die Anmerkung, dass wir mit null-zentrierten Zufallsvariablen
arbeiten, sodass für eine Zufallsvariable $Z$ dann $\Var(Z) = \mathbb{E}[Z^2] =
\mathbb{E}[(Z - \mathbb{E}[Z])^2]$ gilt. Wir haben dann auch noch den folgenden
schönen Ausdruck für die Kovarianz $\cov(\mathbf{X, X})$ wenn die
Wahrscheinlichkeiten uniform sind:

$$\cov(\mathbf{X, X}) = \frac{1}{n} \mathbf{X} \mathbf{X}^\top$$

Wie oben angemerkt, gilt allgemein $\cov(\mathbf{X, Y}) = \cov(\mathbf{Y,
X})^\top$. Wenn nun $\mathbf{X} = \mathbf{Y}$ ergibt sich hieraus gerade, dass
die Kovarianzmatrix symmetrisch sein muss:

$$\cov(\mathbf{X, X}) = \cov(\mathbf{X, X})^\top$$

$\cov(\mathbf{X, X})$ ist also eine quadratische, symmetrische Matrix.

### Diagonalisierung

Wenn wir nun eine Datenmenge $\mathbf{X}$ mit Datenpunkten $\mathbf{x}$ in den
Spalten haben, wissen nun also, wie man die Kovarianzmatrix $\cov(\mathbf{X, X})
= \frac{1}{n} \mathbf{X} \mathbf{X}^\top$ bildet. Wir wissen nun auch, dass
Werte jenseits der Diagonalen in dieser Matrix die *Kovarianz* der
Zufallsvariablen, also hier nun der Attribute, darstellt. Wie oben angemerkt
bedeutet eine Kovarianz ungleich null, dass zwei Attribute korrelieren, also
irgendwie (direkt oder indirekt) von einander abhängen. Das bedeutet wiederum,
dass das eine Attribut redundant ist. Wenn Attribut $\mathbf{X}_1$, z.B. Alter,
schon eindeutig durch Attribut $\mathbf{X}_2$, z.B. Größe, bestimmt wird, so ist
es für uns nicht interessant, beide Attribute zu behalten. Am liebsten hätten
wir also unsere Attribute so gewählt, dass die Kovarianz zwischen ihnen Null
ist, also dass sie vollkommen unabhängig sind. Dann sagt uns jedes Attribut
(also jede Dimension im Raum) etwas vollkommen neues über einen Datenpunkt aus
-- etwas, dass uns ein anderes Attribut nicht auch sagen kann.

Des Weiteren hat die Kovarianzmatrix in diesem Fall nun ja auch die Eigenschaft,
die Varianz in der Diagonalen zu haben. Wir wollen, dass die Werte jenseits der
Diagonalen, also die Kovarianzen, Null sind. Die Varianzen, also die Werte *auf*
der Diagonalen, sollen aber natürlich nicht Null sein (sonst hätten wir keine
Varianz in unseren Daten, das wäre sehr schlecht). Was das also insgesamt
bedeutet, ist dass uns für unsere Kovarianzmatrix am liebsten eine
Diagonalmatrix wäre. Wir suchen also eine *Diagonalisierung* $\mathbf{D_X}$
unserer ursprünglichen Matrix $\mathbf{C_X}$. Da die Kovarianzmatrix
$\mathbf{C_X}$ quadratisch, und insbesondere *symmetrisch* ist, wissen wir auch,
dass es eine solche Diagonalmatrix gibt.

Versuchen wir aber zunächst, eine passende Kovarianzmatrix $\mathbf{D_X}$ zu
finden. Das würden wir so machen, dass wir zunächst einen einzigen Vektor
$\mathbf{p}_1$ wählen, in dessen (normalisierter) Richtung die Varianz von
$\mathbf{X}$ maximal ist. Wir basteln uns also einfach mal einen solchen Vektor
$\mathbf{p}_1$. Damit die Kovarianzmatrix dann diagonal bleibt, muss der nächste
Vektor $\mathbf{p}_2$ dann orthogonal auf $\mathbf{p}_1$ sein. Denn die
Kovarianz $\cov(\mathbf{p}_1, \mathbf{p}_2)$ war ja gerade das Skalarprodukt
$\frac{1}{n}\langle \mathbf{p}_1, \mathbf{p}_2 \rangle$. Wenn $\mathbf{p}_1,
\mathbf{p}_2$ orthogonal sind, wird der Eintrag $(1,2)$ bzw. $(2, 1)$ somit
sicher Null sein. Auch intuitiv wird es dann eben keine Verbindung
bzw. Abhängigkeit zwischen diesen Attributen geben, sodass sie vollkommen
unkorreliert sind. Wir erhalten so letztendlich eine geordnete Menge $\mathbf{P}
= (\mathbf{p}_1, \mathbf{p}_2, \dots, \mathbf{p}_n)$ mit den Vektoren
$\mathbf{p}_i$ in den *Spalten*. Diese Vektoren, welche man *Hauptkomponenten*
nennt, sind unsere neuen Attribute, also insbesondere Basisvektoren des
Datenraumes.

Eben wurde beschrieben, wie man diese Basis $\mathbf{P}$ in etwa finden würde,
bzw. nach welchen Eigenschaften man für diese Hauptkomponenten $\mathbf{p}_i$
suchen würde (Orthogonalität und maximale Varianz). Es wurde aber noch nicht
konkret ein Verfahren angegeben, um sie genau zu bestimmen. Wir wissen hierfür
nur zwei Tatsachen. Zum Einen, dass $\mathbf{P}$ eine Orthonormalbasis sein
soll. Zum Anderen, dass die neue Darstellung $\mathbf{Y}$ unserer Datenmenge
durch $\mathbf{Y} = \mathbf{P}^{-1} \mathbf{X}$ hervorgehen soll. Denn wenn die
Matrix $\mathbf{P}$ eine Basis ist, so ist sie auch eine Basiswechselmatrix aus
$\mathbf{P}$ in die Standardbasis. Ihr Inverses $\mathbf{P}^{-1}$ ist dann also
eine Basiswechselmatrix von der Standardbasis in diese neue, bessere Basis
$\mathbf{P}$. Da $\mathbf{P}$ orthonormal ist, ist $\mathbf{P}^{-1}$ dann im
Uebrigen dasselbe wie $\mathbf{P}^\top$. Das heisst, dass folgende Eigenschaft
gelten soll:

$$\mathbf{Y} = \mathbf{P}^\top \mathbf{X}$$

Für die Kovarianzmatrix $\mathbf{C_Y} = \cov(\mathbf{Y, Y})$ haben wir dann den
Ausdruck

$$\mathbf{C_Y} = \frac{1}{n} \mathbf{Y} \mathbf{Y}^\top.$$

Nun können wir obiges einsetzen, und erhalten so durch Umformung:

$$
\begin{align}
\mathbf{C_Y} &= \frac{1}{n} \mathbf{Y} \mathbf{Y}^\top\\
             &= \frac{1}{n} (\mathbf{P}^\top \mathbf{X})(\mathbf{P}^\top
			 \mathbf{X})^\top\\
			 &= \frac{1}{n} (\mathbf{P}^\top \mathbf{X})(\mathbf{X}^\top
			 \mathbf{P})\\
			 &= \mathbf{P}^\top \frac{1}{n} \mathbf{X}\mathbf{X}^\top
			 \mathbf{P}\\
			 &= \mathbf{P}^\top \mathbf{C_X} \mathbf{P}
\end{align}
$$

Wir haben also herausgefunden, dass sich $\mathbf{C_X}$ sich durch
$\mathbf{P}^\top \mathbf{C_X} \mathbf{P}$ ergibt. Das sieht verdächtig nach
einer Diagonalisierung von $\mathbf{C_X}$ aus. Könnte es denn sein, dass
$\mathbf{P}$ vielleicht aus Eigenvektoren von $\mathbf{C_X}$ besteht? Dann
hätten wir nämlich durch $\mathbf{P}$ die Basistransformation aus $\mathbf{P}$
in die Standardbasis, und mit $\mathbf{P}^\top = \mathbf{P}^{-1}$ dann die
entsprechende Transformation aus der Standardbasis in die
Eigenvektorbasis. $\mathbf{C_X}$ wäre dann also die Kovarianzmatrix unserer
Datenmenge zur Standardbasis und $\mathbf{C_Y}$ zur Eigenvektorbasis. Dann
wüssten wir nämlich auch sicher, dass $\mathbf{C_Y}$ eine Diagonalmatrix ist. Da
im Weiteren auch bekannt ist, dass es für jede symmetrische Matrix eine
Orthonormalbasis des Raumes gibt, die aus Eigenvektoren dieser symmetrischen
Matrix besteht, könnte das tatsächlich funktionieren.

Wir prüfen das nun. Wir benutzen erstmal nur, dass $\mathbf{C_X}$ als
symmetrische, rellwertige Matrix sicher diagonalisierbar ist. Es existiert also
eine invertierbare Matrix $\mathbf{E} \in \GL_n(\mathbb{R})$, sodass es eine
Diagonalmatrix $\mathbf{D}$ gibt, mit der Eigenschaft:

$$\mathbf{D} = \mathbf{E}^{-1} \mathbf{C_X} \mathbf{E}.$$

Da $\mathbf{E}$ eine Orthonormalbasis wäre, gilt sogar:

$$\mathbf{D} = \mathbf{E}^\top \mathbf{C_X} \mathbf{E}.$$

Durch Umformung haben wir dann weiter:

$$\mathbf{C_X} = \mathbf{E} \mathbf{D} \mathbf{E}^\top.$$

Wenn wir nun $\mathbf{C_X}$ in dieser Schreibweise in die obige Formel von
$\mathbf{C_Y}$ einsetzen, dann müsste, wenn unsere Vermutung stimmt, rechts
$\mathbf{D}$ auskommen. Dann wüssten wir also, dass die Kovarianzmatrix gerade
die Diagonalisierung von $\mathbf{C_X}$ ist. Dann müsste $\mathbf{P} =
\mathbf{E}$ sein und die Hauptkomponenten wären gerade die Eigenvektoren der
Kovarianzmatrix $\mathbf{C_X}$. Die Varianzen unserer Daten wären dann sogar
gerade die Eigenwerte von $\mathbf{C_X}$! Probieren wir das also aus:

$$
\begin{align}
\mathbf{C_Y} &= \mathbf{P}^\top \mathbf{C_X} \mathbf{P}\\
	         &= \mathbf{P}^\top (\mathbf{E} \mathbf{D} \mathbf{E}^\top)
			 \mathbf{P}\\
			 &= \mathbf{P}^\top (\mathbf{P} \mathbf{D} \mathbf{P}^\top)
			 \mathbf{P}\\
			 &= (\mathbf{P}^\top \mathbf{P}) \mathbf{D} (\mathbf{P}^\top
			 \mathbf{P})\\
			 &= (\mathbf{P}^{-1} \mathbf{P}) \mathbf{D} (\mathbf{P}^{-1}
			 \mathbf{P})\\
			 &= I_n \mathbf{D} I_n\\
			 &= \mathbf{D}
\end{align}
$$

Es stimmt also, die gesuchte Kovarianzmatrix $\mathbf{C_Y}$ ist gerade die
Diagonalisierung von $\mathbf{C_X}$! Dann stimmen also alle obigen
Behauptungen. Die Hauptkomponenten sind gerade die Eigenvektoren von
$\mathbf{C_X}$ und die entsprechenden Varianzen, also Diagonaleinträge von
$\mathbf{C_Y}$, die Eigenwerte zu jedem Eigenvektor bzw. jeder Hauptkomponente.

<!-- Frage: wieso sind die Kovarianzmatrizen ähnlich? -->

<!-- Wenn $\mathbf{C_Y}$ aus der Diagonalisierung von $\mathbf{C_X}$ hervorgeht, so -->
<!-- sind diese beiden Kovarianzmatrizen also *ähnlich*. Wir können uns noch kurz -->
<!-- überlegen, wieso die Aehnlichkeit zwischen diesen Matrizen gegeben ist. Wir -->
<!-- nennen zwei Matrizen *ähnlich*, wenn sie dieselbe lineare Transformation $f: V -->
<!-- \rightarrow W$ sind, aber zu verschiedenen Basen. Genaür ist die eine Matrix -->
<!-- dann die Darstellungsmatrix der linearen Abbildung zur einen Basis, und die -->
<!-- andere Matrix die Darstellungsmatrix zur anderen Basis. Wieso gilt das hier? -->
<!-- Nun, beide Matrizen drücken ja gerade dasselbe aus: Die Kovarianz unserer -->
<!-- Daten. Sie sind beide dieselbe Abbildung unserer Datenmenge $\mathbf{X}$ auf die -->
<!-- Kovarianzmatrix $\cov(\mathbf{X, X})$. Diese Funtion wäre also: -->

<!-- $$f: \mathbb{R}^{m \times n} \rightarrow \mathbb{R}^{m \times m}: \mathbf{X} -->
<!-- \mapsto \cov(\mathbf{X, X})$$ -->

<!-- Sie bilden also jeweils eine $m \times n$ Matrix ($m$-Attribute in den Zeilen, -->
<!-- $n$ Datenpuntke in den Spalten) auf die $m \times m$ Kovarianzmatrix, die die -->
<!-- Kovarianz der Attribute ausdrückt. Nur ist $\mathbf{C_X}$ diese Kovarianzmatrix -->
<!-- zur Standardbasis, und $\mathbf{C_Y}$ die Kovarianzmatrix zur Eigenvektorbasis. B -->


### Dimensionsreduktion

Wenn wir also die Kovarianzmatrix $\mathbf{C_X}$ zu unserer Datenmenge
$\mathbf{X}$ bilden und dann die Eigenwerte finden, so sind das die Varianzen
der einzelnen Hauptkomponenten. Wir können die Eigenwerte noch absteigend
sortieren und dann die Diagonalmatrix $\mathbf{C_Y}$ bilden. Beispielsweise sähe
das so aus:

$$
\mathbf{C_Y} =
\begin{bmatrix}
8 & 0 & 0 & \dots &\\
0 & 4 & 0 & \dots \\
0 & 0 & 0.2 & \dots \\
0 & 0 & 0 & \ddots & \\
0 & 0 & 0 & 0 & 0.001
\end{bmatrix}
$$

Wir sehen hier also, dass die Varianz unserer Daten entlang der ersten
Hauptkomponente (des erstes Eigenvektors) acht ist. Entlang der Zweiten ist sie
vier, und danach nur mehr $0.2$. Das sagt uns also, dass unsere Daten entlang
der ersten und zweiten Achse am meisten Information tragen. Dort gibt es nämlich
manche Vektoren, die weit vom Ursprung weg sind und somit sehr besonders
sind. Entlang der dritten Hauptkomponente sind Vektoren ziemlich gleich, sodass
dieses Attribut nur mehr wenig zu unseren Daten aussagt. Wir würden nun also
einfach alle Attribute ausser den ersten beiden ignorieren, und unsere Daten nur
mehr als Paare $\mathbf{x} = (x_1, x_2)$ darstellen, wobei $x_1$ das Attribut
entlang der ersten Hauptkomponente $\mathbf{p_1}$ ist und $x_2$ entlang der
zweiten. Der Wert des Attributes $x_i$ ist hier dann eben gerade die Koordinate
des Basisvektors $\mathbf{p}_i$.

Hatte unser Datensatz vorher $50$ Attribute, so hätten wir ihn jetzt auf nur
zwei reduziert. Wenn jeder ein Integer war, so verbrauchen wir jetzt $48$ Mal
weniger Speicherplatz pro Vektor. Auch können wir die Daten jetzt entlang dieser
Achsen plotten und genau sehen, wie ähnlich oder verschieden bestimmte Daten
sind. So merken wir vielleicht, dass es ein Datum in unserer Menge gibt, das
vollkommen verschieden von den anderen ist. Das ist für uns dann womöglich
interessant. Wir könnten zu unseren ursprünglichen Daten zurückgehen und merken,
dass sich dieses Datum tatsächlich in einem oder mehreren Attributen von den
anderen stark unterscheidet. Erst die Hauptkomponentenanalyse hat uns darauf
aufmerksam gemacht. Es hat uns nämlich eine neue Sicht auf unsere Daten gegeben,
wo wir die Struktur und Besonderheiten der Daten leichter erkannt haben.

Hierbei sei angemerkt, dass sich die Hauptkomponentenanalyse wirklich nur dazu
eignet, solche Strukturen bzw. Eigenheiten in den Daten zu finden. Die
Hauptkomponenten selbst als Attribute zu benutzen ist unintutiv. Denn eine
Hauptkomponente ist leider kein leicht verständliches Attribut wie Alter,
Gewicht oder Umfang. Es ist einfach ein Eigenvektor der Kovarianzmatrix. Er
eignet sich also nur dazu, die Eigenschaften der Daten leichter erkenntlich zu
machen.

Da ueber die Basiswechselmatrix $\mathbf{E}$ (die Matrix von Eigenvektoren) auch
eine (verlust-behafetete) Rekonstruktion von Vektoren zur Basis der
Hauptkomponenten wieder zurück in ihre ursprüngliche Basis möglich ist, können
wir unsere Daten mit weniger Dimensionen (und Speicherplatz) speichern und dann
später wiederherstellen. Da nur jene Dimensionen verloren gehen, die sowieso
wenig Informationen enthielten, ist uns das dann auch egal.

[Hier](http://setosa.io/ev/principal-component-analysis/) eine tolle
Visualisierung eines praktischen Beispiels der Hauptkomponentenanalyse.

## Code

Die Implementierung der Hauptkomponentennalyse in Code ist erstaunlich einfach
und kurz:

```Python
def pca(X, cutoff=2):
    """
    Performs principal components analysis on a data-set.

    Arguments:
        X (np.array): A collection of data vectors, ordered in columns.
        cutoff (int): How many principal components to find (2).

    Returns:
        The principal components along with the variance
        of the data along each component.
    """
    # Mean-normalize the data to make it easier
    # to compute the co-variance matrix
    X -= np.mean(X, axis=1).reshape(2, 1)

    # Compute the covariance matrix
    cov = (1.0/X.shape[1]) * X.dot(X.T)

    # Find the principal components (eigenvectors) and
    # variances (eigenvalues) of the data.
    variances, components = np.linalg.eig(cov)

    # The components are in the columns, want them separately (in rows)
    components = components.T

    # Now we can zip each variance with its
    # corresponding row (principal component)
    # This gives us a tuple of (variance, component) pairs
    result = zip(variances, components)

    # Sort according to variance, in descending order
    result.sort(key=lambda e: e[0], reverse=True)

    # Return the first 'cutoff' variances and principal components
    return result[:cutoff]
```

Eine volle Implementierung findet sich
[hier](https://gist.github.com/goldsborough/fddb08539547ed841e121b686638d36b).

## Zusammenfassung

Hier sind noch zusammenfassend die Schritte kompoakt aufgelistet, über welche
man eine Hauptkomponentenanalyse mit Dimensionreduktion auf einem Datensatz
$\mathbf{X} \in \mathbb{R}^{m \times n}$ machen kann. Dieser Datensatz hat also
$n$ Datenpunkte mit jeweils $m$ Attributen in den Spalten.

1. Daten um den Nullpunkt zentrieren: $\mathbf{X}' := \mathbf{X} -
   \mathbb{E}^\star[\mathbf{X}]$.
2. Kovarianzmatrix aufstellen: $\mathbf{C} = \cov(\mathbf{X', X'}) = \frac{1}{n}
   \mathbf{X'} \mathbf{X'}^\top$
3. Finde Eigenwerte und Eigenvektoren von $\mathbf{C}$.
4. Ordne die Eigenvektoren absteigend nach Eigenwert.
5. Wähle optional nur jene Hauptkomponenten (Eigenvektoren), mit höchster
   Varianz (Eigenwerten).
6. Die verbleibenden Hauptkomponenten ergeben als Spalten angeordnet die neue
   Basis der Daten.

Hierbei ist mit $\mathbb{E}^\star$ ist der attribut-weise Mittelwert
gemeint. Das ist dann also ein Spaltenvektor $\mathbf{\mu} = (\mu_1, \dots,
\mu_m)^\top$ mit den Mittelwerten jedes Attributs als Komponenten. Dieser wird
dann über die Spalten von $\mathbf{X}$ "gebroadcastet", sodass von jedem
Attribut in $\mathbf{X}$ der entsprechende Mittelwert dieses Attributs abgezogen
wird.
