# openclaw-ai-conversation-summary

An [OpenClaw](https://github.com/Arxchibobo) skill that generates concise summaries of conversation history, with support for incremental updates.

## Overview

`ai-conversation-summary` is an OpenClaw skill that connects to a summarization API to produce structured overviews of chat histories. It is designed to help users quickly understand what was discussed in a conversation without reading through the full message log.

## Features

- Summarizes conversation content from structured chat lists
- Supports incremental updates — provide a previous summary to extend rather than reprocess from scratch
- Accepts both English and Chinese trigger phrases
- Returns a structured JSON response with a clean summary string

## Usage

### Trigger Phrases

Activate this skill by saying:

- "Summarize this conversation"
- "What did we talk about?"
- "Give me a summary"
- "Recap our discussion"
- "总结一下对话"
- "帮我生成摘要"

### API

The skill calls the following endpoint:

```bash
curl -X POST "https://iautomark.sdm.qq.com/assistant-analyse/v1/assistant/poc/summary/trigger" \
  -H "Content-Type: application/json" \
  -d '{
    "chatList": "<JSON formatted conversation list>",
    "historySummary": "<previous summary for incremental update, optional>"
  }'
```

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chatList` | string | Yes | JSON-formatted list of conversation messages |
| `historySummary` | string | No | Previous summary, used for incremental updates |

### `chatList` Format

```json
[
  {"role": "user", "content": "How is the weather today?"},
  {"role": "assistant", "content": "It is sunny, 25 degrees."}
]
```

### Response

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "summary": "The user asked about the weather, and the assistant reported sunny conditions at 25°C."
  }
}
```

- `code`: `0` indicates success; any other value indicates an error
- `data.summary`: The generated summary text

## Error Handling

- If `code` is non-zero, report the `message` field to the user
- If the request fails entirely, verify network connectivity
- Ensure `chatList` is valid JSON before calling the API

## Requirements

- An OpenClaw-compatible agent environment
- Network access to `iautomark.sdm.qq.com`

## License

MIT
