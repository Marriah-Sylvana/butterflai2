# ButterflAI 2.0 — Week 02 Tutor Handoff

## HANDOFF

HANDOFF
turns: 29
learns by: worked examples, executable Colab code, numerical checks, comparison of figures and outputs; short sequential instructions

domain: point processes | level: 2 | milestone: 2 reached | anchor: compensator
inhomogeneous Poisson process | claim: no | prior: none | end: introduced | "no quote"
intensity function | claim: no | prior: none | end: introduced | "no quote"
compensator | claim: no | prior: none | end: introduced | "no quote"
thinning | claim: no | prior: none | end: introduced | "no quote"
time-rescaling theorem | claim: no | prior: none | end: introduced | "no quote"
other deployed: none demonstrated independently in the visible discussion
other open: independence of rescaled gaps; genuine clustering versus catalog-day ties

domain: statistical inference | level: 2 | milestone: 2 reached | anchor: identifiability
log-likelihood | claim: no | prior: none | end: introduced | "no quote"
maximum likelihood estimate | claim: no | prior: none | end: introduced | "no quote"
quadrature | claim: no | prior: none | end: introduced | "no quote"
unconstrained parameterization | claim: no | prior: none | end: introduced | "no quote"
QQ plot | claim: no | prior: none | end: introduced | "no quote"
Kolmogorov–Smirnov statistic | claim: no | prior: none | end: introduced | "no quote"
identifiability | claim: no | prior: none | end: introduced | "no quote"
profile likelihood | claim: no | prior: none | end: introduced | "no quote"
other deployed: none demonstrated independently in the visible discussion
other open: distinguishing verification from validation; calibration of KS p-values after fitting; evidence for pooling

domain: solar cycle physics | level: 2 | milestone: 2 reached | anchor: emergence rate
emergence rate | claim: no | prior: none | end: introduced | "no quote"
onset | claim: no | prior: none | end: introduced | "no quote"
other deployed: none demonstrated independently in the visible discussion
other open: why group first-observation peaks and smoothed-area peaks need not coincide; mechanism versus phenomenological fit

**Level/quotation note:** These conservative ratings reflect the limited learner-authored technical explanations in the visible exchange: most technical text supplied by the learner was copied notebook specifications, computed results, or tutor directives. Such pasted text is evidence of completed exercises, but not independent deployment of vocabulary. No learner quotations have been invented. Earlier Week 01 HANDOFF levels and priors were not available for faithful carry-forward.

## Week 02 computation and reasoning record

