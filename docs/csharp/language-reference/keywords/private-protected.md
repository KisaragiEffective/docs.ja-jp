---
title: private protected - C# リファレンス
ms.date: 11/15/2017
author: sputier
ms.openlocfilehash: a73d61712075cf24d2b94c505104df1fade629e9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713210"
---
# <a name="private-protected-c-reference"></a>private protected (C# リファレンス)

キーワード組み合わせ `private protected` はメンバー アクセス修飾子です。 private protected メンバーには、包含クラスから派生した型からアクセスできますが、その包含アセンブリ内に限られます。 `private protected` と他のアクセス修飾子の比較については、「[アクセシビリティ レベル](accessibility-levels.md)」を参照してください。

> [!NOTE]
> `private protected` アクセス修飾子は、C# バージョン 7.2 以降で有効です。

## <a name="example"></a>例

基底クラスの private protected メンバーには、変数の静的な型が派生クラス型の場合にのみ、その包含アセンブリで派生型からアクセスできます。 たとえば、次のコード セグメントを考えてみます。  

```csharp
// Assembly1.cs  
// Compile with: /target:library  
public class BaseClass
{
    private protected int myValue = 0;
}

public class DerivedClass1 : BaseClass
{
    void Access()
    {
        var baseObject = new BaseClass();

        // Error CS1540, because myValue can only be accessed by
        // classes derived from BaseClass.
        // baseObject.myValue = 5;  

        // OK, accessed through the current derived class instance
        myValue = 5;
    }
}
```

```csharp
// Assembly2.cs  
// Compile with: /reference:Assembly1.dll  
class DerivedClass2 : BaseClass
{
    void Access()
    {
        // Error CS0122, because myValue can only be
        // accessed by types in Assembly1
        // myValue = 10;
    }
}
```

この例には、2 つのファイル (`Assembly1.cs` と `Assembly2.cs`) が含まれています。
最初のファイルには public 基底クラスである `BaseClass` とそれから派生した型である `DerivedClass1` が含まれています。 `BaseClass` は private protected メンバー `myValue` を持っています。`DerivedClass1` はこれに 2 通りのアクセスを試行します。 最初に `myValue` のインスタンス経由で `BaseClass` にアクセスしようとするとエラーが出ます。 ただし、`DerivedClass1` で継承されたメンバーとして使用してみると成功します。
2 番目のファイルでは、`myValue` の継承されたメンバーとして `DerivedClass2` にアクセスしようとしてエラーを出します。これには Assembly1 の派生型のみがアクセスできるためです。

構造体は継承できないため、構造体メンバーは `private protected` になりません。  

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [アクセス修飾子](access-modifiers.md)
- [アクセシビリティ レベル](accessibility-levels.md)
- [修飾子](index.md)
- [public](public.md)
- [private](private.md)
- [internal](internal.md)
- [Internal Virtual キーワードのセキュリティ関連事項](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/heyd8kky(v=vs.100))
