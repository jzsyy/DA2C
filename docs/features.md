# Feature dictionary / 特征字典

[Router / 路由器](router.md)

Ordered fields from the shared encoder in da2c/routing.py. The denominator for candidate fractions is max(1, candidate count).

以下按共享特征编码器的实际顺序列出34维字段，候选比例分母为max(1,候选数)。

| Dimension | Field | Encoding |
| ---: | --- | --- |
| 1 | valid_fraction | Number of format-valid candidates / n |
| 2 | answer_agreement | 1 if there are at least two valid candidates and all valid candidates have the same normalized answer; otherwise 0 |
| 3 | numeric_confidence_fraction | Candidates with numeric confidence / n |
| 4 | min_confidence | Minimum available numeric confidence; 0 when none is available, interpreted with dimension 3 |
| 5 | label_high_fraction | Candidates labeled high / n |
| 6 | label_medium_fraction | Candidates labeled medium / n |
| 7 | label_low_fraction | Candidates labeled low / n |
| 8 | round | Current intervention round |
| 9 | remaining_fraction | Remaining tokens / max(1, total scheduling target) |
| 10 | budget_thousands | Total scheduling target / 1,000 |
| 11 | diagnostic_valid | Indicator of diagnostic validity |
| 12 | relation_contradiction | Equality indicator |
| 13 | relation_equivalent | Equality indicator |
| 14 | relation_complementary | Equality indicator |
| 15 | relation_insufficient | Equality indicator |
| 16 | relation_invalid | Equality indicator |
| 17 | kind_conditions | Equality indicator |
| 18 | kind_calculation | Equality indicator |
| 19 | kind_dependency | Equality indicator |
| 20 | kind_selection | Equality indicator |
| 21 | kind_unknown | Equality indicator |
| 22 | stage_remember | Equality indicator |
| 23 | stage_understand | Equality indicator |
| 24 | stage_apply | Equality indicator |
| 25 | stage_analyze | Equality indicator |
| 26 | stage_evaluate | Equality indicator |
| 27 | stage_create | Equality indicator |
| 28 | check_supported | Equality indicator |
| 29 | check_refuted | Equality indicator |
| 30 | check_unknown | Equality indicator |
| 31 | source_calculator | Equality indicator |
| 32 | source_question_quote | Equality indicator |
| 33 | source_model_judgment | Equality indicator |
| 34 | source_unverified | Equality indicator |

High/medium/low remain categorical labels. Missing numeric confidence uses zero together with numeric_confidence_fraction=0, not an observed zero probability. Category blocks use equality indicators; absent/out-of-category values yield an all-zero block. Explicit unknown/invalid/unverified select the matching indicator. Ablations encode missing diagnostic/check fields consistently during calibration and evaluation.

high/medium/low保留类别语义。缺失数值置信度的零占位与可用比例为零联合解释，不是观测到零概率。类别块按相等指示编码，缺失或不属于该块的值形成全零块；显式unknown/invalid/unverified进入对应维度。消融的缺失编码在校准与评估时保持一致。
