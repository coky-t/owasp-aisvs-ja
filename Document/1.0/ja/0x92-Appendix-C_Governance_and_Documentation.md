# 付録 C: AI セキュリティガバナンスとドキュメント (AI Security Governance & Documentation) (再編)

## 目標

この付録では、システムライフサイクル全体にわたって AI セキュリティを管理するために、組織構造、ポリシー、ドキュメント、プロセスを確立するための基本要件を提供します。

---

## AC.1 AI リスク管理フレームワークの導入 (AI Risk Management Framework Adoption)

| # | 説明 | レベル | ロール |
| :--------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.1.1** | **検証:** AI 固有のリスク評価手法は文書化され、実装されている。 | 1 | D/V |
| **AC.1.2** | **検証:** リスク評価は、AI ライフサイクルの重要なポイントと、重大な変更の前に実施されている。 | 2 | D |
| **AC.1.3** | **検証:** リスク管理フレームワークは確立した標準 (NIST AI RMF など) に準拠している。 | 3 | D/V |

---

## AC.2 AI セキュリティポリシーと手順 (AI Security Policy & Procedures)

| # | 説明 | レベル | ロール |
| :--------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.2.1** | **検証:** 文書化された AI セキュリティポリシーは存在している。 | 1 | D/V |
| **AC.2.2** | **検証:** ポリシーは少なくとも年に一度、および重大な脅威状況の変化の後に見直され、更新されている。 | 2 | D |
| **AC.2.3** | **検証:** ポリシーはすべての AISVS カテゴリと適用可能な規制要件に対応している。 | 3 | D/V |

---

## AC.3 AI セキュリティの役割と責任 (Roles & Responsibilities for AI Security)

| # | 説明 | レベル | ロール |
| :--------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.3.1** | **検証:** AI セキュリティの役割と責任は文書化されている。 | 1 | D/V |
| **AC.3.2** | **検証:** 責任者は適切なセキュリティ専門知識を有している。 | 2 | D |
| **AC.3.3** | **検証:** AI 倫理委員会またはガバナンスボードは高リスクの AI システムに対して設立されている。 | 3 | D/V |

---

## AC.4 倫理的 AI ガイドラインの施行 (Ethical AI Guidelines Enforcement)

| # | 説明 | レベル | ロール |
| :--------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.4.1** | **検証:** AI 開発とデプロイメントに関する倫理ガイドラインは存在している。 | 1 | D/V |
| **AC.4.2** | **検証:** 倫理違反を検出し報告するためのメカニズムは整備されている。 | 2 | D |
| **AC.4.3** | **検証:** デプロイされた AI システムに対する定期的な倫理レビューは実施されている。 | 3 | D/V |

---

## AC.5 AI 規制コンプライアンスの監視 (AI Regulatory Compliance Monitoring)

| # | 説明 | レベル | ロール |
| :--------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.5.1** | **検証:** 適用可能な AI 規制を特定するためのプロセスは存在している。 | 1 | D/V |
| **AC.5.2** | **検証:** すべての規制要件への準拠は評価されている。 | 2 | D |
| **AC.5.3** | **検証:** 規制の変化は AI システムへのタイムリーなレビューと更新をトリガーしている。 | 3 | D/V |

---

## AC.6 トレーニングデータガバナンス、ドキュメント、プロセス (Training Data Governance, Documentation & Process)

