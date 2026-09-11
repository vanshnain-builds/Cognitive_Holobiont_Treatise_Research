# Milestone 10 — End-to-End Memory/Workspace/Continual-State Reliability Audit

**Date:** 2026-09-11  
**Status:** research specification; no whole-system validation claimed

## 1. Purpose

This milestone connects the previously separate Treatise mechanisms into one auditable stateful system:

`specialists → latent bridges → workspace → memory → router → action/update → verification → persistent state`

The central question is whether their interaction creates measurable positive transfer without unacceptable interference, stale-state propagation, correlated failures, or unsafe persistent adaptation.

## 2. Claim classification

### Established result

1. Sparse MoE systems can route inputs/tokens to selected experts and obtain conditional computation.
2. Expert specialization is possible, but routing does not automatically produce clean task decomposition or balanced utilization.
3. Continual learning suffers catastrophic forgetting; replay, regularization, parameter isolation, prompting and architectural expansion can mitigate it under particular task distributions.
4. Persistent external memory/RAG can change model behavior without changing model parameters.
5. Cross-attention and learned latent bottlenecks provide practical mechanisms for cross-module information exchange.
6. Checkpointing, replication and lineage recovery are established fault-tolerance mechanisms for distributed computation.
7. Byzantine robustness is conditional on an explicit threat model and assumptions about honest updates/data; robustness is not synonymous with anomaly detection.

### Plausible engineering synthesis

1. A bounded workspace can act as a selective inter-organ communication state.
2. A memory layer can store durable facts/experiences while the workspace stores short-lived task state.
3. Reliability signals can gate both communication and memory writes.
4. Versioned, provenance-bearing memory can permit rollback after poisoning or corruption.
5. Specialist expansion can reduce parameter interference when new capabilities arrive, but introduces routing, capacity and synchronization costs.
6. A recovery service can restore a specialist from checkpoints, compressed state, teacher behavior, or a hypernetwork-generated initialization.

### Unsupported/speculative

1. The architecture will produce general intelligence merely by combining these mechanisms.
2. A shared workspace is necessary for consciousness or general intelligence.
3. Persistent memory creates indefinite knowledge retention.
4. Memory/workspace feedback automatically yields self-improvement.
5. Specialist regeneration can be lossless for arbitrary unseen distributions.
6. Faults necessarily make the system stronger after recovery (anti-fragility).
7. Evolutionary self-modification will autonomously produce AGI-level improvements.

### Mathematically incorrect/incomplete

1. Treating arbitrary latent vectors as probability distributions and applying KL divergence without a simplex-valued map.
2. Treating a graph spectral gap as a universal proxy for cognitive speed.
3. Treating attention/router weights as causal importance without intervention.
4. Treating parameter-distance preservation as equivalent to behavioral preservation.
5. Treating successful retrieval as evidence that a memory is correct.
6. Treating a robust aggregator as sufficient protection against arbitrary state corruption, compromised routers, poisoned memories, or adaptive attacks.

## 3. Unified state model

For specialist `i`, let `h_i,t = E_i(x_i,t; θ_i,t)` be its private representation. A typed bridge produces `z_i,t = P_i(h_i,t) ∈ R^d`.

Let the bounded workspace be `w_t ∈ R^(K×d)`, where `K` is the maximum number of workspace slots/tokens. Let persistent memory be `M_t = {(v_j, p_j, q_j, τ_j, g_j)}` where `v_j` is content, `p_j` provenance, `q_j` reliability/quality, `τ_j` version/time metadata and `g_j` access-scope/policy metadata.

Retrieval is `C_t = Retrieve(q_t, M_t)` and workspace update is `w_t = F(w_(t-1), {z_i,t}, C_t, u_t; φ)`.

Routing is `π_t = softmax(g(w_t, C_t, r_t))`, where `r_t` is reliability/context information. The selected specialist set is `S_t` under a capacity constraint.

Persistent state is `M_(t+1) = UpdateMemory(M_t, w_t, outputs_t, provenance_t, verification_t)`.

This makes the Holobiont a partially observable stateful dynamical system rather than a static ensemble.

## 4. Workspace versus memory

These concepts must remain separate.

**Workspace:** short-lived, capacity-limited state for current computation. Important variables: slot count `K`, dimension `d`, update frequency, broadcast sparsity, attention/routing entropy, read/write bandwidth and state lifetime.

