## ADDED Requirements

### Requirement: Asymmetric editorial hero
The landing hero SHALL use an asymmetric two-column layout on viewports at or above the desktop breakpoint — a left-aligned text column carrying the display headline, supporting sentence, and calls to action, and a companion visual column — collapsing to a single left-aligned column below that breakpoint. The hero SHALL NOT be a centred stack of icon-over-headline-over-pill-row.

#### Scenario: Desktop hero
- **WHEN** the landing page renders at 1280px wide
- **THEN** the headline block is left-aligned in its own column with a distinct visual element occupying the adjacent column

#### Scenario: Mobile hero
- **WHEN** the landing page renders at 390px wide
- **THEN** the hero collapses to one column, text stays left-aligned, and the visual element either stacks below the calls to action or is hidden if it would push them below the fold

### Requirement: Single primary call to action
The hero SHALL present exactly one visually primary call to action. All other hero links SHALL be visually subordinate to it. The Google Play "coming soon" state SHALL be communicated as a status line or disabled-state affordance, not as one pill among a row of equal-weight pills.

#### Scenario: Pre-launch state
- **WHEN** the app is not yet published on Google Play
- **THEN** the primary call to action is the Discord invite, and the Google Play availability is stated as supporting text or a non-interactive badge that reads as pending

#### Scenario: Visual weight audit
- **WHEN** the hero is inspected
- **THEN** no more than one element uses the accent-filled button treatment

### Requirement: Differentiated feature presentation
The feature section SHALL NOT present its items as a uniform grid of identically sized cards. It SHALL give at least one feature greater visual weight than the others, and items SHALL be arranged so that no two consecutive rows share the same column structure.

#### Scenario: Feature section layout
- **WHEN** the feature section renders at 1280px wide
- **THEN** at least one feature occupies a larger cell than the others, and the grid uses more than one column configuration across its rows

#### Scenario: Feature section on mobile
- **WHEN** the feature section renders at 390px wide
- **THEN** all features stack in a single readable column and the emphasised feature retains its emphasis through type or surface treatment rather than through cell size

### Requirement: Custom iconography replaces emoji
Feature headings SHALL NOT use emoji as their icon. Each feature SHALL be marked by an inline SVG icon that inherits its colour from the surrounding text colour and is hidden from assistive technology.

#### Scenario: Feature icon markup
- **WHEN** the landing page markup is inspected
- **THEN** each feature's icon is an inline `<svg>` carrying `aria-hidden="true"` and `focusable="false"`, and no emoji character appears in a feature heading

#### Scenario: Icon in light palette
- **WHEN** the page renders in the light palette
- **THEN** every icon's stroke or fill resolves through `currentColor` and remains visible against its surface

### Requirement: Credibility section
The landing page SHALL include a section that establishes the project's honesty claims — free forever, no account, no tracking of collection contents, independent fan project — and links to the privacy policy and disclaimer from within that context rather than only from the footer.

#### Scenario: Visitor evaluating trust
- **WHEN** a visitor scrolls past the feature section
- **THEN** they reach a section stating the no-account / no-cost / local-data position with inline links to `privacy.html` and `disclaimer.html`

### Requirement: Content preservation
The redesign SHALL preserve every capability claim and outbound link present on the current landing page: full catalog with CardMarket prices, collection tracking with playsets and duplicates, wishlist, shareable trade lists, price analytics over time, automatic backups with export/import, the Discord invite, and the Buy Me a Coffee support link.

#### Scenario: Claim audit
- **WHEN** the redesigned landing page is compared against the previous version
- **THEN** all six capability claims are still present, though their wording and grouping may change

#### Scenario: Link audit
- **WHEN** the redesigned landing page's outbound links are enumerated
- **THEN** the Discord invite, the Buy Me a Coffee link, `privacy.html`, and `disclaimer.html` are all reachable

### Requirement: Semantic document structure
The landing page SHALL use one `<h1>`, section headings in non-skipping order, landmark elements (`header`, `main`, `section`, `footer`), and an accessible name on every link. Decorative imagery SHALL carry empty `alt` text.

#### Scenario: Heading outline
- **WHEN** the page's heading outline is extracted
- **THEN** there is exactly one `<h1>` and no heading level is skipped

#### Scenario: Landmark structure
- **WHEN** the page is inspected for landmarks
- **THEN** the primary content is wrapped in a `<main>` element
