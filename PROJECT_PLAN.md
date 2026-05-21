# Penbay EP Interview Assistant — Comprehensive Project Plan

---

## 1. Project Overview

Penbay Law has developed a rigorous 37-artifact internal design package for an **estate-planning (EP) design interview assistant** — a system that guides associates through structured client interviews, scores and routes matters based on family structure and plan taxonomy, triggers clause selections, surfaces attorney confirmation gates, and generates paralegal-ready drafting handoffs. The legal-design layer is complete; the remaining work is engineering.

**End Users:**
- **Estate-planning associates** — conduct guided client interviews through the system
- **Senior attorneys** — review scored recommendations, confirm or override clause selections at Layer 4 gates
- **Paralegals** — receive generated drafting summaries with merge-field outputs for Word templates
- **Penbay IT** — maintain the M365 environment, manage permissions, and own ongoing operations

**Problem Solved:** Today the EP interview process is manual, inconsistent, and dependent on individual attorney knowledge. This system codifies Penbay's legal logic into a repeatable, auditable, scored workflow — reducing errors, ensuring compliance with internal design rules, and accelerating matter throughput.

**Success Looks Like:**
- An associate can complete a full Unified Long-Term Marriage interview end-to-end in the system
- P-code scoring and routing produce correct recommendations against known test cases (artifact 12P)
- Attorney confirmation gates fire at the right moments with the right data
- Word-template outputs are generated automatically and match expected clause selections
- The system runs entirely within Penbay's existing Microsoft 365 tenant

---

## 2. Tech Stack Recommendation

| Technology | Purpose | Why This Over Alternatives |
|---|---|---|
| **Microsoft Copilot Studio** | Guided interview surface with branching logic and role-specific prompts | Native M365 integration; supports topic-based branching and adaptive cards; avoids custom frontend build cost. Preferred over Power Apps for interview flow because of its conversational UX and built-in NLU. |
| **Microsoft 365 Copilot (Declarative Agent)** | Grounded reference and SharePoint-anchored Q&A for attorneys | A declarative agent already exists in the package. Provides grounded answers from the EP Design System 2.0 library without hallucination risk. |
| **Power Automate** | Scoring engine, routing logic, document generation, approval workflows | Native connector ecosystem (Dataverse, SharePoint, Outlook, Approvals). Avoids custom API development. Cloud flows handle the orchestration complexity of P-code scoring and multi-step routing. |
| **Microsoft Dataverse** | Structured data store for matters, people, answers, scores, and clause decisions | Relational model with row-level security, audit logging, and native Power Platform binding. Preferred over SharePoint Lists for structured/relational data — SharePoint Lists lack referential integrity and scale poorly for scoring queries. |
| **SharePoint Online** | Content store for design artifacts, clause library, and merge templates | Already in use at Penbay. Houses the EP Design System 2.0 library. Not a replacement for Dataverse but complements it for document storage. |
| **Word Templates (Merge Fields)** | Paralegal drafting handoff and senior attorney review summaries | Matches Penbay's existing document workflow. Power Automate's Word Online connector supports merge-field population natively. |
| **Power Apps (Model-Driven)** | Attorney confirmation and override workflow (Layer 4 gates) | Model-driven apps bind directly to Dataverse tables. Provides form-based review UX with role-based views. Better than canvas apps here because the data model drives the UI. |

