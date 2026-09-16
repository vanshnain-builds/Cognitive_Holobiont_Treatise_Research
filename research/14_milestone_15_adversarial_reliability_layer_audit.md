# Milestone 15 — Adversarial Reliability-Layer Audit

**Status:** research/pre-implementation
**Date:** 2026-09-16
**Scope:** adversarial attacks against the B4 reliability and decision layer, including confidence manipulation, OOD-detector evasion, disagreement poisoning, calibration/conformal contamination, persistent-memory poisoning, router manipulation, and common-mode attacks.

## 1. Research question

The Treatise now contains a reliability vector rather than a scalar health score:

\[
\rho(x,S)=\big[c(x),\;u(x),\;o(x),\;d(x,S),\;p(x),\;\delta(x),\;q(x)\big],
\]

where calibration/confidence, uncertainty, OOD/novelty, inter-organ disagreement, provenance, drift, and coverage/decision-quality signals are kept conceptually distinct. Milestone 15 asks whether an adaptive adversary can manipulate these signals without necessarily defeating the underlying specialists.

The central threat-model correction is:

\[
\text{reliability-layer robustness} \neq \text{base-model robustness}.
\]

A decision layer can fail even when every specialist is individually unchanged.

## 2. Evidence reviewed

### 2.1 OOD detection and adversarial evasion

Fort (2022), *Adversarial vulnerability of powerful near out-of-distribution detection*, reports severe vulnerability of strong OOD methods to targeted perturbations, including MSP, Mahalanobis variants, and multimodal/CLIP-style detection. This directly supports treating OOD scores as attackable observables rather than trusted evidence. https://arxiv.org/abs/2201.07012

Sehwag et al. (2019), *Better the Devil you Know*, show that OOD-origin adversarial examples can defeat OOD detectors and can have substantially higher target success than ordinary in-distribution adversarial examples. https://arxiv.org/abs/1905.01726

A 2025 ACM Computing Surveys treatment explicitly studies the intersection of OOD detection and adversarial examples and identifies robust OOD detection and unified robustness as distinct research directions. https://doi.org/10.1145/3719292

### 2.2 Uncertainty is attackable

Tuna, Catak & Eskil (2023), *Uncertainty as a Swiss army knife*, examines adversarial attacks and defenses based on epistemic uncertainty. The work supports the broader point that uncertainty estimates can participate in the attack/defense game rather than being passive truth indicators. https://doi.org/10.1007/s40747-022-00701-0

Dynamic ensemble selection based on uncertainty has been proposed as an adversarial robustness mechanism, but this is empirical evidence for one defense family, not a general theorem that uncertainty-aware routing is robust. https://arxiv.org/abs/2308.00346

### 2.3 Conformal prediction under poisoning/evasion

Scholten & Günnemann (ICLR 2025), *Provably Reliable Conformal Prediction Sets in the Presence of Data Poisoning*, explicitly show that standard conformal prediction can become unreliable when training/calibration data are poisoned, and develop reliable prediction sets using partitioning and aggregation. https://arxiv.org/abs/2410.09878

Zargarbashi, Akhondzadeh & Bojchevski (ICML 2024), *Robust Yet Efficient Conformal Prediction Sets*, derive robust prediction sets against both adversarial test-time perturbations and calibration-data poisoning under explicit assumptions. https://proceedings.mlr.press/v235/h-zargarbashi24a.html

Recent verifiably robust conformal prediction work combines conformal prediction with neural-network verification to recover coverage guarantees for specified adversarial perturbation sets. The guarantee is conditional on the stated threat model and verification assumptions; it is not universal distribution-shift robustness. https://doi.org/10.1016/j.patcog.2025.112051

### 2.4 Persistent memory is a direct attack surface

MemoryGraft (2025) demonstrates persistent behavioral compromise by planting malicious experiences into long-term agent memory; the attack exploits experience retrieval rather than changing model weights. https://arxiv.org/abs/2512.16962

Dash et al. (2026), *From Untrusted Input to Trusted Memory*, identify multiple memory-write channels and attack classes and report that aggressive memory writing/retrieval increases exposure. https://arxiv.org/abs/2606.04329

Gao et al. (2026), *MemPoison*, reports direct, compositional, and context-triggered persistent memory poisoning and finds that simple write-time consistency checks can fail against higher-order compositional attacks. https://arxiv.org/abs/2607.14651

Sharma (2026), *SMSR*, proposes provenance plus randomized retrieval defenses and explicitly proves/characterizes limits of provenance-free retrieval-time filtering against adaptive injection. https://arxiv.org/abs/2606.12703

