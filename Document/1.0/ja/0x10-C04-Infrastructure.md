# C4 インフラストラクチャ、構成、デプロイメントセキュリティ (Infrastructure, Configuration & Deployment Security)

## 管理目標

AI インフラストラクチャは、安全な構成、ランタイム分離、信頼できるデプロイメントパイプライン、包括的な監視を通じて、権限昇格、サプライチェーン改竄、ラテラルムーブメントに対して堅牢化される必要があります。セキュリティ、完全性、監査可能性を維持する、管理されたプロセスを通じて、認可されて検証されたインフラストラクチャコンポーネントと構成のみが本番環境に到達します。

**主要なセキュリティ目標:** 暗号署名され、脆弱性スキャンされたインフラストラクチャコンポーネントのみが、セキュリティポリシーを適用し、不変の監査証跡を維持する自動バリデーションパイプラインを通じて、本番環境に到達します。

---

## C4.1 ランタイム環境の分離 (Runtime Environment Isolation)

カーネルレベルの分離プリミティブと強制アクセス制御により、コンテナエスケープと権限昇格を防止します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.1.1** | **検証:** すべての AI コンテナは、CAP_SETUID、CAP_SETGID、およびセキュリティベースラインに文書化されている明示的に必要な機能を除く、すべての Linux 機能を削除している。 | 1 | D/V |
| **4.1.2** | **検証:** seccomp プロファイルは事前に承認された許可リストにあるものを除くすべてのシステムコールをブロックし、違反がある場合はコンテナを終了してセキュリティアラートを生成している。 | 1 | D/V |
| **4.1.3** | **検証:** AI ワークロードは、読み取り専用のルートファイルシステム、一時データ用の tmpfs、および noexec マウントオプションが適用された永続データ用の名前付きボリュームで実行している。 | 2 | D/V |
| **4.1.4** | **検証:** eBPF ベースのランタイムモニタリング (Falco、Tetragon、または同等のもの) は権限昇格の試みを検出し、問題のあるプロセスを組織の応答時間要件内で自動的に強制終了している。 | 2 | D/V |
| **4.1.5** | **検証:** 高リスクの AI ワークロードは、アテステーション検証を備えたハードウェア分離された環境 (Intel TXT、AMD SVM、または専用のベアメタルノード) で実行している。 | 3 | D/V |

---

## C4.2 セキュアなビルドとデプロイメントパイプライン(Secure Build & Deployment Pipelines)

再現可能なビルドと署名されたアーティファクトを通じて、暗号的完全性とサプライチェーンセキュリティを確保します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.2.1** | **検証:** Infrastructure as Code はコミットごとにツール (tfsec、Checkov、または Terrascan) でスキャンされ、重大度が CRITICAL または HIGH の検出結果であるマージをブロックしている。 | 1 | D/V |
| **4.2.2** | **検証:** コンテナビルドはビルド間の同一の SHA256 ハッシュで再現可能であり、Sigstore で署名された SLSA レベル 3 の Provenance Attestation を生成している。 | 1 | D/V |
| **4.2.3** | **検証:** コンテナイメージは CycloneDX または SPDX SBOM が埋め込まれており、レジストリプッシュ前に Cosign で署名されている。署名されていないイメージはデプロイメント時に拒否されている。 | 2 | D/V |
| **4.2.4** | **検証:** CI/CD パイプラインは、組織のセキュリティポリシー制限を超えない有効期間を備えた、HashiCorp Vault、AWS IAM Roles、または Azure Managed Identity からの OIDC トークンを使用している。 | 2 | D/V |
| **4.2.5** | **検証:** Cosign 署名と SLSA Provance はコンテナ実行前のデプロイメント時に検証されており、検証エラーはデプロイメントを失敗にしている。 | 2 | D/V |
| **4.2.6** | **検証:** ビルド環境は、永続的なストレージではなく、本番環境の VPC からネットワーク分離した一時的なコンテナまたは VM で実行している。 | 2 | D/V |

---

## C4.3 ネットワークセキュリティとアクセス制御 (Network Security & Access Control)

