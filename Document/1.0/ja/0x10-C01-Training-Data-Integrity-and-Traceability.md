# C1 トレーニングデータ完全性とトレーサビリティ (Training Data Integrity & Traceability)

## 管理目標

この章では、オリジンのトレーサビリティ、完全性、品質を保持する方法でトレーニングデータの入手、取り扱い、維持を取り扱います。主要なセキュリティ上の懸念は、データが改竄、汚染、破損されていないことを確保することです。

---

## C1.1 トレーニングデータのオリジンとデータセキュリティ (Training Data Origin & Data Security)

Training data origin and data security are critical to the security and trustworthiness of any AI system. Datasets must be sourced from verifiable origins and tracked across their full lifecycle so that tampering or unauthorized modification can be detected. Training data must be protected against tampering, corruption, and poisoning throughout its lifecycle.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.1.1** | **Verify that** training data includes only features, attributes, and fields required for the model's stated purpose. | 1 |
| **1.1.2** | **検証:** 最新インベントリは、出所、責任者、ライセンス、収集方法、使用目的の制約、処理履歴などの、すべてのトレーニングデータソースを維持している。 | 2 |
| **1.1.3** | **Verify that** datasets are watermarked so their use can be attributed and any unauthorized use detected. | 3 |
| **1.1.4** | **Verify that** data integrity is provided when training data is stored and transferred. | 2 |
| **1.1.5** | **Verify that** integrity monitoring is applied to guard against unauthorized modifications or corruption of training data. | 2 |

---

## C1.2 データラベリングとアノテーションのセキュリティ (Data Labeling and Annotation Security)

Labeling and annotation processes must be protected against unauthorized modification, data leakage, and integrity compromise. Annotation platforms should enforce access control, preserve auditability, and protect labeling artifacts and sensitive label content throughout the training pipeline.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.2.1** | **検証:** ラベリングプラットフォームは、誰がアノテーションを作成、変更、または承認できるかを制限する、アクセス制御を強制している。 | 1 |
| **1.2.2** | **Verify that** cryptographic integrity is applied to labeling artifacts. | 2 |
| **1.2.3** | **Verify that** sensitive information in labels is redacted, anonymized, or encrypted before being used in any labeling artifact. | 2 |

---

## C1.3 トレーニングデータの品質とセキュリティ保証 (Training Data Quality and Security Assurance)

Training data quality and security assurance controls help detect corruption, poisoning, labeling errors, and exploitable dataset patterns before they affect model behavior. Pipelines should combine automated validation, poisoning detection, label quality checks, and bias analysis.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.3.1** | **検証:** トレーニングとファインチューニングのパイプラインは、潜在的なデータポイズニングやトレーニングデータ内の意図しない破損を識別するために、ポイズニング検出技法を実装している。 | 2 |
| **1.3.2** | **検証:** 自動的に生成されたラベルは、誤解を招くラベルや信頼性の低いラベルを検出するために、信頼性閾値と一貫性チェックの対象としている。 | 2 |
| **1.3.3** | **検証:** セキュリティ関連の判断に使用されるモデルはバイアスパターンについて評価されている。 | 2 |
| **1.3.4** | **Verify that** disallowed content is detected and removed before training. | 2 |
| **1.3.5** | **検証:** クリーンラベルポイズニング攻撃に対する防御が実装されている。 | 3 |

---

## 参考情報

* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [EU AI Act: Article 10: Data & Data Governance](https://artificialintelligenceact.eu/article/10/)
* [CISA Advisory: Securing Data for AI Systems](https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-142a)
* [MITRE ATLAS: Poison Training Data (AML.T0020)](https://atlas.mitre.org/techniques/AML.T0020)
* [ISO/IEC 42001:2023 Artificial Intelligence Management System](https://www.iso.org/standard/42001)