### AC.6.1 データソーシングとデューディリジェンス (Data Sourcing & Due Diligence)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.1.1** | **検証:** 品質、代表性、倫理的調達、ライセンス遵守について精査されたデータセットのみを許可し、ポイズニング、埋め込まれたバイアス、知的財産侵害のリスクを低減している。 | 1 | D/V |
| **AC.6.1.2** | **検証:** 事前トレーニング済みモデルや外部データセットのプロバイダを含むサードパーティのデータサプライヤは、そのデータやモデルを統合する前に、セキュリティ、プライバシー、倫理的調達、データ品質のデューディリジェンスを受けている。 | 2 | D/V |
| **AC.6.1.3** | **検証:** 外部転送は TLS/auth と完全性チェックを使用している。 | 1 | D |
| **AC.6.1.4** | **検証:** 高リスクのデータソース (来歴不明のオープンソースデータセット、審査されていないサプライヤなど) は、機密性の高いアプリケーションで使用する前に、サンドボックス解析、広範な品質/バイアスチェック、標的を絞ったポイズニング検出などの強化された精査を受けている。 | 2 | D/V |
| **AC.6.1.5** | **検証:** サードパーティから取得した事前トレーニング済みモデルは、ファインチューニングやデプロイメントの前に、埋め込まれたバイアス、潜在的なバックドア、アーキテクチャの完全性、元のトレーニングデータの来歴について評価されている。 | 3 | D/V |

### AC.6.2 バイアスと公平性の管理 (Bias & Fairness Management)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.2.1** | **検証:** データセットは、法的に保護された属性 (人種、性別、年齢など) とモデルの適用ドメインに関連するその他の倫理的にセンシティブな特定 (社会経済的ステータス、位置情報など) にわたって、表現の不均衡と潜在的なバイアスについてプロファイルされている。 | 1 | D/V |
| **AC.6.2.2** | **検証:** 特定されたバイアスは、再バランス調整、対象を絞ったデータ拡張、アルゴリズム調整 (前処理、中処理、後処理技法など)、再重み付けなどの文書化された戦略によって緩和され、緩和策が公平性と全体的なモデルパフォーマンスの両方に与える影響を評価している。 | 2 | D/V |
| **AC.6.2.3** | **検証:** トレーニング後の公平性のメトリクスが評価され、文書化されている。 | 2 | D/V |
| **AC.6.2.4** | **検証:** ライフサイクルバイアス管理ポリシーは所有者とレビュー頻度を割り当てている。 | 3 | D/V |

### AC.6.3 Labeling & Annotation Governance

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
|  **AC.6.3.1** | **Verify that** labelling/annotation quality is ensured via reviewer cross-checks or consensus.                                                                                                                                         |   2   |  D/V |
|  **AC.6.3.2** | **Verify that** data cards are maintained for significant training datasets, detailing characteristics, motivations, composition, collection processes, preprocessing, licenses, and recommended/discouraged uses.                      |   2   |  D/V |
|  **AC.6.3.3** | **Verify that** data cards document bias risks, demographic skews, and ethical considerations relevant to the dataset.                                                                                                                  |   2   |  D/V |
|  **AC.6.3.4** | **Verify that** data cards are versioned alongside datasets and updated whenever the dataset is modified.                                                                                                                               |   2   |  D/V |
|  **AC.6.3.5** | **Verify that** data cards are reviewed and approved by both technical and non-technical stakeholders (e.g., compliance, ethics, domain experts).                                                                                       |   2   |  D/V |
|  **AC.6.3.6** | **Verify that** labeling/annotation quality is ensured via clear guidelines, reviewer cross-checks, consensus mechanisms (e.g., monitoring inter-annotator agreement), and defined processes for resolving discrepancies.               |   2   |  D/V |
|  **AC.6.3.7** | **Verify that** labels critical to safety, security, or fairness (e.g., identifying toxic content, critical medical findings) receive mandatory independent dual review or equivalent robust verification.                              |   3   |  D/V |
|  **AC.6.3.8** | **Verify that** labeling guides and instructions are comprehensive, version-controlled, and peer-reviewed.                                                                                                                              |   2   |  D/V |
|  **AC.6.3.9** | **Verify that** data schemas for labels are clearly defined, and version-controlled.                                                                                                                                                    |   2   |  D/V |
|  **AC.6.3.10** | **Verify that** outsourced or crowdsourced labeling workflows include technical/procedural safeguards to ensure data confidentiality, integrity, label quality, and prevent data leakage.                                              |   2   |  D/V |
|  **AC.6.3.11** | **Verify that** all personnel involved in data annotation are background-checked and trained in data security and privacy.                                                                                                             |   2   |  D/V |
|  **AC.6.3.12** | **Verify that** all annotation personnel sign confidentiality and non-disclosure agreements.                                                                                                                                           |   2   |  D/V |
|  **AC.6.3.13** | **Verify that** annotation platforms enforce access controls and monitor for insider threats.                                                                                                                                          |   2   |  D/V |

