## ADDED Requirements

### Requirement: Dark-first colour system
The site SHALL define its colour roles as CSS custom properties on `:root`, with the dark palette as the default declaration and the light palette applied only inside a `prefers-color-scheme: light` media query. Colour roles SHALL be semantic (surface, elevated surface, text, muted text, hairline, accent, accent-contrast) rather than literal colour names, and the brand accent SHALL remain `#FF8A3D`.

#### Scenario: Visitor with no colour-scheme preference
- **WHEN** a visitor loads any page in a browser reporting no `prefers-color-scheme` preference
- **THEN** the page renders in the dark palette, because dark is the default declaration rather than a media-query override

#### Scenario: Visitor preferring light
- **WHEN** a visitor's OS or browser reports `prefers-color-scheme: light`
- **THEN** every colour role resolves to its light-palette value and no element retains a hardcoded dark colour

#### Scenario: Accent is reserved
- **WHEN** a page is inspected for uses of the accent colour role
- **THEN** the accent is applied only to the primary call to action, active/hover affordances, and at most one editorial emphasis per section — never as a background fill for large surfaces

### Requirement: Contrast accessibility
All text SHALL meet WCAG 2.1 AA contrast against its own background in both the dark and light palettes: at least 4.5:1 for body text and text under 24px, and at least 3:1 for text at or above 24px and for non-text UI boundaries that convey state.

#### Scenario: Muted text on elevated surface
- **WHEN** muted/secondary text is rendered on an elevated surface in either palette
- **THEN** its measured contrast ratio is at least 4.5:1

#### Scenario: Accent text on the page canvas
- **WHEN** the accent colour is used for text or an interactive label on the page canvas in either palette
- **THEN** its measured contrast ratio is at least 4.5:1, using a palette-specific accent tint if the base accent fails

### Requirement: Type scale
The site SHALL define a modular type scale as custom properties covering, at minimum, display, section heading, sub-heading, body, and caption sizes, and every text element SHALL draw its size from that scale rather than a one-off pixel value. The display size SHALL be fluid, interpolating between a mobile and a desktop bound via `clamp()`.

#### Scenario: Narrow viewport
- **WHEN** the page is rendered at 360px wide
- **THEN** the display headline uses the lower bound of its `clamp()` range, wraps without overflowing the viewport, and produces no horizontal scrollbar on `body`

#### Scenario: Wide viewport
- **WHEN** the page is rendered at 1440px wide
- **THEN** the display headline uses the upper bound of its `clamp()` range and sits visibly larger than section headings, establishing typographic hierarchy

#### Scenario: No ad-hoc sizes
- **WHEN** the stylesheet is inspected for `font-size` declarations
- **THEN** every declaration resolves to a type-scale custom property

### Requirement: Spacing, radius, and elevation scales
The site SHALL define spacing, border-radius, and elevation (shadow) values as custom-property scales, and layout rules SHALL compose from those scales. Elevation SHALL be expressed as layered shadows tuned per palette, so that dark-palette surfaces separate by luminance and hairline rather than by a shadow that is invisible on a dark canvas.

#### Scenario: Elevated surface in dark palette
- **WHEN** an elevated surface renders in the dark palette
- **THEN** it is distinguishable from the page canvas by a lighter surface colour and a hairline border, not by drop shadow alone

#### Scenario: Elevated surface in light palette
- **WHEN** the same surface renders in the light palette
- **THEN** it is distinguishable by a soft drop shadow from the elevation scale

### Requirement: Self-hosted typography with no third-party requests
The site SHALL render its typography without issuing any network request to a third-party origin. Fonts SHALL be either self-hosted files under `assets/fonts/` served from the site's own origin, or a pure system font stack.

#### Scenario: Page load network trace
- **WHEN** any page of the site is loaded with an empty cache and network activity is recorded
- **THEN** no request is made to `fonts.googleapis.com`, `fonts.gstatic.com`, or any other external host

#### Scenario: Self-hosted webfont is used
- **WHEN** a self-hosted webfont is chosen over a system stack
- **THEN** it is loaded as a `woff2` subset with `font-display: swap` and a matching system fallback declared in the same `font-family` stack

### Requirement: Motion respects reduced-motion preference
Any transition, transform, or entrance animation SHALL be disabled or reduced to an instantaneous state change when the visitor requests reduced motion.

#### Scenario: Reduced motion requested
- **WHEN** the visitor's system reports `prefers-reduced-motion: reduce`
- **THEN** all decorative transitions and entrance animations are suppressed and content is immediately visible in its final state

### Requirement: Visible focus affordance
Every interactive element SHALL expose a visible keyboard focus indicator that is distinct from its hover state and meets the 3:1 non-text contrast threshold against adjacent colours.

#### Scenario: Keyboard traversal
- **WHEN** a visitor tabs through a page
- **THEN** each focused link and button shows a clearly visible focus ring, and no element removes the outline without providing a replacement indicator
