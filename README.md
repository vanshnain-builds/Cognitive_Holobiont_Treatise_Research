# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 08 — Memory, Continual Learning, and Long-Term Knowledge Persistence Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 08 findings

- Catastrophic forgetting is established; no general method guarantees indefinite retention across arbitrary task sequences.
- Replay, consolidation, gradient constraints, and context-dependent gating are complementary mechanisms with different assumptions and costs.
- A finite external memory can preserve information outside model parameters, but retrieval is not equivalent to truth or internalized knowledge.
- EWC-style penalties constrain parameter movement but require local smoothness/stationarity assumptions to support any loss-change bound; they do not prove indefinite functional preservation.
- Episodic and semantic memory are useful conceptual distinctions, but their combination does not establish human-like continual learning.
- Persistent memory must include provenance, versioning, temporal validity and rollback concepts if the system is expected to operate under stale, contradictory or poisoned writes.
- Replication improves availability but does not by itself establish memory correctness or consistency under correlated corruption.
- Modularity can remove direct parameter interference outside an active specialist set, but shared bridges, routing, memory and decision layers can still create indirect interference.
- Memory compression and indefinite recall require explicit assumptions about the future task family and information sufficiency.
- Retrieval recall, memory integrity, downstream task utility, calibration and freshness must be measured separately.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark, mathematical specification, hypotheses and falsification criteria.
- `research/04_milestone_05_byzantine_reliability_audit.md` — Byzantine threat model, reliability, recovery verification, adversarial robustness, and trust-dynamics audit.
- `research/05_milestone_06_threat_experiment_information_budget.md` — threat-to-experiment matrix, regeneration information budget, recovery gates, and minimum evidence requirements.
- `research/06_milestone_07_latent_communication_modular_representation_audit.md` — latent interfaces, contrastive alignment, shared/private representations, routing, interference, and missing-modality experiments.
- `research/07_milestone_08_memory_continual_learning_persistence_audit.md` — memory stores, continual learning, retrieval, consolidation, forgetting, provenance, stale/poisoned memory, rollback, information limits, and safe incremental updates.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, memory semantics, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. For memory, do not implement a persistent store as if retrieval implies correctness; define provenance, freshness, validation and rollback first.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Switch Transformers (Fedus, Zoph & Shazeer). https://www.jmlr.org/papers/v23/21-0998.html

Hypernetworks: Ha, Dai & Le (2016). https://arxiv.org/abs/1609.09106

Learned communication: Foerster et al. (2016).

Global workspace: VanRullen & Kanai and related Global Workspace literature; cognitive robotics review: https://link.springer.com/article/10.1007/s43154-021-00044-7

Contrastive representation learning: Chen et al. (2020) https://proceedings.mlr.press/v119/chen20j.html; Parulekar et al. (2023) https://proceedings.mlr.press/v195/parulekar23a.html; Zimmermann et al. (2021) https://proceedings.mlr.press/v139/zimmermann21a.html

Cross-modal alignment: Radford et al. (2021), CLIP, https://arxiv.org/abs/2103.00020; Li & Tang (2025), https://arxiv.org/abs/2411.17040; Zhao et al. (2024), https://doi.org/10.1145/3649447

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Mallya & Lazebnik (2018); Farajtabar et al. (2020); Knoblauch et al. (2020); recent continual-learning theory and surveys.

Memory systems: Lewis et al. (2020) RAG; Packer et al. (2023) MemGPT; Park et al. (2023) Generative Agents; Wang et al. (2023) LongMem; recent 2026 surveys on agent memory and memory security.

Byzantine-robust learning: Yin et al. (2018); Xie et al. (2020); Bao et al. (2024); Allouah et al. (2023); Qian et al. (2024).

Consensus: Olfati-Saber, Fax & Murray (2007); switching-topology consensus literature.

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Lee et al. (2018); Yang et al. (2021); Tu et al. (2024).

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017); privacy/security surveys.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, verification, learned inter-organ communication loops, and stateful memory**. Its individual building blocks have substantial prior literature. The composition remains an empirical research question.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, spontaneous AGI-level evolution, indefinite exact memory, and automatic correctness of retrieved memories remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 09:** formal audit of multimodal fusion, working-memory/global-workspace mechanisms, attention/broadcast dynamics, and cross-organ information routing, including causal attribution, bandwidth/latency tradeoffs, missing-modality behavior, and a controlled benchmark for whether a shared workspace adds value beyond ordinary fusion and routing.
