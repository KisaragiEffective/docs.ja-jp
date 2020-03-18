---
title: 配列
ms.date: 12/06/2017
f1_keywords:
- vb.Array
helpviewer_keywords:
- arrays [Visual Basic]
- Visual Basic, arrays
ms.assetid: dbf29737-b589-4443-bee6-a27588d9c67e
ms.openlocfilehash: 9dfe7814b00b4d060fa4ab9aa594faa948217d8d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351865"
---
# <a name="arrays-in-visual-basic"></a>Visual Basic における配列

配列は、互いに論理的に関連付けられている、*要素*と呼ばれる一連の値です。 たとえば、配列は、文法学校の各学年の生徒の数で構成されている場合があります。配列の各要素は、1つの学年の生徒の数です。 同様に、配列はクラスの生徒の成績で構成される場合もあります。配列の各要素は、1つのグレードです。

個々の変数は、各データ項目を格納することができます。 たとえば、アプリケーションで学生の成績を分析する場合は、`englishGrade1`、`englishGrade2`など、生徒の成績ごとに個別の変数を使用できます。このアプローチには、次の3つの主な制限があります。

- デザイン時には、処理する必要があるグレードの数を正確に把握しておく必要があります。
- 多数のグレードを迅速に処理することは、扱いにくくなります。 これにより、アプリケーションで深刻なバグが発生する可能性が高くなります。
- 管理が困難です。 新しいグレードを追加するたびに、アプリケーションの変更、再コンパイル、再デプロイが必要になります。

配列を使用すると、これらの関連する値を同じ名前で参照できます。また、*インデックス*または*添字*と呼ばれる番号を使用して、配列内の位置に基づいて個々の要素を識別できます。 配列のインデックスは、0から、配列内の要素の総数より1小さい値までの範囲です。 Visual Basic 構文を使用して配列のサイズを定義する場合は、配列内の要素の総数ではなく、最もインデックス番号の大きいインデックスを指定します。 配列を1つの単位として操作できます。また、その要素を反復処理することで、デザイン時に含まれる要素の数を正確に把握する必要がなくなります。

説明する前に、簡単な例をいくつか紹介します。

```vb
' Declare a single-dimension array of 5 numbers.
Dim numbers(4) As Integer

' Declare a single-dimension array and set its 4 values.
Dim numbers = New Integer() {1, 2, 4, 8}

' Change the size of an existing array to 16 elements and retain the current values.
ReDim Preserve numbers(15)

' Redefine the size of an existing array and reset the values.
ReDim numbers(15)

' Declare a 6 x 6 multidimensional array.
Dim matrix(5, 5) As Double

' Declare a 4 x 3 multidimensional array and set array element values.
Dim matrix = New Integer(3, 2) {{1, 2, 3}, {2, 3, 4}, {3, 4, 5}, {4, 5, 6}}

' Declare a jagged array
Dim sales()() As Double = New Double(11)() {}
```

## <a name="array-elements-in-a-simple-array"></a>単純な配列の配列要素

`students` という名前の配列を作成して、各学年の生徒の数を文法学校に格納してみましょう。 要素のインデックスの範囲は 0 から 6 までです。 この配列の使用は、7つの変数を宣言するよりも簡単です。

`students` 配列を次の図に示します。 配列の各要素は、次のとおりです。

- 要素のインデックスは、学年を表します (インデックス 0 は幼稚園を表します)。

- 要素に含まれている値は、その学年の生徒の数を表します。

![学生の数の配列を示す図](./media/index/students-array-elements.gif)

次の例には、配列を作成して使用する Visual Basic コードが含まれています。

[!code-vb[simple-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/simple-array.vb)]

この例では、次の3つの処理を行います。

- 7つの要素を持つ `students` 配列を宣言します。 配列宣言内の `6` 数値は、配列内の最後のインデックスを示します。配列内の要素の数より1小さい値です。
- 配列の各要素に値が割り当てられます。 配列要素にアクセスするには、配列名を使用し、個々の要素のインデックスをかっこで囲んで指定します。
- 配列の各値が一覧表示されます。 この例では、 [`For`](../../../language-reference/statements/for-next-statement.md)ステートメントを使用して、インデックス番号で配列の各要素にアクセスします。

