# C5 AI コンポーネントとユーザーのアクセス制御とアイデンティティ (Access Control & Identity for AI Components & Users)

## 管理目標

AI システムへの効果的なアクセス制御には、堅牢なアイデンティティ管理、コンテキストに応じた認可、ゼロトラスト原則に従ったランタイム強制が必要です。これらの制御により、人間、サービス、自律エージェントが、明示的に許可されたスコープ内でのみ、モデル、データ、計算リソースとやり取りでき、継続的検証と監査機能を備えています。

---

## C5.1 アイデンティティ管理と認証 (Identity Management & Authentication)

特権操作のために多要素認証ですべてのエンティティに対して暗号論的に裏付けされたアイデンティティを確立します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.1.1** | **検証:** すべての人間のユーザーとサービスプリンシパルは、一意のアイデンティティとトークンのマッピング (共有アカウントやクレデンシャルではない) で OIDC/SAML プロトコルを使用して、集中型エンタープライズアイデンティティプロバイダ (IdP) を通じて認証している。 | 1 | D/V |
| **5.1.2** | **検証:** 高リスクの操作 (モデルのデプロイメント、重みのエクスポート、トレーニングデータへのアクセス、本番構成の変更) は、多要素認証またはセッション再バリデーションでのステップアップ認証を必要としている。 | 1 | D/V |
| **5.1.3** | **検証:** 新しいプリンシパルは、本番システムへのアクセスを許可する前に、NIST 800-63-3 IAL-2 または同等の標準に準拠したアイデンティティ認証を受けている。 | 2 | D |
| **5.1.4** | **検証:** アクセスレビューは四半期ごとに実施され、休止アカウントの自動検出、クレデンシャルローテーションの強制、プロビジョニング解除ワークフローを実施されている。 | 2 | V |
| **5.1.5** | **検証:** 連合 AI エージェントは、最大有効期間が 24 時間であり、オリジンの暗号論的証明を含む、署名付き JWT アサーションを介して認証している。 | 3 | D/V |

---

## C5.2 リソース認可と最小権限 (Resource Authorization & Least Privilege)

明示的なパーミッションモデルと監査証跡で、すべての AI リソースに対してきめ細かなアクセス制御を実装します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.2.1** | **検証:** すべての AI リソース (データセット、モデル、エンドポイント、ベクトルコレクション、エンベディングインデックス、計算インスタンス) は、明示的な許可リストとデフォルト拒否ポリシーで、ロールベースのアクセス制御を適用している。 | 1 | D/V |
| **5.2.2** | **検証:** 最小権限の原則は、サービスアカウントが読み取り専用パーミッションから始まり、書き込みアクセスには文書化されたビジネス上の正当性を必要とするように、デフォルトで適用されている。 | 1 | D/V |
| **5.2.3** | **検証:** すべてのアクセス制御の変更は承認された変更要求にリンクされ、タイムスタンプ、アクターアイデンティティ、リソース識別子、パーミッションデルタとともに不変的にログ記録されている。 | 1 | V |
| **5.2.4** | **検証:** データ分類ラベル (PII、PHI、輸出規制対象、独自) は一貫したポリシー適用で派生リソース (エンベディング、プロンプトキャッシュ、モデル出力) に自動的に伝播している。 | 2 | D |
| **5.2.5** | **検証:** 不正アクセスの試みや権限昇格イベントは 5 分以内に SIEM システムにコンテキストメタデータとともにリアルタイムアラートをトリガーしている。 | 2 | D/V |

---

## C5.3 動的ポリシー評価 (Dynamic Policy Evaluation)

監査機能を備えたコンテキスト認識型認可決定のための属性ベースアクセス制御 (ABAC) エンジンを導入します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.3.1** | **検証:** 認可決定は、暗号論的完全性保護を備えた認証 API を介してアクセス可能な、専用のポリシーエンジン (OPA、Cedar、または同等のもの) に外部化されている。 | 1 | D/V |
| **5.3.2** | **検証:** ポリシーは、ユーザークリアランスレベル、リクエストコンテキスト、テナント分離、時間的制約など、動的な属性を実行時に評価している。 | 1 | D/V |
| **5.3.3** | **検証:** ポリシー定義はバージョン管理され、ピアレビューされ、本番環境へのデプロイメント前に CI/CD パイプラインでの自動テストを通じて検証されている。 | 2 | D |
| **5.3.4** | **検証:** ポリシー評価結果は構造化された決定根拠を含んでおり、相関分析とコンプライアンスレポートのために SIEM システムに送信されている。 | 2 | V |
| **5.3.5** | **検証:** ポリシーキャッシュの有効期間 (TTL) 値は、高機密リソースでは 5 分を超えず、キャッシュ無効化機能を持つ標準リソースでは 1 時間を超えていない。 | 3 | D/V |

---

## C5.4 クエリ時のセキュリティ強化 (Query-Time Security Enforcement)

