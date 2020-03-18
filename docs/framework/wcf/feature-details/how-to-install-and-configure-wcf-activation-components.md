---
title: '方法 : WCF アクティブ化コンポーネントをインストールして設定する'
ms.date: 03/30/2017
helpviewer_keywords:
- HTTP activation [WCF]
ms.assetid: 33a7054a-73ec-464d-83e5-b203aeded658
ms.openlocfilehash: e71664b4361ba28a50b29499585b20a8adbaefd2
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964466"
---
# <a name="how-to-install-and-configure-wcf-activation-components"></a>方法 : WCF アクティブ化コンポーネントをインストールして設定する

このトピックでは、windows Vista で Windows プロセスアクティブ化サービス (WAS) をセットアップして、HTTP ネットワークプロトコルでは通信しない Windows Communication Foundation (WCF) サービスをホストするために必要な手順について説明します。 以降の各セクションで、この構成に関する手順について概説します。

- WCF アクティブ化コンポーネントをインストール (または、のインストールを確認) します。

- 非 HTTP プロトコルをサポートようにする WAS を構成します。 次の手順では、TCP ライセンス認証用に Windows Vista を構成します。

WAS をインストールして構成した後、「 [How to: Host a Wcf service IN was](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-was.md) 」を参照して、was を使用する非 HTTP エンドポイントを公開する wcf サービスを作成する手順を参照してください。

## <a name="to-install-the-wcf-non-http-activation-components"></a>WCF 非 HTTP アクティブ化コンポーネントをインストールするには

1. をクリックして、**開始**ボタンをクリックし、をクリックし、 **コントロール パネルの** します。

2. **[プログラム]** をクリックし、 **[プログラムと機能]** をクリックします。

3. **[タスク]** メニューの **[Windows の機能の有効化または無効化]** をクリックします。

4. WinFX ノードを見つけて、それを選択して展開します。

5. **[WCF 非 Http アクティブ化コンポーネント]** チェックボックスをオンにして、設定を保存します。

## <a name="to-configure-the-was-to-support-tcp-activation"></a>TCP アクティベーションをサポートするように WAS を構成するには

1. net.tcp アクティベーションをサポートするには、既定の Web サイトをあらかじめ net.tcp ポートにバインドしておく必要があります。 これを行うには、IIS 7.0 管理ツールセットと共にインストールされる Appcmd.exe を使用します。 管理者レベルのコマンド プロンプト ウィンドウで、次のコマンドを実行します。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']
    ```

    > [!NOTE]
    > このコマンドはテキスト 1 行です。 このコマンドは、net.tcp サイト バインディングを、TCP ポート 808 で任意のホスト名をリッスンする既定の Web サイトに追加します。

2. サイト内のすべてのアプリケーションが同じ net.tcp バインディングを共有しますが、net.tcp サポートの有効化はアプリケーションごとに指定できます。 アプリケーションで net.tcp を有効にするには、管理者レベルのコマンド プロンプトから、次のコマンドを実行します。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set app
    "Default Web Site/<WCF Application>" /enabledProtocols:http,net.tcp
    ```

    > [!NOTE]
    > このコマンドはテキスト 1 行です。 このコマンドは、`http://localhost/<WCF Application>` と `net.tcp://localhost/<WCF Application>`の両方を使用して、/\<*WCF アプリケーション*> アプリケーションにアクセスできるようにします。

     このサンプル用に追加した net.tcp サイト バインディングを削除します。

     便宜上次の 2 つの手順が、サンプル ディレクトリにある RemoveNetTcpSiteBinding.cmd というバッチ ファイルに実装されています。

    1. 管理者レベルのコマンド プロンプト ウィンドウで次のコマンドを実行して、有効なプロトコルの一覧から net.tcp を削除します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app
        "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

    2. 権限のレベルが高いコマンド プロンプト ウィンドウで次のコマンドを実行して、net.tcp サイト バインディングを削除します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        --bindings.[protocol='net.tcp',bindingInformation='808:*']
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

## <a name="to-remove-nettcp-from-the-list-of-enabled-protocols"></a>有効なプロトコルの一覧から net.tcp を削除するには

1. 有効なプロトコルの一覧から net.tcp を削除するには、管理者レベルのコマンド プロンプト ウィンドウで次のコマンドを実行します。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http
    ```

    > [!NOTE]
    > このコマンドはテキスト 1 行です。

## <a name="to-remove-the-nettcp-site-binding"></a>net.tcp サイト バインディングを削除するには

1. net.tcp サイト バインディングを削除するには、管理者レベルのコマンド プロンプト ウィンドウで次のコマンドを実行します。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
    -bindings.[protocol='net.tcp',bindingInformation='808:*']
    ```

    > [!NOTE]
    > このコマンドはテキスト 1 行です。

## <a name="see-also"></a>関連項目

- [TCP アクティベーション](../../../../docs/framework/wcf/samples/tcp-activation.md)
- [MSMQ アクティベーション](../../../../docs/framework/wcf/samples/msmq-activation.md)
- [NamedPipe アクティベーション](../../../../docs/framework/wcf/samples/namedpipe-activation.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
