# Milestone 20 — Self-Evolving Cognition, Meta-Learning, and Controlled Cognitive Change Audit

## Scope

Milestone 20 audits the Treatise's strongest remaining architectural claim: that a Cognitive Holobiont can not only route, remember, recover, and defend itself, but can **change its own organization, modules, memory policies, routing rules, and eventually its own capabilities** while retaining safety and useful function.

The key correction is that "self-evolution" is not one property. It decomposes into:

1. parameter adaptation;
2. memory adaptation;
3. module/expert addition or retirement;
4. routing-policy adaptation;
5. architecture/topology modification;
6. objective/utility modification;
7. code or algorithm modification;
8. open-ended evolutionary search.

Evidence for one level must not be generalized to the next.

## 1. Formal decomposition of cognitive change

Let the deployed system state be

\[
S_t=(\theta_t,\psi_t,A_t,w_t,M_t,V_t,G_t),
\]

where \(G_t\) denotes the active module/architecture graph.

Define a change operator

\[
S_{t+1}=\mathcal C(S_t,e_t,u_t,\xi_t),
\]

where \(e_t\) is experience, \(u_t\) is an authorized adaptation action, and \(\xi_t\) is stochasticity.

The Treatise should distinguish

\[
\mathcal C_\theta,\quad \mathcal C_M,\quad \mathcal C_G,\quad \mathcal C_\psi,\quad \mathcal C_J
\]

for parameter, memory, graph, policy, and objective changes.

A system that learns new parameters while preserving a fixed objective is **adaptive**, but not necessarily self-evolving in the stronger sense.

## 2. Stability must be defined across change

Let task utility be \(U_t\), safety cost \(C_t\), and resource cost \(K_t\). A candidate adaptation is acceptable only if

\[
U_{t+1}\ge U_{\min},\qquad C_{t+1}\le C_{\max},\qquad K_{t+1}\le K_{\max}.
\]

For a sequence of changes, define cumulative degradation relative to a frozen baseline:

\[
D_T=\sum_{t=1}^{T}\left[U_t^{\mathrm{base}}-U_t^{\mathrm{evolved}}\right]_+.
\]

This is an operational metric, not a theorem of safety.

For protected tasks \(\mathcal T_P\), a retention constraint is

\[
\sup_{\tau\in\mathcal T_P}\left(R_{\tau,t}-R_{\tau,0}\right)\le\epsilon_P.
\]

This makes continual adaptation falsifiable without assuming that all old behavior must remain unchanged.

## 3. Adaptation versus objective drift

Parameter updates optimize a fixed objective \(J\):

\[
\theta_{t+1}=\theta_t-\eta\nabla_\theta J(\theta_t).
\]

Objective modification changes the optimization target itself:

\[
J_{t+1}=\mathcal O(J_t,e_t).
\]

The Treatise must therefore not infer

\[
\text{continual learning}\Rightarrow\text{self-directed evolution}.
\]

An agent can improve an internal reward while violating an external specification. Objective drift is a separate security and alignment surface.

## 4. Evidence from modular continual learning

