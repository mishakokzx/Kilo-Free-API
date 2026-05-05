# Kilo-Free-API

> Access powerful LLMs **completely free** — no sign-up, no API key, no auth token. Just raw HTTP requests.

Kilo Code exposes an OpenRouter-compatible (and Anthropic-compatible) API endpoint that lets you hit free-tier models directly without any authentication. Zero cost, zero friction.

---

## Base URL

```
https://api.kilo.ai/api/openrouter/
```

---

## List Available Free Models

```bash
curl -X GET "https://api.kilo.ai/api/openrouter/models" \
  | jq '.data[] | select(.id | contains(":free")) | .id'
```

Or use the auto-routing free model alias:

```
kilo-auto/free
```

This automatically picks an available free model for you.

---

## Chat Completions — OpenAI-compatible

### Streaming

```bash
curl -X POST https://api.kilo.ai/api/openrouter/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "kilo-auto/free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "max_tokens": 1024,
    "stream": true
  }'
```

### Non-Streaming

```bash
curl -X POST https://api.kilo.ai/api/openrouter/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "kilo-auto/free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "max_tokens": 1024,
    "stream": false
  }'
```

---

## Messages Endpoint — Anthropic-compatible

```bash
curl -X POST https://api.kilo.ai/api/openrouter/messages \
  -H "Content-Type: application/json" \
  -d '{
    "model": "kilo-auto/free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "max_tokens": 1024,
    "stream": false
  }'
```

---

## Use with SDKs

### OpenAI SDK (Python)

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://api.kilo.ai/api/openrouter/",
    api_key="no-key-needed"  # any string works
)

response = client.chat.completions.create(
    model="kilo-auto/free",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### OpenAI SDK (Node.js)

```js
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://api.kilo.ai/api/openrouter/",
  apiKey: "no-key-needed",
});

const res = await client.chat.completions.create({
  model: "kilo-auto/free",
  messages: [{ role: "user", content: "Hello!" }],
});
console.log(res.choices[0].message.content);
```

### Anthropic SDK (Python)

```python
import anthropic

client = anthropic.Anthropic(
    base_url="https://api.kilo.ai/api/openrouter/",
    api_key="no-key-needed"
)

message = client.messages.create(
    model="kilo-auto/free",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello!"}]
)
print(message.content[0].text)
```

---

## Use with Claude Desktop / Claude Code

Set the base URL to `https://api.kilo.ai/api/openrouter/` and use any `:free` model ID from the models list.

---

## Notes

- **No authentication required** — no `Authorization` header needed
- **Rate limits**: You may occasionally hit `429` errors — it's free, that's expected. Just retry.
- **Cost**: Always `0`. The `cost` field in responses will always be `0`.
- **Model routing**: Use `kilo-auto/free` to let the API pick the best available free model automatically, or specify any model ID ending in `:free` from the models list.

---

## Free Models (sample)

| Model ID | Provider |
|---|---|
| `inclusionai/ling-2.6-1t:free` | Novita |
| `nvidia/nemotron-3-super-120b-a12b:free` | NVIDIA |
| `poolside/laguna-m.1:free` | Poolside |
| `x-ai/grok-code-fast-1:optimized:free` | xAI |
| `stepfun/step-3.5-flash:free` | StepFun |
| `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free` | NVIDIA |

Run the models endpoint to get the full current list.

---

**Enjoy free inference. Don't abuse it.**
