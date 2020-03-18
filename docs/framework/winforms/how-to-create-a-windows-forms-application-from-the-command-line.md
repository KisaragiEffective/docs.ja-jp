---
title: コマンド ラインから Windows フォーム アプリケーションを作成する。
titleSuffix: ''
ms.date: 03/14/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, application development from command line
- Windows Forms, getting started
- Windows Forms, creating basic form
ms.assetid: 45ad3f8b-1c26-4c9f-91a9-3bb0759a47a4
ms.openlocfilehash: da6b9da53a36a44233dde4f0d1c4f147d913c7cf
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739527"
---
# <a name="how-to-create-a-windows-forms-application-from-the-command-line"></a>方法: コマンドラインから Windows フォームアプリケーションを作成する

次の手順では、コマンドラインから Windows フォーム アプリケーションを作成して実行するために完了する必要のある基本的な手順について説明します。 Visual Studio では、これらの手順に対する広範なサポートが用意されています。  「[チュートリアル: WPF での Windows フォームコントロールのホスト](../wpf/advanced/walkthrough-hosting-a-windows-forms-control-in-wpf.md)」も参照してください。
  
## <a name="procedure"></a>手順  
  
#### <a name="to-create-the-form"></a>フォームを作成するには  
  
1. 空のコードファイルに、次の `Imports` または `using` ステートメントを入力します。  
  
     [!code-csharp[System.Windows.Forms.BasicForm#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.BasicForm#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/VB/Form1.vb#2)]  
  
2. Form クラスから継承する `Form1` という名前のクラスを宣言します。
  
     [!code-csharp[System.Windows.Forms.BasicForm#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/CS/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.BasicForm#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/VB/Form1.vb#3)]  
  
3. `Form1`のパラメーターなしのコンストラクターを作成します。
  
     後続の手順で、コンストラクターにさらにコードを追加します。
  
     [!code-csharp[System.Windows.Forms.BasicForm#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/CS/Form1.cs#4)]
     [!code-vb[System.Windows.Forms.BasicForm#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/VB/Form1.vb#4)]  
  
4. `Main` メソッドをクラスに追加します。
  
    1. C# `Main` メソッドに <xref:System.STAThreadAttribute> を適用して、Windows フォームアプリケーションがシングルスレッドアパートメントであることを指定します。 (Visual Basic で開発された Windows フォームアプリケーションでは、既定でシングルスレッドアパートメントモデルが使用されるため、Visual Basic では属性は必要ありません)。  
  
    2. オペレーティングシステムのスタイルをアプリケーションに適用するには、<xref:System.Windows.Forms.Application.EnableVisualStyles%2A> を呼び出します。  
  
    3. フォームのインスタンスを作成して実行します。  
  
     [!code-csharp[System.Windows.Forms.BasicForm#5](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/CS/Form1.cs#5)]
     [!code-vb[System.Windows.Forms.BasicForm#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BasicForm/VB/Form1.vb#5)]  
  
#### <a name="to-compile-and-run-the-application"></a>アプリケーションをコンパイルして実行するには  
  
1. .NET Framework コマンド プロンプトで、`Form1` クラスを作成したディレクトリに移動します。  
  
2. フォームをコンパイルします。  
  
    - を使用してC#いる場合は、次のように入力します。 `csc form1.cs`  
  
         `-or-`  
  
    - Visual Basic を使用している場合は、次のように入力します。 `vbc form1.vb`  
  
3. コマンド プロンプトに `Form1.exe` を入力します。  
  
## <a name="adding-a-control-and-handling-an-event"></a>コントロールの追加とイベントの処理

前の手順は、コンパイルして実行する基本的な Windows フォームを作成する方法を示しました。 次の手順では、コントロールを作成してフォームに追加し、コントロールのイベントを処理する方法を示します。 Windows フォームに追加できるコントロールの詳細については、「[コントロールの Windows フォーム](./controls/index.md)」を参照してください。
  
 Windows フォーム アプリケーションを作成する方法を理解するだけでなく、イベント ベースのプログラミングとユーザー入力を処理する方法を理解する必要があります。 詳細については、「 [Windows フォームでのイベントハンドラーの作成](creating-event-handlers-in-windows-forms.md)」および「[ユーザー入力の処理](./controls/handling-user-input.md)」を参照してください。  
  
#### <a name="to-declare-a-button-control-and-handle-its-click-event"></a>ボタン コントロールを宣言して、クリック イベントを処理するには  
  
1. `button1` という名前のボタン コントロールを宣言します。  
  
2. コンストラクターでボタンを作成し、<xref:System.Windows.Forms.Control.Size%2A>、<xref:System.Windows.Forms.Control.Location%2A>、および <xref:System.Windows.Forms.Control.Text%2A> の各プロパティを設定します。  
  
3. フォームにボタンを追加します。  
  
     次のコード例は、ボタンコントロールを宣言する方法を示しています。
  
     [!code-csharp[System.Windows.Forms.FormWithButton#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.FormWithButton#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/VB/Form1.vb#2)]  
  
4. ボタンで <xref:System.Windows.Forms.Control.Click> イベントを処理するメソッドを作成します。  
  
5. クリック イベント ハンドラーで、メッセージ "Hello World" と共に <xref:System.Windows.Forms.MessageBox> を表示します。  
  
     次のコード例は、ボタンコントロールの click イベントを処理する方法を示しています。
  
     [!code-csharp[System.Windows.Forms.FormWithButton#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/CS/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.FormWithButton#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/VB/Form1.vb#3)]  
  
6. <xref:System.Windows.Forms.Control.Click> イベントを作成したメソッドに関連付けます。  
  
     次のコード例は、イベントをメソッドに関連付ける方法を示します。  
  
     [!code-csharp[System.Windows.Forms.FormWithButton#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/CS/Form1.cs#4)]
     [!code-vb[System.Windows.Forms.FormWithButton#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/VB/Form1.vb#4)]  
  
7. 前の手順で説明したように、アプリケーションをコンパイルして実行します。  
  
## <a name="example"></a>例  
 
次のコード例は、前の手順の完全な例です。
  
 [!code-csharp[System.Windows.Forms.FormWithButton#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/CS/Form1.cs#1)]
 [!code-vb[System.Windows.Forms.FormWithButton#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FormWithButton/VB/Form1.vb#1)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Form>
- <xref:System.Windows.Forms.Control>
- [Windows フォームの表示形式の変更](changing-the-appearance-of-windows-forms.md)
- [Windows フォーム アプリケーションの拡張](./advanced/index.md)
- [Windows フォームについて](getting-started-with-windows-forms.md)
