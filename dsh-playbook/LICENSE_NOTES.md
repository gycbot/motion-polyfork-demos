# 许可说明

本仓库是一份消毒后的制作规范与技能包，供仓库所有者在本机用 WorkBuddy + Hy4 对照实验。把文件放进公开 Git 不等于把其中所有内容改成可商用许可。

## 1. 醒目：anything2explainer 为非商用

**`skills/anything2explainer/` 采用 PolyForm Noncommercial 1.0.0。**

- 非商用使用免费。
- **不得写成可商用。** 会员费、付费交付、商业项目或其他商用，须事先取得作者书面授权，才能使用该工具包的代码、模板或脚本。
- 本仓库**只收录**团队入口 `SKILL.md` 与本警告，**没有**收录官方工具包源码、Remotion 模板、图库、`node_modules` 或媒体。
- 需要完整工具包时，请自行从上游克隆并保留其 LICENSE：https://github.com/Vincentwei1021/anything2explainer
- 本仓库的对照任务（1 分钟精剪）**禁止**把该技能当作主路径。

## 2. 本团队撰写的规范与 skill

下列内容由本团队编写或改编为队内入口，**未另作开源许可声明的，版权由作者保留**，允许仓库所有者本人及其授权协作者为对照实验、学习与改进制作流程而使用、修改本地副本。转载或再分发请保留本说明与原文出处，不要删除第三方许可声明。

包括但不限于：

- `dsh_system/` 下的 README、通信说明、brief schema、skill_map、rules、示例 brief、`dsh-executor`、`AGENTS.md.template`
- 队内入口 skill：`skill-first-task`、`de-ai-zh`、`narration-av-sync`、`motion-house-style`、`oil-cover-agent`、`preview-deliver-split`、`scout-video-repurpose`、`viral-copy-zh`、`user-taste-transfer`、`x-bookmark-drygoods-video`、`x-bookmark-learn-triad`、`video-storyboard-dsh`、`vibe-oneshot-remotion-brief`、`concept-diagram-tutorial-pack-2`、`kinetic-typefx-sell-short`、`grokbot-chat-demo-sop`、`surface-web-craft-kit`，以及各站规摘要
- `TASK_1MIN_HOOK.md`、`README.md` 等本包文档

若干 skill 正文注明「来源只读、不整装」的上游项目，那些上游自己的许可仍然有效，例如：

| 在 skill 中被引用的上游（只读，本仓库未整仓收录其源码） | 其声明的许可 |
|------------------------------------------------------|--------------|
| https://github.com/threerocks/article2media | MIT（见 skill 正文记载） |
| https://github.com/yanhua1010/self-media-content-workflow | MIT（见 skill 正文记载） |
| `alchaincyf/huashu-art-motion` | MIT（见 motion-house-style 正文） |
| tts-director 类「为耳朵改稿」蒸馏（narration-av-sync 注明 MIT，无标签） | 按原文 |

本仓库**没有**把巨大的 vendor 工具包源码无筛选塞进来，也没有收录 voice_library 的 wav、Remotion 工程或媒体。

## 3. 与他人影像、翻译稿有关的内容

对照任务的源片是对 [Tech With Tim《Claude Fable 5.2 is INSANE》](https://www.youtube.com/watch?v=xKlUf-dw_Lw) 的中文翻译搬运成片。

- **成片 mp4、预览、真实帧封面、完整口播稿与字幕不进入本仓库。**
- 使用该源片做精剪时，必须保留出处与「本视频为 AI 技术搬运翻译」，不冒充原作者或官方。
- 精剪产物同样不要 push 进公开仓库。

## 4. 明确未收录、因而也不在本许可范围内的东西

- 密钥、token、cookie、agent-autoloop 配置、`.secrets`
- 本机绝对路径所指的运行时、内网 Funnel、监控密码
- 客户照片、内推码、邮箱登录、私密看板
- grokko_sweet 音色 wav 与完整 voice_library
- anything2explainer 官方工具包全文
- 其它 vendor 大工具包（Humanizer 原文仓、GSAP 插件包、审美技能仓等）

## 5. 软件依赖（操作者本机自行安装）

执行对照任务通常需要本机已有 `ffmpeg` / `ffprobe`。它们遵循各自的上游许可（FFmpeg 主要为 LGPL/GPL，视你安装的构建而定），本仓库不附带其二进制。
