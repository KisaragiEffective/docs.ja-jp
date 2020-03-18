---
title: 文字列を数値に変換する方法 - C# プログラミング ガイド
ms.date: 02/11/2019
helpviewer_keywords:
- conversions [C#]
- conversions [C#], string to int
- converting strings to int [C#]
- strings [C#], converting to int
ms.assetid: 467b9979-86ee-4afd-b734-30299cda91e3
ms.openlocfilehash: 54a4562a5cc493fc287bdf2f6bcf9723557f2a05
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79157040"
---
# <a name="how-to-convert-a-string-to-a-number-c-programming-guide"></a>文字列を数値に変換する方法 (C# プログラミング ガイド)

さまざまな数値型 (`int`、`long`、`double` など) にある `Parse` または `TryParse` メソッドを呼び出すか <xref:System.Convert?displayProperty=nameWithType> クラスにあるメソッドを使用して、[文字列](../../language-reference/builtin-types/reference-types.md)を数値に変換できます。  
  
 文字列では、`TryParse` メソッド (たとえば [`int.TryParse("11", out number)`](xref:System.Int32.TryParse%2A)) または `Parse` メソッド (たとえば [`var number = int.Parse("11")`](xref:System.Int32.Parse%2A)) を呼び出すのがいくらか効率的で簡単です。  <xref:System.IConvertible> を実装している一般的なオブジェクトでは、<xref:System.Convert> メソッドを使用するのがより便利です。  
  
 文字列に含まれていると思われる数値型 (<xref:System.Int32?displayProperty=nameWithType> 型など) の `Parse` または `TryParse` メソッドを使用できます。  <xref:System.Convert.ToInt32%2A?displayProperty=nameWithType> メソッドは、<xref:System.Int32.Parse%2A> を内部的に使用します。  `Parse` メソッドは、変換された数値を返します。`TryParse` メソッドは、変換に成功したかどうかを示す <xref:System.Boolean> 値を返し、変換された数値を [`out` パラメーター](../../language-reference/keywords/out.md)内で返します。 文字列の形式が無効である場合、`Parse` では例外がスローされるのに対し、`TryParse` では `false` が返されます。 `Parse` メソッドを呼び出すときに、常に例外処理を使用して、解析操作が失敗した場合に <xref:System.FormatException> をキャッチする必要があります。  
  
## <a name="calling-the-parse-and-tryparse-methods"></a>Parse メソッドと TryParse メソッドの呼び出し

文字列の先頭と末尾の空白文字は、`Parse` および `TryParse` メソッドによって無視されますが、その他のすべての文字は、適切な数値型 (`int`、`long`、`ulong`、`float`、`decimal` など) を形成する文字である必要があります。  数値を形成する文字列内に空白文字があると、エラーになります。  たとえば、"10"、"10.3"、または "  10  " を解析するために `decimal.TryParse` を使用することはできますが、"10X"、"1 0" (埋め込みスペースに注意)、"10 .3" (埋め込みスペースに注意)、"10e1" (この場合は `float.TryParse` を使用) などからこのメソッドを使用して 10 を解析することはできません。 さらに、値が `null` または <xref:System.String.Empty?displayProperty=nameWithType> の文字列は正常に解析できません。 <xref:System.String.IsNullOrEmpty%2A?displayProperty=nameWithType> メソッドを呼び出すことで、解析を試みる前に null または空の文字列を確認できます。

次の例は、`Parse` および `TryParse` の呼び出しの成功例と失敗例の両方を示しています。  
  
[!code-csharp[Parse and TryParse](~/samples/snippets/csharp/programming-guide/string-to-number/parse-tryparse/program.cs)]  

次の例は、先頭に数字 (16 進数文字を含む)、末尾に数字以外の文字を含むと予想される文字列を解析する 1 つのアプローチを示しています。 <xref:System.Int32.TryParse%2A> メソッドを呼び出す前に、文字列の先頭から新しい文字列に有効な文字を割り当てます。 解析する文字列には少数の文字が含まれるので、例では <xref:System.String.Concat%2A?displayProperty=nameWithType> メソッドを呼び出して新しい文字列に有効な文字を割り当てます。 より大きな文字列の場合は、代わりに <xref:System.Text.StringBuilder> クラスを使用できます。
  
[!code-csharp[Removing invalid characters](~/samples/snippets/csharp/programming-guide/string-to-number/parse-tryparse2/program.cs)]  

## <a name="calling-the-convert-methods"></a>変換メソッドの呼び出し

文字列を数値に変換するために使用できる <xref:System.Convert> クラスのメソッドの一部を次の表に示します。  
  
|数値型|メソッド|  
|------------------|------------|  
|`decimal`|<xref:System.Convert.ToDecimal%28System.String%29>|  
|`float`|<xref:System.Convert.ToSingle%28System.String%29>|  
|`double`|<xref:System.Convert.ToDouble%28System.String%29>|  
|`short`|<xref:System.Convert.ToInt16%28System.String%29>|  
|`int`|<xref:System.Convert.ToInt32%28System.String%29>|  
|`long`|<xref:System.Convert.ToInt64%28System.String%29>|  
|`ushort`|<xref:System.Convert.ToUInt16%28System.String%29>|  
|`uint`|<xref:System.Convert.ToUInt32%28System.String%29>|  
|`ulong`|<xref:System.Convert.ToUInt64%28System.String%29>|  
  
 次の例では、<xref:System.Convert.ToInt32%28System.String%29?displayProperty=nameWithType> メソッドを呼び出して、入力文字列を [int](../../language-reference/builtin-types/integral-numeric-types.md) に変換します。例では、このメソッドからスローされる可能性のある最も一般的な 2 種類の例外 (<xref:System.FormatException> と <xref:System.OverflowException>) をキャッチします。 <xref:System.Int32.MaxValue?displayProperty=nameWithType> を超えずに結果の数値を増やすことができる場合、例では結果に 1 を加算し、出力を表示します。  
  
[!code-csharp[Parsing with Convert methods](~/samples/snippets/csharp/programming-guide/string-to-number/convert/program.cs)]  
  
## <a name="see-also"></a>関連項目

- [型](./index.md)
- [文字列が数値を表しているかどうかを確認する方法](../strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (C#)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs)
