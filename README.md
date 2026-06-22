# PAWPAW WORLD 3D

PAWPAW WORLD is a browser game about PawPaw, a cyber-cat exploring a dense neon city — now rebuilt as a full 3D game with a third-person mouse-look camera.

The game is built with vanilla JavaScript, Three.js (vendored locally in `lib/`, no internet required at runtime), WebAudio, and localStorage. There are no build steps, no paid systems, and no gambling or real-money loot mechanics.

The original 2D Canvas version is preserved untouched in [`legacy-2d/`](legacy-2d/) and still runs by opening `legacy-2d/index.html`.

## Run

```bash
python3 serve.py 8741
```

(`serve.py` is a plain `http.server` with caching disabled so code changes show up on reload; `python3 -m http.server 8741` works too.)

Open:

```text
http://localhost:8741
```

A local server is required (ES modules don't load from `file://`). On the title screen choose **START NEW** (erases any save) or **RESUME WANDER** (continues your save), then click the canvas to capture the mouse.

## Controls

| Input | Action |
|---|---|
| Mouse | Look around (pointer lock — click the screen to capture the mouse) |
| W A S D / arrows | Move relative to the camera |
| Shift | Sprint |
| Space | Jump — double jump after unlock; hold to glide with Cloud Pads; bail off a zipline |
| X | Teleport dash after unlock |
| E | Interact — talk to quest NPCs, take courier jobs, rescue kittens, deliver at the shrine, browse the vendor, open caches, tap vending machines, scan posters, ride ziplines, scratch passers-by |
| F | Meow — scatters pigeons, makes NPCs react, feeds the flow combo |
| Mouse wheel / two-finger / pinch | Zoom the third-person camera (and the map, when open) |
| M | City map — mission route, NPCs, errands, discoveries, activities, distance rings |
| J | Journal — collection, main story, NPC quests, discoveries, achievements, lore archives |
| Esc | Release mouse / pause; closes modals |

## Core Loop

1. **Follow the gold beam.** Twenty story missions plus a repeatable courier tail point across landmarks, collecting, delivery, rescue, ziplines, NPCs, discoveries, FLOW, leveling, and the apex spire.
2. **Help the city's people.** Four quest NPCs (Solder → Vega → Patch → Jin) give multi-step chains, while eight additional named neighbors offer conversations and rewards.
3. **Run courier jobs.** Take timed deliveries from courier boards; back-to-back runs build a streak multiplier.
4. **Chase the rotating City Tasks.** Three short goals are always on-screen (collect kibble, jump rings, catch wisps, knock cans, scatter pigeons…), refilling as you complete them.
5. **Chain a FLOW combo.** Anything you do quickly in a row builds a combo that cashes out for bonus credits.
6. **Explore for secrets.** 8 hidden discoveries are tucked in alley dead-ends and on rooftops you have to climb — each gives credits, XP, and a piece of lore.
7. **Collect & grow.** 140 holo-kibble (10 unlocks Teleport Dash, 25 Double Jump), 18 data chips, 10 lost memories, 8 lost kittens to carry to the Paw Shrine, 4 hidden caches, neon rings, vending machines, and posters.
8. **Spend & level.** Earn credits and XP from everything; upgrade at Mei's stall (sprint/jump/dash, gliding, kibble magnet, snacks, collar colors), level up, and chase achievements.

A nearby-activity hint and an on-screen compass always point to the closest worthwhile thing.

## World

- Five districts, each with its own character: graffiti and steam in the Back Alleys, food stalls / lantern strings / a giant rotating holo-cat in the Neon Market, the shrine + fountain plaza, searchlights and round glass towers in the Corporate Sector, and a lonely antenna farm with holo-fish and an elevated train (sometimes a ghost train) at the Old Signal Docks.
- Procedural buildings (seeded — identical every visit) in three shapes — simple towers, tiered setbacks, and cylinders — with richly **textured PBR facades**: panel seams, lit/dark windows, grime streaks, glowing storefronts, balconies, pipes, water towers, pagoda roofs, animated billboards, and vertical neon signs.
- **Cinematic rendering**: real bloom post-processing, ACES filmic tone mapping, MSAA + anisotropic filtering for crisp textures, image-based lighting so wet roads and metal pick up colored neon reflections, drifting street haze, volumetric lamp cones, and a glowing horizon skyline.
- City life: walking citizens and robots with articulated limbs and speech bubbles, hover-car traffic, patrol drones with scan cones, cleaning bots, a street performer, scatter-able pigeon flocks, holo-cats on rooftops, glowing puddles, steam vents, and glowing eyes watching from dark alleys.
- Cat-scale climb language: pawprints and glowing crates mark a route up almost every building — crate stacks, fire escapes, awnings, balconies, roof-to-roof bridges, rideable **ziplines**, and a starter "CLIMB ME" tower beside the spawn.
- Day/night cycle, drifting rain, and a **reactive soundscape**: an evolving chord-progression music bed with a bell melody through a reverb bus, plus rain/traffic/neon-buzz ambience layers that swell with what's around you, and footsteps.
- Juicy movement: squash-and-stretch cat animation, sit / stretch / purr when idle, pawstep sparks, jump/dash/landing bursts, sprint FOV kick, and a smoothed third-person camera that slides in front of walls instead of clipping.

## Save System

Progress saves automatically to localStorage every few seconds and on exit: collectibles, credits, XP/level, abilities, upgrades, collar color, kitten rescues, shrine level, achievements, **main-mission progress, side-quest states, discoveries, courier streak, rotating tasks**, and your position in the city.

## Project Structure

```text
index.html           Title screen, HUD, and modal markup
style.css            Neon UI styling, animated title, map, trackers
serve.py             Static dev server with caching disabled
lib/three.module.js  Vendored Three.js (r160)
lib/jsm/             Vendored post-processing (EffectComposer, UnrealBloom, OutputPass)
src/data.js          Config, districts, lore, shop, kittens, missions, side-quests, discoveries, shared state
src/world.js         Procedural city, PBR facades, shrine, NPCs, drones, ziplines, rain, day/night
src/player.js        PawPaw mesh, movement physics, zipline riding, mouse-look + zoom camera
src/game.js          Collectibles, missions, side-quests, discoveries, courier jobs, flow combo, XP, save/load
src/ui.js            HUD, trackers, compass, popups, city map, journal/shop/dialogue modals
src/fx.js            Particle bursts, ambient emitters, reward floaters, screen flashes
src/audio.js         WebAudio music bed, reactive ambience layers, sound effects
legacy-2d/           The original 2D Canvas version, unchanged
docs/                Player manual and detailed project report
report_assets/images/  Real in-game screenshots used by the generated report
report_assets/videos/  Opening, gameplay, and PawPaw character-study recordings
build_report.js      Generates PAWPAW_WORLD_Report.html
PAWPAW_WORLD_Report.html  Project report (videos remain external for fast playback)
```

The original 2D Canvas version is preserved in `legacy-2d/` and still runs by opening `legacy-2d/index.html`.

## Verification

Syntax-checked with Node and smoke-tested in a browser through a local server: movement, sprint, jump physics, mouse-look and wheel zoom, the main-mission chain (reach checks complete and advance), the full NPC side-quest loop (accept → errand → return → unlock next), hidden-discovery triggers (with out-of-building nudging so every spot is reachable), the kitten rescue → shrine delivery loop, courier jobs, flow combo, vendor purchases, map/journal open and render, save/load across reloads, and zero console errors at a steady ~60 fps.
