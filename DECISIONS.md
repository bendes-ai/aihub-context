# AIHub – Entscheidungen (Auszug)

Format: Entscheidung, Begründung. Stand 2026-10-17, gekürzt aus der Hub-Doku. Reihenfolge nach Datum.

## 2026-10-12 – Migration und Härtung
- **Von `lokaleki` zu `aihub`**: Compose-Projekt umbenannt, die bestehenden Daten-Volumes weiterverwendet (kein Datenverlust, keine Kopie).
- **Eigenes Netzwerk** statt des alten; das UGREEN-OpenClaw wird nur per URL angesprochen.
- **Hub als Image** statt Quellcode-Mount: reproduzierbar, keine Installation beim Start.
- **Keine Cloud-Kosten**: Nur lokale Modelle sind konfiguriert, damit liegen die Kosten strukturell bei 0 €. Das Budget (Tag/Monat 0 €) setzt der Hub selbst durch.

## 2026-10-12 – Brain
- **Dubletten über den Inhalt** erkennen, sonst wurde derselbe Text mit anderem Dateinamen doppelt importiert.
- **Filter für persönliche Daten** bei jedem Export, bei sensiblen Läufen und beim Speichern sensibler Antworten.
- **Token-Budget** beim Kontextexport, ähnliche Textstücke werden übersprungen.
- **MCP-Server ohne Fremdpakete**; Schreiben nur mit Bestätigung.

## 2026-10-12 – Routing
- **Perplexity ohne API** (Handoff-Karte): Ein API-Zugang bräuchte Schlüssel und Abrechnungsprofil und ist im Abo nicht kostenfrei verlässlich.
- **Claude über das Abo** (`claude -p`, nur Text, keine Werkzeuge): etwa 2 k statt 235 k Kontext-Token je Aufruf.
- **Sensibles bleibt lokal**, nie extern.
- **Kurswechsel 23:30**: „Lokal zuerst“ wurde verworfen, **Perplexity zuerst** (Wunsch des Nutzers). Lokal bleibt Pflicht für Sensibles.
- **Lokaler Benchmark abgebrochen**; die Schwellen der Routing-Kategorien bleiben leer.

## 2026-10-16 – Siri, Ideen, Remote-MCP
- **Zweiter Listener** (Port 3031) statt Pfadfilter im Proxy: Der öffentliche Weg erreicht nur Sprache, Ideen und MCP, die Oberfläche bleibt privat.
- **Stack bleibt Python/stdlib**, keine neue Laufzeit.
- **Remote-MCP nur mit vier Ideen-Werkzeugen**; claude.ai-Connectoren können keinen festen Bearer-Header setzen, daher zusätzlich ein geheimer Pfad.
- **Ideen: Postgres führend**, Nextcloud nur als Senke mit stabiler UID (Idempotenz, mehrfaches Senden legt nichts doppelt an).
- **Direkte Ollama-Aufrufe** für Sprache und Ideen: Eine über LiteLLM abgebrochene Erzeugung lief weiter und blockierte das einzige Modell.
- **Fälligkeit per Regel**, nicht per Modell (das kleine Modell rechnete „bis Freitag“ falsch).
- **Dateiname `docs/mcp-remote.md`** statt `docs/mcp.md`, weil `mcp.md` auf macOS/SMB mit `MCP.md` kollidiert.
