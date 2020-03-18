---
title: コーディング規則
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions [Visual Basic], Visual Basic
- examples [Visual Basic], coding conventions
- Visual Basic code, conventions
ms.assetid: c1df130b-fec6-49a5-becf-0a7e494a1d0f
ms.openlocfilehash: 36cd3a927d2fdf197e6b496d9308fc43a555d59b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346153"
---
# <a name="visual-basic-coding-conventions"></a>Visual Basic のコーディング規則
Microsoft は、ここで示すガイドラインに従ってサンプルおよびドキュメントを開発しています。 同じコーディング規則に従うと、次のような利点があります。  
  
- コードの見た目が統一されるため、コードを読むときに、レイアウトではなく内容に重点を置くことができます。  
  
- これまでの経験に基づいて推測できるようになるため、コードをすばやく理解できます。  
  
- コードのコピー、変更、保守がより簡単になります。  
  
- コードが Visual Basic の "ベスト プラクティス" に従っていることを確認できます。  
  
## <a name="naming-conventions"></a>名前付け規則  
  
- 名前付けのガイドラインについては、「[名前付けのガイドライン](../../../standard/design-guidelines/naming-guidelines.md)」を参照してください。  
  
- "My" または "my" を変数名の一部として使用しないようにします。 `My` オブジェクトとの混同を招くからです。  
  
- 自動生成されたコードに含まれるオブジェクトの名前をこのガイドラインに合わせて変更する必要はありません。  
  
## <a name="layout-conventions"></a>レイアウト規則  
  
- タブを空白として挿入し、4 文字インデントによるスマート インデントを使用します。  
  
- コードエディターでコードを再フォーマットするには、**コードを簡単に一覧表示 (再フォーマット)** します。 詳細については、「[[オプション]、[テキストエディター]、[基本] (Visual Basic)](/visualstudio/ide/reference/options-text-editor-basic-visual-basic)」を参照してください。  
  
- 1 つの行には 1 つのステートメントのみを記述します。 Visual Basic の行区切り記号 (:) は使用しないでください。  
  
- 言語で許可される場所では、明示的な行継続文字 "_" ではなく暗黙的な行継続を使用します。  
  
- 1 つの行には 1 つの宣言のみを記述します。  
  
- **コードの一覧表示 (再フォーマット)** によって、継続行の書式が自動的に設定されない場合は、継続行を手動で1タブストップでインデントします。 ただし、リストの項目は常に左揃えにします。  
  
    ```vb  
    a As Integer,  
    b As Integer  
    ```  
  
- メソッド定義とプロパティ定義の間に少なくとも 1 行の空白行を追加します。  
  
## <a name="commenting-conventions"></a>コメント規則  
  
- コメントは、コード行の末尾ではなく別の行に記述します。  
  
- 英語でコメントを記述する場合、コメント テキストの始まりには英大文字を使用し、終わりにはピリオドを使用します。  
  
