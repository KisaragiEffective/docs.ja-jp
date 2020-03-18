---
title: 名前付きメソッドを使用したデリゲートと匿名メソッド - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], with named vs. anonymous methods
- methods [C#], in delegates
ms.assetid: 98fa8c61-66b6-4146-986c-3236c4045733
ms.openlocfilehash: 1ec366999ca6457471b705fa83f06fcde4293f4e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712378"
---
# <a name="delegates-with-named-vs-anonymous-methods-c-programming-guide"></a>名前付きメソッドを使用したデリゲートと匿名メソッド (C# プログラミング ガイド)
[デリゲート](../../language-reference/builtin-types/reference-types.md)は、名前付きメソッドに関連付けることができます。 名前付きメソッドを使用してデリゲートをインスタンス化するときは、次のように、そのメソッドをパラメーターとして渡します。  
  
 [!code-csharp[csProgGuideDelegates#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#1)]  
  
 これを名前付きメソッドの使用といいます。 名前付きメソッドで作成されたデリゲートは、[静的](../../language-reference/keywords/static.md)メソッドまたはインスタンス メソッドのいずれかでカプセル化できます。 旧バージョンの C# では、デリゲートをインスタンス化するには、名前付きメソッドを使用するしかありませんが、 新しいメソッドを作成するのが、オーバーヘッドの点で望ましくない場合は、C# でデリゲートをインスタンス化し、そのデリゲートが呼び出されたときに処理するコード ブロックを直接指定できます。 ブロックには、ラムダ式または匿名メソッドのいずれかを含めることができます。 詳細については、[匿名関数](../statements-expressions-operators/anonymous-functions.md)に関するページをご覧ください。  
  
## <a name="remarks"></a>Remarks  
 デリゲート パラメーターとして渡すメソッドには、デリゲート宣言と同じシグネチャが必要です。  
  
 デリゲート インスタンスがカプセル化できるのは、静的メソッドまたはインスタンス メソッドのいずれかです。  
  
 デリゲートは [out](../../language-reference/keywords/out-parameter-modifier.md) パラメーターを使用できますが、マルチキャスト イベント デリゲートでこのパラメーターを使用することはお勧めしません。どのデリゲートが呼び出されるかわからないためです。  
  
## <a name="example-1"></a>例 1  
 次のシンプルな例では、デリゲートを宣言して使用します。 デリゲート `Del` とそれに関連付けられているメソッド `MultiplyNumbers` の両方に同じシグネチャが含まれることに注意してください。  
  
 [!code-csharp[csProgGuideDelegates#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#2)]  
  
## <a name="example-2"></a>例 2  
 次の例では、1 つのデリゲートを静的メソッドとインスタンス メソッドの両方に割り当て、各メソッドから特定の情報を戻します。  
  
 [!code-csharp[csProgGuideDelegates#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#3)]  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [デリゲート](./index.md)
- [デリゲートを結合する方法 (マルチキャスト デリゲート)](./how-to-combine-delegates-multicast-delegates.md)
- [イベント](../events/index.md)
