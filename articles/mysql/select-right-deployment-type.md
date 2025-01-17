---
title: 適切なデプロイの種類の選択 - Azure Database for MySQL
description: この記事では、Azure Database for MySQL をサービスとしてのインフラストラクチャ (IaaS) またはサービスとしてのプラットフォーム (PaaS) としてデプロイする前に考慮すべき要素について説明します。
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 08/26/2020
ms.openlocfilehash: 03799c27173a06ba1ffb25ba117750e65bca6b31
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2021
ms.locfileid: "131842513"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Azure で適切な MySQL サーバー オプションを選択する

[!INCLUDE[applies-to-mysql-single-flexible-server](includes/applies-to-mysql-single-flexible-server.md)]

Azure では、MySQL サーバーのワークロードをホスト型仮想マシンのサービスとしてのインフラストラクチャ (IaaS) で実行することも、ホスト型のサービスとしてのプラットフォーム (PaaS) として実行することもできます。 PaaS には 2 つのデプロイ オプションがあり、各デプロイ オプション内にサービス レベルがあります。 IaaS か PaaS かを選択する際には、データベースの管理、修正プログラム、バックアップ、セキュリティ、監視、スケーリングの適用を自分で行うか、これらの操作を Azure に委任するかを決定する必要があります。

決定する際に、次の 2 つのオプションを検討します。

