---
title: セキュリティについてのその他の考慮事項
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, secure calls to Windows API
- security [Windows Forms]
- security [Windows Forms], calling APIs
- Clipboard [Windows Forms], securing access
ms.assetid: 15abda8b-0527-47c7-aedb-77ab595f2bf1
ms.openlocfilehash: c8d51a57194f1dc536bc4b5d0376987dbfd3b2cf
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739809"
---
# <a name="additional-security-considerations-in-windows-forms"></a>Windows フォームのセキュリティに関するその他の考慮事項
.NET Framework セキュリティ設定を使用すると、ローカルコンピューターとは異なる部分信頼環境で、アプリケーションの実行が異なる場合があります。 .NET Framework は、ファイルシステム、ネットワーク、アンマネージ Api などの重要なローカルリソースへのアクセスを制限します。 セキュリティ設定は、セキュリティシステムによって検証できない Microsoft Windows API またはその他の Api を呼び出すことができるかどうかに影響します。 また、ファイルやデータへのアクセス、印刷など、アプリケーションのその他の処理にも影響があります。 部分信頼環境でのファイルやデータへのアクセスの詳細については、「[Windows フォームにおけるファイルおよびデータへのより安全なアクセス](more-secure-file-and-data-access-in-windows-forms.md)」を参照してください。 部分信頼環境での印刷の詳細については、「[Windows フォームでのより安全な印刷](more-secure-printing-in-windows-forms.md)」を参照してください。  
  
 以下のセクションでは、部分信頼環境で実行されているアプリケーションから、クリップボードの操作、ウィンドウ操作の実行、および Windows API の呼び出しを行う方法について説明します。  
  
## <a name="clipboard-access"></a>クリップボードへのアクセス  
 <xref:System.Security.Permissions.UIPermission> クラスはクリップボードへのアクセスを制御し、関連付けられた <xref:System.Security.Permissions.UIPermissionClipboard> 列挙値はアクセスのレベルを示します。 使用されるアクセス許可レベルを次の表に示します。  
  
|UIPermissionClipboard の値|[説明]|  
|---------------------------------|-----------------|  
|<xref:System.Security.Permissions.UIPermissionClipboard.AllClipboard>|クリップボードは制限なしに使用できます。|  
|<xref:System.Security.Permissions.UIPermissionClipboard.OwnClipboard>|クリップボードは制限付きで使用できます。 クリップボードにデータを格納する機能 ([コピー] または [切り取り] のコマンド操作) は制限されません。 テキスト ボックスなど、[貼り付け] を受け入れる固有のコントロールは、クリップボードのデータを受け入れます。しかし、ユーザー コントロールはプログラムでクリップボードからデータを読み取ることができません。|  
|<xref:System.Security.Permissions.UIPermissionClipboard.NoClipboard>|クリップボードは使用できません。|  
  
 既定では、ローカルイントラネットゾーンは <xref:System.Security.Permissions.UIPermissionClipboard.AllClipboard> アクセスを受信し、インターネットゾーンは <xref:System.Security.Permissions.UIPermissionClipboard.OwnClipboard> アクセスを受け取ります。 つまり、アプリケーションはクリップボードにデータをコピーできますが、プログラムでクリップボードに貼り付けたりクリップボードから読み取ったりはできません。 これらの制限により、完全に信頼されていないプログラムは、他のアプリケーションによってクリップボードにコピーされた内容を読み取ることができません。 アプリケーションがクリップボードへの完全なアクセスを必要とするにもかかわらず、アクセス許可がない場合は、アプリケーションに対するアクセス許可を昇格する必要があります。 アクセス許可の昇格の詳細については、「[一般的なセキュリティ ポリシー管理](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ed5htz45(v=vs.100))」を参照してください。  
  
## <a name="window-manipulation"></a>ウィンドウ操作  
 <xref:System.Security.Permissions.UIPermission> クラスは、ウィンドウ操作やその他の UI 関連操作を実行するためのアクセス許可も制御し、関連付けられた <xref:System.Security.Permissions.UIPermissionWindow> 列挙値はアクセスのレベルを示します。 使用されるアクセス許可レベルを次の表に示します。  
  
 既定では、ローカルイントラネットゾーンは <xref:System.Security.Permissions.UIPermissionWindow.AllWindows> アクセスを受信し、インターネットゾーンは <xref:System.Security.Permissions.UIPermissionWindow.SafeTopLevelWindows> アクセスを受け取ります。 つまり、[インターネット] ゾーンでは、アプリケーションがほとんどのウィンドウ操作および UI 関連処理を実行できますが、ウィンドウの外観が変更されます。 変更されたウィンドウには、最初の実行時にバルーン通知が表示され、変更されたタイトル バー テキストが表示され、タイトル バーに閉じるボタンが必要になります。 バルーン通知とタイトル バーは、アプリケーションのユーザーに、アプリケーションが部分信頼で動作していることを示します。  
  
