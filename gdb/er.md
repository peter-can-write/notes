# Entity-Relationship Modell

Das Entity-Relationship (ER) ist eine Methodik zum Datenbanksentwurf. Hierbei
werden Objekte und Beziehungen der konkreten Welt, fuer die eine Datenbank
relevant ist, durch *Entities* und *Relationships* dargestellt. Genauer:

Die grundlegende Struktur des ER-Modells umfasst *Entities*, welche konkrete
Instanzen von Objekten des Systems sind, welche das Modell zu beschreiben
versucht. Zwischen *Entities* koennen bestimmte Beziehungen gegeben sein, welche
durch *Relationships* zwischen den konkreten Objekten beschrieben
werden. Zusatzlich kann ein Entity noch bestimmte Eigenschaften, sogenannte
*Attribute*, besitzen, welche noch weitere Informationen zu den Objekten liefern
und sie womoeglich in einer Menge von vielen struktur-aehnlichen Objekten
unterscheiden. Letztlich kann es bei Relationships noch *Rollen* geben, welche
den einzelnen Teilnehmern der Beziehung, also den Entities, noch zusaetzliche,
fuer die konkrete Beziehung passende, Namen gibt. Entities des selben *Typs*,
also welche allesamt konkrete, verschiedene Instanzen einer bestimmten Klasse
von Objekten des Modeslls sind, werden in *Entitytypen* bzw. *Entitymengen*
zusammengefasst.

In einem ER-Diagramm werden nun immer nur *Entitytypen* angegeben. Diese werden
durch Rechtecke beschrieben, wobei der Name des Entitytyps innerhalb des Rechtecks
dargestellt wird. Zwischen den Entitytypen gibt es dann die Beziehungen, welche
eben fuer alle Instanzen des Entitytyps, also den konkreten Entities,
gelten. Die Beziehungen werden dabei in Rauten angegeben.

Wie oben beschrieben koennen Entities noch ueber Attribute verfuegen. Unter
diesen kann es dann ein oder mehrere bestimmte Attribute geben, welche ein
konkretes Entity unter den anderen seines Typs genau bestimmt. Wir nennen diese
Gruppe von identifizierenden Attributen den *Schluessel* bzw. die
*Schluesselattribute* des Entitytyps. In einem ER-Diagramm werden
Schluesselattribute dabei zur Visualisierung einfach unterstrichen.

Hier unten nun ein ER-Diagramm gemalt. Die Schluessel sind wie gesagt
unterstrichen dargestellt.

```
(_ID_)                           (_HundeNr_)
      \                         /
	   [Person]--<liebt>--[Hund]
	  /                         \
(Name)                           (Rasse)
```

## Beziehungstypen

Eine Beziehung (Relationship) $R$ zwischen den Entitytypen $E_1,E_2,...,E_n$
kann man auch als Relation im mathematischen Sinne aufffassen. Das bedeutet,
eine Beziehung ist nichts weiteres als eine *Teilmenge* des kartesischen
Produkts (Kreuzprodukts) aller Entities:

$$R \subseteq E_1 \times E_1 \times ... \times E_n$$

Das Kreuzprodukt gibt dabei die Menge aller moeglichen Tupel mit Elementen
jeweils aus $E_1, E_2, ..., E_n$ . Die Beziehung selbst waehlt dabei beliebig
viele passende Tupel aus dieser Menge aller Tupel aus. Ein solches Tupel
$(e_1,e_2,...,e_n) \in R$ nennt man dabei eine *Instanz* des Beziehungstyps.

Wieviele Entitytypen an diesem Kreuzprodukt teilnehmen, wird durch den *Grad*
der Beziehung angegeben. Oben waere der Grad z.B. $n$. Der Grad einer Beziehung
$R \subseteq E_1 \times E_2$ ist dementsprechend $2$. Eine solche Beziehung
nennt man dann auch *binaer*.

## Funktionalitaeten

Beziehungstypen koennen bezueglich bestimmten Eigenschaften, sogenannten
*Funktionalitaeten*, untersucht werden. Funktionalitaeten geben an, mit
wievielen Entities ein konkretes Entity, das an einer Beziehung teil nimmt, in
Beziehung stehen darf bzw. kann. Beispielsweise gibt es Situationen, wo es nur
sinnvoll ist, das ein Entity mit nur einem anderen Entity in Beziehung
steht. Ist $e_1$ das konkrete Entity, duerfte $e_1$ also nur genau einmal in
einem Tupel aus $R$ vorkommen. Es gibt hierbei nun mehrere Klassen von solchen
Funktionalitaeten:

