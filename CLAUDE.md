# CLAUDE.md

Guidance for Claude Code when working in this repo.

## Project overview

Personal portfolio site for **Amanda Loftis**, a product design leader
in Greensboro, NC, who specializes in applying cognitive behavioral
science to UI design (Director of UX at Mediavine 2022–2026, Director of
UX and Brand at Fluree 2019–2022, earlier roles at Andmore and Elon
University).

**History.** The repo previously held a mock "Amanda Loftis Design
Studio" marketing site (services, contact form, six invented projects).
In **November 2026** that was replaced:
- **Content** now mirrors Amanda's live Framer site, www.amandaloftis.io:
  same pages, project write-ups, testimonials, resume and images. The
  live site is the source of truth for wording.
- **Design** is a dark "product studio" look modeled on the feel of
  electronicmaterialsoffice.com (see Design system).

There is **no services page, contact form, pricing or booking flow**.
Contact is the `mailto:` email in the nav, the Home closing section and
the footer. Don't reintroduce the old studio pages without being asked.

## Tech stack

- **Plain HTML + CSS + vanilla JS. No framework, no bundler, no package.json.**
- **One shared `styles.css`.** Top-level pages link `styles.css`; pages
  in `projects/` link `../styles.css`. Don't put CSS inline per page or
  fork the stylesheet.
- JS is inline per page in `<script>` blocks (see JS behavior).
- Fonts load from Google Fonts via `<link>` in every `<head>`.
- No build step in the repo: edit a file, refresh the browser.

## File structure

```
index.html                 — Home
projects.html              — all projects, filterable by category
projects/<slug>.html       — 25 project pages (slugs match the live site's /projects/<slug>)
resume.html                — Experience / resume (nav label "Experience")
process.html               — My process
about.html                 — About
amanda-loftis-resume.pdf   — the "Download resume" file (copied from the live site)
styles.css                 — shared stylesheet
images/portfolio/*.webp    — every content image, named by its Framer image id
images/logo.png, avatar.png, Amanda Loftis.png — legacy, unreferenced
```

Page filenames match the live site's URLs (`/about`, `/process`,
`/resume`, `/projects`, `/projects/<slug>`), so hosts that serve
`page.html` at `/page` keep old links working. **If you rename or move a
page, update every link to it**, including nav, drawer, footer, the
prev/next links on project pages, and in-text links inside write-ups.

Images were downloaded from the live site's Framer CDN, scaled to at
most 1600px wide and saved as WebP (quality 80). Every `<img>` has
`width`/`height` attributes and `loading="lazy"`, except the first image
on a page. Add new images the same way.

## Pages

Every page shares the floating pill nav (Projects / Experience / Process
/ About, a LinkedIn icon, and an orange "Get in touch" `mailto:` button),
the mobile drawer, the footer, and the scroll-reveal script.

1. **`index.html` (Home)**: giant serif "Amanda Loftis", the tagline
   statement, category chips linking to `projects.html#<category>`, and a
   glowing `.stage` showing the dashboard project. Then: 6 featured
   project cards, 3 testimonials (Kenrika Mazuch, Matthew Winger (blue
   panel), Matt Grisafi), 4 experience rows linking to `resume.html#<job>`,
   3 "Learn more" cards (Presentations / My process / About me), and a
   closing "Let's talk." with the email.
2. **`projects.html`**: all 25 project cards in the live site's order,
   with All / Case Studies / Workshops / Fonts / Print / Branding filter
   buttons. The filter reads and writes the URL hash
   (`projects.html#fonts`), so other pages can deep-link to a category.
3. **`projects/<slug>.html`**: back link, category chips, serif title,
   intro statement, hero image on a `.stage`, then the write-up as
   `.prose` (h2/h3/p/ul/ol/hr). Images between paragraphs are
   `.prose-media`: one image full width, consecutive images in a
   `.media-grid`, captions from the live site as `<figcaption>`. Ends
   with previous/next project links (wrap-around, in `projects.html`
   order). Links to other projects are relative (`fluree-nexus.html`);
   external links open in a new tab.
