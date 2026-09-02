# Milestone 01 — Literature + Mathematical Audit

## Status
Rigorous audit in progress. No implementation claim is treated as validated until supported by theory and experiment.

## Evidence taxonomy

- **Established:** directly supported by strong literature/theory.
- **Plausible synthesis:** engineering combination of established components, but not itself established.
- **Unsupported/speculative:** claim exceeds available evidence or depends on untested assumptions.
- **Mathematically incorrect/incomplete:** formulation requires correction before it can support a theorem or implementation.

## Architecture-level assessment

The Treatise is best understood as a proposed distributed modular intelligence architecture. Its strongest foundations are conditional expert computation, learned inter-module communication, generated weights, multimodal representation alignment, continual learning, uncertainty estimation, and fault-tolerant distributed systems. The distinctive combination is a research hypothesis rather than an established result.

## Major claims audit

| Treatise idea | Classification | Research basis / correction |
|---|---|---|
| Specialist organs / modular experts | Established foundation; proposed synthesis | MoE and modular/continual-learning literature supports specialization and conditional computation. The Holobiont-level cognitive claims remain empirical. |
| Learned latent bridges between organs | Plausible engineering synthesis | Learned communication and multimodal alignment support the mechanism. A bridge must be typed as `B_ij: H_i -> M_ij` and evaluated for task-relevant sufficiency; invertibility should not be assumed. |
| Reconstruction + utility bridge loss | Plausible, but equation incomplete | If `x_i` is reconstructed, `Dec_i(B_ij(Enc_i(x_i)))` is dimensionally valid. If the receiver needs task output, use `L_task,j` rather than assuming reconstruction guarantees utility. A general form is `L_ij = λ_rec L_rec + λ_task L_task + λ_reg R(B_ij)`. |
| Residual bloodstream / shared vitals | Plausible synthesis | Shared metadata can coordinate routing, confidence and resources. It introduces a common control plane and therefore a possible single point of failure. |
| Shared narrative thread | Plausible synthesis | GRU/state-space memory mechanisms are established, but whether one shared latent improves cognition is empirical. |
| Hypernetwork DNA generating organ weights | Established mechanism, speculative biological analogy | HyperNetworks explicitly generate weights of another network. They do not guarantee exact regeneration, specialization preservation, or coverage of arbitrary organ states. |
| LoRA-like shadow copies | Plausible engineering synthesis | Parameter-efficient adaptation is established. Recovery fidelity must be measured against full checkpoints and task-specific behavior. |
| Latent telepathy | Plausible terminology/mechanism | Learned hidden-state communication is established in multi-agent/multimodal settings. Semantic sufficiency and robustness of arbitrary latent protocols are not established. |
| Dynamic routing matrix A | Established concept, formulation incomplete | Define `A_t` explicitly. For convex routing, require `A_t[i,j] >= 0` and `Σ_j A_t[i,j]=1`. If graph dynamics are used, distinguish `A_t` from Laplacian `L_t = D_t-A_t`. |
| Consensus KL over hidden states | **Mathematically incorrect unless distributions are defined** | KL divergence requires probability measures. For embeddings use squared Euclidean distance, cosine distance, Bregman divergence in a valid coordinate space, or map embeddings to calibrated distributions first. |
| Consensus forces all organs toward common state | Risky / potentially self-defeating | Consensus can reduce diversity and specialization. The objective should preserve task-relevant disagreement: `L = L_task + λ_cons L_cons + λ_div L_div`, with diversity/interference measured explicitly. |
| Spectral gap = speed of thought | **Unsupported/overstated** | Spectral gap controls convergence rates only for specified linear/contractive consensus dynamics under graph assumptions. It is not a general cognitive speed metric. |
| Lyapunov proof of self-healing | **Mathematically incomplete** | A valid proof needs a defined stochastic/time-varying dynamical system, invariant domain, regularity assumptions, bounded/adversarial perturbation model, and a Lyapunov or input-to-state stability argument. |
| Constitutional quorum | Plausible synthesis | Quorum/replication concepts are established in distributed systems. Correct safety/liveness thresholds depend on the fault model and topology; they cannot be asserted universally. |
| Entropy surveillance detects corruption | Unsupported as a standalone detector | Confidence/entropy can be useful but confident incorrect predictions exist. Combine calibration, OOD scores, disagreement, provenance and behavioral tests. |
| Cross-modal veto | Plausible safety mechanism | Can reject inconsistent evidence, but modality correlation and shared failure modes can make a veto unreliable. Must measure false veto and missed corruption rates. |
| Distributed attack surface improves immunity | Speculative | Replication can improve fault tolerance, but additional communication and interfaces also increase attack surface and propagation paths. |
| Regeneration from distributed witnesses | Plausible research direction | Recovery requires enough independent information to reconstruct required behavior. This is a coding/information-recovery problem, not implied by redundancy alone. |
| 85% regeneration threshold | Unsupported hypothesis | Must be replaced by a measured recovery curve `Q(r)` versus surviving independent witnesses, attack type, and task distribution. |
| 30% organ-loss survival | Unsupported hypothesis | Treat as an experimental target, not a guarantee. Specify simultaneous vs sequential loss and correlated failures. |
| Immortality / zero downtime | Unsupported | Reliability must be stated probabilistically with explicit fault model, recovery time, residual risk, and information-loss bounds. |
| Universal hypernetwork regeneration | Speculative | Requires coverage/identifiability assumptions and behavioral verification after regeneration. |
| Shared 80% weights | Empirical heuristic | Shared parameters may improve transfer but can cause interference/homogenization. Sweep sharing ratios and measure specialization retention. |
| Multiplicative cross-modal binding | Plausible mechanism, not solved binding | Requires aligned dimensions, normalization and stability analysis. Compare additive, gated, bilinear and attention fusion. |
| Nash equilibrium from consensus gradient flow | **Not generally valid** | A stationary point of a centralized objective is not automatically a Nash equilibrium of a multi-player game. Define individual utilities and game dynamics if Nash language is retained. |
| Gradient sharing is safe | **False** | Gradient inversion/leakage literature shows gradients can reveal training information. Privacy requires explicit threat model and mechanisms such as secure aggregation and/or differential privacy. |

