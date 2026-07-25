## Why

The public site at `optcg.poordevelopers.com` currently reads as a generic, machine-generated landing page: Inter + centred hero + a six-cell grid of emoji-headed cards on a near-white background. That template is instantly recognisable and undercuts the credibility of the app it is selling — a collector landing on it has no reason to believe a real person built and maintains the tracker. The site is also the only shop window while the app is still "coming soon" on Google Play, so first impression is the whole conversion funnel.

## What Changes

- Replace the visual system with a **dark-first editorial direction**: dark canvas as the default (not just a `prefers-color-scheme` afterthought), large display typography with real typographic contrast, the existing brand accent (`#FF8A3D`) used sparingly as a true accent, and subtle layered depth instead of flat bordered cards.
- Rebuild the hero as an asymmetric editorial layout (headline block + supporting visual column) rather than a centred stack, with a clear primary call to action and the Google Play "coming soon" state expressed as a status line rather than a pill among equals.
- Replace the uniform 6-card emoji grid with a differentiated feature presentation: a small number of weighted, non-uniform blocks with custom inline SVG iconography, so no two rows read the same.
- Introduce a proper type scale, spacing scale, and elevation model as CSS custom properties, replacing the current ad-hoc pixel values.
- Drop the Google Fonts network dependency in favour of a self-hosted or system font stack, so the page renders with no third-party request and no font-swap flash.
- Apply the same visual system to `privacy.html` and `disclaimer.html` so the whole site is coherent, including a shared header/footer treatment and a readable legal-prose measure.
- Add the meta layer the current site lacks: Open Graph / Twitter card tags and a generated social preview image, so shared links look intentional.
- **No new build step.** The site stays hand-written static HTML + CSS deployed by GitHub Pages.

## Capabilities

### New Capabilities
- `visual-design-system`: The site-wide design tokens and primitives — colour roles, type scale, spacing scale, radii, elevation, motion, and the dark/light behaviour — that every page composes from.
- `landing-page`: The structure, content blocks, and behaviour of `index.html` — hero, feature presentation, trust/credibility section, and calls to action.
- `legal-pages`: The shared shell and prose treatment for `privacy.html` and `disclaimer.html`.
- `site-metadata`: Document head concerns shared by every page — title/description, favicon, social preview tags, and font loading strategy.

### Modified Capabilities
<!-- None. `openspec/specs/` is empty; this is the first change in the site repo. -->

## Impact

- **Files rewritten**: `style.css` (full rewrite), `index.html` (full rewrite), `privacy.html` and `disclaimer.html` (markup shell and class names updated; legal copy preserved verbatim).
- **Assets added**: `assets/og-image.png` (social preview), self-hosted font files under `assets/fonts/` if a webfont is chosen over a system stack.
- **Dependencies removed**: `fonts.googleapis.com` / `fonts.gstatic.com` preconnects and stylesheet link.
- **Unchanged**: `CNAME`, GitHub Pages deployment, all legal and disclaimer wording, all outbound links (Discord, Buy Me a Coffee), `assets/favicon.png`.
- **Risk**: legal copy must survive the markup rewrite untouched — the disclaimer and privacy text exist for store-compliance reasons, not decoration.