|UIPermissionWindow の値|[説明]|  
|------------------------------|-----------------|  
|<xref:System.Security.Permissions.UIPermissionWindow.AllWindows>|ユーザーは、すべてのウィンドウとユーザー入力イベントを無制限に使用できます。|  
|<xref:System.Security.Permissions.UIPermissionWindow.SafeTopLevelWindows>|ユーザーは、セーフ トップレベル ウィンドウとセーフ サブウィンドウだけを描画に使用でき、それらのセーフ トップレベル ウィンドウとセーフ サブウィンドウの中のユーザー インターフェイスに対するユーザー入力イベントだけを使用できます。 これらのセーフ ウィンドウには明確なラベルが付けられ、最小サイズと最大サイズに制限があります。 この制限により、偽システムログオン画面やシステムデスクトップなど、有害な可能性のあるスプーフィング攻撃を防止し、プログラムによるアクセスを親ウィンドウ、フォーカス関連 Api、および <xref:System.Windows.Forms.ToolTip> コントロールに制限することができます。|  
|<xref:System.Security.Permissions.UIPermissionWindow.SafeSubWindows>|ユーザーは、セーフ サブウィンドウだけを描画に使用でき、そのセーフ サブウィンドウの中のユーザー インターフェイスに対するユーザー入力イベントだけを使用できます。 ブラウザー内に表示されるコントロールは、セーフ サブウィンドウの一例です。|  
|<xref:System.Security.Permissions.UIPermissionWindow.NoWindows>|ユーザーは、ウィンドウおよびユーザー インターフェイス イベントを使用できません。 ユーザー インターフェイスは使用できません。|  
  
 <xref:System.Security.Permissions.UIPermissionWindow> 列挙体によって識別される各アクセス許可レベルでは、上のレベルよりも多くのアクションが許可されます。 次の表は、<xref:System.Security.Permissions.UIPermissionWindow.SafeTopLevelWindows> と <xref:System.Security.Permissions.UIPermissionWindow.SafeSubWindows> の値によって制限されるアクションを示しています。 各メンバーに対して必要なアクセス許可については、.NET Framework クラス ライブラリのドキュメントで該当するメンバーのトピックを参照してください。  
  
 <xref:System.Security.Permissions.UIPermissionWindow.SafeTopLevelWindows> のアクセス許可では、次の表に示す操作を制限します。  
  
