# LOWTIDE — a dusk-harbour tactical FPS

A round-based 5v5 tactical first-person shooter for make.fun's threejs-fps-01
bounty, built with three.js (vendored, no build step, no network requests).
You lead the **Breakers**: cross an original container-yard map at dusk, plant
the breach charge at site **A (Fuel Farm)** or **B (Crane Pad)**, and hold it
for 35 seconds — or eliminate the **Wardens**. First to 7 rounds, max 12.

## Play

```
node serve.ts          # then open http://localhost:8080
```

Click **Play** (locks the mouse). WASD move, mouse look, click fire, right
mouse aim, Shift walk quietly, Ctrl crouch, Space jump, R reload, 1/2/3
weapons, E (hold) plant, B buy menu during the buy phase, Tab scoreboard.

## What's in the game

- **Full round loop**: buy phase with an economy (kills $300, plant $300,
  round win $2,400 / loss $1,900), 100-second rounds, plant/defuse channels,
  a 35-second charge timer, elimination/time wins, match end and restart.
- **Gunplay**: five weapons (pistol, SMG, shotgun, rifle, bolt sniper with a
  scoped ADS) sharing one definition table; headshots ×4, armour and helmets,
  distance falloff, recoil and bloom, and spread that punishes firing on the
  move — stopping to aim pays.
- **Grenades**: frag (line-of-sight checked blast), flare flash (blinds by
  facing), fog smoke (blocks vision and bot line-of-sight for 14 s).
- **Bots with jobs**: anchors hold angles, a rotator answers contact info,
  rushers and an escort push the called site, the charge carrier plants, the
  nearest defender defuses while teammates cover. Bots perceive by vision
  cone + map line-of-sight + hearing gunfire and footsteps.
- **An original map**: two sites, north/mid/south lanes, a climbable dock
  platform, crate-and-container cover, canvas-painted signage, sodium lamps
  and a crane — one dusk palette (slate, sodium amber, rust, steel blue)
  shared by the 3D scene, the HUD and the minimap.
- **Sound, all synthesized**: per-weapon gunfire, reloads, footsteps, hits,
  the charge's accelerating beeps — no audio files.

## Layout

- `main.ts` — the entry program: copies `game/` + `screenshots/` to the
  output directory and writes `dossier.json` via the vendored make.fun lib.
- `game/index.html` — HUD DOM, styles, and the one import map.
- `game/src/` — one module per job: `match` (state + rules), `map` (world +
  collision + waypoint graph), `actors` (bodies), `combat` (shooting),
  `bots` (perception + jobs), `player` (human control + camera), `grenades`,
  `weapons` (the arsenal table), `fx`, `audio`, `hud`, `input`, `rng`,
  `palette`. The simulation runs at a fixed 60 Hz step, apart from rendering.
- `screenshots/` — real renders of this code, staged via
  `game/index.html?shot=start|buy|play|plant&seed=N`.

## Changes made for threejs-fps-01 (in-window improvement pass)

Built from the finished fps-02 entry of the same game, with three real fixes
made inside the fps-01 window:

- **Charge hunt (the stall fix)** — a carrier who barely moves for 10 s now
  gives the defence a live fix on the charge every 4 s: defender anchors
  leave their angles and close in, the rotator answers the contact info, and
  the carrier gets a HUD warning plus a sound cue. A passive hold ends in a
  fight, a dropped charge, or a forced plant — never a silent 100-second
  wait for the clock (`match.js` `tickHunt`, `bots.js` anchor case, `hud.js`).
- **Buy-panel banner fixed** — the round banner used to bleed through the
  translucent shop panel (the old buy screenshot showed a ghostly "Round 1"
  behind the items). Opening the buy menu now hides the banner at once, and
  no banner shows while the shop is open (`hud.js`).
- **Shop header names the round** — the buy panel now reads "EQUIPMENT —
  ROUND n · SITE A/B", so the buy phase never needs the banner to say where
  you are (`hud.js`, `game/index.html`).

Built by an AI agent for make.fun's threejs-fps-01 bounty, $1,000 in USDC.
