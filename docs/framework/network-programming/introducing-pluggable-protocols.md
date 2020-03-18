---
title: プラグ可能なプロトコルの概要
ms.date: 03/30/2017
helpviewer_keywords:
- data requests, pluggable protocols
- WebRequest class, pluggable protocols
- response to Internet request, pluggable protocols
- URI
- Windows Sockets
- request/response model
- sending data, pluggable protocols
- pluggable protocols
- WebClient class, about WebClient class
- pluggable protocols, about pluggable protocols
- Internet, pluggable protocols
- path identifiers
- Uniform Resource Identifier
- application development [.NET Framework], pluggable protocols
- requesting data from Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
- server identifiers
- scheme identifiers
ms.assetid: 4b48e22d-e4e5-48f0-be80-d549bda97415
ms.openlocfilehash: 72b47b8159f9f6f0dc3a19c5cbf94335507d9e7d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047858"
---
# <a name="introducing-pluggable-protocols"></a>プラグ可能なプロトコルの概要
Microsoft .NET Framework は、アプリケーションにすばやく簡単に統合できる、複数層の拡張可能なインターネット サービスのマネージド実装を提供します。 <xref:System.Net> および <xref:System.Net.Sockets> の名前空間内のインターネット アクセス クラスは、Web ベース アプリケーションとインターネット ベース アプリケーションの両方を実装するために使用できます。  
  
## <a name="internet-applications"></a>インターネット アプリケーション  
 インターネット アプリケーションは、情報を要求するクライアント アプリケーションと、クライアントからの情報の要求に応答するサーバー アプリケーションの 2 種類に大別することができます。 従来のインターネット クライアント/サーバー アプリケーションは、ユーザーがブラウザーを使用して世界中の Web サーバーに格納されているドキュメントやその他のデータにアクセスする World Wide Web です。  
  
 アプリケーションはこれらの役割の 1 つに限定されません。たとえば、使い慣れた中間層アプリケーション サーバーは、別のサーバーからデータを要求することでクライアントからの要求に応答します。この場合、サーバーとクライアントの両方として機能します。  
  
 クライアント アプリケーションは、要求されたインターネット リソースと、要求と応答に使用するための通信プロトコルを識別することによって、要求を実行します。 必要に応じて、クライアントは要求を完了するために必要な、プロキシの場所や認証情報 (ユーザー名、パスワードなど) などの追加データも提供します。 要求が作成されると、要求をサーバーに送信できます。  
  
## <a name="identifying-resources"></a>リソースの識別  
 .NET Framework は、Uniform Resource Identifier (URI) を使用して、要求されたインターネット リソースと通信プロトコルを識別します。 URI は、少なくとも 3 つ、または 4 つのフラグメントで構成されています。要求と応答の通信プロトコルを識別するスキーム識別子、ドメイン ネーム システム (DNS) ホスト名またはインターネット上のサーバーを一意に識別する TCP アドレスのいずれかで構成されるサーバー識別子、要求された情報をサーバー上で検索するパス識別子、クライアントからの情報をサーバーに渡すクエリ文字列 (省略可能) です。 たとえば、URI `http://www.contoso.com/whatsnew.aspx?date=today` はスキーム識別子 `http`、サーバー識別子 `www.contoso.com`、パス `/whatsnew.aspx`、クエリ文字列 `?date=today` で構成されます。  
  
 サーバーは要求を受信し、応答を処理すると、クライアント アプリケーションに応答を返します。 応答には、コンテンツの種類 (たとえば、生のテキストや XML データ) などの補足情報が含まれています。  
  
