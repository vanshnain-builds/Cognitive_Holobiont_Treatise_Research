# Milestone 18 — Adaptive Topology Stability and Game-Theoretic Routing Audit

**Status:** research / pre-implementation  
**Date:** 2026-09-19  
**Scope:** adaptive routing, coupled graph-learning dynamics, stability, strategic routing attacks, robust aggregation under multimodal honest populations, privacy/robustness interaction, and mathematical limits of self-organizing communication.

## 1. Research question

Milestone 17 established that topology is a security-critical state variable. Milestone 18 asks the harder question:

> Can a learned router adapt topology without creating unstable feedback loops, strategic incentives, or false reliability signals?

The central correction is:

\[
\boxed{\text{adaptive routing} \neq \text{stable routing} \neq \text{optimal routing}}
\]

A router changes the data distribution seen by specialists, the evidence available to the reliability layer, and the gradients used to update the router itself. Therefore routing is a coupled control/learning problem, not merely an attention mechanism.

## 2. Evidence reviewed

### 2.1 Sparse MoE routing and capacity

Switch Transformers established the practical value of sparse routing but also documented routing, communication, and training-stability constraints. Expert Choice later showed that changing the capacity/routing formulation can improve expert utilization and optimization behavior. These results establish conditional-computation mechanisms, not a theorem that adaptive routing improves end-to-end cognition.

- Fedus, Zoph & Shazeer (2021), Switch Transformers: https://jmlr.org/papers/v23/21-0998.html
- Zhou et al. (2022), Expert Choice Routing: https://arxiv.org/abs/2202.09368

### 2.2 Adaptive communication and distributed learning

Recent Byzantine-robust distributed-learning work demonstrates that changing peers/communication can materially alter adversarial resilience. This reinforces Milestone 17's conclusion that the graph itself belongs in the threat model.

- GRANITE (2025): https://arxiv.org/abs/2504.17471

### 2.3 Byzantine robustness under privacy constraints

Recent work combines Byzantine robustness with secure/private federated aggregation, but these systems introduce nontrivial computational and statistical tradeoffs. Zero-knowledge approaches can verify aggregation logic without revealing protected values, while practical privacy/robustness frameworks use dimensionality reduction or robust filters. These are evidence for engineering feasibility under explicit assumptions, not universal robustness.

- ByzSFL (2025): https://arxiv.org/abs/2501.06953
- Practical privacy-preserving Byzantine-robust FL (2025): https://arxiv.org/abs/2512.17254
- Certifiably Byzantine-Robust Federated Conformal Prediction: https://arxiv.org/abs/2406.01960

### 2.4 Adaptive adversaries and formal robustness

Adversarial-robust ML literature distinguishes empirical attack resistance from formal certification. Exact robustness verification is computationally difficult for general neural networks, so the Treatise must not turn successful attack sweeps into universal security guarantees.

- *An Overview of Neural Network Verification and Its Applications* / verification literature: https://arxiv.org/abs/2305.13991

## 3. Coupled Holobiont dynamics

Let specialist parameters be \(\theta_t\), router parameters \(\psi_t\), topology \(A_t\), workspace state \(w_t\), and memory \(M_t\). A generic coupled system is

\[
\theta_{t+1}=\theta_t-\eta_\theta\nabla_\theta \mathcal L(\theta_t,\psi_t,A_t,M_t),
\]

\[
\psi_{t+1}=\psi_t-\eta_\psi\nabla_\psi \mathcal L_R(\theta_t,\psi_t,A_t,M_t),
\]

\[
A_t=\mathcal R(s_t;\psi_t),
\]

\[
w_{t+1}=F(w_t,A_t,Z_t),
\]

\[
M_{t+1}=\operatorname{Commit}(M_t,c_t).
\]

The important mathematical point is that \(A_t\) is a function of learned state, while learned state depends on the topology induced by \(A_t\). This is a feedback system.

A stability claim therefore needs a specified dynamical system and a notion of stability (e.g. Lyapunov stability, contraction, bounded regret, or convergence in probability). “The router converged in training” is not sufficient.

## 4. Local stability condition

Let the continuous approximation be

\[
\dot s=f(s),\qquad s=(\theta,\psi,w,m).
\]

Around an equilibrium \(s^*\), with Jacobian

\[
J=\left.\frac{\partial f}{\partial s}\right|_{s=s^*},
\]

local asymptotic stability follows when all eigenvalues satisfy

\[
\operatorname{Re}(\lambda_i(J))<0.
\]

This is a standard local result, but it does not apply directly to discontinuous top-k routing, discrete memory commits, stochastic gradients, or changing graphs. Those require appropriate hybrid/stochastic analysis.

Therefore the Treatise must remove any generic statement of the form

\[
\text{stable specialists}+\text{stable router}\Rightarrow\text{stable Holobiont}.
\]

Coupled subsystems can destabilize each other even when isolated subsystems are individually stable.

## 5. Router-induced distribution shift

Suppose specialist \(i\) is trained on an induced distribution \(D_i(\psi)\). Changing router parameters changes this distribution:

