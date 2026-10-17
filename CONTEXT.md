# AIHub – Kontext für KI-Assistenten ohne Gedächtnis

Stand: 2026-10-17. Quelle: Projektdoku des Hubs bis 16./17.10.2026, nicht live geprüft.
Gedacht zum Einfügen in eine Unterhaltung (z. B. Perplexity ohne Anmeldung) oder zum Verlinken.

## Was der AIHub ist
Ein persönlicher KI-Hub für eine Einzelperson im Heimnetz. Er läuft auf einem UGREEN-NAS (DXP4800, 8 Kerne, 30 GB RAM, keine für große Modelle nutzbare Grafikeinheit). Ziele: laufende Kosten 0 €, Privatsphäre, ein einziger Einstieg („Autopilot“) statt vieler Werkzeuge.

## Bestandteile (Docker Compose, Projekt `aihub`)
- **Hub**: Python (nur Standardbibliothek), Weboberfläche mit Autopilot-Seite `/auto`
- **Ollama**: lokale Modelle. Standard `qwen3.5:4b`, Fallback `gemma4:e4b`, Embeddings `nomic-embed-text`
- **LiteLLM**: einheitlicher Modell-Endpunkt, nur lokale Modelle
- **Postgres + pgvector**: das „Gehirn“ (Dokumente, Textstücke, Ideen)
- **SearXNG**: Websuche, nur intern erreichbar

## Wie eine Frage geroutet wird
- **Recherche / aktuelle Fragen**: zuerst Perplexity Pro. Ohne API: Der Hub zeigt eine **Handoff-Karte** (Frage kopieren, Perplexity öffnen, Antwort einfügen). Alternative: „Ohne Perplexity weiter“ (Websuche).
- **Code, Umsetzung, Planung**: Gehirn, dann Claude Pro über einen Host-Worker (`claude -p`, nur Text). Fallback: lokales Code-Modell, Websuche.
- **Sensibles** (Kinder, Gesundheit, Adresse, …): nur Gehirn und lokale Modelle, nie extern. Der Hub ersetzt dabei persönliche Angaben vor jedem Export.
- Kosten-Budget Tag/Monat = 0 €. Der Hub fragt bei Kosten: genehmigen, kostenlose Alternative oder abbrechen.

## Wissen, das ein externer Assistent haben sollte
- Der Nutzer arbeitet auf Deutsch und will kurze, konkrete Antworten mit Schritten und Befehlen.
- Bevorzugt: Docker Compose, Reverse Proxy, selbst gehostete Lösungen.
- Vor Änderungen an produktiven Diensten: kurz das Risiko nennen.
- Projekte des Nutzers (Schlüssel im Hub): `ki-hub`, `smart-home`, `nas-infrastruktur`, `home-cinema`, `lernhelden`, `richbitch`, `fitness`, sonst `inbox`.
- Ein Assistent ohne Zugriff auf das Gehirn kennt keine früheren Gespräche. Er soll nachfragen, statt Annahmen über den Nutzer zu treffen.

## Grenzen und bekannte Schwächen
- Die lokalen Modelle sind klein und auf dem NAS langsam (ca. 5–12 Token/s); komplexe Fragen gehören an Perplexity oder Claude.
- Die Perplexity-Anbindung ist ein manueller Handoff, kein automatischer Aufruf. Perplexity sieht nur die eingefügte Frage, kein Gedächtnis und keine Hub-Daten.
- Der LAN-Hub nutzt ein schwaches festes Token und ist nur im Heimnetz oder per VPN gedacht.

## Was bewusst nicht in diesem Repo steht
Zugangsdaten, Token, interne Adressen, Namen von Personen, Inhalte aus dem Gehirn, alles zu Kindern oder Gesundheit.
