# ButterflAI 2.0 — Week 1 tutoring handoff

HANDOFF
turns: 33+ (visible continuation messages; earlier tutoring turns not reliably countable from this record)
learns by: independent reading and hands-on scientific computation, with targeted AI help for Python implementation, debugging, and interpretation; prefers research-level discussion over introductory quizzes

domain: point processes | level: developing applied familiarity | milestone: implemented and compared counting-process statistics and Poisson nulls; independent verbal deployment not established | anchor: Fano factor
domain: statistical inference | level: developing applied familiarity | milestone: computed two latitude-model scores and compared rate-matched null models; independent verbal deployment not established | anchor: negative log-likelihood
domain: solar cycle physics | level: established astrophysics/research background, new to this specific pipeline | milestone: completed analysis of 24 aligned hemispheric cycles | anchor: alignment coordinate

point process | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
event time | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
counting process | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
inter-arrival time | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
Poisson process | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
independent increments | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
memoryless | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
Fano factor | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
overdispersion | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
negative log-likelihood | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
Earth Mover's Distance | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
null model | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
emergence | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
hemispheric cycle | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
alignment coordinate | claim: no | prior: none | end: introduced | "no independently worded definition recorded"
other deployed: GitHub/Colab notebook submission workflow (practical use); no additional scientific terms independently deployed in the visible dialogue
other open: interpreting time-rescaling diagnostics; distinguishing rate variation from event dependence; observational first-detection delays

## Week 1 research and session record

The student came prepared: attended and took notes during the live lecture, completed Colab and other environment setup before the first lecture, reviewed prior ButterflAI 1.0 presentations and earlier research/programming work, and read the Week 1 notebook. The student has an astrophysics research background and used AI primarily to accelerate Python implementation and debugging. Prior preparation is self-reported and should not be mistaken for evidence of terminology deployed during this conversation.

During the AI-assisted coding session, Tasks 1–9 were implemented, debugged, and exercised in Colab, yielding Figures 1–7. AI supplied significant portions of the code, while the student executed the notebook, supplied outputs, resolved issues interactively, and rebuilt/saved the work. The handoff does not present this implementation as unaided coding.

- Latitude-model evaluation covered 22,089 events. Finite-event mean negative log-likelihood was about 3.2024 nats/event; 110 events (0.50%) received zero density, making the strict overall mean infinite. Excluding impossible events must be disclosed whenever the finite average is reported.
- Earth Mover's Distance was computed between observed and sampled model latitude distributions in alignment-time windows; it is measured in degrees and does not evaluate event rates.
- The emergence analysis used first observations of groups with peak area above 30 MSH in cycles 12–23: 22,122 events across 24 hemispheric cycles, approximately 253.6 hemicycle-years, and mean rate about 87.2 events per hemicycle-year.
- Within-hemicycle inter-arrival gaps numbered 22,098. Median gap was approximately 2.01 days; 16.4% were zero at catalog timestamp resolution.
- Fano factors increased with window width. At 27 days, observed F was approximately 4.03 north and 3.79 south; a homogeneous Poisson benchmark gave F near 1.
- Rate-matched inhomogeneous Poisson simulations reduced the apparent discrepancy: at 27 days the one-year smoothed null gave approximately 3.96 north and 3.80 south. A three-month smoothing bandwidth changed the comparison. Thus overdispersion relative to a homogeneous Poisson model does not by itself establish intrinsic event clustering or memory.

**Interpretive limits:** First observation is a proxy for physical emergence, not its exact time; observing delays and cadence matter on day-to-rotation timescales. A Fano-factor comparison depends on estimated time-varying rates and smoothing choices. Week 2's point-process likelihood and time-rescaling checks can probe the model more directly. The visible conversation primarily documents implementation, result checking, and GitHub recovery; it does not provide enough independently phrased scientific explanations from the student to mark target terms as 'deployed'.

## Submission note

The completed notebook was committed to the student's GitHub fork. The existing repository has a folder `submissions/weeks/week_01/` whereas a separate student guide was previously read as specifying `submissions/week_01/`. Confirm the grading path with the instructors; place this handoff next to the notebook in the accepted submission directory. Do not submit a pull request unless instructors specifically request one.
