---
description: '詳細については、次を参照してください: Reliable Services'
title: Reliable Service
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], reliable messaging
- Windows Communication Foundation [WCF], reliable messaging
- WCF [WCF], reliable sessions
- Windows Communication Foundation [WCF], reliable sessions
- service contracts [WCF], reliable services
ms.assetid: 07814ed0-0775-47f2-987b-d8134fdd5099
ms.openlocfilehash: c38f949e57b1891da2433875571443656c7044dd
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99779206"
---
# <a name="reliable-services"></a>Reliable Service

キューと信頼できるセッションは、信頼できるメッセージングを実装する Windows Communication Foundation (WCF) 機能です。 このトピックでは、WCF の信頼できるメッセージング機能について説明します。  
  
 信頼できる *メッセージング* とは、信頼できるメッセージの送信元 (*送信元* と呼ばれます) が、信頼できるメッセージの送信先 (*送信先* と呼ばれます) にメッセージを確実に転送する方法です。  
  
 信頼できるメッセージングは、次の機能を実行します。  
  
- メッセージ転送エラーまたはトランスポート エラーに関係なく、送信元から送信先に送られるメッセージの転送が保証されること。  
  
- 送信元と送信先を互いに分離すること。 これにより、送信元または送信先が利用できない場合でも、送信元と送信先でのそれぞれ独立したエラーと回復が可能になると共に、信頼できるメッセージの転送と配信が実現されます。  
  
 信頼できるメッセージングを実現すると、待ち時間が長くなることがよくあります。 *待機* 時間とは、メッセージが送信元から送信先に到着するまでにかかる時間です。 したがって、WCF では、次の種類の信頼できるメッセージングを提供します。  
  
- [信頼できるセッション](./feature-details/reliable-sessions.md)。高待機時間のコストを発生させることなく、信頼性の高い転送を実現します。  
  
- [WCF のキュー](./feature-details/queues-in-wcf.md)。信頼できる転送と、ソースと宛先の分離の両方を提供します。  
  
## <a name="reliable-sessions"></a>信頼できるセッション  

 信頼できるセッションでは、メッセージング (送信元および送信先) エンドポイントを分離する中継局の数や種類に関係なく、WS-ReliableMessaging プロトコルを使用して、送信元から送信先へのエンドツーエンドの信頼できるメッセージ転送を実現します。 これには SOAP を使用しないトランスポート手段 (HTTP プロキシなど)、またはエンドポイント間でメッセージをやりとりする場合に必要となる SOAP を使用する手段 (SOAP ベースのルーターやブリッジなど) が含まれます。 信頼できるセッションでは、メモリ内転送ウィンドウを使用して、トランスポート エラーが発生した場合に SOAP メッセージ レベル エラーをマスクし、接続を再確立します。  
  
 信頼できるセッションは、待ち時間の短い、信頼できるメッセージ転送を実現します。 これらは、TCP が IP ブリッジ経由のパケットで実現するものと同等の転送を、プロキシや中継局経由の SOAP メッセージで実現します。 信頼できるセッションの詳細については、「 [信頼できるセッション](./feature-details/reliable-sessions.md)」を参照してください。  
  
### <a name="queues"></a>キュー  

 WCF のキューは、待機時間の長いコストで、メッセージの信頼性の高い転送と、送信元と送信先の分離の両方を提供します。 WCF キュー通信は、メッセージキュー (MSMQ) 上に構築されています。  
  
 MSMQ は Windows のオプション コンポーネントとして付属します。 MSMQ サービスは Windows サービスの 1 つとして実行されます。 MSMQ サービスは、送信元の代わりに転送キューで転送用のメッセージを取得し、ターゲット キューに配信します。 ターゲット キューは、送信先の代わりにメッセージを受け取り、後で送信先がメッセージを要求したときに配信します。 MSMQ マネージャーは信頼できるメッセージ転送プロトコルを実装するため、転送中にメッセージが失われることはありません。 このプロトコルは、ネイティブまたは SOAP リライアブル メッセージ プロトコル (SRMP) と呼ばれる SOAP ベースのプロトコルです。  
  
 キュー間でのメッセージの信頼できる転送に加え、送信元と送信先の分離により、疎結合されたアプリケーションで信頼できる通信を実現できます。 信頼できるセッションとは異なり、送信元と送信先が同時に実行されている必要はありません。 このため、送信元によるメッセージの生成レートと送信先によるメッセージの消費レートが一致しないときに、キューが実質上、負荷平準化機構として使用されるというシナリオが暗黙的に可能になります。 キューの詳細については、「 [WCF のキュー](./feature-details/queues-in-wcf.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [信頼できるセッションの概要](./feature-details/reliable-sessions-overview.md)
- [WCF でのキュー](./feature-details/queuing-in-wcf.md)
