---
title: WPF ホスト (PresentationHost.exe)
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- WPF Host application [WPF]
- PresentationHost.exe
ms.assetid: 3215bfa1-722c-4ac8-a7c5-bdd02d30afbd
ms.openlocfilehash: bda7efbb1b7a4760199215bdb58c12b3063e290c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743400"
---
# <a name="wpf-host-presentationhostexe"></a>WPF ホスト (PresentationHost.exe)
Windows Presentation Foundation (WPF) ホスト (プレゼンテーションホスト .exe) は、互換性のあるブラウザー (Microsoft Internet Explorer 6 以降を含む) で WPF アプリケーションをホストできるようにするアプリケーションです。 既定では、Windows Presentation Foundation (WPF) ホストは、ブラウザーでホストされる WPF コンテンツのシェルおよび MIME ハンドラーとして登録されます。これには次のものが含まれます。  
  
- Loose (コンパイルされていない) [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイル (.xaml)。  
  
- XAML ブラウザーアプリケーション (XBAP) (xbap)。  
  
 これらの種類のファイルについては、Windows Presentation Foundation (WPF) ホスト:  
  
- Windows Presentation Foundation (WPF) コンテンツをホストするために、登録されている HTML ハンドラーを起動します。  
  
- 必要な共通言語ランタイム (CLR) アセンブリと Windows Presentation Foundation (WPF) アセンブリの適切なバージョンを読み込みます。  
  
- 展開のゾーンに適切なアクセス許可レベルが設定されるようにします。  
  
 このトピックでは、PresentationHost.exe で使用できるコマンド ライン パラメーターについて説明します。  
  
## <a name="usage"></a>使用法  
 `PresentationHost.exe [parameters] uri|filename`  
  
## <a name="parameters"></a>パラメーター  
  
|パラメーター|[説明]|  
|---------------|-----------------|  
|filename|アクティブにするファイルのパス。 は URI でもかまいません。|  
|-debug|アプリケーションをアクティブにする場合に、このアプリケーションをストアにコミットしたり、ストアから実行しません。 これは、ローカル ファイルをアクティブにする場合に限って使用できます。|  
|-debugSecurityZoneURL \<url>|指定された URL からアプリケーションが配置されているかのように、アプリケーションをデバッグする必要があることを示すために、URL 値と共に使用されます。 これは、展開ゾーンと起点サイトの両方を決定します。|  
|-embedding|OLE で必要になります。 `-event` パラメーターまたは `-debug` パラメーターを指定した場合、`-embedding` パラメーターは内部で設定されるため、指定する必要はありません。|  
|-event \<eventname>|この名前のイベントを開き、プレゼンテーションの cluster.exe が初期化され、WPF コンテンツをホストする準備ができたら、そのイベントを通知します。 PresentationHost.exe は、イベントを開く際にエラーが発生すると (そのイベントがまだ作成されていない場合など) 終了します。|  
|-launchApplication \<url>|指定された URL からスタンドアロンの ClickOnce アプリケーションを起動します。 .NET アプリケーションに関する Internet Explorer と WinINet のセキュリティポリシーが適用されます。|  
  
## <a name="scenarios"></a>シナリオ  
  
### <a name="shell-handler"></a>シェル ハンドラー  
 `PresentationHost.exe example.xbap`  
  
### <a name="mime-handler"></a>MIME ハンドラー  
 `PresentationHost.exe -embedding example.xbap`  
  
### <a name="visual-studio-debugging"></a>Visual Studio によるデバッグ  
 `PresentationHost.exe -debug example.xbap`  
  
### <a name="visual-studio-debugging-in-zone"></a>Visual Studio によるゾーンでのデバッグ  
 `PresentationHost.exe -debug -debugSecurityZoneURL http://www.example.com c:\folderpath\example.xbap`  
  
## <a name="see-also"></a>参照

- [セキュリティ](../security-wpf.md)