|コンポーネント|制限される処理|  
|---------------|------------------------|  
|<xref:System.Windows.Forms.Application>|-   <xref:System.Windows.Forms.Application.SafeTopLevelCaptionFormat%2A> プロパティの設定|  
|<xref:System.Windows.Forms.Control>|-<xref:System.Windows.Forms.Control.Parent%2A> プロパティを取得しています。<br />-   `Region` プロパティの設定<br />-<xref:System.Windows.Forms.Control.FindForm%2A>、<xref:System.Windows.Forms.Control.Focus%2A>、<xref:System.Windows.Forms.Control.FromChildHandle%2A> および <xref:System.Windows.Forms.Control.FromHandle%2A>、<xref:System.Windows.Forms.Control.PreProcessMessage%2A>、<xref:System.Windows.Forms.Control.ReflectMessage%2A>、または <xref:System.Windows.Forms.Control.SetTopLevel%2A> メソッドの呼び出し。<br />-返されたコントロールが呼び出し元のコントロールの子でない場合は、<xref:System.Windows.Forms.Control.GetChildAtPoint%2A> メソッドを呼び出します。<br />-   コンテナー コントロール内でのコントロール フォーカスの変更|  
|<xref:System.Windows.Forms.Cursor>|-   <xref:System.Windows.Forms.Cursor.Clip%2A> プロパティの設定<br />-<xref:System.Windows.Forms.Control.Hide%2A> メソッドを呼び出しています。|  
|<xref:System.Windows.Forms.DataGrid>|-<xref:System.Windows.Forms.ContainerControl.ProcessTabKey%2A> メソッドを呼び出しています。|  
|<xref:System.Windows.Forms.Form>|-<xref:System.Windows.Forms.Form.ActiveForm%2A> または <xref:System.Windows.Forms.Form.MdiParent%2A> プロパティを取得しています。<br />-<xref:System.Windows.Forms.Form.ControlBox%2A>、<xref:System.Windows.Forms.Form.ShowInTaskbar%2A>、または <xref:System.Windows.Forms.Form.TopMost%2A> プロパティの設定。<br />-<xref:System.Windows.Forms.Form.Opacity%2A> プロパティを50% 未満に設定しています。<br />-プログラムによって <xref:System.Windows.Forms.FormWindowState.Minimized> されるように <xref:System.Windows.Forms.Form.WindowState%2A> プロパティを設定します。<br />-<xref:System.Windows.Forms.Form.Activate%2A> メソッドを呼び出しています。<br />-<xref:System.Windows.Forms.FormBorderStyle.None>、<xref:System.Windows.Forms.FormBorderStyle.FixedToolWindow>、および <xref:System.Windows.Forms.FormBorderStyle.SizableToolWindow><xref:System.Windows.Forms.FormBorderStyle> 列挙値を使用します。|  
|<xref:System.Windows.Forms.NotifyIcon>|-<xref:System.Windows.Forms.NotifyIcon> コンポーネントの使用は完全に制限されています。|  
  
 <xref:System.Security.Permissions.UIPermissionWindow.SafeSubWindows> 値は、<xref:System.Security.Permissions.UIPermissionWindow.SafeTopLevelWindows> 値によって設定された制限に加えて、次の表に示すアクションを制限します。  
  
|コンポーネント|制限される処理|  
|---------------|------------------------|  
|<xref:System.Windows.Forms.CommonDialog>|-<xref:System.Windows.Forms.CommonDialog> クラスから派生したダイアログボックスを表示します。|  
|<xref:System.Windows.Forms.Control>|-<xref:System.Windows.Forms.Control.CreateGraphics%2A> メソッドを呼び出しています。<br />-   <xref:System.Windows.Forms.Control.Cursor%2A> プロパティの設定|  
|<xref:System.Windows.Forms.Control.Cursor%2A>|-   <xref:System.Windows.Forms.Cursor.Current%2A> プロパティの設定|  
|<xref:System.Windows.Forms.MessageBox>|-<xref:System.Windows.Forms.Form.Show%2A> メソッドを呼び出しています。|  
  
### <a name="hosting-third-party-controls"></a>サードパーティ コントロールのホスト  
 フォームでサードパーティ コントロールをホストしている場合、他の種類のウィンドウ操作が発生する可能性があります。 サードパーティ製のコントロールは、自分で開発してコンパイルしたことのないカスタム <xref:System.Windows.Forms.UserControl> です。 ホスト シナリオを攻略するのは困難ですが、理論上、サードパーティ コントロールが描画サーフェイスを拡張して、フォームの領域全体を対象にする可能性があります。 このようなコントロールは、重要なダイアログ ボックスを模倣し、ユーザーのユーザー名/パスワードの組み合わせや銀行口座番号などの情報を要求する可能性があります。  
  
 このような考えられるリスクを制限するために、信頼できる販売元のサードパーティ コントロールのみを使用します。 確認できないソースからサードパーティ コントロールをダウンロードした場合、攻略行為が実行されないかソース コードを確認することをお勧めします。 ソースに悪意がないことを検証してから、アセンブリを自分でコンパイルし、ソースがアセンブリと一致することを確認します。  
  
## <a name="windows-api-calls"></a>Windows API 呼び出し  
 アプリケーションの設計で Windows API から関数を呼び出す必要がある場合は、アンマネージコードにアクセスすることになります。 この場合、Windows API 呼び出しまたは値を操作しているときに、ウィンドウまたはオペレーティングシステムに対するコードのアクションを特定できません。 <xref:System.Security.Permissions.SecurityPermission> クラスと <xref:System.Security.Permissions.SecurityPermissionFlag> 列挙型の <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> 値は、アンマネージコードへのアクセスを制御します。 アプリケーションは、<xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> アクセス許可が付与されている場合にのみ、アンマネージコードにアクセスできます。 既定では、ローカルで実行されているアプリケーションだけがアンマネージ コードを呼び出すことができます。  
  
 一部の Windows フォームメンバーは、<xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> アクセス許可を必要とするアンマネージアクセスを提供します。 次の表に、アクセス許可を必要とする <xref:System.Windows.Forms> 名前空間のメンバーを示します。 メンバーに対して必要なアクセス許可の詳細については、.NET Framework クラス ライブラリのドキュメントを参照してください。  
  
