---
title: set キーワード - C# リファレンス
ms.date: 03/10/2017
f1_keywords:
- set
- set_CSharpKeyword
helpviewer_keywords:
- set keyword [C#]
ms.assetid: 30d7e4e5-cc2e-4635-a597-14a724879619
ms.openlocfilehash: 0b293709abe64a0a82d8575f6793a07ca6c9533b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173498"
---
# <a name="set-c-reference"></a>set (C# リファレンス)

`set` キーワードは、プロパティまたはインデクサーで、プロパティ値またはインデクサーの要素値を割り当てる "*アクセサー*" メソッドを定義します。 使用例を含む詳細については、「[プロパティ](../../programming-guide/classes-and-structs/properties.md)」、「[自動実装プロパティ](../../programming-guide/classes-and-structs/auto-implemented-properties.md)」、および「[インデクサー](../../programming-guide/indexers/index.md)」を参照してください。

次の例では、`get` という名前のプロパティの `set` アクセサーと `Seconds` アクセサーを定義しています。 また、`_seconds` という名前のプライベート フィールドを使って、プロパティの値を戻しています。

[!code-csharp[set#1](~/samples/snippets/csharp/language-reference/keywords/get/get-1.cs)]

多くの場合、前の例のように、`set` アクセサーは値を割り当てる 1 つのステートメントで構成されます。 C# 7.0 以降では、式形式のメンバーとして `set` アクセサーを実装できます。 次の例では、`get` アクセサーと `set` アクセサーの両方を、式形式のメンバーとして実装しています。

[!code-csharp[set#3](~/samples/snippets/csharp/language-reference/keywords/get/get-3.cs)]
  
プロパティの `get` アクセサーと `set` アクセサーがプライベート バッキング フィールドの値の設定と取得以外の操作を実行しない単純な場合では、自動実装プロパティに対する C# コンパイラのサポートを利用できます。 次の例では、自動実装プロパティとして `Hours` を実装しています。

[!code-csharp[set#2](~/samples/snippets/csharp/language-reference/keywords/get/get-2.cs)]
  
## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../../language-reference/index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [Properties](../../programming-guide/classes-and-structs/properties.md)
