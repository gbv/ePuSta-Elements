# ePuSta Elements — Live Demo

Diese Demo zeigt `ePuStaInline` und `ePuStaGraph` jeweils als JavaScript-Klasse und über data-Attribute.

Alle Anfragen an den ePuSta-Server werden auf lokale Beispiel-JSON-Dateien umgeleitet (`statistics-*.json`), die echte Daten des Dokuments `mir_mods_00079200` (Juli 2025 – Juni 2026) enthalten. Ein laufender ePuSta-Server ist nicht nötig.

## GitHub Pages

Die Demo ist online verfügbar, sobald GitHub Pages für das Repository aktiviert ist:

**https://gbv.github.io/ePuSta-Elements/example/**

### Pages aktivieren

1. Repository auf GitHub öffnen
2. **Settings** → **Pages**
3. Unter **Source**: `Deploy from a branch`
4. Branch `main`, Ordner `/ (root)` wählen
5. **Save** klicken

Nach 1–2 Minuten ist die Demo unter der URL oben erreichbar.

## Lokal

ES-Module lassen sich nicht über `file://` laden, daher wird ein lokaler HTTP-Server benötigt. Vom **Repo-Root** aus starten:

```bash
python3 -m http.server
```

Dann im Browser öffnen:

```
http://localhost:8000/example/
```

Alternativ mit Node.js:

```bash
npx serve .
# Demo unter http://localhost:3000/example/
```
