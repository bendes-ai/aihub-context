# AIHub – Status

Stand: 2026-10-17. Die Angaben stammen aus der Hub-Doku (Statusstand 13.10., Ergänzungen 16./17.10.) und sind **nicht live geprüft**.

## Lief zuletzt
- Fünf Container healthy; Router-Tests 19/19; Gehirn: 44 Dokumente, 405 Textstücke, keine offenen Prüfungen (Stand 13.10.).
- Siri-Kurzbefehle, Ideen mit Erinnerungen und Remote-MCP für Ideen sind seit 16.10. umgesetzt (zuletzt geändert 17.10.).

## Offen
1. **Cron für nächtliche Backups ist nicht gesetzt**: Die Änderung an der Crontab wurde von der Berechtigungsprüfung blockiert und muss manuell erfolgen.
2. ~~Claude-Worker nur als Hintergrundprozess~~ erledigt: Der Worker läuft und ist per systemd aktiviert (geprüft am 18.10.).
3. **Lokaler Benchmark** abgebrochen, „Lokal-Check“ und Schwellen sind leer.
4. ~~Festes UI-Token~~ erledigt: Der LAN-Hub verlangt den echten Token (das feste Wort ist abgeschaltet). Ausgewählte Geräte sind per Adresse davon befreit (siehe unten).
5. **Mobiltest** der Autopilot-Seite fehlt (nur Syntaxprüfung und API getestet).
6. **Perplexity-Handoff** braucht manuelles Einfügen der Antwort.
7. ~~Neuer Zuordnungscode noch nicht im laufenden Hub~~ erledigt am 18.10. (siehe unten).

## Beobachtung vom 17.10.
Öffnet der Hub Perplexity über die Handoff-Karte, ist der Nutzer dort **nicht angemeldet**. Die Ursache wurde nicht geprüft. Folge: Pro-Funktionen, Verlauf und Connectoren stehen nicht zur Verfügung; Perplexity kennt nur die übergebene Frage. Abhilfe in diesem Repo: `CONTEXT.md` einfügen oder verlinken.

## Änderung vom 18.10.2026
- **Sicherheitsregel für sensible Fragen**: Eine Frage mit Kinder-, Gesundheits- oder Adressbezug wird nie automatisch in einem nicht sensiblen Projekt abgelegt. Gewinnt ein sensibles Projekt, bleibt es dabei, sonst bleibt die Frage im Eingang. Auslöser: Eine Frage zu Lern-Apps für ein Kind landete im nicht sensiblen Projekt „Lernhelden“. Extern gesendet wurde sie nicht, denn das Routing erkennt Kinderbezug getrennt davon.
- Tests: Router 19/19, Zuordnung 17/17, Messsatz 36/36. Kontrollsatz 15/18 bei 0 falsch zugeordneten (vorher 1 von 16). Die Zahlen sind nicht direkt vergleichbar, weil der Kontrollsatz um zwei Fälle erweitert wurde und die Stichwörter „Tochter“ und „Sohn“ ergänzt wurden; für diese Begriffe ist der Kontrollsatz nicht mehr unabhängig.
- Deployt am 18.10.: Image neu gebaut, nur der Hub-Container neu gestartet (healthy), Messung im laufenden Hub bestätigt die Werte. Rollback-Image: `aihub/brain:pre-sicherheitsregel-20261018`.

## Änderung vom 18.10.2026 (später): vertraute Geräte
- Einzelne Geräte-Adressen aus einer Liste (`HUB_TRUSTED_IPS`) nutzen den LAN-Hub ohne Token-Abfrage, alle anderen brauchen weiter den echten Token. Es zählt nur die TCP-Gegenstelle; gefälschte Weiterleitungs-Header bringen nichts (getestet). Das Schreiben ins Gehirn (`/api/ingest`) und der öffentliche Listener sind nicht betroffen.
- Rest-Risiko: Adressen sind per DHCP nicht fest und im Heimnetz fälschbar. Rückfall bei geänderter Adresse ist die Token-Abfrage.
