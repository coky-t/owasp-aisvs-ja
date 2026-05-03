# C9 自律オーケストレーションとエージェントアクションセキュリティ (Autonomous Orchestration & Agentic Action Security)

## 管理目標

自律型およびマルチエージェント型のシステムは認可され、意図され、かつ制限されたアクションのみを実行する必要があります。 This chapter focuses on controls unique to agentic AI execution: agent-as-principal identity, agent action chains, model-output-driven authorization risk, intent verification of LLM-decided actions, and multi-agent swarm dynamics. Generic application security controls (transport encryption, schema validation, message integrity, log integrity, concurrency safety, supply chain integrity, service-account rotation, multi-tenant isolation, contextual authorization, and API-edge rate limiting) are covered by ASVS v5 (V2, V4, V8, V11, V12, V13, V15, V16), AISVS C9.4.3 (tamper-evident audit logs), and OWASP SCVS, and are not repeated here.

Boundaries with adjacent controls determine what evidence satisfies each requirement. C9.1 execution budgets apply inside the orchestration runtime and do not substitute for API-edge rate limiting (ASVS v5 V2.4) or anti-extraction/anti-inversion throttling (C11.4, C11.5). C9.2 governs the runtime gate that blocks high-impact actions until approval is received; C14.2 defines the policy that classifies actions as high-risk and assigns approval authority; C13.6.4 covers logging of approval events. C9.5 covers semantic validation of agent-generated outputs flowing into downstream agents, while transport, schema, and replay protections are in ASVS v5 V4/V11/V12 and MCP-specific message and schema controls are in C10.4.

---

## C9.1 実行予算、ループ制御、サーキットブレーカー (Execution Budgets, Loop Control, and Circuit Breakers)

実行時の拡張 (再帰、同時実行、コスト) を制限し、暴走動作を安全に停止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.1.1** | **検証:** 実行ごとの予算 (最大再帰深度、最大ファンアウト/同時実行、経過実時間、トークン、金銭支出) はオーケストレーションランタイムによって構成および適用されている。 | 1 |
| **9.1.2** | **Verify that** cumulative resource and spend counters are tracked per request chain and that exceeding any configured threshold hard-stops the chain via a circuit breaker. | 2 |
| **9.1.3** | **検証:** セキュリティテストは、暴走ループ、予算枯渇、部分的な障害のシナリオをカバーし、安全な終了と一貫した状態を確認している。 | 3 |

---

## C9.2 影響度の高いアクションの承認と不可逆性の制御 (High-Impact Action Approval and Irreversibility Controls)

特権的または不可逆的な結果に対して明示的なチェックポイントを要求します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.2.1** | **Verify that** the agent runtime enforces an execution gate that blocks privileged or irreversible actions (e.g., code merges/deploys, financial transfers, user access changes, destructive deletes, external notifications) until an explicit human approval is received and verified. | 1 |
| **9.2.2** | **Verify that** approval requests display canonicalized and complete action parameters (diff, command, recipient, amount, scope) without truncation or transformation. | 2 |
| **9.2.3** | **Verify that** approvals are cryptographically bound (e.g., signed or MACed) to the exact action parameters, requester identity, and execution context, include a unique single-use nonce, and expire within a defined maximum time-to-live (TTL). | 2 |
| **9.2.4** | **検証:** ロールバックが実現可能である場合は、補償アクションが定義およびテストされ (トランザクションのセマンティクス)、障害はロールバックまたは安全な封じ込めをトリガーしている。 | 3 |

---

## C9.3 ツールとプラグインの分離と安全な統合 (Tool and Plugin Isolation and Safe Integration)

ツールの実行、ロード、出力を制限して、不正なシステムアクセスや安全でない副作用を防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.3.1** | **検証:** 各ツール/プラグインは、ツールの機能に適した最小権限のファイルシステム、ネットワーク送出 (egress)、システムコールパーミッションを備える分離されたサンドボックス (コンテナ/VM/WASM/OS サンドボックス) 内で実行している。 | 1 |
| **9.3.2** | **検証:** ツールごとのクォータとタイムアウト (CPU、メモリ、ディスク、送出 (egress)、実行時間) が強制され、ログ記録されており、 and that quota or timeout breaches fail closed by terminating the tool execution rather than continuing with degraded or uncontrolled behavior. | 1 |
| **9.3.3** | **検証:** ツール出力は、ダウンストリームの推論や後続のアクションに組み込まれる前に、厳格なスキーマとセキュリティポリシーに対して検証されている。 | 1 |
| **9.3.4** | **検証:** ツールマニフェストは、必要な権限、副作用レベル、リソース制限、出力バリデーション要件を宣言し、ランタイムはこれらの宣言を強制している。 | 2 |
| **9.3.5** | **検証:** サンドボックスのエスケープインジケータまたはポリシー違反は自動封じ込め (ツールの無効化/隔離) をトリガーしている。 | 3 |

