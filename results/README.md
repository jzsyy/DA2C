# Reported results / 论文实测汇总

These are the five author-provided aggregate tables used by the manuscript, averaged over seeds 42/43/44. Conversion to UTF-8 BOM preserves every cell, including original labels. [Provenance](provenance.json) records input/output hashes. Per-seed predictions and trajectories are not included in this aggregate package.

以下为论文使用的五张作者实测汇总表，数值为42/43/44三轮均值。仅转换为UTF-8 BOM，全部单元格及原标签不变；[来源清单](provenance.json)记录输入/输出哈希。此包不包含逐种子预测和完整轨迹。

| File / 文件 | Paper / 论文位置 | Contents / 内容 |
| --- | --- | --- |
| [table1_qwen3-8b_main.csv](table1_qwen3-8b_main.csv) | Table I | Task accuracy, benchmark aggregates, task macro mean, tokens/question / 子任务准确率、分基准汇总、宏平均、每题token |
| [table2_ablation.csv](table2_ablation.csv) | Table II | Pooled accuracy, F/D, tokens/question / 微平均、纠错/误改、token |
| [table3_routing.csv](table3_routing.csv) | Fig. 2a | Routing-control exact coordinates / 路由对照精确点位 |
| [table4_budget.csv](table4_budget.csv) | Fig. 2b/c | Scheduling target, accuracy, mean/P50/P95 tokens / 调度目标、准确率及用量分布 |
| [table5_qwen3.8-27b.csv](table5_qwen3.8-27b.csv) | Table III | 27B micro/macro accuracy, tokens/question / 第二基座微/宏平均与token |

Accuracy is in percent; tokens are counts, not currency. F/D are endpoint wrong→correct / correct→wrong counts, averaged then rounded. P50/P95 describe within-run question-level usage and are averaged over runs, not confidence intervals across seeds.

准确率为百分数，token为数量。F/D为初始错误→最终正确/初始正确→最终错误的题数，平均后取整。P50/P95为各轮逐题用量分位数的跨运行均值，不是种子间置信区间。

## Original-label dictionary / 原标签对照

| Original | Interpretation / 解释 |
| --- | --- |
| 完整 | Full DA²C |
| 无诊断 | w/o claim-level diagnosis |
| 普通问题链 | Generic question chain |
| 固定六层 | Fixed cognitive order; at most three questions / 固定层序、最多三问 |
| 打乱问题链 | Shuffled operation schedule |
| 无 Bloom 标签 | w/o Bloom labels |
| 无核查器 | w/o diagnostic checking; review retained / 保留审核 |
| 无重诊断 | w/o re-diagnosis |
| 无接纳门控 | w/o acceptance gate; arithmetic screen retained / 保留算术筛查 |
| 初始状态 / 随机路由 / 固定路由 / 规则路由 | Initial / Random / Fixed schedule / Rule-based |
| 不计成本 / 完整路由 | No cost penalty / Full router |
| 微平均 / 宏平均 / Token/题 | Micro accuracy / Task macro accuracy / Tokens per question |
| 额度 / 均值 | Scheduling target / Mean actual tokens |

Task abbreviations map to the [task inventory](../docs/tasks.md). Definitions and denominators are maintained in [scoring](../docs/scoring.md).

任务缩写对应[完整任务名](../docs/tasks.md)，分母及指标定义统一见[评分说明](../docs/scoring.md)。
