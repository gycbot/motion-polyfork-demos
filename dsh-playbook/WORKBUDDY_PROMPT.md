# 贴给 WorkBuddy 的开场指令

使用前：输入栏模型已切到 **Hy4**；本仓库根目录已作为工作区打开；`source/final_bilibili_16x9.mp4` 已拷入并核过 md5 `18ba7833af875e7e1cadd0530cf92457`。把下面虚线之间的正文复制发送即可。

-----

请作为内容制作执行者，严格按本仓库的规范独立完成对照任务。

1. 先读并遵守：
   - `TASK_1MIN_HOOK.md`（任务正文，最高优先级）
   - `SOURCE_MANIFEST.md`
   - `task/brief.short_video.bilibili.yt-repost-1min.json`
   - `dsh_system/skills/dsh-executor/SKILL.md`（或 `skills/dsh-executor/SKILL.md`）
   - `skills/skill-first-task/SKILL.md`
   - `skills/user-taste-transfer/SKILL.md`
   - `skills/de-ai-zh/SKILL.md`
   - `skills/scout-video-repurpose/SKILL.md`
   - `skills/narration-av-sync/SKILL.md`
   - `skills/viral-copy-zh/SKILL.md`
   - `skills/oil-cover-agent/SKILL.md`
   - `skills/preview-deliver-split/SKILL.md`
   - `skills/motion-house-style/SKILL.md`
   - `standing/README.md` 及其中与本任务相关的站规
2. 任务：从 `source/final_bilibili_16x9.mp4`（已过审中文搬运成片，8:48，16:9，1920×1080@60）截取中间关键部分，做成约 1 分钟（55–70 秒）快节奏抓眼**精剪，不是预告**。多钩子、中文互联网快节奏、语气更夸张（如「炸了」），多剪关键画面。优先三个过夜成品的亮相/结果，以及花费/值不值得的关键数字。不要把整片缩成迷你版，不要用 atempo 整段压时长，不要复用长片里偏陈述的口播。
3. 硬性约束：
   - 第三人称（Tim / Fable，禁用「我测试」这类第一人称）
   - 原片英文原声必须继续完全剥离，不是 duck
   - 口播以 `task/script-1min-hook.draft.md` 为准；仍为 draft_for_review 时不得 TTS、不得渲终稿，只做分镜和选段后停止。过稿后用 grokko_sweet 新配，rate 约 +20%
   - 16:9 1920×1080，优先 60fps；禁止 9:16 / 抖音
   - 简介必须含 https://www.youtube.com/watch?v=xKlUf-dw_Lw 与「本视频为 AI 技术搬运翻译」
   - 封面从本条精剪真实帧提取
   - 数字是原作者说法，不得编造、不得写成我方实测
   - 禁止发布、禁止登录社交平台、禁止把 mp4 加入 git
   - 不要使用 anything2explainer（PolyForm Noncommercial；且本任务不是从零 explainer）
   - 源片 md5 对不上或文件缺失就停，不要去 YouTube 重下
4. 先用 ffprobe 与 md5 核源片；读口播草稿。未过稿则只交分镜与选段后停止。过稿后再配音、按钩子快切拼接、听检、对齐字幕、抽封面、写简介。
5. 交付（写到 `task/output/`）：精剪 mp4、cover.jpg、description.md、self_check.md（含 skills_used、md5、时长、fps、分辨率、音轨、所用源片时码段）、asset_manifest.json、run.jsonl。状态 `final_for_owner`。然后停止，等我目视。

-----
