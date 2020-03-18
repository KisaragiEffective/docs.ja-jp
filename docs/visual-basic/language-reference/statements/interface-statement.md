---
title: Interface ステートメント (Visual Basic)
ms.date: 05/12/2018
f1_keywords:
- vb.Interface
helpviewer_keywords:
- interface statement [Visual Basic]
- interfaces [Visual Basic], interface definition
ms.assetid: 8997af73-bda3-4f79-bd41-ca396b610260
ms.openlocfilehash: b606f24cf3baa13746834dfbf7ca6b9215fd3558
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348056"
---
# <a name="interface-statement-visual-basic"></a>Interface ステートメント (Visual Basic)
インターフェイスの名前を宣言し、インターフェイスが構成されているメンバーの定義を紹介します。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ <attributelist> ] [ accessmodifier ] [ Shadows ] _  
Interface name [ ( Of typelist ) ]  
    [ Inherits interfacenames ]  
    [ [ modifiers ] Property membername ]  
    [ [ modifiers ] Function membername ]  
    [ [ modifiers ] Sub membername ]  
    [ [ modifiers ] Event membername ]  
    [ [ modifiers ] Interface membername ]  
    [ [ modifiers ] Class membername ]  
    [ [ modifiers ] Structure membername ]  
End Interface  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`attributelist`|省略可。 「[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)」を参照してください。|  
|`accessmodifier`|省略可。 次のいずれかになります。<br /><br /> -   [Public](../../../visual-basic/language-reference/modifiers/public.md)<br />-     [Protected](../../../visual-basic/language-reference/modifiers/protected.md)<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)<br />-   [Private](../../../visual-basic/language-reference/modifiers/private.md)<br />-  [Protected Friend](../../language-reference/modifiers/protected-friend.md)<br/>- [Private Protected](../../language-reference/modifiers/private-protected.md)<br /><br /> 「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。|  
|`Shadows`|省略可。 「[Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)」を参照してください。|  
|`name`|必須。 このインターフェイスの名前。 「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|  
|`Of`|省略可。 これがジェネリックインターフェイスであることを指定します。|  
|`typelist`|[Of](../../../visual-basic/language-reference/statements/of-clause.md)キーワードを使用する場合は必須です。 このインターフェイスの型パラメーターのリスト。 必要に応じて、`In` および `Out` ジェネリック修飾子を使用して、各型パラメーターをバリアントとして宣言できます。 [型リスト](../../../visual-basic/language-reference/statements/type-list.md)を参照してください。|  
|`Inherits`|省略可。 このインターフェイスが、別のインターフェイスまたはインターフェイスの属性とメンバーを継承することを示します。 「[Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)」を参照してください。|  
|`interfacenames`|`Inherits` ステートメントを使用する場合は必ず指定します。 このインターフェイスの派生元のインターフェイスの名前。|  
|`modifiers`|省略可。 定義されているインターフェイスメンバーの適切な修飾子。|  
|`Property`|省略可。 インターフェイスのメンバーであるプロパティを定義します。|  
|`Function`|省略可。 インターフェイスのメンバーである `Function` プロシージャを定義します。|  
|`Sub`|省略可。 インターフェイスのメンバーである `Sub` プロシージャを定義します。|  
|`Event`|省略可。 インターフェイスのメンバーであるイベントを定義します。|  
|`Interface`|省略可。 このインターフェイス内で入れ子になっているインターフェイスを定義します。 入れ子になったインターフェイスの定義は、`End Interface` ステートメントで終了する必要があります。|  
|`Class`|省略可。 インターフェイスのメンバーであるクラスを定義します。 メンバークラスの定義は、`End Class` ステートメントで終了する必要があります。|  
|`Structure`|省略可。 インターフェイスのメンバーである構造体を定義します。 メンバー構造の定義は、`End Structure` ステートメントで終了する必要があります。|  
|`membername`|インターフェイスのメンバーとして定義されているプロパティ、プロシージャ、イベント、インターフェイス、クラス、または構造体ごとに必須です。 メンバーの名前。|  
|`End Interface`|`Interface` の定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 *インターフェイス*は、クラスおよび構造体が実装できるメンバーのセット (プロパティやプロシージャなど) を定義します。 インターフェイスは、内部動作ではなく、メンバーのシグネチャのみを定義します。  
  
 クラスまたは構造体は、インターフェイスで定義されているすべてのメンバーに対してコードを提供することによって、インターフェイスを実装します。 最後に、アプリケーションがそのクラスまたは構造体からインスタンスを作成すると、オブジェクトが存在し、メモリ内に実行されます。 詳細については、「[オブジェクトとクラス](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)および[インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)」を参照してください。  
  
 `Interface` は、名前空間またはモジュール レベルでのみ使用できます。 つまり、インターフェイスの*宣言コンテキスト*は、ソースファイル、名前空間、クラス、構造体、モジュール、またはインターフェイスである必要があり、プロシージャまたはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
 インターフェイスは、既定で [Friend](../../../visual-basic/language-reference/modifiers/friend.md)アクセスに設定されます。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。 詳細については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
## <a name="rules"></a>ルール  
  
- **インターフェイスの入れ子。** 1 つのインターフェイスを別のインターフェイス内に定義できます。 外側のインターフェイスは、*包含インターフェイス*と呼ばれ、内側のインターフェイスは*入れ子になったインターフェイス*と呼ばれます。  
  
