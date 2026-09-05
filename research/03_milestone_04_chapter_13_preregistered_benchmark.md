# Milestone 04 — Chapter 13 Benchmark and Falsification Specification

**Status:** pre-implementation specification. No validation of the Cognitive Holobiont is claimed.

## 1. Purpose

Chapter 13 proposes a first two-organ experiment. This milestone converts that proposal into a falsifiable benchmark. The objective is not to demonstrate that the Holobiont is generally intelligent, conscious, immortal, or anti-fragile. The objective is to test whether a small distributed modular system obtains measurable benefit from learned inter-organ communication and adaptive coordination while retaining specialist competence and recovering from controlled faults.

The benchmark must compare mechanisms incrementally. A full-system result without ablations cannot establish which component caused an effect.

## 2. Operational architecture

Let organ `i` have encoder `e_i`, task head `c_i`, and latent state

`h_i = e_i(x_i) in R^{d_i}`.

A learned bridge maps each organ to a shared interface space `R^d`:

`z_i = P_i h_i`, `P_i in R^{d x d_i}`.

For nonlinear bridges, replace `P_i h_i` with `g_i(h_i)` and report parameter count separately.

A router produces logits `s(x) in R^N` and probabilities

`pi_i(x;tau) = exp(s_i(x)/tau) / sum_j exp(s_j(x)/tau)`, `tau > 0`.

A top-k router may retain only the k largest probabilities and renormalize them. Capacity constraints must be reported because sparse MoE literature shows that routing introduces nontrivial communication and training-stability costs. Shazeer et al. established sparse learned expert routing; Switch Transformers simplified routing and documented scaling/training trade-offs. [1,2]

## 3. Benchmark levels

### B0 — Monolithic baseline

One model receives the complete available input and produces the task output. Match the distributed system as closely as possible on total training examples and evaluation data.

### B1 — Independent specialists

Two specialists operate independently. Their outputs are combined by a fixed rule or a simple calibrated ensemble. No learned bridge.

### B2 — Static latent bridge

Specialists communicate through learned projections into a common latent space. No adaptive routing.

### B3 — Adaptive routing

Add the learned router and top-k/sparse execution. Compare against dense communication.

### B4 — Health-aware routing

Add uncertainty, OOD, disagreement, latency, and integrity signals. Quarantine decisions must be evaluated independently from task performance.

### B5 — Controlled regeneration

Remove one specialist during evaluation, then reconstruct it from explicitly defined artifacts. Compare against cold restart, checkpoint restoration, and no-repair controls.

### B6 — Stress adaptation

Expose the system to a known stress family during adaptation and evaluate on both the original distribution and a held-out related stress family. This is required before using the term anti-fragility.

## 4. Data and task principle

The initial experiment should use a multimodal task with genuinely complementary evidence so that communication can have a measurable opportunity to help. A suitable first benchmark is a paired vision-language or image-text classification/reasoning task with controlled modality corruption.

For each example define `(x_v, x_t, y)`. The visual organ receives `x_v`; the language organ receives `x_t`. The complete benchmark must include:

- clean in-distribution evaluation;
- missing-modality evaluation;
- noisy-modality evaluation;
- distribution-shift/OOD evaluation;
- specialist fault evaluation;
- Byzantine message/update evaluation.

Do not choose a task merely because the two specialists can already solve it independently. The communication channel must be tested on complementary-information cases.

## 5. Communication objectives

For shared representations, use a typed quadratic objective:

`L_rep = (1/2) sum_{i,j} w_ij ||z_i-z_j||_2^2`.

For predictive distributions, if `p_i, p_j in Delta^{K-1}` are distributions over the same labels, a directional KL term is

`KL(p_i || p_j) = sum_k p_{ik} log(p_{ik}/p_{jk})`.

For symmetric agreement, use Jensen-Shannon divergence or another explicitly chosen symmetric discrepancy. Hidden vectors must not be passed to KL unless they have first been mapped to valid probability distributions.

The total objective should be written as

`L = L_task + lambda_rep L_rep + lambda_pred L_pred + lambda_bal L_balance + lambda_reg L_reg`.

Every coefficient must be tuned using a validation protocol independent of the final test set.

## 6. Routing objective

A router should be evaluated for both utility and specialization. One possible regularized objective is

`L_router = L_task + lambda_b L_balance + lambda_c C(pi)`,

where `C(pi)` represents measured communication/compute cost.

A load-balance statistic should be reported separately from task loss. A router that sends nearly all examples to one organ may achieve good aggregate accuracy while destroying the proposed distributed specialization.

For temperature adaptation, constrain

`tau in [tau_min,tau_max]`.

Do not assume that uncertainty must always increase or decrease temperature. Treat the sign and magnitude of adaptation as an empirical control policy.

## 7. Consensus and graph model

For representation consensus on an undirected weighted graph `G`, define

`L_G = D-W`,

and stacked `z` gives

`L_rep = z^T (L_G tensor I_d) z`.

