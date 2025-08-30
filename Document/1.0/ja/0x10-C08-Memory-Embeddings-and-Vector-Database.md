# C8 メモリ、エンベディング、ベクトルデータベースセキュリティ (Memory, Embeddings & Vector Database Security)

## 管理目標

エンベディングとベクトルストアは現代の AI システムの「ライブメモリ」として機能し、ユーザーが提供するデータを継続的に受け入れ、検索拡張生成 (Retrieval-Augmented Generation, RAG) を介してモデルのコンテキストに再表示します。管理されていない状態で放置すると、このメモリは PII を漏洩したり、同意を違反したり、元のテキストを再構築するために反転される可能性があります。このコントロールファミリーの目的は、メモリパイプラインとベクトルデータベースを堅牢化することであり、アクセスは最小権限であり、エンベディングはプライバシーを保護しており、保存されたベクトルは期限切れになるか、必要に応じて取り消すことができ、ユーザーごとのメモリは他のユーザーのプロンプトやコンプリーションを汚染しないようにすることです。

---

## C8.1 メモリと RAG インデックスのアクセス制御 (Access Controls on Memory & RAG Indices)

すべてのベクトルコレクションに対してきめ細かいアクセス制御を適用します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.1.1** | **検証:** 行/名前空間レベルのアクセス制御ルールは、テナント、コレクション、またはドキュメントタグごとに、挿入、削除、クエリ操作を制限している。 | 1 | D/V |
| **8.1.2** | **検証:** API キーや JWT はスコープ指定されたクレーム (コレクション ID、アクション動詞など) を持ち、少なくとも四半期ごとに入れ替えられている。 | 1 | D/V |
| **8.1.3** | **検証:** 権限昇格の試み (名前空間をまたぐ類似性クエリなど) は検出されており、5 分以内に SIEM にログ記録されている。 | 2 | D/V |
| **8.1.4** | **検証:** ベクトル DB 監査は、サブジェクト識別子、操作、ベクトル ID/名前空間、類似度閾値、結果数をログ記録している。 | 2 | D/V |
| **8.1.5** | **検証:** アクセス決定は、エンジンがアップグレードされるか、インデックスシャーディングルールが変更されるたびに、バイパス欠陥についてテストされている。 | 3 | V |

---

## C8.2 エンベディングのサニタイゼーションとバリデーション (Embedding Sanitization & Validation)

Pre-screen text for PII, redact or pseudonymise before vectorisation, and optionally post-process embeddings to strip residual signals.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.2.1** | **Verify that** PII and regulated data are detected via automated classifiers and masked, tokenised, or dropped pre-embedding. | 1 | D/V |
| **8.2.2** | **Verify that** embedding pipelines reject or quarantine inputs containing executable code or non-UTF-8 artifacts that could poison the index. | 1 | D |
| **8.2.3** | **Verify that** local or metric differential-privacy sanitization is applied to sentence embeddings whose distance to any known PII token falls below a configurable threshold. | 2 | D/V |
| **8.2.4** | **Verify that** sanitization efficacy (e.g., recall of PII redaction, semantic drift) is validated at least semi-annually against benchmark corpora. | 2 | V |
| **8.2.5** | **Verify that** sanitization configs are version-controlled and changes undergo peer review. | 3 | D/V |

---

## C8.3 メモリの有効期限、失効、削除 (Memory Expiry, Revocation & Deletion)

GDPR "right to be forgotten" and similar laws require timely erasure; vector stores must therefore support TTLs, hard deletes, and tomb-stoning so that revoked vectors cannot be recovered or re-indexed.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.3.1** | **Verify that** every vector and metadata record carries a TTL or explicit retention label honoured by automated cleanup jobs. | 1 | D/V |
| **8.3.2** | **Verify that** user-initiated deletion requests purge vectors, metadata, cache copies, and derivative indices within 30 days. | 1 | D/V |
| **8.3.3** | **Verify that** logical deletes are followed by cryptographic shredding of storage blocks if hardware supports it, or by key-vault key destruction. | 2 | D |
| **8.3.4** | **Verify that** expired vectors are excluded from nearest-neighbour search results in < 500 ms after expiration. | 3 | D/V |

---

## C8.4 エンベディングの反転とリークの防止 (Prevent Embedding Inversion & Leakage)

Recent defences—noise superposition, projection networks, privacy-neuron perturbation, and application-layer encryption—can cut token-level inversion rates below 5%.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.4.1** | **Verify that** a formal threat model covering inversion, membership and attribute-inference attacks exists and is reviewed yearly. | 1 | V |
| **8.4.2** | **Verify that** application-layer encryption or searchable encryption shields vectors from direct reads by infrastructure admins or cloud staff. | 2 | D/V |
| **8.4.3** | **Verify that** defence parameters (ε for DP, noise σ, projection rank k) balance privacy ≥ 99 % token protection and utility ≤ 3 % accuracy loss. | 3 | V |
| **8.4.4** | **Verify that** inversion-resilience metrics are part of release gates for model updates, with regression budgets defined. | 3 | D/V |

