---
title: remove コンテキスト キーワード - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- remove_CSharpKeyword
helpviewer_keywords:
- remove event accessor [C#]
ms.assetid: c8223426-c17b-4fe2-8406-01564cf1dd2b
ms.openlocfilehash: 8ea3ea1910e28c03b2a894c64415cb2ccff942d0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713145"
---
# <a name="remove-c-reference"></a>remove (C# リファレンス)

`remove` コンテキスト キーワードは、カスタム イベント アクセサーを定義するときに使用されます。このアクセサーは、クライアント コードが[イベント](event.md)から登録を解除するときに呼び出されます。 カスタムの `remove` アクセサーを指定するときは、[add](add.md) アクセサーも指定する必要があります。

## <a name="example"></a>例

次の例は、カスタムの [add](add.md) アクセサーと `remove` アクセサーが指定されているイベントを示しています。 サンプル全体については、「[インターフェイス イベントを実装する方法](../../programming-guide/events/how-to-implement-interface-events.md)」を参照してください。

 [!code-csharp[csrefKeywordsContextual#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#15)]

通常は、独自のカスタム イベント アクセサーを提供する必要はありません。 イベントを宣言するときにコンパイラで自動生成されるアクセサーは、ほとんどのシナリオで利用することができます。

## <a name="see-also"></a>参照

- [イベント](../../programming-guide/events/index.md)
