# AS-IS Process: Morrison Low Invoice Creation
**Date:** 2025-11-20
**Source:** Initial Discovery Meeting Transcript
**Prepared by:** IT360 Analysis Team

---

## Executive Summary

Morrison Low's current invoicing process is a **multi-stage, highly manual workflow** spanning ProjectWorks (project management system), Word templates (workaround tool), and human transformation of internal notes into client-ready narratives. The process involves 3 primary stakeholder groups across 3 geographic offices (Auckland, Wellington, Australia) executing identical workflows monthly.

**Key Characteristics:**
- **Frequency:** Monthly (first week of each month - "invoice week")
- **Volume:** 10-15 invoices per consultant average
- **Time Investment:** ~4 hours per consultant monthly
- **Pain Points:** Fragmented data sources, manual data entry, context-switching overhead, repetitive summarization work

---

## Current Process Flow

### Phase 1: Monthly Data Preparation
**Owner:** Project Support Staff (Christina - Wellington, Roxy - Auckland, + 1 Australia)

```
┌─────────────────────────────────────────────────────────┐
│ 1. EXTRACT DATA FROM PROJECTWORKS                      │
│    - Run WIP (Work in Progress) reports per consultant │
│    - Export to PDF format                              │
│    - Manually pull: budget data, timesheet summaries,  │
│      previous invoices, forecast amounts               │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 2. POPULATE WORD TEMPLATES                             │
│    - Create individual Word doc per consultant         │
│    - Each doc contains tables for ALL active projects  │
│      (10-12 projects simultaneously typical)           │
│    - For EACH project table, manually enter:           │
│      • Total budget                                    │
│      • Amount invoiced to date                         │
│      • Time invoiced to date (hours, not $)            │
│      • Previous month's invoice notes (in blue)        │
│      • Current month WIP summary                       │
│      • Forecast amounts                                │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 3. DISTRIBUTE TO CONSULTANTS                           │
│    - Send Word doc + WIP PDF to each consultant        │
│    - Provide Invoice Breakdown Report from ProjectWorks│
└─────────────────────────────────────────────────────────┘
```

**PAIN POINTS:**
- All data entry is manual (no automation)
- ProjectWorks data exists but is fragmented across multiple tabs/reports
- SQL queries attempted but too complex/incomplete (Christina tried, hit limitations)
- Repeated 3x for each office using identical process

---

### Phase 2: Consultant Invoice Preparation
**Owner:** Project Manager/Consultants (e.g., Matt)

```
┌─────────────────────────────────────────────────────────┐
│ 4. CONSULTANT REVIEWS MULTI-SOURCE INFORMATION          │
│    Sources consultant must triangulate:                 │
│    a) Word template (budget, previous notes, forecast)  │
│    b) WIP PDF (current month timesheet summary)         │
│    c) ProjectWorks timesheet entries (30+ individual)   │
│    d) Statement of Work (SOW) - scope reference         │
│    e) Previous month invoice (avoid repetition)         │
│                                                         │
│    Mental workload:                                     │
│    - Context switch between 5 different sources         │
│    - Remember what was billed before                    │
│    - Understand project scope from SOW                  │
│    - Assess budget remaining vs work done               │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 5. CRAFT CLIENT-READY INVOICE NARRATIVE                │
│    Transformation required:                             │
│    • INPUT: 30+ timesheet entries with varying quality  │
│      Examples: "meeting with client" (generic)          │
│                "cost model review" (terse)              │
│                "." (full stop - minimal)                │
│                                                         │
│    • OUTPUT: 5 professional bullet points that:         │
│      - Justify the invoice amount                       │
│      - Use client-appropriate language                  │
│      - Reference SOW deliverables                       │
│      - Don't reveal internal methodology                │
│      - Don't repeat previous month's wording            │
│                                                         │
│    Example transformation:                              │
│    FROM: "cost model review" (1 hour) x 10 entries     │
│    TO: "Cost modeling and cost benefit analysis for     │
│         material recycling facility RFP"                │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 6. DETERMINE BILLING AMOUNT (Value-Based Override)     │
│    - Review total WIP hours × rate = calculated amount │
│    - Make strategic decision:                           │
│      • Bill full amount? ($10,000)                      │
│      • Bill partial? ($8,000 - defer $2K to next phase) │
│      • Write off hours? (over-servicing client)         │
│    - Document override decision in Word template        │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 7. MANUAL ENTRY INTO WORD TEMPLATE                     │
│    - Type narrative into template table                 │
│    - Enter final billing amount                         │
│    - Repeat for each project (10-15 times)              │
│                                                         │
│    TIME IMPACT: 4 hours average per consultant          │
│                 "Interrupts flow" - must down tools     │
└─────────────────────────────────────────────────────────┘
```

