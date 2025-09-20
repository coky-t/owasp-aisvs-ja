# 付録 D: AI 支援のセキュアコーディングガバナンスと検証 (AI-Assisted Secure Coding Governance & Verification)

## 目標

この章では、ソフト開発時に AI 支援のコーディングツールを安全かつ効果的に使用するためのベースライン組織制御を定義し、SDLC 全体にわたるセキュリティとトレーサビリティを確保します。

---

## AD.1 AI 支援のセキュアコーディングワークフロー (AI-Assisted Secure‑Coding Workflow)

既存のセキュリティゲートを弱めることなく、AI ツールを組織のセキュアソフトウェア開発ライフサイクル (SSDLC) に統合します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AD.1.1** | **検証:** 文書化されたワークフローは AI ツールがコードを生成、リファクタ、またはレビューする時期と方法を記述している。 | 1 | D/V |
| **AD.1.2** | **検証:** ワークフローは各 SSDLC フェーズ (設計、実装、コードレビュー、テスト、デプロイメント) にマップしている。 | 2 | D |
| **AD.1.3** | **検証:** メトリクス (脆弱性密度、平均検出時間など) は AI が生成したコードから収集され、人間のみでのベースラインと比較されている。 | 3 | D/V |

---

## AD.2 AI ツールの適性と脅威モデリング (AI Tool Qualification & Threat Modeling)

AI コーディングツールは導入する前にセキュリティ機能、リスク、サプライチェーンの影響を評価されるようにします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AD.2.1** | **検証:** 各 AI ツールの脅威モデルは、不正使用、モデル反転、データ漏洩、依存関係チェーンのリスクを特定している。 | 1 | D/V |
| **AD.2.2** | **検証:** ツールの評価は、ローカルコンポーネントの静的/動的解析と SaaS エンドポイント (TLS、認証/認可、ログ記録) の評価を含んでいる。 | 2 | D |
| **AD.2.3** | **検証:** 評価は認められたフレームワークに従っており、メジャーバージョン変更後に再実行されている。 | 3 | D/V |

---

## AD.3 安全なプロンプトとコンテキストの管理 (Secure Prompt & Context Management)

AI モデルのプロンプトやコンテキストを構築する際に、シークレット、プロプライエタリコード、個人データの漏洩を防ぎます。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AD.3.1** | **検証:** ガイダンスの書面はプロンプトでシークレット、クレデンシャル、機密データを送信することを禁止している。 | 1 | D/V |
| **AD.3.2** | **検証:** 技術的コントロール (クライアントサイドの修正、承認されたコンテキストフィルタ) は機密性の高いアーティファクトを自動的に除去している。 | 2 | D |
| **AD.3.3** | **検証:** プロンプトとレスポンスはトークン化され、転送時と保存時に暗号化され、保持期間はデータ分類ポリシーに準拠している。 | 3 | D/V |

---

## AD.4 AI 生成コードのバリデーション (Validation of AI‑Generated Code)

Detect and remediate vulnerabilities introduced by AI output before the code is merged or deployed.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AD.4.1** | **Verify that** AI‑generated code is always subjected to human code review. | 1 | D/V |
| **AD.4.2** | **Verify that** automated scanners (SAST/IAST/DAST) run on every pull request containing AI‑generated code and block merges on critical findings. | 2 | D |
| **AD.4.3** | **Verify that** differential fuzz testing or property‑based tests prove security‑critical behaviors (e.g., input validation, authorization logic). | 3 | D/V |

---

## AD.5 コード提案の説明可能性と追跡可能性 (Explainability & Traceability of Code Suggestions)

Provide auditors and developers with insight into why a suggestion was made and how it evolved.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AD.5.1** | **Verify that** prompt/response pairs are logged with commit IDs. | 1 | D/V |
| **AD.5.2** | **Verify that** developers can surface model citations (training snippets, documentation) supporting a suggestion. | 2 | D |
| **AD.5.3** | **Verify that** explainability reports are stored with design artifacts and referenced in security reviews, satisfying ISO/IEC 42001 traceability principles. | 3 | D/V |

---

## AD.6 継続的なフィードバックとモデルのファインチューニング (Continuous Feedback & Model Fine‑Tuning)

Improve model security performance over time while preventing negative drift.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **AD.6.1** | **Verify that** developers can flag insecure or non‑compliant suggestions, and that flags are tracked. | 1 | D/V |
| **AD.6.2** | **Verify that** aggregated feedback informs periodic fine‑tuning or retrieval‑augmented generation with vetted secure‑coding corpora (e.g., OWASP Cheat Sheets). | 2 | D |
| **AD.6.3** | **Verify that** a closed‑loop evaluation harness runs regression tests after every fine‑tune; security metrics must meet or exceed prior baselines before deployment. | 3 | D/V |

---

### 参考情報

* [NIST AI Risk Management Framework 1.0](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)
* [ISO/IEC 42001:2023 — AI Management Systems Requirements](https://www.iso.org/standard/81230.html)
* [OWASP Secure Coding Practices — Quick Reference Guide](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
