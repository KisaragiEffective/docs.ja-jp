---
title: デリゲートを結合する方法 (マルチキャスト デリゲート) - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], combining
- multicast delegates [C#]
ms.assetid: 4e689450-6d0c-46de-acfd-f961018ae5dd
ms.openlocfilehash: 7b5b9ba5c9bf70983fac9f869836b4c8c5449eca
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705380"
---
# <a name="how-to-combine-delegates-multicast-delegates-c-programming-guide"></a>デリゲートを結合する方法 (マルチキャスト デリゲート) (C# プログラミング ガイド)
ここでは、マルチキャスト デリゲートを作成する例について説明します。 [デリゲート](../../language-reference/builtin-types/reference-types.md) オブジェクトの便利な性質の 1 つは、`+` 演算子を使用して、複数のオブジェクトを 1 つのデリゲート インスタンスに割り当てられることです。 マルチキャスト デリゲートには、割り当てられたデリゲートの一覧が含まれています。 マルチキャスト デリゲートを呼び出すと、一覧内のデリゲートが順に呼び出されます。 結合できるのは、同じ型のデリゲートのみです。  
  
 `-` 演算子を使用して、マルチキャスト デリゲートからコンポーネント デリゲートを削除することができます。  
  
## <a name="example"></a>例  
 [!code-csharp[csProgGuideDelegates#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#11)]  
  
## <a name="see-also"></a>参照

- <xref:System.MulticastDelegate>
- [C# プログラミングガイド](../index.md)
- [イベント](../events/index.md)
