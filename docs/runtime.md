# Runtime and requests / 运行与请求配置

[Protocol / 实验协议](experiments.md) · [机器可读配置 / JSON](../configs/runtime.json)

The JSON file is the single numerical configuration record. It describes the reported experiments, not a runnable launcher. The author confirmed the final-server settings against the earlier deployment, with A100 hardware and seeds 42/43/44; the 27B router was calibrated independently.

JSON 是数值配置的唯一维护位置，是实验说明而非可执行启动文件。作者确认最终服务器沿用旧部署设置，硬件改为 A100、重复运行种子为42/43/44，27B路由器独立校准。27B使用的是所列社区仓库的GPTQ 8bit权重，不能将其写成未量化部署。

## Sampling and seeds / 采样与种子

DA²C sends temperature and per-call output limits. Its recorded client does not explicitly send `top_p` or a sampling `seed`; server defaults apply to omitted parameters. Initial generation and revision use the configured sampling temperature; extraction, diagnosis, checking, evidence collection, arbitration and review use zero temperature. Each request can receive a smaller cap when the remaining budget is insufficient.

DA²C显式发送温度及逐次输出上限，已有客户端不显式发送`top_p`和采样`seed`，未发送参数由服务端处理，不把未记录的默认值补写为1。初始生成与修订使用配置温度；陈述抽取、诊断、核查、补查证据、仲裁和审核使用零温度。剩余预算不足时，实际输出额度会缩小。

Seeds identify the three repetitions and control client-side randomization: development sampling/splitting, shuffled cognitive schedules, and randomized action selection. The random router derives its local RNG from the experiment seed, question ID and round. MAPS additionally sends a request seed derived from its experiment seed and question/call key. Other baseline request settings are listed in [baselines](baselines.md). A client seed and a provider sampling seed are distinct settings.

种子标识三次重复，并控制客户端开发集抽样/划分、打乱认知操作顺序及随机路由。随机路由由实验种子、题目ID及轮次派生随机状态；MAPS还根据实验种子、题目与调用键派生请求种子。其他基线见[基线说明](baselines.md)。客户端种子与服务端采样种子分别记录。

The recorded CoT/SC/Best-of-N, MAD-M² and AgentPrune request configurations omit a provider seed. AgentPrune seeds development shuffling and graph sampling with client-side RNGs. BloomWise's archived request configuration also omits a provider seed, although its adapter accepts an optional seed argument. The final three-run protocol is author-confirmed; these request-field statements refer to the reused configurations, not a claim of deterministic token-by-token replication.

已记录的CoT/SC/Best-of-N、MAD-M²与AgentPrune请求不发送服务端种子。AgentPrune在客户端对开发数据打乱及图采样设置种子。BloomWise归档请求配置同样未发送服务端种子，适配器则支持可选seed参数。最终三轮协议由作者确认；此处区分复用的请求设置与客户端重复协议。

## GPT-5.4

Both references use the Responses API through DMXAPI, with the request alias `gpt-5.4`. Direct uses reasoning effort `none`; Reasoning uses `medium`. Both request only a final answer and use no collaboration framework, external tool or answer selector. Their exact instruction is in [gpt54.json](../prompts/gpt54.json). One answer is generated per question in each of the three runs. The gateway alias is the recorded request identifier, not a date-pinned model snapshot.

两种闭源参照均通过DMXAPI的Responses接口调用`gpt-5.4`。Direct的reasoning effort为`none`，Reasoning为`medium`，均要求仅返回最终答案，不使用协作框架、外部工具或选择器。三轮运行中，每题每轮各生成一个答案。模型别名是实际请求标识，不将其描述为带日期锁定的权重版本。

Calling dates are supplied in [gpt54_dates.json](../configs/gpt54_dates.json), in UTC and UTC+8. They are extracted from historical response timestamps; the author confirms that the final experiments used the same dates. Historical response counts describe the source archive, not the number of observations in the new three-run aggregate.

调用日期见[日期记录](../configs/gpt54_dates.json)，同时提供UTC和UTC+8。日期提取自旧响应时间戳，作者确认新实验日期相同。文件中的历史响应数量用于描述来源档案，不作为新三种子结果的样本数。

## Version identification / 版本标识

The 27B revision and runtime are pinned in JSON. The 8B repository is identified, but a weight revision was not present in the supplied deployment record; tokenizer hashes alone do not identify model weights. The executable release will include launchers and the recorded model/router identities. Credentials and private server addresses are excluded.

27B的revision和运行环境已固定于JSON。8B已记录仓库名，现有部署材料未给出权重revision，不能用tokenizer哈希替代权重版本。可执行版本发布时会包含启动文件及模型、路由器身份记录；不发布密钥或私有服务器地址。
