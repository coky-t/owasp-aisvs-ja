# C8 メモリ、エンベディング、ベクトルデータベースセキュリティ (Memory, Embeddings & Vector Database Security)

## 管理目標

エンベディングとベクトルストアは検索拡張生成 (Retrieval-Augmented Generation, RAG) を介して AI システムの半永続的および永続的な「メモリ」として機能します。このメモリは高リスクのデータシンクやデータ抽出パスとなる可能性があります。このコントロールファミリーはメモリパイプラインやベクトルデータベースを強化し、アクセスが最小権限であり、ベクトル化前にデータがサニタイズされ、保持が明示的であり、システムがエンベディング反転、メンバーシップ推論、テナント間漏洩に耐性があるようにします。

## C8.1 メモリと RAG インデックスのアクセス制御 (Access Controls on Memory & RAG Indices)

すべてのベクトルコレクションに対してきめ細かなアクセス制御とクエリ時のスコープ適用を強制します。

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **8.1.1** | **検証:** ベクトルの挿入、更新、削除、クエリ操作はデフォルトで拒否された名前空間/コレクション/ドキュメントタグのスコープ制御 (テナント ID、ユーザー ID、データ分類ラベルなど) で強制されている。 | 1 | D/V |
| **8.1.2** | **検証:** ベクトル操作に使用される API クレデンシャルは **スコープされたクレーム** (許可されたコレクション、許可された動詞、テナントバインディングなど) を保持している。 | 1 | D/V |
| **8.1.3** | **検証:** クロススコープアクセス試行 (クロステナント類似性クエリ、名前空間トラバーサル、タグバイパスなど) は検出され、拒否されている。 | 2 | D/V |

## C8.2 エンベディングのサニタイゼーションとバリデーション (Embedding Sanitization & Validation)

ベクトル化前にコンテンツを事前スクリーンし、メモリ書き込みを信頼できない入力として扱い、安全でないペイロードの取り込みを防止します。

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **8.2.1** | **検証:** 規制対象データと機密フィールドはエンベディング前に検出され、ポリシーに基づいてマスク、トークン化、変換、または削除されている。 | 1 | D/V |
| **8.2.2** | **検証:** エンベディング取り込みは、必要なコンテンツ制約に違反する入力 (非 UTF-8、不正なエンコーディング、サイズが大きすぎるペイロード、非表示 ASCII 文字、取得を害することを目的とした実行可能コンテンツ) を拒否または隔離している。 | 1 | D/V |

## C8.3 メモリの有効期限、失効、削除 (Memory Expiry, Revocation & Deletion)

Retention must be explicit and enforceable; deletions must propagate to derived indices and caches.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **8.3.1** | **Verify that** retention times are applied to every stored vector and related metadata across memory storage. | 1 | D/V |
| **8.3.2** | **Verify that** deletion requests purge vectors, metadata, cache copies, and derivative indices within an organization-defined maximum time. | 1 | D/V |
| **8.3.3** | **Verify that** deleted or expired vectors are removed reliably and are unrecoverable. | 2 | D |
| **8.3.4** | **Verify that** expired vectors are excluded from retrieval results within a measured and monitored propagation windows. | 3 | D/V |

## C8.4 エンベディングの反転とリークの防止 (Prevent Embedding Inversion & Leakage)

Address inversion, membership inference, and attribute inference with explicit threat modeling, mitigations, and regression testing gates.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **8.4.1** | **Verify that** sensitive vector collections are protected against direct read access by infrastructure administrators via technical controls such as application-layer encryption, envelope encryption with strict KMS policies, or equivalent compensating controls. | 2 | D/V |
| **8.4.2** | **Verify that** privacy/utility targets for embedding leakage resistance are **defined and measured**, and that changes to embedding models, tokenizers, retrieval settings, or privacy transforms are gated by regression tests against those targets. | 3 | D/V |

## C8.5 ユーザー固有メモリのスコープの強制 (Scope Enforcement for User-Specific Memory)

Prevent cross-tenant and cross-user leakage in retrieval and prompt assembly.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **8.5.1** | **Verify that** every retrieval operation enforces scope constraints (tenant/user/classification) **in the vector engine query** and verifies them again **before prompt assembly** (post-filter). | 1 | D/V |
| **8.5.2** | **Verify that** vector identifiers, namespaces, and metadata indexing prevent cross-scope collisions and enforce uniqueness per tenant. | 1 | D |
| **8.5.3** | **Verify that** retrieval results that match similarity criteria but fail scope checks are discarded. | 2 | D/V |
| **8.5.4** | **Verify that** multi-tenant tests simulate adversarial retrieval attempts (prompt-based and query-based) and demonstrate zero out-of-scope document inclusion in prompts and outputs. | 2 | V |
| **8.5.5** | **Verify that** encryption keys and access policies are segregated per tenant for memory/vector storage, providing cryptographic isolation in shared infrastructure. | 3 | D/V |

## 参考情報 (推奨追補)

* OWASP Foundation. **OWASP Top 10 for Large Language Model Applications (LLM) 2025**. https://owasp.org/www-project-top-10-for-large-language-model-applications/assets/PDF/OWASP-Top-10-for-LLMs-v2025.pdf
