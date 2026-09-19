# TIME TOWER

**Every mistake can be rewritten.** TIME TOWER now uses an **infinite procedural campaign**: Level 1 → Level 2 → Level 100 → Level 1000 and beyond. Each numbered level is rebuilt deterministically from its seed, so it can always be reproduced.

## Run

Open `index.html` in a modern desktop browser. For best results (and to avoid browser storage restrictions), serve the project folder with any static server, e.g. `python3 -m http.server 8080`, then open `http://localhost:8080`.

## Controls

WASD / arrow keys move; **R** rewinds (hold it to choose how far); Space interacts; Escape pauses. The rewind buffer holds five seconds at a time and its cyan meter is consumed as it is used. A failure freezes the scene; hold R to rescue the run or choose restart.

## Endless Tower

Choose **ENDLESS RUN ∞** from the main menu. The normal tower campaign is also infinite; each completion immediately generates the next numbered level. Endless Run starts a fresh score run from Floor 1. Daily Challenge uses a local date-derived seed, so every player on the same day receives the same floor.

## Procedural generation

`generateLevel(levelNumber, seed)` combines validated room templates with a deterministic seed and `difficulty = 1 + log2(level + 1)`. The climb starts gently: Levels 1–3 teach movement using three different room shapes, Levels 4–6 add keys, Levels 7–9 add relays, Levels 10–12 add pressure gates, Levels 13–18 introduce patrols, and Levels 19–24 introduce lasers. Levels 25–39 have six rotating medium-tower patterns, fair-but-misleading side routes, and one moving threat; Levels 40–59 are high-tower circuits with keys, relays, gates, branching routes, and both enemy and laser timing. Level 60+ adds extra hazards and advanced time-core combinations. Each tier adds planning decisions rather than simply increasing movement speed. The `LevelValidator` checks every room before it is played and rejects invalid layouts. Progress and highest unlocked level are saved in local storage.

## Puzzle progression and solutions

Each room is deliberately winnable without relying on luck. The on-screen briefing gives the immediate objective; this is the intended learning curve:

1. Walk right to the exit.
2. Go down into the lower service tunnel, cross below the laser, then come back up at the far end.
3. Go down at the start, collect the key in the lower corridor, return to the top corridor, and open the door.
4. Step on the purple pressure plate. Its door stays powered for three seconds—run through it.
5. Watch the patrol move away from your approach route, then sprint to the exit. Rewind if it turns toward you.
6. Wait for each moving beam to leave the corridor before crossing its tile.
7. Step beside the switch and press Space, pass the now-open relay door, then time the drone patrol.
8. First take the lower route for the key and evade the patrol. Return upstairs, press Space at the relay, pass the relay and key doors, charge the pressure plate, cross the pressure-only door, wait for the final laser, and escape.

## Project layout

- `index.html` – launch point
- `styles.css` – menus and HUD
- `scripts/levels.js` – eight editable ASCII levels
- `scripts/game.js` – gameplay, rewind snapshots, rendering, audio, persistence
- `assets/` – reserved organized folders for replacement art/audio (`player`, `enemies`, `environment`, `ui`, `audio`, `effects`)

## Modify

Player move timing is `MOVE_TIME`, rewind duration is `MAX_HISTORY`, and energy is `MAX_ENERGY` at the top of `scripts/game.js`. Add a level by appending to `TIME_TOWER_LEVELS`; the tile legend is in `levels.js`. Enemy configuration is `enemy:{x,y,axis,range}` and animated beams use `lasers`.

## Export

For a hackathon build, upload these files to GitHub Pages/Netlify, or package the folder with Electron/Tauri. Since it is plain web technology, no game-engine installation is required.
