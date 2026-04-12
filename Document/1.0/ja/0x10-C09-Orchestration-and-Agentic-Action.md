# C9 自律オーケストレーションとエージェントアクションセキュリティ (Autonomous Orchestration & Agentic Action Security)

## 管理目標

自律型およびマルチエージェント型のシステムは **認可され、意図され、かつ制限された** アクションのみを実行する必要があります。このコントロールファミリーは、明示的な認可、サンドボックス化された実行、暗号化されたアイデンティティと改竄防止監査、メッセージセキュリティ、インテント/制約ゲートを適用することで、ツールの不正使用、権限昇格、制御不能な再帰/コスト増加、プロトコル操作、エージェント間またはテナント間の干渉によるリスクを軽減します。

---

## C9.1 実行予算、ループ制御、サーキットブレーカー (Execution Budgets, Loop Control, and Circuit Breakers)

実行時の拡張 (再帰、同時実行、コスト) を制限し、暴走動作を安全に停止します。

> **Scope note:** Controls in this section govern internal orchestration runtime budgets: per-task recursion depth, concurrency, wall-clock time, token spend, and monetary limits. They apply inside the agentic execution layer, not at the API ingress edge (see ASVS v5 V2.4 for generic rate limiting) and not in response to adversarial probing patterns (C11.4, C11.5). A single token-spend cap at the API gateway does not satisfy C9.1 budget enforcement, which requires enforcement within the orchestration runtime itself.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.1.1** | **検証:** 実行ごとの予算 (最大再帰深度、最大ファンアウト/同時実行、経過実時間、トークン、金銭支出) はオーケストレーションランタイムによって構成および適用されている。 | 1 |
| **9.1.2** | **検証:** 累積リソース/支出カウンタはリクエストチェーンごとに追跡され、閾値を超える場合にはそのチェーンをハード停止している。 | 2 |
| **9.1.3** | **検証:** サーキットブレーカーは予算違反で実行を停止している。 | 2 |
| **9.1.4** | **検証:** セキュリティテストは、暴走ループ、予算枯渇、部分的な障害のシナリオをカバーし、安全な終了と一貫した状態を確認している。 | 3 |
| **9.1.5** | **検証:** 予算とサーキットブレーカーのポリシーは Policy as Code として表現され、CI/CD で検証され、ドリフトと安全でない構成変更を防いでいる。 | 3 |

---

## C9.2 影響度の高いアクションの承認と不可逆性の制御 (High-Impact Action Approval and Irreversibility Controls)

特権的または不可逆的な結果に対して明示的なチェックポイントを要求します。

> **Scope note:** C9.2 governs the runtime execution gate: the mechanism by which the agent system blocks a privileged or irreversible action and waits for an explicit approval signal before proceeding. Organizational policy that defines which decisions qualify as high-risk and what approval authority is required is in C14.2. Logging and auditing of approval events is in C13.7.4. All three controls address different aspects of the same workflow and require separate evidence.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.2.1** | **Verify that** the agent runtime enforces an execution gate that blocks privileged or irreversible actions (e.g., code merges/deploys, financial transfers, user access changes, destructive deletes, external notifications) until an explicit human approval is received and verified. | 1 |
| **9.2.2** | **Verify that** approval requests display canonicalized and complete action parameters (diff, command, recipient, amount, scope) without truncation or transformation. | 2 |
| **9.2.3** | **Verify that** approvals are cryptographically bound (e.g., signed or MACed) to the exact action parameters, requester identity, and execution context. | 2 |
| **9.2.4** | **Verify that** approvals include a unique nonce and are single-use to prevent replay or substitution. | 2 |
| **9.2.5** | **Verify that** approvals expire within a defined maximum time-to-live (TTL) and are rejected after expiration. | 2 |
| **9.2.6** | **検証:** ロールバックが実現可能である場合は、補償アクションが定義およびテストされ (トランザクションのセマンティクス)、障害はロールバックまたは安全な封じ込めをトリガーしている。 | 3 |

---

## C9.3 ツールとプラグインの分離と安全な統合 (Tool and Plugin Isolation and Safe Integration)

