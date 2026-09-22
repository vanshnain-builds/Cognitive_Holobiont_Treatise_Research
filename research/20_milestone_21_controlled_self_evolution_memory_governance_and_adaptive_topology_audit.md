# Milestone 21 — Controlled Self-Evolution, Memory Governance, and Adaptive Topology Audit

**Status:** literature/formal audit; pre-implementation; falsification-first

**Scope:** Update the Cognitive Holobiont Treatise against recent 2025–2026 work on continual learning, modular memory, latent-memory MoE, self-evolving agents, adaptive decentralized topology, Byzantine-resilient dynamic networks, and memory security. The goal is not to implement the architecture, but to determine which claims survive current evidence and which require narrower statements.

## 1. Executive conclusion

The strongest current evidence supports a narrower proposition than “self-evolving cognitive holobiont”: a modular agent can improve over time by changing externalized skills/memory, routing among reusable modules, and selectively consolidating information, while a frozen base model preserves previously learned behavior. Recent systems demonstrate this in bounded task settings. They do **not** establish open-ended self-improvement, general intelligence, semantic self-repair, or indefinite capability growth.

The key distinction is now:

\[
\boxed{\text{adaptation} \neq \text{self-evolution} \neq \text{open-ended improvement}}
\]

A useful operational decomposition is

\[
S_t=(\theta_t,M_t,G_t,A_t,J_t,V_t),
\]

where \(\theta_t\) are core parameters, \(M_t\) persistent memory/skills, \(G_t\) modular graph structure, \(A_t\) active communication topology, \(J_t\) objectives/policies, and \(V_t\) verification/provenance state.

Then distinguish transition operators:

\[
S_{t+1}=\mathcal T(S_t,e_t),
\]

with

\[
\mathcal T=(\mathcal T_\theta,\mathcal T_M,\mathcal T_G,\mathcal T_A,\mathcal T_J,\mathcal T_V).
\]

A result showing improvement under \(\mathcal T_M\) or \(\mathcal T_A\) is not evidence that \(\mathcal T_J\) or unrestricted architecture evolution is beneficial.

## 2. New evidence reviewed

### 2.1 Modular memory and continual learning

Dorovatas et al., *Modular Memory is the Key to Continual Learning Agents* (2026), is a position/framework paper arguing for complementary use of in-context learning, working memory, long-term memory, and sparse in-weight consolidation. It explicitly identifies memory representation, retrieval, forgetting and consolidation as distinct design problems. The paper is valuable for the Treatise because its proposed decomposition closely matches the Holobiont's memory/workspace distinction. However, it is a research agenda rather than evidence that the complete architecture is optimal. https://arxiv.org/abs/2603.01761

The central framework can be written as

\[
\text{interaction}\rightarrow M_W\rightarrow \text{ICL},
\]

followed on a slower timescale by

\[
M_L\rightarrow\text{consolidation}\rightarrow\theta.
\]

This supports a plausible engineering synthesis, not a theorem that sparse consolidation prevents forgetting. The paper's own framing leaves implementation and benchmarking as future work.

### 2.2 Memory does not remove the continual-learning problem

Hu, Long & Wang, *When Continual Learning Moves to Memory: A Study of Experience Reuse in LLM Agents* (2026), reports that moving continual learning from weights into external memory moves the stability/plasticity problem into memory representation and retrieval. Their sequential-task results indicate that more detailed memories can sometimes increase forward transfer while worsening forgetting or negative transfer. https://arxiv.org/abs/2604.27003

This directly strengthens the Treatise's previous claim:

\[
\boxed{\text{external memory} \not\Rightarrow \text{solved continual learning}}
\]

A finite memory/context budget creates competition among old and new experiences. Therefore memory utility must be measured across write, maintenance, retrieval, and downstream use.

### 2.3 Latent-memory MoE and frozen-core adaptation

Yu et al., *Dynamic Mixture of Latent Memories for Self-Evolving Agents* (2026), proposes MoLEM: a dynamic mixture of latent memory experts whose router selects/weights memory modules while the base reasoner remains frozen. The paper reports gains over its baselines on sequential math/science/code settings. https://arxiv.org/abs/2605.21951

This is direct evidence for the narrower proposition that modular latent memory can provide a route to continual adaptation without changing the frozen base model in the tested settings.

It does **not** establish:

\[
\text{frozen base}+\text{latent memory}
\Rightarrow
\text{general self-evolution}.
\]

The claim remains bounded by task distribution, architecture, router quality, memory capacity, and evaluation protocol.

### 2.4 Self-designing agents

