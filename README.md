# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 04 — Chapter 13 Benchmark and Falsification Specification**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 04 findings

- The first experiment is now specified as a staged B0–B6 benchmark rather than a single end-to-end comparison.
- The benchmark separates monolithic, independent-specialist, latent-bridge, adaptive-routing, health-aware, regeneration, and stress-adaptation contributions.
- Latent consensus is formally separated from predictive-distribution consensus. KL divergence is restricted to probability distributions in a common simplex.
- Consensus convergence is tied to explicit graph dynamics. For connected undirected linear consensus, `dot{x}=-Lx` gives exponential disagreement decay governed by `lambda_2(L)`; this is not a theorem about cognitive speed.
- Regeneration is defined behaviorally through evaluation loss/performance tolerance rather than an undefined percentage such as “85% regenerated.”
- Byzantine robustness is evaluated under explicit attack and non-IID conditions; optimization guarantees are not transferred automatically to model reconstruction.
- Health detection combines calibration, OOD, disagreement, latency, integrity, and task-error signals rather than entropy alone.
- Anti-fragility is defined more strongly than recovery: improvement must occur on held-out related stress after adaptation, with no unacceptable clean-task regression.
- Self-modification is constrained to a validation-gated architecture-search process.
- Fixed claims such as 60–70% compute reduction, 3–5 consensus rounds, 2–3x latency, 85% regeneration, zero downtime, immortality, and AGI emergence remain hypotheses/speculation rather than established results.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark, mathematical specification, hypotheses and falsification criteria.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Switch Transformers (Fedus, Zoph & Shazeer).

Hypernetworks: Ha, Dai & Le (2016).

Learned communication: Foerster et al. (2016).

Global latent workspace: VanRullen & Kanai (2021).

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Mallya & Lazebnik (2018).

Byzantine-robust learning: Yin et al. (2018) and subsequent non-IID/attack evaluations.

Consensus: Olfati-Saber, Fax & Murray (2007).

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Lee et al. (2018).

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017).

## Next milestone

**Milestone 05:** adversarial/Byzantine threat-model and reliability analysis, including explicit fault taxonomy, attack surfaces, trust-update dynamics, recovery safety, and worst-case degradation bounds. Implementation remains downstream of this analysis.
