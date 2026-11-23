# Awesome Security Agent

[English](README.md) · [繁體中文](README.zh-Hant.md) · **简体中文**

[防御工作手册（英文）](PLAYBOOK.md) · [贡献指南（英文）](CONTRIBUTING.md)

一份独立维护的资源目录，面向开发或使用智能体（Agent）开展安全防御工作的人。

值班时收到一则新的漏洞公告，你得确认它是否影响自己的系统、去哪里找修复说明，以及谁有权批准变更。处理安全运营中心（SOC）的告警，或审查智能体提出的补丁时，也会遇到类似的问题。我们收录能帮助团队回答这些问题的工具、数据来源和方法。

## 使命

让负责网站和基础设施的运维团队能够审查智能体参与的防御工作。我们整理的工具和方法，能帮助团队核对智能体的判断，并审批由此产生的变更。

## 宗旨

让下一班值班人员沿着记录接手。交接时要写清楚智能体检查了什么、哪条记录支持判断、还有什么需要核实，以及由谁决定下一步。

## 资源目录

- [安全助手](#安全助手)
- [修复优先级与公告依据](#修复优先级与公告依据)
- [软件盘点与供应链](#软件盘点与供应链)
- [代码审查与修复验证](#代码审查与修复验证)
- [SOC 与事件证据](#soc-与事件证据)
- [智能体运行框架与权限控制](#智能体运行框架与权限控制)
- [评估与人工复核](#评估与人工复核)

### 安全助手

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [Elastic AI Assistant for Security](https://www.elastic.co/docs/solutions/security/ai/ai-assistant) | 托管助手 | 在 Elastic Security 汇总告警并起草调查用的查询语句。记录研判结论前，先核对原始事件。 |
| [Gemini in Google SecOps](https://docs.cloud.google.com/chronicle/docs/soar/investigate/working-with-cases/investigate-cases-alerts-gemini) | 托管助手 | 使用 Google Security Operations 的调查助手复核事件和告警。 |
| [GitHub Copilot Autofix](https://docs.github.com/en/code-security/concepts/code-scanning/autofix-for-code-scanning) | 托管助手 | 为支持的代码扫描告警获取修复建议，合并前先审查代码差异。 |
| [Microsoft Security Copilot](https://learn.microsoft.com/en-us/copilot/security/microsoft-security-copilot) | 托管助手 | 汇总安全事件，从已启用的安全集成中收集调查所需信息。 |
| [SocTalk](https://soctalk.github.io/soctalk-docs/triage-policies) | 自托管智能体 | 对 Wazuh 告警进行初步研判，设定哪些决定需要分析师复核。接入生产环境的告警队列前，先核对关闭告警的规则。 |

### 修复优先级与公告依据

将公告与软件或资产清单比对，再查阅厂商公告，确认产品分支、受影响范围和建议的修复方式。记录结果时保留来源和日期。如果清单已过期，或无法确认产品分支，就标为“待核实”并写明缺少哪项信息。[修复研判示例（英文）](PLAYBOOK.md#worked-patch-triage-example)

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [CISA 已知被利用漏洞目录](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) | 目录 | 确认环境中存在该产品后，可将已有的漏洞利用记录纳入修复优先级判断。 |
| [CVE List V5](https://github.com/CVEProject/cvelistV5) | 数据集 | 读取 JSON 格式的官方 CVE 记录，查看编号分配机构和参考来源。 |
| [FIRST EPSS](https://www.first.org/epss/) | 风险指标 | 将未来 30 天内观察到漏洞被利用的估计概率，作为确定修复优先级的依据之一。 |
| [GitHub Advisory Database](https://github.com/advisories) | 目录 | 按软件包和生态系统查询公告，查看其中列出的受影响版本和已修复版本。 |
| [KEV Patch（先修清单）](https://kevpatch.com/zh-hans/) | 目录 | 按 CVE 编号或产品名称查询已有利用记录的漏洞，核对版本依据并查阅引用的厂商建议。 |
| [NIST 国家漏洞数据库](https://nvd.nist.gov/developers/vulnerabilities) | API | 获取 CVE 记录的 CVSS 信息和产品适用条件。 |
| [OSV](https://osv.dev/) | 目录与 API | 按软件包名称、版本和生态系统匹配开源软件漏洞公告。 |
| [TWCERT/CC 漏洞公告 RSS](https://www.twcert.org.tw/tw/rss-132-1.xml) | 地区公告订阅 | 跟踪台湾漏洞公告，按完整的 CVE 编号交叉核对。 |

### 软件盘点与供应链

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [CycloneDX](https://cyclonedx.org/) | 标准 | 用软件物料清单（SBOM）描述组件及其依赖关系。 |
| [OWASP Dependency-Track](https://dependencytrack.org/) | 自托管平台 | 导入 CycloneDX 清单，跨项目跟踪组件风险。 |
| [OpenSSF Scorecard](https://scorecard.dev/) | 工具 | 收集上游项目的安全实践信息，例如是否启用分支保护和发布签名。 |
| [Renovate](https://docs.renovatebot.com/) | 自动化工具 | 为依赖更新发起 Pull Request，交由维护者审查和测试。 |
| [Sigstore Cosign 签名验证](https://docs.sigstore.dev/cosign/verifying/verify/) | 工具指南 | 根据预期的签名者身份或密钥验证制品签名。 |
| [SPDX](https://spdx.dev/use/specifications/) | 标准 | 以标准格式交换软件物料清单和许可证信息。 |
| [Syft](https://github.com/anchore/syft) | 工具 | 从容器镜像或本地文件系统生成软件清单。 |

### 代码审查与修复验证

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [Bandit](https://bandit.readthedocs.io/en/latest/) | 工具 | 通过静态分析检查 Python 源代码中常见的安全问题。 |
| [GitHub 代码扫描告警](https://docs.github.com/en/code-security/concepts/code-scanning/code-scanning-alerts) | 平台指南 | 结合触发规则、代码位置和修复信息复核扫描结果。 |
| [Gitleaks](https://github.com/gitleaks/gitleaks) | 工具 | 检查本地文件或仓库历史中是否误提交了凭据等敏感信息。智能体输出应对原值脱敏。 |
| [GitHub 的 SARIF 支持](https://docs.github.com/en/code-security/reference/code-scanning/sarif-files/sarif-support) | 格式指南 | 在代码审查中查看静态分析结果及其规则 ID 和源代码位置。 |
| [Semgrep Community Edition](https://github.com/semgrep/semgrep) | 工具 | 用模式匹配对自己维护的仓库进行静态代码检查。 |

### SOC 与事件证据

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [OCSF](https://schema.ocsf.io/) | 数据模式 | 将不同安全工具产生的事件整理为统一的类别和字段。 |
| [OpenSearch Security Analytics](https://docs.opensearch.org/latest/security-analytics/) | 平台 | 从日志生成检测结果，供分析师复核。 |
| [OpenTelemetry 日志数据模型](https://opentelemetry.io/docs/specs/otel/logs/data-model/) | 标准 | 统一日志中的时间戳、严重程度和链路关联字段。 |
| [Sigma](https://github.com/SigmaHQ/sigma-specification) | 规范 | 用可跨平台转换的格式描述日志检测规则。应用转换后的规则前，先核对字段映射。 |
| [TheHive](https://docs.strangebee.com/thehive/user-guides/analyst-corner/cases/about-cases/) | 事件管理平台 | 记录事件处置任务、可观测项、负责人和分析师的决定。 |
| [Wazuh](https://documentation.wazuh.com/current/user-manual/ruleset/index.html) | 平台 | 解析事件并生成告警，供分析师或只读助手复核。 |

### 智能体运行框架与权限控制

运行框架（harness）管理模型之外的运行流程。它决定智能体可以调用哪些工具、读取哪些数据、何时暂停以等待审批，以及运行过程需要留下哪些记录。

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [gVisor](https://gvisor.dev/docs/) | 运行环境 | 加强容器内工作进程与宿主机之间的隔离。 |
| [JSON Schema](https://json-schema.org/understanding-json-schema/reference) | 规范 | 要求结构化的检查结果包含证据引用和明确的状态。 |
| [LangGraph 运行暂停机制](https://docs.langchain.com/oss/python/langgraph/interrupts) | 框架指南 | 暂停智能体运行，在复核人员作出决定后恢复。 |
| [MCP 安全实践](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices) | 协议指南 | 通过 MCP 接入工具时，核对授权与凭据处理方式。 |
| [OpenAI Agents SDK 审批机制](https://openai.github.io/openai-agents-python/human_in_the_loop/) | 框架指南 | 要求指定的工具调用获得人工批准后才执行。 |
| [Open Policy Agent](https://www.openpolicyagent.org/docs) | 策略引擎 | 在模型之外根据策略判断是否允许工具调用或操作请求。 |
| [OpenTelemetry GenAI 语义约定](https://github.com/open-telemetry/semantic-conventions-genai) | 规范 | 用统一字段追踪智能体与模型的操作。启用导出前，先核对会采集哪些数据。 |

### 评估与人工复核

使用合成案例或经批准的历史记录进行测试。检查引用的记录是否支持结论、交接记录是否保留待核实的信息，以及智能体是否等到批准后才行动。

| 资源 | 类型 | 用途 |
| --- | --- | --- |
| [DeepEval](https://deepeval.com/docs/metrics-introduction) | 评估框架 | 使用选定案例检查输出是否有依据，并评估其他输出特性。 |
| [Elastic 的 SOC 告警研判评估](https://www.elastic.co/security-labs/blog/alert-triage-agentic-soc-self-correcting-agents) | 案例研究 | 了解团队如何用自己的数据对比自动研判和分析师的决定。 |
| [Inspect AI](https://inspect.aisi.org.uk/) | 评估框架 | 使用数据集、评分器和运行日志开展可重复的评估。 |
| [MITRE D3FEND](https://d3fend.mitre.org/) | 知识库 | 用统一的术语审查智能体建议的防御措施。 |
| [NIST AI 风险管理框架](https://www.nist.gov/itl/ai-risk-management-framework) | 治理指南 | 在系统生命周期的各个阶段分配评估、监测和风险决策职责。 |

## 编辑原则

- 优先链接到维护者、规范发布方或官方公告，并对照来源核实条目中的说法。
- 说明每项资源的用途。对于智能体和工具，核对它们需要哪些访问权限，以及复核人员可以去哪里验证输出。证据不足时标为“待核实”，并说明缺少什么。
- 日常研判限于已授权范围内的只读操作。生产环境变更与事件关闭须由负责人明确批准。
- 提交更正时，请附上条目名称、需要修正的说法和一手来源。[贡献指南（英文）](CONTRIBUTING.md)

目录链接和文档核查于 2026 年 11 月 24 日。采用前应在自己的环境中验证工具行为和部署权限。

## 致谢

目录形式受到 Ed Marcavage 的 `awesome-security-agent-harnesses` 启发。先修清单通过逐字段引用来源、标记待核实信息和只读版本盘点来支持修复决策，本目录沿用了这些做法。[KEV Patch 数据方法](https://kevpatch.com/zh-hans/methodology/)
