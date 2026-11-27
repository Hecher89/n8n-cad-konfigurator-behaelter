# n8n-cad-konfigurator-behaelter
Automatisierter CAD-Konfigurator für Behälter: Kundenmaße werden per n8n-Formular erfasst, in FreeCAD parametrisch umgesetzt und als STEP, STL und PDF-Zeichnung ausgegeben. Dazu Notion-Datenbank und KI-gestützte Angebotserstellung. Abschlussprojekt der IHK-Weiterbildung zum KI-Integrationsmanager.

# CAD-Konfigurator für Behälter Architektur im Detail

Abschlussprojekt der IHK-Weiterbildung zum KI-Integrationsmanager.
Alle verwendeten Kunden- und Produktdaten sind fiktiv, orientieren sich aber an realen Fertigungsabläufen.

## Ziel

Aus den Wunschmaßen eines Kunden soll automatisch ein fertigungsnahes Paket entstehen:
ein parametrisches CAD-Modell eines Behälters (Boden, Mantel, Deckel), eine bemaßte
technische Zeichnung und ein Angebot mit Preis. Der manuelle Aufwand für Konfiguration,
Konstruktion und Angebotserstellung soll durch einen durchgängigen n8n-Workflow ersetzt werden.
Das Projekt zeigt, wie KI sinnvoll in einen bestehenden Prozess integriert wird, ohne die
technisch kritischen Berechnungen dem Zufall zu überlassen.

## Prozessablauf

1. **Eingabe:** Der Kunde gibt seine Wunschmaße über ein n8n-Formular ein (entscheidend ist das Innenmaß).
2. **Stammdaten:** Ein passender Standardbehälter und die Preisbasis werden aus einer Notion-Datenbank geladen.
3. **Prüfung:** Regelbasierte Plausibilitätsprüfung der Maße (z. B. Wandstärke, Passung des Deckels).
4. **Konstruktion:** Ein Python-Skript erzeugt in FreeCAD 1.1 (headless) das Modell mit den neuen Maßen.
5. **Ausgabe:** STEP, STL und eine PDF-Zeichnung mit Bemaßung und Schnittdarstellung.
6. **Angebot:** Preisberechnung und KI-gestützter Angebotstext.
7. **Freigabe:** Der Kunde prüft die Maße anhand der PDF-Zeichnung. Mit seiner Bestätigung gilt der Auftrag als angenommen.
8. **Ablage:** Alle Ergebnisse und der Auftragsstatus werden in Notion gespeichert.

## Verarbeitete Inhalte

- **Eingangsdaten:** Kundenname und Kontakt, Innenmaße, weitere Konfigurationsparameter
- **Stammdaten:** Standardbehälter, Preise, Konstruktionsregeln (Notion-Datenbank)
- **Erzeugte Daten:** CAD-Modell (STEP, STL), technische Zeichnung (PDF), Stückliste, Angebot, Auftragsdatensatz

## Architektur

| Komponente | Aufgabe |
|---|---|
| n8n (selbst gehostet, Docker) | Steuerung des gesamten Workflows, Formular, Verzweigungen, Dateiausgabe |
| FreeCAD 1.1 (FreeCADCmd) | Parametrische Konstruktion und Zeichnungsableitung |
| Pförtner-Skript | Schnittstelle zwischen n8n und FreeCAD: nimmt Parameter entgegen, startet FreeCAD, liefert Dateien zurück |
| Notion-Datenbank | Produktstammdaten, Preise, Auftragsablage |
| KI-Modell (lokal oder Cloud) | Angebotstext, Hinweise bei auffälligen Maßen |
| Claude | Entwicklungsunterstützung (Brainstorming, Code-Segmente) |

**Prinzip:** Geometrie und Preise werden regelbasiert und nachvollziehbar berechnet.
KI kommt dort zum Einsatz, wo Sprache und Bewertung gefragt sind. Der Mensch gibt
Ergebnisse frei, bevor sie das Haus verlassen.

## Rahmen

- Datenschutz: Entscheidung zwischen lokalem und Cloud-Modell wird dokumentiert
- Live-Demo: Der Workflow läuft vollständig auf dem eigenen Laptop
- Ausbaustufen (nicht Teil der Abschlussarbeit): zweiteiliger Mantel für große Behälter, Kaufteile

## Status

In Entwicklung. Prototyp: n8n-Formular, FreeCAD-Anbindung über das Pförtner-Skript und
Ausgabe als STEP und STL sind an einem Testkörper nachgewiesen. Behälter-Konstruktion,
Zeichnung und Angebot folgen.
