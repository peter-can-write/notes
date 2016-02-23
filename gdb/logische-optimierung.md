# Anfageoptimierung

Betrachten wir eine Anfrage wie:

```sql
select r1.A
from R1 r1, R2 r2
where r1.B = 'x';
```

Eine kanonische Uebersetzung dieser Anfrage in die relationale Algebra wuerde
ein Kreuzprodukt zwischen $R1$ und $R2$ beinhalten, gefolgt von einer Selektion
und letztlich einer Projektion. Man sieht aber schon leicht, dass es viel
effizienter waere, zuerst die Selektion zu machen, da sie sich nur auf die eine
Relation bezieht. Genau daher versucht ein DBMS Anfragen immer zu
optimieren. Diesen Vorgang nennen wir *Anfrageoptimierung*.

Bei der Anfrageoptimierung gibt es zwei Teilbereiche: die *logische* Optimierung
und die *physische* Optimierung. Die logische Optimierung behandelt, wie man
Ausdruecke der relationalen Algebra mittels bestimmten Aequivalenzumformungen
veraendern kann, sodass sich dadurch die Groesse von Zwischenergebnissen
reduziert. Die physische Optimierung behandelt dann die Optimierung der
Implementierung der relational-algebraischen Operationen wie Joins, Selektionen
sowie die eigentliche physikalische Dateninteraktion bzw. Interaktion mit dem
Hauptspeicher.

## Ablauf

Anfrageoptimierung hat mehrere Stufen:

1. Der Ausdruck ist noch eine "Deklarative Anfrage" (SQL):
   1. Scannen
   2. Parsen
   3. Sichtenaufloesung (echte temporaere Relationen anlegen)

2. Der Ausdruck ist nun ein algebraischer Ausdruck:
   1. Anfrageoptimierer

3. Auswertungsplan/Query Plan (QEP):
   1. Codeerzeugung
   2. Ausfuehrung

Zu 1.: "Zunaechst wird die Anfrage syntaktisch und semantisch analysiert und in
einen aequivalenten Ausdruck der relationalen Algebra umgewandelt. Bei diesem
Schritt werden auch die vorkommenden Sichten durch ihre definierende Anfage
ersetzt."

Zu 2/3.: "Mit der relationalen Algebra wird die Anfrageoptimierung
gestartet. Die Anfrageoptimierung sucht zu einem gegebenen algebraischen
Ausdruck eine effiziente Implementierung, einen sogenannten Auswertungsplan
(query evaluation plan, QEP). Dieser Auswertungsplan kann dann entweder
kompiliert oder interpretiert werden."

## Aequivalenzumformungen

1. Join $\bowtie$, Vereinigung $\cup$, Schnitt $\cap$ und Kreuzprodukt $\times$
   sind kommutativ:

   * $R_1 \botwie R_2 = R_2 \bowtie R_1$
   * $R_1 \cup R_2 = R_2 \cup R_1$
   * $R_1 \cap R_2 = R_2 \cap R_1$
   * $R_1 \times R_2 = R_2 \times R_1$

2. Join $\bowtie$, Vereinigung $\cup$, Schnitt $\cap$ und Kreuzprodukt $\times$
   sind assoziativ:

   * $(R_1 \bowtie R_2) \bowtie R_3 = R_1 \bowtie (R_2 \bowtie R_3)$
   * $(R_1 \cup R_2) \bowtie R_3 = R_1 \bowtie (R_2 \cup R_3)$
   * $(R_1 \cap R_2) \bowtie R_3 = R_1 \bowtie (R_2 \cap R_3)$
   * $(R_1 \times R_2) \bowtie R_3 = R_1 \bowtie (R_2 \times R_3)$

3. Selektionen sind untereinander vertauschbar (sozusagen "kommutativ"):

   $$\sigma_p(\sigma_q(R)) = \sigma_q(\sigma_p(R))$$

   Intuition: Zuerst nach Alter und dann nach Geschlecht filtern ist gleich wie
   zuerst nach Geschlecht und dann nach Alter zu filtern.

