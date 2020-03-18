---
title: '方法 : 値を返さないプロシージャを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- procedure calls [Visual Basic], returning values
- Visual Basic code, procedures
- procedures [Visual Basic], calling
ms.assetid: 259b49a3-a3c1-4254-ba8c-73cdc4127703
ms.openlocfilehash: 3a5de98c6edf795a11bd9f0465aa6919f09eebfa
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74340957"
---
# <a name="how-to-call-a-procedure-that-does-not-return-a-value-visual-basic"></a>方法: 値を返さないプロシージャを呼び出す (Visual Basic)
`Sub` プロシージャは、呼び出し元のコードに値を返しません。 これは、スタンドアロンの呼び出しステートメントを使用して明示的に呼び出します。 単に式の中で名前を使用して呼び出すことはできません。  
  
### <a name="to-call-a-sub-procedure"></a>Sub プロシージャを呼び出すには  
  
1. `Sub` プロシージャの名前を指定します。  
  
2. 引数リストを囲むには、プロシージャ名をかっこで囲みます。 引数がない場合は、必要に応じてかっこを省略できます。 ただし、かっこを使用すると、コードが読みやすくなります。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、`Sub` プロシージャが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
     次の例では、Visual Basic <xref:Microsoft.VisualBasic.Interaction.AppActivate%2A> 関数を呼び出して、アプリケーションウィンドウをアクティブにします。 <xref:Microsoft.VisualBasic.Interaction.AppActivate%2A> は、ウィンドウのタイトルを唯一の引数として受け取ります。 呼び出し元のコードに値は返されません。 メモ帳のプロセスが実行されていない場合、この例では <xref:System.ArgumentException>がスローされます。 `Shell` の手順では、アプリケーションが指定されたパスにあることを前提としています。  
  
     [!code-vb[VbVbalrCatRef#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCatRef/VB/Class1.vb#11)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Interaction.Shell%2A>
- <xref:System.ArgumentException>
- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)
- [方法 : プロシージャを作成する](./how-to-create-a-procedure.md)
- [方法: 値を返すプロシージャを呼び出す](./how-to-call-a-procedure-that-returns-a-value.md)
- [方法: Visual Basic でイベントハンドラーを呼び出す](./how-to-call-an-event-handler.md)
