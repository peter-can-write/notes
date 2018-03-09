# SQL

## Datentypen

Es gibt in Standard-SQL mehrere verschiedene Datentypen, um Daten nach einer
bestimmten Domaene zu charakterisieren. Man beachte aber dass die genauen
Datentypen und vorallem die exotischeren sehr stark vom Datenbanksystem (SQLite,
PostgreSQL, DB2 usw) abhaengen.

### Zahlen

Natuerlich gibt es Datentypen, um Zahlenwerte zu speichern. Der allgemeinste
dieser Datentypen ist `numeric(p,s)`. Dieser Datentyp nimmt als erstes Argument
$p$ (precision) die Gesamtanzahl gespeicherter Vor- und Nachkommastellen, und als
zweites Argument $s$ (scale) die Anzahl der Stellen von diesen $p$, welche als
Nachkommastellen interpretiert werden sollen. Man kann diesen Datentyp also
sozusagen als Festkommatyp sehen.

Es gibt dann oft bestimmte Spezialisierungen von `numeric`, die haeufig benutzte
Konfigurationen von `numeric` darstellen. Diese sind:

* `integer(p)`: Ganzzahlen mit Praezision `p` und keinem scale.

* `integer`: Ganzzahl mit Praezision 10.

* `smallint`: Ganzzahl mit Praezision 5.

* `bigint`: Ganzzahl mit Praezision 19.


* `boolean`: Fuer bool'sche Werte ($true/false$).


* `float(p)`: Floating-Point Wert mit Mantissenpraezision `p`.

* `real`: Floating-Point Wert mit Mantissenpraezision 7.

* `float`: Floating-Point Wert mit Mantissenpraezision 16.

### Zeichenketten

Zeichenketten koennen so wie Zahlen entweder mit fester Groesse abgespeichert
werden, oder mit variabler Groesse.

Fuer die feste Variante wird der Datentyp `character(n)` bzw. `char(n)`
verwendet, welcher also eine feste Groesse fuer Zeichenketten angibt. Enthaelt
der abgespeicherte String weniger Zeichen als festgelegt, wird er mit
Leerzeichen aufgefuellt.

Ist die genaue Groesse des Datentyps noch nicht vorzeitig bekannt, kann der
Datentyp `char varying(n)` bzw. `varchar(n)` verwendet werden. Dieser kann die
Groesse des Strings bis zur Maximalgroesse, welche durch das Argument $n$
angegeben werden muss, dynamisch festlegen.

### Zeitangaben

Der SQL-92 Standard legt mehrere Datentypen fuer Zeitangaben fest:

* `date`: Speichert Jahr, Monat und Tag.

* `time`: Speichert Stunde, Minute und Sekunde.

* `timestamp`: Speichert Jahr, Monat, Tag; Stunde, Minute und Sekunde.

### Binaerobjekte

Letztlich verfuegen fast alle Datenbanksysteme ueber Datentypen, die fuer grosse
binaere Daten geeignet sind. Solche Datentypen kann man verwenden, um vom
Datenbanksystem nicht interpretierbare Daten zu speichern
(z.B. Musikdateien). Dieser Datentyp heist dabei meist `blob`, fuer *binary
large object*. Es kann dann auch noch `clob`, also *character large object*,
oder sogar `xml` Typen geben.

### `null`

Es gibt in SQL so wie in Java den Wert `null`. `null` steht fuer einen
undefinierten Wert in einer Datenbank. Oft ist ein solcher Wert nuetzlich, wenn
ein Wert erst spaeter eingefuegt werden kann. Generell sollte man solche
Ausnahmen jedoch vermeiden.

`null` macht aus jedem Wert beliebigen Typs bei einer belibiegen Operation
wieder `null`. Man sagt auch, `null` *propagiert*. `null + 5` ist also `null`,
`3 / null` ist `null`.

Will man explizit ueberpruefen, ob ein Attributwert `null` ist, muss man `is
null` verwenden. Ein Vergleich mit `=` liefert naemlich den Wert `unknown`. SQL
hat naemlich eine dreiwertige Logik, mit den Werten `true`, `false` und eben
`unknown`. In einer Selektion werden nur Attribute ausgewaehlt, fuer die der
Vergleich zu `true` evaluiert, daher werden `null` Werte auch nie selektiert.

Es gibt nun auch eigene Regeln fuer logische Operationen in dieser dreiwertigen
Logik, anhand dieser Tafeln dargestellt:

|         | `not`   |
|:--------|:--------|
|`true`   |`false`  |
|`unknown`|`unknown`|
|`false`  |`true`   |


|  `and`  |  `true` |`unknown`|`false`|
|:--------|:--------|:--------|:------|
|  `true` |  `true` |`unknown`|`false`|
|`unknown`|`unknown`|`unknown`|`false`|
| `false` | `false` | `false` |`false`|

|   `or`  | `true` |`unknown`| `false` |
|:--------|:-------|:--------|:--------|
|  `true` | `true` | `true`  | `true`  |
|`unknown`| `true` |`unknown`|`unknown`|
| `false` | `true` |`unknown`| `false` |

Diese Definition sind eigentlich sehr intuitiv. Man sollte sich merken, dass
`unknown` nicht so propagiert wie `null`, also nicht jede logische Operation mit
`unknown` gibt wieder `unknown`. Man muss einfach daran denken, dass fuer die
Konjunktion `true` das neutrale Element ist und `false` das dominierende, und
fuer die Disjunktion `false` das neutrale und `true` das dominierende. `unknown`
hat somit also keine Sonderstellung.

## Vergleichsoperatoren

Es gibt in SQL die folgenden Vergleichsoperatoren, welche sowohl fuer skalare
Werte als auch Zeichenketten und Zeitangaben funktionieren:

* `=`: Gleichheit.
* `<`: Kleiner als.
* `>`: Groesser als.
* `<=`: Kleiner gleich.
* `>=`: Groesser gleich.
* `<>`: __Ungleich__ nach Standardidierung!
* `!=`: Ungleich, aber nicht standardisiert!

## Kommentare

In SQL kann man sowohl inline als auch Block-Kommentare
benutzten. Inline-Kommentare werden mit zwei Bindestrichen angefangen: `--
Kommentar`. Block Kommentare werden wie in C/C++/Java durch die Sequenz `/*
... */` angegeben.

## Schemadefinition

Die Schemadefintion (also das Anlegen einer Tabelle) wird in SQL immer mit dem
`create table` Befehl vorgenommen. Nach diesem Befehl folgt der Name der
Tabelle, sowie dann die genaue Schemadefinition:

```sql
create table <name> (
  <column_name> <type> <modifier>,
  <column_name> <type> <modifier>,
  ...
);
```

Ein Beispiel waere:

```sql
create table data (
  id integer primary key,
  name varchar(100)
);
```

Wie man sieht kommt immer zuerst der Name, dann der Datentyp und letzlich noch
bestimmte weitere Eigenschaften, die man dem Attribut geben kann. Hier haben wir
zum Beispiel festgelegt, dass das Attribut `id` der Primaerschluessel der
Relation sein wird. Andere solche *Modifier* koennen sein:

* `not null`
* `unique`
* `foreign key` (wird im Abschnitt zu Referentieller Integritaet besprochen)
* `check <constraint>`
* `default <value>`

Es sollen nun alle diese Modifier, bis auf `foreign key`, besprochen werden.

### `not null`

Mit `not null` kann dem Datenbanksystem gesagt werden, dass ein bestimmtes
Attribut (also eine Spalte) nie den Wert `null` annehmen darf. Erstellt man also
eine Tabelle mit `create table data(id integer not null)`, und will dann eine
`null` einfugen, dann wird vom Datenbanksystem eine Fehlermeldung ausgegeben.

Ein `primary key` Attribut ist auch immer implizit `not null`.

### `unique`

`unique` laesst das Datenbanksystem wissen, dass jeder konkrete Attributwert in
dieser Spalte nur einmal vorkommen darf. Das ist nuetzlich, wenn man noch
zusaetzliche Einschraenkungen auf die Tabelle machen moechte, ausser der
Festlegung des Primaerschluessels. Dieser ist im uebrigen implizit `unique`.

`unique` ist im uebrigen ein Modifier, der entweder inline nach einer
Spaltendeklaration, oder auch nachtraeglich der Schemadeklaration angefuegt
werden kann:

```sql
create table data(id integer unique);
-- oder
create table data(id integer, unique(id));
```

Die nachgestellte Variante ist grundsaetzlich die bessere sowie auch
flexiblerere, da man hierbei nicht nur ein Attribut angeben kann, sondern
viele. Es ist also vorallem die einzige Moeglichkeit eine *Menge von Attributen*
zusammen als `unique` zu erklaeren. Hat man beispielsweise die Attribute $A,B,C$
so wuerde ein `unique` Modifier nach jeder Deklaration im Schema jedes Attribut
*einzeln* unique machen. Das bedeutet, dass jeder Wert in den Spalten von
$A,B,C$ jeweils eindeutig sein muss. Moechte man jedoch ausdruecken, dass nur
jede Kombination der drei Attribute eindeutig sein muss, dann muss man das
nachtraeglich mit `unique(a, b, c)` ausdruecken. So kann es mehrmals den selben
Attributwert in der Spalte `a` geben, wenn jedoch zumindest `b` oder `c` anders
ist also das Tripel als ganzes eindeutig ist, nicht jedes Attribut.

