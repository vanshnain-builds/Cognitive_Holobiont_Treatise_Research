# Milestone 24 — Mechanism Identifiability, Memory Governance, and Non-Stationary Generalization Audit

**Date:** 2026-09-25  
**Status:** research-only; falsification-first; no end-to-end validation claimed.

## Executive conclusion

Milestone 23 established causal attribution, resource matching, dependency-aware evidence, hidden evaluation, and latent-identifiability as evidence gates. Milestone 24 stress-tests them against current evidence on MoE routing, latent-memory adaptation, structured memory, self-evolving skills, persistent skill attacks, continuous knowledge drift, and non-stationary Byzantine learning.

The central correction is:

$$
\boxed{\text{behavioral improvement does not identify a unique cognitive mechanism}}
$$

Controlled MoE work reports that different routing topologies can achieve statistically similar language-modeling quality, while a companion study finds that individual experts can nevertheless be causally manipulated. These are compatible: topology-level equifinality can coexist with expert-level causal specialization.

A second correction is:

$$
\boxed{\text{memory improvement} \neq \text{truth improvement} \neq \text{generalization improvement}}
$$

A third is that self-evolving memory/skill systems change the future data distribution. Before/after comparisons therefore require controls for feedback-induced distribution shift.

## Evidence reviewed

1. **Equifinality in Mixture of Experts: Routing Topology Does Not Determine Language Modeling Quality** (2026): https://arxiv.org/abs/2604.14419
2. **Geometric Routing Enables Causal Expert Control in Mixture of Experts** (2026): https://arxiv.org/abs/2604.14434
3. **Dynamic Mixture of Latent Memories for Self-Evolving Agents (MoLEM)** (2026): https://arxiv.org/abs/2605.21951
4. **TiMem** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.1091/
5. **CLAG** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.824/
6. **CompassMem** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.1123/
7. **Remember Me, Refine Me** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.829/
8. **MemPO** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.1166/
9. **RAG or Learning? ... Continuous Knowledge Drift** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.546/
10. **Self-Play Meets Skill Evolution (SESA)** (2026): https://arxiv.org/abs/2607.29468
11. **SkillJack** (2026): https://arxiv.org/abs/2608.03509
12. **Dynamic Regret for Byzantine-Robust Online Federated Learning** (2026): https://doi.org/10.1109/TSP.2026.3673260
13. **BPFLH** (2026): https://doi.org/10.1109/TDSC.2026.3661522
14. **Agent-Dice** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.908/
15. **Learning How to Remember** (Findings ACL 2026): https://aclanthology.org/2026.findings-acl.1535/

## Formal corrections

### Mechanism identifiability
Let mechanism $m$ generate observable distribution $P_m(Y|X)$. If

$$P_{m_1}(Y|X)=P_{m_2}(Y|X)$$

on the evaluation distribution, observational evaluation cannot identify $m_1$ versus $m_2$. A mechanism claim therefore requires an intervention or identifiable invariant, e.g.

$$P(Y|do(a),m_1)\neq P(Y|do(a),m_2).$$

Benchmark superiority alone is not mechanism identification.

### Correlated specialists
For $n$ exchangeable estimates with variance $\sigma^2$ and pairwise correlation $\rho$,

$$Var(\bar X)=\frac{\sigma^2}{n}[1+(n-1)\rho],$$

so

$$n_{eff}=\frac{n}{1+(n-1)\rho}.$$

For non-exchangeable specialists:

$$Var(\bar X)=\frac{1}{n^2}{\bf1}^T\Sigma{\bf1}.$$

Nominal replica count is therefore not evidence count.

### Non-stationary evaluation
For online losses $f_t$,

$$R_T^{dyn}=\sum_{t=1}^T f_t(x_t)-\sum_{t=1}^T f_t(x_t^*),$$

with path length

$$P_T=\sum_{t=2}^T\|x_t^*-x_{t-1}^*\|.$$

Any theorem connecting regret to $P_T$ requires explicit assumptions; it cannot be transferred directly to a non-convex Holobiont.

### Memory utility

$$U(M)=J_{task}(M)-\lambda_C C(M)-\lambda_R R(M)-\lambda_P P(M),$$

where $C$ is resource cost, $R$ behavioral/security risk, and $P$ provenance uncertainty. This is an engineering objective, not a theorem.

### Promotion and rollback
A candidate memory/skill should pass separate gates:

$$G(c)=G_{local}\land G_{transfer}\land G_{safety}\land G_{provenance}\land G_{drift}.$$

Rollback must track derived-state lineage:

$$L(c)=\{source,transform,version,dependencies,uses\}.$$

