# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 23 — Evaluation Validity, Causal Attribution, and Evidence Independence Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 23 findings

- Observed benchmark improvement must be separated from causal mechanism improvement and generalized capability.
- MoE routing balance is an optimization problem; balanced utilization is not itself evidence of better task quality.
- Latent-memory gains require compute/context/retrieval-matched controls before being interpreted as mechanism-specific gains.
- Continual-learning evaluation must separate acquisition, retention, forward transfer, backward transfer and resource cost.
- Shared lineage, encoders, checkpoints and memory can make nominal specialists statistically dependent; replica count is not evidence count.
- Causal claims require interventions and assumptions; correlation among routing, memory and performance is insufficient.
- Latent-space alignment/reconstruction is not semantic equivalence without stronger identifiability or intervention evidence.
- Adaptive self-evolution can overfit visible evaluators; hidden post-freeze evaluation and evaluator provenance are required.
- Non-stationary environments require tracking metrics such as dynamic regret when an online-learning formulation applies, rather than fixed-point convergence alone.
- Scaffold changes are themselves causal interventions and must be controlled when comparing agent systems.
- Evaluation validity is now treated as an architectural dependency, not a post-hoc reporting step.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — claim/equation/evidence audit.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — consensus, stability, regeneration and self-modification audit.
- `research/03_milestone_04_chapter_13_preregistered_benchmark.md` — pre-implementation benchmark and falsification criteria.
- `research/04_milestone_05_byzantine_reliability_audit.md` — Byzantine reliability and recovery audit.
- `research/05_milestone_06_threat_experiment_information_budget.md` — threat/experiment matrix and regeneration information budget.
- `research/06_milestone_07_latent_communication_modular_representation_audit.md` — latent interfaces and modular representation audit.
- `research/07_milestone_08_memory_continual_learning_persistence_audit.md` — memory, continual learning and persistence audit.
- `research/08_milestone_09_multimodal_global_workspace_routing_audit.md` — multimodal fusion, workspace and routing audit.
- `research/09_milestone_10_end_to_end_memory_workspace_reliability_audit.md` — end-to-end memory/workspace/reliability audit.
- `research/10_milestone_11_formal_end_to_end_prototype_specification.md` — B0–B7 prototype specification.
- `research/11_milestone_12_learning_objectives_optimization_audit.md` — learning-objective and optimization audit.
- `research/12_milestone_13_information_flow_optimization_bounds_audit.md` — information-flow and rate–distortion audit.
- `research/13_milestone_14_reliability_decision_layer_audit.md` — calibration, OOD and selective-risk audit.
- `research/14_milestone_15_adversarial_reliability_layer_audit.md` — adversarial reliability-layer audit.
- `research/15_milestone_16_recovery_verification_adaptive_compromise_audit.md` — recovery and verification audit.
- `research/16_milestone_17_byzantine_graph_consensus_and_failure_domain_audit.md` — graph/consensus/failure-domain audit.
- `research/17_milestone_18_adaptive_topology_stability_and_game_theoretic_routing_audit.md` — adaptive topology and routing stability audit.
- `research/18_milestone_19_adaptive_adversary_equilibrium_and_mechanism_design_audit.md` — adaptive adversary and equilibrium audit.
- `research/19_milestone_20_self_evolving_meta_learning_and_cognitive_change_audit.md` — self-evolution and controlled cognitive-change audit.
- `research/20_milestone_21_controlled_self_evolution_memory_governance_and_adaptive_topology_audit.md` — controlled self-evolution, memory lifecycle governance, latent-memory MoE, adaptive topology, dynamic Byzantine networks, objective protection, and hypotheses H106–H115.
- `research/21_milestone_22_adaptive_memory_skill_security_and_nonstationary_robustness_audit.md` — experience-to-skill attacks, transfer validation, active-subgraph Byzantine risk, dynamic regret, privacy/robustness coupling, workspace evidence, experiments E11.1–E11.8, and hypotheses H116–H124.
- `research/22_milestone_23_evaluation_causal_attribution_and_evidence_independence_audit.md` — causal attribution, evaluator leakage, dependency-aware evidence, resource-matched ablations, latent identifiability, non-stationary evaluation, experiments E12.1–E12.8, and hypotheses H125–H134.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims. Milestone 23 adds causal intervention, evaluator separation, dependency/lineage analysis and resource matching as explicit evidence gates.

## Evidence principles

Biological analogy is not proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont. Nominal specialist count, latent similarity, benchmark score, or consensus are not treated as independent evidence without a justified model.

