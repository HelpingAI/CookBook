# HelpingAI API Reference 📚

Complete reference documentation for the HelpingAI Python SDK, including all methods, parameters, and response formats.

## 🏗️ Client Initialization

### HAI Class

```python
from HelpingAI import HAI

hai = HAI(
    api_key="your-api-key",           # Optional: reads from HAI_API_KEY env var
    organization="your-org-id",       # Optional: organization identifier
    base_url="https://api.helpingai.co/v1",  # Optional: custom base URL
    timeout=60.0                      # Optional: request timeout in seconds
)
```

#### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `api_key` | `str` | `None` | Your API key. If not provided, reads from `HAI_API_KEY` environment variable |
| `organization` | `str` | `None` | Optional organization ID for API requests |
| `base_url` | `str` | `"https://api.helpingai.co/v1"` | Base URL for API requests |
| `timeout` | `float` | `60.0` | Request timeout in seconds |

## 💬 Chat Completions

### Create Chat Completion

```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Hello!"}
    ],
    temperature=0.7,
    max_tokens=1000,
    top_p=0.9,
    frequency_penalty=0.0,
    presence_penalty=0.0,
    stop=None,
    stream=False,
    hide_think=True
)
```

#### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `model` | `str` | **Required** | Model ID to use (`Dhanishtha-2.0-preview` or `Helpingai3-raw`) |
| `messages` | `List[Dict]` | **Required** | List of message objects with `role` and `content` |
| `temperature` | `float` | `1.0` | Sampling temperature (0.0-1.0). Higher = more creative |
| `max_tokens` | `int` | `None` | Maximum tokens to generate |
| `top_p` | `float` | `1.0` | Nucleus sampling parameter (0.0-1.0) |
| `frequency_penalty` | `float` | `0.0` | Penalize frequent tokens (-2.0 to 2.0) |
| `presence_penalty` | `float` | `0.0` | Penalize repeated topics (-2.0 to 2.0) |
| `stop` | `str` or `List[str]` | `None` | Stop sequences to end generation |
| `stream` | `bool` | `False` | Whether to stream the response |
| `user` | `str` | `None` | User identifier for tracking |
| `n` | `int` | `1` | Number of completions to generate |
| `hide_think` | `bool` | `True` | Filter out `<think>` blocks (Dhanishta 2.0 only) |

#### Message Format

```python
messages = [
    {
        "role": "system",    # system, user, or assistant
        "content": "You are a helpful assistant."
    },
    {
        "role": "user",
        "content": "What is the capital of France?"
    },
    {
        "role": "assistant",
        "content": "The capital of France is Paris."
    }
]
```

#### Response Format

```python
{
    "id": "chatcmpl-123",
    "object": "chat.completion",
    "created": 1677652288,
    "model": "Dhanishtha-2.0-preview",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "The capital of France is Paris."
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 9,
        "completion_tokens": 12,
        "total_tokens": 21
    }
}
```

### Streaming Chat Completions

```python
for chunk in hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Tell me a story"}],
    stream=True
):
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

#### Streaming Response Format

```python
{
    "id": "chatcmpl-123",
    "object": "chat.completion.chunk",
    "created": 1677652288,
    "model": "Dhanishtha-2.0-preview",
    "choices": [
        {
            "index": 0,
            "delta": {
                "content": "Hello"
            },
            "finish_reason": null
        }
    ]
}
```

## 🤖 Models

### List Models

```python
models = hai.models.list()
for model in models:
    print(f"ID: {model.id}")
    print(f"Name: {model.name}")
    print(f"Description: {model.description}")
```

#### Response Format

```python
[
    {
        "id": "Dhanishtha-2.0-preview",
        "name": "Dhanishtha-2.0 Preview",
        "description": "World's first intermediate thinking model with multi-phase reasoning",
        "object": "model"
    },
    {
        "id": "Helpingai3-raw",
        "name": "HelpingAI3 Raw",
        "description": "Advanced language model with enhanced emotional intelligence",
        "object": "model"
    }
]
```

### Retrieve Model

```python
model = hai.models.retrieve("Dhanishtha-2.0-preview")
print(f"Model: {model.name}")
print(f"Description: {model.description}")
```

## 🎯 Model-Specific Features

### Dhanishta 2.0 - Intermediate Thinking

```python
# Show thinking process
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Solve: 15% of 240"}],
    hide_think=False  # Show <think> blocks
)

