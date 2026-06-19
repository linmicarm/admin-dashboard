# 🌸 Admin Dashboard

A kawaii / sticker-book admin dashboard — sidebar navigation, a two-row header,
a project card grid, and an announcements / trending rail — built with plain
**HTML + CSS** and laid out almost entirely with **CSS Grid**.

This is The Odin Project's [Admin Dashboard](https://www.theodinproject.com/lessons/node-path-intermediate-html-and-css-admin-dashboard)
capstone for the Grid unit. The assignment is a layout exercise; I treated it as
a chance to ship a *portfolio-quality* piece — real icons, responsive down to
mobile, and accessible — rather than a pixel-match of the reference.

**Live demo:** https://linmicarm.github.io/admin-dashboard/

## Screenshots

**Desktop**

<img width="1911" height="940" alt="image" src="https://github.com/user-attachments/assets/78756889-3061-47cc-8e4e-bb65dc044b85" />

---

## Why I built this

The Odin reference design is a classic admin dashboard, and the point of the
exercise is to lay it out with Grid — top-level page regions placed with one
grid, then nested grids and flexbox inside each region.

I wanted two things beyond "it matches the picture." First, to actually
*understand* the layout decisions rather than copy them — when Grid is the right
tool versus Flexbox, why one track is `1fr` and another is `auto`, and what makes
two columns line up at the bottom. Second, to push it past the assignment's bar:
the brief says responsiveness is optional and uses placeholder content, but a
portfolio piece gets viewed on phones and read by people who care about craft, so
I built it responsive, accessible, and themed in my own visual language instead
of the default Roboto-and-blue.

The brand is the same "soft-girl engineer" kawaii direction as my other projects
(Bento Tea, the volunteer board): candy-pink primary, rotating pastel card
accents like a sticker book, rounded everything, soft diffuse shadows.

---

## Tech stack & decisions

