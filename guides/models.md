# HelpingAI Models Overview 🤖

This guide provides comprehensive information about all HelpingAI models, their capabilities, and when to use each one.

## 🌟 Model Lineup

### Dhanishta 2.0 Preview - Revolutionary Reasoning AI
**Model ID:** `Dhanishtha-2.0-preview`

The world's first AI model with **intermediate thinking capabilities**. Dhanishta 2.0 can pause, reflect, and restart its reasoning process multiple times within a single response.

#### 🧠 Key Features
- **Intermediate Thinking**: Multiple `<think>...</think>` blocks throughout responses
- **Self-Correction**: Identifies and fixes logical inconsistencies mid-response
- **Multi-Phase Reasoning**: Complex problems solved through iterative thinking
- **Structured Emotional Reasoning (SER)**: `<ser>...</ser>` blocks for empathetic responses
- **Multilingual Capabilities**: 39+ languages with consistent reasoning quality
- **Transparent Process**: See exactly how the AI arrives at conclusions

#### 📊 Specifications
- **Base Model**: Qwen3-14B
- **Parameters**: 14.8B
- **Context Length**: 40,960 tokens
- **Languages**: 39+ (including English, Hindi, Chinese, Spanish, French, German, Japanese, Korean, Arabic)
- **Training**: 16.3 days on 8x NVIDIA H100 GPUs

#### 🎯 Best Use Cases
- Complex mathematical problems
- Logic puzzles and riddles
- Educational content requiring step-by-step explanations
- Multi-step reasoning tasks
- Research and analysis requiring multiple perspectives
- Programming problems with detailed solutions
- Scientific problem-solving

#### 📈 Benchmark Performance
| Benchmark | Score | Type |
|-----------|-------|------|
| MMLU | 78.1% | 1-shot |
| HumanEval | 75.0% | 1-shot |
| ARC | 76.0% | 1-shot |
| HellaSwag | 81.0% | 1-shot |
| TruthfulQA MC1 | 75.0% | 1-shot |
| Math 500 | 95.68% | few-shot |
| AIME 2024 | 82.81% | few-shot |

### HelpingAI3-raw - Advanced Emotional Intelligence
**Model ID:** `Helpingai3-raw`

An advanced language model with enhanced emotional intelligence and contextual awareness, specifically designed for empathetic and supportive interactions.

#### 💝 Key Features
- **Enhanced Emotional Understanding**: Deep comprehension of human emotions
- **Contextual Awareness**: Understands nuanced social and emotional contexts
- **Therapeutic Responses**: Trained on therapeutic conversation patterns
- **Crisis Response**: Specialized training for supportive crisis interactions
- **Cultural Sensitivity**: Awareness of diverse cultural contexts
- **Empathetic Communication**: Natural, caring response style

#### 📊 Training Data
- **15M Emotional Dialogues**: Comprehensive emotional conversation training
- **3M Therapeutic Exchanges**: Professional therapy session patterns
- **250K Cultural Conversations**: Diverse cultural interaction training
- **1M Crisis Response Scenarios**: Emergency and crisis support training

#### 🎯 Best Use Cases
- Emotional support and counseling
- Mental health assistance
- Personal advice and guidance
- Therapeutic conversations
- Crisis intervention support
- Empathetic customer service
- Educational mentoring
- Creative writing with emotional depth

## 🔄 Model Comparison

| Feature | Dhanishta 2.0 | HelpingAI3-raw |
|---------|---------------|----------------|
| **Primary Strength** | Reasoning & Problem Solving | Emotional Intelligence |
| **Thinking Process** | Visible with `<think>` blocks | Internal processing |
| **Self-Correction** | ✅ Real-time error fixing | ❌ Single-pass responses |
| **Multilingual** | ✅ 39+ languages | ✅ Multiple languages |
| **Mathematical Reasoning** | ⭐⭐⭐⭐⭐ Excellent | ⭐⭐⭐ Good |
| **Emotional Understanding** | ⭐⭐⭐ Good | ⭐⭐⭐⭐⭐ Excellent |
| **Creative Writing** | ⭐⭐⭐⭐ Very Good | ⭐⭐⭐⭐⭐ Excellent |
| **Educational Content** | ⭐⭐⭐⭐⭐ Excellent | ⭐⭐⭐⭐ Very Good |
| **Therapeutic Use** | ⭐⭐⭐ Good | ⭐⭐⭐⭐⭐ Excellent |

## 🎯 Choosing the Right Model

### Use Dhanishta 2.0 When You Need:

✅ **Complex Problem Solving**
```python
# Perfect for multi-step reasoning
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Solve this step by step: If a train travels 120 km in 2 hours, and then 180 km in 3 hours, what's the average speed?"}],
    hide_think=False
)
```

✅ **Educational Explanations**
```python
# Great for teaching concepts
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Explain photosynthesis step by step"}]
)
```

✅ **Logic and Reasoning Tasks**
```python
# Excellent for puzzles and logic
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Three friends have different colored hats. If Alice doesn't have red, Bob doesn't have blue, and Charlie doesn't have green, and there are only red, blue, and green hats, who has which color?"}]
)
```

### Use HelpingAI3-raw When You Need:

✅ **Emotional Support**
```python
# Perfect for empathetic responses
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{"role": "user", "content": "I'm feeling overwhelmed with work and personal life. I don't know how to cope."}]
)
```

✅ **Creative Writing**
```python
# Excellent for emotional storytelling
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{"role": "user", "content": "Write a heartwarming story about friendship"}]
)
```

✅ **Personal Advice**
```python
# Great for life guidance
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{"role": "user", "content": "How can I improve my relationships with my family?"}]
)
```

## 🔧 Model-Specific Parameters

### Dhanishta 2.0 Special Parameters

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Your question here"}],
    hide_think=False,  # Show/hide thinking process
    temperature=0.7,   # Lower for logical tasks, higher for creative
    max_tokens=2048    # Allow space for thinking blocks
)
```

### HelpingAI3-raw Optimal Settings

```python
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{"role": "user", "content": "Your question here"}],
    temperature=0.8,   # Higher for more empathetic responses
    top_p=0.9,        # Good balance for emotional nuance
    max_tokens=1000   # Sufficient for detailed emotional responses
)
```

## 🌍 Language Support

### Dhanishta 2.0 Supported Languages (39+)
- **European**: English, Spanish, French, German, Italian, Portuguese, Dutch, Russian, Polish, Czech, etc.
- **Asian**: Chinese (Simplified/Traditional), Japanese, Korean, Hindi, Arabic, Thai, Vietnamese, etc.
- **Others**: Hebrew, Turkish, Greek, Swedish, Norwegian, Finnish, and many more

### Language-Specific Reasoning
```python
# Reasoning in multiple languages
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Explique-moi la photosynthèse étape par étape"}],  # French
    hide_think=False
)
```

## 📚 Next Steps

- **[Dhanishta 2.0 Deep Dive](dhanishta-2.0/intermediate-thinking.md)** - Learn about intermediate thinking
- **[Basic Examples](../notebooks/basic/)** - Start with simple examples
- **[Advanced Applications](../examples/applications/)** - Real-world use cases
- **[API Reference](../reference/api.md)** - Complete technical documentation

---

**Choose the right model for your use case and unlock the full potential of AI! 🚀**
