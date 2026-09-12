# Milestone 11 — Formal End-to-End Prototype Specification

**Date:** 2026-09-12  
**Status:** pre-implementation specification; no empirical validation claimed

## 1. Purpose

Milestones 01–10 audited the Treatise components separately and then examined their interactions. This milestone converts those conclusions into a **minimal, falsifiable end-to-end prototype specification**. The goal is not to demonstrate AGI, consciousness, immortality, or autonomous evolution. The goal is to determine whether a small modular system gains measurable utility from the proposed Holobiont mechanisms after matching for model capacity, data, communication, and inference compute.

The prototype is intentionally smaller than the Treatise. It should isolate causal contributions before adding Byzantine recovery, persistent self-modification, or evolutionary search.

## 2. Evidence status of the proposed architecture

### Established / conditional

- Sparse mixture-of-experts can increase parameter capacity while conditionally activating a subset of experts; routing and load balancing are active design problems.
- Learned inter-agent communication can improve cooperative task performance, but emergent messages can be inefficient, non-compositional, or brittle under channel constraints.
- Cross-attention and latent bottlenecks are established mechanisms for information exchange.
- External memory can alter behavior without changing model parameters; long-term memory management remains an active research problem.
- Checkpointing, replicas, dependency-aware recovery, and fault detection are established distributed-systems techniques.
- Conformal/selective prediction can provide distribution-free guarantees under its required assumptions; these guarantees should not be generalized to arbitrary distribution shift or compromised calibration data.

### Plausible engineering synthesis

- A bounded workspace can mediate sparse inter-specialist communication.
- A typed bridge can map specialist representations into a common communication space without forcing private representations to coincide.
- A reliability-aware router can trade expected utility against communication/compute/risk constraints.
- Versioned, provenance-bearing memory can support rollback and conflict handling.
- Specialist-local failure can be isolated and recovered without reconstructing the entire system.

### Unsupported / speculative

- General intelligence emerges from composition of these mechanisms.
- A workspace is necessary or sufficient for consciousness.
- The architecture has universal lossless regeneration.
- More specialists monotonically increase intelligence.
- Fault exposure necessarily produces anti-fragility.
- Self-modification will autonomously discover AGI-level improvements.

### Mathematically incorrect / incomplete if stated without assumptions

- KL divergence between arbitrary hidden vectors.
- Spectral gap as a universal cognitive-speed variable.
- Attention weights as causal explanations.
- Parameter similarity as behavioral equivalence.
- Retrieval as truth certification.
- Robust aggregation as a universal solution to arbitrary Byzantine, router, memory, and state corruption.

## 3. Minimal system definition

Use `N = 4` frozen specialist modules for the first experiment. Suggested roles are deliberately generic so the experiment can be reproduced with several backbone families:

1. specialist A: visual/perceptual feature extraction;
2. specialist B: language/semantic feature extraction;
3. specialist C: structured/relation feature extraction;
4. specialist D: task-specific reasoning/decision support.

For each specialist,

`h_i = E_i(x_i; theta_i)`.

Each bridge maps to a common dimension `d`:

`z_i = P_i(h_i) in R^d`.

Private states remain in their native spaces. Only `z_i` is eligible for workspace communication.

Workspace state:

`w_t in R^(K x d)`

with fixed token/slot capacity `K`.

Persistent memory is initially **selective and versioned**, not an unrestricted vector dump:

`m_j = (content, key, provenance, version, timestamp, confidence, scope)`.

The first prototype should use a deterministic storage backend so that retrieval behavior can be separated from storage-system effects.

## 4. Workspace protocol

At time `t`, each active specialist emits a bridge representation and an optional utility/reliability score:

`z_i,t = P_i(E_i(x_i,t; theta_i))`.

A workspace update is

`w_t = F_phi(w_(t-1), Z_t, C_t, u_t)`

where `Z_t` is the selected set of bridge tokens and `C_t` is retrieved memory context.

