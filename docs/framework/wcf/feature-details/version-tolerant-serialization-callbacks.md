---
description: 詳細については、Version-Tolerant シリアル化のコールバック
title: バージョン トレラントなシリアル化コールバック
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- OnDeserializedAttribute [WCF]
- OnDeserializingAttribute [WCF]
- OnSerializingAttribute [WCF]
- serialization [WCF], setting default values
- OnSerializedAttribute [WCF]
ms.assetid: aa4a3a6f-05ec-4efd-bdbf-2181e13e6468
ms.openlocfilehash: 8ee53e96b90ae6b0f2993a543fc129d75eb3481f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99756007"
---
# <a name="version-tolerant-serialization-callbacks"></a>バージョン トレラントなシリアル化コールバック

データ コントラクトのプログラミング モデルでは、<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> クラスと <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> クラスがサポートする、複数のバージョンに対応するシリアル化コールバック メソッドが完全にサポートされます。  
  
## <a name="version-tolerant-attributes"></a>複数のバージョンに対応する属性  

 4 つのコールバック属性があります。 各属性は、さまざまなタイミングでシリアル化エンジンまたは逆シリアル化エンジンが呼び出すメソッドに適用できます。 次の表では、各属性を使用するタイミングについて説明します。  
  
|属性|対応するメソッドが呼び出されるタイミング|  
|---------------|---------------------------------------------|  
|<xref:System.Runtime.Serialization.OnSerializingAttribute>|型をシリアル化する前に呼び出されます。|  
|<xref:System.Runtime.Serialization.OnSerializedAttribute>|型をシリアル化した後に呼び出されます。|  
|<xref:System.Runtime.Serialization.OnDeserializingAttribute>|型を逆シリアル化する前に呼び出されます。|  
|<xref:System.Runtime.Serialization.OnDeserializedAttribute>|型を逆シリアル化した後に呼び出されます。|  
  
 メソッドは、<xref:System.Runtime.Serialization.StreamingContext> パラメーターを受け入れる必要があります。  
  
 これらのメソッドは、主にバージョン管理と初期化に使用するためのものです。 逆シリアル化時にコンストラクターは呼び出されません。 このため、データ メンバーのデータが受信ストリームにない場合、たとえば、データの送信元が、一部のデータ メンバーが不足している前のバージョンの型である場合、そのデータ メンバーを目的の既定値に適切に初期化できないことがあります。 これを修正するには、次の例で示すように、<xref:System.Runtime.Serialization.OnDeserializingAttribute> でマークされたコールバック メソッドを使用します。  
  
 上記の各コールバック属性でマークできるのは、型ごとに 1 つのメソッドだけです。  
  
### <a name="example"></a>例  

 [!code-csharp[C_DataContract#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#9)]
 [!code-vb[C_DataContract#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#9)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.Serialization.OnSerializingAttribute>
- <xref:System.Runtime.Serialization.OnSerializedAttribute>
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>
- <xref:System.Runtime.Serialization.StreamingContext>
- [バージョントレラントなシリアル化](../../../standard/serialization/version-tolerant-serialization.md)
