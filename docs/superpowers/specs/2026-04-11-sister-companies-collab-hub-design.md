> **Working copy:** This repo (**SMH-collab-hub**) is the primary place to evolve hub **implementation** docs. The same specification text also lives in **SMH-app** at `docs/superpowers/specs/2026-04-11-sister-companies-collab-hub-design.md`. After substantive edits, sync the other copy (or delete the duplicate once you declare a single canonical repo).

---

# Sister-companies collaboration hub (multi-Workspace, single project)

**Date:** 2026-04-11  
**Status:** Draft (updated with org roster and goals)  
**Product:** **Standalone** collaboration hub (may be its **own** app, Sheet+Chat pilot, or separate repo — **does not** need to live inside SMH-app). Coordinates **four sister companies** on **Google Workspace** for shared **bidding and scheduling** communication.  
**Related:** `2026-04-08-smh-client-portal-workspace-design.md` (SMH client portal) is a **separate** product; link by `projectId` or name only if useful — no shared codebase required.

---

## 1. Problem statement

**Primary goal:** Improve **cross-company communication** for **bidding** and **scheduling** across the four sister brands. This layer is for the **small set of people** who need visibility together; **each company handles its own internal** rollout to the rest of its employees (the hub does not replace internal crew dispatch).

Shared work still needs:

- **One place** for agreed **timeline**, **availability**, **bid handoffs**, and **decisions** the group trusts.
- **Structured communication** (not only ad-hoc email/Chat) with **tagging** (company, trade, phase, topic).
- **Optional operational feeds** (e.g. **House Call Pro** where a company already uses it) so context can land in the hub **without** forcing every org into the same tools.

**Success (v1):** A **GC-led** space (Huckleberry / Doug & Bryan — see below) where the listed cross-org **members** can align on bidding and scheduling; other staff stay on each company’s normal channels.

---

## 2. Companies (v1 roster)

| Code | Company |
|------|---------|
| **SMH** | St. Matthews Home |
| **SME** | St. Matthews Electric |
| **SMP** | St. Matthews Plumbing |
| **HH** | Huckleberry Handyman |

**General contractor (in scope for this hub):** **Huckleberry Handyman (HH)** — **Doug** and **Bryan** are the **GC principals** on these projects. They carry **contract / schedule authority** for the work coordinated here and align with **hub administration** (who runs the shared space, invites, and the official cross-org record).

**Sister brands (SMH, SME, SMP)** participate as **trade / partner** orgs on those GC-led jobs (e.g. Billy & Lee for SMH, Richard & Andrew for SME, Tony & Josh for SMP). Other SMH-only jobs outside this program are out of scope unless you add them to the hub explicitly.

### 2.1 Cross-org members (hub access)

These people are in scope for **direct** hub access (map to real **Workspace or guest emails** in implementation):

| Org | Members |
|-----|---------|
| **SME** | Richard, Andrew |
| **SMP** | Tony, Josh |
| **SMH** | Billy, Lee |
| **HH** | Doug, Bryan — **GC principals** |

**Out of scope for v1 hub accounts:** Everyone else at each company — each org **disseminates** its slice internally (text, standup, their own scheduling tools) without requiring hub accounts for all field staff.

### 2.2 Roles (logical)

| Role | Description |
|------|-------------|
| **GC org (HH)** | Doug & Bryan: construction GC on in-scope projects; hub admin, invites, baseline structure; natural owners for **operational imports** (e.g. House Call Pro) when used on these jobs. |
| **Partner orgs (SMH, SME, SMP)** | Contribute and consume bidding/scheduling threads, timeline, tags; escalate to GC when commitments or scope need a single decision. |
| **Participant** | One of the eight named individuals, authenticated by email / Workspace membership per chosen architecture. |

---

## 3. Capabilities (product map)

| Area | Intent |
|------|--------|
| **Bidding** | Shared threads or log: scope questions, deadlines, who is covering which line items, **decisions** visible to all eight members. |
| **Scheduling** | Cross-org **availability**, phase dates, dependencies (“electric rough before drywall”), and **commitments** — not full replacement for each shop’s internal calendar. |
| **Communication** | Chronological (or lightly threaded) **job / opportunity feed**: posts, @mentions, optional **Chat** or email nudges for time-sensitive items. |
| **Tagging** | Labels for **company** (SMH/SME/SMP/HH), **trade**, **topic** (e.g. `permit`, `bid-deadline`) — filterable. |
| **Timeline** | Milestones and key dates for the **shared** plan; each company may mirror internally. |
| **House Call Pro (optional)** | **Inbound** sync or file drop per org; see **Lead intake** (section 7) for how four separate HCP environments feed the hub. |
| **Lead intake** | Any of the four companies may **originate** a job; capture **name, address, email, phone**, **originating org**, and **context** with as little duplicate typing as possible (see section 7). |

