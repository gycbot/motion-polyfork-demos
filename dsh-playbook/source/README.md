# 源片放置处（不要把 mp4 提交进 Git）

把已过审的 Fable 5.2 中文搬运成片拷到本目录，文件名必须是：

```text
final_bilibili_16x9.mp4
```

拷完后先核验，再开工：

```bash
# 时长约 528.2 秒，1920x1080，60 fps
ffprobe -v error -show_entries format=duration,size -show_entries stream=width,height,r_frame_rate,codec_type -of default source/final_bilibili_16x9.mp4

md5sum source/final_bilibili_16x9.mp4
# 期望：18ba7833af875e7e1cadd0530cf92457
```

可选配套（同样不要提交进公开仓库）：若你本地有上一轮的 `subtitles.zh.srt`、`script.md`、`description.md`，也可以放在本目录，方便精剪时对齐字幕与简介。

详见 `SOURCE_MANIFEST.md`、`TASK_1MIN_HOOK.md`。`.gitignore` 已忽略本目录下的媒体与稿件，只保留这份 README。
