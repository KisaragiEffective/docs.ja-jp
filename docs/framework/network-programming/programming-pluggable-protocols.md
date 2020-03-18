---
title: プラグ可能なプロトコルのプログラミング
ms.date: 03/30/2017
helpviewer_keywords:
- downloading Internet resources, pluggable protocols
- WebRequest class, pluggable protocols
- response to Internet request, pluggable protocols
- WebResponse class, pluggable protocols
- sending data, pluggable protocols
- network resources, pluggable protocols
- Internet, pluggable protocols
- programming pluggable protocols
- pluggable protocols, programming
- requesting data from Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
ms.assetid: 66ef8456-7576-4e97-8956-959b216373db
ms.openlocfilehash: 94dfedd317782b9e518df02c84d9af55b1ef2b69
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047398"
---
# <a name="programming-pluggable-protocols"></a>プラグ可能なプロトコルのプログラミング
抽象クラスの <xref:System.Net.WebRequest> と <xref:System.Net.WebResponse> は、プラグ可能なプロトコルの基礎を提供します。 アプリケーションでは、<xref:System.Net.WebRequest> と <xref:System.Net.WebResponse> からプロトコル固有のクラスを派生することにより、使うプロトコルを指定しなくても、インターネット リソースにデータを要求して応答を読み取ることができます。  
  
 プロトコル固有の <xref:System.Net.WebRequest> を作成するには、その前に Create メソッドを登録する必要があります。 <xref:System.Net.WebRequest.RegisterPrefix%28System.String%2CSystem.Net.IWebRequestCreate%29> の静的 <xref:System.Net.WebRequest> メソッドを使って、特定のインターネット スキーム、スキームとサーバー、またはスキームとサーバーとパスに対する要求のセットを処理するための、<xref:System.Net.WebRequest> の子孫を登録します。  
  
 ほとんどの場合は、<xref:System.Net.WebRequest> クラスのメソッドとプロパティを使ってデータを送受信できます。 ただし、プロトコル固有のプロパティにアクセスする必要がある場合は、<xref:System.Net.WebRequest> を特定の派生クラス インスタンスに型キャストできます。  
  
 プラグ可能なプロトコルを利用するには、<xref:System.Net.WebRequest> の子孫で、プロトコル固有のプロパティを設定する必要がない既定の要求-応答トランザクションを提供する必要があります。 たとえば、HTTP 用の <xref:System.Net.HttpWebRequest> クラスを実装する <xref:System.Net.WebRequest> クラスは、既定で `GET` 要求を提供し、Web サーバーから返されたストリームを含む <xref:System.Net.HttpWebResponse> を返します。  
  
## <a name="see-also"></a>参照

- [WebRequest からの派生](deriving-from-webrequest.md)
- [WebResponse からの派生](deriving-from-webresponse.md)
- [.NET Framework のネットワーク プログラミング](index.md)
- [方法: WebRequest を型キャストしてプロトコル固有のプロパティにアクセスする](how-to-typecast-a-webrequest-to-access-protocol-specific-properties.md)
