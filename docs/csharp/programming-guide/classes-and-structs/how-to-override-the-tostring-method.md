---
title: ToString メソッドをオーバーライドする方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- ToString method, overriding in C#
- inheritance [C#], overriding OnPaint and ToString
ms.assetid: 8016db69-1f19-420c-8e17-98e8bebb7749
ms.openlocfilehash: 7c7196df56821c134b31982d7956a75039e9f929
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705575"
---
# <a name="how-to-override-the-tostring-method-c-programming-guide"></a>ToString メソッドをオーバーライドする方法 (C# プログラミング ガイド)

C# では、すべてのクラスまたは構造体が、暗黙的に <xref:System.Object> クラスを継承します。 そのため、C# のすべてのオブジェクトが <xref:System.Object.ToString%2A> メソッドを取得し、そのオブジェクトの文字列表現を返します。 たとえば、`int` 型の変数はすべて `ToString` メソッドを持ち、次のようにその変数の内容を文字列として返すことができます。  
  
 [!code-csharp[csProgGuideInheritance#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#37)]  
  
 カスタムのクラスまたは構造体を作成するときは、クライアント コードにカスタム型の情報を提供するため、<xref:System.Object.ToString%2A> メソッドをオーバーライドする必要があります。  
  
 `ToString` メソッドで書式設定文字列やその他のカスタム形式を使用する方法については、「[型の書式設定](../../../standard/base-types/formatting-types.md)」を参照してください。  
  
> [!IMPORTANT]
> このメソッドを使用して提供する情報を決定するときは、作成したクラスまたは構造体が信頼関係のないコードによって使用されるかどうかを考慮します。 悪意があるコードで利用される可能性がある情報を提供しないように注意してください。  
  
クラスまたは構造体内の `ToString` メソッドをオーバーライドする手順
  
1. 次の修飾子および戻り値の値を指定して、`ToString` メソッドを宣言します。  
  
    ```csharp  
    public override string ToString(){}  
    ```  
  
2. 文字列を返すように、メソッドを実装します。  
  
     次の例では、特定のクラス インスタンスに固有のデータに加えて、クラス名も返されます。  
  
     [!code-csharp[csProgGuideInheritance#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#36)]  
  
     `ToString` メソッドをテストする方法を次のコード例に示します。  
  
     [!code-csharp[csProgGuideInheritance#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#38)]  
  
## <a name="see-also"></a>参照

- <xref:System.IFormattable>
- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [文字列](../strings/index.md)
- [string](../../language-reference/builtin-types/reference-types.md)
- [override](../../language-reference/keywords/override.md)
- [virtual](../../language-reference/keywords/virtual.md)
- [型の書式設定](../../../standard/base-types/formatting-types.md)
