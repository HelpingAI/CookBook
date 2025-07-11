# HelpingAI Cookbook 🧠✨

[![HelpingAI](https://img.shields.io/badge/HelpingAI-Cookbook-blue)](https://helpingai.co)
[![Dhanishta 2.0](https://img.shields.io/badge/Dhanishta%202.0-World's%20First%20Intermediate%20Thinking%20AI-green)](https://huggingface.co/HelpingAI/Dhanishta-2.0-preview)
[![Python](https://img.shields.io/badge/Python-3.7%2B-blue)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> **The comprehensive guide to building with HelpingAI models - featuring the revolutionary Dhanishta 2.0 with intermediate thinking capabilities**

Welcome to the HelpingAI Cookbook! This repository contains examples, guides, and best practices for using HelpingAI's powerful AI models, including the groundbreaking **Dhanishta 2.0** - the world's first AI model with intermediate thinking capabilities.

## 🌟 What Makes This Special?

### 🚀 Dhanishta 2.0: Revolutionary AI Reasoning
- **World's First Intermediate Thinking Model**: See AI think, reconsider, and refine its reasoning in real-time
- **Transparent Reasoning**: `<think>...</think>` blocks show the AI's thought process
- **Self-Correction**: AI can identify and fix its own logical inconsistencies
- **Multi-Phase Reasoning**: Complex problems solved through iterative thinking
- **39+ Languages**: Multilingual reasoning with consistent quality

### 💝 HelpingAI3-raw: Advanced Emotional Intelligence
- **Enhanced Emotional Understanding**: Trained on 15M emotional dialogues
- **Therapeutic Exchanges**: 3M therapeutic conversations for empathetic responses
- **Crisis Response**: 1M crisis scenarios for supportive interactions
- **Cultural Awareness**: 250K cultural conversations for inclusive AI

## 🎯 Quick Start

### Installation

```bash
pip install HelpingAI
```

### Your First API Call

```python
from HelpingAI import HAI

# Initialize the client
hai = HAI()

# Experience Dhanishta 2.0's thinking process
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[
        {"role": "user", "content": "How many letter 'r' are in 'strawberry'?"}
    ],
    hide_think=False  # Show the thinking process!
)

print(response.choices[0].message.content)
```

## 📚 Table of Contents

### 🏁 Getting Started
- [**Quick Start Guide**](guides/getting-started.md) - Your first steps with HelpingAI
- [**Authentication & Setup**](guides/authentication.md) - API keys and configuration
- [**Model Overview**](guides/models.md) - Understanding all available models

### 🧠 Dhanishta 2.0 Guides
- [**Intermediate Thinking**](guides/dhanishta-2.0/intermediate-thinking.md) - Understanding AI reasoning
- [**Advanced Reasoning**](guides/dhanishta-2.0/advanced-reasoning.md) - Complex problem solving
- [**Self-Correction**](guides/dhanishta-2.0/self-correction.md) - How AI fixes its mistakes
- [**Multilingual Reasoning**](guides/dhanishta-2.0/multilingual.md) - Reasoning across languages

### 📓 Jupyter Notebooks

#### Basic Examples
- [**First API Call**](notebooks/basic/01-first-api-call.ipynb) - Hello World with HelpingAI
- [**Chat Completions**](notebooks/basic/02-chat-completions.ipynb) - Basic conversations
- [**Streaming Responses**](notebooks/basic/03-streaming.ipynb) - Real-time responses
- [**Parameter Tuning**](notebooks/basic/04-parameters.ipynb) - Controlling AI behavior

#### Dhanishta 2.0 Notebooks
- [**Thinking Process Demo**](notebooks/dhanishta-2.0/01-thinking-demo.ipynb) - See AI think step-by-step
- [**Complex Reasoning**](notebooks/dhanishta-2.0/02-complex-reasoning.ipynb) - Multi-step problem solving
- [**Self-Correction Examples**](notebooks/dhanishta-2.0/03-self-correction.ipynb) - AI fixing its mistakes
- [**Multilingual Thinking**](notebooks/dhanishta-2.0/04-multilingual.ipynb) - Reasoning in multiple languages

### 📖 Reference
- [**API Reference**](reference/api.md) - Complete API documentation
- [**Model Specifications**](reference/models.md) - Detailed model information
- [**Best Practices**](reference/best-practices.md) - Production-ready guidelines

## 🌟 Featured Examples

### 🧠 Dhanishta 2.0 Thinking in Action

```python
# Watch AI solve a logic puzzle step by step
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{
        "role": "user", 
        "content": "If it takes 5 machines 5 minutes to make 5 widgets, how long would it take 100 machines to make 100 widgets?"
    }],
    hide_think=False
)

# Output shows thinking process:
# <think>
# Let me think about this step by step...
# If 5 machines make 5 widgets in 5 minutes...
# That means each machine makes 1 widget in 5 minutes...
# So 100 machines would each make 1 widget in 5 minutes...
# Therefore 100 machines make 100 widgets in 5 minutes!
# </think>
```

### 💝 Emotional Intelligence with HelpingAI3

```python
# Get empathetic, supportive responses
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{
        "role": "user",
        "content": "I'm feeling overwhelmed with work and don't know how to cope."
    }]
)
# Receives thoughtful, emotionally intelligent response
```

## 🎨 Use Cases

| Use Case | Best Model | Key Features |
|----------|------------|--------------|
| **Complex Problem Solving** | Dhanishta 2.0 | Intermediate thinking, self-correction |
| **Educational Content** | Dhanishta 2.0 | Step-by-step reasoning, explanations |
| **Emotional Support** | HelpingAI3-raw | Empathy, therapeutic responses |
| **Creative Writing** | Both models | Creativity with reasoning or emotion |
| **Multilingual Tasks** | Dhanishta 2.0 | 39+ languages with consistent reasoning |

## 🚀 What's New

- **🆕 Dhanishta 2.0 Preview**: World's first intermediate thinking AI model
- **🧠 Transparent Reasoning**: See how AI thinks with `<think>` blocks
- **🔄 Self-Correction**: AI that can fix its own mistakes
- **🌍 Multilingual Reasoning**: Consistent thinking across 39+ languages
- **💝 Enhanced Emotional AI**: HelpingAI3 with deeper empathy

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- [HelpingAI Website](https://helpingai.co)
- [API Documentation](https://docs.helpingai.co)
- [Dhanishta 2.0 on Hugging Face](https://huggingface.co/HelpingAI/Dhanishta-2.0-preview)
- [Python SDK](https://pypi.org/project/HelpingAI/)
- [Discord Community](https://discord.gg/helpingai)

---

**Built with ❤️ by the HelpingAI Team**

*Empowering developers to build the future of AI applications*