# Hide thinking process (clean output)
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Solve: 15% of 240"}],
    hide_think=True   # Filter out <think> blocks
)
```

#### Thinking Block Format

```
<think>
I need to calculate 15% of 240.
15% = 15/100 = 0.15
0.15 × 240 = 36
</think>

15% of 240 is 36.
```

### HelpingAI3-raw - Emotional Intelligence

```python
# Optimized for emotional responses
response = hai.chat.completions.create(
    model="Helpingai3-raw",
    messages=[{
        "role": "user", 
        "content": "I'm feeling overwhelmed. Can you help?"
    }],
    temperature=0.8,  # Higher for more empathetic responses
    max_tokens=800
)
```

## 🛠️ Error Handling

### Exception Types

```python
from HelpingAI import (
    HAIError,              # Base exception
    AuthenticationError,   # Invalid API key
    InvalidRequestError,   # Bad request
    RateLimitError,       # Rate limit exceeded
    ServiceUnavailableError,  # Service down
    APIConnectionError,   # Network issues
    TimeoutError         # Request timeout
)
```

### Error Handling Example

```python
try:
    response = hai.chat.completions.create(
        model="Dhanishtha-2.0-preview",
        messages=[{"role": "user", "content": "Hello!"}]
    )
    print(response.choices[0].message.content)

except AuthenticationError:
    print("Invalid API key")
except RateLimitError:
    print("Rate limit exceeded")
except InvalidRequestError as e:
    print(f"Invalid request: {e}")
except HAIError as e:
    print(f"API error: {e}")
```

### Error Response Format

```python
{
    "error": {
        "message": "Invalid API key provided",
        "type": "invalid_request_error",
        "code": "invalid_api_key"
    }
}
```

## 📊 Response Objects

### ChatCompletion

```python
@dataclass
class ChatCompletion:
    id: str
    object: str
    created: int
    model: str
    choices: List[Choice]
    usage: Optional[CompletionUsage]
```

### Choice

```python
@dataclass
class Choice:
    index: int
    message: ChatCompletionMessage
    finish_reason: str
```

### ChatCompletionMessage

```python
@dataclass
class ChatCompletionMessage:
    role: str
    content: str
    function_call: Optional[FunctionCall] = None
    tool_calls: Optional[List[ToolCall]] = None
```

### CompletionUsage

```python
@dataclass
class CompletionUsage:
    prompt_tokens: int
    completion_tokens: int
    total_tokens: int
```

## 🔧 Advanced Usage

### Custom Headers

```python
hai = HAI(
    api_key="your-key",
    organization="your-org"
)

# Headers are automatically set:
# Authorization: Bearer your-key
# HelpingAI-Organization: your-org
```

### Timeout Configuration

```python
# Global timeout
hai = HAI(timeout=30.0)

# Per-request timeout (not directly supported, use global)
```

### Base URL Override

```python
# For custom deployments
hai = HAI(base_url="https://your-custom-endpoint.com/v1")
```

## 📈 Best Practices

### Parameter Recommendations

| Use Case | Temperature | Max Tokens | Top P | Model |
|----------|-------------|------------|-------|-------|
| **Logical Reasoning** | 0.3 | 1500 | 0.9 | Dhanishta 2.0 |
| **Creative Writing** | 0.8 | 2000 | 0.9 | Both |
| **Emotional Support** | 0.7 | 1000 | 0.9 | HelpingAI3-raw |
| **Mathematical Problems** | 0.2 | 1500 | 0.8 | Dhanishta 2.0 |
| **Educational Content** | 0.5 | 1500 | 0.9 | Dhanishta 2.0 |

### Rate Limiting

- **Free Tier**: 100 requests/hour
- **Pro Tier**: 1000 requests/hour
- **Enterprise**: Custom limits

### Token Limits

- **Maximum Context**: 40,960 tokens (Dhanishta 2.0)
- **Recommended Max Tokens**: 2048 for thinking tasks
- **Minimum**: 1 token

---

**For more examples and advanced usage, see our [Cookbook Examples](../examples/)! 📚**
