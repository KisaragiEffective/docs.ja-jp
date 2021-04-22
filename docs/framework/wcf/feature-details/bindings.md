---
description: '詳細情報: Windows Communication Foundation バインディング'
title: Windows Communication Foundation バインディング
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], bindings
- Windows Communication Foundation [WCF], bindings
- bindings [WCF]
ms.assetid: 83639133-89f7-43f0-b4ef-8d9e57c08d25
ms.openlocfilehash: 0104a33acbef7738607865b135561860f395dd71
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643632"
---
# <a name="windows-communication-foundation-bindings"></a>Windows Communication Foundation バインディング

Windows Communication Foundation (WCF) では、アプリケーション ソフトウェアを作成する方法と、アプリケーション ソフトウェアが他のソフトウェアと通信する方法は分離されています。 バインディングを使用して、クライアントとサービスが相互に通信するために必要なトランスポート、エンコーディング、およびプロトコルの詳細を指定します。 WCF は、バインディングを使用してエンドポイントの基になるネットワーク上の表現を生成します。このため、バインディングの詳細の大半について通信している双方が合意する必要があります。 これを実現する最も簡単な方法は、サービスのクライアントがサービスのエンドポイントと同じバインディングを使用することです。 この方法の詳細については、「[サービスとクライアントを構成するためのバインディングの使用](../using-bindings-to-configure-services-and-clients.md)」を参照してください。  
  
 バインディングは、バインド要素のコレクションで構成されます。 各要素は、エンドポイントがクライアントと通信する方法の一部分を記述します。 バインディングには、1 つ以上のトランスポート バインド要素、1 つ以上のメッセージ エンコーディング バインド要素 (既定では、トランスポート バインド要素によって提供されます)、および任意の数の他のプロトコル バインド要素が含まれている必要があります。 このように、ランタイムをビルドするプロセスでは、各バインド要素からランタイムにコードを提供できます。  
  
 WCF には、一般的なバインド要素を含むさまざまなバインディングが用意されています。 これらのバインディングは、既定の設定で使用することも、ユーザーの要件に応じて既定値を変更することもできます。 これらのシステム指定のバインディングには、バインド要素およびその設定を直接制御できるプロパティが含まれます。 また、バインディングの各バージョンに独自の名前を付けることで、複数のバージョンを同時に使用することもできます。 詳細については、「[システムが提供するバインディングの構成](configuring-system-provided-bindings.md)」を参照してください。  
  
 システム指定のバインディングに用意されていないバインド要素のコレクションが必要になった場合には、その必要なバインド要素のコレクションで構成されるカスタム バインドを作成できます。 このようなカスタム バインドは簡単に作成でき、新しいクラスも必要ありませんが、バインド要素およびその設定を制御するためのプロパティは提供されません。 バインド要素へのアクセスと設定の変更は、バインド要素を含むコレクションを通じて行います。 詳細については、「[カスタム バインディング](../extending/custom-bindings.md)」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  

 [システムが提供するバインディングの構成](configuring-system-provided-bindings.md)  
 一般的なシナリオをサポートするために WCF に用意されたバインディングを使用および変更する方法について説明します。  
  
 [サービスとクライアントを構成するためのバインディングの使用](../using-bindings-to-configure-services-and-clients.md)  
 サービスおよびクライアントで使用する Windows Communication Foundation (WCF) バインディングをコードで強制的に定義する方法と、構成を使用して宣言によって定義する方法について説明します。  
  
 [カスタム バインディング](../extending/custom-bindings.md)  
 <xref:System.ServiceModel.Channels.CustomBinding> の概要と使用する状況について説明します。  
  
## <a name="reference"></a>関連項目  

 <xref:System.ServiceModel.Channels.Binding>  
  
 <xref:System.ServiceModel.Channels.BindingElement>  
  
 <xref:System.ServiceModel.Channels.CustomBinding>  
  
## <a name="related-sections"></a>関連項目  

 [バインディングの拡張](../extending/extending-bindings.md)
