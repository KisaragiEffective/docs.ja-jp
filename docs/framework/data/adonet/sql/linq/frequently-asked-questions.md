---
title: よく寄せられる質問
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 252ed666-0679-4eea-b71b-2f14117ef443
ms.openlocfilehash: 3cc879e97438138554f1d39cf588e01bfbba28a6
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634704"
---
# <a name="frequently-asked-questions"></a>よく寄せられる質問

以下のセクションでは、LINQ を実装するときに発生する可能性のある一般的な問題について説明します。

[トラブルシューティング](troubleshooting.md)では、その他の問題に対処します。

## <a name="cannot-connect"></a>接続できません

Q. データベースに接続できません。

A: 接続文字列が正しいことと、SQL Server インスタンスが実行されていることを確認してください。 また、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] では、名前付きパイプ プロトコルを有効にする必要があります。 詳細については、「[チュートリアルによる学習](learning-by-walkthroughs.md)」を参照してください。

## <a name="changes-to-database-lost"></a>データベースの変更内容が失われる

Q. データベース内のデータを変更しましたが、アプリケーションを再実行すると、変更が元に戻っています。

A: 変更結果をデータベースに保存するために、必ず <xref:System.Data.Linq.DataContext.SubmitChanges%2A> を呼び出してください。

## <a name="database-connection-open-how-long"></a>データベース接続: どれほどの時間開いているか

Q. データベース接続が開いている時間は、どれほどですか。

A: 通常、接続は、クエリ結果を処理するまでは開いた状態を保ちます。 すべての結果を処理するのに時間がかかる可能性がある場合、結果をキャッシュに入れることが可能であれば、クエリに対して <xref:System.Linq.Enumerable.ToList%2A> を適用してください。 通常のシナリオでは、各オブジェクトが一度だけ処理されるため、`DataReader` と [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のどちらの場合もストリーミング モデルがより優れています。

接続が厳密にどのように使用されるかは、以下に応じて異なります。

- 接続オブジェクトを使って <xref:System.Data.Linq.DataContext> が作成されている場合は、接続状態。

- 接続文字列の設定 (たとえば、複数のアクティブな結果セット (MARS) の有効化)。 詳細については、「 [複数のアクティブな結果セット (MARS)](../multiple-active-result-sets-mars.md)」を参照してください。

## <a name="updating-without-querying"></a>クエリを使用しない更新

Q. データベースに対してあらかじめクエリを実行せずに、テーブル データを更新できますか?

A: [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] にはセットベースの更新コマンドはありませんが、次のいずれかの技法を使用すると、あらかじめクエリを実行せずに更新できます。

- <xref:System.Data.Linq.DataContext.ExecuteCommand%2A> を使って SQL コードを送信する。

- オブジェクトの新しいインスタンスを作成し、更新に関連したすべての現在値 (フィールド) を初期化する。 その後、<xref:System.Data.Linq.DataContext> を使ってオブジェクトを <xref:System.Data.Linq.Table%601.Attach%2A> に関連付け、更新対象のフィールドを変更します。

## <a name="unexpected-query-results"></a>予期しないクエリ結果

Q. 予期しない結果がクエリから返されます。 どうなっているのか調べる方法はありますか。

A: [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] には、生成された SQL コードを調べるためのツールがいくつか用意されています。 このうち、最も重要なものは <xref:System.Data.Linq.DataContext.Log%2A> です。 詳細については、「[デバッグのサポート](debugging-support.md)」を参照してください。

## <a name="unexpected-stored-procedure-results"></a>予期しないストアド プロシージャ結果

Q. 戻り値が `MAX()` によって計算されるストアド プロシージャがあります。 ストアドプロシージャを O/R デザイナー画面にドラッグすると、戻り値が正しくありません。

A: [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] には、ストアド プロシージャを介して、データベースによって生成される値を返す方法が 2 つあります。

- 出力結果に名前を付ける。

- 出力パラメーターを明示的に指定する。

間違っている出力例を以下に示します。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は結果をマップできないため、常に 0 が返されます。

```sql
create procedure proc2

as

begin

select max(i) from t where name like 'hello'

end
```

出力パラメーターを使用して正しい出力をする例を次に示します。

```sql
create procedure proc2

@result int OUTPUT

as

select @result = MAX(i) from t where name like 'hello'

go
```

出力結果に名前を付けて正しい出力をする例を次に示します。

```sql
create procedure proc2

as

begin

select nax(i) AS MaxResult from t where name like 'hello'

end
```

詳細については、「[ストアドプロシージャを使用](customizing-operations-by-using-stored-procedures.md)した操作のカスタマイズ」を参照してください。

## <a name="serialization-errors"></a>シリアル化のエラー

Q. シリアル化しようとすると、次のエラーが表示されます。 "型 ' System. Data. ChangeTracker + StandardChangeTracker '...がシリアル化可能として設定されていません。 "

A: [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のコード生成では、<xref:System.Runtime.Serialization.DataContractSerializer> のシリアル化がサポートされます。 <xref:System.Xml.Serialization.XmlSerializer> および <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> はサポートされません。 詳細については、「[Serialization](serialization.md)」 (シリアル化) を参照してください。

## <a name="multiple-dbml-files"></a>複数の DBML ファイル

Q. いくつかのテーブルを共有する複数の DBML ファイルが存在する場合は、コンパイラ エラーが発生します。

A: コンテキストの**名前空間**と**エンティティの名前**空間のプロパティをオブジェクトリレーショナルデザイナーから、各 DBML ファイルの個別の値に設定します。 この手法により、名前および名前空間の競合を防ぐことができます。

## <a name="avoiding-explicit-setting-of-database-generated-values-on-insert-or-update"></a>挿入または更新で、データベース生成値を明示的に設定しなくても良いようにする

Q. あるデータベース テーブルに `DateCreated` という列があり、その既定値は SQL `Getdate()` です。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] を使用して新しいレコードを挿入しようとすると、値が `NULL` に設定されます。 データベースの既定値が設定されるようにするには、どうしたらいいですか。

A: ID 列 (自動インクリメント)、rowguidcol 列 (データベースが生成した GUID)、およびタイムスタンプ列の場合は、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] が、このような状況に自動的に対処します。 それ以外の場合は、手動で <xref:System.Data.Linq.Mapping.ColumnAttribute.IsDbGenerated%2A>=`true` <xref:System.Data.Linq.Mapping.ColumnAttribute.AutoSync%2A>を設定し、=<xref:System.Data.Linq.Mapping.AutoSync.Always>/<xref:System.Data.Linq.Mapping.AutoSync.OnInsert>/<xref:System.Data.Linq.Mapping.AutoSync.OnUpdate> プロパティを手動で設定する必要があります。