ツールの実行、ロード、出力を制限して、不正なシステムアクセスや安全でない副作用を防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.3.1** | **検証:** 各ツール/プラグインは、ツールの機能に適した最小権限のファイルシステム、ネットワーク送出 (egress)、システムコールパーミッションを備える分離されたサンドボックス (コンテナ/VM/WASM/OS サンドボックス) 内で実行している。 | 1 |
| **9.3.2** | **検証:** ツールごとのクォータとタイムアウト (CPU、メモリ、ディスク、送出 (egress)、実行時間) が強制され、ログ記録されている。 | 1 |
| **9.3.7** | **Verify that** quota or timeout breaches fail closed, terminating the tool execution rather than continuing with degraded or uncontrolled behavior. | 1 |
| **9.3.3** | **検証:** ツール出力は、ダウンストリームの推論や後続のアクションに組み込まれる前に、厳格なスキーマとセキュリティポリシーに対して検証されている。 | 1 |
| **9.3.4** | **検証:** ツールバイナリやパッケージはロード前に完全性検証 (署名、チェックサムなど) されている。 | 2 |
| **9.3.5** | **検証:** ツールマニフェストは、必要な権限、副作用レベル、リソース制限、出力バリデーション要件を宣言し、ランタイムはこれらの宣言を強制している。 | 2 |
| **9.3.6** | **検証:** サンドボックスのエスケープインジケータまたはポリシー違反は自動封じ込め (ツールの無効化/隔離) をトリガーしている。 | 3 |

---

## C9.4 エージェントとオーケストレータのアイデンティティ、署名、改竄防止の監査 (Agent and Orchestrator Identity, Signing, and Tamper-Evident Audit)

すべてのアクションを帰属可能にし、すべての変異を検出可能にします。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.4.1** | **検証:** 各エージェントインスタンス (およびオーケストレータ/ランタイム) は一意の暗号アイデンティティを持ち、ダウンストリームシステムへのファーストクラスのプリンシパルとして認証している (エンドユーザークレデンシャルを再使用していない)。 | 1 |
| **9.4.2** | **検証:** エージェントが開始したアクションは実行チェーン (チェーン ID) に暗号的にバインドされ、否認防止と追跡可能性のために署名され、タイムスタンプ付けされている。 | 2 |
| **9.4.3** | **検証:** 監査ログは、追加専用/WORM/不変ログストア、各レコードが前のレコードのハッシュを含む暗号ハッシュチェーン、または独立して検証可能な同等の完全性保証により、改竄防止を備えている。 | 2 |
| **9.4.5** | **検証:** 監査ログレコードは、誰が何を実行したか、開始ユーザー識別子、委譲範囲、認可決定 (ポリシー/バージョン)、ツールパラメータ、承認 (適用可能な場合)、結果を再構築するのに十分なコンテキストを含んでいる。 | 2 |
| **9.4.4** | **検証:** エージェントのアイデンティティクレデンシャル (鍵/証明書/トークン) は定義されたスケジュールと侵害の兆候で入れ替わり、傷害の疑いやなりすましの試みですぐに失効して隔離している。 | 3 |
| **9.4.6** | **Verify that** agent state persisted between invocations (including memory, task context, goals, and partial results) is integrity-protected (e.g., via cryptographic MACs or signatures), and that the runtime rejects or quarantines state that fails integrity verification before resuming execution. | 3 |

---

## C9.5 安全なメッセージングとプロトコルの堅牢化 (Secure Messaging and Protocol Hardening)

エージェント間およびエージェントとツール間の通信をハイジャック、インジェクション、リプレイ、非同期化から保護します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.5.1** | **検証:** エージェント間およびエージェントとツール間のチャネルは、現在推奨されているプロトコル (TLS 1.3 以降など) を強力な証明書/トークンバリデーションとともに使用して、相互認証と暗号化を強制している。 | 1 |
| **9.5.2** | **検証:** すべてのメッセージは厳密にスキーマ検証され、不明なフィールド、不正なペイロード、サイズが大きすぎるフレームは拒否されている。 | 1 |
| **9.5.3** | **検証:** メッセージの完全性はツールパラメータを含む完全なペイロードをカバーし、リプレイ保護 (ノンス/シーケンス番号/タイムスタンプウィンドウ) が適用されている。 | 2 |
| **9.5.4** | **検証:** ダウンストリームエージェントに伝播されるエージェントの出力は、スキーマバリデーションに加えて、意味的制約 (値の範囲、論理的一貫性など) に対して検証されている。 | 2 |

---

## C9.6 認可、委譲、継続的施行 (Authorization, Delegation, and Continuous Enforcement)

