# Meltemi Predictability Extraction

Virtual environment for extracting predictability (surprisal / computational proxy for cloze probability) from Greek stimuli using Meltemi and other Hugging Face models.

Pre-requisites:

- hugging face account
- python installed

## Setup

Create and activate the virtual environment (unless you created one when setting up this project, i.e., via File > New Project, in which case skip to installing dependencies):

```bash
python3 -m venv meltemi-env
source meltemi-env/bin/activate      # Mac/Linux
meltemi-env\Scripts\activate         # Windows
```

Install dependencies:

```bash
pip install transformers huggingface_hub torch bitsandbytes accelerate
```

## Hugging Face access

1. Create a free account at huggingface.co
2. Accept the license/terms on the Meltemi model page (https://huggingface.co/ilsp/Meltemi-7B-v1.5)[https://huggingface.co/ilsp/Meltemi-7B-v1.5]
3. Generate an access token under Settings > Access Tokens
4. Log in from the terminal:

```bash
huggingface-cli login
```

Important: to 'copy' the token into the Terminal just right-click ONCE and hit Enter (don't use Ctrl+V).

## Downloading the model

The first time you load the model, weights are downloaded and cached locally (default location: `~/.cache/huggingface`). After that, everything runs offline.

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "ilsp/Meltemi-7B-v1.5"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name)
```

## Hardware notes

- Full precision: ~28GB RAM/VRAM
- 8-bit quantization: ~8 to 10GB
- 4-bit quantization: ~5 to 6GB, runs on CPU or a modest GPU

If local hardware is insufficient, consider Google Colab or university compute resources for the scoring pass.

## Alternative model for prototyping

`nlpaueb/bert-base-greek-uncased-v1` (GreekBERT) is much smaller (~110M parameters) and useful for testing the pipeline before running the full stimuli set through Meltemi.

## Scope of this environment

- Load Meltemi (or GreekBERT) locally
- Score Greek stimuli sentences at the critical word position
- Extract token-level probabilities and compute surprisal
- No text generation needed, scoring only

## Files

- `requirements.txt`: pinned package versions
- `score_stimuli.py`: scoring script (to be added)
- `stimuli/`: input sentence sets
- `output/`: extracted predictability/surprisal values