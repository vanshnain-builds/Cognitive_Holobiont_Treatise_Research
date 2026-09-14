# Milestone 13 — Information-Flow, Rate–Distortion, Identifiability, and Coupled Optimization Audit

**Status:** pre-implementation research / falsification-first

**Scope:** formalize the B0–B3 bridge/workspace/memory/router stack as a constrained information-processing system. This milestone asks which quantities can actually be bounded before implementation and which Treatise claims remain engineering hypotheses.

## 1. Executive conclusion

The strongest mathematically defensible version of the Holobiont is a **task-oriented distributed representation system with finite communication and memory budgets**. The literature supports information bottlenecks, task-oriented feature communication, multi-view representation learning, sparse conditional computation, and explicit resource constraints. It does **not** support a universal theorem that a shared latent space, workspace, memory, or routing loop increases intelligence.

The key correction is to separate four quantities that are often conflated:

1. **representation information:** what a latent contains about inputs/targets;
2. **communication information:** what survives transmission through a finite channel;
3. **decision utility:** what the receiver can actually use for the downstream task;
4. **system resource cost:** bits, FLOPs, memory, latency, and energy.

The Treatise should therefore optimize and report a vector-valued frontier rather than a single “cognitive efficiency” number.

---

## 2. Formal system

For organ/specialist `i`, define

\[
h_i = f_i(x_i;\theta_i),
\qquad
z_i = P_i(h_i;\phi_i).
\]

A communication channel with budget `B_i` produces

\[
\tilde z_i = C_i(z_i;\xi_i),
\qquad
\ell(C_i) \le B_i,
\]

where `\ell` is a declared coding-length/bit-cost functional. The shared workspace is

\[
w_{t+1}=F_\psi(w_t,\{\tilde z_i\}_{i\in A_t},u_t),
\qquad
w_t\in\mathbb R^{K\times d_w}.
\]

The router chooses an active set `A_t` using observable state `s_t`:

\[
\pi(A_t\mid s_t).
\]

A downstream organ receives a readout

\[
\hat h_j=D_j(w_t;\omega_j),
\]

and the final decision is

\[
\hat y = G(\hat h_1,\ldots,\hat h_m,m_t).
\]

Persistent memory evolves as

\[
M_{t+1}=\operatorname{Commit}(M_t,c_t),
\]

with an explicit provenance/validation policy. This is important: memory is state, not merely an external database.

### Coupled objective

Use constrained optimization rather than an arbitrary weighted sum:

\[
\min_{\Theta}
\;R_D(\Theta)
\]

subject to

\[
\mathbb E[B_t]\le B_{\max},
\quad
\mathbb E[C_t]\le C_{\max},
\quad
\mathbb E[M_t]\le M_{\max},
\quad
\mathbb E[L_t]\le L_{\max},
\quad
\mathcal R_t\le r_{\max},
\]

where `R_D` is task risk, `B_t` communication, `C_t` compute, `M_t` memory footprint, `L_t` latency, and `\mathcal R_t` a declared reliability/risk metric.

A scalar Lagrangian is permitted only after the constraints and units are specified:

\[
\mathcal L
=R_D+\lambda_B(B-B_{\max})
+\lambda_C(C-C_{\max})
+\lambda_M(M-M_{\max})
+\lambda_L(L-L_{\max})
+\lambda_R(\mathcal R-r_{\max}).
\]

The Pareto frontier is the primary object; a single scalar score is secondary.

---

## 3. Information-flow claims

### 3.1 Data processing inequality — established

If

\[
X\rightarrow Z\rightarrow \hat Y
\]

forms a Markov chain, then

\[
I(X;\hat Y)\le I(X;Z).
\]

Therefore a bridge cannot create information about `X` that was absent from `Z`. It can only transform, select, compress, or exploit information already present.

This does **not** imply that more `I(X;Z)` is better for the task. Irrelevant information can increase rate and hurt robustness or generalization.

### 3.2 Information Bottleneck — established, but objective-dependent

The classical IB objective is

\[
\min_{p(z|x)} I(X;Z)-\beta I(Z;Y).
\]

Equivalently, maximize `I(Z;Y)-\beta^{-1}I(X;Z)` under a chosen parameterization. This formalizes a rate/predictiveness tradeoff. The original IB work explicitly connects relevant-information preservation with a short code. See Tishby, Pereira & Bialek (2000):

https://arxiv.org/abs/physics/0004057

Deep VIB gives a tractable variational approximation, but its bounds depend on variational distributions and assumptions. See Alemi et al. (2016):

https://arxiv.org/abs/1612.00410

