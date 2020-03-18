---
title: 'チュートリアル : Windows API の呼び出し'
ms.date: 07/20/2015
helpviewer_keywords:
- DLLs, calling
- Windows API, walkthroughs
- platform invoke, walkthroughs
- API calls [Visual Basic], walkthroughs [Visual Basic]
- Windows API, calling
- walkthroughs [Visual Basic], API calls
- DllImport attribute, calling Windows API
- Declare statement [Visual Basic], declaring DLL functions
ms.assetid: 9280ca96-7a93-47a3-8d01-6d01be0657cb
ms.openlocfilehash: ec6b8ddc8769fadde52aaebd6ad3701183fac77a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74338664"
---
# <a name="walkthrough-calling-windows-apis-visual-basic"></a>チュートリアル: Windows API の呼び出し (Visual Basic)
Windows API は、Windows オペレーティング システムの一部であるダイナミック リンク ライブラリ (Dll) です。 独自の手順を記述するのが難しい場合は、タスクを使用してタスクを実行します。 たとえば、Windows には `FlashWindowEx` という名前の関数が用意されています。これを使用すると、アプリケーションのタイトルバーを明るい網掛けと暗い影の間で交互にすることができます。  
  
 Windows API を使用して、コード内の利点は、数十個は既に書き込まれている便利な関数の使用を待機しているが含まれているため、開発時間を保存することができます。 欠点は、問題が生じた場合、および晙の処理が難しくなって Windows API であることができます。  
  
 Windows API では、相互運用性の特殊なカテゴリを表します。 Windows API では、マネージ コードを使用しないでください、タイプ ライブラリ、および Visual Studio で使用されるものとは異なるデータ型を使用して、組み込みはありません。 これらの違いにより、Windows Api は COM オブジェクトではないため、Windows Api との相互運用性と、プラットフォーム呼び出しまたは PInvoke を使用して .NET Framework が実行されます。 プラットフォーム呼び出しは、マネージコードが Dll で実装されているアンマネージ関数を呼び出すことができるようにするサービスです。 詳細については、「[アンマネージ DLL 関数](../../../framework/interop/consuming-unmanaged-dll-functions.md)の使用」を参照してください。 Visual Basic で PInvoke を使用するには、`Declare` ステートメントを使用するか、`DllImport` 属性を空のプロシージャに適用します。  
  
 Windows API の呼び出しでは、Visual Basic では、以前は、プログラミングの重要な部分をされたが、Visual Basic .NET を使用して必要なことはほとんどありません。 可能な限り、.NET Framework のマネージコードを使用して、Windows API 呼び出しの代わりにタスクを実行する必要があります。 このチュートリアルは、使用する必要のある状況についての情報を提供します。 Windows API が必要です。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="api-calls-using-declare"></a>Declare を使用した API 呼び出し  
 Windows Api を呼び出す最も一般的な方法は、`Declare` ステートメントを使用することです。  
  
### <a name="to-declare-a-dll-procedure"></a>DLL プロシージャを宣言するには  
  
1. 呼び出す関数の名前とその引数、引数の型、戻り値を指定し、それを含む DLL の名前と場所を決定します。  
  
    > [!NOTE]
    > Windows API については、プラットフォーム SDK の Windows API で Win32 SDK ドキュメントを参照してください。 Windows API を使用する定数の詳細については、Windows.h、Platform SDK に含まれているなどのヘッダー ファイルを調べてください。  
  
2. **[ファイル]** メニューの **[新規作成]** をクリックし、 **[プロジェクト]** をクリックして、新しい Windows アプリケーションプロジェクトを開きます。 **[新しいプロジェクト]** ダイアログ ボックスが表示されます。  
  
3. Visual Basic プロジェクトテンプレートの一覧から **[Windows アプリケーション]** を選択します。 新しいプロジェクトが表示されます。  
  
