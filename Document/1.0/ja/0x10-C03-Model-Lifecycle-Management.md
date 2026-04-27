# C3 モデルライフサイクル管理と変更管理 (Model Lifecycle Management & Change Control)

## 管理目標

AI システムは、不正または安全でないモデル変更が本番環境に到達することを防ぐ変更管理プロセスを実装する必要があります。これらの管理は、開発からデプロイメントを通じて廃止に至るまでのライフサイクル全体を通じてモデルの完全性を確保して、迅速なインシデント対応を可能にし、すべての変更に対する説明責任を維持します。

**主要なセキュリティ目標:** 完全性、追跡可能性、回復可能性を維持する管理プロセスを採用することで、認可され、検証されたモデルのみが本番環境に到達します。

---

## C3.1 モデル認可と完全性 (Model Authorization & Integrity)

完全性を検証され、認可されたモデルのみが本番環境に到達します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.1.1** | **検証:** モデルレジストリはデプロイされたすべてのモデルアーティファクトのインベントリを維持し、機械可読なモデル/AI 部品表 (MBOM/AIBOM) (SPDX や CycloneDX など) を生成している。 | 1 |
| **3.1.2** | **検証:** すべてのモデルアーティファクト (重み、構成、トークナイザ、ベースモデル、ファインチューン、アダプタ、安全性/ポリシーモデル) は認可されたエンティティで暗号署名されている。 | 1 |
| **3.1.3** | **検証:** モデルアーティファクトの署名と完全性チェックサムは、デプロイメント承認時とロード時に検証され、署名されていないアーティファクト、改竄されたアーティファクト、一致しないアーティファクトは拒否されている。 | 1 |
| **3.1.4** | **検証:** リネージと依存関係の追跡は、環境 (開発、ステージング、本番など) ごとにすべての消費サービスとエージェントの識別を可能にする、依存関係グラフを維持している。 | 3 |
| **3.1.5** | **検証:** モデルオリジンの完全性とトレースのレコードは、認可エンティティのアイデンティティ、トレーニングデータのチェックサム、合格/不合格ステータスでのバリデーションテスト結果、署名のフィンガープリント/証明書チェーン ID、作成タイムスタンプ、承認されたデプロイメント環境を含んでいる。 | 3 |

---

## C3.2 モデルバリデーションとテスト (Model Validation & Testing)

モデルは、定義されたセキュリティと安全性のバリデーションにデプロイメント前に合格する必要があります。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.2.1** | **検証:** モデルは、デプロイメント前に、自動入力バリデーションテストを受けている。 | 1 |
| **3.2.2** | **検証:** モデルは、デプロイメント前に、自動出力サニタイゼーションテストを受けている。 | 1 |
| **3.2.3** | **検証:** モデルは、デプロイメント前に、定義された合格/不合格の閾値での安全性評価を受けている。 | 1 |
| **3.2.4** | **検証:** セキュリティテストは、バージョン管理された評価ハーネスを使用して、エージェントワークフロー、ツールと MCP の統合、RAG とメモリのインタラクション、マルチモーダル入力、ガードレール (安全性モデルまたは検出サービス) をカバーしている。 | 2 |
| **3.2.5** | **検証:** すべてのモデル変更 (デプロイメント、構成、廃止) は、タイムスタンプ、認証されたアクターのアイデンティティ、変更タイプ、前後のステータス、トレースメタデータ (環境と消費サービス/エージェント)、モデル識別子 (バージョン/ダイジェスト/署名) を含む不変の監査レコードを生成している。 | 2 |
| **3.2.6** | **検証:** バリデーションの失敗は、事前に指名された権限のある担当者が文書化されたビジネス上の正当性とともに明示的にオーバーライドを承認しない限り、モデルのデプロイメントを自動的にブロックしている。 | 3 |
| **3.2.7** | **Verify that** models subjected to post-training quantization, pruning, or distillation are re-evaluated against the same safety and alignment test suite on the compressed artifact before deployment, and that evaluation results are retained as distinct records linked to the compressed artifact's version or digest. | 2 |

---

## C3.3 制御されたデプロイメントとロールバック (Controlled Deployment & Rollback)

