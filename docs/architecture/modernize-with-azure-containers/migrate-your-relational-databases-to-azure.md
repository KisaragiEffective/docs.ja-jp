---
title: リレーショナル データベースを Azure に移行する
description: Azure Cloud および Windows コンテナーを使用して既存の .NET アプリケーションを最新化する | リレーショナル データベースを Azure に移行する
ms.date: 04/28/2018
ms.openlocfilehash: efd1548c3f74fc27450f4949d71a1c4d61907ba5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73093615"
---
# <a name="migrate-your-relational-databases-to-azure"></a>リレーショナル データベースを Azure に移行する

展望:Azure には、最も包括的なデータベース移行が用意されています。

Azure では、データベース サーバーを IaaS VM に直接移行 (純粋なリフト アンド シフト) することも、Azure SQL Database に移行して追加のベネフィットを得ることもできます。 Azure SQL Database には、マネージド インスタンスと完全なサービスとしてのデータベース (DBaaS) オプションが用意されています。 図 3-1 は、Azure で使用できる複数のリレーショナル データベース移行パスを示しています。

![Azure のデータベース移行パス](./media/image3-1.png)

**図 3-1.** Azure のデータベース移行パス

## <a name="when-to-migrate-to-azure-sql-database-managed-instance"></a>Azure SQL Database Managed Instance に移行するタイミング

データを Azure に移行する際には、ほとんどの場合、Azure SQL Database Managed Instance が最適な選択肢となります。 SQL Server データベースを移行するときに、アプリケーションを再設計したり、データやデータ アクセス コードを変更したりする必要がないことをほぼ 100% 保証する必要がある場合は、Azure SQL Database の Managed Instance 機能を選択します。

Azure SQL Database Managed Instance は、SQL Server インスタンスレベルの機能に対する追加の要件や、標準の Azure SQL Database (単一データベース モデル) で提供される機能を超える分離要件がある場合に最適なオプションです。 最後の 1 つは、最も PaaS 指向の選択肢ですが、従来の SQL server と同じ機能を提供するわけではありません。 移行により摩擦が表面化する可能性があります。

たとえば、インスタンスレベルの SQL Server 機能に対して多額の投資を行ってきた組織は、SQL Managed Instance に移行することでベネフィットが得られる可能性があります。 インスタンスレベルの SQL Server 機能の例としては、SQL 共通言語ランタイム (CLR) の統合、SQL Server エージェント、データベース間クエリなどがあります。 これらの機能のサポートは、標準の Azure SQL Database (単一データベース モデル) では使用できません。

高度に規制された業界で事業を行っていて、セキュリティのために分離を維持する必要がある組織も、SQL Managed Instance モデルを選択することによってベネフィットが得られる可能性があります。

Azure SQL Database の Managed Instance には、次の特性があります。

- Azure Virtual Network を使用したセキュリティの分離

- 次の機能を使用した、アプリケーション サーフェスの互換性:

  - SQL Server エージェントと SQL Server プロファイラー

  - データベース間参照とクエリ、SQL CLR、レプリケーション、変更データ キャプチャ (CDC)、および Service Broker

- 最大 35 TB のデータベース サイズ

- 次の機能を使用した、最小限のダウンタイムでの移行:

  - Azure Database Migration Service

  - ネイティブのバックアップと復元、およびログ配布

これらの機能を使用すると、既存のアプリケーション データベースを Azure SQL Database に移行するときに、Managed Instance モデルによって PaaS のベネフィットのほぼ 100% が SQL Server に提供されます。 Managed Instance は SQL Server 環境なので、アプリケーションの設計を変更することなく、インスタンスレベルの機能を引き続き使用できます。

Managed Instance は、現在 SQL Server を使用していて、クラウドでのネットワーク セキュリティに柔軟性を必要とする企業にとっておそらく最適な選択肢です。 これは、SQL データベース用のプライベート仮想ネットワークを持つのと似ています。

## <a name="when-to-migrate-to-azure-sql-database"></a>Azure SQL Database に移行するタイミング

前述のように、標準の Azure SQL Database はフル マネージドのリレーショナル DBaaS です。 SQL Database では現在、世界中の 38 のデータセンターで、数百万の実稼働データベースが管理されています。 単純なトランザクション データ処理アプリケーションの管理から、世界規模の高度なデータ処理を必要とするデータ集約型のミッション クリティカルなアプリケーションの駆動まで、幅広いアプリケーションとワークロードをサポートしています。

その PaaS の完全な機能により、基本的な標準の SQL データベースを使用し、追加のインスタンス機能を使用しないアプリケーションがある場合は、"既定の選択肢" として標準の Azure SQL Database に移行することで、より良い価格設定となり、最終的にはコストが削減されます。 SQL CLR 統合、SQL Server エージェント、データベース間クエリなどの SQL Server 機能は、標準の Azure SQL Database ではサポートされていません。 これらの機能は、Azure SQL Database Managed Instance モデルでのみ使用できます。

Azure SQL Database は、アプリ開発者向けに構築された唯一のインテリジェントなクラウド データベース サービスです。 また、ダウンタイムを発生させることなく、すぐにスケーリングできる唯一のクラウド データベース サービスでもあるため、マルチテナント アプリを効率的に配信するのに役立ちます。 最終的に、Azure SQL Database によって、イノベーションにより多くの時間がかけられるようになり、市場投入までの時間が短縮されます。 お好みの言語とプラットフォームを使用して、セキュリティで保護されたアプリをビルドして SQL データベースに接続できます。

Azure SQL Database を使用することで、次のベネフィットが得られます。

- 学習してアプリに適応する組み込みのインテリジェンス (機械学習)

