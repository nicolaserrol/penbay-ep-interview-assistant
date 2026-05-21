# Onboarding Checklist Verification Guide — Penbay Law Tenant

This guide walks through how to verify each item from `ONBARODING_CHECKLIST.md` is complete and ready, with every step explicitly anchored to the `penbaylaw.com` tenant.

**Account in use:** `mvp-projects@goldenaccessventures.com` (guest in penbaylaw.com)

**Critical lesson learned:** Your account's *home* tenant is `goldenaccessventures.com`. Microsoft web tools default to your home tenant on sign-in, NOT penbay. **You must actively switch tenant context to penbaylaw.com before every step** that touches Power Platform, Copilot Studio, Dataverse, M365 Copilot, or Teams. Failing to do this lands you in the wrong tenant silently — that is how the original EP Design Sheet (Dev) ended up provisioned in GAV instead of penbay.

---

## Prerequisite — Tenant Context Switching (READ FIRST)

Before doing anything in any Microsoft web tool below, force your tenant context to penbay. This is the single most important habit for the rest of the checklist.

### One-time setup

1. **Obtain penbaylaw.com's tenant GUID.** Ask your IT contact (#14). Alternatively, sign in to `https://portal.office.com` in penbay context and check Microsoft Entra admin center → Overview → Tenant ID. Record it here:
   - **Penbay Law tenant GUID:** `f3db2981-1aec-46c0-a3b0-85174ee5c5e0`

2. **Bookmark tenant-scoped URLs.** Save these in your browser, replacing `f3db2981-1aec-46c0-a3b0-85174ee5c5e0` with the GUID above:
   - Power Platform admin: `https://admin.powerplatform.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`
   - Power Apps maker: `https://make.powerapps.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`
   - Power Automate maker: `https://make.powerautomate.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`
   - Copilot Studio: `https://copilotstudio.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`
   - M365 Copilot: `https://copilot.cloud.microsoft/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`
   - Teams (web): `https://teams.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`

### Before every step that touches a Microsoft web tool

1. Open the tool using the **tenant-scoped bookmark** (not the bare URL).
2. After sign-in, look at the top-right avatar / account chip. It must read:
   > **Penbay Law**
3. If it reads **"Golden Access Ventures"** or anything else: sign out, close the tab, reopen the tenant-scoped URL, and try again.
4. Alternative if URL param doesn't switch context: click avatar → **Switch directory** → choose **Penbay Law**.

### Verification habit

- **Before starting any item below, confirm the tenant chip reads "Penbay Estate Planning Law Cent…"** (the full display name of the penbaylaw.com tenant — Microsoft truncates it in the UI).
- **If at any point the chip flips back to "Golden Access Ventures" mid-task, STOP** and re-switch before continuing. Otherwise you risk creating resources in the wrong tenant again.

### Expected behavior for guest accounts in penbay tenant

When you successfully switch to penbay tenant, you may notice things that look like errors but are actually normal:

- **M365 Apps panel** at `https://portal.office.com` will show: *"This is unavailable due to your account permissions and company's settings"*. This is expected — guest accounts don't get M365 app licenses in the host tenant. ✅ Not a blocker.
- **Empty environment list** in Power Platform admin center until IT provisions the env. ✅ Expected.
- **Limited dashboard tiles** in M365 admin pages. ✅ Expected for guests.

The only things you NEED to work in penbay context are: SharePoint (already ✅), Power Platform admin (after IT grants access), Copilot Studio (after IT assigns license), and M365 Copilot agent registration UI (after IT grants permission). Everything else being limited is by design and not a problem.

---

## 1. Identity — Licensed/Guest Account + MFA in Penbay Tenant

**Definition of done:**
- `mvp-projects@goldenaccessventures.com` can sign in *to the penbaylaw.com tenant context*.
- MFA is enrolled and required at sign-in to penbay.
- The account appears in penbaylaw.com's Entra ID directory (as a guest).

**Step-by-step check (penbay-scoped):**
1. Open a private/incognito browser window.
2. Go to `https://portal.office.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0` (with the GUID filled in).
3. Sign in as `mvp-projects@goldenaccessventures.com`.
4. Complete the MFA challenge.
   - If MFA is not enforced, go to `https://mysignins.microsoft.com/security-info` (penbay context) and confirm at least one method is registered.
5. Click avatar (top right) → confirm chip reads **Penbay Law**.
6. Go to `https://myaccount.microsoft.com` → **Organizations** → confirm **penbaylaw.com** is listed as one of your orgs.

