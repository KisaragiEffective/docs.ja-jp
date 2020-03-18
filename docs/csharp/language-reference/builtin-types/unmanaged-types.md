---
title: アンマネージド型 - C# リファレンス
ms.date: 09/06/2019
helpviewer_keywords:
- unmanaged type [C#]
ms.openlocfilehash: 8a4599514115aa21f17c32848ce203fea704072e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78846466"
---
# <a name="unmanaged-types-c-reference"></a>アンマネージド型 (C# リファレンス)

型は、次のいずれかの型である場合、**アンマネージド型**です。

- `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、または `bool`
- すべての[列挙](enum.md)型
- すべての[ポインター](../../programming-guide/unsafe-code-pointers/pointer-types.md) 型
- アンマネージド型のフィールドのみが含まれるすべてのユーザー定義の[構造体](struct.md)型で、かつ C# 7.3 以前の場合は、構築された型 (1 つ以上の型引数が含まれる型) でない

C# 7.3 以降、[`unmanaged` 制約](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint)を使用して、型パラメーターが非ポインターで、null 非許容で、アンマネージド型であることを指定できます。

C# 8.0 以降では、次の例に示すように、アンマネージド型のフィールドのみが含まれる "*構築された*" 構造体型もアンマネージド型になります。

[!code-csharp[unmanaged constructed types](snippets/UnmanagedTypes.cs#ProgramExample)]

ジェネリック構造体は、構築されたアンマネージド型およびアンマネージドでない型の両方のソースになる場合があります。 前の例では、ジェネリック構造体 `Coords<T>` を定義し、構築されたアンマネージド型の例を示します。 アンマネージド型でない例は `Coords<object>` です。 アンマネージドでない `object` 型のフィールドがあるため、これはアンマネージドではありません。 構築された "*すべての*" 型をアンマネージド型にする場合は、ジェネリック構造体の定義で `unmanaged` 制約を使用します。

[!code-csharp[unmanaged constraint in type definition](snippets/UnmanagedTypes.cs#AlwaysUnmanaged)]

## <a name="c-language-specification"></a>C# 言語仕様

詳しくは、「[C# 言語仕様](~/_csharplang/spec/unsafe-code.md#pointer-types)」の「[ポインター型](~/_csharplang/spec/introduction.md)」をご覧ください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [ポインター型](../../programming-guide/unsafe-code-pointers/pointer-types.md)
- [メモリおよびスパンに関連する型](../../../standard/memory-and-spans/index.md)
- [sizeof 演算子](../operators/sizeof.md)
- [stackalloc 演算子](../operators/stackalloc.md)
