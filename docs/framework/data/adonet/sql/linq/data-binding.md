---
title: データ バインディング
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: cbec8b02-a1e8-4ae8-a83b-bb5190413ac5
ms.openlocfilehash: c7048d292bdf5c1372d5f8f174f7f0e84efa7593
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634717"
---
# <a name="data-binding"></a>データ バインディング

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、グリッドコントロールなどの一般的なコントロールへのバインドをサポートしています。 具体的には、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、表示と更新に関して、データグリッドにバインドし、マスター/詳細バインドを処理するための基本的なパターンを定義します。

## <a name="underlying-principle"></a>基本原則

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、データベースで実行するために LINQ クエリを SQL に変換します。 その結果は、厳密に型指定された `IEnumerable` になります。 これらのオブジェクトは通常の共通言語ランタイム (CLR: Common Language Runtime) オブジェクトであるため、一般的なオブジェクト データ バインディングを使用して結果を表示できます。 一方、変更操作 (挿入、更新、および削除) には、追加的な手順が必要です。

## <a name="operation"></a>操作

Windows フォーム コントロールに対する暗黙バインディングは、<xref:System.ComponentModel.IListSource> を実装することで実現されます。 データソース generic <xref:System.Data.Linq.Table%601> (Visual Basic でC#の`Table<T>` または `Table(Of T)`) と汎用 `DataQuery` が <xref:System.ComponentModel.IListSource>を実装するように更新されました。 ユーザー インターフェイス (UI) データ バインディング エンジン (Windows フォームおよび Windows Presentation Foundation) はいずれも、データ ソースが <xref:System.ComponentModel.IListSource> を実装しているかどうかをテストします。 このため、クエリの直接表示をコントロールのデータソースに書き込むと、次の例のように、暗黙的にコレクション生成 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] が呼び出されます。

[!code-csharp[DLinqDataBinding#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqDataBinding/cs/Program.cs#1)]
[!code-vb[DLinqDataBinding#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqDataBinding/vb/Module1.vb#1)]

Windows Presentation Foundation でも同じようになります。

[!code-csharp[DLinqDataBinding#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqDataBinding/cs/Program.cs#2)]
[!code-vb[DLinqDataBinding#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqDataBinding/vb/Module1.vb#2)]

コレクション生成は、ジェネリック <xref:System.Data.Linq.Table%601> およびジェネリック `DataQuery` によって <xref:System.ComponentModel.IListSource.GetList%2A> で実装されます。

## <a name="ilistsource-implementation"></a>IListSource の実装

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、次の2つの場所に <xref:System.ComponentModel.IListSource> を実装します。

- データソースは <xref:System.Data.Linq.Table%601>です。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] テーブルを参照して、テーブルに参照を保持する `DataBindingList` コレクションに格納します。

- データ ソースが <xref:System.Linq.IQueryable%601> の場合 : シナリオは 2 つあります。

  - [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] によって <xref:System.Linq.IQueryable%601>から基になる <xref:System.Data.Linq.Table%601> が検出された場合、ソースはエディションを許可し、状況は最初の箇条書きと同じになります。

  - [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 基になる <xref:System.Data.Linq.Table%601>が見つからない場合、ソースではエディション (`groupby`など) が許可されません。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、クエリを参照して汎用 `SortableBindingList`を設定します。これは、特定のプロパティの T エンティティの並べ替え機能を実装する単純な <xref:System.ComponentModel.BindingList%601> です。

## <a name="specialized-collections"></a>専用コレクション

このドキュメントでここまでに説明した多くの機能について、<xref:System.ComponentModel.BindingList%601> を特化したクラスが用意されています。 ジェネリック `SortableBindingList` クラスと、ジェネリック `DataBindingList` クラスです。 いずれも内部クラスとして宣言されています。

### <a name="generic-sortablebindinglist"></a>ジェネリック SortableBindingList

これは <xref:System.ComponentModel.BindingList%601> を継承したクラスで、並べ替え可能な <xref:System.ComponentModel.BindingList%601> です。 並べ替えはメモリ内で処理され、データベース自体とのやり取りは行われません。 <xref:System.ComponentModel.BindingList%601> は <xref:System.ComponentModel.IBindingList> を実装していますが、既定では並べ替えをサポートしていません。 ただし、<xref:System.ComponentModel.BindingList%601> は、仮想*コア*メソッドで <xref:System.ComponentModel.IBindingList> を実装します。 これらのメソッドを簡単にオーバーライドできます。 ジェネリック `SortableBindingList` は、<xref:System.ComponentModel.BindingList%601.SupportsSortingCore%2A>、<xref:System.ComponentModel.BindingList%601.SortPropertyCore%2A>、<xref:System.ComponentModel.BindingList%601.SortDirectionCore%2A>、および <xref:System.ComponentModel.BindingList%601.ApplySortCore%2A> をオーバーライドします。 `ApplySortCore` は <xref:System.ComponentModel.IBindingList.ApplySort%2A> によって呼び出され、指定のプロパティで T の項目のリストを並べ替えます。

そのプロパティが T にない場合、例外が発生します。

並べ替えを実現するために、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] はジェネリック <xref:System.Collections.Generic.Comparer%601.System%23Collections%23IComparer%23Compare%2A> を継承し、指定された型 T、`PropertyDescriptor`、および方向の既定の比較子を実装するジェネリック `SortableBindingList.PropertyComparer` クラスを作成します。 このクラスは、T の `Comparer` を動的に作成します (T は `PropertyType` の `PropertyDescriptor`)。 そして、静的なジェネリック `Comparer` から既定の比較子が取得されます。 既定のインスタンスはリフレクションを使用して取得されます。

ジェネリック `SortableBindingList` は `DataBindingList` の基本クラスでもあります。 ジェネリック `SortableBindingList` には、項目の追加と削除の追跡を中断および再開するための 2 つの仮想メソッドがあります。 これら 2 つのメソッドは、並べ替えなどの基本機能にも使用できますが、実際にはジェネリック `DataBindingList` などの上位クラスによって実装されます。

### <a name="generic-databindinglist"></a>ジェネリック DataBindingList

このクラスは、ジェネリック `SortableBindingLIst` を継承しています。 ジェネリック `DataBindingList` は、最初にコレクションの読み込みに使用したジェネリック `Table` の基になるジェネリック `IQueryable` に対する参照を保持しています。 ジェネリック `DatabindingList` では、`InsertItem`() と `RemoveItem`() がオーバーライドされて、コレクションに対する項目の追加と削除を追跡する処理が追加されています。 また、追跡を中断および再開する機能の抽象メソッドが実装され、条件に応じた追跡が可能となっています。 この結果、ジェネリック `DataBindingList` では、親クラスが持つ追跡機能のポリモーフィックな使用法がすべて活用されています。

## <a name="binding-to-entitysets"></a>EntitySet へのバインディング

`EntitySet` へのバインディングは特別なケースです。`EntitySet` は既に、<xref:System.ComponentModel.IBindingList> を実装したコレクションであるためです。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] により、並べ替えとキャンセル (<xref:System.ComponentModel.ICancelAddNew>) のサポートが追加されます。 `EntitySet` クラスは内部リストを使用してエンティティを格納します。 このリストは、ジェネリック配列 (ジェネリック `ItemList` クラス) を基にした低水準のコレクションです。

### <a name="adding-a-sorting-feature"></a>並べ替え機能の追加

Array には、T の `Array.Sort()` を使用できる並べ替えメソッド (`Comparer`) があります。[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、このトピックで前に説明したジェネリック `SortableBindingList.PropertyComparer` クラスを使用して、プロパティに応じた `Comparer` と、並べ替えの方向を取得します。 ジェネリック `ApplySort` には、この機能を呼び出すための `ItemList` メソッドが追加されています。

`EntitySet` 側では、並べ替えのサポートを次のように宣言する必要があります。

- <xref:System.ComponentModel.IBindingList.SupportsSorting%2A> は、`true` を返します。

- <xref:System.ComponentModel.IBindingList.ApplySort%2A> では、`entities.ApplySort()` を呼び出した後で `OnListChanged()` を呼び出します。

- <xref:System.ComponentModel.IBindingList.SortDirection%2A> プロパティおよび <xref:System.ComponentModel.IBindingList.SortProperty%2A> プロパティでは、現在の並べ替えの定義を公開します。これはローカル メンバーに格納されています。

TEntity を使用して、EntitySet\<> を TEntity にバインドする場合は、EntitySet\<> を呼び出す必要があります。これは、Bindingsource.list .getnewbindinglist を更新します。

TEntity を使用して、bindingsource プロパティを設定し、bindingsource 内にという名前のプロパティを持つクラスに BindingSource を設定した場合は、EntitySet\<TEntity > を呼び出す必要はありません。このクラスには、EntitySet\<> を公開する必要があります。Bindingsource.list .getnewbindinglist は、BindingSource を更新しようとしていますが、並べ替え機能が失われています。

## <a name="caching"></a>キャッシュ

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] クエリは <xref:System.ComponentModel.IListSource.GetList%2A>を実装します。 Windows フォームの BindingSource クラスは、このインターフェイスがあると、1 つの接続に対して GetList() を 3 回呼び出します。 この状況を回避するために、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、格納するインスタンスごとにキャッシュを実装し、生成された同じコレクションを常に返します。

## <a name="cancellation"></a>キャンセル

<xref:System.ComponentModel.IBindingList> には <xref:System.ComponentModel.IBindingList.AddNew%2A> メソッドが定義されています。バインドされたコレクションから新しい項目を作成するためにコントロールが使用するメソッドです。 `DataGridView` コントロールでは、表示されている最後の行のヘッダーにアスタリスクが表示されている状況で、この機能が明確に示されます。 このアスタリスクは、新しい項目を追加できることを示します。

コレクションは、この機能に加えて、<xref:System.ComponentModel.ICancelAddNew> も実装することができます。 この機能によって、コントロールでは、キャンセルを行ったり、新しく編集された項目が検証されているかどうかを確認したりできるようになります。

<xref:System.ComponentModel.ICancelAddNew> は、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のすべてのデータ バインド コレクション (ジェネリック `SortableBindingList` およびジェネリック `EntitySet`) に実装されます。 どちらの実装でも、コードは次のように動作します。

- いったん挿入した項目をコレクションから削除できます。

- UI が編集をコミットしていない場合、変更を追跡しません。

- 編集がキャンセルされた場合 (<xref:System.ComponentModel.ICancelAddNew.CancelNew%2A>)、変更を追跡しません。

- 編集がコミットされた場合 (<xref:System.ComponentModel.ICancelAddNew.EndNew%2A>)、追跡を行うことができます。

- 新しい項目が <xref:System.ComponentModel.IBindingList.AddNew%2A> で追加されたものでない場合、コレクションは通常どおり動作します。

## <a name="troubleshooting"></a>トラブルシューティング

このセクションでは、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のデータ バインディング アプリケーションのトラブルシューティングに役立つ可能性がある項目について説明します。

- プロパティを使用する必要があります。フィールドのみの使用では不十分です。 この使用は Windows フォームで必要です。

- 既定では、`image`、`varbinary`、および `timestamp` データベースの型は、バイト配列にマップされます。 このシナリオでは `ToString()` がサポートされていないため、これらのオブジェクトは表示できません。

- 主キーにマップされたクラスメンバーに setter がありますが、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] はオブジェクト id の変更をサポートしていません。 したがって、対応付けで使用されているデータベースの主キー/一意キーは更新できません。 グリッドを変更すると、<xref:System.Data.Linq.DataContext.SubmitChanges%2A>を呼び出すと例外が発生します。

- 1 つのエンティティが 2 つの別個のグリッド (たとえばマスター グリッドと詳細グリッド) にバインドされている場合、マスター グリッドで `Delete` を行っても、詳細グリッドには反映されません。

## <a name="see-also"></a>関連項目

- [背景情報](background-information.md)
