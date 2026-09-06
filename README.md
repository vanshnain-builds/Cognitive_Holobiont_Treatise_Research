# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 05 — Byzantine, Adversarial, Reliability, and Recovery Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 05 findings

- Byzantine robustness of optimization is separated from Byzantine detection, attribution, inference integrity, and regeneration.
- Honest non-IID specialization can naturally produce disagreement, so disagreement alone cannot be treated as proof of a malicious organ.
- Adaptive, norm-matched, intermittent, and colluding attacks must be included; obvious outliers are insufficient.
- Trust is formalized as a calibrated risk estimate over an observable history, with explicit quarantine/reintegration policy and hysteresis.
- Recovery is treated as an untrusted computation until independently verified.
- Checkpoint restoration, model reconstruction, and behavioral regeneration are distinct claims.
- Hypernetwork regeneration is a plausible synthesis only for a restricted specialist family and sufficient surviving information; universal regeneration is unsupported.
- Consensus safety/liveness and neural coordination are separated. Spectral-gap convergence bounds apply only to specified consensus dynamics and do not prove cognitive speed.
- Self-healing is measured through availability, downtime, and recovery-time distributions rather than a zero-downtime assumption.
- Adversarial input robustness is a separate axis from software/communication fault tolerance.
- Calibration, OOD detection, and uncertainty are distinct properties and should not be collapsed into an unvalidated scalar trust score.
- Anti-fragility requires improvement on held-out related stress after adaptation, not merely recovery or memorization of the training attack.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark, mathematical specification, hypotheses and falsification criteria.
- `research/04_milestone_05_byzantine_reliability_audit.md` — Byzantine threat model, reliability, recovery verification, adversarial robustness, and trust-dynamics audit.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Switch Transformers (Fedus, Zoph & Shazeer).

Hypernetworks: Ha, Dai & Le (2016); Chang, Flokas & Lipson (2023).

Learned communication: Foerster et al. (2016).

Global latent workspace: VanRullen & Kanai (2021).

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Mallya & Lazebnik (2018); recent continual-learning surveys.

Byzantine-robust learning: Yin et al. (2018) and subsequent non-IID/adaptive-attack evaluations.

Consensus: Olfati-Saber, Fax & Murray (2007); switching-topology consensus literature.

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Lee et al. (2018); recent uncertainty/OOD surveys.

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017).

Fault tolerance/self-healing: recent systematic reviews of distributed/cloud fault tolerance and self-healing systems.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, and verification loops**. Its individual building blocks have substantial prior literature. The composition remains an empirical research question.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, and spontaneous AGI-level evolution remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 06:** turn the reliability findings into a complete threat-to-experiment matrix and information-budget analysis for regeneration, including formal attack surfaces, artifact sufficiency, recovery verification, worst-case degradation metrics, and the minimum evidence required before a prototype can be considered scientifically informative.
