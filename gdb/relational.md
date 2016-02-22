# Relationale Modell

Das relationale Modell als Datenbankentwurfstechnik basiert auf mathematischen
Relationen. Gegeben eine (Multi-)Menge $D_1,...,D_n$ von $n$ *Domaenen*, welche
nicht unbedingt verschieden sein muessen. Mit einer Domaene ist hierbei ein
*Typ*, beispielsweise Zahlen (integer) oder Zeichenketten (strings)
gemeint. Dann ist eine Relation $R$ eine Teilmenge des kartesischen Produkts der
$n$ Domaenen:

$R \subseteq D_1 \times ... \times D_n$

Hierbei nennt man die Menge der Domaenen auch das *Schema* der Relation. Egal
welche konkreten Werte die Tupel der Relation $R$ annehmen, das Schema bleibt
immer konstant. Die Menge der einzelnen Tupel $(d_1,d_2,...,d_n) \in R$ nennt
man dann die *Auspraegung* der Relation. Diese Daten duerfen sich natuerlich
veraendern. Die *Stelligkeit* (arity) des Schemas gibt hierbei an, wieviele
Domaenen gekreuzt wurden, also eben genau die Zahl $n$.

Im Bereich der Datenbanksysteme wird der obige *mathematische* Formalismus noch
erweitert bzw. ein wenig veraendert. Zunaechst wird eine Relation als ganzes
meist als Tabelle dargestellt, und somit auch oft einfach als "Tabelle"
bezeichnet. Die Domaenen nennen wir dann die *Attribute* bzw. *Felder* der
Tabelle. Diese Attribute haben dann auch noch Namen, nicht nur einen Typ, da ja
eine Domaene alleine nur z.B. `integer` ist, aber wir natuerlich gerne den
`integer`n einen Namen wie *Alter* oder *Telefonnummer* geben moechten, je nach
Situation. In einem Telefonbuch wuerde eine passende Relation dann wohl die
Attribute *Name* und *Telefonnummer* enthalten. Die Tupel der Relation werden in
einer Tabelle dann als Zeilen dargestellt, wobei die einzelnen Elemente der
Tupel natuerlich dem Schema angepasst sind. Beispiel:

|    Telefonbuch     | <-- Name der Relation/Tabelle
|Name  |Telefonnummer| <-- Attribute des Schemas
|:-----|:------------|
|Peter | 1234567890  |-- Tupel der Auspraegung
|Hans  | 6666666666  | /

Relationenschemata werden nun in einem bestimmten Format angegeben:

$\text{Name}: \{[\text{Attribut}: \text{Datentyp}]\}$

Jede Relation hat also natuerlich einen Namen. Eine Relation ist dann wie oben
gesagt eine Teil*menge* des Kreuzprodukts, also werden zunaechst Mengenklammern
angegeben. Dann enthaelt die Relation, also die Menge Tupel, welche durch eckige
Klammern angegeben werden. In den Tupelklammern wird dann das Schema
spezifiziert. Zu jedem Attribut des Schemas gibt es nun einen Namen, sowie einen
Datentypen wie `string`, `integer` oder `date`. Der Datentyp wird hierbei oft
weggelassen, wenn er fuer die momentane Fragestellung nicht relevant ist. Dieses
Schema-Tupel kann gewissermassen als Konstruktor fuer die eigentlichen Tupel der
Auspraegung gesehen werden.

Jene Menge von Attributen, dass ein Tupel der Auspraegung innerhalb der gesamten
Auspraegung eindeutig identifiziert, also die *Schluesselattribute*, werden
hierbei unterstrichen angezeichnet.

## ER $\Rightarrow$ Relation

Wie setzt man nun ein Entity-Relationship Diagramm in die eben vorgestellte
relationale Strukturierungsweise um?

### Entities

Nehmen wir uns dazu zuerst Entities. Entities koennen relativ direkt in ein
Schema umgewandelt werden, wobei man nur die Attribute des Entitytypen zu
betrachten braucht. Sei als Beispiel $E$ einen Entitytypen mit den Attributen
$A_1,...,A_n$, wobei $A_k$ mit $1 \leq k \leq n$ das Schluesselattribut des
Entitytypen ist. Wir koennen daraus direkt das folgende Schema bauen:

$\text{Name}: \{[A_1: T_1, ..., \underline{A_k: D_k}, ..., A_n: D_n]\}$

Hierbei sind $D_1,...,D_n$ also die Datentypen der Attribute, welche man sich
eben selbst ausdenken muss (z.B. string, integer, date usw.), oder einfach
weglassen kann. Die Schluesselattribute (hier nur $A_k$) sind wie man sieht auch
unterstrichen.

Man beachte, dass Generalisierung in Tabellen nicht direkt umsetzbar ist, weil
Attribute nicht "vererbt" werden koennen. Entweder man hebt die Generalisierung
vollkommen auf und gibt die gemeinsamen Attribute in die Untertypen, oder man
faktorisiert die gemeinsamen Attribute wirklich in eine eigene Relation, wobei
man das Schluesselattribut dann aber den Ober- und Untertypenrelationen zuordnen
muss. Ueber einen Join koennte man dann nachtraeglich die Attribute vereinen.

Bezueglich schwachen Entities sei nur nochmals angemerkt, dass ihr Schluessel
aus dem teil-identifizierenden Attribut des schwachen Entities *sowie auch* dem
Schluessel des uebergeordneten starken Entities besteht. Somit muss man beim
erstellen eines Schemas fuer ein schwaches Entity also auch den Schluessel des
starken Entities als Schluessel und gleichzeitig Fremdschluessel
uebernehmen. Fuer Beziehungen zwischen dem schwachen Entity mit anderen Entities
muss man dann eben auch diese Kombination aus Schluesseln des schwachen und
starken Entities als Fremdschluessel in die Beziehungsrelation uebernehmen.

### Beziehungen

Beziehungen lassen sich nicht so direkt, alleine ueber den Attributen der
Beziehungen, in Schemata uebersetzen. Das folgt daraus, dass Beziehungen
natuerlich zwischen Entities gegeben sind, und irgendwie Referenz zu den in
Beziehung stehenden Entities machen muss. Hierbei ist die Regel jedoch einfach,
dass eine Beziehung neben ihren Attributen noch die *Schluesselattribute* aller
Entities, die die Beziehung verbindet, beinhalten muss. Ueber den funktionalen
Abhaengigkeiten kann man dann wiederum die Schluesselmenge der
Beziehungsrelation bestimmen (welche also nicht unbedingt *alle*
Schluesselattribute sind).