### AC.6.4 Dataset Quality Gates & Quarantine

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.4.1** | **Verify that** failed datasets are quarantined with audit trails.                                                                                                                                                                                |   2   |  D/V |
| **AC.6.4.2** | **Verify that** quality gates block sub-par datasets unless exceptions are approved.                                                                                                                                                              |   2   |  D/V |
| **AC.6.4.3** | **Verify that** manual spot-checks by domain experts cover a statistically significant sample (e.g., ≥1% or 1,000 samples, whichever is greater, or as determined by risk assessment) to identify subtle quality issues not caught by automation. |   2   |   V  |

### AC.6.5 Threat/Poisoning Detection & Drift

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.5.1** | **Verify that** flagged samples trigger manual review before training.                            |   2   |  D/V |
| **AC.6.5.2** | **Verify that** results feed the model's security dossier and inform ongoing threat intelligence. |   2   |   V  |
| **AC.6.5.3** | **Verify that** detection logic is refreshed with new threat intel.                               |   3   |  D/V |
| **AC.6.5.4** | **Verify that** online-learning pipelines monitor distribution drift.                             |   3   |  D/V |

### AC.6.6 Deletion, Consent, Rights, Retention & Compliance

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
|  **AC.6.6.1** | **Verify that** training data deletion workflows purge primary and derived data and assess model impact, and that the impact on affected models is assessed and, if necessary, addressed (e.g., through retraining or recalibration).                              |   1   |  D/V |
|  **AC.6.6.2** | **Verify that** mechanisms are in place to track and respect the scope and status of user consent (and withdrawals) for data used in training, and that consent is validated before data is incorporated into new training processes or significant model updates. |   2   |   D  |
|  **AC.6.6.3** | **Verify that** workflows are tested annually and logged.                                                                                                                                                                                                          |   2   |   V  |
|  **AC.6.6.4** | **Verify that** explicit retention periods are defined for all training datasets.                                                                                                                                                                                  |   1   |  D/V |
|  **AC.6.6.5** | **Verify that** datasets are automatically expired, deleted, or reviewed for deletion at the end of their lifecycle.                                                                                                                                               |   2   |  D/V |
|  **AC.6.6.6** | **Verify that** retention and deletion actions are logged and auditable.                                                                                                                                                                                           |   2   |  D/V |
|  **AC.6.6.7** | **Verify that** data residency and cross-border transfer requirements are identified and enforced for all datasets.                                                                                                                                                |   2   |  D/V |
|  **AC.6.6.8** | **Verify that** sector-specific regulations (e.g., healthcare, finance) are identified and addressed in data handling.                                                                                                                                             |   2   |  D/V |
|  **AC.6.6.9** | **Verify that** compliance with relevant privacy laws (e.g., GDPR, CCPA) is documented and reviewed regularly.                                                                                                                                                     |   2   |  D/V |
| **AC.6.6.10** | **Verify that** mechanisms exist to respond to data subject requests for access, rectification, restriction, or objection.                                                                                                                                         |   2   |  D/V |
| **AC.6.6.11** | **Verify that** requests are logged, tracked, and fulfilled within legally mandated timeframes.                                                                                                                                                                    |   2   |  D/V |
| **AC.6.6.12** | **Verify that** data subject rights processes are tested and reviewed regularly for effectiveness.                                                                                                                                                                 |   2   |  D/V |

### AC.6.7 Versioning & Change Management

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.7.1** | **Verify that** an impact analysis is performed before updating or replacing a dataset version, covering model performance, fairness, and compliance. |   2   |  D/V |
| **AC.6.7.2** | **Verify that** results of the impact analysis are documented and reviewed by relevant stakeholders.                                                  |   2   |  D/V |
| **AC.6.7.3** | **Verify that** rollback plans exist in case new versions introduce unacceptable risks or regressions.                                                |   2   |  D/V |

