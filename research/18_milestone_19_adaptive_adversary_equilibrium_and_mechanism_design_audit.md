# Milestone 19 — Adaptive Adversary, Mechanism Design, and Equilibrium Audit

## Scope

This milestone audits a stronger claim than Milestone 18: whether an adaptive Cognitive Holobiont can remain reliable when routing, trust, verification, and specialists are simultaneously adapting to an intelligent adversary. The central correction is that the system is no longer adequately represented by a single-agent optimization problem.

## 1. Formal system model

Let the Holobiont state be

\[
S_t=(\theta_t,\psi_t,A_t,w_t,M_t,V_t),
\]

where \(\theta\) are specialist parameters, \(\psi\) router/trust parameters, \(A_t\) topology, \(w_t\) workspace state, \(M_t\) memory, and \(V_t\) verification state.

Let an adaptive adversary choose \(a_t\in\mathcal A(S_t)\), producing observations and perturbations. A general transition is

\[
S_{t+1}\sim P(\cdot\mid S_t,\pi_t,a_t,\xi_t).
\]

The defender policy \(\pi\) and adversary policy \(\alpha\) form a coupled stochastic game rather than an ordinary supervised-learning objective.

A robust objective can be written

\[
\min_{\pi\in\Pi}\max_{\alpha\in\mathcal A}
J(\pi,\alpha),
\]

but this expression is only meaningful after specifying observability, action spaces, timing, budgets, stochasticity, and whether policies are stationary, online, or history-dependent.

## 2. Key mathematical correction: minimax is not automatically an equilibrium

The expression

\[
\min_\pi\max_\alpha J(\pi,\alpha)
\]

does not by itself prove existence of a saddle point. Minimax equality requires appropriate compactness/convexity/continuity assumptions (or a different equilibrium concept).

For non-convex neural policies and discrete topology actions, these assumptions generally fail. Therefore the Treatise must not claim that adversarial training finds a globally robust equilibrium merely because a minimax loss was written down.

The operational target should instead be an explicitly defined approximate equilibrium or a bounded-regret/robustness property under a finite threat model.

## 3. Threat-model hierarchy

The dossier separates:

1. **Oblivious adversary** — attack chosen before deployment trajectory.
2. **Adaptive observation adversary** — observes system outputs/topology before acting.
3. **White-box adaptive adversary** — knows parameters and defense mechanism.
4. **Strategic adversary** — optimizes long-horizon utility, not immediate loss.
5. **Colluding adversary** — coordinates multiple compromised organs/clients.
6. **Common-mode adversary** — attacks shared memory, verifier, router, data lineage, or infrastructure.

A result under class 1 must not be generalized to classes 2–6.

## 4. Routing as a mechanism-design problem

A router allocates scarce resources among specialists. Let specialist \(i\) have utility \(u_i(r_i)\), communication cost \(c_i\), and risk contribution \(q_i\). The defender selects allocation \(r\) under capacity constraints:

\[
\max_r\sum_i u_i(r_i)
\]

subject to

\[
\sum_i c_i r_i\le C_{\max},\qquad
\sum_i q_i r_i\le Q_{\max}.
\]

This resembles constrained mechanism/resource allocation, but the utilities and risks may depend on the allocation itself. Thus static knapsack intuition is insufficient.

A routing policy can create externalities:

\[
u_i=u_i(r_i,r_{-i}),
\]

because traffic changes expert specialization, queueing, memory freshness, and evidence dependence.

## 5. New correction: routing entropy is not an equilibrium criterion

For routing probabilities \(p_t\),

\[
H(p_t)=-\sum_i p_{i,t}\log p_{i,t}.
\]

Entropy measures dispersion of routing probabilities. It does not establish utility, fairness, stability, independence, or correctness.

The Treatise therefore rejects all universal claims of the form

\[
H(p)\uparrow\Rightarrow\text{health}\uparrow
\]

or

