---
name: motion-house-style
description: >-
  Use when planning, building, or reviewing any motion/Remotion short: enforces
  2–4s new-on-screen, mute/phone-readable type, BANNED AI-slop patterns, and
  steps.yaml+wf_check. Not for VO-only narration pipelines.
---
# motion-house-style（动效硬规范 · t2496u PROD）

叠站规 `STANDING_MOTION_HOUSE_STYLE_T2496` + `STANDING_WF_STEPS_GATE_T2491`。
机读步骤：同目录 `steps.yaml`（**verified: true** · run `t2496-motion-house-smoke`）。校验：`python3 <TEAM_PATH> team/runs/<task>`。
**SAMPLE≠授发**；缺项≠0 禁 shipped；STATE 未全勾不算交付。

## When
- Remotion / vibe / AVP / UI 动效短片的分镜、代码、静帧自查、成片自检
- 口播主导片：节奏可按 VO（`narration-av-sync`），**文字/BANNED/手机可读仍强制**
- 不替代 `genshin-char-voice-pilot`（配音旁路）

## 渲染契约（Remotion）
1. 画面只由 `useCurrentFrame` / 时间推导；禁 CSS `transition`、`setTimeout`、`requestAnimationFrame`、`Date.now`、裸 `Math.random`（用带种子的 random）
2. 字体本地 `staticFile` / 仓库字体，禁 CDN
3. 草稿可 30fps；终版优先 60fps；需要运动模糊再开 subframe 策略
4. 成片先写临时文件再 rename，避免半成品被当完成

## LOOK（硬条）
- **每 2–4 秒屏幕必须有新东西**；禁 Dead time（无内容停顿）
- 一种 display 标题字体 + 一种 UI 字体 + 一种强调色；大胆设计只用一处
- **静音可读**（平台默认静音）；**手机可读**：缩到约 360 宽字仍清楚
- morph / 变形容器内文字必须有**独立 enter/exit timing**，禁止两态文字重叠
- 前 2 秒优先 kinetic type 钩子；六维观感标准写进 brief，关键帧/局部动态自检「前 2s 钩子」不到 8 分不得开渲
- **每单必附参考画面或参考片**（无参考易退回默认套路）

## BANNED（活清单，出片踩坑就追加）
- 渐变背景上居中大标题
- 所有元素同时淡入
- 角标 / 画面边框
- UI 发光
- 粒子爆发
- 弹跳缓动（bouncy easing）
- 空白停顿
- 只写「不要 AI 味」而不列具体套路（无效）

## 制作自检（关键帧对标，先对齐再渲染）
出图：`contact`（关键变化帧总览，约 30 帧）+ `phone`（约 360 宽可读测）+ 循环片才要 `seam`（首尾并排）。动作或转场问题另附最短局部动态片；静帧不能证明动作或声音。
打分 1–10：前 2s 钩子、手机可读、动效、变化（2–4s）、构图、品牌。这六维在 brief 里对着对标样本先写好观感标准；制作中用关键帧/局部动态自检，**全 ≥8 才准开渲**。只交一次达标成品，附对标样本和所用 skill；不另开 agent/窗独立审，不做全片后三轮 critic，不把用户当终审补洞，不叫醒内容审核。

## Steps（摘要；细节以 steps.yaml 为准）
1. `brief` → brief + 参考画面
2. `beats` → beats.json（纯动效）或 VO 时间轴（口播）
3. `shotlist` → 标注每 2–4s 新元素
4. `approved`（选题/分镜锁，用户已批或合同已写明豁免）→ APPROVED
5. `build` → Remotion 可渲
6. `stills` → 每拍/关键变化静帧 + 必要时局部动态，对标样本自检达标
7. `render` → out/final.mp4（自检未达标禁渲）
8. `qa` → contact + phone（+seam）作为成片技术证据，不是新的审核轮
9. `deliver` → 只交一次达标成品；SAMPLE≠授发

## 岗位映射（不新建 Bot）
选题/导演 ≈ brief+shotlist · 视频制作 ≈ build+stills自检+render · 队长交付闸 ≈ deliver（一次交达标成品；不映射内容审核）

## 来源
t2496u · PAPER `team/research/scout/2026-11-05-t2496-overnight-motion-studio-PAPER.md`（同源 @0xCarnagee house-style；作者未在 GrokBot 实跑，已本地化到 Remotion）

## t103u 并轨（2026-11-06）
- **15s proof→full**：见 `team/queue/artifacts/2026-11-06-t103u-L1-proof15s-gate.md` + `STANDING_WF_STEPS_GATE_T2491` APPEND；全片前由制作岗用关键帧/局部动态自检对齐，不另开人审样片轮；用户明确保留的选题/脚本锁照旧。
- **系列数据合同 + 六维标准**：见 `team/queue/artifacts/2026-11-06-t103u-L2-zaslon-remotion-norm.md`（六维前移进 brief 与制作自检；不装 zaslon）。

