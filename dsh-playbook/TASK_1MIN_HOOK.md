# TASK · 约 1 分钟快节奏抓眼精剪（WorkBuddy + Hy4 对照任务）

这是与 dsh 对照的**同一任务**。请在选好 Hy4、放好技能与源片之后执行。完成后停在 `final_for_owner`，**不要发布、不要登录任何社交平台**。

完整 brief 的机器可读副本：`task/brief.short_video.bilibili.yt-repost-1min.json`（路径已改成占位符）。

## 0. 先决条件

1. 已把过审成片拷到 `source/final_bilibili_16x9.mp4`，md5 为 `18ba7833af875e7e1cadd0530cf92457`，时长 8:48.2，1920×1080@60。见 `SOURCE_MANIFEST.md`。**本仓库没有这份 mp4，缺文件就停，不要去 YouTube 重下原片。**
2. 本机有 `ffmpeg` / `ffprobe`（以及可用的 md5 工具）。
3. 已按根目录 `README.md` 选中 **Hy4**，并让模型能读到本仓库的 `skills/`、`standing/`、`dsh_system/skills/dsh-executor/SKILL.md`。
4. 开工前先读：
   - `skills/skill-first-task/SKILL.md`
   - `skills/user-taste-transfer/SKILL.md`
   - `skills/de-ai-zh/SKILL.md`
   - `skills/dsh-executor/SKILL.md`
   - `skills/scout-video-repurpose/SKILL.md`
   - `skills/narration-av-sync/SKILL.md`
   - `skills/oil-cover-agent/SKILL.md`
   - `skills/preview-deliver-split/SKILL.md`
   - `skills/viral-copy-zh/SKILL.md`
   - `standing/README.md`（搬运线摘要）以及其中列出的站规

**不要**把 `anything2explainer` 当本任务主路径（这是已有成片再剪，不是从零做科普片）。该技能是 PolyForm Noncommercial，见 `LICENSE_NOTES.md`。

## 1. 任务一句话

从已过审的 Fable 5.2 中文搬运成片（8:48，16:9，1920×1080@60）中，**截取中间关键部分**，做成约 **1 分钟**（允许 55–70 秒）快节奏、抓眼的**精剪（不是预告）**。要多钩子，适应中文互联网快节奏，语气更夸张（如「炸了」），多剪关键画面；主人认为长片「太平淡、太陈述」。画幅仍是 16:9，**不做 9:16 / 抖音**。第三人称。原声继续保持剥净。口播用 `task/script-1min-hook.draft.md`（过稿前不得配音）。保留原 YouTube 出处与「本视频为 AI 技术搬运翻译」。不发布。

## 2. 必做

- 只剪 `source/final_bilibili_16x9.mp4`。不要重新下载 YouTube，不要用未切广告的原片。
- 快节奏抓眼：优先三个过夜成品（Emberwake / Nocturn / Converge）的亮相或能玩证据，以及花费/值不值得的关键数字（约 $92、26% 周额度等，必须标成**原作者说法**，不得改数字、不得写成我方实测）。
- **不要**把 8:48 从头到尾缩成迷你版，**不要**用 `atempo` / 改语速把整段压成 1 分钟，**不要**片头片尾注水。
- 画幅 16:9 1920×1080，优先 60 fps。
- 口播第三人称（Tim / Fable，不要用「我」）。**不要**把长片里偏陈述的口播硬切复用当成片——主人已明确那版太平淡。以 `task/script-1min-hook.draft.md` 为准；若仍标记 `draft_for_review`，只做分镜和选段，**不得 TTS、不得渲终稿**。过稿后用 grokko_sweet 新配，语速加快：在原 `zh-CN-XiaoxiaoNeural`、`rate=+8%`、`pitch=+15Hz` 基础上把 rate 提到约 +20%，句间气口尽量掐掉。不要为凑时长把整段 8:48 atempo 压进 1 分钟。
- 原片英文原声必须继续完全剥离，不是 duck，不得残留英文人声。可薄叠无歌词、已获授权的 BGM，不得压过人声；没有已核授权曲就不要硬加 BGM（长片最终也没有 BGM）。
- 中文字幕与精剪口播对齐。成片里字幕已烧进画面，硬切后通常字幕会跟着走；若切点砍在句子中间，必须修好（重剪到句完，或重排字幕），禁止半句话。
- 简介（`output/description.md`）仍须含：
  - https://www.youtube.com/watch?v=xKlUf-dw_Lw
  - 原作者 Tech With Tim、原作标题
  - 固定句：**本视频为 AI 技术搬运翻译**
