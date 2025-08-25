# C6 モデル、フレームワーク、データのサプライチェーンセキュリティ (Supply Chain Security for Models, Frameworks & Data)

## 管理目標

AI サプライチェーン攻撃は、バックドア、バイアス、実行可能コードを埋め込んで、サードパーティモデル、フレームワーク、データセットを悪用します。これらのコントロールは、エンドツーエンドの来歴、脆弱性管理、監視を提供し、モデルのライフサイクル全体を保護します。

---

## C6.1 事前学習済みモデルの審査と来歴 (Pretrained Model Vetting & Provenance)

ファインチューニングやデプロイメントの前に、サードパーティのモデルのオリジン、ライセンス、隠し動作を評価および認証します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.1.1** | **検証:** すべてのサードパーティモデルアーティファクトは、ソースリポジトリとコミットハッシュを識別する、署名付き来歴レコードを含んでいる。 | 1 | D/V |
| **6.1.2** | **検証:** モデルは、インポート前に自動ツールを使用して、悪意のあるレイヤやトロイの木馬トリガーがないかスキャンされている。 | 1 | D/V |
| **6.1.3** | **検証:** 転移学習は、隠し動作を検出するために、敵対的評価をファインチューンしている。 | 2 | D |
| **6.1.4** | **検証:** モデルライセンス、輸出規制タグ、データオリジンステートメントは ML-BOM エントリに記録されている。 | 2 | V |
| **6.1.5** | **検証:** 高リスクモデル (公開アップロードされた重み、未検証の作成者) は、人間によるレビューと承認が行われるまで、隔離されたままにしている。 | 3 | D/V |

---

## C6.2 フレームワークとライブラリのスキャン (Framework & Library Scanning)

ランタイムスタックを安全に保つために、CVE や悪意のあるコードについて ML フレームワークとライブラリを継続的にスキャンします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.2.1** | **検証:** CI パイプラインは AI フレームワークと重要なライブラリに対して依存関係スキャナを実行している。 | 1 | D/V |
| **6.2.2** | **検証:** 重大な脆弱性 (CVSS ≥ 7.0) は本番イメージへの昇格をブロックしている。 | 1 | D/V |
| **6.2.3** | **検証:** 静的コード解析はフォークまたはベンダー化された ML ライブラリに対して実行している。 | 2 | D |
| **6.2.4** | **検証:** フレームワークのアップグレード提案は、パブリック CVE フィードを参照する、セキュリティ影響評価を含んでいる。 | 2 | V |
| **6.2.5** | **検証:** ランタイムセンサーは、署名付き SBOM から逸脱する、予期しない動的ライブラリのロードについてアラートしている。 | 3 | V |

---

## C6.3 依存関係の固定と検証 (Dependency Pinning & Verification)

すべての依存関係を不変のダイジェストに固定し、ビルドを再現し、同一かつ改竄のないアーティファクトを保証します。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.3.1** | **検証:** すべてのパッケージマネージャはロックファイルを介してバージョン固定を強制している。 | 1 | D/V |
| **6.3.2** | **検証:** 不変ダイジェストは、コンテナ参照の変更可能タグの代わりに、使用されている。 | 1 | D/V |
| **6.3.3** | **検証:** 再現可能なビルドチェックは、同一な出力を確保するために、CI 実行間でハッシュを比較している。 | 2 | D |
| **6.3.4** | **検証:** ビルドアテステーションは監査の追跡可能性のために 18 か月間保存されている。 | 2 | V |
| **6.3.5** | **検証:** 期限切れの依存関係は、固定されたバージョンを更新またはフォークするために、自動 PR をトリガーしている。 | 3 | D |

---

## C6.4 信頼できるソースの強制 (Trusted Source Enforcement)

暗号論的に検証され、組織が承認したソースからのアーティファクトのダウンロードのみを許可し、それ以外のものはブロックします。

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.4.1** | **検証:** モデルの重み、データセット、コンテナは、承認されたドメインまたは内部レジストリからのみダウンロードされている。 | 1 | D/V |
| **6.4.2** | **検証:** Sigstore/Cosign 署名は、アーティファクトがローカルにキャッシュされる前に、発行者のアイデンティティを検証している。 | 1 | D/V |
| **6.4.3** | **検証:** 送出 (egress) プロキシは、信頼できるソースポリシーを適用して、認証されていないアーティファクトダウンロードをブロックしている。 | 2 | D |
| **6.4.4** | **検証:** リポジトリの許可リストは、各エントリのビジネス上の正当性の証跡とともに、四半期ごとにレビューされている。 | 2 | V |
| **6.4.5** | **検証:** ポリシー違反はアーティファクトの隔離と、依存するパイプライン実行のロールバックをトリガーしている。 | 3 | V |

