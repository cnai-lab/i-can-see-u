Evidence checked: **2026-11-01 14:52 IST (Asia/Jerusalem)**. Current SLURM queue: no pipeline jobs; interactive job `23768678` is unrelated.

Covers all **16 datasets currently present**, including 10 under `not_used_in_paper`. Input counts come from matching statistics CSVs, falling back to raw Parquet metadata. Result-file sizes are not used as input sizes.

| Dataset | Input size | Step 1: preprocessing | Step 2: BERTopic | Step 3: graph/authors | Classification sweep | Step 4 | Next action |
|---|---|---|---|---|---|---|---|
| Armenia | Unavailable | ⚠️ Terminal run unverified | ⚠️ 4/4 present; terminal unverified | ✅ Complete | ✅ 35/35 tasks | ⏸️ Campaign not configured | Recover terminal logs; retain validated topic data |
| China | 7,084,808 rows / 906.4 MiB | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ✅ 35/35 tasks | ⏸️ Campaign not configured | Verify Step 4 campaign configuration |
| China_1 | 3,670,754 posts | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ✅ 35/35 tasks | ✅ Historical six-campaign run | No verified missing stage |
| Cuba | 6,155,331 posts | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ✅ 35/35 tasks | ✅ Historical six-campaign run | No verified missing stage |
| Ecuador | 1,470,529 posts | ✅ Complete | ⚠️ 4/4 present; terminal unverified | ✅ Complete | ✅ 35/35 tasks | ⏸️ Campaign not configured | Recover terminal logs; retain validated topic data |
| Iran | 6,855,822 rows / 1018.1 MiB | ✅ Validated merge; 6,853,639 rows | ⏳ Ready from merged input | ⏸️ Waiting for Step 2 | ⏸️ Waiting for Step 2 | ⏸️ Campaign not configured | Run Step 2 from merged input, then {graph, sweep} |
| Iran_1 | 1,322,789 posts | ⚠️ Terminal run unverified | ⚠️ 4/4 present; terminal unverified | ⚠️ Latest run incomplete | ✅ 35/35 tasks | ✅ Historical six-campaign run | Investigate/retry graph; retain Step 2 data |
| Iran_2 | 1,631,793 posts / 224.2 MiB | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ⏳ Ready/not submitted | ⏸️ Campaign not configured | Submit classification sweep |
| Iran_3 | 2,004,438 posts / 296.5 MiB | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ⏳ Ready/not submitted | ⏸️ Campaign not configured | Submit classification sweep |
| Iran_4 | 561,585 posts / 90.5 MiB | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ⏳ Ready/not submitted | ⏸️ Campaign not configured | Submit classification sweep |
| Iran_5 | 45,405 posts / 9.6 MiB | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ⏳ Ready/not submitted | ⏸️ Campaign not configured | Submit classification sweep |
| Iran_6 | 1,289,812 posts / 215.9 MiB | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ⏳ Ready/not submitted | ⏸️ Campaign not configured | Submit classification sweep |
| Russia | 7,199,287 rows / 1.1 GiB | ✅ Complete | ✅ Complete; 4/4 present | ⚠️ OOM failure | ⚠️ 29/35 tasks; OOM | ⏸️ Campaign not configured | Recover graph and six sweep tasks |
| Russia_1 | 3,650,083 posts | ✅ Complete | ✅ Complete; 4/4 present | ✅ Complete | ✅ 35/35 tasks | ✅ Historical six-campaign run | No verified missing stage |
| UAE | 1,707,792 posts | ⚠️ Terminal run unverified | ⚠️ 4/4 present; terminal unverified | ✅ Complete | ✅ 35/35 tasks | ✅ Historical six-campaign run | Recover terminal logs; retain validated topic data |
| Venezuela | 1,972,732 posts | ⚠️ Terminal run unverified | ⚠️ 4/4 present; terminal unverified | ✅ Complete | ✅ 35/35 tasks | ✅ Historical six-campaign run | Recover terminal logs; retain validated topic data |

**Additional test datasets**

No `*_Test` dataset directories or input Parquets are currently present. Test datasets are excluded from the summary.

**Summary — 16 primary datasets**

