# C10 モデルコンテキストプロトコル (MCP) セキュリティ (Model Context Protocol (MCP) Security)

## 管理目標

コンテキストの混乱、不正なツール呼び出し、テナント間のデータ露出を防ぐため、MCP ベースのツールとリソースの統合における安全な発見、認証、認可、トランスポート、使用を確保します。

---

## C10.1 コンポーネントの完全性とサプライチェーンの衛生管理 (Component Integrity & Supply Chain Hygiene)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.1.1** | **検証:** MCP サーバーおよびクライアントコンポーネントは信頼できるソースからのみ取得され、署名、チェックサム、または安全なパッケージメタデータを使用して検証され、改竄されたビルドや署名されていないビルドは拒否されている。 | 1 |
| **10.1.2** | **Verify that** only allowlisted MCP server identifiers (name, version, and registry) are permitted in production and that the runtime rejects connections to unlisted or unregistered servers at load time. | 1 |

---

## C10.2 認証と認可 (Authentication & Authorization)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.2.1** | **検証:** MCP クライアントは OAuth 2.1 認可フレームワークを使用して MCP サーバーに認証し、リクエストごとに有効な OAuth アクセストークンを提示しており、MCP サーバーは OAuth 2.1 リソースサーバー要件に従ってトークンを検証している。 | 1 |
| **10.2.2** | **検証:** MCP サーバーは、発行者、対象者、有効期限、スコープクレームを含む OAuth アクセストークンを検証し、ツール呼び出しを許可する前に特定の MCP サーバーに対してトークンが発行されていることを確保している。 | 1 |
| **10.2.3** | **検証:** MCP サーバーは、明示的な所有者、環境、リソースの定義を必要とする制御された技術的なオンボーディングメカニズムを通じて登録されている。登録されていないサーバーや発見できないサーバーは本番環境で呼び出すことはできない。 | 1 |
| **10.2.4** | **検証:** MCP レイヤでの認可決定は MCP サーバーのポリシーロジックによって強制されており、モデルが生成する出力はアクセス制御チェックに影響を及ぼしたり、上書きしたり、バイパスすることはできない。 | 1 |
| **10.2.5** | **検証:** MCP サーバーは、外部認可サーバーによって発行されたトークンを検証し、トークンやユーザークレデンシャルを保存しないことによってのみ、OAuth 2.1 リソースサーバーとして機能している。 | 2 |
| **10.2.6** | **検証:** MCP `tools/list` およびリソース発見レスポンスはエンドユーザーの認可済みスコープに基づいてフィルタされており、エージェントはユーザーが呼び出すことを許可されているツールとリソースの定義のみを受け取っている。 | 2 |
| **10.2.7** | **検証:** MCP サーバーはすべてのツール呼び出しに対してアクセス制御を実施し、ユーザーのアクセストークンはリクエストされたツールと指定された特定の引数値の両方を認可することを検証している。 | 2 |
| **10.2.8** | **Verify that** MCP session identifiers are treated as state, not identity: generated using cryptographically secure random values, bound to the authenticated user, and never relied on for authentication or authorization decisions. | 1 |
| **10.2.9** | **Verify that** MCP servers do not pass through access tokens received from clients to downstream APIs and instead obtain a separate token scoped to the server's own identity (e.g., via on-behalf-of or client credentials flow). | 2 |
| **10.2.10** | **Verify that** MCP servers acting as OAuth proxies to third-party APIs enforce per-client consent before forwarding authorization requests, preventing cached approvals from being reused across dynamically registered clients. | 2 |
| **10.2.11** | **Verify that** MCP clients request only the minimum scopes needed for the current operation and elevate progressively via step-up authorization for higher-privilege operations. | 2 |
| **10.2.14** | **Verify that** MCP servers reject wildcard or overly broad scope requests, enforcing that granted scopes are limited to those explicitly required for the requested operation. | 2 |
| **10.2.12** | **Verify that** MCP servers enforce deterministic session teardown, destroying cached tokens, in-memory state, temporary storage, and file handles when a session terminates, disconnects, or times out. | 2 |
| **10.2.13** | **Verify that** autonomous agents authenticate using cryptographically bound identity credentials (e.g., key-based proof-of-possession) rather than bearer-only tokens, ensuring that agent identity cannot be transferred, replayed, or impersonated by forwarding a shared secret. | 2 |

---

## C10.3 安全なトランスポートとネットワーク境界保護 (Secure Transport & Network Boundary Protection)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.3.1** | **検証:** 認証され、暗号化されたストリーミング可能な HTTP は本番環境のプライマリ MCP トランスポートとして使用されており、代替トランスポート (stdio や SSE など) は明示的な理由でローカルまたは厳密に制御された環境に制限されている。 | 2 |
| **10.3.2** | **検証:** ストリーミング可能な HTTP MCP トランスポートは証明書バリデーションを備えた認証済みの暗号化チャネル (TLS 1.3 以降) を使用している。 | 2 |
| **10.3.3** | **検証:** SSE ベースの MCP トランスポートはプライベートで、認証された内部チャネル内でのみ使用されており、パブリックインターネットに公開されていない。 | 2 |
| **10.3.6** | **Verify that** SSE-based MCP transport endpoints enforce TLS, authentication, schema validation, payload size limits, and rate limiting. | 2 |
| **10.3.4** | **検証:** MCP サーバーは、DNS 再バインディング攻撃を防ぐために、すべての HTTP ベースのトランスポート (SSE およびストリーミング可能な HTTP を含む) の `Origin` ヘッダと `Host` ヘッダを検証しており、信頼できない、一致しない、または欠落しているオリジンからのリクエストを拒否している。 | 2 |
| **10.3.5** | **Verify that** intermediaries do not alter or remove the `Mcp-Protocol-Version` header on streamable-HTTP transports unless explicitly required by the protocol specification, preventing protocol downgrade via header stripping. | 2 |

