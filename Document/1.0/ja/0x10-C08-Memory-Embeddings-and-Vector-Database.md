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

テキストを PII について事前に選別して、ベクトル化の前に編集または仮名化し、残留信号を除去するためにオプションでエンベディングを後処理します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.2.1** | **検証:** PII と規制データは自動分類器を介して検出され、エンベディング前にマスク、トークン化、または削除されている。 | 1 | D/V |
| **8.2.2** | **検証:** エンベディングパイプラインは、インデックスを汚染する可能性のある実行可能コードまたは非 UTF-8 アーティファクトを含む入力を拒否または隔離している。 | 1 | D |
| **8.2.3** | **検証:** ローカルまたはメトリックの差分プライバシーサニタイゼーションは、既知の PII トークンとの距離が設定可能な閾値を下回る文のエンベディングに適用されている。 | 2 | D/V |
| **8.2.4** | **検証:** サニタイゼーションの有効性 (PII 編集のリコール、意味的ドリフトなど) は、少なくとも半年ごとにベンチマークコーパスに対して検証されている。 | 2 | V |
| **8.2.5** | **検証:** サニタイゼーション設定はバージョン管理されており、変更はピアレビューを受けている。 | 3 | D/V |

---

## C8.3 メモリの有効期限、失効、削除 (Memory Expiry, Revocation & Deletion)

GDPR の「忘れられる権利」や同様の法律はタイムリーな消去を求めています。したがって、ベクトルストアは、実行したベクトルが復元されたり、再インデックス付けされたりしないように、TTL、ハード削除、トゥームストーンをサポートする必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.3.1** | **検証:** すべてのベクトルとメタデータレコードは、自動クリーンアップジョブによって尊重される、TTL または明示的な保持ラベルを有している。 | 1 | D/V |
| **8.3.2** | **検証:** ユーザーが開始した削除リクエストは、ベクトル、メタデータ、キャッシュコピー、派生インデックスを 30 日以内に削除している。 | 1 | D/V |
| **8.3.3** | **検証:** 論理削除の後は、ハードウェアがサポートしている場合にはストレージブロックの暗号シュレッディング、または key-valut のキーの破棄が行われている。 | 2 | D |
| **8.3.4** | **検証:** 期限切れのベクトルは期限切れ後の 500 ミリ秒以内に最近傍探索結果から除外されている。 | 3 | D/V |

---

## C8.4 エンベディングの反転とリークの防止 (Prevent Embedding Inversion & Leakage)

最近の防御策 (ノイズ重ね合わせ、投影ネットワーク、プライバシーニューロン摂動、アプリケーション層暗号化) はトークンレベルの反転率を 5 ％未満に削減できます。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.4.1** | **検証:** 反転攻撃、メンバーシップ攻撃、属性推論攻撃をカバーするフォーマルな脅威モデルが存在しており、毎年見直されている。 | 1 | V |
| **8.4.2** | **検証:** アプリケーション層暗号化または検索可能な暗号化は、インフラストラクチャ管理者またはクラウドスタッフによる直接読み取りから、ベクトルを保護している。 | 2 | D/V |
| **8.4.3** | **検証:** 防御パラメータ (DP の ε、ノイズ σ、投影ランク k) はプライバシー 99 % 以上のトークン保護とユーティリティ 3 % 以下の精度損失のバランスを取っている。 | 3 | V |
| **8.4.4** | **検証:** 反転耐性メトリクスはモデル更新のリリースゲートの一環であり、回帰予算が定義されている。 | 3 | D/V |

---

## C8.5 ユーザー固有メモリのスコープの強制 (Scope Enforcement for User-Specific Memory)

テナント間の漏洩は依然として RAG の最大のリスクです。不適切にフィルタされた類似性クエリが別の顧客のプライベートドキュメントを表示する可能性があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **8.5.1** | **検証:** すべての取得クエリは、LLM プロンプトに渡される前に、テナント/ユーザー ID によって事後フィルタされている。 | 1 | D/V |
| **8.5.2** | **検証:** コレクション名または名前空間 ID はユーザーまたはテナントごとにソルト化されているため、ベクトルはスコープ間で衝突できない。 | 1 | D |
| **8.5.3** | **検証:** 類似性結果は、設定可能な距離閾値を超えていて、呼び出し元のスコープ外にあると、破棄されており、セキュリティアラートをトリガーしている。 | 2 | D/V |
| **8.5.4** | **検証:** マルチテナントストレステストは、スコープ外のドキュメントを取得しようとする敵対的クエリをシミュレートしており、漏洩がゼロであることを示している。 | 2 | V |
| **8.5.5** | **検証:** 暗号鍵はテナントごとに分離されており、物理ストレージが共有されている場合でも暗号論的分離を確保している。 | 3 | D/V |

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