- 封面从**本条精剪**的真实帧提取，写下取帧时码（`output/cover.jpg` + 在 `self_check.md` 记录）。不要用长片 07:47 的封面文件直接充数，除非你确认那一帧也落在精剪里并重新抽取。
- 工作目录建议：在本仓库旁或 `task/work/` 下写文件（已被 gitignore）。交付放在 `task/output/` 或你声明的 `output/`。
- `self_check.md` 必须含：`skills_used`（技能名、本仓库内相对路径、用在哪份产物）、成片 md5、时长、fps、分辨率、音轨说明、字幕处理、封面取帧时码、**用了源成片的哪些时码段**、未发布。
- 还要有 `asset_manifest.json`、`run.jsonl`（步骤记录，禁止写密钥）。
- 状态写成 `final_for_owner`。SAMPLE ≠ 发布。

## 3. 禁止

- 发布到任何平台、登录社交平台、花钱、打印或泄露密钥。
- 制作或交付 9:16 竖屏 / 抖音稿（翻译精剪转抖音线仍 HOLD）。
- 重新下载 YouTube，或使用未去掉广告/CTA 的原片。
- 第一人称口播（「我测试」「我睡了一觉」等）。
- 把原声 duck 垫底或残留英文人声。
- 用变速把整部 8:48 压成 1 分钟。
- 编造未在成片中出现的花费、能力或官方数字。
- 收益承诺、课程销售、硬广、会员导流。
- 把本机绝对路径写进简介。
- 把成片母带或精剪 mp4 `git add` 进这个公开仓库。

## 4. 验收锚点

- `output/` 下有约 60 秒（55–70 s）的 16:9 1080p 精剪 mp4，优先 60 fps。
- 内容是中间关键部分的快节奏抓眼剪辑，不是整片缩放。
- 音轨无原片原声，口播为第三人称 grokko_sweet（按已过稿的精剪钩子稿新配，语速加快；不是预告，也不是长片陈述口播的迷你硬切）。
- 有对齐的中文字幕，以及从本片真实帧取的封面。
- 简介含原 YouTube 链接与「本视频为 AI 技术搬运翻译」。
- `self_check` 写清时码段、md5、时长、fps、音轨；未发布。
- 没有 9:16 交付。

## 5. 建议做法（给执行模型，不是唯一实现）

1. 用 `ffprobe` 核源片；计算 md5。对不上就停。
2. 按已过稿（或草稿）口播拆钩子，多抽三个成品的关键实机画面和花费数字镜头，快切、密切，不要长时间空镜陈述，不要做成只抛钩不展成品的预告。合计剪完后落在 55–70 秒。切点落在句子和场景完整处。
3. 若口播仍是 `draft_for_review`：只输出分镜、候选时码和草稿对齐方案，然后停止等过稿。过稿后再 TTS（grokko_sweet，rate 约 +20%）并用 `ffmpeg` 按口播节奏硬切拼接，保持 1920×1080@60、音频连续、无原声串入。不要整片 `atempo`。
4. 听检：无英文人声、无半句话、无第一人称；语气要抓眼，不要变回长片那种平淡陈述。
5. 处理字幕与封面；写简介、self_check、manifest、run 日志。
6. 若精剪 ≤30 秒可直接作为预览；若在 55–70 秒，按 `preview-deliver-split` 需要时可再切两段方便查看，但母带保留完整 1 分钟文件在本地，**不要把 mp4 推进 Git**。
7. 停。把本地产物路径、md5、时码段写进 self_check，交给操作者目视。不上传。

## 6. 许可与出处提醒

本任务处理的是对 [Tech With Tim 原片](https://www.youtube.com/watch?v=xKlUf-dw_Lw) 的已有中文翻译搬运成片的再剪。必须保留出处与「本视频为 AI 技术搬运翻译」，不冒充原作者或官方。不要把源片或精剪上传到这个 Git 仓库。
