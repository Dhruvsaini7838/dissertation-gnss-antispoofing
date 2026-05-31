# Multi-Heuristic GNSS Anti-Spoofing for Location-Bound Cryptographic Systems
**MSc Dissertation : University of Surrey**
**Student:** Dhruv Saini · 6898312 · MSc Cyber Security (Placement Year)
**Supervisor:** Prof Ioana Boureanu · Surrey Centre for Cyber Security
**Submission:** September 2026
---
## Overview
This repository contains the research code and evaluation pipeline for an MSc dissertation investigating whether heuristic-based GNSS anti-spoofing methods provide sufficient entropy unpredictability guarantees for use as a cryptographic gate in location-bound key derivation systems.
The core contribution is a **four-heuristic anti-spoofing ensemble** evaluated against a labelled GNSS spoofing dataset. The research reframes GNSS spoofing not just as a navigation safety problem, but as a **cryptographic entropy injection attack** — where a spoofed signal fed into an HKDF-based key derivation function can reduce the unpredictability of the derived key material.
This dissertation evaluates one subsystem of a broader location-bound encryption architecture. The full key derivation pipeline is out of scope for this repository.
---
## Research Questions
### RQ1 — Entropy Unpredictability
> Do heuristic-based GNSS anti-spoofing methods provide sufficient entropy unpredictability guarantees for cryptographic key derivation use cases?
- **Part 1a:** What is the detection rate (DR) and false positive rate (FPR) of the four-heuristic ensemble against labelled spoofing events?
- **Part 1b:** What is the min-entropy reduction in HKDF key outputs when a spoofed GNSS input passes the ensemble undetected?
### RQ2 — Threshold Comparison
> How do the detection thresholds required for cryptographic entropy integrity differ from those in GNSS navigation safety literature?
- **Part 2a:** What false-negative tolerances do navigation safety standards accept (minimum detection rate thresholds)?
- **Part 2b:** Are those tolerances appropriate for a non-recoverable, one-shot cryptographic key derivation context?

---
## The Four Heuristics
| ID | Heuristic | Signal Feature | Spoofing Indicator |
|----|-----------|---------------|--------------------|
| H1 | SNR Drop | Per-satellite signal-to-noise ratio | Sudden, simultaneous drop across multiple satellites |
| H2 | Uniform SNR | SNR variance across constellation | All satellites converge to near-identical SNR — unnatural in authentic conditions |
| H3 | Velocity Anomaly | Position deltas over time | Kinematically implausible transition between consecutive position fixes |
| H4 | Multi-Constellation Divergence | Cross-system position fix | Inconsistent position reported by GPS, GLONASS, and Galileo simultaneously |
Each heuristic produces a per-epoch confidence score. The ensemble combines these into a single `spoof_confidence` value, thresholded for classification.
---
## Datasets
| Dataset | Source | Licence | Role |
|---------|--------|---------|------|
| Mendeley GNSS Interference & Spoofing Part III | [data.mendeley.com](https://data.mendeley.com/datasets/nxk9r22wd6/3) | CC BY 4.0 | Primary — labelled spoofing ground truth for DR/FPR computation |
| IGS GPS RINEX | [igs.org/data](https://igs.org/data) | Open access | Primary — authentic baseline for entropy measurement and FPR evaluation |
| CICIDS2017 | [unb.ca/cic/datasets](https://www.unb.ca/cic/datasets/ids-2017.html) | Open research | Secondary — network anomaly features; defer if time-constrained |
Full download instructions, station IDs, and preprocessing notes are in [`data/dataset_sources.md`](data/dataset_sources.md).
---
## Evaluation Metrics
| Metric | Definition | Maps to |
|--------|-----------|---------|
| Detection Rate (DR) | TP / (TP + FN) — proportion of spoofing epochs correctly classified | RQ1a |
| False Positive Rate (FPR) | FP / (FP + TN) — proportion of authentic epochs incorrectly flagged | RQ1a |
| Min-entropy (H∞) | Worst-case unpredictability of HKDF key output under spoofed input | RQ1b |
| MDR threshold delta | Gap between navigation safety MDR and cryptographic requirement | RQ2 |
------
## Dissertation Structure
| Chapter | Content |
|---------|---------|
| 1 — Introduction | Problem framing, aim, objectives, dissertation outline |
| 2 — Literature Review | GNSS anti-spoofing state of the art; gap in cryptographic threshold analysis |
| 3 — Research Approach | Methodology justification, dataset selection, evaluation design |
| 4 — System Design & Development | Four-heuristic ensemble design and implementation |
| 5 — Evaluation | DR/FPR results, min-entropy analysis, threshold comparison (RQ1 & RQ2) |
| 6 — Conclusion | Contributions, limitations, future work |
---
## Contact
**Dhruv Saini**
MSc Cyber Security (Placement Year) · University of Surrey
ds01903@surrey.ac.uk
