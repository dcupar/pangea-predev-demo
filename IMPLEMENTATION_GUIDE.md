# Pre.dev Integration Implementation Guide

This guide is for developers implementing the pre.dev AI specifications feature into the Pangea platform.

---

## Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Pre.dev API Integration](#predev-api-integration)
4. [Client Flow Implementation](#client-flow-implementation)
5. [Agency Flow Implementation](#agency-flow-implementation)
6. [Data Models](#data-models)
7. [UI Components Reference](#ui-components-reference)
8. [Testing Checklist](#testing-checklist)

---

## Overview

### What We're Building

Integrating pre.dev's AI specification generator into Pangea to:
- Auto-generate technical specifications from client project descriptions
- Provide agencies with detailed project breakdowns before bidding
- Improve brief quality and reduce back-and-forth communication

### Key User Flows

**Client Flow:**
1. Client submits project description (brief)
2. Pre.dev API generates specifications (async, ~60-90 seconds)
3. Client views specifications on their dashboard
4. Specifications are attached to the opportunity for agencies

**Agency Flow:**
1. Agency browses opportunities list
2. Opens opportunity detail with brief data
3. Views AI-generated specifications for better proposal preparation

---

## Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Client App    │────▶│   Pangea API    │────▶│   Pre.dev API   │
│   (Frontend)    │◀────│   (Backend)     │◀────│   (External)    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                               │
                               ▼
                        ┌─────────────────┐
                        │    Database     │
                        │  (Specs Cache)  │
                        └─────────────────┘
```

### Flow Sequence

```
1. Client submits brief
   └─▶ POST /api/briefs

2. Backend triggers spec generation
   └─▶ POST pre.dev/api/generate (async)

3. Pre.dev webhook or polling returns specs
   └─▶ Store in database, link to brief

4. Client/Agency views specs
   └─▶ GET /api/briefs/{id}/specifications
```

---

## Pre.dev API Integration

### Authentication

```typescript
// Environment variables
PREDEV_API_KEY=your_api_key
PREDEV_API_URL=https://api.pre.dev/v1
```

### Generate Specifications

```typescript
// POST /api/generate
interface GenerateSpecsRequest {
  project_description: string;
  project_type?: 'web' | 'mobile' | 'api' | 'fullstack';
  complexity_hint?: 'simple' | 'medium' | 'complex';
  webhook_url?: string; // For async callback
}

interface GenerateSpecsResponse {
  job_id: string;
  status: 'pending' | 'processing' | 'completed' | 'failed';
  estimated_time_seconds: number;
}
```

### Retrieve Specifications

```typescript
// GET /api/specs/{job_id}
interface SpecificationResult {
  job_id: string;
  status: 'completed';
  specifications: {
    executive_summary: string;
    core_functionalities: Functionality[];
    tech_stack: TechStack;
    milestones: Milestone[];
    complexity_score: number; // 1-10
    estimated_hours: number;
    implementation_guidelines: string[];
  };
}

interface Functionality {
  name: string;
  description: string;
  priority: 'must-have' | 'should-have' | 'nice-to-have';
}

interface TechStack {
  frontend: string[];
  backend: string[];
  database: string[];
  infrastructure: string[];
  third_party: string[];
}

interface Milestone {
  number: number;
  title: string;
  description: string;
  user_stories: UserStory[];
  estimated_hours: number;
}

interface UserStory {
  title: string;
  description: string;
  acceptance_criteria: string[];
  subtasks: Subtask[];
}

interface Subtask {
  title: string;
  type: 'frontend' | 'backend' | 'design' | 'devops' | 'testing';
  estimated_hours: number;
}
```

### Webhook Callback

```typescript
// POST {your_webhook_url}
interface WebhookPayload {
  job_id: string;
  status: 'completed' | 'failed';
  specifications?: SpecificationResult['specifications'];
  error?: string;
}
```

### Error Handling

```typescript
// Common error codes
400 - Invalid project description (too short/long)
401 - Invalid API key
429 - Rate limit exceeded
500 - Pre.dev internal error

// Retry strategy
const RETRY_DELAYS = [1000, 2000, 5000]; // ms
const MAX_RETRIES = 3;
```

---

## Client Flow Implementation

### 1. Brief Submission Form

**Location:** Homepage / New Project page

```typescript
// Component: BriefSubmissionForm
interface BriefFormData {
  project_description: string; // Min 100 chars, max 5000
  company_name: string;
  contact_email: string;
  budget_range: { min: number; max: number };
  timeline: string;
  engagement_type: 'project' | 'team_augmentation';
}

// On submit:
async function handleSubmit(data: BriefFormData) {
  // 1. Create brief in database
  const brief = await createBrief(data);

  // 2. Trigger spec generation (async)
  await triggerSpecGeneration(brief.id, data.project_description);

  // 3. Redirect to loading/dashboard
  router.push('/dashboard?brief=' + brief.id);
}
```

### 2. Loading/Generation Screen

**Location:** `/loading` or modal overlay

```typescript
// Component: SpecGenerationLoader
interface LoaderProps {
  briefId: string;
}

// Steps to display:
const GENERATION_STEPS = [
  { label: 'Analyzing project requirements', duration: 15 },
  { label: 'Identifying tech stack & architecture', duration: 20 },
  { label: 'Creating implementation milestones', duration: 25 },
  { label: 'Estimating timeline & resources', duration: 15 },
  { label: 'Finalizing specifications document', duration: 15 },
];

// Poll for completion:
useEffect(() => {
  const interval = setInterval(async () => {
    const status = await getSpecStatus(briefId);
    if (status === 'completed') {
      clearInterval(interval);
      router.push('/project/' + briefId);
    }
  }, 3000); // Poll every 3 seconds

  return () => clearInterval(interval);
}, [briefId]);
```

### 3. Client Dashboard

**Location:** `/dashboard`

```typescript
// Component: ClientDashboard
// Shows list of projects with spec status

interface ProjectCard {
  id: string;
  title: string;
  status: 'generating' | 'ready' | 'matched';
  spec_status: 'pending' | 'completed';
  created_at: Date;
  applications_count: number;
}
```

### 4. Project Detail Page

**Location:** `/project/[id]`

```typescript
// Component: ProjectDetail
// Shows project info + specs summary + link to full specs

interface ProjectDetailProps {
  project: Project;
  specifications: SpecificationSummary;
}

// Spec summary card shows:
// - Milestones count
// - User stories count
// - Subtasks count
// - Complexity score
// - "View Full Specifications" button
```

### 5. Spec Viewer

**Location:** `/project/[id]/specifications`

```typescript
// Component: SpecViewer
// Full specifications display with collapsible sections

interface SpecViewerProps {
  specifications: FullSpecifications;
}

// Sections:
// 1. Executive Summary
// 2. Core Functionalities (cards)
// 3. Tech Stack (grouped badges)
// 4. Milestones (accordion with user stories)
// 5. Implementation Guidelines
```

---

## Agency Flow Implementation

### 1. Opportunities List

**Location:** `/opportunities`

```typescript
// Component: OpportunitiesList
interface OpportunityRow {
  id: string;
  title: string;
  company: string;
  budget_range: string;
  engagement_type: string;
  deadline: Date;
  status: 'open' | 'closing_soon' | 'closed';
  has_specifications: boolean; // Show badge if specs available
  tags: string[]; // Tech stack tags
}

// Filter options:
// - Status (open/closed)
// - Engagement type
// - Budget range
// - Tech stack
// - Has AI specs (new filter!)
```

### 2. Opportunity Detail

**Location:** `/opportunities/[id]`

```typescript
// Component: OpportunityDetail
interface OpportunityDetailProps {
  opportunity: Opportunity;
  brief: Brief;
  specifications?: Specifications;
}

// Sections:
// 1. Header (title, company, status badges)
// 2. Brief Data (description, requirements, tech stack, budget)
// 3. AI Specifications Card (if available)
//    - Shows spec stats
//    - "View Full Specifications" button
// 4. Attached Documents
// 5. Activity Timeline
// 6. Apply/Express Interest CTA
```

### 3. Spec Viewer (Agency View)

**Location:** `/opportunities/[id]/specifications`

Same component as client spec viewer, but with agency-specific context:
- "Back to Opportunity" button
- No edit capabilities
- Optional: Download as PDF

---

## Data Models

### Database Schema Updates

```sql
-- Add specifications table
CREATE TABLE specifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  brief_id UUID REFERENCES briefs(id),
  predev_job_id VARCHAR(255),
  status VARCHAR(50) DEFAULT 'pending',
  raw_response JSONB,
  executive_summary TEXT,
  complexity_score DECIMAL(3,1),
  estimated_hours INTEGER,
  milestones_count INTEGER,
  user_stories_count INTEGER,
  subtasks_count INTEGER,
  created_at TIMESTAMP DEFAULT NOW(),
  completed_at TIMESTAMP,
  error_message TEXT
);

-- Add index for quick lookups
CREATE INDEX idx_specs_brief_id ON specifications(brief_id);
CREATE INDEX idx_specs_status ON specifications(status);

-- Update briefs table
ALTER TABLE briefs ADD COLUMN has_specifications BOOLEAN DEFAULT FALSE;
ALTER TABLE briefs ADD COLUMN specifications_id UUID REFERENCES specifications(id);
```

### TypeScript Interfaces

```typescript
// types/specifications.ts

export interface Specifications {
  id: string;
  briefId: string;
  status: 'pending' | 'processing' | 'completed' | 'failed';
  executiveSummary: string;
  coreFunctionalities: CoreFunctionality[];
  techStack: TechStack;
  milestones: Milestone[];
  complexityScore: number;
  estimatedHours: number;
  implementationGuidelines: string[];
  createdAt: Date;
  completedAt?: Date;
}

export interface CoreFunctionality {
  name: string;
  description: string;
  priority: 'must-have' | 'should-have' | 'nice-to-have';
  icon?: string;
}

export interface TechStack {
  frontend: TechItem[];
  backend: TechItem[];
  database: TechItem[];
  infrastructure: TechItem[];
  thirdParty: TechItem[];
}

export interface TechItem {
  name: string;
  reason?: string;
}

export interface Milestone {
  number: number;
  title: string;
  description: string;
  estimatedHours: number;
  userStories: UserStory[];
}

export interface UserStory {
  id: string;
  title: string;
  description: string;
  acceptanceCriteria: string[];
  subtasks: Subtask[];
}

export interface Subtask {
  id: string;
  title: string;
  type: 'frontend' | 'backend' | 'design' | 'devops' | 'testing';
  estimatedHours: number;
  completed?: boolean;
}
```

---

## UI Components Reference

### Color Palette

```css
:root {
  --pangea-green-dark: #176448;
  --pangea-green: #22896a;
  --pangea-green-hover: #1d7a5e;
  --pangea-green-light: #d1fae5;

  --text-primary: #1a202c;
  --text-secondary: #4a5568;
  --text-muted: #718096;

  --bg-primary: #ffffff;
  --bg-secondary: #f7fafc;
  --border-color: #e2e8f0;
}
```

### Key Components

| Component | Location | Description |
|-----------|----------|-------------|
| `SpecCard` | Dashboard/Detail | Summary card with stats and CTA |
| `SpecViewer` | Full page | Complete specs display |
| `MilestoneAccordion` | SpecViewer | Expandable milestone sections |
| `TechStackBadges` | Multiple | Tech stack tags display |
| `GenerationLoader` | Loading page | Animated progress steps |
| `StatusBadge` | Lists | Open/Closed/Generating status |

### Mockup Files Reference

| File | Purpose | Key Elements |
|------|---------|--------------|
| `homepage-hero.html` | Landing page | Project description input |
| `signup-modal.html` | Registration | Form + plan selection |
| `loading.html` | Generation | Progress steps animation |
| `client-homepage.html` | Dashboard | Project cards with status |
| `project-page.html` | Project detail | Specs summary card |
| `spec-viewer.html` | Full specs | All spec sections |
| `agency-opportunities.html` | Opportunities list | Table with filters |
| `opportunity-detail.html` | Opportunity detail | Brief + specs card |

---

## Testing Checklist

### Unit Tests

- [ ] Pre.dev API client methods
- [ ] Specification data transformations
- [ ] Status polling logic
- [ ] Error handling and retries

### Integration Tests

- [ ] Brief submission triggers spec generation
- [ ] Webhook properly updates spec status
- [ ] Specs correctly linked to briefs/opportunities
- [ ] Agency can view specs for open opportunities

### E2E Tests

- [ ] Complete client flow: submit → loading → view specs
- [ ] Complete agency flow: browse → detail → view specs
- [ ] Error states: API failure, timeout, invalid data
- [ ] Loading states render correctly

### Manual QA

- [ ] Responsive design on mobile/tablet
- [ ] Loading animation smooth
- [ ] Accordion expand/collapse works
- [ ] Back navigation works correctly
- [ ] PDF download (if implemented)

---

## Environment Setup

```bash
# Required environment variables
PREDEV_API_KEY=your_api_key_here
PREDEV_API_URL=https://api.pre.dev/v1
PREDEV_WEBHOOK_SECRET=your_webhook_secret

# Optional
PREDEV_TIMEOUT_MS=90000
PREDEV_MAX_RETRIES=3
```

---

## Questions?

Contact the product team for:
- Pre.dev API access and credentials
- Design clarifications
- Priority decisions on features

Refer to the mockups at: https://pangea-predev-demo.vercel.app
