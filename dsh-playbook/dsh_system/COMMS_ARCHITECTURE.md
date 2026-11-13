# dsh 五层通信架构（t2895u）

本文件是中间通信层的正本，不是临时备忘。实现落在本目录的 `bin/`、`protocols/messages/`、`skill_map.json` 与 `rules.autoloop.json`。Grok Bot 只写需求与资产、做验收和向主人汇报；**全部制作（对标检索、文案、分镜、日后渲染）由 dsh 完成**。发布永远只从 Grok Bot 出去，dsh 与调度不持有平台登录、不发帖。

时区：Asia/Shanghai（UTC+8）。密钥只进 dsh 子进程环境，不写进 brief、卡片、回执、本文件或日志。

## 1. 角色

| 角色 | 做什么 | 不做什么 |
|---|---|---|
| **Grok Bot（L1）** | 锁需求 brief、提供资产路径、验收、把草稿交给主人审、按 GROK_GEN 履约生图/生视频（非平凡生成才履约）、向主人汇报 | 不代替 dsh 做对标检索、不代写口播/分镜/成片、不绕过 L2 直接开渲 |
| **agent-autoloop（L2）** | 排队、按规则派活、保留轮次与本机卡片、记录状态；监听 `127.0.0.1:3350` | 不写文案、不渲染、不把卡片发到飞书/企微/Slack/Discord |
| **grokbot local-agent（L3）** | **官方产品** [robbin/grokbot-local-agent](https://github.com/robbin/grokbot-local-agent)：按文档用 `<LOCAL_PI_TOOLS>` 委派、落盘 `<REDACTED_AUTH_PATH>`、webhook 唤醒、`pending`/`ack`（始终带 `--owner`）。Pi CLI 已装（1.5.2）。| 不是第二套「替 dsh 做成本片」的智能体（制作仍是 L4 dsh，t2894u）。闭环还差：主人本人 `pi /login`、例程 webhook 凭据。在那之前 `bin/local_agent_bridge.py` 只作信封/emit 辅助，**官方脚本可用后退役桥**。勿让 Agent 读 `<REDACTED_AUTH_PATH>`。 |
| **dsh（L4）** | 在锁定工作区做全部生产：自己检索高互动参考、写标题/口播/分镜、日后渲染；用已注入的 skill 与扩展/网络工具 | 不发布、不登录社交平台、不读密钥文件、主人未批文案前不渲染 |

L5 发布仍只属于 Grok Bot，本层不实现。

## 2. 主路径（必须走这条，禁止临时直调充当正式生产）

```
Grok Bot 锁 brief
    → bin/aal_submit.py <brief.json>
    → inbox/ → inbox/claimed/  （inbox 监视器）
    → agent-autoloop emit brief.approved
    → 规则「制作内容」派给 dsh-dispatch
    → 官方规则把活派给工人 dsh-dispatch（command：aal_dispatch.py → dsh_dispatch.py；卡片/later 走 Autoloop 原生回执）
    → bin/dsh_dispatch.py  （注入 skill、锁工作区、开/续 dsh CLI）
    → dsh --profile headless [--session-id <同一会话>] --json <prompt>

**现行模式（t2901u，live web）**：当正在跑的 `dsh --profile web`（3080）已在 `session/list` 里持有该会话的写锁时，`dsh_dispatch --continue` **不再**开 headless（会报 `already owned by an active write handle`，且 Funnel 看不到直播）。改为对 web 进程 `POST /api/session/prompt`（typert `client-request`，method `session/prompt`，`mode=queue`，cookie 由 `dsh-rc/rc_manage.ensure_cookie` 从启动日志兑换，永不打印）。`SessionPromptValue` 为 `{accepted:true}`，调度轮询 `session/list.running` 直到结束；墙钟到了也不 `session/cancel`。主人打开同一 Funnel 会话即可直播看到这一轮。仅当 web 未持有该会话时才回退 headless CLI。
    → 工作区产物 + state.json + 消息信封
    → 桥写 draft_for_review / question / asset_request 卡片
    → Grok Bot 验收并呈主人
```

后续轮（同一 dsh 会话，禁止每次新开一条互不相干的会话）：

```
Grok Bot 写 owner_feedback 或 follow-up
    → bin/aal_followup.py --task-id <id> --type <owner_feedback|brief.followup> --prompt-file <path>
    → agent-autoloop emit brief.followup | owner.feedback
    → aal_dispatch → dsh_dispatch.py --continue --task-id <id> --prompt-file <path>
    → dsh --session-id <已记录的 CLI session>
```

Grok 生图履约仍用已有事件 `grok_gen.fulfilled` → `dsh_dispatch.py --resume --task-id <id>`（同一 CLI 会话，若已记录）。

调试用的直接 `dsh_dispatch.py --brief` 或临时 `dsh` CLI **不算正式生产路径**，只用于探针。正式素材任务必须留下 Autoloop 事件与总线消息。

## 3. 消息类型

所有消息共用信封（`protocols/messages/envelope.schema.json`）：

```json
{
  "id": "msg-…",
  "type": "brief|question|asset_request|asset_delivery|progress|draft_for_review|owner_feedback|done",
  "task_id": "…",
  "from": "grok_bot|autoloop|local_agent|dsh|owner",
  "to": "…",
  "created_at": "2026-11-13T15:00:00+08:00",
  "in_reply_to": null,
  "payload": {}
}
```

| type | 谁写 | payload 要点 | 落地 |
|---|---|---|---|
| **brief** | Grok Bot | 完整 ContentBrief（见 `brief.schema.json`）+ 可选 `skill_tags` / 资产路径 / 站规指针。制作内容不要预写好 | `inbox/` → `inbox/claimed/`；工作区 `brief.json` |
| **question** | dsh 或桥 | `title`, `text`, `options[]`, `recommend` | `messages/question-*.json`；本机 `cards/card-*.json` |
| **asset_request** | dsh | 与 `gen_requests.json` 相同（GROK_GEN） | 工作区 `gen_requests.json`；状态 `waiting_grok_gen` |
| **asset_delivery** | Grok Bot | `gen_results.json` + `grok_gen/<id>.*` | 履约后 emit `grok_gen.fulfilled` |
| **progress** | 桥或 dsh | `phase`, `steps`, `note`（无密钥、无中间文案充数） | `messages/progress.jsonl`、`state.json` |
| **draft_for_review** | 桥在 dsh 收束后 | 产物路径清单、待审说明、`copy_review_required: true` | `messages/draft_for_review.json`；本机卡片；状态 **`draft_for_review`** |
| **owner_feedback** | Grok Bot 代记主人意见 | `decision`: `copy_approved` / `revise` / `hold`；`notes`；禁止把「智能体自称已批」写成已批 | `messages/owner_feedback.json`；emit `owner.feedback` |
| **done** | 桥 | 终态、路径、token/耗时摘要 | `DONE.json`、`queue/done/`、`messages/done.json` |

GROK_GEN 的请求/结果 schema 仍是 `protocols/gen_requests.schema.json` 与 `gen_results.schema.json`。`asset_request` / `asset_delivery` 是通信层名称，文件格式不另起一套。

## 4. 磁盘位置

| 位置 | 内容 |
|---|---|
| `dsh_system/inbox/` | 新 brief 投递；监视器认领到 `inbox/claimed/`，不合格到 `inbox/rejected/` |
| `dsh_system/queue/{pending,ack,done}/` | 调度回执副本 |
| `dsh_system/cards/` | 本机卡片（只文件，不外发） |
| `dsh_system/bus/<task_id>/messages.jsonl` | 该任务通信总线（追加） |
| `dsh_system/logs/aal/` | Autoloop / 监视器日志 |
| `<WORK_DIR>/` | 锁定工作区：`brief.json`、`AGENTS.md`、`.skills/`、`.dsh/skills/`、`output/`、`state.json`、`messages/`、`gen_requests.json`、`logs/` |
| `<WORK_DIR>/skills_manifest.json` | 实际注入的 skill 与缺失项 |
| `<LOCAL_DSH_RUNTIME>` | 共享 DSH_HOME，dsh web（3080）可见同一会话 |

工作区外的 skill 正本在 `skills/<name>/SKILL.md`。调度把需要的副本拷进工作区 `.skills/<name>/`（外加 executor 到 `.dsh/skills/dsh-executor/`），并在 `brief.json` 写入 `skills` 与 `skill_paths`。

## 5. 状态机（含文案审稿闸 · t2882u）

```
brief_drafted          Grok 正在锁需求（尚未提交）
brief_submitted        已入 inbox
brief_approved         Autoloop 已收到 brief.approved
queued                 等待唯一工人（parallel=1）
running                dsh CLI 正在本轮
  ├─ question          暂停等本机卡片答复 → 答完 brief.followup 续跑
  ├─ waiting_grok_gen  暂停等 asset_delivery → grok_gen.fulfilled 续跑
  └─（进度可写 progress，不改变主状态）
draft_for_review       【硬闸】文案/分镜草稿已落盘，等待主人审。默认停在这里
  ├─ revise            owner_feedback.decision=revise → running（同一会话）
  ├─ hold              搁置
  └─ copy_approved     主人明确批过文案
may_produce            仅在 copy_approved 之后，才允许渲染/制作后续
completed              该阶段产物收束（仍 SAMPLE≠授发；发布是 L5）
failed                失败，可重试或卡片升级
  needs_continue       墙钟空闲收束；同一 session_id 续跑，不是失败、不是重开
```

硬规则：

- 从 `draft_for_review` **不能**直接进入渲染。没有工作区 `messages/owner_feedback.json` 且 `decision=copy_approved`，`dsh_dispatch` 不得开渲，brief / prompt 必须写明禁止渲染。
- 「最近文案都要主人审过再往下做」（t2882u）覆盖任何「发布不必再审」的旧口径，仅针对文案与分镜。存活检查仍是发布前的另一闸，不在本层。
- 智能体或调度自称「已批准」无效。
- 草稿文件开头必须有【草稿·待主人审】。
- 终态 `completed` 在文案未批时不得使用；有合格草稿时应标 `draft_for_review`。

## 6. 重试与超时

| 环节 | 默认 | 行为 |
|---|---|---|
| inbox 投递 Autoloop | 监视器循环重试失败的 emit | 不覆盖已 claimed 的同一 task_id |
| Autoloop 工人 `timeout` | **90 分钟**（规则字段单位是分钟） | 必须大于单轮墙钟，避免中台先把还在写文件的 dsh 杀掉 |
| `dsh_dispatch` 墙钟 `timeout_s` | **2700 秒（45 分钟）**；brief 字段 `timeout_s` 可改 | **仍在产出则绝不杀**：最近 180 秒内有 JSONL 事件（模型输出、工具、写文件）就视为在产出，到点只记日志、顺延。只有**已经空闲**才按墙钟收束 |
| 硬顶 `hard_timeout_s` | **5400 秒（90 分钟）** | 同样只在空闲后生效；还在写就继续。`hard_timeout_s: 0` 表示不设硬顶 |
| `max_steps` | **80，软上限**；brief 字段 `max_steps` 可改 | 只记账，**不杀进程**。曾经 18 / 20 / 28 步一到就 SIGTERM，那是根因之一，已废除 |
| 超时之后 | 自动 `--continue --session-id <同一会话>`，最多 2 次 | **禁止重开一条新会话、禁止推倒重写**。从 `output/` 与 `logs/checkpoints/` 断点续做 |
| 逐步检查点 | 每一步 `step_end` | 把 `output/`、`gen_requests.json`、`asset_manifest.json`、`self_check.md`、`run.jsonl` 拷到 `logs/checkpoints/step-NNN-step/`，中断不丢稿 |
| 车道探测 | `auto`：先 OpenCode Go（deepseek-v4.1-flash），失败再官方 flash | 探测与运行均不打印密钥 |
| GROK_GEN 等待 | 仅当 brief.`wait_for_grok_gen=true` | `gen_requests.json` 默认只是清单，状态走 `draft_for_review`，不把「有清单」误判成等待生图。真要履约时卡片约 30 分钟后复查；未履约不把占位图当终稿 |
| 同一工作区并发 | maxActive=1 | 禁止对同一 task_id 再开第二条互斥的 dsh 进程 |
| 会话续跑 | `state.json` 的 `dsh_cli_session_id` | `dsh --session-id`；OpenCode 请求头仍用稳定的 `dsh-<task_id>` |

### 为什么任务会被打断（已修）

实测两处硬杀：

1. **包装层墙钟太短**：第一轮 wrapper `--timeout 900`，实际跑到 **1191 秒 / 26 步** 仍被标 `timed_out=true`（进程在磁盘等待时 SIGTERM 还会拖一会儿）。更早的默认是 720 秒（12 分钟），视频草稿根本写不完 `self_check`。
2. **步数硬帽**：`StepGuard` 在第 19/21/29 步直接 `SIGTERM`。冒烟任务曾撞上约 18 步帽。制作一轮对标+文案+分镜通常远超这个数。

对策：墙钟默认 ≥45 分钟；步数改为软帽；**只要还在产出就不杀**；一旦空闲收束，用**同一个 `session_id` 自动续跑**，并依赖逐步检查点。调度里遗留的 720/900 秒、18/20/28 步参数会当作「未设置」抬到新默认，除非 brief 显式写了 `timeout_s` / `max_steps`。

dsh 自己也必须**每写完一个产物就落盘**（标题、口播、分镜各自成文件），不要攒到最后一刻再写 `self_check.md`。

临时失败（进程崩溃、车道 5xx）用「没跑成」结论触发 Autoloop 自动再试。需要主人或 Grok 决定的，走卡片，不伪装成临时失败。空闲超时后的续跑不是失败，不走「没跑成」自动新开会话。

## 7. Skill 地图（调度自动注入）

机器可读正本：`skill_map.json`。`dsh_dispatch.seed_workspace` 调用 `skill_inject.py`：按 format + 自动标签 + brief 里可选的 `skill_tags` 解析列表，把找得到的 `SKILL.md`（及旁边的 `steps.yaml`）拷进 `.skills/<name>/`，写出 `skills_manifest.json`，并把 `skills` / `skill_paths` 写回工作区 `brief.json`。找不到的记 `missing`，禁止假装已用。

**每单必注：** `skill-first-task`，`user-taste-transfer`，`de-ai-zh`，以及执行器 `dsh-executor`。

| 任务类型 | format / 标签 | 额外注入 |
|---|---|---|
| vibe 短片 | `short_video` + 标签 `vibe_video` | vibe-oneshot-remotion-brief，motion-house-style，kinetic-typefx-sell-short，viral-copy-zh，narration-av-sync，oil-cover-agent，preview-deliver-split，scout-video-repurpose，surface-web-craft-kit |
| 含 Grok Bot 对话演示 | 主题命中或标签 `grokbot_demo` | grokbot-chat-demo-sop |
| 小红书图文 | `image_text` + `xiaohongshu` 或标签 `xhs_image_text` | viral-copy-zh，oil-cover-agent |
| 转帖/翻译/二创 | 标签 `repost_translation` | viral-copy-zh，x-bookmark-drygoods-video，x-bookmark-learn-triad，scout-video-repurpose |
| 长讲解 / 教程 | `long_video` 或 `explainer_tutorial` | narration-av-sync，concept-diagram-tutorial-pack-2，video-storyboard-dsh，motion-house-style，oil-cover-agent |
| 网页 | `webpage` | surface-web-craft-kit |
| 开渲阶段（仅 copy_approved 后） | 标签 `remotion_render` | remotion-best-practices / remotion-markup / remotion-captions / remotion-render（若本机技能目录存在才注入） |

dsh **必须**在 `self_check.md` 写一节 `skills_used`：技能名、工作区内路径、用在了哪一条产出。只用名字、没有落点，不算用过。

Grok 在 brief 里只提供路径和标签，不代替 dsh 读 skill、不对标检索、不写脚本。参考网站清单等资产以**路径**交给 dsh，由它自己打开和使用。

## 8. dsh CLI 会话约定（供调度固化）

正式轮次命令形态（密钥只在子进程环境）：

```text
dsh --profile headless --patch <job/combined.patch.yml> --json [--session-id <session-…>] <prompt>
```

- 第一轮若工作区还没有 `dsh_cli_session_id`，不加 `--session-id`，从 JSONL 的 `{"type":"session","sessionId":"…"}` 回写 `state.json`。
- 后续轮必须带同一 `--session-id`。
- `DSH_HOME=<LOCAL_DSH_RUNTIME>`，cwd=工作区，`DSH_PERMISSION_MODE=workspace-write`。
- 每一轮的 argv（无环境、无密钥）追加到工作区 `logs/cli_turns.md`。



## 10. 官方工具怎么用（t2903u，勿用自研桩顶替）

### agent-autoloop（已装、已在跑）

正本：[KKKKhazix/agent-autoloop](https://github.com/KKKKhazix/agent-autoloop)，本机 `<LOCAL_AUTOLOOP_CONFIG>` v1.8.3。日常接口用官方 CLI，不要另写一套中台：

- 发事件：`agent-autoloop emit <类型> --subject <task_id> --summary … --detail …`
- 看进展：`agent-autoloop status`
- 答卡片：`agent-autoloop answer <编号> <序号>`
- 规则：`<LOCAL_AUTOLOOP_CONFIG>`（与 `team/dsh_system/rules.autoloop.json` 同步）。工人 `timeout` 单位是**分钟**，制作工人为 **90**。改规则会自动生效。
- 回执：Agent 最后一行结论 + ` ```agent-autoloop ` 块里的 `ask` / `notice` / `later`（`later: [{key, cancel: true}]` 取消轮询）。
- 通道：本生产线只用本机 filecards，不把卡片发到飞书/企微/Slack。

`bin/aal_submit.py` / `aal_followup.py` 只是把 brief 写成官方 `emit`；`aal_dispatch.py` 是规则里登记的工人命令。不要绕过 `emit` 直接开渲。

### grokbot-local-agent（脚本已装，登录未完成）

正本：[robbin/grokbot-local-agent](https://github.com/robbin/grokbot-local-agent)（MIT），检出 `<LOCAL_PI_TOOLS>/grokbot-local-agent` @`2e48e05`，可执行文件按[官方安装说明](https://github.com/robbin/grokbot-local-agent/blob/main/docs/setup-guide.md)复制到 `<LOCAL_PI_TOOLS>`。

- 委派：`<LOCAL_PI_TOOLS> run <cwd> "<prompt>" --owner <本 Bot ID> [--timeout] [--model …]`
- 查看：`pi-task status|list|pending --owner <id>`；结果在 `<REDACTED_AUTH_PATH>`
- 汇报后：`pi-task ack <ID> --owner <id>`（不带 owner 的 ack 不校验归属，禁止这样用）
- 通知：例程模板 `routines/pi-task-notify.md`；凭据只进环境变量 `PI_NOTIFY_URL`、`PI_NOTIFY_SECRET`。
- **必须主人做**：交互终端 `pi` → `/login`（默认模型 `deepseek/deepseek-flash`）。未登录不得假装已可用，不得用桥脚本冒充 pi-task。

### 浏览器渲染（t2900u，由 dsh 自己做）

成片、Remotion Studio/预览、HTML/canvas/GSAP 抓帧一律由 **dsh 在锁定工作区**用本机 headless Chrome 完成：可执行文件 `/opt/google/chrome/chrome` 或 `/usr/bin/google-chrome-stable`；`npx remotion render … --browser-executable=…`，需要时加 `--no-sandbox --disable-dev-shm-usage`。沙箱若拒读 `/opt/google/chrome` 或 `/dev/shm`，对该命令升权 `danger-full-access` 一次并写理由，禁止改由 Grok 代渲。调度在 `copy_approved` 后按 skill 地图注入 `remotion_render` 标签。headless 子进程会注入 `CHROME_PATH` / `PUPPETEER_EXECUTABLE_PATH` / `PUPPETEER_ARGS=--no-sandbox --disable-dev-shm-usage`；已在跑的 web 进程无法补环境，工具命令请用 `/opt/google/chrome/chrome` 或包装脚本 `bin/chrome-for-remotion`。


## 9. 与本任务的关系

选题「为什么你的 Grok Bot 用得不如别人」走：`short_video` + 自动标签 `vibe_video`、`grokbot_demo`。停在 `draft_for_review`（对标、标题、口播、分镜、生图请求清单），主人批文案之前不渲染。