### AC.6.8 Synthetic Data Governance

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.8.1** | **Verify that** the generation process, parameters, and intended use of synthetic data are documented.                         |   2   |  D/V |
| **AC.6.8.2** | **Verify that** synthetic data is risk-assessed for bias, privacy leakage, and representational issues before use in training. |   2   |  D/V |

### AC.6.9 Access Monitoring

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.9.1** | **Verify that** access logs are regularly reviewed for unusual patterns, such as large exports or access from new locations. |   2   |  D/V |
| **AC.6.9.2** | **Verify that** alerts are generated for suspicious access events and investigated promptly.                                 |   2   |  D/V |

### AC.6.10 Adversarial Training Governance

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.6.10.1** | **Verify that** if adversarial training is used, the generation, management, and versioning of adversarial datasets are documented and controlled.                                           |   2   |  D/V |
| **AC.6.10.2** | **Verify that** the impact of adversarial robustness training on model performance (against both clean and adversarial inputs) and fairness metrics is evaluated, documented, and monitored. |   3   |  D/V |
| **AC.6.10.3** | **Verify that** strategies for adversarial training and robustness are periodically reviewed and updated to counter evolving adversarial attack techniques.                                  |   3   |  D/V |

---

## AC.7 モデルライフサイクルガバナンスとドキュメント (Model Lifecycle Governance & Documentation)

| # | 説明 | レベル | ロール |
| :--------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.7.1** | **検証:** すべてのモデルアーティファクトは、各バージョンコンポーネントがいつ増加するかを指定する、文書化された基準を備えたセマンティックバージョン管理 (MAJOR.MINOR.PATCH) を使用している。 | 2 | D/V |
| **AC.7.2** | **検証:** 緊急デプロイメントは、事前に合意された期間内に、文書化されたセキュリティリスク評価と、事前に指定されたセキュリティ機関からの承認を必要としている。 | 2 | D/V |
| **AC.7.3** | **検証:** ロールバックアーティファクト (以前のモデルバージョン、構成、依存関係) は組織のポリシーに従って保持されている。 | 2 | V |
| **AC.7.4** | **検証:** 監査ログへのアクセスは適切な認可を必要としており、すべてのアクセス試行はユーザーアイデンティティとタイムスタンプとともにログ記録されている。 | 2 | D/V |
| **AC.7.5** | **検証:** 廃止されたモデルアーティファクトはデータ保持ポリシーに従って保持されている。 | 1 | D/V |

---

## AC.8 プロンプト、入力、出力の安全性ガバナンス (Prompt, Input, and Output Safety Governance)

### AC.8.1 プロンプトインジェクションの防御 (Prompt Injection Defense)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.8.1.1** | **検証:** 敵対的評価テスト (レッドチームの「メニーショット」プロンプトなど) は、すべてのモデルまたはプロンプトテンプレートのリリース前に、成功率の閾値と回帰の自動ブロッカーを使用して、実行されている。 | 2 |  D/V |
| **AC.8.1.2** | **検証:** すべてのプロンプトフィルタルールの更新、分類モデルのバージョン、ブロックリストの変更はバージョン管理され、監査可能である。 | 3 |  D/V |

### AC.8.2 敵対的サンプルへの耐性 (Adversarial-Example Resistance)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.8.2.1** | **検証:** 堅牢性メトリクス (既知の攻撃スイートの成功率) は自動化によって時間の経過とともに追跡され、回帰はアラートをトリガーしている。 | 3 |  D/V |

### AC.8.3 コンテンツとポリシーの審査 (Content & Policy Screening)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.8.3.1** | **検証:** スクリーニングモデルやルールセットは、新たに観察されたジェイルブレイクやポリシーバイパスのパターンを組み込んで、少なくとも四半期ごとに再トレーニング/アップデートされている。 | 2 | D |

### AC.8.4 入力レート制限と不正使用防止 (Input Rate Limiting & Abuse Prevention)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.8.4.1** | **検証:** 不正使用防止ログは保持され、新たな攻撃パターンがないかレビューされている。 | 3 | V |

