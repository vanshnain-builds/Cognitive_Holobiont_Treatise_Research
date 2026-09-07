# Milestone 06 — Threat, Experiment, and Regeneration Information-Budget Audit

**Date:** 2026-09-07  
**Status:** research/design specification; no validation claim

## Executive conclusion

The most defensible near-term research object is a fault-aware modular inference system. The Treatise combines established building blocks—conditional expert routing, representation alignment, uncertainty estimation, robust aggregation, secure aggregation, redundancy, checkpointing, and controlled reconstruction—but its emergent claims (universal regeneration, immortality, zero downtime, consciousness, autonomous AGI evolution) remain unsupported hypotheses.

The central research question is: **what information survives a specialist failure, what information is required to reconstruct its function, and how can the system distinguish honest specialization from malicious or faulty behavior?**

## 1. Claim classification

| Claim | Classification | Required correction |
|---|---|---|
| Specialists handle different subproblems | Established result | Supported by modular networks and sparse MoE. |
| Sparse routing increases capacity without proportional per-input compute | Established, conditional | Must account for communication, load balance, and training stability. Switch Transformer is a primary reference: https://www.jmlr.org/papers/v23/21-0998.html |
| Specialists communicate in learned latent spaces | Established in narrower settings | Does not by itself establish the Holobiont architecture. |
| Shared latent bridges align heterogeneous specialists | Plausible engineering synthesis | Define common space, alignment loss, and information-loss metric. |
| Consensus reduces disagreement | Established for specified dynamics | State graph/connectivity/update assumptions. |
| Spectral gap measures cognitive speed | Unsupported/speculative | It bounds convergence for particular consensus systems, not cognition. |
| Disagreement proves a faulty organ | Mathematically/operationally incomplete | Honest non-IID specialization can produce legitimate disagreement. |
| Trust combines performance, uncertainty, disagreement and history | Plausible engineering synthesis | Calibrate and test false quarantine/reintegration. |
| Secure aggregation guarantees privacy | Established only under protocol assumptions | Secure aggregation is not differential privacy and does not remove malicious-client risk. https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/ |
| Hypernetworks can generate specialist weights | Established in restricted architectures | Does not imply arbitrary faithful regeneration. https://arxiv.org/abs/1609.09106 |
| A surviving generator can regenerate any specialist | Unsupported/speculative | Requires a restricted specialist family and sufficient surviving information. |
| 85% parameter regeneration equals 85% functional recovery | Mathematically incorrect | Use held-out behavioral/task-space fidelity. |
| Shadow copies guarantee recovery | Unsupported | Requires explicit independence, freshness, and integrity assumptions. |
| Redundancy implies immortality | Unsupported/speculative | Reliability depends on a defined failure model and redundancy independence. |
| Recovery creates anti-fragility | Unsupported/speculative | Recovery is not improvement; test held-out related stress. |
| Global workspace broadcast creates consciousness | Unsupported/speculative | Broadcast is a computational mechanism, not evidence of subjective experience. |
| Self-modification produces open-ended AGI evolution | Unsupported/speculative | Adaptation does not imply general intelligence or open-ended evolution. |

## 2. Regeneration information budget

Let a specialist be \(f_\theta:\mathcal X\to\mathcal Y\), surviving artifact \(Z\), and reconstruction \(\hat\theta=g_\phi(Z)\). Define behavioral recovery by

\[
\Delta_{task}=R_D(f_{\hat\theta})-R_D(f_\theta),
\]

where \(R_D\) is risk on an untouched evaluation distribution. Representation recovery may use

\[
\Delta_{rep}=\mathbb E_{x\sim D}[d(\psi(f_\theta(x)),\psi(f_{\hat\theta}(x)))].
\]

Parameter distance \(\|\hat\theta-\theta\|\) is auxiliary only: neural parameterizations may have symmetries and parameter closeness is not a behavioral guarantee.

Vary retained artifacts across architecture/configuration, preprocessing, task objective, examples/statistics, adapters, optimizer state, calibration state, routing/interface parameters, checkpoints, and provenance. The research framing is an empirical rate-distortion relationship:

\[
C(\epsilon)=\min I(Z;F)\quad\text{s.t.}\quad \mathbb E[d(F,\hat F)]\le\epsilon.
\]

This is a conceptual information-theoretic framing, not a claim that the exact quantity is tractable for large neural networks.

## 3. Threat taxonomy

- **T1 Crash/omission:** no response; heartbeat/timeout detection.
- **T2 Staleness:** valid but obsolete state; require version metadata.
- **T3 Random fault:** noisy/corrupted output; vary magnitude and persistence.
- **T4 Byzantine:** arbitrary malicious output/update.
- **T5 Collusion:** multiple attackers coordinate to appear consistent.
- **T6 Honest non-IID disagreement:** legitimate specialization causes variation.
- **T7 Adaptive attack:** attacker changes behavior after observing detection.
- **T8 Input adversarial attack:** malicious input rather than compromised organ.
- **T9 Detector compromise:** health/trust layer is itself attacked.

Byzantine-robust learning results are conditional on assumptions. Median/trimmed-mean methods have formal guarantees, but particular robust rules can be attacked; non-IID heterogeneity makes detection harder. Primary references: https://proceedings.mlr.press/v80/yin18a.html ; https://proceedings.mlr.press/v115/xie20a.html ; https://proceedings.mlr.press/v238/bao24a.html ; https://proceedings.mlr.press/v206/allouah23a.html

## 4. Trust and quarantine

Define the target quantity rather than treating an arbitrary score as probability:

\[
q_i(t)=P(unsafe_i\mid H_i(t)),
\]

