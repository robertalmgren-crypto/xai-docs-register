# Provider Docs Register

Agent-vänligt register över officiella dokumentationssidor för de providers som används i Canvas Agents-pipelines.

Avsedd för **GitHub-agenter** som bygger och underhåller kod/pipelines. xAI-registret (detaljerat) finns kvar; övriga providers har rena länkar + prioritet.

## Snabblänkar för agenter

| Resurs | URL |
|--------|-----|
| **Repo** | https://github.com/robertalmgren-crypto/xai-docs-register |
| **Provider index (Markdown)** | https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/provider-docs.md |
| **Provider index (JSON)** | https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/provider-docs.json |
| **xAI detaljerat register** | https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md |

## Hur agenter hämtar

```bash
# Hela provider-indexet (Markdown)
curl -sL https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/provider-docs.md

# Maskinläsbart JSON
curl -sL https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/provider-docs.json

# xAI djupregister
curl -sL https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md
```

## Innehåll

- `provider-docs.md` — prioriterad lista med länkar per provider (agentvänlig)
- `provider-docs.json` — samma data som JSON (för programmatisk användning)
- `xai-docs-register.md` — detaljerat xAI-register (Model Capabilities, Files, Voice, Video, Tools m.m.)

## Prioritet för GitHub-agenter

1. **xAI** — renast, `.md`-vänligt, tools + Imagine
2. **OpenAI** — function calling + tools
3. **Anthropic** — tool use
4. **Google Gemini** — function calling, image, Veo
5. **ElevenLabs** — TTS (explicit agent-stöd med `/llms.txt`)
6. **Google Cloud** — Translation, YouTube (mer verbose)

---

*Senast uppdaterad: 2026-08-02*
