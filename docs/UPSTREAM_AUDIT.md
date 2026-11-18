# N.E.K.O Upstream Audit for NOVA

This audit uses the current fork's tracked source tree and upstream developer documentation. It distinguishes documented architecture from runtime behavior still requiring Windows tests.

## Verified repository paths
- app/main_server/character_runtime.py — per-character runtime/session ownership.
- app/memory_server/ — independent memory service.
- main_logic/core/ — session coordination.
- main_logic/omni_realtime_client/ — realtime voice client.
- main_logic/omni_offline_client/ — text/vision client.
- main_logic/tts_client/ — TTS worker/client layer.
- main_logic/proactive_chat/ — proactive interaction pipeline.
- memory/ — memory implementation.
- config/character_defaults.py and config/character_fields.py — character defaults and reserved avatar/voice fields.
- static/live2d/ — Live2D renderer modules.
- frontend/react-neko-chat/ — shared React chat UI.
- docs/frontend/live2d.md — Live2D runtime and model management contract.
- docs/api/websocket/audio-streaming.md — audio transport and interruption contract.
- docs/architecture/memory-system.md — per-character memory architecture.

## Capability assessment

| Capability | Evidence | NOVA assessment |
|---|---|---|
| Live2D anime avatar | static/live2d/ and docs/frontend/live2d.md | Existing foundation; test model import/runtime on Windows |
| Change avatar format | Character schema has avatar/model type fields; upstream documents Live2D, VRM, MMD, PNGTuber | Formats documented; test switching while preserving identity/memory |
| Expressions and motions | live2d-emotion.js; /live2d_emotion_manager; emotion mapping API | Existing foundation; map NOVA emotion labels |
| Voice conversation | main_logic/omni_realtime_client/ and audio WebSocket docs | Existing realtime path; provider-dependent |
| Text conversation | main_logic/omni_offline_client/ | Existing text path |
| Barge-in | speech_id, playback queue filtering, interruption docs | Protocol documented; measure actual latency |
| Long-term memory | app/memory_server/, memory/, memory architecture docs | Rich existing pipeline; test Persian extraction/recall |
| Proactive conversation | main_logic/proactive_chat/ and config/proactive_settings.py | Existing subsystem; tune to avoid intrusive behavior |
| Screen understanding | realtime screen media path and offline image path documented | Existing input path; test permissions/provider |
| Live webcam understanding | docs mention camera frames in supported visual inputs | Continuous webcam behavior not yet verified; do not advertise as ready |
| Persian UI | documented locale list omits Persian | Missing; add fa localization and RTL |
| Persian ASR/TTS | provider-dependent | Must benchmark selected providers |
| Windows support | project docs list Windows/source support | Must perform clean Windows installation and smoke tests |

## Important runtime facts
1. Text and realtime audio are separate session modes; a failed realtime session does not silently become text mode.
2. Audio input uses PCM16 binary WebSocket frames in the documented first-party protocol.
3. Server audio output is a header followed by binary audio chunks. speech_id helps discard stale audio after interruption.
4. The Live2D renderer is shared with model preview and initialized from the main page; do not create a second competing renderer for a new chat window.
5. Live2D models use Cubism .model3.json manifests and referenced model, texture, motion, expression, and optional physics files. Keep relative paths intact.
6. Memory is per character and separates recent context, facts, pinned facts, reflections, persona, and chronological backing history.

## Known gaps and cautions
- Upstream UI locale list currently does not include Persian.
- The default character example is not NOVA's final identity or visual design.
- Provider support varies; OpenAI-compatible configuration does not prove realtime voice, Persian speech, vision, or tool support.
- Camera-frame message types do not prove a polished always-on webcam feature.
- Model and voice asset licenses must be checked individually. Do not assume all Workshop or bundled assets can be redistributed.
- Documentation/code presence is not a substitute for Windows runtime testing.

## Primary upstream references
- docs/frontend/live2d.md
- docs/api/websocket/audio-streaming.md
- docs/architecture/memory-system.md
- docs/architecture/tts-pipeline.md
- docs/architecture/index.md
- docs/guide/quick-start.md
- docs/guide/local-and-offline.md
- docs/api/rest/characters.md
- docs/api/rest/live2d.md


## Localization implementation details

The main frontend's current locale contract is documented in docs/frontend/i18n.md. It loads exactly eight locale JSON files from static/locales, and the documentation requires all eight to remain in sync when changing shared UI strings. The React chat source has its own i18n adapter at frontend/react-neko-chat/src/i18n.ts. Prompt locale normalization is centralized in config/prompts/_locale.py, whose NEKO_CORE_LOCALES currently contains zh-CN, zh-TW, en, ja, ko, ru, es, and pt. Therefore Persian support is a cross-layer change, not just adding a translated JSON file: locale selection/normalization, main UI, React chat, prompt templates, character defaults, and RTL layout all need coordinated work and tests.

## Existing Live2D quality tests

The repository includes focused tests such as tests/frontend/live2d_expression_lipsync.test.cjs, tests/frontend/live2d_audio_ownership.test.js, tests/frontend/live2d_lipsync_ownership.test.js, and tests/frontend/live2d_mode_selection_restore.test.cjs. These are useful regression gates when changing avatar behavior. Their presence does not mean the NOVA model has already been tested on the user's Windows machine.
