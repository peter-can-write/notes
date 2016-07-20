# Zusammenfassung

## Matrizen

Seien $A \in K^{m \times n}$ und $B \in K^{n \times k}$ zwei Matrizen, dann
gilt:

* $A + B$ ergibt sich elementweise. Hierfuer muss $m = k$.
* $A \circ B$ ergibt sich elementweise (Hadamard Produkt). Hierfuer muss $m = k$.
* $s \cdot A$ ergibt sich elementweise.
* $A \cdot B$ ergibt sich durch Skalarprodukt jeder Zeile von $A$ mit jeder
  Spalte von $B$.
* $A \cdot B \in K^{m \times k}$
* $A$ muss gleich viele Spalten haben wie $B$ Zeilen hat.
* $A^\top$ ergibt sich durch Vertauschen von Zeilen mit Spalten.
* Gilt $A = A^\top$, so nennt man $A$ symmetrisch.
* $Av \in K^m$ fuer Vektor $v \in K^n$

## Vektorraeume

### Vektorraume

Sei $V$ eine Menge von Vektoren, dann ist $V$ ein Vektorraum, wenn gilt:

* $\langle V, + \rangle$ ist eine abelsche Gruppe.
* $\langle V, \cdot \rangle$ ist distributiv fuer Skalare.
* $\langle V, \cdot \rangle$ ist kommutativ fuer Skalare.
* $\langle V, \cdot \rangle$ ist assoziativ.
* $\langle V, \cdot \rangle$ hat die $1$ als neutrales Element.

### Untervektorraeume

Sei $U$ ein Untervektorraum von $V$, dann gilt:

* $U \neq \emptyset$
* $U$ ist bezueglich allen Linearkombinationen seiner Elemente abgeschlossen.
* $u + v \in U$ fuer $u,v \in U$
* $s \cdot u \in U$ fuer $s \in K, u \in U$
* $U$ besitzt dasselbe neutrale Element (dieselbe Null) wie $V$.
* Jeder Untervektorraum ist ein Vektorraum.
* Jeder Untervektorraum ist eine Gerade durch den Nullpunkt.
* Ein affiner Untervektorraum $v + U$ mit $v \in V$ ist eine affine Gerade.

Seien nun $U_1, U_2 \subseteq V$ Unterraeume und $M$ ein Menge von
Untervektorraeumen, dann gilt:

* $U_1 \cap U_2$ ist ein Untervektorraum.
* $U_1 + U_2$ ist ein Untervektorraum.
* $\bigcap_{U \in M} U$ ist ein Untervektorraum.

### Erzeugnis

* $\langle U \rangle$ ist der minimale Unterraum, der $U$ enthaelt.
* $\langle U \rangle$ ist der Schnitt aller Unterraeume, die $U$ enthalten.
* $\langle U_1 \cup U_2 \rangle = U_1 + U_2$

## Lineare Abbildungen

Hier eine Zusammenfassung der wichtigsten Eigenschaften, die in diesem Kapitel
besprochen wurden.

### Lineare Abbildungen

Sei $f: V \rightarrow W$ eine lineare Abbildung, dann gilt:

* $f(v + w) = f(v) + f(w)$
* $f(av) = a f(v)$
* $f(0) = 0$
* $\Kern(f) = \{ v \in V \,|\, f(v) = 0\}$
* $\Bild(f) = \{ w \in W \,|\, \exists v \in V: f(v) = w\}$
* Jeder $k$-dimensionale Vektorraum ist isomorph zum $K^n$
* Sei $K$ die Basis von $\Kern(f)$ und $U$ die Basis von $f^{-1}(\Bild(f))$ (die
  Urbilder der Basis des Bildes), dann ist $K \uplus U$ eine Basis von $f$
* $\dim(V) = \dim(\Kern(f)) + \dim(\Bild(f))$ fuer $f: V \rightarrow V$
* Fuer $f: V \rightarrow W$ mit $\dim(V) = \dim(W)$ gilt:
  $\text{surjektiv } \iff \text{ injektiv } \iff { bijektiv }$
* $\Kern(f_A) = \dim(V) - \dim(\Bild(f_A)) = n - \rang(A)$
* Spaltenrang ($\dim(\Bild(f))$) ist gleich Zeilenrang $\rang(A)$
* Die Anzahl linear unabhaengiger Zeilen gleicht jener der Spalten

### Inverse

Sei $A$ eine Matrix, dann gilt:

