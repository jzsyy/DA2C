# Scoring and accounting / 评分与计费

[Task inventory / 任务](tasks.md) · [Experimental protocol / 实验协议](experiments.md)

## Answers / 答案

Scoring extracts the declared final-answer field/tag and normalizes the answer for its task. Choice tasks compare option labels (with supported option-text normalization); boolean expressions use True/False; formal fallacies use valid/invalid; sports understanding uses yes/no; GSM8K compares final numerical values after formatting normalization. An empty or invalid answer counts as incorrect. Standard answers are used offline for scoring, not supplied to online agents. A reasoning trace is not searched for whichever fragment matches the gold answer.

评分读取声明的最终答案字段/标签，按任务规范化。选择题比较选项标签（支持对应选项文本规范化）；boolean expressions使用True/False，formal fallacies使用valid/invalid，sports understanding使用yes/no，GSM8K按格式规范化后的最终数值比较。空答案或无效答案记错；标准答案仅用于离线评分，不提供给在线agent，也不从推理文本中寻找恰好命中标准答案的片段。

## Aggregation / 汇总

- Per-task accuracy: correct / all questions in that task, including failures. / 单任务准确率：正确题数除以全部题目，包含失败题。
- Main-table Overall: equal-weight mean of 13 task accuracies. BBH Avg.: mean of six BBH tasks. MMLU W.Avg.: pooled accuracy over six MMLU tasks. / 主表Overall为13任务等权宏平均；BBH为六任务均值；MMLU为六任务样本加权平均。
- Ablation/routing/budget accuracy: pool all 3,975 questions in each run. The 27B table reports both micro and task macro averages. / 消融、路由、预算表按3975题微平均；27B表同时列微平均和任务宏平均。
- Tokens/question: total online tokens over all 3,975 questions divided by 3,975 within each run. / 每题token：每轮全部在线token除以3975。
- Published cells are means of the three runs, seeds 42/43/44. F (initial wrong→final correct) and D (initial correct→final wrong) are averaged and then rounded. / 表格数值为三轮均值；F纠错、D误改先平均后取整。

## Token scope / Token口径

Online usage includes input and output for initial candidates, claim extraction, diagnosis, checking, evidence rechecking, repair, arbitration, acceptance review, selectors and retries. A reused frozen initial state is still charged its full generation cost to every ablation/control. Offline router collection/fitting and AgentPrune graph optimization are separate from online inference.

在线用量包含初始候选、陈述抽取、诊断、核查、补查证据、修复、仲裁、审核、选择器及重试的输入/输出。共享冻结初始候选仍对每个消融/对照计入完整生成费用。离线路由采集/拟合与AgentPrune图优化不混入在线推理。

For GPT Responses, total = input_tokens + output_tokens. Cached input tokens and reasoning tokens are subsets, not extra quantities added again. Missing usage is unknown, not zero; measured coverage and any scheduling estimates belong in raw run records. Token counts are not monetary charges across different models/providers. The 8000-token setting is a scheduling target checked before calls and charged against returned usage; per-call output allowances can shrink.

GPT Responses总量为input_tokens+output_tokens，cached input与reasoning tokens为其中子集，不重复加总。缺失usage记未知而非零，原始记录分别保存实测覆盖率及调度估计。跨模型/供应商的token不直接换算为相同费用。8000是调用前检查、按返回usage扣账的调度目标，逐次输出额度可缩小。