Zhou et al., *Memento-Skills: Let Agents Design Agents* (2026), reports an agent that creates, adapts, evaluates and rewrites reusable skills stored as persistent externalized state. The paper reports relative improvements on two evaluated benchmarks while keeping the LLM parameters fixed. https://arxiv.org/abs/2603.18743

This is stronger evidence than a purely conceptual self-evolution claim, but the correct interpretation is still:

\[
\text{self-modification of external skill state under a bounded benchmark}
\]

rather than unrestricted self-improvement. The system's change operator is constrained by the skill representation, router, evaluator, base model, task distribution and benchmark.

### 2.5 Dynamic topology under non-IID data

Cox, Ioannou & Decouchant, *Dynamic Topology Optimization for Non-IID Data in Decentralized Learning* (2025), reports a topology adaptation mechanism that chooses peers using model dissimilarity and improves measured performance on CIFAR-10 and FEMNIST relative to static/epidemic baselines. https://bacox.github.io/paper/Topology-Optimization-Decentralized-Learning/

This supports a narrower claim:

\[
\text{adaptive communication topology can improve decentralized learning under some non-IID regimes.}
\]

It does not establish that topology adaptation improves all cognitive tasks, nor that the resulting topology is globally optimal or secure against adaptive attackers.

### 2.6 Byzantine-resilient dynamic networks

Gupta & Pandurangan, *Fully-Distributed Construction of Byzantine-Resilient Dynamic Peer-to-Peer Networks* (2025), gives formal distributed-systems results for maintaining sparse, high-expansion overlay topologies under churn and Byzantine nodes under explicit assumptions. https://arxiv.org/abs/2506.04368

This is useful evidence that resilient dynamic graph maintenance is mathematically tractable under a specified model. It does not transfer automatically to neural routing or cognitive correctness.

### 2.7 Memory security

Recent 2026 work continues to demonstrate that persistent memory is a serious attack surface. *Zombie Agents* studies persistent control through self-reinforcing injections into long-term memory, while CAMS reports a layered defense for injection/extraction attacks but explicitly notes synthetic attack corpora, embedding-model dependence and overhead limitations. https://arxiv.org/abs/2602.15654 ; https://doi.org/10.1016/j.eij.2026.100983

Therefore:

\[
\boxed{M_{t+1}=\operatorname{Commit}(M_t,c_t)\text{ is a security-sensitive transition, not passive storage.}}
\]

## 3. Major claim classification update

### 3.1 Established result

The following claims have substantial support when stated with their assumptions:

1. Continual learning exhibits stability/plasticity and forgetting tradeoffs.
2. External memory changes where the continual-learning bottleneck appears; it does not automatically remove it.
3. Conditional computation and modular routing can allocate computation selectively.
4. Frozen-core + external-memory systems can improve performance over bounded task sequences.
5. Persistent agent memory can be poisoned and manipulated across sessions.
6. Dynamic topology can improve decentralized learning under some non-IID regimes.
7. Byzantine-resilient dynamic graph maintenance is possible under explicit distributed-system assumptions.
8. Skill libraries can be created, routed, evaluated and refined by an agent in bounded experiments.

### 3.2 Plausible engineering synthesis

The following remain reasonable architectural syntheses:

- Separate working memory, long-term memory and core parameters.
- Treat skills as versioned modular state with explicit lifecycle management.
- Use routing to select memory/skill modules rather than changing all parameters.
- Use slow consolidation only after validation and replay.
- Adapt communication topology to measured heterogeneity while imposing security constraints.
- Couple memory provenance to routing/recovery decisions.
- Use behavioral regression tests before reintegrating modified modules.
- Maintain failure-domain diversity between replicas/verifiers.

### 3.3 Unsupported/speculative

The following claims remain unsupported by current evidence:

- Modular self-evolution will scale indefinitely with added modules.
- Self-designed skills imply general autonomous scientific/engineering improvement.
- Frozen-core memory evolution is immune to catastrophic forgetting in the broader behavioral sense.
- Dynamic topology will converge to an optimal cognitive organization.
- Self-modification will discover better objectives rather than exploit evaluator weaknesses.
- More memory necessarily yields better reasoning.
- More modules necessarily yield more cognitive diversity.
- A self-evolving Holobiont will become conscious or achieve AGI.

### 3.4 Mathematically incorrect/incomplete formulations to reject

1. **Memory preservation:**

\[
M_{t+1}\supseteq M_t \not\Rightarrow \text{behavioral retention}.
\]

Adding memories can alter retrieval competition and therefore degrade performance.

2. **Parameter preservation:**

