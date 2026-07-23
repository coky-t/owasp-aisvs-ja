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

このプロジェクトは [Jim Manico](https://linkedin.com/in/jmanico) によって設立されました。現在のプロジェクトリーダーは [Jim Manico](https://linkedin.com/in/jmanico), [Otto Sulin](https://github.com/ottosulin), [Rico Komenda](https://github.com/RicoKomenda), [Russ Memisyazici](https://linkedin.com/in/vtxmm) です。

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

## 検証レベル

各 AISVS 要件はセキュリティ保証の深度を示す検証レベル (1, 2, 3) を割り当てられています。

| レベル| 説明 | 使用対象 |
| :---: | --- | --- |
| **1** | すべての AI システムが実装すべき不可欠なベースラインコントロール。 | 内部ツールや低リスクシステムを含む、すべての AI アプリケーション。 |
| **2** | 機密データを扱うシステムや重要な判断を行うシステム向けの標準コントロール。 | 本番システム、顧客向け AI、個人データを処理するシステム。 |
| **3** | 高度な攻撃に対する防御を必要とする高保証環境向けの高度なコントロール。 | 重要インフラストラクチャ、安全性が極めて重要な AI、高価値ターゲット、規制対象の業界。 |

組織は AI システムのリスクプロファイルに基づいてターゲットレベルを選択する必要があります。ほとんどの本番システムでは少なくともレベル 2 を目指すべきです。

## AISVS の使用方法

* **設計時。** AI システムをアーキテクトする際、要件をセキュリティチェックリストとして使用します。
* **開発時。** 要件を CI/CD パイプライン、コードレビュー、テストに統合します。
* **セキュリティ評価時。** ペネトレーションテストや監査に対する検証フレームワークとして使用します。
* **調達時。** AI ベンダーやサードパーティのモデルを評価する際、具体的な要件を参照します。

## 要件の章構成

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

## 付録

* [付録 A: 用語集](1.0/ja/0x90-Appendix-A_Glossary.md)
* [付録 B: AI セキュリティコントロールインベントリ (AI Security Controls Inventory)](1.0/ja/0x91-Appendix-B_AI_Security_Controls_Inventory.md)
* [付録 C: AI 支援のセキュアコーディング (AI-Assisted Secure Coding)](1.0/ja/0x92-Appendix-C_AI_for_Code_Generation.md)

## Research Wiki

本標準の各要件に対して、[Research Wiki](https://github.com/OWASP/AISVS/blob/main/1.0/research/README.md) は要件の記述にとどまらない実装上の背景を提供します。

| 項目 | 内容 |
| --- | --- |
| **緩和される脅威 (Threat Mitigated)** | そのコントロールが防御する具体的な攻撃技法、CVE、実際のインシデント |
| **検証アプローチ (Verification Approach)** | 具体的な監査手順、ツール、収集する証跡 |
| **ギャップと注記 (Gaps & Notes)** | ツールの成熟度評価、未解決の調査課題、実装上の留意点 |

この Wiki は 60 ページにわたる全 191 の要件をカバーしており、セクションごとの脅威動向の概要、推奨ツール、最新規格や研究文献への参照情報があります。

---

## AISVS 要件の参照方法

各要件は `C<chapter>.<section>.<requirement>` という形式の識別子を有します。各要素は数字です。例: `C9.4.3`

- `C<chapter>` の値はその要件に由来する章に対応しています。たとえば、`C9.#.#` という要件はすべて 'オーケストレーションとエージェントセキュリティ (Orchestration & Agentic Security)' の章のものです。
- `<section>` の値は要件が現れる章内の節に対応しています。たとえば、`C9.4.#` という要件はすべて 'エージェントとオーケストレータのアイデンティティ (Agent and Orchestrator Identity)' の節のものです。
- `<requirement>` の値は章および節内の特定の要件を識別するものです。たとえば、`C9.4.3` は本標準のバージョン 1.0 では以下のとおりです。

> **検証:** エージェントのアイデンティティクレデンシャルは定義されたスケジュールで入れ替えている。

本標準のバージョン間で識別子が変更となる可能性があるため、他のドキュメント、レポート、ツールでは `v<version>-C<chapter>.<section>.<requirement>` という形式を使用することを推奨します。`version` は AISVS のバージョンタグです。例: `v1.0-C9.4.3`

注: バージョン番号の前の `v` は常に小文字にする必要があります。

識別子が `v<version>` 要素を含めずに使用されている場合、それは最新の AISVS コンテンツを参照していると想定すべきです。本標準が成長して変更すると、これは問題となり、これが作成者や開発者がバージョン要素を含む必要がある理由です。

---

## バージョン管理

AISVS では `v<MAJOR>.<MINOR>` (例: `v1.0`, `v1.01`, `v2.0`) という二つの部分からなるバージョン番号を使用します。メジャーバージョンは章や節の変更をカバーし、マイナーバージョンは既存の構成内での要件の追加、削除、内容の編集をカバーします。パッチ修正は個別のバージョンなしでブランチ内で出荷します。詳細なポリシーについては [RELEASE.md](https://github.com/OWASP/AISVS/blob/main/RELEASE.md) で説明しています。

AISVS の各安定板リリースはこのリポジトリ内で番号付けされたフォルダとして公開されます。バージョンがリリースされると、そのフォルダはロックされます。以降のすべての取り組みは新しいフォルダで行います。これは [OWASP ASVS](https://github.com/OWASP/ASVS) で使用されたアプローチを反映しています。

```text
/
├── 1.0/        <- published stable release (locked)
├── 1.01-dev/   <- next minor release (in progress)
```

---

## Contributing

We welcome contributions from the community. Please [open an issue](https://github.com/OWASP/AISVS/issues) to report bugs or suggest improvements. We may ask you to [submit a pull request](https://github.com/OWASP/AISVS/pulls) based on the discussion.

To report a security issue with the AISVS project itself, please follow the [Security Policy](https://github.com/OWASP/AISVS/blob/main/SECURITY.md).

## ライセンス

本プロジェクトのすべてのコンテンツは **[Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/)** ライセンスの下にあります。