Die Schluesselattribute der Entities, die die Relation verbindet, nennt man
dabei *Fremdschluessel* in der Beziehungs-Relation, da sie dazu dienen, Tupel
aus den (externen) Entities zu identifizieren. Man beachte auch noch, dass man
oft die Namen der Attribute der Entity-Schemen veraendern muss, wenn man sie in
das Beziehungs-Schema ueberfuehrt. Beispielsweise kann sowohl die Relation
*Mensch* als auch die Relation *Hund* das Schluesselattribut *Name* haben. Fuer
die Beziehungs-Relation *liebt* zwischen Mensch und Hund koennte man jetzt nicht
zweimal Name als Fremdschluessel haben, da man dann nicht wuesste, ob *Waui* der
Name eines Menschen oder eines Hundes ist. Man wuerde also die Relation so
erstellen muessen:

$\text{liebt}: \{[\text{MenschName}: string, \text{HundName}: string]\}$.

### Verfeinerung

Es gibt nun die Moeglichkeit, Relationen mit __selben Schluesselattributen__
zusammenzufassen. Man nennt diese Methode des Zusammenfassens von Relationen
einer Menge an Relationen dabei *Verfeinerung* des Modells. Bei der
Verfeinerung zweier Relationen mit selbem Schluessel kann man hierbei die
Attribute der einen Relation in die andere Relation uebernehmen, und
gegebenenfalls noch umbennnen.

Nehmen wir hierfuer die Beziehung *lesen* zwischen *Professoren* und
*Vorlesungen*. Man wuerde zunaechst diese drei Schemata fuer diese Entities
bzw. die Beziehung *lesen* erstellen:

1. $\text{Vorlesungen}: \{[\underline{\text{VorlNr}}, \text{Titel}]\}$
2. $\text{Professoren}: \{[\underline{\text{PersNr}}, \text{Name}]\}$
3. $\text{lesen}: \{[\underline{\text{VorlNr}}, \text{PersNr}]\}$

Fuer die Relation *Vorlesungen* ist natuerlich *VorlNr* das Schluesselattribut,
fuer *Professoren* *PersNr* und fuer die Beziehungsrelation *lesen* ebenso
*VorlNr*, weil ueber die Vorlesung eindeutig der lesende Professor (und somit
dann das ganze Schema) identifizierbar ist.

Wir haben hier also die Situation, dass Relation (1) und (2) die selben
Schluesselattribute, *VorlNr*, besitzen. Nach obiger Regel duerfen wir sie also
im Sinne der Verfeinerung zusammenfassen. Wir ziehen hierfuer alle Attribute aus
der einen Relation in die andere Relation, und loesen die erste Relation
auf. Hierfuer haben wir nun fuer obiges Beispiel zwei Moeglichkeiten:

1. Wir schieben die Attribute von *Vorlesungen* in die Relation *lesen*:
   $\text{lesen}: \{[\underline{\text{VorlNr}}, \text{PersNr}, \text{Titel}]\}$

2. Wir schieben die Attribute von *lesen* in die Relation *Vorlesungen*:
   $\text{Vorlesungen}:
   \{[\underline{\text{VorlNr}}, \text{Titel}, \text{gelesenVon}]\}$

#### `null`-Werte

Was man hier bei der Wahl, welche Relation man in welche andere aufloest,
beachten sollte, ist dass man `null`-Werte vermeidet. `null`-Werte koennen bei
der Verfeinerung entstehen, wenn man die Attribute der einen Relation in die
andere Relation zieht, aber diese neuen Attribute dann nicht fuer jedes konkrete
Tupel mit sinnvollen Werten bestimmt werden koennen. Dazu mache man sich
bewusst, dass auch wenn verschiedene Relationen dieselben Attribute haben, sie
natuerlich andere Auspraegungen haben koennen, wobei die konkreten Elemente der
Tupel einer bestimmten Auspraegung dann auch eigene Eigenschaften besitzen
koennten.

Man nehme beispielsweise die Relationen *Mensch* und *liebt*:

* $\text{Mensch}: \{[\underline{\text{PersNr}}, \text{Name}]\}$
* $\text{liebt}: \{[\underline{\txt{PersNr}}, \text{KatzeNr}]\}$

Hierbei sind die Schluessel der beiden Relationen beide *PersNr*. Man koennte
diese Relationen also nun verfeinern. Jedoch wuerde aus einer solchen
Verfeinerung ein Problem, bzw. ein "unschoener" Datenbankzustand folgen: Nicht
jeder der 7 Millarden Menschen auf der Welt liebt eine Katze. Wuerde man also
das Attribut *Name* in die Relation *liebt* uebernehmen, so gaebe es unzaehlige
Tupel, wo *KatzeNr* `null` waere. Auch wenn man das Attribut *KatzeNr* in die
Relation *Mensch* uebernimmt, gaebe es ebenso unzaehlige Eintraege, wo *KatzeNr*
den Wert `null` haben muesste. Eine solche Verfeinerung sollte man also, auch
wenn sie moeglich ist, vermeiden.

## Relationale Algebra

Es gibt zwei Sprachen mit der man Verfahren zum Extrahieren von Daten aus einer
Datenbank (bzw. deren Relationen) beschreiben kann: die relationale Algebra und
das Tupelkalkuel. Ersteres wird hier nun behandelt.

Die relationale Algebra hat als Traegermenge die Relationen des konkreten
Universums indem wir uns befinden, und als Operatoren eine Reihe von
Manipulationen auf den Relationen. Diese Algebra ist abgeschlossen, also jede
Operation ueber Relationen ergibt auch wieder eine Relation, wenn sie auch
manchmal nur aus einem Tupel mit nur einem Attribut, besteht, was man dann lax
als einen alleinestehenden Wert sehen koennte.

Hier nun aufgelistet und dann weiter beschrieben die Operationen der
relationalen Algebra:

* Die Selektion ueber einem Praedikat $p$: $\sigma_p$
* Die Projektion einer Menge von Attributen: $\Pi_A$
* Die Umbennenung eines Attributs zu einem anderen: $\rho_{a\leftarrow b}$
* Mehrere Varianten des Joins: $\bowtie$
* Die relationale Division: $\div$
* Die relationale Differenz: $-$
* Die relationale Vereinigung: $\cup$
* Der relationale Schnitt: $\cap$
* Das Kreuzprodukt: $\times$
* Die Gruppierung und Aggregation: $\gamma$

### Selektion

Die Selektion der relationalen Algebra waehlt aus einer Relation (bzw. deren
Auspraegung) alle jene Tupel aus, welche eine bestimmte Bedingung
erfuellen. Eine solche Bedingung nennt man allgemein ein *Praedikat* (auch bei
anderen Operatoren), und hier spezifisch das *Selektionspraedikat*. Die
Selektion hat dabei als Operator das Symbol $\sigma$ und das Praedikat $p$ als
Subskript: $\sigma_p$. Es erhaelt als Argument eine Relation und gibt eine neue
Relation zurueck, welche alle jene Tupel der Eingaberelation enethaelt, die wie
gesagt das Praedikat erfuellen.

Das Praedikat kann hierbei allgemein als eine Formel $F$ beschrieben werden, die
besteht aus:

