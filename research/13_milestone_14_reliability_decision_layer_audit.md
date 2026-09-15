# Milestone 14 — Reliability / Decision Layer Audit

**Status:** research-only; no whole-system validation claimed.

## Scope

This milestone audits the layer above B0–B3 that turns model outputs, disagreement, novelty, calibration, and uncertainty into decisions about **prediction, routing, memory writes, escalation, and recovery**. The central question is whether the Treatise can define a mathematically defensible system-level "health/confidence" variable without conflating distinct phenomena.

## 1. Core conclusion

There is no single generally valid scalar health variable. At minimum, the system must distinguish:

1. **Aleatoric uncertainty** — irreducible uncertainty conditional on the available information.
2. **Epistemic/model uncertainty** — uncertainty about the model/function or unexplored regions.
3. **Predictive calibration** — whether reported probabilities correspond to empirical correctness frequencies under the relevant distribution.
4. **OOD/novelty evidence** — evidence that an input differs from the reference distribution; OOD is not synonymous with error.
5. **Disagreement** — variation among organs/experts; disagreement can be legitimate under heterogeneous specialisation.
6. **Selective risk** — observed error among cases on which the system chooses to act.
7. **Decision loss** — application-specific cost of acting, abstaining, deferring, routing, or recovering.

Therefore a Holobiont reliability layer should be a **vector of calibrated signals plus a decision policy**, not an unqualified scalar.

## 2. Formal decision layer

Let the system observe a state

`S_t = (x_t, p_t, u_t, o_t, d_t, m_t, r_t)`

where `p_t` is predictive probability information, `u_t` uncertainty features, `o_t` OOD/novelty features, `d_t` inter-organ disagreement, `m_t` memory/provenance state, and `r_t` routing/reliability state.

Let the action space be

`A = {predict, abstain, defer(j), request_more_evidence, quarantine(i), recover(i), write_memory}`.

Define a loss `L(a,y,S)` that includes task loss and explicit operational costs. The rational target is

`pi*(S) = argmin_a E[L(a,Y,S) | S]`

under an explicit uncertainty model or a statistically justified risk-control procedure.

This is preferable to a rule such as `health = 1 - uncertainty`, because different uncertainty sources can imply different actions.

## 3. Selective prediction is the correct mathematical abstraction for abstention

For predictor `f` and selection function `g(x) in {0,1}`, define coverage

`C(g) = P(g(X)=1)`

and selective risk

`R_sel(f,g) = E[L(f(X),Y) g(X)] / C(g)`

when `C(g)>0`.

A reliability policy can therefore be evaluated through its **risk–coverage curve**, not merely average accuracy.

SelectiveNet provides an established neural architecture for jointly learning prediction and rejection and demonstrates improved risk–coverage trade-offs on its evaluated tasks. This supports selective prediction as an engineering primitive, not the stronger claim that its guarantees transfer automatically to arbitrary distribution shifts. citeturn1search16

## 4. Calibration is not correctness

A classifier is perfectly calibrated under distribution `P` if

`P(Y=1 | q(X)=p) = p`

for relevant probability values `p`.

Temperature scaling modifies logits as

`q_k(x) = softmax(z(x)/T)_k`

and can substantially improve calibration without changing argmax predictions on the calibration distribution. Guo et al. established this as a strong practical baseline, while also showing that modern neural networks can be poorly calibrated. citeturn1search12turn1search13

**Correction:** calibrated probability is not a guarantee that an individual prediction is correct. Calibration is distribution-dependent and can degrade under shift.

## 5. OOD detection is a separate decision problem

The classic maximum-softmax baseline showed that confidence can help distinguish some correct, incorrect, and OOD cases, but also explicitly left room for methods that outperform it. citeturn1academia48turn1search0

Thus the Treatise must not define

`OOD score = 1 - confidence`

as a mathematical identity.

The correct abstraction is a score `s_OOD(x)` followed by a threshold or decision rule whose operating characteristics are measured on specified shifts.

Required metrics include AUROC/AUPR where appropriate, false-positive/false-negative tradeoffs, and—more importantly for the Holobiont—**risk conditional on accepting a sample**.

## 6. Ensembles: useful but not an oracle

Deep ensembles provide a practical source of predictive uncertainty and can perform well under distribution shift, but ensemble uncertainty is not automatically calibrated or epistemically correct. The literature contains counterexamples showing that ensembling can have subtle calibration interactions. citeturn1search14turn0academia48

