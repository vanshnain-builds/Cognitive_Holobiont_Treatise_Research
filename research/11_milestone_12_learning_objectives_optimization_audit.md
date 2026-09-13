# Milestone 12 — Learning Objectives, Optimization Dynamics, and Capacity-Matched Baselines

**Date:** 2026-09-13  
**Status:** Pre-implementation mathematical specification  
**Purpose:** Remove remaining ambiguity in B0–B3, identify ill-posed objectives and degenerate optima, and define capacity-matched controls before coding.

---

## 0. Executive finding

The previous milestones established that the proposed Holobiont components have substantial precedent, but the composition is not yet validated. Milestone 12 focuses on the most dangerous remaining failure mode: an objective can be mathematically well-defined while optimizing the wrong property, or can have trivial/degenerate solutions.

The central conclusion is:

> **B0–B3 should be treated as a constrained multi-objective optimization problem with explicit utility, information, compute, bandwidth, calibration, and interference measurements. A single scalar loss must not be allowed to hide a system-level regression.**

The minimal optimization stack is:

`specialists → typed bridges → bounded workspace → selective memory → router`.

Fault injection, regeneration, Byzantine behavior, and self-modification remain downstream experiments.

---

# 1. Claim classification used in this milestone

| Class | Meaning | Holobiont interpretation |
|---|---|---|
| 1. Established | Supported by reproducible prior research under stated assumptions | Sparse conditional computation, attention/cross-attention, contrastive representation learning, replay/regularization families, retrieval memory, etc. |
| 2. Plausible engineering synthesis | Reasonable composition of established mechanisms, but system-level benefit is unproven | Typed latent bridges + workspace + routing + memory |
| 3. Unsupported/speculative | No adequate evidence for the Treatise's stronger claim | Universal regeneration, consciousness, immortality, spontaneous AGI evolution |
| 4. Mathematically incorrect/incomplete | Ill-typed, missing assumptions, or stronger than the derivation permits | Unqualified KL between hidden vectors; universal spectral-gap=cognition claim; lossless regeneration without an information assumption |

A claim may move from 2 to 1 only after evidence under the exact proposed setting. A successful component experiment does not validate the complete Treatise.

---

# 2. Formal system for B0–B3

Let there be `m` specialist organs. Specialist `i` receives modality/task input `x_i` and produces a hidden representation

\[
h_i=f_i(x_i;\theta_i)\in\mathbb R^{d_i}.
\]

A typed bridge maps it into a workspace-compatible space:

\[
z_i=P_i(h_i;\phi_i)\in\mathbb R^{d_w}.
\]

The workspace has `K` slots/tokens:

\[
w_t\in\mathbb R^{K\times d_w}.
\]

The workspace update is a learned function

\[
w_{t+1}=F_\psi(w_t,\mathcal Z_t,u_t),
\]

where \(\mathcal Z_t\) is the selected set of specialist messages and \(u_t\) is optional control/state information.

A specialist decoder or readout receives the workspace through

\[
\hat h_j=D_j(w_t;\omega_j),
\]

and produces task output

\[
\hat y_j=g_j(\hat h_j;\theta_j^o).
\]

The router outputs a distribution

\[
p(r_t=i\mid s_t)=\operatorname{softmax}(a(s_t))_i,
\]

where \(s_t\) includes the current query and reliability/state features.

Memory is an external state

\[
M_t=\operatorname{Commit}(M_{t-1},c_t)
\]

and retrieval returns

\[
C_t=\operatorname{Retrieve}(M_t,q_t;k).
\]

Crucially, retrieval is an information source, not a truth certificate.

---

# 3. B0 — specialist baseline

## 3.1 Objective

For specialist tasks \(j=1,\ldots,m\), define

\[
\mathcal L_{B0}(\theta)=\sum_j\alpha_j\,\mathbb E_{(x,y)\sim D_j}[\ell_j(f_j(x;\theta_j),y)].
\]

