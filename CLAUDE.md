# CLAUDE.md

Guidance for Claude Code when working in this repo.

## Project overview

Marketing website for **Amanda Loftis Design Studio**, a one-person design
studio based in Greensboro, NC, offering brand systems, websites,
illustration, and copy — the whole digital presence built by one person
instead of handed off between vendors.

Goals of the site: explain the studio's holistic pitch (one person, whole
picture), showcase past projects, break down the four service pillars, tell
Amanda's story on an About page, and convert visitors via a Contact page
form.

**As of September 2026 the site went through a full visual and structural
redesign**, replacing an earlier cobalt/coral, Sora/Roboto-Serif single-page
design (anchor-navigated, with a pricing section and a JS contact modal).
That design (bright candy palette, ribbon marquees) was then restyled in
**November 2026** into the current dark "product studio" look — black
canvas, one orange accent, giant Instrument Serif headlines, wide Archivo
statements, Space Mono labels — still built as five real, separate pages
(see Design system).
**There is no pricing page, no contact modal, and no booking-link CTA in
the current design** — don't reintroduce them without being asked; they
belonged to the previous iteration.

## Tech stack

- **Plain HTML + CSS + vanilla JS. No framework, no bundler, no package.json.**
- **One shared `styles.css`** — every HTML page links to it with
  `<link rel="stylesheet" href="styles.css">` (or `../styles.css` from
  inside `work/`). Don't move CSS inline per-page and don't fork
  `styles.css` into a per-page copy.
- JS is inline per-page in a `<script>` block — there's no shared JS file,
  and that's fine at this size (see JS behavior below).
- Fonts load from Google Fonts via `<link>` tags in every page's `<head>`
  — no self-hosted font files.
- No build step. Editing a file and opening it (or refreshing) in a browser
  is the entire dev loop.

## File structure

```
index.html        — Home page
work.html          — Work index (filterable project grid)
services.html       — Services page
about.html            — About page
contact.html           — Contact page (real page, not a modal)
styles.css               — shared stylesheet for every page on the site
images/
  logo.png                 — legacy wordmark lockup from the previous design — currently unreferenced
  avatar.png                — legacy About portrait from the previous design — currently unreferenced
  Amanda Loftis.png           — legacy asset — currently unreferenced
work/
  piedmont-global-preschool.html   — project detail page
  fluree.html                       — project detail page
  date-night-questions.html          — project detail page
  sable-and-fen.html                   — project detail page
  ridgeline-trail-co.html                — project detail page
  harbor-financial.html                    — project detail page
```

**All five top-level pages, `styles.css`, `images/`, and `work/` must stay
in the same relative locations to each other.** Top-level pages link to
`styles.css` and to each other directly by filename (`work.html`,
`contact.html`, etc.); pages inside `work/` link back up with
`../styles.css`, `../index.html`, `../work.html`, and so on. If any of
these are renamed or moved, every page that references them needs
updating, not just one.

The current design ships with **zero real photography** — every image slot
on every page is a plain gray/tinted placeholder `<div>` labeled `IMAGE` or
`PORTRAIT`. The `images/` files above are leftovers from the previous
design and are not linked from any current page; don't wire them back in
without being asked — when real photography is ready, replace the
placeholder `.ph` divs with `<img>` tags at the same box dimensions and
`border-radius`, using `object-fit: cover`.

## Page structure

Five real, separate top-level pages — no anchor-scrolling single-pager, no
client-side router, just plain `<a href="services.html">`-style links. Every
page shares the same floating pill nav, footer, and scroll-reveal script.

1. **`index.html` (Home)** — a full-width glowing image panel (`.ph--hero
   .ph--glow`) with the giant serif headline "Designed for you." and a
   subline below it, plus a small `.hero-card` at the right ("Brand, web,
   illustration & copy / Greensboro, NC" + a "Let's connect" button →
   Contact). Then, in order: a "What I do here" statement linking to
   Services; an image + "Strategy" statement two-up linking to Contact; a
   6-panel "What I bring" grid (numbered 01–06); a dotted "Beyond the build"
   panel (statement on the left; logistics copy, a "Sites built on" Wix /
   Squarespace / Framer tag row and a button on the right); a 2×2 "Recent
   projects" grid (4 of the 6 `work/*.html` pages, plus an "All work" link);
   and a centered closing CTA with the giant serif "Let's make it."
