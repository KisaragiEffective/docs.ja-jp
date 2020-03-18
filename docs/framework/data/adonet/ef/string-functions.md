---
title: 文字列関数
ms.date: 03/30/2017
ms.assetid: 338f0c26-8aee-43eb-bd1a-ec0849a376b9
ms.openlocfilehash: 7680c3de4740ce3241c1273eaf7af528fb765a44
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452449"
---
# <a name="string-functions"></a>文字列関数
.NET Framework Data Provider for SQL Server (SqlClient) には、入力された `String` に対して操作を実行し、その結果を `String` または数値として返す `String` 関数が用意されています。 これらの関数は、SqlClient の SqlServer 名前空間に存在します。 Entity Framework は、プロバイダーの名前空間プロパティを使用することにより、型や関数など、特定のコンストラクターに対してこのプロバイダーによってどのプレフィックスが使用されているかを特定できます。  
  
 次の表に、SqlClient の `String` 関数を示します。  
  
|関数|説明|  
|--------------|-----------------|  
|`ASCII(expression)`|文字列式の左端の文字の ASCII コード値を返します。<br /><br /> **引数**<br /><br /> `expression`: ASCII `String` 型の任意の有効な式。<br /><br /> **戻り値**<br /><br /> `Int32`。<br /><br /> **例**<br /><br /> `SqlServer.ASCII('A')`|  
|`CHAR(expression)`|`Int32` コードを ASCII String に変換します。<br /><br /> **引数**<br /><br /> `expression`: `Int32`。<br /><br /> **戻り値**<br /><br /> ASCII の `String`。<br /><br /> **例**<br /><br /> `SqlServer.char(97)`|  
|`CHARINDEX(expression1, expression2 [, start_location])`|指定された式の、文字列内での開始位置を返します。<br /><br /> **引数**<br /><br /> `expression1`: 検索する文字列を含む式。 String (ASCII または Unicode) 型または Binary 型の式を指定できます。<br /><br /> `expression2`: 式。通常は、文字列を検索する対象の列を指定します。 String (ASCII または Unicode) 型または Binary 型の式を指定できます。<br /><br /> `start_location`:(省略可能) expression2 で expression1 の検索を開始するための文字位置を表す Int64 (SQL Server 2000 では返されません) または Int32。 start_location を指定しなかった場合、または負の値や 0 を指定した場合は、expression2 の先頭から検索が開始されます。<br /><br /> **戻り値**<br /><br /> `Int32`。<br /><br /> **例**<br /><br /> `SqlServer.CHARINDEX('h', 'habcdefgh', 2)`|  
|`DIFFERENCE(expression, expression)`|2 つの文字列の `SOUNDEX` 値を比較し、両者の類似性を評価します。<br /><br /> **引数**<br /><br /> ASCII または Unicode の `String` 型。 `expression` 引数には定数、変数、または列を指定できます。<br /><br /> **戻り値**<br /><br /> 2 つの文字式の SOUNDEX 値の差を表す `Int32` を返します。 値の範囲は 0 ～ 4 です。 0 は類似性が低いか類似性がないことを示し、4 は類似性が高いか同じ値であることを示します。<br /><br /> **例**<br /><br /> `// The following example returns a DIFFERENCE value of 4,`<br /><br /> `//the least possible difference or the best match.`<br /><br /> `SqlServer.DIFFERENCE('Green','Greene');`|  
|`LEFT(expression, count)`|文字列の左端から、指定された数だけ文字を返します。<br /><br /> **引数**<br /><br /> `expression`: Unicode または ASCII の String 型。 CAST 関数を使用して、明示的に character_expression を変換します。<br /><br /> `count`: character_expression から取得する文字数を指定する `Int64` (SQL Server 2000 では返されません) または `Int32` 型。<br /><br /> **戻り値**<br /><br /> Unicode または ASCII の `String`。<br /><br /> **例**<br /><br /> `SqlServer.LEFT('SQL Server', 4)`|  
|`LEN(expression)`|指定された文字列式の、末尾の空白を除いた文字数を返します。<br /><br /> **引数**<br /><br /> `expression`: `String` (Unicode または ASCII) 型または `Binary` 型の式。<br /><br /> **戻り値**<br /><br /> `Int32`。<br /><br /> **例**<br /><br /> `SqlServer.LEN('abcd')`|  
|`LOWER(expression)`|大文字が小文字に変換された状態の `String` 式を返します。<br /><br /> **引数**<br /><br /> `expression`: `String` 型の任意の有効な式。<br /><br /> **戻り値**<br /><br /> `String` です。<br /><br /> **例**<br /><br /> `SqlServer.LOWER('AbB')`|  
|`LTRIM(expression)`|先頭のスペースを削除した後の `String` 式を返します。<br /><br /> **引数**<br /><br /> `expression`: `String` 型の任意の有効な式。<br /><br /> **戻り値**<br /><br /> `String` です。<br /><br /> **例**<br /><br /> `SqlServer.LTRIM('   d')`|  
|`NCHAR(expression)`|Unicode 標準の定義に従って、指定された整数コードの Unicode の `String` を返します。<br /><br /> **引数**<br /><br /> `expression`: `Int32`。<br /><br /> **戻り値**<br /><br /> Unicode の `String`。<br /><br /> **例**<br /><br /> `SqlServer.NCHAR(65)`|  
|`PATINDEX('%pattern%', expression)`|指定された `String` 式の中で、特定のパターンが最初に現れる先頭位置を返します。<br /><br /> **引数**<br /><br /> `'%pattern%'`: ASCII または Unicode の `String` 型。 ワイルドカード文字も使用できます。ただし、パターンの前後に % 文字が必要です (先頭の文字または末尾の文字を検索する場合は除く)。<br /><br /> `expression`: 指定パターンの検索対象となる ASCII または Unicode の `String`。<br /><br /> **戻り値**<br /><br /> `Int32`。<br /><br /> **例**<br /><br /> `SqlServer.PATINDEX('abc', 'ab')`|  
|`QUOTENAME('char_string' [, 'quote_char'])`|Unicode の `String` に区切り記号を追加して返すことで、入力文字列から区切り記号で囲まれた有効な SQL Server 2005 識別子を作成します。<br /><br /> **引数**<br /><br /> `char_string`: Unicode の `String`。<br /><br /> `quote_char`: 区切り記号として使用する 1 つの文字を指定します。 単一引用符 (')、左または右の角かっこ ([ ])、二重引用符 (") のいずれかを指定できます。 `quote_char` を指定しない場合は、角かっこが使用されます。<br /><br /> **戻り値**<br /><br /> Unicode の `String`。<br /><br /> **例**<br /><br /> `SqlServer.QUOTENAME('abc[]def')`|  
|`REPLACE(expression1, expression2, expression3)`|文字式を別の文字式に置き換えます。<br /><br /> **引数**<br /><br /> `expression1`: 検索範囲となる文字列式。 `expression1` には、Unicode または ASCII の文字列型を指定できます。<br /><br /> `expression2`: 検索する部分文字列。 `expression2` には、Unicode または ASCII の文字列型を指定できます。<br /><br /> `expression3`: 置換文字列。 `expression3` には、Unicode または ASCII の文字列型を指定できます。<br /><br /> **例**<br /><br /> `SqlServer.REPLACE('aabbcc', 'bc', 'zz')`|  
|`REPLICATE(char_expression, int_expression)`|文字式を指定された回数繰り返します。<br /><br /> **引数**<br /><br /> `char_expression`: Unicode または ASCII の `String` 型。<br /><br /> `int_expression`: `Int64` (SQL Server 2000 ではサポートされません) または `Int32`。<br /><br /> **戻り値**<br /><br /> Unicode または ASCII の `String` 型。<br /><br /> **例**<br /><br /> `SqlServer.REPLICATE('aa',2)`|  
|`REVERSE(expression)`|入力文字列の文字位置を逆転した Unicode または ASCII の String を返します。<br /><br /> **引数**<br /><br /> `expression`: Unicode または ASCII の `String` 型。<br /><br /> **戻り値**<br /><br /> Unicode または ASCII の `String` 型。<br /><br /> **例**<br /><br /> `SqlServer.REVERSE('abcd')`|  
|`RIGHT(char_expression, count)`|文字列の右端から指定された数の文字を返します。<br /><br /> **引数**<br /><br /> `char_expression`: Unicode または ASCII の文字列型。 CAST 関数を使用して、明示的に character_expression を変換します。<br /><br /> `count`: character_expression から取得する文字数を指定する `Int64` (SQL Server 2000 では返されません) または `Int32` 型。<br /><br /> **戻り値**<br /><br /> ASCII の `String` 型。<br /><br /> **例**<br /><br /> `SqlServer.RIGHT('SQL Server', 6)`|  
|`RTRIM(expression)`|末尾のスペースを削除した後の Unicode または ASCII の String を返します。<br /><br /> **引数**<br /><br /> `expression`: Unicode または ASCII の `String` 型。<br /><br /> **戻り値**<br /><br /> Unicode または ASCII の `String` 型。<br /><br /> **例**<br /><br /> `SqlServer.RTRIM('   d   e      ')`|  
|`SOUNDEX(expression)`|2つの文字列の類似性を評価する4文字 (SOUNDEX) コードを返します。**引数**<br /><br /> `expression`: Unicode または ASCII の String 型。<br /><br /> **戻り値**<br /><br /> ASCII の `String`。 4 文字の (SOUNDEX) コードは、2 つの文字列の類似性を評価する文字列です。<br /><br /> **例**<br /><br /> `Select SqlServer.SOUNDEX('Smith'), SqlServer.SOUNDEX('Smythe') FROM {1}`<br /><br /> **戻り値**<br /><br /> `----- -----  S530  S530`|  
|`SPACE(int_expression)`|連続する空白文字で構成される ASCII の `String` を返します。<br /><br /> **引数**<br /><br /> `int_expression`: 空白文字の数を表す `Int64` (SQL Server 2000 では返されません) または `Int32`。<br /><br /> **戻り値**<br /><br /> ASCII の `String`。<br /><br /> **例**<br /><br /> `SqlServer.SPACE(2)`|  
|`STR(float_expression [, length [, decimal]])`|数値データから変換された ASCII の `String` を返します。<br /><br /> **引数**<br /><br /> `float _expression`: 小数点付きの概数型 (`Double`) の式。<br /><br /> `length`: (省略可) 全体の長さを表す `Int32`。 これには小数点、負号、数字、空白文字も含まれます。 既定値は 10 です。<br /><br /> `decimal`:(省略可能) 小数点の右側の桁数を表す `Int32`。 16 以下である必要があります。 decimal に 16 を超える値を指定した場合、結果は小数点以下 16 桁に切り捨てられます。<br /><br /> **戻り値**<br /><br /> ASCII の `String`。<br /><br /> **例**<br /><br /> `SqlServer.STR(212.0)`|  
|`STUFF(str_expression, start, length, str_expression_to_insert)`|文字列式内で、指定された長さ分の文字を削除し、指定された開始位置に別の文字列を挿入します。<br /><br /> **引数**<br /><br /> `str_expression`: Unicode または ASCII の `String`。<br /><br /> `Int64` (SQL Server 2000 では返されません)、または削除と挿入を開始する場所を指定する `Int32` 値 `start:` ます。<br /><br /> `length`: 削除する文字数を指定する `Int64` 型 (SQL Server 2000 では返されません) または `Int32` 型の値。<br /><br /> `str_expression_to_insert`: Unicode または ASCII の `String`。<br /><br /> **戻り値**<br /><br /> Unicode または ASCII の `String`。<br /><br /> **例**<br /><br /> `SqlServer.STUFF('abcd', 2, 2, 'zz')`|  
|`SUBSTRING(str_expression, start, length)`|`String` 式の一部分を返します。<br /><br /> **引数**<br /><br /> `str_expression`: `String` (ASCII または Unicode) 型または `Binary` 型の式。<br /><br /> `start`: 部分文字列の取得開始位置を指定する `Int64` ( SQL Server 2000 では返されません) または `Int32`。 1 は文字列の先頭文字を表します。<br /><br /> `length`: 式から取得する文字数を指定する `Int64` (SQL Server 2000 では返されません) または `Int32` 型。<br /><br /> **戻り値**<br /><br /> `String` (ASCII または Unicode) 型または `Binary` 型。<br /><br /> **例**<br /><br /> `SqlServer.SUBSTRING('abcd', 2, 2)`|  
|`UNICODE(expression)`|Unicode 標準に定義された、入力式の先頭文字の整数値を返します。<br /><br /> **引数**<br /><br /> `expression`: Unicode の `String`。<br /><br /> **戻り値**<br /><br /> `Int32`。<br /><br /> **例**<br /><br /> `SqlServer.UNICODE('a')`|  
|`UPPER(expression)`|小文字が大文字に変換された状態の `String` 式を返します。<br /><br /> **引数**<br /><br /> `expression`: ASCII または Unicode の String 型の式。<br /><br /> **戻り値**<br /><br /> ASCII または Unicode の `String` 型。<br /><br /> **例**<br /><br /> `SqlServer.UPPER('AbB')`|  
  
 SqlClient でサポートされる `String` 関数の詳細については、「[文字列関数 (transact-sql)](/sql/t-sql/functions/string-functions-transact-sql)」を参照してください。 
  
## <a name="see-also"></a>参照

- [Entity Framework 用 SqlClient 関数](sqlclient-for-ef-functions.md)
- [Entity Framework 用の .NET Framework Data Provider for SQL Server (SqlClient) の既知の問題](known-issues-in-sqlclient-for-entity-framework.md)
