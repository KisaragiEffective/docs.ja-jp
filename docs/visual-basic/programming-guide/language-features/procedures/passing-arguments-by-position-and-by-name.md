---
title: 位置と名前による引数渡し
ms.date: 02/01/2018
helpviewer_keywords:
- arguments [Visual Basic], passing by name
- procedures [Visual Basic], arguments
- := passing arguments
- procedure arguments
- Visual Basic code, procedures
- named arguments [Visual Basic], passing arguments
- arguments [Visual Basic], passing by position
- Function procedures [Visual Basic], passing arguments
- named parameters [Visual Basic], passing arguments
- named arguments
- passing parameters [Visual Basic], named parameter arguments
- passing parameters [Visual Basic], by position
- procedures [Visual Basic], calling
- named parameters
- Sub procedures [Visual Basic], passing arguments
- argument passing
- passing parameters [Visual Basic], by name
- argument passing [Visual Basic], by position
- arguments [Visual Basic], listing by name
ms.assetid: 1ad7358f-1da9-48da-a95b-f3c7ed41eff3
ms.openlocfilehash: b6588335f7634cc87a9fc14cbfc4ba80baad1abb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352618"
---
# <a name="passing-arguments-by-position-and-by-name-visual-basic"></a>位置と名前による引数渡し (Visual Basic)

`Sub` または `Function` プロシージャを呼び出すと、プロシージャの定義に出現する順序で*位置によって*引数を渡すことができます。また、位置に関係なく*名前で*渡すこともできます。

引数を名前で渡す場合は、引数の宣言名の後にコロンと等号 (`:=`) を指定し、その後に引数の値を指定します。 名前付き引数は任意の順序で指定できます。

たとえば、次の `Sub` プロシージャでは、3つの引数が使用されます。

[!code-vb[SampleProcedure](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#1)]

このプロシージャを呼び出すと、位置、名前、または両方の組み合わせを使用して引数を指定できます。

## <a name="passing-arguments-by-position"></a>渡す (位置によって引数を)

`Display` メソッドは、次の例に示すように、位置によって渡され、コンマで区切られた引数を使用して呼び出すことができます。

[!code-vb[ByPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#2)]

位置指定引数リストで省略可能な引数を省略した場合は、コンマで区切ります。 次の例では、`age` 引数を指定せずに `Display` メソッドを呼び出します。

[!code-vb[ByPositionWithOptionalArgument](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#3)]

## <a name="passing-arguments-by-name"></a>引数を名前で渡す

または、次の例に示すように、名前によって渡された引数を使用して `Display` を呼び出すこともできます。また、コンマで区切ることもできます。

[!code-vb[ByName](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#4)]

この方法で名前によって引数を渡すことは、複数の省略可能な引数を持つプロシージャを呼び出す場合に特に便利です。 名前で引数を指定する場合は、位置指定引数がないことを示すために連続するコンマを使用する必要はありません。 名前によって引数を渡すことにより、渡す引数と省略する引数を追跡しやすくなります。

## <a name="mixing-arguments-by-position-and-by-name"></a>位置と名前による引数の混在

次の例に示すように、1つのプロシージャ呼び出しで位置と名前の両方を引数として指定できます。

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#5)]

前の例では、`birth` が名前で渡されるため、省略された `age` 引数の代わりにコンマを追加する必要はありません。

15.5 より前のバージョンの Visual Basic では、位置と名前の組み合わせによって引数を指定する場合は、位置引数をすべて先に指定する必要があります。 名前で引数を指定すると、残りの引数はすべて名前で渡される必要があります。  たとえば、次の `Display` メソッドの呼び出しでは、コンパイラエラー [BC30241: 名前付き引数が必要](../../../misc/bc30241.md)です。

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#6)]

Visual Basic 15.5 以降では、位置指定引数は、終了位置引数が正しい位置にある場合に名前付き引数に従うことができます。 Visual Basic 15.5 の下でコンパイルした場合、`Display` メソッドの前の呼び出しが正常にコンパイルされ、コンパイラエラー [BC30241](../../../misc/bc30241.md)が生成されなくなります。

この機能は、名前付き引数と位置指定引数を任意の順序で組み合わせることができ、コードを読みやすくするために名前付き引数を使用する場合に特に便利です。 たとえば、次の `Person` クラスコンストラクターには、`Person`型の2つの引数が必要です。どちらも `Nothing`できます。

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#7)]

名前付き引数と位置指定引数を混在させることで、`father` 引数と `mother` 引数の値が `Nothing`場合に、コードの意図を明確にすることができます。

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#8)]

名前付き引数を使用して位置指定引数を実行するには、次の要素を Visual Basic プロジェクト (\*.vbproj) ファイルに追加する必要があります。

```xml
<PropertyGroup>
  <LangVersion>15.5</LangVersion>
</PropertyGroup>
```

詳細について[は、「Visual Basic 言語バージョンの設定](../../../language-reference/configure-language-version.md)」を参照してください。

## <a name="restrictions-on-supplying-arguments-by-name"></a>名前による引数の指定に関する制限事項

必須の引数を入力しないように、名前で引数を渡すことはできません。 省略可能な引数のみを省略できます。

パラメーター配列を名前で渡すことはできません。 これは、プロシージャを呼び出すときに、パラメーター配列に対してコンマで区切られた少数の引数を指定し、コンパイラが1つの名前に複数の引数を関連付けることができないためです。

## <a name="see-also"></a>参照

- [手順](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [省略可能なパラメーター](./optional-parameters.md)
- [パラメーター配列](./parameter-arrays.md)
- [Optional](../../../../visual-basic/language-reference/modifiers/optional.md)
- [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)
