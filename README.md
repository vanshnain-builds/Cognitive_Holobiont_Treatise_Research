# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 20 — Self-Evolving Cognition, Meta-Learning, and Controlled Cognitive Change Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 20 findings

- Self-evolution is not a single capability: parameter, memory, routing, graph, code, and objective changes must be evaluated separately.
- Continual learning and modular adaptation do not by themselves establish self-directed evolution or increasing intelligence.
- Expert growth has a routing-selection cost; more modular capacity is not free.
- Self-evolving agent research demonstrates bounded adaptation of memory, tools, workflows, code, and goals, but not reliable open-ended self-improvement.
- Objective modification is a distinct security/alignment surface from parameter adaptation.
- Protected external objectives and independent verification are plausible engineering controls, not universal guarantees against specification gaming.
- Two-timescale routing/specialist adaptation requires explicit stability analysis; isolated component stability does not imply coupled stability.
- Multimodal memory must be evaluated as a lifecycle—writing, maintenance, retrieval, and use—not as a single storage-quality score.
- Persistent memory is a demonstrated attack surface, including delayed and compositional poisoning; evolving memory cannot automatically be treated as trusted state.
- Behavioral change and lineage must be measured across versions; version count is not independent evidence when branches share data, code, memory, routers, or verifiers.
- Bounded cognitive-change budgets are a testable engineering control, not a theorem of safe self-modification.

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
- `research/12_milestone_13_information_flow_optimization_bounds_audit.md` — coupled information-flow model, rate–distortion and finite-bit constraints, latent identifiability, workspace capacity, routing stability, memory sufficiency, gradient conflict, Pareto objectives, hypotheses H42–H48, and preregistration requirements.
- `research/13_milestone_14_reliability_decision_layer_audit.md` — calibration, selective prediction, OOD detection, uncertainty aggregation, correlated/common-mode evidence, conformal risk control, decision-aware deferral/recovery, hypotheses H49–H55, and the B4 decision-layer gate.
- `research/14_milestone_15_adversarial_reliability_layer_audit.md` — adaptive attacks against confidence/OOD/disagreement signals, conformal and calibration poisoning, persistent-memory poisoning, router manipulation, common-mode attacks, robust decision objectives, hypotheses H56–H63, and adversarial B4 experiments.
- `research/15_milestone_16_recovery_verification_adaptive_compromise_audit.md` — recovery hierarchy, behavioral verification, RPO/RTO, verifier independence, common-mode recovery failure, Byzantine/self-stabilization assumptions, regeneration limits, attack surfaces, hypotheses H64–H71, and recovery experiments.
- `research/16_milestone_17_byzantine_graph_consensus_and_failure_domain_audit.md` — dynamic graph formalization, Byzantine learning vs agreement, heterogeneous-honest baselines, routing attacks, graph bottlenecks, failure-domain diversity, privacy/robustness separation, hypotheses H72–H79, and distributed-implementation decision gates.
- `research/17_milestone_18_adaptive_topology_stability_and_game_theoretic_routing_audit.md` — coupled routing/specialist dynamics, local stability conditions, router-induced distribution shift, strategic topology attacks, multimodal-honest Byzantine filtering, privacy/robustness interaction, dependency-aware evidence, hypotheses H80–H87, and the adaptive-routing decision gate.
- `research/18_milestone_19_adaptive_adversary_equilibrium_and_mechanism_design_audit.md` — adaptive adversary hierarchy, stochastic-game formulation, minimax/equilibrium assumptions, trust-routing feedback, mechanism/resource allocation, dependency-adjusted evidence, behavioral recovery, regeneration limits, hypotheses H88–H96, and the strategic-routing decision gate.
- `research/19_milestone_20_self_evolving_meta_learning_and_cognitive_change_audit.md` — decomposition of cognitive change, modular continual learning, routing cost, self-evolving agents, objective drift, two-timescale adaptation, coupled co-evolution, memory lifecycle, persistent-memory poisoning, lineage, change budgets, hypotheses H97–H105, and the self-evolution decision gate.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before serious prototyping, fix task/data splits, specialist roles and model versions, bridge/routing definitions, objectives, fault and Byzantine threat models, regeneration artifacts, memory semantics, workspace capacity, communication accounting, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. Causal intervention controls are required before interpreting attention or routing weights. Milestone 12 requires explicit loss terms, gradient diagnostics, capacity matching, and degenerate-optimum checks. Milestone 13 requires an explicit channel/coding model for information capacity or rate–distortion quantities, task-risk definitions for communicated representations, and sensitivity analysis for neural mutual-information estimators. Milestone 14 requires separate calibration/OOD/selective-risk evaluation, common-mode-fault tests, explicit decision costs, and exact conformal assumptions. Milestone 15 requires adaptive attacker models, attack-budget sweeps, calibration-poisoning tests, detector-evasion tests, router manipulation, memory-poisoning persistence, common-mode attacks, and matched clean-utility/resource comparisons. Milestone 16 additionally requires behavioral recovery tests, trusted-state lineage, verifier failure-domain analysis, checkpoint/replica integrity tests, RPO/RTO accounting, recovery-loop poisoning tests, common-mode verifier attacks, and statistical confidence on recovery error. Milestone 17 additionally requires dynamic-topology controls, explicit graph/consensus assumptions, heterogeneous-honest baselines, routing attack tests, failure-domain dependency analysis, topology/communication accounting, privacy-vs-robustness separation, and fixed-topology controls before adaptive-graph claims. Milestone 18 additionally requires isolated-component stability controls, router-learning-rate and top-k/soft-routing sweeps, starvation/oscillation measurements, direct and indirect routing attacks, multimodal-honest Byzantine baselines, privacy-noise sweeps, dependency-aware evidence metrics, and one-variable-at-a-time release of adaptive topology. Milestone 19 additionally requires adaptive-adversary threat classes, stochastic-game assumptions, trust-feedback hysteresis measurements, dependency-adjusted evidence, strategic topology attacks, behavioral recovery verification, hypernetwork conditioning/task-coverage tests, and adaptive-vs-oblivious attack comparisons. Milestone 20 additionally requires separate adaptation/evolution definitions, protected external objectives, routing-cost accounting, two-timescale stability sweeps, lifecycle memory evaluation, objective-drift attacks, lineage/dependency analysis, behavioral-change budgets, and bounded self-modification sandboxes.