**Procurement / Licensing Flags:**
- Copilot Studio requires per-tenant licensing (confirm Penbay's current plan includes it)
- Microsoft 365 Copilot licenses are needed for declarative agent users
- Power Automate premium connectors (Dataverse) require Power Automate Premium or per-flow licensing
- No third-party tools or external SaaS required

---

## 3. Phase Breakdown

### Phase 1 — Setup & Foundation (M1: Build-Readiness Validation)

**Entry Condition:** SOW signed, Penbay Law engagement confirmed.

**Work:**
- Review all 37 artifacts for implementability (three-bucket sort: ready / needs-trim / defer)
- Reconcile duplicate artifact copies and version drift (12B, 12P noted)
- Lock M365 architecture decision (interview surface, data layer, auth model, Power Platform scope)
- Secure all 14 platform access items for builder (robert@mvp.dev)
- Define Unified Long-Term Marriage phase-one scope boundary
- Produce single consolidated build specification
- Deliver M2/M3 acceptance criteria and firm fixed-price quote

**Exit Condition:** Build specification signed off by Penbay Law. Architecture decision committed in writing. All platform access provisioned and verified. M2/M3 quote accepted.

---

### Phase 2 — Backend / Data Layer (M2 Build — Part 1)

**Entry Condition:** M1 sign-off received. Builder has full platform access. Dev environment provisioned.

**Work:**
- Implement Dataverse schema (matters, people, answers, scores, clause decisions, alerts)
- Build P-code priority scoring engine in Power Automate
- Build family-structure routing logic
- Implement clause-trigger mapping flows
- Build attorney confirmation gate logic (Layer 4 hard gates)
- Configure SharePoint document libraries for merge templates and clause library
- Set up approval flows for attorney review workflow

**Exit Condition:** All scoring, routing, and clause-trigger flows return correct results against test fixtures from artifact 12P. Dataverse tables populated with anonymized test data. Approval flows fire correctly.

---

### Phase 3 — Frontend / Interview Flow (M2 Build — Part 2)

**Entry Condition:** Dataverse schema deployed. Scoring/routing flows operational.

**Work:**
- Build Copilot Studio interview topics for Unified Long-Term Marriage track
- Implement module-by-module branching logic with adaptive cards
- Build role-specific prompts and field validation
- Create alert surfacing within the interview flow
- Build Power Apps model-driven app for attorney confirmation/override
- Configure declarative agent for grounded EP reference
- Implement Word template merge-field population via Power Automate

**Exit Condition:** An associate can complete the full Unified Long-Term Marriage interview end-to-end in Copilot Studio. Attorney confirmation app shows correct data. Word outputs generate with correct merge fields.

---

### Phase 4 — Integration & Hardening (M2 Build — Part 3)

**Entry Condition:** Interview flow and backend flows independently functional.

**Work:**
- End-to-end integration testing (interview → scoring → routing → confirmation → output)
- Edge case testing against all Unified Long-Term Marriage scenarios in 12P
- Error handling for flow failures, timeout scenarios, and data validation
- Performance testing under concurrent usage
- Security review (row-level security, DLP compliance, data residency)
- Fix defects from integration testing

**Exit Condition:** All acceptance criteria from the build spec pass in the dev/staging environment. No critical or high-severity bugs open.

---

### Phase 5 — Launch & Handoff (M3: Test and Handoff)

**Entry Condition:** All M2 acceptance criteria pass. Staging environment stable.

**Work:**
- UAT with Penbay associates, attorneys, and paralegals
- User training sessions
- Admin/IT documentation and runbook
- Production environment provisioning and promotion
- Production deployment and smoke testing
- Handoff session with Penbay IT and maintenance team
- Phase-two backlog handoff with expansion roadmap

**Exit Condition:** UAT sign-off from Penbay Law. Production deployment successful. Training completed. Documentation delivered. Maintenance team briefed.

---

## 4. Task List Per Phase

### Phase 1 — Setup & Foundation (M1)

| # | Task | Description | Effort | Assignee Role | Dependencies |
|---|------|-------------|--------|---------------|--------------|
| 1.1 | Kick-off session | Align on SOW scope, timeline, communication cadence, access requirements | XS (< 2h) | Project Lead | — |
| 1.2 | Platform access request | Submit all 14 access items to Penbay IT (identity, SharePoint, Power Platform, Copilot Studio, Dataverse, DLP, test data, IT contact) | S (2–4h) | Project Lead | 1.1 |
| 1.3 | Platform access verification | Verify each access item is provisioned and functional; resolve blockers | M (4–8h) | Full-Stack Dev | 1.2 |
| 1.4 | Priority artifact deep read | Read and annotate priority artifacts: 00, 04, 06, 11, 12C, 12D, 12E, 12H, 15 | L (1–2d) | Full-Stack Dev | 1.3 |
| 1.5 | Full 37-artifact validation pass | Review remaining artifacts; sort all 37 into ready / needs-trim / defer buckets with reasoning | XL (3–5d) | Full-Stack Dev | 1.4 |
| 1.6 | Duplicate reconciliation | Reconcile duplicate copies of 12B, 12P, and any other version-drifted artifacts | S (2–4h) | Full-Stack Dev | 1.5 |
| 1.7 | Build-readiness inventory | Produce three-bucket inventory document with per-artifact reasoning | M (4–8h) | Full-Stack Dev | 1.6 |
| 1.8 | Architecture decision draft | Draft M365 architecture decision: interview surface, data layer, auth model, Power Platform scope | M (4–8h) | Full-Stack Dev | 1.4 |
| 1.9 | Mid-week working session | Walk architecture options and tenant constraints with Penbay Law | S (2–4h) | Project Lead | 1.8 |
| 1.10 | Architecture decision finalization | Finalize and document architecture decision; update 12R | S (2–4h) | Full-Stack Dev | 1.9 |
| 1.11 | Phase-one scope boundary | Define exact modules, fields, scenarios, clauses, alerts, outputs for Unified Long-Term Marriage | M (4–8h) | Full-Stack Dev | 1.7, 1.10 |
| 1.12 | Phase-two backlog | Document all deferred items with deferral reasoning | S (2–4h) | Full-Stack Dev | 1.11 |
| 1.13 | Data-model boundary design | Design Dataverse schema boundaries that support later expansion without rework | M (4–8h) | Full-Stack Dev | 1.11 |
| 1.14 | Consolidated build specification | Write single engineering spec: data model, interview flow, scoring, routing, clause triggers, confirmation, outputs, alerts | XL (3–5d) | Full-Stack Dev | 1.7, 1.10, 1.11, 1.13 |
| 1.15 | Acceptance criteria (M2) | Extract and trim test cases from 12P to phase-one scope; document acceptance criteria | M (4–8h) | Full-Stack Dev | 1.11, 1.14 |
| 1.16 | M2/M3 quote | Produce firm fixed-price quote for Build and Test/Handoff milestones | M (4–8h) | Project Lead | 1.14, 1.15 |
| 1.17 | Sign-off session | Present build spec, walk acceptance criteria, obtain sign-off | S (2–4h) | Project Lead | 1.14, 1.15, 1.16 |

### Phase 2 — Backend / Data Layer (M2 Build — Part 1)

| # | Task | Description | Effort | Assignee Role | Dependencies |
|---|------|-------------|--------|---------------|--------------|
| 2.1 | Dev environment setup | Provision "EP Design Sheet (Dev)" Power Platform environment; verify roles | S (2–4h) | DevOps | 1.17 |
| 2.2 | Dataverse schema — core tables | Create tables: Matters, People, Relationships, Answers | L (1–2d) | Full-Stack Dev | 2.1 |
| 2.3 | Dataverse schema — scoring tables | Create tables: Scores, P-Codes, Priority Rankings | M (4–8h) | Full-Stack Dev | 2.2 |
| 2.4 | Dataverse schema — clause tables | Create tables: Clause Decisions, Clause Library, Alerts | M (4–8h) | Full-Stack Dev | 2.2 |
| 2.5 | Dataverse security model | Configure row-level security, business units, security roles | M (4–8h) | Full-Stack Dev | 2.2, 2.3, 2.4 |
| 2.6 | Test data seeding | Load 3–5 anonymized Unified Long-Term Marriage matters into Dataverse | S (2–4h) | Full-Stack Dev | 2.5 |
| 2.7 | P-code scoring engine | Build Power Automate cloud flow implementing P-code priority scoring rules | XL (3–5d) | Full-Stack Dev | 2.3, 2.6 |
| 2.8 | Family-structure routing logic | Build routing flow based on family-structure classification | L (1–2d) | Full-Stack Dev | 2.2, 2.6 |
| 2.9 | Clause-trigger mapping flows | Build flows that map scored answers to clause selections | L (1–2d) | Full-Stack Dev | 2.4, 2.7 |
| 2.10 | Attorney confirmation gate logic | Build Layer 4 hard gates and routing rules in Power Automate | L (1–2d) | Full-Stack Dev | 2.7, 2.8 |
| 2.11 | SharePoint template library setup | Configure document libraries for Word merge templates and clause library files | S (2–4h) | Full-Stack Dev | 2.1 |
| 2.12 | Approval flow configuration | Set up Approvals connector flows for attorney review workflow | M (4–8h) | Full-Stack Dev | 2.10 |
| 2.13 | Backend validation against test fixtures | Run all scoring/routing/clause flows against 12P test cases | L (1–2d) | QA | 2.7, 2.8, 2.9, 2.10 |

### Phase 3 — Frontend / Interview Flow (M2 Build — Part 2)

| # | Task | Description | Effort | Assignee Role | Dependencies |
|---|------|-------------|--------|---------------|--------------|
| 3.1 | Copilot Studio bot creation | Create bot, configure authentication, connect to Dataverse | S (2–4h) | Full-Stack Dev | 2.5 |
| 3.2 | Interview topics — intake modules | Build opening modules: client identification, family structure, marital status | L (1–2d) | Full-Stack Dev | 3.1 |
| 3.3 | Interview topics — asset modules | Build asset inventory, valuation, and characterization modules | L (1–2d) | Full-Stack Dev | 3.2 |
| 3.4 | Interview topics — planning modules | Build plan-category selection, scenario branching, and goal-setting modules | L (1–2d) | Full-Stack Dev | 3.3 |
| 3.5 | Interview topics — review modules | Build summary review, confirmation, and submission modules | M (4–8h) | Full-Stack Dev | 3.4 |
| 3.6 | Branching logic implementation | Implement conditional topic routing based on answers and family structure | L (1–2d) | Full-Stack Dev | 3.2, 3.3, 3.4 |
| 3.7 | Adaptive cards for complex inputs | Design and implement adaptive cards for multi-field inputs, tables, selections | L (1–2d) | Full-Stack Dev | 3.2 |
| 3.8 | Alert surfacing in interview | Display real-time alerts during interview based on answer-triggered rules | M (4–8h) | Full-Stack Dev | 3.6, 2.9 |
| 3.9 | Power Apps — attorney confirmation app | Build model-driven app for attorney review, confirmation, and override | XL (3–5d) | Full-Stack Dev | 2.10, 2.12 |
| 3.10 | Word template merge-field mapping | Map Dataverse fields to Word template merge fields; build generation flow | L (1–2d) | Full-Stack Dev | 2.11, 2.9 |
| 3.11 | Declarative agent configuration | Configure M365 Copilot declarative agent grounded on EP Design System 2.0 | M (4–8h) | Full-Stack Dev | 2.11 |
| 3.12 | Interview flow walkthrough | End-to-end walkthrough of complete interview with test data | M (4–8h) | QA | 3.5, 3.6, 3.8 |

### Phase 4 — Integration & Hardening (M2 Build — Part 3)

| # | Task | Description | Effort | Assignee Role | Dependencies |
|---|------|-------------|--------|---------------|--------------|
| 4.1 | End-to-end integration testing | Test full flow: interview → scoring → routing → confirmation → output | XL (3–5d) | QA | 3.12, 2.13 |
| 4.2 | Acceptance criteria validation | Run every M2 acceptance criterion from the build spec against staging | L (1–2d) | QA | 4.1 |
| 4.3 | Edge case testing | Test boundary conditions, unusual family structures within ULTM, missing data scenarios | L (1–2d) | QA | 4.1 |
| 4.4 | Error handling & recovery | Add flow error handling, retry logic, user-facing error messages | L (1–2d) | Full-Stack Dev | 4.1 |
| 4.5 | Performance testing | Test concurrent interview sessions, flow execution times, Dataverse query performance | M (4–8h) | Full-Stack Dev | 4.1 |
| 4.6 | Security review | Validate row-level security, DLP compliance, data residency, auth model | M (4–8h) | Full-Stack Dev | 4.1 |
| 4.7 | Bug fixes — critical/high | Resolve all critical and high-severity defects from testing | L (1–2d) | Full-Stack Dev | 4.1, 4.2, 4.3 |
| 4.8 | Regression testing | Re-run acceptance criteria after bug fixes | M (4–8h) | QA | 4.7 |

### Phase 5 — Launch & Handoff (M3)

| # | Task | Description | Effort | Assignee Role | Dependencies |
|---|------|-------------|--------|---------------|--------------|
| 5.1 | UAT environment preparation | Provision UAT environment, promote solution, load test data | M (4–8h) | DevOps | 4.8 |
| 5.2 | UAT execution | Penbay associates/attorneys run through test scenarios; defects logged | XL (3–5d) | QA | 5.1 |
| 5.3 | UAT defect resolution | Fix issues discovered during UAT | L (1–2d) | Full-Stack Dev | 5.2 |
| 5.4 | User training — associates | Train associates on interview flow, Copilot Studio usage | M (4–8h) | Project Lead | 5.2 |
| 5.5 | User training — attorneys | Train attorneys on confirmation app, override workflow | M (4–8h) | Project Lead | 5.2 |
| 5.6 | Admin/IT documentation | Write runbook: environment config, DLP policies, security roles, monitoring, troubleshooting | L (1–2d) | Full-Stack Dev | 4.6 |
| 5.7 | User guide | Write end-user guide for associates and attorneys | L (1–2d) | Project Lead | 5.4, 5.5 |
| 5.8 | Production environment provisioning | Provision production environment, configure security, DLP | M (4–8h) | DevOps | 5.3 |
| 5.9 | Production deployment | Deploy solution to production, configure connections, validate | M (4–8h) | Full-Stack Dev | 5.8 |
| 5.10 | Production smoke testing | Run core test scenarios in production | S (2–4h) | QA | 5.9 |
| 5.11 | UAT sign-off | Obtain formal sign-off from Penbay Law stakeholders | XS (< 2h) | Project Lead | 5.2, 5.3 |
| 5.12 | Handoff session | Walk Penbay IT through operations, monitoring, and phase-two backlog | M (4–8h) | Project Lead | 5.6, 5.9, 5.11 |
| 5.13 | Phase-two backlog handoff | Deliver documented backlog: multi-scenario tracks, MAPT, EP-APP, analytics | S (2–4h) | Project Lead | 5.12 |

---

## 5. Timeline & Milestones

| Week | Phase | Key Activities | Milestone |
|------|-------|----------------|-----------|
| **1** | Phase 1 | Kick-off. Platform access requests submitted. Priority artifact deep read (00, 04, 06, 11, 12C, 12D, 12E, 12H, 15). Build-readiness pass begins. | **M1 Kickoff** |
| **2** | Phase 1 | Architecture decision finalized. Phase-one scope boundary locked. Build spec drafted. M2/M3 quote issued. Sign-off session. | **M1 Complete — Build Spec Sign-Off** |
| **3** | Phase 2 | Dev environment provisioned. Dataverse schema deployed (core, scoring, clause tables). Security model configured. Test data loaded. | — |
| **4** | Phase 2 | P-code scoring engine built. Family-structure routing logic built. Clause-trigger mapping flows built. | — |
| **5** | Phase 2 | Attorney confirmation gates built. Approval flows configured. Backend validation against 12P test fixtures. | **Backend Complete** |
| **6** | Phase 3 | Copilot Studio bot created. Interview topics built (intake, asset modules). Adaptive cards designed. | **Mid-Point Review** |
| **7** | Phase 3 | Planning and review interview modules built. Branching logic implemented. Alert surfacing wired up. | — |
| **8** | Phase 3 | Attorney confirmation Power App built. Word template merge-field flows built. Declarative agent configured. | **Interview Flow Complete** |
| **9** | Phase 4 | End-to-end integration testing. Acceptance criteria validation. Edge case testing. | **Feature Freeze** |
| **10** | Phase 4 | Error handling hardened. Performance testing. Security review. Bug fixes and regression testing. | **M2 Complete — Build Acceptance** |
| **11** | Phase 5 | UAT environment prepared. UAT execution begins with Penbay associates and attorneys. | **UAT Start** |
| **12** | Phase 5 | UAT defect resolution. User training (associates + attorneys). Documentation delivered. | — |
| **13** | Phase 5 | Production provisioning and deployment. Smoke testing. UAT sign-off. Handoff session. Phase-two backlog delivered. | **M3 Complete — Go-Live** |

**Hard Deadlines / External Dependencies:**
- Platform access (14 items) must be provisioned by end of Week 1 — any delay here cascades everything
- Anonymized test data (3–5 ULTM matters) and clause library export needed from Penbay by start of Week 3
- Mid-week working session with Penbay Law required in Week 1 for architecture decision
- DLP policy clearance from Penbay IT required before Power Automate flows can use Dataverse/SharePoint/Outlook connectors
- Copilot Studio and M365 Copilot licensing must be confirmed before Week 3

---

## 6. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| **Platform access delays** — Penbay IT is slow to provision the 14 access items, especially guest account MFA, DLP clearance, and Copilot Studio access | High | High | Submit all 14 requests on Day 1. Establish direct IT contact (item 14). Escalate blockers through Penbay Law sponsor. Have fallback tasks (artifact review) that don't require platform access. |
| **Scope creep into multi-scenario tracks** — Stakeholders push to include scenarios beyond Unified Long-Term Marriage during the build | Medium | High | Lock phase-one scope boundary in M1 with explicit exclusions list. Maintain phase-two backlog document. Require formal change request for any scope additions. |
| **Artifact ambiguity or gaps** — Some of the 37 artifacts have conflicting versions, missing content, or untestable rules | Medium | Medium | Build-readiness pass (Workflow 1) specifically addresses this. Flag gaps early in Week 1. The "needs-trim" bucket provides a structured path to resolve ambiguities with Penbay Law before build begins. |
| **Copilot Studio limitations** — Complex branching logic or adaptive card requirements exceed Copilot Studio's current capabilities | Medium | High | Architecture decision (Workflow 2) evaluates surface options explicitly. If Copilot Studio can't handle a specific flow, fall back to Power Apps canvas app for that module. Design interview topics to be modular so individual modules can be swapped. |
| **Dataverse vs SharePoint data layer mismatch** — Choosing the wrong data layer leads to rework when scaling to phase two | Low | High | Architecture decision locks this in writing during M1. Design Dataverse schema with explicit expansion boundaries. If starting on SharePoint Lists as interim, document migration path to Dataverse. |
| **Power Automate flow complexity** — P-code scoring engine and multi-step routing exceed flow performance or expression limits | Medium | Medium | Prototype scoring flow early in Week 3. If flow complexity is too high, decompose into child flows. Consider Dataverse calculated columns or business rules for simpler scoring components. |
| **Legal content not ready for testing** — Clause library export or test data not provided by Penbay Law on time | Medium | Medium | Request test data and clause library in the access request (items 12–13). Use placeholder/synthetic data for initial development; swap real data for validation testing. |
| **Licensing gaps** — Required M365 licenses (Copilot Studio, M365 Copilot, Power Automate Premium) not available in tenant | Low | High | Confirm licensing in the architecture decision working session (Week 1). If gaps exist, identify trial or developer licenses for the dev environment. |

---

## 7. Definition of Done

- [ ] All 37 artifacts reviewed and sorted into ready / needs-trim / defer buckets with written reasoning (M1)
- [ ] M365 architecture decision documented and signed off: interview surface, data layer, auth model, Power Platform scope (M1)
- [ ] Unified Long-Term Marriage phase-one scope boundary defined item by item with explicit exclusions (M1)
- [ ] Single consolidated build specification delivered covering data model, interview flow, scoring, routing, clause triggers, attorney confirmation, outputs, and alerts (M1)
- [ ] All M2 acceptance criteria (drawn from 12P, trimmed to phase-one scope) pass in staging environment (M2)
- [ ] P-code scoring engine returns correct priority rankings for all test fixtures (M2)
- [ ] Layer 4 attorney confirmation gates fire correctly with routing hard gates enforced (M2)
- [ ] Word template outputs generate with correct merge fields for all test scenarios (M2)
- [ ] End-to-end interview flow completable by an associate for Unified Long-Term Marriage track (M2)
- [ ] No critical or high-severity bugs open (M2)
- [ ] Security review passed: row-level security, DLP compliance, auth model validated (M2)
- [ ] UAT sign-off received from Penbay Law stakeholders (M3)
- [ ] User training completed for associates and attorneys (M3)
- [ ] Admin documentation and IT runbook delivered (M3)
- [ ] Production deployment successful with smoke tests passing (M3)
- [ ] Handoff session completed with Penbay IT maintenance team (M3)
- [ ] Phase-two backlog delivered with expansion roadmap (multi-scenario tracks, MAPT, EP-APP) (M3)
- [ ] All deliverables delivered as Word and PDF documents per SOW requirement (All milestones)

---

**Note:** Section 04 (Pricing/Investment) was not present in the provided documents — it appears to be missing from the extracted SOW images. The M2/M3 firm fixed-price quote will be produced as a deliverable of M1, per the SOW terms. If you have the pricing section, share it and I'll incorporate it.
