# Ablauf Spiel
Aus Sicht eines Pummel
## Variante 1
1. Pummel auswählen
2. Pummel bewegen
3. Pummel angreifen
Rundenende (Buff aktiviert)
## Variante 2
1. Pummel auswählen
2. Pummel angreifen
3. Pummel bewegen
Rundenende (Buff aktiviert)
## Variante 3
1. Pummel auswählen
2. Pummel bewegen
Rundenende (Buff aktiviert)
## Variante 4
1. Pummel auswählen
2. Pummel angreifen
Rundenende (Buff aktiviert)
## Variante 5
Rundenende (Buff aktiviert)
# Geländefelder
| Geländefeld | Bewegbarkeit | Spezial                                               |
|-------------|--------------|-------------------------------------------------------|
| Wiese       | Alle         | -                                                     |
| Wüste       | Alle         | -                                                     |
| Lava        | Fluffy       | -                                                     |
| Wasser      | Fluffy       | -                                                     |
| Matsch      | Alle         | -                                                     |
| Berg        | Alle         | Über Berg kann nicht geschossen werden                |
| Wiese       | Alle         | Mampfred kann sich bei Schaden auf Feld einmal heilen |
# Spielfiguren
| Pummelz  | Health | Attack | Agility | Attackrange | Ability                                          |
|----------|--------|--------|---------|-------------|--------------------------------------------------|
| Bellie   | 2      | 2      | 1       | 1           | -                                                |
| Sneip    | 1      | 3      | 2       | 3           | -                                                |
| Hoppel   | 3      | 1      | 3       | 2           | -                                                |
| Wolli    | 6      | 1      | 1       | 1           | -                                                |
| Haley    | 5      | 1      | 2       | 1           | Heilt Nachbarn +1 im Umkreis 1                   |
| Buffy    | 5      | 1      | 2       | 1           | Verstärkt Nachbarn +1 im Umkreis 1               |
| Mampfred | 5      | 2      | 1       | 1           | Heilt sich auf Wiese mit Gras                    |
| Spot     | 4      | 1      | 1       | 1           | Alle Truppen Attackrange +1 wenn auf Berg        |
| Chilly   | 4      | 1      | 1       | 1           | Wird zu Killy wenn angegriffen                   |
| Killy    | ?      | 4      | 4       | 4           | Health: 4 - Schaden durch Angriff                |
| Czaremir | 9      | 3      | 1       | 1           | Bei Tod wird eigene Runde übersprungen           |
| Bummz    | 3      | 1      | 1       | 1           | Explodiert bei Tod im Umkreis 1 mit Schaden 1    |
| Link     | 5      | 2      | 2       | 2           | Attacke plus 1 bei bei direkt benachbarten Links |
| Fluffy   | 3      | 2      | 2       | 2           | Kann sich auf allen Geländefeldern bewegen       |
# Rückschlüsse auf Taktik

Töten > Damage

|  Pummel  |                                                     Taktik                                                 |
|----------|------------------------------------------------------------------------------------------------------------|
| Bellie   | Blockiert Zugänge zu anderen Pummelz, bevorzugt hinter Wolli / schräg von Wolli (zweite Verteidigungslinie) |
| Sneip    | Schnell außer Range vom Gegner bringen. Decken mit anderen Einheiten. Wenn mehrere, Assault-Taktik |
| Hoppel   | Spontane Eingreiftruppe. Bietet sich für Hit and Run an |
| Wolli    | Nah zum Gegner, Angriffsfläche bieten, schwache truppen mit körper shielden |
| Haley    | Hinter Wollis in zweiter Reihe platzieren oder hinter Bellie. Wenn Czaremir weniger als 5 Leben hat sofort zu Czaremir |
| Buffy    | In die Nähe von Sneip stellen. Alternativ in zweite Reihe |
| Mampfred | Variabel in Verteidigung und Angriff einsetzbar. Soll bei 2 verbleibenden Leben evaluieren, ob heilen notwendig ist. Wenn ja, soll Mampfred sofort zum nächsten Grasbüschel | 
| Spot     | SOFORT ALLE AUF DEN BERG! (Anderweitig Nutzlos, kombinieren mit Sneips in Ecken um Fett weit zu schießen)  |
| Chilly   | Auf Feld in Reichweite mit geringem Schaden gehen (idealerweise 1). 
| Killy    | Wenn Killy gerade erst entstanden ist, dann schießt Killy und rennt weg. Wenn Bummz vorhanden, gehe in Attackrange und greiße Bummz an. Priorität hat Bummz mit den meisten Gegenern und wenigsten Freunden in der Nähe |
| Czaremir | Offensiv stark mit range boost, sofort zurückziehen wenn Kill nächste Runde möglich wäre |
| Bummz    | Weit weg von friendlys. Notfalls direkt neben den Gegner Stellen. Im Endgame auf Range 1 Units zulaufen |
| Link     | Idealerweise alle Clustern, Mittiger Link macht am meisten Damage. Freien Weg für Schussbahn einplanen (evtl. Special Tactic für Link Map) |
| Fluffy   | Nur für eine Karte bis jetzt. Hit and Run auf unzugänglichen Feldern. Soll außerhalb von Sneips Range Runde beenden |

# Gesamtkonzept
Reihenfolge der Initialisierung der Felder
![Alt-Text](/images/Unbenannt.png)

# Implementierung
## Spielfeld verarbeiten
## Züge erstellen
## Züge abspeichern
uint (unsigned 32 Bit) Container für 25 Bit Datenspeicher.
Erste Zeile gibt Feldnamen an
Zweite Zeile Speichergröße
Dritte Zeile Operation(en) zum erhalten der Werte
| A/M        | oldX              | oldY              | newX              | newY              | priority     |
|------------|-------------------|-------------------|-------------------|-------------------|--------------|
| 1 Bit      | 4 Bit             | 4 Bit             | 4 Bit             | 4 Bit             | 8 Bit        |
| & 0x100000 | & 0x0F00000 >> 16 | & 0x00F0000 >> 12 | & 0x000F000 >> 8  | & 0x0000F00 >> 4  | & 0x00000FF  |
- A/M: Art des Befehls(Angriff oder Movement)
- oldX: Position vor Aktion in x Richtung
- oldY: Position vor Aktion in y Richtung
- newX: Zielposition der Aktion in x Richtung
- newY: Zielposition der Aktion in y Richtung
- priority: Spielerinterner Zugablauf

Sonderfälle:
- Figur präferiert Zugvariante 5: Bit nach A/M = 1 & alte Position = neue Position
- Figur präferiert Zugvariante 3: A/M = 1 & alte Position = neue Position
- Figur präferiert Zugvariante 4: A/M = 0 & alte Position = neue Position

Liste mit uint wird nach Vergleich des letzten Bytes (Big-Endian Verarbeitung) sortiert (priority1 < priority2 < priority3 usw.)

## Züge ausführen
