---
title: カスタム イベント アクセサーを実装する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- accessors [C#], event accessors
- add accessor [C#]
- events [C#], add accessor
- events [C#], remove accessor
- remove accessor [C#]
ms.assetid: bf903abf-03a4-4f7b-ab6b-b7e59bc2ee1e
ms.openlocfilehash: 34e816799f472e8945962e334b9a90b2582e0393
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705354"
---
# <a name="how-to-implement-custom-event-accessors-c-programming-guide"></a>カスタム イベント アクセサーを実装する方法 (C# プログラミング ガイド)
イベントは、宣言元のクラス内でしか呼び出せない特殊なマルチキャスト デリゲートです。 クライアント コードは、イベント発生時に呼び出されるメソッドへの参照を提供することにより、イベントにサブスクライブします。 これらのメソッドは、イベント アクセサーを使用してデリゲートの呼び出しリストに追加されます。イベント アクセサーはプロパティ アクセサーに似ていますが、イベント アクセサーには `add` および `remove` という名前が付いている点が異なります。 ほとんどの場合、カスタム イベント アクセサーを指定する必要はありません。 コードでカスタム イベント アクセサーを指定していない場合は、コンパイラによって自動的に追加されます。 ただし、カスタム動作の指定が必要な場合もあります。 「[インターフェイス イベントを実装する方法](./how-to-implement-interface-events.md)」トピックは、そのような場合の一例を示しています。
  
## <a name="example"></a>例  
 次の例は、カスタム イベント アクセサーの add と remove を実装する方法を示しています。 アクセサー内で任意のコードに置き換えることができますが、新しいイベント ハンドラー メソッドを追加または削除する前に、イベントをロックすることをお勧めします。  
  
[!code-csharp[IDrawingObject.OnDraw](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#IDrawingObjectOnDraw)]  
  
## <a name="see-also"></a>参照

- [イベント](./index.md)
- [event](../../language-reference/keywords/event.md)