## <a name="requests-and-responses-in-the-net-framework"></a>.NET Framework での要求と応答  
 .NET Framework では、特定のクラスを使用して、要求/応答モデルを通じてインターネット リソースにアクセスするために必要な 3 つの情報を提供します。求めているインターネット リソースの URI が含まれている <xref:System.Uri> クラス、リソースの要求が含まれている <xref:System.Net.WebRequest> クラス、および受信した応答のコンテナーを提供する <xref:System.Net.WebResponse> クラスです。  
  
 クライアント アプリケーションは、ネットワーク リソースの URI を `WebRequest` メソッドに渡すことによって、<xref:System.Net.WebRequest.Create%2A> インスタンスを作成します。 この静的メソッドは、HTTP などの特定のプロトコルの `WebRequest` を作成します。 返される `WebRequest` は、サーバーへの要求と、要求が行われたときに送信されるデータ ストリームへのアクセスの両方を制御するプロパティへのアクセスを提供します。 <xref:System.Net.WebRequest.GetResponse%2A> の `WebRequest` メソッドは、クライアント アプリケーションから URI で識別されたサーバーに要求を送信します。 応答遅延が発生する場合、<xref:System.Net.WebRequest.BeginGetResponse%2A>WebRequest**で** メソッドを使用して、要求を非同期的に実行し、<xref:System.Net.WebRequest.EndGetResponse%2A> メソッドを使用して後で応答を返すことができます。  
  
 **GetResponse** メソッドと **EndGetResponse** メソッドは、サーバーによって返されるデータへのアクセスを提供する **WebResponse** を返します。 このデータは、<xref:System.Net.WebResponse.GetResponseStream%2A> メソッドによって要求元のアプリケーションにストリームとして提供されるため、データ ストリームが使用される任意の場所で、アプリケーションで使用できます。  
  
 **WebRequest** クラスと **WebResponse** クラスは、プラグ可能なプロトコルの基礎です。各リソースが使用しているプロトコルの具体的な詳細を考慮しなくても、インターネット リソースを使用するアプリケーションを開発できるようにするネットワーク サービスの実装です。 **WebRequest** の派生クラスは、インターネット リソースに実際に接続する詳細を管理するため、**WebRequest** クラスに登録されます。  
  
 例として、<xref:System.Net.HttpWebRequest> クラスは、HTTP を使用してインターネット リソースへの接続の詳細を管理します。 既定では、**WebRequest.Create** メソッドが "http:" または "https:" (HTTP およびセキュリティで保護された HTTP のプロトコル識別子) で始まる URI を検出すると、返された **WebRequest** をそのまま使用することができます。または、**HttpWebRequest** に型キャストしてプロトコル固有のプロパティにアクセスすることもできます。 ほとんどの場合、**WebRequest** は要求の実行に必要なすべての情報を提供します。  
  
 要求/応答のトランザクションとして表すことができるプロトコルはすべて、**WebRequest** で使用できます。 **WebRequest** および **WebResponse** からプロトコル固有のクラスを派生して、アプリケーションで使用するためにそれらを静的 <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> メソッドに登録できます。  
  
 インターネット要求のためにクライアントの承認が必要な場合は、<xref:System.Net.WebRequest.Credentials%2A>WebRequest**の** プロパティが必要な資格情報を提供します。 これらの資格情報は、基本的な HTTP 認証またはダイジェスト認証の名前/パスワードの単純な組み合わせにすることも、NTLM 認証または Kerberos 認証の名前/パスワード/ドメインのセットにすることもできます。 1 つの資格情報のセットは、<xref:System.Net.NetworkCredential> インスタンスに格納できます。複数のセットは、<xref:System.Net.CredentialCache> インスタンスに同時に格納できます。 **CredentialCache** は、要求の URI と、サーバーでサポートされている認証スキームを使用して、サーバーに送信する資格情報を決定します。  
  
## <a name="simple-requests-with-webclient"></a>WebClient による単純な要求  
 インターネット リソースに対して単純な要求を行う必要があるアプリケーションの場合、<xref:System.Net.WebClient> クラスがインターネット サーバーにデータをアップロードする、またはインターネット サーバーからデータをダウンロードするための共通のメソッドを提供します。 **WebClient** は **WebRequest** クラスに依存して、インターネット リソースへのアクセスを提供しています。そのため、**WebClient** クラスは登録されている任意のプラグ可能なプロトコルを使用できます。  
  
 要求/応答モデルを使用できないアプリケーションの場合、またはネットワーク上でリッスンするだけでなく、要求を送信する必要があるアプリケーションの場合、**System.Net.Sockets** 名前空間が <xref:System.Net.Sockets.TcpClient>、<xref:System.Net.Sockets.TcpListener>、および <xref:System.Net.Sockets.UdpClient> クラスを提供します。 これらのクラスは、さまざまなトランスポート プロトコルを使用して接続のための詳細を処理し、ネットワーク接続をストリームとしてアプリケーションに公開します。  
  
 Windows ソケット インターフェイスに精通している開発者、またはソケット レベルでのプログラミングにより提供されるコントロールが必要な開発者は、**System.Net.Sockets** クラスにより自身のニーズを満たすことができます。 **System.Net.Sockets** クラスは、**System.Net** クラス内でのマネージド コードからネイティブ コードへの遷移ポイントです。 ほとんどの場合、**System.Net.Sockets** クラスは、Windows の 32 ビットに相当するものにデータをマーシャリングするだけでなく、必要なセキュリティ チェックをすべて処理します。  
  
## <a name="see-also"></a>参照

- [プラグ可能なプロトコルのプログラミング](programming-pluggable-protocols.md)
- [.NET Framework のネットワーク プログラミング](index.md)
- [ネットワーク プログラミングのサンプル](network-programming-samples.md)
