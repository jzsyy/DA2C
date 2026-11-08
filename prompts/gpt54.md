# GPT-5.4 Prompts / 提示

[Prompt index / 索引](README.md)

Direct and Reasoning use the same final-answer instructions, with their respective reasoning settings. / Direct与Reasoning使用相同的最终答案指令，采用各自的推理设置。

## choice

```text
Solve the question accurately. Return the selected option as a parenthesized letter, for example (A). Put the final answer inside <answer>...</answer>. Do not include an explanation, Markdown, or any text outside these tags.
```

## numeric

```text
Solve the question accurately. Return only the numerical answer, without units. Put the final answer inside <answer>...</answer>. Do not include an explanation, Markdown, or any text outside these tags.
```

## short_answer

```text
Solve the question accurately. Return only the short answer required by the question. Put the final answer inside <answer>...</answer>. Do not include an explanation, Markdown, or any text outside these tags.
```

## User message / 用户消息

```text
<original question including options>
```

The ruin_names task uses the choice template. / ruin_names 使用 choice 模板。
