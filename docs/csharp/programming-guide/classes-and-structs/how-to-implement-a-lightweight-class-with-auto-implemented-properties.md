---
title: 自動実装するプロパティを使用して簡易クラスを実装する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- auto-implemented properties [C#]
- properties [C#], auto-implemented
ms.assetid: 1dc5a8ad-a4f7-4f32-8506-3fc6d8c8bfed
ms.openlocfilehash: 6d121f6be768d41d22ea01d871662913b2daae2b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170274"
---
# <a name="how-to-implement-a-lightweight-class-with-auto-implemented-properties-c-programming-guide"></a>自動実装するプロパティを使用して簡易クラスを実装する方法 (C# プログラミング ガイド)

この例では、一連の自動実装プロパティのカプセル化のみを行う、変更できない簡易クラスの作成方法を示します。 参照型のセマンティクスを使用する必要がある場合は、構造体ではなく次のようなコンストラクトを使用します。

変更できないプロパティの作成方法は 2 つあります。

- [set](../../language-reference/keywords/set.md) アクセサーは、[private](../../language-reference/keywords/private.md) として宣言することができます。  プロパティは型の中のみで設定可能で、コンシューマーは変更できません。

  `set` アクセサーを private で宣言した場合、オブジェクト初期化子を使用してプロパティを初期化することはできません。 コンストラクターまたはファクトリ メソッドを使用する必要があります。
- [get](../../language-reference/keywords/get.md) アクセサーのみを宣言し、その型のコンストラクターを除くすべての場所でプロパティを変更できないようにすることができます。

次の例では、get アクセサーのみを持つプロパティが、get と private set を持つプロパティとどのように異なるかを示しています。

```csharp
class Contact
{
    public string Name { get; }
    public string Address { get; private set; }

    public Contact(string contactName, string contactAddress)
    {
        // Both properties are accessible in the constructor.
        Name = contactName;
        Address = contactAddress;
    }

    // Name isn't assignable here. This will generate a compile error.
    //public void ChangeName(string newName) => Name = newName;

    // Address is assignable here.
    public void ChangeAddress(string newAddress) => Address = newAddress
}
```

## <a name="example"></a>例

次の例は、自動実装プロパティを持つ変更できないクラスを実装する 2 つの方法を示します。 それぞれの方法では、プロパティの 1 つを private `set` で、1 つを `get` のみで宣言します。  最初のクラスはプロパティの初期化にコンストラクターのみを使用しますが、2 番目のクラスは、コンストラクターを呼び出す静的ファクトリ メソッドを使用します。

```csharp
// This class is immutable. After an object is created,
// it cannot be modified from outside the class. It uses a
// constructor to initialize its properties.
class Contact
{
    // Read-only property.
    public string Name { get; }

    // Read-write property with a private set accessor.
    public string Address { get; private set; }

    // Public constructor.
    public Contact(string contactName, string contactAddress)
    {
        Name = contactName;
        Address = contactAddress;
    }
}

// This class is immutable. After an object is created,
// it cannot be modified from outside the class. It uses a
// static method and private constructor to initialize its properties.
public class Contact2
{
    // Read-write property with a private set accessor.
    public string Name { get; private set; }

    // Read-only property.
    public string Address { get; }

    // Private constructor.
    private Contact2(string contactName, string contactAddress)
    {
        Name = contactName;
        Address = contactAddress;
    }

    // Public factory method.
    public static Contact2 CreateContact(string name, string address)
    {
        return new Contact2(name, address);
    }
}

public class Program
{
    static void Main()
    {
        // Some simple data sources.
        string[] names = {"Terry Adams","Fadi Fakhouri", "Hanying Feng",
                            "Cesar Garcia", "Debra Garcia"};
        string[] addresses = {"123 Main St.", "345 Cypress Ave.", "678 1st Ave",
                                "12 108th St.", "89 E. 42nd St."};

        // Simple query to demonstrate object creation in select clause.
        // Create Contact objects by using a constructor.
        var query1 = from i in Enumerable.Range(0, 5)
                    select new Contact(names[i], addresses[i]);

        // List elements cannot be modified by client code.
        var list = query1.ToList();
        foreach (var contact in list)
        {
            Console.WriteLine("{0}, {1}", contact.Name, contact.Address);
        }

        // Create Contact2 objects by using a static factory method.
        var query2 = from i in Enumerable.Range(0, 5)
                        select Contact2.CreateContact(names[i], addresses[i]);

        // Console output is identical to query1.
        var list2 = query2.ToList();

        // List elements cannot be modified by client code.
        // CS0272:
        // list2[0].Name = "Eugene Zabokritski";

        // Keep the console open in debug mode.
        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
}

/* Output:
    Terry Adams, 123 Main St.
    Fadi Fakhouri, 345 Cypress Ave.
    Hanying Feng, 678 1st Ave
    Cesar Garcia, 12 108th St.
    Debra Garcia, 89 E. 42nd St.
*/
```

コンパイラによって、各自動実装プロパティにバッキング フィールドが作成されます。 このフィールドは、ソース コードから直接アクセスできません。

## <a name="see-also"></a>参照

- [Properties](./properties.md)
- [struct](../../language-reference/builtin-types/struct.md)
- [オブジェクト初期化子とコレクション初期化子](./object-and-collection-initializers.md)
