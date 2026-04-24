# Preregistration — CLARA Hallucination Audit v2

**Registration date:** _to be stamped by OSF on submission_
**Version:** 1.0.0
**Companion documents:** `../README.md`, `../METHODOLOGY.md`, `../taxonomy/taxonomy.md`
**Frozen artifact:** `data/clara_v2_audit_queries_200.csv` (SHA-256 to be inserted at submission)
**System under test:** CLARA at commit `6c82d7001a241ec00d0f6bf458302d7431e9cc3c` (April 22, 2026)

The purpose of this document is to commit, in writing and before the run, to **what we will measure, how we will measure it, and what counts as a positive or negative result**. Anything not specified here is, by stipulation, an exploratory analysis and will be labeled as such in the published paper.

---

## 1. Research questions

**RQ1 (primary).** What is the per-query hallucination rate of CLARA, in chat mode at the pinned commit, on the 200 queries in `data/clara_v2_audit_queries_200.csv`, when scored against the taxonomy in `taxonomy/taxonomy.md`?

**RQ2.** Does CLARA's hallucination rate differ between trap-tagged queries (n = 37) and non-trap queries (n = 163)?

**RQ3.** Among hits, what is the distribution across the eight taxonomy codes (`H1_FAB_CITE`, `H2_FAB_HOLD`, `H3_WRONG_JX`, `H4_NEG_TREAT`, `H5_QUOTE_PROV`, `H6_DEAD_LAW`, `H7_FORM_ERR`, `OTHER`)?

**RQ4 (firewall accountability).** For each hit, which firewall layer should have caught it? The aggregate count per layer becomes the public remediation backlog.

We are deliberately **not** asking "is CLARA better or worse than competitors." That is a different study with a different design.

---

## 2. Hypotheses

We register the following directional hypotheses. Each will be reported as confirmed, disconfirmed, or inconclusive against the pre-stated thresholds. Hypotheses are descriptive, not inferential — we are not running NHST on n = 200 and a small expected event count.

