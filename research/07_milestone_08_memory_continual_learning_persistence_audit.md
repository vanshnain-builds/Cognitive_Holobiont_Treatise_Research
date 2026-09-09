# Milestone 08 — Memory, Continual Learning, and Long-Term Knowledge Persistence Audit

**Date:** 2026-09-09  
**Status:** Research / pre-implementation

## Executive conclusion

The Treatise's memory layer is best decomposed into episodic memory, semantic memory, retrieval/indexing, bounded working memory, consolidation/update policy, provenance/versioning, and rollback/forgetting. Prior work strongly supports individual mechanisms, but does not establish indefinite exact recall, universally correct memory, or immunity to catastrophic forgetting.

## Claim classification

| Treatise claim | Classification | Evidence judgment |
|---|---|---|
| Multiple memory stores can reduce interference | (1)/(2) | Replay, consolidation, gating and complementary-memory mechanisms have substantial support; the exact Holobiont composition remains a synthesis. |
| External memory can preserve information outside parameters | (1) | Retrieval-augmented and external-memory systems establish the mechanism. |
| Retrieval makes old information available to inference | (1) | Established, but retrieval quality and downstream use remain imperfect. |
| Retrieved memory is equivalent to learned knowledge | (3) | Retrieval is access, not proof of correctness or internalization. |
| Continual learning can proceed indefinitely without forgetting | (3) | No general result supports this claim. |
| Consolidation guarantees preservation | (4) | Consolidation penalties require explicit smoothness/locality assumptions and do not by themselves prove functional invariance. |
| Episodic + semantic memory guarantees human-like learning | (3) | Biological analogy does not establish machine behavior. |
| Vector similarity implies memory consistency | (4) | Similarity is not truth, freshness, authority, causality, or version correctness. |
| High-confidence memories should persist indefinitely | (3) | Confidence can be miscalibrated and facts can become stale. |
| Replicated memory is self-healing by replication alone | (2) | Replication can improve availability but cannot guarantee correctness under correlated corruption or stale replicas. |
| Provenance/versioning enables safer rollback | (1)/(2) | Provenance and version control are established engineering mechanisms; Holobiont-level safety benefits require experiments. |
| Small compressed memory can preserve arbitrary history | (3) | Possible only for restricted task/data families with exploitable structure. |

## Continual-learning foundation

Catastrophic forgetting is well established. Kirkpatrick et al. introduced Elastic Weight Consolidation (EWC), which penalizes changes to parameters important to previous tasks. The result is useful evidence for stability mechanisms, not a universal no-forgetting theorem. https://pmc.ncbi.nlm.nih.gov/articles/PMC5380101/

Orthogonal Gradient Descent, context-dependent gating, and replay/consolidation combinations provide complementary approaches with different assumptions and costs. https://proceedings.mlr.press/v108/farajtabar20a.html ; https://pmc.ncbi.nlm.nih.gov/articles/PMC6217392/ ; https://proceedings.mlr.press/v199/sarfraz22a.html

Knoblauch et al. show that optimal continual learning can require perfect memory and is generally NP-hard. This is a theoretical limitation on unrestricted claims, not a statement that practical continual learning is impossible. https://proceedings.mlr.press/v119/knoblauch20a

A 2026 analytical study reports phase-transition-like continual-learning behavior associated with task similarity and architecture, strengthening the case for evaluating task relatedness explicitly. https://pmc.ncbi.nlm.nih.gov/articles/PMC12890896/

## Rigorous forgetting metric

Let tasks arrive as distributions $D_1,\ldots,D_T$, and let $R_t^{(k)}$ be risk on task $k$ immediately after learning task $t$. For $k<t$ define

$$F_{k,t}=R_t^{(k)}-R_k^{(k)}.$$

For larger-is-better utility $U$ use $F_{k,t}=U_k^{(k)}-U_t^{(k)}$. An aggregate final forgetting score is

$$F_T=\frac{1}{T-1}\sum_{k=1}^{T-1}F_{k,T}.$$