**Persistent memory:** long-lived state. Important variables: capacity, retrieval precision/recall, write policy, provenance, versioning, deletion/rollback semantics, access control, poisoning resistance and staleness.

A large memory does not imply a large workspace. Therefore claims that more memory improves reasoning must be separated experimentally from claims that more workspace improves integration.

## 5. Positive transfer versus interference

Let task performance for task `k` under condition `c` be `R_k(c)` (lower is better).

Positive transfer from component `j` is `T_(j→k) = R_k(with j) - R_k(without j)`. Thus negative `T` is beneficial under a loss metric.

Interference is `I_(j→k) = R_k(after learning j) - R_k(before learning j)`.

A provisional aggregate transfer/interference score is `B_TI = mean(T_beneficial) - λ_I mean(max(I,0))`. This is an engineering metric, not a universal theoretical quantity. The Holobiont hypothesis requires pre-specified improvement and confidence intervals rather than only higher average benchmark accuracy.

## 6. Memory staleness

Let memory age be `a = t - τ`. A model such as `U(v,a)=U_0(v)exp(-κa)` can be used as a testable hypothesis, not as a law.

A more direct empirical staleness measure is

`S(v,t) = E[ℓ(f(M_t,v),y) | current distribution] - E[ℓ(f(M_τ,v),y) | reference distribution]`.

Age is evidence about time, not a correctness certificate: recent memory can be wrong and old memory can remain correct.

## 7. Persistent memory security

Recent work demonstrates that malicious experiences can be implanted into long-term agent memory and later retrieved on semantically similar tasks, producing persistent behavioral drift. See MemoryGraft (2025): https://arxiv.org/abs/2512.16962.

Recent work also proposes signed memory and smoothed retrieval with a formal certificate against defined runtime memory-poisoning attacks. This supports provenance as a useful security primitive but does not certify arbitrary memory systems. See SMSR (2026): https://arxiv.org/abs/2606.12703.

For the Holobiont, durable memory should carry `provenance = (source, time, producer, evidence, transformation, version, authorization)`.

A memory write should follow `candidate → validation → authorization → versioned commit`. A failed validation must not silently become persistent truth.

## 8. Shared memory and access control

Multi-user/multi-agent memory requires asymmetric access control. Collaborative Memory (2025) models private and shared fragments with provenance and dynamic policies: https://arxiv.org/abs/2505.18279.

The Treatise should distinguish `shared semantic knowledge` from `private episodic/contextual state`. Cross-organ sharing should be policy-filtered rather than unconditional broadcast.

## 9. Continual learning and modular growth

Recent theoretical work on MoE continual learning supports a limited version of the Treatise's modular-growth intuition: in an overparameterized linear setting, MoE can specialize experts and reduce forgetting, but more experts can require more training rounds, and continual router updates can prevent convergence unless handled carefully. Li et al. (2024): https://arxiv.org/abs/2406.16437.

Thus `more organs ⇒ monotonically better intelligence` is not established.

A more realistic objective is

`U = performance - λ_compute C - λ_comm B - λ_mem S - λ_forget F - λ_risk Q`.

Adding an organ is beneficial only when marginal task utility exceeds marginal resource and reliability cost.

Recent similarity-aware MoE continual-learning work further supports explicit modeling of task overlap and negative transfer: https://arxiv.org/abs/2603.23436.

## 10. Router stability

The router is itself an adaptive component. Let `π_t(x)=Router(x;ψ_t)`. Router drift can be measured by

`D_router(t,t+1)=E_x[D_KL(π_t(x)||π_(t+1)(x))]`

when both distributions share support and numerical smoothing is applied as necessary.

A large router change can alter behavior while specialists remain unchanged. Recovery tests must therefore include specialist failure with fixed router, router failure with fixed specialists, simultaneous corruption, stale router state, and restoration from a previous router checkpoint.

## 11. Information persistence and regeneration

Suppose memory state `M` compresses historical data `H`. If two histories map to the same stored state but require different future outputs, exact reconstruction is impossible without an additional information source.

Therefore regeneration claims must specify a future task family `Q` and tolerance `ε`, for example

`P[d(ŷ_q,y_q)≤ε] ≥ 1-δ, q~Q`.

"The knowledge survives" is not a sufficient scientific criterion.

## 12. Recovery hierarchy

**R0:** no recovery.  
**R1:** checkpoint restoration.  
**R2:** replicated-state restoration.  
**R3:** teacher-guided reconstruction.  
**R4:** hypernetwork reconstruction/initialization.  
**R5:** functional adaptation under the current workload.