\[
\psi\rightarrow D_i(\psi)\rightarrow \theta_i'\rightarrow D_i(\psi').
\]

This creates a feedback loop. A router may initially prefer a specialist because it is strong on current traffic; reduced traffic then changes its training signal and calibration; the router may subsequently abandon it. This can create specialization collapse, oscillation, or starvation.

The Treatise should therefore measure per-specialist traffic share

\[
p_i(t)=\frac{N_i(t)}{\sum_jN_j(t)},
\]

gradient exposure, calibration drift, and task-conditioned utility rather than only average routing entropy.

## 6. Routing entropy is not a health metric

For routing probabilities \(p_t\),

\[
H_t=-\sum_i p_i\log p_i.
\]

High entropy can indicate useful diversity or indecision. Low entropy can indicate excellent specialization or expert collapse.

Thus no universal mapping

\[
H_t\rightarrow\text{health}
\]

is justified.

A more meaningful statistic is conditional utility:

\[
U_i=E[U\mid r=i],
\]

plus utilization, calibration, latency, and counterfactual routing performance.

## 7. Strategic routing attack model

An adaptive attacker can optimize not only model outputs but the topology:

\[
\max_{A'\in\mathcal A_B}
\;\mathcal L_{target}(A')
\]

subject to a topology-change budget such as

\[
\|A'-A\|_0\le B_E.
\]

For probabilistic routing, a stronger attacker may manipulate router inputs or reliability features:

\[
\max_{\delta\in\Delta}
\mathcal L(f_{\psi}(x+\delta))
\]

while minimizing detection probability.

The Treatise should test both direct edge manipulation and indirect feature manipulation. A router that is robust to one is not thereby robust to the other.

## 8. No-regret interpretation

If routing is framed as a repeated decision problem with reward \(u_t(i)\), a no-regret policy satisfies, for suitable bounded rewards,

\[
\frac1T\left(\max_i\sum_{t=1}^Tu_t(i)-\sum_{t=1}^Tu_t(i_t)\right)\rightarrow0.
\]

This is a legitimate online-learning property under its assumptions. It does not establish globally optimal topology because the rewards depend on the routing policy itself and on changing specialist states.

The Holobiont therefore should not claim

\[
\text{no regret}\Rightarrow\text{optimal cognitive organization}.
\]

## 9. Robust aggregation under multimodal honest populations

A robust aggregator should be analyzed relative to an honest set \(H\) whose update distribution may be multimodal:

\[
G_H=\bigcup_{k=1}^K G_k.
\]

A center-based detector assumes a geometry that may not match this distribution. For specialized organs, large inter-cluster distances can be legitimate.

Experiments should compare:

1. coordinate-wise median;
2. trimmed mean;
3. Krum-style distance filtering;
4. cluster-aware filtering;
5. oracle honest-cluster filtering.

The result should be reported as a Pareto surface over attack success, false quarantine, clean utility, and communication cost.

## 10. Privacy/robustness interaction

A privacy mechanism may add noise:

\[
\tilde g_i=g_i+\xi_i.
\]

If robust aggregation relies on geometric separation between honest and malicious updates, increasing noise variance can reduce that separation.

This does not prove that privacy and Byzantine robustness are incompatible; it proves that their interaction must be measured under a concrete mechanism.

The relevant quantities are:

\[
\text{privacy loss},\quad
\text{attack success},\quad
\text{clean utility},\quad
\text{false quarantine},\quad
\text{communication/compute cost}.
\]

## 11. New reliability condition: evidence diversity

For reliability estimates based on multiple specialists, define a dependency graph \(D\) where edge weight \(d_{ij}\in[0,1]\) measures shared failure causes.

A crude effective-evidence diagnostic is

\[
N_{eff}=\frac{(\sum_i w_i)^2}{\sum_{i,j}w_iw_j\rho_{ij}},
\]

where \(\rho_{ij}\) is an empirically estimated correlation. This is a diagnostic inspired by effective sample size, not a theorem of semantic independence.

The Treatise must not claim that \(N_{eff}\) proves correctness. It can only quantify redundancy under a chosen statistical model.

## 12. New experiment matrix

| Experiment | Intervention | Primary outcome | Required control |
|---|---|---|---|
| A1 | router learning-rate sweep | stability/utility | frozen router |
| A2 | top-k vs soft routing | task risk/cost | matched compute |
| A3 | traffic starvation | forgetting/calibration | balanced traffic |
| A4 | adaptive edge attack | attack success | fixed graph |
| A5 | reliability-feature attack | false acceptance | clean reliability features |
| A6 | multimodal honest updates | false quarantine | oracle clusters |
| A7 | privacy-noise sweep | robustness/privacy frontier | no-noise baseline |
| A8 | common-lineage replicas | false confidence | diverse lineage |
| A9 | router oscillation | time-to-stable-state | fixed routing |
| A10 | changing workload | regret/utility | stationary workload |

## 13. Hypotheses H80–H87

**H80 — coupled instability:** a router can destabilize specialist training even when specialists and router are individually stable under frozen counterparts.

**H81 — starvation feedback:** persistent low routing probability causes measurable specialist forgetting/calibration drift, which can reinforce router starvation.

**H82 — routing attack:** bounded topology manipulation can produce reliability degradation without directly corrupting specialist parameters.

**H83 — feature attack:** manipulating reliability inputs can be at least as effective as direct edge manipulation under matched attacker budget.

**H84 — multimodal honest robustness:** cluster-aware Byzantine filtering reduces false quarantine relative to single-center filtering on specialized honest populations.

**H85 — privacy/robustness tradeoff:** increasing privacy noise can worsen Byzantine detection at some operating points, producing a measurable Pareto frontier rather than a monotonic improvement.

**H86 — effective evidence:** dependency-aware evidence selection reduces false confidence relative to replica-count-only selection at matched storage/compute.

**H87 — online routing:** no-regret routing can improve cumulative utility in stationary or slowly changing environments but need not minimize end-to-end risk in nonstationary coupled systems.

## 14. Claim classification

### Established

- Sparse conditional computation can reduce activated computation under appropriate architectures and workloads.
- Routing/capacity design affects expert utilization and optimization.
- Local asymptotic stability is characterized by Jacobian eigenvalues for the specified smooth continuous system.
- No-regret guarantees are available for specified online-learning settings.
- Adversarial robustness requires explicit threat models; empirical attack resistance is not a universal certificate.
- Privacy mechanisms and Byzantine robustness impose distinct statistical/system constraints.

### Plausible engineering synthesis

- Treating routing as a feedback-control problem.
- Traffic-aware anti-starvation mechanisms.
- Failure-domain-aware routing and evidence selection.
- Cluster-aware Byzantine detection for multimodal specialists.
- Joint privacy/robustness resource frontiers.

### Unsupported/speculative

- Adaptive routing necessarily creates superior cognitive organization.
- No-regret routing implies optimal Holobiont cognition.
- Routing entropy measures system health.
- More diverse edges always improve reliability.
- Privacy-preserving aggregation automatically preserves Byzantine robustness.

### Mathematically incorrect/incomplete

- Stable components \(\Rightarrow\) stable coupled Holobiont.
- \(H(p)\) \(\Rightarrow\) reliability/health.
- \(\|A'-A\|_0\le B\) \(\Rightarrow\) bounded semantic damage without a task model.
- Effective sample size \(N_{eff}\) \(\Rightarrow\) correctness.
- No-regret \(\Rightarrow\) global optimum in a nonstationary coupled environment.

## 15. Decision gate

Do not enable autonomous topology adaptation until the fixed-topology B0–B4 baselines establish:

1. measurable communication benefit;
2. measurable workspace benefit;
3. measurable memory benefit;
4. measurable reliability benefit;
5. stable resource-normalized performance;
6. robustness to direct and indirect routing attacks;
7. acceptable false-quarantine rate under multimodal honest specialists.

The first adaptive-routing experiment should update only \(\psi\) while keeping specialist parameters, memory policy, and verifier policy frozen. Subsequent experiments may release one additional degree of freedom at a time.

## 16. Open mathematical problems

1. Derive stability conditions for discrete stochastic top-k routing coupled to nonconvex specialist optimization.
2. Characterize routing equilibria under endogenous specialist utility.
3. Derive regret bounds when action rewards depend on the learner's induced training distribution.
4. Formalize Byzantine robustness for multimodal honest update manifolds.
5. Relate graph manipulation budgets to task-risk degradation under explicit channel/task assumptions.
6. Quantify privacy noise versus robust-aggregation breakdown under concrete threat models.
7. Derive dependency-aware reliability bounds without assuming independence.
8. Establish conditions for safe adaptive topology changes with rollback guarantees.

## 17. Provenance

- Fedus, Zoph & Shazeer, Switch Transformers: https://jmlr.org/papers/v23/21-0998.html
- Zhou et al., Expert Choice Routing: https://arxiv.org/abs/2202.09368
- GRANITE: https://arxiv.org/abs/2504.17471
- ByzSFL: https://arxiv.org/abs/2501.06953
- Practical privacy-preserving Byzantine-robust FL: https://arxiv.org/abs/2512.17254
- Certifiably Byzantine-Robust Federated Conformal Prediction: https://arxiv.org/abs/2406.01960
- Neural-network verification survey/work: https://arxiv.org/abs/2305.13991
- Fault-Tolerant Federated Reinforcement Learning: https://arxiv.org/abs/2110.14074

These sources are used to support bounded component claims only. None establishes the complete Cognitive Holobiont architecture, consciousness, AGI, indefinite self-repair, or autonomous evolutionary intelligence.

## 18. Status

Milestone 18 strengthens the Treatise's central methodological position: adaptive topology must be treated as a learned feedback controller operating under adversarial, statistical, and resource constraints. The next implementation-relevant evidence should come from frozen-component controls followed by single-variable adaptive-routing experiments. No whole-system validation is claimed.
