---
title: ステートメント
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], declaring
- colons (:) [Visual Basic]
- constants [Visual Basic], defining
- underlines
- constants [Visual Basic], statements
- blue underline [Visual Basic]
- procedures [Visual Basic], statements
- variables [Visual Basic], assigning
- line breaks [Visual Basic], in code
- executable statements [Visual Basic]
- variables [Visual Basic], defining
- statements [Visual Basic], about statements
ms.assetid: fcfdee1a-82b7-4846-98f7-9ca3f5160089
ms.openlocfilehash: f63f0f0212913f95baab2a8a43c4b7f25a859cd9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352502"
---
# <a name="statements-in-visual-basic"></a>Visual Basic におけるステートメント

Visual Basic のステートメントは完全な命令です。 キーワード、演算子、変数、定数、および式を含めることができます。 各ステートメントは、次のいずれかのカテゴリに属します。

- 変数、定数、またはプロシージャの名前を**宣言する宣言ステートメント**。データ型を指定することもできます。

- **実行可能なステートメント**。アクションを開始します。 これらのステートメントは、メソッドまたは関数を呼び出すことができ、コードのブロックをループまたは分岐できます。 実行可能なステートメントには、変数または定数に値または式を割り当てる**代入ステートメント**が含まれます。

このトピックでは、各カテゴリについて説明します。 また、このトピックでは、複数のステートメントを1行に結合する方法と、ステートメントを複数の行にわたって継続する方法についても説明します。

## <a name="declaration-statements"></a>宣言ステートメント

宣言ステートメントを使用して、プロシージャ、変数、プロパティ、配列、および定数に名前を指定し、定義します。 プログラミング要素を宣言するときに、そのデータ型、アクセスレベル、およびスコープを定義することもできます。 詳細については、「[宣言された要素の特性](./declared-elements/declared-element-characteristics.md)」を参照してください。

次の例には、3つの宣言が含まれています。