\[
H(p)\downarrow\Rightarrow\text{collapse}.
\]

## 6. Strategic manipulation of trust

If trust score \(T_i\) affects routing and routing affects future observations used to update \(T_i\), then an attacker can attempt a feedback-loop attack:

\[
T_i\rightarrow r_i\rightarrow y_i\rightarrow T_i'.
\]

A specialist may be promoted because it behaves well under the current evaluation distribution, then receive sufficient traffic to influence memory/workspace state, after which the resulting state makes its future outputs appear more trustworthy.

This creates a potential positive-feedback vulnerability.

The dossier therefore requires measuring trust hysteresis, promotion/demotion delay, and attack persistence rather than only instantaneous attack success.

## 7. Evidence independence

Let \(E_1,\ldots,E_n\) be evidence streams. Majority voting assumes some degree of independence or bounded correlation. Under common-mode dependence,

\[
E_i=G+\epsilon_i,
\]

where \(G\) is shared corrupted information, increasing \(n\) does not necessarily reduce error.

A practical dependency-adjusted effective sample size can be approximated under exchangeable correlation \(\rho\) by

\[
n_{\mathrm{eff}}\approx\frac{n}{1+(n-1)\rho},
\]

for the variance of a sample mean under the stated equal-correlation model.

This is not a universal evidence theorem; it is a diagnostic model requiring estimation of dependence.

## 8. New recovery criterion

Recovery should require both:

