# Data

The data lives on Hugging Face in two datasets.

| Dataset | What's inside | Access |
|---|---|---|
| [`PardisSzah/alignment-veto`](https://huggingface.co/datasets/PardisSzah/alignment-veto) | The 864 questions (tier, scale) and the human survey answers per country (mean + distribution) | Public |
| [`PardisSzah/alignment-veto-responses`](https://huggingface.co/datasets/PardisSzah/alignment-veto-responses) | ~1.53M raw model responses from 26 LLMs, with refusal flags and normalized logits | Gated, request access |

The responses are gated because they contain unfiltered model outputs on sensitive topics (gender, sexuality, religion), some of which are offensive or misaligned.

## Running the code

Put the files in one folder and point the scripts at it:

```bash
export ALIGNMENT_VETO_ROOT=/path/to/data
# expected layout
# $ALIGNMENT_VETO_ROOT/tiers.csv                        questions (from PardisSzah/alignment-veto)
# $ALIGNMENT_VETO_ROOT/new_weights_transposed.xlsx      human answers (from PardisSzah/alignment-veto)
# $ALIGNMENT_VETO_ROOT/MENA_TRANSLATED_reasoning/*.xlsx model responses (from the gated dataset)
# $ALIGNMENT_VETO_ROOT/results/                         outputs are written here
```

```python
from huggingface_hub import snapshot_download
snapshot_download("PardisSzah/alignment-veto", repo_type="dataset", local_dir="data")
# model responses: after your access request is approved (run `huggingface-cli login` first)
snapshot_download("PardisSzah/alignment-veto-responses", repo_type="dataset", local_dir="data/_responses")
# then move the xlsx files:  mv data/_responses/data data/MENA_TRANSLATED_reasoning
```

## Human survey sources

Human answers are country-level aggregates from the [World Values Survey Wave 7](https://www.worldvaluessurvey.org) and the [Arab Opinion Index](https://arabcenterdc.org/). Please cite both if you use them.