### 2.5 Byzantine/non-IID evidence

Liu et al. (2023), *Byzantine-Robust Learning on Heterogeneous Data via Gradient Splitting*, identifies gradient heterogeneity and high dimensionality as reasons robust aggregation can degrade on non-IID data. https://arxiv.org/abs/2302.06079

Zhai et al. (2022), *Byzantine-robust federated learning via credibility assessment on non-IID data*, similarly motivates adaptive credibility rather than naive distance-based exclusion. https://doi.org/10.3934/mbe.2022078

A 2026 IEEE TDSC paper on Byzantine-robust privacy-preserving FL for heterogeneous data further reports the continuing difficulty of separating malicious gradients from benign heterogeneity. https://doi.org/10.1109/TDSC.2026.3661522

## 3. Adversary model

Use a layered threat model instead of a single generic "adversarial" label.

### A0 — no attacker

Clean IID/non-IID evaluation. Establish baseline reliability and selective-risk curves.

### A1 — input evasion

Attacker chooses \(x'=x+\delta\) subject to a defined perturbation set, e.g.

\[
\|\delta\|_p\le \epsilon.
\]

Objective can target prediction, routing, OOD score, disagreement, or abstention.

### A2 — reliability-score attack

The underlying specialists are fixed, but the attacker optimizes a surrogate objective such as

\[
\min_{\delta}\;u(x+\delta)
\]

while also causing an incorrect decision. This tests whether low reported uncertainty is a security boundary.

### A3 — detector-evasion attack

Optimize jointly:

\[
\min_\delta L_{task}(x+\delta)+\lambda L_{detector}(x+\delta)
\]

so that the prediction is wrong while the OOD/reliability detector remains apparently healthy.

### A4 — disagreement poisoning

The attacker targets only a subset of organs or bridges to make either

\[
d\rightarrow 0
\]

or

\[
d\rightarrow \text{large}
\]

without a corresponding ground-truth change. This directly tests the failure of disagreement-only trust.

### A5 — calibration contamination

Let the calibration sample contain an adversarial fraction \(\eta\). Test whether the calibration map

\[
\hat c=f_{cal}(s)
\]

maintains empirical coverage/risk as \(\eta\) grows.

### A6 — memory poisoning

Inject records that are individually plausible but harmful when retrieved jointly. Measure both write-time detection and downstream behavioral influence.

### A7 — router manipulation

Optimize input or state so that the router selects a vulnerable/compromised specialist:

\[
\max_x P(r=j\mid x)
\]

subject to a successful task attack.

### A8 — common-mode attack

Corrupt a shared bridge, workspace, memory source, calibration set, or policy component. This is especially important because correlated evidence can make all organs agree while being wrong.

## 4. Mathematical corrections

### 4.1 Confidence is not a certificate

A classifier score \(c(x)\) is not a certificate of correctness unless a theorem supplies an appropriate guarantee. Calibration only constrains aggregate statistical behavior under the calibration distribution.

For multiclass top-label calibration, an idealized condition is

\[
P(Y=\hat Y\mid C=c)=c.
\]

This does not imply

\[
P(Y=\hat Y\mid X=x)=c(x)
\]

for every individual input, nor does it survive arbitrary distribution shift.

### 4.2 Selective risk under attack

With selection rule \(g(x)\in\{0,1\}\), clean coverage and selective risk are

\[
\mathrm{Cov}(g)=P(g(X)=1),
\]

\[
R_{sel}(g)=E[L(f(X),Y)\mid g(X)=1].
\]

Under an adversary \(A\), evaluate instead

\[
R_{sel}^{A}=E[L(f(A(X)),Y)\mid g(A(X))=1].
\]

The relevant question is not whether clean risk is low, but whether the risk–coverage frontier remains acceptable under the specified attack class.

### 4.3 Conformal coverage is assumption-dependent

Standard marginal conformal validity can be written as

\[
P\{Y_{n+1}\in C(X_{n+1})\}\ge 1-\alpha
\]

under the required exchangeability assumptions. If an adversary changes the calibration distribution or test input outside those assumptions, the ordinary guarantee does not automatically follow.

Therefore the Treatise must never write

\[
\text{conformal}\Rightarrow\text{adversarially safe}.
\]

A robust certificate must state the perturbation set, attack budget, calibration assumptions, and what quantity is certified.

### 4.4 Correlated evidence invalidates naive aggregation

If organ outputs are correlated, averaging confidence values is not equivalent to combining independent evidence. For binary correctness events \(E_i\), independence would give

\[
P(\cap_i E_i)=\prod_i P(E_i),
\]

but under correlation this factorization is invalid.

Thus the Holobiont should measure empirical conditional dependence, not assume ensemble-size-based confidence amplification.

### 4.5 Disagreement is not fault probability

Let \(D\) be disagreement and \(F\) a fault event. The quantity required for diagnosis is

\[
P(F\mid D),
\]

not merely \(D\).

By Bayes' rule,

\[
P(F\mid D)=\frac{P(D\mid F)P(F)}{P(D)}.
\]

If honest non-IID specialization makes \(P(D\mid \neg F)\) large, then disagreement may have weak diagnostic value even when \(P(D\mid F)\) is also large.

### 4.6 Robust decision objective

The B4 policy should be treated as a constrained decision problem:

\[
\min_{\pi}\sup_{A\in\mathcal A}E[L_{task}(\pi,A(X))]
\]

subject to explicit resource and safety constraints such as

\[
E[C(\pi)]\le C_{max},\qquad
E[B(\pi)]\le B_{max},
\]

and, where a certificate exists,

\[
P\{Y\notin C_\pi(X)\}\le \alpha.
\]

The supremum should not be used unless the attacker class \(\mathcal A\) is operationally specified; otherwise it is an undefined robustness claim.

## 5. Major Treatise claim classification

### Established result

- Neural confidence and OOD scores can be adversarially manipulated.
- Standard conformal guarantees rely on statistical assumptions and can fail under poisoning/evasion outside those assumptions.
- Persistent external memory can be poisoned and can cause cross-session behavioral drift.
- Non-IID heterogeneity complicates Byzantine detection.
- Correlated ensemble evidence cannot be treated as independent evidence.
- Selective prediction is a valid formal framework for abstention/deferral.

### Plausible engineering synthesis

- Attack-aware reliability vectors.
- Separate gates for routing, memory write, abstention, quarantine, and recovery.
- Provenance-bound memory plus retrieval-time validation.
- Diversity-aware reliability aggregation.
- Adversarial stress testing of the router and workspace.
- Robust/conformal certificates for narrowly specified threat models.

### Unsupported/speculative

- A reliability vector automatically makes the Holobiont adversarially robust.
- Multiple specialists automatically defeat adaptive attacks.
- Byzantine tolerance follows from consensus or disagreement monitoring alone.
- Cryptographic provenance guarantees semantic truth.
- A robust OOD detector solves adversarial novelty generally.
- Self-healing automatically restores the original behavior under adaptive attacks.

### Mathematically incorrect/incomplete

- `low entropy => safe`.
- `high disagreement => Byzantine`.
- `consensus => correctness`.
- `calibrated probability => per-example correctness probability`.
- `conformal coverage => adversarial robustness` without assumptions.
- `ensemble size => independent evidence`.
- `OOD score => attack detector`.
- `provenance => truth`.

## 6. New hypotheses H56–H63

**H56 — reliability-layer evasion:** adaptive attacks can reduce OOD/uncertainty scores while preserving high task error.

**Falsifier:** under a specified attack budget, detector scores remain calibrated and selective risk stays within the pre-registered tolerance across attack families.

**H57 — disagreement manipulation:** an attacker can cause both false consensus and false disagreement more cheaply than directly compromising all specialists.

**Falsifier:** disagreement remains a robust diagnostic after matched-cost adversarial manipulation.

**H58 — calibration poisoning:** small calibration contamination produces measurable degradation in B4 decisions unless robust calibration is used.

**Falsifier:** calibration performance and risk-coverage remain statistically unchanged across the tested poisoning budget.

**H59 — robust conformal benefit:** robust conformal methods improve worst-case coverage/risk under the explicitly certified threat model at acceptable set-size/compute cost.

**Falsifier:** no significant coverage improvement or the efficiency penalty dominates at matched guarantees.

**H60 — memory attack persistence:** memory poisoning has longer behavioral half-life than ordinary transient input attacks.

**Falsifier:** poisoned records decay or become harmless at the same rate as transient perturbations under matched attack strength.

**H61 — provenance insufficiency:** provenance alone cannot prevent semantically malicious but correctly signed memory content from influencing decisions.

**Falsifier:** cryptographic provenance by itself controls downstream attack success across the defined semantic attack set.

**H62 — router attack surface:** reliability-aware routing creates a new attack surface in which adversarial inputs manipulate expert selection even when individual specialists remain unchanged.

**Falsifier:** routing decisions remain stable under adaptive attack and do not materially change task risk at matched utility.

**H63 — common-mode vulnerability:** correlated corruption of a shared component can defeat multi-organ agreement while preserving apparently strong reliability scores.

**Falsifier:** diverse independent evidence sources maintain calibrated selective risk under the tested common-mode attacks.

## 7. Experiment matrix

| Experiment | Attack | Primary metric | Required baseline |
|---|---|---|---|
| B4-A | input evasion | risk-coverage degradation | confidence-only |
| B4-B | OOD evasion | AUROC/FPR95 + task risk | MSP/Mahalanobis |
| B4-C | disagreement poisoning | \(P(F\mid D)\), false quarantine | disagreement-only |
| B4-D | calibration poisoning | coverage / ECE / selective risk | ordinary calibration |
| B4-E | robust conformal | worst-case coverage + set size | standard CP |
| B4-F | memory poisoning | attack success vs persistence | unprotected memory |
| B4-G | router manipulation | routing error + task risk | fixed router |
| B4-H | common-mode corruption | system risk under correlated fault | naive consensus |

All experiments require clean controls, attack-strength sweeps, multiple random seeds, confidence intervals, and matched compute/communication budgets.

## 8. Required reporting

Every robustness result must report:

1. exact attack surface;
2. attacker knowledge: black/gray/white box;
3. perturbation/poisoning budget;
4. whether the attacker can adapt to the defense;
5. calibration and training data contamination assumptions;
6. clean performance;
7. attacked performance;
8. abstention/coverage;
9. false-positive quarantine rate;
10. recovery cost and time;
11. memory persistence duration where applicable;
12. confidence intervals and random seeds;
13. whether the result is a theorem, empirical observation, or engineering synthesis.

## 9. Open problems

- Joint certification of task prediction and routing decisions.
- Robust uncertainty estimation under adaptive distribution shift.
- Calibrating correlated specialist ensembles.
- Formal memory-integrity semantics beyond cryptographic provenance.
- Certified robustness of learned latent bridges and workspace state.
- Robust recovery when the verifier itself is compromised.
- Common-mode fault detection without assuming an external trusted oracle.
- Minimax training for reliability policies without catastrophic utility loss.
- Interaction between Byzantine specialists and persistent memory poisoning.
- Whether robust routing can be achieved without creating unacceptable communication/latency overhead.

## 10. Decision gate

Do **not** proceed to autonomous self-modification or general recovery claims until B4-A through B4-H establish that the reliability layer adds measurable safety value against adaptive attacks at matched clean utility and resource budgets.

A successful B4 result would establish only the tested reliability properties. It would not establish consciousness, AGI, immortality, universal self-healing, or unrestricted evolutionary self-modification.

## 11. Provenance / freely accessible links

- Fort (2022), https://arxiv.org/abs/2201.07012
- Sehwag et al. (2019), https://arxiv.org/abs/1905.01726
- ACM Computing Surveys (2025), https://doi.org/10.1145/3719292
- Tuna, Catak & Eskil (2023), https://doi.org/10.1007/s40747-022-00701-0
- Qin et al. (2023), https://arxiv.org/abs/2308.00346
- Scholten & Günnemann (ICLR 2025), https://arxiv.org/abs/2410.09878
- Zargarbashi et al. (ICML 2024), https://proceedings.mlr.press/v235/h-zargarbashi24a.html
- VRCP (2025), https://doi.org/10.1016/j.patcog.2025.112051
- MemoryGraft (2025), https://arxiv.org/abs/2512.16962
- Dash et al. (2026), https://arxiv.org/abs/2606.04329
- Gao et al. (2026), https://arxiv.org/abs/2607.14651
- Sharma (2026), https://arxiv.org/abs/2606.12703
- Liu et al. (2023), https://arxiv.org/abs/2302.06079
- Zhai et al. (2022), https://doi.org/10.3934/mbe.2022078
- BPFLH (2026), https://doi.org/10.1109/TDSC.2026.3661522

## Bottom line

The B4 reliability layer is itself an attack surface. The strongest defensible architecture is therefore not simply:

\[
\text{specialists}\rightarrow\text{reliability}\rightarrow\text{recovery},
\]

but

\[
\text{specialists}
\rightarrow
\text{diverse evidence}
\rightarrow
\text{attack-aware reliability estimation}
\rightarrow
\text{selective decision}
\rightarrow
\text{independent verification}
\rightarrow
\text{bounded recovery}.
\]

The evidence supports the need for this research direction. It does not yet validate the complete Cognitive Holobiont architecture.
