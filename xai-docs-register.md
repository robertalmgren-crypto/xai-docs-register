# xAI Documentation — Strukturerat Register

> Agent-vänlig översikt över xAI Model Capabilities, Files, Voice, Video, Advanced API och Community.
> Källa: sammanställd från officiell xAI-dokumentation.

**Senast uppdaterad:** 2026-07-26

**Publikt repo för alla agenter:** https://github.com/robertalmgren-crypto/xai-docs-register

**Raw URL (för fetch):** https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md

---

## Innehållsförteckning

1. Persisting Generated Output
2. Referencing Files as Input
3. Voice Overview
4. Ephemeral Tokens
5. Tools with Grok Speech-to-Speech
6. Best Practices (Voice)
7. Custom Voices
8. Docs MCP Server
9. Files — Chat with Files
10. Managing Files
11. Public URLs
12. Asynchronous Requests
13. WebSocket Mode (Responses API)
14. Video Generation
15. Image-to-Video
16. Video Editing
17. Reference-to-Video
18. Video Extension
19. Streaming

---

## 1. Persisting Generated Output

Använd `storage_options` på Imagine-requests för att spara assets till Files API + valfri public URL.

**Fält:** filename (required), expires_after (1h–30d), public_url (bool|object), public_url.expires_after

**file_output:** file_id, filename, expires_at, public_url, public_url_expires_at, public_url_error

**Viktigt:** Ephemeral URL alltid. Public URL kan aldrig överleva filen. Max 1000 aktiva public URLs/team. Synkron validering.

## 2. Referencing Files as Input

Ersätt URL/base64 med `file_id`. Privathämtning server-side.

image_file_id, image_file_ids[], video_file_id, reference_image_file_ids[] — kan mixas med URL:er.

## 3. Voice Overview

S2S (wss realtime), TTS, STT. Enterprise: SOC2, HIPAA, GDPR. Audio lagras aldrig.

## 4. Ephemeral Tokens

POST /v1/realtime/client_secrets. Browser: xai-client-secret. i sec-websocket-protocol. Exponera aldrig API-nyckel i klient.

## 5. Tools with Grok S2S

file_search, web_search, x_search, mcp, function. Function flow: function_call_arguments.done → execute → conversation.item.create → response.create. Vänta på alla parallella outputs.

## 6. Best Practices (Voice)

Parallell WS+mic, buffra. 24kHz PCM. Vänta playback vid tool calls. server_vad. Streama deltas.

## 7. Custom Voices

Max 120s klipp, 90–120s rekommenderas. USA only (ej Illinois). API create = Enterprise. 30 röster/team. voice_id 8 tecken.

## 8. Docs MCP Server

https://docs.x.ai/api/mcp — Streamable HTTP, stateless. Tools: list_doc_pages, get_doc_page, search_docs. Cursor/Zed/Windsurf/OpenCode.

## 9. Files — Chat with Files

attachment_search aktiveras automatiskt. Agentic. Multi-file, multi-turn. Max 48 MB. Agentiska modeller. Format: txt/md/kod/csv/json/pdf.

## 10. Managing Files

Upload/list/get/content/delete. TTL expires_after MÅSTE före file i multipart. Max 48 MB.

## 11. Public URLs

CDN-länk utan API-nyckel. Revoke. Expiry 1h–30d. Max 50 MiB. png/jpeg/mp4/pdf. Max 1000/team. Idempotent create.

## 12. Asynchronous Requests

AsyncClient + Semaphore. asyncio.gather. Höj timeout för reasoning. Batch API som alternativ.

## 13. WebSocket Mode (Responses)

wss://api.x.ai/v1/responses. previous_response_id. generate:false warmup. ZDR/store=false OK. ~20% lägre latency. 25 min limit. previous_response_not_found.

## 14. Video Generation

duration 1–15s. aspect_ratio 1:1/16:9/9:16/…. resolution 480p/720p/1080p. Status: pending/done/expired/failed. VideoGenerationError codes. Concurrent med AsyncClient.

## 15. Image-to-Video

URL / base64 / file_id. AI SDK prompt={image,text}. Aspect defaultar till input.

## 16. Video Editing

/v1/videos/edits. Scene preservation. duration/aspect/resolution ärvs (max 720p). Concurrent branching.

## 17. Reference-to-Video

Referensbilder utan att låsa first frame. virtual try-on, product placement. Mix URL/base64/file_id. <IMAGE_n> i prompt.

## 18. Video Extension

/v1/videos/extensions. duration = tillagd längd (inte total).

## 19. Streaming

SSE, stream:true. Text-modeller only. Höj timeout för reasoning (3600s).

---

## Officiella länkar

- Docs MCP: https://docs.x.ai/api/mcp
- docs.x.ai (Imagine, Files, Voice, Video, Advanced)

*Sammanställning för agenter. Auktoritativ källa: officiell xAI-dokumentation.*
