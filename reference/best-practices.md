# HelpingAI Best Practices Guide 🎯

This guide covers best practices for building production-ready applications with HelpingAI models, including prompt engineering, parameter optimization, error handling, and deployment strategies.

## 🧠 Model Selection

### Choose the Right Model for Your Use Case

#### Dhanishta 2.0 - Best For:
- **Complex reasoning tasks** requiring step-by-step thinking
- **Educational applications** needing explanations
- **Mathematical and logical problems**
- **Multi-step problem solving**
- **Transparent decision-making processes**

#### HelpingAI3-raw - Best For:
- **Emotional support and counseling**
- **Creative writing and storytelling**
- **Personal advice and guidance**
- **Empathetic customer service**
- **Therapeutic conversations**

```python
# Example: Choose based on task type
def select_model(task_type):
    if task_type in ["math", "logic", "reasoning", "education"]:
        return "Dhanishtha-2.0-preview"
    elif task_type in ["emotional", "creative", "support", "therapy"]:
        return "Helpingai3-raw"
    else:
        return "Dhanishtha-2.0-preview"  # Default to reasoning model
```

## 📝 Prompt Engineering

### 1. Clear and Specific Instructions

**❌ Poor Prompt:**
```python
"Help me with math"
```

**✅ Good Prompt:**
```python
"Solve this algebra problem step by step: If 2x + 5 = 17, what is the value of x? Show your work."
```

### 2. Leverage Dhanishta 2.0's Thinking Process

**✅ Encourage Step-by-Step Thinking:**
```python
messages = [{
    "role": "user",
    "content": "Please solve this step by step, showing your reasoning: [problem]"
}]
```

**✅ Request Verification:**
```python
messages = [{
    "role": "user", 
    "content": "Solve this problem and then verify your answer: [problem]"
}]
```

### 3. System Messages for Context

**✅ Set Clear Context:**
```python
messages = [
    {
        "role": "system",
        "content": "You are an expert mathematics tutor. Always show your work step by step and explain your reasoning clearly."
    },
    {
        "role": "user",
        "content": "What is 15% of 240?"
    }
]
```

### 4. Multi-turn Conversations

**✅ Maintain Context:**
```python
conversation = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Explain photosynthesis"},
    {"role": "assistant", "content": "[Previous response]"},
    {"role": "user", "content": "How does this relate to climate change?"}
]
```

## ⚙️ Parameter Optimization

### Temperature Guidelines

| Use Case | Temperature | Reasoning |
|----------|-------------|-----------|
| **Mathematical Problems** | 0.2-0.3 | Need deterministic, accurate results |
| **Logical Reasoning** | 0.3-0.5 | Balance accuracy with flexibility |
| **Educational Content** | 0.4-0.6 | Clear but engaging explanations |
| **Creative Writing** | 0.7-0.9 | Encourage creativity and variety |
| **Emotional Support** | 0.6-0.8 | Warm, varied, empathetic responses |

```python
# Example: Dynamic temperature based on task
def get_optimal_temperature(task_type):
    temperature_map = {
        "math": 0.3,
        "logic": 0.4,
        "education": 0.5,
        "creative": 0.8,
        "emotional": 0.7
    }
    return temperature_map.get(task_type, 0.7)
```

### Token Management

**✅ Allow Sufficient Tokens for Thinking:**
```python
# For Dhanishta 2.0 with thinking process
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=messages,
    max_tokens=2048,  # Allow space for <think> blocks
    hide_think=False
)
```

**✅ Optimize for Production:**
```python
# For production with clean output
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=messages,
    max_tokens=1000,  # Smaller for efficiency
    hide_think=True   # Clean output
)
```

### Other Parameters

```python
# Optimal settings for different scenarios
REASONING_CONFIG = {
    "temperature": 0.3,
    "top_p": 0.9,
    "frequency_penalty": 0.1,
    "presence_penalty": 0.1,
    "max_tokens": 2048
}

CREATIVE_CONFIG = {
    "temperature": 0.8,
    "top_p": 0.9,
    "frequency_penalty": 0.3,
    "presence_penalty": 0.3,
    "max_tokens": 1500
}

EMOTIONAL_CONFIG = {
    "temperature": 0.7,
    "top_p": 0.9,
    "frequency_penalty": 0.2,
    "presence_penalty": 0.2,
    "max_tokens": 1000
}
```

