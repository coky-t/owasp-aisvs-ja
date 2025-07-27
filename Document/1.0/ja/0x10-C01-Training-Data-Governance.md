# C1 トレーニングデータガバナンスとバイアス管理 (Training Data Governance & Bias Management)

## 管理目標

トレーニングデータは、来歴、セキュリティ、品質、公平性を保持する方法で、調達、処理、維持する必要があります。そうすることで、法的義務を果たし、トレーニングの中で現れるバイアス、ポイズニング、プライバシー侵害のリスクを低減し、AI ライフサイクル全体に効果をもたらす可能性があります。

---

## C1.1 トレーニングデータの来歴 (Training Data Provenance)

すべてのデータセットの検証可能なインベントリを維持し、信頼できるソースのみを受け入れ、監査可能なようにすべての変更をログ記録します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.1.1** | **検証:** すべてのトレーニングデータソースの最新インベントリ (出所、管理者/所有者、ライセンス、収集方法、使用目的の制約、処理履歴) を維持している。 | 1 | D/V |
| **1.1.2** | **検証:** 品質、代表性、倫理的調達、ライセンス遵守について精査されたデータセットのみを許可し、ポイズニング、埋め込まれたバイアス、知的財産侵害のリスクを低減している。 | 1 | D/V |
| **1.1.3** | **検証:** データ最小化を実施し、不要な属性や機密性の高い属性を除外している。 | 1 | D/V |
| **1.1.4** | **検証:** すべてのデータセットの変更はログ記録される承認ワークフローの対象となっている。 | 2 | D/V |
| **1.1.5** | **検証:** ラベル付けや注釈の品質はレビュー担当者のクロスチェックまたはコンセンサスによって確保している。 | 2 | D/V |
| **1.1.6** | **検証:** 重要なトレーニングデータセットに対して「データカード」や「データセットのデータシート」を維持し、特性、動機、構成、収集プロセス、前処理、推奨される使用方法、推奨されない使用方法を詳述している。 | 2 | D/V |

---

## C1.2 トレーニングデータのセキュリティと完全性 (Training Data Security & Integrity)

アクセスを制限し、保存時と転送時に暗号化し、完全性を検証して、トレーニングデータの改竄や窃取をブロックします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.2.1** | **検証:** アクセス制御はストレージとパイプラインを保護している。 | 1 | D/V |
| **1.2.2** | **検証:** データセットは転送時に暗号化され、すべての機密情報や個人を識別できる情報 (PII) については保存時に、業界標準の暗号アルゴリズムと鍵管理手法を使用して暗号化されている。 | 2 | D/V |
| **1.2.3** | **検証:** 暗号化ハッシュまたはデジタル署名を使用して、保存および転送時のデータ完全性を確保し、自動化された異常検出技法を適用して、標的型データポイズニング攻撃など不正な変更や破損から保護している。 | 2 | D/V |
| **1.2.4** | **検証:** データセットバージョンは追跡され、ロールバックを可能にしている。 | 3 | D/V |
| **1.2.5** | **検証:** 古いデータは、データ保持ポリシー、規制要件に準拠して、安全に削除または匿名化され、攻撃対象領域を縮小している。 | 2 | D/V |

---

## C1.3 表現の完全性と公平性 (Representation Completeness & Fairness)

人口統計上のバイアスを検出し、緩和策を適用して、モデルのエラー率がグループ間で公平にします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.3.1** | **検証:** データセットは、法的に保護された属性 (人種、性別、年齢など) とモデルの適用ドメインに関連するその他の倫理的にセンシティブな特定 (社会経済的ステータス、位置情報など) にわたって、表現の不均衡と潜在的なバイアスについてプロファイルされている。 | 1 | D/V |
| **1.3.2** | **検証:** 特定されたバイアスは、再バランス調整、対象を絞ったデータ拡張、アルゴリズム調整 (前処理、中処理、後処理技法など)、再重み付けなどの文書化された戦略によって緩和され、緩和策が公平性と全体的なモデルパフォーマンスの両方に与える影響を評価している。 | 2 | D/V |
| **1.3.3** | **検証:** トレーニング後の公平性の指標が評価され、文書化されている。 | 2 | D/V |
| **1.3.4** | **検証:** ライフサイクルバイアス管理ポリシーは所有者とレビュー頻度を割り当てている。 | 3 | D/V |

