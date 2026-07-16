# BallSheet

A browser aim trainer with **true 1:1 sensitivity matching** — what your hand does here is exactly what it does in your game.

**Play it: https://plivdan.github.io/ballsheet/**

Overlap your cursor ball with the target ball to eat it. Every eat scores points and restores HP. HP drains faster the longer you survive — when it hits zero, the run ends.

## Why sensitivity matching matters

Most browser aim games use your OS pointer, so your mousepad-to-screen mapping depends on Windows settings, pointer acceleration, and window size. Aim training only transfers to your game if the *physical distance your hand moves* maps identically.

BallSheet uses the Pointer Lock API with `unadjustedMovement` (raw input — no OS acceleration, no pointer scaling) and maps it the way aim trainers do:

```
counts per 360° = DPI × cm/360 ÷ 2.54
cursor px per count = screen width ÷ counts per 360°
```

One full 360° worth of mouse travel sweeps the full width of your screen. Enter your DPI and cm/360 — or just pick your game and type your in-game sens, and the converter fills in cm/360 using the game's yaw value:

| Game | Yaw (°/count) |
|---|---|
| Marvel Rivals | 0.0175 |
| CS2 / CS:GO | 0.022 |
| Apex Legends | 0.022 |
| Valorant | 0.07 |
| Overwatch 2 | 0.0066 |

Yaw values follow [KovaaK's custom sensitivity scales](https://wiki.kovaaks.com/en/home/KovaaK's/CustomSensitivityScales).

`cm/360 = 360 × 2.54 ÷ (yaw × sens × DPI)`

## Modes

| Key | Mode | Cursor/Target | HP | Notes |
|---|---|---|---|---|
| 1 | SB — Small Ball | 30/30 | 100 | precise flicks |
| 2 | BB — Big Ball | 30/60 | 100 | the classic |
| 3 | BBB — Burst Big Ball | 30/60 | 49 | half-HP sprint |
| 4 | SBB — Shorter Big Ball | 30/60 | 75 | shorter clock |
| 5 | BC — Ball Cheese | 69/69 | 100 | 69 everything |
| 6 | SSB — Small Balls | 5/5 | 100 | pixel-perfect |

Scoring: each eat is worth `score_per_ball × min(reaction, cheese) ÷ cheese`, so instant "cheese" eats (target spawning on your cursor) are worth less. HP drain is `pressure × ln(1 + elapsed)` per second — survival gets exponentially harder.

## Notes

- Chrome or Edge recommended — they support raw input (`unadjustedMovement`). Other browsers fall back to OS-adjusted movement and show a warning.
- For exact parity with fullscreen native trainers, run your display at 100% scaling.
- Run history and settings are stored in `localStorage`. Nothing leaves your machine.
- Keys: `R` restart · `E`/`M` menu · `1–6` modes · `F` fullscreen · `Esc` release mouse.

## Credits

A web reimplementation of **BallSheetOGL** by [helloimxtal](https://github.com/helloimxtal), via an intermediate Python/pygame-ce port. Game constants (mode presets, drain curve, cheese scoring) are kept faithful to the original.

## Roadmap

- Adaptive mode: Fitts'-law-based skill model (speed, throughput, consistency, precision, endurance, flick, direction) driving per-weakness training stimuli
- Session analytics: RT distributions, fatigue curves, spatial heatmaps, movement rose
- Import history from the desktop Python version
