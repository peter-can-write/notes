# Relationale Entwurfstheorie

Die Relationale Entwurfstheorie behandelt das Verfeinern von relationalen
Schemata.

## Funktionale Abhaengigkeiten

In der relationalen Entwurfstheorie arbeiten wir stark mit dem Begriff
*funktionale Abhaengigkeit*. Sei $\mathcal{R}$ das Schema einer Relation, und
$\alpha \subseteq \mathcal{R}$ und $\beta \subseteq \mathcal{R}$ beide
Teilmengen der Relation. $\alpha$ und $\beta$ sind also beide eine Menge von
Attributen, die auch im Schema der Relation enthalten sind. Dann sagen wir
$\beta$ ist *funktional abhaengig* von $\alpha$, oder $\alpha$ *bestimmt*
$\beta$ *funktional* bzw. *eindeutig*, wenn fuer eine konkrete Auspraegung $R$
des Relationenschemas $\mathcal{R}$ gilt:

* Sind $r,t$ zwei Tupel aus $R$, dann muss stets sein, dass wenn $r.\alpha =
  t.\alpha$, die Attribute $\alpha$ dieser beiden Tupel also gleich sind, auch
  $r.\beta = t.\beta$ gilt. Das heisst intuitiv, dass es in der Relation pro
  konkreter, unterscheidbarer Menge von Attributwerten $\alpha$ auch nur eine
  einzige, eindeutige Menge von konkreten Attributwerten $\beta$ gibt. Denn
  $\alpha$ *bestimmt* $\beta$ innerhalb der Relation eben *eindeutig*.

* Sei $c$ eine konkrete Menge von Attributwerten fuer die Attributmenge
  $\alpha$. Dann gilt immer, dass $|\Pi_{\beta}(\sigma_{\alpha=c})| = 1$. Also
  wiederum, dass es fuer konkrete Werte $c$ der Attribute $\alpha$ nur eine
  eindeutige Menge von Attributen fuer $\beta$ gibt.

Ist nun $\beta$ also von $\alpha$ funktionale abhaengig, dann notieren wir das
mit $\alpha \rightarrow \beta$. Wir sagen dann auch, dass $\alpha$ Determinante
von $\beta$ ist. Ist $\alpha = \{A_1,...,A_n\}$ und $\beta={B_1,...,B_n}$, dann
schreiben wir oft auch anstatt $\{A_1,...,A_n\} \rightarrow \{B_1,...,B_n\}$
einfach: $A_1A_2...A_n \rightarrow B_1B_2...B_n$.

Aus obigen Definitionen folgen einige triviale Tautologien:

1. $\alpha \rightarrrow \beta \forall \beta \subseteq \alpha$: $\alpha$ bestimmt
   natuerlich immer sich selbst eindeutig (fuer *konkretes* $\alpha$), oder eine
   Untermenge von sich selbst.

2. $\alpha \rightarrow \emptyset$: $\alpha$ bestimmt nichts, eigentlich also
   eine nutzlose Aussage.

Das Konzept der funktionalen Abhaengigkeiten ist wohl am besten an einer realen
Situation illustriert. Gegeben sei diese Relation:

|Kind |Mutter|Vater |Adresse|
|:----|:-----|:-----|:------|
|Hansi|Helga |Holger| Weg 3 |
|Ferdi|Sarah |Moritz| Weg 9 |
| .   |  .   |  .   |   .   |
| .   |  .   |  .   |   .   |
| .   |  .   |  .   |   .   |

Die funktionalen Abhaengigkeiten dieser Relation waeren dann:

* Kind $\rightarrow$ Mutter, Vater
* Mutter $\rightarrow$ Vater
* Vater $\rightarrow$ Mutter
* Adresse $\rightarrow$ Mutter,Vater
* Kind $\rightarrow$ Adresse
* Mutter $\rightarrow$ Adresse
* Vater $\rightarrow$ Adresse

Konkret bedeutet beispielsweise Kind $\rightarrow$ Mutter,Vater, dass es in
dieser Relation fuer ein konkretes Kind immer nur eine eindeutige Mutter und
einen eindeutigen Vater gibt. Hingegen gilt weder Mutter $\rightarrow$ Kind noch
Vater $\rightarrow$ Kind, da es ja fuer konkrete Muetter bzw. Vaeter ja viele
Kinder geben kann. Wenn nun also z.B. eben ein Kind seinen/ihren Vater eindeutig
bestimmt bedeutet das, dass wir nie zwei Tupel haben duerfen, wo das Kind gleich
ist aber der konkrete Eintrag fuer das Attribut *Vater* verschieden ist -- das
Kind also zwei Vaeter haette.

### Schleussel

Funktionale Abhaengigkeiten koennen dazu verwendet werden, um Schluessel fuer
Relationen zu bestimmen. Sei also nun der Begriff *Schluessel* bezueglich einer
Relation eindeutig definiert und die verschiedenen Formen beschrieben:

Ein *Superschluessel* (super key) ist in einer Relation eine Menge von
Attributen, die ein Tupel innerhalb der Auspraegung der Relation eindeutig
identifiziert. Der trivialste Superschluessel ist die Relation selbst bzw. ihr
Schema. Wenn jedes Tupel nur einmal enthalten sein darf, dann wuerde das ganze
Tupel natuerlich gut zur Identifikation des Tupels selbst dienen.

Ein *Kandidatenschluessel* (candidate key) ist nun ein *minimaler
Superschluessel*. Die Minimalitaet ist hier nicht auf die Anzahl an Attributen
bezogen, sondern viel mehr auf ihre *Irreduzibilitaet*. Ist beispielsweise das
Attribut $A$ ein Superschluessel, dann ist auch $AB$ ein Superschluessel fuer
beliebiges Attribut $B$ der Relation. Aber da $AB$ noch zu einer Attributmenge
mit kleinerer Kardinalitaet reduziert werden kann, die auch noch Superschluessel
ist, hier eben zu $A$, ist $AB$ zwar Superschluessel, aber nicht
Kandidatenschluessel.

Jedoch kann es fuer eine Relation nun mehrere Kandidatenschluessel geben, welche
alle minimal sind und die Relation eindeutig identifizieren. Man moechte aber in
Praxis natuerlich nur einen dieser Schluessel verwenden muessen, um ein Tupel zu
finden. Also waehlen wir aus der Menge aller Kandidatenschluessel einen
beliebigen aus, der fuer die Relation zur Identifikation von Tupeln dienen
soll. Genau dieser Schluessel kann dann auch als Fremdschluessel in anderen
relationen eingesetzt werden. Dieser *ausgewaehlte* Kandidatenschluessel wird
dann *Primaerschluessel* (primary key) genannt.

### Huelle

Sei $F$ eine Menge von funktionalen Abhaengigkeiten, z.B. $\{A \rightarrow BC, C
\rightarrow D\}$ fuer $A,B,C,D \subseteq \mathcal{R}$ fuer ein Relationenschema
$\mathcal{R}$. Dann bezeichnet $F^+$ die *Huelle* (engl.: *closure*) von
$F$. Diese Huelle $F^+$ enthaelt dabei alle moeglichen, aus $F$ herleitbaren
funktionalen Abhaengigkeiten ueber der Menge von Attributen von
$F$.

