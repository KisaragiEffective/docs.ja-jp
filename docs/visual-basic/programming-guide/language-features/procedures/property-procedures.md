---
title: Property プロシージャ
ms.date: 07/20/2015
helpviewer_keywords:
- Set statement [Visual Basic], Property procedures
- Visual Basic code, procedures
- return values [Visual Basic], Property procedures
- syntax [Visual Basic], Property procedures
- procedures [Visual Basic], property
- Visual Basic code, properties
- procedures [Visual Basic], calling
- properties [Visual Basic], custom
- property procedures
- Get statement [Visual Basic], property procedures
ms.assetid: 46a98379-e1a2-45dd-a48c-b51213f5ab07
ms.openlocfilehash: a4b8ac3e27348764f537ee9502ce1fbb165bb3ef
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352560"
---
# <a name="property-procedures-visual-basic"></a>Property プロシージャ (Visual Basic)

プロパティプロシージャは、モジュール、クラス、または構造体のカスタムプロパティを操作する一連の Visual Basic ステートメントです。 プロパティプロシージャは、*プロパティアクセサー*とも呼ばれます。

Visual Basic には、次のプロパティプロシージャが用意されています。

- `Get` プロシージャは、プロパティの値を返します。 これは、式のプロパティにアクセスするときに呼び出されます。
- `Set` プロシージャは、オブジェクト参照を含む、プロパティを値に設定します。 これは、プロパティに値を割り当てるときに呼び出されます。

通常は、`Get` ステートメントと `Set` ステートメントを使用して、ペアでプロパティプロシージャを定義しますが、プロパティが読み取り専用 ([Get ステートメント](../../../../visual-basic/language-reference/statements/get-statement.md)) または書き込み専用 ([Set ステートメント](../../../../visual-basic/language-reference/statements/set-statement.md)) の場合は、いずれかのプロシージャだけを定義できます。

自動実装プロパティを使用する場合は、`Get` と `Set` の手順を省略できます。 詳細については、「[自動実装プロパティ](./auto-implemented-properties.md)」を参照してください。

クラス、構造体、およびモジュールのプロパティを定義できます。 プロパティは既定で `Public` されます。これは、プロパティのコンテナーにアクセスできるアプリケーション内の任意の場所から呼び出すことができることを意味します。

プロパティと変数の比較については、「[Visual Basic のプロパティと変数の違い](differences-between-properties-and-variables.md)」を参照してください。

## <a name="declaration-syntax"></a>宣言の構文

プロパティ自体は、[Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)および `End Property` ステートメント内に囲まれたコードのブロックによって定義されます。 このブロックの内部では、各プロパティプロシージャは、宣言ステートメント (`Get` または `Set`) と一致する `End` 宣言内に囲まれた内部ブロックとして表示されます。

プロパティとそのプロシージャを宣言する構文は次のとおりです。

```vb
[Default] [Modifiers] Property PropertyName[(ParameterList)] [As DataType]
    [AccessLevel] Get
        ' Statements of the Get procedure.
        ' The following statement returns an expression as the property's value.
        Return Expression
    End Get
    [AccessLevel] Set[(ByVal NewValue As DataType)]
        ' Statements of the Set procedure.
        ' The following statement assigns newvalue as the property's value.
        LValue = NewValue
    End Set
End Property
' - or -
[Default] [Modifiers] Property PropertyName [(ParameterList)] [As DataType]
```

`Modifiers` は、オーバーロード、オーバーライド、共有、およびシャドウに関するアクセスレベルと情報、およびプロパティが読み取り専用であるか書き込み専用であるかを指定できます。 `Get` または `Set` プロシージャの `AccessLevel` には、プロパティ自体に対して指定されたアクセスレベルよりも制限の厳しい任意のレベルを指定できます。 詳細については、「[Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)」を参照してください。

### <a name="data-type"></a>データ型

プロパティのデータ型とプリンシパルアクセスレベルは、プロパティプロシージャではなく、`Property` ステートメントで定義されます。 プロパティは、データ型を1つだけ持つことができます。 たとえば、`Decimal` 値を格納するプロパティを定義することはできませんが、`Double` 値を取得することはできません。

