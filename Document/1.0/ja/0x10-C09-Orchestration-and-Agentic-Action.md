# C9 自律オーケストレーションとエージェントアクションセキュリティ (Autonomous Orchestration & Agentic Action Security)

## 管理目標

自律型またはマルチエージェント型の AI システムが、明示的に意図され、認証され、監査可能であり、コストとリスクの閾値の境界内でのアクション **のみ** を実行できるようにします。これは、自律システムの侵害、ツールの不正使用、エージェントループの検出、通信のハイジャック、アイデンティティのなりすまし、スウォーム操作、インテント操作などの脅威から保護します。

---

## 9.1 エージェントのタスク計画と再帰予算 (Agent Task-Planning & Recursion Budgets)

再帰計画を抑制し、特権アクションに対して人間によるチェックポイントを強制します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.1.1** | **検証:** エージェント実行あたりの最大再帰深度、幅、経過実時間、トークン、金銭コストは集中的に構成され、バージョン管理されている。 | 1 | D/V |
| **9.1.2** | **検証:** 特権アクションや取り消し不可能なアクション (コードコミット、送金など) は、実行前に、監査可能なチャネルを介して人間による明示的な承認を必要としている。 | 1 | D/V |
| **9.1.3** | **検証:** リアルタイムリソースモニターは予算閾値を超えるとサーキットブレーカー不通をトリガーし、それ以上のタスク拡張を停止している。 | 2 | D |
| **9.1.4** | **検証:** サーキットブレーカーイベントは、フォレンジックレビューのために、エージェント ID、トリガー条件、キャプチャされた計画の状態とともにログ記録されている。 | 2 | D/V |
| **9.1.5** | **検証:** セキュリティテストは予算枯渇や暴走計画のシナリオをカバーし、データ損失のない安全な停止を確認している。 | 3 | V |
| **9.1.6** | **検証:** 予算ポリシーは Policy as Code として表現されており、構成ドリフトをブロックするために CI/CD に適用されている。 | 3 | D |

---

## 9.2 ツールプラグインのサンドボックス化 (Tool Plugin Sandboxing)

ツールのインタラクションを分離して、不正なシステムアクセスやコード実行を防止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.2.1** | **検証:** すべてのツール/プラグインは、最小権限のファイルシステム、ネットワーク、システムコールポリシーを備えた、OS、コンテナ、または WASM レベルのサンドボックス内で実行している。 | 1 | D/V |
| **9.2.2** | **検証:** サンドボックスのリソースクォータ (CPU、メモリ、ディスク、ネットワーク送出 (egress)) と実行タイムアウトが適用され、ログ記録されている。 | 1 | D/V |
| **9.2.3** | **検証:** ツールのバイナリまたは記述子はデジタル署名されている。署名はロード前に検証されている。 | 2 | D/V |
| **9.2.4** | **検証:** サンドボックステレメトリは SIEM に配信している。異常 (送信接続の試行など) はアラートを発している。 | 2 | V |
| **9.2.5** | **検証:** 高リスクのプラグインは、本番デプロイメント前に、セキュリティレビューとペネトレーションテストを受けている。 | 3 | V |
| **9.2.6** | **検証:** サンドボックス脱出の試行は自動的にブロックされており、問題のあるプラグインは調査中隔離されている。 | 3 | D/V |

---

## 9.3 自律ループとコスト制限 (Autonomous Loop & Cost Bounding)

制御されていないエージェント間の再帰とコストの爆発を検出して停止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.3.1** | **検証:** エージェント間呼び出しは、ランタイムが減少と強制する、ホップ制限または TTL を含んでいる。 | 1 | D/V |
| **9.3.2** | **検証:** エージェントは、自己呼び出しや周期的なパターンを見つけるために、一意の呼び出しグラフ ID を維持している。 | 2 | D |
| **9.3.3** | **検証:** 累積計算ユニットと支出カウンタはリクエストチェーンごとに追跡されている。制限を超えるとチェーンを中止している。 | 2 | D/V |
| **9.3.4** | **検証:** 形式解析またはモデル検査はエージェントプロトコルに無制限の再帰がないことを論証している。 | 3 | V |
| **9.3.5** | **検証:** ループ中止イベントはアラートを生成し、継続的な改善メトリクスをフィードしている。 | 3 | D |

