# Milestone 16 — Recovery, Verification, and Adaptive Compromise Audit

**Status:** research/pre-implementation  
**Date:** 2026-09-17  
**Scope:** verified recovery after specialist, bridge, workspace, memory, router, or verifier compromise; checkpoint integrity; rollback/recovery-point objectives; Byzantine recovery; verifier independence; common-mode verifier failure; self-stabilization; and limits of end-to-end recovery guarantees.

## 1. Research question

The Treatise currently proposes a recovery hierarchy: isolate a failed component, restore a trusted state when available, regenerate only when justified, then verify behavior before reintegration. Milestone 16 asks a narrower question:

> Under which explicit assumptions can recovery restore an acceptable behavioral state, and what prevents the recovery mechanism itself from becoming a common-mode failure or an attacker-controlled oracle?

The central correction is:

\[
\boxed{\text{recovery} \neq \text{correctness restoration}}
\]

A checkpoint can restore bytes without restoring correct behavior; a verifier can approve a wrong state; and a regeneration mechanism can reproduce a compromised policy if its conditioning artifacts are themselves corrupted.

## 2. Evidence reviewed

### 2.1 Classical Byzantine agreement and composition

Byzantine agreement gives a useful distributed-systems reference point, but its guarantees are protocol- and threat-model-specific. Recent work on concurrent/parallel composition under party and communication-channel attacks derives tight thresholds involving both corrupted parties and attacked channels, reinforcing that adding more communication paths does not automatically improve safety. https://arxiv.org/abs/2609.09623

For the Holobiont, this means any claim of consensus-based recovery must state the number of potentially compromised organs, channels, and trusted roots, plus authentication assumptions.

### 2.2 Self-stabilization

Self-stabilizing distributed systems provide an established formal concept for convergence from arbitrary transient state corruption to a legitimate state under explicit assumptions. Automated synthesis work demonstrates that protocols can be synthesized from temporal specifications and topologies. https://arxiv.org/abs/1509.05664

Recent self-stabilizing graph construction results further show that strong convergence properties can be achieved under severe memory constraints, but only for the specified protocol and network model. https://arxiv.org/abs/2505.06596

This supports borrowing the *formal idea* of self-stabilization, not claiming that a neural architecture is self-stabilizing merely because it has reset/recovery logic.

### 2.3 Byzantine-resilient machine learning

Byzantine ML and federated-learning work establishes that arbitrary workers can compromise aggregation unless the algorithm has explicit assumptions and resilience mechanisms. Fault-tolerant federated reinforcement learning also emphasizes that Byzantine resilience depends on the learning objective and stochastic process, so a guarantee from supervised FL cannot simply be transplanted to a cognitive/stateful system. https://arxiv.org/abs/2405.00491  
https://arxiv.org/abs/2110.14074

Recent dynamic-gossip work studies Byzantine resilience when both model exchange and peer sampling are dynamic, showing that the communication graph itself can become an attack surface. https://arxiv.org/abs/2504.17471

### 2.4 Robust prediction after Byzantine corruption

Certifiably Byzantine-robust federated conformal prediction demonstrates that prediction-set guarantees can be combined with Byzantine robustness under explicit assumptions. The useful lesson is architectural: certification can be attached to a decision artifact, but the certificate only applies to the modeled attack/statistical regime. https://arxiv.org/abs/2406.01960

### 2.5 Hypernetwork regeneration

Hypernetworks can generate target-network weights, and continual-learning work shows partial weight generation can mitigate forgetting in defined task streams. HyperInterval similarly explores generating weights from lower-dimensional interval embeddings. These results establish weight generation as an engineering technique, not arbitrary exact reconstruction of a lost specialist. https://arxiv.org/abs/2306.10724  
https://arxiv.org/abs/2405.15444

## 3. Recovery state model

For specialist \(i\), define persistent state

\[
S_i=(\theta_i,\phi_i,M_i,V_i,C_i),
\]

where \(\theta_i\) are model parameters, \(\phi_i\) interface parameters, \(M_i\) specialist memory/state, \(V_i\) version metadata, and \(C_i\) recovery/checkpoint metadata.

A recovery operation produces

\[
\hat S_i = G(A_i),
\]

where \(A_i\) is the surviving artifact set: checkpoint, adapter, teacher outputs, memory, hypernetwork embedding, provenance, or independent reference data.

Define behavioral recovery error on an evaluation distribution \(D\):

\[
\Delta_D(\hat S_i,S_i)
=
E_{(x,y)\sim D}
\left[
\ell(f_{\hat S_i}(x),y)-\ell(f_{S_i}(x),y)
\right].
\]

If the original state is unavailable, use an externally defined reference behavior \(f^*\), not the recovered state itself.

### Important correction

A byte-identical checkpoint gives

\[
\hat\theta_i=\theta_i
\]

