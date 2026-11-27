# CAD-Konfigurator für Behälter

Abschlussprojekt der IHK-Weiterbildung zum KI-Integrationsmanager.
Alle verwendeten Kunden- und Produktdaten sind fiktiv, orientieren sich aber an realen Fertigungsabläufen.

## Ziel

Aus den Wunschmaßen eines Kunden soll automatisch ein fertigungsnahes Paket entstehen:
ein parametrisches CAD-Modell eines Behälters (Boden, Mantel, Deckel), eine bemaßte
technische Zeichnung, eine Stückliste und ein Angebot mit Preis. Der manuelle Aufwand
für Konfiguration, Konstruktion und Angebotserstellung soll durch einen durchgängigen
n8n-Workflow ersetzt werden.
Das Projekt zeigt, wie KI sinnvoll in einen bestehenden Prozess integriert wird, ohne die
technisch kritischen Berechnungen dem Zufall zu überlassen.

## Prozessablauf

1. **Eingabe:** Der Kunde gibt seine Wunschmaße über ein n8n-Formular ein (entscheidend ist das Innenmaß).
2. **Stammdaten:** Ein passender Standardbehälter und die Preisbasis werden aus einer Notion-Datenbank geladen.
3. **Prüfung:** Regelbasierte Plausibilitätsprüfung der Maße (z. B. Wandstärke, Passung des Deckels).
4. **Konstruktion:** Ein Python-Skript erzeugt in FreeCAD 1.1 (headless) das Modell mit den neuen Maßen.
5. **Ausgabe:** STEP, STL und eine PDF-Zeichnung mit Bemaßung und Schnittdarstellung.
6. **Stückliste:** Aus den Konfigurationsdaten wird eine Stückliste mit allen Bauteilen erzeugt.
7. **Angebot:** Preisberechnung und KI-gestützter Angebotstext.
8. **Freigabe:** Der Kunde prüft die Maße anhand der PDF-Zeichnung. Mit seiner Bestätigung gilt der Auftrag als angenommen.
9. **Ablage:** Alle Ergebnisse und der Auftragsstatus werden in Notion gespeichert.

## Verarbeitete Inhalte

- **Eingangsdaten:** Kundenname und Kontakt, Innenmaße, weitere Konfigurationsparameter
- **Stammdaten:** Standardbehälter, Preise, Konstruktionsregeln (Notion-Datenbank)
- **Erzeugte Daten:** CAD-Modell (STEP, STL), technische Zeichnung (PDF), Stückliste, Angebot, Auftragsdatensatz

## Verwendete Tools

- **n8n** (selbst gehostet über Docker): Workflow-Steuerung
- **FreeCAD 1.1** (FreeCADCmd): parametrische Konstruktion und Zeichnungsableitung
- **Python**: Konstruktions- und Pförtner-Skripte
- **Notion**: Datenbank für Stammdaten, Preise und Aufträge
- **Gmail**: Übertragung der Daten, Freigabeprozess
- **KI-Modell** (lokal oder Cloud): Angebotstext und Hinweise bei auffälligen Maßen
- **Claude**: Brainstorming und Code-Segmente während der Entwicklung
- **GitHub**: Versionierung und Projektdokumentation
- **Eigener Laptop** mit Kamera und Mikrofon für die Live-Demo

## Architektur

| Komponente | Aufgabe |
|---|---|
| n8n (selbst gehostet, Docker) | Steuerung des gesamten Workflows, Formular, Verzweigungen, Dateiausgabe, Stückliste |
| FreeCAD 1.1 (FreeCADCmd) | Parametrische Konstruktion und Zeichnungsableitung |
| Pförtner-Skript | Schnittstelle zwischen n8n und FreeCAD: nimmt Parameter entgegen, startet FreeCAD, liefert Dateien zurück |
| Notion-Datenbank | Produktstammdaten, Preise, Auftragsablage |
| Gmail | Kundenkommunikation und Datentransfare |
| KI-Modell (lokal oder Cloud) | Angebotstext, Hinweise bei auffälligen Maßen |
| Claude | Entwicklungsunterstützung (Brainstorming, Code-Segmente) |

**Prinzip:** Geometrie und Preise werden regelbasiert und nachvollziehbar berechnet.
KI kommt dort zum Einsatz, wo Sprache und Bewertung gefragt sind. Der Mensch gibt
Ergebnisse frei, bevor sie das Haus verlassen.

## Workflow Ablauf

Geplante technische Abfolge in n8n (wird im Verlauf an die Umsetzung angepasst):

    Formular (Maße) 
      → Notion: Stammdaten laden 
      → Code-Node: Plausibilitätsprüfung 
      → Verzweigung: Maße in Ordnung? 
           nein → Fehlermeldung an den Anwender
           ja   → HTTP-Request an das Pförtner-Skript 
      → FreeCAD: Modell erzeugen (STEP, STL, PDF-Zeichnung) 
      → Stückliste erzeugen 
      → KI-Node: Angebotstext 
      → Dateien bündeln und ausgeben 
      → Freigabe durch den Kunden 
      → Notion: Ergebnisse und Auftragsstatus speichern

## Herausforderungen

Bereits gelöst:

- **Anbindung von n8n an FreeCAD:** n8n und FreeCAD sind getrennte Programme. Gelöst durch ein Pförtner-Skript, das n8n per HTTP-Request anspricht und FreeCADCmd startet.
- **Dateiausgabe in n8n:** Das erzeugte Modell kam zunächst als namenlose Binärdatei (`data.bin`) an. Gelöst durch eine saubere Benennung (z. B. `wuerfel.stl`).
- **Mehrere Ausgabeformate:** STL allein ist für die CAD-Weiterverarbeitung wenig nützlich. Zusätzlich wird STEP erzeugt, beide Dateien werden über einen Compression-Node als ZIP ausgegeben.

In Bearbeitung:

- Automatische PDF-Zeichnung mit Bemaßung und Schnittdarstellung
- Format und Inhalt der Stückliste
- Plausibilitätsregeln für die Maße

## Lernerfahrung

Zwischenstand, wird laufend ergänzt:

- Wie zwei unterschiedliche Systeme (Workflow-Tool und CAD-Programm) über eine definierte Schnittstelle zusammenarbeiten
- Warum Dateiformate zweckgebunden gewählt werden müssen (STL für Druck und Vorschau, STEP für CAD-Weiterverarbeitung)
- Wo KI im Prozess sinnvoll ist (Sprache, Bewertung) und wo regelbasierte Berechnung besser ist (Geometrie, Preise)

## Rahmen

- Datenschutz: Entscheidung zwischen lokalem und Cloud-Modell wird dokumentiert
- Live-Demo: Der Workflow läuft vollständig auf dem eigenen Laptop
- Ausbaustufen (nicht Teil der Abschlussarbeit): zweiteiliger Mantel für große Behälter, Einbindung von Kaufteilen

## Status

In Entwicklung. Prototyp: n8n-Formular, FreeCAD-Anbindung über das Pförtner-Skript und
Ausgabe als STEP und STL sind an einem Testkörper nachgewiesen. Behälter-Konstruktion,
Zeichnung und Angebot folgen.

Offen: Format und Inhalt der Stückliste sind noch festzulegen.
