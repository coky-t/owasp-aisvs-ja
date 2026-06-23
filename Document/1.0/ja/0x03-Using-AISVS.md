# AISVS を使用するには

人工知能セキュリティ検証標準 (AISVS) は現代の AI アプリケーションとサービスのセキュリティ要件を定義しており、アプリケーション開発者が制御できる側面に重点を置いています。

The AISVS is intended for anyone developing or evaluating the security of AI applications, including developers, architects, security engineers, and auditors. This chapter introduces the structure and use of the AISVS, including its verification levels, intended use cases, and how it is positioned alongside other security standards.

## How to Read This Standard

### Chapter Structure

Each of the 12 requirement chapters follows the same format:

* **Control Objective.** A brief statement of the security goal for the chapter.
* **Sections.** Requirements are grouped into related sections, each with a short description of the defense goal.
* **Requirement Tables.** Individual requirements are presented in tables with the following columns:

| Column | Meaning |
| --- | --- |
| **#** | Unique requirement identifier (e.g., 1.1.1, 9.3.2). |
| **Description** | The requirement text, always beginning with "Verify that" to emphasize testability. |
| **Level** | The verification level (1, 2, or 3) indicating the depth of assurance required; see the verification levels below. |

### Appendices

Three appendices support the core requirements:

* **Appendix A (Glossary)** defines key terms and acronyms used throughout the standard.
* **Appendix B (AI Security Controls Inventory)** is a cross-reference of every defense technique in AISVS, organized by security control category (authentication, authorization, encryption, input validation, and so on) with mappings back to specific requirement identifiers.
* **Appendix C (AI-Assisted Secure Coding)** provides controls for the safe use of AI coding tools during software development.

## 人工知能セキュリティ検証レベル

AISVS は三段階のセキュリティ検証レベルを定義しています。各レベルは深さと複雑さを増していき、組織が AI システムのリスクレベルに合わせてセキュリティ態勢を調整できます。

Organizations may begin at Level 1 and progressively adopt higher levels as security maturity and threat exposure increase. AISVS levels are aligned with [ASVS](https://owasp.org/www-project-application-security-verification-standard/) levels and are intended to be applied at the matching ASVS level (see Alignment with ASVS Levels below).

### レベルの定義

AISVS v1.0 の各要件は以下のいずれかのレベルに割り当てられます。

#### レベル 1 要件

レベル 1 は最も重要で基本的なセキュリティ要件を含みます。これらは他の前提条件や脆弱性に依存しない一般的な攻撃の防止に重点を置いています。レベル 1 コントロールのほとんどは、実装が容易であるか、その労力を正当化するのに十分なほど不可欠です。

#### レベル 2 要件

レベル 2 はより高度な攻撃やあまり一般的でない攻撃、および広範な脅威に対する多層防御に対応します。これらの要件は、より複雑なロジックや、特定のターゲットに対する攻撃を前提条件とすることがあります。

#### レベル 3 要件

レベル 3 は、一般的に実装が困難であったり、状況に応じて適用可能であるコントロールを含みます。これらは、ニッチな攻撃、標的型攻撃、高度に複雑な攻撃に対する多層防御や緩和策を表すことがよくあります。

## Alignment with ASVS Levels

AISVS levels are aligned with [ASVS](https://owasp.org/www-project-application-security-verification-standard/) levels. Verifying an AI application against AISVS Level _N_ assumes the application has also been, or is being, verified against ASVS Level _N_. The two standards are designed to be applied together at matching levels:

| AISVS Level | Corresponding ASVS Level | Typical use |
| :---: | :---: | --- |
| 1 | 1 | Baseline security for any AI application that handles untrusted input or operates on data of any sensitivity. |
| 2 | 2 | AI applications handling sensitive business data, regulated data, or operating in adversarial contexts. |
| 3 | 3 | High-assurance AI applications such as those handling life-safety decisions, critical infrastructure, or highly sensitive personal data. |

If an AISVS requirement appears to overlap with an ASVS requirement, the AISVS version is restated only because it has AI-specific implementation details, attack surface, or evidence that an auditor needs to evaluate differently.

## Scope of the AISVS

AISVS is intentionally narrow. It only defines security requirements that are specific to AI and ML systems, or where general security controls have AI-specific nuances that warrant restating. It is not a self-contained security program for an AI application. AISVS assumes that the underlying application, infrastructure, and organizational practices are already verified against established general-purpose standards, and adds the AI-specific layer.

The following are intentionally out of scope and are not duplicated in AISVS chapters:

* **General application security.** Authentication, session management, authorization, transport security, input and output handling for non-AI surfaces, secrets management, file upload handling, error handling, and similar controls are defined by the [OWASP Application Security Verification Standard (ASVS)](https://owasp.org/www-project-application-security-verification-standard/).
* **General software supply chain security.** Dependency scanning, version pinning, lockfile enforcement, build provenance, reproducible builds, generic SBOM generation, and CI/CD pipeline integrity are defined by the [OWASP Software Component Verification Standard (SCVS)](https://owasp.org/www-project-software-component-verification-standard/), [SLSA](https://slsa.dev/), and the [CIS Controls](https://www.cisecurity.org/controls).
* **General infrastructure and platform hardening.** Container, host, network, cloud, and Kubernetes baseline hardening are defined by the [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks), [NIST SP 800-53](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final), [NIST SP 800-190](https://csrc.nist.gov/pubs/sp/800/190/final), and the [NIST Cybersecurity Framework (CSF)](https://www.nist.gov/publications/nist-cybersecurity-framework-csf-20).
* **General data protection and privacy operations.** Data classification, encryption at rest and in transit, retention scheduling, secure deletion of conventional storage, audit log immutability, and consent management platform operation are defined by ASVS, [ISO/IEC 27001](https://www.iso.org/standard/27001), and applicable privacy regulations such as the GDPR.
* **General logging and monitoring.** Log storage access control, retention, backup, encryption, redaction, tamper protection, SIEM integration, and operational telemetry are defined by ASVS and standard observability practice.
* **AI governance and risk management.** Organizational AI governance, AI impact assessments, fairness and ethics documentation, model cards, public transparency reports, and risk-management process design are defined by [ISO/IEC 42001](https://www.iso.org/standard/81230.html), [ISO/IEC 23894](https://www.iso.org/standard/77304.html), and the [NIST AI RMF](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-ai-rmf-10).
* **Vendor-specific guidance.** AISVS is vendor-neutral. It specifies what to verify, not which product to use.

When verifying an AI application against AISVS, the equivalent level of those underlying standards should be verified in parallel.

## Cross-References Inside AISVS

AISVS chapters are organized by control family rather than by attack or component. As a result, defending against a given AI threat usually requires applying requirements from several chapters together. For example, defending against prompt injection in an agentic application combines requirements from C2 (input validation), C7 (model behavior), C9 (orchestration and agentic security), C10 (MCP-specific controls), C11 (adversarial robustness), and C12 (detection and logging).

When applying AISVS, treat the standard as a whole and consult Appendix B (AI Security Controls Inventory) for a cross-cutting view of where each defense technique appears.

## AISVS Requirements and Scope in Assessments

Requirements can often be assessed using a combination of technical testing and vendor documentation, such as model cards for AI models. Another option is to mark requirements outside the organization's control as out of scope.
