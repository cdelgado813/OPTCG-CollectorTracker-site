## ADDED Requirements

### Requirement: Social preview metadata
Every page SHALL declare Open Graph and Twitter card metadata: `og:title`, `og:description`, `og:type`, `og:url`, `og:image`, `twitter:card` set to `summary_large_image`, and a `theme-color`. Values SHALL be page-specific for title, description, and URL.

#### Scenario: Landing page shared to Discord
- **WHEN** `https://optcg.poordevelopers.com/` is pasted into a link-unfurling client
- **THEN** the unfurl shows the landing page title, its description, and the social preview image

#### Scenario: Legal page shared
- **WHEN** `privacy.html` is shared
- **THEN** the unfurl shows the privacy page's own title and description, not the landing page's

### Requirement: Social preview image
The repository SHALL contain `assets/og-image.png` at 1200×630, rendered in the site's dark palette, carrying the product name and the core claim, and referenced as an absolute `https://optcg.poordevelopers.com/...` URL in `og:image`.

#### Scenario: Image dimensions
- **WHEN** `assets/og-image.png` is inspected
- **THEN** it measures 1200×630 pixels

#### Scenario: Absolute URL
- **WHEN** the `og:image` meta tag is read
- **THEN** its content is an absolute URL, because relative paths are not resolved by all unfurling clients

### Requirement: Canonical URL and language
Every page SHALL declare a `<link rel="canonical">` pointing at its absolute URL on the custom domain, and SHALL keep `lang="en"` on the `<html>` element.

#### Scenario: Canonical on landing page
- **WHEN** `index.html` is inspected
- **THEN** its canonical URL is `https://optcg.poordevelopers.com/`

### Requirement: No third-party head dependencies
The document head of every page SHALL contain no `<link>` or `<script>` referencing an external origin. Any preconnect hints to font CDNs SHALL be removed.

#### Scenario: Head audit
- **WHEN** the `<head>` of any page is inspected
- **THEN** every `href` and `src` is a same-origin relative path

### Requirement: Shared head block consistency
The favicon reference, viewport declaration, charset, and stylesheet link SHALL be identical across all three pages so the head block can be maintained as a single copied unit.

#### Scenario: Head comparison
- **WHEN** the head blocks of `index.html`, `privacy.html`, and `disclaimer.html` are compared
- **THEN** they differ only in title, description, canonical URL, and the page-specific Open Graph values
