---
description: '詳細情報: WS-MetadataExchange のバインディング'
title: WS-MetadataExchange のバインディング
ms.date: 03/30/2017
ms.assetid: 10f8de5d-b81d-4ea7-b37e-7f2c00c39714
ms.openlocfilehash: 3bcea573ad436a85285fab5657e58defa9113d9c
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643944"
---
# <a name="ws-metadataexchange-bindings"></a>WS-MetadataExchange のバインディング

ここでは、既定のメタデータ交換バインディングを各種のトランスポート用に構築する方法を説明します。  
  
## <a name="the-default-bindings"></a>既定のバインディング  
  
|既定のバインディング名|バインディングを構築する方法|  
|--------------------------|------------------------------------|  
|mexHttpBinding|トランスポート レベルのセキュリティが無効な <xref:System.ServiceModel.WSHttpBinding>。|  
|mexHttpsBinding|トランスポート レベルのセキュリティをサポートする <xref:System.ServiceModel.WSHttpBinding>。|  
|mexNamedPipeBinding|<xref:System.ServiceModel.Channels.CustomBinding> で既定値を使用する <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>。|  
|mexTcpBinding|<xref:System.ServiceModel.Channels.CustomBinding> で既定値を使用する <xref:System.ServiceModel.Channels.TcpTransportBindingElement>。|