* Finde $A^{-1}$ durch $(A | I_n)$ in Zeilenstufenform.
* $A^{-1} A = A A^{-1}$
* $A^{-1}$ existiert nur, wenn $A$ quadratisch.
* $GL_n(K)$ ist die Menge aller invertierbaren $n \times n$ Matrizen ueber $K$.

### Darstellungsmatrizen

Sei $f: V \rightarrow W$ eine lineare Abbildung, dann gilt:

* $f$ ist durch das Bild der Basis von $V$ eindeutig bestimmt.
* Jede Menge von $n$ Vektoren in $W$ tritt als Bild der Basisvektoren von $V$
  fuer eine lineare Abbildung auf.
* Finde $D_{B,C}$ durch Abbildung von $B$ durch $f$ und Darstellung des Bildes
  zur Basis $C$.
* Sei $D_{B,C}$ die Darstellungsmatrix von $f$, dann gilt: $f(v) = D_{B,C}v$
* $D_{B,C} \cdot D_{C,E} = D_{B,E}$

Sei nun $A \in K^{m \times n}$ und $f: K^n \rightarrow K^m: v \mapsto Av$, dann:

* $f_A \circ f_B = f_{AB}$
* $f_A^{-1} = f_{A^{-1}}$
* $f_A \circ f_{A^{-1}} = \Id$
* $A$ ist Darstellungsmatrix von $f_A$ zur Standardbasis:
  $A = D_{I_n,I_n}(f_A)$
* $A^{-1}$ ist Darstellungsmatrix von $f_A^{-1}$ zur Standardbasis:
  $A^{-1} = D_{I_n,I_n}(f_A^{-1})$
* $D_{B,I_n} = A \cdot B$
* $D_{B,C} = A \cdot B$ zur Basis $C$

### Basiswechsel

Sei $A, D$ Matrizen und $B, C$ Basen, dann gilt:

* $S_{B,C} = D_{C,B}(\Id)$
* $S_{B,C} = B^{-1} C$
* $S_{B,C}^{-1} = S_{C,B}$
* Aehnliche Matrizen haben dieselbe Determinante.
* Aehnliche Matrizen haben dasselbe charakteristische Polynom.
* Die Basiswechselmatrix $S_{I_n, B}$ ist $B$ selbst.
* Sei auch $E$ eine Basis, dann gilt: $S_{B,C} \cdot S_{C,E} = S_{B,E}$
* $Bx = Cy$ fuer Koordinaten $x,y$ von $v$ zur Basis $B,C$

### Aehnlichkeit

Seien $A,B$ zwei Matrizen, dann gilt:

* Sind $A,B$ aehnlich, so sind sie Darstellungsmatrizen derselben Funktion zu
  verschiedener Basis.
* $B = T^{-1} A S$ mit $S \in \GL_n(\mathbb{R})$ wenn $A,B$ aequivalent
* $B = S^{-1} A S$ mit $S \in \GL_n(\mathbb{R})$ wenn $A,B$ aehnlich
* $A = S B S^{-1}$ mit $S \in \GL_n(\mathbb{R})$ wenn $A,B$ aehnlich

## Basen

Hier die Zusammenfassung der wichtigsten Eigenschaften dieses Kapitels. Sei
hierzu $E$ ein Erzeugendensystem und $B$ eine Basis, sowie $V$ der entsprechende
Vektorraum. Dann gilt:

* $E$ ist Erzeugendensystem, wenn $\langle E \rangle = V$
* $B$ ist Basis, wenn $B$ linear unabhaengig und Erzeugendensystem
* Ist $B$ Basis und $v \in V$, gilt: $v = \sum_i a_i b_i$ fuer geeignete $a_i$
* Es sind aequivalent:
  1. $B$ ist Basis.
  2. $B$ ist inklusionsmaximal und linear unabhaengig.
  3. $B$ ist inklusionsminal und Erzeugendensystem.
* $|B| = \dim(V)$
* Sei $U$ eine linear unabhaengige Menge, dann gilt: $|E| \geq |U|$
* Sei $U$ eine linear unabhaengige Menge mit $|U| = \dim(V)$, dann ist $U$ Basis
  von $V$.
* Sei $U$ Unterraum von $V$, dann gilt:
  1. $\dim(U) \leq \dim(V)$
  2. $\dim(U) = \dim(V) \iff U = V$
* Alle Basen haben gleich viele Elemente, naemlich $\dim(V)$
* $\dim(K^n) = n$
* Sei

