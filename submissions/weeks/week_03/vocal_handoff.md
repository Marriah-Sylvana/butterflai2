# HANDOFF — Week 03: The Latent Magnetic Belt

**Project:** ButterflAI 2.0 — Solar Sunspot Emergence  
**Session date:** October 27, 2026  
**Notebook:** `03_latent_belt.ipynb`  
**Status:** Completed; notebook pushed to GitHub.

## 1. Scientific objective

Investigate whether a latent, migrating magnetic belt can explain both the temporal emergence rate of sunspots and their latitude distribution, extending Week 2's time-only emergence-rate model.

The model uses a shared temporal profile and equatorward drift, with hemicycle-specific magnetic amplitudes and a global emission scale.

The student completed Tasks 20–31, including alignment, two-dimensional likelihood construction, partial pooling, synthetic simulation and recovery, leave-one-cycle-out evaluation, numerical scoring sensitivity, and comparisons of belt-width prescriptions.

## 2. Computational results

### Alignment and reference clock

- Fixed `MU_REF = 15°` to define a consistent relative-time coordinate.
- Fitted shared latitude-decay timescale: `mu_b = 8.024 ± 0.109 years`.
- Hemicycle clock shifts ranged from approximately −0.230 to +0.320 years.
- Typical shift uncertainty: approximately 0.111 years.
- All 16,611 development events remained within the fixed time window after alignment.

The student connected this process to coordinate reference systems already familiar from astronomy, including spatial grids and standardized time coordinates.

### Frozen integration domain

- Relative time: `s ∈ [−6, 11]` years.
- Absolute latitude: `[0°, 60°]`.
- Integration mesh: `201 × 241` nodes, totaling 48,441 evaluation points.
- Mesh and domain were kept fixed for model comparisons.

The student clarified that the computational mesh is a numerical sampling grid, not a physical mechanism that confines observed sunspots.

### Pooled magnetic-belt fit

Initial constant-width fit:

- `k = 7.490`
- `theta = 0.8461 years`
- `mu_a = 14.606°`
- `mu_b = 7.339 years`
- `w = 6.486°`
- `c = 0.053532`
- Per-hemicycle amplitudes approximately 116–345 times the fixed critical-field scale.

The field threshold was fixed at `B_crit = 1`, establishing an amplitude convention while leaving important physical-identifiability limitations.

### Synthetic simulation and parameter recovery

- Generated synthetic emergence catalogs using two-dimensional thinning.
- A representative four-cycle comparison contained 9,077 observed events and 9,016 synthetic events.
- Largest fractional recovery error across six shared parameters: 1.33%.
- Mean recovery error for hemicycle amplitudes: 2.8%; worst: 8.0%.

**Interpretation:** The simulator and fitter are internally consistent in this test. This does not establish that the Sun possesses the particular latent magnetic-belt structure assumed by the model.

### Held-out evaluation

Nine leave-one-cycle-out folds evaluated both hemispheres of each excluded development cycle.

Negative log-likelihood per hemicycle-year:

- In-sample: −26.322
- Held out, amplitude predicted from area record: −25.178
- Held out, amplitude fitted using the wing's own events: −25.462

Paired penalty for area-record amplitude:

- Mean: +0.284 nats per hemicycle-year.
- Standard error: 0.084.
- Between-hemicycle score spread: approximately 18.1.

The student recognized why within-hemicycle comparisons are more meaningful than unpaired comparisons of raw likelihood scores.

**Methodological qualification:** The provided Task 28 execution cell uses `start=POOLED["q"]`, initialized from a pooled development fit. Although the model is refitted within each training fold, independent initialization sensitivity has not been established. Some results were loaded from existing CSV caches, so their original calculation settings were not independently verified during this session.

Held-out clock alignment also uses the held-out hemicycle's own emergence observations. These results should therefore be described as retrospective held-out-shape evaluations, not fully prospective forecasts.

### Model diagnostics

Temporal KS goodness-of-fit:

- Median KS distance: latent belt 0.0355; Week 2 direct rate 0.0258.
- Passed at the 5% level: latent belt 11/18; direct rate 17/18.

The pooled magnetic-belt model sacrifices temporal flexibility while adding latitude information and sharing temporal parameters across hemicycles.

Physical-plausibility diagnostics:

- Mean absolute held-out event-count error: 8.8%.
- Worst count error: 31.6%.
- Mean modeled latitude centroid: 15.38°, versus observed 14.86°.
- Mean modeled latitude spread: 7.71°, versus observed 6.93°.

The constant-width model predicts nearly the same normalized latitude distribution across hemicycles, whereas the observations display meaningful differences.

### Scoring sensitivity: Task 29

The fitted model was held fixed while evaluation conventions changed.

| Evaluation | Score change |
|---|---:|
| Latitude domain extended to 90° | +0.5031 |
| Time window extended to s = 15 | +5.2090 |
| Coarse 101 × 31 mesh | −0.0396 |

