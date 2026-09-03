# CLAUDE.md

Guidance for Claude Code when working in this repo.

## Project overview

Marketing website for **Amanda Loftis Design Studio**, a web design business based in
Greensboro, NC. Serves small business owners looking for Wix, Squarespace, or
Framer sites, plus add-on services (logo design, illustration, photography,
marketing design, copywriting).

Goals of the site: showcase past projects, present pricing packages + à la
carte add-ons, tell Amanda's story on an About section, and convert visitors
via a "start the conversation" form + Google Calendar booking link.

## Tech stack

- **Plain HTML + CSS + vanilla JS. No framework, no bundler, no package.json.**
- **As of the addition of individual project pages, CSS moved out of
  `index.html` and into a shared `styles.css`.** Every HTML page links to it
  with `<link rel="stylesheet" href="styles.css">` (or `../styles.css` from
  inside `work/`). This was a deliberate change from the original
  single-file philosophy — with 7 HTML pages now sharing one design system,
  a shared stylesheet is what keeps a palette/type edit a one-file change
  instead of a seven-file one. **Don't move CSS back inline per-page** and
  don't fork `styles.css` into a per-page copy — every page should keep
  pointing at the same file.
- JS is still small enough to stay inline per-page in a `<script>` block
  (see JS behavior below) — there's no shared JS file, and that's fine at
  this size.
- Supporting images live in `images/`, referenced by relative path.
- Fonts load from Google Fonts via a `<link>` tag in every page's `<head>`
  — no self-hosted font files.
- No build step. Editing a file and opening it (or refreshing) in a browser
  is the entire dev loop.

## File structure

```
index.html               — the marketing site (one page, anchor-linked sections)
styles.css                — shared stylesheet for every page on the site
images/
  logo.png                 — "Amanda Loftis Design Studio" wordmark lockup (transparent bg)
  avatar.png                — illustrated portrait used on the About section (transparent bg)
work/
  willow-rye.html            — project detail page
  pinecrest-dental.html       — project detail page
  marigold-studio-yoga.html    — project detail page
  the-corner-nook.html          — project detail page
  bramblewood-landscaping.html    — project detail page
  nora-grace-photography.html      — project detail page
```

**`index.html`, `styles.css`, `images/`, and `work/` must all stay in the
same relative locations to each other.** Paths are relative throughout:
`index.html` links to `images/logo.png` and `styles.css`; pages inside
`work/` link back up with `../styles.css`, `../images/logo.png`, and
`../index.html#section`. If any of these are renamed or moved, every page
that references them needs updating, not just one.

## Page structure

`index.html` is still a single scrolling page, anchor-navigated
(`<a href="#section-id">`, `scroll-behavior: smooth`) — no client-side
router. Sections, in order, each an `id`'d `<section>`:

1. `header` — sticky nav, logo image, anchor links, mobile hamburger toggle
2. `#top` — hero: three stacked, rotated, colored "block" headline lines
3. `#services` — 6-item list of services under a plain "Services" heading
4. `.logistics-banner` — full-bleed cobalt band between Services and Work;
   no `id`, not part of the anchor-nav (nothing links directly to it) —
   see "Logistics banner" under Recurring motifs below
5. `#work` — portfolio grid (6 placeholder project cards, image-forward —
   just a large thumb, project name, and a "View project" link, both
   left-aligned under the thumb; no description text or platform-tag pill).
   **Each card now links to its own page** under `work/` (see below)
   instead of `href="#"`.
6. `#pricing` — 3 pricing plan cards + à la carte add-ons list
7. `#about` — bio + illustrated avatar + quick-fact badges
8. `#contact` — CTA section, opens the modal form
9. `footer` — logo, nav links repeated, address line
10. `#modalOverlay` — hidden-by-default modal containing the contact form

