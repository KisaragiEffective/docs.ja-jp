---
title: アーキテクチャ
ms.date: 03/30/2017
ms.assetid: fcd45b99-ae8f-45ab-8b97-d887beda734e
ms.openlocfilehash: de33c9964f3c03b18593b0df0607f941d2117be0
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980315"
---
# <a name="adonet-architecture"></a>ADO.NET のアーキテクチャ
従来のデータ処理は、主に接続をベースとした 2 層モデルに基づいていました。 近年、データ処理では多層アーキテクチャの採用が増えてきており、アプリケーションのスケーラビリティを高める非接続型アプローチが主流になりつつあります。  
  
## <a name="adonet-components"></a>ADO.NET のコンポーネント  
 データにアクセスして操作する ADO.NET の2つの主要なコンポーネントは、.NET Framework データプロバイダーと <xref:System.Data.DataSet>です。  
  
### <a name="net-framework-data-providers"></a>.NET Framework データ プロバイダー  
 .NET Framework データ プロバイダーは、データの操作と、データに対する高速かつ前方参照専用、読み込み専用のアクセスを実行することを明確な目標としてデザインされたコンポーネントです。 `Connection` オブジェクトはデータ ソースへの接続を実現します。 `Command` オブジェクトによりデータベース コマンドにアクセスし、データの取得、データの修正、ストアド プロシージャの実行、およびパラメーター情報の送信または取得を実行できます。 `DataReader` は、データ ソースからのパフォーマンスの高いデータ ストリームを実現します。 最後に、`DataAdapter` は `DataSet` オブジェクトとデータ ソース間のブリッジとして機能します。 `DataAdapter` では `Command` オブジェクトを使用してデータ ソースに対して SQL コマンドを実行して、`DataSet` にデータを読み込むと同時に、`DataSet` のデータに加えられた変更をデータ ソースと調整します。 詳細については、「 [.NET Framework データプロバイダー](data-providers.md) 」および「 [ADO.NET でのデータの取得および変更](retrieving-and-modifying-data.md)」を参照してください。  
  
### <a name="the-dataset"></a>DataSet  
 ADO.NET `DataSet` は、どのデータ ソースにも依存しないデータ アクセスを行うことを目的としています。 したがって、複数の異なるデータ ソースと併用したり、XML データと併用したり、アプリケーションにとってローカルなデータを管理するために使用したりできます。 `DataSet` には、1 つ以上の <xref:System.Data.DataTable> オブジェクトのコレクションが含まれます。このオブジェクトは、データの行と列に加え、主キー、外部キー、制約、および `DataTable` オブジェクトのリレーション情報で構成されます。 詳細については、「[データセット、datatable、および DataViews](./dataset-datatable-dataview/index.md)」を参照してください。  
  
 次の図は、.NET Framework データプロバイダーと `DataSet`の関係を示しています。  
  
 ![ADO.Net グラフィック](./media/ado-1-bpuedev11.png "ado_1_bpuedev11")  
ADO.NET のアーキテクチャ  
  
### <a name="choosing-a-datareader-or-a-dataset"></a>DataReader または DataSet の選択  
 アプリケーションで `DataReader` を使用するか (「 [DataReader を使用してデータを取得](retrieving-data-using-a-datareader.md)する」を参照)、または `DataSet` (「データセット」、「datatable」、[および「dataviews](./dataset-datatable-dataview/index.md)」を参照) を決定するときは、アプリケーションで必要な機能の種類を検討してください。 以下を実行する場合は `DataSet` を使用します。  
  
- アプリケーションでデータをローカルにキャッシュすると、そのデータを操作できます。 クエリの実行結果を読み取るだけで良い場合は、`DataReader` の使用をお勧めします。  
  
- 層間で、または XML Web サービスからデータをリモート処理する場合。  
  
- Windows フォーム コントロールとの連結、または複数ソースに属するデータの組み合わせや関連付けなど、データと動的に対話する場合。  
  