| Area | Choice | Why |
|------|--------|-----|
| Layout | CSS Grid (top level) | One grid on `body` places all three regions at once via `grid-template-areas`. The sidebar spans both rows; the right column splits into header + main. No nesting at the top level keeps the page skeleton readable in one rule. |
| Components | Flexbox (inside regions) | The sidebar clusters, header rows, and nav items are one-dimensional (a row or a column), which is exactly what Flexbox is for. Grid for the 2D page, Flex for the 1D pieces. |
| Card grid | `grid-template-columns: 1fr 1fr` | Fixed 2-column grid to match the reference. I started with `repeat(auto-fit, minmax(...))` but deliberately switched — see the decisions below. |
| Icons | Inline SVG from [Material Design Icons](https://pictogrammers.com/library/mdi/) | The actual paths the assignment points to, inlined so they inherit `fill` from CSS (`currentColor`-style theming) with no extra requests or icon-font baggage. |
| Type | Fredoka (display) + Nunito (body), via Google Fonts | Rounded, soft faces that carry the kawaii brand. Not the default Roboto. |
| Theming | CSS custom properties | Every color, shadow, and radius is a `--token` in `:root`, so the whole palette is one block to change. Same approach that made Bento Tea cheap to restyle. |

### Design rationale

- **`auto` vs `1fr` for the body rows.** The header row is `auto` (size to its
  content — the search bar and greeting define its height) and the main row is
  `1fr` (claim all leftover height). With `min-height: 100vh` on the body, this
  is what makes the main area stretch to the bottom of the screen instead of
  floating with a gap underneath. Flipping them would balloon the header and
  shrink main — wrong shape entirely. Rule of thumb: `auto` for "size to
  content," `1fr` for "claim the leftover."
- **Fixed 2-column card grid over `auto-fit`.** `auto-fit, minmax(220px, 1fr)`
  reflows the column count to fit available width — great for a fluid dashboard,
  but it can't guarantee the stable 2×3 layout the reference wants. I chose the
  fixed grid on purpose and documented the tradeoff (it won't drop to 1 column on
  its own — that's handled with a media query instead).
- **Equal-height columns, used twice.** Both the project grid and the right rail
  fill the full height of the main grid row and distribute their internal space,
  so their bottoms line up. Details in "Problems" below — this was the most
  instructive layout puzzle in the build.
- **Spend boldness in one place.** The signature is the rotating pastel card
  accents (a sticker-book motif). Everything around it — spacing, type, the
  neutral surfaces — stays quiet so the one bold idea reads clearly.

---

## Accessibility

Treated as a first-class requirement, not an afterthought:

- **Real focus states** — visible `:focus-visible` outlines in a high-contrast
  plum, so keyboard users can always see where they are against the pink.
- **Contrast** — the muted text and primary pink were both darkened from my first
  pass to pass WCAG AA on white. (Pretty pastels fail contrast easily; this was
  a real revision, not a freebie.)
- **Labels on icon-only controls** — every bell / star / eye / share button has
  an `aria-label`; decorative SVGs and avatar blobs are `aria-hidden`.
- **Landmarks & headings** — `<aside>` / `<header>` / `<main>` / `<nav>`, each
  section linked to its heading with `aria-labelledby`, plus a skip-to-content
  link.
- **Collapsed sidebar stays readable to screen readers** — when the sidebar
  shrinks to icons-only on tablet, the text labels are hidden with the `sr-only`
  clip technique, **not** `display: none`, so assistive tech still announces
  "Home, Profile, Messages…".
- **Reduced motion** — all transitions are disabled under
  `prefers-reduced-motion: reduce`.

---

## Responsive behavior

The assignment says responsiveness is optional; I built it because a portfolio
piece gets opened on phones. Three breakpoints:

| Width | What changes |
|-------|--------------|
| > 1024px | Full layout: sidebar + header + (projects \| rail). |
| ≤ 1024px | Right rail drops below the projects and its two cards sit side-by-side as a row. |
| ≤ 768px | Sidebar collapses to an icon-only rail (labels kept for screen readers). |
| ≤ 560px | Everything stacks; sidebar becomes a horizontal top bar; cards go 1-per-row; action buttons stretch full width. |

---

## Run locally

No build step — it's static HTML/CSS.

```bash
# clone, then just open the file
open index.html        # macOS
# or: xdg-open index.html  (Linux) / start index.html (Windows)
```

Or serve it with any static server (e.g. `npx serve .`) if you want clean URLs.

---

## Project structure

```
index.html     all four regions; SVG icons inlined; a11y attributes
styles.css     design tokens + body grid + per-region styles + media queries
README.md
```

---

## Problems I ran into (and how I solved them)

- **The card grid wouldn't show 3 columns even after I lowered the `minmax`
  floor.** Back when it was `auto-fit, minmax(220px, 1fr)`, I expected a third
  column to appear on a 1200px screen and it stayed at two. The floor wasn't the
  constraint — the *available width* was. The projects column isn't 1200px wide;
  the sidebar (250px), the rail (280px), and all the gaps/padding eat into it,
  leaving roughly 560px for projects, which only fits two 220px cards plus a gap.
  `auto-fit` was doing its job correctly against the real space. Lesson: with
  `auto-fit minmax`, reason about the *container's* width, not the viewport's —
  "works at this screen size" tells you nothing until you subtract the siblings.
- **Project cards and the rail didn't line up at the bottom.** Both columns are
  grid items in the same `.main-content` row, so they were already the *same
  height* — but their content sat at the top, leaving the shorter column with
  dead space below. The fix isn't forcing heights; it's letting the content fill
  the height that's already there. I made `.projects` a flex column with the card
  grid set to `flex: 1` and explicit `repeat(3, 1fr)` rows, so the three rows
  split the full column height evenly. Lesson: equal *height* and aligned
  *bottoms* are different things — grid gives you the first for free, but you
  still have to tell the content to use it.
- **First attempt at aligning the rail left an ugly gap.** I aligned the rail's
  bottom with `justify-content: space-between`, which pinned announcements to the
  top and trending to the bottom — bottoms aligned, but with a big empty void in
  the middle. `space-between` distributes *leftover space between items*; what I
  actually wanted was the cards to *grow* and absorb that space. Fix: `flex: 1`
  on each rail section (and its card) so they stretch to fill instead. Lesson:
  `space-between` puts the gap between things; `flex-grow` feeds the space into
  the things — pick based on whether you want bigger gaps or bigger boxes.
- **The search bar sat small with a gap instead of stretching.** The header rows
  use `justify-content: space-between`, which left the search input at its
  natural width with empty space beside it. Adding `flex: 1` to the search made
  it grow into that space. It's a quiet illustration of a rule: `flex-grow`
  consumes free space *before* `justify-content` can distribute it, so once the
  input grows there's nothing left for `space-between` to act on.

---

## What I learned

- **Grid for 2D, Flexbox for 1D.** The whole project is a clean illustration:
  one grid for the page (rows *and* columns at once), Flexbox for every
  one-directional piece inside it. Reaching for the wrong one is where layouts
  start fighting back.
- **`auto` and `1fr` are answers to different questions** — "be as big as your
  content" vs "take everything that's left." Almost every layout has exactly one
  greedy `1fr` region and the rest sized to content.
- **Equal height ≠ aligned content.** Grid columns in the same row are already
  equal height; making their *contents* reach the bottom is a second, separate
  step. I used the same fill-and-distribute pattern in two places once I saw it.
- **`space-between` vs `flex-grow` is a recurring fork.** Three different
  problems in this build (rail gap, search width, header rows) came down to the
  same question: do I want the leftover space *between* items or *inside* them?
- **Pretty pastels fail contrast.** My first palette looked right and didn't pass
  WCAG AA. Accessibility isn't a checkbox you add at the end — it constrains the
  design choices themselves.

---

## Optimizations / what I'd do next time

- **A sprinkle of interactivity.** Everything is static. A working search filter
  over the project cards, or an "add project" that injects a card, would show the
  layout *doing* something — a natural next step now that the structure is solid.
- **Move the font `@import` to a `<link>` in `<head>`.** A linked stylesheet with
  `preconnect` loads fonts a touch faster than `@import` inside CSS.
- **Sidebar toggle on mobile.** The phone layout turns the sidebar into a top
  bar, which works, but a proper hamburger drawer would feel more like a real
  app.
- **Subgrid for the cards.** If I wanted every card's title / body / actions row
  to align across the grid (not just within each card), `subgrid` is the clean
  way — worth trying once browser support is a non-issue.

---

## Credits

Icons from [Material Design Icons](https://pictogrammers.com/library/mdi/)
(Pictogrammers, Apache 2.0). Fonts (Fredoka, Nunito) from
[Google Fonts](https://fonts.google.com), Open Font License.
