# Experimental Overview

[English](experiments.md) | [简体中文](experiments.zh-CN.md) | [Project](../README.md)

## Evaluation

Experiments cover 13 tasks and 3,975 questions from BBH, GSM8K, and MMLU, using Qwen3-8B and Qwen3.8-27B on A100 GPUs. Reported values average three runs with seeds 42, 43, and 44.

Comparisons include CoT, Self-Consistency, Best-of-N, BloomWise, MAD-M², AgentPrune, and MAPS (text-adapted). GPT-5.4 Direct and Reasoning provide closed-source references. BLES uses online adjacent-level agreement stopping; BLM aggregates six levels. MAPS retains personality-guided collaboration and feedback for text tasks.

## Analysis

- **Components:** diagnostic question chains, verification feedback, acceptance review, and re-diagnosis.
- **Routing:** learned gain-and-cost decisions versus random, fixed, rule-based, and gain-only selection.
- **Budget:** accuracy and online usage at 4,000, 8,000, and 16,000-token scheduling targets.
- **Backbones:** separate router calibration for each backbone.

Ablations share frozen initial candidates and include their full generation cost. Each variant refits its router on the same development partitions using the full method's selected hyperparameters.

## Measurement

The main comparison reports task-macro accuracy; ablation, routing, and budget analyses use micro accuracy over all questions. Online usage includes input and output tokens for generation, collaboration, selection, and retries. Main comparisons report each method's operating point; budget scans evaluate DA²C.

[Key settings](runtime.md)
