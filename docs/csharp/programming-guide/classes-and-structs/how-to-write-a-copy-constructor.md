---
title: コピー コンストラクターを記述する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# Language, copy constructor
- copy constructor [C#]
ms.assetid: fba899b5-fc41-428e-a745-3ebdbf37990a
ms.openlocfilehash: aa6feb1b07f491a90a78684e254910d387b9bccd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714851"
---
# <a name="how-to-write-a-copy-constructor-c-programming-guide"></a>コピー コンストラクターを記述する方法 (C# プログラミング ガイド)
C# では、オブジェクトのコピー コンストラクターが提供されていませんが、独自に作成することができます。  
  
## <a name="example"></a>例  
 次の例の `Person`[クラス](../../language-reference/keywords/class.md)では、`Person` のインスタンスを引数として受け取るコピー コンストラクターが定義されています。 引数のプロパティの値は、`Person`の新しいインスタンスのプロパティに割り当てられています。 このコードには、コピーするインスタンスの `Name` プロパティと `Age` プロパティをクラスのインスタンス コンストラクターに渡す代替のコピー コンストラクターが含まれています。  
  
 [!code-csharp[csProgGuideObjects#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#16)]  
  
## <a name="see-also"></a>参照

- <xref:System.ICloneable>
- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [コンストラクター](./constructors.md)
- [ファイナライザー](./destructors.md)
