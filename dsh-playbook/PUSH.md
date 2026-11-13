# 推送到 GitHub（尚未推送）

Cursor / 打包工人**不能代替仓库所有者新建 GitHub 仓库**。本目录已经在本地 `git init`，并有一份干净的初始 commit。

## 不要推到这些已有仓库

下列都是 `gycbot` 名下已有内容的公开库，**不适合**作为本规范包，推上去会污染展示站或其它项目：

- `gycbot/grokbot-showcase`
- `gycbot/avengers-remix`
- `gycbot/dsh-monitor`（加密监控页，不是 playbook）
- `gycbot/motion-polyfork-demos`

不要 force-push。不要改写这些仓库的历史或 GitHub Pages。

## 建议的新仓库

- 建议名： **`gycbot/dsh-playbook`**
- 可见性：由所有者决定。若公开，请再次确认没有误加 mp4 或密钥（先看 `git status` 与 `SANITIZE.md`）。
- 空仓库即可，不要勾选「用 README 初始化」，以免第一推需要合并。

所有者在 GitHub 网页创建空仓库后，把 URL 交给队长，再执行（把 URL 换成真实地址）：

```bash
cd /path/to/prod-20261114-dsh-playbook-pack
git remote add origin https://github.com/gycbot/dsh-playbook.git
git branch -M main
git push -u origin main
```

若已用 SSH：

```bash
git remote add origin git@github.com:gycbot/dsh-playbook.git
git branch -M main
git push -u origin main
```

不要使用 `--force`。推送前再跑一次：

```bash
git status
git log --oneline
git ls-files | grep -Ei '\.(mp4|wav|key|env)$' && echo 'ABORT: media or key staged' || echo 'no media/key in index'
```

本包打包时**尚未**设置 `origin`，也**尚未**推送。待空仓库 URL 确认后再推。
