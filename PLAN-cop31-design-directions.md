# COP31 public page: three design directions

Prepared 7 November 2026. Companion to `mock-sheet.png` (three phone panels above, three tablet panels below).

## Why the current page reads as a wireframe

It is honest about structure and silent about material. Eight specific causes:

1. **Flat fills only.** Paper and ink, no light. Every surface sits on the same plane, so nothing tells the eye what is above what.
2. **Hairlines carry all separation.** A 16 percent rule between rows is a drawing convention, not a finish. Real interfaces separate with value, not lines.
3. **No elevation scale.** There is exactly one depth: zero. Nothing lifts, so the current session cannot outrank the day header, which cannot outrank a row.
4. **Mono type at 8.5 to 10px everywhere.** Mono for every label, index, time and button makes the page read as a printed specification or a dev tool.
5. **No colour temperature movement.** Paper is one value top to bottom. Real pages shift from a lit top to a settled bottom.
6. **No interaction finish.** No press state, no hover lift, no transition on the controls. Taps feel dead.
7. **Square geometry everywhere.** Zero radius is a defensible Swiss choice, but combined with hairlines it reads as unstyled HTML.
8. **No image, texture or material cue.** Nothing in the page admits it is a screen.

The research agrees that depth has to do a job rather than decorate: daisyUI's 2026 field guide opens with "interfaces feel physical again, depth has a job now... translucency and spatial layers work best when they clarify hierarchy or interaction instead of decorating every surface". setproduct's 2026 guide puts the same point operationally: "build the solid version first, and let translucency be the reward, not the foundation". Muzli notes the trap for dark: "shadows don't work on dark backgrounds" - depth there has to come from border light and value contrast.

## Direction A: warm paper, soft depth (recommended)

Same editorial bones he already approved, finished properly.

| Token | Value | Job |
|---|---|---|
| paper | `#f2ede3` with two radial washes (`#fdfaf3`, `#efe7d7`) | lit surface, not a flat fill |
| ink | `#16150f` | text |
| accent | `#0f5138` | current session, filled |
| gold | `#9a6412` | next session, never the accent |
| radius | 14px rows, 16px cells, 999px pills | |
| elevation | `0 1px 1px + 0 2px 6px` (rest), `0 2px 4px + 0 10px 24px` (card), `0 18px 40px` (lift) | three steps, one light source |
| type | serif display for name and day numerals, system sans 14.5/1.34 body, mono only for times | |

What changes: rows become softly lit cards on a washed paper; the day numeral sits in a lit strip; the current session is a filled green card with an inner top highlight; the next session is a warm cream card with a gold rule; buttons are pills with a 1px light top edge and a real press state; the sticky bar gets the same treatment as it lifts off the page.

Strength: keeps the editorial identity he has already steered toward, and adds material without adding noise.

Risk: warm paper plus shadows can drift toward "cosy stationery". Held in check by keeping the palette to paper, ink, green, gold and refusing texture images.

## Direction B: night glass

One dark canvas, layered frosted panels, luminous accents.

| Token | Value | Job |
|---|---|---|
| canvas | `#0a1114` to `#060c0f` vertical, plus two coloured radial glows | ambient light source |
| glass | `rgba(255,255,255,.055)` + `backdrop-filter: blur(18px) saturate(150%)` | panels |
| edge | `rgba(255,255,255,.10)` inset top highlight | reads as a rim |
| accent | `#5ce1a8` current, `#8fd4ff` next | two lights only |
| radius | 16 to 18px | |

What changes: everything above, plus depth drawn from rim light and value rather than shadow, and one pulse on the current session dot.

Strength: the most "2026" of the three, and the one that looks least like a document. Suits a phone held in a dim hall.

Risk: low-contrast secondary text is easy to ship by accident, and a dark page is worse in Antalya daylight. Needs the light theme as a real second design, not an inversion.

## Direction C: product modern

Neutral canvas, white cards, three-step elevation, blue and amber state colours.

Strength: instantly legible, familiar, nothing to explain to anyone.
Risk: this is the generic language the whole brief was trying to escape. It is the safest and the least distinctive, and it is what an AI template produces by default.

Recommendation: **A, with B's palette as the dark theme.** C is on the sheet only to show the baseline being rejected.

## The ten changes that do the actual work

Applied to A, each is a line of CSS:

1. Three-step elevation scale, one light source, applied to rows, cells and the sticky bar.
2. Radial washes on the body so the top of the page is lit.
3. 1px inner top highlight on every raised surface (`inset 0 1px 0 rgba(255,255,255,.7)`).
4. Current session becomes a filled accent card with a 12 percent lift shadow in the accent hue.
5. Next session becomes a warm card with a gold hairline, not a green one.
6. Pills for controls, with a 1px top light edge and a real `:active` translate.
7. Radius scale: 14 rows, 16 cells, 999 pills. Nothing at 0, nothing above 18.
8. Mono restricted to times, indices and micro-labels. Everything readable is sans.
9. Transitions at 160 to 200ms on background, shadow and transform; killed under `prefers-reduced-motion`.
10. Sticky bar becomes a lifted surface with the same highlight and shadow as the cells, so scrolling feels like content passing under a real object.

## What none of this fixes

The next-session cell truncates at 390px. Any direction needs a shorter title there: use the session's short name, or drop the title and keep `Mon 9 Nov, 12:00, Blue Zone` only.

## Verification, per direction

- Render at 320, 390, 768, 1024, 1440. No element where `scrollWidth > clientWidth`.
- Computed contrast for every secondary text pair, target 4.5:1 minimum, measured not assumed.
- Force the current and next states, read the computed background, shadow and colour.
- Dark theme measured separately; no pure black, no pure white text.
- Screenshot each state; a design this reliant on light has to be judged as pixels, not as CSS.

## Files

- Mockups: `/home/simon/cop31-grid/mock-{A,B,C}-{m,t}.png`, contact sheet `mock-sheet.png`
- Generator: `mockups.py` in the build directory, reads the live `data.json`, so the mockups are his real schedule, not lorem ipsum.

## Header treatments (added after review)

Three headers were mocked on direction A, same body underneath. `header-sheet.png` shows all three at 390px.

- **H1 Chronograph.** A live Antalya clock, seconds ticking, UTC+3 stated. Beneath it a 24 hour ribbon of the day: each session drawn as a real block placed by its start and end, a hairline seam where two sessions abut, a red cursor at the current minute. It answers "where am I in the day" without reading a list, and it is the only element that could not exist on a printed page.
- **H2 Nameplate.** Newspaper masthead logic: a heavy rule, the name at display size, a colophon row of facts in mono, and a 190px ghost day numeral behind the name at 5 percent ink.
- **H3 Pass.** The header as a ticket stub: identity and a four-cell detail grid on the card, a perforated tear line with notches on both edges, and the actions sitting in the tear-off strip so the perforation is structural rather than decorative.

**Recommendation: H1.** It is the one that uses the medium, and the ribbon doubles as the current-position indicator the sticky bar already needs.

Two structural fixes fell out of building it, and both belong in whichever direction ships:

1. **The status strip stacks on a phone.** A half-width cell at 390px cannot hold a real session title: 72 characters need three lines and would still clip. Stacked, the same title fits two lines, and the current cell collapses to a single line because it is empty most of the day.
2. **The next cell leads with its start time.** A 21px time, then the title, then date and venue. Time is what a reader wants from that cell; the title is context, and context can be short without the cell looking broken.
