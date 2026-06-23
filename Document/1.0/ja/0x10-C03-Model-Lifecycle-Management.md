# C3 モデルライフサイクル管理と変更管理 (Model Lifecycle Management & Change Control)

## 管理目標

This chapter addresses control of model changes so that unauthorized or unsafe modifications cannot reach production.

---

## C3.1 モデル認可と完全性 (Model Authorization & Integrity)

完全性を検証され、認可されたモデルのみが本番環境に到達する必要があります。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.1.1** | **検証:** モデルレジストリはデプロイされたすべてのモデルアーティファクトのインベントリとそれらのオリジンを維持している。 | 1 |
| **3.1.2** | **検証:** すべてのモデルアーティファクト (重み、構成、トークナイザ、ベースモデル、ファインチューン、アダプタ、安全性/ポリシーモデル) は認可されたエンティティで暗号署名されている。 | 2 |
| **3.1.3** | **検証:** モデルの暗号署名はデプロイメント承認時とロード時に検証されている。 | 2 |

---

## C3.2 モデルバリデーションとテスト (Model Validation & Testing)

モデルは、定義されたセキュリティと安全性のバリデーションにデプロイメント前に合格する必要があります。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.2.1** | **検証:** モデルは、自動入力バリデーションテスト、安全性評価テスト、出力サニタイゼーションテストをデプロイメント前に受けている。 | 1 |
| **3.2.2** | **Verify that** models subjected to post-training quantization are re-evaluated against the same safety and alignment test suite on the compressed artifact before deployment. | 2 |
| **3.2.3** | **Verify that** provider model, version, or routing changes trigger security re-evaluation before continued use. | 3 |

---

## C3.3 制御されたデプロイメントとロールバック (Controlled Deployment & Rollback)

ライフサイクル管理をサポートするため、モデルデプロイメントは制御され、監視され、元に戻すことができる必要があります。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.3.1** | **検証:** 本番環境デプロイメントは自動ロールバックトリガーを備えたロールアウトメカニズムを実装している。 | 2 |
| **3.3.2** | **検証:** ロールバック機能は完全なモデル状態を復元している。 | 2 |
| **3.3.3** | **Verify that** model versions running in parallel use isolated runtime state so that AI-specific shared resources are not shared across deployments. | 2 |

---

## C3.4 セキュア開発プラクティス (Secure Development Practices)

Model development environments must be separated from production environments.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.4.1** | **Verify that** AI-specific runtime components are not shared across environment boundaries (e.g., development, staging, production). | 1 |
| **3.4.2** | **Verify that** model training and fine-tuning environments are isolated from production environments. | 2 |

---

## C3.5 Pipeline Fine-Tuning

Fine-tuning pipelines are high-privilege operations that can alter deployed model behavior at scale. Multi-stage pipelines compound this risk because a compromise at any intermediate stage produces a subtly altered artifact that subsequent stages accept.

| # | Description | Level |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **3.5.1** | **Verify that** models used in RLHF fine-tuning are versioned and integrity-verified before use in a training run. | 2 |
| **3.5.2** | **Verify that** RLHF training stages include automated detection of reward hacking or reward model over-optimization. | 3 |
| **3.5.3** | **Verify that** in multi-stage fine-tuning pipelines, each stage's output is integrity-verified before it is consumed by the next stage. | 3 |
| **3.5.4** | **Verify that** fine-tuning checkpoints are registered as distinct artifacts. | 3 |

---

## 参考情報

* [MITRE ATLAS](https://atlas.mitre.org/)
* [OWASP AI Testing Guide](https://owasp.org/www-project-ai-testing-guide/)
* [NIST SP 800-218A: Secure Software Development Practices for Generative AI](https://csrc.nist.gov/pubs/sp/800/218/a/final)
* [ISO/IEC 42001:2023 Artificial Intelligence Management System](https://www.iso.org/standard/42001)
* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
