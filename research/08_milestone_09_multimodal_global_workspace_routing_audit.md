# Milestone 09 — Multimodal Fusion, Global Workspace, Working Memory, and Cross-Organ Routing Audit

Date: 2026-09-10
Status: research / pre-implementation

## Scope

This milestone audits claims about a shared workspace, multimodal integration, cross-organ communication, attention/broadcast dynamics, adaptive routing, and missing or contradictory modalities. Cognitive-science theories are treated as hypotheses and computational inspiration, not as proof of machine consciousness or general intelligence.

## Claim classification

| Claim | Status | Assessment |
|---|---|---|
| Independent specialists can process different modalities/tasks | 1 Established | Standard modular and multimodal design |
| Learned shared latents can support cross-modal communication | 1 Established | Demonstrated in specific alignment/translation tasks |
| Workspace + cycle consistency can reduce matched-data requirements | 1 Established for studied settings | Devillers et al. report this in their experiments |
| Workspace is a useful cognitive-inspired broadcast mechanism | 2 Plausible engineering synthesis | Computational GWT-inspired architectures exist |
| A workspace is necessary for multimodal intelligence | 3 Unsupported/speculative | Alternative fusion architectures work |
| Attention weights are causal importance | 4 Mathematically/interpretively incomplete | Requires intervention tests |
| Missing modalities can always be reconstructed without information loss | 3 Unsupported/speculative | Recovery is conditional and can add error |
| Spectral gap directly determines cognitive speed | 4 Incorrect as a general claim | It bounds convergence for specified consensus dynamics |
| More communication necessarily creates more intelligence | 3 Unsupported/speculative | Bandwidth, latency and interference can offset gains |

## Formal architecture

For organ i:

h_i = f_i(x_i; theta_i),   h_i in R^(d_i)

A typed encoder produces a common workspace representation:

z_i = E_i h_i + e_i,   z_i in R^d.

The workspace is a state-transition mechanism:

w_t = F(w_(t-1), {z_(i,t)} for i in A_t, u_t),

where A_t is the active set. Broadcast to organ j is

b_(j,t) = D_j(w_t),

followed by

h'_(j,t) = G_j(h_(j,t), b_(j,t); theta_j).

This prevents the Treatise from treating “workspace” as merely concatenation.

## Cross-attention

A standard exchange mechanism is

Attn(Q,K,V) = softmax(QK^T / sqrt(d_k)) V.

Cross-attention already supplies a strong baseline for inter-organ communication. A separate workspace must therefore beat cross-attention under matched parameter, compute and communication budgets.

## Evidence for a workspace

Devillers, Maytie and VanRullen implement a Global Workspace-inspired model with frozen modality-specific systems, encoders/decoders around a shared workspace, and cycle-consistency training. They report cross-modal alignment/translation with less matched multimodal data in the studied tasks and show downstream benefits; ablations identify the workspace and cycle-consistency objective as important.

Bao et al. report a Global Workspace Network for multimodal pain recognition that outperformed simple concatenation on their benchmark and modeled attention across modalities over time.

These results support a useful computational pattern. They do not establish necessity for general intelligence or consciousness.

## Shared/private representations

A fully shared representation can discard modality-specific information. A safer formulation is

h_i -> (z_shared, z_private_i)

with the shared component optimized for cross-modal/task-relevant information and the private component retaining modality-specific information.

The benchmark should compare this against an equal-capacity fully shared representation. Success requires better robustness or transfer, not merely lower alignment loss.

## Cycle consistency is not semantic proof

For encoder E_i and decoder D_i:

L_cycle = E[ ell(D_i(E_i(x_i)), x_i) ].

Low cycle loss shows reconstruction, not unique semantic alignment. Multiple latent parameterizations can reconstruct the same observations. Cross-modal semantic alignment therefore needs paired-data tests, transfer tests, intervention tests, or other task-level evidence.

## Workspace capacity and information budget

If the workspace carries m tokens of width d, its raw state contains O(md) scalar coordinates before quantization/redundancy. This is an engineering capacity statement, not a universal cognitive-capacity law.

Define communication efficiency as

eta = Delta U / B,

where Delta U is utility improvement over a no-communication baseline and B is transmitted bytes/tokens/bits.

Any claim of lossless universal communication must specify the source distribution, distortion metric and channel capacity.

## Missing-modality robustness

Let M be the observed modality subset. Define

R(M) = E[ell(y_hat_M, y)]

and degradation relative to all modalities S as

Delta_M = R(M) - R(S).

Test random dropout, block/temporal dropout, systematic absence, corrupted input, contradictory input and distribution-shifted input. Compare graceful degradation, imputation, and modality-invariant fallback. Missing-modality surveys show that recovery can help but is conditional and can introduce error.

## Reliability-aware routing

A candidate gate is

alpha_i = exp(s_i/tau) / sum_j exp(s_j/tau),

where s_i = g_i(z_i, c_i) and c_i can contain uncertainty, quality, freshness, missingness and provenance signals.

