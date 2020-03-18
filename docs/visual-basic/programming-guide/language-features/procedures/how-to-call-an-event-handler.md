---
title: '方法: イベントハンドラーを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- event handlers [Visual Basic], calling
- event handlers
- procedures [Visual Basic], event handlers
- procedures [Visual Basic], calling
ms.assetid: 72e18ef8-144e-40df-a1f4-066a57271e28
ms.openlocfilehash: 0c626a9ad92fe2cd0ea117a9abdd2965a09df2ea
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74340428"
---
# <a name="how-to-call-an-event-handler-in-visual-basic"></a>方法 : Visual Basic でイベント ハンドラーを呼び出す

*イベント*は、何らかのプログラムコンポーネントによって認識されるアクションまたは発生 (たとえば、マウスクリックやクレジットの制限を超えた場合) で、応答するコードを記述できます。 イベント*ハンドラー*は、イベントに応答するために記述するコードです。

 Visual Basic のイベントハンドラーは、`Sub` プロシージャです。 ただし、通常、他の `Sub` プロシージャと同じ方法で呼び出すことはありません。 代わりに、プロシージャをイベントのハンドラーとして指定します。 これは、 [Handles](../../../language-reference/statements/handles-clause.md)句と[WithEvents](../../../language-reference/modifiers/withevents.md)変数、または[AddHandler ステートメント](../../../language-reference/statements/addhandler-statement.md)を使用して行うことができます。 `Handles` 句の使用は、Visual Basic でイベントハンドラーを宣言する既定の方法です。 これは、統合開発環境 (IDE: integrated development environment) でプログラミングするときに、デザイナーによってイベントハンドラーが記述される方法です。 `AddHandler` ステートメントは、実行時にイベントを動的に発生させるのに適しています。

 イベントが発生すると、Visual Basic によってイベントハンドラープロシージャが自動的に呼び出されます。 イベントにアクセスできるコードでは、 [RaiseEvent ステートメント](../../../language-reference/statements/raiseevent-statement.md)を実行することによってイベントが発生する可能性があります。

 複数のイベントハンドラーを同じイベントに関連付けることができます。 場合によっては、イベントからハンドラーの関連付けを解除できます。 詳細については、「[イベント](../events/index.md)で定義されているインターフェイスのプライベート C++ 固有の実装です。

### <a name="to-call-an-event-handler-using-handles-and-withevents"></a>ハンドルと WithEvents を使用してイベントハンドラーを呼び出すには

1. イベントが [Event ステートメント](../../../language-reference/statements/event-statement.md)を使用して宣言されていることを確認します。

2. [WithEvents](../../../language-reference/modifiers/withevents.md)キーワードを使用して、モジュールまたはクラスレベルでオブジェクト変数を宣言します。 この変数の `As` 句には、イベントを発生させるクラスを指定する必要があります。

3. イベント処理 `Sub` プロシージャの宣言で、`WithEvents` 変数とイベント名を指定する[Handles](../../../language-reference/statements/handles-clause.md)句を追加します。

4. イベントが発生すると、Visual Basic は `Sub` プロシージャを自動的に呼び出します。 コードでは、`RaiseEvent` ステートメントを使用してイベントを発生させることができます。

     次の例では、イベントを定義し、イベントを発生させるクラスを参照する `WithEvents` 変数を定義します。 イベント処理 `Sub` プロシージャは、`Handles` 句を使用して、処理するクラスとイベントを指定します。

     [!code-vb[VbVbcnProcedures#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#4)]

### <a name="to-call-an-event-handler-using-addhandler"></a>AddHandler を使用してイベントハンドラーを呼び出すには

1. `Event` ステートメントを使用してイベントが宣言されていることを確認します。

2. [AddHandler ステートメント](../../../language-reference/statements/addhandler-statement.md)を実行して、イベント処理 `Sub` プロシージャをイベントに動的に接続します。

3. イベントが発生すると、Visual Basic は `Sub` プロシージャを自動的に呼び出します。 コードでは、`RaiseEvent` ステートメントを使用してイベントを発生させることができます。

     次の例では、フォームの <xref:System.Windows.Forms.Form.Closing> イベントを処理するための `Sub` プロシージャを定義します。 次に、 [AddHandler ステートメント](../../../language-reference/statements/addhandler-statement.md)を使用して、`catchClose` プロシージャを <xref:System.Windows.Forms.Form.Closing>のイベントハンドラーとして関連付けます。

     [!code-vb[VbVbcnProcedures#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#5)]

     [RemoveHandler ステートメント](../../../language-reference/statements/removehandler-statement.md)を実行すると、イベントからイベントハンドラーの関連付けを解除できます。

## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](index.md)
- [Sub プロシージャ](sub-procedures.md)
- [Sub ステートメント](../../../language-reference/statements/sub-statement.md)
- [AddressOf 演算子](../../../language-reference/operators/addressof-operator.md)
- [方法 : プロシージャを作成する](how-to-create-a-procedure.md)
- [方法 : 値を返さないプロシージャを呼び出す](how-to-call-a-procedure-that-does-not-return-a-value.md)
