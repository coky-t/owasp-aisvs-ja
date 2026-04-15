# C4 インフラストラクチャ、構成、デプロイメントセキュリティ (Infrastructure, Configuration & Deployment Security)

## 管理目標

AI-specific infrastructure components must be hardened against model theft, data leakage, and cross-tenant contamination. This chapter covers AI workload sandboxing, AI accelerator hardware security, and edge/distributed AI deployment security. General infrastructure security controls (container hardening, network segmentation, secrets management, CI/CD pipeline security) are covered by ASVS, CIS Benchmarks, and NIST SP 800-53, and are not repeated here. AI-specific supply chain controls are covered in C6.

---

## C4.1 AI ワークロードのサンドボックス化とバリデーション (AI Workload Sandboxing & Validation)

信頼できない AI モデルを安全なサンドボックスで分離し、高信頼実行環境 (TEE) と機密コンピューティングテクノロジを使用して機密性の高い AI ワークロードを保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------ | :---: |
| **4.1.1** | **検証:** 外部の AI モデルや信頼できない AI モデルは分離されたサンドボックス内で実行している。 | 1 |
| **4.1.2** |**検証:** サンドボックス化されたワークロードはデフォルトで送信ネットワーク接続を持たず、必要なアクセスは明示的に定義されている。 | 1 |
| **4.1.3** | **Verify that** model artifact loading enforces an explicit allowlist of serialization formats that do not permit arbitrary code execution during deserialization, and that formats capable of arbitrary code execution (e.g., Python pickle with unrestricted globals) are rejected by default. | 1 |
| **4.1.4** | **検証:** ワークロードアテステーションはモデルをロードする前に実行されており、実行環境が改竄されていないことを暗号証明で確保している。 | 2 |
| **4.1.5** | **検証:** 機密ワークロードは、ハードウェアによる分離、メモリ暗号化、完全性保護を提供する高信頼実行環境 (TEE) 内で実行している。 | 3 |
| **4.1.6** | **検証:** コンフィデンシャル推論サービスは、シールされたモデルの重みと保護された実行での暗号化された計算を通じて、モデルの抽出を防いでいる。 | 3 |
| **4.1.7** | **検証:** 秘匿マルチパーティ計算 (SMPC) は、個々のデータセットやモデルパラメータを公開することなく、協調 AI トレーニングを可能にしている。 | 3 |

---

## C4.2 AI ハードウェアセキュリティ (AI Hardware Security)

GPU、TPU、特殊な AI アクセラレータなどの AI 固有のハードウェアコンポーネントを保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------ | :---: |
| **4.2.1** | **検証:** ワークロードを実行する前に、AI アクセラレータの完全性はハードウェアベースのアテステーションメカニズム (TPM、DRTM など) を使用して検証されている。 | 2 |
| **4.2.2** | **検証:** アクセラレータ (GPU) メモリは、ジョブ間のメモリサニタイゼーションを備えたパーティショニングメカニズムを通じて、ワークロード間で分離されている。 | 2 |
| **4.2.3** | **検証:** アクセラレータファームウェアは、ブート時にバージョン固定され、署名され、証明されている。署名されていないファームウェアやデバッグファームウェアはブロックされている。 | 2 |
| **4.2.4** | **検証:** VRAM とオンパッケージメモリはジョブ/テナント間でゼロ設定されており、デバイスのリセットポリシーはテナント間のデータ残留を防いでいる。 | 2 |
| **4.2.5** | **検証:** パーティショニング/アイソレーション機能 (MIG/VM パーティショニングなど) はテナントごとに適用され、パーティション間にわたるピアツーピアのメモリアクセスを不正できる。 | 2 |
| **4.2.6** | **検証:** ハードウェアセキュリティモジュール (HSM) または同等の改竄防止ハードウェアは、適切な保証レベル (FIPS 140-3 Level 3 または Common Criteria EAL4+ など) の認定で、AI モデルの重みと暗号鍵を保護している。 | 3 |
| **4.2.7** | **検証:** アクセラレータ相互接続 (NVLink/PCIe/InfiniBand/RDMA/NCCL) は承認されたトポロジーと認証されたエンドポイントに制限されており、プレーンテキストのテナント間リンクは許可されていない。 | 3 |
| **4.2.8** | **検証:** アクセラレータテレメトリ (電力消費、温度、エラー訂正、パフォーマンスカウンタ) は一元化されたセキュリティ監視にエクスポートされ、サイドチャネルや隠れチャネルを示す異常についてアラートしている。 | 3 |

---

## C4.3 エッジと分散型の AI セキュリティ (Edge & Distributed AI Security)

エッジコンピューティング、連合学習、マルチサイトアーキテクチャなどの分散 AI デプロイメントを保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------ | :---: |
| **4.3.1** | **検証:** エッジ AI デバイスは証明書バリデーションによる相互認証 (相互 TLS など) を使用して中央インフラストラクチャに対して認証している。 | 1 |
| **4.3.2** | **検証:** エッジデバイスまたはモバイルデバイスにデプロイされたモデルはパッケージ化時に暗号署名され、デバイス上のランタイムがロードまたは推論する前にこれらの署名またはチェックサムを検証している。検証されていないモデルや改変されたモデルは拒否される必要がある。 | 1 |
| **4.3.3** | **検証:** 分散型 AI 協調は、協力者バリデーションと悪意のあるノードの検出を備える、ビザンチンフォールトトレラントコンセンサスメカニズムを使用している。 | 3 |
| **4.3.4** | **検証:** デバイス上の推論ランタイムは、モデルのダンプ、デバッグ、中間エンベディングとアクティベーションの抽出を防ぐために、プロセス、メモリ、ファイルアクセスの分離を強制している。 | 3 |
| **4.3.5** | **検証:** ローカルに保存されるモデルの重みと機密パラメータは、ユーザースペースからアクセスできない鍵で、ハードウェア基盤のキーストアまたはセキュアエンクレーブ (Android Keystore, iOS Secure Enclave, TPM/TEE など) を使用して暗号化されている。 | 3 |
| **4.3.6** | **検証:** モバイル、IoT、組み込みアプリケーション内にパッケージ化されたモデルは保存時に暗号化または難読化され、信頼できるランタイムまたはセキュアエンクレーブ内でのみ復号されており、アプリパッケージやファイルシステムからの直接抽出を防いでいる。 | 3 |

---

## 参考情報

* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [NVIDIA Multi-Instance GPU (MIG) Documentation](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/)
* [Confidential Computing Consortium](https://confidentialcomputing.io/)
* [ARM TrustZone for AI](https://www.arm.com/technologies/trustzone-for-cortex-a)