The workspace has a strict token budget `K` and communication budget `B_max` bytes per decision.

The no-workspace baseline must not receive equivalent hidden-state access through an accidental side channel.

## 5. Routing objective

Let specialist `i` have estimated utility `U_i(x,w,C)`, cost `c_i`, and risk estimate `q_i`. The router produces a distribution `pi` over candidate specialists.

A constrained formulation is

`maximize_pi E[sum_i pi_i U_i]`

subject to

`sum_i pi_i c_i <= C_max`,

`sum_i pi_i b_i <= B_max`,

`sum_i pi_i q_i <= Q_max`,

`pi_i >= 0`, `sum_i pi_i = 1`.

This is a decision objective, not a guarantee. The first implementation may use a Lagrangian surrogate:

`L = -E[U] + lambda_c E[c] + lambda_b E[b] + lambda_q E[q]`.

Risk estimates must be evaluated for calibration under clean and shifted conditions before being trusted by the router.

## 6. Communication mechanisms to compare

The primary bridge ablation is:

**C0:** no inter-specialist communication.  
**C1:** raw projected messages.  
**C2:** learned continuous bridge with task loss.  
**C3:** learned bridge + information bottleneck / message sparsity.  
**C4:** shared/private representation: private specialist state plus a constrained shared channel.

The main comparison is not simply accuracy. Record utility as a function of communication budget.

Define

`DeltaU(B) = U(B) - U(0)`

and communication efficiency

`eta(B) = DeltaU(B) / B`.

This is an experimental metric, not a universal information-theoretic theorem.

## 7. Memory protocol

Use three memory conditions:

**M0:** no persistent memory.  
**M1:** raw episodic log with fixed storage budget.  
**M2:** validated selective memory with typed records and provenance.

A memory write follows:

`candidate -> validation -> authorization -> versioned commit`.

The retrieval layer returns records with provenance and version identifiers. The model must not be allowed to silently rewrite an existing record merely because a new generated statement has higher embedding similarity.

Introduce contradiction tests where a newly observed statement conflicts with an existing memory. The system must expose the conflict rather than silently replacing evidence.

### Memory evaluation

Measure:

- retrieval precision and recall;
- answer correctness conditional on retrieved memory;
- stale-memory error;
- contradiction detection;
- poisoning acceptance rate;
- rollback success;
- storage bytes per retained useful fact;
- retrieval latency.

Long-context and agent-memory research indicates that long histories can remain difficult even with large context windows and that structured consolidation/retrieval can alter the accuracy/cost trade-off. This motivates the M0/M1/M2 comparison rather than assuming that more memory is always better. See BEAM and recent long-term memory work in References.

## 8. Continual-learning protocol

Use a sequence of task distributions `D_1, ..., D_T`.

After training on task `j`, evaluate all previously learned tasks. Define forgetting on task `k` after learning task `j` as

`F_(k,j) = R_k(after j) - R_k(after k)`

for loss `R` (positive values indicate increased loss).

Also report forward transfer and backward transfer separately. The modular condition must be compared with a matched-capacity monolithic model.

Router drift is measured as

`D_router(t,t+1) = E_x[D_KL(pi_t(x) || pi_(t+1)(x))]`

with smoothing or a symmetric alternative if support mismatch makes KL unstable.

The key hypothesis is conditional:

`modularity -> lower interference` only if router stability and communication costs remain acceptable.

## 9. Recovery protocol

Recovery is staged and must never be silently accepted as correctness.

**R0:** fail-stop.  
**R1:** checkpoint restoration.  
**R2:** replica restoration.  
**R3:** teacher-guided reconstruction.  
**R4:** hypernetwork-generated reconstruction.  
**R5:** current-task functional adaptation.

For R1/R2, verify identity of the restored state against a trusted checkpoint hash/version. For R3–R5, behavioral verification is mandatory.