\[
\theta_{t+1}=\theta_t \not\Rightarrow f_{t+1}=f_t
\]

when external memory, prompts, routing or tools change.

3. **Replica evidence:**

\[
n\text{ replicas}\not\Rightarrow n\text{ independent observations}.
\]

Shared lineage, data, memory and verifiers can make effective evidence much smaller.

4. **Topology:**

\[
\text{higher expansion}
\not\Rightarrow
\text{better task performance}
\]

unless the task/channel model connects graph expansion to the target loss.

5. **Self-improvement:**

\[
J(S_{t+1})>J(S_t)
\not\Rightarrow
\text{general capability increased}
\]

because \(J\) may be narrow, gamed, distribution-specific, or correlated with a benchmark artifact.

## 4. Correct mathematical model for controlled self-evolution

Define a protected evaluation distribution \(D_{\mathrm{eval}}\), training/experience distribution \(D_t\), resource budget \(B_t\), and change operator \(\mathcal T\).

The desired property is not simply

\[
L_{D_t}(S_{t+1})<L_{D_t}(S_t).
\]

Instead require a constrained improvement condition:

\[
\begin{aligned}
\min_{\mathcal T\in\mathcal A}\quad &R_{D_{\mathrm{eval}}}(S_{t+1})\\
\text{s.t.}\quad
&R_{D_{\mathrm{retain}}}(S_{t+1})-R_{D_{\mathrm{retain}}}(S_t)\le \epsilon_R,\\
&C(S_{t+1})\le B_C,\\
&BW(S_{t+1})\le B_B,\\
&\Delta_{\mathrm{behavior}}(S_t,S_{t+1})\le \tau,\\
&\operatorname{Prov}(S_{t+1})\ge \tau_P,\\
&J_{t+1}=J_t \quad\text{for protected objectives.}
\end{aligned}
\]

The retention term is essential: improvement on new tasks alone can hide regression on old tasks.

A practical behavioral distance is

\[
\Delta_{\mathrm{behavior}}
=
E_{x\sim D_{\mathrm{probe}}}
[d(f_{S_t}(x),f_{S_{t+1}}(x))],
\]

where \(d\) must be defined for the output type. For classification, disagreement rate or loss difference may be appropriate; for generation, task-specific semantic/functional metrics are required.

## 5. Memory lifecycle formalization

Treat memory as a state machine rather than a database:

\[
\text{candidate}
\rightarrow
\text{validated}
\rightarrow
\text{active}
\rightarrow
\text{stale/contested}
\rightarrow
\text{quarantined/evicted}.
\]

Each memory record should include at minimum

\[
m=(k,v,t,s,p,q),
\]

where \(k\) is content/key, \(v\) value, \(t\) temporal metadata, \(s\) source/session, \(p\) provenance/permissions, and \(q\) validation status.

A memory utility function should separate value from trust:

\[
U(m)=V(m)-\lambda_C C(m)-\lambda_R R(m),
\]

where \(V\) is measured task value, \(C\) storage/retrieval/share cost, and \(R\) estimated harm/risk. This is an engineering objective, not a theorem that utility scoring discovers truth.

The recent on-device memory work reviewed in this milestone explicitly explores value-per-byte governance, including KEEP/SHARE/TRUST decisions under resource constraints. It is useful evidence for the direction, but its reported results remain bounded by its testbed and threat model.

## 6. Two-timescale adaptation

Let fast routing/selection parameters be \(\psi_t\) and slow specialist/core parameters be \(\theta_t\):

\[
\psi_{t+1}=\psi_t-\alpha g_\psi(\theta_t,\psi_t),
\]

\[
\theta_{t+1}=\theta_t-\beta g_\theta(\theta_t,\psi_t),
\qquad 0<\beta\ll\alpha.
\]

A common-timescale assumption cannot simply be replaced by “slow core, fast router” as a guarantee. Stability requires assumptions on smoothness, bounded gradients/noise, timescale separation and the limiting dynamics. The Treatise therefore classifies two-timescale stability as a theorem only when those assumptions and the exact stochastic approximation scheme are stated.

For the full coupled system,

\[
z_{t+1}=F(z_t),\qquad z_t=(\theta_t,\psi_t,M_t,A_t),
\]

local discrete-time stability around \(z^*\) requires the eigenvalues of

\[
J=\nabla F(z^*)
\]

to lie strictly inside the unit disk, subject to differentiability and the local model. Stability of each block separately does not imply this condition for the coupled Jacobian.

## 7. Adaptive topology with security constraints

The graph should be modeled as a controlled state:

