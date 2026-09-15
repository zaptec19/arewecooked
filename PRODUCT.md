# Are We Cooked? — product context

## Register

**Brand.** This is a game, not a tool. The interface *is* the artifact: a player's
whole impression of the game is the screen they're holding. Product-register
discipline (contrast, hit targets, reduced-motion, consistent affordances) still
applies, but "the UI should disappear" does not — it's the thing being designed.

## Users & purpose

Three to five people around one table, passing a single phone or tablet, usually
loud, usually mid-conversation, often standing. One person reads the ticket out
loud; everyone else is watching the device from an angle or waiting their turn.
Sessions run 20–25 minutes.

The job: keep a fast round loop legible to a group — read the ticket, call a
station, see who matched — with enough shared spectacle (the reveal, the heat
track) that people watch the screen instead of their own phones.

Constraints that fall out of that: text readable across a table, tap targets that
work one-handed while holding the device out, no state that's lost if the tab
closes mid-service, and a privacy phase that genuinely hides other players' picks.

## Personality

Three words: **greasy, loud, printed.** A service kitchen at full tilt — thermal
tickets on a rail, chipped paint, a bell on the pass, heat lamps. Warm and
handmade, never polished-corporate.

## Anti-references

- A quiz app. Nothing about this should read as multiple-choice assessment UI.
- Corporate training software / anything clinical. The game has a hidden
  competency layer (see README) and the design's job is to make that invisible;
  clinical vocabulary or a "results dashboard" feel would give it away instantly.
- Cute mobile-game chrome: cartoon gradients, coin-shower particle spam, mascots.
- Flat neutral SaaS cards. The physical version is a printed deck; the digital
  one should feel printed too.

## Accessibility

- **Shape, not color alone.** Five stations are color-coded, and colorblind
  players are the normal case at a table of five. Every station carries a distinct
  silhouette that identifies it without color.
- Body copy and all game text at 4.5:1 or better on its surface; ticket text is
  large by default because it's read at arm's length.
- Every animation has a `prefers-reduced-motion` alternative — this game flashes
  and shakes on heat, which needs an opt-out.
- Sound is additive only; nothing is communicated by audio alone, and it's
  mutable from the always-visible HUD.

## Strategic design principles

1. **The ticket is the hero.** Every round funnels to one printed docket. It gets
   the texture, the tilt, the typography contrast.
2. **The heat is a threat, not a stat.** It's on screen at all times and it reacts.
3. **The room escalates.** Prep → Dinner Rush → Closing should be felt in the
   ambient light before it's read in the label.
4. **Earning is a moment.** Marks land, badges catch the light, once.
