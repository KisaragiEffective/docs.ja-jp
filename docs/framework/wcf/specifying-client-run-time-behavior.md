---
description: 詳細については、「クライアント Run-Time の動作の指定」を参照してください。
title: クライアントのランタイム動作の指定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- behaviors [WCF], system-provided client
ms.assetid: d16d3405-be70-4edb-8f62-b5f614ddeca5
ms.openlocfilehash: 66d7e33ae0e12cadb9532ad523fd806b35e2aa7e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99755669"
---
# <a name="specifying-client-run-time-behavior"></a>クライアントのランタイム動作の指定

Windows Communication Foundation (WCF) サービスなどの Windows Communication Foundation (WCF) クライアントは、クライアントアプリケーションに合わせて実行時の動作を変更するように構成できます。 クライアントのランタイム動作を指定する際には、3 つの属性を使用できます。 双方向クライアント コールバック オブジェクトは、<xref:System.ServiceModel.CallbackBehaviorAttribute> 属性と <xref:System.ServiceModel.Description.CallbackDebugBehavior> 属性を使用して、そのランタイム動作を変更できます。 もう 1 つの属性 <xref:System.ServiceModel.Description.ClientViaBehavior> は、論理送信先を直接のネットワーク送信先と区別するために使用できます。 さらに、双方向クライアントのコールバック型では、サービス側の動作の一部を使用できます。 詳細については、「 [サービス Run-Time 動作の指定](specifying-service-run-time-behavior.md)」を参照してください。  
  
## <a name="using-the-callbackbehaviorattribute"></a>CallbackBehaviorAttribute の使用  

 <xref:System.ServiceModel.CallbackBehaviorAttribute> クラスを使用して、クライアント アプリケーションのコールバック コントラクト実装の実行動作を構成または拡張できます。 この属性は、インスタンス化動作とトランザクションの設定を除き、<xref:System.ServiceModel.ServiceBehaviorAttribute> クラスと同様の機能をコールバック クラスに対して実行します。  
  
 <xref:System.ServiceModel.CallbackBehaviorAttribute> クラスは、コールバック コントラクトを実装するクラスに適用する必要があります。 双方向以外のコントラクト実装に適用した場合は、実行時に <xref:System.InvalidOperationException> 例外がスローされます。 <xref:System.ServiceModel.CallbackBehaviorAttribute> オブジェクトを使用してマーシャリングするスレッドを決定するコールバック オブジェクトの <xref:System.Threading.SynchronizationContext> クラス、メッセージ検証を実施する <xref:System.ServiceModel.CallbackBehaviorAttribute.ValidateMustUnderstand%2A> プロパティ、およびデバッグのために例外を <xref:System.ServiceModel.CallbackBehaviorAttribute.IncludeExceptionDetailInFaults%2A> オブジェクトとしてサービスに返す <xref:System.ServiceModel.FaultException> プロパティのコード例を次に示します。  
  
 [!code-csharp[CallbackBehaviorAttribute#3](../../../samples/snippets/csharp/VS_Snippets_CFX/callbackbehaviorattribute/cs/client.cs#3)]
 [!code-vb[CallbackBehaviorAttribute#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/callbackbehaviorattribute/vb/client.vb#3)]  
  
## <a name="using-callbackdebugbehavior-to-enable-the-flow-of-managed-exception-information"></a>CallbackDebugBehavior を使用したマネージド例外情報のフローの有効化  

 アプリケーション構成ファイルまたはプログラムで、<xref:System.ServiceModel.Description.CallbackDebugBehavior.IncludeExceptionDetailInFaults%2A> プロパティを `true` に設定すると、デバッグのためにクライアント コールバック オブジェクト内のマネージド例外情報をサービスに戻すフローを有効にできます。  
  
 マネージド例外情報をサービスに戻すのは、セキュリティ リスクになる可能性があります。これは、例外の詳細により、非承認のサービスで使用可能な内部クライアントの実装についての情報が公開されるからです。 さらに、<xref:System.ServiceModel.Description.CallbackDebugBehavior> プロパティをプログラムで設定することはできますが、配置するときに <xref:System.ServiceModel.Description.CallbackDebugBehavior.IncludeExceptionDetailInFaults%2A> を無効にすることを忘れがちになります。  
  
 セキュリティの問題にかかわるので、以下を強くお勧めします。  
  
- <xref:System.ServiceModel.Description.CallbackDebugBehavior.IncludeExceptionDetailInFaults%2A> プロパティの値を `true` に設定するには、アプリケーション構成ファイルを使用します。  
  
- これは、制御されたデバッグ シナリオの場合に限って行います。  
  
 SOAP メッセージのクライアントコールバックオブジェクトからマネージ例外情報を返すように WCF に指示するクライアント構成ファイルのコード例を次に示します。  
  
 [!code-xml[SCA.CallbackContract#4](../../../samples/snippets/csharp/VS_Snippets_CFX/sca.callbackcontract/cs/client.exe.config#4)]  

## <a name="using-the-clientviabehavior-behavior"></a>ClientViaBehavior 動作の使用  

 <xref:System.ServiceModel.Description.ClientViaBehavior> 動作を使用して、トランスポート チャネルを作成する対象の URI (Uniform Resource Identifier) を指定できます。 この動作は、直接のネットワーク送信先が、メッセージの処理先と異なる場合に使用します。 これにより、呼び出し側のアプリケーションが最終的な送信先を知っているとは限らないとき、または送信先の `Via` ヘッダーがアドレスでないときに、複数のホップを経由するメッセージ交換が可能になります。  
  
## <a name="see-also"></a>関連項目

- [サービスのランタイム動作の指定](specifying-service-run-time-behavior.md)
