# Pre.dev Integration Demo - Loom Script for Developers

**Video Duration:** ~8-10 minutes
**Audience:** Development team
**Demo URL:** https://pangea-predev-demo.vercel.app

---

## INTRO (30 seconds)

> "Hey team, in this video I'm going to walk you through the pre.dev integration we're building for Pangea. I'll show you the mockups for both the client and agency flows, explain the technical architecture, and point you to the implementation docs."
>
> "The goal is to auto-generate detailed technical specifications from client project descriptions using pre.dev's AI. This helps agencies understand projects better before bidding, and reduces the back-and-forth on requirements."

---

## SECTION 1: CLIENT FLOW (3-4 minutes)

### Homepage Hero
**[Navigate to: homepage-hero.html]**

> "Let's start with the client flow. On the homepage, we're adding this hero section where clients can describe their project. This is the entry point for spec generation."
>
> "The key input here is the project description field. We need to validate it's at least 100 characters - the more detail the client provides, the better the AI specs will be."
>
> "When they click 'Get Started', we'll capture this description and trigger the signup flow."

### Signup Modal
**[Navigate to: signup-modal.html]**

> "After entering their project idea, clients see this signup modal. It collects the standard brief information - company name, email, budget range, engagement type."
>
> "The important thing here is we're capturing the project description from the previous step and including it with the brief submission."
>
> "When they submit, two things happen: we create the brief in our database, and we fire off the request to pre.dev's API to start generating specs."

### Loading Screen
**[Navigate to: loading.html]**

> "This is the loading experience while pre.dev generates the specifications. In production, this takes about 60-90 seconds."
>
> "We're showing a progress animation with steps: analyzing requirements, identifying tech stack, creating milestones, estimating timeline, and finalizing."
>
> "Technically, we'll be polling our backend every few seconds to check if the pre.dev webhook has delivered the completed specs. Once done, we redirect to the dashboard."

### Client Dashboard
**[Navigate to: client-homepage.html]**

> "Here's the client dashboard after specs are generated. They see their project with the status indicators."
>
> "Notice the project card shows 'AI Specs Ready' - that badge appears once pre.dev completes generation. They can click through to see the full project."

### Project Page
**[Navigate to: project-page.html]**

> "On the project detail page, we have the AI Specifications card here on the right. It shows key stats from the generated specs: number of milestones, user stories, subtasks, and the complexity score."
>
> "This gives the client a quick overview without overwhelming them. The 'View Full Specifications' button opens the complete breakdown."

### Spec Viewer
**[Navigate to: spec-viewer.html]**

> "This is the full specifications viewer. This is the main output from pre.dev that we're displaying to users."
>
> **[Scroll through sections]**
>
> "We have the Executive Summary at the top - a high-level overview of what the project entails."
>
> "Core Functionalities section breaks down the main features with priority levels - must-have, should-have, nice-to-have."
>
> "Tech Stack shows the recommended technologies, grouped by frontend, backend, database, and infrastructure."
>
> "The Milestones section is the meat of it - each milestone has user stories, and each story has detailed subtasks with hour estimates."
>
> **[Expand a milestone]**
>
> "So for example, Milestone 1 here has user stories like 'User Registration' with subtasks for frontend components, backend API endpoints, database schema - all with time estimates."
>
> "This level of detail is what makes this valuable for agencies when evaluating opportunities."

---

## SECTION 2: AGENCY FLOW (2-3 minutes)

### Opportunities List
**[Navigate to: agency-opportunities.html]**

> "Now let's look at the agency side. This is the opportunities portal where agencies browse available projects."
>
> "The key addition here is agencies can now see which opportunities have AI-generated specifications. See the 'AI Specs' badge on the CrossCast project."
>
> "We could also add a filter to show only opportunities with specs - that's an option to consider."

### Opportunity Detail
**[Navigate to: opportunity-detail.html]**

> "When an agency clicks into an opportunity, they see the full brief data we already capture - project description, requirements, budget, timeline."
>
> "The new addition is this AI Specifications card. It shows the same stats - milestones, user stories, complexity score - and the button to view full specs."
>
> "This is the same spec viewer component the client sees, just accessed from the agency context."
>
> **[Click View Full Specifications]**
>
> "And they get the complete breakdown to inform their proposal. No more guessing what the project really involves."

---

## SECTION 3: TECHNICAL OVERVIEW (2 minutes)

> "Let me quickly cover the technical implementation. I've put together a full implementation guide in the repo - check IMPLEMENTATION_GUIDE.md for all the details."

### Architecture
> "The flow is: Client submits brief to our API, we call pre.dev's generate endpoint asynchronously, they process it and send a webhook when complete, we store the specs and link them to the brief."
>
> "Pre.dev's API takes the project description and returns structured data - executive summary, functionalities, tech stack, milestones with user stories and subtasks."

### Key Integration Points
> "For the frontend team: You'll need the loading component with polling, the spec summary card, and the full spec viewer. All the HTML/CSS is in the mockups as reference."
>
> "For the backend team: We need a new specifications table, the pre.dev API client, webhook handler, and endpoints to retrieve specs by brief ID."
>
> "The data models and TypeScript interfaces are all documented in the implementation guide."

### API Details
> "Pre.dev gives us a job_id when we submit. We poll or wait for webhook. Response includes everything structured - I've documented the full response schema."
>
> "Generation takes 60-90 seconds typically. We should show the loading animation during that time, or let users navigate away and notify when ready."

---

## WRAP UP (30 seconds)

> "That's the overview. To summarize what we're building:"
>
> "1. New spec generation triggered on brief submission"
> "2. Loading experience during generation"
> "3. Spec viewer component used in both client and agency flows"
> "4. Pre.dev API integration with webhook handling"
>
> "Check the GitHub repo for the mockups and implementation guide. The Vercel deployment has all the clickable mockups to reference."
>
> "Let me know if you have questions. We can do a deeper technical dive once you've reviewed the docs."

---

## SCREEN RECORDING CHECKLIST

Before recording:

- [ ] Open demo URL: https://pangea-predev-demo.vercel.app
- [ ] Have all pages open in tabs for quick navigation
- [ ] Test that all links between pages work
- [ ] Clear browser cache
- [ ] Disable notifications
- [ ] Check mic audio levels

Tab order for recording:
1. index.html (start here briefly)
2. homepage-hero.html
3. signup-modal.html
4. loading.html
5. client-homepage.html
6. project-page.html
7. spec-viewer.html
8. agency-opportunities.html
9. opportunity-detail.html
10. spec-viewer.html (from agency context)

---

## KEY POINTS TO EMPHASIZE

1. **Value proposition**: Better project understanding = better proposals = happier clients
2. **Async generation**: 60-90 seconds is normal, need good loading UX
3. **Same spec viewer**: Reusable component for both client and agency
4. **Structured data**: Pre.dev returns structured JSON we can display and query
5. **Implementation docs**: Everything they need is in IMPLEMENTATION_GUIDE.md
