<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="images/aisvs-logo-dark.png">
    <source media="(prefers-color-scheme: light)" srcset="images/aisvs-logo-light.png">
    <img alt="OWASP AISVS Logo" src="images/aisvs-logo-light.png" width="520">
  </picture>
</p>

# OWASP Artificial Intelligence Security Verification Standard (AISVS)

[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

この著作物は [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa] の下でライセンスされています。


[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: https://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg

## AISVS とは？

**Artificial Intelligence Security Verification Standard (AISVS)** は AI 対応システム向けのテスト可能なセキュリティ要件のコミュニティ主導のカタログです。開発者、アーキテクト、セキュリティエンジニア、監査担当者に対し、データ収集やモデルトレーニングから、デプロイメント、モニタリング、廃止に至るまで、AI アプリケーションのライフサイクル全体を通じて、セキュリティの設計、構築、テスト、検証のための構造化されたフレームワークを提供します。

AISVS は [OWASP Application Security Verification Standard (ASVS)](https://owasp.org/www-project-application-security-verification-standard/) をモデルとしており、すべての要件が **検証可能、テスト可能、実装可能** であるべきという同じ理念に基づいています。

## プロジェクトリーダー

このプロジェクトは [Jim Manico](https://linkedin.com/in/jmanico) によって設立されました。現在のプロジェクトリーダーは [Jim Manico](https://linkedin.com/in/jmanico), [Otto Sulin](https://github.com/ottosulin), [Rico Komenda](https://github.com/RicoKomenda), [Russ Memisyazici](https://linkedin.com/in/vtxmm), [Raza Sharif](mailto:raza@cybersecai.co.uk) です。

---

### AISVS ではないもの

* **ガバナンスフレームワークではありません。** ガバナンスは [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework), [ISO/IEC 42001](https://www.iso.org/standard/42001), EU AI Act 準拠ガイドで十分に網羅されています。
* **リスク管理フレームワークではありません。** AISVS は、リスクフレームワークが指す技術的制御を提供しますが、リスク評価方法論を定義してはいません。
* **ツール推奨リストではありません。** AISVS はベンダーニュートラルであり、特定の製品やフレームワークを推奨してはいません。

### AISVS は他の標準をどのように補完するか

| 標準 | 焦点 | AISVS との関係 |
| --- | --- | --- |
| OWASP ASVS | ウェブアプリケーションセキュリティ | AISVS は ASVS の概念を AI 固有の脅威に拡張します |
| [OWASP Top 10 for LLMs](https://owasp.org/www-project-top-10-for-large-language-model-applications/) | 主要な LLM リスクの認識 | AISVS はこれらのリスクを緩和するための詳細なコントロールを提供します |
| [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) | エージェント型 AI リスクの認識 | AISVS はエージェント固有の脅威に対処するための詳細なコントロールを提供します |
| NIST AI RMF | AI リスクガバナンス | AISVS は AI RMF が参照するテスト可能な技術的コントロールを提供します |
| ISO/IEC 42001 | AI マネジメントシステム | AISVS は実装レベルのセキュリティ検証で補完します |

---

## 最新安定版

最新安定版は **AISVS 1.0** であり、以下にあります。

| 形式 | リンク |
| --- | --- |
| PDF | [AISVS 1.0 PDF (英語)](https://github.com/OWASP/AISVS/raw/main/1.0/dist/AISVS-1.0.pdf) |
| マークダウン (ソース) | [オンラインで閲覧 (英語)](https://github.com/OWASP/AISVS/tree/main/1.0/en) |

---

## Verification Levels

Each AISVS requirement is assigned a verification level (1, 2, or 3) indicating the depth of security assurance:

| Level | Description | When to use |
| :---: | --- | --- |
| **1** | Essential baseline controls that every AI system should implement. | All AI applications, including internal tools and low-risk systems. |
| **2** | Standard controls for systems handling sensitive data or making consequential decisions. | Production systems, customer-facing AI, systems processing personal data. |
| **3** | Advanced controls for high-assurance environments requiring defense against sophisticated attacks. | Critical infrastructure, safety-critical AI, high-value targets, regulated industries. |

Organizations should select a target level based on the risk profile of their AI system. Most production systems should aim for at least Level 2.

## How to use AISVS

* **During design.** Use requirements as a security checklist when architecting AI systems.
* **During development.** Integrate requirements into CI/CD pipelines, code reviews, and testing.
* **During security assessments.** Use as a verification framework for penetration testing and audits.
* **For procurement.** Reference specific requirements when evaluating AI vendors and third-party models.

## Requirement Chapters

1. [トレーニングデータ完全性とトレーサビリティ (Training Data Integrity & Traceability)](1.0/ja/0x10-C01-Training-Data-Integrity-and-Traceability.md)
2. [入力バリデーション (Input Validation)](1.0/ja/0x10-C02-Input-Validation.md)
3. [モデルライフサイクル管理と変更管理 (Model Lifecycle Management & Change Control)](1.0/ja/0x10-C03-Model-Lifecycle-Management.md)
4. [インフラストラクチャ、構成、デプロイメントセキュリティ (Infrastructure, Configuration & Deployment Security)](1.0/ja/0x10-C04-Infrastructure.md)
5. [AI コンポーネントとユーザーのアクセス制御とアイデンティティ (Access Control & Identity for AI Components & Users)](1.0/ja/0x10-C05-Access-Control-and-Identity.md)
6. [モデルのサプライチェーンセキュリティ (Supply Chain Security for Models)](1.0/ja/0x10-C06-Supply-Chain.md)
7. [モデル動作、出力制御、安全保証 (Model Behavior, Output Control & Safety Assurance)](1.0/ja/0x10-C07-Model-Behavior.md)
8. [メモリ、エンベディング、ベクトルデータベースセキュリティ (Memory, Embeddings & Vector Database Security)](1.0/ja/0x10-C08-Memory-Embeddings-and-Vector-Database.md)
9. [オーケストレーションとエージェントセキュリティ (Orchestration & Agentic Security)](1.0/ja/0x10-C09-Orchestration-and-Agentic-Action.md)
10. [モデルコンテキストプロトコル (MCP) セキュリティ (Model Context Protocol (MCP) Security)](1.0/ja/0x10-C10-MCP-Security.md)
11. [敵対的堅牢性 (Adversarial Robustness)](1.0/ja/0x10-C11-Adversarial-Robustness.md)
12. [監視、ログ記録、異常検出 (Monitoring, Logging & Anomaly Detection)](1.0/ja/0x10-C12-Monitoring-and-Logging.md)

## Appendices

* [付録 A: 用語集](1.0/ja/0x90-Appendix-A_Glossary.md)
* [付録 B: AI セキュリティコントロールインベントリ (AI Security Controls Inventory)](1.0/ja/0x91-Appendix-B_AI_Security_Controls_Inventory.md)
* [付録 C: AI 支援のセキュアコーディング (AI-Assisted Secure Coding)](1.0/ja/0x92-Appendix-C_AI_for_Code_Generation.md)

## Research Wiki

For every requirement in the standard, the [Research Wiki](https://github.com/OWASP/AISVS/blob/main/1.0/research/README.md) provides implementation context beyond the requirement text:

| Column | What it tells you |
| --- | --- |
| **Threat Mitigated** | Specific attack techniques, CVEs, and real-world incidents the control defends against |
| **Verification Approach** | Concrete audit steps, tools, and evidence to collect |
| **Gaps & Notes** | Tool maturity ratings, open research questions, and implementation caveats |

The wiki covers all 191 requirements across 60 pages, with per-section threat landscape summaries, tooling recommendations, and references to current standards and research literature.

---

## How to Reference AISVS Requirements

Each requirement has an identifier in the format `C<chapter>.<section>.<requirement>`, where each element is a number, for example `C9.4.3`.

- The `C<chapter>` value corresponds to the chapter from which the requirement comes; for example, all `C9.#.#` requirements are from the 'Orchestration & Agentic Security' chapter.
- The `<section>` value corresponds to the section within that chapter where the requirement appears; for example, all `C9.4.#` requirements are in the 'Agent and Orchestrator Identity' section.
- The `<requirement>` value identifies the specific requirement within the chapter and section; for example, `C9.4.3` which as of version 1.0 of this standard is:

> Verify that agent identity credentials rotate on a defined schedule.

Since identifiers may change between versions of the standard, it is preferable for other documents, reports, or tools to use the following format: `v<version>-C<chapter>.<section>.<requirement>`, where `version` is the AISVS version tag. For example: `v1.0-C9.4.3`.

Note: The `v` preceding the version number should always be lowercase.

If identifiers are used without including the `v<version>` element, they should be assumed to refer to the latest AISVS content. As the standard grows and changes, this becomes problematic, which is why writers or developers should include the version element.

---

## Versioning

AISVS uses a two-part version number, `v<MAJOR>.<MINOR>` (for example, `v1.0`, `v1.01`, `v2.0`). Major versions cover chapter and section changes, minor versions cover additions, removals, and material edits to requirements within the existing structure, and patch fixes ship in-branch without a separate version. The full policy is documented in [RELEASE.md](RELEASE.md).

Each stable release of AISVS is published as a numbered folder in this repository. Once a version is released, its folder is locked; all future work happens in a new folder. This mirrors the approach used by [OWASP ASVS](https://github.com/OWASP/ASVS).

```text
/
├── 1.0/        <- published stable release (locked)
├── 1.01-dev/   <- next minor release (in progress)
├── 2.0-dev/    <- next major release (in progress)
```

---

## Contributing

We welcome contributions from the community. Please [open an issue](https://github.com/OWASP/AISVS/issues) to report bugs or suggest improvements. We may ask you to [submit a pull request](https://github.com/OWASP/AISVS/pulls) based on the discussion.

To report a security issue with the AISVS project itself, please follow the [Security Policy](https://github.com/OWASP/AISVS/blob/main/SECURITY.md).

## ライセンス

本プロジェクトのすべてのコンテンツは **[Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/)** ライセンスの下にあります。
