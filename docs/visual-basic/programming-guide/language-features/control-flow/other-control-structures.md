---
title: その他の制御構造
ms.date: 07/20/2015
helpviewer_keywords:
- statements [Visual Basic], control flow
- control structures [Visual Basic]
ms.assetid: 24b811f7-98ba-40ec-8dd3-4d528cfa4574
ms.openlocfilehash: 758df361f421684655147ae288af3f350e53c4d7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348131"
---
# <a name="other-control-structures-visual-basic"></a>その他の制御構造 (Visual Basic)
Visual Basic には、リソースを破棄したり、オブジェクト参照を繰り返す回数を減らしたりするのに役立つ制御構造が用意されています。  
  
## <a name="usingend-using-construction"></a>Using...End Using の構築  
 `Using...End Using` の構築では、SQL 接続などのリソースを使用するステートメントブロックを確立します。 必要に応じて、`Using` ステートメントを使用してリソースを取得することもできます。 `Using` ブロックを終了すると Visual Basic、他のコードが使用できるようにリソースが自動的に破棄されます。 リソースはローカルおよび破棄可能である必要があります。 詳細については、「[Using ステートメント](../../../../visual-basic/language-reference/statements/using-statement.md)」を参照してください。  
  
## <a name="withend-with-construction"></a>With...End With の構築  
 `With...End With` の構築では、オブジェクト参照を一度指定した後、そのメンバーにアクセスする一連のステートメントを実行できます。 これにより、コードを簡略化し、パフォーマンスを向上させることができます。これは、Visual Basic にアクセスする各ステートメントの参照を再確立する必要がないためです。 詳細については、「[End With ステートメント](../../../../visual-basic/language-reference/statements/with-end-with-statement.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [制御フロー](../../../../visual-basic/programming-guide/language-features/control-flow/index.md)
- [条件判断構造](../../../../visual-basic/programming-guide/language-features/control-flow/decision-structures.md)
- [ループ構造](../../../../visual-basic/programming-guide/language-features/control-flow/loop-structures.md)
- [入れ子になった制御構造](../../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)
- [Using ステートメント](../../../../visual-basic/language-reference/statements/using-statement.md)
- [With...End With ステートメント](../../../../visual-basic/language-reference/statements/with-end-with-statement.md)