---

## C1.4 トレーニングデータラベリングの品質、完全性、セキュリティ (Training Data Labeling Quality, Integrity, and Security)

ラベルを保護し、重要なデータについては技術的なレビューを要求します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.4.1** | **検証:** ラベル付けや注釈の品質は、明確なガイドライン、レビュー担当者によるクロスチェック、同意メカニズム (注釈者間の合意の監視など)、不整合を解決するために定義されたプロセスによって確保されている。 | 2 | D/V |
| **1.4.2** | **検証:** 暗号化ハッシュまたはデジタル署名がアーティファクトのラベル付けに適用され、完全性と真正性を確保している。 | 2 | D/V |
| **1.4.3** | **検証:** ラベリングインタフェースとプラットフォームは強力なアクセス制御を実施し、すべてのラベリングアクティビティの改竄防止監査ログを維持し、不正な変更から保護している。 | 2 | D/V |
| **1.4.4** | **検証:** 安全性、セキュリティ、公平性にとって重要なラベル (有毒コンテンツの特定、重要な医療所見など) は、独立した二重レビューまたは同等の堅牢な検証を必ず受けている。 | 3 | D/V |
| **1.4.5** | **検証:** 誤って取得されたり、ラベルに必然的に存在する機密情報 (PII など) は、データ最小化の原則に従って、保存時および転送時に訂正、仮名化、匿名化、または暗号化されている。 | 2 | D/V |
| **1.4.6** | **検証:** ラベリングガイドとインストラクションは包括的であり、バージョン管理され、ピアレビューされている。 | 2 | D/V |
| **1.4.7** | **検証:** データスキーマ (ラベルに対するものを含む) は明確に定義され、バージョン管理されている。 | 2 | D/V |
| **1.4.8** | **検証:** アウトソースまたはクラウドソースしたラベリングワークフローは技術的/手順的な保護策を含み、データの機密性、完全性、ラベルの品質を確保し、データ漏洩を防いでいる。 | 2 | D/V |

---

## C1.5 トレーニングデータの品質保証 (Training Data Quality Assurance)

Combine automated validation, manual spot-checks, and logged remediation to guarantee dataset reliability.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.5.1** | **Verify that** automated tests catch format errors, nulls, and label skews on every ingest or significant transformation. | 1 | D |
| **1.5.2** | **Verify that** failed datasets are quarantined with audit trails. | 1 | D/V |
| **1.5.3** | **Verify that** manual spot-checks by domain experts cover a statistically significant sample (e.g., ≥1% or 1,000 samples, whichever is greater, or as determined by risk assessment) to identify subtle quality issues not caught by automation. | 2 | V |
| **1.5.4** | **Verify that** remediation steps are appended to provenance records. | 2 | D/V |
| **1.5.5** | **Verify that** quality gates block sub-par datasets unless exceptions are approved. | 2 | D/V |

---

## C1.6 データポイズニングの検出 (Data Poisoning Detection)

Apply statistical anomaly detection and quarantine workflows to stop adversarial insertions.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.6.1** | **Verify that** anomaly detection techniques (e.g., statistical methods, outlier detection, embedding analysis) are applied to training data at ingest and before major training runs to identify potential poisoning attacks or unintentional data corruption. | 2 | D/V |
| **1.6.2** | **Verify that** flagged samples trigger manual review before training. | 2 | D/V |
| **1.6.3** | **Verify that** results feed the model's security dossier and inform ongoing threat intelligence. | 2 | V |
| **1.6.4** | **Verify that** detection logic is refreshed with new threat intel. | 3 | D/V |
| **1.6.5** | **Verify that** online-learning pipelines monitor distribution drift. | 3 | D/V |
| **1.6.6** | **Verify that** specific defenses against known data poisoning attack types (e.g., label flipping, backdoor trigger insertion, influential instance attacks) are considered and implemented based on the system's risk profile and data sources. | 3 | D/V |
| **1.6.7** | **Verify that** LLM training and fine-tuning pipelines implement controls to detect and mitigate adversarial free-text prompt contamination risks (such as embedded jailbreak instructions, role-switching commands, universal triggers), using a combination of automated detection techniques and manual review. | 3 | D/V |