4. Konjunktionen $\land$ ine einer Selektion koennen in mehrere Selektion
   aufgebrochen werden ("kaskadieren"):

  $$\sigma_{p_1 \land p_2 \land ... \land p_n}(R) =
  \sigma_{p_1}(\sigma_{p_2}(...\sigma_{p_n}(R)...))$$

Dies ist wichtig, da man sie dann nach unten schieben ("propagieren") kann.

5. Falls fuer eine Reihe von Projektionen $l_1, ..., l_n$ gilt, dass eine feiner
   ist, als alle anderen, also $l_1 \subseteq l_2 \subseteq l_3 ... \subseteq
   l_n$, dann genuegt es, die feinste anzuwenden. Die groeberen Filter koennen
   also eliminiert werden. Seien $l_1, ..., l_n$ Attributmengen:

   $$\Pi_{l_1}(Pi_{l_2}(... (\Pi_{l_n}(R)))) = \Pi_{l_1}(R)$$

   Intuition: Wenn man zuerst das Alter, Gehalt und Geschlecht rausprojeziert, dann
   das Gehalt und das Alter, und schliesslich das Alter, dann genuegt es auch nur,
   das Alter zu projezieren. Wenn ein Filter feiner ist als alle anderen, dann
   reicht auch der. Beispiel: $$\Pi_{\text{Alter}}(\Pi_{\text{Alter,Gehalt})(R) =
   \Pi_{\text{Alter}(R)$$

6. Eine Selektion $\sigma$ kann mit einer Projektion $\Pi$ vertauscht werden,
   wenn die Selektion sich nur auf Attribute bezieht, die nach der Projektion
   noch vorhanden sind. Dies hat den Vorteil, dass die Menge von Attributen, die
   zwischengespeichert werden muss, kleiner ist.

   $$\Pi_l(\sigma_p(R)) = \sigma_p(Pi_l(R)$$

7. Eine Selektion kann mit einem Kreuzprodukt oder einem Join vertauscht werden,
   wenn sich die Selektion nur auf Attribute bezieht, die in einer der
   Relationen vorkommt. Dadurch koennen schon vorzeitig Tupel eliminiert werden.

   $$\sigma_p(R_1 \bowtie R_2) = \sigma_p(R_1) \bowtie R_2$$
   $$\sigma_p(R_1 \times R_2) = \sigma_p(R_1) \times R_2$$

8. Ebenso kann auch eine Selektion $p$ der Form $p_1 \land p_2$ mit einem
   Kreuzprodukt oder einem Join vertauscht werden, wenn sich das Praedikat $p_1$
   nur auf $R_1$ und das Praedikat $p_2$ nur auf $R_2$ bezieht:

	$$\sigma_p(R_1 \bowtie R_2) = \sigma_{p_1}(R_1) \bowtie \sigma_{p_2}(R_2),
    \text{ wo } p = p_1 \land p_2$$

9. Bezieht sich ein Join-Praedikat $p$ nur auf die Attribute in einer
   Projektionsliste $l = \{A_1, ..., A_n\} \cup \{B_1, ..., B_m\}$, dann darf
   man eine Projektion $Pi_l$ mit einem Join $\bowtie_p$ vertauschen:

   $$\Pi_l(R_1 \bowtie_p R_2) = \Pi_{A_1, ..., A_n}(R_1) \bowtie_p \Pi_{B_1,
   ..., B_m}(R_2)$$

   Bezieht sich das Join-Praedikat $p$ noch auf weitere Attribute $\{A'_1, ...,
   A'_n\}$ aus $R_1$ und $\{B'_1, ..., B'_n\}$ aus $R_2$, dann muss man zum
   Joinen diese Attribute noch mit-projezieren, und kann erst danach die
   vollstaendige Projektion $\Pi_l$ durchfuehren. Dennoch kann man somit die
   Anzahl an Attributen (also Daten) reduzieren:

   $$\Pi_l(R_1 \bowtie_p R_2) = \Pi_l(\Pi_{A_1, ..., A_n, A'_1, ..., A'_n}(R_1)
    \bowtie_p \Pi_{B_1, ..., B_m, B'_1, ..., B'_m}(R_2))$$

10. Da bei Mengenoperationen $\cup, \cap, \setminus$ das Schema der Relationen
    gleich sein muss, kann man Selektion mit diesen Operationen vertauschen::

    * $\sigma_p(R_1 \cup R_2) = \sigma_p(R_1) \cup \sigma_p(R_2)$
	* $\sigma_p(R_1 \cap R_2) = \sigma_p(R_1) \cap \sigma_p(R_2)$
	* $\sigma_p(R_1 \setminus R_2) = \sigma_p(R_1) \setminus \sigma_p(R_2)$

    Intuition: Ob man zuerst aus $R$ und $S$ alle Frauen selektiert und sie dann
    vereint ist das selbe wie $R$ und $S$ zuerst zu vereinen, und dann alle
    Frauen zu selektieren. Wobei man bei $\sigma_p(R \cup S)$ die volle
    Vereinigung im Speicher halten muss.

11. Eine Projektion kann mit einer Vereinigung $\cup$, nicht aber mit Schnitt
    $\cap$ oder Differenz $\setminus$ vertauscht werden:

    $\Pi_l(R_1 \cup R_2) = \Pi_l(R_1) \cup \Pi_l(R_2)$

    Fuer Schnitte und Differenzen ist dies nicht erlaubt, da diese Operationen
    zwei Tupel nur als gleich sehen, wenn alle ihre Attribute gleich sind. Haben
    zwei Tupel $t_1$ und $t_2$ beispielsweise alle Attribute in $l$ gleich aber
    alle in $R - l$, dann wurden sie mit Projektion geschnitten bzw. entfernt
    werden, ohne aber nicht.

12. Ein Kreuzprodukt $\times$ gefolgt von einer Selektion $\sigma_p$ wobei sich
    das Selektionspraedikat $p$ nur auf Attribute der beiden Relationen
    $R_{1,2}$ bezieht, kann in eine Joinoperation umgewandelt werden. Also wenn
    $p = A \phi B$ wobei $\phi$ eine beliebige Relation $\in R_1 \times R_2$ und
    $A$ ein Attribut aus $R_1$, $B$ ein Attribut aus $R_2$, dann gilt:

    $$\sigma_p(R_1 \times R_2) = R_1 \theta_p R_2$$

    __Wichtig!__ Hierbei koennen eine sehr grosse Zahl an Tupeln vermieden
    werden. Diese Regel wird vorallem bei typischen SQL Anfragen mit
    Kreuzprodukten und Selektionen (`select ... from A,B,C where P`) haeufig
    angewandt.

13. DeMorgan'sche Gesetze gelten natuerlich fuer Praedikate:

    * $\neg(\p_1 \land \p_2) = \neg \p_1 \lor \neg \p_2$
	* $\neg(p_1 \lor \p_2) = \neg p_1 \land \neg p_2$

14. Kein relationenalgebraisches Gesetz, aber trotzdem wichtig: Wenn moeglich
    und gueltig, darf man Projektionen einfuegen, um irrelvante Attribute aus
    Zwischenergebnissen zu eliminieren.

### Heuristiken

Was man immer tun sollte:

1. Selektionen aufbrechen, um sie nach unten schieben zu koennen.

2. Verschieben der Selektionen swoeit wiemoeglich nach unten im Operatorbaum
   (selection pushing).

3. Zusammenfassen von Selektionen und Kreuzprodukten zu Joins.

4. Bestimmung der Reihenfolge der Joins, um Zwischenergebnisse zu minimieren.

5. Wenn moeglich, Projektionen einfuegen.
