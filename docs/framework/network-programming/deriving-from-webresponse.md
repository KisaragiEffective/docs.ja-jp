---
title: WebResponse からの派生
ms.date: 03/30/2017
helpviewer_keywords:
- Deriving from WebResponse
ms.assetid: f11d4866-a199-4087-9306-a5a4c18b13db
ms.openlocfilehash: bd06928b08eb085ef13371687fb1e5b92c6c1d86
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048575"
---
# <a name="deriving-from-webresponse"></a>WebResponse からの派生
<xref:System.Net.WebResponse> クラスは、.NET Framework プラグ可能なプロトコル モデルに適合するプロトコル固有の応答を作成するための基本メソッドとプロパティを提供する抽象基底クラスです。 <xref:System.Net.WebRequest> クラスを使用してリソースからデータを要求するアプリケーションは、**WebResponse** で応答を受信します。 プロトコル固有の **WebResponse**の子孫は、**WebResponse** クラスの抽象メンバーを実装する必要があります。  
  
 関連付けられた **WebRequest** クラスは、**WebResponse** の子孫を作成する必要があります。 たとえば、<xref:System.Net.HttpWebResponse> インスタンスは、<xref:System.Net.HttpWebRequest.GetResponse%2A?displayProperty=nameWithType> または <xref:System.Net.HttpWebRequest.EndGetResponse%2A?displayProperty=nameWithType> の呼び出しの結果としてのみ作成されます。 各 **WebResponse** には、リソースへの要求の結果が含まれており、再利用されることを意図していません。  
  
## <a name="contentlength-property"></a>ContentLength プロパティ  
 <xref:System.Net.WebResponse.ContentLength%2A> プロパティは、<xref:System.Net.WebResponse.GetResponseStream%2A> メソッドによって返されるストリームから利用可能なデータのバイト数を示します。 **ContentLength** プロパティは、サーバーによって返されるヘッダーのバイト数またはメタデータ情報を示しません。要求されたリソース自体のデータのバイト数のみを示します。  
  
## <a name="contenttype-property"></a>ContentType プロパティ  
 <xref:System.Net.WebResponse.ContentType%2A> プロパティは、サーバーによって送信中のコンテンツ タイプを識別するためにクライアントに送信するようにプロトコルで求められる特別な情報を提供します。 通常、これは返される任意のデータの MIME コンテンツ タイプです。  
  
## <a name="headers-property"></a>Headers プロパティ  
 <xref:System.Net.WebResponse.Headers%2A> プロパティには、応答に関連付けられているメタデータの名前/値ペアの任意のコレクションが含まれています。 名前/値のペアとして表すことができるプロトコルで必要なすべてのメタデータは、**Headers** プロパティに含めることができます。  
  
 ヘッダー メタデータを使用するために **Headers** プロパティを使用する必要はありません。 プロトコル固有のメタデータをプロパティとして公開することができます。たとえば、<xref:System.Net.HttpWebResponse.LastModified%2A?displayProperty=nameWithType> プロパティは、**Last-Modified** HTTP ヘッダーを公開します。 ヘッダー メタデータをプロパティとして公開する場合、**Headers** プロパティを使用して同じプロパティが設定されないようにする必要があります。  
  
## <a name="responseuri-property"></a>ResponseUri プロパティ  
 <xref:System.Net.WebResponse.ResponseUri%2A> プロパティには、実際に応答を提供したリソースの URI が含まれています。 リダイレクトをサポートしないプロトコルの場合、**ResponseUri** は応答を作成した <xref:System.Net.WebRequest.RequestUri%2A>WebRequest**の** プロパティと同じになります。 プロトコルが要求のリダイレクトをサポートしている場合は、**ResponseUri** に応答の URI が含まれます。  
  
## <a name="close-method"></a>Close メソッド  
 <xref:System.Net.WebResponse.Close%2A> メソッドは、要求と応答によって作成されたすべての接続を閉じ、応答で使用されているリソースをクリーンアップします。 **Close** メソッドは、応答で使用されたすべてのストリーム インスタンスを閉じますが、応答ストリームが <xref:System.IO.Stream.Close%2A?displayProperty=nameWithType> メソッドへの呼び出しにより以前に閉じられた場合は、例外をスローしません。  
  
## <a name="getresponsestream-method"></a>GetResponseStream メソッド  
 <xref:System.Net.WebResponse.GetResponseStream%2A> メソッドは要求されたリソースからの応答を含むストリームを返します。 応答ストリームには、リソースによって返されるデータのみが含まれています。応答に含まれるヘッダーやメタデータをすべて応答から除去し、プロトコル固有のプロパティまたは **Headers** プロパティ使用して、アプリケーションに公開する必要があります。  
  
 **GetResponseStream** メソッドによって返されるストリーム インスタンスは、アプリケーションによって所有され、**WebResponse** を閉じずに閉じることができます。 規則により、**WebResponse.Close** メソッドの呼び出しも **GetResponse** で返されるストリームを閉じます。  
  
## <a name="see-also"></a>参照

- <xref:System.Net.WebResponse>
- <xref:System.Net.HttpWebResponse>
- <xref:System.Net.FileWebResponse>
- [プラグ可能なプロトコルのプログラミング](programming-pluggable-protocols.md)
- [WebRequest からの派生](deriving-from-webrequest.md)
