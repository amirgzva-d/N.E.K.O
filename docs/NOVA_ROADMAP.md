# NOVA Roadmap

This roadmap applies to the NOVA layer on this N.E.K.O. fork. It intentionally starts with a companion that can talk and express itself; Windows automation is a later project phase.

## Milestone 0 — Baseline and safe launch

- [x] Create the user's N.E.K.O. fork and confirm the upstream source tree is present.
- [x] Confirm the project currently requires Python `==3.11.*`.
- [x] Confirm existing Live2D renderer, emotion manager, React chat, voice/session, provider, and memory architecture from repository documentation.
- [x] Record NOVA's chosen anime Live2D architecture.
- [ ] Set up a clean Windows development environment and run the unmodified fork.
- [ ] Record startup errors, hardware requirements, and a reproducible baseline before code changes.

**Exit condition:** the unmodified fork launches on the target Windows PC and the default Live2D character renders.

## Milestone 1 — A working Live2D companion

- [ ] Select or create a legally usable anime Live2D model with a complete Cubism resource package.
- [ ] Load it through the existing model manager; preserve its relative asset paths.
- [ ] Create a NOVA character card with a stable identity and Persian-first instructions.
- [ ] Map the semantic emotion set to expressions and motions actually present in the selected model.
- [ ] Verify idle animation, blinking, model scaling, drag/placement, and graceful neutral fallback.
- [ ] Keep the default upstream character and assets available as a recovery option.

**Exit condition:** NOVA appears as a desktop companion and visibly reacts to basic conversation states.

## Milestone 2 — Persian conversation

- [ ] Validate Persian text chat with at least two supported model/provider routes.
- [ ] Test Persian ASR and TTS separately; measure recognition quality, pronunciation, latency, and voice naturalness.
- [ ] Verify streaming output and audio playback on Windows.
- [ ] Test interruption/barge-in end-to-end: user speech stops current assistant playback and is accepted as the next turn.
- [ ] Add clear UI states for listening, thinking, speaking, muted, and connection failure.
- [ ] Provide an explicit text fallback if realtime voice cannot start.

**Exit condition:** natural Persian text and voice conversations work reliably, including interruption.

## Milestone 3 — Personality and memory

- [ ] Author a stable NOVA persona: values, boundaries, humor, tone, independent opinions, and respectful disagreement.
- [ ] Separate stable character identity from user facts and relationship memories.
- [ ] Verify memory capture, recall, correction, deletion/forgetting behavior, and contradiction handling.
- [ ] Test continuity after app restart and after changing the LLM provider.
- [ ] Add anti-repetition and uncertainty behavior to proactive suggestions.

**Exit condition:** the same recognizable character remains consistent across sessions and provider changes.

## Milestone 4 — Natural avatar performance

- [ ] Synchronize mouth movement with the actual audio playback timeline.
- [ ] Coordinate facial expression, motion, and spoken tone without conflicting animations.
- [ ] Add listening gaze/idle behavior and context-aware reactions.
- [ ] Profile CPU/GPU use and reduce idle rendering cost.
- [ ] Validate the chosen model's license and redistribution permissions.

**Exit condition:** speech, mouth movement, emotion, and body motion feel like one coherent performance.

## Milestone 5 — Proactive companion and screen awareness

- [ ] Add user-configurable proactive conversation, quiet hours, and cooldowns.
- [ ] Start with explicit user prompts and opt-in context sources; avoid unsolicited screen capture.
- [ ] Add screen/window awareness only after consent, visible capture status, and a reliable stop control are tested.
- [ ] Keep webcam perception out of scope until a separate design, permission flow, and supported implementation are verified.

**Exit condition:** NOVA can initiate useful conversation without becoming intrusive, and perception is transparent and controllable.

## Later phase — Desktop actions

Windows automation, file operations, browser actions, and application control are deliberately deferred. They require a separate permission model, action previews, confirmations for consequential actions, audit logs, and an emergency stop.

## Current status

The repository architecture and roadmap documents have been added. No runtime code has been modified yet, and no Windows launch or feature test has been claimed. The next engineering task is to establish a clean Windows baseline before making code changes.
