# 🔊 Voice Reply Skill for OpenClaw

让 AI "说话"的技能，支持多种 TTS 提供商和输出模式。

## 功能特性

### 3 种 TTS 输出方式

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| 即时语音 | 单次生成音频，无需改变默认设置 | 临时需要语音回复 |
| 会话模式 | 用 `/tts` 命令控制当前会话的语音行为 | 当前对话需要语音 |
| 持久配置 | 修改 `openclaw.json` 设置默认语音回复行为 | 长期启用语音 |

### 支持的 TTS 提供商

- **Edge TTS** - 免 API Key，微软官方，中文效果优秀
- **OpenAI** - GPT-4o-mini TTS，音质高
- **ElevenLabs** - 多语言高质量音色

### 支持的音色

- 中文女声：`zh-CN-XiaoxiaoNeural`
- 英文女声：`en-US-MichelleNeural`
- OpenAI：`alloy`, `echo`, `fable` 等
- ElevenLabs：多种自定义音色

## 触发关键词

语音回复、发语音、读出来、播报、朗读、切换音色

## 快速开始

1. 安装 skill：
   ```
   clawhub install voice-reply
   ```

2. 使用命令：
   - `/tts always` - 始终语音回复
   - `/tts inbound` - 收到语音时回复语音
   - `/tts audio ...` - 单次生成语音
   - `/tts off` - 关闭语音

## 配置示例

详见 [references/config-examples.md](references/config-examples.md)

## 文档

- [SKILL.md](SKILL.md) - 完整技能文档
- [OpenClaw 官方文档](https://docs.openclaw.ai/tools/tts)