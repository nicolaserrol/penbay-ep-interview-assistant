## 01 EXECUTIVE SUMMARY

Penbay Law has prepared a substantial internal design package for an estate-planning design interview assistant: a 37-artifact set covering governing legal rules, family-structure routing, plan and scenario taxonomy, P-code priority scoring, scoring-engine rules, module flow, clause mapping, attorney confirmation workflow, output templates, and Microsoft 365 implementation decisions. The legal-design layer is rigorous. The remaining work is engineering: pressure-testing the rules for build-readiness, choosing the right Microsoft 365 surface, trimming to a buildable phase-one scope, and converting what survives into a single engineering specification that a build can run against.

This Statement of Work covers Milestone 1 only. M1 produces a signed-off build specification, an architecture decision in writing, and acceptance criteria for the build phase. Milestone 2 (Build) and Milestone 3 (Test and Handoff) will be quoted at firm fixed prices once M1 is delivered, so neither party commits to a build number against an unvalidated specification.

### Current Technology Stack

| TOOL | CURRENT USE |
|---|---|
| **Microsoft Copilot Studio** | Candidate surface for guided interview, branching logic, and role-specific prompts. |
| **Microsoft 365 Copilot** | Candidate surface for grounded reference and SharePoint-anchored Q&A. A declarative agent already exists in the package. |
| **Power Automate** | Candidate orchestration layer for scoring, routing, document generation, and approvals. |
| **Microsoft Dataverse** | Candidate structured data store for matters, people, answers, scores, and clause decisions. |
| **SharePoint Online** | Existing content store; candidate alternative to Dataverse for initial implementation. |
| **Word Templates with Merge Fields** | Candidate output engine for paralegal drafting handoff and senior attorney review summaries. |
| **Power Apps** | Candidate review surface for attorney confirmation and override workflow. |

---

## 02 SCOPE OF WORK

### WORKFLOW 1 — Build-Readiness Pass Across the 37-Artifact Package

Every artifact in the existing build package is reviewed for implementability and sorted into one of three buckets: ready as-is, requires trim or tighten before build, defer to a later phase. Each item carries a written reason. The output is a clear inventory of what the build phase will execute against and what is parked.

**Automation Deliverables:**
- Three-bucket build-readiness inventory across all 37 artifacts
- Per-artifact reasoning and any required modifications
- Reconciliation of duplicate artifact copies and version drift
- Identification of any further missing source content

**Integration Path:** Source artifacts -> validation pass -> build-readiness inventory

---

### WORKFLOW 2 — Microsoft 365 Architecture Decision

The package leaves several architecture choices open. M1 closes them in writing. The decision covers which Microsoft 365 surface the associate opens, where structured data lives, how authentication and permissions work, what the Penbay IT team owns going forward, and which Power Platform components are in scope versus skipped for phase one.

**Automation Deliverables:**
- Selected interview surface (Copilot Studio, M365 Copilot, Power Apps, or hybrid)
- Selected data layer (Dataverse versus SharePoint Lists) with reasoning
- Authentication and permission model documented against the existing tenant
- Power Platform component map: in scope, out of scope, deferred
- Update to the existing Microsoft 365 Implementation Decisions Log (12R)

**Integration Path:** Decision log review -> tenant assessment -> architecture commitment in writing

---

### WORKFLOW 3 — Phase-One Scope Boundary for the Unified Long-Term Marriage Track

The package supports multiple family-structure tracks and plan categories. Phase one targets the Unified Long-Term Marriage track only. M1 defines the exact list of modules, fields, scenarios, clause concepts, alerts, and outputs that constitute the minimum complete prototype for that track. Everything outside that line moves to an explicit phase-two backlog so future expansion does not require a rebuild.

**Automation Deliverables:**
- Unified Long-Term Marriage minimum complete prototype scope, item by item
- Phase-two backlog with reasons for deferral
- Data-model boundary that supports later expansion to other tracks without rework
- Explicit exclusions list to prevent scope drift during the build phase

**Integration Path:** Track selection -> scope cut -> phase-one boundary + phase-two backlog

---

### WORKFLOW 4 — Translated Build Specification

The output of M1 is one engineering document the build can run against without ambiguity. It consolidates the validated artifacts, the architecture decisions, the phase-one scope, and the acceptance criteria into a single contract for Milestone 2.

**Automation Deliverables:**
- Single consolidated build specification covering data model, interview flow, scoring, routing, clause triggers, attorney confirmation, outputs, and alerts
- M2 acceptance criteria drawn from the existing Test Cases and Expected Outputs (12P), trimmed to phase-one scope
- Confirmed Layer 4 attorney confirmation gates and routing hard gates
- Firm fixed-price quote for Milestone 2 (Build) and Milestone 3 (Test and Handoff), based on the validated specification

**Integration Path:** Validation output + architecture + scope -> single build spec + firm M2/M3 quote

---

## 03 PROJECT TIMELINE

**Estimated Duration:** 2 Weeks · M1 Kickoff -> Build Spec Sign-Off

| | FOCUS | DELIVERABLES |
|---|---|---|
| **WEEK 1** | Validation and Architecture | Full read of priority artifacts (00, 04, 06, 11, 12C, 12D, 12E, 12H, 15). Build-readiness pass underway. Architecture decision draft. Mid-week working session with Penbay Law to walk surface options and tenant constraints. |
| **WEEK 2** | Scope Cut, Build Spec, M2/M3 Quote | Phase-one scope boundary finalized. Phase-two backlog written. Single consolidated build specification delivered. M2 acceptance criteria documented. Firm fixed-price quote for M2 and M3 issued. Sign-off session and milestone handoff. |

---

## 05 INCLUSIONS & EXCLUSIONS

### What's Included ✓
- Full validation pass across all 37 artifacts in the existing build package
- Three-bucket build-readiness inventory with per-artifact reasoning
- Reconciliation of duplicate artifact copies (12B, 12P) and identification of any further gaps
- Microsoft 365 architecture decision in writing, including surface, data layer, auth model, and Power Platform component scope
- Unified Long-Term Marriage phase-one scope boundary, item by item
- Phase-two backlog covering all deferred items with reasons
- Single consolidated build specification covering data model, interview flow, scoring, routing, clause triggers, attorney confirmation, outputs, and alerts
- Acceptance criteria for Milestone 2, drawn from the existing Test Cases and Expected Outputs (12P) trimmed to phase-one scope
- Confirmed Layer 4 attorney confirmation gates and routing hard gates
- Firm fixed-price quote for Milestone 2 (Build) and Milestone 3 (Test and Handoff)
- Mid-week working session and end-of-engagement sign-off session
- All M1 deliverables delivered as Word and PDF documents

### What's Not Included ✗
- Any actual application build (covered in Milestone 2)
- Testing, training, and handoff (covered in Milestone 3)
- Multi-scenario tracks beyond Unified Long-Term Marriage (deferred to phase two)
- MAPT and EP-APP scenarios (deferred to phase two)
- Custom enterprise integrations beyond the Microsoft 365 stack
- DecisionVault import or other third-party intake integrations
- Production deployment, ALM pipelines, or environment promotion automation
- Legal content review or revision of the existing artifacts (Penbay Law retains full ownership of legal logic)
- Final clause language drafting (clauses remain placeholder until legal review)
- Power BI reporting or analytics dashboards