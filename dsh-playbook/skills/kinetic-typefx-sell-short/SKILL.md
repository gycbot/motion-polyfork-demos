---
name: kinetic-typefx-sell-short
description: >-
  Use when producing Remotion kinetic Chinese type shorts (9:16 Douyin/TikTok or
  16:9) that sell or demo a product using Grokko type-atlas techniques
  (裂升/词槽/描边填色/马克笔/卡堆等); also when 小金豆 or 视频制作 need to reuse the typefx
  sell-pitch pipeline.
---
# kinetic-typefx-sell-short

把「Grokko 字效图鉴」手法叠进竖屏/横屏卖货或演示短片。叠 `motion-house-style` + `user-taste-transfer`。**SAMPLE≠授发**；缺 contact / film_gate 自检不算交付。

## When
- 抖音/TikTok 9:16 卖货演示、手册/决策表/作业纸种草片
- 需要前 2 秒数字/痛点钩子 + 中段指标点亮 + 尾段交付物墙
- 复用已验证字效，禁止重新发明霓虹粒子套路

## 金样（只学做法，题材重写）
| 角色 | 路径提示 |
|------|----------|
| 图鉴源（8 卡） | `team/queue/artifacts/t135u-explore-samples/remotion/src/GrokkoTypeAtlas.tsx`（1920×1080 教学片） |
| 卖货竖屏金样 | `grok-juejin/digital-sku-shelf/wow-pack/douyin_9x16/` · `final-v2-typefx.mp4` + `remotion-typefx/src/SellPitchTypeFx.tsx` |
| 分镜合同 | 同目录 `storyboard-v2.md` |

## 字效卡菜单（至少选 4，卖货片建议 5）
| 卡名 | 用途 | 卖货默认用法 |
|------|------|--------------|
| **type-assembly** 裂升 | 前 2s 钩子 | 超大数字/结果词逐字裂升 + 强调色下划线 |
| **pill-slot** 词槽 | 痛点清单 | 3–5 个坏习惯词槽轮换 → 收束一句新状态 |
| **outline-fill** 描边填色 | 硬指标 | 2–3 个百分比/倍数/时延：描边预热 → 填色点亮 |
| **marker-underline** 马克笔 | 产品名/承诺 | 标题马克笔下划 + 2–3 步动作卡 + 价位（可选） |
| **card-stack** 卡堆 | 交付物墙 | 4 张内容卡扇出：「付费后真正拿到的」 |
| vertical-roll | 同义替换滚词 | 可选；竖屏慎用过密 |
| typewriter | 改口重打 | 可选；适合纠错梗 |
| draw-svg | 路径描线 | 可选；logo/图标 |
| **blur-slide** | 段间过渡 | 1 个短标签词；快进→停稳→软出，禁闪 |

## LOOK 硬条
- 色板默认：**炭黑 + 珊瑚**（`#0E0E0C` / `#E85A2A`）；**禁霓虹紫主色**、禁粒子爆发、禁角标边框
- **零免责小字、零页码、零「仅供参考」角标**（`user-taste-transfer`）
- 每 2–4 秒必须有新字事件；静音可读；缩到约 360 宽仍清楚
- 字体本地 `staticFile`（Noto Sans SC / Black）；禁 CDN
- 画面只由 `useCurrentFrame` 推导；禁 CSS transition / setTimeout / 裸 Math.random

## Steps
1. **brief** — 一句话钩子 + 目标平台比例（默认 1080×1920）+ 要卖/要演示的交付物清单（≥3）
2. **pick-cards** — 从菜单选 ≥4 卡；前 2s 必须是 type-assembly 或同等力度数字钩；尾段优先 card-stack
3. **beats** — 写 `storyboard.md`：时间轴 | 卡名 | 画面文案（中文 ≤12 字/主标题）
4. **fork** — 复制金样 `SellPitchTypeFx.tsx`（或从图鉴抽对应组件），改文案与 SEG 时间，**不要**从零重写 easing
5. **build** — Remotion Composition；草稿 30fps 可；时长卖货片 15–20s 优先。开工前按 `skill-first-task` 写齐对标样本、本金样路径和观感标准（小改沿用上一版）
5.5. **开渲前自检** — 先出钩子段关键帧 + 一段局部动态，对照 `motion-house-style` 六分项和对标样本，全 ≥8 才准全片渲染；不达标在这一步改，不整片渲完再说
6. **render** — 先写临时文件再 rename 成 `final-*.mp4`
7. **qa** — 出 `contact`（全片帧总览）+ 可选 phone（360 宽）；跑 film_gate：空画面 / 蓝紫底≥30% / 运动过稀 = 红灯回做
8. **复核** — 只核渲染是否把 5.5 已过的标准完整保住（film_gate + contact）；不是全片后的 critic 轮，不另开 agent 审，不交用户先看一眼
9. **deliver** — 只交一次达标成片 + contact + storyboard + 一行复用记录（样本｜金样/组件｜证明路径｜本次差异）给队长/需求方；**SAMPLE≠授发**，禁自动发帖

## 文案槽位（卖货默认五段）
1. 钩子数字/结果（裂升）
2. 痛点词槽 → 新状态一句
3. 2–3 个可核验指标（描边填色）
4. 产品名 + 可抄步骤（马克笔）
5. 交付物卡堆 + 一句「付钱拿到什么」

禁止：收益承诺、涨粉承诺、代充/租号、整段英文主字幕（可用英文专有名词作标签）。

## 平台改编
- **抖音 9:16**：本 skill 默认；竖屏字号主标题 ≥64、数字钩 ≥140
- **小红书**：优先抽关键帧做 3:4 轮播（叠 `scout-video-repurpose`），不要原片硬发
- **B 站 16:9**：从图鉴横版工程改编，或把竖屏安全区居中 letterbox（次选）

## 岗位
- **视频制作**：fork → build → render → qa
- **小金豆/队长**：brief 过目、deliver 闸、授发决定
- 不新建 Bot；不替代 `auto-video-pipeline` 全流程口播片（口播片另叠 `narration-av-sync`）

## 完成定义
- [ ] storyboard 含卡名对照表
- [ ] 成片路径 + contact
- [ ] film_gate 自检写进 storyboard
- [ ] 标明 SAMPLE≠授发
