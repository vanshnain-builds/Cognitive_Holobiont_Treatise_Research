# Milestone 23 — Evaluation Validity, Causal Attribution, and Evidence Independence Audit

**Date:** 2026-09-24  
**Status:** research-only; falsification-first; no end-to-end validation claimed.

## 1. Purpose

Milestones 1–22 progressively constrained claims about modular learning, latent communication, memory, reliability, adversaries, recovery, topology, and controlled self-evolution. A remaining failure mode is methodological: a system can appear to improve because the **evaluation protocol itself is adaptive, contaminated, correlated, or causally ambiguous**.

Core distinction:

\[
\boxed{\text{observed improvement}\neq\text{causal mechanism}\neq\text{generalized capability}}.
\]

This milestone audits benchmark/evaluator adaptation, correlated evidence and pseudo-replication, resource confounding, latent-interface identifiability, and continual-learning evaluation.

## 2. Evidence reviewed

### 2.1 Expert routing and conditional computation

The 2026 \(\phi\)-Balancing work argues that common MoE load-balancing objectives operate on noisy mini-batch assignment statistics and proposes a population-level convex potential with an online mirror-descent/EMA implementation. This supports treating routing balance as an optimization problem rather than merely a heuristic regularizer, but does **not** establish that balanced utilization maximizes task quality. [Chen et al., 2026, arXiv:2605.15403](https://arxiv.org/abs/2605.15403).

A 2026 empirical analysis of OLMoE/OpenMoE reports non-monotone routing dynamics: early training emphasizes balance, while later training permits specialization/quality tradeoffs. A final routing statistic can therefore hide important training-phase behavior. This remains model-specific evidence, not a universal law. [Mouzouni, 2026, arXiv:2604.04230](https://arxiv.org/abs/2604.04230).

### 2.2 Latent memory and modular adaptation

MoLEM reports dynamic mixtures of latent memories with a frozen reasoning backbone and gains on sequential math/science/code tasks. This supports a bounded engineering claim that external latent modules can add task-specific capability without changing base parameters in tested settings. It does **not** establish intrinsic latent identifiability, elimination of forgetting in general, or open-ended self-evolution. [Yu et al., 2026, arXiv:2605.21951](https://arxiv.org/abs/2605.21951).

LatentMem reports role-conditioned latent memory composition for multi-agent systems. Its results support studying memory specialization and compression, but task improvement alone cannot distinguish retrieval quality, extra compute, extra context, or changed optimization. [Fu et al., 2026, arXiv:2602.03036](https://arxiv.org/abs/2602.03036).

### 2.3 Continual-learning evaluation

CurLL evaluates forward/backward transfer and skill dependencies rather than only final accuracy, supporting the Treatise requirement to report a performance matrix over the task sequence. [Kalyan et al., 2025, ACL/BabyLM](https://aclanthology.org/2025.babylm-main.20/).

Recent multimodal continual-learning benchmarking likewise emphasizes long-horizon, task-incremental, and modality-incremental evaluation. [CLeaRS, 2026](https://arxiv.org/abs/2604.00820).

Thus:

\[
\boxed{\text{final accuracy alone is insufficient evidence for continual self-improvement}.}
\]

### 2.4 Non-stationary Byzantine learning

Dynamic-regret work on Byzantine online federated learning replaces static-regret/fixed-point assumptions with a moving comparator. A representative formulation is

\[
R_T^{dyn}=\sum_{t=1}^{T} f_t(x_t)-\sum_{t=1}^{T}f_t(x_t^*),
\]

with path length

\[
P_T=\sum_{t=2}^{T}\|x_t^*-x_{t-1}^*\|.
\]

This reinforces the distinction between tracking a moving target and converging to a fixed optimum. [Dynamic Regret for Byzantine-Robust Online Federated Learning, 2026](https://doi.org/10.1109/tsp.2026.3673260).

### 2.5 Privacy and Byzantine geometry

BPFLH studies Byzantine-robust privacy-preserving FL for heterogeneous data and notes that global gradient-distance rules can confuse benign non-IID updates with malicious ones. It combines element-wise gradient comparison with CKKS encryption. The evidence supports joint evaluation of privacy and robustness, but the result remains protocol- and threat-model-specific. [BPFLH, IEEE TDSC, 2026](https://doi.org/10.1109/TDSC.2026.3661522).

### 2.6 Representation identifiability

ICLR 2026 work distinguishes **statistical identifiability** (stability across runs) from **structural identifiability** (alignment with an underlying ground truth), and proves near-identifiability under explicit assumptions. This constrains Treatise language around “shared latent semantics.” [Nelson et al., ICLR 2026](https://proceedings.iclr.cc/paper_files/paper/2026/hash/f67e5f99b23b108a3a6665f410034bcd-Abstract-Conference.html).

Multi-view causal representation work likewise establishes identifiability only under explicit partial-observability assumptions and up to stated transformations. [Yao et al., 2024](https://arxiv.org/abs/2311.04056).

Therefore:

\[
\boxed{\text{latent agreement}\not\Rightarrow\text{semantic identity}.}
\]

### 2.7 Causal intervention

Causal inference over time provides a formal route for distinguishing intervention effects from observational association in dynamical systems. This motivates intervention-based ablations rather than relying only on correlations among routing, memory and performance. [Cinquini et al., AAAI 2025](https://ojs.aaai.org/index.php/AAAI/article/view/33626).

### 2.8 Agent evaluation and reliability

The Princeton HAL effort illustrates the need for standardized, cost-aware and reliability-focused evaluation, and documents that benchmark scores can change materially with scaffolds and grading choices. This reinforces a Treatise requirement: the scaffold is part of the evaluated system boundary. [HAL, Princeton, ICLR 2026](https://hal.cs.princeton.edu/).

## 3. Formal evaluation model

Let a Holobiont configuration be

\[
C=(\theta,M,G,A,\pi,V),
\]

where \(\theta\) are parameters, \(M\) memory/skills, \(G\) the module graph, \(A\) the active topology, \(\pi\) the routing/decision policy, and \(V\) the verification policy.

For task distribution \(D\) and explicit attack/environment family \(\mathcal A\), define

\[
J(C;D,\mathcal A)=E_{a\sim\mathcal A,\,x\sim D}[L(C;x,a)].
\]

A mechanism \(m\) should be compared with a controlled intervention, not an arbitrary baseline. Conceptually,

\[
\tau_m=E[Y\mid do(m=1)]-E[Y\mid do(m=0)].
\]

For sequential systems,

\[
\tau_m(T)=E\left[\sum_{t=1}^{T}w_tY_t\mid do(m=1)\right]
-E\left[\sum_{t=1}^{T}w_tY_t\mid do(m=0)\right].
\]

These are causal quantities only under appropriate intervention, randomization, consistency, and measurement assumptions.

## 4. Pseudo-replication and evidence independence

Under an equal-correlation approximation for \(n\) specialists with common marginal variance \(\sigma^2\),

\[
\operatorname{Var}(\bar X)=\frac{\sigma^2}{n}\left[1+(n-1)\rho\right].
\]

The corresponding diagnostic effective sample size is

\[
n_{eff}\approx\frac{n}{1+(n-1)\rho}.
\]

This is not a universal dependence correction.

Consequences:

- specialists copied from one checkpoint are not independent replications;
- modalities generated by one shared encoder are not independent evidence streams;
- routers sharing one memory store can share common-mode errors;
- evaluation seeds do not create independent datasets.

The Treatise should report nominal replicate count **and** a dependency/lineage graph.

## 5. Resource-matched causal ablations

A recurring confound is

\[
\text{Holobiont gain}=\text{mechanism gain}+\text{extra resources}.
\]

Comparisons must match or explicitly account for

\[
B=\text{communication bytes},\quad F=\text{FLOPs},\quad P=\text{active parameters},
\]

\[
R=\text{retrieval count},\quad L=\text{latency},\quad E=\text{energy},\quad S=\text{memory bytes}.
\]

Use a Pareto vector

\[
\mathcal P=(\text{risk},B,F,P,R,L,E,S)
\]

rather than accuracy alone. A mechanism is not a demonstrated efficiency improvement if it gains accuracy through uncontrolled compute, memory, context, or inference-time search.

## 6. Evaluation leakage and adaptive evaluators

For a self-evolving system, the evaluation function can itself become an optimization target. Repeatedly exposing validation feedback creates a selection process of the form

\[
\max_{C_t}\hat J_{val}(C_t),
\]

which can overfit the finite validation set instead of improving the intended distribution.

The Treatise therefore requires:

1. hidden test sets unavailable to adaptation;
2. fresh post-freeze task instances;
3. evaluator separation from training/reflection memory;
4. provenance logs for every evaluation query;
5. fixed primary metrics before experiments;
6. preregistered acceptance criteria where feasible;
7. a held-out adversarial suite not used for selection.

For adaptive agents, evaluator and scaffold changes are interventions and must be recorded.

## 7. Latent-interface identifiability audit

For

\[
z_A=E_A(h_A),\qquad z_B=E_B(h_B),\qquad \hat h_B=D_B(z_A),
\]

high cosine similarity or reconstruction should not be called a “semantic shared latent space.”

Use evidence levels:

- **L0 — geometric compatibility:** retrieval/reconstruction improves.
- **L1 — task sufficiency:** \(R(Y\mid Z)\le\epsilon\).
- **L2 — cross-run stability:** alignment persists across seeds/checkpoints under a stated transformation class.
- **L3 — intervention robustness:** latent manipulation has predictable downstream effects.
- **L4 — structural/causal identifiability:** theory or validated structure establishes latent correspondence under explicit assumptions.

Reserve “semantic equivalence” for L3/L4 evidence, not L0.

## 8. Continual-learning metric correction

For tasks \(1,\dots,T\), let \(a_{i,j}\) be performance on task \(j\) after learning task \(i\).

\[
MFN=\frac1T\sum_{j=1}^{T}a_{T,j},
\qquad
MFT=\frac1T\sum_{i=1}^{T}a_{i,i},
\]

and a common backward-transfer/forgetting statistic is

\[
BWT=\frac1{T-1}\sum_{j=1}^{T-1}(a_{T,j}-a_{j,j}).
\]

Forward transfer requires an explicit pre-training/control definition and should not be inferred from final accuracy.

Resource-normalized empirical efficiencies may be reported as

\[
\eta_F=\frac{MFN-MFN_0}{\Delta F},\qquad
\eta_B=\frac{MFN-MFN_0}{\Delta B},
\]

where \(MFN_0\) is the matched baseline. These are empirical ratios, not information-theoretic invariants.

## 9. Non-stationary evaluation

If

\[
D_t\neq D_{t+1},
\]

a fixed test score does not establish tracking.

Define windowed risk

\[
R_t=E_{(x,y)\sim D_t}[L_t(C_t;x,y)]
\]

and, where an online-learning formulation applies, compare against an explicit moving comparator.

Report adaptation delay, recovery after drift, dynamic regret when appropriate, retention on stable tasks, false adaptation under null drift, and adaptation resource cost.

## 10. Claim ledger

### Established result (under stated assumptions)

- Expert routing balance can be formulated as an optimization problem; current MoE work provides population-level and dynamic analyses.
- Continual learning requires separate learning, retention, transfer and resource measurements.
- Representation identifiability is conditional and may involve nontrivial ambiguity classes.
- Byzantine/privacy results are threat-model and heterogeneity dependent.
- Causal effects require interventions and assumptions, not correlation alone.
- Evaluation scaffolds and grading procedures can materially affect agent results.

### Plausible engineering synthesis

- Treat evaluation protocol as part of the Holobiont system boundary.
- Dependency-aware evidence accounting.
- Causal ablation matrices for routing/memory/workspace mechanisms.
- Hidden evaluator partitions and evaluator provenance.
- Resource-matched comparisons across communication, compute, memory and latency.
- Latent-interface evidence levels L0–L4.
- Dynamic-regret plus retention reporting for non-stationary self-evolution.

### Unsupported/speculative

- A higher benchmark score proves a new cognitive mechanism.
- Independent-looking specialists provide independent evidence.
- A shared latent embedding is universally semantic.
- Public validation improvement by a self-evolving agent proves general improvement.
- More elaborate evaluation scaffolds necessarily produce better cognition.
- A Pareto improvement on one benchmark establishes general intelligence improvement.

### Mathematically incorrect/incomplete without additional assumptions

\[
\text{accuracy gain}\Rightarrow\text{causal mechanism gain}
\]

\[
n\text{ specialists}\Rightarrow n\text{ independent samples}
\]

\[
\operatorname{cos}(z_i,z_j)\approx1\Rightarrow z_i,z_j\text{ semantically equivalent}
\]

\[
MFN\uparrow\Rightarrow BWT\geq0
\]

\[
R_t\downarrow\Rightarrow\text{better tracking under drift}
\]

\[
\text{held-out benchmark success}\Rightarrow\sup_{a\in\mathcal A}R(C;a)\leq\tau.
\]

## 11. New experiment matrix: E12

### E12.1 — Causal routing ablation

Randomize routing mechanism while holding expert set, active FLOPs, token budget and memory fixed; estimate \(\tau_{routing}\).

### E12.2 — Memory-vs-compute control

Compare latent-memory, raw-memory and extra-compute baselines at matched inference budget.

### E12.3 — Dependency-aware replication

Vary shared checkpoint/data/encoder lineage while keeping nominal specialist count fixed; estimate how uncertainty changes with measured dependence.

### E12.4 — Hidden-evaluator self-evolution

Compare public-validation adaptation with a fully hidden evaluator; measure evaluator overfitting and transfer.

### E12.5 — Latent identifiability

Test cross-seed alignment, transformation classes, intervention predictability and task sufficiency separately.

### E12.6 — Non-stationary tracking

Inject controlled distribution shifts with known change points and compare adaptation delay, dynamic regret and false adaptation.

### E12.7 — Scaffold intervention

Hold model parameters fixed and change tools/planner/memory scaffold independently to estimate scaffold causal effect.

### E12.8 — Reliability-evaluator attack

Let an adaptive adversary optimize against the public reliability protocol while evaluation uses hidden attacks; measure degradation under white-box adaptation.

## 12. New hypotheses H125–H134

- **H125:** Apparent Holobiont gains shrink when communication/compute/memory are matched against strong dense and sparse baselines.
- **H126:** Shared lineage materially reduces effective evidence relative to nominal specialist count.
- **H127:** Public evaluator adaptation produces larger gains on the public set than on a hidden post-freeze set.
- **H128:** Latent alignment scores overestimate semantic equivalence when evaluated without intervention tests.
- **H129:** Latent-memory gains persist partly after compute matching, but the effect size is smaller than the unmatched comparison.
- **H130:** Adaptive topology improves non-IID performance only within a resource/security Pareto region; beyond it, routing overhead dominates.
- **H131:** Non-stationary adaptation can improve dynamic regret while increasing false adaptation and forgetting on stationary tasks.
- **H132:** Reliability policies optimized on known attacks degrade more under adaptive hidden attacks than under matched static attacks.
- **H133:** Scaffold changes can produce performance differences comparable to or larger than the mechanism being studied.
- **H134:** Dependency-aware evidence estimates reduce overconfident recovery/consensus decisions compared with naive replica counting.

## 13. Open problems

1. How should causal effects be estimated when the system itself changes routing during the experiment?
2. What dependence model is adequate for heterogeneous specialists sharing some but not all lineage?
3. How can latent-space interventions be made semantically meaningful without circular task supervision?
4. What is the right resource currency for agentic systems: FLOPs, wall-clock, energy, tokens, or a multi-objective budget?
5. How should hidden evaluators be generated so they remain representative without becoming predictable?
6. Can robust conformal/selective methods retain guarantees when the evaluator is adaptively attacked?
7. How can genuine transfer be separated from retrieval leakage and benchmark memorization?
8. Can dynamic-regret guarantees extend to jointly adaptive topology and memory systems with Byzantine participants?

## 14. Research decision

The Treatise now treats **evaluation validity as an architectural dependency**, not a post-hoc reporting step.

The strongest defensible methodological criterion is

\[
\boxed{\text{A Holobiont mechanism is supported only when its effect survives causal intervention, resource matching, hidden evaluation, dependency analysis, and the stated threat/distribution model.}}
\]

This is a research criterion, not a validation result.

## 15. Status

Milestone 23 does **not** validate the Cognitive Holobiont, self-evolution, consciousness, AGI, autonomous recovery, or general intelligence. It strengthens the falsification protocol by making causal attribution, evaluator independence, evidence dependence, latent identifiability, resource matching and non-stationary evaluation explicit requirements before component-level results can be interpreted as evidence for the Treatise.