モデルデプロイメントは制御され、監視され、元に戻すことができる必要があります。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.3.1** | **検証:** 本番環境デプロイメントは、事前に合意されたエラー率、レイテンシ閾値、ガードレールアラート、またはツール/MCP 障害率に基づいて、自動ロールバックトリガーを備えた段階的なロールアウトメカニズム (カナリアデプロイメントやブルーグリーンデプロイメントなど) を実装している。 | 2 |
| **3.3.2** | **検証:** ロールバック機能は完全なモデル状態 (重み、構成、アダプタや安全性/ポリシーモデルなどの依存関係) をアトミックに復元している。 | 2 |
| **3.3.3** | **検証:** 緊急モデルシャットダウン機能は、事前に定義された応答時間内に、モデルエンドポイントを無効にできる。 | 3 |
| **3.3.4** | **検証:** 緊急シャットダウンは、エージェントツールと MCP アクセス、RAG コネクタ、データベースと API のクレデンシャル、メモリストアバインディングを非アクティブ化することなど、システムのすべての部分にカスケードしている。 | 3 |
| **3.3.5** | **Verify that** model versions running in parallel (e.g., A/B tests, canary, shadow deployments) use isolated runtime state so that AI-specific shared resources (e.g., KV caches, prompt caches, session state, retrieval indices) are not shared across deployment cohorts. | 2 |

---

## C3.4 セキュア開発プラクティス (Secure Development Practices)

モデルの開発とトレーニングのプロセスは、侵害を防ぐために、セキュアプラクティスに従う必要があります。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.4.1** | **Verify that** AI-specific runtime components (agent orchestration services, tool/MCP servers, model registries, and prompt/policy stores) are not shared across environment boundaries (e.g., development, staging, production). | 1 |
| **3.4.2** | **Verify that** AI-specific configuration artifacts (prompt templates, agent policies and routing graphs, tool or MCP contracts and schemas, and action catalogs or capability allow-lists) are stored in version control with change history and require approved review before deployment. | 1 |
| **3.4.3** | **Verify that** model training and fine-tuning environments are isolated from production model endpoints, agent orchestration services, tool/MCP servers, and live RAG data sources. | 2 |

---

## C3.5 Hosted and Provider-Managed Model Controls

Hosted and provider-managed models may change behavior without notice. These controls help ensure visibility, reassessment, and safe operation when the organization does not control the model weights.

| # | Description | Level |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.5.1** | **Verify that** hosted model dependencies are inventoried with provider, endpoint, provider-exposed model identifier, version or release identifier when available, and fallback or routing relationships. | 1 |
| **3.5.2** | **Verify that** provider model, version, or routing changes trigger security re-evaluation before continued use in high-risk workflows. | 2 |
| **3.5.3** | **Verify that** logs record the exact hosted model identifier returned by the provider, or explicitly record that no such identifier was exposed. | 2 |
| **3.5.4** | **Verify that** high-assurance deployments fail closed or require explicit approval when the provider does not expose sufficient model identity or change notification information for verification. | 3 |

---

## C3.6 Fine-Tuning Pipeline Authorization & Reward Model Integrity

Fine-tuning pipelines are high-privilege operations that can alter deployed model behavior at scale. Multi-stage pipelines compound this risk because a compromise at any intermediate stage produces a subtly altered artifact that subsequent stages accept. Reward models used in RLHF are ML artifacts subject to tampering yet often treated as static infrastructure rather than versioned, validated components.

| # | Description | Level |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.6.1** | **Verify that** initiating a fine-tuning or retraining run requires authorization from a person who did not request the run (separation of duties). | 3 |
| **3.6.2** | **Verify that** reward models used in RLHF fine-tuning are versioned, cryptographically signed, and integrity-verified before use in a training run. | 2 |
| **3.6.3** | **Verify that** RLHF training stages include automated detection of reward hacking or reward model over-optimization (e.g., held-out human-preference probe sets, divergence thresholds, or KL penalty monitoring), with the run blocked from promotion if detection thresholds are exceeded. | 3 |
| **3.6.4** | **Verify that** in multi-stage fine-tuning pipelines, each stage's output is integrity-verified before the next stage consumes it, and intermediate checkpoints are registered as distinct artifacts enabling per-stage rollback. | 3 |

---

## 参考情報

* [MITRE ATLAS](https://atlas.mitre.org/)
* [OWASP AI Testing Guide](https://owasp.org/www-project-ai-testing-guide/)
* [MLOps Principles](https://ml-ops.org/content/mlops-principles)
* [Reinforcement fine-tuning](https://platform.openai.com/docs/guides/reinforcement-fine-tuning)
* [What is AI adversarial robustness?: IBM Research](https://research.ibm.com/blog/securing-ai-workflows-with-adversarial-robustness)
