# C1 トレーニングデータガバナンスとバイアス管理 (Training Data Governance & Bias Management)

## 管理目標

トレーニングデータは、来歴、セキュリティ、品質、公平性を保持する方法で、調達、処理、維持する必要があります。そうすることで、法的義務を果たし、トレーニングの中で現れるバイアス、ポイズニング、プライバシー侵害のリスクを低減し、AI ライフサイクル全体に効果をもたらす可能性があります。

---

## C1.1 トレーニングデータの来歴 (Training Data Provenance)

すべてのデータセットの検証可能なインベントリを維持し、信頼できるソースのみを受け入れ、監査可能なようにすべての変更をログ記録します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.1.1** | **検証:** すべてのトレーニングデータソースの最新インベントリ (出所、管理者/所有者、ライセンス、収集方法、使用目的の制約、処理履歴) を維持している。 | 1 | D/V |
| **1.1.2** | **Verify that** training data processes exclude unnecessary features, attributes, or fields (e.g., unused metadata, sensitive PII, leaked test data). | 1 | D/V |
| **1.1.3** | **検証:** すべてのデータセットの変更はログ記録される承認ワークフローの対象となっている。 | 2 | D/V |
| **1.1.4** | **Verify that** datasets or subsets are watermarked or fingerprinted where feasible. | 3 | D/V |

---

## C1.2 トレーニングデータのセキュリティと完全性 (Training Data Security & Integrity)

トレーニングデータへのアクセスを制限し、保存時と転送時にそれを暗号化し、その完全性を検証して、改竄、窃取、データポイズニングを防止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.2.1** | **検証:** アクセス制御はトレーニングデータのストレージとパイプラインを保護している。 | 1 | D/V |
| **1.2.2** | **Verify that** all access to training data is logged, including user, time, and action. | 2 | D/V |
| **1.2.3** | **検証:** トレーニングデータセットは転送時と保存時に、業界標準の暗号アルゴリズムと鍵管理手法を使用して暗号化されている。 | 2 | D/V |
| **1.2.4** | **検証:** 暗号化ハッシュまたはデジタル署名を使用して、トレーニングデータの保存時および転送時のデータ完全性を確保している。 | 2 | D/V |
| **1.2.5** | **検証:** 自動化された検出技法を適用して、トレーニングデータの不正な変更や破損から保護している。 | 2 | D/V |
| **1.2.6** | **Verify that** obsolete training data is securely purged or anonymized. | 2 | D/V |
| **1.2.7** | **Verify that** all training dataset versions are uniquely identified, stored immutably, and auditable to support rollback and forensic analysis. | 3 | D/V |

---

## C1.3 トレーニングデータラベリングの品質、完全性、セキュリティ (Training Data Labeling Quality, Integrity, and Security)

ラベルを保護し、重要なデータについては技術的なレビューを要求します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.3.1** | **検証:** 暗号化ハッシュまたはデジタル署名がアーティファクトのラベル付けに適用され、完全性と真正性を確保している。 | 2 | D/V |
| **1.3.2** | **検証:** ラベリングインタフェースとプラットフォームは強力なアクセス制御を実施し、すべてのラベリングアクティビティの改竄防止監査ログを維持し、不正な変更から保護している。 | 2 | D/V |
| **1.3.3** | **検証:** ラベル内の機密情報は、保存時および転送時にデータフィールドレベルで訂正、匿名化、または暗号化されている。 | 3 | D/V |

---

## C1.4 トレーニングデータの品質とセキュリティ保証 (Training Data Quality and Security Assurance)

自動バリデーション、手動スポットチェック、ログ記録された修復を組み合わせて、データセットの信頼性を保証します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.4.1** | **検証:** 自動テストは、すべての取り込みや重要なデータ変換で、フォーマットエラーやヌルを捕捉している。 | 1 | D |
| **1.4.2** | **Verify that** LLM training and fine-tuning pipelines implement poisoning detection & data integrity validation (e.g., statistical methods, outlier detection, embedding analysis) to identify potential poisoning attacks (e.g., label flipping, backdoor trigger insertion, role-switching commands, influential instance attacks) or unintentional data corruption in training data. | 2 | D/V |
| **1.4.3** | **Verify that** appropriate defenses, such as adversarial training (using generated adversarial examples), data augmentation with perturbed inputs, or robust optimization techniques, are implemented and tuned for relevant models based on risk assessment. | 3 | D/V |
| **1.4.4** | **Verify that** automatically generated labels (e.g., via LLMs or weak supervision) are subject to confidence thresholds and consistency checks to detect hallucinated, misleading, or low-confidence labels. | 2 | D/V |
| **1.4.5** | **検証:** 自動テストは、すべての取り込みや重要なデータ変換で、ラベルスキューを捕捉している。 | 3 | D |

---

## C1.5 データリネージとトレーサビリティ (Data Lineage and Traceability)

Track the full journey of each data point from source to model input for auditability and incident response.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.5.1** | **Verify that** the lineage of each data point, including all transformations, augmentations, and merges, is recorded and can be reconstructed. | 2 | D/V |
| **1.5.2** | **Verify that** lineage records are immutable, securely stored, and accessible for audits. | 2 | D/V |
| **1.5.3** | **Verify that** lineage tracking covers synthetic data generated via privacy-preserving or generative techniques and that all synthetic data is clearly labeled and distinguishable from real data throughout the pipeline. | 2 | D/V |

---

## 参考情報

* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [EU AI Act – Article 10: Data & Data Governance](https://artificialintelligenceact.eu/article/10/)
* [MITRE ATLAS: Adversary Tactics for AI](https://atlas.mitre.org/)
* [Survey of ML Bias Mitigation Techniques – MDPI](https://www.mdpi.com/2673-6470/4/1/1)
* [Data Provenance & Lineage Best Practices – Nightfall AI](https://www.nightfall.ai/ai-security-101/data-provenance-and-lineage)
* [Data Labeling Quality Standards – LabelYourData](https://labelyourdata.com/articles/data-labeling-quality-and-how-to-measure-it)
* [Training Data Poisoning Guide – Lakera.ai](https://www.lakera.ai/blog/training-data-poisoning)
* [CISA Advisory: Securing Data for AI Systems](https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-142a)
* [ISO/IEC 23053: AI Management Systems Framework](https://www.iso.org/sectors/it-technologies/ai)
* [IBM: What is AI Governance?](https://www.ibm.com/think/topics/ai-governance)
* [Google AI Principles](https://ai.google/principles/)
* [GDPR & AI Training Data – DataProtectionReport](https://www.dataprotectionreport.com/2024/08/recent-regulatory-developments-in-training-artificial-intelligence-ai-models-under-the-gdpr/)
* [Supply-Chain Security for AI Data – AppSOC](https://www.appsoc.com/blog/ai-is-the-new-frontier-of-supply-chain-security)
* [OpenAI Privacy Center – Data Deletion Controls](https://privacy.openai.com/policies?modal=take-control)
* [Adversarial ML Dataset – Kaggle](https://www.kaggle.com/datasets/cnrieiit/adversarial-machine-learning-dataset)
