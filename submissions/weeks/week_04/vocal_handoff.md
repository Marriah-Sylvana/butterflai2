# HANDOFF — Week 04: Scoreboard 1

**Project:** ButterflAI 2.0 — Solar Sunspot Emergence  
**Student:** Marriah  
**Session date:** November 2, 2026  
**Notebook:** `04_scoreboard1.ipynb`  
**Submission:** `submissions/weeks/week_04/`  
**Status:** Tasks 32–43 completed; final candidate accepted by the demonstration class scoreboard. Local/GitHub file placement still needs verification before the final push.

## 1. Scientific objective and original hypothesis

Week 4 compared alternative classical point-process models under one fixed leave-one-solar-cycle-out evaluation. The prediction target is the emergence of northern and southern sunspot events in time and absolute latitude. The time coordinate is **s, years relative to each hemicycle's 15° latitude crossing**, not normalized solar-cycle phase.

Marriah's original, independently proposed knob was a **generalized latitude profile**, replacing the Gaussian offset penalty with `exp(-0.5 * abs(offset / w)**beta)`. The Gaussian is recovered at `beta = 2`; a value below 2 allows heavier tails. The initial hypothesis was that heavier latitude tails could improve held-out likelihood by representing off-belt emergences more accurately.

The original knob remained a genuine ablation in the notebook, even though it was not retained in the final combined candidate.

## 2. Frozen evaluation protocol

- **Development cycles:** 12, 13, 14, 15, 17, 18, 19, 21, 23; nine leave-one-cycle-out folds / 18 held-out hemicycles.
- **Sealed cycles:** 16, 20, 22; not to be fitted, inspected, tuned on, or scored for model selection.
- **Fixed domain:** `s ∈ [-6, 11]` years, `|latitude| ∈ [0°, 60°]`.
- **Fixed integration mesh:** `201 × 241`.
- **Protocol fingerprint:** `5e55384fbc54`.
- **Held-out information used:** independently measured sunspot-area record to predict amplitude, and the held-out wing's own event-derived 15°-crossing clock alignment. Shared shape parameters and amplitude mapping were fitted on training cycles only.
- **Important scope limitation:** This is retrospective conditional prediction of a held-out wing's emergence distribution, **not** an advance forecast of when a cycle starts or how large it will be. The event-derived alignment is explicitly part of the frozen protocol.

Comparisons use paired held-out likelihood differences per hemicycle-year. In the notebook's displayed NLL convention, *more negative is better*; gains reported below are positive improvements over the baseline.

## 3. Ablation experiments and model selection

The 14 catalog variants were evaluated and cached, including temporal shape, hemisphere pooling, width prescriptions, emission alternatives, conditional bounds, and the original generalized profile. Conditional wing-specific variants are protocol demonstrations/bounds rather than comparable honest predictions.

**Selected single-knob findings (held-out gain over baseline; nats per hemicycle-year):**

