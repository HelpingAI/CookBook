# HelpingAI Troubleshooting Guide 🔧

This comprehensive guide helps you diagnose and resolve common issues when working with HelpingAI models.

## 🚨 Common Issues and Solutions

### 🔑 Authentication Problems

#### Issue: "Invalid API key" Error
```
AuthenticationError: Invalid API key provided
```

**Solutions:**
1. **Check API Key Format**
   ```python
   # Correct format
   api_key = "hai-1234567890abcdef..."  # Should start with "hai-"
   ```

2. **Verify Environment Variable**
   ```bash
   # Check if environment variable is set
   echo $HAI_API_KEY
   
   # Set environment variable
   export HAI_API_KEY="your-api-key-here"
   ```

3. **Check API Key in Code**
   ```python
   import os
   from HelpingAI import HAI
   
   # Explicit API key
   hai = HAI(api_key="your-api-key")
   
   # Or from environment
   hai = HAI()  # Reads from HAI_API_KEY
   ```

#### Issue: "Organization not found"
```
AuthenticationError: Organization not found
```

**Solution:**
```python
# Remove organization parameter if not needed
hai = HAI(api_key="your-key")  # Don't include organization

# Or verify correct organization ID
hai = HAI(api_key="your-key", organization="correct-org-id")
```

### ⏰ Rate Limiting Issues

#### Issue: "Rate limit exceeded"
```
RateLimitError: Rate limit exceeded. Please wait before making more requests.
```

**Solutions:**
1. **Implement Exponential Backoff**
   ```python
   import time
   from HelpingAI import RateLimitError
   
   def api_call_with_retry(max_retries=3):
       for attempt in range(max_retries):
           try:
               return hai.chat.completions.create(...)
           except RateLimitError:
               if attempt < max_retries - 1:
                   wait_time = 2 ** attempt  # Exponential backoff
                   time.sleep(wait_time)
               else:
                   raise
   ```

2. **Check Rate Limits**
   ```python
   # Free tier: 100 requests/hour
   # Pro tier: 1000 requests/hour
   # Implement request tracking
   
   import time
   from collections import deque
   
   class RateLimiter:
       def __init__(self, max_requests=100, time_window=3600):
           self.max_requests = max_requests
           self.time_window = time_window
           self.requests = deque()
       
       def can_make_request(self):
           now = time.time()
           # Remove old requests
           while self.requests and now - self.requests[0] > self.time_window:
               self.requests.popleft()
           
           return len(self.requests) < self.max_requests
       
       def record_request(self):
           self.requests.append(time.time())
   ```

### 🌐 Network and Connection Issues

#### Issue: "Connection timeout"
```
APIConnectionError: Connection timeout
```

**Solutions:**
1. **Increase Timeout**
   ```python
   hai = HAI(timeout=120.0)  # 2 minutes timeout
   ```

2. **Check Network Connection**
   ```python
   import requests
   
   try:
       response = requests.get("https://api.helpingai.co/health", timeout=10)
       print(f"API Status: {response.status_code}")
   except requests.exceptions.RequestException as e:
       print(f"Network issue: {e}")
   ```

3. **Retry with Backoff**
   ```python
   from HelpingAI import APIConnectionError
   
   def robust_api_call():
       for attempt in range(3):
           try:
               return hai.chat.completions.create(...)
           except APIConnectionError:
               if attempt < 2:
                   time.sleep(5)  # Wait 5 seconds
               else:
                   raise
   ```

### 📝 Request Format Issues

#### Issue: "Invalid request format"
```
InvalidRequestError: Invalid request format
```

**Common Causes and Solutions:**

1. **Invalid Message Format**
   ```python
   # ❌ Wrong
   messages = ["Hello"]
   
   # ✅ Correct
   messages = [{"role": "user", "content": "Hello"}]
   ```

2. **Invalid Model Name**
   ```python
   # ❌ Wrong
   model = "gpt-3.5-turbo"
   
   # ✅ Correct
   model = "Dhanishtha-2.0-preview"  # or "Helpingai3-raw"
   ```

3. **Invalid Parameters**
   ```python
   # ❌ Wrong
   temperature = 2.0  # Too high
   max_tokens = 50000  # Too high
   
   # ✅ Correct
   temperature = 0.8  # 0.0 - 1.0
   max_tokens = 2048  # Within limits
   ```

### 🧠 Model-Specific Issues

#### Issue: Dhanishta 2.0 Not Showing Thinking Process

**Problem:**
```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Solve: 2+2"}]
)
# No <think> blocks in response
```

**Solution:**
```python
response = hai.chat.completions.create(
    model="Dhanishtha-2.0-preview",
    messages=[{"role": "user", "content": "Solve: 2+2"}],
    hide_think=False  # Show thinking process
)
```

#### Issue: Poor Emotional Responses from Dhanishta 2.0

**Problem:** Using Dhanishta 2.0 for emotional support

**Solution:** Use HelpingAI3-raw instead
```python
# ❌ Not optimal
model = "Dhanishtha-2.0-preview"  # Better for reasoning

# ✅ Better choice
model = "Helpingai3-raw"  # Better for emotional intelligence
```

### 💾 Memory and Performance Issues

#### Issue: "Context length exceeded"
```
InvalidRequestError: This model's maximum context length is 40960 tokens
```

