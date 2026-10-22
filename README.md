# GearBound

*Steam & Thunder Artillery*

A turn-based artillery game for the browser (HTML5 Canvas + vanilla JS, no build step, no network dependencies).
The gameplay is modeled on classic delay-based artillery games: slope-relative aiming, hold-to-charge power, wind,
destructible terrain, and delay-based turn order. All names, art and text are original.

**Play:** open `index.html`, or the single-file build **`dist/gearbound.html`** (works offline from file://; `dist/game.html` is a legacy copy).
For phones, tablets and online play over the internet, host the folder **`dist/web/`** on any static https host (see *Deploying*).

**Flow:** Title (type a pilot name, pick an avatar; no sign-in of any kind) → **Main lobby** (room list, create / join-by-code / recent rooms,
scrolling **rankings panel**, shop icon) → **Waiting room** (Team A vs Team B up to 4v4, rigs, items, map, chat, ready toggles, **10→0 countdown**)
→ painted **loading screen** → **Battle** → **Results** (stats, gold, XP and rank-up) → back to the room.
Your profile (pilots, gold, XP, owned items, loadout) and settings are saved on the device in `localStorage`.

## Controls
| Key | Action |
|---|---|
| ← / → | Move (uses the per-turn MOVE bar; moving adds delay) |
| ↑ / ↓ | Raise / lower the barrel (limited to the rig's angle range, relative to its tilt on the slope) |
| Hold **Space** | Charge power, release to fire (the white marker shows your last shot's power) |
| 1 / 2 / 3, Tab | Select Shot 1 / Shot 2 / **Overdrive** shot (needs a full OVERDRIVE gauge) |
| P | Pass the turn (+500 delay) |
| W A S D / mouse drag | Scroll the camera. **C** re-centers it |
| M | Mute all sound |
| 5–8 | Use the power-up in that item slot (before you fire) |
| Enter | Chat (online) |
| ⚙ (top right) | Settings: **Music**/**Effects** volume, mute, Graphics quality (Auto/High/Low), on-screen touch controls, TURN relay (server/user/password) |

**Touch (phones/tablets, auto-detected):** ◀ ▶ drive, ▲ ▼ aim, hold the big **FIRE** button to charge and release to shoot,
1/2/OD shot buttons, PASS, the item bar for power-ups, ⊕ re-centres the camera. Drag the battlefield to look around, pinch to zoom,
tap the minimap to jump the camera. Phones in portrait get a "rotate your device" hint during battle; menus work in both orientations.

## Rules
- **Delay-based turns:** each shot has a delay cost (Shot 1 < Shot 2 < Overdrive), and moving adds delay per pixel.
  Whoever has the lowest total delay goes next, so cheap shots can earn you extra turns. The order and delay totals are shown top-right.
- **Turn timer:** 20 s. If time runs out while you charge, the shot fires; otherwise the turn is passed.
- **Wind (360°):** each turn rolls a strength (0–12) and a direction anywhere on the circle. The top-centre dial's arrow points where
  the wind blows, with the strength in the middle: an upward gust lifts long lobs, a downward one drops them short. Some shots resist wind.
- **Weather columns:** from turn 2, each turn has a 30% chance (max 2 active) to open a column for 2–3 turns, announced with a banner:
  **Lightning Column** +25% damage, **Sunbeam** +20%, **Laser Pillar** +30% (a shell is charged once per column), and **Tornado**:
  a shell entering it while still *climbing* is flung ×1.35 faster, one entering on the way *down* is braked ×0.6.
- **Damage** falls off with distance from the blast center, and **armor** reduces it. A direct hit adds a +15% bonus.
- **Terrain** is a destructible bitmap. Explosions leave craters, rigs fall when the ground under them is gone,
  and any rig that falls into the void at the bottom is destroyed.
- **Overdrive gauge:** it fills by +20% per turn and also from damage dealt or taken. Firing the Overdrive shot empties it.
- The last team standing wins.

## Rig roster (14)
| Rig | Role | HP | Armor | Shot 1 | Shot 2 | Overdrive |
|---|---|---|---|---|---|---|
| **Bastion** | Siege | 1100 | 20% | Slug | Twin Slug (near + far) | Quake Mortar (huge crater) |
| **Skipper** | Scout | 900 | 8% | Hopper (bounces twice) | Scatter (3-shell fan) | Cluster Storm (5 bomblets at apex) |
| **Lancer** | Precision | 850 | 12% | Pulse Bolt (flat, wind-resistant) | Bore Drill (tunnels) | Sunlance (sky beam) |
| **Lodestone** | Control | 1000 | 16% | Mag Slug | Lodestone Pull (yanks enemies 60px toward the blast) | Polarity Storm (drags 110px) |
| **Carom** | Trickster | 860 | 8% | Carom Shell (2 ricochets, +15%/bounce) | Bank Shot (4 bounces) | Pinball Barrage (3 ricochet shells) |
| **Beetle** | Ground control | 980 | 14% | Crawler Mine (crawls 260px to a rig) | Lob Shell | Mine Parade (3 mines) |
| **Hornet** | Hunter | 820 | 6% | Stinger (fast dart, half wind) | Cog Drone (homes within 420px) | Swarm (3 drones) |
| **Mason** | Builder | 1150 | 22% | Brick Lob | Rampart (ADDS a 44px mound) | Landslide (damage + new rock) |
| **Glacier** | Disruptor | 940 | 12% | Frost Shell (+80 delay to targets) | Cold Snap (+160) | Deep Freeze (+300) |
| **Volt** | Sniper | 880 | 10% | Rail Spike (pierces terrain and rigs) | Chain Bolt (jumps to 3 enemies) | Thunderhead (chains to 5) |
| **Flak** | Area denial | 1000 | 15% | Flak Burst (airburst, 6 shards) | Oil Bomb (burning oil, 3 turns) | Shrapnel Rain (12 shards) |
| **Gyro** | Skirmisher | 900 | 10% | Boomerang (curves back) | Twin Rang | Gyre Wheel |
| **Orrery** | Field control | 900 | 10% | Pulse Orb | Gravity Well (bends every shell for 2 turns) | Singularity |
| **Spindle** | Tunneler | 920 | 12% | Auger Shell (burrows 150px into ground, then erupts) | Spiderlings (3 small burrowers fan out underground) | Deep Eruption (steers under the nearest enemy and erupts beneath it) |

**Spindle** is a brass spider walker with the pilot riding in an open hatch on top. Its shells do not stop at the ground:
on contact they keep their heading and tunnel at a fixed speed (no gravity, wind, weather columns or wells underground),
carving a tunnel. If a shell breaks out of the far side of a hill before its burrow length runs out it flies on and
explodes on its next contact; otherwise it *erupts*: it climbs straight up to the surface (carving a shaft) and explodes there.
Everything is deterministic, and online play syncs the tunnels like any other crater. A Teleport Shot from Spindle teleports normally.

`node tools/shotstest.js` fires every shot of every rig and reports projectile kinds, damage, delay added, pushes and hazards.

## Power-ups (items)
Buy them in the Shop → Power-ups tab, equip up to 4 in the waiting room (slots 5–8). **One item per turn, used before you fire,
and every item adds turn delay immediately** (shots cost 580–1050, a pass 500). Online, the host validates ownership, the one-per-turn
rule and the delay from its own catalog.

| Item | Delay | Effect |
|---|---|---|
| Repair Kit | +320 | Restore 30% max HP |
| Field Repair Crate | +380 | Every living teammate restores 18% |
| Twin Loader | +400 | Fire this turn's shot twice |
| Teleport Shot | +360 | You reappear where your shot lands (no damage; off-map/void = fizzle, you stay) |
| Aegis Plate | +210 | Block 40% of incoming damage until your next turn |
| Pressure Valve | +120 | +15% damage this shot |
| Still Air | +90 | Wind drops to 0 this turn |
| Gust Vane | +70 | Reverse this turn's wind |

## Ranks and XP
40 ranks from **Little Egg** (0 XP) to **Diamond Dragon** (500,000 XP), each with its own painted creature icon shown next to the
pilot's name in the lobby, rooms, nameplates and results. The main lobby has a scrolling rankings panel that highlights your rank.
**Match XP** (typical match ≈ 100–250): win 120 / loss 50, +1 per 20 damage (cap 100), +25 per kill, +2 per shot (cap 20), +10 for surviving.

## Shop tiers and perks
**Gold items** (hats, outfits, pets) are bought with gold earned in battle. Each one grants a *small* combat bonus that the
simulation applies (`js/perks.js`, used in `main.js` and `physics.js`). The tooltip and card for every item list its stats.

| Item | Bonus |
|---|---|
| Flight Cap | +6% move distance |
| Tin Crown | +4% attack damage |
| Tall Hat | −3% shot delay |
| Horned Helm | +5% max HP |
| Royal Coat | +3% attack, +2% max HP |
| Ember Vest | +3% attack |
| Night Suit | −3% shot delay |
| Glim (wisp) | 6% chance to deflect half of a hit |
| Pip (bird) | +6% move, 2% deflect |
| Blob (slime) | +3% max HP, 4% deflect |

**Caps on the total from all equipped items:** attack ≤ +8%, shot delay ≤ −6%, max HP ≤ +8%, move ≤ +15%, deflect chance ≤ 8%
(a deflect halves one hit). The starter kit has no perks. Deflect rolls use the battle's seeded RNG.

**Premium items** (`currency: 'premium'`) are cosmetic or economy-only and never stronger in combat: shell trails (Prism, Comet),
explosion skins (Confetti Burst, Clockwork Sparks), emotes (Tip of the Hat, Victory Jig), name colors (Gilded, Aurora), and
Gold/XP gain boosts (+20%). They appear under the shop's **Premium ✦** tab priced "Coming soon" and can't be bought yet.

## No sign-in: pilot name + avatar
There are **no third-party logins** (no Google/Apple/Microsoft/Yahoo buttons). You type a pilot name, pick an avatar, and the
profile lives on that device. `js/auth.js` is still loaded as an internal abstraction, but no screen calls it.

**Moving progress between devices:** the title screen's **⇄ Move my progress to another device** button shows an **export code** (`GB1-…`, a checksummed copy of the
profile) that you can paste into *Import* on another device. Real cross-device sync would later need a small account system of our
own (e.g. a username + PIN or a one-time transfer code backed by a tiny database); no Big-Tech sign-in is planned.
Because profiles are local, gold/XP can be edited by a determined player; online, XP and gold are self-reported.

## Online play
Peer-to-peer over WebRTC using PeerJS (vendored in `js/vendor/`), **host-authoritative**: the host runs the simulation and
streams snapshots (positions, HP, projectiles, effects, sounds, terrain carves) to everyone; clients send inputs only.
- **Main lobby:** live room directory (via the PeerJS broker), *Create room*, *Join by 5-letter code*, recent rooms.
- **Waiting room:** up to 4v4, team switching, rig/item picks, map pick (host), chat, ready toggles; when everyone is ready the
  host starts a **10→0 countdown** (un-readying cancels it).
- If a pilot leaves or their connection goes silent for 15 s, the host drops them and **skips their turns**. Closing the tab sends a "bye" immediately.
- Uses the public PeerJS cloud broker (`0.peerjs.com`, wss) and public STUN. Strict NATs/corporate networks may need a TURN relay:
  fill in TURN server / user / password in Settings → Online.
- Tested: desktop↔desktop and desktop↔emulated-iPhone, 6 turns each, host and client terrain/HP/positions identical every turn
  (`node tools/onlinetest.js`, `MOBILE_CLIENT=1` for the phone variant).

## Phones, tablets and the web app
- Touch controls (see *Controls*) appear automatically on touch devices; toggle them in Settings.
- Responsive layouts for title, lobby, shop, waiting room, battle HUD and results in phone landscape/portrait and tablet sizes;
  safe-area insets for notches; page zoom disabled (`viewport` + gesture blocking); audio unlocks on the first tap (iOS).
- **Graphics: Auto** picks *Low* on phones/low-memory devices: lower canvas resolution, fewer particles and lighter post-processing.
- **Add to Home Screen:** `dist/web` ships a web-app manifest (fullscreen, landscape), icons, and a service worker that caches the game
  for offline play after the first visit (online matches still need a connection).
- `node tools/mobiletest.js` runs iPhone 14 and iPad (landscape + portrait) emulation, plays touch turns and saves screenshots to `screenshots/mobile/`.

## Deploying (GitHub Pages or any static host)
`node build.js` writes **`dist/web/`**: `index.html` (the whole game inlined, ~4.9 MB), `manifest.webmanifest`, `icons/`, `sw.js`, `.nojekyll`.
Upload that folder to any https static host. For GitHub Pages: push the contents of `dist/web/` to a `gh-pages` branch (or a `/docs`
folder) and enable Pages in the repo settings; the game is then at `https://<user>.github.io/<repo>/`. HTTPS is required for the
service worker and is what lets phones talk to the PeerJS broker (wss) and do WebRTC. All paths are relative, so any sub-path works.

## Riders, cosmetics and pets
Each player's avatar **rides on** their rig. Every rig defines a `mount` point (hatch, saddle or seat) in its local space.
The rider is drawn there inside the rig's transform, so it moves and tilts with the rig. It bobs while driving and
leans back from firing recoil. Equipped clothing (face, skin tone, hair colour, hat and outfit) show on the painted rider. A pet hovers near
the rig or perches on its back when the rig is idle. Every item is a data entry with a draw function in `js/avatar.js`,
which is the starting point for a future shop.

## Music (original, procedurally sequenced)
All music is original and composed for this project. It is written as note data in `js/music.js` and played live by a small
WebAudio step sequencer (lead, bass, chord stabs/arpeggios, pads, and synthesized kick/snare/hat/"clank" drums), so it works
offline with no audio files. Tracks crossfade (about 1.4 s) when you change area. Audio starts after your first click or key press,
as browser autoplay rules require.

| Area | Track | Feel |
|---|---|---|
| Title / login | **GearBound Overture** | C major, 132 BPM, heroic march |
| Room / lobby | **Hangar Lounge** | F major, 108 BPM, swung jazzy lounge with a walking bass |
| Shop | **Brass & Bolts Bazaar** | G major, 120 BPM, bouncy staccato with clockwork "clank" percussion |
| Map: Verdant Ridge | **Green Ridge Gallop** | D major, 144 BPM, galloping adventure |
| Map: Ember Dunes | **Ember Caravan** | A harmonic minor, 126 BPM, desert caravan |
| Map: Frostmere | **Frostmere Chimes** | E minor, 136 BPM, FM bell lead and arpeggios |
| Victory sting | **Gears of Glory** | about 3 s fanfare, then the lobby track fades back in |
| Defeat / draw sting | **Rusty Sunset** | about 4 s descending chromatic line |

To add a track, add an entry to `TRACKS` with `bpm`, `chords` (one per bar), `melody` (8th-note tokens, `-` = hold, `.` = rest),
the `bass`/`stab` styles and `drums` patterns.

## Art and rendering (painted art overhaul)
GearBound uses **painted raster art** for all of its visuals. The art was generated as original concept paintings for this project.
It contains no GunBound sprites, names or likenesses. Everything is embedded in the HTML, so the game still runs offline from one file.

**What is painted:**
- **Backdrops:** sky and vista for each map, plus a mid-ground silhouette layer.
- **Terrain:** a seamless soil/rock texture and a grass/sand/snow top strip for each map.
- **Rigs:** the three rig hulls, with the barrels as separate rotating sprites.
- **Riders and items:** 4 rider outfits, 4 hats and 3 pets.
- **Icons:** 9 shot icons and 10 premium icons.
- **Title key art.**

### Asset pipeline
- `assets/src/*.jpg` are the original generated images, kept untouched.
- `python3 tools/build_assets.py` (needs Pillow and numpy) processes them in these steps:
  1. **Chroma-key** the flat background. It estimates the key colour from the border, flood-fills from the edges,
     and removes enclosed holes such as wheel spokes only when they are close to the key colour.
  2. Soften the alpha edges, un-premultiply and **despill**.
  3. **Trim**, then **split** the sheets (riders, pets/hats and icon grids) into individual sprites.
  4. Make the terrain textures **seamless** (roll + cross-blend). Crop the top strips, and record each strip's surface line.
  5. Write each sprite as **WebP** to `assets/web/` (52 files, about 1.2 MB).
  6. Generate `js/assets.js`, which holds `AG.ASSET_SRC` (base64 data URIs), `AG.ASSET_META` (pivots, surface offsets)
     and `AG.FONT_SRC`.
- **Fonts:** Fredoka (display/HUD) and Nunito (body), SIL Open Font License 1.1. They are subset to WOFF in `assets/fonts/`
  (see `assets/fonts/LICENSE.md`).
- `js/art.js` decodes everything at boot, behind a small loader. It also caches tinted copies and pixel data,
  which terrain sampling uses, and builds the soft particle sprites.
- **Size:** `dist/gearbound.html` is about **1.8 MB** in total, well under the 15 MB budget.

### Renderer
- **Scene:** Canvas2D draws the scene into an offscreen canvas:
  - parallax painted backdrop, mid and near layers, haze and fog bands, and wind-driven cloud puffs;
  - ambient motes (pollen, embers, snow);
  - textured terrain with depth darkening, the painted top strip, a distance-field bevel and rim light, AO, scorch,
    extruded back walls and a blurred contact shadow;
  - sprite rigs and riders, particle explosions (fire, smoke, shock ring, textured debris), and animated water, lava and ice.
- **Post-processing:** `js/post.js` runs a **WebGL** pass:
  - a bright pass, plus a two-pass Gaussian **bloom** at quarter resolution;
  - up to 16 **dynamic point lights** (explosions, muzzle flashes, projectiles, the Sunlance beam);
  - a **colour grade** per map (tint, lift, saturation, contrast), a **vignette**, the hit flash and film grain.
  - If WebGL is unavailable, or with `?nogl`, it falls back to plain Canvas2D with a 2D vignette.
- **Camera:** smooth eased follow, with a slight zoom that eases in while aiming (1.18×), out in flight (0.96×) and back afterwards.
- **Overlays:**
  - A crisp overlay canvas carries the turn banner, off-screen arrows and messages.
  - The **HUD** is DOM/CSS: glassy panels with brass accents.
    - The pilot card has a portrait and a timer ring, next to the minimap, wind gauge and turn order.
    - The dock holds the HP/MOVE/OVERDRIVE bars, an angle gauge and the power track with a last-shot marker.
    - Shot cards carry painted icons and animate when selected or locked.
- **Screens:** title, lobby, shop and settings are restyled. They have key art, a metallic logo, glass panels,
  pill tabs and painted previews and icons.
- Before/after: `screenshots/before-art-overhaul.png` and `screenshots/after-art-overhaul.png` show the same map, seed and roster
  (`tools/showcase.js`). The earlier 2.5D pass is in `before-3d.png` and `after-3d.png`.

### Polish pass (rough edges fixed)
- **Explosions:** the white "small sun" is gone. Each blast has a warm orange core that ramps up over about 3 frames and blooms briefly
  (`AG.blastHeat`). It then hands over to orange flame puffs, which cool to soot and give way to smoke rising in.
  The dynamic light and screen flash were toned down to match.
- **Keying:** a new `depink()` step in the pipeline cleans magenta out of rigs, riders, pets, hats and barrels. It:
  - cuts saturated key-coloured holes;
  - removes pink pixels near the edges (for example the Lancer thruster glow and the wisp pet);
  - neutralises faint pink on the edge ring;
  - erodes the alpha edge by about 1 px.
- **Skipper:** its cannon now sits on a brass turret ring on top of the dome, instead of coming out of the nose.
- **Wheels:** the pipeline cuts wheels out of the hull paintings into their own sprites (`wheel_<rig>_<i>`).
  - They spin by the distance driven: the Bastion's 9 road wheels and sprockets, and the Skipper's big gear wheel.
    The Skipper's frame plate stays static and is drawn in front of its wheel.
  - The Lancer's side gear turns slowly while it hovers.
- **Riders:**
  - There are three painted faces: **Kit**, **Wren** (ponytail) and **Rook** (spiky hair, nose bandage). Each one has all four outfits.
  - **Skin tone** and **hair colour** are applied at runtime through skin and hair masks built for each sprite (`rmask_*`).
    The HUD portrait and shop icons use the same tinted art.
  - The shop has free **Face**, **Skin tone** and **Hair** tabs (6 hair colours).
- **Frostmere:** the backdrop is blurred, desaturated and lower-contrast, so the terrain reads first.
- **Spawns:** rigs always spawn on solid ground. That means the column reaching the world bottom, with level footing and open sky,
  never on a floating island or a pit lip. `tools/spawntest.js` checks 144 spawns across 3 maps × 12 seeds.
- **Parallax:** the mid and near layers are each drawn as a single copy, scaled to cover the full parallax travel.
  The near layer is a flipped, enlarged copy, so there are no mirrored repeats.
- **Ember Dunes pits:** each map now has one wide sloping basin (about 420–560 px) with a lava chute in the middle, instead of a slot canyon.

### Known art limitations
- The tread links on the Bastion are part of the hull painting. The wheels spin, but the track itself doesn't scroll.
- Hats use one fit per face, so a hat can sit slightly high on Rook's spiky hair.
- Skin and hair masks are colour-based. Tiny areas, such as the eyebrows and eyes, get tinted with the hair colour.
- The Frostmere backdrop is calmer, but still more detailed than the other two maps.

## Project structure
```
index.html          entry point (loads the scripts in order, no modules, so file:// works)
css/style.css       menu / overlay styling
js/assets.js        GENERATED by tools/build_assets.py: WebP/WOFF data URIs + sprite metadata
js/util.js          AG namespace, CONFIG tuning constants, seeded RNG, WebAudio synth sounds
js/art.js           image/font loader, tinted/pixel caches, soft particle sprites
js/terrain.js       bitmap terrain: generation, carving, 2.5D render (lit front face + extruded thickness), map themes
js/avatar.js        rider avatar drawing, clothing/hat/skin/pet catalogs, Pet entity
js/perks.js         gold-item combat perks + caps, premium (cosmetic/economy) catalog
js/auth.js          (unused) old sign-in abstraction, kept for reference; the game has no third-party login
js/items.js         power-up catalog, one-per-turn rule, delay costs, effects (host-validated online)
js/weather.js       360° wind and weather columns (lightning / sunbeam / laser / tornado)
js/ranks.js         40 ranks + match XP;  js/rank_art.js generated rank icons
js/loading.js       painted loading screens (art in js/loading_art.js, decoded on demand)
js/net.js, js/online.js  PeerJS transport, room directory, host-authoritative match sync
js/touch.js         touch controls, pinch/drag camera, quality scaling (lowFx)
js/mobiles.js       RIGS roster data (stats, shots, mount points), procedural rig art, Mobile entity
js/physics.js       projectiles, wind, shot behaviors (shell/bouncer/cluster/drill/lance), explosions, damage, effects
js/turns.js         Player (team, delay, special gauge, stats) + TurnSystem (lowest-delay-first)
js/music.js         WebAudio step sequencer, original track data per area, crossfades, victory/defeat stings
js/render.js        painted parallax sky/haze/fog, ambient motes, animated void, light collection, off-screen arrows, banner
js/post.js          WebGL post pass: bloom, dynamic lights, grading, vignette, grain (Canvas2D fallback)
js/hud.js           DOM battle HUD (pilot card, timer ring, minimap, wind, turn order, bars, angle gauge, power, shot cards)
js/ui.js            in-world name plates/HP pills, messages (legacy canvas HUD kept as reference)
js/screens.js       DOM screens: Title, Room/Lobby (4v4 slots, ready, map pick), Shop, Victory, Settings panel; profile save
js/main.js          Battle state machine (aim → flight → settle), menu backdrop demo, input, camera, render loop
build.js            node build.js → dist/gearbound.html (everything inlined) + dist/game.html copy + dist/web/ (deployable PWA folder)
tools/onlinetest.js two browsers play an online match through the real PeerJS broker and compare state every turn
tools/mobiletest.js iPhone 14 / iPad emulation with touch turns + screenshots
tools/shotstest.js  every shot of all 14 rigs;  tools/rankshots.js, tools/featureshots.js, tools/rigshots.js  screenshots
tools/playtest.js   headless Chrome test of the full flow (title → room → shop → battle → win), checking the music for each area and capturing screenshots
tools/shottest.js   fires every shot of every rig at a target and reports the damage
tools/closeup.js    zoomed render of the rigs with riders and pets, plus battle views of the Ember and Frostmere maps
tools/beforeafter.js renders the same scene with an old build and the current one (2.5D comparison)
tools/showcase.js   per-map gameplay, explosion and before/after screenshots (+ ?nogl fallback check)
tools/spawntest.js  asserts every spawn is on solid ground (3 maps x 12 seeds)
tools/shoplooks.js  screenshots of the shop Face / Skin tone / Hair tabs
tools/snap.js       quick title/map/explosion snapshots to /tmp
tools/build_assets.py  art pipeline (assets/src → assets/web + js/assets.js)
tools/audiocheck.js  confirms every music track and sting produces sound (measured with an AnalyserNode)
```
Teams are part of the data model. The Room supports up to 4v4 (Teams A and B), and `TurnSystem` and `Player` already handle more teams (A–D).

### Testing
```
cd tools && npm i playwright-core && cd ..
python3 tools/build_assets.py      # only after changing assets/src
node build.js && node tools/playtest.js && node tools/shotstest.js && node tools/mobiletest.js && node tools/onlinetest.js && node tools/shottest.js && node tools/showcase.js && node tools/closeup.js && node tools/spawntest.js && node tools/shoplooks.js
node tools/boomtest.js && node tools/riggallery.js && node tools/shoticons.js && node tools/rankshots.js   # boomerang hits, 14-rig gallery, 42 shot icons, rank art
```
(These scripts use `/usr/bin/google-chrome` with SwiftShader WebGL, so headless frame rates don't reflect real GPUs. Set `CHROME=/path/to/chrome` to use a different browser for `playtest.js`.)

## Known issues
- Painted art is AI-assisted and then keyed on magenta. The edges are now colour-bled, so the magenta halo is gone at normal zoom. At very high zoom a faint seam can still show, for example a small patched block on the top of Gyro's wheel. Some purple-themed items (Orrery, Volt and a few premium cosmetics) are purple by design.
- Spindle's *Deep Eruption* commits to its dive. Once it burrows, it keeps drilling through caves and air gaps at a fixed speed until it erupts. That keeps it deterministic online, but it can look odd crossing a gap. Its eruption can land slightly off-centre on a target that is moving or on a slope, so it often does less than its full 330 damage. In the test it did 157.
- Spindle has no walking-leg animation. It skitters with a bob and squash instead.
- Online play needs a TURN relay on strict networks (see Online). The automated online test runs on a local TURN server only.
- Touch controls were checked in an emulated iPhone 14 and iPad (landscape and portrait) with real CDP touch events, not on physical devices.

## Roadmap
1. **Gameplay:** sudden death (rising void / forced Overdrive), fall damage, more handcrafted maps, caves and moving islands.
2. **Online:** reconnect to a match in progress, spectators, a small relay/server for strict networks, server-checked XP and gold.
3. **Accounts (own, no Big Tech):** optional username + PIN or transfer-code sync for cross-device progress.
4. **Art:** animated rider poses, per-rig firing/recoil frames, more hand-painted terrain sets.
5. **Tech:** ES modules plus a tiny bundler, unit tests for physics and turns, fixed-point math for cross-browser determinism.