Recent work supports a limited engineering claim: separating task-specific capacity and learning routing can reduce interference while enabling reuse. CaRE uses bi-level routing for long task sequences; CRAM combines centroid-guided routing, adaptive-rank modules, and orthogonality constraints for multimodal continual instruction tuning. These results support **useful modular continual adaptation under tested task distributions**, not lifelong intelligence. [CaRE](https://arxiv.org/abs/2602.03473); [CRAM](https://arxiv.org/abs/2606.02502).

The empirical target is

\[
\text{plasticity gain}-\text{retention loss}-\text{resource cost}.
\]

## 5. Evolving routing has a cost

AIR-MoE uses coarse candidate selection followed by exact scoring for granular MoE routing and explicitly treats routing cost as a scaling issue. [AIR-MoE](https://arxiv.org/abs/2605.04952).

Thus

\[
\text{more experts}\not\Rightarrow\text{free modular capacity}.
\]

If expert count is \(N\), naive router scoring can scale with \(N\), while approximate routing trades computation for possible routing error.

A resource objective is

\[
J=R+\lambda_C C+\lambda_B B+\lambda_L L+\lambda_M M,
\]

where risk \(R\), compute \(C\), bandwidth \(B\), latency \(L\), and memory \(M\) are measured separately.

## 6. Self-evolving agents: evidence boundary

Recent surveys organize self-evolving agents by what evolves, when it evolves, and how evolution is triggered. They document adaptation of memory, tools, models, workflows, and code while identifying safety, scalability, evaluation, and co-evolution as open problems. [Self-Evolving Agents survey](https://arxiv.org/abs/2507.21046).

Recent engineering work on self-evolving software agents reports autonomous generation of goals and executable behaviors in constrained dynamic environments while explicitly noting limits in behavioral inheritance and stability. [Self-Evolving Software Agents](https://arxiv.org/abs/2604.27264).

Classification:

- autonomous code/goal modification: **demonstrated engineering research** in bounded settings;
- reliable open-ended self-improvement: **unestablished**;
- automatic emergence of general intelligence: **speculative**.

## 7. Protected reference objective

If policy and objective can both change, a naive evolutionary loop is

\[
S_{t+1}=\operatorname{Mutate}(S_t),\qquad S_{t+1}=\arg\max U(S).
\]

This permits evaluator optimization rather than intended-objective optimization.

Introduce an externally governed specification \(J_0\) and require

\[
\max_{S'}U(S')
\]

subject to

\[
\mathcal V(S',J_0)=1,
\]

where \(\mathcal V\) is independently evaluated.

This does not solve specification gaming in general; it makes the missing assumption explicit.

## 8. Two-timescale adaptation

The Holobiont has at least two adaptation rates:

\[
\theta_{t+1}=\theta_t-\eta_\theta g_t,
\qquad
\psi_{t+1}=\psi_t-\eta_\psi h_t.
\]

If \(\eta_\psi\gg\eta_\theta\), routing can react rapidly to slowly changing specialists. If \(\eta_\theta\gg\eta_\psi\), specialists may drift before routing catches up.

Two-timescale stochastic approximation has convergence results only under explicit assumptions. Recent multi-agent RL work similarly proves approximate equilibrium concepts only under explicit regret, evaluation, and stochastic-approximation assumptions. [Equilibrium in Multi-Agent Reinforcement Learning](https://arxiv.org/abs/2608.22840).

The Treatise therefore rejects arbitrary convergence claims for non-convex neural routing/training loops.

## 9. Coupled co-evolution

With adaptive attackers, routers, and specialists:

\[
\begin{aligned}
S_{t+1}&=F(S_t,\pi_t,\alpha_t),\\
\pi_{t+1}&=G(\pi_t,S_t,\alpha_t),\\
\alpha_{t+1}&=H(\alpha_t,S_t,\pi_t).
\end{aligned}
\]

The coupled local Jacobian is

\[
J_{\mathrm{coupled}}=
\begin{bmatrix}
F_S & F_\pi & F_\alpha\\
G_S & G_\pi & G_\alpha\\
H_S & H_\pi & H_\alpha
\end{bmatrix}.
\]

Even if

\[
\rho(F_S)<1,\quad\rho(G_\pi)<1,\quad\rho(H_\alpha)<1,
\]

it does not follow that

\[
\rho(J_{\mathrm{coupled}})<1.
\]

Cross-coupling can destabilize the combined process. This formalizes why isolated-component stability experiments are insufficient.

## 10. Memory evolution is a distinct mechanism

WorldMemArena evaluates multimodal agent memory as a lifecycle of writing, maintenance, retrieval, and use. It reports that better memory writing/storage does not necessarily yield better downstream performance, and that systems remain unstable across domains. [WorldMemArena](https://arxiv.org/abs/2605.29341).

The Treatise therefore separates

\[
\text{memory quality}\neq\text{memory utility}\neq\text{task performance}.
\]

The memory loop is

\[
\text{observe}\rightarrow\text{write}\rightarrow\text{maintain}\rightarrow\text{retrieve}\rightarrow\text{use}\rightarrow\text{update}.
\]

## 11. Persistent memory as an evolutionary attack surface

Recent studies report persistent memory poisoning through external observations and delayed cross-session activation. Sleeper Memory Poisoning reports that malicious content can become persistent memory and later trigger attacker-intended behavior; MemPoison reports that write-time defenses can suppress direct corruption while failing against compositional or dormant multi-record attacks. [Sleeper Memory Poisoning](https://arxiv.org/abs/2605.15338); [MemPoison](https://arxiv.org/abs/2607.14651).

Self-evolving memory must therefore be treated as an untrusted state transition:

\[
M_{t+1}=\operatorname{Commit}(M_t,c_t;V_M),
\]

where \(V_M\) is a separately evaluated admission/maintenance policy.

## 12. Cognitive-change accounting

Let \(\Delta_t\) denote behavioral change between consecutive versions:

\[
\Delta_t=D_{\mathcal T}(f_{t+1},f_t).
\]

Large \(\Delta_t\) should trigger additional evaluation.

A change budget can be defined as

\[
\sum_{t\in\mathcal W}\Delta_t\le B_{\mathrm{change}}.
\]

This is an engineering control, not a universal safety theorem.

## 13. Lineage-aware evolution

Each version should carry lineage

\[
L_t=(parent,data,code,memory,router,verifier,evaluation).
\]

Two versions generated from the same poisoned memory, code commit, or verifier do not constitute independent evidence. Evolutionary branch count must therefore be separated from effective independent evidence.

## 14. New hypotheses H97–H105

**H97 — modular continual adaptation improves retention/plasticity tradeoffs relative to a capacity-matched monolithic baseline on long task sequences.**

**H98 — routing-selection overhead becomes a material fraction of total cost as expert granularity increases.**

**H99 — self-evolving code/architecture systems exhibit measurable behavioral drift even when aggregate task reward improves.**

**H100 — protected-objective verification reduces evaluator-gaming compared with reward-only selection.**

**H101 — two-timescale routing/training regimes exhibit qualitatively different stability and specialization behavior.**

**H102 — coupled defender/attacker adaptation can produce cycles or metastable states not visible in isolated-component stability tests.**

**H103 — memory lifecycle quality predicts downstream utility better when writing, maintenance, retrieval, and use are evaluated separately.**

**H104 — lineage-aware evaluation detects common-mode evolutionary failures that raw version count misses.**

**H105 — bounded cognitive-change budgets reduce catastrophic behavioral drift at a measurable adaptation cost.**

## 15. Experiment matrix

### E1 — fixed-objective continual adaptation
Compare monolithic, modular, and MoE systems over long task sequences. Match parameter count, active FLOPs, memory, and communication.

### E2 — routing granularity/cost
Sweep expert count and granularity while measuring routing FLOPs, latency, expert utilization, load balance, and task risk.

### E3 — memory lifecycle
Separate writing, maintenance, retrieval, and use. Test whether each stage independently predicts downstream performance.

### E4 — self-modification sandbox
Allow bounded code/module changes under a frozen external evaluator. Measure utility gain, behavioral drift, safety violations, and change magnitude.

### E5 — objective-drift attack
Allow internal reward modification while an external protected objective remains fixed. Measure specification gaming and divergence.

### E6 — two-timescale stability
Sweep \(\eta_\psi/\eta_\theta\), top-k, router temperature, and update frequency. Measure oscillation, starvation, specialization, and regret.

### E7 — co-evolution
Compare fixed attacker, adaptive attacker, adaptive defender, and jointly adaptive settings. Report cycle frequency, attack persistence, and recovery quality.

### E8 — lineage stress
Create independent, shared-parent, shared-memory, shared-code, and shared-verifier evolutionary branches. Compare raw branch count with effective independent evidence.

### E9 — change-budget gate
Compare unrestricted adaptation with bounded \(\sum\Delta_t\) policies. Measure adaptation benefit versus protected-task degradation.

## 16. Statistical protocol

Primary endpoints:

- protected-task retention;
- new-task utility;
- cumulative forgetting;
- behavioral drift;
- routing overhead;
- expert utilization;
- memory lifecycle success;
- attack success;
- specification-gaming rate;
- recovery behavioral error;
- change-budget violations;
- compute/bandwidth/latency.

Use multiple independent seeds, task-order permutations, matched resource budgets, confidence intervals, and predeclared primary endpoints. For long-horizon evolution, report trajectories rather than only final scores.

## 17. Claim classification

### Established

- Continual learning can adapt models while suffering stability/plasticity and forgetting tradeoffs.
- Modular/MoE routing can support conditional computation and task specialization.
- Routing cost can become significant as expert granularity increases.
- Self-evolving agent research demonstrates bounded forms of memory/tool/code/goal adaptation.
- Persistent agent memory is a demonstrated attack surface.
- Multi-agent equilibrium guarantees require explicit game, information, regret, and stochastic-approximation assumptions.

### Plausible engineering synthesis

- Separate parameter, memory, routing, graph, and objective evolution.
- Use protected external objectives and independent verification for self-modification.
- Maintain version lineage and dependency-aware evidence.
- Enforce behavioral-change budgets.
- Treat memory evolution as an untrusted state transition.
- Use two-timescale diagnostics for specialist/router adaptation.

### Unsupported/speculative

- Self-evolution necessarily produces increasing intelligence.
- Open-ended self-modification converges to beneficial architectures.
- Autonomous objective modification preserves original intent.
- Evolutionary pressure automatically produces robust cognition.
- Unlimited modular growth produces indefinite capability scaling.
- A self-evolving Holobiont will autonomously discover AGI.

### Mathematically incorrect/incomplete

- Continual-learning improvement implies self-evolution.
- Parameter/architecture change magnitude alone measures cognitive improvement.
- Stable isolated learning rules imply stable co-evolution.
- Version count equals independent evolutionary evidence.
- Reward improvement implies objective preservation.
- Memory retrieval quality implies memory truth.
- More experts imply more useful capacity without routing/resource accounting.

## 18. Literature anchors reviewed

- CaRE — Scaling Continual Learning with Bi-Level Routing Mixture-of-Experts (2026): https://arxiv.org/abs/2602.03473
- CRAM — Centroid-Routing and Adaptive MoE for Multimodal Continual Instruction Tuning (2026): https://arxiv.org/abs/2606.02502
- AIR-MoE — Adaptive Inverted-Index Routing for Granular Mixtures-of-Experts (2026): https://arxiv.org/abs/2605.04952
- WorldMemArena — Evaluating Multimodal Agent Memory Through Action-World Interaction (2026): https://arxiv.org/abs/2605.29341
- Equilibrium in Multi-Agent Reinforcement Learning (2026): https://arxiv.org/abs/2608.22840
- DelAC — Multi-agent RL of Team-Symmetric Stochastic Games (2026): https://arxiv.org/abs/2605.12555
- Self-Evolving Agents survey (2025): https://arxiv.org/abs/2507.21046
- Self-Evolving Software Agents (2026): https://arxiv.org/abs/2604.27264
- Hidden in Memory: Sleeper Memory Poisoning in LLM Agents (2026): https://arxiv.org/abs/2605.15338
- MemPoison — Persistent Memory Threats and Structural Blind Spots (2026): https://arxiv.org/abs/2607.14651
- Poison Once, Exploit Forever — Environment-Injected Memory Poisoning (2026): https://arxiv.org/abs/2604.02623
- Switch Transformers / sparse conditional computation: https://jmlr.org/papers/v23/21-0998.html
- HyperNetworks: https://arxiv.org/abs/1609.09106

## 19. Decision gate

Do not enable unrestricted self-modification, objective modification, autonomous architecture search, or open-ended evolutionary deployment.

First establish E1–E9 under frozen external evaluation, explicit protected tasks, matched resource budgets, version lineage, bounded change, and adversarial memory tests. A successful sandbox experiment establishes only the tested adaptation property; it does not validate open-ended self-improvement, consciousness, AGI, anti-fragility, or indefinite self-healing.

## 20. Status

The Cognitive Holobiont remains **falsification-first and pre-implementation**. Milestone 20 strengthens the Treatise by separating adaptation from evolution, memory utility from memory truth, routing adaptation from stable organization, and reward improvement from objective preservation.