2. **`work.html` (Work)** — giant serif "Selected work." header, a
   **working** filter row (All/Branding/Web/Illustration/Print `<button>`s
   with `aria-pressed`, filtering by `data-tags` on each `.work-card`), and
   a 6-card grid (image, caption, mono tags). Each card's image and title
   link to its page under `work/`.
3. **`services.html`** — glowing hero panel with the giant serif "What I do
   best.", a two-column statement ("Take one piece or the whole system…"),
   4 numbered service panels (Brand systems / Websites / Illustration &
   imagery / Copy & content, each with mono tags), and a closing panel with
   a "Built with" Wix/Squarespace/Framer tag row above the "Not sure which
   piece you need?" CTA → Contact.
4. **`about.html`** — "Hi, I'm Amanda." serif headline + statement + bio
   paragraphs beside a tall portrait placeholder; then a centered spinning
   orange starburst, the statement "Everything built with intention, none
   of it off the shelf.", and 3 spec cards (9 yrs / 40+ / 1).
5. **`contact.html`** — giant serif "Let's talk.", then a two-column split:
   statement + studio email on the left, a dotted panel with the contact
   form (Name/Email/Company/Message, `required` on Name/Email/Message) on
   the right. The form is **front-end only** — see JS behavior.

**`work/*.html` are real separate pages**, each a standalone HTML document
(own `<head>`, nav, footer) covering one project: a mono back-to-work link,
tag row, serif `<h1>` + one-line statement, a large glowing image
placeholder, a 3-up gallery of labeled placeholders, a three-column
write-up ("The brief" / "What we built" / "The result", each under an
orange eyebrow), a hairline-topped meta row (platform, services, timeline),
and a closing CTA panel linking to `../contact.html`. **These pages don't
have their own contact form.**

## Design system

**As of November 2026 the site was restyled to a dark "product studio"
look**, modeled on the feel of electronicmaterialsoffice.com: near-black
canvas, one hot orange accent, huge light serif display headlines, wide
grotesk "statement" paragraphs with a white lead and gray continuation,
tiny orange monospace eyebrows, dark rounded panels, and a floating
translucent pill nav. This replaced the September 2026 bright "candy"
design (cream background, mint/royal/gold/peach tints, wavy ribbon
marquees, circular "Let's Connect" badge) — don't reintroduce those pieces
without being asked.

All design tokens are CSS custom properties defined once in `:root` at the
top of `styles.css`. **Always use the existing variables instead of
hardcoding new hex values.** If a genuinely new color is needed, add it as
a named variable in `:root` rather than inlining a hex code, and say so.

### Colors (`:root`)

| Variable | Hex | Used for |
|---|---|---|
| `--black` | `#000000` | page background |
| `--surface` | `#111111` | panels/cards, footer, mobile drawer |
| `--surface-2` | `#1A1A1A` | image placeholder base |
| `--line` | `#2F2F2F` | hairlines, tag/filter outlines, input borders, hero card |
| `--gray` | `#6D6D6D` | the gray "continuation" half of statements and captions, meta labels |
| `--gray-light` | `#CACACA` | body copy, nav links, footer links |
| `--white` | `#FFFFFF` | headlines, the bold lead of statements/captions |
| `--daredevil` | `#FF5B22` | **the only accent**: buttons, eyebrows, panel numbers, nav dot, active filter, focus ring, starburst, image glows |
| `--daredevil-dim` | `#7C2A0C` | deep orange for glow gradients |
| `--nav-glass` | `rgba(47,47,47,.7)` | floating nav pill (with backdrop blur) |