The negative-log-likelihood score can appear better solely because numerical integration becomes less accurate. Fixed meshes, domains, and normalization conventions are essential for valid comparisons.

### Latitude spread and width models

Observed scaling with event count:

- Residual latitude-spread exponent: +0.3879; correlation +0.950.
- Raw latitude-spread exponent: +0.3385; correlation +0.905.

Three width prescriptions were evaluated:

| Model | In-sample LL/year | Held-out area-record LL/year |
|---|---:|---:|
| Constant width | 26.3221 | 25.1785 |
| Amplitude-dependent | 26.7965 | 25.6097 |
| Latitude-dependent | 26.3933 | 25.2602 |

Higher log-likelihood values are better.

- Amplitude-dependent fitted exponent: 0.3992.
- Latitude-dependent model: `w = 5.1126 + 0.0885 mu`.
- Paired improvement over constant width, amplitude model: mean 0.431, median 0.166 nats per hemicycle-year; 15/18 hemicycles improved.
- Latitude model: mean improvement 0.082; median 0.064; 12/18 improved.
- Cycle 19N contributes an unusually large improvement of approximately 3.17 to the amplitude-dependent model's paired score.

**Interpretation:** Amplitude-dependent belt width performs best among the three tested prescriptions. The mean improvement is influenced substantially by a strong hemicycle, so medians and sign counts must accompany means.

## 3. Scientific reasoning developed in the tutor discussion

### Intrinsic belt width versus emergence-region width

The student challenged an initially ambiguous analogy describing a magnetic-field profile whose height changes while its width stays fixed.

The distinction was clarified:

- The intrinsic Gaussian width parameter can remain unchanged.
- Increasing the field amplitude raises the field strength at all latitudes.
- A larger latitude region can consequently exceed a fixed emergence threshold.
- A wider observed emergence distribution does not necessarily establish an intrinsically wider magnetic belt.

This distinction is critical when interpreting the inferred width–amplitude scaling.

### Alternative physical hypothesis: magnetic transport

The student proposed that solar magnetic structures may behave in ways analogous to interacting circulation patterns in Earth's oceans and atmosphere.

The scientific interpretation is that evolving distributions of sunspot emergence could reflect magnetic transport, interacting field structures, and plasma flows rather than changes in one Gaussian belt-width parameter alone.

Relevant physical processes include differential rotation, convection, meridional flow, and magnetic-flux transport.

This is a proposed interpretation for future investigation, **not an established result of the Week 3 statistical analysis**.

The tutor and student distinguished the magnetic field itself from the electrically conducting plasma whose motion is coupled to field evolution through magnetohydrodynamics.

### Statistical interpretation

The student developed the distinction between:

- Numerical scoring improvements and real model improvements.
- In-sample fit and held-out generalization.
- Raw likelihood differences and paired within-hemicycle comparisons.
- Empirical latitude-spread scaling and its causal interpretation.
- A statistical latent field and a physically verified subsurface magnetic structure.

The student explicitly requested clarification of unfamiliar terminology when necessary, and connected the numerical mesh to familiar geographic and astronomical coordinate-grid concepts.

## 4. Outstanding questions and limitations

1. **Physical identifiability:** The latent field's absolute strength is not established. Large inferred amplitudes reduce the discriminating influence of the critical threshold.
2. **Weak-cycle regime:** The proposed emergence behavior below critical field strength has not been tested against cycles 12–23.
3. **Width mechanism:** Better performance from amplitude-dependent width does not identify the underlying magnetohydrodynamic mechanism.
4. **Generalization:** Test whether optimizer initialization using full-development parameters materially affects leave-one-cycle-out results.
5. **Uncertainty:** Consider cycle-level paired uncertainty because northern and southern hemicycles may be correlated.
6. **Model sensitivity:** Assess whether the amplitude-width advantage remains substantial when influential cycle 19N is excluded.
7. **Documentation consistency:** The notebook's closing narrative reports an amplitude-width mean/median improvement of 0.68/0.29, whereas the executed comparison produced 0.431/0.166. The closing reference-latitude gauge cost also differs from an earlier diagnostic (approximately 27 versus 22.46 nats). These differences remain unreconciled.
8. **Reproducibility:** Verify provenance and settings of cached cross-validation outputs before treating them as newly recomputed results.

## 5. Carry forward to Week 4

Reuse the established alignment convention, frozen integration domain, shared latent-belt model, synthetic-wing generator, and leave-one-cycle-out framework.

Maintain paired model comparisons, record both the mean and median improvement, and distinguish predictive evidence from descriptive fit and physical interpretation.

Preserve the student's magnetic-transport hypothesis as an avenue for future physical modeling.

**End-of-session status:** Week 3 notebook completed and pushed. Handoff prepared for saving to `submissions/weeks/week_03/vocal_handoff.md` and committing to GitHub.
