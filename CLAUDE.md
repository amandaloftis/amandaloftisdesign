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
The current design is a bright "editorial creative studio" look — candy
palette, oversized Archivo display type, Instrument Serif italic accents,
and animated wavy ribbon marquees — built as five real, separate pages.
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
client-side router, just plain `<a href="services.html">`-style links.

1. **`index.html` (Home)** — hero (cream wordmark "DESIGNED FOR YOU" over a
   two-up image split, a circular "Let's Connect" badge linking to Contact,
   and a mint ribbon marquee), a centered statement section linking to
   Services, an image+copy split band linking to Contact, a 6-card "What I
   bring" bento grid, a gold ribbon, a **centered** ("Beyond the Build")
   dark band on non-design logistics (image, eyebrow, headline, two body
   paragraphs — including a "Sites built on Wix, Squarespace, and Framer"
   line — and a button, all stacked and centered via `.section--center`,
   not the original design handoff's side-by-side image+copy layout)
   linking to Contact, a 4-tile "Recent Projects" preview (linking to 4 of
   the 6 `work/*.html` pages, plus an "All work" link to `work.html`), and
   a closing **centered** purple CTA band (also `.section--center`, not
   the handoff's left-heading/right-button `.cta-row`) linking to Contact.
   The wordmark string changed
   from the original design handoff's "AMANDA LOFTIS" — `.display-xl`'s
   `font-size` clamp was scaled down (`clamp(31px, 8.3vw, 125px)`, from the
   original `clamp(44px, 11.4vw, 168px)`) to keep the longer "DESIGNED FOR
   YOU" on one line without clipping at the `white-space: nowrap` treatment;
   re-check this clamp if the copy changes again.
2. **`work.html` (Work)** — purple hero, a **working** filter-pill row
   (All/Branding/Web/Illustration/Print — filters by `data-tags` on each
   `.work-card`, toggling the `hidden` attribute via the inline script; see
   JS behavior below), and a 6-card grid. Each card's image and title link
   to its own page under `work/`.
3. **`services.html`** — full-bleed photo hero, an orange ribbon
   overlapping it, a centered statement, 4 numbered service cards (Brand
   systems / Websites / Illustration & imagery / Copy & content), and a
   closing electric-blue CTA band with a "Built with" platform-badge row
   (`.platform-row`/`.platform-badge` — currently text pills reading Wix,
   Squarespace, Framer, standing in for real logo marks) above the
   "Not sure which piece you need?" CTA, linking to Contact.
4. **`about.html`** — mint band (bio copy + portrait placeholder), a
   purple ribbon, then a centered block with a rotating orange starburst
   SVG, a statement line, and 3 fact cards (9 yrs / 40+ / 1 person).
5. **`contact.html`** — a photo hero with a centered headline, then a
   centered mint contact form (Name/Email/Company/Message, `required` on
   Name/Email/Message) and the studio email below it. The form is
   **front-end only** — see JS behavior.

**`work/*.html` are real separate pages**, each a standalone HTML document
(own `<head>`, `<header>`, `<footer>`) covering one project: a back-to-work
link, category tags, an `<h1>` + one-line description, a large image
placeholder, a 3-item tinted gallery placeholder (each labeled — "Homepage",
"Mobile view", etc., reusing the `bg-*` card-tint classes for variety), a
three-part write-up ("The brief" / "What we built" / "The result"), a small
meta row (platform, services, timeline), and a closing CTA linking to
`../contact.html`. This detail-page template isn't part of the original
design handoff (which only specified the `work.html` grid, with no
drill-down) — it was extended from the previous site's project-page
pattern and restyled with the current design system's tokens/classes to
stay visually consistent. **These pages don't have their own contact form**
— the CTA and nav both link back to `contact.html`.

## Design system

All design tokens are CSS custom properties defined once in `:root` at the
top of `styles.css`. **Always use the existing variables instead of
hardcoding new hex values.** If a genuinely new color is needed, add it as
a named variable in `:root` rather than inlining a hex code, and say so.

### Colors (`:root`)

| Variable | Hex | Used for |
|---|---|---|
| `--ink` | `#131313` | body text, dark sections (`.section--dark`, footer), primary button hover/fill |
| `--cream` | `#F4F0EA` | page background |
| `--sand` | `#E7E2DA` | neutral image placeholder (`.ph`) / card tint (`.bg-sand`) |
| `--electric` | `#3939FF` | nav bar background, link hover, focus ring, `.section--blue` CTA band, filter-pill active state |
| `--daredevil` | `#FF5B22` | primary accent: "Let's Connect" badge, orange ribbon, submit button (`.btn--orange`), starburst SVG |
| `--arctic` | `#AEE6ED` | mint bands (`.section--mint`), mint cards/tiles (`.bg-mint`), contact form panel |
| `--royal` | `#DBB8FF` | purple bands (`.section--purple`), purple cards (`.bg-royal`) |
| `--gold` | `#F2BB05` | yellow bands/cards (`.bg-gold`), fact cards, footer link hover |
| `--peach` | `#FFD9C9` | bento/card tint (`.bg-peach`) |
| `--periwinkle` | `#C9D4FF` | bento/card tint (`.bg-peri`) |

This palette **fully replaced** the previous cobalt/coral/cream palette —
that palette's variables (`--cobalt`, `--coral`, `--pink`, `--dusty-blue`,
`--mustard`, `--paper`, `--ink-soft`, `--line`) no longer exist in
`styles.css`. Don't reintroduce them; if old inline references to them turn
up anywhere, that's leftover content that needs updating to the current
token set.

### Typography

- `--display: 'Archivo', sans-serif` — all headings (`.h1`/`.h2`/`.h3`/
  `.display-xl`), nav brand, ribbon marquee text, tile labels, buttons,
  card numbers/tags, footer wordmark. Weights 700/800 (this family replaced
  Sora). Letter-spacing is consistently tight/negative (`-0.02em` to
  `-0.045em`) — don't loosen it for a "friendlier" heading.
- `--serif: 'Instrument Serif', Georgia, serif` — italic-only, used via
  `<em class="serif">` for single emphasized words inside headings
  (`cohesive`, `not`, `best`, `life`, `Work`, `questions`, `Design Studio`,
  etc.). Never used for body text or at non-italic style. This family
  replaced Caveat's role as the site's "human touch" accent — but unlike
  Caveat, it's used consistently across every page, not sparingly in one
  spot.
- `--body: 'Plus Jakarta Sans', sans-serif` — everything else: body copy,
  ledes, form fields, card text, footer notes. Weights 400–700. This
  replaced Roboto Serif; there's no longer a separate `--label` font family
  — small tracked uppercase text (`.eyebrow`, tags, meta labels) now just
  uses `--body` at small size + letter-spacing + `text-transform: uppercase`
  at each call site, the same way the old `--label` treatment worked, just
  without its own font-family variable.

### Recurring motifs (don't reinvent these — extend them)

- **Wavy ribbon marquee** (`.ribbon`): an SVG `<textPath>` looping via a
  SMIL `<animate attributeName="startOffset" from="0%" to="-50%"
  dur="18s" repeatCount="indefinite">`, referencing a shared `<path id="wave">`
  defined once per page in a hidden `<defs>` block near the top of `<body>`.
  Duplicate the `<use>`/`<text>`/`<textPath>` block and change the `stroke`/
  `fill` colors and repeated marquee string per instance — it's not a
  shared component. **The SVG must scale uniformly** (`width:100%;
  height:auto`, no `preserveAspectRatio="none"`) or the text glyphs detach
  from the stroked band. Every page that includes a ribbon also needs the
  small reduced-motion script (see JS behavior) so the `<animate>` element
  gets stripped for users who prefer reduced motion — copy that script
  along with the ribbon, don't add a ribbon without it.
- **Circular badge** (`.badge`): a solid daredevil-orange circle, used once
  on the Home hero ("Let's Connect" → Contact). Not a repeated pill-badge
  system like the previous design's dotted badges — there's only one on
  the whole site.
- **Nav social icon** (`.nav__social`, and `.nav-drawer__social` inside the
  mobile drawer): a white inline-SVG Instagram glyph (the Feather/Lucide
  rounded-square-camera icon, `stroke="currentColor"`, no fill) at the far
  right of the nav bar, linking to `https://instagram.com/amandaloftisdesign`
  — **a placeholder handle**, swap for Amanda's real Instagram URL before
  launch. Sits inside `.nav__right` alongside `.nav__links` on desktop, and
  is duplicated at the bottom of the mobile drawer (see JS behavior below)
  since the drawer replaces `.nav__right` entirely at narrow widths — update
  both copies of the icon/link together, they're not shared markup.
- **Platform badge row** (`.platform-row`/`.platform-badge`): a "Built with"
  label + pill row (currently reading Wix / Squarespace / Framer) inside
  Services' closing blue CTA band, reusing the translucent-chip look from
  `.card__tags`. These are plain text pills, not real brand logo marks —
  swap in actual SVG logos if/when that's wanted, but don't hand-draw
  approximations of the brand marks themselves.
- **Bento / numbered cards** (`.card`, `.bg-*`): rounded, flat-color cards
  used for "What I bring" (Home) and the 4 numbered services (Services).
  Tints rotate through `.bg-mint/.bg-royal/.bg-gold/.bg-peach/.bg-sand/.bg-peri`
  — no card should reuse an adjacent card's tint in the same grid.
- **Project tiles** (`.tile`): Home's "Recent Projects" preview — an image
  placeholder with a translucent-ink label pinned bottom-left, hover tints
  the background royal. Distinct from `.work-card`, the fuller card used on
  `work.html` itself (image + tag pills + title + blurb).
- **Photo hero** (`.photo-hero`): a full-bleed sand placeholder band used
  on Services (bottom-left aligned text) and Contact (centered text). The
  project detail pages don't use `.photo-hero` — they use a plain `.section`
  + a large `.ph` block instead, since they need a back-link and tag row
  above the image.
- **Starburst** (`.star`): a 16-point orange SVG asterisk on About, spinning
  40s linear infinite. Respects `prefers-reduced-motion` via a CSS rule in
  `styles.css` (`.star { animation: none }` under the media query) — no JS
  needed for this one, unlike the ribbons.
- **Facts row** (`.facts`/`.fact`): 3 flat gold cards, used once on About.

## JS behavior

All JS is inline in a closing `<script>` tag, and **pages don't all carry
the same script** — don't assume every page has the same JS available.

- **Reduced-motion guard for ribbons**: every page with a `.ribbon` SVG
  (`index.html`, `services.html`, `about.html`) carries this snippet, which
  removes the SMIL `<animate>` elements (freezing the marquee at its start
  position) when the user prefers reduced motion:
  ```html
  <script>
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    document.querySelectorAll('animate').forEach(function (el) { el.remove(); });
  }
  </script>
  ```
  `work.html`, `contact.html`, and the `work/*.html` detail pages have no
  ribbons and don't carry this script.
- **Work-page filtering** (`work.html` only): clicking a `.filters span`
  pill sets `aria-pressed` on the clicked pill (clearing the others) and
  toggles the `hidden` attribute on each `.work-card` by matching the
  pill's `data-filter` against the card's space-separated `data-tags`. This
  is real, working filtering — not the "visual only" placeholder from the
  original design handoff.
- **Contact form**: `contact.html`'s `<form onsubmit="return false">` is
  **not wired to a backend** and has no JS handler at all — submitting just
  no-ops. `required` is set on Name, Email, and Message for baseline
  browser-native validation. If asked to make the form actually send
  somewhere, replace the `onsubmit="return false"` with a real submit
  handler (e.g. point it at Formspree/Basin, or add a `fetch()` call to a
  backend).
- **Mobile nav drawer** (every page): below 680px, CSS hides `.nav__right`
  (the links + Instagram icon) and shows `.nav__toggle`, a hamburger button.
  A `#navDrawer` panel (fixed, right-aligned, slides in via
  `transform: translateX()`) and a `#navScrim` backdrop live as siblings
  right after `</header>` — not nested inside the sticky `<header>` — so
  they can cover the full viewport height regardless of scroll position.
  Each page repeats this identical inline script (same IDs everywhere:
  `navToggle`, `navDrawer`, `navScrim`, `navDrawerClose`):
  ```html
  <script>
  (function () {
    var toggle = document.getElementById('navToggle');
    var drawer = document.getElementById('navDrawer');
    var scrim = document.getElementById('navScrim');
    var closeBtn = document.getElementById('navDrawerClose');
    function openDrawer() { /* adds .open to drawer+scrim, locks body scroll */ }
    function closeDrawer() { /* removes .open, unlocks body scroll */ }
    toggle.addEventListener('click', openDrawer);
    closeBtn.addEventListener('click', closeDrawer);
    scrim.addEventListener('click', closeDrawer);
    drawer.querySelectorAll('a').forEach(function (a) { a.addEventListener('click', closeDrawer); });
    document.addEventListener('keydown', function (e) { if (e.key === 'Escape') closeDrawer(); });
  })();
  </script>
  ```
  The drawer duplicates the same nav links (with the same `aria-current`)
  and the Instagram icon found in `.nav__right` — **when changing nav
  links, update both the desktop `.nav__links` copy and the
  `.nav-drawer__links` copy on every page**, they're not shared markup.
  This is deliberately re-added on top of the original design handoff,
  which had no hamburger at all (nav links just wrapped via flex-wrap);
  the 680px breakpoint that drives it (in `styles.css`) is the **one
  fixed `@media` breakpoint on the site** — see the note in "Conventions
  for making changes" below.

## Content that is still placeholder

Be aware these are illustrative, not real, and shouldn't be presented to
end users as final without checking with the person running this project:

- **Every image on every page** is a plain `.ph`/`.photo-hero` placeholder
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
- Reuse existing CSS custom properties for color; reuse `.btn`/`.card`/
  `.tile`/`.ph`/`.badge`/`.ribbon` classes and the color-tint modifiers
  (`.bg-*`, `.section--*`) for anything that matches those patterns rather
  than writing new one-off styles.
- New pages under `work/` should follow the existing template: same
  `<head>` font links, same `<header>`/`<footer>` markup with paths
  adjusted for the folder depth, same back-link/tags/hero/gallery/write-up/
  meta/CTA structure. Don't invent a different page shell per project.
- This design is fully fluid (`clamp()` for type/spacing, `auto-fit`/
  `minmax()` grids, `flex-wrap` everywhere) — **the only fixed `@media`
  breakpoint in `styles.css` is the 680px one that swaps the nav between
  its inline links and the hamburger/drawer** (see JS behavior above).
  Don't add other fixed breakpoints for general layout; keep using
  `clamp()`/`auto-fit` for anything that isn't a hard on/off UI toggle like
  the nav. Verify layout changes at ~375px, ~768px, and ~1440px, and also
  check just above/below 680px specifically when touching the nav.
- Respect the existing `prefers-reduced-motion` handling (the CSS rule for
  `.star`, and the inline-script pattern for ribbons) — don't add new
  animations without accounting for both.

## Design inspiration references

A reference board of 4 outside design examples, kept here for future design
decisions — palettes, type pairings, and layout motifs to pull from when
asked to design something new or push an existing design further. **None of
this is a spec for the current site** — it's a menu, not a directive. (The
current redesign already leans on several of these throughlines — oversized
display type, a candy-bright pop palette on a neutral base, and circular
badge/sticker accents — more directly than the previous iteration did.)

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
texture and confidence from — while keeping the current Archivo/Instrument
Serif/Plus Jakarta Sans type pairing and electric/daredevil/arctic/royal/
gold candy palette documented above.