Keep the palette monochrome + one orange. Don't add a second accent color.
The previous candy tokens (`--cream`, `--sand`, `--electric`, `--arctic`,
`--royal`, `--gold`, `--peach`, `--periwinkle`, `--ink`) no longer exist.

### Typography

Loaded from Google Fonts in every page's `<head>`:
`Archivo` (with the **`wdth` axis, 62–125**), `Instrument Serif`, `Space Mono`.

- `--serif: 'Instrument Serif'` — **roman (non-italic) for giant display
  headlines** (`.display`, `.display--md`, footer wordmark, mobile drawer
  links), tight `-0.035em` tracking and `.86` line-height. Italic only for
  the footer's "Design Studio".
- `--sans: 'Archivo'` — everything else. `.statement` uses it **widened**
  (`font-stretch: 118%`) at weight 500 with tight negative tracking; the
  pattern is `<p class="statement"><b>White lead sentence.</b> gray
  continuation</p>`. `.statement--sm`/`--xs` are smaller sizes. `.h3`,
  buttons and `.spec__k` numbers also use widened Archivo. Body copy is
  regular-width Archivo.
- `--mono: 'Space Mono'` — small uppercase labels only: `.eyebrow` (orange;
  `.eyebrow--gray` for neutral labels), `.tag`, filter buttons, form
  labels, placeholder labels, back link, footer bottom line.

### Recurring components (reuse — don't reinvent)

- **Floating nav** (`.nav`): fixed pill 8px from the top, glass background,
  orange-dot serif brand "Amanda Loftis®" at left; links, Instagram icon
  (`.nav__social`, placeholder handle `instagram.com/amandaloftisdesign` —
  swap before launch) and a small orange "Let's talk" button at right. The
  current page gets `aria-current="page"` and an orange dot. `<main>` has
  64px top padding to clear it.
- **Statement** (`.statement`) and **caption** (`.caption`): both use the
  white-`<b>`-lead + gray-continuation pattern.
- **Eyebrow** (`.eyebrow`): tiny orange mono label above most sections.
- **Panels** (`.panel`): dark `#111` cards with 22px radius. `.panel--dots`
  adds the 12px dot-grid texture; `.panel__num` is the orange mono "01".
  Alternate dotted/plain panels within a grid.
- **Spec cards** (`.panel.spec` + `.spec__k`/`.spec__v`): eyebrow + huge
  wide number + label (About facts).
- **Image placeholders** (`.ph`): dark gradient boxes with a mono
  "IMAGE"/"PORTRAIT" label bottom-left. Modifiers: `.ph--glow` (orange
  radial glow — the signature look), `.ph--dots`, `.ph--hero` (tall hero),
  `.ph--wide` (16:10), `.ph--tall` (4:5). When real photography is ready,
  replace with `<img>` at the same radius using `object-fit: cover`.
- **Figure** (`.figure`): media + caption + optional `.tags`, used for work
  cards and project previews.
- **Buttons** (`.btn`): orange, 8px radius, widened Archivo, auto-appended
  `↗` arrow, orange glow on hover. `.btn--sm`, `.btn--ghost` variants.
- **Tags** (`.tags` > `.tag`): outlined mono pills. Platform names
  (Wix/Squarespace/Framer) are plain text tags, not hand-drawn logo marks.
