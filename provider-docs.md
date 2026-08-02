# Provider Docs — Agent Register

> Rena, prioriterade länkar till officiell dokumentation för providers som används i Canvas Agents.
> Avsedd för GitHub-agenter som bygger och underhåller pipelines/kod.
>
> **Senast uppdaterad:** 2026-08-02
>
> **Raw (fetch):** https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/provider-docs.md  
> **JSON:** https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/provider-docs.json

---

## 1. xAI (högsta prioritet)

Detaljerat register finns i [xai-docs-register.md](xai-docs-register.md).

| Ämne | URL |
|------|-----|
| Function Calling | https://docs.x.ai/developers/tools/function-calling |
| Tools Overview | https://docs.x.ai/developers/tools/overview |
| Web Search tool | https://docs.x.ai/developers/tools/web-search |
| Models | https://docs.x.ai/developers/models |
| Grok Imagine (image + video) | https://x.ai/api/imagine |
| Video generation | https://docs.x.ai/developers/model-capabilities/video/generation |
| Docs MCP | https://docs.x.ai/api/mcp |
| Huvuddokumentation | https://docs.x.ai |

**Agent-tips:** Många sidor har `.md`-version. Docs MCP: `list_doc_pages`, `get_doc_page`, `search_docs`.

---

## 2. OpenAI

| Ämne | URL |
|------|-----|
| Function Calling | https://developers.openai.com/api/docs/guides/function-calling |
| Tools | https://developers.openai.com/api/docs/guides/tools |
| API Docs (home) | https://developers.openai.com/api/docs |
| API Reference | https://developers.openai.com/api/reference/overview |

**Agent-tips:** Stöd för `/llms.txt` och `.md` på flera sidor.

---

## 3. Anthropic (Claude)

| Ämne | URL |
|------|-----|
| Tool Use / Function calling | https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview |
| Platform Docs (home) | https://platform.claude.com/docs |

---

## 4. Google Gemini

| Ämne | URL |
|------|-----|
| Function Calling | https://ai.google.dev/gemini-api/docs/function-calling |
| Image Generation (Nano Banana / Gemini image) | https://ai.google.dev/gemini-api/docs/image-generation |
| Video Generation (Veo) | https://ai.google.dev/gemini-api/docs/video |
| Gemini API (home) | https://ai.google.dev/gemini-api/docs |
| API Reference | https://ai.google.dev/api |

**Agent-tips:** Function calling- och image/video-sidorna är de mest användbara. Undvik rena landingssidor.

---

## 5. ElevenLabs

| Ämne | URL |
|------|-----|
| Text-to-Speech (convert) | https://elevenlabs.io/docs/api-reference/text-to-speech/convert |
| API Reference (intro) | https://elevenlabs.io/docs/api-reference/introduction |
| TTS capability overview | https://elevenlabs.io/docs/overview/capabilities/text-to-speech |
| Docs home | https://elevenlabs.io/docs/overview/intro |

**Agent-tips:** Explicit agent-stöd — `/llms.txt` och `.md` på nästan varje sida.

---

## 6. Google Cloud (lägre prioritet)

| Ämne | URL |
|------|-----|
| Cloud Translation | https://docs.cloud.google.com/translate/docs |
| YouTube Data API | https://developers.google.com/youtube/v3/docs |

**Agent-tips:** Verbose Cloud-docs. Extrahera endast parametrar/endpoints/limits.

---

## Användning i agent-workflow

1. Hämta detta index (`provider-docs.md` eller `.json`).
2. Välj relevanta URL:er utifrån nod/provider (t.ex. `videogen` → Veo, `grokvideo` → xAI Imagine, `agent` + tools → function calling-sidor).
3. `webfetch` / `curl` endast de tekniska sidorna.
4. Instruktion till agenten: *"Extrahera parametrar, enums, limits, supported modes och schema. Ignorera marknadsföring och tutorial-text."*

---

*Detta register är en sammanställning för agenter. För auktoritativ information, använd alltid leverantörens officiella dokumentation.*