## Key literature anchors

Sparse MoE: Shazeer et al. (2017); Fedus, Zoph & Shazeer, Switch Transformers. https://jmlr.org/papers/v23/21-0998.html

Expert Choice Routing: Zhou et al. (2022). https://arxiv.org/abs/2202.09368

Hypernetworks: Ha, Dai & Le (2016). https://arxiv.org/abs/1609.09106

Learned communication: Foerster et al. (2016). https://arxiv.org/abs/1605.06676

Global workspace: Devillers, Maytie & VanRullen. https://arxiv.org/abs/2306.15711; Bao et al. https://arxiv.org/abs/2001.09485

Contrastive representation learning: Chen et al. (2020). https://proceedings.mlr.press/v119/chen20j.html; Parulekar et al. (2023). https://proceedings.mlr.press/v195/parulekar23a.html; Zimmermann et al. (2021). https://proceedings.mlr.press/v139/zimmermann21a.html

Cross-modal alignment/fusion: Zhao, Zhang & Geng (2024). https://doi.org/10.1145/3649447; Radford et al. (2021), CLIP. https://arxiv.org/abs/2103.00020

Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Knoblauch et al. (2020).

Memory systems: Lewis et al. (2020), RAG; Packer et al. (2023), MemGPT; Park et al. (2023), Generative Agents; Wang et al. (2023), LongMem.

Byzantine-robust learning: Yin et al. (2018); Xie et al. (2020); Liu et al. (2023). https://arxiv.org/abs/2302.06079; Farhadkhani et al. (2024). https://arxiv.org/abs/2405.00491; Parsa et al. (2025). https://arxiv.org/abs/2511.03529; FedCLEAN (2025). https://arxiv.org/abs/2501.12123; FLTG (2025). https://arxiv.org/abs/2505.12851; OptiGradTrust (2025). https://arxiv.org/abs/2507.23638

Federated heterogeneity/privacy: Yi et al. (2024), FedP3. https://arxiv.org/abs/2404.09816

Privacy-preserving Byzantine robustness: ByzSFL (2025). https://arxiv.org/abs/2501.06953; Practical privacy-preserving Byzantine-robust FL (2025). https://arxiv.org/abs/2512.17254

Calibration/OOD/selective prediction: Guo et al. (2017). https://proceedings.mlr.press/v70/guo17a.html; Hendrycks & Gimpel (2017). https://arxiv.org/abs/1610.02136; Geifman & El-Yaniv (2019). https://proceedings.mlr.press/v97/geifman19a.html

Robust conformal prediction: Zargarbashi et al. (2024). https://proceedings.mlr.press/v235/h-zargarbashi24a.html; certifiably Byzantine-robust federated conformal prediction. https://arxiv.org/abs/2406.01960; adversarial calibration robustness (2025). https://arxiv.org/abs/2511.18562

Self-stabilization: Faghih et al. (2015). https://arxiv.org/abs/1509.05664; Blin, Petit & Tixeuil (2025). https://arxiv.org/abs/2505.06596

Dynamic Byzantine/distributed evidence: GRANITE (2025). https://arxiv.org/abs/2504.17471; Fault-Tolerant Federated Reinforcement Learning. https://arxiv.org/abs/2110.14074; Chen et al. (2026), Byzantine agreement under reorder/channel attacks. https://arxiv.org/abs/2609.09623

Graph information flow: Over-Squashing survey (2023). https://arxiv.org/abs/2308.15568; Epping et al. (2024), Graph Neural Networks Do Not Always Oversmooth. https://arxiv.org/abs/2406.02269

## Status

The project remains **falsification-first and pre-implementation**. Milestone 20 does not validate self-evolving cognition, strategic equilibrium, privacy-preserving Byzantine resilience, autonomous regeneration, self-organizing cognition, consciousness, or AGI-level claims; it separates adaptation from evolution and narrows self-modification into protected objectives, measurable behavioral change, resource accounting, lineage-aware evidence, explicit threat models, and falsifiable experiments.