## 🛡️ Error Handling

### Comprehensive Error Handling

```python
from HelpingAI import HAI, HAIError, AuthenticationError, RateLimitError
import time
import logging

class RobustHAIClient:
    def __init__(self, max_retries=3, retry_delay=1.0):
        self.hai = HAI()
        self.max_retries = max_retries
        self.retry_delay = retry_delay
        self.logger = logging.getLogger(__name__)
    
    def create_completion_with_retry(self, **kwargs):
        """Create completion with automatic retry logic"""
        for attempt in range(self.max_retries):
            try:
                return self.hai.chat.completions.create(**kwargs)
            
            except RateLimitError:
                if attempt < self.max_retries - 1:
                    wait_time = self.retry_delay * (2 ** attempt)  # Exponential backoff
                    self.logger.warning(f"Rate limited. Waiting {wait_time}s before retry {attempt + 1}")
                    time.sleep(wait_time)
                else:
                    raise
            
            except AuthenticationError:
                self.logger.error("Authentication failed. Check API key.")
                raise
            
            except HAIError as e:
                if attempt < self.max_retries - 1:
                    self.logger.warning(f"API error: {e}. Retrying...")
                    time.sleep(self.retry_delay)
                else:
                    raise
        
        raise Exception("Max retries exceeded")
```

### Input Validation

```python
def validate_input(messages, model, max_tokens=None):
    """Validate input parameters before API call"""
    
    # Validate messages
    if not messages or not isinstance(messages, list):
        raise ValueError("Messages must be a non-empty list")
    
    for msg in messages:
        if not isinstance(msg, dict) or 'role' not in msg or 'content' not in msg:
            raise ValueError("Each message must have 'role' and 'content'")
        
        if msg['role'] not in ['system', 'user', 'assistant']:
            raise ValueError("Role must be 'system', 'user', or 'assistant'")
    
    # Validate model
    valid_models = ["Dhanishtha-2.0-preview", "Helpingai3-raw"]
    if model not in valid_models:
        raise ValueError(f"Model must be one of: {valid_models}")
    
    # Validate token limits
    if max_tokens and max_tokens > 4096:
        raise ValueError("max_tokens cannot exceed 4096")
    
    return True
```

## 🚀 Production Deployment

### Environment Configuration

```python
import os
from dataclasses import dataclass

@dataclass
class HAIConfig:
    api_key: str
    base_url: str = "https://api.helpingai.co/v1"
    timeout: float = 60.0
    max_retries: int = 3
    retry_delay: float = 1.0
    
    @classmethod
    def from_env(cls):
        return cls(
            api_key=os.getenv("HAI_API_KEY"),
            base_url=os.getenv("HAI_BASE_URL", "https://api.helpingai.co/v1"),
            timeout=float(os.getenv("HAI_TIMEOUT", "60.0")),
            max_retries=int(os.getenv("HAI_MAX_RETRIES", "3")),
            retry_delay=float(os.getenv("HAI_RETRY_DELAY", "1.0"))
        )
```

### Caching Strategies

```python
import hashlib
import json
from functools import lru_cache

class CachedHAIClient:
    def __init__(self, cache_size=1000):
        self.hai = HAI()
        self.cache = {}
        self.cache_size = cache_size
    
    def _generate_cache_key(self, **kwargs):
        """Generate cache key from request parameters"""
        # Remove non-deterministic parameters
        cache_params = {k: v for k, v in kwargs.items() 
                       if k not in ['stream', 'user']}
        return hashlib.md5(json.dumps(cache_params, sort_keys=True).encode()).hexdigest()
    
    def create_completion(self, use_cache=True, **kwargs):
        """Create completion with optional caching"""
        if not use_cache or kwargs.get('stream', False):
            return self.hai.chat.completions.create(**kwargs)
        
        cache_key = self._generate_cache_key(**kwargs)
        
        if cache_key in self.cache:
            return self.cache[cache_key]
        
        response = self.hai.chat.completions.create(**kwargs)
        
        # Manage cache size
        if len(self.cache) >= self.cache_size:
            # Remove oldest entry (simple FIFO)
            oldest_key = next(iter(self.cache))
            del self.cache[oldest_key]
        
        self.cache[cache_key] = response
        return response
```

### Monitoring and Logging

