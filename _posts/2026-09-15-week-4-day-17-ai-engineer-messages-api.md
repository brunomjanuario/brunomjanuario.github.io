---
title: "Week 4, Day 17 — AI Engineer vs ML Engineer, and the Anthropic Messages API"
date: 2026-09-15 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, ai-engineering, anthropic, claude, api, python]
---

Day 17 starts Week 4 and a completely different phase of the roadmap. Three
weeks of math and NumPy were about what's *inside* a model. This week is about
**building with models that already exist** — starting with what an AI engineer
actually does, and the Anthropic **Messages API**. These are my notes, cleaned
up.

## AI engineer vs ML engineer

The roadmap opens by separating two job titles that get used interchangeably,
and the distinction turns out to be about **where in the stack you work**.

**ML engineers** build and maintain machine learning systems. They clean data,
build pipelines, train models, and keep them running in production. The model
is the thing they produce.

**AI engineers** ship products on top of models that already exist. They don't
train models — they use them. An AI chatbot, a document summariser, a support
triage tool: the model is a component, and the job is everything around it.

**Researchers** are the third group: they train and fine-tune models to push
capability forward.

What clarified it for me is that these are genuinely different skill sets, not
seniority levels. Weeks 1 to 3 — linear algebra, calculus, NumPy — are the ML
engineer's foundation. Week 4 onward is the AI engineer's, and the reason the
roadmap braids them together is that understanding what's inside the box makes
you better at building with it, even when you never open it.

## Quickstart

Getting to a first API call is four steps:

1. **Set your API key** as an environment variable.
2. **Create a project and install the SDK** — `pip install anthropic`.
3. **Write the code** — `import anthropic`.
4. **Run it.**

The key lives in the environment rather than in the code, which is the first
habit worth forming: an API key in a source file is a key you will eventually
commit.

## The Messages API

Everything goes through one endpoint. A basic request:

```python
import anthropic

client = anthropic.Anthropic()

message = client.messages.create(
    model="claude-opus-5",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello, Claude"}],
)
print(message)
```

Three required pieces: **which model**, a **cap on the response length**, and
the **messages** themselves. Each message is a dict with a `role` and
`content`.

### The response

What comes back is structured, not just a string:

```json
{
  "id": "msg_01XFDUDYJgAACzvnptvVoYEL",
  "type": "message",
  "role": "assistant",
  "content": [{ "type": "text", "text": "Hello!" }],
  "model": "claude-opus-5",
  "stop_reason": "end_turn",
  "stop_sequence": null,
  "usage": { "input_tokens": 12, "output_tokens": 6 }
}
```

The fields worth knowing:

- **`content` is a list of blocks**, not a string. Each block has a `type` —
  here it's `text`, but it can also be a tool call or other block types. Real
  code iterates the list and checks the type rather than assuming
  `content[0].text`.
- **`stop_reason`** says *why* the model stopped. More on this below.
- **`usage`** reports input and output tokens — which is what you're billed
  on, so it's also your cost meter.

## Multi-turn conversations

The thing that surprised me most: **the API is stateless**. It doesn't remember
anything between calls.

To hold a conversation, you send the **entire history** in the `messages` array
every time, with the newest message last:

```python
messages = [
    {"role": "user", "content": "Hello, Claude"},
    {"role": "assistant", "content": "Hello! How can I help?"},
    {"role": "user", "content": "What did I just say?"},
]
```

So "memory" in a chatbot isn't a feature of the model — it's your application
resending the transcript. Which has a direct consequence: every turn costs more
input tokens than the last, because the whole conversation is re-sent each
time. Conversation length is a cost problem, not just a UX one.

## Images

Claude can read images as well as text. The `content` field becomes a **list of
blocks** instead of a plain string, with an `image` block alongside the text:

```python
messages=[
    {
        "role": "user",
        "content": [
            {
                "type": "image",
                "source": {
                    "type": "base64",
                    "media_type": image_media_type,
                    "data": image_data,
                },
            },
            {"type": "text", "text": "What is in the above image?"},
        ],
    }
]
```

There are three ways to supply the image, differing only in the `source` block:

- **base64** — the bytes inline, as above.
- **url** — Claude fetches it:

```python
{
    "type": "image",
    "source": {"type": "url", "url": "https://example.com/photo.jpg"},
}
```

- **file** — upload once with the Files API and reference it by `file_id`,
  which avoids re-sending the same bytes on every request.

The pattern underneath is the one to remember: **`content` is either a string
or a list of typed blocks**. Once you need anything beyond plain text, it
becomes a list, and everything — text, images, documents — is just another
block type.

## Stop reasons

`stop_reason` tells you why the model stopped generating, and the lesson from
the docs is blunt: **always check it**. The response arrives with HTTP 200
either way, so a truncated or refused answer looks exactly like a good one
until you look.

| `stop_reason` | What happened | What to do |
|---|---|---|
| `end_turn` | Finished naturally | Use the response |
| `max_tokens` | Hit your `max_tokens` cap | Raise the cap, or continue the response |
| `stop_sequence` | Emitted one of your stop sequences | Check `stop_sequence` to see which |
| `tool_use` | Claude is calling a tool | Run it and return the result |
| `pause_turn` | A server-tool loop hit its iteration limit | Send the content back to continue |
| `refusal` | Claude declined | Read `stop_details`, retry on a fallback model |
| `model_context_window_exceeded` | Filled the context window | Treat the response as truncated |

Two of these are silent failures if you ignore them. `max_tokens` means the
answer is cut off mid-thought — and since the quickstart's `max_tokens=1024` is
small, it's the one you'll hit first in real use. `model_context_window_exceeded`
means the same thing for a different reason, and it's the one that eventually
bites long conversations, given that every turn resends the whole history.

`stop_details` carries extra structure, but only for `refusal` — it's empty for
the others, so guard before reading it.

## Takeaway

The mental model shift is that a model call is an **HTTP request**, not a
magic box. It takes structured input, returns structured output, costs money
per token, and can fail in ways that still return a 200.

Two things follow from statelessness, and both are the same fact wearing
different clothes. A conversation is something **your** code maintains by
resending history, which means context length and cost grow together with every
turn. That one design decision explains most of what makes LLM applications
awkward to build, and it's why later weeks spend so much time on context
management.

The other habit worth forming now is **checking `stop_reason` before trusting
`content`**. It's the same instinct as `np.isnan(matrix).any()` from Week 3:
the failure doesn't announce itself, so you have to ask.

## Sources

1. [Anthropic API — Quickstart](https://platform.claude.com/docs/en/get-started)
2. [Messages API reference](https://platform.claude.com/docs/en/api/messages)
3. [Vision — working with images](https://platform.claude.com/docs/en/docs/build-with-claude/vision)