### check

Ein `check` ist eine explizite Konsistenzbedingung fuer Attributwerte einer
bestimmten Spalte, welche bei Schemadefinition festgelegt werden kann. So kann
man beispielsweise sicherstellen, dass ein integer (z.B. das Alter einer Person)
immer innerhalb einer bestimmten Unter- und Obergrenze liegt (fuer das Alter
also z.B. $0$ bis $130$). Versucht man dann einen Wert einzufuegen, der gegen
diese check-constraint verstoesst, sollte das Datenbanksystem eine Fehlermeldung
ausgeben.

Ein solcher *`check`-constraint* kann hier ebenso wie `unique` entweder inline
neben dem betroffenen Attribut, oder out-of-line nachgestellt werden
(empfohlen). Man kann in einem solchem Check die Standard SQL-Syntax verwenden,
um Vergleiche durchzfuehren. Hierbei koennen auch andere Attribute referentiert
werden, um beispielsweise sicherzustellen, dass beim Einfuegen der Wert von
Attribut `a` immer kleiner ist als der von Attribut `b`:

`create table data(x int, y int, check(x < y));`

Allgemein ist die Syntax hierbei:

```sql
create table <name> (
  <attribute> <type> check (<condition>),
  ...
);
```

Weitere Bedingungen, die man uerberpruefen koennte:

* Dass `<attribute> between <x> and <y>`, z.B. `Semester between 1 and 13`, oder
* Dass `<attribute> in (<elements>)` z.B. `Rang in ('C2', 'C3', 'C4')`

Komplexere Checks koennen hierbei auch mit dem Schluesselwort `constraint`
benannt werden:

```sql
create table <name> (
  ...
  constraint <constraint_name> check (<condition>)
);
```

Das hat den Vorteil, dass man spaeter solche Checks wieder loeschen kann:

```sql
alter table <name> drop constraint <constraint_name>;
```

Eine weitere Moeglichkeite, komplexere Checks durchzufuehren ist mit
`Trigger`. Hierbei kann man SQL in bestimmten Situationen bestimmte Funtionen
aufrufen lassen, die man selbst definieren kann. Die Syntax ist hierbei relativ
verschieden von DBMS zu DBMS, aber ein Beispiel waere in DB2 Syntax:

```sql
create trigger <Name>
no cascade -- rekursiere nicht
<before|after|instead of> <insert|update|delete> of <Relation> on <Attribut>
referencing old as <Name fuer alten Wert>
            new as <Name fuer neuen Wert>

for each row
[when (<condition>)]
<action>;
```

So kann man beispielsweise einen Trigger fuer eine Datenbank eines Restaurants
erstellen, der, immer wenn der Mayonnaise-Bestand zu Ende zu gehen droht, eine
neue Mayonnaise-Lieferung in die Einkaufs-Datenbank (wie Einkaufs-Liste, nur
cooler) speichert.

http://www.postgresql.org/docs/9.1/static/sql-createtrigger.html

### default

Mit dem `default` Modifier kann man einem Attribut einen Default-Wert geben,
sodass man beim Einfuegen eines neuen Tupels keinen Wet fuer dieses Attribut
anzugeben braucht. Beispiel:

`create table data(x int, y int default 5);`

Dann kann man zwar wie herkoemmlich einfuegen:

`insert into data values(3, 7);`

Aber `y` eben auch weglassen:

`insert into data values(3); -- y dann gleich 5`

## Schemaveraenderungen

Mit dem `alter`-Befehl kann eine Reihe von Veraenderungen auf einer bereits
existenten Tabelle durchgefuehrt werden. Veraendert werden koennen dabei
Spaltennamen, Constraints und Datentypen. Es koennen auch neue Spalten
hinzugeuegt werden:

Sei `data` kreeirt mit dem Befehl: `create table data(x int, y char);`, dann:

Um eine neue Spalte `neue_spalte` als Varchar mit maximal 30
Zeichen *hinzuzufuegen*:

`alter table data add column neue_spalte varchar(30);`

Um eine Spalte zu *loeschen*:

`alter table data drop column neue_spalte;`

Um eine ganze Relation zu loeschen:

`drop table data;`

Das kann man auch nur machen, wenn es die Tabelle wirklich gibt, um eine
Fehlermeldung zu vermeiden, falls es sie nicht gibt:

`drop table if exists data;`

## Auswaehlen von Daten

Das wohl wichtigste an einer Datenbank ist es, Daten aus dieser zu
selektieren. Hierfuer wird in SQL der `select` Befehl verwendet. Dieser hat
allgemein die Struktur:

```sql
select A_1,...,A_n
from   R_1,...,R_k
where P;
```

Hierbei sind $A_1,\ldots,A_n$ die selektierten (bzw. eigentlich projezierten)
Attribute, $R_1,\ldots,R_k$ die Relationen, aus welchen die Attributwerte entnommen
und auf dessen Schemata gearbeitet wird, und $P$ das Selektionspraedikat wie
jenes wie fuer $\sigma$ aus der relationalen Algebra. Eine einfache Anfrage
waere es zum Beipsiel, alle Studenten mit Namen *Fichte* zu finden:

```sql
select MatrNr
from Studenten
where Name = 'Fichte';
```

Hierbei sei angemerkt, dass in der Attributangabe auch `*` als *Wildcard*
angegeben werden kann, wobei dann das ganze Schema projeziert wird, also alle
Attribute ausgegeben werden. Ebenso kann Attribute auch umbenennen, was vor
Allem bei generischen Aggregationsfunktionsnamen (siehe unten) hilfreich sein
kann. Diese Umbennung macht man dabei mit einem `as` Statement, wobei die Syntax
`<Attribut> as <Name>` ist. Das `as` kann hierbei auch weggelassen werden:

```sql
select MatrNr as M, Name Bla -- mit/ohne 'as'
from Studenten
where Name = 'Fichte';
```

### Sortieren von Daten

Eine erste Modifikation die wir an den selektierten Daten vornehmen koennen,
ist, die Reihenfolge, in welcher sie in der Ergebnisrelation aufgelistet, zu
veraendern -- die Daten also zu *sortieren*. Eine Sortierung der Daten kann mit
dem `order by` Befehl realisiert werden, z.B. `order by Name`.

Dieser Befehl sollte immer __am Ende__ einer Anfrage angefuegt werden. Hierbei
ist es moeglich entweder aufsteigend (`asc`) oder absteigend (`desc`) zu
sortieren. Diese Modifier `asc|desc` werden dabei immer nach dem Attribut
angefuegt, also oben entweder `order by Name asc` oder `order by Name
desc`. `asc` ist dabei der Default-Wert, wenn man `asc|desc` weglaesst.

Man kann dann auch nach mehreren Attributen sortieren, wobei die Reihenfolge der
Attribute, welche man hierbei auflistet, auch die Reihenfolge bestimmt, in
welcher sortiert wird. Schreibt man beispielsweise `order by A asc, B desc`,
wird zuerst aufsteigend nach dem Wert des Attributs `A` sortiert. Tupel die nach
dieser Ordnung gleich sind, werden dann absteigend nach dem Wert des Attributs
`B` sortiert. Wie man sieht wird `asc|desc` auch wirklich *nach dem Attribut*
angegeben, damit man fuer jedes Attribut eine andere Ordnung festlegen kann.

### Elimination von Duplikaten

Duplikate werden in SQL nicht automatisch aus einer (Ergebnis-)Relation
eliminiert, da dies zu ineffizient waere. Dies steht im Kontrast du den
Operatoren in der relationalen Algebra, da diese auf mathematischen Mengen
operiert, wo Duplikatelimination quasi automatisch und kostenfrei
passiert. Diese Anforderung nach Duplikatelimination muss in SQL also explizit
mit dem Schluesselwort `distinct` gemacht werden. Diese Schluesselwort wird
dabei direkt nach dem `select` Schluesselwort angegeben, also `select distinct
...`.

Hat man beispielsweise folgende Auspraegung einer Relation $R$:

|A|B|
|-|-|
|a|1|
|b|1|

Dann wuerde folgende Anfrage Duplikate (zweimal die $1$) in ihrem Ergebnis
enthalten:

```sql
select B from R;
```

aber folgende nicht

```sql
select distinct B from R;
```

### Mehrere Relationen

Moechte man aus mehr als einer Relation Attributwerte selektieren, oder
zumindest Referenzen auf Attribute anderer Relationen machen, gibt man hierfuer
jeweils nach einem Komma noch weitere Relation nach dem `from` Schluesselwort
an:

```sql
select ...
from R, S, T, ...
where ...;
```

Hierbei wird zunaechst immer das *Kreuzprodukt* $\times$ aus allen angegebenen
Relationen bestimmt. Dieses kann dann natuerlich durch die `where`-Klausel
eingeschraenkt werden. Der Anfrageoptimierer des Datenbanksysstems gibt
letztendlich natuerlich auch sein bestes, das Kreuzprodukt zu vermeiden. Aber im
Allgemeinen arbeitet man also immer mit dem Kreuzprodukt der Relationen. Gibt
man auch keine Bedingung, also `where`-Klausel an (sodass das Selektionpraedikt
implizit `true` ist), wird hierbei auch wirklich das Kreuzprodukt ausgegeben:

```sql
select *
from A, B;
```

ist dann aequivalent $A \times B$.

