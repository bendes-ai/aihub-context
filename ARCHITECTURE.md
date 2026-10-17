# AIHub – Architektur

Stand: 2026-10-17, aus der Projektdoku (nicht live geprüft).

## Container
| Container | Zweck | Erreichbarkeit |
|---|---|---|
| `AIHub` | Hub (Python, `http.server`, psycopg), Port 3030 (Oberfläche) und 3031 (öffentlicher Listener) | 3030 nur LAN/VPN, 3031 über Reverse Proxy |
| `AIHub-ollama` | lokale Modelle (Ollama 0.40.2) | intern |
| `AIHub-litellm` | einheitlicher Endpunkt für Modelle | intern |
| `AIHub-postgres-memory` | Postgres 16 mit pgvector, das „Gehirn“ | intern |
| `AIHub-searxng` | Metasuche (JSON-API), kein veröffentlichter Port | nur intern |

Gehärtet: `no-new-privileges`, Hub `read_only` mit tmpfs, Log-Rotation, gepinnte Images, Healthchecks mit `depends_on: service_healthy`, Speicherlimits. Ollama: `keep_alive` 24 h, höchstens 3 Modelle geladen, 1 parallele Anfrage.

## Gehirn (Wissensbasis)
- Postgres + pgvector, Hybridsuche. Import aus einem Eingangsordner, Dubletten werden am **Inhalt** erkannt, nicht am Dateinamen.
- Kontextexport mit Token-Budget (`export-context --max-tokens`) und Filter für persönliche Daten (E-Mail, IBAN, Telefon, Adresse, PLZ/Ort, Schlüssel, konfigurierte Namen).
- Ein MCP-Server (stdio, ohne Fremdpakete) für lokale Clients; Schreiben nur mit Bestätigung.
- Nächtliche Dumps (7 täglich, 4 wöchentlich); Restore-Test einmal bestanden.

## Ideen und Sprache (seit 16.10.)
- Per iPhone-Kurzbefehl („Recherche“, „Idee“) per Sprache. Ideen liegen führend in Postgres (Tabellen für Ideen, Senken und Erinnerungs-Log).
- Optionaler Abgleich in Nextcloud-Aufgaben (CalDAV), von dort in Apple Erinnerungen. Ein Job erinnert höchstens einmal je Zeitfenster.
- Fälligkeiten (heute, morgen, Wochentage) werden **per Regel** bestimmt, nicht vom Modell, weil das kleine Modell Wochentage falsch rechnete.
- Sprachweg: Gehirn (nur bei Treffer ab 0,75) → Websuche → Claude-Abo (nur mit laufendem Worker) → zuletzt lokal.

## Remote-MCP (nur Ideen)
Ein eigener Listener bietet vier Werkzeuge: Ideen auflisten, anlegen, erledigen und Projektkontext. Das Gehirn, auch Kinderinhalte, wird nie über diesen Weg angeboten. Authentifizierung über eigene Token; Anfragen mit Browser-`Origin` werden abgelehnt.

## Claude-Anbindung
`claude -p` mit Abo-Anmeldung, ohne API-Schlüssel, nur Text und ohne Werkzeuge. Der Container reicht Aufträge über eine Datei-Queue an einen Worker auf dem Host. Der Worker läuft derzeit als Hintergrundprozess.

## Mac-Collector
Ein launchd-Job auf dem Mac schickt alle 15 Minuten neue Claude-Code-Verläufe der letzten 30 Tage in das Gehirn (Geheimnisse werden vorher geschwärzt).

## Perplexity-Handoff
Der Hub öffnet `https://www.perplexity.ai/search?q=<Frage>`. Bei sensiblen Läufen steht dort statt der Frage der Text „[privat]“. Die Antwort wird manuell zurück in den Hub eingefügt und im Gehirn gespeichert.
