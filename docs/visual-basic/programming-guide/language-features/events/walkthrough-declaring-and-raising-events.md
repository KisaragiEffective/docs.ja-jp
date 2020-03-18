---
title: イベントの宣言と発生
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], events
- events [Visual Basic], walkthroughs
- declaring events [Visual Basic], walkthroughs
- events [Visual Basic], declaring
- events [Visual Basic], raising
- raising events [Visual Basic], walkthroughs
ms.assetid: 8ffb3be8-097d-4d3c-b71e-04555ebda2a2
ms.openlocfilehash: 6f4c303604f9cf55b4ecd500636e0d2772b6234c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345087"
---
# <a name="walkthrough-declaring-and-raising-events-visual-basic"></a>チュートリアル: イベントの宣言と発生 (Visual Basic)
このチュートリアルでは、`Widget`という名前のクラスのイベントを宣言して発生させる方法について説明します。 手順を完了したら、「[チュートリアル: イベントの処理](../../../../visual-basic/programming-guide/language-features/events/walkthrough-handling-events.md)」の関連トピックを参照することをお勧めします。これは、`Widget` オブジェクトからのイベントを使用してアプリケーションにステータス情報を提供する方法を示しています。  
  
## <a name="the-widget-class"></a>ウィジェットクラス  
 `Widget` クラスがあることを前提としています。 `Widget` クラスには、実行に時間がかかる可能性があり、アプリケーションで何らかの完了インジケーターを設定できるようにするためのメソッドが用意されています。  
  
 もちろん、`Widget` オブジェクトにパーセント (パーセント) のダイアログボックスを表示させることもできますが、その場合は、`Widget` クラスを使用したすべてのプロジェクトでそのダイアログボックスがスタックします。 オブジェクトの設計では、オブジェクトを使用するアプリケーションで、フォームまたはダイアログボックスを管理するだけではなく、ユーザーインターフェイスを処理することをお勧めします。  
  
 `Widget` の目的は、他のタスクを実行することです。そのため、`PercentDone` イベントを追加し、`Widget`のメソッドを呼び出すプロシージャでそのイベントを処理し、ステータスの更新を表示することをお勧めします。 `PercentDone` イベントには、タスクを取り消すためのメカニズムも用意されています。  
  
#### <a name="to-build-the-code-example-for-this-topic"></a>このトピックのコード例をビルドするには  
  
1. 新しい Visual Basic Windows アプリケーションプロジェクトを開き、`Form1`という名前のフォームを作成します。  
  
2. `Form1`に2つのボタンとラベルを追加します。  
  
3. 次の表のように、各オブジェクトに名前を付けます。  
  
    |オブジェクト|プロパティ|設定|  
    |------------|--------------|-------------|  
    |`Button1`|`Text`|開始タスク|  
    |`Button2`|`Text`|キャンセル|  
    |`Label`|`(Name)`, `Text`|lblPercentDone, 0|  
  
4. **[プロジェクト]** メニューの **[クラスの追加]** をクリックして、`Widget.vb` という名前のクラスをプロジェクトに追加します。  
  
#### <a name="to-declare-an-event-for-the-widget-class"></a>ウィジェットクラスのイベントを宣言するには  
  
- `Widget` クラスでイベントを宣言するには、`Event` キーワードを使用します。 イベントは `Widget`の `PercentDone` イベントが示すように、`ByVal` 引数と `ByRef` 引数を持つことができます。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#1)]  
  
 呼び出し元のオブジェクトが `PercentDone` イベントを受け取ると、`Percent` 引数には、完了したタスクの割合が含まれます。 `Cancel` 引数を `True` に設定して、イベントを発生させたメソッドを取り消すことができます。  
  
> [!NOTE]
> イベント引数は、プロシージャの引数と同じように宣言できます。ただし、イベントに `Optional` または `ParamArray` 引数を指定することはできません。また、イベントには戻り値がありません。  
  
 `PercentDone` イベントは、`Widget` クラスの `LongTask` メソッドによって発生します。 `LongTask` は2つの引数を取ります。これは、メソッドが処理を実行している時間の長さと、`LongTask` が一時停止して `PercentDone` イベントが発生するまでの最小時間間隔です。  
  
#### <a name="to-raise-the-percentdone-event"></a>PercentDone イベントを発生させるには  
  
1. このクラスで使用される `Timer` プロパティへのアクセスを簡略化するには、クラスモジュールの宣言セクションの先頭に `Class Widget` ステートメントの上に `Imports` ステートメントを追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#2)]  
  
2. `Widget` クラスに次のコードを追加します。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#3)]  
  
 アプリケーションが `LongTask` メソッドを呼び出すと、`Widget` クラスは `MinimumInterval` 秒ごとに `PercentDone` イベントを発生させます。 イベントが返されると、`LongTask` `Cancel` 引数が `True`に設定されているかどうかを確認します。  
  
 ここでは、いくつかの免責事項が必要です。 わかりやすくするために、`LongTask` の手順では、タスクの実行時間が事前にわかっていることを前提としています。 これはほとんどの場合、そうではありません。 タスクを均等なサイズのチャンクに分割することは困難です。多くの場合、ユーザーにとって最も重要なのは、何かが発生したことを示す前にが経過した時間だけです。  
  
 このサンプルの別の欠陥が発見された可能性があります。 `Timer` プロパティは、深夜0時以降に経過した秒数を返します。そのため、深夜の直前にアプリケーションが起動された場合は、アプリケーションが停止します。 時間を測定するためのより慎重なアプローチは、このような境界条件を考慮に入れるか、`Now`などのプロパティを使用して完全に回避することです。  
  
 `Widget` クラスでイベントを発生させることができるようになったので、次のチュートリアルに進むことができます。 [チュートリアル:](../../../../visual-basic/programming-guide/language-features/events/walkthrough-handling-events.md)イベントの処理では、`WithEvents` を使用してイベントハンドラーを `PercentDone` イベントに関連付ける方法を示します。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.DateAndTime.Timer%2A>
- <xref:Microsoft.VisualBasic.DateAndTime.Now%2A>
- [チュートリアル : イベントの処理](../../../../visual-basic/programming-guide/language-features/events/walkthrough-handling-events.md)
- [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)
