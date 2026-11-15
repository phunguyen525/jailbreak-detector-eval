# How the labels were made

All 2,356 sampled prompts were relabelled with Claude Opus 5.5 and a written rubric based on the definition and strategies in Shen et al. Like a detector, Opus saw only the prompt text, not the original tags or the detector scores. The author also labelled 217 prompts with the same rubric.

## Jailbreak labels (`data/labels.csv`)

The sample contains all 1,356 jailbreak-tagged prompts and 1,000 regular prompts drawn at random (seed 7). It is shuffled (seed 11) and numbered `P00001` to `P02356`.

Claude Opus 5.5 labelled every prompt with [`rubric_jailbreak.md`](rubric_jailbreak.md), through the Message Batches API with effort `high` and JSON-schema output.

The author labelled 217 prompts:
- 100 random prompts (50 per tag), without seeing the model's labels. The two agreed on 97.8% of the prompts they both labelled (91 of 93), with a Cohen's κ of 0.956 (95% CI 0.890–1.000).
- 117 prompts that the model declined or was unsure about. Opus declined 78 and marked 13 as unsure. For the other 26, a second model (Claude Sonnet 5.5) gave a different label, so we also counted Opus as unsure about them. Sonnet's labels were used only to pick these prompts.

The sample has 1,251 jailbreaks and 1,105 non-jailbreaks.

| Column | Meaning |
|---|---|
| `pid` | prompt ID in the sample |
| `uid` | row number in the deduplicated dataset |
| `orig` | original tag (1 jailbreak, 0 regular) |
| `source` | who decided the final label |
| `final` | our label (yes or no) |
| `words` | prompt length in words (split on whitespace), used in notebook 04 |

## Technique labels (`data/techniques.csv`)

Each of the 1,251 jailbreaks is annotated with the manual techniques of NIST AI 100-2e2025 (section 3.3.1), using [`rubric_techniques.md`](rubric_techniques.md). A prompt can use several. The four competing-objectives techniques are role-play, refusal suppression, style injection and prefix injection. The four mismatched-generalization techniques are special encoding, character transformation, word transformation and prompt-level transformation. The rubric also allows `other`.

The rubric adds three conventions. Fiction and simulations count as role-play. Ignoring earlier rules, new rule sets, ethical exceptions, threats or rewards count as refusal suppression. A fixed opening tag counts as prefix injection.

Opus labelled 1,155 jailbreaks, and the author labelled the 96 that the model declined and another 181. For 121 of these 181, the author's labels are final. The other 60 were drawn at random as a check and keep Opus's labels.

| `source` | Prompts | Final label from |
|---|---|---|
| `opus_api` | 1,034 | Opus |
| `author_p1` | 121 | the author, blind to Opus |
| `author_p2` | 96 | the author (Opus declined) |

On the 181 prompts labelled by both, agreement on the four competing-objectives techniques was approximately 90%. The two gave the same labels for all four techniques on 165 of them (91.2%). Mismatched-generalization techniques are too rare to assess.
