# G4F Provider Status Report

Generated on: `2026-06-15 13:50:54`

## Summary

- **Working:** 6
- **Failed:** 31
- **Skipped (Driver):** 47
- **Skipped (API Key):** 13

## Working Providers

| Provider Name | URL | Response Time | Supported Models |
| :--- | :--- | :--- | :--- |
| [OpenAIFM](https://www.openai.fm) | https://www.openai.fm | 1.62s | `friendly`, `patient_teacher`, `noir_detective`, `cowboy`, `calm` (+12 more) |
| [CohereForAI_C4AI_Command](https://coherelabs-c4ai-command.hf.space) | https://coherelabs-c4ai-command.hf.space | 3.88s | `command-a-03-2025`, `command-r-plus-08-2024`, `command-r-08-2024`, `command-r-plus`, `command-r` (+2 more) |
| [Felo](https://felo.ai) | https://felo.ai | 4.0s | `felo-chat`, `felo-search`, `felo-scholar`, `felo-social`, `felo-document` |
| [WeWordle](https://chat-gpt.com) | https://chat-gpt.com | 3.3s | `v3`, `gpt-4`, `gpt-4o`, `gpt-4o-mini`, `deepseek` (+2 more) |
| [Perplexity](https://www.perplexity.ai) | https://www.perplexity.ai | 10.41s | `auto`, `turbo`, `gpt41`, `gpt5`, `gpt5_thinking` (+41 more) |
| [Yqcloud](https://chat9.yqcloud.top) | https://chat9.yqcloud.top | 6.41s | `gpt-4` |

## Failed Providers

| Provider Name | Error Description | Latency |
| :--- | :--- | :--- |
| CachedSearch | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.02s |
| Claude | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.02s |
| Custom | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.08s |
| DeepInfra | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| EasyChat | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| EdgeTTS | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.03s |
| BlackForestLabs_Flux1KontextDev | `No media files provided for image generation.` | 0.16s |
| GeminiPro | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.02s |
| Groq | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.02s |
| HuggingFace | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| HuggingSpace | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| LMArena | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| MarkItDown | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| MetaAI | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| Nvidia | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| Ollama | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| OllamaSwarm | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.0s |
| OpenRouterFree | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 1.62s |
| OperaAria | `403, message='Forbidden', url='https://auth.opera.com/account/v2/external/anonymous/signup'` | 3.12s |
| PollinationsAI | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.61s |
| PollinationsAudio | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.31s |
| PollinationsImage | `ChatCompletion.create() missing 1 required positional argument: 'model'` | 0.02s |
| PhindAi | `Response 403: Cloudflare detected` | 4.36s |
| StabilityAI_SD35Large | `GPU token limit exceeded: data: {"error": "You have exceeded your ZeroGPU quota (65s requested vs. 68s left). Try again ` | 3.25s |
| TeachAnything | `unsupported operand type(s) for *: 'ClientTimeout' and 'int'` | 3.25s |
| Qwen | `<!doctypehtml><meta charset="UTF-8"><meta name="aliyun_waf_aa"content="ff926c7f07e45e2e487a29a6197d3460"><meta name="ali` | 3.42s |
| StabilityAI_SD35Large | `GPU token limit exceeded: data: {"error": "You have exceeded your ZeroGPU quota (65s requested vs. 68s left). Try again ` | 5.58s |
| AnyProvider | `Timeout after 20 seconds` | 20.0s |
| BlackForestLabs_Flux1Dev | `Timeout after 20 seconds` | 20.0s |
| CopilotApp | `Timeout after 20 seconds` | 20.0s |
| YouTube | `Timeout after 20 seconds` | 20.0s |

## Skipped - Requires WebDriver / Headless Automation

- AIBadgr (URL: https://aibadgr.com)
- Anthropic (URL: https://console.anthropic.com)
- Antigravity (URL: https://antigravity.google)
- ApiAirforce (URL: https://api.airforce)
- BingCreateImages (URL: https://www.bing.com/images/create)
- BlackboxPro (URL: https://www.blackbox.ai)
- CablyAI (URL: https://cablyai.com/chat)
- Cerebras (URL: https://inference.cerebras.ai/)
- Cloudflare (URL: https://playground.ai.cloudflare.com)
- Cohere (URL: https://cohere.com)
- Copilot (URL: https://copilot.microsoft.com)
- CopilotAccount (URL: https://copilot.microsoft.com)
- CopilotSession (URL: https://copilot.microsoft.com)
- DeepSeek (URL: https://platform.deepseek.com)
- DeepSeekAPI (URL: https://chat.deepseek.com)
- FenayAI (URL: https://fenayai.com)
- Gemini (URL: https://gemini.google.com)
- GeminiCLI (URL: None)
- GigaChat (URL: https://developers.sber.ru/gigachat)
- GithubCopilot (URL: https://github.com/copilot)
- GithubCopilotAPI (URL: https://github.com/copilot)
- GlhfChat (URL: https://glhf.chat)
- GoogleSearch (URL: https://google.com)
- Grok (URL: https://grok.com)
- HailuoAI (URL: https://www.hailuo.ai)
- HuggingChat (URL: https://huggingface.co/chat)
- HuggingFaceAPI (URL: https://api-inference.huggingface.com)
- HuggingFaceMedia (URL: https://huggingface.co)
- MetaAIAccount (URL: https://www.meta.ai)
- MicrosoftDesigner (URL: https://designer.microsoft.com)
- MiniMax (URL: https://www.hailuo.ai/chat)
- OpenRouter (URL: https://openrouter.ai)
- OpenaiAPI (URL: https://platform.openai.com)
- OpenaiAccount (URL: https://chatgpt.com)
- OpenaiChat (URL: https://chatgpt.com)
- PerplexityApi (URL: https://www.perplexity.ai)
- Pi (URL: https://pi.ai/talk)
- PuterJS (URL: https://docs.puter.com/playground)
- QwenCode (URL: https://qwen.ai)
- Reka (URL: https://space.reka.ai)
- Replicate (URL: https://replicate.com)
- ThebApi (URL: https://theb.ai)
- Together (URL: https://together.xyz)
- Video (URL: None)
- WhiteRabbitNeo (URL: https://www.whiterabbitneo.com)
- You (URL: https://you.com)
- xAI (URL: https://console.x.ai)

## Skipped - Requires API Authentication Keys

- Azure (URL: https://ai.azure.com)
- BackendApi (URL: None)
- CreateImagesProvider (URL: None)
- Feature (URL: None)
- GLM (URL: https://chat.z.ai)
- GradientNetwork (URL: https://chat.gradient.network)
- HuggingFaceInference (URL: https://huggingface.co)
- Local (URL: None)
- OpenaiTemplate (URL: None)
- RotatedProvider (URL: None)
- SearXNG (URL: http://searxng:8080)
- Yupp (URL: https://yupp.ai)
- gTTS (URL: None)
