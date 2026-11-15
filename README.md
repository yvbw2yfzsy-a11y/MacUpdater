# macupdate

Automatische nächtliche Updates für deinen Mac – in fester Reihenfolge:
Craft-Apps (GitHub) → Homebrew → App Store.

macOS-Updates erledigt macOS selbst (Systemeinstellungen › Allgemein › Softwareupdate ›
Automatische Updates). Ist macOS gerade mit einem Update beschäftigt – laden, vorbereiten,
installieren –, wartet macupdate bis zu 45 Minuten und lässt den Lauf sonst aus.

## Einrichten (im Terminal)

1. Die Datei `macupdate` herunterladen. Sie landet normalerweise in „Downloads“.
2. `bash ~/Downloads/macupdate --install` ausführen. Es fragt nach Uhrzeit, App-Store-Updates, fehlenden Apps und Aufwecken. Admin-Rechte (Passwort oder Touch ID) braucht nur das Einrichten.
3. Zum Testen `macupdate -c` ausführen. Das zeigt nur, was es Neues gibt, und installiert nichts.
4. Danach einmal `macupdate -u` starten, während du am Mac sitzt.

## Befehle

| Option | Wirkung |
|---|---|
| `-u`, `--update` | alles jetzt aktualisieren |
| `-c`, `--check` | nur prüfen, nichts installieren |
| `-s`, `--skip` | den nächsten nächtlichen Lauf überspringen |
| `--unskip` | Überspringen wieder aufheben |
| `-e`, `--edit` | Einstellungsdatei bearbeiten. Eine geänderte Uhrzeit gilt sofort nach dem Schließen. |
| `--status` | Zeitplan, nächster und letzter Lauf, App-Store-Helper, laufendes macOS-Update |
| `-l`, `--log` | Protokoll des letzten Laufs |
| `-v`, `--version` | Versionsnummer |
| `-h`, `--help` | Hilfe |
| `--install` | einrichten oder aktualisieren |
| `--uninstall` | alles wieder entfernen |

## Craft-Apps

macupdate installiert und prüft sie wie der offizielle ArtCraft Launcher:

- Download nur aus den GitHub-Releases des Besitzers (`storytold/…`), geprüft gegen `SHA256SUMS.txt`.
- Die App wird ohne Finder-Infos und erweiterte Attribute aus dem DMG kopiert – sonst lehnt `codesign --strict` sie als „detritus“ ab.
- Vor dem Austausch: Signatur intakt, Developer-ID-Signatur des erwarteten Teams mit der erwarteten Bundle-ID, notarisiert, von Gatekeeper akzeptiert, und die App meldet die Version des Releases.
- Ohne GitHub-API (Limit erreicht) kommt der Dateiname aus `SHA256SUMS.txt`, nicht aus einem geratenen Schema.
- Die vorige Version bleibt als Backup in `~/Library/Application Support/macupdate/backup`.

## App Store und Sicherheit

`mas update` braucht root. `--install` richtet dafür einen kleinen Helper ein
(`/Library/PrivilegedHelperTools/local.macupdate.appstore`), den macupdate per sudo ohne Passwort
starten darf. Die sudo-Regel erlaubt nur diesen Helper und nur ohne Argumente; er startet nur
eine root-eigene Kopie von `mas` mit `update`. Ein Passwort wird nirgends gespeichert – auch
verschlüsselt nicht: Der Schlüssel müsste für den nächtlichen Lauf greifbar sein, und ein
Passwort öffnet den ganzen Mac, diese Regel nur App-Store-Updates.

Kopiert wird das eigentliche mas-Programm – bei Homebrew ab mas 7 `libexec/bin/mas`, nicht der
Wrapper unter `bin/mas`. Ändert sich mas später (z. B. durch `brew upgrade`), läuft nachts weiter
die bisherige Kopie. `--install` zeigt dann Pfad, Version und SHA-256 der neuen Version und
übernimmt sie nur nach Bestätigung: Der Homebrew-Ordner gehört deinem Benutzer, jedes Programm,
das als du läuft, könnte mas dort austauschen. Nach `--install`, `-e` und `--uninstall` gibt
macupdate die Admin-Rechte sofort wieder ab (`sudo -k`).

Umstieg von macupdate 1.x: `--install` entfernt den alten Root-Helper, dessen sudo-Regel und ein
dort gespeichertes Passwort.