---

## 9.4 プロトコルレベルの不正使用防止 (Protocol-Level Misuse Protection)

エージェントと外部システム間の通信チャネルを保護し、ハイジャックや操作を防止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.4.1** | **検証:** すべてのエージェントとツールの間およびエージェント間のメッセージは認証 (相互 TLS や JWT など) されており、エンドツーエンドで暗号化されている。 | 1 | D/V |
| **9.4.2** | **検証:** スキーマは厳密に検証されている。不明なフィールドや不正なメッセージは拒否されている。 | 1 | D |
| **9.4.3** | **検証:** 完全性チェック (MAC またはデジタル署名) は、ツールパラメータを含む、メッセージペイロード全体をカバーしている。 | 2 | D/V |
| **9.4.4** | **検証:** リプレイ保護 (ノンスまたはタイムスタンプウィンドウ) はプロトコル層で適用されている。 | 2 | D |
| **9.4.5** | **検証:** プロトコル実装は、インジェクションやデシリアライゼーションの欠陥について、ファジングと静的解析を受けている。 | 3 | V |

---

## 9.5 エージェントアイデンティティと改竄防止 (Agent Identity & Tamper-Evidence)

アクションが帰属可能であり、変更が検出可能であることを確保します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.5.1** | **検証:** 各エージェントインスタンスは一意の暗号アイデンティティ (鍵ペアまたはハードウェアルートのクレデンシャル) を保持している。 | 1 | D/V |
| **9.5.2** | **検証:** すべてのエージェントアクションは署名され、タイムスタンプ付けされている。ログは否認防止のための署名を含んでいる。 | 2 | D/V |
| **9.5.3** | **検証:** 改善防止ログは追記専用または一回のみ書き込み可能なメディアに保存されている。 | 2 | V |
| **9.5.4** | **検証:** アイデンティティキーは定義されたスケジュールと侵害インジケータで入れ替えられている。 | 3 | D |
| **9.5.5** | **検証:** なりすましまたはキー競合は影響を受けるエージェントの即時隔離をトリガーしている。 | 3 | D/V |

---

## 9.6 マルチエージェント群のリスク軽減 (Multi-Agent Swarm Risk Reduction)

隔離と形式安全性モデリングを通じて集団行動の危険性を緩和します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.6.1** | **検証:** 異なるセキュリティドメインで動作するエージェントは、分離されたランタイムサンドボックスまたはネットワークセグメントで実行している。 | 1 | D/V |
| **9.6.2** | **検証:** 群集の行動はモデル化されており、デプロイメント前に生存性と安全性について形式検証されている。 | 3 | V |
| **9.6.3** | **検証:** ランタイム監視は、発生する安全でないパターン (オシレーション、デッドロックなど) を検出し、矯正アクションを開始している。 | 3 | D |

---

## 9.7 ユーザーとツールの認証/認可 (User & Tool Authentication / Authorization)

エージェントによってトリガーされるすべてのアクションに対して堅牢なアクセス制御を実装します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.7.1** | **検証:** エージェントはダウンストリームのシステムに対してファーストクラスのプリンシパルとして認証しており、決してエンドユーザークレデンシャルを再使用していない。 | 1 | D/V |
| **9.7.2** | **検証:** きめ細かい認可ポリシーは、エージェントが呼び出せるツールと提供できるパラメータを制限している。 | 2 | D |
| **9.7.3** | **検証:** 権限チェックは、セッション開始時だけでなく、呼び出しごとに再評価 (継続的な認可) されている。 | 2 | V |
| **9.7.4** | **検証:** 委譲された権限は自動的に期限切れとなり、タイムアウトまたはスコープ変更の後に再度の同意を必要としている。 | 3 | D |