For the Holobiont, disagreement among organs must therefore be treated as an observable feature, not a direct estimate of Byzantine probability or model uncertainty.

## 7. Conformal / selective risk control

Recent open work makes the connection between selective prediction and formal risk control especially relevant. Selective Conformal Risk Control combines sample selection with conformal risk control and reports finite-sample or PAC-style guarantees under its stated exchangeability/calibration assumptions. citeturn0search0turn0search1

SCoRE generalizes selective trust decisions to bounded continuously-valued risks using e-values and conformal inference, again under explicit statistical assumptions. citeturn0search2turn0search8

**Important limitation:** these guarantees do not mean "safe under arbitrary distribution shift." Exchangeability, calibration-set integrity, score construction, and the target risk definition remain part of the theorem's assumptions.

## 8. Cost-aware deferral

The Holobiont's routing layer should include deferral/recovery costs explicitly. For actions `a`, define

`J(pi) = E[L_task + lambda_B B + lambda_C C + lambda_L L + lambda_M M + lambda_D D + lambda_F F]`

where terms represent task loss, communication, compute, latency, memory, deferral/recovery, and failure costs.

A recent open clinical study combines calibrated prediction, conformal sets, and cost-aware deferral under temporal distribution shift, illustrating the usefulness of an explicit cost function rather than a generic confidence threshold. It is application-specific evidence, not a universal guarantee. citeturn0search4turn0search5

## 9. Multi-organ uncertainty aggregation

For organ outputs `q_i(y|x_i)`, naive averaging

`q(y) = sum_i w_i q_i(y)`

is only justified by an explicit ensemble model or decision rule. If weights depend on the same evidence, or organ errors are correlated, treating each organ as independent evidence overstates effective information.

A simple correlated-evidence diagnostic is the residual covariance matrix

`Sigma_e = Cov(e_1,...,e_n)`

where `e_i` is a defined prediction residual/error feature. Effective diversity should be reported alongside accuracy and calibration.

**Unsupported shortcut:**

`health = mean_i confidence_i`

or

`health = 1 - mean_i entropy_i`

has no general statistical justification.

## 10. Reliability should be action-specific

Different signals should gate different actions:

| Signal | Strongest defensible use | Not justified by itself |
|---|---|---|
| Calibration error | probability correction / operating-point selection | truth guarantee |
| Predictive entropy | uncertainty feature | OOD proof |
| Ensemble disagreement | epistemic/disagreement feature | Byzantine attribution |
| OOD score | novelty detection | incorrectness proof |
| Conformal score | coverage/risk control under assumptions | arbitrary-shift safety |
| Memory provenance | write/read trust policy | semantic truth |
| Router instability | routing health diagnostic | organ maliciousness |
| Repeated task failure | escalation/recovery trigger | root-cause attribution |

## 11. Recovery and memory-write gates

A proposed memory-write rule should require more than high confidence:

`write = accept AND provenance_ok AND policy_ok AND risk_bound_ok AND consistency_ok`.

A recovery rule should similarly be separated from diagnosis:

`recover(i)` may be triggered by sustained decision-level risk, but the system must not infer that organ `i` is Byzantine merely because it is low-confidence or disagrees with peers.

This preserves the distinction established in earlier Byzantine audits between **anomaly, uncertainty, heterogeneity, and malicious behavior**.

## 12. Common-mode failure is a first-class threat

If every organ consumes the same corrupted memory item, shared workspace state, or poisoned external source, then consensus can become unanimous and wrong.

Therefore

`high agreement != high correctness`.

The experiment suite must include common-mode corruption where all organs receive the same false evidence. Independent evidence channels and provenance diversity should be measured.

## 13. Calibration under shift

Calibration must be evaluated separately on:

- IID/in-distribution test data
- covariate shift
- label/prior shift where applicable
- missing-modality conditions
- corrupted inputs
- adversarial inputs
- memory-poisoned contexts
- common-mode workspace corruption

A single global ECE number is insufficient. Reliability diagrams, Brier/NLL, selective risk, coverage, and shift-stratified results should be reported.

Calibration estimators themselves can be unstable in finite samples; the dossier therefore requires sensitivity to binning/estimator choices rather than treating ECE as ground truth.