デフォルト拒否ポリシーと暗号化通信でゼロトラストネットワークを実装します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.3.1** | **検証:** Kubernetes NetworkPolicies または同等のポリシーは、必要なポート (443, 8080 など) に対して明示的な許可ルールで、デフォルト拒否の送受信 (ingress/egress) を実装している。 | 1 | D/V |
| **4.3.2** | **検証:** SSH (ポート 22), RDP (ポート 3389), クラウドメタデータエンドポイント (169.254.169.254) はブロックされているか、証明書ベースの認証を必要としている。 | 1 | D/V |
| **4.3.3** | **検証:** 送出 (egress) トラフィックは HTTP/HTTPS プロキシ (Squid、Istio、またはクラウド NAT ゲートウェイ) を通じてドメイン許可リストでフィルタされており、ブロックされたリクエストはログ記録されている。 | 2 | D/V |
| **4.3.4** | **検証:** サービス間通信は組織のポリシーに従ってローテーションされる証明書での相互 TLS を使用しており、証明書バリデーションを強制している (検証スキップフラグなし)。 | 2 | D/V |
| **4.3.5** | **検証:** AI インフラストラクチャは直接インターネットアクセスできない専用の VPC/VNet で実行しており、NAT ゲートウェイまたは要塞ホストを通じてのみ通信している。 | 2 | D/V |

---

## C4.4 シークレットと暗号鍵管理 (Secrets & Cryptographic Key Management)

ハードウェア支援のストレージとゼロトラストアクセスでの自動ローテーションを通じてクレデンシャルを保護します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.4.1** | **検証:** シークレットは、保存時に AES-256 を使用して暗号化される、HashiCorp Vault、AWS Secrets Manager、Azure Key Vault、または Google Secret Manager に格納されている。 | 1 | D/V |
| **4.4.2** | **検証:** 暗号鍵は組織の暗号ポリシーに従って鍵ローテーションを実施する FIPS 140-2 レベル 2 HSM (AWS CloudHSM, Azure Dedicated HSM) で生成されている。 | 1 | D/V |
| **4.4.3** | **検証:** シークレットローテーションは、人員変更やセキュリティインシデントによってトリガーされる、ゼロダウンタイムデプロイメントと即時ローテーションで自動化されている。 | 2 | D/V |
| **4.4.4** | **検証:** コンテナイメージはツール (GitLeaks、TruffleHog、または detect-secrets) でスキャンされ、API キー、パスワード、証明書を含むビルドをブロックしている。 | 2 | D/V |
| **4.4.5** | **検証:** 本番環境シークレットへのアクセスはハードウェアトークン (YubiKey, FIDO2) での MFA を必要としており、ユーザーアイデンティティとタイムスタンプとともに不変な監査ログに記録されている。 | 2 | D/V |
| **4.4.6** | **検証:** シークレットは Kubernetes シークレット、マウントされたボリューム、または init コンテナを介して挿入されており、シークレットが環境変数やイメージに埋め込まれないことを確保している。 | 2 | D/V |

---

## C4.5 AI ワークロードのサンドボックス化とバリデーション (AI Workload Sandboxing & Validation)

包括的な動作解析で、信頼できない AI モデルを安全なサンドボックスに分離します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.5.1** | **検証:** 外部 AI モデルは、gVisor、microVMs (such as Firecracker, CrossVM)、または --security-opt=no-new-privileges と --read-only フラグを指定した Docker コンテナで実行している。 | 1 | D/V |
| **4.5.2** | **検証:** サンドボックス環境はネットワーク接続がない (--network=none)、またはすべての外部リクエストが iptables ルールでブロックされ、ローカルホストアクセスのみを可能にしている。 | 1 | D/V |
| **4.5.3** | **検証:** AI モデルバリデーションは組織で定義されたテストカバレッジでの自動化されたレッドチームテストと、バックドア検出のための動作分析を含んでいる。 | 2 | D/V |
| **4.5.4** | **検証:** AI モデルが本番環境に昇進される前に、そのサンドボックス結果は認可されたセキュリティ担当者によって暗号署名され、不変な監査ログに保存されている。 | 2 | D/V |
| **4.5.5** | **検証:** サンドボックス環境は評価の間ごとに破棄されており、ファイルシステムとメモリを完全にクリーンアップしてゴールデンイメージから再作成されている。 | 2 | D/V |

---

## C4.6 インフラストラクチャセキュリティの監視 (Infrastructure Security Monitoring)

