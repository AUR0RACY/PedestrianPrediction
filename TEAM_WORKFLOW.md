# Team Workflow — Pedestrian Trajectory Prediction

> **Quick view:** Three-person ML project · PIE dataset · trajectory **regression** (not crossing classification) · 1/2/3-second history × 1/2/3-second future · ADE/FDE · November 2026.

**Repository:** [AUR0RACY/PedestrianPrediction](https://github.com/AUR0RACY/PedestrianPrediction)  
**Team kickoff:** **Wednesday, November 18, 2026**  
**Official deadline:** **End of November — confirm the exact date, time and time zone in Brightspace on Nov 18.**  
**Proposed team target:** complete the reviewed report **Nov 29**; use **Nov 30 only if the official deadline permits** for final export, upload and verification.  
**Scope:** Keep the research strong but manageable. A reproducible experiment matters more than adding another complex model.

## 1. Dashboard — what matters first

| Priority | Scope | Status | Why |
|---|---|---|---|
| **P0 — must finish** | Reliable data pipeline, leakage-free video split, Constant Position, Constant Velocity, Random Forest, 9 time-window settings, ADE/FDE, IEEE-style report | ☐ To do | Complete answers to H1/H2 |
| **P1 — polish** | Dataset statistics, trajectory plots, heatmap, model-comparison table, error analysis, references, clean reproduction | ☐ To do | Similar depth and presentation to the example report |
| **P2 — only if ahead** | MLP/GRU, age or behaviour subgroup analysis, advanced uncertainty testing | ☐ Optional | Do **not** delay P0 or submission |

**Sample-paper alignment:** The instructor's full-mark example is an 8-page, IEEE-style research paper with Abstract, Introduction, Background/Related Work, Methods, Results, Conclusions/Future Work and References. It includes a clear hypothesis, multiple model comparisons, explanatory figures/tables, and limitations. **It is a style/depth reference, not a verified rubric or page limit.** Use **trajectory plots, ADE/FDE and time-window comparisons**, not the sample's classification confusion matrices/t-SNE results.

### Research questions

| ID | What changes? | What stays fixed? | Question / hypothesis |
|---|---|---|---|
| **H1** | Observation length: **1, 2, 3 s** | Predict **2 s** ahead | Is shorter, more recent history more useful? Team's proposed directional hypothesis: **1 s error < 2 s error < 3 s error**; test it without assuming it is true. |
| **H2** | Forecast horizon: **1, 2, 3 s** | Observe **2 s** | Does forecasting farther into the future increase ADE/FDE? |
| **Model comparison** | Constant Position vs Constant Velocity vs RF | Same video split and eligible evaluation cohort | Does learned regression outperform simple baselines? |
| **H3, optional** | Motion/behaviour or age groups | Fixed model/window | Is error different across groups? Only if group labels and independent sample sizes are sufficient. |

**Technical protocol:** PIE bounding-box **bottom-centre** image coordinates; 30 FPS source sampled to **5 Hz**. For a D-second observation, use `5D+1` points (including the current time); predict the next `5H` points over an H-second horizon. Report errors in **pixels**. **Split by video before creating overlapping windows**; reuse comparable test anchors for fair comparisons; never tune on the test set. Moving-camera/ego-motion is a documented limitation.

## 2. Responsibilities — decide names at the Nov 18 meeting

| Role | Main owner | Primary code/results | Writing responsibility | Cross-review |
|---|---|---|---|---|
| **A — Data lead** | **Member A: TBD** | PIE parsing, cleaning, 5 Hz sampling, coordinates, occlusion policy, video split, sequence-builder, dataset counts | **Dataset & Preprocessing** in Methods | Reviews B's training-data interface and C's sample counts |
| **B — Modeling lead** | **Member B: TBD** | Constant Position, Constant Velocity, RF, validation tuning, reproducible runs, experiment log | **Models & Implementation** in Methods | Reviews A's sequence shapes and C's model interpretation |
| **C — Evaluation/paper lead** | **Member C: TBD** | ADE/FDE tests, nine-cell comparison, trajectory visualizations, graphs/tables, paper integration | **Results, Discussion, paper layout** | Reviews A's leakage controls and B's metrics |
| **Everyone** | A + B + C | Decide H1/H2; sanity-check at least one shared result; review PRs; rerun code | **Introduction, Related Work, Abstract, Conclusion, citations** | Final scientific and submission sign-off |

**Important:** C coordinates the document but **is not expected to write the whole paper**. A and B must submit complete Methods subsections and help interpret results. Assign names and GitHub handles on Nov 18; all work is visible through Issues/PRs.

## 3. Relaxed timeline — 5 phases, with parallel work

| Phase | Dates (2026) | A — Data | B — Models | C — Evaluation/report | Milestone / checkpoint |
|---|---|---|---|---|---|
| **0. Kickoff** | **Nov 18 (Wed)** | Confirm dataset convention, sample format | Agree on models/features | Create report outline and shared bibliography | **Research scope + owners agreed** |
| **1. Build foundations** | **Nov 19–22 (Thu–Sun)** | Parser, track cleaning, deterministic video split, sequence arrays | Baseline implementation; small working example | ADE/FDE tests, literature, IEEE template | **G1 · Nov 22:** data + baseline work on 2s→2s |
| **2. Establish models** | **Nov 23–25 (Mon–Wed)** | Audit counts/splits; finish dataset section | RF train/validation, compare with baselines, commit runs | Make pipeline figure; draft related work + methods skeleton | **G2 · Nov 25:** reproducible RF and 2s→2s comparison |
| **3. Run research experiments** | **Nov 26–27 (Thu–Fri)** | Verify consistent cohort and data counts | Run **all 9** observation/horizon settings; save model outputs | Evaluate H1/H2, make results tables and main figures | **G3 · Nov 27:** complete experiment matrix and draft Results |
| **4. Write, review, submit** | **Nov 28–30 (Sat–Mon)** | Finalize Dataset, perform code/data audit | Finalize Models, run clean reproduction | Integrate IEEE paper, captions, limitations, final PDF | **G4 · Nov 29:** team-reviewed report; **Nov 30:** submit only if before official LMS deadline |

**Less-pressure rule:** These are *team targets*, not daily high-stakes deadlines. People can work within their phase. The critical handoffs are **Nov 22 → Nov 25 → Nov 27 → Nov 29**. Start report writing **in parallel** with coding rather than after all experiments finish. If the official LMS deadline is before Nov 30, immediately move the final checkpoint earlier.

## 4. Master task checklist — check off as work is merged

Copy each checkbox into a GitHub Issue if a more interactive task board is preferred. Each item has **one owner, one target date and a tangible deliverable**.

### Phase 0 — Kickoff · Nov 18

- [ ] **ALL · Nov 18:** Confirm the **actual Brightspace deadline, time zone, submission type, page limit and grading requirements**; post them at the top of this file.
- [ ] **ALL · Nov 18:** Assign Member A/B/C names and GitHub usernames; agree on when/how to communicate.
- [ ] **ALL · Nov 18:** Confirm H1 (1/2/3 s observation), H2 (1/2/3 s future), 5 Hz, image-plane positions, ADE/FDE, P0 model list.
- [ ] **C · Nov 18:** Create the IEEE-style report outline and shared reference list.
- [ ] **ALL · Nov 18:** Create GitHub Issues for the work below; each person takes their own issues.

### Phase 1 — Foundations · Nov 19–22

- [ ] **A · Nov 21:** Extract `video_id`, `ped_id`, frame, bbox, occlusion and bottom-centre `(x,y)` from PIE.
- [ ] **A · Nov 22:** Implement continuous 5 Hz tracks and sliding input/target windows; verify shapes for 1/2/3 seconds.
- [ ] **A · Nov 22:** Store deterministic video-disjoint train/validation/test split IDs; report number of videos/tracks/windows and exclusions.
- [ ] **B · Nov 22:** Implement Constant Position and Constant Velocity; produce one 2s→2s comparison.
- [ ] **C · Nov 22:** Unit-test ADE/FDE using manually checkable toy trajectories and confirm pixel units.
- [ ] **C + B · Nov 22:** Gather cited related-work sources (PIE dataset, trajectory forecasting baselines, evaluation metrics).
- [ ] **ALL · Nov 22:** Review split/leakage prevention and confirm data-to-model interface (**G1**).

### Phase 2 — Models · Nov 23–25

- [ ] **B · Nov 24:** Train Random Forest on the 2s→2s setting; fix seed, features, preprocessing and validation protocol.
- [ ] **B · Nov 25:** Tune only on validation set; save configuration and baseline-vs-RF table with ADE/FDE and sample counts.
- [ ] **A · Nov 25:** Finalize dataset quality summary and write Dataset/Preprocessing subsection.
- [ ] **C · Nov 25:** Draw the data→sequence→models→metrics pipeline diagram and write the Related Work draft.
- [ ] **B · Nov 25:** Write Models/Implementation subsection, including selected hyperparameters and training environment.
- [ ] **ALL · Nov 25:** Peer review model, split and metric code; confirm a reproducible 2s→2s run (**G2**).

### Phase 3 — Experiments · Nov 26–27

- [ ] **B · Nov 27:** Run the complete **3×3 observation × forecast** matrix for required models, using the same split and documented settings.
- [ ] **C · Nov 27:** Produce the master table with **ADE, FDE, windows, tracks and videos** for each condition/model.
- [ ] **C · Nov 27:** Test **H1** at a fixed 2s horizon and **H2** at a fixed 2s observation window; state whether supported.
- [ ] **A + C · Nov 27:** Verify that comparisons use the same eligible test anchors (or explicitly explain differences).
- [ ] **C · Nov 27:** Export plots: trajectory overlays, 3×3 ADE heatmap, horizon/observation error curves.
- [ ] **A + B + C · Nov 27:** Review at least three clear successes and three failures; discuss camera movement, turning, occlusion and track overlap.
- [ ] **ALL · Nov 27:** Agree on final numbers and stop tuning on the test set (**G3**).

### Phase 4 — Report and submission · Nov 28–30

- [ ] **A · Nov 28:** Submit final Dataset/Preprocessing paragraphs, data table and caveats to the shared paper.
- [ ] **B · Nov 28:** Submit final Models/Experimental Setup paragraphs, model comparison and implementation details.
- [ ] **C · Nov 28:** Integrate Results/Discussion with correctly captioned figures, tables, axes and units.
- [ ] **ALL · Nov 28:** Jointly draft/review Introduction and Conclusion; draft Abstract **after** final results are known.
- [ ] **C · Nov 29:** Produce the complete two-column PDF with IEEE numbered references and original figures.
- [ ] **A + B · Nov 29:** Independently verify methods match code and numbers match CSVs; check all claims/citations.
- [ ] **ALL · Nov 29:** Review final PDF visually (missing figures, overlaps, page count, names, units) and approve (**G4**).
- [ ] **ALL · Nov 30 / earlier official deadline:** Submit the required PDF/code package; re-open uploaded files to confirm they are correct.

### Optional — ONLY after P0 is complete

- [ ] **B · stretch:** Add an MLP model and comparison if there is time.
- [ ] **A + C · stretch:** Analyse standing/walking/turning groups using only information available at observation time.
- [ ] **A + C · stretch:** Analyse age groups **only if** `annotations_attributes.zip` is available and the categories have sufficient independent trajectories.
- [ ] **C · stretch:** Add uncertainty intervals or repeated seeds if the core pipeline and report are already stable.

## 5. Deliverables and file ownership

| Due | Owner | Required output (names are suggested; use actual paths if different) | Acceptance criterion |
|---|---|---|---|
| Nov 22 | A | `src/.../data.py`, `src/.../split.py`, split manifest | Continuous sequences; videos do not overlap across splits |
| Nov 22 | B | `src/.../models.py` baselines | 2s→2s runs; outputs correct shapes |
| Nov 22 | C | `src/.../evaluation.py` tests | ADE/FDE match hand-calculated cases |
| Nov 25 | A | `results/tables/dataset_summary.csv`, Methods draft | Counts of videos/tracks/windows and quality exclusions |
| Nov 25 | B | `results/tables/model_comparison.csv`, model config, Methods draft | Reproducible RF vs baselines |
| Nov 25 | C | `results/figures/pipeline.png`, paper template, references | Figure clear, sources cited |
| Nov 27 | B | Run script/configs + per-setting model outputs | 9 combinations recorded and reproducible |
| Nov 27 | C | `results/tables/window_matrix.csv`, core plots | ADE/FDE + counts, captions and units |
| Nov 28 | A + B + C | Complete sections in shared paper | No blank Methods/Results placeholders |
| Nov 29 | C coordinating; all approving | `paper/main.tex` or agreed equivalent + final PDF candidate | Comparable structure/depth to sample, references checked |
| Official cutoff | All | Submission and backup | Correct files uploaded and verified |

## 6. Report outline (use as a writing checklist)

| Section | Content and evidence required | Lead | First draft |
|---|---|---|---|
| **Title, authors, Abstract, Keywords** | Research aim, PIE, models, experiment grid, actual ADE/FDE findings; abstract written last | C + all | Nov 29 |
| **I. Introduction** | Why trajectory forecasting matters; H1/H2; contributions; distinguish crossing vs regression | All | Nov 28 |
| **II. Background & Related Work** | Cited prior work and how this project differs | C + B | Nov 25 |
| **III. Research Objectives & Methods** | Sampling, bbox bottom-centre, dataset, leakage-free split, features, models, 3×3 design, ADE/FDE equations | A + B + C | Nov 28 |
| **IV. Results & Discussion** | Dataset counts; model table; 3×3 heatmap; overlay examples; H1/H2 interpretation; error cases | C + all | Nov 28 |
| **V. Conclusion & Future Work** | State the actual answer to H1/H2; limitations; plausible next steps | All | Nov 29 |
| **References** | IEEE-style numbered citations consistent with text and figures | C; all check | Nov 29 |

**Minimum visual package:** (1) workflow diagram; (2) observed vs true vs predicted trajectories; (3) 3×3 experiment ADE heatmap; (4) ADE/FDE by horizon/observation. **Minimum tables:** (1) dataset/split summary; (2) model scores; (3) strongly preferred 3×3 experiment results. Report sample size and pixel units. Do **not** copy the sample paper's classification-only confusion matrices/t-SNE.

## 7. GitHub collaboration — keep it lightweight

| Rule | Agreement |
|---|---|
| Branches | A: `data/<task>`, B: `model/<task>`, C: `analysis/<task>` / `paper/<task>` |
| One task | GitHub Issue with **owner + due date + deliverable + success criterion** |
| Progress | Update the Issue / checkboxes when deliverables are pushed, not merely started |
| Code review | Open PR into `main`; another member checks correctness and runs related tests |
| Merge policy | One working version of configuration/splits/metrics on `main`; no rewriting colleagues' work without review |
| Check-in | Short asynchronous message **every 1–2 days**; call only for blocking issues |
| Data hygiene | Do **not** commit raw PIE annotations/videos or large model checkpoints; track scripts/configs/small tables/figures |
| If behind | Cut **P2 first**. Then reduce optional subgroup/extra plots. Never skip honest metrics, split integrity, or full report |

### Nov 18 meeting agenda (~30–40 minutes)

- [ ] **5 min:** Confirm due date/time and whether there are specific report/code submission rules.
- [ ] **10 min:** Agree on regression target, bbox coordinates, 5 Hz, three model baselines, H1/H2, ADE/FDE.
- [ ] **10 min:** Assign A/B/C; identify each person's available days; confirm handoff deadlines.
- [ ] **10 min:** Confirm repo access and assign first GitHub Issues.
- [ ] **5 min:** Decide where to write the shared paper (Overleaf IEEE template or other agreed editor).

### Final submission verification (one last checklist)

- [ ] Official deadline and submission portal verified.
- [ ] Every team member included in author list; names/order approved.
- [ ] Train/validation/test videos are disjoint; test set not used for tuning.
- [ ] All 9 observation/horizon combinations reported, with units and cohort counts.
- [ ] H1 and H2 answered **even if the hypotheses are not supported**.
- [ ] Models and preprocessing in paper match the committed implementation.
- [ ] Figures/tables numbered, captioned, cited in text and visually legible.
- [ ] Sources cited; limitations and failed cases discussed.
- [ ] Fresh-clone rerun instructions work; raw annotation ZIP not committed.
- [ ] All three reviewed the final PDF.
- [ ] Uploaded submission opened again and checked.

---

**Scheduling principle:** Nov 18 is the **first real team meeting**. Nothing is expected to have been assigned or completed before it. Dates are **proposed internal checkpoints**, not official course deadlines. If the LMS deadline is before the planned Nov 29–30 finish, adjust the plan at kickoff and remove optional work.
