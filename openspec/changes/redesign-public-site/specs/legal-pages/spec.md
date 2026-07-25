## ADDED Requirements

### Requirement: Legal copy is preserved verbatim
The rewrite of `privacy.html` and `disclaimer.html` SHALL preserve the existing legal and disclaimer prose word for word, including the "Last updated" date, the callout summaries, and the section headings. Only markup structure, class names, and presentation may change.

#### Scenario: Text diff after rewrite
- **WHEN** the rendered text content of each legal page before and after the rewrite is extracted and compared
- **THEN** the two are identical apart from navigation and footer chrome

#### Scenario: Updated date
- **WHEN** the redesigned legal pages are inspected
- **THEN** the "Last updated" line still reads July 2026, because the redesign changes no policy

### Requirement: Shared page shell
`privacy.html` and `disclaimer.html` SHALL use the same header and footer markup and classes as the landing page, and SHALL declare no inline `style` attributes. The header brand SHALL link back to `index.html`.

#### Scenario: Shell consistency
- **WHEN** a visitor navigates from the landing page to a legal page
- **THEN** the header and footer are visually and structurally identical, with no shift in brand mark position or type

#### Scenario: Inline styles removed
- **WHEN** the legal pages' markup is inspected
- **THEN** no element carries a `style` attribute; the header brand link is styled entirely from `style.css`

### Requirement: Readable prose measure
Legal prose SHALL be constrained to a comfortable reading measure of approximately 60 to 75 characters per line, independent of the wider container used by the landing page's layout sections.

#### Scenario: Wide viewport prose
- **WHEN** a legal page renders at 1440px wide
- **THEN** paragraph text does not span the full container width and stays within the target measure

### Requirement: Legal page heading hierarchy and readability
Legal pages SHALL use body-text colour for paragraph prose rather than the muted/secondary role, reserving muted colour for the "Last updated" line and footer text, and SHALL keep section headings visually distinct from body text through the type scale.

#### Scenario: Paragraph contrast
- **WHEN** a legal page paragraph is measured for contrast in either palette
- **THEN** it uses the primary text colour role and meets at least 4.5:1

### Requirement: Cross-linking between legal pages
Each legal page SHALL link to the other legal page and back to the landing page from its footer.

#### Scenario: Navigating from privacy
- **WHEN** a visitor reaches the footer of `privacy.html`
- **THEN** links to `index.html`, `disclaimer.html`, and Discord are present

#### Scenario: Navigating from disclaimer
- **WHEN** a visitor reaches the footer of `disclaimer.html`
- **THEN** links to `index.html`, `privacy.html`, and Discord are present