For continuous linear consensus

`dot z = -L_G z`,

if the graph is connected and undirected,

`||z(t)-1 z_bar||_2 <= exp(-lambda_2(L_G)t) ||z(0)-1 z_bar||_2`.

Therefore a disagreement-reduction target `epsilon` has sufficient convergence time

`t_epsilon <= (1/lambda_2) log(1/epsilon)`

for the corresponding normalized initial disagreement. This bound belongs to the linear consensus model only. The neural Holobiont contains nonlinear transformations, stochasticity, delays, discrete routing, and possible topology changes. Those effects require separate experiments/theory.

For directed graphs, use row-stochastic/column-stochastic dynamics and report the precise connectivity and spectral assumptions. Do not substitute a symmetric `lambda_2` theorem without justification. Olfati-Saber, Fax & Murray provide the relevant consensus framework for directed information flow, failures, topology changes and delays. [3]

## 8. Regeneration protocol

Define original specialist `f_theta` and regenerated specialist `f_hat_theta`.

Regeneration is successful only if it satisfies a pre-registered behavioral criterion. For scalar task loss `ell`, define

`R = E_{x~D_eval}[ell(f_hat_theta(x),y)] - E_{x~D_eval}[ell(f_theta(x),y)]`.

A successful reconstruction could require

`R <= delta_R`

with a confidence interval entirely below the pre-specified tolerance, or an equivalent performance-retention criterion.

For probabilistic predictions, report NLL and Brier score in addition to accuracy. For representations, define the exact downstream task used to establish equivalence.

The phrase “85% regenerated” is prohibited unless 85% is tied to an explicit metric. A weight-space similarity is not automatically a behavior-space similarity.

## 9. Regeneration artifacts

Test separate information sources:

- H0: no reconstruction information;
- H1: last valid checkpoint;
- H2: hypernetwork-generated initialization;
- H3: distributed shadow/witness representations;
- H4: H2 + H3 + constrained fine-tuning.

HyperNetworks demonstrate target-weight generation, but not arbitrary recovery of destroyed models. [4]

Measure recovery from each artifact and quantify information loss from deleting selected witnesses.

## 10. Byzantine model

The first security experiment should use a declared fault fraction `f/n`, with separate attacks:

1. random sign/scale perturbation;
2. targeted model replacement;
3. label/data poisoning;
4. adaptive message manipulation;
5. colluding witnesses.

Robust aggregation results must not be transferred automatically from federated optimization to neural regeneration. Yin et al. provide formal median/trimmed-mean guarantees under stated optimization assumptions. Empirical studies show robust aggregation can degrade substantially under non-IID data and some attack settings. [5,6]

Report:

- clean accuracy;
- attacked accuracy;
- recovery accuracy;
- false quarantine rate;
- missed Byzantine rate;
- recovery time;
- communication overhead.

## 11. Health model

Do not use entropy as a sole failure detector. Define an observable health vector

`q_i = [E_i, O_i, D_i, T_i, I_i, A_i]`,

where the components represent task error, OOD rate, inter-organ disagreement, latency, integrity/provenance, and availability.

Train or calibrate a risk function

`r_i = P(F_i=1 | q_i, context)`.

The detector must be evaluated independently using AUROC, AUPRC, expected calibration error, false-quarantine cost, and missed-fault cost.

Guo et al. demonstrate that neural confidence can be poorly calibrated; deep ensembles provide a practical uncertainty baseline; Mahalanobis feature-space methods provide one OOD baseline. [7,8,9]

## 12. Privacy

If training updates or witness states leave an organ, define the privacy threat explicitly. Federated learning keeps training data decentralized but does not by itself guarantee privacy. Secure aggregation can hide individual updates from an aggregator under its cryptographic threat model, but aggregate leakage, malicious clients, side channels and inference attacks remain separate questions. [10,11]

For the first benchmark, do not claim privacy merely because raw examples remain local. If privacy is a research objective, report a formal mechanism and privacy budget where applicable.

## 13. Anti-fragility criterion

Use three measurements:

`P0` = performance before stress;
`P1` = immediate post-stress performance;
`P2` = performance after adaptation on a held-out, related stress distribution.

Robustness: acceptable `P1` degradation.

Resilience: recovery of `P1` toward `P0`.

Anti-fragility evidence: `P2 > P0` by a pre-specified practically meaningful margin on held-out future stress, without unacceptable regression on clean data.

Repeated exposure to the same attack is not enough to establish general anti-fragility because the system may simply memorize the attack.

## 14. Cognitive mitosis / self-modification safety

Treat cloning as controlled architecture search. A child specialist is accepted only after validation and safety gates. A minimal abstract process is

`g_child ~ Mutation(g_parent)`

`theta_child = HyperNet(g_child, context)`

followed by constrained adaptation. The parent must remain immutable during the trial. No self-modification should directly alter production routing or safety policy without an external validation gate.