### AC.8.5 入力の来歴と属例 (Input Provenance & Attribution)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.8.5.1** | **検証:** すべてのユーザー入力は取り込み時にメタデータ (ユーザーID、セッション、ソース、タイムスタンプ、IP アドレス) でタグ付けされている。 | 1 | D/V |
| **AC.8.5.2** | **検証:** 来歴メタデータが保持され、すべての処理された入力について監査可能である。 | 2 | D/V |
| **AC.8.5.3** | **検証:** 異常な入力ソースや信頼できない入力ソースはフラグ付けされ、強化された精査またはブロックの対象としている。 | 2 | D/V |

---

## AC.9 Multimodal Validation, MLOps & Infrastructure Governance

### AC.9.1 マルチモーダルセキュリティバリデーションパイプライン (Multimodal Security Validation Pipeline)

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.1.1** | **検証:** 様式固有のコンテンツ分類子は、文書化されたスケジュール (最低四半期ごと) に従って更新され、新しい脅威パターン、敵対的サンプル、パフォーマンスベンチマークがベースライン閾値を上回るように維持している。 | 3 | D/V |

### AC.9.2 CI/CD & Build Security

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.2.1** | **Verify that** infrastructure-as-code is scanned on every commit, and merges are blocked on critical or high-severity findings. |   1   |  D/V |
| **AC.9.2.2** | **Verify that** CI/CD pipelines use short-lived, scoped identities for access to secrets and infrastructure.                     |   2   |  D/V |
| **AC.9.2.3** | **Verify that** build environments are isolated from production networks and data.                                               |   2   |  D/V |

### AC.9.3 Container & Image Security

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.3.1** | **Verify that** container images are scanned to block hardcoded secrets (e.g., API keys, credentials, certificates).                                                          |   2   |  D/V |
| **AC.9.3.2** | **Verify that** container images are scanned according to organizational schedules with CRITICAL vulnerabilities blocking deployment based on organizational risk thresholds. |   1   |  D/V |

### AC.9.4 Monitoring, Alerting & SIEM

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.4.1** | **Verify that** security alerts integrate with SIEM platforms (Splunk, Elastic, or Sentinel) using CEF or STIX/TAXII formats with automated enrichment. |   2   |   V  |

### AC.9.5 Vulnerability Management

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.5.1** | **Verify that** HIGH severity vulnerabilities are patched according to organizational risk management timelines with emergency procedures for actively exploited CVEs. |   2   |  D/V |

### AC.9.6 Configuration & Drift Control

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.6.1** | **Verify that** configuration drift is detected using tools (Chef InSpec, AWS Config) according to organizational monitoring requirements with automatic rollback for unauthorized changes. |   2   |  D/V |

### AC.9.7 Production Environment Hardening

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.7.1** | **Verify that** production environments block SSH access, disable debug endpoints, and require change requests with organizational advance notice requirements except emergencies. |   2   |  D/V |

### AC.9.8 Release Promotion Gates

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.8.1** | **Verify that** promotion gates include automated security tests (SAST, DAST, container scanning) with zero CRITICAL findings required for approval. |   2   |  D/V |

### AC.9.9 Workload, Capacity & Cost Monitoring

| # | 説明 | レベル | ロール |
| :----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.9.1** | **Verify that** GPU/TPU utilization is monitored with alerts triggered at organizationally defined thresholds and automatic scaling or load balancing activated based on capacity management policies. |   1   |  D/V |
| **AC.9.9.2** | **Verify that** AI workload metrics (inference latency, throughput, error rates) are collected according to organizational monitoring requirements and correlated with infrastructure utilization.     |   1   |  D/V |
| **AC.9.9.3** | **Verify that** cost monitoring tracks spending per workload/tenant with alerts based on organizational budget thresholds and automated controls for budget overruns.                                  |   2   |   V  |
| **AC.9.9.4** | **Verify that** capacity planning uses historical data with organizationally defined forecasting periods and automated resource provisioning based on demand patterns.                                 |   3   |   V  |

