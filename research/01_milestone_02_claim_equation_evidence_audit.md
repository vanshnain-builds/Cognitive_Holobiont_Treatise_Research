# Milestone 02 — Claim, Equation, Evidence Audit

Date: 2026-09-03

## Executive finding

The Cognitive Holobiont is best treated as a research architecture assembled from established components, not as an established new intelligence paradigm. The strongest parts have prior art in sparse Mixture-of-Experts, learned communication, hypernetworks, continual learning, uncertainty estimation, federated learning, Byzantine robustness, and fault-tolerant distributed systems. The strongest Holobiont claims—universal regeneration, immortality, zero downtime, anti-fragility, consciousness/identity, and guaranteed aligned consensus—remain hypotheses.

## Claim ledger

| Treatise claim | Classification | Evidence / correction |
|---|---|---|
| Specialist organs + selective activation | 1 established component; 2 system synthesis | Sparse MoE provides conditional computation, but MoE experts are not autonomous brains. See Shazeer 2017 and Switch 2021. |
| Learned bridges / shared latent language | 1 established that learned communication/alignment is possible; 2 Holobiont synthesis | Multi-agent learned communication and multimodal cross-attention support the mechanism, not automatic semantic interoperability among heterogeneous models. |
| Narrative Thread as shared temporal state | 2 plausible synthesis | A recurrent state is mathematically straightforward; calling it autobiographical identity or consciousness is not established. |
| Hypernetwork DNA generates any organ | 1 hypernetworks established; 3 universal regeneration unsupported | Hypernetworks generate target weights, but compactness does not imply arbitrary reconstruction. |
| LoRA shadow recovers 80% | 2 plausible mechanism; 3 numerical claim unsupported | Recovery must be measured against a held-out task distribution with an explicit metric. |
| Consensus loss over hidden states using KL | 4 mathematically incomplete/incorrect as written | KL requires probability distributions. Hidden vectors need a common embedding space or a separate probability head. |
| Spectral gap = speed of thought | 4 unsupported mathematical interpretation | Algebraic connectivity controls decay for particular consensus dynamics; it is not a cognition-speed law. |
| V=L_consensus+L_immunity proves convergence/Nash equilibrium | 4 incomplete | The stated derivative assumes gradient-flow dynamics and regularity/invariance conditions. LaSalle does not imply a Nash equilibrium without an explicit game. |
| Entropy detects corrupt organs | 2 useful signal; 3 insufficient alone | Calibration/OOD/ensemble disagreement/provenance/behavioral tests should be combined. |
| Gradients preserve privacy | 4 incorrect implication | Gradient inversion demonstrates leakage; secure aggregation and privacy accounting are separate requirements. |
| 85% regeneration threshold | 3 unsupported | Replace with a predefined statistical equivalence/non-inferiority criterion. |
| Failures improve the system (anti-fragility) | 3 hypothesis | Robustness is not anti-fragility. Require positive post-stress improvement on held-out future tasks. |
| Cognitive mitosis/symbiosis | 3 speculative | Graph clustering is established; viable cognitive descendants require measurable preservation of capability, memory, safety and interfaces. |

## Mathematical corrections

### 1. Typed latent communication

For organ i let h_i in H_i and choose a shared interface Z=R^d:

z_i = E_i(h_i,x_i),  z_i in Z.

A bridge D_{j->i}: Z -> H_i can then translate another organ's message. This removes the dimensional/type ambiguity in direct comparisons of arbitrary h_i.

### 2. Consensus

If z_i are in the same vector space, use:

L_disc = sum_(i,j) A_ij ||z_i-z_j||^2.

If p_i are probability vectors, p_i in Delta^(K-1), then KL is valid:

KL(p_i||p_j) = sum_k p_ik log(p_ik/p_jk).

For symmetric disagreement, Jensen-Shannon divergence is bounded and symmetric.

### 3. Routing

For directed routing define scores s_ij and:

A_ij = exp(s_ij/T) / sum_(k != i) exp(s_ik/T), i != j.

Then each row sums to one. A directed stochastic matrix must not be silently treated as a symmetric adjacency matrix. Its Laplacian and convergence properties depend on the chosen convention.

### 4. Consensus dynamics

A clean continuous-time abstraction is:

dot z_i = sum_j A_ij(z_j-z_i) + u_i,

where u_i contains task/local-learning forces. The pure-consensus case is u_i=0. With u_i nonzero, ordinary consensus theorems no longer directly apply.

For a connected undirected graph with Laplacian L and dot x=-Lx:

||x(t)-x_bar 1||_2 <= exp(-lambda_2(L)t)||x(0)-x_bar 1||_2.

This establishes a convergence-rate bound for that system only. It does not establish “spectral gap proportional to speed of thought.”

### 5. Lyapunov argument

The Treatise uses V=L_consensus+L_immunity and then writes a gradient-flow derivative. That step is valid only if the actual dynamics satisfy dot h_i=-nabla_(h_i)V and the necessary differentiability/boundedness/invariance assumptions hold. The real architecture has routing changes, delays, stochastic learning and discrete quarantine events, so a hybrid/stochastic stability analysis is needed.

