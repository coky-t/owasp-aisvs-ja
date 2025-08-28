# C7 モデル動作、出力制御、安全保証 (Model Behavior, Output Control & Safety Assurance)

## 管理目標

モデル出力は **構造化されており、信頼性があり、安全であり、説明可能であり、本番環境で継続的に監視されている** 必要があります。そうすることで、ハルシネーション、プライバシー漏洩、有害コンテンツ、暴走行為を軽減し、ユーザーの信頼と規制遵守を向上します。

---

## C7.1 出力形式の強制 (Output Format Enforcement)

厳格なスキーマ、制約のあるデコード、ダウンストリームバリデーションは、不正なコンテンツや悪意のあるコンテンツが拡散する前にそれらを阻止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.1.1** | **検証:** レスポンススキーマ (JSON スキーマなど) はシステムプロンプトで提供されており、すべての出力は自動的に検証されている。適合しない出力は修復または拒否をトリガーしている。 | 1 | D/V |
| **7.1.2** | **検証:** 制約付きデコーディング (ストップトークン、正規表現、最大トークン) は、オーバーフローやプロンプトインジェクションサイドチャネルを防ぐために、有効にされている。 | 1 | D/V |
| **7.1.3** | **検証:** ダウンストリームコンポーネントは信頼できないものとして扱い、スキーマまたはインジェクションセーフなデシリアライザに対して検証している。 | 2 | D/V |
| **7.1.4** | **検証:** 不適切な出力イベントはログ記録され、レート制限され、監視対象として表示されている。 | 3 | V |

---

## C7.2 ハルシネーションの検出と緩和 (Hallucination Detection & Mitigation)

不確実性の推定とフォールバック戦略は捏造された回答を抑制します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.2.1** | **検証:** トークンレベルの対数確率、アンサンブル自己一貫性、ファインチューンされたハルシネーション検出器は各回答の信頼スコアを割り当てている。 | 1 | D/V |
| **7.2.2** | **検証:** 設定可能な信頼度閾値を下回るレスポンスはフォールバックワークフロー (検索拡張生成、二次モデル、人間によるレビューなど) をトリガーしている。 | 1 | D/V |
| **7.2.3** | **検証:** ハルシネーションインシデントは根本原因メタデータでタグ付けされており、ポストモーテムとファインチューニングパイプラインに供給されている。 | 2 | D/V |
| **7.2.4** | **検証:** 閾値と検出器は、主要なモデルや知識ベースの更新後に、再調整されている。 | 3 | D/V |
| **7.2.5** | **検証:** ダッシュボードの視覚化はハルシネーション率を追跡している。 | 3 | V |

---

## C7.3 出力の安全性とプライバシーフィルタリング (Output Safety & Privacy Filtering)

ポリシーフィルタとレッドチームカバレッジはユーザーと機密データを保護します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.3.1** | **検証:** 生成前および生成後の分類子は、ポリシーに準拠して、ヘイト、ハラスメント、自傷行為、過激主義、性的に露骨なコンテンツをブロックしている。 | 1 | D/V |
| **7.3.2** | **検証:** PII/PCI 検出と自動修正はすべてのレスポンスに対して実行している。違反はプライバシーインシデントを提起している。 | 1 | D/V |
| **7.3.3** | **検証:** 機密タグ (営業秘密など) は、テキスト、画像、コードの漏洩を防ぐために、さまざまな様式に伝播している。 | 2 | D |
| **7.3.4** | **検証:** フィルタのバイパス試行や高リスク分類は二次承認またはユーザー再認証を必要としている。 | 3 | D/V |
| **7.3.5** | **検証:** フィルタリング閾値は規制管理区域とユーザーの年齢/ロールのコンテキストを反映している。 | 3 | D/V |

---

## C7.4 出力と動作の制限 (Output & Action Limiting)

レート制限と承認ゲートは乱用や過剰な自律性を防止します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **7.4.1** | **検証:** ユーザーごとおよび API キーごとのクォータは、リクエスト、トークン、コストを制限しており、429 エラーでは指数バックオフを適用している。 | 1 | D |
| **7.4.2** | **検証:** 特権アクション (ファイル書き込み、コード実行、ネットワーク呼び出し) はポリシーベースの承認またはヒューマンインザループを必要としている。 | 1 | D/V |
| **7.4.3** | **検証:** クロスモーダル一貫性チェックは、同じリクエストに対して生成される画像、コード、テキストが悪意のあるコンテンツを密かに持ち込むために使用できないようにしている。 | 2 | D/V |
| **7.4.4** | **検証:** エージェント委譲の深さ、再帰制限、許可されるツールのリストは明示的に構成されている。 | 2 | D |
| **7.4.5** | **検証:** 制限違反は SIEM 取り込みのために構造化されたセキュリティイベントを発している。 | 3 | V |