Fuer das eben genannte Beispiel sieht man, dass $A$ $C$ bestimmt (und nebenbei
auch $B$). $C$ bestimmt dann weiter $D$. Das bedeutet in Prosa, dass es fuer
einen konkreten Attributwert von $A$ nur einen eindeutigen Attributwert von $C$
gibt, und fuer konkreten Attributwert von $C$ nur einen eindeutigen Attributwert
von $D$. Ist somit $A$ konkret gewaehlt, gibt es nur ein $C$ und daher nur ein
$A$. Also gilt immer dass es fuer konkretes $A$ nur ein eindeutiges konkretes
$D$ gibt. Daher muss auch $A \rightarrow D$ gelten.

Man sieht also, dass sich aus der urspruenglichen Menge von FD (functional
dependencies) noch eine groessere Menge herleiten laesst. Genau diese
anschauliche Argumentation wird in den Armstrong-Axiomen formal festgelegt,
wobei diese Axiome *Inferenzregeln* sind, also rein syntaktisch und nicht
semantisch arbeiten. Sei $\mathcal{R}$ hierbei dann wieder ein Schema und
$\alpha,\beta,\gamma,\delta \subseteq \mathcal{R}$. Dann kann man mittels den
folgenden drei Axiomen die vollstaendige Huelle $F^+$ von $F$ ermitteln:

1. Reflexivitaet: $\alpha \rightarrow \beta \forall \beta \subseteq \alpha$.

2. Verstaerkung: $\alpha \rightarrow \beta \Rightarrow \alpha\gamma \rightarrow
   \beta\gamma$. Bestimmt $\alpha$ also funktional $\beta$, dann kann man auf
   beiden Seiten noch eine beliebige Attributmenge $\gamma$ hinzufuegen.

3. Transitivitaet: $\alpha \rightarrow \beta \land \beta \rightarrow \gamma
   \Rightarrow \alpha \rightarrow \gamma$. Wie oben im Beispiel. Gibt es fuer
   konkretes $\alpha$ nur ein eindeutiges $\beta$, und fuer eindeutiges $\beta$
   nur ein eindeutiges $\gamma$, dann gilt also dass immer wenn $\alpha$ konkret
   gewaehlt wird auch $\gamma$ eindeutig ist.

Hierzu gibt es dann noch weitere Regeln, die aus obigen hergeleitet werden
koennen aber dennoch nuetzlich sind, um sie im Hinterkopf zu behalten:

1. Vereinigungsregel: $\alpha \rightarrow \beta \land \alpha \rightarrow \gamma
   \Rightarrow \alpha \rightarrow \beta\gamma$. Man kann also FDs mit selben
   linken Seiten zusammenfassen.

2. Dekompositionsregel: Invers zur Vereinigungsregel darf man man diese
   vereinigte FD dann wieder zurueck aufspalten: $\alpha \rightarrow \beta\gamma
   \Rightarrow \alpha \rightarrow \beta \land \alpha \rightarrow \gamma$.

3. Pseudo-Transitivaetsraegel: $\alpha \rightarrow \beta \land \beta\gamma
   \rightarrow \delta \Rightarrow \alpha\gamma \rightarrow \delta$. Wenn also
   nur die Attributmenge $\gamma$ zur Transitivitaet fehlt, dann darf man
   $\gamma$ noch zu $\alpha$ hinzufuegen. Das ist vor Allem nuetzlich, wenn man
   Schluessel von Relationen finden will.

4. Inoffiziell: $\alpha \rightarrow \beta \Rightarrow \alpha\gamma \rightarrow
   \beta$. Wenn $\alpha$ schon $\beta$ bestimmt, kann man beliebiges $\gamma$ zu
   $\alpha$ hinzufuegen und diese Eigenschaft nie zerstoeren. Diese Eigenschaft
   folgt aus der Verstaerkung von $\alpha \rightarrow \beta$ zu $\alpha\gamma
   \rightarrow \beta\gamma$, und dann aus der Dekomposition zu $\alpha\gamma
   \rightarrow \beta$ (bzw. dann auch $\alpha\gamma \rightaarrow \gamma$).

### Attributhuellen

Eben wurde erklaert, wie fuer eine Menge von funktionalen Abhaengigkeiten alle
moeglichen FDs fuer die *ganze Relation* gefunden werden kann. Wir sind aber
oftmals lediglich daran interessiert, welche Attribute man von einem bestimmten
Attribut $\alpha$ aus "erreichen", also bestimmen kann. D.h. wir wollen die
Frage beantworten, welche moeglichen $\beta \subseteq \mathcal{R}$ existieren,
sodass wir mit den Armstrong-Axiomen eine FD $\alpha \rightarrow \beta$
herleiten koennen. Dann ist $\beta$ naemlich von $\alpha$ abhaengig und wir
wuerden $\beta$ in die *Attributhuelle* von $\alpha$ geben. Diese Attributhuelle
notieren wir dann mit $\alpha^+$ aehnlich wie $F^+$ die Huelle von $F$ war.

Konkret bedeutet dass einfach, dass man von einer FD $\alpha \rightarrow \beta$
angefangen, einfach so lange Armstrong-Axiomen anwendet, bis wir $\alpha^+$
gefunden haben. Es sei hier aber angemerkt, dass die Verstaerkungsregel hier
nicht angewendet werden kann, da bei $\alpha\gamma \rightarrow \beta\gamma$
$\alpha\gamma$ ja nicht $\alpha$ ist.

Also sei das Resultat $Res$ zuerst die leere Menge, dann:

1. Fuege die reflexiv erreichbaren Attribute hinzu, also alle Teilmengen von
   $\alpha$: Somit $Res := Res \cup \{\beta\} \forall \beta \subseteq \alpha$.

2. Fuege alle Attribute hinzu, fuer welche es schon eine FD $\alpha
   \rightarrow \beta$ in $F$ gibt: Sei also $(\alpha \rightarrow \beta) \in F$,
   dann sei $Res := Res \cup \{\beta\}$.

3. Fuege alle transitiv erreichbaren Attribute zu $Res$ hinzu: Wenn also gilt
   $\exists \gamma \subseteq \mathcal{R}: \alpha \rightarrow
   \beta \land \beta \rightarrow \gamma$ dann $Res := Res \cup \{\gamma\}$.

Man kann die obigen Schritte sogar in einen schoenen Algorithmus packen. Sei
$\alpha$ eine Menge von Attributen. Gesucht ist $\alpha^+$. Wir bestimmen
hierfuer eine Menge $Res$ sodass $Res = \alpha^+$. Dann:

1. Initialisiere $Res$ als $Res := \alpha$. Fuege also zuerst alle Attribute in
   der Attributmenge $\alpha$ zum Ergebnis hinzu.

2. Dann, solange wie sich $Res$ nach der letzten Operation geandert hat (also
   solange bis die Schleife irgendeine Wirkung hatte):

   1. Siehe ob fuer jede FD $\beta \rightarrow \gamma$ gilt, dass $\beta \in
      \subseteq Res$, also ob $\beta$ schon als erreichbar von $\alpha$ bestimmt
      wurde. Wenn dem so ist, dann ist also auch ueber Transitivitat $\gamma$
      von $\alpha$ erreichbar.

  2. Fuege also $\gamma$ zum Erebnis hinzu: $Res := Res \cup \gamma$.