- **Text link** (`.text-link`): underlined secondary link ("View the
  services", "All work", email).
- **Starburst** (`.star`): 16-point orange SVG on About, spinning 40s;
  disabled under `prefers-reduced-motion` in CSS.
- **Footer** (`.footer`): rounded `#111` panel inset from the page edges,
  big serif wordmark, Pages/Elsewhere link columns, mono bottom line.

Layout helpers: `.section` (big top padding; `--tight` variant), `.stack`
(`--center`), `.split` + `.split__aside`, `.row-between`, and `.grid`
(`--2`/`--3`/`--4`, all `auto-fit`/`minmax`).

## JS behavior

All JS is inline per page. Every page carries:

- A one-liner in `<head>`: `document.documentElement.classList.add('js')`.
- **Mobile nav drawer**: below 680px CSS hides `.nav__right` and shows
  `.nav__toggle`. `#navDrawer` and `#navScrim` sit as siblings right after
  `</header>`. Same IDs everywhere (`navToggle`, `navDrawer`, `navScrim`,
  `navDrawerClose`); the script opens/closes on toggle, close button, scrim,
  link click and Escape, and locks body scroll while open. **When changing
  nav links, update the desktop `.nav__links`, the `.nav-drawer__links`
  and the footer link column on every page** — they're not shared markup.
- **Scroll reveal**: elements with `.reveal` fade up when they enter the
  viewport (IntersectionObserver adds `.is-in`). CSS only hides them when
  `html.js` is present **and** `prefers-reduced-motion: no-preference`, so
  content is always visible without JS or with reduced motion.

Page-specific:

- **Work filtering** (`work.html` only): clicking a `#filters button` sets
  its `aria-pressed="true"` (clearing the others) and toggles `hidden` on
  each `.work-card` by matching `data-filter` against its `data-tags`.
- **Contact form**: `contact.html`'s `<form onsubmit="return false">` is
  **not wired to a backend** — submitting no-ops. To make it send, replace
  that with a real handler (Formspree/Basin, or `fetch()` to a backend).

There are no ribbons or SMIL animations any more, so the old
"strip `<animate>` for reduced motion" script is gone.

## Content that is still placeholder

Be aware these are illustrative, not real, and shouldn't be presented to
end users as final without checking with the person running this project:

- **Every image on every page** is a plain `.ph` placeholder
  box — there is no real photography or portrait anywhere on the current
  site.
- The 6 project names and one-line blurbs on `work.html` (Piedmont Global
  Preschool, Fluree, Date Night Questions, Sable & Fen, Ridgeline Trail
  Co., Harbor Financial) come from the design handoff. **Everything else on
  their linked `work/*.html` pages — the "brief"/"what we built"/"result"
  write-ups, gallery labels, platform/services/timeline meta — was invented
  to fill out the detail-page template** and needs replacing with real
  project details before this goes live.
- The contact email (`hello@amandaloftis.io`, shown on `contact.html`) is
  the address from the design handoff — confirm it's the address Amanda
  wants published before launch.
- The About page's "9 yrs / 40+ / 1" facts are from the design handoff;
  confirm they're current.
- There is currently **no pricing page and no booking-link CTA anywhere on
  the site** — both existed in the previous design and were dropped in
  this redesign. If pricing or scheduling needs to come back, that's a
  deliberate addition to plan, not something to quietly restore.

## Conventions for making changes

- CSS lives in the shared `styles.css`, not per-page. New pages should
  `<link>` to it (with the correct relative path for their folder depth),
  not copy styles inline. JS is still fine inline per-page at this
  project's size.
- Reuse existing CSS custom properties for color; reuse `.statement`/
  `.caption`/`.eyebrow`/`.panel`/`.ph`/`.figure`/`.btn`/`.tag` and the
  layout helpers (`.section`, `.stack`, `.split`, `.grid--*`) rather than
  writing new one-off styles or inline styles.
- New pages under `work/` should follow the existing template: same
  `<head>` font links, same `<header>`/`<footer>` markup with paths
  adjusted for the folder depth, same back-link/tags/hero/gallery/write-up/
  meta/CTA structure, and `.reveal` on the blocks. Don't invent a different page shell per project.
- This design is fully fluid (`clamp()` for type/spacing, `auto-fit`/
  `minmax()` grids, `flex-wrap` everywhere) — **the only fixed `@media`
  breakpoint in `styles.css` is the 680px one that swaps the nav between
  its inline links and the hamburger/drawer** (see JS behavior above).
  Don't add other fixed breakpoints for general layout; keep using
  `clamp()`/`auto-fit` for anything that isn't a hard on/off UI toggle like
  the nav. Verify layout changes at ~375px, ~768px, and ~1440px, and also
  check just above/below 680px specifically when touching the nav.
- Respect the existing `prefers-reduced-motion` handling (the CSS rule for
  `.star`, and the reveal styles that only apply under
  `no-preference`) — don't add new animations without accounting for it.

## Design inspiration references

A reference board of 4 outside design examples, kept here for future design
decisions — palettes, type pairings, and layout motifs to pull from when
asked to design something new or push an existing design further. **None of
this is a spec for the current site** — it's a menu, not a directive. (The
current dark design keeps two of these throughlines — oversized display
type and small tracked labels — but deliberately drops the candy palette
and sticker/collage motifs.)

