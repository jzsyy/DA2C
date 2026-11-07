# Prompt templates / 提示模板

These files expose the instructions used by the existing implementation. DA²C templates are extracted from the local source that the author confirmed was reused for the final experiments. They are not newly written illustrative prompts. Dynamic task text, candidate claims, IDs and check records are populated at runtime; no test answers, credentials or private endpoints are included.

这些文件公开已有实现使用的指令。DA²C模板从本地代码提取，作者确认最终实验沿用这些模板，不是为文档重新编写的示意提示。题目、候选陈述、ID与核查记录在运行时填入，不包含标准答案、密钥或私有接口。

| File / 文件 | Contents / 内容 |
| --- | --- |
| [da2c.json](da2c.json) | Independent generation, extraction, diagnosis, scoped checks, evidence, revision, arithmetic feedback, review and arbitration; source hashes. / 独立推理到仲裁的完整模块指令及源文件哈希。 |
| [format_contract.json](format_contract.json) | Exact output-format suffix function used by the request envelope. / 请求封装使用的格式约束函数。 |
| [text_baselines.json](text_baselines.json) | CoT/SC solve instruction, task suffixes, Best-of-N selector. / 基础采样与选择器提示。 |
| [gpt54.json](gpt54.json) | Direct and Reasoning final-answer instructions. / GPT最终答案格式指令。 |

## Assembly and visibility / 拼接及信息范围

For DA²C, combine the `system_envelope` with the module instruction and its applicable format suffix. The user message is a JSON object containing the public question, task/answer type and the module's permitted state fields. Initial reasoners do not see each other's candidates. The diagnostic module sees extracted statements and answer agreement; statement checking omits candidate final-answer fields, while an assertion may still contain numerical task values. Revision is directed to the original question, not merely the diagnostic subquestion. Review receives the proposed standalone solution; arbitration selects among supplied existing solutions.

DA²C系统消息由`system_envelope`、模块指令及适用格式后缀构成；用户消息为公开题目、任务/答案类型与模块允许状态字段组成的JSON。初始reasoner互不查看候选；诊断读取抽取陈述和答案关系；陈述核查不传入候选最终答案字段，但陈述本身可能包含题目数值；修订回答原题而非只回答诊断子问题；审核读取独立修订解答，仲裁仅选择已有解答。

`STEP1/STEP2` and `SOL1/SOL2` demonstrate the runtime ID schema, not fixed task IDs or expected answers. Schema repair can add the previous response, validation feedback and the remaining retry count; at most one format retry is allowed. Full dynamic payload construction and ablation-specific transformations accompany the implementation release. Third-party collaborative baseline prompts are identified through the pinned [upstream sources](../docs/baselines.md) and are not relicensed as original DA²C material.

`STEP1/STEP2`及`SOL1/SOL2`展示运行时ID格式，不是固定题号或预期答案。格式重试会加入前次响应、校验反馈和剩余重试次数，最多一次。完整动态载荷构造及消融变换随代码发布；第三方协作基线提示通过[上游版本](../docs/baselines.md)定位，不作为DA²C原创材料重新授权。