---

## 9.8 エージェント間通信セキュリティ (Agent-to-Agent Communication Security)

すべてのエージェント間メッセージを暗号化して完全性を保護し、盗聴や改竄を阻止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.8.1** | **検証:** 相互認証と完全前方秘匿性 (perfect-forward-secret) 暗号化 (TLS 1.3) はエージェントチャネルで必須としている。 | 1 | D/V |
| **9.8.2** | **検証:** メッセージの完全性と発信元は処理前に検証されている。失敗はアラートを発し、メッセージを取り下げている。 | 1 | D |
| **9.8.3** | **検証:** 通信メタデータ (タイムスタンプ、シーケンス番号) は、フォレンジック再構築をサポートするために、ログ記録されている。 | 2 | D/V |
| **9.8.4** | **検証:** 形式検証またはモデル検査は、プロトコル状態マシンが安全でない状態に陥らないことを確認している。 | 3 | V |

---

## 9.9 意思の検証と制約の適用 (Intent Verification & Constraint Enforcement)

エージェントアクションがユーザーの表明した意思とシステム制約に一致していることを検証します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.9.1** | **検証:** 実行前制約ソルバーは、ハードコードされた安全性およびポリシーのルールに対して、提案されたアクションをチェックしている。 | 1 | D |
| **9.9.2** | **検証:** 影響の大きいアクション (金銭的、破壊的、プライバシー配慮が必要) は、開始したユーザーからの明示的な意思の確認を必要としている。 | 2 | D/V |
| **9.9.3** | **検証:** 事後条件チェックは、完了したアクションが副作用なしに意図した効果を達成したことを検証している。不整合はロールバックをトリガーしている。 | 2 | V |
| **9.9.4** | **検証:** 形式手法 (モデルチェック、定理証明など) またはプロパティベースのテストは、エージェント計画が宣言されたすべての制約を満たしていることを実証している。 | 3 | V |
| **9.9.5** | **検証:** 意思の不一致や制約違反のインシデントは継続的な改善サイクルと脅威情報の共有につなげている。 | 3 | D |

---

## 9.10 エージェント推論戦略セキュリティ (Agent Reasoning Strategy Security)

ReAct, Chain-of-Thought, Tree-of-Thoughts アプローチを含むさまざまな推論戦略の選択と実行を保護します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.10.1** | **検証:** 推論戦略の選択は決定論的基準 (入力の複雑さ、タスクの種類、セキュリティコンテキスト) を使用しており、同一の入力は同じセキュリティコンテキスト内では同一の戦略選択を生成している。 | 1 | D/V |
| **9.10.2** | **検証:** 各推論戦略 (ReAct, Chain-of-Thought, Tree-of-Thoughts) はその認知アプローチに固有の専用の入力バリデーション、出力サニタイゼーション、実行時間制限を有している。 | 1 | D/V |
| **9.10.3** | **検証:** 推論戦略の遷移は、監査証跡の再構築のために、入力特性、選択基準値、実行メタデータなどの完全なコンテキストとともにログ記録されている。 | 2 | D/V |
| **9.10.4** | **検証:** Tree-of-Thoughts 推論は、ポリシー違反、リソース制限、安全境界が検出された際に、探索を終了するブランチプルーニングメカニズムを含んでいる。 | 2 | D/V |
| **9.10.5** | **検証:** ReAct (Reason-Act-Observe) サイクルは、推論ステップのバリデーション、アクションの認可、続行前の観察のサニタイゼーションといった、各フェーズでのバリデーションチェックポイントを含んでいる。 | 2 | D/V |
| **9.10.6** | **検証:** 推論戦略のパフォーマンスメトリクス (実行時間、リソース使用量、出力品質) は監視されており、設定された閾値をメトリクスが逸脱する場合に自動的にアラートしている。 | 3 | D/V |
| **9.10.7** | **検証:** 複数の戦略を組み合わせたハイブリッド推論アプローチは、セキュリティコントロールをバイパスすることなく、すべての構成戦略の入力バリデーションと出力制約を維持している。 | 3 | D/V |
| **9.10.8** | **検証:** 推論戦略のセキュリティテストは、不正な入力でのファジング、戦略の切り替えを強制するように設計された敵対的プロンプト、各認知アプローチの境界条件テストを含んでいる。 | 3 | D/V |

