# Requirements Gap Analysis & Next Meeting Preparation
**Date:** 2025-11-20
**Project:** Morrison Low Invoice Automation
**Purpose:** Identify critical unknowns and prepare structured discovery questions

---

## Executive Summary

Based on the initial discovery meeting, we have a **solid understanding of the problem and current process** but **critical gaps remain** in technical capabilities, business rules, and stakeholder requirements. This document organizes unknowns by priority and provides a structured meeting agenda for the next customer session.

**Gap Summary:**
- ✅ **Well Understood:** Current process pain points, stakeholder roles, strategic goals
- ⚠️ **Partially Understood:** Technical architecture, approval workflows, data quality
- ❌ **Unknown:** ProjectWorks API capabilities, SOW storage, error handling requirements, security constraints

---

## Gap Analysis by Domain

### 1. TECHNICAL ARCHITECTURE & INTEGRATION

#### 1.1 ProjectWorks API Capabilities ⚠️ **CRITICAL BLOCKER**

**What We Know:**
- ProjectWorks has an API (mentioned in transcript)
- Christina attempted SQL queries but hit limitations
- API can pull timesheets, jobs, reports

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| What API endpoints are available? (full list) | Determines integration scope | 🔴 High |
| Can we READ: timesheet notes with full text? | AI summarization source data | 🔴 High |
| Can we READ: previous month invoice descriptions? | Context for AI ("avoid repetition") | 🟡 Medium |
| Can we READ: project SOW documents? | AI needs scope context | 🔴 High |
| Can we READ: budget vs. actual data? | Dashboard requirements | 🟡 Medium |
| Can we WRITE: invoice line descriptions? | Core automation capability | 🔴 High |
| Can we WRITE: timesheet selection flags? | "Wiggling" automation | 🟡 Medium |
| Authentication method? (OAuth, API key, etc.) | Security & development approach | 🟡 Medium |
| Rate limits or throttling? | Performance constraints | 🟢 Low |
| Webhook support for real-time updates? | UX responsiveness | 🟢 Low |

**Pre-Meeting Action Required:**
- ✅ **REQUEST:** ProjectWorks API documentation (send before next meeting if possible)
- ✅ **REQUEST:** Access to ProjectWorks sandbox/test environment for API exploration

---

#### 1.2 Statement of Work (SOW) Storage & Access ⚠️ **HIGH PRIORITY**

**What We Know:**
- Consultants reference SOW when crafting invoice narratives
- SOW defines project scope and deliverables
- AI could "compare notes to SOW" (mentioned as value-add)

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| Where are SOWs stored? (ProjectWorks? SharePoint? File system?) | Integration complexity | 🔴 High |
| File format? (PDF, Word, structured data, contract system?) | AI parsing approach | 🔴 High |
| Is SOW linked to project in ProjectWorks? | Automated retrieval feasibility | 🟡 Medium |
| How often do SOWs change? (versioning concern) | Data freshness strategy | 🟢 Low |
| Are SOWs standardized in format/structure? | AI training consistency | 🟡 Medium |

**Discovery Questions:**
1. "Can you show us where a typical Statement of Work is stored for a project?"
2. "When Matt is invoicing, how does he access the SOW - does he search for it or is it linked in ProjectWorks?"
3. "Are SOWs always written documents, or do some projects just have verbal scope agreements?"

---

#### 1.3 System Integration Landscape

**What We Know:**
- ProjectWorks → Xero (automated sync, multi-entity)
- Word templates used as workaround
- Multi-office setup (NZ + AU)

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| Any other systems in play? (CRM, document mgmt, etc.) | Complete integration picture | 🟢 Low |
| IT policies on cloud services? | Deployment constraints | 🟡 Medium |
| Existing integrations we can reference? | Proof of concept | 🟢 Low |
| VPN/network access requirements? | Development environment setup | 🟡 Medium |

---

### 2. BUSINESS RULES & APPROVAL WORKFLOWS

#### 2.1 Approval Process Details ⚠️ **MEDIUM-HIGH PRIORITY**

