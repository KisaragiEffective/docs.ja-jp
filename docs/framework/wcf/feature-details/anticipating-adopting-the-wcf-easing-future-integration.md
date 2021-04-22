---
description: '詳細情報: Windows Communication Foundation 導入の準備: 将来的な統合の容易化'
title: Windows Communication Foundation 導入の準備:将来的な統合の容易化
ms.date: 03/30/2017
ms.assetid: 3028bba8-6355-4ee0-9ecd-c56e614cb474
ms.openlocfilehash: 512e35e8a4cd6057c96e96a1474f393c3f006525
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643879"
---
# <a name="anticipating-adopting-the-windows-communication-foundation-easing-future-integration"></a>Windows Communication Foundation 導入の準備:将来的な統合の容易化

現在 ASP.NET を使用しており、将来 WCF を採用する予定である場合、このトピックは、新規の ASP.NET Web サービスが WCF アプリケーションと共に問題なく動作するためのガイダンスになります。  
  
## <a name="general-recommendations"></a>一般的な推奨事項  

 ASP.NET 2.0 を使用して、すべての新規サービスを作成します。 こうすることで、拡張および強化された新バージョンの機能にアクセスできます。 またこれにより、ASP.NET 2.0 コンポーネントと WCF コンポーネントとを同じアプリケーション内で使用する可能性にも対応できます。  
  
## <a name="protocols"></a>プロトコル  

 WS-I Basic Profile 1.1 への準拠を検証するには、次のように ASP.NET 2.0 の新機能を使用します。  
  
```csharp  
[WebService(Namespace = "http://tempuri.org/")]  
[WebServiceBinding(  
     ConformsTo = WsiProfiles.BasicProfile1_1,  
     EmitConformanceClaims=true)]  
public interface IEcho  
```  
  
 WCF の定義済みバインディングである <xref:System.ServiceModel.BasicHttpBinding> を使用することによって、WS-I Basic Profile 1.1 準拠の ASP.NET Web サービスは WCF クライアントと相互運用できます。  
  
## <a name="service-development"></a>サービスの開発  

 SOAPAction HTTP ヘッダーではなく、SOAP メッセージの本文要素の完全修飾名に基づいてメッセージをメソッドにルーティングする場合は、<xref:System.Web.Services.Protocols.SoapDocumentServiceAttribute> 属性の使用を避けます。 WCF は、SOAPAction HTTP ヘッダーを使用してメッセージをルーティングします。  
  
## <a name="data-representation"></a>データ表現  

 <xref:System.Xml.Serialization.XmlSerializer> によって既定で型のシリアル化が行われる XML は、XML の名前空間が明示的に定義されている場合、<xref:System.Runtime.Serialization.DataContractSerializer> によって型のシリアル化が行われる XML と意味的に同一です。 将来 WCF を採用する予定の場合、ASP.NET Web サービスで使用するデータ型を定義するには、次の操作を実行します。  
  
1. XML スキーマではなく、.NET Framework クラスを使用して型を定義します。  
  
2. <xref:System.SerializableAttribute> と <xref:System.Xml.Serialization.XmlRootAttribute> だけをそのクラスに追加します。後者を使用して型の名前空間を明示的に定義してください。 .NET Framework クラスを XML に変換する方法を制御する目的で、<xref:System.Xml.Serialization> 名前空間の属性を追加しないでください。  
  
 この手法を採用すると、後から <xref:System.Runtime.Serialization.DataContractAttribute> および <xref:System.Runtime.Serialization.DataMemberAttribute> を追加することで、転送のためにクラスをシリアル化する XML に大きな変更を加えることなく .NET クラスをデータ コントラクトにすることができます。 ASP.NET Web サービスがメッセージ内で使用する型は、WCF アプリケーションによってデータ コントラクトとして処理することが可能です。これにはさまざまな利点がありますが、特に WCF アプリケーションではより高いパフォーマンスを引き出すことができます。  
  
## <a name="security"></a>セキュリティ  

 インターネット インフォメーション サービス (IIS) に用意されている認証オプションは使用しないでください。 これらは、WCF クライアントではサポートされません。 サービスをセキュリティで保護する必要がある場合は、WCF で提供されるオプションを使用します。こちらのオプションの方が選択の幅が広く、標準プロトコルに基づいているためです。  
  
## <a name="see-also"></a>関連項目

- [Windows Communication Foundation 導入の準備:将来の移行の簡略化](anticipating-adopting-wcf-migration.md)
