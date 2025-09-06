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
| **9.2.3** | **検証:** ツールのバイナリまたは記述子はデジタル署名されており、署名はロード前に検証されている。 | 2 | D/V |
| **9.2.4** | **検証:** サンドボックステレメトリは SIEM に配信しており、異常 (送信接続の試行など) はアラートを発している。 | 2 | V |
| **9.2.5** | **検証:** 高リスクのプラグインは、本番デプロイメント前に、セキュリティレビューとペネトレーションテストを受けている。 | 3 | V |
| **9.2.6** | **検証:** サンドボックス脱出の試行は自動的にブロックされており、問題のあるプラグインは調査中隔離されている。 | 3 | D/V |

---

## 9.3 自律ループとコスト制限 (Autonomous Loop & Cost Bounding)

制御されていないエージェント間の再帰とコストの爆発を検出して停止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.3.1** | **検証:** エージェント間呼び出しは、ランタイムが減少と強制する、ホップ制限または TTL を含んでいる。 | 1 | D/V |
| **9.3.2** | **検証:** エージェントは、自己呼び出しや周期的なパターンを見つけるために、一意の呼び出しグラフ ID を維持している。 | 2 | D |
| **9.3.3** | **検証:** 累積計算ユニットと支出カウンタはリクエストチェーンごとに追跡されており、制限を超えるとチェーンを中止している。 | 2 | D/V |
| **9.3.4** | **検証:** 形式解析またはモデル検査はエージェントプロトコルに無制限の再帰がないことを論証している。 | 3 | V |
| **9.3.5** | **検証:** ループ中止イベントはアラートを生成し、継続的な改善メトリクスをフィードしている。 | 3 | D |

---

## 9.4 プロトコルレベルの不正使用防止 (Protocol-Level Misuse Protection)

エージェントと外部システム間の通信チャネルを保護し、ハイジャックや操作を防止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.4.1** | **検証:** すべてのエージェントとツールの間およびエージェント間のメッセージは認証 (相互 TLS や JWT など) されており、エンドツーエンドで暗号化されている。 | 1 | D/V |
| **9.4.2** | **検証:** スキーマは厳密に検証されており、不明なフィールドや不正なメッセージは拒否されている。 | 1 | D |
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

隔離とフォーマルな安全性モデリングを通じて集団行動の危険性を緩和します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.6.1** | **検証:** 異なるセキュリティドメインで動作するエージェントは、分離されたランタイムサンドボックスまたはネットワークセグメントで実行している。 | 1 | D/V |
| **9.6.2** | **検証:** 群集の行動はモデル化されており、デプロイメント前に生存性と安全性についてフォーマルに検証されている。 | 3 | V |
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

Encrypt and integrity-protect all inter-agent messages to thwart eavesdropping and tampering.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.8.1** | **Verify that** mutual authentication and perfect-forward-secret encryption (e.g. TLS 1.3) are mandatory for agent channels. | 1 | D/V |
| **9.8.2** | **Verify that** message integrity and origin are validated before processing; failures raise alerts and drop the message. | 1 | D |
| **9.8.3** | **Verify that** communication metadata (timestamps, sequence numbers) is logged to support forensic reconstruction. | 2 | D/V |
| **9.8.4** | **Verify that** formal verification or model checking confirms that protocol state machines cannot be driven into unsafe states. | 3 | V |

---

## 9.9 意図の検証と制約の適用 (Intent Verification & Constraint Enforcement)

Validate that agent actions align with the user's stated intent and system constraints.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.9.1** | **Verify that** pre-execution constraint solvers check proposed actions against hard-coded safety and policy rules. | 1 | D |
| **9.9.2** | **Verify that** high-impact actions (financial, destructive, privacy-sensitive) require explicit intent confirmation from the initiating user. | 2 | D/V |
| **9.9.3** | **Verify that** post-condition checks validate that completed actions achieved intended effects without side effects; discrepancies trigger rollback. | 2 | V |
| **9.9.4** | **Verify that** formal methods (e.g., model checking, theorem proving) or property-based tests demonstrate that agent plans satisfy all declared constraints. | 3 | V |
| **9.9.5** | **Verify that** intent-mismatch or constraint-violation incidents feed continuous-improvement cycles and threat-intel sharing. | 3 | D |

---

## 9.10 エージェント推論戦略セキュリティ (Agent Reasoning Strategy Security)