Let `f` be the verified pre-failure specialist and `f_hat` the recovered specialist. Define evaluation loss gap

`Delta_R = R_Q(f_hat) - R_Q(f)`.

A recovery passes only if the pre-registered threshold is satisfied:

`Delta_R <= epsilon_R`

with a confidence interval or other stated statistical criterion.

The evaluation distribution `Q` must be specified before failure injection. No post-hoc selection of favorable recovery examples is allowed.

## 10. Failure taxonomy for the prototype

The first fault experiments use deterministic injected failures:

1. crash / fail-stop;
2. delayed response;
3. stale checkpoint;
4. corrupted local state;
5. incorrect output from an otherwise responsive specialist;
6. router corruption;
7. workspace corruption;
8. memory-record poisoning;
9. common-mode dependency failure.

Do not initially combine all faults. Each failure type must have an isolated experiment before mixed-fault trials.

Byzantine behavior should be introduced only after clean recovery and detection baselines are stable. Robust aggregation is conditional on its threat model; heterogeneous data can itself generate honest disagreement and can defeat naive anomaly rules.

## 11. Verification architecture

Separate four questions:

1. **Is the component alive?**
2. **Is its output statistically plausible?**
3. **Is its output useful for the current task?**
4. **Is the persistent state authorized and provenance-valid?**

No single scalar health score should be treated as answering all four.

A minimal health vector is

`r_i = (liveness, calibration, OOD_score, consistency, provenance_state)`.

The router consumes a calibrated subset of these signals. Provenance state is a policy/security property rather than a probability and should not be numerically conflated with predictive confidence.

## 12. Byzantine extension

For a later extension, assume at most `f` Byzantine participants among `n` participants and state the exact assumptions of the chosen aggregation protocol. Robust aggregation guarantees from the literature generally require restrictions on the loss/data/update model and cannot be transferred to arbitrary neural state.

The benchmark must include:

- IID honest participants;
- heterogeneous honest participants;
- untargeted Byzantine updates;
- targeted model-poisoning updates;
- colluding attackers;
- adaptive attacks aware of the detector;
- detector/router compromise.

Report clean utility and attack success together. A defense that reduces attack success by destroying clean specialization is not a successful Holobiont defense.

## 13. Statistical design

For every primary condition:

- use at least 5 independent random seeds where computationally feasible;
- fix train/validation/test partitions;
- pre-specify primary metrics and thresholds;
- report mean, standard deviation, confidence interval and effect size;
- retain per-task results rather than only a global mean;
- match parameter count, training examples/tokens, inference compute and communication budget as closely as possible;
- measure wall-clock latency separately from FLOPs;
- evaluate on in-distribution and shifted test sets;
- keep fault schedules and attack seeds fixed within paired comparisons;
- do not tune against the final test set.

For paired task outcomes, use bootstrap confidence intervals or an appropriate paired test. Report the full distribution of recovery times rather than only MTTR.

## 14. Primary benchmark matrix

| Experiment | Communication | Workspace | Memory | Router | Fault | Primary question |
|---|---|---|---|---|---|---|
| B0 | none | none | none | static | none | monolithic/specialist baseline |
| B1 | C0 | none | none | static | none | value of decomposition |
| B2 | C1/C2/C3/C4 | bounded | none | adaptive | none | value/efficiency of latent communication |
| B3 | best communication | bounded | M0/M1/M2 | adaptive | none | memory-workspace interaction |
| B4 | best | bounded | best | adaptive | continual tasks | transfer vs forgetting |
| B5 | best | bounded | best | adaptive | crash/stale/corruption | recovery utility |
| B6 | best | bounded | best | adaptive/corrupted | adversarial | reliability and isolation |
| B7 | best | bounded | best | adaptive | common-mode | redundancy limits |

The decision to add a subsystem requires that it improves a pre-registered primary metric without violating resource and reliability constraints.

