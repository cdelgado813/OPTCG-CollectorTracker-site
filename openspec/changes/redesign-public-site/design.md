## Context

The site is four hand-written files (`index.html`, `privacy.html`, `disclaimer.html`, `style.css`) plus a favicon, served by GitHub Pages at `optcg.poordevelopers.com` via `CNAME`. There is no build step, no package manager, no CI. That is a feature, not a gap — the maintainer is one person and the site changes rarely.

The current design fails for a specific, diagnosable reason rather than a vague "it looks bad" one. It exhibits the full set of generic-landing-page tells:

1. Inter at weight 800 for every heading, with almost no size contrast between the `h1` (34px) and `h2` (24px).
2. A perfectly centred hero: icon, headline, sentence, pill row — a vertical stack with no compositional tension.
3. Six identically sized cards in an `auto-fit` grid, each led by an emoji.
4. A near-white canvas (`#FAFAFC`) with `#FFFFFF` cards, 1px hairlines, 18px radii, and one soft shadow — the default component-library look.
5. Dark mode bolted on as a `prefers-color-scheme` override of six variables, which produces a serviceable but characterless dark theme.

The redesign direction chosen with the maintainer is **dark-first editorial**: dark canvas as the primary identity, strong typographic hierarchy, the existing `#FF8A3D` accent used sparingly, and depth built from surface luminance rather than shadow. The scope is the landing page plus the two legal pages.

One material constraint shapes the hero: **there are no app screenshots in this repository.** `assets/` contains only `favicon.png`. Sourcing, framing, and optimising real screenshots was explicitly excluded from this change's scope, so the hero's visual column must be something the redesign can build from CSS and inline SVG alone.

## Goals / Non-Goals

**Goals:**

- Produce a site that reads as made by a person with taste, not assembled from a component library.
- Keep the whole thing static, dependency-free, and editable by hand in a text editor.
- Eliminate the third-party font request, so the site has zero external network dependencies.
- Meet WCAG 2.1 AA contrast and keyboard-accessibility baselines in both palettes.
- Keep the legal prose byte-for-byte intact through the markup rewrite.
- Make shared links unfurl properly, which the site currently cannot do at all.

**Non-Goals:**

- No build tooling, framework, bundler, or CSS preprocessor.
- No app screenshots or device mockups — deliberately deferred; the hero must work without them.
- No JavaScript for layout or content. A manual theme toggle is out of scope; the system preference governs.
- No copy rewrite of the legal pages, and no change to the six capability claims' substance.
- No analytics, no cookie banner, no consent tooling — the site collects nothing and will keep collecting nothing.
- No new pages beyond the existing three.

## Decisions

### 1. Dark as the default declaration, light as the media-query override

The current stylesheet declares light on `:root` and overrides in `@media (prefers-color-scheme: dark)`. That is inverted for this direction. Dark becomes the base declaration; light lives in `@media (prefers-color-scheme: light)`.

This matters beyond aesthetics: browsers that report no preference, and unfurl/screenshot bots, will render the dark identity — which is the one the brand is being built around.

*Alternative considered:* keep light as base and add a `data-theme` toggle. Rejected — a toggle needs JavaScript and localStorage, which contradicts the zero-dependency goal for a marketing page with three routes.

### 2. Semantic colour roles, and a second accent tint for text

Replace the current literal-ish variables with semantic roles:

```
--canvas          page background
--surface         raised block
--surface-high    the emphasised feature block
--hairline        1px separators
--ink             primary text
--ink-muted       secondary text
--accent          #FF8A3D — fills, borders, marks
--accent-ink      accent used as *text* colour
--accent-contrast text placed on top of an accent fill
```

The `--accent-ink` split exists because `#FF8A3D` on a dark canvas passes 4.5:1 comfortably, but on the light palette's near-white canvas it does **not** — it lands around 2.4:1. So the light palette sets `--accent-ink` to a darkened variant (roughly `#B4500C`) while `--accent` stays `#FF8A3D` for fills. Any implementation that uses a single accent variable for both fills and text will fail the contrast requirement in one palette or the other.

The existing app-derived `--trade` (`#0E9F6E`) and `--wishlist` (`#E91E63`) hues are retained as `--hue-trade` / `--hue-wishlist` and used only as small feature-icon tints, so the site still echoes the app's colour language without turning into a rainbow.

### 3. Depth from luminance, not shadow

On a dark canvas, `0 4px 20px rgba(15,42,74,0.08)` is invisible. Elevation is therefore expressed differently per palette: in dark, a surface separates from the canvas by being lighter plus a hairline; in light, by a soft shadow plus a hairline. Both are driven by the same `--elevation-1` / `--elevation-2` custom properties, redefined inside the light media query.

A single low-opacity radial gradient anchored behind the hero, tinted with the accent, supplies atmosphere without imagery. It is pure CSS and costs nothing.

### 4. System font stack over a self-hosted webfont

Two options satisfy "no third-party request": self-host a `woff2` subset, or use a system stack.

Chosen: a **system stack**, but not the generic one — a deliberate pairing that produces editorial contrast:

- Display/headings: a tightened, heavy system-UI stack with negative letter-spacing at large sizes, which is what actually creates the "designed" feel — size jump and tracking, not the typeface itself.
- Body: the same stack at normal tracking and a slightly larger base size (17px) with `line-height: 1.65`.

The typographic identity comes from a genuine scale jump (display ≈ 3.5× body, versus the current 1.9×) and tight display tracking. A webfont would add per-file maintenance and a FOUT for a marginal gain on a three-page site.