1. Den Attributnamen der Argumentrelation $R$ oder beliebige Konstanten.

2. Den arithmetischen Vergleichsoperatoren $<,\leq,>,\geq,=,\neq$.

3. Den logischen (Bool'schen) Operatoren: $\land$ (Konjunktion), $\lor$
   (Disjunktion) und $\neg$ (Negation).

Ein Beispiel fuer ein Praedikat waere die Formel $F: \text{Semester} > 10$,
wobei die Selektion $$\sigma_{\text{Semester} > 10}(\text{Studenten})$$ dann
eben genau jene Studenten in eine neue, zurueckgegebene Relation packt, fuer
dessen konkrete Werte des Attributs *Semester* groesser als die Konstante $10$
sind.

### Projektion

Waehrend der Selektionsoperator $\sigma$ Zeilen (Tupel) selektiert hat, waehlt
der Projektionsoperator $\Pi$ Spalten (Attribute) einer Relation aus. Als
Subskript erhaelt dieser Projektionsoperator nun nicht ein Praedikat, sondern
eine Menge von Attributen $\{A_1,...,A_n\}$, die eben aus der Eingaberelation
projeziert werden sollen. Die Mengenklammern werden dabei aber ueblicherweise
weggelassen, also wird nur eine Sequenz angegeben:
$$\Pi_{A_1,...,A_n}$$.

Betrachtet man die Relation $$\text{Studenten}:
\{[\underline{\text{MatrNr}}, \text{Name}, \text{Semester}]\}$$, dann wuerde die
Projektion $$\Pi_{\text{Name, Semester}}$$ nur den Namen und das Semester aus
der Relation projezieren. Die Ausgaberelation haette also das Schema
$\{\text{Name}, \text{Semester}\}$.

Allgemein darf in einer Relation nie ein Duplikattupel enthalten sein. Da jede
Relation einen Schluessel hat, ist das auch sichergestellt, da ein Schluessel
per Definition ja eindeutig sein muss. Wenn man nun aber, wie eben, nur
Nicht-Schluesselattribute projeziert, kann es durchaus geschehen, dass man
Duplikattupel erhaelt. Beispielsweise koennte es zwei Studenten mit dem Namen
*Irmgard* geben, welche beide im dritten Semester sind. Diese Irmgards sind
gewiss zwei verschiedene Personen und hatten vor der Projektion auch
verschiedene Matrikelnummern (was der Schluessel der Relation war), aber durch
die Projektion von Name und Semester geht diese Eindeutigkeit verloren. Daher
muss man die Duplikattupel danach auch einfach eliminieren, wenn man manuell die
Projektion nachahmen wollen wuerde. Bei der Anwendung der Projektion in einer
relational-algebraischen Formel kann man diese Eigenschaft der
quasi-automatischen Duplikatelimination natuerlich schon als gegeben betrachten.

### Vereinigung

Die relationale Vereinigung $\cup$ nimmt zwei Relationen als Argument, und gibt
eine neue Relation zurueck, die alle Tupel enthaelt, die entweder in der einen
oder der anderen Relation enthalten sind. Wenn man die Relationen wirklich als
die Mengen von Tupeln betrachtet, die sie ja auch (grundlegend) sind, dann ist
dieser Operator auch einfach die herkoemmliche Mengenvereinigung $\cup$.

Es ist hierbei __sehr wichtig__ anzumerken, dass nur Relationen mit dem __exakt
selben Schema__ vereinigt werden koennen. Selbes Schema bedeutet dabei, dass die
Relationen die selben Attribut*namen* und Attribut*typen* (Domaenen) haben
muessen. Man muss zuvor also moeglicherweise eine Projektion und/oder eine
Umbenennung durchfuehren, bevor man die beiden Relationen vereinigen kann.

Seien beispielsweise die folgenden Relationen gegeben:

$\text{Menschen}: \{[\underline{\text{Name}}, \text{Telefonnummer}]\}$
$\text{Katzen}: \{[\underline{\text{Name}}, \text{Rasse}]\}$

Dann koennte man hierbei nicht direkt eine Vereinigung durchfuehren. Man muesste
zuerst den Namen der beiden Relationen herausprojezieren, um ein einheitliches
Schema (das hier nur das Attribut *Name* besitzen wuerde) zu erstellen:

$$\Pi_{\text{Name}}(\text{Menschen}) \cup \Pi_{\text{Name}}(\text{Katzen})$$

### Differenz

Die *relationale Differenz* $R - S$ aus zwei Relationen $R,S$ ist die Menge aller
Tupel in $R$, die nicht auch in $S$ enthalten sind. Man "subtrahiert" also die
Tupel aus $S$ aus der Relation $R$. Wie bei der Vereinigung muessen auch hier
die beiden Relationen __das selbe Schema__ haben.

Beispielsweise waere $\text{Menschen} - \text{Maenner}$ die Relation, die nur
Tupel fuer Frauen enthaelt (politisch korrekt: Nicht-Maenner).

### Kartesisches Produkt

Das *kartesische Produkt* bzw. *Kreuzprodukt* zweier Relationen $R,S$ ist
natuerlich gleich dem kartesischen Produkt bei allgemeinen Mengen. Da Symbol
dieses Operators ist $\times$ und es erhaelt zwei Eingaberelationen. Es gibt
dabei eine neue Relation aus, die fuer jedes Tupel in $R$ genau $|S|$ Tupel
enthaelt, wobei also ein konkretes Tupel $r$ aus $R$ mit jedem konkreten Tupel
$s$ aus $S$ vereinigt(/konkateniert) wird. Die Elemente aus $r$ und $s$ werden also
sozusagen ausgepackt und in ein neues Tupel gesteckt, welches dann in der
Ausgaberelation enthalten ist.

Die Kardinalitaet der Relation $R \times S$ ist natuerlich $|R| \cdot |S|$, weil
es fuer jedes $r \in R$ genau $|S|$ Partner gibt.

Die Schemata der Relationen koennen hierfuer verschieden sein. Das Schema der
ausgegebenen Relation ist genau die Vereinigung der Schemata von $R$ und $S$,
enthaelt also alle Attribute des Schemas von $R$ und des Schemas von $S$. Wenn
Attribute der beiden Relationen gleichbenannt sind, dann sind sie danach dennoch
ueber *qualifizierte* Namen verfuegbar, werden durch die Vereinigung also nicht
eliminiert. Der qualifizierte Name eines Attributs $A$ einer Relation $R$ ist
dabei $R.A$.

Haette man die Relationen $R = \{[a,1][b,2]\}$ und $S = \{[x,8], [y,9]\}$ mit
gemeinsamen Schema $\{[\text{Buchstabe}, \text{Zahl}]\}$, dann haette die
Relation $R \times S$ gerade die vier ($|R| \cdot |S| = 2 \cdot 2$) Tupel:

$$\{[a,1,x,8][a,1,y,9][b,2,x,8][b,2,y,9]\}$$

mit Schema $\{[R.\text{Buchstabe}, R.\text{Zahl}, S.\text{Buchstabe},S.\text{Zahl}\}$

### Umbenennung

Oftmals ist es notwendig, Attribute oder sogar ganze Relationen
umzubenennen. Moechte man beispielsweise eine beliebige Operation zwischen einer
Relation und sich selbst durchfuehren, so ist das nur moeglich, wenn die eine
Kopie der Relation temporaer in eine neue umbenannt wird (beispielsweise wuerde
selbst bei einem Kreuzprodukt die Qualifizierung versagen, da man ja dennoch
$R.A$ zweimal haette). Auch kann es notwendig sein, ein Attribut umzubenennen
(beispielsweise um volle Schemagleichheit zu erlangen).

Fuer diese beiden (separaten) Operatoren wird jeweils der Operator $\rho$
verwendet. Fuer beide Varianten des Operators ist die Eingabe eine Relation. Die
Umbenennung wird immer im Subskript spezifiziert.

Fuer Relationenumbenennungen wird im Subskript also der neue Name
angegeben. $\rho_X(A)$ nennt die Relation $A$ zu $X$ um. Es sei angemerkt, dass
eine Umbennung in Wirklichkeit einfach eine Kopie der Auspraegung der Relation
erstellt und dieser Kopie den spezifizierten neuen Namen gibt.

Fuer Attributumbennungen wird hingegen das Format $\rho_{A_{neu}\leftarrow
A_{alt}}$ benutzt. Der alte Attributname $A_{alt}$ steht also rechts und ein
Pfeil, der nach *links* zeigt, zeigt auf den neuen Namen $A_{neu}$.

### Aggregation

Die Aggregation $\gamma$ ist eine erweiterte Operation, die ueber die
Standardalgebra hinausgeht. Ihr Sinn ist es, die Tupel einer Relation nach
bestimmten Kritereien zusammenzufassen (zu *gruppieren*), und womoeglich noch
zusaetzliche Operationen auf diesen Gruppen durchzufuehren.

Nimmt man beispielsweise die Relation
$\text{Studenten}: \{[\underline{\text{MatrNr}}, \text{Semester}]\}$ zur Hand,
dann koennen wir mit der Operation $\gamma_{\text{Semester}}(Studenten)$ die
Studenten nach Semester gruppieren. Konkret bedeutet das, dass zuerst eine
*Projektion* nach dem Attribut *Semester* durchgefuehrt wird, und dann die
Duplikate eliminiert werden.

Bevor die Duplikatelimination durchgefuehrt wird, koennen wir jedoch noch etwas
mit den ganzen Tupeln machen. Hierfuer gibt es dann eine Reihe von
Standardfunktionen, welche alle jeweils ein Attribut oder $*$ als "Wildcard"
fuer *alle Attribute* als Argument haben. Das Ergebnis der Aggregatoperation
wird als zusaetzliches Attribut in das Schema der Ausgabe-Relation gegeben.

* $count$: Zaehlt die Tupel jeder Gruppe. So wuerde $count(*)$ beispielsweise
  alle Tupel fuer eine Gruppe zaehlen.

* $sum$: Summiert die Werte der Tupeleintraege fuer ein Attribut.

* $min$: Selektiert den minimalen Wert der Tupeleintraege fuer ein Attribut.

* $max$: Selektiert den maximalen Wert der Tupeleintraege fuer ein Attribut.

* $avg$: Berechnet den Durchschnitt der Werte fuer ein Attribut.

Hierbei sind $min,max,sum$ und $avg$ natuerlich nur fuer numerische Werte
sinnvoll.

Als Beispiel koennen wir die Operation $count(*)$ fuer die obige Gruppierung
$\gamma_{\text{Semester}$ durchfuehren. Das schreiben wir dann als
$\gamma_{\text{Semester};count(*)}(Studenten)$ an. Konkret passiert dabei
folgendes:

1. Die Relation *Studenten* wird nach dem Attribut *Semester* projeziert.
2. Fuer jeden unterscheidbaren Wert aus dieser resultierenden Relation wird nun
   gezaehlt, wieviele Tupel es denn mit diesem unterscheidbaren Wert
   gibt. Beispielsweise, wievele Tupel es mit einer $1$ gibt -- also wieviele
   Ersemester.
3. Dann wird die Ausgaberelation angelegt. Sie hat das Attribut *Semester* sowie
   das Attribut $count(*)$, wobei fuer das Attribut *Semester* jeweils die Zahl
   des Semesters angegeben wird und durch $count(*)$ eben wieviele Tupel es mit
   dieser Zahl gab. Diese Relation enthaelt dann keine Duplikate mehr.

### Joins

Ein Join (deutsch "Verbund") ist in der relationalen Algebra ein sehr wichtiger
Operator. Sein grundlegender Gedanke ist es, beim Kreuzprodukt schon eine
Vorausfilterung zu erledigen. Nehmen wir zwei Relationen $R,S$, dann gilt:
$|R \times S| = |R| \cdot |S|$. Wir erhalten also eine immense Anzahl an Tupeln
beim Kreuzprodukt. Von diesen brauchen wir aber letztendlich vielleicht nur
wenige, also wuerden wir dann eine Selektion $\sigma$ machen. Genau diese beiden
Operationen vereinigt der Join (ungefaehr). Es gibt hierbei ziemlich viele
Varianten des Joins, welche dann weiter erklaert werden:

* Den *natuerlichen Verbund* (natural-join)
* Den *allgemeinen Verbund* bzw. *Theta-Join*.
* Den *Semi-Join*.
* Den *Left-Outer-Join*.
* Den *Right-Outer-Join*.

#### Natuerlicher Verbund

Der natuerliche Verbund ist zwar nicht der allgemeinste, aber intuitivste
Verbund. Sein Symbol ist $\bowtie$. Gegeben seien zwei Relationen $R,S$. Diese
Relationen seien so definiert, dass $R$ das Attribut $A$ sowie $B$ hat, und die
Relation $S$ das selbe Attribut $B$ aber noch ein weiteres nicht-gemeinsames
Attribut $C$ hat. Dann wird der natuerliche Verbund fuer jedes Tupel $r \in R$
und jedes Tupel $s \in S$ immer dann, wenn $r.B = s.B$, ein neues Tupel
erstellen, worin $r.A,r.B=s.B,s.C$ enthalten sind. Also immer dann wenn ein
Attributwert des gemeinsamen Attributs in $R$ und $S$ gleich sind, werden das
ganze Tupel aus $R$ und das ganze Tupel aus $S$ vereinigt, wobei das gemeinsame
Tupel nur einmal uebernommen wird. Das Attribut bzw. die Attribute, ueber welche
*gejoined* wird, also die gemeinsamen, nennt man dabei das/die *Join-Attribut/e*.

Kurz und buendig: es werden alle jene Tupel uebernommen wo gemeinsame Attribute
gemeinsame Attributwerte haben.

Ein Beispiel:

|      R       |
|:-------------|
| Name | Alter |
|:-----|:------|
|  A   |   37  |
|  A   |   20  |
|  X   |   12  |
|  C   |   19  |

|        S        |
|:----------------|
| Name | Ort      |
|:-----|:---------|
|  A   | Mainz    |
|  Y   | Koeln    |
|  C   | Muenchen |


Dann ist der Verbund $S \bowtie R$:

| Name | Ort      | Alter |
|:-----|:---------|:------|
|  A   | Mainz    |   37  |
|  A   | Mainz    |   20  |
|  C   | Muenchen |   19  |

Was man hier beobachten kann:

* Das Tupel mit $X$ aus der Relation $R$ wurde gar nicht uebernommen, weil in
  $S$ kein Tupel mit einem $X$ Eintrag in der Spalte des Join-Attributs gegeben
  ist. Selbes gilt fuer $Y$ in der Relation $S$.

* In der resultierenden Relation gibt es einen Eintrag fuer $C$, weil $C$ im
  Join-Attribut sowohl in $R$ als auch in $S$ enthalten ist. Wie man sieht,
  werden bei einem solchen Tupel neben dem gemeinsamen Attribut auch die
  Attributwerte der Attribute uebernommen, die nur in einer Relation enthalten
  sind (Ort aus $S$ und Alter aus $R$).

* Weil es in $R$ zwei Eintraege fuer $A$ gibt, und einen Eintrag mit $A$ im
  Join-Attribut in $S$, gibt es auch $2 \cdot 1 = 2$ solche Eintraege mit $A$ im
  Join-Attribut in der resultierenden Relation. Denn immer dann, wenn der
  Attributwert des gemeinsamen Attributs gleich ist, wird ein Eintrag
  erstellt. Eben auch, wenn es mehrere passende *Join-Partner* gibt.

"Im Hintergrund" fuehrt der Join-Operator mehrere primitivere Operationen durch:

1. Zuerst wird ein Kreuzprodukt zwischen $R$ und $S$ gebildet: $R \times S$

2. Dann werden all jene Tupel selektiert, wo die gemeinsamen Attribute die
   selben Attributwerte haben, also wo $R.B_1=S.B_1,...,R.B_n=S.B_n$
   gilt. $R.B_1,...,R.B_1$ sowie $S.B_1,...,S.B_n$ seien hierbei alle
   gemeinsamen Attribute von $R$ und $S$.

3. Letztlich wird noch eine Projektion nach
   $A_1,...,A_n,R.B_1,...,R.B_n,C_1,...,C_n$ durchgefuehrt. Hierbei sind
   $A_1,...,A_n$ alle Attribute, die nur in $R$ vorkommen (im Beispiel oben waren
   das nur *Alter* in $R$). Dann sind $C_1,...,C_n$ alle Attribute, die nur in $S$
   vorkommen (oben *Ort*). Letztlich sind $R.B_1,....,R.B_n$ die gemeinsamen
   Attribute. Weil diese in einem selektierten Tupel zweimal (*eben mit identischen
   Werten*) enthalten sind, werden nur die Werte $R$ selektiert (ebenso koennte man
   also auch $S.B_1,...,S.B_n$ projezieren, mit exakt gleichem Resultat).

Das Schema von $R \bowtie S$ ist dann: $R - S, R \cap S, S - R$. Also die Attribute,
die nur in $R$ enthalten sind, dann die gemeinsamen, und dann die, die nur in
$S$ enthalten sind.

Letztlich sei nur noch angemerkt, dass wenn $R \cap S$ leer ist, $R \bowtie S$
aehnlich der Definition des Allquantors genau gleich $R \times S$ sein
wird. Auch sei noch gesagt, dass waehren $|R \times S| = |R| \cdot |S|$ galt,
nun gilt $|R \bowtie S| = |R-S| + |R \cap S| + |S-R|$ ist.

### Theta-Join

Der *Theta-Join* oder *allgemeine Verbund* $\bowtie_\theta$ ist nun wirklich nur
syntaktischer Zucker fuer eine Selektion nach einem Kreuzprodukt. Wie oben
beschrieben war ein natuerlicher Verbund noch durch eine besondere Selektion
sowie eine Projektion definiert. Ein Theta-Join $R \bowtie_\theta S$ ist nun
exakt identisch zum Ausdruck $\sigma_\theta(R \times S)$. $\theta$ ist also ein
allgemeines *Praedikat* wie bei der Selektion, wird hier aber als
*Join-Praedikat* bezeichnet. Im Sinne der relationalen Algebra mag ein
Theta-Join unnuetz erscheinen, aber in der Realisierung von Datenbanken kann man
sich dadurch die Aufblaehung des Resultats durch das Kreuzprodukt sparen.

Man bemerke an der Definition das hierbei auch keinerlei "Projektion von
gemeinsamen Attributen" oder sonstige Magie stattfindet. Ist das Praedikat
$\theta$ beispielsweise $R.A = S.A$ fuer zwei Relationen $R,S$, dann werden im
Resultat sowohl $R.A$ als auch $S.A$ enthalten sein, selbst wenn sie identisch
sind. Beim natuerlichen Verbund wuerden diese beiden Attribute (angenommen es
ist nur dieses gemeinsam) ja zu einem zusammenfallen (was aus der Projektion im
letzten Schritt der Definition des natuerlichen Verbunds folgt).

### Aeusserer Join

Bisher (also fuer natuerliche und Theta-Verbuende) wurden Tupel, die keinen
*Join-Partner* finden, immer aus dem Ergebnis eliminiert. Der aeussere Join ist
nun der erste Join, der Tupel ohne Join-Partner auch erhaelt. Bei dieser
konkreten Variante werden alle Tupel, die einen Join-Partner finden, wie auch
beim natuerlichen Verbund uebernommen. Jedoch werden nun zusaetzlich auch alle
anderen Tupel uebernommen. Findet sich jedoch kein Join-Partner, werden alle
anderen Attributwerte, die normalerweise durch die Attributwerte des
Join-Partners aus der anderen Relation gefuellt wuerden, mit `null` gefuellt.

Das Symbol des links-aeusseren Join ist wie das normale Join-Symbol $\bowtie$,
aber links und rechts stehen noch Hoerner weg:

```
__      __
  |\--/|
  | \/ |
  | /\ |
__|/--\|__
```


Seien $R,S$ also Relationen. Dann enthaelt $R$ outer-join $S$ zunaenachst alle
Tupel aus $R \bowtie S$. Zusaetzlich gibt es dann aber fuer jeden Eintrag in
$R$, wofuer sich kein Join-Partner aus $S$ finden konnte, einen Eintrag, mit den
Attributewerten dieses Tupels, und `null` fuer die Werte des nicht-vorhandenen
Join-Partner. Dann gibt es noch fuer jedes Tupel in $S$, wofuer sich kein
Eintrag finden lies, ein analoges Tupel mit Attributwerten aus $S$ und sonst nur
`null`.

Hier zwei Beispielrelationen:

|     R     |
|:----------|
|Name |Alter|
|:----|:----|
|Peter| 18  |
|Hans | 69  |

|        S        |
|:----------------|
|Name  |    Ort   |
|:-----|:---------|
|Peter | Muenchen |
|Franz | Hogwarts |


$R$ outer-join $S$ wuerde nun so aussehen:

|  Name | Alter |    Ort   |
|:------|:------|:---------|
| Peter |   18  | Muenchen |
| Hans  |   69  |   null   |
| Franz |  null | Hogwarts |

Was man hier beobachten kann:

1. Weil das Tupel aus $R$ mit *Peter* als Attributwert fuer das gemeinsame
  Attribut *Name* einen Join-Partner hat, wird dieses Tupel vollstaendig
  uebernommen. Es waere auch im natuerlichen Verbund (als einziges) Tupel
  enthalten.

2. Weil Hans keinen Join-Partner in $S$ findet, wird er zwar uebernommen, aber
   der Wert aller Attribute aus $S$, also hier nur *Ort*, ist `null.`

3. Weil Franz keinen Join-Partner in $R$ findet, wird er zwar uebernommen, aber
   der Wert aller Attribute aus $R$, also hier nur *Alter*, ist `null`.

#### Links-Auesserer Join

Der Links-Aussere Join ist nun wie der Aeussere, aber er rettet nur die Loser
aus der linken Relation (im Sinne des Operators). Nach obigem Beispiel waere das
Resultat also:

|  Name | Alter |    Ort   |
|:------|:------|:---------|
| Peter |   18  | Muenchen |
| Hans  |   69  |   null   |

#### Rechts-Aeusserer Join

Symmetrisch zum Links-Aeusseren Join werden bein Rechts-Aeusseren Join nur die
partnerlosen Tupel aus der rechten Relation gerettet. Also wieder das Beispiel:

|  Name | Alter |    Ort   |
|:------|:------|:---------|
| Peter |   18  | Muenchen |
| Franz |  null | Hogwarts |

### Semi-Join

Ein Semi-Join ist allgemein eine Operation, die gleich wie ein natuerlicher
Verbund ist, aber dann nur die Attribute einer Relation uebernimmt. Also fuer
alle Tupel die einen Join-Partner finden (wo gemeinsame Attribute einen
gemeinsamen Attributwert haben), werden nur die Attributwerte aus der einen oder
der anderen Relation uebernommen. Ein Semi-Join ist also aequivalent zu einem
natuerlichen Verbund mit anschliessender Projektion nach linken oder rechten
Attributwerten. Die gemeinsamen Attribute sind darin sehr wohl enthalten.

Das Symbol fuer einen Semi-Join ist wie das fuer einen natuerlichen Verbund,
wobei entweder der linke oder der rechte vertikale Strich wegelassen wird. Wo
der Strich bleibt, bestimmt, welche Attribute erhalten bleiben.

```
|\  /
| \/
| /\
|/  \
```

#### Links-Semi-Join

Der Links-Semi-Join erhaelt nun nur die Attribute der linken Relation. Nach
obigem Beispiel, wo nur Peter einen Join-Partner fand, wuerde also wie beim
natuerlichen Verbund nur Peter als Tupel uebrig bleiben, aber dann auch nur die
Attribute der linken Seite ($R$) erhalten bleiben:

|Name |Alter|
|:----|:----|
|Peter| 18  |

#### Rechts-Semi-Join

Symmetrisch wuerden beim Rechts-Semi-Join nur die rechten Attribute von Peter
erhalten bleiben:

|Name  |    Ort   |
|:-----|:---------|
|Peter | Muenchen |

### Anti-Semi-Join

Ein Anti-Semi-Join ist nun wie ein Semi-Join, wobei aber genau all jene Tupel
aus der einen oder anderen Relation (nach der Semantik des Semi-Joins)
uebernommen werden, die im Semi-Join *nicht* erhalten bleiben wuerden.

Dann also:

1. Links-Anti-Semi-Join:

|Name |Alter|
|:----|:----|
|Hans | 69  |

2. Rechts-Anti-Semi-Join:

|Name  |    Ort   |
|:-----|:---------|
|Franz | Hogwarts |

### Mengendifferenz

Die relationale Mengendifferenz $\cap$ ist eigentlich identisch zum natuerlichen
Verbund, nur dass hier die Restriktion gilt, dass die Relationen das selbe
Schema haben muessen. Weil die Relationen aber das selbe Schema haben muessen,
wuerde ein natuerlicher Verbund genau das selbe tun: alle Tupel selektieren, die
in beiden Relationen enthalten sind. Weil die Schemata gleich sind, ist beim
natuerlichen Verbund auch die ganze Relation die Menge der Join-Attribute, daher
die Gleichheit.

### Relationale Division

Die relationale Divsion $\div$ ist nun ein Operator, der dazu nuetzlich ist,
eine Form von Allquantifizierung durchzufuehren. Seien $R,S$ zwei Relationen,
dann erhaelt man durch $R \div S$ all jene Tupel aus $R$ mit Attribut $A \in R$,
wo fuer die gemeinsamen Attribute $R \cap S$ gilt, dass es in $R$ ein Tupel mit
$A$ gepaart mit *jedem* der Tupel in $S$ gibt.

Nochmals genauer. $R,S$ sind also Relationen. Nun muss fuer die Relation $S$
zunaechst gelten, dass ihr Schema Teilmenge des Schemas von $R$ ist, also die
gleichen oder eine Teilmenge der Attribute enthaelt. Dann enthaelt $S$ also eine
Reihe von Tupeln $t_1,...,t_n$. Nun sei $t$ ein Tupel aus $R$, mit Attributen
$A_1,...,A_n$ die nicht in $S$ vorkommen, sowie den gemeinsamen Attributen
$B_1,...,B_n$. Gilt nun fuer die Attribute $A_1,...,A_n$, dass es fuer jedes
Tupel $(B_1,...,B_n)$ in $S$ ein passendes Tupel $(A_1,...,A_n,B_1,...,B_n)$
gibt, dann kommt das Tupel $(A_1,...,A_n)$ in die Ergebnisrelation.

Man beachte wirklich dass hierbei nur die Attribute aus $R$ ins Resultat kommen,
ohne die gemeinsamen Attribute.


Beispiel: Sei $R$:

|      R    |
|:----------|
|  A  |  B  |
|:----|:----|
|$a_1$|$b_1$|
|$a_1$|$b_2$|
|$a_1$|$b_3$|
|$a_2$|$b_1$|
|$a_3$|$b_1$|
|$a_3$|$b_2$|

Und sei $S$:

|  S  |
|:----|
|  B  |
|:----|
|$b_1$|
|$b_2$|
|$b_3$|

Dann ist $R \div S$:

|  A  |
|:----|
|$a_1$|

Wir beobachten hier: $R \div S$ wird alle Attribute $A$ aus $R$ selektieren,
welche fuer jedes Tupel in $S$ ein passendes Tupel haben. Also alle jene Tupel,
die in $R$ mit jeweils $b_1,b_2,b_3$ gepaart sind. In diesem Fall ist das genau
das Attribut $a_1$, welches also in ein Tupel im Resultat uebernommen wird.

Wir koennen die relationale Division durch andere Operatoren ausdruecken:

$R \div S = \Pi_{R-S}(R) - \Pi_{R-S}((\Pi_{R-S}(R) \times S) - R)$

Was wir hier machen:

1. $\Pi_{R-S}(R)$: projeziere die Attribute aus $R$, die nur in $R$ und nicht in
   $S$ vorkommen.

2. $\Pi_{R-S}(R) \times S$: kreuze diese Attribute mit allen Attributen in
   $S$. Dadurch erhalten wir alle Attribute aus $R$ mit allen Attributen aus $S$
   gepaart. Also die "Traumwelt". Aus dieser Relation wuerde die relationale
   Division alle Tupel erhalten.

3. $(\Pi_{R-S}(R) \times S) - R$: ziehe von dieser "Traumwelt" die echte
   Relation ab. Wir erhalten dadurch alle Tupel die die relationale Division
   nicht erhalten wuerde.

4. $\Pi_{R-S}(R) - \Pi_{R-S}((\Pi_{R-S}(R))$: Wir projezieren aus $R$ nochmals
   die Attribute, die nur in $R$ enthalten sind und nicht in $S$. Dann ziehen
   wird diese Attribute, die wir nicht wollen, von der Gesamtheit ab. Wir
   erhalten dadurch die gewollten Tupel.

So koennen wir beispielsweise alle jene Studenten auswaehlen, die alle
vierstuendigen Vorlesungen hoeren:

```
        -
      /  \
     /    \
    /      \
   /        \
  |     Pi_MatrNr
Pi_MatrNr |
  |       -
hoeren  /   \
       X     \
     /   \    \
    /     \  hoeren
   /       \
Pi_MatrNr Pi_VorlNr
   |         |
hoeren    Sigma_SWS=4
             |
		 Vorlesungen
```

Also, selektiere alle Vorlesungen mit 4 SWS, dann kreuze mit allen
Matrikelnummern um die "Traumwelt" zu erhalten. Ziehe von dieser Traumwelt die
Realitaet ab. Dann erhaelt man die Matrikelnummern aller Studenten die nicht
alle Vorlesungen mit 4 SWS hoeren (nach Projektion von MatNr). Ziehe also von
allen hoerenden Studenten jene ab. Dadurch ergibt sich eben die Menge aller
Studenten die schon alle diese Vorlesungen hoeren.

Oder, mit Division:

$hoeren \div \Pi_{VorlNr}(\sigma_{SWS='4'}Vorlesungen)$

## Relationenkalkuel

In der relationalen Algebra beschreibt man Schritt fuer Schritt, wie die
gewuenschten Relationen erschaffen werden koennen. Man gibt also explizit an,
welche Operationen durchgefuehrt werden, und jede Operation hat eine neue
(Teil-)Relation zur Folge. Sie ist also gewissermassen eine *prozedurale*
Sprache.

Das Relationenkalkuel, von welchem es die Variante des Tupel- und des
Domaenenkalkuels gibt, ist nun eine *deklarative* Sprache. Das bedeutet, es wird
nicht explizit beschrieben, wie die Zielrelation erschaffen wird. Eher wird die
Relation beschrieben, durch praedikatenlogische Ausdruecke.

### Tupelalkuel

Das Tupelkalkuel lehnt sehr stark an die Praedikatenlogik an. Generell hat ein
Ausdruck im Tupelkalkuel immer die folgende Form: $$\{t \,|\, P(t)\}$$. Hierbei
nennt man $t$ die *Tupelvariable* des Ausdrucks, und $P(t)$ ein Praedikat. So
sind in dieser Menge also alle Tupel $t$ enthalten, fuer die das Praedikat
$P(t)$ erfuellt ist. Ein Beispiel waere die Anfrage "Finde alle Professoren mit
dem Namen Hansi": $$\{p \,|\, p \in \text{Professoren} \land p.\text{Name} =
'\text{Hansi}'\}$$.

#### Aufbau neuer Relationen

Was uns in der relationalen Algebra durch Joins und Projektionen moeglich war,
ist im Tupelkalkuel auch moeglich: neue Relationen zu erschaffen. Hierbei sei
allgemein angemerkt, dass das Tupelkalkuel die selbe Maechtigkeit wie die
relationale Algebra hat. Fuer diese Operation gibt man nun links vom Strich
nicht eine Tupelvariable an, sondern einen Tupelkonstruktor $[...]$. In diesen
kommen dann alle (qualifizierten) *Attribute*, welche in der neuen Relation
enthalten sein sollen. Eine solche Anfrage hat dann allgemein die Struktur:

$$\{[t_1.A_1,...,t_n.A_n] \,|\, P(t_1,...,t_n)\}$$

Hierbei sind im Tupelkonstruktur die einzelnen Tupel $t_1,...,t_n$ aufgelistet,
dessen Attribute $t_1.A_1,...,t_n.A_n$ dann die neue Relation aufbauen. Hierbei
kann $t_i = t_j$ fuer $i \neq j$, also man kann auch mehrere Attribute aus einem
Tupel entnehmen. Die Attribute $A_1,...,A_n$ muessen jedoch natuerlich in den
Schemata der Relationen von $t_1,...,t_n$ enthalten sein. Das Praedikat $P$ hat
dann alle diese Tupelvariablen $t_1,...,t_n$ als freie Variablen.

Moechte man nun zum Beispiel den Join von Studenten und der *hoeren* Relation
beschreiben, so geht das eben auch mit dieser Notation, da ein Join ja eine neue
Relation mit neuem Schema ergibt:

$\{[s.\text{MatrNr}, s.\text{Name}, s.\text{SWS}, h.\text{VorlNr}] \,|\, s \in
Studenten \land h \in hoeren \land s.\text{MatrNr}=h.\text{MatrNr}\}$$$.

##### Tupelkonstruktor

Allgemein hat ein Tupelkonstruktor die Syntax
$[\text{attribut}: \text{wert}]$. Es wird also der Name jeden Attributs
angegeben, sodass man es spaeter mit der $.A$ Syntax referenzieren kann, sowie
auch der Wert des Attributs.

Oft schreibt man aber, wie oben weiter, einfach $[a_1,...,a_n]$.gibt also
Variablen an, die fuer bestimmte Attributwerte stehen. Das is jedoch nur eine
verkuerzte Schreibweise fuer $[a_1: a_1, ..., a_n: a_n]$. Man gibt also den
Attributen sozusagen implizit denselben Namen wie die Variable. Ist die Variable
selbst ein Attribut, z.B. $t.A_1$, dann wird hier $A_1$ als Attributname
uebernommen.

Die verallgemeinerte Form ist aber nuetzlich, wenn Tupel erstellen muss, wobei
manche Attribute explizite, konstante Werte enthalten sollen. Nehmen wir
beispielsweise einen outer-join, beispielsweise den links-auesseren. Hierbei
wird dem natuerlichen Join noch jedes Tupel aus der linken Relation ohne
Join-Partner hinzugefuegt, wobei die Attribute der rechten Relation den Wert
`null` erhalten. Wir wollen nun diese Operation im Tupelkalkuel ausdruecken. Wir
werden hierbei aber lieber den allgemeinen Join $\bowtie_\theta$ beachten.

Zuerst sollte man merken, dass man hier zwei verschiedene "Aufrufe" zum
Tupelkonstruktor benoetigt. Einmal, fuer die allgemein-verbundenen Tupel und
einmal, fuer die geretten Tupel der linken Relation. Da wir immer nur einen
Ausdruck links vom Strich haben koennen, koennen wir diese zwei Aufrufe nicht in
einer einzigen Menge berechnen, zumindest nicht gleich. Wir muessen also mit
einer *Vereinigung* arbeiten.

Seien nun also $R = \{[a_1,...,a_n]\}, S = \{[b_1,...,b_m]\}$ zwei
Relationen. Wir wollen $R \bowtie_{a_1=b_1} S$ beschreiben. Dann ist zunachst
der allgemeine Verbund so definiert:

$$\{[r.a_1,...,r.a_n,s.b_1,...,s.b_m] \,|\, r \in R \land s \in S \land
r.a_1=s.b_1\}$$

Dann die schwierigere Beschreibung fuer die restlichen Tupel:

$$\{[r.a_1,...,r.a_n,b_1: null,...,b_n:null] \,|\, r \in R \land \not\exists s
\in S(r.a_1 = s.b_1)\}$$

Fuer diesen zweiten Ausdruck haben wir also alle Tupel gefunden, fuer die kein
Join-Partner existiert. Dann haben wir die Werte aus dem Tupel aus $R$ (der
linken Seite) uebernommen, und die restlichen Attribute, die normalerweise die
Attribute des Join-Partners waeren, mit $null$ bestimm.

Das Endergebnis des Links-Aeusseren-Joins folgt dann aus der Vereinigung dieser
beiden Teilausdruecke bzw. Teilmengen.

#### Syntax

Wie man schon in den Beispielen oben sah ist die Syntax des Tupelkalkuels stark
auf der Praedikatenlogik aufgebaut. Sie erweitert sie aber um den Operator
$\in$, um auszudruecken, dass ein Tupel in einer Relation enthalten sein soll
(normalerweise muesste man ein passendes Praedikat definieren), sowie um weitere
arithmetische Vergleiche als nur $=$. Erlaubt sind also:

* Logische Operatoren: $\land,\lor,\rightarrow,\neg$
* Quantoren:
  + "fuer alle": $\land$
  + "es existiert": $\exists$
* Arithmetische Vergleichsoperatoren: $<,\leq,=,\neq,\geq,>$
* Konstanten: $a,b,c$
* Den Operator $\in$

#### Sichere Ausdruecke

Es kann in der Praedikatenlogik Ausdruecke geben, die eine unendliche Menge von
Tupeln kreeirt. Beispielsweise der Ausdruck $\{x \,|\, true\}$ oder $\{t \,|\,
\neg(t \in \text{Professoren})\}$ produziert eine unendliche Menge an Tupeln,
welche also niemals in einer Datenbank gespeichert oder verarbeitet werden
koennten. Daher muessen wir den Begriff von *sicheren Ausdruecken* einfuehren.

Dafuer benoetigen wir den Begriff *Domaene*. Die Domaene, auch *Universum
genannt, ist die Menge aller Attributwerte. Wenn man einen Allquantor $\forall
x$ schreibt, muss dieser ja ueber irgendeine Menge von Werten iterieren. Diese
Menge ist also genau die Domaene der Formel. Man beachte dass diese Menge nur
die Attribut*werte* enthaelt, und noch nicht vollkommene Tupel. Schreibt man
also $a \in A$, so meint man, es existiert ein Tupel $a := (a_1,...,a_n)$, sodass
jeder Wert in diesem Tupel innerhalb der Domaene ist, und das Tupel als ganzes
dem Schema der Relation $A$ entspricht. Auch alle Konstanten, die z.B. fuer
Vergleiche verwendet werden, muessen aus dieser Attributmenge stammen.

Eine Formel ist nun sicher, wenn das Ergebnis der Formel immer in der Domaene
der Formel ist. Da die Domaene endlich gewaehlt wird, ist das Ergebnis auch
endlich, also sicher.

#### Rezept

Man beachte, dass es im Tupelkalkuel zwei Moeglichkeiten gibt, um zu beschreiben
dass "wenn fuer alle $x$ $F$, dann $G$". Die erste Moeglichkeit ist es, die
Implikation zu verwenden: $\forall x F \Rightarrow G$. Die zweite Moeglichkeit
ist es, mit der Definition des Allquantors mit Existenzquantoren zu arbeiten:
wenn fuer alle $x$ F gilt, und dann G, dann gibt es auch keine Situation wo $F$
gilt und $G$ nicht. So kann es leichter sein, mit Existenzquantoren zu arbeiten
und zu beschreiben, dass $\not\exists x: F \land \neg G$.

Ein Beispiel waere: "Finde alle Studenten, die alle Vorlesungen von Sokrates
gehoert haben". Dann waeren nach obiger Beschreibung die Moeglichkeiten:

1. Woertlich nach der Problemstellung:

$$\{s \,|\, s \in \text{Studenten} \land \exists p \in
\text{Professoren}(p.\text{Name}='\text{Sokrates}'\forall v \in
\text{Vorlesungen}((v.\text{gelesenVon}=p.\text{persNr} \Rightarrow \exists h
\in \text{hoeren}(h.\text{MatrNr}=s.\text{MatrNr}\land
v.\text{VorlNr}=h.\text{VorlNr}))))\}$$

2. Es existiert fuer einen solchen Studenten keine Vorlesungen, die von Sokrates
   gelesen wird und die er nicht hoert:

$$\{s \,|\, s \i \text{Studenten} \land \not\exists v \in
   \text{Vorlesungen}(\exists p \in
   \text{Professoren}(p.\text{Name}='\text{Sokrates}'
   v.\text{gelesenVon}=p.\text{PersNr}) \land \not\exists h
   \in \text{hoeren}(h.\text{VorlNr}=v.\text{VorlNr}\land
   h.\text{MatrNr}=s.\text{MatrNr}))\}$$
