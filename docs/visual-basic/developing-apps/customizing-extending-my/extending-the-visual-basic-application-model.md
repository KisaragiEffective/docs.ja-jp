---
title: Visual Basic アプリケーション モデルの拡張
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic Application Model, extending
ms.assetid: e91d3bed-4c27-40e3-871d-2be17467c72c
ms.openlocfilehash: 46c18ab540c90c4147514685c2acc824755b435f
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976859"
---
# <a name="extending-the-visual-basic-application-model"></a>Visual Basic アプリケーション モデルの拡張

<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> クラスの `Overridable` メンバーをオーバーライドすることによって、アプリケーションモデルに機能を追加できます。 この手法を使用すると、アプリケーションモデルの動作をカスタマイズし、アプリケーションの起動時およびシャットダウン時に独自のメソッドの呼び出しを追加できます。

## <a name="visual-overview-of-the-application-model"></a>アプリケーションモデルの視覚的な概要

このセクションでは、Visual Basic アプリケーションモデルでの関数呼び出しの順序を視覚的に示します。 次のセクションでは、各関数の目的について詳しく説明します。

次の図は、通常の Visual Basic Windows フォームアプリケーションのアプリケーションモデル呼び出しシーケンスを示しています。 `Sub Main` プロシージャが <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> メソッドを呼び出すと、シーケンスが開始されます。

![アプリケーションモデル呼び出しシーケンスを示す図。](./media/extending-the-visual-basic-application-model/application-model-call-sequence.gif)

Visual Basic アプリケーションモデルでは、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> イベントと <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> イベントも提供されます。 次の図は、これらのイベントを発生させるメカニズムを示しています。

![StartupNextInstance イベントを発生させる OnStartupNextInstance メソッドを示す図。](./media/extending-the-visual-basic-application-model/raise-startupnextinstance-event.gif)

![UnhandledException イベントを発生させている OnUnhandledException メソッドを示す図。](./media/extending-the-visual-basic-application-model/raise-unhandledexception-event.gif)

## <a name="overriding-the-base-methods"></a>基本メソッドのオーバーライド

<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> メソッドは、`Application` メソッドが実行される順序を定義します。 既定では、Windows フォームアプリケーションの `Sub Main` プロシージャは <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> メソッドを呼び出します。

アプリケーションが通常のアプリケーション (複数インスタンスアプリケーション)、または単一インスタンスアプリケーションの最初のインスタンスの場合、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> メソッドは、次の順序で `Overridable` メソッドを実行します。

1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A>. 既定では、このメソッドは、メインアプリケーションスレッドの visual スタイル、テキスト表示スタイル、および現在のプリンシパルを設定します (アプリケーションが Windows 認証を使用している場合)。また、コマンドライン引数として `/nosplash` も `-nosplash` も使用されていない場合は、`ShowSplashScreen` を呼び出します。

     この関数が `False` を返すと、アプリケーションの起動シーケンスは取り消されます。 これは、アプリケーションが実行されない状況がある場合に便利です。

     <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A> メソッドは、次のメソッドを呼び出します。

    1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShowSplashScreen%2A>. アプリケーションにスプラッシュスクリーンが定義されているかどうかを判断し、表示されている場合はスプラッシュスクリーンを別のスレッドで表示します。

         <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShowSplashScreen%2A> メソッドには、少なくとも <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MinimumSplashScreenDisplayTime%2A> プロパティによって指定されたミリ秒数のスプラッシュスクリーンを表示するコードが含まれています。 この機能を使用するには、(`My.Application.MinimumSplashScreenDisplayTime` プロパティを2秒に設定する)**プロジェクトデザイナー**を使用してアプリケーションにスプラッシュスクリーンを追加するか、または <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A> または <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A> メソッドをオーバーライドするメソッドで `My.Application.MinimumSplashScreenDisplayTime` プロパティを設定する必要があります。 詳細については、「<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MinimumSplashScreenDisplayTime%2A>」を参照してください。

    2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A>. スプラッシュスクリーンを初期化するコードをデザイナーが出力できるようにします。

         既定では、このメソッドは何も行いません。 Visual Basic**プロジェクトデザイナー**でアプリケーションのスプラッシュスクリーンを選択すると、デザイナーは、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.SplashScreen%2A> プロパティをスプラッシュスクリーンフォームの新しいインスタンスに設定するメソッドを使用して、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A> メソッドをオーバーライドします。

2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartup%2A>. `Startup` イベントを発生させるための機能拡張ポイントを提供します。 この関数が `False` を返すと、アプリケーションの起動シーケンスは停止します。

     既定では、このメソッドは <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> イベントを発生させます。 イベントハンドラーがイベント引数の <xref:System.ComponentModel.CancelEventArgs.Cancel> プロパティを `True` に設定した場合、メソッドは `False` を返してアプリケーションの起動をキャンセルします。

3. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnRun%2A>. 初期化が完了した後、メインアプリケーションの実行を開始する準備ができたときの開始点を提供します。

     既定では、Windows フォームメッセージループに入る前に、このメソッドは `OnCreateMainForm` (アプリケーションのメインフォームを作成するため) を呼び出し、`HideSplashScreen` (スプラッシュスクリーンを閉じるために) メソッドを呼び出します。

    1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateMainForm%2A>. デザイナーがメインフォームを初期化するコードを出力する方法を提供します。

         既定では、このメソッドは何も行いません。 ただし、Visual Basic**プロジェクトデザイナー**でアプリケーションのメインフォームを選択すると、デザイナーは、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MainForm%2A> プロパティをメインフォームの新しいインスタンスに設定するメソッドを使用して、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateMainForm%2A> メソッドをオーバーライドします。

    2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.HideSplashScreen%2A>. アプリケーションにスプラッシュスクリーンが定義されていて、それが開いている場合、このメソッドはスプラッシュスクリーンを閉じます。

         既定では、このメソッドはスプラッシュスクリーンを閉じます。

4. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance%2A>. アプリケーションの別のインスタンスが起動したときの、単一インスタンスアプリケーションの動作をカスタマイズする方法を提供します。

     既定では、このメソッドは <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> イベントを発生させます。

5. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnShutdown%2A>. `Shutdown` イベントを発生させるための機能拡張ポイントを提供します。 メインアプリケーションでハンドルされない例外が発生した場合、このメソッドは実行されません。

     既定では、このメソッドは <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> イベントを発生させます。

6. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnUnhandledException%2A>. 上記のいずれかのメソッドでハンドルされない例外が発生した場合に実行されます。

     既定では、このメソッドは、デバッガーがアタッチされておらず、アプリケーションが `UnhandledException` イベントを処理している限り、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> イベントを発生させます。

 アプリケーションが単一インスタンスアプリケーションであり、アプリケーションが既に実行されている場合、アプリケーションの後続のインスタンスは、アプリケーションの元のインスタンスで <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance%2A> メソッドを呼び出し、終了します。

 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance(Microsoft.VisualBasic.ApplicationServices.StartupNextInstanceEventArgs)> コンストラクターは、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> プロパティを呼び出して、アプリケーションのフォームに使用するテキストレンダリングエンジンを決定します。 既定では、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> プロパティは `False`を返します。これは、GDI テキストレンダリングエンジンが使用されることを示します。これは Visual Basic 2005 以降のバージョンでは既定値です。 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> プロパティをオーバーライドして `True`を返すことができます。これは、Visual Basic .NET 2002 および Visual Basic .NET 2003 の既定値である GDI + テキストレンダリングエンジンを使用することを示します。

## <a name="configuring-the-application"></a>アプリケーションの構成

 Visual Basic アプリケーションモデルの一部として、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> クラスには、アプリケーションを構成する保護されたプロパティが用意されています。 これらのプロパティは、実装するクラスのコンストラクターで設定する必要があります。

 既定の Windows フォームプロジェクトでは、**プロジェクトデザイナー**は、デザイナー設定を使用してプロパティを設定するコードを作成します。 プロパティは、アプリケーションの起動時にのみ使用されます。アプリケーションの起動後に設定しても効果はありません。

|property|決まり|プロジェクトデザイナーの [アプリケーション] ウィンドウでの設定|
|---|---|---|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.IsSingleInstance%2A>|アプリケーションを単一インスタンスアプリケーションと複数インスタンスアプリケーションのどちらとして実行するかを指定します。|[**単一インスタンスアプリケーションを作成**する] チェックボックス|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.EnableVisualStyles%2A>|アプリケーションで Windows XP に一致する視覚スタイルを使用する場合はです。|**XP visual スタイルを有効にする**チェックボックス|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.SaveMySettingsOnExit%2A>|アプリケーションが終了したときに、アプリケーションのユーザー設定の変更を自動的に保存する場合は。|**[シャットダウン時に設定を保存**する] チェックボックス|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShutdownStyle%2A>|スタートアップフォームが閉じたときや、最後のフォームが閉じたときなど、アプリケーションが終了する原因。|**シャットダウンモード**の一覧|

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>
- [Visual Basic アプリケーション モデルの概要](../../../visual-basic/developing-apps/development-with-my/overview-of-the-visual-basic-application-model.md)
- [[アプリケーション] ページ (プロジェクト デザイナー)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
