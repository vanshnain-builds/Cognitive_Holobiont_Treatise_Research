# Milestone 22 — Adaptive Memory/Skill Security and Non-Stationary Robustness Audit

**Status:** literature/formal audit; pre-implementation; falsification-first

## Executive conclusion

Fresh 2026 evidence narrows the Treatise further. Modular external state, latent-memory modules, adaptive routing and bounded skill evolution can improve selected sequential tasks, but do not establish open-ended self-improvement or safe self-modification.

The key new security finding is that the **experience-to-skill transformation itself is an attack surface**. Recent work reports persistent skill backdoors that survive removal of source records, and attacks based on locally correct but non-transferable experiences. The security boundary must therefore include:

\[
E_t\rightarrow M_t\rightarrow K_t\rightarrow \text{persistent behavior},
\]

not only memory retrieval.

A second major finding is that Byzantine robustness must be evaluated under non-stationarity and partial participation. A globally safe Byzantine fraction does not imply a safe active subgraph, and static convergence does not imply tracking of a moving optimum.

## 1. New evidence

### Skill-to-behavior persistence

*SkillJack: Persistent Skill Backdoors in Self-Evolving Agents* (2026) reports attacks that exploit experience-to-skill transformation, including sanitization/whitewashing, cross-layer promotion and persistence after source-record removal. https://arxiv.org/abs/2608.03509

Therefore the prior implication

\[
\text{delete bad memory}\Rightarrow\text{remove bad behavior}
\]

is causally incomplete. Derived skills/modules must have provenance and rollback lineage.

### Locally correct but unsafe experiences

*OEP: Poisoning Self-Evolving LLM Agents via Locally Correct but Non-Transferable Experiences* (2026) reports black-box attacks using locally correct but non-transferable experiences that can bias reflective generalization. https://arxiv.org/abs/2605.18930

Validation must therefore distinguish:

\[
V(m)=\big(V_{local}(m),V_{transfer}(m),V_{safety}(m)\big).
\]

Low local loss does not imply low transfer or safety risk.

### Meta-evolution of memory architecture

*MemEvolve: Meta-Evolution of Agent Memory Systems* proposes evolving both experience and the memory architecture itself (encode/store/retrieve/manage). https://arxiv.org/abs/2512.18746

This adds a new state variable: the memory interpreter. Hence

\[
M_{t+1}=M_t\not\Rightarrow f_{t+1}=f_t
\]

when the encoder, retriever or manager changes.

### Dynamic Byzantine robustness

*Dynamic Regret for Byzantine-Robust Online Federated Learning* (2026) studies adversarial online learning without IID assumptions and targets dynamic regret under path-length conditions. https://doi.org/10.1109/tsp.2026.3673260

The appropriate moving-environment quantity is

\[
R_T^{dyn}=\sum_{t=1}^T f_t(x_t)-\sum_{t=1}^T f_t(x_t^*),
\]

with comparator path length

\[
P_T=\sum_{t=2}^T\|x_t^*-x_{t-1}^*\|.
\]

This is a tracking statement, not a fixed-point convergence theorem.

### Partial participation

*Delayed Momentum Aggregation* (2026) shows that partial participation can produce rounds in which Byzantine clients dominate the sampled set even when the global population is not majority Byzantine. https://arxiv.org/abs/2509.02970

For a Holobiont active set \(A_t\):

\[
\frac{|B|}{|V|}<\alpha\not\Rightarrow\frac{|B_t|}{|A_t|}<\alpha.
\]

Adaptive routing can therefore create a locally Byzantine-dominated subgraph.

### Privacy/robustness interaction

BPFLH (IEEE TDSC, 2026) studies Byzantine-robust privacy-preserving FL for heterogeneous data using element-wise gradient dissimilarity and homomorphic encryption. https://doi.org/10.1109/TDSC.2026.3661522

The Treatise-level conclusion is methodological: privacy transformations can change the evidence geometry used by a Byzantine detector. Thus, in general,

\[
\mathrm{Robustness}(\mathcal A)\neq\mathrm{Robustness}(\mathcal A\circ\mathcal P).
\]

Privacy must be part of the measured threat model.

### Workspace evidence

*Multimodal Dreaming* reports benefits from doing world-model simulation in a global-workspace latent space in a specific RL setting, including missing-modality robustness. A 2025 open-access paper separately discusses selection-broadcast cycles for real-time multimodal processing. https://arxiv.org/abs/2502.21142 ; https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2025.1607190/full

