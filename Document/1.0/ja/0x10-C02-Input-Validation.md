# C2 入力バリデーション (Input Validation)

## 管理目標

すべての入力の堅牢なバリデーションは AI システムで最もダメージのある攻撃のいくつかに対する最前線の防御策です。プロンプトインジェクション攻撃は、システム命令を上書きしたり、機密データを漏洩したり、モデルを許可されていない動作に誘導する可能性があります。専用のフィルタとその他のバリデーションが配備されていない限り、コンテキストウィンドウを悪用するジェイルブレイクが引き続き有効になることが研究で示されています。エージェント型システムとマルチステップシステムでは、ツールからの入力、取得したドキュメント、MCP サーバーのレスポンス、サブエージェントの出力は、直接ユーザー入力と同じリスクを伴い、同じバリデーションパイプラインを必要とします。この章では AI システムに固有の脅威、すなわちプロンプトインジェクション、モデルの動作を標的とした敵対的入力、AI 特有のコンテンツスクリーニング、および敵対的摂動、ステガノグラフィペイロード、クロスモーダル攻撃を含むマルチモーダル攻撃ベクトルに焦点を当てます。

---

## C2.1 プロンプトインジェクションの防御 (Prompt Injection Defense)

プロンプトインジェクションは AI システムにとって最大のリスクの一つです。この戦術に対する防御策として、パターンフィルタ、データ分類器、命令階層適用を組み合わせて採用します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.1.1** | **Verify that** input normalization is applied before tokenization or embedding. | 1 |
| **2.1.2** | **Verify that** encoding and representation smuggling in inputs is detected and mitigated. Approved mitigations include canonicalization, strict schema validation, policy-based rejection, or explicit marking. | 1 |
| **2.1.3** | **Verify that** all inputs that may steer model behavior are treated as untrusted and screened by a prompt injection detection ruleset or classifier and blocked. | 1 |
| **2.1.4** | **Verify that** input length controls prevent content from exceeding the context window, and that inputs exceeding token limits are rejected rather than truncated. | 1 |
| **2.1.5** | **Verify that** the system implements a character set limitation for all inputs, allowing only characters that are explicitly required using an allow-list approach. | 1 |
| **2.1.6** | **Verify that** the system enforces an instruction hierarchy in which system and developer messages override user instructions and other untrusted inputs, even after processing user instructions. | 2 |
| **2.1.7** | **Verify that** the system detects many-shot jailbreaking patterns. | 3 |

## C2.2 Content & Policy Screening

Syntactically valid prompts may request disallowed content such as policy-violating instructions, harmful content, or restricted material. Input-side content screening prevents such prompts from reaching the model.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.2.1** | **Verify that** every prompt is scored by a content classifier for violence, self-harm, hate, and sexual content against configurable thresholds. Prompts that exceed those thresholds are rejected or sanitized before reaching model context. | 1 |
| **2.2.2** | **Verify that** prompt content classification is evaluated for languages that are not supported. | 1 |
| **2.2.3** | **Verify that** screening logs include classifier confidence scores and policy category tags with applied stage and trace metadata. | 2 |
| **2.2.4** | **Verify that** non-text inputs (image/video/audio) are checked for adversarial perturbations, steganographic payloads, hidden or embedded content, or known attack patterns. | 2 |
| **2.2.5** | **Verify that** coordinated attacks spanning multiple input types (e.g., steganographic payloads in images combined with prompt injection in text) are detected and blocked. | 3 |

---

## 参考情報

* [OWASP LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
* [LLM Prompt Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html)
* [MITRE ATLAS: Adversarial Input Detection](https://atlas.mitre.org/mitigations/AML.M0015)
* [Mitigate jailbreaks and prompt injections](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks)
