---
title: '方法 : プロシージャにパラメーターを定義する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedure parameters [Visual Basic], defining data types for
- procedures [Visual Basic], parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters [Visual Basic], defining
ms.assetid: 7962808d-407e-4e84-984e-43e9857c53c9
ms.openlocfilehash: 411959a7be92ea49a59558b508e992bfba8eff95
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344882"
---
# <a name="how-to-define-a-parameter-for-a-procedure-visual-basic"></a>方法: プロシージャに対してパラメーターを定義する (Visual Basic)
*パラメーター*を使用すると、呼び出し元のコードは、呼び出し時にプロシージャに値を渡すことができます。 プロシージャの各パラメーターは、変数を宣言する場合と同じ方法で宣言し、名前とデータ型を指定します。 また、渡す機構と、パラメーターが省略可能かどうかも指定します。  
  
 詳細については、「[プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)」を参照してください。  
  
### <a name="to-define-a-procedure-parameter"></a>プロシージャパラメーターを定義するには  
  
1. プロシージャの宣言で、プロシージャのパラメーターリストにパラメーター名を追加し、他のパラメーターとコンマで区切ります。  
  
2. パラメーターのデータ型を決定します。  
  
3. パラメーター名の後に `As` 句を入力して、データ型を指定します。  
  
4. パラメーターに渡すメカニズムを決定します。 通常、プロシージャが呼び出し元のコードで値を変更できないようにする場合は、パラメーターを値で渡します。  
  
5. パラメーター名の前に、 [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)または[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)を渡して、渡すメカニズムを指定します。 詳細については、「[引数を値で渡す方法と参照渡しの違い](./differences-between-passing-an-argument-by-value-and-by-reference.md)」を参照してください。  
  
6. パラメーターが省略可能な場合は、渡される機構の前に[オプション](../../../../visual-basic/language-reference/modifiers/optional.md)を指定し、等号 (`=`) と既定値を指定してパラメーターのデータ型に従います。  
  
     次の例では、3つのパラメーターを持つ `Sub` プロシージャのアウトラインを定義します。 最初の2つは必須で、3番目のオプションは省略可能です。 パラメーターの宣言は、パラメーターリスト内でコンマで区切られます。  
  
     [!code-vb[VbVbcnProcedures#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#33)]  
  
     最初のパラメーターは `customer` オブジェクトを受け入れます。引数は[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)で渡されるため、`updateCustomer` は `c` に渡された変数を直接更新できます。 プロシージャは[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)で渡されるため、最後の2つの引数の値を変更することはできません。  
  
     呼び出し元のコードが `level` パラメーターの値を指定していない場合は、Visual Basic 既定値の0に設定されます。  
  
     型チェックスイッチ ([Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)) が `Off`場合、パラメーターを定義するときに、`As` 句は省略可能です。 ただし、1つのパラメーターで `As` 句を使用する場合は、すべてのパラメーターでそれを使用する必要があります。 型チェックスイッチが `On`場合は、すべてのパラメーター定義に対して `As` 句が必要です。  
  
     すべてのプログラミング要素のデータ型の指定は、*厳密な*型指定と呼ばれます。 `Option Strict On`を設定すると、Visual Basic は厳密な型指定を適用します。 これは、次の理由から強くお勧めします。  
  
    - これにより、変数とパラメーターの IntelliSense サポートが有効になります。 これにより、コードを入力するときに、プロパティとその他のメンバーを表示できます。  
  
    - これにより、コンパイラは型チェックを実行できます。 これは、オーバーフローなどのエラーによって実行時に失敗する可能性があるステートメントをキャッチするのに役立ちます。 また、サポートされていないオブジェクトのメソッドの呼び出しもキャッチします。  
  
    - これにより、コードの実行時間が短縮されます。 その理由の1つは、プログラミング要素のデータ型を指定しない場合、Visual Basic コンパイラによって `Object` 型に割り当てられることです。 コンパイルされたコードは、`Object` とその他のデータ型との間で変換を行う必要があります。これにより、パフォーマンスが低下します。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Function プロシージャ](./function-procedures.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [再帰プロシージャ](./recursive-procedures.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [クラスとオブジェクト](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [オブジェクト指向プログラミング (Visual Basic)](../../concepts/object-oriented-programming.md)
