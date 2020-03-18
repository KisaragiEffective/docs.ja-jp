---
title: group 句 - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- group
- group_CSharpKeyword
helpviewer_keywords:
- group keyword [C#]
- group clause [C#]
ms.assetid: c817242e-b12c-4baa-a57e-73ee138f34d1
ms.openlocfilehash: 75a366ec24e4e48af7e87d3372950aad8d76435b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713469"
---
# <a name="group-clause-c-reference"></a>group 句 (C# リファレンス)

`group` 句は、グループのキー値に一致する 0 個以上の項目を含む <xref:System.Linq.IGrouping%602> オブジェクトのシーケンスを返します。 たとえば、各文字列の最初の文字に基づいて文字列のシーケンスをグループ化することができます。 この場合、最初の文字がキーで、型は [char](../builtin-types/char.md) であり、各 `Key` オブジェクトの <xref:System.Linq.IGrouping%602> プロパティに格納されています。 コンパイラは、キーの型を推論します。

次の例で示すように、クエリ式は `group` 句で終了できます。

[!code-csharp[cscsrefQueryKeywords#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#10)]

各グループで追加のクエリ操作を実行する場合、[into](into.md) コンテキスト キーワードを使用して一時的な識別子を指定できます。 `into` を使用する場合、次の抜粋に示すように、クエリを続行し、最終的には `select` ステートメントまたは別の `group` 句でそれを終了する必要があります。

[!code-csharp[cscsrefQueryKeywords#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#11)]

この記事の「例」のセクションでは、`group` を含む場合と含まない場合の `into` の使用方法の完全な例があります。

## <a name="enumerating-the-results-of-a-group-query"></a>グループ クエリの結果を列挙する

<xref:System.Linq.IGrouping%602> クエリによって生成される `group` オブジェクトは基本的には、リストのリストであるため、各グループのアイテムにアクセスするには、入れ子になった [foreach](foreach-in.md) ループを使用する必要があります。 外側のループがグループ キーを反復処理し、内側のループがグループ自体の各項目を反復処理します。 グループには、キーがある場合はありますが、要素はありません。 次に、前のコード例でクエリを実行する `foreach` ループを示します。

[!code-csharp[cscsrefQueryKeywords#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#12)]

## <a name="key-types"></a>キーの種類

グループ キーは、文字列、組み込みの数値型、またはユーザー定義の名前付きの型または匿名型など、任意の型にすることができます。

### <a name="grouping-by-string"></a>文字列でグループ化する

前のコード例では、`char` を使用していました。 姓を完全に指定するなど、簡単に代わりに文字列のキーを指定できます。

[!code-csharp[cscsrefQueryKeywords#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#13)]

### <a name="grouping-by-bool"></a>ブールでグループ化する

次の例では、結果を 2 つのグループに分割するためのキー用のブール値の用途を示します。 値は `group` 句のサブ式で生成されることに注意してください。

[!code-csharp[cscsrefQueryKeywords#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#14)]

### <a name="grouping-by-numeric-range"></a>数値の範囲でグループ化する

次の例では、パーセンタイルの範囲を示す数値のグループ キーを作成する式を使用しています。 [ 句でメソッドを 2 度呼び出さなくて済むように、メソッド呼び出しの結果を格納する便利な場所として ](let-clause.md)let`group` を使用できます。 クエリ式でメソッドを安全に使用する方法の詳細については、「[クエリ式の例外の処理](../../linq/handle-exceptions-in-query-expressions.md)」を参照してください。

[!code-csharp[cscsrefQueryKeywords#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#15)]

### <a name="grouping-by-composite-keys"></a>複合キーでグループ化する

1 つ以上のキーを使用して要素をグループ化するには、複合キーを使用します。 複合キーは、キー要素を保持する、匿名型または名前付きの型を使用して作成できます。 次の例では、クラス `Person` が、`surname` と `city` という名前のメンバーで宣言されていると想定しています。 `group` 句により、同じ名前と同じ市の人物のセットごとに、別のグループが作成されます。

```csharp
group person by new {name = person.surname, city = person.city};
```

クエリ変数を別のメソッドに渡す場合には、名前付きの型を使用します。 キーの自動実装プロパティを使用して特殊クラスを作成し、<xref:System.Object.Equals%2A> メソッドと <xref:System.Object.GetHashCode%2A> メソッドをオーバーライドします。 これらのメソッドを厳密にオーバーライドする必要がない構造体を使用することも可能です。 詳細については、「[自動実装するプロパティを使用して簡易クラスを実装する方法](../../programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties.md)」と「[ディレクトリ ツリーで重複するファイルを照会する方法](../../programming-guide/concepts/linq/how-to-query-for-duplicate-files-in-a-directory-tree-linq.md)」を参照してください。 後述の記事には、名前付きの型を複合キーで使用する方法のコード例があります。

## <a name="example"></a>例

次の例では、グループにその他のクエリ ロジックが適用されていない場合、ソース データをグループに並べる標準的なパターンを示します。 これは連結なしのグループ化と呼ばれます。 文字列の配列の要素は、最初の文字でグループ化されます。 クエリの結果は、型 <xref:System.Linq.IGrouping%602> のパブリック `Key` プロパティを含む `char` 型とグループに各項目を含む <xref:System.Collections.Generic.IEnumerable%601> コレクションです。

`group` 句の結果は、シーケンスのシーケンスです。 そのため、返される各グループ内の各要素にアクセスするには、次の例のように、グループ キーを反復処理するループ内で入れ子になった `foreach` ループを使用します。

[!code-csharp[cscsrefQueryKeywords#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#16)]

## <a name="example"></a>例

この例では、作成後に、*と共に*continuation`into` を使用し、グループに追加のロジックを実行する方法を示します。 詳しくは、「[into](into.md)」をご覧ください。 次の例では、キー値が母音であるものだけを選択するために各グループに問い合せを行います。

[!code-csharp[cscsrefQueryKeywords#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#17)]

## <a name="remarks"></a>解説

コンパイル時に `group` 句が <xref:System.Linq.Enumerable.GroupBy%2A> メソッドの呼び出しに変換されます。

## <a name="see-also"></a>参照

- <xref:System.Linq.IGrouping%602>
- <xref:System.Linq.Enumerable.GroupBy%2A>
- <xref:System.Linq.Enumerable.ThenBy%2A>
- <xref:System.Linq.Enumerable.ThenByDescending%2A>
- [クエリ キーワード](query-keywords.md)
- [統合言語クエリ (LINQ)](../../linq/index.md)
- [入れ子になったグループの作成](../../linq/create-a-nested-group.md)
- [クエリ結果のグループ化](../../linq/group-query-results.md)
- [グループ化操作でのサブクエリの実行](../../linq/perform-a-subquery-on-a-grouping-operation.md)