Haben wir erstmal die Attributhuelle $\alpha^+$ fuer ein Attribut $\alpha \in
\mathcal{R}$ bestimmt, dann koennen wir auch sehr einfach ueberprufefen, ob
$\alpha$ ein Superschluessel von $R$ ist. Hierzu muesste naemlich jedes Attribut
von $\mathcal{R}$, also die ganze Relation, *eindeutig* bestimmt sein. Das gilt
genau dann, wenn $\alpha^+ = \mathcal{R}$.

#### Beispiel

Sei $\mathcal{R} = \{A,B,C,D,E,F\}$ ein abstraktes Relationenschema mit den FDs:

* $A  \rightarrow BC$
* $C  \rightarrow DA$
* $E  \rightarrow ABC$
* $F  \rightarrow CD$
* $CD \rightarrow BEF$

Dann suchen wir nun die Attributhuelle von $A$, also alle von $A$ erreichbaren
Attribute.

1. Initial: $Res := \{A\}$

2. Dann fuer jede FD $\beta \rightarrow \gamma$, sehen wir ob $\beta$ schon in
   $Res$ enthalten ist:

   1. $A \rightarrow BC$: Ja, $A \subseteq \{A\}$,
      also $Res := Res \cup \{B,C\} = \{A,B,C\}$.

   2. $C \rightarrow DA$: Ja, $C \subseteq \{A,B,C\}$,
      also $Res := Res \cup \{D,A\} = \{A,B,C,D\}$.

   3. $E \rightarrow ABC$: Nein, $E \not\subseteq \{A,B,C,D\}$. Weiter.

   4. $F \rightarrow CD$: Nein, $F \not\subseteq \{A,B,C,D\}$. Weiter.

   5. $CD \rightarrow BEF$: Ja, $\{C,D\} \subseteq \{A,B,C,D\}$,
      also $Res := Res \cup \{B,E,F\} = \{A,B,C,D,E,F\}$

3. Fertig. Nun gilt: $A^+ := Res = \{A,B,C,D,E,F\}$.

Da auch gilt, dass $A^+ = \mathcal{R}$, ist $A$ also auch Superschluessel.

### Kanonische Ueberdeckung

Wie man nach dem letzten Beispiel sieht, kann eine Attributhuelle sehr schnell
aufblaehen. Fuer alle Attribute, also fuer $F^+$, ware die Maechtigkeit der
Huelle natuerlich noch viel groesser. Viele der FDs sind jedoch eigentlich
unnuetz oder sagen wenig aus, z.B. die reflexiven oder jene, die durch
Vereinigung entstehen. Auch wennn die Menge der FDs nicht gleich der ganzen
Huelle ist, kann sie viele solcher unnoetigen FDs besitzen.

Oftmals wollen wir daher in die andere Richtung. Wir wollen nicht die maximale
Menge an FDs von einer Ausgansmenge ermitteln, sondern eine minimale, die die
Eigenschaften der Relation gut zum Ausdruck bringen und "stellvertretend" fuer
alle weiteren, aus diesen herleitbaren FDs stehen. Diese *minimale* Menge an FDs
nennt man die *kanonische Ueberdeckung*. Da man gegebenenfalls aus zwei
verschiedenen Mengen $F,G$ die selbe Huelle herleiten kann, also sodass gilt $F
\neq G \land F^+ = G^+$ (in Zeichen $F \equiv G$), gibt es auch nicht immer eine
eindeutige kanonische Ueberdeckung.

Wir koennen die kanonische Ueberdeckung nun aber in vier Schritten
berechnen. Wir gehen dabei von einer urspruenglichen Menge von FDs $F$
aus. Dann:

1. Fuehren wir eine *Linksreduktion* durch. Dabei eliminieren wir alle
   unnoetigen Attribute aus der linken Seite jeder FD. Formal: Sei $\alpha
   \rightarrow \beta$. Dann pruefen wir, ob es ein $A \in \alpha$ gibt, sodass
   $\beta$ in der Attributhuelle von $\alpha - A$ ist, __bevor wir $A$
   rausnehmen__. Wir berechnen also die Attributhuelle von $\alpha - A$ nach
   obigem Algorithmus und nehmen dabei auch noch $\alpha \rightarrow \beta$ als
   ganzes unter Betracht (wir muessen hier $A$ also noch nicht aus $\alpha$
   rausnehmen, wir wollen ja nur sehen, ob momentan schon $\alpha - A$ eindeutig
   $\beta$ bestimmt). Wenn dem so ist, dann ist $A$ offensichtlich redundant und
   $\alpha - A$ genuegt. Wir nehmen also $\alpha \righarrow \beta$ aus der Menge
   der FDs also raus und fuegen $(\alpha - A) \rightarrow \beta$ hinzu.

2. Fuehren wir eine *Rechtsreduktion* durch. Hierbei wenden wir wieder die
   Armstrong-Axiome an, vorallem die Transitivitaets- und
   Dekompositionsregel. Haben wir also z.B. drei FDs $A \rightarrow B, B
   \rightarrow C, A \rightarrow CD$, dann sagen wir: $A$ bestimmt ueber $B$
   schon $C$, also duerfen wir $C$ aus $A \rightarrow CD$ eliminieren. Formal:
   Sei $\alpha \rightarrow \beta$ mit $B \in \beta$. Gilt dass $B$ noch immer in
   der Attributhuelle von $\alpha$ ist, nachdem wir $B$ aus $\beta$ entfernen,
   dann duerfen wir zum Zwecke der Minimalisierung $B$ auch entsprechend aus
   $\beta$ entfernen.

3. Entferne FDs der Form $\alpha \rightarrow \emptyset$. Diese sind
   moeglicherweise in Schritt (2) entstanden.

4. Fasse FDs mit selber linker Seite zusammen. Existieren also FDs $\alpha
   \rightarrow \beta, \alpha \rightarrow \gamma$, dann ersetzen wir diese beiden
   FDs durch die neue FD $\alpha \rightarrow \beta\gamma$.

#### Beispiel

Sei $\mathcal{R} = \{A,B,C,D,E,F\}$ wieder dasselbe abstrakte Relationenschema
mit den FDs:

1. $A  \rightarrow BC$
2. $C  \rightarrow DA$
3. $E  \rightarrow ABC$
4. $F  \rightarrow CD$
5. $CD \rightarrow BEF$

Dann suchen wir nun die kanonische Ueberdeckung von der Menge $F$ der FDs:

1. Linksreduktion:

	1. Wir ueberpruefen, ob $BEF$ schon in der Attributhuelle von $D$ enthalten
       ist, sodass wir $C$ entfernen koennten. Da $D$ in keiner weiteren FD
       links ist, kann das nicht sein.

	2. Wir ueberpruefen, ob $BEF$ schon in der Attributhuelle von $C$ enthalten
       ist, sodass wir $D$ entfernen koennten. $C$ ist links in einer weiteren
       FD, also koennte das sein. Wir bilden die Attributhuelle von $C$:

	   1. $\{C\}$
	   2. $C \rightarrow DA$: $\{A,C,D\}$
	   3. $CD \rightarrow BEF$: $\{A,C,D,B,E,F\}$ (da $CD$ schon drin war)

	   Also ja, $BEF$ ist auch ohne $D$ durch $C$ eindeutig bestimmt. Wir
       eliminieren also $D$ aus der FD $CD \rightarrow BEF$.

	Neue FDs:

	1. $A  \rightarrow BC$
	2. $C  \rightarrow DA$
	3. $E  \rightarrow ABC$
	4. $F  \rightarrow CD$
	5. $C \rightarrow BEF$

