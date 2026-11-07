# Experimental protocol

[English](experiments.md) | [简体中文](experiments.zh-CN.md) | [Project](../README.md)

This package documents the reported A100 experiments with seeds 42/43/44. Specifications, source-extracted prompts and aggregate measurements are available now; implementation and reproduction scripts are planned upon acceptance.

## Reading order

| Topic | Authoritative location |
| --- | --- |
| Tasks, counts and input identities | [Tasks](tasks.md), [JSON](../configs/tasks.json) |
| Backbones, sampling, output caps and GPT requests | [Runtime](runtime.md), [JSON](../configs/runtime.json), [dates](../configs/gpt54_dates.json) |
| Baseline settings and adaptations | [Baselines](baselines.md) |
| Calibration, features and routing controls | [Router](router.md), [feature dictionary](features.md) |
| Actual instructions | [Prompts](../prompts/README.md) |
| Answer normalization, aggregation and token scope | [Scoring](scoring.md) |
| Five reported aggregate tables | [Results](../results/README.md) |
| Availability, licenses and snapshot | [Release](release.md) |

## Controlled ablations

Variants share the same questions, backbone, scoring and frozen initial candidates for each question and seed. Full initial-generation cost is charged to every variant. Each variant re-collects action outcomes and refits coefficients on the same development partitions while keeping the full method's selected alpha/lambda. Model weights remain frozen. Common limits and generation settings are defined in [runtime.json](../configs/runtime.json).

| Variant | Changed mechanism |
| --- | --- |
| Generic question chain | Retain claim targeting and one question at a time; remove cognitive categories, ordering and operation descriptions. |
| Fixed cognitive order | Follow Remember→Understand→Apply→Analyze→Evaluate→Create; at most three questions with normal early stopping. |
| Shuffled question chain | Seed-permute the operation schedule, not generated question text; generate each next question from the current state. |
| w/o Bloom labels | Neutral category IDs; retain functional descriptions, category features and adaptive selection. |
| w/o claim-level diagnosis | Remove diagnosis and its pre-check; target the first complete candidate matching the current answer. Rechecking revisits original conditions; repair revisits the whole candidate. Disable verify_step; retain arbitration, stopping, arithmetic screening and review. |
| w/o diagnostic checking | Retain diagnosis and target localization; mark omitted check features missing. Retain arithmetic screening and acceptance review. |
| w/o acceptance gate | Retain diagnostic checks and arithmetic screening; directly commit format-valid revisions not blocked by the screen. |
| w/o re-diagnosis | Reuse the initial diagnostic/check record; stop if the target disappears or an action repeats without new evidence. |

Missing information is encoded consistently during variant calibration and testing, not represented as supported evidence. Removing claim-level diagnosis changes both action information and available actions. Fixed order retains the three-question limit.

All tables report three-run means. Main-table Overall uses task-macro accuracy; ablation/routing/budget results pool all 3,975 questions. Exact definitions are in [scoring](scoring.md). This package contains no mock experiment values.
