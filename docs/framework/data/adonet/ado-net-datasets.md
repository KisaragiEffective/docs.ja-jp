---
title: データセット
ms.date: 03/30/2017
ms.assetid: 82b641bb-6001-4512-bf1a-2830acdd92ab
ms.openlocfilehash: 86c14f516ff82e4d9acf7cc3078e04590971a8a1
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980302"
---
# <a name="adonet-datasets"></a>ADO.NET データセット
<xref:System.Data.DataSet> オブジェクトは、ADO.NET を使用して、切断された分散データシナリオをサポートするための中心的なオブジェクトです。 データ**セット**は、データソースに関係なく、一貫性のあるリレーショナルプログラミングモデルを提供する、メモリ常駐型のデータ表現です。 複数の異なるデータ ソースや XML データと組み合わせて使用でき、アプリケーションにとってローカルなデータの管理にも使用できます。 データ**セット**は、関連テーブル、制約、およびテーブル間のリレーションシップを含む、データの完全なセットを表します。 次の図は、**データセット**オブジェクトモデルを示しています。  
  
 ![ADO.Net グラフィック](./media/ado-1-bpuedev11.png "ado_1_bpuedev11")  
DataSet オブジェクト モデル  
  
 **データセット**内のメソッドとオブジェクトは、リレーショナルデータベースモデルのメソッドとオブジェクトと一貫性があります。  
  
 また、**データセット**は、その内容を xml として永続化し、xml スキーマ定義言語 (XSD) スキーマとしてそのスキーマを再読み込みすることもできます。 詳しくは、「[DataSet での XML の使用](./dataset-datatable-dataview/using-xml-in-a-dataset.md)」を参照してください。  
  
## <a name="the-datatablecollection"></a>DataTableCollection  
 ADO.NET**データセット**には、<xref:System.Data.DataTable> オブジェクトによって表される0個以上のテーブルのコレクションが含まれます。 <xref:System.Data.DataTableCollection> には、**データセット**内のすべての**DataTable**オブジェクトが含まれています。  
  
 **DataTable**は <xref:System.Data> 名前空間で定義され、メモリ常駐データの1つのテーブルを表します。 このテーブルには、共にテーブルのスキーマを定義する <xref:System.Data.DataColumnCollection> で表現される列と <xref:System.Data.ConstraintCollection> で表現される制約のコレクションが含まれます。 **DataTable**には、テーブル内のデータを含む、<xref:System.Data.DataRowCollection>によって表される行のコレクションも含まれます。 <xref:System.Data.DataRow> には、行に格納された値の変更を識別できるように、行の現在の状態と共に、行の現在のバージョンと元のバージョンの両方が保持されます。  
  
## <a name="the-dataview-class"></a>DataView クラス  
 <xref:System.Data.DataView> では、<xref:System.Data.DataTable> に格納されているデータのさまざまなビューを作成できます。この機能は、データ連結アプリケーションで頻繁に使用されます。 <xref:System.Data.DataView> を使用すると、テーブルのデータをさまざまな並べ替え順序で公開したり、行の状態やフィルター式に基づいてデータをフィルター処理したりできます。 詳細については、「 [Dataviews](./dataset-datatable-dataview/dataviews.md)」を参照してください。  
  
## <a name="the-datarelationcollection"></a>DataRelationCollection  
 **データセット**には、<xref:System.Data.DataRelationCollection> オブジェクト内のリレーションシップが含まれます。 <xref:System.Data.DataRelation> オブジェクトによって表されるリレーションシップは、ある**datatable**内の行を別の**datatable**の行に関連付けます。 リレーションシップは、リレーショナル データベースの主キー列と外部キー列の間に存在する結合パスに似ています。 **DataRelation**は、**データセット**の2つのテーブルの一致する列を識別します。  
  
 リレーションシップを使用すると、**データセット**内の1つのテーブルから別のテーブルに移動できます。 **DataRelation**の重要な要素は、リレーションシップの名前、関連付けられているテーブルの名前、および各テーブル内の関連する列です。 <xref:System.Data.DataColumn> オブジェクトの配列をキー列として指定することによって、テーブルごとに複数の列を使用してリレーションシップを構築できます。 <xref:System.Data.DataRelationCollection>にリレーションシップを追加する場合は、必要に応じて**UniqueKeyConstraint**と**ForeignKeyConstraint**を追加して、関連する列の値に変更が加えられたときに整合性制約を適用することができます。  
  
 詳細については、「 [datarelation の追加](./dataset-datatable-dataview/adding-datarelations.md)」を参照してください。  
  
## <a name="xml"></a>XML  
 XML ストリームまたは XML ドキュメントから**データセットにデータ**を読み込むことができます。 XML ストリームまたはドキュメントを使用して、データ**セット**にデータ、スキーマ情報、またはその両方を提供できます。 XML ストリームまたはドキュメントから提供された情報は、データ**セット**内に既に存在する既存のデータまたはスキーマ情報と組み合わせることができます。 詳しくは、「[DataSet での XML の使用](./dataset-datatable-dataview/using-xml-in-a-dataset.md)」を参照してください。  
  
## <a name="extendedproperties"></a>ExtendedProperties  
 **DataSet**、 **DataTable**、および**DataColumn**には、 **extendedproperties**プロパティがあります。 **Extendedproperties**は、結果セットの生成に使用された select ステートメントや、データが生成された時刻などのカスタム情報を配置できる**PropertyCollection**です。 **Extendedproperties**コレクションは、**データセット**のスキーマ情報と共に保持されます。  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 LINQ to DataSet は、データセットに格納されている非接続データに対して統合言語クエリ機能を提供します。 LINQ to DataSet は、Visual Studio IDE を使用しているときに、標準の LINQ 構文を使用し、コンパイル時の構文チェック、静的な型指定、IntelliSense のサポートを提供します。  
  
 詳細については、「[LINQ to DataSet](linq-to-dataset.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [ADO.NET の概要](ado-net-overview.md)
- [DataSet、DataTable、および DataView](./dataset-datatable-dataview/index.md)
- [ADO.NET でのデータの取得および変更](retrieving-and-modifying-data.md)
