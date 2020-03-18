---
title: Visual Basic におけるプロシージャ
ms.date: 04/28/2017
helpviewer_keywords:
- procedures [Visual Basic], structured code
- Visual Basic code, procedures
- procedures [Visual Basic], types of
- structured code [Visual Basic], procedures
- procedures
ms.assetid: 9effbcf0-80a0-4d1a-98f4-2c6920592766
ms.openlocfilehash: b959f4b6986bc325c97c7cbe9aeee0341832f6cc
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345994"
---
# <a name="procedures-in-visual-basic"></a>Visual Basic におけるプロシージャ
*プロシージャ*は、宣言ステートメント (`Function`、`Sub`、`Operator`、`Get`、`Set`)、および一致する `End` 宣言で囲まれた Visual Basic ステートメントのブロックです。 Visual Basic 内のすべての実行可能なステートメントは、何らかのプロシージャ内になければなりません。  
  
## <a name="calling-a-procedure"></a>プロシージャの呼び出し  
 コード内の他の場所からプロシージャを呼び出します。 これは、*プロシージャ コール*と呼ばれています。 プロシージャの実行が終了すると、それを呼び出したコード (*呼び出しコード*と呼ばれます) に制御が戻ります。 呼び出しコードは、名前でプロシージャを指定して、これに制御を転送するステートメント、またはステートメント内の式です。  
  
## <a name="returning-from-a-procedure"></a>プロシージャからの復帰  
 プロシージャは、実行が終了すると、呼び出しコードに制御を戻します。 これを行うには、[Return ステートメント](../../../../visual-basic/language-reference/statements/return-statement.md)、プロシージャに適した [Exit ステートメント](../../../../visual-basic/language-reference/statements/exit-statement.md)、またはプロシージャの [End \<キーワード> ステートメント](../../../../visual-basic/language-reference/statements/end-keyword-statement.md)を使用することができます。 これで、プロシージャ コールの次の時点で、制御が呼び出しコードに渡されます。  
  
- `Return` ステートメントでは、ただちに呼び出しコードに制御が戻ります。 `Return` ステートメントより後のステートメントは実行されません。 同じプロシージャ内に複数の `Return` ステートメントを含めることができます。  
  
- `Exit Sub` または `Exit Function` ステートメントでは、ただちに呼び出しコードに制御が戻ります。 `Exit` ステートメントより後のステートメントは実行されません。 同じプロシージャ内に複数の `Exit` ステートメントを含めることができ、さらに同じプロシージャ内に `Return` ステートメントと `Exit` ステートメントを混在させることができます。  
  
- プロシージャに `Return` または `Exit` のステートメントが含まれていない場合、プロシージャ本体の最後のステートメントの後の `End Sub` または `End Function`、`End Get` または `End Set` で終了します。 `End` ステートメントはただちに呼び出しコードに制御を戻します。 `End` ステートメントは、プロシージャ内に 1 つだけ含めることができます。  
  
## <a name="parameters-and-arguments"></a>パラメーターと引数  
 プロシージャは、ほとんどの場合、呼び出すたびにデータごとに動作する必要があります。 この情報は、プロシージャ コールの一部としてプロシージャに渡すことができます。 プロシージャは、*パラメーター*を 0 個、またはそれ以上でも定義することができ、それぞれが渡す必要がある値を表しています。 プロシージャ定義の各パラメーターに相当するのが、プロシージャ コールの*引数*です。 引数は、指定したプロシージャ コールの対応するパラメーターに渡される値を表しています。  
  
## <a name="types-of-procedures"></a>プロシージャの種類  
 Visual Basic は、次のようないくつかの種類のプロシージャを使用します。  
  
- [Sub プロシージャ](./sub-procedures.md)はアクションを実行しますが、呼び出しコードに値を返しません。  
  
- イベント処理プロシージャは、ユーザーの操作またはプログラムの起動によって発生したイベントに応答して実行される `Sub` プロシージャです。  
  
- [Function プロシージャ](./function-procedures.md)は、呼び出しコードに値を返します。 返す前に他の操作を実行することができます。

    C# で書かれた一部の関数は、*参照戻り値*を返します。 関数の呼び出し元は、戻り値を変更することができ、この変更は呼び出されたオブジェクトの状態に反映されます。 Visual Basic は参照によって値を返すことはできませんが、Visual Basic 2017 から、Visual Basic のコードで参照戻り値を使用できるようになりました。 詳細については、[参照戻り値](ref-return-values.md)に関するページを参照してください。
  
- [Property プロシージャ](./property-procedures.md)は、オブジェクトまたはモジュールのプロパティの値を返し、割り当てます。  
  
- [演算子プロシージャ](./operator-procedures.md)は、オペランドの一方または両方が新しく定義されたクラスまたは構造体である場合に、標準の演算子の動作を定義します。  
  
- [Visual Basic におけるジェネリック プロシージャ](../../../../visual-basic/programming-guide/language-features/data-types/generic-procedures.md)は、標準のパラメーターだけでなく 1 つまたは複数の*型パラメーター*も定義するため、呼び出しコードが呼び出しのたびに特定のデータ型を渡すことができます。  
  
## <a name="procedures-and-structured-code"></a>プロシージャと構造化されたコード  
 アプリケーションの実行可能コードのすべての行が、`Main`、 `calculate`、`Button1_Click` などの何らかのプロシージャの内部にある必要があります。 大きなプロシージャを小さなプロシージャに分割すると、アプリケーションが読みやすくなります。  
  
 プロシージャは、頻繁に使用する計算、テキストやコントロールの操作、データベースの操作など、繰り返される、または共有されるタスクを実行する場合に便利です。 プロシージャはコード内のさまざまな場所から呼び出すことができるため、プロシージャをアプリケーションの文書パーツとして使用することができます。  
  
 プロシージャでコードを構成すると、次のような利点があります。  
  
- プロシージャを使用して、プログラムを個々の論理単位に分割できます。 プロシージャを使用せずにプログラム全体をデバッグするよりも、個別の単位の方が簡単にデバッグできます。  
  
- 1 つのプログラムで使用するために開発したプロシージャを、他のプログラムでも使用できます。多くの場合、プログラムをほとんどまたはまったく変更せずに使用できます。 これにより、コードの重複を避けることができます。  
  
## <a name="see-also"></a>参照

- [方法 : プロシージャを作成する](./how-to-create-a-procedure.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Function プロシージャ](./function-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [再帰プロシージャ](./recursive-procedures.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [Visual Basic におけるジェネリック プロシージャ](../../../../visual-basic/programming-guide/language-features/data-types/generic-procedures.md)
- [クラスとオブジェクト](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