---

## 9.11 エージェントライフサイクル状態管理とセキュリティ (Agent Lifecycle State Management & Security)

暗号化された監査証跡と定義されたリカバリ手順で、エージェントの初期化、状態遷移、終了を保護します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.11.1** | **検証:** エージェントの初期化は、ハードウェア支援のクレデンシャルでの暗号化されたアイデンティティの確立と、エージェント ID、タイムスタンプ、構成ハッシュ、初期化パラメータを含む不変の起動監査ログを含んでいる。 | 1 | D/V |
| **9.11.2** | **検証:** エージェントの状態遷移は暗号論的に署名され、タイムスタンプ付けされ、トリガーイベント、以前の状態ハッシュ、新しい状態ハッシュ、実行されたセキュリティバリデーションなどの完全なコンテキストとともにログ記録されている。 | 2 | D/V |
| **9.11.3** | **検証:** エージェントのシャットダウン手順は、暗号消去またはマルチパス上書きを使用した安全なメモリ消去、認証局通知でのクレデンシャルの失効、改竄防止修了証明書の生成を含んでいる。 | 2 | D/V |
| **9.11.4** | **検証:** エージェントリカバリメカニズムは、暗号チェックサム (最低でも SHA-256) を使用して状態の完全性を検証しており、破損が検出された場合は自動アラートと手動承認要件で既知の正常な状態にロールバックしている。 | 3 | D/V |
| **9.11.5** | **検証:** エージェント永続化メカニズムは、エージェントごとに AES-256 鍵で機密状態データを暗号化し、構成可能なスケジュール (最大 90 日) においてダウンタイムなしのデプロイメントで安全な鍵ローテーションを実装している。 | 3 | D/V |

---

## 9.12 ツール統合セキュリティフレームワーク (Tool Integration Security Framework)

定義されたリスク評価と承認プロセスでの動的ツールローディング、実行、結果のバリデーションのためのセキュリティコントロールです。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.12.1** | **検証:** ツール記述子は、必要な権限 (読み取り/書き込み/実行)、リスクレベル (低/中/高)、 リソース制限 (CPU、メモリ、ネットワーク)、ツールマニフェストに記載されたバリデーション要件を指定するセキュリティメタデータを含んでいる。 | 1 | D/V |
| **9.12.2** | **検証:** ツール実行結果は、タイムアウト制限およびエラー処理手順との統合の前に、期待されるスキーマ (JSON スキーマ、XML スキーマ) およびセキュリティポリシー (出力サニタイゼーション、データ分類) に対して検証されている。 | 1 | D/V |
| **9.12.3** | **検証:** ツールインタラクションログは、SIEM 統合のための構造化ログで、権限の使用状況、データアクセスパターン、実行時間、リソース消費量、戻りコードなどの詳細なセキュリティコンテキストを含んでいる。 | 2 | D/V |
| **9.12.4** | **検証:** 動的ツールローディングメカニズムは PKI インフラストラクチャを使用してデジタル署名を検証し、実行前にサンドボックス分離とパーミッション検証で安全なローディングプロトコルを実装している。 | 2 | D/V |
| **9.12.5** | **検証:** ツールのセキュリティ評価は、静的解析、動的テスト、文書化された承認基準と SLA 要件でのセキュリティチームレビューなどの必須の承認ゲートで、新しいバージョンに対して自動的にトリガーされている。 | 3 | D/V |

---

## C9.13 Model Context Protocol (MCP) Security

