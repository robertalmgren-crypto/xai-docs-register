# xAI Documentation — Strukturerat Register

> Agent-vänlig översikt över xAI Model Capabilities, Files, Voice, Video, Advanced API och Community.
> Källa: sammanställd från officiell xAI-dokumentation.

**Senast uppdaterad:** 2026-07-26

**Publikt repo:** https://github.com/robertalmgren-crypto/xai-docs-register  
**Raw URL (för agenter):** https://raw.githubusercontent.com/robertalmgren-crypto/xai-docs-register/main/xai-docs-register.md

---

## Innehållsförteckning

1. [Persisting Generated Output](#1-persisting-generated-output)
2. [Referencing Files as Input](#2-referencing-files-as-input)
3. [Voice Overview](#3-voice-overview)
4. [Ephemeral Tokens](#4-ephemeral-tokens)
5. [Tools with Grok Speech-to-Speech](#5-tools-with-grok-speech-to-speech)
6. [Best Practices (Voice)](#6-best-practices-voice)
7. [Custom Voices](#7-custom-voices)
8. [Docs MCP Server](#8-docs-mcp-server)
9. [Files — Chat with Files](#9-files--chat-with-files)
10. [Managing Files](#10-managing-files)
11. [Public URLs](#11-public-urls)
12. [Asynchronous Requests](#12-asynchronous-requests)
13. [WebSocket Mode (Responses API)](#13-websocket-mode-responses-api)
14. [Video Generation (översikt)](#14-video-generation-översikt)
15. [Image-to-Video](#15-image-to-video)
16. [Video Editing](#16-video-editing)
17. [Reference-to-Video](#17-reference-to-video)
18. [Video Extension](#18-video-extension)
19. [Streaming](#19-streaming)

---

## 1. Persisting Generated Output

Använd `storage_options` på Imagine-requests (image/video) för att spara genererade assets till Files API och eventuellt skapa permanent public URL.

### `storage_options`-fält

| Fält | Typ | Beskrivning |
|------|-----|-------------|
| `filename` | string (**required**) | Filnamn. Extension styr public URL-path. |
| `expires_after` | integer (optional) | Sekunder tills filen raderas (3600–2592000). Utelämna = permanent. |
| `public_url` | boolean or object | `true` = default public URL. Object för egen `expires_after`. |
| `public_url.expires_after` | integer | Sekunder tills public URL upphör (1h–30d). Får ej överleva filen. |

### `file_output` i response

- `file_id` — stabil Files API-identifierare (alltid)
- `filename`, `expires_at`, `public_url`, `public_url_expires_at`
- `public_url_error` — vid partiellt fel (fil sparad, URL misslyckades)

### Viktiga beteenden

- Ephemeral URL returneras **alltid**, oberoende av `storage_options`.
- Public URL kan aldrig överleva filen.
- `n > 1` → varje asset får egen `file_id` och `public_url`.
- Fungerar på `/images/generations`, `/images/edits`, `/videos/*`.
- Max **1 000 aktiva public URLs** per team.
- Validering är synkron — ogiltig config avvisas före generation.

---

## 2. Referencing Files as Input

Ersätt public URL eller base64 med `file_id` från Files API. Servern hämtar filen privat.

**Användningsfall:**
- `image_file_id` / `image_file_ids[]` — redigera / blanda lagade bilder
- `image_file_id` — image-to-video (första frame)
- `video_file_id` — redigera lagrad video
- `reference_image_file_ids[]` — reference-to-video

**Krav:** Korrekt content-type (PNG/JPEG/WebP för bild, MP4 för video) och filen måste vara fullt uppladdad. Kan mixas med URL:er i samma request.

---

## 3. Voice Overview

xAI Voice APIs — Speech-to-Speech, Text-to-Speech, Speech-to-Text — drivna av Grok, enterprise-grade, sub-sekund latens.

- **Speech-to-Speech:** `wss://api.x.ai/v1/realtime?model=grok-voice-latest` + tool use + Ephemeral Tokens
- **TTS:** `POST /v1/tts` (unary eller WebSocket-streaming), inline speech tags
- **STT:** `POST /v1/stt`, 12 format, word-level timestamps, diarization, 25 språk
- **Enterprise:** SOC 2 Type II, HIPAA Eligible, GDPR. Audio lagras aldrig, används ej för träning.

---

## 4. Ephemeral Tokens

Kortlivade tokens för klient-sidan (browser/mobil) så API-nyckeln aldrig exponeras.

**Flöde:**
1. Server: `POST /v1/realtime/client_secrets` med API-nyckel
2. Server skickar token till klient
3. Klient autentiserar WebSocket med token
4. Token går ut efter `expires_after.seconds`

**Browser:** prefixa med `xai-client-secret.` i `sec-websocket-protocol`.

> Exponera aldrig API-nyckeln i klientkod.

---

## 5. Tools with Grok Speech-to-Speech

Konfigureras i `session.update`. Server-side tools körs av xAI; custom functions kräver klienthantering.

| Typ | Beskrivning |
|-----|-------------|
| `file_search` | Sök i Collections (`vector_store_ids`, `max_num_results`) |
| `web_search` | Webbsök (domains, location, image understanding) |
| `x_search` | X/Twitter-sök (handles, dates, media understanding) |
| `mcp` | Remote MCP-servrar (`server_url`, `server_label`, auth) |
| `function` | Custom JSON-schema — klient måste hantera response |

**Function call-flöde:**
1. `response.function_call_arguments.done`
2. Klient exekverar
3. `conversation.item.create` (function_call_output)
4. `response.create`

Parallella anrop: vänta på **alla** outputs innan `response.create`.

---

## 6. Best Practices (Voice)

- Starta WebSocket + mikrofonstreaming **parallellt**; buffra audio
- 24 kHz PCM16 little-endian, ~100 ms chunks
- Vid tool call: vänta tills playback är klar innan `response.create` (undvik audio-överlapp)
- Aktivera `server_vad` för naturlig barge-in
- Streama `response.output_audio.delta` direkt till speaker
- Föredra ephemeral tokens

---

## 7. Custom Voices

Klona röst från referensklipp (max **120 s**). Används överallt där built-in voice fungerar.

- **Tillgänglighet:** endast USA (ej Illinois). API create kräver Enterprise.
- **Rekommendation:** 90–120 s, tyst rum, en röst, matcha use case.
- **Limit:** 30 custom voices per team.
- **Endpoints:** `POST/GET/PATCH/DELETE /v1/custom-voices`, `GET .../audio`
- `voice_id` = 8 tecken, lowercase alphanumeric

---

## 8. Docs MCP Server

xAI hostar en MCP-server för direkt tillgång till officiell dokumentation.

- **Endpoint:** `https://docs.x.ai/api/mcp`
- **Transport:** Streamable HTTP (stateless)
- **Tools:** `list_doc_pages`, `get_doc_page` (slug), `search_docs` (query)
- **Klienter:** Cursor, Zed, Windsurf, OpenCode, valfri MCP-kompatibel klient

---

## 9. Files — Chat with Files

Bifoga dokument till chat → systemet aktiverar automatiskt `attachment_search` och gör requesten **agentisk**.

- Multi-file, multi-turn (kontext lever kvar)
- Kombinera med code execution för dataanalys
- **Max 48 MB/fil**
- Kräver agentiska modeller (t.ex. grok-4.5)
- Format: `.txt`, `.md`, kod, `.csv`, `.json`, `.pdf` + textbaserade
- Debitering per tool-invocation utöver token-kostnader

**vs Collections:** Files = omedelbar chat-kontext. Collections = persistent lagring + semantisk sök.

---

## 10. Managing Files

Komplett API: upload, list, get metadata, content (stream), delete.

- Upload från path / bytes / file handle. `purpose: "assistants"`
- **TTL:** `expires_after` (3600–2592000 s). **Måste komma FÖRE `file` i multipart body.**
- List: `limit`≤100, `order`, `sort_by`, `pagination_token`
- File object: `id`, `filename`, `bytes`, `created_at`, `expires_at`, `purpose`
- Max 48 MB. Filer scoped till team.

---

## 11. Public URLs

Permanent delbar CDN-länk (ingen API-nyckel). Revoke när som helst eller auto-expiry.

- `POST /v1/files/{id}/public-url` → `public_url`
- `POST .../public-url/revoke`
- Expiry: 1h–30d. Kan aldrig överleva filen.
- Idempotent create (max en aktiv URL per fil)
- **Max 50 MiB** för public URL
- Content-types: `image/png`, `image/jpeg`, `video/mp4`, `application/pdf`
- Max **1 000 aktiva** public URLs per team
- Radering av filen revokar automatiskt

---

## 12. Asynchronous Requests

Använd `AsyncClient` (xai_sdk) eller `AsyncOpenAI` för parallella requests.

- Kontrollera concurrency med `asyncio.Semaphore(max_in_flight)`
- `asyncio.gather` för parallell körning inom limit
- Höj timeout (t.ex. 3600s) för reasoning-modeller
- Respektera rate limits i API-konsolen
- Alternativ: **Batch API** för att köa och hämta senare

---

## 13. WebSocket Mode (Responses API)

Långlivad WebSocket till `wss://api.x.ai/v1/responses` istället för ny HTTP per turn.

- Efter första response: skicka endast nya input-items + `previous_response_id`
- Server håller state i minnet på socketen → fungerar med `store=false` och ZDR
- Warmup: `generate: false` för att förbereda state utan att köra modellen
- Upp till ~20 % lägre latency på agentiska workloads
- **En connection = seriella turns** (köar). Parallellt → flera connections.
- Max **25 minuter** per connection → `websocket_connection_limit_reached`
- `previous_response_not_found` om id saknas i cache och ej kan hydreras

---

## 14. Video Generation (översikt)

Asynkron process: start → poll. SDK `generate()` / `extend()` hanterar polling.

| Parameter | Värden |
|-----------|--------|
| `duration` | 1–15 sekunder (edit behåller original, max ~8.7s) |
| `aspect_ratio` | 1:1, 16:9, 9:16, 4:3, 3:4, 3:2, 2:3 (default 16:9) |
| `resolution` | 480p (default), 720p, 1080p (endast grok-imagine-video-1.5 + image-to-video). Edit max 720p |

**Status:** `pending` → `done` | `expired` | `failed`

**Fel:** `VideoGenerationError` — `invalid_argument`, `permission_denied`, `failed_precondition`, `service_unavailable`, `internal_error`

Concurrent: `AsyncClient` + `asyncio.gather`.

---

## 15. Image-to-Video

Animera stillbild utifrån prompt.

- Input: public URL, base64 data URI, eller `file_id`
- AI SDK: `prompt = { image, text }`
- Output aspect ratio defaultar till input-bildens; kan överskrivas (stretchar)

---

## 16. Video Editing

Prompt-baserad edit med stark scene preservation — ändrar bara det du ber om.

- Endpoint: `/v1/videos/edits` (SDK: `video_url=...` / `mode: "edit-video"`)
- `duration`, `aspect_ratio`, `resolution` **ärvs** från input (capped 720p)
- Concurrent edits från samma source för branching

---

## 17. Reference-to-Video

En eller flera referensbilder styr innehåll (personer, objekt, kläder) utan att låsa first frame.

- Use cases: virtual try-on, product placement, character-consistent storytelling
- Input: HTTPS URL / base64 / `file_id` — kan mixas
- AI SDK: `mode: "reference-to-video"` + `referenceImageUrls`
- I prompten: referera med `<IMAGE_1>`, `<IMAGE_2>`, ...

---

## 18. Video Extension

Fortsätt från sista frame. Resultat = original + extension (sammanhängande).

- Endpoint: `/v1/videos/extensions` (SDK: `client.video.extend` / `mode: "extend-video"`)
- `duration` = längd på **tillagd** del (inte total). Ex: 10s input + duration=5 → 15s output
- Samma polling-mönster som övrig video

---

## 19. Streaming

Server-Sent Events (SSE) för realtids text-output.

- Stöds av modeller med **text-output**. Stöds **inte** av image-generation.
- Sätt `"stream": true`
- Event streams med `delta.content`; avslutas med `data: [DONE]`
- SDK: `chat.stream()`, `for await chunk`, `streamText`
- **Höj timeout** (t.ex. 3600s) för reasoning-modeller så connectionen inte stängs för tidigt

---

## Relaterade officiella länkar

- Docs MCP: `https://docs.x.ai/api/mcp`
- Imagine / Files: `/developers/model-capabilities/imagine/files`
- Chat with Files: `/developers/model-capabilities/files/chat-with-files`
- Managing Files: `/developers/files/managing-files`
- Public URLs: `/developers/files/public-urls`
- Video: Generation, Image-to-Video, Editing, Extension, Reference-to-Video
- Streaming, Asynchronous Requests, WebSocket Mode, Batch API
- Voice / Custom Voices / Ephemeral Tokens

---

*Detta register är en sammanställning för agenter och utvecklare. För auktoritativ information, använd alltid officiell xAI-dokumentation via Docs MCP eller docs.x.ai.*
