# STANDING · 配音音画同轴（t187u/t188u → t2268u SUPERSEDE）

**状态**: ACTIVE HARD · 2026-11-03  
**触发**: t188u 禁再犯脱轴；t2268u 用户钉死 VO-first / 禁 rate 凑槽 / 默认关剧透 UI；IG-HLE 复盘 STOP

## 硬规则
1. **禁**整篇 `narration.txt` 一次 TTS 硬贴 `<Audio>`。
2. **禁**为凑 SUBS/SEG 槽升/降 TTS `--rate` / atempo（一块一慢丑）。
3. **VO-first**：先自然语速定旁白真实时长，再排画面切点/镜长；画面去匹配语音。短槽优先改镜长，不拿语速硬凑。
4. 画面已锁时：改镜长贴口播，仍禁 rate 硬凑。
5. **默认 UI**：不渲染 ChapterCard、不渲染底部章节进度条（防剧透）；除非 brief 点名。
6. 三点抽检「该秒字幕=口播」→ 再开渲。

## 正本 / 叠层
- Skill: `narration-av-sync`（sand-workflow:narration-av-sync）
- Overlay: `team/skills/explainer-avp-mechanism-overlay.md` → **X1b** + **X2 默认关剧透 UI**
- SAMPLE≠授发

## 队内
- IG vs HLE 复盘片：**STOP**（t2268u）；勿再渲 v4e/改点；sample_v4d **作废**收工。只记流程，不交付新样。
