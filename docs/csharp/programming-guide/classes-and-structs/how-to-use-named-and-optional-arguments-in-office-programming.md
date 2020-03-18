---
title: Office プログラミングで名前付き引数と省略可能な引数を使用する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- named and optional arguments [C#], Office programming
- optional arguments [C#], Office programming
- named arguments [C#], Office programming
ms.assetid: 65b8a222-bcd8-454c-845f-84adff5a356f
ms.openlocfilehash: 36b5c8b49404606c8240d24953c3677d5612d30e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714871"
---
# <a name="how-to-use-named-and-optional-arguments-in-office-programming-c-programming-guide"></a>Office プログラミングで名前付き引数と省略可能な引数を使用する方法 (C# プログラミング ガイド)

C# 4 で導入された名前付き引数と省略可能な引数を使うと、C# プログラミングの便利さ、柔軟性、読みやすさが向上します。 さらに、Microsoft Office オートメーション API などの COM インターフェイスへのアクセスが大幅に楽になります。

次の例の [ConvertToTable](<xref:Microsoft.Office.Interop.Word.Range.ConvertToTable%2A>) メソッドには、列と行の数、書式設定、罫線、フォント、色など、テーブルの特性を表す 16 個のパラメーターがあります。 ほとんどの場合はこれらすべての特性に具体的な値を指定することはないので、16 個のパラメーターはすべて省略可能です。 しかし、名前付きの省略可能な引数を使わないと、各パラメーターに値またはプレースホルダー値を指定する必要があります。 名前付きの省略可能な引数を使うと、プロジェクトに必要なパラメーターの値だけを指定できます。

以下の手順を行うには、Microsoft Office Word がコンピューターにインストールされている必要があります。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-a-new-console-application"></a>新しいコンソール アプリケーションを作成するには

1. Visual Studio を起動します。

2. **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。

3. **[Templates Categories (テンプレート カテゴリ)]** ウィンドウで、 **[Visual C#]** を展開し、 **[Windows]** をクリックします。

4. **[テンプレート]** ウィンドウの上部で、 **[ターゲット フレームワーク]** ボックスに **[.NET Framework 4]** が表示されていることを確認します。

5. **[テンプレート]** ペインの **[コンソール アプリケーション]** をクリックします。

6. **[名前]** フィールドに、プロジェクトの名前を入力します。

7. **[OK]** をクリックします。

     **ソリューション エクスプローラー**に新しいプロジェクトが表示されます。

## <a name="to-add-a-reference"></a>参照を追加するには

1. **ソリューション エクスプローラー**で、プロジェクトの名前を右クリックし、 **[参照の追加]** をクリックします。 **[参照の追加]** ダイアログ ボックスが表示されます。

2. **[.NET]** ページの **[コンポーネント名]** の一覧で、**Microsoft.Office.Interop.Word** を選びます。

3. **[OK]** をクリックします。

## <a name="to-add-necessary-using-directives"></a>ディレクティブを使用して必要なものを追加するには

1. **ソリューション エクスプローラー**で、*Program.cs* ファイルを右クリックし、 **[コードの表示]** をクリックします。

2. 次の `using` ディレクティブをコード ファイルの先頭に追加します。

     [!code-csharp[csProgGuideNamedAndOptional#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#4)]

## <a name="to-display-text-in-a-word-document"></a>Word 文書にテキストを表示するには

1. `Program`Program.cs*の* クラスに、Word アプリケーションと Word 文書を作成する次のメソッドを追加します。 [Add](<xref:Microsoft.Office.Interop.Word.Documents.Add%2A>) メソッドには、4 つの省略可能なパラメーターがあります。 この例では、それらの既定値を使います。 そのため、呼び出しステートメントに引数は必要ありません。

     [!code-csharp[csProgGuideNamedAndOptional#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#6)]

2. 文書内でテキストを表示する場所と表示するテキストを定義する次のコードを、メソッドの最後に追加します。

     [!code-csharp[csProgGuideNamedAndOptional#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#7)]

## <a name="to-run-the-application"></a>アプリケーションを実行するには

1. 次のステートメントを Main に追加します。

     [!code-csharp[csProgGuideNamedAndOptional#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#8)]

2. <kbd>CTRL</kbd>+<kbd>F5</kbd> を押してプロジェクトを実行します。 指定したテキストを含む Word 文書が表示されます。

## <a name="to-change-the-text-to-a-table"></a>テキストをテーブルに変更するには
  
1. `ConvertToTable` メソッドを使って、テーブル内のテキストを囲みます。 このメソッドには、16 個の省略可能なパラメーターがあります。 次の例に示すように、IntelliSense では省略可能なパラメーターは角かっこで囲まれています。

     ![ConvertToTable メソッドのパラメーターのリスト](./media/how-to-use-named-and-optional-arguments-in-office-programming/convert-table-parameters.png)

     名前付きの省略可能な引数を使うと、変更するパラメーターの値だけを指定できます。 簡単なテーブルを作成するには、`DisplayInWord` メソッドの最後に次のコードを追加します。 この引数は、`range` 内のテキスト文字列のコンマがテーブルのセルを区切ることを指定します。

     [!code-csharp[csProgGuideNamedAndOptional#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#9)]

     以前のバージョンの C# で `ConvertToTable` を呼び出すには、次のコードで示すように、パラメーターごとに参照引数が必要です。
  
     [!code-csharp[csProgGuideNamedAndOptional#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#14)]

2. <kbd>CTRL</kbd>+<kbd>F5</kbd> を押してプロジェクトを実行します。

## <a name="to-experiment-with-other-parameters"></a>他のパラメーターを調べるには

1. テーブルを 1 列 3 行に変更するには、`DisplayInWord` の最後の行を次のステートメントに置き換えてから、<kbd>CTRL</kbd>+<kbd>F5</kbd> キーを押します。  

     [!code-csharp[csProgGuideNamedAndOptional#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#10)]

2. テーブルに対して定義済みの書式を指定するには、`DisplayInWord` の最後の行を次のステートメントに置き換えてから、<kbd>CTRL</kbd>+<kbd>F5</kbd> キーを押します。 書式には、[WdTableFormat](<xref:Microsoft.Office.Interop.Word.WdTableFormat>) 定数のどれでも指定できます。

     [!code-csharp[csProgGuideNamedAndOptional#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#11)]

## <a name="example"></a>例

ここまでの例をすべて含んだコードを次に示します。

 [!code-csharp[csProgGuideNamedAndOptional#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#12)]

## <a name="see-also"></a>参照

- [名前付き引数と省略可能な引数](./named-and-optional-arguments.md)
