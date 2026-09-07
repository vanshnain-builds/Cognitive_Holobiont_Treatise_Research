# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 06 — Threat-to-Experiment Matrix and Regeneration Information-Budget Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 06 findings

- The central scientific question is now the relationship between surviving information, specialist reconstruction, and trustworthy fault detection.
- Parameter similarity is not an adequate definition of regeneration; recovery is evaluated behaviorally on held-out data.
- A regeneration information-budget experiment varies which artifacts survive: architecture, preprocessing, objective, examples/statistics, adapters, optimizer state, calibration state, routing state, checkpoints, and provenance.
- Crash, omission, staleness, random corruption, Byzantine behavior, collusion, honest non-IID disagreement, adaptive attacks, input attacks, and detector compromise are separated threat classes.
- Honest specialization can resemble malicious disagreement; detection must explicitly control false quarantine.
- Consensus agreement is separated from correctness. Spectral-gap bounds apply to specified consensus dynamics only.
- Secure aggregation is separated from differential privacy and from Byzantine robustness.
- Calibration, OOD detection, and uncertainty remain distinct evaluation dimensions.
- Regenerated specialists require structural, functional, behavioral, safety, calibration, and provenance gates.
- Anti-fragility is defined as improvement on held-out related stress, not merely recovery from the observed failure.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark, mathematical specification, hypotheses and falsification criteria.
- `research/04_milestone_05_byzantine_reliability_audit.md` — Byzantine threat model, reliability, recovery verification, adversarial robustness, and trust-dynamics audit.
- `research/05_milestone_06_threat_experiment_information_budget.md` — threat-to-experiment matrix, regeneration information budget, recovery gates, and minimum evidence requirements.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Switch Transformers (Fedus, Zoph & Shazeer). https://www.jmlr.org/papers/v23/21-0998.html

Hypernetworks: Ha, Dai & Le (2016). https://arxiv.org/abs/1609.09106

Learned communication: Foerster et al. (2016).

Global latent workspace: VanRullen & Kanai (2021).

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Mallya & Lazebnik (2018); recent continual-learning surveys.

Byzantine-robust learning: Yin et al. (2018); Xie et al. (2020); Bao et al. (2024); Allouah et al. (2023); Qian et al. (2024). https://proceedings.mlr.press/v80/yin18a.html https://proceedings.mlr.press/v115/xie20a.html https://proceedings.mlr.press/v238/bao24a.html https://proceedings.mlr.press/v206/allouah23a.html https://proceedings.mlr.press/v235/qian24b.html

Consensus: Olfati-Saber, Fax & Murray (2007); switching-topology consensus literature.

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Lee et al. (2018); Yang et al. (2021); Tu et al. (2024). https://arxiv.org/abs/2110.11334 https://proceedings.mlr.press/v235/tu24a.html

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017). https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/

Fault tolerance/self-healing: distributed-systems self-healing literature, including the fault-correction versus fault-tolerance tradeoff.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, and verification loops**. Its individual building blocks have substantial prior literature. The composition remains an empirical research question.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, and spontaneous AGI-level evolution remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 07:** formalize the latent communication and modular representation layer, including interface identifiability, dimensionality, information bottlenecks, routing objectives, multimodal fusion, representation collapse/interference, and a preregistered bridge-comparison experiment.
