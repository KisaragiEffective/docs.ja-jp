---
title: クエリの作成
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], writing
- LINQ [Visual Basic], walkthroughs
- LINQ [Visual Basic], writing queries
- writing LINQ queries [Visual Basic]
ms.assetid: f0045808-b9fe-4d31-88d1-473d9957211e
ms.openlocfilehash: 6d2e472cc996c42aa091ed95c6954d0879c98372
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636758"
---
# <a name="walkthrough-writing-queries-in-visual-basic"></a>チュートリアル: Visual Basic でクエリを記述する

このチュートリアルでは、Visual Basic 言語機能を使用して、統合言語クエリ (LINQ) クエリ式を記述する方法について説明します。 このチュートリアルでは、Student オブジェクトのリスト、クエリの実行方法、およびその変更方法に関するクエリを作成する方法について説明します。 クエリには、オブジェクト初期化子、ローカル型推論、匿名型など、いくつかの機能が組み込まれています。

このチュートリアルを完了すると、目的の特定の LINQ プロバイダーのサンプルとドキュメントに進むことができます。 LINQ プロバイダーには、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]、LINQ to DataSet、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]が含まれます。

## <a name="create-a-project"></a>プロジェクトの作成

### <a name="to-create-a-console-application-project"></a>コンソールアプリケーションプロジェクトを作成するには

1. Visual Studio を起動します。

2. **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。

3. **[インストールされたテンプレート]** の一覧で、 **[Visual Basic]** をクリックします。

