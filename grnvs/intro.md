# Einführung

## ISO/OSI Modell

Das ISO/OSI Modell bezeichnet das *Open Systems Interconnect* (OSI) Modell der
*International Standardization Organization* (ISO). Es unterteilt den
Kommunikationsvorgang im Internet in sieben Schichten, wobei jede Schicht
bestimmte Dienste erbringt. Die Schichten lauten wie folgt (von unten nach oben):

1. Physikalische Schicht (elektromagnetische Signale)
2. Sicherungsschicht (Ethernet, Direktverbindungen)
3. Vermittlungsschicht (IP)
4. Transportschicht (TCP/UDP)
5. Sitzungssschicht (Verbindungssitzungen, z.B. für TCP)
6. Darstellungsschicht (wie Daten Anwendungen präsentiert werden, z.B. JSON)
7. Anwendungsschicht (HTTP, DNS)

Innerhalb dieses Modells sprechen wir von *vertikaler* und *horizontaler*
Kommunikation:

* Vertikale Kommunikation beschreibt den Vorgang, wie Daten zwischen den
  Schichten übermittelt wird. Also beim Senden, die Übertragung der Daten "nach
  unten", und beim Empfangen, das Übertragen der Daten "nach oben".
* Horizontale Kommunikation meint, wie Schichten jeweils miteinander, bei
  Empfänger und Sender, kommunizieren. Beispielsweise spricht man von
  horizontaler Kommunikation, wenn die Vermittlungsschicht Daten sendet, die
  zur Interpretation der Vermittlungsschicht beim Empfänger gedacht sind.

Man beachte, dass das ISO/OSI Modell auch Schwächen hat. So kann man
beispielsweise nicht alle Dienste nur einer Schicht zuordnen, sondern oft
mehreren. Auch widerspricht die Anordnung der Schichten oft anderen Interessen,
beispielsweise der Effizienz der Übertragung.

## Vertikale Kommunikation

Es gibt nun ein Modell für die verschiedenen Bausteine, die Schichten bei der
vertikalen Kommunikation untereinander austauschen. Wir betrachten hierzu die
$N$-te Schicht, welche Informationen aus der vorherigen, höheren $N+1$-ten
Schicht erhält und seine eigenen Informationen an die nächste, niedrigere
$N-1$-te Schicht sendet.

1. Die $N$-Schicht erhält aus der oberen Schicht eine *Interface Data Unit*
   (IDU). Diese besteht aus *Interface Control Information* (ICI) sowie den
   eigentlichen Nutzdaten, der *Service Data Unit* (SDU).
2. Zunächst wird die ICI gelesen, welche Informationen der $N+1$-Schicht für die
   $N$-Schicht enthält. Diese ICI wird danach nicht weiter gesendet, sie ist
   also nur für diese Kommunikation zwischen den zwei Schichten notwendig.
3. Die SDU, also die eigentlichen, interessanten Nutzdaten, werden dann von der
   $N$-Schicht in eine Protocol Data Unit (PDU) verpackt.
4. Mit dazu kommt noch ein *Protocol Control Information* Block,
welcher Daten zur *horizontalen Kommunikation* enthält. Diese sollen also beim
Empfänger auch auf der $N$-Schicht interpretiert werden. Beispielsweise könnte
hier enthalten sein, dass das TCP Protokoll verwendet wird. Dann weiß die
Transportschicht beim Empfänger, dass sie ein TCP und kein UDP Paket entpacken
soll. Diese Information nennt man üblicherweise *Header*.
5. Sobald die PDU aus der SDU und den PCI fertig gebaut wurde, fügt die
   $N$-Schicht noch ihre eigenen ICI für die $N$-Schicht hinzu.
6. Das Resultat ist eine neue IDU für die $N-1$-Schicht.

Es gibt für die PDU auf manchen Schichten spezielle Namen:

* Transportschicht (4): Segment (TCP), Datagramm (UDP)
* Vermittlungsschicht (3): Paket
* Sicherungsschicht (2): Rahmen
