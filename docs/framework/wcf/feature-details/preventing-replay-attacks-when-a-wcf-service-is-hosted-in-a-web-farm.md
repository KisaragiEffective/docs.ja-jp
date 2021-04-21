---
description: '詳細情報: WCF サービスが Web ファームでホストされている場合のリプレイ攻撃の回避'
title: WCF サービスが Web ファームでホストされている場合のリプレイ攻撃の回避
ms.date: 03/30/2017
ms.assetid: 1c2ed695-7a31-4257-92bd-9e9731b886a5
ms.openlocfilehash: 015059ef650b3ec213c54b89763bac7d46dd218f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99793611"
---
# <a name="preventing-replay-attacks-when-a-wcf-service-is-hosted-in-a-web-farm"></a>WCF サービスが Web ファームでホストされている場合のリプレイ攻撃の回避

メッセージ セキュリティ WCF の使用により、受信メッセージから NONCE を作成し、生成された NONCE が内部 `InMemoryNonceCache` にあるかどうかを確認することで、リプレイ攻撃が防止される場合。 その場合、メッセージはリプレイとして破棄されます。 WCF サービスが Web ファームでホストされている場合、`InMemoryNonceCache` は Web ファームのノード間で共有されていないため、サービスはリプレイ攻撃に対して脆弱です。  このシナリオの危険性を軽減するために、WCF 4.5 では抽象クラス <xref:System.ServiceModel.Security.NonceCache> からクラスを派生させることによって独自の共有 NONCE キャッシュを実装する機能拡張ポイントを提供しています。  
  
## <a name="noncecache-implementation"></a>NonceCache の実装  

 独自の共有 NONCE キャッシュを実装するには、<xref:System.ServiceModel.Security.NonceCache> からクラスを派生させ、<xref:System.ServiceModel.Security.NonceCache.CheckNonce%2A> メソッドと <xref:System.ServiceModel.Security.NonceCache.TryAddNonce%2A> メソッドをオーバーライドします。 <xref:System.ServiceModel.Security.NonceCache.CheckNonce%2A> は、指定された NONCE がキャッシュに存在するかどうかを確認します。 <xref:System.ServiceModel.Security.NonceCache.TryAddNonce%2A> は NONCE をキャッシュに追加しようとします。 クラスを実装したら、インスタンスを作成して、そのインスタンスをクライアント側でのリプレイ検出用には <xref:System.ServiceModel.Channels.LocalClientSecuritySettings.NonceCache%2A>、サーバー側でのリプレイ検出用には <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.NonceCache%2A> に割り当てて準備します。 この機能のためにすぐに使用できる構成サポートはありません。  
  
## <a name="see-also"></a>関連項目

- [メッセージのセキュリティ](message-security-in-wcf.md)
- [SymmetricSecurityBindingElement](../diagnostics/wmi/symmetricsecuritybindingelement.md)
