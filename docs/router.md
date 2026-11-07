# Router specification / 路由器说明

[Protocol / 实验协议](experiments.md) · [Selected parameters / 最终参数](../configs/runtime.json)

## Calibration / 校准

Each backbone is calibrated separately on 200 GSM8K training and 200 MMLU validation questions, excluding test overlaps by question fingerprints. The question-level split is 60% fit, 20% hyperparameter selection and 20% independent checking. For each state, four intervention branches share candidates and diagnostics, with two repeats per action. Later states follow seeded random actions without gold-answer-based trajectory selection. Labels are the change in correctness of the answer retained after the intervention and review, in {-1,0,+1}.

两个基座分别使用200道GSM8K训练题及200道MMLU验证题校准，通过题目指纹排除测试重叠。按题目划分60%拟合、20%选参、20%独立检查。同一状态的四个干预分支共享候选与诊断，每动作重复两次；后续状态由固定种子随机动作产生，不按标准答案挑选轨迹。收益标签为干预与审核后保留答案的正确性变化，取-1/0/+1。

Each action has two Ridge models: correctness gain and log(1 + token cost). Feature standardization is fitted on that action's fitting partition. Alpha is selected from {0.1,1,10} by gain-prediction MSE on the selection partition. Lambda is selected from {0,0.01,0.03,0.1,0.3}: retain choices within 0.005 of the highest selection gain, then prefer lower cost, breaking remaining ties toward larger lambda. The independent check partition is not used for selection.

每动作拟合收益与log(1+token成本)两个Ridge模型，标准化统计量仅来自该动作拟合集。alpha从{0.1,1,10}中按选择集收益预测MSE选择；lambda从{0,0.01,0.03,0.1,0.3}中选择：保留距最高选择集收益不超过0.005的候选，再优先低成本，同成本时优先较大lambda。独立检查集不参与选参。

## Features and decision / 特征与决策

The ordered 34 features encode candidate validity and agreement (2), numeric/categorical confidence (5), round and budget (3), diagnostic validity (1), relation (5), error kind (5), cognitive operation (6), check verdict (3) and source (4). See the [feature dictionary](features.md) for field order and missing-value encoding. No gold answer, question ID or retrospective correctness label is a predictor.

34维有序特征包括：候选有效性与一致性2维、数值/类别置信度5维、轮数与预算3维、诊断有效性1维、关系5维、错误类别5维、认知操作6维、核查结论3维及来源4维。字段顺序与缺失编码见[特征表](features.md)。预测输入不包含标准答案、题目ID或事后正确性。

`utility(a) = predicted_gain(a) - lambda * predicted_tokens(a) / 1000`; stopping has utility zero. DA²C chooses the highest positive-utility structurally available action whose predicted cost fits the remaining budget. Marginal branch cost includes intervention, any repair/review and subsequent diagnosis; total online cost additionally includes initial candidates and the first diagnostic/check cycle. Every actual request also checks its input and required output allowance.

效用为预计正确性增量减去lambda乘以预计token/1000，停止效用为零。DA²C在结构可用、预计成本不超过剩余预算的动作中选取最高正效用动作。分支边际成本包含干预、可能的修复/审核及后续诊断；完整在线成本还计入初始候选和首次诊断核查。实际请求另行检查输入及所需输出额度。

## Routing controls / 路由对照

All controls retain diagnosis, checks, review, re-diagnosis and request-time budget checks, with shared initial candidates. They do not use Ridge gain/cost predictions or its predicted-cost feasibility mask.

各对照共享初始候选，保留诊断、核查、审核、重新诊断和实际请求预算检查，不使用Ridge收益/成本预测及其预测成本掩码。

| Control / 对照 | Selection rule / 动作规则 |
| --- | --- |
| Random | Uniform draw from structurally available actions plus stop; no redraw after stop. / 从结构可用动作与stop中均匀抽取，停止后不重抽。 |
| Fixed schedule | Round 1 verify_step, round 2 arbitrate; stop if specified action unavailable. / 首轮verify_step、次轮arbitrate，不可用即停。 |
| Rule-based | Invalid diagnosis/unavailable check or exhausted request budget: stop. Equivalent/complementary plus supported: stop. Otherwise conditions→recheck_evidence; calculation→verify_step; dependency→targeted_rereason; selection→arbitrate; unknown kind→verify_step. Unavailable chosen action: stop. / 先处理诊断无效、核查不可用、预算耗尽及已支持的等价/互补状态，再按错误类别映射动作；所选动作不可用即停。 |

A valid `unknown` verdict means insufficient evidence, not a failed check interface. Each ablation refits its own coefficients using the full method's selected alpha/lambda and the same development split. Fitted weights, standardization arrays and calibration identities are planned with the executable release; configuration metadata is available now.

有效的`unknown`表示证据不足，不等同于核查接口失败。各消融沿用完整方法的alpha/lambda和开发划分，独立重新拟合系数。当前公开配置元数据；拟合权重、标准化数组与校准身份记录随可执行版本发布。
