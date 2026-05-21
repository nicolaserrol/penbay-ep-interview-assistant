# Onboarding Checklist Verification Guide

This guide walks through how to verify each item from `ONBARODING_CHECKLIST.md` is complete and ready. Each section defines "done," lists what you need, and gives a step-by-step check.

**Account in use:** `mvp-projects@goldenaccessventures.com`
**Already confirmed:** Items #2, #3, #4 (SharePoint access to EP Design System 2.0)

---

## 1. Identity — Licensed/Guest Account + MFA

**Definition of done:**
- `mvp-projects@goldenaccessventures.com` can sign in to the penbaylaw.com tenant (either as a licensed member or as a guest).
- MFA is enrolled and required at sign-in.
- The account appears in penbaylaw.com's Entra ID (Azure AD) directory.

**What you need:**
- The password / passkey for `mvp-projects@goldenaccessventures.com`.
- An authenticator app (Microsoft Authenticator recommended) on your phone.

**Step-by-step check:**
1. Open a private/incognito browser window (avoid cached sessions).
2. Go to `https://portal.office.com`.
3. Sign in as `mvp-projects@goldenaccessventures.com`.
   - If you see a tenant picker, choose the **penbaylaw.com** tenant. If only your home tenant appears, the guest invite hasn't been redeemed yet — check the inbox for the "You're invited" email and click **Accept invitation**.
4. Complete the MFA challenge when prompted.
   - If MFA is not requested, go to `https://mysignins.microsoft.com/security-info` and confirm at least one method is registered (Authenticator app, phone, or passkey). If nothing is listed, register one now.
5. Verify your account context:
   - Click your avatar (top right) → **View account**.
   - Confirm the organization shown is **penbaylaw.com** (or that you can switch to it).
6. Open `https://myaccount.microsoft.com` and confirm under **Organizations** that penbaylaw.com is listed.

**Pass criteria:**
- Sign-in succeeds against penbaylaw.com tenant.
- MFA prompt was enforced.
- penbaylaw.com appears as one of your organizations.