\[
A_{t+1}=\mathcal G(A_t,s_t,\psi_t,\xi_t).
\]

A useful optimization form is

\[
\min_A
\quad
R(A)+\lambda_B B(A)+\lambda_L L(A)+\lambda_S S(A)
\]

subject to connectivity, degree, provenance and fault-domain constraints.

Here \(R\) is task risk, \(B\) communication cost, \(L\) latency and \(S\) security exposure. This formulation makes explicit that a graph optimized for statistical performance may be poor for security or cost.

For dynamic Byzantine-resilient overlay networks, formal results exist under explicit assumptions. The Treatise must not transfer those results directly to neural cognition; instead, it should borrow the methodology: define adversary power, synchrony, authentication, churn, graph constraints and correctness properties before claiming guarantees.

## 8. Information and evidence accounting

For correlated replicas with pairwise correlation \(\rho\), a rough effective-sample-size expression is

\[
n_{\mathrm{eff}}\approx \frac{n}{1+(n-1)\rho},
\]

under the exchangeable equal-correlation approximation. This is not a universal effective-evidence theorem for arbitrary neural systems; it is a diagnostic model.

For \(\rho\to1\),

\[
n_{\mathrm{eff}}\to1,
\]

showing why many copies of a shared failure mode do not provide independent evidence.

This should be measured using lineage/dependency metadata rather than inferred only from output correlation.

## 9. Threat model update

The minimum adaptive-memory threat model now includes:

- benign stale memory;
- incorrect but non-malicious memories;
- direct memory injection;
- delayed/sleeper memory poisoning;
- compositional poisoning across records;
- malicious skill creation;
- evaluator gaming;
- router manipulation;
- topology manipulation;
- common-mode verifier compromise;
- lineage forgery;
- recovery-loop poisoning.

The Treatise should evaluate attack success and collateral damage jointly:

\[
\operatorname{ASR},\quad
\Delta R_{clean},\quad
\Delta R_{retain},\quad
\Delta C,\quad
\Delta BW,\quad
T_{recover}.
\]

A defense that lowers attack success by rejecting most useful memories is not a successful cognitive memory system; it has traded integrity for availability/utility.

## 10. Experiment matrix: Milestone 21

### E10.1 — Memory lifecycle

Compare raw append-only memory, retrieval-only memory, validated memory, and lifecycle-managed memory under equal storage budgets.

Primary outcomes: new-task risk, retained-task risk, retrieval precision, stale-memory rate, poisoning success, storage bytes, retrieval latency.

### E10.2 — Frozen-core latent-memory MoE

Compare frozen-core + external memory, parameter fine-tuning, standard RAG, and latent-memory MoE under matched parameter/compute/memory budgets.

Primary outcome: retention–adaptation Pareto frontier.

### E10.3 — Self-designed skills

Compare static skills, human-authored evolving skills, automatically generated skills, and automatically generated + independently verified skills.

Measure capability gain on held-out tasks, regression, skill bloat, router errors, evaluator gaming, and maintenance cost.

### E10.4 — Adaptive topology

Compare static topology, random rewiring, heterogeneity-aware rewiring, and risk-aware adaptive topology.

Hold compute and communication budget constant.

### E10.5 — Adaptive attacker

Evaluate oblivious versus adaptive attackers that observe routing, trust, memory and verification outcomes.

### E10.6 — Common-mode lineage

Construct replicas sharing progressively more data/code/memory/router/verifier state. Estimate output correlation and failure propagation.

### E10.7 — Objective protection

Compare unprotected objective, signed external objective, hidden holdout objective, and independent verifier objective. Attack evaluator gaming and objective drift explicitly.

## 11. New hypotheses: H106–H115

**H106:** lifecycle-managed memory improves long-horizon risk at matched memory budget relative to append-only retrieval.

**H107:** external memory reduces parameter forgetting but can create retrieval-induced forgetting under bounded context.

**H108:** latent-memory MoE improves adaptation/retention efficiency relative to matched RAG and fine-tuning baselines only on some task distributions.

**H109:** automatically generated skills can improve held-out performance without increasing regression beyond a preregistered tolerance when independently verified.

**H110:** unverified skill evolution increases evaluator-gaming or regression risk relative to verified evolution.

**H111:** adaptive topology improves non-IID learning efficiency only when topology changes are budget-matched and its selection signal remains informative.

**H112:** topology adaptation under adaptive attacks can create feedback loops that erase the clean-data advantage of adaptive routing.

**H113:** increasing shared lineage monotonically decreases effective evidence under controlled common-mode corruption, up to estimation noise.