**Non-goals (v1):** Every employee in the hub; cross-org payroll; replacing internal CRM or crew scheduling; mandatory HCP for all four brands; full two-way HCP write-back unless explicitly scoped later.

---

## 4. Constraints

- **Google-native:** Gmail, Drive, Chat, Calendar, and/or Apps Script — **no mandatory** third-party login for core access if avoidable.
- **Separate Workspaces:** Users may have **different primary domains**. Identity and sharing must work across domains (shared Drive folders, **guest** access, or **Google Groups** with external members — option-dependent).
- **Trust boundary (hub):** For **in-scope projects**, **HH (Doug / Bryan)** is both **construction GC** and the authority for the official **cross-org** bidding/scheduling record in the hub. Posts may later support **internal** vs **GC-approved** flags if needed. If a thread ever spans a job **not** GC’d by HH, treat that as a **different program** or document an exception in the runbook.

---

## 5. Architectural approaches (choose one for v1)

### Option A — **Extend SMH-app** (optional; not required)

- **Idea:** Fold partner tables and a “collab view” into the existing SMH Apps Script + spreadsheet.
- **Pros:** One deployment if you explicitly want everything in SMH-app.
- **Cons:** Product owner asked for **wholly separate** capability; cross-Workspace identity in Apps Script stays awkward. **Treat as optional** — only if you later **choose** to merge.

### Option B — **Neutral “hub” Workspace** (recommended starting point)

- **Idea:** A **dedicated Google Workspace** or **Shared drive** (owned or administered by **HH** as GC) holds: **Hub Sheet** (or **AppSheet** / **Looker Studio** later), **one folder per job or bid package**, and **one Chat space** per initiative. The eight members are **invited** with explicit roles; other companies’ staff stay off the hub.
- **Pros:** Permissions are **native Google**: Drive sharing, Chat membership, Calendar invites. Each brand keeps day-to-day mail in its own tenant; leaders **visit** the hub for shared bidding/scheduling.
- **Cons:** Operational overhead: **someone (HH)** must administer invites, membership, and offboarding.

### Option C — **Chat-first + Sheet log** (lightweight v0)

- **Idea:** No custom web app at first: **Chat space** per project + **linked Google Sheet** tab for structured fields (timeline rows, HCP import paste area) + **Drive folder**.
- **Pros:** Hours to stand up; trains the org on **where** to look before building UI.
- **Cons:** Weak **tagging/filtering** UX; HCP mapping still manual unless scripted.

**Recommendation:** **Option B** (standalone hub tenant or Shared drive under HH) with **Option C** as a **zero-code pilot**. **Option A** is **not** assumed; if SMH-app ever ingests hub data, treat it as an **integration** (IDs, webhooks, or export), not a requirement to host the hub.

---

## 6. Conceptual data model (org-agnostic)

These are **logical** entities; physical storage may be Sheets tables, BigQuery later, etc.

- **Organization** — `id`, `code` (one of SMH, SME, SMP, HH), `name`, `workspaceDomain` (optional), `role` (`general_contractor` for HH on in-scope programs, `partner` for SMH/SME/SMP unless you extend roles).
- **Project** (or **BidPackage** / opportunity) — `id`, `displayName`, `address` (optional), `generalContractorOrgId` (HH for in-scope work), `status`, `driveFolderId`, `chatSpaceId` (optional).
- **Membership** — `projectId`, `orgId` (SMH, SME, SMP, HH), `workspaceUserEmail`, `role` (`viewer` | `contributor` | `gc_admin` for Doug/Bryan or delegated HH admins).
- **Lead (intake)** — `id`, `status` (`draft` | `approved` | `merged_to_project`), `originatingOrg` (one of SMH, SME, SMP, HH), `customerName`, `address`, `email`, `phone`, `contextText`, `rawAttachmentId` (Drive file or Chat message link), `hcpTenantHint` (optional), `createdBy`, `createdAt`.
- **FeedEvent** — `projectId`, `authorEmail`, `body`, `tags[]`, `createdAt`, `source` (`manual`, `hcp_import`, `hcp_api`, `lead_photo_ocr`, `lead_form`, `system`).
- **TimelineItem** — `projectId`, `label`, `start`, `end`, `status`, `visibility` (`internal` | `all_partners`).
- **HcpImportBatch** — `projectId`, `importedAt`, `rawReference` (file id or job id), `rowCount` (audit).

