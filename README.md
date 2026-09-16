# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 15 — Adversarial Reliability-Layer Audit**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 15 findings

- The B4 reliability/decision layer is itself an attack surface and must not be treated as a trusted oracle.
- Confidence, uncertainty, OOD scores, disagreement, provenance, drift, and selective coverage can each be manipulated or become misleading under adaptive attacks.
- Standard conformal guarantees require explicit exchangeability/statistical assumptions and do not automatically survive calibration poisoning or adversarial test-time perturbations.
- Robust conformal methods can recover guarantees for narrowly specified threat models, but this is not universal adversarial robustness.
- Persistent memory is a durable attack surface: malicious records can survive across sessions and influence later behavior, including through compositional or trigger-conditioned retrieval.
- Cryptographic provenance establishes origin/integrity metadata, not semantic truth or safety.
- Disagreement remains a weak fault signal when honest specialists are heterogeneous; attackers can target either false disagreement or false consensus.
- Correlated/common-mode corruption can make multiple organs agree on the same wrong state.
- Byzantine/non-IID literature reinforces the need to distinguish heterogeneity from malicious behavior.
- The correct robustness object is an explicitly defined decision policy under an explicitly defined attacker class, not a universal scalar health score.
- Autonomous self-modification/recovery remains gated until adversarial reliability experiments demonstrate measurable safety value at matched clean utility and resource budgets.

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

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Implementation gate

Implementation follows specification. Before the first serious prototype, the benchmark must fix the task/data split, specialist roles and model versions, bridge/routing definitions, objective functions, fault and Byzantine threat models, regeneration artifacts, memory semantics, workspace size and token capacity, communication accounting, primary metrics, statistical replication, acceptance criteria, and reproducibility artifacts. For multimodal routing, causal intervention controls must be defined before interpreting attention or routing weights. Milestone 12 additionally requires explicit loss terms, gradient diagnostics, capacity matching, and checks for degenerate optima before coding. Milestone 13 additionally requires an explicit channel/coding model whenever information capacity or rate–distortion quantities are reported, task-risk definitions for communicated representations, and sensitivity analysis for any neural mutual-information estimator. Milestone 14 additionally requires separate calibration/OOD/selective-risk evaluation, common-mode-fault tests, explicit decision costs, and exact assumptions for any conformal guarantee. Milestone 15 additionally requires adaptive attacker models, attack-budget sweeps, calibration-poisoning tests, detector-evasion tests, router manipulation tests, memory-poisoning persistence tests, common-mode attacks, and matched clean-utility/resource comparisons.

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

Calibration/OOD: Guo et al. (2017); Lakshminarayanan et al. (2017); Hendrycks & Gimpel (2017); Lee et al. (2018); Yang et al. (2021); Tu et al. (2024).

Selective prediction and risk control: Geifman & El-Yaniv (2019), https://proceedings.mlr.press/v97/geifman19a.html; Xu, Guo & Wei (2026), https://arxiv.org/abs/2512.12844; Bai & Jin (2026), https://arxiv.org/abs/2603.24704.

Federated learning/security: McMahan et al. (2017); Bonawitz et al. (2017); privacy/security surveys.

Recent Milestone 11 sources: Zhou et al. (2022), Expert Choice Routing, https://arxiv.org/abs/2202.09368; Karimireddy, He & Jaggi (2020), heterogeneous Byzantine robustness, https://arxiv.org/abs/2006.09365; Yin et al. (2018), https://arxiv.org/abs/1803.01498; Xu, Guo & Wei (2025), selective conformal risk control, https://arxiv.org/abs/2512.12844; Bao et al. (2024), online selective conformal prediction, https://arxiv.org/abs/2403.07728; Tavakoli et al. (2025), BEAM long-term memory benchmark, https://arxiv.org/abs/2510.27246; Wei et al. (2025), Evo-Memory, https://arxiv.org/abs/2511.20857; Li et al. (2026), LycheeMemory V2, https://arxiv.org/abs/2608.12990; Zhao et al. (2026), structured long-term agent memory, https://arxiv.org/abs/2607.16211; Gandhi & Kozyrakis (2026), sparse MoE checkpointing, https://www.usenix.org/conference/nsdi26/presentation/gandhi; Deng et al. (2025), distributed fault detection, https://www.usenix.org/conference/nsdi25/presentation/deng; Chen et al. (2026), role-based RL fault tolerance, https://www.usenix.org/conference/osdi26/presentation/chen-zhenqian; Foerster et al. (2016), https://arxiv.org/abs/1605.06676.

Recent Milestone 12 sources: Fedus, Zoph & Shazeer (2022), Switch Transformers, https://jmlr.org/papers/v23/21-0998.html; Zhou et al. (2022), Expert Choice Routing, https://arxiv.org/abs/2202.09368; Zoph et al. (2022), ST-MoE, https://arxiv.org/abs/2202.08906; Jaegle et al. (2021), Perceiver, https://arxiv.org/abs/2103.03206; Alemi et al. (2016), Deep Variational Information Bottleneck, https://arxiv.org/abs/1612.00410; Liu et al. (2021), Conflict-Averse Gradient Descent, https://arxiv.org/abs/2110.14048; Hwang et al. (2024), source-reliability-aware RAG, https://arxiv.org/abs/2410.22954; Zou et al. (2026), environment-injected memory poisoning, https://arxiv.org/abs/2604.02623; Gao et al. (2026), MemPoison, https://arxiv.org/abs/2607.14651.

