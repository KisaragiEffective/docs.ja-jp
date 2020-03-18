---
title: Out (ジェネリック修飾子)
ms.date: 07/20/2015
f1_keywords:
- vb.VarianceOut
helpviewer_keywords:
- Out keyword [Visual Basic]
- covariance, Out keyword [Visual Basic]
ms.assetid: c4418369-1518-4a46-9a1e-054c61038eca
ms.openlocfilehash: 0460015b44971fa638dba47183690ffcc89ca55f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351422"
---
# <a name="out-generic-modifier-visual-basic"></a>Out (ジェネリック修飾子) (Visual Basic)

ジェネリック型パラメーターの場合、`Out` キーワードは、型が共変であることを指定します。

## <a name="remarks"></a>コメント

共変性は、ジェネリック パラメーターによって指定された型よりも強い派生型を使用できるようにする機能です。 これにより、バリアント インターフェイスを実装するクラスの暗黙の型変換とデリゲート型の暗黙の型変換が可能となります。

詳細については、「[共変性と反変性](../../programming-guide/concepts/covariance-contravariance/index.md)」を参照してください。

## <a name="rules"></a>ルール

`Out` キーワードは、ジェネリック インターフェイスとデリゲートで使用できます。

ジェネリック インターフェイスでは、次の条件を満たす場合に型パラメーターを共変として宣言できます。

- 型パラメーターがインターフェイス メソッドの戻り値の型としてのみ使用され、メソッド引数の型として使用されない。

    > [!NOTE]
    > この規則には例外が 1 つあります。 共変のインターフェイスで反変の汎用デリゲートをメソッド パラメーターとして使用する場合は、共変の型をこのデリゲートのジェネリック型パラメーターとして使用できます。 共変および反変の汎用デリゲートの詳細については、「[デリゲートの変性](../../programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」および「[Func および Action 汎用デリゲートでの変性の使用](../../programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)」を参照してください。

- 型パラメーターがインターフェイス メソッドのジェネリック制約として使用されない。

ジェネリックデリゲートでは、メソッドの戻り値の型としてのみ使用され、メソッドの引数には使用されない場合、型パラメーターを共変として宣言できます。

共変性および反変性は参照型ではサポートされますが、値の型ではサポートされません。

Visual Basic では、デリゲート型を指定しなくても、共変のインターフェイスでイベントを宣言することはできません。 また、共変のインターフェイスは、入れ子になったクラス、列挙型、または構造体を持つことはできませんが、入れ子になったインターフェイスを持つことができます

## <a name="behavior"></a>動作

共変の型パラメーターを持つインターフェイスを使用すると、そのインターフェイスのメソッドは、型パラメーターによって指定された型よりも強い派生型を返すことができます。 たとえば、.NET Framework 4 の <xref:System.Collections.Generic.IEnumerable%601> では T 型が共変なので、特別な変換メソッドを使用しなくても `IEnumerable(Of String)` 型のオブジェクトを `IEnumerable(Of Object)` 型のオブジェクトに割り当てることができます。

共変のデリゲートには、型は同じでありながらより強い派生ジェネリック型パラメーターを持つ別のデリゲートを割り当てることができます。

## <a name="example"></a>例

次の例では、共変のジェネリック インターフェイスを宣言、拡張、および実装する方法を示します。 また、共変のインターフェイスを実装するクラスの暗黙的な変換を使用する方法も示します。

[!code-vb[vbVarianceKeywords#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#3)]

## <a name="example"></a>例

次の例では、共変の汎用デリゲートを宣言、インスタンス化、および呼び出す方法を示します。 また、デリゲート型に対して暗黙的な変換を使用する方法についても説明します。

[!code-vb[vbVarianceKeywords#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#4)]

## <a name="see-also"></a>参照

- [ジェネリック インターフェイスの分散](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)