**If it fails:** Email your IT point of contact (item #14) with the exact error message and screenshot.

---

## 2–4. SharePoint Access ✅ ALREADY CONFIRMED

**Status:** Confirmed by user.

**Recommended quick re-verification (5 min):**
1. Open the [Design System 2.0 library](https://penbaylawcom.sharepoint.com/sites/LegalTechnicalDepartment/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2FLegalTechnicalDepartment%2FShared%20Documents%2FEP%20Design%20System%202%2E0&viewid=b4b0b9ed%2Def26%2D4974%2D9652%2Dfe80635d59d1&csf=1&CID=bc278f6d%2D9ff5%2D4bee%2D9388%2D4a3c2d8c48b4&FolderCTID=0x0120003DB69ED1374F4743AF82CDC6476740BB) signed in as `mvp-projects@goldenaccessventures.com`.
2. Confirm you can:
   - **Read** the folder contents (item #2/#3).
   - **Edit**: Upload a test file (e.g., `_smoke-test.txt`), then delete it (item #2).
   - **Create a subfolder** named `_engineering-artifacts` (or similar) at the root of the library (item #4). Leave it in place — you'll use it for build outputs.
3. Locate the Word merge templates and clause library files referenced in the build spec (sections 12F, 12K, 12L). Confirm you can open and read them.

---

## 5. Power Platform Environment ✅ CONFIRMED

**Status:** Environment **"EP Design Sheet (Dev)"** created — Type: **Developer**, State: **Running**, visible in both Power Platform admin center and `make.powerapps.com`.

**Caveats to be aware of with a Developer-type environment:**
- Single-owner (your account only); ownership can't be transferred.
- Auto-reclaimed after ~90 days of inactivity.
- Smaller capacity than Sandbox/Production.
- If the project later transitions to a team, you'll need to migrate to a Sandbox environment.

**Remaining follow-up:**

- **Confirm Developer type is acceptable for the MVP scope.** Send a one-line note to your IT contact (#14): *"FYI — I provisioned a Developer-type Power Platform environment named 'EP Design Sheet (Dev)' for the MVP build. Let me know if you'd prefer this on a Sandbox environment instead."*

**Decision log entry:**
- Environment name: `EP Design Sheet (Dev)`
- Type: Developer
- Region: (record from environment overview page)
- Dataverse URL: `https://org60b4cf85.crm.dynamics.com/` ✅ provisioned

---

## 6. Environment Maker + System Customizer Roles ✅ CONFIRMED

**Status:** Smoke test passed — created a "Test Solution" in EP Design Sheet (Dev) without errors. Account has **System Administrator** on the environment, which supersedes both Environment Maker and System Customizer.

**Definition of done:**
- Your account has the privileges of **Environment Maker** and **System Customizer** on the target environment — either as named roles, or implicitly via a higher-privilege role.

**Important note on role hierarchy:**
- **System Administrator** is the top-level Dataverse role and is a strict superset of both Environment Maker and System Customizer. If your account has System Administrator, you have full Maker + Customizer privileges even if those roles aren't listed separately.
- When you create a Developer-type environment, you are automatically made System Administrator on the Dataverse — so this item is usually satisfied by default.

**Step-by-step check:**

### Look at assigned roles (informational):
1. In `https://admin.powerplatform.microsoft.com`, open the target environment.
2. Click **Settings** (top bar) → **Users + permissions** → **Users**.
3. Find and click `mvp-projects@goldenaccessventures.com`.
4. Look at **Direct Assigned Roles**. Acceptable outcomes:
   - **System Administrator** is listed → you're covered (superset of both target roles).
   - **Environment Maker** AND **System Customizer** are both listed → you're covered.
   - Only one or neither is listed AND no System Administrator → roles need to be added (loop in IT).

### Functional smoke test (decisive — do this regardless):
1. Go to `https://make.powerapps.com`.
2. Switch to the target environment (top-right environment picker).
3. Click **Solutions** in the left nav → **+ New solution**.
4. If the dialog opens and accepts a name/publisher without error, your Maker + Customizer privileges are working.
5. Cancel out — don't actually create the solution yet.

**Pass criteria:**
- Role check OR smoke test confirms you have the necessary privileges.

**If it fails:** Ask IT to add Environment Maker + System Customizer (or System Administrator) to your user in the dev environment.

---

## 7. DLP Policy Heads-Up (Connectors)

**Definition of done:**
- Whoever owns Data Loss Prevention policy in the tenant has been notified that this build will use these connectors:
  - **SharePoint**
  - **Office 365 Outlook**
  - **Approvals**
  - **Dataverse**
- None of these connectors are blocked in the target environment's DLP policy.

**Step-by-step check:**

### Quick test (functional — fastest signal):
1. Go to `https://make.powerautomate.com`.
2. Switch to the target environment.
3. Click **+ Create → Instant cloud flow**.
4. Name it `_dlp-smoke-test`, choose **Manually trigger a flow**, click **Create**.
5. Click **+ New step** and search for each connector in turn:
   - **SharePoint** — try adding "Get items"
   - **Office 365 Outlook** — try adding "Send an email (V2)"
   - **Approvals** — try adding "Start and wait for an approval"
   - **Microsoft Dataverse** — try adding "List rows"
6. If any connector shows a banner like *"This connector is blocked by your admin"* or *"Cannot be used in this environment due to data loss prevention policies,"* note which one.
7. Delete the test flow when done.

### Admin confirmation:
1. Identify the DLP policy owner — usually a tenant admin (your IT contact in item #14 will know).
2. Send them a one-line note: *"FYI — the EP Design Sheet build in the [environment name] environment will use SharePoint, Office 365 Outlook, Approvals, and Dataverse connectors. Please confirm none are blocked."*
3. Wait for written confirmation.

**Pass criteria:**
- All four connectors can be added to a flow without DLP blocking.
- Written confirmation from the DLP owner.

---

## 8. Copilot Studio Access

**Definition of done:**
- You can open Copilot Studio in the target environment and create a new copilot/agent.

**Step-by-step check:**
1. Go to `https://copilotstudio.microsoft.com`.
2. Sign in as `mvp-projects@goldenaccessventures.com`.
3. Use the environment picker (top right) to switch to the target environment.
4. Click **Create** in the left nav.
5. You should see options for **New agent** (or "New copilot"). Click into the dialog.
6. If the dialog opens without an error and you see the option to name a new agent, access is working.
7. **Don't create the agent yet** — just confirm the dialog opens. Cancel out.

**Pass criteria:**
- Copilot Studio loads, the target environment is selectable, and the "New agent" flow opens.

**If it fails:**
- "License required" error → IT needs to assign a Copilot Studio license to your account.
- Environment not visible → confirm item #6 first (Maker role missing means environment is hidden).

---

## 9. Dataverse Table Creation Permission

**Definition of done:**
- You can create a new custom table in the target environment's Dataverse.
- Decision logged: **Dataverse** or **SharePoint lists** as the data store.

**Step-by-step check:**
1. Go to `https://make.powerapps.com`.
2. Switch to the target environment.
3. Click **Tables** in the left nav.
4. Click **+ New table → New table** (not "Set of tables").
5. In the dialog:
   - Display name: `EP Smoke Test`
   - Plural: `EP Smoke Tests`
6. Click **Save**. Wait for table provisioning (~30 seconds).
7. If it succeeds, you have create permission.
8. **Delete the smoke-test table** immediately: open the table → **Edit → Delete table**.

**Pass criteria:**
- Smoke-test table created without permission error, then deleted cleanly.

**Decision log:**
- If Dataverse worked → proceed with Dataverse as the data store.
- If Dataverse is blocked or unavailable → confirm with IT that SharePoint lists are the fallback, and update the build plan.

**If it fails:** System Customizer role is missing (loop back to item #6).

---

## 10. M365 Declarative Agent Registration

**Definition of done:**
- You can register and test the `EP Design System 2.0 agent.agent` declarative agent against the penbaylaw.com SharePoint site.
- The IT owner of M365 Copilot agent rollout is aware and has approved.

**Step-by-step check:**

### Prerequisite — confirm the IT owner is looped in:
1. Identify who owns the M365 Copilot rollout (ask your IT contact, item #14).
2. Send them a short note: *"I'll be registering a declarative agent (`EP Design System 2.0 agent.agent`) for testing against the LegalTechnicalDepartment SharePoint site. Confirming you're aware and that upload of custom agents is permitted for my account."*
3. Get written acknowledgment.

### Functional check (lightweight):
1. Go to `https://copilot.cloud.microsoft` (Microsoft 365 Copilot) signed in as `mvp-projects@goldenaccessventures.com`.
2. Look for **Agents** or **Create an agent** in the side panel.
3. Alternatively, open Teams → Apps → **Build your own app / Upload a custom app**.
4. Confirm you have an **Upload** or **Sideload** option for custom agents (`.zip` package containing the declarative agent manifest).

### Decisive check (when you're ready):
1. Build a minimal `.agent` package (declarative agent manifest + instructions JSON).
2. Upload it via Teams Admin Center or Copilot Studio's "Publish" path.
3. Confirm it appears in your Copilot agent list and responds when invoked.

**Pass criteria:**
- IT owner has acknowledged in writing.
- You can reach the agent upload UI without a "blocked by admin" error.

**If it fails:** Tenant policy may block custom agent upload — IT needs to enable it for your account or the dev environment.

---

## 11. Teams Test Channel (Conditional)

**Definition of done — IF attorney confirmation is via Teams:**
- You are a member of a test team where you can post and read messages.
- You have the team name, channel name, and link.

**Definition of done — IF attorney confirmation is email-based:**
- Skip this entirely. Note in your build plan: "Attorney confirmation is email-based per onboarding decision."

**Step-by-step check (Teams path):**
1. Confirm with the project owner: is attorney confirmation happening in Teams or email?
2. If Teams:
   - Open Teams (`https://teams.microsoft.com`).
   - Look for the test team in your team list. Common name: something like "EP Design Sheet Test" or "Legal Tech Test."
   - If missing, ask IT to add you to the test team.
   - Once added, post a test message: `smoke test — please ignore`.
   - Confirm you can read replies and reactions.
   - Delete the test message.

**Pass criteria:**
- Teams: you're a member, can post, and can read. OR
- Email: project owner has confirmed email-based and this item is skipped.

---

## 12. Anonymized Sample Matter Records

**Definition of done:**
- 3–5 sample matter records are in hand, covering the **Unified Long-Term Marriage** track.
- Each record has **real-shape data** (correct field names, plausible ranges) but **no real PII**.
- Files are stored in your `_engineering-artifacts` subfolder (created in item #4) or another agreed location.

**Step-by-step check:**
1. Ask the project owner or paralegal lead to provide the records.
2. For each record, verify:
   - Field names match the production schema (party names, dates, marriage duration, asset categories, etc.).
   - **No real PII**: names should be obvious placeholders (e.g., "Jane Doe", "John Smith"), addresses should be fake, SSNs/account numbers must be redacted or fake.
   - At least one record exercises **edge cases** (e.g., commingled assets, separate property claims, long marriage exceeding the threshold).
3. Open each file and scan for accidental PII leaks (real-looking SSNs, real addresses, real client names you recognize).
4. Save all five into a folder named `sample-matters/` inside `_engineering-artifacts/`.

**Pass criteria:**
- 3–5 files received, all anonymized, all cover the Unified Long-Term Marriage track.
- No real PII visible on inspection.

---

## 13. Clause Library Export

**Definition of done:**
- You have the current clause library in its native format (Word, Excel, or SharePoint list export).
- File is stored alongside the sample matters.

**Step-by-step check:**
1. Ask the project owner: "In what format does the team maintain the clause library today?"
2. Get the export:
   - **Word**: a `.docx` with all clauses.
   - **Excel**: a `.xlsx` with one clause per row.
   - **SharePoint list**: export to CSV via the list's **Export to CSV** button, or get a link to the list itself.
3. Open the file and confirm:
   - Each clause has a stable identifier (clause ID, name, or numbering).
   - Clauses are categorized (by section, topic, or matter type).
   - You can identify clauses related to the build spec sections **12F, 12K, 12L**.
4. Save into `_engineering-artifacts/clause-library/`.

**Pass criteria:**
- Export received in a parseable format.
- 12F, 12K, 12L referenced clauses are findable.

---

## 14. IT Point of Contact

**Definition of done:**
- You have a name and email of one person on penbaylaw.com's IT side.
- You've verified the email works by sending an intro note.

**Step-by-step check:**
1. Ask the project owner for the IT contact name and email.
2. Send a short intro email from `mvp-projects@goldenaccessventures.com`:
   > Hi [Name],
   >
   > [Project owner] connected us — I'm the engineer building the EP Design Sheet MVP. I'll likely need to ping you directly when permission switches need flipping (DLP, agent uploads, environment roles, etc.). Just establishing the channel here — no action needed.
   >
   > Thanks,
   > [Your name]
3. Confirm the email delivers (no bounce within 1 hour).
4. Save the contact in your address book labeled `Penbay IT — primary`.

**Pass criteria:**
- Contact name + email recorded.
- Intro email sent and not bounced.

---

## Master Checklist Summary

| # | Item | Status |
|---|---|---|
| 1 | Identity (licensed/guest + MFA) | ⏳ To check |
| 2 | SharePoint edit on Design System 2.0 | ✅ Confirmed |
| 3 | SharePoint read on related folders | ✅ Confirmed |
| 4 | Permission to create subfolder | ✅ Confirmed |
| 5 | Power Platform environment | ✅ Created — Dataverse confirmed (`org60b4cf85`) |
| 6 | Environment Maker + System Customizer | ✅ Confirmed (System Admin) |
| 7 | DLP policy heads-up | ⏳ To check |
| 8 | Copilot Studio access | ⏳ To check |
| 9 | Dataverse table creation | ⏳ To check |
| 10 | M365 declarative agent registration | ⏳ To check |
| 11 | Teams test channel (if applicable) | ⏳ Conditional |
| 12 | Sample matter records (3–5) | ⏳ To check |
| 13 | Clause library export | ⏳ To check |
| 14 | IT point of contact | ⏳ To check |

---

## Recommended Order

Do them in this sequence — each builds on the previous:

1. **#14** (IT contact) — needed for everything else when blocked.
2. **#1** (identity + MFA) — gateway to all M365/Power Platform access.
3. **#5** (environment exists) — prerequisite for #6–#10.
4. **#6** (roles on environment) — prerequisite for #7–#10.
5. **#7** (DLP) — gate that can silently block #8–#10.
6. **#8** (Copilot Studio) and **#9** (Dataverse) — parallel.
7. **#10** (declarative agent) — needs IT acknowledgment lead time.
8. **#11** (Teams) — clarify path with project owner first.
9. **#12** (sample matters) and **#13** (clause library) — parallel, request from project owner together.

When all items are ✅, you're cleared to start the build.
