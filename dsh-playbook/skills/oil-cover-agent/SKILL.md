---
name: oil-cover-agent
description: >-
  Use when generating Xiaohongshu or Bilibili covers from a real finished video
  frame (evidence cover), especially AI-tool demo videos. Prefer agent-native
  mode without paid vision API keys.
---
# oil-cover (agent-native)

## When
Need a polished cover for Xiaohongshu or Bilibili from an **existing** video; user or contract names oil-cover / evidence cover / 真实画面封面.

## Default mode
**agent-native**: use the host's vision + image tools. **Do not** require or default to a paid external vision/ZenMux key unless the owner authorized script mode for that run. Do not install or suggest Oil UI Pro for covers.

## Standing
Read `standing/STANDING_COVER_OIL_T2005.md` when present (oil-cover path + oil-ui method distill + tone checkpoints + oil-visual pilot bounds). Still forbid plain first-frame screenshots as growth covers (HYPE_COVER). Teaching-安利 line t1876「原图直用」is an exception and does **not** apply to drygoods/evidence covers.

Realism distill (2026-11-05 scout land, **no vendor install**): `team/queue/artifacts/2026-11-05-oil-cover-realism-APPEND.md`.

## Steps
1. Locate vendor skill at `team/vendor/t1810-self-learn/oil-cover/` (or the team's current oil-cover path). Read `SKILL.md`, `references/cover-rules.md`, and `references/agent-native-flow.md`.
2. Reuse existing accepted covers if video/title/aspect unchanged.
3. Pick a clear evidence frame (readable UI / result), not a blank template shell. Write `analysis.md` before generating.
4. **Realism 五杠杆（GenerateImage / 修图 prompt 前勾）** — 写物理，不写 vibe；禁 hyperrealistic / 8k / cinematic / masterpiece 等塑感词。
   1. **出处**：谁拍的、什么设备/情境（证据封面优先 `screen grab from finished demo video frame`）。
   2. **光线**：只写一个真实光源及落点（例 window light from camera-left）；禁棚灯/香水广告光。
   3. **构图**：画幅比、机位距离、允许一个普通相机瑕疵；平台比按站规。
   4. **物理细节**：每主体 2–3 锚点（纹理/磨损/界面像素）；禁磨皮脸、塑料高光。
   5. **排除项**：文末 3–5 条字面排除（No studio lighting / No color grading / No retouching / No watermark spam / No income promise text unless owner-authorized）。
5. Produce required aspects for the target platform (commonly 3:4 XHS and/or landscape Bilibili). Keep title text short, high contrast, true to the video; strip income/「免费」promises unless owner-authorized facts.
6. Creator avatar: default off. If config enables it, composite locally after generation; do **not** reserve portrait safe-areas in the base prompt, and do not treat avatar occlusion as a redo gate (upstream 2026-10-28).
7. Save outputs next to the video or the contract's artifact folder; report paths + one-line why the frame was chosen. SAMPLE≠ship.

## Stack
If oil-cover is unavailable, fall back to `thumbnail-design` patterns—but still forbid plain first-frame screenshots as the final cover for growth posts. oil-visual is for explainer/transparent assets only (see `platforms/cover/SOP_oil_visual_pilot.md`); never replace 小墨团 / product icon anchors.
