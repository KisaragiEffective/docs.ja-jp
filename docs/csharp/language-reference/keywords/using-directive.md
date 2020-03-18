---
title: using ディレクティブ - C# リファレンス
ms.date: 07/20/2015
helpviewer_keywords:
- using directive [C#]
ms.assetid: b42b8e61-5e7e-439c-bb71-370094b44ae8
ms.openlocfilehash: 4f7ddad8c3dc12391ef6bf345a73ebb384400b38
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77093150"
---
# <a name="using-directive-c-reference"></a>using ディレクティブ (C# リファレンス)

`using` ディレクティブは、次の 3 つの用途で使用します。

- 名前空間で型の使用を許可する場合。これにより、その名前空間内では型を修飾せずに使用できます。

    ```csharp
    using System.Text;
    ```

- アクセスを型名で修飾することなく、型の静的メンバーおよび入れ子にされた型へのアクセスを許可する場合。

    ```csharp
    using static System.Math;
    ```

    詳細については、「[using static ディレクティブ](using-static.md)」を参照してください。

- 名前空間または型のエイリアスを作成する場合。 これは "*using エイリアス ディレクティブ*" と呼ばれます。

    ```csharp
    using Project = PC.MyCompany.Project;
    ```

`using` キーワードは、*using ステートメント*の作成にも使用します。ファイルやフォントなどの <xref:System.IDisposable> オブジェクトを正しく処理できるようになります。 詳細については、「[using ステートメント](using-statement.md)」を参照してください。

## <a name="using-static-type"></a>using static 型

アクセスを型名で修飾することなく型の静的メンバーにアクセスできます。

```csharp
using static System.Console;
using static System.Math;
class Program
{
    static void Main()
    {
        WriteLine(Sqrt(3*3 + 4*4));
    }
}
```

## <a name="remarks"></a>解説

`using` ディレクティブのスコープは、このディレクティブが存在するファイルに限定されます。

`using` ディレクティブは、次のように記述することができます。

- ソース コード ファイルの先頭、任意の名前空間または型定義の前。
- 任意の名前空間内、ただし、この名前空間内で宣言された任意の名前空間または型の前。

それ以外の場合は、コンパイラ エラー [CS1529](../../misc/cs1529.md) が生成されます。

`using` 別名ディレクティブを作成すると、名前空間または型の識別子を修飾しやすくなります。 いずれの `using` ディレクティブにおいても、前に置かれる `using` に関係なく、完全に修飾された名前空間または型を使用する必要があります。 `using` ディレクティブの宣言内では、`using` 別名を使用することができません。 たとえば、次の場合はコンパイラ エラーが生成されます。

```csharp
using s = System.Text;
using s.RegularExpressions; // Generates a compiler error.
```

`using` ディレクティブを作成すると、名前空間内の型を、名前空間を指定することなく使用できます。 `using` ディレクティブでは、指定した名前空間に入れ子になった別の名前空間へのアクセスは許可されません。

名前空間は、ユーザー定義とシステム定義の 2 つのカテゴリに分類されます。 ユーザー定義の名前空間は、コードで定義された名前空間です。 システム定義の名前空間の一覧については、「[.NET API ブラウザー](../../../../api/index.md)」を参照してください。

## <a name="example-1"></a>例 1

次の例は、名前空間の `using` エイリアスを定義して使用する方法を示しています。

[!code-csharp[csrefKeywordsNamespace#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace2.cs#8)]

using エイリアス ディレクティブの右側には、オープン ジェネリック型を配置できません。 たとえば、`List<T>` の using エイリアスを作成することはできませんが、`List<int>` の using エイリアスは作成できます。

## <a name="example-2"></a>例 2

次の例は、クラスの `using` ディレクティブと `using` エイリアスを定義する方法を示しています。

[!code-csharp[csrefKeywordsNamespace#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace2.cs#9)]

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/namespaces.md#using-directives)に関するページの [using ディレクティブ](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [名前空間の使用](../../programming-guide/namespaces/using-namespaces.md)
- [C# のキーワード](index.md)
- [名前空間](../../programming-guide/namespaces/index.md)
- [using ステートメント](using-statement.md)
