# Changelog

All notable changes to the CLARA Hallucination Audit v2 bundle are recorded here. The bundle follows semantic versioning at the bundle level (not at the level of individual files inside it).

The OSF preregistration freezes a specific tag. Cosmetic post-tag fixes (typos, broken links) are logged here without bumping the registered version. Any change to the dataset, the taxonomy, or the methodology requires a new bundle version and a new OSF registration that cites the prior one.

## [Unreleased]

_Reserved for post-v1.0.0 typo / link fixes that do not require a new OSF registration._

## [1.0.0] — Pending OSF submission

Initial release for OSF preregistration.

### Added
- `README.md` — bundle overview and honest-framing disclosures.
- `METHODOLOGY.md` — dataset construction, run protocol, hand-coding procedure, threats to validity.
- `prereg/preregistration.md` — research questions, hypotheses, stop rules, analysis plan.
- `taxonomy/taxonomy.md` — H1–H7 + OTHER definitions, severity weights, out-of-scope categories.
- `data/clara_v2_audit_queries_200.csv` — 200 frozen queries (80 / 50 / 30 / 40 distribution; 37 trap-tagged; 14 anchored-tagged; 20 domains).
- `build/build-v2-200queries.py` — deterministic stdlib-only Python builder for the dataset.
- `LICENSE-CODE` (MIT) and `LICENSE-DATA` (CC-BY-4.0).

### Pinned
- CLARA commit `6c82d7001a241ec00d0f6bf458302d7431e9cc3c` (April 22, 2026) as the system under test for the v2 audit.