自動修復とリアルタイムアラートで、インフラストラクチャを継続的にスキャンおよび監視します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.6.1** | **検証:** コンテナイメージは組織のスケジュールに従ってスキャンされており、重大 (CRITICAL) な脆弱性がある場合は組織のリスク閾値に基づいてデプロイメントをブロックしている。 | 1 | D/V |
| **4.6.2** | **検証:** インフラストラクチャは、組織で定義されたコンプライアンス閾値で、CIS ベンチマークまたは NIST 800-53 コントロールに合格しており、不合格したチェックは自動修復している。 | 1 | D/V |
| **4.6.3** | **検証:** 重大度が HIGH の脆弱性は、積極的に悪用される CVE に対する緊急手順を伴う、組織のリスク管理タイムラインに従ってパッチ適用されている。 | 2 | D/V |
| **4.6.4** | **検証:** セキュリティアラートは自動エンリッチメントを備えた CEF または STIX/TAXII 形式を使用する SIEM プラットフォーム (Splunk、Elastic、または Sentinel) と統合している。 | 2 | V |
| **4.6.5** | **検証:** インフラストラクチャメトリクスは SLA ダッシュボードとエグゼクティブレポートを備えた監視システム (Prometheus, DataDog) にエクスポートされている。 | 3 | V |
| **4.6.6** | **検証:** 構成ドリフトは組織の監視要件に従ってツール (Chef InSpec, AWS Config) を使用して検出されており、不正な変更については自動ロールバックしている。 | 2 | D/V |

---

## C4.7 AI インフラストラクチャリソース管理 (AI Infrastructure Resource Management)

リソース枯渇攻撃を防ぎ、クォータと監視を通じて公平なリソース割り当てを確保します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.7.1** | **検証:** GPU/TPU の使用率は監視されており、組織で定義された閾値でアラートし、キャパシティ管理ポリシーに基づいてアクティブ化される自動スケーリングまたはロードバランシングしている。 | 1 | D/V |
| **4.7.2** | **検証:** AI ワークロードメトリクス (推論の遅延、スループット、エラー率) は組織の管理要件に従って収集されており、インフラストラクチャの使用率との相互関係を比較されている。 | 1 | D/V |
| **4.7.3** | **検証:** Kubernetes ResourceQuotas または同等のものは組織のリソース割り当てポリシーに従って個々のワークロードを制限しており、ハード制限が適用されている。 | 2 | D/V |
| **4.7.4** | **検証:** コスト監視はワークロード/テナントごとの支出を追跡しており、組織の予算閾値に基づいてアラートし、予算超過に対して自動制御している。 | 2 | V |
| **4.7.5** | **検証:** キャパシティ計画は組織で定義された予測期間での履歴データを使用しており、需要パターンに基づいて自動リソースプロビジョニングしている。 | 3 | V |
| **4.7.6** | **検証:** リソース枯渇は、キャパシティポリシーに基づくレート制限やワークロード分離など、組織の対応要件に従ってサーキットブレーカーをトリガーしている。 | 2 | D/V |

---

## C4.8 環境分離とプロモーション制御 (Environment Separation & Promotion Controls)

自動プロモーションゲートとセキュリティバリデーションで、厳格な環境境界を適用します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.8.1** | **検証:** 開発/テスト/本番環境は、IAM ロール、セキュリティグループ、ネットワーク接続を共有しない、個別の VPC/VNet で実行している。 | 1 | D/V |
| **4.8.2** | **検証:** 環境プロモーションは組織で認可された担当者から暗号署名と不変な監査証跡での承認を必要としている。 | 1 | D/V |
| **4.8.3** | **検証:** 本番環境は SSH アクセスをブロックし、デバッグエンドポイントを無効にし、緊急時を除き、組織への事前通知要件を伴う変更リクエストを要求している。 | 2 | D/V |
| **4.8.4** | **検証:** Infrastructure as Code の変更は、メインブランチにマージする前に、自動テストとセキュリティスキャンでのピアレビューを必要としている。 | 2 | D/V |
| **4.8.5** | **検証:** 非本番データは組織のプライバシー要件、合成データ生成、または PII 削除が検証された完全なデータマスキングに従って匿名化されている。 | 2 | D/V |
| **4.8.6** | **検証:** プロモーションゲートは自動セキュリティテスト (SAST、DAST、コンテナスキャン) を含み、承認には重大 (CRITICAL) な問題が一切ないことを要求している。 | 2 | D/V |

