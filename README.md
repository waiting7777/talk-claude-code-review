# Claude Code 團隊怎麼審 AI 寫的程式

Slidev 投影片，10 分鐘分享。整理自 Simon Willison 訪談 Cat Wu、Thariq（AI Engineer World's Fair 2026）。圖片取自 Wayne AI 工作室〈EP2〉影片截圖，各頁已標出處。

```sh
npm ci
npm run dev      # 本機播放，講稿在 presenter 模式（按 p 或開 /presenter）
npm run export   # 匯出 PDF
```

## 一頁版（archify）

`explainer.html`：用 archify 做的一頁式說明，結論先講，圖都是自己生成的，直接用瀏覽器打開。原稿是 `explainer.archify.md`，重做：

```sh
node ~/.claude/skills/archify/bin/archify.mjs finalize explainer explainer.archify.md explainer.html --json
```
