---
title: 構成を使用しない AJAX サービス
ms.date: 03/30/2017
ms.assetid: e6db7acd-5679-45d4-b98a-8449c6873838
ms.openlocfilehash: bf80f00bbca370c973dab9f20024284a465be521
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716205"
---
# <a name="ajax-service-without-configuration"></a>構成を使用しない AJAX サービス

このサンプルでは、Windows Communication Foundation (WCF) を使用して、構成を使用せずに、基本的な ASP.NET 非同期 JavaScript and XML (AJAX) サービス (Web ブラウザークライアントから JavaScript コードを使用してアクセスできるサービス) を作成する方法を示します。設定。 このサービスは .svc ファイルの特殊な構文を使用して AJAX エンドポイントを自動的に有効にします。

WCF での AJAX サポートは、`ScriptManager` コントロールを介して ASP.NET AJAX で使用できるように最適化されています。 ASP.NET AJAX で WCF を使用する例については、 [ajax のサンプル](ajax.md)を参照してください。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

 このサンプルは、HTTP POST を使用した AJAX サービスに基づいています。 「[基本的な AJAX サービス](../../../../docs/framework/wcf/samples/basic-ajax-service.md)のサンプル」で説明されているように、サービスをホストするために <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> を使用します。

```text
<%ServiceHost
    language=c#
    Debug="true"
    Service="Microsoft.Ajax.Samples.CalculatorService
    Factory="System.ServiceModel.Activation.WebScriptServiceHostFactory"
%>
```

<xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> は、<xref:System.ServiceModel.Description.WebScriptEndpoint> をサービスに自動的に追加します。 エンドポイントに対する構成変更が不要な場合、`<system.ServiceModel>` セクションはサービスの Web.config ファイルから完全に削除できます。 Web.config ファイルには、ConfigFreeClientPage.aspx で使用される ASP.NET 設定がいくつか含まれます。 そうでない場合は、Web.config ファイル全体を削除できます。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\ConfigFreeAjaxService`

#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. Windows Communication Foundation のサンプルについては、 [1 回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)に従ってセットアップ手順を実行してください。

2. 「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の説明に従って、ソリューション ConfigFreeAjaxService をビルドします。

3. `http://localhost/ServiceModelSamples/ConfigFreeClientPage.aspx` に移動します (プロジェクトディレクトリ内からブラウザーで Configfreeclientpage.aspx を開かないでください)。

> [!NOTE]
> このサンプルを実行する場合、IIS の ServiceModelSamples フォルダーで匿名認証と Windows 認証が同時に有効になっていないことを確認してください。 有効になっている場合は、Windows 認証を無効にしてください。 サンプルの実行が終了したら、Windows 認証を有効にし、"iisreset" を実行します。

## <a name="see-also"></a>参照

- [基本的な AJAX サービス](../../../../docs/framework/wcf/samples/basic-ajax-service.md)
