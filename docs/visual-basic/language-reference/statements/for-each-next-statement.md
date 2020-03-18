---
title: For Each...Next ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.ForEach
- vb.ForEachNext
- vb.Each
- ForEachNext
helpviewer_keywords:
- infinite loops
- Next statement [Visual Basic], For Each...Next
- endless loops
- loop structures [Visual Basic], For Each...Next
- loops, endless
- Each keyword [Visual Basic]
- instructions, repeating
- For Each statement [Visual Basic]
- collections, instruction repetition
- loops, infinite
- For Each...Next statements
- For keyword [Visual Basic], For Each...Next statements
- Exit statement [Visual Basic], For Each...Next statements
- iteration
ms.assetid: ebce3120-95c3-42b1-b70b-fa7da40c75e2
ms.openlocfilehash: 572f1efc0148bd8df4f13fa5651e74249caa45a7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351205"
---
# <a name="for-eachnext-statement-visual-basic"></a>For Each...Next ステートメント (Visual Basic)

コレクション内の各要素に対してステートメントのグループを繰り返します。

## <a name="syntax"></a>構文

```vb
For Each element [ As datatype ] In group
    [ statements ]
    [ Continue For ]
    [ statements ]
    [ Exit For ]
    [ statements ]
Next [ element ]
```

## <a name="parts"></a>指定項目

|用語|Definition|
|---|---|
|`element`|`For Each` ステートメントで必要です。 `Next` ステートメントでは省略可能です。 変動. コレクションの要素を反復処理するために使用されます。|
|`datatype`|[`Option Infer`](option-infer-statement.md)が on (既定値) または `element` が既に宣言されている場合は省略可能です。`Option Infer` がオフで `element` がまだ宣言されていない場合は必須です。 `element`のデータ型。|
|`group`|必須。 コレクション型またはオブジェクトである型を持つ変数。 `statements` を繰り返すコレクションを参照します。|
|`statements`|省略可。 `group`内の各項目に対して実行される `For Each` と `Next` 間の1つ以上のステートメント。|
|`Continue For`|省略可。 `For Each` ループの開始位置に制御を転送します。|
|`Exit For`|省略可。 `For Each` ループから制御を転送します。|
|`Next`|必須。 `For Each` ループの定義を終了します。|

## <a name="simple-example"></a>簡単な例

コレクションまたは配列の各要素に対して一連のステートメントを繰り返す場合は、`For Each`...`Next` ループを使用します。

> [!TIP]
> [For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)は、ループの各反復処理を制御変数に関連付けて、その変数の初期値と最終値を決定できる場合に適しています。 ただし、コレクションを処理する場合、初期値と最終値の概念は意味がなく、コレクションに含まれる要素の数がわかるとは限りません。 このような場合は、`For Each`...`Next` ループの方が適していることがよくあります。

次の例では、`For Each`...`Next` ステートメントは、リストコレクションのすべての要素を反復処理します。

