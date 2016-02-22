# Grundlagen Datenbanken -- 03.02.2016

Transaktionen werden parallel ausgefuehrt, weil es so lange dauert, auf den
Plattenspeicher zu warten (kann in der Zwischenzeit auf einer anderen
Transaktion weiterarbeiten). Auf Einzelkernrechnern musste das mit Time-Slicing
passieren (scheduling), aber bei Mehrkernrechnern heute kann wirklich parallel
arbeiten.

Das Ziel ist die Serialisierbarkeit bzw. Isolation. Dem Benutzer vorgaukeln,
dass jede Transaktion seriell passiert. Die parallele Ausfuehrung muss
aequivalent zu einer parallelen Ausfuehrung sein.

Phantomprobleme: Wenn die selbe Aktion waehrend einer Transaktion andere
Ergebnisse produziert (weil eine andere Transaktion etwas gemacht hat). Sperren
helfen da nicht, weil andere Transaktionen unabhaengig sind.

Die Reihenfolge von lesen/writen ist egal, wenn es sich um verschiedene Daten
handelt. Es sind nur Situationen gefaehrlich, wo zwei Transaktionen das selbe
Datum bearbeiten, und *mindestens* einer der beiden ein Schreibzugriff ist. Es
ist auch egal, wenn zwei Transaktionen das selbe Datum lesen, da es bei beiden
immer das selbe sein wird.

Die Historie ist die Vereinigung der Transaktionen. $\bigcup_{i=1}^n T_i$

$<$ ist eine partielle Ordnung. Fuer Konfliktoperationen ist sie total.

Die Vorher-Relation fur einzelne Transaktionen sind alle Untermengen der
Vorher-Relation bezueglich der Historie. $p <_H q, \forall p,q$ wenn $p,q$
konfliktierend.

Zwei Historien $H,H'$ sind aequivalent $H \equiv H'$, wenn die Reihenfolge der
Konfliktoperationen gleich ist (nicht unbedingt nacheinander, aber die selbe
Operation der Konfliktoperation ist irgendwo vor der anderen Operation).

Eine Historie ist seriellisierbar, wenn man die nichtkonfliktierenden
Operationen vertauschen kann,  sodass man daraus eine serielle Historie
erstellen kann.

$r_1(A) \rightarrow r_2(C) \rightarrow w_1(A)$ vertauschbar zu,
$r_1(A) \rightarrow w_1(A) \rightarrow r_2(A)$ also seriellisierbar.

Eine Historie ist seriliellsierbar wenn der Seriellisierbarkeitsgraph azylisch
ist.

Topologische Sortierung eines Graphen: Man kann die Knoten auf einer Achse
malen, wenn er azyklisch ist. Man malt dabei immer den Knoten, der keine
eingehenden Kanten hat, als naechstes auf die Achse. Dann entfernt man alle
ausgehenden Kanten von diesem Knoten. Dann muss es mindestens einen weiteren
Knoten ohne eingehende Kanten geben.

Jede topologische Sortierung entrspicht einem seriellen Schedule.

$T_i$ liest von $T_j$: $T_i$ liest ein Datum nachdem $T_j$ es geschrieben hat.

Eine Ruecksetzbare Historie ist eine, wo die schreibende Transaktion vor der
lesenden comittet. Sonst, wenn die schreibende ein Abort macht oder ein undo,
wird das geschriebene Datum wieder rueckgemacht und die lesende Transaktion hat
vielleicht comittet, nachdem es etwas veraendert hat.

ACA = Avoiding Cascading Abort

Eine historie ist strikt, wenn zwischen einem write und einer beliebigen anderen
Operation entweder ein abort oder ein commit erfolgen muss.

SR: Seriellisierbare Historien
RC: Ruecksetzbare Historien
ACA: Historien ohne kasksadierendes Ruecksetzen
ST: Strikte Historien

Meist nur strike Historien und seriellisierbarein echten Datenbanksystemen.

Concurrency Control.

Scheduling arbeitet mit Sperren. Dabei kann es zu Deadlocks kommen. Dann muss
eine Transaktion "geopfert", also aborted werden.

Es gibt zwei Sperrmodi: S(shared bzw. read lock) und X(exclusive bzw. write
lock)

Eine read-lock wird zum lesen angefordert und wird immer erlaubt, wenn es keine
lock gibt oder nur eine read-lock auf dem Datum liegt (mehrere Transaktionen
duerfen lesen). Aber es darf nicht read-lock gegeben werden, wenn gerade eine
write-lock auf dem Datum liegt. write-lock wird nur gegeben, wenn gerade weder
einew write- noch eine read-lock auf dem Datum liegt (also nur no-lock).

2-Phase-locking kann ACA nicht verhindern.

Strenges 2PL: alle Sperren die eine Transaktion macht werden bis zum Ende der
Transaktion (commit/abort) erhalten. Dadurch kann nie eine andere Transaktion
dirty data lesen (wenn es liest nachdem eine andere Transaktion ihre write-lock
geloest hat).

Beim strengen 2PL kann es leicht zu Deadlock kommen.

Beziehung zwischen Wartegraph und Seriellisierbarkeitsgraph: Wenn es eine Kante
im Wartegraphen gibt, gibt es die umgekehrte Kante im
Seriellisierbarkeitsgraphen. Denn wenn $T_1$ auf $T_2$ wartet, kommt $T_1$ nach
$T_2$ im Seriellisierbarkeitsgraph. Wenn der Wartegraph azyklisch ist, dann auch
der Seriellisierbarkeitsgraph. Durch DFS kann man Zyklen im Wartegraphen suchen.

Die Commit-Reihenfolge bestimmt die Seriellisierbarkeits-Reihenfolge. Wenn $T_1$
vor $T_2$ commitet, dann gilt $T_1 | T_2$ (topologische Ordnung bezueglich der
Seriellisierbarkeit).

## Zentraluebung

ER-Diagramme, Fukntionalitaeten, Min-Max Angaben, Uebersetzung ER <-> Relational

Stichworte: Schema, Auspraegung, Tupel, Attribute

Relationale Algebra
Tupelkalkuel, kein Domaenenkalkuel

SQL -> 1/3 der Punkte, keine Rekursion, trigger

Relationale Entwurfstheorie
FDs, Armstrong axiome, FD, Kannonische Ueberdeuckung

Normalformen

Physische Datenorgansiation
Speicherhierarchie
HDD/RAID
TID-Konzept
Kein R-Baum

Kanonische Uebersetzung SQL -> Relationale Algebrabaum

Transaktionsverwaltung
BOT, read, write, commit, abort
ACID kennen
keine Kostenmodelle

steal/no-steal, Protokollierung

nichts von Mehrbenutzersynchronisation