Confidence must not be treated as correctness. Evaluate calibration, OOD detection, corruption response and task loss independently.

## Causal attribution

Do not infer causal importance from attention weights. Use interventions such as

Y_do(r_i=0) - Y_do(r_i=1)

under randomized controls. Also remove one organ, remove one broadcast edge, replace a message with noise, replay a stale message, swap messages between organs, freeze routing, randomize routing, and equalize communication budgets.

## Routing objective

A candidate objective is

L = L_task + lambda_B L_bandwidth + lambda_I L_interference + lambda_U L_uncertainty + lambda_C L_consistency.

Every term must have an operational metric. Bandwidth should include payload and measured communication time; interference should measure performance changes in other tasks/organs; uncertainty should use a defined reliability metric; consistency must compare representations that are actually comparable.

## Latency and bandwidth

For p messages of dimension d and q bytes/scalar:

B_step = p d q.

This is payload only. End-to-end latency also includes serialization, scheduling, synchronization, transport and accelerator overhead.

Cross-attention score computation is O(n_q n_k d_k), so a bottleneck workspace may reduce interaction cost but its encoders, decoders and transport costs must be included.

Claims such as fixed 60–70% compute reduction or 2–3x latency require workload- and hardware-specific measurements.

## Controlled experiment matrix

| System | Specialists | Workspace | Cross-attention | Dynamic gating | Missing-modality training |
|---|---|---|---|---|---|
| B0 | yes | no | no | no | no |
| B1 concat | yes | no | no | no | optional |
| B2 cross-attention | yes | no | yes | no | yes |
| B3 static workspace | yes | yes | optional | no | yes |
| B4 dynamic workspace | yes | yes | optional | yes | yes |
| B5 reliability-aware | yes | yes | optional | yes | yes |

The causal question is whether B3–B5 outperform B1–B2 after controlling parameters, training compute, communication budget and modality information.

## Hypotheses

H17: A shared workspace improves cross-modal transfer over matched-capacity fusion/cross-attention under limited paired data.

H18: Reliability-aware routing reduces corruption-induced degradation compared with fixed fusion.

H19: There exists a communication budget below which performance remains within a predeclared tolerance while payload is substantially reduced.

H20: Shared/private latent separation improves robustness to modality-specific nuisance variation at matched capacity.

H21: Removing selected broadcast channels causes reproducible task degradation beyond random message-removal controls.

Each hypothesis must be falsifiable and tested with multiple seeds, confidence intervals, and matched compute/parameter budgets.

## Current evidence position

Established: specialized encoders, cross-attention, shared latent alignment in specific tasks, missing-modality evaluation, and dynamic gating.

Plausible synthesis: a bounded workspace as an inter-organ communication bus; reliability-conditioned routing; shared/private latent channels; sparse broadcast; combination with memory and fault isolation.

Unsupported/speculative: workspace necessity for general intelligence; universal semantic shared latents; monotonic intelligence from communication; lossless recovery of arbitrary missing information.

Mathematically incomplete/incorrect: applying KL to arbitrary hidden vectors; using spectral gap as a universal cognition-speed measure; treating attention weights as causal attribution; claiming lossless reconstruction without channel/source assumptions; claiming fixed compute/latency improvements without a cost model.

## Implementation gate

Before the first serious prototype, freeze the task/modality suite, specialist checkpoints, workspace size, parameter budget, routing definition, corruption/missingness distributions, communication accounting, causal intervention protocol, statistical replication, and pass/fail thresholds. Do not implement a full Holobiont until these controls are fixed.

## Primary/open literature

- Devillers, Maytie & VanRullen, Semi-supervised Multimodal Representation Learning through a Global Workspace. https://arxiv.org/abs/2306.15711
- Bao et al., Multimodal Data Fusion based on the Global Workspace Theory. https://arxiv.org/abs/2001.09485
- Jaegle et al., Perceiver: General Perception with Iterative Attention. https://arxiv.org/abs/2103.03206
- Zhao, Zhang & Geng, Deep Multimodal Data Fusion, ACM Computing Surveys 56(9), 2024. https://doi.org/10.1145/3649447
- Wu et al., Deep Multimodal Learning with Missing Modality: A Survey. https://arxiv.org/abs/2409.07825
- Lee et al., Multimodal Sensor Fusion with Differentiable Filters. https://arxiv.org/abs/2010.13021
- Lee & Pavlovic, Private-Shared Disentangled Multimodal VAE. https://arxiv.org/abs/2012.13024
- Wang et al., Cross-Attention is Not Enough: Incongruity-Aware Dynamic Hierarchical Fusion. https://arxiv.org/abs/2305.13583
- Fedus, Zoph & Shazeer, Switch Transformers, JMLR 23 (2022). https://www.jmlr.org/papers/v23/21-0998.html

## Bottom line

The strongest evidence supports treating the proposed workspace as an engineering hypothesis for selective information sharing among specialized modules. The decisive next step is a matched-capacity causal benchmark, not a larger architecture. No whole-system validation is claimed.
