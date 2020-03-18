---
title: Office 相互運用オブジェクトにアクセスする方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- optional parameters [C#], Office programming
- named and optional arguments [C#], Office programming
- dynamic [C#], Office programming
- optional arguments [C#], Office programming
- named arguments [C#], Office programming
- Office programming [C#]
ms.assetid: 041b25c2-3512-4e0f-a4ea-ceb2999e4d5e
ms.openlocfilehash: b5d2da011ec6318c8b07f1eb4d383a4d56488239
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75700836"
---
# <a name="how-to-access-office-interop-objects-c-programming-guide"></a>Office 相互運用オブジェクトにアクセスする方法 (C# プログラミング ガイド)

C# には、Office API オブジェクトへのアクセスを容易にする機能があります。 新機能は、名前付き引数と省略可能な引数、`dynamic` と呼ばれる新しい型、値パラメーターの場合と同様に COM メソッドの参照パラメーターに引数を渡す機能などです。

このトピックでは、新機能を使用して、Microsoft Office Excel ワークシートを作成および表示するコードを記述します。 その後、Excel ワークシートにリンクされているアイコンを含む Office Word 文書を追加するコードを記述します。

このチュートリアルを実行するには、Microsoft Office Excel 2007 と Microsoft Office Word 2007 またはそれ以降のバージョンがコンピューターにインストールされている必要があります。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-a-new-console-application"></a>新しいコンソール アプリケーションを作成するには

1. Visual Studio を起動します。

2. **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。 **[新しいプロジェクト]** ダイアログ ボックスが表示されます。

3. **[インストールされたテンプレート]** ペインで、 **[Visual C#]** を展開し、 **[Windows]** をクリックします。

4. **[新しいプロジェクト]** ダイアログ ボックスの上部で、 **.NET Framework 4** (またはそれ以降のバージョン) がターゲット フレームワークとして選択されていることを確認します。

5. **[テンプレート]** ペインの **[コンソール アプリケーション]** をクリックします。

6. **[名前]** フィールドに、プロジェクトの名前を入力します。

7. **[OK]** をクリックします。

     **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。

## <a name="to-add-references"></a>参照を追加するには

1. **ソリューション エクスプローラー**で、プロジェクトの名前を右クリックし、 **[参照の追加]** をクリックします。 **[参照の追加]** ダイアログ ボックスが表示されます。

2. **[アセンブリ]** ページの **[コンポーネント名]** 一覧で **[Microsoft.Office.Interop.Word]** を選択し、Ctrl キーを押しながら **[Microsoft.Office.Interop.Excel]** を選択します。  アセンブリが表示されない場合は、アセンブリがインストールされ表示されることを確認する必要があります。 「[方法:Office のプライマリ相互運用機能アセンブリをインストールする](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies)」を参照してください。

3. **[OK]** をクリックします。

## <a name="to-add-necessary-using-directives"></a>ディレクティブを使用して必要なものを追加するには

1. **ソリューション エクスプローラー**で、*Program.cs* ファイルを右クリックし、 **[コードの表示]** をクリックします。

2. 次の `using` ディレクティブをコード ファイルの先頭に追加します。

     [!code-csharp[csProgGuideOfficeHowTo#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#1)]

## <a name="to-create-a-list-of-bank-accounts"></a>銀行口座の一覧を作成するには

1. 次のクラス定義を **Program.cs** の `Program` クラスの下に貼り付けます。

     [!code-csharp[csProgGuideOfficeHowTo#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#2)]

2. 次のコードを `Main` メソッドに追加して、2 つの口座を含む `bankAccounts` 一覧を作成します。

     [!code-csharp[csProgGuideOfficeHowTo#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#3)]

## <a name="to-declare-a-method-that-exports-account-information-to-excel"></a>口座情報を Excel にエクスポートするメソッドを宣言するには

1. 次のメソッドを `Program` クラスに追加して、Excel ワークシートを設定します。

     <xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A> メソッドには、特定のテンプレートを指定する省略可能なパラメーターがあります。 C# 4 の新機能であるオプションのパラメーターでは、パラメーターの既定値を使用する場合は、そのパラメーターの引数を省略することができます。 次のコードで引数が渡されないため、`Add` は、既定のテンプレートを使用して、新しいブックを作成します。 以前のバージョンの C# では、同等のステートメントには、プレースホルダーの引数 `ExcelApp.Workbooks.Add(Type.Missing)` が必要です。

     [!code-csharp[csProgGuideOfficeHowTo#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#4)]

2. 次のコードを `DisplayInExcel` の末尾に追加します。 このコードでは、ワークシートの最初の行の最初の 2 つの列に値が挿入されます。

     [!code-csharp[csProgGuideOfficeHowTo#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#5)]

3. 次のコードを `DisplayInExcel` の末尾に追加します。 `foreach` ループでは、ワークシートの連続した行の最初の 2 つの列に口座の一覧の情報が配置されます。

     [!code-csharp[csProgGuideOfficeHowTo#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#7)]

4. 次のコードを `DisplayInExcel` の末尾に追加して、コンテンツに合わせて列の幅を調整します。

     [!code-csharp[csProgGuideOfficeHowTo#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#13)]

     C# の以前のバージョンでは、`ExcelApp.Columns[1]` が `Object` を返し、`AutoFit` が Excel <xref:Microsoft.Office.Interop.Excel.Range> メソッドであるため、これらの操作の明示的なキャストが必要です。 次の行にキャストを示します。

     [!code-csharp[csProgGuideOfficeHowTo#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#14)]

     C# 4 以降のバージョンでは、アセンブリが [-link](../../language-reference/compiler-options/link-compiler-option.md) コンパイラ オプションで参照される場合、または同等に、Excel の **[相互運用機能型の埋め込み]** プロパティが true に設定されている場合は、返される `Object` が `dynamic` に自動的に変換されます。 このプロパティの既定値は true です。

## <a name="to-run-the-project"></a>プロジェクトを実行するには

1. `Main` の末尾に次の行を追加します。

     [!code-csharp[csProgGuideOfficeHowTo#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#8)]

2. Ctrl キーを押しながら、F5 キーを押します。

     2 つの口座からのデータを含む Excel ワークシートが表示されます。

## <a name="to-add-a-word-document"></a>Word 文書を追加するには

1. C# 4 およびそれ以降のバージョンで Office プログラミングを強化するその他の方法を説明するために、次のコードでは、Word アプリケーションを開き、Excel ワークシートにリンクするアイコンを作成します。

     この手順の後半で提供されている `CreateIconInWordDoc` メソッドを `Program` クラスに貼り付けます。 <xref:Microsoft.Office.Interop.Word.Documents.Add%2A> メソッドと <xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A> メソッドの呼び出しに伴う複雑さが、`CreateIconInWordDoc` の名前付き引数と省略可能な引数によって軽減されます。 これらの呼び出しによって、C# 4 で導入された、参照パラメーターを持つ COM メソッドの呼び出しを簡略化する 2 つの他の新しい機能が組み込まれます。 第一に、値パラメーターの場合と同様に参照パラメーターに引数を渡すことができます。 つまり、参照パラメーターごとに変数を作成することなく値を直接渡すことができます。 コンパイラは引数の値を保持する一時変数を生成し、呼び出しから戻るときに変数を破棄します。 第二に、引数リスト内の `ref` キーワードを省略できます。

     `Add` メソッドには 4 つの参照パラメーターがあり、これらはすべてオプションです。 C# 4.0 およびそれ以降のバージョンでは、既定値を使用する場合は、任意またはすべてのパラメーターの引数を省略できます。 C# 3.0 およびそれ以前のバージョンでは、各パラメーターの引数を指定する必要があり、そのパラメーターが参照パラメーターであるため、引数は変数である必要があります。

     `PasteSpecial` メソッドは、クリップボードの内容を挿入します。 このメソッドには 7 つの参照パラメーターがあり、これらはすべてオプションです。 次のコードでは、クリップボードの内容のソースへのリンクを作成する `Link` とリンクをアイコンとして表示する `DisplayAsIcon` の 2 つの参照パラメーターの引数を指定します。 C# 4.0 およびそれ以降のバージョンでは、これら 2 つに名前付き引数を使用して、その他を省略できます。 これらは参照パラメーターですが、`ref` キーワードを使用したり、引数として渡す変数を作成したりする必要はありません。 値は直接渡すことができます。 C# 3.0 およびそれ以前のバージョンでは、各参照パラメーターに変数引数を渡す必要があります。

     [!code-csharp[csProgGuideOfficeHowTo#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#9)]

     C# 3.0 およびそれ以前のバージョンの言語では、次のより複雑なコードが必要です。

     [!code-csharp[csProgGuideOfficeHowTo#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#10)]

2. 次のステートメントを `Main` の末尾に追加します。

     [!code-csharp[csProgGuideOfficeHowTo#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#11)]

3. 次のステートメントを `DisplayInExcel` の末尾に追加します。 `Copy` メソッドはクリップボードにワークシートを追加します。

     [!code-csharp[csProgGuideOfficeHowTo#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#12)]

4. Ctrl キーを押しながら、F5 キーを押します。

     アイコンを含む Word 文書が表示されます。 ワークシートを前面に表示するアイコンをダブルクリックします。

## <a name="to-set-the-embed-interop-types-property"></a>[相互運用機能型の埋め込み] プロパティを設定するには

1. 実行時に、プライマリ相互運用機能アセンブリ (PIA) を必要としない COM 型を呼び出すときに、追加の拡張が可能です。 PIA への依存関係を削除することによって、バージョンに依存しない、より簡単な展開が実現されます。 PIA を使用しないプログラミングのメリットの詳細については、「[チュートリアル: マネージド アセンブリからの型の埋め込み](../../../standard/assembly/embed-types-visual-studio.md)」をご覧ください。

     また、`dynamic` ではなく `Object` 型を使用して、COM メソッドに必要とされ、COM メソッドによって返される型を簡単に表現できるため、プログラミングがより簡単になります。 型が `dynamic` の変数は、明示的なキャストが不要になる実行時まで評価されません。 詳細については、「[dynamic 型の使用](../types/using-type-dynamic.md)」を参照してください。

     C# 4 の既定の動作では、PIA を使用せずに型情報が埋め込まれます。 この既定のため、前の例のいくつかは、明示的なキャストが必要ないために簡素化されます。 たとえば、`worksheet` での `DisplayInExcel` の宣言は、`Excel._Worksheet workSheet = excelApp.ActiveSheet` ではなく `Excel._Worksheet workSheet = (Excel.Worksheet)excelApp.ActiveSheet` と記述されます。 同じメソッドの `AutoFit` への呼び出しでも、既定値を使用せずに明示的なキャストが必要になります。これは、`ExcelApp.Columns[1]` が `Object` を返し、`AutoFit` が Excel のメソッドであるためです。 次のコードはキャストを示しています。

     [!code-csharp[csProgGuideOfficeHowTo#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#14)]

2. 既定値を変更し、型情報を埋め込むのではなく PIA を使用するには、**ソリューション エクスプ ローラー**で **[参照設定]** ノードを展開し、 **[Microsoft.Office.Interop.Excel]** または **[Microsoft.Office.Interop.Word]** を選択します。

3. **[プロパティ]** ウィンドウが表示されない場合は、**F4** キーを押します。

4. プロパティの一覧で **[相互運用機能型の埋め込み]** を見つけて、値を **[False]** に変更します。 同様に、コマンド プロンプトで [-link](../../language-reference/compiler-options/link-compiler-option.md) の代わりに [-reference](../../language-reference/compiler-options/reference-compiler-option.md) コンパイラ オプションを使用してコンパイルすることができます。

## <a name="to-add-additional-formatting-to-the-table"></a>テーブルに追加の書式設定を追加するには

1. `AutoFit` の `DisplayInExcel` への 2 つの呼び出しを次のステートメントに置き換えます。

     [!code-csharp[csProgGuideOfficeHowTo#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#15)]

     <xref:Microsoft.Office.Interop.Excel.Range.AutoFormat%2A> メソッドには、7 つの値パラメーターがあり、これらはすべて省略可能です。 名前付き引数と省略可能な引数を使用すると、一部またはすべてのパラメーターに引数を指定することができます。引数を指定しないこともできます。 前のステートメントでは、1 つのパラメーター `Format` にのみ引数が指定されています。 `Format` はパラメーター リストの最初のパラメーターであるため、パラメーター名を指定する必要はありません。 ただし、次のコードに示すように、パラメーター名が含まれている場合、ステートメントがわかりやすい場合があります。

     [!code-csharp[csProgGuideOfficeHowTo#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#16)]

2. Ctrl + F5 キーを押して結果を表示します。 その他の形式は、<xref:Microsoft.Office.Interop.Excel.XlRangeAutoFormat> 列挙型のページに掲載されています。

3. 手順 1 のステートメントと、C# 3.0 およびそれ以前のバージョンで必要な引数が示されている次のコードを比較します。

     [!code-csharp[csProgGuideOfficeHowTo#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/program.cs#17)]

## <a name="example"></a>例

コード例全体を次に示します。

[!code-csharp[csProgGuideOfficeHowTo#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideofficehowto/cs/walkthrough.cs#18)]

## <a name="see-also"></a>関連項目

- <xref:System.Type.Missing?displayProperty=nameWithType>
- [dynamic](../../language-reference/builtin-types/reference-types.md)
- [dynamic 型の使用](../types/using-type-dynamic.md)
- [名前付き引数と省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)
- [Office プログラミングで名前付き引数と省略可能な引数を使用する方法](../classes-and-structs/how-to-use-named-and-optional-arguments-in-office-programming.md)
