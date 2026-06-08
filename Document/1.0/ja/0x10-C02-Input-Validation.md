# C2 入力バリデーション (Input Validation)

## 管理目標

すべての入力の堅牢なバリデーションは AI システムで最もダメージのある攻撃のいくつかに対する最前線の防御策です。プロンプトインジェクション攻撃は、システム命令を上書きしたり、機密データを漏洩したり、モデルを許可されていない動作に誘導する可能性があります。専用のフィルタとその他のバリデーションが配備されていない限り、コンテキストウィンドウを悪用するジェイルブレイクが引き続き有効になることが研究で示されています。エージェント型システムとマルチステップシステムでは、ツールからの入力、取得したドキュメント、MCP サーバーのレスポンス、サブエージェントの出力は、直接ユーザー入力と同じリスクを伴い、同じバリデーションパイプラインを必要とします。この章では AI システムに固有の脅威、すなわちプロンプトインジェクション、モデルの動作を標的とした敵対的入力、AI 特有のコンテンツスクリーニング、および敵対的摂動、ステガノグラフィペイロード、クロスモーダル攻撃を含むマルチモーダル攻撃ベクトルに焦点を当てます。

---

## C2.1 プロンプトインジェクションの防御 (Prompt Injection Defense)

プロンプトインジェクションは AI システムにとって最大のリスクの一つです。この戦術に対する防御策として、パターンフィルタ、データ分類器、命令階層適用を組み合わせて採用します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.1.1** | **検証:** モデルの動作を制御する可能性のあるすべての外部入力または派生入力は信頼できないものとして扱われ、プロンプトに含めたりアクションをトリガーするために使用する前に、プロンプトインジェクション検出ルールセットまたは分類器によってスクリーンされている。 | 1 |
| **2.1.2** | **検証:** 入力長制御はユーザー提供のコンテンツがコンテキストウィンドウの定義された割合を超えることを防止しており、トークン制限を超える入力は暗黙的に切り捨てられるのではなく拒否されている。これによりシステム命令と安全性指示がモデルの有効な注意から逸れることが防いでいる。 | 1 |
| **2.1.3** | **検証:** システムはモデルプロンプトのユーザー入力に対して文字セット制限を実装しており、許可リストアプローチを使用してビジネス上の目的で明示的に必要な文字のみを許可している。 | 1 |
| **2.1.4** | **検証:** サードパーティコンテンツ (ウェブページ、PDF、電子メール) から生成されたプロンプトは、メインプロンプトに連結される前に、個別にサニタイズされている (たとえば、命令のようなディレクティブを除外し、HTML、マークダウン、スクリプトコンテンツを中和している)。 | 2 |
| **2.1.5** | **検証:** システムは、単一のコンテキストウィンドウに含まれるユーザー提供のデモンストレーションの数について、リクエストごとの制限を適用している。 | 2 |
| **2.1.6** | **検証:** プロンプトインジェクションスクリーニングは、呼び出しエージェントのロールやパーミッションレベルなど、リクエスト時に解決される属性ベースのルールを介してユーザー固有の属性 (年齢層、認可ロール、リージョンコンテンツポリシー分類など) を尊重している。 | 2 |
| **2.1.7** | **検証:** システムは、システムおよび開発者メッセージがユーザー命令やその他の信頼できない入力を上書きするという命令階層を、ユーザー命令の処理後であっても、強制している。 | 3 |
| **2.1.8** | **Verify that** the instruction hierarchy is preserved across multi-step interactions and tool-augmented workflows, including prompt composition and intermediate outputs, such that user-controlled content cannot override system or developer instructions. | 3 |
| **2.1.9** | **検証:** システムは、複数回のジェイルブレーキングに一致する、体系的でコンテキストに応じた動作の上書き試行を示すパターンを検出している。 | 3 |
| **2.1.10** | **検証:** 検出されたコンテキストに応じた動作の上書き試行はプロンプトインジェクションイベントとして分類され、処理されている。 | 3 |

