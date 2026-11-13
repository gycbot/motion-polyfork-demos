<!-- PACK NOTICE: 此文件已消毒（本机绝对路径、密钥位置、内网入口已替换为占位符）。
这是原 dsh 制作系统的规范说明，供理解 brief、配方与质量条。
在 WorkBuddy 里对照完成 TASK 时：不要尝试启动 agent-autoloop、dsh_dispatch 或本机 OpenCode 钥匙；
把本仓库当规范与技能包，执行引擎就是 WorkBuddy + Hy4。
anything2explainer 为 PolyForm Noncommercial，见仓库根目录 LICENSE_NOTES.md；本对照任务不要用它。 -->

# dsh 五层内容体系 · Phase 1 + L2（2026-11-13）

本目录是按主人 t2880u 说明书落地的 **第一期可运行骨架**：Grok Bot 决策与发布，dsh 在锁定 workspace 里出素材，中间用磁盘上的 brief / 状态 / 回执衔接。这里写的是 **今天实际做成了什么**，不是说明书原文的完整实现。未装或未证实的工具在清单里如实标注，不假装已经可用。

## 五层：今天实现了什么

| 层 | 说明书角色 | Phase 1 实际状态 |
|---|---|---|
| L1 决策 | Grok Bot：热点、选题、锁 brief、验收、发布 | **保留在 Bot**。本目录只提供 `brief.schema.json` 与示例 brief。热点扫描（A1）**未做**，仍由 Bot 执行。 |
| L2 调度 | 独立中台 `agent-autoloop`，`<LOCAL_AUTOLOOP_CONFIG>` | **已安装正式产品**：[KKKKhazix/agent-autoloop](https://github.com/KKKKhazix/agent-autoloop) **v1.8.3**（MIT，提交 `1bd9aa2549d9506053c778c6fef9f8a68b78d5bf`），装在 `<LOCAL_AUTOLOOP_CONFIG>`，零运行时依赖。中台在 `127.0.0.1:3350` 后台运行（`bin/aal_start.sh` / `aal_stop.sh` / `aal_status.sh`）。inbox 里的 brief 变成 `brief.approved` 事件，按官方格式规则交给命令行 Agent `dsh-dispatch`（包装本目录的 `dsh_dispatch.py`，在锁定工作区跑 dsh）。拿不准时发**本机卡片文件**（`cards/<id>.json`），**不接**飞书/企微/Slack/Discord。内置 `dsh` 插件要求 dsh ≥ 0.2.2，本机是 0.2.1-rc.2，所以走包装层，保留 OpenCode Go 补丁与工作区纪律。 |
| L3 委派 | `grokbot-local-agent` / `pi-task`、pending/ack、保活、Wake on Done、用量刹车 | **部分自研**：dispatcher 把 `pending` / `ack` / `DONE` 落在磁盘（workspace + `queue/`）。`grokbot-local-agent`、`pi-task`、`bot-keepalive`、`Wake on Done`、`grokbot-usage-cli` **均未安装**。用量文件若不存在则告警并继续。完成标记供 Bot **轮询读取**，尚未接 webhook 精确唤醒。 |
| L4 执行 | dsh 在临时 workspace 内生产，无平台登录 | **已接通**。工作根目录：`<WORK_DIR>/`（选 `<WORKSPACE_PATH>` 而不是 `/tmp`，便于持久与验收；磁盘与 `/tmp` 同盘）。dsh `cwd` 锁在该目录，`DSH_PERMISSION_MODE=workspace-write`，密钥只进子进程环境。调度使用共享 `DSH_HOME=<LOCAL_DSH_RUNTIME>`（任务补丁仍走 job 目录里的 `--patch`，不把密钥写入共享配置），会话出现在 `<LOCAL_DSH_SESSION>/`，可供 3080 端口的 dsh web 远程控制在手机上看。 |
| L5 发布 | 仅 Grok Bot | **未做、也不会由 dsh 或 dispatcher 发布。** |

核心纪律已落实：dsh 不持有社交平台登录态或平台 API key；配置与回执里不写入任何密钥；发布只从 Bot 出去。

## 密钥接线（DeepSeek V4.1 Flash · OpenCode Go 主路）

主人提供的钥匙存放在 `<REDACTED_SECRET_PATH>`（权限 600）。这是 **OpenCode** 钥匙（前缀 `oc_`），不是 DeepSeek 官方 `sk-` 钥匙。主人已订阅 **OpenCode Go（$10/月）**：这把钥匙走 **Go 推理端点**，不走 Zen 余额。

- dispatcher / probe **只从该文件读入环境变量** `OPENCODE_API_KEY`，再交给 dsh。配置与回执里不写密钥。
- **主路补丁**：`patches/opencode-go.cordis.patch.yml`
  - `apiKeyEnv: OPENCODE_API_KEY`
  - `baseURL: https://opencode.ai/inference/go/openai/v1`（补全地址为 `.../chat/completions`）
  - 默认模型 `deepseek-v4.1-flash`
  - 每个请求必须带请求头：`x-opencode-session`（按任务稳定，dispatcher 把 `{{OPENCODE_SESSION}}` 写成 `dsh-<task_id>`）和 `User-Agent: dsh/0.2.1`（通用 UA 会被拒）
  - **补丁里没有密钥。**
- **这是推理模型（reasoning）**：需要足够的 `max_tokens`，既要覆盖隐藏 thinking，也要留下可见回答。补丁把 `defaultMaxTokens` / 模型 `maxTokens` 设为 **32768**。探测补全请用较大值（本仓库 probe 用 1024），不要用 16 这种过小的上限，否则可能只算出思考、看不到正文。
- 地区：Go 端点要求账号启用 **Global**。未启用时会 HTTP 400（hosted outside Europe and US, select Global），这只能由主人在 OpenCode 控制台打开，脚本改不了。主人已于 2026-11-13 启用。
- 验证（2026-11-13，上海时区）：
  - `GET https://opencode.ai/inference/go/openai/v1/models` → **HTTP 200**，目录含 `deepseek-v4.1-flash`。
  - 带 `x-opencode-session` + `User-Agent: dsh/0.2.1` 对 `deepseek-v4.1-flash` 做补全 → **HTTP 200**，可见回复 `PONG`。
  - 真实 dsh 短任务（`dsh --profile headless --patch <go combined> --json`，提示只回 PONG、不用工具）→ **exit 0，约 4.5 秒**，会话日志里 `request/header` 与 `assistant/message` 的 provider/model 为 **`opencode-go` / `deepseek-v4.1-flash`**，最终文本 `PONG`。不是只测了 curl。
- **自动回退**：`dsh_dispatch.py --provider auto` 先探测 OpenCode Go；Go 出错时回退到盒子里已有的 **DeepSeek 官方 API**，模型名 **`deepseek-flash`**（官方目录没有 `deepseek-v4.1-flash` 这个名字）。官方钥匙仍只从 `<REDACTED_SECRET_PATH>` 读入子进程。
- 旧的 Zen 补丁 `patches/opencode-zen.cordis.patch.yml`（`https://opencode.ai/zen/v1`）仅保留给调试：`--provider opencode-zen`。Zen 付费档会走 Zen 余额（此前 402），与 Go 月费不是同一条计费路。
- `--provider` 可选：`auto`（默认，Go 优先）、`opencode-go`、`opencode-zen`、`deepseek-official`。

切勿把密钥写入 report、state.json、receipt、workspace 或 git。

## 怎么用

正式生产路径与角色、消息类型、状态机（含文案审稿闸 t2882u）、超时重试和 skill 地图见 **[COMMS_ARCHITECTURE.md](./COMMS_ARCHITECTURE.md)**。Grok Bot 只锁需求与资产、验收和汇报；对标检索/文案/分镜/渲染由 dsh 完成。调度会按 `skill_map.json` 把技能拷进工作区 `.skills/`，dsh 须在 `self_check.md` 记录 `skills_used`。同一任务的后续轮用 `bin/aal_followup.py` 走同一条 dsh CLI 会话（`--session-id`）。不要用临时直调 dsh 充当正式素材任务。


### L1 Bot 怎么提交任务、读卡片、续跑

1. **锁 brief**：按 `brief.schema.json` 写好并校验。全部文案默认是草稿，主人（t2882u）审过之前不要往下做，更不要发布。
2. **提交**：`dsh_system/bin/aal_submit.py <brief.json>`  
   文件进入 `inbox/`，监视器校验后移到 `inbox/claimed/<task_id>.json`，向本机中台发出 `brief.approved`（subject = task_id）。也可以在中台已启动时直接 `agent-autoloop emit brief.approved --id brief-<task_id> --subject <task_id> --summary '…' --detail '{"brief":"绝对路径","task_id":"…","workspace":"<WORK_DIR>"}'`（不要把本地路径放进 `--link`，它只接受网址）。
3. **跟进**：轮询工作区 `<WORK_DIR>/DONE.json` 或 `queue/done/<task_id>.json`，以及中台 `agent-autoloop status`。
4. **读卡片**：拿不准的决定写在 `dsh_system/cards/card-<id>.json`，目录索引在 `cards/index.json`。**只落本机文件，不发外部通道。** 回答：`agent-autoloop answer <id> <序号>` 或 `bin/aal_answer.py <id> <序号或选项原文>`。`fyi` 通知会自动结案，文件仍保留给 Bot 读。
5. **Grok 生图续跑**：dsh 写出 `gen_requests.json` 后，L2 会发一张「已履约，继续制作」卡片，并约 30 分钟后复查。Bot 把文件写到工作区 `grok_gen/` 并写 `gen_results.json` 后，选「继续」，或 `agent-autoloop emit grok_gen.fulfilled --id grok-<task_id> --subject <task_id> --summary '…' --detail '{"task_id":"…","workspace":"…"}'`。包装层会调用 `dsh_dispatch.py --resume --task-id <id>`。
6. **启停**：`bin/aal_start.sh`、`bin/aal_stop.sh`、`bin/aal_status.sh`。安装（幂等）：`bin/aal_install.sh`。中台只监听 `127.0.0.1:3350`。

直接派活（绕过 L2，仍可用）的原命令：

```bash
# 探测车道（不跑任务，不打印密钥）
dsh_system/bin/probe_opencode.py
dsh_system/bin/dsh_dispatch.py --probe-only

# 派一个已锁定的 brief
dsh_system/bin/dsh_dispatch.py \
  --brief dsh_system/examples/brief.image_text.xhs.smoke.json

# Grok 生图履约后续跑
dsh_system/bin/dsh_dispatch.py --resume --task-id <task_id>
```

产物与状态都在 `<WORK_DIR>/`：

- `brief.json` `AGENTS.md` `.dsh/skills/dsh-executor/SKILL.md`
- `output/` `asset_manifest.json` `self_check.md` `run.jsonl`
- `state.json` `pending.json` `ack.json` `receipt.json` `DONE` / `DONE.json`
- 需要 Grok 生图时：`gen_requests.json`，状态 `waiting_grok_gen`（协议见 `protocols/GROK_GEN.md`）

Grok Bot 轮询 `DONE.json` 或 `queue/done/<task_id>.json` 即可，不必靠记忆。调度会话可在 `<LOCAL_DSH_SESSION>/` 看到，dsh web（3080）远程控制也能看到。

## Grok 生图/生视频协议（摘要）

dsh 不能直接调用 Grok 的生成接口。它写 `gen_requests.json`（字段：`id`, `kind` image|video, `prompt`, `ref_images`, `aspect`, `duration`, `purpose`），把 `state.json` 设为 `waiting_grok_gen` 后退出。Bot 把文件写到 `grok_gen/<id>.*` 并写 `gen_results.json`，再 `--resume`。详见 `protocols/GROK_GEN.md`。

## 执行质量条（skill）

`skills/dsh-executor/SKILL.md` 会在每次派活时拷进 workspace。要点：精修成品、尽量 3 个真实高互动参考（打不开就写 `evidence_gap`，禁止编造）、昂贵渲染前先做关键帧、只交一份成品、中文去 AI 腔、禁止 AGI 宣称、硬切广告、配音标注 `grokko_sweet`、`commercial_use: true` 时不用 anything2explainer。

## 工具清单（诚实盘点）

状态含义：

- **INSTALLED**：本机已装，路径可核对。
- **AVAILABLE**：找到公开仓库或文档，给出 URL 与已知许可证；**本机未装**。Phase 1 **没有**为省事而大批安装（磁盘约 94% 已用，约 7.7G 剩余；主人规则：不花钱、不绑卡）。
- **NOT_FOUND**：按常见公开源未找到可安装的实在项目，或只有产品站而无可安装开源包。
- **STUB**：本 Phase 用本地脚本/文件代替，不是原产品。

许可证以仓库当时声明为准，未打开 LICENSE 的标「见仓库」。

### L1 决策侧（Bot 用，本机未作为 Phase 1 安装目标）

| 名称 | 状态 | 来源 / 许可 | 说明 |
|---|---|---|---|
| xai-plus / grok-x-trends | NOT_FOUND（作为可装 CLI/skill 包） | — | 说明书中的 X 趋势工具；未在本机找到独立安装。Bot 已有 X 连接器，热点扫描仍归 L1。 |
| trendradar | NOT_FOUND（未核实到可装包） | 社区清单有提及，未核到唯一仓库 | 不下放。 |
| okdata | NOT_FOUND | — | 不下放。 |
| jev-social-research | AVAILABLE（待核对） | [socai-io/jev-social](https://github.com/socai-io/jev-social)（许可见仓库） | 名称相近，**未装、未核是否即说明书所指**。 |
| grokbot-usage-cli | AVAILABLE，未装 | [bcharleson/grokbot-usage-cli](https://github.com/bcharleson/grokbot-usage-cli) | dispatcher 已预留读 `<LOCAL_USAGE_FILE>`；文件不存在则告警继续。 |
| Planify MCP | NOT_FOUND | — | 发布层，L5，Phase 1 不做。 |
| dsh-social-publishing | NOT_FOUND | — | 发布层，L5，禁止由 dsh 触发。 |

### L2 / L3 基础设施

| 名称 | 状态 | 来源 / 许可 | 说明 |
|---|---|---|---|
| agent-autoloop | **INSTALLED** | [KKKKhazix/agent-autoloop](https://github.com/KKKKhazix/agent-autoloop) **MIT**，v1.8.3（`1bd9aa25`） | 装在 `<LOCAL_AUTOLOOP_CONFIG>`，CLI `<LOCAL_BIN>/agent-autoloop`。规则：`<LOCAL_AUTOLOOP_CONFIG>`（规范副本 `rules.autoloop.json`）。本目录 `rules.json` 仍是 dispatcher 的配方映射。卡片通道是自研 `plugins/file-cards`（只写本地 JSON）。内置 dsh 适配器未启用（要 0.2.2+）。参考仓 yangfan27/agent-autoloop-wb-dsh 未装，本机包装层已覆盖派活与续跑。 |
| grokbot-local-agent | 脚本+Pi CLI 已装；**登录与 webhook 未完成** | [robbin/grokbot-local-agent](https://github.com/robbin/grokbot-local-agent) @2e48e05；`<LOCAL_PI_TOOLS>`；Pi 1.5.2 | 按官方文档：主人 `pi /login` + 例程凭据后才做冒烟与 pending/ack。未闭环前不退役 local_agent_bridge。 |
| pi-task | NOT_FOUND（本机 which 无） | 通常随 Pi 助手 | 未装。 |
| bot-keepalive | AVAILABLE，未装 | [realTaki/bot-keepalive](https://github.com/realTaki/bot-keepalive) | 保活会消耗 Bot 额度；Phase 2 再议。 |
| Wake on Done | AVAILABLE，未装 | [eze-is/eze-grokbot](https://github.com/eze-is/eze-grokbot)（wake-on-done） | 需建 relay Bot 与 webhook，**未装**（涉及对外通知，需主人明确批准）。现用 `DONE.json` 轮询。 |

### L4 执行侧（dsh 插件 / 制作工具）

| 名称 | 状态 | 来源 / 许可 | 说明 |
|---|---|---|---|
| dsh 本体 | INSTALLED | `@deepseek-ai/dsh` 0.2.1-rc.2，`<LOCAL_BIN>/dsh` | headless / web / free-test profiles。 |
| dsh-our-free-model 等免费车道插件 | INSTALLED（web / free-test） | npm `dsh-our-free-model` 等 | 免费 Zen 车道此前对本出口 429；与新钥匙探测一致。 |
| dsh-art-workshop | AVAILABLE，未装 | [liuzhuomin/dsh-art-workshop](https://github.com/liuzhuomin/dsh-art-workshop)（许可见仓库）；介绍页 [dsh.works](https://dsh.works/p/dsh-art-workshop) | 全链路工坊。磁盘紧，Phase 1 不装。 |
| dsh-motion | AVAILABLE，未装 | 与说明书「一句话做动画」更接近：[vlln/dsh-motion](https://plugin.dshdesk.com/plugins/vlln/dsh-motion/)；另有 [cogito0823/dsh-motion](https://github.com/cogito0823/dsh-motion)（Manim 数理动画，另一项目） | 两个不同仓库，安装前要选对。 |
| dsh-media-desk | AVAILABLE，未装 | [woniuniuniu/dsh-media-desk](https://github.com/woniuniuniu/dsh-media-desk) **MIT** | 图文工作台。账号导入走付费第三方，默认不装。 |
| dsh-imagegen-video | AVAILABLE，未装 | [q253595422/dsh-imagegen-video](https://github.com/q253595422/dsh-imagegen-video) **Apache-2.0** | 仍需生图/视频 API。 |
| dsh-media-workbench | AVAILABLE，未装 | 作者 cinderzhan，**MIT**（[dshindex](https://dshindex.com/plugins/dsh-media-workbench)） | Web UI 工作台。 |
| dsh-reel-studio / dsh-reel | AVAILABLE，未装 | [Eileenes/dsh-reel](https://github.com/Eileenes/dsh-reel) **MIT** | Remotion 宣传片模板，依赖较重。 |
| dsh-handraw-style | AVAILABLE，未装 | [yang0/dsh-handraw-style](https://github.com/yang0/dsh-handraw-style)（许可见仓库） | 共享画风套图。 |
| anything2explainer | AVAILABLE，未装（有队内站规） | PolyForm **Noncommercial**；队内 `STANDING_ANYTHING2EXPLAINER_MERGE_T2221.md` | `commercial_use: true` 时禁用。 |
| HyperFrames | AVAILABLE，未装 | heygen-com/hyperframes，**Apache 2.0** | 确定性 HTML/GSAP→MP4。Phase 2 优先考虑（需 Node 22+、FFmpeg，安装体积要先估）。 |
| video-talkcraft | AVAILABLE，未装 | [Vincentwei1021/video-talkcraft](https://github.com/Vincentwei1021/video-talkcraft)（许可见仓库） | 口播对齐 Remotion。 |
| Motion Harness | AVAILABLE，未装 | [mad-helpers/motion-harness](https://github.com/mad-helpers/motion-harness)（许可见仓库） | 程序化动效管线。 |
| Motion Prompt | AVAILABLE，未装 | [showheyohtaki/motion-prompt](https://github.com/showheyohtaki/motion-prompt) **MIT**；[图库](https://showheyohtaki.github.io/motion-prompt/) | 命名动效库，依赖很轻，Phase 2 值得先装。 |
| design-to-motion | AVAILABLE（迁移中） | 原在 elayadesign/ai-design-skills，**MIT**；2026-11 有独立仓迁移记录 | 装前再核当前公开 URL。 |
| landing-page-design | AVAILABLE，未装 | elayadesign/ai-design-skills 现含此项（许可见仓库） | 网页 format。 |
| open-claude-ui / claude-ui-skills | 未在本机核实到唯一仓库 | — | 标为待核，不假装已找到。 |
| awwwards-mcp | AVAILABLE，未装 | [INSANE0777/Awwwards-mcp](https://github.com/INSANE0777/Awwwards-mcp)（许可见仓库） | 网页参考。 |
| Lazyweb / Mobbin | 产品/资料库，非本机已装工具 | Mobbin 为商业产品；Lazyweb 需再核是否有可免费自用的开源客户端 | **不装、不绑卡**。网页 brief 可手写 `reference_sources.md`。 |

### 本机已有、可作 Phase 1 替代的能力

| 名称 | 状态 | 路径 / 说明 |
|---|---|---|
| Remotion 相关 bits | INSTALLED（队内 vendor） | `<LOCAL_VENDOR_PATH>` 等；不是 video-talkcraft 本体。 |
| dsh-video-storyboard | INSTALLED（vendor） | `<LOCAL_VENDOR_PATH>`，MIT。拆片用，不是生产线。 |
| de-ai-zh / sparkling | INSTALLED（skill/vendor） | 去 AI 腔，执行 skill 已要求使用其原则。 |
| agent-reach | INSTALLED | 只读读帖，可补参考证据；执行层需要时再用，Phase 1 冒烟未强制。 |
| HTML/CSS + Chrome | INSTALLED | 图文卡片的 Phase 1 主路径。 |
| 官方 DeepSeek `deepseek-flash` | INSTALLED（已有钥匙） | 在 OpenCode 余额不足时的制作回退车道。 |

## Phase 1 刻意做成的 STUB

- （已取消）完整 agent-autoloop 中台：已于 2026-11-13 安装 v1.8.3，不再是 stub
- pi-task / grokbot-local-agent → 磁盘 pending/ack/DONE
- bot-keepalive / Wake on Done → 尚未安装；DONE 文件供轮询
- 各媒体插件流水线 → skill 要求用 HTML/CSS、分镜、源码，需要时走 Grok 协议
- 用量刹车的真实读数 → 文件缺失则告警继续（Phase 1 并行度本来就是 1）

## Phase 2 建议（先小后大）

1. **OpenCode 钥匙**：若要坚持说明书中的 `deepseek-v4.1-flash`，需在 OpenCode 控制台给该组织充值；在此之前制作走官方 `deepseek-flash`。不要把钥匙再贴到聊天里。
2. **磁盘清理后再装工具**：优先轻量的 **Motion Prompt（MIT）**；其次评估 **HyperFrames（Apache 2.0）** 体积后再决定。不要一次装齐媒体插件。
3. **A1 热点扫描** 继续留在 Grok Bot，跑稳两周再谈「发现层」下放；选题权不放。
4. **Wake on Done / bot-keepalive**：要装需主人明确批准（relay Bot、webhook）。可先用现成 DONE 轮询。
5. **grokbot-usage-cli**：若要让额度刹车有真实数字，再装并定期写 `<LOCAL_USAGE_FILE>`（注意其读取账号端点的权限边界）。

## 文件地图

```
dsh_system/
  README.md
  brief.schema.json
  rules.json                    # dispatcher 配方（不是中台规则）
  rules.autoloop.json           # L2 正式规则的规范副本
  rules.phase1-stub.json        # 安装前备份的 Phase 1 stub
  examples/brief.image_text.xhs.smoke.json
  examples/brief.image_text.xhs.aal-smoke.json
  protocols/GROK_GEN.md
  protocols/gen_requests.schema.json
  protocols/gen_results.schema.json
  patches/*.cordis.patch.yml
  skills/dsh-executor/SKILL.md
  plugins/file-cards/channel.ts
  bin/dsh_dispatch.py
  bin/dsh_lib.py
  bin/probe_opencode.py
  bin/aal_install.sh aal_start.sh aal_stop.sh aal_status.sh
  bin/aal_dispatch.py aal_inbox_watch.py aal_submit.py aal_answer.py
  inbox/  inbox/claimed/  cards/  queue/{pending,ack,done}/
<LOCAL_AUTOLOOP_CONFIG>              # 程序、config.json（0600，含中台密钥）、正式 rules.json、hub.db
```

## 冒烟结果（2026-11-13 14:00 上海时间 · OpenCode Go 主路）

- probe：Go `models` 200；Go `deepseek-v4.1-flash` 补全 200（`PONG`）；官方 `deepseek-flash` 回退车道仍 200。
- 真实 dsh 短跑：工作区 `/tmp/dsh-go-smoke-run`，session `dsh-smoke-go-run`，4.54 秒，exit 0，最终文本 `PONG`。会话记录确认走的是 `opencode-go` / `deepseek-v4.1-flash`，不是官方 flash，也不是 Zen。

## 冒烟结果（2026-11-13 13:36–13:39 上海时间）

任务：`examples/brief.image_text.xhs.smoke.json`  
工作区：`<WORKSPACE_ROOT>/smoke-20261113-xhs-image-text/`  
车道：自动回退到官方 **`deepseek-flash`**（OpenCode `deepseek-v4.1-flash` 402）。  
耗时约 212 秒，18 步熔断（`max_steps`），约 1.5 万输入 / 2.3 万输出 token。

dsh 写出了 `output/cards.html`（三张 1080×1440、画风统一的 HTML 卡片）。**没有**写出 `asset_manifest.json`、`self_check.md`、可粘贴的小红书正文，也 **没有** 把 PNG 留在 `output/cards/`（末几步在用 Chrome 截图，曾挂起，熔断时被切断）。`run.jsonl` 目前只有 dispatcher 的起止事件，dsh 自己的工具账在 `logs/dsh.jsonl`。

审阅截图（**验收人用 Chrome 从同一 HTML 截出，不是 dsh 交付的 PNG**）：`logs/smoke-review/review-card-{1,2,3}.png`。

质量判断（诚实）：

- 文案方法向、可照做，没有 AGI 宣称，也没有「赋能/闭环」类套话。三张钩子清楚，系列感在。
- 第 1、3 张视觉可用，接近可发的封面级。
- **第 2 张不合格**：底部黑底提示框与页脚重叠，字被切掉（「未确认」一行压在页脚上）。不能直接当成品发。
- 没有 3 个真实高互动参考（`evidence_gap`），也没有独立的小红书标题+正文+标签文件。
- 结论：管道打通，图文草稿 **部分可用**；要当可发布成品，至少要修第 2 张重叠、补文案与自查，并把截图做到交付物里。建议把「写 HTML」和「截图+写合约文件」分成两轮，或把默认 `max-steps` 再留出截图余量。

## 冒烟结果（2026-11-13 14:18–14:20 上海时间 · L2 agent-autoloop 端到端）

任务：`examples/brief.image_text.xhs.aal-smoke.json`（小红书三卡**文案草稿**，安全主题「用 AI 之前先核三件事」，明确只要 Markdown、不要 HTML/截图/生图，全部标【草稿·待主人审】）。

路径：`aal_submit.py` → `inbox/` → 监视器发出 `brief.approved` → 中台规则「制作内容」派给 `dsh-dispatch` → `dsh_dispatch.py` 在锁定工作区跑 dsh。

- 工作区：`<WORKSPACE_ROOT>/smoke-20261113-aal-xhs-copy/`
- 车道：**opencode-go / deepseek-v4.1-flash**（Go 主路已通，没有回退）
- 耗时 89.84 秒，11 步，约 1.3 万输入 / 0.7 万输出 token，exit 0，状态 `completed`
- 产物：`output/copy.md`、`output/card-{1,2,3}.md`、`asset_manifest.json`、`self_check.md`、`run.jsonl`；四份文案开头均有【草稿·待主人审】；`evidence_gap` 已记账（未打开外网参考）；没有 HTML、没有发布、没有 `gen_requests.json`
- 回执：`receipt.json`、`DONE`、`DONE.json`、`queue/done/smoke-20261113-aal-xhs-copy.json`
- 卡片：`cards/card-1.json`（fyi 通知「草稿待审」，已自动结案，文件仍在），索引 `cards/index.json`
- 中台：`agent-autoloop why 1` 可见 14:18:41 触发、14:20:17 结束
- **会话可见性**：`<LOCAL_DSH_SESSION>`（共享 `DSH_HOME=<LOCAL_DSH_RUNTIME>`）。会话文件扫描未发现钥匙形态字符串；`<REDACTED_CREDENTIALS_FILE>` 与 headless 配置的修改时间未变。

结论：L2 管道打通（排队 → 派活 → dsh 出稿 → DONE → 本机卡片）。文案是草稿，按 t2882u **须主人审过再往下做**，不能当已发布成品。

