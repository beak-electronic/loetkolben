# BEAK Lötkolben-Tetris v0.5

Ein-Datei HTML/Canvas-Minispiel (Schwester von BEAK Breakout / Leiterbahn-Puzzle).

## Lokal

```bash
python3 build.py                 # → index.html (fonts+Ava inline; Musik separat)
python3 build.py --pages         # + Manifest/Icon-Links
PLAYWRIGHT_SKIP_VALIDATE_HOST_REQUIREMENTS=1 node tests/playwright.test.js
```

Musik liegt unter `music/` (nicht eingebettet): `titel.*` (Menü), `loetkolben.*` (Spiel).

## Sounds

| Event | Quelle |
|-------|--------|
| Level-Start / Level-Up | Ava `los-gehts` |
| Reihen-Clear | Synth-Zischen+Ping, dann Ava `prima` |
| Game Over | Ava `mist` |
| Drehen | Synth-Klick |
| Hard Drop | Synth-Thud |
| Lock / Soft-Drop-Aufsetzen | Synth-Tick |
| Menü-Musik | `music/titel` (Schleife 80 s) |
| Spiel-Musik | `music/loetkolben` (Schleife 66.2066 s) |

Musik-Knopf (♪) bzw. Taste **M** — Zustand in `localStorage` (`loetkolben-tetris.musik`). Ava/SFX bleiben davon unabhängig.

## Steuerung

- **Tastatur:** ←/→ bewegen, ↓ Soft Drop, Leertaste Hard Drop, ↑/X drehen, C/Shift Hold, P/Esc Pause, M Musik
- **Touch:** Wischen L/R, Tippen drehen, Wischen runter Drop, Hold-/Pause-/Musik-Button

## Rekord

Lokal in `localStorage` (`loetkolben-tetris.rekord` + Top 5). Kein Online-Highscore.

## iOS Home-Screen (Blur/Chrome)
Wie Bestückungs-Memory: `apple-mobile-web-app-status-bar-style=default`, opaker `#top-chrome` + `.ios-pwa-status-strip`, `--chrome-extra` gegen Liquid-Glass-Blur, DPR bis 3, opake Pause-/Musik-Buttons.

## Audio (iOS / Home-Screen-App)
Musik und Ava laufen über Web Audio. Auf iOS (besonders Standalone/PWA):
- `navigator.audioSession.type = 'playback'` (ignoriert Lautlos-Schalter, iOS 16.4+)
- AudioContext create/resume synchron in `touchend`/`pointerup`/`click`
- Nach App-Wechsel/Sperre: `interrupted` → Neustart beim nächsten Tap (`pageshow`/`visibilitychange`)
- Musik-URLs mit `?v=0.5` Cache-Bust (kein Service Worker)
- Diagnose-Overlay: `?debug=audio`