The weights \(\alpha_j>0\) must be fixed before evaluation.

B0 must include:

1. independent specialists;
2. a dense shared-capacity baseline where applicable;
3. parameter-count and active-FLOP accounting;
4. identical train/validation/test splits across comparisons;
5. multiple random seeds.

## 3.2 Why B0 matters

Without B0, an apparent Holobiont gain may simply be a gain from increased parameters, additional training, or a stronger backbone.

The baseline comparison must therefore report both:

\[
\text{quality per active FLOP}
\]

and

\[
\text{quality per total parameter / memory footprint}.
\]

No fixed percentage compute saving should be assumed. Sparse architectures can reduce arithmetic while introducing communication, dispatch, synchronization, and capacity-overflow costs. Switch Transformer explicitly identifies communication and training instability as practical issues in sparse MoE systems. [Fedus, Zoph & Shazeer, 2022](https://jmlr.org/papers/v23/21-0998.html)

---

# 4. B1 — learned latent bridge

## 4.1 Bridge objective

For paired observations from two organs `i,j`, a contrastive alignment objective can be written

\[
\mathcal L_{\mathrm{NCE}}
=-\frac1N\sum_{n=1}^{N}
\log
\frac{\exp(s(z_i^{(n)},z_j^{(n)})/\tau)}
{\sum_{r=1}^{N}\exp(s(z_i^{(n)},z_j^{(r)})/\tau)}.
\]

For normalized embeddings, a common choice is

\[
s(z,z')=\frac{z^\top z'}{\|z\|\|z'\|}.
\]

The temperature \(\tau>0\) is a hyperparameter and must be tuned using training/validation data only.

## 4.2 Degenerate-solution audit

Alignment alone is insufficient. A constant representation \(z_i=c\) can make pairwise agreement high while destroying task information. Therefore B1 must contain a task-preservation term:

\[
\mathcal L_{B1}
=
\mathcal L_{\mathrm{task}}
+\lambda_a\mathcal L_{\mathrm{NCE}}
+\lambda_v\mathcal L_{\mathrm{var}}.
\]

A variance/covariance regularizer is one possible anti-collapse mechanism; the exact choice should be treated as an experimental factor rather than assumed optimal.

The key acceptance test is not latent cosine similarity. It is **held-out downstream utility under fixed communication budget**.

## 4.3 Information-bottleneck interpretation

If the bridge is intentionally compressed, one may add

\[
\beta I(H_i;Z_i)
\]

to the conceptual objective, but mutual information is generally difficult to estimate reliably in high-dimensional neural systems. A variational approximation can instead produce an implementable surrogate. Alemi et al. established the Deep Variational Information Bottleneck framework; it does not prove that every compressed latent is semantically sufficient. [Alemi et al., 2016](https://arxiv.org/abs/1612.00410)

Thus the Treatise's strongest safe statement is:

> A bottleneck may improve communication efficiency if it preserves task-relevant information; the preservation must be measured, not inferred from compression.

---

# 5. B2 — bounded workspace

## 5.1 Workspace update

A simple attention-based workspace can be represented as

\[
Q=W_Qw_t,
\qquad K_i=W_Kz_i,
\qquad V_i=W_Vz_i,
\]

with attention

\[
A_i=\operatorname{softmax}\left(\frac{QK_i^\top}{\sqrt{d_k}}\right),
\]

and update

\[
w_{t+1}=\operatorname{Norm}\left(w_t+\sum_i A_iV_i\right).
\]

The exact transformer parameterization is not essential; what matters experimentally is that the workspace has finite capacity `K` and finite message budget.

Perceiver-style architectures provide a strong precedent for a learned fixed-size latent bottleneck that interacts with larger inputs. This supports feasibility of a bounded latent workspace, not its cognitive necessity. [Jaegle et al., 2021](https://arxiv.org/abs/2103.03206)

## 5.2 Capacity constraint

Define a message budget

\[
B_t=\sum_i b_i(t)
\]

where \(b_i(t)\) is the number of transmitted scalars/tokens/bits after the actual encoding and transport representation is specified.

A bounded-workspace experiment must report utility as a function of both `K` and `B`:

\[
U(K,B).
\]

The claim “small workspace is sufficient” is supported only if utility saturates within a demonstrably small region while the no-workspace and larger-workspace controls are included.

## 5.3 No monotonicity assumption

It is not safe to assume

\[
U(K+1,B)\ge U(K,B)
\]

for a learned finite-sample system. More capacity can increase optimization difficulty, interference, overfitting, or routing instability. The experiment must measure the curve rather than impose monotonicity.

---

# 6. B3 — routing objective

## 6.1 Constrained formulation

Let `U` be expected task utility, `C` active compute, `B` communication, and `R` a risk/reliability cost. The cleanest formulation is

\[
\max_\pi\;\mathbb E[U(\pi)]
\]

subject to

\[
\mathbb E[C(\pi)]\le C_{\max},
\qquad
\mathbb E[B(\pi)]\le B_{\max},
\qquad
\mathbb E[R(\pi)]\le R_{\max}.
\]

The equivalent Lagrangian is

\[
\mathcal J(\pi)
=
-\mathbb E[U]
+\lambda_C\mathbb E[C]
+\lambda_B\mathbb E[B]
+\lambda_R\mathbb E[R].
\]

The multipliers must be treated as optimization variables or experimentally fixed constants; they cannot be silently changed between baselines.

## 6.2 Load balancing

For `N` routed items and `E` experts/organs, let

\[
n_e=\sum_{t=1}^{N}\mathbf 1[r_t=e].
\]

A simple imbalance measure is

\[
\mathrm{CV}_n
=
\frac{\operatorname{std}(n_1,\ldots,n_E)}
{\operatorname{mean}(n_1,\ldots,n_E)}.
\]

But uniform routing is not necessarily optimal. Expert Choice Routing explicitly allows variable token-to-expert assignment while fixing expert capacity, illustrating that balancing and utility can be jointly designed. [Zhou et al., 2022](https://arxiv.org/abs/2202.09368)

Therefore the Treatise should **not** encode “uniform organ usage” as a universal objective. The desired property is sufficient capacity and specialization without pathological collapse.

Switch Transformer similarly demonstrates sparse conditional computation with routing and capacity constraints, while documenting training instability and communication overhead as practical concerns. [Fedus et al., 2022](https://jmlr.org/papers/v23/21-0998.html)

## 6.3 Router entropy is not health

Define router entropy

\[
H(r_t)=-\sum_i p_i\log p_i.
\]

Low entropy can indicate useful specialization or catastrophic collapse. High entropy can indicate uncertainty or wasteful indiscriminate routing. Therefore

\[
H(r_t)\not\equiv \text{health}.
\]

Routing statistics must be interpreted jointly with task utility, expert load, calibration, and failure outcomes.

---

# 7. Memory objective

Memory should not be optimized merely for maximum retrieval similarity.

Let a memory record be

\[
c=(k,v,s,p,t,e),
\]

where `k` is a key, `v` content, `s` source/provenance, `p` permission/policy metadata, `t` timestamp/version, and `e` evidence metadata.

A write gate can be represented as

\[
g(c)=\mathbf 1[
q(c)\ge q_{\min}
\land
\operatorname{Auth}(c)=1
\land
\operatorname{Prov}(c)\ge p_{\min}
].
\]

This is a policy mechanism, not a correctness theorem.

Retrieval score may be

\[
S(q,c)=\operatorname{sim}(q,k_c)+\lambda_pP(c)+\lambda_rR(c)-\lambda_s\operatorname{Stale}(c),
\]

but source reliability must not be assumed from similarity alone. Reliability-aware RAG work explicitly studies heterogeneous source reliability. [Hwang et al., 2024](https://arxiv.org/abs/2410.22954)

Recent agent-memory security work also shows that persistent memory can become a durable attack surface, including poisoning through normal interaction/environmental observation. [Zou et al., 2026](https://arxiv.org/abs/2604.02623); [Gao et al., 2026](https://arxiv.org/abs/2607.14651)

---

# 8. Continual learning objective

For task sequence \(D_1,\ldots,D_T\), define

\[
R_{k,t}=\mathbb E_{(x,y)\sim D_k}[\ell(f_t(x),y)].
\]

A forgetting matrix is

\[
F_{k,t}=R_{k,t}-R_{k,k},\qquad t>k.
\]

Average forgetting over tasks can be reported as

\[
\bar F_T
=\frac1{T-1}\sum_{k=1}^{T-1}F_{k,T}.
\]

The Holobiont claim is not “modularity prevents forgetting.” A safer hypothesis is:

> Isolating task-specialized parameters may reduce some forms of gradient interference, but shared bridges, workspace parameters, router parameters, and memory can still forget or interfere.

Multi-task optimization literature provides direct evidence that conflicting gradients can make joint optimization worse than independent learning in some settings. [Liu et al., 2021](https://arxiv.org/abs/2110.14048)

EWC supplies a useful regularization baseline:

\[
\mathcal L(\theta)
=
\mathcal L_{new}(\theta)
+
\frac\lambda2\sum_iF_i(\theta_i-\theta_i^*)^2.
\]

This is a local parameter-stability mechanism, not a global theorem of no forgetting.

---

# 9. Information budget and regeneration

For a surviving artifact `S`, a regenerated specialist \(\hat f\) is useful only if

\[
\mathbb E_{x\sim D_{eval}}
[d(f(x),\hat f(x))]\le \epsilon
\]

or, for task loss,

\[
R_D(\hat f)-R_D(f)\le\epsilon_R.
\]

The required information depends on the hypothesis class, task distribution, training data, architecture, and recovery target.

There is no general theorem that a finite checkpoint, latent summary, or hypernetwork can recover an arbitrary destroyed model to arbitrary accuracy.

The correct experiment therefore varies artifact budget:

\[
I\in\{I_1,I_2,\ldots,I_L\}
\]

and estimates

\[
\epsilon_R(I).
\]

A regeneration claim becomes empirical only if the recovery curve is reproducible and compared with simpler checkpoint/replica/teacher baselines.

HyperNetworks establish that one network can generate another network's weights, but that mechanism alone does not establish behavioral recovery after arbitrary information loss. [Ha, Dai & Le, 2016](https://arxiv.org/abs/1609.09106)

---

# 10. Capacity-matched baseline construction

Every B1–B3 result must have controls that match as closely as possible:

- training data;
- optimizer family;
- number of training steps/tokens;
- parameter count;
- active parameters per inference step;
- activation memory;
- communication volume;
- wall-clock hardware;
- random-seed count;
- evaluation budget.

At minimum compare:

| System | Purpose |
|---|---|
| Dense monolith | Upper/simple shared baseline |
| Independent specialists | Tests whether decomposition itself helps |
| Specialists + raw bridge | Tests communication without learned alignment |
| Specialists + learned bridge | Tests bridge value |
| Specialists + workspace | Tests integration value |
| Specialists + router | Tests selective computation |
| Full B0–B3 | Tests composition |

A claim of improvement is only meaningful against the closest matched control.

---

# 11. Optimization diagnostics

For each trainable subsystem, record gradient statistics:

\[
g_a=\nabla_\theta L_a,
\qquad
g_b=\nabla_\theta L_b.
\]

The cosine conflict statistic is

\[
\rho_{ab}
=
\frac{g_a^\top g_b}{\|g_a\|\|g_b\|}.
\]

If \(\rho_{ab}<0\), the objectives locally conflict. This does not prove that the tasks are globally incompatible, but it identifies an optimization conflict at that update.

For the Holobiont, measure this separately for:

- specialist task loss vs bridge loss;
- task loss vs workspace loss;
- task loss vs routing regularizer;
- task loss vs memory-write objective.

Do not assume that gradient orthogonality means semantic independence.

---

# 12. Stability of the learned state

The full state can be represented as

\[
s_t=(\theta_t,\phi_t,\psi_t,\pi_t,M_t).
\]

A generic update is

\[
s_{t+1}=G(s_t,\xi_t),
\]

where \(\xi_t\) denotes stochastic inputs.

A stability claim requires an explicit metric `d` and a contraction/non-expansion property, for example

\[
\mathbb E[d(G(s,\xi),G(s',\xi))]
\leq \kappa d(s,s'),
\qquad \kappa<1.
\]

Without such assumptions, calling the architecture “self-stabilizing” is speculative.

Likewise, a Lyapunov function \(V(s)\) must satisfy a stated decrease condition such as

\[
\mathbb E[V(s_{t+1})\mid s_t]-V(s_t)\le -c\,W(s_t)+b
\]

for explicit nonnegative `W` and constants `c>0,b\ge0`. Merely writing down a positive scalar health function is not a Lyapunov proof.

---

# 13. Consensus: restrict the theorem to the variable being averaged

For a connected undirected graph with Laplacian `L`, standard continuous consensus is

\[
\dot x=-Lx.
\]

Writing

\[
x(t)-\bar x\mathbf 1=e^{-Lt}(x(0)-\bar x\mathbf1)
\]

and using the second-smallest eigenvalue \(\lambda_2(L)>0\),

\[
\|x(t)-\bar x\mathbf1\|_2
\le
e^{-\lambda_2(L)t}
\|x(0)-\bar x\mathbf1\|_2.
\]

This yields

\[
t_\epsilon
\le
\frac1{\lambda_2(L)}
\log\frac{\|x(0)-\bar x\mathbf1\|_2}{\epsilon}
\]

for the disagreement tolerance `epsilon`.

This theorem concerns **consensus error in a specified state variable**. It does not imply cognitive speed, intelligence, correctness, or faster reasoning.

For directed, switching, delayed, nonlinear, or stochastic graphs, additional assumptions are required. The Treatise must never silently transfer the undirected fixed-graph result to those systems.

---

# 14. What is established vs synthesis vs speculation

## Established (Class 1)

- Sparse MoE can increase total parameter capacity while activating only a subset per input.
- Routing/load balancing and capacity constraints materially affect training and inference.
- Learned latent representations can align paired/multiview data under suitable objectives.
- Fixed-size latent bottlenecks are practical architectural devices.
- Retrieval can extend usable external knowledge beyond a model's native context.
- Continual learning has measurable forgetting/interference and established baseline families.
- Gradient conflict is a real optimization phenomenon.
- Standard consensus has spectral convergence results under explicit graph assumptions.
- Robust federated methods can have guarantees only under specified threat/data assumptions.

## Plausible engineering synthesis (Class 2)

- Typed bridges between heterogeneous specialists.
- A finite workspace as a selective inter-organ communication bus.
- Reliability-aware routing combining task utility and uncertainty/fault signals.
- Selective versioned memory integrated with workspace state.
- Hierarchical recovery using replicas/checkpoints before reconstruction.
- Joint optimization with explicit communication and compute budgets.

## Unsupported/speculative (Class 3)

- A shared workspace is sufficient or necessary for general intelligence.
- Inter-organ communication necessarily creates emergent cognition.
- A finite memory system can preserve arbitrary indefinite experience exactly.
- A hypernetwork can reconstruct any destroyed specialist from finite surviving state.
- Self-repair implies immortality or indefinite availability.
- Faults or adversarial attacks necessarily improve the system (“anti-fragility”).
- Evolutionary/self-modifying AI necessarily produces AGI or consciousness.

## Mathematically incorrect/incomplete (Class 4)

- Applying KL divergence directly to arbitrary hidden vectors.
- Equating consensus spectral gap with cognition speed without a dynamical model connecting them.
- Treating attention weights as causal importance without interventions.
- Claiming lossless regeneration without specifying information sufficiency and a target distribution/behavioral metric.
- Treating router entropy, confidence, or disagreement as interchangeable definitions of health.
- Claiming compute/latency savings from parameter sparsity alone while omitting communication and systems overhead.
- Treating retrieval similarity as a correctness certificate.

---

# 15. Experiment hypotheses H35–H41

### H35 — learned bridges
At matched bandwidth and backbone capacity, learned bridges improve cross-specialist transfer over raw hidden-state transmission.

**Falsifier:** no improvement or a matched raw bridge is equal/better across predefined tasks.

### H36 — anti-collapse
A bridge objective containing task preservation avoids the degenerate utility loss produced by alignment-only training.

**Falsifier:** alignment-only is equally useful under held-out transfer and information-preservation metrics.

### H37 — bounded workspace
There exists a finite workspace capacity `K*` beyond which additional workspace capacity provides no statistically significant improvement under fixed compute/bandwidth.

**Falsifier:** utility continues increasing materially throughout the tested capacity range or the result is unstable across seeds/tasks.

### H38 — adaptive routing
Under matched active compute, learned routing improves the utility/compute/bandwidth Pareto frontier relative to fixed routing.

**Falsifier:** fixed routing matches or beats adaptive routing after overhead accounting.

### H39 — modular continual learning
Specialist isolation reduces forgetting relative to a matched monolithic learner, without unacceptable degradation in transfer.

**Falsifier:** no reduction in forgetting or the gain disappears after capacity/training-budget matching.

### H40 — selective memory
Validation/provenance-gated memory improves long-horizon task reliability relative to relevance-only retrieval at matched storage and retrieval cost.

**Falsifier:** no reliability improvement, or gating causes larger utility loss than the reliability gain.

### H41 — optimization conflict
Shared bridge/workspace/router parameters exhibit measurable gradient conflict that predicts some instances of negative transfer or instability.

**Falsifier:** conflict statistics have no relationship to measured degradation after appropriate controls.

---

# 16. Required metrics before implementation claims

For every B0–B3 experiment report:

### Utility
- task accuracy/F1 or task-appropriate loss;
- transfer performance;
- worst-task performance;
- average performance.

### Efficiency
- active FLOPs;
- total parameter count;
- activation memory;
- communication bytes/tokens;
- wall-clock latency p50/p95/p99;
- energy if measurable.

### Representation
- effective rank;
- cross-organ alignment;
- task linear-probe performance;
- robustness under perturbation;
- information retained under compression.

### Reliability
- ECE/Brier or task-appropriate calibration metric;
- selective risk/coverage;
- OOD detection AUROC/AUPR/FPR95 where appropriate;
- disagreement statistics;
- router drift.

### Continual learning
- average accuracy;
- backward transfer;
- forward transfer;
- forgetting matrix;
- memory growth.

### Statistical discipline
- predeclared primary metric;
- predeclared seed count;
- confidence intervals;
- paired comparisons where possible;
- correction for multiple comparisons when many hypotheses are tested;
- report negative results.

---

# 17. Open mathematical problems

1. **Interface sufficiency:** characterize conditions under which a learned bridge preserves the task-relevant sigma-algebra of a specialist.
2. **Workspace capacity:** derive useful upper/lower bounds linking finite latent capacity to task-relevant mutual information or decision risk.
3. **Router dynamics:** analyze stability when routing changes the data distribution seen by each specialist.
4. **Joint learning:** characterize when optimizing \(L_{task}+\lambda L_{bridge}+\mu L_{route}\) creates negative transfer.
5. **Memory consistency:** formalize stale and contradictory memory as state-estimation uncertainty rather than a binary truth label.
6. **Recovery information:** derive lower bounds on artifact bits required for behavioral recovery under a defined hypothesis class.
7. **Fault-aware inference:** connect uncertainty estimates to safe routing decisions under distribution shift.
8. **Distributed learning:** characterize convergence when heterogeneous specialists exchange compressed representations rather than gradients.
9. **Common-mode failure:** derive reliability bounds when multiple specialists depend on the same backbone, dataset, bridge, router, or memory.
10. **Self-modification:** formalize safe state transitions so that architecture changes cannot bypass verification constraints.

---

# 18. Decision gate

**Do not implement the complete Holobiont yet.**

Implementation is justified for the small B0–B3 falsification instrument only after these are fixed in experiment configuration:

- datasets/tasks;
- specialist roles;
- model checkpoints/architectures;
- bridge dimensions;
- bridge loss;
- workspace slots and update rule;
- router objective and capacity;
- memory schema and gating policy;
- compute/bandwidth accounting;
- primary/secondary metrics;
- seed/statistical plan;
- exact baseline matching procedure.

The first implementation should be intentionally small enough to isolate causal effects. Recovery, Byzantine defense, and self-modification should not be enabled in the same first experiment because doing so would make attribution difficult.

---

# 19. Primary literature anchors

- Fedus, Zoph & Shazeer (2022), **Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity**, JMLR. https://jmlr.org/papers/v23/21-0998.html
- Zhou et al. (2022), **Mixture-of-Experts with Expert Choice Routing**. https://arxiv.org/abs/2202.09368
- Zoph et al. (2022), **ST-MoE: Designing Stable and Transferable Sparse Expert Models**. https://arxiv.org/abs/2202.08906
- Jaegle et al. (2021), **Perceiver: General Perception with Iterative Attention**. https://arxiv.org/abs/2103.03206
- Alemi et al. (2016), **Deep Variational Information Bottleneck**. https://arxiv.org/abs/1612.00410
- Ha, Dai & Le (2016), **HyperNetworks**. https://arxiv.org/abs/1609.09106
- Liu et al. (2021), **Conflict-Averse Gradient Descent for Multi-task Learning**. https://arxiv.org/abs/2110.14048
- Packer et al. (2023), **MemGPT: Towards LLMs as Operating Systems**. https://arxiv.org/abs/2310.08560
- Lewis et al. (2020), **Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks**. https://arxiv.org/abs/2005.11401
- Hwang et al. (2024), **Retrieval-Augmented Generation with Estimation of Source Reliability**. https://arxiv.org/abs/2410.22954
- Kirkpatrick et al. (2017), **Overcoming catastrophic forgetting in neural networks**. https://arxiv.org/abs/1612.00796
- Olfati-Saber, Fax & Murray (2007), **Consensus and Cooperation in Networked Multi-Agent Systems**. https://ieeexplore.ieee.org/document/4118472
- Yin et al. (2018), **Byzantine-Robust Distributed Learning: Towards Optimal Statistical Rates**. https://arxiv.org/abs/1803.01498
- Karimireddy, He & Jaggi (2020), **Learning from History for Byzantine Robust Optimization**. https://arxiv.org/abs/2011.02970
- Zou et al. (2026), **Poison Once, Exploit Forever: Environment-Injected Memory Poisoning Attacks on Web Agents**. https://arxiv.org/abs/2604.02623
- Gao et al. (2026), **MemPoison: Uncovering Persistent Memory Threats and Structural Blind Spots in LLM Agents**. https://arxiv.org/abs/2607.14651

---

# 20. Provenance and research discipline

This milestone uses primary papers and authoritative open repositories where available. Search results are discovery aids; decisive claims should be checked against the paper itself and its stated assumptions.

No result in this dossier is interpreted as validation of the complete Cognitive Holobiont. Experimental hypotheses are intentionally written so that the architecture can fail.
