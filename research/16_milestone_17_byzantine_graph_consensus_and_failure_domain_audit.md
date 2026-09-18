# Milestone 17 — Byzantine Graph Dynamics, Consensus, and Failure-Domain Audit

**Status:** research/pre-implementation  
**Date:** 2026-09-18  
**Scope:** dynamic communication graphs, Byzantine robustness, consensus, graph mixing, over-squashing/long-range communication, federated heterogeneity, failure-domain diversity, and the boundary between distributed agreement and semantic correctness.

## 1. Research question

The Treatise increasingly models organs as a dynamically routed graph rather than a fixed ensemble. Milestone 17 asks:

> When communication topology, participant reliability, and model heterogeneity all vary, what can actually be guaranteed by consensus or robust aggregation, and what remains an empirical engineering hypothesis?

Central correction:

\[
\boxed{\text{consensus} \neq \text{truth} \neq \text{recovery correctness}}
\]

Agreement is a property of outputs or states relative to a protocol. Correctness requires an external specification, honest-reference assumptions, or a valid statistical/semantic criterion.

## 2. Evidence reviewed

### 2.1 Byzantine federated learning under heterogeneity

Byzantine-robust FL research shows that arbitrary client updates can damage learning and that robustness is especially difficult when honest clients are statistically heterogeneous. Recent work explicitly formulates adaptive aggregation weights and studies Byzantine resilience under heterogeneous data:

- Parsa, Daghestani, Teixeira & Johansson (2025), *Byzantine-Robust Federated Learning with Learnable Aggregation Weights*: https://arxiv.org/abs/2511.03529
- Kang et al. (2024), *Certifiably Byzantine-Robust Federated Conformal Prediction*: https://arxiv.org/abs/2406.01960
- Farhadkhani et al. (2024), Byzantine-robust learning under heterogeneous data: https://arxiv.org/abs/2405.00491

These support explicit threat models and robustness mechanisms. They do not establish that a general Holobiont can identify Byzantine organs from behavior alone.

### 2.2 Dynamic graphs and adversarial communication

Recent distributed-learning work studies Byzantine resilience when communication/peer sampling is dynamic, making the topology itself part of the threat surface. This is directly relevant to an adaptive Holobiont router:

- GRANITE (2025): https://arxiv.org/abs/2504.17471

The key design consequence is that routing policy must be included in the threat model. A robust node on an attacker-controlled communication graph is not automatically a robust system.

### 2.3 Graph information bottlenecks

GNN theory identifies over-squashing as a limitation on long-range information propagation. A recent survey summarizes the relationship between graph geometry, bottlenecks, rewiring, normalization, and long-range interactions:

- *Over-Squashing in Graph Neural Networks: A Comprehensive Survey* (2023): https://arxiv.org/abs/2308.15568

However, oversmoothing is not universal. Epping et al. (2024) derive regimes in which deep GCNs avoid oversmoothing, demonstrating that blanket claims of inevitable representation collapse are incorrect:

- *Graph Neural Networks Do Not Always Oversmooth* (2024): https://arxiv.org/abs/2406.02269

Therefore the Treatise should not equate graph depth with inevitable communication failure; topology, initialization, architecture, and task determine the regime.

### 2.4 Federated system/model heterogeneity

Federated learning has both statistical and system heterogeneity. FedP3 explicitly formulates model personalization under heterogeneous memory, compute, and bandwidth constraints:

- Yi et al. (2024), *FEDP3: Federated Personalized and Privacy-Friendly Network Pruning under Model Heterogeneity*: https://arxiv.org/abs/2404.09816

This supports treating resource heterogeneity as a first-class constraint rather than assuming identical organs.

## 3. Formal dynamic Holobiont graph

Let the system at time \(t\) be a directed weighted graph

\[
G_t=(V,E_t,A_t),
\]

where \(V\) are organs, \(E_t\) active communication edges, and \(A_t=[a_{ij,t}]\) nonnegative edge weights.

Let the normalized communication operator be \(P_t\), with

\[
P_t\mathbf 1=\mathbf 1
\]

for a row-stochastic protocol.