These are not equivalent. R1/R2 can preserve a previous verified state; R3–R5 can change behavior and require behavioral verification.

## 13. End-to-end reliability metrics

Report at least: task risk/accuracy, forgetting, transfer, memory retrieval precision/recall, stale-memory error, poisoned-memory acceptance, router drift, workspace utilization, communication bytes/query, p50/p95/p99 latency, compute, detection time, quarantine false-positive rate, recovery time, recovery fidelity, post-recovery calibration, correlated-failure probability, and availability.

Operational availability can be defined as `A=uptime/(uptime+downtime)` over a fixed interval. MTTR is `MTTR=(1/N)Σ_i(t_recovered,i-t_failed,i)`.

These are reliability quantities, not evidence of immortality.

## 14. Correlated failure

Nominal specialist redundancy can fail to protect the system if specialists share a router, workspace, embedding model, memory index, retrieval model, hypernetwork, hardware/network substrate, training data or software dependency.

Define a dependency graph `G_dep` and measure failure-domain diversity, not just replica count. Pairwise failure correlation can be summarized by `ρ_fail=P(F_i=1,F_j=1)-P(F_i=1)P(F_j=1)` as an interpretable experimental statistic. High positive correlation reduces the practical value of nominal redundancy.

## 15. Reliability-aware routing

Let candidate organ `i` have expected utility `U_i(x)`, estimated risk `q_i(x)`, and cost `c_i`. A constrained routing problem is

`max_π E[Σ_i π_iU_i]`

subject to

`Σ_iπ_iq_i≤q_max`, `Σ_iπ_ic_i≤C_max`, `π_i≥0`, `Σ_iπ_i=1`.

This is a valid engineering formulation. It does not establish that `q_i` is calibrated or trustworthy; that must be tested separately.

## 16. Consensus and persistent state

Consensus should be applied only where agreement is semantically appropriate. For ordinary connected undirected consensus `x_dot=-Lx`,

`||x(t)-x̄1||₂ ≤ exp(-λ₂(L)t)||x(0)-x̄1||₂`.

This is a convergence statement for specified dynamics, not a theorem that consensus creates cognition. Private specialist representations and task-specific memories should not automatically be forced to consensus. More plausible consensus variables are health estimates, protocol versions, routing metadata, checkpoint identifiers and shared calibration statistics.

## 17. Hypotheses H22–H27

**H22:** bounded workspace improves cross-task transfer over no-workspace and direct-message baselines at matched budgets. Falsify if improvement disappears after compute/bandwidth matching.

**H23:** validated selective memory outperforms raw full-history persistence under equal storage budgets. Falsify if full history matches or exceeds it without unacceptable latency/staleness.

**H24:** provenance-gated memory writes reduce persistent poisoning without exceeding a pre-specified clean-utility loss. Falsify if attack success remains comparable or clean utility loss is excessive.

**H25:** modular growth reduces forgetting relative to monolithic fine-tuning only when routing remains stable. Falsify if modular growth gives no retention benefit or router stability has no measurable effect.

**H26:** failure-domain diversity predicts recovery resilience. Falsify if equal replica counts with common-mode dependencies perform equivalently under controlled common-mode faults.

**H27:** explicit memory/workspace separation reduces stale-state propagation versus a unified store after capacity and retrieval costs are matched. Falsify if the unified design performs equally or better.

## 18. Required experiment matrix

| Condition | Workspace | Persistent memory | Router | Faults | Primary question |
|---|---|---|---|---|---|
| E0 | none | none | static | none | specialist baseline |
| E1 | bounded | none | adaptive | none | workspace value |
| E2 | bounded | selective | adaptive | none | memory/workspace interaction |
| E3 | bounded | full history | adaptive | none | stale-context effect |
| E4 | bounded | selective + provenance | adaptive | poisoning | persistent-memory security |
| E5 | bounded | selective | adaptive | crash | recovery |
| E6 | bounded | selective | adaptive | Byzantine | adversarial reliability |
| E7 | bounded | selective | corrupted | crash + corruption | common-mode failure |
| E8 | bounded | selective | adaptive | non-IID continual tasks | retention/transfer |

Every comparison should be matched on parameter count, examples/tokens, communication budget and inference compute where feasible.

## 19. Statistical protocol