**Solutions:**
1. **Truncate Messages**
   ```python
   def truncate_conversation(messages, max_tokens=35000):
       # Keep system message and recent messages
       system_msgs = [msg for msg in messages if msg['role'] == 'system']
       other_msgs = [msg for msg in messages if msg['role'] != 'system']
       
       # Estimate tokens (rough: 1 token ≈ 4 characters)
       total_chars = sum(len(msg['content']) for msg in messages)
       
       if total_chars > max_tokens * 4:
           # Keep recent messages
           recent_msgs = other_msgs[-10:]  # Last 10 messages
           return system_msgs + recent_msgs
       
       return messages
   ```

2. **Summarize Old Context**
   ```python
   def summarize_old_context(old_messages):
       summary_prompt = "Summarize this conversation: " + str(old_messages)
       
       summary_response = hai.chat.completions.create(
           model="Helpingai3-raw",
           messages=[{"role": "user", "content": summary_prompt}],
           max_tokens=200
       )
       
       return summary_response.choices[0].message.content
   ```

### 🔍 Debugging Techniques

#### Enable Detailed Logging
```python
import logging

# Set up logging
logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

def debug_api_call():
    try:
        logger.info("Making API call...")
        response = hai.chat.completions.create(
            model="Dhanishtha-2.0-preview",
            messages=[{"role": "user", "content": "Test"}]
        )
        logger.info(f"Response received: {len(response.choices[0].message.content)} chars")
        return response
    except Exception as e:
        logger.error(f"API call failed: {e}")
        raise
```

#### Response Validation
```python
def validate_response(response):
    """Validate API response structure"""
    checks = {
        "has_choices": hasattr(response, 'choices') and len(response.choices) > 0,
        "has_message": hasattr(response.choices[0], 'message'),
        "has_content": hasattr(response.choices[0].message, 'content'),
        "content_not_empty": bool(response.choices[0].message.content.strip())
    }
    
    for check, passed in checks.items():
        if not passed:
            print(f"❌ Validation failed: {check}")
            return False
    
    print("✅ Response validation passed")
    return True
```

## 🛠️ Diagnostic Tools

### API Health Check
```python
def health_check():
    """Check API connectivity and basic functionality"""
    print("🔍 Running HelpingAI Health Check...")
    
    try:
        # Test basic connectivity
        response = hai.chat.completions.create(
            model="Helpingai3-raw",
            messages=[{"role": "user", "content": "Hello"}],
            max_tokens=10
        )
        
        print("✅ Basic connectivity: OK")
        print(f"✅ Response received: {len(response.choices[0].message.content)} chars")
        
        # Test Dhanishta 2.0
        response2 = hai.chat.completions.create(
            model="Dhanishtha-2.0-preview",
            messages=[{"role": "user", "content": "What is 2+2?"}],
            max_tokens=50,
            hide_think=False
        )
        
        has_thinking = "<think>" in response2.choices[0].message.content
        print(f"✅ Dhanishta 2.0 thinking: {'OK' if has_thinking else 'Not visible'}")
        
        # Test models list
        models = hai.models.list()
        print(f"✅ Available models: {len(models)}")
        
        return True
        
    except Exception as e:
        print(f"❌ Health check failed: {e}")
        return False

# Run health check
health_check()
```

### Performance Monitor
```python
import time

class PerformanceMonitor:
    def __init__(self):
        self.calls = []
    
    def monitor_call(self, **kwargs):
        start_time = time.time()
        
        try:
            response = hai.chat.completions.create(**kwargs)
            duration = time.time() - start_time
            
            self.calls.append({
                "timestamp": start_time,
                "duration": duration,
                "success": True,
                "model": kwargs.get("model"),
                "tokens": getattr(response.usage, 'total_tokens', None) if hasattr(response, 'usage') else None
            })
            
            return response
            
        except Exception as e:
            duration = time.time() - start_time
            self.calls.append({
                "timestamp": start_time,
                "duration": duration,
                "success": False,
                "error": str(e),
                "model": kwargs.get("model")
            })
            raise
    
    def get_stats(self):
        if not self.calls:
            return "No calls recorded"
        
        successful_calls = [call for call in self.calls if call["success"]]
        failed_calls = [call for call in self.calls if not call["success"]]
        
        avg_duration = sum(call["duration"] for call in successful_calls) / len(successful_calls) if successful_calls else 0
        
        return {
            "total_calls": len(self.calls),
            "successful_calls": len(successful_calls),
            "failed_calls": len(failed_calls),
            "success_rate": len(successful_calls) / len(self.calls) * 100,
            "avg_duration": round(avg_duration, 2)
        }

# Usage
monitor = PerformanceMonitor()
response = monitor.monitor_call(
    model="Helpingai3-raw",
    messages=[{"role": "user", "content": "Hello"}]
)
print(monitor.get_stats())
```

## 📞 Getting Help

### Community Resources
- **Discord Community**: [discord.gg/helpingai](https://discord.gg/helpingai)
- **GitHub Issues**: Report bugs and feature requests
- **Documentation**: [docs.helpingai.co](https://docs.helpingai.co)

### Support Channels
- **Email Support**: support@helpingai.co
- **Technical Issues**: Include error messages and code snippets
- **Feature Requests**: Describe use case and expected behavior

### Before Contacting Support
1. **Check this troubleshooting guide**
2. **Run the health check tool**
3. **Gather error messages and logs**
4. **Prepare minimal reproduction code**
5. **Note your environment details** (Python version, OS, etc.)

---

**Most issues can be resolved with proper error handling and parameter tuning! 🔧✨**
