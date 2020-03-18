---
title: WebRequest からの派生
ms.date: 03/30/2017
helpviewer_keywords:
- WebRequest class, pluggable protocols
- protocol-specific request handler
- sending data, pluggable protocols
- pluggable protocols, class criteria
- Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
ms.assetid: 9810c177-973e-43d7-823c-14960bd625ea
ms.openlocfilehash: 6bee864f8d24076d16f226c29d61801e856739d9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048610"
---
# <a name="deriving-from-webrequest"></a>WebRequest からの派生
<xref:System.Net.WebRequest> クラスは、.NET Framework プラグ可能なプロトコル モデルに適合するプロトコル固有の要求ハンドラーを作成するための基本メソッドとプロパティを提供する抽象基底クラスです。 **WebRequest** クラスを使用するアプリケーションは、使用されるプロトコルを指定することなく、サポートされている任意のプロトコルを使用してデータを要求できます。  
  
 プロトコル固有のクラスがプラグ可能なプロトコルとして使用されるようにするには、クラスが <xref:System.Net.IWebRequestCreate> インターフェイスを実装する、およびそれを <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> メソッドに登録するという 2 つの条件を満たす必要があります。 クラスは、**WebRequest** のすべての抽象メソッドとプロパティをオーバーライドし、プラグ可能なインターフェイスを提供する必要があります。  
  
 **WebRequest** インスタンスは 1 回限りの使用を意図しており、別の要求を作成する場合は、新しい **WebRequest** を作成します。 **WebRequest** は、開発者がテンプレート <xref:System.Runtime.Serialization.ISerializable>WebRequest **をシリアル化して、そのテンプレートを追加の要求用に再構成できるように、** インターフェイスをサポートしています。  
  
## <a name="iwebrequest-create-method"></a>IWebRequest Create メソッド  
 <xref:System.Net.IWebRequestCreate.Create%2A> メソッドは、プロトコル固有のクラスの新しいインスタンスの初期化を行います。 新しい **WebRequest** が作成されると、<xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> メソッドは要求された URI を **RegisterPrefix** メソッドに登録されている URI プレフィックスと照合します。 適切なプロトコル固有の子孫の **Create** メソッドは、プロトコル固有のフィールドを変更することなく、プロトコルの標準的な要求/応答のトランザクションを実行できる子孫の初期化されたインスタンスを返す必要があります。  
  
## <a name="connectiongroupname-property"></a>ConnectionGroupName プロパティ  
 <xref:System.Net.WebRequest.ConnectionGroupName%2A> プロパティは、1 つの接続を介して複数の要求が作成できるように、リソースへの接続のグループを指定するために使用されます。 接続の共有を実装するには、プーリングと接続の割り当てのプロトコル固有のメソッドを使用する必要があります。 たとえば、指定した <xref:System.Net.ServicePointManager> クラスは <xref:System.Net.HttpWebRequest> クラスの接続の共有を実装します。 **ServicePointManager** クラスは、各接続グループに特定のサーバーへの接続を提供する <xref:System.Net.ServicePoint> を作成します。  
  
## <a name="contentlength-property"></a>ContentLength プロパティ  
 <xref:System.Net.WebRequest.ContentLength%2A> プロパティは、データをアップロードするときに、サーバーに送信されるデータのバイト数を指定します。  
  
 通常、<xref:System.Net.WebRequest.Method%2A> プロパティは、**ContentLength** プロパティが 0 より大きい値に設定されている場合に、アップロードが行われることを示すために設定する必要があります。  
  
## <a name="contenttype-property"></a>ContentType プロパティ  
 <xref:System.Net.WebRequest.ContentType%2A> プロパティは、送信中のコンテンツ タイプを識別するためにサーバーに送信するようにプロトコルで求められる特別な情報を提供します。 通常、これはアップロードされる任意のデータの MIME コンテンツ タイプです。  
  
## <a name="credentials-property"></a>Credentials プロパティ  
 <xref:System.Net.WebRequest.Credentials%2A> プロパティには、サーバーでの要求の認証に必要な情報が含まれています。 プロトコルの認証プロセスの詳細を実装する必要があります。 <xref:System.Net.AuthenticationManager> クラスは、要求の認証と認証トークンの提供を行います。 プロトコルで使用する資格情報を提供するクラスは、<xref:System.Net.ICredentials> インターフェイスを実装する必要があります。  
  
