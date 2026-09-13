# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 12 — Learning Objectives, Optimization Dynamics, and Capacity-Matched Baselines**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 12 findings

- The remaining pre-implementation risk is objective misspecification: a mathematically valid loss can optimize a property that does not correspond to system-level utility.
- B0–B3 are now formalized as a constrained multi-objective stack over utility, compute, communication, risk, representation quality, and memory behavior.
- Learned latent alignment must include task-preservation/anti-collapse controls; alignment alone can admit degenerate representations.
- A bounded workspace is a finite latent bottleneck, not evidence of a necessary cognitive substrate.
- Router entropy is not a health metric; low entropy may mean useful specialization or collapse, while high entropy may mean uncertainty or waste.
- Load balancing must be treated as a capacity/utility problem rather than universal uniform expert usage.
- Memory write quality, provenance, authorization, staleness, and retrieval utility are separate dimensions; retrieval similarity is not truth.
- Continual learning must measure forgetting and transfer separately from parameter stability.
- Capacity-matched controls must account for active FLOPs, parameters, memory, communication, wall-clock latency, and training budget.
- No complete Holobiont implementation should begin until B0–B3 configurations and primary metrics are fixed; recovery, Byzantine behavior, and self-modification remain downstream experiments.

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
- `research/09_milestone_10_end_to_end_memory_workspace_reliability_audit.md` — end-to-end state model, memory/workspace separation, persistent-memory security, router stability, information limits, recovery hierarchy, correlated failures, reliability-aware routing, and hypotheses H22–H27.
- `research/10_milestone_11_formal_end_to_end_prototype_specification.md` — concrete B0–B7 prototype specification, bridge/workspace/router/memory objectives, recovery hierarchy, failure taxonomy, complexity accounting, falsification criteria, hypotheses H28–H34, and implementation gate.
- `research/11_milestone_12_learning_objectives_optimization_audit.md` — formal B0–B3 learning objectives, bridge/workspace/router/memory optimization, anti-collapse and gradient-conflict diagnostics, capacity matching, information-budget constraints, hypotheses H35–H41, and the pre-implementation decision gate.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, memory semantics, workspace size and token capacity, communication accounting, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. For multimodal routing, causal intervention controls must be defined before interpreting attention or routing weights. Milestone 12 additionally requires explicit loss terms, gradient diagnostics, capacity matching, and checks for degenerate optima before coding.

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

Recent Milestone 11 sources: Zhou et al. (2022), Expert Choice Routing, https://arxiv.org/abs/2202.09368; Karimireddy, He & Jaggi (2020), heterogeneous Byzantine robustness, https://arxiv.org/abs/2006.09365; Yin et al. (2018), https://arxiv.org/abs/1803.01498; Xu, Guo & Wei (2025), selective conformal risk control, https://arxiv.org/abs/2512.12844; Bao et al. (2024), online selective conformal prediction, https://arxiv.org/abs/2403.07728; Tavakoli et al. (2025), BEAM long-term memory benchmark, https://arxiv.org/abs/2510.27246; Wei et al. (2025), Evo-Memory, https://arxiv.org/abs/2511.20857; Li et al. (2026), LycheeMemory V2, https://arxiv.org/abs/2608.12990; Zhao et al. (2026), structured long-term agent memory, https://arxiv.org/abs/2607.16211; Gandhi & Kozyrakis (2026), sparse MoE checkpointing, https://www.usenix.org/conference/nsdi26/presentation/gandhi; Deng et al. (2025), distributed fault detection, https://www.usenix.org/conference/nsdi25/presentation/deng; Chen et al. (2026), role-based RL fault tolerance, https://www.usenix.org/conference/osdi26/presentation/chen-zhenqian; Foerster et al. (2016), https://arxiv.org/abs/1605.06676.

Recent Milestone 12 sources: Fedus, Zoph & Shazeer (2022), Switch Transformers, https://jmlr.org/papers/v23/21-0998.html; Zhou et al. (2022), Expert Choice Routing, https://arxiv.org/abs/2202.09368; Zoph et al. (2022), ST-MoE, https://arxiv.org/abs/2202.08906; Jaegle et al. (2021), Perceiver, https://arxiv.org/abs/2103.03206; Alemi et al. (2016), Deep Variational Information Bottleneck, https://arxiv.org/abs/1612.00410; Liu et al. (2021), Conflict-Averse Gradient Descent, https://arxiv.org/abs/2110.14048; Hwang et al. (2024), source-reliability-aware RAG, https://arxiv.org/abs/2410.22954; Zou et al. (2026), environment-injected memory poisoning, https://arxiv.org/abs/2604.02623; Gao et al. (2026), MemPoison, https://arxiv.org/abs/2607.14651.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, verification, learned inter-organ communication loops, a bounded shared workspace, and stateful memory**. Its individual building blocks have substantial prior literature. The composition remains an empirical research question.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, spontaneous AGI-level evolution, indefinite exact memory, and automatic correctness of retrieved memories remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 13:** formalize the bridge/workspace/memory information-flow and optimization coupling as a single constrained system; derive identifiability, rate-distortion/information-budget implications, routing stability conditions, and measurable lower/upper bounds for the B0–B3 prototype. The objective is to determine which quantities can be theoretically bounded before empirical implementation and to finalize a minimal preregistration-ready configuration without introducing downstream fault/recovery confounders.
