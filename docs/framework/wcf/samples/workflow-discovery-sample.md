---
title: ワークフローの探索のサンプル
ms.date: 03/30/2017
ms.assetid: 82cc43f1-3c8f-4771-ac19-a75ac936e2c3
ms.openlocfilehash: eafe031b71836eae8de5ce15cd669459c866e89f
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094762"
---
# <a name="workflow-discovery-sample"></a>ワークフローの探索のサンプル
このサンプルでは、ワークフロー サービスを探索可能にする方法と、特定のサービスを検索するカスタム コード アクティビティを作成する方法を示します。  
  
## <a name="demonstrates"></a>対象  
 探索検索アクティビティとワークフローの使用方法  
  
## <a name="discussion"></a>ディスカッション  
 サンプルの最初の部分では、構成を使用してワークフロー サービスを探索可能にしています。 また、カスタム メタデータ (スコープなど) と共に構成を使用すると、サービスを適切に適用することができます。 このサンプルは、クライアントでカスタム コード アクティビティを使用します。カスタム コード アクティビティは、探索を使用して、特定のコントラクトに一致するサービスを検索します。 コード アクティビティは URI を出力します。この URI は、後で送信アクティビティで使用されます。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. このサンプルでは、適切な URL Acl を実行する必要がある HTTP エンドポイントを使用します (詳細については、「 [http および HTTPS の構成](../feature-details/configuring-http-and-https.md)」を参照してください)。 権限のレベルが高いコマンド プロンプトで次のコマンドを実行すると、適切な ACL が追加されます。 シェルが変数形式を理解していない場合は、次の引数について、ドメインとユーザー名に置き換えます。  
  
     **netsh http add urlacl url =http://+:8000/ user =% DOMAIN%\\% UserName%**  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\WorkflowDiscovery`