[!code-vb[VbVbalrStatements#80](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#80)]

最初の宣言は、`Sub` ステートメントです。 一致する `End Sub` ステートメントと共に、`applyFormat`という名前のプロシージャを宣言します。 また、`applyFormat` が `Public`であることを指定します。これは、それを参照できるすべてのコードがそれを呼び出すことができることを意味します。

2番目の宣言は `Const` ステートメントであり、`Integer` データ型と33の値を指定して `limit`定数を宣言します。

3番目の宣言は、変数 `thisWidget`を宣言する `Dim` ステートメントです。 データ型は、特定のオブジェクト、つまり `Widget` クラスから作成されたオブジェクトです。 任意の基本データ型または使用しているアプリケーションで公開されている任意のオブジェクト型の変数を宣言できます。

### <a name="initial-values"></a>初期値

宣言ステートメントを含むコードを実行すると、Visual Basic は、宣言された要素に必要なメモリを予約します。 要素が値を保持している場合は、Visual Basic そのデータ型の既定値に初期化します。 詳細については、「[Dim ステートメント](../../language-reference/statements/dim-statement.md)」の「動作」を参照してください。

次の例に示すように、変数に初期値を宣言の一部として割り当てることができます。

[!code-vb[VbVbalrStatements#81](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#81)]

変数がオブジェクト変数の場合は、次の例に示すように、 [New 演算子](../../../visual-basic/language-reference/operators/new-operator.md)キーワードを使用して宣言するときに、そのクラスのインスタンスを明示的に作成できます。

[!code-vb[VbVbalrStatements#82](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#82)]

宣言ステートメントで指定する初期値は、実行が宣言ステートメントに到達するまで変数に割り当てられないことに注意してください。 この時間が経過するまで、変数にはそのデータ型の既定値が含まれます。

## <a name="executable-statements"></a>実行可能なステートメント

実行可能なステートメントがアクションを実行します。 プロシージャを呼び出したり、コード内の別の場所に分岐したり、複数のステートメントをループ処理したり、式を評価したりすることができます。 代入ステートメントは、実行可能なステートメントの特殊なケースです。

次の例では、`If...Then...Else` コントロール構造体を使用して、変数の値に基づいてさまざまなコードブロックを実行します。 各コードブロック内では、`For...Next` ループが指定された回数実行されます。

[!code-vb[VbVbalrStatements#83](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#83)]

前の例の `If` ステートメントでは、パラメーター `clockwise`の値を確認します。 値が `True`場合、`aWidget`の `spinClockwise` メソッドを呼び出します。 値が `False`場合、`aWidget`の `spinCounterClockwise` メソッドを呼び出します。 `If...Then...Else` コントロールの構造体は、`End If`で終了します。

各ブロック内の `For...Next` ループは、`revolutions` パラメーターの値と等しい回数、適切なメソッドを呼び出します。

## <a name="assignment-statements"></a>代入ステートメント

代入ステートメントは、代入演算子 (`=`) の右側にある値を取得し、次の例のように左側の要素に格納することで構成される代入演算を実行します。

[!code-vb[VbVbalrStatements#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#73)]

前の例では、代入ステートメントによって、リテラル値42が変数 `v`に格納されています。

### <a name="eligible-programming-elements"></a>対象となるプログラミング要素

代入演算子の左側のプログラミング要素は、値を受け入れて格納できる必要があります。 これは、[読み取り専用](../../../visual-basic/language-reference/modifiers/readonly.md)ではない変数またはプロパティであるか、配列要素である必要があることを意味します。 代入ステートメントのコンテキストでは、このような要素は "左辺値" として*左辺*値と呼ばれることもあります。

代入演算子の右側の値は、式によって生成されます。式は、リテラル、定数、変数、プロパティ、配列要素、他の式、または関数呼び出しの任意の組み合わせで構成されます。 これを次の例に示します。

[!code-vb[VbVbalrStatements#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#74)]

前の例では、変数 `y` に保持されている値を変数 `z`に保持されている値に追加した後、関数 `findResult`への呼び出しによって返された値を追加します。 この式の合計値は、変数 `x`に格納されます。

### <a name="data-types-in-assignment-statements"></a>代入ステートメントのデータ型

次の例に示すように、代入演算子は数値に加えて `String` 値も割り当てることができます。

[!code-vb[VbVbalrStatements#75](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#75)]

次の例に示すように、`Boolean` リテラルまたは `Boolean` 式を使用して `Boolean` 値を割り当てることもできます。

[!code-vb[VbVbalrStatements#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#76)]

同様に、`Char`、`Date`、または `Object` データ型のプログラミング要素に適切な値を割り当てることができます。 また、インスタンスの作成元のクラスとして宣言された要素にオブジェクトインスタンスを割り当てることもできます。

### <a name="compound-assignment-statements"></a>複合代入ステートメント

*複合代入ステートメント*は、最初に式に対して操作を実行してから、プログラミング要素に代入します。 次の例では、演算子の左辺の変数の値を右側の式の値でインクリメントする、`+=`のいずれかの演算子を示します。

[!code-vb[VbVbalrStatements#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#77)]

前の例では、`n`の値に1を加算し、その新しい値を `n`に格納しています。 これは、次のステートメントの省略形に相当します。

[!code-vb[VbVbalrStatements#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#78)]

この型の演算子を使用すると、さまざまな複合代入演算を実行できます。 これらの演算子の一覧と詳細については、「[代入演算子](../../../visual-basic/language-reference/operators/assignment-operators.md)」を参照してください。

連結代入演算子 (`&=`) は、次の例に示すように、既存の文字列の末尾に文字列を追加する場合に便利です。

[!code-vb[VbVbalrStatements#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#79)]

### <a name="type-conversions-in-assignment-statements"></a>代入ステートメントでの型変換

変数、プロパティ、または配列要素に割り当てる値は、そのターゲット要素に適したデータ型である必要があります。 一般に、destination 要素と同じデータ型の値を生成することをお勧めします。 ただし、割り当て時に他の型に変換できる型もあります。

データ型間の変換の詳細については、「[Visual Basic での型変換](./data-types/type-conversions.md)」を参照してください。 簡単に言えば、Visual Basic は、指定された型の値を、拡大変換先の他の型に自動的に変換します。 *拡大変換*は、常に実行時に成功し、データを失うことのないの1つです。 たとえば、Visual Basic は、`Integer` が `Double`に拡大変換されるため、必要に応じて `Integer` 値を `Double` に変換します。 詳細については、「[拡大変換と縮小変換](./data-types/widening-and-narrowing-conversions.md)」を参照してください。

*縮小変換*(拡大されていないもの) は、実行時またはデータ損失のリスクを発生させます。 型変換関数を使用して縮小変換を明示的に実行することも、`Option Strict Off`を設定することによって暗黙的にすべての変換を実行するようにコンパイラに指示することもできます。 詳細については、「[暗黙的な変換と明示的な変換](./data-types/implicit-and-explicit-conversions.md)」を参照してください。

## <a name="putting-multiple-statements-on-one-line"></a>1行に複数のステートメントを配置する

1つの行に複数のステートメントを記述して、コロン (`:`) 文字で区切ることができます。 これを次の例に示します。

[!code-vb[VbVbalrStatements#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#70)]

この形式の構文を使用すると、コードの読み取りと保守が困難になる場合があります。 したがって、1つのステートメントを1行に収めることをお勧めします。

## <a name="continuing-a-statement-over-multiple-lines"></a>複数行にわたってステートメントを続行する

ステートメントは通常1行に配置されますが、長すぎる場合は、行連結シーケンスを使用して次の行に進むことができます。これは、スペースの後にアンダースコア文字 (`_`) が続き、その後に復帰が続くもので構成されます。 次の例では、`MsgBox` の実行可能なステートメントは2つの行にわたって続きます。

[!code-vb[VbVbalrStatements#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#71)]

### <a name="implicit-line-continuation"></a>暗黙の行の連結

多くの場合、アンダースコア文字 (`_`) を使用せずに、次の連続する行でステートメントを続行できます。 次の構文要素は、ステートメントを次のコード行で暗黙的に続行します。

- コンマ (`,`) の後。 例 :

   [!code-vb[VbVbalrLineContinuation#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#1)]

- 開いているかっこ (`(`) の後、または閉じかっこ (`)`) の前。 例 :

   [!code-vb[VbVbalrLineContinuation#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#2)]

- 左中かっこ (`{`) の後、または右中かっこ (`}`) の前。 例 :

    [!code-vb[VbVbalrLineContinuation#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#3)]

    詳細については、「[オブジェクト初期化子: 名前付き、匿名型](./objects-and-classes/object-initializers-named-and-anonymous-types.md)、または[コレクション初期化子](./collection-initializers/index.md)」を参照してください。

- 開いている埋め込み式 (`<%=`) の後、または XML リテラル内の埋め込み式 (`%>`) の終了前。 例 :

   [!code-vb[VbVbalrLineContinuation#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#4)]

   詳細については、「[XML での埋め込み式](./xml/embedded-expressions-in-xml.md)」を参照してください。

- 連結演算子 (`&`) の後。 例 :

   [!code-vb[VbVbcnConventions#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/vb/Class1.vb#9)]

   詳細については、「[機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)」を参照してください。

- 代入演算子 (`=`、`&=`、`:=`、`+=`、`-=`、`*=`、`/=`、`\=`、`^=`、`<<=`、`>>=`) 後。 例 :

   [!code-vb[VbVbalrLineContinuation#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#5)]

   詳細については、「[機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)」を参照してください。

- 二項演算子 (`+`、`-`、`/`、`*`、`Mod`、`<>`、`<`、`>`、`<=`、`>=`、`^`) の後に、式の中で、`>>`、`<<`、`And`)。`AndAlso``Or``OrElse``Like``Xor` 例 :

   [!code-vb[VbVbalrLineContinuation#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#7)]

   詳細については、「[機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)」を参照してください。

- `Is` 演算子と `IsNot` 演算子の後。 例 :

   [!code-vb[VbVbalrLineContinuation#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#8)]

   詳細については、「[機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)」を参照してください。

- メンバー修飾子文字 (`.`) の後、およびメンバー名の前。 例 :

   [!code-vb[VbVbalrLineContinuation#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#5)]

   ただし、`With` ステートメントを使用する場合、または型の初期化リストに値を指定する場合は、メンバー修飾子文字の後に行連結文字 (`_`) を含める必要があります。 `With` ステートメントまたはオブジェクトの初期化リストを使用している場合は、代入演算子 (`=`など) の後にある行を分割することを検討してください。 例 :

   [!code-vb[VbVbalrLineContinuation#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#14)]

   詳細については、「」を参照してください。 [ステートメント](../../../visual-basic/language-reference/statements/with-end-with-statement.md)またはオブジェクト初期化子を使用した終了[: 名前付きの型と匿名型](./objects-and-classes/object-initializers-named-and-anonymous-types.md)。

- XML 軸プロパティ修飾子 (`.` または `.@` または `...`) の後。 ただし、`With` キーワードを使用する場合は、メンバー修飾子を指定するときに、行連結文字 (`_`) を含める必要があります。 例 :

   [!code-vb[VbVbalrLineContinuation#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#9)]

   詳細については、「[XML 軸のプロパティ](../../../visual-basic/language-reference/xml-axis/index.md)」を参照してください。

- 属性を指定するときに、小なり記号 (<) またはそれより大きい記号 (`>`) の後。 属性を指定する場合は、大なり記号 (`>`) の後にもなります。 ただし、アセンブリレベルまたはモジュールレベルの属性を指定する場合は、行連結文字 (`_`) を含める必要があります。 例 :

   [!code-vb[VbVbalrLineContinuation#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#10)]

   詳細については、「[属性の概要](../../../visual-basic/programming-guide/concepts/attributes/index.md)」を参照してください。

- クエリ演算子の前後 (`Aggregate`、`Distinct`、`From`、`Group By`、`Group Join`、`Join`、`Let`、`Order By`、`Select`、`Skip`、`Skip While`、`Take`、`Take While`、`Where`、および `In`) に対して実行されます。`Into``On``Ascending``Descending` 複数のキーワード (`Order By`、`Group Join`、`Take While`、および `Skip While`) で構成されているクエリ演算子のキーワード間で行を分割することはできません。 例 :

   [!code-vb[VbVbalrLineContinuation#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#11)]

   詳細については、「[クエリ](../../../visual-basic/language-reference/queries/index.md)」を参照してください。

- `For Each` ステートメントの `In` キーワードの後。 例 :

   [!code-vb[VbVbalrLineContinuation#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#12)]

   詳細については、[For Each...Next ステートメント](../../../visual-basic/language-reference/statements/for-each-next-statement.md) を参照してください。

- コレクション初期化子内の `From` キーワードの後。 例 :

   [!code-vb[VbVbalrLineContinuation#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#13)]

   詳細については、「[コレクション初期化子](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)」を参照してください。

## <a name="adding-comments"></a>コメントの追加

ソースコードは、それを記述したプログラマであっても、常に自明であるとは限りません。 コードを文書化するために、ほとんどのプログラマは、埋め込みコメントを自由に使用します。 コード内のコメントは、後で読み取りや操作を行うためのプロシージャまたは特定の命令を説明することができます。 Visual Basic は、コンパイル時にコメントを無視し、コンパイルされたコードには影響しません。

コメント行の先頭には、アポストロフィ (`'`) または `REM` の後にスペースが付きます。 文字列内を除き、コード内の任意の場所に追加できます。 ステートメントにコメントを追加するには、ステートメントの後にコメントを続けて、アポストロフィまたは `REM` を挿入します。 コメントは、独自の行を使用することもできます。 次の例は、これらの可能性を示しています。

[!code-vb[VbVbalrStatements#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#72)]

## <a name="checking-compilation-errors"></a>コンパイルエラーの確認

コード行を入力した後に、線が青い下線付きで表示されている場合 (エラーメッセージも表示される場合があります)、ステートメントに構文エラーがあります。 ステートメントに問題がないことを確認する必要があります (タスク一覧を検索するか、マウスポインターを使用してエラーをポイントし、エラーメッセージを読み取って)、修正します。 コード内のすべての構文エラーを修正するまで、プログラムは正しくコンパイルされません。

## <a name="related-sections"></a>関連項目

|用語|Definition|
|---|---|
|[代入演算子](../../../visual-basic/language-reference/operators/assignment-operators.md)|`=`、`*=`、`&=`などの代入演算子を扱う言語リファレンスページへのリンクを示します。|
|[演算子および式](./operators-and-expressions/index.md)|要素を演算子と組み合わせて新しい値を生成する方法について説明します。|
|[方法 : コード内でステートメントを分割および連結する](../../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)|1つのステートメントを複数の行に分割する方法と、複数のステートメントを同じ行に配置する方法を示します。|
|[方法 : ステートメントへのラベル付け](../../../visual-basic/programming-guide/program-structure/how-to-label-statements.md)|コード行にラベルを付ける方法について説明します。|
