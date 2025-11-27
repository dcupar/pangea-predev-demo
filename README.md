# Pangea Pre.dev Integration Mockups

Static HTML/CSS mockups for the prompt-first Project Work flow integration.

## Files

- `homepage-hero.html` - Full homepage hero with prompt input, CTA, and signup modal
- `signup-modal.html` - Standalone modal component with all states (signup, login, loading, success)
- `styles/pangea-mockup.css` - Shared CSS with Pangea brand colors and design tokens

## How to View

Open any HTML file directly in your browser:

```bash
open homepage-hero.html
```

Or start a local server:

```bash
npx serve .
```

## Brand Colors

| Token | Value | Usage |
|-------|-------|-------|
| `--pangea-green-dark` | #1a3d2e | Hero background |
| `--pangea-green-hero` | #1e4d3d | Hero gradient start |
| `--pangea-green-primary` | #0CAD80 | Primary buttons, links |
| `--pangea-green-primary-hover` | #0a9970 | Button hover state |

## User Flow

1. User enters project description in hero textarea
2. Clicks "Generate Specifications"
3. Signup modal appears (or login for returning users)
4. User submits form
5. Loading state shows spec generation progress
6. Success state with redirect to project page

## Notes for Webflow Implementation

- All styles use CSS custom properties for easy theming
- Font: Inter (Google Fonts)
- Modal uses `position: fixed` overlay pattern
- Responsive breakpoints at 768px