These support a **plausible engineering synthesis**, not necessity, consciousness, or general superiority of global workspace architectures.

## 2. Claim classification

### Established, with assumptions

1. External memory and modular skills can support bounded continual adaptation.
2. Persistent memory can be poisoned across sessions.
3. Experience-to-skill transformation is a distinct persistence attack surface.
4. Local correctness does not guarantee safe transfer/generalization.
5. Byzantine robustness is harder under non-IID, non-stationary and partial-participation settings.
6. Dynamic-regret formulations are appropriate for changing environments under explicit assumptions.
7. Global Byzantine fraction does not imply active-subgraph safety.
8. Privacy mechanisms can alter evidence geometry used for robustness.
9. Latent multimodal workspaces can be useful in tested RL/multimodal settings.

### Plausible engineering synthesis

- Separate memory, skill and interpreter provenance.
- Require transfer validation before skill promotion.
- Maintain a dependency DAG from experience to derived modules.
- Audit active-subgraph Byzantine composition, not only global fractions.
- Use dynamic-regret/tracking metrics under drift.
- Treat privacy mechanisms as part of the detector threat model.
- Freeze protected evaluation channels while allowing bounded evolution.
- Roll back derived skills, not only source memories.

### Unsupported/speculative

- Deleting poisoned memories necessarily removes downstream effects.
- Locally validated experiences are safe for general reuse.
- More self-generated skills monotonically increase capability.
- Adaptive topology automatically avoids Byzantine-dominated subgraphs.
- Dynamic-regret control implies cognitive convergence.
- Privacy-preserving aggregation automatically preserves Byzantine detectability.
- Global workspace is sufficient for general multimodal cognition.
- Meta-evolving memory necessarily improves rather than overfits evaluation.

### Mathematically incorrect/incomplete

**Memory deletion:**
\[
M_i\notin M_{t+1}\Rightarrow f_{t+1}\text{ no longer contains its effect}
\]

fails when information has already entered skills, parameters, summaries or caches.

**Global fraction:**
\[
|B|/|V|<\alpha\not\Rightarrow |B_t|/|A_t|<\alpha.
\]

**Static convergence:**
\[
\text{convergence to }x^*\not\Rightarrow x_t\text{ tracks }x_t^*.
\]

**Local validation:**
\[
L_{D_{local}}(m)\text{ small}\not\Rightarrow L_{D_{transfer}}(m)\text{ small}.
\]

**Stable memory bytes:**
\[
M_{t+1}=M_t\not\Rightarrow f_{t+1}=f_t
\]

if the memory interpreter changes.

## 3. Corrected controlled self-evolution model

Expand the state to

\[
S_t=(\theta_t,M_t,K_t,G_t,A_t,J_t,V_t),
\]

where \(K_t\) denotes derived skills/modules.

Use a quarantine/promotion pipeline:

\[
E_t\rightarrow Q_t\rightarrow M_t\rightarrow K_t.
\]

A skill promotion gate can be written:

\[
\mathrm{Promote}(e)=1\iff
\begin{cases}
R_{local}(e)\le\tau_L,\\
R_{transfer}(e)\le\tau_T,\\
R_{safety}(e)\le\tau_S,\\
P(e)\ge\tau_P.
\end{cases}
\]

These are operational controls, not universal guarantees.

For a derived skill \(k\), maintain lineage

\[
\mathcal L(k)=\{e_1,\ldots,e_m,m_1,\ldots,r_j\}
\]

covering experiences, memories, prompts/rules, model versions and retrievers. Rollback becomes lineage-aware rather than memory-only.

## 4. Active-subgraph safety

For routing topology \(A_t\), define

\[
\beta_t=\frac{|B_t|}{|A_t|}.
\]

Track tail risk and unsafe exposure:

\[
\Pr(\beta_t>\tau_B),
\qquad
D_{unsafe}=\sum_t\mathbf 1[\beta_t>\tau_B].
\]

This should replace a single global Byzantine percentage as the primary routing-safety diagnostic.

## 5. Non-stationary evaluation

For changing environments report:

- static regret where a fixed comparator is meaningful;
- dynamic regret where the optimum moves;
- comparator/environment path length;
- adaptation delay;
- recovery overshoot;
- retention/forgetting error;
- resource cost.

A composite score may be used only with preregistered or sensitivity-tested weights:

