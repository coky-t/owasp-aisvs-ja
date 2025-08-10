# C2 ユーザー入力バリデーション (User Input Validation)

## 管理目標

ユーザー入力の堅牢なバリデーションは AI システムで最もダメージのある攻撃のいくつかに対する最前線の防御策です。プロンプトインジェクション攻撃は、システム命令を上書きしたり、機密データを漏洩したり、モデルを許可されていない動作に誘導する可能性があります。専用のフィルタと命令階層が配備されていない限り、非常に長いコンテキストウィンドウを悪用する「マルチショット」ジェイルブレイクが有効になることが研究で示されています。また、ホモグリフスワップやリートスピークなどの巧妙な敵対的摂動攻撃はモデルの決定を静かに変更する可能性があります。

---

## C2.1 プロンプトインジェクションの防御 (Prompt Injection Defense)

プロンプトインジェクションは AI システムにとって最大のリスクの一つです。この戦術に対する防御策として、静的パターンフィルタ、動的分類器、命令階層適用を組み合わせて採用します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.1.1** | **検証:** ユーザー入力は、継続的に更新される既知のプロンプトインジェクションパターン (ジェイルブレイクキーワード、「前のものを無視」、ロールプレイチェーン、間接 HTML/URL 攻撃) のライブラリに対してスクリーニングされている。 | 1 |  D/V |
| **2.1.2** | **検証:** システムは、コンテキストウィンドウ拡張後でも、システムメッセージや開発者メッセージがユーザー命令を上書きする、命令階層を適用している。 | 1 |  D/V |
| **2.1.3** | **検証:** 敵対的評価テスト (レッドチームの「メニーショット」プロンプトなど) は、すべてのモデルまたはプロンプトテンプレートのリリース前に、成功率の閾値と回帰の自動ブロッカーを使用して、実行されている。 | 2 |  D/V |
| **2.1.4** | **検証:** サードパーティコンテンツ (ウェブページ、PDF、電子メール) から生成されたプロンプトは、メインプロンプトに連結される前に、隔離された解析コンテキストでサニタイズされている。 | 2 | D |
| **2.1.5** | **検証:** すべてのプロンプトフィルタルールの更新、分類モデルのバージョン、ブロックリストの変更はバージョン管理され、監査可能である。 | 3 |  D/V |

---

## C2.2 敵対的例示への耐性 (Adversarial-Example Resistance)

自然言語処理 (NLP) モデルは、人間が見逃しがちだがモデルは誤分類する傾向がある、文字や単語レベルの微妙な摂動に対して依然として脆弱です。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.2.1** | **検証:** 基本的な入力正規化手順 (Unicode NFC、ホモグリフマッピング、空白トリミング) はトークン化の前に実行している。 | 1 | D |
| **2.2.2** | **検証:** 統計的異常検出は、言語標準への編集距離が異常に高い入力、トークンが過剰に繰り返される入力、エンベディング距離が異常な入力にフラグを立てている。 | 2 |  D/V |
| **2.2.3** | **検証:** 推論パイプラインは、高リスクのエンドポイントに対して、オプションで敵対的トレーニング強化モデルのバリアントや防御レイヤ (ランダム化、防御蒸留など) をサポートしている。 | 2 | D |
| **2.2.4** | **検証:** 敵対的であることが疑われる入力は隔離され、(PII リダクション後) 完全なペイロードとともにログ記録されている。 | 2 | V |
| **2.2.5** | **検証:** 堅牢性マトリクス (既知の攻撃スイートの成功率) は時間の経過とともに追跡され、回帰はリリースブロッカーをトリガーしている。 | 3 |  D/V |

---

## C2.3 スキーマ、タイプ、長さのバリデーション (Schema, Type & Length Validation)

不正な形式の入力や大きすぎる入力を特徴とする AI 攻撃は、解析エラー、フィールド間のプロンプトの流出、リソース枯渇を引き起こす可能性があります。決定論的なツール呼び出しを実行する場合も、厳格なスキーマ適用が前提条件になります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.3.1** | **検証:** すべての API や関数呼び出しエンドポイントは明示的な入力スキーマ (JSON スキーマ、Protobuf、またはマルチモーダル同等物) を定義し、プロンプトアセンブリの前に入力が検証されている。 | 1 | D |
| **2.3.2** | **検証:** 最大トークンやバイト制限を超える入力は、安全なエラーで拒否され、暗黙的に切り捨てられることはない。 | 1 |  D/V |
| **2.3.3** | **検証:** 型チェック (数値範囲、列挙値、画像/音声の MIME タイプなど) はクライアントコードだけでなくサーバーサイドでも強制されている。 | 2 |  D/V |
| **2.3.4** | **検証:** セマンティックバリデータ (JSON スキーマなど) は、アルゴリズムによる DoS を防ぐために、一定時間で実行している。 | 2 | D |
| **2.3.5** | **検証:** バリデーションの失敗は、セキュリティトリアージを支援するために、リダクション後のペイロードスニペットと正確なエラーコードとともにログ記録されている。 | 3 | V |

