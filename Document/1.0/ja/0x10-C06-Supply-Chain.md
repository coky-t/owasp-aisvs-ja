# C6 モデル、フレームワーク、データのサプライチェーンセキュリティ (Supply Chain Security for Models, Frameworks & Data)

## 管理目標

AI サプライチェーン攻撃は、バックドア、バイアス、実行可能コードを埋め込んで、サードパーティモデル、フレームワーク、データセットを悪用します。これらのコントロールは、モデルのライフサイクル全体を通して、AI 固有のサプライチェーンアーティファクトのトレーサビリティ、審査、監視を確保します。

Generic software supply chain controls (dependency scanning, version pinning, lockfile enforcement, container digest pinning, build attestation, reproducible builds, SBOM generation, CI/CD audit logging, etc.) are covered by ASVS v5 (V13, V15), OWASP SCVS, SLSA, and CIS Controls, and are not repeated here. This chapter focuses on supply chain risks unique to AI: model artifact integrity, backdoor detection in pretrained weights, dataset poisoning, AI-specific bills of materials, and model-publisher trust.

---

## C6.1 事前学習済みモデルの審査とオリジンの完全性 (Pretrained Model Vetting & Origin Integrity)

ファインチューニングやデプロイメントの前に、サードパーティのモデルのオリジンと隠し動作を評価および認証します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.1.1** | **検証:** すべてのサードパーティモデルアーティファクトは、そのソース、バージョン、完全性チェックサムを識別する、署名付きオリジンおよび完全性レコードを含んでいる。 | 1 |
| **6.1.2** | **検証:** モデルは、インポート前に自動ツールを使用して、悪意のあるレイヤやトロイの木馬トリガーがないかスキャンされている。 | 1 |
| **6.1.3** | **検証:** 高リスクモデル (公開アップロードされた重み、未検証の作成者など) は、人間によるレビューと承認が行われるまで、隔離されたままにしている。 | 2 |
| **6.1.4** | **検証:** サードパーティまたはオープンソースのモデルは、非開発環境にインポートまたは昇格される前に、定義された動作受入テストスイート (デプロイメントコンテキストに関連する安全性、整合性、機能境界をカバーする) に合格している。 | 2 |
| **6.1.5** | **検証:** 転移学習は、隠し動作を検出するために、敵対的評価をファインチューンしている。 | 3 |

---

## C6.2 AI アーティファクトのための信頼できるソースの強制 (Trusted Source Enforcement for AI Artifacts)

組織が承認したソースからの AI アーティファクトのダウンロードのみを許可し、モデルパブリッシャのアイデンティティを検証します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.2.1** | **検証:** モデルの重み、データセット、ファインチューニングアダプタは、承認されたソースまたは内部レジストリからのみダウンロードされている。 | 1 |
| **6.2.2** | **Verify that** cryptographic signing keys used to authenticate model publishers are pinned per source registry, and that key rotation events require explicit re-approval before updated keys are trusted. | 3 |

---

## C6.3 サードパーティデータセットのリスク評価 (Third-Party Dataset Risk Assessment)

外部データセットのポイズニングと法令遵守を評価し、ライフサイクル全体にわたって監視します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.3.1** | **検証:** 許可されていないコンテンツ (著作権で保護されたマテリアル、PII など) はトレーニング前に自動スクラビングで検出され、削除されている。 | 1 |
| **6.3.2** | **検証:** 外部データセットはポイズニングリスクスコアリング (データフィンガープリンティング、外れ値検出) を受けている。 | 2 |
| **6.3.3** | **検証:** データセットのオリジン、リネージ、ライセンス条項は AI BOM エントリに記録されている。 | 2 |
| **6.3.4** | **検証:** 定期的な監視はホストされているデータセットのドリフトや破損を検出している。 | 3 |

---

## C6.4 サプライチェーン攻撃の監視 (Supply Chain Attack Monitoring)

脅威インテリジェンスの強化とインシデント対応の準備を通じて、AI 固有のサプライチェーンの脅威を検出します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.4.1** | **Verify that** incident response playbooks include procedures specific to compromised AI artifacts, such as rollback of poisoned models, revocation of model signatures, and re-evaluation of downstream systems that consumed affected artifacts. | 2 |
| **6.4.2** | **検証:** 脅威インテリジェンスのエンリッチメントは、アラートトリアージで、AI 固有のインジケータ (モデルポイズニングの侵害インジケータなど) をタグ付けしている。 | 3 |

---

## C6.5 モデルアーティファクトのための AI BOM (AI BOM for Model Artifacts)

詳細な AI 固有の部品表 (AI BOM) を生成して署名することで、ダウンストリームのコンシューマがデプロイ時にコンポーネントの完全性を検証できます。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.5.1** | **検証:** すべてのモデルアーティファクトは、データセット、重み、ハイパーパラメータ、ライセンス、輸出管理タグ、データオリジンの記述をリストした、バージョン管理された AI BOM を発行している。 | 1 |
| **6.5.2** | **検証:** AI BOM はデプロイメント前に暗号署名されている。 | 2 |
| **6.5.3** | **検証:** AI BOM 完全性チェックは、コンポーネントメタデータ (ハッシュおよびライセンス) が欠落している場合、失敗している。 | 2 |
| **6.5.4** | **検証:** ダウンストリームのコンシューマは、デプロイメント時にインポートされたモデルを検証するために、API を介して AI BOM をクエリできる。 | 2 |

---

## 参考情報

* [OWASP LLM03:2025 Supply Chain](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)
* [MITRE ATLAS: Supply Chain Compromise](https://atlas.mitre.org/techniques/AML.T0010)
* [SBOM Overview: CISA](https://www.cisa.gov/sbom)
* [CycloneDX: Machine Learning Bill of Materials](https://cyclonedx.org/capabilities/mlbom/)
* [OWASP AIBOM](https://genai.owasp.org/owasp-aibom/)
