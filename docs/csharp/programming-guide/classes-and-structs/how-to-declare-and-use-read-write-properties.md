---
title: 読み取り/書き込みプロパティを宣言および使用する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- get accessor [C#], declaring properties
- set accessor [C#]
- properties [C#], declaring
- read/write properties [C#]
- accessors [C#], declaring properties with
ms.assetid: a4962fef-af7e-4c4b-a929-4ae4d646ab8a
ms.openlocfilehash: 4b9db5f15746ab9a1f42239150c6783154723371
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170287"
---
# <a name="how-to-declare-and-use-read-write-properties-c-programming-guide"></a>読み取り/書き込みプロパティを宣言および使用する方法 (C# プログラミング ガイド)
プロパティは、オブジェクトのデータへの保護されていない、制御されず未確認のアクセスに伴うリスクなしにパブリック データ メンバーの利便性を提供します。 これは*アクセサー*を通じて行われます。アクセサーは、基になるデータ メンバーの値を割り当てたり、取得したりする特殊なメソッドです。 [set](../../language-reference/keywords/set.md) アクセサーはデータ メンバーの割り当てを可能にし、[get](../../language-reference/keywords/get.md) アクセサーはデータ メンバーの値を取得します。  
  
 このサンプルでは、`Person` (string) および `Name` (int) という 2 つのプロパティを持つ `Age` クラスを示します。 両方のプロパティは `get` および `set` アクセサーを提供するため、読み取り/書き込みプロパティと見なされます。  
  
## <a name="example"></a>例  
 [!code-csharp[csProgGuideObjects#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#33)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 前述の例では、`Name` および `Age` プロパティは[パブリック](../../language-reference/keywords/public.md)であり、`get` および `set` アクセサーの両方が含まれます。 そのため、任意のオブジェクトがこれらのプロパティを読み書きできます。 ただし、アクセサーのいずれかを除外することが望ましい場合もあります。 たとえば、次のように `set` アクセサーを省略すると、プロパティが読み取り専用になります。  
  
 [!code-csharp[csProgGuideObjects#87](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#87)]  
  
 また、一方のアクセサーをパブリックに公開し、もう一方を private または protected にすることもできます。 詳細については、「[アクセサーのアクセシビリティの制限 (C# プログラミング ガイド)](./restricting-accessor-accessibility.md)」を参照してください。  
  
 プロパティを宣言すると、プロパティをクラスのフィールドのように使用できます。 そのため、プロパティ値の取得と設定の両方で、次のように自然な構文を使用できます。  
  
 [!code-csharp[csProgGuideObjects#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#35)]  
  
 プロパティの `set` メソッドでは、特殊な `value` 変数を使用できることに注意してください。 この変数には、ユーザーが指定した値が含まれます。たとえば、次のように指定します。  
  
 [!code-csharp[csProgGuideObjects#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#36)]  
  
 `Age` オブジェクトの `Person` プロパティの値を増分する場合は、次のような簡潔な構文を使用します。  
  
 [!code-csharp[csProgGuideObjects#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#37)]  
  
 `set` メソッドと `get` メソッドがそれぞれ使用されてプロパティがモデル化されている場合、上記と同じ内容のコードは次のようになります。  
  
```csharp  
person.SetAge(person.GetAge() + 1);
```  
  
 この例では、`ToString` メソッドが次のようにオーバーライドされます。  
  
 [!code-csharp[csProgGuideObjects#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#38)]  
  
 プログラムでは `ToString` が明示的に使用されないことに注意してください。 既定では、`WriteLine` 呼び出しによって呼び出されます。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [Properties](./properties.md)
- [クラスと構造体](./index.md)