## 14. Proposed reliability vector

Define

`rho_t = [cal_t, risk_t, ood_t, dis_t, prov_t, drift_t, cov_t]`

with each component explicitly defined and separately calibrated/evaluated.

A policy then computes

`a_t = pi_theta(rho_t, S_t)`

subject to risk/resource constraints.

The research question is whether this vector-policy approach improves the Pareto frontier over simpler baselines:

1. confidence-only
2. entropy-only
3. disagreement-only
4. OOD-only
5. confidence + disagreement
6. calibrated vector policy
7. conformal/selective policy
8. oracle upper bound using labels (evaluation only)

## 15. New hypotheses

### H49 — calibrated selective routing

At matched coverage and compute, calibrated selective routing reduces conditional task risk relative to confidence-only routing.

**Falsifier:** no improvement or worse risk after confidence calibration.

### H50 — disagreement is non-identifying

Under heterogeneous but honest specialists, disagreement alone cannot reliably distinguish faulty from merely specialized organs.

**Falsifier:** a disagreement-only rule maintains high fault-attribution precision/recall across non-IID conditions.

### H51 — vector reliability dominates scalar health

A vector of separately evaluated reliability signals improves routing/recovery decisions relative to any single scalar confidence score at matched cost.

**Falsifier:** scalar baseline Pareto-dominates the vector policy.

### H52 — common-mode faults defeat consensus

Correlated corruption causes consensus-based confidence to become overconfident unless evidence/provenance diversity is modeled.

**Falsifier:** consensus remains reliably calibrated under controlled common-mode corruption without independent evidence.

### H53 — conformal/selective control improves accepted-case risk

When exchangeability/calibration assumptions hold, conformal selective policies achieve their target risk/coverage more reliably than uncalibrated thresholding.

**Falsifier:** repeated matched experiments violate the claimed finite-sample/PAC target under the stated assumptions.

### H54 — decision-aware recovery beats confidence-triggered recovery

Recovery policies that optimize expected task/recovery cost outperform policies triggered by a fixed uncertainty threshold.

**Falsifier:** fixed threshold has a better cost-risk frontier over the preregistered fault/shift suite.

### H55 — provenance reduces persistent-memory failure propagation

Validated provenance and write gates reduce long-horizon error propagation from poisoned or stale memory at matched memory budget.

**Falsifier:** provenance gating produces no reliability benefit or introduces larger downstream losses at matched resources.

## 16. B4 experiment matrix

**B4.1 Calibration:** specialist and fused outputs; temperature scaling and non-parametric baselines; IID and shifted evaluation.

**B4.2 Selective prediction:** risk–coverage curves; abstention/defer costs; confidence vs calibrated confidence.

**B4.3 OOD:** multiple semantic and corruption shifts; measure detection and accepted-case risk.

**B4.4 Correlated disagreement:** vary specialist error correlation while holding marginal accuracy fixed.

**B4.5 Common-mode fault:** poison shared memory/workspace/source evidence and measure consensus overconfidence.

**B4.6 Reliability vector:** train/fit a policy over independent calibration data; evaluate on held-out shifts/faults.

**B4.7 Recovery:** compare confidence-triggered, disagreement-triggered, conformal/selective, and decision-cost policies.

## 17. Statistical requirements

All primary comparisons require:

- fixed train/calibration/test splits before evaluation;
- at least 5 independent random seeds for stochastic learning experiments unless a stronger repeated-measures design is preregistered;
- bootstrap confidence intervals for risk/coverage curves and paired tests where appropriate;
- shift/fault stratification;
- calibration-set separation from final test data;
- explicit multiple-comparison handling when many routing policies are compared;
- complete compute, communication, latency, memory, and abstention accounting.

Conformal claims must state the exact exchangeability or related assumption and the target risk/coverage quantity.

## 18. Claim classification update

### Established result

- Selective prediction has a formal risk–coverage framework.
- Neural networks can be poorly calibrated; temperature scaling is a strong baseline.
- OOD detection is a distinct problem with established benchmark methods.
- Deep ensembles can provide useful predictive uncertainty.
- Conformal methods can provide finite-sample distribution-free guarantees for defined quantities under exchangeability-style assumptions.
- Cost-aware deferral is a legitimate decision-theoretic formulation.

### Plausible engineering synthesis

