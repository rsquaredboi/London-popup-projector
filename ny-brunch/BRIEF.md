# NY Brunch — tennis deck brief

Working notes for the tennis re-skin. Source assets pulled from the shared Drive folder
(Colorful Ones / Postcard / Edits), 2026-08-24.

## Campaign

**Line:** "Smash the Quiet. Ace the Point."
**Product line:** "Second serves are for tennis. With Nancy, one is enough."
**Mascot line:** "Love – Love / Not On Our Watch"

**Postcard script** (the long copy — reads as the deck's spine):

> At this club, love means everything,
> and every point is yours to play.
> Start slow. Warm up. Take the long rally.
> Find the sweet spot nobody else can reach.
> Play singles or mixed doubles.
> Hold serve, or break it.
> No faults here. No lines you can't call.
> And when it's match point, take it.
> Advantage, you.
>
> Game. Set. Yours.

## Palette (sampled from the vector postcard — exact, not eyeballed)

| Token | Hex | Use |
|---|---|---|
| Nancy pink | `#EF538E` | Postcard ground, court surface |
| Marigold | `#FBC829` | Display type, wordmark |
| Chartreuse | `#9CCB3B` | Ball body |
| Chartreuse light | `#B5D564` | Ball highlight |
| Sage | `#8C9B84` | Product body |
| Sun yellow | `#FED904` | Mascot-world wordmark |
| White | `#FFFFFF` | Court lines |

## Typography

Campaign sets in **PP Editorial New** (Pangram Pangram) — Ultralight for display,
Regular for body, Ultrabold for the small caps/URLs. Embedded as a subset in the postcard
PDF; **not** a free/Google font, so it needs a licensed webfont to ship on a public page.

## Three visual worlds in the supplied assets

1. **Campaign hero** — photoreal court, textured ball, multicolour ribbon swoosh
   (`hero-chartreuse.jpg`, `hero-pink.jpg`)
2. **Product on court** — wand with ball head, ribbon trail (`product-court.jpg`)
3. **Mascot / kawaii** — painted ball characters, palm-lined court, sticker outlines
   (`mascot-duo.jpg`, `mascot-trio.jpg`, `mascot-hero.jpg`)

## Known constraints

- **Every supplied asset is portrait** (4:5 — 1632×2048 and 1080×1350). The projector is
  16:9 landscape. Full-bleed use means cropping away the composition, so these are staged
  as framed elements with extended grounds rather than backgrounds.
- Assets here are optimised (JPEG q88, max 2048px) for projector playback. Originals stay
  in Drive.
