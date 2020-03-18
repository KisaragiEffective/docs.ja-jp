---
title: '方法: 実行時にイベントハンドラーを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms, event handling
- event handlers [Windows Forms], creating
- run time [Windows Forms], creating event handlers at
- examples [Windows Forms], event handling
- Button control [Windows Forms], event handlers
ms.assetid: 2e7c9e1a-61fe-444d-8113-3c5bacf1c8cb
ms.openlocfilehash: 0b496a3da77c5bcf7a08c435edba468a7c5809cb
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739498"
---
# <a name="how-to-create-event-handlers-at-run-time-for-windows-forms"></a>方法 : Windows フォームで実行時にイベント ハンドラーを作成する

Visual Studio の Windows フォームデザイナーを使用してイベントを作成するだけでなく、実行時にイベントハンドラーを作成することもできます。 これにより、プログラムが最初に起動したときにイベント ハンドラーを接続する代わりに、コード内に記述されている条件に基づいて、実行時にイベント ハンドラーを接続できます。

## <a name="create-an-event-handler-at-run-time"></a>実行時にイベントハンドラーを作成する

1. イベントハンドラーを追加するフォームを開きます。

2. 処理するイベントに対応するメソッド シグネチャを持つメソッドをフォームに追加します。

     たとえば、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Click> イベントを処理する場合は、次のようなメソッドを作成します。

    ```vb
    Private Sub Button1_Click(ByVal sender As Object, ByVal e As EventArgs)
       ' Add event handler code here.
    End Sub
    ```

    ```csharp
    private void button1_Click(object sender, System.EventArgs e)
    {
    // Add event handler code here.
    }
    ```

    ```cpp
    private:
       void button1_Click(System::Object ^ sender,
          System::EventArgs ^ e)
       {
          // Add event handler code here.
       }
    ```

3. アプリケーションに応じて、イベント ハンドラーにコードを追加します。

4. イベント ハンドラーをどのフォームまたはコントロールに対して作成するかを指定します。

5. フォームのクラスのメソッドに、イベントを処理するためのイベント ハンドラーを指定するコードを追加します。 たとえば、次のコードでは、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Click> イベントを処理 `button1_Click` イベントハンドラーを指定しています。

    ```vb
    AddHandler Button1.Click, AddressOf Button1_Click
    ```

    ```csharp
    button1.Click += new EventHandler(button1_Click);
    ```

    ```cpp
    button1->Click += gcnew System::EventHandler(this, &Form1::button1_Click);
    ```

     上の Visual Basic コードに示されている <xref:System.ComponentModel.EventHandlerList.AddHandler%2A> メソッドは、ボタンのクリックイベントハンドラーを確立します。

## <a name="see-also"></a>参照

- [Windows フォーム内のイベント ハンドラーの作成](creating-event-handlers-in-windows-forms.md)
- [イベント ハンドラーの概要](event-handlers-overview-windows-forms.md)
- [Visual Basic での継承されたイベント ハンドラーのトラブルシューティング](../../visual-basic/programming-guide/language-features/events/troubleshooting-inherited-event-handlers.md)
