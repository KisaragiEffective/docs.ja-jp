---
title: ポインター変換 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- pointers [C#], conversions
ms.assetid: f0e87502-477a-4ede-a31f-7a3e262e46fb
ms.openlocfilehash: 517166331d2bcf73132269ce2adcf68d5f60b4fe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76745367"
---
# <a name="pointer-conversions-c-programming-guide"></a>ポインター変換 (C# プログラミング ガイド)
定義済みの暗黙的なポインター変換を次の表に示します。 暗黙的な変換は、メソッドの呼び出しや代入ステートメントなど、多くの状況で発生することがあります。  
  
## <a name="implicit-pointer-conversions"></a>暗黙的なポインター変換  
  
|ソース|ターゲット|  
|----------|--------|  
|任意のポインター型|void*|  
|null|任意のポインター型|  
  
 明示的なポインター変換は、暗黙的な変換がない場合に、キャスト式を使用して、変換を実行するために使用します。 次の表はこの変換についてまとめたものです。  
  
## <a name="explicit-pointer-conversions"></a>明示的なポインター変換  
  
|ソース|ターゲット|  
|----------|--------|  
|任意のポインター型|その他のポインター型|  
|sbyte、byte、short、ushort、int、uint、long または ulong|任意のポインター型|  
|任意のポインター型|sbyte、byte、short、ushort、int、uint、long または ulong|  
  
## <a name="example"></a>例  
 次の例では、`int` へのポインターは `byte` へのポインターに変換されます。 ポインターは、変数の最下位バイト アドレスを指すことに注意してください。 `int` のサイズ (4 バイト) まで結果を連続してインクリメントする場合、変数の残りのバイトを表示することができます。  
  
 [!code-csharp[csProgGuidePointers#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers2.cs#3)]  
  
 [!code-csharp[csProgGuidePointers#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers.cs#4)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [ポインター型](pointer-types.md)
- [参照型](../../language-reference/keywords/reference-types.md)
- [値型](../../language-reference/builtin-types/value-types.md)
- [unsafe](../../language-reference/keywords/unsafe.md)
- [fixed ステートメント](../../language-reference/keywords/fixed-statement.md)
- [stackalloc](../../language-reference/operators/stackalloc.md)
