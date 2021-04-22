---
title: .NET Core と .NET 5 以降で使用できない .NET Framework テクノロジ
titleSuffix: ''
description: .NET Core と .NET 5.0 以降のバージョンで使用できない .NET Framework テクノロジについて学習します。
author: cartermp
ms.date: 03/08/2021
ms.topic: reference
ms.openlocfilehash: d22352cee76c546531a6487387fc8e2f4105fac4
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740957"
---
# <a name="net-framework-technologies-unavailable-on-net-core-and-net-5"></a>.NET Core と .NET 5 以降で使用できない .NET Framework テクノロジ

.NET Framework ライブラリで使用できるテクノロジの中には、.NET 5+ (および .NET Core) で使用できないものがあります。たとえば、アプリ ドメイン、リモート処理、コード アクセス セキュリティ (CAS) などです。 ご利用のライブラリがこのページに一覧表示されているテクノロジの 1 つ以上に依存する場合は、記載されている代替方法を検討してください。

API の互換性の詳細については、[.NET の破壊的変更](../compatibility/breaking-changes.md)に関するページを参照してください。

## <a name="application-domains"></a>アプリケーション ドメイン

アプリケーション ドメイン (AppDomains) はアプリを互いに分離します。 AppDomain にはランタイム サポートが必要で、リソースを大量に消費します。 追加のアプリ ドメインの作成はサポートされておらず、今後この機能を追加する予定はありません。 コードの分離のためには、代替方法として別個のプロセスやコンテナーを使用します。 アセンブリを動的に読み込むには、<xref:System.Runtime.Loader.AssemblyLoadContext> クラスを使用します。

.NET Framework からのコードの移行をより簡単にするために、.NET 5 以降では <xref:System.AppDomain> API サーフェスの一部を公開しています。 API の中には、正常に機能するもの (<xref:System.AppDomain.UnhandledException?displayProperty=nameWithType> など)、処理を行わないメンバー (<xref:System.AppDomain.SetCachePath%2A> など)、<xref:System.PlatformNotSupportedException> をスローするもの (<xref:System.AppDomain.CreateDomain%2A> など) があります。 [dotnet/runtime GitHub リポジトリ](https://github.com/dotnet/runtime)で [`System.AppDomain` 参照ソース](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/AppDomain.cs)に対して使用する種類を確認してください。 必ず、実装されているバージョンに合ったブランチを選択してください。

## <a name="remoting"></a>リモート処理

.NET リモート処理は、.NET 5+ (および .NET Core) ではサポートされていません。 .NET リモート処理は、問題のあるアーキテクチャであると判断されました。 これは、現在サポートされていないアプリケーション ドメインとの間の通信に使用されています。 また、リモート処理にはランタイム サポートも必要で、維持するのに高いコストがかかります。

プロセスとの間の通信については、リモート処理に代わる方法として、<xref:System.IO.Pipes> クラスまたは <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> クラスなどのプロセス間通信 (IPC) メカニズムを検討してください。

マシン間では、代替方法としてネットワーク ベースのソリューションを使用してください。 可能であれば、HTTP などのオーバーヘッドの少ないプレーンテキストのプロトコルを使用してください。 この場合、ASP.NET Core で使用される Web サーバーの [Kestrel Web サーバー](/aspnet/core/fundamentals/servers/kestrel)も選択できます。 また、ネットワーク ベースのマシン間のシナリオとして、<xref:System.Net.Sockets> の使用も検討してください。 その他のオプションについては、[.NET オープン ソース開発者プロジェクト:メッセージング](https://github.com/Microsoft/dotnet/blob/master/dotnet-developer-projects.md#messaging)に関する記事をご覧ください。

## <a name="code-access-security-cas"></a>コード アクセス セキュリティ (CAS)

サンドボックスは、マネージド アプリケーションやライブラリによって使用または実行されるリソースの制限を、ランタイムまたはフレームワークに依存しています。これは [.NET Framework によりサポートされていない](../../framework/misc/code-access-security.md)ため、.NET Core と .NET 5 以降でもサポートされません。 .NET Framework やランタイムでは、特権の昇格が発生するケースが多すぎるため、このまま CAS をセキュリティ境界と見なすことはできません。 また、CAS は実装が複雑化しており、その使用を予定していないアプリケーションでは、多くの場合で正確性のパフォーマンスに影響します。

仮想化、コンテナー、ユーザー アカウントなど、オペレーティング システムが提供するセキュリティ境界を使用して、最小限の特権セットでプロセスを実行します。

## <a name="security-transparency"></a>セキュリティ透過性

CAS と同様に、セキュリティ透過性はサンドボックス コードをセキュリティ クリティカルなコードから宣言的に分離しますが、[現在はセキュリティ境界としてはサポートされていません](../../framework/misc/security-transparent-code.md)。 この機能は、Silverlight で頻繁に使用されます。

仮想化、コンテナー、ユーザー アカウントなど、オペレーティング システムが提供するセキュリティ境界を使用して、最低限の特権セットでプロセスを実行します。

## <a name="systementerpriseservices"></a>System.EnterpriseServices

<xref:System.EnterpriseServices?displayProperty=fullName> (COM+) は、.NET Core および .NET 5 以降ではサポートされていません。

## <a name="workflow-foundation-and-wcf"></a>Workflow Foundation と WCF

Windows Workflow Foundation (WF) と Windows Communication Foundation (WCF) は、.NET 5 以降 (.NET Core を含む) ではサポートされていません。 代替方法については、「[CoreWF](https://github.com/UiPath/corewf)」、および [CoreWCF](https://github.com/CoreWCF/CoreWCF) に関するページを参照してください。

## <a name="see-also"></a>関連項目

- [.NET Framework から .NET への移植の概要](index.md)
