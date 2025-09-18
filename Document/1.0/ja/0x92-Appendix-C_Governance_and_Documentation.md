# 付録 C: AI セキュリティガバナンスとドキュメント (AI Security Governance & Documentation)

## 目標

この付録では、システムライフサイクル全体にわたって AI セキュリティを管理するために、組織構造、ポリシー、プロセスを確立するための基本要件を提供します。

---

## AC.1 AI リスク管理フレームワークの導入 (AI Risk Management Framework Adoption)

システムライフサイクル全体にわたって AI 固有のリスクを特定、評価、緩和するための公式のフレームワークを提供します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AC.1.1** | **検証:** AI 固有のリスク評価手法は文書化され、実装されている。 | 1 | D/V |
| **AC.1.2** | **検証:** リスク評価は、AI ライフサイクルの重要なポイントと、重大な変更の前に実施されている。 | 2 | D |
| **AC.1.3** | **検証:** リスク管理フレームワークは確立した標準 (NIST AI RMF など) に準拠している。 | 3 | D/V |

---

## AC.2 AI セキュリティポリシーと手順 (AI Security Policy & Procedures)

安全な AI 開発、デプロイメント、運用のための組織標準を定義して適用します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AC.2.1** | **検証:** 文書化された AI セキュリティポリシーは存在している。 | 1 | D/V |
| **AC.2.2** | **検証:** ポリシーは少なくとも年に一度、および重大な脅威状況の変化の後に見直され、更新されている。 | 2 | D |
| **AC.2.3** | **検証:** ポリシーはすべての AISVS カテゴリと適用可能な規制要件に対応している。 | 3 | D/V |

---

## AC.3 AI セキュリティの役割と責任 (Roles & Responsibilities for AI Security)

組織全体にわたる AI セキュリティの明確な説明責任を確立します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AC.3.1** | **検証:** AI セキュリティの役割と責任は文書化されている。 | 1 | D/V |
| **AC.3.2** | **検証:** 責任者は適切なセキュリティ専門知識を有している。 | 2 | D |
| **AC.3.3** | **検証:** AI 倫理委員会またはガバナンスボードは高リスクの AI システムに対して設立されている。 | 3 | D/V |

---

## AC.4 倫理的 AI ガイドラインの施行 (Ethical AI Guidelines Enforcement)

確立された倫理原則に従って AI システムが動作することを確保します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AC.4.1** | **検証:** AI 開発とデプロイメントに関する倫理ガイドラインは存在している。 | 1 | D/V |
| **AC.4.2** | **検証:** 倫理違反を検出し報告するためのメカニズムは整備されている。 | 2 | D |
| **AC.4.3** | **検証:** デプロイされた AI システムに対する定期的な倫理レビューは実施されている。 | 3 | D/V |

---

## AC.5 AI 規制コンプライアンスの監視 (AI Regulatory Compliance Monitoring)

Maintain awareness of and compliance with evolving AI regulations.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AC.5.1** | **Verify that** processes exist to identify applicable AI regulations. | 1 | D/V |
| **AC.5.2** | **Verify that** compliance with all regulatory requirements is assessed. | 2 | D |
| **AC.5.3** | **Verify that** regulatory changes trigger timely reviews and updates to AI systems. | 3 | D/V |

---

