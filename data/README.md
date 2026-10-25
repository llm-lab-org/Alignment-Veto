# Data

The data lives on Hugging Face in two datasets.

| Dataset | What's inside | Access |
|---|---|---|
| [`PardisSzah/alignment-veto`](https://huggingface.co/datasets/PardisSzah/alignment-veto) | All 864 questions (tier, scale, source), and human survey answers for the 291 WVS-sourced questions | Public |
| [`PardisSzah/alignment-veto-responses`](https://huggingface.co/datasets/PardisSzah/alignment-veto-responses) | ~1.53M raw model responses from 26 LLMs, with refusal flags and normalized logits | Gated, request access |

The responses are gated because they contain unfiltered model outputs on sensitive topics (gender, sexuality, religion), some of which are offensive or misaligned.

## Running the code

Put the files in one folder and point the scripts at it:

```bash
export ALIGNMENT_VETO_ROOT=/path/to/data
# expected layout
# $ALIGNMENT_VETO_ROOT/tiers.csv                        questions (from PardisSzah/alignment-veto; see note below on the WVS-only subset)
# $ALIGNMENT_VETO_ROOT/new_weights_transposed.xlsx      human answers (from PardisSzah/alignment-veto; WVS-only subset for now)
# $ALIGNMENT_VETO_ROOT/MENA_TRANSLATED_reasoning/*.xlsx model responses (from the gated dataset)
# $ALIGNMENT_VETO_ROOT/results/                         outputs are written here
```

```python
from huggingface_hub import snapshot_download
snapshot_download("PardisSzah/alignment-veto", repo_type="dataset", local_dir="data")
# rename the WVS-only files to the names the code expects:
#   mv data/tiers_wvs_only.csv data/tiers.csv
#   mv data/new_weights_transposed_wvs_only.xlsx data/new_weights_transposed.xlsx
# model responses: after your access request is approved (run `huggingface-cli login` first)
snapshot_download("PardisSzah/alignment-veto-responses", repo_type="dataset", local_dir="data/_responses")
# then move the xlsx files:  mv data/_responses/data data/MENA_TRANSLATED_reasoning
```

## Human survey sources

The 864 questions come from two surveys: **291 from World Values Survey (WVS) Wave 7**, **573 from the Arab Opinion Index (AOI)** — `questions.csv`'s `source` column marks each one. All question texts are published.

For the human *answer* data, WVS's terms permit redistributing aggregates, so `human_answers.csv` / `tiers_wvs_only.csv` / `new_weights_transposed_wvs_only.xlsx` cover the 291 WVS questions now. AOI's terms for aggregate redistribution haven't been confirmed yet, so the AOI-sourced human answers (573 questions, all 16 countries) and their Arabic translations aren't published here — email the corresponding author (zahraei2@illinois.edu) to request them for research use while that's being sorted out.

Please cite [World Values Survey Wave 7](https://www.worldvaluessurvey.org) and the [Arab Opinion Index](https://arabcenterdc.org/) if you use this data.
