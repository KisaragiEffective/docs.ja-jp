---
title: メタデータ形式
ms.date: 03/30/2017
ms.assetid: baad1e68-28fc-4a6a-8a43-75e47e7fa871
ms.openlocfilehash: e7208a8d5fd6d100ac2a2c4fb1369a571c63e7fc
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76212169"
---
# <a name="metadata-formats"></a>メタデータ形式
Windows Communication Foundation (WCF) は、次の表のメタデータ形式をサポートしています。  
  
## <a name="metadata-specifications-and-usage"></a>メタデータの仕様と用途  
  
|[プロトコル]|仕様と用途|  
|--------------|-----------------------------|  
|WSDL 1.1|[Web サービス記述言語 (WSDL) 1.1](https://www.w3.org/TR/wsdl/)<br /><br /> WCF では、Web サービス記述言語 (WSDL) を使用してサービスを記述します。|  
|XML スキーマ|[Xml スキーマパート 2: データ型 Second エディション](https://www.w3.org/TR/2004/REC-xmlschema-2-20041028/)と[XML スキーマパート 1: 2 番目のエディション](https://www.w3.org/TR/2004/REC-xmlschema-1-20041028/)<br /><br /> WCF では、XML スキーマを使用して、メッセージで使用されるデータ型を記述します。|  
|WS-Policy|[Web サービスポリシー 1.2-Framework (WS-POLICY)](https://www.w3.org/Submission/WS-Policy/)<br /><br /> [Web サービスポリシー 1.5-フレームワーク](https://www.w3.org/TR/ws-policy/)<br /><br /> WCF では、ドメイン固有のアサーションと共に WS-POLICY 1.2 または1.5 仕様を使用して、サービスの要件と機能を記述します。|  
|WS-PolicyAttachments|[Web サービスポリシー 1.2-添付ファイル (WS-I 添付ファイル)](https://www.w3.org/Submission/WS-PolicyAttachment/)<br /><br /> WCF では、WS-POLICY 添付ファイルを実装して、WSDL 内のさまざまなスコープでポリシー式をアタッチします。|  
|WS-Metadata Exchange|[Web サービスメタデータ交換 (Ws-metadataexchange) バージョン1.1](https://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF では、XML スキーマ、WSDL、および WS-POLICY を取得するために、Ws-metadataexchange が実装されています。|  
|WS Addressing Binding for WSDL|[Web サービスアドレス指定 1.0-WSDL バインド](https://www.w3.org/TR/ws-addr-wsdl/)<br /><br /> WCF では、wsdl でアドレス指定情報をアタッチするために、WS-ADDRESSING Binding for WSDL が実装されています。|  
  
## <a name="see-also"></a>関連項目

- [システム標準の相互運用性バインディングがサポートしている Web サービス プロトコル](../../../../docs/framework/wcf/feature-details/web-services-protocols-supported-by-system-provided-interoperability-bindings.md)
- [WSDL とポリシー](../../../../docs/framework/wcf/feature-details/wsdl-and-policy.md)
