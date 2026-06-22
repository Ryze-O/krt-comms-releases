<!--
  Kanonische Quelle des README für das Public-Release-Repo
  (Ryze-O/krt-comms-releases). NICHT direkt im Public-Repo editieren —
  Änderungen hier vornehmen; die Release-CI (.github/workflows/release.yml)
  spiegelt diese Datei bei jedem Release als README.md ins Public-Repo.
-->
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

---

## Installation — Linux (für Dummys)

TS3-Linux hat keinen eingebauten Plugin-Installer-Dialog (anders als Windows).
Zwei Schritte: **(1) Abhängigkeiten installieren**, **(2) Plugin-Dateien in den
Plugin-Ordner kopieren.**

### 1. Abhängigkeiten

Das Plugin braucht **Qt5 SerialPort**, **Qt5 WebSockets** und **libevdev**.
Qt5-Core/GUI/Network bringt der TeamSpeak-Client selbst mit; `qtbase` ziehen die
SerialPort-Pakete automatisch mit. Such dir deine Distribution:

**Debian / Ubuntu / Mint / Pop!_OS:**
```bash
sudo apt install libqt5serialport5 libqt5websockets5 libevdev2
```

**Fedora / RHEL / Nobara:**
```bash
sudo dnf install qt5-qtserialport qt5-qtwebsockets libevdev
```

**Arch / Manjaro / CachyOS / EndeavourOS:**
```bash
sudo pacman -S qt5-serialport qt5-websockets libevdev
```

**Bazzite / Fedora Silverblue / Kinoite (rpm-ostree-basiert):**
```bash
sudo rpm-ostree install qt5-qtserialport qt5-qtwebsockets libevdev
sudo systemctl reboot
```
Der **Reboot ist Pflicht** — `rpm-ostree` aktiviert die Libs erst beim nächsten
Start.

> **Bazzite & Co. — wichtig:** TS3 darf **nicht** die Flatpak-Version sein
> (`flatpak list | grep -i teamspeak` muss leer bleiben). Die Flatpak-Sandbox
> sieht weder `~/.ts3client/plugins/` noch die per rpm-ostree installierten
> Libs. Falls schon installiert: `flatpak uninstall com.teamspeak.TeamSpeak3`,
> dann TS3 nativ als `.run` von https://teamspeak.com/de/downloads/ einrichten.

### 2. Plugin installieren (distributionsunabhängig)

1. TeamSpeak 3 schließen.
2. Die heruntergeladene `.ts3_plugin` ist ein **ZIP-Archiv**. Mit einem
   Archiv-Programm öffnen und die Datei `krt_comms_rebuild_linux_amd64.so`
   **sowie** den Ordner `krt_comms_rebuild` in folgenden Ordner schieben:
   ```
   ~/.ts3client/plugins/
   ```
   Lieber Terminal? Eine Zeile tut's auch:
   ```bash
   mkdir -p ~/.ts3client/plugins && \
   unzip -o krt_comms_rebuild_*_linux_amd64.ts3_plugin -d /tmp/krtc && \
   cp -r /tmp/krtc/plugins/* ~/.ts3client/plugins/ && rm -rf /tmp/krtc
   ```
3. **Updates** laufen genauso — einfach die Dateien überschreiben. Vorher nichts
   löschen.
4. TeamSpeak 3 starten → **Extras → Optionen → Addons → Plugins** — KRT Comms
   Rebuild aktivieren → **Plugins → KRT Comms Rebuild → Funkverwaltung öffnen**.

### Hotkeys unter Linux (wichtig!)

Die TS3-eigenen Hotkeys (**Optionen → Hotkeys**) feuern auf Linux **nur**, wenn
das TS3-Fenster den Fokus hat — fürs Spielen unbrauchbar.

Lösung: Das Plugin hat einen eigenen Tab **„Linux-Hotkeys"** in der
Funkverwaltung (nur unter Linux sichtbar). Dort bindest du Tasten/Maustasten
direkt über `evdev` — die feuern global, egal welches Fenster fokussiert ist.

1. Funkverwaltung öffnen → Tab **„Linux-Hotkeys"**
2. Pro Aktion (PTT pro Funkgerät, Anklopfen, Broadcast-Gruppen, …) auf
   „Aufnehmen" klicken und die gewünschte Taste/Maustaste drücken
3. Modifier (Strg/Alt/Shift) werden automatisch mit erfasst

> **Falls die Linux-Hotkeys nicht feuern:** Auf manchen Distributionen (oft
> Ubuntu/Debian/GNOME) muss dein Benutzer in der `input`-Gruppe sein, um
> `/dev/input` lesen zu dürfen:
> ```bash
> sudo usermod -aG input $USER
> ```
> Danach **einmal aus- und wieder einloggen**. Viele KDE-/Gaming-Distros
> (CachyOS, Bazzite, …) brauchen das nicht. **Sicherheitshinweis:** Wer in der
> `input`-Gruppe ist, kann systemweit alle Tastatureingaben mitlesen — auf
> einem Single-User-PC unbedenklich, auf geteilten Rechnern bedenken.

Die TS3-Hotkey-Einstellung kannst du leer lassen — der Linux-Tab ersetzt sie.

### Deinstallation (Linux)

```bash
rm -f ~/.ts3client/plugins/krt_comms_rebuild_linux_amd64.so
rm -rf ~/.ts3client/plugins/krt_comms_rebuild
```

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
Unter Linux: neue Dateien wie oben beschrieben über die alten kopieren.

---

## Hilfe & Bug-Reports

- **TS3-ClientLog** (Extras → ClientLog, Filter `krt_comms`) enthält die
  wichtigsten Diagnose-Meldungen.
- **Bug melden:** In der Funkverwaltung → Info-Tab gibt's einen Button
  „Bug melden", der einen vorausgefüllten Report mit Versions-/OS-Infos
  in die Zwischenablage kopiert. Den dann im Discord/Forum posten.
- **Issues:** [github.com/Ryze-O/krt-comms-releases/issues](https://github.com/Ryze-O/krt-comms-releases/issues)