必須フィルタリングと低レベルセキュリティポリシーでのデータベース層のセキュリティ制御を実装します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.4.1** | **検証:** すべてのベクトルデータベースと SQL クエリは、アプリケーションコードではなく、データベースエンジンレベルで適用される必須のセキュリティフィルタ (テナント ID、機密ラベル、ユーザースコープ) を含んでいる。 | 1 | D/V |
| **5.4.2** | **検証:** 行レベルセキュリティ (RLS) ポリシーとフィールドレベルのマスキングは、すべてのベクトルデータベース、検索インデックス、トレーニングデータセットのポリシー検証で有効化されている。 | 1 | D/V |
| **5.4.3** | **検証:** 失敗した認可評価は、空の結果セットを返すのではなく、クエリを直ちに中止し、明示的な認可エラーコードを返すことで、"Confused Deputy 攻撃" を防いでいる。 | 2 | D |
| **5.4.4** | **検証:** ポリシー評価レイテンシは継続的に監視され、認可バイパスを可能にする可能性のあるタイムアウト状態に対して自動アラートを発報している。 | 2 | V |
| **5.4.5** | **検証:** クエリ再試行メカニズムは、アクティブユーザーセッション内での動的なパーミッション変更を考慮して認可ポリシーを再評価している。 | 3 | D/V |

---

## C5.5 出力フィルタリングとデータ損失防止 (Output Filtering & Data Loss Prevention)

AI 生成コンテンツでの不正なデータ公開を防ぐために、後処理制御を導入します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.5.1** | **検証:** 推論後フィルタリングメカニズムは、コンテンツを要求者に配信する前に、認可されていない PII、機密情報、独自データをスキャンして修正している。 | 1 | D/V |
| **5.5.2** | **検証:** モデル出力内の引用、参照、ソース属性は呼び出し元のエンタイトルメントに対して検証されており、不正アクセスが検出された場合は削除されている。 | 1 | D/V |
| **5.5.3** | **検証:** 出力形式の制限 (サニタイズされた PDF、メタデータが削除された画像、承認されたファイルタイプ) はユーザーパーミッションレベルとデータ分類に基づいて適用されている。 | 2 | D |
| **5.5.4** | **検証:** 修正アルゴリズムは決定論的であり、バージョン管理されており、コンプライアンス調査とフォレンジック解析をサポートするために監査ログを維持している。 | 2 | V |
| **5.5.5** | **検証:** 高リスクの修正イベントは、データの公開なしでフォレンジック検索できるように、元のコンテンツの暗号論的ハッシュを含む適応型ログを生成している。 | 3 | V |

---

## C5.6 マルチテナントの分離 (Multi-Tenant Isolation)

共有 AI インフラストラクチャ内のテナント間では暗号化と論理的分離を確保します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.6.1** | **検証:** メモリキャッシュ、エンベディングストア、キャッシュエントリ、一時ファイルはテナントごとに名前空間で分離されており、テナント削除またはセッション終了時に安全に消去している。 | 1 | D/V |
| **5.6.2** | **検証:** すべての API リクエストは、セッションコンテキストとユーザーエンタイトルメントに対して暗号論的に検証された、認証済みのテナント識別子を含んでいる。 | 1 | D/V |
| **5.6.3** | **検証:** ネットワークポリシーは、サービルメッシュとコンテナオーケストレーションプラットフォーム内でのテナント間通信に対して、デフォルト拒否のルールを実装している。 | 2 | D |
| **5.6.4** | **検証:** 暗号鍵はテナントごとに一意であり、顧客管理鍵 (CMK) サポートがあり、テナントデータストア間で暗号論的に分離している。 | 3 | D |

---

## C5.7 自律エージェント認可 (Autonomous Agent Authorization)

Control permissions for AI agents and autonomous systems through scoped capability tokens and continuous authorization.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.7.1** | **Verify that** autonomous agents receive scoped capability tokens that explicitly enumerate permitted actions, accessible resources, time boundaries, and operational constraints. | 1 | D/V |
| **5.7.2** | **Verify that** high-risk capabilities (file system access, code execution, external API calls, financial transactions) are disabled by default and require explicit authorizations for activation with business justifications. | 1 | D/V |
| **5.7.3** | **Verify that** capability tokens are bound to user sessions, include cryptographic integrity protection, and ensure that they cannot be persisted or reused in offline scenarios. | 2 | D |
| **5.7.4** | **Verify that** agent-initiated actions undergo secondary authorization through the ABAC policy engine with full context evaluation and audit logging. | 2 | V |
| **5.7.5** | **Verify that** agent error conditions and exception handling include capability scope information to support incident analysis and forensic investigation. | 3 | V |

---

## 参考情報

### Standards & Frameworks

* [NIST SP 800-63-3: Digital Identity Guidelines](https://pages.nist.gov/800-63-3/)
* [Zero Trust Architecture – NIST SP 800-207](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-207.pdf)
* [OWASP Application Security Verification Standard (AISVS)](https://owasp.org/www-project-application-security-verification-standard/)

### Implementation Guides

* [Identity and Access Management in the AI Era: 2025 Guide – IDSA](https://www.idsalliance.org/blog/identity-and-access-management-in-the-ai-era-2025-guide/)
* [Attribute-Based Access Control with OPA – Permify](https://medium.com/permify-tech-blog/attribute-based-access-control-abac-implementation-with-open-policy-agent-opa-b47052248f29)
* [How We Designed Cedar to Be Intuitive, Fast, and Safe – AWS](https://aws.amazon.com/blogs/security/how-we-designed-cedar-to-be-intuitive-to-use-fast-and-safe/)

### AI-Specific Security

* [Row Level Security in Vector DBs for RAG – Bluetuple.ai](https://medium.com/bluetuple-ai/implementing-row-level-security-in-vector-dbs-for-rag-applications-fdbccb63d464)
* [Tenant Isolation in Multi-Tenant Systems – WorkOS](https://workos.com/blog/tenant-isolation-in-multi-tenant-systems)
* [Handling AI Agent Permissions – Stytch](https://stytch.com/blog/handling-ai-agent-permissions/)
* [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
