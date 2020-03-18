---
title: .NET Framework のネットワーク プログラミング
ms.date: 03/30/2017
helpviewer_keywords:
- Networking
- Internet
- Internet, .NET Framework Internet services
- Network Resources
ms.assetid: 8d455610-67a0-4fa8-a62f-7747064a9256
ms.openlocfilehash: 1e7f0123ab07fd4e83eea957b72bf79eeeecef2b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74204695"
---
# <a name="network-programming-in-the-net-framework"></a>.NET Framework のネットワーク プログラミング
Microsoft .NET Framework は、アプリケーションにすばやく簡単に統合できる、複数層の拡張可能なインターネット サービスのマネージド実装を提供します。 ネットワーク アプリケーションは、プラグ可能なプロトコルを基に自動的に新しいインターネット プロトコルを使用するように作成することも、ソケット レベルでネットワークを使用できるように Windows ソケット インターフェイスのマネージド実装を使用することもできます。  
  
## <a name="in-this-section"></a>このセクションの内容  

 [プラグ可能なプロトコルの概要](introducing-pluggable-protocols.md)  
 必要なアクセス プロトコルに関係なく、インターネット リソースにアクセスする方法について説明します。  
  
 [データの要求](requesting-data.md)  
 プラグ可能なプロトコルを使用して、インターネット リソースにデータをアップロードしたり、ダウンロードしたりする方法について説明します。  
  
 [プラグ可能なプロトコルのプログラミング](programming-pluggable-protocols.md)  
 プロトコル固有のクラスを派生させて、プラグ可能なプロトコルを実装する方法について説明します。  
  
 [アプリケーション プロトコルの使用](using-application-protocols.md)  
 TCP、UDP、HTTP などのネットワーク プロトコルを利用するアプリケーションのプログラミングについて説明します。  
  
 [インターネット プロトコル バージョン 6](internet-protocol-version-6.md)  
 インターネット プロトコル スイート (IPv4) の現在のバージョンより優れたインターネット プロトコル Version 6 (IPv6) の利点について説明し、IPv6 のアドレス指定、ルーティングおよび自動構成、および IPv6 を有効/無効にする方法について説明します。  
  
 [インターネット アプリケーションの構成](configuring-internet-applications.md)  
 .NET Framework 構成ファイルを使用して、インターネット アプリケーションを構成する方法について説明します。  
  
 [.NET Framework のネットワークのトレース](network-tracing.md)  
 .NET Framework のネットワークのトレースを使用して、メソッド呼び出しについての情報、およびマネージド アプリケーションによって生成されるネットワーク トラフィックについての情報を取得する方法を説明します。  
  
 [ネットワーク アプリケーションのキャッシュ管理](cache-management-for-network-applications.md)  
 <xref:System.Net.WebClient?displayProperty=nameWithType>、 <xref:System.Net.WebRequest?displayProperty=nameWithType>、および <xref:System.Net.HttpWebRequest?displayProperty=nameWithType> クラスを使用するアプリケーションでキャッシュを使用する方法について説明します。  
  
 [ネットワーク プログラミングにおけるセキュリティ](security-in-network-programming.md)  
 標準のインターネット セキュリティと認証の手法を使用する方法について説明します。  
  
 [System.Net クラスのベスト プラクティス](best-practices-for-system-net-classes.md)  
 インターネット アプリケーションを最大限に活用するためのヒントとテクニックを紹介します。  
  
 [プロキシを介したインターネットへのアクセス](accessing-the-internet-through-a-proxy.md)  
 プロキシの設定方法について説明します。  
  
 [ネットワーク情報](networkinformation.md)  
 ネットワーク イベント、変更、統計、およびプロパティの情報を収集する方法について説明し、 <xref:System.Net.NetworkInformation.Ping?displayProperty=nameWithType> クラスを使用してリモート ホストに到達可能であるかどうかを判定する方法についても説明します。  
  
 [バージョン 2.0 での System.Uri 名前空間の変更](changes-to-the-system-uri-namespace-in-version-2-0.md)  
 Version 2.0 で正しくない動作を修正し、使いやすさを改善し、セキュリティを強化するために <xref:System.Uri?displayProperty=nameWithType> クラスに加えられたいくつかの変更について説明します。  
  
 [System.Uri での International Resource Identifier のサポート](international-resource-identifier-support-in-system-uri.md)  
 Version 3.5、3.0 SP1、および 2.0 SP1 で、International Resource Identifier (IRI) および国際化ドメイン名 (IDN) をサポートするために、 <xref:System.Uri?displayProperty=nameWithType> クラスに追加された強化機能について説明します。  
  
 [バージョン 3.5 のソケット パフォーマンスの強化](socket-performance-enhancements-in-version-3-5.md)  
 Version 3.5、3.0 SP1、および 2.0 SP1 で、 <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> クラスに追加された一連の強化機能について説明します。これらの機能によって、目的に特化した高パフォーマンスのソケット アプリケーションで使用できる代替非同期パターンが提供されます。  
  
 [ピア名解決プロトコル](peer-name-resolution-protocol.md)  
 Version 3.5 で、ピア名解決プロトコル (PNRP)、サーバーを使用しない動的な名前登録、および名前解決プロトコルをサポートするために追加されたサポートについて説明します。 これらの新機能は、 <xref:System.Net.PeerToPeer?displayProperty=nameWithType> 名前空間によってサポートされます。  
  
 [ピアツーピア コラボレーション](peer-to-peer-collaboration.md)  
 Version 3.5 で、PNRP に基づくピア ツー ピア コラボレーションをサポートするために追加されたサポートについて説明します。 これらの新機能は、 <xref:System.Net.PeerToPeer.Collaboration?displayProperty=nameWithType> 名前空間によってサポートされます。  
  
 [バージョン 3.5 SP1 における HttpWebRequest の NTLM 認証への変更](changes-to-ntlm-authentication-for-httpwebrequest-in-version-3-5-sp1.md)  
 System.Net 名前空間の <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>、 <xref:System.Net.HttpListener?displayProperty=nameWithType>、 <xref:System.Net.Security.NegotiateStream?displayProperty=nameWithType>、および関連クラスによる統合 Windows 認証の処理方法に影響を与える、Version 3.5 SP1 で行われたセキュリティの変更について説明します。  
  
 [統合 Windows 認証と拡張保護](integrated-windows-authentication-with-extended-protection.md)  
 <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>名前空間および関連名前空間の <xref:System.Net.HttpListener?displayProperty=nameWithType>、 <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>、 <xref:System.Net.Security.SslStream?displayProperty=nameWithType>、 <xref:System.Net.Security.NegotiateStream?displayProperty=nameWithType>、 <xref:System.Net?displayProperty=nameWithType> 、および関連クラスによる統合 Windows 認証の処理方法に影響を与える、拡張保護用の強化機能について説明します。  
  
 [IPv6 および Teredo を使用した NAT トラバーサル](nat-traversal-using-ipv6-and-teredo.md)  
 IPv6 および Teredo を使用する NAT トラバースをサポートするために、 <xref:System.Net?displayProperty=nameWithType>、 <xref:System.Net.NetworkInformation?displayProperty=nameWithType>、および <xref:System.Net.Sockets?displayProperty=nameWithType> 名前空間に追加された強化機能について説明します。  
  
 [Windows ストア アプリのネットワーク分離](network-isolation-for-windows-store-apps.md)  
 <xref:System.Net>、 <xref:System.Net.Http>、および <xref:System.Net.Http.Headers> 名前空間のクラスを Windows 8.x ストア アプリで使用したときのネットワーク分離の影響について説明します。  
  
 [ネットワーク プログラミングのサンプル](network-programming-samples.md)  
 <xref:System.Net>、 <xref:System.Net.Cache>、 <xref:System.Net.Configuration>、 <xref:System.Net.Mail>、 <xref:System.Net.Mime>、 <xref:System.Net.NetworkInformation>、 <xref:System.Net.PeerToPeer>、 <xref:System.Net.Security>、 <xref:System.Net.Sockets> 名前空間のクラスを使用する、ダウンロード可能なネットワーク プログラミング サンプルへのリンク。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Net?displayProperty=nameWithType>  
 最近のネットワークで使用されている多くのプロトコル用の単純なプログラミング インターフェイスを提供します。 この名前空間の <xref:System.Net.WebRequest?displayProperty=nameWithType> および <xref:System.Net.WebResponse?displayProperty=nameWithType> クラスは、プラグ可能なプロトコルの基礎です。  
  
 <xref:System.Net.Cache?displayProperty=nameWithType>  
 取得したリソースのキャッシュ ポリシーを <xref:System.Net.WebRequest?displayProperty=nameWithType> クラスおよび <xref:System.Net.HttpWebRequest?displayProperty=nameWithType> クラスを使用して定義するために使用する型および列挙体を定義します。  
  
 <xref:System.Net.Configuration?displayProperty=nameWithType>  
 アプリケーションが System.Net 名前空間の構成設定にプログラムでアクセスして更新するために使用するクラス。  
  
 <xref:System.Net.Http?displayProperty=nameWithType>  
 最新の HTTP アプリケーション用のプログラミング インターフェイスを提供するクラス。  
  
 <xref:System.Net.Http.Headers?displayProperty=nameWithType>  
 <xref:System.Net.Http?displayProperty=nameWithType> 名前空間で使用される HTTP ヘッダーのコレクションのサポートを提供します。  
  
 <xref:System.Net.Mail?displayProperty=nameWithType>  
 SMTP プロトコルを使用してメールを作成および送信するクラス。  
  
 <xref:System.Net.Mime?displayProperty=nameWithType>  
 Defines types that are used to represent Multipurpose Internet Mail Exchange (MIME) headers used by classes in the <xref:System.Net.Mail?displayProperty=nameWithType> 名前空間のクラスが、MIME (Multipurpose Internet Mail Exchange) ヘッダーを表すために使用する型を定義します。  
  
 <xref:System.Net.NetworkInformation?displayProperty=nameWithType>  
 ネットワーク イベント、変更、統計、およびプロパティについての情報をプログラムで収集するクラス。  
  
 <xref:System.Net.PeerToPeer?displayProperty=nameWithType>  
 ピア名前解決プロトコル (PNRP) のマネージド実装を開発者に提供します。  
  
 <xref:System.Net.PeerToPeer.Collaboration?displayProperty=nameWithType>  
 ピア ツー ピア コラボレーション インターフェイスのマネージド実装を開発者に提供します。  
  
 <xref:System.Net.Security?displayProperty=nameWithType>  
 ホスト間の安全な通信のためのネットワーク ストリームを提供するクラス。  
  
 <xref:System.Net.Sockets?displayProperty=nameWithType>  
 ネットワークへのアクセスの制御を支援する必要のある開発者のための、Windows ソケット (Winsock) インターフェイスのマネージド実装が用意されています。  
  
 <xref:System.Net.WebSockets?displayProperty=nameWithType>  
 WebSocket インターフェイスのマネージド実装を開発者に提供します。  
  
 <xref:System.Uri?displayProperty=nameWithType>  
 URI (Uniform Resource Identifier) をオブジェクトとして表現することによって、その構成要素に簡単にアクセスできるようにします。  
  
 <xref:System.Security.Authentication.ExtendedProtection?displayProperty=nameWithType>  
 アプリケーションの拡張保護を使用した認証をサポートします。  
  
 <xref:System.Security.Authentication.ExtendedProtection.Configuration?displayProperty=nameWithType>  
 アプリケーションの拡張保護を使用した認証の構成をサポートします。  
  
## <a name="see-also"></a>参照

- [.NET Framework でのトランスポート層セキュリティ (TLS) のベスト プラクティス](tls.md)
- [ネットワーク プログラミング方法のトピック](network-programming-how-to-topics.md)
- [ネットワーク プログラミングのサンプル](network-programming-samples.md)
- [HttpClient のサンプル](https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664)