---

## 7. Lead intake — four HCP tenants, minimal re-entry

**Today:** Leads can start at **any** of the four companies. Each runs its **own** House Call Pro account. People often **text a photo** of the HCP customer block (name, address, email, phone) plus a **short context** message.

**Goal:** Get that into the hub (Sheet / Chat / app) **without retyping** into a “new” system whenever possible — while accepting that **four separate HCP products** will not magically merge without **some** integration or capture layer.

### 7.0 Channel preference (product)

Preferred order for sending leads and context (easiest first):

1. **Google Chat** — primary intake surface (dedicated space or thread, images + text, membership matches your eight hub users or wider internal before GC triage).  
2. **SMS** — second choice; requires a **bridge** (e.g. Twilio number → email/Chat/webhook) if Chat is not used; treat as **PII** and document retention.  
3. **Email** — third choice; still viable (forward-to-inbox, attachments, labels).

Pilot designs should **default to Chat** before investing in SMS plumbing.

### 7.1 Recommended strategy (layered)

| Tier | Approach | Best for | Trade-off |
|------|-----------|----------|-----------|
| **1 — Immediate (zero build)** | **Pinned Chat template** (preferred) or **Google Form** in the intake space: same five fields as HCP; paste from HCP “copy” if available, or type once. Optional **photo still attached** for audit. | Pilot this week | One extra step; no OCR. |
| **2 — Fit current habit (low build)** | Dedicated **“leads” Chat space** (first choice) where people post **photo + text**; optional **SMS bridge** later so texts land in the same triage queue. If email is used, a **Gmail label + Drive save attachments** pattern works the same. **GC (Doug/Bryan) or one delegate** moves items into a **Drive “Inbox”** or **Lead** sheet; first pass can be **manual promote** to **`draft` Lead**. | Matches photo habit | Human triage until automated. |
| **3 — Automation from photos** | New image in Inbox → **Apps Script, or Cloud Function** → **Google Document AI / Vision OCR** → populate a **`draft` Lead** row (name, address, email, phone); sender’s **domain or a required tag** (`#SMH`, `#SME`, …) sets `originatingOrg`. **GC approves** draft → creates **Project**. | Scales past a few leads/week | OCR mistakes; PII in Drive; small build. |
| **4 — Best data (higher setup)** | Per-tenant **HCP → webhook or Zapier/Make → shared endpoint** (or scheduled **CSV export** to a **watched Drive folder** per org) appends structured rows — **four** small recipes, one hub target. | Accurate fields, no OCR | Four setups; depends on HCP plan/API. |

**Practical recommendation:** Start **Tier 1 + 2** (template or form **and** a single intake inbox with photos) so nothing blocks the pilot. Add **Tier 3** or **Tier 4** for whichever hurts first (**volume** → OCR; **accuracy** → HCP automation per org).

### 7.2 Originating org

Every lead must record **which company sourced it** (`originatingOrg`). Options: **hashtag in the message**, **picklist on Form**, **sender email domain**, or **separate Chat threads per org** that all forward into one triage queue (clearer routing, more threads).

### 7.3 Privacy

Photos and forms contain **PII**. Keep intake in the **hub GC-controlled** Workspace or Shared drive; retention and who can see **draft** leads should match your client-privacy norms (see open decisions). **SMS** adds carrier metadata and possibly a third-party inbox — include in privacy review if you enable Tier 2 SMS bridging.

### 7.4 Hubdoc (existing stack)

The team already uses **Hubdoc** (strong at **reading** and filing documents). **Optional path:** route **HCP screenshots, exports, or PDFs** into Hubdoc’s normal capture flow (email-in or upload), use extracted fields for **accounting / filing**, then **manually or via export** copy key columns into the hub **Lead** sheet — or evaluate Hubdoc / Xero ecosystem **exports and APIs** if a repeatable field mapping is available. Treat Hubdoc as a **document intelligence + archive** layer, not necessarily the real-time Chat feed; it can **reduce retyping** where you already trust its extraction.

### 7.5 Google Lens vs automated OCR

