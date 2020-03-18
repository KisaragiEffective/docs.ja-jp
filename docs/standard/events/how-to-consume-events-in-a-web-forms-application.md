---
title: '方法 : Web フォーム アプリケーションでイベントを利用する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- events [.NET Framework], Web Forms
- Web Forms controls, and events
- event handlers [.NET Framework], Web Forms
- events [.NET Framework], consuming
- Web Forms, event handling
ms.assetid: 73bf8638-c4ec-4069-b0bb-a1dc79b92e32
ms.openlocfilehash: 1f95fd0dcc12f2d4e47ee07e1e6bb15d91000f0f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73124778"
---
# <a name="how-to-consume-events-in-a-web-forms-application"></a>方法 : Web フォーム アプリケーションでイベントを利用する
ASP.NET Web フォーム アプリケーションの一般的なシナリオとして、Web ページにコントロールを入力し、ユーザーがクリックしたコントロールに基づいて特定のアクションを実行するというものがあります。 たとえば、<xref:System.Web.UI.WebControls.Button?displayProperty=nameWithType> コントロールは、ユーザーが Web ページでそれをクリックすると、イベントを発生させます。 そのイベントを処理することで、アプリケーションはそのボタン クリックに最適なアプリケーション ロジックを実行できます。  
  
### <a name="to-handle-a-button-click-event-on-a-webpage"></a>Web ページ上のボタン クリック イベントを処理するには  
  
1. <xref:System.Web.UI.WebControls.Button> コントロールを含む ASP.NET Web フォーム ページ (Web ページ) を作成します。このコントロールの `OnClick` 値には、次の手順で定義するメソッドの名前を設定します。  
  
    ```xml  
    <asp:Button ID="Button1" runat="server" Text="Click Me" OnClick="Button1_Click" />  
    ```  
  
2. <xref:System.Web.UI.WebControls.Button.Click> イベント デリゲート シグネチャに一致し、`OnClick` 値に定義した名前を含むイベント ハンドラーを定義します。  
  
    ```csharp  
    protected void Button1_Click(object sender, EventArgs e)  
    {  
        // perform action  
    }  
    ```  
  
    ```vb  
    Protected Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click  
        ' perform action  
    End Sub  
    ```  
  
     <xref:System.Web.UI.WebControls.Button.Click> イベントはデリゲート タイプに <xref:System.EventHandler> クラスを、イベント データに <xref:System.EventArgs> クラスを使用します。 ASP.NET ページ フレームワークは、<xref:System.EventHandler> のインスタンスを作成し、<xref:System.Web.UI.WebControls.Button.Click> インスタンスの <xref:System.Web.UI.WebControls.Button> イベントにこのデリゲート インスタンスを追加するコードを自動的に生成します。  
  
3. 手順 2 で定義したイベント ハンドラー メソッドで、イベントの発生時に必要となるアクションを実行するコードを追加します。  
  
## <a name="see-also"></a>参照

- [イベント](../../../docs/standard/events/index.md)
