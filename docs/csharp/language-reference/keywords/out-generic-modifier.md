---
title: out キーワード (ジェネリック修飾子) - C# リファレンス
ms.date: 07/20/2015
helpviewer_keywords:
- covariance, out keyword [C#]
- out keyword [C#]
ms.assetid: f8c20dec-a8bc-426a-9882-4076b1db1e00
ms.openlocfilehash: 97ddae2efe55be89840f7a483c18d61259020283
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713285"
---
# <a name="out-generic-modifier-c-reference"></a>out (ジェネリック修飾子) (C# リファレンス)

ジェネリック型パラメーターの `out` キーワードは、型パラメーターが共変であることを指定します。 `out` キーワードは、ジェネリック インターフェイスとデリゲートで使用できます。

共変性は、ジェネリック パラメーターによって指定された型よりも強い派生型を使用できるようにする機能です。 これにより、共変性のインターフェイスを実装するクラスの暗黙の型変換とデリゲート型の暗黙の型変換が可能となります。 共変性および反変性は参照型ではサポートされますが、値の型ではサポートされません。

共変の型パラメーターを持つインターフェイスを使用すると、そのインターフェイスのメソッドは、型パラメーターによって指定された型よりも強い派生型を返すことができます。 たとえば、.NET Framework 4 の <xref:System.Collections.Generic.IEnumerable%601> では T 型が共変なので、特別な変換メソッドを使用しなくても `IEnumerable(Of String)` 型のオブジェクトを `IEnumerable(Of Object)` 型のオブジェクトに割り当てることができます。

共変のデリゲートには、型は同じでありながらより強い派生ジェネリック型パラメーターを持つ別のデリゲートを割り当てることができます。

詳細については、「[共変性と反変性](../../programming-guide/concepts/covariance-contravariance/index.md)」を参照してください。

## <a name="example---covariant-generic-interface"></a>例 - 共変のジェネリック インターフェイス

次の例では、共変のジェネリック インターフェイスを宣言、拡張、および実装する方法を示します。 また、共変のインターフェイスを実装するクラスの暗黙的な変換を使用する方法も示します。

[!code-csharp[csVarianceKeywords#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csvariancekeywords/cs/program.cs#3)]

ジェネリック インターフェイスでは、次の条件を満たす場合に型パラメーターを共変として宣言できます。

- 型パラメーターがインターフェイス メソッドの戻り値の型としてのみ使用され、メソッド引数の型として使用されない。

    > [!NOTE]
    > この規則には例外が 1 つあります。 共変のインターフェイスで反変の汎用デリゲートをメソッド パラメーターとして使用する場合は、共変の型をこのデリゲートのジェネリック型パラメーターとして使用できます。 共変および反変の汎用デリゲートの詳細については、「[デリゲートの変性](../../programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」および「[Func および Action 汎用デリゲートでの変性の使用](../../programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)」を参照してください。

- 型パラメーターがインターフェイス メソッドのジェネリック制約として使用されない。

## <a name="example---covariant-generic-delegate"></a>例 - 共変の汎用デリゲート

次の例では、共変の汎用デリゲートを宣言、インスタンス化、および呼び出す方法を示します。 また、デリゲート型を暗黙的に変換する方法も示します。

[!code-csharp[csVarianceKeywords#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csvariancekeywords/cs/program.cs#4)]

汎用デリゲートでは、メソッドの戻り値の型としてのみ使用され、メソッド引数には使用されない型を共変として宣言できます。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [ジェネリック インターフェイスの変性](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [in](in-generic-modifier.md)
- [修飾子](index.md)
