# Qwen2.5-Coder Fine-Tuning Notebook

This repository contains a Jupyter notebook for experimenting with QLoRA fine-tuning of the Qwen2.5-Coder 3B Instruct model. The notebook loads the model with Unsloth, adds LoRA adapters, reads a ChatML-style JSON dataset, runs supervised fine-tuning, and saves the resulting model files. It also includes an optional Hugging Face upload step.

The training notebook is the main entry point:

    qwen_fine_tuning.ipynb

## Before running

- Use a Python environment with a compatible PyTorch/CUDA setup, or run the notebook in a supported hosted notebook environment.
- The notebook installs its training libraries in its first cells.
- Provide your own `training_data.jsonl` in the notebook's working directory. The expected records contain chat messages with roles and content. This training file is not included in the repository.
- Review the model and dataset terms before training or sharing resulting weights.

Run the notebook from top to bottom. Training produces local output; the repository does not include a published fine-tuned checkpoint or verified benchmark results.

The included files under `sample_data/` are not the notebook's expected chat-training dataset.
