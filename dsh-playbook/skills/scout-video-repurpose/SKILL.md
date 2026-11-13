---
name: scout-video-repurpose
description: >-
  Use when turning tech-scout or motion-exploration demo videos into reusable
  social assets: adapts each good video natively per platform—3:4 multi-image
  carousels (图文) for Xiaohongshu, 9:16 vertical hooked video for Douyin, and
  16:9 landscape showcase/B-roll for Bilibili.
---
# Scout Video Multi-Platform Repurpose

Turn high-quality exploration, motion-design, or tech-demo videos into platform-native assets instead of cross-posting the raw horizontal MP4 everywhere.

## 1. Intake & Quality Filter
Reuse upstream, still-valid ≥3 multi-platform references and the verified skill evidence already on the contract; do not start a new sample search. Before building the full three-end pack, prove one target end with one card, a short local motion clip, or a phone-readable frame, then scale. An adapted asset pack is not a finished tutorial or share video.
Only admit videos into the shared pool (`team/shared/scout-video-pool/INDEX.md`) that pass visual quality checks:
- Clean visuals with **no small-print disclaimers** ("Unofficial", "AI-generated", footnotes) and **no page-index microcopy** (`1/8`, `P1`).
- Smooth shot transitions and clear visual focal points.
- Check IP/persona constraints before public use (e.g. if a character IP is under a private-only hold on a specific channel, either hold public release there or crop/extract pure UI/motion B-roll cards without the held persona).

## 2. Platform-Native Format Adaptation (Mandatory — Never Raw Cross-Post)

### A. Xiaohongshu (小红书) — **Image-Text Carousel First (图文优先, 3:4 `1080×1440`)**
Xiaohongshu readers strongly favor swipeable multi-image posts (`图文`) over raw landscape videos for tech/design/efficiency shares:
1. **Extract peak crisp frames** from each distinct visual beat/card of the source video (avoid motion-blurred transition frames).
2. **Compose 3:4 (`1080×1440`) cards**:
   - **Cover (Slide 1)**: High-contrast visual evidence frame + bold, readable headline hook (`oil-cover-agent` / contrast hook style). Zero small-print footer, zero `1/N` page numbers.
   - **Body Slides (Slides 2–6)**: One clear point or demo step per card, framed cleanly on a matching dark/light 3:4 canvas with large mobile-readable callout text (no tiny crops, no raw 16:9 letterbox strips).
3. *(Optional secondary)* Short 3:4 vertical video/live-cover only when motion itself IS the subject (e.g. shader/typography animation showcase), still reframed to 3:4 rather than tiny 16:9.

### B. Douyin (抖音) — **9:16 Vertical Short Video (`1080×1920`)**
Douyin requires native 9:16 full-screen vertical presence and immediate hook retention:
1. **Reframe to `1080×1920` (9:16)** — **NEVER** drop a tiny 16:9 video in the middle of giant black bars:
   - Prefer re-rendering from the Remotion composition at `1080×1920` when source code exists.
   - Otherwise, build a structured 3-zone vertical layout: **Top (`~18%`)** persistent bold hook header → **Center (`~55–65%`)** zoomed/cropped active focal region (1.35×–1.6× center-of-action crop over subtle blurred/matched ambient backdrop) → **Bottom (`~20%`)** punchy synchronized captions.
2. **Pacing & Audio**: Front-load the strongest visual punch into `0–2s`, keep `≥3` clear beats (`2–4s` per beat), and pair with VO or legal BGM (never ship dead-silent video to Douyin).

### C. Bilibili (B站) — **16:9 Landscape Video (`1920×1080`)**
Bilibili favors information-dense 16:9 horizontal demos and breakdowns:
1. Keep native `1920×1080` landscape framing; combine related short demos into a structured `30–90s` showcase or use individual clips as high-density B-roll inside explainer videos.
2. Pair with clear chapter/beat titles, VO (`narration-av-sync`), and a dedicated 16:9/4:3 cover.

## 3. Packaging & Handoff
Write adapted outputs under `team/shared/scout-video-pool/packs/<pack_id>/{xhs_carousel,douyin_9x16,bili_16x9}/` with a `HANDOFF.md` recording source MD5, frame timestamps, aspect ratios verified via `ffprobe`/image dimensions, and SAMPLE≠授发 gate status. Also record: the unique successor role and owner, deadline (Asia/Shanghai), which reusable steps the pack teaches, the tutorial or share-video script/output path, per-end differences, and the authorization stop. An adapted pack ≠ tutorial closeout; if the same role can finish the tutorial or share video, do it. Changing files alone is not closeout; mark「已交接/待作品」until the work lands. HOLD or unapproved topics do not force a video.

## APPEND · 2026-11-10 pm-selflearn · article2media 交付前自查（纸面蒸馏 · 禁整装）

来源只读：https://github.com/threerocks/article2media（MIT · 末提交 2026-11-09 17:43 亚洲/上海 · sha 5e752beaa7cd396804b732032ada482da6851e43）；**不** git clone / npx skills add / 装插件。叠 P0-8/P0-10：下列为制作岗渲染前/交付前自查，不合格当场改，不另开审核、不另开 agent。回执：`<INTERNAL_RECEIPT>`。

### 图文卡片版面（3:4 / 4:5 / 1:1 / 9:16）
交付前自查：**溢出、截断、重叠、单字成行**；图表过小、图片或字体未载入同查。已有项不重复：禁页码/免责小字、禁 16:9 小横条、手机可读大字、一卡一点，仍按本文第 2 节执行。