---

## C4.9 インフラストラクチャのバックアップとリカバリ (Infrastructure Backup & Recovery)

自動バックアップ、テスト済みのリカバリ手順、災害復旧機能を通じて、インフラストラクチャ耐性を確保します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.9.1** | **検証:** インフラストラクチャ構成は、組織のバックアップスケジュールに従って、3-2-1 バックアップ戦略の実装により地理的に分離されたリージョンにバックアップされている。 | 1 | D/V |
| **4.9.2** | **検証:** バックアップシステムは、ランサムウェア保護のために個別のクレデンシャルとエアギャップストレージを備える、分離されたネットワークで実行している。 | 2 | D/V |
| **4.9.3** | **検証:** リカバリ手順は、RTO および RPO ターゲットが組織の要件を満たす、組織のスケジュールに従って自動テストを通じてテストおよび検証されている。 | 2 | V |
| **4.9.4** | **検証:** 災害復旧は、モデルの重みの復元、GPU クラスタの再構築、サービス依存関係のマッピングでの AI 固有のランブックを含んでいる。 | 3 | V |

---

## C4.10 インフラストラクチャのコンプライアンスとガバナンス (Infrastructure Compliance & Governance)

継続的な評価、文書化、自動制御を通じて、規制コンプライアンスを維持します。

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.10.1** | **検証:** インフラストラクチャコンプライアンスは、自動証跡収集で SOC 2、ISO 27001、または FedRAMP のコントロールに対して、組織のスケジュールに従って評価されている。 | 2 | D/V |
| **4.10.2** | **検証:** インフラストラクチャドキュメントは、ネットワークダイアグラム、データフローマップ、脅威モデルを含み、組織の変更管理要件に従って更新されている。 | 2 | V |
| **4.10.3** | **検証:** インフラストラクチャの変更は、リスクの高い変更に対する規制承認ワークフローでの、自動化されたコンプライアンス影響評価を受けている。 | 3 | D/V |

---

## C4.11 AI ハードウェアセキュリティ (AI Hardware Security)

Secure AI-specific hardware components including GPUs, TPUs, and specialized AI accelerators.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.11.1** | **Verify that** AI accelerator firmware (GPU BIOS, TPU firmware) is verified with cryptographic signatures and updated according to organizational patch management timelines. | 2 | D/V |
| **4.11.2** | **Verify that** before workload execution, AI accelerator integrity is validated by hardware attestation using TPM 2.0, Intel TXT, or AMD SVM. | 2 | D/V |
| **4.11.3** | **Verify that** GPU memory is isolated between workloads using SR-IOV, MIG (Multi-Instance GPU), or equivalent hardware partitioning with memory sanitization between jobs. | 2 | D/V |
| **4.11.4** | **Verify that** the AI hardware supply chain includes provenance verification with manufacturer certificates and tamper-evident packaging validation. | 3 | V |
| **4.11.5** | **Verify that** hardware security modules (HSMs) protect AI model weights and cryptographic keys with FIPS 140-2 Level 3 or Common Criteria EAL4+ certification. | 3 | D/V |

---

## C4.12 エッジと分散型の AI インフラストラクチャ (Edge & Distributed AI Infrastructure)

Secure distributed AI deployments including edge computing, federated learning, and multi-site architectures.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.12.1** | **Verify that** edge AI devices authenticate to central infrastructure using mutual TLS with device certificates rotated according to organizational certificate management policy. | 2 | D/V |
| **4.12.2** | **Verify that** edge devices implement secure boot with verified signatures and rollback protection preventing firmware downgrade attacks. | 2 | D/V |
| **4.12.3** | **Verify that** distributed AI coordination uses Byzantine fault-tolerant consensus algorithms with participant validation and malicious node detection. | 3 | D/V |
| **4.12.4** | **Verify that** edge-to-cloud communication includes bandwidth throttling, data compression, and offline operation capabilities with secure local storage. | 3 | D/V |

---

## C4.13 マルチクラウドとハイブリッドインフラストラクチャセキュリティ (Multi-Cloud & Hybrid Infrastructure Security)