A generic consensus iteration is

\[
x_{t+1}=P_t x_t.
\]

For a fixed primitive stochastic matrix \(P\), standard consensus analysis gives convergence to a rank-one limit under appropriate connectivity/aperiodicity conditions. For time-varying graphs, stronger joint-connectivity and stochastic-matrix conditions are required.

The Treatise must therefore avoid writing

\[
\lambda_2(L) > 0 \Rightarrow \text{Holobiont converges}
\]

without specifying whether \(L\) is fixed, symmetric, time-varying, directed, weighted, and whether the actual update is a Laplacian consensus process.

## 4. Spectral-gap correction

For a fixed symmetric stochastic operator with eigenvalues

\[
1=\lambda_1>\lambda_2\ge\cdots,
\]

contraction of disagreement can be bounded in appropriate norms by the magnitude of the second-largest eigenvalue. For example, under a standard doubly-stochastic linear iteration,

\[
\|x_t-\bar x\mathbf 1\|
\le
|\lambda_2|^t\|x_0-\bar x\mathbf 1\|.
\]

This is a mathematical statement about a particular linear process.

It does **not** imply

\[
\text{spectral gap} \equiv \text{cognitive speed}
\]

or that larger spectral gap always improves a learned system. Communication latency, computation, stochastic gradients, nonlinear updates, changing topology, and semantic task structure are outside the simple bound.

## 5. Byzantine consensus vs Byzantine learning

Classical Byzantine agreement asks honest parties to agree despite faulty parties under explicit protocol assumptions. Byzantine learning asks for useful model behavior despite malicious or corrupted updates. These are related but not interchangeable problems.

For a robust aggregator \(A\), a generic learning update is

\[
\theta_{t+1}=\theta_t-\eta A(g_{1,t},\ldots,g_{n,t}).
\]

A Byzantine-resilience theorem must state the allowed set of adversarial gradients/updates, honest-gradient distribution, heterogeneity, dimension, optimization assumptions, and communication protocol.

The invalid shortcut is:

\[
\text{Byzantine agreement theorem}
\Rightarrow
\text{Byzantine-robust neural learning theorem}.
\]

A consensus protocol can make honest nodes agree on a poisoned value if the reference/value semantics are compromised.

## 6. Honest heterogeneity vs Byzantine deviation

Let honest update distribution be \(H_t\) and adversarial update distribution be \(B_t\). Robustness requires a separation assumption of some form, but the Treatise should not assume that every honest update is close to one central vector.

A useful abstract condition is

\[
\operatorname{dist}(g_i,H_t)\le \rho_h
\quad\text{for honest }i,
\]

while an attacker may choose

\[
\operatorname{dist}(g_i,H_t)\gg \rho_h.
\]

If honest specialists naturally occupy multiple modes, a single-center rule can incorrectly quarantine legitimate specialization.

Thus the system should evaluate robustness against both:

1. **unimodal honest heterogeneity**, and
2. **multimodal honest specialization**.

## 7. Dynamic routing is itself a state variable

Let the router choose edges according to

\[
A_t=\mathcal R(s_t;\psi_t),
\]

where \(s_t\) contains task state and reliability information.

Then the system state is not merely model state \(\theta_t\), but

\[
S_t=(\theta_t,\psi_t,A_t,w_t,M_t,V_t).
\]

A compromised router can therefore:

- isolate an honest organ;
- concentrate traffic on a malicious organ;
- create correlated evidence;
- starve an expert so that its reliability estimate decays;
- repeatedly switch topology and prevent convergence;
- route verifier queries through the same failure domain.

This makes routing manipulation a system-level attack rather than a local model attack.

## 8. Graph diversity is not statistical independence

Let replicas \(R_1,\ldots,R_m\) share common ancestor artifact \(Z\). Conditional on a corruption event \(C\), they may remain highly correlated:

\[
P(R_1,\ldots,R_m\mid C)
\ne
\prod_j P(R_j\mid C).
\]

Therefore replica count is insufficient as an evidence metric.

The dossier introduces a failure-domain matrix

\[
D_{ij}=\text{shared failure-domain score}(i,j),
\]