4. **`resume.html`**: "Resume." header with jump chips and a Download
   resume button, then one section per job (`#mediavine`, `#fluree`,
   `#andmore`, `#elon`: logo, years, title, company link, description,
   highlight images with captions), `#presentations` (talks + "I've also
   spoken at"), and `#specialty` (4 panels).
5. **`process.html`**: intro, process diagrams, 6 numbered step panels,
   and a blue panel linking to the AI case study.
6. **`about.html`**: intro, 3 photos, "What I do" and "Tools I use" pill
   lists, and a Favorite project panel (Date Night Questions).

**Project categories** (from the live site's category pages) are in each
card's `data-tags` and each project page's chips: `case-studies`,
`workshops`, `fonts`, `print`, `branding`. When adding a project, add
its card to `projects.html` in the right position, update the prev/next
links on it and its two neighbors, and add it to Home's featured grid
only if asked.

## Design system

Dark "product studio" look: near-black canvas, huge light serif display
headlines, wide grotesk "statement" paragraphs (white lead, gray
continuation), tiny monospace labels, dark rounded panels, glowing
image stages, and a floating translucent pill nav.

All tokens are CSS custom properties in `:root` at the top of
`styles.css`. **Use the variables; don't hardcode hex values.** If a new
color is really needed, add a named variable and say so.

### Colors (`:root`)

| Variable | Hex | Role |
|---|---|---|
| `--black` | `#000000` | page background |
| `--surface` | `#111111` | panels, footer, drawer, project-nav cards |
| `--surface-2` | `#1A1A1A` | image backgrounds, pill-list items |
| `--line` | `#2F2F2F` | hairlines, outlines, image borders, nav pill |
| `--gray` | `#6D6D6D` | gray half of statements/captions, neutral labels |
| `--gray-light` | `#CACACA` | body copy, nav/footer links |
| `--white` | `#FFFFFF` | headlines, bold leads |
| `--daredevil` | `#FF5B22` | **actions**: `.btn`, nav dot, focus ring, orange glows |
| `--blue` | `#256AEC` | **active/feature**: active filter, `.panel--blue`, blue glows in `.stage` |
| `--sky` | `#A8EBFF` | **labels/highlights**: `.eyebrow`, panel numbers, years, quote marks, link underline and hover |
| `--daredevil-dim` | `#7C2A0C` | deep orange for gradients |

Keep those roles: orange = do something, blue = selected/featured
surface, sky = label/highlight. Don't swap them around per page.

### Typography

Google Fonts: `Archivo` (with the `wdth` axis 62–125), `Instrument
Serif`, `Space Mono`.

- `--serif` (Instrument Serif, roman): giant display headlines only
  (`.display`, `.display--md`, footer wordmark, drawer links).
- `--sans` (Archivo): everything else. `.statement` (and `--sm`/`--xs`)
  is widened (`font-stretch: 118%`), weight 500, tight negative
  tracking, written as `<b>White lead.</b> gray continuation`.
  `.caption` follows the same pattern at small size. `.h3`, `.btn`,
  `.role__title` and `.prose` headings are also widened.
- `--mono` (Space Mono): small uppercase labels: `.eyebrow`, `.tag`,
  `.chips`, filters, `.back-link`, years, footer bottom line.

### Components (reuse, don't reinvent)

- `.nav` / `.nav-drawer` / `.nav-scrim`: floating pill nav + mobile drawer.
- `.display`, `.statement`, `.caption`, `.eyebrow`, `.h3`, `.body`, `.text-link`.
- `.btn` (orange, auto `↗` arrow; `--sm`, `--ghost`).
- `.tags`/`.tag` (static outlined labels); `.chips` (outlined link pills).
- `.panel` (`--dots`, `--blue`, `--line`) with `.panel__num`.
- `.img` (bordered rounded image); `.thumb` (fixed-ratio cover crop with
  hover zoom); `.stage` (glowing panel presenting one image; `--orange`
  variant; project pages alternate the two).
- `.figure` + `figcaption`; `.work-grid` + `.work-card`; `.filters`.
- `.quotes`/`.quote` (testimonials), `.roles`/`.role` (experience rows),
  `.pill-list` (skills/tools), `.prose` + `.prose-media` + `.media-grid`
  (write-ups), `.project-nav` (prev/next).
- Layout: `.section` (`--tight`), `.stack` (`--center`), `.split` +
  `.split__aside`, `.row-between`, `.grid` (`--2`/`--3`/`--4`).

## JS behavior

Every page has, inline:
- `document.documentElement.classList.add('js')` in `<head>`.
- **Mobile nav drawer** script (IDs `navToggle`, `navDrawer`, `navScrim`,
  `navDrawerClose`): opens/closes on toggle, close button, scrim, link
  click and Escape; locks body scroll. **Nav links live in three places on
  every page (`.nav__links`, `.nav-drawer__links`, footer) — update all
  three on every page.**
- **Scroll reveal**: `.reveal` elements fade up when they enter the
  viewport. Hidden only under `html.js` + `prefers-reduced-motion:
  no-preference`, so content is always visible without JS or with
  reduced motion.

`projects.html` also has the filter script (buttons with `aria-pressed`,
hides `.work-card`s by `data-tags`, syncs `location.hash`).

## Content notes

- Wording, testimonials, dates and images come from www.amandaloftis.io
  as of November 2026. A few short section intros were written for this
  design and aren't on the live site; check with Amanda before treating
  them as final: Home "Featured work", "Kind words", "Experience"
  ("Nearly a decade…") and "Learn more" card captions; the Projects
  page intro; and the Resume "Presentations" intro.
- Alt text: the live site has alt text on only a few images. The rest
  use generic alt text (the project title or caption); improve it when
  real descriptions are available.
- Social links: LinkedIn `linkedin.com/in/amandaloftis`, Instagram
  `instagram.com/amanda.helene.loftis`, Dribbble `dribbble.com/amandaloftis`.

## Conventions for making changes

- CSS goes in `styles.css`. Reuse tokens and the components above
  instead of one-off inline styles.
- New project pages copy an existing `projects/*.html` shell (same
  `<head>`, nav with `aria-current` on Projects, footer, scripts).
- Fully fluid layout (`clamp()`, `auto-fit`/`minmax()`, `flex-wrap`).
  **The only fixed `@media` breakpoint is 680px** (nav links ↔ hamburger).
  Don't add other layout breakpoints. Check changes at ~375px, ~768px
  and ~1440px, and just above/below 680px when touching the nav. Nothing
  should scroll horizontally (`.prose` uses `overflow-wrap: anywhere`
  for long URLs).
- Respect `prefers-reduced-motion` for any new animation.

## Design inspiration references

A reference board of 4 outside design examples, kept here for future design
decisions — palettes, type pairings, and layout motifs to pull from when
asked to design something new or push an existing design further. **None of
this is a spec for the current site** — it's a menu, not a directive. (The
current dark design keeps two of these throughlines, oversized display
type and small tracked labels, but deliberately drops the pastel palette
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
Archivo/Space Mono type pairing and black + orange/blue/sky palette
documented above.