Recent Milestone 13 sources: Tishby, Pereira & Bialek (2000), https://arxiv.org/abs/physics/0004057; Alemi et al. (2016), https://arxiv.org/abs/1612.00410; Alemi et al. (2018), Fixing a Broken ELBO, https://arxiv.org/abs/1711.00464; Shao, Mao & Zhang, task-oriented communication, https://arxiv.org/abs/2102.04170; Balcan et al., distributed learning/communication complexity/privacy, https://arxiv.org/abs/1204.3514; Mölter & Goodhill (2020), https://www.mdpi.com/1099-4300/22/4/490; Hyvärinen & Morioka (2017), https://proceedings.mlr.press/v54/hyvarinen17a.html; Hyvärinen, Khemakhem & Monti (2023), https://doi.org/10.1007/s10463-023-00884-4; Yao et al. (2024), https://openreview.net/forum?id=6YpW4G8L1j; Jaegle et al. (2021), https://arxiv.org/abs/2103.03206; Bao et al. (2020), https://arxiv.org/abs/2001.09485; Fedus et al. (2022), https://jmlr.org/papers/v23/21-0998.html; Zhou et al. (2022), https://arxiv.org/abs/2202.09368; Liu et al. (2021), https://arxiv.org/abs/2110.14048; Gopalan et al. (2025), https://machinelearning.apple.com/research/communication-complexity.

Recent Milestone 14 sources: Guo et al. (2017), calibration, https://proceedings.mlr.press/v70/guo17a.html; Hendrycks & Gimpel (2017), OOD baseline, https://arxiv.org/abs/1610.02136; Lakshminarayanan et al. (2017), deep ensembles, https://arxiv.org/abs/1612.01474; Geifman & El-Yaniv (2019), SelectiveNet, https://proceedings.mlr.press/v97/geifman19a.html; Xu, Guo & Wei (2026), SCRC, https://arxiv.org/abs/2512.12844; Bai & Jin (2026), SCoRE, https://arxiv.org/abs/2603.24704; Sokol, Moniz & Chawla (2026), conformalized selective regression, https://doi.org/10.1007/s44248-026-00113-2; Kwon & Kim (2026), cost-aware deferral under shift, https://www.nature.com/articles/s41598-026-40637-w; Rahaman & Thiery (2020), deep ensembles and calibration, https://arxiv.org/abs/2007.08792; Zhang, Kailkhura & Han (2020), Mix-n-Match calibration, https://arxiv.org/abs/2003.07329.

Recent Milestone 15 sources: Fort (2022), adversarial OOD vulnerability, https://arxiv.org/abs/2201.07012; Sehwag et al. (2019), OOD adversarial examples, https://arxiv.org/abs/1905.01726; ACM Computing Surveys (2025), OOD/adversarial intersection, https://doi.org/10.1145/3719292; Tuna, Catak & Eskil (2023), uncertainty attacks/defenses, https://doi.org/10.1007/s40747-022-00701-0; Qin et al. (2023), uncertainty-based dynamic ensemble selection, https://arxiv.org/abs/2308.00346; Scholten & Günnemann (ICLR 2025), poisoning-robust conformal prediction, https://arxiv.org/abs/2410.09878; Zargarbashi et al. (ICML 2024), robust conformal sets, https://proceedings.mlr.press/v235/h-zargarbashi24a.html; VRCP (2025), https://doi.org/10.1016/j.patcog.2025.112051; MemoryGraft (2025), https://arxiv.org/abs/2512.16962; Dash et al. (2026), memory poisoning benchmark, https://arxiv.org/abs/2606.04329; Gao et al. (2026), MemPoison, https://arxiv.org/abs/2607.14651; Sharma (2026), SMSR, https://arxiv.org/abs/2606.12703; Liu et al. (2023), heterogeneous Byzantine robustness, https://arxiv.org/abs/2302.06079; Zhai et al. (2022), https://doi.org/10.3934/mbe.2022078; BPFLH (2026), https://doi.org/10.1109/TDSC.2026.3661522.

## Current evidence position

The most defensible near-term interpretation is a **fault-aware modular inference system with explicit detection, isolation, recovery, verification, learned inter-organ communication loops, a bounded shared workspace, stateful memory, and a decision layer that separately models uncertainty, novelty, calibration, disagreement, provenance, drift, and operational cost — while treating all of those signals as potentially attackable**. Its individual building blocks have substantial prior literature. The composition and adversarial robustness remain empirical research questions.

Claims of universal regeneration, literal immortality, zero downtime, consciousness from global workspace, universal spectral-gap/cognition relationships, fixed compute/latency improvements, spontaneous AGI-level evolution, indefinite exact memory, automatic correctness of retrieved memories, automatic fault attribution from disagreement, a universal scalar health variable, and adversarially trustworthy uncertainty/OOD signals remain unsupported hypotheses rather than established outcomes.

## Next milestone

**Milestone 16:** formalize and audit the **recovery/verification layer under adaptive compromise**: verifier independence, checkpoint integrity, state reconstruction vs. behavioral recovery, recovery-point/recovery-time tradeoffs, common-mode verifier failure, Byzantine recovery policies, and whether any meaningful end-to-end recovery guarantee can be proved under bounded fault and attack assumptions.
