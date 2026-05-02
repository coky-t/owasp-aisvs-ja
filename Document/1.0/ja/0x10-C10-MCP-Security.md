# C10 モデルコンテキストプロトコル (MCP) セキュリティ (Model Context Protocol (MCP) Security)

## 管理目標

コンテキストの混乱、不正なツール呼び出し、テナント間のデータ露出を防ぐため、MCP ベースのツールとリソースの統合における安全な発見、認証、認可、トランスポート、使用を確保します。

---

## C10.1 コンポーネントの完全性とサプライチェーンの衛生管理 (Component Integrity & Supply Chain Hygiene)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.1.1** | **検証:** MCP サーバーおよびクライアントコンポーネントは信頼できるソースからのみ取得され、署名、チェックサム、または安全なパッケージメタデータを使用して検証され、改竄されたビルドや署名されていないビルドは拒否されている。 | 1 |
| **10.1.2** | **Verify that** only allowlisted MCP server identifiers (name, version, and registry) are permitted in production and that the runtime rejects connections to unlisted or unregistered servers at load time. | 1 |
| **10.1.3** | **Verify that** all MCP tool and resource schemas include cryptographically verifiable provenance metadata — including author, timestamp, version hash, signature, and approved‑by fields. | 2 |

---

## C10.2 認証と認可 (Authentication & Authorization)

> **Scope note:** General agent authorization principles (model output must not override access control, delegation context, continuous authorization) are covered in C9.6. Controls in this section address MCP-protocol-specific authentication and authorization mechanisms. Refer to ASVSv5 V10 for OAuth specific details.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.2.1** | **Verify that** MCP clients and servers implement the OAuth 2.1 authorization framework: clients present a valid access token for each request, and servers validate the token's issuer, audience, expiration, and scope claims, acting as resource servers that do not store tokens or user credentials. | 1 |
| **10.2.2** | **検証:** MCP サーバーは、明示的な所有者、環境、リソースの定義を必要とする制御された技術的なオンボーディングメカニズムを通じて登録されている。登録されていないサーバーや発見できないサーバーは本番環境で呼び出すことはできない。 | 1 |
| **10.2.3** | **Verify that** MCP session identifiers are treated as state, not identity: generated using cryptographically secure random values, bound to the authenticated user, and never relied on for authentication or authorization decisions. | 1 |
| **10.2.4** | **検証:** MCP `tools/list` およびリソース発見レスポンスはエンドユーザーの認可済みスコープに基づいてフィルタされており、エージェントはユーザーが呼び出すことを許可されているツールとリソースの定義のみを受け取っている。 | 2 |
| **10.2.5** | **検証:** MCP サーバーはすべてのツール呼び出しに対してアクセス制御を実施し、ユーザーのアクセストークンはリクエストされたツールと指定された特定の引数値の両方を認可することを検証している。 | 2 |
| **10.2.6** | **Verify that** MCP servers do not pass through access tokens received from clients to downstream APIs and instead obtain a separate token scoped to the server's own identity (e.g., via on-behalf-of or client credentials flow). | 2 |
| **10.2.7** | **Verify that** MCP clients request only the minimum scopes needed for the current operation and elevate progressively via step-up authorization for higher-privilege operations. | 2 |
| **10.2.8** | **Verify that** MCP servers enforce deterministic session teardown, destroying cached tokens, in-memory state, temporary storage, and file handles when a session terminates, disconnects, or times out. | 2 |
| **10.2.9** | **Verify that** autonomous agents authenticate using cryptographically bound identity credentials (e.g., key-based proof-of-possession) rather than bearer-only tokens, ensuring that agent identity cannot be transferred, replayed, or impersonated by forwarding a shared secret. | 2 |

---

## C10.3 安全なトランスポートとネットワーク境界保護 (Secure Transport & Network Boundary Protection)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.3.1** | **検証:** 認証され、暗号化されたストリーミング可能な HTTP は本番環境のプライマリ MCP トランスポートとして使用されており、代替トランスポート (stdio や SSE など) は明示的な理由でローカルまたは厳密に制御された環境に制限されている。 | 1 |
| **10.3.2** | **Verify that** SSE-based MCP transports are used only within private, authenticated internal channels with TLS, schema validation, payload size limits, and rate limiting enforced, and are not exposed to the public internet. | 2 |
| **10.3.3** | **検証:** MCP サーバーは、DNS 再バインディング攻撃を防ぐために、すべての HTTP ベースのトランスポート (SSE およびストリーミング可能な HTTP を含む) の `Origin` ヘッダと `Host` ヘッダを検証しており、信頼できない、一致しない、または欠落しているオリジンからのリクエストを拒否している。 | 2 |
| **10.3.4** | **Verify that** intermediaries do not alter or remove the `Mcp-Protocol-Version` header on streamable-HTTP transports unless explicitly required by the protocol specification, preventing protocol downgrade via header stripping. | 2 |
| **10.3.5** | **Verify that** SSE-based MCP transport endpoints enforce TLS, authentication, schema validation, payload size limits, and rate limiting. | 2 |
| **10.3.6** | **Verify that** MCP clients enforce a minimum acceptable protocol version and reject `initialize` responses that propose a version below that minimum, preventing a server or intermediary from forcing use of a protocol version with weaker security properties. | 2 |