---

## C9.4 エージェントとオーケストレータのアイデンティティ、署名、改竄防止の監査 (Agent and Orchestrator Identity, Signing, and Tamper-Evident Audit)

すべてのアクションを帰属可能にし、すべての変異を検出可能にします。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.4.1** | **検証:** 各エージェントインスタンス (およびオーケストレータ/ランタイム) は一意の暗号アイデンティティを持ち、ダウンストリームシステムへのファーストクラスのプリンシパルとして認証している (エンドユーザークレデンシャルを再使用していない)。 | 1 |
| **9.4.2** | **検証:** エージェントが開始したアクションは実行チェーン (チェーン ID) に暗号的にバインドされ、否認防止と追跡可能性のために署名され、タイムスタンプ付けされている。 | 2 |
| **9.4.3** | **検証:** 監査ログレコードは、誰が何を実行したか、開始ユーザー識別子、委譲範囲、認可決定 (ポリシー/バージョン)、ツールパラメータ、承認 (適用可能な場合)、結果を再構築するのに十分なコンテキストを含んでいる。 | 2 |
| **9.4.4** | **検証:** エージェントのアイデンティティクレデンシャル (鍵/証明書/トークン) は定義されたスケジュールと侵害の兆候で入れ替わり、傷害の疑いやなりすましの試みですぐに失効して隔離している。 | 3 |
| **9.4.5** | **Verify that** agent state persisted between invocations (including memory, task context, goals, and partial results) is integrity-protected (e.g., via cryptographic MACs or signatures), and that the runtime rejects or quarantines state that fails integrity verification before resuming execution. | 3 |

---

## C9.5 安全なメッセージングとプロトコルの堅牢化 (Secure Messaging and Protocol Hardening)

エージェント間およびエージェントとツール間の通信をハイジャック、インジェクション、不正な改変から保護します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.5.1** | **検証:** ダウンストリームエージェントに伝播されるエージェントの出力は、スキーマバリデーションに加えて、意味的制約 (値の範囲、論理的一貫性など) に対して検証されている。 | 2 |

---

## C9.6 認可、委譲、継続的施行 (Authorization, Delegation, and Continuous Enforcement)

すべてのアクションは実行時に認可され、スコープによって制約されていることを確保します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.6.1** | **検証:** エージェントのアクションは、ランタイムによって強制される、きめ細かいポリシーに基づいて認可されている。エージェントが呼び出せるツール、エージェントが提供できるパラメータ値 (許可リスト、データスコープ、アクションタイプなど) を制限し、ポリシー違反はブロックされている。 | 1 |
| **9.6.2** | **検証:** エージェントがユーザーの代わりに動作する場合、ランタイムは完全性が保護された委譲コンテキスト (ユーザー ID、テナント、セッション、スコープ) を伝播し、ユーザーのクレデンシャルを使用せずに、すべてのダウンストリーム呼び出しでそのコンテキストを適用している。 | 2 |
| **9.6.3** | **検証:** すべてのアクセス制御の決定は、AI モデル自体ではなく、アプリケーションロジックまたはポリシーエンジンによって強制されており、モデルが生成する出力 (「ユーザーはこれを行うことが許可されている」など) はアクセス制御チェックを上書きやバイパスできない。 | 2 |
| **9.6.4** | **Verify that** secrets and credentials required by an agent at runtime are not exposed within the model's observable context, including the context window, system prompts, or tool call parameters, and are instead provided via out-of-band mechanisms such as credential proxies, secrets manager injection, runtime sidecar authentication, or short-lived scoped tokens. | 2 |
| **9.6.5** | **Verify that** when an agent acts under delegated authority, the policy decision point evaluates both the agent's own granted permissions and the initiating principal's delegated scope as independent constraints, denying the action if either is insufficient for the requested operation. | 2 |
| **9.6.6** | **Verify that** agent-to-agent task delegation is restricted by an explicit peer authorization policy (e.g., an approved agent registry or allowlist) so that even authenticated agents can only delegate to or accept delegations from pre-approved peers, with delegation attempts from unlisted agents rejected by default. | 2 |

---

## C9.7 意思の検証と制約ゲート (Intent Verification and Constraint Gates)