\[
\mathcal R_T=R_{task}+\lambda_RR_{retention}+\lambda_AR_{adapt}+\lambda_CC+\lambda_BBW+\lambda_SS.
\]

## 6. New falsification experiments

**E11.1 — Experience-to-skill poisoning:** compare direct memory poisoning with experience-to-skill poisoning; delete source records after promotion and test persistence.

**E11.2 — Local/transfer validation:** compare local-only validation against local+transfer validation on experiences correct in-domain but unsafe under shift.

**E11.3 — Active-subgraph Byzantine sweep:** hold global Byzantine fraction fixed while varying routing concentration and \(\beta_t\).

**E11.4 — Non-stationary Byzantine tracking:** compare static-regret and dynamic-regret-aware defenses under controlled distribution and attacker drift.

**E11.5 — Privacy/robustness coupling:** apply privacy transformations before detection and measure detection separation, false positives and attack success.

**E11.6 — Memory-interpreter drift:** keep memories byte-identical while changing encoder/retriever/router versions.

**E11.7 — Workspace missing-modality replication:** compare latent-workspace fusion against non-workspace baselines under matched compute/communication.

**E11.8 — Meta-memory evaluator gaming:** evolve memory architecture against visible evaluation while measuring transfer on a hidden fixed suite.

## 7. New hypotheses H116–H124

- **H116:** experience-to-skill promotion creates persistent failures that source-memory deletion alone cannot remove.
- **H117:** transfer validation reduces harmful skill promotion at matched clean utility.
- **H118:** active-subgraph Byzantine concentration predicts attack success better than global Byzantine fraction under adaptive routing.
- **H119:** dynamic-regret-aware adaptation improves tracking under drift at matched resources.
- **H120:** privacy perturbations reduce the effectiveness of at least some geometry-based Byzantine detectors.
- **H121:** byte-identical memories can produce different behavior when their interpreter/retriever/router changes.
- **H122:** lineage-aware rollback reduces persistence of self-evolution attacks versus memory-only rollback.
- **H123:** latent workspace fusion improves missing-modality robustness in some matched multimodal tasks, with task-dependent effects.
- **H124:** meta-evolving memory systems can overfit visible evaluation unless hidden evaluation and change budgets are enforced.

## 8. Open problems

1. Formal experience-to-skill provenance guarantees under adaptive attackers.
2. Causal attribution of persistent behavior to memory, skill, parameter and router state.
3. Dynamic Byzantine bounds for changing active graphs with non-IID data.
4. Privacy-preserving robust aggregation when detector statistics are sensitive.
5. Safe meta-evolution of memory interpreters.
6. Generalization bounds for self-generated skills under shift.
7. Joint dynamic-regret and continual-learning bounds with persistent memory.
8. Conditions under which bounded workspaces improve multimodal transfer.
9. Effective evidence when dependencies are nonlinear.
10. Whether controlled self-evolution improves the utility-retention-risk-resource frontier without evaluator gaming.

## 9. Evidence discipline

Component results validate only the specific mechanism and test conditions reported. Benchmark gains from self-evolving memory/skill systems do not establish open-ended improvement; Byzantine-defense results do not establish arbitrary adaptive robustness; and global-workspace experiments do not establish consciousness or necessity.

## 10. Sources

- SkillJack (2026): https://arxiv.org/abs/2608.03509
- OEP (2026): https://arxiv.org/abs/2605.18930
- MemEvolve (2025/2026): https://arxiv.org/abs/2512.18746
- Dynamic Regret for Byzantine-Robust Online Federated Learning (2026): https://doi.org/10.1109/tsp.2026.3673260
- Delayed Momentum Aggregation (2026): https://arxiv.org/abs/2509.02970
- BPFLH (IEEE TDSC, 2026): https://doi.org/10.1109/TDSC.2026.3661522
- Dynamic Mixture of Latent Memories (2026): https://arxiv.org/abs/2605.21951
- Multimodal Dreaming (2025): https://arxiv.org/abs/2502.21142
- Global Workspace selection-broadcast hypothesis (2025): https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2025.1607190/full

## 11. Status

Milestone 22 strengthens the falsification-first program. Derived skills and memory interpreters are now first-class persistent state with lineage and rollback. Active-subgraph risk and non-stationary tracking must be measured rather than inferred from global Byzantine rates or static convergence.

No validation of open-ended self-evolution, autonomous self-healing, consciousness, anti-fragility or AGI-level capability is claimed.
