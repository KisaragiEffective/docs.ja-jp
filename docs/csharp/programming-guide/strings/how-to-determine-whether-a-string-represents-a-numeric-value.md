---
title: 文字列が数値を表しているかどうかを確認する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- numeric strings [C#]
- validating numeric input [C#]
- strings [C#], numeric
ms.assetid: a4e84e10-ea0a-489f-a868-503dded9d85f
ms.openlocfilehash: 15a21a6298f8f0a57e0189554246202b220dd259
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79157066"
---
# <a name="how-to-determine-whether-a-string-represents-a-numeric-value-c-programming-guide"></a>文字列が数値を表しているかどうかを確認する方法 (C# プログラミング ガイド)
文字列が指定された数値型の有効な表現であるかどうかを確認するには、静的 `TryParse` メソッドを使用します。このメソッドには、すべてのプリミティブ数値型が実装されており、また <xref:System.DateTime>、<xref:System.Net.IPAddress> などの型も実装されています。 次の例では、"108" が有効な [int](../../language-reference/builtin-types/integral-numeric-types.md) かどうかを確認する方法を示します。  
  
```csharp  
int i = 0;
string s = "108";  
bool result = int.TryParse(s, out i); //i now = 108  
```  
  
 文字列が数値以外の文字、または指定した型で表すには大きすぎる (または小さすぎる) 数値の場合、`TryParse` は false を返し、out パラメーターを 0 に設定します。 それ以外の場合は true を返し、out パラメーターを文字列の数値に設定します。  
  
> [!NOTE]
> 文字列が数値文字列だけを含んでいても、使用する `TryParse` メソッドの型として有効ではない場合があります。 たとえば、"256" は `byte` の有効値ではありませんが、`int` としては有効です。 " 98.6" は `int` の有効値ではありませんが、有効な `decimal` です。  
  
## <a name="example"></a>例  
 次の例では、`TryParse` 値、`long` 値、および `byte` 値の文字列表現を指定して `decimal` を使用する方法を示します。  
  
 [!code-csharp[csProgGuideStrings#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#14)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 プリミティブ数値型は、`Parse` 静的メソッドも実装します。このメソッドは、文字列が有効な数値でない場合は例外をスローします。 一般に、数値が有効でない場合は単に false を返す `TryParse` の方が効率的です。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 テキスト ボックス、コンボ ボックスなどのコントロールからのユーザー入力を検証するには、常に `TryParse` メソッドまたは `Parse` メソッドを使用してください。  
  
## <a name="see-also"></a>参照

- [バイト配列を int に変換する方法](../types/how-to-convert-a-byte-array-to-an-int.md)
- [文字列を数値に変換する方法](../types/how-to-convert-a-string-to-a-number.md)
- [16 進文字列と数値型の間で変換する方法](../types/how-to-convert-between-hexadecimal-strings-and-numeric-types.md)
- [数値文字列の解析](../../../standard/base-types/parsing-numeric.md)
- [型の書式設定](../../../standard/base-types/formatting-types.md)