---

## C2.4 コンテンツとポリシーの審査 (Content & Policy Screening)

開発者は、許可されていないコンテンツ (不正な命令、ヘイトスピーチ、著作権で保護されたテキストなど) を要求する構文的に有効なプロンプトを検出し、それらの拡散を防ぐことができる必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.4.1** | **検証:** コンテンツ分類器 (ゼロショットまたはファインチューンされている) は、設定可能な閾値で、すべての値に、暴力、自傷、ヘイト、性的コンテンツ、不正なリクエストについてスコア付けしている。 | 1 | D |
| **2.4.2** | **検証:** ポリシーに違反する入力は標準化された拒否または安全な完了を受け取るため、ダウンストリームの LLM 呼び出しに伝播していない。 | 1 |  D/V |
| **2.4.3** | **検証:** スクリーニングモデルやルールセットは、新たに観察されたジェイルブレイクやポリシーバイパスのパターンを組み込んで、少なくとも四半期ごとに再トレーニング/アップデートされている。 | 2 | D |
| **2.4.4** | **検証:** スクリーニングは、リクエスト時に解決される属性ベースのルールを介して、ユーザー固有のポリシー (年齢、地域の法的制約) を尊重している。 | 2 | D |
| **2.4.5** | **検証:** スクリーニングログは SOC 相関と将来のレッドチームのリプレイのために分類器の信頼スコアとポリシーカテゴリを含んでいる。 | 3 | V |

---

## C2.5 入力レート制限と不正使用防止 (Input Rate Limiting & Abuse Prevention)

開発者は、入力レートを制限し、異常な使用パターンを検出することで、AI システムに対する不正使用、リソース枯渇、自動攻撃を防ぐ必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.5.1** | **検証:** ユーザーごと、IP ごと、API キーごとのレート制限は、すべての入力エンドポイントに対して適用されている。 | 1 | D/V |
| **2.5.2** | **検証:** バーストおよび持続レート制限は、DoS 攻撃やブルートフォース攻撃を防ぐために調整されている。 | 2 | D/V |
| **2.5.3** | **検証:** 異常な使用パターン (連続したリクエスト、入力フラッディングなど) は自動ブロックまたはエスカレーションをトリガーしている。 | 2 | D/V |
| **2.5.4** | **検証:** 不正使用防止ログは保持され、新たな攻撃パターンがないかレビューされている。 | 3 | V |

---

## C2.6 マルチモーダル入力バリデーション (Multi-Modal Input Validation)

AI システムは、インジェクション、回避、リソース不正使用を防ぐために、非テキスト入力 (画像、音声、ファイル) に対する堅牢なバリデーションを含む必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.6.1** | **検証:** すべての非テキスト入力 (画像、音声、ファイル) は処理前にタイプ、サイズ、形式について検証されている。 | 1 | D |
| **2.6.2** | **検証:** ファイルは取り込む前にマルウェアやステガノグラフィのペイロードについてスキャンされている。 | 2 | D/V |
| **2.6.3** | **検証:** 画像/音声入力は敵対的な摂動や既知の攻撃パターンについてチェックされている。 | 2 | D/V |
| **2.6.4** | **検証:** マルチモーダル入力バリデーションの失敗はログ記録され、調査のためのアラートをトリガーしている。 | 3 | V |

---

## C2.7 入力の来歴と属例 (Input Provenance & Attribution)

AI システムは、すべてのユーザー入力の発生元を監視してタグ付けすることで、監査、不正使用の追跡、コンプライアンスをサポートする必要があります。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.7.1** | **検証:** すべてのユーザー入力は取り込み時にメタデータ (ユーザーID、セッション、ソース、タイムスタンプ、IP アドレス) でタグ付けされている。 | 1 | D/V |
| **2.7.2** | **検証:** 来歴メタデータが保持され、すべての処理された入力について監査可能である。 | 2 | D/V |
| **2.7.3** | **検証:** 異常な入力ソースや信頼できない入力ソースはフラグ付けされ、強化された精査またはブロックの対象としている。 | 2 | D/V |

---

## C2.8 リアルタイム適応型脅威検出 (Real-Time Adaptive Threat Detection)

