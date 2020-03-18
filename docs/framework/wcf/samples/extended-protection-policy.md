---
title: 拡張保護ポリシー
ms.date: 03/30/2017
ms.assetid: e2616a10-317e-4c34-8023-0c015a80a82f
ms.openlocfilehash: 1cb6d44e8f6ee8f54f776453e5a1783ab0cfa4f0
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716422"
---
# <a name="extended-protection-policy"></a>拡張保護ポリシー
拡張保護は、man-in-the-middle (MITM) 攻撃に対するセキュリティ イニシアチブです。 MITM 攻撃はセキュリティの脅威です。MITM は、クライアントの資格情報を取得し、その資格情報をサーバーに転送します。  
  
## <a name="demonstrates"></a>例  
 拡張保護  
  
## <a name="discussion"></a>ディスカッション  
 アプリケーションが HTTPS で Kerberos、Digest または NTLM を使用して認証を実行する場合、最初にトランスポート レベルのセキュリティ (TLS) チャネルが構築され、その後セキュリティ チャネルを使用して認証が行われます。 しかし、SSL によって生成されるセッション キーと認証時に生成されるセッション キーはバインドされていません。 サーバーはセキュリティ チャネルがクライアントまたは MITM のどちらで確立されたものであるか知ることができないため、トランスポート チャネルそのものはセキュリティで保護されていても、MITM がクライアントとサーバーの間に自身を構築し、クライアントからの要求の転送を開始する可能性があります。 このシナリオの解決策として、外部の TLS チャネルを内部の認証チャネルにバインドする方法があります。この方法により、サーバーは、MITM が存在するかどうかを検出できます。  
  
> [!NOTE]
> このサンプルは、IIS でホストされている場合にのみ機能します。Cassini (Visual Studio Development Server) は HTTPS をサポートしないため、Cassini 上では機能しません。  
  
> [!NOTE]
> この機能は現在 Windows 7 でのみ使用できます。 次の手順は、Windows 7 でのみ実行できます。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. **[コントロールパネル]** 、 **[プログラムの追加と削除]** 、 **[Windows の機能]** からインターネットインフォメーションサービスをインストールします。  
  
2. Windows の**機能**、**インターネットインフォメーションサービス**、 **World Wide Web サービス**、**セキュリティ**、および**windows 認証**に**windows 認証**をインストールします。  
  
3. **Windows の機能**、 **Microsoft .NET Framework 3.5.1**、 **Windows Communication Foundation HTTP アクティブ化**に**Windows Communication Foundation http アクティブ化**をインストールします。  
  
4. このサンプルを実行するには、サーバーとの安全なチャネルを確立する必要のあるクライアントが必要なので、インターネット インフォメーション サービス (IIS) マネージャーからインストールすることのできるサーバー証明書が存在する必要があります。  
  
    1. IIS マネージャーを開きます。 **[サーバー証明書]** を開きます。ルートノード (コンピューター名) を選択すると、 **[機能ビュー]** タブに表示されます。  
  
    2. このサンプルをテストする目的で、自己署名証明書を作成します。 インターネット エクスプローラーで証明書の安全性に関するプロンプトが表示されないようにする場合は、信頼されたルート証明機関ストアに証明書をインストールします。  
  
5. 既定の Web サイトの **[操作]** ウィンドウを開きます。 [**サイト**と**バインド**の編集] をクリックします。 HTTPS が存在しない場合は、ポート番号を 443 に指定して HTTPS を追加します。 前の手順で作成した SSL 証明書を割り当てます。  
  
6. サービスをビルドします。 これにより、仮想ディレクトリが IIS に作成され、サービスを Web ホストにするために必要な .dll ファイル、.svc ファイルおよび .config ファイルがコピーされます。  
  
7. IIS マネージャーを開きます。 前の手順で作成した仮想ディレクトリ (**Extendedprotection**) を右クリックします。 **[アプリケーションに変換]** を選択します。  
  
8. この仮想ディレクトリの**認証**モジュールを IIS マネージャーで開き、 **Windows 認証**を有効にします。  
  
9. この仮想ディレクトリの **[Windows 認証]** の **[詳細設定]** を開き、 **[必須]** に設定します。  
  
10. ブラウザー ウィンドウから HTTPS URL にアクセスして (完全修飾ドメイン名を指定します) サービスをテストできます。 リモート コンピューターからこの URL にアクセスする場合は、すべての受信 HTTP および HTTPS 通信に対してファイアウォールが開かれていることを確認してください。  
  
11. クライアント構成ファイルを開き、クライアントの完全修飾ドメイン名、または `<<full_machine_name>>` を置き換えるエンドポイント アドレス属性を指定します。  
  
12. クライアントを実行します。 クライアントはサービスと通信します。これで、安全なチャネルが確立され、拡張保護が使用されます。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Security\ExtendedProtection`