#### Qualifizierung

Natuerlich kann es sein, dass mehrere Relationen ein oder mehr Attribute mit
demselben Namen haben. Dann muss es in einer Anfrage, die ueber mehreren
solcher Relation operiert, eine Moeglichkeit geben, diese gleichbenannten
Attribute voneinander zu unterscheiden.

Sind die Relationen noch alle unterscheidbar, also wenn dieselbe Relation nicht
zweimal in der `from`-Klausel angegeben wird, kann man noch mit Qualifizierung
wie in der relationalen Algebra oder dem Relationenkalkuel arbeiten, also die
Attribute mit `Relation.Attribut` angeben. Beispielsweise so:

```sql
select *
from Assistenten, Professoren
where Assistenten.PersNr < Professoren.PersNr;
```

Ohne Qualifizierung wuerde hier nicht bekannt sein, welche Personalnummer wann
gemeint ist, so ist das noch moeglich. Mit dieser Methode ist es im Uebrigen
auch moeglich, die Wildcard `*` nur fuer eine bestimmte Relation zu
verwenden. `R.*` steht also fuer alle Attribute der Relation `R`.

Was aber, wenn wir dieselbe Relation zweimal oder oefter im Kreuzprodukt haben?
Dann hilft `Relation.Attribut` auch nicht weiter.

In einem solchen Fall muessen wir wie im Tupelkalkuel *Tupelvariablen*
einfuehren. Das macht man, indem man nach jeder (oder einer beliebigen) Relation in
der `from`-Klausel eine Variable anfuegt (vor dem Komma), die der Relation in
der weiteren Anfrage zur Identifikation dient. Dabei kann man zwischen dem
Relationennamen und dem Namen der Tupelvariablen noch ein `as` schreiben, wenn
man das moechte:

```sql
select *
from Studenten s1, Studenten as s2,
where s1.MatrNr > s2.MatrNr;
```

Diese Anfrage, die zweimal auf der selben Relation *Studenten* operiert,
selektiert beispielsweise alle Studenten die eine hoehere Matrikelnummer haben,
als irgendein anderer Student. Wie man sieht, kann man die Tupelvariable
entweder mit oder ohne `as` angeben. Man bemerke, dass das Einfuehren der
Tupelvariable in der relationalen Algebra genau der Umbennung der Relation in
die Tupelvariable entspricht.

### Gruppierung und Aggregation

Wie in der relationalen Algebra sind auch in SQL Aggregatfunktionen und
Gruppierung moeglich. Hierbei stehen die selben Funktionen wie in der
relationalen Algebra zur Verfuegung:

* `avg`: Berechnet den Durschnitt (average) einer Gruppe von Werten.
* `sum`: Berechnet die Summe einer Gruppe von Werten.
* `min`: Selektiert das Minimum aller Werte einer Gruppe.
* `max`: Selektiert das Maximum aller Werte einer Gruppe.
* `count`: Zaehlt die Anzahl an Zeilen einer Gruppe.

Was versteht man hierbei aber unter einer *Gruppe*? Hierfuer muss der `group
by`-Befehl eingefuehrt werden. Mit diesem Befehl kann man Tupel einer Relation
*gruppieren*. Konkret bedeutet das, dass man eine bestimmte Teilmenge der Tupel auf
nur ein einziges Tupel reduziert bzw. komprimiert, welches also Repraesentant der
ganzen Gruppe ist. Fuegt man einem SQL-Ausdruck nun also `group by <Attribut>`
an, so wird zunaechst die Relation in "Aequivalenzklassen" bezueglich diesen
Attributs partitioniert. Dann hat man also *Gruppen*, wobei fuer alle Tupel in
der Gruppe der Attributwert des besonderen Attributs gleich ist. Beispielsweise
kann Studenten nach Semestern gruppieren:

```sql
select Semester
from Studenten
group by Semester;
```

Auf diesen Gruppen kann man dann Aggregatoperationen ausfuehren, um andere
Attributwerte des Schemas gruppenweise zu einem Wert zu komprimieren, der dann
dem Gruppenrepresentanten in der Ergebnisrelation beigefuegt werden
kann. Beispielsweise kann man zaehlen, wieviele Tupel es pro Gruppe gibt:

```sql
select Semester, count(*) as Anzahl
from Studenten
group by Semester;
```

Die Ergebnisrelation haette also zwei Spalten `Semester` und `count(*)`, wobei
letztes hier noch zu `Anzahl` umbenannt wird. Unten sind Beispiele fuer die
anderen Aggregatfunktionen gegeben.

Es sei hier gesagt, dass wenn man gar nicht gruppiert, die ganze Relation als
Gruppe betrachtet wird. Also dann werden Aggregatfunktionen auf allen Tupeln
ausgefuehrt. Beispielsweise zaehlt `select count(*) from R` die Zeilen der
Relation `R`.

Was man hier unbedingt anmerken sollte, ist, dass wenn man einmal gruppiert, man
dann *nur gruppierte Werte auch selektieren kann*. Wir erinnern uns: Die
Gruppierung fuehrt zu einer Komprimierung jeder Gruppe auf nur ein Tupel. Wenn
man also Studenten nach ihrem Semester gruppieren moechte, sodass man fuer jede
unterscheidbare Semesteranzahl nur ein Tupel hat, dann ist es doch unsinning,
auch noch den Namen jeden Studenten mitzuselektieren:

```sql
select Name, Semester
from Studenten
group by Semester;
```

Dies kann natuerlich nicht moeglich sein, da der Name der vielen tausenden
Studenten nicht eindeutig ist! Daher gilt also:

__Man darf nur Attribute selektieren, nach denen man gruppiert!__

Wenn man also einmal `group by` macht, darf man nur mehr jene Attribute
selektieren, nach denen man gruppiert, oder natuerlich Ergebnisse von
Aggregatfunktionen. Wenn man nach mehreren Attributen gruppiert, bedeutet das,
dass fuer jede Gruppe also genau diese Menge an Attributen einheitlich sein muss
bzw. dass die Relation genau nach dieser Menge an Attributen partitioniert
wird. Wenn man also nach der gesamten Relation gruppiert erhaelt man natuerlich
wieder die ganze Relation, da man ja keinerlei Verfeinerung durchgefuehrt
haette. Ebenso wenn man nach dem Primaerschluessel oder irgendeiner Gruppe von
Attributen die als `unique` deklariert wurden, gruppiert, wuerde keine echte
bzw. effektive Gruppierung durchgefuehrt (da diese Attribute ja nur einmal in
der gesamten Relation vorkommen).

#### `having`

`having` ist wie `where` fuer Gruppen. Moechte man also irgendwelche Bedingungen
fuer Gruppen ueberpruefen, ist `having` dazu geeignet. Vor allem nach
Aggregatfunktionen ist das sehr nuetzlich. Selektieren wir beispielsweise alle
Studenten die mehr als zwei Vorlesungen hoeren:

```sql
select Name
from Studenten s, hoeren h
where s.MatrNr = h.MatrNr
group by s.Name
having count(*) > 2;
```

`where` ist dabei nuetzlich, um *noch vor der Gruppierung* Tupel zu
selektieren. Beispielsweise wollen wir die obige Anfrage nun nur fuer Studenten
in Semestern niederiger als $5$ betrachten:

```sql
select Name
from Studenten s, hoeren h
where
  s.MatrNr = h.MatrNr and
  s.Semester < 5
group by s.Name
having count(*) > 2;
```

#### `count`

Nochmal zu `count`. Diese Funktion kann eigentlich mehr als nur einen `*` als
Argument nehmen, auch wenn das die haeufigste Einsatzweise ist. Sie kann
naemlich einen beliebigen SQL-Ausdruck als Argument nehmen. Man mag sich fragen,
wieso es nuetzlich waere, ein bestimmtes Attribut zu zaehlen, wenn doch jedes
Attribut offensichtlich in jeder Zeile einen Wert hat. Nun, dazu muss man
wissen, dass `null` Werte nicht gezaehlt werden.

Man betrachte folgende Auspraegung einer Relation $R = \{[A,B]\}$:

| A  |B|
|----|-|
|  a |1|
|null|2|
|null|3|

Dann beobachte man die Ergebnisse folgender drei Queries:

1. `select count(*) from R`: 3
2. `select count(B) from R`: 3
3. `select count(A) from R`: 1

Man sieht, das Argument hat also sehr wohl eine Wirkung. Auch sieht man, dass
`null`-Werte nicht gezaehlt werden. Auch sei angemerkt, dass es oft nuetzlich
sein kann, vor dem Zaehlen eine Duplikatelimination bei den Attributen zu
machen. Das geht dann ganz einfach mit `count(distinct <Attribut>)`.

#### `avg`

Mit `avg` koennen wir den Durchschnitt von (skalaren) Werten
berechnen. Beispielsweise, das Durchschnittssemester aller Studenten:

```sql
select avg(Semester)
from Studenten;
```

#### `min/max`

Die `min` und `max` Funktionen selektieren aus einer Gruppe den minimalen
bzw. maximalen Wert des Attributs, das ihnen als Argument uebergeben wird. Was
ist das hoechste und niedrigste Semester der Studenten?

```sql
select max(Semester), min(Semester)
from Studenten;
```

Man beachte, dass wenn man hier die Namen moechte, man eine verschachtelte
Unteranfrage verwenden muss:

```sql
select s1.Name, s1.Semester, s2.Name, s2.Semester
from Studenten s1, Studenten s2
where
  s1.Semester = (select min(Semester) from Studenten) and
  s2.Semester = (select max(Semester) from Stduenten);
```

oder ein Vereinigung verwenden muss:

```sql
select Name, Semester
from Studenten
where Semester = (select min(Semester) from Studenten)
union all
select Name, Semester
from Studenten where Semester = (select max(Semester) from Studenten);
```

## Geschachtelte Anfragen

Oftmals ist es notwendig, innerhalb einer Anfrage noch eine weitere Anfrage zu
machen. Beispielsweise, wenn man den Wert eines Attributs mit dem aggregierten
Wert einer anderen (oder derselben) Relation vergleichen moechte. Aber vor Allem
auch in Kombination mit Schluesselwoertern wie `exists` oder `in`.

Allgemein unterscheiden wir hierbei zwischen Anfragen, die nur einen Wert (ein
Attribut) zurueckgeben, und Anfragen die mehr als nur einen Wert, also ein
ganzes Tupel oder viele zurueckgeben. Dann unterscheiden wir noch zwischen
*korrelierten* und *unkorellierten* Anfragen. Jegliche Unteranfrage muss in SQL
immer in Klammern geschrieben werden.

### Einzelne Werte

Unteranfragen, die nur ein Attribut eines einzigen Tupel zurueckgeben, haben die
nette Eigenschaft, dass sie automatisch entschachtelt werden und somit als Wert
gelten. Dadurch kann man, wie oben im Beispiel angedeutet, Werte einer Anfrage
mit dem aggregierten Wert einer anderen Anfrage vergleichen. Das ist vor Allem
in der `select`-Klausel und in Vergleichen in der `where`-Klausel
nuetzlich. Suchen wir beispielsweise alle Studenten, die nicht im niedrigsten
Semester sind:

```sql
select Name
from Studenten
where Semester != (select min(Semester) from Studenten);
```

Wie man sieht, werden Unteranfragen also immer in Klammern angegeben, und da es
nur einen minimalen Semester-Wert gibt, wird in dieser Unteranfrage auch nur ein
Wert zurueckgegeben, weswegen die Unteranfrage die Semantik eines skalaren Werts
hat (wie eine Konstante).

Mit Unteranfragen ist es so auch moeglich, zusaetzliche Attribute zu den bereits
selektierten einer Klausel hinzuzufuegen. Hierbei gibt man die Unteranfrage in
die `select`-Klausel, neben den anderen Attributen, wobei man aber wiederum
darauf achten muss, dass wirklich nur ein Wert ausgegeben wird. Ansonsten wird
vom System eine Fehlermeldung ausgegeben. So koennten wir beispielsweise ohne
`group-by` die Summe der Semesterwochenstunden von Professoren ermitteln und
deren Namen anfuegen:

```sql
select Name, (select sum(SWS) from Vorlesungen where PersNr = gelesenVon)
from Professoren;
```

Hierbei kann der Summe dann noch entweder in der Klausel oder ausserhalb ein
neuer Name gegeben werden:

```sql
select
  Name,
  (select sum(SWS) as Foo from Vorlesungen where PersNr = gelesenVon) as Bar
from Professoren;
```

Wobei hier der aussere Name `Bar` Praezedenz haette.

#### Korelliertheit

Nach der obigen Anfrage kann man auch schon auf die Eigenschaft der
*Korelliertheit* von Unteranfragen zu sprechen kommen. Die eben besprochene
Unteranfrage zur Ermittlung der Summe der Semesterwochenstunden (die
Lehrbelastung) von Professoren nennt man *korelliert*, weil sie innerhalb der
Unteranfrage Referenzen auf Relationen ausserhalb ihres "Scopes" macht. Es ist
ja nicht selbstverstaendlich, dass alle Symbole ausserhalb der Unteranfrage
auch innerhalb sichtbar sind. In SQL ist das aber moeglich, und auch in die
andere Richtung (wie man weiter unten bei der Erstellung temporaerer Relationen
sieht). Die Klammern der Unteranfragen sind also sozusagen transparent.

Ein Beispiel fuer eine unkorellierte Unteranfrage wurde schon oben gegeben, als
die Semesterzahl der Studenten mit dem Minimum verglichen wurde. Ein weiteres
triviales Beispiel waere:

```sql
select Name, (select max(SWS) from Vorlesungen)
from Studenten;
```

Hier bezieht sich die Unteranfrage sogar auf eine voellig andere Relation, also
gibt es hier natuerlich keinerlei Korreliertheit. Eine wichtige Eigenschaft
einer solchen unkorellierten Anfrage ist, dass sie vom DBMS nur ein einziges Mal
ausgewertet werden muss. Hingegen muss eine korellierte Anfrage wie in einem
nested-loop fuer jedes neue Tupel der Relation, auf welche sich die Unteranfrage
bezieht, neu ausgewertet werden. Dies ist natuerlich ein grosses
Effizienzhindernis und daher sollten korrellierte Anfragen wenn moeglich
vermieden werden.

### Viele Werte

Wenn die Unteranfrage die Eigenschaft hat, mehr als nur einen Wert
zurueckzugeben, also ein ganzes Tupel oder viele, dann gilt die Unteranfrage
nicht mehr als skalarer Wert. Mit dem Tupel oder den vielen Tupeln das hierbei
zurueckgegebn wird, kann dann in verschiedenen Situationen verschiedenes
durchgefuehrt werden.

In der `from` Klausel kann eine solche Unteranfrage dazu benutzt werden,
temporaere Relationen zu erstellen. Beispielsweise wollen wir eine temporaere
Relation kreeiren, die die Anzahl der Vorlesungen fuer jeden Studenten
beinhaltet. Dann kann man die Studenten ermitteln, die mehr als zwei Vorlesungen
lesen:

```sql
select temp.MatrNr, temp.Name, temp.Anzahl
from (
  select s.MatrNr, s.Name, count(*) as Anzahl
  from Studenten s, hoeren h
  where s.MatrNr = h.MatrNr
  group by s.MatrNr, s.Name
) as temp
where temp.Anzahl > 2;
```

Dies ginge hier natuerlich auch mit einer `having`-Klausel, aber die
Maechtigkeit der Unteranfragen-Syntax wird hier gut dargestellt. Es gibt hier
die Regel, dass eine Unteranfrage in der `from`-Klausel __immer einen Alias__
haben muss, also umbennant werden muss. Oben wird sie in `temp`
umbenannt. Spaeter wird noch das `with`-Konstrukt eingefuehrt, womit solche
temporaeren Relationen auch ausserhalb der eigentlichen Anfrage erstellt werden
koennen.

Wo Unteranfragen mit mehreren Tupeln besonders wichtig und hilfreich sind, ist
in der `where`-Klausel, wo sie im Zusammenhang mit `exists`, `in` sowie weiteren
Schluesselwortertn verwendet werden koennen.

#### `exists`

`exists` ist hierbei ein Schluesselwort, dass `true`, zurueckgibt, wenn in einer
Unteranfrage mindestens ein Tupel enthalten ist. Es ist also der Existenzquantor
des Tupelkalkuels bzw. der Praedikatenlogik. Beispielsweise wollen wir alle
Assistenten finden, die aelter als ein Professor sind, von dem sie angestellt
werden (wir nehmen dazu an, es gaebe ein Attribut `Alter` fuer beide Relationen):

```sql
select *
from Assistenten a
where exists (
  select *
  from Professoren p
  where p.PersNr = a.Boss
  and a.Alter > p.Alter
);
```

Spaeter wird noch erklaert, wie dieses Schlueselwort zur Nachahmung des
Allquantors benutzt werden kann. Was wir hier auch merken, ist das manche
Unteranfragen entschachtelt werden koennen:


```sql
select distinct a.*
from Assistenten a, Professoren p
where a.Boss = p.PersNr and a.Alter > p.Alter;
```

Das waere also eine weniger komplexe Anfrage die das DBMS sechneller
durchfuehren koennte. Das ist aber natuerlich nicht immer moeglich.

#### `in`

Das Schluesselwort `in` ist prinzipiell syntaktischer Zucker fuer eine `exists`
Anfrage, kann aber bei vielen DBMS (PostgreSQL, MySQL, MSFT SQLServer; nicht
SQLite) dazu verwendet werden, zu ueberpruefen, ob ein Attribut oder ein Tupel
unter den selektierten einer Unteranfrage enthalten ist. Suchen wir alle
Studenten, die eine Vorlesung hoeren:

```sql
select s.Name
from Studenten s
where s.MatrNr in (
  select h.MatrNr
  from hoeren h
);
```

Aber wie gesagt geht das auch mit `exists`:

```sql
select s.Name
from Studenten s
where exists (
  select h.VorlNr
  from hoeren h
  where h.MatrNr = s.MatrNr
);
```

#### `all`/`any`

`all` und `any` sind zusaetzliche Operationen, die wie `in` eigentlich schon
durch `exists` ausgedrueckt werden koennten. Sie sind jedoch sehr nuetzlich,
wenn man einen arithmetischen Vergleich zwischen einem Attribut und vielen
anderen Attributen einer Unteranfrage machen will. Die Syntax ist dabei wie
folgt:

```sql
select ...
from R
where R.A =,<,>,<=,>=,!= all/any(...);
```