where \(H_i(t)\) contains authenticated history, task validation, uncertainty, latency, errors, and peer evidence.

A hysteretic policy is

\[
\text{quarantine if }q_i>\tau_{out},\qquad
\text{reintegrate if }q_i<\tau_{in},
\]

with \(\tau_{in}<\tau_{out}\). This reduces rapid state oscillation. Calibration must be measured; a score is not automatically a probability. ByMI is a useful reference for false-discovery-rate-controlled Byzantine identification: https://proceedings.mlr.press/v235/qian24b.html

## 5. Consensus versus correctness

For continuous-time consensus on a connected undirected graph,

\[
\dot x=-Lx,
\]

and

\[
\|x(t)-\bar{x}\mathbf1\|_2\le e^{-\lambda_2(L)t}\|x(0)-\bar{x}\mathbf1\|_2.
\]

Thus the disagreement-decay time scales with \(1/\lambda_2(L)\) for this specific system. This does **not** prove cognitive speed. The Holobiont's directed, dynamic, delayed, stochastic, nonlinear routing requires a separate theorem.

Consensus establishes agreement, not truth. A colluding incorrect coalition can agree, so correctness needs independent validation/provenance signals.

## 6. Privacy boundary

Separate transport confidentiality, authentication/integrity, secure aggregation, differential privacy where required, access control, auditability, and leakage testing. Secure aggregation protects individual contributions from the aggregator under its protocol/threat model; it is not a universal privacy guarantee. https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/

## 7. Calibration and OOD

Calibration and OOD detection are distinct. Report ECE, NLL, Brier score and reliability diagrams for calibration; report AUROC, AUPR, FPR95 and multiple distribution shifts for OOD. ECE is

\[
ECE=\sum_b\frac{|I_b|}{n}|acc(I_b)-conf(I_b)|.
\]

ECE is diagnostic, not a complete guarantee. Recent VLM work finds models are not inherently calibrated and that post-hoc calibration can improve reliability under shifts: https://proceedings.mlr.press/v235/tu24a.html. OOD definitions and failure modes should follow established taxonomies: https://arxiv.org/abs/2110.11334 ; https://arxiv.org/abs/2404.05219.

## 8. Recovery verification gates

Every regenerated specialist should pass: **A structural compatibility; B held-out functional performance; C behavioral reference-set agreement where appropriate; D OOD/adversarial safety; E calibration; F provenance/reproducibility.** Parameter matching alone is insufficient.

## 9. Experiment matrix

**E1:** monolith vs independent specialists vs routed specialists.  
**E2:** no bridge vs fixed projection vs learned bridge.  
**E3:** dense vs top-k vs learned vs uncertainty-aware routing.  
**E4:** increase honest distributional heterogeneity and measure false quarantine.  
**E5:** sign-flip, scaling, targeted-direction, intermittent, adaptive, and colluding attacks.  
**E6:** attack the detector/trust mechanism itself.  
**E7:** remove retained artifact classes one at a time and measure regeneration degradation.  
**E8:** correlated specialist/shadow failures to test redundancy assumptions.  
**E9:** adapt to one stress family, then test unseen related stress.

Define anti-fragility operationally as improvement on held-out related stress:

\[
\Delta_{AF}=R_{pre}(D_{stress})-R_{post}(D_{heldout\ stress}).
\]

Require pre-registered uncertainty intervals and baseline comparisons; recovery alone does not satisfy anti-fragility.

## 10. Minimum evidence gate

Before claiming that a prototype demonstrates the Treatise, require reproducible specialization, measurable communication/routing trade-offs, bounded false-positive behavior, held-out recovery, multiple attack classes, simpler baselines, independent seeds/runs with uncertainty intervals, and complete artifact provenance.

No evidence reviewed in this milestone validates literal immortality, consciousness, universal regeneration, zero downtime, or spontaneous AGI evolution.

## 11. Open problems

1. Minimum persistent information needed for specified behavioral recovery.
2. Trust calibration under concept drift and adversarial manipulation.
3. Byzantine detection without suppressing legitimate specialization.
4. Dynamic-graph consensus with failures, delay, and stochastic routing.
5. Distinguishing stale-but-useful from compromised specialists.
6. Preserving calibration and OOD behavior after regeneration.
7. Independence assumptions needed for redundancy to improve reliability.
8. Avoiding overfitting of adaptation to a known attack family.
9. Operational definition of self-healing versus ordinary fault tolerance.
10. Operational definitions for claims that are currently unfalsifiable.

## Decision

**Do not implement the full architecture yet.** The next scientifically useful step is a small controlled benchmark in which the simplest modular baseline is progressively augmented. The purpose is to measure whether each added mechanism yields statistically defensible benefit relative to communication, latency, compute, and failure-mode costs.

## Key sources

- Switch Transformers — https://www.jmlr.org/papers/v23/21-0998.html
- Byzantine-Robust Distributed Learning — https://proceedings.mlr.press/v80/yin18a.html
- Fall of Empires — https://proceedings.mlr.press/v115/xie20a.html
- BOBA — https://proceedings.mlr.press/v238/bao24a.html
- Fixing by Mixing — https://proceedings.mlr.press/v206/allouah23a.html
- ByMI — https://proceedings.mlr.press/v235/qian24b.html
- Fault Tolerant ML — https://proceedings.mlr.press/v235/dahan24a.html
- HyperNetworks — https://arxiv.org/abs/1609.09106
- Secure Aggregation — https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/
- Generalized OOD Detection survey — https://arxiv.org/abs/2110.11334
- VLM calibration — https://proceedings.mlr.press/v235/tu24a.html
- OOD/adversarial survey — https://arxiv.org/abs/2404.05219
