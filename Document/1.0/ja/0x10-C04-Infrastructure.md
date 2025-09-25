# C4 インフラストラクチャ、構成、デプロイメントセキュリティ (Infrastructure, Configuration & Deployment Security)

## 管理目標

AI インフラストラクチャは、安全な構成、ランタイム分離、信頼できるデプロイメントパイプライン、包括的な監視を通じて、権限昇格、サプライチェーン改竄、ラテラルムーブメントに対して堅牢化される必要があります。セキュリティ、完全性、監査可能性を確保する、管理されたプロセスを通じて、検証されて認可されたインフラストラクチャコンポーネントのみが本番環境に到達します。

**主要なセキュリティ目標:** 暗号署名され、脆弱性スキャンされたインフラストラクチャコンポーネントのみが、セキュリティポリシーを適用し、不変の監査証跡を維持する自動バリデーションパイプラインを通じて、本番環境に到達します。

---

## C4.1 ランタイム環境の分離 (Runtime Environment Isolation)

OS レベルの分離プリミティブにより、コンテナエスケープと権限昇格を防止します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.1.1** | **検証:** すべての AI ワークロードは、たとえばコンテナの場合は不要な Linux 機能を削除するなどにより、オペレーティングシステムに必要な最小限のパーミッションで実行している。 | 1 | D/V |
| **4.1.2** | **検証:** ワークロードは、サンドボックス、seccomp プロファイル、AppArmor、SELinux などの悪用を制限するテクノロジによって保護されており、構成は適切である。 | 1 | D/V |
| **4.1.3** | **検証:** ワークロードは読み取り専用のルートファイルシステムで実行し、書き込み可能なマウントは明示的に定義され、制限オプション (noexec, nosuid, nodev など) で堅牢化されている。 | 2 | D/V |
| **4.1.4** | **検証:** ランタイム監視は権限昇格やコンテナ脱出の動作を検出し、問題のあるプロセスを自動的に終了している。 | 2 | D/V |
| **4.1.5** | **検証:** 高リスクの AI ワークロードは、リモートアテステーションに成功した後にのみ、ハードウェア分離された環境 (TEE、信頼できるハイパーバイザー、ベアメタルノードなど) で実行している。 | 3 | D/V |

---

## C4.2 セキュアなビルドとデプロイメントパイプライン(Secure Build & Deployment Pipelines)

再現可能なビルドと署名されたアーティファクトを通じて、暗号論的完全性とサプライチェーンセキュリティを確保します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.2.1** | **検証:** ビルドは再現可能であり、独立した検証できるビルドアーティファクトに応じて署名された来歴メタデータを生成している。 | 1 | D/V |
| **4.2.2** | **検証:** ビルドはソフトウェア部品表 (SBOM) を生成し、デプロイメントのために許可される前に署名されている。 | 2 | D/V |
| **4.2.3** | **検証:** ビルドアーティファクト (コンテナイメージなど) の署名と来歴メタデータはデプロイメント時に検証されており、検証されていないアーティファクトは拒否されている。 | 2 | D/V |


---

## C4.3 ネットワークセキュリティとアクセス制御 (Network Security & Access Control)

デフォルト拒否ポリシーと暗号化通信でゼロトラストネットワークを実装します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.3.1** | **Verify that** network policies enforce default-deny ingress and egress, with only required services explicitly allowed. | 1 | D/V |
| **4.3.2** | **Verify that** administrative access protocols (e.g., SSH, RDP) and access to cloud metadata services are restricted and require strong authentication. | 1 | D/V |
| **4.3.3** | **Verify that** egress traffic is restricted to approved destinations and all requests are logged. | 2 | D/V |
| **4.3.4** | **Verify that** inter-service communication uses mutual TLS with certificate validation and regular automated rotation. | 2 | D/V |
| **4.3.5** | **Verify that** AI workloads and environments (dev, test, prod) run in isolated network segments (VPCs/VNets) with no direct internet access and no shared IAM roles, security groups, or cross-environment connectivity. | 2 | D/V |

## C4.4 シークレットと暗号鍵管理 (Secrets & Cryptographic Key Management)

Protect secrets and cryptographic keys with secure storage, automated rotation, and strong access controls.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.4.1** | **Verify that** secrets are stored in a dedicated secrets management system with encryption at rest and isolated from application workloads. | 1 | D/V |
| **4.4.2** | **Verify that** cryptographic keys are generated and stored in hardware-backed modules (e.g., HSMs, cloud KMS). | 1 | D/V |
| **4.4.3** | **Verify that** secrets rotation is automated. | 2 | D/V |
| **4.4.4** | **Verify that** access to production secrets requires strong authentication. |
| **4.4.5** | **Verify that** secrets are deployed to applications at runtime through a secrets management solution. Secrets must never be embedded in source code, configuration files, build artifacts, container images, or environment variables. | 2 | D/V |

