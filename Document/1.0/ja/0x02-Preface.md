# 序文

**人工知能セキュリティ検証標準 (AISVS) バージョン 1.0** へようこそ。

## AISVS が存在する理由

AI システムは従来のアプリケーションセキュリティ標準では対処するように設計されていなかったセキュリティリスクをもたらします。プロンプトインジェクションは攻撃者が細工した入力を通じてモデルの命令を上書きし、言語モデルをデータ抽出、不正なアクション、または安全性のバイパスのためのツールに変えることができます。トレーニングデータはバックドアの設置やモデルの動作劣化によって侵害される可能性があります。モデルは敵対的入力を通じて抽出、反転、操作される可能性があります。自律エージェントは、正当な命令と区別できないプロンプトインジェクションされた命令に基づいて、現実世界に影響のあるアクションを取る可能性があります。検索パイプラインは機密情報の漏洩やモデルコンテキストへの悪意のあるコンテンツの注入に悪用される可能性があります。さらに、モデル、データセット、フレームワークのサプライチェーンは、既存のソフトウェアコンポジション解析だけでは解決できない、新たな完全性の課題を抱えています。

AISVS は、組織がこれらのリスクのために専用に設計された、構造化されテスト可能な一連のセキュリティコントロールを提供するために作成されました。既存の標準を置き換えるものではなく、それらではカバーできないギャップを埋めるものです。

## 設計原則

The standard is organized into 14 chapters (controls). Each chapter is split into thematic sections, which together provide a holistic approach to achieve the control objective. Sections are broken into requirements. A section may not provide requirements for each level and must not contain requirements present elsewhere in the standard.

Each requirement must address a single concern that can, in common scenarios, be implemented and audited as one technical mechanism. A requirement may specify progressively stricter criteria at higher levels, which are, if present, represented as separate requirements in the section. Requirements must use clear, technology-neutral language to the extent possible, referencing well-known technologies as examples where deemed useful for clarity.

Every requirement in AISVS follows these principles derived from the standard's name:

* **Artificial Intelligence.** Each control operates at the AI or ML layer (data, model, pipeline, agent, or inference) and addresses risks specific to AI systems rather than general application security. The standard does not duplicate controls from other broadly adopted standards or frameworks (such as ASVS), unless the control has unique, AI-specific implementation concerns.
* **Security.** Each requirement directly mitigates an identified security, privacy, or safety risk. Controls that serve only operational, governance, compliance, or business objectives are out of scope.
* **Verification.** Requirements are written so that conformance can be objectively validated through testing, inspection, or audit. Sufficient implementation guidance or tooling must exist to allow both implementation and effective verification of the requirement; purely theoretical, subjective, or aspirational guidance is excluded.
* **Standard.** All chapters follow a consistent structure and terminology to form a coherent, navigable reference document.

## 本標準の読み方

### 章構成

Each of the 14 requirement chapters follows the same format:

* **Control Objective.** A brief statement of the security goal for the chapter.
* **Sections.** Requirements are grouped into related sections, each with a short description of the defense goal.
* **Requirement Tables.** Individual requirements are presented in tables with the following columns:

| Column | Meaning |
|---|---|
| **#** | Unique requirement identifier (e.g., 1.1.1, 9.3.2). |
| **Description** | The requirement text, always beginning with "Verify that" to emphasize testability. |
| **Level** | The verification level (1, 2, or 3) indicating the depth of assurance required. |

### 付録

Four appendices support the core requirements:

* **Appendix A (Glossary)** defines key terms and acronyms used throughout the standard.
* **Appendix B (References)** lists external standards, research, and frameworks referenced by AISVS requirements.
* **Appendix C (AI-Assisted Secure Coding)** provides controls for the safe use of AI coding tools during software development.
* **Appendix D (AI Security Controls Inventory)** is a cross-reference of every defense technique in AISVS, organized by security control category (authentication, authorization, encryption, input validation, and so on) with mappings back to specific requirement identifiers.

## スコープ境界

AISVS focuses on security controls that are specific to AI and ML systems. It intentionally excludes:

* **General application security.** Requirements covered by the [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) (such as session management, CSRF protection, or SQL injection prevention) are not repeated here. Organizations should apply ASVS alongside AISVS.
* **AI governance and risk management.** Organizational governance, risk assessment methodology, and compliance processes are better addressed by frameworks such as the NIST AI RMF and ISO/IEC 42001.
* **Vendor-specific guidance.** AISVS is vendor-neutral. It specifies what to verify, not which product to use.

## 謝辞

AISVS v1.0 is the result of a collaborative effort by its project leads, working group members, and community contributors. We thank everyone who has contributed requirements, reviews, and feedback to make this standard possible. A full list of contributors is available in the [Frontispiece](0x01-Frontispiece.md).

By adopting AISVS, organizations can systematically evaluate and strengthen the security posture of their AI systems, building a foundation of secure AI engineering practices that evolves alongside the technology itself.
