# Tasks / 任务清单

Frozen task inventory; hashes identify the formatted files and ordered samples. / 哈希标识格式化文件和有序样本集。

| Benchmark | Task / 任务 | N | Answer type / 答案类型 |
| --- | --- | ---: | --- |
| BBH | `boolean_expressions` | 250 | short_answer |
| BBH | `disambiguation_qa` | 250 | choice |
| BBH | `formal_fallacies` | 250 | short_answer |
| BBH | `geometric_shapes` | 250 | choice |
| BBH | `ruin_names` | 250 | short_answer |
| BBH | `sports_understanding` | 250 | short_answer |
| GSM8K | `gsm8k_test` | 1319 | short_answer |
| MMLU | `college_biology_test` | 144 | choice |
| MMLU | `college_medicine_test` | 173 | choice |
| MMLU | `electrical_engineering_test` | 145 | choice |
| MMLU | `high_school_world_history_test` | 237 | choice |
| MMLU | `human_aging_test` | 223 | choice |
| MMLU | `marketing_test` | 234 | choice |

`ruin_names` uses option-letter normalization despite its legacy short_answer metadata. GSM8K uses numerical scoring.
`ruin_names` 的旧元数据为 short_answer，评分仍按选项字母规范化；GSM8K 使用数值评分。

Sources / 数据来源: [BBH](https://github.com/suzgunmirac/BIG-Bench-Hard), [GSM8K](https://github.com/openai/grade-school-math), [MMLU](https://github.com/hendrycks/test).
Dataset text is not redistributed here; use the original sources and licenses. / 此处不重新分发题目正文，使用原始来源及许可。

[Machine-readable inventory / 机器可读清单](../configs/tasks.json) · [Protocol / 实验协议](experiments.md)