Also, non-increasing V does not imply convergence to a unique point. LaSalle-type reasoning identifies an invariant set under its assumptions. “Nash equilibrium” requires a specified game and unilateral-deviation condition; it does not follow from Lyapunov descent.

### 6. Regeneration

Let H_eta generate an initial failed-organ parameterization:

theta_hat_k^0 = H_eta(t_k,c_t,z_k).

Then adapt on witness data D_w:

theta_hat_k = argmin_theta [ L_w(theta) + lambda D_KL(p_old || p_theta) + gamma R(theta) ].

The KL term is between output distributions, not arbitrary parameter vectors. Functional distortion is preferable to raw-weight distance because neural parameterizations have symmetries.

A recovery ratio should be explicitly defined, e.g.

R_rec = M(f_hat)/M(f_original),

where M and its test distribution are fixed before the experiment.

## Reliability correction

The statement “many organs make death practically impossible” is not an availability theorem. Under an intentionally simple independent parallel model:

A_sys = 1 - product_i(1-a_i).

But common-mode failure, correlated software bugs, shared infrastructure, network partitions, supply-chain compromise and correlated adversarial attacks can dominate. The Holobiont needs a reliability model that includes correlation and dependency structure.

## Privacy correction

Local training does not imply private training. Deep Leakage from Gradients shows that shared gradients can reveal training examples. Secure aggregation protects individual updates under its stated threat model, but multi-round participation can introduce additional leakage. The regeneration system therefore needs explicit assumptions and separate tests for secure aggregation, differential privacy, gradient inversion, membership inference and collusion.

## Literature-supported components

- Shazeer et al., Sparsely-Gated MoE: https://arxiv.org/abs/1701.06538
- Fedus et al., Switch Transformers: https://arxiv.org/abs/2101.03961
- Zhou et al., Expert Choice Routing: https://arxiv.org/abs/2202.09368
- Ha et al., HyperNetworks: https://arxiv.org/abs/1609.09106
- von Oswald et al., Continual Learning with HyperNetworks: https://arxiv.org/abs/1906.00695
- Foerster et al., Learned multi-agent communication: https://arxiv.org/abs/1605.06676
- Tsai et al., Multimodal Transformer: https://aclanthology.org/P19-1656/
- Rusu et al., Progressive Neural Networks: https://arxiv.org/abs/1606.04671
- Kirkpatrick et al., EWC: https://www.pnas.org/doi/10.1073/pnas.1611835114
- Li & Hoiem, Learning without Forgetting: https://arxiv.org/abs/1606.09282
- Lakshminarayanan et al., Deep Ensembles: https://arxiv.org/abs/1612.01474
- Guo et al., Calibration: https://arxiv.org/abs/1706.04599
- Lee et al., OOD/adversarial detection: https://arxiv.org/abs/1807.03888
- Yin et al., Byzantine-robust distributed learning: https://arxiv.org/abs/1803.01498
- Castro & Liskov, PBFT/proactive recovery: https://www.microsoft.com/en-us/research/publication/practical-byzantine-fault-tolerance-proactive-recovery/
- Ongaro & Ousterhout, Raft: https://www.usenix.org/conference/atc14/technical-sessions/presentation/ongaro
- McMahan et al., FedAvg: https://arxiv.org/abs/1602.05629
- Zhu et al., Deep Leakage from Gradients: https://arxiv.org/abs/1906.08935
- Bonawitz et al., Secure Aggregation: https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/

## Minimum experiment matrix

E1 Coordination: dense generalist vs standard MoE vs independent specialists+router vs specialists+bridges vs Holobiont.

E2 Specialization: sweep bridge strength and measure native-task performance plus representational similarity.

E3 Regeneration: compare checkpoint restore, retraining, LoRA shadow, hypernetwork, and hypernetwork+witness adaptation after controlled organ loss/corruption.

E4 Byzantine: random, sign-flip, targeted poisoning, inconsistent messages, collusion and adaptive multi-round attacks.

Primary metrics: task utility, native specialization retention, recovery fidelity, time-to-service, communication bytes, compute/FLOPs, latency, false-positive quarantine, attacker success rate, calibration, OOD detection and collateral degradation.

## Anti-fragility test

For stress event e define:

AF(e)=M_post(D_future)-M_pre(D_future).

A system is anti-fragile only if exposure to a predefined family of stresses produces statistically reliable positive AF on held-out future distributions. Recovery alone is robustness, not anti-fragility.

## Next milestone

Milestone 03 will perform a formal chapter-by-chapter audit of Chapters 8–10: directed consensus, bridge-temperature dynamics, Lyapunov assumptions, game-theoretic language, regeneration objectives, shadow-copy equations, cognitive mitosis, and evolutionary/self-modification. Every equation will receive domain/codomain definitions and every guarantee will receive explicit assumptions and a falsification test.