but this does not prove the checkpoint was correct before compromise. Conversely, a behaviorally equivalent regenerated model need not satisfy

\[
\hat\theta_i=\theta_i.
\]

Therefore recovery must be defined behaviorally and operationally, not as parameter equality.

## 4. Recovery objectives

### 4.1 Recovery point objective

Let \(t_c\) be compromise detection time and \(t_r\) recovery completion time. If the newest trusted checkpoint was created at \(t_{cp}\), then the recovery point loss is

\[
RPO=t_c-t_{cp}.
\]

For stateful AI this should be measured separately for model, memory, router, workspace configuration, and provenance state.

### 4.2 Recovery time objective

\[
RTO=t_r-t_c.
\]

A system that recovers perfectly after hours of downtime may be less useful than one that recovers approximately in seconds. The Treatise must therefore report the joint frontier

\[
(\Delta_D, RPO, RTO, C_{rec}, B_{rec}).
\]

### 4.3 Safe reintegration

Let \(V(\hat S_i;D_{val})\) be a validation predicate. Reintegration should require

\[
V(\hat S_i;D_{val})=1
\]

plus provenance/version checks and, where relevant, an independent verifier.

The verifier must not simply evaluate the same corrupted artifact used to generate \(\hat S_i\).

## 5. Recovery hierarchy

The dossier defines five recovery levels:

**R0 — restart:** restart process/container with current trusted state.

**R1 — checkpoint rollback:** restore the newest checkpoint that passed integrity and behavioral gates.

**R2 — replica restore:** recover from an independently maintained replica/checkpoint lineage.

**R3 — teacher/reference recovery:** regenerate or distill behavior from an independent teacher/reference model or trusted dataset.

**R4 — hypernetwork/regenerative recovery:** generate weights from surviving conditioning artifacts.

The order is intentional: stronger claims require stronger assumptions. R4 should not be treated as superior merely because it is more biologically evocative.

## 6. Verification independence

Let \(C\) denote corruption of the recovered artifact and \(A\) denote verifier approval.

A useful diagnostic quantity is

\[
P(C\mid A)=
\frac{P(A\mid C)P(C)}{P(A)}.
\]

If the verifier shares the same corrupted memory, bridge, workspace, model family, or training data as the recovery generator, then \(P(A\mid C)\) may remain high.

Thus the architecture should seek **failure-domain diversity**, not merely multiple copies of the same verifier.

### Common-mode verifier failure

If generator \(G\), verifier \(V\), and reference \(R\) all depend on a shared artifact \(Z\), then conditional independence cannot be assumed:

\[
P(G,V,R\mid C)\ne P(G\mid C)P(V\mid C)P(R\mid C).
\]

A practical design should therefore maintain at least one evidence path outside the suspected failure domain where feasible.

This is an engineering recommendation, not a theorem that independence guarantees truth.

## 7. Recovery correctness bounds

Suppose a recovered model \(\hat f\) differs from a reference model \(f^*\) in expected loss by

\[
|L_D(\hat f)-L_D(f^*)|\le \epsilon.
\]

This is a meaningful behavioral recovery statement if the distribution \(D\), loss \(L\), sampling procedure, and confidence level are specified.

If a finite evaluation sample of size \(n\) is used, the empirical estimate requires a statistical confidence argument. For bounded loss \(\ell\in[0,1]\), Hoeffding-style concentration gives, for a fixed recovered model and iid evaluation sample,

\[
P\left(
|\hat L_n-L_D|\ge \epsilon
\right)
\le 2e^{-2n\epsilon^2}.
\]

Equivalently, with confidence \(1-\delta\),

\[
|\hat L_n-L_D|
\le
\sqrt{\frac{\log(2/\delta)}{2n}}.
\]

This bound is only valid under the stated iid/bounded-loss conditions and does not cover adaptive reuse of the validation set without further analysis.

### Important limitation

No finite held-out benchmark proves universal recovery across an unrestricted future task distribution.

## 8. Self-stabilization vs self-healing

A formal self-stabilizing protocol requires a defined set of legitimate states and a convergence property such as:

\[
\forall s_0\in S,
\exists T<\infty:
\quad s_t\in L \quad\forall t\ge T,
\]

where \(L\) is the legitimate-state set.

For the Holobiont, a corresponding definition could be:

\[
\Pr\left[
\forall t\ge T,
Q(S_t)\ge q_{min}
\right]\ge 1-\delta
\]

under a specified fault class and bounded recovery process.

This would be a genuine probabilistic self-stabilization claim.

The current Treatise does **not** yet establish such a theorem.

## 9. Byzantine recovery limits

Let \(n\) be participating organs and \(f\) Byzantine organs. Classical Byzantine agreement thresholds depend strongly on authentication, synchrony, communication topology, and adversarial capabilities. Recent composition results further show that channel attacks can alter the required threshold.