- **Azure Database for MySQL**。 このオプションは、MySQL コミュニティ エディションの安定バージョンに基づくフル マネージドの MySQL データベース エンジンです。 このサービスとしてのリレーショナル データベース (DBaaS) は、Azure クラウド プラットフォームでホストされ、業界内のカテゴリとしては PaaS に分類されます。 Azure 上の MySQL のマネージド インスタンスでは、MySQL サーバーがオンプレミスまたは Azure VM に存在する場合には大がかりな構成が必要となるような組み込みの機能、すなわち、パッチの自動適用、高可用性、自動バックアップ、エラスティック スケーリング、エンタープライズ グレードのセキュリティ、コンプライアンスとガバナンス、監視、アラートなどを使用できます。 MySQL をサービスとして使用する場合は、従量課金制になり、中断することなく制御の強化のためにスケールアップまたはスケールアウトするオプションが用意されています。 MySQL コミュニティ エディションを搭載した [Azure Database for MySQL](overview.md) は、次の 2 つのデプロイ モードで利用できます。

   - [フレキシブル サーバー](flexible-server/overview.md) - Azure Database for MySQL フレキシブル サーバーは、データベース管理機能と構成設定のよりきめ細かな制御と柔軟性のために設計された、運用環境対応のフル マネージド データベース サービスです。 フレキシブル サーバー アーキテクチャにより、ユーザーは単一の可用性ゾーン内および複数の可用性ゾーンにまたがる高可用性を選択できます。 また、フレキシブル サーバーでは、より優れたコスト最適化制御によって、サーバーを停止/起動する機能や、完全なコンピューティング能力を継続的には必要としないワークロードに最適な、バースト可能なコンピューティング層を実現できます。 フレキシブル サーバーでは、予約インスタンスもサポートされ、最大 63% のコストを削減でき、コンピューティング容量要件が予測できる運用環境ワークロードに最適です。 サービスでは、MySQL 5.7 と 8.0 のコミュニティ バージョンがサポートされています。 このサービスは現時点で一般提供されており、さまざまな [Azure リージョン](flexible-server/overview.md#azure-regions)で利用できます。 フレキシブル サーバーは、運用環境ワークロードのすべての新規開発と Azure Database for MySQL サービスへの移行に最適です。

   - [単一サーバー](single-server-overview.md)は、最小限のカスタマイズ用に設計されたフル マネージド データベース サービスです。 単一サーバー プラットフォームは、修正プログラムの適用、バックアップ、高可用性、最小限のユーザー構成と制御によるセキュリティなど、データベース管理機能のほとんどを処理するよう設計されています。 このアーキテクチャは、単一の可用性ゾーンで 99.99% の可用性を備えた組み込みの高可用性を実現するよう最適化されています。 MySQL 5.6 (廃止)、5.7、8.0 のコミュニティ バージョンをサポートしています。 このサービスは現時点で一般提供されており、さまざまな [Azure リージョン](https://azure.microsoft.com/global-infrastructure/services/)で利用できます。 単一サーバーは、**既に単一サーバーを活用している既存のアプリケーションにのみ** 最適です。 新たに開発または移行する場合は、デプロイ オプションとしてフレキシブル サーバーをお勧めします。 フレキシブル サーバーと単一サーバーのデプロイ オプションの違いについては、[最適なデプロイ オプションを選択する](select-right-deployment-type.md)方法に関するドキュメントを参照してください。
 
- **Azure VM 上の MySQL**。 このオプションは、IaaS の業界カテゴリに分類されます。 このサービスを使用すると、Azure クラウド プラット フォーム上の管理された仮想マシン内で MySQL サーバーを実行できます。 MySQL の最近のバージョンとエディションはすべて、仮想マシンにインストールできます。

## <a name="comparing-the-mysql-deployment-options-in-azure"></a>Azure の MySQL デプロイ オプションの比較

これらのオプションの主な違いを次の表に示します。

| 属性          | Azure Database for MySQL<br/>シングル サーバー |Azure Database for MySQL<br/>フレキシブル サーバー  |Azure VM 上の MySQL |
|:-------------------|:-------------------------------------------|:---------------------------------------------|:------------------|
| [**全般**](flexible-server/overview.md)  | | | |
| 一般公開 | 一般公開 | 一般公開 | 一般公開 |
| サービス レベル アグリーメント (SLA) | 99.99% の可用性の SLA |可用性ゾーンの使用時に 99.99%| 可用性ゾーンの使用時に 99.99%|
| 基盤 O/S | Windows | Linux  | ユーザー管理 |
| MySQL のエディション | Community Edition | Community Edition | Community または Enterprise Edition |
| MySQL バージョンのサポート | 5.6 (廃止)、5.7、および 8.0| 5.7、8.0 | 任意のバージョン|
| アプリケーション コロケーションのための可用性ゾーンの選択 | いいえ | はい | はい |
| 接続文字列のユーザー名 | `<user_name>@server_name`. たとえば、`mysqlusr@mypgServer` のように指定します。 | ユーザー名のみ。 たとえば、`mysqlusr` のように指定します。 | ユーザー名のみ。 たとえば、`mysqlusr` のように指定します。 | 
| [**コンピューティングおよびストレージのスケーリング**](flexible-server/concepts-compute-storage.md) | | | |
| コンピューティング レベル | Basic、General Purpose、Memory Optimized | Burstable、General Purpose、Memory Optimized | Burstable、General Purpose、Memory Optimized |
| コンピューティングのスケーリング | サポートされています (Basic レベルとの間のスケーリングは **サポートされていません**)| サポートされています | サポートされています|
| ストレージ サイズ | 5 GiB ～ 16 TiB| 20 GiB ｰ 16 TiB | 32 GiB ～ 32,767 GiB|
| オンライン ストレージのスケーリング | サポートされています| サポートされています| サポートされていません|
| ストレージの自動スケーリング | サポートされています| サポートされています| サポートされていません|
| IOPS のスケーリング | サポートされていません| サポートされています| サポートされていません|
| [**コストの最適化**](https://azure.microsoft.com/pricing/details/mysql/flexible-server/) | | | |
| 予約インスタンスの価格 | サポートされています | サポートされています | サポートされています |
| 開発用サーバーの停止および開始 | サーバーは最大 7 日間停止できます | サーバーは最大 30 日間停止できます | サポートされています |
| 低コストのバースト可能 SKU | サポートされていません | サポートされています | サポートされています |
| [**ネットワーク/セキュリティ**](concepts-security.md) | | | |
| ネットワーク接続 | - サーバー ファイアウォールがあるパブリック エンドポイント。<br/> - Private Link がサポートされているプライベート アクセス。|- サーバー ファイアウォールがあるパブリック エンドポイント。<br/> - Virtual Network 統合を使用したプライベート アクセス。| - サーバー ファイアウォールがあるパブリック エンドポイント。<br/> - Private Link がサポートされているプライベート アクセス。|
| SSL/TLS | 既定で有効になり、TLS v1.2、1.1、および 1.0 がサポートされます | 既定で有効になり、TLS v1.2、1.1、および 1.0 がサポートされます| TLS v1.2、1.1、および 1.0 でサポートされています |
| 保存データの暗号化 | カスタマー マネージド キーを使用してサポートされます (BYOK) | サービス マネージド キーを使用してサポートされます | サポートされていません|
| Azure AD Authentication | サポートされています | サポートされていません | サポートされていません|
| Azure Defender のサポート | はい | いいえ | いいえ |
| [Server Audit] | サポートされています | サポートされています | ユーザー管理 |
| [**修正プログラムの適用とメンテナンス**](flexible-server/concepts-maintenance.md) | | |
| オペレーティング システムの修正プログラムの適用| 自動  | 自動  | ユーザー管理 |
| MySQL のマイナー バージョンのアップグレード  | 自動  | 自動 | ユーザー管理 |
| MySQL のメジャー バージョンのインプレース アップグレード | 5\.6 から 5.7 までサポートされます | サポートされていません | ユーザー管理 |
| メンテナンス管理 | システム管理 | お客様による管理 | ユーザー管理 |
| メンテナンス期間 | 15 時間以内の任意の時間 | 1 時間の期間 | ユーザー管理 |
| 計画メンテナンスの通知 | 3 日 | 5 日 | ユーザー管理 |
| [**高可用性**](flexible-server/concepts-high-availability.md) | | | |
| 高可用性 | 組み込みの HA (ホット スタンバイなし)| 組み込みの HA (ホット スタンバイなし)、ホット スタンバイを使用した同じゾーンとゾーン冗長 HA | ユーザー管理 |
| ゾーン冗長性 | サポートされていません | サポートされています | サポートされています|
| スタンバイ ゾーンの配置 | サポートされていません | サポートされています | サポートされています|
| 自動フェールオーバー | はい (別のサーバーをスピン)| はい | ユーザー管理|
| ユーザーが開始した強制フェールオーバー | いいえ | はい | ユーザー管理 |
| アプリケーションの透過的なフェールオーバー | はい | はい | ユーザー管理|
| [**レプリケーション**](flexible-server/concepts-read-replicas.md) | | | |
| 読み取りレプリカのサポート | はい | はい | ユーザー管理 |
| サポートされる読み取りレプリカの数 | 5 | 10 | ユーザー管理 |
| レプリケーションのモード | 非同期 | 非同期 | ユーザー管理 |
| 読み取りレプリカでの GTID のサポート | サポートされています | サポートされています | ユーザー管理 |
| リージョン間のサポート (geo レプリケーション) | はい | サポートなし | ユーザー管理 |
| ハイブリッド シナリオ | [データイン レプリケーション](./concepts-data-in-replication.md)を使用してサポートされます| [データイン レプリケーション](./flexible-server/concepts-data-in-replication.md)を使用してサポートされます | ユーザー管理 |
| 受信データのレプリケーションでの GTID のサポート | サポートされています | サポートされています | ユーザー管理 |
| 送信データのレプリケーション | サポートされていません | プレビュー段階 | サポートされています |
| [**バックアップと回復**](flexible-server/concepts-backup-restore.md) | | | |
| 自動バックアップ | はい | はい | いいえ |
| バックアップ保持期間 | 7 から 35 日 | 1 から 35 日 | ユーザー管理 |
| バックアップの長期的な保有期間 | ユーザー管理 | ユーザー管理 | ユーザー管理 |
| バックアップのエクスポート | 論理バックアップを使用してサポートされます | 論理バックアップを使用してサポートされます | サポートされています |
| 保有期間内の任意の時点へのポイントインタイム リストア機能 | はい | はい | ユーザー管理 |
| 高速復元ポイント | いいえ | はい | いいえ |
| 別のゾーンで復元する機能 | サポートされていません | はい | はい |
| 別の VNET に復元する機能 | いいえ | はい | はい |
| 別のリージョンに復元する機能 | はい (geo 冗長) | いいえ | ユーザー管理 |
| 削除されたサーバーを復元する機能 | はい | いいえ | いいえ |
| [**ディザスター リカバリー**](flexible-server/concepts-business-continuity.md) | | | | 
| Azure リージョン間での DR | リージョン間での読み取りレプリカの使用、geo 冗長バックアップ | サポートされていません | ユーザー管理 |
| 自動フェールオーバー | いいえ | サポートされていません | いいえ |
| 同じ r/w エンドポイントを使用できる | いいえ | サポートされていません | いいえ |
| [**監視**](flexible-server/concepts-monitoring.md) | | | |
| Azure Monitor の統合およびアラート | サポートされています | サポートされています | ユーザー管理 |
| データベース操作の監視 | サポートされています | サポートされています | ユーザー管理 |
| Query Performance Insights | サポートされています | サポートされています (Workbooks を使用)| ユーザー管理 |
| サーバー ログ | サポートされています | サポートされています (診断ログを使用) | ユーザー管理 |
| [監査ログ] | サポートされています | サポートされています | サポートされています | 
| エラー ログ | サポートされていません | サポートされています | サポートされています |
| Azure Advisor のサポート | サポートされています | サポートされていません | サポートされていません |
| **プラグイン** | | | |
| validate_password | サポートされていません | プレビュー段階 | サポートされています |
| caching_sha2_password | サポートされていません | プレビュー段階 | サポートされています |
| [**開発者の生産性**](flexible-server/quickstart-create-server-cli.md) | | | |
| フリート管理 | Azure CLI、PowerShell、REST、Azure Resource Manager でサポートされています | Azure CLI、PowerShell、REST、Azure Resource Manager でサポートされています  | Azure CLI、PowerShell、REST、Azure Resource Manager を使用して VM でサポートされています |
| Terraform のサポート | サポートされています | サポートされています | サポートされています |
| GitHub のアクション | サポートされています | サポートされています | ユーザー管理 |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>PaaS と IaaS のいずれかを選択するときのビジネスの要因

MySQL データベースをホストするために PaaS と IaaS のどちらを選択するかの決定に影響する可能性のある要素がいくつかあります。

### <a name="cost"></a>コスト

データベースをホストするための最適なソリューションを決定する主な考慮事項は、多くの場合、コストの削減です。 これは、資金が少ないスタートアップ企業であるか、厳しい予算の制約下で運用している老舗企業内のチームであるかに関係なく当てはまります。 このセクションでは、Azure Database for MySQL と Azure VM 上の MySQL に適用される Azure の課金とライセンスの基礎について説明します。

#### <a name="billing"></a>課金

Azure Database for MySQL は、リソースの料金が異なる複数のサービス レベルでサービスとして利用できます。 すべてのリソースは、固定レートで時間単位で課金されます。 現在サポートされているサービス レベル、コンピューティング サイズ、ストレージ容量の最新情報については、[料金のページ](https://azure.microsoft.com/pricing/details/mysql/)を参照してください。 サービス レベルとコンピューティング サイズを動的に調整して、アプリケーションのさまざまなスループット ニーズを満たすことができます。 インターネット トラフィックの送信に対しては、通常の[データ転送料金](https://azure.microsoft.com/pricing/details/data-transfers/)で課金されます。

Azure Database for MySQL では、Microsoft がデータベース ソフトウェアの構成、修正プログラムの適用、およびアップグレードを行います。 これらの自動化されたアクションにより、管理コストが削減されます。 また、Azure Database for MySQL には [自動化されたバックアップ](./concepts-backup.md)機能があります。 こうした機能は、特に多数のデータベースがある場合の大幅なコスト削減に役立ちます。 対照的に、Azure VM 上の MySQL では、任意の MySQL バージョンを選択して実行できます。 どの MySQL バージョンを使用するかを問わず、プロビジョニングされた VM、データに関連するストレージ コスト、バックアップ、監視データ、ログ ストレージ、および使用される特定の MySQL ライセンスの種類 (存在する場合) について支払いをします。

Azure Database for MySQL は、あらゆる種類のノード レベルの中断に対して組み込みの高可用性を提供し、サービスに対する 99.99% の SLA 保証を維持します。 ただし、VM 内でデータベースの高可用性を実現するには、MySQL データベースで利用できる [MySQL レプリケーション](https://dev.mysql.com/doc/refman/8.0/en/replication.html)などの高可用性オプションを使用します。 サポートされている高可用性オプションを使用しても、追加の SLA は提供されません。 ただし、追加コストと管理オーバーヘッドで 99.99% を超えるデータベース可用性を実現できます。

価格の詳細については、次の記事を参照してください。

- [Azure Database for MySQL の価格](https://azure.microsoft.com/pricing/details/mysql/)
- [仮想マシンの価格](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure 料金計算ツール](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>管理

多くの企業にとって、クラウド サービスへの移行の決定には、コストだけでなく、管理の複雑さの軽減も重要な要素です。

IaaS の場合、Microsoft では次のことを行います。

- 基になるインフラストラクチャを管理します。
- 基になるハードウェアと OS に対して自動化されたパッチ適用を提供します。

PaaS の利用時には、Microsoft が以下のことを行います。

- 基になるインフラストラクチャを管理します。
- 基になるハードウェア、OS、およびデータベース エンジンに対して修正プログラムを自動的に適用します。
- データベースの高可用性を管理します。
- 自動的にバックアップを実行してすべてのデータをレプリケートし、ディザスター リカバリーを提供します。
- 既定で保存データと移動中のデータを暗号化します。
- サーバーを監視して、クエリ パフォーマンスの分析情報とパフォーマンスに関する推奨事項についての機能を提供します。

各オプションの管理上の考慮事項について、次の一覧で説明します。

- Azure Database for MySQL では、データベースを引き続き管理できます。 しかしながら、データベース エンジン、オペレーティング システム、ハードウェアを管理する必要はなくなります。 引き続き管理できる項目の例を次に示します。

  - データベース
  - サインイン
  - インデックスのチューニング
  - クエリのチューニング
  - 監査
  - セキュリティ

  また、別のデータ センターに高可用性を構成するために必要な構成や管理は、最小限で済むか、まったく行わないで済みます。

- Azure VM 上の MySQL では、オペレーティング システムと MySQL サーバー インスタンスの構成を全面的に制御できます。 VM 利用時は、オペレーティング システムとデータベース ソフトウェアの更新またはアップグレードをいつ行うかと、どのパッチを適用するかを決定します。 また、ウイルス対策アプリケーションなどの追加ソフトウェアをインストールするタイミングも決定します。 修正プログラムの適用、バックアップ、高可用性の実現を大幅に簡素化するために、自動化された機能がいくつか用意されています。 VM のサイズ、ディスクの数、ストレージの構成を制御できます。 詳細については、[Azure の仮想マシンおよびクラウド サービスのサイズ](../virtual-machines/sizes.md)に関する記事を参照してください。

### <a name="time-to-move-to-azure"></a>Azure へ移行するタイミング

- Azure Database for MySQL は、開発者の生産性と新しいソリューションの製品化に要する時間の短縮が重要な場合において、クラウド用に設計されたアプリケーションに最適なソリューションです。 このサービスは、DBA のようなプログラムによる機能を備えることで、基になるオペレーティング システムとデータベースを管理する必要性が減少するため、クラウドの設計者と開発者に適しています。

- 新しいオンプレミス ハードウェアの購入に伴う時間とコストを回避したい場合、サービスではサポートされない MySQL エンジンのきめ細かな制御とカスタマイズが必要なアプリケーションや、基になっている OS のアクセスを必要とするアプリケーションには、Azure VM 上の MySQL が適切なソリューションです。 このソリューションは、既存のオンプレミス アプリケーションとデータベースを Azure にそのまま移行する場合、Azure Database for MySQL が適さない場合にも適しています。

プレゼンテーション層、アプリケーション層、データ層を変更する必要がないため、既存のソリューションを再設計する時間と予算が節約されます。 その代わりに、すべてのソリューションを Azure に移行することと、Azure プラットフォームで必要となる可能性のあるパフォーマンス最適化に取り組むことに集中できます。

## <a name="next-steps"></a>次のステップ

- 「[Azure Database for MySQL の価格](https://azure.microsoft.com/pricing/details/MySQL/)」をご確認ください。
- [初めてのサーバーを作成](./quickstart-create-mysql-server-database-using-azure-portal.md)してみましょう。
