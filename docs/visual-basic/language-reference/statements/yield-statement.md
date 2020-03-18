---
title: Yield ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Yield
helpviewer_keywords:
- iterators, Yield statement [Visual Basic]
- iterators [Visual Basic]
- Yield statement [Visual Basic]
ms.assetid: f33126c5-d7c4-43e2-8e36-4ae3f0703d97
ms.openlocfilehash: 72a8dafdc5aa834a644e1e70a309cfc0827b5fdf
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352728"
---
# <a name="yield-statement-visual-basic"></a>Yield ステートメント (Visual Basic)
コレクションの次の要素を `For Each...Next` ステートメントに送信します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Yield expression  
```  
  
## <a name="parameters"></a>パラメーター  
  
|用語|Definition|  
|---|---|  
|`expression`|必須。 `Yield` ステートメントを含む反復子関数または `Get` アクセサーの型に暗黙的に変換できる式。|  
  
## <a name="remarks"></a>コメント  
 `Yield` ステートメントは、コレクションの1つの要素を一度に返します。 `Yield` ステートメントは、コレクションに対してカスタムの反復処理を実行する反復子関数または `Get` アクセサーに含まれています。  
  
 For Each を使用して、iterator 関数を使用します。 [次のステートメント](../../../visual-basic/language-reference/statements/for-each-next-statement.md)または LINQ クエリ。 `For Each` ループの各反復処理では、iterator 関数が呼び出されます。 Iterator 関数で `Yield` ステートメントに到達すると、`expression` が返され、コード内の現在の場所が保持されます。 次回、反復子メソッドが呼び出されると、この位置から実行が再開されます。  
  
 `Yield` ステートメントの `expression` の型から反復子の戻り値の型への暗黙的な変換が存在する必要があります。  
  
 `Exit Function` または `Return` ステートメントを使用して、イテレーションを終了できます。  
  
 "Yield" は予約語ではなく、`Iterator` 関数または `Get` アクセサーで使用される場合にのみ特別な意味を持ちます。  
  
 反復子関数と `Get` アクセサーの詳細については、「[反復子](../../programming-guide/concepts/iterators.md)」を参照してください。  
  
## <a name="iterator-functions-and-get-accessors"></a>反復子関数と Get アクセサー  
 反復子関数または `Get` アクセサーの宣言は、次の要件を満たしている必要があります。  
  
- [反復子](../../../visual-basic/language-reference/modifiers/iterator.md)修飾子を含める必要があります。  
  
- 戻り値の型は、<xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator>、または <xref:System.Collections.Generic.IEnumerator%601> であることが必要です。  
  
- `ByRef` パラメーターを持つことはできません。  
  
 反復子関数は、イベント、インスタンスコンストラクター、静的コンストラクター、または静的デストラクターでは実行できません。  
  
 反復子関数は、匿名関数にすることができます。 詳細については、「[反復子](../../programming-guide/concepts/iterators.md)」をご覧ください。  
  
## <a name="exception-handling"></a>例外処理  
 `Yield` ステートメントは、[Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)の`Try` ブロック内にある場合があります。 `Yield` ステートメントを持つ `Try` ブロックは `Catch` ブロックと、`Finally` ブロックを持つことができます。  
  
 `Yield` ステートメントを `Catch` ブロックまたは `Finally` ブロックの内部に指定することはできません。  
  
 `For Each` 本体 (iterator 関数の外側) が例外をスローした場合、iterator 関数の `Catch` ブロックは実行されませんが、iterator 関数の `Finally` ブロックが実行されます。 Iterator 関数内の `Catch` ブロックは、iterator 関数内で発生した例外のみをキャッチします。  
  
## <a name="technical-implementation"></a>技術的な実装  
 次のコードは、iterator 関数から `IEnumerable (Of String)` を返し、`IEnumerable (Of String)`の要素を反復処理します。  
  
```vb  
Dim elements As IEnumerable(Of String) = MyIteratorFunction()  
    …  
For Each element As String In elements  
Next  
```  
  
 `MyIteratorFunction` の呼び出しでは、関数の本体は実行されません。 この呼び出しでは、`IEnumerable(Of String)` が `elements` 変数に返されます。  
  
 `For Each` ループの反復処理では、<xref:System.Collections.IEnumerator.MoveNext%2A> について `elements` メソッドが呼び出されます。 この呼び出しでは、次の `MyIteratorFunction` ステートメントに到達するまで、`Yield` の本体が実行されます。 `Yield` ステートメントは、ループ本体で使用する `element` 変数の値だけでなく、`IEnumerable (Of String)`である要素の <xref:System.Collections.Generic.IEnumerator%601.Current%2A> プロパティも決定する式を返します。  
  
 `For Each` ループの以降の各反復処理では、反復子本体の実行が中断した場所から続行し、`Yield` ステートメントに到達したときに再度停止します。 反復子関数の末尾、または `Return` または `Exit Function` ステートメントに到達すると、`For Each` ループは完了します。  
  
## <a name="example"></a>例  
 次の例では、For... の内部に `Yield` ステートメントがあります。 [次](../../../visual-basic/language-reference/statements/for-next-statement.md)のループ。 `Main` 内の[各](../../../visual-basic/language-reference/statements/for-each-next-statement.md)ステートメント本体の各反復処理では、`Power` iterator 関数の呼び出しが作成されます。 Iterator 関数を呼び出すごとに、`Yield` ステートメントの次の実行に進みます。これは、`For…Next` ループの次の反復処理で行われます。  
  
 Iterator メソッドの戻り値の型は <xref:System.Collections.Generic.IEnumerable%601>であり、反復子インターフェイス型です。 反復子メソッドが呼び出されると、数値の累乗を含む列挙可能なオブジェクトが返されます。  
  
 [!code-vb[VbVbalrStatements#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#98)]  
  
## <a name="example"></a>例  
 次の例は、反復子である `Get` アクセサーを示しています。 プロパティの宣言には、`Iterator` 修飾子が含まれています。  
  
 [!code-vb[VbVbalrStatements#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#99)]  
  
 その他の例については、「[反復子](../../programming-guide/concepts/iterators.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- [ステートメント](../../../visual-basic/language-reference/statements/index.md)
