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
| **5.2.1** | **検証:** すべての AI リソース (データセット、モデル、エンドポイント、ベクターコレクション、エンベディングインデックス、計算インスタンス) は、明示的な許可リストとデフォルト拒否ポリシーで、ロールベースのアクセス制御を適用している。 | 1 | D/V |
| **5.2.2** | **検証:** 最小権限の原則は、サービスアカウントが読み取り専用パーミッションから始まり、書き込みアクセスには文書化されたビジネス上の正当性を必要とするように、デフォルトで適用されている。 | 1 | D/V |
| **5.2.3** | **検証:** すべてのアクセス制御の変更は承認された変更要求にリンクされ、タイムスタンプ、アクターアイデンティティ、リソース識別子、パーミッションデルタとともに不変的にログ記録されている。 | 1 | V |
| **5.2.4** | **検証:** データ分類ラベル (PII、PHI、輸出規制対象、独自) は一貫したポリシー適用で派生リソース (エンベディング、プロンプトキャッシュ、モデル出力) に自動的に伝播している。 | 2 | D |
| **5.2.5** | **検証:** 不正アクセスの試みや権限昇格イベントは 5 分以内に SIEM システムにコンテキストメタデータとともにリアルタイムアラートをトリガーしている。 | 2 | D/V |

---

## C5.3 動的ポリシー評価 (Dynamic Policy Evaluation)

Deploy attribute-based access control (ABAC) engines for context-aware authorization decisions with audit capabilities.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.3.1** | **Verify that** authorization decisions are externalized to a dedicated policy engine (OPA, Cedar, or equivalent) accessible via authenticated APIs with cryptographic integrity protection. | 1 | D/V |
| **5.3.2** | **Verify that** policies evaluate dynamic attributes at runtime including user clearance level, resource sensitivity classification, request context, tenant isolation, and temporal constraints. | 1 | D/V |
| **5.3.3** | **Verify that** policy definitions are version-controlled, peer-reviewed, and validated through automated testing in CI/CD pipelines before production deployment. | 2 | D |
| **5.3.4** | **Verify that** policy evaluation results include structured decision rationales and are transmitted to SIEM systems for correlation analysis and compliance reporting. | 2 | V |
| **5.3.5** | **Verify that** policy cache time-to-live (TTL) values do not exceed 5 minutes for high-sensitivity resources and 1 hour for standard resources with cache invalidation capabilities. | 3 | D/V |

---

## C5.4 クエリ時のセキュリティ強化 (Query-Time Security Enforcement)

Implement database-layer security controls with mandatory filtering and row-level security policies.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.4.1** | **Verify that** all vector database and SQL queries include mandatory security filters (tenant ID, sensitivity labels, user scope) enforced at the database engine level, not application code. | 1 | D/V |
| **5.4.2** | **Verify that** row-level security (RLS) policies and field-level masking are enabled with policy inheritance for all vector databases, search indices, and training datasets. | 1 | D/V |
| **5.4.3** | **Verify that** failed authorization evaluations will prevent "confused deputy attacks" by immediately aborting queries and returning explicit authorization error codes rather than returning empty result sets. | 2 | D |
| **5.4.4** | **Verify that** policy evaluation latency is continuously monitored with automated alerts for timeout conditions that could enable authorization bypass. | 2 | V |
| **5.4.5** | **Verify that** query retry mechanisms re-evaluate authorization policies to account for dynamic permission changes within active user sessions. | 3 | D/V |

---

## C5.5 出力フィルタリングとデータ損失防止 (Output Filtering & Data Loss Prevention)

Deploy post-processing controls to prevent unauthorized data exposure in AI-generated content.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.5.1** | **Verify that** post-inference filtering mechanisms scan and redact unauthorized PII, classified information, and proprietary data before delivering content to requestors. | 1 | D/V |
| **5.5.2** | **Verify that** citations, references, and source attributions in model outputs are validated against caller entitlements and removed if unauthorized access is detected. | 1 | D/V |
| **5.5.3** | **Verify that** output format restrictions (sanitized PDFs, metadata-stripped images, approved file types) are enforced based on user permission levels and data classifications. | 2 | D |
| **5.5.4** | **Verify that** redaction algorithms are deterministic, version-controlled, and maintain audit logs to support compliance investigations and forensic analysis. | 2 | V |
| **5.5.5** | **Verify that** high-risk redaction events generate adaptive logs that include cryptographic hashes of original content for forensic retrieval without data exposure. | 3 | V |

---

## C5.6 マルチテナントの分離 (Multi-Tenant Isolation)

Ensure cryptographic and logical isolation between tenants in shared AI infrastructure.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **5.6.1** | **Verify that** memory spaces, embedding stores, cache entries, and temporary files are namespace-segregated per tenant with secure purging on tenant deletion or session termination. | 1 | D/V |
| **5.6.2** | **Verify that** every API request includes an authenticated tenant identifier that is cryptographically validated against session context and user entitlements. | 1 | D/V |
| **5.6.3** | **Verify that** network policies implement default-deny rules for cross-tenant communication within service meshes and container orchestration platforms. | 2 | D |
| **5.6.4** | **Verify that** encryption keys are unique per tenant with customer-managed key (CMK) support and cryptographic isolation between tenant data stores. | 3 | D |

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
