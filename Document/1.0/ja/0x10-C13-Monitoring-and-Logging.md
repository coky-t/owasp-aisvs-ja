# C13 監視、ログ記録、異常検出 (Monitoring, Logging & Anomaly Detection)

## 管理目標

このセクションでは、モデルやその他の AI コンポーネントが認識、実行、返却する内容についてリアルタイムかつフォレンジックな可視性を実現し、脅威を検出、トリアージ、学習するための要件を示します。

## C13.1 リクエストとレスポンスのログ記録 (Request & Response Logging)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.1.1** | **Verify that** AI interactions are logged with security-relevant metadata (e.g. timestamp, user ID, session ID, model version, token count, input hash, system prompt version, confidence score, safety filter outcome, and safety filter decisions) without logging prompt or response content by default. | 1 |
| **13.1.2** | **検証:** ログは、適切な保持ポリシーとバックアップ手順で、安全でアクセス制御されたリポジトリに保存されている。 | 1 |
| **13.1.3** | **検証:** ログストレージシステムは、ログに含まれる機密情報を保護するために、保存時および転送時の暗号化を実装している。 | 1 |
| **13.1.4** | **検証:** プロンプトと出力内の機密データは、PII、クレデンシャル、プロプライエタリ情報に対する構成可能な修正ルールで、ログ記録前に自動的に修正またはマスクされている。 | 1 |
| **13.1.5** | **検証:** ポリシー決定と安全フィルタリングアクションは、コンテンツモデレーションシステムの監査とデバッグを可能にするために、十分な詳細でログ記録されている。 | 2 |
| **13.1.6** | **検証:** ログの完全性は、暗号署名や書き込み専用ストレージなどによって、保護されている。 | 2 |
| **13.1.7** | **Verify that** log entries for AI inference events capture a structured, interoperable schema that includes at minimum model identifier, token usage (input and output), provider name, and operation type, to enable consistent AI observability across tools and platforms. | 2 |
| **13.1.8** | **Verify that** full prompt and response content is logged only when a security-relevant event is detected (e.g., safety filter trigger, prompt injection detection, anomaly flag), or when required by explicit user consent and a documented legal basis. | 2 |

---

## C13.2 不正使用の検出と警告 (Abuse Detection and Alerting)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.2.1** | **検証:** システムは、シグネチャベースの検出を使用して、既知のジェイルブレイクパターン、プロンプトインジェクションの試み、敵対的入力を検出し、警告している。 | 1 |
| **13.2.2** | **検証:** システムは、標準のログ形式とプロトコルを使用して、既存のセキュリティ情報およびイベント管理 (SIEM) プラットフォームと統合している。 | 1 |
| **13.2.3** | **検証:** 強化されたセキュリティイベントは、モデル識別子、信頼スコア、安全フィルタの決定などの AI 固有のコンテキストを含んでいる。 | 2 |
| **13.2.4** | **検証:** 行動異常検出は、異常な会話パターン、過度の再試行、体系的な調査行動を識別している。 | 2 |
| **13.2.5** | **検証:** リアルタイム警告メカニズムは、潜在的なポリシー違反や攻撃の試みが検出されると、セキュリティチームに通知している。 | 2 |
| **13.2.6** | **検証:** カスタムルールは、協調的なジェイルブレイクの試み、プロンプトインジェクションキャンペーン、モデル抽出攻撃などの AI 固有の脅威パターンを検出するために、含んでいる。 | 2 |
| **13.2.7** | **検証:** 自動インシデント対応ワークフローは侵害されたモデルを隔離し、悪意のあるユーザーをブロックしている。 | 3 |
| **13.2.8** | **Verify that** session-level conversation trajectory analysis detects multi-turn jailbreak patterns where no individual turn is overtly malicious in isolation but the aggregate conversation exhibits attack indicators. | 3 |
| **13.2.9** | **Verify that** per-user and per-session token consumption triggers an alert when consumption exceeds defined thresholds. | 2 |
| **13.2.10** | **Verify that** LLM API traffic is monitored for covert channel indicators, including Base64-encoded payloads, structured non-human query patterns, and communication signatures consistent with malware command-and-control activity using LLM endpoints. | 3 |

---

## C13.3 モデル、データ、パフォーマンスのドリフトの検出 (Model, Data, and Performance Drift Detection)