covering data, checkpoint lineage, code, model family, keys, infrastructure, router, memory, and verifier dependencies.

A practical diversity objective can then penalize redundant evidence:

\[
\max \;U(\mathcal R)-\lambda\sum_{i<j}D_{ij}.
\]

This is an engineering objective, not a theorem that low dependency guarantees correctness.

## 9. Information bottleneck on graph communication

If many upstream nodes must communicate through a narrow intermediary, the intermediary may have to compress information.

Let incoming representations be \(Z_1,\ldots,Z_m\) and intermediary state be \(W\). If a task requires information spread across many sources, finite \(W\) can create an information bottleneck.

The Treatise should therefore test

\[
R(K,B,G)
\]

as task risk as a function of workspace capacity \(K\), communication budget \(B\), and graph topology \(G\).

There is no universal monotonic law that increasing connectivity always reduces risk: additional edges can increase communication cost, create attack surfaces, and introduce noisy or redundant messages.

## 10. Privacy and robustness are different objectives

Federated learning can keep raw data local, but locality does not imply privacy. Model updates, gradients, memory artifacts, and side channels can leak information.

Likewise, Byzantine robustness does not imply privacy.

The Treatise therefore separates:

\[
\text{privacy risk},\quad
\text{integrity risk},\quad
\text{availability risk},\quad
\text{model-performance risk}.
\]

A design claiming all four must provide separate threat models and measurements.

## 11. Consensus and semantic correctness

Let each organ produce a proposition \(q_i\). Consensus may produce

\[
q_1=\cdots=q_n=q.
\]

Correctness requires an external criterion

\[
q\in Q^*.
\]

Consensus only establishes agreement under the protocol; it does not establish \(q\in Q^*\).

For the Holobiont, an external correctness mechanism can include:

- ground-truth labels;
- trusted environment feedback;
- independently generated tests;
- formal invariants;
- calibrated selective-risk guarantees under their assumptions.

Without one of these, consensus should be logged as an **agreement signal**, not a truth signal.

## 12. Graph experiment matrix

| Experiment | Graph manipulation | Primary metric | Main control |
|---|---|---|---|
| G-A | fixed sparse vs dense graph | task risk / communication | matched bandwidth |
| G-B | rewiring rate sweep | convergence + utility | fixed edge budget |
| G-C | honest multimodal specialists | false quarantine | oracle honest labels |
| G-D | Byzantine edge manipulation | attack success | fixed topology |
| G-E | malicious traffic concentration | task risk | random routing |
| G-F | common-lineage replicas | false confidence | independent lineage |
| G-G | over-squashing bottlenecks | transfer/risk vs hop count | rewired graph |
| G-H | verifier routing attack | false approval | independent verifier path |
| G-I | privacy/robustness tradeoff | utility + privacy + attack success | separate controls |
| G-J | topology oscillation | time-to-stable-state | fixed topology |

Every experiment must report clean utility, communication bytes, active edges, latency, compute, attack budget, false quarantine, false acceptance, recovery time, and uncertainty intervals.

## 13. Hypotheses H72–H79

**H72 — dynamic topology cost:** adaptive rewiring improves task utility only when its benefit exceeds added communication/latency and attack-surface cost.

**Falsifier:** adaptive routing dominates or matches fixed topology at lower total cost across the preregistered workload, or provides no measurable utility benefit.

**H73 — honest multimodality:** single-center Byzantine detection has higher false-quarantine rates on genuinely multimodal specialist populations than a distribution-aware baseline.

**Falsifier:** no measurable difference under matched clean utility.

**H74 — routing attack surface:** manipulating the router can degrade reliability even when individual specialists remain uncompromised.

**Falsifier:** router manipulation fails to produce a statistically significant degradation under the specified attack budget.

**H75 — failure-domain diversity:** lineage/data/code-diverse replicas reduce correlated false acceptance compared with additional same-lineage replicas at matched storage cost.

**Falsifier:** no reduction or an unfavorable cost-adjusted tradeoff.

**H76 — graph bottleneck:** narrow communication cuts increase task risk for tasks requiring distributed information integration.

