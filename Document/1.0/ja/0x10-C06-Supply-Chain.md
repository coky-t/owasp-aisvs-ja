# C6 モデルのサプライチェーンセキュリティ (Supply Chain Security for Models)

## 管理目標

AI サプライチェーン攻撃は、バックドア、バイアス、実行可能コードを埋め込んで、サードパーティモデル、フレームワーク、データセットを悪用します。これらのコントロールは、モデルのライフサイクル全体を通して、AI 固有のサプライチェーンアーティファクトのトレーサビリティ、審査、監視を確保します。 This chapter focuses on supply chain risks unique to AI: model artifact integrity, backdoor detection in pretrained weights, dataset poisoning, AI-specific bills of materials, and model-publisher trust.

---

## C6.1 モデルアーティファクトの完全性 (Model Artifact Integrity)

Authenticate third-party model origins and check for hidden behavior before fine-tuning or deployment. Download AI artifacts only from approved sources.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.1.1** | **検証:** モデルはインポート前に悪意のあるコードがないかスキャンされている。 | 1 |
| **6.1.2** | **Verify that** model weights, datasets, and fine-tuning adapters are downloaded only from approved sources. | 1 |
| **6.1.3** | **検証:** すべてのサードパーティモデルアーティファクトは完全性検証されている。 | 2 |
| **6.1.4** | **検証:** モデルは、非開発環境に昇格される前に、動作受入テストスイートに合格している。 | 2 |

---

## C6.2 AI BOM とサプライチェーンの監視 (AI BOM & Supply Chain Monitoring)

Generate and sign detailed AI-specific bills of materials and ensure readiness to respond to supply chain compromise events.

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------------------------------- | :---: |
| **6.2.1** | **検証:** すべてのモデルアーティファクトは、データセット、重み、ライセンス、データオリジンの記述をリストした、バージョン管理されて機械読み取り可能な AI BOM を発行している。 | 1 |
| **6.2.2** | **検証:** AI BOM はデプロイメント前に暗号署名されている。 | 2 |
| **6.2.3** | **検証:** AI BOM 完全性チェックは、コンポーネントメタデータが欠落している場合、失敗している。 | 2 |

---

## 参考情報

* [OWASP LLM03:2025 Supply Chain](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)
* [MITRE ATLAS: Supply Chain Compromise](https://atlas.mitre.org/techniques/AML.T0010)
* [SBOM Overview: CISA](https://www.cisa.gov/sbom)
* [CycloneDX: Machine Learning Bill of Materials](https://cyclonedx.org/capabilities/mlbom/)
* [OWASP AIBOM](https://genai.owasp.org/owasp-aibom/)
