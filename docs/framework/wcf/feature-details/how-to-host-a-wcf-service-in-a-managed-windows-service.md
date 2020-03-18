---
title: '方法 : マネージド Windows サービスで WCF サービスをホストする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8e37363b-4dad-4fb6-907f-73c30fac1d9a
ms.openlocfilehash: 698a5134683341fedf2a37f7d6383770e14c232c
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964796"
---
# <a name="how-to-host-a-wcf-service-in-a-managed-windows-service"></a>方法 : マネージド Windows サービスで WCF サービスをホストする

このトピックでは、Windows サービスによってホストされる Windows Communication Foundation (WCF) サービスを作成するために必要な基本的な手順について説明します。 このシナリオは、メッセージがアクティブ化されていないセキュリティで保護された環境でインターネットインフォメーションサービス (IIS) の外部でホストされている長時間実行される WCF サービスである、マネージ Windows サービスホストオプションによって有効になります。 サービスの有効期限は代わりにオペレーティング システムによって制御されます。 このホスト オプションは Windows のすべてのバージョンで使用できます。

Windows サービスは、Microsoft 管理コンソール (MMC) の Microsoft.ManagementConsole.SnapIn を使用して管理し、システムのブート時に自動的に起動するように構成できます。 このホストオプションは、WCF サービスをホストするアプリケーションドメイン (AppDomain) をマネージ Windows サービスとして登録することで構成されます。これにより、サービスのプロセス有効期間は Windows サービス用のサービスコントロールマネージャー (SCM) によって制御されます。

サービス コードには、サービス コントラクトのサービス実装、Windows サービス クラス、およびインストーラー クラスが含まれています。 サービス実装クラス `CalculatorService`は、WCF サービスです。 一方、`CalculatorWindowsService` は Windows サービスです。 Windows サービスとして限定するため、このクラスは `ServiceBase` を継承し、`OnStart` メソッドと `OnStop` メソッドを実装しています。 `OnStart` では、<xref:System.ServiceModel.ServiceHost> 型の `CalculatorService` が作成され、開かれます。 `OnStop` では、このサービスが停止され、破棄されます。 ホストはベース アドレスをサービス ホストに提供する必要もあります。サービス ホストは、アプリケーション設定で構成されます。 インストーラー クラスは <xref:System.Configuration.Install.Installer> を継承します。このクラスを使用すると、Installutil.exe ツールにより、プログラムを Windows サービスとしてインストールできます。

## <a name="construct-the-service-and-provide-the-hosting-code"></a>サービスを構築してホスティング コードを提供する

1. **Service**という名前の新しい Visual Studio**コンソールアプリ**プロジェクトを作成します。

2. Program.cs を Service.cs に変更します。

3. 名前空間を `Microsoft.ServiceModel.Samples`に変更します。

4. 次のアセンブリへの参照を追加します。

    - System.ServiceModel.dll

    - System.ServiceProcess.dll

    - System.Configuration.Install.dll

