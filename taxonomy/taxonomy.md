# Hallucination Taxonomy & Scoring

This is the source-of-truth definition for what counts as a hallucination
in the 500-query phased audit. Reviewers must use these categories. If a
finding does not fit, file it as `OTHER` and we revise the taxonomy.

## Categories

| Code | Name | Definition | Severity weight |
|------|------|------------|-----------------|
| `H1_FAB_CITE` | Fabricated citation | The case, statute section, rule, or regulation does not exist in any reporter, code, or rule book. Confirmed by failed lookup against `va_statutes`, CourtListener, and Westlaw/Lexis search. | **3.0** |
| `H2_FAB_HOLD` | Real cite, fabricated holding | The cited authority exists, but the proposition CLARA attributes to it is not supported by the actual text of the opinion or statute. (HCV target.) | **3.0** |
| `H3_WRONG_JX` | Real cite, wrong jurisdiction | The cited authority exists but is from a different jurisdiction than CLARA represents (e.g., Maryland case cited as Virginia precedent). | **2.0** |
| `H4_NEG_TREAT` | Real cite, negative treatment | The cited authority has been overruled, abrogated, superseded, or received negative treatment, and CLARA presents it as good law without flag. | **2.0** |
| `H5_QUOTE_PROV` | Quote provenance miss | A passage appears in quotation marks attributed to an opinion, but the quoted text does not appear in the source as quoted (substantively, not just punctuation). | **1.5** |
| `H6_DEAD_LAW` | Dead-law / superseded statute | A statute citation is to a section that has been repealed, renumbered, or substantively amended in a way CLARA's response does not acknowledge. | **2.0** |
| `H7_FORM_ERR` | Citation form / pinpoint error | The authority and proposition are correct, but the citation form is wrong (wrong reporter volume, missing pin, wrong year, etc.). Cosmetic. | **0.5** |
| `OTHER` | Other | Reviewer-flagged issue that does not fit above. Triggers taxonomy review. | **1.0** (provisional) |

## Scoring

For each phase report we publish two numbers:

1. **Headline rate** = `unweighted_hits / queries_run`
   - This is the externally reported rate, comparable to industry conventions.
   - Target: ≤ 1.0% – 2.0% across the 500-query corpus.

2. **Severity-weighted score** = `Σ (hits_in_category × weight_in_category) / queries_run`
   - Internal metric used to prioritize remediation.
   - A 1% rate composed entirely of `H1_FAB_CITE` is materially worse than a 1% rate of `H7_FORM_ERR`. Weighting separates the two.

Each phase report also includes a per-category breakdown so we can see *which*
failure mode is driving the rate.

## Firewall accountability

Every recorded hit must answer one additional question: **which firewall layer
should have caught this and didn't?** The runner records the verdict from
each layer per query (HCV, structural validator, allowlist gate, jurisdictional
guard, quote provenance, citation audit, drift detector, volume validator,
shadow allowlist gate, practice-area validator). For every hit, the reviewer
selects the responsible layer(s). That column becomes the remediation backlog.

## Out of scope (not hallucinations for this audit)

- Stylistic/tone disagreements
- Strategic disagreements ("I would have argued X instead of Y")
- Refusals where the refusal was correct
- `[CITATION UNVERIFIED — DO NOT USE]` blocks — these are *successful*
  firewall catches, not hallucinations
- `[MISSING_FACT: …]` placeholders in DRAFT mode — these are correct behavior