Progressive Networks and PackNet provide precedents for preserving old capabilities while adding new ones, but they do not establish unrestricted autonomous evolutionary improvement. [12,13]

## 15. Required statistical protocol

For every comparison:

- at least 5 independent random seeds for the initial study;
- report mean, standard deviation and 95% confidence intervals;
- use paired evaluation examples whenever the comparison permits;
- pre-specify primary metrics before inspecting the test set;
- report all failed runs and aborted recoveries;
- separate hyperparameter tuning data from final test data;
- publish exact configuration, model hashes, dataset versions and fault-injection seeds.

A result should not be called a win if it improves one metric while violating a pre-registered primary constraint.

## 16. Primary hypotheses

### H1 — Complementary latent communication

`B2 > B1` on the complementary-information subset without statistically significant specialist-retention loss.

### H2 — Adaptive routing improves efficiency

`B3` achieves equal-or-better task performance than `B2` while reducing measured compute or communication under a defined budget.

### H3 — Health-aware routing improves reliability

`B4` reduces expected fault-related decision cost relative to `B3`, including false-quarantine cost.

### H4 — Regeneration is behaviorally meaningful

`B5` reconstructs a removed specialist to a pre-registered behavioral tolerance better than cold restart and checkpoint-only controls under artifact loss.

### H5 — Byzantine robustness is nontrivial

The complete system degrades gracefully under declared Byzantine fractions compared with naive aggregation, without assuming IID data unless that is explicitly the experimental condition.

### H6 — Anti-fragility is stronger than recovery

After adaptation to stress family A, performance improves on held-out stress family B relative to the pre-stress system, with no unacceptable clean-task regression.

## 17. Claims that remain unsupported

The following Treatise-level claims must not be presented as established outcomes:

- universal or near-universal regeneration;
- literal “immortality” of the intelligence;
- guaranteed zero downtime;
- spontaneous evolution toward AGI;
- consciousness from a global latent workspace;
- a universal relationship between graph spectral gap and cognitive speed;
- fixed 60–70% compute savings;
- fixed 3–5 consensus rounds;
- fixed 2–3x latency;
- fixed 85% regeneration fidelity.

Each may become a research hypothesis, but none is a theorem merely because the constituent mechanisms exist in the literature.

## 18. Literature ledger

[1] Shazeer et al. (2017), *Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer*. https://arxiv.org/abs/1701.06538

[2] Fedus, Zoph & Shazeer (2021), *Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity*. https://arxiv.org/abs/2101.03961

[3] Olfati-Saber, Fax & Murray (2007), *Consensus and Cooperation in Networked Multi-Agent Systems*. https://www.cds.caltech.edu/~murray/papers/2007c_ofm07-ieeeproc.html

[4] Ha, Dai & Le (2016), *HyperNetworks*. https://arxiv.org/abs/1609.09106

[5] Yin et al. (2018), *Byzantine-Robust Distributed Learning: Towards Optimal Statistical Rates*. https://proceedings.mlr.press/v80/yin18a.html

[6] Li, Ngai & Voigt (2023), *An Experimental Study of Byzantine-Robust Aggregation Schemes in Federated Learning*. https://arxiv.org/abs/2302.07173

[7] Guo et al. (2017), *On Calibration of Modern Neural Networks*. https://proceedings.mlr.press/v70/guo17a.html

[8] Lakshminarayanan, Pritzel & Blundell (2017), *Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles*. https://arxiv.org/abs/1612.01474

[9] Lee et al. (2018), *A Simple Unified Framework for Detecting Out-of-Distribution Samples and Adversarial Attacks*. https://proceedings.neurips.cc/paper/2018/hash/abdeb6f575ac5c6676b747bca8d09cc2-Abstract.html

[10] McMahan et al. (2017), *Communication-Efficient Learning of Deep Networks from Decentralized Data*. https://research.google/pubs/communication-efficient-learning-of-deep-networks-from-decentralized-data/

[11] Bonawitz et al. (2017), *Practical Secure Aggregation for Privacy-Preserving Machine Learning*. https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/

[12] Li & Hoiem (2016), *Learning without Forgetting*. https://arxiv.org/abs/1606.09282

[13] Mallya & Lazebnik (2018), *PackNet: Adding Multiple Tasks to a Single Network by Iterative Pruning*. https://openaccess.thecvf.com/content_cvpr_2018/html/Mallya_PackNet_Adding_Multiple_CVPR_2018_paper.html

## 19. Decision gate before implementation

Implementation may begin only after the benchmark has:

- fixed task/dataset and split policy;
- fixed specialist roles and model versions;
- fixed bridge dimension and routing parameterization;
- defined every loss term and coefficient-selection procedure;
- defined the fault and Byzantine models;
- defined regeneration artifacts and fidelity metric;
- defined primary/secondary metrics;
- defined statistical replication;
- defined acceptance and falsification criteria;
- defined reproducibility artifacts.

Until then, claims about the full Cognitive Holobiont remain hypotheses.