前の例の `students` 配列は、1つのインデックスを使用するため、1次元配列です。 複数のインデックスまたは添字を使用する配列を*多次元*と呼びます。 詳細については、この記事の残りの部分と[Visual Basic の配列ディメンション](../../language-features/arrays/array-dimensions.md)を参照してください。

## <a name="creating-an-array"></a>配列の作成

配列のサイズは、次のいくつかの方法で定義できます。

- 配列が宣言されている場合は、サイズを指定できます。

  [!code-vb[creating1](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#1)]

- `New` 句を使用して、作成時に配列のサイズを指定できます。

  [!code-vb[creating2](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#2)]

既存の配列がある場合は、 [`ReDim`](../../../language-reference/statements/redim-statement.md)ステートメントを使用してそのサイズを再定義できます。 `ReDim` ステートメントが配列内の値を保持するように指定することも、空の配列を作成するように指定することもできます。 次に、 `ReDim` ステートメントを使用して既存の配列のサイズを変更する例をいくつか示します。

[!code-vb[redimensioning](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#3)]

詳細については、「 [ReDim ステートメント](../../../language-reference/statements/redim-statement.md)」を参照してください。

## <a name="storing-values-in-an-array"></a>配列への値の格納

配列のそれぞれの位置には、 `Integer`型のインデックスを使用してアクセスできます。 かっこで囲まれたインデックスを使用して配列のそれぞれの位置を参照することで、配列の値を格納および取得することができます。 多次元配列のインデックスは、コンマ (,) で区切られます。 配列の次元ごとに 1 つのインデックスが必要です。

次の例は、配列の値を格納および取得するいくつかのステートメントを示しています。

[!code-vb[store-and-retrieve](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/store-and-retrieve.vb)]

## <a name="populating-an-array-with-array-literals"></a>配列に配列リテラルを設定する

配列リテラルを使用すると、配列を作成するときに、初期値のセットを配列に設定できます。 配列リテラルは、中かっこ (`{}`) で囲んだ値のコンマ区切りの一覧で構成されます。

配列リテラルを使用して配列を作成する場合、配列の型を指定するか、型の推定を使用して配列の型を決定することができます。 次の例では、両方のオプションを示します。

[!code-vb[create-with-literals](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#4)]

型の推定を使用する場合、配列の型は、リテラル値のリストの中で最も優先される*型*によって決まります。 最も優先される型は、配列内の他のすべての型の幅を広げることができる型です。 この一意の型を特定できない場合、最も優先度の高い型は、配列内の他のすべての型から縮小変換できる一意の型になります。 これらの一意の型をどちらも特定できない場合は、 `Object`が最も優先度の高い型になります。 たとえば、配列リテラルに指定された値の一覧に `Integer`型、 `Long`型、および `Double`型の値が含まれている場合、結果の配列の型は `Double`です。 `Integer` と `Long` は `Double`にのみ拡大されるため、`Double` は最も優先される型です。 詳細については、[拡大変換と縮小変換](../../language-features/data-types/widening-and-narrowing-conversions.md)に関するページを参照してください。

> [!NOTE]
> 型の推定は、型のメンバーでローカル変数として定義されている配列に対してのみ使用できます。 明示的な型定義が存在しない場合、クラスレベルで配列リテラルを使用して定義された配列の型は `Object[]`になります。 詳細については、「[ローカル型の推論](../variables/local-type-inference.md)」を参照してください。

前の例では、すべての配列リテラルが `Integer`型であっても `Double` 型の配列として `values` が定義されていることに注意してください。 配列リテラルの値を `Double` 値に拡大変換できるため、この配列を作成できます。

また、*入れ子になった配列リテラル*を使用して、多次元配列を作成および設定することもできます。 入れ子になった配列リテラルには、結果の配列と一致する複数の次元が必要です。 次の例では、入れ子になった配列リテラルを使用して、整数の2次元配列を作成します。

[!code-vb[nested-array-literals](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#5)]

入れ子になった配列リテラルを使用して配列を作成および設定する場合、入れ子になった配列リテラル内の要素の数が一致しないと、エラーが発生します。 配列のリテラルとは異なる数の次元を持つように配列変数を明示的に宣言した場合にも、エラーが発生します。

1次元配列の場合と同じように、入れ子になった配列リテラルを使用して多次元配列を作成する場合は、型の推定を利用できます。 推論される型は、すべての入れ子レベルのすべての配列リテラルに含まれるすべての値の中で最も優先度の高い型です。 次の例では、`Integer` と `Double`型の値から `Double[,]` 型の2次元配列を作成します。

[!code-vb[nested-type-inference](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#6)]

その他の例については、「[方法:Visual Basic](../../language-features/arrays/how-to-initialize-an-array-variable.md)で配列変数を初期化します。

## <a name="iterating-through-an-array"></a>配列の反復処理

配列を反復処理する場合、最小のインデックスから最大のインデックスまで、または最大から最小まで、配列内の各要素にアクセスします。 通常、[For...Nextステートメント](../../../language-reference/statements/for-next-statement.md)または[For Each...Next ステートメント](../../../language-reference/statements/for-each-next-statement.md)のいずれかを使用して、配列の要素を反復処理します。 配列の上限がわからない場合、<xref:System.Array.GetUpperBound%2A?displayProperty=nameWithType>を呼び出すことでインデックスの最大値を取得できます。 最小のインデックス値は、ほとんどの場合 0 ですが、<xref:System.Array.GetLowerBound%2A?displayProperty=nameWithType>を呼び出すことでインデックスの最小値を取得できます。

次の例は、 [`For...Next`](../../../language-reference/statements/for-next-statement.md)ステートメントを使用して、1 次元配列を反復処理します。

[!code-vb[iterate-one-dimensional-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate1d.vb)]

次の例は、[`For...Next`](../../../language-reference/statements/for-next-statement.md)ステートメントを使用して、多次元配列を反復処理します。 <xref:System.Array.GetUpperBound%2A> メソッドには、次元を指定するパラメーターがあります。 `GetUpperBound(0)` は 1 次元目の最も大きいインデックスを返し、`GetUpperBound(1)` は 2 次元目の最も大きいインデックスを返します。

[!code-vb[iterate-two-dimensional-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate2d.vb)]

次の例では、[For Each...Next ステートメント](../../../language-reference/statements/for-each-next-statement.md)を使用して、1 次元配列と、2 次元配列を反復処理します。

[!code-vb[iterate-for-each-next](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate-for-each-next.vb)]

## <a name="array-size"></a>配列のサイズ

配列のサイズは、そのすべての次元の長さの積です。 これは、現在配列に含まれている要素の総数を表します。  たとえば、次の例では、各次元に4つの要素を持つ2次元配列を宣言しています。 この例の出力結果が示すように、配列のサイズは 16 (または (3 + 1) * (3 + 1) です。

[!code-vb[array-size](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-size.vb)]

> [!NOTE]
> 配列のサイズに関するこの説明は、ジャグ配列には適用されません。 ジャグ配列とジャグ配列のサイズの決定の詳細については、「[ジャグ](#jagged-arrays)配列」を参照してください。

配列のサイズは、<xref:System.Array.Length%2A?displayProperty=nameWithType> プロパティを使用して確認できます。 多次元配列の各次元の長さは、<xref:System.Array.GetLength%2A?displayProperty=nameWithType> メソッドを使用して見つけることができます。

配列変数のサイズを変更するには、新しい配列オブジェクトを割り当てるか、または[`ReDim` statement](../../../language-reference/statements/redim-statement.md)ステートメントを使用します。 次の例では、`ReDim` ステートメントを使用して、100要素の配列を51要素の配列に変更します。

[!code-vb[resize-an-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-size2.vb)]

配列のサイズを扱う際に考慮する必要がある点がいくつかあります。

|||
|---|---|
|次元の長さ|各次元のインデックスは0から始まるため、0から上限までの範囲になります。 したがって、特定の次元の長さは、その次元の宣言された上限を超えています。|
|長さの制限|配列のすべての次元の長さは、`Integer` データ型の最大値に制限されます。これは、または (2 ^ 31)-1 <xref:System.Int32.MaxValue?displayProperty=nameWithType> ます。 しかし、配列のサイズの総数も、システムで利用できるメモリによって制限されます。 使用可能なメモリの量を超える配列を初期化しようとすると、ランタイムによって <xref:System.OutOfMemoryException>がスローされます。|
|サイズおよび要素のサイズ|配列のサイズは、その要素のデータ型には依存しません。 サイズは常に、メモリ内で使用されるバイト数ではなく、要素の合計数を表します。|
|メモリの使用量|配列がどのようにメモリに格納されるかに関して、前提を置くことは安全ではありません。 ストレージは、プラットフォームのデータ幅が異なると変わります。したがって、同じ配列でも、32 ビットのシステムよりも 64 ビットのシステムの方が多くのメモリを使用します。 配列を初期化すると、システム構成に応じて、要素をできるだけ近くに集めるように、またはすべてがハードウェア自体の境界に合致するように、共通言語ランタイム (CLR: Common Language Runtime) によってストレージが割り当てられます。 また、配列は制御情報のためにストレージのオーバーヘッドを必要とします。このオーバーヘッドは、次元が追加されるごとに増加します。|

## <a name="the-array-type"></a>配列型

すべての配列は、その要素のデータ型とは異なるデータ型を持ちます。 すべての配列を包括する 1 つのデータ型はありません。 代わりに、配列のデータ型は、配列の次元数 ( *ランク*) と配列の要素のデータ型によって決まります。 2つの配列変数のデータ型は、ランクが同じで、その要素のデータ型が同じである場合にのみ、同じデータ型になります。 配列の次元の長さは、配列のデータ型には影響しません。

すべての配列は、<xref:System.Array?displayProperty=nameWithType> クラスから継承しています。また、`Array` 型として変数を宣言できますが、`Array` 型の配列は作成できません。 たとえば、次のコードでは、`arr` 変数を型 `Array` として宣言し、<xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> メソッドを呼び出して配列をインスタンス化していますが、配列の型は Object [] であることが証明されています。

[!code-vb[array-class](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-class.vb)]

また、[ReDim ステートメント](../../../language-reference/statements/redim-statement.md) は、`Array` 型として宣言された変数上では使用できません。 これらの理由やタイプセーフについては、すべての配列を特定の型として宣言することをお勧めします。

いくつかの方法で、配列またはその要素のいずれかのデータ型を知ることができます。

- 変数の <xref:System.Object.GetType%2A> メソッドを呼び出して、変数の実行時の型を表す <xref:System.Type> オブジェクトを取得できます。 <xref:System.Type> オブジェクトでは、プロパティおよびメソッドに詳細情報が保持されます。
- 変数を <xref:Microsoft.VisualBasic.Information.TypeName%2A> 関数に渡して、実行時の型の名前を持つ `String` を取得できます。

次の例では、`GetType` メソッドと `TypeName` 関数の両方を呼び出して、配列の型を決定します。 配列の型が `Byte(,)`です。 <xref:System.Type.BaseType%2A?displayProperty=nameWithType> プロパティは、バイト配列の基本型が <xref:System.Array> クラスであることも示します。

[!code-vb[array-type](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-type.vb)]

## <a name="arrays-as-return-values-and-parameters"></a>戻り値およびパラメーターとしての配列

`Function` プロシージャから配列を返すには、[Function ステートメント](../../../language-reference/statements/function-statement.md)の戻り値の型として配列のデータ型と次元数を指定します。 関数内で、同じデータ型と次元数を持つローカルの配列変数を宣言します。 [Return ステートメント](../../../language-reference/statements/return-statement.md)には、かっこを使用せずにローカルの配列変数を含めます。

配列を `Sub` プロシージャまたは `Function` プロシージャのパラメーターとして指定するには、パラメーターを配列として定義して、データ型と次元数を指定します。 プロシージャの呼び出しで、同じデータ型と次元数を持つ配列変数を渡します。

次の例では、`GetNumbers` 関数は `Integer`型の1次元配列 `Integer()`を返します。 `ShowNumbers` プロシージャは、 `Integer()` の引数を受け取ります。

[!code-vb[return-value-and-params](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/return-values-and-params.vb)]

次の例では、`GetNumbersMultiDim` 関数は `Integer`型の2次元配列 `Integer(,)`を返します。  `ShowNumbersMultiDim` プロシージャは、 `Integer(,)` の引数を受け取ります。

[!code-vb[multidimensional-return-value](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/return-values-and-params-2d.vb)]

## <a name="jagged-arrays"></a>ジャグ配列

アプリケーションのデータ構造は、2 次元の配列であっても四角形の 2 次元配列ではない場合があります。 たとえば、配列を使用して、その月の各日の高気温に関するデータを格納することができます。 配列の最初の次元は月を表しますが、2番目の次元は日数を表し、月の日数は一様ではありません。 配列*の配列*とも呼ばれる*ジャグ配列*は、このようなシナリオ向けに設計されています。 ジャグ配列は、要素が配列でもある配列です。 ジャグ配列と、ジャグ配列の各要素は、1 次元でも多次元でもかまいません。

次の例では、月の配列を使用します。各要素は日付の配列です。 この例では、異なる月の日数が異なるため、ジャグ配列を使用しています。  この例では、ジャグ配列を作成し、値を代入し、その値を取得して表示する方法を示します。

[!code-vb[jagged-arrays](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged.vb)]

前の例では、`For...Next` ループを使用して、要素ごとにジャグ配列に値を代入しています。 入れ子になった配列リテラルを使用して、ジャグ配列の要素に値を代入することもできます。 ただし、入れ子になった配列リテラル (`Dim valuesjagged = {{1, 2}, {2, 3, 4}}`など) を使用しようとすると、コンパイラエラー [BC30568](../../../,,/../misc/bc30568.md)が生成されます。 エラーを修正するには、内部配列リテラルをかっこで囲みます。 次の例に示すように、かっこによって配列リテラル式が評価され、結果の値が外側の配列リテラルと共に使用されます。

[!code-vb[jagged-array-initialization](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged-assign.vb)]

ジャグ配列は、配列を格納する要素を持つ1次元配列です。 したがって、<xref:System.Array.Length%2A?displayProperty=nameWithType> プロパティと `Array.GetLength(0)` メソッドは、1次元配列内の要素の数を返します。ジャグ配列は多次元ではないため、`Array.GetLength(1)` は <xref:System.IndexOutOfRangeException> をスローします。 各サブ配列の <xref:System.Array.Length%2A?displayProperty=nameWithType> プロパティの値を取得することによって、各サブ配列内の要素の数を決定します。 次の例は、ジャグ配列内の要素の数を確認する方法を示しています。

[!code-vb[jagged-array-size](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged-length.vb)]

## <a name="zero-length-arrays"></a>長さ0の配列

初期化されていない配列 (値が `Nothing`である配列) と*長さ0の配列*または空の配列 (要素を持たない配列) を区別 Visual Basic。初期化されていない配列は、次元が指定されていないか、値が割り当てられていない配列です。 次に例を示します。

```vb
Dim arr() As String
```

長さ0の配列は、次元-1 で宣言されています。 次に例を示します。

```vb
Dim arrZ(-1) As String
```

次のような場合に、長さ 0 の配列を作成する必要があります。

- <xref:System.NullReferenceException> 例外が発生しない場合、コードは <xref:System.Array.Length%2A> や <xref:System.Array.Rank%2A>などの <xref:System.Array> クラスのメンバーにアクセスするか、Visual Basic などの <xref:Microsoft.VisualBasic.Information.UBound%2A>関数を呼び出す必要があります。

- `Nothing` を特別なケースとしてチェックする必要がないため、コードを単純にしておく必要があります。

- コードで、長さ 0 の配列を 1 つ以上のプロシージャに渡す必要があるアプリケーション プログラミング インターフェイス (API: Application Programming Interface) とやり取りする場合、または API の 1 つ以上のプロシージャから長さ 0 の配列が返される場合。

## <a name="splitting-an-array"></a>配列の分割

場合によっては、1つの配列を複数の配列に分割する必要があります。 これには、配列が分割される点を特定し、その配列を2つ以上の個別の配列に spitting ます。

> [!NOTE]
> このセクションでは、区切り記号に基づいて1つの文字列を文字列配列に分割する方法については説明しません。 文字列の分割の詳細については、<xref:System.String.Split%2A?displayProperty=nameWithType> メソッドを参照してください。

配列を分割するための最も一般的な条件は次のとおりです。

- 配列の要素数。 たとえば、指定された数を超える要素の配列を、ほぼ同じ数の部分に分割することができます。 このためには、<xref:System.Array.Length%2A?displayProperty=nameWithType> または <xref:System.Array.GetLength%2A?displayProperty=nameWithType> のいずれかの方法で返された値を使用できます。

- 要素の値。配列を分割する場所を示す区切り記号として機能します。 <xref:System.Array.FindIndex%2A?displayProperty=nameWithType> および <xref:System.Array.FindLastIndex%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、特定の値を検索できます。

配列を分割する必要があるインデックスを特定したら、<xref:System.Array.Copy%2A?displayProperty=nameWithType> メソッドを呼び出して個々の配列を作成できます。

次の例では、配列を、ほぼ等しいサイズの2つの配列に分割します。 (配列要素の合計数が奇数の場合、最初の配列には2番目の配列よりも1つ多くの要素があります)。

[!code-vb[splitting-an-array-by-length](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/split1.vb)]

次の例では、配列の区切り記号として機能する値が "zzz" である要素の存在に基づいて、文字列の配列を2つの配列に分割します。 新しい配列には、区切り記号を含む要素は含まれません。

[!code-vb[splitting-an-array-by-delimiter](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/split2.vb)]

## <a name="joining-arrays"></a>配列の結合

また、複数の配列を結合して1つの大きな配列を作成することもできます。 これを行うには、<xref:System.Array.Copy%2A?displayProperty=nameWithType> メソッドも使用します。

> [!NOTE]
> ここでは、文字列配列を1つの文字列に結合する方法については説明しません。 文字列配列の結合の詳細については、<xref:System.String.Join%2A?displayProperty=nameWithType> メソッドを参照してください。

各配列の要素を新しい配列にコピーする前に、新しい配列を格納するのに十分な大きさになるように配列を初期化しておく必要があります。 2 つの方法のいずれかでこれを行うことができます。

- [`ReDim Preserve`](../../../language-reference/statements/redim-statement.md)ステートメントを使用して、新しい要素を追加する前に配列を動的に拡張します。 これは最も簡単な方法ですが、大きな配列をコピーする場合は、パフォーマンスが低下し、メモリの消費が過剰になる可能性があります。
- 新しい大きな配列に必要な要素の合計数を計算し、各ソース配列の要素を追加します。

次の例では、2番目の方法を使用して、10個の要素を持つ4つの配列を1つの配列に追加します。

[!code-vb[joining-an-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/join.vb)]

この場合、ソース配列はすべて小さいため、新しい配列の要素を追加するときに、配列を動的に拡張することもできます。 次の例でこれを確認できます。

[!code-vb[joining-an-array-dynamically](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/join2.vb)]

## <a name="collections-as-an-alternative-to-arrays"></a>配列の代わりとしてのコレクション

配列は、数が固定されている厳密に型指定されたオブジェクトの作成および処理に最も適しています。 コレクションは、オブジェクトのグループをより柔軟に処理できます。 [`ReDim` ステートメント](../../../language-reference/statements/redim-statement.md)を使用して配列のサイズを明示的に変更する必要がある配列とは異なり、コレクションは、アプリケーションの変更のニーズに合わせて動的に拡張および縮小されます。

`ReDim` を使用して配列を再ディメンションすると、Visual Basic 新しい配列が作成され、前の配列が解放されます。 これには、実行時間がかかります。 したがって、作業している項目の数が頻繁に変更される場合や、必要な項目の最大数を予測できない場合は、通常、コレクションを使用するとパフォーマンスが向上します。

コレクションによっては、コレクションに含まれるオブジェクトのキーを割り当てると、そのキーを使用してオブジェクトを迅速に取り出すことができます。

含まれる要素が 1 つのデータ型だけのコレクションの場合は、 <xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間のクラスのいずれかを使用できます。 ジェネリック コレクションでは、タイプ セーフが強制されるため、他のデータ型を追加することはできません。

コレクションの詳細については、「[コレクション](../../concepts/collections.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

|項目|定義|
|----------|----------------|
|[Visual Basic における配列のディメンション](../../language-features/arrays/array-dimensions.md)|配列のランクと次元について説明します。|
|[方法: Visual Basic](../../language-features/arrays/how-to-initialize-an-array-variable.md) で配列変数を初期化します|配列に初期値を設定する方法について説明します。|
|[方法: Visual Basic](../../language-features/arrays/how-to-sort-an-array.md) での配列の並べ替え|配列の要素をアルファベット順に並べ替える方法について説明します。|
|[方法: 配列を別の配列に割り当て](../../language-features/arrays/how-to-assign-one-array-to-another-array.md)|配列を別の配列変数に代入するときの手順と規則を説明します。|
|[配列のトラブルシューティング](../../language-features/arrays/troubleshooting-arrays.md)|配列を使用しているときに発生する一般的な問題について説明します。|

## <a name="see-also"></a>関連項目

- <xref:System.Array?displayProperty=nameWithType>
- [Dim ステートメント](../../../language-reference/statements/dim-statement.md)
- [ReDim ステートメント](../../../language-reference/statements/redim-statement.md)
