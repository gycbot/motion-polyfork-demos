---
name: narration-av-sync
description: >-
  Use when adding voiceover to Remotion/explainer or match-review videos so
  picture follows natural VO timing—never rate-stretch speech to fit locked
  slots. Also when fixing 棒读/断续碎切 and needing emotion-full per-sentence takes
  (pairs with voice-find-and-emotion).
---
# narration-av-sync

Hard rule (t188u → **t2268u SUPERSEDE**): voice and picture share one axis. **Picture matches voice.** Do not speed up/slow down TTS to chase pre-locked slots (ugly stop-start pacing).

找声/气质/IndexTTS 情绪流程见 **`voice-find-and-emotion`**（本 skill 管「时轴与禁凑长」）。

## Write for the ear (pre-step · 2026-11-04 · distilled from tts-director MIT, no tags)
Before any TTS, rewrite the script for listening; keep every fact.
- Short sentences; one idea per sentence. Split long written clauses.
- Put the punch word at the end of the sentence.
- Spoken register: contractions / 口语词，对一个观众说「你」，不说「用户们」。
- Punctuation is the breath score: 逗号=换气，「……」/「——」=停顿；不用 SSML。
- No opening greeting; first words carry the hook.
- Vary sentence openers; no 「!」 on every line.
- Spell out what TTS may misread (数字、英文缩写、网址读法)。
- Say numbers, dates, names and CTA keywords plainly and unhurried.
- Show the rewritten text for approval before synthesizing.
- Pacing still comes from natural-rate TTS + picture follows voice (rules below); never fix pace with rate hacks.

## 情绪与连贯（老板硬钉 · 2026-11-10）
- **禁棒读**：平调、每字等重、无起伏 → 不过耳审。
- **禁断续碎切**：一句内不无故切开再拼；不按镜头硬切口气。
- **按句情绪 take**：紧张/鼓劲/惊慌/凝重等写进合成标签；同一角色全片同一 ref。
- 中文对白字幕时间跟波形；专名以脚本为准；舞台提示词不念。

## Design-first (preferred)
1. Write narration in natural spoken rhythm first; synthesize per-sentence TTS at **default rate** (no slot-fitting rate hacks).
2. Measure each clip’s real duration → build `SUBS`/`SEG` (and shot lengths) from those durations.
3. Only then lock Composition frames and render.
4. Spot-check 3 timestamps: on-screen text matches spoken words that second.

## If picture already exists
1. Still synth per slot/sentence at natural rate (forbid whole-file one-shot TTS paste).
2. If VO longer than shot → **lengthen/hold/cut picture** (extend SEG/`from`–`to`), do not raise `--rate`.
3. If VO shorter → shorten the shot or add a motivated visual beat; trailing silence pad only as a last resort and never as the default rhythm.
4. Concat → wire `<Audio>` from 0 → render.

## Forbidden
- Whole-track TTS hard-pasted onto timed comps.
- Stepwise `--rate` / pitch-stretch to force-fit slots.
- Shipping samples with uneven sped-up then slowed speech.
- Flat/choppy 「棒读」delivery shipped without pure-listen pass.

## UI (t2268u)
For match-review / narrative spoiler-sensitive cuts: **no Chapter card**, **no bottom progress bar** (they spoil structure). Explainer defaults: hide unless the brief explicitly asks for chapter UI.

## Related
- `voice-find-and-emotion`
- Overlay X1 / **X1b** in `explainer-avp-mechanism-overlay.md`
- Standing: `STANDING_NARRATION_AV_SYNC_T188.md` (APPEND t2268u)
- Distill source (readonly): `team/vendor/_read/aligh98-tts-director/` · ElevenLabs tags unused