---

## C8.5 ユーザー固有メモリのスコープの強制 (Scope Enforcement for User-Specific Memory)

Cross-tenant leakage remains a top RAG risk: improperly filtered similarity queries can surface another customer's private docs.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.5.1** | **Verify that** every retrieval query is post-filtered by tenant/user ID before being passed to the LLM prompt. | 1 | D/V |
| **8.5.2** | **Verify that** collection names or namespaced IDs are salted per user or tenant so vectors cannot collide across scopes. | 1 | D |
| **8.5.3** | **Verify that** similarity results above a configurable distance threshold but outside the caller's scope are discarded and trigger security alerts. | 2 | D/V |
| **8.5.4** | **Verify that** multi-tenant stress tests simulate adversarial queries attempting to retrieve out-of-scope documents and demonstrate zero leakage. | 2 | V |
| **8.5.5** | **Verify that** encryption keys are segregated per tenant, ensuring cryptographic isolation even if physical storage is shared. | 3 | D/V |

---

## C8.6 高度なメモリシステムセキュリティ (Advanced Memory System Security)

Security controls for sophisticated memory architectures including episodic, semantic, and working memory with specific isolation and validation requirements.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.6.1** | **Verify that** different memory types (episodic, semantic, working) have isolated security contexts with role-based access controls, separate encryption keys, and documented access patterns for each memory type. | 1 | D/V |
| **8.6.2** | **Verify that** memory consolidation processes include security validation to prevent injection of malicious memories through content sanitization, source verification, and integrity checks before storage. | 2 | D/V |
| **8.6.3** | **Verify that** memory retrieval queries are validated and sanitized to prevent extraction of unauthorized information through query pattern analysis, access control enforcement, and result filtering. | 2 | D/V |
| **8.6.4** | **Verify that** memory forgetting mechanisms securely delete sensitive information with cryptographic erasure guarantees using key deletion, multi-pass overwriting, or hardware-based secure deletion with verification certificates. | 3 | D/V |
| **8.6.5** | **Verify that** memory system integrity is continuously monitored for unauthorized modifications or corruption through checksums, audit logs, and automated alerts when memory content changes outside normal operations. | 3 | D/V |

---

## 参考情報

* [Vector database security: Pinecone – IronCore Labs](https://ironcorelabs.com/vectordbs/pinecone-security/)
* [Securing the Backbone of AI: Safeguarding Vector Databases and Embeddings – Privacera](https://privacera.com/blog/securing-the-backbone-of-ai-safeguarding-vector-databases-and-embeddings/)
* [Enhancing Data Security with RBAC of Qdrant Vector Database – AI Advances](https://ai.gopubby.com/enhancing-data-security-with-role-based-access-control-of-qdrant-vector-database-3878769bec83)
* [Mitigating Privacy Risks in LLM Embeddings from Embedding Inversion – arXiv](https://arxiv.org/html/2411.05034v1)
* [DPPN: Detecting and Perturbing Privacy-Sensitive Neurons – OpenReview](https://openreview.net/forum?id=DF5TVzpTW0)
* [Art. 17 GDPR – Right to Erasure](https://gdpr-info.eu/art-17-gdpr/)
* [Sensitive Data in Text Embeddings Is Recoverable – Tonic.ai](https://www.tonic.ai/blog/sensitive-data-in-text-embeddings-is-recoverable)
* [PII Identification and Removal – NVIDIA NeMo Docs](https://docs.nvidia.com/nemo-framework/user-guide/latest/datacuration/personalidentifiableinformationidentificationandremoval.html)
* [De-identifying Sensitive Data – Google Cloud DLP](https://cloud.google.com/sensitive-data-protection/docs/deidentify-sensitive-data)
* [Remove PII from Conversations Using Sensitive Information Filters – AWS Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-sensitive-filters.html)
* [Think Your RAG Is Secure? Think Again – Medium](https://medium.com/%40vijay.poudel1/think-your-rag-is-secure-think-again-heres-how-to-actually-lock-it-down-c4c30e3864e7)
* [Design a Secure Multitenant RAG Inferencing Solution – Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/secure-multitenant-rag)
* [Best Practices for Multi-Tenancy RAG with Milvus – Milvus Blog](https://milvus.io/blog/build-multi-tenancy-rag-with-milvus-best-practices-part-one.md)
