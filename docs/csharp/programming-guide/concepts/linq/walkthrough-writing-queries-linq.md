---
title: 'チュートリアル: C# でのクエリの作成 (LINQ)'
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], walkthroughs
- LINQ [C#], writing queries
- queries [LINQ in C#], writing
- writing LINQ queries
ms.assetid: 2962a610-419a-4276-9ec8-4b7f2af0c081
ms.openlocfilehash: f2135c6c3649ba2fc87e3b49770439688a58269b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73418051"
---
# <a name="walkthrough-writing-queries-in-c-linq"></a>チュートリアル: C# でのクエリの作成 (LINQ)
このチュートリアルでは、LINQ クエリ式の記述に使用される C# 言語機能について説明します。  
  
## <a name="create-a-c-project"></a>C# プロジェクトの作成  
  
> [!NOTE]
> 以下に示すのは Visual Studio 用の手順です。 別の開発環境を使用している場合は、System.Core.dll への参照と `using` 名前空間の <xref:System.Linq?displayProperty=nameWithType> ディレクティブを使用したコンソール プロジェクトを作成してください。  
  
#### <a name="to-create-a-project-in-visual-studio"></a>Visual Studio でプロジェクトを作成するには  
  
1. Visual Studio を起動します。  
  
2. メニュー バーで、 **[ファイル]** 、 **[新規作成]** 、 **[プロジェクト]** の順に選択します。  
  
     **[新しいプロジェクト]** ダイアログ ボックスが表示されます。  
  
3. **[インストール済み]** 、 **[テンプレート]** 、 **[Visual C#]** の順に展開し、 **[コンソール アプリケーション]** を選択します。  
  
4. **[名前]** テキスト ボックスで、別の名前を入力するか、既定の名前をそのまま使用し、 **[OK]** ボタンを選択します。  
  
     **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。  
  
5. プロジェクトには、System.Core.dll への参照と、`using` 名前空間の <xref:System.Linq?displayProperty=nameWithType> ディレクティブが使用されています。  
  
## <a name="create-an-in-memory-data-source"></a>メモリ内データ ソースの作成  
 クエリのデータ ソースは、`Student` オブジェクトのシンプルなリストです。 各 `Student` レコードには、名前、姓、およびクラスでのテストの点数を表す整数の配列が含まれます。 このコードをプロジェクトにコピーします。 これには、次のような特徴があります。  
  
- `Student` クラスは自動実装プロパティで構成されています。  
  
- リスト内の各生徒は、オブジェクト初期化子で初期化されます。  
  
- リスト自体は、コレクション初期化子で初期化されます。  
  
 この全体的なデータ構造は、コンストラクターへの明示的な呼び出しや、明示的なメンバー アクセスを行うことなく初期化され、インスタンス化されます。 これらの新機能について詳しくは、「[自動実装プロパティ](../../classes-and-structs/auto-implemented-properties.md)」および「[オブジェクト初期化子とコレクション初期化子](../../classes-and-structs/object-and-collection-initializers.md)」をご覧ください。  
  
#### <a name="to-add-the-data-source"></a>データ ソースを追加するには  
  
- `Student` クラスと、初期化された生徒リストを、プロジェクト内の `Program` クラスに追加します。  
  
     [!code-csharp[CsLinqGettingStarted#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#11)]  
  
#### <a name="to-add-a-new-student-to-the-students-list"></a>生徒リストに新しい生徒を追加するには  
  
1. 新しい `Student` を `Students` リストに追加し、目的の名前とテスト点数を使用します。 オブジェクト初期化子の構文をより効果的に学習するために、新しい生徒の情報をすべて入力するようにしてください。  
  
## <a name="create-the-query"></a>クエリの作成  
  
#### <a name="to-create-a-simple-query"></a>簡単なクエリを作成するには  
  
- アプリケーションの `Main` メソッドで、シンプルなクエリを作成します。このクエリでは、実行時に、最初のテストの点数が 90 を超える全生徒のリストを生成するようにします。 `Student` オブジェクト全体が選択されているため、クエリの型は `IEnumerable<Student>` です。 コードでは、[var](../../../language-reference/keywords/var.md) キーワードを使用して暗黙的型指定を使用することもできますが、結果を明確に示すために、明示的型指定を使用します。 (`var` について詳しくは、「[暗黙的に型指定されたローカル変数](../../classes-and-structs/implicitly-typed-local-variables.md)」をご覧ください。)  
  
     なお、クエリの範囲変数 (`student`) は、ソース内の各 `Student` への参照として機能し、各オブジェクトのメンバー アクセスを提供します。  
  
 [!code-csharp[CsLINQGettingStarted#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#12)]  
  
## <a name="execute-the-query"></a>クエリの実行  
  
#### <a name="to-execute-the-query"></a>クエリを実行するには  
  
1. クエリを実行する `foreach` ループを記述します。 コードについては、以下の点に注意してください。  
  
    - 返されたシーケンス内の各要素は、`foreach` ループ内の反復変数を通じてアクセスされます。  
  
    - この変数の型は `Student` で、クエリ変数の型は互換性があります (`IEnumerable<Student>`)。  
  
2. このコードを追加したら、アプリケーションをビルドして実行し、 **[コンソール]** ウィンドウで結果を表示します。  
  
 [!code-csharp[CsLINQGettingStarted#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#13)]  
  
#### <a name="to-add-another-filter-condition"></a>別のフィルター条件を追加するには  
  
1. クエリを改良するために、`where` 句で複数のブール条件を結合することができます。 次のコードでは、最初の点数が 90 より高く、最後の点数が 80 未満の生徒が返されるようにするための条件を追加しています。 `where` 句は次のコードのようになります。  
  
    ```csharp
    where student.Scores[0] > 90 && student.Scores[3] < 80  
    ```  
  
     詳細については、「[where 句](../../../language-reference/keywords/where-clause.md)」を参照してください。  
  
## <a name="modify-the-query"></a>クエリの変更  
  
#### <a name="to-order-the-results"></a>結果を並べ替えるには  
  
1. クエリの結果は、一定の基準で順序付けしたほうが見やすくなります。 返されたシーケンスは、ソース要素内のアクセス可能なフィールドによって順序付けすることができます。 たとえば、次の `orderby` 句では、各生徒の名前を基準にして、A から Z へのアルファベット順で結果を並べるようにしています。 次の `orderby` 句をクエリに追加します (`where` ステートメントの直後、`select` ステートメントの前)。  
  
    ```csharp
    orderby student.Last ascending  
    ```  
  
2. `orderby` 句を変更し、最初のテストの点数を基準にして、結果を逆順 (最高点から最低点) で並べるようにします。  
  
    ```csharp
    orderby student.Scores[0] descending  
    ```  
  
3. `WriteLine` 書式文字列を変更して、点数を表示できるようにします。  
  
    ```csharp
    Console.WriteLine("{0}, {1} {2}", student.Last, student.First, student.Scores[0]);  
    ```  
  
     詳細については、「[orderby 句](../../../language-reference/keywords/orderby-clause.md)」を参照してください。  
  
#### <a name="to-group-the-results"></a>結果をグループ化するには  
  
1. グループ化は、クエリ式の強力な機能です。 グループ句を使用したクエリでは、グループのシーケンスが生成され、各グループ自体に、`Key` と、そのグループの全メンバーで構成されたシーケンスが含まれます。 次の新しいクエリでは、生徒の姓の頭文字をキーに使用して、生徒をグループ化しています。  
  
     [!code-csharp[CsLINQGettingStarted#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#14)]  
  
2. クエリの型が変更されたことに注意してください。 `char` 型をキーに持つグループのシーケンスと、`Student` オブジェクトのシーケンスが生成されるようになりました。 クエリの型が変更されたため、次のコードでは `foreach` 実行ループも変更されています。  
  
     [!code-csharp[CsLINQGettingStarted#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#15)]  
  
3. アプリケーションを実行し、 **[コンソール]** ウィンドウで結果を表示します。  
  
     詳細については、「[group 句](../../../language-reference/keywords/group-clause.md)」を参照してください。  
  
#### <a name="to-make-the-variables-implicitly-typed"></a>変数を暗黙的に型指定するには  
  
1. `IEnumerables` の `IGroupings` を明示的にコーディングするのは非常に面倒です。 `foreach` を使用すれば、同じクエリや `var` ループをはるかに効率的に記述できます。 `var` キーワードは、オブジェクトの型を変更しません。型を推論するようにコンパイラに指示するだけです。 `studentQuery` の型と反復変数 `group` を `var` に変更し、クエリを再実行します。 内部の `foreach` ループで、反復変数の型は `Student` のままになっており、クエリは以前と同様に機能します。 反復変数 `s` を `var` に変更し、クエリを再実行します。 まったく同じ結果が得られます。  
  
     [!code-csharp[CsLINQGettingStarted#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#16)]  
  
     [var](../../../language-reference/keywords/var.md) について詳しくは、「[暗黙的に型指定されたローカル変数](../../classes-and-structs/implicitly-typed-local-variables.md)」をご覧ください。  
  
#### <a name="to-order-the-groups-by-their-key-value"></a>グループをキー値で順序付けるには  
  
1. 前のクエリを実行すると、グループはアルファベット順になりません。 これを変えるには、`orderby` 句の後に `group` 句を記述する必要があります。 しかし `orderby` 句を使用するには、まず、`group` 句によって作成されたグループへの参照として機能する識別子が必要になります。 この識別子は、次のように `into` キーワード使用して記述します。  
  
     [!code-csharp[csLINQGettingStarted#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#17)]  
  
     このクエリを実行すると、グループがアルファベット順に並べ替えられます。  
  
#### <a name="to-introduce-an-identifier-by-using-let"></a>let を使用して識別子を導入するには  
  
1. `let` キーワードを使用すると、任意の式の結果の識別子をクエリ式に導入できます。 この識別子は、次の例のように便利に使用できます。式の結果を格納することで、何度も計算を行う必要がなくなり、パフォーマンスの向上につながります。  
  
     [!code-csharp[csLINQGettingStarted#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#18)]  
  
     詳細については、「[let 句](../../../language-reference/keywords/let-clause.md)」を参照してください。  
  
#### <a name="to-use-method-syntax-in-a-query-expression"></a>クエリ式でメソッド構文を使用するには  
  
1. 「[LINQ でのクエリ構文とメソッド構文](./query-syntax-and-method-syntax-in-linq.md)」で説明したように、一部のクエリ操作は、メソッド構文を使用することでのみ表現できます。 次のコードは、ソース シーケンス内の各 `Student` の合計点数を計算し、そのクエリの結果に対して `Average()` メソッドを呼び出して、クラスの平均点数を計算します。
  
     [!code-csharp[csLINQGettingStarted#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#19)]  
  
#### <a name="to-transform-or-project-in-the-select-clause"></a>select 句で変換またはプロジェクトを実行するには  
  
1. クエリでは、ソース シーケンス内の要素とは異なる要素のシーケンスを生成することがよくあります。 前のクエリと実行ループを削除またはコメント アウトして、次のコードに置き換えます。 クエリが (`Students` ではなく) 文字列のシーケンスを返すことに注意してください。このことは、`foreach` ループに反映されます。  
  
     [!code-csharp[csLINQGettingStarted#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#20)]  
  
2. このチュートリアルの前のコードでは、クラスの平均点が約 334 と示されました。 合計点がクラス平均よりも高い `Students` のシーケンスを (生徒の `Student ID` と共に) 生成するには、`select` ステートメントで匿名型を使用できます。  
  
     [!code-csharp[csLINQGettingStarted#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#21)]  
  
## <a name="next-steps"></a>次の手順  
 C# でのクエリ操作の基本が理解できたら、興味がある種類の LINQ プロバイダーについて、ドキュメントやサンプルを読んでみましょう。  
  
 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)  
  
 [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)  
  
 [LINQ to XML (C#)](./linq-to-xml-overview.md)  
  
 [LINQ to Objects (C#)](./linq-to-objects.md)  
  
## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ) (C#)](./index.md)
- [LINQ クエリ式](../../../linq/index.md)