- **メンバー宣言。** インターフェイスのメンバーとしてプロパティまたはプロシージャを宣言する場合は、そのプロパティまたはプロシージャの*シグネチャ*のみを定義します。 これには、要素の型 (プロパティまたはプロシージャ)、パラメーターとパラメーターの型、および戻り値の型が含まれます。 このため、メンバー定義は1行のコードだけを使用し、`End Function` や `End Property` などの終了ステートメントは、インターフェイスでは有効ではありません。  
  
     これに対し、列挙体、構造体、または入れ子になったクラスまたはインターフェイスを定義する場合は、そのデータメンバーを含める必要があります。  
  
- **メンバー修飾子。** モジュールメンバーを定義するときにアクセス修飾子を使用することはできません。また、[Overloads](../../../visual-basic/language-reference/modifiers/overloads.md)を除く[Shared](../../../visual-basic/language-reference/modifiers/shared.md)またはプロシージャ修飾子を指定することもできません。 [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)を使用して任意のメンバーを宣言できます。また、プロパティを定義するときには、 [ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md)または[WriteOnly](../../../visual-basic/language-reference/modifiers/writeonly.md)として[Default](../../../visual-basic/language-reference/modifiers/default.md)を使用できます。  
  
- **継承。** インターフェイスで[Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)が使用されている場合は、1つまたは複数の基本インターフェイスを指定できます。 それぞれが同じ名前のメンバーを定義している場合でも、2つのインターフェイスから継承できます。 その場合、実装するコードでは、実装するメンバーを指定するために名前の修飾を使用する必要があります。  
  
     インターフェイスは、より制限の厳しいアクセスレベルを持つ別のインターフェイスから継承することはできません。 たとえば、`Public` インターフェイスは、`Friend` インターフェイスから継承することはできません。  
  
     インターフェイスは、その中に入れ子にされたインターフェイスから継承することはできません。  
  
- **ション.** クラスが[Implements](../../../visual-basic/language-reference/statements/implements-clause.md)ステートメントを使用してこのインターフェイスを実装する場合は、インターフェイス内で定義されているすべてのメンバーを実装する必要があります。 さらに、実装コード内の各シグネチャは、このインターフェイスで定義されている対応するシグネチャと正確に一致する必要があります。 ただし、実装するコード内のメンバーの名前が、インターフェイスで定義されているメンバー名と一致する必要はありません。  
  
     クラスがプロシージャを実装している場合、プロシージャを `Shared`として指定することはできません。  
  
- **既定のプロパティ。** インターフェイスでは、プロパティ名を使用せずに参照できる、最大で1つのプロパティを*既定のプロパティ*として指定できます。 このようなプロパティを指定するには、[既定](../../../visual-basic/language-reference/modifiers/default.md)の修飾子を使用して宣言します。  
  
     これは、インターフェイスが none を継承する場合にのみ、既定のプロパティを定義できることに注意してください。  
  
## <a name="behavior"></a>動作  
  
- **アクセスレベル。** すべてのインターフェイスメンバーは、暗黙的に [Public](../../../visual-basic/language-reference/modifiers/public.md) アクセスを持ちます。 メンバーを定義するときに、アクセス修飾子を使用することはできません。 ただし、インターフェイスを実装するクラスは、実装されている各メンバーのアクセスレベルを宣言できます。  
  
     クラスインスタンスを変数に割り当てる場合、そのメンバーのアクセスレベルは、変数のデータ型が基になるインターフェイスまたは実装しているクラスであるかどうかによって異なります。 これを次の例に示します。  
  
     [!code-vb[VbVbalrStatements#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#39)]  
  
     `varAsInterface`を通じてクラスメンバーにアクセスすると、すべてのメンバーにパブリックアクセス権が与えられます。 ただし、`varAsClass`を通じてメンバーにアクセスする場合は、`doSomething` `Sub` プロシージャにプライベートアクセスがあります。  
  
- **スコープ。 ** インターフェイスは、名前空間、クラス、構造体、またはモジュール全体でスコープ内にあります。  
  
     すべてのインターフェイスメンバーのスコープは、インターフェイス全体です。  
  
- **有効期間。 ** インターフェイスには、それ自体が有効期間やメンバーを持つことはありません。 クラスがインターフェイスを実装し、そのクラスのインスタンスとしてオブジェクトが作成される場合、オブジェクトは、そのオブジェクトが実行されているアプリケーション内の有効期間を持ちます。 詳細については、「 [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)」の「有効期間」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`Interface` ステートメントを使用して `thisInterface`という名前のインターフェイスを定義します。これは、`Property` ステートメントと `Function` ステートメントで実装する必要があります。  
  
 [!code-vb[VbVbalrStatements#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#40)]  
  
 `Property` ステートメントと `Function` ステートメントでは、`End Property` で終わるブロックと、インターフェイス内の `End Function` が導入されていないことに注意してください。 インターフェイスは、メンバーのシグネチャのみを定義します。 完全な `Property` と `Function` ブロックは `thisInterface`を実装するクラスに表示されます。  
  
## <a name="see-also"></a>参照

- [インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)
- [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)
- [Module ステートメント](../../../visual-basic/language-reference/statements/module-statement.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [ジェネリック インターフェイスの分散](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)
- [Out](../../../visual-basic/language-reference/modifiers/out-generic-modifier.md)
