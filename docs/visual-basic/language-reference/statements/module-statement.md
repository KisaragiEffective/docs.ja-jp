---
title: Module ステートメント
ms.date: 07/20/2015
f1_keywords:
- Module
- vb.Module
helpviewer_keywords:
- modules, classes
- modules
- Module statement [Visual Basic]
- modules, declaring
- standard modules
- classes [Visual Basic], vs. modules
- declarations [Visual Basic], modules
ms.assetid: a1243afc-14a5-45df-95d5-51118aeac362
ms.openlocfilehash: 56fc4f9383f1fc4779358ef18a4e5c611d897eda
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348013"
---
# <a name="module-statement"></a>Module ステートメント

モジュールの名前を宣言し、モジュールが構成する変数、プロパティ、イベント、およびプロシージャの定義を紹介します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ accessmodifier ]  Module name
    [ statements ]
End Module
```

## <a name="parts"></a>指定項目

`attributelist`  
省略可。 「[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)」を参照してください。

`accessmodifier`  
省略可。 次のいずれかになります。

- [Public](../../../visual-basic/language-reference/modifiers/public.md)

- [Friend](../../../visual-basic/language-reference/modifiers/friend.md)

「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

`name`  
必須。 このモジュールの名前。 「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。

`statements`  
省略可。 モジュールの変数、プロパティ、イベント、プロシージャ、および入れ子にされた型を定義するステートメントを指定します。

`End Module`  
`Module` の定義を終了します。

## <a name="remarks"></a>コメント

`Module` ステートメントでは、名前空間全体で使用できる参照型を定義します。 *モジュール*(*標準モジュール*と呼ばれることもあります) はクラスに似ていますが、重要な違いがいくつかあります。 すべてのモジュールにはインスタンスが1つだけあり、変数を作成したり、変数に割り当てたりする必要はありません。 モジュールは、継承をサポートしていないか、インターフェイスを実装していません。 モジュールが、クラスまたは構造体の意味では*型*ではないことに注意してください。モジュールのデータ型を持つプログラミング要素を宣言することはできません。

`Module` は、名前空間レベルでのみ使用できます。 つまり、モジュールの*宣言コンテキスト*はソースファイルまたは名前空間である必要があり、クラス、構造体、モジュール、インターフェイス、プロシージャ、またはブロックにすることはできません。 モジュールは、別のモジュール内、または任意の型の中で入れ子にすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。

モジュールには、プログラムと同じ有効期間があります。 メンバーはすべて `Shared`ので、プログラムの有効期間と同じになります。

モジュールは既定で[Friend](../../../visual-basic/language-reference/modifiers/friend.md)アクセスになります。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。 詳細については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

モジュールのすべてのメンバーは、暗黙的に `Shared`ます。

## <a name="classes-and-modules"></a>クラスとモジュール

これらの要素には多くの類似点がありますが、重要な相違点もいくつかあります。

- **用語。** 以前のバージョンの Visual Basic では、*クラスモジュール*(cls ファイル)と*標準モジュール*(.bas ファイル) という 2 種類のモジュールが認識されています。 現在のバージョンは、これらをそれぞれ*クラス*と*モジュール*と呼びます。

- **共有メンバー。** クラスのメンバーが共有メンバーまたはインスタンスメンバーであるかどうかを制御できます。

- **オブジェクト指向。** クラスはオブジェクト指向ですが、モジュールはそうではありません。 したがって、オブジェクトとしてインスタンス化できるのはクラスだけです。 詳細については、「[オブジェクトとクラス](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)」を参照してください。

## <a name="rules"></a>ルール

- **修飾子。** すべてのモジュールメンバーは暗黙的に [Shared](../../../visual-basic/language-reference/modifiers/shared.md) されます。 メンバーを宣言するときに `Shared` キーワードを使用することはできません。また、メンバーの共有ステータスを変更することもできません。

- **継承。** モジュールは、すべてのモジュールが継承する <xref:System.Object>以外の型から継承することはできません。 特に、1つのモジュールが別のモジュールから継承することはできません。

  <xref:System.Object>を指定する場合でも、モジュール定義で[Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)を使用することはできません。

- **既定のプロパティ。** モジュールでは、既定のプロパティを定義することはできません。 詳細については、「[Default](../../../visual-basic/language-reference/modifiers/default.md)」を参照してください。

## <a name="behavior"></a>動作

- **アクセスレベル。** モジュール内では、各メンバーを独自のアクセス レベルで宣言できます。 モジュールメンバーの既定の [Public](../../../visual-basic/language-reference/modifiers/public.md) アクセスは、変数と定数を除き、既定で [Private](../../../visual-basic/language-reference/modifiers/private.md) アクセスになります。 モジュールがそのメンバーの 1 つ以上のアクセス権を持っている場合、指定されたモジュールへのアクセス レベルが優先されます。

- **スコープ。 ** モジュールは、名前空間全体でスコープ内にあります。

  すべてのモジュールメンバーのスコープは、モジュール全体です。 すべてのメンバーに*型の上位変換*が適用されていることに注意してください。これにより、そのスコープがモジュールを含む名前空間に昇格します。 詳細については、「[型の上位変換](../../../visual-basic/programming-guide/language-features/declared-elements/type-promotion.md)」を参照してください。

- **評価。** プロジェクトには複数のモジュールを含めることができます。また、2 つ以上の複数のモジュールで同じ名前を持つメンバーを宣言することができます。 ただし、そのようなメンバーへの参照がそのモジュールの外部からのものである場合は、適切なモジュール名を使用して修飾する必要があります。 詳細については、「[宣言された要素の参照](../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)」を参照してください。

## <a name="example"></a>例

[!code-vb[VbVbalrStatements#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#69)]

## <a name="see-also"></a>参照

- [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)
- [Namespace ステートメント](../../../visual-basic/language-reference/statements/namespace-statement.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
- [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [型の上位変換](../../../visual-basic/programming-guide/language-features/declared-elements/type-promotion.md)
