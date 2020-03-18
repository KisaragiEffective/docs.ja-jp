---
title: リフレクションを使用した属性へのアクセス (C#)
ms.date: 07/20/2015
ms.assetid: dce3a696-4ceb-489a-b5e4-322a83052f18
ms.openlocfilehash: 990b6487e50bfb2d123c3871e5f85da063711d9e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69595497"
---
# <a name="accessing-attributes-by-using-reflection-c"></a>リフレクションを使用した属性へのアクセス (C#)
カスタム属性を定義し、それらをソース コード内に配置することができても、その情報を取得して操作する手段がなければ、ほとんど価値はありません。 リフレクションを使用すれば、カスタム属性を使用して定義された情報を取得することができます。 鍵となるメソッドは `GetCustomAttributes` です。このメソッドは、ソース コード属性の実行時の等価オブジェクトを配列で返します。 このメソッドには、いくつかのオーバー ロード バージョンがあります。 詳細については、<xref:System.Attribute> を参照してください。  
  
 次のような属性指定は、  
  
```csharp  
[Author("P. Ackerman", version = 1.1)]  
class SampleClass  
```  
  
 概念的には次の記述と同じです。  
  
```csharp  
Author anonymousAuthorObject = new Author("P. Ackerman");  
anonymousAuthorObject.version = 1.1;  
```  
  
 ただし、属性について `SampleClass` が照会されるまで、コードは実行されません。 `GetCustomAttributes` について `SampleClass` を呼び出すと、`Author` オブジェクトが作成され、上記のように初期化されます。 クラスに他の属性がある場合は、他の属性オブジェクトも同様に作成されます。 `GetCustomAttributes` はその後、`Author` オブジェクトと配列内の他の属性オブジェクトを返します。 その後、この配列を反復処理し、各配列要素の型に基づいてどの属性が適用されたかを確認して、属性オブジェクトから情報を抽出することができます。  
  
## <a name="example"></a>例  
 完全な例を次に示します。 カスタム属性が定義され、複数のエンティティに適用された後、リフレクションを使用して取得されています。  
  
```csharp  
// Multiuse attribute.  
[System.AttributeUsage(System.AttributeTargets.Class |  
                       System.AttributeTargets.Struct,  
                       AllowMultiple = true)  // Multiuse attribute.  
]  
public class Author : System.Attribute  
{  
    string name;  
    public double version;  
  
    public Author(string name)  
    {  
        this.name = name;  
  
        // Default value.  
        version = 1.0;  
    }  
  
    public string GetName()  
    {  
        return name;  
    }  
}  
  
// Class with the Author attribute.  
[Author("P. Ackerman")]  
public class FirstClass  
{  
    // ...  
}  
  
// Class without the Author attribute.  
public class SecondClass  
{  
    // ...  
}  
  
// Class with multiple Author attributes.  
[Author("P. Ackerman"), Author("R. Koch", version = 2.0)]  
public class ThirdClass  
{  
    // ...  
}  
  
class TestAuthorAttribute  
{  
    static void Test()  
    {  
        PrintAuthorInfo(typeof(FirstClass));  
        PrintAuthorInfo(typeof(SecondClass));  
        PrintAuthorInfo(typeof(ThirdClass));  
    }  
  
    private static void PrintAuthorInfo(System.Type t)  
    {  
        System.Console.WriteLine("Author information for {0}", t);  
  
        // Using reflection.  
        System.Attribute[] attrs = System.Attribute.GetCustomAttributes(t);  // Reflection.  
  
        // Displaying output.  
        foreach (System.Attribute attr in attrs)  
        {  
            if (attr is Author)  
            {  
                Author a = (Author)attr;  
                System.Console.WriteLine("   {0}, version {1:f}", a.GetName(), a.version);  
            }  
        }  
    }  
}  
/* Output:  
    Author information for FirstClass  
       P. Ackerman, version 1.00  
    Author information for SecondClass  
    Author information for ThirdClass  
       R. Koch, version 2.00  
       P. Ackerman, version 1.00  
*/  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Reflection>
- <xref:System.Attribute>
- [C# プログラミングガイド](../../index.md)
- [属性に格納されている情報の取得](../../../../standard/attributes/retrieving-information-stored-in-attributes.md)
- [リフレクション (C#)](../reflection.md)
- [属性 (C#)](./index.md)
- [カスタム属性の作成 (C#)](./creating-custom-attributes.md)
