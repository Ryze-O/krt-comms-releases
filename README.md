# KRT Comms Rebuild — Releases

Public-Releases des Funk-Plugins für TeamSpeak 3.

**Quellcode ist privat** — dieses Repo enthält ausschließlich die fertigen
`.ts3_plugin`-Builds zum Download. Wer mitentwickeln will, fragt im Discord.

---

## Aktuellste Version herunterladen

Oben rechts auf **Releases** klicken (oder direkt:
[Latest Release](https://github.com/Ryze-O/krt-comms-releases/releases/latest)).

Dort findest du je Plattform eine Datei:

| Plattform | Datei                                            |
|-----------|--------------------------------------------------|
| Windows   | `krt_comms_rebuild_<version>_x64.ts3_plugin`      |
| Linux     | `krt_comms_rebuild_<version>_linux_amd64.ts3_plugin` |

---

## Installation — Windows (für Dummys)

1. Datei `krt_comms_rebuild_<version>_x64.ts3_plugin` herunterladen
2. **Doppelklick** auf die Datei
3. TS3 fragt „Wollen Sie das Plugin installieren?" → **Ja**
4. TS3 komplett schließen und neu starten
5. Im TS3-Menü: **Extras → Optionen → Addons → Plugins** — KRT Comms Rebuild
   muss aktiviert sein (Häkchen)
6. Im TS3-Menü: **Plugins → KRT Comms Rebuild → Funkverwaltung öffnen**

Fertig.

## Installation — Linux (für Dummys)

**Voraussetzung:** `libqt5serialport5` muss installiert sein. Falls nicht:

```bash
sudo apt install libqt5serialport5
```

Dann:

1. Datei `krt_comms_rebuild_<version>_linux_amd64.ts3_plugin` herunterladen
2. Im Dateimanager **Doppelklick** auf die Datei. Wenn TS3 dadurch nicht
   automatisch aufgeht, geht's auch manuell:

```bash
# .ts3_plugin ist eine ZIP. Interne Struktur ist verschachtelt — wir
# entpacken in einen Temp-Ordner und kopieren dann den korrekten Inhalt
# nach ~/.ts3client/plugins:
mkdir -p ~/.ts3client/plugins
TMP=$(mktemp -d)
unzip krt_comms_rebuild_*_linux_amd64.ts3_plugin -d "$TMP"
cp -r "$TMP"/plugins/* ~/.ts3client/plugins/
rm -rf "$TMP"
```

Anschließend sollten in `~/.ts3client/plugins/` direkt liegen:

```
krt_comms_rebuild_linux_amd64.so
krt_comms_rebuild/        (Ordner mit .wav, .png)
```

3. TS3 komplett schließen und neu starten
4. **Extras → Optionen → Addons → Plugins** — KRT Comms Rebuild aktivieren
5. **Plugins → KRT Comms Rebuild → Funkverwaltung öffnen**

---

## Berechtigung (wichtig)

Das Plugin sendet nur, wenn dein TS3-Account auf **das-kartell.org** Mitglied
einer Server-Gruppe mit `(KRT)` im Namen ist (z.B. „Lieutenant Commander
(KRT) 8"). Ohne diese Gruppe siehst du das HUD und hörst Funk, kannst aber
nichts senden.

Wenn du sehen willst, in welchen Gruppen du bist:
**Funkverwaltung → Info-Tab → „Server-Gruppen anzeigen…"** schreibt deine
Mitgliedschaft + alle Server-Gruppen ins TS3-ClientLog (Extras → ClientLog,
Channel-Filter `krt_comms`).

Freischaltung läuft über den TS3-Server-Admin.

---

## Updates

Ab v1.8.8 prüft das Plugin selbst auf Updates. Wenn eine neuere Version
verfügbar ist, erscheint beim TS3-Start ein Popup mit Download-Button —
SaltyChat-Style. Einfach klicken, Datei runterladen, Doppelklick, fertig.

---

## Hilfe & Bug-Reports

- **TS3-ClientLog** (Extras → ClientLog, Filter `krt_comms`) enthält die
  wichtigsten Diagnose-Meldungen.
- **Bug melden:** In der Funkverwaltung → Info-Tab gibt's einen Button
  „Bug melden", der einen vorausgefüllten Report mit Versions-/OS-Infos
  in die Zwischenablage kopiert. Den dann im Discord/Forum posten.
- **Issues:** [github.com/Ryze-O/krt-comms-releases/issues](https://github.com/Ryze-O/krt-comms-releases/issues)
