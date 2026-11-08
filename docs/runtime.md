# Key Settings / 关键设置

[Experimental overview](experiments.md) | [实验概览](experiments.zh-CN.md)

## Backbones / 基座

| Setting / 设置 | Qwen3-8B | Qwen3.8-27B |
| --- | --- | --- |
| Model repository / 模型仓库 | Qwen/Qwen3-8B | btbtyler09/Qwen3.8-27B-GPTQ-8bit |
| Precision / 精度 | BF16 | GPTQ 8bit |
| vLLM | 0.8.5 | 0.29.0 |
| Context / 上下文 | 16,384 | 16,384 |
| Built-in thinking / 内置思考 | Disabled / 关闭 | Disabled / 关闭 |
| Ridge alpha | 1 | 0.1 |
| Cost penalty lambda / 成本惩罚 | 0.03 | 0.01 |

## DA²C

Initial generation and revision use temperature 0.2; other modules use 0. Each question allows up to two interventions and three diagnostic questions, with an 8,000-token scheduling target. Initial outputs are capped at 2,048 tokens.

初始生成和修订温度为0.2，其余模块为0。每题最多两次干预、三个诊断问题，调度目标为8,000 token；初始输出上限为2,048 token。

| Module / 模块 | Output cap / 输出上限 |
| --- | ---: |
| Diagnosis / 诊断 | 384 |
| Checking / 核查 | 320 |
| Evidence rechecking / 补查证据 | 256 |
| Revision / 修订 | 768 |
| Arbitration / 仲裁 | 384 |
| Acceptance review / 接纳审核 | 384 |

Each backbone's router is calibrated on 200 GSM8K training and 200 MMLU validation questions, split by question into 60% fitting, 20% parameter selection, and 20% independent checking after excluding test overlap.

各基座的路由器使用200道 GSM8K 训练题和200道 MMLU 验证题独立校准，排除测试重叠后，按题目划分60%拟合、20%选参、20%独立检查。
