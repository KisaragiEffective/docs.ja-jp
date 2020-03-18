---
title: LINQ to DataSet の概要
ms.date: 03/30/2017
ms.assetid: dc20a8fb-03f6-4b68-9c2b-7f7299e3070b
ms.openlocfilehash: 20d9be052ba2567c16718b4b1c1019427c9b5df1
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634821"
---
# <a name="linq-to-dataset-overview"></a>LINQ to DataSet の概要
<xref:System.Data.DataSet> は、ADO.NET のコンポーネントのうち、広く使用されているコンポーネントの1つです。 これは、ADO.NET がベースになっている非接続型プログラミングモデルの重要な要素であり、さまざまなデータソースからのデータを明示的にキャッシュすることができます。 プレゼンテーション層では、<xref:System.Data.DataSet> とデータ バインディングの GUI コントロールとが密接に連携します。 中間層では、リレーショナル形式のデータを維持するキャッシュとして機能し、単純で高速なクエリと、階層的なナビゲーション サービスを提供します。 データベースでの要求数を減らすために使用される一般的な手法は、中間層でキャッシュに <xref:System.Data.DataSet> を使用することです。 たとえば、データドリブンの ASP.NET Web アプリケーションについて考えてみます。 ほとんど変更されないアプリケーション データがかなりの割合で存在し、しかも、複数のセッションまたは複数のユーザー間で共通して使用されているケースがよくあります。 このデータを Web サーバー上のメモリに維持しておくことで、データベースに対する要求数を減らし、高速な対話処理をユーザーに提供できます。 <xref:System.Data.DataSet> のもう1つの便利な側面は、アプリケーションが1つ以上のデータソースからのデータのサブセットをアプリケーション領域に取り込むことができることです。 アプリケーションは、そのデータをリレーショナル形式を維持したままインメモリで操作できます。  
  
 こうした突出した特長がある反面、<xref:System.Data.DataSet> のクエリ機能には制限があります。 <xref:System.Data.DataTable.Select%2A> メソッドでデータのフィルター処理や並べ替えを行ったり、<xref:System.Data.DataRow.GetChildRows%2A> メソッドや <xref:System.Data.DataRow.GetParentRow%2A> メソッドを使って階層のナビゲーションを行うことはできます。 しかし、さらに複雑な処理を行うには、開発者が独自にクエリを作成する必要があります。 その結果、アプリケーションで期待したパフォーマンスが得られなかったり、メンテナンスが難しくなったりする可能性があります。  
  
 LINQ to DataSet を使用すると、<xref:System.Data.DataSet> オブジェクトにキャッシュされたデータに対するクエリを簡単かつ迅速に行うことができます。 これらのクエリはアプリケーション コードに文字列リテラルとして記述するのではなく、プログラミング言語そのもので表現できます。 つまり、開発者はクエリ言語を個別に習得する必要はありません。 また、visual studio IDE では、コンパイル時の構文チェック、静的な型指定、および LINQ の IntelliSense サポートが提供されるため、Visual Studio の開発者は、LINQ to DataSet を使用すると、より生産的に作業を行うことができます。 また、LINQ to DataSet を使用して、1つまたは複数のデータソースから統合されたデータを照会することもできます。 これにより、データの表現方法や扱いに柔軟性が要求されるさまざまなシナリオが実現します。 特に、汎用のレポート作成、分析、ビジネス インテリジェンスを行うアプリケーションでは、この手法を用いたデータ操作が欠かせません。  
  
## <a name="querying-datasets-using-linq-to-dataset"></a>LINQ to DataSet を使った DataSet のクエリ  
 LINQ to DataSet を使用して <xref:System.Data.DataSet> オブジェクトのクエリを開始する前に、<xref:System.Data.DataSet>にデータを設定する必要があります。 <xref:System.Data.Common.DataAdapter> クラスや[LINQ to SQL](./sql/linq/index.md)を使用するなど、<xref:System.Data.DataSet>にデータを読み込むには、いくつかの方法があります。 データが <xref:System.Data.DataSet> オブジェクトに読み込まれたら、クエリを開始できます。 LINQ to DataSet を使用してクエリを実行することは、他の LINQ 対応データソースに対して統合言語クエリ (LINQ) を使用することと似ています。 LINQ クエリは、<xref:System.Data.DataSet> 内の1つのテーブル、または <xref:System.Linq.Enumerable.Join%2A> と <xref:System.Linq.Enumerable.GroupJoin%2A> の標準クエリ演算子を使用して複数のテーブルに対して実行できます。  
  
 LINQ クエリは、型指定された <xref:System.Data.DataSet> オブジェクトと型指定のないオブジェクトの両方に対してサポートされます。 アプリケーションのデザイン時に <xref:System.Data.DataSet> のスキーマがわかっている場合は、型指定された <xref:System.Data.DataSet> を使用することをお勧めします。 型指定された <xref:System.Data.DataSet> のテーブルおよび行は、列ごとに型指定されたメンバーを持ちます。これにより、クエリが簡素化され、読みやすくなります。  
  
 LINQ to DataSet に実装された標準クエリ演算子に加えて、<xref:System.Data.DataRow> オブジェクトのセットに対するクエリを容易にする、<xref:System.Data.DataSet>固有の拡張機能がいくつか追加されています。 これらの <xref:System.Data.DataSet> 固有の拡張機能には、<xref:System.Data.DataRow> の列値にアクセスするためのメソッドのほか、一連の行を比較するための演算子があります。  
  
## <a name="n-tier-applications-and-linq-to-dataset"></a>n 層アプリケーションと LINQ to DataSet  
 n 層データ アプリケーションとは、複数の論理レイヤー (つまり層) に分離されるデータ中心のアプリケーションです。 一般に、n 層アプリケーションには、プレゼンテーション層、中間層、およびデータ層が含まれます。 アプリケーション コンポーネントを別個の層に分離すると、アプリケーションの保守容易性とスケーラビリティが向上します。 N 層データアプリケーションの詳細については、「 [n 層アプリケーションでのデータセットの操作](/visualstudio/data-tools/work-with-datasets-in-n-tier-applications)」を参照してください。  
  
 n 層アプリケーションでは、Web アプリケーションの情報をキャッシュするために <xref:System.Data.DataSet> が中間層で使用されます。 LINQ to DataSet のクエリ機能は、拡張メソッドによって実装され、既存の ADO.NET 2.0 <xref:System.Data.DataSet>を拡張します。  
  
## <a name="see-also"></a>関連項目

- [DataSet のクエリ](querying-datasets-linq-to-dataset.md)
- [統合言語クエリ (LINQ) - C#](../../../csharp/programming-guide/concepts/linq/index.md)
- [統合言語クエリ (LINQ) - Visual Basic](../../../visual-basic/programming-guide/concepts/linq/index.md)
- [LINQ to SQL](./sql/linq/index.md)