Secure AI workloads across multiple cloud providers and hybrid cloud-on-premises deployments.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.13.1** | **Verify that** multi-cloud AI deployments use cloud-agnostic identity federation (OIDC, SAML) with centralized policy management across providers. | 2 | D/V |
| **4.13.2** | **Verify that** cross-cloud data transfer uses end-to-end encryption with customer-managed keys and data residency controls enforced per jurisdiction. | 2 | D/V |
| **4.13.3** | **Verify that** hybrid cloud AI workloads implement consistent security policies across on-premise and cloud environments with unified monitoring and alerting. | 2 | D/V |
| **4.13.4** | **Verify that** cloud vendor lock-in prevention includes portable infrastructure-as-code, standardized APIs, and data export capabilities with format conversion tools. | 3 | V |
| **4.13.5** | **Verify that** multi-cloud cost optimization includes security controls preventing resource sprawl as well as unauthorized cross-cloud data transfer charges. | 3 | V |

---

## C4.14 インフラストラクチャオートメーションと GitOps セキュリティ (Infrastructure Automation & GitOps Security)

Secure infrastructure automation pipelines and GitOps workflows for AI infrastructure management.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.14.1** | **Verify that** GitOps repositories require signed commits with GPG keys and branch protection rules preventing direct pushes to main branches. | 2 | D/V |
| **4.14.2** | **Verify that** infrastructure automation includes drift detection with automatic remediation and rollback capabilities triggered according to organizational response requirements for unauthorized changes. | 2 | D/V |
| **4.14.3** | **Verify that** automated infrastructure provisioning includes security policy validation with deployment blocking for non-compliant configurations. | 2 | D/V |
| **4.14.4** | **Verify that** infrastructure automation secrets are managed through external secret operators (External Secrets Operator, Bank-Vaults) with automatic rotation. | 2 | D/V |
| **4.14.5** | **Verify that** self-healing infrastructure includes security event correlation with automated incident response and stakeholder notification workflows. | 3 | V |

---

## C4.15 量子耐性インフラストラクチャセキュリティ (Quantum-Resistant Infrastructure Security)

Prepare AI infrastructure for quantum computing threats through post-quantum cryptography and quantum-safe protocols.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.15.1** | **Verify that** AI infrastructure implements NIST-approved post-quantum cryptographic algorithms (CRYSTALS-Kyber, CRYSTALS-Dilithium, SPHINCS+) for key exchange and digital signatures. | 3 | D/V |
| **4.15.2** | **Verify that** quantum key distribution (QKD) systems are implemented for high-security AI communications with quantum-safe key management protocols. | 3 | D/V |
| **4.15.3** | **Verify that** cryptographic agility frameworks enable rapid migration to new post-quantum algorithms with automated certificate and key rotation. | 3 | D/V |
| **4.15.4** | **Verify that** quantum threat modeling assesses AI infrastructure vulnerability to quantum attacks with documented migration timelines and risk assessments. | 3 | V |
| **4.15.5** | **Verify that** hybrid classical-quantum cryptographic systems provide defense-in-depth during the quantum transition period with performance monitoring. | 3 | D/V |

---

## C4.16 コンフィデンシャルコンピューティングとセキュアエンクレーブ (Confidential Computing & Secure Enclaves)

Protect AI workloads and model weights using hardware-based trusted execution environments and confidential computing technologies.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.16.1** | **Verify that** sensitive AI models execute within Intel SGX enclaves, AMD SEV-SNP, or ARM TrustZone with encrypted memory and attestation verification. | 3 | D/V |
| **4.16.2** | **Verify that** confidential containers (Kata Containers, gVisor with confidential computing) isolate AI workloads with hardware-enforced memory encryption. | 3 | D/V |
| **4.16.3** | **Verify that** remote attestation validates enclave integrity before loading AI models with cryptographic proof of an execution environment's authenticity. | 3 | D/V |
| **4.16.4** | **Verify that** confidential AI inference services prevent model extraction through encrypted computation with sealed model weights and protected execution. | 3 | D/V |
| **4.16.5** | **Verify that** trusted execution environment orchestration manages secure enclave lifecycle with remote attestation and encrypted communication channels. | 3 | D/V |
| **4.16.6** | **Verify that** secure multi-party computation (SMPC) enables collaborative AI training without exposing individual datasets or model parameters. | 3 | D/V |