- **Scope and convention:** Week 02 `02_rate_1d.ipynb`. An event is a sunspot group's **first observation**, not a physically dated emergence. `s` means years since that hemisphere's mean-latitude crossing of 15 degrees; it is a time shift, **not** normalized phase. Observation window fixed at `s ∈ [-6,+11]` years, 4001 quadrature nodes. Development set: 18 hemicycles in cycles 12–23, excluding sealed cycles **16, 20, and 22**; 16,611 observed events in development. No fitting, plotting, tuning, or scoring on the withheld cycles.
- **Tasks 10–12, likelihood and fitting:** Implemented event-time point-process likelihood `sum(log λ(s_i)) - integral λ(s) ds`; trapezoid integration; gamma-shaped positive intensity and unconstrained parameter mapping; independent multi-start Nelder–Mead fits. Constant-rate baseline **−164.57 nats/hemicycle-year**; gamma **−203.87**, an in-sample improvement of **39.30** (lower is better). All 18 fitted expected counts match observations; maximum absolute discrepancy approximately **1.96e−5 events**. Quadrature mesh refinement demonstrated roughly fourfold convergence of successive differences under step halving.
- **Tasks 13–14, simulation and recovery:** Thinning a constant-rate Poisson proposal under a finite rate bound. Example 19N: 5,552 proposed, 1,589 kept, expected count ~1,590. Twenty synthetic replicates generated from fitted 19N parameters and refit with the same gamma family. Worst `abs(bias / replicate SD)` was below 1 (about 0.72); notebook check threshold 3 passed. Recovered derived peak and width were steadier than some original shape parameters. **This checks implementation under a correctly specified generating family, not truth of that family for the Sun.**
- **Task 15, deliberate failures:** Dropping the integral led to unbounded scale/shape behavior: compared with proper 19N `a≈4.5907`, `k≈7.872`, predicted 1,590, the incorrect fit produced `a≈104.09`, `k≈293.878` and expected count `inf`. The optimizer stopped numerically, not at a meaningful statistical optimum. Incorrect integration span `[−3,+11]` instead of `[−6,+11]` moved the 19N fitted peak by about **1.36 years** and implied **2,061.574** events over the true window versus 1,590 observed; `[−6,+6]` implied 1,609.440. Window and event sample must match.
- **Tasks 16–17, residual timing:** Built the compensator by cumulative trapezoid integration, interpolated to event times, and computed adjacent rescaled gaps, including the first gap from window start. Daily timestamp ties: zero-gap fraction 10.3%–25.3% by hemicycle, median 15.6%; 19N maximum 25.3%. Explicit policy: independent uniform within-one-day jitter before re-sorting. KS statistic implementation matched SciPy exactly for test samples `n=200,2000`. Median KS: fitted gamma+jitter **0.0259**, gamma with ties **0.1554**, constant+jitter **0.2790**. Pooled KS values **0.0150**, **0.1705**, **0.2690**, respectively. Nominal KS `p>0.05` in 17/18 gamma+jitter fits; constant rejected in all 18. **Caution:** In-sample fitting and artificial tie-breaking complicate standard KS p-value calibration; distributional agreement does not establish independence or exclude physical clustering. The QQ plot favored the gamma-shaped time dependence.
- **Task 18, profile likelihood and identifiability:** At each fixed onset, reoptimized amplitude, theta, and k. 19N best onset at the permitted lower boundary `−6` (boundary issue); 13N best onset near `−3` (interior). Across profile range for 19N, `k` changed by factor **2.45**, theta by **1.73**, yet width only ~**10.3%** and peak position ~**0.53 years**. For 13N, corresponding factors **3.88**, **2.17**, width ~**10.1%**, peak ~**0.72 years**. Across-hemicycle variation relative to within-ridge ambiguity (within 4 NLL units) was far greater for **derived peak and width** than for raw k and theta. **11/18** onset estimates hit the window edge; do not claim that edge is the physical start of solar emergence. The ridge-range ratios are descriptive, not formal significance tests.
- **Task 19, independent area plausibility:** Joined `fits` to `amplitudes` on `hemicycle`, using area amplitude `A` and `s_peak_area`; produced `plausibility` and the three expected uppercase correlation variables. Correlation fitted emergence peak vs 12-month-smoothed area peak **+0.683**; event count vs amplitude **+0.941**; fitted width vs amplitude **−0.301**. Mean `s_peak − s_peak_area` **−0.144 years**, SD **0.718 years**, maximum absolute difference **1.384 years** (21S). Emergence frequency and area on the disk are different observables; correlation is not causal validation.
- **Figures:** Figures 1–8 were generated and discussed; Figure 8 includes profile likelihood, parameter compensation, fitted emergence peak versus area peak, and count versus amplitude.

## Prediction integrity and self-check

The instructor's TUTOR DIRECTIVEs called for learner-owned, **pre-computation** predictions and explanations. Much of this session instead proceeded by direct implementation and interpretation of outputs; the learner explicitly prioritized finishing the assignment. Do **not** retroactively claim that the learner made the requested predictions, independently formulated technical conclusions, or deployed all targets. Predictions about integral omission and identifiability were primarily assistant-supplied. The scientific numbers and executable work were verified through notebook outputs; independent conceptual articulation remains an open learning goal.

## Carry into Week 03

**Open question—pooling:** Would shared gamma shape parameters across 18 hemicycles stabilize weakly identified individual fits, or erase genuine between-wing differences? Decide using a fair, properly validated comparison rather than assuming pooling helps. Week 03 introduces pooling and held-out scoring; **cycles 16, 20, and 22 remain sealed** until the designated program reveal. Distinguish *verification* (simulator/fitter recovery) from *validation* (does a candidate intensity adequately describe solar events?). Start the next tutor session by inviting the learner to describe in their own words how the compensator works, why profile likelihood diagnoses identifiability, and what pooled fitting might gain or sacrifice.

## Submission status

User reported the completed `02_rate_1d.ipynb` saved to Google Drive and GitHub. This `vocal_handoff.md` was generated in the working environment for placement beside that notebook in the **Week 02 submission folder**. Git push and remote presence of this HANDOFF have **not** been verified in this tutor session; they remain to be completed.
