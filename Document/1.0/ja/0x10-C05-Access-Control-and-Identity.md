# C5 AI コンポーネントとユーザーのアクセス制御とアイデンティティ (Access Control & Identity for AI Components & Users)

## 管理目標

AI systems introduce access control challenges beyond traditional application security: classification labels must follow data through AI-specific transformations (embeddings, caches, model outputs), multi-tenant inference infrastructure creates novel side channels, and retrieval-augmented pipelines must enforce caller entitlements at every stage. This chapter addresses AI-specific access control and identity concerns, including runtime isolation of the policy decision point from agent execution and authorization-aware output filtering where entitlements vary per caller.

---

## C5.1 Authentication

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.1.1** | **検証:** 高リスクの AI 操作 (モデルのデプロイメント、重みのエクスポート、トレーニングデータへのアクセス、本番構成の変更) は、ステップアップ認証を必要としている。 | 3 |
| **5.1.2** | **検証:** 連合またはマルチシステムデプロイメントの AI エージェントは、有効期間が短く、最小限のスコープで、暗号で署名されたトークンを使用して認証している。 | 3 |

---

## C5.2 AI Resource Authorization & Classification

Enforce the caller's authorization context through AI-specific query pipelines (RAG retrieval, embedding lookups, inference chains) so that the AI system does not return data that the caller is not entitled to access.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.2.1** | **検証:** すべての AI リソース (データセット、エンドポイント、ベクトルコレクション、エンベディングインデックス、計算インスタンス) は、明示的な許可リストとデフォルト拒否ポリシーで、アクセス制御を適用している。 | 2 |
| **5.2.2** | **Verify that** retrieval pipelines (e.g., RAG queries, embedding lookups) enforce the end-user's authorization context at each retrieval and assembly stage, rather than relying solely on the service account's permissions. | 2 |
| **5.2.3** | **Verify that** sensitive data is retrieved via retrieval pipelines (e.g., RAG queries, embedding lookups) to prevent permanent storage in models. | 2 |
| **5.2.4** | **Verify that** post-inference filtering mechanisms prevent responses from including data that the requestor is not authorized to receive. | 2 |
| **5.2.5** | **Verify that** the policy decision point for agent authorization is isolated from the agent's execution environment. | 2 |
| **5.2.6** | **Verify that** privileged access to model weights, training pipelines, and production AI configuration is granted just in time, with a defined maximum session duration and automatic expiry. Zero Standing Privilege (ZSP) to these resources is encouraged. | 3 |
| **5.2.7** | **検証:** データ分類ラベルはダウンストリームリソース (エンベディング、プロンプトキャッシュ、モデル出力) に伝播している。 | 3 |

---

## C5.3 マルチテナントの分離 (Multi-Tenant Isolation)

Prevent cross-tenant information leakage through AI-specific shared infrastructure components such as inference caches and shared model state.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.3.1** | **Verify that** shared model serving infrastructure prevents one tenant's fine-tuning, inference, or embedding operations from influencing or observing another tenant's operations. | 2 |
| **5.3.2** | **Verify that** one tenant cannot influence or observe another tenant's operations through shared compute resources. Satisfying this requirement typically requires hardware partitioning, confidential computing, or dedicated per-tenant compute allocation. | 3 |

---

## 参考情報

* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
* [NIST SP 800-63-3: Digital Identity Guidelines](https://csrc.nist.gov/pubs/sp/800/63/3/final)
* [I Know What You Asked: Prompt Leakage via KV-Cache Sharing in Multi-Tenant LLM Serving (NDSS 2025)](https://www.ndss-symposium.org/ndss-paper/i-know-what-you-asked-prompt-leakage-via-kv-cache-sharing-in-multi-tenant-llm-serving/)