---

## C1.7 ユーザーデータの削除と同意の実施 (User Data Deletion & Consent Enforcement)

Honor deletion and consent-withdrawal requests across datasets, backups, and derived artifacts.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.7.1** | **Verify that** deletion workflows purge primary and derived data and assess model impact, and that the impact on affected models is assessed and, if necessary, addressed (e.g., through retraining or recalibration). | 1 | D/V |
| **1.7.2** | **Verify that** mechanisms are in place to track and respect the scope and status of user consent (and withdrawals) for data used in training, and that consent is validated before data is incorporated into new training processes or significant model updates. | 2 | D |
| **1.7.3** | **Verify that** workflows are tested annually and logged. | 2 | V |

---

## C1.8 サプライチェーンのセキュリティ (Supply Chain Security)

Vet external data providers and verify integrity over authenticated, encrypted channels.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.8.1** | **Verify that** third-party data suppliers, including providers of pre-trained models and external datasets, undergo security, privacy, ethical sourcing, and data quality due diligence before their data or models are integrated.| 2 | D/V |
| **1.8.2** | **Verify that** external transfers use TLS/auth and integrity checks. | 1 | D |
| **1.8.3** | **Verify that** high-risk data sources (e.g., open-source datasets with unknown provenance, unvetted suppliers) receive enhanced scrutiny, such as sandboxed analysis, extensive quality/bias checks, and targeted poisoning detection, before use in sensitive applications.| 2 | D/V |
| **1.8.4** | **Verify that** Verify that pre-trained models obtained from third parties are evaluated for embedded biases, potential backdoors, integrity of their architecture, and the provenance of their original training data before fine-tuning or deployment. | 3 | D/V |

---

## C1.9 敵対的サンプルの検出 (Adversarial Sample Detection)

Implement measures during the training phase, such as adversarial training, to enhance model resilience against adversarial examples.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.9.1** | **Verify that** appropriate defenses, such as adversarial training (using generated adversarial examples), data augmentation with perturbed inputs, or robust optimization techniques, are implemented and tuned for relevant models based on risk assessment. | 3 | D/V |
| **1.9.2** | **Verify that** if adversarial training is used, the generation, management, and versioning of adversarial datasets are documented and controlled. | 2 | D/V |
| **1.9.3** | **Verify that** the impact of adversarial robustness training on model performance (against both clean and adversarial inputs) and fairness metrics is evaluated, documented, and monitored. | 3 | D/V |
| **1.9.4** | **Verify that** strategies for adversarial training and robustness are periodically reviewed and updated to counter evolving adversarial attack techniques.| 3 | D/V |

---

## C1.10 データリネージとトレーサビリティ (Data Lineage and Traceability)

Track the full journey of each data point from source to model input for auditability and incident response.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.10.1** | **Verify that** the lineage of each data point, including all transformations, augmentations, and merges, is recorded and can be reconstructed. | 2 | D/V |
| **1.10.2** | **Verify that** lineage records are immutable, securely stored, and accessible for audits or investigations. | 2 | D/V |
| **1.10.3** | **Verify that** lineage tracking covers both raw and synthetic data. | 2 | D/V |

---

## C1.11 合成データ管理 (Synthetic Data Management)

Ensure synthetic data is properly managed, labeled, and risk-assessed.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.11.1** | **Verify that** all synthetic data is clearly labeled and distinguishable from real data throughout the pipeline. | 2 | D/V |
| **1.11.2** | **Verify that** the generation process, parameters, and intended use of synthetic data are documented. | 2 | D/V |
| **1.11.3** | **Verify that** synthetic data is risk-assessed for bias, privacy leakage, and representational issues before use in training. | 2 | D/V |