| Step | Complete | Validated merge | Failed/incomplete | Unverified | Ready | Waiting/not configured |
|---|---:|---:|---:|---:|---:|---:|
| Step 1 | 11 | 1 | 0 | 4 | 0 | 0 |
| Step 2 | 10 | 0 | 0 | 5 | 1 | 0 |
| Step 3 | 13 | 0 | 2 | 0 | 0 | 1 |
| Classification sweep | 9 | 0 | 1 | 0 | 5 | 1 |
| Step 4 | 6 | 0 | 0 | 0 | 0 | 10 |

**Changes and evidence**

- **Iran:** the merged Step 1 artifact replaces the previous stale-file assessment. It contains **6,853,639 rows**, exactly the sum of Iran_1–Iran_6, with zero nulls in the three required cleaned-text/translation columns, `source_dataset`, `accountid`, `post_time`, and `is_control`. Its provenance and completed merge validation are documented in [note.md](not_used_in_paper/Labeled_Datasets/Iran/Iran_part_all/step_1_translate_topics_entities/note.md). Step 2 can consume this merged translation input. No combined-Iran Step 2, graph, or sweep output exists yet.
- The merge does **not** certify a successful combined-Iran Step 1 job or global feature recomputation. K-Means topics and reuse features remain specific to their source parts. Identical topic IDs across parts do not imply the same topic. Validate these feature semantics before interpreting merged graph/classification results. The original job `21893717` failed with CUDA OOM on September 30; rerunning translation is not required solely to replace the now-superseded stale file.
- **15/15 existing Step 2 Parquets contain all four topic configurations** (`10,128,256,512`), each with its top-words column and one non-null topic assignment per row. Topic `-1` counts as a valid assignment. Do not regenerate these columns solely because terminal reports are missing.
- **Armenia, Iran_1, UAE, Venezuela:** Step 1/2 terminal reports are not retained. Required Step 1 columns and nonempty output row counts pass metadata checks, but terminal success and latest-run write integrity cannot be independently certified. These are explicitly unverified rather than inferred complete from a Parquet alone.
- **Ecuador:** all four topic configurations are present and downstream runs completed, but the Step 2 report stops without a terminal success milestone. No matching later error log was found; failure is not assumed from this absence.
- **Iran_1 graph:** latest report `21072102` stops after copying the Step 2 input, and the output timestamp predates that run. The older successful graph run does not establish current completion.
- **Russia:** graph job `21893687` was OOM-killed on October 1. Sweep tasks **25, 28, 30, 32, 34, 35** were OOM-killed; **29/35** task reports reached completion and the result CSV contains **261 rows**. Resolve memory pressure before retrying.
- **Iran_2–Iran_6:** Steps 1–3 have completed artifacts and terminal success reports. Their classification sweeps are ready; they do not depend on Step 3.
- Other sweep datasets have 35/35 terminal task reports. Armenia records seven skipped percentage configurations; completion of tasks does not imply that every requested percentage produced results.
- **Step 4:** the shared six-campaign run recorded `Pipeline completed` and saved within/cross-campaign outputs. It covers China_1, Cuba, Iran_1, Russia_1, UAE, and Venezuela. Its results are historical and are not automatically refreshed after upstream changes. The notebook hardcodes this campaign set, while the sbatch input parameter remains commented out. Step 4 is not verified for the other ten datasets.
- The previous report's archive-only China_2 and Russia_2–Russia_5 folders, and Ecuador_Test, are no longer present in the inspected input/result locations. They are excluded from the current inventory rather than counted as failed or silently retained.
- Archived `old_2026_07_22` results and US_24 are excluded. No SLURM jobs were submitted, cancelled, or modified.

**Current artifact locations**