## APPEND · t163u huashu 六种做坏 + film_gate 句（2026-11-06 · 纸面蒸馏 · 禁整装）

来源只读：`alchaincyf/huashu-art-motion` MIT；**不** `npx skills add`、不换 Playwright 引擎。

### 六种做坏（开工前 / 开渲前自检各对一遍）
1. **翻页式 PPT**：固定页眉页码、要点逐条淡入、每镜同一运镜、结尾居中金句卡 → 一镜一事件；相邻运镜有来有回；结尾落在主物动作上。
2. **照搬示范**：样例构图/角色/素材原样进成片 → 只学做法，题材重设计。
3. **没有具体的物**：抽象只写字和图标 → 每镜必须有可指认的主物+动作。
4. **死背景 / 空画面**：大面积纯色或星空底不变、主角不在画 → 主体占画一半以上；中段连续空画面红灯。
5. **有真实素材却纯代码画** → 截图/产品帧满幅，代码只标注运镜（口播/证据片适用）。
6. **一眼 AI 味**：蓝紫渐变底、深紫星空、霓虹青紫拼贴、每镜换画风、Q 版白壳机器人滥充 → 一片一画风一套色板。

### film_gate 量化红灯（Remotion 人工/脚本对照，非 huashu deliver.py）
- **空画面**：成片中段连续 ≥0.3s 近乎纯色、无边缘/无主物 → 红灯，记时间码回制作。
- **蓝紫底**：蓝紫/霓虹青紫底帧占全片 **≥30%** → 红灯（题材本身需要时须在 HANDOFF 写豁免理由，降黄）。
- **运动过稀**：几乎无镜头内运动、整片像静帧翻页 → 红灯（解说片允许长停，但须有物/字事件；对照 2–4s 新信息）。
- **推近过密**：连续多镜只往里推、无拉回 → 黄灯，正常速审一遍。

叠站规 `STANDING_MOTION_HOUSE_STYLE_T2496` APPEND t163u；正本纸面 `queue/artifacts/2026-11-06-t163u-huashu-film-gate.md`。

## APPEND · t192u 视频成本闸（EianLu∪H3）· 2026-11-10 · 纸面蒸馏 · 禁整装

来源只读：X 收藏深拆 t192u（EianLu Seedance 闸 + Miller/H3 身份板/穿帮序）；**不**装 miliagent / 不买 RunningHub·Seedance 未批额度。

### film_gate 成本与抽卡（叠既有空画面/蓝紫/运动过稀）
1. **成本阶梯**：错留字 < 错留图 < 错留视频（按秒付钱）；先身份板/资产（角色多角度、一段一事、场景一张）再开渲或抽卡。
2. **开渲前自检**：制作岗用关键帧/局部动态对标样本自检，达标才渲染；不另开 agent/窗独立审，主脑自检即制作内证明，不把用户当终审。
3. **穿帮序**：能剪/代码修 → 首尾帧补 → 最后才重抽；改 seed 数字才叫抽卡。

纸面：`queue/artifacts/2026-11-10-t192u-x-bookmark-GATE-PAPER.md` · 站规指针 `STANDING_MOTION_HOUSE_STYLE_T2496` APPEND t192u。

## APPEND · 2026-11-10 pm-selflearn · article2media film_gate 增量（纸面蒸馏 · 禁整装）

来源只读：https://github.com/threerocks/article2media（MIT · 末提交 2026-11-09 17:43 亚洲/上海 · sha 5e752beaa7cd396804b732032ada482da6851e43）；**不**整装、不改口播 TTS 主路。叠已有 film_gate 与 P0-8/P0-10。下列为开渲前/成片交付前自查，不合格当场改，不另开审核。回执：`<INTERNAL_RECEIPT>`。

已有项不重复，故不纳入：画面静止≤5s（宽于已有「每 2–4 秒须有新东西」+空画面红灯）；字幕安全区（已有）；语速 4.5 字/秒（与 narration-av-sync「不硬拉语速」冲突）。

1. **字幕闪断 / 丢帧**：字幕停留须稳定、换句硬切不闪断；成片抽检「该有字幕的帧」不丢字。
2. **响度**：手机外放可听、不削波；与「第一帧有声」并存。若制作通道已有测量/loudnorm 工具，对照综合响度（来源 README 写 -14 LUFS）；不新装工具，不把单一数值写成必须过闸的新脚本。
3. **口播首句钩子**：有旁白时，首句须在约 5 秒内说完钩子；叠已有「前 2 秒视觉钩」，二者都要，不互相替代。
