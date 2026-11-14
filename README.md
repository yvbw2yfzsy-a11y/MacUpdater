Einrichten (im Terminal):

Die Datei macupdate herunterladen. Sie landet normalerweise in „Downloads“.
bash ~/Downloads/macupdate --install ausführen. Es fragt einmal nach deinem Admin-Passwort und ein paar Einstellungen (Uhrzeit, Aufwecken, fehlende Apps, macOS-Passwort).
Zum Testen macupdate -c ausführen. Das zeigt nur, was es Neues gibt, und installiert nichts.
Danach einmal macupdate -u starten, während du am Mac sitzt.

Befehle

-u / --update: alles jetzt aktualisieren, in deiner Reihenfolge (Craft-Apps → Homebrew → App Store → macOS)
-s / --skip: den nächsten Nachtlauf auslassen; --unskip hebt das wieder auf
-e / --settings: Einstellungsdatei öffnen. Eine geänderte Uhrzeit gilt sofort nach dem Schließen.
-h / --help: Hilfe
Zusätzlich: -c (nur prüfen), --status, -l (Protokoll), --uninstall