Deleting one source record is not sufficient if summaries, skills, adapters or parameters retain its information.

## Claim-status matrix

### (1) Established, with scope
- Different MoE routing topologies can be empirically equifinal in at least some controlled settings.
- Individual experts can be causally manipulated in at least some MoE systems.
- Structured memory can improve tested long-horizon agent performance.
- Continuous knowledge drift can induce temporal inconsistency and forgetting.
- Persistent memory/skill state creates security risks.
- Byzantine robustness is affected by non-IID heterogeneity and non-stationarity.
- Privacy mechanisms can change the evidence available to robustness mechanisms.

### (2) Plausible engineering synthesis
- Separate topology, routing policy, specialization and task-quality claims.
- Treat memory and skills as versioned derived state.
- Require intervention-based mechanism tests.
- Evaluate memory under temporal drift.
- Use active-subgraph and dependency-aware robustness metrics.
- Combine local, transfer, safety, provenance and drift gates for promotion.
- Use hidden evaluation after self-evolution.

### (3) Unsupported/speculative
- One routing topology is universally superior.
- Expert specialization implies compositional reasoning.
- Latent-memory accumulation produces open-ended self-improvement.
- Structured memory guarantees factual correctness.
- Self-play skill evolution necessarily improves general intelligence.
- Self-evolving memory converges to a stable beneficial equilibrium.

### (4) Mathematically incorrect/incomplete without extra assumptions
$$balanced\ routing\Rightarrow optimal\ cognition$$

$$routing\ intervention\Rightarrow identified\ cognitive\ module$$

$$M_{t+1}\supseteq M_t\Rightarrow retention$$

$$local\ validation\Rightarrow transfer\ validity$$

$$global\ Byzantine\ fraction\ safe\Rightarrow active\ subgraph\ safe$$

$$static\ regret\ small\Rightarrow nonstationary\ tracking\ good$$

$$retrieval\ accuracy\ high\Rightarrow memory\ truth\ high.$$

## New falsification experiments

**E13.1 — Mechanism-equivalence matrix:** compare routers at matched active parameters, FLOPs, bandwidth and training budget; intervene separately on topology, router scores, expert outputs, expert parameters and communication edges.

**E13.2 — Memory-form equivalence:** compare flat retrieval, clustered memory, temporal hierarchy, event graph, latent-memory MoE and procedural skill memory while matching memory budget, retrieval calls and context tokens.

**E13.3 — Drift stress test:** introduce controlled change points and report dynamic regret, path length, backward transfer, adaptation latency and false persistence.

**E13.4 — Derived-state rollback:** poison an experience, allow it to become a summary/skill/adapter, delete the source, and test behavioral return to a trusted pre-injection checkpoint.

**E13.5 — Adaptive evaluator attack:** expose self-evolution to a proxy metric while withholding the primary metric; compare visible, hidden and transfer performance.

**E13.6 — Dependency-aware replication:** sweep specialist correlation $\rho$ and compare nominal $n$ with empirical effective evidence.

**E13.7 — Active-subgraph Byzantine concentration:** hold global Byzantine fraction fixed while changing routing and measure $\beta_t=|B_t|/|A_t|$ and attack persistence.

**E13.8 — Privacy–robustness frontier:** jointly sweep privacy/noise and Byzantine fraction; report the Pareto surface.

## Open problems

1. Characterize when adaptive modular architectures are observationally equivalent but interventionally distinguishable.
2. Derive uncertainty bounds when specialist covariance changes with routing.
3. Formalize retrieval-induced distribution shift in continual agent memory.
4. Define sufficient lineage information for behavioral rollback.
5. Bound adaptive adversaries that observe routing/trust updates.
6. Connect dynamic-regret bounds to graph-changing distributed systems.
7. Define resource-normalized comparisons across different parameterization and memory forms.
8. Characterize when latent-space agreement implies downstream functional equivalence.

## Decision gate

Milestone 24 does not authorize unrestricted implementation of self-evolving cognition. Before integrated experiments, require intervention-identifiable hypotheses, matched-resource baselines, hidden evaluation, memory/skill lineage, derived-state rollback, temporal drift, active-subgraph Byzantine testing, dependency-aware replication, explicit privacy accounting, and predeclared primary endpoints.

## Status

No validation of the complete Cognitive Holobiont is claimed. No claim of consciousness, AGI, open-ended self-improvement, autonomous self-healing, or universally optimal cognitive organization is supported.

The current falsifiable question is:

$$\boxed{\text{Can controlled modular change improve utility, retention and robustness under matched resources, causal intervention, drift, dependency and adversarial evaluation?}}$$