## 15. Complexity accounting

For each condition report:

`C_total = C_expert + C_bridge + C_workspace + C_memory + C_router + C_verification`.

Do not report a single FLOP count as total cost. Include communication bytes, serialization, retrieval, verification and recovery overhead.

For a query with selected experts `S`, communication is approximately

`B_comm = sum_(i in S) B_i + B_workspace_in + B_workspace_out`.

Latency should be measured as an end-to-end critical-path quantity, not as the sum of independent component averages:

`T_e2e = T_serial + max(T_parallel) + T_comm + T_retrieval + T_verify + T_recovery`.

The exact decomposition depends on implementation scheduling and should be instrumented rather than assumed.

## 16. Falsification criteria

The Holobiont proposal is weakened if:

- the workspace gives no improvement after compute/bandwidth matching;
- learned bridges provide no transfer beyond raw projections;
- selective memory provides no utility/cost advantage over raw history;
- modular continual learning does not reduce forgetting relative to matched monolithic training;
- router drift does not predict behavioral drift;
- recovery provides no measurable advantage over restart/checkpoint baselines;
- common-mode failures erase redundancy benefits;
- reliability-aware routing fails under calibrated distribution shift;
- poisoning defenses either fail to reduce attack success or cause unacceptable clean-utility loss.

The hypothesis is strengthened only by reproducible effect sizes across multiple seeds, tasks, model families and fault schedules.

## 17. What this prototype cannot establish

Even a successful B0–B7 study cannot establish:

- consciousness;
- subjective experience;
- universal intelligence;
- literal immortality;
- lossless arbitrary regeneration;
- indefinite exact memory;
- autonomous scientific discovery at AGI level;
- safe unrestricted self-modification.

Those require separate operational definitions and evidence.

## 18. Research hypotheses

**H28 — Communication Pareto improvement:** At matched specialist capacity, a learned sparse bridge achieves a better task-utility/communication Pareto frontier than raw projected communication.

**H29 — Workspace bottleneck:** There exists a finite workspace capacity `K*` beyond which additional workspace tokens produce diminishing task utility relative to communication/compute cost.

**H30 — Selective memory:** Provenance-gated selective memory has lower storage-adjusted error than raw episodic persistence under equal storage budgets.

**H31 — Router stability:** Large distributional drift in routing decisions predicts a measurable increase in downstream task risk, after controlling for specialist parameter drift.

**H32 — Verified recovery:** Recovery methods that include an explicit behavioral verification gate reduce post-recovery task-risk spikes relative to unverified restoration/reconstruction.

**H33 — Common-mode dependency:** Failure-domain diversity predicts resilience better than raw replica count under controlled correlated failures.

**H34 — Reliability-aware routing:** When risk estimates are calibrated, constrained routing reduces failure-weighted loss at a tolerable compute/latency cost; when risk estimates are miscalibrated, routing can become actively harmful.

## 19. Decision gate for implementation

The research phase has reached a point where a **small prototype is justified as a falsification instrument**. The recommended first implementation is B0–B3 only. Do not begin with Byzantine attacks, persistent poisoning, hypernetwork regeneration, or self-modification. Those layers add confounders before the basic causal value of modularity, communication, workspace, and memory is known.

After B0–B3 are reproducible, add continual learning (B4), then crash/recovery (B5), then adversarial/common-mode failures (B6/B7). Hypernetwork reconstruction should be compared against ordinary checkpoint and teacher-guided baselines rather than evaluated in isolation.

## 20. New literature reviewed for Milestone 11

