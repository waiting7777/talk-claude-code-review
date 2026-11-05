---
theme: seriph
title: 'Claude Code 團隊怎麼審 AI 寫的程式'
info: |
  ## Claude Code 團隊怎麼審 AI 寫的程式
  整理自 Simon Willison 與 Cat Wu、Thariq 的爐邊對談（AI Engineer World's Fair 2026）
author: Wayne Zheng
keywords: ai,code-review,claude-code,eval
exportFilename: claude-code-review
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Claude Code 團隊<br/>怎麼審 AI 寫的程式

## 從 code review 到人的判斷

<div class="abs-br m-6 text-sm opacity-60">
整理自 Simon Willison 訪談 Cat Wu、Thariq（2026-07）
</div>

<!--
大家好。今天分享一段訪談：做 Claude Code 的團隊，自己怎麼審 AI 寫的程式。
先講清楚，這是他們一個團隊的做法，不代表 Anthropic 全公司，也不是我們要照做的東西。
-->

---

# 開頭的一題

<div class="mt-8 text-2xl leading-relaxed">

AI 寫 code 變快了，**review 變成最貴的一段。**

</div>

<div class="mt-10 p-6 bg-blue-900/10 rounded-lg border border-blue-900/30 text-xl">

"Claude Tag currently lands **65%** of our product eng PRs."

<div class="text-right text-base opacity-60 mt-3">— Cat Wu，Claude Code 產品負責人</div>

</div>

<div class="mt-8 text-xl text-center">那他們還需要 code review 嗎？</div>

<!--
Cat Wu 說，產品工程團隊裡，內部版的 Claude 已經合併了大約 65% 的 PR。
那問題來了：他們還審嗎？怎麼審？
主持人 Simon Willison 是 Django 共同作者，問的就是這一題。
-->

---
layout: center
---

# 今天講三件事

<img src="/img/f002.jpg" class="h-80 mx-auto rounded shadow" />

<div class="text-center mt-4 text-lg">怎麼審 ・ 信任從哪裡來 ・ 怎麼給 AI 下指示</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
原本訪談有五十多分鐘，今天挑三件：怎麼審、怎麼測、怎麼給 AI 下指示。
每件事我都會講「他們怎麼做」和「為什麼這樣做」。
-->

---

# 一、怎麼審：兩層

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

<img src="/img/f010.jpg" class="rounded shadow" />

</div>
<div class="text-lg leading-relaxed">

**第一層：code owner**

核心的地方（例如 system prompt）有 owner，動到就要他核准，而且他親自看。

**第二層：review bot**

每個 PR 都跑，很多時候大部分的審查是它做的。

</div>
</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
這段是工程師 Thariq 講的。
重要的地方有 code owner，動到就要他核准。外圍的，GitHub 上有一個 review bot，每個 PR 都會跑。
-->

---

# 最值得學的是順序

<img src="/img/f012.jpg" class="h-72 mx-auto rounded shadow" />

<div class="mt-4 text-lg text-center">

一開始全部人審 → 某一塊 bot 抓到 100% 的問題 → 才撤掉那塊的人工

**花了六個多月，一塊一塊換下來**

</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
Cat Wu 說，他們花了六個多月才走到現在。信任是一小步一小步建立的。
注意：「抓到 100%」是她說的撤掉人工的條件，訪談沒講他們怎麼量，這點我照實說。
-->

---

# 一塊一塊換，每一塊都有依據

<img src="/img/f016.jpg" class="h-80 mx-auto rounded shadow" />

<div class="text-center mt-4 text-lg">不是一次全換，是先全部人審，再按檔案範圍逐步放手</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室（頻道觀點）</div>

<!--
這張是整理影片的觀點：最值得學的不是「AI 審」，是這個順序。
先全部人審，再一塊一塊換下來，每一塊都有依據。
-->

---

# 二、信任從哪裡來：eval

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

<img src="/img/f018.jpg" class="rounded shadow" />

</div>
<div class="text-lg leading-relaxed">

出事故 → 回頭找造成事故的 PR

→ 問「review 要怎麼改才抓得到？」

→ **把那個 PR 加進 eval 集**

漏一次，案例就多一個。

</div>
</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
Cat Wu 的答案是一組評估用的測試案例，叫 eval。
重點是：這個測試集測的是自動審查本身。漏掉一次，案例就多一個。
-->

---

# 改 review bot，先過 eval

<img src="/img/f020.jpg" class="h-72 mx-auto rounded shadow" />

<div class="mt-4 text-lg text-center">

之後對 review 做任何改動，都要跑過 eval，指標不能退步

