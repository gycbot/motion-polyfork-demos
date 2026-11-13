---
name: de-ai-zh
description: >-
  Use when editing Chinese (or EN) copy, scripts, captions, or UI text to remove
  AI-slop and template tone while keeping facts—Humanizer-zh / stop-slop /
  shuorenhua / taste path.
---
# De-AI zh pipeline

Goal: remove AI writing/UI slop without inventing facts or erasing the author's voice.

Vendor originals (read when needed):
- ZH rewrite: `<OPTIONAL_VENDOR_DEAI>`
- ZH say-it-plain: `<OPTIONAL_VENDOR_DEAI>`
- EN slop cut: `<OPTIONAL_VENDOR_DEAI>` (+ phrases.md / structures.md)
- EN humanizer: `<OPTIONAL_VENDOR_DEAI>`
- Style distill (optional): `<OPTIONAL_VENDOR_DEAI>`
- UI anti-slop: `<OPTIONAL_VENDOR_DEAI>` (+ tells.md)

## Pipeline

1. **Detect** — mark AI tells (宏大空开头、首先其次综上所述、三段排比、假互动结尾、not-X-but-Y、三联金句、过密破折号、平均正确无立场).
2. **Pick lane**
   - 中文稿 → Humanizer-zh rules, then shuorenhua pass if still stiff.
   - 英文稿 → stop-slop then humanizer.
   - 界面/封面网页感 → taste-skill tells (禁紫渐变三卡玻璃拟态默认).
3. **Rewrite** — keep facts, hedges, numbers, names; cut filler; vary rhythm; prefer concrete verbs.
4. **Voice** — if user samples exist, match them (nuwa-style). Else keep input register. Never fabricate personal anecdotes.
5. **Diff check** — no new claims; no stronger certainty; terminology intact for tech/内推.

## Output

Return: (a) revised text, (b) 3–8 bullet tells removed, (c) any fact you refused to invent.

## Don't

- Don't aim to "beat AI detectors".
- Don't add 赋能/闭环/打造/在当今时代.
- Don't flatten account hype hooks the user explicitly wants (exaggeration for covers may stay if labeled).
- Don't replace film/aesthetic SoT colors via taste alone when a standing film palette exists.
