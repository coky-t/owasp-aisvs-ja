# C12 プライバシー保護と個人データ管理 (Privacy Protection & Personal Data Management)

## 管理目標

AI ライフサイクル全体 (収集、トレーニング、推論、インシデント対応) にわたって厳格なプライバシー保証を維持し、個人データが明確な同意、必要最小限の範囲、証明可能な消去、形式プライバシー保証でのみ処理されるようにします。 This chapter focuses on AI-specific privacy concerns: privacy properties of training data and derived model artifacts, deletion and unlearning across ML artifacts, differential privacy budget management for training, purpose binding for datasets and models, consent-aware inference gating, and federated-learning privacy controls.

Generic data protection controls (sensitive data classification, encryption at rest and in transit, retention scheduling, secure deletion of conventional storage, audit log immutability, and the existence and operation of a Consent Management Platform or equivalent record of opt-in, purpose, and retention metadata) are covered by ASVS v5 V14 and V16 and are not repeated here.

---

## C12.1 匿名化とデータ最小化 (Anonymization & Data Minimization)

Remove or transform personal identifiers before training to prevent re-identification and minimize privacy exposure.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.1.1** | **Verify that** direct and quasi-identifiers in training and fine-tuning datasets are removed, hashed, or generalized before the data is used to fit or update a model. | 1 |
| **12.1.2** | **検証:** 自動監査はトレーニングデータセットにおける k-匿名性または l-多様性を測定し、閾値がポリシーを下回ると警告を発している。 | 2 |
| **12.1.3** | **検証:** モデルの特徴量重要度レポートは ε = 0.01 相互情報量を超える識別子の漏洩がないことを証明している。 | 2 |
| **12.1.4** | **検証:** 形式証明や合成データ証明はリンク攻撃の下でも再識別リスクが 0.05 以下であることを示している。 | 3 |

---

## C12.2 忘れられる権利と削除の強制 (Right-to-be-Forgotten & Deletion Enforcement)

Ensure data-subject deletion requests propagate across all AI artifacts and that model unlearning is verifiable.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.2.1** | **Verify that** data-subject deletion requests propagate to AI-derived artifacts including training and fine-tuning datasets, model checkpoints, evaluation sets, derived caches, and feature stores within a service level agreement of less than 30 days. Embedding and RAG index propagation is governed by C8.3. | 1 |
| **12.2.2** | **検証:** 「機械学習解除 (machine-unlearning)」ルーチンは、認定された学習解除アルゴリズムを使用して、物理的に再訓練するか、削除を近似している。 | 2 |
| **12.2.3** | **検証:** シャドウモデルの評価は忘れられたレコードが与える影響が学習解除後の出力の 1% 未満であることを証明している。 | 2 |

---

## C12.3 差分プライバシーの安全対策 (Differential-Privacy Safeguards)

Track and enforce privacy budgets to provide formal guarantees against individual data leakage.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.3.1** | **Verify that** differential privacy budget consumption (both ε and δ values) is tracked and recorded per training round, and that cumulative consumption triggers an alert when ε exceeds defined policy thresholds. | 2 |
| **12.3.2** | **検証:** ブラックボックスプライバシー監査は宣言値の 10% 以内で ε̂ を推定している。 | 2 |
| **12.3.3** | **検証:** 形式証明はトレーニング後のすべてのファインチューンとエンベディングをカバーしている。 | 3 |

---

## C12.4 目的の制限とスコープクリープの保護 (Purpose-Limitation & Scope-Creep Protection)

Prevent models and datasets from being used beyond their originally consented purpose.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.4.1** | **検証:** すべてのデータセットとモデルのチェックポイントは元の同意に沿った機械読み取り可能な目的タグを伝達している。 | 1 |
| **12.4.2** | **検証:** ランタイムモニターは、データセットまたはモデルの宣言された目的と一致しないクエリを検出し、検出されたクエリはソフト拒否をトリガーするか、レビュー待ちでブロックされている。 | 1 |
| **12.4.3** | **検証:** Policy as Code ゲートは DPIA レビューなしでの新しいドメインへのモデルのデプロイメントをブロックしている。 | 3 |
| **12.4.4** | **検証:** 形式トレーサビリティ証明はすべての個人データのライフサイクルが同意された範囲内にとどまっていることを示している。 | 3 |

---

## C12.5 同意管理と法的根拠の追跡 (Consent Management & Lawful-Basis Tracking)

Enforce consent at AI-specific decision points (training data ingestion, inference) and propagate withdrawal across AI artifacts.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.5.1** | **Verify that** models validate consent token scope before inference and refuse processing when the token is absent, invalid, or does not cover the requested operation. | 2 |
| **12.5.2** | **検証:** 拒否または撤回された同意は 24 時間以内に処理パイプラインを停止している。 | 2 |

---

## C12.6 プライバシー制御を備えた連合学習 (Federated Learning with Privacy Controls)

Apply differential privacy and privacy auditing to federated learning to protect individual participant data. Robust aggregation and anti-poisoning aggregator selection (e.g., Krum, Trimmed-Mean) are integrity controls and are covered by C1 (training data integrity) and C11 (adversarial robustness).

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **12.6.1** | **検証:** クライアントの更新は集約前にローカル差分プライバシーノイズの追加を採用している。 | 1 |
| **12.6.2** | **検証:** トレーニングメトリクスは差分プライベートであり、決して単一クライアントの損失を明らかにしていない。 | 2 |
| **12.6.3** | **Verify that** federated learning systems implement canary-based privacy auditing to empirically bound privacy leakage, with audit results logged and reviewed per training cycle. | 3 |
| **12.6.4** | **検証:** 形式証明は全体的な ε 予算が 5 未満の効用損失であることを示している。 | 3 |

---

### 参考情報

* [OWASP LLM02:2025 Sensitive Information Disclosure](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/)
* [General Data Protection Regulation (GDPR)](https://gdpr-info.eu/)
* [California Consumer Privacy Act (CCPA)](https://oag.ca.gov/privacy/ccpa)
* [EU Artificial Intelligence Act](https://artificialintelligenceact.eu/)
* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