4. DLL を使用するクラスまたはモジュールに、次の `Declare` 関数を追加します。  
  
     [!code-vb[VbVbalrInterop#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#9)]  
  
### <a name="parts-of-the-declare-statement"></a>Declare ステートメントの部分  
 `Declare` ステートメントには、次の要素が含まれています。  
  
#### <a name="auto-modifier"></a>Auto 修飾子  
 `Auto` 修飾子は、共通言語ランタイムの規則 (または、指定されている場合はエイリアス名) に従って、メソッド名に基づいて文字列を変換するようにランタイムに指示します。  
  
#### <a name="lib-and-alias-keywords"></a>Lib キーワードと Alias キーワード  
 `Function` キーワードの後の名前は、インポートされた関数にアクセスするためにプログラムが使用する名前です。 これは、呼び出している関数の実際の名前と同じにすることも、任意の有効なプロシージャ名を使用してから、`Alias` キーワードを使用して、呼び出す関数の実際の名前を指定することもできます。  
  
 `Lib` キーワードを指定し、その後に、呼び出し元の関数が含まれている DLL の名前と場所を指定します。 Windows システムディレクトリにあるファイルのパスを指定する必要はありません。  
  
 呼び出す関数の名前が有効な Visual Basic プロシージャ名ではない場合、またはアプリケーション内の他の項目の名前と競合する場合は、`Alias` キーワードを使用します。 `Alias` 呼び出される関数の実際の名前を示します。  
  
#### <a name="argument-and-data-type-declarations"></a>引数とデータ型の宣言  
 引数とそのデータ型を宣言します。 Windows で使用されるデータ型が Visual Studio のデータ型に対応していないため、この部分は困難になる可能性があります。 Visual Basic は、引数を互換性のあるデータ型 (*マーシャリング*と呼ばれます) に変換することによって、さまざまな処理を実行します。 <xref:System.Runtime.InteropServices> 名前空間で定義されている <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性を使用して、引数のマーシャリング方法を明示的に制御できます。  
  
> [!NOTE]
> 以前のバージョンの Visual Basic では、パラメーター `As Any`を宣言できるようになりました。つまり、任意のデータ型のデータを使用できます。 Visual Basic では、すべての `Declare` ステートメントに特定のデータ型を使用する必要があります。  
  
#### <a name="windows-api-constants"></a>Windows API 定数  
 引数の中には、定数の組み合わせもあります。 たとえば、このチュートリアルで示されている `MessageBox` API は、メッセージボックスの表示方法を制御する `Typ` と呼ばれる整数の引数を受け取ります。 これらの定数の数値を確認するには、ファイルの `#define` ステートメントを調べます。 通常、数値は16進数で表示されるので、電卓を使用して追加したり、decimal に変換したりすることができます。 たとえば、感嘆符 `MB_ICONEXCLAMATION` 0x00000030、Yes/No style `MB_YESNO` 0x00000004 の定数を結合する場合は、数値を加算して、0x00000030 または 52 decimal の結果を取得できます。 10進数の結果を直接使用することもできますが、これらの値をアプリケーションで定数として宣言し、`Or` 演算子を使用して組み合わせることをお勧めします。  
  
##### <a name="to-declare-constants-for-windows-api-calls"></a>Windows API 呼び出しの定数を宣言するには  
  
1. 呼び出している Windows 関数のドキュメントを参照してください。 使用する定数の名前と、これらの定数の数値が含まれている .h ファイルの名前を確認します。  
  
2. メモ帳などのテキストエディターを使用してヘッダー (.h) ファイルの内容を表示し、使用している定数に関連付けられている値を検索します。 たとえば、`MessageBox` API では、定数 `MB_ICONQUESTION` を使用して、メッセージボックスに疑問符が表示されます。 `MB_ICONQUESTION` の定義は WinUser. h にあり、次のように表示されます。  
  
     `#define MB_ICONQUESTION             0x00000020L`  
  
3. これらの定数をアプリケーションで使用できるようにするには、クラスまたはモジュールに同等の `Const` ステートメントを追加します。 例 :  
  
     [!code-vb[VbVbalrInterop#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#11)]  
  
###### <a name="to-call-the-dll-procedure"></a>DLL プロシージャを呼び出すには  
  
1. `Button1` という名前のボタンをプロジェクトのスタートアップフォームに追加し、それをダブルクリックしてコードを表示します。 ボタンのイベントハンドラーが表示されます。  
  
2. 追加したボタンの `Click` イベントハンドラーにコードを追加して、プロシージャを呼び出し、適切な引数を指定します。  
  
     [!code-vb[VbVbalrInterop#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#12)]  
  
3. F5 キーを押してプロジェクトを実行します。 メッセージボックスには、 **[はい] と [** **いいえ**] の両方のボタンが表示されます。 いずれかをクリックします。  
  
#### <a name="data-marshaling"></a>データのマーシャリング  
 Visual Basic は、Windows API 呼び出しのパラメーターと戻り値のデータ型を自動的に変換しますが、`MarshalAs` 属性を使用して、API が想定するアンマネージデータ型を明示的に指定することができます。 相互運用マーシャリングの詳細については、「[相互運用マーシャリング](../../../framework/interop/interop-marshaling.md)」を参照してください。  
  
##### <a name="to-use-declare-and-marshalas-in-an-api-call"></a>API 呼び出しで Declare と MarshalAs を使用するには  
  
1. 呼び出す関数の名前とその引数、データ型、および戻り値を決定します。  
  
2. `MarshalAs` 属性へのアクセスを簡単にするには、次の例に示すように、クラスまたはモジュールのコードの先頭に `Imports` ステートメントを追加します。  
  
     [!code-vb[VbVbalrInterop#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#13)]  
  
3. インポートされた関数の関数プロトタイプを使用しているクラスまたはモジュールに追加し、`MarshalAs` 属性をパラメーターまたは戻り値に適用します。 次の例では、型 `void*` を想定する API 呼び出しが `AsAny`としてマーシャリングされています。  
  
     [!code-vb[VbVbalrInterop#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#14)]  
  
## <a name="api-calls-using-dllimport"></a>DllImport を使用した API 呼び出し  
 `DllImport` 属性は、タイプライブラリを使用せずに Dll 内の関数を呼び出すための2番目の方法を提供します。 `DllImport` は `Declare` ステートメントを使用することとほぼ同じですが、関数の呼び出し方法をより詳細に制御できます。  
  
 呼び出しが共有 (場合によっては*静的*) メソッドを参照している限り、ほとんどの Windows API 呼び出しで `DllImport` を使用できます。 クラスのインスタンスを必要とするメソッドは使用できません。 `Declare` ステートメントとは異なり、`DllImport` 呼び出しでは `MarshalAs` 属性を使用できません。  
  
### <a name="to-call-a-windows-api-using-the-dllimport-attribute"></a>DllImport 属性を使用して Windows API を呼び出すには  
  
1. **[ファイル]** メニューの **[新規作成]** をクリックし、 **[プロジェクト]** をクリックして、新しい Windows アプリケーションプロジェクトを開きます。 **[新しいプロジェクト]** ダイアログ ボックスが表示されます。  
  
2. Visual Basic プロジェクトテンプレートの一覧から **[Windows アプリケーション]** を選択します。 新しいプロジェクトが表示されます。  
  
3. `Button2` という名前のボタンをスタートアップフォームに追加します。  
  
4. [`Button2`] をダブルクリックして、フォームのコードビューを開きます。  
  
5. `DllImport`へのアクセスを簡単にするには、スタートアップフォームクラスのコードの先頭に `Imports` ステートメントを追加します。  
  
     [!code-vb[VbVbalrInterop#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#13)]  
  
6. フォームの `End Class` ステートメントの前に空の関数を宣言し、関数に `MoveFile`という名前を指定します。  
  
7. `Public` と `Shared` 修飾子を関数宣言に適用し、Windows API 関数が使用する引数に基づいて `MoveFile` のパラメーターを設定します。  
  
     [!code-vb[VbVbalrInterop#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#16)]  
  
     関数は、任意の有効なプロシージャ名を持つことができます。`DllImport` 属性は、DLL 内の名前を指定します。 また、パラメーターと戻り値の相互運用性マーシャリングも処理するため、API で使用されるデータ型に似た Visual Studio データ型を選択できます。  
  
8. `DllImport` 属性を空の関数に適用します。 最初のパラメーターは、呼び出し元の関数を含む DLL の名前と場所です。 Windows システムディレクトリにあるファイルのパスを指定する必要はありません。 2番目のパラメーターは、Windows API で関数の名前を指定する名前付き引数です。 この例では、`DllImport` 属性によって、`MoveFile` への呼び出しが KERNEL32.DLL の `MoveFileW` に強制的に転送されます。DLL. `MoveFileW` メソッドは、パス `src` からパス `dst`にファイルをコピーします。  
  
     [!code-vb[VbVbalrInterop#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#17)]  
  
9. 関数を呼び出すコードを `Button2_Click` イベントハンドラーに追加します。  
  
     [!code-vb[VbVbalrInterop#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#18)]  
  
10. Test.txt という名前のファイルを作成し、ハードドライブの C:\ Tmp ディレクトリに配置します。 必要に応じて、Tmp ディレクトリを作成します。  
  
11. F5 キーを押して、アプリケーションを起動します。 メインフォームが表示されます。  
  
12. **[Button2]** をクリックします。 ファイルを移動できる場合、"ファイルは正常に移動されました" というメッセージが表示されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Declare Statement](../../../visual-basic/language-reference/statements/declare-statement.md)
- [Auto](../../../visual-basic/language-reference/modifiers/auto.md)
- [Alias](../../../visual-basic/language-reference/statements/alias-clause.md)
- [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md)
- [マネージド コードでのプロトタイプの作成](../../../framework/interop/creating-prototypes-in-managed-code.md)
- [コールバック メソッドとしてのデリゲートのマーシャ リング](../../../framework/interop/marshaling-a-delegate-as-a-callback-method.md)