- Zhou et al., **Mixture-of-Experts with Expert Choice Routing**, arXiv:2202.09368. https://arxiv.org/abs/2202.09368 — supports the view that routing strategy and expert capacity are first-class optimization variables.
- Karimireddy, He & Jaggi, **Byzantine-Robust Learning on Heterogeneous Datasets via Bucketing**, arXiv:2006.09365. https://arxiv.org/abs/2006.09365 — demonstrates that non-IID heterogeneity changes Byzantine-robustness assumptions and attack behavior.
- Yin et al., **Byzantine-Robust Distributed Learning: Towards Optimal Statistical Rates**, arXiv:1803.01498. https://arxiv.org/abs/1803.01498 — gives formal robustness/statistical results under specified models; not a universal neural-state guarantee.
- Xu, Guo & Wei, **Selective Conformal Risk Control**, arXiv:2512.12844. https://arxiv.org/abs/2512.12844 — relevant to selective routing/abstention with explicit coverage/risk assumptions.
- Bao et al., **CAP: A General Algorithm for Online Selective Conformal Prediction with FCR Control**, arXiv:2403.07728. https://arxiv.org/abs/2403.07728 — relevant to online selection and finite-sample risk control under stated assumptions.
- Tavakoli et al., **Beyond a Million Tokens: Benchmarking and Enhancing Long-Term Memory in LLMs**, arXiv:2510.27246. https://arxiv.org/abs/2510.27246 — supports explicit evaluation of long-term memory rather than assuming context length equals memory quality.
- Wei et al., **Evo-Memory: Benchmarking LLM Agent Test-time Learning with Self-Evolving Memory**, arXiv:2511.20857. https://arxiv.org/abs/2511.20857 — supports evaluating memory as an evolving state and measuring continual experience reuse.
- Li et al., **LycheeMemory V2**, arXiv:2608.12990. https://arxiv.org/abs/2608.12990 — recent evidence that memory-consolidation granularity affects accuracy/cost trade-offs.
- Zhao et al., **Accurate and Efficient Long-Term Memory for LLM Agents**, arXiv:2607.16211. https://arxiv.org/abs/2607.16211 — relevant to structured conflict-aware memory and validation at write time.
- Gandhi & Kozyrakis, **Sparse Checkpointing for Fast and Reliable MoE Training**, NSDI 2026. https://www.usenix.org/conference/nsdi26/presentation/gandhi — directly relevant to sparse modular-state checkpointing and localized recovery.
- Deng et al., **Minder: Faulty Machine Detection for Large-scale Distributed Model Training**, NSDI 2025. https://www.usenix.org/conference/nsdi25/presentation/deng — supports separating fault detection from task correctness and measuring detection latency/precision.
- Chen et al., **RobustRL: Role-Based Fault Tolerance System for RL Post-Training**, OSDI 2026. https://www.usenix.org/conference/osdi26/presentation/chen-zhenqian — relevant to role-level fault isolation and detect/restart/reconnect recovery.
- Foerster et al., **Learning to Communicate with Deep Multi-Agent Reinforcement Learning**, arXiv:1605.06676. https://arxiv.org/abs/1605.06676 — foundational learned communication mechanism.
- Devillers, Maytié & VanRullen, **A Computational Model of Global Workspace**, arXiv:2306.15711. https://arxiv.org/abs/2306.15711 — relevant computational precedent for specialist systems communicating through a workspace.
- Lewis et al., **Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks**, arXiv:2005.11401. https://arxiv.org/abs/2005.11401 — foundational external-memory/retrieval architecture.

## 21. Provenance notes

Sources were prioritized in this order: primary papers with freely accessible full text, open conference proceedings from established systems venues, then surveys for discovery/context. Newer 2026 arXiv results are treated as preliminary evidence until independently reproduced. Quantitative claims from source papers are not transferred to the Holobiont; they only motivate analogous measurements.

## 22. Next research target

**Milestone 12:** formalize the mathematical learning objectives and optimization dynamics for B0–B3, including bridge training, workspace update equations, sparse routing/load balancing, memory write/read objectives, information-budget constraints, and a capacity-matched baseline construction. The purpose is to remove remaining implementation ambiguity before coding and to identify any objective that is ill-posed or permits degenerate solutions.