A claim of no catastrophic forgetting should be evaluated against a pre-registered tolerance and confidence interval, e.g. $|F_T|\leq\epsilon_F$, over a declared family of task sequences and memory budgets. One benchmark run is not evidence of a universal guarantee.

## What EWC actually establishes

A standard objective is

$$\mathcal L(\theta)=\mathcal L_{new}(\theta)+\frac{\lambda}{2}\sum_i F_i(\theta_i-\theta_i^*)^2.$$

This penalizes movement of parameters weighted by estimated importance. To connect parameter displacement to old-task loss, use a local Taylor expansion:

$$\mathcal L_{old}(\theta^*+\Delta\theta)\approx\mathcal L_{old}(\theta^*)+\nabla\mathcal L_{old}(\theta^*)^T\Delta\theta+\frac12\Delta\theta^TH\Delta\theta.$$

If $\theta^*$ is stationary and $H\preceq MI$ locally, then

$$\mathcal L_{old}(\theta^*+\Delta\theta)-\mathcal L_{old}(\theta^*)\leq\frac{M}{2}\|\Delta\theta\|_2^2.$$

This is a local bound. It is not a global or indefinite preservation theorem. Any stronger Treatise claim is (4) unless the missing assumptions are supplied.

## Replay and memory sufficiency

Experience replay is a strong continual-learning family, but its benefit depends on buffer size, task sequence, sampling policy and distribution shift. Similarity-weighted replay and replay-plus-consolidation work suggest that old examples need not be equally valuable. https://pmc.ncbi.nlm.nih.gov/articles/PMC9271163/ ; https://proceedings.mlr.press/v199/sarfraz22a.html

A simple replay objective is

$$\mathcal L_t=\mathcal L(D_t;\theta)+\beta\mathcal L(M_t;\theta),$$

where $M_t$ is a memory sample. $M_t$ is not equivalent to the historical distribution unless it is sufficient for the future task family.

A useful empirical memory-sufficiency diagnostic is

$$G_t=E_{D_{past}}[\ell(f_{\theta_t}(x),y)]-E_{M_t}[\ell(f_{\theta_t}(x),y)].$$

This is a diagnostic, not a theorem.

## Memory representation and provenance

A persistent memory record should conceptually include more than a vector:

$$m=(k,v,t_{write},s,p,c,\tau),$$

where $k$ is a retrieval key, $v$ content, $t_{write}$ write time, $s$ source identifier, $p$ provenance, $c$ calibrated confidence metadata, and $\tau$ validity/supersession state.

This is an engineering specification, not a claim that these fields solve memory safety. Recent surveys of autonomous-agent memory emphasize write/manage/read loops, contradiction handling, privacy, latency and learned forgetting. A 2026 memory-security survey frames persistent memory as a lifecycle security problem spanning write, store, retrieve, execute, propagation, and forget/rollback, with provenance and versioning as important controls. https://arxiv.org/abs/2603.07670 ; https://arxiv.org/abs/2604.16548

## Retrieval is not truth

RAG establishes that parametric and non-parametric memory can be combined effectively for knowledge-intensive tasks. https://arxiv.org/abs/2005.11401

The implication

$$\operatorname{retrieved}(m)\Rightarrow\operatorname{true}(m)$$

is invalid. Retrieval likelihood is not truth probability. A safer conceptual model is

$$P(y\mid x,M)\propto\sum_{m\in TopK(x,M)}P(y\mid x,m)P(m\mid x,M).$$

For safety-critical memory, retrieval should be followed where possible by provenance checks, contradiction detection, temporal-validity checks and task-level verification.

## Long-term memory precedents

MemGPT uses hierarchical virtual-context management to move information between fast context and slower external memory. https://arxiv.org/abs/2310.08560

Generative Agents combine stored experiences, reflection and retrieval for planning. https://arxiv.org/abs/2304.03442

LongMem separates a frozen backbone from a memory retriever/reader and demonstrates long-form external memory. https://arxiv.org/abs/2306.07174

