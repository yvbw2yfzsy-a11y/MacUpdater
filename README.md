# macupdate

Automatische nächtliche Updates für deinen Mac – in fester Reihenfolge:
Craft-Apps (GitHub) → Homebrew → App Store → macOS.

## Einrichten (im Terminal)

1. Die Datei `macupdate` herunterladen. Sie landet normalerweise in „Downloads“.
2. `bash ~/Downloads/macupdate --install` ausführen. Es fragt einmal nach deinem Admin-Passwort und ein paar Einstellungen (Uhrzeit, macOS-Passwort, fehlende Apps, Aufwecken).
3. Zum Testen `macupdate -c` ausführen. Das zeigt nur, was es Neues gibt, und installiert nichts.
4. Danach einmal `macupdate -u` starten, während du am Mac sitzt.

## Befehle

| Option | Wirkung |
|---|---|
| `-u`, `--update` | alles jetzt aktualisieren |
| `-c`, `--check` | nur prüfen, nichts installieren |
| `-s`, `--skip` | den nächsten Nachtlauf auslassen |
| `--unskip` | Auslassen wieder aufheben |
| `-e`, `--edit` | Einstellungsdatei bearbeiten. Eine geänderte Uhrzeit gilt sofort nach dem Schließen. |
| `--status` | Zeitplan, nächster und letzter Lauf |
| `-l`, `--log` | Protokoll des letzten Laufs |
| `-v`, `--version` | Versionsnummer |
| `-h`, `--help` | Hilfe |
| `--install` | einrichten oder aktualisieren |
| `--uninstall` | alles wieder entfernen |

`--settings` funktioniert weiterhin als alter Name für `--edit`.
