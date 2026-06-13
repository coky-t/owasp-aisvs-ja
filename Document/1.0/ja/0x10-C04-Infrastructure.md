# C4 インフラストラクチャ、構成、デプロイメントセキュリティ (Infrastructure, Configuration & Deployment Security)

## 管理目標

AI-specific infrastructure components must be hardened against model theft, data leakage, and cross-tenant contamination. This chapter covers AI workload sandboxing, AI accelerator hardware security, and edge/distributed AI deployment security.

---

## C4.1 AI ワークロードのサンドボックス化とバリデーション (AI Workload Sandboxing & Validation)

信頼できない AI モデルを安全なサンドボックスで分離し、高信頼実行環境 (TEE) と機密コンピューティングテクノロジを使用して機密性の高い AI ワークロードを保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------ | :---: |
| **4.1.1** | **検証:** AI モデルは分離されたサンドボックス内で実行している。 | 1 |
| **4.1.2** | **Verify that** model artifact loading enforces an explicit allowlist of serialization formats that do not permit arbitrary code execution during deserialization.| 1 |
| **4.1.3** | **検証:** ワークロードアテステーションはモデルをロードする前に実行されており、そのアテステーションは実行環境が改竄されていないことを証明で提供している。 | 2 |
| **4.1.4** | **検証:** コンフィデンシャル推論サービスは分離された実行環境を通じて実行時にモデルの重みを保護している。 | 3 |

---

## C4.2 AI ハードウェアセキュリティ (AI Hardware Security)

GPU、TPU、特殊な AI アクセラレータなどの AI 固有のハードウェアコンポーネントを保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------ | :---: |
| **4.2.1** | **Verify that** execution within a trusted execution environment (TEE) provides hardware-enforced isolation, memory encryption, and integrity protection. | 3 |
| **4.2.2** | **Verify that** AI accelerator (GPU) integrity is validated using hardware-based attestation mechanisms before each workload executes. | 3 |
| **4.2.3** | **Verify that** accelerator (GPU) memory is isolated between workloads through partitioning mechanisms with memory sanitization between jobs. | 3 |
| **4.2.4** | **Verify that** AI accelerator (GPU) firmware is version-pinned, signed, and attested at boot. | 2 |
| **4.2.5** | **検証:** アクセラレータ相互接続は承認されたトポロジーと認証されたエンドポイントに制限されている。 | 3 |

---

## C4.3 エッジと分散型の AI セキュリティ (Edge & Distributed AI Security)

エッジコンピューティング、連合学習、マルチサイトアーキテクチャなどの分散 AI デプロイメントを保護します。

| # | 説明 | レベル |
| :--------: | ------------------------------------------------------------------------------------------ | :---: |
| **4.3.1** | **検証:** エッジ AI デバイスは強力な認証を使用して中央インフラストラクチャに対して認証している。 | 1 |
| **4.3.2** | **検証:** エッジデバイスまたはモバイルデバイスにデプロイされたモデルはパッケージ化時に暗号署名され、デバイス上のランタイムがロードまたは推論する前にこれらの署名またはチェックサムを検証している。 | 2 |
| **4.3.3** | **検証:** 推論ランタイムは、プロセス、メモリ、ファイルアクセスの分離を強制している。 | 3 |
| **4.3.4** | **検証:** ローカルに保存されるモデルの重みと機密パラメータは、ハードウェア基盤のキーストアまたはセキュアエンクレーブを使用して暗号化されている。 | 3 |
| **4.3.5** | **検証:** モバイル、IoT、組み込みアプリケーション内にパッケージ化されたモデルは保存時に暗号化され、信頼できるランタイムまたはセキュアエンクレーブ内でのみ復号されており、アプリパッケージやファイルシステムからの直接抽出を防いでいる。 | 3 |

---

## 参考情報

* [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
* [NVIDIA Multi-Instance GPU (MIG) Documentation](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/)
* [Confidential Computing Consortium](https://confidentialcomputing.io/)
* [ARM TrustZone for AI](https://www.arm.com/technologies/trustzone-for-cortex-a)
