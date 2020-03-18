---
title: Private Protected (Visual Basic)
ms.date: 05/10/2018
helpviewer_keywords:
- Private Protected keyword [Visual Basic]
- Private Protected keyword [Visual Basic], syntax
ms.openlocfilehash: 265141f77f4a61a61414a07214830feaa8a1ab05
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351340"
---
# <a name="private-protected-visual-basic"></a>Private Protected (Visual Basic)

キーワード組み合わせ `Private Protected` はメンバー アクセス修飾子です。 `Private Protected` メンバーは、それを含んでいるクラスのすべてのメンバーと、それを含むクラスから派生した型によってアクセスできますが、それを含んでいるアセンブリ内に存在する場合に限ります。

`Private Protected` は、クラスのメンバーに対してのみ指定できます。構造体のメンバーに `Private Protected` を適用することはできません。構造体を継承することはできません。

`Private Protected` アクセス修飾子は Visual Basic 15.5 以降でサポートされています。 これを使用するには、次の要素を Visual Basic プロジェクト (\*.vbproj) ファイルに追加します。 システムに Visual Basic 15.5 以降がインストールされている限り、最新バージョンの Visual Basic コンパイラでサポートされているすべての言語機能を利用できます。

```xml
<PropertyGroup>
   <LangVersion>latest</LangVersion>
</PropertyGroup>
```

詳細については、「[Visual Basic 言語バージョンの設定](../../language-reference/configure-language-version.md)」を参照してください。

> [!NOTE]
> Visual Studio で、`private protected` で F1 ヘルプを選択すると、 [private](private.md)または[protected](protected.md)のヘルプが表示されます。 IDE は、複合単語ではなくカーソルの下にある1つのトークンを選択します。

## <a name="rules"></a>ルール

- **宣言コンテキスト。** `Private Protected` はクラスレベルでのみ使用できます。 つまり、`Protected` 要素の宣言コンテキストはクラスである必要があり、ソースファイル、名前空間、インターフェイス、モジュール、構造体、またはプロシージャにすることはできません。

## <a name="behavior"></a>動作

- **アクセスレベル。** クラス内のすべてのコードは、その要素にアクセスできます。 基底クラスから派生し、同じアセンブリに含まれるクラスのコードは、基底クラスのすべての `Private Protected` 要素にアクセスできます。 ただし、基底クラスから派生し、別のアセンブリに含まれているクラスのコードは、基底クラスの `Private Protected` 要素にアクセスすることはできません。

- **アクセス修飾子。** アクセス レベルを指定するキーワードは、*アクセス修飾子*と呼ばれます。 アクセス修飾子の比較については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

`Private Protected` 修飾子は、次のコンテキストで使用できます。

- 入れ子になったクラスの[Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)

- [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)

- [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)

- クラスで入れ子にされたデリゲートの[Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)

- [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)

- クラスに入れ子になっている列挙型の[Enum ステートメント](../../../visual-basic/language-reference/statements/enum-statement.md)

- [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)

- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)

- クラスで入れ子にされたインターフェイスの[Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)

- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)

- クラスで入れ子になった構造体の[Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)

- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)

## <a name="see-also"></a>関連項目

- [Public](../../../visual-basic/language-reference/modifiers/public.md)
- [Protected](../../../visual-basic/language-reference/modifiers/protected.md)
- [Friend](friend.md)
- [Private](../../../visual-basic/language-reference/modifiers/private.md)
- [Protected Friend](./protected-friend.md)
- [Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [Visual Basic におけるプロシージャ](../../../visual-basic/programming-guide/language-features/procedures/index.md)
- [構造体](../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
