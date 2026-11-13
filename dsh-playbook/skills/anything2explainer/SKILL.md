<!-- LICENSE WARNING (pack): 本技能及官方工具包为 PolyForm Noncommercial 1.0.0。
非商用可免费使用；任何商用须事先取得作者书面授权。
本仓库只收录这份团队入口 SKILL.md，不含工具包源码、模板、node_modules 或媒体。
官方仓库：https://github.com/Vincentwei1021/anything2explainer
本对照任务（1 分钟精剪）是对已有成片的再剪，不是从零做科普 explainer，禁止把本技能当主路径。 -->

---
name: anything2explainer
description: >-
  Use when making a knowledge, explainer, 科普, or vibe知识 video, or turning a
  topic or article into a narrated motion-graphics explainer. Start with this
  skill to break the topic down (research, narration, chapters, storyboard)
  before any other writing or rendering.
---
# anything2explainer

Turn a topic or article into an original narrated explainer. This skill comes first: finish the breakdown (research, narration, chapters, storyboard) before other visual or hook skills, and before any render.

The official toolkit (Remotion template, figure library, TTS/timeline scripts, QC, and the RAG sample) lives in the `anything2explainer` checkout that contains `SKILL.md`, `reference/`, `template/`, and `examples/rag/`. If it is already on the machine, use that checkout. If it is missing, clone `https://github.com/Vincentwei1021/anything2explainer` and work from there. After locating it, also read its official `SKILL.md` and the files under `reference/` — those are the detailed rules. This skill is the start-of-job gate and the license/order contract.

## License

The toolkit is PolyForm Noncommercial 1.0.0: noncommercial use is free; any commercial use of the toolkit needs prior authorization from the author. Use it only for noncommercial sharing. If the film will later be used for membership fees, paid work, or other commercial use, stop and get the author's written authorization before using this toolkit's code, templates, or scripts. Do not copy the license into a deliverable as if it were yours, and keep the toolkit LICENSE next to any local checkout.

## When to use

- A knowledge, explainer, 科普, or vibe知识 video, or an article/document to turn into one.
- Do not use this skill as the main path for live-action, talking-head, pure UI product demos, or shot-for-shot replicas of someone else's video.

## Do this first (breakdown, no render)

1. **Lock duration and language** from the brief if those are already set. If they are not, ask once and do not default. Duration drives length, chapter count, and shot count: under 3 minutes is one chapter (no chapter cards); 3–5 minutes is 3–4 chapters of at least 60s each; 5–8 minutes is 4–6 chapters.
2. **Research** following `reference/research-brief.md`. Write a research note covering the situation, how it works, contrasts, disputes, a real example, a list of numbers/metaphors, and a glossary. Every number, English term, year, and person that will appear on screen or in the voiceover must have a source URL. Unverified facts stay off screen and out of the VO. Do not invent engagement stats.
3. **Narration** following `reference/narration-guidance.md` and `reference/narration-storyboard.md`. One through-line and one running example. Open from a situation, cause before effect, turn numbers into a felt scale, and hook each chapter to the next. Format: `# CHAPTER n title` (chapter titles are navigation labels, Chinese ≤6 characters), blank lines start a new shot (2–4 sentences), `|` splits subtitle chunks (≤16 Chinese characters). Chapter-end leaves a hook; chapter-open points back before starting the new point.
4. **Storyboard** from the narration: shots are picture units, not sentences. Short lines join a neighbor. Each shot is at least 120 frames. List beats, subject and size, light, sustained action, and a 30–45 frame hold at the end. At most one Glitch per shot, and only on the key term. Full-piece light-sweeps ≤2, written on a whitelist.
5. **Stop for copy review** after research + narration + storyboard (and titles if the job needs them). Do not run TTS, do not render, and do not lock frame numbers until the owner approves the copy. After approval, do not change wording without a new review — frame numbers are hardcoded to the locked script.

## After copy approval

Follow the official toolkit flow: build or reuse a project from `template/scripts/new_project.sh`, generate the timeline from the locked narration, preview the first 30 seconds, then build remaining shots, render, and QC with `selfcheck.py`, `motion_check.py`, and `frame_metrics.py`. Check disk space first (plan for about 2GB plus 5GB free).

Voice: if the brief or house voice is already specified, use that and do not change speed with `atempo` to force a duration. If none is specified, follow the official toolkit defaults.

Visual: follow this skill's composition and motion rules. If the brief locks a different house style, keep the breakdown and QC rules and swap only the visual skin. Credit the look as style homage, not a copy of another account. Screen images are drawn in code; do not use frames from existing videos. Optional B-roll must be rights-cleared and listed in a manifest.

## Hard rules (from the official skill)

- Facts on screen must be traceable to the research note.
- One visual focus per frame; the hero is large enough and lit; no debris field in the background.
- Elements may not sit fully still for more than 3 seconds; do not fake motion with bobbing or breathing; hold 30–45 frames after the last element lands, then leave.
- Subtitle band and chapter progress bar stay clear of content.
- Chinese speech rate stays natural (about 5.5 characters/sec, no speed-up). If finished length is more than 15% off the brief, add or cut sentences and rebuild — do not stretch or squeeze the voice.

## Quality bar

Use `examples/rag/` (the RAG sample) and `examples/contrast/` as the quality and counterexample bar before you start drawing shots.