2. Rechtsreduktion:

	1. Wir pruefen: Ist $B$ noch in der Attributhuelle von $A$, wenn wir es aus
       aus $A \rightarrow BC$ entfernen? Ja, denn $A \rightarrow C \rightarrow
       B$.

	2. Wir pruefen: Ist $C$ noch in der Attributhuelle von $A$, wenn wir es aus
       $A \rightarrow C$ entfernen? Da $A$ nie mehr links ist, kann das nicht
       sein.

	3. $D$ in (2)? Ja, da $C \rightarrow F \rightarrow D$.

	4. $A$ in (2)? Ja, da $C \rightarrow E \rightarrow A$.

	5. $A$ in (3)? Nein, da $A$ nie mehr rechts.

	6. $B$ in (3)? Ja, da $E \rightarrow C \rightarrow B$.

	7. $C$ in (3)? Ja, da $E \rightarrow A \rightarrow C$.

	8. $C$ in (4)? Nein, da $D$ nie mehr links (keine Transitivitaet moeglich).

	9. $D$ in (4)? Nein, da $D$ nie mehr rechts.

	10. $BEF$ in (5)? Nein, keines mehr rechts.

Resultat:

1. $A \rightarrow C$
2. $C \rightarrow \emptyset$
3. $E \rightarrow A$
4. $F \rightarrow CD$
5. $C \rightarrow BEF$

Da (2) rechts die leere Menge hat, eliminieren wir (2). Da keine weiteren FDs
selbe linken Seiten haben, ist keine weitere Zusammenfassung moeglich.

Endergebnis:

1. $A \rightarrow C$
2. $E \rightarrow A$
3. $F \rightarrow CD$
4. $C \rightarrow BEF$

## Normalformen

Schlechte Relationenschemata koennen eine Vielzahl von Problemen mit sich
bringen. Allgemein sprechen wir dabei von *Anomalien*. Hiervon gibt es dann
*Updateanomalien*, *Einfuegeanomalien* sowie *Loeschanomalien*. Nehmen wir
beispielsweise das schlechte Schema $R: \{[A, 7]\}$. Dieses Schema hat ein
Attribut $A$, und dann in der zweiten Spalte einfach die Konstante $7$
stehen. Bei einer solchen Relation gaebe es eine Updateanomalie. Moechte man
naemlich in einer Auspraegung dieser Tabelle mit $10^{100}$ Zeilen die Konstante
$7$ doch zu $11$ aendern, so muss man alle $10^{100}$ Werte aendern.

Um solche unvorteilhaften Situationen zu vermeiden, helfen uns
*Normalformen*. Normalformen sind bestimmte Kriterien, die eine Relation und
ihre FDs erfuellen muessen. Es gibt hierbei verschieden strenge Normalformen,
und jede Normalform baut auf den vorherigen auf. Eine Relation in 3. Normalform
ist also auch implizit in 1. und 2. Normalform. Insgesamt behaneln wir fuenf
Normalformen:

1. Die 1. Normalform (1NF).
1. Die 2. Normalform (2NF).
3. Die 3. Normalform (3NF).
4. Die Boyce-Codd Normalform (BCNF).
5. Die 4. Normalform (4NF).

### Zerlegungen

Ist eine Relation in einer fuer uns zu niedrigen, oder in gar keiner,
Normalform, so behelfen wir uns, indem wir die Relation in kleinere
*zerlegen*. Ist $R$ also eine Relation, dann zerlegen wir sie in
kleinere Relationen $R_1,...,R_n$ mit dem Ziel, dass diese
Teilrelationen dann in der gewuenschten Normalform sind. Bei allen
Transformationen einer Relation von einer (oder keiner) Normalform in eine
andere wollen wir zwei Eigenschaften der Transformation im Auge behalten:

1. Verlustlosigkeit.
2. Abhaengigkeitserhaltung.

#### Verlustlosigkeit

Wir nennen eine Zerlegung einer Relation $R$ in kleinere Relationen
$R,...,R_n$ verlustlos, wenn alle *Informationen* der urspruenglichen Relation
$R$ aus den Teilrelationen $R_1,...,R_n$ *rekonstruierbar* sind.

Formal bedeutet das, dass die uerspruengliche Relation $R$ durch sukzessive
natuerliche Verbuende aus den Teilrelationen wiederhergestellt werden kann: $$R
= \bowtie_{i=1}^n R_i$$. Somit ist auch schon klar, dass zwischen den einzelnen
Teilrelationen auch noch irgendeine Art von Verbindung gegeben sein muss, da es
ja ein Joinattribut geben muss. Es gibt also eine Menge von Attributen $\alpha
\subseteq \mathcal{R}$, sodass $\forall R_i: \alpha \in \mathcal{R_i}$.

Eine hinreichende aber nicht notwendige Bedingung ($\Rightarrow$, nicht
$\Leftrightarrow$) fuer eine verlustlose Zerlegung ist nun, dass fuer je zwei
Teilrelationen gilt, dass in zumindest einer Relation die gemeinsame
Attributmenge Superschluessel ist. Sei $\mathcal{R}$ also zerlegt in
$\mathcal{R}_1,\mathcal{R}_2$, dann muss gelten:

* $(\mathcal{R}_1 \cap \mathcal{R}_2) \rightarrow \mathcal{R}_1 \in
  F_{\mathcal{R}}^+$, oder

* $(\mathcal{R}_1 \cap \mathcal{R}_2) \rightarrow \mathcal{R}_2 \in
  F_{\mathcal{R}}^+$

Das bedeutet also wie gesagt, dass die gemeinsame Attributmenge $\mathcal{R}_1
\cap \mathcal{R}_2$ entweder $\mathcal{R}_1$ oder $\mathcal{R}_2$ bestimmt. Das
geht natuerlich nur, wenn diese funktionale Abhaengigkeit zwischen der
gemeinsamen Attributmenge und den restlichen Attributen aus der ersten oder
zweiten Teilrelation auch schon vorher gueltig war bzw. aus den gegebenen FDs
herleitbar war. Dies wird durch das $\in F_{\mathcal{R}}^+$ ausgedrueckt.

Der Sinn davon sei abstrakt an Beispielen illustriert.

1. Wir nehmen zuerst an, es gelten beide obige Bedingungen (eine wuerde auch
   genuegen). Beispielsweise in der Relation $\mathcal{R}:
   \{[\underline{A},B,C]\}$, wobei also gilt $A \rightarrow BC$ und somit $A
   \rightarrow B, A \rightarrow C$. Dann zerlegen wir diese Relation in zwei
   Teilrelationen $\mathcal{R}_1: \{[\underline{A}, B]\}, \mathcal{R}_2:
   \{[\underline{A}, C]\}$. In beiden Relationen gilt, dass das gemeinsame
   Attribut $A$ Superschluessel ist. Wir sehen, dass wenn wir diese Relationen
   joinen, auch wieder genau jedes vorherige Tupel rekonstruiert wuerde. Diese
   Zerlegun waere also verlustlos.

2. Wir nehmen dann an, nur eine der Bedingungen gilt, die ander aber nicht. Sei
   also $\mathcal{R}: \{[\underline{A}, B, C]\}$ mit zusaetzlicher FD (neben $A
   \rightarrow BC$) $B \rightarrow C$ und $\mathcal{R}_1:
   \{[\underline{A}, B]\}$ und $\mathcal{R}_2: \{[\underline{B}, C]\}$. Hier ist
   das gemeinsame Attribut also $B$. Nur in $R_2$ gilt, dass $B$ Superschluessel
   ist. Wuerde man $\mathcal{R}_1$ mit $\mathcal{R}_2$ nun joinen, waere auch
   kein Verlust geschehen: Fuer ein $A$ gibt es nur ein $B$, und dieses wird an
   das eindeutige $B$ in $\mathcal{R}_2$ gejoined.

