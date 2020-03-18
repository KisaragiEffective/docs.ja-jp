---
title: サービス デバッグ動作
ms.date: 03/30/2017
ms.assetid: 9d8fd3fb-dc39-427a-8235-336a7e7162ba
ms.openlocfilehash: 6a0a1c5d9f9741da978c633d35ea1e39664bed5b
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716303"
---
# <a name="service-debug-behavior"></a>サービス デバッグ動作

このサンプルでは、サービス デバッグ動作の設定を構成する方法を示します。 このサンプルは、`ICalculator` サービスコントラクトを実装する[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいています。 このサンプルは、サービス デバッグ動作を構成ファイルで明示的に定義します。 コードで強制的に定義することもできます。

この例では、クライアントはコンソール アプリケーション (.exe) であり、サービスはインターネット インフォメーション サービス (IIS) によってホストされます。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

サーバーの Web.config ファイルでは、次のサンプルに示すように、ヘルプ ページと例外処理を有効にするサービス デバッグ動作を定義します。

```xml
<behaviors>
     <serviceBehaviors>
         <behavior name="CalculatorServiceBehavior">
         <!-- WARNING: Setting includeExceptionDetailInFaults = "True" could result in leaking secured server information to the client.-->
         <!-- Please set this to false when deploying -->
             <serviceDebug includeExceptionDetailInFaults="True" httpHelpPageEnabled="True"/>
         </behavior>
     </serviceBehaviors>
</behaviors>
```

[\<servicedebug >](../../../../docs/framework/configure-apps/file-schema/wcf/servicedebug.md)は、サービスデバッグ動作のプロパティを変更できる構成要素です。 ユーザーはこの動作を変更して、次を実現することができます。

- 例外が <xref:System.ServiceModel.FaultContractAttribute> を使用して宣言されていない場合でも、サービスでは、アプリケーション コードによってスローされる例外を返すことができます。 これを行うには、`includeExceptionDetailInFaults` を `true` に設定します。 この設定は、サーバーが予期しない例外をスローしている場合のデバッグ時に役立ちます。

  > [!IMPORTANT]
  > この設定を本運用環境で有効にすると、セキュリティが不十分になります。 サーバーの予期しない例外には、クライアントを対象としていない情報が含まれる場合があるため、`includeExceptionDetailsInFaults` を `true` に設定すると、情報の漏えいが発生する可能性があります。

- [\<serviceDebug >](../../../../docs/framework/configure-apps/file-schema/wcf/servicedebug.md)では、ユーザーがヘルプページを有効または無効にすることもできます。 各サービスは、サービスの WSDL を取得するエンドポイントなど、サービスに関する情報が含まれるヘルプ ページをオプションで公開できます。 これを有効にするには、`httpHelpPageEnabled` プロパティを `true` に設定します。 これにより、サービスのベース アドレスへの GET 要求に対して、ヘルプ ページを返すことができます。 このアドレスは、別の属性 `httpHelpPageUrl` を設定して変更できます。 HTTP の代わりに HTTPS を使用すると、このアドレスをセキュリティ保護できます。 これを行うには、`httpsHelpPageEnabled` と `httpsHelpPageUrl` を設定します。

このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 最初の 3 つの操作 (加算、減算、乗算) が正常に行われる必要があります。 最後の操作 (除算) は、0 による除算の例外によってエラーとなります。

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Behaviors\ServiceDebug`