**`work/*.html` are real separate pages, not anchors** — each is a
standalone HTML document (own `<head>`, `<header>`, `<footer>`) covering one
portfolio project: a back-to-work link, a tag line (platform · category), a
big colored hero image (same italic-initial treatment as the card it was
linked from — `.project-hero-image` reuses the same `.sky`/`.ink`/`.blush`
color modifiers as `.project-thumb`, so a card's color always matches its
own detail page), a 3-item image gallery (also placeholder color blocks,
each labeled — "Homepage", "Mobile view", etc.), a three-part write-up
("The brief" / "What we built" / "The result"), a small meta row (platform,
services, timeline), and a closing CTA linking back to
`../index.html#contact`. **These pages do not have their own contact
modal** — the CTA and nav both link back to the anchors on `index.html`
rather than duplicating the form. If a project page ever needs its own
lead-capture form instead of linking out, that's a bigger call to make
deliberately, not something to add to one page at a time.

## Design system

All design tokens are CSS custom properties defined once in `:root` at the
top of the `<style>` block. **Always use the existing variables instead of
hardcoding new hex values.** If a genuinely new color is needed, add it as a
named variable in `:root` rather than inlining a hex code, and say so.

### Colors (`:root`)

| Variable | Hex | Used for |
|---|---|---|
| `--cream` | `#F7F2E9` | page background (warm ivory) |
| `--paper` | `#F7E6D7` | card backgrounds (portfolio, pricing, form) — peach blush |
| `--ink` | `#161D25` | primary text, default dark surfaces (near-black) |
| `--ink-soft` | `#4A5058` | secondary/body copy text — derived tint of ink, not a swatch color |
| `--cobalt` | `#203391` | **the site's primary brand color.** Primary buttons (`.btn-primary`), ghost-button hover fill, the featured pricing plan, one hero block, one portfolio thumb, default badge border |
| `--coral` | `#E16D3E` | secondary accent: flower doodle SVGs, one hero block, highlighted badge border (`.badge.on-sky`), nav-link hover underline, focus outline |
| `--pink` | `#F7D0D9` | soft accent: hero block, portfolio thumb, featured plan tag text |
| `--periwinkle` | `#B0BFFA` | light accent: hero block, one service swatch, one portfolio thumb |
| `--dusty-blue` | `#ACC4D8` | accent: badge border variant (`.badge.on-blush`) |
| `--mustard` | `#E0A342` | paired with a black border (About photo frame, one service swatch) — deliberately used sparingly |
| `--line` | `rgba(22,29,37,0.16)` | hairline borders/dividers |

**Color history:** the palette went through two prior iterations — first
derived from a client-supplied reference of scalloped circular badges
(mint/cream, coral/pink, black/chartreuse), then a navy accent was added and
promoted alongside coral to be the two most prominent colors. It has since
been **fully replaced** by a 9-color brand palette supplied directly by the
client (ivory / peach / near-black / cobalt / rust-orange / pink / periwinkle
/ dusty-blue / mustard), with **cobalt formally established as the primary
brand color** — it now drives every primary button and the featured pricing
plan, not just the hero. Coral remains a strong secondary accent. Mint and
chartreuse from the earlier palette are gone entirely; don't reintroduce them.
CSS variable *names* like `--cobalt`/`--coral`/`--pink` etc. reflect their
actual current meaning — unlike the previous iteration, there's no
`--sky`/`--blush` naming mismatch to account for at the variable level (see
below for a remaining mismatch at the *class name* level).

### Typography

