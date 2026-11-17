# Team Workflow & Delivery Plan

**Project:** Pedestrian Trajectory Prediction Using Machine Learning  
**Course:** Machine Learning Group Project (Western University)  
**Team:** 3 members — replace Member A/B/C with names and GitHub usernames at kickoff  
**Working period:** November 17–29, 2026  
**Provisional submission buffer:** November 30, 2026 — **confirm the actual LMS deadline and time immediately**  
**Repository:** [AUR0RACY/PedestrianPrediction](https://github.com/AUR0RACY/PedestrianPrediction)  
**Reference:** Instructor-provided full-mark report, *Unsupervised Domain Adaptation for Image Classification* (8-page IEEE-style paper). We follow its **organization and depth**, not its classification methods or results.

> **Definition of the project:** This is a **regression / trajectory forecasting** project. From 1, 2, or 3 seconds of observed pedestrian bounding-box coordinates in the PIE dataset, predict 1, 2, or 3 seconds of future 2D **image-plane** positions. The target is **not** “cross / not cross.” ADE and FDE are measured in pixels unless otherwise stated. Camera motion is a stated limitation.

## 1. Research scope — decisions locked at kickoff

| Decision | Working specification | Must be frozen by |
|---|---|---|
| Dataset | PIE official `annotations.zip`; no video download needed for coordinate baselines | Nov 17 |
| Sampling | 5 Hz, sampled from 30 FPS; preserve continuous frame sequence | Nov 18 |
| Position representation | Bottom-centre of each pedestrian box: `((x1+x2)/2, y2)`; use consistent 1920×1080 coordinates | Nov 18 |
| Historical observation | **1 s, 2 s, 3 s** | Nov 17 |
| Future horizon | **1 s, 2 s, 3 s** | Nov 17 |
| Core algorithms | Constant Position, Constant Velocity, Random Forest Regressor | Nov 19 |
| Optional algorithm | MLP only after core results are frozen; GRU is stretch, not a requirement | Nov 24 |
| Primary metrics | ADE and FDE; report window-weighted and track-level macro results | Nov 20 |
| Validation approach | Video-disjoint train/validation/test; split before constructing windows; use shared eligible test anchors for comparison | Nov 19 |
| Optional groups | Behaviour/turning from **observed frames only**; age only if separate attributes ZIP and adequate independent sample counts | Nov 23 |
| Reproducibility | One canonical config, fixed seed, saved split IDs, CSV summary tables, scripts and figure outputs | Nov 25 |

### Research questions and hypotheses

- **H1 — Observation duration:** With the **future horizon fixed at 2 seconds**, does the most recent 1 second of observation produce lower forecasting error than 2 seconds, and 2 seconds lower than 3 seconds? This is the team's **directional hypothesis to test**, **not** a guaranteed outcome. The Constant Velocity baseline may not change if velocity is always estimated from the same latest portion of history.
- **H2 — Prediction horizon:** With **observation fixed at 2 seconds**, do ADE/FDE generally increase as the future horizon changes from 1 to 2 to 3 seconds?
- **H3 — Optional exploratory analysis:** Is error different between behaviour/turning categories or age groups? Only run if group labels and independent sample sizes are adequate. Keep H1/H2 and model comparison as the mandatory core.

### Minimum viable submission (MVS)

A reproducible, leakage-free pipeline using PIE annotations; 3 baselines/models (Constant Position, Constant Velocity, Random Forest); all **9 observation × prediction** combinations; a results CSV with ADE/FDE and sample counts; at least **4 well-captioned figures**, at least **2 tables**, an IEEE-style ~8-page report with citations and limitations, and a final repository commit. No unsupported performance or causality claims.

## 2. Responsibilities and handoffs

**Member A — Data Lead / Dataset section (35% of technical workload)**  
Owns extraction, sequence construction, group splits, sampling/occlusion checks, data summary tables, and the report's Dataset + Preprocessing subsections. Deliverables: `src/.../data.py`, `src/.../split.py`, `results/tables/dataset_summary.csv`, `results/figures/data_examples.png`, methods text + reproducible commands. **Handoff:** A shares the stable input/target array contract and video splits with B and C by **Nov 19, 18:00 ET**.

**Member B — Modeling Lead / Models section (35% of technical workload)**  
Owns Constant Position, Constant Velocity, Random Forest, validation-only hyperparameter selection, model comparison, training commands and environment specification. Deliverables: `src/.../models.py`, `src/.../train.py` (or existing CLI), `results/tables/model_comparison.csv`, `results/logs/experiment_config.json`, and the Model/Implementation subsections. **Handoff:** B shares reproducible per-window predictions + run manifests with C by **Nov 23, 18:00 ET**.

**Member C — Evaluation & Paper Lead / Results section (30% of technical workload)**  
Owns ADE/FDE verification, nine-cell experiment table, trajectory overlays, error curves, uncertainty/failure cases, source bibliography and formatting the integrated IEEE report. Deliverables: `src/.../evaluation.py`, `results/tables/window_matrix.csv`, `results/figures/*.png`, `paper/main.tex` (or agreed equivalent), Results + Discussion drafts. **Handoff:** C returns a unified results figure/table pack by **Nov 25, 18:00 ET**.

These ownership percentages are workload-balancing targets, **not grading weights**. All three review major results and co-author the Introduction, research interpretation, Abstract, Conclusion, and references. A and B must review C's interpretation; C must review A's leakage controls and B's model evaluation.

| Shared decision / integration task | Accountable owner | Reviewer(s) | Sign-off date |
|---|---|---|---|
| Freeze research question, metrics and sample definition | A | B + C | Nov 17 |
| Validate video split and sample eligibility | A | C | Nov 19 |
| Baselines + reproducible ML training | B | A | Nov 22 |
| Results integrity, comparison fairness and visuals | C | A + B | Nov 25 |
| Full report, citations, figure labels | C | A + B | Nov 27 |
| Final code/report freeze and submission | **All three** | All three | Nov 29 |

## 3. Daily timeline (Eastern Time)

> **Deadline caution:** User specified “end of November,” not an exact date or time. Planning assumes **Nov 29 final freeze** and **Nov 30 contingency only**. If LMS says Nov 27/28, move the internal freeze forward and skip optional analyses.

| Date | Member A — Data | Member B — Models | Member C — Evaluation/report | End-of-day checkpoint |
|---|---|---|---|---|
| **Tue Nov 17** | Define coordinate convention and data dictionary | Confirm baseline/model specs | Create IEEE report outline and citation list | Kickoff; assign names/branches; lock H1/H2 |
| **Wed Nov 18** | Verify PIE parser, 5 Hz timing, occlusion policy | Build baseline unit tests; specify feature interface | Build ADE/FDE verification tests and example trajectory figure | Data contract + experiment config draft |
| **Thu Nov 19** | Freeze video-level split and anchor eligibility; publish counts | Run Constant Position/Velocity on one 2s→2s split | Independently verify split/leakage and metrics | **Gate 1: data pipeline approved** |
| **Fri Nov 20** | Create dataset summary and quality chart | Train first Random Forest 2s→2s, log seed/params | Draft Introduction + Related Work with citations | First RF vs baseline comparison |
| **Sat Nov 21** | Audit short/occluded tracks; share fixed split IDs | Tune RF using validation only; freeze choice | Draft Methods skeleton and paper figures template | **Gate 2: main model reproducible** |
| **Sun Nov 22** | Assist model debugging; resolve data edge cases | Finish 1/2/3 observation runs at fixed 2s horizon | Check H1 table/plot and common-anchor fairness | H1 results + model code PR |
| **Mon Nov 23** | Verify shared test cohort and sample counts | Finish 1/2/3 horizon runs at fixed 2s history | Create H2 table/plot + trajectory overlays | **Gate 3: all 9 conditions runnable** |
| **Tue Nov 24** | Draft Dataset/Preprocessing text + Table 1 | Model comparison + optional simple feature ablation | Consolidate 9-cell heatmap and first Results draft | CSV exports + preliminary figures |
| **Wed Nov 25** | Deliver methods section and dataset figure | Deliver Models section, hyperparameter table | Integrate all experimental tables/figures and limitation notes | **Gate 4: result tables/figures frozen** |
| **Thu Nov 26** | Review C's result interpretation | Review C's results and complete implementation notes | Draft 1st full IEEE-style paper, Conclusions + Abstract | Complete **v1 report** for team review |
| **Fri Nov 27** | Technical fact-check + citation check | Verify rerun from clean environment | Revise narrative, captions, references and formatting | **Gate 5: full team-reviewed v2** |
| **Sat Nov 28** | Test replication and verify no leaked raw data | Final reproduce/seed check and compare CSVs | Polish visuals, two-column layout and proofreading | **Submission candidate** |
| **Sun Nov 29** | Final sign-off | Final sign-off | Final PDF export and checks | **Freeze: final PDF + commit + backup** |
| **Mon Nov 30** | Submission confirmation | Submission confirmation | Upload/verify only if official due date permits | **Buffer, not planned research day** |

**Daily check-in:** ~10 minutes at 20:00 ET (or asynchronous GitHub thread): what shipped, what is blocked, next deliverable. Report blockers by **noon the following day**. High-priority integration questions are resolved in a group call rather than left in an unreviewed PR.

## 4. Experiment protocol and who runs what

1. **Single source of truth (A):** Dataset = official PIE annotation ZIP; 30 FPS source, sampled to 5 Hz. Extract bounding-box bottom-centre; maintain `video_id`, `pedestrian_id`, frame and occlusion. Avoid using future behavioural information in model inputs.
2. **No-leak split (A, reviewed by C):** Split **videos** into train/validation/test before overlapping windows are generated. Store deterministic split manifests and report counts of **windows, tracks and videos** per subset. Match common test anchors for fair comparisons.
3. **Baselines + ML (B):** Constant Position, Constant Velocity and Random Forest with past positions/velocity features only. Keep the same training budget and validation procedure wherever practical; never choose hyperparameters from test metrics.
4. **Nine conditions (B executes; C audits):** Observation = {1s, 2s, 3s}; future horizon = {1s, 2s, 3s}, at 5 Hz. For observation duration D seconds, include `5D+1` observed points including current `t=0`; future horizon H has `5H` target points after `t=0`.
5. **Measure (C):** For each condition/model, report **ADE** and **FDE** in **pixels**, sample/track/video counts, and optionally video-cluster bootstrap intervals. Also report per-track macro ADE/FDE to prevent long tracks dominating the conclusion.
6. **Evaluate hypotheses (C + all):** H1: compare RF over observation lengths at **fixed 2s prediction**. H2: compare error over 1/2/3s prediction at **fixed 2s observation** (FDE is especially interpretable). Clearly report if H1 is **rejected**, not just if supported.
7. **Stress-test (all):** Inspect at least 3 successes and 3 failures; discuss turning, standing, occlusion, camera motion and overlapping-window dependence. Optional subgroup analysis only after the core nine-cell experiment is complete.
8. **Reproduce (A + B, reviewed by C):** A different member runs the exact commands on a clean clone. CSV/figures must be regenerated without editing the source code. Raw PIE ZIP and large model binaries are excluded from commits.

### Required result artifacts

| Artifact | Purpose | Owner | Due |
|---|---|---|---|
| `results/tables/dataset_summary.csv` | Tracks, videos, duration, occlusion, counts by split | A | Nov 20 |
| `results/tables/model_comparison.csv` | Constant Position vs Velocity vs RF, held-out ADE/FDE | B | Nov 24 |
| `results/tables/window_matrix.csv` | All 9 combinations; per-model ADE/FDE/counts | C | Nov 25 |
| `results/figures/experiment_pipeline.png` | Reproducible workflow drawing | A + C | Nov 24 |
| `results/figures/trajectory_examples.png` | History / ground truth / prediction overlay | C | Nov 24 |
| `results/figures/ade_heatmap.png` | Observation × horizon result matrix | C | Nov 25 |
| `results/figures/horizon_error.png` | ADE/FDE vs future length, with model legends | C | Nov 25 |
| `results/figures/error_cases.png` | Failure cases and explanation | B + C | Nov 26 |
| `paper/main.tex` / `final_report.pdf` | Submitted IEEE-style research report | C editor; all authors | Nov 29 |

## 5. Final report blueprint — modeled on instructor's full-mark example

Target **approximately 8 pages**, two-column IEEE layout, subject to actual course instructions. The sample report uses Abstract, Introduction, Background and Related Work, Methods (objectives, pipeline, models, data, analysis, threats to validity/rationale), Results with figures/tables, Conclusion/Future Work, and References. It is an **example, not proof of formal rubric or page limit**. *Our methods and visualizations must fit trajectory regression rather than the sample's image classification.*

| Suggested section | What we must write | Lead | Target space |
|---|---|---|---|
| **Title, authors, Abstract, Index Terms** | 150–200-word summary written **last**; task, PIE data, 3 models, 9 conditions, core ADE/FDE findings; no unverified claims | All (C integrates) | ~0.4 page |
| **I. Introduction** | Autonomous-driving motivation; difference between **crossing intention** and **2D trajectory regression**; H1/H2 and contribution list | All (C first draft) | ~0.7 page |
| **II. Background & Related Work** | PIE dataset paper, trajectory baselines, prediction horizon / model comparison work; identify narrow study gap and justify baselines | C + B | ~1.0 page |
| **III.A Research Objectives** | Operational definitions of observation window, horizon, target; directional H1 and H2; optional exploratory H3 | All | ~0.4 page |
| **III.B Learning Pipeline / Data** | Dataset stats, 5 Hz extraction, bottom-centre, normalization if used, quality filtering, video split, sample count, leakage prevention | A | ~0.8 page |
| **III.C Models / Experimental Setup** | Constant Position, Constant Velocity, RF features, train/validation/test, hyperparameters, seeds, 3×3 grid | B | ~1.0 page |
| **III.D Metrics & Threats to Validity** | ADE/FDE equations and units, track-macro statistics, uncertainty, ego-motion, overlapping windows, occlusion | C + A | ~0.5 page |
| **IV. Results & Discussion** | Dataset table; model comparison table; 3×3 grid; figure overlays and curves; H1/H2 test; failure analysis and honest limitations | C (A/B verify) | ~2.1 pages |
| **V. Conclusion & Future Work** | Direct answer to hypotheses, strengths/limitations; optional ego-motion compensation / behaviour analysis | All | ~0.5 page |
| **References** | Cite PIE and relevant prior methods using one consistent IEEE-style numbered bibliography | C (all verify) | ~0.6 page |

**Paper-ready visuals (minimum 4):** (1) PIE preprocessing/model pipeline, (2) example observed/actual/predicted trajectories, (3) ADE heatmap for the nine time settings, (4) ADE/FDE vs forecast horizon or observation length; add failure-case montage if space permits. **Required tables (minimum 2):** data/split statistics and held-out model scores, plus strongly preferred 3×3 experiment table. Every figure/table needs a number, caption, axis labels, units and in-text discussion.

**Do not copy the sample's t-SNE class separation or confusion matrices**: those serve its **classification** task. Replace them with regression-suitable trajectory plots and displacement-error comparisons. Sample references/model diagrams are organizational inspiration only; cite sources used and produce original analyses.

## 6. GitHub teamwork rules

- **Branches:** `data/<topic>` (A), `model/<topic>` (B), `analysis/<topic>` or `paper/<topic>` (C); merge to `main` via pull request. GitHub repository currently contains only a basic README, so **first merge the prepared starter-code ZIP** if it has not yet been uploaded.
- **Issue board:** One task per Issue, with owner, deadline, deliverable path and acceptance criterion. Start with: `Data parser & split`, `Constant Velocity baseline`, `Random Forest tuning`, `ADE/FDE tests`, `9-window experiments`, `Figures & error analysis`, `Literature and methods draft`, `IEEE report integration`, `Final reproduce & submission`.
- **Review:** At least one **other team member** reviews each PR. The reviewer runs any relevant test (e.g. `python -m pytest -q`) and checks shapes/units/seed/no leakage. Avoid changing `main` just before final freeze.
- **Commit standard:** `data: validate 5 Hz windows`, `model: add random forest baseline`, `analysis: plot nine-cell ADE heatmap`, `paper: revise related work`.
- **Data policy:** Do not commit raw PIE annotations/videos, secrets, trained model artifacts, or huge result dumps. Commit source code, configs, concise CSV summaries, figures, README, and final PDF only if submission rules permit.
- **Definition of done:** Every Issue ends with a linked PR, reproducible run command, tested output file, a short README/methodology update if needed, and reviewer approval.

## 7. Quality gates, risk plan and last-day checklist

| Gate | Date | Pass condition | If blocked |
|---|---|---|---|
| **G1 Data ready** | Nov 19 | Same split IDs; continuous sequences; train/test videos disjoint; 1/2/3s feasible | Use stricter subset; document exclusions |
| **G2 Models ready** | Nov 21 | CV and RF reproducible on independent holdout; ADE/FDE verified | Drop MLP immediately; finish baselines + RF |
| **G3 Experiments ready** | Nov 23 | Nine combinations executed; consistent evaluation cohort; output CSV | Run RF + CV only, no subgroup research |
| **G4 Results frozen** | Nov 25 | Final plots/tables generated; all captions, units, sample counts | Use fewer figures, not unverified claims |
| **G5 Report reviewed** | Nov 27 | Complete numbered sections; no placeholders; citations and evidence match | Trim optional H3 and late tuning |
| **G6 Final** | Nov 29 | Clean rerun + PDF visual inspection + three sign-offs + backup | Revert to stable main and submit verified core |

**Technical risks:** PIE footage is captured by a moving camera (2D image motion ≠ world motion); overlapping windows inflate naive independence; group sizes may differ; H1 may not be supported; strict occlusion criteria can reduce samples; an extra deep model may consume time without improving scientific claims. The response is to explicitly document limitations, compare like-with-like and prioritize controlled experiments over model count.

**Submission checklist:** [ ] official deadline checked; [ ] team names and author order confirmed; [ ] methods exactly match code; [ ] test set never used in tuning; [ ] ADE/FDE units correct; [ ] all nine runs + seeds stored; [ ] tables/figures referenced in text; [ ] H1/H2 answered, whether supported or not; [ ] full citations; [ ] generated PDF checked for clipping; [ ] raw PIE data absent from Git; [ ] fresh clone and rerun documented; [ ] all three approve; [ ] uploaded file re-opened to verify.

---

**Suggested first team meeting agenda (Nov 17–18, 30 minutes):** confirm names/handles and submission timestamp (5 min); agree on 2D target, 5 Hz, video split and metrics (10 min); assign A/B/C ownership and milestones (10 min); create the first three Issues and agree on daily check-ins (5 min).