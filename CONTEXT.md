# Wall of Came — Hello Nancy 🍋

Context file for the **London pop-up projector** deck. Written for anyone (human or AI
assistant) picking the project up cold.

---

## What this is

A single-file, fullscreen **projector loop** for a Hello Nancy pop-up event in London
(Regent's Park). It throws real customer reviews of the "Lem" product onto a wall in nine
rotating visual treatments, forever, with no input required.

It is **not** a website and not a click-through deck. It is signage: you open it, press
`F`, and leave it running for the length of the event.

- **Live:** https://rsquaredboi.github.io/London-popup-projector/ (GitHub Pages, `main` branch)
- **Repo:** https://github.com/rsquaredboi/London-popup-projector

---

## Running it

Open `index.html` in any browser — there is no build step, no dependencies, no server
required. For the real thing:

1. Open on the machine driving the projector
2. Press `F` for fullscreen
3. Leave it; it auto-advances and never ends

The page sets `cursor:none` and `overflow:hidden`, so it looks broken-ish in a small
window. That's expected — it's designed for one edge-to-edge display.

### Keyboard controls

| Key | Action |
|---|---|
| `1`–`9` | Jump straight to concept 1–9 |
| `T` | Back to the Truck opener (concept 1) |
| `←` / `→` | Previous / next quote within the current concept |
| `Space` | Pause / resume auto-advance |
| `F` | Toggle fullscreen |

An on-screen HUD (top-left concept name, bottom hint line) fades out after 2.5s of no
mouse or key activity and returns on movement.

---

## Files

| File | Role |
|---|---|
| `index.html` | **Everything.** Markup, CSS, and JS in one ~20KB file. |
| `nancy-cream.png` | "Hello Nancy" wordmark — persistent top-center logo, and the big centered mark on the truck slide |
| `nancy-logo.png` | Alternate logo. **Currently unreferenced by `index.html`.** |
| `truck-cut.png` | Cut-out Nancy van, drives in on the Truck slide |
| `pin-robe.png`, `pin-daisy.png`, `pin-crown.png` | Nancy × Nick enamel pins — rotate in the bottom-right, one per concept |

Two fonts load from Google Fonts: **Fraunces** (display serif, the quotes) and **Anton**
(condensed sans, all the uppercase chrome). Offline, it falls back to Georgia / Impact and
looks noticeably worse — worth checking the venue has wifi, or self-hosting the fonts
before the event.

---

## How it works

### The loop

`show(i)` is the single entry point for rendering. It:

1. Runs the previous concept's `cleanup()` (every concept returns a teardown function that
   clears its own intervals/timeouts — **this is the contract**; leak one and you get
   overlapping renders)
2. Sets `#stage` and `#bg` to the concept's class (`c0`–`c10`), which is what actually
   drives the look — the CSS does the visual work, the JS just supplies markup
3. Swaps the bottom-right pin (`PINS[cur % 3]`) and re-triggers its pop-in animation
4. Updates the HUD label, then calls the concept's `f()` to render

`startAuto()` advances to the next concept every **14 seconds** unless paused. Within a
concept, quotes cycle on their own faster timer (2.3s–4s depending on the treatment).

The page boots with `show(0); startAuto();` at the very bottom of the script, and picks a
**random starting quote** — so no two runs open identically.

### Concepts

Defined in the `C` array. Order in the array = the `1`–`9` key order. The `c:` value is the
CSS class number, which is **not** the same as the array index:

| # | Name | CSS | Treatment |
|---|---|---|---|
| 1 | Truck | `c10` | Van drives in, taglines swap above it (own tagline list, not the quotes) |
| 2 | Slam | `c0` | Quote scales in hard |
| 3 | Split-flap | `c2` | Words flip down like a departure board, staggered 0.07s |
| 4 | Lemon rain | `c3` | 26 falling 🍋/🫐/🥑 behind the quote |
| 5 | Karaoke | `c4` | Words light up one at a time, 240ms apart |
| 6 | Neon | `c6` | Pink neon glow with a flicker cycle |
| 7 | Ransom | `c7` | Each word randomly sized, rotated, and colour-blocked |
| 8 | Credits | `c8` | All 39 quotes scroll bottom-to-top over 32s |
| 9 | Glitch | `c9` | RGB channel-split with clip-path jitter |

The Truck slide is the designated opener and is special-cased: it hides the persistent
top-center logo and the rotating pin, because it carries its own centered wordmark.

### Content

- **`QUOTES`** — 39 real customer reviews, each `{q, by}`. This is the actual content of the
  piece and the thing most likely to need editing. Attribution is first name + last initial.
- **Truck taglines** — 10 event slogans, defined inline inside the Truck concept
- **Footer ticker** — a separate 10-tagline list, joined with `★`, duplicated and scrolled
  for a seamless 30s loop

All three lists are independent. Adding a tagline to one does not add it anywhere else.

### Type sizing

`fitLen(el, text)` picks a font size from the quote's **character count** via a hardcoded
ladder (`<13ch → 10.5vw`, `<21 → 8.5vw`, … `else 4.6vw`). It's crude and it works. If you
add a quote much longer than the current maximum it will overflow — check it on the real
display rather than trusting the ladder.

### Brand tokens

Defined as CSS custom properties on `:root`:

```
--pink    #ED1E9C    --plum     #2A0A1E    --lemon     #FCD94A
--pink2   #C0117A    --cream    #FBEFD8    --lemon-dk  #E8B400
--pink-lt #F4D9E6                          --leaf      #5FB85A
```

Each concept picks a background from these via `.bg0`–`.bg10`. The lemon mascot in the
top-right is inline SVG (the `MASCOT` template string), injected into `.mascot` on load.

---

## Things that will confuse you

- **Dead CSS.** `#intro` (a full splash-screen block, ~8 rules) has **no matching element in
  the body** — the splash was removed but its styles stayed. Same for `.c1` (scrolling
  marquee rows) and `.c5` (sticky notes): both are fully styled concepts that were pruned
  from the `C` array. `.cLem` is explicitly `display:none` with a comment, and `.deco` is
  unused. None of it renders; all of it is safe to delete.
- **`nancy-logo.png` is never referenced.** The logo in use is `nancy-cream.png`.
- **Concept number ≠ CSS class number.** Concept 1 is `c10`, concept 2 is `c0`. The gaps are
  where `c1` and `c5` used to live.
- **Every concept must return a cleanup function.** Even the ones with nothing to clean
  (`Credits` returns `()=>{}`).
- **`←`/`→` re-render the current concept** rather than stepping quotes in place, so the
  entry animation replays on each press. Intentional.

---

## Deployment

GitHub Pages serves the **`main` branch, root directory**. Push to `main` and the live URL
updates within a minute or two. There is no build, no CI, and no staging environment —
`main` is production.

Work on a branch if you want to keep `main` clean, but note that **branches have no
preview URL**; you'll need to open `index.html` locally to see changes before merging.