**What We Know:**
- Finance Manager (Anna) is involved
- Project Directors (PD) review
- Pushback mechanism exists
- Consultants get questioned on under-billing

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **Exact approval sequence:** Consultant → Project Support → Finance → PD? Or different? | Workflow design | 🟡 Medium |
| **What triggers rejection/pushback?** (% under-billed, specific clients, dollar thresholds?) | Validation rules in system | 🟡 Medium |
| **Approval turnaround time?** (hours, days?) | UX performance expectations | 🟢 Low |
| **Who has final authority?** (Can consultant override finance pushback?) | System permissions | 🟡 Medium |
| **Batch approval or one-by-one?** | UI workflow pattern | 🟡 Medium |

**Discovery Questions:**
1. "Walk us through what happens after Matt submits his invoice narratives - who sees it next and what do they check?"
2. "Anna, when you review invoices, what red flags make you push back to the consultant?"
3. "What's the typical turnaround time from consultant submission to final approval?"

---

#### 2.2 Billing Override Logic ("The $8K vs. $10K Decision") ⚠️ **MEDIUM PRIORITY**

**What We Know:**
- Consultants can choose to bill less than actual hours
- Reasons: defer to next phase, write-off, value-based pricing
- Project support must "wiggle" timesheet selection to match target

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **How often does this happen?** (every invoice, 50%, rare?) | Feature priority | 🟡 Medium |
| **Is there a standard deferred billing policy?** | System rules | 🟢 Low |
| **Who decides override amount - consultant alone or requires approval first?** | Workflow sequence | 🟡 Medium |
| **How is unbilled time tracked?** (manual notes, ProjectWorks flag?) | Carry-forward logic | 🟡 Medium |
| **Can unbilled time be lost currently?** (risk assessment) | Error prevention priority | 🟡 Medium |

**Discovery Questions:**
1. "On average, how many of your 15 monthly invoices involve billing LESS than the actual hours worked?"
2. "When you decide to defer $2K to next phase, how do you remember to bill it later? Is there a tracking mechanism?"
3. "Christina/Roxy, when you're 'wiggling' timesheets to hit $8K, how do you decide which timesheet lines to include vs. exclude?"

---

### 3. DATA QUALITY & CONTENT

#### 3.1 Timesheet Note Quality Spectrum ⚠️ **HIGH PRIORITY (AI Training)**