Ensure secure discovery, authentication, authorization, transport, and use of MCP-based tool and resource integrations to prevent context confusion, unauthorized tool invocation, or cross-tenant data exposure.

### Component Integrity & Supply Chain Hygiene

| # | Description | Level | Role |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **9.13.1** | **Verify that** MCP server, client, and tool implementations are manually reviewed or automatically analyzed to identify insecure function exposure, unsafe defaults, missing authentication, or missing input validation. | 1 | D/V |
| **9.13.2** | **Verify that** external or open-source MCP servers or packages undergo automated vulnerability and supply-chain scanning (e.g., SCA) before integration, and that components with known critical vulnerabilities are not used. | 1 | D/V |
| **9.13.3** | **Verify that** MCP server and client components are obtained only from trusted sources and verified using signatures, checksums, or secure package metadata, rejecting tampered or unsigned builds. | 1 | D/V |

### Authentication & Authorization

| # | Description | Level | Role |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **9.13.4** | **Verify that** MCP clients and servers mutually authenticate using strong, non-user credentials (e.g., mTLS, signed tokens, or platform-issued identities), and that unauthenticated MCP endpoints are rejected. | 2 | D/V |
| **9.13.5** | **Verify that** MCP servers are registered through a controlled technical onboarding mechanism requiring explicit owner, environment, and resource definitions; unregistered or undiscoverable servers must not be callable in production. | 2 | D/V |
| **9.13.6** | **Verify that** each MCP tool or resource defines explicit authorization scopes (e.g., read-only, restricted queries, side-effect levels), and that agents cannot invoke MCP functions outside their assigned scope. | 2 | D/V |

### Secure Transport & Network Boundary Protection

| # | Description | Level | Role |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **9.13.7** | **Verify that** authenticated, encrypted streamable-HTTP is used as the primary MCP transport in production environments; alternate transports (stdio, SSE) are restricted to local or tightly controlled environments with explicit justification. | 2 | D/V |
| **9.13.8** | **Verify that** streamable-HTTP MCP transports use authenticated, encrypted channels (TLS 1.3 or later) with certificate validation and forward secrecy to ensure confidentiality and integrity of streamed MCP messages. | 2 | D/V |
| **9.13.9** | **Verify that** SSE-based MCP transports are used only within private, authenticated internal channels and enforce TLS, authentication, schema validation, payload size limits, and rate limiting; SSE endpoints must not be exposed to the public internet. | 2 | D/V |
| **9.13.10** | **Verify that** MCP servers validate the `Origin` and `Host` headers on all HTTP-based transports (including SSE and streamable-HTTP) to prevent DNS rebinding attacks, and reject requests from untrusted, mismatched, or missing origins. | 2 | D/V |

### Schema, Message, and Input Validation

| # | Description | Level | Role |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **9.13.11** | **Verify that** MCP tool and resource schemas (e.g., JSON schemas or capability descriptors) are validated for authenticity and integrity using signatures, checksums, or server attestation to prevent schema tampering or malicious parameter modification. | 2 | D/V |
| **9.13.12** | **Verify that** all MCP transports enforce message-framing integrity, strict schema validation, maximum payload sizes, and rejection of malformed, truncated, or interleaved frames to prevent desynchronization or injection attacks. | 2 | D/V |
| **9.13.13** | **Verify that** MCP servers perform strict input validation for all function calls, including type checking, boundary checking, enumeration enforcement, and rejection of unrecognized or oversized parameters. | 2 | D/V |

### Outbound Access & Agent Execution Safety

| # | Description | Level | Role |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **9.13.14** | **Verify that** MCP servers may only initiate outbound requests to approved internal or external destinations following least-privilege egress policies, and cannot access arbitrary network targets or internal cloud metadata services. | 2 | D/V |
| **9.13.15** | **Verify that** outbound MCP actions implement execution limits (timeouts, recursion limits, concurrency caps, circuit breakers) to prevent unbounded agent-driven tool invocation or chained side effects. | 2 | D/V |
| **9.13.16** | **Verify that** MCP request and response metadata (server ID, resource name, tool name, session identifier, tenant, environment) is logged with integrity protection and correlated to agent activity for forensic analysis. | 2 | D/V |

