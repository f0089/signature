# Signature

Erzeugt realistische Kugelschreiber-Unterschriften aus einem Namen. Die Handbewegung wird simuliert, die Tinte mit Druck, Tempo, Riefen und Klecksen gerendert. Es gibt keine KI und keine Schreibschrift-Fonts.

**Web-Adresse:** https://f0089.github.io/signature/

## Zugang

Beim Öffnen wird ein 4-stelliger Zugangscode abgefragt. Der Code wird auf dem Server (Supabase) geprüft. Nur bei einem gültigen Code liefert der Server den Schlüssel, mit dem der Browser das verschlüsselte Programm (`app.bin`) entschlüsselt. Ohne gültigen Code ist das Programm nicht lesbar, auch nicht für jemanden, der dieses Repository durchsucht.

- Nach 8 Fehlversuchen innerhalb von 15 Minuten wird das Gerät für 15 Minuten gesperrt.
- Mit „Auf diesem Gerät merken“ bleibt man angemeldet. Das Schloss-Symbol oben rechts im Programm meldet wieder ab.

### Codes verwalten

Die Codes stehen im Supabase-Projekt **signature** in der Tabelle `access_codes`:

1. supabase.com/dashboard öffnen, Projekt „signature“ wählen
2. Links **Table Editor** → Tabelle `access_codes`
3. **Neuer Code:** „Insert row“ und bei `code` vier Ziffern eintragen. `label` ist eine frei wählbare Notiz, zum Beispiel für wen der Code ist.
4. **Code sperren:** bei `active` auf `false` stellen. Geräte, die sich mit diesem Code gemerkt hatten, werden beim nächsten Öffnen abgemeldet.
5. Die Spalten `uses` und `last_used_at` zeigen, wie oft und wann ein Code zuletzt benutzt wurde.

Die Tabelle `app_secret` bitte nicht ändern, sonst lässt sich das Programm nicht mehr entschlüsseln.

## Aufbau

| Datei | Zweck |
| --- | --- |
| `index.html` | Startseite mit der Code-Abfrage |
| `app.bin` | Das Programm, verschlüsselt (AES-256-GCM) |
| Supabase-Funktion `signature-unlock` | prüft den Code und gibt bei Erfolg den Schlüssel heraus |

Kostenlose Supabase-Projekte werden nach längerer Zeit ohne Nutzung pausiert. Falls die Seite meldet, dass der Server nicht antwortet, das Projekt im Supabase-Dashboard wieder starten.
