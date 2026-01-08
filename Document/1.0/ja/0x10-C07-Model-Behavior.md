# C7 モデル動作、出力制御、安全保証 (Model Behavior, Output Control & Safety Assurance)

## 管理目標

このコントロールカテゴリは、モデル出力が技術的に制約、検証、監視され、安全でない、不正な、またはリスクの高いレスポンスがユーザーやダウンストリームのシステムに到達できないようにします。

---

## C7.1 出力形式の強制 (Output Format Enforcement)

モデルがインジェクションを防ぐのに役立つ方法でデータを出力するようにします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.1.1** | **検証:** アプリケーションはすべてのモデル出力を厳密なスキーマ (JSON スキーマなど) に対して検証し、一致しない出力を拒否している。 | 1 | D/V |
| **7.1.2** | **検証:** システムは、バッファをオーバーフローしたり、意図しないコマンドを実行する前に、生成を厳密に遮断するための「停止シーケンス」またはトークン制限を使用している。 | 1 | D/V |
| **7.1.3** | **検証:** モデル出力を処理するコンポーネントはそれを信頼できない入力として扱っている (パラメータ化クエリや安全なデシリアライザを使用するなど)。 | 2 | D/V |
| **7.1.4** | **検証:** システムは、出力が不正なフォーマットのために拒否された場合、特定のエラータイプをログ記録している。 | 3 | V |

---

## C7.2 ハルシネーションの検出と緩和 (Hallucination Detection & Mitigation)

モデルが不確実である場合や噓をついている場合を検出し、その情報がユーザーに届かないようにします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.2.1** | **検証:** システムは生成された回答に対して数値の信頼スコア (たとえば、対数確率を使用する) を計算している。 | 1 | D/V |
| **7.2.2** | **検証:** アプリケーションは、信頼スコアが定義された閾値を下回ると、自動的に回答をブロックするか、フォールバックメッセージに切り替えている。 | 1 | D/V |
| **7.2.3** | **検証:** ハルシネーションイベント (低い信頼性のレスポンス) は分析のために入力/出力メタデータとともにログ記録されている。 | 2 | D/V |

---

## C7.3 出力の安全性とプライバシーフィルタリング (Output Safety & Privacy Filtering)

不適切なコンテンツがユーザーに表示される前に検出して除去する技術的コントロールです。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.3.1** | **検証:** 自動分類器はすべてのレスポンスをスキャンし、ヘイト、ハラスメント、性的暴力のカテゴリに一致するコンテンツをブロックしている。 | 1 | D/V |
| **7.3.2** | **検証:** システムはすべての回答を PII (クレジットカードや電子メールなど) についてスキャンし、表示前に自動的に訂正している。 | 1 | D/V |
| **7.3.3** | **検証:** システム内で「機密」とラベル付けされたデータはブロックまたは訂正している。 | 2 | D |
| **7.3.4** | **検証:** システムは、モデルが高リスクコンテンツを生成する場合、人間による承認手順または再認証を必要としている。 | 3 | D/V |
| **7.3.5** | **検証:** 安全フィルタはユーザーの役割や場所に基づいて異なる設定を可能としている (たとえば、未成年者向けの厳格なフィルタ)。 | 3 | D/V |

---

## C7.4 出力と動作の制限 (Output & Action Limiting)

Prevent the model from doing too much, too fast, or accessing things it should not.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.4.1** | **Verify that** the system enforces hard limits on requests and tokens per user to prevent cost spikes and denial of service. | 1 | D |
| **7.4.2** | **Verify that** the model cannot execute high-impact actions (like writing files, sending emails, or executing code) without explicit user confirmation. | 1 | D/V |
| **7.4.3** | **Verify that** the agent framework explicitly configures and enforces the maximum depth of recursive calls, delegation limits, and the list of allowed external tools. | 2 | D |

---

## C7.5 説明可能性と透明性 (Explainability & Transparency)

Ensure the user knows why a decision was made.

| # | 説明 | レベル | ロール |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------ | :---: | :--: |
| **7.5.1** | **Verify that** the UI displays a confidence score or "reasoning summary" to the user for critical decisions. | 2 | D/V |
| **7.5.2** | **Verify that** explanations provided to the user are sanitized to remove system prompts or backend data. | 2 | D/V |
| **7.5.3** | **Verify that** technical evidence of the model's decision (like attention maps or log-probs) are logged.| 3 | D |

---

## C7.6 監視統合 (Monitoring Integration)

Ensure the application sends the right signals for security teams to watch.

| # | 説明 | レベル | ロール |
| :-------: | -------------------------------------------------------------------------------------------------------------------------------------------- | :---: | :--: |
| **7.6.1** | **Verify that** the system logs real-time metrics for safety violations (e.g., "Hallucination Detected", "PII Blocked").| 1 | D |
| **7.6.2** | **Verify that** the system triggers an alert if safety violation rates exceed a defined threshold within a specific time window. | 1 | V |
| **7.6.3** | **Verify that** logs include the specific model version and other details necessary to investigate potential abuse. | 2 | V |

---

## 7.7 生成メディアの安全対策 (Generative Media Safeguards)

Prevent the creation of illegal or fake media.

| # | 説明 | レベル | ロール |
| :-------: | -------------------------------------------------------------------------------------------------------------------------------------------- | :---: | :--: |
| **7.7.1** | **Verify that** the system refuses to generate media (images/audio) that depicts real people without verified consent. | 1 | D/V |
| **7.7.2** | **Verify that** input filters block prompts requesting explicit or deepfake content before the model processes them. | 2 | D/V  |
| **7.7.3** | **Verify that** the system checks generated content for copyright violations before releasing it. | 2 | V |
| **7.7.4** | **Verify that** all generated media includes an invisible watermark or cryptographic signature to prove it was AI-generated. | 3 | D/V  |
| **7.7.5** | **Verify that** attempts to bypass filters are detected and logged as security events. | 3 | V |

## 参考情報

* [OWASP Top 10 for LLMs: LLM07: Insecure Output Handling](https://owasp.org/www-project-top-10-for-large-language-model-applications/#llm07-insecure-output-handling)