## Corrected mathematical core

Let each organ have a typed latent space `H_i = R^{d_i}` and task output space `Y_i`. A bridge is

`B_ij: H_i -> H_j`.

For a source latent `h_i` and receiver context `c_j`, the receiver produces

`y_j = f_j(B_ij(h_i), c_j)`.

A task-aware bridge objective can be written

`L_bridge = λ_rec E[d_i(x_i, Dec_i(B_ij(Enc_i(x_i))))] + λ_task E[ell_j(f_j(B_ij(h_i),c_j), y_j)] + λ_reg R(B_ij)`.

Reconstruction is optional: it should be included only when source-information preservation is actually desired. Utility can be more important than invertibility.

For routing, define a row-stochastic matrix

`A_t ∈ R^{N×N}`, `A_t[i,j] >= 0`, `Σ_j A_t[i,j] = 1`.

If consensus operates on states `h_i`, one simple linear reference model is

`h(t+1) = A_t h(t)`.

For a fixed doubly stochastic `A` with appropriate connectivity/aperiodicity, convergence to the average is controlled by the magnitude of the second-largest eigenvalue (or singular value in non-symmetric cases). This mathematical result is far narrower than the Treatise's claim that a spectral gap measures cognition speed.

A task-preserving consensus objective is better represented as

`L_total = L_task + λ_c L_cons + λ_div L_div + λ_safe L_safe + λ_reg L_reg`.

For Euclidean embeddings,

`L_cons = Σ_(i,j) w_ij ||P_i h_i - P_j h_j||²`,

where `P_i` maps each organ to a shared comparison space. This avoids applying KL divergence directly to arbitrary hidden vectors.

## Byzantine robustness implications

Yin et al. provide statistical guarantees for coordinate-wise median and trimmed-mean distributed optimization under explicit assumptions. Their analysis also shows the two methods have different assumptions and rates. Later work shows robust aggregation can still fail under time-coupled attacks, and Byzantine attackers can create fake minima near saddle points in non-convex optimization. Therefore a Holobiont cannot claim immunity merely because it uses consensus or robust aggregation.

For an organ-level fault model, distinguish:

