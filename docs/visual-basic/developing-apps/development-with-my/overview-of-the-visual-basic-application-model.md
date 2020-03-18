---
title: Visual Basic アプリケーション モデルの概要
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application object [Visual Basic], Visual Basic application model
- Visual Basic application model
ms.assetid: 17538984-84fe-43c9-82c8-724c9529fe8b
ms.openlocfilehash: aa47304cf2bded93bdb95ffe7dfa35bb37d9a643
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976457"
---
# <a name="overview-of-the-visual-basic-application-model"></a>Visual Basic アプリケーション モデルの概要

Visual Basic は、Windows フォームアプリケーション (Visual Basic アプリケーションモデル) の動作を制御するための適切に定義されたモデルを提供します。 このモデルには、アプリケーションの起動とシャットダウンを処理するためのイベントと、未処理の例外をキャッチするためのイベントが含まれています。 また、単一インスタンスアプリケーションを開発するためのサポートも提供します。 アプリケーションモデルは拡張可能であるため、より多くの制御を必要とする開発者は、オーバーライド可能なメソッドをカスタマイズできます。  
  
## <a name="uses-for-the-application-model"></a>アプリケーションモデルの使用  

 一般的なアプリケーションでは、起動時およびシャットダウン時にタスクを実行する必要があります。 たとえば、起動時にアプリケーションでスプラッシュスクリーンの表示、データベース接続の確立、保存された状態の読み込みなどを行うことができます。 アプリケーションがシャットダウンされると、データベース接続を閉じたり、現在の状態を保存したりできます。 また、アプリケーションは、ハンドルされない例外の発生中など、アプリケーションが予期せずシャットダウンしたときに、特定のコードを実行できます。  
  
 Visual Basic アプリケーションモデルを使用すると、*単一インスタンス*アプリケーションを簡単に作成できます。 単一インスタンスアプリケーションは、アプリケーションの1つのインスタンスだけを同時に実行できるという点で、通常のアプリケーションとは異なります。 単一インスタンスアプリケーションの別のインスタンスを起動しようとすると、元のインスタンスに対して、別の起動が試行されたことを `StartupNextInstance` イベントによって通知されます。 通知には、後続のインスタンスのコマンドライン引数が含まれます。 その後、アプリケーションの後続のインスタンスは、初期化が発生する前に閉じられます。  
  
 単一インスタンスアプリケーションが開始され、それが最初のインスタンスであるか、またはアプリケーションの後続のインスタンスであるかが確認されます。  
  
- 最初のインスタンスの場合は、通常どおりに起動します。  
  
- その後、最初のインスタンスの実行中にアプリケーションを起動しようとするたびに、動作が大きく異なります。 後続の試行では、最初のインスタンスに対してコマンドライン引数について通知した後、すぐに終了します。 最初のインスタンスは、`StartupNextInstance` イベントを処理して、後続のインスタンスのコマンドライン引数を判別し、実行を継続します。  
  
     次の図は、後続のインスタンスが最初のインスタンスを通知する方法を示しています。  
  
     ![単一インスタンスアプリケーションイメージを示す図。](./media/overview-of-the-visual-basic-application-model/single-instance-application.gif)  
  
 `StartupNextInstance` イベントを処理することによって、単一インスタンスアプリケーションの動作を制御できます。 たとえば、Microsoft Outlook は通常、単一インスタンスのアプリケーションとして実行されます。Outlook の実行中に Outlook を再度起動しようとすると、元のインスタンスにフォーカスが移りますが、別のインスタンスは開かれません。  
  
## <a name="events-in-the-application-model"></a>アプリケーションモデルのイベント  

 アプリケーションモデルには、次のイベントがあります。  
  
- **アプリケーションの起動**。 アプリケーションは、起動時に <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> イベントを発生させます。 このイベントを処理することにより、メインフォームが読み込まれる前にアプリケーションを初期化するコードを追加できます。 また、`Startup` イベントは、必要に応じて、スタートアッププロセスのそのフェーズ中にアプリケーションの実行をキャンセルするためにも提供します。  
  
     アプリケーションのスタートアップコードの実行中にスプラッシュスクリーンを表示するようにアプリケーションを構成できます。 既定では、`/nosplash` または `-nosplash` のいずれかのコマンドライン引数を使用すると、アプリケーションモデルによってスプラッシュスクリーンが抑制されます。  
  
- **単一インスタンスアプリケーション**。 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> イベントは、単一インスタンスアプリケーションの後続のインスタンスが開始されたときに発生します。 イベントは、後続のインスタンスのコマンドライン引数を渡します。  
  
- **未処理の例外**。 アプリケーションでハンドルされない例外が発生すると、<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> イベントが発生します。 そのイベントのハンドラーは、例外を調べて、実行を継続するかどうかを判断できます。  
  
     `UnhandledException` イベントは、状況によっては発生しません。 詳細については、「<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>」を参照してください。  
  
- **ネットワーク接続の変更**。 コンピューターのネットワークの可用性が変更されると、アプリケーションによって <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged> イベントが発生します。  
  
     `NetworkAvailabilityChanged` イベントは、状況によっては発生しません。 詳細については、「<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>」を参照してください。  
  
- **アプリケーションをシャットダウン**します。 アプリケーションは、シャットダウンしようとしているときに通知する <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> イベントを提供します。 このイベントハンドラーでは、アプリケーションが実行する必要のある操作 (たとえば、終了と保存など) が完了していることを確認できます。 メインフォームが閉じられたときにシャットダウンするようにアプリケーションを構成することも、すべてのフォームが閉じるときにのみシャットダウンするように構成することもできます。  
  
## <a name="availability"></a>可用性  

 既定では、Visual Basic アプリケーションモデルは Windows フォームプロジェクトで使用できます。 別のスタートアップオブジェクトを使用するようにアプリケーションを構成した場合、またはカスタム `Sub Main`を使用してアプリケーションコードを開始する場合は、そのオブジェクトまたはクラスが、アプリケーションモデルを使用するための <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> クラスの実装を提供する必要がある場合があります。 スタートアップオブジェクトの変更の詳細については、「 [[アプリケーション] ページ (プロジェクトデザイナー) (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- [Visual Basic アプリケーション モデルの拡張](../../../visual-basic/developing-apps/customizing-extending-my/extending-the-visual-basic-application-model.md)
