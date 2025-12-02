# Pangea Pre.dev Prototype - V2 Updates

**Updated:** December 1, 2025 (Revision 2)
**Based on:** Stakeholder Feedback Session + Follow-up Refinements

---

## Summary of Changes

This document outlines all updates made to the prototype based on feedback. Each change is marked and can be demonstrated.

---

## 1. Homepage Hero (homepage-hero-v2.html) - UPDATED (Rev 2)

### Changes Made:

**UI Structure:**
- **Top Toolbar** - Voice button + New/Existing project toggle
  - Speak button with microphone icon (Web Speech API - Chrome/Edge)
  - "Listening..." state with pulsing animation
  - New project / Existing project pill toggle

- **Main Textarea** - Large, clean input area
  - Short placeholder: "What do you want to build?"
  - 160px minimum height

- **Brownfield Support** - SINGLE field only (simplified)
  - Appears when "Existing project" selected
  - One field: "Current Tech Stack"
  - Placeholder: "e.g., React, Node.js, PostgreSQL, AWS"

- **Fast Spec / Deep Spec Toggle** - NEW (inside prompt box)
  - Fast spec: ~30-60 sec, 10 credits (MVPs/prototypes)
  - Deep spec: ~2-5 min, 50 credits (production apps)
  - Hover tooltips explain each option with timing/credit cost

- **Engagement Options** - OUTSIDE prompt box (reverted)
  - 4 equal cards below the prompt box
  - "Build a Product" with AI badge (default selected)
  - "Hire Devs", "Fractional CTO", "Remote Team"
  - Not inside the prompt field - separate section

**Signup Flow:**
- **Shows during loading** - Qualifies leads before spending credits
  - Modal appears when "Generate specs" clicked
  - SSO buttons (Google, Microsoft)
  - Email input as fallback
  - User signs in while specs generate

**Copy:**
- Title: "Describe your product, get specs in minutes"
- Subtitle: "Transform your idea into technical specs, milestones, and cost estimates." (shortened)

**Social Proof:**
- 2,400+ projects delivered
- 500+ vetted agencies
- 48hrs to proposals

---

## 2. Project Page (project-page.html) - UPDATED

### Changes Made:
- **Edit Brief Removed** - Functionality dropped to reduce complexity
  - Edit state banners removed (editable, reviewing, locked)
  - Demo toggle removed
  - Edit Brief button removed
  - Page is now read-only view of generated specifications
  - Rationale: Editing would cost additional credits, unclear product rules, UI not suited for reviewing changes

- **Updated Project Example** - Changed from generic to specific
  - "Dog Walking Mobile App" instead of "SaaS Project Management Platform"
  - "Acme Corp" as company name

---

## 3. Onboarding Flow (onboarding.html) - UPDATED

### Changes Made:
- **Pre-selected Engagement Type** - Accepts URL parameter
  - Links from homepage now pre-select the relevant option
  - Example: `onboarding.html?type=augmentation` pre-selects "Team Augmentation"
  - Supported types: `augmentation`, `bot`, `fractional`

---

## 4. Loading Page (loading.html) - UPDATED

### Changes Made:
- **Updated Copy** - More compelling
  - Old: "Generating Your Specifications"
  - New: "Building Your Technical Blueprint"
  - Subhead: "We're transforming your vision into actionable specs—milestones, tech stack recommendations, and accurate estimates that agencies can quote against."

---

## 5. Index/Demo Hub (index.html) - UPDATED

### Changes Made:
- **Added V2 Link** - Homepage Hero V2 is now the default
- **Preserved V1** - Original version still accessible for comparison
- **Updated navigation** - "Start Client Flow" now points to V2

---

## Files Modified

| File | Status | Key Changes |
|------|--------|-------------|
| `homepage-hero-v2.html` | **NEW** | Voice input, restructured options, brownfield, SSO |
| `project-page.html` | UPDATED | Edit state banners, demo toggle |
| `onboarding.html` | UPDATED | Pre-selected engagement type support |
| `loading.html` | UPDATED | Improved copy |
| `index.html` | UPDATED | Navigation to V2 |

---

## What Was NOT Changed (Requires Separate Resourcing)

| Request | Why | What's Needed |
|---------|-----|---------------|
| Animated Avatar (Sarah.ai-style) | Complex feature requiring 3D assets, TTS, conversation engine | 4-8 weeks, dedicated team, $15-50K |
| Actual SSO Implementation | Requires backend + Auth0 config | Backend developer, 1-2 weeks |

---

## How to Review

1. **Start at:** `index.html`
2. **Click:** "Start Client Flow" (goes to V2)
3. **Test Voice Input:** Click microphone, speak (requires Chrome/Edge)
4. **Test Brownfield:** Toggle to "Existing Project" to see tech stack input
5. **Check Engagement Options:** Note all 4 are now equal cards
6. **View Edit States:** Go to `project-page.html` and use demo toggle
7. **Compare V1 vs V2:** Both versions accessible from index

---

## Questions for Product

Before further development, we need answers on:

1. **Brownfield Depth:** How detailed should tech stack input be?
2. **Credit Display:** Should users see their credit balance?