`all` gibt fuer eine Unteranfrage dann `true` zurueck, wenn der Vergleich fuer
alle Attribute der Unteranfrage wahr ist. `any` gibt dannn `true` zurueck, wenn
der Vergleich fuer zumindest ein Attribut erfuellt ist. Wir wollen
beispielsweise alle Studenten finden, die nicht alleine in ihrem Semester
studieren:

```sql
select s1.Name
from Studenten s1
where s1.Semester = any (
  select s2.Semester
  from Studenten s2
  where s2.MatrNr != s1.MatrNr
);
```

Wie gesagt geht das aber auch mit `exists`:

```sql
select s1.Name
from Studenten s1
where exists (
  select *
  from Studenten s2
  where s1.MatrNr != s2.MatrNr and
        s1.Semester = s2.Semester
);
```

`all` koennte nuetzlich sein, um zu bestimmen, ob die Semesteranzahl eines
Studenten groesser ist als alle andern:

```sql
select Name
from Studenten
where Semester >= all(select Semester from Studenten);
```

Hierbei kann aber vorallem Aggregierung nuetzlich sein, um eine Entschachtelung
zu erreichen:

```sql
select Name
from Studenten
where Semester = (select max(Semester) from Studenten);
```

### Mengenoperationen

Wie in der relationalen Algebra kann man auf den Relationen auch
Mengenoperationen durchfuehren. Hierfuer gibt es die Befehle `union` bzw. `union
all`, `except` bzw. `minus`, `intersect`, sowie das bereits vorgestellte
Schluesselwort `in`. Wie auch in der relationalen Algebra muss fuer die
Mengenoperationen immer __das Schema der beiden Relationen gleich sein__.

Mit `union` kann man Relationen gleichen Schemas vereinigen. Wir wollen
beispielsweise die Namen aller Studenten *und* Professoren:

```sql
(select Name from Professoren)
union
(select Name from Studenten);
```

`union` nimmt hierbei immer implizit eine Duplikatelimination vor. Da es aber
natuerlich viele Menschen mit einem bestimmten Namen geben kann, wollen wir das
verhindern. Wir wollen SQL also sagen, dass wir `all`e Duplikate
wollen. Hierfuer gibt es dann den `union all` Befehl, der eben Duplikte erhaelt.

`intersect` fuehrt die herkoemmliche Schnittoperation der Mengenlehre durch. Wir
wollen z.B. die gemeinsamen Namen von Studenten und Professoren finden:

```sql
(select Name from Professoren)
intersect
(select Name from Studenten);
```

Mit `except` berechnen wir dann die Mengendifferenz aus zwei Relationen. Wir
wollen z.B. alle Studenten, die nicht im ersten oder letzten Semester sind:

```sql
(select Name from Studenten)
except
(select Name from Studenten
 where Semester = (select min(Semester) from Studenten))
except
(select Name from Studenten
 where Semester = (select max(Semester) from Studenten));
```

### Implikation und Allquantor

Wie schon mehrmals gezeigt besitz SQL einen Existenzquantor mit `exists`. Es
besitzt jedoch keinen Allquantor $\forall$. Dieser muss dann mit den
existierenden Operationen nachgebildet werden. Selbes gilt fuer die Implikation
$\Rightarrow$. Hierfuer muss man sich an die mathematischen Aequivalenzen fuer
diese Operatoren halten:

* $\forall x F \equiv \not\exists x: \neg F$. Wenn die Formel $F$ fuer alle $x$
  gilt, dann gibt es kein $x$, sodass die Formel fuer dieses $x$ nicht gilt.

* $F \Rightarrow G \equiv \neg F \lor G$. Ex falso quodlibet oder wenn $F$, dann
  auch $G$.

Suchen wir beispielsweise alle Studenten, die alle Vorlesungen von Sokrates
hoeren. In Tupelkalkuel:

$\{s \,|\, s \in \text{Studenten} \land \forall v \in \text{Vorlesungen}(\exists p \in
\text{Professoren}(p.\text{Name}=\text{‘Sokrates’} \land p.\text{PersNr}=v.\text{gelesenVon}) \Rightarrow \exists h
\in \text{hoeren}(h.\text{MatrNr}=s.\text{MatrNr} \land h.\text{VorlNr}=v.\text{VorlNr}))\}$

Fuer jede Vorlesung muss also gelten, wenn sie von Sokrates gelesen werden, dann
muss der Student sie hoeren. Diese Formel enthaelt aber sowohl einen Allquantor,
sowie auch eine Implikation. Beide diese Operatoren koennen wir in SQL nicht
*direkt* ausdruecken.

Wir koennen aber ebenso sagen, dass es fuer einen solchen Studenten keine
Vorlesung gibt, die von Sokrates gelesen wird und die der Student nicht besucht:

$\{s \,|\, s \in \text{Studenten} \land \not\exists v \in \text{Vorlesungen}(\exists p \in
\text{Professoren}(p.\text{Name}=\text{‘Sokrates’} \land p.\text{PersNr}=v.\text{gelesenVon}) \land \not\exists h
\in \text{hoeren}(h.\text{MatrNr}=s.\text{MatrNr} \land v.\text{VorlNr}=h.\text{vorlNr}))\}$

Dieser Ausdruck hat, wie man sieht, nur mehr Existenzquantoren. Wir koennen ihn
fast wortwoertlich in SQL uebersetzen:

```sql
select s.Name
from Studenten s
where not exists (
      select v.VorlNr
      from Vorlesungen v
      where exists (
            select *
            from Professoren p
            where p.Name = 'Sokrates' and
                  v.gelesenVon = p.PersNr
      ) and
      not exists (
            select *
            from hoeren h
            where h.MatrNr = s.MatrNr and
                  h.VorlNr = v.VorlNr
      )
);
```
Den ersten Existenzquantor in der Unteranfrage koennen wir hier sogar
entschachteln. Den zweiten nicht mehr, weil `not exists` fuer alle Eintraege
etwas ueberpruefen muss, waehren wir fuer den ersten Existenzquantor nur ein
geeignetes Tupel finden muessen.

```sql
select s.Name
from Studenten s
where not exists (
      select v.VorlNr
      from Vorlesungen v, Professoren p
      where p.Name = 'Sokrates' and
            v.gelesenVon = p.PersNr and
            not exists (
                select *
                from hoeren h
                where h.MatrNr = s.MatrNr and
                      h.VorlNr = v.VorlNr
            )
);
```

### Joins

Wie schon beschrieben werden bei einer Selektion implizit alle Relationen nach
dem `from`-statement gekreuzt, sodass danach daraus selektiert werden kann. Wird
danach selektiert, ist diese Operation also genau aequivalent zu einem
Theta-Join. Waehrend fuer diese Syntax nach der Auflistung der Relationen
`where` zur Selektion der Tupel verwendet wird, wird bei allen `join`-Befehlen
ausser dem `cross join` immer `on` anstelle von `where` benutzt. Es gibt dann
folgende Join-Arten:

* `cross join`: Kreuzprodukt.
* `natural join`: Natuerlicher Verbund.
* `join` oder `inner-join`: Theta-join (die Bedingung $\theta$ folgt dann mit
  `on`)
* `left|right|full outer-join`: Auesserer Join.

Das Schluesselwort `cross join` ist hierbei exakt ident zum Komma `,` zwischen
Relationen. Die Bedingung (Praedikat) darf danach auch nicht mit `on` sondern
muss mit `where` angegeben werden.

Der natuerliche Verbund, durch `natural join`, hat natuerlich die selbe Semantik
wie in der relationalen Algebra. Und somit hat er auch nie eine (explizite)
Bedingung (also nie `on`). Mit `join ... on` kann man nun aber `... where`
ersetzen:

```sql
select Titel
from
  (Studenten s join hoeren h on h.MatrNr = s.MatrNr)
  join Vorlesungen v on v.VorlNr = h.VorlNr
where s.Name = 'Fichte';
```

Was man hier sieht:

1. Der ganze Ausdruck ist mit `join` viel schwieriger zu schreiben weil man immer
   nur auf zwei Relationen arbeiten kann.

2. Man kann trotzdem noch `where` Bedingungen haben, um danach noch Tupel zu
   selektieren (diese Bedingung haette man aber auch in den ersten Join geben
   koennen).

Was jedoch schon nuetzlich ist und nur umstaendlich anders nachahmbar ist, sind
die auesseren Joins, die fuer Tupel ohne Join-Partner eben auch noch Eintraege
im Ergebnis produzieren. Wir wollen beispielsweise alle Studenten finden, und
wenn sie Vorlesungen hoeren, dann auch diese sehen:

```sql
select s.Name, v.Titel
from (Studenten s left outer join hoeren h on h.MatrNr = s.MatrNr)
     left outer join Vorlesungen v on v.VorlNr = h.VorlNr;
```

oder wie gesagt mit Mengenoperationen:

```sql
/* Zuerst die mit Vorlesungen */
select s.Name, v.Titel
from Studenten s, hoeren h, Vorlesungen v
where s.MatrNr = h.MatrNr and
      h.VorlNr = v.VorlNr

union all -- wird auch keine geben, aber effizienter

/* Dann die ohne */
select s.Name, null
from Studenten s
where not exists (
      select *
      from hoeren h
      where h.MatrNr = s.MatrNr
);
```

## Modularisierung