A later analysis of the ELBO/latent-information relationship shows that identical ELBO values can correspond to different latent representations, so an ELBO optimum is not automatically a unique or semantically ideal code:

https://arxiv.org/abs/1711.00464

### 3.3 Communication-constrained inference — established

Task-oriented communication work directly formulates compact feature transmission for downstream inference rather than reconstruction. See Shao, Mao & Zhang:

https://arxiv.org/abs/2102.04170

This is close to the Holobiont's proposed latent bridge. The defensible claim is therefore:

> **A learned compact representation can improve task performance under a communication constraint in suitable settings.**

The stronger claim

> **any shared latent bridge is information-efficient**

is unsupported.

---

## 4. Finite-bit lower bound and rate–distortion reasoning

For a discrete message `M` transmitted over a noiseless channel with at most `b` bits,

\[
|\mathcal M|\le 2^b,
\qquad
H(M)\le b.
\]

For a general source `X` and reconstruction `\hat X`, the rate–distortion function is

\[
R(D)=\inf_{p(\hat x|x):\;\mathbb E[d(X,\hat X)]\le D} I(X;\hat X).
\]

A code operating below the relevant asymptotic rate cannot achieve the target distortion under the theorem's source/channel assumptions.

For the Holobiont this yields a useful experimental rule:

\[
B < R_{X\to T}(D)
\]

should be expected to impose a task-information bottleneck when `T` is the target-relevant variable and the distortion criterion is appropriate.

But `R(D)` is not automatically the right quantity for neural decision systems. If the receiver only needs a label or action, reconstruction distortion may be a poor metric. The benchmark should therefore use **task distortion**

\[
d_T(x,\hat t)
\]

or excess risk rather than pixel/text reconstruction error whenever possible.

The communication-complexity literature independently establishes that distributed learning can require nontrivial communication even when local computation is cheap. See Balcan et al.:

https://arxiv.org/abs/1204.3514

A newer distributed-estimation treatment explicitly relates estimation error and communication complexity:

https://machinelearning.apple.com/research/communication-complexity

### Treatise correction

Any statement of the form

\[
\text{lower bandwidth} \Rightarrow \text{better cognition}
\]

is **mathematically unsupported**. Lower bandwidth is only an efficiency property; utility can decrease below a critical rate.

---

## 5. Identifiability of shared latent spaces

### Established limitation

Latent coordinates generally have symmetries. Without additional assumptions, different parameterizations can represent the same observable distribution/function.

The nonlinear ICA literature demonstrates that identifiability can be recovered only under specific structural assumptions. See Hyvärinen & Morioka (2017):

https://proceedings.mlr.press/v54/hyvarinen17a.html

and the later identifiability treatment by Hyvärinen, Khemakhem & Monti:

https://doi.org/10.1007/s10463-023-00884-4

For multi-view/partial-observation settings, Yao et al. (ICLR 2024) derive conditions under which shared latent factors can be identifiable up to transformations under their model assumptions:

https://openreview.net/forum?id=6YpW4G8L1j

### Consequence for Holobiont

The statement

\[
z_i=z_j \Rightarrow \text{same meaning}
\]

is invalid without a semantic/task definition and an identifiability assumption.

Likewise, minimizing

\[
\|z_i-z_j\|^2
\]

can encourage trivial alignment. A bridge objective needs a task-preservation term, anti-collapse mechanism, or an equivalent constraint.

A safer criterion is **functional equivalence**:

\[
\mathbb E_{x\sim D}
[d_T(g_i(z_i(x)),g_j(z_j(x)))]\le\epsilon.
\]

This tests whether the receiver obtains task-relevant behavior, not merely coordinate agreement.

---

## 6. Mutual-information estimation is not a free measurement tool

The Treatise should not report neural mutual-information estimates as ground truth without estimator analysis. Finite-sample MI estimation can be severely biased, especially in high-dimensional neural populations. See Mölter & Goodhill (2020):

https://www.mdpi.com/1099-4300/22/4/490

Therefore the benchmark should prefer quantities with direct operational definitions:

- predictive log loss,
- accuracy/F1 where appropriate,
- calibration error,
- conditional task risk,
- communication bits,
- effective rank,
- reconstruction distortion when reconstruction is actually the task,
- intervention-based utility.

MI can be a secondary diagnostic with estimator sensitivity analysis.

---

## 7. Workspace capacity and information bottlenecks

A fixed workspace

\[
w_t\in\mathbb R^{K\times d_w}
\]

is a finite computational bottleneck. Perceiver-style architectures establish that learned fixed-size latent arrays can mediate large input sets:

https://arxiv.org/abs/2103.03206

Global-workspace-inspired multimodal networks also provide computational precedents for modality-specific processing followed by shared workspace processing. Bao et al. (2020):

https://arxiv.org/abs/2001.09485

Devillers et al. provide a more explicit computational Global Workspace architecture:

https://arxiv.org/abs/2306.15711

### Information-capacity caution

If the workspace is represented with `n=K d_w` real-valued degrees of freedom, it is incorrect to claim a simple finite Shannon capacity from `n` alone: real-valued activations have unbounded differential entropy unless noise, quantization, amplitude, or a coding model is specified.

A valid channel capacity requires a channel/noise model. For example, an additive Gaussian channel with power constraint `P` and noise variance `N` has per-real-dimension capacity

\[
C=\frac12\log_2(1+P/N)
\]

bits/use.

Thus the Treatise must specify **quantization/noise/range/precision** before translating workspace dimensions into an information budget.

---

## 8. Routing: stability and utility

Let the router output

\[
p_i(s)=\frac{e^{a_i(s)}}{\sum_j e^{a_j(s)}}.
\]

For a small perturbation `\Delta a`, the softmax Jacobian is

\[
J_{ij}=p_i(\delta_{ij}-p_j).
\]

Hence

\[
\|\Delta p\|_2
\le
\|J\|_2\|\Delta a\|_2+o(\|\Delta a\|_2).
\]

This establishes local sensitivity but **not** global router stability. A router can oscillate if its inputs depend on rapidly changing expert states or delayed reliability estimates.

The benchmark therefore needs:

- routing churn,
- expert utilization,
- load variance,
- regret/excess risk,
- latency,
- compute,
- sensitivity to perturbations in health estimates.

### Load balancing is not universally optimal

Switch Transformer demonstrates sparse conditional computation and routing/load-management tradeoffs:

https://jmlr.org/papers/v23/21-0998.html

Expert Choice Routing changes the assignment direction and reports improvements under its own experimental setup:

https://arxiv.org/abs/2202.09368

Therefore the Treatise's routing layer should be compared against multiple routing regimes rather than hard-coding “uniform utilization”.

---

## 9. A useful utility-per-bit quantity

Define a no-communication baseline risk `R_0` and communicated risk `R(B)`. Then define

\[
\eta(B)=\frac{R_0-R(B)}{B}.
\]

This is an operational measure of **risk reduction per transmitted bit**.

It is not an information-theoretic invariant and should not be interpreted as one. It is useful for comparing learned bridges at equal bandwidth.

A more complete frontier is

\[
(R, B, C, L, M, Q),
\]

where `Q` contains reliability metrics.

The correct scientific question becomes:

> Does the Holobiont dominate matched-capacity baselines on this Pareto frontier?

rather than:

> Does communication make the system smarter?

---

## 10. Memory as an information channel

Let episodic memory write `c_t` and retrieval produce `r_t`:

\[
M_{t+1}=W(M_t,c_t),
\qquad
r_t=\operatorname{Retrieve}(M_t,q_t).
\]

A finite memory cannot guarantee exact retention of an arbitrary unbounded stream unless the future task distribution imposes compressibility/relevance structure.

For a task family `T`, define memory sufficiency operationally by

\[
R_T(M)\le R_T^*+\epsilon.
\]

This is preferable to claiming “permanent memory”.

The Treatise should also measure stale-memory harm:

\[
\Delta_{stale}
=
R(D\mid M_{stale})-R(D\mid M_{fresh}).
\]

Retrieval similarity is not truth. Provenance, validation, contradiction handling and versioning remain necessary engineering controls.

---

## 11. Continual learning and shared interfaces

For tasks `1,...,T`, define

\[
R_{k,t}=\mathbb E_{(x,y)\sim D_k}[\ell(f_t(x),y)].
\]

Forgetting after learning task `t` can be measured as

\[
F_{k,t}=R_{k,t}-R_{k,k}.
\]

But in the Holobiont there are at least four possible interference locations:

1. specialist parameters `\theta_i`,
2. bridge parameters `\phi_i`,
3. router/workspace parameters `\psi`,
4. persistent memory `M`.

A system can therefore have low specialist forgetting while still experiencing global performance degradation due to bridge/router drift.

The benchmark must freeze three components while changing one to identify the causal source of degradation.

---

## 12. Gradient conflict

For objectives `L_i`, define

\[
g_i=\nabla_\theta L_i.
\]

A pairwise conflict exists when

\[
g_i^\top g_j<0.
\]

For the average gradient

