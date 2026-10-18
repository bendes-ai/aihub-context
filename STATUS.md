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
7. **Aktualisierung des Hub-Wissens**: Der Hub läuft erst nach einem Neubau des Images mit dem neuen Zuordnungscode (siehe unten).

## Beobachtung vom 17.10.
Öffnet der Hub Perplexity über die Handoff-Karte, ist der Nutzer dort **nicht angemeldet**. Die Ursache wurde nicht geprüft. Folge: Pro-Funktionen, Verlauf und Connectoren stehen nicht zur Verfügung; Perplexity kennt nur die übergebene Frage. Abhilfe in diesem Repo: `CONTEXT.md` einfügen oder verlinken.

## Änderung vom 18.10.2026
- **Sicherheitsregel für sensible Fragen**: Eine Frage mit Kinder-, Gesundheits- oder Adressbezug wird nie automatisch in einem nicht sensiblen Projekt abgelegt. Gewinnt ein sensibles Projekt, bleibt es dabei, sonst bleibt die Frage im Eingang. Auslöser: Eine Frage zu Lern-Apps für ein Kind landete im nicht sensiblen Projekt „Lernhelden“. Extern gesendet wurde sie nicht, denn das Routing erkennt Kinderbezug getrennt davon.
- Tests: Router 19/19, Zuordnung 17/17, Messsatz 36/36. Kontrollsatz 15/18 bei 0 falsch zugeordneten (vorher 1 von 16). Die Zahlen sind nicht direkt vergleichbar, weil der Kontrollsatz um zwei Fälle erweitert wurde und die Stichwörter „Tochter“ und „Sohn“ ergänzt wurden; für diese Begriffe ist der Kontrollsatz nicht mehr unabhängig.
- Der laufende Hub-Container nutzt den neuen Code erst nach dem Neubau des Images.
