# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 07 — Latent Communication and Modular Representation Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 07 findings

- A shared latent interface is plausible, but equal dimensionality does not imply semantic alignment, identifiability, invertibility, or task sufficiency.
- Organ-specific hidden states should be mapped through explicitly typed learned interfaces before cross-organ communication.
- Coordinate-level latent agreement is not equivalent to semantic correctness because latent representations can be transformed without changing the represented function.
- Contrastive alignment is supported by substantial theory and empirical work, but depends on positive-pair quality, negative sampling, augmentations, and optimization regime.
- A shared/private representation decomposition is preferable as a hypothesis to forcing all modality information into one universal latent.
- Communication quality must be evaluated against bitrate, task performance, effective rank, transfer, calibration, robustness, and latency—not latent similarity alone.
- Routing relevance and organ health should be represented as separate quantities; health-aware routing is an engineering hypothesis, not a theorem.
- Global Workspace Theory motivates selective broadcasting as an architectural analogy, but a shared latent workspace does not establish machine consciousness.
- Representation collapse is a direct failure mode of naive agreement objectives; anti-collapse and task-preservation tests are mandatory.
- The next experiments compare no communication, raw concatenation, contrastive projection, and shared/private bridge designs under fixed specialists.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark, mathematical specification, hypotheses and falsification criteria.
- `research/04_milestone_05_byzantine_reliability_audit.md` — Byzantine threat model, reliability, recovery verification, adversarial robustness, and trust-dynamics audit.
- `research/05_milestone_06_threat_experiment_information_budget.md` — threat-to-experiment matrix, regeneration information budget, recovery gates, and minimum evidence requirements.
- `research/06_milestone_07_latent_communication_modular_representation_audit.md` — latent interfaces, contrastive alignment, shared/private representations, routing, interference, and missing-modality experiments.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. For the latent layer, do not implement a universal latent language before the controlled bridge comparison in Milestone 07.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Switch Transformers (Fedus, Zoph & Shazeer). https://www.jmlr.org/papers/v23/21-0998.html

Hypernetworks: Ha, Dai & Le (2016). https://arxiv.org/abs/1609.09106

Learned communication: Foerster et al. (2016).

Global workspace: VanRullen & Kanai and related Global Workspace literature; cognitive robotics review: https://link.springer.com/article/10.1007/s43154-021-00044-7

Contrastive representation learning: Chen et al. (2020) https://proceedings.mlr.press/v119/chen20j.html; Parulekar et al. (2023) https://proceedings.mlr.press/v195/parulekar23a.html; Zimmermann et al. (2021) https://proceedings.mlr.press/v139/zimmermann21a.html

Cross-modal alignment: Radford et al. (2021), CLIP, https://arxiv.org/abs/2103.00020; Li & Tang (2025), https://arxiv.org/abs/2411.17040; Zhao et al. (2024), https://doi.org/10.1145/3649447

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Mallya & Lazebnik (2018); recent continual-learning surveys.

Byzantine-robust learning: Yin et al. (2018); Xie et al. (2020); Bao et al. (2024); Allouah et al. (2023); Qian et al. (2024).

Consensus: Olfati-Saber, Fax & Murray (2007); switching-topology consensus literature.

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Lee et al. (2018); Yang et al. (2021); Tu et al. (2024).

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017); privacy/security surveys.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, verification, and learned inter-organ communication loops**. Its individual building blocks have substantial prior literature. The composition remains an empirical research question.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, and spontaneous AGI-level evolution remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 08:** formal audit of memory, continual learning, and long-term knowledge persistence, including memory consistency, interference, retrieval quality, consolidation, forgetting bounds, provenance, stale knowledge, and experiments for safe incremental updating.