## AC.6 トレーニングデータガバナンス、ドキュメント、プロセス (Training Data Governance, Documentation and Process)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **1.1.2** | **検証:** 品質、代表性、倫理的調達、ライセンス遵守について精査されたデータセットのみを許可し、ポイズニング、埋め込まれたバイアス、知的財産侵害のリスクを低減している。 | 1 | D/V |
| **1.1.5** | **検証:** ラベル付けや注釈の品質はレビュー担当者のクロスチェックまたはコンセンサスによって確保している。 | 2 | D/V |
| **1.1.6** | **検証:** 重要なトレーニングデータセットに対して「データカード」や「データセットのデータシート」を維持し、特性、動機、構成、収集プロセス、前処理、推奨される使用方法、推奨されない使用方法を詳述している。 | 2 | D/V |
| **1.3.2** | **検証:** 特定されたバイアスは、再バランス調整、対象を絞ったデータ拡張、アルゴリズム調整 (前処理、中処理、後処理技法など)、再重み付けなどの文書化された戦略によって緩和され、緩和策が公平性と全体的なモデルパフォーマンスの両方に与える影響を評価している。 | 2 | D/V |
| **1.3.3** | **検証:** トレーニング後の公平性のメトリクスが評価され、文書化されている。 | 2 | D/V |
| **1.3.4** | **検証:** ライフサイクルバイアス管理ポリシーは所有者とレビュー頻度を割り当てている。 | 3 | D/V |
| **1.4.1** | **検証:** ラベル付けや注釈の品質は、明確なガイドライン、レビュー担当者によるクロスチェック、同意メカニズム (注釈者間の合意の監視など)、不整合を解決するために定義されたプロセスによって確保されている。 | 2 | D/V |
| **1.4.4** | **検証:** 安全性、セキュリティ、公平性にとって重要なラベル (有毒コンテンツの特定、重要な医療所見など) は、独立した二重レビューまたは同等の堅牢な検証を必ず受けている。 | 3 | D/V |
| **1.4.6** | **検証:** ラベリングガイドとインストラクションは包括的であり、バージョン管理され、ピアレビューされている。 | 2 | D/V |
| **1.4.6** | **検証:** ラベルに対するデータスキーマは明確に定義され、バージョン管理されている。 | 2 | D/V |
| **1.3.1** | **検証:** データセットは、法的に保護された属性 (人種、性別、年齢など) とモデルの適用ドメインに関連するその他の倫理的にセンシティブな特定 (社会経済的ステータス、位置情報など) にわたって、表現の不均衡と潜在的なバイアスについてプロファイルされている。 | 1 | D/V |
| **1.5.3** | **検証:** ドメイン専門家による手動スポットチェックは、統計的に優位なサンプル (例: 1% 以上または 1,000 サンプルのいずれか大きい方、またはリスク評価によって決定) をカバーし、自動では捕捉できない精緻な品質問題を特定している。 | 2 | V |
| **1.8.4** | **検証:** アウトソースまたはクラウドソースしたラベリングワークフローは技術的/手順的な保護策を含み、データの機密性、完全性、ラベルの品質を確保し、データ漏洩を防いでいる。 | 2 | D/V |
| **1.5.4** | **検証:** 修復手順は来歴記録に追加されている。 | 2 | D/V |
| **1.6.2** | **検証:** フラグが付けられたサンプルはトレーニング前に手動レビューをトリガーしている。 | 2 | D/V |
| **1.6.3** | **検証:** 結果はモデルのセキュリティファイルに反映し、継続的な脅威インテリジェンスに情報提供している。 | 2 | V |
| **1.6.4** | **検証:** 検出ロジックは新しい脅威インテリジェンスで更新されている。 | 3 | D/V |
| **1.6.5** | **検証:** オンライン学習パイプラインは分布ドリフトを監視している。 | 3 | D/V |
| **1.7.1** | **検証:** トレーニングデータ削除ワークフローはプライマリデータと派生データを消去し、モデルへの影響を評価している。また、影響を受けるモデルへの影響が評価され、必要に応じて対処されている (再トレーニングや再キャリブレーションなど)。 | 1 | D/V |
| **1.7.2** | **検証:** トレーニングで使用されるデータに対するユーザーの同意 (および撤回) のスコープとステータスを追跡して尊重するためのメカニズムが存在している。また、データが新しいトレーニングプロセスや重要なモデルアップデートに組み込まれる前にその同意が検証されている。 | 2 | D |
| **1.7.3** | **検証:** ワークフローは毎年テストされ、ログ記録されている。 | 2 | V |
| **1.8.1** | **検証:** 事前トレーニング済みモデルや外部データセットのプロバイダを含むサードパーティのデータサプライヤは、そのデータやモデルを統合する前に、セキュリティ、プライバシー、倫理的調達、データ品質のデューディリジェンスを受けている。 | 2 | D/V |
| **1.8.2** | **検証:** 外部転送は TLS/auth と完全性チェックを使用している。 | 1 | D |
| **1.8.3** | **検証:** 高リスクのデータソース (来歴不明のオープンソースデータセット、審査されていないサプライヤなど) は、機密性の高いアプリケーションで使用する前に、サンドボックス解析、広範な品質/バイアスチェック、標的を絞ったポイズニング検出などの強化された精査を受けている。 | 2 | D/V |
| **1.8.4** | **検証:** サードパーティから取得した事前トレーニング済みモデルは、ファインチューニングやデプロイメントの前に、埋め込まれたバイアス、潜在的なバックドア、アーキテクチャの完全性、元のトレーニングデータの来歴について評価されている。 | 3 | D/V |
| **1.5.3** | **Verify that** if adversarial training is used, the generation, management, and versioning of adversarial datasets are documented and controlled. | 2 | D/V |
| **1.5.3** | **Verify that** the impact of adversarial robustness training on model performance (against both clean and adversarial inputs) and fairness metrics is evaluated, documented, and monitored. | 3 | D/V |
| **1.5.4** | **Verify that** strategies for adversarial training and robustness are periodically reviewed and updated to counter evolving adversarial attack techniques.| 3 | D/V |
| **1.4.2** | **検証:** 不合格のデータセットは監査証跡とともに隔離されている。 | 2 | D/V |
| **1.4.3** | **検証:** 品質ゲートは、例外が承認されない限り、標準以下のデータセットをブロックしている。 | 2 | D/V |
| **1.11.2** | **Verify that** the generation process, parameters, and intended use of synthetic data are documented. | 2 | D/V |
| **1.11.3** | **Verify that** synthetic data is risk-assessed for bias, privacy leakage, and representational issues before use in training. | 2 | D/V || **1.12.2** | **Verify that** access logs are regularly reviewed for unusual patterns, such as large exports or access from new locations. | 2 | D/V |
| **1.12.3** | **Verify that** alerts are generated for suspicious access events and investigated promptly. | 2 | D/V |
| **1.13.1** | **Verify that** explicit retention periods are defined for all training datasets. | 1 | D/V |
| **1.13.2** | **Verify that** datasets are automatically expired, deleted, or reviewed for deletion at the end of their lifecycle. | 2 | D/V |
| **1.13.3** | **Verify that** retention and deletion actions are logged and auditable. | 2 | D/V |
| **1.14.1** | **Verify that** data residency and cross-border transfer requirements are identified and enforced for all datasets. | 2 | D/V |
| **1.14.2** | **Verify that** sector-specific regulations (e.g., healthcare, finance) are identified and addressed in data handling. | 2 | D/V |
| **1.14.3** | **Verify that** compliance with relevant privacy laws (e.g., GDPR, CCPA) is documented and reviewed regularly. | 2 | D/V |
| **1.16.1** | **Verify that** mechanisms exist to respond to data subject requests for access, rectification, restriction, or objection. | 2 | D/V |
| **1.16.2** | **Verify that** requests are logged, tracked, and fulfilled within legally mandated timeframes. | 2 | D/V |
| **1.16.3** | **Verify that** data subject rights processes are tested and reviewed regularly for effectiveness. | 2 | D/V |
| **1.17.1** | **Verify that** an impact analysis is performed before updating or replacing a dataset version, covering model performance, fairness, and compliance. | 2 | D/V |
| **1.17.2** | **Verify that** results of the impact analysis are documented and reviewed by relevant stakeholders. | 2 | D/V |
| **1.17.3** | **Verify that** rollback plans exist in case new versions introduce unacceptable risks or regressions. | 2 | D/V |
| **1.18.1** | **Verify that** all personnel involved in data annotation are background-checked and trained in data security and privacy. | 2 | D/V |
| **1.18.2** | **Verify that** all annotation personnel sign confidentiality and non-disclosure agreements. | 2 | D/V |
| **1.18.3** | **Verify that** annotation platforms enforce access controls and monitor for insider threats. | 2 | D/V |