3. Nun also der schlimme Fall, wo keine der Bedinungen gilt. Sei dafuer
   $\mathal{R}: \{[\underline{A,B},C]\}$ und $\mathcal{R}_1:
   \{[\underline{A,B}]\}, \mathcal{R}_2: \{[\underline{B,C}]\}$. Hier ist das
   gemeinsame Attribut wieder $B$, aber es ist nirgends Superschluessel. Was
   hier also sein koennte, ist dass man in $R_2$ fuer ein $B$ zwei verschiedene
   $C$ Werte einfuegen koennte. Sagen wir dann, es gibt ein Paar $[A,B] \in
   R_1$, wo das $B$ dieses mit den zwei $C$ ist. Joinen wir dann $R_2$ mit
   $R_1$, gibt es zwei Tupel $[A,B,C_1], [A,B,C_2]$. Die FD $AB \rightarrow C$
   waere also verletzt und die Information, dass es fuer konkrete $A,B$ nur ein
   $C$ gibt, geht verloren.

#### Abhaengigkeitserhaltung

Das Kriterium der Abhaengigkeitserhaltung bezieht sich auf die funktionalen
Abhaengikeiten der Relation. Sie besagt, dass wenn man ein Relationenschema
$\mathcal{R}$ in viele Teilschemata $\mathcal{R}_1,...,\mathcal{R}_n$ zerlegt,
die funktionalen Abhaengikeiten $F_{\mathcal{R}}$ vollstaendig und verlustlos
ueber den Teilschemata verteilt wird. Es soll also gelten: $$F_{\mathcal{R}} =
\bigcup_i^{n} F_{\mathcal{R}_i}$$. Die Bedingung ist also gar nicht schwer.

Nehmen wir ein Gegenbeispiel. Sei $\mathcal{R}: \{[\underline{A}, B, C]\}$ mit
den FDs $F_{\mathcal{R}} = \{A \rightarrow BC\}$. Wir zerlegen $\mathcal{R}$ in
zwei Schemata $\mathcal{R}_1: \{[\underline{B, C}]\}$ und $\mathcal{R}_2:
\{[\underline{A}, C]\}$. Nun sind die nicht-trivialen FDs der neuen
Teilschemata: $F_{\mathcal{R}_1} = \emptyset$ und $F_{\mathcal{R}_2}: \{A
\rightarrow B\}$. Eindeutig ist $F_{\mathcal{R}_1} \cup F_{\mathcal{R}_2} \neq
F_{\mathcal{R}}$. Dies waere also eine verlustlose, aber nicht
abhaengigkeitserhaltende Zerlegung.

### 1. NF

Die erste Normalform besagt, dass alle Attribute des Schemas der Relation
*atomar* sein muessen. Atomar bedeutet dabei einfach, dass sie keine weitere
Struktur in Form von Mengen oder Relationen aufweisen, sondern einfache Werte
sind. Relationen in erster Normalform duerfen also keine geschachtelten
Relationen enthalten. Ein Tupel $[1, a, \{x, y, z\}]$ wuerde also auf Grund der
nicht atomaren Menge $\{x, y, z\}$ auf eine Verletzung der ersten Normalform
hindeuten.

### 2. NF

Die zweite Normalform kuemmert sich schon um etwas tiefliegendere
architektonische Angelegenheiten. Genauer gilt fuer eine Relation in zweiter
Normalform, dass sie nur ein einziges *Konzept* modelliert. Sie verhindert also,
dass man logisch nicht verbundene Daten in die selbe Relation gibt.

Soviel also zum Zweck. Konkret sagt die zweite Normalform nun aus, dass __jedes
Nichtschluesselattribut voll funktional abhaengig von jedem
*Kandidatenschluessel*__ ist. Was bedeutet aber *volle* funktionale
Abhaengigkeit? Wir sagen, dass $\beta$ *voll* funktional abhaengig von $\alpha$
ist, wenn es keine Teilmenge von $\alpha$ gibt, die $\beta$ schon alleine
bestimmt. Fuer die zweite Normalform muss also gelten, dass jedes
Nichtschluesselattribut von jedem __Kandidatenschluessel__ (nicht
Superschluessel!) als ganzes abhaengig ist, und nicht nur von einem Teil.

Sei $\mathcal{R}: \{[A, B, C]\}$ eine Relation. Dann ist $R$ nicht in 2NF, wenn
es beispielsweise folgende FDs gibt: $AB \rightarrow C, B \rightarrow C$. Die
Kandidatenschluessel dieser Relation sind hier nur $AB$. Es muesste also sein,
dass jedes Nichtschluesselattribut, hier nur $C$, von $AB$ *voll* funktional
abhaengig ist. Aber es gilt wie man sieht, dass $C$ schon von $B$, also einem
*Teil* eines (des) Kandidatenschluessels, abhaengig ist. Somit ist die 2. NF
verletzt.

Wieso ist diese Situation nun aber schlimm? Da $A$ eigentlich keinerlei Einfluss
auf eines der andern Attribute zu haben scheint, ist es in dieser Relation
eigentlich vollkommen redundant. Augenscheinlich hat $A$ ueberhaupt nichts mit
$B$ oder $C$ zu tun. Wir brauchen es auch nur im Kandidatenschluessel, weil $A$
sonst nicht eindeutig identifizierbar waere. Was wir also tun sollten ist, $B$
mit $C$ in eine eigene Tabelle zu geben und $A$ separat (alleine) speichern.

### 3. NF

Das Ziel der dritten Normalform ist die Bereinigung der Relation von transitiven
Abhaengigkeiten von Schluesselattributen. In einer Relation in zweiter
Normalform kann es noch sein, dass wir die FDs $A \rightarrow B, B \rightarrow
C$ haben. $C$ ist *voll* funktional abhaengig vom Schluessel $A$, und $B$
sowieso, also ist diese Relation in zweiter Normalform. Die dritte Normalform
wird nun die transitive Abhaengikeit zwischen dem Schluessel $A$ und dem
Nichtschluesselattribut $B$ adressieren. Dies fuehrt wiederum zu einer
"strikteren" Definition des Konzepts einer Tabelle.

Konkret sagt die dirtte Normalform nun, dass fuer jede funktionale Abhaengigkeit
$\alpha \rightarrow \beta$ eines von diesen gelten muss:

1. Die FD ist trivial: $\beta \subseteq \alpha$

2. $\alpha$ ist *Superschluessel* (Achtung, nicht Kandidatenschluessel)

3. $\beta$ ist *prim*.

Hierbei bedeutet *prim*, dass $\beta$ Teilmenge eines Kandidatenschluessels
ist. Ist $AB$ beispielsweise ein Kandidatenschluessel, dann wuerde man alle
Attributmengen in $\{AB,A,B\}$ prim nennen.

Nehmen wir die Beispielrelation $\mathcal{R}:
\{[\underline{A, B}, C, D]\}$. Sind die FDs dann (neben $AB \rightarrow BC$):

