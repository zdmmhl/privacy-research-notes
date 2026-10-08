# Review Notes for the Historical DP Proposal

This is shared team research planning from 2025. It includes no implemented toolkit or completed privacy/utility evaluation.

Important corrections:

- Smaller epsilon provides a stronger differential-privacy bound, with delta and the adjacency definition held fixed. The historical literature section reverses this direction in one sentence.
- Differential privacy is not “dynamic programming”; “Dukov” is a mistaken rendering of Dwork.
- Federated learning keeps data local but does not by itself provide differential privacy or prevent inference from updates.
- PATE privatizes teacher voting and accounts for its queries; it does not inherently require DP-SGD gradient clipping in its teachers.
- Differential privacy bounds distributions under a specified neighboring-data model. It is not a guarantee of zero disclosure, anonymity, universal attack immunity, or legal compliance.
- The source's prediction that accuracy will decrease only slightly is a proposed expectation, not a measured outcome. User-level versus record-level protection and contribution limits must be specified.
- Literature summaries, deployment examples, and the claimed enterprise research gap were not comprehensively revalidated for this archival edition.
- Several paragraphs are duplicated, and bibliographic years and author names need reconciliation against the original papers.
- Proposed timelines, prototypes, evaluation metrics, and business benefits remain plans.

For evaluating the scope of a guarantee, use the primary [NIST SP 800-226 guidance](https://csrc.nist.gov/pubs/sp/800/226/final), rather than treating proposal prose as validated evidence.