---

## C10.4 スキーマ、メッセージ、入力バリデーション (Schema, Message, and Input Validation)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.4.1** | **検証:** MCP ツールのレスポンスは、プロンプトインジェクション、悪意のあるツール出力、コンテキスト操作を防ぐために、モデルコンテキストに注入される前に検証されている。 | 1 |
| **10.4.2** | **検証:** MCP ツールおよびリソーススキーマ (JSON スキーマや機能記述子など) は、スキーマの改竄や悪意のあるパラメータ改変を防ぐために、署名を使用して真正性と完全性を検証されている。 | 2 |
| **10.4.3** | **検証:** すべての MCP トランスポートは、非同期攻撃やインジェクション攻撃を防ぐために、メッセージフレーミングの完全性、厳密なスキーマバリデーション、最大ペイロードサイズを強制しており、不正なフレーム、切り捨てられたフレーム、不連続なフレームを拒否している。 | 2 |
| **10.4.4** | **検証:** MCP サーバーは、型チェック、境界バリデーション、列挙の強制など、すべての関数呼び出しに対して厳密な入力バリデーションを実施しており、認識されないパラメータやサイズが大きすぎるパラメータを拒否している。 | 2 |
| **10.4.5** | **Verify that** MCP clients maintain a hash or versioned snapshot of tool definitions and that any change to a tool definition (via `notifications/tools/list_changed` or between sessions) triggers re-approval before the modified tool can be invoked. | 2 |
| **10.4.6** | **Verify that** MCP server error and exception responses do not expose stack traces, internal file paths, tokens, or tool implementation details to the client or model context. | 1 |
| **10.4.7** | **Verify that** MCP implementations reject JSON-RPC messages containing duplicate keys at any nesting level, preventing parser disagreement where different components resolve the same message to different values. | 2 |
| **10.4.8** | **Verify that** intermediaries evaluating message content either forward the canonicalized representation they evaluated or reject messages where multiple byte representations could produce different parsed structures. | 3 |
| **10.4.9** | **Verify that** MCP implementations support per-message cryptographic signing using asymmetric algorithms (e.g., ECDSA) so that each tool call and response carries a verifiable signature binding the message content to the sender's identity, preventing undetected message modification between MCP endpoints. | 2 |
| **10.4.10** | **Verify that** MCP implementations enforce application-layer replay protection by requiring a unique nonce and a timestamp within a bounded time window for each signed message, and that the receiver rejects messages with duplicate nonces or timestamps outside the permitted window. | 2 |
| **10.4.11** | **Verify that** MCP servers sign tool responses so that the calling agent can verify that the response originated from the expected server and was not modified in transit, preventing tool response spoofing or tampering by intermediaries. | 2 |

---

## C10.5 アウトバウンドアクセスとエージェント実行の安全性 (Outbound Access & Agent Execution Safety)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.5.1** | **検証:** MCP サーバーは最小権限の送出 (egress) ポリシーに従って承認された内部または外部の宛先へのアウトバウンドリクエストのみを開始でき、任意のネットワークターゲットや内部クラウドメタデータサービスにはアクセスできない。 | 2 |
| **10.5.2** | **検証:** アウトバウンド MCP アクションは、無制限のエージェント駆動型ツールの呼び出しや連鎖的な副作用を防ぐために、、実行制限 (タイムアウト、再帰制限、並列実行上限、サーキットブレーカーなど) を実装している。 | 2 |
| **10.5.3** | **Verify that** MCP tool invocations classified as high-risk or destructive (e.g., data deletion, financial transactions, system configuration changes) require explicit user confirmation before execution. | 2 |

---

## C10.6 トランスポート制限と高リスク境界管理 (Transport Restrictions & High-Risk Boundary Controls)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.6.1** | **検証:** stdio ベースの MCP トランスポートは、シェル実行、ターミナルインジェクション、プロセス生成機能から分離された、共存する単一プロセス開発シナリオに制限されている。stdio はネットワークやマルチテナント境界を超えてはいけない。 | 3 |
| **10.6.2** | **検証:** MCP サーバーは許可リストにある機能とリソースのみを露出しており、ユーザーまたはモデルが提供する入力によって影響を受ける関数名の動的ディスパッチ、リフレクション呼び出し、実行を禁止している。 | 3 |
| **10.6.3** | **検証:** テナント境界、環境境界 (開発/テスト/本番)、データドメイン境界は MCP レイヤで強制されており、テナント間や環境間のサーバーやリソースの発見を防いでいる。 | 3 |
| **10.6.4** | **Verify that** MCP security controls enforce fail-closed semantics: if a signature verification, authentication check, or policy evaluation fails or cannot be completed, the default action is to deny the request rather than permit it. | 2 |

---

## 参考情報

* [Model Context Protocol (MCP) Specification](https://modelcontextprotocol.io/)
* [OWASP MCP Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/detail/sp/800-207/final)
