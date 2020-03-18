---
title: 非同期操作 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, asynchronous operations
- asynchronous operations [WCF Data Services]
- WCF Data Services, client library
ms.assetid: 679644c7-e3fc-422c-b14a-b44b683900d0
ms.openlocfilehash: 5191f3d03facaafc64f6df494ff90cd0ce1c1988
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346183"
---
# <a name="asynchronous-operations-wcf-data-services"></a>非同期操作 (WCF Data Services)
Web アプリケーションは、内部ネットワーク内で実行するアプリケーションより長い、クライアントとサーバーとの間の待機時間に対応する必要があります。 アプリケーションのパフォーマンスとユーザーエクスペリエンスを最適化するには、Web 経由で WCF Data Services サーバーにアクセスするときに、<xref:System.Data.Services.Client.DataServiceContext> クラスと <xref:System.Data.Services.Client.DataServiceQuery%601> クラスの非同期メソッドを使用することをお勧めします。  
  
 WCF Data Services サーバーは HTTP 要求を非同期的に処理しますが、WCF Data Services クライアントライブラリの一部のメソッドは同期され、要求-応答の exchange 全体が完了するまで待機してから、実行を続行します。 WCF Data Services クライアントライブラリの非同期メソッドは、この交換が完了するまで待機しないため、アプリケーションはその間に応答性の高いユーザーインターフェイスを維持することができます。  
  
 非同期操作を実行するには、<xref:System.Data.Services.Client.DataServiceContext> の2つのメソッドを使用し、それぞれ*Begin*と*End*で始まるクラスを <xref:System.Data.Services.Client.DataServiceQuery%601> します。 *Begin*メソッドは、操作の完了時にサービスが呼び出すデリゲートを登録します。 *終了*メソッドは、完了した操作からのコールバックを処理するために登録されているデリゲートで呼び出す必要があります。 *End*メソッドを呼び出して非同期操作を完了するには、操作を開始するために使用したのと同じ <xref:System.Data.Services.Client.DataServiceQuery%601> または <xref:System.Data.Services.Client.DataServiceContext> インスタンスから実行する必要があります。 各*Begin*メソッドは、状態オブジェクトをコールバックに渡すことができる `state` パラメーターを受け取ります。 この状態オブジェクトは、コールバックで指定された <xref:System.IAsyncResult> から取得され、対応する*End*メソッドを呼び出して非同期操作を完了するために使用されます。 たとえば、<xref:System.Data.Services.Client.DataServiceQuery%601> インスタンスで `state` メソッドを呼び出すときにインスタンスを <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> パラメーターとして指定した場合、同じ <xref:System.Data.Services.Client.DataServiceQuery%601> インスタンスが <xref:System.IAsyncResult> によって返されます。 この <xref:System.Data.Services.Client.DataServiceQuery%601> のインスタンスは、<xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> メソッドを呼び出してクエリ操作を完了するために使用されます。 詳細については、「[方法: 非同期データサービスクエリを実行する](how-to-execute-asynchronous-data-service-queries-wcf-data-services.md)」を参照してください。  
  
> [!NOTE]
> Silverlight 用の .NET Framework で提供されるクライアント ライブラリでは、非同期操作だけがサポートされます。 詳細については、「 [WCF Data Services (Silverlight)](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95))」を参照してください。  
  
 .NET Framework クライアント ライブラリは、以下の非同期操作をサポートします。  
  
|操作|メソッド|  
|---------------|-------------|  
|<xref:System.Data.Services.Client.DataServiceQuery%601> の実行。|-   <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A><br />-   <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A>|  
|<xref:System.Data.Services.Client.DataServiceContext> からのクエリの実行。|-   <xref:System.Data.Services.Client.DataServiceContext.BeginExecute%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndExecute%2A>|  
|<xref:System.Data.Services.Client.DataServiceContext> からのバッチ クエリの実行。|-   <xref:System.Data.Services.Client.DataServiceContext.BeginExecuteBatch%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndExecuteBatch%2A>|  
|<xref:System.Data.Services.Client.DataServiceContext> への関連エンティティの読み込み。|-   <xref:System.Data.Services.Client.DataServiceContext.BeginLoadProperty%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndLoadProperty%2A>|  
|オブジェクトに対する変更の <xref:System.Data.Services.Client.DataServiceContext> への保存。|-   <xref:System.Data.Services.Client.DataServiceContext.BeginSaveChanges%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndSaveChanges%2A>|  
  
## <a name="threading-considerations-for-asynchronous-operations"></a>非同期操作のスレッドに関する考慮事項  
 マルチスレッドアプリケーションでは、非同期操作のコールバックとして登録されたデリゲートは、最初の要求を作成する*Begin*メソッドの呼び出しに使用されたのと同じスレッドで必ずしも呼び出されるとは限りません。 特定のスレッドでコールバックを呼び出す必要があるアプリケーションでは、応答を処理する*End*メソッドの実行を目的のスレッドに明示的にマーシャリングする必要があります。 たとえば、Windows Presentation Foundation (WPF) ベースのアプリケーションおよび Silverlight ベースのアプリケーションでは、<xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> オブジェクトで <xref:System.Windows.Threading.Dispatcher> メソッドを使用して応答を UI スレッドにマーシャリングする必要があります。 詳細については、「[データサービスのクエリ (WCF Data Services/Silverlight)](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc903932(v=vs.95))」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