| Dataset | Raw input directory | Result directory |
|---|---|---|
| Armenia | `data/not_used_in_paper/Labeled_Datasets/Armenia` | `results/not_used_in_paper/Labeled_Datasets/Armenia` |
| China | `data/not_used_in_paper/Labeled_Datasets/China` | `results/not_used_in_paper/Labeled_Datasets/China` |
| China_1 | `data/Labeled_Datasets/China_1` | `results/Labeled_Datasets/China_1` |
| Cuba | `data/Labeled_Datasets/Cuba` | `results/Labeled_Datasets/Cuba` |
| Ecuador | `data/not_used_in_paper/Labeled_Datasets/Ecuador` | `results/not_used_in_paper/Labeled_Datasets/Ecuador` |
| Iran | `data/not_used_in_paper/Labeled_Datasets/Iran` | `results/not_used_in_paper/Labeled_Datasets/Iran` |
| Iran_1 | `data/Labeled_Datasets/Iran_1` | `results/Labeled_Datasets/Iran_1` |
| Iran_2 | `data/not_used_in_paper/Labeled_Datasets/Iran_2` | `results/not_used_in_paper/Labeled_Datasets/Iran_2` |
| Iran_3 | `data/not_used_in_paper/Labeled_Datasets/Iran_3` | `results/not_used_in_paper/Labeled_Datasets/Iran_3` |
| Iran_4 | `data/not_used_in_paper/Labeled_Datasets/Iran_4` | `results/not_used_in_paper/Labeled_Datasets/Iran_4` |
| Iran_5 | `data/not_used_in_paper/Labeled_Datasets/Iran_5` | `results/not_used_in_paper/Labeled_Datasets/Iran_5` |
| Iran_6 | `data/not_used_in_paper/Labeled_Datasets/Iran_6` | `results/not_used_in_paper/Labeled_Datasets/Iran_6` |
| Russia | `data/not_used_in_paper/Labeled_Datasets/Russia` | `results/not_used_in_paper/Labeled_Datasets/Russia` |
| Russia_1 | `data/Labeled_Datasets/Russia_1` | `results/Labeled_Datasets/Russia_1` |
| UAE | `data/Labeled_Datasets/UAE` | `results/Labeled_Datasets/UAE` |
| Venezuela | `data/Labeled_Datasets/Venezuela` | `results/Labeled_Datasets/Venezuela` |

The 11 ordinary completed Step 1 runs have terminal success reports and output timestamps within their recorded runs. Output row counts are nonempty and do not exceed raw input/statistics counts when those are available. All Step 1 outputs have the three required columns. Retained error logs for the newer runs were checked; some older corresponding error logs are no longer available. Historical log paths refer to the previous folder layout and are not commands for the current workspace.

**Recommended commands**

Commands below are provided for review and were **not executed**. They use current artifact locations. The sbatch templates still reference `i_can_see_u_env`; verify/update that environment setting to the project's `backbone_env_v2` before running them. Step 4 is omitted pending exact campaign/configuration verification. The full-pipeline helper starts again at Step 1 and selects only sweep task 28, so it is not suitable for this recovery report's full 35-task sweep.

Iran — begin at Step 2 using the separately validated merge, then queue graph and sweep as parallel consumers:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
step_1_output="../results/not_used_in_paper/Labeled_Datasets/Iran/Iran_part_all/step_1_translate_topics_entities/Iran_part_all_step_1.gzip.parquet"
step_2_output="../results/not_used_in_paper/Labeled_Datasets/Iran/Iran_part_all/step_2_BERTopic_clustering/Iran_part_all_step_2.gzip.parquet"
step_2_job_id=$(sbatch --parsable ../config/run_2_BERTopic_clustering.sbatch "$step_1_output" "10,128,256,512")
step_2_job_id="${step_2_job_id%%;*}"
graph_job_id=$(sbatch --parsable --dependency="afterok:${step_2_job_id}" ../config/run_3_key_authors_graph.sbatch "$step_2_output")
sweep_job_id=$(sbatch --parsable --dependency="afterok:${step_2_job_id}" ../config/run_3_classification_sweep.sbatch "$step_2_output")
printf 'Step 2: %s; graph: %s; sweep: %s\n' "$step_2_job_id" "$graph_job_id" "$sweep_job_id"
```

Iran_2 — submit the missing sweep directly:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
sweep_job_id=$(sbatch --parsable ../config/run_3_classification_sweep.sbatch "../results/not_used_in_paper/Labeled_Datasets/Iran_2/Iran_2_part_all/step_2_BERTopic_clustering/Iran_2_part_all_step_2.gzip.parquet")
printf 'Sweep: %s\n' "$sweep_job_id"
```

