# CLARA Hallucination Audit — v2 (200-Query Independent Validation)

**A methodology-first preregistration**

---

## Project at a glance

CLARA is a Virginia-specific legal AI assistant built around a multi-layer hallucination firewall (allowlist-first citation gating, Virginia Code structural validation, reporter volume validation, holding-coherence checking, quote provenance, jurisdiction locking, dead-law detection, and shadow-mode AG-opinion and negative-treatment verifiers, among others). This project preregisters a fixed 200-query dataset and a hand-coding rubric that will be used to measure CLARA's citation-level hallucination behavior at a single pinned commit, before the audit is run and before any results are recorded.

The intent is not to publish a benchmark, and not to compare CLARA against other systems. The intent is to commit, in writing and on a timestamped surface, to a methodology that a third party can reproduce — and then to publish whatever the data shows.

---

## What is being registered

- **Dataset.** 200 Virginia-law queries, IDs `v2-001`–`v2-200`, distribution locked at `general_research` 80 / `jurisdiction_specific` 50 / `false_premise` 30 / `factual_recall` 40. 20 legal-practice domains, with primary weighting on civil procedure, VRLTA, criminal, contracts, defamation, evidence, sovereign immunity, med-mal, and discovery. 37 queries are explicitly trap-tagged (false premises, phantom citations, stale-amendment framings); 14 target CLARA's anchored knowledge blocks.
- **Builder.** A pure-stdlib, deterministic Python script that produces the dataset byte-for-byte. SHA-256 of both files is recorded in the preregistration.
- **Taxonomy.** Eight codes (`H1_FAB_CITE`, `H2_FAB_HOLD`, `H3_WRONG_JX`, `H4_NEG_TREAT`, `H5_QUOTE_PROV`, `H6_DEAD_LAW`, `H7_FORM_ERR`, `OTHER`) with severity weights. Citation fabrication and fabricated-holding errors are weighted 6× higher than form errors; weighted and unweighted rates will both be reported.
- **Run protocol.** A single run against CLARA at commit `6c82d70` (April 22, 2026), `chat` mode, all firewall layers at production defaults, one fresh session per query, no retries, no human-in-the-loop intervention.
- **Coding protocol.** Two coders, blind, against the published rubric. Disagreements adjudicated by a third reviewer (external to the build team) who chooses between the two existing verdicts rather than re-coding from scratch. Pre-adjudication Cohen's κ ≥ 0.70 is the registered acceptance threshold; below 0.70, the headline rate is not published and the audit is re-registered with a revised rubric.

---

## Hypotheses (each with a pre-stated threshold)

- **H-A.** Headline (unweighted) rate on the full 200-query set is ≤ 4.0%; disconfirmed if > 6.0%.
- **H-B.** Trap-tagged subset shows ≥ 2× the rate of the non-trap subset.
- **H-C.** Pure fabricated citations (`H1_FAB_CITE`) account for ≤ 50% of total hits.
- **H-D.** Anchored-tagged queries produce ≤ 1 hit total.
- **H-E.** A responsible firewall layer can be named for ≥ 80% of hits.

Each hypothesis will be reported as confirmed / disconfirmed / inconclusive against the threshold above. Any analysis not listed here is exploratory and will be labeled as such in the published paper.

---

## What this audit is *not*

- **Not** a head-to-head benchmark against other legal AI products. The queries are Virginia-specific and biased toward known CLARA failure modes; the headline rate is a stress-test rate, not an open-domain rate.
- **Not** a replication of prior published legal-AI hallucination studies. This is **different work, in different places, quantified via hand-coding** — independently designed, independently scored, intentionally narrower in scope.
- **Not** automated. Verdicts are produced by human attorneys against the published rubric; no LLM-as-judge.
- **Not** a finished study with results. Results are deliberately withheld until after the methodology is timestamped here.

---

## Independence and conflicts of interest

The CLARA team designed the queries, controls the system under test, and will perform the run. Coders may be CLARA team members or external attorneys; the third-reviewer adjudicator will be external to the CLARA build team. This is the principal limitation of the audit's independence and is disclosed openly here and in the preregistration. v2 is a step toward fully independent third-party evaluation, not the destination.

---

## Reproducibility

Anyone can rebuild the dataset from the bundle:

```
python3 build/build-v2-200queries.py
sha256sum data/clara_v2_audit_queries_200.csv
```

Anyone with API access to CLARA can re-run the audit by checking out the pinned commit and feeding `data/clara_v2_audit_queries_200.csv` through the runner under `tests/hallucination-audit/`. Re-runs against later CLARA commits are explicitly **not** the registered v2 audit and must be reported as separate runs.

---

## Where to find what

| Artifact | Location |
|---|---|
| Full README and honest-framing disclosures | `README.md` |
| Construction, run protocol, coding procedure, threats to validity | `METHODOLOGY.md` |
| Hypotheses, stop rules, analysis plan, κ threshold | `prereg/preregistration.md` |
| Hit-code definitions and severity weights | `taxonomy/taxonomy.md` |
| Frozen 200-query dataset | `data/clara_v2_audit_queries_200.csv` |
| Deterministic builder | `build/build-v2-200queries.py` |
| Licenses (MIT for code, CC-BY-4.0 for data and docs) | `LICENSE-CODE`, `LICENSE-DATA` |
| Live mirror | _GitHub repo URL — added at registration_ |

---

**Bundle version:** 1.0.0 · **CLARA commit pinned:** `6c82d70` (April 22, 2026) · **License:** MIT (code) + CC-BY-4.0 (data, taxonomy, docs)
