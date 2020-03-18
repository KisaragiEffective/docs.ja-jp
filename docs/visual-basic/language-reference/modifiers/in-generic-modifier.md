---
title: In (ジェネリック修飾子)-Visual Basic
ms.date: 07/20/2015
f1_keywords:
- vb.VarianceIn
helpviewer_keywords:
- contravariance, In keyword [Visual Basic]
- In keyword [Visual Basic]
ms.assetid: 59bb13c5-fe96-42b8-8286-86293d1661c5
ms.openlocfilehash: 9c0f7d454767112e1e309af81407b5fdef22eee9
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004869"
---
# <a name="in-generic-modifier-visual-basic"></a>In (ジェネリック修飾子) (Visual Basic)

ジェネリック型パラメーターの `In` キーワードは、型パラメーターが反変であることを指定します。

## <a name="remarks"></a>コメント

反変性は、ジェネリック パラメーターによって指定された型よりも弱い派生型を使用できるようにする機能です。 これにより、バリアント インターフェイスを実装するクラスの暗黙の型変換とデリゲート型の暗黙の型変換が可能となります。

詳細については、「[共変性と反変性](../../programming-guide/concepts/covariance-contravariance/index.md)」を参照してください。

## <a name="rules"></a>ルール

`In` キーワードは、ジェネリック インターフェイスとデリゲートで使用できます。
  
型パラメーターは、ジェネリックインターフェイスまたはデリゲートで反変として宣言できます。メソッド引数の型としてのみ使用され、メソッドの戻り値の型としては使用されません。 `ByRef` パラメーターを共変または反変にすることはできません。

共変性と反変性は参照型でサポートされており、値型ではサポートされていません。

Visual Basic では、デリゲート型を指定せずに反変のインターフェイスでイベントを宣言することはできません。 また、反変のインターフェイスは、入れ子になったクラス、列挙型、または構造体を持つことはできませんが、入れ子になったインターフェイスを持つことができます

## <a name="behavior"></a>動作

反変の型パラメーターを持つインターフェイスを使用すると、そのインターフェイスのメソッドは、インターフェイス型パラメーターによって指定された型よりも弱い派生型の引数を受け取ることができます。 たとえば、.NET Framework 4 の <xref:System.Collections.Generic.IComparer%601> インターフェイスでは、型 T は反変であるため、`Employee` から継承される場合は、特殊な変換メソッドを使用せずに、`IComparer(Of Person)` 型のオブジェクトを `IComparer(Of Employee)` 型のオブジェクトに割り当てることができます。`Person`

反変のデリゲートには、型は同じでありながらより弱い派生ジェネリック型パラメーターを持つ別のデリゲートを割り当てることができます。

## <a name="example---contravariant-generic-interface"></a>例-反変のジェネリックインターフェイス

次の例では、反変のジェネリック インターフェイスを宣言、拡張、および実装する方法を示します。 また、このインターフェイスを実装するクラスの暗黙的な変換を使用する方法も示します。

[!code-vb[vbVarianceKeywords#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#1)]

## <a name="example---contravariant-generic-delegate"></a>例-反変の汎用デリゲート

次の例では、反変の汎用デリゲートを宣言、インスタンス化、および呼び出す方法を示します。 また、デリゲート型を暗黙的に変換する方法も示します。

[!code-vb[vbVarianceKeywords#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#2)]

## <a name="see-also"></a>参照

- [ジェネリック インターフェイスの分散](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [Out](out-generic-modifier.md)