* Bilde aus $E$ eine Basis durch elementare Zeilenoperationen mit $E$ in den
  Zeilen einer Matrix.
* Sei $S$ eine linear unabhaengige Menge, dann ergaenze sie durch $(S | I_N)$ in
  Zeilenstufenform zu einer Basis.

## Determinante

### Permutationen

1. $\sgn(\sigma\tau) = \sgn(\sigma) \cdot \sgn(\tau)$
2. $\sgn(\sigma) \cdot \sgn(\sigma) = 1 \Rightarrow \sgn(\sigma) = \sgn(\tau)$
3. $\sgn(\sigma) = \sgn(\sigma^{-1}) = \sgn(\sigma)^{-1}$

### Determinante

1. $\det(A \cdot B) = \det(A) \cdot \det(B)$
2. $\det(A^{-1}) = \det(A)^{-1}$
3. $\det(A) = \det(A^\top)$
4. $\det(B) = \sgn(\sigma) \cdot \det(A)$, wenn $B$ durch $\sigma$ aus $A$
   hervorgeht.
5. Sind zwei Spalten oder Zeilen einer Matrix gleich, ist ihre Determinante $0$.
6. $A \text{ ist regulaer } \iff \det(0) \neq 0$
7. Sind $A$ und $B$ aehnlich, gilt $\det(A) = \det(B)$.
8. Sei $A = \diag(a_1, ..., a_n)$ dann gilt $\det(A) = \Pi_i a_i$
9. Sei $A$ eine obere oder untere Driecksmatrix mit Diagonale $\diag(a_1, ...,
   a_n)$ dann gilt $\det(A) = \Pi_i a_i$.
10. Sei $A$ in Block-Dreiecksgestalt mit $\begin{bmatrix}B & 0 \\ C &
    D\end{bmatrix}$. Dann gilt $\det(A) = \det(B) \cdot \det(D)$
11. Sei $A$ antisymmetrisch mit ungerader Dimension, dann ist $\det(A) = 0$.
12. Sei $A \in K^{2 \times 2}$, dann gilt $A^{-1} = \det(A)^{-1} \cdot \adj(A)$
13. Gauss Operationen:
	1. Vertauschen einer Zeile: Vorzeichen veraendert sich.
	2. Skalieren einer Zeile mit $s$: Determinante wird um $s$ skaliert.
	3. Skalieren der Matrix um $s$: Determinante wird um $s^n$ skaliert.
	4. Addieren des $s$-fachen einer Zeile auf eine andere: Determinante bleibt
       gleich.
14. Alles aus (13) auch fuer Spalten.
15. $\det(f) = \det(D_B(f))$ fuer beliebige Darstellungsmatrix $D_B(f)$.

### Aequivalenzen

Sei $A$ quadratisch, dann sind aequivalent:

* $A$ ist regulaer
* $A$ ist invertierbar
* Die Zeilen von $A$ sind linear unabhaengig
* Die Spalten von $A$ sind linear unabhaengig
* $f_A$ ist injektiv
* $f_A$ ist surjektiv
* $f_A$ ist bijektiv
* $Ax = b$ ist eindeutig loesbar
* $\kern(f_A) = \{0\}$
* $\det(A) \neq 0$

## Eigenvektoren

 1. Ist $v$ Eigenvektor von $A$ zu Eigenwert $\lambda$, gilt $Av = \lambda v$
 2. Das charakteristische Polynom ergibt sich aus $\chi_A = \det(xI_n - A)$
 3. Eigenraeume findet man durch: $E_\lambda = Kern(A - \lambda I_n)$
 4. $m_a(\lambda)$ (algebraisch) ist die Vielfachheit der Nullstelle $\lambda$
 5. $m_g(\lambda) = \dim(E_\lambda)$
 6. $1 \leq m_g(\lambda) \leq m_g(\lambda)$
 7. $E_\lambda = \{0\}$ gilt nur, wenn $\lambda$ nicht Eigenwert ist
 8. $\dim(E_\lambda)$ ist gleich den Freheitsgraden von $A - \lambda I_n$
 9. Aehnliche Matrizen haben dasselbe charakteristische Polynom.
10. Eigenwerte zu verschiedenen Eigenraeumen sind linear unabhaengig.
11. Ist $v$ Eigenvektor zu Eigenwert $\lambda$, gilt das auch fuer Vielfachen $av$
12. Eine Matrix $A$ ist genau dann diagonalisierbar, wenn gilt:
     1. $A$ zerfaellt in Eigenwerte aus demselben Raum wie $A$
     2. $m_a(\lambda) = m_g(\lambda) \forall \lambda$
     3. Die Basisvektoren der Eigenraeume von $A$ bilden eine Basis des $K^n$.
