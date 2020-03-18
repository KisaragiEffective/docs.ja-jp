---
title: Overrides (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- Overrides
- vb.Overrides
helpviewer_keywords:
- properties [Visual Basic], redefining
- procedures [Visual Basic], overriding
- procedures [Visual Basic], redefining
- overriding
- Overrides keyword [Visual Basic]
- overriding, Overrides keyword
- properties [Visual Basic], overriding
ms.assetid: 9f5e6144-ce10-465e-842b-1a8f8760af90
ms.openlocfilehash: 04f1cb27d6a8366c2dd13f8fdc1d975d382f1cfd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351383"
---
# <a name="overrides-visual-basic"></a>Overrides (Visual Basic)

プロパティまたはプロシージャが基本クラスから継承された同じ名前のプロパティまたはプロシージャをオーバーライドすることを示します。

## <a name="rules"></a>ルール

- **宣言コンテキスト。** `Overrides` は、プロパティまたはプロシージャの宣言ステートメントでのみ使用できます。

- **結合された修飾子。** 同じ宣言内で `Shadows` または `Shared` と共に `Overrides` を指定することはできません。 オーバーライドする要素は暗黙的にオーバーライド可能であるため、`Overridable` と `Overrides` を結合することはできません。

- **一致する署名。** この宣言のシグネチャは、オーバーライドするプロパティまたはプロシージャの*シグネチャ*と完全に一致する必要があります。 つまり、パラメーター リストには、同じ数のパラメーターを、同じ順序、同じデータ型で指定する必要があります。

  オーバーライドする宣言は、シグネチャに加え、次の点でも完全に一致している必要があります。

  - アクセス レベル

  - 戻り値の型 (戻り値がある場合)

- **汎用署名。** ジェネリック プロシージャでは、シグネチャに型パラメーターの数が含まれます。 したがって、オーバーライドする宣言は、その点でも基底クラスのバージョンに一致している必要があります。

- **追加の一致。** この宣言は、基底クラスのバージョンのシグネチャに一致していることに加え、次の点でも基底クラスと一致している必要があります。

  - アクセスレベル修飾子 ( [Public](../../../visual-basic/language-reference/modifiers/public.md)など)

  - 各パラメーターのメカニズムを渡す ([ByVal](../../../visual-basic/language-reference/modifiers/byval.md)または[ByRef](../../../visual-basic/language-reference/modifiers/byref.md))

  - ジェネリック プロシージャの型パラメーターごとの制約リスト

- **シャドウとオーバーライド。** シャドウとオーバーライドは、どちらも継承された要素を再定義しますが、その方法は大きく異なります。 詳細については、「 [Visual Basic でのシャドウ](../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)」を参照してください。

`Overrides` を使用する場合は、ライブラリ API と C# が連携しやすくなるように、コンパイラが暗黙的に `Overloads` を追加します。

`Overrides` 修飾子は、次のコンテキストで使用できます。

- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)

- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)

- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)

## <a name="see-also"></a>関連項目

- [MyBase](../../../visual-basic/language-reference/modifiers/mustoverride.md)
- [NotOverridable](../../../visual-basic/language-reference/modifiers/notoverridable.md)
- [Overridable](../../../visual-basic/language-reference/modifiers/overridable.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [Visual Basic でのシャドウ処理](../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)
- [Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [型リスト](../../../visual-basic/language-reference/statements/type-list.md)