- Reliability-vector state for the Holobiont.
- Separate gates for prediction, routing, memory writes, quarantine, and recovery.
- Conformal/selective risk control as one possible high-level trust gate.
- Provenance diversity as a common-mode-failure defense.
- Decision-aware recovery minimizing expected task plus operational cost.

### Unsupported/speculative

- A universal scalar "health" variable.
- Confidence as a universal proxy for truth.
- Consensus as proof of correctness.
- Disagreement as proof of Byzantine behavior.
- OOD detection as complete safety monitoring.
- Conformal guarantees under arbitrary distribution shift.
- A calibrated reliability layer guaranteeing safe autonomous self-healing.

### Mathematically incorrect/incomplete

- `health = 1 - entropy` as a universal quantity.
- `health = average(confidence_i)` under correlated evidence without a probabilistic justification.
- `disagreement => Byzantine`.
- `OOD => incorrect`.
- `calibrated probability => correct prediction`.
- `conformal coverage => individual-example guarantee`.
- `consensus => correctness` under common-mode corruption.

## 19. Decision gate after Milestone 14

Do not implement autonomous recovery based on an undefined health score. First implement/evaluate B4 as an offline decision layer on frozen B0–B3 outputs.

Proceed to autonomous recovery only if:

1. calibration is characterized under the intended shifts;
2. selective policies produce reproducible risk–coverage improvements;
3. disagreement is shown not to be a reliable fault label under honest heterogeneity;
4. common-mode failure is measurable;
5. memory-write and recovery gates have explicit cost/risk semantics;
6. any conformal guarantee is reported only under its stated assumptions.

## 20. Open problems

- Online calibration under nonstationary specialist populations.
- Dependence-aware conformal control for correlated organs.
- Joint calibration of multimodal set-valued outputs.
- Reliability estimation when labels arrive only after delayed feedback.
- Decision-theoretic fusion of epistemic, aleatoric, OOD, and provenance signals.
- Robust uncertainty estimates under adversarial and memory-poisoned contexts.
- Formal bounds for recovery decisions with endogenous routing and memory.
- How to prevent the reliability layer itself becoming a single point of failure.

## 21. Sources / provenance

Primary and authoritative open sources reviewed for this milestone include:

- Guo et al., *On Calibration of Modern Neural Networks*, ICML/PMLR 2017. https://proceedings.mlr.press/v70/guo17a.html
- Hendrycks & Gimpel, *A Baseline for Detecting Misclassified and Out-of-Distribution Examples in Neural Networks*, ICLR 2017. https://arxiv.org/abs/1610.02136
- Lakshminarayanan et al., *Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles*, NeurIPS 2017 / arXiv. https://arxiv.org/abs/1612.01474
- Geifman & El-Yaniv, *SelectiveNet: A Deep Neural Network with an Integrated Reject Option*, ICML/PMLR 2019. https://proceedings.mlr.press/v97/geifman19a.html
- Xu, Guo & Wei, *Selective Conformal Risk Control*, arXiv 2512.12844, v2 (2026). https://arxiv.org/abs/2512.12844
- Bai & Jin, *Conformal Selective Prediction with General Risk Control*, arXiv 2603.24704 (2026). https://arxiv.org/abs/2603.24704
- Sokol, Moniz & Chawla, *Conformalized selective regression*, Discover Data 2026. https://doi.org/10.1007/s44248-026-00113-2
- Kwon & Kim, *Conformal selective prediction with cost aware deferral for safe clinical triage under distribution shift*, Scientific Reports 2026. https://www.nature.com/articles/s41598-026-40637-w
- Rahaman & Thiery, *Uncertainty Quantification and Deep Ensembles*, arXiv 2007.08792. https://arxiv.org/abs/2007.08792
- Zhang, Kailkhura & Han, *Mix-n-Match: Ensemble and Compositional Methods for Uncertainty Calibration in Deep Learning*, arXiv 2003.07329. https://arxiv.org/abs/2003.07329

## Final evidence position

The strongest defensible Milestone 14 conclusion is that **reliability should be modeled as a decision layer over multiple separately characterized signals, with explicit costs and statistical assumptions**. This is a plausible engineering synthesis grounded in established selective prediction, calibration, OOD, uncertainty, and conformal-risk literature. It does not validate autonomous self-healing, universal fault attribution, or a single Holobiont health variable.
