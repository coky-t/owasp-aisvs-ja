# C9 自律オーケストレーションとエージェントアクションセキュリティ (Autonomous Orchestration & Agentic Action Security)

## 管理目標

自律型およびマルチエージェント型のシステムは **認可され、意図され、かつ制限された** アクションのみを実行する必要があります。このコントロールファミリーは、明示的な認可、サンドボックス化された実行、暗号化されたアイデンティティと改竄防止監査、メッセージセキュリティ、インテント/制約ゲートを適用することで、ツールの不正使用、権限昇格、制御不能な再帰/コスト増加、プロトコル操作、エージェント間またはテナント間の干渉によるリスクを軽減します。

---

## C9.1 実行予算、ループ制御、サーキットブレーカー (Execution Budgets, Loop Control, and Circuit Breakers)

実行時の拡張 (再帰、同時実行、コスト) を制限し、暴走動作を安全に停止します。

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.1.1** | **検証:** 実行ごとの予算 (最大再帰深度、最大ファンアウト/同時実行、経過実時間、トークン、金銭支出) はオーケストレーションランタイムによって構成および適用されている。 | 1 | D/V |
| **9.1.2** | **検証:** 累積リソース/支出カウンタはリクエストチェーンごとに追跡され、閾値を超える場合にはそのチェーンをハード停止している。 | 2 | D/V |
| **9.1.3** | **検証:** サーキットブレーカーは予算違反で実行を停止している。 | 2 | D/V |
| **9.1.4** | **検証:** セキュリティテストは、暴走ループ、予算枯渇、部分的な障害のシナリオをカバーし、安全な終了と一貫した状態を確認している。 | 3 | V |
| **9.1.5** | **検証:** 予算とサーキットブレーカーのポリシーは Policy as Code として表現され、CI/CD で検証され、ドリフトと安全でない構成変更を防いでいる。 | 3 | D/V |

---

## C9.2 影響度の高いアクションの承認と不可逆性の制御 (High-Impact Action Approval and Irreversibility Controls)

特権的または不可逆的な結果に対して明示的なチェックポイントを要求します。

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.2.1** | **検証:** 特権的または不可逆的なアクション (コードのマージ/デプロイ、送金、ユーザーアクセスの変更、破壊的な削除、外部への通知) は明示的なヒューマンインザループ (human-in-loop) での承認を要求している。 | 1 | D/V |
| **9.2.2** | **検証:** 承認リクエストは正確なアクションパラメータ (diff/command/recipient/amount/scope) を提示し、承認をそれらのパラメータにバインドし、「一つの承認と別の実行」を防いでいる。 | 2 | D/V |
| **9.2.3** | **検証:** ロールバックが実現可能である場合は、補償アクションが定義およびテストされ (トランザクションのセマンティクス)、障害はロールバックまたは安全な封じ込めをトリガーしている。 | 3 | V |

---

## C9.3 ツールとプラグインの分離と安全な統合 (Tool and Plugin Isolation and Safe Integration)

ツールの実行、ロード、出力を制限して、不正なシステムアクセスや安全でない副作用を防止します。

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.3.1** | **検証:** 各ツール/プラグインは、ツールの機能に適した最小権限のファイルシステム、ネットワーク送出 (egress)、システムコールパーミッションを備える分離されたサンドボックス (コンテナ/VM/WASM/OS サンドボックス) 内で実行している。 | 1 | D/V |
| **9.3.2** | **検証:** ツールごとのクォータとタイムアウト (CPU、メモリ、ディスク、送出 (egress)、実行時間) が強制され、ログ記録され、クォータ違反はフェイルクローズされている。 | 1 | D/V |
| **9.3.3** | **検証:** ツールマニフェストは、必要な権限、副作用レベル、リソース制限、出力バリデーション要件を宣言し、ランタイムはこれらの宣言を強制している。 | 2 | D/V |
| **9.3.4** | **検証:** ツール出力は、ダウンストリームの推論や後続のアクションに組み込まれる前に、厳格なスキーマとセキュリティポリシーに対して検証されている。 | 2 | D/V |
| **9.3.5** | **検証:** ツールバイナリは完全性保護されており、ロード前に検証されている。 | 2 | D/V |
| **9.3.6** | **検証:** サンドボックスのエスケープインジケータまたはポリシー違反は自動封じ込め (ツールの無効化/隔離) をトリガーしている。 | 3 | D/V |

---

## C9.4 エージェントとオーケストレータのアイデンティティ、署名、改竄防止の監査 (Agent and Orchestrator Identity, Signing, and Tamper-Evident Audit)

