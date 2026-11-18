# NOVA Architecture (project layer)

NOVA is the personal companion layer built on this N.E.K.O. fork. The first milestone is a polished Persian-speaking, anime-style 2D Live2D companion. Windows automation and unrestricted computer control are explicitly out of scope for this milestone.

## Product decision

- Avatar: 2D anime-style Live2D.
- Runtime foundation: this N.E.K.O. fork; do not replace its existing Live2D renderer.
- Conversation, memory, avatar presentation, and model providers remain separate contracts.
- Reuse upstream capabilities first. Add NOVA code only where a verified gap exists.
- Microphone, screen, and camera access must be opt-in, visibly active, and easy to stop.

## Verified upstream integration points

These are documented in the current upstream tree and were confirmed in this fork:

| Capability | Existing owner / contract | NOVA approach |
| --- | --- | --- |
| Main UI and Live2D renderer | `templates/`, `static/live2d/`; renderer modules include `live2d-core.js`, `live2d-model.js`, `live2d-emotion.js`, `live2d-interaction.js`, `live2d-init.js` | Keep the renderer; add a NOVA character/model and tune presentation |
| Emotion and motion | `window.LanLan1.setEmotion(name)`; `/live2d_emotion_manager` | Define a consistent emotion vocabulary and map it to model expressions/motions |
| Chat UI | `frontend/react-neko-chat/` (React 18 + TypeScript) | Extend the existing chat surface; do not create a competing chat implementation |
| Conversation transport | Main Server WebSocket `/ws/{lanlan_name}`; `OmniOfflineClient` for text and `OmniRealtimeClient` for audio | Preserve protocol and session manager; add only provider-neutral UX where needed |
| Speech output | `main_logic/tts_client`, provider registry and per-character TTS workers | Select a Persian-capable voice after testing; retain interruption and audio chunk contracts |
| Durable memory | `app/memory_server/`, `memory/`; recent history, facts, reflections, persona and hybrid recall | Use existing memory first; add NOVA-specific persona policy only if it does not duplicate upstream memory |
| Provider selection | `config/api_providers.json`, `config/api_profiles.py`, `utils/api_config_loader.py` | Keep the model/provider swappable; never hard-code a vendor into personality logic |
| Proactive behavior | Session manager proactive path and supported context sources | Start conservatively with user-controlled opt-in and quiet hours |
| Screen understanding | Screenshot/image messages and the configured vision role | Keep disabled by default; request consent and show a visible indicator |

## Target logical architecture

```text
NOVA desktop experience
  ├─ Live2D anime character (existing N.E.K.O. renderer)
  ├─ chat / captions / microphone controls
  └─ permission and activity indicators
           │
           ▼
N.E.K.O. Main Server + character session
  ├─ text and realtime audio sessions
  ├─ ASR / TTS / playback interruption
  ├─ expression + motion events
  ├─ optional screen-context input
  └─ proactive conversation scheduler
           │
           ├──────── Model adapter (swappable providers)
           │
           └──────── Memory Server
                        ├─ recent context
                        ├─ facts and reflections
                        └─ durable persona / relationship context
```

The diagram describes logical ownership, not a new process topology. Keep N.E.K.O.'s existing Main, Memory, and Agent service boundaries.

## Personality contract

The character definition should be data, not scattered prompt strings. NOVA's character profile should eventually contain:

- stable identity, values, boundaries, speaking style, humor, and conversational preferences;
- how to disagree, admit uncertainty, ask follow-up questions, and offer unsolicited ideas;
- Persian language and tone preferences;
- relationship context kept distinct from factual memory;
- rules for when proactive conversation is welcome or intrusive.

The LLM may improvise within this profile, but should not rewrite the stable identity from a single conversation. Memory updates should remain evidence-based and correctable.

## Emotion and animation contract

Use a small provider-neutral set of semantic events, for example:

- `neutral`, `happy`, `curious`, `thinking`, `surprised`, `sad`, `concerned`, `playful`;
- optional action hints such as `greet`, `listen`, `speak`, `idle`, `agree`, `disagree`.

Map those events to each chosen Live2D model's actual expressions and motions through the existing emotion manager. Missing assets must fall back gracefully to neutral. Lip-sync should use the existing supported audio/renderer path; do not assume every model has identical mouth parameters.

## Important boundaries and unknowns

- The repository documents eight UI locales, but Persian is not listed. Persian UI, prompts, ASR, and TTS need separate validation; language support must not be assumed from generic provider compatibility.
- N.E.K.O. documents screen/window understanding. This audit does not establish a complete live webcam-perception path. Treat webcam as a later, separately permissioned feature.
- Realtime audio and text are distinct session modes; the docs say a failed voice session does not automatically fall back to text. NOVA should explain failures clearly and offer an explicit fallback.
- A subscription to the ChatGPT consumer app is not an API credential. Provider adapters must use supported API/local endpoints and their own terms.
- Do not import Open-LLM-VTuber or Warashi wholesale. Compare specific components and licenses before adapting code.
- Do not add computer-control tools in the first milestone.

## Implementation rules

1. Inspect the current owner and tests before changing a behavior.
2. Prefer extending documented interfaces over patching generated frontend assets.
3. Keep secrets out of Git and logs.
4. Keep model provider, character identity, memory storage, and avatar assets independently replaceable.
5. Test Windows first, then smaller screens and performance.
