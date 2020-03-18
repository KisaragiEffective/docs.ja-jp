---
title: アクセサーのアクセシビリティの制限 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- read-only properties [C#]
- read-only indexers [C#]
- accessors [C#]
- properties [C#], read-only
- asymmetric accessor accessibility [C#]
- indexers [C#], read-only
ms.assetid: 6e655798-e112-4301-a680-6310a6e012e1
ms.openlocfilehash: a332fef814f0c81914eb7b8c308de68f719fbaac
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714692"
---
# <a name="restricting-accessor-accessibility-c-programming-guide"></a>アクセサーのアクセシビリティの制限 (C# プログラミング ガイド)
プロパティまたはインデクサーの [get](../../language-reference/keywords/get.md) および [set](../../language-reference/keywords/set.md) 部分は、*アクセサー*と呼ばれます。 既定では、これらのアクセサーは、それらが属するプロパティまたはインデクサーと同じ可視性またはアクセス レベルを持っています。 詳細については、「[アクセシビリティ レベル](../../language-reference/keywords/accessibility-levels.md)」を参照してください。 ただし、これらのアクセサーのいずれかにアクセスを制限すると便利な場合があります。 通常、これには、`set` アクセサーのアクセシビリティを制限しながら、`get` アクセサーのパブリック アクセスを維持する操作が含まれます。 次に例を示します。  
  
 [!code-csharp[csProgGuideIndexers#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#6)]  
  
 この例では、`Name` というプロパティが `get` および `set` アクセサーを定義します。 `get` アクセサーは、プロパティ自体のアクセシビリティ レベル (この場合は `public`) を受け取り、同時に `set` アクセサーは、[protected](../../language-reference/keywords/protected.md) アクセサー修飾子をアクセサー自体に適用することによって明示的に制限されます。  
  
## <a name="restrictions-on-access-modifiers-on-accessors"></a>アクセサーのアクセス修飾子の制限  
 プロパティまたはインデクサーでのアクセサー修飾子の使用は、以下の条件の対象になります。  
  
- インターフェイスまたは明示的な [interface](../../language-reference/keywords/interface.md) メンバーの実装でアクセサー修飾子を使用することはできません。  
  
- アクセサー修飾子は、プロパティまたはインデクサーに `set` と `get` アクセサーの両方がある場合にのみ使用できます。 この場合、修飾子は、2 つのアクセサーのいずれかでのみ許可されます。  
  
- プロパティまたはインデクサーに [override](../../language-reference/keywords/override.md) 修飾子がある場合は、アクセサー修飾子はオーバーライドされるアクセサーのアクセサー (存在する場合) と一致する必要があります。  
  
- アクセサーのアクセシビリティ レベルは、プロパティまたはインデクサー自体のアクセシビリティ レベルよりも制限されていなければなりません  
  
## <a name="access-modifiers-on-overriding-accessors"></a>オーバーライドするアクセサーのアクセス修飾子  
 プロパティまたはインデクサーをオーバーライドする場合は、オーバーライドされたアクセサーがオーバーライドするコードにアクセスできなければなりません。 プロパティ/インデクサーとそのアクセサー両方のアクセシビリティが、対応するオーバーライドされるプロパティ/インデクサーおよびアクセサーと一致している必要があります。 次に例を示します。  
  
 [!code-csharp[csProgGuideIndexers#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#7)]  
  
## <a name="implementing-interfaces"></a>インターフェイスの実装  
 アクセサーを使用してインターフェイスを実装する場合、アクセサーがアクセス修飾子を持つことはできません。 ただし、`get` などの 1 つのアクセサーを使用してインターフェイスを実装する場合、他のアクセサーは、次の例のように、アクセス修飾子を持つことができます。  
  
 [!code-csharp[csProgGuideIndexers#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#8)]  
  
## <a name="accessor-accessibility-domain"></a>アクセサー アクセシビリティ ドメイン  
 アクセサーでアクセス修飾子を使用する場合、アクセサーの[アクセシビリティ ドメイン](../../language-reference/keywords/accessibility-domain.md)はこの修飾子によって決まります。  
  
 アクセサーでアクセス修飾子を使用しなかった場合、アクセサーのアクセシビリティ ドメインは、プロパティまたはインデクサーのアクセシビリティ レベルによって決まります。  
  
## <a name="example"></a>例  
 次の例は、`BaseClass`、`DerivedClass`、および `MainClass` という 3 つのクラスを含んでいます。 すべてのクラスの `BaseClass`、`Name`、`Id` に 2 つのプロパティがあります。 この例は、`Id`protected`DerivedClass` や `Id`private`BaseClass` などの制限アクセス修飾子を使用するときに、[ のプロパティ ](../../language-reference/keywords/protected.md) によって [ のプロパティ ](../../language-reference/keywords/private.md) を非表示にする方法を示しています。 そのため、このプロパティに値を割り当てるときには、代わりに `BaseClass` クラスのプロパティが呼び出されます。 [public](../../language-reference/keywords/public.md) によってアクセス修飾子を置き換えると、プロパティがアクセス可能になります。  
  
 この例はまた、`private` の `protected` プロパティの `set` アクセサー上の `Name` や `DerivedClass` などの制限されるアクセス修飾子が、アクセサーへのアクセスを防ぎ、アクセサーに割り当てたときにエラーが生成されることも示しています。  
  
 [!code-csharp[csProgGuideIndexers#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#5)]  
  
## <a name="comments"></a>コメント  
 宣言 `new private string Id` を `new public string Id` によって置き換えた場合、次の出力を取得します。  
  
 `Name and ID in the base class: Name-BaseClass, ID-BaseClass`  
  
 `Name and ID in the derived class: John, John123`  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [Properties](./properties.md)
- [インデクサー](../indexers/index.md)
- [アクセス修飾子](./access-modifiers.md)
