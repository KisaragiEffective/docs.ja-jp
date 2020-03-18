---
title: ファイルとディレクトリの操作
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], reading text
- reading files [Visual Basic], text
- I/O [Visual Basic], walkthroughs
- text, writing to files
- text, reading from files
- reading text from files [Visual Basic], walkthroughs
- Visual Basic code, file access
- files [Visual Basic], writing text
- I/O [Visual Basic], writing text to files
- file access, walkthroughs
- writing to files [Visual Basic], walkthroughs
- I/O [Visual Basic], reading text from files
ms.assetid: cae77565-9f78-4e46-8e42-eb2f9f8e1ffd
ms.openlocfilehash: 83dc6ce0d29c1c368c36b51fc84ecad34d72e01f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74333804"
---
# <a name="walkthrough-manipulating-files-and-directories-in-visual-basic"></a>チュートリアル : Visual Basic によるファイルとディレクトリの操作

このチュートリアルでは、Visual Basic でのファイル I/O の基礎について概説します。 具体的には、ディレクトリ内のテキスト ファイルをリストして調査する小さなアプリケーションを作成する方法について説明します。 このアプリケーションは、選択された各テキスト ファイルについて、ファイルの属性とコンテンツの最初の行を取得します。 ログ ファイルに情報を書き込むオプションもあります。  
  
 このチュートリアルでは、Visual Basic で利用可能な、`My.Computer.FileSystem Object` のメンバーを使用します。 詳細については、「<xref:Microsoft.VisualBasic.FileIO.FileSystem>」を参照してください。 チュートリアルの最後では、<xref:System.IO> 名前空間のクラスを使用する同等の例を示します。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-the-project"></a>プロジェクトを作成するには  
  