実行をユーザーの意図と厳格な制約にバインドすることで、「技術的には認可されているが意図していない」アクションを防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.7.1** | **検証:** 実行前ゲートは提案されたアクションとパラメータを厳格なポリシー制約 (拒否ルール、データ処理制約、許可リスト、副作用予算) に照らして評価し、なんらかの違反で実行をブロックしている。 | 1 |
| **9.7.2** | **Verify that** post-execution checks confirm the intended outcome was achieved and detect unintended side effects, and that any mismatch triggers containment and, where supported, compensating actions. | 2 |
| **9.7.3** | **Verify that** all write operations to persistent external state are authorized by either explicit human approval or an independent policy-based authorization mechanism that evaluates the operation against the original user intent, and not solely on agent-generated output. | 2 |
| **9.7.4** | **Verify that** when the policy decision point (PDP) used for governance evaluation is unavailable (e.g., timeout, network partition, service failure), agent execution fails closed by blocking the proposed action, and the unavailability event is logged with sufficient detail for incident investigation. | 2 |
| **9.7.5** | **検証:** リモートリソースから取得されたプロンプトテンプレートとエージェントポリシー構成はロード時に承認済みバージョン (ハッシュや署名など) と比較して完全性検証されている。  | 3 |

---

## C9.8 マルチエージェントのドメイン分離と群集リスク制御 (Multi-Agent Domain Isolation and Swarm Risk Controls)

ドメイン間の干渉と緊急の安全でない集団行動を軽減します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.8.1** | **検証:** 異なるテナント、セキュリティドメイン、環境 (開発/テスト/本番) のエージェントは、クロスドメイン発掘と呼び出しを防止するデフォルト拒否制御での、分離されたランタイムとネットワークセグメントで実行している。 | 1 |
| **9.8.2** | **Verify that** each agent is restricted to its own memory namespace and is technically prevented from reading or modifying peer agent state, preventing unauthorized cross-agent access within the same swarm. | 2 |
| **9.8.3** | **Verify that** each agent operates with an isolated context window that peer agents cannot read or influence, preventing unauthorized cross-agent context access within the same swarm. | 3 |
| **9.8.4** | **Verify that** runtime monitoring detects unsafe emergent behavior (oscillation, deadlocks, uncontrolled broadcast, abnormal call graphs) and automatically applies corrective actions (throttle, isolate, terminate). | 3 |
| **9.8.5** | **Verify that** swarm-level aggregate action rate limits (e.g., total external API calls, file writes, or network requests per time window across all agents) are enforced to prevent bursts that cause denial-of-service or abuse of external systems. | 3 |
| **9.8.6** | **Verify that** a swarm-level shutdown capability exists that can halt all active agent instances or selected problematic instances in an organized fashion and prevents new agent spawning, with shutdown completable within a pre-defined response time. | 3 |

---

## C9.9 Architectural Data-Flow Isolation and Origin Enforcement

Prevent data-flow attacks that exploit agentic tool-calling pipelines by manipulating tool arguments without altering the control flow, through architectural separation of planning from untrusted data processing and origin-aware policy enforcement at the tool-call boundary. These controls complement C2.1 (input-level filtering) and C9.7 (intent verification gates), which do not address attacks where the correct tools are called in the correct sequence but with attacker-controlled arguments derived from untrusted data. Any isolation mechanism that achieves the stated outcome satisfies the requirement.

| # | Description | Level |
| :--: | --- | :---: |
| **9.9.1** | **Verify that** the system architecturally separates, or applies an equivalent isolation mechanism to separate, plan generation (control flow) from untrusted data processing, such that the component determining which tools to call and in what sequence does not directly process untrusted content (e.g., tool outputs, retrieved documents, external messages). | 2 |
| **9.9.2** | **Verify that** components processing untrusted data (e.g., for extraction, summarization, or parsing) are isolated, or equivalently constrained, from tool-calling capabilities, ensuring that compromised data processing cannot trigger unauthorized tool invocations. | 2 |
| **9.9.3** | **Verify that** security policies for tool execution are expressed as auditable, versioned, machine-interpretable code or configuration, not solely as natural language instructions within prompts. | 2 |
| **9.9.4** | **Verify that** values passed to tools carry origin metadata tracking their source (user input, specific tool, external source). | 3 |
| **9.9.5** | **Verify that** security policies are evaluated against value origin before each tool invocation. | 3 |
| **9.9.6** | **Verify that** tool invocations where argument origin violates the applicable security policy are blocked before execution. | 3 |
| **9.9.7** | **Verify that** data-flow integrity is enforced such that untrusted data cannot modify tool arguments beyond what the security policy explicitly permits, even when the control flow (sequence of tool calls) remains as intended. | 3 |
| **9.9.8** | **Verify that** the system's data-flow dependency graph is maintained per session and is available for review by authorized security and operations personnel for post-hoc audit, enabling identification of which data sources influenced each tool invocation and its arguments. | 3 |

---

## 参考情報

* [OWASP LLM06:2025 Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)
* [OWASP LLM10:2025 Unbounded Consumption](https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/)
* [OWASP Agentic AI Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
* [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026)
* [OpenAPI x-agent-trust Extension (OAI Extensions Registry)](https://spec.openapis.org/registry/extension/x-agent-trust.html)
