# Design system — Are We Cooked?

One file, one system. Everything below lives in the `<style>` block of
`index.html`; there is no build step and no external asset.

## Theme

A service kitchen at night, seen from the pass: **tile, steel and shadow, lit by
one lamp**. The room is cool and dark. Everything warm in the frame is warm for a
reason — the printed ticket, the heat gauge, the committing action, a Mark being
earned. Warmth is a signal, not a mood, so it is rationed.

Not a light theme: the table is indoors under warm light, the device is passed
hand to hand, and a cold dark ground makes the cream ticket the only bright
object — the same way a docket reads under a heat lamp.

## Color

A neutral ramp carries every surface; one accent hue per shift carries the light.
Nothing in the room is "tinted brown" — the shift reads as *lighting on neutral
material*, not as a filter over the whole screen.

**Neutrals** — one hue (~218°), low chroma, six steps:
`--n-900 #0a0d12` · `--n-800 #121821` · `--n-700 #1a212b` · `--n-600 #232c38` ·
`--n-500 #2f3a48` · `--n-400 #3c4858`. Aliased by role: `--room-deep`,
`--surf-1`, `--surf-2`, `--surf-3` (hover), `--line`. A scored row is
`color-mix(in srgb, var(--gold) 11%, var(--n-700))` — the ramp with gold washed
through it, never a separate brown fill.

**Text pairs** — one for dark surfaces (`--text #eaeff5` at 14:1, `--muted
#9dabbb` at 6.9:1 on `--n-700`), one for light (`--steel-ink #171d25` at 14.8:1,
`--steel-sub #4f5a68` at 6.1:1 on `--steel-paper`).

**Stations** — restated as one family rather than five isolated picks: even
lightness, even hue spacing, and all five pitched brighter than any ambient
light so they never collide with the room.

| Station | Hex | vs Prep | vs Dinner | vs Closing |
|---|---|---|---|---|
| Grill | `#ff6b57` | 6.4 | 6.6 | 7.1 |
| Prep | `#59a7f0` | 7.0 | 7.2 | 7.8 |
| Sauté | `#f7b23f` | 9.7 | 10.0 | 10.8 |
| Garde Manger | `#4fc98c` | 8.6 | 8.9 | 9.6 |
| Expo | `#a98cff` | 6.7 | 6.9 | 7.5 |

Grill sits at 7.1:1 against the red Closing room specifically — the clash case.
Badge labels are `--badge-ink #11151b` on the station colour (6.5–9.9:1), not
white, which was failing on the lighter hues.

**Fire (reserved)** — `--heat`, `--heat-deep`, `--ember`, `--gold`. Kitchen Heat,
the committing button, Marks, badges, the burn banner. Nothing decorative.

**The papers** — `--paper #f6f1e6`, an off-white rather than cream, carrying a
per-shift `--paper-tint` at 5–7% so the docket sits *in* the room's light instead
of on top of it, plus a stronger drop shadow to separate it. `--steel-paper
#ecf0f5` for picker rows keeps the three layers legible: room / ticket / options.

### Shift themes

| Token | Prep Shift | Dinner Rush | Closing |
|---|---|---|---|
| `--room-base` | `#121820` | `#17130f` | `#0c0809` |
| `--lamp-1` | cool white 13% | amber 26% | red 30% |
| `--vig` / `--vig-r` | `.44` / `128%` | `.64` / `100%` | `.88` / `70%` |
| `--shift-accent` | `#8fb3d0` | `#f0a65c` | `#f07a62` |
| `--paper-tint` | cool 5% | amber 7% | red 6% |

Registered with `@property` and transitioned over 400ms. The lamp alpha is
deliberately low — the accent tints the room, it never fills it.

### Background

Four composed layers, no repeating pattern doing heavy lifting: a lamp over the
pass, a low floor bounce, running-bond tile grout at **2.8%** white on a 168×84
tile (down from 5.5% on 132×66 — texture, not wallpaper), and the shift's base
fill. Over it, brushed-steel fibre at 3.5% and fine grain at 3.5% so the
gradients never band, then the vignette.

## Space, radius, elevation

- **Spacing**: 4px base — `--s1 4` … `--s8 32`. Every padding, margin and gap in
  the app resolves to one of these; there are no raw pixel values left.