## Implementation gate

Implementation follows specification. Before serious prototyping, fix task/data splits, specialist roles and model versions, bridge/routing definitions, objectives, fault and Byzantine threat models, regeneration artifacts, memory semantics, workspace capacity, communication accounting, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. Later milestones additionally require explicit optimization terms, information/channel assumptions, reliability/OOD tests, adversarial attacks, behavioral recovery verification, graph/topology controls, adaptive-adversary models, protected objectives, lineage/dependency analysis, bounded self-modification, memory/skill provenance, active-subgraph safety, non-stationary tracking, causal ablations, hidden evaluators, and resource-matched baselines.

## Key literature anchors

- Sparse MoE: Shazeer et al. (2017). https://arxiv.org/abs/1701.06538
- Switch Transformers: Fedus, Zoph & Shazeer (2022). https://jmlr.org/papers/v23/21-0998.html
- Expert Choice Routing: Zhou et al. (2022). https://arxiv.org/abs/2202.09368
- HyperNetworks: Ha, Dai & Le (2016). https://arxiv.org/abs/1609.09106
- Contrastive representation learning: Chen et al. (2020). https://proceedings.mlr.press/v119/chen20j.html
- Continual learning: Kirkpatrick et al. (2017); Li & Hoiem (2016); Knoblauch et al. (2020).
- Dynamic Mixture of Latent Memories for Self-Evolving Agents (2026). https://arxiv.org/abs/2605.21951
- LatentMem: Customizing Latent Memory for Multi-Agent Systems (2026). https://arxiv.org/abs/2602.03036
- Memento-Skills: Let Agents Design Agents (2026). https://arxiv.org/abs/2603.18743
- MemEvolve: Meta-Evolution of Agent Memory Systems (2025/2026). https://arxiv.org/abs/2512.18746
- SkillJack: Persistent Skill Backdoors in Self-Evolving Agents (2026). https://arxiv.org/abs/2608.03509
- OEP: Poisoning Self-Evolving LLM Agents (2026). https://arxiv.org/abs/2605.18930
- Dynamic Regret for Byzantine-Robust Online Federated Learning (2026). https://doi.org/10.1109/tsp.2026.3673260
- BPFLH: Byzantine-Robust Privacy-Preserving FL for Heterogeneous Data (2026). https://doi.org/10.1109/TDSC.2026.3661522
- $\phi$-Balancing for Mixture-of-Experts Training (2026). https://arxiv.org/abs/2605.15403
- Three Phases of Expert Routing (2026). https://arxiv.org/abs/2604.04230
- Statistical and structural identifiability in representation learning (ICLR 2026). https://proceedings.iclr.cc/paper_files/paper/2026/hash/f67e5f99b23b108a3a6665f410034bcd-Abstract-Conference.html
- Multi-View Causal Representation Learning with Partial Observability (2024). https://arxiv.org/abs/2311.04056
- CurLL continual-learning benchmark (ACL/BabyLM 2025). https://aclanthology.org/2025.babylm-main.20/
- CLeaRS continual vision-language learning benchmark (2026). https://arxiv.org/abs/2604.00820
- Causal inference over time (AAAI 2025). https://ojs.aaai.org/index.php/AAAI/article/view/33626
- HAL: Holistic Agent Leaderboard (ICLR 2026). https://hal.cs.princeton.edu/
- Multimodal Dreaming: A Global Workspace Approach to World Model-Based RL (2025). https://arxiv.org/abs/2502.21142
- Global Workspace selection-broadcast hypothesis (2025). https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2025.1607190/full
- Dynamic Topology Optimization for Non-IID Data in Decentralized Learning (2025). https://bacox.github.io/paper/Topology-Optimization-Decentralized-Learning/
- Byzantine-Resilient Dynamic Peer-to-Peer Networks (2025). https://arxiv.org/abs/2506.04368
- Calibration: Guo et al. (2017). https://proceedings.mlr.press/v70/guo17a.html
- Selective prediction: Geifman & El-Yaniv (2019). https://proceedings.mlr.press/v97/geifman19a.html

## Status

The project remains **falsification-first and pre-implementation**. Milestone 23 does not validate self-evolving cognition, open-ended self-improvement, adaptive equilibrium, privacy-preserving Byzantine resilience, autonomous regeneration, self-organizing cognition, consciousness, or AGI-level claims. The current research question is whether controlled modular change can improve the utility–retention–risk–resource frontier **after** causal intervention, resource matching, hidden evaluation, dependency analysis, latent-identifiability checks, and explicit threat/distribution assumptions.