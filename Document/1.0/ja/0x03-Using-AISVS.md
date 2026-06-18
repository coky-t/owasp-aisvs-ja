# AISVS を使用するには

人工知能セキュリティ検証標準 (AISVS) は現代の AI アプリケーションとサービスのセキュリティ要件を定義しており、アプリケーション開発者が制御できる側面に重点を置いています。

The AISVS is intended for anyone developing or evaluating the security of AI applications, including developers, architects, security engineers, and auditors. This chapter introduces the structure and use of the AISVS, including its verification levels, intended use cases, and how it is positioned alongside other security standards.

## 人工知能セキュリティ検証レベル

AISVS は三段階のセキュリティ検証レベルを定義しています。各レベルは深さと複雑さを増していき、組織が AI システムのリスクレベルに合わせてセキュリティ態勢を調整できます。

Organizations may begin at Level 1 and progressively adopt higher levels as security maturity and threat exposure increase. AISVS levels are aligned with [ASVS](https://owasp.org/www-project-application-security-verification-standard/) levels and are intended to be applied at the matching ASVS level (see [Alignment with ASVS Levels](#alignment-with-asvs-levels)).

### レベルの定義

AISVS v1.0 の各要件は以下のいずれかのレベルに割り当てられます。

#### レベル 1 要件

レベル 1 は最も重要で基本的なセキュリティ要件を含みます。これらは他の前提条件や脆弱性に依存しない一般的な攻撃の防止に重点を置いています。レベル 1 コントロールのほとんどは、実装が容易であるか、その労力を正当化するのに十分なほど不可欠です。

#### レベル 2 要件

レベル 2 はより高度な攻撃やあまり一般的でない攻撃、および広範な脅威に対する多層防御に対応します。これらの要件は、より複雑なロジックや、特定のターゲットに対する攻撃を前提条件とすることがあります。

#### レベル 3 要件

レベル 3 は、一般的に実装が困難であったり、状況に応じて適用可能であるコントロールを含みます。これらは、ニッチな攻撃、標的型攻撃、高度に複雑な攻撃に対する多層防御や緩和策を表すことがよくあります。

## Scope of the AISVS

AISVS is intentionally narrow. It only defines security requirements that are specific to AI and ML systems, or where general security controls have AI-specific nuances that warrant restating. It is not a self-contained security program for an AI application. AISVS assumes that the underlying application, infrastructure, and organizational practices are already verified against established general-purpose standards, and adds the AI-specific layer on top.

The following are intentionally out of scope and are not duplicated in AISVS chapters:

* **General application security.** Authentication, session management, authorization, transport security, input and output handling for non-AI surfaces, secrets management, file upload handling, error handling, and similar controls are defined by the [OWASP Application Security Verification Standard (ASVS)](https://owasp.org/www-project-application-security-verification-standard/).
* **General software supply chain security.** Dependency scanning, version pinning, lockfile enforcement, build provenance, reproducible builds, generic SBOM generation, and CI/CD pipeline integrity are defined by the [OWASP Software Component Verification Standard (SCVS)](https://owasp.org/www-project-software-component-verification-standard/), [SLSA](https://slsa.dev/), and the [CIS Controls](https://www.cisecurity.org/controls).
* **General infrastructure and platform hardening.** Container, host, network, cloud, and Kubernetes baseline hardening are defined by the [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks), [NIST SP 800-53](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final), [NIST SP 800-190](https://csrc.nist.gov/pubs/sp/800/190/final), and the [NIST Cybersecurity Framework (CSF)](https://www.nist.gov/publications/nist-cybersecurity-framework-csf-20).
* **General data protection and privacy operations.** Data classification, encryption at rest and in transit, retention scheduling, secure deletion of conventional storage, audit log immutability, and consent management platform operation are defined by ASVS, [ISO/IEC 27001](https://www.iso.org/standard/27001), and applicable privacy regulations such as the GDPR.
* **General logging and monitoring.** Log storage access control, retention, backup, encryption, redaction, tamper protection, SIEM integration, and operational telemetry are defined by ASVS and standard observability practice.
* **AI governance and risk management.** Organizational AI governance, AI impact assessments, fairness and ethics documentation, model cards, public transparency reports, and risk-management process design are defined by [ISO/IEC 42001](https://www.iso.org/standard/81230.html), [ISO/IEC 23894](https://www.iso.org/standard/77304.html), and the [NIST AI RMF](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-ai-rmf-10).

When verifying an AI application against AISVS, the equivalent level of those underlying standards should be verified in parallel. AISVS by itself is not a substitute for, and does not subsume, any of them.

## Alignment with ASVS Levels

AISVS levels are aligned with [ASVS](https://owasp.org/www-project-application-security-verification-standard/) levels. Verifying an AI application against AISVS Level _N_ assumes the application has also been, or is being, verified against ASVS Level _N_. The two standards are designed to be applied together at matching levels:

| AISVS Level | Corresponding ASVS Level | Typical use |
| :---: | :---: | --- |
| 1 | 1 | Baseline security for any AI application that handles untrusted input or operates on data of any sensitivity. |
| 2 | 2 | AI applications handling sensitive business data, regulated data, or operating in adversarial contexts. |
| 3 | 3 | High-assurance AI applications such as those handling life-safety decisions, critical infrastructure, or highly sensitive personal data. |

If an AISVS requirement appears to overlap with an ASVS requirement, the AISVS version is restated only because it has AI-specific implementation details, attack surface, or evidence that an auditor needs to evaluate differently. In all other cases, the ASVS requirement governs and AISVS does not repeat it.

## Cross-References Inside AISVS

AISVS chapters are organized by control family rather than by attack or component. As a result, defending against a given AI threat usually requires applying requirements from several chapters together. For example, defending against prompt injection in an agentic application combines requirements from C2 (input validation), C7 (output controls), C9 (agent action gating and human oversight), C10 (MCP-specific controls), C11 (adversarial robustness), and C13 (detection and logging).

Individual chapters and sections do not enumerate which other AISVS chapters cover related concerns. When applying AISVS, treat the standard as a whole and consult Appendix D (AI Security Controls Inventory) for a cross-cutting view of where each defense technique appears.