[!code-vb[VbVbalrStatements#121](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#121)]

その他の例については、「[コレクション](../../../standard/collections/index.md)と[配列](../../../visual-basic/programming-guide/language-features/arrays/index.md)」を参照してください。

## <a name="nested-loops"></a>Nested Loops

ループを入れ子にするには、別のループ内にループを挿入し `For Each` ます。

次の例では、入れ子になった `For Each`...`Next` を示します。 構成.

[!code-vb[VbVbalrStatements#122](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#122)]

ループを入れ子にする場合、各ループには一意の `element` 変数が必要です。

また、さまざまな種類の制御構造を相互に入れ子にすることもできます。 詳細については、「[入れ子になった制御構造](../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)」を参照してください。

## <a name="exit-for-and-continue-for"></a>Exit For と Continue For

[Exit For](../../../visual-basic/language-reference/statements/exit-statement.md)ステートメントを実行すると、`For`...`Next` が終了します ループし、`Next` ステートメントの後のステートメントに制御を転送します。

`Continue For` ステートメントは、ループの次の反復処理に制御を直ちに転送します。 詳細については、「[Continue ステートメント](../../../visual-basic/language-reference/statements/continue-statement.md)」を参照してください。

次の例では、`Continue For` ステートメントと `Exit For` ステートメントを使用する方法を示します。

[!code-vb[VbVbalrStatements#123](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#123)]

`For Each` ループには、任意の数の `Exit For` ステートメントを含めることができます。 入れ子になった `For Each` ループ内で使用すると、`Exit For` によって最も内側のループが終了し、次の上位レベルの入れ子に制御が移ります。

`Exit For` は、たとえば、`If`...`Then`...`Else` 構造など、何らかの条件を評価した後に使用されることがよくあります。 次の条件に `Exit For` を使用することもできます。

- 反復処理を続行することは不要または不可能です。 これは、エラー値または終了要求が原因である可能性があります。

- 例外は、`Try`...`Catch`...`Finally`でキャッチされます。`Finally` ブロックの末尾に `Exit For` を使用することもできます。

- 無限ループがあります。これは、大規模または無限の回数実行されるループです。 このような条件を検出した場合は、`Exit For` を使用してループをエスケープできます。 詳細については、「[Do...Loop ステートメント](../../../visual-basic/language-reference/statements/do-loop-statement.md)」を参照してください。

## <a name="iterators"></a>反復子

*反復子*を使用して、コレクションに対してカスタムの反復処理を実行します。 反復子は、関数または `Get` アクセサーにすることができます。 `Yield` ステートメントを使用して、コレクションの各要素を一度に1つ返します。

`For Each...Next` ステートメントを使用して、反復子を呼び出します。 `For Each` ループの各イテレーションは、反復子を呼び出します。 反復子で `Yield` ステートメントに到達すると、`Yield` ステートメント内の式が返され、コード内の現在の位置が保持されます。 次回、反復子が呼び出されると、この位置から実行が再開されます。

次の例では、iterator 関数を使用します。 Iterator 関数には、For... の内部にある `Yield` ステートメントがあります。 [次](../../../visual-basic/language-reference/statements/for-next-statement.md)のループ。 `ListEvenNumbers` メソッドでは、`For Each` ステートメント本体の各反復処理によって、次の `Yield` ステートメントに進む反復子関数の呼び出しが作成されます。

[!code-vb[VbVbalrStatements#127](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#127)]

詳細については、「[反復子](../../programming-guide/concepts/iterators.md)、 [Yield ステートメント](../../../visual-basic/language-reference/statements/yield-statement.md)、および[反復子](../../../visual-basic/language-reference/modifiers/iterator.md)」を参照してください。

## <a name="technical-implementation"></a>技術的な実装

`For Each`...`Next` ステートメントを実行すると、ループが開始される前に、Visual Basic によってコレクションが1回だけ評価されます。 ステートメントブロックで `element` または `group`が変更された場合、これらの変更はループの繰り返しに影響しません。

コレクション内のすべての要素が `element`に連続して割り当てられると、`For Each` ループが停止し、`Next` ステートメントに続くステートメントに制御が移ります。

[Option Infer ステートメント](option-infer-statement.md)が on (既定の設定) の場合、Visual Basic コンパイラは `element`のデータ型を推測できます。 Off で、`element` がループの外側で宣言されていない場合は、`For Each` ステートメントで宣言する必要があります。 `element` のデータ型を明示的に宣言するには、`As` 句を使用します。 要素のデータ型が `For Each`...`Next` コンストラクトの外側で定義されている場合を除き、そのスコープはループの本体です。 ループ内で `element` 宣言することはできません。

必要に応じて、`Next` ステートメントで `element` を指定することもできます。 これにより、特に `For Each` ループが入れ子になっている場合に、プログラムの読みやすさが向上します。 対応する `For Each` ステートメントに表示されるものと同じ変数を指定する必要があります。

ループ内の `element` の値を変更しないようにすることもできます。 これにより、コードの読み取りとデバッグが困難になる可能性があります。 `group` の値を変更しても、ループが最初に入力されたときに決定されたコレクションまたはその要素には影響しません。

ループを入れ子にしているときに、外側の入れ子レベルの `Next` ステートメントが内部レベルの `Next` より前に検出された場合、コンパイラはエラーを通知します。 ただし、コンパイラは、すべての `Next` ステートメントで `element` を指定した場合にのみ、この重複エラーを検出できます。

コードが特定の順序でコレクションを走査することに依存している場合、コレクションが公開する列挙子オブジェクトの特性がわかっていない限り、`For Each`...`Next` ループは最適な選択ではありません。 走査の順序は Visual Basic によって決定されるのではなく、列挙子オブジェクトの <xref:System.Collections.IEnumerator.MoveNext%2A> メソッドによって決定されます。 したがって、コレクションのどの要素が `element`に最初に返されるか、または指定された要素の後に返される次の要素であるかを予測できない場合があります。 `For`...`Next` や `Do`...`Loop`などの別のループ構造を使用して、より信頼性の高い結果を得ることができます。

ランタイムは、`group` 内の要素を `element`に変換できる必要があります。 [`Option Strict`] ステートメントでは、拡大変換と縮小変換の両方を許可するかどうかを制御します (`Option Strict` はオフ、既定値)。または、拡大変換のみが許可されるかどうかを制御します (`Option Strict` がオンの場合)。 詳細については、「[縮小変換](#narrowing-conversions)」を参照してください。

`group` のデータ型は、コレクションまたは列挙可能な配列を参照する参照型である必要があります。 ほとんどの場合、`group` は、`System.Collections` 名前空間の <xref:System.Collections.IEnumerable> インターフェイスまたは `System.Collections.Generic` 名前空間の <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装するオブジェクトを参照します。 `System.Collections.IEnumerable` は、コレクションの列挙子オブジェクトを返す <xref:System.Collections.IEnumerable.GetEnumerator%2A> メソッドを定義します。 列挙子オブジェクトは、`System.Collections` 名前空間の `System.Collections.IEnumerator` インターフェイスを実装し、<xref:System.Collections.IEnumerator.Current%2A> プロパティと <xref:System.Collections.IEnumerator.Reset%2A> および <xref:System.Collections.IEnumerator.MoveNext%2A> メソッドを公開します。 Visual Basic は、これらを使用してコレクションを走査します。

### <a name="narrowing-conversions"></a>縮小変換

`Option Strict` が `On`に設定されている場合、通常、縮小変換ではコンパイラエラーが発生します。 ただし、`For Each` ステートメントでは、`group` 内の要素から `element` への変換は実行時に評価されて実行され、縮小変換によって発生するコンパイラエラーは抑制されます。

次の例では、`n` の初期値としての `m` の割り当ては `Option Strict` がオンのときにはコンパイルされません。これは、`Long` から `Integer` への変換は縮小変換であるためです。 ただし、`For Each` ステートメントでは、`number` への代入によって `Long` から `Integer`への変換が必要な場合でも、コンパイラエラーは報告されません。 多数の数値が含まれている `For Each` ステートメントでは、<xref:Microsoft.VisualBasic.CompilerServices.Conversions.ToInteger%2A> が大きい数値に適用されると、実行時エラーが発生します。

[!code-vb[VbVbalrStatements#89](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class3.vb#89)]

### <a name="ienumerator-calls"></a>IEnumerator 呼び出し

`For Each`...`Next` ループの実行が開始されると、`group` が有効なコレクションオブジェクトを参照していることが Visual Basic 検証されます。 そうでない場合は、例外がスローされます。 それ以外の場合は、<xref:System.Collections.IEnumerator.MoveNext%2A> メソッドと列挙子オブジェクトの <xref:System.Collections.IEnumerator.Current%2A> プロパティを呼び出して、最初の要素を返します。 `MoveNext` が次の要素がないことを示している場合 (つまり、コレクションが空の場合)、`For Each` ループは停止し、`Next` ステートメントに続くステートメントに制御が移ります。 それ以外の場合、Visual Basic は最初の要素に `element` を設定し、ステートメントブロックを実行します。

Visual Basic が `Next` ステートメントを検出するたびに、`For Each` ステートメントに戻ります。 この場合も、`MoveNext` と `Current` を呼び出して次の要素を返します。もう一度、ブロックを実行するか、結果に応じてループを停止します。 このプロセスは、次の要素が存在しないこと、または `Exit For` ステートメントが発生したことが `MoveNext` によって示されるまで続行されます。

**コレクションの変更。** <xref:System.Collections.IEnumerable.GetEnumerator%2A> によって返される列挙子オブジェクトは、要素の追加、削除、置換、または並べ替えを行うことによって、コレクションを変更することはできません。 `For Each`...`Next` ループを開始した後にコレクションを変更した場合、列挙子オブジェクトは無効になり、次に要素にアクセスしようとすると <xref:System.InvalidOperationException> 例外が発生します。

ただし、この変更のブロックは Visual Basic によって決定されるのではなく、<xref:System.Collections.IEnumerable> インターフェイスの実装によって決まります。 反復処理中に変更を許可する方法で `IEnumerable` を実装することができます。 このような動的な変更を検討している場合は、使用しているコレクションの `IEnumerable` 実装の特性を理解していることを確認してください。

**コレクション要素の変更。** 列挙子オブジェクトの <xref:System.Collections.IEnumerator.Current%2A> プロパティは[ReadOnly](../../../visual-basic/language-reference/modifiers/readonly.md)であり、各コレクション要素のローカルコピーを返します。 つまり、`For Each`...`Next` ループで要素自体を変更することはできません。 行った変更は、`Current` からのローカルコピーにのみ適用され、基になるコレクションには反映されません。 ただし、要素が参照型の場合は、その要素が指すインスタンスのメンバーを変更できます。 次の例では、各 `thisControl` 要素の `BackColor` メンバーを変更します。 ただし、`thisControl` 自体を変更することはできません。

```vb
Sub LightBlueBackground(thisForm As System.Windows.Forms.Form)
    For Each thisControl In thisForm.Controls
        thisControl.BackColor = System.Drawing.Color.LightBlue
    Next thisControl
End Sub
```

前の例では、各 `thisControl` 要素の `BackColor` メンバーを変更できますが、`thisControl` 自体を変更することはできません。

**配列の反復処理。** <xref:System.Array> クラスは <xref:System.Collections.IEnumerable> インターフェイスを実装するため、すべての配列は <xref:System.Array.GetEnumerator%2A> メソッドを公開します。 これは、`For Each`...`Next` ループで配列を反復処理できることを意味します。 ただし、読み取ることができるのは配列要素だけです。 変更することはできません。

## <a name="example"></a>例

次の例では、C:\ 内のすべてのフォルダーを一覧表示します。<xref:System.IO.DirectoryInfo> クラスを使用したディレクトリ。

[!code-vb[VbVbalrStatements#124](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#124)]

## <a name="example"></a>例

次の例では、コレクションを並べ替えるための手順を示しています。 この例では、<xref:System.Collections.Generic.List%601>に格納されている `Car` クラスのインスタンスを並べ替えます。 `Car` クラスは、<xref:System.IComparable%601> のメソッドの実装を必要とする <xref:System.IComparable%601.CompareTo%2A> インターフェイスを実装します。

<xref:System.IComparable%601.CompareTo%2A> メソッドを呼び出すたびに、並べ替えに使用される1つの比較が行われます。 `CompareTo` メソッドのユーザーが作成したコードは、現在のオブジェクトと別のオブジェクトとの各比較の値を戻します。 現在のオブジェクトが別のオブジェクトよりも小さい場合はゼロ未満の値を、大きい場合はゼロ以上の値を、等しい場合はゼロを戻します。 これによって、より大きい、より小さい、等しい、の条件をコードに定義することができます。

`ListCars` のメソッドでは、`cars.Sort()` ステートメントがリストを並べ替えます。 <xref:System.Collections.Generic.List%601.Sort%2A> の <xref:System.Collections.Generic.List%601> メソッドへの呼び出しによって、`CompareTo` メソッドは `Car` 内の `List` オブジェクトに自動的に呼び出されます。

[!code-vb[VbVbalrStatements#125](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#125)]

## <a name="see-also"></a>参照

- [コレクション](../../../standard/collections/index.md)
- [For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)
- [ループ構造](../../../visual-basic/programming-guide/language-features/control-flow/loop-structures.md)
- [While...End While ステートメント](../../../visual-basic/language-reference/statements/while-end-while-statement.md)
- [Do...Loop ステートメント](../../../visual-basic/language-reference/statements/do-loop-statement.md)
- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [オブジェクト初期化子 : 名前付きの型と匿名型](../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [コレクション初期化子](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [配列](../../../visual-basic/programming-guide/language-features/arrays/index.md)
