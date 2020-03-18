---
title: COM 相互運用機能を使用したプログラミングでインデックス付きプロパティを使用する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- indexed properties [C#]
- Office programming [C#], indexed properties
- properties [C#], indexed
ms.assetid: 756bfc1e-7c28-4d4d-b114-ac9288c73882
ms.openlocfilehash: 864e2274f0e0e79b4843e0bb67b5c4384eac8588
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712066"
---
# <a name="how-to-use-indexed-properties-in-com-interop-programming-c-programming-guide"></a>COM 相互運用機能を使用したプログラミングでインデックス付きプロパティを使用する方法 (C# プログラミング ガイド)
"*インデックス付きプロパティ*" により、パラメーターを持つ COM プロパティが C# プログラミングでいっそう使いやすくなります。 インデックス付きプロパティは、[名前付き引数と省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)、新しい型 ([dynamic](../../language-reference/builtin-types/reference-types.md))、[埋め込み型情報](../../../standard/assembly/embed-types-visual-studio.md)などの Visual C# の他の機能と連携して、Microsoft Office プログラミングをいっそう強力なものにします。  
  
 以前のバージョンの C# では、プロパティとしてメソッドにアクセスできるのは、`get` メソッドがパラメーターを持たず、`set` メソッドが 1 つだけ値パラメーターを持つ場合に限られました。 しかし、すべての COM プロパティがこのような制限を満たしているわけではありません。 たとえば、Excel の <xref:Microsoft.Office.Interop.Excel.Range.Range%2A> プロパティには、範囲の名前のパラメーターを必要とする `get` アクセサーがあります。 これまでは、このような `Range` プロパティに直接アクセスすることはできず、次の例に示すように、`get_Range` メソッドを代わりに使う必要がありました。  
  
 [!code-csharp[csProgGuideIndexedProperties#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#1)]  
  
 インデックス付きプロパティを使うと、次のようなコードを記述できます。  
  
 [!code-csharp[csProgGuideIndexedProperties#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#2)]  
  
> [!NOTE]
> また、前の例では、[省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)機能も使われており、`Type.Missing` を省略できます。  
  
 C# 3.0 以前で `Value` オブジェクトの <xref:Microsoft.Office.Interop.Excel.Range> プロパティの値を設定する場合と同様に、2 つの引数が必要です。 1 つのパラメーターでは、範囲の値の型を指定する省略可能なパラメーターの引数を渡します。 そしてもう 1 つのパラメーターで、`Value` プロパティの値を渡します。 次の例は、これらの方法を示したものです。 どちらも、セル A1 の値を `Name` に設定しています。
  
 [!code-csharp[csProgGuideIndexedProperties#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#3)]  
  
 インデックス付きプロパティを使うと、次のようなコードを記述できます。  
  
 [!code-csharp[csProgGuideIndexedProperties#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#4)]  
  
 独自のインデックス付きプロパティを作成することはできません。 この機能では、既存のインデックス付きプロパティの使用のみがサポートされます。  
  
## <a name="example"></a>例  
 次に完全なコードの例を示します。 Office API にアクセスするプロジェクトを設定する方法の詳細については、「[C# の機能を使用して Office 相互運用オブジェクトにアクセスする方法](./how-to-access-office-onterop-objects.md)」をご覧ください。
  
 [!code-csharp[csProgGuideIndexedProperties#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#5)]  
  
## <a name="see-also"></a>参照

- [名前付き引数と省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)
- [dynamic](../../language-reference/builtin-types/reference-types.md)
- [dynamic 型の使用](../types/using-type-dynamic.md)
- [Office プログラミングで名前付き引数と省略可能な引数を使用する方法](../classes-and-structs/how-to-use-named-and-optional-arguments-in-office-programming.md)
- [C# の機能を使用して Office 相互運用オブジェクトにアクセスする方法](./how-to-access-office-onterop-objects.md)
- [チュートリアル: Office のプログラミング](./walkthrough-office-programming.md)