\[
g=\sum_i \alpha_i g_i,
\qquad \alpha_i\ge0,\quad \sum_i\alpha_i=1,
\]

negative pairwise inner products can cause an update to increase one task's local loss.

This is established as an optimization issue in multi-task learning; conflict-averse methods explicitly address it. See Liu et al. (2021):

https://arxiv.org/abs/2110.14048

### Holobiont implication

The claim that modular organs automatically eliminate interference is too strong. They can **localize** interference, but shared bridge/workspace/router parameters can reintroduce it.

---

## 13. A more rigorous decomposition of “cognitive efficiency”

Do not use a single quantity such as

\[
\text{Cognitive Efficiency}=\frac{\text{intelligence}}{\text{compute}}.
\]

“Intelligence” is not a mathematically defined scalar in the Treatise.

Instead report

\[
\mathbf U
=
(-R_D,\,-B,\,-C,\,-L,\,-M,\,Q),
\]

and test Pareto dominance.

If a scalar is required for a preregistered experiment, define it before data collection and justify every normalization. Otherwise scalarization can hide regressions in safety or reliability.

---

## 14. Claim classification update

### (1) Established result

- Data-processing inequality.
- Rate–distortion theory under its source/channel assumptions.
- Information Bottleneck as a formal compression/predictiveness objective.
- Finite communication limits in distributed estimation/learning.
- Nonlinear/multi-view representation identifiability under explicit assumptions.
- Sparse conditional computation and MoE routing mechanisms.
- Gradient conflict as a real optimization phenomenon.
- Fixed-size latent bottlenecks are viable architectures.

### (2) Plausible engineering synthesis

- Typed latent bridges between specialized organs.
- A bounded workspace as an integration/broadcast mechanism.
- Routing optimized over task utility plus compute/bandwidth/risk constraints.
- Versioned memory with provenance and validation.
- Combining learned communication with modular experts and reliability mechanisms.
- Measuring the whole system through a Pareto frontier.

### (3) Unsupported/speculative

- A shared latent space is a universal semantic language.
- Workspace integration necessarily creates consciousness or general intelligence.
- More inter-organ communication monotonically increases intelligence.
- Arbitrary specialist state can be regenerated from compact artifacts.
- Finite memory can preserve unlimited exact knowledge.
- Router consensus guarantees correctness.
- Anti-fragility follows automatically from modular recovery.

### (4) Mathematically incorrect/incomplete unless repaired

- `\|z_i-z_j\|` interpreted as semantic equivalence without a defined metric/task.
- KL divergence applied directly to arbitrary deterministic hidden vectors.
- Workspace dimension treated as Shannon capacity without a channel/noise/quantization model.
- Spectral gap equated with cognitive speed without a dynamical model linking them.
- Router entropy equated with health.
- MI neural estimates treated as ground truth in high dimensions.
- Parameter/FLOP sparsity converted directly into wall-clock speedup.
- Regeneration percentage stated without a behavioral distance or task metric.

---

## 15. Experiment hypotheses H42–H48

### H42 — Task-oriented rate–distortion frontier

At matched specialist backbones, a task-oriented bridge should dominate raw feature transmission at some intermediate bandwidth range.

**Falsifier:** raw communication matches or dominates the learned bridge across the preregistered bandwidth range.

### H43 — Alignment/task tension

Adding a pure alignment objective without task preservation will produce at least one setting with lower latent distance but worse downstream risk.

**Falsifier:** no such degradation appears across all tested seeds/tasks.

### H44 — Workspace saturation

Performance will exhibit diminishing returns as workspace capacity `K d_w` increases.

**Falsifier:** performance continues increasing approximately linearly over the entire tested capacity range without a measurable knee.

### H45 — Communication phase transition

There exists a task-dependent bandwidth region below which performance degrades sharply.

**Falsifier:** no statistically meaningful performance transition is observed.

### H46 — Router stability/utility tradeoff

Aggressive adaptive routing will improve resource efficiency in some regimes but increase routing churn or instability in others.

**Falsifier:** adaptive routing improves or matches all primary metrics without increased instability.

### H47 — Localized vs shared interference

Specialist isolation will reduce within-specialist forgetting, while shared bridge/workspace/router training will account for a measurable fraction of residual forgetting.

**Falsifier:** freezing shared interfaces produces no meaningful reduction in forgetting.

### H48 — Memory sufficiency, not memory size

A provenance-gated, task-relevant memory policy will outperform an equal-size nearest-neighbor memory store on long-horizon tasks at matched retrieval cost.

**Falsifier:** simple similarity retrieval matches or dominates the gated policy.

---

## 16. Preregistration requirements added

