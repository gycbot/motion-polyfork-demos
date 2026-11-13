# dsh 制作规范与技能包

给仓库所有者在自己的电脑上，用 **腾讯 WorkBuddy + 混元 Hy4**，对照完成与 dsh 相同的一项制作任务。

本仓库是**消毒后的规范与技能包**，不是 dsh 运行时，也不是成片仓库。

> **对照任务**：从已过审的 Fable 5.2 中文搬运成片（8:48，16:9，1920×1080@60，md5 `18ba7833af875e7e1cadd0530cf92457`）截取中间关键部分，做成约 1 分钟快节奏抓眼精剪。第三人称、原声剥净、grokko_sweet、保留 [原 YouTube 出处](https://www.youtube.com/watch?v=xKlUf-dw_Lw) 与「本视频为 AI 技术搬运翻译」。**不做 9:16 / 抖音。不发布。**
>
> 完整说明：[`TASK_1MIN_HOOK.md`](TASK_1MIN_HOOK.md)。源片须单独拷到本地，见 [`SOURCE_MANIFEST.md`](SOURCE_MANIFEST.md) 与 [`source/README.md`](source/README.md)。**成片母带不要放进 Git。**

---

## 许可先看（醒目）

**`skills/anything2explainer/` 为 PolyForm Noncommercial 1.0.0：非商用可免费使用，不得写成可商用。任何商用须事先取得作者书面授权。** 本仓库只收录该技能的入口 `SKILL.md`，不含工具包源码、模板或媒体。本对照任务是对已有成片的再剪，**不要**把 anything2explainer 当主路径。详见 [`LICENSE_NOTES.md`](LICENSE_NOTES.md)。

其余团队撰写的规范与 skill 默认保留版权，供仓库所有者本人及授权协作者对照实验；第三方引用的许可在各 skill 正文与 `LICENSE_NOTES.md` 中保留。

消毒范围见 [`SANITIZE.md`](SANITIZE.md)。

---

## 这是什么 / 不是什么

| 是 | 不是 |
|----|------|
| dsh 制作用的 brief 规范、技能、搬运线站规摘要，以及一项可独立执行的对照任务 | dsh / agent-autoloop / OpenCode 的可执行运行时 |
| 给 WorkBuddy 读的文档与 `SKILL.md` | 成片 mp4、预览、封面帧、客户照片、音色 wav |
| 路径已改成相对路径或占位符的示例 brief | 密钥、cookie、内网入口、本机绝对路径 |

原系统里，dsh 是制作工人，由调度按 brief 注入技能后开工。在你这台电脑上，**执行引擎换成 WorkBuddy，模型选 Hy4**；规范与验收不变。

Hy4 限免信息以产品页为准。所有者所述期限为 **2026-11-15 23:59（Asia/Shanghai）**。过期后积分或价格可能变化，请以 WorkBuddy 模型面板当时显示为准。

---

## 在 WorkBuddy 里如何选 Hy4

依据 WorkBuddy 官方「选择模型」说明（[文档](https://www.workbuddy.cn/docs/workbuddyapp/features/Model)）：

1. 打开 WorkBuddy（桌面端优先，便于读写本仓库和跑 `ffmpeg`）。
2. 点击**输入栏中的模型名称**（例如「Auto ▾」），弹出模型选择面板。
3. 选择 **Hy4**（界面上也可能写作「混元 Hy4」或「Hunyuan Hy4」）。**不要停留在 Auto**，以便这次对照固定是 Hy4，而不是被自动换成别的模型。
4. 如面板提供「思考强度」「上下文窗口」，本任务建议开到较高思考档，上下文尽量开大（精剪要读若干份 SKILL 与站规）。
5. 公开文档的模型表可能仍只列出 Hy3 等、尚未改到 Hy4；若你的客户端列表里有 Hy4，以客户端为准。若完全看不到 Hy4，检查应用是否已更新，不要用别的模型冒充这次对照。

Hy4 于 2026-11 前后在 WorkBuddy 等产品上线，属于带视觉与长程任务能力的模型，适合本任务这种「先读规范再剪视频」的多步工作。

---

## 如何放技能和规范

本仓库的技能已经是 WorkBuddy 能识别的形态：每个目录一个 `SKILL.md`，目录名为 kebab-case。

**推荐做法（最稳，先做这个）：**

1. 把本仓库放到你的电脑上（clone 或拷贝整个目录）。
2. 用 WorkBuddy **打开这个仓库的根目录**作为工作区，使它能直接读 `TASK_1MIN_HOOK.md`、`skills/`、`standing/`、`dsh_system/`。
3. 把过审成片拷到 `source/final_bilibili_16x9.mp4` 并核 md5（见 `SOURCE_MANIFEST.md`）。
4. 把 [`WORKBUDDY_PROMPT.md`](WORKBUDDY_PROMPT.md) 的正文贴进对话，开始任务。

**可选：再把技能安装进 WorkBuddy 技能库**（便于以后别的任务自动调用）：

1. 左侧进入「技能」→「添加技能」→「上传技能」。
2. 按目录上传 `skills/` 下各技能文件夹（每个文件夹内必须有且仅用 `SKILL.md` 这个文件名，大小写不能改）。
3. 本对照任务建议**启用**：`skill-first-task`、`user-taste-transfer`、`de-ai-zh`、`dsh-executor`、`scout-video-repurpose`、`narration-av-sync`、`viral-copy-zh`、`oil-cover-agent`、`preview-deliver-split`、`motion-house-style`、`x-bookmark-drygoods-video`。
4. **关闭或不装 `anything2explainer`**（本任务不用；且为非商用许可）。
5. 不要一次把仓库里所有技能全打开，以免占满上下文、干扰调用。规范文档（`dsh_system/`、`standing/`、`TASK_*.md`）不是 Skill，放在工作区里让模型去读即可，不必当成技能上传。

制作规范的入口：

- `dsh_system/README.md` — 原 dsh 系统说明（已消毒；不要尝试启动里面的调度脚本）
- `dsh_system/COMMS_ARCHITECTURE.md` — 原通信架构（了解即可）
- `dsh_system/brief.schema.json` — brief 字段合同
- `dsh_system/skill_map.json` — 按 format / tag 注入哪些技能（路径已改成包内相对路径）
- `dsh_system/rules.json` — format→配方与产出物合同（已去掉本机调度事件）
- `dsh_system/examples/` — yt-repost 长片与本 1 分钟任务的示例 brief
- `dsh_system/skills/dsh-executor/SKILL.md` — 执行质量条与交付物（`output/`、`self_check.md`、`asset_manifest.json`、`run.jsonl`）
- `standing/README.md` — 搬运线站规摘要

---

## 如何跑 TASK_1MIN_HOOK

1. 选中 Hy4，打开本仓库根目录，源片已核验。
2. 本机确认 `ffmpeg`、`ffprobe` 可用（终端里跑 `ffmpeg -version`）。没有就先安装，不要让模型假装已经剪完。
3. 将 `WORKBUDDY_PROMPT.md` 贴进输入框发送；或直接说：「请严格按 `TASK_1MIN_HOOK.md` 执行，先读列出的技能和站规，不要发布。」
4. 模型应在本地 `task/output/`（或它声明的 `output/`）写出约 60 秒 16:9 精剪、封面、简介、`self_check.md` 等。
5. 你目视、听检、对 md5 与时码。精剪 mp4 **留在本地**，不要 `git add`、不要上传平台。
6. 对照点：同一份 TASK、同一份源片、同一套规范；一边是原先的 dsh 工人，一边是 WorkBuddy+Hy4。比较的是能不能独立按文档开工、精剪是否抓到中间关键部分、音轨/人称/出处是否合格——不是比谁更快发布。

逐步细则与禁止项见 [`TASK_1MIN_HOOK.md`](TASK_1MIN_HOOK.md)。

---

## 目录

```text
.
├── README.md                    # 本文件
├── TASK_1MIN_HOOK.md            # 对照任务（先读）
├── WORKBUDDY_PROMPT.md          # 可直接粘贴给 WorkBuddy 的开场指令
├── SOURCE_MANIFEST.md           # 源片文件名 / md5 / 时长（无 mp4）
├── LICENSE_NOTES.md
├── SANITIZE.md
├── PUSH.md                      # 建议的仓库名与推送命令（需你先建空仓）
├── source/README.md             # 把 mp4 拷到这里
├── task/brief.short_video.bilibili.yt-repost-1min.json
├── dsh_system/                  # 制作规范
├── skills/                      # SKILL.md 与必要 helper
└── standing/                    # 搬运线相关站规（已消毒）
```

推送到 GitHub 的步骤见 [`PUSH.md`](PUSH.md)。本包在本地已 `git init` 并提交干净的初始 commit，**尚未推送**：现有公开库（`gycbot/grokbot-showcase`、`gycbot/avengers-remix` 等）不适合承接本包，请先新建空仓库再推，不要 force-push，不要覆盖展示站。
