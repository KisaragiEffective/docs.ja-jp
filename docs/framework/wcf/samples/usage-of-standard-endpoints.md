---
title: 標準エンドポイントの使用
ms.date: 03/30/2017
ms.assetid: ecd6a62f-9619-4778-a497-6f888087a9ea
ms.openlocfilehash: 2b210bfe683669aeebf54a1701f07d492e6abdb4
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715339"
---
# <a name="usage-of-standard-endpoints"></a>標準エンドポイントの使用

このサンプルでは、サービスの構成ファイルでの標準エンドポイントの使用方法を示します。 標準エンドポイントでは、単一のプロパティを使用してアドレス、バインディング、およびコントラクトの組み合わせをそのエンドポイントに関連付けられている追加のプロパティで記述することで、エンドポイントの定義を簡単に行うことができます。 このサンプルでは、カスタムの標準エンドポイントを定義して実装する方法、およびエンドポイントの特定のプロパティを定義する方法を示します。

## <a name="sample-details"></a>サンプルの詳細

サービス エンドポイントは、アドレス、バインディング、およびコントラクトの 3 つのパラメーターを指定して指定できます。 指定できるその他のパラメーターには、動作構成、ヘッダー、リッスン URI などがあります。 アドレス、バインディング、およびコントラクトのいずれかまたはすべてに、変更できない値が含まれている場合があります。 このような理由から、標準エンドポイントを使用できます。 このようなエンドポイントの例として、メタデータ交換エンドポイントや探索エンドポイントがあります。 また、標準エンドポイントでは、固定された性質の情報を提供したり、独自の標準エンドポイントを作成したりしなくても、サービス エンドポイントを構成できるため、ユーザビリティが向上します。たとえば、適切な既定の値のセットを指定し、構成ファイルの詳細を削減してユーザビリティを向上させることができます。

このサンプルは、2 つの標準エンドポイントを定義するサービスおよびサービスと通信するクライアントの 2 つのプロジェクトで構成されています。 構成ファイルでサービスの標準エンドポイントを定義する方法を次の例に示します。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <extensions>
      <endpointExtensions>
        <add name="customEndpoint" type="Microsoft.Samples.StandardEndpoints.CustomEndpointCollectionElement, service" />
      </endpointExtensions>
    </extensions>
    <services>
      <service name="Microsoft.Samples.StandardEndpoints.CalculatorService">
        <endpoint address="http://localhost:8000/Samples/Service.svc/customEndpoint" contract="Microsoft.Samples.StandardEndpoints.ICalculator" kind="customEndpoint" />
        <endpoint kind="mexEndpoint" />
      </service>
    </services>
    <behaviors>
      <serviceBehaviors>
        <behavior>
          <serviceMetadata httpGetEnabled="True"/>
        </behavior>
      </serviceBehaviors>
    </behaviors>
    <standardEndpoints>
      <customEndpoint>
        <standardEndpoint property="True" />
      </customEndpoint>
    </standardEndpoints>
  </system.serviceModel>
</configuration>
```

サービスに定義されている最初のエンドポイントは、`customEndpoint` の一種で、その定義については、`<standardEndpoints>` セクションで確認できます。このセクションでは、プロパティ `property` に `true` の値が指定されています。 これは、新しいプロパティでカスタマイズされたエンドポイントの例です。 2 つ目のエンドポイントはメタデータ エンドポイントを表します。このエンドポイントでは、アドレス、バインディング、およびコントラクトの値が固定されています。

標準エンドポイント要素を定義するには、`StandardEndpointElement` から派生するクラスを作成する必要があります。 このサンプルでは、`CustomEndpointElement` クラスは、次の例のように定義されています。

```csharp
public class CustomEndpointElement : StandardEndpointElement
{
    public bool Property
    {
        get { return (bool)base["property"]; }
        set { base["property"] = value; }
    }

    protected override ConfigurationPropertyCollection Properties
    {
        get
        {
            ConfigurationPropertyCollection properties = base.Properties;
            properties.Add(new ConfigurationProperty("property", typeof(bool), false, ConfigurationPropertyOptions.None));
            return properties;
        }
    }

    protected override Type EndpointType
    {
        get { return typeof(CustomEndpoint); }
    }

    protected override ServiceEndpoint CreateServiceEndpoint(ContractDescription contract)
    {
        return new CustomEndpoint();
    }

    protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ServiceEndpointElement serviceEndpointElement)
    {
        CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;
        customEndpoint.Property = this.Property;
    }

    protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ChannelEndpointElement channelEndpointElement)
    {
        CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;
        customEndpoint.Property = this.Property;
    }

    protected override void OnInitializeAndValidate(ServiceEndpointElement serviceEndpointElement)
    {
    }

    protected override void OnInitializeAndValidate(ChannelEndpointElement channelEndpointElement)
    {
    }
}
```

`CreateServiceEndpoint` 関数で、`CustomEndpoint` オブジェクトが作成されます。 その定義を次の例に示します。

```csharp
public class CustomEndpoint : ServiceEndpoint
{
    public CustomEndpoint()
        : this(string.Empty)
    {
    }

    public CustomEndpoint(string address)
        : this(address, ContractDescription.GetContract(typeof(ICalculator)))
    {
    }

    public CustomEndpoint(string address, ContractDescription contract)
        : base(contract)
    {
        this.Binding = new BasicHttpBinding();
        this.IsSystemEndpoint = false;
    }

    public bool Property
    {
        get;
        set;
    }
}
```

 サービスとクライアントとの間で通信を実行するために、クライアントでサービスに対するサービス参照が作成されます。 このサンプルをビルドして実行した場合、サービスが実行されて、クライアントはそのサービスと通信します。 サービスが変更されるたびに、サービス参照を更新する必要があります。

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2012 を使用して、StandardEndpoints .sln ファイルを開きます。

2. 複数のプロジェクトを起動できるようにします。

    1. **ソリューションエクスプローラー**で、標準エンドポイントソリューションを右クリックし、 **[プロパティ]** を選択します。

    2. **[共通プロパティ]** で、 **[スタートアッププロジェクト]** を選択し、 **[マルチスタートアッププロジェクト]** をクリックします。

    3. サービスプロジェクトを一覧の先頭に移動し、**アクション**を **[開始]** に設定します。

    4. サービスプロジェクトの後に、**アクション**が **[開始]** に設定されているクライアントプロジェクトを移動します。

         これで、Client プロジェクトは Service プロジェクトの後に実行されます。

3. ソリューションを実行するには、F5 キーを押します。

> [!NOTE]
> これらの手順が機能しない場合は、次の手順に従って、環境が適切に設定されていることを確認してください。
>
> 1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。
> 2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](building-the-samples.md)」の手順に従います。
> 3. 1つまたは複数のコンピューター構成でサンプルを実行するには、「 [Windows Communication Foundation サンプルの実行](running-the-samples.md)」の手順に従います。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\StandardEndpoints`