13. Ist eine Matrix $A$ diagonalisierbar, so ist $A$ aehnlich zu einer
    Diagonalmatrix $\diag(\lambda_1, ..., \lambda_r)$.
14. $\sum_\lambda m_g(\lambda) = \sum_\lambda m_a(\lambda) = n$
15. Jede Matrix, dessen charakteristisches Polynom in Linearfaktoren zerfaellt,
    ist aehnlich zu einer oberen Dreiecksmatrix, mit Eigenwerten in der
    Diagonalen (und diese kann in Jordan-Normalform gebracht werden).

## Hauptachsentransformation

### Symmetrische Matrizen

Sei $A$ eine symmetrische Matrix $\in \mathbb{R}^{n \times n}$, dann gilt:

* $A$ zerfaellt in relle Eigenwerte.
* Eigenvektoren von $A$ zu verschiedenen Eigenwerten sind orthogonal.
* Ist ein Unterraum $U \neq \{0\}$ bezueglich Multiplikation mit $A$
  abgeschlossen, so enthaelt $U$ einen Eigenvektor von $A$.

### Hauptachsentransformationssatz

* Es gibt eine Orthonormalbasis des $\mathbb{R}^n$ mit Eigenvektoren von $A$.
* $A$ ist transformierbar in eine orthonormale Diagonalmatrix.

### Definitheit

Sei $A$ eine symmetrische Matrix $\in \mathbb{R}^{n \times n}$ und $\lambda$ ein
*beliebiger* Eigenwert von $A$, dann nennt man $A$:

* *positiv definit*,      falls $\lambda > 0$
* *positiv semi-definit*, falls $\lambda \geq 0$
* *negativ semi-definit*, falls $\lambda \leq 0$
* *negativ definit*,      falls $\lambda < 0$
* *indefinit*,            wenn $\lambda > 0$ und $\lambda < 0$ moeglich

Sei $\circ$ eine Relation aus $\{>, \geq, \leq, <\}$ und $v \neq 0$ ein
beliebiger Vektor, dann gilt:

$$\lambda \circ 0 \iff \langle v, Av \rangle \circ 0$$

## Komplexe Zahlen

Hier die in diesem Kapitel besprochenen Eigenschaften.

1. $z = a + bi$ mit:
	* $z$: Komplexe Zahl
	* $a$: Realteil
	* $b$: Imaginaerteil
	* $i$: Imaginaere Einheit
2. $\bar(a + bi) = a - bi$
3. $|z| = \sqrt{z \cdot \bar{z}} = \sqrt{a^2 + b^2}$
4. $|z_1 z_2| = |z_1| \cdot |z_2|$
5. $\frac{z_1}{z_2} = \frac{z_1\bar{z_2}}{z_2\bar{z_2}}$
6. $(a + bi) + (c + di) = (a + c) + (b + d)i$
7. $(a + bi)(c + di) = (ac - bd) + (ad + bc)i$
8. $z^{-1} = \frac{\bar{z}}{|z|^2}$
9. $|z_1 + z_2| \leq |z_1| + |z_2|$
10. $|z| = 0 \iff z = 0$
11. $|z| \geq 0 \iff a \geq 0 \lor b \geq 0$
12.

## Lineare Gleichungssysteme

Hier die wichtigsten Erkenntnisse aus diesem Kapitel. Sei hierfuer $A \in K^{m
\times n}$ eine Matrix. Dann gilt:

### LGS allgemein

* $A$ ist in Zeilenstufenform, wenn fuer jede Zeile $z$ gilt:
  1. Faengt $z$ mit $k$ Nullen an, muessen unter diesen Nullen nur Nullen
     folgen.
  2. Ist $j_z$ der erste Eintrag ungleich Null, so muessen unter dieser Spalte
     nur Nullen folgen.

* $A$ ist in *strenger* Zeilenstufenform, wenn $A$ in Zeilenstufenform und fuer
  jede Zeile gilt:
  3. Ist $j_z$ der Erste Eintrag ungleich Null, duerfen ueber diesem Eintrag nur
     Nullen stehen.