**Pass criteria:**
- Sign-in succeeds against penbay tenant.
- MFA enforced.
- penbaylaw.com listed under your organizations.

**If it fails:** Email your IT contact (#14) with the exact error and screenshot.

---

## 2–4. SharePoint Access ✅ ALREADY CONFIRMED (inherently in penbay tenant)

**Status:** Confirmed. SharePoint URL is `penbaylawcom.sharepoint.com`, which is inherently penbaylaw.com — no tenant ambiguity possible. These items are genuinely complete.

**Recorded:**
- ✅ Read + edit access to the Design System 2.0 library.
- ✅ Read access to related folders (12F, 12K, 12L templates / clause library files).
- ✅ Permission to create a subfolder (`_engineering-artifacts/` recommended).

---

## ⚠️ Open gate: Project Owner Decision (blocks #5–#10)

Before touching items #5–#10, get the project owner's explicit answer on:

**Question:** *The Power Platform environment work needs to live somewhere. Should we (Path 1) provision a new environment in your penbaylaw.com tenant — which means your IT side creates it and grants me System Administrator — or (Path 2) keep the dev work in my GAV tenant and hand over artifacts later?*

- **Path 1** = clean end-to-end, all artifacts in your tenant from day one. Requires IT action.
- **Path 2** = faster to start, but adds an integration step later. Item #10 (declarative agent) is harder under Path 2.

**This guide assumes Path 1 going forward.** If Path 2 is chosen, re-scope items #5–#10 accordingly.

---

## 5. Power Platform Environment (in Penbay Tenant)

**Status:** ⚠️ Pending. The existing EP Design Sheet (Dev) lives in **goldenaccessventures.com**, not penbay. A new environment must be created in penbay.

**Definition of done:**
- A Power Platform environment named **EP Design Sheet (Dev)** exists in penbaylaw.com tenant.
- Dataverse is provisioned.
- `mvp-projects@goldenaccessventures.com` has System Administrator (or Maker + Customizer) on it.

**Step-by-step:**

### Step 5.1 — Send IT the comprehensive access brief

This is the **one-shot ask** that covers everything #5–#11 need from penbay IT. Sending it now (rather than item-by-item) saves multiple round-trips. Drop-in template:

> **Subject:** Access requests for EP Design Sheet MVP build (`mvp-projects@goldenaccessventures.com`)
>
> Hi [name],
>
> [Project owner] connected us — I'm the engineer building the EP Design Sheet MVP for Penbay Law. You've already granted my guest account `mvp-projects@goldenaccessventures.com` access to the 37-artifact SharePoint library in the LegalTechnicalDepartment site, which is working great. To do the rest of the build (per the SOW — Copilot Studio, M365 Copilot, Power Automate, Dataverse, Power Apps), I need additional access in your tenant. Listing everything here so you can grant in one pass.
>
> **1. Power Platform environment**
> - Create a new environment named **EP Design Sheet (Dev)** in penbaylaw.com tenant.
> - Sandbox type preferred (Developer-type is OK if Sandbox is restricted by policy).
> - Dataverse enabled.
> - Add `mvp-projects@goldenaccessventures.com` as **System Administrator** on the environment.
>
> **2. License assignments to the guest account**
> Please assign these licenses to `mvp-projects@goldenaccessventures.com` in your tenant:
> - **Power Apps Premium** (or per-app plan) — for canvas/model-driven apps and Dataverse access.
> - **Power Automate Premium** — for premium connectors and approval flows.
> - **Microsoft Copilot Studio** — separate license/add-on; required for building the guided-interview agent.
> - **Microsoft 365 Copilot** — required for registering and testing the declarative agent.
>
> If guest-account license assignment is restricted in your tenant, an alternative is to create a contractor/service account (e.g., `mvp-build@penbaylaw.com`) with these licenses; I'll use that account instead of my guest identity.
>
> **3. DLP / connector policy**
> Please confirm none of these connectors are blocked by your DLP policy in the EP Design Sheet (Dev) environment:
> - SharePoint
> - Office 365 Outlook
> - Approvals
> - Microsoft Dataverse
>
> **4. M365 Copilot custom agent permission**
> Please confirm I'm permitted to upload and test a custom declarative agent (`EP Design System 2.0 agent.agent`) against the LegalTechnicalDepartment SharePoint site. If M365 Copilot rollout is owned by someone else on your side, please loop them in or share their contact.
>
> **5. (Conditional) Teams test team**
> If attorney confirmation will happen in Teams (per the design package), please add me to a test team in your tenant. If it'll be email-based, ignore this item.
>
> **Scope note:** SOW is anchored to the Microsoft 365 stack — Copilot Studio, M365 Copilot, Power Automate, Dataverse, SharePoint, Power Apps. No third-party integrations or custom Azure resources requested for phase one.
>
> Happy to jump on a 15-minute call if easier than back-and-forth. Thanks for the help.

**Why one combined brief instead of per-item:** Items #5 (environment), #6 (roles), #7 (DLP), #8 (Copilot Studio license), #9 (Dataverse), #10 (declarative agent), and #11 (Teams) all need a different switch flipped by penbay IT. Asking once is faster than asking five times.

### Step 5.2 — Wait for IT confirmation
Hard blocker. Don't proceed until IT confirms environment exists and your access is in place.

### Step 5.3 — Verify env exists in penbay
1. Open the tenant-scoped admin URL: `https://admin.powerplatform.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. **Confirm the account chip reads "Penbay Law"** before doing anything else.
3. In Environments, confirm **EP Design Sheet (Dev)** appears.
4. Click into it. Note the **Tenant** / **Organization** info — should be Penbay Law.
5. Confirm Dataverse URL is shown (will be a `https://orgXXXXX.crm.dynamics.com/` distinct from the GAV one).
6. Record:
   - Environment ID: `__________`
   - Organization ID: `__________`
   - Dataverse URL: `__________`
   - Region: `__________`
   - Type: `__________` (Developer / Sandbox / Production)

**Pass criteria:** Env visible in penbay tenant context, Dataverse provisioned, details recorded.

---

## 6. Environment Maker + System Customizer Roles (in Penbay Env)

**Tenant Context:** Must be in Penbay Law. Verify chip before starting.

**Step-by-step:**

### Step 6.1 — Role check (informational)
1. Open `https://admin.powerplatform.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. Confirm tenant chip = **Penbay Law**.
3. Click into **EP Design Sheet (Dev)** → **Settings → Users + permissions → Users**.
4. Find `mvp-projects@goldenaccessventures.com`.
5. Look at **Direct Assigned Roles**. Acceptable:
   - **System Administrator** is listed → covered (superset of both target roles).
   - **Environment Maker** AND **System Customizer** both listed → covered.

### Step 6.2 — Functional smoke test (decisive)
1. Open `https://make.powerapps.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. Confirm tenant chip = **Penbay Law**.
3. Switch to **EP Design Sheet (Dev)** in the environment picker.
4. Click **Solutions → + New solution**.
5. If dialog opens cleanly (accepts name/publisher), privileges are working.
6. **Cancel** — do not actually create the solution.

**Pass criteria:** Role list shows expected role OR smoke test succeeds.

**If it fails:** Loop IT — they need to add System Administrator (or Maker + Customizer) to your user in penbay env.

---

## 7. DLP Policy Heads-Up (Connectors in Penbay Env)

**Tenant Context:** Must be in Penbay Law. DLP policies are per-tenant + per-environment — a result in GAV does NOT carry over.

**Step-by-step:**

### Step 7.1 — Connector smoke test in penbay env
1. Open `https://make.powerautomate.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. Confirm tenant chip = **Penbay Law**.
3. Switch to **EP Design Sheet (Dev)**.
4. **+ Create → Instant cloud flow** → name it `_dlp-smoke-test-penbay` → trigger **Manually trigger a flow** → Create.
5. Add each connector in turn, watching only for a **"blocked by admin"** banner (config errors are fine to ignore):
   - **SharePoint** — "Get items"
   - **Office 365 Outlook** — "Send an email (V2)"
   - **Approvals** — "Start and wait for an approval"
   - **Microsoft Dataverse** — "List rows"
6. Note which (if any) show a block banner.
7. **Delete** the test flow when done.

### Step 7.2 — DLP owner heads-up
Send (or confirm IT already covered in #5.1):
> *"FYI — the EP Design Sheet (Dev) build in penbaylaw.com will use SharePoint, Office 365 Outlook, Approvals, and Microsoft Dataverse connectors. None are blocked in this environment per smoke test. Flagging for awareness."*

**Pass criteria:** All four connectors load without a block banner AND heads-up sent (or confirmed not required).

---

## 8. Copilot Studio Access (in Penbay Tenant)

**Tenant Context:** Must be in Penbay Law.

**Step-by-step:**
1. Open `https://copilotstudio.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. Confirm tenant chip = **Penbay Law** (NOT Golden Access Ventures).
3. Click the environment picker (top-right).
4. Under **Supported environments**, confirm **EP Design Sheet (Dev)** appears.
   - If only "Golden Access Ventures" or unrelated GAV environments appear, your tenant context is wrong — re-switch.
   - If penbay tenant is correct but EP Design Sheet (Dev) is missing, your Copilot Studio license isn't applied — loop IT.
5. Select **EP Design Sheet (Dev)**.
6. Click **+ Create agent** → confirm dialog opens → **Cancel** (don't create yet).

**Pass criteria:** Copilot Studio loads in penbay tenant, EP Design Sheet (Dev) selectable, agent creation dialog opens cleanly.

---

## 9. Dataverse Table Creation Permission (in Penbay Env)

**Tenant Context:** Must be in Penbay Law.

**Step-by-step:**
1. Open `https://make.powerapps.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. Confirm tenant chip = **Penbay Law**.
3. Switch to **EP Design Sheet (Dev)**.
4. Click **Tables** → **+ New table → New table**.
5. Display name: `EP Smoke Test`. Plural: `EP Smoke Tests`. Save.
6. Wait ~30s for provisioning. If it succeeds, you have create permission.
7. **Delete** the smoke-test table: open it → **Edit → Delete table**.

**Pass criteria:** Smoke-test table created without error and cleanly deleted.

**Decision log:**
- Dataverse worked → use Dataverse as the data store.
- Dataverse failed/blocked → confirm with IT that SharePoint lists are the fallback, and adjust the build plan.

---

## 10. M365 Declarative Agent Registration (in Penbay Tenant)

**Tenant Context:** Must be in Penbay Law — and this is the item most affected by tenant choice. Declarative agents need to be deployed in the tenant where the users live.

**Step-by-step:**

### Step 10.1 — Confirm M365 Copilot rollout owner is looped in
1. Identify the owner (ask IT contact #14 — may be the same person).
2. If not already covered by your initial brief in #5.1, send:
   > *"I'll be registering a declarative agent (`EP Design System 2.0 agent.agent`) for testing against the LegalTechnicalDepartment SharePoint site in penbay tenant. Confirming you're aware and upload of custom agents is permitted for my account."*
3. Get written acknowledgment.

### Step 10.2 — Functional check (lightweight)
1. Open `https://copilot.cloud.microsoft/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
2. Confirm tenant chip = **Penbay Law**.
3. Look for **Agents** or **Create / Upload custom agent** in the side panel.
4. Confirm an **Upload / Sideload** option exists for custom agent packages (`.zip` containing the declarative agent manifest).
5. If "blocked by admin" banner appears → loop IT.

### Step 10.3 — Decisive check (later, when ready)
1. Build a minimal `.agent` package.
2. Upload via Teams Admin Center or Copilot Studio's Publish path *in penbay tenant context*.
3. Confirm it appears in the agent list and responds.

**Pass criteria:** Written acknowledgment from M365 Copilot owner AND upload UI reachable in penbay tenant without a block banner.

---

## 11. Teams Test Channel (Conditional, Penbay Tenant)

**Tenant Context:** If using Teams path, must be in Penbay Law.

**Step-by-step:**
1. Confirm with project owner: Teams or email for attorney confirmation?
2. **If email:** skip this item; record in build plan: *"Attorney confirmation is email-based per onboarding decision."*
3. **If Teams:**
   - Open `https://teams.microsoft.com/?tenantId=f3db2981-1aec-46c0-a3b0-85174ee5c5e0`.
   - Confirm you can see Penbay Law in the org switcher (top-left, sometimes shown as the tenant name).
   - Ask IT to add you to a penbay test team (suggest name: "EP Design Sheet Test").
   - Once added: post `smoke test — please ignore`.
   - Confirm you can read messages and reactions.
   - Delete the test message.

**Pass criteria:** Teams path = member of penbay test team with read+write confirmed. OR email path = decision documented.

---

## 12. Anonymized Sample Matter Records

**Tenant Context:** Files end up in penbay SharePoint (`_engineering-artifacts/sample-matters/`), but this is a data-handoff task, not a Microsoft-tool task.

**Step-by-step:**
1. Ask project owner / paralegal lead for 3–5 anonymized records covering the **Unified Long-Term Marriage** track.
2. For each record verify:
   - Field names match production schema.
   - **No real PII** — fake names, addresses, account numbers.
   - At least one edge case (commingled assets, separate-property claims, long-marriage threshold).
3. Spot-check each file for accidental real-looking SSNs, addresses, or recognizable client names.
4. Save into `_engineering-artifacts/sample-matters/` in the penbay SharePoint library.

**Pass criteria:** 3–5 anonymized records received, edge cases covered, no PII leaks, stored in penbay SharePoint.

---

## 13. Clause Library Export

**Tenant Context:** Penbay SharePoint storage.

**Step-by-step:**
1. Ask the project owner: "In what format does the team maintain the clause library today?" (Word, Excel, SharePoint list)
2. Get the export.
3. Open and confirm:
   - Each clause has a stable identifier.
   - Clauses are categorized.
   - Clauses for build-spec sections **12F, 12K, 12L** are findable.
4. Save into `_engineering-artifacts/clause-library/` in penbay SharePoint.

**Pass criteria:** Export received, parseable, 12F/12K/12L clauses findable.

---

## 14. IT Point of Contact (Penbay Law)

**Step-by-step:**
1. Get name + email of one penbaylaw.com IT person from the project owner.
2. Send an intro email from `mvp-projects@goldenaccessventures.com`:
   > Hi [Name], [Project owner] connected us — I'm the engineer building the EP Design Sheet MVP. I'll be pinging you for permission changes (DLP, agent uploads, environment roles, etc.). Just establishing the channel here.
3. Confirm no bounce within 1 hour.
4. Save contact as `Penbay IT — primary`.

**Pass criteria:** Contact recorded, intro email delivered.

---

## Cleanup — Remove the GAV-Tenant Environment

Once everything above is ✅, remove the stale GAV environment to avoid name collision and confusion:

1. Open `https://admin.powerplatform.microsoft.com` (this time **without** the penbay tenant param — defaults to GAV).
2. Confirm tenant chip = **Golden Access Ventures**.
3. Click into **EP Design Sheet (Dev)**.
4. Click **⋯ → Delete environment**.
5. Confirm deletion.

After deletion, you should only have the penbay-tenant **EP Design Sheet (Dev)** going forward.

---

## Master Checklist Summary

| # | Item | Status |
|---|---|---|
| 1 | Identity (penbay tenant + MFA) | ⏳ To verify in penbay context |
| 2 | SharePoint edit on Design System 2.0 | ✅ Confirmed (penbay-inherent) |
| 3 | SharePoint read on related folders | ✅ Confirmed (penbay-inherent) |
| 4 | Permission to create subfolder | ✅ Confirmed (penbay-inherent) |
| 5 | Power Platform environment in penbay | ⏳ Awaiting IT provisioning |
| 6 | Maker + Customizer in penbay env | ⏳ Pending #5 |
| 7 | DLP heads-up in penbay env | ⏳ Pending #5 |
| 8 | Copilot Studio access in penbay | ⏳ Pending #5 |
| 9 | Dataverse table creation in penbay | ⏳ Pending #5 |
| 10 | M365 declarative agent in penbay | ⏳ Pending #5 + Copilot owner ack |
| 11 | Teams test channel (penbay) | ⏳ Conditional on path |
| 12 | Sample matter records | ⏳ Request from project owner |
| 13 | Clause library export | ⏳ Request from project owner |
| 14 | IT point of contact (penbay) | ⏳ To establish |
| — | Cleanup: delete GAV env | ⏳ Final step |

---

## Recommended Order

Run items in this sequence — each builds on the previous:

1. **Prerequisite** — get penbay tenant GUID, save bookmarks, practice tenant chip verification.
2. **#14** — establish penbay IT contact (needed for #5).
3. **#1** — verify your guest identity works in penbay context.
4. **Project owner decision** — confirm Path 1.
5. **#5.1** — send IT the provisioning brief.
6. **#5.2** — wait for IT confirmation (hard blocker).
7. **#5.3** — verify env in penbay context, record details.
8. **#6** — smoke test roles in penbay env.
9. **#7** — connector smoke test in penbay env.
10. **#8** + **#9** in parallel — Copilot Studio + Dataverse, both in penbay.
11. **#10** — declarative agent registration in penbay.
12. **#11** — Teams (if Teams path).
13. **#12** + **#13** in parallel — request sample data + clause library.
14. **Cleanup** — delete the GAV-tenant EP Design Sheet (Dev).

When every row in the master checklist is ✅, onboarding is complete and you're cleared to start the build.

---

## Anti-Drift Habits

Three habits to internalize so the GAV-tenant slip doesn't repeat:

1. **Always open Microsoft tools via the tenant-scoped bookmark**, never via plain URL.
2. **Read the account chip before every action.** If it says anything other than "Penbay Law," stop and switch.
3. **When in doubt about which tenant a resource lives in**, try adding another `@goldenaccessventures.com` user to it: if no guest-invite flow appears, you're in GAV; if a guest invite is required, you're in penbay.