Developers should employ advanced threat detection systems for AI that adapt to new attack patterns and provide real-time protection with compiled pattern matching.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.8.1** | **Verify that** threat detection patterns are compiled into optimized regex engines for high performance real-time filtering with minimal latency impact. | 1 | D/V |
| **2.8.2** | **Verify that** threat detection systems maintain separate pattern libraries for different threat categories (prompt injection, harmful content, sensitive data, system commands). | 1 | D/V |
| **2.8.3** | **Verify that** adaptive threat detection incorporates machine learning models that update threat sensitivity based on attack frequency and success rates. | 2 | D/V |
| **2.8.4** | **Verify that** real-time threat intelligence feeds automatically update pattern libraries with new attack signatures and IOCs (Indicators of Compromise). | 2 | D/V |
| **2.8.5** | **Verify that** threat detection false positive rates are continuously monitored and pattern specificity is automatically tuned to minimize legitimate use case interference. | 3 | D/V |
| **2.8.6** | **Verify that** contextual threat analysis considers input source, user behavior patterns, and session history to improve detection accuracy. | 3 | D/V |
| **2.8.7** | **Verify that** threat detection performance metrics (detection rate, processing latency, resource utilization) are monitored and optimized in real-time. | 3 | D/V |

---

## C2.9 マルチモーダルセキュリティバリデーションパイプライン (Multi-Modal Security Validation Pipeline)

Developers should provide security validation for text, image, audio, and other AI input modalities with specific types of threat detection and resource isolation.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.9.1** | **Verify that** each input modality has dedicated security validators with documented threat patterns (text: prompt injection, images: steganography, audio: spectrogram attacks) and detection thresholds. | 1 | D/V |
| **2.9.2** | **Verify that** multi-modal inputs are processed in isolated sandboxes with defined resource limits (memory, CPU, processing time) specific to each modality type and documented in security policies. | 2 | D/V |
| **2.9.3** | **Verify that** cross-modal attack detection identifies coordinated attacks spanning multiple input types (e.g., steganographic payloads in images combined with prompt injection in text) with correlation rules and alert generation. | 2 | D/V |
| **2.9.4** | **Verify that** multi-modal validation failures trigger detailed logging including all input modalities, validation results, threat scores, and correlation analysis with structured log formats for SIEM integration. | 3 | D/V |
| **2.9.5** | **Verify that** modality-specific content classifiers are updated according to documented schedules (minimum quarterly) with new threat patterns, adversarial examples, and performance benchmarks maintained above baseline thresholds. | 3 | D/V |

---

## 参考情報

* [LLM01:2025 Prompt Injection – OWASP Top 10 for LLM & Generative AI Security](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
* [Generative AI's Biggest Security Flaw Is Not Easy to Fix](https://www.wired.com/story/generative-ai-prompt-injection-hacking)
* [Many-shot jailbreaking \ Anthropic](https://www.anthropic.com/research/many-shot-jailbreaking)
* [$PDF$ OpenAI GPT-4.5 System Card](https://cdn.openai.com/gpt-4-5-system-card-2272025.pdf)
* [Notebook for the CheckThat Lab at CLEF 2024](https://ceur-ws.org/Vol-3740/paper-53.pdf)
* [Mitigate jailbreaks and prompt injections – Anthropic](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks)
* [Chapter 3 MITRE ATT\&CK – Adversarial Model Analysis](https://ama.drwhy.ai/mitre-attck.html)
* [OWASP Top 10 for LLM Applications 2025 – WorldTech IT](https://wtit.com/blog/2025/04/17/owasp-top-10-for-llm-applications-2025/)
* [OWASP Machine Learning Security Top Ten](https://owasp.org/www-project-machine-learning-security-top-10/)
* [Few words about AI Security – Jussi Metso](https://www.jussimetso.com/index.php/2024/09/28/few-words-about-ai-security/)
* [How To Ensure LLM Output Adheres to a JSON Schema | Modelmetry](https://modelmetry.com/blog/how-to-ensure-llm-output-adheres-to-a-json-schema)
* [Easily enforcing valid JSON schema following – API](https://community.openai.com/t/feature-request-function-calling-easily-enforcing-valid-json-schema-following/263515?utm_source)
* [AI Safety + Cybersecurity R\&D Tracker – Fairly AI](https://www.fairly.ai/blog/ai-cybersecurity-tracker)
* [Anthropic makes 'jailbreak' advance to stop AI models producing harmful results](https://www.ft.com/content/cf11ebd8-aa0b-4ed4-945b-a5d4401d186e)
* [Pattern matching filter rules - IBM](https://www.ibm.com/docs/ssw_aix_71/security/intrusion_pattern_matching_filter_rules.html)
* [Real-time Threat Detection](https://www.darktrace.com/cyber-ai-glossary/real-time-threat-detection)
