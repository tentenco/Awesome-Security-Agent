# Awesome Security Agent

[繁體中文](README.zh-Hant.md) · [Defensive playbook](PLAYBOOK.md) · [Contribute](CONTRIBUTING.md)

An independent directory for people who build or use agents for defensive security.

A new advisory lands during your shift. You need to know whether it applies to your systems, where the fix is documented, and who can approve the change. The same questions come up with a SOC alert or an agent-written patch. We collect resources that help teams answer them.

## Mission

Our mission is to make agent-assisted defense auditable for the teams that operate websites and infrastructure. We curate tools and methods that let those teams check a finding and approve any change it leads to.

## Purpose

Use this directory to put together a workflow someone else can pick up on the next shift. Its output should say what the agent checked, which record supports its finding, what still needs verification, and who owns the next decision.

## Directory

- [Security assistants](#security-assistants)
- [Patch triage and advisory evidence](#patch-triage-and-advisory-evidence)
- [Inventory and software supply chain](#inventory-and-software-supply-chain)
- [Code review and patch validation](#code-review-and-patch-validation)
- [SOC and case evidence](#soc-and-case-evidence)
- [Agent harnesses and controls](#agent-harnesses-and-controls)
- [Evaluation and analyst review](#evaluation-and-analyst-review)

### Security assistants

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [Elastic AI Assistant for Security](https://www.elastic.co/docs/solutions/security/ai/ai-assistant) | Managed assistant | Summarize alerts and draft investigation queries in Elastic Security. Check the underlying events before recording a verdict. |
| [Gemini in Google SecOps](https://docs.cloud.google.com/chronicle/docs/soar/investigate/working-with-cases/investigate-cases-alerts-gemini) | Managed assistant | Review cases and alerts in Google Security Operations using its investigation assistant. |
| [GitHub Copilot Autofix](https://docs.github.com/en/code-security/concepts/code-scanning/autofix-for-code-scanning) | Managed assistant | Get suggested fixes for supported code-scanning alerts and review the diff before merging. |
| [Microsoft Security Copilot](https://learn.microsoft.com/en-us/copilot/security/microsoft-security-copilot) | Managed assistant | Summarize incidents and assemble investigation context from enabled security integrations. |
| [SocTalk](https://soctalk.github.io/soctalk-docs/triage-policies) | Self-hosted agent | Triage Wazuh alerts and configure which decisions require analyst review. Check the closure policy before connecting a production queue. |

### Patch triage and advisory evidence

Match the advisory to your software or asset inventory. Use the vendor advisory to check the product branch, affected range, and recommended fix. Keep the source and date with the result.

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [CISA Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) | Catalog | Bring documented exploitation into patch prioritization after checking whether the product is present. |
| [CVE List V5](https://github.com/CVEProject/cvelistV5) | Dataset | Read official CVE records in JSON and identify the assigning authority and references. |
| [FIRST EPSS](https://www.first.org/epss/) | Risk signal | Use the estimated probability of observed exploitation in the next 30 days as one input to prioritization. |
| [GitHub Advisory Database](https://github.com/advisories) | Catalog | Look up advisories by package and ecosystem, including affected and patched versions when available. |
| [KEV Patch](https://kevpatch.com/) | Catalog | Look up known exploited vulnerabilities by CVE or product. Review version evidence and follow the cited vendor guidance. |
| [NIST National Vulnerability Database](https://nvd.nist.gov/developers/vulnerabilities) | API | Retrieve CVSS information and product applicability metadata for CVE records. |
| [OSV](https://osv.dev/) | Catalog and API | Match open-source advisories against package names, versions, and ecosystems. |
| [TWCERT/CC advisory feed](https://www.twcert.org.tw/tw/rss-132-1.xml) | Regional feed | Follow Taiwan advisories and cross-reference them by exact CVE identifier. |

### Inventory and software supply chain

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [CycloneDX](https://cyclonedx.org/) | Standard | Describe software components and dependency relationships in a bill of materials. |
| [OWASP Dependency-Track](https://dependencytrack.org/) | Self-hosted platform | Consume CycloneDX inventories and track component risk across projects. |
| [OpenSSF Scorecard](https://scorecard.dev/) | Tool | Collect signals about upstream practices such as branch protection and signed releases. |
| [Renovate](https://docs.renovatebot.com/) | Automation | Open dependency update pull requests for maintainers to review and test. |
| [Sigstore Cosign verification](https://docs.sigstore.dev/cosign/verifying/verify/) | Tool guide | Verify an artifact's signature against the expected signing identity or key. |
| [SPDX](https://spdx.dev/use/specifications/) | Standard | Exchange software bill of materials (SBOM) and licensing information in a standard format. |
| [Syft](https://github.com/anchore/syft) | Tool | Build a software inventory from a container image or a local filesystem. |

### Code review and patch validation

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [Bandit](https://bandit.readthedocs.io/en/latest/) | Tool | Run static checks for common security issues in Python source. |
| [GitHub code scanning alerts](https://docs.github.com/en/code-security/concepts/code-scanning/code-scanning-alerts) | Platform guide | Review findings with their rule, location, and remediation context. |
| [Gitleaks](https://github.com/gitleaks/gitleaks) | Tool | Check local files or repository history for accidentally committed secrets. Redact values from agent output. |
| [SARIF support in GitHub](https://docs.github.com/en/code-security/reference/code-scanning/sarif-files/sarif-support) | Format guide | Carry static-analysis findings with rule IDs and source locations into code review. |
| [Semgrep Community Edition](https://github.com/semgrep/semgrep) | Tool | Run pattern-based static checks on source code in a repository you maintain. |

### SOC and case evidence

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [OCSF](https://schema.ocsf.io/) | Schema | Normalize events from different security tools into common categories and fields. |
| [OpenSearch Security Analytics](https://docs.opensearch.org/latest/security-analytics/) | Platform | Generate findings from log data and give analysts a place to review them. |
| [OpenTelemetry Logs Data Model](https://opentelemetry.io/docs/specs/otel/logs/data-model/) | Standard | Keep timestamps, severity, and trace correlation fields consistent across logs. |
| [Sigma](https://github.com/SigmaHQ/sigma-specification) | Specification | Describe log detection rules in a portable format. Check field mappings before using a translated rule. |
| [TheHive](https://docs.strangebee.com/thehive/user-guides/analyst-corner/cases/about-cases/) | Case platform | Record incident tasks, observables, owners, and analyst decisions. |
| [Wazuh](https://documentation.wazuh.com/current/user-manual/ruleset/index.html) | Platform | Decode events and produce alerts that an analyst or read-only assistant can review. |

### Agent harnesses and controls

The harness is what runs around the model. It decides which tools the agent can call, what data it can see, when to stop for approval, and what to record about the run.

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [gVisor](https://gvisor.dev/docs/) | Runtime | Add isolation between a containerized worker and its host. |
| [JSON Schema](https://json-schema.org/understanding-json-schema/reference) | Specification | Require structured findings to carry an evidence reference and an explicit status. |
| [LangGraph interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts) | Framework guide | Pause an agent run and resume it after a reviewer supplies a decision. |
| [MCP security best practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices) | Protocol guide | Review authorization and credential handling when connecting tools through MCP. |
| [OpenAI Agents SDK approvals](https://openai.github.io/openai-agents-python/human_in_the_loop/) | Framework guide | Require a person to approve selected tool calls before they run. |
| [Open Policy Agent](https://www.openpolicyagent.org/docs) | Policy engine | Evaluate tool or action requests against policy outside the model. |
| [OpenTelemetry GenAI conventions](https://github.com/open-telemetry/semantic-conventions-genai) | Specification | Use shared fields to trace agent and model operations. Review what the exporter captures before enabling it. |

### Evaluation and analyst review

Test with synthetic cases or approved historical records. Check whether the cited record supports the conclusion, whether uncertainty survives the handoff, and whether the agent waited for authorization.

| Resource | Kind | Where it fits |
| --- | --- | --- |
| [DeepEval](https://deepeval.com/docs/metrics-introduction) | Evaluation framework | Measure grounding and other output properties against curated cases. |
| [Elastic's SOC triage evaluation](https://www.elastic.co/security-labs/blog/alert-triage-agentic-soc-self-correcting-agents) | Case study | Read how a team compared automated dispositions with analyst decisions on its own data. |
| [Inspect AI](https://inspect.aisi.org.uk/) | Evaluation framework | Run repeatable evaluation tasks with datasets, scorers, and logs. |
| [MITRE D3FEND](https://d3fend.mitre.org/) | Knowledge base | Use a common vocabulary when reviewing the defensive measures an agent recommends. |
| [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) | Governance guide | Assign responsibility for evaluation, monitoring, and risk decisions across the system lifecycle. |

## Editorial standards

- Link to the maintainer, specification publisher, or official advisory. Check claims against the linked material.
- Identify what each resource does. For agents and tools, check what they need access to and where a reviewer can verify their output. Mark an uncertain finding as **needs verification** and name the missing evidence.
- Keep routine triage read-only and within the team's authorized scope. Put production changes and case closures behind an explicit approval step.
- Send a correction with the entry, the disputed claim, and a primary source. The [contribution guide](CONTRIBUTING.md) describes how we review listings and updates.

Catalog links and documentation were checked on 24 November 2026. Product behavior and deployment controls need their own evaluation.

## Credits

Ed Marcavage's `awesome-security-agent-harnesses` inspired the directory. The editorial method comes from KEV Patch's use of field-level sources, explicit uncertainty, and read-only inventory for patch decisions. [KEV Patch methodology](https://kevpatch.com/methodology/)