**Falsifier:** no bottleneck effect across the preregistered tasks or a capacity-matched baseline performs equivalently.

**H77 — spectral correction:** spectral-gap-based convergence predicts only the corresponding linear communication dynamics, not end-to-end cognitive utility or semantic correctness.

**Falsifier:** the same spectral statistic reliably predicts both across diverse nonlinear learned systems without additional task/system variables.

**H78 — privacy/robustness separation:** improving Byzantine robustness does not automatically improve privacy, and privacy mechanisms can alter robustness/utility tradeoffs.

**Falsifier:** a single intervention consistently improves both without measurable tradeoff across the specified threat models.

**H79 — consensus correctness gap:** high consensus can coexist with common-mode incorrectness.

**Falsifier:** under all specified common-mode corruptions, consensus remains a calibrated proxy for correctness at the preregistered operating point.

## 14. Claim classification

### Established

- Byzantine robustness requires explicit threat/heterogeneity assumptions.
- Distributed consensus has formal convergence results under specified protocol and graph assumptions.
- Dynamic communication changes the failure surface and can require stronger analysis.
- Graph bottlenecks can restrict long-range message passing in neural graph systems.
- Federated learning faces statistical and system heterogeneity.
- Privacy and Byzantine robustness are distinct properties.

### Plausible engineering synthesis

- Treating the router as a security-critical state variable.
- Failure-domain-diverse evidence paths.
- Distribution-aware quarantine rather than single-center anomaly rules.
- Joint optimization of topology, communication, risk, and compute.
- Explicit graph bottleneck diagnostics for the Holobiont workspace.

### Unsupported/speculative

- Consensus creates semantic truth.
- More communication edges always improve cognition.
- More replicas automatically create independent evidence.
- Dynamic routing necessarily improves intelligence.
- A particular spectral statistic universally predicts cognitive performance.
- Byzantine robustness can be inferred from consensus alone.

### Mathematically incorrect/incomplete

- \(\lambda_2(L)>0\Rightarrow\) arbitrary Holobiont convergence.
- \(n>3f\Rightarrow\) arbitrary Holobiont learning/recovery correctness.
- consensus \(\Rightarrow\) truth.
- replica count \(\Rightarrow\) independent evidence.
- graph connectivity \(\Rightarrow\) lower task risk without a task/channel model.
- privacy \(\Rightarrow\) Byzantine robustness.

## 15. Decision gate before distributed implementation

Do not implement a full adaptive graph until the B0–B4 single-topology baselines establish:

1. measurable benefit from communication;
2. measurable benefit from workspace integration;
3. measurable benefit from selective memory;
4. measurable reliability benefit from the B4 layer;
5. explicit attack robustness at each interface;
6. resource-normalized comparisons.

The first distributed prototype should use a **fixed topology control** and a **single-variable topology intervention**. Otherwise simultaneous changes in routing, memory, specialists, and graph structure make causal attribution impossible.

## 16. Open mathematical problems

1. Derive end-to-end bounds linking time-varying graph mixing, bounded message channels, and task risk.
2. Characterize robust aggregation under multimodal honest specialist distributions.
3. Formalize failure-domain diversity as a dependency graph and derive useful reliability bounds without assuming independence.
4. Establish conditions under which routing adaptation preserves stability of the coupled learning/communication process.
5. Derive rate–distortion bounds for task-relevant distributed representations over changing graphs.
6. Determine when robust privacy mechanisms preserve Byzantine resilience and when they fundamentally conflict.
7. Define probabilistic self-stabilization for a learned nonlinear system with changing topology.

## 17. Provenance and evidence policy

Primary research is preferred for decisive claims; surveys are used to identify competing explanations and open problems. All equations above are stated either as standard mathematical forms under explicit assumptions or as proposed operational definitions. Proposed definitions are not presented as established theorems.

## 18. Status

Milestone 17 does **not** validate distributed cognition, consensus-based correctness, autonomous routing, or self-healing. It narrows the Treatise to testable graph, aggregation, privacy, and failure-domain hypotheses and identifies the assumptions required before any distributed-systems theorem can be claimed.
