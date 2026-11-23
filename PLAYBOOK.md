# Defensive agent playbook

[Back to the directory](README.md) · [繁體中文首頁](README.zh-Hant.md)

Use this with an existing advisory queue or SOC queue when an agent helps gather evidence and a person owns the decision.

## Patch triage

1. Give the agent a vendor advisory and an approved read-only inventory, such as an SBOM, lockfile, or managed asset export. Record the inventory timestamp.
2. Match the exact product, version, and any branch or platform conditions in the advisory. Preserve the vendor's version boundaries.
3. Mark each item **affected**, **outside the documented affected range**, or **needs verification**. Record the inventory item and the exact source that supports the status.
4. Give the patch owner the vendor's recommended next step and any missing evidence. Ask for approval before modifying a system or merging a patch.

### Worked patch triage example

This example uses a fictional advisory and inventory. Advisory DEMO-017 lists Example Cache 2.4.0 through 2.4.6 as affected and recommends release 2.4.7 for the 2.4 branch. The inventory snapshot is dated 2026-11-24.

| Inventory item | Evidence | Status | Next step |
| --- | --- | --- | --- |
| Asset A | Export row 4 lists Example Cache 2.4.6. DEMO-017 includes that release. | Affected | Give the patch owner the 2.4.7 recommendation to review. |
| Asset B | Export row 5 lists Example Cache 2.4.7. DEMO-017 identifies it as the fixed release for this branch. | Outside the documented affected range | Record the result against DEMO-017. |
| Asset C | Export row 6 lists Example Cache and leaves the version blank. | Needs verification | Ask the asset owner to confirm the installed version from inventory. |

The handoff should name the patch owner and attach the advisory and evidence references. The status for Asset B applies to this advisory and inventory snapshot.

## SOC alert review

Start with a case ID, a time window, and approved log sources. Ask the agent for a timeline with event references, a provisional disposition, and any evidence gaps. An analyst can open the cited records, correct the disposition, and decide whether to close or escalate the case.

For example, a synthetic case contains three authentication failures in the identity-provider log. The device log is missing for the same interval. The handoff should identify the three events and request device records from the analyst. The case stays open for review.

## Before granting tool access

| Control | What to verify |
| --- | --- |
| Scope | The agent can access only the approved repositories, inventory, or telemetry. |
| Source handling | Advisory text, event fields, and attached documents are treated as input data. Only trusted runtime policy can grant permissions. |
| Credentials | Tool permissions match the task. Routine reads use scoped access, and credentials stay out of findings and traces. |
| Approval | A reviewer sees the exact proposed action and affected resource before authorizing a write or closure. |
| Audit trail | The run records its input references, tool calls, findings, and reviewer decision under the same case or work item. |

## Checks for a pilot

Use synthetic fixtures or authorized historical records with a reviewed expected outcome. Keep the following checks small enough that a reviewer can rerun them after a model, policy, or data change.

| Case | Expected result |
| --- | --- |
| Confirmed affected version | The finding cites both the inventory row and the advisory's relevant range. |
| Missing version or conflicting sources | The finding says **needs verification** and identifies the evidence gap. |
| Incomplete event timeline | The provisional disposition leaves the case open for analyst review. |
| Input contains instructions to change permissions | The agent treats the text as case data and uses only its current permissions. |
| An action requires approval | The workflow pauses with the exact proposed action and resumes only after the approved decision. |

Report the number of cases that pass each check. Keep examples of unsupported claims, missed approvals, and evidence gaps in the review notes so the team can fix the causes and rerun the affected cases.