Weiter oben wurde gezeigt, wie mit geschachtelten Anfragen eine temporaere
Relation erstellt werden kann. Dies kann aber unuebersichtlich sein. Viel
besser ist es oft, mit dem sogennannte `with ... as` Konstrukt temporaere
Tabellen zu erstellen. Die Syntax von `with ... as` ist wie folgt:

```sql
with <Name1>(<Schema>) as (<Attribute>),
     <Name2>(<Schema>) as (<Attribute>),
   ...
   ;
```

Was man sich unbedingt merken sollte, ist das man in einer SQL-Anfrage immer nur
ein einziges `with` Statement haben kann. Denn wenn man mehr als eine temporaere
Relation bauen will, dann muss das wie oben angezeigt noch im selben
`with`-statement passieren. Das `(<Schema)` ist hierbei auch optional und wird
nur benoetigt, wenn es nicht aus dem Statement nach dem `as` heraus ersichtlich
ist.

Beispielsweise wollen wir in einem sehr langen SQL-Statement oft direkt von der
*hoeren*-Relation auf die entsprechende Vorlesung in der *Vorlesungen*-Relation
zugreifen. Daher moechten wir uns zuerst schon eine solche temporaere Relation
bauen. Auch hatten wir gerne dass jeder Professor seinen Assistenten bei sich in
der Relation hat. Dann wollen wir jene Studenten selektieren, die alle
Vorlesungen des Professoren hoeren, der Platon als Assistent hat:

```sql
with hoeren2 as (
     select s.Name, h.*, v.Titel
     from Studenten s, hoeren h, Vorlesungen v
     where s.MatrNr = h.MatrNr and
            h.VorlNr = v.VorlNr
  ), Professoren2 as (
     select p.PersNr as Prof, a.Name as Assi
     from Professoren p, Assistenten a
     where p.PersNr = a.Boss
)

select s.Name
from Studenten s
where not exists (
      select *
      from Vorlesungen v, Professoren2 p
      where p.Assi = 'Platon' and
            v.gelesenVon = p.Prof
      and not exists (
          select *
          from hoeren2 h
          where h.MatrNr = s.MatrNr and
                h.VorlNr = v.VorlNr
      )
);
```

Es gibt dann noch ein spezielles Konstrukt, `values`, womit man so einer
Relation sogar manuell Werte einfuegen kann. Da man hier nicht aus einer anderen
Relation mit einem bereits definierten Schema selektiert, muss man manuell, wie
oben beschrieben, das Schema angeben. Das bedeutet jetzt nicht, dass man die
Datentypen und Integritaetsbedingungen hinzufuegen muss. Man muss lediglich in
Klammern die Namen der Attribute angeben. `values` ist dabei keine Funktion,
sondern ein Schluesselwort, dass besagt, dass danach Tupel folgen. Ein
einzelnes Tupel koennte man also mit `values (1, 2, 3)` einfuegen, viele Tupel
mit `values (1,2,3),(4,5,6),(7,8,9),...`. Beispiel:

```sql
with Foo(A, B, C) as (values (1, 2, 3), (4, 5, 6))
```

Die Klammern nach dem `as` sind hier noch immer unbedingt notwendig.

## Veraendern von Daten

Natuerlich muessen Daten in der Datenbank auch ab und an veraendert, geloescht
oder neu eingefuegt werden. Das geht jeweils mit den `update`, `delete` und
`insert` Befehlen. Es sei angemerkt dass Veraenderungen am Datebestand immer in
zwei Schritten geschieht. Wenn man beispielsweise Tupel loescht, werden zuerst
alle diese zu loeschenden Tupel gesammelt, und erst dann werden sie wirklich
geloescht. Wenn das nicht so waere, koennten zuerst geloschte Tupel die
Selektionskriterien fuer folgende Tupel beeinflussen; die Loeschreihenfolge
koennte also sonst einen Einfluss haben.

### `insert`

Man kann Daten wie beim `with-as` Konstrukt entweder manuell spezifizieren, oder
durch eine Anfrage. Manuell geht das wieder mit dem Schluesselwort `values`:

```sql
insert into Studenten values (1, 'Peter', 5), (2, 'Hansi', 9);
```

Mit einem Statement geht es dann so, wobei das Schema natuerlich ident mit der
Zielrelation sein muss:

```sql
/* Wir machen alle Professoren zu Erstsemestern */
insert into Studenten
  (select PersNr as MatrNr, Name, 1 from Professoren);
```

Wenn man nicht alle Attribute einfuegen kann oder will, kann man nach der
Zielrelation auch in Klammern explizit die Attribute angeben. Wenn man ein
Attribut dann weglaesst, wird entweder der `default`-Wert uebernommen, der
bei Schemadefinition eingestellt werden kann, oder `null`, wenn es keinen
solchen `default`-Wert fuer das konkrete Attribut gibt:

```sql
create table data(x int default 5, y int, z int);

insert into data(z) values (7); -- fuegt 5,null,7 ein
```

### `update`

Fuer den `update`-Befehl wird das besondere Schluesselwort `set` verwendet,
womit man Attributen neue Werte geben kann. Allgemein:

```sql
update <Relation>
set <Attribut> = <Neuer Wert>
where <Bedingung fuer Tupel>;
```

In der `where`-Klausel koennen also jene Tupel selektiert werden, die veraendert
werden sollen. Beispielsweise wollen wir Xenokrates zu *Peter* umbennen und ihn
um ein Semester nach oben schieben:

```sql
update Studenten
set Name = 'Peter', Semester = Semester + 1
where Name = 'Xenokrates';
```

Es sei angemerkt dass es in manchen Datenbanksystemen (SQLite) nicht moeglich
ist, der zu veraendernden Relation einen Alias zu geben. Daher muss man oft mit
dem `in` statements eine Selektion fuer ein Tupel machen:

```sql
update Assistenten
set Boss = null
where PersNr in (
  select a.PersNr
  from Assistenten a, Professoren p
  where p.Name = 'Sokrates' and a.Boss = p.PersNr;
)
```

### `delete`

Loeschen geht ohne grosse Ueberaschungen aehnlich wie selektieren und updaten:

```sql
delete
from <Relationen>
where <Bedingungen fuer Tupel>;
```

Loeschen wir alle Studenten mit Semester groesser $13$:

```sql
delete
from Studenten
where Semester > 13;
```

## Zusaetzliche Sprachkonstrukte

Hier seien weitere Sprachkonstrukte von SQL erlaeutert.

### Casting

Es kann manchmal noetig sein, Werte eines bestimmten Datentyps zu einem anderen
Datentyp zu transformieren. Das geht wie in anderen Programmiersprachen ueber
*Casting*. Gecastet wird in SQL hierbei explizit ueber die Funktion `cast`,
wobei folgen Syntax zu befolgen ist:

```sql
cast(<Wert> as <Datentyp>)
```

Beispielsweise wenn man eine arithmetische Division durchfuehren moechte, aber
beide Operanden vom Datentyp `integer` sind, muss man zumindest einen der beiden
Werte zu einer Dezimahlzahl transformieren:

```sql
select name, cast(Semester as real)/(select avg(Semester) from Studenten)
from Studenten;
```

### limit

Wird das Schluesselwort `limit n` mit einem ganzzahligen Argument `n` einer
Anfrage nachgehaengt, werden nur maximal `n` ausgegeben (die `n` ersten die das
System findet). Wir wollen beispielsweise 4 Studenten heute essen, aber uns ist
egal welche:

```sql
select Name from Studenten limit 4;
```

### coalesce

Die `coalesce(x,y)` Funktion nimmt zwei Argumente `x` und `y`. Falls `x` gerade
`null` ist, wird `y` angegeben, ansonsten `x`. `y` ist sozusagen also der
Default-Wert fuer `x`, falls `x` null ist.

### `case`

`case` ist aehnlich zu anderen `switch/case`-Ausdruecken in vielen anderen
Sprachen. Hierbei ist die Syntax wie folgt:

```sql
(case
  when <condition> then <expression>
  when <condition> then <expression>
  ...
  else <expression>
end)
```

Ein solches Statement beginnt also mit `case` und endet mit `end`. Dann folgt
nach jedem `when`-Schluesselwort eine Bedingung, gefolgt von `then`. Dann kommt
der jeweilige Ausdruck, der aus diesem Statement zurueckgegeben werden soll,
falls die Bedingung nach dem `when` gilt. `else` ist am Ende so wie `default` in
C/C++/Java.

Diese Bedingungen werden sequentiell ueberprueft, also die erste die passt wird
genommen.

### `between`

Mit `<attribute> between <lower_bound> and <upper_bound>` kann man ueberpruefen,
ob ein Wert innerhalb eines bestimmten Rahmens liegt. Es ist aequivalent zu
`attribute >= lower_bound and attribute <= upper_bound`. Man bemerke, dass die
Grenzen auf beiden Seiten also inklusiv sind.

```sql
select Name
from Studenten
where Semester between 1 and 4;
```

Fuer kleine Wertebereiche kann man hier auch da `in`-Statement verwenden, um zu
ueberpruefen, ob der Wert `in` einer Menge `(...)` liegt:

```sql
select Name
from Studenten
where Semester in (1,2,3,4);
```

### `like`

`like` kann zum Pattern-Matching von Strings verwendet werden. Die einzigen zwei
speziellen Zeichen sind hier `%` und `_`:

