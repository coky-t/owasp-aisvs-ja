# C12 監視、ログ記録、異常検出 (Monitoring, Logging & Anomaly Detection)

## 管理目標

この章は、モデルやその他の AI コンポーネントが認識、実行、返却する内容についてリアルタイムかつフォレンジックな可視性を取り扱い、AI 特有の脅威を検出されトリアージできます。

---

## C12.1 リクエストとレスポンスのログ記録 (Request & Response Logging)

AI requests and responses must be logged to create an audit trail and support incident response.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.1.1** | **Verify that** AI interactions are logged with session context and AI-specific telemetry. | 1 |
| **12.1.2** | **Verify that** safety filtering and policy decisions are logged with sufficient detail to support audit, debugging, and forensic analysis of content moderation systems. | 2 |
| **12.1.3** | **Verify that** log entries for AI inference events follow a structured, interoperable schema that includes at least the model identifier, token usage (input and output), provider name, and operation type. | 2 |
| **12.1.4** | **Verify that** RAG pipeline retrieval events are logged, including the query, documents retrieved, and knowledge source. | 2 |

---

## C12.2 検出と警告 (Detection and Alerting)

AI-specific attack patterns (jailbreak, prompt injection, model extraction, multi-turn trajectory attacks, covert channels over LLM endpoints) must be detected, and security events enriched with AI-specific context so downstream detection and response systems can act on them.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.2.1** | **検証:** システムは、既知のジェイルブレイクパターン、プロンプトインジェクションの試み、敵対的入力を検出し、警告している。 | 1 |
| **12.2.2** | **検証:** 行動異常検出は、異常な会話パターン、過度の再試行、調査行動を識別している。 | 2 |
| **12.2.3** | **検証:** カスタムルールは、協調的なジェイルブレイクの試み、プロンプトインジェクション、システムプロンプト抽出試行の AI 固有の脅威パターンを検出している。 | 2 |
| **12.2.4** | **Verify that** extraction-alert events include offending query metadata to support investigation. | 2 |
| **12.2.5** | **Verify that** token usage is tracked at granular attribution levels including per user, per session, per feature endpoint, and per team or workspace. | 2 |
| **12.2.6** | **Verify that** LLM API traffic is monitored for covert-channel indicators and communication signatures to identify malware and command-and-control (C2) activity. | 3 |

---

## C12.3 モデル、データ、パフォーマンスのドリフトの検出 (Model, Data, and Performance Drift Detection)

Drift and degradation across model outputs, input distributions, and data schemas must be monitored to identify quality regressions and security-relevant behavioral shifts.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.3.1** | **Verify that** data drift detection monitors input distribution changes that may impact model performance, using statistically validated methods matched to the input data type (e.g., KS test or PSI for tabular numeric features, embedding-distance metrics for text or image). | 1 |
| **12.3.2** | **Verify that** hallucination detection monitors identify and flag model outputs that contain factually incorrect, inconsistent, or fabricated information. | 2 |
| **12.3.3** | **Verify that** hallucination rates are tracked as continuous time-series metrics to enable trend analysis and detection of sustained model degradation. | 2 |
| **12.3.4** | **Verify that** unexplained behavioral shifts are identified from gradual expected operational drift. | 3 |

---

## C12.4 プロアクティブなセキュリティ動作監視 (Proactive Security Behavior Monitoring)

Security threats arising from proactive (agent-initiated) behavior must be detected and prevented, including pre-execution validation, behavior pattern analysis, and audit trails for approval of security-critical actions.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.4.1** | **Verify that** autonomous action triggers include proactive behavior-pattern analysis, security evaluation, and threat-landscape assessment. | 2 |
| **12.4.2** | **Verify that** audit logs capture security-critical proactive actions, including approver identity, timestamp, action parameters, and decision outcomes. | 2 |
| **12.4.3** | **Verify that** kill-switch activations and override commands are logged. | 2 |

---

## C12.5 Training Data & Model Lifecycle Audit

The provenance and change history of training data, model artifacts, and knowledge sources must be auditable throughout the AI development lifecycle.

| # | Description | Level |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.5.1** | **Verify that** dataset lineage records each dataset and its components, including all transformations, augmentations, and merges. | 1 |
| **12.5.2** | **Verify that** all labeling activities are recorded in logs. | 1 |
| **12.5.3** | **Verify that** all model changes generate immutable audit records. | 2 |
| **12.5.4** | **Verify that** every ingested document is tagged at write time with source, writer identity, and timestamp. | 2 |

---

## 参考情報

* [OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
* [MITRE ATLAS - Adversarial Threat Landscape for AI Systems](https://atlas.mitre.org/)
* [NIST AI Risk Management Framework (AI RMF 1.0)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
* [OWASP Agentic AI Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
* [Microsoft Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit)
* [NIST SP 800-207 Zero Trust Architecture](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-207.pdf)
