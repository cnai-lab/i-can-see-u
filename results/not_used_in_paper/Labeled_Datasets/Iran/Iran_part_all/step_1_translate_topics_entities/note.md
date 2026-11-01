# Merged Iran Step 1 results

This file is a merge of the preprocessing (Step 1) results from Iran_1, Iran_2, Iran_3, Iran_4, Iran_5, and Iran_6.

Created: 2026-11-01T14:47:49+02:00.
Output: `Iran_part_all_step_1.gzip.parquet`.
Total rows: 6,853,639.

| Source dataset | Rows | Source file |
|---|---:|---|
| Iran_1 | 1,322,573 | `results/Labeled_Datasets/Iran_1/Iran_1_part_all/step_1_translate_topics_entities/Iran_1_part_all_step_1.gzip.parquet` |
| Iran_2 | 1,631,552 | `results/not_used_in_paper/Labeled_Datasets/Iran_2/Iran_2_part_all/step_1_translate_topics_entities/Iran_2_part_all_step_1.gzip.parquet` |
| Iran_3 | 2,003,842 | `results/not_used_in_paper/Labeled_Datasets/Iran_3/Iran_3_part_all/step_1_translate_topics_entities/Iran_3_part_all_step_1.gzip.parquet` |
| Iran_4 | 561,295 | `results/not_used_in_paper/Labeled_Datasets/Iran_4/Iran_4_part_all/step_1_translate_topics_entities/Iran_4_part_all_step_1.gzip.parquet` |
| Iran_5 | 45,348 | `results/not_used_in_paper/Labeled_Datasets/Iran_5/Iran_5_part_all/step_1_translate_topics_entities/Iran_5_part_all_step_1.gzip.parquet` |
| Iran_6 | 1,289,029 | `results/not_used_in_paper/Labeled_Datasets/Iran_6/Iran_6_part_all/step_1_translate_topics_entities/Iran_6_part_all_step_1.gzip.parquet` |

Rows are concatenated in Iran_1 through Iran_6 order, preserving the row order within each file. No deduplication, filtering, translation, clustering, or feature recomputation was performed.

All original columns are retained. Columns absent from an individual part are filled with nulls. The added `source_dataset` column identifies each row's source. The original `__index_level_0__` values are retained as a regular column; pandas index metadata is removed so they are not treated as a unique merged index.

K-Means topic labels and topic-reuse/account-reuse features were computed independently within each source part. Identical topic numbers across parts do not imply a shared topic, including when the K-Means column name is identical. Reuse features have not been recomputed across parts. This merged artifact is not evidence that the original combined-Iran preprocessing job finished successfully.

Validation: the merged Parquet schema, total row count, per-source row counts, and null counts for postid and the three cleaned-text/translation columns match the source files.
