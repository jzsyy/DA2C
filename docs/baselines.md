# Baseline implementations / 基线实现

[Protocol / 实验协议](experiments.md) · [Runtime / 基座与GPT请求](runtime.md)

Temperatures, output caps and tools below describe the reported operating points. An omitted request field means it was not explicitly sent, not that a particular server default was measured. Local Qwen runs disable built-in thinking. Full input and output usage includes selectors, discussion, feedback and retries.

下表记录实验实际运行点。请求参数“未发送”不等于已经测得某个服务端默认值。本地Qwen关闭内置thinking；计费涵盖选择器、讨论、反馈及重试的输入和输出。

| Method | Sampling and procedure / 采样与过程 | Tools and budget / 工具及预算 |
| --- | --- | --- |
| CoT | One natural-language reasoning sample; T=0.2, top_p=1, output≤2048. / 单次自然语言推理。 | No external tool; 8000 scheduling target. / 无外部工具，8000调度目标。 |
| Self-Consistency | Three independent CoT samples, normalized-answer vote; same sampling. / 三个独立样本规范化答案投票。 | Same as CoT; full sampling cost. / 同CoT，计入全部采样。 |
| Best-of-N | Three CoT samples plus a same-backbone selector of existing candidates; same sampling. / 三样本加同基座选择器，只选已有候选。 | Same target; selector charged. / 同预算，选择器计费。 |
| BloomWise-BLES | Actual online adjacent-level agreement early stop; final-level answer if no agreement. T=0.2, output≤2048; top_p omitted. / 在线相邻层一致早停，否则返回末层答案。 | No shared 8000 total cap. / 不施加共同8000总额度。 |
| BloomWise-BLM | Six cognitive-level generations and aggregation; same sampling as BLES. / 六层生成后集成。 | Same as BLES. / 同BLES。 |
| MAD-M² (S) | Subjective masking, 3 agents, 2 rounds including initialization; T=0.2, top_p=1, output≤2048; logprobs enabled. / 主观记忆屏蔽，三agent两轮（含初始化）。 | No shared total cap; no external calculator. / 不施加共同总额度，无外部计算器。 |
| AgentPrune | T=0.2, top_p=1, output≤2048; 2 rounds; general graph 5 agents, math graph 4; FinalRefer decision. / 通用图五agent、数学图四agent，讨论两轮。 | GSM8K mathematical roles retain Python; no shared total cap. / 数学角色保留Python，不施加共同总额度。 |
| MAPS (text-adapted) | Interpreter→Aligner→Scholar→Solver; Critic chooses the lowest-scored stage and reruns it and downstream stages. T=0.2, Critic T=0; top_p omitted; output≤2048. / 四阶段推理加Critic反馈。 | Bounded calculator; up to 2 revisions; score threshold 0.8 after dividing 1–5 scores by 5; 8000 target. / 受限计算器、最多两次修订、归一化阈值0.8、8000目标。 |

AgentPrune uses the authors' public graph optimization, pruning and regularization implementation: FullConnected initialization, 10 optimization iterations, batch size 4, learning rate 0.1, pruning every 5 iterations at rate 0.25, with spatial and temporal optimization. The general graph is optimized on MMLU development data and transferred to BBH; the mathematical graph uses GSM8K training data. A training-example/test overlap identified in the adapted mathematical decision prompt is removed.

AgentPrune采用作者公开代码的图优化、剪枝及正则化实现：FullConnected初始化、10次优化迭代、batch size 4、学习率0.1、每5次迭代剪枝、比例0.25，空间与时间连接均优化。通用图在MMLU开发数据优化后迁移至BBH，数学图使用GSM8K训练数据。适配数学决策提示中识别出的训练示例/测试重叠已移除。

MAPS preserves the personality-guided staged reasoning and reflective feedback while removing image inputs and using text task/answer schemas. Its local adapter is `maps-upstream-text-1`. This name remains **MAPS (text-adapted)** throughout the tables and documentation.

MAPS保留人格指导的分阶段推理与反思反馈，移除图像输入、使用文本题目与答案格式。本地适配器为`maps-upstream-text-1`，表格及文档统一标为**MAPS (text-adapted)**。

## Source pins / 上游版本

| Method | Upstream source / 源码 | Commit |
| --- | --- | --- |
| MAD-M² | [tmlr-group/MAD-MM](https://github.com/tmlr-group/MAD-MM) | `f02069add08280b764d059a2f06ca0043aa093e2` |
| BloomWise | [marilena1123/BloomWise](https://github.com/marilena1123/BloomWise) | `c8d843473672371ad7a35badc8471f80f6d4ffbc` |
| AgentPrune | [yanweiyue/AgentPrune](https://github.com/yanweiyue/AgentPrune) | `c544dd6a1858c02c6d5d371d23c6e6ff55e0be21` |
| MAPS (text-adapted) | [exoskeletonzj/MAPS](https://github.com/exoskeletonzj/MAPS) | Local author-file hashes in the adapter provenance; see [source identity](../configs/maps_source.json). / 适配来源以作者文件哈希标识。 |

The CoT/Self-Consistency/Best-of-N runners are local implementations of these procedures, not the original authors' executable code. Their exact shared solve and selector instructions are in [text_baselines.json](../prompts/text_baselines.json). Upstream baseline prompts retain their original licensing; adapted code and a complete source inventory are planned with the implementation release.

CoT、自一致性及Best-of-N使用本地实现，不称为原作者源码。实际解题与选择器提示见[Prompt文件](../prompts/text_baselines.json)。第三方基线提示沿用原许可，适配代码和完整源码清单随实现发布。
