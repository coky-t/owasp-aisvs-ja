# C7 モデル動作、出力制御、安全保証 (Model Behavior, Output Control & Safety Assurance)

## 管理目標

このコントロールカテゴリは、モデル出力が技術的に制約、検証、監視され、安全でない、不正な、またはリスクの高いレスポンスがユーザーやダウンストリームのシステムに到達できないようにします。 The chapter focuses on AI-specific output handling concerns and intentionally avoids duplicating controls that already exist in OWASP ASVS v5 or in other AISVS chapters.

General application output controls such as output encoding and escaping, parameterized queries, safe deserialization, anti-automation, security event logging, and error handling are addressed by ASVS v5 chapters V1, V2, V14, and V16.

---

## C7.1 出力形式の強制 (Output Format Enforcement)

モデルがインジェクションを防ぐのに役立つ方法でデータを出力するようにします。

> **Scope note:** 7.1.1 applies only to applications that expect structured output (e.g., JSON, XML, typed function-call responses). Free-form text outputs are out of scope for schema validation.

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **7.1.1** | **検証:** アプリケーションはすべてのモデル出力を厳密なスキーマ (JSON スキーマなど) に対して検証し、一致しない出力を拒否している。 | 1 |
| **7.1.2** | **検証:** システムは、バッファをオーバーフローしたり、意図しないコマンドを実行する前に、生成を厳密に遮断するための「停止シーケンス」またはトークン制限を使用している。 | 1 |
| **7.1.3** | **Verify that** model outputs crossing a trust boundary into downstream interpreters (e.g., databases, shells, deserializers, template engines, browsers) are treated as untrusted input and processed using the corresponding safe APIs as defined in OWASP ASVS v5 chapters V1.2 and V1.5. | 1 |

---

## C7.2 ハルシネーションの検出と緩和 (Hallucination Detection & Mitigation)

モデルが不正確または捏造された可能性のあるコンテンツを生成する場合を検出し、信頼できない出力がユーザーやダウンストリームシステムに届かないようにします。

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **7.2.1** | **検証:** システムは、信頼性または不確実性の推定手法 (信頼性スコアリング、検索ベースの検証、モデル不確実性の推定など) を使用して、生成された回答の信頼性を評価している。 | 1 |
| **7.2.2** | **検証:** アプリケーションは、信頼スコアが定義された閾値を下回ると、自動的に回答をブロックするか、フォールバックメッセージに切り替えている。 | 2 |
| **7.2.3** | **検証:** ハルシネーションイベント (低い信頼性のレスポンス) は分析のために入力/出力メタデータとともにログ記録されている。 | 2 |
| **7.2.4** | **Verify that** the system tracks tool and function invocation history within a request chain and flags high-confidence factual assertions that were not preceded by relevant verification tool usage, as a practical hallucination detection signal independent of confidence scoring. | 2 |
| **7.2.5** | **検証:** ポリシーによって高リスクまたは高影響として分類されたレスポンスについて、システムは、信頼できるソースに対する検索ベースのグラウンディング、決定論的なルールベースのバリデーション、ツールベースのファクトチェック、個別に提供されるモデルによる合意レビューなど、独立したメカニズムを通じて追加の検証ステップを実行している。 | 3 |

---

## C7.3 出力の安全性とプライバシーフィルタリング (Output Safety & Privacy Filtering)

不適切なコンテンツがユーザーに表示される前に検出して除去する技術的コントロールです。

| # | 説明 | レベル |
| :--------: | --------------------------------------------------------------------------------------------------------------------- | :---: |
| **7.3.1** | **検証:** 自動分類器はすべてのレスポンスをスキャンし、ヘイト、ハラスメント、性的暴力のカテゴリに一致するコンテンツをブロックしている。 | 1 |
| **7.3.2** | **Verify that** output filters detect and block responses that disclose system prompt content, including verbatim reproduction and semantically equivalent paraphrases of instructions, role definitions, or policy directives. | 2 |
| **7.3.3** | **Verify that** LLM client applications prevent model-generated output from triggering automatic outbound requests (e.g., auto-rendered images, iframes, or link prefetching) to attacker-controlled endpoints, for example by disabling automatic external resource loading or restricting it to explicitly allowlisted origins as appropriate. | 2 |
| **7.3.4** | **Verify that** generated outputs are analyzed for statistical steganographic covert channels (e.g., biased token-choice patterns or output distribution anomalies) that could encode hidden data across the model's valid output space, and that detections are flagged for review. | 3 |

---

## C7.4 説明可能性と透明性 (Explainability & Transparency)

ユーザーが決定の理由を理解していることを確認します。

| # | 説明 | レベル |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------ | :---: |
| **7.4.1** | **検証:** ユーザーに提供される説明はシステムプロンプトやバックエンドデータを削除するようにサニタイズされている。 | 1 |
| **7.4.2** | **検証:** モデルの解釈可能性アーティファクト (アテンションマップ、特徴属性など) のような、モデルの決定の技術的証跡がログ記録されている。 | 3 |

---

## C7.5 生成メディアの安全対策 (Generative Media Safeguards)

Provide cryptographic provenance for synthetic media so that downstream consumers can distinguish AI-generated content from authentic content.

| # | 説明 | レベル |
| :-------: | -------------------------------------------------------------------------------------------------------------------------------------------- | :---: |
| **7.5.1** | **検証:** 生成されたすべてのメディアは、AI によって生成されたことを証明する、不可視の透かしまたは暗号署名を含んでいる。 | 3 |

---

## C7.6 Source Attribution & Citation Integrity

Ensure RAG-grounded outputs are traceable to their source documents and that cited claims are verifiably supported by retrieved content.

| # | 説明 | レベル |
| :-------: | -------------------------------------------------------------------------------------------------------------------------------------------- | :---: |
| **7.6.1** | **Verify that** responses generated using retrieval-augmented generation (RAG) include attribution to the source documents that grounded the response. | 1 |
| **7.6.2** | **Verify that** RAG attributions are derived from retrieval metadata and are not generated by the model, ensuring provenance cannot be fabricated. | 1 |
| **7.6.3** | **Verify that** each sourced claim in a RAG-grounded response can be traced to a specific retrieved chunk, and that the system detects and flags responses where claims are not supported by any retrieved content before the response is served. | 3 |
| **7.6.4** | **Verify that** RAG responses in which unsupported claims are detected are blocked or redacted before being served to the user. | 3 |

---

## 参考情報

* [OWASP LLM05:2025 Improper Output Handling](https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/)
* [OWASP LLM06:2025 Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)