Therefore the Treatise must not state a universal rule such as

\[
n>3f \Rightarrow \text{Holobiont recovery is correct}.
\]

At most, a theorem for a particular consensus/recovery protocol may inherit an appropriate threshold under the protocol's assumptions.

For heterogeneous ML, even a robust aggregator can face honest-but-different updates. Thus the fault predicate should be defined relative to an expected honest behavior set, not merely distance from the median.

## 10. Regeneration information lower bound

If a lost specialist implements an arbitrary function \(f\) from a sufficiently rich hypothesis class, exact reconstruction from a finite artifact cannot be guaranteed without assumptions limiting the function class or supplying enough information.

A task-level guarantee can instead be stated:

\[
\exists G,\;A_i\quad
E_{D}[\ell(f_{G(A_i)}(X),Y)]
\le
E_D[\ell(f_i(X),Y)]+\epsilon.
\]

This is an existence/engineering target, not a theorem about an arbitrary specialist.

If only \(B\) bits of information about the lost state survive, the number of distinguishable artifact states is at most \(2^B\). Exact recovery of \(N\) equally likely discrete states requires at least

\[
B\ge \log_2 N
\]

bits in the lossless identification setting. For continuous/high-dimensional models, a rate–distortion or task-distortion formulation is required instead of pretending that parameter recovery is free.

## 11. Attack surfaces in recovery

### A9 — checkpoint substitution

Replace a valid checkpoint with an attacker-controlled artifact that passes superficial integrity checks.

### A10 — verifier mimicry

Train the recovered model to match the verifier's known tests while failing unseen behavior.

### A11 — provenance compromise

Compromise metadata/signing keys so malicious state appears authentic.

### A12 — recovery-loop poisoning

Repeatedly trigger recoveries so that the recovery mechanism gradually learns or stores attacker-influenced state.

### A13 — replica correlation

Compromise a shared ancestor/checkpoint source so all replicas restore the same corrupted lineage.

### A14 — regeneration conditioning attack

Manipulate the hypernetwork embedding, teacher outputs, task descriptor, or other conditioning artifact used to generate replacement weights.

### A15 — verifier common-mode attack

Compromise the supposedly independent validation source through shared data, code, model family, or infrastructure.

### A16 — rollback oscillation

Cause repeated false positives/negatives so the system alternates between states and never reaches a stable legitimate state.

## 12. Recovery experiment matrix

| Experiment | Fault/attack | Primary metric | Required comparison |
|---|---|---|---|
| R-A | crash | RTO/RPO | restart |
| R-B | checkpoint corruption | behavioral recovery error | integrity-only rollback |
| R-C | malicious checkpoint substitution | attack success after verification | naive checksum |
| R-D | verifier compromise | false approval rate | independent verifier |
| R-E | common-mode reference corruption | recovery risk | shared-reference verifier |
| R-F | Byzantine organ recovery | task risk + convergence | naive consensus |
| R-G | replica lineage compromise | correlated recovery failure | independent lineage |
| R-H | hypernetwork conditioning attack | regeneration error | checkpoint/teacher restore |
| R-I | repeated recovery poisoning | cumulative drift | one-shot recovery |
| R-J | rollback oscillation | time-to-stable-state | fixed threshold policy |

All experiments must report clean performance, fault injection budget, attacker knowledge, detection latency, RPO, RTO, recovery cost, behavioral recovery error, false approval/quarantine rates, and confidence intervals.

## 13. Hypotheses H64–H71

**H64 — verification independence:** a verifier outside the recovery generator's failure domain reduces false reintegration under targeted recovery attacks.

**Falsifier:** independent verification provides no statistically significant reduction at matched evaluation cost.

**H65 — common-mode failure:** correlated generator/verifier/reference corruption can produce high-confidence false approval.

**Falsifier:** the shared-domain architecture maintains the same false-approval rate as domain-diverse verification across the tested common-mode attacks.

**H66 — recovery hierarchy:** checkpoint/replica recovery achieves lower behavioral recovery error than hypernetwork regeneration when trusted state exists, at lower or comparable recovery cost.

**Falsifier:** regeneration matches or beats trusted-state recovery across the preregistered tasks and resource budget.

**H67 — regeneration conditioning:** regeneration quality is sensitive to the information and integrity of its conditioning artifacts.

**Falsifier:** attack or ablation of conditioning artifacts does not materially change recovery behavior.

**H68 — Byzantine recovery:** disagreement-aware recovery improves robustness over naive consensus under heterogeneous honest specialists and bounded Byzantine corruption.

**Falsifier:** no robustness benefit or an unacceptable false-quarantine cost dominates.

**H69 — recovery poisoning:** repeated recovery cycles can accumulate persistent drift when recovery artifacts are not independently validated.