---

## C6.5 サードパーティデータセットのリスク評価 (Third‑Party Dataset Risk Assessment)

Evaluate external datasets for poisoning, bias, and legal compliance, and monitor them throughout their lifecycle.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.5.1** | **Verify that** external datasets undergo poisoning risk scoring (e.g., data fingerprinting, outlier detection). | 1 | D/V |
| **6.5.2** | **Verify that** bias metrics (demographic parity, equal opportunity) are calculated before dataset approval. | 1 | D |
| **6.5.3** | **Verify that** provenance and license terms for datasets are captured in ML‑BOM entries. | 2 | V |
| **6.5.4** | **Verify that** periodic monitoring detects drift or corruption in hosted datasets. | 2 | V |
| **6.5.5** | **Verify that** disallowed content (copyright, PII) is removed via automated scrubbing prior to training. | 3 | D |

---

## C6.6 サプライチェーン攻撃の監視 (Supply Chain Attack Monitoring)

Detect supply‑chain threats early through CVE feeds, audit‑log analytics, and red‑team simulations.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.6.1** | **Verify that** CI/CD audit logs stream to SIEM detections for anomalous package pulls or tampered build steps. | 1 | V |
| **6.6.2** | **Verify that** incident response playbooks include rollback procedures for compromised models or libraries. | 2 | D ||
| **6.6.3** | **Verify that** threat‑intel enrichment tags ML‑specific indicators (e.g., model‑poisoning IoCs) in alert triage. | 3 | V |

---

## C6.7 モデルアーティファクトのための ML-BOM (ML‑BOM for Model Artifacts)

Generate and sign detailed ML‑specific SBOMs (ML‑BOMs) so downstream consumers can verify component integrity at deploy time.

| # | 説明 | レベル | ロール |
|:--------:|---------------------------------------------------------------------------------------------------------------------|:---:|:---:|
| **6.7.1** | **Verify that** every model artifact publishes a ML‑BOM that lists datasets, weights, hyperparameters, and licenses. | 1 | D/V |
| **6.7.2** | **Verify that** ML‑BOM generation and Cosign signing are automated in CI and required for merge. | 1 | D/V |
| **6.7.3** | **Verify that** ML‑BOM completeness checks fail the build if any component metadata (hash, license) is missing. | 2 | D |
| **6.7.4** | **Verify that** downstream consumers can query ML-BOMs via API to validate imported models at deploy time. | 2 | V |
| **6.7.5** | **Verify that** ML‑BOMs are version‑controlled and diffed to detect unauthorized modifications. | 3 | V |

---

## 参考情報

* [ML Supply Chain Compromise – MITRE ATLAS](https://misp-galaxy.org/mitre-atlas-attack-pattern/)
* [Supply‑chain Levels for Software Artifacts (SLSA)](https://slsa.dev/)
* [CycloneDX – Machine Learning Bill of Materials](https://cyclonedx.org/capabilities/mlbom/)
* [What is Data Poisoning? – SentinelOne](https://www.sentinelone.com/cybersecurity-101/cybersecurity/data-poisoning/)
* [Transfer Learning Attack – OWASP ML Security Top 10](https://owasp.org/www-project-machine-learning-security-top-10/docs/ML07_2023-Transfer_Learning_Attack)
* [AI Data Security Best Practices – CISA](https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-142a)
* [Secure CI/CD Supply Chain – Sumo Logic](https://www.sumologic.com/blog/secure-azure-devops-github-supply-chain-attacks)
* [AI & Transparency: Protect ML Models – ReversingLabs](https://www.reversinglabs.com/blog/ai-and-transparency-how-ml-model-creators-can-protect-against-supply-chain-attacks)
* [SBOM Overview – CISA](https://www.cisa.gov/sbom)
* [Training Data Poisoning Guide – Lakera.ai](https://www.lakera.ai/blog/training-data-poisoning)
* [Dependency Pinning for Reproducible Python – Medium](https://medium.com/data-science-collective/guarantee-a-locked-reproducible-environment-with-every-python-run-c0e2bf19fb53)
