# C12 監視、ログ記録、異常検出 (Monitoring, Logging & Anomaly Detection)

## 管理目標

モデルやその他の AI コンポーネントが認識、実行、返却する内容についてリアルタイムかつフォレンジックな可視性を実現することで、AI 特有の脅威を検出、トリアージ、学習できます。

This chapter focuses on controls unique to AI systems for monitoring, logging, and anomaly detection: AI-specific log content (model identifier, token usage, safety filter outcomes, prompt/response handling), AI-specific abuse and attack detection (jailbreak, prompt injection, extraction, multi-turn trajectory, covert channels over LLM endpoints), model and data drift detection, AI-specific telemetry signals (token attribution, output/input ratio anomalies), AI incident response, proactive agent behavior monitoring, and training data and model lifecycle audit logging.

---

## C12.1 リクエストとレスポンスのログ記録 (Request & Response Logging)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.1.1** | **Verify that** AI interactions are logged with basic session and model context: timestamp, user ID, session ID, and model version. | 1 |
| **12.1.2** | **Verify that** AI interaction log entries include AI-specific telemetry: token count, input hash, system prompt version, confidence score where the model or provider exposes one, and safety filter outcome. | 1 |
| **12.1.3** | **Verify that** AI interaction logs exclude prompt and response content by default, and that content logging is enabled only on explicit opt-in with documented justification. | 1 |
| **12.1.4** | **検証:** ポリシー決定と安全フィルタリングアクションは、コンテンツモデレーションシステムを監査してデバッグするために、十分な詳細でログ記録されている。 | 2 |
| **12.1.5** | **Verify that** log entries for AI inference events follow a structured, interoperable schema that includes at least the model identifier, token usage (input and output), provider name, and operation type, so AI observability stays consistent across tools and platforms. | 2 |
| **12.1.6** | **Verify that** full prompt and response content is logged only when a security-relevant event is detected (e.g., safety filter trigger, prompt injection detection, anomaly flag), or when required by explicit user consent and a documented legal basis. | 2 |
| **12.1.7** | **Verify that** screening logs include classifier confidence scores and policy category tags with applied stage and trace metadata. | 2 |
| **12.1.8** | **Verify that** logs record the exact hosted model identifier returned by the provider. | 2 |
| **12.1.9** | **Verify that** RAG pipeline retrieval events are logged, including the query, documents retrieved, and knowledge source. | 2 |

---

## C12.2 不正使用の検出と警告 (Abuse Detection and Alerting)

Detect AI-specific attack patterns (jailbreak, prompt injection, model extraction, multi-turn trajectory attacks, covert channels over LLM endpoints) and enrich security events with AI-specific context so that downstream detection and response systems can act on them.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.2.1** | **検証:** システムは、シグネチャベースの検出を使用して、既知のジェイルブレイクパターン、プロンプトインジェクションの試み、敵対的入力を検出し、警告している。 | 1 |
| **12.2.2** | **検証:** 強化されたセキュリティイベントは、モデル識別子、信頼スコア、安全フィルタの決定などの AI 固有のコンテキストを含んでいる。 | 2 |
| **12.2.3** | **検証:** 行動異常検出は、異常な会話パターン、過度の再試行、体系的な調査行動を識別している。 | 2 |
| **12.2.4** | **検証:** カスタムルールは、協調的なジェイルブレイクの試み、プロンプトインジェクションキャンペーン、システムプロンプト抽出試行、モデル抽出攻撃などの AI 固有の脅威パターンを検出している。 | 2 |
| **12.2.5** | **Verify that** per-user and per-session token consumption triggers an alert when consumption exceeds defined thresholds. | 2 |
| **12.2.6** | **検証:** 自動インシデント対応ワークフローは侵害されたモデルを隔離し、悪意のあるユーザーをブロックしている。 | 2 |
| **12.2.7** | **Verify that** extraction-alert events include offending query metadata (e.g., source principal, query volume, input distribution statistics) to support investigation. | 2 |
| **12.2.8** | **Verify that** session-level conversation trajectory analysis detects multi-turn jailbreak patterns where no single turn looks overtly malicious on its own, but the conversation as a whole shows attack indicators. | 3 |
| **12.2.9** | **Verify that** LLM API traffic is monitored for covert channel indicators, including Base64-encoded payloads, structured non-human query patterns, and communication signatures consistent with malware command-and-control activity using LLM endpoints. | 3 |

---

## C12.3 モデル、データ、パフォーマンスのドリフトの検出 (Model, Data, and Performance Drift Detection)

