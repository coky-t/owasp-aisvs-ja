# OWASP Artificial Intelligence Security Verification Standard (AISVS)

[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

この著作物は [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa] の下でライセンスされています。


[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg

## はじめに

人工知能セキュリティ検証標準 (AISVS) は開発者、アーキテクト、セキュリティ専門家に、AI 駆動型アプリケーションのセキュリティと倫理的考慮事項を評価および検証するための構造化されたチェックリストを提供することに重点を置いています。既存の OWASP 標準 (ウェブアプリケーション向けの ASVS など) をモデルとした AISVS は、以下の領域における要件のカテゴリを定義します。

1. [トレーニングデータガバナンスとバイアス管理 (Training Data Governance & Bias Management)](1.0/ja/0x10-C01-Training-Data-Governance.md)
2. [ユーザー入力バリデーション (User Input Validation)](1.0/ja/0x10-C02-User-Input-Validation.md)
3. [モデルライフサイクル管理と変更管理 (Model Lifecycle Management & Change Control)](1.0/ja/0x10-C03-Model-Lifecycle-Management.md)
4. [インフラストラクチャ、構成、デプロイメントセキュリティ (Infrastructure, Configuration & Deployment Security)](1.0/ja/0x10-C04-Infrastructure.md)
5. [アクセス制御とアイデンティティ (Access Control & Identity)](1.0/ja/0x10-C05-Access-Control-and-Identity.md)
6. [モデル、フレームワーク、データのサプライチェーンセキュリティ (Supply Chain Security for Models, Frameworks & Data)](1.0/ja/0x10-C06-Supply-Chain.md)
7. [モデル動作、出力制御、安全保証 (Model Behavior, Output Control & Safety Assurance)](1.0/ja/0x10-C07-Model-Behavior.md)
8. [メモリ、エンベディング、ベクトルデータベースセキュリティ (Memory, Embeddings & Vector Database Security)](1.0/ja/0x10-C08-Memory-Embeddings-and-Vector-Database.md)
9. [自律オーケストレーションとエージェントアクションセキュリティ (Autonomous Orchestration & Agentic Action Security)](1.0/ja/0x10-C09-Orchestration-and-Agentic-Action.md)
10. [敵対的堅牢性と攻撃耐性 (Adversarial Robustness & Attack Resistance)](1.0/ja/0x10-C10-Adversarial-Robustness.md)
11. [プライバシー保護と個人データ管理 (Privacy Protection & Personal Data Management)](1.0/ja/0x10-C11-Privacy.md)
12. [監視、ログ記録、異常検出 (Monitoring, Logging & Anomaly Detection)](1.0/ja/0x10-C12-Monitoring-and-Logging.md)
13. [人間による監視と信頼 (Human Oversight and Trust)](1.0/ja/0x10-C13-Human-Oversight.md)

**バグを見つけた場合やご意見がある場合には [issue を記録](https://github.com/OWASP/ASIVS/issues) してください。その後、その issue での議論に基づいて [プルリクエストを開く](https://github.com/OWASP/AISVS/pulls) ようにお願いすることがあります。**

## プロジェクトリーダー

このプロジェクトは [Jim Manico](https://github.com/jmanico) と [Russ Memisyazici](https://github.com/vtknightmare) の二人のプロジェクトリーダーが主導しています。

## ライセンス

本プロジェクトのすべてのコンテンツは **[Creative Commons Attribution-Share Alike v4.0](https://creativecommons.org/licenses/by-sa/4.0/)** ライセンスの下にあります。