### AC.9.10 Approvals & Audit Trails

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.10.1** | **Verify that** environment promotion requires approval from organizationally defined authorized personnel with cryptographic signatures and immutable audit trails. |   1   |  D/V |

### AC.9.11 IaC Governance

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.11.1** | **Verify that** infrastructure-as-code changes require peer review with automated testing and security scanning before merge to main branch. |   2   |  D/V |

### AC.9.12 Data Handling in Non-Production

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.12.1** | **Verify that** non-production data is anonymized according to organizational privacy requirements, synthetic data generation, or complete data masking with PII removal verified. |   2   |  D/V |

### AC.9.13 Backup & Disaster Recovery

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.13.1** | **Verify that** infrastructure configurations are backed up according to organizational backup schedules to geographically separate regions with 3-2-1 backup strategy implementation.     |   1   |  D/V |
| **AC.9.13.2** | **Verify that** recovery procedures are tested and validated through automated testing according to organizational schedules with RTO and RPO targets meeting organizational requirements. |   2   |   V  |
| **AC.9.13.3** | **Verify that** disaster recovery includes AI-specific runbooks with model weight restoration, GPU cluster rebuilding, and service dependency mapping.                                     |   3   |   V  |

### AC.9.14 Compliance & Documentation

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.14.1** | **Verify that** infrastructure compliance is assessed according to organizational schedules against SOC 2, ISO 27001, or FedRAMP controls with automated evidence collection. |   2   |  D/V |
| **AC.9.14.2** | **Verify that** infrastructure documentation includes network diagrams, data flow maps, and threat models updated according to organizational change management requirements. |   2   |   V  |
| **AC.9.14.3** | **Verify that** infrastructure changes undergo automated compliance impact assessment with regulatory approval workflows for high-risk modifications.                         |   3   |  D/V |

### AC.9.15 Hardware & Supply Chain

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.15.1** | **Verify that** AI accelerator firmware (GPU BIOS, TPU firmware) is verified with cryptographic signatures and updated according to organizational patch management timelines. |   2   |  D/V |
| **AC.9.15.2** | **Verify that** the AI hardware supply chain includes provenance verification with manufacturer certificates and tamper-evident packaging validation.                          |   3   |   V  |

### AC.9.16 Cloud Strategy & Portability

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.16.1** | **Verify that** cloud vendor lock-in prevention includes portable infrastructure-as-code, standardized APIs, and data export capabilities with format conversion tools. |   3   |   V  |
| **AC.9.16.2** | **Verify that** multi-cloud cost optimization includes security controls preventing resource sprawl as well as unauthorized cross-cloud data transfer charges.          |   3   |   V  |

### AC.9.17 GitOps & Self-Healing

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.17.1** | **Verify that** GitOps repositories require signed commits with GPG keys and branch protection rules preventing direct pushes to main branches.          |   2   |  D/V |
| **AC.9.17.2** | **Verify that** self-healing infrastructure includes security event correlation with automated incident response and stakeholder notification workflows. |   3   |   V  |

### AC.9.18 Zero-Trust, Agents, Provisioning & Residency Attestation

| # | 説明 | レベル | ロール |
| :-----------: | :------------------------------------------------------------------------------- | :---: | :--: |
| **AC.9.18.1** | **Verify that** cloud resource access includes zero-trust verification with continuous authentication.                                                     |   2   |  D/V |
| **AC.9.18.2** | **Verify that** automated infrastructure provisioning includes security policy validation with deployment blocking for non-compliant configurations.       |   2   |  D/V |
| **AC.9.18.3** | **Verify that** automated infrastructure provisioning validates security policies during CI/CD, with non-compliant configurations blocked from deployment. |   2   |  D/V |
| **AC.9.18.4** | **Verify that** data residency requirements are enforced by cryptographic attestation of storage locations.                                                |   3   |  D/V |
| **AC.9.18.5** | **Verify that** cloud provider security assessments include agent-specific threat modeling and risk evaluation.                                            |   3   |  D/V |