### 1. Brand swatch cards (style-guide reference)

A clean brand-guideline format: one card per color, name in bold sans, small
"PRIMARY"/"SECONDARY" tag top-right, hex/RGB/CMYK in a 3-column table below,
on a cobalt-blue backdrop.

Palette shown: Ink `#282828` (primary), Paper `#F9F6EF` (primary), Cobalt
`#193497` (secondary), Pink Eraser `#EDA398` (secondary).

The card-with-data-table format itself is reusable any time a palette or
spec needs to be documented visually rather than just listed.

### 2. Podcast quote card (paper-collage / sticker style)

A single pull-quote treated like a torn notepad sheet, layered over an
offset solid-color backing card, on a dark grainy background. Textured
near-black navy bg (`~#0E141C`), burnt-orange backing sheet + emphasized
quote line (`~#EA632E`), warm cream "paper" card (`~#F9F8ED`), soft lavender
circular badge accent (`~#EFDAFA`).

Notes: bold rounded condensed sans headline with one emphasized line in the
accent color rather than the whole quote; small icon + label eyebrow above
the headline; perforated/torn top edge, drop shadow, slight rotation sell
the paper-cutout feel; circular sticker badges pinned at corners.

### 3. Editorial magazine spread

A column-blocked magazine page: oversized serif display type on a rust
panel, a cream sidebar with small-caps labels and body copy, a rotated
vertical index tab, and a blush photo-illustration panel with a periwinkle
pull-quote, plus hand-drawn circle/arrow annotations over the clean type.

Approximate palette: rust `#DE6E43`, cream `#FCF7E3`, dusty blue `#ACC4D8`,
peach blush `#F7E6D7`, muted periwinkle-indigo quote text (~`#5C5FA6`).

Notes: a quirky high-contrast serif for the big display headline; small
tracked all-caps for section labels; a narrow vertical rotated-text tab as
a wayfinding device; hand-drawn ink marks over otherwise clean typesetting
add a human, annotated feel — cheap to fake with a rough inline SVG stroke.

### 4. Social media template moodboard (Canva-style collage)

A scattered, overlapping grid of social post templates — tilted rectangular
cards mixing bold headline type, short punchy copy, and real photography,
on a candy-bright pastel palette: periwinkle blue `#AFC1F8`, chartreuse/olive
`#DBDA7D`, coral `#EF8464`, soft pink `#F2CDD0`, cream `#FFFCEE`.

Notes: bold condensed/rounded sans headlines mixed at very different scales
card-to-card; cards overlap and tilt like a scattered photo stack rather
than sitting in a rigid grid; photography sits directly inside color-blocked
cards rather than in its own neutral frame.

### Cross-references / throughlines

Things that show up in most or all four references, useful as general
principles rather than any one palette:

1. **Oversized, confident display type carries the design** — every example
   leans on type scale rather than decoration for impact.
2. **A warm neutral (paper/cream) base with one or two saturated "pop"
   colors laid on top**, not an all-over saturated palette.
3. **Small tracked all-caps labels as wayfinding.**
4. **Rotation / collage / offset-layering as a structural device** —
   stacked cards, tilted grids, rotated tabs.
5. **Circular badge/sticker accents** recur constantly.

If asked to design something new in this project (a new section, a new
one-off page, a social template, etc.), these are the references to pull
texture and confidence from — while keeping the current Instrument Serif/
Archivo/Space Mono type pairing and black + daredevil-orange palette
documented above.
