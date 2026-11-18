# NOVA — Project Overview

NOVA is a personal, Persian-first virtual companion built on the user's N.E.K.O fork.

## Product decisions
- Initial appearance: 2D anime-style Live2D.
- The avatar must be replaceable later (VRM 3D, MMD, or PNGTuber) without replacing identity, personality, memory, or model provider.
- First milestone: conversation and character experience only. Windows automation is out of scope.
- Text chat first, then voice, with explicit microphone, camera, and screen permissions.

## Experience goals
1. Natural Persian text and voice conversation.
2. Stable identity, values, temperament, humor, preferences, and respectful disagreement.
3. Live2D lip-sync, idle motion, blinking, gaze, expressions, and emotion-linked gestures.
4. Interruptible speech (barge-in), turn-taking, and clear listening/speaking states.
5. Optional proactive conversation with quiet hours, rate limits, and user controls.
6. Per-character long-term memory with inspect, correct, and delete/export controls.
7. Optional screen understanding and separate webcam understanding, both opt-in and visibly indicated.
8. Replaceable LLM, ASR, and TTS providers. A ChatGPT consumer subscription is not API access.

## Upstream foundation
NOVA reuses N.E.K.O.'s existing runtime rather than rebuilding it. The current repository documents Live2D, VRM, MMD, and PNGTuber; Live2D emotion mapping; realtime audio and text modes; per-character memory; proactive conversation; provider configuration; and plugin extension points.

See UPSTREAM_AUDIT.md for verified paths and limitations.

## First-milestone non-goals
- Autonomous control of Windows applications.
- Arbitrary file access or shell execution.
- Always-on webcam or screen recording.
- Automatic cloud upload of private conversations.
- Building a new LLM, speech recognizer, or avatar renderer from scratch.

## Quality principles
- Persian correctness and naturalness are release requirements.
- The character must not pretend to see, hear, remember, or do something without the relevant input or tool result.
- Memory must distinguish confirmed facts from uncertain inferences.
- Proactive messages must be useful, infrequent, and easy to disable.
- Visual assets and voices must have documented usage rights.
