# DA²C

## 面向大语言模型多智能体推理的分歧感知自适应协作

**Disagreement-Aware Adaptive Collaboration for Multi-Agent LLM Reasoning**

[English](README.md) | [简体中文](README.zh-CN.md)

DA²C 将陈述级证据转化为定向协作。自适应诊断问题定位需要核查的推理环节，成本感知路由器选择干预动作或停止，审核后的修订反馈到下一次决策。

## 方法框架

![DA²C 工作框架](assets/framework.png)

1. **独立生成：** 两个推理智能体分别形成候选答案及依据。
2. **诊断核查：** 以 Bloom 认知操作组织问题，定位推理问题并收集核查证据。
3. **按需协作：** 路由器选择补查证据、验证步骤、定向重推、仲裁或停止。
4. **审核更新：** 接纳修订后更新答案，并重新诊断当前状态。

## 论文配套说明

- [实验概览](docs/experiments.zh-CN.md)
- [关键设置](docs/runtime.md)

## 代码发布

实现代码、实验配置、运行与评估脚本将在论文录用后公开。

## 许可证

文档与原创图表采用 CC BY 4.0，自有软件采用 MIT。详见[LICENSE](LICENSE)。
