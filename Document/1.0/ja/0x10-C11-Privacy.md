# C11 プライバシー保護と個人データ管理 (Privacy Protection & Personal Data Management)

## 管理目標

AI ライフサイクル全体 (収集、トレーニング、推論、インシデント対応) にわたって厳格なプライバシー保証を維持し、個人データが明確な同意、必要最小限の範囲、証明可能な消去、形式プライバシー保証でのみ処理されるようにします。

---

## 11.1 匿名化とデータ最小化 (Anonymization & Data Minimization)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **11.1.1** | **検証:** 直接識別子と準識別子は削除され、ハッシュされている。 | 1 | D/V |
| **11.1.2** | **検証:** 自動監査は k-匿名性/l-多様性 を測定し、閾値がポリシーを下回ると警告を発している。 | 2 | D/V |
| **11.1.3** | **検証:** モデルの特徴量重要度レポートは ε = 0.01 相互情報量を超える識別子の漏洩がないことを証明している。 | 2 | V |
| **11.1.4** | **検証:** 形式証明や合成データ証明はリンク攻撃の下でも再識別リスクが 0.05 以下であることを示している。 | 3 | V |

---

## 11.2 忘れられる権利と削除の強制 (Right-to-be-Forgotten & Deletion Enforcement)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **11.2.1** | **検証:** データ主体の削除要求は、30 日未満のサービスレベル契約内で、未加工データセット、チェックポイント、エンベディング、ログ、バックアップに伝播している。 | 1 | D/V |
| **11.2.2** | **検証:** 「機械学習解除 (machine-unlearning)」ルーチンは、認定された学習解除アルゴリズムを使用して、物理的に再訓練するか、削除を近似している。 | 2 | D |
| **11.2.3** | **検証:** シャドウモデルの評価は忘れられたレコードが与える影響が学習解除後の出力の 1% 未満であることを証明している。 | 2 | V |
| **11.2.4** | **検証:** 削除イベントは不変にログ記録されており、規制当局により監査可能である。 | 3 | V

---

## 11.3 差分プライバシーの安全対策 (Differential-Privacy Safeguards)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **11.3.1** | **検証:** プライバシー損失会計ダッシュボードは累積 ε がポリシー閾値を超えると警告を発している。 | 2 | D/V |
| **11.3.2** | **検証:** ブラックボックスプライバシー監査は宣言値の 10% 以内で ε̂ を推定している。 | 2 | V |
| **11.3.3** | **検証:** 形式証明はトレーニング後のすべてのファインチューンとエンベディングをカバーしている。 | 3 | V |

---

## 11.4 目的の制限とスコープクリープの保護 (Purpose-Limitation & Scope-Creep Protection)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **11.4.1** | **Verify that** every dataset and model checkpoint carries a machine-readable purpose tag aligned to the original consent. | 1 | D |
| **11.4.2** | **Verify that** runtime monitors detect queries inconsistent with declared purpose and trigger soft refusal. | 1 | D/V |
| **11.4.3** | **Verify that** policy-as-code gates block redeployment of models to new domains without DPIA review. | 3 | D |
| **11.4.4** | **Verify that** formal traceability proofs show every personal data lifecycle remains within consented scope. | 3 | V |

---

## 11.5 同意管理と法的根拠の追跡 (Consent Management & Lawful-Basis Tracking)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **11.5.1** | **Verify that** a Consent-Management Platform (CMP) records opt-in status, purpose, and retention period per data-subject. | 1 | D/V |
| **11.5.2** | **Verify that** APIs expose consent tokens; models must validate token scope before inference. | 2 | D |
| **11.5.3** | **Verify that** denied or withdrawn consent halts processing pipelines within 24 hours. | 2 | D/V |

---

## 11.6 プライバシー制御を備えた連合学習 (Federated Learning with Privacy Controls)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **11.6.1** | **Verify that** client updates employ local differential privacy noise addition before aggregation. | 1 | D |
| **11.6.2** | **Verify that** training metrics are differentially private and never reveal single-client loss. | 2 | D/V |
| **11.6.3** | **Verify that** poisoning-resistant aggregation (e.g., Krum/Trimmed-Mean) is enabled. | 2 | V |
| **11.6.4** | **Verify that** formal proofs demonstrate overall ε budget with less than 5 utility loss. | 3 | V |

---

### 参考情報

* [GDPR & AI Compliance Best Practices](https://www.exabeam.com/explainers/gdpr-compliance/the-intersection-of-gdpr-and-ai-and-6-compliance-best-practices/)
* [EU Parliament Study on GDPR & AI, 2020](https://www.europarl.europa.eu/RegData/etudes/STUD/2020/641530/EPRS_STU%282020%29641530_EN.pdf)
* [ISO 31700-1:2023 — Privacy by Design for Consumer Products](https://www.iso.org/standard/84977.html)
* [NIST Privacy Framework 1.1 (2025 Draft)](https://www.nist.gov/privacy-framework)
* [Machine Unlearning: Right-to-Be-Forgotten Techniques](https://www.kaggle.com/code/tamlhp/machine-unlearning-the-right-to-be-forgotten)
* [A Survey of Machine Unlearning, 2024](https://arxiv.org/html/2209.02299v6)
* [Auditing DP-SGD — ArXiv 2024](https://arxiv.org/html/2405.14106v4)
* [DP-SGD Explained — PyTorch Blog](https://medium.com/pytorch/differential-privacy-series-part-1-dp-sgd-algorithm-explained-12512c3959a3)
* [Purpose-Limitation for AI — IJLIT 2025](https://academic.oup.com/ijlit/article/doi/10.1093/ijlit/eaaf003/8121663)
* [Data-Protection Considerations for AI — URM Consulting](https://www.urmconsulting.com/blog/data-protection-considerations-for-artificial-intelligence-ai)
* [Top Consent-Management Platforms, 2025](https://www.enzuzo.com/blog/best-consent-management-platforms)
* [Secure Aggregation in DP Federated Learning — ArXiv 2024](https://arxiv.org/abs/2407.19286)
