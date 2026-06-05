# C1 トレーニングデータ完全性とトレーサビリティ (Training Data Integrity & Traceability)

## 管理目標

この章では、オリジンのトレーサビリティ、完全性、品質を保持する方法でトレーニングデータの入手、取り扱い、維持を取り扱います。主要なセキュリティ上の懸念は、データが改竄、汚染、破損されていないことを確保することです。

---

## C1.1 トレーニングデータのオリジンとトレーサビリティ (Training Data Origin & Traceability)

Training data origin and traceability are critical to the security and trustworthiness of any AI system. Datasets must be sourced from verifiable origins and tracked across their full lifecycle so that tampering or unauthorized modification can be detected.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.1.1** | **検証:** すべてのトレーニングデータソースの最新インベントリ (出所、責任者、ライセンス、収集方法、使用目的の制約、処理履歴) を維持している。 | 1 |
| **1.1.2** | **Verify that** training data processes include only features, attributes, and fields required for the model's stated purpose, excluding all others (e.g., unused metadata, sensitive PII, leaked test data). | 1 |
| **1.1.3** | **検証:** すべてのデータセットの変更はログ記録される承認ワークフローの対象としている。 | 1 |
| **1.1.4** | **Verify that** datasets or subsets are watermarked or fingerprinted to enable downstream attribution and detection of unauthorized use. | 3 |

---

## C1.2 トレーニングデータのセキュリティと完全性 (Training Data Security & Integrity)

Training data must be protected against tampering, corruption, and poisoning throughout its lifecycle. Generic data security controls (access control on storage, access logging, and encryption at rest and in transit) are covered by OWASP ASVS v5 (V6, V7, V8) and must be implemented as a baseline. This section addresses AI-specific concerns: integrity verification against data poisoning, immutable dataset versioning for rollback, and the residual-memorization risk that persists in trained models after training data is retired.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.2.1** | **Verify that** when training data is retired or removed, an impact assessment is documented covering all models trained on that data, the assessed residual memorization risk, and the selected mitigation (targeted fine-tuning, machine unlearning, model retraining, or documented risk acceptance). | 1 |
| **1.2.2** | **検証:** 暗号化ハッシュまたはデジタル署名を使用して、トレーニングデータの保存時および転送時のデータ完全性を確保している。 | 2 |
| **1.2.3** | **検証:** 自動化された完全性監視を適用して、トレーニングデータの不正な変更や破損から保護している。 | 2 |
| **1.2.4** | **検証:** すべてのトレーニングデータセットのバージョンは、ロールバックとフォレンジック解析をサポートするために、一意に識別され、不変に保存され、監査可能である。 | 2 |

---

## C1.3 データラベリングとアノテーションのセキュリティ (Data Labeling and Annotation Security)

Labeling and annotation processes must be protected against unauthorized modification, attribution loss, data leakage, and integrity compromise. Annotation platforms should enforce access control, preserve auditability, retain verified annotator attribution, and protect labeling artifacts, preference data, and sensitive label content throughout the training pipeline.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.3.1** | **検証:** ラベリングインタフェースとプラットフォームは、誰がアノテーションを作成、変更、または承認できるかを制限する、アクセス制御を強制している。 | 1 |
| **1.3.2** | **検証:** すべてのラベリングアクティビティは、アノテーターのアイデンティティ、タイムスタンプ、実行されたアクションを含め、監査ログに記録されている。 | 1 |
| **1.3.3** | **検証:** アノテーターのアイデンティティメタデータはデータセットとともにエクスポートおよび保持されるため、すべてのアノテーションまたはプリファレンスペアは、トレーニングパイプライン全体を通して特定の検証済みの人間のアノテーターに帰属できる。 | 2 |
| **1.3.4** | **検証:** 暗号化ハッシュまたはデジタル署名はラベリングアーティファクト、アノテーションデータ、ファインチューニングフィードバックレコード (RLHF プリファレンスペアを含む) に適用され、完全性と真正性を確保している。 | 2 |
| **1.3.5** | **Verify that** sensitive information in labels is redacted, anonymized, or encrypted both at rest and in transit before being used in any labeling artifact. | 2 |

---

## C1.4 トレーニングデータの品質とセキュリティ保証 (Training Data Quality and Security Assurance)

Training data quality and security assurance controls help detect corruption, poisoning, labeling errors, and exploitable dataset patterns before they affect model behavior. Pipelines should combine automated validation, poisoning detection, label quality checks, and bias analysis.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.4.1** | **検証:** 自動テストは、すべての取り込みや重要なデータ変換で、フォーマットエラーやヌルを捕捉している。 | 1 |
| **1.4.2** | **検証:** トレーニングとファインチューニングのパイプラインは、潜在的なデータポイズニングやトレーニングデータ内の意図しない破損を識別するために、データ完全性バリデーションとポイズニング検出技法 (統計解析、外れ値検出、エンベディング解析など) を実装している。 | 2 |
| **1.4.3** | **検証:** (モデルや弱いスーパービジョンなどを介して) 自動的に生成されたラベルは、誤解を招くラベルや信頼性の低いラベルを検出するために、信頼性閾値と一貫性チェックの対象としている。 | 2 |
| **1.4.4** | **検証:** 自動テストは、すべての取り込みや重要なデータ変換で、ラベルスキューを捕捉している。 | 2 |
| **1.4.5** | **検証:** セキュリティ関連の判断 (不正使用の検出、不正スコアリング、自動的な信頼性判断など) に使用されるモデルは、デプロイメント前および重要なモデルの更新後に、攻撃者がコントロールを回避するために悪用する可能性のある体系的なバイアスパターン (例: 信頼できる言語スタイルや人口統計パターンを模倣して検出をバイパスするなど) について評価されている。 | 2 |
| **1.4.6** | **Verify that** training-time defenses against data poisoning are selected and applied based on a documented risk assessment, with the chosen defense (e.g., adversarial training, data augmentation with perturbed inputs, or robust optimization) and its tuning rationale recorded alongside the model artifact. | 2 |
| **1.4.7** | **検証:** 信頼できない、または部分的に信頼できるトレーニングデータソースにさらされるモデルに対しては、クリーンラベルポイズニングに対する防御 (入力の精製、k-NN フィルタリング、データ分割および集約など) が実行されている。 | 3 |

---

## C1.5 データリネージとトレーサビリティ (Data Lineage and Traceability)

Data lineage and traceability controls ensure that datasets can be tracked from source through transformation, augmentation, merge, and final model input. Lineage records should be complete, tamper-resistant, auditable, and sufficient to support reproducibility, incident response, rollback, and investigation of compromised or inappropriate training data.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **1.5.1** | **検証:** すべての変換、拡張、マージを含む各データセットとそのコンポーネントのリネージは記録され、再構築できる。 | 1 |
| **1.5.2** | **検証:** リネージレコードは不変であり、安全に保存され、監査のためにアクセス可能である。 | 2 |
| **1.5.3** | **検証:** リネージ追跡は、拡張、合成、またはプライバシー保護技法を介して生成された合成データをカバーしている。 | 2 |
| **1.5.4** | **検証:** 合成データは明確にラベル付けされ、パイプライン全体を通して実際のデータと区別可能である。 | 2 |

---

## 参考情報

* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [EU AI Act: Article 10: Data & Data Governance](https://artificialintelligenceact.eu/article/10/)
* [CISA Advisory: Securing Data for AI Systems](https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-142a)
* [OpenAI Privacy Center: Data Deletion Controls](https://privacy.openai.com/policies?modal=take-control)
