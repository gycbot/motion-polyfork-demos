# 消毒记录（SANITIZE）

打包日期：2026-11-14（Asia/Shanghai）。目的：让规范与 skill 可以放到公开 Git 仓库，供所有者在本机用 WorkBuddy 对照，同时不泄漏密钥、内网与不应公开的媒体。

## 做了哪些消毒

1. **密钥与凭据：整份不收录**
   - 未收录任何 `.secrets`、钥匙文件、cookie、`auth.json`、`.env`、agent-autoloop 的 `rules.autoloop.json`（其中含渠道与代理配置）。
   - 原 dsh README / STATUS 里提到的本机钥匙路径、监控页密码文件、Funnel / Tailscale 内网入口，已删除或改为 `<REDACTED_SECRET_PATH>`、`<REDACTED_INTERNAL_URL>`。本说明不再复述具体文件名。
   - `dsh_system/rules.json` 去掉了指向本机调度脚本和用量文件的事件配置，只保留 format→配方合同。

2. **本机绝对路径：改成相对路径或占位符**
   - `/home/box/...`、`/workspace/...`、`~/.agent-autoloop` 等已替换为 `skills/`、`dsh_system/`、`standing/`、`<SOURCE_DIR>`、`<WORK_DIR>`、`<LOCAL_VOICE_REF>`、`<REDACTED_*>`、`<OPTIONAL_*>`。
   - 示例 brief 是重写的，不是原文搜索替换；资产与站规指针全部是包内相对路径或占位符。
   - `dsh_system/skill_map.json` 的 `skill_root`、`executor.source`、`optional_roots`、`toolkits` 已改为包内相对路径，并注明工具包未捆绑。

3. **未收录的整类内容**
   - 成片 / 预览 mp4、封面 jpg、口播 wav、客户照片。
   - `STATUS.md`、运行日志、queue、inbox、bus、cards、monitor、bin 调度脚本、OpenCode 补丁、虚拟环境。
   - 内推相关 skill 与海报流水线、账号登录说明书、私密看板。
   - 巨大 vendor 工具包源码（anything2explainer 模板与依赖、de-ai 上游全文、GSAP 技能仓、voice_library 等）。只保留队内入口 `SKILL.md` 和少量 helper（如 `steps.yaml`）。
   - skill 的 `.bak` 备份。

4. **站规**
   - 只收录与搬运 / 制作质量相关的站规，并做了路径消毒。
   - 保留 t 号（如 t2923u、t2788u）是允许的。
   - 已去掉密钥、登录步骤、本机路径。账号运营层面的「何时公开」在站规原文里可能仍有历史决策，**本对照任务一律不发布**，以 `TASK_1MIN_HOOK.md` 为准。
   - 原声规则以站规后写补充为准：必须**削尽**原片原声，不是压低垫底。见 `standing/README.md`。

5. **Git 防护**
   - `.gitignore` 忽略 mp4/wav/密钥/`.env`/`source/` 下的媒体与稿件。
   - 建议仓库名是新的空仓；明确不要推到 `gycbot/grokbot-showcase` 或 `gycbot/avengers-remix`，不要 force-push。

## 打包后复查（初始 commit 前）

- 全文检索不再出现 `/home/box`、`/workspace/` 字面量（本文件与 README 如需举例只用占位符）。
- 未发现 `api_key=`、`oc_`/`sk-`/`gho_` 这类密钥值、私钥头、明文密码。
- 未发现 Tailscale / 内网 Funnel 主机名、邮箱登录。
- 仓库内无 mp4、wav、jpg、`.key`。

若你在使用中发现残留的本机路径或凭据，请先不要推送，把它改成占位符并记入本文件。