## <a name="multiple-dataloadoptions"></a>複数の DataLoadOptions

Q. 最初の読み込みオプションを上書きせずに、追加の読み込みオプションを指定できますか?

A: ［はい、 次の例のように、最初のオプションは上書きされません。

```vb
Dim dlo As New DataLoadOptions()
dlo.LoadWith(Of Order)(Function(o As Order) o.Customer)
dlo.LoadWith(Of Order)(Function(o As Order) o.OrderDetails)
```

```csharp
DataLoadOptions dlo = new DataLoadOptions();
dlo.LoadWith<Order>(o => o.Customer);
dlo.LoadWith<Order>(o => o.OrderDetails);
```

## <a name="errors-using-sql-compact-35"></a>SQL Compact 3.5 の使用に関するエラー

Q. SQL Server Compact 3.5 データベースからテーブルをドラッグすると、エラーが発生します。

A: オブジェクトリレーショナルデザイナーは、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ランタイムでは SQL Server Compact 3.5 をサポートしていません。 この場合、独自のエンティティ クラスを作成して適切な属性を追加する必要があります。

## <a name="errors-in-inheritance-relationships"></a>継承関係でのエラー

Q. オブジェクトリレーショナルデザイナーでツールボックスの継承図形を使用して2つのエンティティを接続しましたが、エラーが発生しました。

A: 関係を作成するだけでは不十分です。 識別子の列、基本クラスの識別子の値、派生クラスの識別子の値などの情報を提供する必要があります。

## <a name="provider-model"></a>プロバイダー モデル

Q. パブリック プロバイダー モデルを利用できますか?

A: いいえ。使用可能なパブリック プロバイダー モデルはありません。 現時点では、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は SQL Server と SQL Server Compact 3.5 のみをサポートしています。

## <a name="sql-injection-attacks"></a>SQL 注入攻撃

Q. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、SQL 注入攻撃からどのように保護されますか?

A: ユーザー入力の連結により形成される従来の SQL クエリでは、SQL 注入が大きなリスクでした。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] では、クエリで <xref:System.Data.SqlClient.SqlParameter> を使用することにより、そのような注入を防止します。 ユーザー入力はパラメーター値に変換されます。 この手法は、顧客の入力で悪意のあるコマンドが使用されることを防止します。

## <a name="changing-read-only-flag-in-dbml-files"></a>DBML ファイル内の読み取り専用フラグの変更

Q. DBML ファイルからオブジェクト モデルを作成するときに、どうすれば、特定のプロパティから setter を取り除くことができますか?

A: これは高度なシナリオですが、以下の手順に従ってください。