4. プロジェクトの種類の一覧で、 **[コンソールアプリケーション]** をクリックします。 **[名前]** ボックスにプロジェクトの名前を入力し、 **[OK]** をクリックします。

    プロジェクトが作成されます。 既定では、このファイルには、system.servicemodel への参照が含まれています。 また、[参照] ページの [インポートされた**名前空間**] の一覧にある[プロジェクトデザイナー (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)には、<xref:System.Linq?displayProperty=nameWithType> 名前空間が含まれています。

5. [[コンパイル] ページのプロジェクトデザイナー (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)で、[**推定]** が **[オン**] に設定されていることを確認します。

## <a name="add-an-in-memory-data-source"></a>メモリ内データソースを追加する

このチュートリアルのクエリのデータソースは、`Student` オブジェクトの一覧です。 各 `Student` オブジェクトには、学生の本文で、名、姓、クラスの年、および教育の順位が含まれています。

### <a name="to-add-the-data-source"></a>データ ソースを追加するには

- `Student` クラスを定義し、クラスのインスタンスの一覧を作成します。

  > [!IMPORTANT]
  > `Student` クラスを定義し、チュートリアルの例で使用するリストを作成するために必要なコードは、 [「方法: 項目のリストを作成](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md)する」に記載されています。 そこからコピーして、プロジェクトに貼り付けることができます。 新しいコードは、プロジェクトの作成時に表示されたコードを置き換えます。

### <a name="to-add-a-new-student-to-the-students-list"></a>学生リストに新しい学生を追加するには

- `getStudents` メソッドのパターンに従って、`Student` クラスの別のインスタンスを一覧に追加します。 学生を追加すると、オブジェクト初期化子が導入されます。 詳細については、「[オブジェクト初期化子: 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)」を参照してください。

## <a name="create-a-query"></a>クエリを作成する

実行すると、このセクションに追加されたクエリによって、教育機関のランクが上位10に配置される学生のリストが生成されます。 クエリは毎回、完全な `Student` オブジェクトを選択するので、クエリ結果の型は `IEnumerable(Of Student)`になります。 ただし、クエリの型は、通常、クエリ定義では指定されていません。 代わりに、コンパイラはローカル型の推論を使用して型を決定します。 詳細については、「[ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」を参照してください。 クエリの範囲変数 `currentStudent`は、ソース内の各 `Student` インスタンスへの参照として機能します。 `students`は、`students`内の各オブジェクトのプロパティへのアクセスを提供します。

### <a name="to-create-a-simple-query"></a>簡単なクエリを作成するには

1. プロジェクトの `Main` メソッドで、次のようにマークされている場所を見つけます。

    [!code-vb[VbLINQWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#1)]

    次のコードをコピーし、に貼り付けます。

    [!code-vb[VbLINQWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#2)]

2. コード内の `studentQuery` 上にマウスポインターを置き、コンパイラによって割り当てられた型が `IEnumerable(Of Student)`ことを確認します。

## <a name="run-the-query"></a>クエリを実行する

変数 `studentQuery` には、クエリの実行結果ではなく、クエリの定義が含まれています。 クエリを実行するための一般的なメカニズムは、`For Each` ループです。 返されたシーケンスの各要素は、ループ反復変数を介してアクセスされます。 クエリ実行の詳細については、「初めての[LINQ クエリの作成](../../../../visual-basic/programming-guide/concepts/linq/writing-your-first-linq-query.md)」を参照してください。

### <a name="to-run-the-query"></a>クエリを実行するには

1. プロジェクトのクエリの下に、次の `For Each` ループを追加します。

    [!code-vb[VbLINQWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#3)]

2. マウスポインターをループコントロール変数 `studentRecord` 上に置くと、データ型が表示されます。 `studentQuery` は `Student` インスタンスのコレクションを返すため、`studentRecord` の型は `Student`されることが推定されます。

3. CTRL キーを押しながら F5 キーを押して、アプリケーションをビルドして実行します。 コンソールウィンドウに結果が記録されます。

## <a name="modify-the-query"></a>クエリの変更

指定された順序でクエリを実行すると、クエリの結果を簡単にスキャンできます。 返されたシーケンスは、使用可能な任意のフィールドに基づいて並べ替えることができます。

### <a name="to-order-the-results"></a>結果を並べ替えるには

1. `Where` ステートメントとクエリの `Select` ステートメントの間に、次の `Order By` 句を追加します。 `Order By` 句は、各学生の姓に従って、結果をアルファベット順に並べ替えます。

    ```vb
    Order By currentStudent.Last Ascending
    ```

2. 姓と名の順序を指定するには、両方のフィールドをクエリに追加します。

    ```vb
    Order By currentStudent.Last Ascending, currentStudent.First Ascending
    ```

    `Descending` を指定して、Z から A への順序を指定することもできます。

3. CTRL キーを押しながら F5 キーを押して、アプリケーションをビルドして実行します。 コンソールウィンドウに結果が記録されます。

### <a name="to-introduce-a-local-identifier"></a>ローカル識別子を導入するには

1. このセクションのコードを追加して、クエリ式にローカル識別子を導入します。 ローカル識別子は、中間結果を保持します。 次の例では、`name` は、学生の姓と名の連結を保持する識別子です。 ローカル識別子は便宜上使用できます。または、複数回計算される式の結果を格納することによって、パフォーマンスを向上させることができます。

    [!code-vb[VbLINQWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#4)]

2. CTRL キーを押しながら F5 キーを押して、アプリケーションをビルドして実行します。 コンソールウィンドウに結果が記録されます。

### <a name="to-project-one-field-in-the-select-clause"></a>Select 句で1つのフィールドを射影するには

1. このセクションからクエリと `For Each` ループを追加して、ソース内の要素とは異なる要素を持つシーケンスを生成するクエリを作成します。 次の例では、ソースは `Student` オブジェクトのコレクションですが、返されるのは各オブジェクトの1つのメンバーのみです。姓が指定されている学生の名は、最初の名前になります。 `currentStudent.First` は文字列であるため、`studentQuery3` によって返されるシーケンスのデータ型は `IEnumerable(Of String)`、文字列のシーケンスです。 前の例と同様に、`studentQuery3` のデータ型の割り当ては、コンパイラがローカル型の推論を使用して判断するために残されています。

    [!code-vb[VbLINQWalkthrough#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#5)]

2. コード内の `studentQuery3` 上にマウスポインターを置いて、割り当てられている型が `IEnumerable(Of String)`ことを確認します。

3. CTRL キーを押しながら F5 キーを押して、アプリケーションをビルドして実行します。 コンソールウィンドウに結果が記録されます。

### <a name="to-create-an-anonymous-type-in-the-select-clause"></a>Select 句で匿名型を作成するには

1. クエリで匿名型がどのように使用されるかを確認するには、このセクションのコードを追加します。 クエリで使用するのは、完全なレコード (前の例では`currentStudent` レコード) または1つのフィールド (前のセクションで`First`) ではなく、データソースから複数のフィールドを返す場合です。 結果に含めるフィールドを含む新しい名前付きの型を定義する代わりに、`Select` 句でフィールドを指定すると、そのフィールドをプロパティとして持つ匿名型がコンパイラによって作成されます。 詳細については、「[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。

    次の例では、学術ランクが 1 ~ 10 の範囲の seniors の名前とランクを返すクエリを作成します。 この例では、`Select` 句が匿名型のインスタンスを返し、匿名型に使用可能な名前がないため、`studentQuery4` の型を推論する必要があります。

    [!code-vb[VbLINQWalkthrough#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#6)]

2. CTRL キーを押しながら F5 キーを押して、アプリケーションをビルドして実行します。 コンソールウィンドウに結果が記録されます。

## <a name="additional-examples"></a>その他の例

基本を理解したところで、LINQ クエリの柔軟性と機能を示すその他の例を次に示します。 各例には、その動作についての簡単な説明が付いています。 各クエリのクエリ結果変数の上にマウスポインターを置くと、推論された型が表示されます。 結果を生成するには、`For Each` ループを使用します。

[!code-vb[VbLINQWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#7)]

## <a name="additional-information"></a>追加情報

クエリの使用に関する基本的な概念を理解した後は、関心のある特定の種類の LINQ プロバイダーのドキュメントとサンプルを読むことができます。

- [LINQ to Objects](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)

- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)

- [LINQ to XML](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md)

- [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)

## <a name="see-also"></a>関連項目

- [統合言語クエリ (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/index.md)
- [Visual Basic の LINQ の概要](../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [オブジェクト初期化子 : 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