These are useful precedents. None establishes unlimited, immutable, universally correct memory.

## Distributed memory consistency

If replicas store $M_1,\ldots,M_r$, replication alone gives no universal guarantee of consistency or correctness. A conceptual memory version can be represented as

$$M_i(k)=(v_i,ver_i,t_i,s_i).$$

The Holobiont therefore needs an explicit conflict policy based on source authority, version precedence, temporal validity, confidence, corroboration, and rollback authority. This is a plausible systems synthesis, not a consequence of vector similarity or consensus alone.

## Stale memory

A memory can be correct when written and false later. Define age

$$A(m,t)=t-t_{write}.$$

A time-dependent retention score may be written as

$$q(m,t)=q_0(m)g(A(m,t)),$$

but the function $g$ must be learned/validated for the domain rather than assumed to be exponential. For rapidly changing facts, revalidation is preferable to merely lowering retrieval score.

## Memory poisoning

Persistent memory creates a durable attack surface because a bad write can influence later retrieval and decisions. Model the provenance chain as

$$source\rightarrow observation\rightarrow transformation\rightarrow memory\ entry\rightarrow retrieval\rightarrow decision.$$

This makes audit and rollback testable. Internal origin alone should not imply trust.

## Safe incremental write rule

A conservative update can be specified as

$$M_{t+1}=\operatorname{Validate}(\operatorname{Merge}(M_t,W_t)).$$

Validation should be an explicit predicate:

$$V(W_t,M_t)=V_{schema}\land V_{source}\land V_{consistency}\land V_{policy}\land V_{task}.$$

Only validated writes become authoritative. This is a plausible engineering synthesis and must be compared with an unsafe append-only baseline.

## Modularity and interference

If parameters are partitioned as

$$\theta=(\theta_1,\ldots,\theta_N)$$

and task $t$ activates subset $S_t$, specialist-local updates can enforce

$$\Delta\theta_i=0\quad\text{for }i\notin S_t.$$

This removes direct parameter updates outside the active set, but not indirect interference through bridges, routers, shared normalization, memory, or decision layers. A useful decomposition is

$$I_{old,new}=I_{params}+I_{bridge}+I_{router}+I_{memory}+I_{decision}.$$

This should become a core measurement of the Holobiont rather than assuming modularity solves all interference.

## Information-budget constraint

If memory stores only $B$ bits, it cannot preserve arbitrary information about an unbounded history. A finite-memory system must exploit structure in the task family. An indefinite-exact-recall claim therefore needs an assumption such as

$$H(Z_{past}\mid M_t,\mathcal T)\leq\epsilon,$$

where $Z_{past}$ is the task-relevant historical information and $\mathcal T$ describes the future task family. Without such an assumption, perfect persistent memory with finite storage is not a valid general claim.

## Experiments

### H12 — Typed memory

Compare a single undifferentiated vector store against equal-budget typed episodic/semantic memory. Primary outcomes: final old-task risk, forgetting, retrieval utility and memory cost.

### H13 — Provenance against poisoning

Inject false, stale and contradictory writes. Compare no provenance, source/version metadata, and full validation/rollback. Primary outcomes: poisoning success, clean utility, write latency and recovery time.

### H14 — Modular interference

Compare monolithic and specialist-local updates. Measure parameter, bridge, router, memory and decision interference separately.

### H15 — Task similarity

Sweep controlled task similarity and test whether forgetting/transfer changes sharply with task relatedness, rather than treating all task sequences as equivalent.

### H16 — Retrieval recall vs downstream utility

Test whether increasing $Rel@K$ monotonically improves downstream utility under clean, stale, contradictory and poisoned memories. The hypothesis is that it will not.

## Baselines

1. Stateless specialist.
2. Raw replay buffer.
3. EWC.
4. Replay + EWC.
5. External vector retrieval.
6. Retrieval + reranking/provenance.
7. Typed episodic/semantic memory.
8. Typed memory + validation + rollback.

