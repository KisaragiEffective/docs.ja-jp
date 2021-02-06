---
description: '詳細情報: ローカルチャネル'
title: ローカル チャネル
ms.date: 03/30/2017
ms.assetid: fa1917a4-f701-4e82-a439-14a16282c7cc
ms.openlocfilehash: a580abc1e8dcd89c40c9fdc02001deb094eb0b05
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99631750"
---
# <a name="local-channel"></a>ローカル チャネル

ローカルチャネルは、同じアプリケーションドメイン内の通信に使用される Windows Communication Foundation (WCF) トランスポートチャネルです。 これは、クライアントとサービスが同じアプリケーション ドメイン内で実行されており、通常の WCF チャネル スタックのオーバーヘッド (メッセージのシリアル化と逆シリアル化) を回避する必要がある場合に役に立ちます。  
  
## <a name="demonstrates"></a>対象  

 ローカル チャネル  
  
## <a name="discussion"></a>ディスカッション  

 このサンプルは、2 つのプロジェクト ファイルで構成されます。  
  
- **Localchannel**: 現在のアプリケーションドメイン内のローカルチャネルのプログラムによる表現。 このプロジェクトでは、送信側コンポーネントがメッセージをメモリ内キューに入れて、受信側コンポーネントがメッセージをキューから削除して受信します。  
  
- **Clientandservice**: このプロジェクトは、コンソールアプリケーションでサービスをホストし、クライアントを実行して、同じアプリケーションドメイン内からサービスを呼び出します。  
  
 ローカル チャネルのデザインでは、速度を上げるためにチャネル スタックとシリアル化プロセスの両方がスキップされます。 ローカル トランスポート チャネルは、キューを使用してサービス呼び出しをクライアントからサービスに転送し、値をクライアントに返すことによって実装されます。 このサンプルでは、パラメーターと戻り値をシリアル化するのではなく、オブジェクトをコピーします。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. LocalChannel ソリューションをビルドして実行します。  
  
2. サービス ホストが起動し、クライアントがローカル チャネルを使用してサービスを呼び出します。 コンソール ウィンドウが表示され、サービス呼び出しの結果が示されます。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) とサンプルをダウンロードして [!INCLUDE[wf1](../../../../includes/wf1-md.md)] ください。 このサンプルは、次のディレクトリに格納されます。  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\LocalChannel`
