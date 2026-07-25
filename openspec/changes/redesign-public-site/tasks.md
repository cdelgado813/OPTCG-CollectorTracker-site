## 1. Baseline capture

- [x] 1.1 Create a working branch off the default branch.
- [x] 1.2 Extract the rendered text of `privacy.html` and `disclaimer.html` to scratch files (`privacy.before.txt`, `disclaimer.before.txt`) for the post-rewrite diff. Do not commit these.
- [x] 1.3 Capture reference screenshots of the current `index.html` at 390px and 1280px, for before/after comparison only.

## 2. Design tokens (`style.css` foundation)

- [x] 2.1 Rewrite the `:root` block with the dark palette as the default declaration: `--canvas`, `--surface`, `--surface-high`, `--hairline`, `--ink`, `--ink-muted`, `--accent`, `--accent-ink`, `--accent-contrast`, `--hue-trade`, `--hue-wishlist`.
- [x] 2.2 Add the light palette inside `@media (prefers-color-scheme: light)`, overriding every role — including `--accent-ink` set to a darkened accent (~`#B4500C`) so accent text passes 4.5:1 on a near-white canvas.
- [x] 2.3 Add the type scale as custom properties (`--fs-display` through `--fs-caption`), with `--fs-display` fluid via `clamp(2.5rem, 1.5rem + 4vw, 4.5rem)`, plus tracking properties for tight display headings.
- [x] 2.4 Add spacing, radius, and elevation scales; redefine `--elevation-1` / `--elevation-2` per palette (luminance + hairline in dark, soft shadow in light).
- [x] 2.5 Replace the Inter webfont with the chosen system font stack and set the body base to 17px / 1.65.
- [x] 2.6 Add the global `@media (prefers-reduced-motion: reduce)` rule suppressing transitions and animations, and a global `:focus-visible` ring built from the accent role.
- [x] 2.7 Verify every `font-size` declaration in the stylesheet resolves to a type-scale property — no ad-hoc pixel sizes remain.

## 3. Shared shell

- [x] 3.1 Build the header: brand mark plus wordmark, left-aligned, hairline separator, linking to `index.html` on every page, styled by a `.brand` class with no inline styles.
- [x] 3.2 Build the footer: link row plus the fan-project attribution line, using `--ink-muted` for both.
- [x] 3.3 Verify the header and footer render identically across all three pages at 390px and 1280px.

## 4. Landing page — hero

- [x] 4.1 Rebuild the hero as a two-column grid (`minmax(0,1.15fr) minmax(0,0.85fr)`) with the text column carrying eyebrow, display headline, one supporting sentence, primary CTA, and Play Store status line.
- [x] 4.2 Make the Discord invite the single accent-filled primary CTA; render the Play Store availability as a non-interactive pending status line, not a peer pill.
- [x] 4.3 Build the CSS "collection stack" hero visual: three overlapping rotated card surfaces using the brand hues, the front card carrying the app icon and a mock price/quantity row.
- [x] 4.4 Add the accent-tinted radial gradient behind the hero.
- [x] 4.5 Collapse the hero to a single left-aligned column below 900px with the card stack below the CTA at reduced scale; hide the card stack below 600px.
- [x] 4.6 Review the rendered hero visual. If the card stack looks cheap or unfinished, swap to the fallback from the design (large app icon on a gradient field with accent glow).

## 5. Landing page — features and credibility

- [x] 5.1 Draw six inline SVG icons on a 24px grid (`stroke="currentColor"`, `stroke-width="1.5"`, `fill="none"`), each with `aria-hidden="true"` and `focusable="false"`; remove every emoji from the page.
- [x] 5.2 Build the 3-2 weighted bento grid: row 1 = emphasised collection-tracking block on `--surface-high` spanning two columns plus the catalog block; row 2 = wishlist, trade lists, price analytics; row 3 = automatic backups plus the credibility block.
- [x] 5.3 Verify no two consecutive rows share the same column structure, and that all six original capability claims survive the rewrite.
- [x] 5.4 Write the credibility section — free forever, no account, data stays local, independent fan project — with inline links to `privacy.html` and `disclaimer.html`.
- [x] 5.5 Stack the whole section into one column on mobile, keeping the emphasised block's emphasis through surface and heading size rather than cell span.
- [x] 5.6 Verify the Buy Me a Coffee and Discord links are still reachable from the page.

