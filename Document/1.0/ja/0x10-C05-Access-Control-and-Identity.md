# C5 AI コンポーネントとユーザーのアクセス制御とアイデンティティ (Access Control & Identity for AI Components & Users)

## 管理目標

AI systems introduce access control challenges beyond traditional application security: classification labels must follow data through AI-specific transformations (embeddings, caches, model outputs), multi-tenant inference infrastructure creates novel side channels, and retrieval-augmented pipelines must enforce caller entitlements at every stage. 

This chapter addresses AI-specific access control and identity concerns only. General identity management and authentication (centralized IdP, federation, MFA, step-up authentication) are covered by ASVS v5 V6. General authorization patterns (RBAC/ABAC design, externalized PDP, dynamic attribute evaluation, policy caching), access control audit logging, and multi-tenant networking are covered by ASVS v5 V8, V14, and V16. 

Agent-specific authorization policies and delegation are covered in C9.6; single-system agent identity is in C9.4.1; this chapter covers runtime isolation of the policy decision point from agent execution (complementing C9.6.3). Vector database scope enforcement is in C8.1 and C8.5. General output safety filtering (PII redaction, content moderation, confidential data blocking) is in C7.3; this chapter covers authorization-aware output filtering where entitlements vary per caller.

---

## C5.1 Authentication

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.1.1** | **検証:** 高リスクの AI 操作 (モデルのデプロイメント、重みのエクスポート、トレーニングデータへのアクセス、本番構成の変更) は、セッション再バリデーションでのステップアップ認証を必要としている。 | 2 |
| **5.1.2** | **検証:** 連合またはマルチシステムデプロイメントの AI エージェントは、リスクレベルに適した最大有効期間を持ち、オリジンの暗号論的証明を含む、有効期間が短く、暗号で署名された認証トークン (署名付き JWT アサーションなど) を介して認証している。 | 3 |

---

## C5.2 AI Resource Authorization & Classification

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.2.1** | **検証:** すべての AI リソース (データセット、モデル、エンドポイント、ベクトルコレクション、エンベディングインデックス、計算インスタンス) は、明示的な許可リストとデフォルト拒否ポリシーで、アクセス制御 (RBAC, ABAC など) を適用している。 | 1 |
| **5.2.2** | **Verify that** privileged access to model weights, training pipelines, and production AI configuration is provisioned on a just-in-time basis with a defined maximum session duration and automatic expiry, and permanent standing privileged access to these resources is not permitted. | 2 |
| **5.2.3** | **Verify that** a documented data classification taxonomy covering AI-specific data types (embeddings, model weights, prompt templates, RAG context assemblies, fine-tuning datasets, agent tool schemas) is defined, and that AI assets are labeled in accordance with this taxonomy. | 2 |
| **5.2.4** | **検証:** データ分類ラベル (PII、PHI、独自など) は派生リソース (エンベディング、プロンプトキャッシュ、モデル出力) に自動的に伝播している。 | 3 |

---

## C5.3 Query-Time Authorization

Enforce the caller's authorization context through AI-specific query pipelines (RAG retrieval, embedding lookups, inference chains) so that the AI system does not return data the caller is not entitled to access.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.3.1** | **Verify that** AI inference and retrieval pipelines (e.g., RAG queries, embedding lookups) enforce the end-user's authorization context at each retrieval and assembly stage, rather than relying solely on the service account's permissions. | 1 |

---

## C5.4 Output Entitlement Enforcement

Ensure that AI-generated outputs, including citations and source attributions, respect the caller's data entitlements.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.4.1** | **検証:** 推論後フィルタリングメカニズムは、要求者が受信することを認可されていない機密情報や専有データをレスポンスに含むことを防いでいる。 | 1 |
| **5.4.2** | **検証:** モデル出力内の引用、参照、ソース属性は呼び出し元のエンタイトルメントに対して検証されており、不正アクセスが検出された場合は削除されている。 | 2 |

---

## C5.5 Policy Decision Point Isolation

Ensure that authorization decision infrastructure for AI agents is protected from compromise and manipulation by the agents it governs.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.5.1** | **Verify that** the policy decision point for agent authorization is isolated from the agent's execution environment such that a compromised agent runtime cannot influence or bypass evaluation. | 3 |
| **5.5.2** | **Verify that** the policy decision point receives structured action descriptions (e.g., action type, target resource, parameters) rather than the agent's raw reasoning context. | 3 |

---

## C5.6 マルチテナントの分離 (Multi-Tenant Isolation)

Prevent cross-tenant information leakage through AI-specific shared infrastructure components such as inference caches and shared model state.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------- | :---: |
| **5.6.1** | **検証:** 推論時の KV キャッシュエントリは認証されたセッションまたはテナントアイデンティティによって分割され、自動プレフィックスキャッシュは、タイミングベースのプロンプト再構築攻撃を防ぐために、異なるセキュリティプリンシパル間ではキャッシュされたプレフィックスを共有していない。 | 2 |
| **5.6.2** | **Verify that** shared model serving infrastructure prevents one tenant's fine-tuning, inference, or embedding operations from influencing or observing another tenant's operations through shared model state, adapter weights, or compute resources. | 2 |

---

## References

* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/detail/sp/800-207/final)
* [NIST SP 800-63-3: Digital Identity Guidelines](https://csrc.nist.gov/pubs/sp/800/63/3/final)
* [I Know What You Asked: Prompt Leakage via KV-Cache Sharing in Multi-Tenant LLM Serving (NDSS 2025)](https://www.ndss-symposium.org/ndss-paper/i-know-what-you-asked-prompt-leakage-via-kv-cache-sharing-in-multi-tenant-llm-serving/)
