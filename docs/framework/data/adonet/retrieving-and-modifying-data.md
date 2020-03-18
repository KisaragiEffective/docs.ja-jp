---
title: データの取得と変更
ms.date: 03/30/2017
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.openlocfilehash: 65c373ecff004e219527754bf2e9cc56837dc305
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980055"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>ADO.NET でのデータの取得および変更
データベース アプリケーションの主な機能は、データ ソースとの接続およびデータベースに格納されているデータの取得です。 ADO.NET の .NET Framework データプロバイダーは、アプリケーションとデータソースの間のブリッジとして機能します。これにより、 **DataReader**または**DataAdapter**を使用して、コマンドを実行したり、データを取得したりすることができます。 データベースに格納されているデータを更新する機能は、データベース アプリケーションの重要な機能の 1 つです。 ADO.NET では、データの更新に**DataAdapter**および <xref:System.Data.DataSet>、および**Command**オブジェクトを使用する必要があります。また、トランザクションの使用が必要になる場合もあります。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [データ ソースへの接続](connecting-to-a-data-source.md)  
 データ ソースへの接続を確立する方法、および接続イベントを使用する方法について説明します。  
  
 [接続文字列](connection-strings.md)  
 接続文字列のキーワード、セキュリティ情報、セキュリティ情報の格納や取得など、接続文字列を使用するうえでのさまざまな側面について説明します。  
  
 [接続プール](connection-pooling.md)  
 .NET Framework Data Provider の接続プールについて説明します。  
  
 [コマンドおよびパラメーター](commands-and-parameters.md)  
 コマンドおよびコマンド ビルダーを作成する方法、パラメーターを構成する方法、およびコマンドを実行してデータを取得および変更する方法について説明します。  
  
 [DataAdapter と DataReader](dataadapters-and-datareaders.md)  
 DataReaders、DataAdapters、パラメーター、DataAdapter イベントの処理、およびバッチ操作の実行について説明します。  
  
 [トランザクションとコンカレンシー](transactions-and-concurrency.md)  
 ローカル トランザクションや分散トランザクションの実行方法、およびオプティミスティック コンカレンシーの使用方法について説明します。  
  
 [ID 値および Autonumber 値の取得](retrieving-identity-or-autonumber-values.md)  
 SQL Server テーブル内の**id**列、または Microsoft access テーブルの**オートナンバー**フィールドに対して生成された値を、テーブルの挿入行の列にマップする例を示します。 `DataTable` での ID 値の結合について説明します。  
  
 [バイナリ データの取得](retrieving-binary-data.md)  
 `CommandBehavior`を使用してバイナリデータまたは大規模なデータ構造を取得する方法について説明します。`SequentialAccess` `DataReader`の既定の動作を変更するには  
  
 [ストアド プロシージャでのデータの変更](modifying-data-with-stored-procedures.md)  
 ストアド プロシージャの入力パラメーターおよび出力パラメーターを使用してデータベースに行を挿入し、新しい ID 値を返す方法について説明します。  
  
 [データベース スキーマ情報の取得](retrieving-database-schema-information.md)  
 データベースまたはカタログ、データベース内のテーブルおよびビュー、テーブルに対して存在する制約、およびその他のスキーマ情報をデータ ソースから取得する方法について説明します。  
  
 [DbProviderFactories](dbproviderfactories.md)  
 プロバイダー ファクトリ モデルについて説明し、`System.Data.Common` 名前空間の基本クラスの使用方法を示します。  
  
 [ADO.NET のデータ追跡](data-tracing.md)  
 ADO.NET が備える組み込みデータ トレース機能のしくみについて説明します。  
  
 [Performance Counters](performance-counters.md)  
 `SqlClient` および `OracleClient` で使用できるパフォーマンス カウンターについて説明します。  
  
 [非同期の概要](asynchronous-programming.md)  
 非同期プログラミングの ADO.NET サポートについて説明します。  
  
 [SqlClient ストリーミング サポート](sqlclient-streaming-support.md)  
 メモリに完全に読み込まれることなく SQL Server からデータをストリームするアプリケーションを作成する方法について説明します。  
  
## <a name="see-also"></a>関連項目

- [ADO.NET でのデータ型のマッピング](data-type-mappings-in-ado-net.md)
- [DataSet、DataTable、および DataView](./dataset-datatable-dataview/index.md)
- [ADO.NET アプリケーションのセキュリティ保護](securing-ado-net-applications.md)
- [SQL Server と ADO.NET](./sql/index.md)
- [ADO.NET の概要](ado-net-overview.md)
