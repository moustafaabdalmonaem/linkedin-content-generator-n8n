# LinkedIn Content Generator

An n8n workflow that turns a single topic into 3 ready-to-post LinkedIn post variations, plus relevant hashtags — in seconds.

## Overview

Writing consistent, high-quality LinkedIn content is time-consuming. This workflow takes a topic (and optional audience/context) submitted through a simple form, and uses an LLM to generate three distinct post styles so the user can pick the one that fits best, edit lightly, and publish.

## Workflow Steps

1. **TopicForm** — A form trigger where the user submits:
   - Topic (required)
   - Target Audience (optional)
   - A key point to include (optional)
2. **GeneratePostVariations** — Sends the topic and context to an LLM (`gpt-oss-120b` via Groq) with a prompt engineered to produce three distinct LinkedIn post styles:
   - **Educational** — a step-by-step or listicle-style post that teaches something useful
   - **Personal Story** — a first-person anecdote that leads into a lesson or point
   - **Opinion / Hot Take** — a confident point of view or contrarian take
   Each post opens with a strong hook and closes with a call-to-action or engagement question, formatted like a real LinkedIn post (short paragraphs, line breaks, no markdown symbols).
3. **OutputParser** — A structured JSON schema output parser that guarantees the LLM response is returned as clean, parseable data (`variations[]` + `hashtags[]`) instead of free-form text.
4. **FormatOutput** — Maps the parsed AI output into named fields (`educational_post`, `personal_story_post`, `opinion_post`, `hashtags`).
5. **RespondToForm** — Returns all 3 post variations and the hashtag set directly to the user as the form's response.

## Tech Stack

- **n8n** (self-hosted, free tier) — workflow automation
- **Groq API** (free tier) — LLM inference (`openai/gpt-oss-120b`)
- **LangChain nodes** (`@n8n/n8n-nodes-langchain`) — LLM chaining and structured output parsing

## Setup Notes

- This workflow uses a **Groq** credential for the LLM node (`gpt-oss-120b`). After importing, open the `gpt-oss-120b` node and select/create your own Groq API credential — the credential ID in the JSON is a placeholder and won't resolve automatically.
- The workflow runs entirely on a form submission — no external database or paid API is required, making it easy to run on a free, locally hosted n8n instance.

## Screenshots

- `workflow-screenshot.png` — the full workflow canvas in n8n, showing the form trigger, LLM classification/generation chain, and the completion form response.
- `output-example.png` — an example of the completion screen shown to the user after submitting a topic, listing the 3 post variations and hashtags.

## Possible Extensions

- Auto-publish the chosen post to LinkedIn via the LinkedIn API instead of just displaying it.
- Feed in the user's past LinkedIn posts as context so the AI mimics their personal writing voice.
- Add a Google Sheets/Notion step to log every generated post as a running content calendar.
- Add an image/carousel generation step to accompany each post.
<img width="960" height="472" alt="workflow-screenshot" src="https://github.com/user-attachments/assets/4cba8616-134a-4e49-b742-18771042ce9b" />
<img width="512" height="460" alt="output-example" src="https://github.com/user-attachments/assets/435979ba-51c3-4071-816b-7f6fb9f438f0" />