### Transport Restrictions & High-Risk Boundary Controls

| # | Description | Level | Role |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **9.13.17** | **Verify that** stdio-based MCP transports are limited to co-located, single-process development scenarios, isolated from shell execution, terminal injection, and process-spawning capabilities; stdio must never cross network or multi-tenant boundaries. | 3 | D/V |
| **9.13.18** | **Verify that** MCP servers expose only allow-listed functions and resources, and prohibit dynamic dispatch, reflective invocation, or execution of function names influenced by user or model-provided input. | 3 | D/V |
| **9.13.19** | **Verify that** tenant boundaries, environment boundaries (dev/test/prod), and data domain boundaries are enforced at the MCP layer, preventing cross-tenant or cross-environment server or resource discovery. | 3 | D/V |

---

### 参考情報

* [MITRE ATLAS tactics ML09](https://atlas.mitre.org/)
* [Circuit-breaker research for AI agents — Zou et al., 2024](https://arxiv.org/abs/2406.04313)
* [Trend Micro analysis of sandbox escapes in AI agents — Park, 2025](https://www.trendmicro.com/vinfo/us/security/news/cybercrime-and-digital-threats/unveiling-ai-agent-vulnerabilities-code-execution)
* [Auth0 guidance on human-in-the-loop authorization for agents — Martinez, 2025](https://auth0.com/blog/secure-human-in-the-loop-interactions-for-ai-agents/)
* [Medium deep-dive on MCP & A2A protocol hijacking — ForAISec, 2025](https://medium.com/%40foraisec/security-analysis-potential-ai-agent-hijacking-via-mcp-and-a2a-protocol-insights-cd1ec5e6045f)
* [Rapid7 fundamentals on spoofing attack prevention — 2024](https://www.rapid7.com/fundamentals/spoofing-attacks/)
* [Imperial College verification of swarm systems — Lomuscio et al.](https://sail.doc.ic.ac.uk/projects/swarms/)
* [NIST AI Risk Management Framework 1.0, 2023](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)
* [WIRED security briefing on encryption best practices, 2024](https://www.wired.com/story/encryption-apps-chinese-telecom-hacking-hydra-russia-exxon)
* [OWASP Top 10 for LLM Applications, 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
* [Comprehensive Vulnerability Analysis is Necessary for Trustworthy LLM-MAS](https://arxiv.org/html/2506.01245v1)
* [How Is LLM Reasoning Distracted by Irrelevant Context?
An Analysis Using a Controlled Benchmark](https://www.arxiv.org/pdf/2505.18761)
* [Large Language Model Sentinel: LLM Agent for Adversarial Purification](https://arxiv.org/pdf/2405.20770)
* [Securing Agentic AI: A Comprehensive Threat Model and Mitigation Framework for Generative AI Agents](https://arxiv.org/html/2504.19956v2)
* [Model Context Protocol Specification](https://modelcontextprotocol.io)
* [Model Context Protocol Tools & Resources Specification](https://modelcontextprotocol.io/specification/2025-06-18/basic)
* [Model Context Protocol Transport Documentation](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports)
* [OWASP GenAI Security Project — “A Practical Guide for Securely Using Third-Party MCP Servers 1.0”](https://genai.owasp.org/resource/cheatsheet-a-practical-guide-for-securely-using-third-party-mcp-servers-1-0/)
* [Cloud Security Alliance – Model Context Protocol Security Working Group](https://modelcontextprotocol-security.io)
* [CSA MCP Security: Top 10 Risks](https://modelcontextprotocol-security.io/top10/)
* [CSA MCP Security: TTPs & Hardening Guidance](https://modelcontextprotocol-security.io/ttps/)
