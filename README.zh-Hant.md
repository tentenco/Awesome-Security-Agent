# Awesome Security Agent

[English](README.md) · [資源目錄](README.md#directory) · [防禦工作手冊](PLAYBOOK.md)

一份獨立整理的資源目錄，給用 Agent 處理防禦性資安工作的人。

值班時收到一則新公告，你得先確認自己的環境有沒有用到受影響的版本、廠商建議怎麼修，以及誰能核准變更。處理 SOC 告警或審查 Agent 建議的修補時，也會遇到類似的問題。我們收錄能幫團隊回答這些問題的工具、資料來源和實務做法。

## 使命

我們的使命是讓網站與基礎設施維運團隊也能查核 Agent 的防禦工作。我們整理的工具和方法，讓團隊能回到資料核對判斷，並自行核准後續變更。

## 宗旨（Purpose）

讓下一班接手的人能沿著紀錄繼續查。交付內容要交代 Agent 查了什麼、哪一筆資料支持判斷、還缺哪些證據，以及下一步由誰決定。

## 從哪裡開始

| 你要做的事 | 資源分類 |
| --- | --- |
| 選擇能協助調查或修補審查的 Agent | [資安助理](README.md#security-assistants) |
| 確認 CVE 是否適用，以及去哪裡找廠商修補資訊 | [修補排序與公告依據](README.md#patch-triage-and-advisory-evidence) |
| 盤點實際部署的套件、版本與相依性 | [軟體盤點與供應鏈](README.md#inventory-and-software-supply-chain) |
| 核對靜態分析結果並審查修補差異 | [程式碼審查與修補驗證](README.md#code-review-and-patch-validation) |
| 從原始事件回查告警、整理案件交接 | [SOC 與案件證據](README.md#soc-and-case-evidence) |
| 限定 Agent 可讀取的資料和可執行的動作 | [Agent 執行框架與權限控制](README.md#agent-harnesses-and-controls) |
| 驗證 Agent 有沒有引錯資料，或把未知寫成定論 | [評估與人工複核](README.md#evaluation-and-analyst-review) |

收到漏洞公告時，可以先用 [KEV Patch](https://kevpatch.com/zh-hant/) 查 CVE 或產品，再拿自己的版本清單對照廠商公告。若盤點資料過期或無法確認產品分支，就標成「待核對」並註明缺哪一項。[工作手冊的範例](PLAYBOOK.md#worked-patch-triage-example) 示範怎麼把這個判斷交給下一班。

## 編輯原則

- 用官方公告、專案文件或規格支持條目中的敘述。遇到未經自己驗證的產品能力，以文件載明的範圍為準。
- 出現「待核對」時，寫清楚缺哪份清單、哪個版本欄位或哪一筆事件。引用時保留資料來源與日期。
- 日常分流以獲授權範圍內的唯讀查詢為主。正式環境變更與案件結案須由負責人核准。
- 如果條目有錯，請附上條目名稱、需要修正的敘述與第一手來源。[貢獻指南](CONTRIBUTING.md)

目錄連結與文件查核於 2026 年 11 月 24 日。各工具的實際行為與部署權限，須於採用前依自己的環境驗證。

## 致謝

目錄形式受到 Ed Marcavage 的 `awesome-security-agent-harnesses` 啟發。逐欄保留來源、標示未知與唯讀版本盤點的做法來自「先修清單」。[KEV Patch 資料方法](https://kevpatch.com/zh-hant/methodology/)