|コンポーネント|メンバー|  
|---------------|------------|  
|<xref:System.Windows.Forms.Application>|-   <xref:System.Windows.Forms.Application.AddMessageFilter%2A> メソッド<br /><xref:System.Windows.Forms.Application.CurrentInputLanguage%2A> プロパティの -   <br />-   `Exit` メソッド<br />-   <xref:System.Windows.Forms.Application.ExitThread%2A> メソッド<br /><xref:System.Windows.Forms.Application.ThreadException> イベントの -   |  
|<xref:System.Windows.Forms.CommonDialog>|-   <xref:System.Windows.Forms.CommonDialog.HookProc%2A> メソッド<br />-   <xref:System.Windows.Forms.CommonDialog.OwnerWndProc%2A>\ メソッド<br />-   <xref:System.Windows.Forms.CommonDialog.Reset%2A> メソッド<br />-   <xref:System.Windows.Forms.CommonDialog.RunDialog%2A> メソッド|  
|<xref:System.Windows.Forms.Control>|-   <xref:System.Windows.Forms.Control.CreateParams%2A> メソッド<br />-   <xref:System.Windows.Forms.Control.DefWndProc%2A> メソッド<br />-   <xref:System.Windows.Forms.Control.DestroyHandle%2A> メソッド<br />-   <xref:System.Windows.Forms.Control.WndProc%2A> メソッド|  
|<xref:System.Windows.Forms.Help>|<xref:System.Windows.Forms.Help.ShowHelp%2A> メソッドの -   <br />-   <xref:System.Windows.Forms.Help.ShowHelpIndex%2A> メソッド|  
|<xref:System.Windows.Forms.NativeWindow>|-   <xref:System.Windows.Forms.NativeWindow> クラス|  
|<xref:System.Windows.Forms.Screen>|-   <xref:System.Windows.Forms.Screen.FromHandle%2A> メソッド|  
|<xref:System.Windows.Forms.SendKeys>|-   <xref:System.Windows.Forms.SendKeys.Send%2A> メソッド<br />-   <xref:System.Windows.Forms.SendKeys.SendWait%2A> メソッド|  
  
 アプリケーションがアンマネージコードを呼び出すためのアクセス許可を持っていない場合は、アプリケーションで <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> アクセス許可を要求するか、機能を実装するための別の方法を検討する必要があります。多くの場合、Windows フォームには Windows API 関数の代替手段が用意されています。 代わりの手段がなく、アプリケーションがアンマネージ コードにアクセスする必要がある場合は、アプリケーションに対するアクセス許可を昇格する必要があります。  
  
 アンマネージ コードを呼び出すアクセス許可を与えられたアプリケーションは、ほとんどの処理を実行できます。 そのため、アンマネージ コードを呼び出すアクセス許可は、信頼されたソースからのアプリケーションに対してだけ与えるようにしてください。 また、アプリケーションによっては、アンマネージ コードの呼び出しを生成するアプリケーション機能の一部をオプションにするか、完全に信頼された環境でのみ有効にすることもできます。 危険なアクセス許可の詳細については、「[危険なアクセス許可とポリシー管理](../misc/dangerous-permissions-and-policy-administration.md)」を参照してください。 アクセス許可の昇格の詳細については、「[一般的なセキュリティ ポリシー管理](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ed5htz45(v=vs.100))」を参照してください。  
  
## <a name="see-also"></a>参照

- [Windows フォームにおけるファイルおよびデータへのより安全なアクセス](more-secure-file-and-data-access-in-windows-forms.md)
- [Windows フォームでのより安全な印刷](more-secure-printing-in-windows-forms.md)
- [Windows フォームのセキュリティの概要](security-in-windows-forms-overview.md)
- [Windows フォームのセキュリティ](windows-forms-security.md)
- [ClickOnce アプリケーションのセキュリティ](/visualstudio/deployment/securing-clickonce-applications)
