# AISVS を使用するには

人工知能セキュリティ検証標準 (AISVS) は現代の AI アプリケーションとサービスのセキュリティ要件を定義しており、アプリケーション開発者が制御できる側面に重点を置いています。

AISVS は、開発者、アーキテクト、セキュリティエンジニア、監査人など、AI アプリケーションのセキュリティを開発したり評価するすべての方を想定しています。この章では、検証レベル、想定されるユースケース、他のセキュリティ標準との位置づけなど、AISVS の構成と使用について紹介します。

## 本標準の読み方

### 章の構成

12 の各要件の章は同じ以下の形式に従います。

* **管理目標。** その章のセキュリティ目標を簡潔に示したものです。
* **セクション。** 要件は関連するセクションにグループ化されており、それぞれには防御目標の簡単な説明があります。
* **要件表。** 個々の要件は以下の列のある表で示されます。

| 列 | 意味 |
| --- | --- |
| **#** | 一意の要件識別子 (例: 1.1.1, 9.3.2) |
| **説明** | 要件の本文。検証可能性を強調するため、常に「検証:」で始まります。 |
| **レベル** | 求められる保証の深さを示す検証レベル (1, 2, 3)。後述の検証レベルを参照してください。 |

### 付録

三つの付録が中核となる要件をサポートします。

* **付録 A (用語集)** 本標準全体を通じて主要な用語や頭字語を定義します。
* **付録 B (AI セキュリティコントロールインベントリ)** AISVS のすべての防御技法のクロスリファレンスであり、セキュリティコントロールカテゴリ (認証、認可、暗号化、入力バリデーションなど) ごとに整理され、特定の要件識別子とマッピングします。
* **付録 C (AI 支援のセキュアコーディング)** ソフトウェア開発時における AI コーディングツールの安全な使用のためのコントロールを提示します。

## 人工知能セキュリティ検証レベル

AISVS は三段階のセキュリティ検証レベルを定義しています。各レベルは深さと複雑さを増していき、組織が AI システムのリスクレベルに合わせてセキュリティ態勢を調整できます。