```python
import logging
import time
from functools import wraps

def monitor_api_calls(func):
    """Decorator to monitor API call performance"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        logger = logging.getLogger(__name__)
        
        try:
            result = func(*args, **kwargs)
            duration = time.time() - start_time
            
            # Log successful calls
            logger.info(f"API call successful - Duration: {duration:.2f}s")
            
            # Log token usage if available
            if hasattr(result, 'usage') and result.usage:
                logger.info(f"Tokens used: {result.usage.total_tokens}")
            
            return result
            
        except Exception as e:
            duration = time.time() - start_time
            logger.error(f"API call failed - Duration: {duration:.2f}s - Error: {e}")
            raise
    
    return wrapper

# Usage
@monitor_api_calls
def create_completion(**kwargs):
    return hai.chat.completions.create(**kwargs)
```

## 🔒 Security Best Practices

### API Key Management

```python
# ✅ Good: Use environment variables
import os
hai = HAI(api_key=os.getenv("HAI_API_KEY"))

# ❌ Bad: Hardcode API keys
hai = HAI(api_key="hai-1234567890abcdef")  # Never do this!
```

### Input Sanitization

```python
import re

def sanitize_user_input(text):
    """Sanitize user input to prevent injection attacks"""
    # Remove potentially harmful patterns
    text = re.sub(r'<script.*?</script>', '', text, flags=re.IGNORECASE | re.DOTALL)
    text = re.sub(r'javascript:', '', text, flags=re.IGNORECASE)
    
    # Limit length
    if len(text) > 10000:
        text = text[:10000]
    
    return text.strip()
```

### Rate Limiting

```python
from collections import defaultdict
import time

class RateLimiter:
    def __init__(self, max_requests=100, time_window=3600):
        self.max_requests = max_requests
        self.time_window = time_window
        self.requests = defaultdict(list)
    
    def is_allowed(self, user_id):
        now = time.time()
        user_requests = self.requests[user_id]
        
        # Remove old requests
        user_requests[:] = [req_time for req_time in user_requests 
                           if now - req_time < self.time_window]
        
        if len(user_requests) >= self.max_requests:
            return False
        
        user_requests.append(now)
        return True
```

## 📊 Performance Optimization

### Batch Processing

```python
async def process_multiple_requests(requests):
    """Process multiple requests efficiently"""
    import asyncio
    
    async def process_single_request(request):
        # Simulate async API call
        return hai.chat.completions.create(**request)
    
    # Process in batches to avoid overwhelming the API
    batch_size = 5
    results = []
    
    for i in range(0, len(requests), batch_size):
        batch = requests[i:i + batch_size]
        batch_results = await asyncio.gather(
            *[process_single_request(req) for req in batch]
        )
        results.extend(batch_results)
        
        # Small delay between batches
        if i + batch_size < len(requests):
            await asyncio.sleep(0.1)
    
    return results
```

### Memory Management

```python
def process_large_conversation(messages, chunk_size=10):
    """Process large conversations in chunks"""
    if len(messages) <= chunk_size:
        return hai.chat.completions.create(
            model="Dhanishtha-2.0-preview",
            messages=messages
        )
    
    # Keep system message and recent context
    system_msgs = [msg for msg in messages if msg['role'] == 'system']
    recent_msgs = messages[-chunk_size:]
    
    # Combine system and recent messages
    processed_msgs = system_msgs + recent_msgs
    
    return hai.chat.completions.create(
        model="Dhanishtha-2.0-preview",
        messages=processed_msgs
    )
```

## 🧪 Testing Strategies

### Unit Testing

```python
import unittest
from unittest.mock import Mock, patch

class TestHAIIntegration(unittest.TestCase):
    def setUp(self):
        self.hai = HAI(api_key="test-key")
    
    @patch('HelpingAI.HAI.chat.completions.create')
    def test_basic_completion(self, mock_create):
        # Mock response
        mock_response = Mock()
        mock_response.choices = [Mock()]
        mock_response.choices[0].message.content = "Test response"
        mock_create.return_value = mock_response
        
        # Test
        response = self.hai.chat.completions.create(
            model="Dhanishtha-2.0-preview",
            messages=[{"role": "user", "content": "Test"}]
        )
        
        self.assertEqual(response.choices[0].message.content, "Test response")
        mock_create.assert_called_once()
```

---

**Follow these best practices to build robust, efficient, and secure applications with HelpingAI! 🚀**
