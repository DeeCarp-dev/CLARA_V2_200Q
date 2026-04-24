# Relationship to the Five-Phase 374-Query Audit

This bundle (the **v2 200-query preregistration**) is one of two CLARA hallucination-audit deposits on OSF. The other is the **five-phase 374-query registered report** (`clara-audit-374`). They are **complementary, not redundant**, and a reviewer who reads only one is missing half the picture.

This document explains how the two studies relate, what each one is designed to do that the other cannot, and why both are necessary.

---

## At a glance

| Dimension                       | This bundle (v2 200-query)                                              | 374-query bundle                                                              |
|---------------------------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Type**                        | Preregistration (methodology-first)                                     | Registered report (post-hoc archival of completed work)                       |
| **Status**                      | Methodology frozen; results to come                                     | Methodology and results both frozen                                           |
| **Question answered**           | "What rate does CLARA produce on a frozen, single-shot stress test?"    | "What rate did CLARA produce during a five-phase find-and-fix cycle?"        |
| **Run model**                   | Single shot, one CLARA commit, no remediation mid-run                    | Iterative: fixes shipped between phases (Fix #1 – Fix #5)                     |
| **Hand-coding bar**             | Two coders blind + adjudicator; pre-stated Cohen's κ ≥ 0.70 threshold   | Attorney coding with senior-attorney adjudication; κ not formally computed   |
| **Independence**                | Adjudicator external to the build team                                  | Coding team overlaps with build team (disclosed)                              |
| **Bias mitigation**             | Hypotheses and thresholds locked in writing **before** the run          | Three-arm comparison (CLARA post-fix / pre-correction / bare-LLM baseline)   |
| **Comparable to Stanford 2024?**| No (single-jurisdiction, hand-coded against a custom rubric)            | No headline-to-headline; the per-response number (~3.6%) is the closer comparison |
| **Headline metric**             | Per-query hallucination rate, post-firewall, hand-coded against `taxonomy/` | Per-citation 0.43% (post-firewall) and per-response ~3.6% (pre-redaction)   |

---

## What each study can do that the other cannot

### What the 374-query bundle does that this one cannot

- **Reports actual CLARA performance on a substantial query set with a published headline number** (0.43% per-citation, ~3.6% per-response).
- **Provides three-arm side-by-side data** — every prompt has CLARA post-fix, CLARA pre-correction, and bare-LLM baseline responses captured in one CSV. A reader can re-derive any rate they want from the raw data.
- **Documents the find-and-fix cycle** — five architectural fixes, ~14 net-new blocklist entries, and five real-case false-positive corpus closures, all shipped during the audit window. This is what an internal QA pass looks like; it is not what an independent benchmark looks like.
- **Provides the per-firewall-layer accountability table** — `reports/firewall-effectiveness.clara-2026-04-21.md` lists, for each of the ~10 firewall layers, what fraction of fabrication attempts that layer caught. This is the public remediation backlog.

### What this bundle (v2) does that the 374-query bundle cannot

- **Locks the methodology before the results exist.** The hypotheses (H-A through H-E), the κ ≥ 0.70 acceptance threshold, the substitution rule (no query may be added/removed/edited after registration), and the analysis plan are all timestamped on OSF before any v2 query is run. Nothing can be retro-fit.
- **Pre-commits to publishing whatever the data shows.** §8 of `prereg/preregistration.md` is a written commitment to publish disconfirming results. The 374-query audit is also published warts-and-all (Finding F-1, the inverse-coherence canary failures, etc.), but it was not pre-committed in writing.
- **Names an external adjudicator.** §9 of the preregistration commits the third-reviewer adjudicator to be external to the CLARA build team. The 374-query audit's adjudication was internal.
- **Pre-stated κ threshold.** If pre-adjudication κ falls below 0.70, the v2 headline rate is **not** published. This is the single strongest credibility lever in either bundle and is the answer to "your coders work for you — how do we know they were consistent?"

---

## Why we need both

A skeptical reader can attack each study from the opposite direction:

- The 374-query audit has a favorable headline (0.43%) but is not preregistered, the coders work for the build team, and the methodology bundles the find-and-fix cycle into the headline number. A skeptic can reasonably ask: "Would a fresh prompt set, scored by neutral coders, produce the same number?"
- The v2 200-query preregistration answers exactly that question, but **only after it runs**. A skeptic reading v2 alone, before the results land, sees only a methodology document — no number to anchor against.

Together they answer both halves:

1. The 374-query audit shows what a real five-phase find-and-fix cycle produces, with all three arms preserved so any rate can be recomputed.
2. The v2 200-query preregistration commits, in writing and on a timestamped surface, to a single-shot, externally-adjudicated, κ-gated re-measurement on a fresh prompt set — and to publishing the result regardless of which way it cuts.

If you read only the 374-query bundle, you are looking at internal QA. If you read only this bundle, you are looking at a methodology document with no data. Reading both is the audit.

---

## Pointers

- **374-query bundle, OSF DOI:** _to be added once issued_
- **374-query bundle, GitHub:** _to be added once published_
- **374-query bundle, headline document:** `RESULTS.md` inside the 374 bundle (canonical numbers)
- **374-query bundle, methodology disclosure:** `METHODOLOGY.md` §1.2 (iterative remediation) and §3.5 (κ not formally computed)

---

**Both bundles use the same eight-code taxonomy** (`taxonomy/taxonomy.md`, identical across the two bundles). Hits in v2 will be coded against the same definitions used in the 374-query audit, so when v2 results publish they can be set side-by-side with the 374-query per-citation rate without any taxonomy translation.
