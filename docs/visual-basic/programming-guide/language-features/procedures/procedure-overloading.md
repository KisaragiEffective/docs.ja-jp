---
title: プロシージャのオーバーロード
ms.date: 07/20/2015
helpviewer_keywords:
- signatures
- Overloads keyword [Visual Basic]
- hiding by signature
- Visual Basic code, procedures
- procedures [Visual Basic], signatures for
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
- parameters [Visual Basic], lists
- signatures [Visual Basic], procedure
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- Shadows keyword [Visual Basic]
- procedure overloading
- procedures [Visual Basic], parameter lists
ms.assetid: fbc7fb18-e3b2-48b6-b554-64c00ed09d2a
ms.openlocfilehash: 41a971896fe726cbe9849fd46334910e7288afe0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352598"
---
# <a name="procedure-overloading-visual-basic"></a>プロシージャのオーバーロード (Visual Basic)

プロシージャを*オーバーロード*することは、同じ名前でパラメーターリストが異なる複数のバージョンで定義することを意味します。 オーバーロードの目的は、プロシージャの名前を区別せずに、密接に関連する複数のバージョンを定義することです。 これを行うには、パラメーターリストを変更します。

## <a name="overloading-rules"></a>オーバーロード (規則を)

プロシージャをオーバーロードすると、次の規則が適用されます。

- **同じ名前**です。 オーバーロードされた各バージョンは、同じプロシージャ名を使用する必要があります。

- **署名が異なり**ます。 オーバーロードされた各バージョンは、次のいずれかの点で、他のすべてのオーバーロードされたバージョンと異なる必要があります。

  - パラメーターの数

  - パラメーターの順序

  - パラメーターのデータ型

  - 型パラメーターの数 (ジェネリックプロシージャの場合)

  - 戻り値の型 (変換演算子の場合のみ)

  プロシージャ名と共に、前の項目は総称してプロシージャの*シグネチャ*と呼ばれます。 オーバーロードされたプロシージャを呼び出すと、コンパイラはシグネチャを使用して、呼び出しが定義と正しく一致することを確認します。

- **項目はシグネチャの一部ではありません**。 シグネチャを変更せずにプロシージャをオーバーロードすることはできません。 特に、次の項目のうち1つ以上を変更してプロシージャをオーバーロードすることはできません。

  - プロシージャ修飾子キーワード (`Public`、`Shared`、`Static` など)

  - パラメーターまたは型パラメーター名

  - 型パラメーターの制約 (ジェネリックプロシージャの場合)

  - `ByRef` や `Optional` などのパラメーター修飾子キーワード

  - 値を返すかどうか

  - 戻り値のデータ型 (変換演算子を除く)

  前の一覧の項目は、署名に含まれていません。 オーバーロードされたバージョンを区別するために使用することはできませんが、シグネチャによって適切に区別されるオーバーロードされたバージョン間で変更することはできます。

- **遅延**バインディングされた引数。 遅延バインディングされたオブジェクト変数をオーバーロードされたバージョンに渡す場合は、適切なパラメーターを <xref:System.Object>として宣言する必要があります。

## <a name="multiple-versions-of-a-procedure"></a>プロシージャの複数のバージョン

顧客の残高に対してトランザクションを投稿するための `Sub` プロシージャを記述していて、名前またはアカウント番号で顧客を参照できるようにするとします。 これに対応するために、次の例に示すように、2つの異なる `Sub` プロシージャを定義できます。

[!code-vb[VbVbcnProcedures#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#73)]

### <a name="overloaded-versions"></a>オーバーロードされたバージョン

別の方法として、1つのプロシージャ名をオーバーロードする方法があります。 次のように、 [Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)キーワードを使用して、各パラメーターリストのプロシージャのバージョンを定義できます。

[!code-vb[VbVbcnProcedures#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#72)]

#### <a name="additional-overloads"></a>追加のオーバーロード

また、`Decimal` または `Single`でトランザクション量を受け入れる場合は、`post` をオーバーロードして、このバリエーションに対応できるようにすることもできます。 前の例の各オーバーロードに対してこれを行った場合は、4つの `Sub` プロシージャがありますが、これらはすべて同じ名前ですが、4つの異なるシグネチャを持ちます。

## <a name="advantages-of-overloading"></a>オーバーロードの利点

プロシージャをオーバーロードする利点は、呼び出しの柔軟性です。 前の例で宣言した `post` プロシージャを使用するには、呼び出し元のコードが `String` または `Integer`として顧客 id を取得し、どちらの場合でも同じプロシージャを呼び出すことができます。 次に例を示します。

[!code-vb[VbVbcnProcedures#56](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#56)]

[!code-vb[VbVbcnProcedures#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#57)]

## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [方法 : プロシージャの複数のバージョンを定義する](./how-to-define-multiple-versions-of-a-procedure.md)
- [方法 : オーバーロードされたプロシージャを呼び出す](./how-to-call-an-overloaded-procedure.md)
- [方法 : 省略可能なパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [方法 : 不特定数のパラメーターを受け取るプロシージャをオーバーロードする](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [プロシージャのオーバーロードに関する注意事項](./considerations-in-overloading-procedures.md)
- [オーバーロードの解決](./overload-resolution.md)
- [Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)
- [Visual Basic におけるジェネリック型](../../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
