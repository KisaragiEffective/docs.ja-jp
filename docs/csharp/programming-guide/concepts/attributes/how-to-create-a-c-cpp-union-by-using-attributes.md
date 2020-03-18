---
title: 属性を使用して C/C++ の共用体を作成する方法 (C#)
ms.date: 07/20/2015
ms.assetid: 85f35e56-26e0-4d31-9f3a-89bd4005e71a
ms.openlocfilehash: ff8ce560444581a28b257820573224f89a274cd9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141573"
---
# <a name="how-to-create-a-cc-union-by-using-attributes-c"></a>属性を使用して C/C++ の共用体を作成する方法 (C#)

属性を使用すると、構造体のメモリ内での配置をカスタマイズできます。 たとえば、`StructLayout(LayoutKind.Explicit)` 属性と `FieldOffset` 属性を使用すると、C/C++ の共用体と呼ばれるものを作成できます。

## <a name="example"></a>例

このコード セグメントでは、`TestUnion` のすべてのフィールドがメモリ内の同じ場所で開始されます。

```csharp
// Add a using directive for System.Runtime.InteropServices.

[System.Runtime.InteropServices.StructLayout(LayoutKind.Explicit)]
struct TestUnion
{
    [System.Runtime.InteropServices.FieldOffset(0)]
    public int i;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public double d;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public char c;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public byte b;
}
```

## <a name="example"></a>例

次の例でも、明示的に設定されたさまざまな場所でフィールドが開始されます。

```csharp
// Add a using directive for System.Runtime.InteropServices.

[System.Runtime.InteropServices.StructLayout(LayoutKind.Explicit)]
struct TestExplicit
{
    [System.Runtime.InteropServices.FieldOffset(0)]
    public long lg;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public int i1;

    [System.Runtime.InteropServices.FieldOffset(4)]
    public int i2;

    [System.Runtime.InteropServices.FieldOffset(8)]
    public double d;

    [System.Runtime.InteropServices.FieldOffset(12)]
    public char c;

    [System.Runtime.InteropServices.FieldOffset(14)]
    public byte b;
}
```

2 つの整数フィールド、`i1` および `i2` は、`lg` と同じメモリ位置を共有します。 このような構造体配置の制御は、プラットフォームを呼び出すときに便利です。

## <a name="see-also"></a>参照

- <xref:System.Reflection>
- <xref:System.Attribute>
- [C# プログラミングガイド](../../index.md)
- [属性](../../../../standard/attributes/index.md)
- [リフレクション (C#)](../reflection.md)
- [属性 (C#)](index.md)
- [カスタム属性の作成 (C#)](creating-custom-attributes.md)
- [リフレクションを使用した属性へのアクセス (C#)](accessing-attributes-by-using-reflection.md)