Secure selection and execution of different reasoning strategies including ReAct, Chain-of-Thought, and Tree-of-Thoughts approaches.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.10.1** | **Verify that** reasoning strategy selection uses deterministic criteria (input complexity, task type, security context) and identical inputs produce identical strategy selections within the same security context. | 1 | D/V |
| **9.10.2** | **Verify that** each reasoning strategy (ReAct, Chain-of-Thought, Tree-of-Thoughts) has dedicated input validation, output sanitization, and execution time limits specific to its cognitive approach. | 1 | D/V |
| **9.10.3** | **Verify that** reasoning strategy transitions are logged with complete context including input characteristics, selection criteria values, and execution metadata for audit trail reconstruction. | 2 | D/V |
| **9.10.4** | **Verify that** Tree-of-Thoughts reasoning includes branch pruning mechanisms that terminate exploration when policy violations, resource limits, or safety boundaries are detected. | 2 | D/V |
| **9.10.5** | **Verify that** ReAct (Reason-Act-Observe) cycles include validation checkpoints at each phase: reasoning step verification, action authorization, and observation sanitization before proceeding. | 2 | D/V |
| **9.10.6** | **Verify that** reasoning strategy performance metrics (execution time, resource usage, output quality) are monitored with automated alerts when metrics deviate beyond configured thresholds. | 3 | D/V |
| **9.10.7** | **Verify that** hybrid reasoning approaches that combine multiple strategies maintain input validation and output constraints of all constituent strategies without bypassing any security controls. | 3 | D/V |
| **9.10.8** | **Verify that** reasoning strategy security testing includes fuzzing with malformed inputs, adversarial prompts designed to force strategy switching, and boundary condition testing for each cognitive approach. | 3 | D/V |

---

## 9.11 エージェントライフサイクル状態管理とセキュリティ (Agent Lifecycle State Management & Security)

Secure agent initialization, state transitions, and termination with cryptographic audit trails and defined recovery procedures.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.11.1** | **Verify that** agent initialization includes cryptographic identity establishment with hardware-backed credentials and immutable startup audit logs containing agent ID, timestamp, configuration hash, and initialization parameters. | 1 | D/V |
| **9.11.2** | **Verify that** agent state transitions are cryptographically signed, timestamped, and logged with complete context including triggering events, previous state hash, new state hash, and security validations performed. | 2 | D/V |
| **9.11.3** | **Verify that** agent shutdown procedures include secure memory wiping using cryptographic erasure or multi-pass overwriting, credential revocation with certificate authority notification, and generation of tamper-evident termination certificates. | 2 | D/V |
| **9.11.4** | **Verify that** agent recovery mechanisms validate state integrity using cryptographic checksums (SHA-256 minimum) and rollback to known-good states when corruption is detected with automated alerts and manual approval requirements. | 3 | D/V |
| **9.11.5** | **Verify that** agent persistence mechanisms encrypt sensitive state data with per-agent AES-256 keys and implement secure key rotation on configurable schedules (maximum 90 days) with zero-downtime deployment. | 3 | D/V |

---

## 9.12 ツール統合セキュリティフレームワーク (Tool Integration Security Framework)

Security controls for dynamic tool loading, execution, and result validation with defined risk assessment and approval processes.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **9.12.1** | **Verify that** tool descriptors include security metadata specifying required privileges (read/write/execute), risk levels (low/medium/high), resource limits (CPU, memory, network), and validation requirements documented in tool manifests. | 1 | D/V |
| **9.12.2** | **Verify that** tool execution results are validated against expected schemas (JSON Schema, XML Schema) and security policies (output sanitization, data classification) before integration with timeout limits and error handling procedures. | 1 | D/V |
| **9.12.3** | **Verify that** tool interaction logs include detailed security context including privilege usage, data access patterns, execution time, resource consumption, and return codes with structured logging for SIEM integration. | 2 | D/V |
| **9.12.4** | **Verify that** dynamic tool loading mechanisms validate digital signatures using PKI infrastructure and implement secure loading protocols with sandbox isolation and permission verification before execution. | 2 | D/V |
| **9.12.5** | **Verify that** tool security assessments are automatically triggered for new versions with mandatory approval gates including static analysis, dynamic testing, and security team review with documented approval criteria and SLA requirements. | 3 | D/V |

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