Iran_3 — submit the missing sweep directly:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
sweep_job_id=$(sbatch --parsable ../config/run_3_classification_sweep.sbatch "../results/not_used_in_paper/Labeled_Datasets/Iran_3/Iran_3_part_all/step_2_BERTopic_clustering/Iran_3_part_all_step_2.gzip.parquet")
printf 'Sweep: %s\n' "$sweep_job_id"
```

Iran_4 — submit the missing sweep directly:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
sweep_job_id=$(sbatch --parsable ../config/run_3_classification_sweep.sbatch "../results/not_used_in_paper/Labeled_Datasets/Iran_4/Iran_4_part_all/step_2_BERTopic_clustering/Iran_4_part_all_step_2.gzip.parquet")
printf 'Sweep: %s\n' "$sweep_job_id"
```

Iran_5 — submit the missing sweep directly:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
sweep_job_id=$(sbatch --parsable ../config/run_3_classification_sweep.sbatch "../results/not_used_in_paper/Labeled_Datasets/Iran_5/Iran_5_part_all/step_2_BERTopic_clustering/Iran_5_part_all_step_2.gzip.parquet")
printf 'Sweep: %s\n' "$sweep_job_id"
```

Iran_6 — submit the missing sweep directly:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
sweep_job_id=$(sbatch --parsable ../config/run_3_classification_sweep.sbatch "../results/not_used_in_paper/Labeled_Datasets/Iran_6/Iran_6_part_all/step_2_BERTopic_clustering/Iran_6_part_all_step_2.gzip.parquet")
printf 'Sweep: %s\n' "$sweep_job_id"
```

Russia — after addressing memory pressure, retry graph and only the six incomplete sweep tasks in parallel:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
step_2_output="../results/not_used_in_paper/Labeled_Datasets/Russia/Russia_part_all/step_2_BERTopic_clustering/Russia_part_all_step_2.gzip.parquet"
graph_job_id=$(sbatch --parsable ../config/run_3_key_authors_graph.sbatch "$step_2_output")
sweep_job_id=$(sbatch --parsable --array=25,28,30,32,34,35 ../config/run_3_classification_sweep.sbatch "$step_2_output")
printf 'Graph: %s; sweep: %s\n' "$graph_job_id" "$sweep_job_id"
```

Iran_1 — inspect the latest failed/incomplete graph run first. The existing Step 2 columns pass separate data checks; after confirming the missing terminal evidence, this retries only the graph consumer:

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you/src
graph_job_id=$(sbatch --parsable ../config/run_3_key_authors_graph.sbatch "../results/Labeled_Datasets/Iran_1/Iran_1_part_all/step_2_BERTopic_clustering/Iran_1_part_all_step_2.gzip.parquet")
printf 'Graph: %s\n' "$graph_job_id"
```

Armenia — Recover missing terminal evidence; retain existing topic columns. No verified missing eligible stage requires submission.

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you
find "results/not_used_in_paper/Labeled_Datasets/Armenia" -type f     \( -name 'progress_report*.txt' -o -name '*_step_2.gzip.parquet' \) -print
```

Ecuador — Recover missing terminal evidence; retain existing topic columns. No verified missing eligible stage requires submission.

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you
find "results/not_used_in_paper/Labeled_Datasets/Ecuador" -type f     \( -name 'progress_report*.txt' -o -name '*_step_2.gzip.parquet' \) -print
```

UAE — Recover missing terminal evidence; retain existing topic columns. No verified missing eligible stage requires submission.

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you
find "results/Labeled_Datasets/UAE" -type f     \( -name 'progress_report*.txt' -o -name '*_step_2.gzip.parquet' \) -print
```

Venezuela — Recover missing terminal evidence; retain existing topic columns. No verified missing eligible stage requires submission.

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you
find "results/Labeled_Datasets/Venezuela" -type f     \( -name 'progress_report*.txt' -o -name '*_step_2.gzip.parquet' \) -print
```

China — Completed through graph/sweep. Verify the Step 4 campaign configuration before queuing that stage.

```bash
cd /groups/puzis_dtgroup/cnalab/i_can_see_you
find "results/not_used_in_paper/Labeled_Datasets/China" -type f     \( -name 'progress_report*.txt' -o -name '*_step_2.gzip.parquet' \) -print
```

China_1, Cuba, and Russia_1 have no verified missing stage to submit. Their shared Step 4 results are historical.
