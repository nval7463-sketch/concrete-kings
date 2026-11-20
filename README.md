# Concrete Kings: first-person boxing

A first-person boxing game in the browser. You can fight real people online 1v1 or practice against bots in the Training Arena.

## Controls

| Key | Move |
| --- | --- |
| W A S D | Move around the ring. You always face your opponent. |
| Left Shift | Boost step: a quick burst in the direction you're moving (forward if none). Uses the yellow boost meter. Punch right after a forward boost for a LUNGE (+25% damage). |
| J (or left click) | Jab: fast, longest reach, cheap on stamina |
| K (or right click) | Hook: big swing at mid range, alternates hands |
| L | Uppercut: short range and slow, but breaks a raised guard |
| Space (hold) | Block: stops jabs and hooks |
| Q / E | Slip left / slip right: dodges any punch and sets up a 1.6x COUNTER |
| Esc | Menu / pause |

Block beats jab and hook, uppercut beats block, and slip beats everything.

## Online 1v1

Play Online gives two options:

- **Host a 1v1:** you get a 5-character code (like YQ4EM). Send it to your friend. The fight starts as soon as they join.
- **Join with Code:** type the code your friend sent you.

Your fighter name is set from the main menu (Fighter Name).

## Training Arena bots

| Bot | Difficulty |
| --- | --- |
| Sandbag Sammy | Practice dummy |
| Novice, Rookie | Easy |
| Brawler, Slugger, Southpaw | Medium |
| Contender, Counterpuncher | Hard |
| Speedster, Champion | Very Hard |
| Legend, Phantom | Extreme |

Each bot has its own style (wild brawler, patient counterpuncher, slippery southpaw...) and many wear fist effects. Harder bots pay bigger purses.

## Tournaments

- **Offline cups:** Amateur Cup, Pro Circuit or World Title. You plus 7 random bots in an 8-fighter bracket (quarterfinal, semifinal, final). Other matches are decided by bot strength. Lose once and you're out; win the final for the cup prize ($3,000 / $9,000 / $25,000).
- **Host Online Tournament:** you get a code, friends join a lobby (up to 8 fighters). Toggle **Bots** on to fill the empty spots, or off for players only (anyone without an opponent gets a free pass). Pick the bot level, then Start. Player-vs-player matches are real online 1v1s; player-vs-bot matches run on that player's machine. Matches start on their own a few seconds after the bracket shows. The champion gets $5,000.
- **Join Online Tournament:** type the code.

## Stamina

Every punch and slip costs stamina (the blue bar). It only refills after a short breather. Landing a clean hit gives some back (more for heavy shots, counters and long combos), while misses cost extra. Run out and punches come out weak and slow, and the bar blinks with a TIRED warning.

## After the fight

The winner gets a victory screen with 6 dances to pick from (keys 1 to 6): Victory Bounce, Shuffle, Flex, Robot, Shadowbox and Champion Spin. The loser sits on the canvas crying. Online, each player sees the screen for their own result, and the winner's dance shows on the loser's screen too.

## Punch FX Shop

Fight purses go into a wallet (saved in the player's browser). A knockout win pays about $1,200 against the Rookie, about $3,800 against the Champion, 1.5x online, and a loss pays 30% of your points. Fist effects are purely visual and get flashier by tier:

| Effect | Price | Tier |
| --- | --- | --- |
| Shadow Fist | $3,000 | 1 |
| Toxic Knuckles | $5,000 | 1 |
| Inferno Fist | $8,000 | 2 |
| Frostbite | $8,000 | 2 |
| Thunder Hands | $14,000 | 3 |
| Neon Rush | $20,000 | 3 |
| 24K Champion | $32,000 | 4 |
| Cosmic Fist | $50,000 | 5 |

Each effect has its own aura, trail and hit starburst. Online, your opponent sees your effect.

## Music

Three original songs (Rooftop Feelings, Velvet Rope, Easy Street) are synthesized in the browser and rotate at low volume. Change the level under Settings > Music. There are no audio files.

## Performance

Each boxer part is one draw call with baked colors, and the arena is baked into a few meshes, so a fight takes about 41 to 47 draw calls. The crowd is a single instanced draw. There are no shadow maps. Particles only update while alive, the animation code allocates nothing per frame, and all shaders are compiled during menus and the fight countdown. Resolution scales itself down when frames get slow, and there are Low, Medium and High graphics presets. Turn on Settings > Show FPS to check a machine.

## Run it on your computer

```bash
npm install
```

```bash
python3 build.py
```

```bash
npm start
```

Open http://localhost:3000. To test online play alone, open it in two browser windows: host in one, join with the code in the other.

## Put it online (Render, no terminal)

1. Make a GitHub repository and upload everything in this folder except `node_modules`. Upload the `public` folder as a folder, since it holds `index.html` and `media/attract.mp4`.
2. On render.com: New + > Web Service > pick the repository.
3. Build command `npm install`, start command `npm start`.
4. Share the address Render gives you. Both players must use the same address.

Keep exactly one server running, because match rooms live in its memory.

## Project layout

| Path | What it is |
| --- | --- |
| `blender/build_models.py` | Builds the gloves, arms, head, torso, shorts, legs, ring posts and stools in Blender and exports them to `src/models.js`. `blender/boxers.blend` is the saved scene. |
| `trailer/index.html` | The HyperFrames composition for the menu's background video. Render it with `npx hyperframes render -o ../public/media/attract.mp4` from inside `trailer/`. |
| `src/sim.js` | Fight rules (timings, damage, block/slip/counter). The same code runs bots and the online host. |
| `src/bot.js` | Training Arena opponents: Sandbag Sammy, Rookie, Contender and Champion. |
| `src/render.js` | Three.js arena, IK-posed boxers with knees, first-person arms, glove effects, particles, victory dances and the crying pose. |
| `src/audio.js` | Every sound is synthesized live, so there are no audio files. |
| `src/game.js` | Menus, shop, HUD, camera, KO sequence and replay, win/lose scene, host/join codes and netcode. |
| `server.js` | Static server plus the WebSocket room relay. |
| `build.py` | Joins `src/` into one `public/index.html`. `--debug` adds a test hook. |

Rebuild the models (needs Blender, or `pip install bpy` on Python 3.13):

```bash
python3 blender/build_models.py
```
