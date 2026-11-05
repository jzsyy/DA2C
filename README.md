# DA²C

## Disagreement-Aware Adaptive Collaboration for Multi-Agent LLM Reasoning

[English](README.md) | [简体中文](README.zh-CN.md)

DA²C organizes multi-agent reasoning around two questions: **what needs to be checked, and which collaboration action is worth its cost?** It combines claim-level adaptive diagnosis with gain-and-cost-aware action selection, turning candidate answers into a feedback-driven reasoning process.

## Framework

![DA²C framework: independent candidates, adaptive diagnosis, and cost-aware collaboration](assets/framework.png)

1. **Independent candidates.** Two reasoners independently read the task and generate candidate answers with concise rationales. Answer normalization and voting initialize the reasoning state.
2. **Adaptive diagnosis.** A diagnostic agent identifies claims to inspect and uses Bloom-guided cognitive operations to construct targeted questions. Answer agreement alone does not bypass diagnosis.
3. **Scoped checking.** Checks distinguish restricted calculation, source quotation, and model judgment, recording both the evidence source and the verdict.
4. **Cost-aware collaboration.** A development-calibrated Ridge router compares expected correctness gains and token costs. It selects an available action: recheck evidence, verify a step, targeted repair, arbitrate among existing candidates, or stop.
5. **Review and update.** Proposed revisions undergo acceptance review. The updated state is diagnosed again until a stopping condition or a budget/round limit is reached. The system returns an answer and a process trace.

## Experimental Documentation

The reported model settings, module token caps, calibration protocol, and baseline/ablation definitions are documented separately:

- [Experimental settings (English)](docs/experiments.md)
- [实验设置说明（简体中文）](docs/experiments.zh-CN.md)

## Release Status and Schedule

**Current status:** this documentation package contains the bilingual project overview, framework figure, and experimental settings. The implementation and reproduction scripts have not yet been released.

| Stage | Availability |
| --- | --- |
| Project overview | Available now: English and Chinese READMEs and the framework figure |
| Experimental documentation | English and Chinese descriptions of the reported protocol |
| Implementation release | Planned upon paper acceptance |
| Reproduction materials | Planned with the implementation release |

The schedule is tied to paper acceptance; no calendar release date has been set. This README will be updated when the release becomes available.

## Planned Release Contents

- **Implementation code:** independent reasoning, adaptive diagnostic question chains, scoped checking, action routing, and revision review.
- **Experiment configurations:** model and task settings, router calibration settings, baseline/adaptation settings, and ablation configurations.
- **Run scripts:** instructions and scripts for executing the supported experimental workflows.
- **Evaluation scripts:** accuracy, answer correction and damage, token usage, routing analysis, and result aggregation.

The release will document method adaptations, including **MAPS (text-adapted)** rather than an exact multimodal reproduction. Model weights and third-party datasets remain subject to their original access conditions and licenses. Private credentials and local deployment secrets will not be distributed.
