### Pipelining
+ Bearbeitung eines Objekts wird in Teilschritte zerlegt (Phasen der Pipeline)
+ jede Einheit genau eine spezielle Teiloperation ausfuehrt

*Pipeline* - Gesamtheit der Verarbeitungseinheiten
+ ideal benoetig ein Befehl k Takte
+ k Befehle gleichzeitig in Verareitung

*Pipeline-Stufe* - eine Verarbeitungseinheit, durch Pipeline-Register von anderen Stufen getrennt

*Latenz* - Zeit, die ein Befehl zum durchlaufen der Pipeline benoetigt (im Idealfall bei k Stufen: L = k)

+ Im Idealfall benoetigt jede Stufe genau einen Takt, bleibt immer gefuellt

Aber in der realitaet:
- Pipeline-Hemmnisse
- Stufenlaufzeit durch Auslesen/Laden der Pipeline-Register verlaengert
- Takt beruecksichtigt Maximum (Stufenlaufzeit ungleich)
- Pipeline-Einschwungzeit

* Zykluszeit, Taktzyklus: abhaengig vom langsamsten Pipelinestufe

Gleitkommaverarbeitung wird in weiter Stufen zerlegt (+mehrere Gleitkommarechenwerke)

Problem: alle Befehle muessen alle Stufen der Pipeline durchlaufen, obwohl manche das gar nicht benoetigen.

Oft in der Praxis werden nicht unterschiedliche Resourcen bei
Pipelinestufen genutzt und das fuehrt zu *Pipelinekonflikten*. Und das
fuehrt dazu, dass die Pipeline nicht immer gefuellt bleibt.

Wenn eine Instruktion angehalten wird, werden auch alle Befehle, die
nach dieser Intruktion zur Ausfuehrung angestossen wurden,
angehalten. ( durch einfuegen von NOP Operationen )

*Datenkonflikte* - eine Instruktion benoetigt das Ergebnis einer
vorangehenden und noch nicht abgeschlossenen Operation in der
Pipeline.

*Steuerkonflikte* - treten bei Verzweigungsbefehlen auf.

Zwischen datenabhaengigen Befehlen koennen Datenkonflikte auftreten,
wenn sie so nahe im Programm stehen, dass ihre Ueberlappung innerhalb
der Pipeline die Zugriffsreihenfolge auf die Register veraendern
wuerde.

Konfliktaufloesung: Erkennen von Konflikten und Aufloesen des
Konflikts durch Anhalten der Pipeline.

Steuerkonflikteaufloesung: Vorhersage, ob der Sprung genommen
wird. Und fortfahren bei korrekter Vorhersage, sonst Verwerfen der
geholten Befehle.

+ Statische und dynamische Sprungvorhersage

```asm
div f0, f2, f4
add f10, f0, f8
load f12, f8, 10
```

Nachteil des bisherigen Konzepts: die Ladeoperation ist unabhaegig,
wird aber trotzdem blockiert.

### Superskalarprinzip

Mehrere Maschinenbefehle koennen gleichzeitg ausgefuert werden.

Superskalare Architektur betont die gleichzeitige Ausfuehrung von
skalaren Operationen im Unterschied zur parallelen Ausfuehrung von
Vektoroperationen auf Vektorrechnern.

*Thread* ist ein eingenstaendiger Handlungsfaden innerhalb eines
Programms. Sie teilen sich alle saemtlichen Systemressourcen mit
Ausnahme von Prozessorregister.

+ Wieso benutzt man das? - Wenn aktueller Thread blockiert ist, koennen
andere Threads weiter ausgefuehrt werden und bessere Ausnutzung der
Funktionseinheiten.

- Mehrere Registersaetze, Steuerung des Threadwechsels.

* Ansaetze fuer Multithreading:

- *Cycle-by-cycle Interleavin* der Prozessor waehlt in jedem Takt einen
  der ausfuehrungbereiten Threads aus.
- *Block Interleaving* Befehle eines Threads so lange ausgefuehrt, bis
  ein Befehl mit langer Latenzzeit ausgefuehrt wird, dann wechsel.
  