## <a name="headers-property"></a>Headers プロパティ  
 <xref:System.Net.WebRequest.Headers%2A> プロパティには、要求に関連付けられているメタデータの名前/値ペアの任意のコレクションが含まれています。 名前/値のペアとして表すことができるプロトコルで必要なすべてのメタデータは、**Headers** プロパティに含めることができます。 通常、この情報は、<xref:System.Net.WebRequest.GetRequestStream%2A> メソッドまたは <xref:System.Net.WebRequest.GetResponse%2A> メソッドを呼び出す前に設定する必要があります。要求が作成されると、メタデータは読み取り専用と見なされます。  
  
 ヘッダー メタデータを使用するために **Headers** プロパティを使用する必要はありません。 プロトコル固有のメタデータをプロパティとして公開することができます。たとえば、<xref:System.Net.HttpWebRequest.UserAgent%2A?displayProperty=nameWithType> プロパティは、**User-agent** HTTP ヘッダーを公開します。 ヘッダー メタデータをプロパティとして公開する場合、**Headers** プロパティを使用して同じプロパティが設定されないようにする必要があります。  
  
## <a name="method-property"></a>Method プロパティ  
 <xref:System.Net.WebRequest.Method%2A> プロパティには、要求がサーバーに実行を求める動詞やアクションが含まれています。 **Method** プロパティの既定値は、プロトコル固有のプロパティを設定しなくても、標準的な要求/応答アクションを有効にする必要があります。 たとえば、<xref:System.Net.HttpWebResponse.Method%2A> メソッドの既定値は GET で、Web サーバーからリソースを要求し、応答を返します。  
  
 **Method** プロパティが、アップロードが行われることを示す動詞またはアクションに設定されている場合、通常、**ContentLength** プロパティは 0 より大きい値に設定されている必要があります。  
  
## <a name="preauthenticate-property"></a>PreAuthenticate プロパティ  
 アプリケーションは <xref:System.Net.WebRequest.PreAuthenticate%2A> プロパティを設定して、認証チャレンジを待機するのではなく、最初の要求で認証情報が送信されることを示します。 **PreAuthenticate** プロパティは、プロトコルが最初の要求で送信された認証資格情報をサポートしている場合にのみ意味があります。  
  
## <a name="proxy-property"></a>Proxy プロパティ  
 <xref:System.Net.WebRequest.Proxy%2A> プロパティには、要求されたリソースへのアクセスに使用される <xref:System.Net.IWebProxy> インターフェイスが含まれています。 **Proxy** プロパティは、プロトコルがプロキシ処理された要求をサポートしている場合にのみ意味があります。 プロトコルで既定のプロキシが必要な場合は、それを設定する必要があります。  
  
 企業のファイアウォールの背後などの一部の環境では、プロトコルでプロキシの使用が必要な場合があります。 その場合、**IWebProxy** インターフェイスを実装して、プロトコルで機能するプロキシ クラスを作成する必要があります。  
  
## <a name="requesturi-property"></a>RequestUri プロパティ  
 <xref:System.Net.WebRequest.RequestUri%2A> プロパティには、**WebRequest.Create** メソッドに渡された URI が含まれます。 これは読み取り専用で、**WebRequest** が作成されると変更できません。 プロトコルでリダイレクトがサポートされている場合は、別の URI で識別されたリソースからの応答を受け取れます。 応答した URI へのアクセスを提供する必要がある場合は、その URI を含む追加のプロパティを指定する必要があります。  
  
## <a name="timeout-property"></a>Timeout プロパティ  
 <xref:System.Net.WebRequest.Timeout%2A> プロパティには、待機する時間の長さ (ミリ秒) が含まれます。この時間を超えると、要求がタイムアウトになり、例外がスローされます。 **Timeout** は、<xref:System.Net.WebRequest.GetResponse%2A> メソッドで作成された同期要求のみに適用されます。 非同期要求は、<xref:System.Net.WebRequest.Abort%2A> メソッドを使用して保留中の要求をキャンセルする必要があります。  
  
 **Timeout** プロパティの設定は、プロトコル固有のクラスがタイムアウト プロセスを実装する場合にのみ意味があります。  
  
