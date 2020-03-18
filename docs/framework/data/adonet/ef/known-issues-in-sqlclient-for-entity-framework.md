---
title: Entity Framework 用の .NET Framework Data Provider for SQL Server (SqlClient) の既知の問題
ms.date: 03/30/2017
ms.assetid: 48fe4912-4d0f-46b6-be96-3a42c54780f6
ms.openlocfilehash: 32a1dd22111498ab5b3b75940f5485b2957367e8
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452501"
---
# <a name="known-issues-in-sqlclient-for-entity-framework"></a>Entity Framework 用の .NET Framework Data Provider for SQL Server (SqlClient) の既知の問題
ここでは、.NET Framework Data Provider for SQL Server (SqlClient) に関連する既知の問題について説明します。  
  
## <a name="trailing-spaces-in-string-functions"></a>文字列関数の末尾のスペース  
 SQL Server は、文字列値の末尾のスペースを無視します。 そのため、文字列の末尾にスペースを挿入すると、予期できない結果が生じたり、場合によってはエラーが発生することもあります。  
  
 文字列の末尾にスペースを入れる必要がある場合は、SQL Server が文字列をトリミングしないように、末尾に空白文字を追加することを検討してください。 末尾のスペースが不要である場合は、クエリ パイプラインに渡す前にスペースを削除してください。  
  
## <a name="right-function"></a>RIGHT 関数  
 `null`, 0`RIGHT(nvarchar(max)` または `)`, 0`RIGHT(varchar(max)` に、最初の引数として `)` 以外の値を、2 番目の引数として 0 を渡すと、`NULL` 文字列の代わりに `empty` 値が返されます。  
  
## <a name="cross-and-outer-apply-operators"></a>CROSS APPLY 演算子および OUTER APPLY 演算子  
 SQL Server 2005 では、クロスおよび外部 APPLY 演算子が導入されました。 場合によっては、クエリ パイプラインにより、CROSS APPLY 演算子または OUTER APPLY 演算子を含む Transact-SQL ステートメントが生成されることがあります。 SQL Server 2005 より前の SQL Server のバージョンを含む一部のバックエンドプロバイダーは、これらの演算子をサポートしていないため、これらのバックエンドプロバイダーでクエリを実行することはできません。  
  
 CROSS APPLY 演算子または OUTER APPLY 演算子を含むクエリの生成につながる可能性がある一般的なシナリオを次に示します。  
  
- ページングを使用した相関サブクエリ  
  
- 相関サブクエリ全体、またはナビゲーションによって生成されたコレクション全体を対象とした `AnyElement`  
  
- 要素セレクターを受け取るグループ化メソッドを使用する LINQ クエリ  
  
- CROSS APPLY 演算子または OUTER APPLY 演算子が明示的に指定されたクエリ  
  
- REF コンストラクターを引数に取る DEREF コンストラクターを含むクエリ。  
  
## <a name="skip-operator"></a>SKIP 演算子  
 SQL Server 2000 を使用している場合は、非キー列で ORDER BY を使用して SKIP を使用すると、正しくない結果が返される可能性があります。 キー以外の列に重複するデータが存在する場合、指定された数を超える行はスキップされます。 これは、SQL Server 2000 に対する SKIP の変換方法によるものです。 たとえば、次のクエリでは、`E.NonKeyColumn` の値が重複している場合、5行を超える行がスキップされる可能性があります。  
  
```sql  
SELECT [E] FROM Container.EntitySet AS [E] ORDER BY [E].[NonKeyColumn] DESC SKIP 5L  
```  
  
## <a name="targeting-the-correct-sql-server-version"></a>適切なバージョンの SQL Server を対象としたクエリの実行  
 Entity Framework は、ストレージモデル (.ssdl) ファイルの Schema 要素の `ProviderManifestToken` 属性に指定されている SQL Server バージョンに基づく Transact-sql クエリを対象としています。 このバージョンは、実際に接続する SQL Server のバージョンとは異なる場合があります。 たとえば、SQL Server 2005 を使用していて、`ProviderManifestToken` 属性が2008に設定されている場合、生成された Transact-sql クエリはサーバーで実行されない可能性があります。 たとえば、SQL Server 2008 で導入された新しい日付と時刻の型を使用するクエリは、以前のバージョンの SQL Server では実行されません。 SQL Server 2005 を使用していても、`ProviderManifestToken` 属性が2000に設定されている場合、生成された Transact-sql クエリの最適化が低下するか、クエリがサポートされていないという例外が発生する可能性があります。 詳細については、このトピックの「CROSS APPLY 演算子および OUTER APPLY 演算子」を参照してください。  
  
 データベースの一部の動作は、データベースの互換性レベルの設定に依存します。 `ProviderManifestToken` 属性が2005に設定されていて、SQL Server バージョンが2005であるにもかかわらず、データベースの互換性レベルが "80" (SQL Server 2000) に設定されている場合、生成された Transact-sql は SQL Server 2005 を対象としますが、互換性レベルの設定により予期したとおりに実行されない可能性があります。 たとえば、ORDER BY リストの列名とセレクターの列名が一致する場合、順序付けが失われることがあります。  
  
## <a name="nested-queries-in-projection"></a>投影内の入れ子になったクエリ  
 projection 句内の入れ子になったクエリは、サーバーでデカルト積に変換されないことがあります。 SQL Server を含む一部のバックエンドサーバーでは、TempDB テーブルのサイズが非常に大きくなる可能性があります。 サーバーのパフォーマンスが低下する可能性があります。  
  
 projection 句内の入れ子になったクエリの例を次に示します。  
  
```sql  
SELECT c, (SELECT c, (SELECT c FROM AdventureWorksModel.Vendor AS c  ) As Inner2 FROM AdventureWorksModel.JobCandidate AS c  ) As Inner1 FROM AdventureWorksModel.EmployeeDepartmentHistory AS c  
```  
  
## <a name="server-generated-guid-identity-values"></a>サーバー生成の GUID ID 値  
 Entity Framework は、サーバーで生成された GUID 型の id 値をサポートしますが、プロバイダーは、行が挿入された後にサーバーで生成された id 値を返すことをサポートする必要があります。 SQL Server 2005 以降では、SQL Server データベースのサーバー生成 GUID 型を[OUTPUT 句](/sql/t-sql/queries/output-clause-transact-sql)を使用して返すことができます。
  
## <a name="see-also"></a>参照

- [Entity Framework 用 SqlClient](sqlclient-for-the-entity-framework.md)
- [LINQ to Entities の既知の問題および注意点](./language-reference/known-issues-and-considerations-in-linq-to-entities.md)
