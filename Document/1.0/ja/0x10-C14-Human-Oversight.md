# C14 人間による監視と信頼 (Human Oversight and Trust)

## 管理目標

Ensure that humans retain effective control over AI systems through reliable shutdown and graceful-degradation paths, an explicit policy that determines which AI decisions and agent actions require human approval, and an independent audit trail of human oversight interventions.

This chapter focuses on controls unique to human oversight of AI systems: kill-switch and intermediate-state mechanisms specific to AI runtime structure (model inference, agent runtimes, tool/MCP servers, retrieval connectors), the policy that classifies AI decisions and agent actions as high-risk, the system's behavior when a human approver is not available within the required timeframe, and logging of human-initiated override events.

---

## C14.1 キルスイッチとオーバーライドメカニズム (Kill-Switch & Override Mechanisms)

AI システムの安全でない動作が観察された場合、シャットダウンまたはロールバックパスを提供します。 and ensure these mechanisms remain functional over time.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **14.1.1** | **検証:** 手動キルスイッチメカニズムは、AI モデルの推論と出力を即座に停止するために、存在している。 | 1 |
| **14.1.2** | **Verify that** kill-switch and intermediate-state mechanisms are exercised at a defined frequency, and that each test confirms the system reaches the target state within the documented response time and that all dependent components (e.g., agent runtimes, tool/MCP servers, retrieval connectors) transition as specified. | 2 |
| **14.1.3** | **Verify that** the system can be placed into at least two intermediate operational states between full operation and complete shutdown (e.g., disabling specific tools or MCP servers, removing a retrieval source, switching to a safer or smaller model, enforcing read-only mode for agents), and that each state has defined entry triggers and can be exited independently without requiring a full system restart or shutdown. | 2 |
| **14.1.4** | **Verify that** override and kill-switch commands for autonomous agents are delivered through an out-of-band channel (e.g., infrastructure controls, hypervisor-level signals, network-layer isolation) that is architecturally isolated from the agent runtime, ensuring commands remain enforceable even if the agent runtime is compromised or manipulated. | 2 |

---

## C14.2 ヒューマンインザループの意思決定チェックポイント (Human-in-the-Loop Decision Checkpoints)

Define which AI decisions and agent actions require human approval so that runtime gates can enforce them, and define the system's behavior when approval is not provided in time.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **14.2.1** | **Verify that** a documented human oversight policy defines which AI decisions and agent actions are classified as high-risk, the criteria used to make that determination, the approval authority required before execution, and whether any deviation from fail-closed default behavior on approval TTL expiry is permitted and under what conditions. | 1 |
| **14.2.2** | **Verify that** when a human-approval gate is not satisfied within the defined approval time-to-live, the system applies a documented default action that is fail-closed (blocking the pending action). | 2 |

---

## C14.3 Logging of Human Oversight Interventions

Capture human-initiated oversight events so that override and mode-change actions are independently auditable and reconstructable.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **14.3.1** | **Verify that** kill-switch activations, intermediate operational state transitions, and override commands are logged with the operator identity, the channel used, the originating trigger or justification, the prior and resulting system state, and the timestamp. | 1 |

## 参考情報

* [MITRE ATLAS: Human In-the-Loop for AI Agent Actions](https://atlas.mitre.org/mitigations/AML.M0029)
* [NIST AI 100-1: AI Risk Management Framework (AI RMF 1.0)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
* [NIST AI 600-1: Generative AI Profile (AI RMF Companion)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)
* [ISO/IEC 42001:2023 Artificial Intelligence Management System](https://www.iso.org/standard/42001)
* [ISO/IEC 23894:2023 Artificial Intelligence Risk Management Guidance](https://www.iso.org/standard/77304.html)
* [Regulation (EU) 2024/1689 (EU AI Act), Article 14: Human Oversight](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)
* [OECD Recommendation on Artificial Intelligence](https://legalinstruments.oecd.org/en/instruments/OECD-LEGAL-0449)
* [OWASP Top 10 for LLM Applications 2025: LLM06 Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)
* [OWASP AI Exchange: Human Oversight Controls](https://owaspai.org/)
