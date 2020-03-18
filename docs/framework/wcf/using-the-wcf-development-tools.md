---
title: WCF 開発ツールの使用
ms.date: 03/30/2017
ms.assetid: 054adb87-c244-4d5a-83d1-0b2b44bd454b
ms.openlocfilehash: 8253a9136b2310deeb7c6d162a9f190c13ba02da
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837728"
---
# <a name="using-the-wcf-development-tools"></a>WCF 開発ツールの使用
このセクションでは、WCFservice の開発に役立つ Visual Studio 開発ツールについて説明します。  
  
 Visual Studio テンプレートを基礎として使用して、独自のサービスをすばやく構築し、WCF サービスの自動ホストと WCF テストクライアントを使用してサービスをデバッグおよびテストできます。 これらのツールによって、高速でシームレスなデバッグとテストのサイクルが実現し、初期の段階でホスト モデルにコミットする必要がなくなります。  
 
 > [!NOTE]
 > Visual Studio 2017 以降では、WCF 開発ツールは既定でインストールされません。 これらの機能を使用するには、Visual Studio インストーラーで Windows Communication Foundation コンポーネントが選択されていることを確認する必要があります。
  
## <a name="the-wcf-developer-tools"></a>WCF の開発者用ツール  
 [WCF Visual Studio テンプレート](wcf-vs-templates.md)  
  
 Visual Studio の事前定義された Visual Studio プロジェクトと項目テンプレートを使用して、WCF サービスとその周辺アプリケーションをすばやく構築できます。  
  
 [WCF サービス ホスト (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)  
  
 WCF サービスの自動ホスト (Wcfsvchost.exe) を使用すると、Visual Studio デバッガー (F5) を起動して、実装したサービスを自動的にホストおよびテストすることができます。 その後、WCF テストクライアント (Wcftestclient.exe) または独自のクライアントを使用してサービスをテストし、潜在的なエラーを見つけて修正することができます。  
  
 [WCF のテスト用クライアント (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)  
  
 WCF テストクライアント (Wcftestclient.exe) は、任意の型のパラメーターを入力し、その入力をサービスに送信し、サービスから返される応答を表示するための GUI ツールです。 WCF サービスの自動ホストと組み合わせると、シームレスなサービステストエクスペリエンスが提供されます。  
  
 [XML からのデータ型クラスの生成](generating-data-type-classes-from-xml.md)  
  
 クリップボードに格納されている XML データは、コード ページに貼り付けることができます。 データで定義されているクラスは、コード型に変換されます。  
  
## <a name="using-the-tools-without-administrator-privilege"></a>管理特権を必要としないツールの使用  
 管理者特権を持たないユーザーが WCF サービスを開発できるようにするには、Visual Studio のインストール時に、名前空間 "http://+:8731/Design_Time_Addresses" に対して ACL (Access Control リスト) が作成されます。 この ACL は (UI) に設定され、コンピューターにログオンしているすべての対話ユーザーが含まれます。 管理者は、この ACL にユーザーを追加または削除したり、追加のポートを開いたりできます。この ACL によって、既定の構成で、WCF テンプレートまたは WF テンプレートでデータを送受信できるようになります。 また、ユーザーは、管理者特権を付与せずに、WCF サービスの自動ホスト (Wcfsvchost.exe) を使用することもできます。  
  
 アクセス権を変更するには、管理者特権で Windows Vista の Netsh.exe ツールを使用します。 Netsh.exe の使用例を次に示します。  
  
```console  
netsh http add urlacl url=http://+:8001/MyService user=<domain>\<user>  
```  
  
 Netsh.exe の詳細については、「 [Netsh.exe ツールとコマンドラインスイッチの使用方法](https://docs.microsoft.com/previous-versions/tn-archive/bb490939(v=technet.10))」を参照してください。  
  
## <a name="see-also"></a>参照

- [WCF Visual Studio テンプレート](wcf-vs-templates.md)
- [WCF サービス ホスト (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
- [WCF のテスト用クライアント (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