Use independent seeds and report mean, dispersion, confidence intervals and effect sizes. Use paired evaluation tasks where possible. Select hyperparameters without repeatedly optimizing on the held-out test set. Pre-register failure schedules and recovery criteria to avoid selecting favorable trajectories after observing outcomes.

## 20. Evidence audit

| Treatise proposition | Classification | Required evidence |
|---|---|---|
| Specialists reduce interference | Established/conditional | continual-learning + modular baseline |
| Adaptive routing improves efficiency | Established/conditional | matched-compute MoE benchmark |
| Shared workspace improves integration | Plausible synthesis | causal matched-budget benchmark |
| Persistent memory preserves knowledge | Conditional | retention + retrieval benchmark |
| Provenance protects memory | Established for defined threats / synthesis for Holobiont | adversarial memory benchmark |
| Failed specialist can be reconstructed | Plausible synthesis | recovery experiment |
| Arbitrary specialist can be losslessly regenerated | Unsupported | information-sufficiency proof/benchmark |
| Consensus creates cognitive coherence | Speculative | operationalize coherence and test |
| Spectral gap measures thought speed | Incorrect as universal statement | restrict to specified consensus dynamics |
| Recovery creates anti-fragility | Unsupported | controlled stress-performance test |
| More organs always improve intelligence | Unsupported | marginal utility/resource study |
| Self-modification yields autonomous AGI improvement | Unsupported/speculative | safety-constrained longitudinal evaluation |

## 21. Prototype gate

The evidence is sufficient to build a **small falsifiable prototype**, not to claim the full thesis is validated. Initial system: `3–5 frozen specialists + learned bridge + bounded workspace + selective versioned memory + adaptive router + crash/recovery injection`.

Byzantine attacks, persistent poisoning, model regeneration and self-modification should remain separate experimental layers until the clean system has reproducible baselines.

## 22. Open problems

1. Minimum workspace capacity for useful cross-organ integration.
2. Communication cost at which transfer becomes uneconomic.
3. Safe consolidation of noisy experiences.
4. Representation of conflicting memories without premature resolution.
5. Calibration of reliability under shift/adversarial manipulation.
6. Information sufficient for behavioral specialist reconstruction.
7. Quantification of neural failure-domain diversity.
8. Modularity versus routing collapse/redundancy.
9. Auditable and revocable persistent memory.
10. Formal safety constraints for self-modification.

## 23. References

- Fedus, Zoph & Shazeer, Switch Transformers: https://arxiv.org/abs/2101.03961
- Shazeer et al., Sparsely-Gated MoE: https://arxiv.org/abs/1701.06538
- Li et al., Theory on MoE in Continual Learning: https://arxiv.org/abs/2406.16437
- Krishnamurthy, Watkins & Gaertner, Expert Specialization: https://arxiv.org/abs/2302.14703
- Mclaughlin, Lee & Su, Similarity-Aware MoE CL: https://arxiv.org/abs/2603.23436
- Ha, Dai & Le, HyperNetworks: https://arxiv.org/abs/1609.09106
- Lewis et al., RAG: https://arxiv.org/abs/2005.11401
- Packer et al., MemGPT: https://arxiv.org/abs/2310.08560
- Park et al., Generative Agents: https://arxiv.org/abs/2304.03442
- Srivastava & He, MemoryGraft: https://arxiv.org/abs/2512.16962
- Sharma, SMSR: https://arxiv.org/abs/2606.12703
- Rezazadeh et al., Collaborative Memory: https://arxiv.org/abs/2505.18279
- Zuo et al., Byzantine-resilient FL: https://arxiv.org/abs/2408.09539
- Blanchard et al., Byzantine Tolerant Gradient Descent: https://arxiv.org/abs/1703.02757
- Yin et al., Byzantine-Robust Distributed Learning: https://proceedings.mlr.press/v80/yin18a.html
- Olfati-Saber, Fax & Murray, Consensus: https://ieeexplore.ieee.org/document/4118472
- Guo et al., Calibration: https://arxiv.org/abs/1706.04599
- Lakshminarayanan et al., Deep Ensembles: https://arxiv.org/abs/1612.01474

## 24. Decision

Proceed to a small controlled prototype while treating it strictly as a falsification instrument. Preserve independently observable and ablatable private specialist state, bounded workspace state, persistent memory, router state and recovery state. Do not conflate component success with validation of the complete Cognitive Holobiont thesis.
