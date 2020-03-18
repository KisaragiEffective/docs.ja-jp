---
title: '方法 : Visual Studio を構成して Web サービスを呼び出す XAML ブラウザー アプリケーションをデバッグする'
ms.date: 03/30/2017
helpviewer_keywords:
- debugging XBAPs that call a Web service [WPF]
- debugging security exceptions for XBAPs [WPF]
- security exception for XBAPs [WPF], debugging
- configuring Visual Studio to debug XAML browser applications [WPF]
- configuring Visual Studio to debug XBAPs [WPF]
ms.assetid: fd1db082-a7bb-4c4b-9331-6ad74a0682d0
ms.openlocfilehash: d8cfae2fb47876d578c51e5f4acdfe0c31e752fe
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460905"
---
# <a name="how-to-configure-visual-studio-to-debug-a-xaml-browser-application-to-call-a-web-service"></a>方法 : Visual Studio を構成して Web サービスを呼び出す XAML ブラウザー アプリケーションをデバッグする
XAML ブラウザーアプリケーション (Xbap) は、インターネットゾーンのアクセス許可セットに制限されている部分信頼セキュリティサンドボックス内で実行されます。 このアクセス許可セットは、Web サービスの呼び出しを、XBAP アプリケーションの起点サイトにある Web サービスのみに制限します。 ただし、XBAP が Visual Studio 2005 からデバッグされている場合、それが参照している Web サービスと同じ起点サイトであるとは見なされません。 これにより、XBAP が Web サービスを呼び出そうとすると、セキュリティ例外が発生します。 ただし、Visual Studio 2005 XAML ブラウザーアプリケーション (WPF) プロジェクトは、デバッグ中に呼び出される Web サービスと同じ起点サイトがあることをシミュレートするように構成できます。 これにより、XBAP はセキュリティ例外を発生させることなく、Web サービスを安全に呼び出すことができます。

## <a name="configuring-visual-studio"></a>Visual Studio の構成
 Web サービスを呼び出す XBAP をデバッグするように Visual Studio 2005 を構成するには、次のようにします。

1. **ソリューション エクスプローラー**でプロジェクトを選択し、 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。

2. **プロジェクトデザイナー**で、 **[デバッグ]** タブをクリックします。

3. **[開始アクション]** セクションで、 **[外部プログラムの開始]** を選択し、次のように入力します。

     `C:\WINDOWS\System32\PresentationHost.exe`

4. **[開始オプション]** セクションで、 **[コマンドライン引数]** ボックスに次の内容を入力します。

     `-debug`  *ファイル名*

     **-Debug**パラメーターの*filename*値は、. xbap ファイル名です。例えば：

     `-debug c:\example.xbap`

> [!NOTE]
> これは、Visual Studio 2005 XAML ブラウザーアプリケーション (WPF) プロジェクトテンプレートを使用して作成されたソリューションの既定の構成です。

1. **ソリューション エクスプローラー**でプロジェクトを選択し、 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。

2. **プロジェクトデザイナー**で、 **[デバッグ]** タブをクリックします。

3. **[開始オプション]** セクションで、次のコマンドラインパラメーターを **[コマンドライン引数]** テキストボックスに追加します。

     `-debugSecurityZoneURL`  *URL*

     **-Debugsecurityゾーン url**パラメーターの*url*値は、アプリケーションの元のサイトとしてシミュレートする場所の url ですが、

 例として、次の URL の Web サービスを使用する XAML ブラウザーアプリケーション (XBAP) を考えてみます。

 `http://services.msdn.microsoft.com/ContentServices/ContentService.asmx`

 この Web サービスの起点サイト URL は次のとおりです。

 `http://services.msdn.microsoft.com`

 その結果、完全な**debugsecurityゾーン url**コマンドラインパラメーターと値は次のようになります。

 `-debugSecurityZoneURL http://services.msdn.microsoft.com`

## <a name="see-also"></a>関連項目

- [WPF ホスト (PresentationHost.exe)](wpf-host-presentationhost-exe.md)
