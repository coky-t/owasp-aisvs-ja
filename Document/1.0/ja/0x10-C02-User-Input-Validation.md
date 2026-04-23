# C2 ユーザー入力バリデーション (User Input Validation)

## 管理目標

ユーザー入力の堅牢なバリデーションは AI システムで最もダメージのある攻撃のいくつかに対する最前線の防御策です。プロンプトインジェクション攻撃は、システム命令を上書きしたり、機密データを漏洩したり、モデルを許可されていない動作に誘導する可能性があります。専用のフィルタとその他のバリデーションが配備されていない限り、コンテキストウィンドウを悪用するジェイルブレイクが引き続き有効になることが研究で示されています。

> **スコープに関する注記:** この章では、一般的なアプリケーション入力バリデーションを超えた、AI 特有の入力バリデーションについての懸念事項を取り上げます。標準的な入力バリデーション (スキーマの強制、型チェック、文字許可リスト、レート制限、ファイルアップロードバリデーション、サーバーサイドの強制、バリデーション失敗のログ記録) は OWASP ASVS v5 の V1, V2, V4, V5, V16 の章で取り上げられており、ベースラインとして実装すべきです。この章では AI システムに固有の脅威、すなわちプロンプトインジェクション、モデルの動作を標的とした敵対的入力、AI 特有のコンテンツスクリーニング、マルチモーダル攻撃ベクトルに焦点を当てます。

---

## C2.1 プロンプトインジェクションの防御 (Prompt Injection Defense)

プロンプトインジェクションは AI システムにとって最大のリスクの一つです。この戦術に対する防御策として、パターンフィルタ、データ分類器、命令階層適用を組み合わせて採用します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.1.1** | **検証:** モデルの動作を制御する可能性のあるすべての外部入力または派生入力は信頼できないものとして扱われ、プロンプトに含めたりアクションをトリガーするために使用する前に、プロンプトインジェクション検出ルールセットまたは分類器によってスクリーンされている。 | 1 |
| **2.1.2** | **検証:** システムは、システムおよび開発者メッセージがユーザー命令やその他の信頼できない入力を上書きするという命令階層を、ユーザー命令の処理後であっても、強制している。この強制は複数ステップの命令やツール拡張ワークフローにわたって維持されなければならない。そのような場合、プロンプトの合成や中間出力はユーザーが制御するコンテンツがシステムまたは開発者の命令に影響を与えたり上書きしてはいけない。 | 1 |
| **2.1.3** | **Verify that** input length controls prevent user-supplied content from exceeding a defined proportion of the context window, and that inputs exceeding token limits are rejected rather than silently truncated, ensuring system instructions and safety directives are not displaced from the model's effective attention. | 1 |
| **2.1.4** | **検証:** サードパーティコンテンツ (ウェブページ、PDF、電子メール) から生成されたプロンプトは、メインプロンプトに連結される前に、個別にサニタイズされている (たとえば、命令のようなディレクティブを除外し、HTML、マークダウン、スクリプトコンテンツを中和している)。 | 2 |
| **2.1.5** | **検証:** システムは、単一のコンテキストウィンドウに含まれるユーザー提供のデモンストレーションの数について、リクエストごとの制限を適用している。 | 2 |
| **2.1.6** | **検証:** システムは、複数回のジェイルブレーキングに一致する、体系的でコンテキストに応じた動作の上書き試行を示すパターンを検出している。 | 2 |
| **2.1.7** | **検証:** 検出されたコンテキストに応じた動作の上書き試行はプロンプトインジェクションイベントとして分類され、処理されている。 | 2 |
| **2.1.8** | **Verify that** prompt injection screening respects user-specific policies (age and regional legal constraints) via attribute-based rules resolved at request time, including the role or permission level of the calling agent. | 2 |
| **2.1.9** | **Verify that** the system implements a character set limitation for user inputs to model prompts, allowing only characters that are explicitly required for business purposes using an allow-list approach. | 1 |