* $1:1$-Beziehungen. Hierbei ist die Funktion sozusagen bijektiv (man beachte:
  partiell). Fuer ein Entity $a$ vom Typ $A$ gibt es genau ein einziges,
  eindeutiges Entity $b$ vom Typ $B$, sodass $(a,b) \in R$.

  Ein Beispiel waere jegliche Form von Beziehung zwischen zwei Menschen, sagen
  wir verschiedenen Geschlechts. Es gibt fuer jeden Mann genau eine Frau und
  umgekehrt.

* $1:N$ oder $N:1$ Beziehungen. Hier gibt es nun fuer ein konkretes Entity $a$
  vom Typ $A$ nicht nur ein eindeutiges gepartnertes Entity $b$ vom Typ $B$,
  sondern es kann eben *beliebig* viele solcher Entities vom Typ $B$ geben,
  sodass $(a,b) \in R$. *Beliebig* heisst hierbei, dass es kein einziges, genau
  eines oder mehr als ein solches $b \in B$ gibt.

  Ein Beispiel hier waere die Mutter-Kind Beziehung *KindVon*. Fuer eine Mutter
  kann es viele Kinder geben, die *KindVon* dieser Mutter sind. Aber fuer ein
  Kind gibt es immer nur eine eindeutige Mutter.

* $N:M$ Beziehungen. Diese sehr allgemeine Form laesst nun fuer beide Seiten
  beliebige viele Partner zu. Das bedeutet, dass es viele $b \in B$ geben kann,
  sodass fuer ein $a \in A$ gilt: $(a, b) \in R$. Ander als bei $1:N$
  Beziehungen kann es nun aber auch in die andere Richtung mehrere $a$s geben,
  mit denen ein $b$ gepartnert ist.

  Ein Beispiel hier ist die Beziehung *hoert* zwischen den Entitytypen *Studenten*
  und *Vorlesungen*. Hierbei kann also ein Student $N$ Vorlesungen
  hoeren. Ebenso kann aber nun eine Vorlesungen auch von mehr als einem, also
  einer beliebigen Anzahl $M$ an Studenten besucht werden.

Alle Funktionalitaeten, die eine $1$ auf einer Seite haben, haben fuer jede
Instanz der Beziehung zumindest ein Entity, dass auf einer Seite eines Tupels
der Beziehung maximal einmal vorkommen darf. Dies kann man also im Sinne einer
*partiell* definierten Funktion darstellen. Die Funktion ist dabei nur partiell,
weil nicht jedes Entity eines Typs in der Beziehung mit einem Partner enthalten
sein muss. Fuer $1:1$ kann die Funktion in beide Richtungen gehen:

$$A \rightarrow B$$
$$B \rightarrow A$$

Bei $1:N$ bzw. $N:1$ Beziehungen muss die Abbildung immer auf die Seite mit der
$1$ sein, da eine Funktion ja *rechtseindeutig* sein muss. Gibt es
beispielsweise fuer $N$ Angestellte nur eine Firma, gilt:

$$\text{Angestellter } \rightarrow \text{ Firma}$$

Umgekehrt waere es nicht erlaubt, weil man auf Basis einer konkreten Firma nicht
einen eindeutigen Angestellten bestimmen kann.

### Mehrstellige Funktionalitaeten

Oben wurden jeweils nur binaere Beziehungen betrachtet; also solche, wo nur zwei
Entitytypen an einer Beziehung teilnehmen. Wie lassen sich Funktionalitaeten
aber realisieren bzw. darstellen, wenn mehr als zwei Entitytypen an der
Beziehung teilnehmen? Hierbei braucht man nun eine Verallgemeinerung des oben
vorgestellten Konzepts der Identifizierung von Funktionalitaeten mit
partiell-definierten Funktionen. Steht bei einer mehr-als-zwei-stelligen
Beziehung bei einem Entity eine $1$ (im Sinne der Funktionalitaetsangaben),
bedeutet das, dass wenn man von jedem anderen Entitytypen, der an der Beziehung
teilnimmt, eine Instanz waehlt, daraus dann zusammen genau das eindeutig
passende Entity mit der $1$-Funktionalitaet bestimmbar ist. Formal: Ist $E$ eine
Entity mit der Funktionalitaet $1$, dann muss aus dem Kreuzprodukt aller anderen
Entities genau nur ein solches zu jedem Tupel passendendes Entity aus $E$
folgen:

$$R: E_1 \times ... \times E_n \rightarrow E$$

Man nehme dazu die Beispielentities *Professoren*, *Studenten* und
*Vorlesungen*, welche durch der Beziehung *hoeren* verbunden sind. Man sucht
sich nun je zwei Entitytypen raus (allgemein: fuer $N$ Entitytypen wuerde man sich
$N - 1$ Entitytypen raussuchen) und ueberprueft, wieviele Entities vom letzten
Typ es fuer diese Kombination von Entitytypen geben darf. Diese Zahl bestimmt
dann die Funktionalitaetsangabe fuer dieses letzte Entity. Nehmen wir also:

* Studenten und Vorlesungen. Dann gibt es fuer einen Student und eine Vorlesung
  nur einen und genau einen eindeutigen Professor. Also wuerden wir neben dem
  Entitytyp *Professor* eine $1$ schreiben.

* Studenten und Professoren. Da ein Student einen Professor in zwei Vorlesungen
  haben *koennte*, bleibt nichts anderes uebrig, als fuer die Vorlesung $N$ als
  Funktionalitaet anzugeben.

* Vorlesungen und Professoren. Natuerlich besuchen viele Studenten eine
  Vorlesung mt einem Professor, also hat der Entitytyp *Studenten* die
  Funktionalitaetsangabe $M$.

Das ER-Diagramm wuerde also so aussehen (ohne Attribute):

```
[Studenten]-M----<hoeren>----N-[Vorlesungen]
                    |
					|
					1
					|
	          [Professoren]
```

Da hier also nur einmal eine $1$ steht, gibt es auch nur eine partielle
Funktion, die wir fuer die Beziehung *hoeren* festlegen koennen:

$$\text{Studenten } \times \text{ Vorlesungen} \rightarrow \text{Professoren}$$

## $(min,max)$-Notation

Bei den Funktionalitaetsangaben musste man, sobald es zu einem Entity bezueglich
einer Beziehung mehr als nur ein anderes passendes Entity gab, eine $N$ als
Funktionalitaet angeben. Diese Notation ist sichtlich nur sehr ungenau. Daher
gibt es noch eine zweite Notation, genannt $(min,max)$-Notation, die eine
verfeinerte Angabe der Funktionalitaeten erlaubt.

Genauer gibt man bei dieser Notation fuer jedes Entity, das an einer Beziehung
teilnimmt, an, wie oft das Entity *mindestens* und wie oft *maximal* teilnehmen
muss. Ist $R$ also wieder eine Relation aus $E_1 \times ... \times E_n$, so gilt
fuer jedes Entity $e_i$ aus der Entitymenge (-typ) $E_i$, dass es mindestens
$min_i$ und maximal $max_i$ Tupel der Form $(...,e_1,...)$ geben muss.

Hierbei gibt es noch zwei Sonderfaelle:

* Kann ein Entity auch gar nicht an einer Beziehung teilnehmen, notieren wir
  eine $0$.

* Kann ein Entity unbeschraenkt oft an einer Beziehung teilnehmen, notieren wir
  das mit einem $*$.

Betrachten wir dazu nun wieder die Beziehung *hoeren* zwischen *Professoren*,
*Studenten* und *Vorlesungen*. Bei der Visualisierung muss man nun gewaltig
aufpassen. Wenn wir bei den Funktionalitaetsangaben eine $1:N$ Beziehung hatten,
dann gaben wir das $N$ auf die Seite des Entitytyps, wofuer es $N$ gab (z.B. $N$
Vorlesungen fuer $1$ Professor). Die Definition der $(min,max)$-Notation ist
jetzt aber so, dass sie angibt, wie oft ein Entity an einer Beziehung
teilnimmt. Das bedeutet bei einer $1:N$-Beziehung beispielsweise, dass jedes
Entity auf der $N$-Seite nur einmal in einem Tupel vorkommt bzw. an der
Beziehung teilnimmt, weil es ja dafuer nur $1$ passendes gegenueberligendes
Entity gibt (nach Deifinition der $1:N$-Funktionalitaet). D.h., gaben wir beim
$N$-Enittytyp vorher ein $N$ an, geben wir hier nun $(1,1)$ an, weil das Entity
ja nur $1$ mal an der Beziehung teilnimmt. Die Angaben sind sozusagen
vertauscht. Man sollte daher so denken (sei hier nur eine binaere Beziehung
betrachtet):

