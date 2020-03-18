---
title: HTTP
ms.date: 03/30/2017
helpviewer_keywords:
- protocols, HTTP
- sending data, HTTP
- HttpWebResponse class, sending and receiving data
- HTTP
- receiving data, HTTP
- application protocols, HTTP
- Internet, HTTP
- network resources, HTTP
- HTTP, about HTTP
- HttpWebRequest class, sending and receiving data
ms.assetid: 985fe5d8-eb71-4024-b361-41fbdc1618d8
ms.openlocfilehash: c8c799a50e5d63bbf411c338eb9e93f85a942bb0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048012"
---
# <a name="http"></a>HTTP
.NET Framework は、<xref:System.Net.HttpWebRequest> クラスと <xref:System.Net.HttpWebResponse> クラスを使用して、すべてのインターネット トラフィックの大部分を構成する HTTP プロトコルに対して包括的なサポートを提供します。 <xref:System.Net.WebRequest> と <xref:System.Net.WebResponse> から派生したこれらのクラスは、静的メソッド <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> が "http" または "https" で始まる URI に遭遇するたびに既定で返されます。 ほとんどの場合、**WebRequest** クラスと **WebResponse** クラスは、要求を行うために必要なすべてを提供しますが、プロパティとして公開されている HTTP 固有の機能にアクセスする必要がある場合は、これらのクラスを **HttpWebRequest** または **HttpWebResponse** に型キャストすることができますです。  
  
 **HttpWebRequest** と **HttpWebResponse** は、標準の HTTP 要求-応答のトランザクションをカプセル化し、一般的な HTTP ヘッダーへのアクセスを提供します。 これらのクラスは、パイプライン処理、チャック内データの送受信、認証、事前認証、暗号化、プロキシのサポート、サーバー証明書の検証、接続の管理を含むほとんどの HTTP 1.1 の機能もサポートします。 カスタム ヘッダー、およびプロパティを介して提供されていないヘッダーを格納し、**Headers** プロパティを介してアクセスすることができます。  
  
 **HttpWebRequest** は、**WebRequest** によって使用される既定のクラスであり、**WebRequest.Create** メソッドに URI を渡す前に登録する必要はありません。  
  
 <xref:System.Net.HttpWebRequest.AllowAutoRedirect%2A> プロパティを **true** (既定) に設定することで自動的に HTTP リダイレクトに従うアプリケーションを作成することができます。 アプリケーションが、要求をリダイレクトし、<xref:System.Net.HttpWebResponse.ResponseUri%2A>HttpWebResponse**の** プロパティに要求に応答した実際の Web リソースが含まれます。 **AllowAutoRedirect** を **false** に設定した場合、アプリケーションは、HTTP プロトコル エラーとしてリダイレクトを処理できる必要があります。  
  
 アプリケーションでは、<xref:System.Net.WebException> を <xref:System.Net.WebException.Status%2A> に設定し、<xref:System.Net.WebExceptionStatus> をキャッチすることによって HTTP プロトコル エラーを受信します。 <xref:System.Net.WebException.Response%2A> プロパティは、サーバーによって送信された **WebResponse** を含み、実際の HTTP エラーがに発生したことを示します。  
  
## <a name="see-also"></a>参照

- [プロキシを介したインターネットへのアクセス](accessing-the-internet-through-a-proxy.md)
- [アプリケーション プロトコルの使用](using-application-protocols.md)
- [方法: HTTP 固有のプロパティにアクセスする](how-to-access-http-specific-properties.md)
