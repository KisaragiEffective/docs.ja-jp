---
title: 匿名関数 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [C#], as anonymous functions
- anonymous functions [C#]
- anonymous methods [C#]
ms.assetid: 6ce3f04d-0c71-4728-9127-634c7e9a8365
ms.openlocfilehash: cfb0190ee263e65e8130a8925f76357a382eafa3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712001"
---
# <a name="anonymous-functions-c-programming-guide"></a>匿名関数 (C# プログラミング ガイド)

匿名関数は、デリゲート型が必要とされる任意の場所で使用できる "インライン" のステートメントまたは式です。 名前付きデリゲートを初期化するときや、メソッドのパラメーターとして名前付きデリゲート型の代わりに渡したりするときに使用できます。

[ラムダ式](lambda-expressions.md)または[匿名メソッド](../../language-reference/operators/delegate-operator.md)を使って匿名関数を作成できます。 ラムダ式を使った方がより簡潔で表現性に優れた方法でインライン コードを記述できるため、こちらを使うことをお勧めします。 匿名メソッドとは異なり、一部の型のラムダ式は式ツリー型に変換することができます。

## <a name="the-evolution-of-delegates-in-c"></a>C\# のデリゲートの進化

 C# 1.0 では、コードのどこかで定義したメソッドを使用して明示的に初期化することで、デリゲートのインスタンスを作成していました。 C# 2.0 では、デリゲートの呼び出しで実行できる名前なしのインライン ステートメント ブロックを記述する方法として、匿名メソッドの概念が導入されました。 C# 3.0 では、ラムダ式が導入されました。ラムダ式は、概念的には匿名メソッドと似ていますが、より表現力が豊かで簡潔です。 これら 2 つの機能は総称として*匿名関数*と呼ばれます。 一般的に、バージョン 3.5 以降の .NET Framework をターゲットとするアプリケーションであれば、ラムダ式を使用することが推奨されます。  
  
 次の例は、C# 1.0 から C# 3.0 のデリゲート作成の進化を示しています。  
  
 [!code-csharp[csProgGuideLINQ#65](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#65)]  
  
## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/expressions.md#anonymous-function-expressions)」の[無名関数の式](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。
  
## <a name="see-also"></a>参照

- [ステートメント、式、および演算子](./index.md)
- [ラムダ式](./lambda-expressions.md)
- [デリゲート](../delegates/index.md)
- [式ツリー (C#)](../concepts/expression-trees/index.md)
