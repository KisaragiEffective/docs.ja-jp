---
title: using static ディレクティブ - C# リファレンス
ms.date: 03/10/2017
helpviewer_keywords:
- using static directive [C#]
ms.assetid: 8b8f9e34-c75e-469b-ba85-6f2eb4090314
ms.openlocfilehash: 55847aceb9fdf032ba533b82ee59be53761fa2c2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712950"
---
# <a name="using-static-directive-c-reference"></a>using static ディレクティブ (C# リファレンス)

`using static` ディレクティブは、型名を指定せずにアクセスできる静的メンバーおよび入れ子にされた型を指定します。 構文は次のとおりです。

```csharp
using static <fully-qualified-type-name>;
```

*fully-qualified-type-name* は、型名を指定せずに参照できる静的メンバーおよび入れ子にされた型の名前です。 完全修飾型名 (完全な名前空間名と型名) を指定しないと、C# によってコンパイラ エラー [CS0246](../compiler-messages/cs0246.md) "型または名前空間名 'type/namespace' が見つかりませんでした。using ディレクティブまたはアセンブリ参照が不足しています" が生成されます。

`using static` ディレクティブは、インスタンス メンバーがある場合でも、静的メンバーがあるすべての型 (または入れ子にされた型) に適用されます。 ただし、インスタンス メンバーは、型のインスタンスを通してのみ呼び出すことができます。

`using static` ディレクティブは、C# 6 で導入されました。

## <a name="remarks"></a>解説

通常は、静的メンバーを呼び出すときに、型名とメンバー名を指定します。 同じ型名を繰り返し入力してその型のメンバーを呼び出すと、コードが冗長でわかりにくくなる可能性があります。 たとえば、次の `Circle` クラスの定義は、<xref:System.Math> クラスのメンバー数を参照します。

[!code-csharp[using-static#1](~/samples/snippets/csharp/language-reference/keywords/using/using-static1.cs#1)]

メンバーが参照されるたびに <xref:System.Math> クラスを明示的に参照する必要がなくなれば、`using static` ディレクティブによって生成されるコードがより簡潔になります。

[!code-csharp[using-static#2](~/samples/snippets/csharp/language-reference/keywords/using/using-static2.cs#1)]

`using static` は、指定した型で宣言されているアクセス可能な静的メンバーおよび入れ子になった型のみをインポートします。  継承されたメンバーはインポートされません。  using static ディレクティブを使用して、Visual Basic モジュールを含む、名前付きの型からインポートすることができます。  F# の最上位の関数が、有効な C# 識別子を名前に持つ名前付きの型の静的メンバーとしてメタデータに存在する場合は、その F# 関数をインポートできます。

 `using static` を使用すると、指定した型で宣言された拡張メソッドを拡張メソッドの参照で使用できるようになります。  ただし、コード内で修飾なしで参照している場合は、拡張メソッドの名前はスコープにインポートされません。

 同じコンパイル単位または名前空間の多様な `using static` ディレクティブによってさまざまな型からインポートされた同じ名前を持つメソッドが、メソッド グループを形成します。  これらのメソッド グループ内のオーバーロードの解決方法は、C# の通常の規則に従います。

## <a name="example"></a>例

次の例では、`using static` ディレクティブを使用して、<xref:System.Console> クラス、<xref:System.Math> クラス、および <xref:System.String> クラスの静的メンバーを、型名を指定せずに使用できるようにしています。

[!code-csharp[using-static#3](~/samples/snippets/csharp/language-reference/keywords/using/using-static3.cs)]

上の例では、`using static` ディレクティブを <xref:System.Double> 型に適用することもできます。 それにより、型名を指定せずに、<xref:System.Double.TryParse(System.String,System.Double@)> メソッドを呼び出せるようになります。 ただし、どの数値型の `using static` メソッドが呼び出されたかを判断するために `TryParse` ステートメントを確認する必要が出てくるため、コードが読みにくくなります。

## <a name="see-also"></a>参照

- [using ディレクティブ](using-directive.md)
- [C# リファレンス](../index.md)
- [C# のキーワード](index.md)
- [名前空間の使用](../../programming-guide/namespaces/using-namespaces.md)
- [名前空間](../../programming-guide/namespaces/index.md)