すべてのアクションは実行時に認可され、スコープによって制約されていることを確保します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.6.1** | **検証:** エージェントのアクションは、ランタイムによって強制される、きめ細かいポリシーに基づいて認可されている。エージェントが呼び出せるツール、エージェントが提供できるパラメータ値 (許可リスト、データスコープ、アクションタイプなど) を制限し、ポリシー違反はブロックされている。 | 1 |
| **9.6.2** | **検証:** エージェントがユーザーの代わりに動作する場合、ランタイムは完全性が保護された委譲コンテキスト (ユーザー ID、テナント、セッション、スコープ) を伝播し、ユーザーのクレデンシャルを使用せずに、すべてのダウンストリーム呼び出しでそのコンテキストを適用している。 | 2 |
| **9.6.3** | **検証:** 認可は現在のコンテキスト (ユーザー、テナント、環境、データ分類、時間、リスク) を使用してすべての呼び出しで再評価 (継続的な認可) されている。 | 2 |
| **9.6.4** | **検証:** すべてのアクセス制御の決定は、AI モデル自体ではなく、アプリケーションロジックまたはポリシーエンジンによって強制されており、モデルが生成する出力 (「ユーザーはこれを行うことが許可されている」など) はアクセス制御チェックを上書きやバイパスできない。 | 2 |
| **9.6.5** | **Verify that** secrets and credentials required by an agent at runtime are not exposed within the model's observable context, including the context window, system prompts, or tool call parameters, and are instead provided via out-of-band mechanisms such as credential proxies, secrets manager injection, runtime sidecar authentication, or short-lived scoped tokens. | 2 |
| **9.6.6** | **Verify that** when an agent acts under delegated authority, the policy decision point evaluates both the agent's own granted permissions and the initiating principal's delegated scope as independent constraints, denying the action if either is insufficient for the requested operation. | 2 |
| **9.6.7** | **Verify that** agent-to-agent task delegation is restricted by an explicit peer authorization policy (e.g., an approved agent registry or allowlist) so that even authenticated agents can only delegate to or accept delegations from pre-approved peers, with delegation attempts from unlisted agents rejected by default. | 2 |

---

## C9.7 意思の検証と制約ゲート (Intent Verification and Constraint Gates)

実行をユーザーの意図と厳格な制約にバインドすることで、「技術的には認可されているが意図していない」アクションを防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.7.1** | **検証:** 実行前ゲートは提案されたアクションとパラメータを厳格なポリシー制約 (拒否ルール、データ処理制約、許可リスト、副作用予算) に照らして評価し、なんらかの違反で実行をブロックしている。 | 1 |
| **9.7.2** | **Verify that** post-execution checks confirm the intended outcome was achieved. | 2 |
| **9.7.3** | **Verify that** post-execution checks detect unintended side effects. | 2 |
| **9.7.4** | **Verify that** any mismatch between intended outcome and actual results triggers containment and, where supported, compensating actions. | 2 |
| **9.7.5** | **検証:** リモートリソースから取得されたプロンプトテンプレートとエージェントポリシー構成はロード時に承認済みバージョン (ハッシュや署名など) と比較して完全性検証されている。 | 3 |
| **9.7.6** | **Verify that** all write operations to persistent external state are authorized by either explicit human approval or an independent policy-based authorization mechanism that evaluates the operation against the original user intent, and not solely on agent-generated output. | 2 |

---

## C9.8 マルチエージェントのドメイン分離と群集リスク制御 (Multi-Agent Domain Isolation and Swarm Risk Controls)

ドメイン間の干渉と緊急の安全でない集団行動を軽減します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.8.1** | **検証:** 異なるテナント、セキュリティドメイン、環境 (開発/テスト/本番) のエージェントは、クロスドメイン発掘と呼び出しを防止するデフォルト拒否制御での、分離されたランタイムとネットワークセグメントで実行している。 | 1 |
| **9.8.2** | **検証:** ランタイム監視は安全でない緊急動作 (変動、デッドロック、制御されていないブロードキャスト、異常な呼び出しグラフ) を検出し、自動的に是正アクション (抑制、隔離、終了) を適用している。 | 3 |
| **9.8.3** | **Verify that** each agent is restricted to its own memory namespace and is technically prevented from reading or modifying peer agent state, preventing unauthorized cross-agent access within the same swarm. | 2 |
| **9.8.4** | **Verify that** each agent operates with an isolated context window that peer agents cannot read or influence, preventing unauthorized cross-agent context access within the same swarm. | 3 |
| **9.8.7** | **Verify that** each agent is issued dedicated credentials scoped to its role that are not shared with or accessible to peer agents within the same swarm. | 3 |
| **9.8.5** | **Verify that** swarm-level aggregate action rate limits (e.g., total external API calls, file writes, or network requests per time window across all agents) are enforced to prevent bursts that cause denial-of-service or abuse of external systems. | 3 |
| **9.8.6** | **Verify that** a swarm-level shutdown capability exists that can halt all active agent instances or selected problematic instances in an organized fashion and prevents new agent spawning, with shutdown completable within a pre-defined response time. | 3 |
| **9.8.8** | **Verify that** when multiple agents concurrently access shared mutable state, authorization checks and the state mutations they gate are executed atomically (e.g., via transactions, optimistic locking, or compare-and-swap) to prevent TOCTOU vulnerabilities where a concurrent state change invalidates a prior authorization decision before the guarded action completes. | 2 |

---

## 参考情報

* [OWASP LLM06:2025 Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)
* [OWASP LLM10:2025 Unbounded Consumption](https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/)
* [OWASP Agentic AI Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/detail/sp/800-207/final)
* [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026)