1. crash/omission fault;
2. random noisy fault;
3. stale/replayed message;
4. Byzantine arbitrary message;
5. targeted semantic corruption;
6. colluding Byzantine organs;
7. correlated/common-mode failure.

Each requires separate detection and recovery experiments.

## Uncertainty and OOD

Entropy should be treated as one signal, not a proof of integrity. A stronger organ trust score should combine calibrated confidence, predictive disagreement, OOD score, provenance/health metadata, and temporal consistency. Ensemble literature and calibrated OOD work motivate this multi-signal approach, but no universal detector is guaranteed under arbitrary distribution shift.

## Hypernetwork regeneration

A regeneration theorem candidate should not claim exact weight recovery. Define a behavior metric `D_task(f_W, f_W')` over a specified task distribution `P_T`. A regeneration procedure is successful when

`P_{x~P_T}[f_W(x) != f_W'(x)] <= ε`

or an appropriate task loss gap is below `ε`, with high confidence over the evaluation sample. This is a behavioral recovery criterion, not parameter equality.

## Research hypotheses for next milestone

### H1 — Bridge sufficiency
Task-aware latent bridges can recover most of the benefit of direct cross-organ access while transmitting substantially fewer dimensions than raw hidden states.

### H2 — Selective consensus
Consensus applied only to shared/task-relevant subspaces improves coordination while preserving specialization better than full-state consensus.

### H3 — Trust-weighted routing
Routing that combines calibrated uncertainty, OOD score, disagreement and health state is more robust to organ corruption than entropy-only routing.

### H4 — Regeneration threshold is task-dependent
There is no universal percentage-of-organs threshold. Recovery quality depends on redundancy structure, witness independence and the task distribution.

### H5 — Temporal attacks defeat naive robustness
A routing/aggregation mechanism robust to independent one-step Byzantine messages can still fail under coordinated time-dependent attacks.

### H6 — Communication is a primary systems bottleneck
The performance of the architecture will depend on communication bandwidth, bridge computation, synchronization and routing overhead—not only model FLOPs.

## Required evaluation before implementation claims

- Strong monolithic baseline.
- Independent-specialists baseline with no communication.
- Full-state communication baseline.
- Learned bridge baseline.
- Shared-state consensus baseline.
- Selective consensus baseline.
- Entropy-only trust vs calibrated/OOD/disagreement trust.
- No-fault, crash, random corruption, Byzantine, collusion and time-coupled attacks.
- Organ-loss sweeps and recovery-time measurements.
- Sharing-ratio and communication-bandwidth sweeps.
- Multiple random seeds and confidence intervals.

## Key sources

- Shazeer et al., *Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer*.
- Fedus, Zoph & Shazeer, *Switch Transformers*.
- Ha, Dai & Le, *HyperNetworks* — https://arxiv.org/abs/1609.09106
- Yin et al., *Byzantine-Robust Distributed Learning: Towards Optimal Statistical Rates* — https://proceedings.mlr.press/v80/yin18a.html
- Yin et al., *Defending Against Saddle Point Attack in Byzantine-Robust Distributed Learning* — https://proceedings.mlr.press/v97/yin19a.html
- Karimireddy, He & Jaggi, *Learning from History for Byzantine Robust Optimization* — https://proceedings.mlr.press/v139/karimireddy21a.html
- Chen et al., *Byzantine-Robust Online and Offline Distributed Reinforcement Learning* — https://proceedings.mlr.press/v206/chen23b.html
- Wu et al., *Continual Learning for Large Language Models: A Survey* — https://arxiv.org/abs/2402.01364
- Shi et al., *Continual Learning of Large Language Models: A Comprehensive Survey* — https://arxiv.org/abs/2404.16789
- Kumar et al., *Calibrated ensembles can mitigate accuracy tradeoffs under distribution shift* — https://proceedings.mlr.press/v180/kumar22a.html
- Dinari & Freifeld, *Variational- and metric-based deep latent space for out-of-distribution detection* — https://proceedings.mlr.press/v180/dinari22a.html

## Next milestone

Construct the complete claim-by-claim matrix for the Treatise's formal chapters, with exact source passages/results, assumptions, corrected equations, competing approaches, and falsification experiments. Then formalize the minimum viable benchmark and statistical analysis plan before any production implementation.
