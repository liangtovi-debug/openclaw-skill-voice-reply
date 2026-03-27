# TTS Config Examples

Use these examples when the user wants persistent defaults in `~/.openclaw/openclaw.json`.

## Turn on spoken replies by default

```json5
{
  messages: {
    tts: {
      auto: "always",
      provider: "edge",
    },
  },
}
```

## Chinese Edge default

```json5
{
  messages: {
    tts: {
      auto: "always",
      provider: "edge",
      edge: {
        enabled: true,
        voice: "zh-CN-XiaoxiaoNeural",
        lang: "zh-CN",
        outputFormat: "audio-24khz-48kbitrate-mono-mp3",
      },
    },
  },
}
```

## Reply with audio only after inbound voice messages

```json5
{
  messages: {
    tts: {
      auto: "inbound",
      provider: "edge",
    },
  },
}
```

## Use tagged mode for on-demand audio

```json5
{
  messages: {
    tts: {
      auto: "tagged",
      provider: "openai",
      modelOverrides: {
        enabled: true,
      },
      openai: {
        model: "gpt-4o-mini-tts",
        voice: "alloy",
      },
    },
  },
}
```

## OpenAI as the default provider

```json5
{
  messages: {
    tts: {
      auto: "always",
      provider: "openai",
      openai: {
        apiKey: "OPENAI_API_KEY",
        model: "gpt-4o-mini-tts",
        voice: "alloy",
      },
    },
  },
}
```

## ElevenLabs as the default provider

```json5
{
  messages: {
    tts: {
      auto: "always",
      provider: "elevenlabs",
      elevenlabs: {
        apiKey: "ELEVENLABS_API_KEY",
        voiceId: "voice_id",
        modelId: "eleven_multilingual_v2",
      },
    },
  },
}
```

## Practical defaults

- Use Edge when the user wants zero-key setup.
- Use OpenAI when the user wants better quality and already has `OPENAI_API_KEY`.
- Use ElevenLabs when the user specifically wants ElevenLabs voices.
- Prefer `/tts ...` commands instead of editing config when the request is only for the current conversation.
