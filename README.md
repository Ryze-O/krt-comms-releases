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

Dazu, falls du sie brauchst: das Stream-Deck-Plugin
(`de.kartell.krt-comms.streamDeckPlugin`) samt Icon-Pack
(`krt-comms-icons.streamDeckIconPack`), die Touch-Portal-Datei
(`krt-comms-touchportal.tpp`) und das Mining-Zusatzprogramm
(`krt_mining_sidecar_win.zip` für Windows: entpacken, `mining_sidecar.exe`
starten, Anleitung liegt bei; `krt_mining_sidecar.zip` als Python-Skript für
Linux).

---

## Installation — Windows (für Dummys)

1. **TeamSpeak ganz beenden** — nicht nur das Fenster schließen. TS läuft sonst
   unten rechts im Infobereich (neben der Uhr) weiter: Rechtsklick auf das
   TS-Symbol → **Beenden**.
2. Datei `krt_comms_rebuild_<version>_x64.ts3_plugin` herunterladen
3. **Doppelklick** auf die Datei
4. TS3 fragt „Wollen Sie das Plugin installieren?" → **Ja**
5. TeamSpeak starten. Im TS3-Menü: **Extras → Optionen → Erweiterungen → Plugins** —
   bei **KRT Comms Rebuild** und **KRT Comms Original-Adapter** muss „Aktiviert“
   stehen (der Adapter verbindet dich mit Nutzern des alten KRT Comms)
6. Im TS3-Menü: **Plugins → KRT Comms Rebuild → Funkverwaltung**

Fertig. Die vollständige Anleitung steckt im Plugin selbst: Funkverwaltung →
Seite **Hilfe**.

### „Failed to install Add-On"?

TS3 bietet dann an, es als Administrator zu versuchen — das scheitert meist
auch. Ursache ist fast nie das Paket:

1. **TeamSpeak läuft noch im Infobereich** und hält die Plugin-Datei fest. Das
   ist mit Abstand der häufigste Fall. Dass es auch als Administrator
   scheitert, spricht genau dafür: Ein Rechteproblem wäre damit weg, eine
   gesperrte Datei nicht. TeamSpeak wie in Schritt 1 beenden, nochmal.
2. **Datei entsperren.** Was aus Browser oder Discord kommt, markiert Windows
   als „aus dem Internet". Rechtsklick auf die `.ts3_plugin`-Datei →
   *Eigenschaften* → unten **Zulassen** anhaken → OK.
3. **Download unvollständig?** Dateigröße mit der Angabe auf der
   Release-Seite vergleichen.
4. **Von Hand installieren** (geht immer, auch ohne Adminrechte):
   `.ts3_plugin` in `.zip` umbenennen und entpacken, dann `Windows-Taste + R`
   → `%APPDATA%\TS3Client\plugins` → Enter, und den **Inhalt** des entpackten
   Ordners `plugins` dort hineinkopieren (vorhandenes überschreiben).

### Umstieg vom alten KRT Comms

Das Paket enthält einen **Original-Adapter**, der denselben Dateinamen trägt
wie das alte KRT Comms (`krt_comms_win64.dll`). Die Installation ersetzt das
alte Plugin also — über den Adapter hörst und erreichst du Leute, die noch das
alte KRT Comms benutzen, trotzdem weiter. Deine alten Einstellungen werden
nicht übernommen; Frequenzen trägst du einmal neu ein. Verschlüsselte
Frequenzen funktionieren nur zwischen Nutzern des Rebuilds.

---

## Installation — Linux (für Dummys)

TS3-Linux hat keinen eingebauten Plugin-Installer-Dialog (anders als Windows).
Zwei Schritte: **(1) Abhängigkeiten installieren**, **(2) Plugin-Dateien in den
Plugin-Ordner kopieren.**

### 1. Abhängigkeiten

Das Plugin braucht **Qt5** (Core, GUI, Widgets, Network, WebSockets, SerialPort)
und **libevdev**. Anders als auf Windows bringt der TeamSpeak-Linux-Client diese
Libs **nicht** mit — sie müssen vom System kommen. Such dir deine Distribution:

**Debian / Ubuntu / Mint / Pop!_OS:**
```bash
sudo apt install libqt5core5a libqt5gui5 libqt5widgets5 libqt5network5 libqt5websockets5 libqt5serialport5 libevdev2
```

