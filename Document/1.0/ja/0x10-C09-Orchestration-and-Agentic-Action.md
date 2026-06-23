# C9 オーケストレーションとエージェントセキュリティ (Orchestration & Agentic Security)

## 管理目標

この章は、自律型およびマルチエージェント型のシステムは認可され、意図され、かつ制限されたアクションのみを実行することの確保を取り扱います。

---

## C9.1 実行予算、ループ制御、サーキットブレーカー (Execution Budgets, Loop Control, and Circuit Breakers)

実行時の拡張 (再帰、同時実行、コスト) を制限する必要があり、暴走動作を安全に停止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.1.1** | **Verify that** per-tool quotas and timeouts (e.g., CPU, memory, disk, egress, and execution time) are enforced. | 1 |
| **9.1.2** | **検証:** 実行ごとの予算 (最大再帰深度、トークン使用、金銭支出) はランタイムによって構成および適用されている。 | 1 |
| **9.1.3** | **Verify that** a swarm-level kill-switch exists that can halt all active agent instances. | 2 |

---

## C9.2 影響度の高いアクションの承認と不可逆性の制御 (High-Impact Action Approval and Irreversibility Controls)

Privileged, high-impact, or hard-to-reverse agent actions must require trusted approval checkpoints.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.2.1** | **Verify that** the agent runtime blocks execution of privileged, high-impact, or irreversible actions until explicit human approval is received and verified. | 1 |
| **9.2.2** | **Verify that** approval requests display canonicalized and complete action parameters, such as diffs, commands, recipients, amounts, resources, and scopes, without truncation or unsafe transformation. | 2 |
| **9.2.3** | **Verify that** each high-impact action has a trusted reversibility classification, such as read-only, reversible, externally reversible, or irreversible. | 2 |
| **9.2.4** | **Verify that** the agent runtime enforces reversibility classifications by blocking, requiring approval, or restricting actions based on their impact and ability to be reversed. | 2 |
| **9.2.5** | **Verify that** any self-modification capability (e.g., prompt rewriting, tool-list changes, parameter updates) is restricted by enforceable boundaries. | 2 |
| **9.2.6** | **Verify that** agentic systems include an AI-augmented review of planned high-risk actions before execution that adds to, and does not replace, the deterministic policy gate. | 2 |
| **9.2.7** | **Verify that** the AI-augmented review mechanism is protected against manipulation by adversarial inputs, and cannot be overridden or bypassed through prompt injection. | 2 |
| **9.2.8** | **Verify that** approvals are cryptographically bound to action parameters, requester identity, execution context, and a unique single-use nonce. | 3 |
| **9.2.9** | **Verify that** cryptographic key material or credentials used to issue approvals are isolated from the agent runtime. | 3 |
| **9.2.10** | **Verify that** approval gates for multi-step or multi-agent action chains enforce the highest-impact reversibility classification present anywhere in the chain. | 3 |

---

## C9.3 コンポーネントの分離とツールの認可 (Component Isolation and Tool Authorization)

ツールとプラグインの実行、ロード、出力を制限する必要があり、不正なシステムアクセスや安全でない副作用を防止します。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.3.1** | **検証:** 各ツール/プラグインは最小権限のサンドボックスで実行しているか、モデル操作から分離されている。 | 1 |
| **9.3.2** | **検証:** ツール出力はスキーマに対して検証されている。 | 1 |
| **9.3.3** | **検証:** ツールマニフェストは、必要な権限、リソース制限、出力バリデーション要件を宣言している。 | 2 |
| **9.3.4** | **Verify that** the runtime enforces the privileges, resource limits, and output-validation requirements declared in tool manifests. | 2 |
| **9.3.5** | **Verify that** components processing untrusted data are isolated from tool-calling capabilities, ensuring that compromised data processing cannot trigger unauthorized tool invocations. | 2 |
| **9.3.6** | **Verify that** there is architectural separation between processing of untrusted tool outputs and agent operations. | 2 |
| **9.3.7** | **Verify that** external resources named in model output are verified against an approved allow-list or registry before the agent installs or invokes them. | 2 |
| **9.3.8** | **検証:** ポリシー違反は自動ツール封じ込めをトリガーしている。 | 3 |

---

## C9.4 エージェントとオーケストレータのアイデンティティ (Agent and Orchestrator Identity)

すべてのアクションを帰属可能にし、すべての変異を検出可能にする必要があります。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.4.1** | **検証:** 各エージェントインスタンスは一意の暗号アイデンティティを持ち、ダウンストリームシステムへのファーストクラスのプリンシパルとして認証している。 | 2 |
| **9.4.2** | **検証:** エージェントが開始したアクションは、否認防止のために、実行チェーンの各ステップに暗号的にバインドされている。 | 2 |
| **9.4.3** | **検証:** エージェントのアイデンティティクレデンシャルは定義されたスケジュールで入れ替えている。 | 3 |
| **9.4.4** | **Verify that** agent state persisted between invocations is integrity-protected. | 3 |

---

## C9.5 エージェントの認可、委譲、継続的施行 (Agent Authorization, Delegation, and Continuous Enforcement)

すべてのアクションは実行時に認可され、スコープによって制約されている必要があります。

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.5.1** | **検証:** エージェントのアクションは、ランタイムによって強制される、きめ細かいポリシーに基づいて認可されている。エージェントが呼び出せるツール、エージェントが提供できるパラメータ値を制限している。 | 2 |
| **9.5.2** | **Verify that** when an agent acts on a user's behalf, the runtime propagates an integrity-protected, scope-limited token that carries the user's authorization context and is enforced at every downstream call. | 2 |
| **9.5.3** | **検証:** すべてのアクセス制御の決定は、AI モデル自体ではなく、アプリケーションロジックまたはポリシーエンジンによって強制されている。 | 2 |
| **9.5.4** | **Verify that** secrets and credentials required by an agent at runtime are not exposed within the model's observable context, including the context window, system prompts, or tool call parameters. | 2 |
| **9.5.5** | **Verify that** inter-agent task delegation is restricted by an explicit authorization policy. | 2 |
| **9.5.6** | **Verify that** long-running agent sessions re-evaluate current backend authorization policy on every privileged action. | 3 |

---

## C9.6 Shutdown and Graceful Degradation

Shutdown and graceful degradation paths must remain under human control, with mechanisms that stay reliable and are exercised over time.

| # | 説明 | レベル |
| :--: | --- | :---: |
| **9.6.1** | **検証:** 手動キルスイッチメカニズムは、AI モデルの推論と出力を即座に停止するために、存在している。 | 1 |
| **9.6.2** | **Verify that** when a human-approval gate is not satisfied within the defined approval time, the system blocks the pending action. | 2 |
| **9.6.3** | **Verify that** kill-switch commands are implemented through an out-of-band channel that is isolated from the agent runtime. | 3 |

---

## 参考情報

* [OWASP Agentic AI Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
* [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026)
* [OWASP LLM06:2025 Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)
* [NIST AI 100-1: AI Risk Management Framework (AI RMF 1.0)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
* [Regulation (EU) 2024/1689 (EU AI Act), Article 14: Human Oversight](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)