**PAIN POINTS:**
- Heavy cognitive load (triangulating 5+ sources)
- Creative writing work (summarization = non-billable time)
- Quality of timesheet notes varies wildly by consultant
- Project managers write better notes (accountability) but non-PMs write minimal notes
- Must remember context from previous months
- Repetitive work (same mental process 10-15x per month)

---

### Phase 3: Finalization & System Entry
**Owner:** Project Support Staff + Finance Review

```
┌─────────────────────────────────────────────────────────┐
│ 8. PROJECT SUPPORT COPIES NOTES BACK TO PROJECTWORKS   │
│    - Manually copy consultant's narrative from Word     │
│    - Paste into ProjectWorks invoice description field  │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 9. TIMESHEET LINE ITEM ADJUSTMENT (Complex!)           │
│    Problem: Consultant said "bill $8,000" but WIP shows│
│            $10,245 worth of timesheet entries           │
│                                                         │
│    Project Support must:                                │
│    - Manually select/deselect individual timesheet lines│
│    - "Wiggle" combinations to get closest to target     │
│      (e.g., if target is $8,000, find combination of    │
│       timesheet entries totaling $7,998 or $8,002)      │
│    - Unbilled timesheets carry forward to next month    │
│      (must not be lost!)                                │
│                                                         │
│    QUOTED: "You've got to really do some wiggling to    │
│             get it to that amount. It's not just a      │
│             simple override."                           │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 10. APPROVAL WORKFLOW (Details unclear from transcript) │
│     - Finance Manager (Anna) reviews                    │
│     - Project Directors (PD) review                     │
│     - Pushback mechanism exists (consultants get        │
│       questioned on under-billing)                      │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 11. INVOICE GENERATION IN PROJECTWORKS                 │
│     - Final invoice created in ProjectWorks             │
│     - Includes: narrative, line items, $ amounts        │
│     - Does NOT include hourly breakdown (by design)     │
│       "We don't provide that breakdown to clients"      │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ 12. EXPORT TO XERO (Automated)                         │
│     - ProjectWorks auto-syncs to correct Xero instance  │
│       (NZ company → NZ Xero, AU company → AU Xero)      │
│     - Multi-currency supported                          │
│     - Finance manager performs final checks in Xero     │
└─────────────────────────────────────────────────────────┘
```

**PAIN POINTS:**
- Double data entry (Word → ProjectWorks)
- Complex "wiggling" algorithm done manually
- Risk of unbilled time being lost
- Approval workflow bottleneck (not fully documented)

---

## System Architecture (Current State)

```
┌──────────────────┐
│  PROJECTWORKS    │  ← Single source of truth (project mgmt system)
│  (Project Mgmt)  │     - Timesheets (consultant entries)
│                  │     - Projects, budgets, forecasts
│  Multi-currency  │     - Invoice generation
│  NZ + AU offices │     - WIP reports
└────────┬─────────┘
         │ Manual extract (PDF, manual queries)
         ↓
┌──────────────────┐
│   WORD TEMPLATE  │  ← Workaround tool (consolidation hub)
│   (Consultant    │     - Brings fragmented data together
│    Workspace)    │     - 1 doc per consultant, multi-project tables
│                  │     - Not sent to clients (internal only)
└────────┬─────────┘
         │ Manual copy/paste
         ↓
┌──────────────────┐
│  PROJECTWORKS    │  ← Invoice finalized
│  (Invoice Gen)   │
└────────┬─────────┘
         │ Automated sync
         ↓
┌──────────────────┐
│      XERO        │  ← Accounting system (2 instances)
│  (2 entities:    │     - NZ company Xero
│   NZ + AU)       │     - AU company Xero
└──────────────────┘
```

