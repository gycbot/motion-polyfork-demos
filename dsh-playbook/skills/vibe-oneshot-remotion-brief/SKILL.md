---
name: vibe oneshot remotion brief
description: >-
  Use when turning an approved one-line vibe hook into a 15s Remotion storyboard
  brief (no empty prerender; render only after user-approved topic card).
---
# vibe oneshot remotion brief

## When
User-approved one-line vibe hook → structured 15s Remotion storyboard brief → Composition.
**Do not** prerender with empty/placeholder hook. Render only after user-approved topic card (§26).

## Hard gates
1. Hook text must come from a **user-approved topic card** (captain written exemption only if explicitly granted). Unapproved: brief only. See OPERATING §26 + `team/queue/artifacts/2026-10-26-topic-user-gate.md`.
2. No 15s empty-hook prerender (token burn).
3. No new installs (no Claude Code / HyperFrames / video-talkcraft runtime / showcase Playwright as prod deps); use already-installed Remotion plugin 49339676 only.
4. No team AI watermark on frames; platform AI tick is ops-only at publish.
5. BGM: flat first → climax on the **aha second** (never climax at open). HANDOFF BGM_NOTE: flat-start sec + climax-align sec.
6. Direction words in VO/subs must match on-screen pointer in the same frame (or mark N/A).
7. Hook ≤12 chars preferred; must land by **3s**; ban cloudy geo trivia; aha second required.
8. CRAFT: platform fit > de-AI > premium; **ban** black/neon glow particles.
9. MOTION (premium-motion rules, adopted 2026-10-26; source `team/learning/2026-10-26-karla-ui-scrolltide-premium-motion.md`): "low framerate" complaints are usually missing easing/settle/blur, not missing fps.
   - Spatial moves/rotations never `linear`: use a **named** easing (easeOutCubic / cubic-bezier(0.45,0,0.2,1) / spring damping 14–16, stiffness 90–110). Progress bars/infinite spinners exempt.
   - Every step/cut gets a **settle plateau**: ~0.6s move + ~0.5s hold; 0.15–0.25s near-still after peak; no second bounce peak.
   - Fast moves (big UI rotation, long list travel) get directional/radial **motion blur**; "already 60fps" is not a pass.
   - No flat solid background as the only premium cue; use material/texture background.

## Steps
1. Copy the **checkbox skeleton** from `team/queue/artifacts/2026-10-26-opus-oneshot-storyboard-template.md` into this job's brief path.
2. Fill: approved hook, aha second, fact anchor, style lock (retro encyclopedia), beats table with columns: **timecode · picture · subtitle · motion (markup + easing name) · settle sec · blur Y/N · BGM hit · direction**.
3. Tick hook-window checks: on-screen by 0.0s, clear by 3s, ≤12-char preference.
4. Write BGM_NOTE: track/source, flat-start sec, climax-align sec (= aha). First key frame of aha beat = climax hit.
5. Anti-slideshow: each beat ≥1 visible change (no static hold filling duration).
6. Implement Remotion Composition from brief props (`hook`, `beats`, `ahaAtSec`, `subs`). Default 9:16 1080×1920; no narration. Whitelist: remotion-captions, remotion-markup, remotion-render (+ best-practices router).
7. Cover: still from render or cover-design collab — no new plugin.
8. Before render: `STANDING_PRODUCE_CHECKLIST` + edit-wiki INDEX + ≥1 principle + `edit-from-evidence-team`. Scrub one fast move frame-by-frame: easing visible, settle present, blur on.
9. After render: HANDOFF with path, md5, v/a/container durations, wiki pages, BGM_NOTE, direction checkbox, anti-slideshow tick, **motion three ticks (easing name / settle / blur)**.
10. Mark produce complete only; review gate separate. Never publish.

## Out of scope
Finance ECB fills; overnight vibe under render freeze; paid Opus/HyperFrames/talkcraft/Scrolltide/Scrollshaders; OpenMontage clone.
