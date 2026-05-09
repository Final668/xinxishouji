# GitHub Pages PR 冲突排查说明

## 当前截图里的问题

截图中显示 Pull Request 下方出现：

- `Checks awaiting conflict resolution`
- `This branch has conflicts that must be resolved`
- 冲突文件：`README.md`、`index.html`

这说明 **PR 分支和目标分支 `main` 同时修改了相同文件**，GitHub 无法自动判断应该保留哪一份内容，所以 Pages 构建检查会等待冲突解决。

## 为什么会这样

当前仓库的 `main` 分支已经有一版 GitHub Pages 首页，而 PR 分支又把 `index.html` 改成新的“多平台信息搜集工作台”。同时，两个分支都修改了 `README.md` 的“在线网页”说明，因此 GitHub 标记这两个文件冲突。

这不是网页代码一定坏了，而是合并前的 Git 冲突。截图里也能看到 GitHub Pages 已经部署过该分支，但 PR 仍然不能合并，直到冲突解决。

## 应该保留哪一版

如果目标是让网页出现明显的“开始搜集信息”按钮、规则配置入口、模拟运行日志和 JSON 导出，应保留 PR 分支里的新版内容：

- `index.html`：保留“合规数据采集工作台”版本。
- `README.md`：保留包含“开始搜集信息”“规则配置”“模拟结果预览”“JSON 任务配置”的说明。

## 在 GitHub 网页上解决

1. 打开 PR 页面。
2. 点击 **Resolve conflicts**。
3. 分别处理 `README.md` 和 `index.html`。
4. 删除冲突标记：
   - `<<<<<<<`
   - `=======`
   - `>>>>>>>`
5. 对 `index.html`，保留包含以下入口的版本：
   - 顶部导航 `开始搜集`
   - 按钮 `🚀 立即开始搜集信息`
   - 区块 `开始搜集信息`
   - 区块 `搜集结果预览`
6. 对 `README.md`，保留“在线网页”章节里的 GitHub Pages 地址和静态工作台说明。
7. 点击 **Mark as resolved**。
8. 点击 **Commit merge**。

## 在命令行解决

如果可以在本地访问 GitHub 仓库，可以执行：

```bash
git checkout codex/find-how-to-browse-webpage-0n2n2o
git fetch origin main
git merge origin/main
```

如果出现冲突，打开 `README.md` 和 `index.html`，按上面的保留策略处理后执行：

```bash
git add README.md index.html
git commit
git push origin codex/find-how-to-browse-webpage-0n2n2o
```

## 解决后如何确认

冲突解决并推送后，PR 页面应该不再显示 `This branch has conflicts`。随后等待 `pages build and deployment` 检查重新运行，通过后再合并 PR。
