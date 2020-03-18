---
title: リフレクションを使用してアセンブリのメタデータにクエリを実行する方法 (LINQ) (C#)
ms.date: 07/20/2015
ms.assetid: c4cdce49-b1c8-4420-b12a-9ff7e6671368
ms.openlocfilehash: 6e68cfea2bf3e03aed9de3e4a18cf9941ece34e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168922"
---
# <a name="how-to-query-an-assemblys-metadata-with-reflection-linq-c"></a>リフレクションを使用してアセンブリのメタデータにクエリを実行する方法 (LINQ) (C#)

.NET Framework クラス ライブラリのリフレクション API を使用すると、.NET アセンブリ内のメタデータを調べ、そのアセンブリ内にある型、型メンバー、パラメーターなどのコレクションを作成できます。 これらのコレクションは、ジェネリック <xref:System.Collections.Generic.IEnumerable%601> インターフェイスをサポートするため、LINQ を使用して照会できます。  
  
次の例では、LINQ でリフレクションを使用して、指定した検索条件に一致するメソッドについてのメタデータを取得する方法を示します。 この例のクエリでは、配列などの列挙可能な型を返すすべてのメソッドの名前をアセンブリ内で検索します。  
  
## <a name="example"></a>例  
  
```csharp  
using System;
using System.Linq;
using System.Reflection;  

class ReflectionHowTO  
{  
    static void Main()  
    {  
        Assembly assembly = Assembly.Load("System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken= b77a5c561934e089");  
        var pubTypesQuery = from type in assembly.GetTypes()  
                    where type.IsPublic  
                        from method in type.GetMethods()  
                        where method.ReturnType.IsArray == true
                            || ( method.ReturnType.GetInterface(  
                                typeof(System.Collections.Generic.IEnumerable<>).FullName ) != null  
                            && method.ReturnType.FullName != "System.String" )  
                        group method.ToString() by type.ToString();  

        foreach (var groupOfMethods in pubTypesQuery)  
        {  
            Console.WriteLine("Type: {0}", groupOfMethods.Key);  
            foreach (var method in groupOfMethods)  
            {  
                Console.WriteLine("  {0}", method);  
            }  
        }  

        Console.WriteLine("Press any key to exit... ");  
        Console.ReadKey();  
    }  
}
```  

この例では、<xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType> メソッドを使用して、指定したアセンブリ内の型の配列を返します。 パブリック型のみが返されるように、[where](../../../language-reference/keywords/where-clause.md) フィルターが適用されています。 パブリック型ごとに、<xref:System.Reflection.MethodInfo> 呼び出しから返される <xref:System.Type.GetMethods%2A?displayProperty=nameWithType> 配列を使用してサブクエリが生成されます。 これらの結果はフィルター処理され、戻り値の型が配列か、<xref:System.Collections.Generic.IEnumerable%601> を実装する型であるメソッドのみが返されます。 最後に、型名をキーとして使用して、これらの結果がグループ化されます。  
  
## <a name="see-also"></a>参照

- [LINQ to Objects (C#)](./linq-to-objects.md)
