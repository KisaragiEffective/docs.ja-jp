---
title: 接続のグループ化
ms.date: 03/30/2017
helpviewer_keywords:
- Internet, connections
- connections [.NET Framework], grouping
- WebRequest class, connection grouping
- network resources, connections
- connection pooling
ms.assetid: 2ec502e8-4ba0-4c22-9410-f28eaf4eee63
ms.openlocfilehash: 007366764a7b8e1208e22ef5895e6a9093b090e4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048641"
---
# <a name="connection-grouping"></a>接続のグループ化
接続のグループ化では、1 つのアプリケーション内の特定の要求を定義済みの接続プールに関連付けます。 これは、ユーザーの代わりにバック エンド サーバーに接続し、デリゲートをサポートする認証プロトコル (Kerberos など) を使用する中間層アプリケーションや、以下の例のように、独自の資格情報を指定する中間層アプリケーションで必要になる場合があります。 たとえば、Joe というユーザーが、自分の給与情報を表示する内部 Web サイトにアクセスするとします。 Joe の認証後、中間層アプリケーション サーバーは、Joe の資格情報を使用してバック エンド サーバーに接続し、Joe の給与情報を取得します。 次に、Susan がサイトにアクセスし、自分の給与情報を要求します。 中間層アプリケーションが Joe の資格情報を使用して既に接続しているため、バック エンド サーバーは Joe の情報で応答します。 ただし、アプリケーションがバック エンド サーバーに送信される各要求をユーザー名から形成される接続グループに割り当てると、各ユーザーは個別の接続プールに属すことになり、誤って他のユーザーと認証情報を共有することがなくなります。  
  
 要求を特定の接続グループに割り当てるには、要求を行う前に、<xref:System.Net.WebRequest.ConnectionGroupName%2A> の <xref:System.Net.WebRequest> プロパティに名前を割り当てる必要があります。  
  
## <a name="see-also"></a>参照

- [接続の管理](managing-connections.md)
- [方法: グループの接続にユーザー情報を割り当てる](how-to-assign-user-information-to-group-connections.md)
