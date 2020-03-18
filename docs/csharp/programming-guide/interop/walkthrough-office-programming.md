---
title: 'チュートリアル: Office のプログラミング (C# および Visual Basic)'
ms.date: 07/20/2015
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Office, programming in Visual Basic and C#
- Office programming [C#]
- Office programming [Visual Basic]
ms.assetid: 519cff31-f80b-4f0e-a56b-26358d0f8c51
ms.openlocfilehash: 6c27442cb5c0c4172f503c945849e47560c2b33d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635354"
---
# <a name="walkthrough-office-programming-c-and-visual-basic"></a>チュートリアル: Office のプログラミング (C# および Visual Basic)

Visual Studio には、Microsoft Office のプログラミングを改善する C# および Visual Basic の新機能が導入されています。 便利な C# の機能には、名前付き引数、省略可能な引数、型 `dynamic` の戻り値があります。 COM プログラミングでは、`ref` キーワードを省略し、インデックス付きプロパティにアクセスできます。 Visual Basic の機能には、自動実装プロパティ、ラムダ式内のステートメント、コレクション初期化子などがあります。

両方の言語で、ユーザーのコンピューターにプライマリ相互運用機能アセンブリ (PIA) を配置せずに COM コンポーネントとやり取りするアセンブリを配置できる型情報を埋め込むことができます。 詳細については、「[チュートリアル: マネージド アセンブリからの型の埋め込み](../../../standard/assembly/embed-types-visual-studio.md)」を参照してください。

このチュートリアルでは、Office プログラミングのコンテキストで機能を示しますが、これらの機能の多くは一般的なプログラミングにも便利です。 このチュートリアルでは、Excel ブックを作成する Excel アドイン アプリケーションを使用します。 次に、ブックへのリンクを含む Word 文書を作成します。 最後に、PIA 依存関係の有効/無効を切り替える方法を確認します。

## <a name="prerequisites"></a>前提条件

このチュートリアルを実行するには、Microsoft Office Excel と Microsoft Office Word をコンピューターにインストールしておく必要があります。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

### <a name="to-set-up-an-excel-add-in-application"></a>Excel アドイン アプリケーションをセットアップするには

1. Visual Studio を起動します。

2. **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。

3. **[インストールされたテンプレート]** ペインで、 **[Visual Basic]** または **[Visual C#]** を展開し、 **[Office]** を展開して、Office 製品のバージョン年度をクリックします。

4. **[テンプレート]** ペインで、 **[Excel]\< バージョン、[アドイン]** の順にクリックします。

5. **[テンプレート]** ペインの上部で、 **[ターゲット フレームワーク]** ボックスに **[.NET Framework 4]** またはそれ以降のバージョンが表示されていることを確認します。

6. 必要に応じて、 **[名前]** ボックスにプロジェクトの名前を入力します。

7. **[OK]** をクリックします。

8. **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。

### <a name="to-add-references"></a>参照を追加するには

1. **ソリューション エクスプローラー**で、プロジェクトの名前を右クリックし、 **[参照の追加]** をクリックします。 **[参照の追加]** ダイアログ ボックスが表示されます。

2. **[アセンブリ]** タブの **[コンポーネント名]** 一覧で、`<version>.0.0.0`Microsoft.Office.Interop.Excel[、バージョン ](https://en.wikipedia.org/wiki/Microsoft_Office#Versions) (Office 製品番号のキーについては、**Microsoft バージョン**に関するページを参照してください) を選択し、Ctrl キーを押しながら **Microsoft.Office.Interop.Word**、`version <version>.0.0.0` を選択します。 アセンブリが表示されない場合は、アセンブリがインストールされ、表示されることの確認が必要になることがあります (「[方法: Office のプライマリ相互運用機能アセンブリをインストールする](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies)」を参照)。

3. **[OK]** をクリックします。

### <a name="to-add-necessary-imports-statements-or-using-directives"></a>必要な Imports ステートメントまたはディレクティブの使用を追加するには

1. **ソリューション エクスプローラー**で、 **[ThisAddIn.vb]** または **[ThisAddIn.cs]** ファイルを右クリックし、 **[コードの表示]** をクリックします。

2. 次の `Imports` ステートメント (Visual Basic) または `using` ディレクティブ (C#) が含まれていない場合は、コード ファイルの先頭に追加します。

     [!code-csharp[csOfficeWalkthrough#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#1)]

     [!code-vb[csOfficeWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#1)]

### <a name="to-create-a-list-of-bank-accounts"></a>銀行口座の一覧を作成するには

1. **ソリューション エクスプローラー**で、プロジェクト名を右クリックし、 **[追加]** をクリックしてから **[クラス]** をクリックします。 Visual Basic を使用している場合は Account.vb、C# を使用している場合は Account.cs という名前をクラスに付けます。 **[追加]** をクリックします。

2. `Account` クラスの定義を次のコードに置き換えます。 このクラス定義では、*自動実装プロパティ*が使用されます。 詳細については、「[自動実装プロパティ](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)」を参照してください。

     [!code-csharp[csOfficeWalkthrough#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/account.cs#2)]

     [!code-vb[csOfficeWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/account.vb#2)]

3. 2 つの口座を含む `bankAccounts` 一覧を作成するには、次のコードを追加する、`ThisAddIn_Startup`ThisAddIn.vb*または*ThisAddIn.cs*の* メソッドに追加します。 一覧の宣言では、*コレクション初期化子*が使用されます。 詳細については、「[コレクション初期化子](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)」を参照してください。

     [!code-csharp[csOfficeWalkthrough#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#3)]

     [!code-vb[csOfficeWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#3)]

### <a name="to-export-data-to-excel"></a>データを Excel にエクスポートするには

1. 同じファイル内で、次のメソッドを `ThisAddIn` クラスに追加します。 このメソッドは、Excel ブックを設定し、データを Excel ブックにエクスポートします。

     [!code-csharp[csOfficeWalkthrough#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#4)]

     [!code-vb[csOfficeWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#4)]

     C# の 2 つの新しい機能は、このメソッドで使用されます。 これら両方の機能は、Visual Basic で既に存在します。

    - [Add](<xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A>) メソッドには、特定のテンプレートを指定する*省略可能なパラメーター*があります。 C# 4 の新機能であるオプションのパラメーターでは、パラメーターの既定値を使用する場合は、そのパラメーターの引数を省略することができます。 前の例では引数が渡されないため、`Add` は、既定のテンプレートを使用して、新しいブックを作成します。 以前のバージョンの C# では、同等のステートメントには、プレースホルダーの引数 `excelApp.Workbooks.Add(Type.Missing)` が必要です。

         詳細については、「[名前付き引数と省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)」を参照してください。

    - `Range`Range`Offset` オブジェクトの [ および ](<xref:Microsoft.Office.Interop.Excel.Range>) プロパティでは*インデックス付きプロパティ*機能を使用します。 この機能では、次の一般的な C# 構文を使用して COM 型からこれらのプロパティを使用することができます。 また、インデックス付きプロパティを使用すると、`Value` プロパティを使用せずに、`Range` オブジェクトの `Value2` プロパティを使用できます。 `Value` プロパティはインデックス付きですが、インデックスはオプションです。 次の例では、省略可能な引数とインデックス付きプロパティは連携しています。

         [!code-csharp[csOfficeWalkthrough#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#5)]

         以前のバージョンの言語では、次の特殊な構文が必要です。

         [!code-csharp[csOfficeWalkthrough#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#6)]

         独自のインデックス付きプロパティを作成することはできません。 この機能では、既存のインデックス付きプロパティの使用のみがサポートされます。

         詳細については、「[COM 相互運用機能を使用したプログラミングでインデックス付きプロパティを使用する方法](./how-to-use-indexed-properties-in-com-interop-rogramming.md)」を参照してください。

2. 次のコードを `DisplayInExcel` の末尾に追加して、コンテンツに合わせて列の幅を調整します。

     [!code-csharp[csOfficeWalkthrough#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#7)]

     [!code-vb[csOfficeWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#7)]

     これらの追加機能では、C# の別の機能である、`Object`dynamic[ 型がある場合と同様に Office などの COM ホストから返される ](../../language-reference/builtin-types/reference-types.md) 値の処理を示します。 これは、 **[相互運用機能型の埋め込み]** が既定値の `True` に設定されている場合、または同様に、アセンブリが [-link](../../language-reference/compiler-options/link-compiler-option.md) コンパイラ オプションによって参照されている場合に発生します。 `dynamic` 型では既に Visual Basic で使用できる遅延バインディングが可能であり、C# 3.0 以前のバージョンの言語で必要だった明示的なキャストが回避されます。

     たとえば、`excelApp.Columns[1]` は `Object` を返し、`AutoFit` は Excel の [Range](<xref:Microsoft.Office.Interop.Excel.Range>) メソッドであるとします。 `dynamic` がない場合、`excelApp.Columns[1]` のインスタンスとして、`Range` によって返されたオブジェクトをキャストしてから、`AutoFit` メソッドを呼び出す必要があります。

     [!code-csharp[csOfficeWalkthrough#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#8)]

     相互運用機能型の埋め込みの詳細については、このトピックの後半の「PIA 参照を検索するには」および「PIA の依存関係を復元するには」の手順を参照してください。 `dynamic` の詳細については、「[dynamic (C# リファレンス)](../../language-reference/builtin-types/reference-types.md)」または「[dynamic 型の使用 (C# プログラミング ガイド)](../types/using-type-dynamic.md)」を参照してください。

### <a name="to-invoke-displayinexcel"></a>DisplayInExcel を起動するには

1. `ThisAddIn_StartUp` メソッドの末尾に、次のコードを追加します。 `DisplayInExcel` に対する呼び出しには、2 つの引数が含まれています。 最初の引数は、処理する口座の一覧の名前です。 2 番目の引数は、データの処理方法を定義する複数行のラムダ式です。 各口座の `ID` 値と `balance` 値が隣接するセルに表示され、残高が 0 より少ない場合、行が赤で表示されます。 詳しくは、「[ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」をご覧ください。

     [!code-csharp[csOfficeWalkthrough#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#9)]

     [!code-vb[csOfficeWalkthrough#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#9)]

2. プログラムを実行するには、F5 キーを押します。 口座からのデータを含む Excel ワークシートが表示されます。

### <a name="to-add-a-word-document"></a>Word 文書を追加するには

1. `ThisAddIn_StartUp` メソッドの末尾に次のコードを追加して、Excel ブックへのリンクを含む Word 文書を作成します。

     [!code-csharp[csOfficeWalkthrough#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#10)]

     [!code-vb[csOfficeWalkthrough#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#10)]

     このコードは、COM プログラミングで `ref` キーワードを省略する機能、名前付き引数、省略可能な引数など、C# の新機能のいくつかを示します。 Visual Basic でこれらの機能は既に存在します。 [PasteSpecial](<xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A>) メソッドには 7 つのパラメーターがあります。これらはすべて省略可能な参照パラメーターとして定義されます。 名前付き引数と省略可能な引数を使用すると、アクセスするパラメーターを名前で指定し、これらのパラメーターにのみ引数を渡すことができます。 この例では、引数は、クリップボードのブックへのリンクを作成する必要があること (`Link` パラメーター)、およびリンクがアイコンとして Word 文書に表示されること (`DisplayAsIcon` パラメーター) を示すために渡されます。 Visual C# では、これらの引数の `ref` キーワードを省略することもできます。

### <a name="to-run-the-application"></a>アプリケーションを実行するには

1. F5 キーを押してアプリケーションを実行します。 Excel が起動し、`bankAccounts` の 2 つの口座からの情報が格納されたテーブルが表示されます。 Excel テーブルへのリンクを含む Word 文書が表示されます。

### <a name="to-clean-up-the-completed-project"></a>完成したプロジェクトをクリーンアップするには

1. Visual Studio で、 **[ビルド]** メニューの **[ソリューションのクリーン]** をクリックします。 それ以外の場合は、コンピューターで Excel を起動するたびにアドインが実行されます。

### <a name="to-find-the-pia-reference"></a>PIA 参照を検索するには

1. もう一度アプリケーションを実行しますが、 **[ソリューションのクリーン]** はクリックしません。

2. **[開始]** を選択します。 **Microsoft Visual Studio \<バージョン>** を見つけ、開発者コマンド プロンプトを開きます。

3. [Developer Command Prompt for Visual Studio]\(Visual Studio 用開発者コマンド プロンプト\) ウィンドウに「`ildasm`」と入力し、Enter キーを押します。 [IL DASM] ウィンドウが表示されます。

4. [IL DASM] ウィンドウの **[ファイル]** メニューで **[ファイル]**  >  **[開く]** をクリックします。 **[Visual Studio \<バージョン>]** をダブルクリックし、 **[プロジェクト]** をダブルクリックします。 プロジェクトのフォルダーを開き、bin/Debug フォルダーで*プロジェクト名*.dll を見つけます。 *プロジェクト名*.dll をダブルクリックします。 新しいウィンドウに、他のモジュールおよびアセンブリへの参照に加えて、プロジェクトの属性が表示されます。 名前空間 `Microsoft.Office.Interop.Excel` と `Microsoft.Office.Interop.Word` はアセンブリに含まれています。 Visual Studio の既定では、コンパイラは、参照 PIA からアセンブリに必要な型をインポートします。

     詳細については、「[方法 : アセンブリの内容を表示する](../../../standard/assembly/view-contents.md)」を参照してください。

5. **MANIFEST** アイコンをダブルクリックします。 プロジェクトによって参照される項目を含んでいるアセンブリの一覧を含むウィンドウが表示されます。 `Microsoft.Office.Interop.Excel` および `Microsoft.Office.Interop.Word` は一覧に含まれていません。 プロジェクトに必要な型がアセンブリにインポートされているため、PIA への参照は必要ありません。 これにより、配置が容易になります。 PIA がユーザーのコンピューター上に存在している必要がなく、アプリケーションに特定のバージョンの PIA を配置する必要がないので、すべてのバージョンに必要な API が存在している場合は、複数のバージョンの Office を使用するようにアプリケーションを設計できます。

     PIA の配置が不要になったため、以前のバージョンを含む複数のバージョンの Office で動作する高度なシナリオで、アプリケーションを作成できます。 ただし、これは、コードで、使用している Office のバージョンでは利用できない APIを使用していない場合にのみ機能します。 特定の API が以前のバージョンで利用可能かどうかは常に明確ではないため、以前のバージョンの Office の使用はお勧めできません。

    > [!NOTE]
    > Office 2003 より前の Office では、PIA は発行されません。 そのため、Office 2002 以前のバージョンの相互運用機能アセンブリを生成する唯一の方法は、COM 参照のインポートです。

6. マニフェスト ウィンドウとアセンブリ ウィンドウを閉じます。

### <a name="to-restore-the-pia-dependency"></a>PIA の依存関係を復元するには

1. **ソリューション エクスプローラー**で、 **[すべてのファイルを表示]** ボタンをクリックします。 **[参照]** フォルダーを展開し、 **[Microsoft.Office.Interop.Excel]** を選択します。 F4 キーを押して **[プロパティ]** ウィンドウを表示します。

2. **[プロパティ]** ウィンドウで、 **[相互運用機能型の埋め込み]** プロパティを **[True]** から **[False]** に変更します。

3. `Microsoft.Office.Interop.Word` について、この手順の手順 1 と 2 を繰り返します。

4. C# では、`Autofit` メソッドの最後で `DisplayInExcel` への 2 つの呼び出しをコメント化します。

5. プロジェクトが正常に実行することを確認するには、F5 キーを押します。

6. アセンブリ ウィンドウを開くには、前の手順の手順 1 ～ 3 を繰り返します。 `Microsoft.Office.Interop.Word` と `Microsoft.Office.Interop.Excel` は、埋め込まれたアセンブリの一覧には表示されません。

7. **MANIFEST** アイコンをダブルクリックし、参照アセンブリのリストをスクロールします。 `Microsoft.Office.Interop.Word` と `Microsoft.Office.Interop.Excel` の両方が一覧に表示されています。 アプリケーションが Excel と Word の PIA を参照し、 **[相互運用機能型の埋め込み]** プロパティを **[False]** に設定しているため、両方のアセンブリがエンド ユーザーのコンピューター上に存在する必要があります。

8. Visual Studio で、 **[ビルド]** メニューの **[ソリューションのクリーン]** をクリックして、完成したプロジェクトをクリーンアップします。

## <a name="see-also"></a>参照

- [自動実装プロパティ (Visual Basic)](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)
- [自動実装するプロパティ (C#)](../classes-and-structs/auto-implemented-properties.md)
- [コレクション初期化子](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [オブジェクト初期化子とコレクション初期化子](../classes-and-structs/object-and-collection-initializers.md)
- [省略可能なパラメーター](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)
- [位置と名前による引数渡し](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-position-and-by-name.md)
- [名前付き引数と省略可能な引数](../classes-and-structs/named-and-optional-arguments.md)
- [事前バインディングと遅延バインディング](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [dynamic](../../language-reference/builtin-types/reference-types.md)
- [dynamic 型の使用](../types/using-type-dynamic.md)
- [ラムダ式 (Visual Basic)](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [ラムダ式 (C#)](../statements-expressions-operators/lambda-expressions.md)
- [COM 相互運用機能を使用したプログラミングでインデックス付きプロパティを使用する方法](./how-to-use-indexed-properties-in-com-interop-rogramming.md)
- [チュートリアル: Microsoft Office アセンブリからの型情報の埋め込み (Visual Studio)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/ee317478(v%3dvs.120))
- [チュートリアル: マネージド アセンブリからの型の埋め込み](../../../standard/assembly/embed-types-visual-studio.md)
- [チュートリアル: 初めての Excel 用 VSTO アドインの作成](/visualstudio/vsto/walkthrough-creating-your-first-vsto-add-in-for-excel)
- [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md)
- [相互運用性](./index.md)