---

## C4.5 AI ワークロードのサンドボックス化とバリデーション (AI Workload Sandboxing & Validation)

Isolate untrusted AI models in secure sandboxes and protect sensitive AI workloads using trusted execution environments (TEEs) and confidential computing technologies.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.5.1** | **Verify that** external or untrusted AI models execute in isolated sandboxes.| 1 | D/V |
| **4.5.2** | **Verify that** sandboxed workloads have no outbound network connectivity by default, with any required access explicitly defined.| 1 | D/V |
| **4.5.3** | **Verify that** workload attestation is performed before model or workload loading, ensuring cryptographic proof of a trusted execution environment. | 2 | D/V |
| **4.5.4** | **Verify that** confidential workloads execute within a trusted execution environment (TEE) that provides hardware-enforced isolation, memory encryption, and integrity protection. | 3 | D/V |
| **4.5.5** | **Verify that** confidential inference services prevent model extraction through encrypted computation with sealed model weights and protected execution. | 3 | D/V |
| **4.5.6** | **Verify that** orchestration of trusted execution environments includes lifecycle management, remote attestation, and encrypted communication channels. | 3 | D/V |
| **4.5.7** | **Verify that** secure multi-party computation (SMPC) enables collaborative AI training without exposing individual datasets or model parameters. | 3 | D/V |

---

## C4.6 AI インフラストラクチャリソース管理、バックアップとリカバリ (AI Infrastructure Resource Management, Backup and Recovery)

Prevent resource exhaustion attacks and ensure fair resource allocation through quotas and monitoring. Maintain infrastructure resilience through secure backups, tested recovery procedures, and disaster recovery capabilities.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.6.1** | **Verify that** workload's resource consumption is limited appropriately with e.g. Kubernetes ResourceQuotas or similar to mitigate Denial of Service attacks. | 2 | D/V |
| **4.6.2** | **Verify that** resource exhaustion triggers automated protections (e.g., rate limiting or workload isolation) once defined CPU, memory, or request thresholds are exceeded. | 2 | D/V |
| **4.6.3** | **Verify that** backup systems run in isolated networks with separate credentials, and the storage system is either run in an air-gapped network or implements WORM (write-once-read-many) protection against unauthorized modification. | 2 | D/V |

---

## C4.7 AI ハードウェアセキュリティ (AI Hardware Security)

GPU、TPU、特殊な AI アクセラレータなどの AI 固有のハードウェアコンポーネントを保護します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.7.1** | **Verify that** before workload execution, AI accelerator integrity is validated using hardware-based attestation mechanisms (e.g., TPM, DRTM, or equivalent). | 2 | D/V |
| **4.7.2** | **Verify that** accelerator (GPU) memory is isolated between workloads through partitioning mechanisms with memory sanitization between jobs. | 2 | D/V |
| **4.7.3** | **Verify that** hardware security modules (HSMs) protect AI model weights and cryptographic keys with certification to FIPS 140-3 Level 3 or Common Criteria EAL4+. | 3 | D/V |

---

## C4.8 エッジと分散型の AI セキュリティ (Edge & Distributed AI Security)

エッジコンピューティング、連合学習、マルチサイトアーキテクチャなどの分散 AI デプロイメントを保護します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.8.1** | **Verify that** edge AI devices authenticate to central infrastructure using mutual TLS. | 2 | D/V |
| **4.8.2** | **Verify that** edge devices implement secure boot with verified signatures and rollback protection to prevent firmware downgrade attacks. | 2 | D/V |
| **4.8.3** | **Verify that** distributed AI coordination uses Byzantine fault-tolerant consensus mechanisms with participant validation and malicious node detection. | 3 | D/V |
| **4.8.4** | **Verify that** edge-to-cloud communication supports bandwidth throttling, data compression, and secure offline operation with encrypted local storage. | 3 | D/V |

---

## 参考情報

* [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
* [CIS Controls v8](https://www.cisecurity.org/controls/v8)
* [OWASP Container Security Verification Standard](https://github.com/OWASP/Container-Security-Verification-Standard)
* [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/)
* [SLSA Supply Chain Security Framework](https://slsa.dev/)
* [Cloud Security Alliance: Cloud Controls Matrix](https://cloudsecurityalliance.org/research/cloud-controls-matrix/)
* [ENISA: Secure Infrastructure Design](https://www.enisa.europa.eu/topics/critical-information-infrastructures-and-services)
* [ISO 27001:2022 Information Security Management](https://www.iso.org/standard/27001)
* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