- `--heading: 'Sora', sans-serif` — all headlines and headline-adjacent
  elements: `h1`/`h2`/`h3` (via a base rule, so any plain heading picks this
  up automatically), the hero's rotated `.block` lines, `.service-name`, the
  big italic initial letter in `.project-thumb`, and every CTA button
  (`.btn`, covering `.btn-primary`/`.btn-ghost` everywhere they're used).
  Bold, rounded-corner grotesk — this replaced Fraunces as the site's
  display font. **Weight is 700 (bold) everywhere Sora is used** — every
  `.block`/`h1`/`h2`/`h3`/`.btn`/etc. font-weight was standardized to 700;
  don't introduce 600 (semibold) anywhere in this family, even for a "should
  feel a little lighter" heading — 700 is the one weight this site uses for
  Sora.
- `--body: 'Roboto Serif', serif` — the base font for everything else: set
  once on `body` and inherited by paragraphs, descriptions, list items, and
  form fields (`.form-row input/textarea/select`, the modal's service
  checkboxes, `.service-desc`). This replaced Forum, which briefly replaced
  Fraunces' body-copy role. Roboto Serif is a full variable family (real
  weights and italics available via its `wght`/`opsz`/`ital` axes) — unlike
  Forum, there's no single-weight landmine here, but this site still only
  ever uses it at default weight/style; don't add bold/italic body copy
  without checking whether `--heading` (Sora) is the better fit first.
- `--label: 'Roboto Serif', serif` — small tracked/uppercase labels: nav
  links, badges, pricing figures, form labels, footer links. **Same font
  family as `--body`** (this used to be `--mono: 'Space Mono'` — Space Mono
  was dropped from the project entirely, including the Google Fonts import
  and a hardcoded `font-family="Space Mono, monospace"` inside the hero
  stamp SVG, which was updated too). What still visually separates a
  "`--label`" element from body copy is size + letter-spacing +
  `text-transform: uppercase` at each call site, not the font itself — don't
  remove those just because the family matches `--body` now, or labels stop
  reading as labels. **No longer used for buttons** — CTAs use `--heading`
  (Sora, 700).
- `--script: 'Caveat', cursive` — used once, sparingly, for the "Hi, I'm
  Amanda" lead-in on the About section, and inline (hardcoded, not via the
  variable) in the hero stamp SVG. Not used in the nav/footer logo anymore
  (see below) since that's now an image.

### Recurring motifs (don't reinvent these — extend them)

- **Dotted pill badges** (`.badge`): `border: 1.5px dotted`, `border-radius:
  999px`, `--label` font, uppercase, tracked. Default border is cobalt.
  Variants: `.badge.on-sky` (coral border), `.badge.on-blush` (dusty-blue
  border) — note the class names are legacy from an earlier palette
  iteration and no longer describe their own colors; that's expected, don't
  "fix" them by renaming without being asked, since it'd touch markup across
  the file.
- **Rotated color blocks** (`.block.sky/.ink/.blush/.coral/.navy`): inline
  headline treatment used for the hero. Each has a slight `rotate()`
  transform and a solid background from the palette above (again, class
  names like `.sky`/`.blush`/`.navy` are legacy tokens — `.sky` now renders
  periwinkle, `.blush` renders pink, `.navy` renders cobalt). This is the
  single boldest design move on the page — per the design brief, "spend
  boldness in one place" — don't add more rotated blocks elsewhere without
  a reason.
- **Flower doodle SVG** (`.flower`): a small 6-petal asterisk/flower shape,
  colored via `currentColor` → `var(--coral)`, scattered as a decorative
  accent near the hero, About photo, and contact section. Reused inline as
  raw `<svg>` in a few places — not a sprite/icon system.
- **Services heading**: uses the same shared `.section-head h2` pattern as
  every other section (Work, Pricing, About) — just "Services", no
  paragraph underneath. An earlier iteration had a one-off pink blob badge
  overlapping the headline, sourced from a client reference image; that was
  removed in favor of consistency with the rest of the page. Don't
  reintroduce a one-off heading treatment here without being asked.
- **Service rows** (`#services .service-row`): a circular pink-and-cobalt
  arrow icon (`.service-arrow`, literal `→` character) next to a stacked
  bold cobalt name (`.service-name` — Sora 700, title case, not uppercase)
  and a plain-sentence-case tagline in the standard body treatment
  (`.service-desc` — `--body` font, `--ink-soft` color, no
  uppercase/tracking), with a thin cobalt-tinted divider between rows. Only
  the name and arrow icon stay cobalt-branded; the tagline intentionally
  reads like ordinary body copy, not a label — don't put it back in
  `--label`/all-caps/cobalt, and don't re-add `text-transform: uppercase` to
  the name either.
