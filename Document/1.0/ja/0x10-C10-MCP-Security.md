# C10 モデルコンテキストプロトコル (MCP) セキュリティ (Model Context Protocol (MCP) Security)

## 管理目標

コンテキストの混乱、不正なツール呼び出し、テナント間のデータ露出を防ぐため、MCP ベースのツールとリソースの統合における安全な発見、認証、認可、トランスポート、使用を確保します。This chapter covers MCP-protocol-specific controls.

---

## C10.1 コンポーネントの完全性 (Component Integrity)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.1.1** | **検証:** MCP は信頼できるソースからのみ取得され、暗号検証されている。 | 1 |
| **10.1.2** | **Verify that** only allowlisted MCP servers are permitted. | 2 |

---

## C10.2 認証と認可 (Authentication & Authorization)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.2.1** | **Verify that** MCP servers validate access tokens for each request and do not rely on transport security alone. | 1 |
| **10.2.2** | **Verify that** MCP servers validate the presented access token's issuer, audience, expiration, and scope claims in accordance with OAuth 2.1. | 1 |
| **10.2.3** | **Verify that** MCP servers acting as OAuth 2.1 resource servers do not store or persist access tokens or user credentials. | 1 |
| **10.2.4** | **Verify that** MCP tools/list only returns tools that the resource owners authorized scopes allow. | 2 |
| **10.2.5** | **検証:** MCP サーバーはすべてのツール呼び出しに対してアクセス制御を実施し、ユーザーのアクセストークンはリクエストされたツールと指定された特定の引数値の両方を認可することを検証している。 | 2 |
| **10.2.6** | **Verify that** MCP servers ensure that all session artifacts are removed when a session terminates. | 2 |
| **10.2.7** | **Verify that** MCP servers do not pass through access tokens received from clients to downstream APIs. | 2 |

---

## C10.3 安全なトランスポート (Secure Transport)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.3.1** | **検証:** 認証され、暗号化されたストリーミング可能な HTTP はリモートサービスのための MCP トランスポートとして使用されている。 | 1 |
| **10.3.2** | **Verify that** stdio transport is permitted only in controlled local environments. | 1 |
| **10.3.3** | **検証:** MCP サーバーは、DNS 再バインディング攻撃を防ぐために、すべての HTTP ベースのトランスポートの Origin ヘッダと Host ヘッダの両方を個別に検証している。 | 2 |
| **10.3.4** | **Verify that** MCP clients enforce a minimum acceptable protocol version and reject initialize responses that propose a version below that minimum. | 2 |
| **10.3.5** | **Verify that** access tokens between the MCP client and server are sender-constrained using mTLS or DPoP. | 3 |

---

## C10.4 スキーマ、メッセージ、入力バリデーション (Schema, Message, and Input Validation)

| # | 説明 | レベル |
| :--: | --- | :---: |
| **10.4.1** | **検証:** MCP ツール/リストおよびツールのレスポンスは、間接プロンプトインジェクションを防ぐために、プロンプトインジェクションガードレールシステムを介して検証されている。 | 1 |
| **10.4.2** | **Verify that** MCP tools/list and tool responses are schema validated before being injected into the model context. | 1 |
| **10.4.3** | **Verify that** MCP servers reject unrecognized or oversized parameters in function calls. | 1 |
| **10.4.4** | **Verify that** all MCP servers enforce strict schema validation. | 2 |
| **10.4.5** | **Verify that** all MCP transports enforce maximum payload size limits. | 2 |
| **10.4.6** | **Verify that** MCP servers sign tool responses with a unique nonce and timestamp so that MCP clients can avoid replay attacks. | 2 |
| **10.4.7** | **Verify that** MCP clients maintain a snapshot of tool definitions and that any change to a tool definition triggers re-approval before the modified tool can be invoked. | 3 |

---

## 参考情報

* [Model Context Protocol (MCP) Specification](https://modelcontextprotocol.io/)
* [OWASP MCP Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
