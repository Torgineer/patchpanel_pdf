# Patchpanel-Portplan

Python-Skript zum Erstellen eines übersichtlichen Patchpanel-Portplans als PDF. Die Anschlussbelegung wird direkt in der Python-Datei gepflegt.

## Funktionen

- Tabelle mit den Spalten **Port**, **Ziel**, **PoE** und **Bemerkung**.
- A4-Hochformat als Standard, optional A4-Querformat.
- Aktuelles lokales Datum und Uhrzeit in der PDF.
- Optionaler Zeitstempel im Dateinamen.
- An das Seitenformat angepasste Spaltenbreiten.
- Automatischer Zeilenumbruch für längere Einträge.
- Wiederholung des Tabellenkopfs auf Folgeseiten.
- Seitenzahlen in der Fußzeile.
- Abwechselnde Zeilenfarben zur besseren Lesbarkeit.

## Voraussetzungen

- Python 3
- Python-Paket `reportlab`

## Installation

Öffne ein Terminal im Projektordner und installiere die Abhängigkeit:

```bash
python -m pip install reportlab
```

Unter Windows kannst du alternativ den Python-Launcher verwenden:

```powershell
py -m pip install reportlab
```

## Verwendung

1. Öffne `patchpanel_portplan.py`.
2. Passe die Einstellungen und die Liste `PORTS` an.
3. Speichere die Datei und starte das Skript:

```bash
python patchpanel_portplan.py
```

Unter Windows alternativ:

```powershell
py patchpanel_portplan.py
```

Die PDF wird im Unterordner `output` neben der Python-Datei gespeichert. Der Ordner wird bei Bedarf automatisch angelegt. Nach erfolgreicher Erstellung gibt das Skript den vollständigen Pfad zur PDF im Terminal aus.

## Portbelegung konfigurieren

Die Liste `PORTS` enthält pro Port einen Eintrag mit vier Werten:

```python
# (Port, Ziel, PoE, Bemerkung)
PORTS = [
    (1, "Wohnzimmer", "Nein", "TV-Anschluss"),
    (2, "Schlafzimmer", "Nein", ""),
    (3, "Flur", "Ja", "Access Point"),
    (4, "", "", ""),
]
```

| Feld | Bedeutung | Beispiel |
| --- | --- | --- |
| Port | Portnummer auf dem Patchpanel | `1` |
| Ziel | Raum, Anschluss oder angeschlossenes Gerät | `"Wohnzimmer"` |
| PoE | Manuell eingetragene PoE-Angabe | `"Ja"`, `"Nein"` oder `""` |
| Bemerkung | Freitext für zusätzliche Informationen | `"Access Point"` |

Die Reihenfolge der Einträge bestimmt die Reihenfolge in der PDF. Weitere Ports lassen sich durch zusätzliche Einträge ergänzen. Jeder Eintrag muss genau vier Werte enthalten.

Die mitgelieferten Belegungen sind Beispiele. Das Skript übernimmt die Angaben unverändert; es erkennt oder prüft keine Netzwerkverbindungen oder PoE-Versorgung.

Für einen manuellen Zeilenumbruch innerhalb einer Zelle verwende `\n`:

```python
(5, "Arbeitszimmer", "Nein", "Doppeldose\nAnschluss links"),
```

## Einstellungen

Die Konfiguration befindet sich oben in der Python-Datei.

### Titel

```python
TITEL = "Netzwerkschrank – Patchpanel-Portplan"
```

### Seitenformat

```python
HOCHFORMAT = True
```

- `True`: A4-Hochformat.
- `False`: A4-Querformat.

Beim regulären Skriptstart wird dieser Wert an die Funktion übergeben:

```python
if __name__ == "__main__":
    erstelle_pdf(hochformat=HOCHFORMAT)
```

Bei einem direkten Funktionsaufruf ist Hochformat der Standard:

```python
erstelle_pdf()                  # A4-Hochformat
erstelle_pdf(hochformat=False)  # A4-Querformat
```

### Datum und Uhrzeit

Der Zeitstempel wird bei jedem Funktionsaufruf neu erzeugt. Die sichtbare Datumszeile verwendet folgendes Format:

```text
Stand: 26.10.2026 – 17:25 Uhr
```

Das Format wird innerhalb von `erstelle_pdf()` festgelegt:

```python
unterzeile = erstellt_am.strftime("Stand: %d.%m.%Y – %H:%M Uhr")
```

### Dateiname

```python
ZEITSTEMPEL_IM_DATEINAMEN = True
```

Mit `True` enthält der Dateiname Datum und Uhrzeit einschließlich Sekunden:

```text
patchpanel_portplan_2026-10-26_17-25-00.pdf
```

Mit `False` wird immer derselbe Dateiname verwendet:

```text
patchpanel_portplan.pdf
```

Eine vorhandene Datei gleichen Namens wird überschrieben. Auch mit aktiviertem Zeitstempel können zwei Aufrufe innerhalb derselben Sekunde denselben Dateinamen erzeugen.

### Ausgabeordner

```python
AUSGABEORDNER = Path(__file__).resolve().parent / "output"
```

Standardmäßig liegt der Ausgabeordner neben dem Skript. Für einen anderen Ordner passe diese Variable an.

## Projektstruktur

```text
patchpanel-portplan/
├── .gitignore
├── patchpanel_portplan.py
├── README.md
└── output/                     # Wird beim Ausführen angelegt
    └── .gitignore
```