## <a name="abort-method"></a>Abort メソッド  
 <xref:System.Net.WebRequest.Abort%2A> メソッドは、保留中のサーバーへの非同期要求をキャンセルします。 要求がキャンセルされた後に、**GetResponse**、**BeginGetResponse**、**EndGetResponse**、**GetRequestStream**、**BeginGetRequestStream**、**EndGetRequestStream** のいずれかを呼び出すと、<xref:System.Net.WebException> に設定された <xref:System.Net.WebException.Status%2A> プロパティで <xref:System.Net.WebExceptionStatus> がスローされます。  
  
## <a name="begingetrequeststream-and-endgetrequeststream-methods"></a>BeginGetRequestStream メソッドと EndGetRequestStream メソッド  
 <xref:System.Net.WebRequest.BeginGetRequestStream%2A> メソッドは、サーバーへのデータのアップロードに使用されるストリームへの非同期要求を開始します。 <xref:System.Net.WebRequest.EndGetRequestStream%2A> メソッドは非同期要求を完了し、要求されたストリームを返します。 これらのメソッドは標準の .NET Framework 非同期パターンを使用して、**GetRequestStream** メソッドを実装します。  
  
## <a name="begingetresponse-and-endgetresponse-methods"></a>BeginGetResponse メソッドと EndGetResponse メソッド  
 <xref:System.Net.WebRequest.BeginGetResponse%2A> メソッドは、サーバーへの非同期要求を開始します。 <xref:System.Net.WebRequest.EndGetResponse%2A> メソッドは非同期要求を完了し、要求された応答を返します。 これらのメソッドは標準の .NET Framework 非同期パターンを使用して、**GetResponse** メソッドを実装します。  
  
## <a name="getrequeststream-method"></a>GetRequestStream メソッド  
 <xref:System.Net.WebRequest.GetRequestStream%2A> メソッドは要求されたサーバーにデータを書き込むために使用されるストリームを返します。 返されたストリームは、シークしない書き込み専用ストリームにする必要があります。これはサーバーに書き込まれるデータの一方向のストリームとすることを目的にしています。 ストリームは <xref:System.IO.Stream.CanRead%2A> プロパティと <xref:System.IO.Stream.CanSeek%2A> プロパティに false を返し、<xref:System.IO.Stream.CanWrite%2A> プロパティに true を返します。  
  
 通常、**GetRequestStream** メソッドはサーバーへの接続を開き、データがサーバーに送信中であることを示すヘッダー情報を送信してからストリームを返します。 **Getrequeststream** が要求を開始するため、**GetRequestStream** を呼び出した後に、**Header** プロパティまたは **ContentLength** プロパティを設定することは、通常許可されません。  
  
## <a name="getresponse-method"></a>GetResponse メソッド  
 <xref:System.Net.WebRequest.GetResponse%2A> メソッドは、サーバーからの応答を表す <xref:System.Net.WebResponse> クラスのプロトコル固有の子孫を返します。 **GetRequestStream** メソッドによって要求がすでに開始されている場合を除き、**GetResponse** メソッドは **RequestUri** で識別されたリソースへの接続を作成し、作成されている要求のタイプを示すヘッダー情報を送信してから、リソースからの応答を受け取ります。  
  
 **GetResponse** メソッドが呼び出されると、すべてのプロパティは読み取り専用と見なされます。 **WebRequest** インスタンスは 1 回限りの使用を意図しており、別の要求を作成する場合は、新しい **WebRequest** を作成する必要があります。  
  
 **GetResponse** メソッドは、受信した応答を含めるための適切な **WebResponse** の子孫を作成します。  
  
## <a name="see-also"></a>参照

- <xref:System.Net.WebRequest>
- <xref:System.Net.HttpWebRequest>
- <xref:System.Net.FileWebRequest>
- [プラグ可能なプロトコルのプログラミング](programming-pluggable-protocols.md)
- [WebResponse からの派生](deriving-from-webresponse.md)
