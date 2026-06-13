# C8 メモリ、エンベディング、ベクトルデータベースセキュリティ (Memory, Embeddings & Vector Database Security)

## 管理目標

エンベディングとベクトルストアは検索拡張生成 (Retrieval-Augmented Generation, RAG) を介して AI システムの半永続的および永続的な「メモリ」として機能します。このメモリは高リスクのデータシンクやデータ抽出パスとなる可能性があります。このコントロールファミリーはメモリパイプラインやベクトルデータベースを強化し、アクセスが最小権限であり、ベクトル化前にデータがサニタイズされ、保持が明示的であり、システムがエンベディング反転、メンバーシップ推論、テナント間漏洩に耐性があるようにします。

---

## C8.1 メモリと RAG インデックスのアクセス制御 (Access Controls on Memory & RAG Indices)

すべてのベクトルコレクションに対してきめ細かなアクセス制御とクエリ時のスコープ適用を強制します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.1.1** | **検証:** ベクトルの挿入、更新、削除、クエリ操作はデフォルトで拒否された名前空間/コレクション/ドキュメントタグのスコープ制御 (テナント ID、ユーザー ID、データ分類ラベルなど) で強制されている。 | 1 |
| **8.1.2** | **検証:** 取り込まれたすべてのドキュメントは書き込み時に、ソース、書き込み者アイデンティティ (認証済みユーザーまたはシステムプリンシパル)、タイムスタンプ、バッチ ID、エンベディングモデルのバージョンでタグ付けされている。 | 2 |
| **8.1.3** | **Verify that** document metadata tags applied at ingestion are immutable after the initial write and cannot be changed by later pipeline stages or user operations. | 2 |
| **8.1.4** | **検証:** RAG パイプラインの取得イベントは、発行されたクエリ、取得されたドキュメントやチャンク、類似度スコア、知識ソースをログ記録している。 | 2 |
| **8.1.5** | **Verify that** a high-severity security alert is generated whenever a canary record is selected by retrieval, matched by similarity search, or passed to the model as context. | 2 |
| **8.1.6** | **検証:** 検索異常検出は、エンベディング密度の外れ値、類似度結果における特定のドキュメントの繰り返し優位、およびベクトルデータベースポイズニングを示唆する可能性のある検索バイアス分布の急激な変化を特定している。 | 3 |

---

## C8.2 エンベディングのサニタイゼーションとバリデーション (Embedding Sanitization & Validation)

ベクトル化前にコンテンツを事前スクリーンし、メモリ書き込みを信頼できない入力として扱い、安全でないペイロードの取り込みを防止します。Hidden instructions and other prompt-injection payloads in ingested or retrieved content are screened by the same input-validation pipeline as direct user input.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.2.1** | **検証:** 一度埋め込まれたデータは結果として得られるインデックスから確実に訂正できないため、機密フィールドはエンベディング前に検出され、マスク、トークン化、変換、または削除されている。 | 1 |
| **8.2.2** | **Verify that** content crafted to manipulate retrieval geometry (e.g., text engineered to project into attacker-chosen embedding neighborhoods so it is preferentially retrieved) is detected and rejected or quarantined before vectorization. | 3 |
 **8.2.3** | **検証:** 通常のクラスタリングパターンを外れるベクトルは、プロダクションインデックスに入る前に、フラグ付けされ、隔離されている。 | 2 |
| **8.2.4** | **検証:** エージェント出力、ツール出力、オーケストレーション結果は、明示的なソースバリデーション (書き込みをコミットする前にコンテンツのソースを検証する、コンテンツオリジンのチェックや書き込み認可制御など) なしでは、信頼できるエージェントメモリに自動的に書き戻されることはない。 | 2 |
| **8.2.5** | **検証:** メモリに書き込漏れた新しいコンテンツはすでに保存されているものとの矛盾がないかチェックされ、矛盾があるとアラートをトリガーしている。 | 3 |

---

## C8.3 メモリの有効期限、失効、削除 (Memory Expiry, Revocation & Deletion)

Retention and revocation must be explicit and enforceable for memory and RAG indices, and deletions must propagate through derivative indices and caches within a measured propagation window.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.3.1** | **検証:** 期限切れのベクトルは、定義された伝播ウィンドウ内の検索結果から除外されている。 | 2 |
| **8.3.2** | **検証:** メモリは、保持削除とは別個独立した操作を通じて、セキュリティ上の理由 (隔離、選択的消去、完全リセット) でリセットできる。 | 2 |
| **8.3.3** | **検証:** 隔離されたコンテンツは調査のために保持されているが、隔離されている間は検索結果から除外されている。 | 2 |

---

## C8.4 エンベディングの反転とリークの防止 (Prevent Embedding Inversion & Leakage)

明示的な脅威モデリング、緩和策、回帰テストゲートで反転、メンバーシップ推論、属性推論を対処します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.4.1** | **検証:** エンベディング漏洩耐性のプライバシー/ユーティリティターゲットは **定義および測定** されており、エンベディングモデル、トークナイザ、検索設定、プライバシー変換の変更はそれらのターゲットに対する回帰テストによってゲート制御されている。 | 3 |

---

## C8.5 ユーザー固有メモリのスコープの強制 (Scope Enforcement for User-Specific Memory)

検索およびプロンプトアセンブリでのテナント間およびユーザー間の漏洩を防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **8.5.1** | **検証:** すべての検索操作は **ベクトルエンジンクエリでの** スコープ制約 (テナント/ユーザー/分類) を適用し、**プロンプトアセンブリの前に** (ポストフィルター) 再度検証している。 | 2 |
| **8.5.2** | **検証:** ベクトル識別子、名前空間、メタデータインデックスはスコープ間の衝突を防ぎ、テナントごとに一意性を強制している。 | 1 |
| **8.5.3** | **検証:** 類似基準に一致するがスコープチェックに失敗した検索結果は破棄されている。 | 1 |
| **8.5.4** | **検証:** マルチテナントテストは敵対的検索試行 (プロンプトベースおよびクエリベース) をシミュレートし、プロンプトや出力にスコープ外のドキュメントが現れないことを示している。 | 2 |

---

## 参考情報

* [OWASP LLM08:2025 Vector and Embedding Weaknesses](https://genai.owasp.org/llmrisk/llm082025-vector-and-embedding-weaknesses/)
* [OWASP LLM04:2025 Data and Model Poisoning](https://genai.owasp.org/llmrisk/llm042025-data-and-model-poisoning/)
* [OWASP LLM02:2025 Sensitive Information Disclosure](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/)
* [MITRE ATLAS: RAG Poisoning](https://atlas.mitre.org/techniques/AML.T0070)
* [MITRE ATLAS: False RAG Entry Injection](https://atlas.mitre.org/techniques/AML.T0071)
* [MITRE ATLAS: Infer Training Data Membership](https://atlas.mitre.org/techniques/AML.T0024.000)
* [MITRE ATLAS: Invert AI Model](https://atlas.mitre.org/techniques/AML.T0024.001)