1. $B \rightarrow C$, so ist diese Relation nicht in dritter Normalform, weil
   $C$ Nichtschluesselattribut ist, aber $B$ nicht Superschluessel ist und $C$
   auch nicht prim.

2. $A \rightarrow B$, so ist die Relation in dritter Normalform. $A$ ist zwar
   (alleine) nicht Superschluessel, aber $B$ ist im Kandidatenschluessel
   enthalten also prim.

#### Synthesealgorithmus

Der Synthesealgorithmus ist nun ein Algorithmus, der dazu dient, eine beliebige
Relation $R$ in die *3. Normalform* zu bringen (wenn sie nicht schon in einer
hoeheren ist). Der schwierigste Schritt dabei wurde schon weiter oben behandelt:
das Bilden der kanonischen Ueberdeckung. Hier nun aber der genau Algorithmus:

Sei $\mathcal{R}$ das Schema einer Relation und $F$ die Menge der FDs der
Relation, wobei es zumindest eine FD darin gibt, die die dritte Normalform
verletzt (sonst terminiere).

1. Bilde die kanonische Ueberdeckung $F_c$ von $F$, also:

	1. Bilde die Linksreduktion.
	2. Bilde die Rechtsreduktion.
	3. Eliminiere FDs der Form $\alpha \rightarrow \emptyset$.
	4. Fasse FDs mit selber linker Seite zusammen.

2. Bilde nun fuer jede in $F_c$ verbleibende FD $fd_i: \alpha \rightarrow \beta$
   eine neue Relation $R_i := \alpha \cup \beta$. In jeder dieser neuen Relation
   ist dann immer $\alpha$ Kandidatenschluessel der jeweiligen
   Relation.

3. Pruefe, ob einer der Kandidatenschluessel dieser neu kreeirten Relationen
   auch Kandidatenschluessel der urspruenglichen Relation ist. Dann kommen wir
   naemlich von jeder Relation eindeutig zur naechsten (Verlustlosigkeit also
   garantiert; immer nur ein Tupel).

   Falls nicht, so fuege noch eine zusaetzliche Relation hinzu, die den
   Kandidatenschluessel der Relation (also der urspruenglichen, $R$) beeinhaltet
   (also als Fremdschluessel).

4. Pruefe letztlich, ob in Schritt (3) zwei Relationen $R_1,R_2$ entstanden
   sind, sodass $\mathcal{R}_1 \subseteq \mathcal{R}_2$, das Schema von $R_1$
   also vollstaendig in dem von $R_2$ enthalten ist. Wenn ja, dann verwerfe
   $R_1$.

Somit erhaelt man also eine Zerlegung von $\mathcal{R}$ in Teilrelationen
$\mathcal{R}_1,...,\mathcal{R}_n$, welche alle in dritter Normalform sind. Fuer
diesen Algorithmus gilt dann auch noch, dass er:

1. verlustlos,
2. abhaengigkeitserhaltend

ist.

### BCNF

Die Boyce-Codd Normalform (BCNF) ist nun eine kleine Steigerung der dritten
Normalform, die die Ausrede fuer prime Nichtschluesselattribute entfernt, wo
$\alpha$ nicht Superschluessel ist. Fuer jede FD $\alpha \rightarrow \beta$ muss
also nun wirklich gelten, dass die FD trivial ist, oder, wenn nicht, $\alpha$
__immer__ *Superschluessel* ist.

#### Dekompositionsalgorithmus

Fuer die BCNF gibt es nun einen eigenen Algorithmus, der sich
*Dekompositionsalgorithmus* nennt. Er hat die Eigenschaft, eine Relation
*verlustlos* aber __nicht__ abhaengigkeitserhaltend in die BCNF zu
ueberfuehren.

Wir wollen also die Relation $R$ mit Schema $\mathcal{R}$ und funktionalen
Abhaengigkeiten $F_{\mathcal{R}}$ in die BCNF ueberfuehren.

1. Starte mit $Z = \{\mathcal{R}\}$.

2. Solange es dann noch eine Relation $R_i$ in $Z$ gibt, die nicht in BCNF ist:

	1. Finde eine fuer $R_i$ geltende FD $\alpha \rightarorow \beta$ die die
       BCNF verletzt, sodass:

		* Sie nicht trivial ist: $\beta \not\subseteq \alpha$

		* $\alpha$ nicht Superschluessel ist: $\alpha \not\rightarrow \mathcal{R}$

	2. Zerlege $R_i$ in *zwei* neue Relationen:

		1. $R_{i_1} := \alpha \cup \beta$.
		   Somit ist in $R_{i_1}$ $\alpha$ immer Superschluessel.

        2. $R_{i_2} = R_i - \beta$.
		   Alle anderen Attribute, damit der Algorithmus mit diesen fortfahren
           kann (wenn sie nicht schon passen).

	3. Entferne $R_i$ aus $Z$ und fuege dafuer $R_{i_1}$ und $R_{i_2}$ ein.

	4. Zurueck zu (2).

Hierbei sollte man in 2.1 moeglichst eine FD waehlen, wobei $\beta$ alle von
$\alpha$ abhaengigen Attribute sind, damit der Algorithmus schneller terminiert.

##### Beispiel

Sei $\mathcal{R} = \{[A, B, C, D]\}$ mit den FDs:

* $AB \rightarrow CD$
* $DA \rightarrow B$

Wir wollen diese Relation in die BCNF Normalform bringen. Dafuer pruefen wir
zunaechst, in welcher Normalform die Relation ueberhaupt ist. Hierfuer brauchen
wir zunaechst die Kandidatenschluessel. Hier gibt es die (nicht trivialen)
Kandidatenschluessel $AB$ und $ACD$. Wir pruefen nun, die Eigenschaften der FDs:

1. $AB \rightarrow CD$: $AB$ ist Kandidatenschluessel, also sicher OK.

2. $AD \rightarrow B$: Hier ist $AD$ nicht Superschluessel, aber $B$ ist prim.

Diese Relation ist also in dritter Normalform, noch nicht in BCNF. Wir
koennen also den Dekompositionsalgorithmus anwenden, um sie in BCNF zu
ueberfuehren.

1. Starte also mit $Z = \{R\}$.

2. Da $R$ noch nicht in BCNF:

	1. Waehle eine FD, die die BCNF verletzt: $AD \rightarrow B$.

	2. Zerlege sie in zwei Relationen:

		1. $\mathcal{R_1} = AD \cup B = ABD$ (laxe Mengenschreibweise)
		2. $\mathcal{R_2} = \mathcal{R} - B = ABC$

	3. Ersetze $\mathcal{R}$ in $Z$ durch gewonnene $\mathcal{R_1,R_2}$: $Z :=
       \{\mathcal{R_1,R_2}\} = \{\{A,B,D\},\{A,B,C\}\}$.

	4. Wir pruefen, ob $R_1,R_2$ in BCNF sind:

		1. $\mathcal{R_1} = \{A,B,D\}$ mit FDs $AB \rightarrow D$ (durch
           Dekompositionsregel) und $AD \rightarrow B$. Hier sind $AB,AD$
           Superschluessel also sind beide FDs in Ordndung. $R_1$ ist also in
           BCNF.

		2. $\mathcal{R_2} = \{A,B,C\}$ mit FDs $AB \rightarrow C$ (durch
           Dekompositionsregel). $AB$ ist auch hier Superschluessel und daraus
           folgt, dass auch $R_2$ in BCNF ist.

	5. Da alle $R_i \in Z$ in BCNF sind, terminieren wir.