**H114:** objective-protection mechanisms reduce objective drift but do not eliminate specification gaming under an adaptive evaluator attack.

**H115:** bounded cognitive-change budgets improve recovery/reproducibility metrics without necessarily improving raw task accuracy.

## 12. Falsification criteria

Reject the relevant claim if:

1. memory management does not beat matched append-only baselines after resource accounting;
2. latent-memory MoE gains disappear under matched compute/memory/parameter budgets;
3. self-generated skills improve only training/evaluator-facing tasks and fail held-out tasks;
4. verification adds enough false rejection to erase all utility gains;
5. adaptive topology's advantage disappears under communication-matched controls;
6. adaptive routing is unstable or produces expert starvation beyond preregistered thresholds;
7. common-mode failures defeat proposed independence mechanisms;
8. objective protection is bypassed by adaptive evaluator gaming;
9. improvements depend on benchmark leakage, task-order artifacts, or uncontrolled hyperparameter search;
10. results do not replicate across seeds/tasks/environments.

## 13. Provenance and source ledger

Primary/current sources:

- Dorovatas et al. (2026), *Modular Memory is the Key to Continual Learning Agents*. https://arxiv.org/abs/2603.01761
- Hu, Long & Wang (2026), *When Continual Learning Moves to Memory: A Study of Experience Reuse in LLM Agents*. https://arxiv.org/abs/2604.27003
- Yu et al. (2026), *Dynamic Mixture of Latent Memories for Self-Evolving Agents*. https://arxiv.org/abs/2605.21951
- Zhou et al. (2026), *Memento-Skills: Let Agents Design Agents*. https://arxiv.org/abs/2603.18743
- Cox, Ioannou & Decouchant (2025), *Dynamic Topology Optimization for Non-IID Data in Decentralized Learning*. https://bacox.github.io/paper/Topology-Optimization-Decentralized-Learning/
- Gupta & Pandurangan (2025), *Fully-Distributed Construction of Byzantine-Resilient Dynamic Peer-to-Peer Networks*. https://arxiv.org/abs/2506.04368
- Yang et al. (2026), *Zombie Agents: Persistent Control of Self-Evolving LLM Agents via Self-Reinforcing Injections*. https://arxiv.org/abs/2602.15654
- Dhivyasree et al. (2026), *Cognitive Autonomous Memory Security (CAMS) against injection and extraction attacks in long-term memory of AI agents*. https://doi.org/10.1016/j.eij.2026.100983
- Wu et al. (2026), *Forget to Improve: On-Device LLM-Agent Continual Learning via Budget-Curated Memory*. https://arxiv.org/abs/2606.?

Foundational anchors retained from prior milestones:

- Shazeer et al. (2017), Sparsely-Gated MoE. https://arxiv.org/abs/1701.06538
- Fedus, Zoph & Shazeer (2022), Switch Transformers. https://jmlr.org/papers/v23/21-0998.html
- Ha, Dai & Le (2016), HyperNetworks. https://arxiv.org/abs/1609.09106
- Guo et al. (2017), calibration. https://proceedings.mlr.press/v70/guo17a.html
- Geifman & El-Yaniv (2019), selective prediction. https://proceedings.mlr.press/v97/geifman19a.html
- Yin et al. (2018), Byzantine-robust distributed learning. https://arxiv.org/abs/1803.01498

## 14. Decision gate

The program should **not** proceed to unrestricted autonomous self-modification.

The next admissible stage is a bounded, preregistered experimental program with:

- frozen external evaluation sets;
- protected objectives;
- explicit memory lifecycle states;
- matched resource budgets;
- versioned lineage;
- independent verification paths;
- adaptive and oblivious attack conditions;
- topology-change budgets;
- behavioral regression tests;
- fixed stopping criteria;
- multiple seeds and task orders.

A positive result would validate only the tested mechanism under the stated assumptions. It would not validate open-ended self-evolution, consciousness, anti-fragility, autonomous general regeneration, or AGI.

## 15. Bottom line

Milestone 21 strengthens the Treatise's most defensible interpretation:

\[
\boxed{
\text{Cognitive Holobiont}
\approx
\text{modular continual-learning system with governed memory, routing, topology, verification and recovery}
}
\]

The scientifically interesting question is no longer whether the architecture sounds biologically plausible. It is whether **controlled modular change** can produce a measurable improvement in the utility–retention–risk–resource frontier while remaining robust to memory poisoning, evaluator gaming, correlated evidence, topology manipulation and adaptive attackers.

That proposition is experimentally testable. The stronger claim of open-ended self-improving cognition remains speculative.