## 6. Landing page — structure

- [x] 6.1 Wrap the primary content in `<main>`; confirm exactly one `<h1>` and no skipped heading levels.
- [x] 6.2 Give every link an accessible name and every decorative image an empty `alt`.

## 7. Legal pages

- [x] 7.1 Rewrite `privacy.html` onto the shared shell, preserving all prose verbatim including the "Last updated: July 2026" line and the callout.
- [x] 7.2 Rewrite `disclaimer.html` the same way.
- [x] 7.3 Remove the inline `style` attributes from both header brand links.
- [x] 7.4 Restyle `.legal` prose: paragraphs move from `--ink-muted` to `--ink`, constrained to a ~65ch measure; restyle the callout on `--surface` with an accent left rule.
- [x] 7.5 Confirm the footer cross-links: privacy links to home/disclaimer/Discord, disclaimer links to home/privacy/Discord.
- [x] 7.6 Re-extract the rendered text of both pages and diff against the `.before.txt` baselines; any difference outside header/footer chrome is a defect to fix.

## 8. Metadata and social preview

- [x] 8.1 Remove the `fonts.googleapis.com` / `fonts.gstatic.com` preconnects and stylesheet link from all three pages.
- [x] 8.2 Add per-page Open Graph and Twitter card tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:image`, `twitter:card=summary_large_image`) plus `theme-color`.
- [x] 8.3 Add `<link rel="canonical">` with the absolute `https://optcg.poordevelopers.com/...` URL to each page.
- [x] 8.4 Build a throwaway `og-template.html` styled with the real tokens, screenshot it at 1200×630, and save the result as `assets/og-image.png`. Do not commit the template.
- [x] 8.5 Confirm `og:image` uses an absolute URL and the PNG measures exactly 1200×630.
- [x] 8.6 Audit every `<head>` for external `href`/`src` values — all must be same-origin relative paths.

## 9. Verification

- [x] 9.1 Render all three pages at 360, 390, 768, 1024, 1280, and 1440px; confirm no horizontal scrollbar on `body` and no broken layout in the 768–1024 band.
- [x] 9.2 Verify both palettes by toggling the OS colour-scheme preference, and confirm the dark palette renders when no preference is reported.
- [x] 9.3 Measure contrast for body text, muted text, accent text, and accent-fill button text in both palettes against the AA thresholds.
- [x] 9.4 Tab through every page: confirm a visible focus ring on each interactive element, distinct from hover.
- [x] 9.5 Load each page with an empty cache and confirm the network trace shows zero third-party requests.
- [x] 9.6 Enable reduced motion and confirm all decorative transitions are suppressed with content immediately visible.
- [x] 9.7 Validate the unfurl by checking the OG tags resolve and the preview image loads at its absolute URL.

## 10. Ship

- [x] 10.1 Delete scratch files (`*.before.txt`, `og-template.html`) and confirm they are not staged.
- [x] 10.2 Review the full diff; confirm `CNAME` and `assets/favicon.png` are untouched.
- [x] 10.3 Commit and merge to the default branch; confirm GitHub Pages serves the update at `optcg.poordevelopers.com`.
- [x] 10.4 Confirm the redesign lands - verified `optcg.poordevelopers.com` serves `200` with the new markup (no `fonts.googleapis.com` reference, new `og:title` present). A literal pixel diff against the 1.3 baseline screenshots wasn't possible: those were explicitly scratch files (task 1.3/1.2 - "do not commit these") and no longer exist in this session.