1. **[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。  
  
     **[新しいプロジェクト]** ダイアログ ボックスが表示されます。  
  
2. **[インストールされたテンプレート]** ペインで、 **[Visual Basic]** を展開し、 **[Windows]** をクリックします。 中央の **[テンプレート]** ペインで、 **[Windows フォーム アプリケーション]** をクリックします。  
  
3. **[名前]** ボックスに「`FileExplorer`」と入力してプロジェクト名を設定し、 **[OK]** をクリックします。  
  
     Visual Studio の**ソリューション エクスプローラー**にプロジェクトが追加され、Windows フォーム デザイナーが開きます。  
  
4. 次の表にあるコントロールをフォームに追加し、それらのプロパティに対応する値を設定します。  
  
    |Control|property|値|  
    |-------------|--------------|-----------|  
    |**ListBox**|**名前**|`filesListBox`|  
    |**Button**|**名前**<br /><br /> **テキスト**|`browseButton`<br /><br /> **参照**|  
    |**Button**|**名前**<br /><br /> **テキスト**|`examineButton`<br /><br /> **調査**|  
    |**CheckBox**|**名前**<br /><br /> **テキスト**|`saveCheckBox`<br /><br /> **結果の保存**|  
    |**FolderBrowserDialog**|**名前**|`FolderBrowserDialog1`|  
  
### <a name="to-select-a-folder-and-list-files-in-a-folder"></a>フォルダーを選択し、フォルダー内のファイルをリストするには  
  
1. フォーム上のコントロールをダブルクリックして、`Click` の `browseButton` イベント ハンドラーを作成します。 コード エディターが開きます。  
  
2. `Click` イベント ハンドラーに次のコードを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#103)]  
  
     `FolderBrowserDialog1.ShowDialog` 呼び出しにより、 **[フォルダーの参照]** ダイアログ ボックスが開きます。 ユーザーが **[OK]** をクリックすると、次の手順で追加する <xref:System.Windows.Forms.FolderBrowserDialog.SelectedPath%2A> メソッドに `ListFiles` プロパティが引数として渡されます。  
  
3. 次の `ListFiles` メソッドを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#104](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#104)]  
  
     このコードでは、まず最初に **ListBox** をクリアします。  
  
     次に、<xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A> メソッドによって、ディレクトリ内の各ファイルを表す文字列のコレクションを取得します。 `GetFiles` メソッドは、検索パターンの引数を受け取り、特定のパターンに一致するファイルを取得します。 この例では、拡張子 .txt が付いているファイルのみが返されます。  
  
     その後、`GetFiles` メソッドによって返された文字列が、**ListBox** に追加されます。  
  
4. アプリケーションを実行します。 **[参照]** ボタンをクリックします。 **[フォルダーの参照]** ダイアログ ボックスで、.txt ファイルが含まれているフォルダーを参照し、そのフォルダーを選択して **[OK]** をクリックします。  
  
     `ListBox` に、選択したフォルダー内のファイルが追加されます。  
  
5. アプリケーションの実行を停止します。  
  
### <a name="to-obtain-attributes-of-a-file-and-content-from-a-text-file"></a>テキスト ファイルからファイルの属性とコンテンツを取得するには  
  
1. フォーム上のコントロールをダブルクリックして、`Click` の `examineButton` イベント ハンドラーを作成します。  
  
2. `Click` イベント ハンドラーに次のコードを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#105](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#105)]  
  
     このコードは、`ListBox` で選択された項目を検証します。 その後、`ListBox` からファイル パスのエントリを取得します。 <xref:Microsoft.VisualBasic.FileIO.FileSystem.FileExists%2A> メソッドを使用して、ファイルがまだ存在するかどうかを確認します。  
  
     ファイル パスは引数として `GetTextForOutput` メソッドに送られます (このメソッドは次の手順で追加します)。 このメソッドは、ファイル情報を含んだ文字列を返します。 ファイル情報は、**MessageBox** に表示されます。  
  
3. 次の `GetTextForOutput` メソッドを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#107](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#107)]  
  
     このコードでは、<xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFileInfo%2A> メソッドを使用してファイル パラメーターを取得します。 ファイル パラメーターは、<xref:System.Text.StringBuilder> に追加されます。  
  
     <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileReader%2A> メソッドは、ファイルの内容を <xref:System.IO.StreamReader> に読み込みます。 コンテンツの最初の行が `StreamReader` から取得され、`StringBuilder` に追加されます。  
  
4. アプリケーションを実行します。 **[参照]** をクリックし、.txt ファイルが含まれているフォルダーを参照します。 **[OK]** をクリックします。  
  
     `ListBox` でファイルを選択し、 **[調査]** をクリックします。 `MessageBox` にファイル情報が表示されます。  
  
5. アプリケーションの実行を停止します。  
  
### <a name="to-add-a-log-entry"></a>ログ エントリを追加するには  
  
1. `examineButton_Click` イベント ハンドラーの末尾に、次のコードを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#106](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#106)]  
  
     このコードは、ログ ファイルのパスを設定して、選択したファイルと同じディレクトリにログ ファイルを配置します。 ログ エントリのテキストは現在の日付と時刻に設定され、その後にファイル情報が記録されます。  
  
     <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> 引数を `append` に設定した `True` メソッドを使用して、ログ エントリを作成します。  
  
2. アプリケーションを実行します。 テキスト ファイルを参照し、`ListBox` でファイルを選択して、 **[結果の保存]** チェック ボックスをオンにした後、 **[調査]** をクリックします。 ログ エントリが `log.txt` ファイルに書き込まれていることを確認します。  
  
3. アプリケーションの実行を停止します。  
  
### <a name="to-use-the-current-directory"></a>現在のディレクトリを使用するには  
  
1. フォームをダブルクリックして、`Form1_Load` のイベント ハンドラーを作成します。  
  
2. イベント ハンドラーに次のコードを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#102)]  
  
     このコードは、フォルダー ブラウザーの既定のディレクトリを現在のディレクトリに設定します。  
  
3. アプリケーションを実行します。 **[参照]** を最初にクリックすると、 **[フォルダーの参照]** ダイアログ ボックスが開き、現在のディレクトリが表示されます。  
  
4. アプリケーションの実行を停止します。  
  
### <a name="to-selectively-enable-controls"></a>コントロールを選択的に有効化するには  
  
1. 次の `SetEnabled` メソッドを追加します。  
  
     [!code-vb[VbVbcnMyFileSystem#108](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#108)]  
  
     `SetEnabled` メソッドは、`ListBox` で項目が選択されているかどうかに応じて、コントロールを有効または無効にします。  
  
2. フォーム上の `SelectedIndexChanged` コントロールをダブルクリックして、`filesListBox` の `ListBox` イベント ハンドラーを作成します。  
  
3. 新しい `SetEnabled` イベント ハンドラーで、`filesListBox_SelectedIndexChanged` に対する呼び出しを追加します。  
  
4. `SetEnabled` イベント ハンドラーの末尾に、`browseButton_Click` に対する呼び出しを追加します。  
  
5. `SetEnabled` イベント ハンドラーの末尾に、`Form1_Load` に対する呼び出しを追加します。  
  
6. アプリケーションを実行します。 **で項目が選択されていない場合は、** [結果の保存 **] チェック ボックスと** [調査]`ListBox` ボタンが無効になります。  
  
## <a name="full-example-using-mycomputerfilesystem"></a>My.Computer.FileSystem を使用した完全な例  

 完全な例を次に示します。  
  
 [!code-vb[VbVbcnMyFileSystem#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#101)]  
  
## <a name="full-example-using-systemio"></a>System.IO を使用した完全な例  

 次に示す同等の例では、<xref:System.IO> オブジェクトではなく、`My.Computer.FileSystem` 名前空間のクラスを使用しています。  
  
 [!code-vb[VbVbcnMyFileSystem#111](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class3.vb#111)]  
  
## <a name="see-also"></a>参照

- <xref:System.IO>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CurrentDirectory%2A>
- [チュートリアル : .NET Framework のメソッドによるファイル操作](../../../../visual-basic/developing-apps/programming/drives-directories-files/walkthrough-manipulating-files-by-using-net-framework-methods.md)