Monitor and detect drift and degradation across model outputs, input distributions, and data schemas to identify quality regressions and security-relevant behavioral shifts.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.3.1** | **Verify that** model performance metrics (accuracy, precision, recall, F1 score, confidence scores, latency, and error rates) are continuously monitored across model versions and time periods and compared against documented baselines. | 1 |
| **13.3.2** | **Verify that** baseline performance profiles are formally documented and version-controlled, and are reviewed at a defined frequency or after any model or data pipeline change. | 2 |
| **13.3.3** | **Verify that** automated alerting triggers when performance metrics exceed predefined degradation thresholds or deviate significantly from baselines. | 2 |
| **13.3.4** | **Verify that** hallucination detection monitors identify and flag instances when model outputs contain factually incorrect, inconsistent, or fabricated information. | 2 |
| **13.3.5** | **Verify that** data drift detection monitors input distribution changes that may impact model performance, using statistically validated methods appropriate to the data type. | 1 |
| **13.3.6** | **Verify that** schema drift in incoming data (unexpected field additions, removals, type changes, or format variations) is detected and triggers alerting. | 2 |
| **13.3.7** | **Verify that** concept drift detection identifies changes in the relationship between inputs and expected outputs. | 2 |
| **13.3.8** | **Verify that** degradation root cause analysis correlates performance drops with data changes, infrastructure issues, or external factors. | 3 |
| **13.3.9** | **Verify that** sudden unexplained behavioral shifts are distinguished from gradual expected operational drift, with a security escalation path defined for unexplained sudden drift. | 3 |
| **13.3.10** | **Verify that** performance degradation alerts trigger a defined remediation workflow (e.g., manual review, retraining, or replacement). | 2 |
| **13.3.11** | **Verify that** hallucination rates are tracked as continuous time-series metrics to enable trend analysis and detection of sustained model degradation. | 2 |

---

## C13.4 パフォーマンスと動作のテレメトリ (Performance & Behavior Telemetry)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.4.1** | **検証:** リクエストのレイテンシ、トークン消費量、メモリ使用量、スループットなどの運用メトリクスは継続的に収集および監視されている。 | 1 |
| **13.4.2** | **検証:** 成功率と失敗率はエラーの種類とその根本原因の分類で追跡されている。 | 1 |
| **13.4.3** | **検証:** リソース利用率の監視は、GPU/CPU 使用率、メモリ消費量、ストレージ要件を含んでおり、閾値違反でアラートしている。 | 2 |
| **13.4.4** | **Verify that** token usage is tracked at granular attribution levels including per user, per session, per feature endpoint, and per team or workspace. | 2 |
| **13.4.5** | **Verify that** output-to-input token ratio anomalies are detected and alerted. | 2 |

---

## C13.5 AI インシデント対応の計画と実行 (AI Incident Response Planning & Execution)

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.5.1** | **Verify that** incident response plans specifically address AI-related security events including model compromise, data poisoning, adversarial attacks, model inversion, prompt injection campaigns, and model extraction, with specific containment and investigation steps for each scenario. | 1 |
| **13.5.2** | **検証:** インシデント対応チームは、モデルの動作と攻撃ベクトルを調査するために、AI 固有のフォレンジックツールと専門知識にアクセスしている。 | 2 |
| **13.5.3** | **検証:** インシデント後の分析は、モデルの再トレーニングの考慮、安全フィルタの更新、学んだ教訓のセキュリティコントロールへの統合を含んでいる。 | 3 |

---

## C13.6 DAG 視覚化とワークフローセキュリティ (DAG Visualization & Workflow Security)

ワークフロー視覚化システムを情報漏洩や改竄攻撃から保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.6.1** | **検証:** DAG 視覚化データは、保存または転送前に機密情報を削除するために、サニタイズされている。 | 1 |
| **13.6.2** | **検証:** ワークフロー視覚化アクセス制御は、認可されたユーザーのみがエージェントの決定パスと推論トレースを閲覧できるようにしている。 | 1 |
| **13.6.3** | **検証:** DAG データの完全性は暗号署名と改竄防止ストレージメカニズムを通じて保護されている。 | 2 |
| **13.6.4** | **検証:** ワークフロー視覚化システムは、細工されたノードやエッジデータを通じたインジェクション攻撃を防ぐために、入力バリデーションを実装している。 | 2 |
| **13.6.5** | **検証:** リアルタイム DAG 更新はレート制限されており、視覚化システムに対するサービス拒否攻撃を防ぐために検証されている。 | 3 |

---

## C13.7 プロアクティブなセキュリティ動作監視 (Proactive Security Behavior Monitoring)

プロアクティブなエージェントの動作分析を通じてセキュリティ脅威を検出および防止します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **13.7.1** | **検証:** プロアクティブなエージェントの動作は、リスク評価の統合により実行前にセキュリティ検証されている。 | 1 |
| **13.7.2** | **検証:** 自律的なイニチアチブトリガーはセキュリティコンテキストの評価と脅威状況の評価を含んでいる。 | 2 |
| **13.7.3** | **検証:** プロアクティブな動作パターンは、潜在的なセキュリティ影響と意図しない結果について分析されている。 | 2 |
| **13.7.4** | **検証:** セキュリティ上重要なプロアクティブなアクションは監査証跡を含む明示的な承認チェーンを必要としている。 | 3 |
| **13.7.5** | **検証:** 動作異常検出は、侵害を示す可能性のあるプロアクティブなエージェントパターンの逸脱を識別している。 | 3 |

---

## 参考情報

* [OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
* [MITRE ATLAS - Adversarial Threat Landscape for AI Systems](https://atlas.mitre.org/)
* [NIST AI Risk Management Framework (AI RMF 1.0)](https://www.nist.gov/system/files/documents/2023/01/26/AI%20RMF%201.0.pdf)
* [NIST AI 100-1 - Artificial Intelligence Risk Management Framework](https://doi.org/10.6028/NIST.AI.100-1)