*Alternative considered:* self-hosting a distinctive display face (e.g. a grotesk with real character) for headings only, ~25KB subset. This is the stronger long-term option if the site grows, and is recorded as an open question rather than being decided against on the merits. The task list is structured so swapping the display stack later touches one custom property.

### 5. Fluid type scale via `clamp()`

Every size is a custom property; the display size is `clamp(2.5rem, 1.5rem + 4vw, 4.5rem)`. This removes the need for typographic breakpoints entirely — the only media queries in the stylesheet are for layout structure and colour scheme.

### 6. Hero: asymmetric two-column with a CSS-built visual

Desktop layout is a `grid-template-columns: minmax(0, 1.15fr) minmax(0, 0.85fr)`. The left column holds an eyebrow, the display headline, one supporting sentence, the primary CTA, and the Play Store status line. The right column holds the hero visual.

Since screenshots are out of scope, the hero visual is a **CSS-composed "collection stack"**: three overlapping card-shaped surfaces at slight rotations, built from divs with the app's brand hues, the largest carrying the app icon and a small mock price/quantity row rendered in text and inline SVG. This reads as the product's subject matter (cards) without claiming to be a screenshot, and it is the single element most responsible for the page not looking templated.

Below 900px the grid collapses to one column, text stays left-aligned, and the card stack moves below the CTA at reduced scale. Below 600px the card stack is hidden — on a phone, the CTA should be above the fold, not pushed down by decoration.

*Alternative considered:* leave the visual column empty and let the headline run wide. Rejected — an asymmetric layout with nothing in the second column just looks like a broken centred layout.

### 7. Feature section: a 3-2 weighted bento, not a 6-cell uniform grid

The six features become a deliberately uneven arrangement:

- **Row 1** — one wide emphasised block (collection tracking, the app's core value) spanning two columns on `--surface-high`, alongside one standard block (full catalog).
- **Row 2** — three standard blocks (wishlist, trade lists, price analytics).
- **Row 3** — one wide block for automatic backups, paired with the credibility statement.

The rule the spec encodes is that no two consecutive rows share a column structure. On mobile everything stacks; the emphasised block keeps its emphasis through its surface and heading size rather than through span.

Emoji are replaced by inline SVG icons drawn on a 24px grid with `stroke="currentColor"`, `stroke-width="1.5"`, `fill="none"` — six of them, hand-written, `aria-hidden="true"`. Inline rather than a sprite sheet or icon font because six icons across one page is not worth a second request or a `<use>` indirection.

### 8. Legal pages share the landing shell; prose gets its own measure

Header and footer markup is copied verbatim across all three pages (there is no templating engine and introducing one is out of scope). The three inline `style` attributes currently on the legal header links are removed and replaced by a `.brand` class.

Legal prose moves from `--ink-muted` to `--ink` — the current pages set every paragraph in secondary grey, which is the single biggest readability problem on those pages — and is constrained to a `65ch` measure inside the wider container.

### 9. Copy preservation is enforced by extraction, not by eyeballing

Before rewriting the legal pages, the rendered text of each is extracted to a scratch file. After the rewrite, it is extracted again and diffed. The prose exists for Play Store compliance; "I'm fairly sure I didn't change it" is not good enough.

### 10. Social preview image generated from the site's own CSS

`assets/og-image.png` is produced by writing a throwaway `og-template.html` styled with the real tokens, screenshotting it at 1200×630, and saving the PNG. The template file is not committed — only the PNG is. This keeps the preview image visually identical to the site without adding an image pipeline.

## Risks / Trade-offs

- **Legal copy silently altered during the markup rewrite** → Extract-and-diff the rendered text before and after (Decision 9); treat any diff outside header/footer chrome as a defect.
- **Accent fails contrast in the light palette** → Split `--accent` from `--accent-ink` (Decision 2) and verify both palettes against 4.5:1 as an explicit task, not as an assumption.
- **The bento layout collapses badly at intermediate widths (700–1000px)** → Verify at 360, 390, 768, 1024, 1280, and 1440px specifically; the 768–1024 band is where uneven grids usually break.
- **The CSS card stack reads as cheap or unfinished** → It is the highest-risk element in the design. If it does not land, the fallback is a single large app icon on a gradient field with the accent glow, which is safe and still not template-generic. Decide by looking at it, not in advance.
- **A system font stack renders inconsistently across Windows / macOS / Android** → Acceptable: the hierarchy is carried by size and tracking, which survive a typeface substitution. Revisit only if the display stack looks materially wrong on a target platform.
- **Hiding the hero visual below 600px loses the design's signature on the most common viewport** → Mitigated by the accent glow and the type treatment, which persist at every width; the mobile hero must be reviewed on its own terms, not judged as a shrunken desktop hero.
- **No screenshots means the site still cannot show the product** → Accepted for this change and flagged as the obvious next one; the hero visual column is built so a real screenshot can be dropped into it later without relayout.

## Migration Plan

The site is static and served from the default branch by GitHub Pages, so deployment is a push and rollback is `git revert`. Work happens on a branch and is verified locally against `file://` or a trivial static server before merge. `CNAME` is not touched. There is no cache-busting concern beyond GitHub Pages' own CDN, which invalidates on push.

## Open Questions

- **Self-hosted display face?** Decision 4 chose a system stack. If the maintainer wants a stronger typographic identity, a ~25KB `woff2` subset for headings only is a one-property change. Worth revisiting after seeing the system-stack version rendered.
- **Does the Google Play link go live during this change?** The spec assumes the pre-launch state, with Discord as the primary CTA. If the app ships to Play before this lands, the primary CTA becomes the Play badge and the status line is dropped.
- **Should the credibility section carry a screenshot of the app's own privacy screen?** Out of scope here; noted because it would strengthen the section considerably.
