---
title: Class ステートメント
ms.date: 05/12/2018
f1_keywords:
- vb.Class
helpviewer_keywords:
- class modules
- Class statement [Visual Basic]
- classes [Visual Basic], fields
- fields [Visual Basic], of classes
- class types [Visual Basic], class statements
- classes [Visual Basic], creating
- classes [Visual Basic], data members
- data members [Visual Basic], of classes
ms.assetid: f2664f38-eb5a-4d4b-a374-1d041521fb6c
ms.openlocfilehash: 3cb276f134e90ce3b3009234eb980d89477e0d09
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354155"
---
# <a name="class-statement-visual-basic"></a>Class ステートメント (Visual Basic)
クラスの名前を宣言し、クラスが構成されている変数、プロパティ、イベント、およびプロシージャの定義を紹介します。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ MustInherit | NotInheritable ] [ Partial ] _  
Class name [ ( Of typelist ) ]  
    [ Inherits classname ]  
    [ Implements interfacenames ]  
    [ statements ]  
End Class  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`attributelist`|省略可。 「[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)」を参照してください。|  
|`accessmodifier`|省略可。 次のいずれかになります。<br /><br /> -   [Public](../../../visual-basic/language-reference/modifiers/public.md)<br />-   [Protected](../../../visual-basic/language-reference/modifiers/protected.md)<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)<br />-   [Private](../../../visual-basic/language-reference/modifiers/private.md)<br />-   [Protected Friend](../../language-reference/modifiers/protected-friend.md)<br />- [Private Protected](../../language-reference/modifiers/private-protected.md)<br/><br/> 「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。|  
|`Shadows`|省略可。 「[Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)」を参照してください。|  
|`MustInherit`|省略可。 「[MustInherit](../../../visual-basic/language-reference/modifiers/mustinherit.md)」を参照してください。|  
|`NotInheritable`|省略可。 「[NotInheritable](../../../visual-basic/language-reference/modifiers/notinheritable.md)」を参照してください。|  
|`Partial`|省略可。 クラスの部分定義を示します。 「[Partial](../../../visual-basic/language-reference/modifiers/partial.md)」を参照してください。|  
|`name`|必須。 このクラスの名前。 「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|  
|`Of`|省略可。 これがジェネリッククラスであることを指定します。|  
|`typelist`|[Of](../../../visual-basic/language-reference/statements/of-clause.md)キーワードを使用する場合は必須です。 このクラスの型パラメーターのリスト。 [型リスト](../../../visual-basic/language-reference/statements/type-list.md)を参照してください。|  
|`Inherits`|省略可。 このクラスが別のクラスのメンバーを継承することを示します。 「[Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)」を参照してください。|  
|`classname`|`Inherits` ステートメントを使用する場合は必ず指定します。 このクラスの派生元であるクラスの名前。|  
|`Implements`|省略可。 このクラスが 1 つ以上のインターフェイスのメンバーを実装することを示します。 「[Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)」を参照してください。|  
|`interfacenames`|`Implements` ステートメントを使用する場合は必ず指定します。 このクラスが実装するインターフェイスの名前。|  
|`statements`|省略可。 このクラスのメンバーを定義するステートメント。|  
|`End Class`|必須。 `Class` の定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 `Class` ステートメントでは、新しいデータ型を定義します。 *クラス*は、オブジェクト指向プログラミング (OOP) の基本的な構成要素です。 詳細については、「[オブジェクトとクラス](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)」を参照してください。  
  
 `Class` は、名前空間またはモジュール レベルでのみ使用できます。 つまり、クラスの*宣言コンテキスト*は、ソースファイル、名前空間、クラス、構造体、モジュール、またはインターフェイスである必要があり、プロシージャまたはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
 クラスの各インスタンスは、他のすべてのインスタンスとは独立した有効期間を持ちます。 この有効期間は、[New 演算子](../../../visual-basic/language-reference/operators/new-operator.md)句によって作成されるか、<xref:Microsoft.VisualBasic.Interaction.CreateObject%2A>などの関数によって開始されます。 インスタンスを指すすべての変数が[Nothing](../../../visual-basic/language-reference/nothing.md)または他のクラスのインスタンスに設定されている場合に終了します。  
  
 クラスは既定で[Friend](../../../visual-basic/language-reference/modifiers/friend.md)アクセスになります。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。 詳細については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
