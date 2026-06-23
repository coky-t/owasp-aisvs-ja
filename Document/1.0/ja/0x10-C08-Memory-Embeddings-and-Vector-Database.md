# C8 メモリ、エンベディング、ベクトルデータベースセキュリティ (Memory, Embeddings & Vector Database Security)

## 管理目標

この章は、検索拡張生成 (Retrieval-Augmented Generation, RAG) を介して AI システムの半永続的および永続的な「メモリ」として機能するエンベディングとベクトルストアの保護を取り扱います。

---

## C8.1 メモリと RAG インデックスのアクセス制御 (Access Controls on Memory & RAG Indices)

きめ細かなアクセス制御とクエリ時のスコープ適用がすべてのベクトルコレクションに対して適用される必要があります。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.1.1** | **Verify that** vector identifiers and namespaces enforce uniqueness per tenant and prevent cross-tenant collisions. | 1 |
| **8.1.2** | **Verify that** document metadata tags are immutable after the initial write. | 2 |
| **8.1.3** | **Verify that** retrieval operations enforce scope constraints. | 2 |

---

## C8.2 エンベディングのサニタイゼーションとバリデーション (Embedding Sanitization & Validation)

コンテンツはベクトル化前に事前スクリーンされる必要があり、メモリ書き込みを信頼できない入力として扱い、安全でないペイロードの取り込みを防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.2.1** | **検証:** 機密フィールドはエンベディング前に検出され、マスク、トークン化、または削除されている。 | 1 |
| **8.2.2** | **検証:** 通常のクラスタリングパターンを外れるベクトルは、プロダクションインデックスに入る前に、フラグ付けされ、隔離されている。 | 2 |
| **8.2.3** | **検証:** エージェント出力とツール出力は、明示的なソースバリデーションなしでは、信頼できるエージェントメモリに自動的に書き込まれることはない。 | 2 |
| **8.2.4** | **Verify that** content crafted to manipulate retrieval results is detected and rejected or quarantined before vectorization. | 3 |
| **8.2.5** | **検証:** メモリに書き込漏れた新しいコンテンツはすでに保存されているものとの矛盾がないかチェックされ、矛盾があるとアラートをトリガーしている。 | 3 |

---

## C8.3 メモリの有効期限と失効 (Memory Expiry & Revocation)

Retention and revocation must be explicit and enforceable for memory and RAG indices.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.3.1** | **検証:** 期限切れのベクトルは検索結果から除外されている。 | 2 |
| **8.3.2** | **検証:** メモリはリセットできる。 | 2 |
| **8.3.3** | **検証:** 隔離されたコンテンツは保持されているが、すべての検索結果から除外されている。 | 2 |

---

## 参考情報

* [OWASP LLM08:2025 Vector and Embedding Weaknesses](https://genai.owasp.org/llmrisk/llm082025-vector-and-embedding-weaknesses/)
* [OWASP LLM04:2025 Data and Model Poisoning](https://genai.owasp.org/llmrisk/llm042025-data-and-model-poisoning/)
* [OWASP LLM02:2025 Sensitive Information Disclosure](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/)
* [MITRE ATLAS: RAG Poisoning](https://atlas.mitre.org/techniques/AML.T0070)
* [MITRE ATLAS: Infer Training Data Membership](https://atlas.mitre.org/techniques/AML.T0024.000)
