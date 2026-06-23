# 序文

**人工知能セキュリティ検証標準 (AISVS) バージョン 1.0** へようこそ。

By adopting AISVS, organizations can systematically evaluate and strengthen the security posture of their AI systems, building a foundation of secure AI engineering practices that evolves alongside the technology itself.

## AISVS が存在する理由

AI システムは従来のアプリケーションセキュリティ標準では対処するように設計されていなかったセキュリティリスクをもたらします。プロンプトインジェクションは攻撃者が細工した入力を通じてモデルの命令を上書きし、言語モデルをデータ抽出、不正なアクション、または安全性コントロールのバイパスのためのツールに変えることができます。トレーニングデータはバックドアの設置やモデルの動作劣化によって侵害される可能性があります。モデルは敵対的入力を通じて抽出、反転、操作される可能性があります。自律エージェントは、正当な命令と区別できないプロンプトインジェクションされた命令に基づいて動作し、現実世界に影響のあるアクションを取る可能性があります。検索パイプラインは機密情報の漏洩やモデルコンテキストへの悪意のあるコンテンツの注入に悪用される可能性があります。モデル、データセット、フレームワークのサプライチェーンは、既存のソフトウェアコンポジション解析だけでは解決できない、新たな完全性の課題を抱えています。

AISVS は、組織がこれらのリスクのために専用に設計された、構造化されテスト可能な一連のセキュリティコントロールを提供するために作成されました。既存の標準を置き換えるものではなく、それらではカバーできないギャップを埋めるものです。

## 設計原則

AISVS is organized into 12 control families. Each control family is divided into focused sections that support its control objective. Each section contains verification requirements. AISVS defines three verification levels, defined under Using the AISVS; sections need not include requirements at every level.

Each requirement must address a single concern that can ordinarily be implemented and verified as one technical mechanism. Requirements must not duplicate controls defined elsewhere in AISVS. Higher assurance levels may introduce stricter criteria, but those criteria must be stated as separate requirements. Requirements should use clear, technology-neutral language, referencing specific technologies only as examples where they improve clarity.

Every AISVS requirement follows four design principles derived from the standard’s name:

* **Artificial Intelligence.** Requirements must address AI/ML-specific assets, workflows, or runtime behavior, including datasets, models, training and evaluation pipelines, retrieval systems, agents, tools, memory, and inference-time operation. AISVS does not duplicate general application security controls from standards such as ASVS unless the control has AI-specific implementation or verification concerns.
* **Security.** Requirements must mitigate an identifiable security, privacy, or safety risk. Controls that serve only operational, governance, compliance, or business objectives are out of scope.
* **Verification.** Requirements must be objectively verifiable through testing, inspection, or audit. Sufficient implementation guidance or tooling must exist to support both implementation and verification. Purely theoretical, subjective, or aspirational guidance is excluded.
* **Standard.** Requirements must use consistent structure, terminology, and assurance-level semantics so AISVS remains coherent, navigable, and suitable for repeatable assessment.
