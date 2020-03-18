---
title: dotnet-svcutil.xmlserializer の使用
description: .NET Core プロジェクト用にシリアル化アセンブリを事前に生成する `dotnet-svcutil.xmlserializer` NuGet パッケージの使用方法について説明します。
author: huanwu
ms.date: 11/27/2018
ms.openlocfilehash: 4811647c294118cb160d25805e7d3ada97f071f9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75344900"
---
# <a name="using-dotnet-svcutilxmlserializer-on-net-core"></a>.NET Core で dotnet-svcutil.xmlserializer を使用する

`dotnet-svcutil.xmlserializer` NuGet パッケージは、.NET Core プロジェクト用にシリアル化アセンブリを事前に生成することができます。 これは、クライアント アプリケーション内の型で、WCF サービス コントラクトによって使われ XmlSerializer によってシリアル化できるものに対して、C# のシリアル化コードを事前に生成します。 これにより、それらの型のオブジェクトをシリアル化または逆シリアル化するときに XML シリアル化の起動時のパフォーマンスが向上します。

## <a name="prerequisites"></a>前提条件

* [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 以降
* 任意のコード エディター

コマンド `dotnet --info` を使用して、既にインストールされている .NET Core SDK およびランタイムのバージョンを確認することができます。

## <a name="getting-started"></a>作業の開始

.NET Core コンソール アプリケーションで `dotnet-svcutil.xmlserializer` を使用するには:

1. .NET Framework で既定のテンプレート 'WCF Service Application' を使用して、'MyWCFService' という名前の WCF サービスを作成します。 次のようなサービス メソッドに `[XmlSerializerFormat]` 属性を追加します。

   ```csharp
    [ServiceContract]
    public interface IService1
    {
        [XmlSerializerFormat]
        [OperationContract(Action = "http://tempuri.org/IService1/GetData", ReplyAction = "http://tempuri.org/IService1/GetDataResponse")]
        string GetData(int value);
    }
    ```

2. .NET Core 2.1 以降のバージョンを対象とする WCF クライアント アプリケーションとして .NET Core コンソール アプリケーションを作成します。 たとえば、次のコマンドを使用して、'MyWCFClient' という名前のアプリを作成します。

    ```dotnetcli
    dotnet new console --name MyWCFClient
    ```

    プロジェクトが .NET Core 2.1 以降を対象としていることを確認するには、プロジェクト ファイル内の `TargetFramework` XML 要素を調べます。

    ```xml
    <TargetFramework>netcoreapp2.1</TargetFramework>
    ```

3. 次のコマンドを実行して、`System.ServiceModel.Http` へのパッケージ参照を追加します。

    ```dotnetcli
    dotnet add package System.ServiceModel.Http
    ```

4. WCF クライアント コードを追加します。

    ```csharp
    using System.ServiceModel;

        class Program
        {
            static void Main(string[] args)
            {
                var myBinding = new BasicHttpBinding();
                var myEndpoint = new EndpointAddress("http://localhost:2561/Service1.svc"); //Fill your service url here
                var myChannelFactory = new ChannelFactory<IService1>(myBinding, myEndpoint);
                IService1 client = myChannelFactory.CreateChannel();
                string s = client.GetData(1);
                ((ICommunicationObject)client).Close();
            }
        }

    [ServiceContract]
    public interface IService1
    {
        [XmlSerializerFormat]
        [OperationContract(Action = "http://tempuri.org/IService1/GetData", ReplyAction = "http://tempuri.org/IService1/GetDataResponse")]
        string GetData(int value);
    }
    ```

5. 次のコマンドを実行して `dotnet-svcutil.xmlserializer` パッケージへの参照を追加します。
  
    ```dotnetcli
    dotnet add package dotnet-svcutil.xmlserializer
    ```

    コマンドを実行すると、次のようなエントリがプロジェクト ファイルに追加されます。
  
    ```xml
    <ItemGroup>
      <DotNetCliToolReference Include="dotnet-svcutil.xmlserializer" Version="1.0.0" />
    </ItemGroup>
    ```

6. `dotnet build` を実行してアプリケーションをビルドします。 すべて正常に終了すると、出力フォルダーに *MyWCFClient.XmlSerializers.dll* という名前のアセンブリが生成されます。 ツールがアセンブリの生成に失敗した場合は、ビルド出力に警告が表示されます。

7. ブラウザーで `http://localhost:2561/Service1.svc` を実行するなどして、WCF サービスを開始します。 次にクライアント アプリケーションを起動します。実行時に、事前に生成されたシリアライザーが自動的に読み込まれ、使用されます。
