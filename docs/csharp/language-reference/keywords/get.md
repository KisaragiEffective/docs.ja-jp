---
title: get - C# リファレンス
ms.date: 03/10/2017
f1_keywords:
- get_CSharpKeyword
- get
helpviewer_keywords:
- get keyword [C#]
ms.assetid: a52de048-fbe0-41b0-82ec-8e4ac04d3a71
ms.openlocfilehash: 61d8c02aaf13f43ff8ea17c1e868ea9fd52893c9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173628"
---
# <a name="get-c-reference"></a>get (C# リファレンス)

`get` キーワードは、プロパティの*アクセサー*メソッド、またはプロパティ値かインデクサー要素を返すインデクサーを定義します。 詳しくは、「[プロパティ (C# プログラミング ガイド)](../../programming-guide/classes-and-structs/properties.md)」、「[自動実装するプロパティ (C# プログラミング ガイド)](../../programming-guide/classes-and-structs/auto-implemented-properties.md)」、および「[インデクサー (C# プログラミング ガイド)](../../programming-guide/indexers/index.md)」をご覧ください。  
  
次の例では、`get` という名前のプロパティの `set` アクセサーと `Seconds` アクセサーを定義しています。 また、`_seconds` という名前のプライベート フィールドを使って、プロパティの値を戻しています。  

 [!code-csharp[get#1](../../../../samples/snippets/csharp/language-reference/keywords/get/get-1.cs)]  
  
多くの場合、前の例のように、`get` アクセサーは値を返す 1 つのステートメントで構成されます。 C# 7.0 以降では、式形式のメンバーとして `get` アクセサーを実装できます。 次の例では、`get` アクセサーと `set` アクセサーの両方を、式形式のメンバーとして実装しています。

 [!code-csharp[get#3](../../../../samples/snippets/csharp/language-reference/keywords/get/get-3.cs)]

プロパティの `get` アクセサーと `set` アクセサーがプライベート バッキング フィールドの値の設定と取得以外の操作を実行しない単純な場合では、自動実装プロパティに対する C# コンパイラのサポートを利用できます。 次の例では、自動実装プロパティとして `Hours` を実装しています。
  
 [!code-csharp[get#2](../../../../samples/snippets/csharp/language-reference/keywords/get/get-2.cs)]  
  
## <a name="c-language-specification"></a>C# 言語仕様

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](./index.md)
- [Properties](../../programming-guide/classes-and-structs/properties.md)