1. .dbml ファイル内で、<xref:System.Data.Linq.ITable.IsReadOnly%2A> フラグを `True` に変更することにより、プロパティを変更します。

2. 部分クラスを追加します。 読み取り専用メンバー用のパラメーターを使ってコンストラクターを作成します。

3. 既定の <xref:System.Data.Linq.Mapping.UpdateCheck> 値 (<xref:System.Data.Linq.Mapping.UpdateCheck.Never>) がアプリケーションにとって適切な値かどうかを検討します。

    > [!CAUTION]
    > Visual Studio でオブジェクトリレーショナルデザイナーを使用している場合は、変更が上書きされる可能性があります。

## <a name="aptca"></a>APTCA

Q. System.Data.Linq は、部分的に信頼されているコードが使用できるようにマークされていますか?

A: はい、<xref:System.Security.AllowPartiallyTrustedCallersAttribute> 属性でマークされた .NET Framework アセンブリの中にあります。 このマークがない場合、.NET Framework 内のアセンブリは、完全に信頼されたコードによってのみ使用されることを意図しています。

部分的に信頼された呼び出し元を許可する [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] の主なシナリオは、*信頼*の構成が Medium である Web アプリケーションから [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] アセンブリにアクセスできるようにすることです。

## <a name="mapping-data-from-multiple-tables"></a>複数のテーブルからのデータのマッピング

Q. エンティティに、複数のテーブルから取得されたデータが含まれています。 どのようにマップできますか?

A: データベース内にビューを作成して、エンティティをそのビューにマップすることができます。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、テーブル用と同じ SQL をビュー用に生成します。

> [!NOTE]
> このシナリオでのビューの使用には、制限があります。 この手法は、<xref:System.Data.Linq.Table%601> に対して実行される操作が、基になるビューでサポートされるような場合に、最も安全に機能します。 どのような操作が意図されるかは、開発者だけが知っています。 たとえば、ほとんどのアプリケーションは読み取り専用であり、もう1つの処理可能な数値は、ビューに対してストアドプロシージャを使用することによってのみ、`Create`/`Update`/`Delete` 操作を実行します。

## <a name="connection-pooling"></a>接続プール

Q. <xref:System.Data.Linq.DataContext> プールに役立つコンストラクトはありますか?

A: <xref:System.Data.Linq.DataContext> のインスタンスは再使用しないようにしてください。 <xref:System.Data.Linq.DataContext> はそれぞれ、特定の 1 つの編集/クエリ セッション用の状態 (ID キャッシュを含む) を保持します。 データベースの現在の状態に基づく新しいインスタンスを得るには、新しい <xref:System.Data.Linq.DataContext> を使用してください。

それでも、基になる ADO.NET 接続プールを使用できます。 詳しくは、「[SQL Server の接続プール (ADO.NET)](../../sql-server-connection-pooling.md)」をご覧ください。

## <a name="second-datacontext-is-not-updated"></a>2 番目の DataContext が更新されない

Q. データベース内の値を格納するために、<xref:System.Data.Linq.DataContext> の 1 つのインスタンスを使用しました。 しかし、同じデータベースに対する 2 番目の <xref:System.Data.Linq.DataContext> では、更新された値が反映されません。 2 番目の <xref:System.Data.Linq.DataContext> インスタンスは、キャッシュされた値を返すようです。

A: この動作は意図されたものです。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、最初のインスタンスと同じインスタンス/値を返し続けます。 更新の際には、オプティミスティック コンカレンシーが使用されます。 元のデータを使ってデータベースの現在の状態を検査し、実際に未変更であることをアサートします。 変更されている場合は競合が発生するため、アプリケーションでそれを解決する必要があります。 1 つのオプションとして、アプリケーションは、元の状態をデータベースの現在の状態にリセットして、更新を再試行できます。 詳細については、「[方法: 変更の競合を管理する](how-to-manage-change-conflicts.md)」を参照してください。

また、<xref:System.Data.Linq.DataContext.ObjectTrackingEnabled%2A> を false に設定することもできます。この場合、キャッシュと変更追跡が無効になります。 その後は、クエリを実行するたびに最新の値を取得できます。

## <a name="cannot-call-submitchanges-in-read-only-mode"></a>読み取り専用モードで SubmitChanges を呼び出すことができない

Q. 読み取り専用モードで <xref:System.Data.Linq.DataContext.SubmitChanges%2A> を呼び出そうとすると、エラーが発生します。

A: 読み取り専用モードでは、変更を追跡するコンテキスト機能は無効です。

## <a name="see-also"></a>関連項目

- [参照](reference.md)
- [トラブルシューティング](troubleshooting.md)
- [LINQ to SQL におけるセキュリティ](security-in-linq-to-sql.md)