- **Logistics banner** (`.logistics-banner`, between Services and Work): a
  full-bleed cobalt band — the only full-width, non-`--paper`/`--cream`
  section background on the whole site. Small `--label` eyebrow ("Beyond
  the build") in pink, a Sora headline and Roboto Serif body copy in cream/
  light-periwinkle for contrast, and a single `.btn-ghost` (cream border,
  fills cream on hover) opening the same contact modal as everywhere else.
  Exists to mention non-design-build services (site migrations, domain/DNS
  setup, general upkeep) without giving them their own full section or
  pricing line — it's intentionally a short interruption, not a section
  with its own content depth. Don't give it an `id`/nav-anchor entry unless
  asked; it's meant to be encountered while scrolling, not linked to
  directly.

## JS behavior

All JS is inline in a closing `<script>` tag, but **`index.html` and the
`work/*.html` pages don't carry the same script** — don't assume every page
has the same JS available.

- Mobile nav toggle: `#navToggle` toggles `.open` on `#navLinks`. **Present
  on every page**, including all six `work/*.html` pages.
- Modal: `openModal()` / `closeModal()` toggle `.open` on `#modalOverlay`,
  lock/unlock `body` scroll. Closes on overlay click or `Escape`. **Only
  exists on `index.html`** — the modal markup itself lives only there.
  `work/*.html` pages don't have it and their CTAs link to
  `../index.html#contact` instead of calling `openModal()`. If a project
  page ever needs the modal directly, either duplicate the modal markup +
  JS there or turn this into a shared include — don't just call
  `openModal()` from a page that doesn't have the function defined.
- Form submit is **not wired to a backend.** `#projectForm`'s `submit`
  handler calls `preventDefault()` and swaps `#formView` for
  `#thankyouView` — it's a front-end-only "thank you" state. If asked to
  make the form actually send somewhere, this is the handler to replace
  (e.g. point it at Formspree/Basin, or add a `fetch()` call to a backend).
- Google Calendar links currently point at the placeholder
  `https://calendar.google.com/` — replace with Amanda's real booking link
  wherever `calendar.google.com` appears (hero/contact CTA + modal footer).

## Content that is still placeholder

Be aware these are illustrative, not real, and shouldn't be presented to end
users as final without checking with the person running this project:

- All 6 portfolio project names in `#work` (the cards no longer show
  descriptions or platform tags — see Recurring motifs above), and
  everything on their linked `work/*.html` pages — names, taglines,
  platform/category tags, the "brief"/"what we built"/"result" write-ups,
  gallery labels, timelines, and services lists are all invented and should
  be replaced with real project details before this goes live.
- Pricing numbers and package inclusions in `#pricing`.
- The footer email address (`amanda@amandaloftis.io`).
- The Google Calendar booking URL (currently a bare placeholder link).

## Conventions for making changes

- CSS lives in the shared `styles.css`, not per-page — see "Tech stack"
  above. New pages should `<link>` to it (with the correct relative path
  for their folder depth), not copy styles inline. JS is still fine inline
  per-page at this project's size.
- New images go in `images/` and are referenced with a relative path
  (`images/whatever.png`, or `../images/whatever.png` from inside `work/`),
  matching the existing `logo.png`/`avatar.png` pattern — don't inline new
  images as base64 unless asked.
- Reuse existing CSS custom properties for color; reuse `.badge`, `.block`,
  `.flower`, `.btn-primary` / `.btn-ghost` classes for anything that matches
  those patterns rather than writing new one-off styles.
- New pages under `work/` (or any future subfolder) should follow the
  existing template: same `<head>` font links, same `<header>`/`<footer>`
  markup with paths adjusted for the folder depth, same nav-toggle script.
  Don't invent a different page shell per project.
- Mobile breakpoints are handled with two `@media` queries (`900px` and
  `680px`) near the bottom of `styles.css` — check both when changing
  layout-affecting CSS, including on the `work/*.html` pages, which share
  the same breakpoints.
- Respect the existing `prefers-reduced-motion` block; don't add new
  animations without accounting for it there.

## Design inspiration references

A reference board of 4 outside design examples, kept here for future design
decisions — palettes, type pairings, and layout motifs to pull from when
asked to design something new or push an existing design further. **None of
this is a spec for the current site** — it's a menu, not a directive.

### 1. Brand swatch cards (style-guide reference)

A clean brand-guideline format: one card per color, name in bold sans, small
"PRIMARY"/"SECONDARY" tag top-right, hex/RGB/CMYK in a 3-column table below,
on a cobalt-blue backdrop.

Palette shown: Ink `#282828` (primary), Paper `#F9F6EF` (primary), Cobalt
`#193497` (secondary), Pink Eraser `#EDA398` (secondary).

This was the reference that foreshadowed the site's current direction —
its ink/paper/cobalt/dusty-pink combination is structurally what the site's
palette became once cobalt was formalized as primary. The card-with-data-
table format itself is reusable any time a palette or spec needs to be
documented visually rather than just listed.

### 2. Podcast quote card (paper-collage / sticker style)

A single pull-quote treated like a torn notepad sheet, layered over an
offset solid-color backing card, on a dark grainy background. Textured
near-black navy bg (`~#0E141C`), burnt-orange backing sheet + emphasized
quote line (`~#EA632E`), warm cream "paper" card (`~#F9F8ED`), soft lavender
circular badge accent (`~#EFDAFA`).

Notes: bold rounded condensed sans headline with one emphasized line in the
accent color rather than the whole quote; small icon + label eyebrow above
the headline; perforated/torn top edge, drop shadow, slight rotation sell
the paper-cutout feel; circular sticker badges pinned at corners — same
spirit as this site's dotted-pill badges, just solid-fill instead of
outlined.

### 3. Editorial magazine spread

A column-blocked magazine page: oversized serif display type on a rust
panel, a cream sidebar with small-caps labels and body copy, a rotated
vertical index tab, and a blush photo-illustration panel with a periwinkle
pull-quote, plus hand-drawn circle/arrow annotations over the clean type.

Approximate palette: rust `#DE6E43`, cream `#FCF7E3`, dusty blue `#ACC4D8`,
peach blush `#F7E6D7`, muted periwinkle-indigo quote text (~`#5C5FA6`).

Notes: a quirky high-contrast serif for the big display headline — this is
actually closer to the original Fraunces-era look of this site than the
current Sora/Roboto Serif pairing; worth remembering if a future one-off
page wants that more editorial, serif-driven feel. Small tracked all-caps for
section labels; a narrow vertical rotated-text tab as a wayfinding device;
hand-drawn ink marks over otherwise clean typesetting add a human, annotated
feel — cheap to fake with a rough inline SVG stroke.

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
   colors laid on top**, not an all-over saturated palette — matches this
   site's own cream base + cobalt/coral pop strategy.
3. **Small tracked all-caps labels as wayfinding** — same job the site's
   `--label` badges already do here.
4. **Rotation / collage / offset-layering as a structural device** —
   stacked cards, tilted grids, rotated tabs. This site already does this
   with its rotated hero blocks and tilted portfolio cards; these examples
   reinforce that direction rather than suggesting something new.
5. **Circular badge/sticker accents** recur constantly — same family as the
   dotted pill badges and stamp logo already established for this project.

If asked to design something new in this project (a new section, a new
one-off page, a social template, etc.), these are the references to pull
texture and confidence from — while keeping the established cobalt/coral/
cream palette and Sora/Roboto Serif type pairing documented above.