1. Bei den Funktionalitaeten ueberlegt man sich fur ein *konkretes* Entity eines
   Entitytyps, wieviele passende konkrete Entities es vom anderen Typ geben
   kann, und schreibt diese Angabe beim *anderen Enitytyp* hin.

2. Bei der $(min,max)$-Notation ueberlegt man sich, wie oft ein *konkretes*
   Entity eines Entitytyps an einer Beziehung teilnimmt, und schreibt es bei
   diesem Entitytyp hin.

Wir ueberlegen uns hier also:

* Wie oft kan ein konkreter Student an der *hoeren*-Beziehung teilnehmen?
  Beliebig oft, also $(0,*)$.

* Wie oft kann eine konkrete Vorlesung an der *hoeren*-Beziehung teilnehmen?
  Weil eben mindestens drei Studenten aber maximal beliebig viele eine Vorlesung
  hoeren duerfen, gilt hier $(3,*)$.

* Wie oft kann ein konkreter Professor eine Vorlesung hoeren? Zwischen $0$ und
  $10$ mal, also: $(0, 10)$.

Hier nun also die Situation von oben visualisiert:

```
[Studenten]-(0,*)----<hoeren>----(3,*)-[Vorlesungen]
                        |
				     	|
				      (0,10)
					    |
	               [Professoren]
```

## Existenzabhaengige Entityptypen

Es gibt oft Situationen, wo Entities nicht alleine identifizierbar
sind. Z.B. waere der Entitytyp Raum, mit Schluesselattribut *Raumnummer* alleine
nicht ausreichend, um innerhalb der Menge aller Raume einen genau zu
identifizieren. Viele verschiedene Gebaeude koennen natuerlich einen Raum mit
der selben Nummer haben. Daher nennt man ein solches Entity, wie das Entity
*Raum*, ein *schwaches* Entity. Schwache Entities sind immer von anderen,
*starken* Entities abhaengig. Es gilt also:

* Ein schwaches Entity ist oft in seiner ganzen Existenz voll von einem Gebaeude
  abhaengig.

* Ein schwaches Entity ist auch oft nur ueber dem Schluessel seines
  uebergeordneten starken Entities eindeutig identifizierbar.

Es gilt, dass ein schwaches Entity immer nur genau einem starken Entity
zugeordnet sein kann. Es gibt hierbei also nur $1:N$ und $1:1$
Funktionalitaeten, wo es fuer ein starkes viele schwache Entities geben kann,
oder nur genau eines. Bei der $N:M$ Funktionalitaet koennte man nicht genau
festlegen, wievielen starken Entities ein schwaches zugeordnet ist (nur $N$),
und wie dann die Identifikation realisiert werden soll. Man beachte allerdings,
dass diese Einschraenkung nur zwischen dem starken und schwachen Entity
gilt. Ein schwaches Entity kann mit anderen Entities wohl auch $N:M$ Beziehungen
eingehen, weil ein schwaches Entity ja dann in Kombination mit seinem starken
Entity sehr wohl identifizierbar ist.

In einem ER-Diagramm werden schwache Entities und die Beziehungen, ueber welche
sie mit ihrem starken Entity verbunden sind (z.B. *liegt_in* bei Gebaeuden und
Raeumen), doppelt umrandet gezeichnet. Auch wurde gesagt, dass schwache Entities
oft keinen eigenen Schluessel haben. Beispielsweise kann es viele Raeume auf der
Welt mit Nummer $1$ geben, also waere ein Raum mit einer gegebenen Raumnummer
innerhalb der Entitymenge *Raum* nicht eindeutig identifizierbar. Wie gesagt
folgt die Eindeutigkeit erst aus der Raum-Gebaeude Verbindung. Somit wird das
"teil-identifizierende" Attribut des schwachen Entities (z.B. Raumnummer) auch
nicht durchgehend unterstrichen gezeichnet, wie ein Schluessel, sondern nur
gestrichelt unterzeichnet. Ansonsten gibt es keine weiteren Anforderungen.

```
(Raumnummer)
 - - - - -  \
			 [[Raum]]====<<liegt_in>>----[Gebaeude]---(GebaeudeNr)
(Groesse)---/                                         -----------

```

## Generalisierung

