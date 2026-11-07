# 实验协议

[English](experiments.md) | [简体中文](experiments.zh-CN.md) | [项目首页](../README.zh-CN.md)

本材料对应论文报告的A100实验，采用42/43/44三个种子。当前公开实验说明、源码提取的Prompt和实测汇总表；实现与复现脚本计划录用后发布。

## 阅读顺序

| 内容 | 唯一维护位置 |
| --- | --- |
| 完整任务名、题量与输入身份 | [任务清单](tasks.md)、[JSON](../configs/tasks.json) |
| 基座、采样、模块上限及GPT调用 | [运行配置](runtime.md)、[JSON](../configs/runtime.json)、[日期](../configs/gpt54_dates.json) |
| 基线实现与适配 | [基线](baselines.md) |
| 校准、特征及路由对照 | [路由器](router.md)、[特征字典](features.md) |
| 实际模块指令 | [Prompt](../prompts/README.md) |
| 答案规范化、汇总及计费 | [评分](scoring.md) |
| 五张实测汇总表 | [结果](../results/README.md) |
| 可用材料、许可及快照 | [发布说明](release.md) |

## 受控消融

所有变体共享题目、基座、评分及同题同种子的冻结初始候选，各自计入完整初始生成成本。各变体在相同开发划分重新采集动作结果、拟合系数，固定完整方法的alpha/lambda，不另行搜索。基座权重冻结，共同轮数、预算与生成参数见[runtime.json](../configs/runtime.json)。

| 变体 | 改变的机制 |
| --- | --- |
| Generic question chain | 保留陈述定位、每次一个针对性问题；移除认知类别、层序及功能描述。 |
| Fixed cognitive order | Remember→Understand→Apply→Analyze→Evaluate→Create固定顺序，最多三问，正常早停。 |
| Shuffled question chain | 打乱操作调度顺序，不是打乱已生成的问题；后续问题仍由当前状态生成。 |
| w/o Bloom labels | 改用中性编号，保留功能描述、类别特征及自适应选择。 |
| w/o claim-level diagnosis | 移除诊断及前置核查，以固定顺序取当前答案对应的完整候选。补查原题条件、重审整份候选，禁用verify_step；保留仲裁、停止、算术筛查与审核。 |
| w/o diagnostic checking | 保留诊断与定位，核查特征记缺失；保留算术筛查和接纳审核。 |
| w/o acceptance gate | 保留诊断核查和算术筛查，格式有效且未被算术筛查阻止的修订直接提交。 |
| w/o re-diagnosis | 沿用首次诊断/核查，目标消失或无新证据重复动作时停止。 |

缺失信息在校准与测试中使用一致编码，不表示核查通过。移除陈述级诊断同时改变动作信息与可用动作。“固定层序”仍受最多三问约束。

表格均为三轮均值。主表Overall为任务宏平均；消融、路由和预算表按3975题微平均。精确定义见[评分说明](scoring.md)。本包不包含模拟实验数值。