\[
\text{integrity}(S')\ge\tau_I
\]

and

\[
\text{behavioral risk}(S')\le\tau_R
\]

on a predeclared verification distribution \(D_V\).

For adversarial deployment, this becomes

\[
\sup_{a\in\mathcal A_V}R(S';a)\le\tau_R
\]

only if the verification set and attack class are sufficiently specified. A finite clean benchmark cannot establish this supremum.

## 9. Hypernetwork/regeneration correction

If a hypernetwork produces

\[
\hat\theta=H_\phi(z),
\]

then successful regeneration requires that the mapping \((\phi,z)\mapsto\theta\) contain sufficient information for the desired behavioral equivalence.

Parameter reconstruction error

\[
\|\hat\theta-\theta\|
\]

is not itself a behavioral guarantee. The relevant criterion is

\[
\Delta_D=E_{x\sim D}[\ell(f_{\hat\theta}(x),f_\theta(x))],
\]

or task loss relative to the required target.

No result should claim arbitrary specialist regeneration without a specified generator family, conditioning information, task class, and behavioral tolerance.

## 10. New hypotheses H88–H96

**H88 — adaptive attacks reduce reliability more than oblivious attacks** at matched perturbation budget.

**H89 — trust-routing positive feedback produces measurable hysteresis** under delayed/adaptive attacks.

**H90 — dependency-adjusted evidence predicts false-consensus failures better than raw replica count.**

**H91 — topology randomization helps only when it reduces attacker observability or dependency; random rewiring alone is not guaranteed to improve robustness.**

**H92 — robust routing under non-IID honest specialists requires cluster/mode-aware baselines.**

**H93 — robust conformal/selective gates improve attack-aware risk-coverage only when their threat-model assumptions are met.**

**H94 — behavioral verification catches a nontrivial fraction of integrity-preserving but semantically corrupted recoveries.

**H95 — hypernetwork regeneration quality depends more strongly on conditioning information and task coverage than on parameter-space reconstruction error alone.**

**H96 — coupled defender/adversary adaptation can produce cycles even when each isolated learning rule is stable.**

## 11. Experiment matrix

### A1 — adversary adaptivity sweep

Compare oblivious, black-box adaptive, white-box adaptive, and strategic attackers at matched budgets.

### A2 — trust feedback

Measure promotion/demotion delay, trust hysteresis, routing concentration, attack persistence, and recovery time.

### A3 — dependency stress

Construct independent, partially shared, and common-mode evidence lineages. Compare raw replica count against estimated effective evidence.

### A4 — mechanism variants

Compare static routing, soft routing, top-k routing, constrained routing, and randomized routing under identical resource budgets.

### A5 — multimodal honest heterogeneity

Create distinct honest specialist modes before introducing Byzantine specialists. Report false quarantine separately from attack success.

### A6 — verification adversary

Attack the recovery verifier itself, including shared-data, shared-model, shared-key, and shared-router cases.

### A7 — regeneration

Compare checkpoint, replica, teacher-conditioned, and hypernetwork recovery under matched recovery compute and behavioral tolerance.

## 12. Statistical protocol

Primary endpoints:

- clean task risk
- adversarial task risk
- attack success rate
- false quarantine rate
- recovery behavioral error
- RPO/RTO
- communication
- compute
- latency
- calibration/risk-coverage
- effective evidence/dependency

Use fixed seeds plus multiple independent runs, report confidence intervals, and preregister primary comparisons. Attack budgets and resource budgets must be matched before claiming superiority.

## 13. Claim classification

### Established

- Adaptive adversaries require stronger assumptions than oblivious attacks.
- Minimax formulations require assumptions for minimax equality/equilibrium guarantees.
- Correlated evidence reduces the value of naive majority voting.
- Parameter-space distance does not generally imply functional equivalence.
- Robust aggregation depends on the honest-data distribution and threat model.

### Plausible engineering synthesis

- Trust-aware routing as a feedback system.
- Dependency-aware evidence accounting.
- Mechanism-style routing with explicit resource/risk constraints.
- Behavioral verification before reintegration.
- Threat-model-indexed recovery policies.

### Unsupported/speculative

- Adaptive routing converges to globally optimal cognitive organization.
- A self-modifying Holobiont can automatically discover stable equilibria.
- More independent-looking organs guarantee robustness.
- Randomized topology universally defeats adaptive attackers.
- Hypernetwork regeneration can reconstruct arbitrary lost cognition.

### Mathematically incorrect/incomplete

- \(\min\max J\) automatically implies a saddle point.
- Stable defender dynamics plus stable attacker dynamics implies stable coupled dynamics.
- Routing entropy is a health/stability theorem.
- Replica count is equivalent to independent evidence.
- Parameter reconstruction error is a behavioral recovery guarantee.
- Clean verification risk bounds imply worst-case adversarial risk without a threat-model bridge.

## 14. Literature anchors reviewed

- FedCLEAN (2025): non-IID-aware activation-error clustering for Byzantine filtering. https://arxiv.org/abs/2501.12123
- FLTG (2025): angle-based, non-IID-aware Byzantine aggregation. https://arxiv.org/abs/2505.12851
- OptiGradTrust (2025): multi-feature adaptive trust weighting for Byzantine FL. https://arxiv.org/abs/2507.23638
- Certifiably Byzantine-Robust Federated Conformal Prediction (2024): explicit Byzantine threat model and break-point analysis. https://arxiv.org/abs/2406.01960
- Robust/adversarial conformal calibration analysis (2025). https://arxiv.org/abs/2511.18562
- Switch Transformers: conditional computation and routing/capacity tradeoffs. https://jmlr.org/papers/v23/21-0998.html
- Expert Choice Routing: alternative expert-capacity/routing formulation. https://arxiv.org/abs/2202.09368
- HyperNetworks: generated network weights and conditioning. https://arxiv.org/abs/1609.09106

## 15. Decision gate

Do not implement autonomous strategic routing or self-modification yet. First establish B4/A-series evidence that adaptive attackers do not exploit the trust-routing loop, that dependency-aware verification meaningfully differs from replica count, and that behavioral recovery gates reject semantically compromised states.

The complete Cognitive Holobiont remains unvalidated. These experiments can falsify components; success on them would still not establish consciousness, AGI, indefinite self-healing, universal regeneration, or anti-fragility.
