---
title: Delegate ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Delegate
helpviewer_keywords:
- delegate keyword [Visual Basic]
- Delegate statement [Visual Basic]
ms.assetid: f799c518-0817-40cc-ad0b-4da846fdba57
ms.openlocfilehash: 662d2c3c0767adfe406e0a6f1b1e6dccd704e795
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354074"
---
# <a name="delegate-statement"></a>Delegate ステートメント
デリゲートを宣言するために使用されます。 デリゲートは、型の `Shared` メソッドまたはオブジェクトのインスタンスメソッドを参照する参照型です。 パラメーターと戻り値の型が一致するプロシージャを使用すると、このデリゲートクラスのインスタンスを作成できます。 このプロシージャは、後でデリゲートインスタンスを使用して呼び出すことができます。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ <attrlist> ] [ accessmodifier ] _  
[ Shadows ] Delegate [ Sub | Function ] name [( Of typeparamlist )] [([ parameterlist ])] [ As type ]  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|定義|  
|---|---|  
|`attrlist`|任意。 このデリゲートに適用される属性のリスト。 複数の属性を指定するときは、コンマで区切ります。 [属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)は山かっこ ("`<`" と "`>`") で囲む必要があります。|  
|`accessmodifier`|任意。 デリゲートにアクセスできるコードを指定します。 次のいずれかになります。<br /><br /> - [Public](../../../visual-basic/language-reference/modifiers/public.md) デリゲートを宣言する要素にアクセスできるすべてのコードは、それにアクセスできます。<br />-   [Protected](../../../visual-basic/language-reference/modifiers/protected.md) デリゲートのクラスまたは派生クラス内のコードだけがアクセスできます。<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)。 同じアセンブリ内のコードだけが、デリゲートにアクセスできます。<br />- [Private](../../../visual-basic/language-reference/modifiers/private.md) デリゲートを宣言する要素内のコードだけがアクセスできます。<br /><br /> - [Protected Friend](../../language-reference/modifiers/protected-friend.md)デリゲートのクラス、派生クラス、または同じアセンブリ内のコードのみがデリゲートにアクセスできます。 <br />- [Private Protected](../../language-reference/modifiers/private-protected.md) デリゲートのクラス内または同じアセンブリ内の派生クラス内のコードのみがデリゲートにアクセスできます。 |  
|`Shadows`|任意。 このデリゲートが、基底クラスで同じ名前を持つプログラミング要素、またはオーバーロードされた要素のセットを直しして非表示にすることを示します。 宣言された要素は、他の任意の種類の要素でシャドウできます。<br /><br /> シャドウされた要素は、その要素をシャドウする派生クラスからは使用できません。ただし、シャドウする要素がアクセスできない要素の場合は例外です。 たとえば、`Private` 要素が基本クラス要素をシャドウする場合、`Private` 要素にアクセスするためのアクセス許可を持たないコードは、代わりに基本クラス要素にアクセスします。|  
|`Sub`|省略可能ですが、`Sub` または `Function` のいずれかが表示されている必要があります。 このプロシージャを、値を返さないデリゲート `Sub` プロシージャとして宣言します。|  
|`Function`|省略可能ですが、`Sub` または `Function` のいずれかが表示されている必要があります。 このプロシージャを、値を返すデリゲート `Function` プロシージャとして宣言します。|  
|`name`|必須。 デリゲート型の名前。標準の変数の名前付け規則に従います。|  
|`typeparamlist`|任意。 このデリゲートの型パラメーターのリスト。 複数の型パラメーターは、コンマで区切ります。 必要に応じて、`In` および `Out` ジェネリック修飾子を使用して、各型パラメーターをバリアントとして宣言できます。 [型リスト](../../../visual-basic/language-reference/statements/type-list.md)をかっこで囲み、`Of` キーワードと共に導入する必要があります。|  
|`parameterlist`|任意。 呼び出されたときにプロシージャに渡されるパラメーターのリスト。 [パラメーターリスト](../../../visual-basic/language-reference/statements/parameter-list.md)はかっこで囲む必要があります。|  
|`type`|`Function` プロシージャを指定する場合は必須です。 戻り値のデータ型。|  
  
## <a name="remarks"></a>コメント  
 `Delegate` ステートメントは、デリゲートクラスのパラメーターと戻り値の型を定義します。 パラメーターと戻り値の型が一致するプロシージャを使用すると、このデリゲートクラスのインスタンスを作成できます。 デリゲートの `Invoke` メソッドを呼び出すことによって、後でデリゲートインスタンスを使用してプロシージャを呼び出すことができます。  
  
 デリゲートは、名前空間、モジュール、クラス、または構造体レベルで宣言できますが、プロシージャ内では宣言できません。  
  
 各デリゲート クラスでは、オブジェクト メソッドの仕様を渡すコンストラクターを定義します。 デリゲート コンス トラクターに渡す引数は、メソッドへの参照、またはラムダ式である必要があります。  
  
 メソッドへの参照を指定するには、次の構文を使用します。  
  
 `AddressOf` [`expression`.]`methodname`  
  
 コンパイル時の `expression` の型は、シグネチャがデリゲート クラスのシグネチャと同じで、指定された名前のメソッドを持つクラスまたはインターフェイスである必要があります。 `methodname` は、共有メソッドまたはインスタンス メソッドのいずれかにできます。 クラスの既定メソッドに対してデリゲートを作成する場合も、`methodname` は省略できません。  
  
 ラムダ式を指定するには、次の構文を使用します。  
  
 `Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`  
  
 関数のシグネチャは、デリゲート型のシグネチャと一致している必要があります。 ラムダ式について詳しくは、「[ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」をご覧ください。  
  
 デリゲートの詳細については、「[デリゲート](../../../visual-basic/programming-guide/language-features/delegates/index.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、`Delegate` ステートメントを使用して、2つの数値を操作するデリゲートを宣言し、数値を返します。 `DelegateTest` メソッドは、この型のデリゲートのインスタンスを受け取り、それを使用して数値のペアを操作します。  
  
 [!code-vb[VbVbalrDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>関連項目

- [AddressOf 演算子](../../../visual-basic/language-reference/operators/addressof-operator.md)
- [Of](../../../visual-basic/language-reference/statements/of-clause.md)
- [デリゲート](../../../visual-basic/programming-guide/language-features/delegates/index.md)
- [方法 : ジェネリック クラスを使用する](../../../visual-basic/programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [共変性と反変性](../../programming-guide/concepts/covariance-contravariance/index.md)
- [In](../../../visual-basic/language-reference/modifiers/in-generic-modifier.md)
- [Out](../../../visual-basic/language-reference/modifiers/out-generic-modifier.md)
