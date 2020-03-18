---
title: フェデレーション サンプル
ms.date: 03/30/2017
ms.assetid: 7e9da0ca-e925-4644-aa96-8bfaf649d4bb
ms.openlocfilehash: c143ad9315df94a2d836a2c2c742ffa9a7728511
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837884"
---
# <a name="federation-sample"></a>フェデレーション サンプル
このサンプルではフェデレーション セキュリティを示します。  
  
## <a name="sample-details"></a>サンプルの詳細  
 Windows Communication Foundation (WCF) では、`wsFederationHttpBinding`を使用したフェデレーションセキュリティアーキテクチャの展開がサポートされています。 `wsFederationHttpBinding` は、セキュリティで保護された、信頼できる、相互運用が可能なバインディングを提供します。このバインディングでは、要求/応答の通信のための基になるトランスポート機構として HTTP を使用でき、エンコーディングのためのワイヤ形式として Text/XML を使用できます。 WCF でのフェデレーションの詳細については、「[フェデレーション](../../../../docs/framework/wcf/feature-details/federation.md)」を参照してください。  
  
 シナリオは、次の 4 つの部分から構成されます。  
  
- BookStore サービス  
  
- BookStore STS  
  
- HomeRealm STS  
  
- BookStore クライアント  
  
 BookStore サービスは、`BrowseBooks` と `BuyBook` の 2 つの操作をサポートします。 サービスでは、`BrowseBooks` 操作には匿名アクセスできますが、`BuyBooks` 操作にアクセスするには認証済みのアクセス権限が必要です。 認証は、BookStore STS によって発行されたトークンの形式を取ります。 BookStore サービス用の構成ファイルは、次のように `wsFederationHttpBinding` を使用して、クライアントを BookStore STS にポイントします。  
  
```xml  
<wsFederationHttpBinding>  
<!-- This is the Service binding for the BuyBooks endpoint. It redirects clients to the BookStore STS -->  
    <binding name='BuyBookBinding'>  
        <security mode="Message">  
            <message>  
                <issuerMetadata  
  address='http://localhost/FederationSample/BookStoreSTS/STS.svc/mex' >  
                    <identity>  
                        <dns value ='BookStoreSTS.com'/>  
                    </identity>  
                </issuerMetadata>  
            </message>  
        </security>  
    </binding>  
</wsFederationHttpBinding>  
```  
  
 次に BookStore STS は、HomeRealm STS によって発行されたトークンを使用した、クライアントの認証を要求します。 ここでも、BookStore STS 用の構成ファイルは、`wsFederationHttpBinding` を使用して、クライアントを HomeRealm STS にポイントします。  
  
```xml  
<wsFederationHttpBinding>  
 <!-- This is the binding for the clients requesting tokens from this STS. It redirects clients to the HomeRealm STS -->  
    <binding name='BookStoreSTSBinding'>  
        <security mode='Message'>  
            <message>  
                <issuerMetadata  
 address='http://localhost/FederationSample/HomeRealmSTS/STS.svc/mex' >  
                    <identity>  
                        <dns value ='HomeRealmSTS.com' />  
                    </identity>  
                </issuerMetadata>  
            </message>  
        </security>  
    </binding>  
</wsFederationHttpBinding>  
```  
  
 `BuyBook` 操作にアクセスするときに発生するイベントの順序は、次のとおりです。  
  
1. クライアントは、Windows 資格情報を使用して HomeRealm STS に対する認証を行います。  
  
2. HomeRealm STS は、BookStore STS に対する認証に使用できるトークンを発行します。  
  
3. クライアントは、HomeRealm STS によって発行されたトークンを使用して BookStore STS に対する認証を行います。  
  
4. BookStore STS は、BookStore サービスに対する認証に使用できるトークンを発行します。  
  
5. クライアントは、BookStore STS によって発行されたトークンを使用して BookStore サービスに対する認証を行います。  
  
6. クライアントは `BuyBook` 操作にアクセスします。  
  
 このサンプルの設定および実行方法については、次の手順を参照してください。  
  
> [!NOTE]
> このサンプルを実行するには、 **wwwroot**ディレクトリに対する書き込みアクセス許可が必要です。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. SDK コマンド ウィンドウを開きます。 Setup.bat をサンプルのパスで実行します。 これにより、サンプルに必要な仮想ディレクトリが作成され、必要な証明書が適切な権限を付与されてインストールされます。  
  
    > [!NOTE]
    > Setup.bat バッチ ファイルは、Windows SDK コマンド プロンプトから実行します。 MSSDK 環境変数が SDK のインストール ディレクトリを指している必要があります。 この環境変数は、Windows SDK コマンド プロンプトで自動設定されます。 Windows Vista では、セットアップで IIS 管理者スクリプトが使用されるため、IIS 6.0 管理互換がインストールされていることを確認する必要があります。 Windows Vista でセットアップスクリプトを実行するには、管理者特権が必要です。  
  
2. Visual Studio で FederationSample を開き、 **[ビルド]** メニューの **[ソリューションのビルド]** をクリックします。 これによって共通のプロジェクト ファイル、Bookstore サービス、Bookstore STS、および HomeRealm STS が作成され、IIS に展開されます。 さらに Bookstore クライアント アプリケーションがビルドされ、FederationSample\BookStoreClient\bin\Debug フォルダーに実行可能ファイル BookStoreClient.exe が配置されます。  
  
3. BookStoreClient.exe をダブルクリックします。 BookStoreClient ウィンドウが表示されます。  
  
4. 書店で利用可能な書籍を参照するには、 **[参照]** をクリックします。  
  
5. 特定の本を購入するには、一覧で本を選択し、 **[Book book]** をクリックします。 アプリケーションが起動し、HomeRealm セキュリティ トークン サービスを使用した Windows 認証によって認証を行います。  
  
     サンプルは、ユーザーが 15 ドル以下の本を購入できるように構成されています。 15 ドルを超える本を購入しようとすると、クライアントは、BookStore サービスからアクセス拒否のメッセージを受け取ります。  
  
    > [!NOTE]
    > サンプルでは、購入後にユーザーの与信限度額を更新しません。 ユーザーは、(固定の) 与信限度額以内なら何度でも本を購入できます。  
  
#### <a name="to-clean-up"></a>クリーンアップするには  
  
1. Cleanup.bat を実行します。 これによって、設定中に作成された仮想ディレクトリが削除されます。同時に、設定中にインストールされた証明書も削除されます。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\Federation`  
