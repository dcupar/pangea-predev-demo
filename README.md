# Pangea + Pre.dev Integration Prototype

Static HTML/CSS mockups demonstrating the integration of pre.dev AI specifications into the Pangea platform.

**Live Demo:** https://pangea-predev-demo.vercel.app

---

## Overview

This prototype shows how AI-generated technical specifications enhance the brief submission and opportunity evaluation process for both clients and agencies.

## Documentation

| Document | Description |
|----------|-------------|
| [IMPLEMENTATION_GUIDE.md](./IMPLEMENTATION_GUIDE.md) | Full technical implementation guide for developers |
| [LOOM_SCRIPT.md](./LOOM_SCRIPT.md) | Presentation script for developer walkthrough video |

---

## Mockups

### Client Flow

| File | Description |
|------|-------------|
| `homepage-hero.html` | Landing page with project description input |
| `signup-modal.html` | Registration form and plan selection |
| `loading.html` | Spec generation progress animation |
| `client-homepage.html` | Client dashboard with project list |
| `project-page.html` | Project detail with specs summary card |
| `spec-viewer.html` | Full AI-generated specifications display |

### Agency Flow

| File | Description |
|------|-------------|
| `agency-opportunities.html` | Opportunities list with filters |
| `opportunity-detail.html` | Full brief data with specs access |
| `spec-viewer.html` | Same spec viewer (shared component) |

---

## User Flows

### Client Flow
```
Homepage → Signup → Loading → Dashboard → Project → Specs
```

### Agency Flow
```
Opportunities List → Opportunity Detail → Specs
```

---

## How to Run Locally

```bash
# Option 1: Python server
python3 -m http.server 8888

# Option 2: Node server
npx serve .

# Option 3: Open directly
open index.html
```

---

## Brand Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `--pangea-green-dark` | #176448 | Dark green, logo |
| `--pangea-green` | #22896a | Primary buttons |
| `--pangea-green-hover` | #1d7a5e | Hover states |
| `--pangea-green-light` | #d1fae5 | Backgrounds, badges |

---

## Tech Stack

- Pure HTML/CSS (no frameworks)
- Inter font (Google Fonts)
- Responsive design
- CSS custom properties for theming

---

## For Developers

See [IMPLEMENTATION_GUIDE.md](./IMPLEMENTATION_GUIDE.md) for:
- Pre.dev API integration details
- Database schema updates
- TypeScript interfaces
- Component specifications
- Testing checklist