**KEY INSIGHT:** Word template exists purely as a **workaround for ProjectWorks UI limitations**. It consolidates data that exists in ProjectWorks but is too fragmented/slow to access efficiently.

---

## Stakeholder Analysis

### 1. Consultants (Project Managers)
**Primary Users:** Matt (example), ~20-30 consultants firm-wide (estimate)

**Current Workload:**
- 10-15 invoices per month
- 4 hours monthly average
- High variability in timesheet note quality

**Needs:**
- Consolidated view of all project data (budget, WIP, previous invoices)
- Efficient summarization of timesheet notes
- Ability to override billing amounts (value-based pricing)
- Fast workflow (minimize interruption to billable work)

**Frustrations:**
- "Interrupts your flow" - must context-switch from client work
- Manual summarization of 30+ entries into 5 bullets
- Toggling between multiple systems/documents

---

### 2. Project Support Staff
**Users:** Christina (Wellington - 5 years exp), Roxy (Auckland - 5 weeks), 1 AU staff

**Current Workload:**
- Data extraction from ProjectWorks (all consultants)
- Word template population (manual data entry)
- Copy notes back to ProjectWorks
- Complex timesheet line selection ("wiggling")

**Needs:**
- Automated data extraction
- Reduce manual data entry
- Simplified timesheet selection logic
- Multi-office support (same process 3x)

**Frustrations:**
- "Very manual process" - repetitive data entry
- ProjectWorks UI limitations (data fragmented across tabs)
- Complex SQL queries attempted but failed (schema limitations)

---

### 3. Finance Oversight
**User:** Anna (Finance Manager)

**Current Role:**
- Ensures all billable time is captured
- Pushback on consultants who under-bill
- Final Xero review

**Needs:**
- Visibility into billing vs. actual hours
- Ensure no unbilled time lost
- Audit trail of consultant override decisions

**Frustrations:**
- Tension with consultants (finance wants full billing, consultants want value-based pricing)
- Risk of revenue leakage

---

## Key Observations & Insights

### 1. **Root Cause: ProjectWorks UI Limitations**
Word template is a workaround, not a requirement. ProjectWorks contains all necessary data but:
- Fragmented across multiple tabs/reports
- No consolidated view per consultant
- Slow navigation
- Limited reporting/query capabilities

### 2. **Dual Billing Philosophy Tension**
- **Time-based:** Track every hour (finance wants this)
- **Value-based:** Bill what the work is worth (consultants want this)
- Current process supports both but creates friction

### 3. **AI Experimentation Culture**
- Already trialing multiple AI tools (Fireflies, transcription services)
- 15 months of AI tool testing
- Consultants already using Copilot for summarization (ad-hoc)
- Receptive to AI solutions, not skeptical

### 4. **Quality Variance in Source Data**
- Project managers: detailed timesheet notes (incentivized - they write invoices)
- Non-PM consultants: minimal notes (".", "meeting", terse entries)
- **Implication:** AI training data quality will vary significantly

### 5. **Multi-Office Complexity (Low)**
- Same process across 3 offices
- Same ProjectWorks instance (multi-currency enabled)
- ProjectWorks → Xero sync handles entity routing
- **Implication:** Solution scales automatically if built right

---

## Questions Raised (To Be Addressed in Gap Analysis)

1. **API Capabilities:** What can we read/write via ProjectWorks API?
2. **SOW Storage:** Where are Statements of Work stored? (AI needs this context)
3. **Approval Workflow:** Who approves? What do they check? Current SLA?
4. **Timesheet Note Examples:** Can we see best/worst/typical samples?
5. **Error Tolerance:** What happens if AI gets it wrong?
6. **Security/Compliance:** Any restrictions on AI processing client data?
7. **Pilot Candidates:** Which consultants willing to test first?

---

## Next Steps

1. **Requirements Gap Analysis:** Identify missing information needed for solution design
2. **Meeting Preparation:** Organize questions by priority for next customer meeting
3. **API Discovery:** Request ProjectWorks API documentation
4. **Sample Data Request:** Ask for anonymized timesheet note examples

---

_Document Status: High-level draft for review. Will refine based on additional discovery._