---

## AC.7 Model Lifecycle Governance & Documentation

Establish governance processes for model lifecycle documentation, approval workflows, and audit trail requirements.

| # | Description | Level | Role |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AC.7.1** | **Verify that** all model artifacts use semantic versioning (MAJOR.MINOR.PATCH) with documented criteria specifying when each version component increments. | 2 | D/V |
| **AC.7.2** | **Verify that** emergency deployments require documented security risk assessment and approval from a pre-designated security authority within pre-agreed timeframes. | 2 | D/V |
| **AC.7.3** | **Verify that** rollback artifacts (previous model versions, configurations, dependencies) are retained according to organizational policies. | 2 | V |
| **AC.7.4** | **Verify that** audit log access requires appropriate authorization and all access attempts are logged with user identity and a timestamp. | 2 | D/V |
| **AC.7.5** | **Verify that** retired model artifacts are retained according to data retention policies. | 1 | D/V |

---

## C2.1 プロンプトインジェクションの防御 (Prompt Injection Defense)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.1.3** | **検証:** 敵対的評価テスト (レッドチームの「メニーショット」プロンプトなど) は、すべてのモデルまたはプロンプトテンプレートのリリース前に、成功率の閾値と回帰の自動ブロッカーを使用して、実行されている。 | 2 |  D/V |
| **2.1.5** | **検証:** すべてのプロンプトフィルタルールの更新、分類モデルのバージョン、ブロックリストの変更はバージョン管理され、監査可能である。 | 3 |  D/V |