## <a name="rules"></a>ルール  
  
- **入れ子。** 1 つのクラスを別のクラス内で定義できます。 外側のクラスは、*含んでいるクラス*と呼ばれ、内側のクラスは*入れ子になったクラス*と呼ばれます。  
  
- **継承。** クラスが[Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)を使用する場合、指定できる基底クラスまたはインターフェイスは1つだけです。 クラスは、複数の要素から継承することはできません。  
  
     クラスは、より制限の厳しいアクセスレベルを持つ別のクラスから継承することはできません。 たとえば、`Public` クラスは、`Friend` クラスから継承することはできません。  
  
     クラスは、入れ子にされたクラスから継承することはできません。  
  
- **ション.** クラスで[Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)を使用する場合は、`interfacenames`で指定するすべてのインターフェイスで定義されているすべてのメンバーを実装する必要があります。 この例外は、基底クラスのメンバーの再実装です。 詳細については、「[Implements](../../../visual-basic/language-reference/statements/implements-clause.md)」の「再実装」を参照してください。  
  
- **既定のプロパティ。** クラスは、*既定のプロパティ*として最大で1つのプロパティを指定できます。 詳細については、「[Default](../../../visual-basic/language-reference/modifiers/default.md)」を参照してください。  
  
## <a name="behavior"></a>動作  
  
- **アクセスレベル。** クラス内では、各メンバーを独自のアクセスレベルで宣言できます。 クラスメンバーの既定の[Public](../../../visual-basic/language-reference/modifiers/public.md)アクセスは、変数と定数を除き、既定で[Private](../../../visual-basic/language-reference/modifiers/private.md)アクセスになります。 クラスのメンバーの1つよりもアクセスが制限されている場合は、クラスのアクセスレベルが優先されます。  
  
- **検索.** クラスは、名前空間、クラス、構造体、またはモジュールを含んでいる範囲内にあります。  
  
     すべてのクラスメンバーのスコープは、クラス全体です。  
  
     **最短.** Visual Basic は、静的クラスをサポートしていません。 静的クラスに相当する機能は、モジュールによって提供されます。 詳細については、「[Module ステートメント](../../../visual-basic/language-reference/statements/module-statement.md)」を参照してください。  
  
     クラスメンバーの有効期間は、どのように宣言されているかによって異なります。 詳細については、「[Visual Basic の有効期間](../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)」を参照してください。  
  
- **評価.** クラスの外部のコードでは、メンバーの名前をそのクラスの名前で修飾する必要があります。  
  
     入れ子になったクラス内のコードがプログラミング要素への修飾されていない参照を作成する場合、Visual Basic は、入れ子になったクラスで最初に要素を検索し、次にそれを含んでいるクラスの要素を検索します。  
  
## <a name="classes-and-modules"></a>クラスとモジュール  
 これらの要素には多くの類似点がありますが、重要な相違点もいくつかあります。  
  
- **用語。** 以前のバージョンの Visual Basic では、*クラスモジュール*(cls ファイル) と*標準モジュール*(.bas ファイル) という 2 種類のモジュールが認識されています。 現在のバージョンは、これらの*クラス*と*モジュール*をそれぞれ呼び出します。  
  
- **共有メンバー。** クラスのメンバーが共有メンバーまたはインスタンスメンバーであるかどうかを制御できます。  
  
- **オブジェクトの向き。** クラスはオブジェクト指向ですが、モジュールはそうではありません。 クラスの1つ以上のインスタンスを作成できます。 詳細については、「[オブジェクトとクラス](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`Class` ステートメントを使用して、クラスと複数のメンバーを定義しています。  
  
 [!code-vb[VbVbalrStatements#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#62)]  
  
## <a name="see-also"></a>参照

- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [構造体とクラス](../../../visual-basic/programming-guide/language-features/data-types/structures-and-classes.md)
- [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)
- [Module ステートメント](../../../visual-basic/language-reference/statements/module-statement.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [オブジェクトの有効期間 : オブジェクトの作成と破棄](../../../visual-basic/programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
- [Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [方法 : ジェネリック クラスを使用する](../../../visual-basic/programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