- コメント デリミター (') とコメント テキストの間に空白を 1 つ挿入します。  
  
     [!code-vb[VbVbalrGuidelines#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#2)]  
  
- 整形されたアスタリスク (*) のブロックでコメントを囲まないようにします。  
  
## <a name="program-structure"></a>プログラムの構造  
  
- `Main` メソッドを使用するときには、新しいコンソール アプリケーションの既定の構造を使用し、コマンド ライン引数には `My` を使用します。  
  
     [!code-vb[VbVbalrGuidelines#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#3)]  
  
## <a name="language-guidelines"></a>言語ガイドライン  
  
### <a name="string-data-type"></a>文字列型 (String)  
  
- 次のコードに示すように、短い文字列を連結するときは[文字列補間](https://docs.microsoft.com/dotnet/visual-basic/programming-guide/language-features/strings/interpolated-strings)を使用します。
  
     ```vb
     MsgBox($"hello{vbCrLf}goodbye")
     ```
  
- ループ内での文字列の追加には <xref:System.Text.StringBuilder> オブジェクトを使用します。  
  
     [!code-vb[VbVbalrGuidelines#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#5)]  
  
### <a name="relaxed-delegates-in-event-handlers"></a>イベント ハンドラー内の厳密でないデリゲート  
 イベント ハンドラーの引数 (Object および EventArgs) は明示的に修飾しません。 イベントに渡されるイベント引数 (sender as Object、e as EventArgs など) を使用しない場合は、厳密でないデリゲートを使用して、コードでイベント引数を省略します。  
  
 [!code-vb[VbVbalrGuidelines#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#7)]  
  
### <a name="unsigned-data-type"></a>Unsigned データ型  
  
- 特に必要でない限り、unsigned 型ではなく `Integer` を使用します。  
  
### <a name="arrays"></a>配列  
  
- 宣言行で配列を初期化するときは短い構文を使用します。 たとえば、次のような構文を使用します。  
  
     [!code-vb[VbVbalrGuidelines#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#8)]  
  
     次のような構文は使用しません。  
  
     [!code-vb[VbVbalrGuidelines#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#9)]  
  
- 配列指定子は、変数ではなく型に指定します。 たとえば、次のような構文を使用します。  
  
     [!code-vb[VbVbalrGuidelines#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#11)]  
  
     次のような構文は使用しません。  
  
     [!code-vb[VbVbalrGuidelines#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#10)]  
  
- 基本データ型の配列の宣言と初期化では、{ } 構文を使用します。 たとえば、次のような構文を使用します。  
  
     [!code-vb[VbVbalrGuidelines#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#12)]  
  
     次のような構文は使用しません。  
  
     [!code-vb[VbVbalrGuidelines#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#13)]  
  
### <a name="use-the-with-keyword"></a>With キーワードの使用  
 同じオブジェクトの呼び出しを複数回使用する場合には、`With` キーワードの使用を検討します。  
  
 [!code-vb[VbVbalrGuidelines#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#15)]  
  
### <a name="use-the-trycatch-and-using-statements-when-you-use-exception-handling"></a>例外処理を使用する場合の Try...Catch/Using ステートメントの使用  
 `On Error Goto`は使用しないでください。  
  
### <a name="use-the-isnot-keyword"></a>IsNot キーワードの使用  
 `IsNot` の代わりに `Not...Is Nothing` キーワードを使用します。  
  
### <a name="new-keyword"></a>New キーワード  
  
- 短い形式のインスタンス化を使用します。 たとえば、次のような構文を使用します。  
  
     [!code-vb[VbVbalrGuidelines#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#21)]  
  
     この行は次の行と同じ結果をもたらします。  
  
     [!code-vb[VbVbalrGuidelines#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#22)]  
  
- 新しいオブジェクトには、パラメーターなしのコンストラクターの代わりにオブジェクト初期化子を使用します。  
  
     [!code-vb[VbVbalrGuidelines#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#23)]  
  
### <a name="event-handling"></a>イベント処理  
  
- `Handles` ではなく `AddHandler` を使用します。  
  
     [!code-vb[VbVbalrGuidelines#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#24)]  
  
- `AddressOf` を使用し、デリゲートの明示的なインスタンス化は避けます。  
  
     [!code-vb[VbVbalrGuidelines#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#25)]  
  
- イベントを定義するときには、短い構文を使用し、デリゲートの定義はコンパイラに任せます。  
  
     [!code-vb[VbVbalrGuidelines#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#26)]  
  
- `Nothing` メソッドを呼び出す前にイベントが `RaiseEvent` (null) かどうか確認しないようにします。 `RaiseEvent` は、イベントを発生させる前に `Nothing` を確認します。  
  
### <a name="using-shared-members"></a>共有メンバーの使用  
 `Shared` メンバーの呼び出しにはクラス名を使用し、インスタンス変数からは行わないようにします。  
  
### <a name="use-xml-literals"></a>XML リテラルの使用  
 XML リテラルを使用すると、XML 操作時に行う最も一般的なタスク (読み込み、クエリ、変換など) を簡素化できます。 XML を使用して開発を行う場合は、次のガイドラインに従います。  
  
- XML API を直接呼び出す代わりに XML リテラルを使用して XML ドキュメントおよびフラグメントを作成します。  
  
- ファイル レベルまたはプロジェクト レベルで XML 名前空間をインポートし、XML リテラルによるパフォーマンスの最適化を利用します。  
  
- XML 軸プロパティを使用して XML ドキュメント内の要素と属性にアクセスします。  
  
- `Add` メソッドなどの API 呼び出しを使用する代わりに、埋め込み式を使用して既存の値から値を組み込んで XML を作成します。  
  
     [!code-vb[VbVbalrGuidelines#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#27)]  
  
### <a name="linq-queries"></a>LINQ クエリ  
  
- クエリ変数にはわかりやすい名前を使用します。  
  
     [!code-vb[VbVbalrGuidelines#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#28)]  
  
- クエリ内で要素の名前を指定して、匿名型のプロパティ名の大文字と小文字の使用が正しい Pascal 形式になるようにします。  
  
     [!code-vb[VbVbalrGuidelines#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#29)]  
  
- 結果のプロパティ名があいまいになる場合は、プロパティ名を変更します。 たとえば、クエリが顧客名と注文 ID を返す場合、それらの名前を結果の `Name` と `ID` のままにはせずに変更します。  
  
     [!code-vb[VbVbalrGuidelines#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#30)]  
  
- クエリ変数と範囲変数の宣言で型の推論を使用します。  
  
     [!code-vb[VbVbalrGuidelines#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#31)]  
  
- 各クエリ句を `From` ステートメントの下に揃えます。  
  
     [!code-vb[VbVbalrGuidelines#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#32)]  
  
- `Where` 句を他のクエリ句より先に使用し、それ以降のクエリ句では、フィルター化されたデータセットが処理されるようにします。  
  
     [!code-vb[VbVbalrGuidelines#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#33)]  
  
- `Join` 句を使用して暗黙的に結合操作を定義する代わりに、`Where` 句を使用して明示的に結合操作を定義します。  
  
     [!code-vb[VbVbalrGuidelines#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#34)]  
  
## <a name="see-also"></a>関連項目

- [安全なコーディングのガイドライン](../../../standard/security/secure-coding-guidelines.md)