**Falsifier:** cumulative behavioral drift remains statistically indistinguishable from the trusted-recovery control.

**H70 — RPO/RTO tradeoff:** tighter checkpoint intervals reduce behavioral loss after compromise but increase storage/communication cost.

**Falsifier:** no measurable Pareto tradeoff exists in the tested workloads.

**H71 — self-stabilization gap:** reset/recovery logic alone does not establish probabilistic convergence to a legitimate state.

**Falsifier:** a formally specified recovery controller is proven and experimentally demonstrated to satisfy a convergence property under the declared fault model.

## 14. Claim classification

### Established result

- Checkpoint/rollback is a standard fault-recovery technique.
- RPO and RTO are meaningful operational recovery metrics.
- Byzantine agreement and self-stabilization have formal guarantees under explicit protocol/network assumptions.
- Hypernetworks can generate model weights.
- Byzantine-resilient ML can provide convergence/robustness results under explicit assumptions.
- Finite-sample generalization/concentration bounds can quantify uncertainty of a fixed validation estimate under stated assumptions.

### Plausible engineering synthesis

- Hierarchical recovery from trusted checkpoint → independent replica → teacher/reference → hypernetwork regeneration.
- Failure-domain-diverse verification.
- Behavioral rather than parameter-based recovery tests.
- Versioned recovery lineage and rollback.
- Recovery policies that jointly optimize behavioral error, RPO, RTO, communication, and compute.

### Unsupported/speculative

- Hypernetwork regeneration can reconstruct arbitrary lost specialists.
- More replicas automatically imply more reliable recovery.
- Consensus guarantees recovered correctness.
- Cryptographic integrity implies semantic correctness.
- A recovery loop is automatically self-stabilizing.
- Self-healing implies anti-fragility.

### Mathematically incorrect/incomplete

- \(n>3f\Rightarrow\) universal Holobiont correctness.
- Parameter similarity \(\Rightarrow\) behavioral equivalence.
- Checkpoint integrity \(\Rightarrow\) checkpoint correctness.
- Independent-looking validators \(\Rightarrow\) statistical independence.
- Finite validation accuracy \(\Rightarrow\) universal recovery guarantee.
- Regeneration success on one benchmark \(\Rightarrow\) arbitrary specialist recoverability.

## 15. Decision gate

Do **not** implement autonomous regeneration/self-modification until R-A through R-H have established:

1. a measurable recovery benefit over restart/rollback baselines;
2. a defined trusted-state lineage;
3. behavioral rather than byte-level verification;
4. explicit verifier failure domains;
5. attack-budget sweeps;
6. RPO/RTO/resource measurements;
7. statistical uncertainty on recovery error;
8. negative controls for common-mode corruption;
9. reproducible recovery logs and lineage metadata.

Only after these gates should R-I/R-J and autonomous recovery-loop experiments be attempted.

## 16. Open mathematical problems

1. Derive probabilistic self-stabilization conditions for a learned recovery controller.
2. Characterize recovery under Byzantine organs plus adaptive communication-graph attacks.
3. Bound false reintegration probability when generator and verifier share partial representations.
4. Develop task-oriented rate–distortion bounds for specialist regeneration.
5. Quantify recovery quality as a function of surviving checkpoint/teacher/memory bits.
6. Establish conditions under which independent replicas are actually failure-domain diverse.
7. Analyze repeated recovery as a stochastic dynamical system with absorbing safe states and attacker-controlled transitions.
8. Derive end-to-end recovery guarantees combining statistical prediction error with distributed-systems fault assumptions.

## 17. Primary links

- Chen et al. (2026), Byzantine agreement under reorder/channel attacks: https://arxiv.org/abs/2609.09623
- Faghih et al. (2015), automated synthesis of self-stabilizing protocols: https://arxiv.org/abs/1509.05664
- Blin, Petit & Tixeuil (2025), deterministic self-stabilizing BFS: https://arxiv.org/abs/2505.06596
- Farhadkhani et al. (2024), Byzantine robust optimization and data poisoning: https://arxiv.org/abs/2405.00491
- Fault-Tolerant Federated Reinforcement Learning: https://arxiv.org/abs/2110.14074
- GRANITE (2025), Byzantine-resilient dynamic gossip learning: https://arxiv.org/abs/2504.17471
- Certifiably Byzantine-Robust Federated Conformal Prediction: https://arxiv.org/abs/2406.01960
- Partial Hypernetworks for Continual Learning: https://arxiv.org/abs/2306.10724
- HyperInterval: https://arxiv.org/abs/2405.15444

## 18. Status

This milestone **does not validate autonomous self-healing, regeneration, or self-modification**. It narrows those claims into explicit recovery objectives, threat models, statistical tests, and protocol assumptions. The next research stage should address **evolutionary/self-modifying optimization and safety constraints only after recovery verification is experimentally bounded**.
