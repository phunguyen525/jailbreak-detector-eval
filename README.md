# Do Open-Source Detectors Catch In-the-Wild Jailbreak Prompts?

Phu Nguyen, University of New Brunswick

Code, labels and detector scores for the report of the same name ([`report.pdf`](report.pdf)). The report evaluates four open-source detectors (ProtectAI v2, Prompt Guard 2 22M and 86M, and PIGuard) on the in-the-wild jailbreak prompts of Shen et al. (CCS 2024), and annotates each jailbreak with the manual jailbreak techniques of NIST AI 100-2e2025.

## Main findings

- On the original prompts, ProtectAI and PIGuard perform best (AUC 0.954 and 0.944). ProtectAI and Prompt Guard 2 22M miss many jailbreaks that do not explicitly tell the model to stop refusing or how to begin its answer.
- Every detector loses AUC after the prompts are encoded (Base64, ROT13), rewritten (Pig Latin) or translated (Zulu).
- On Base64 and ROT13, the Prompt Guard 2 models flag few prompts, while ProtectAI and PIGuard flag almost all of them whether they are benign or not.

## Contents

| Path | Content |
|---|---|
| `data/labels.csv` | the 2,356 sampled prompts: original tag, our label, who decided it, prompt length in words |
| `data/techniques.csv` | NIST technique labels for the 1,251 jailbreaks |
| `data/scores_rq1.csv` | detector scores for all 14,478 prompts in their original form |
| `data/scores_rq2.csv` | detector scores for the 2,356 sampled prompts in all seven forms |
| `labeling/` | the two rubrics and how the labels were made |
| `notebooks/` | the seven notebooks listed below |
| `results/` | the tables and figure made by notebooks 04 and 07 |

The dataset is release `2023_12_25` of [`TrustAIRLab/in-the-wild-jailbreak-prompts`](https://huggingface.co/datasets/TrustAIRLab/in-the-wild-jailbreak-prompts). `uid` is the row number in the deduplicated dataset, and `pid` is the prompt ID in the sample. In `scores_rq2.csv`, each column is named `<form>__<detector>`. The forms are `orig`, `b64_wrap`, `b64_raw`, `rot13_wrap`, `rot13_raw`, `piglatin` and `zulu`.

A prompt's score is the highest score over its 510-token chunks, and a prompt counts as flagged at 0.5. Metrics weight each sampled regular prompt by 13.122 (13,122 regular prompts / 1,000 sampled), so that they describe the full dataset.

## Reproducing

| Notebook | Content |
|---|---|
| `01_score_detectors` | deduplicate the dataset, score all prompts |
| `02_label_jailbreaks_opus` | build the sample, Opus jailbreak labels |
| `03_label_techniques_opus` | Opus technique labels |
| `04_analyze_rq1` | RQ1 tables and figure |
| `05_translate_zulu` | Zulu translation with NLLB-200 |
| `06_score_rq2` | score the six transformed forms |
| `07_analyze_rq2` | RQ2 table |

The notebooks were run on Google Colab. Notebooks 04 and 07 rebuild the tables and figure in `results/` from the files in `data/`. Notebooks 02 and 03 need an Anthropic API key, and Prompt Guard 2 needs a Hugging Face token.

Prompt texts are not included because many of them are working jailbreaks. Notebooks 01, 05 and 06 regenerate them from the public dataset.

## Reference

X. Shen, Z. Chen, M. Backes, Y. Shen, and Y. Zhang. "Do Anything Now": Characterizing and Evaluating In-The-Wild Jailbreak Prompts on Large Language Models. In ACM SIGSAC Conference on Computer and Communications Security (CCS), 2024.