Monitor and detect drift and degradation across model outputs, input distributions, and data schemas to identify quality regressions and security-relevant behavioral shifts.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.3.1** | **Verify that** model performance metrics (accuracy, precision, recall, F1 score, confidence scores, latency, and error rates) are continuously monitored across model versions and time periods. | 1 |
| **12.3.2** | **Verify that** data drift detection monitors input distribution changes that may impact model performance, using statistically validated methods matched to the input data type (e.g., KS test or PSI for tabular numeric features, embedding-distance metrics for text or image). | 1 |
| **12.3.3** | **Verify that** baseline performance profiles are formally documented and version-controlled, and are reviewed at a defined frequency or after any model or data pipeline change. | 2 |
| **12.3.4** | **Verify that** automated alerting triggers when performance metrics exceed predefined degradation thresholds or deviate significantly from baselines. | 2 |
| **12.3.5** | **Verify that** hallucination detection monitors identify and flag model outputs that contain factually incorrect, inconsistent, or fabricated information. | 2 |
| **12.3.6** | **Verify that** schema drift in incoming data (unexpected field additions, removals, type changes, or format variations) is detected and triggers alerting. | 2 |
| **12.3.7** | **Verify that** concept drift detection identifies changes in the relationship between inputs and expected outputs. | 2 |
| **12.3.8** | **Verify that** performance degradation alerts trigger a defined remediation workflow (e.g., manual review, retraining, or replacement). | 2 |
| **12.3.9** | **Verify that** hallucination rates are tracked as continuous time-series metrics to enable trend analysis and detection of sustained model degradation. | 2 |
| **12.3.10** | **Verify that** training pipeline instrumentation continuously monitors runtime duration, loss trajectory, and convergence rate against established baselines for equivalent dataset size and model architecture. Statistically significant deviations (e.g., abnormally prolonged training duration or erratic loss curves) trigger automated alerts and gate the resulting model artifact pending investigation, since such anomalies may indicate adversarial poisoning payloads in the training data. | 2 |
| **12.3.11** | **Verify that** degradation root cause analysis correlates performance drops with data changes, infrastructure issues, or external factors. | 3 |
| **12.3.12** | **Verify that** sudden unexplained behavioral shifts are distinguished from gradual expected operational drift, with a security escalation path defined for unexplained sudden drift. | 3 |

---

## C12.4 パフォーマンスと動作のテレメトリ (Performance & Behavior Telemetry)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.4.1** | **Verify that** token usage is tracked at granular attribution levels including per user, per session, per feature endpoint, and per team or workspace. | 2 |
| **12.4.2** | **Verify that** output-to-input token ratio anomalies are detected and alerted. | 2 |

---

## C12.5 AI インシデント対応の計画と実行 (AI Incident Response Planning & Execution)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.5.1** | **Verify that** incident response plans specifically address AI-related security events including model compromise, data poisoning, adversarial attacks, model inversion, prompt injection campaigns, and model extraction, with specific containment and investigation steps for each scenario. | 1 |
| **12.5.2** | **検証:** インシデント対応チームは、モデルの動作と攻撃ベクトルを調査するために、AI 固有のフォレンジックツールと専門知識にアクセスしている。 | 2 |
| **12.5.3** | **検証:** インシデント後の分析は、モデルの再トレーニングの考慮、安全フィルタの更新、学んだ教訓のセキュリティコントロールへの統合を含んでいる。 | 3 |

---

## C12.6 プロアクティブなセキュリティ動作監視 (Proactive Security Behavior Monitoring)

Detect and prevent security threats arising from proactive (agent-initiated) behavior, including pre-execution validation, behavior pattern analysis, and audit trails for approval of security-critical actions.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.6.1** | **検証:** プロアクティブなエージェントの動作は、リスク評価での統合など、実行前にセキュリティ検証されている。 | 1 |
| **12.6.2** | **検証:** 自律的なイニチアチブトリガーはセキュリティコンテキストの評価と脅威状況の評価を含んでいる。 | 2 |
| **12.6.3** | **検証:** プロアクティブな動作パターンは、潜在的なセキュリティ影響と意図しない結果について分析されている。 | 2 |
| **12.6.4** | **Verify that** audit logs capture the complete approval chain for security-critical proactive actions, including approver identity, timestamp, action parameters, and decision outcome. | 2 |
| **12.6.5** | **Verify that** audit log records include identity, scope, authorization decisions, tool parameters, and outcomes. | 2 |
| **12.6.6** | **Verify that** kill-switch activations and override commands are logged. | 2 |
| **12.6.7** | **Verify that** self-modifications are explicitly classified as security-relevant events and logged with sufficient detail to reconstruct what changed, when, by which agent or principal, and under what authorization. | 2 |
| **12.6.8** | **検証:** 動作異常検出は、侵害を示す可能性のあるプロアクティブなエージェントパターンの逸脱を識別している。 | 3 |

---

## C12.7 Training Data & Model Lifecycle Audit

Ensure that the provenance and change history of training data, model artifacts, and knowledge sources are auditable throughout the AI development lifecycle.

| # | Description | Level |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.7.1** | **Verify that** the lineage of each dataset and its components, including all transformations, augmentations, and merges, is recorded and can be reconstructed. | 1 |
| **12.7.2** | **Verify that** all labeling activities are recorded in logs. | 1 |
| **12.7.3** | **Verify that** all model changes (deployment, configuration, retirement) generate immutable audit records. | 2 |
| **12.7.4** | **Verify that** every ingested document is tagged at write time with source, writer identity, and timestamp. | 2 |
| **12.7.5** | **Verify that** all training datasets are uniquely identified, with change tracking, to support rollback and forensic analysis. | 3 |

---

## 参考情報

* [OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
* [MITRE ATLAS - Adversarial Threat Landscape for AI Systems](https://atlas.mitre.org/)
* [NIST AI Risk Management Framework (AI RMF 1.0)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
* [NIST AI 100-1 - Artificial Intelligence Risk Management Framework](https://doi.org/10.6028/NIST.AI.100-1)
* [OWASP Agentic AI Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
* [Microsoft Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit)
* [NIST SP 800-207 Zero Trust Architecture](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-207.pdf)
