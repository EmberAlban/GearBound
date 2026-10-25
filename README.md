# GearBound

*Steam & Thunder Artillery*

A turn-based artillery game for the browser (HTML5 Canvas + vanilla JS, no build step, no network dependencies).
The gameplay is modeled on classic delay-based artillery games: slope-relative aiming, hold-to-charge power, wind,
destructible terrain, and delay-based turn order. All names, art and text are original.

**Play:** open `index.html`, or the single-file build **`dist/gearbound.html`** (works offline from file://; `dist/game.html` is a legacy copy).
For phones, tablets and online play over the internet, host the folder **`dist/web/`** on any static https host (see *Deploying*).

**Flow:** Title (type a pilot name once — it sticks — and pick an avatar; no sign-in needed for offline play; online play needs a free pilot account, see below) → **Main lobby** (room list, create / join-by-code / recent rooms,
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
| Enter | Chat (online): All/Team toggle, quick chats, Esc closes |
| Esc / ⚙ → ⏏ Leave match | Exit the match (with a confirm: *Stay* / *Leave match*). Offline or as a client you forfeit (no XP/gold, used items stay spent; online the others play on); as host it ends the match for everyone |
| ⚙ (top right) | Settings: **Music**/**Effects** volume, mute, Graphics quality (Auto/High/Low), on-screen touch controls, TURN relay (server/user/password). The panel has a pinned header with a **✕** close (the same one the Friends panel uses), scrolls inside itself so the ✕ never leaves short or standalone (Add to Dock) windows, and **Esc** closes it — Esc closes the top-most open panel (Friends, gifts, emote picker, rig details, dialogs) the same way |

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
  Small / Medium / Large funnels that reach from the ground to far above the top of the map (no lobbing over them;
  the pull tapers gently with height). A shell inside swirls around in slow motion, then leaves gently: never faster
  than 1.12× its entry speed. Climbing shells get a small nudge, falling ones are braked.
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
| **Lancer** | Precision | 850 | 12% | Pulse Bolt (flat, wind-resistant) | Bore Drill (tunnels in, bursts at the surface) | Sunlance (sky beam) |
| **Lodestone** | Control | 1000 | 16% | Mag Slug | Lodestone Pull (yanks enemies 60px toward the blast) | Polarity Storm (drags 110px) |
| **Carom** | Trickster | 860 | 8% | Carom Shell (one low skip, bursts on the 2nd impact, +15%) | Bank Shot (low skip, bursts on the 2nd impact) | Pinball Barrage (3 skip shells, each bursts on its 2nd impact) |
| **Beetle** | Ground control | 980 | 14% | Crawler Mine (crawls 260px to a rig) | Lob Shell | Mine Parade (3 mines) |
| **Hornet** | Hunter | 820 | 6% | Stinger (fast dart, half wind) | Cog Drone (homes within 420px) | Swarm (3 drones) |
| **Mason** | Builder | 1150 | 22% | Brick Lob | Rampart (ADDS a 44px mound) | Landslide (damage + new rock) |
| **Glacier** | Disruptor | 940 | 12% | Frost Shell (+80 delay to targets) | Cold Snap (+160) | Deep Freeze (+300) |
| **Volt** | Sniper | 880 | 10% | Rail Spike (flat, fast; bursts on contact with a dotted cut) | Chain Bolt (jumps to 3 enemies) | Thunderhead (chains to 5) |
| **Flak** | Area denial | 1000 | 15% | Flak Burst (airburst, 6 shards) | Oil Bomb (burning oil, 3 turns) | Shrapnel Rain (12 shards) |
| **Gyro** | Skirmisher | 900 | 10% | Boomerang (curves back) | Twin Rang | Gyre Wheel |
| **Orrery** | Field control | 900 | 10% | Pulse Orb | Gravity Well (bends every shell for 2 turns) | Singularity |
| **Spindle** | Tunneler | 920 | 12% | Auger Shell (skims under the surface, then erupts) | Spiderlings (3 small skimmers, staggered eruptions) | Deep Eruption (skims deeper, steers under the nearest enemy and erupts) |

**Spindle** is a brass spider walker with the pilot riding in an open hatch on top and its drill cannon mounted on the
**back of the abdomen**, behind the pilot (shells leave from that rear muzzle). Its shells do not stop at the ground: on contact
they dive to a shallow depth and **skim along just under the surface** (following the ground, carving a tunnel, kicking up dirt),
then **erupt** upward near the end of their run (or when they pass under an enemy) and explode at the surface. If the ground ends
under a skim (cliff, cave) the shell flies on and explodes on its next contact. Spiderlings split into 3 small skimmers that
erupt at staggered distances (they drill through each other's tunnels and the shared entry crater instead of mistaking them for open air); Deep Eruption skims deeper and steers into the nearest enemy.
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

**Collector items** cost gold only (there is no second currency) and are priced 5–10× the priciest regular item
(3,500–7,000 gold), so they are long-term goals. They are cosmetic or economy-only and never stronger in combat: shell
trails (Prism, Comet, Bubble), explosion skins (Confetti Burst, Clockwork Sparks, Petal Burst), emotes (Tip of the Hat,
Victory Jig, Big Wave), name colors (Gilded, Aurora), the Prism rig skin, and Gold/XP gain boosts (+20%, not in bot
matches). They live under the shop's **Collector ✦** tab, can be gifted at their gold price, and are free for the admin.
Owned as `prem:<id>`; older save key forms are migrated on load, never dropped. The trails, blasts, emotes, name colors
and Prism skin are collectibles for now (no in-battle visual yet); the two boosts apply to match payouts.

## Pilot accounts and cloud saves
There are **no Google/Apple/Facebook/Microsoft logins**. Offline and practice play need no account at all: you type a pilot
name, pick an avatar and play, and the profile lives on that device. A free **pilot account** is just a **pilot name + a secret
password** (6–72 characters, with a 👁 Show / 🙈 Hide toggle). It is needed for **online play** and keeps your progress in
the cloud. The title screen has **🔑 Log in / ✨ New pilot account**. ⚙ Settings has **Log out**, **Change password** and
renaming (the same name filter and uniqueness rules apply).

How it works (js/cloud.js, a tiny `fetch` client, no SDK):
- Backend: Supabase project `msvjemuwhkroqbkmsptc`. The publishable key in js/cloud.js is public by design; all protection is RLS + grants.
- **Sign-up** goes through the Edge Function `supabase/functions/signup` (`verify_jwt` off; CORS: emberalban.github.io,
  localhost/127.0.0.1 on any port, and `Origin: null` for the single-file `dist/gearbound.html` opened from disk). It applies
  the family-friendly filter (`namefilter.js` is generated from `js/namefilter.js` by `node tools/gen-edge-filter.js` — rerun
  it and redeploy the function after changing the word lists) plus reserved words, checks the name is free
  (case/space/`_.-`-insensitive: "Ace Pilot" = "ace_pilot"), and creates the user with the service-role key using the hidden
  address `<name_key>@pilots.gearbound.invalid` (`email_confirm: true`). That way, e-mail confirmation can stay ON in the
  project and the client never calls `signUp`. The same function handles **rename** (`{action:'rename'}`, requires the user's token).
- **Rate limits** (signup_attempts table): 30 attempts and 5 new accounts per IP per hour/day, 10 renames per pilot per hour,
  300 new accounts per hour overall.
- **Login** uses the password grant; the session (with refresh token) is kept in localStorage, so you stay logged in.
- **Cloud saves**: loaded on login. The first login on a device that already has progress **merges** (max XP and gold, union of
  owned cosmetics and item counts, friends kept; the cloud's friend code wins). After that every save (match end, shop,
  gifts, settings) uploads after 2.5 s (debounced). Offline changes are flagged and uploaded when the network returns; on a
  conflict both sides are merged. Logging into a different pilot on the same device **stashes** the previous pilot's save
  (`gearbound.acct.<id>`) instead of mixing them.
- **XP never goes down by accident**: the client refuses to upload a save with less XP than the cloud has, and the database
  trigger (`supabase/accounts.sql`) keeps the old save if XP drops, if the save is not a profile, or if it exceeds 300 KB.
  It logs each case to `save_anomalies` (client-side anomalies go to `gearbound.cloud.log`). The previous save is always kept in `prev_save`.
- **Cloud gift mailbox** (`supabase/gifts_by_code.sql`): if a friend can't be reached P2P, a gift is also dropped in the
  `gifts` table, addressed by friend code. The recipient picks it up after logging in (and every minute) with the same checks
  as P2P gifts; duplicates are ignored by gift id. RLS: you can only send gifts "from" your own friend code and only read or
  delete gifts addressed to yours.
- Database setup, in order: `supabase/setup.sql`, `supabase/accounts.sql`, `supabase/gifts_by_code.sql`. Deploy the function
  with `supabase functions deploy signup --no-verify-jwt`; it reads `SUPABASE_URL` / `SUPABASE_SERVICE_ROLE_KEY` from its env.

Limits (honest list):
- **No password reset by the player**: there is no e-mail. An admin can set a new password in the Supabase dashboard
  (Authentication → Users), where the login address is `<name_key>@pilots.gearbound.invalid`.
- The "account needed for online play" rule is enforced **in the client**. Matches are peer-to-peer, so a modified client could
  still join rooms without an account.
- A save is still written by the player's own device, so a determined player could raise their own XP/gold; the server only
  stops decreases and junk.

**Moving progress between devices:** the title screen's **⇄ Move my progress to another device** button shows an **export code** (`GB1-…`, a checksummed copy of the
profile) that you can paste into *Import* on another device. With a pilot account (below) this happens automatically.
Because profiles are local, gold/XP can be edited by a determined player; online, XP and gold are self-reported.

**Saves never wipe (and the old `/gearbound/` path):** the profile is migrated forward instead of being reset, the last 3 good
saves are kept as rolling backups (`gearbound.profile.bak`), an unreadable save is kept raw (`gearbound.profile.raw`) and the best
recoverable value wins, imports merge (max XP/gold, union of items) and the browser is asked for persistent storage.
The "wipe" players saw came from the repo rename: the old site `emberalban.github.io/gearbound/` now 404s, and iPhone/iPad
home-screen apps keep their own storage, separate from Safari, so the new site can't read a save made in an icon added from the
old path. Saves made in a normal browser tab share the same origin (`emberalban.github.io`) and are picked up automatically.
For home-screen apps, `rescue/gearbound/index.html` must be served at the **old** `/gearbound/` path (for example a `gearbound/` folder in an `emberalban.github.io` user-site repo; not yet tested): opening the old icon then
reads that app's save and sends it to the new site as `#gb1=<transfer code>`, which the new build merges in automatically
(or the player copies the code into *Move my progress → Import*).

**Sticky pilot name:** the name is asked for once. Returning pilots see *Welcome back, <name>* and go straight in; the name is also
kept in its own key (`gearbound.pilotName`) so it survives a lost/reset profile. Rename only in ⚙ Settings → Pilot name.

## Family-friendly filter
`js/namefilter.js` (offline word list, leetspeak/spacing/repeat-letter folding, Scunthorpe-safe word boundaries) is applied to:
pilot names (refused with a message), names received from other players (masked to a safe name on the host *and* every
client, so a hacked client can't push a bad name), room names (masked), chat and gift messages (bad words → `****`). `node tools/filtertest.js`.

## Online play
Peer-to-peer over WebRTC using PeerJS (vendored in `js/vendor/`), **host-authoritative**: the host runs the simulation and
streams snapshots (positions, HP, projectiles, effects, sounds, terrain carves) to everyone; clients send inputs only.
- **Main lobby:** live room directory (via the PeerJS broker), *Create room*, *Join by 5-letter code*, recent rooms.
- **Rig picker** (lobby and waiting room): one tap on a rig card selects *and* applies it (no extra confirm); **Done** stays
  visible at the top on phones in both orientations.
- **Waiting room:** up to 4v4, team switching, rig/item picks, map pick (host), chat, ready toggles; when everyone is ready the
  host starts a **10→0 countdown** (un-readying cancels it).
- If a pilot leaves (or forfeits, or doesn't come back within the 60 s reconnect grace), the host **skips their turns**. Closing the tab sends a "bye" immediately.
- Uses the public PeerJS cloud broker (`0.peerjs.com`, wss) and public STUN. Strict NATs/corporate networks may need a TURN relay:
  fill in TURN server / user / password in Settings → Online.
- **Reconnect grace:** a pilot whose connection drops mid-match (phone backgrounded, network switch) keeps the seat for 60 s and
  gets every missed terrain change on return; the host *holds* that pilot's turn instead of timing it out. Heartbeats every 2 s,
  20 s of total silence = presumed dropped (that only starts the grace, it never skips a connected pilot's turn).
- **Turn ack:** the host starts a turn's 20 s timer only after the active pilot's client acknowledges the turn, so a laggy link no
  longer loses half its turn (or the whole turn) before it even sees it. Inputs carry the turn number; stale ones are ignored.
- **Backpressure:** snapshots are full state, so the host skips stale ones on a slow link instead of building a multi-second queue;
  deltas (terrain carves, effects, sounds) always ride on the next snapshot.
- **Terrain self-healing:** at every turn start the host sends a checksum of the ground; a client whose ground differs (a lost
  or mis-applied carve) asks for the whole ground (run-length coded, ~10 KB) and replaces its copy, so a desync cannot outlive one
  turn. Non-finite carve values are never sent (they would decode as 0 on the clients).
- **Fairness:** only the active pilot's rig moves; other clients never receive the shooter's power while it charges (they see
  "X is aiming…"), only the shot once fired.
- **Force Start (host):** start without waiting for every READY (team rule still applies), with a confirm dialog. Unready pilots are
  pulled in with the rig they have selected.
- **Sudden Death setting (host, waiting room):** Off / 20 / 30 / 40 turns. From that turn on a *SUDDEN DEATH* banner shows and every
  pilot's Overdrive gauge is kept full.
- **Live ranks:** XP changes of pilots in the waiting room update their rank icons live.
- **Chat:** in the waiting room (inline) and in the match (under the minimap, **Enter** to open, **Esc** closes). All / Team toggle,
  quick-chat presets, name + rank icon per line, 120-char limit, no HTML, host-side rate limit (burst 4 / 8 s). Typing never triggers
  game shortcuts.
- **Vote kick (3+ pilots):** from the room slot or the in-match players menu. Host-tallied; passes only with a *strict* majority of
  eligible voters (everyone but the target, starter counts Yes) **and** at least 2 Yes, so ties fail; 30 s timeout (no vote = No),
  one vote at a time, 60 s cooldown per starter. The kicked pilot forfeits (turns skipped), sees "You were voted out" and can't rejoin
  that room this session. If the host is voted out the match ends for everyone with a notice (no host migration).
- **Bot matches** (any pilot flagged `bot`) pay only **1 XP + 1 gold for a win** and nothing otherwise. The current build has no bots,
  so this only matters once they exist.

## Friends and presence
The **Friends** button (lobby and waiting room header, with an online-count badge) opens a corner panel (a bottom sheet on phones).
Each device has an 8-character friend code. Add by code → the other side gets a request → accept. Presence runs through a small
PeerJS peer per player on the public broker (no servers, no logins): green dot = online, *in room XYZ* with a **Join** button when
the room is open, **Invite** while you're in a room. Online friends are listed first. Friends are stored in the profile.
Offline friends show when they were last online, and 💬 opens a 1:1 chat (see *Round 10/24 fixes*).

## Gifts and admin mode
- **Gift** button on every shop item: pick a friend, optional message (filtered). The sender pays the normal price. The gift goes
  P2P to the friend's presence peer and is retried until the recipient **acks** it; offline friends get it the next time both are
  online. Gift ids make delivery idempotent (never lost, never duplicated, even across reloads). The recipient sees a toast and an
  unwrap animation in the gift inbox, then **Accept** puts the item in their inventory.
- **Admin mode** is defined only by public keys in `js/admin_keys.js` (`AG.ADMIN_PUBLIC_KEYS`, empty = nobody is admin).
  The admin device holds an ECDSA P-256 private key (WebCrypto, never leaves the device except in your own backup). Admin powers:
  every shop item free, free gifts, gold/XP grants to friends, a ★ Admin badge. Admin gifts and grants are **signed**, and every
  recipient verifies the signature against `ADMIN_PUBLIC_KEYS`; unsigned/forged "admin" gifts are rejected.
- **Remove XP / gold** (admin): the gift dialog and admin settings have a *Remove* mode for yourself or a friend. It sends a signed
  `xpcut` / `goldcut` correction that applies as soon as it arrives (no unwrap). XP cuts are kept in a per-id ledger (`xpCuts`) and the
  cloud copy keeps *gross* XP (`xpGross:1`), so the server's no-decrease trigger never fires and the cut is never undone by a merge.
  Limits: recipients need this update first (older builds reject the new gift kind), older builds reading a new cloud save show gross
  XP, a gold cut can be undone by a max-gold merge with an older device copy, and cuts to friends apply only when they come online.

### Admin key setup (owner, once)
1. Open the deployed game (https) or `dist/gearbound.html` on **your own** device.
2. Click **⚙** (top right) → scroll down → open **Admin (owner only)** → **🔑 Set up admin key**.
3. The panel shows *"Not active yet…"* and **Public key (safe to share)** with a code starting `GBPK1.`. Press **⧉ Copy public key**.
4. Paste it into `js/admin_keys.js` inside the list, as a quoted string:
   `AG.ADMIN_PUBLIC_KEYS = Object.freeze([ 'GBPK1.…your code…' ]);`
5. `node build.js`, commit, and redeploy `dist/web/` (GitHub Pages).
6. Reload the game on that device: Settings → Admin shows **★ Admin active on this device** and the lobby shows the ★ Admin badge.
7. **Backup (important):** Admin → **Export admin backup** → **Show backup** → **⧉ Copy backup** (`GBADMIN1.…`). Keep it somewhere
   private (password manager). It *is* your private key: anyone with it is admin. Clearing site data without a backup loses the key
   (then repeat from step 2 and replace the public key).
8. Other devices of yours: Settings → Admin → **Import admin backup** → paste → **Import**.

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

### God sets, wings and headpieces (gold premium)
Four new slots, drawn procedurally in `js/cosmetics.js` (cute chibi style) on top of the painted pilot, so they show
everywhere the pilot is drawn: battle, waiting-room cards, the lobby/profile preview, results and the shop.
- **Sets** (Helm + Armour): Zeus, Poseidon, Neo Poseidon, Hades, Athena, Apollo. Wearing the matching helm and armour
  adds a full-set sparkle. Helms replace the hat while worn.
- **Wings** (11): Angel, Devil, Fairy, Bee, Bat, Bird, Butterfly, Dragon, Phoenix, Crystal Pixie, Gear Mecha. They
  flap, buzz or shimmer in real time.
- **Headpieces** (13): Halo, Dark Halo, Devil Horns, Ram Horns, Unicorn Horn, Cat Ears, Bunny Ears, Bug Antennae,
  Flower Crown, Tiara, Floating Crown, orbiting Star Ring, Gear Halo.
- Gold only (no combat stats). Admin gets them free, and they can be gifted at their gold price. They sync online
  through the avatar (unknown ids are dropped) and save locally and to the cloud with the profile.
- **Shop preview:** by default it shows the pilot alone, with no rig. It is a full standing chibi with idle life
  (breathing, sway, weight shift, looking around, blinking and a small hop) plus animated wings and headpieces. It
  updates live. "⚙ Show my rig" switches back to the rig, and the Rig skins tab always shows the rig. The item
  cards for helms, armour, wings and headpieces let you try the item on the standing pilot.

### Halloween Candy Hunt (event, ends at the end of Nov 7 2026)
- **When it runs:** from deploy until **Nov 8 2026 00:00 in the player's local time** (a hard end; `AG.Halloween.END`).
  While it runs, the lobby is decorated and shows a banner reading "Ends in N days" (or hours), with a link to the
  candy shop.
- **Candy per match:**
  - bot match: a win gives 1 candy; losses and bonuses give nothing
  - online: a win gives 3 and a loss gives 1
  - bonuses: +1 for dealing 300 or more damage, +1 per kill (capped at 2), and +2 for the first match of the day
  - a forfeit or retreat gives 0
  - candy shows in the HUD, the lobby, the shop and the results (with a pop-up)
- **Candy shop:** the 🎃 Halloween tab has 31 candy-only items: helms, costumes, wings, headpieces, pets and rig skins
  (3 of the rig skins are animated). Candy items can't be gifted. Bought items stay forever, and after the end the
  tab only lists the ones you own.
- **Leftover candy is a keepsake:** it stays on your profile as a souvenir and is **never converted to gold**. This
  avoids a gold windfall that would unbalance the gold shop, and keeps candy meaningful.
- **Admin tools:** the admin can grant and remove candy (for themselves or friends, through signed gifts). Every
  change goes into a per-player ledger (`candyLog`, the last 60 entries), and admin entries are signed.
- **Limitation:** candy merges like gold (the higher balance of two devices wins), so spending on one device can be
  undone by merging with an older save that has more candy.

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
- **Icons:** 9 shot icons and 14 collector icons.
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

### Round 10/24 fixes
- **Steady menus:** the title and waiting-room backdrop no longer jitter (the demo battle never keeps camera shake, the pan is
  clock-driven, map cards are still). `tools/bgsteadytest.js`.
- **Carom:** shells skip once, low (restitution 0.32, capped hop), and explode on the 2nd impact, for all three shots. `tools/caromtest.js`.
- **Craters open only from the surface** (Lancer's Bore Drill and every other blast): no hidden cavities under the surface.
  `tools/cavitytest.js` audits every rig.
- **Pilots** are chibi riders sitting on top of every rig (torso visible, hands on the controls) and they move: breathing, blinking,
  looking around, small fidgets, and reactions when they fire or get hit.
- **Bye emote** has a new self-generated voice line.
- **Toy World is an open toy chest:** smooth, drivable rolling hills of toys, chest-wood planks inside, the chest rims as platforms,
  the open lid in the background, fair spawns for 8. `tools/toyworldtest.js`. Spawn and respawn checks now accept normal slopes
  (a wall or overhang beside the spot is still refused).
- **Friends "last online":** offline friends show *Last online 5 minutes ago / an hour ago / yesterday / 3 days ago*; online friends
  show *Online now* (or *In match*, *in room ABC123*); no data = *Last online: unknown*. Labels refresh on their own every minute.
  Logged-in pilots send a heartbeat every 60 s and on closing/hiding the tab (`touch_last_seen`), and friends' times come from
  `friends_last_seen(codes)` (`supabase/last_seen.sql`: a private `pilot_presence` table, RLS on, no direct access; the RPC returns
  only friend code, pilot name and last-seen time). Without an account the time the friend was last seen by this device is used.
- **Friend chat:** 💬 on every friend row opens a mini chat (draggable on desktop, docked compact at the top on phones), with
  session history, a big ✕ to close and − to minimise into a bubble with an unread badge (also counted on the Friends button).
  Messages go P2P over the friends presence channel, through the same family-friendly filter and rate limits as room chat on both
  ends. A message to an offline friend waits (shown as *waiting*) and is delivered once when they come back while your tab is open.
  `tools/friendstest.js`.

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
js/perks.js         gold-item combat perks + caps, collector (cosmetic/economy, gold-priced) catalog
js/cloud.js         pilot accounts (name + password) and cloud saves on Supabase; see supabase/ for SQL + the signup Edge Function
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
js/overlays.js      shared ✕ close button + sticky header for overlay panels; Esc closes the top-most panel
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
js/namefilter.js    family-friendly filter;  js/chat.js room/match chat;  js/votekick.js vote kick
js/friends.js, js/friendsui.js  friend codes, presence peers, Friends panel;  js/gifts.js, js/giftsui.js gifting + inbox
js/admin.js, js/admin_keys.js   signed admin mode (ECDSA P-256) and the list of admin public keys
tools/filtertest.js unit test of the filter;  tools/nettest.js online match under latency/jitter/spikes/bandwidth/background
tools/socialtest.js 5 browsers: chat, team chat, vote kick (fail/pass/kicked/host out), fairness, Sudden Death, live ranks
tools/lobbytest.js  sticky name, bot reward, Sudden Death banner, friends presence (desktop + iPhone), filter, Force Start
tools/gifttest.js   gifting + admin signatures + offline queue;  tools/spindleshots.js Spindle rear-cannon close-ups + muzzle check
```
Teams are part of the data model. The Room supports up to 4v4 (Teams A and B), and `TurnSystem` and `Player` already handle more teams (A–D).

### Testing
```
cd tools && npm i playwright-core && cd ..
python3 tools/build_assets.py      # only after changing assets/src
node build.js && node tools/playtest.js && node tools/shotstest.js && node tools/mobiletest.js && node tools/onlinetest.js && node tools/shottest.js && node tools/showcase.js && node tools/closeup.js && node tools/spawntest.js && node tools/shoplooks.js
node tools/boomtest.js && node tools/riggallery.js && node tools/shoticons.js && node tools/rankshots.js   # boomerang hits, 14-rig gallery, 42 shot icons, rank art
node tools/filtertest.js && node tools/spindleshots.js
node tools/accounttest.js && node tools/accountshots.js   # accounts against a mock Supabase; screenshots → screenshots/accounts/
# node tools/livee2e.js  — ONE real run against the live project with a throwaway pilot; delete it afterwards (see tools/livee2e-result.txt)
# online tests (need a TURN relay reachable from headless Chrome; TURN='url,user,password')
TURN=... node tools/onlinetest.js && TURNS=9 TURN=... node tools/nettest.js && TURN=... node tools/socialtest.js && TURN=... node tools/lobbytest.js && TURN=... node tools/gifttest.js
# or everything, sequentially (about 50 min headless); resumable: a test that already passed with the same
# app code + same test script is skipped. Results are appended to tools/last-test-run.txt
TURN_URL='turn:host:3478?transport=tcp,user,pass' tools/runall.sh
```
(These scripts use `/usr/bin/google-chrome` with SwiftShader WebGL, so headless frame rates don't reflect real GPUs. Set `CHROME=/path/to/chrome` to use a different browser for `playtest.js`.)

## Known issues
- Painted art is AI-assisted and then keyed on magenta. The edges are now colour-bled, so the magenta halo is gone at normal zoom. At very high zoom a faint seam can still show, for example a small patched block on the top of Gyro's wheel. Some purple-themed items (Orrery, Volt and a few premium cosmetics) are purple by design.
- Spindle's eruptions are steered but not guaranteed: on a target that is moving or on a slope the shell can come up slightly off-centre, so Deep Eruption often does less than its full 330 damage.
- Spindle has no walking-leg animation. It skitters with a bob and squash instead.
- Online play needs a TURN relay on strict networks (see Online). The automated online test runs on a local TURN server only.
- Pilot accounts: no self-service password reset (no e-mail), the online-needs-an-account rule is client-side only (P2P), and
  merging two devices takes the higher gold of the two (so it can't lose gold, but it can't subtract gold spent on the other
  device either). When two devices merge, the cloud's friend code wins; friends who only knew the other device's code need to re-add.
- Progress is stored on each player's device (and copied to the cloud with an account), so a determined player could edit their own save; signatures prevent impersonating the admin or forging admin gifts.
- Normal (non-admin) gifts are not provably paid for: a player who edits their own save could send gifts without really spending gold.
  Admin "free" purchases are local to the admin device.
- Bots: the 1 XP + 1 gold bot rule is keyed on the `bot` flag. Easy bots hit about 25% of shots (measured 24.85% over 12 easy matches; bottest allows 17-30%).
- Emote 8 is now **Aww, come on!** (pout face; `COMEON!`, `GRR!`, `AWW!` all work) with its own recorded "come on" clip. It still travels as `grr` on the wire, so older builds show their Grr for it.
- **Emote voices (v5):** two recorded sets, `fem` and `masc`, 11 clips each, embedded in `js/emote_audio.js` (built by `node tools/emotevoices5.js [dir]` from `fem_<id>.mp3` / `masc_<id>.mp3`; trimmed, mono 24 kHz 40 kbps, about 178 KB; it replaces the old espeak `tools/emotevoices.sh`). Each pilot face picks a set: fem = Wren, Juno, Ivy, Tam; masc = Kit, Rook, Milo, Pax (unknown face = masc). Online, the sender's set travels with the emote (`{t:'emote', id, v}`, relayed as `e.v`), so everyone hears the sender's voice; an emote from an older build (no `v`) uses the sender's pilot face. Older builds ignore `v` and play their own old clips. Mute and the SFX volume apply. There is no manual voice override yet.
- If the host is voted out (or quits), the match ends for everyone: there is no host migration.
- Presence/friends and room directory rely on the public PeerJS broker (`0.peerjs.com`); if it's down or rate-limits, friends show
  offline and rooms can only be joined by code. Presence updates take a few seconds.
- The online/lag tests use simulated latency, jitter, spikes and bandwidth limits plus a hidden (backgrounded) host on a local TURN
  relay, not real mobile networks. A backgrounded *host* tab is throttled by the browser; the match keeps running (the host
  simulates on a timer when hidden) but can feel slower to the others.
- In about 1 of 4 nettest runs (before the checksum fix) both joiners ended up with identical ground that differed from the host's,
  starting the turn after Joiner1's forced reconnect. The exact lost carve was never pinned down; the per-turn terrain checksum and
  full-ground resync now repair it within a turn (nettest also corrupts a joiner's ground on purpose and checks it heals), but the
  ground can look wrong on a client for up to one shot before it is corrected.
- Headless test Chrome (SwiftShader) stalls frames with several pages open, so the online tests use generous timeouts; that
  says nothing about real devices, but it is why the tests take minutes.
- HUD/loading portraits are the painted bust only; helms, wings and headpieces are not drawn on those small static portraits (they are on every live preview and in battle).
- Touch controls were checked in an emulated iPhone 14 and iPad (landscape and portrait) with real CDP touch events, not on physical devices.

## Roadmap
1. **Gameplay:** bots, fall damage, more handcrafted maps, caves and moving islands.
2. **Online:** host migration, spectators, a small relay/server for strict networks, server-checked XP and gold.
3. **Accounts (own, no Big Tech):** optional username + PIN or transfer-code sync for cross-device progress.
4. **Art:** animated rider poses, per-rig firing/recoil frames, more hand-painted terrain sets.
5. **Tech:** ES modules plus a tiny bundler, unit tests for physics and turns, fixed-point math for cross-browser determinism.