Wir haben also hier aus $R$ zwei Relationen $R_1 = \{A,B,D\}, R_2 = \{A,B,C\}$
gewonnen, die in BCNF sind. In diesem Fall ist die Zerlegung sogar nicht nur
verlustlos (garantiert), sondern auch abhaengigkeitserhaltend (nicht
garantiert).

## Mehrwertige Abhaengigkeiten

Mehrwertige Abhaengigkeiten (engl.: *multivalued dependencies*, *MVD*) sind eine
Verallgemeinerung funktionaler Abhaengigkeiten. Das bedeutet erstmal, dass jede
FD auch eine MVD ist, aber nicht umgekehrt. Intuitiv vermittelt nun eine
*mehrwertige Abhaengigkeit* zwischen zwei Attributmengen $\alpha,\beta \subseteq
\mathcal{R}$, dass $\beta$ zwar abhaengig von $\alpha$ ist, aber unabhaengig von
allen weiteren Attributen $\gamma$ die mit $\alpha$ in einem Tupel sein
koennen. Das notieren wir dann mit $\alpha \twoheadrightarrow \beta$.

Wenn es also einmal ein $\beta$ gibt, dass mit $\alpha$ in einem Tupel ist, dann
muss es fuer alle moeglichen $\gamma$, die mit $\alpha$ irgendwo in einem Tupel
sind, auch das Tupel $[\alpha, \beta, \gamma]$ geben. Das $\beta$ ist also
unabhaengig von den anderen Attributen $\gamma$. Fuer alle Tupel
$[\alpha, \beta, \gamma]$ kann man die $\beta$ vertauschen, diese Tupel muss es
dann auch geben. Haben wir also die Tupel $[\alpha, \beta_1, \gamma_1]$ sowie
$[\alpha, \beta_2, \gamma_2]$, dann *impliziert* das, dass es auch die Tupel
$[\alpha, \beta_2, \gamma_1]$ und $[\alpha, \beta_1, \gamma_2]$ gibt. Da die
$\beta$ ja unabhaengig $\gamma_i$ sind, kann man $\beta_1,\beta_2$ vertauschen
und es muss die Tupel dann auch geben. Wenn $\gamma$ leer ist, gilt die Aussage
sowieso, da man dann gar nicht vertauschen kann.

Wir wollen ein Beispiel nehmen. Man stelle sich dafuer eine Autofirma vor, die
eine Relation $\text{Autos}: \{[\text{Modell}, \text{Farbe}, \text{Jahr}]\}$
verwaltet. Sagen wir nun, dass es ein Modell jeweils in zwei Farben gibt, die
jedes Jahr gleich bleiben. Beispielsweise gibt es $\text{Golf}$ immer in Rot und
Blau, $\text{Schiebkarre}$ immer in $\text{Grau1}$ und $\text{Grau2}$. Daher
sind die Farben also abhaengig von dem Auto, aber unabhaengig von dem
Jahr. Daraus resultiert nun, dass wenn es einmal ein Tupel
$[\text{Golf}, \text{Rot}, \text{1999}]$ gibt, es auch *implizit* immer ein
Tupel $[\text{Golf}, \text{Blau}, \text{1999}]$ gibt. Bzw. nach der Idee des
"Vertauschens" von oben gilt, dass man die Farben fuer die Autos immer
vertauschen kann und es die Tupel dann auch geben muss. Wir haben hier also die
mehrwertige Abhaengigkeit $\text{Modell} \rightarrow \text{Farbe}$.

Augenscheinlich fuehren mehrwertige Abhaengigkeiten zu Redundanz und
Anomalien. Meistens modellieren sie nicht nur ein Konzept. Oben muesste es
natuerlich die Relationen $\text{Farben}: \{[\text{Modell}, \text{Farbe}]\}$ und
$\text{Jahre}: \{[\text{Modell}, \text{Jahr}]\}$ separat geben.

Es gibt nun wie fuer FDs wieder eine Reihe von Inferenzregeln zur Herleitung
aller fuer eine Relation geltenden mehrwertigen Abhaengigkeiten. Es sei hier
nochmals angemerkt, dass jede FD auch eine MVD ist. Daher gelten die Regeln:

1. *Reflexivitaet*: $\beta \subseteq \alpha \Rightarrow \alpha \rightarrow
   \beta$

2. *Verstaerkung*: $\alpha \rightarrow \beta \Rightarrow \alpha\gamma
   \rightarrow \beta\gamma$.

3. *Transitivitaet*: $\alpha \rightarrow \beta \land \beta \rightarrow \gamma
   \Rightarrow \alpha \rightarrow \gamma$.

4. *Vereinigung*: $\alpha \rightarrow \beta \land \alpha \rightarrow \gamma
   \Rightarrow \alpha \rightarrow \beta\gamma$.

Da jede FD eine MVD ist kann man fuer obige Regeln immer $\rightarrow$ durch
$\twoheadrightarrow$ ersetzen, sodass man die *mehrwertige Reflexivitaet*,
*mehrwertige Verstaerkung* usw. erhaelt. Es sei aber angemerkt, dass eine Regel
fuer mehrwertige Abhaengigkeiten __nicht__ gilt: es gibt keine mehrwertige
*Dekomposition*, $\alpha \rightarrow \beta\gamma \Rightarrow \alpha \rightarrow
\beta \land \alpha \rightarrow \gamma$ gilt also __nicht__!

Weiters gelten dann:

1. __Komplement__: $\alpha \twoheadrightarrow \beta \Rightarrow \alpha
   \twoheadrightarrow \mathcal{R} - (\alpha \cup \beta)$.

2. Schnittmenge: $\alpha \twoheadrightarrow \beta \land \alpha
   \twoheadrightarrow \gamma \Rightarrow \alpha \twoheadrightarrow (\beta \cap
   \gamma)$

3. Differenz: $\alpha \twoheadrightarrow \beta \land \alpha \twoheadrightarrow
   \gamma \Rightarrow \alpha \twoheadrightarrow (\beta - \gamma)$

Sowie die Regel die FDs mit MVDs vebindet: $$\text{Verallgemeinerung}: \alpha
\rightarrow \beta \Rightarrow \alpha \twoheadrightarrow \beta$$ Man denke sich
dazu: Wenn $\alpha \rigtharrow \beta$, sodass es fuer ein $\alpha$ also nur ein
eindeutiges $\beta$ gibt, dann ist es trivial dass $\alpha \twoheadrightarrow
\beta$, weil es nie ein weiteres $\beta$ fuer $\alpha$ geben wird, sodass man
ein neues Tupel fuer die MVD ergaenzen muesste. Man denke auch ans Vertauschen:
Wenn es fuer $\alpha$ nur ein $\beta$ gibt dann erhaelt man durch Vertauschen
von $\beta$ fuer jedes $\gamma$ eben die Tupel die schon enthalten sind.

Man beachte aber doch, dass zwar jede FD eine MVD ist, aber nicht jede MVD eine
FD. Das bedeutet *insbesondere*, dass __MVDs nicht zur Schluesselbestimmung
beitragen!!__

### 4. NF

Es gibt nun anschliessend an die Diskussion der MVDs auch eine weitere
Normalform, die strenger als die BCNF ist (sie also einschliesst), und sich auf
MVDs bezieht. Fuer jede MVD $\alpha \twoheadrightarrow \beta$ muss nun gelten:

1. Entweder die MVD ist trivial, oder
2. $\alpha$ ist superschluessel.

Man beachte aber, dass eine triviale MVD nicht genau gleich definiert ist wie
eine triviale FD. Eine MVD $\alpha \rightarrow \beta$ ist nun genau dann
trivial, wenn eines hier gilt:

1. $\beta \subseteq \alpha$ (Reflexivitaet, gleich wie bei FDs).

2. $\beta = \mathcal{R} - \alpha$.

Fuer (2) ueberlege man sich, das eine der Definition von MVDs war: Gilt $\alpha
\rightarrow \beta$, dann muss fuer jedes $\gamma$ das mit $\alpha$ einmal in
einem Tupel ist, es auch ein Tupel $[\alpha, \beta, \gamma]$ geben. Wenn jetzt
nun aber $\beta = \mathcal{R} - \alpha$, dann gilt doch, dass $\alpha \cup \beta
= \mathcal{R}$ und somit insbesondere, dass es nie ein weiteres $\gamma$ geben
kann (das nicht Teilmenge von $\alpha$ oder $\gamma$) ist. Wenn es nun also nie
ein solches $\gamma$ gibt, dann muss man $\beta$ nie vertauschen weil es nie
*verschiedene* konkrete Attributwerte fuer $\gamma$ geben kann.

#### Dekompositionsalgorithmus

Der Dekompositionsalgorithmus kann exakt ident fuer MVDs angewandt werden um
eine Relation $R$ in die 4NF zu bringen. Man nehme dabei die Defintion des
Dekompositionsalgorithmus und ersetze nach der Verallgemeinerungsregel jedes
Vorkommen von $\rightarrow$ durch $\twoheadrightarrow$ und man erhaelt den
Dekompositionsalgorithmus fuer die 4NF. Naehere Definition ist auch nicht
noetig, aber ein Beispiel sei betrachtet.

Gegeben das Schema $\text{Assis}:
\{[\text{PersNr, Name, Fachgebiet, Boss, Sprache, ProgrSprache}]\}$. Mit den
geltenden Abhaengigkeiten:

1. $\text{PersNr} \rightarrow \text{Name,Fachgebiet,Boss}$
2. $\text{PersNr} \twoheadrightarrow \text{Sprache}$
3. $\text{PersNr} \twoheadrightarrow \text{ProgrSprache}$

Zuerst wenden wir die Verallgemeinerungsregel an um die FD $\text{PersNr}
\rightarrow \text{Name,Fachgebiet,Boss}$ in die MVD $\text{PersNr}
\twoheadrightarrow \text{Name,Fachgebiet,Boss}$ umzuwandeln. Dann bestimmen wir
zunaechst den Kandidatenschluessel. Es ist hier essentiell zu wissen, dass MVDs
nicht in die Schluesselbestimmung einfliessen. D.h. Es gibt gar keine FD von
$\text{PersNr}$ auf $\text{Sprache}$ und $\text{ProgrSprache}$. Das bedeutet
wiederum, dass wir diese beiden Attribute auch noch in den Kandidatenschluessel
ziehen muessen. Wir haben also den Schluessel $$\kappa = \{\text{PersNr, Sprache,
ProgrSprache}\}$$.

Dann pruefen wir, in welcher Normalform die Relation ist:

1. NF: Ja.
2. NF: Nein. Der $\text{Name$} ist z.B. nur von $\text{PersNr}$ abhaengig, also
   nicht voll funktional von jedem (dem einzigen) *Kandidatenschluessel* (nicht
   Superschluessel) abhaengig.

Nehmen wir an, die Relation waere ohne den MVDs noch in BCNF, dann wuerden die
MVDs aber auch die 4NF verletzen: Bei $\text{PersNr} \twoheadrightarrow
\text{Sprache}$ und $\text{PersNr} \twoheadrightarrow \text{ProgrSprache}$ ist
in beiden Faellen $\text{PersNr}$ nicht Superschluessel (nur Teil davon, aber
das ist egal). Also waere die 4NF auf Grundlage der MVDs auch verletzt.

Dann fuehren wir
wie gewohnt den Dekompositionsalgorithmus aus:

1. Starte mit $Z = \{\mathcal{R}\}$.

2. Solange $\mathcal{R}$ noch nicht in 4NF ist:

	1. Waehle eine die 4NF verletzende MVD: $\text{PersNr} \twoheadrightarrow
       \text{Name,Fachgebiet,Boss}$

	2. Zerlege sie in zwei Relationenschemata $\mathcal{R}_1,\mathcal{R}_2$:

	   1. $\mathcal{R}_1 := \alpha \cup \beta = \{[\text{PersNr, Name, Fachgebiet, Boss}]\}$

	   2. $\mathcal{R}_2 := \mathcal{R} - \beta = \{[\text{PersNr, Sprache, ProgrSprache}]\}$

   3. Ersetze $\mathcal{R}$	in $Z$ durch die beiden neuen Relationen.

3. Springe zu (2), da $\mathcal{R_2}$ noch nicht in 4NF ist:

	1. Waehle eine die 4NF verletzende MVD: $\text{PersNr} \twoheadrightarrow
       \text{Sprache}$

	2. Zerlege sie in zwei Relationenschemata $\mathcal{R}_3,\mathcal{R}_4$:

	   1. $\mathcal{R}_3 := \alpha \cup \beta = \{[\text{PersNr, Sprache}]\}$

	   2. $\mathcal{R}_4 := \mathcal{R} - \beta = \{[\text{PersNr, ProgrSprache}]\}$

   3. Ersetze $\mathcal{R}$	in $Z$ durch die beiden neuen Relationen.

4. Fertig.

Wir haben also nun drei neue Relationenschemata in 4NF:

1. $\text{Assis}: \{[\text{PersNr, Name, Fachgebiet, Boss}]\}$

2. $\text{Sprachen}: \{[\text{PersNr, Sprache}]\}$

3. $\text{Programmiersprachen}: \{[\text{PersNr, ProgrSprache}]\}$

Offensichtlich ist dies eine viel schoenere Zerlegung. Die urspruengliche
Abhaengigkeit des Schluessels $\text{PersNr, Sprache, ProgrSprache}$ ist hierbei
nicht erhalten geblieben, aber das ist uns auch recht.

Es sei angemerkt dass man auch auf die Trivialitaet von MVDs achten muss. Wenn
$ABC$ Superschluessel einer Relation $\mathcal{R}: \{[A,B,C]\}$ mit MVD $AB
\twoheadrightarrow C$ ist, darf man nicht sagen "$AB$ ist nicht Superschluessel,
daher ist die Relation nicht in 4NF". Dieses Kriterium gilt ja nur fuer
__nicht-triviale__ MVDs. Da hier $C = \mathcal{R} - AB$ ist die MVD aber
trivial, und daher OK. Spaetestens im Dekompositionsalgorithmus wuerde man
merken dass bei $AB \cup C$ wieder $ABC$ rauskommt, was ja auf eine
Endlosschleife hindeuten wuerde.

## Gotchas

* Vergesse beim bestimmen von Schluesseln nicht auf die Attribute der Relation,
  die in keiner FD enthalten sind!
* Schaue bei der Linksreduktion einfach, ob bei $AB \rightarrow C$ entweder $A
  \rightarrow B$ oder umgekehrt gilt, dann ist $C$ immer in der Attributhuelle
  vom Determinanten!