- データ ソースとの接続を開かずにデータに対する広範な処理を実行する場合。他のクライアントが使用できるように、接続が解放されます。  
  
 `DataSet` の機能が必要ない場合は、`DataReader` を使用して前方参照専用、読み取り専用の方法でデータを返すことにより、アプリケーションのパフォーマンスを向上させることができます。 `DataReader` `DataSet`はを`DataAdapter` `DataReader`使用しての内容を塗りつぶします (「[DataAdapter からのデータセットの読み込み](populating-a-dataset-from-a-dataadapter.md)」を参照してください)。を使用して、パフォーマンスを向上させることができます。、の内容を作成して格納するために必要な処理を回避します。`DataSet` `DataSet`  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 LINQ to DataSet はクエリ機能を備え、DataSet オブジェクトでキャッシュされたデータに対するコンパイル時の型チェックを行います。 これを使用すると、C# や Visual Basic などのいずれかの .NET Framework 開発言語でクエリを記述できます。 詳細については、「[LINQ to DataSet](linq-to-dataset.md)」を参照してください。  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 LINQ to SQL では、リレーショナル データベースのデータ構造と対応付けられたオブジェクト モデルに対し、中間概念モデルを使用することなくクエリを実行できます。 それぞれのテーブルは独立したクラスで表現され、オブジェクト モデルとリレーショナル データベース スキーマとが緊密に結び付けられます。 LINQ to SQL では、オブジェクト モデルの統合言語クエリが Transact-SQL に変換され、データベースに送信されて実行されます。 データベースから結果が返されると、LINQ to SQL によって、その結果が再びオブジェクトへと変換されます。 詳細については、「[LINQ to SQL](./sql/linq/index.md)」を参照してください。  
  
## <a name="adonet-entity-framework"></a>ADO.NET Entity Framework  
 ADO.NET Entity Framework は、開発者がリレーショナル ストレージ スキーマに対して直接プログラムを作成するのではなく、概念アプリケーション モデルに対してプログラムを作成して、データ アクセス アプリケーションを作成できるように設計されています。 その目的は、データ指向アプリケーションに必要なコードの量と保守作業の量を減らすことです。 詳細については、「 [ADO.NET Entity Framework](./ef/index.md)」を参照してください。  
  
## <a name="wcf-data-services"></a>WCF Data Services  
 WCF Data Services は、Web またはイントラネットにデータサービスを配置するために使用されます。 データは、エンティティ データ モデルの仕様に従ってエンティティおよびリレーションシップとして構成されます。 このモデルで展開されるデータは、標準 HTTP プロトコルによってアドレス指定可能です。 詳細については、「[WCF Data Services 4.5](../wcf/index.md)」を参照してください。  
  
## <a name="xml-and-adonet"></a>XML と ADO.NET  
 ADO.NET は、XML の機能を活用して、データへの切断されたアクセスを提供します。 ADO.NET は、.NET Framework の XML クラスを使用して手動で設計されています。どちらも1つのアーキテクチャのコンポーネントです。  
  
 ADO.NET および .NET Framework 内の XML クラスが `DataSet` オブジェクトに集約されています。 `DataSet` に、XML ソース (ファイルまたは XML ストリーム) に含まれるデータを入力できます。 `DataSet` は、XML スキーマ定義言語 (XSD) スキーマを含む、W3C (World Wide Web Consortium) 準拠の XML として作成できます。これには `DataSet` 内のデータのソースは関係ありません。 `DataSet` のネイティブのシリアル化形式は XML であることから、層間でデータを移動するための媒体として優れており、XML Web サービスとの間でデータとスキーマ コンテキストをリモート処理する場合には `DataSet` が最適な選択となります。 詳細については、「[XML ドキュメントと XML データ](../../../standard/data/xml/index.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [ADO.NET の概要](ado-net-overview.md)