* `%` ist wie `.\*` fuer RegEx, also beliebig viele (0 - infinity) Zeichen.
* `_` steht fuer *genau einen* character (wie `.`).

Man kann `like` auch negieren, also `not like` schreiben.

So kann man beispielsweise alle Studenten finden, dessen Namen mit "F" beginnen:

```sql
select *
from Studenten
where Name like 'F%';
```

Oder gegensaetzlich:

```sql
select *
from Studenten
where Name not like 'F%';
```

In vielen grossen Datenbanksystemen (z.B. PostgreSQL) sind auch wirkliche
regulare Ausdruecke moeglich.

http://www.postgresql.org/docs/9.4/static/functions-matching.html

### nullif

`nullif` ist in SQL eine Funktion, die einen Wert zu `null` umwandelt, wenn es
gleich einem anderen Wert ist. Beispielsweise wollen wir dass fuer unsere
Anfrage ein leerer String dasselbe wie `null` ist, dann schreiben wir:

```sql
select name, nullif(name, '')
from ...;
```

Das koennen wir dann z.B. auch mit `coalesce` verbinden:

```sql
select name, coalesce(nullif(name, ''), 'Empty!')
from ...;
```

## Sichten

Eine Sicht (`view`) ist in SQL ein Konstrukt, um virtuelle Relationen auf
anderen, existenten Relationen in der Datenbasis aufzubauen. Wenn wir in
verschiedenen Relationen Daten gespeichert haben, aber dann diese Daten gerne
ein wenig umorganisiert haetten, entweder um weniger Attribute einer einzigen
Relation zu zeigen oder um verschiedene Attribute vieler Relationen zu vereinen,
dann haben wir bisher zwei Moeglichkeiten:

1. Wir erstellen eine vollkommen neue Relation und legen sie in der Datenbank
   ab. Das fuehrt natuerlich zu einer enormen Datenredundanz.

2. Wir erstellen in jeder Anfrage eine temporaere Tabelle mit dem `with-as`
   Konstrukt. Das ist natuerlich umstaendlich, wenn man bei jeder Anfrage dieses
   `with-as`-Statement definieren muss.

Gerne haetten wir eine Moeglichkeit, in unserer Datenbank in einem speziellen
Format zu speichern, welche Attribute wir gerne aus welchen Relationen
selektieren moechten. Genau das ist eine *Sicht* bzw. eine `view`.

Wir moechten beispielsweise oft Vorlesungen auch gleich mit dem Namen ihres
Professors abrufen. Dann koennen wir folgende Sicht definieren:

```sql
create view VorlesungenMitProfessoren
select v.*, p.Name
from Vorlesungen v, Professoren p
where v.gelesenVon = p.PersNr;
```

Diese Sicht wird dann in der Datenbank als spezielles Objekt, wie eine Tabelle,
*permanent* abgelegt. Wenn wir sie dann in einer Anfrage referenzieren, wird sie
on-the-fly neu berechnet. Grundsaetzlich erscheint sie dem SQL-Benutzer aber wie
eine voellig normale Tabelle -- zumindest fuer Selektionen. Welchen Sinn kann
eine solche Sicht nun also haben?

1. Vereinfachung von komplexen Tabellenstrukturen bzw. Beziehungen zwischen
   diesen. Den Vorteil von Sichten bezueglich dieser Eigenschaft haben wir schon
   oben gezeigt. Anstatt jedes Mal die *Vorlesungen* Tabelle auf die
   *Professoren* Tabelle zu joinen, koennen wir direkt mit dieser Sicht
   arbeiten.

2. Einschraenkung von Zugriffsrechten fuer bestimmte Benutzer. Wir moechten
   z.B. das Passwort von Nutzern fuer unsere Applikation mit dem Benutzernamen
   und echten Namen der Benutzer abspeichern. Den Hackern moechten wir dann aber
   nur die Benutzernamen zeigen, und nicht die Passwoerter. Dann koennen wir
   solche sensitiven Information vor den Hackern mit einer Sicht verbergen. Das
   waere aehnlich dem Konzept von Zugriffsrechten in Betriebssystemen:

   ```sql
   create table users(login varchar(30), realname varchar(50), password
   varchar(10));

   create view l33tH4x0rView select login from users;
   ```

3. Zur Modellierung von Vererbung. Wir koennen durch Sichten Vererbung auf zwei
   Weisen modellieren:

   1. Indem wir die Obertypen in separaten Relationen
      abspeichern, und in den Relationen fuer die Untertypen nur deren
      spezialisierte Werte speichern. Dann koennen wir die "echten"
      Untertyp-Relationen mit einer Sicht definieren, wobei wir die Attribute
      des Obertyps noch an den Untertyp dranjoinen.

   2. Indem wir die Untertypen voll definieren, und dann eine Sicht erstellen,
      die nur jene Attribute der Untertypen enthaelt, die eigentlich der Obertyp
      definieren sollte.

  Die erste Variante ist besser, wenn wir oefter Zugriffe auf die Obertypen
  machen und nur selten die Untertypen in ihrer vollen Definition
  benoetigen. Denn die vollen Definition muessen dann ja neu berechnet
  werden. Die zweite Variante ist besser, wenn wir oefter die ganzen Untertypen
  brauchen und nur selten die Obertypen.

Man muss bei Sichten jedoch beachten, dass sie oft nicht veraenderbar
sind. Generell sind Sichten nur dann veraenderbar wenn:

1. Sie sich nur auf eine Tabelle (also eine weitere Sicht oder eine Relation)
   beziehen.

2. Sie den Schluessel dieser Tabelle auch enthalten.

3. Sie nur Attribute und Tupel enthalten, die in der Basistabelle auch enthalten
   sind. Das bedeutet insbesondere, dass keine Aggregationen durchgefuehrt
   werden duerfen, sowie auch generell keine Gruppierungen durch `group by`.

## Referentielle Integritaet

Ist ein Teil des Schemas einer Relation $S$ der Schluessel einer anderen
Relation $R$, so nennt man diese Attributmenge in $S$ einen *Fremdschluessel*
auf die Relation $R$. Referentielle Integritaet beschreibt nun die Eigenschaft
dieses Fremdschluessels in $S$, immer auf ein existentes Tupel in $R$ zu
zeigen. Ansonsten waere der Fremdschluessel eine *Dangling Reference*. Diese
Eigenschaft, dass ein Fremdschluessel immer gueltig sein soll, nennt man
*referentielle Integritaet*.

Sei $\alpha \in S$ der Fremdschluessel auf $R$, wobei $\kappa$ der Schluessel
von $R$ ist, sodass also gilt $\alpha = \kappa$. Damit $\alpha$ dann
referentielle Integritaet aufweisst, muss fuer ein Tupel $s \in S$ folgendes
gelten:

1. $s.\alpha$ enthaelt entweder nur `null`-Attribute, oder keinen einzigen
   `null`-Wert.

2. Wenn es keinen `null`-Wert enthaelt, dann muss ein Tupel $r \in R$ existieren
   sodass $s.\alpha = r.\alpha$. $s.\alpha$ darf also nie eine dangling
   Reference sein.

Wir duerften also nicht einfach einen beliebigen Schluessel in $S$
einfuegen. Der Schluessel muss auf ein echtes Tupel in $R$ zeigen. Genauer gibt
es hier mehrere Anforderungen an die beiden Relationen $S$ und $R$, sodass
referentielle Integritaet garantiert sein kann (wieder fuer Tupel $s \in S, r
\in R$):

1. $\Pi_\alpha(S) \subseteq \Pi_\kappa(R)$: Jeder Fremdschluessel in $S$ muss
   als Pendant auch immer einen echten Schluessel in $R$ besitzen.

2. Wenn wir ein neues Tupel $s$ in $S$ einfuegen, muss also immer ein $r \in R$
   existieren sodass $s.\alpha = r.\kappa$ bzw. sodass $s.\alpha \in
   \Pi_\kappa(R)$ ist.

3. Wenn wir die Fremdschluesselattribute $s.\alpha$ des Tupels $s \in S$
   veraendern zu $s.\alpha'$, somuss danach das selbe gelten wie in (2):
   $s.\alpha' \in \Pi_\kappa{R}$.

4. Gibt es fuer einen Schluessel $r.\kappa$ mit $r \in R$ auch nur eine Kopie
   $s.\alpha$ wo $s \in S$ und $\alpha$ Fremdschluessel, dann darf $r.\kappa$
   weder geloescht noch veraendert werden. Man darf einen Schluessel in $R$ also
   nicht einfach veraendern, da sonst alle Tupel in $S$, dessen Fremdschluessel
   eben eine Kopie dieses veraenderten Schluessels waren, ungueltig
   waeren. Analoges gilt natuerlich fuer das Loeschen des Schluessels in $R$.

### Referentielle Integritaet in SQLite

In SQL kann referentielle Integritaet durch das Schluesselwort `references`
bzw. `foreign key` erreicht werden.

`references` wird dabei inline neben einem Attribut in der Schemadefintion
angegeben, wobei nach `references` dann eine Relation folgt. Mit `x integer
references R` drueckt man zum Beispiel aus, dass $x$ als `integer` definiert ist
und die Relation $R$ referenziert. Implizit wird hierbei der Primaerschluessel
von $R$ gemeint, also ist $x$ dann eben Fremdschluessel. Soll ein oder mehrere andere
Attribute referenziert werden, kann man in Klammern nach der Relation diese
Tupel noch anfuehren. Also allgemein:

