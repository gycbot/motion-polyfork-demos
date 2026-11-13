---
name: video-storyboard-dsh
description: >-
  Use when studying a video, breaking down a viral cut, or reviewing a finished
  cut and you need the whole piece compressed into an addressable contact sheet
  (numbered cells + timestamps + transcript + audio envelope), or when you need
  the CLI to emit a contact sheet + timeline. Not for Remotion / final render.
---
# Video storyboard (dsh)

Primary source (t1674u): **dsh-video-storyboard** by SAOS-3624 — vendor at
`<LOCAL_VENDOR_PATH>` (MIT 0.2.2; keep LICENSE /
NOTICE / CREDITS). Standing: `<TEAM_PATH>`.

Role: give a text-only agent a low-token way to **look at** a video (one image ≈
384 tokens for 9–16 frames) instead of accepting someone else's summary.

## When to use
- 学片 / 拆爆款 / 审成片：先出可引用宫格，再写结构/钩子/密度结论
- 需要 CLI contact sheet + 时间线清单
- 长片要 `--batch` 窗口表，而不是一次丢几十张单帧

## When NOT to use
- Remotion / 成片渲染 / 字幕烧录 → 走现有制作技能（remotion-* 等）
- 只要纯口播文字、无画面索引 → 可只用 `STANDING_BILI_AUDIO_ASR_T1673`（Whisper）
- 用户本机执行 → **禁止**；只在 box / 云电脑

## Dependencies
| Need | Check |
|------|--------|
| Node ≥ 20 | `node -v` |
| ffmpeg + ffprobe | `ffmpeg -version`（PATH 或 `DSH_FFMPEG_PATH`） |
| npm deps | **无** runtime deps；peer `@deepseek-ai/dsh-tools` 仅 DSH 插件模式，CLI 不需要 |
| ASR（可选） | OpenAI-compatible `/audio/transcriptions`；见 vendor `docs/asr-contract.md` |

## Main commands（box）

```bash
VENDOR=<LOCAL_VENDOR_PATH>
cd "$VENDOR"

# 探测 / 分段预算（不出图）
node bin/storyboard.mjs /path/to/video.mp4 --plan 300

# 短片：一张 9 格 + 文本清单
node bin/storyboard.mjs /path/to/video.mp4 --cells 9 --out <TEAM_PATH> --text --transcript off

# 窗口细看
node bin/storyboard.mjs /path/to/video.mp4 --from 120 --to 180 --cells 6 --text

# 长片批处理
node bin/storyboard.mjs /path/to/film.mp4 --batch 300 --out <TEAM_PATH>
# → film-w01.png … + film-timeline.md
```

Artifacts default under `<TEAM_PATH>`（按任务分子目录）。

## Reusable algorithm points（队内要点）
1. **切景**：HSV 三通道差（64×36），阈值按目标切点速率反推（非死 `3×p95`）— `src/scan.js`
2. **密度分格**：剪辑率 + 字/秒 + 响度突变积分等分，吸附真实切点（位移 ≤ 理想格长 30%）— `src/density.js` / `plan.js`
3. **最锐帧**：段内 3 候选比 Laplacian 方差（默认 `pick=sharp`）— `src/sheet.js`
4. **ASR**：≤45s 切片、静音中点切、过滤音乐幻觉段；无 ASR 仍出图 — `src/asr.js`
5. **长片**：`--batch` + 切点吸附窗口；文本 `detail` / `cellRange` / `textOnly` 控 token
6. **Token 账**：一图 384；先粗看再 zoom；中文台词常比图贵

## Boundaries vs other stacks
| Stack | This skill | Other |
|-------|------------|--------|
| 看片索引 | **本 skill** | — |
| 成片制作 | 不替代 | Remotion / remotion-* / vibe 制作线 |
| 口播转写兜底 | 可选 ASR 层 | `STANDING_BILI_AUDIO_ASR_T1673` Whisper |
| DSH 插件宿主 | 可选；队内默认 CLI | 不强装 dsh-tools |

## Smoke
```bash
cd <LOCAL_VENDOR_PATH> && npm test
# 最小合成片（无第三方画面）：ffmpeg lavfi 纯色/色条 → storyboard
```

## Ban
- 不替用户开上游 GitHub issue（除非另令）
- 不装付费 ASR / GPU 栈当默认
- 不把 AGPL/付费依赖拖进默认产线