**What We Know:**
- Wide variability: some use ".", others write detailed notes
- Project managers write better notes (accountability)
- Non-PM consultants write minimal notes
- Christina "managed to stop them from just putting a full stop"

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **Can we see anonymized examples?** (best, worst, typical) | AI training data assessment | 🔴 High |
| **Percentage breakdown:** How many consultants write good notes vs. poor notes? | AI accuracy expectations | 🟡 Medium |
| **Who are the "best" note-takers?** (can we learn from them?) | Training data goldmine | 🟡 Medium |
| **Has note quality improved over time?** | Change management feasibility | 🟢 Low |
| **Are there guidelines/training on good notes?** (Karen & Dan's training materials) | Existing standards to preserve | 🟡 Medium |

**Pre-Meeting Request:**
- ✅ **REQUEST:** 10-15 anonymized timesheet note examples spanning quality spectrum
- ✅ **REQUEST:** Copy of Karen & Dan's "invoicing training notes" mentioned in transcript

**Discovery Questions:**
1. "Can you show us 3-5 examples of timesheet notes from the current month - both good and problematic ones?"
2. "If we could wave a magic wand and improve timesheet note quality, what would 'good' look like?"
3. "Would consultants be willing to write slightly better notes if they knew AI would auto-summarize them?"

---

#### 3.2 Invoice Narrative Standards & Examples

**What We Know:**
- Transform internal notes → client-appropriate language
- 5 bullet points typical output
- Must justify invoice amount
- Can't reveal proprietary methodology
- Must reference SOW deliverables

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **Can we see 5-10 actual invoice narratives?** (approved, sent to clients) | AI training target output | 🔴 High |
| **Are there "bad" examples we should avoid?** | Negative training data | 🟡 Medium |
| **Client-specific tone differences?** (corporate vs. govt vs. private) | Customization needs | 🟢 Low |
| **Prohibited words/phrases?** | Content filtering rules | 🟡 Medium |

**Pre-Meeting Request:**
- ✅ **REQUEST:** 5-10 anonymized final invoice narratives (what clients actually saw)

---

### 4. USER EXPERIENCE & WORKFLOW

#### 4.1 Consultant Work Environment & Patterns

**What We Know:**
- ~4 hours monthly invoicing time
- 10-15 invoices per consultant
- "Interrupts flow" frustration
- First week of month is "invoice week"

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **How many consultants total?** (firm-wide count) | Scale/license planning | 🟡 Medium |
| **Where do they invoice from?** (office, home, travel) | Desktop vs. mobile priority | 🟡 Medium |
| **Device preferences?** (Windows, Mac, tablet) | UI compatibility | 🟢 Low |
| **Time of day preference?** (morning, end-of-day, weekend) | Performance/availability needs | 🟢 Low |
| **Invoicing deadline?** (hard date or flexible?) | Urgency factor | 🟡 Medium |

**Discovery Questions:**
1. "How many consultants firm-wide will eventually use this system?"
2. "Do consultants typically invoice from the office, or do some work from home/on the road?"
3. "What's the hard deadline for invoices each month - is it day 5, day 7?"

---

#### 4.2 Pilot & Rollout Strategy

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **Who would pilot first?** (1-3 consultants) | Early adopter identification | 🟡 Medium |
| **Pilot success criteria?** (what convinces them to scale?) | Acceptance testing | 🟡 Medium |
| **Training time availability?** | Adoption planning | 🟢 Low |
| **Parallel run tolerance?** (old + new process simultaneously) | Risk mitigation | 🟡 Medium |

**Discovery Questions:**
1. "Which 1-2 consultants would be great pilot candidates - people who are open to trying new tools and will give honest feedback?"
2. "How much training/learning curve time is reasonable before consultants get frustrated?"

---

### 5. QUALITY, SECURITY & RISK

#### 5.1 Error Tolerance & Consequences ⚠️ **MEDIUM-HIGH PRIORITY**

**What We Know:**
- Client relationships are critical (consulting firm)
- Finance needs to ensure no revenue leakage
- Some clients push back on invoices already

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **What happens if AI summary is wrong and goes to client?** | Risk assessment | 🟡 Medium |
| **Have you had invoice disputes before? What caused them?** | Error patterns to avoid | 🟡 Medium |
| **Who catches errors currently?** (consultant, finance, client feedback?) | Quality gate location | 🟡 Medium |
| **Client tolerance for invoice errors?** (understanding vs. strict) | Acceptable error rate | 🟡 Medium |
| **Any clients on high-alert/sensitive?** | Risk stratification | 🟢 Low |

**Discovery Questions:**
1. "Tell us about a time an invoice went wrong - what happened and what was the impact?"
2. "If AI generates a summary that's 80% right but has one incorrect detail, how bad is that?"
3. "Would you rather have AI be conservative (generic but safe) or creative (detailed but riskier)?"

---

#### 5.2 Data Security & Compliance

**What We Know:**
- Client data is sensitive (consulting confidentiality)
- Multi-jurisdictional (NZ + AU)

**What We DON'T Know:**
| Question | Why It Matters | Blocker Level |
|----------|----------------|---------------|
| **Any regulatory requirements?** (privacy laws, industry regs) | Compliance constraints | 🟡 Medium |
| **Can timesheet data leave NZ/AU for AI processing?** (cloud services) | AI model hosting location | 🟡 Medium |
| **Client confidentiality agreements?** | Data handling rules | 🟡 Medium |
| **Who owns AI-generated content?** (liability question) | Legal clarity | 🟢 Low |
| **Data retention policies?** | Storage requirements | 🟢 Low |

**Discovery Questions:**
1. "Are there any client contracts that restrict use of AI or cloud services on their project data?"
2. "Are you comfortable with AI processing happening in cloud (e.g., Azure Australia/NZ region) or does it need to be on-premises?"

---

## PRIORITIZED MEETING AGENDA (Next Session)

### 🔴 **CRITICAL (Must Answer)** - 30 minutes

**Block 1: Technical Feasibility (15 min)**
1. **ProjectWorks API:**
   - "Can we access ProjectWorks API documentation before this meeting ends?" (immediate action)
   - "Let's quickly test: can we pull a sample timesheet via API right now?" (live demo if possible)
   - "Can we see the API endpoints list or schema?"

2. **SOW Storage:**
   - "Show us where a Statement of Work is stored for an active project"
   - "How does a consultant access the SOW when invoicing?"

**Block 2: Data Samples (15 min)**
3. **Timesheet Note Examples:**
   - "Can you pull up 5 random timesheet entries from last month - we want to see real quality variance"
   - Screen share ProjectWorks timesheet view

4. **Invoice Narrative Examples:**
   - "Show us 2-3 final invoice narratives that were sent to clients (anonymized)"
   - "What does 'good' look like vs. 'needs improvement'?"

---

### 🟡 **HIGH PRIORITY (Should Answer)** - 20 minutes

**Block 3: Approval Workflow (10 min)**
5. "Walk us through the approval process step-by-step - who sees what, when?"
6. "Anna, what makes you push back on an invoice?"
7. "What's the typical turnaround time from submission to final approval?"

**Block 4: Billing Override Logic (10 min)**
8. "How often do consultants bill less than actual hours - every invoice or occasionally?"
9. "Christina, walk us through the 'wiggling' process - how do you select which timesheets to include?"
10. "How is deferred/unbilled time tracked to ensure it's not lost?"

---

### 🟢 **MEDIUM PRIORITY (Nice to Know)** - 10 minutes

**Block 5: Pilot Planning**
11. "Who would be great pilot candidates - 1-2 consultants open to testing?"
12. "How many consultants firm-wide will eventually use this?"
13. "What would convince you that the pilot is successful?"

**Block 6: Risk & Security**
14. "Tell us about a time an invoice went wrong - what happened?"
15. "Any restrictions on using cloud AI services for client data?"

---

### 🔵 **LOW PRIORITY (Defer if Time Runs Short)** - 10 minutes

**Block 7: Edge Cases & Preferences**
16. "Do consultants invoice from office, home, or on the road?"
17. "Any client-specific tone requirements for invoice narratives?"
18. "What's the hard deadline for invoices each month?"

---

## PRE-MEETING REQUESTS (Send ASAP)

**Email to Morrison Low Team:**

> Hi Matt, Christina, Anne,
>
> Thanks for the excellent discovery session! To prepare for our next meeting, could you help us with these items?
>
> **Technical Documentation:**
> 1. ProjectWorks API documentation (if available)
> 2. Access to ProjectWorks sandbox/test environment (if possible)
>
> **Sample Data (anonymized/redacted as needed):**
> 3. 10-15 timesheet note examples showing quality range (best, typical, worst)
> 4. 5-10 final invoice narratives that were sent to clients
> 5. Karen & Dan's "invoicing training notes" mentioned in our meeting
>
> **Quick Info:**
> 6. Total consultant count (firm-wide)
> 7. Where are Statements of Work typically stored?
>
> No rush - even partial answers help us hit the ground running!
>
> Cheers,
> IT360 Team

---

## RISK ASSESSMENT

### High-Risk Unknowns (Could Kill Project)
1. ❌ **ProjectWorks API insufficient** → Can't read/write necessary data → Manual process remains
2. ❌ **SOW documents inaccessible** → AI lacks scope context → Poor summarization quality
3. ❌ **Security restrictions prohibit cloud AI** → Architecture pivot required → Cost/timeline impact

### Medium-Risk Unknowns (Could Delay/Complicate)
4. ⚠️ **Timesheet note quality too poor** → AI can't summarize effectively → Behavioral change required first
5. ⚠️ **Approval workflow too complex** → System must support elaborate branching → Scope expansion

### Low-Risk Unknowns (Manageable)
6. ✅ **Multi-device support** → Can start desktop-only, add mobile later
7. ✅ **Pilot candidate identification** → Can find volunteers during rollout planning

---

## SUCCESS CRITERIA FOR NEXT MEETING

**We'll know the meeting was successful if we can answer:**
- ✅ "Can ProjectWorks API support our integration needs?" (Yes/No with evidence)
- ✅ "What does good timesheet data look like vs. bad?" (Have seen real examples)
- ✅ "Where do SOWs live and can we access them programmatically?" (Clear answer)
- ✅ "What's the approval workflow sequence?" (Documented steps)
- ✅ "Who will pilot and how do we measure success?" (Names + criteria)

**If we get these answers, we can:**
- Finalize technical architecture
- Estimate development timeline with confidence
- Complete the Product Requirements Document
- Begin UX wireframe design

---

_Next Steps: Review this analysis, refine questions, schedule next meeting with appropriate stakeholders._