すべてのアクションを帰属可能にし、すべての変異を検出可能にします。

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.4.1** | **検証:** 各エージェントインスタンス (およびオーケストレータ/ランタイム) は一意の暗号アイデンティティを持ち、ダウンストリームシステムへのファーストクラスのプリンシパルとして認証している (エンドユーザークレデンシャルを再使用していない)。 | 1 | D/V |
| **9.4.2** | **検証:** エージェントが開始したアクションは実行チェーン (チェーン ID) に暗号的にバインドされ、否認防止と追跡可能性のために署名され、タイムスタンプ付けされている。 | 2 | D/V |
| **9.4.3** | **検証:** 監査ログは改竄防止 (追加専用/WORM/不変ログストア) を備えており、誰が何を実行したか、開始ユーザー識別子、委譲範囲、認可決定 (ポリシー/バージョン)、ツールパラメータ、承認 (適用可能な場合)、結果を再構築するのに十分なコンテキストを含んでいる。 | 2 | D/V |
| **9.4.4** | **検証:** エージェントのアイデンティティクレデンシャル (鍵/証明書/トークン) は定義されたスケジュールと侵害の兆候で入れ替わり、傷害の疑いやなりすましの試みですぐに失効して隔離している。 | 3 | D/V |

---

## C9.5 安全なメッセージングとプロトコルの堅牢化 (Secure Messaging and Protocol Hardening)

Protect agent-to-agent and agent-to-tool communications from hijacking, injection, replay, and desynchronization.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.5.1** | **Verify that** agent-to-agent and agent-to-tool channels enforce mutual authentication and encryption with modern protocols (e.g., TLS 1.3) and strong certificate/token validation. | 1 | D/V |
| **9.5.2** | **Verify that** all messages are strictly schema-validated; unknown fields, malformed payloads, and oversized frames are rejected. | 1 | D/V |
| **9.5.3** | **Verify that** message integrity covers the full payload including tool parameters, and that replay protections (nonces/sequence numbers/timestamp windows) are enforced. | 2 | D/V |

---

## C9.6 認可、委譲、継続的施行 (Authorization, Delegation, and Continuous Enforcement)

Ensure every action is authorized at execution time and constrained by scope.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.6.1** | **Verify that** agent actions are authorized against fine-grained policies that restrict which tools an agent may invoke and which parameters it may supply (allow-list plus parameter constraints), enforced by the runtime. | 2 | D/V |
| **9.6.2** | **Verify that** when an agent acts on a user’s behalf, the runtime propagates an integrity-protected delegation context (user ID, tenant, session, scopes) and enforces that context at every downstream call without using the user’s credentials. | 2 | D/V |
| **9.6.3** | **Verify that** authorization is re-evaluated on every call (continuous authorization) using current context (tenant, environment, data classification, time, risk), and that delegated scopes are time-bound and automatically expire. | 3 | D/V |

---

## C9.7 意思の検証と制約ゲート (Intent Verification and Constraint Gates)

Prevent “technically authorized but unintended” actions by binding execution to user intent and hard constraints.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.7.1** | **Verify that** pre-execution gates evaluate proposed actions and parameters against hard policy constraints (deny rules, data handling constraints, allow-lists, side-effect budgets) and block execution on any violation. | 1 | D/V |
| **9.7.2** | **Verify that** high-impact actions require explicit user intent confirmation that is integrity-protected and bound to the exact action parameters (and expires quickly) to prevent stale or substituted approvals. | 2 | D/V |
| **9.7.3** | **Verify that** post-condition checks confirm the intended outcome and detect unintended side effects; any mismatch triggers containment (and compensating actions where supported). | 2 | V |

---

## C9.8 マルチエージェントのドメイン分離と群集リスク制御 (Multi-Agent Domain Isolation and Swarm Risk Controls)

Reduce cross-domain interference and emergent unsafe collective behavior.

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.8.1** | **Verify that** agents in different tenants, security domains, or environments (dev/test/prod) run in isolated runtimes and network segments, with default-deny controls that prevent cross-domain discovery and calls. | 1 | D/V |
| **9.8.2** | **Verify that** runtime monitoring detects unsafe emergent behavior (oscillation, deadlocks, uncontrolled broadcast, abnormal call graphs) and automatically applies corrective actions (throttle, isolate, terminate). | 3 | D/V |

---

## C9.9 モデルコンテキストプロトコル (MCP) セキュリティ (Model Context Protocol (MCP) Security)