---

## C10.4 スキーマ、メッセージ、入力バリデーション (Schema, Message, and Input Validation)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.4.1** | **検証:** MCP ツールのレスポンスは、プロンプトインジェクション、悪意のあるツール出力、コンテキスト操作を防ぐために、モデルコンテキストに注入される前に検証されている。 | 1 |
| **10.4.2** | **Verify that** MCP server error responses do not expose internal details (stack traces, file paths, tokens, tool implementation) to the client or model context, preventing information leakage that could be exploited via the model. | 1 |
| **10.4.3** | **検証:** すべての MCP トランスポートは、非同期攻撃やインジェクション攻撃を防ぐために、メッセージフレーミングの完全性、厳密なスキーマバリデーション、最大ペイロードサイズを強制しており、不正なフレーム、切り捨てられたフレーム、不連続なフレームを拒否している。 | 2 |
| **10.4.4** | **検証:** MCP サーバーは、型チェック、境界バリデーション、列挙の強制など、すべての関数呼び出しに対して厳密な入力バリデーションを実施しており、認識されないパラメータやサイズが大きすぎるパラメータを拒否している。 | 2 |
| **10.4.5** | **Verify that** MCP clients maintain a hash or versioned snapshot of tool definitions and that any change to a tool definition (via `notifications/tools/list_changed` or between sessions) triggers re-approval before the modified tool can be invoked. | 2 |
| **10.4.6** | **検証:** MCP ツールおよびリソーススキーマ (JSON スキーマや機能記述子など) とスキーママニフェストは、スキーマの改竄や悪意のあるパラメータ改変を防ぐために、署名を使用して真正性と完全性を検証されている。 | 3 |
| **10.4.7** | **Verify that** intermediaries evaluating message content either forward the canonicalized representation they evaluated or reject messages where multiple byte representations could produce different parsed structures. | 3 |
| **10.4.8** | **Verify that** MCP servers sign tool responses with a unique nonce and timestamp within a bounded time window so that the calling agent can verify origin, integrity, and freshness, preventing spoofing, tampering, and replay of tool responses by intermediaries. | 3 |

---

## C10.5 アウトバウンドアクセスとエージェント実行の安全性 (Outbound Access & Agent Execution Safety)

> **Scope note:** Execution budgets, circuit breakers, and human approval gates for agent-initiated actions (including MCP tool invocations) are covered in C9.1 and C9.2. Controls in this section address MCP-specific outbound network constraints.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.5.1** | **検証:** MCP サーバーは最小権限の送出 (egress) ポリシーに従って承認された内部または外部の宛先へのアウトバウンドリクエストのみを開始でき、任意のネットワークターゲットや内部クラウドメタデータサービスにはアクセスできない。 | 2 |

---

## C10.6 トランスポート制限と高リスク境界管理 (Transport Restrictions & High-Risk Boundary Controls)

> **Scope note:** General agent sandbox isolation is covered in C9.3. Tenant and environment isolation for multi-agent systems is covered in C9.8. Controls in this section address MCP-specific transport and dispatch restrictions.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.6.1** | **Verify that** MCP security controls enforce fail-closed semantics: if tool schema validation, MCP-Protocol-Version negotiation fails, authentication check, or policy evaluation fails or cannot be completed, the default action is to deny the request rather than permit it. | 2 |
| **10.6.2** | **検証:** MCP サーバーは許可リストにある機能とリソースのみを露出しており、ユーザーまたはモデルが提供する入力によって影響を受ける関数名の動的ディスパッチ、リフレクション呼び出し、実行を禁止している。 | 3 |

---

## 参考情報

* [Model Context Protocol (MCP) Specification](https://modelcontextprotocol.io/)
* [OWASP MCP Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