Before B0–B3 implementation, freeze:

- datasets and splits;
- specialist roles;
- model checkpoints/versions;
- random seeds and number of independent runs;
- bridge architecture;
- quantization/coding method;
- workspace dimensions `K,d_w`;
- router architecture and temperature;
- memory capacity and retrieval budget;
- optimizer and training schedule;
- exact communication accounting;
- exact compute accounting;
- primary/secondary metrics;
- confidence intervals/statistical tests;
- exclusion criteria;
- failure-handling rules;
- all baselines and ablations.

No post-hoc selection of the most favorable bandwidth, capacity, routing temperature, memory size, or fault setting should be reported as the primary result.

---

## 17. Open problems

1. Derive task-specific communication lower bounds for heterogeneous multimodal specialists.
2. Determine when a learned shared latent is identifiable enough for cross-organ transfer.
3. Quantify the utility loss caused by workspace bottlenecks under realistic noise/quantization.
4. Establish stability conditions for routers coupled to endogenous reliability estimates.
5. Characterize when modularity reduces catastrophic forgetting versus merely moving interference into shared parameters.
6. Develop memory sufficiency metrics that distinguish retrieval relevance from factual correctness.
7. Obtain recovery guarantees that connect artifact information content to behavioral reconstruction error.
8. Establish end-to-end bounds under simultaneous communication loss, memory corruption, routing errors, and specialist failure.

---

## 18. Decision gate

**Do not proceed directly to the complete self-healing/evolutionary Holobiont.**

The next implementation, when undertaken, should be limited to B0–B3 and must answer three questions:

1. Is learned inter-organ communication better than raw communication at equal bandwidth?
2. Does a bounded workspace provide utility beyond direct communication at equal compute/parameter budgets?
3. Does structured memory provide long-horizon benefit beyond an equal-cost retrieval baseline?

If the answer to these is negative, the Treatise should be revised before adding recovery, Byzantine consensus, regeneration, or evolutionary mechanisms.

---

## 19. Source ledger

Primary/open-access anchors consulted for this milestone:

- Tishby, Pereira & Bialek, **The Information Bottleneck Method** (2000): https://arxiv.org/abs/physics/0004057
- Alemi et al., **Deep Variational Information Bottleneck** (2016): https://arxiv.org/abs/1612.00410
- Alemi et al., **Fixing a Broken ELBO** (2018): https://arxiv.org/abs/1711.00464
- Shao, Mao & Zhang, **Learning Task-Oriented Communication for Edge Inference: An Information Bottleneck Approach**: https://arxiv.org/abs/2102.04170
- Balcan et al., **Distributed Learning, Communication Complexity and Privacy**: https://arxiv.org/abs/1204.3514
- Mölter & Goodhill, **Limitations to Estimating Mutual Information in Large Neural Populations** (2020): https://www.mdpi.com/1099-4300/22/4/490
- Hyvärinen & Morioka, **Nonlinear ICA of Temporally Dependent Stationary Sources** (2017): https://proceedings.mlr.press/v54/hyvarinen17a.html
- Hyvärinen, Khemakhem & Monti, **Identifiability of latent-variable and structural-equation models: from linear to nonlinear** (2023): https://doi.org/10.1007/s10463-023-00884-4
- Yao et al., **Multi-view causal representation learning with partial observability** (ICLR 2024): https://openreview.net/forum?id=6YpW4G8L1j
- Jaegle et al., **Perceiver: General Perception with Iterative Attention** (2021): https://arxiv.org/abs/2103.03206
- Bao et al., **Multimodal Data Fusion based on the Global Workspace Theory** (2020): https://arxiv.org/abs/2001.09485
- Devillers et al., **A Global Workspace for Multimodal Agents** / computational global workspace work: https://arxiv.org/abs/2306.15711
- Fedus, Zoph & Shazeer, **Switch Transformers**: https://jmlr.org/papers/v23/21-0998.html
- Zhou et al., **Mixture-of-Experts with Expert Choice Routing**: https://arxiv.org/abs/2202.09368
- Liu et al., **Conflict-Averse Gradient Descent for Multi-task Learning**: https://arxiv.org/abs/2110.14048
- Gopalan et al., **The Communication Complexity of Distributed Estimation** (2025): https://machinelearning.apple.com/research/communication-complexity

## 20. Provenance note

This milestone is a research synthesis. Literature findings are not being presented as validation of the Cognitive Holobiont. Where a source establishes only a component property, the dossier records only that component property. The composition, system-level utility, consciousness claims, universal regeneration, immortality, anti-fragility, and AGI-level emergence remain empirical/speculative questions.
