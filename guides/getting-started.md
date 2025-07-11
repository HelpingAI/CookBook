# Getting Started with HelpingAI 🚀

Welcome to HelpingAI! This guide will help you get up and running with our powerful AI models, including the revolutionary **Dhanishta 2.0** with intermediate thinking capabilities.

## 📋 Prerequisites

- Python 3.7 or higher
- An internet connection
- A HelpingAI API key (we'll show you how to get one)

## 🔧 Installation

### Install the HelpingAI Python SDK

```bash
pip install HelpingAI
```

### Verify Installation

```python
import HelpingAI
print(f"HelpingAI version: {HelpingAI.__version__}")
```

## 🔑 Authentication

### Getting Your API Key

1. Visit [HelpingAI Dashboard](https://helpingai.co/dashboard)
2. Sign up or log in to your account
3. Navigate to the API Keys section
4. Generate a new API key
5. Copy and securely store your key

### Setting Up Authentication

#### Method 1: Environment Variable (Recommended)

```bash
# On Windows
set HAI_API_KEY=your-api-key-here

# On macOS/Linux
export HAI_API_KEY=your-api-key-here
```

#### Method 2: Direct in Code

```python
from HelpingAI import HAI

hai = HAI(api_key="your-api-key-here")
```

#### Method 3: Configuration File

Create a `.env` file in your project root:

```env
HAI_API_KEY=your-api-key-here
```

## 🎯 Your First API Call

### Basic Chat Completion

```python
from HelpingAI import HAI

# Initialize the client
hai = HAI()

# Create a simple chat completion
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[
        {"role": "user", "content": "Hello! Can you help me understand emotional intelligence?"}
    ]
)

# Print the AI's response
print(response.choices[0].message.content)
```

### Experience Dhanishta 2.0's Thinking Process

```python
# See how AI thinks step by step
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[
        {"role": "user", "content": "How many letter 'r' are in the word 'strawberry'?"}
    ],
    hide_think=False  # Show the thinking process!
)

print(response.choices[0].message.content)
```

**Expected Output:**
```
<think>
I need to count the letter 'r' in the word 'strawberry'.
Let me go through each letter:
s-t-r-a-w-b-e-r-r-y

Looking at each position:
1. s - not an 'r'
2. t - not an 'r'
3. r - this is an 'r' (1st one)
4. a - not an 'r'
5. w - not an 'r'
6. b - not an 'r'
7. e - not an 'r'
8. r - this is an 'r' (2nd one)
9. r - this is an 'r' (3rd one)
10. y - not an 'r'

So I count 3 letter 'r's in 'strawberry'.
</think>

The word "strawberry" contains 3 letter 'r's. You can see them in positions 3, 8, and 9: st**r**awbe**rr**y.
```

## 🤖 Understanding Models

### Available Models

```python
# List all available models
models = hai.models.list()
for model in models:
    print(f"Model ID: {model.id}")
    print(f"Description: {model.description}")
    print("---")
```

### Model Comparison

| Model | Best For | Key Features |
|-------|----------|--------------|
| **Dhanishtha-2.0-preview** | Complex reasoning, problem-solving | Intermediate thinking, self-correction, 39+ languages |
| **Helpingai3-raw** | Emotional support, empathy | Enhanced emotional intelligence, therapeutic responses |

### Choosing the Right Model

**Use Dhanishta 2.0 when you need:**
- Step-by-step reasoning
- Complex problem solving
- Mathematical calculations
- Logical puzzles
- Educational explanations
- Multilingual reasoning

**Use HelpingAI3-raw when you need:**
- Emotional support
- Empathetic responses
- Therapeutic conversations
- Personal advice
- Creative writing with emotion

## 💬 Basic Chat Patterns

### Simple Conversation

```python
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[
        {"role": "system", "content": "You are a helpful AI assistant."},
        {"role": "user", "content": "What's the weather like today?"}
    ]
)
```

### Multi-turn Conversation

```python
messages = [
    {"role": "system", "content": "You are a helpful AI assistant."},
    {"role": "user", "content": "Tell me about quantum computing"},
    {"role": "assistant", "content": "Quantum computing is a revolutionary technology..."},
    {"role": "user", "content": "How does it differ from classical computing?"}
]

response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=messages
)
```

## 🌊 Streaming Responses

Get real-time responses as they're generated:

```python
# Stream responses in real-time
for chunk in hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{"role": "user", "content": "Tell me a story about friendship"}],
    stream=True
):
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

## ⚙️ Parameter Control

### Basic Parameters

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Write a creative story"}],
    
    # Creativity and randomness
    temperature=0.8,         # 0.0 = deterministic, 1.0 = very creative
    top_p=0.9,              # Nucleus sampling (0.1 = focused, 1.0 = diverse)
    
    # Response length
    max_tokens=500,         # Maximum tokens in response
    
    # Repetition control
    frequency_penalty=0.3,  # Reduce word repetition (-2.0 to 2.0)
    presence_penalty=0.3,   # Encourage topic diversity (-2.0 to 2.0)
    
    # Special features
    hide_think=True,        # Filter out reasoning blocks (Dhanishta 2.0 only)
)
```

### Parameter Guidelines

| Parameter | Range | Purpose | Recommended Values |
|-----------|-------|---------|-------------------|
| `temperature` | 0.0-1.0 | Controls creativity | 0.3 for factual, 0.7 for creative |
| `max_tokens` | 1-4096 | Response length | 500-1000 for conversations |
| `top_p` | 0.0-1.0 | Nucleus sampling | 0.9 for most cases |
| `frequency_penalty` | -2.0-2.0 | Reduces repetition | 0.3 for variety |
| `presence_penalty` | -2.0-2.0 | Encourages new topics | 0.3 for diversity |

## 🛠️ Error Handling

```python
from HelpingAI import HAI, HAIError, AuthenticationError, RateLimitError

hai = HAI()

try:
    response = hai.chat.completions.create(
        model="Dhanishtha-2.0-preview",
        messages=[{"role": "user", "content": "Hello!"}]
    )
    print(response.choices[0].message.content)
    
except AuthenticationError:
    print("Invalid API key. Please check your authentication.")
    
except RateLimitError:
    print("Rate limit exceeded. Please wait before making more requests.")
    
except HAIError as e:
    print(f"An error occurred: {e}")
```

## 🎯 Next Steps

Now that you're set up, explore these advanced topics:

1. **[Dhanishta 2.0 Intermediate Thinking](dhanishta-2.0/intermediate-thinking.md)** - Deep dive into AI reasoning
2. **[Advanced Examples](../notebooks/advanced/)** - Complex use cases and applications
3. **[Best Practices](../reference/best-practices.md)** - Production-ready guidelines
4. **[API Reference](../reference/api.md)** - Complete API documentation

## 🆘 Need Help?

- 📖 [FAQ](../reference/faq.md) - Common questions and answers
- 🐛 [Troubleshooting](../reference/troubleshooting.md) - Common issues and solutions
- 💬 [Discord Community](https://discord.gg/helpingai) - Chat with other developers
- 📧 [Support](mailto:support@helpingai.co) - Direct support

---

**Ready to build amazing AI applications? Let's go! 🚀**