### <a name="access-level"></a>アクセスレベル

ただし、プロパティのプリンシパルアクセスレベルを定義し、そのプロパティプロシージャのいずれかでアクセスレベルをさらに制限することができます。 たとえば、`Public` プロパティを定義し、`Private Set` プロシージャを定義できます。 `Get` プロシージャは `Public`のままです。 アクセスレベルを変更できるのは、プロパティのプロシージャの1つだけです。このアクセスレベルは、プリンシパルアクセスレベルよりも制限されます。 詳細については、「[方法: 混合アクセスレベルを持つプロパティを宣言](how-to-declare-a-property-with-mixed-access-levels.md)する」を参照してください。

## <a name="parameter-declaration"></a>パラメーターの宣言

各パラメータの宣言は、[Sub プロシージャ](sub-procedures.md)の場合と同じ方法で行います。ただし、引き渡し方法は`ByVal`である必要があります。

パラメーターリストの各パラメーターの構文は次のとおりです。

```vb
[Optional] ByVal [ParamArray] parametername As datatype
```

パラメーターが省略可能な場合は、宣言の一部として既定値を指定する必要もあります。 既定値を指定する構文は次のとおりです。

```vb
Optional ByVal parametername As datatype = defaultvalue
```

## <a name="property-value"></a>［プロパティ値］

`Get` のプロシージャでは、プロパティの値として呼び出し元の式に戻り値が指定されます。

`Set` のプロシージャでは、新しいプロパティ値が `Set` ステートメントのパラメーターに渡されます。 パラメーターを明示的に宣言する場合は、プロパティと同じデータ型を使用して宣言する必要があります。 パラメーターを宣言しない場合、コンパイラは、暗黙的なパラメーター `Value` を使用して、プロパティに割り当てられる新しい値を表します。

## <a name="calling-syntax"></a>呼び出し構文

プロパティを参照することによって、プロパティプロシージャを暗黙的に呼び出します。 プロパティの名前は、変数の名前を使用するのと同じ方法で使用します。ただし、省略可能なすべての引数の値を指定する必要があります。また、引数リストをかっこで囲む必要があります。 引数を指定しない場合は、必要に応じてかっこを省略できます。

`Set` プロシージャへの暗黙的な呼び出しの構文は次のとおりです。

```vb
propertyname[(argumentlist)] = expression
```

`Get` プロシージャへの暗黙的な呼び出しの構文は次のとおりです。

```vb
lvalue = propertyname[(argumentlist)]
Do While (propertyname[(argumentlist)] > expression)
```

### <a name="illustration-of-declaration-and-call"></a>宣言と呼び出しの図

次のプロパティは、完全名を2つの構成名 (名と姓) として格納します。 呼び出し元のコードが `fullName`を読み取る場合、`Get` プロシージャは2つの構成名を結合し、完全な名前を返します。 呼び出し元のコードによって新しい完全名が割り当てられると、`Set` プロシージャは2つの構成名に分割しようとします。 スペースが見つからない場合は、そのすべてが最初の名前として格納されます。

[!code-vb[VbVbcnProcedures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#8)]

次の例は、`fullName`のプロパティプロシージャの一般的な呼び出しを示しています。

[!code-vb[VbVbcnProcedures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#9)]

## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](index.md)
- [Function プロシージャ](function-procedures.md)
- [演算子プロシージャ](operator-procedures.md)
- [プロシージャのパラメーターと引数](procedure-parameters-and-arguments.md)
- [Visual Basic のプロパティと変数の違い](differences-between-properties-and-variables.md)
- [方法 : プロパティを作成する](how-to-create-a-property.md)
- [方法 : プロパティ プロシージャを呼び出す](how-to-call-a-property-procedure.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティに値を格納する](how-to-put-a-value-in-a-property.md)
- [方法 : プロパティから値を取得する](how-to-get-a-value-from-a-property.md)
