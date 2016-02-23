# Transaktionen

Eine *Transaktion* ist in einem Datenbanksystem eine Einheit atomarer
Operationen, die einen Beginn und ein Ende hat. Diese Operationen sind dabei
Lese- oder Schreibvorgaenge auf Daten, die durch das Datebanksystem verwaltet
werden. Die Buendelung von Operationen erleichtert die Fehlerbehandlung sowie
die Synchronisation vieler Benutzer.

## Aufbau

Eine Transaktion besteht also aus einer Menge von elementaren Operationen, die
wir mit `read` und `write` bezeichnen. Dann gibt es aber auch noch eine Reihe
von "Meta"-Befehlen, die sich auf die Verwaltung der Transaktion selbst
beziehen. Diese Befehle sind:

* *BOT*: Begin of Transaction. Mit diesem (oftmals impliziten) Befehl wird dem
   DBMS mitgeteilt, dass nun eine neue Transaktion begonnen werden soll.

* *commit*: Beende die Transaktion erfolgreich. Es sollen alle nach BOT
  gefolgten Operationen festgeschrieben werden.

* *abort*: Breche die Transaktion ab. Hierbei sollen alle bisher
  Operationen rueckgaengig gemacht werden.

In modernen DBMS gibt es dann noch das Konzept von *Savepoints*
bzw. *Sicherungspunkten*. Ein Sicherungspunkt kann nach einem BOT einen
Zeitpunkt waehrend der Transaktion fixieren, zu welchem man danach rueckkehren
kann. Man muss dann also nicht die gesamte Transaktion zuruecksetzen, sondern
nur bis zum letzten Sicherungspunkt (wenn man das moechte). Hierfuer gibt es die
folgenden Befehle:

* *define savepoint*: Fixiere den momentanen Zeitpunkt als einen
  Sicherungspunkt.

* *backup transaction*: abort bis zum letzten Sicherungspunkt. In manchen DBMS
  ist es auch moeglich, anzugeben, zu welchem Sicherungspunkt man zurueckkehren
  moechte.

### SQL

Da Transaktionen ein grundlegendes Konzept von Datenbanken sind, wird es
natuerlich auch von SQL unterstuetzt. Hierfuer gibt es die folgenden Befehle:

* `begin`: Starte eine neue Transaktion. Nicht alle DBMS benoetigen diese
  Anforderung explizit, sondern starten sie einfach nach dem ersten
  Statement. PostgreSQL benoetigt `begin`.

* `commit`: Fuehrt fuer die Transaktion einen commit durch.

* `rollback`: Wie `abort`.

## Eigenschaften

Die Eigenschaften, die Transaktionen bzw. Transaktionssysteme haben sollten,
werden unter der Abkuerzung *ACID* zusammengefasst. *ACID* steht hierbei fuer:

1. *Atomicity*: Eine Transaktion muss entweder vollstaendig erfolgreich
   abschliessen, oder gar nicht. Wenn die Transaktion erfolgreich *commit*-ed
   wird, muss sie auch vollstaendig erfolgreich sein; wenn die Transaktion nicht
   vollstaendig ist, also z.B. manuell oder durch das System `abort`ed wird,
   oder wenn ein Systemfehler (Stromausfall) passiert, dann muss die Transaktion
   auch vollstaendig scheitern. Vollstaendig heisst hier immer relativ zum BOT.

2. *Consistency*: Das endgueltige Resultat einer Transaktion muss immer
   konsistent sein. Zusammen mit Atomaritaet bedeutet das also, dass eine
   abgeschlossene Transaktion immer alle Konsistenzbedingungen fuer den
   jeweiligen Datensatz erfuellen muss, wenn die Daten dann permanent
   gespeichert werden. Konsistenzbedingungen koennen hierbei referentielle
   Integritaet sein, oder andere `check`s.

3. *Isolation*: Isolation bezieht sich auf die Thematik der
   Mehrbenutzersynchronisation. Konkret bedeutet *Isolation*, dass wenn mehrere
   Transaktionen parallel vom System behandelt werden, diese das nicht merken
   duerfen. Jede Transaktion muss also so ausgefuehrt werden, als ob es die
   gesamte Datenbasis fuer sich alleine haette.

4. *Durability*: Die Wirkung einer abgeschlossenen Transaktion muss dauerhaft in
   der Datenbank erhalten bleiben. D.h., das DBMS ist dafuer zustaendig, dafuer
   zu sorgen, dass egal welche Fehler auftreten, eine abgeschlossene Transaktion
   nach einem `commit` __immer__ erhalten bleibt.

Man beachte fuer (2), dass die Konsistenz der Datenbasis erst nach Abschluss
einer Transaktion gegeben sein muss. *Waehrend* der Transaktion sind
Inkosistenzen erlaubt. Sonst waeren zyklische Konsistenzbedingungen auch gar
nicht behandelbar. Z.B. hat Relation $A$ einen Fremdschluessel auf Relation $B$
und umgekehrt. Muessten Konsistenzbedingunen auch waehrend einer Transaktion
noetig sein, dann koennte man z.B. nicht in Relation $A$ einen Fremdschluessel
auf ein Tupel in $B$ einfuegen, dass es noch nicht gibt. Denn um dieses Tupel in
$B$ einzufuegen muesset man erst das passende in $A$ einfuegen -- ein Kreis, der
immer *momentaer* eine Inkonsistenz herbeifuehren wuerde.