**Fedora / RHEL / Nobara:**
```bash
sudo dnf install qt5-qtbase qt5-qtbase-gui qt5-qtwebsockets qt5-qtserialport libevdev
```

**Arch / Manjaro / CachyOS / EndeavourOS:**
```bash
sudo pacman -S qt5-base qt5-websockets qt5-serialport libevdev
```
(Arch: `qt5-base` enthält Core/GUI/Widgets/Network in einem Paket.)

**Bazzite / Fedora Silverblue / Kinoite (rpm-ostree-basiert):**
```bash
sudo rpm-ostree install qt5-qtbase qt5-qtbase-gui qt5-qtwebsockets qt5-qtserialport libevdev
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
   Archiv-Programm öffnen und den **gesamten Inhalt** des Ordners `plugins`
   (beide `.so`-Dateien **und** den Ordner `krt_comms_rebuild`) in folgenden
   Ordner schieben:
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
4. TeamSpeak 3 starten → **Extras → Optionen → Erweiterungen → Plugins** — KRT Comms
   Rebuild und KRT Comms Original-Adapter aktivieren → **Plugins → KRT Comms
   Rebuild → Funkverwaltung**.

### Hotkeys unter Linux (wichtig!)

Die TS3-eigenen Hotkeys (**Optionen → Hotkeys**) feuern auf Linux **nur**, wenn
das TS3-Fenster den Fokus hat — fürs Spielen unbrauchbar.

Lösung: Das Plugin hat eigene **Linux-Hotkeys** auf der Seite **Hotkeys** der
Funkverwaltung (nur unter Linux sichtbar). Dort bindest du Tasten/Maustasten
direkt über `evdev` — die feuern global, egal welches Fenster fokussiert ist.

1. Funkverwaltung öffnen → Seite **Hotkeys** → Bereich **Linux-Hotkeys**
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

Die TS3-Hotkey-Einstellung kannst du leer lassen — die Linux-Hotkeys ersetzen sie.

### Overlays hinter Vollbild-Spielen (KDE, GNOME)

Ab v2.8.2 bleiben HUD und Overlays auch über Star Citizen im Vollbild sichtbar.
Macht das Probleme, lässt es sich abschalten: **HUD Overlay → Interaktion →
Über Vollbild-Spielen anzeigen**, danach TS3 neu starten.

### Deinstallation (Linux)

```bash
rm -f ~/.ts3client/plugins/krt_comms_rebuild_linux_amd64.so
rm -f ~/.ts3client/plugins/krt_comms_linux_amd64.so
rm -rf ~/.ts3client/plugins/krt_comms_rebuild
```

---

## Berechtigung (wichtig)

Das Plugin sendet nur, wenn dein TS3-Account auf **das-kartell.org** Mitglied
einer Server-Gruppe mit `(KRT)` im Namen ist (z.B. „Lieutenant Commander
(KRT) 8"). Ohne diese Gruppe siehst du das HUD und hörst Funk, kannst aber
nichts senden.

In welchen Gruppen du bist, zeigt TeamSpeak selbst: dich im Channel-Baum
anklicken, rechts stehen deine Server-Gruppen.

Freischaltung läuft über den TS3-Server-Admin.

---

## Updates

Das Plugin prüft selbst auf Updates. Gibt es eine neuere Version, erscheint
beim TS3-Start ein Hinweis mit Download-Knopf. Datei herunterladen, **TeamSpeak
ganz beenden (auch im Infobereich)**, Doppelklick, fertig.
Unter Linux: neue Dateien wie oben beschrieben über die alten kopieren.

---

## Hilfe & Bug-Reports

- **Anleitung:** im Plugin, Funkverwaltung → Seite **Hilfe**.
- **Fehler melden:** Im TS3-Menü **Plugins → KRT Comms Rebuild → Log für ryze
  exportieren** legt ein Paket auf deinen Desktop. Das mit einer kurzen
  Beschreibung an ryze schicken. (Derselbe Knopf steht auch unter Hilfe →
  Über KRT Comms Rebuild, daneben „Bug melden" für einen vorausgefüllten
  Bericht.)
- **Issues:** [github.com/Ryze-O/krt-comms-releases/issues](https://github.com/Ryze-O/krt-comms-releases/issues)
