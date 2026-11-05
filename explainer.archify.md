# Claude Code 團隊還需要 code review 嗎？ {#claude-code-review}

**需要。** 核心改動由 code owner 親自審，外圍改動交給 review bot，漏掉的問題再變成測試，讓 bot 越來越準。

- 產品工程約 65% 的 PR 由內部版 Claude 合併，但核心程式仍有人親自看。[](source:simon)
- 人工審查是一塊一塊撤掉的，花了六個多月。[](source:simon)
- 信任來自 eval：每次事故的 PR 都加進測試集。[](source:simon)
- 給 AI 的指示變短了，少寫禁令，多給背景。[](source:simon)

@answer scope="Claude Code 單一團隊的做法 · 2026-07 訪談 · 不代表 Anthropic 全公司"
@meta locale=zh-TW style=clean
@source simon title="Simon Willison：A Fireside Chat with Cat and Thariq from the Claude Code team（2026-07-21）" url=https://simonwillison.net/2026/Jul/21/cat-and-thariq/
@source talk title="AI Engineer World's Fair 2026 原始影片" url=https://www.youtube.com/watch?v=uU5Gv2h8-9g
@source wayne title="Wayne AI 工作室：EP2｜Claude Code 團隊怎麼審 AI 寫的程式？" url=https://www.youtube.com/watch?v=DolrT8FzAv4

## 一個 PR 由誰來審？ {#who-reviews}

@workflow id=review-route title="PR 審查分工"
| pr | 新的 PR | frontend |
| bot | review bot 先審 | backend |
| core | 動到核心？ | backend |
| owner | code owner 核准 | security |
| merge | 合併 | backend |
| pr > bot | 每個 PR |
| bot > core | |
| core > owner | 是 | emphasis |
| core > merge | 否 |
| owner > merge | 核准 |

@steps id=review-steps
| s1 | review bot 先審 | bot 會跑每一個 PR，很多時候大部分審查是它做的。 | review-route | bot |
| s2 | 動到核心？ | 核心指 Claude Code 本體與 system prompt 這類地方。 | review-route | core |
| s3 | code owner 核准 | 核心改動一定要 owner 核准，他對那塊品質負責。 | review-route | owner |

@metric id=merged-share label="內部版 Claude 合併的產品工程 PR" value=65 unit="%" note="Cat Wu 在訪談中的說法"

## 人工審查怎麼撤掉？ {#trust-steps}

@steps id=trust-order
| t1 | 全部人審 | 一開始每個改動都由人審。 |
| t2 | 看 bot 抓得準不準 | 按檔案範圍觀察，bot 是否抓到全部問題。 |
| t3 | 一塊一塊放手 | 某塊抓到 100% 的問題，才撤掉那塊的人工。 |
| t4 | 核心永遠有人 | 最關鍵的改動仍由 code owner 手動審。 |

@callout level=warning title="照實講的地方"
「抓到 100%」是 Cat Wu 說的撤人工條件，訪談沒講怎麼量。
整個過程花了六個多月，不是一次換掉。

## 漏掉的問題怎麼變成測試？ {#eval-loop}

@workflow id=eval-loop-flow title="事故回饋到 eval"
| incident | 發生事故 | external |
| find | 找出造成的 PR | backend |
| improve | 改 review bot | backend |
| add | PR 加進 eval | database |
| gate | 跑 eval 不退步 | security |
| incident > find | |
| find > improve | 為何沒抓到 |
| improve > add | |
| add > gate | 每次改動 | emphasis |

@steps id=eval-steps
| e1 | 找出造成的 PR | 事故檢討時回頭找那個 PR，問 review 要怎麼改才抓得到。 | eval-loop-flow | find |
| e2 | PR 加進 eval | 漏掉一次，測試集就多一個案例。 | eval-loop-flow | add |
| e3 | 跑 eval 不退步 | 之後改 bot 或換新模型，都先整套跑過，跑贏才換。 | eval-loop-flow | gate |

使用者抱怨的行為也一樣處理：排優先序，一個一個做成行為測試。Cat Wu 也承認覆蓋還沒到 100%。[](source:simon)

## 給 AI 的指示怎麼寫？ {#prompt-style}

| 面向 | 以前 | 現在 |
| --- | --- | --- |
| 長度 | 完整版 | 最新模型縮短約八成 {good:更短} |
| 範例 | 放很多範例 | 拿掉範例，模型發揮更好 |
| 寫法 | 多寫「不要做 X」 {caution:易衝突} | 多給背景 |
| 驗證規則 | 前端改動一律驗證 | 大幅改動介面時，才在本機跑起來看 |

Thariq 說，他們以前把模型限制得太多了。禁令和使用者指示衝突時，模型會搞混。[](source:simon)

@callout level=note title="適用範圍"
這是他們對最新模型的經驗。自己的模型適不適合變短，要用自己的測試確認。

## 回去可以檢查什麼？ {#takeaway}

- [ ] 打開你寫給 AI 的規則檔，逐句問：這一句每一次都成立嗎？
- [ ] 不是每次都成立的句子，改成寫清楚什麼情況才要做。
- [ ] 想想一個出於善意的人，會怎麼誤解這段指示。

中文整理與插圖可參考 Wayne AI 工作室的影片。[](source:wayne) 原始對談影片見 AI Engineer World's Fair。[](source:talk)