---

## C2.2 敵対的サンプルへの耐性 (Adversarial-Example Resistance)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.2.5** | **検証:** 堅牢性メトリクス (既知の攻撃スイートの成功率) は自動化によって時間の経過とともに追跡され、回帰はアラートをトリガーしている。 | 3 |  D/V |

---

## C2.4 コンテンツとポリシーの審査 (Content & Policy Screening)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.4.3** | **検証:** スクリーニングモデルやルールセットは、新たに観察されたジェイルブレイクやポリシーバイパスのパターンを組み込んで、少なくとも四半期ごとに再トレーニング/アップデートされている。 | 2 | D |

---

## C2.5 入力レート制限と不正使用防止 (Input Rate Limiting & Abuse Prevention)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.5.4** | **検証:** 不正使用防止ログは保持され、新たな攻撃パターンがないかレビューされている。 | 3 | V |

---

## C2.7 入力の来歴と属例 (Input Provenance & Attribution)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.7.1** | **検証:** すべてのユーザー入力は取り込み時にメタデータ (ユーザーID、セッション、ソース、タイムスタンプ、IP アドレス) でタグ付けされている。 | 1 | D/V |
| **2.7.2** | **検証:** 来歴メタデータが保持され、すべての処理された入力について監査可能である。 | 2 | D/V |
| **2.7.3** | **検証:** 異常な入力ソースや信頼できない入力ソースはフラグ付けされ、強化された精査またはブロックの対象としている。 | 2 | D/V |

---

## C2.9 マルチモーダルセキュリティバリデーションパイプライン (Multi-Modal Security Validation Pipeline)

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.9.5** | **検証:** 様式固有のコンテンツ分類子は、文書化されたスケジュール (最低四半期ごと) に従って更新され、新しい脅威パターン、敵対的サンプル、パフォーマンスベンチマークがベースライン閾値を上回るように維持している。 | 3 | D/V |

### 参考情報

* [NIST AI Risk Management Framework 1.0](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)
* [ISO/IEC 42001:2023 — AI Management Systems Requirements](https://www.iso.org/standard/81230.html)
* [ISO/IEC 23894:2023 — Artificial Intelligence — Guidance on Risk Management](https://www.iso.org/standard/77304.html)
* [EU Artificial Intelligence Act — Regulation (EU) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)
* [ISO/IEC 24029‑2:2023 — Robustness of Neural Networks — Methodology for Formal Methods](https://www.iso.org/standard/79804.html)
