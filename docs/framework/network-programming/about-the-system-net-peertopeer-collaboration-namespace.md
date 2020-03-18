---
title: System.Net.PeerToPeer.Collaboration 名前空間について
ms.date: 03/30/2017
ms.assetid: b5d8c1c1-6844-4947-9759-c7f1b564bded
ms.openlocfilehash: f0c9ecaacc1d875aac8eed61a85ca7579f5cb8a1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "64649524"
---
# <a name="about-the-systemnetpeertopeercollaboration-namespace"></a>System.Net.PeerToPeer.Collaboration 名前空間について
<xref:System.Net.PeerToPeer.Collaboration> 名前空間は、ピア ツー ピア共同作業インフラストラクチャを使用してピア共同作業アクティビティを実装するために使用されるクラスと API を提供します。  
  
## <a name="classes"></a>クラス  
 ピア ツー ピア共同作業アクティビティの実装に使用される主なクラスは次のとおりです。  
  
- ピアの連絡先を格納するために使用できる <xref:System.Net.PeerToPeer.Collaboration.ContactManager>。  
  
- ゲーム、チャット クライアント、または会議ソリューションなど、共同作業を行う <xref:System.Net.PeerToPeer.Collaboration.PeerApplication>。  
  
- アクティビティで共同作業を行うピア。  これらのピアは <xref:System.Net.PeerToPeer.Collaboration.PeerContact>、<xref:System.Net.PeerToPeer.Collaboration.PeerNearMe>、または <xref:System.Net.PeerToPeer.Collaboration.PeerEndPoint> オブジェクトとして表すことができます。  
  
- 使用可能なアプリケーションと、そのアプリケーションに参加するピアを指定する、静的な <xref:System.Net.PeerToPeer.Collaboration.PeerCollaboration> クラス自体。  
  
 <xref:System.Net.PeerToPeer.Collaboration.PeerContact.Invite%2A> メソッドは、共同作業セッションにピアを招待するために使用されます。  呼び出し元のピアは、共同作業セッションに関連付けられているアプリケーション、オブジェクト、またはプレゼンス情報の更新を通知するイベントの別のピアにサブスクライブできます。 プレゼンス クラスは <xref:System.Net.PeerToPeer.Collaboration.Peer> が共同作業で使用可能かどうかを指定し、<xref:System.Net.PeerToPeer.Collaboration.PeerScope> クラスはピアに許可される参加レベルとして <xref:System.Net.PeerToPeer.Collaboration.PeerScope.Internet> (グローバル)、<xref:System.Net.PeerToPeer.Collaboration.PeerScope.NearMe> (サブネット) または <xref:System.Net.PeerToPeer.Collaboration.PeerScope.None> を指定するために使用されます。  
  
 共同作業セッションは、次の 4 つの手順で構成されます。  
  
- 検出。 アプリケーション、ピア、およびプレゼンス情報を検出または公開します。  たとえば、同じゲームをインストールしている、ローカル サブネット上の他のユーザーを見つけます。  
  
- 招待。 <xref:System.Net.PeerToPeer.Collaboration.PeerCollaboration> セッションを開始する、またはそのセッションに参加するリモート ピア (複数可) のセキュリティで保護された招待状を送信および承諾します。  
  
- 連絡先の管理。 <xref:System.Net.PeerToPeer.Collaboration.ContactManager> に連絡先として検出されたピアを追加します。  
  
- 通信。 通信が確立されたら、<xref:System.Net> API、<xref:System.Net.PeerToPeer> API、または Windows Communication Foundation のピア チャネル クラス (マルチパーティ通信用) を使用します。  
  
 たとえば、ホスト ピアは共同作業セッションを開始し、<xref:System.Net.PeerToPeer.Collaboration.ContactManager.CreateContact%2A> メソッドを利用して、リモート ピアとそのローカル ピアのいずれかをホスト ピアの Contact Manager に追加します。  その後、3 人のユーザーは独自のプライベート共同作業セッションに参加します。  
  
 一般的な P2P アプリケーションには、共有ノート作成機能またはホワイトボードを利用する電話会議、サーバーレスのチャット アプリケーション、双方向広告、およびオンライン ゲーム セッションなどがあります。  
  
## <a name="see-also"></a>参照

- <xref:System.Net.PeerToPeer.Collaboration>