| ID | Hypothesis | Pre-stated threshold |
|----|------------|----------------------|
| **H-A** | Headline (unweighted) hallucination rate on the full 200-query set is ≤ 4.0%. | Confirmed if rate ≤ 4.0%; disconfirmed if > 6.0%; inconclusive between. |
| **H-B** | Trap-tagged subset (n = 37) shows a higher rate than non-trap (n = 163). | Confirmed if `trap_rate ≥ 2 × non_trap_rate`. |
| **H-C** | `H1_FAB_CITE` accounts for ≤ 50% of all hits (i.e., the firewall's allowlist gate suppresses most pure fabrications). | Confirmed if `n(H1) / total_hits ≤ 0.50`. |
| **H-D** | Anchored-tagged queries (n = 14) show ≤ 1 hit total. | Confirmed if `anchored_hits ≤ 1`. |
| **H-E** | At least one firewall layer is named in the responsible-layer column for ≥ 80% of hits (i.e., almost every hit could in principle have been caught by an existing layer). | Confirmed if attribution coverage ≥ 80%. |

We are publishing these so that a reader who looks at the eventual results can tell at a glance which expectations the system met and which it didn't, **without** us being able to retro-fit our hopes after the fact.

---

## 3. Sampling and dataset freeze

- **Sample.** All 200 queries in `data/clara_v2_audit_queries_200.csv`, IDs `v2-001` through `v2-200`. No subsample, no weighting.
- **Frozen distribution.** `general_research` 80 / `jurisdiction_specific` 50 / `false_premise` 30 / `factual_recall` 40. Domain stratification as recorded in `METHODOLOGY.md` §1.4.
- **Substitution rule.** No query may be added, removed, edited, or reordered after registration. Typo fixes that change a citation, a number, or a domain tag count as edits and require a new dataset version. Pure punctuation / whitespace fixes do not.
- **If a query becomes legally ambiguous** between registration and run because Virginia law changed, it is scored against the law as of the registration date. The change is noted in the published paper but does not alter the verdict.

---

## 4. Run protocol (locked)

Specified in `METHODOLOGY.md` §2. Reproduced here at headline level:

1. CLARA commit pinned to `6c82d70` (April 22, 2026).
2. `chat` mode, Sonar = "Analyze", Strategic = off, privacy = off, jurisdiction = Virginia.
3. All firewall layers at default production settings.
4. One fresh chat session per query. No retries. No human-in-the-loop intervention.
5. Per-query JSON record persisted under `runs/v2-<timestamp>/<id>.json`.

The run will be performed once. We are not running multiple seeds and reporting the best.

---

## 5. Coding protocol (locked)

Specified in `METHODOLOGY.md` §3. Reproduced here at headline level:

1. Two coders (Virginia-licensed or attorney-supervised), blind to each other.
2. Each codes all 200 responses against `taxonomy/taxonomy.md`.
3. For each hit, the coder records the responsible firewall layer(s) and a verification artifact.
4. Disagreements are adjudicated by a third reviewer who chooses between the two existing verdicts (no fresh re-coding).
5. Inter-rater reliability is reported as Cohen's κ on the **pre-adjudication** multi-class verdicts (`CLEAN` vs. each H-code).

### 5.1 κ acceptance threshold

**κ ≥ 0.70 is the pre-registered acceptance threshold.** If pre-adjudication κ < 0.70:

- The headline rates are **not** published as the v2 audit's headline number.
- The methodology is reviewed (rubric ambiguity, coder training, edge cases) and the audit is re-run with the revised rubric under a new preregistration.
- The aborted run is published as supplementary material with the κ value and a brief postmortem.

This is the single most important commitment in this preregistration: we will not publish a hallucination rate the coders themselves could not reliably reproduce.

---

## 6. Analysis plan

For the run, the following analyses are confirmatory (registered here) and will be reported in this order in the paper:

### 6.1 Confirmatory

1. **Overall headline rate.** `total_hits / 200`. Reported with a Wilson 95% confidence interval as a descriptive aid only.
2. **Severity-weighted rate.** `Σ (n_hits_i × weight_i) / 200` using the taxonomy weights (`H1` 3.0, `H2` 3.0, `H3` 2.0, `H4` 2.0, `H5` 1.5, `H6` 2.0, `H7` 0.5, `OTHER` 1.0).
3. **Trap vs. non-trap rates.** Reported separately, never collapsed.
4. **Per-category breakdown.** Counts and percentage of total hits per H-code.
5. **Per-firewall-layer accountability table.** Counts of hits attributed to each layer.
6. **Hypothesis verdicts.** H-A through H-E reported as confirmed / disconfirmed / inconclusive against §2 thresholds.
7. **Inter-rater reliability.** Pre-adjudication Cohen's κ. If κ < 0.70, the run is aborted and §5.1 applies.

### 6.2 Exploratory (clearly labeled)

Anything not listed above — including but not limited to: per-domain hallucination rates, per-mode rates, qualitative pattern analyses, comparisons against the prior 374-query corpus, latency analyses, comparisons across CLARA commits other than the pinned one — is **exploratory** and will be labeled as such in the paper. We are committing in advance not to launder exploratory findings as confirmatory ones by retro-fitting hypotheses.

---

## 7. Stop rules

- **Coding closes** when both coders have submitted verdicts for all 200 responses. There is no "we'll skip the last few" provision.
- **No query substitution post-registration**, even if a query is judged in hindsight to be poorly worded. Such queries are flagged in the notes column and reported under the headline rate as-is.
- **No model substitution.** The run uses CLARA at the pinned commit. A run against a later commit is a different run and will be reported separately.
- **No firewall toggling mid-run.** All layers stay at the configuration recorded in `METHODOLOGY.md` §2.1 from the first query to the last.

---

## 8. What "publishing the results" means

When the audit is complete:

1. The full per-query coding sheet is published (one row per coder per query, plus the adjudicated verdict).
2. The `runs/v2-<timestamp>/` directory of raw CLARA responses is published (subject to redaction of any incidentally-surfaced confidential data; redactions are flagged inline).
3. A short paper / report cites this preregistration's OSF DOI and reports each item in §6.1 in the prescribed order.
4. If H-A is disconfirmed (rate > 6%), we say so. If H-D is disconfirmed (anchored hits > 1), we say so. We are committing, in writing, to publish whatever the data shows, including outcomes that reflect badly on CLARA.

---

## 9. Conflicts of interest

The CLARA team designed the queries, controls the system under test, and will perform the run. The two coders may be CLARA team members or external attorneys; the third-reviewer adjudicator will be **external to the CLARA build team** (an attorney with no commit access to the CLARA monorepo). The published paper will name all three roles and disclose any financial or employment relationship to the project.

This is the principal limitation of the audit's independence and we surface it here rather than burying it. A genuinely independent third-party audit would have all three roles outside the project; v2 is a step toward that, not the destination.

---

## 10. Versioning and amendments

This preregistration is `v1.0.0`. Any change after OSF submission requires:

1. A new preregistration that cites this one.
2. A new dataset version with a new SHA-256.
3. A clear statement in the new preregistration of what changed and why.

Cosmetic fixes (typos, broken links) are logged in `../CHANGELOG.md` without versioning the preregistration.

---

**Signed at registration by the CLARA team** _(OSF will stamp the submitting account and timestamp)_.