---

## C2.2 Pre-Tokenization Input Normalization

AI models process text through tokenizers and embeddings that can be exploited via encoding tricks invisible to conventional input validation. Normalization before tokenization closes attack vectors such as homoglyph substitution, invisible character injection, and bidirectional text manipulation that bypass standard allow-list filters but alter model behavior.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.2.1** | **Verify that** input normalization is applied before tokenization or embedding. This includes Unicode NFC canonicalization, homoglyph mapping, removal of control and invisible Unicode characters, and bidirectional text neutralization. | 1 |
| **2.2.2** | **Verify that** inputs identified as adversarial by any detection mechanism are blocked from inclusion in prompts or execution of actions. | 1 |
| **2.2.3** | **Verify that** inputs which still contain suspicious encoding artifacts after normalization are rejected or flagged for review. | 2 |
| **2.2.4** | **検証:** 入力のエンコーディングと表現のスマグリング (不可視の Unicode/制御文字、ホモグリフのスワップ、混合方向テキスト) は検出され、緩和されている。承認された緩和策は、正規化、厳密なスキーマバリデーション、ポリシーベースの拒否、明示的なマーキングを含んでいる。 | 3 |

---

## C2.3 コンテンツとポリシーの審査 (Content & Policy Screening)

Syntactically valid prompts may request disallowed content such as policy-violating instructions, harmful content, or restricted material. Input-side content screening prevents such prompts from reaching the model.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.3.1** | **Verify that** every inbound prompt is scored by a content classifier for violence, self-harm, hate, and sexual content against configurable thresholds. Prompts that exceed those thresholds are rejected or sanitized before reaching model context. | 1 |
| **2.3.2** | **Verify that** prompt content classification is evaluated for unsupported-language abuse. Identified gaps are mitigated through compensating controls such as language detection with rejection, conservative thresholds, or human review routing. | 1 |
| **2.3.3** | **検証:** ポリシーに違反する入力は拒否されるため、ダウンストリームのモデルやツール/MCP 呼び出しに伝播していない。 | 1 |
| **2.3.4** | **検証:** スクリーニングログは SOC 相関と将来のレッドチームのリプレイのために分類器の信頼スコアとポリシーカテゴリタグを、適用されたステージ (プロンプト前またはレスポンス後) とトレースメタデータ (ソース、ツールまたは MCP サーバー、エージェント ID、セッション) とともに、含んでいる。 | 2 |

---

## C2.4 マルチモーダル入力バリデーション (Multi-Modal Input Validation)

AI systems that accept non-textual inputs (images, audio, video, files) face unique attack vectors where malicious content can be embedded across modalities and extracted into text that feeds the model's context.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **2.4.1** | 非テキスト入力から抽出されたテキスト (画像からテキスト、音声からテキストなど) および非表示または埋め込まれたコンテンツ (メタデータ、レイヤ、代替テキスト、コメント) は 2.1.1 に従って信頼できないものとして扱われている。 | 1 |
| **2.4.2** | **検証:** 画像/音声入力は敵対的な摂動、ステガノグラフィペイロード、既知の攻撃パターンについてチェックされており、検出するとモデル使用前にゲーティング (ブロックまたは機能のデグレード) をトリガーしている。 | 3 |
| **2.4.3** | **検証:** クロスモーダル攻撃検出は相関ルールを使用して、複数の入力タイプにまたがる協調攻撃 (画像内のステガノグラフィペイロードとテキスト内のプロンプトインジェクションの組み合わせなど) を識別し、アラートを発している。確認された検出はブロックされるか、HITL (human-in-the-loop) の承認を要求している。 | 3 |

---

## 参考情報

* [OWASP LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
* [LLM Prompt Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html)
* [MITRE ATLAS: Adversarial Input Detection](https://atlas.mitre.org/mitigations/AML.M00150)
* [Mitigate jailbreaks and prompt injections](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks)