組織はレベル 1 で開始し、セキュリティ成熟度や脅威への露出増加に応じて、段階的に上位のレベルを採用できます。AISVS レベルは [ASVS](https://owasp.org/www-project-application-security-verification-standard/) レベルと整合しており、対応する ASVS レベルに適用することを意図しています (以下の ASVS レベルとの整合性を参照)。

### レベルの定義

AISVS v1.0 の各要件は以下のいずれかのレベルに割り当てられます。

#### レベル 1 要件

レベル 1 は最も重要で基本的なセキュリティ要件を含みます。これらは他の前提条件や脆弱性に依存しない一般的な攻撃の防止に重点を置いています。レベル 1 コントロールのほとんどは、実装が容易であるか、その労力を正当化するのに十分なほど不可欠です。

#### レベル 2 要件

レベル 2 はより高度な攻撃やあまり一般的でない攻撃、および広範な脅威に対する多層防御に対応します。これらの要件は、より複雑なロジックや、特定のターゲットに対する攻撃を前提条件とすることがあります。

#### レベル 3 要件

レベル 3 は、一般的に実装が困難であったり、状況に応じて適用可能であるコントロールを含みます。これらは、ニッチな攻撃、標的型攻撃、高度に複雑な攻撃に対する多層防御や緩和策を表すことがよくあります。

## ASVS レベルとの整合性

AISVS レベルは [ASVS](https://owasp.org/www-project-application-security-verification-standard/) レベルと整合しています。AI アプリケーションを AISVS レベル _N_ に照らして検証することは、そのアプリケーションが ASVS レベル _N_ でも検証済みであるか、検証中であることを想定しています。その二つの標準は、対応するレベルで共に適用されるように設計されています。

| AISVS レベル | 対応する ASVS レベル | 一般的な用途 |
| :---: | :---: | --- |
| 1 | 1 | 信頼できない入力を処理したり、機密性の高いデータを操作する、あらゆる AI アプリケーションのベースラインセキュリティ。 |
| 2 | 2 | 機密性の高い業務データや規制対象データを扱ったり、敵対的なコンテキストで操作する、AI アプリケーション。 |
| 3 | 3 | 人命の安全性に関わる判断、重要なインフラストラクチャ、極めて機密性の高い個人データを扱うものなど、高保証の AI アプリケーション。 |

AISVS 要件が ASVS 要件と重複するように見える場合でも、AISVS バージョンでのみ言い換えられているのは、AI 特有の実装詳細、攻撃対象領域、あるいは監査人が異なって評価する必要がある証拠を持つためです。

## AISVS のスコープ

AISVS は意図的に絞っています。これは、AI および ML システムに特有のセキュリティ要件、あるいは一般的なセキュリティコントロールだが AI 特有のニュアンスを有するので言い換える必要があるもののみを定義します。これは、AI アプリケーションのための自己完結したセキュリティプログラムではありません。AISVS は、基盤となるアプリケーション、インフラストラクチャ、組織的な慣行が、確立された汎用的な標準に基づいてすでに検証済みであることを前提としており、AI 特有のレイヤーを追加します。

以下は意図的にスコープ外としており、AISVS の章とは重複していません。

* **一般的なアプリケーションセキュリティ。** 認証、セッション管理、認可、トランスポートセキュリティ、非 AI サーフェスの入出力処理、シークレット管理、ファイルアップロード処理、エラー処理、および類するコントロールは [OWASP Application Security Verification Standard (ASVS)](https://owasp.org/www-project-application-security-verification-standard/) で定義されています。
* **一般的なソフトウェアサプライチェーンセキュリティ。** 依存関係スキャン、バージョンピン留め、ロックファイル強制、ビルド来歴、再現可能なビルド、一般的な SBOM 生成、CI/CD パイプライン完全性は、[OWASP Software Component Verification Standard (SCVS)](https://owasp.org/www-project-software-component-verification-standard/), [SLSA](https://slsa.dev/), [CIS Controls](https://www.cisecurity.org/controls) で定義されています。
* **一般的なインフラストラクチャとプラットフォームの堅牢化。** コンテナ、ホスト、ネットワーク、クラウド、Kubernetes についてのベースラインの堅牢化は、[CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks), [NIST SP 800-53](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final), [NIST SP 800-190](https://csrc.nist.gov/pubs/sp/800/190/final), [NIST Cybersecurity Framework (CSF)](https://www.nist.gov/publications/nist-cybersecurity-framework-csf-20) で定義されています。
* **一般的なデータ保護とプライバシーの運用。** データ分類、保存時および転送時の暗号化、保持スケジューリング、従来のストレージの安全な削除、監査ログの不変性、同意管理プラットフォームの運用は、ASVS、[ISO/IEC 27001](https://www.iso.org/standard/27001)、および GDPR などの適用可能なプライバシー規制で定義されています。
* **一般的なログ記録と監視。** ログストレージのアクセス制御、保持、バックアップ、暗号化、改訂、改竄保護、SIEM 統合、運用テレメトリは、ASVS と標準的な可観測性プラクティスで定義されています。
* **AI ガバナンスとリスク管理。** 組織的 AI ガバナンス、AI 影響評価、公平性および倫理の文書化、モデルカード、公開透明性レポート、リスク管理プロセス設計は、[ISO/IEC 42001](https://www.iso.org/standard/81230.html), [ISO/IEC 23894](https://www.iso.org/standard/77304.html), [NIST AI RMF](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-ai-rmf-10) で定義されています。
* **ベンダー固有のガイダンス。** AISVS はベンダー中立です。検証するものを規定しており、どの製品を使用するかではありません。

AI アプリケーションを AISVS に基づいて検証する際には、それらの基盤となる標準を同等のレベルで並行して検証する必要があります。

## Cross-References Inside AISVS

AISVS chapters are organized by control family rather than by attack or component. As a result, defending against a given AI threat usually requires applying requirements from several chapters together. For example, defending against prompt injection in an agentic application combines requirements from C2 (input validation), C7 (model behavior), C9 (orchestration and agentic security), C10 (MCP-specific controls), C11 (adversarial robustness), and C12 (detection and logging).

When applying AISVS, treat the standard as a whole and consult Appendix B (AI Security Controls Inventory) for a cross-cutting view of where each defense technique appears.

## AISVS Requirements and Scope in Assessments

Requirements can often be assessed using a combination of technical testing and vendor documentation, such as model cards for AI models. Another option is to mark requirements outside the organization's control as out of scope.
