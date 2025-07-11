# Dhanishta 2.0: Intermediate Thinking Guide 🧠

> **Revolutionary AI Reasoning**: The world's first AI model that shows its thinking process and can reconsider its reasoning multiple times within a single response.

## 🌟 What is Intermediate Thinking?

Intermediate thinking is a groundbreaking capability that allows Dhanishta 2.0 to:

- **Pause and Reflect**: Stop mid-response to think through problems
- **Show Its Work**: Display reasoning process in `<think>...</think>` blocks
- **Self-Correct**: Identify and fix logical errors in real-time
- **Multi-Phase Reasoning**: Approach complex problems from multiple angles
- **Iterative Refinement**: Improve answers through successive thinking cycles

## 🔍 How It Works

### The Thinking Process

When Dhanishta 2.0 encounters a complex problem, it doesn't just generate a response. Instead, it:

1. **Initial Analysis**: Breaks down the problem
2. **Reasoning Phase**: Works through the logic step-by-step
3. **Verification**: Checks its own work
4. **Refinement**: Adjusts reasoning if needed
5. **Final Response**: Provides the polished answer

### Thinking Blocks Explained

```
<think>
This is where Dhanishta 2.0 shows its internal reasoning process.
You can see exactly how it approaches problems, what it considers,
and how it arrives at conclusions.
</think>
```

## 🎯 Basic Example: Letter Counting

Let's see intermediate thinking in action with a simple example:

```python
from HelpingAI import HAI

hai = HAI()

response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{
        "role": "user", 
        "content": "How many letter 'r' are in the word 'strawberry'?"
    }],
    hide_think=False  # Show the thinking process
)

print(response.choices[0].message.content)
```

**Output:**
```
<think>
I need to count the letter 'r' in the word 'strawberry'.
Let me go through each letter carefully:

s-t-r-a-w-b-e-r-r-y

Position by position:
1. s - not an 'r'
2. t - not an 'r'  
3. r - this is an 'r' (first one)
4. a - not an 'r'
5. w - not an 'r'
6. b - not an 'r'
7. e - not an 'r'
8. r - this is an 'r' (second one)
9. r - this is an 'r' (third one)
10. y - not an 'r'

So I count 3 letter 'r's total.
</think>

The word "strawberry" contains **3** letter 'r's. You can see them at positions 3, 8, and 9: st**r**awbe**rr**y.
```

## 🧮 Complex Example: Mathematical Reasoning

Here's how Dhanishta 2.0 handles a more complex problem:

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{
        "role": "user",
        "content": "If it takes 5 machines 5 minutes to make 5 widgets, how long would it take 100 machines to make 100 widgets?"
    }],
    hide_think=False
)
```

**Output:**
```
<think>
This is a classic rate problem. Let me think through this step by step.

Given information:
- 5 machines make 5 widgets in 5 minutes

First, I need to figure out the rate per machine:
- If 5 machines make 5 widgets in 5 minutes
- That means each machine makes 1 widget in 5 minutes
- So the rate is: 1 widget per machine per 5 minutes

Now for the question: 100 machines making 100 widgets
- Each machine makes 1 widget in 5 minutes
- So 100 machines would each make 1 widget in 5 minutes
- That means 100 machines make 100 widgets in 5 minutes

Wait, let me double-check this reasoning...
- Rate per machine = 1 widget per 5 minutes
- 100 machines × 1 widget per machine = 100 widgets
- Time needed = 5 minutes (since each machine works in parallel)

Yes, that's correct.
</think>

The answer is **5 minutes**.

Here's the reasoning: If 5 machines make 5 widgets in 5 minutes, then each machine makes 1 widget in 5 minutes. When you have 100 machines, each one still takes 5 minutes to make 1 widget, but they work in parallel. So 100 machines make 100 widgets in the same 5 minutes.
```

## 🔄 Self-Correction in Action

One of Dhanishta 2.0's most impressive features is its ability to catch and correct its own mistakes:

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{
        "role": "user",
        "content": "What's 15% of 240?"
    }],
    hide_think=False
)
```

**Output:**
```
<think>
I need to calculate 15% of 240.

15% = 15/100 = 0.15

So 15% of 240 = 0.15 × 240

Let me calculate: 0.15 × 240
= 0.15 × 240
= 36

Wait, let me double-check this calculation:
15% of 240 = (15/100) × 240 = (15 × 240)/100 = 3600/100 = 36

Yes, that's correct.
</think>

15% of 240 is **36**.

To calculate this: 15% = 15/100 = 0.15, and 0.15 × 240 = 36.
```

## 🌍 Multilingual Thinking

Dhanishta 2.0 can think in multiple languages while maintaining reasoning quality:

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{
        "role": "user",
        "content": "¿Cuántas letras 'a' hay en la palabra 'banana'?"  # Spanish
    }],
    hide_think=False
)
```

**Output:**
```
<think>
Necesito contar las letras 'a' en la palabra 'banana'.

Vamos letra por letra:
b-a-n-a-n-a

1. b - no es 'a'
2. a - esta es una 'a' (primera)
3. n - no es 'a'
4. a - esta es una 'a' (segunda)
5. n - no es 'a'
6. a - esta es una 'a' (tercera)

Entonces hay 3 letras 'a' en 'banana'.
</think>

La palabra "banana" contiene **3** letras 'a'. Están en las posiciones 2, 4 y 6: b**a**n**a**n**a**.
```

## ⚙️ Controlling the Thinking Process

### Show or Hide Thinking

```python
# Show thinking process
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Your question"}],
    hide_think=False  # Shows <think> blocks
)

# Hide thinking process (cleaner output)
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Your question"}],
    hide_think=True   # Filters out <think> blocks
)
```

### Optimal Parameters for Thinking

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Complex problem here"}],
    temperature=0.7,      # Balanced creativity and logic
    max_tokens=2048,      # Allow space for thinking blocks
    top_p=0.9,           # Good nucleus sampling
    hide_think=False     # Show the reasoning process
)
```

## 🎯 Best Practices

### 1. **Allow Sufficient Tokens**
Thinking blocks consume tokens, so set `max_tokens` appropriately:
```python
max_tokens=2048  # For complex reasoning tasks
```

### 2. **Use Lower Temperature for Logic**
For mathematical or logical problems:
```python
temperature=0.3  # More deterministic reasoning
```

### 3. **Higher Temperature for Creative Thinking**
For creative problem-solving:
```python
temperature=0.8  # More creative approaches
```

### 4. **Encourage Step-by-Step Thinking**
```python
messages=[{
    "role": "user",
    "content": "Please solve this step by step: [your problem]"
}]
```

## 🚀 Advanced Applications

### Educational Content
Perfect for creating step-by-step tutorials and explanations.

### Research Analysis
Excellent for breaking down complex research problems.

### Code Debugging
Shows the logical process of finding and fixing bugs.

### Mathematical Proofs
Demonstrates mathematical reasoning step-by-step.

## 📚 Next Steps

- **[Advanced Reasoning Examples](advanced-reasoning.md)** - Complex problem-solving
- **[Self-Correction Guide](self-correction.md)** - How AI fixes its mistakes
- **[Multilingual Reasoning](multilingual.md)** - Thinking across languages
- **[Jupyter Notebooks](../../notebooks/dhanishta-2.0/)** - Interactive examples

---

**Experience the future of AI reasoning with Dhanishta 2.0! 🧠✨**
