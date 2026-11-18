# Two-Person Team Workflow & Report Plan

**Project:** Pedestrian Trajectory Prediction Using Machine Learning  
**Repository:** [AUR0RACY/PedestrianPrediction](https://github.com/AUR0RACY/PedestrianPrediction)  
**Team size:** **2 members** — assign names and GitHub usernames at the next meeting  
**Planning dates:** November 18–30, 2026 (Eastern Time)  
**Official submission cutoff:** **Not yet confirmed. Check Brightspace before treating Nov 30 as available.**  
**Internal target:** Complete a reviewable report by **Nov 28**; finalize by **Nov 29**, earlier if the official deadline requires.  
**Grounding:** Instructor-provided *CS4451/9548 Course Project Report* marking scheme (100 points) and full-mark IEEE-style report sample. The sample's ~8 pages is **not** a confirmed page limit.

> **This is a trajectory REGRESSION project, not crossing-intention classification.** Input: pedestrian's past image-plane positions; output: future image-plane positions. PIE bounding-box annotations, 5 Hz sampling, 1/2/3-second observation windows, 1/2/3-second forecast horizons, and ADE/FDE in pixels. **No invented experimental results.**

## 1. Rubric at a glance — plan effort by marks

| Rubric category (exact scheme) | Points | What the report must demonstrate | Main drafting lead |
|---|---:|---|---|
| **Format & Style** | **10** | Strict IEEE conference template; centered title/author block with title, **Last and First names**, department, Western University and email | B (A checks) |
| **Abstract** | **5** | Entire story: context, approach, contribution, methodology, actual results, analysis, conclusions, future work and lessons | **Both**; write last |
| **Introduction** | **10** | Context, current problem, **research gap**, objectives, work, findings, novelty/progress, theory/practice impact, report roadmap | A (B reviews) |
| **Background & Related Work** | **20** | Explain and critically compare prior research; identify a justified gap; use in-text citations | B (A contributes sources) |
| **Methods – Research Objectives** | **5** | **Numbered objective IDs** plus significance for research and practice | A (B agrees) |
| **Methods – Research Methodology** | **25** | Technical design and rationale; **empirical-study details**, RQs/hypotheses, descriptive statistics, data gathering, analysis approach and **threats to validity**; citations | A for data/splits; B for models/metrics |
| **Results** | **20** | Exact experimental evidence, tables/charts/validation; **explain why** results occur and what is new | B (A verifies results) |
| **Conclusions & Future Work** | **5** | Directly answer objectives/hypotheses, next steps and **lessons learnt** | **Both** |
| **TOTAL** | **100** | **Methods (30) + Related Work (20) + Results (20) = 70 points** | Shared accountability |

**Scoring implication:** A simple, defensible set of models + thorough research writing is worth more than rushing an extra neural network. Do not claim a new algorithm if the contribution is an **empirical comparison**.

## 2. Final two-person split

| | **Member A — Data + Linear Methods Lead** | **Member B — Modeling + Evaluation/Report Lead** |
|---|---|---|
| **Primary technical work** | PIE extraction; coordinate choice; 5 Hz preprocessing; video-disjoint splits; window generation; descriptive stats; Constant Position/Velocity; Linear/Ridge regression | Random Forest; training/validation configuration; ADE/FDE verification; 3×3 experiments (with A); model comparison; plots; error analysis |
| **Main paper sections** | Introduction; numbered Research Objectives; Dataset/Data Gathering, Preprocessing, Split and Validity subsections of Methods | Related Work; Model/Experiment/Metrics subsections of Methods; Results and Discussion; IEEE formatting |
| **Co-authored** | Abstract, Conclusions/Future Work, limitations, references, final proofread | Abstract, Conclusions/Future Work, limitations, references, final proofread |
| **Review responsibility** | Verify B's model configurations, figures, results, no test-set tuning | Verify A's data processing, sample counts and split leakage |
| **Handoff** | Clean, tested train/val/test arrays with IDs and a documented data contract **by Nov 22** | Model comparison and experiment CSV/plots **by Nov 27** |
| **Code branch suggestion** | `data/sequence-pipeline`, `model/linear-baselines` | `model/random-forest`, `analysis/metrics-and-figures` |

**Balance check:** A carries most data engineering, the linear methods and the Introduction/Methods writing; B carries nonlinear modeling, evaluation, research literature and Results/layout. **Neither person is solely responsible for writing the final paper.** Both must review the whole manuscript.

### Working model scope

| Tier | Models | Decision |
|---|---|---|
| **Essential** | Constant Velocity (non-ML baseline), **Linear/Ridge Regression**, **Random Forest Regressor** | Fits coursework and gives simple vs linear vs nonlinear comparison |
| **Very low-effort reference** | Constant Position | Include if already implemented; useful to anchor results |
| **Optional only** | MLP neural network, SVR/kernel methods, age/behaviour analysis | Add **only if** all essential experiments and a full paper draft are ready |

## 3. Timeline: five phases, not daily micromanagement

| Phase / time | **A — data + linear** | **B — models + evaluation** | **Joint checkpoint / definition of done** |
|---|---|---|---|
| **P0. Kickoff · Nov 18–19** | Agree on position definition, time-grid and split policy; check data files | Make IEEE paper skeleton; agree on metrics and models | **By Nov 19:** names, official deadline, title, objectives H1/H2, roles, repo ownership and report template decided |
| **P1. Stable data · Nov 20–22** | Build/test parser, videos split, sliding windows; baselines and counts | Write Related Work outline; verify ADE/FDE on toy trajectories; check A's split | **G1 · Nov 22:** runnable 2s→2s input/target and error metrics, reproducible split |
| **P2. Core ML · Nov 23–25** | Implement Ridge; descriptive stats; first Methods + Introduction draft | Train/validate RF; baseline comparison; write Related Work + Models subsection | **G2 · Nov 25:** independent-video comparison for CV/Ridge/RF; paper roughly half drafted |
| **P3. Experiments · Nov 26–27** | Batch-run or independently check the 9 settings; finish Methods and limitations | Generate 3×3 ADE/FDE table, trajectory figures, H1/H2 comparisons; write Results | **G3 · Nov 27:** results frozen (or documented reduced-scope fallback); complete report **first draft** |
| **P4. Report & submit · Nov 28–30** | Check results against CSVs, citations and methods; co-write conclusions | Format in IEEE, refine discussion, captions, abstract and PDF; co-write conclusions | **G4 · Nov 28:** full draft reviewed; **G5 · Nov 29:** final sign-off; **Nov 30:** submission buffer only if permitted |

**Working rhythm:** one **15-minute check-in every 1–2 days**, asynchronously if busy; flag blocked tasks as soon as discovered. Keep intermediate code on separate branches; merge only reviewed, runnable work. A milestone is complete when the output is pushed and independently checked, not when someone says they have started it.

## 4. Master checklist — all tasks, owners, dates, proof of completion

### P0 — Kickoff (Nov 18–19)

- [ ] **[ALL | Nov 19]** Confirm exact Brightspace **due date/time, course ID, page limit if any, file/code requirements and allowed submissions**. Update this header.
- [ ] **[ALL | Nov 19]** Agree on Member A/B names, GitHub permissions, report authors and communication channel.
- [ ] **[ALL | Nov 19]** Confirm RQ/H1/H2 and the image-plane regression target; decide 1/2/3s × 1/2/3s and 5 Hz.
- [ ] **[A | Nov 19]** Define data contract: `video_id`, `ped_id`, `frame`, bbox bottom-centre, `X` and `Y` shapes, units and missing/occlusion policy.
- [ ] **[B | Nov 19]** Create IEEE two-column manuscript skeleton with the instructor's required author header.
- [ ] **[ALL | Nov 19]** Confirm GitHub holds **actual runnable source code**, not only README/workflow; upload existing starter-code folder or create the initial minimal scripts; correct outdated README text that refers to crossing classification.

### P1 — Data + evaluation foundations (Nov 20–22)

- [ ] **[A | Nov 21]** Parse PIE annotations and create continuous pedestrian tracks at 5 Hz; output a descriptive statistics table (videos, pedestrians, track lengths, missing/occluded samples).
- [ ] **[A | Nov 22]** Split by **video** into train/validation/test **before** sliding-window construction; save fixed split IDs and seed.
- [ ] **[A | Nov 22]** Build samples for 1/2/3-second observation and 1/2/3-second prediction; verify that X uses past-only data and Y uses future-only data; report shape tests.
- [ ] **[A | Nov 22]** Run Constant Position and Constant Velocity on one 2s→2s configuration.
- [ ] **[B | Nov 22]** Implement and unit-test Euclidean-distance ADE/FDE; verify 5 Hz and pixel units.
- [ ] **[B | Nov 22]** Collect **relevant peer-reviewed literature** (PIE, trajectory forecasting, baseline methods, forecast-horizon evaluation), maintaining in-text citation candidates and an annotated comparison matrix.
- [ ] **[ALL | Nov 22]** Review splits, counts, hand-calculated metrics, and expected model input/output (**G1**).

### P2 — Linear + nonlinear model comparison (Nov 23–25)

- [ ] **[A | Nov 24]** Train Linear/Ridge Regression on 2s→2s and document features, scaling/relative coordinates and rationale.
- [ ] **[A | Nov 25]** Draft **Introduction + O1/O2/O3 numbered objectives + Data/Preprocessing/Splits + descriptive stats and threats to validity**.
- [ ] **[B | Nov 24]** Train Random Forest; tune only on **validation**, record hyperparameters, data split, seed and training environment.
- [ ] **[B | Nov 25]** Export CV/Ridge/RF comparison on the **same held-out test cohort** with ADE, FDE and number of videos/tracks/windows.
- [ ] **[B | Nov 25]** Draft **Background & Related Work** with comparison/critique and citations; complete Models/Metrics subsections.
- [ ] **[ALL | Nov 25]** Independently rerun the 2s→2s experiment and verify it against saved results (**G2**).

### P3 — Research experiments + first report (Nov 26–27)

- [ ] **[A | Nov 27]** Generate or audit the **9 observation × prediction** combinations and ensure identical dataset policies across models.
- [ ] **[B | Nov 27]** Save a machine-readable results table: model, history, future, ADE, FDE, window count, track count, video count.
- [ ] **[B | Nov 27]** Plot **(1) example trajectory overlays, (2) a 3×3 ADE heatmap, (3) ADE/FDE-vs-horizon chart**, with titles, captions, units and sample sizes.
- [ ] **[A + B | Nov 27]** Test **H1** at fixed 2s future and **H2** at fixed 2s observation using **comparable test anchors**; report unexpected/negative findings honestly.
- [ ] **[A + B | Nov 27]** Inspect successful and failed trajectories and explain plausible causes: turns, occlusion, camera ego-motion, sample dependence and differing horizon difficulty.
- [ ] **[A | Nov 27]** Submit final draft of Methods/Data, objectives and Introduction to common report file.
- [ ] **[B | Nov 27]** Submit Related Work, Methods/Models, Results/Discussion, full source citations and figures; assemble **complete v1 report (G3)**.

### P4 — IEEE paper quality + submission (Nov 28–30)

- [ ] **[A | Nov 28]** Check technical reproducibility, split integrity, descriptions and consistency of paper numbers with code output.
- [ ] **[B | Nov 28]** Finish IEEE formatting, numbered figure/table captions, author block and references; check first-page requirements.
- [ ] **[A + B | Nov 28]** Co-write Conclusion, Future Work and **Lessons Learnt**; explicitly answer each numbered objective and hypothesis.
- [ ] **[A + B | Nov 28]** Write Abstract **last**, containing actual empirical results, contributions and limitations.
- [ ] **[A + B | Nov 29]** Cross-review every section against the rubric; verify literature analysis, gap, method rationale, descriptive stats, threats to validity, and results interpretation.
- [ ] **[A + B | Nov 29]** Visually inspect the compiled PDF; test source links/refs, figure readability, no placeholders, correct names/departments/emails and allowed page count. Approve final version (**G5**).
- [ ] **[A + B | Official deadline]** Submit the required files; re-open the submission, confirm uploaded versions and retain a backup. **Do not assume Nov 30 is valid without checking LMS.**

### Optional — only once full v1 is ready

- [ ] **[B]** Add MLP neural network as an additional model, or a kernel regressor if efficient.
- [ ] **[A + B]** Behaviour/age subgroup analysis, provided metadata and independent sample counts are sufficient.
- [ ] **[A + B]** Video/track-clustered uncertainty intervals or repeated model seeds.

## 5. Objectives / empirical-study details to include in Methods (30 pts)

| Objective ID | Research objective | Practical / research significance | Mandatory evidence |
|---|---|---|---|
| **O1** | Examine effect of **1/2/3-second observation length** on trajectory-prediction error | How much recent movement history is informative? | H1 controlled comparison, future horizon **fixed at 2s** |
| **O2** | Examine effect of **1/2/3-second prediction horizon** on error | How far into the future can positions be forecast reliably? | H2 comparison, observation **fixed at 2s** |
| **O3** | Compare **Constant Velocity, Ridge and Random Forest** on identical data | Does linear/nonlinear ML improve on a physically interpretable baseline? | Test ADE/FDE for models; hyperparameters chosen on validation only |

**Empirical-study section must explicitly report:** research questions/hypotheses; annotation source and how data are extracted; descriptive stats (video/track/window counts); sampling rate; inclusion/exclusion criteria; time-window definitions; features and target; split/manifests; equations for ADE/FDE; comparison controls; training and selection settings; analysis techniques; dataset limitations and **Threats to Validity**. These are all explicitly rewarded under the marking scheme.

**Caution:** Overlapping windows are not independent; use a **video-disjoint** split and consider equal-weight-per-track summaries. Compare H1/H2 on the **same eligible testing subset** where possible. Image-plane displacement in a moving-car dataset is **not real-world movement in metres**.

## 6. Report sections + concrete output tracker

| Deliverable | Primary owner | Reviewer | Target | File/acceptance criterion |
|---|---|---|---|---|
| Dataset summary and train/val/test manifest | A | B | Nov 22 | `results/dataset_summary.csv`, split IDs, exclusions explained |
| Data parser / trajectory sequence code | A | B | Nov 22 | Re-run 2s→2s from annotations without manual edits |
| ADE/FDE test and model interfaces | B | A | Nov 22 | Toy-example tests and stable feature/target shape |
| Constant Velocity and Ridge | A | B | Nov 24 | Works on shared split; reproducible configuration |
| Random Forest + model comparison | B | A | Nov 25 | CSV with ADE/FDE/counts for fixed 2s→2s |
| **Related Work** bibliography + paragraphs | B | A | Nov 25 | Literature **analysis**, not just a list of papers |
| **Methods + numbered objectives** | A for data; B for models | Each other | Nov 27 | Addresses all empirical-study prompts and citations |
| 9 time-window results | B computes, A audits | Each other | Nov 27 | `results/window_matrix.csv` + config/seed; compare common test anchors |
| Figures and Results discussion | B | A | Nov 27 | 3 core plots with descriptive captions, causal caution |
| Full first report draft | B integrates; A writes assigned parts | Each other | Nov 27 | All required headings, tables and references present |
| Final PDF + reproducible GitHub commit | Both | Both | Nov 29 (provisional) | IEEE, review checklist signed off, ready to submit |

## 7. Risk plan: protect the 100-point rubric

| Trigger / date | What to cut | What must **never** be cut |
|---|---|---|
| **No stable 2s→2s run by Nov 22** | Pause all optional models and analyses; pair-debug data contract | Valid split, working ADE/FDE, reproducibility |
| **Random Forest unstable by Nov 25** | Keep Constant Velocity + Ridge as fallback; continue RF only if it can be made reliable | Research objectives, comparison protocol, Methods and citations |
| **9 runs incomplete by Nov 27** | Use a **5-condition minimum**: H1 = (1,2,3)s history at 2s horizon; H2 = 2s history at (1,2,3)s horizon, with 2s→2s overlap; disclose reduced scope | At least one controlled comparison for each core RQ, sample counts, honest limitations |
| **Manuscript still incomplete Nov 28** | Stop tuning and all extensions. Prioritize Methods (30), Related Work (20), Results (20), then Format (10), Intro (10), Abstract/Conclusion (10) | Complete evidence-based report and official submission |

**No evidence of required number of models, number of plots, or exact page limit appears in the supplied rubric.** The 9-condition plan, three model families and example-report length are **team design choices**, not claimed marking rules. Any fixed course-specific requirements in Brightspace override this plan.

## 8. GitHub protocol and next meeting (two people)

| Practice | Rule |
|---|---|
| Work allocation | Create separate Issues for A and B; paste task IDs and due dates from the checklist |
| Branches | A uses `data/*` or `model/linear-*`; B uses `model/rf-*`, `analysis/*`, `paper/*` |
| Review | Each PR must be reviewed by the **other person**; no unreviewed changes to experiment settings |
| Recording experiments | Save config/seed/split IDs, commands, counts, metrics and generated CSV/figures |
| Repository hygiene | **Do not commit raw PIE ZIP, videos, or large checkpoints.** Update README to describe trajectory regression accurately |
| Meeting | 25–30 min: confirm official deadline (5), choose A/B & working hours (5), freeze H1/H2/models (10), assign first Issues and report editor (10) |

### Final rubric sign-off checklist

- [ ] **10/10 Format:** IEEE conference template, centered author/title information, no PDF/layout errors.
- [ ] **5/5 Abstract:** Full narrative including real results, contributions, future work, lessons.
- [ ] **10/10 Intro:** Problem, gap, objectives, evidence, contributions, impact and roadmap.
- [ ] **20/20 Related Work:** Relevant literature **analyzed** with citations and clearly justified gap.
- [ ] **5/5 Objectives:** O1/O2/O3 IDs and significance stated.
- [ ] **25/25 Methodology:** Data source, descriptive stats, decisions and rationale, study design, analysis, validity threats, references.
- [ ] **20/20 Results:** Relevant test results/plots/tables, validation, detailed interpretation and novelty of the *study*.
- [ ] **5/5 Conclusion:** Answers hypotheses and objectives; lessons, limitations and future work.
- [ ] **Final safety:** Correct submission files and time confirmed; both students approve.

> This is a **planning checklist**, not a promise of marks: the grader decides the actual score. Checkboxes under rubric headings indicate completeness of coverage, not grades awarded.