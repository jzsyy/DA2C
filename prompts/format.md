# Output Format / 输出格式

[Prompt index / 索引](README.md)

This function builds the output-format suffix for each request. / 此函数生成各请求使用的输出格式后缀。

<details>
<summary>Format suffix function / 格式后缀函数</summary>

```python
def output_contract(role: str, instructions: str) -> str:
    """Concrete field shapes supplement, rather than replace, method instructions."""
    shapes = {
        "solve": {"answer": "<final label or short answer>", "rationale": "<concise evidence>",
                  "confidence": 0.5, "claims": [{"text": "<claim>", "depends_on": [], "quote": ""}]},
        "direct": {"answer": "<final label or short answer>"},
        "check": {"status": "unknown", "finding": "<check result>"},
        "arbitrate": {"selected_candidate_id": "<existing candidate ID>", "justification": "<reason>"},
        "evidence": {"quotes": [], "finding": "<relevant conditions>"},
        "paths": {"paths": ["<solution method, not an answer>"]},
        "diagnose": {"relation": "insufficient", "kind": "unknown", "target_id": "<existing claim ID>",
                     "statement": "<named claim>", "question": "<one answer-blind check>",
                     "expression": "", "claimed_result": "", "quote": ""},
    }
    if role == "maps_solver":
        role = "solve"
    if role in {"maps_interpreter", "maps_aligner", "maps_scholar"}:
        shapes[role] = {"finding": "<stage-specific analysis, not the final answer>", "quotes": []}
    if role == "maps_critic":
        shapes[role] = {"scores": {s: 0.5 for s in ("interpreter", "aligner", "scholar", "solver")},
                        "feedback": {s: "<specific question or correction>" for s in ("interpreter", "aligner", "scholar", "solver")}}
    if role not in shapes:
        return ""
    shape = shapes[role]
    if role == "diagnose" and "return stage" in instructions:
        shape["stage"] = "<requested category>"
    contract = " Output shape (replace placeholders and illustrative values): " + json.dumps(shape)
    contract += " Use double-quoted JSON keys; no headings, markdown or text outside the object."
    if role in {"solve", "direct"}:
        contract += ' For choice, answer is the label alone, without option text. A calculator request instead uses {"tool_expression":"<expression>"}.'
    if role == "diagnose":
        contract += " Copy target_id exactly from a supplied claim, not a candidate ID. Leave unavailable optional checks empty; do not invent quotations."
    return contract
```

</details>