```sql
create table <Name>(
  <Attribut> <Typ> references <Relation>(<Referenzierte Attribute>),
  ...
);
```

Sei $R$ nun wieder eine Tabelle und $S$ eine weitere, wobei $S$ die Tabelle $R$
mit obiger Syntax referenzziert. Was schon alleine durch die Angabe von
`references` erreicht wird ist, dass SQL immer eine Fehlermeldung ausgibt wenn:

1. Man in der referenzierten Relation $R$ einen Schluessel veraendert, auf den
   es noch Referenzen (Fremdschluessel) in der Tabelle $S$ gibt.

2. Man in der referenzierten Relation $R$ einen Schluessel, auf den es wie in
   (1) noch Referenzen gibt, zu loeschen versucht (also mit `delete`).

3. Man in $S$ einen Fremdschluesselwert einfuegen moechte, wo es aber keinen
   gleichen Schluessel in $R$ gibt.

4. Man in $S$ einen gueltigen Fremdschluessel verandern moechte, sodass er
   danach auf kein in $R$ existentes Tupel zeigen wuerde.

Wie `check` oder `primary key` kann man diese Integritaetsbedingung auch
out-of-line, also nach Definition des grundlegenden Schemas durch Attributnamen
und Domaenen, spezifizieren. Hierzu gibt man das Attribut, neben das man oben
`references` geschrieben hatte als Argument zum Ausdruck `foreign key` und
schreibt danach wie oben `references ...`. Wie fuer `check` und `primary key` hat
das wieder den Vorteil, dass man die Schemadefinition von diesen zusaetzlichen
Bedingungen und Eigenschaften trennen kann um die Schemadefinition
uebersichtlicher zu machen. Es ist auch wie bei `primary key` die einzige
Moeglichkeit, Fremschluessel mit mehr als einem Attribut zu erstellen. Das ist
natuerlich immer dann notwendig, wenn der Schluessel in der referenzierten
Relation auch mehrere Attribute umfasst.

Die allgemeine Syntax waere also nun:

```sql
create table <Name>(
  <Attribut> <Typ>,
  ...

  foreign key(<Attribut1>, <Attribut2>)
  references <Relation>(<Referenzierte Attribute>)
);
```

Mit `unique foreign key` kann man noch zusaetzlich angeben, dass nur ein Tupel
in der referenzier*enden* Relation (jene oben) einen bestimmten Fremdschluessel
haben kann. Das wuerde also auf eine $1:1$-Funktionalitaet hindeuten,
wohingengen ohne das `unique` eine $N:1$-Funktionalitaet erlauebt waere.

### Aktionen

Was ist, wenn wir nun doch eine Aenderung an einem Schluessel in einer
referenzierte Relation durchfuehren wollen wuerden? Hierfuer gibt es eine Reihe
von Aktionen, die wir an das `references`-Schluesselwort anhaengen
koennen. Generell hat man dann die Syntax:

```sql
<Attribut> <Typ>
  references <Relation>(<Referenzierte Attribute>)
  on <condition> <action>;
```

Bzw. aehnliches fuer die out-of-line Variante:

```sql
foreign key(<Attribut1>, <Attribut2>)
  references <Relation>(<Referenzierte Attribute>)
  on <condition> <action>
```

Was man durch diese Angaben bewirken kann, ist das bei einer Aenderung des
Schluessels in der referenzierten Relation eine bestimmmte *referentielle
Aktion* (*referntial action*) auf dem Tupel mit dem Fremdschluessel
durchgefuehrt werden kann.

`<condition>` kann hierbei dann sein:

* `update`: Fuehre die `<action>` bei einer Veraenderung des Schluessels aus.
* `delete`: Fuehre die `<action>` aus, wenn der Schluessel bzw. das ganze Tupel
            des Schluessels geloescht wird.

und `<action>` kann sein:

* `cascade`: fuehre die gleiche Operation fuer den Fremdschluessel aus, also
             veraendere den Fremdschluessel gleichfalls oder loesche gleichfals
             das ganze Tupel des Fremdschluessels.

* `set null`: setze den Fremdschluessel zu `null`.

* `set default`: setze den Fremdschluessel auf seinen Default-Wert, wenn in der
                 Schemadefinition mit `default` einer angegeben wurde.

* `restrict`: gib eine Fehlermeldung aus, wenn man versucht, eine Veraenderung
              durchzufuehren. Diese Aktion ist auch die Default-Aktion, wenn man
              nur `references` angibt.

So koennen wir nun beispielsweise sagen, dass wenn ein Professor stirbt, jedes
*gelesenVon*-Attribut in der Relation *Vorlesungen* auf `null` gesetzt wird (bis
sich ein anderer Dozent findet):

```sql
create table Vorlesungen(
  ...
  gelesenVon integer references Professoren on delete set null
  ...
);
```

### foreign key

Ein foreign key (Fremdschluessel) ist der primary key (Primaerschluessel) einer
anderen Relation. Man kann beim Erstellen einer Tabelle mit foreign
keys bestimmen, was bei einer bestimmten Operation am Schluessel in der Relation
mit dem Eintrag in der fremden Relation passiert.

Sei $S$ eine Relation mit Fremdschluessel $k$, die auf eine Relation $R$ verweist:

```sql
create table S(
  k integer,
  foreign key(k) references R(k)
  on <condition> <action>
);
```

Note hier dass die condition sich auf eine Aktion auf die referenzierten Tupel in $R$ bezieht, nicht auf die Spalten in $S$.

Wo `<condition>` ist eines von:

* update
* delete

Und `<action>` ist eines von (*referential actions*):


## Rekursion

Reference: http://www.postgresql.org/docs/8.4/static/queries-with.html

Recursive queries are of the form

```sql
with recursive <table_name>(n) as (
  select 1 -- anchor, the first selection, non-recursive

  union all -- to join subsequent stacks

  -- recursive query, evaluated
  -- while returns something
  select n + 1 from <table_name>
) ...
```

In actuality, this is not recursion. The "recursive" query will just repeat for all the new input it gets.

Example:

```sql
with recursive vorher(vorlesung, semester) as (
  select v.vorlnr, 0
  from Vorlesungen v
  where v.titel = 'Der Wiener Kreis'

  union all

  select vs.vorgaenger, vo.semester + 1
  from Voraussetzen vs, vorher vo
  where vs.nachfolger = vo.vorlesung
)
select * from vorher;
```

1. Here, the table first includes only 'Der Wiener Kreis'.
2. Then, for each (remaining) entry in the table, where there exists a tuple such that the vorlesung is the nachfolger
3. Add it to the table
4. Are there any remaining entries in the table? Yes, the ones we just added.
5. Back to 2.

Note how this is really more of a loop between 2 and 4, not so much recursion.

Values from 1 to 10:

```sql
with recursive numbers(n) as (
  select 1

  union all

  select n + 1
  from numbers
  where n < 10
)

select * from numbers;
```

Other example, friends of Fichte with Grad:

```sql
with recursive friends(name, matrnr, grad) as (
    select s.name, s.matrnr, 0
    from Studenten s
    where s.name = 'Fichte'

    union all

    select s.name, s.matrnr, f.grad + 1
    from Studenten s, hoeren h1, hoeren h2, friends f
    where
        s.matrnr = h1.matrnr and
        h2.matrnr = f.matrnr and
        h1.matrnr != h2.matrnr and
        h1.vorlnr = h2.vorlnr and
        f.grad < 5
)
select name, matrnr, min(grad) as grad
from friends
group by name, matrnr;
```


## Examples

Alle Studenten, die alle 4-stuendigen Vorlesungen hoeren:

```sql
select s.name
from Studenten s
where not exists (
    select *
    from Vorlesungen v
    where v.SWS = 4 and not exists (
        select *
        from hoeren h
        where h.matrnr = s.matrnr
          and h.vorlnr = v.vorlnr
    )
);
```

oder auch:

```sql
select s.name
from Studenten s
where not exists (
    select *
    from Vorlesungen v
    where v.SWS = 4
      and v.vorlnr not in
      (select h.vorlnr  from hoeren h where h.matrnr = s.matrnr)
);
```

Studenten mit Semester > 5 and Notenschnitt < 2.5:

```sql
select s.matrnr, s.name, avg(p.note)
from Studenten s, pruefen p
where s.matrnr = p.matrnr
  and s.semester > 5
group by s.matrnr, s.name
having avg(p.note) < 2.5;
```

Cast:
```sql
 cast(avg(p.note) as integer)
 ```

Tupelkalkuel:

$\{s \, | \, s \in \text{Studenten} \land \forall v \in \text{Vorlesungen} (\exists h \in \text{hoeren}
 (h.\text{VorlNn} = v.\text{VorlNr} \land h.\text{MatrnNr} = s.\text{MatrNr}))\}$

(Studenten, die alle Vorlesungen hoeren)

```sql
select *
from Studenten s
where not exists
(
    select *
    from Vorlesungen v
    where not exists
    (
        select *
        from hoeren h
        where h.matrnr = s.matrnr  
          and v.vorlnr = h.vorlnr
    )
);
```

Da $\forall x P \equiv \not \exists x (\neg P)$

## Gotchas

* Immer ueberlegen, ob es Duplikate geben koennte!
