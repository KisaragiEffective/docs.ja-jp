---
description: '詳細情報: 方法: サービス データのパーティション分割'
title: '方法: サービス データのパーティション分割'
ms.date: 03/30/2017
ms.assetid: 1ccff72e-d76b-4e36-93a2-e51f7b32dc83
ms.openlocfilehash: fd40ee37a80a8d5039231c6efe6369006022afad
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643242"
---
# <a name="how-to-service-data-partitioning"></a>方法: サービス データのパーティション分割

このトピックでは、メッセージを同じ送信先サービスの複数のインスタンスにパーティション分割するのに必要な、基本的な手順について説明します。 サービス データのパーティション分割は、一般的に、優れた品質のサービスを提供するためにサービスを拡張する必要がある場合や、さまざまな顧客からの要求を特定の方法で処理する必要がある場合に使用されます。 たとえば、重要な顧客 ("ゴールド" 顧客) からのメッセージは、標準顧客からのメッセージよりも高い優先度で処理される必要がある場合があります。  
  
 この例では、メッセージが regularCalc サービスの 2 つのインスタンスの 1 つにルーティングされます。 サービスの両方のインスタンスは同じですが、calculator1 エンドポイントで表されるサービスは、重要な顧客から受け取ったメッセージを処理し、calculator 2 エンドポイントは、その他の顧客からのメッセージを処理します。  
  
 クライアントから送信されたメッセージには、どちらのサービス インスタンスにメッセージをルーティングする必要があるかを識別するのに使用できる一意のデータはありません。 各クライアントが特定の送信先サービスにデータをルーティングできるようにするため、メッセージの受信に使用される 2 つのサービス エンドポイントを実装します。  
  
> [!NOTE]
> この例では、特定のエンドポイントを使用してデータをパーティション分割しますが、ヘッダーや本文データなど、メッセージ自体に含まれている情報を使用することもできます。  
  
### <a name="implement-service-data-partitioning"></a>サービス データのパーティション分割の実装  
  
1. サービスによって公開されるサービス エンドポイントを指定することによって、ルーティング サービスの基本的な構成を作成します。 次の例では、メッセージの受信に使用する、2 つのサービス エンドポイントを定義します。 また、regularCalc サービス インスタンスへのメッセージ送信に使用する、クライアント エンドポイントも定義します。  
  
    ```xml  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/router" />  
          </baseAddresses>  
        </host>  
        <!--Set up the inbound endpoints for the Routing Service-->  
        <!--create the endpoints for the calculator service-->  
        <endpoint address="calculator1"  
                  binding="wsHttpBinding"  
                  name="calculator1Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <endpoint address="calculator2"  
                  binding="wsHttpBinding"  
                  name="calculator2Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
       </service>  
    </services>  
    <client>  
    <!--set up the destination endpoints-->  
        <endpoint name="CalcEndpoint1"  
                  address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                  binding="netTcpBinding"  
                  contract="*" />  
  
        <endpoint name="CalcEndpoint2"  
                  address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                  binding="netTcpBinding"  
                  contract="*" />  
     </client>  
    ```  
  
2. 送信先エンドポイントにメッセージをルーティングするのに使用するフィルターを定義します。  この例では、EndpointName フィルターを使用して、どのサービス エンドポイントがメッセージを受信したかを判断します。 次の例では、必要なルーティング セクションおよびフィルターを定義します。  
  
    ```xml  
    <filters>  
      <!--define the different message filters-->  
      <!--define endpoint name filters looking for messages that show up on the virtual endpoints-->  
      <filter name="HighPriority" filterType="EndpointName"  
              filterData="calculator1Endpoint"/>  
      <filter name="NormalPriority" filterType="EndpointName"  
              filterData="calculator2Endpoint"/>  
    </filters>  
    ```  
  
3. 各フィルターをクライアント エンドポイントと関連付けるフィルター テーブルを定義します。 この例では、メッセージの受信に使用された特定のエンドポイントに基づいて、メッセージがルーティングされます。 メッセージは 2 つのフィルターのどちらかにのみ一致するため、フィルターが評価される順序をフィルターの優先順位で制御する必要はありません。  
  
     次のコードでは、フィルター テーブルを定義し、前に定義されたフィルターを追加します。  
  
    ```xml  
    <filterTables>  
       <filterTable name="filterTable1">  
         <!--add the filters to the message filter table-->  
         <add filterName="HighPriority" endpointName="CalcEndpoint1"/>  
         <add filterName="NormalPriority" endpointName="CalcEndpoint2"/>  
       </filterTable>  
    </filterTables>  
    ```  
  
4. 受信メッセージをテーブルに含まれているフィルターと照合して評価するには、ルーティング動作を使用して、フィルター テーブルをサービス エンドポイントと関連付ける必要があります。 次の例は、"filterTable1" をサービス エンドポイントと関連付ける方法を示しています。  
  
    ```xml  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
## <a name="example"></a>例  

 構成ファイル全体の一覧を次に示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright (c) Microsoft Corporation. All rights reserved -->  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/router" />  
          </baseAddresses>  
        </host>  
        <!--Set up the inbound endpoints for the Routing Service-->  
        <!--create the endpoints for the calculator service-->  
        <endpoint address="calculator1"  
                  binding="wsHttpBinding"  
                  name="calculator1Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <endpoint address="calculator2"  
                  binding="wsHttpBinding"  
                  name="calculator2Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
  
      </service>  
    </services>  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
    <client>  
<!--set up the destination endpoints-->  
      <endpoint name="CalcEndpoint1"  
                address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <endpoint name="CalcEndpoint2"  
                address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
    </client>  
    <routing>  
      <!-- use the namespace table element to define a prefix for our custom namespace-->  
  
      <filters>  
        <!--define the different message filters-->  
        <!--define endpoint name filters looking for messages that show up on the virtual endpoints-->  
        <filter name="HighPriority" filterType="EndpointName"  
                filterData="calculator1Endpoint"/>  
        <filter name="NormalPriority" filterType="EndpointName"  
                filterData="calculator2Endpoint"/>  
      </filters>  
  
      <filterTables>  
        <filterTable name="filterTable1">  
          <!--add the filters to the message filter table-->  
          <add filterName="HighPriority" endpointName="CalcEndpoint1"/>  
          <add filterName="NormalPriority" endpointName="CalcEndpoint2"/>  
        </filterTable>  
      </filterTables>  
  
    </routing>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [ルーティング サービス](../samples/routing-services.md)