---

## C7.5 出力の説明可能性 (Output Explainability)

Transparent signals improve user trust and internal debugging.

| # | 説明 | レベル | ロール |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------ | :---: | :--: |
| **7.5.1** | **Verify that** user-facing confidence scores or brief reasoning summaries are exposed when risk assessment deems appropriate. | 2 | D/V |
| **7.5.2** | **Verify that** generated explanations avoid revealing sensitive system prompts or proprietary data. | 2 | D/V |
| **7.5.3** | **Verify that** the system captures token-level log-probabilities or attention maps and stores them for authorized inspection. | 3 | D |
| **7.5.4** | **Verify that** explainability artefacts are version-controlled alongside model releases for auditability. | 3 | V |

---

## C7.6 監視統合 (Monitoring Integration)

Real-time observability closes the loop between development and production.

| # | 説明 | レベル | ロール |
| :-------: | -------------------------------------------------------------------------------------------------------------------------------------------- | :---: | :--: |
| **7.6.1** | **Verify that** metrics (schema violations, hallucination rate, toxicity, PII leaks, latency, cost) stream to a central monitoring platform. | 1 | D |
| **7.6.2** | **Verify that** alert thresholds are defined for each safety metric, with on-call escalation paths. | 1 | V |
| **7.6.3** | **Verify that** dashboards correlate output anomalies with model/version, feature flag, and upstream data changes. | 2 | V |
| **7.6.4** | **Verify that** monitoring data feeds back into retraining, fine-tuning, or rule updates within a documented MLOps workflow. | 2 | D/V |
| **7.6.5** | **Verify that** monitoring pipelines are penetration-tested and access-controlled to avoid leakage of sensitive logs. | 3 | V |

---

## 7.7 生成メディアの安全対策 (Generative Media Safeguards)

Ensure that AI systems do not generate illegal, harmful, or unauthorized media content by enforcing policy constraints, output validation, and traceability.

| # | 説明 | レベル | ロール |
| :-------: | -------------------------------------------------------------------------------------------------------------------------------------------- | :---: | :--: |
| **7.7.1** | **Verify that** system prompts and user instructions explicitly prohibit the generation of illegal, harmful, or non-consensual deepfake media (e.g., image, video, audio).        | 1     | D/V  |
| **7.7.2** | **Verify that** prompts are filtered for attempts to generate impersonations, sexually explicit deepfakes, or media depicting real individuals without consent.                   | 2     | D/V  |
| **7.7.3** | **Verify that** the system uses perceptual hashing, watermark detection, or fingerprinting to prevent unauthorized reproduction of copyrighted media.                             | 2     | V    |
| **7.7.4** | **Verify that** all generated media is cryptographically signed, watermarked, or embedded with tamper-resistant provenance metadata for downstream traceability.                  | 3     | D/V  |
| **7.7.5** | **Verify that** bypass attempts (e.g., prompt obfuscation, slang, adversarial phrasing) are detected, logged, and rate-limited; repeated abuse is surfaced to monitoring systems. | 3     | V    |

## 参考情報

* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [ISO/IEC 42001:2023 – AI Management System](https://www.iso.org/obp/ui/en/)
* [OWASP Top-10 for Large Language Model Applications (2025)](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
* [Practical Techniques to Constrain LLM Output](https://mychen76.medium.com/practical-techniques-to-constraint-llm-output-in-json-format-e3e72396c670)
* [Dataiku – Structured Text Generation Guide](https://blog.dataiku.com/your-guide-to-structured-text-generation)
* [VL-Uncertainty: Detecting Hallucinations](https://arxiv.org/abs/2411.11919)
* [HaDeMiF: Hallucination Detection & Mitigation](https://openreview.net/forum?id=VwOYxPScxB)
* [Building Confidence in LLM Outputs](https://www.alkymi.io/data-science-room/building-confidence-in-llm-outputs)
* [Explainable AI & LLMs](https://duncsand.medium.com/explainable-ai-140912d31b3b)
* [Sensitive Information Disclosure in LLMs](https://virtualcyberlabs.com/llm-sensitive-information-disclosure/)
* [OpenAI Rate-Limit & Exponential Back-off](https://hackernoon.com/openais-rate-limit-a-guide-to-exponential-backoff-for-llm-evaluation)
* [Arize AI – LLM Observability Platform](https://arize.com/)
