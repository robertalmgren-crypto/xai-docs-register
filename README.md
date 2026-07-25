# xAI Documentation — Strukturerat Register

Agent-vänlig översikt över **xAI Model Capabilities**, Files, Voice, Video, Advanced API och Community.

Sammanställd från officiell xAI-dokumentation. Avsedd för **alla** agenter och utvecklare (inte bara Grok).

## Snabblänkar för agenter

| Resurs | URL |
|--------|-----|
| **Repo** | https://github.com/robertalmgren-crypto/xai-docs-register |
| **Raw Markdown (fetch)** | https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md |
| **HTML-vy** | https://github.com/robertalmgren-crypto/xai-docs-register/blob/main/xai-docs-register.md |

## Hur agenter hämtar registret

```bash
# Valfri agent / script
curl -sL https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md
```

```python
import urllib.request
url = "https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md"
text = urllib.request.urlopen(url).read().decode()
```

MCP-kompatibla klienter kan också peka på den råa URL:en eller clona repot.

## Innehåll

1. Persisting Generated Output (`storage_options`, public URL)
2. Referencing Files as Input (`file_id`)
3. Voice Overview (S2S, TTS, STT)
4. Ephemeral Tokens
5. Tools with Grok S2S
6. Best Practices (Voice)
7. Custom Voices
8. Docs MCP Server (`https://docs.x.ai/api/mcp`)
9. Files — Chat with Files (`attachment_search`)
10. Managing Files (upload, TTL, list, delete)
11. Public URLs
12. Asynchronous Requests
13. WebSocket Mode (Responses API)
14. Video Generation
15. Image-to-Video
16. Video Editing
17. Reference-to-Video
18. Video Extension
19. Streaming

Se **[xai-docs-register.md](xai-docs-register.md)** för detaljer.

## Officiell dokumentation

- [Docs MCP](https://docs.x.ai/api/mcp) — `list_doc_pages`, `get_doc_page`, `search_docs`
- [docs.x.ai](https://docs.x.ai)

---

*Senast uppdaterad: 2026-07-26*