---

## C2.2 Pre-Tokenization Input Normalization

AI models process text through tokenizers and embeddings that can be exploited via encoding tricks invisible to conventional input validation. Normalization before tokenization closes attack vectors such as homoglyph substitution, invisible character injection, and bidirectional text manipulation that bypass standard allow-list filters but alter model behavior.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.2.1** | **Verify that** input normalization (Unicode NFC canonicalization, homoglyph mapping, removal of control and invisible Unicode characters, and bidirectional text neutralization) is applied before tokenization or embedding, and that inputs which still contain suspicious encoding artifacts after normalization are rejected or flagged for review. | 1 |
| **2.2.2** | **検証:** 敵対的であることが疑われる入力は隔離され、ログ記録されている。 | 1 |
| **2.2.3** | **検証:** 入力と出力の両方でエンコーディングと表現のスマグリング (不可視の Unicode/制御文字、ホモグリフのスワップ、混合方向テキスト) は検出され、緩和されている。承認された緩和策は、正規化、厳密なスキーマバリデーション、ポリシーベースの拒否、明示的なマーキングを含んでいる。 | 3 |

---

## C2.3 コンテンツとポリシーの審査 (Content & Policy Screening)

Syntactically valid prompts may request disallowed content such as policy-violating instructions, harmful content, or restricted material. Input-side content screening prevents such prompts from reaching the model. For output-side content filtering, see C7.3.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.3.1** | **Verify that** a content classifier scores every inbound prompt for violence, self-harm, hate, sexual content and illegal requests, with configurable thresholds, before the prompt is included in model context. | 1 |
| **2.3.2** | **検証:** ポリシーに違反する入力は拒否されるため、ダウンストリームのモデルやツール/MCP 呼び出しに伝播していない。 | 1 |
| **2.3.3** | **検証:** スクリーニングログは SOC 相関と将来のレッドチームのリプレイのために分類器の信頼スコアとポリシーカテゴリタグを、適用されたステージ (プロンプト前またはレスポンス後) とトレースメタデータ (ソース、ツールまたは MCP サーバー、エージェント ID、セッション) とともに、含んでいる。 | 3 |

---

## C2.4 マルチモーダル入力バリデーション (Multi-Modal Input Validation)

AI systems that accept non-textual inputs (images, audio, video, files) face unique attack vectors where malicious content can be embedded across modalities and extracted into text that feeds the model's context.

> **Scope note:** Standard file upload validation (type, size, format, malware scanning, path traversal prevention) is covered by OWASP ASVS v5 chapter V5 and should be implemented as a baseline. This section addresses AI-specific risks: extraction of text from non-text inputs feeding into prompts, adversarial perturbations targeting model perception, and coordinated cross-modal attacks.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.4.1** | 非テキスト入力から抽出されたテキスト (画像からテキスト、音声からテキストなど) および非表示または埋め込まれたコンテンツ (メタデータ、レイヤ、代替テキスト、コメント) は 2.1.1 に従って信頼できないものとして扱われている。 | 1 |
| **2.4.2** | **検証:** 画像/音声入力は敵対的な摂動、ステガノグラフィペイロード、既知の攻撃パターンについてチェックされており、検出するとモデル使用前にゲーティング (ブロックまたは機能のデグレード) をトリガーしている。 | 2 |
| **2.4.3** | **検証:** クロスモーダル攻撃検出は、相関ルールとアラート生成で複数の入力タイプ (画像内のステガノグラフィペイロードとテキスト内のプロンプトインジェクションの組み合わせなど) にまたがる協調攻撃を識別しており、確認された検出はブロックされるか、HITL (human-in-the-loop) の承認を要求している。 | 3 |

---

## 参考情報

* [OWASP LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
* [LLM Prompt Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html)
* [MITRE ATLAS: Adversarial Input Detection](https://atlas.mitre.org/mitigations/AML.M00150)
* [Mitigate jailbreaks and prompt injections](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks)
