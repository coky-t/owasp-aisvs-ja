# C3 モデルライフサイクル管理と変更管理 (Model Lifecycle Management & Change Control)

## 管理目標

AI システムは、不正または安全でないモデル変更が本番環境に到達することを防ぐ変更管理プロセスを実装する必要があります。この管理は、開発からデプロイメントを通じて廃止に至るまでのライフサイクル全体を通じてモデルの完全性を確保して、迅速なインシデント対応を可能にし、すべての変更に対する説明責任を維持します。

**主要なセキュリティ目標:** 完全性、追跡可能性、回復可能性を維持する管理プロセスを採用することで、認可され、検証されたモデルのみが本番環境に到達します。

---

## C3.1 モデル認可と完全性 (Model Authorization & Integrity)

完全性を検証され、認可されたモデルのみが本番環境に到達します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.1.1** | **検証:** すべてのモデルアーティファクト (重み、構成、トークナイザ) はデプロイメント前に認可されたエンティティで暗号署名されている。 | 1 | D/V |
| **3.1.2** | **検証:** モデル完全性はデプロイメント時に検証され、署名検証の失敗はモデルのローディングを防いでいる。 | 1 | D/V |
| **3.1.3** | **検証:** モデルの来歴レコードは、認可エンティティのアイデンティティ、トレーニングデータのチェックサム、合格/不合格ステータスでのバリデーションテスト結果、作成タイムスタンプを含んでいる。 | 2 | D/V |
| **3.1.4** | **検証:** すべてのモデルアーティファクトは、各バージョンコンポーネントが増える時期を指定する文書化された基準を持つ、セマンティックバージョン管理 (MAJOR.MINOR.PATCH) を使用している。 | 2 | D/V |
| **3.1.5** | **検証:** 依存関係の追跡は、すべての消費システムの迅速な識別を可能にする、リアルタイムインベントリを維持している。 | 2 | V |

---

## C3.2 モデルバリデーションとテスト (Model Validation & Testing)

モデルは、定義されたセキュリティと安全性のバリデーションにデプロイメント前に合格する必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.2.1** | **検証:** モデルは、デプロイメント前に、入力バリデーション、出力サニタイゼーション、事前に合意された組織の合格/不合格閾値での安全性評価を含む自動セキュリティテストを受けている。 | 1 | D/V |
| **3.2.2** | **検証:** バリデーションの失敗は、事前に指名された権限のある担当者が文書化されたビジネス上の正当性とともに明示的にオーバーライドを承認した後、モデルのデプロイメントを自動的にブロックします。 | 1 | D/V |
| **3.2.3** | **検証:** テスト結果は暗号署名され、検証対象の特定のモデルバージョンハッシュに不変的にリンクされている。 | 2 | V |
| **3.2.4** | **検証:** 緊急デプロイメントは、事前に合意された期間内での、文書化されたセキュリティリスク評価と事前に指定されたセキュリティ機関からの承認を必要としている。 | 2 | D/V |

---

## C3.3 制御されたデプロイメントとロールバック (Controlled Deployment & Rollback)

Model deployments must be controlled, monitored, and reversible.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.3.1** | **Verify that** production deployments implement gradual rollout mechanisms (canary deployments, blue-green deployments) with automated rollback triggers based on pre-agreed error rates, latency thresholds, or security alert criteria. | 1 | D |
| **3.3.2** | **Verify that** rollback capabilities restore the complete model state (weights, configurations, dependencies) atomically within pre-defined organizational time windows. | 1 | D/V |
| **3.3.3** | **Verify that** deployment processes validate cryptographic signatures and compute integrity checksums before model activation, failing deployment on any mismatch. | 2 | D/V |
| **3.3.4** | **Verify that** emergency model shutdown capabilities can disable model endpoints within pre-defined response times via automated circuit breakers or manual kill switches. | 2 | D/V |
| **3.3.5** | **Verify that** rollback artifacts (previous model versions, configurations, dependencies) are retained according to organizational policies with immutable storage for incident response. | 2 | V |

---

## C3.4 変更の説明責任と監査 (Change Accountability & Audit)

