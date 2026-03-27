---
name: voice-reply
description: |
  🔊 OpenClaw 语音回复技能 - 让 AI "说话"
  
  支持 3 种 TTS 输出方式：
  1. 即时语音 - 单次生成音频，无需改变默认设置
  2. 会话模式 - 用 /tts 命令控制当前会话的语音行为（始终语音/收到语音时回复/标记触发）
  3. 持久配置 - 修改 openclaw.json 设置默认语音回复行为
  
  支持多种 TTS 提供商：Edge TTS（免 Key）、OpenAI GPT-4o-mini TTS、ElevenLabs
  支持中文女声（Xiaoxiao）、英文等多种音色
  
  触发关键词：语音回复、发语音、读出来、播报、朗读、切换音色
metadata: { "openclaw": { "emoji": "🔊", "homepage": "https://docs.openclaw.ai/tools/tts" } }
---

# Voice Reply

Use OpenClaw's native TTS stack. Do not invent an external speech pipeline unless the user explicitly asks for a custom integration.

## Choose the path

### 1. One-off audio for specific text

Use the built-in `tts` tool when the user wants a spoken version of text right now but does not want to change the session defaults.

Tool shape:

```json
{
  "text": "请把这段话读出来",
  "channel": "telegram"
}
```

Notes:

- `text` is required.
- `channel` is optional and helps OpenClaw choose a channel-compatible output format.
- The tool already returns `MEDIA:` and may include `[[audio_as_voice]]` for Telegram-compatible voice bubbles.
- If the user asked for audio only, do not repeat the same content again in a long text reply after a successful tool call.

### 2. Session-level spoken replies

Use `/tts` commands when the user wants OpenClaw to keep speaking in the current conversation:

- `/tts always`: speak every eligible reply
- `/tts inbound`: only speak after inbound voice/audio messages
- `/tts tagged`: only speak replies that contain TTS directives
- `/tts off`: disable spoken replies
- `/tts status`: inspect current state
- `/tts audio ...`: generate a one-off audio reply without enabling automatic TTS

Prefer session commands over config edits when the user asks for temporary behavior such as "这次会话里都用语音回复" or "只在我发语音时回语音".

### 3. Persistent defaults

Edit `~/.openclaw/openclaw.json` when the user wants a lasting default such as:

- "以后默认语音回复"
- "把默认声音换成中文女声"
- "默认用 OpenAI 朗读"

See `references/config-examples.md` for ready-to-apply snippets.

## Decision rules

- If the user asks "读出来", "发语音", "语音回复", "播报", or "把这段转成语音", generate audio instead of only explaining how TTS works.
- If the user wants only one spoken answer, prefer the `tts` tool or `/tts audio`.
- If the user wants audio on every reply in the current session, use `/tts always`.
- If the user wants audio only after inbound voice messages, use `/tts inbound`.
- If the user wants audio only when explicitly requested per reply, use `/tts tagged`.
- If the reply already contains other media, OpenClaw normally skips automatic TTS. Do not fight that behavior unless the user clearly wants a separate audio attachment too.

## Provider and voice selection

- Prefer the user's already-configured provider when it works.
- If the user did not specify a provider, Edge TTS is the safest default because it does not require an API key.
- If the user explicitly requests OpenAI or ElevenLabs and credentials are missing, state that clearly and offer Edge as a fallback.
- For Chinese Edge output, prefer:
  - `messages.tts.edge.voice: "zh-CN-XiaoxiaoNeural"`
  - `messages.tts.edge.lang: "zh-CN"`
- For English Edge output, `en-US-MichelleNeural` is a good default.
- For OpenAI, start with:
  - `messages.tts.openai.model: "gpt-4o-mini-tts"`
  - `messages.tts.openai.voice: "alloy"`

## Per-reply TTS directives

Use `[[tts:...]]` directives for per-reply overrides, especially when `messages.tts.auto` is `tagged`.

Example:

```text
可以的，我直接用语音发你一版。

[[tts:provider=openai voice=alloy model=gpt-4o-mini-tts]]
[[tts:text]]这是一段更适合朗读的版本。[[/tts:text]]
```

Guidelines:

- Use `[[tts:text]]...[[/tts:text]]` only when the spoken text should differ from the visible text.
- Keep tables, raw JSON, tool traces, and private notes out of the TTS text block.
- Avoid per-reply provider overrides if the environment may disallow `modelOverrides.allowProvider`.
- Prefer config edits for Edge voice changes; per-reply directive support is best suited to OpenAI or ElevenLabs overrides.

## Config editing guardrails

- Preserve existing JSON5 comments and unrelated keys in `~/.openclaw/openclaw.json`.
- Modify only the smallest relevant `messages.tts` subtree.
- Prefer session commands first for temporary requests.
- Prefer config edits only for persistent defaults.
- If the user asks for better Chinese quality and has OpenAI credentials, OpenAI is a good upgrade path. If they want zero-key setup, stay on Edge.

## Reference

- `references/config-examples.md`