---

## C1.12 データアクセス監視と異常検出 (Data Access Monitoring & Anomaly Detection)

Monitor and alert on unusual access to training data to detect insider threats or exfiltration.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.12.1** | **Verify that** all access to training data is logged, including user, time, and action. | 2 | D/V |
| **1.12.2** | **Verify that** access logs are regularly reviewed for unusual patterns, such as large exports or access from new locations. | 2 | D/V |
| **1.12.3** | **Verify that** alerts are generated for suspicious access events and investigated promptly. | 2 | D/V |

---

## C1.13 データ保持と有効期限のポリシー (Data Retention & Expiry Policies)

Define and enforce data retention periods to minimize unnecessary data storage.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.13.1** | **Verify that** explicit retention periods are defined for all training datasets. | 1 | D/V |
| **1.13.2** | **Verify that** datasets are automatically expired, deleted, or reviewed for deletion at the end of their lifecycle. | 2 | D/V |
| **1.13.3** | **Verify that** retention and deletion actions are logged and auditable. | 2 | D/V |

---

## C1.14 規制と管轄区域のコンプライアンス (Regulatory & Jurisdictional Compliance)

Ensure all training data complies with applicable laws and regulations.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.14.1** | **Verify that** data residency and cross-border transfer requirements are identified and enforced for all datasets. | 2 | D/V |
| **1.14.2** | **Verify that** sector-specific regulations (e.g., healthcare, finance) are identified and addressed in data handling. | 2 | D/V |
| **1.14.3** | **Verify that** compliance with relevant privacy laws (e.g., GDPR, CCPA) is documented and reviewed regularly. | 2 | D/V |

---

## C1.15 データ透かしとフィンガープリント (Data Watermarking & Fingerprinting)

Detect unauthorized reuse or leakage of proprietary or sensitive data.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.15.1** | **Verify that** datasets or subsets are watermarked or fingerprinted where feasible. | 3 | D/V |
| **1.15.2** | **Verify that** watermarking/fingerprinting methods do not introduce bias or privacy risks. | 3 | D/V |
| **1.15.3** | **Verify that** periodic checks are performed to detect unauthorized reuse or leakage of watermarked data. | 3 | D/V |

---

## C1.16 データ主体の権利管理 (Data Subject Rights Management)

Support data subject rights such as access, rectification, restriction, and objection.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.16.1** | **Verify that** mechanisms exist to respond to data subject requests for access, rectification, restriction, or objection. | 2 | D/V |
| **1.16.2** | **Verify that** requests are logged, tracked, and fulfilled within legally mandated timeframes. | 2 | D/V |
| **1.16.3** | **Verify that** data subject rights processes are tested and reviewed regularly for effectiveness. | 2 | D/V |

---

## C1.17 データセットバージョンの影響分析 (Dataset Version Impact Analysis)

Assess the impact of dataset changes before updating or replacing versions.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.17.1** | **Verify that** an impact analysis is performed before updating or replacing a dataset version, covering model performance, fairness, and compliance. | 2 | D/V |
| **1.17.2** | **Verify that** results of the impact analysis are documented and reviewed by relevant stakeholders. | 2 | D/V |
| **1.17.3** | **Verify that** rollback plans exist in case new versions introduce unacceptable risks or regressions. | 2 | D/V |

---

## C1.18 データアノテーションワークフォースのセキュリティ (Data Annotation Workforce Security)

Ensure the security and integrity of personnel involved in data annotation.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.18.1** | **Verify that** all personnel involved in data annotation are background-checked and trained in data security and privacy. | 2 | D/V |
| **1.18.2** | **Verify that** all annotation personnel sign confidentiality and non-disclosure agreements. | 2 | D/V |
| **1.18.3** | **Verify that** annotation platforms enforce access controls and monitor for insider threats. | 2 | D/V |

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
