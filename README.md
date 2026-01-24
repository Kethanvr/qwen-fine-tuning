# 🤖 Next.js & React TypeScript AI Assistant

A custom AI coding assistant finetuned on **Qwen2.5-Coder-3B** using QLoRA, specialized in Next.js, React, TypeScript, and modern web development.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Made with Unsloth](https://img.shields.io/badge/Made%20with-Unsloth-orange)](https://github.com/unslothai/unsloth)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Dataset](#dataset)
- [Training](#training)
- [Usage](#usage)
- [Installation](#installation)
- [Results](#results)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## 🎯 Overview

This project demonstrates **parameter-efficient finetuning** of a large language model (LLM) using **QLoRA** (Quantized Low-Rank Adaptation). The resulting model provides accurate, context-aware coding assistance specifically for:

- **Next.js 14+** App Router and Server Components
- **React 19** with modern Hooks
- **TypeScript** type patterns and best practices
- **Tailwind CSS** integration and styling

**Key Achievement:** Trained a production-quality model in **~40 minutes** using **free Google Colab resources** (T4 GPU).

---

## ✨ Features

- 🎯 **Specialized Knowledge**: Focused on modern web development stack
- ⚡ **Fast Training**: QLoRA enables training on free GPUs
- 💰 **Cost-Effective**: $0 training cost using Google Colab
- 📊 **Small Dataset**: Only 70 high-quality examples needed
- 🔧 **Parameter Efficient**: Trains only 1-10% of model parameters
- 🚀 **Production Ready**: Can be deployed to HuggingFace or used locally

---

## 🛠️ Tech Stack

### Model
- **Base Model**: Qwen2.5-Coder-3B-Instruct (3 billion parameters)
- **Quantization**: 4-bit using bitsandbytes
- **Finetuning Method**: QLoRA (Low-Rank Adaptation)

### Training Framework
- **Unsloth**: 2x faster training, 60% less memory usage
- **HuggingFace Transformers**: Model loading and tokenization
- **TRL (Transformer Reinforcement Learning)**: Training pipeline
- **PEFT**: Parameter-Efficient Fine-Tuning library

### Infrastructure
- **Platform**: Google Colab (Free Tier)
- **GPU**: NVIDIA Tesla T4 (15GB VRAM)
- **Training Time**: ~40 minutes
- **Memory Usage**: ~8GB peak

---

## 📊 Dataset

### Dataset Creation

1. **Source**: Generated using Gemini API (free tier)
2. **Format**: JSONL with ChatML structure
3. **Size**: 70 Q&A pairs
4. **Quality Focus**: Detailed, accurate, production-ready examples

### Topics Covered

- Next.js App Router & Architecture (14 examples)
- React Hooks & Patterns (13 examples)
- TypeScript with React (12 examples)
- Tailwind CSS Integration (6 examples)
- Common Errors & Debugging (25 examples)

### Data Format

```json
{
  "messages": [
    {
      "role": "system",
      "content": "You are a Next.js, React, and TypeScript expert assistant."
    },
    {
      "role": "user",
      "content": "How do I use useState in React with TypeScript?"
    },
    {
      "role": "assistant",
      "content": "You should define an interface for the object..."
    }
  ]
}
```

---

## 🚀 Training

### Hyperparameters

```python
# LoRA Configuration
r = 16                    # LoRA rank
lora_alpha = 16          # LoRA scaling
lora_dropout = 0         # Dropout (0 for speed)

# Training Configuration
max_steps = 200          # Training steps
learning_rate = 2e-4     # Learning rate
batch_size = 2           # Per-device batch size
gradient_accumulation = 4 # Effective batch size = 8
warmup_steps = 5         # LR warmup
max_seq_length = 2048    # Context window
```

### Training Process

```bash
# 1. Data Preparation
python prepare_data.py

# 2. Training
python train.py

# 3. Evaluation
python test_model.py
```

### Performance Metrics

- **Training Time**: 40 minutes
- **GPU Memory**: 8GB peak usage
- **Loss**: Converged smoothly
- **Inference Speed**: ~2-3 tokens/second on T4

---

## 💻 Usage

### Quick Start

```python
from unsloth import FastLanguageModel
from peft import PeftModel

# Load base model
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/Qwen2.5-Coder-3B-Instruct-bnb-4bit",
    max_seq_length=2048,
    dtype=None,
    load_in_4bit=True,
)

# Load finetuned adapters
model = PeftModel.from_pretrained(model, "path/to/model")

# Enable inference mode
FastLanguageModel.for_inference(model)

# Ask a question
messages = [
    {"role": "user", "content": "How do I use Server Components in Next.js?"}
]

inputs = tokenizer.apply_chat_template(
    messages,
    tokenize=True,
    add_generation_prompt=True,
    return_tensors="pt",
).to("cuda")

outputs = model.generate(
    input_ids=inputs,
    max_new_tokens=256,
    use_cache=True
)

print(tokenizer.decode(outputs[0]))
```

### Using from HuggingFace

```python
# If uploaded to HuggingFace
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="YOUR_USERNAME/nextjs-assistant",
    max_seq_length=2048,
    load_in_4bit=True,
)
```

---

## 🔧 Installation

### Prerequisites

- Python 3.8+
- CUDA-compatible GPU (for training/inference)
- Google Colab account (for free GPU access)

### Local Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/nextjs-ai-assistant.git
cd nextjs-ai-assistant

# Install dependencies
pip install -r requirements.txt

# Optional: Install Unsloth for faster training
pip install "unsloth[colab-new] @ git+https://github.com/unslothai/unsloth.git"
```

### Google Colab Setup

1. Open the notebook: `nextjs_assistant_training.ipynb`
2. Runtime → Change runtime type → T4 GPU
3. Run all cells
4. Upload your `training_data.jsonl` when prompted

---

## 📈 Results

### Model Performance

✅ **Strengths:**
- Accurate Next.js App Router patterns
- Proper TypeScript typing examples
- Context-aware React Hook explanations
- Tailwind CSS best practices

⚠️ **Limitations:**
- May need longer answers with more training steps
- Limited to training data scope
- Best for questions similar to training examples

### Example Outputs

**Question**: "How do I use useState in React with TypeScript?"

**Response**:
```typescript
interface User {
  id: number;
  name: string;
}

const [users, setUsers] = useState<User[]>([]);
```

---

## 📁 Project Structure

```
nextjs-ai-assistant/
│
├── data/
│   ├── training_data.jsonl         # Training dataset
│   └── prepare_data.py             # Data preparation script
│
├── notebooks/
│   └── training_notebook.ipynb     # Complete training notebook
│
├── scripts/
│   ├── train.py                    # Training script
│   ├── test_model.py               # Testing script
│   └── convert_to_jsonl.py         # Data conversion utility
│
├── model/
│   └── my_nextjs_assistant/        # Saved model (not in git)
│
├── requirements.txt                 # Python dependencies
├── README.md                        # This file
└── LICENSE                          # MIT License
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit your changes**: `git commit -m 'Add amazing feature'`
4. **Push to the branch**: `git push origin feature/amazing-feature`
5. **Open a Pull Request**

### Ideas for Contributions

- Expand the dataset with more examples
- Add support for other frameworks (Vue, Angular)
- Create a web UI using Gradio/Streamlit
- Implement RAG (Retrieval-Augmented Generation)
- Add evaluation metrics

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **[Unsloth](https://github.com/unslothai/unsloth)** - For making LLM training accessible and fast
- **[Alibaba Cloud](https://www.alibabacloud.com/)** - For the Qwen2.5-Coder model
- **[HuggingFace](https://huggingface.co/)** - For the transformers library and model hosting
- **[Google Colab](https://colab.research.google.com/)** - For free GPU access
- **Community** - For datasets, tutorials, and support

---

## 📚 Resources

### Learn More

- [Unsloth Documentation](https://github.com/unslothai/unsloth)
- [QLoRA Paper](https://arxiv.org/abs/2305.14314)
- [Qwen2.5-Coder Model Card](https://huggingface.co/Qwen/Qwen2.5-Coder-3B-Instruct)
- [HuggingFace PEFT](https://github.com/huggingface/peft)

### Related Projects

- [LLM Finetuning Resources](https://github.com/mlabonne/llm-course)
- [Awesome LLM](https://github.com/Hannibal046/Awesome-LLM)

---

## 📞 Contact

**Your Name** - [@kethan_vr](https://twitter.com/kethan_vr)

**Project Link**: https://huggingface.co/Kethanvr/my_nextjs_assistant

**Portfolio**: [kethanvr.me](https://kethanvr.me)

---

## ⭐ Star History

If you find this project useful, please consider giving it a star!

[![Star History Chart](https://api.star-history.com/svg?repos=YOUR_USERNAME/nextjs-ai-assistant&type=Date)](https://star-history.com/#YOUR_USERNAME/nextjs-ai-assistant&Date)

---

<div align="center">

**Made with ❤️ by [Kethan VR](https://github.com/Kethanvr)**

If this project helped you, consider [buying me a coffee](https://ko-fi.com/kethanvr) ☕

</div>
