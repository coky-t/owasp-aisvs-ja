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

モデルデプロイメントは制御され、監視され、元に戻すことができる必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.3.1** | **検証:** 本番環境デプロイメントは、事前に合意されたエラー率、レイテンシ閾値、またはセキュリティアラート基準に基づいて、自動ロールバックトリガーを備えた段階的なロールアウトメカニズム (カナリアデプロイメント、ブルーグリーンデプロイメント) を実装している。 | 1 | D |
| **3.3.2** | **検証:** ロールバック機能は、事前に定義された組織の時間枠内で、完全なモデル状態 (重み、構成、依存関係) をアトミックに復元している。 | 1 | D/V |
| **3.3.3** | **検証:** デプロイメントプロセスは、モデルアクティベーション前に暗号署名を検証し、完全性チェックサムを計算して、不一致がある場合はデプロイメントを失敗している。 | 2 | D/V |
| **3.3.4** | **検証:** 緊急モデルシャットダウン機能は、自動回路ブレーカーまたは手動キルスイッチを介して、事前に定義された応答時間内にモデルエンドポイントを無効にすることが可能である。 | 2 | D/V |
| **3.3.5** | **検証:** ロールバックアーティファクト (以前のモデルバージョン、構成、依存関係) は、インシデント対応用の不変ストレージで、組織のポリシーに従って維持されている。 | 2 | V |

---

## C3.4 変更の説明責任と監査 (Change Accountability & Audit)

すべてのモデルライフサイクルの変更は追跡可能かつ監査可能である必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.4.1** | **検証:** すべてのモデル変更 (デプロイメント、構成、廃止) は、タイムスタンプ、認証されたアクターのアイデンティティ、変更タイプ、前後のステータスを含む不変の監査レコードを生成します。 | 1 | V |
| **3.4.2** | **検証:** 監査ログへのアクセスは適切な認可を必要とし、すべてのアクセス試行はユーザーアイデンティティとタイムスタンプとともにログ記録されている。 | 2 | D/V |
| **3.4.3** | **検証:** プロンプトテンプレートとシステムメッセージは Git リポジトリでバージョン管理されており、デプロイメント前に指名されたレビュー担当者によるコードレビューと承認を必須としている。 | 2 | D/V |
| **3.4.4** | **検証:** 監査レコードは、保持期間内の任意のタイムスタンプに対するモデル状態を完全に再構築できるように、十分な詳細 (モデルハッシュ、構成スナップショット、依存関係バージョン) を含んでいる。 | 2 | V |

---

## C3.5 セキュア開発プラクティス (Secure Development Practices)

モデルの開発とトレーニングのプロセスは、侵害を防ぐために、セキュアプラクティスに従う必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **3.5.1** | **検証:** モデル開発、テスト、本番の環境は物理的または論理的に分離されている。共有インフラストラクチャを持たず、独自のアクセス制御を備え、データストアは隔離されている。 | 1 | D |
| **3.5.2** | **検証:** モデルのトレーニングとファインチューニングは、制御されたネットワークアクセスでの隔離された環境で行われている。 | 1 | D |
| **3.5.3** | **検証:** トレーニングデータソースは、モデル開発で使用する前に、完全性チェックにより検証され、文書化された保管チェーンを備えた信頼できるソースを介して認証されている。 | 1 | D/V |
| **3.5.4** | **検証:** モデル開発アーティファクト (ハイパーパラメータ、トレーニングスクリプト、構成ファイル) はバージョン管理に保存され、トレーニングで使用する前にピアレビューでの承認を必要としている。 | 2 | D |

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
