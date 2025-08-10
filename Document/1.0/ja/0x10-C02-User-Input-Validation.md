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

AI attacks featuring malformed or oversized inputs can cause parsing errors, prompt spillage across fields, and resource exhaustion.  Strict schema enforcement is also a prerequisite when performing deterministic tool calls.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.3.1** | **Verify that** every API or function call endpoint defines an explicit input schema (JSON Schema, Protobuf or multimodal equivalent) and that inputs are validated before prompt assembly. | 1 | D |
| **2.3.2** | **Verify that** inputs exceeding maximum token or byte limits are rejected with a safe error and never silently truncated. | 1 |  D/V |
| **2.3.3** | **Verify that** type checks (e.g., numeric ranges, enum values, MIME types for images/audio) are enforced server-side, not only in client code. | 2 |  D/V |
| **2.3.4** | **Verify that** semantic validators (e.g., JSON Schema) run in constant time to prevent algorithmic DoS. | 2 | D |
| **2.3.5** | **Verify that** validation failures are logged with redacted payload snippets and unambiguous error codes to aid security triage. | 3 | V |

---

## C2.4 コンテンツとポリシーの審査 (Content & Policy Screening)

Developers should be able to detect syntactically valid prompts that request disallowed content (such as illicit instructions, hate speech, and copyrighted text) then prevent them from propagating.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.4.1** | **Verify that** a content classifier (zero shot or fine tuned) scores every input for violence, self-harm, hate, sexual content and illegal requests, with configurable thresholds. | 1 | D |
| **2.4.2** | **Verify that** inputs which violate policies will receive standardized refusals or safe completions so they will not propagate to downstream LLM calls. | 1 |  D/V |
| **2.4.3** | **Verify that** the screening model or rule set is retrained/updated at least quarterly, incorporating newly observed jailbreak or policy bypass patterns.  | 2 | D |
| **2.4.4** | **Verify that** screening respects user-specific policies (age, regional legal constraints) via attribute-based rules resolved at request time.  | 2 | D |
| **2.4.5** | **Verify that** screening logs include classifier confidence scores and policy category tags for SOC correlation and future red-team replay. | 3 | V |

---

## C2.5 入力レート制限と不正使用防止 (Input Rate Limiting & Abuse Prevention)

Developers should prevent abuse, resource exhaustion, and automated attacks against AI systems by limiting input rates and detecting anomalous usage patterns.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.5.1** | **Verify that** per-user, per-IP, and per-API-key rate limits are enforced for all input endpoints. | 1 | D/V |
| **2.5.2** | **Verify that** burst and sustained rate limits are tuned to prevent DoS and brute force attacks. | 2 | D/V |
| **2.5.3** | **Verify that** anomalous usage patterns (e.g., rapid-fire requests, input flooding) trigger automated blocks or escalations. | 2 | D/V |
| **2.5.4** | **Verify that** abuse prevention logs are retained and reviewed for emerging attack patterns. | 3 | V |

---

## C2.6 マルチモーダル入力バリデーション (Multi-Modal Input Validation)

AI systems should include robust validation for non-textual inputs (images, audio, files) to prevent injection, evasion, or resource abuse.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.6.1** | **Verify that** all non-text inputs (images, audio, files) are validated for type, size, and format before processing. | 1 | D |
| **2.6.2** | **Verify that** files are scanned for malware and steganographic payloads before ingestion. | 2 | D/V |
| **2.6.3** | **Verify that** image/audio inputs are checked for adversarial perturbations or known attack patterns. | 2 | D/V |
| **2.6.4** | **Verify that** multi-modal input validation failures are logged and trigger alerts for investigation. | 3 | V |

---

## C2.7 入力の来歴と属例 (Input Provenance & Attribution)

AI systems should support auditing, abuse tracking, and compliance by monitoring and tagging the origins of all user inputs.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **2.7.1** | **Verify that** all user inputs are tagged with metadata (user ID, session, source, timestamp, IP address) at ingestion. | 1 | D/V |
| **2.7.2** | **Verify that** provenance metadata is retained and auditable for all processed inputs. | 2 | D/V |
| **2.7.3** | **Verify that** anomalous or untrusted input sources are flagged and subject to enhanced scrutiny or blocking. | 2 | D/V |

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
