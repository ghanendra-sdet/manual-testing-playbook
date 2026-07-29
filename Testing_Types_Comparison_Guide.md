# 🎯 Testing Types Comparison Guide

> **Quick-Reference Companion to Part 2 (Types of Testing)**
> Difficulty Level: Beginner to Intermediate | Estimated Reading Time: 20 minutes

Part 2 teaches each testing type in depth, one at a time. This guide does the opposite job: it
puts eleven of the most-confused testing types **side by side** — Smoke, Sanity, Regression,
Retesting, System, Integration, End-to-End, UAT, API, UI, and Exploratory — so you can see at a
glance when each one runs, who owns it, what it's actually for, how it's done, and what it looks
like on a real project. Bookmark this one; it's built for skimming during an interview prep
session or a "wait, what's the difference between Sanity and Smoke again?" moment.

---

## 📋 Table of Contents

1. [The Comparison Table](#the-comparison-table)
2. [The Confusing Pairs, Untangled](#the-confusing-pairs-untangled)
3. [Recommended Execution Sequence](#recommended-execution-sequence)
4. [The Flow Diagram](#the-flow-diagram)
5. [The Frequency Pyramid](#the-frequency-pyramid)
6. [📌 Fact Sheet — 60-Second Version](#-fact-sheet--60-second-version)

---

## The Comparison Table

| Testing Type | When in the SDLC | Who Performs It | Purpose / Goal | How It's Executed | Real-World Examples (2, from portfolio repos) |
|---|---|---|---|---|---|
| **Smoke** | Immediately after a new build is deployed, *before* any deeper testing starts | QA (often automated as a CI gate); sometimes Dev/DevOps | Build acceptance — "is this build even stable enough to test?" | A short, shallow pass over the most critical paths only — login, homepage, core navigation | **1)** [Reseller Management Platform](https://github.com/ghanendra-sdet/reseller-management-platform): a 5-minute post-deployment check (login, dashboard load, merchant list render) run before any real regression begins.<br>**2)** [BBPS Bill Payment Platform](https://github.com/ghanendra-sdet/bbps-bill-payment-platform): confirming the biller category list actually loads and a single bill fetch succeeds before testing any specific biller flow in depth |
| **Sanity** | After a *specific* small fix or minor change, before committing to a full regression cycle | QA | Confirm one narrow area works after a targeted change — not "is the build stable," but "did *this* fix behave" | A focused, unscripted check around the changed area only, no test cases required | **1)** After [BUG-COL-1078](https://github.com/ghanendra-sdet/fintech-collection-engine/blob/main/sample-defect-report.md) (GST rounding, Fintech Collection Engine) shipped, a sanity check re-viewed the transaction detail screen and the CSV export side by side — nothing else.<br>**2)** After [BUG-HRM-7021](https://github.com/ghanendra-sdet/hrms-platform/blob/main/sample-defect-report.md) (an editable DOB field on HRMS Platform ESS) was fixed, a sanity check confirmed the field was locked — without re-running the other 13 fields in that form |
| **Regression** | After *any* code change, before every release | QA (heavily automation-driven) | Prove existing functionality still works — nothing that used to pass has silently broken | Full or risk-prioritized re-run of the existing suite | **1)** [Fintech Collection Engine](https://github.com/ghanendra-sdet/fintech-collection-engine)'s real 64-case `regression-checklist.md`, run before every release across Login → Dashboard → Collection → Settlement → Reports.<br>**2)** [HRMS Platform](https://github.com/ghanendra-sdet/hrms-platform)'s ESS `regression-checklist.md` — the full 14-field Personal/Contact Details matrix re-verified every release, not just the field that last changed |
| **Retesting** (Confirmation Testing) | Immediately after a specific defect is marked Fixed | QA — ideally the same tester who logged the original defect | Confirm *that exact bug* is gone — different question from regression, which asks "did I break something else" | Re-execute the *exact* steps to reproduce from the original defect report | **1)** [BUG-PAY-3081](https://github.com/ghanendra-sdet/fintech-payout-engine/blob/main/sample-defect-report.md) (duplicate payment on retry, Fintech Payout Engine) was retested by re-running its original repro steps verbatim before it moved to Verified.<br>**2)** [BUG-HIP-6014](https://github.com/ghanendra-sdet/healthcare-insurance-platform/blob/main/sample-defect-report.md) (Member portal showing "Final" while Payer still showed "Need Review") was retested by re-checking that exact cross-portal pairing after the fix, not the whole claims module |
| **System** | After integration testing, on a complete, feature-frozen build | QA team, black-box | Validate the **entire system** end-to-end against requirements, as one product — not module by module | Full functional + non-functional suite executed against a near-production environment | **1)** [Healthcare Insurance Platform](https://github.com/ghanendra-sdet/healthcare-insurance-platform) tested as one complete system across all 4 portals (Provider/Payer/Employer/Member) before UAT sign-off.<br>**2)** [Travel Marketplace Platform](https://github.com/ghanendra-sdet/travel-marketplace-platform) tested as a whole product — search, compare, fare-lock, book, and pay — before it's ever exposed to real third-party suppliers in E2E |
| **Integration** | After unit testing, as soon as two or more modules/services are wired together | QA + Dev, often collaboratively | Verify the *interfaces* and data handoff between components — the seams, not the parts | Target the specific boundary between two services; verify data survives the handoff correctly | **1)** [Fintech Collection Engine](https://github.com/ghanendra-sdet/fintech-collection-engine)'s Settlement Calculation Service → Ledger Service boundary — the exact seam where [BUG-COL-1042](https://github.com/ghanendra-sdet/fintech-collection-engine/blob/main/sample-defect-report.md) (missing ledger debit entry) originated.<br>**2)** [AI Dispute Resolution Engine](https://github.com/ghanendra-sdet/ai-dispute-resolution-engine)'s read-boundary into six upstream products (Collection, Payout, Connected Banking, BBPS, Reseller, YOBO) — data has to arrive intact from each before the AI can correlate anything |
| **End-to-End (E2E)** | Late-stage, on a fully integrated build including real/near-real third-party dependencies | QA | Validate a complete real-world business journey across the *whole* system, not just internal modules | Simulate the full user journey start to finish, including external integrations | **1)** [Travel Marketplace Platform](https://github.com/ghanendra-sdet/travel-marketplace-platform)'s full search → fare-lock → book → pay flow, running live against third-party supplier APIs.<br>**2)** [BBPS Bill Payment Platform](https://github.com/ghanendra-sdet/bbps-bill-payment-platform)'s complete discover-biller → fetch-bill → pay → settlement chain, run all the way through an actual external biller integration, not mocked |
| **UAT** (User Acceptance Testing) | Final stage, right before release, in a staging/UAT environment | **Business stakeholders / actual end users** — QA facilitates but doesn't own the pass/fail call | Confirm the system actually meets business needs and is genuinely ready for real users | Business-scenario-based walkthroughs performed by the people who'll actually use it | **1)** [HRMS Platform](https://github.com/ghanendra-sdet/hrms-platform)'s ESS module UAT — real HR stakeholders validating the employee self-service flows before company-wide rollout.<br>**2)** [Healthcare Insurance Platform](https://github.com/ghanendra-sdet/healthcare-insurance-platform)'s claims UAT — actual Payer and Provider-side stakeholders walking through real claim scenarios before go-live, since QA alone can't judge "does this match how claims actually get adjudicated" |
| **API** | Can start before the UI even exists; continues through integration and system testing | QA / SDET | Validate business logic, data contracts, and status codes at the service layer, independent of any UI | Direct calls to endpoints (Postman, Playwright API requests) — status codes, response schemas, data correctness | **1)** [BBPS Bill Payment Platform](https://github.com/ghanendra-sdet/bbps-bill-payment-platform)'s biller discovery → bill fetch → pay API chain, tested independently of the biller-selection UI.<br>**2)** [Fintech Collection Engine](https://github.com/ghanendra-sdet/fintech-collection-engine)'s transaction-status, commercial/GST-calculation, and settlement APIs — validated directly against expected values before the dashboard ever renders them |
| **UI** | Once a build is deployed to a testable environment; continues through system testing | QA | Validate look, layout, responsiveness, and UI-level functional correctness | Manual exploration plus scripted checks — layout, cross-device/cross-browser rendering, interaction correctness | **1)** [Travel Marketplace Platform](https://github.com/ghanendra-sdet/travel-marketplace-platform): verifying a fare shown on mobile matches the same fare on desktop, pixel-for-pixel in the number, not just "close enough".<br>**2)** [LMS Platform](https://github.com/ghanendra-sdet/lms-platform): verifying a course progress bar and certificate preview render correctly across screen sizes — a visually broken certificate undermines the whole "credential" premise of the product |
| **Exploratory** | Any stage — most valuable early (requirements are shaky) and for high-risk/complex areas | Experienced QA, unscripted | Uncover defects scripted test cases wouldn't think to check, via simultaneous learning + test design + execution | Time-boxed, charter-driven sessions (Session-Based Test Management) with real-time notes, not pre-written steps | **1)** [AI Dispute Resolution Engine](https://github.com/ghanendra-sdet/ai-dispute-resolution-engine): a chartered exploratory session specifically probing edge cases in the AI's dispute recommendations — the kind of thing no one would think to script in advance.<br>**2)** [Travel Marketplace Platform](https://github.com/ghanendra-sdet/travel-marketplace-platform): an exploratory charter around third-party supplier flakiness (slow responses, stale prices, mid-session timeouts) — exactly the unpredictable conditions a fixed script can't anticipate |

> [!TIP]
> **🎭 Meme Break — Distracted Boyfriend**
>
> 🚶 *The tester, walking with:* **"Running the full 64-case regression suite for the third time
> today"**
> 👀 *Looking back at:* **"That ONE exploratory session no one scheduled, that just found a
> Blocker in 20 minutes"**

---

## The Confusing Pairs, Untangled

Four pairs get mixed up constantly. Here's the one-line difference for each:

| Pair | The Mix-Up | The Actual Difference |
|---|---|---|
| **Smoke vs. Sanity** | "Aren't they both quick checks after a build?" | Smoke asks *"is this build stable enough to test at all?"* (broad, shallow, every build). Sanity asks *"did this one specific fix work?"* (narrow, deep, only after a targeted change). |
| **Sanity vs. Retesting** | "Aren't they both about a fix?" | Sanity checks the **area around** a change with no fixed script. Retesting re-runs the **exact original failing steps** from one specific defect report — nothing more, nothing less. |
| **System vs. Integration** | "Isn't the whole system just integrated modules?" | Integration testing checks the **seams** between two or three components. System testing checks the **entire product** as a black box, against requirements, after all the seams are already presumed sound. |
| **System vs. E2E** | "Isn't E2E just System testing renamed?" | System testing validates the product **on its own**. E2E testing validates a real business journey **including external, third-party dependencies** the product doesn't control (payment gateways, supplier APIs, SMS providers). |

<details>
<summary>🧠 <strong>Quick Check:</strong> A tester finds that after fixing BUG-COL-1078 (GST rounding), the fix works — but nobody re-ran the other 63 cases in the Collection Engine regression suite before release. What went wrong, and which two testing types does this expose the gap between?</summary>

The team did **Retesting** (confirmed BUG-COL-1078 itself was fixed) but skipped **Regression**
(confirming nothing *else* broke as a side effect of the fix). Retesting only ever answers "is
this one bug gone?" — it says nothing about whether the fix touched shared code (like a shared
GST-rounding function) that other flows also depend on. That's exactly why release checklists
require both: retest the specific fix, then regression-test the surrounding system.

</details>

---

## Recommended Execution Sequence

There's no single universal order — it depends on release cadence and risk — but this is the
sequence most real projects converge on, mapped against the SDLC:

1. **Smoke** — the moment a new build lands in a testable environment. If it fails, the build is
   rejected and nothing below this line even starts.
2. **Integration** — as soon as the relevant modules/services are wired together, tested at their
   boundaries.
3. **API** — runs in parallel with (often *before*) UI testing, since the service layer is
   frequently ready before the UI is.
4. **System** — once the build is feature-complete and integration-stable, tested as one product.
5. **UI** — woven through System testing: layout, responsiveness, and interaction correctness.
6. **Regression** — every time a change lands, re-validating what already worked.
7. **Sanity** — after each individual fix, before that fix is folded into the next regression run.
8. **Retesting** — immediately confirms each specific defect is actually closed.
9. **Exploratory** — runs continuously alongside all of the above, but gets a dedicated,
   time-boxed session before any major release — this is where scripted testing's blind spots
   get caught.
10. **End-to-End** — once the system is stable, validated against real (or near-real) third-party
    integrations.
11. **UAT** — the final gate. Business stakeholders, not QA, decide go/no-go here.

> [!TIP]
> **🎭 Meme Break — Expanding Brain**
>
> 🧠 *Level 1: "We'll just do regression testing at the end."*
> 🧠🧠 *Level 2: Running smoke tests on every build so broken builds get rejected in minutes, not
> days.*
> 🧠🧠🧠 *Level 3: Layering sanity + retesting + regression so each fix is both confirmed AND
> proven not to have broken anything else.*
> 🧠🧠🧠🧠 *Level 4: Booking a dedicated exploratory charter before every release, because the
> scripted suite — however good — only ever finds what it was written to look for.*

---

## The Flow Diagram

**Row 1 — while the build is stabilizing:**

```mermaid
flowchart LR
    A["Smoke Testing<br/>Build check"] --> B["Integration<br/>Modules linked"] --> C["API Testing<br/>Service layer"] --> D["UI Testing<br/>Frontend checks"]

    classDef smoke fill:#dbeafe,stroke:#1e40af,color:#1e3a8a
    class A,B,C,D smoke
```

> [!NOTE]
> **Sanity, Retesting, and Exploratory testing aren't a fixed stop on this line at all** — they
> run continuously, threaded through both rows, every single time a fix lands or a build moves
> forward.

**Row 2 — once the build is stable, through to release:**

```mermaid
flowchart LR
    E["System Testing<br/>Full app check"] --> F["End-to-End<br/>User journeys"] --> G["Regression<br/>No breakage"] --> H["UAT<br/>Client approval"] --> I(["Release"])

    classDef system fill:#d1fae5,stroke:#065f46,color:#065f46
    classDef regression fill:#fee2e2,stroke:#991b1b,color:#991b1b
    classDef uat fill:#ede9fe,stroke:#5b21b6,color:#5b21b6
    classDef release fill:#f3f4f6,stroke:#374151,color:#111827

    class E,F system
    class G regression
    class H uat
    class I release
```

Row 1 feeds directly into Row 2 (UI Testing passing means the build is ready for System Testing),
and Row 2 ends in the only two outcomes that matter: **UAT sign-off → Release**, or a defect that
sends the build back through Sanity → Retesting → Regression before it re-enters this line.

<details>
<summary>🧠 <strong>Quick Check:</strong> In the diagram above, why does the loop from "Bugs Found?" go through Sanity → Retesting → Regression — in that specific order — rather than straight back to System Testing?</summary>

Because each of those three answers a different, narrower question in increasing order of scope:
Sanity checks the area around the fix (fast, no script), Retesting proves *that exact defect* is
gone (using its original repro steps), and Regression proves the fix didn't break anything else
in the wider system. Skipping straight back to System Testing would waste time re-running
everything broad before confirming the fix even worked narrowly — cheaper checks come first.

</details>

---

## The Frequency Pyramid

Not every testing type runs equally often. Some run on *every single build* (many times a day);
others run once, right before release. Picturing this as a pyramid — frequent-and-shallow at the
top, rare-and-broad at the bottom — helps explain why teams automate the top of the pyramid first:

```
Frequency & Depth of Execution
═══════════════════════════════════════════════════════════════

Smoke            ████████████████████████████████  Every single build (minutes)
Sanity           ██████████████████████████████     After every targeted fix
Retesting        ████████████████████████████       After every defect closure
API              ████████████████████████           Continuously, often pre-UI
Integration      ██████████████████████              Every service boundary change
UI               ████████████████████                 Every UI-affecting build
Regression       ██████████████                         Every release candidate
Exploratory      ████████████                             Chartered sessions, pre-release
System           ████████                                    Feature-complete builds
End-to-End       ██████                                       Late-stage, pre-release
UAT              ██                                              Once, final gate
```

> [!IMPORTANT]
> **Read the pyramid as "how often," not "how important."** UAT sits at the bottom because it
> happens once — not because it matters least. A missed UAT sign-off blocks a release just as
> hard as a failed smoke test blocks a build. The shape explains automation priority (automate
> the top first, where the same checks repeat constantly) — it says nothing about severity.

---

## 📌 Fact Sheet — 60-Second Version

- **Smoke** = "is the build even worth testing?" **Sanity** = "did this one fix work?" Different
  questions, different scope, both fast and unscripted.
- **Retesting** re-runs one specific defect's original repro steps. **Regression** re-runs the
  broader suite to catch unrelated side effects. Do both, in that order, after every fix.
- **Integration** tests the seams between components. **System** tests the whole product as one
  black box. **End-to-End** adds real third-party dependencies the product doesn't control.
- **UAT is the only testing type in this table where QA doesn't make the pass/fail call** —
  business stakeholders do; QA facilitates.
- **API testing doesn't need a UI to exist** — it's often the earliest thing testable once a
  service is deployed, which is why mature teams start there.
- **Exploratory testing is unscripted by design** — its value is finding what a written test case
  wouldn't have thought to check, via chartered, time-boxed sessions (SBTM), not random clicking.
- The realistic execution order is **Smoke → Integration → API/System/UI → (Sanity → Retesting →
  Regression, looped per fix) → Exploratory → End-to-End → UAT** — not a strict waterfall, but
  the sequence most release cycles converge on.
- The **frequency pyramid** explains automation priority: automate what repeats constantly
  (Smoke, Sanity, Regression) before what happens once (UAT) — frequency, not importance, drives
  that order.
- Every real-world example on this page links back to an actual defect or checklist from this
  account's portfolio projects — cross-reference [Part 2](./Part_02_Types_of_Testing.md) for the
  full deep-dive on each type individually.

---

*Companion to [Part 2: Types of Testing](./Part_02_Types_of_Testing.md). Part of the
[Manual Testing Playbook](./00_Table_of_Contents.md).*
