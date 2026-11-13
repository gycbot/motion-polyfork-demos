# Grok 生图/生视频协议（Phase 1）

dsh **不能**直接调用 Grok Bot 的 image / video 生成能力。需要时走磁盘协议，由 Grok Bot 履行后再让 dsh 续跑。发布仍只由 Grok Bot 做。

## 目录约定

全部路径相对于该任务的 `workspace`（`<WORK_DIR>/`）：

| 文件 | 谁写 | 含义 |
|---|---|---|
| `gen_requests.json` | dsh | 待生成请求列表，符合 `gen_requests.schema.json` |
| `state.json` | dsh 先写 `waiting_grok_gen`；Bot 履约后改回 `running` 或由 dispatcher 续跑 | 当前状态 |
| `grok_gen/<id>.*` | Grok Bot | 每个请求的产物（png/jpg/webp/mp4） |
| `gen_results.json` | Grok Bot | 履约结果，符合 `gen_results.schema.json` |

## 流程

1. dsh 在 workspace 内做完文案、排版、分镜后，若需要 Grok 生图/生视频：写出 `gen_requests.json`，把 `state.json` 的 `status` 设为 `waiting_grok_gen`，然后**干净退出**（不要空转等待）。
2. dispatcher 看到该状态后写 `DONE` 标记（`status=waiting_grok_gen`），供 Grok Bot 轮询。
3. Grok Bot 逐条生成，把文件写到 `grok_gen/<id>.<ext>`，写 `gen_results.json`。
4. Grok Bot（或人）再次调用 dispatcher：`dsh_dispatch.py --resume <task_id>`。dispatcher 确认 `gen_results.json` 后把状态改为 `running`，把结果路径告诉 dsh，dsh 把成品纳入 `output/` 与 `asset_manifest.json`，写 `self_check.md`，结束。

## 请求字段

每条：`id`, `kind`（`image`|`video`）, `prompt`, `ref_images`（已在 workspace 内的相对路径，可空）, `aspect`, `duration`（视频）, `purpose`。

prompt 要写清画幅、用途、禁项（水印、假数据、品牌侵权）。不要在 prompt 或 JSON 里放任何 API key。

## 约束

- 只请求当前 brief 真正需要的条目，能用 HTML/CSS 或已有素材完成的不要请 Grok 生成（省额度）。
- `commercial_use: true` 时，prompt 不得要求受 PolyForm Noncommercial 等非商用许可证约束的风格包。
- Grok Bot 未履约前，dsh 不得把占位图当成终稿写入 `self_check.md` 的通过项。