Control model capacity, storage budget and compute where possible.

## Statistical protocol

Use multiple seeds, confidence intervals, fixed task sequences, multiple task permutations where order matters, and matched compute/memory accounting. Evaluate clean and corrupted-memory conditions separately. Pre-register primary metrics and acceptance thresholds before interpreting results.

## Open problems

1. What representation is sufficient for long-term memory across heterogeneous organs?
2. How should episodic memories be promoted to semantic memory?
3. How can consolidation be task-conditional without hidden interference?
4. How should contradictions be resolved across sources?
5. How should temporal validity be learned without over-forgetting stable facts?
6. What information budget is sufficient to recover a specialist?
7. Can provenance survive latent compression?
8. Can a retrieval index detect its own corruption?
9. How should deletion/rollback propagate across replicated memory?
10. Can continual-learning guarantees extend to coupled specialist-router-memory dynamics?

## Decision gate

Do not claim that the Holobiont has solved continual learning or persistent memory until experiments establish statistically reliable retention, controlled memory/compute cost, stale/contradictory-memory robustness, poisoning resistance, rollback, task-sequence generalization, and reproducibility across seeds and task orders.

## Primary sources

- Kirkpatrick et al. (2017), *Overcoming catastrophic forgetting in neural networks*. https://pmc.ncbi.nlm.nih.gov/articles/PMC5380101/
- Farajtabar et al. (2020), *Orthogonal Gradient Descent for Continual Learning*. https://proceedings.mlr.press/v108/farajtabar20a.html
- Masse et al. (2018), *Alleviating catastrophic forgetting using context-dependent gating and synaptic stabilization*. https://pmc.ncbi.nlm.nih.gov/articles/PMC6217392/
- Sarfraz et al. (2022), *SYNERgy between SYNaptic Consolidation and Experience Replay for General Continual Learning*. https://proceedings.mlr.press/v199/sarfraz22a.html
- Knoblauch et al. (2020), *Optimal Continual Learning has Perfect Memory and is NP-hard*. https://proceedings.mlr.press/v119/knoblauch20a
- Shan, Li & Sompolinsky (2026), *Order parameters and phase transitions of continual learning in deep neural networks*. https://pmc.ncbi.nlm.nih.gov/articles/PMC12890896/
- Lewis et al. (2020), *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*. https://arxiv.org/abs/2005.11401
- Packer et al. (2023), *MemGPT: Towards LLMs as Operating Systems*. https://arxiv.org/abs/2310.08560
- Park et al. (2023), *Generative Agents: Interactive Simulacra of Human Behavior*. https://arxiv.org/abs/2304.03442
- Wang et al. (2023), *Augmenting Language Models with Long-Term Memory*. https://arxiv.org/abs/2306.07174
- Du (2026), *Memory for Autonomous LLM Agents: Mechanisms, Evaluation, and Emerging Frontiers*. https://arxiv.org/abs/2603.07670
- Lin et al. (2026), *A Survey on Long-Term Memory Security in LLM Agents*. https://arxiv.org/abs/2604.16548
- Yoo et al. (2024), *Layerwise Proximal Replay*. https://proceedings.mlr.press/v235/yoo24a.html
- Bhat et al. (2022), *Consistency is the Key to Further Mitigating Catastrophic Forgetting*. https://proceedings.mlr.press/v199/bhat22b.html

## Status

**Established:** catastrophic forgetting, replay-based retention, parameter consolidation, external retrieval memory, and multiple memory-management architectures.

**Plausible synthesis:** typed episodic/semantic memory with provenance, validation, rollback, and specialist-local updates.

**Unsupported:** indefinite exact memory, universal conflict-free memory, automatic correctness of retrieved memories, or memory alone producing human-like continual learning.

**Mathematically incomplete:** any finite-memory or consolidation equation presented as a universal guarantee without explicit information, smoothness, distributional, or stability assumptions.

No validation of the complete Cognitive Holobiont architecture is claimed by this milestone.