換新模型也一樣：**整套跑一遍，跑贏才換**

</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
"The main reason we're building up this eval base over time is so that new models can be a drop-in replacement."
有了這套 eval，新模型出來就整套跑一遍，確認比舊的好才換上去。
-->

---

# 不只程式，行為也測

<img src="/img/f025.jpg" class="h-72 mx-auto rounded shadow" />

<div class="mt-4 text-lg text-center">

用戶抱怨 → 排優先序 → 一個一個做成行為測試

<span class="opacity-70">（例如用戶很不喜歡 AI 說「該去睡覺了」）</span>

</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
改 system prompt 時，Simon 問怎麼知道真的變好？
Cat 說沒有完全的把握，但會盡量確保不退步。很多影響使用感受的行為，就一個一個做成測試。
她也承認覆蓋還沒到 100%。
-->

---

# 三、給 AI 的指示：變短

<img src="/img/f028.jpg" class="h-72 mx-auto rounded shadow" />

<div class="mt-4 text-lg text-center">

最新模型的 system prompt **縮短約八成**

"One of the patterns we saw is that we were over-constraining Claude."

</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
Thariq 說，他們發現以前把模型限制得太多了。早期模型需要很多範例，現在把範例拿掉反而更好。
提醒：這是他們對最新模型的經驗，自己的模型適不適合變短，要自己測。
-->

---

# 少寫禁令，多給背景

<div class="grid grid-cols-2 gap-6 mt-2">
<div>

<img src="/img/f031.jpg" class="rounded shadow" />

<div class="text-base mt-2">「不要做 X」和使用者指示衝突時，模型會搞混</div>

</div>
<div>

<img src="/img/f032.jpg" class="rounded shadow" />

<div class="text-base mt-2">

~~前端改動一律驗證~~<br/>
→ 大幅改動使用者介面時，在本機跑起來看過結果

</div>

</div>
</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
Cat 舉的例子：原本寫「前端改動一律驗證」，但只是換一個字串、使用者又說快速修一下時，不一定需要。
所以改成只有大幅改動介面時才要在本機看結果。
-->

---
layout: center
---

# 帶走一個問題

<div class="mt-8 p-8 bg-blue-900/10 rounded-lg border border-blue-900/30 text-center">
<div class="text-3xl" style="line-height:1.8">你寫給 AI 的規則檔，</div>
<div class="text-3xl" style="line-height:1.8">每一句都是<b>每一次都成立</b>嗎？</div>
</div>

<div class="mt-8 text-lg text-center opacity-80">

Cat：想想一個出於善意的人，可能會怎麼誤解這段提示

</div>

<!--
這是今天大家最用得上的一段。回去打開自己的規則檔，逐句問：這一句每一次都成立嗎？
不是每次都成立的，就改成講清楚什麼情況下才要做。
-->

---

# 他們還需要 code review 嗎？

<img src="/img/f047.jpg" class="h-72 mx-auto rounded shadow" />

<div class="mt-4 text-lg text-center">

**需要。** 核心的人審，外圍的交給 bot，出事以後用 eval 讓 bot 越來越準

看多仔細，取決於這個改動**壞了會多嚴重**

</div>

<div class="abs-br m-4 text-xs opacity-50">圖：Wayne AI 工作室</div>

<!--
回到開頭那一題：他們還需要 code review 嗎？照他們自己的說法，需要。
核心的改動由 code owner 親自審，外圍的交給自動審查，出事以後再用測試集讓它越來越準。
我們這邊適不適合、怎麼用，之後可以跟 lead 討論。
-->

---

# 來源

<div class="mt-8 text-lg leading-loose">

- 原始訪談與逐字稿：Simon Willison，〈A Fireside Chat with Cat and Thariq from the Claude Code team〉，2026-07-21<br/>
  <span class="text-base opacity-70">simonwillison.net/2026/Jul/21/cat-and-thariq/</span>
- 原始影片：AI Engineer World's Fair 2026<br/>
  <span class="text-base opacity-70">youtube.com/watch?v=uU5Gv2h8-9g</span>
- 中文整理與圖片：Wayne AI 工作室，〈EP2｜Claude Code 團隊怎麼審 AI 寫的程式？〉<br/>
  <span class="text-base opacity-70">youtube.com/watch?v=DolrT8FzAv4</span>

</div>

<div class="mt-8 text-base opacity-70">以上是 Claude Code 一個團隊的做法，不代表 Anthropic 全公司。</div>

<!--
謝謝大家。原始逐字稿有八千字，有興趣可以看。中文整理那支十分鐘，圖很清楚，推薦。
-->