All model lifecycle changes must be traceable and auditable.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.4.1** | **Verify that** all model changes (deployment, configuration, retirement) generate immutable audit records including a timestamp, an authenticated actor identity, a change type, and before/after states. | 1 | V |
| **3.4.2** | **Verify that** audit log access requires appropriate authorization and all access attempts are logged with user identity and a timestamp. | 2 | D/V |
| **3.4.3** | **Verify that** prompt templates and system messages are version-controlled in git repositories with mandatory code review and approval from designated reviewers before deployment. | 2 | D/V |
| **3.4.4** | **Verify that** audit records include sufficient detail (model hashes, configuration snapshots, dependency versions) to enable complete reconstruction of model state for any timestamp within retention period. | 2 | V |

---

## C3.5 セキュア開発プラクティス (Secure Development Practices)

Model development and training processes must follow secure practices to prevent compromise.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.5.1** | **Verify that** model development, testing, and production environments are physically or logically separated. They have no shared infrastructure, distinct access controls, and isolated data stores. | 1 | D |
| **3.5.2** | **Verify that** model training and fine-tuning occur in isolated environments with controlled network access. | 1 | D |
| **3.5.3** | **Verify that** training data sources are validated through integrity checks and authenticated via trusted sources with documented chain of custody before use in model development. | 1 | D/V |
| **3.5.4** | **Verify that** model development artifacts (hyperparameters, training scripts, configuration files) are stored in version control and require peer review approval before use in training. | 2 | D |

---

## C3.6 モデルの廃止と廃棄 (Model Retirement & Decommissioning)

Models must be securely retired when they are no longer needed or when security issues are identified.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.6.1** | **Verify that** model retirement processes automatically scan dependency graphs, identify all consuming systems, and provide pre-agreed advance notice periods before decommissioning. | 1 | D |
| **3.6.2** | **Verify that** retired model artifacts are securely wiped using cryptographic erasure or multi-pass overwriting according to documented data retention policies with verified destruction certificates. | 1 | D/V |
| **3.6.3** | **Verify that** model retirement events are logged with timestamp and actor identity, and model signatures are revoked to prevent reuse. | 2 | V |
| **3.6.4** | **Verify that** emergency model retirement can disable model access within pre-established emergency response timeframes through automated kill switches if critical security vulnerabilities are discovered. | 2 | D/V |

---

## 参考情報

* [MLOps Principles](https://ml-ops.org/content/mlops-principles)
* [Securing AI/ML Ops – Cisco.com](https://sec.cloudapps.cisco.com/security/center/resources/SecuringAIMLOps)
* [Audit logs security: cryptographically signed tamper-proof logs](https://www.cossacklabs.com/blog/audit-logs-security/)
* [Machine Learning Model Versioning: Top Tools & Best Practices](https://lakefs.io/blog/model-versioning/)
* [AI Hygiene Starts with Models and Data Loaders – SEI Blog](https://insights.sei.cmu.edu/documents/6190/AI-Hygiene-Starts-with-Models-and-Data-Loaders_1G0KTRh.pdf)
* [How to handle versioning and rollback of a deployed ML model?](https://learn.microsoft.com/en-au/answers/questions/1845378/how-to-handle-versioning-and-rollback-of-a-deploye)
* [Reinforcement fine-tuning – OpenAI API](https://platform.openai.com/docs/guides/reinforcement-fine-tuning)
* [Auditing Machine Learning models: Governance, Data Security and …](https://www.linkedin.com/pulse/auditing-machine-learning-models-governance-data-security-negrete-yn81f)
* [Adversarial Training to Improve Model Robustness](https://medium.com/%40amit25173/adversarial-training-to-improve-model-robustness-5e285b516713)
* [What is AI adversarial robustness? – IBM Research](https://research.ibm.com/blog/securing-ai-workflows-with-adversarial-robustness)
* [Exploring Data Provenance: Ensuring Data Integrity and Authenticity](https://www.astera.com/type/blog/data-provenance/)
* [MITRE ATLAS](https://atlas.mitre.org/)
* [AWS Prescriptive Guidance – Best practices for retiring applications …](https://docs.aws.amazon.com/pdfs/prescriptive-guidance/latest/migration-app-retirement-best-practices/migration-app-retirement-best-practices.pdf)
