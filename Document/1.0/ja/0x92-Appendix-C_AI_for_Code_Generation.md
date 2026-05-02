# 付録 C AI 支援のセキュアコーディング (AI-Assisted Secure Coding)

## 目標

この章では、ソフト開発時に AI 支援のコーディングツールを安全かつ効果的に使用するためのベースライン組織制御を定義し、SDLC 全体にわたるセキュリティとトレーサビリティを確保します。

---

## AC.1 AI 支援のセキュアコーディングワークフロー (AI-Assisted Secure-Coding Workflow)

既存のセキュリティゲートを弱めることなく、AI ツールを組織のセキュアソフトウェア開発ライフサイクル (SSDLC) に統合します。

| # | 説明 | レベル |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.1.1** | **検証:** 文書化されたワークフローは AI ツールがコードを生成、リファクタ、またはレビューする時期と方法を記述している。 | 1 |
| **AC.1.2** | **検証:** ワークフローは各 SSDLC フェーズ (設計、実装、コードレビュー、テスト、デプロイメント) にマップしている。 | 2 |
| **AC.1.3** | **検証:** メトリクス (脆弱性密度、平均検出時間など) は AI が生成したコードから収集され、人間のみでのベースラインと比較されている。 | 3 |

---

## AC.2 AI ツールの適性と脅威モデリング (AI Tool Qualification & Threat Modeling)

AI コーディングツールは導入する前にセキュリティ機能、リスク、サプライチェーンの影響を評価されるようにします。

| # | 説明 | レベル |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.2.1** | **検証:** 各 AI ツールの脅威モデルは、不正使用、モデル反転、データ漏洩、依存関係チェーンのリスクを特定している。 | 1 |
| **AC.2.2** | **検証:** ツールの評価は、ローカルコンポーネントの静的/動的解析と SaaS エンドポイント (TLS、認証/認可、ログ記録) の評価を含んでいる。 | 2 |
| **AC.2.3** | **検証:** 評価は認められたフレームワークに従っており、メジャーバージョン変更後に再実行されている。 | 3 |

---

## AC.3 安全なプロンプトとコンテキストの管理 (Secure Prompt & Context Management)

AI モデルのプロンプトやコンテキストを構築する際に、シークレット、プロプライエタリコード、個人データの漏洩を防ぎます。

| # | 説明 | レベル |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.3.1** | **検証:** ガイダンスの書面はプロンプトでシークレット、クレデンシャル、機密データを送信することを禁止している。 | 1 |
| **AC.3.2** | **検証:** 技術的コントロール (クライアントサイドの修正、承認されたコンテキストフィルタ) は機密性の高いアーティファクトを自動的に除去している。 | 2 |
| **AC.3.3** | **検証:** プロンプトとレスポンスはトークン化され、転送時と保存時に暗号化され、保持期間はデータ分類ポリシーに準拠している。 | 3 |

---

## AC.4 AI 生成コードのバリデーション (Validation of AI-Generated Code)

コードがマージまたはデプロイされる前に、AI 出力によって導入された脆弱性を検出して修復します。

| # | 説明 | レベル |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.4.1** | **検証:** AI が生成したコードは常に人間のコードレビューを受けている。 | 1 |
| **AC.4.2** | **検証:** 自動スキャナ (SAST/IAST/DAST) は AI が生成したコードを含むすべてのプルリクエストに対して実行しており、重大な検出結果ではマージをブロックしている。 | 2 |
| **AC.4.3** | **検証:** 差分ファズテストまたはプロパティベースのテストはセキュリティ上重要な動作 (入力バリデーション、認可ロジックなど) を証明している。 | 3 |

---

## AC.5 コード提案の説明可能性と追跡可能性 (Explainability & Traceability of Code Suggestions)

提案が行われた理由とその経緯についての洞察を監査担当者と開発者に提供します。

| # | 説明 | レベル |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.5.1** | **検証:** プロンプト/レスポンスのペアはコミット ID とともにログ記録されている。 | 1 |
| **AC.5.2** | **検証:** 開発者は提案を裏付けるモデル引用 (トレーニングスニペット、ドキュメント) を提示できる。 | 2 |
| **AC.5.3** | **検証:** 説明可能性レポートは設計成果物とともに保存され、セキュリティレビューで参照され、ISO/IEC 42001 追跡可能性原則を満たしている。 | 3 |

