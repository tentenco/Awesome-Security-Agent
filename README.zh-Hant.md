# Awesome Security Agent

[English](README.md) · **繁體中文** · [简体中文](README.zh-Hans.md)

[防禦工作手冊（英文）](PLAYBOOK.md) · [貢獻指南（英文）](CONTRIBUTING.md)

一份獨立整理的資源目錄，給用 Agent 處理防禦性資安工作的人。

值班時收到一則新公告，你得先確認自己的環境有沒有用到受影響的版本、廠商建議怎麼修，以及誰能核准變更。處理 SOC 告警或審查 Agent 建議的修補時，也會遇到類似的問題。我們收錄能幫團隊回答這些問題的工具、資料來源和實務做法。

## 使命

我們的使命是讓網站與基礎設施維運團隊也能查核 Agent 的防禦工作。我們整理的工具和方法，讓團隊能回到資料核對判斷，並自行核准後續變更。

## 宗旨（Purpose）

讓下一班接手的人能沿著紀錄繼續查。交付內容要交代 Agent 查了什麼、哪一筆資料支持判斷、還缺哪些證據，以及下一步由誰決定。

## 資源目錄

- [資安助理](#資安助理)
- [修補排序與公告依據](#修補排序與公告依據)
- [軟體盤點與供應鏈](#軟體盤點與供應鏈)
- [程式碼審查與修補驗證](#程式碼審查與修補驗證)
- [SOC 與案件證據](#soc-與案件證據)
- [Agent 執行框架與權限控制](#agent-執行框架與權限控制)
- [評估與人工複核](#評估與人工複核)

### 資安助理

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [Elastic AI Assistant for Security](https://www.elastic.co/docs/solutions/security/ai/ai-assistant) | 託管助理 | 在 Elastic Security 整理告警摘要並草擬調查用的查詢語句。記錄判定前，先核對原始事件。 |
| [Gemini in Google SecOps](https://docs.cloud.google.com/chronicle/docs/soar/investigate/working-with-cases/investigate-cases-alerts-gemini) | 託管助理 | 用 Google Security Operations 的調查助理複核案件和告警。 |
| [GitHub Copilot Autofix](https://docs.github.com/en/code-security/concepts/code-scanning/autofix-for-code-scanning) | 託管助理 | 針對支援的程式碼掃描告警取得修補建議，合併前先審查差異。 |
| [Microsoft Security Copilot](https://learn.microsoft.com/en-us/copilot/security/microsoft-security-copilot) | 託管助理 | 摘要資安事件，從已啟用的資安整合收集調查所需資料。 |
| [SocTalk](https://soctalk.github.io/soctalk-docs/triage-policies) | 自行架設的 Agent | 分流 Wazuh 告警，設定哪些判定需要分析師複核。接上正式告警佇列前，先核對結案規則。 |

### 修補排序與公告依據

用軟體或資產清單對照公告，再回到廠商公告核對產品分支、受影響範圍與建議修法。記錄結果時保留來源與日期。若盤點資料過期或無法確認產品分支，就標成「待核對」並註明缺哪一項。[修補分流範例（英文）](PLAYBOOK.md#worked-patch-triage-example)

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [CISA 已知遭利用漏洞目錄](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) | 目錄 | 確認環境有該產品後，可把已知遭利用的紀錄納入修補排序。 |
| [CVE List V5](https://github.com/CVEProject/cvelistV5) | 資料集 | 讀取 JSON 格式的官方 CVE 紀錄，查找核發機構與參考來源。 |
| [FIRST EPSS](https://www.first.org/epss/) | 風險指標 | 將未來 30 天觀察到漏洞遭利用的估計機率，作為排序依據之一。 |
| [GitHub Advisory Database](https://github.com/advisories) | 目錄 | 按套件名稱與生態系統查詢公告，並查看有資料的受影響版本與修補版本。 |
| [KEV Patch（先修清單）](https://kevpatch.com/zh-hant/) | 目錄 | 用 CVE 或產品名稱查詢已知遭利用的漏洞，核對版本依據後查閱所引用的廠商建議。 |
| [NIST 國家漏洞資料庫](https://nvd.nist.gov/developers/vulnerabilities) | API | 取得 CVE 紀錄的 CVSS 資訊與產品適用條件。 |
| [OSV](https://osv.dev/) | 目錄與 API | 按套件名稱、版本和生態系統比對開源軟體漏洞公告。 |
| [TWCERT/CC 漏洞公告 RSS](https://www.twcert.org.tw/tw/rss-132-1.xml) | 地區公告訂閱 | 追蹤臺灣漏洞公告，以完整 CVE 編號交叉核對。 |

### 軟體盤點與供應鏈

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [CycloneDX](https://cyclonedx.org/) | 標準 | 用軟體物料清單（SBOM）描述元件與相依關係。 |
| [OWASP Dependency-Track](https://dependencytrack.org/) | 自行架設的平台 | 匯入 CycloneDX 清單，跨專案追蹤元件風險。 |
| [OpenSSF Scorecard](https://scorecard.dev/) | 工具 | 查核上游專案有沒有啟用分支保護、替發行版本簽章等做法。 |
| [Renovate](https://docs.renovatebot.com/) | 自動化工具 | 為相依套件更新開啟 Pull Request，由維護者審查與測試。 |
| [Sigstore Cosign 簽章驗證](https://docs.sigstore.dev/cosign/verifying/verify/) | 工具指南 | 以預期的簽署者身分或金鑰驗證產物簽章。 |
| [SPDX](https://spdx.dev/use/specifications/) | 標準 | 以標準格式交換軟體物料清單與授權資訊。 |
| [Syft](https://github.com/anchore/syft) | 工具 | 從容器映像檔或本機檔案系統建立軟體清單。 |

### 程式碼審查與修補驗證

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [Bandit](https://bandit.readthedocs.io/en/latest/) | 工具 | 用靜態分析檢查 Python 原始碼中的常見資安問題。 |
| [GitHub 程式碼掃描告警](https://docs.github.com/en/code-security/concepts/code-scanning/code-scanning-alerts) | 平台指南 | 搭配觸發規則、程式碼位置和修補資訊複核發現的問題。 |
| [Gitleaks](https://github.com/gitleaks/gitleaks) | 工具 | 檢查本機檔案或儲存庫歷史是否誤提交憑證等機密資料。Agent 輸出須遮蔽原值。 |
| [GitHub 的 SARIF 支援](https://docs.github.com/en/code-security/reference/code-scanning/sarif-files/sarif-support) | 格式指南 | 把靜態分析結果連同規則 ID 與原始碼位置帶入程式碼審查。 |
| [Semgrep Community Edition](https://github.com/semgrep/semgrep) | 工具 | 以模式比對靜態檢查自己維護的儲存庫原始碼。 |

### SOC 與案件證據

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [OCSF](https://schema.ocsf.io/) | 結構定義 | 統一不同資安工具產生的事件分類與欄位。 |
| [OpenSearch Security Analytics](https://docs.opensearch.org/latest/security-analytics/) | 平台 | 從日誌產生偵測結果，提供分析師複核的介面。 |
| [OpenTelemetry 日誌資料模型](https://opentelemetry.io/docs/specs/otel/logs/data-model/) | 標準 | 統一日誌中的時間戳記、嚴重程度與追蹤關聯欄位。 |
| [Sigma](https://github.com/SigmaHQ/sigma-specification) | 規格 | 用可跨平台轉換的格式描述日誌偵測規則。套用轉換後的規則前，先核對欄位對應。 |
| [TheHive](https://docs.strangebee.com/thehive/user-guides/analyst-corner/cases/about-cases/) | 案件管理平台 | 記錄事件處理任務、觀測項目、負責人與分析師的判定。 |
| [Wazuh](https://documentation.wazuh.com/current/user-manual/ruleset/index.html) | 平台 | 解析事件並產生告警，供分析師或唯讀助理複核。 |

### Agent 執行框架與權限控制

執行框架（harness）管理模型以外的執行流程。它決定 Agent 可以呼叫哪些工具、讀取哪些資料、何時暫停以等待核准，以及執行過程要留下哪些紀錄。

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [gVisor](https://gvisor.dev/docs/) | 執行環境 | 加強容器內工作程序與宿主機之間的隔離。 |
| [JSON Schema](https://json-schema.org/understanding-json-schema/reference) | 規格 | 要求結構化的查核結果包含證據引用和明確狀態。 |
| [LangGraph 執行暫停機制](https://docs.langchain.com/oss/python/langgraph/interrupts) | 框架指南 | 暫停 Agent 執行，等複核人員作出決定後再繼續。 |
| [MCP 安全實務](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices) | 協定指南 | 透過 MCP 串接工具時，核對授權與憑證處理方式。 |
| [OpenAI Agents SDK 核准機制](https://openai.github.io/openai-agents-python/human_in_the_loop/) | 框架指南 | 要求特定工具呼叫取得人工核准後才執行。 |
| [Open Policy Agent](https://www.openpolicyagent.org/docs) | 政策引擎 | 在模型之外，依政策判斷工具呼叫或動作請求是否可執行。 |
| [OpenTelemetry GenAI 語意慣例](https://github.com/open-telemetry/semantic-conventions-genai) | 規格 | 用共同欄位追蹤 Agent 與模型操作。啟用匯出前，先確認會收集哪些資料。 |

### 評估與人工複核

用合成案例或經核准的歷史紀錄測試。確認引用的紀錄是否支持結論、交接時是否保留不確定性，以及 Agent 有沒有等到核准再行動。

| 資源 | 類型 | 用途 |
| --- | --- | --- |
| [DeepEval](https://deepeval.com/docs/metrics-introduction) | 評估框架 | 用選定的案例檢查輸出是否有來源依據，並評估其他輸出特性。 |
| [Elastic 的 SOC 告警分流評估](https://www.elastic.co/security-labs/blog/alert-triage-agentic-soc-self-correcting-agents) | 案例研究 | 參考團隊如何用自家的資料，比較自動判定與分析師的決定。 |
| [Inspect AI](https://inspect.aisi.org.uk/) | 評估框架 | 用資料集、評分方式和執行紀錄做可重複的評估。 |
| [MITRE D3FEND](https://d3fend.mitre.org/) | 知識庫 | 用一致的術語審查 Agent 建議的防禦措施。 |
| [NIST AI 風險管理框架](https://www.nist.gov/itl/ai-risk-management-framework) | 治理指南 | 在系統生命週期各階段分配評估、監測與風險決策的責任。 |

收到漏洞公告時，可以先用 [KEV Patch](https://kevpatch.com/zh-hant/) 查 CVE 或產品，再拿自己的版本清單對照廠商公告。若盤點資料過期或無法確認產品分支，就標成「待核對」並註明缺哪一項。[工作手冊的範例](PLAYBOOK.md#worked-patch-triage-example) 示範怎麼把這個判斷交給下一班。

## 編輯原則

- 用官方公告、專案文件或規格支持條目中的敘述。遇到未經自己驗證的產品能力，以文件載明的範圍為準。
- 出現「待核對」時，寫清楚缺哪份清單、哪個版本欄位或哪一筆事件。引用時保留資料來源與日期。
- 日常分流以獲授權範圍內的唯讀查詢為主。正式環境變更與案件結案須由負責人核准。
- 如果條目有錯，請附上條目名稱、需要修正的敘述與第一手來源。[貢獻指南（英文）](CONTRIBUTING.md)

目錄連結與文件查核於 2026 年 11 月 24 日。各工具的實際行為與部署權限，須於採用前依自己的環境驗證。

## 致謝

目錄形式受到 Ed Marcavage 的 `awesome-security-agent-harnesses` 啟發。逐欄保留來源、標示未知與唯讀版本盤點的做法來自「先修清單」。[KEV Patch 資料方法](https://kevpatch.com/zh-hant/methodology/)