| Change | Gain | Interpretation |
|---|---:|---|
| Amplitude-dependent width | +0.6879 | Strongest honest individual gain; improved 16/18 wings. |
| Combined width prescription | +0.6492 | Helpful alone but not necessarily additive to other width changes. |
| Piecewise width | +0.4935 | Useful but inferior to amplitude width. |
| Two-sided temporal profile | +0.1856 | Modest positive effect. |
| Generalized latitude profile (Marriah's knob) | +0.1207 | Small positive effect independently; improved 13/18 wings. |
| Direct emission | +0.0026 | Essentially tied with baseline. |

The original greedy three-knob combination—`profile-generalized__shape-two_sided__width-amplitude`—achieved a paired gain of **+0.8975 ± 0.4175**, improving **14/18** wings.

Two follow-up leave-one-knob-out tests evaluated whether each addition earned its complexity:

- **Test A, remove generalized profile:** performance worsened by only **+0.0215 ± 0.0223** nats/hemicycle-year; median +0.0077. The simpler model was better in 8/18 wings. This is not convincing incremental evidence for retaining the extra profile parameter once the other knobs are present.
- **Test B, remove two-sided temporal shape:** performance worsened by **+0.1663 ± 0.1387**; median +0.0722. The simpler model was better in 8/18 wings. There is more apparent value to keeping temporal flexibility, but its incremental evidence is still uncertain.

**Final selection:** `shape-two_sided__width-amplitude` (candidate). It retains almost all the three-knob improvement with less complexity:

| Final metric | Value |
|---|---:|
| Held-out NLL per hemicycle-year | **−26.2966** |
| Baseline held-out NLL | −25.4206 |
| Paired gain over baseline | **+0.8760 ± 0.4030** |
| Median paired improvement | +0.3293 |
| Hemicycles improved | **15/18** |
| Median temporal KS | **0.0415** |
| KS not rejected at 5% | 8/18 |
| Reported fitted parameter count | 26 |
| Between-hemicycle NLL spread | 18.39 |

**Scientific question in the final report:** “Does combining a two-sided temporal emergence profile with amplitude-dependent latitude-belt width improve held-out sunspot-emergence prediction relative to baseline?”

The final selection reflects an observed complexity/performance tradeoff; it does not establish that the simpler and larger models are statistically equivalent or that their mechanisms are physically verified.

## 4. Metric gaming and protocol integrity

Task 41 tested ways to purchase apparent score gains by allowing information from the evaluated wing into the fit:

| Evaluation change | Apparent gain over honest baseline |
|---|---:|
| Own-event amplitude | +0.2794 |
| Leaky clock | +0.0400 |
| Wing-specific rise | +2.9630 |
| Best honest combined model (three knobs) | +0.8975 |

The 0.5-nat reference divided by the 17-year window is approximately **0.0294 nat per hemicycle-year**. The wing-specific-rise violation bought a much larger apparent gain than legitimate modeling improvements. It was therefore correctly treated as a conditional bound rather than a leaderboard candidate. The protocol fingerprint ensures that reports using different time windows/meshes cannot silently be compared.

## 5. Physical plausibility: held-out cycle 19

Task 42 generated simulated butterfly diagrams and observed-minus-expected residual maps for development cycle 19. The observed cycle has **2,711 events**, with **1,590 in 19N** and **1,121 in 19S**. The submission report for the selected final candidate records a **10.6% mean absolute hemispheric count error** on this example.

Detailed numerical and visual comparisons shown during the session for the **earlier three-knob** candidate were:

- Expected counts: 19N 1,443.01 versus 1,590 observed; 19S 1,254.35 versus 1,121 observed.
- Mean latitude centroid: 15.16° predicted versus 16.76° observed (baseline 14.80°).
- Mean latitude spread: 7.67° predicted versus 8.05° observed (baseline 7.00°).
- Simulation produced recognizable equatorward-migrating butterfly wings, but residuals showed underprediction of earlier activity, excess expected activity after the main observed concentration, a latitude centroid too near the equator, and a missed north/south event-count asymmetry.

**Attribution caveat:** The above centroid/spread and residual-map details describe the earlier *three-knob* model. They must not be represented as verified diagnostic values for the selected two-knob model without inspecting its refreshed Task 42 output. The final report's 10.6% count error is explicitly printed for the selected candidate.

The butterfly check supports basic qualitative plausibility of the compared model family, while identifying systematic limitations. Better likelihood does not establish a complete physical theory of solar emergence.

## 6. Class scoreboard and submission result

Task 43 wrote the final model report with student identifier **Marriah**, corrected scientific question, and matching fingerprint. The accepted report was named `scoreboard1_report_marriah.json` (the initially generated full-name and `unset` reports were cleaned up). The example leaderboard accepted Marriah's candidate above Alice's baseline and correctly refused Bob's deliberately incompatible fingerprint. This is a **development/demo leaderboard**, not a final test on the sealed cycles or a claim of winning the entire class.

The desired submission folder contains:

- `04_scoreboard1.ipynb` — complete, valid Jupyter notebook, including the original Task 40 result, Tests A and B, and the final-selection cell.
- `vocal_handoff.md` — this research handoff.
- `scoreboard1_report_marriah.json` — the accepted final candidate's report.

**Repository caution:** During GitHub web-interface file moves, an apparent notebook at `submissions/weeks/week04/04_scoreboard1.ipynb` was shown as **2 bytes / invalid JSON**. The correct destination uses `week_04` (underscore). Do not assume the GitHub-stored notebook is valid just because the commit succeeded. Validate the local `.ipynb` JSON and commit a real copy from VS Code; inspect the remote file after pushing. Keep the valid notebook and avoid committing accidental 2-byte placeholders or duplicate reports.

## 7. Open questions and recommendations for Week 5

1. **Generalization and selection:** Nine development cycles were reused for choosing among models. The final ranking remains provisional until evaluation on cycles 16, 20, and 22 under the course's authorized reveal process.
2. **Time residuals:** Check whether early-underprediction/later-overprediction occurs consistently across development cycles before changing the temporal law. Cycle 19 alone does not establish a universal timing defect.
3. **Spatial residuals:** The observed/predicted centroid and spread differences on cycle 19 motivate further spatial checks; the earlier three-knob profile should not be credited with a unique gain without conditional ablation evidence.
4. **Original knob:** Inspect fitted `beta` across folds to determine whether the optimizer reliably favors heavier-than-Gaussian tails, and test its stability; the independent positive ablation need not imply value in the final joint model.
5. **Correlation and uncertainty:** Northern and southern wings from the same cycle may be correlated, so paired hemicycle-level standard errors should not be overinterpreted as independent-cycle uncertainty.
6. **Physical identifiability:** Neither latent magnetic-field strength nor transport mechanism is uniquely established by improved point-process likelihood. The magnetic-transport idea from Week 3 remains a prospective physical hypothesis, not a demonstrated Week 4 result.
7. **Maintain the ruler:** Preserve the integration domain, time origin, amplitude-information rule, withheld-cycle seal, and protocol fingerprint when extending the model family to any neural-field methods.

## 8. Working practices for the next session

- Preserve results and implementation cells; document later model-selection amendments rather than overwriting the experimental history.
- Compare paired held-out changes, uncertainty, KS, physical plausibility, and complexity; avoid judging a candidate by a single leaderboard number.
- Use the notebook's actual objects/sign conventions and confirm that any generated report corresponds to the current `MY_RESULT` rather than stale cached metadata.
- For Git submission, verify all three requested files exist at the intended local path and that `.ipynb` passes a JSON/notebook validation before pushing.

**End-of-session status:** Scientific tasks completed and final candidate accepted in the demo scoreboard. GitHub file-location/validity cleanup and joint notebook–handoff commit remain to be completed.