- **Google Lens** (phone): great for a **single user** to grab **text from a photo** and **paste into Chat** — effectively Tier 1 with no backend. There is **no** Lens “organization webhook” that posts into your Sheet; it is not a substitute for a **system** intake pipeline.  
- **Automated** extraction from images saved to Drive/Chat is better served by **Google Cloud Vision API** or **Document AI** (or Hubdoc’s pipeline where applicable) — same class of **OCR** technology, **server-side**, with auditable results and optional **`draft` Lead** approval.

---

## 8. Integration sketch: House Call Pro

- **Direction:** **Inbound only** in early versions. **Lead lifecycle:** any org may source the lead (section 7); **HH (GC)** still owns **approval** into a canonical **Project** for GC-led work. **Ongoing job data** may still flow from whichever HCP instance is attached to that job after conversion.
- **Mechanism (job-level):** Export CSV, scheduled file drop to Drive, or HCP API if licensed — **implementation plan** picks one **per tenant** where needed.
- **Mapping:** HCP entities (customer, job, appointment) → **Project** + **FeedEvent** rows (e.g. “Visit completed — {summary}”).
- **Idempotency:** Re-imports should **not duplicate** events (hash or external id column).

---

## 9. Relationship to SMH-app (optional link only)

- This hub may be a **different** product, repo, spreadsheet, and deployment from **SMH-app** (`Code.js` / client portal).
- **If** both apply to the same real-world job, use a **shared human-readable key** or optional **SMH `projectId` reference** column — no requirement to embed the hub inside Apps Script.
- SMH **client portal** remains **token-based** and **client-safe** per its own spec; **do not** expose hub bidding chatter to clients without a separate product decision.

---

## 10. Phased delivery (suggested)

| Phase | Outcome |
|-------|---------|
| **P0 — Mapping** | This document + org roster + GC (HH / Doug & Bryan) + example bid/schedule lifecycle (paper). |
| **P1 — Pilot** | Option C or B-minimal: Drive + Sheet + Chat; **lead intake** (section 7 Tier 1–2); optional manual HCP CSV per org; weekly review. |
| **P2 — App surface** | Optional web UI (Apps Script or Cloud Run later) for feed, tags, timeline filters. |
| **P3 — Automation** | HCP automation, richer notifications, optional **approved vs draft** posts. |

---

## 11. Open decisions (fill before implementation plan)

1. **Workspace email for each member** — map Richard, Andrew, Tony, Josh, Billy, Lee, Doug, Bryan to addresses (and which tenant hosts each primary mailbox).  
2. **Hub hosting** — new small Workspace vs Shared drive in an existing tenant (likely HH-controlled).  
3. **Canonical project / opportunity id** — hub-native ids vs optional **SMH `Projects.id`** when the same job exists in SMH-app.  
4. **HCP (or similar) account owner** on GC-led jobs — default is **HH**; confirm actual licensee and login used for imports.  
5. **HCP mechanism:** CSV export vs API vs Zapier/Make — compliance and cost.  
6. **Default surface:** Chat-only pilot vs Sheet + Chat vs small custom web UI.  
7. **Retention:** how long to keep import batches, **draft leads**, photos, and hub messages.  
8. **Lead intake v1:** Chat-first intake space + template; **SMS bridge** vendor and number if needed; **Hubdoc** role (archive-only vs export-into-Lead); who besides GC may **approve** drafts.  
9. **HCP automation pilot org** — which of the four tenants gets API/export first.

---

## 12. Self-review

- **Placeholders:** Per-user emails, HCP field mapping, notification rules, **per-org HCP automation**, **SMS bridge**, and **Hubdoc export path** deferred to implementation plan.  
- **Consistency:** GC = **HH (Doug, Bryan)** for in-scope hub projects; partner orgs and eight-member roster align with Option B; **all four orgs** can originate leads per section 7; intake channel order **Chat → SMS → email** locked in section 7.0.  
- **Scope:** Cross-org **bidding + scheduling** for the roster; internal employee comms explicitly out of scope for v1 hub accounts.  
- **Edge case:** Jobs **not** GC’d by HH but still discussed in chat — either exclude from this hub or document **exception** GC / authority so the record stays unambiguous.

---

## 13. Next step

After you approve or edit this spec: add `docs/superpowers/plans/2026-04-11-sister-companies-collab-hub-implementation.md` with P1 pilot checklist (Drive layout, Sheet tabs, **Leads** + promote-to-Project flow, Chat membership template, **lead intake** channel, HCP CSV column map per org).
