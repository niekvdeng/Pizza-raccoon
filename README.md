# Pizza Raccoon

A raccoon steals ten real Mexican artefacts back from a firm of smugglers in
uniform, using a pizza cutter. Top-down, browser-based, no install.

**Play:** open `index.html` in any browser.
**Published build:** <https://claude.ai/code/artifact/3c0bf033-fd86-48e8-902d-d1823d2935de> (private)

---

## The loop

Patrimonio S.A. have crated up ten artefacts and posted guards over every one.
Lift one with `E` and it is *in your paws, not banked* — it only counts once you
have carried it through the green door of the **casa segura** on the plaza.

You can carry **three at a time**. Carrying raises your **heat** floor, which
widens guard vision cones, speeds them up, and past heat 3 spawns hunters who
walk in from off-screen. Banking drops heat hard. That tension — one safe trip
versus three risky ones — is the game.

Bank all ten and **El Capitán** arrives in the plaza. Nine hits and you win.

Getting caught is a shakedown, not a death: unbanked artefacts go back where
they were, you lose half your strength, and you wake on the plaza. There is no
fail state.

## Controls

| Key | Action |
|---|---|
| `WASD` / arrows | move |
| `Space` | swing the pizza cutter |
| `E` | take artefact · deliver at the casa segura · share a slice with the dog |
| `C` | codex |
| `M` | mute all audio |

## Systems worth knowing about

**World** — 96×72 tiles, generated from a fixed seed (`buildWorld`). Five biomes
assigned by warped nearest-anchor: desert, agave fields, village plaza, jungle
ruins, lagoon. Structures (pyramid, houses, market, cemetery, safe house) are
stamped afterwards. A flood fill from the spawn marks reachable tiles; anything
unreachable gets relocated rather than stranding an artefact.

**Guards** — three ranks sharing one parameterised humanoid sprite. Each paints
its vision cone on the ground: turquoise patrolling, red alerted. States run
patrol → alert (`!`) → chase → search (`?`). Trees, bushes and agave cut their
sight range by nearly half and bleed heat faster, so cover is real.

**The dog** — a stray near the market. Share a slice (`E`, costs 22 hunger) and
it follows you for ~80 seconds, breaking off on its own to bark at any guard
within nine tiles. A barked-at guard drops the chase for 4.4 seconds. One slice
buys one safe passage.

**Hunger** — drains as you move. At zero you move at half speed *and* your swing
does half damage, so pizza feeds combat indirectly.

**Art** — every sprite is canvas paths drawn in a 16-unit space and rasterised
at 3×. No image files exist. Characters are built from one function per species,
parameterised by direction, walk frame and palette — which is why the sergeant
and the Capitán cost nothing extra over the basic agent.

**Music** — a mariachi band synthesised live in WebAudio; there is no audio
file. Guitarrón (triangle doubled an octave up), vihuela (four detuned sawtooths
strummed 11ms apart), and two trumpets in thirds with a pitch scoop into each
note. Bars alternate 6/8 and 3/4 accents — the *sesquiáltera* hemiola that makes
it read as Mexican rather than generically Latin. The score follows the game:
D major when calm, D minor and faster once heat passes 2, a separate progression
for the boss. Mode changes land on a bar line.

## Tuning

The numbers most likely to need adjusting, all in `index.html`:

| What | Where |
|---|---|
| Carry limit | `CARRY_MAX` |
| Guard HP, speed, vision | the `AG` table |
| Heat gain / decay | the `/* heat */` block in `update()` |
| Hunger drain, pizza value | `update()` — `state.hunger` lines |
| Dog loyalty duration, bark range, cooldown | `feedDog` / `updateDog` |
| Knockout duration | `a.down=` in `swipe()` |
| Music tempo and density | `Music.P` |

## Ideas not built yet

- Steal a guard's cap and wear it as a disguise; breaks if you carry an artefact
  in the open or get too close.
- Smashable crates that drop pizza.
- Day/night cycle — thinner patrols after dark, shorter sight lines both ways.
- Best-run record in `localStorage`, with a badge for zero shakedowns.