Das Konzept der Generalisierung uebertragt die Prinzipien der
Objekt-Orientierung im Software-Entwurf auf die konzeptuelle Ebene der
ER-Diagramme. Bei der Generalisierung werden Eigenschaften, also Attribute,
aehnlicher Entitytypen aus diesen entnommen und einem neuen, uebergeordneten
Entitytyp hinzugefueft. Dieser neue Entitytyp wird dann *Obertyp* genannt, die
Entitytypen die diese Attribute urspruenglich enthielten dann die *Untertypen*
des Obertyps. Attribute, die innerhalb der Entitytypen nicht "faktorisierbar"
sind, bleiben hierbei bei den urspruenglichen Entitytypen (nun Untertypen)
erhalten.

Man beachte, dass die Untertypen dann von ihrem Obertyp *erben*. Das bedeutet,
dass sie implizit alle Attribute des Obertyps enthalten, auch wenn sie nicht
explizit bei den Untertypen angegeben werden, sondern eben nur bei dem
Obertyp. Ebenso sei gesagt, dass der Obertyp auch Entities enthalten kann, die
in keinem Untertypen enthalten sind.

In einem ER-Diagramm werden Obertypen und Untertypen dann durch ein neuartiges
Element vereint. Dieses Element ist ein Sechseck, und wird innen mit *is-a*
beschriftet.

Wir koennen z.B. sagen, dass sowohl Professoren als auch Studenten Menschen
sind, somit also einen Namen und eine Personalnummer besitzen. Ohne
Generalisierung muessten wir nun die Attribute *Name* und *PersNr* beiden
Entitytypen *Student* und *Professor* hinzufuegen. Wir erstellen aber nun einen
generalisierten Obertyp *Mensch*, also ein neuer Entitytyp, und geben die
gemeinsamen Attribute *Name* und *PersNr* dem Entitytyp *Mensch*. Wir verbinden
dann den Obertyp *Mensch* ueber ein *is-a*-Sechseck zu den Untertypen *Student*
und *Professor*. Ein Student hat dabei noch ein Attribut *Semester*, was
offensichtlich nicht jedem Menschen zugeordnet werden kann, und ein Professor
mag noch einen *Rang* haben.

```
(Semester)--[Student] [Professor]--(Rang)
                  \____/
				  /    \
				  |is-a|
				  \____/
				    |
	    (Name)--[Mensch]--(PersNr)
```

Es gibt nun noch zwei Eigenschaften, die Generalisierungen haben koennen. Sie
koennen sein:

* *Disjunkt*: es gibt kein konkretes Objekt, das mehr als einem Untertyp
  zugeordnet werden koennte.

* *Vollstaendig*: der generalisierte Obertyp ist nur die Vereinigung aller
  Untertypen. Wie gesagt kann ein Obertyp ja noch weitere Entities enthalten,
  die keinem der Untertypen zugeordnet werden koennen. Beispielsweise gibt es in
  unserem Universum (im mathematischen Sinne) sicher noch Objekte, die nur
  Mensch, aber weder Professor noch Student sind. Beispielsweise
  Putzkraefte. Eine Putzkraft waere also nur ein Mensch, somit im Obertyp
  *Mensch* enthalten. Gibt es aber nun kein solches Objekt, dass keinem
  Untertypen zugeordnet werden kann, dann sprechen wir von einer vollstaendigen
  Generalisierung (wenn der Obertyp also nur abstrakt ist, im
  programmiertechnischen Sinne).

## Aggregation

War Generalisierung noch die *is-a* Beziehung, ist Aggregation nun die *has-a*
Beziehung. Es gibt gewiss Situationen, wo Entities nicht in einer *is-a*
Beziehung stehen, aber dennoch verbunden sind. Beispielsweise Menschen und
Nasen. Eine Nase ist kein Mensch, und ein Mensch ist keine Nase. Dennoch gibt es
hier wohl irgendeine Verbindung zwischen diesen Objekten.

Man koennte nun eine Beziehung fuer Nasen und Menschen machen: *naseVon*. Aber
oftmals will man nur Ausdruecken, dass man einen Mensch in weitere Entities
dekomposieren kann, ohne explizit Beziehungen erstellen zu muessen. Dafuer gibt
es die *has-a* Beziehung, welche in ER-Diagrammen aber mit *teil-von* bezeichnet
wird. Eine Nase ist sehr wohl *teil-von* einem Menschen.

In einem ER-Diagramm zeichnen wir Aggregation wieder in einer Raute. Das kann
man eben so interpretieren, dass *teil-von* eine generische Beziehung ist
bzw. eine Art Platzhalter.

```
[Nase]--<Teil-von>--[Mensch]
```