---

## C4.17 ゼロ知識インフラストラクチャ (Zero-Knowledge Infrastructure)

Implement zero-knowledge proof systems for privacy-preserving AI verification and authentication without revealing sensitive information.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.17.1** | **Verify that** zero-knowledge proofs (ZK-SNARKs, ZK-STARKs) verify AI model integrity and training provenance without exposing model weights or training data. | 3 | D/V |
| **4.17.2** | **Verify that** ZK-based authentication systems enable privacy-preserving user verification for AI services without revealing identity-related information. | 3 | D/V |
| **4.17.3** | **Verify that** private set intersection (PSI) protocols enable secure data matching for federated AI without exposing individual datasets. | 3 | D/V |
| **4.17.4** | **Verify that** zero-knowledge machine learning (ZKML) systems enable verifiable AI inferences with cryptographic proof of correct computation. | 3 | D/V |
| **4.17.5** | **Verify that** ZK-rollups provide scalable, privacy-preserving AI transaction processing with batch verification and reduced computational overhead. | 3 | D/V |

---

## C4.18 サイドチャネル攻撃の防止 (Side-Channel Attack Prevention)

Protect AI infrastructure from timing, power, electromagnetic, and cache-based side-channel attacks that could leak sensitive information.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.18.1** | **Verify that** AI inference timing is normalized using constant-time algorithms and padding to prevent timing-based model extraction attacks. | 3 | D/V |
| **4.18.2** | **Verify that** power analysis protection includes noise injection, power line filtering, and randomized execution patterns for AI hardware. | 3 | D/V |
| **4.18.3** | **Verify that** cache-based side-channel mitigation uses cache partitioning, randomization, and flush instructions to prevent information leakage. | 3 | D/V |
| **4.18.4** | **Verify that** electromagnetic emanation protection includes shielding, signal filtering, and randomized processing to prevent TEMPEST-style attacks. | 3 | D/V |
| **4.18.5** | **Verify that** microarchitectural side-channel defenses include speculative execution controls and memory access pattern obfuscation. | 3 | D/V |

---

## C4.19 ニューロモルフィックと特殊 AI ハードウェアセキュリティ (Neuromorphic & Specialized AI Hardware Security)

Secure emerging AI hardware architectures including neuromorphic chips, FPGAs, custom ASICs, and optical computing systems.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.19.1** | **Verify that** neuromorphic chip security includes spike pattern encryption, synaptic weight protection, and hardware-based learning rule validation. | 3 | D/V |
| **4.19.2** | **Verify that** FPGA-based AI accelerators implement bitstream encryption, anti-tamper mechanisms, and secure configuration loading with authenticated updates. | 3 | D/V |
| **4.19.3** | **Verify that** custom ASIC security includes on-chip security processors, hardware root of trust, and secure key storage with tamper detection. | 3 | D/V |
| **4.19.4** | **Verify that** optical computing systems implement quantum-safe optical encryption, secure photonic switching, and protected optical signal processing. | 3 | D/V |
| **4.19.5** | **Verify that** hybrid analog-digital AI chips include secure analog computation, protected weight storage, and authenticated analog-to-digital conversion. | 3 | D/V |

---

## C4.20 プライバシー保護コンピューティングインフラストラクチャ (Privacy-Preserving Compute Infrastructure)

Implement infrastructure controls for privacy-preserving computation to protect sensitive data during AI processing and analysis.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.20.1** | **Verify that** homomorphic encryption infrastructure enables encrypted computation on sensitive AI workloads with cryptographic integrity verification and performance monitoring. | 3 | D/V |
| **4.20.2** | **Verify that** private information retrieval systems enable database queries without revealing query patterns with cryptographic protection of access patterns. | 3 | D/V |
| **4.20.3** | **Verify that** secure multi-party computation protocols enable privacy-preserving AI inference without exposing individual inputs or intermediate computations. | 3 | D/V |
| **4.20.4** | **Verify that** privacy-preserving key management includes distributed key generation, threshold cryptography, and secure key rotation with hardware-backed protection. | 3 | D/V |
| **4.20.5** | **Verify that** privacy-preserving compute performance is optimized through batching, caching, and hardware acceleration while maintaining cryptographic security guarantees. | 3 | D/V |