* $\rang(A)$ ist durch Zeilen- und Spaltenzahl beschraenkt: $\rang(A) \leq \min\{m,n\}$
* $A$ ist regulaer, wenn $\rang(A) = n$
* $A$ ist singulaer, wenn $\rang(A) < n$
* $Ax = b$ ist:
  1. Eindeutig loesbar, wenn $\rang(A) = n$.
  2. Uneindeutig loesbar, wenn $\rang(A) < n$ und $\rang(A) = \rang(A | b)$
  2. Nicht loesbar, wenn $\rang(A) < \rang(A | b)$
* Wenn $Ax = b$ loesbar, ist $n - \rang(A)$ die Anzahl an Freiheitsgraden.
* Der Loesungsraum von $Ax = b$ ist jener von $Ax = 0$ plus einem Loesungsvektor
  von $Ax = b$

### Homogene LGS

* $Ax = 0$ ist immer loesbar.
* $\rang(A) = \rang(A | b) = \rang(A | 0)$
* Ist $A$ eindeutig loesbar, so gibt es nur die triviale Loesung $0$.
* Ist $A$ uneindeutig loesbar, so gibt es $n - \rang(A)$ Freiheitsgrade.

## Orthogonalitaet

### Skalarprodukt

Seien $v,w$ zwei Vektoren. Dann gilt:

* $\langle v, w \rangle = v^\top w = \sum_i v_i w_i$
* $\langle u, v + aw \rangle = \langle u,v \rangle + a \langle u, w \rangle$
* $\langle v, w \rangle = \langle w, v \rangle$
* $(K^n)^\perp = \{0\}$

### Laenge (Betrag)

* $|v| = \sqrt{\langle v, v \rangle} = \sqrt{v^\top v}$
* $|v|^2 = \langle v, v \rangle = v^\top v$
* $|av| = |a||v|$
* $|v + w| \leq |v| + |w|$ (Dreiecksungleichung)
* $| \langle v, w \rangle | \leq |v||w|$ (Cauchy-Schwarz)
* $|v| = 0 \iff v = 0$
* $v_0 = v/|v|$ (Normierter Vektor; Einheitsvektor)

### Orthogonalitaet

Sei $A \in \mathbb{R}^{n \times n}$ eine orthogonale Matrix (und dessen Spalten
entsprechend ein Orthonormalsystem). Dann gilt:

* $\langle v_i, v_j \rangle = \delta_{i,j}$ (fuer Spalten $v_i, v_j$)
* $A^\top = A^{-1}$
* $|v_i| = 1$
* $A^k = I_n$, wenn $k$ gerade, sonst $A$
* $|Av| = |v|$
* $\langle Av, Aw \rangle = \langle v, w \rangle$
* Die Spalten von $A$ sind linear unabhaengig
* $A$ ist eine Basis des $\mathbb{R}^n$
* Gram-Schmidt Update:
  1. $w_i = v_i - \sum_j^m (u_j^\top v) u_j$
  2. $u_i = w_i/|w_i|$
* Jeder Unterraum des $\mathbb{R}^n$ hat eine Orthnormalbasis (via Gram-Schmidt)
* Jede orthogonale Matrix $A$ ist invertierbar, da $A^{-1} = A^\top$
* $U^\perp$ ist $\Kern(B^\top)$ mit Basis $B$ von $U$

## Hauptkomponentenanalyse

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

## Spektrale Graphentheorie

Hier nun die wichtigsten Eigenschaften, die in diesem Kapitel vorgestellt
wurden. Sei dafuer $G = (V, E)$ ein Graph und $A$ dessen Adjazenzmatrix. Dann
sei noch $L$ die Laplace-Matrix von $L$ und letzlich $G' = (V', E')$ ein
weiterer Graph.

* Das Spektrum von $G$ ist $\{\lambda_1, ..., \lambda_n\}$.
* Das Spektrum ist eine Graphinvariante, die Adjazenzmatrix nicht.
* Ist $G$ isomorph zu $G'$, so sind ihre Spektren gleich.
* Ist $G$ isomorph zu $G'$, so sind ihre Laplace-Spektren gleich.
* Sind Spektrum oder Laplace-Spektrum oder beide Spektren zweier Graphen gleich,
  sagt noch nicht, dass $G$ isomorph zu $G'$ sein muss.
* $L$ hat den Grad der Knoten in der Diagonalen und $-A$ ueberall sonst.
* $L$ ist positiv semidefinit.
* Die Anzahl der Zusammenhangskomponenten von $G$ ist die Vielfachheit des
  Eigenwertes Null im Laplace-Spektrum.
