# AIHub – Status

Stand: 2026-10-17. Die Angaben stammen aus der Hub-Doku (Statusstand 13.10., Ergänzungen 16./17.10.) und sind **nicht live geprüft**.

## Lief zuletzt
- Fünf Container healthy; Router-Tests 19/19; Gehirn: 44 Dokumente, 405 Textstücke, keine offenen Prüfungen (Stand 13.10.).
- Siri-Kurzbefehle, Ideen mit Erinnerungen und Remote-MCP für Ideen sind seit 16.10. umgesetzt (zuletzt geändert 17.10.).

## Offen
1. **Cron für nächtliche Backups ist nicht gesetzt**: Die Änderung an der Crontab wurde von der Berechtigungsprüfung blockiert und muss manuell erfolgen.
2. **Claude-Worker** läuft nur als Hintergrundprozess, die systemd-Unit liegt bereit, ist aber nicht aktiviert.
3. **Lokaler Benchmark** abgebrochen, „Lokal-Check“ und Schwellen sind leer.
4. **Festes UI-Token** im LAN-Hub ist schwach (nur LAN/VPN).
5. **Mobiltest** der Autopilot-Seite fehlt (nur Syntaxprüfung und API getestet).
6. **Perplexity-Handoff** braucht manuelles Einfügen der Antwort.
7. **Dokumentation**: `AGENTS.md` im Hub-Ordner beschreibt noch das alte `lokaleki`-Setup.

## Beobachtung vom 17.10.
Öffnet der Hub Perplexity über die Handoff-Karte, ist der Nutzer dort **nicht angemeldet**. Die Ursache wurde nicht geprüft. Folge: Pro-Funktionen, Verlauf und Connectoren stehen nicht zur Verfügung; Perplexity kennt nur die übergebene Frage. Abhilfe in diesem Repo: `CONTEXT.md` einfügen oder verlinken.