Ensure secure discovery, authentication, authorization, transport, and use of MCP-based tool and resource integrations to prevent context confusion, unauthorized tool invocation, or cross-tenant data exposure.

### コンポーネントの完全性とサプライチェーンの衛生管理 (Component Integrity & Supply Chain Hygiene)

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.9.1** | **Verify that** MCP server and client components are obtained only from trusted sources and verified using signatures, checksums, or secure package metadata, rejecting tampered or unsigned builds. | 1 | D/V |

### 認証と認可 (Authentication & Authorization)

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.9.2** | **Verify that** MCP clients and servers mutually authenticate using strong, non-user credentials (e.g., mTLS, DPoP), and that unauthenticated MCP requests are rejected. | 2 | D/V |
| **9.9.3** | **Verify that** MCP servers are registered through a controlled technical onboarding mechanism requiring explicit owner, environment, and resource definitions; unregistered or undiscoverable servers must not be callable in production. | 2 | D/V |
| **9.9.4** | **Verify that** each MCP tool or resource defines explicit authorization scopes (e.g., read-only, restricted queries, side-effect levels), and that agents cannot invoke MCP functions outside their assigned scope. | 2 | D/V |

### 安全なトランスポートとネットワーク境界保護 (Secure Transport & Network Boundary Protection)

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.9.5** | **Verify that** authenticated, encrypted streamable-HTTP is used as the primary MCP transport in production environments; alternate transports (stdio, SSE) are restricted to local or tightly controlled environments with explicit justification. | 2 | D/V |
| **9.9.6** | **Verify that** streamable-HTTP MCP transports use authenticated, encrypted channels (TLS 1.3 or later) with certificate validation. | 2 | D/V |
| **9.9.7** | **Verify that** SSE-based MCP transports are used only within private, authenticated internal channels and enforce TLS, authentication, schema validation, payload size limits, and rate limiting; SSE endpoints must not be exposed to the public internet. | 2 | D/V |
| **9.9.8** | **Verify that** MCP servers validate the `Origin` and `Host` headers on all HTTP-based transports (including SSE and streamable-HTTP) to prevent DNS rebinding attacks, and reject requests from untrusted, mismatched, or missing origins. | 2 | D/V |

### スキーマ、メッセージ、入力バリデーション (Schema, Message, and Input Validation)

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.9.9** | **Verify that** MCP tool and resource schemas (e.g., JSON schemas or capability descriptors) are validated for authenticity and integrity using signatures to prevent schema tampering or malicious parameter modification. | 2 | D/V |
| **9.9.10** | **Verify that** all MCP transports enforce message-framing integrity, strict schema validation, maximum payload sizes, and rejection of malformed, truncated, or interleaved frames to prevent desynchronization or injection attacks. | 2 | D/V |
| **9.9.11** | **Verify that** MCP servers perform strict input validation for all function calls, including type checking, boundary checking, enumeration enforcement, and rejection of unrecognized or oversized parameters. | 2 | D/V |

### アウトバウンドアクセスとエージェント実行の安全性 (Outbound Access & Agent Execution Safety)

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.9.12** | **Verify that** MCP servers may only initiate outbound requests to approved internal or external destinations following least-privilege egress policies, and cannot access arbitrary network targets or internal cloud metadata services. | 2 | D/V |
| **9.9.15** | **Verify that** outbound MCP actions implement execution limits (timeouts, recursion limits, concurrency caps, circuit breakers) to prevent unbounded agent-driven tool invocation or chained side effects. | 2 | D/V |

### トランスポート制限と高リスク境界管理 (Transport Restrictions & High-Risk Boundary Controls)

| # | 説明 | レベル | ロール |
| :--: | --- | :---: | :--: |
| **9.9.16** | **Verify that** stdio-based MCP transports are limited to co-located, single-process development scenarios, isolated from shell execution, terminal injection, and process-spawning capabilities; stdio must never cross network or multi-tenant boundaries. | 3 | D/V |
| **9.9.17** | **Verify that** MCP servers expose only allow-listed functions and resources, and prohibit dynamic dispatch, reflective invocation, or execution of function names influenced by user or model-provided input. | 3 | D/V |
| **9.9.18** | **Verify that** tenant boundaries, environment boundaries (dev/test/prod), and data domain boundaries are enforced at the MCP layer, preventing cross-tenant or cross-environment server or resource discovery. | 3 | D/V |

---

## 参考情報

* [Model Context Protocol (MCP) Specification](https://modelcontextprotocol.io/)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final)
