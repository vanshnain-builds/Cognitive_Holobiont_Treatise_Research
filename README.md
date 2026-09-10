# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 09 — Multimodal Fusion, Global Workspace, Working Memory, and Cross-Organ Routing Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 09 findings

- Specialized modality encoders, cross-attention, shared latent alignment in specific tasks, missing-modality evaluation, and dynamic gating are established engineering techniques.
- A Global Workspace-inspired architecture has direct computational precedent: frozen modality-specific systems can encode/decode through a shared workspace and use cycle consistency for cross-modal alignment in limited-data settings.
- A shared workspace is therefore a plausible engineering synthesis for selective inter-organ communication, but its necessity for general intelligence is not established.
- Cross-attention is a strong baseline for the same communication problem; a separate workspace must beat it under matched parameter, compute and communication budgets.
- A shared latent is not automatically a universal semantic language. Cycle-consistency or low latent distance alone does not prove semantic equivalence.
- Shared/private latent representations should be compared with fully shared representations at equal capacity because modality-specific information can be discarded by forced sharing.
- Attention weights are not sufficient causal attribution; routing importance requires interventions and randomized controls.
- Missing-modality recovery is conditional. Performance must be measured for random, temporal, systematic, corrupted, contradictory and shifted missingness.
- Spectral gap bounds convergence for specified consensus dynamics but is not a universal measure of cognitive speed.
- Communication has a measurable information/latency budget. Payload, transport, synchronization and accelerator overhead must all be counted.
- Claims of fixed compute/latency improvements require workload- and hardware-specific evidence.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark, mathematical specification, hypotheses and falsification criteria.
- `research/04_milestone_05_byzantine_reliability_audit.md` — Byzantine threat model, reliability, recovery verification, adversarial robustness, and trust-dynamics audit.
- `research/05_milestone_06_threat_experiment_information_budget.md` — threat-to-experiment matrix, regeneration information budget, recovery gates, and minimum evidence requirements.
- `research/06_milestone_07_latent_communication_modular_representation_audit.md` — latent interfaces, contrastive alignment, shared/private representations, routing, interference, and missing-modality experiments.
- `research/07_milestone_08_memory_continual_learning_persistence_audit.md` — memory stores, continual learning, retrieval, consolidation, forgetting, provenance, stale/poisoned memory, rollback, information limits, and safe incremental updates.
- `research/08_milestone_09_multimodal_global_workspace_routing_audit.md` — multimodal fusion, workspace formalization, broadcast/working-memory distinctions, missing modalities, reliability-aware routing, causal attribution, communication budgets, and controlled experiments.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, memory semantics, workspace size and token capacity, communication accounting, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. For multimodal routing, causal intervention controls must be defined before interpreting attention or routing weights.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Switch Transformers (Fedus, Zoph & Shazeer). https://www.jmlr.org/papers/v23/21-0998.html

Hypernetworks: Ha, Dai & Le (2016). https://arxiv.org/abs/1609.09106

Learned communication: Foerster et al. (2016).

Global workspace: Devillers, Maytie & VanRullen, https://arxiv.org/abs/2306.15711; Bao et al., https://arxiv.org/abs/2001.09485; cognitive workspace review literature.

Contrastive representation learning: Chen et al. (2020) https://proceedings.mlr.press/v119/chen20j.html; Parulekar et al. (2023) https://proceedings.mlr.press/v195/parulekar23a.html; Zimmermann et al. (2021) https://proceedings.mlr.press/v139/zimmermann21a.html

Cross-modal alignment/fusion: Zhao, Zhang & Geng, Deep Multimodal Data Fusion (2024), https://doi.org/10.1145/3649447; Radford et al. (2021), CLIP, https://arxiv.org/abs/2103.00020

Missing modalities: Wu et al. (2024), https://arxiv.org/abs/2409.07825; Lee et al., differentiable multimodal filters, https://arxiv.org/abs/2010.13021

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Mallya & Lazebnik (2018); Farajtabar et al. (2020); Knoblauch et al. (2020); recent continual-learning theory and surveys.

Memory systems: Lewis et al. (2020) RAG; Packer et al. (2023) MemGPT; Park et al. (2023) Generative Agents; Wang et al. (2023) LongMem; recent agent-memory and memory-security research.

Byzantine-robust learning: Yin et al. (2018); Xie et al. (2020); Bao et al. (2024); Allouah et al. (2023); Qian et al. (2024).

Consensus: Olfati-Saber, Fax & Murray (2007); switching-topology consensus literature.

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Lee et al. (2018); Yang et al. (2021); Tu et al. (2024).

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017); privacy/security surveys.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, verification, learned inter-organ communication loops, a bounded shared workspace, and stateful memory**. Its individual building blocks have substantial prior literature. The composition remains an empirical research question.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, spontaneous AGI-level evolution, indefinite exact memory, and automatic correctness of retrieved memories remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 10:** formal audit of memory/workspace interaction, long-horizon state consistency, cross-organ learning dynamics, information persistence, and end-to-end reliability. The focus will be whether shared memory and workspace create measurable positive transfer without increasing interference, stale-state propagation, or failure correlation.
