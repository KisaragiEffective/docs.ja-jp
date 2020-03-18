---
title: .NET Core で使用できない .NET Framework テクノロジ
titleSuffix: ''
description: .NET Core で使用できない .NET Framework テクノロジの詳細情報
author: cartermp
ms.date: 04/30/2019
ms.openlocfilehash: bd2488de653ecdfed261100b4c9019bea58fcab3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77092942"
---
# <a name="net-framework-technologies-unavailable-on-net-core"></a>.NET Core で使用できない .NET Framework テクノロジ

.NET Framework ライブラリで使用できるテクノロジの中には、.NET Core で使用できないものがあります。たとえば、AppDomain、リモート処理、コード アクセス セキュリティ (CAS)、セキュリティ透過性、System.EnterpriseServices などです。 ライブラリがこれらのテクノロジの 1 つ以上に依存する場合、以下に示す代替方法を検討してください。 API の互換性の詳細については、「[.NET Core の破壊的変更](../compatibility/breaking-changes.md)」を参照してください。

API またはテクノロジが現在実装されていないからといって、意図的にサポートされていないわけではありません。 GitHub リポジトリで .NET Core を検索して、発生した特定の問題が設計によるものかどうかを確認します。 このようなインジケーターが見つからない場合は、特定の API とテクノロジを求めるために [dotnet/runtime リポジトリ](https://github.com/dotnet/runtime/issues)でイシューを報告してください。 移植に関する要求の問題は、[port-to-core](https://github.com/dotnet/runtime/labels/port-to-core) ラベルでマークされています。

## <a name="appdomains"></a>AppDomain

アプリケーション ドメイン (AppDomains) はアプリを互いに分離します。 AppDomain ではランタイム サポートが必要で、通常は非常に高額です。 追加のアプリ ドメインの作成はサポートされておらず、今後この機能を追加する予定はありません。 コードの分離のためには、代替方法として別個のプロセスやコンテナーを使用します。 アセンブリを動的に読み込むには、<xref:System.Runtime.Loader.AssemblyLoadContext> クラスを使用します。

.NET Framework からのコードの移行を簡単にするために、.NET Core では <xref:System.AppDomain> API サーフェスの一部を公開しています。 API の中には、正常に機能するもの (<xref:System.AppDomain.UnhandledException?displayProperty=nameWithType> など)、処理を行わないメンバー (<xref:System.AppDomain.SetCachePath%2A> など)、<xref:System.PlatformNotSupportedException> をスローするもの (<xref:System.AppDomain.CreateDomain%2A> など) があります。 [dotnet/runtime GitHub リポジトリ`System.AppDomain`で ](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Private.CoreLib/src/System/AppDomain.cs)[ 参照ソース](https://github.com/dotnet/runtime)に対して使用する種類を確認してください。 必ず、実装されているバージョンに合ったブランチを選択してください。

## <a name="remoting"></a>リモート処理

.NET リモート処理は、問題のあるアーキテクチャであると判断されました。 AppDomain 間の通信で使用されていますが、今後はサポートされなくなります。 また、リモート処理にはランタイム サポートが必要で、維持するのに高いコストがかかります。 これらの理由から、.NET リモート処理は .NET Core でサポートされておらず、また将来サポートが追加される予定もありません。

プロセス間の通信では、リモート処理に代わる方法として、<xref:System.IO.Pipes> クラスまたは <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> クラスなどのプロセス間通信 (IPC) メカニズムを検討してください。

マシン間では、代替方法としてネットワーク ベースのソリューションを使用してください。 可能であれば、HTTP などのオーバーヘッドの少ないプレーンテキストのプロトコルを使用してください。 この場合、ASP.NET Core で使用される Web サーバーの [Kestrel Web Server](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel) も選択できます。 また、ネットワーク ベースのマシン間のシナリオとして、<xref:System.Net.Sockets> の使用も検討してください。 その他のオプションについては、[.NET オープン ソース開発者プロジェクト: メッセージング](https://github.com/Microsoft/dotnet/blob/master/dotnet-developer-projects.md#messaging)に関する記事をご覧ください。

## <a name="code-access-security-cas"></a>CAS (コード アクセス セキュリティ)

サンド ボックスは、マネージド アプリケーションやライブラリが使用または実行するリソースの制限を、ランタイムまたはフレームワークに依存しています。これは [.NET Framework ではサポートされていない](../../framework/misc/code-access-security.md)ため、.NET Core でもサポートされていません。 .NET Framework やランタイムでは、特権の昇格が発生するケースが多すぎるため、このまま CAS をセキュリティ境界と見なすことはできません。 さらに、CAS は実装が複雑化しており、その使用を予定していないアプリケーションでは、多くの場合で正確性のパフォーマンスに影響します。

仮想化、コンテナー、ユーザー アカウントなど、オペレーティング システムが提供するセキュリティ境界を使用して、最小限の特権セットでプロセスを実行します。

## <a name="security-transparency"></a>セキュリティ透過性

CAS と同様に、セキュリティ透過性はサンドボックス コードをセキュリティ クリティカルなコードから宣言的に分離しますが、[現在はセキュリティ境界としてはサポートされていません](../../framework/misc/security-transparent-code.md)。 この機能は、Silverlight で頻繁に使用されます。

仮想化、コンテナー、ユーザー アカウントなど、オペレーティング システムが提供するセキュリティ境界を使用して、最低限の特権セットでプロセスを実行します。

## <a name="systementerpriseservices"></a>System.EnterpriseServices

System.EnterpriseServices (COM+) は、.NET Core でサポートされていません。

## <a name="see-also"></a>参照

- [.NET Framework から .NET Core への移植の概要](../porting/index.md)