5. 次の using ステートメントを Service.cs に追加します。

     [!code-csharp[c_HowTo_HostInNTService#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#0)]
     [!code-vb[c_HowTo_HostInNTService#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#0)]

6. 次のコードに示すように、`ICalculator` サービス コントラクトを定義します。

     [!code-csharp[c_HowTo_HostInNTService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#1)]
     [!code-vb[c_HowTo_HostInNTService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#1)]

7. 次のコードに示すように、`CalculatorService` というクラスにサービス コントラクトを実装します。

     [!code-csharp[c_HowTo_HostInNTService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#2)]
     [!code-vb[c_HowTo_HostInNTService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#2)]

8. `CalculatorWindowsService` クラスから継承する <xref:System.ServiceProcess.ServiceBase> という新しいクラスを作成します。 `serviceHost` というローカル変数を追加して、<xref:System.ServiceModel.ServiceHost> インスタンスを参照します。 `Main` を呼び出す `ServiceBase.Run(new CalculatorWindowsService)` というメソッドを定義します。

     [!code-csharp[c_HowTo_HostInNTService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#3)]
     [!code-vb[c_HowTo_HostInNTService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#3)]

9. 次のコードに示すように、新しい <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> インスタンスを作成して開くことによって <xref:System.ServiceModel.ServiceHost> メソッドをオーバーライドします。

     [!code-csharp[c_HowTo_HostInNTService#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#4)]
     [!code-vb[c_HowTo_HostInNTService#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#4)]

10. 次のコードに示すように、<xref:System.ServiceProcess.ServiceBase.OnStop%2A> を閉じて <xref:System.ServiceModel.ServiceHost> メソッドをオーバーライドします。

     [!code-csharp[c_HowTo_HostInNTService#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#5)]
     [!code-vb[c_HowTo_HostInNTService#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#5)]

11. `ProjectInstaller` から継承し、<xref:System.Configuration.Install.Installer> に設定された <xref:System.ComponentModel.RunInstallerAttribute> でマークされた `true` という新しいクラスを作成します。 これで、Installutil.exe ツールで Windows サービスをインストールできるようになります。

     [!code-csharp[c_HowTo_HostInNTService#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#6)]
     [!code-vb[c_HowTo_HostInNTService#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#6)]

12. プロジェクトを作成したときに生成された `Service` クラスを削除します。

13. アプリケーション構成ファイルをプロジェクトに追加します。 ファイルの内容を次の構成 XML に置き換えます。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <system.serviceModel>    <services>
          <!-- This section is optional with the new configuration model
               introduced in .NET Framework 4. -->
          <service name="Microsoft.ServiceModel.Samples.CalculatorService"
                   behaviorConfiguration="CalculatorServiceBehavior">
            <host>
              <baseAddresses>
                <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
              </baseAddresses>
            </host>
            <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service  -->
            <endpoint address=""
                      binding="wsHttpBinding"
                      contract="Microsoft.ServiceModel.Samples.ICalculator" />
            <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
            <endpoint address="mex"
                      binding="mexHttpBinding"
                      contract="IMetadataExchange" />
          </service>
        </services>
        <behaviors>
          <serviceBehaviors>
            <behavior name="CalculatorServiceBehavior">
              <serviceMetadata httpGetEnabled="true"/>
              <serviceDebug includeExceptionDetailInFaults="False"/>
            </behavior>
          </serviceBehaviors>
        </behaviors>
      </system.serviceModel>
    </configuration>
    ```

     **ソリューションエクスプローラー**で app.config ファイルを右クリックし、 **[プロパティ]** を選択します。 **[出力ディレクトリにコピー]** で、 **[新しい場合はコピー]** する を選択します。

     この例では、構成ファイルにエンドポイントを明示的に指定します。 エンドポイントをサービスに追加しない場合、ランタイムによって既定のエンドポイントが追加されます。 この例では、サービスには <xref:System.ServiceModel.Description.ServiceMetadataBehavior> に設定された `true` があるので、サービスで公開メタデータも有効化されています。 既定のエンドポイントについては、「[Simplified Configuration](../../../../docs/framework/wcf/simplified-configuration.md)」 (簡易構成) と「[Simplified Configuration for WCF Services](../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md)」 (WCF サービスの簡易構成) を参照してください。

## <a name="install-and-run-the-service"></a>サービスをインストールして実行する

1. ソリューションをビルドして `Service.exe` 実行可能ファイルを作成します。

2. Visual Studio の開発者コマンドプロンプトを開き、プロジェクトディレクトリに移動します。 コマンド プロンプトで「`installutil bin\service.exe`」と入力して、Windows サービスをインストールします

     コマンド プロンプトで「`services.msc`」と入力してサービス コントロール マネージャー (SCM) にアクセスします。 Windows サービスは、[Services] に "WCFWindowsServiceSample" として表示されます。 WCF サービスは、Windows サービスが実行されている場合にのみ、クライアントに応答できます。 サービスを開始するには、SCM でそのサービスを右クリックし、[開始] を選択するか、コマンドプロンプトで「 **net Start WCFWindowsServiceSample** 」と入力します。

3. サービスを変更する場合は、まずそのサービスを停止してからアンインストールする必要があります。 サービスを停止するには、SCM でサービスを右クリックし、[停止] を選択するか、コマンドプロンプトで「 **net Stop WCFWindowsServiceSample** 」と入力します。 Windows サービスを停止してクライアントを実行すると、クライアントがこのサービスにアクセスしようとしたときに <xref:System.ServiceModel.EndpointNotFoundException> 例外が発生することに注意してください。 Windows サービスをアンインストールするには、コマンドプロンプトで「 **installutil.exe/u bin\service.exe** 」と入力します。

## <a name="example"></a>使用例

このトピックで使用されているコードの完全な一覧を次に示します。

[!code-csharp[c_HowTo_HostInNTService#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#8)]
[!code-vb[c_HowTo_HostInNTService#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#8)]

"自己ホスト" オプションと同様、Windows サービス ホスト環境では、ホスト コードをアプリケーションの一部として記述する必要があります。 サービスはコンソール アプリケーションとして実装され、独自のホスティング コードが指定されます。 インターネット インフォメーション サービス (IIS) でホストされる Windows プロセス アクティブ化サービス (WAS) など、他のホスト環境ではホスティング コードを記述する必要はありません。

## <a name="see-also"></a>関連項目

- [簡略化された構成](../../../../docs/framework/wcf/simplified-configuration.md)
- [マネージド アプリケーションのホスト](../../../../docs/framework/wcf/feature-details/hosting-in-a-managed-application.md)
- [ホスティング サービス](../../../../docs/framework/wcf/hosting-services.md)
- [AppFabric のホスティング機能](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
