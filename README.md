# Are We Cooked? — playable prototype

A pass-and-play kitchen card game for 3–5 players on one phone or tablet.
20 tickets, 3 shifts, one shared Kitchen Heat meter. Single file, no build step,
no backend, no login, nothing saved.

## How to run

Double-click `index.html`, or open it in any browser:

```bash
open index.html
```

That's it — it's one self-contained HTML file with inline CSS and JS. No install,
no server, no network calls. Closing the tab wipes the game; nothing is written to
disk or to `localStorage`.

If you'd rather serve it (e.g. to open it on a phone on the same Wi-Fi):

```bash
python3 -m http.server 8765
```

## How it plays

0. **How to play** — a four-card tutorial opens automatically the first time the
   app is opened in a browser session (what the game is, the five stations, the
   round loop, how you win and lose). "I know how to play" skips it; "Let's cook"
   finishes it. Both land on setup. The **?** button at the top-right of the
   setup screen reopens it any time — for a refresher or a late arrival.
1. **Start** — pick 3, 4, or 5 players and name them (defaults are Chef 1…5).
2. **Ticket Up** — the Head Chef taps the face-down card to pull the next Ticket
   and reads the one line out loud. Head Chef rotates every round.
3. **Call It** — the device goes around the table. Each player, Head Chef included,
   gets a "pass to \_\_\_" screen, then picks 1 of the same 5 Stations and locks it in.
   Previous picks are never shown during this phase.
4. **Flip** — everyone's call is revealed at once, then the Ticket flips to show the
   Station printed on it.
5. **Mark it** — everyone who matched takes 1 Mark in that Station's color.
   If *nobody* matched, the shared Kitchen Heat meter ticks up by 1.
6. **End** — Kitchen Heat hitting 5 ends the game immediately: the kitchen goes under,
   no winner, everyone loses together. Otherwise play runs all 20 Tickets through
   Prep Shift (7) → Dinner Rush (7) → Closing (6).
7. **Scores** — most Marks is Chef of the Night (ties share it). Each player's Marks
   are broken out by Station color, and 3+ of one color earns that Station's Badge.

Kitchen Heat and the shift/ticket counter sit in a header that's visible on every
play screen — the shared pressure is part of the tension, so it's never hidden.

### No judge, ever

Every Ticket carries its correct Station in the card data (`TICKETS[].station`).
Scoring is a string comparison against that value — `S.picks[i] === ticket.station`.
There is no judge role, no text input, no performance, and no subjective call
anywhere in the game. A shy table and a loud table score identically.

The deck is shuffled *within* each shift, so the three shifts always play in order
but the tickets inside them vary between sessions.

## The five Stations

| Station | What it does on the line |
|---|---|
| 🔥 Grill | Hot, high-pressure cooking that can't be rushed. |
| 🔪 Prep | Everything made ready before the ticket hits the line. |
| 🍳 Sauté | Fast pan work — has to keep moving, no matter what. |
| 🥗 Garde Manger | The cold station — salads, starters, anything delicate. |
| 🔔 Expo | Calls out every ticket, keeps the kitchen in sync. |

These one-liners are the `job` field on `STATIONS` in `index.html` — the single
source that feeds the picker, the reveal, the tutorial legend and the tally. They
describe the station's actual role so a player with no restaurant background can
reason from the ticket to the station instead of guessing at tone.

Every player holds all five, every round. Colors: grill = warm red, prep = soft blue,
sauté = orange, garde manger = green, expo = purple. Badges at 3+ Marks in a color:
Iron Grill, Mise en Place, Full Send, Gentle Hands, On the Pass.

The deck is balanced at exactly 4 Tickets per Station, so a perfect game is
20 Marks and all five Badges.

## The hidden layer (reviewer note — not visible to players)

Under the kitchen skin, each Station is a one-to-one stand-in for one of the five
EQ pillars:

| Station | Pillar | In-game framing |
|---|---|---|
| 🔥 Grill | self-regulation | staying steady under heat |
| 🔪 Prep | self-awareness | check yourself before you touch the line |
| 🍳 Sauté | motivation / drive | push through it |
| 🥗 Garde Manger | empathy | handle the person gently |
| 🔔 Expo | social skill | say the thing, clearly, out loud |

That mapping lives **only** in a comment block in `index.html` (search for
`DESIGN NOTE`) and in this README. No player-facing string in the app uses the words
EQ, emotional intelligence, empathy, self-awareness, self-regulation, or any
clinical vocabulary — `grep -i` over the file confirms those words appear only
inside that comment. The game tracks Marks per Station and nothing else: no score
out of 100, no trait readout, no profile. A player experiences a fast matching
game about reading a chaotic shift; the practice is the point, the label isn't.

## Look and feel

The visual system (docket texture, station silhouettes, heat track, per-shift
lighting, motion, sound) is documented in [DESIGN.md](DESIGN.md), and the
strategic context — users, personality, anti-references, accessibility stance —
in [PRODUCT.md](PRODUCT.md).

Sound is on by default and mutes from the speaker button in the HUD. Everything
animated has a `prefers-reduced-motion` path that keeps the end state and drops
the movement.

## Dev hooks (QA only)

A normal load exposes **no globals** — the whole game runs inside an IIFE, so there is
no `window.endGame` for a player to stumble into. QA opts in with a flag on the URL:

```
index.html?dev=1        (or index.html#dev)
```

That attaches one namespaced object, `window.__cooked`:

| Call | What it does |
|---|---|
| `__cooked.endGame()` | jump straight to the end-of-service screen |
| `__cooked.endBust()` | jump straight to the kitchen-went-under screen |
| `__cooked.state` | live state: players, marks, heat, deck, picks |
| `__cooked.give(playerIndex, stationKey, n)` | hand out Marks to set up a scenario |

Drop the query param and the hooks are gone.

## Files

- `index.html` — the whole game (data, logic, styling)
- `README.md` — this file
