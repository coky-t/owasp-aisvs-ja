# C13 人間による監視、説明責任、ガバナンス (Human Oversight, Accountability & Governance)

## 管理目標

この章では、AI システムでの人間の監視と明確な説明責任の連鎖を維持し、AI ライフサイクル全体にわたって説明可能性、透明性、倫理的管理責任を確保するための要件を提供します。

---

## C13.1 キルスイッチとオーバーライドメカニズム (Kill-Switch & Override Mechanisms)

AI システムの安全でない動作が観察された場合、シャットダウンまたはロールバックパスを提供します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.1.1** | **検証:** 手動キルスイッチメカニズムは、AI モデルの推論と出力を即座に停止するために、存在している。 | 1 | D/V |
| **13.1.2** | **検証:** オーバーライドコントロールは認可された担当者のみがアクセスできる。 | 1 | D |
| **13.1.3** | **検証:** ロールバック手順は以前のモデルバージョンまたはセーフモード操作に戻ることができる。 | 3 | D/V |
| **13.1.4** | **検証:** オーバーライドメカニズムは定期的にテストされている。 | 3 | V |

---

## C13.2 ヒューマンインザループの意思決定チェックポイント (Human-in-the-Loop Decision Checkpoints)

ステークスが事前定義されたリスク閾値を超える場合は人間の承認を必要とします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.2.1** | **検証:** リスクの高い AI による決定は実行前に明示的な人間の承認を必要としている。 | 1 | D/V |
| **13.2.2** | **検証:** リスク閾値は明確に定義されており、人間のレビューワークフローを自動的にトリガーしている。 | 1 | D |
| **13.2.3** | **検証:** 時間的制約のある決定は、人間の承認が必要な時間枠内に得られない場合に、フォールバック手順を用意している。 | 2 | D |
| **13.2.4** | **検証:** エスカレーション手順は、該当する場合、さまざまな意思決定タイプまたはリスクカテゴリに対して明確な権限レベルを定義している。 | 3 | D/V |

---

## C13.3 責任の連鎖と監査可能性 (Chain of Responsibility & Auditability)

オペレータのアクションとモデルの決定をログ記録します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.3.1** | **検証:** すべての AI システムの決定と人間の介入は、タイムスタンプ、ユーザーアイデンティティ、決定の根拠とともにログ記録されている。 | 1 | D/V |
| **13.3.2** | **検証:** 監査ログは改竄できず、完全性検証メカニズムを含んでいる。 | 2 | D |

---

## C13.4 説明可能な AI 技法 (Explainable-AI Techniques)

特徴量の重要度、反事実、局所的説明を表示します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.4.1** | **検証:** AI システムは人間が読み取り可能な形式でその決定について基本的な説明を提供している。 | 1 | D/V |
| **13.4.2** | **検証:** 説明の品質は人間による評価研究とメトリクスを通じて検証されている。 | 2 | V |
| **13.4.3** | **検証:** 特徴量の重要度スコアまたは帰属手法 (SHAP, LIME など) は重要な決定に利用できる。 | 3 | D/V |
| **13.4.4** | **検証:** 反事実的説明は、ユースケースとドメインに当てはまる場合、入力がどのように改変されると出力が変わるかを示している。 | 3 | V |

---

## C13.5 モデルカードと使用状況開示 (Model Cards & Usage Disclosures)

Maintain model cards for intended use, performance metrics, and ethical considerations.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.5.1** | **Verify that** model cards document intended use cases, limitations, and known failure modes. | 1 | D |
| **13.5.2** | **Verify that** performance metrics across different applicable use cases are disclosed. | 1 | D/V |
| **13.5.3** | **Verify that** ethical considerations, bias assessments, fairness evaluations, training data characteristics, and known training data limitations are documented and updated regularly. | 2 | D |
| **13.5.4** | **Verify that** model cards are version-controlled and maintained throughout the model lifecycle with change tracking. | 2 | D/V |

---

## C13.6 不確実性の定量化 (Uncertainty Quantification)

Propagate confidence scores or entropy measures in responses.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.6.1** | **Verify that** AI systems provide confidence scores or uncertainty measures with their outputs. | 1 | D |
| **13.6.2** | **Verify that** uncertainty thresholds trigger additional human review or alternative decision pathways. | 2 | D/V |
| **13.6.3** | **Verify that** uncertainty quantification methods are calibrated and validated against ground truth data. | 2 | V |
| **13.6.4** | **Verify that** uncertainty propagation is maintained through multi-step AI workflows. | 3 | D/V |

---

## C13.7 ユーザー向け透明性レポート (User-Facing Transparency Reports)

Provide periodic disclosures on incidents, drift, and data usage.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **13.7.1** | **Verify that** data usage policies and user consent management practices are clearly communicated to stakeholders. | 1 | D/V |
| **13.7.2** | **Verify that** AI impact assessments are conducted and results are included in reporting. | 2 | D/V |
| **13.7.3** | **Verify that** transparency reports published regularly disclose AI incidents and operational metrics in reasonable detail. | 2 | D/V |

### 参考情報

* [EU Artificial Intelligence Act — Regulation (EU) 2024/1689 (Official Journal, 12 July 2024)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)
* [ISO/IEC 23894:2023 — Artificial Intelligence — Guidance on Risk Management](https://www.iso.org/standard/77304.html)
* [ISO/IEC 42001:2023 — AI Management Systems Requirements](https://www.iso.org/standard/81230.html)
* [NIST AI Risk Management Framework 1.0](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)
* [NIST SP 800-53 Revision 5 — Security and Privacy Controls](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf)
* [A Unified Approach to Interpreting Model Predictions (SHAP, ICML 2017)](https://arxiv.org/abs/1705.07874)
* [Model Cards for Model Reporting (Mitchell et al., 2018)](https://arxiv.org/abs/1810.03993)
* [Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning (Gal & Ghahramani, 2016)](https://arxiv.org/abs/1506.02142)
* [ISO/IEC 24029-2:2023 — Robustness of Neural Networks — Methodology for Formal Methods](https://www.iso.org/standard/79804.html)
* [IEEE 7001-2021 — Transparency of Autonomous Systems](https://standards.ieee.org/ieee/7001/6929/)
* [Human Oversight under Article 14 of the EU AI Act (Fink, 2025)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5147196)
