# Cognitive Holobiont Research

Rigorous research dossier for the **Cognitive Holobiont Treatise**.

## Current milestone

**Milestone 03 — Formal Audit of Chapters 8–10**

The Treatise is evaluated as a conceptual distributed modular-intelligence architecture. Each major claim is classified as **established**, **plausible engineering synthesis**, **unsupported/speculative**, or **mathematically incorrect/incomplete**. No implementation or validation claim is made without evidence.

### Milestone 03 findings

- Representation consensus and probabilistic prediction consensus must be separated. KL divergence is valid only for probability distributions in a common simplex; generic hidden states require a common typed latent space and an appropriate metric.
- Standard spectral-gap convergence results apply to specified consensus dynamics. For a connected undirected graph, `dot{x}=-Lx` yields exponential disagreement decay governed by `lambda_2(L)`, but this does not establish “spectral gap = speed of thought.”
- Directed consensus requires different assumptions and cannot automatically inherit symmetric-Laplacian proofs.
- Bridge temperature can be formalized as a positive softmax temperature, but the direction of temperature adaptation under uncertainty/stress is an empirical control problem, not a universal rule.
- The Treatise's Lyapunov proof sketch is mathematically incomplete because the real architecture is nonlinear, stochastic, delayed and hybrid; a valid proof needs an explicit dynamical model and stability conditions.
- Consensus optimization is not automatically a Nash equilibrium. Nash language requires explicit individual utilities and unilateral-deviation conditions.
- HyperNetworks establish learned weight generation, not arbitrary faithful regeneration. Regeneration must be evaluated as behavioral/task fidelity under a defined distribution and error tolerance.
- “85% regeneration” is undefined until the metric, evaluation distribution, baseline and confidence interval are specified.
- Entropy is only one health signal. Calibration, OOD detection, disagreement, provenance, latency and behavioral tests should be combined into a fault-risk model.
- Byzantine robustness is assumption-dependent; robust aggregation guarantees cannot be transferred directly to nonlinear model reconstruction.
- Attack-memory can plausibly improve resilience, but anti-fragility requires statistically demonstrated improvement on held-out future stressors, not merely recovery after an attack.
- Dynamic sleeping/hibernation may reduce average compute, but the Treatise's 60–70% figure requires workload and hardware measurements.
- The proposed 3–5 consensus rounds and 2–3x latency are empirical hypotheses, not theoretical consequences.
- Zero downtime and “immortality” remain speculative system-level claims.

## Research dossier

- `research/00_milestone_01_literature_and_math_audit.md` — initial literature and mathematical audit.
- `research/01_milestone_02_claim_equation_evidence_audit.md` — chapter/claim/equation audit and evidence matrix.
- `research/02_milestone_03_formal_audit_chapters_8_10.md` — formal consensus, stability, regeneration and self-modification audit.

## Methodology

For each claim: define the proposition → type every mathematical object → identify primary evidence → compare conflicting findings → state assumptions → derive or correct equations → define a falsification experiment → specify statistical evaluation. Surveys are used for discovery; primary papers are preferred for decisive claims.

## Evidence principles

The project does not treat biological analogy as proof. Reliability, regeneration, anti-fragility, consciousness, autonomy and AGI-level claims require explicit operational definitions and measurable tests. A successful component experiment does not validate the whole Holobiont.

## Next milestone

**Milestone 04:** formalize Chapter 13 as a preregistered benchmark specification: datasets, specialist models, bridge architecture, objective functions, routing, fault injection, recovery protocol, baselines, statistical replication, and acceptance/falsification criteria. Implementation remains downstream of this specification.