---

## AC.6 継続的なフィードバックとモデルのファインチューニング (Continuous Feedback & Model Fine-Tuning)

ネガティブドリフトを防ぎながら、モデルのセキュリティパフォーマンスを時間の経過とともに向上します。

| # | 説明 | レベル |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.6.1** | **検証:** 開発者は安全でない提案や準拠していない提案にフラグ付けでき、そのフラグは追跡されている。 | 1 |
| **AC.6.2** | **検証:** 集約されたフィードバックは、精査されたセキュアコーディングコーパス (OWASP チートシートなど) とともに、定期的なファインチューニングや検索拡張生成に活用している。 | 2 |
| **AC.6.3** | **検証:** 閉ループ評価ハーネスはファインチューンごとに回帰テストを実行している。セキュリティメトリクスはデプロイメント前に以前のベースラインを満たすか上回る必要がある。 | 3 |

---

## AC.7 AI-Generated Infrastructure & Pipeline Artifacts

Ensure that AI-generated infrastructure-as-code (IaC), CI/CD workflows, deployment configurations, and security policy artifacts are subject to appropriate validation and governance controls.

| # | Description | Level |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.7.1** | **Verify that** AI-generated infrastructure-as-code, CI/CD workflows, and security policy artifacts are clearly identified and tracked. | 1 |
| **AC.7.2** | **Verify that** AI-generated infrastructure and pipeline configurations require appropriate review and approval prior to execution. | 2 |
| **AC.7.3** | **Verify that** AI-generated infrastructure and workflow changes are subject to security validation, configuration checks, and policy enforcement equivalent to or stricter than application code. | 3 |

---

## AC.8 Autonomous Agent Change Control Constraints

Ensure that autonomous AI agents involved in code or configuration generation are subject to appropriate separation of duties and cannot independently approve or promote their own changes.

| # | Description | Level |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.8.1** | **Verify that** autonomous agents cannot approve, merge, sign, or deploy artifacts that they have generated. | 1 |
| **AC.8.2** | **Verify that** AI systems operate with scoped identities and permissions that prevent self-promotion of generated artifacts across environments. | 2 |
| **AC.8.3** | **Verify that** separation of duties is enforced between artifact generation, review, approval, and deployment stages for AI-generated changes. | 3 |

---

## AC.9 AI Artifact Origin Validation for Deployment

Ensure that deployment and promotion pipelines validate the origin and generation history of AI-generated artifacts before they are promoted.

| # | Description | Level |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.9.1** | **Verify that** AI-generated artifacts include origin and generation metadata identifying the AI system that produced them, the generation context, and associated audit records. | 1 |
| **AC.9.2** | **Verify that** deployment pipelines validate the presence and integrity of origin and generation metadata for AI-generated artifacts prior to promotion. | 2 |
| **AC.9.3** | **Verify that** artifacts lacking required origin and generation information, or produced by untrusted AI systems or environments, are rejected during deployment. | 3 |

---

## AC.10 Generation Audit Trail Completeness and Validation

Ensure that AI-generated artifacts include complete and consistent origin and generation records, and that these records are validated prior to integration or deployment.

In practice, policy-based enforcement depends on the availability and quality of origin and generation records. Incomplete or inconsistent records can lead to missed detections or enforcement gaps. These controls ensure that origin tracking is treated as a first-class requirement and validated prior to artifact acceptance.

| # | Description | Level |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|
| **AC.10.1** | **Verify that** AI-generated artifacts include required origin and generation fields (e.g., model identity, generation context, human involvement, and session identifiers). | 1 |
| **AC.10.2** | **Verify that** origin and generation metadata is validated for completeness and consistency (e.g., no missing or ambiguous fields, normalized representations). | 2 |
| **AC.10.3** | **Verify that** artifacts with incomplete, inconsistent, or unverifiable origin and generation metadata are rejected prior to merge or deployment. | 3 |

---

## 参考情報

* [NIST AI Risk Management Framework 1.0](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)
* [ISO/IEC 42001:2023: AI Management Systems Requirements](https://www.iso.org/standard/42001)
* [OWASP Secure Coding Practices: Quick Reference Guide](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
