# 🔄 HANDOFF: Samsung TV Setup (CleanFrame + TizenTube)

**Stand:** 20.11.2026  
**GitHub-Fork:** `https://github.com/pmhofer/CleanFrame` (geforkt von `nthnclrk/CleanFrame`, Branch `main`)  
**Ziel-Ordner lokal (Mac / iCloud Drive):** `~/Library/Mobile Documents/com~apple~CloudDocs/CleanFrame-Projekt/CleanFrame`

---

## 1. Projektziel des Users

* **Werbefreier, minimalistischer Home-Screen** für seinen Samsung Tizen Smart TV (Modell & Firmware werden am TV im lokalen WLAN noch ausgelesen).
* **Kein Root, 100 % rückgängig machbar:** Zuerst sichere Installation als normale App (Phase 3), erst danach – nach erfolgreichem Test und expliziter Freigabe – optional prüfen, ob Home-Taste / schnellerer Startup (Phase 4) auf dem Modell sicher möglich ist.
* **Werbefreies YouTube mit rein:** Installation von **[reisxd/TizenTube](https://github.com/reisxd/TizenTube)** (Standalone-App, inkl. AdBlock, SponsorBlock, optional DeArrow / Shorts ausblenden) und **Verknüpfung der YouTube-Kachel im CleanFrame-Dock direkt auf die TizenTube-App-ID**.
* **Samsung TV-Einstellungen (Helligkeit, Bildmodus, Ton):** Bleiben über die normale Fernbedienung jederzeit erreichbar + direkt in CleanFrame unter *Settings → Samsung* (`com.samsung.tv.coba.setting` in `app/device.js` – exakte ID wird am TV geprüft).

---

## 2. Bereits erledigt (gepusht in `pmhofer/CleanFrame`, Branch `main`)

* ✅ Fork `pmhofer/CleanFrame` von `nthnclrk/CleanFrame` angelegt.
* ✅ **24-Stunden-Uhr + deutsches Datum** in `app/main.js` (`clockText()`, `dateText()`, z. B. `20:15` + `Freitag, 20. November`) und `app/style.css` (`.clock-date` mit automatischer Adaptive-Ink-Anpassung für alle Hintergrundbilder sowie Dark/Light-Modus; ohne `inset`/`gap` für Tizen-Kompatibilität).
* ✅ **Standard-Layout:** Dock unten (`photo` + `dock` + `glass`).
* ✅ **13 Wunsch-Apps** als `PRIMARY_APPS` in `app/main.js` eingetragen (`CAP` von 9 auf 14 erhöht):
  * Mit SVG-Logo: **Netflix, YouTube, Prime Video, Apple TV, Disney+**
  * Mit Monogramm (auf dem TV werden echte App-IDs & native Icons ausgelesen): **ARD Mediathek, ZDF Mediathek, RTL+, Joyn, WOW, MagentaTV, DAZN, Twitch**
* ✅ Alle Checks bestanden: `npm run check` (ESLint ES5, `scripts/check.py`, 10/10 `check:device`-Tests) & `npm run check:public`.

---

## 3. Noch offen / Nächste Schritte (Reihenfolge)

### Schritt A: Repo lokal auf dem Mac ins iCloud Drive klonen & Vorschau prüfen
```bash
mkdir -p ~/Library/Mobile\ Documents/com~apple~CloudDocs/CleanFrame-Projekt && cd $_
git clone https://github.com/pmhofer/CleanFrame.git
cd CleanFrame
npm ci
npm run dev:public
```
Browser-Preview: `http://localhost:4795/home/` (Steuerung: Pfeiltasten, Enter, Escape).

### Schritt B: TV-Daten & Developer Mode (im lokalen WLAN)
1. Am Samsung TV ablesen:
   * *Einstellungen → Unterstützung → Über diesen TV* → **Modellnummer & Software-Version** (Mindestanforderung: Tizen 5.0+).
   * *Einstellungen → Allgemein → Netzwerk → Netzwerkstatus* → **TV-IP**.
2. Mac-IP im selben WLAN ablesen: `ipconfig getifaddr en0`
3. Developer Mode am TV aktivieren:
   * *Home → Apps → App-Einstellungen* → Fernbedienung `12345` → **Developer Mode: Ein** → Mac-IP als **Host PC IP** eintragen → TV **10 Sek. komplett stromlos machen** (Stecker ziehen).
4. Optionaler Sofort-Speed-Tipp am TV: *Einstellungen → Allgemein → Smart-Funktionen → Smart Hub automatisch starten* **AUS**.

### Schritt C: CleanFrame signieren & als normale App installieren (Phase 3)
* Offizielle Samsung-Tools installieren (Tizen Studio CLI + TV Extension + Samsung Certificate Extension).
* Per `sdb connect <TV_IP>:26101` verbinden, Tizen-Version, Architektur & alle installierten App-IDs auslesen.
* Eigenes Zertifikat + App-ID in `.local/signing.json` (git-ignored!) anlegen.
* App bauen, signieren, installieren und mit Fernbedienung testen (inkl. echte IDs für ARD/ZDF/RTL+/Joyn/WOW/MagentaTV/DAZN/Twitch & Samsung TV-Settings).

### Schritt D: TizenTube installieren & ins CleanFrame-Dock verknüpfen
* **[reisxd/TizenTube](https://github.com/reisxd/TizenTube)** (Standalone) über **[TizenBrew Device Manager](https://github.com/reisxd/tizenbrew-device-manager)** oder **[Apps2Samsung](https://github.com/Apps2Samsung/Apps2Samsung)** auf dem Samsung TV installieren.
* Die echte TizenTube-App-ID auf dem TV auslesen und in `app/main.js` (und ggf. `app/device.js`) als Ziel der **YouTube-Kachel** im CleanFrame-Dock eintragen, neu bauen & updaten.
* Danach im Developer Mode am TV die Host PC IP auf `127.0.0.1` umstellen, damit TizenTube Werbung direkt ab Frame 1 blockt.

### Schritt E (Optional, nur nach expliziter Freigabe): Phase 4 – Home-Taste & schneller Startup
* Nur wenn Schritt C & D stabil laufen: prüfen, ob das konkrete TV-Modell & die Firmware mit `docs/agent-tv-setup-guide.md` (Stage 7/8) kompatibel sind.
* Pflicht vor jedem Schreibvorgang: vollständiges Backup der Originalwerte in `.local/native-home/original.json` + Rollback-Test.

---

## 4. Copy-Paste-Prompt für den neuen Account / neuen Chat

```text
Lies HANDOFF.md, AGENTS.md und docs/agent-tv-setup-guide.md im Repo https://github.com/pmhofer/CleanFrame.

Wir setzen mein Samsung-TV-Projekt fort (CleanFrame + TizenTube für werbefreies YouTube).
Aktueller Stand: Phase 1 (24h-Uhr, deutsches Datum, Dock unten, meine 13 Wunsch-Apps) ist fertig und gepusht.

Hilf mir jetzt beim nächsten Schritt:
- Falls du lokal auf meinem Mac im WLAN bist: Starte mit Schritt B (TV-Modell, Firmware, TV-IP, Mac-IP, Developer Mode & SDB-Verbindung), danach CleanFrame-Installation (Schritt C) und TizenTube-Installation + Verknüpfung auf die YouTube-Kachel (Schritt D).
- Falls du in der Cloud bist: Sag mir kurz Bescheid, dann feinschleifen wir zuerst weitere UI-Wünsche im Fork.

WICHTIG: Keine Native-Home-/Startup-Änderungen (Phase 4) ohne meine ausdrückliche Freigabe und vorheriges Backup.
```