- **Radii**: three steps and a pill — `--r-sm 10`, `--r-md 14`, `--r-lg 18`,
  `--r-pill`. (Was six ad-hoc values between 9 and 18.)
- **Borders**: 1px for structure, 2px for selected/active. (Was 1/3/4/5px.)
- **Elevation**: `--sh-1/2/3` soft steps for surfaces, `--press` / `--press-in`
  for the hard tactile button shadow. The docket's drop-shadow sits above all of
  them — it is the one thing allowed to shout.

## Typography

Two families on a contrast axis — rounded geometric sans against monospace.

- **Voice** (`--font-…` default sans): title, scenario lines, chef names, buttons.
  Weights 700–900, `clamp()` on the ticket line only, `text-wrap: balance`.
- **System layer** (`--font-docket`, `ui-monospace` stack): shift label, ticket
  number, the `PREP SHIFT · TICKET 1/20` meta line, section labels, HEAT label.
  Uppercase, `.12–.16em` tracking. This is the printed-docket register; it should
  never be used for anything a player reads as sentence copy.

System fonts only — the game must open offline from a file, so no webfont.

## Components

- **Docket** (`.docket` > `.ticket`) — torn top and bottom edge via a 19-tooth
  `clip-path` polygon with *irregular* tooth depth (10–16px, seeded) so it reads
  torn rather than pinked. Two texture layers: fine grain (SVG turbulence, 34%
  multiply) over coarse paper fibre (16%, contrast-boosted), plus 4px print lines
  and a slight paper gradient. The wrapper carries the rotation (±2.6–3°,
  alternating each ticket) and the drop shadow, because `clip-path` clips
  `box-shadow`. `.slim` is the compact variant that heads the call and reveal
  screens.
- **Station silhouettes** — 24×24 inline SVG: flame, chef's knife, pan, leaf,
  bell, each with a light inner facet (`.hl`). One set, used in the picker, the
  reveal list, the answer card, the mark tally chips, the badges and the replay
  button.
- **Heat gauge** — five 21×26 flames seated in a scorched housing (inset shadow,
  ember-lit rail) whose `--fill` bar rises behind them with the heat. Outline when
  unlit, solid + glow when lit. Unlit flames breathe (`ember`, 4.6s), lit ones
  flicker (`flicker`, 2.3s, desynced -0.8s per index); at 3+ the HEAT label
  pulses. A new one runs `ignite` (0.4 → 2.1 → 1 with a brightness burst) while
  the room flashes red at the edges (1.05s, double-peaked) and the whole pass
  shakes and rotates (0.62s).
- **Mark token** — a 30px disc in the station color carrying its silhouette,
  dropped 46px on `markDrop` with a double bounce and an expanding impact ring,
  staggered 95ms per player.
- **Badge** — station-colored pill, foil gradient, inner white rim and a lift
  shadow. `badgeIn` lands it, one shine sweep crosses it, then two sparkles pop
  as the sweep clears. Staggered 140ms. Runs once, on first render only.
- **Buttons** — one shape everywhere: 16px radius, 6px hard bottom shadow,
  3–4px press travel. Primary is `--heat`, earned/terminal actions are `--gold`.

## Layout

Single 600px column, 16px gutters, bottom-anchored primary action so the thumb
lands in the same place on every screen. The HUD is a fixed three-part row:
shift + ticket count, sound toggle, heat track. Nothing is hidden behind a menu.

## Motion

Exponential ease-out (`cubic-bezier(.16,1,.3,1)`) for anything that lands; 150–250ms
for state, 500–800ms for the felt moments (ignite, mark drop, badge shine). The
heat track is the only thing that loops, deliberately — it's the running threat.

`prefers-reduced-motion: reduce` collapses every duration to ~0 and keeps the end
states: tokens and badges are visible at full opacity, the shine is removed, the
screen flash and shake are disabled.

## Sound

Four WebAudio one-shots, synthesized at runtime, no files: printer chatter on a
ticket pull, a bell when the calls are in, a sizzle when a ticket dies, a clink
per Mark. Additive only — nothing is communicated by audio alone. Mutable from
the HUD; the context is created on the first tap.
