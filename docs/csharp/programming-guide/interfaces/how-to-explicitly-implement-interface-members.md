---
title: インターフェイス メンバーを明示的に実装する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [C#], explicitly implementing
ms.assetid: 514cde76-f981-474e-8b40-9493619f899c
ms.openlocfilehash: dff094aca237ed6146bd9b52813c40549bc99b9b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77627786"
---
# <a name="how-to-explicitly-implement-interface-members-c-programming-guide"></a>インターフェイス メンバーを明示的に実装する方法 (C# プログラミング ガイド)
この例では[インターフェイス](../../language-reference/keywords/interface.md)、`IDimensions`、およびクラス `Box` を宣言します。これは、インターフェイス メンバーの `GetLength` と `GetWidth` を明示的に実装します。 メンバーには、インターフェイス インスタンス `dimensions` を介してアクセスします。  
  
## <a name="example"></a>例  
 [!code-csharp[csProgGuideInheritance#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#8)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
  
- `Main` メソッドでは、次の行はコメントアウトされます。これらの行でコンパイル エラーが発生するためです。 明示的に実装されるインターフェイス メンバーに、[クラス](../../language-reference/keywords/class.md) インスタンスからアクセスすることはできません。  
  
     [!code-csharp[csProgGuideInheritance#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#45)]  
  
- `Main` メソッドの以下の行では、メソッドがインターフェイスのインスタンスから呼び出されるため、ボックスのサイズを正常に出力できます。  
  
     [!code-csharp[csProgGuideInheritance#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#46)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](../classes-and-structs/index.md)
- [インターフェイス](./index.md)
- [2 つのインターフェイスのメンバーを明示的に実装する方法](./how-to-explicitly-implement-members-of-two-interfaces.md)