- オンデマンドのデータベース プロビジョニング

- あらゆるワークロードに対応する幅広いオファー

- 99.99% の可用性 SLA、メンテナンス不要

- データ保護のための geo レプリケーションと復元サービス

- Azure SQL Database のポイント イン タイム リストア機能

- ハイブリッドおよび移行を含む SQL Server 2016 との互換性

標準の Azure SQL Database は、Azure SQL Database Managed Instance よりも PaaS に近いです。 マネージド クラウドからより多くのベネフィットを得られるため、標準の Azure SQL Database をお勧めします。 ただし、Azure SQL Database には、通常およびオンプレミスの SQL Server インスタンスとの主な違いがいくつかあります。 既存のアプリケーションのデータベース要件、および自社の要件とポリシーによっては、クラウドへの移行を計画するときに、これが最適な選択肢ではない場合があります。

## <a name="when-to-move-your-original-rdbms-to-a-vm-iaas"></a>元の RDBMS を VM (IaaS) に移行するタイミング

移行オプションの 1 つは、Oracle、IBM DB2、MySQL、PostgreSQL、SQL Server などの元のリレーショナル データベース管理システム (RDBMS) を、Azure VM 上で実行されている同様のサーバーに移動することです。 最小限の変更、またはまったく変更を加えずに、クラウドへの移行を最速で行う必要がある既存のアプリケーションがある場合、クラウドの IaaS に直接移行することが正しい選択肢となる場合があります。 クラウドのベネフィットをすべて活用するのが最善の方法ではない場合もありますが、最初はそれが最短の近道となるでしょう。

現在、Microsoft Azure では、IaaS VM としてデプロイされる、最大 [331 台の異なるデータベース サーバー](https://azuremarketplace.microsoft.com/marketplace/apps/category/databases?page=1&subcategories=databases-all)がサポートされています。 これには、SQL Server、Oracle、MySQL、PostgreSQL、IBM DB2 などの一般的な RDBMS や、MongoDB、Cassandra、DataNoSQL x、MariaDB、Cloudera などの SQL 以外の他の多くのデータベースが含まれます。

> [!NOTE]
> データをクラウドに移行する最速の方法は、RDBMS を Azure VM に移動することかもしれませんが (IaaS であるため)、この方法では、IT チーム (データベース管理者および IT プロフェッショナル) に多大な投資を行う必要があります。 エンタープライズ チームは、SQL Server の高可用性、ディザスター リカバリー、修正プログラムの適用を設定して管理できる必要があります。 この場合は、完全な管理者アクセス権がある、カスタマイズされた環境も必要です。

## <a name="when-to-migrate-to-sql-server-as-a-vm-iaas"></a>VM として SQL Server に移行するタイミング (IaaS)

少数ですが、通常の VM として SQL Server に移行する必要がある場合もあります。 サンプル シナリオとして、SQL Server Reporting Services を使用する必要がある場合が挙げられます。 ただし、ほとんどの場合、Azure SQL Database Managed Instance で、オンプレミスの SQL サーバーから移行するために必要なすべてのものを提供できるため、SQL Server VM への移行は、最後の手段として行う必要があります。

## <a name="use-azure-database-migration-service-to-migrate-your-relational-databases-to-azure"></a>Azure Database Migration Service を使用してリレーショナル データベースを Azure に移行する

Azure Database Migration Service を使用して、SQL Server、Oracle、MySQL などのリレーショナル データベースを Azure に移行することができます。これは、ターゲット データベースが Azure SQL Database、Azure SQL Database Managed Instance、または Azure VM 上の SQL Server のいずれでも関係ありません。

自動化されたワークフローと評価レポートを使用すると、データベースを移行する前に必要な変更を行うことができます。 準備が整うと、サービスによってソース データベースが Azure に移行されます。

元の RDBMS を変更するたびに、再テストが必要になる場合があります。 また、テスト結果によっては、アプリケーションの SQL 文またはオブジェクト リレーショナル マッピング (ORM) コードの変更が必要になる場合があります。

その他のデータベース (IBM DB2 など) があり、リフト アンド シフト アプローチを選択した場合は、より複雑なデータ移行を実行する場合を除き、これらのデータベースを Azure で IaaS VM として引き続き使用することをお勧めします。 より複雑なデータ移行では、新しいスキーマと異なるプログラミング ライブラリを持つ別の種類のデータベースに移行するため、追加の作業が必要になります。

Azure Database Migration Service を使用してデータベースを移行する方法については、「[Get to the cloud faster with Azure SQL Database Managed Instance and Azure Database Migration Service](https://channel9.msdn.com/Events/Build/2017/P4008)」 (Azure SQL Database Managed Instance と Azure Database Migration Service を使用してクラウドにすばやく移行する) を参照してください。

## <a name="additional-resources"></a>その他の技術情報

- **クラウド SQL Server オプションを選択する:Azure SQL Database (PaaS) または Azure VM 上の SQL Server (IaaS)**

    <https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas>

- **Azure SQL DB Managed Instance と Database Migration Service を使用してクラウドにすばやく移行する**

    <https://channel9.msdn.com/Events/Build/2017/P4008>

- **SQL Server データベースのクラウド内の SQL Database への移行**

    <https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate>

- **Azure SQL Database**

    <https://azure.microsoft.com/services/sql-database/?v=16.50>

- **仮想マシン上の SQL Server**

    <https://azure.microsoft.com/services/virtual-machines/sql-server/>

> [!div class="step-by-step"]
> [前へ](lift-and-shift-existing-apps-azure-iaas.md)
> [次へ](modernize-existing-apps-to-cloud-optimized/index.md) <!-- Next Chapter -->