---

## C4.15 エージェントフレームワーククラウド統合セキュリティとハイブリッドデプロイメント (Agent Framework Cloud Integration Security & Hybrid Deployment)

Security controls for cloud-integrated agent frameworks with hybrid on-premises/cloud architectures.

| # | 説明 | レベル | ロール |
|:--------:|--------------------------------------------------------------------------------------------|:---:|:---:|
| **4.15.1** | **Verify that** cloud storage integration uses end-to-end encryption with agent-controlled key management. | 1 | D/V |
| **4.15.2** | **Verify that** hybrid deployment security boundaries are clearly defined with encrypted communication channels. | 2 | D/V |
| **4.15.3** | **Verify that** cloud resource access includes zero-trust verification with continuous authentication. | 2 | D/V |
| **4.15.4** | **Verify that** data residency requirements are enforced by cryptographic attestation of storage locations. | 3 | D/V |
| **4.15.5** | **Verify that** cloud provider security assessments include agent-specific threat modeling and risk evaluation. | 3 | D/V |

---

## 参考情報

* [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
* [CIS Controls v8](https://www.cisecurity.org/controls/v8)
* [OWASP Container Security Verification Standard](https://github.com/OWASP/Container-Security-Verification-Standard)
* [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/)
* [SLSA Supply Chain Security Framework](https://slsa.dev/)
* [NIST SP 800-190: Application Container Security Guide](https://csrc.nist.gov/publications/detail/sp/800-190/final)
* [Cloud Security Alliance: Cloud Controls Matrix](https://cloudsecurityalliance.org/research/cloud-controls-matrix/)
* [ENISA: Secure Infrastructure Design](https://www.enisa.europa.eu/topics/critical-information-infrastructures-and-services)
* [NIST SP 800-53: Security Controls for Federal Information Systems](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
* [ISO 27001:2022 Information Security Management](https://www.iso.org/standard/27001)
* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [CIS Kubernetes Benchmark](https://www.cisecurity.org/benchmark/kubernetes)
* [FIPS 140-2 Security Requirements](https://csrc.nist.gov/publications/detail/fips/140/2/final)
* [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final)
* [IEEE 2857: Privacy Engineering for AI Systems](https://standards.ieee.org/ieee/2857/7273/)
* [NIST SP 800-161: Cybersecurity Supply Chain Risk Management](https://csrc.nist.gov/publications/detail/sp/800-161/rev-1/final)
* [NIST Post-Quantum Cryptography Standards](https://csrc.nist.gov/Projects/post-quantum-cryptography)
* [Intel SGX Developer Guide](https://www.intel.com/content/www/us/en/developer/tools/software-guard-extensions/overview.html)
* [AMD SEV-SNP White Paper](https://www.amd.com/system/files/TechDocs/SEV-SNP-strengthening-vm-isolation-with-integrity-protection-and-more.pdf)
* [ARM TrustZone Technology](https://developer.arm.com/ip-products/security-ip/trustzone)
* [ZK-SNARKs: A Gentle Introduction](https://blog.ethereum.org/2016/12/05/zksnarks-in-a-nutshell/)
* [NIST SP 800-57: Cryptographic Key Management](https://csrc.nist.gov/publications/detail/sp/800-57-part-1/rev-5/final)
* [Side-Channel Attack Countermeasures](https://link.springer.com/book/10.1007/978-3-319-75268-6)
* [Neuromorphic Computing Security Challenges](https://ieeexplore.ieee.org/document/9458103)
* [FPGA Security: Fundamentals, Evaluation, and Countermeasures](https://link.springer.com/book/10.1007/978-3-319-90385-9)
* [Microsoft SEAL: Homomorphic Encryption Library](https://github.com/Microsoft/SEAL)
* [HElib: Homomorphic Encryption Library](https://github.com/homenc/HElib)
* [PALISADE Lattice Cryptography Library](https://palisade-crypto.org/)
* [Differential Privacy: A Survey of Results](https://link.springer.com/chapter/10.1007/978-3-540-79228-4_1)
* [Secure Aggregation for Federated Learning](https://eprint.iacr.org/2017/281.pdf)
* [Private Information Retrieval: Concepts and Constructions](https://link.springer.com/book/10.1007/978-3-030-16234-8)
