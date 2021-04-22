---
description: '詳細情報: Windows Communication Foundation の採用'
title: Windows Communication Foundation の採用
ms.date: 03/30/2017
ms.assetid: 49ba71e2-9468-4082-84c5-cf8daf95e34a
ms.openlocfilehash: c8d5808b246e40b504d2d07dcdd6dc3fd622361d
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643918"
---
# <a name="adopt-windows-communication-foundation"></a>Windows Communication Foundation の採用

ASP.NET を使用して開発した既存のアプリケーションのメンテナンスを続行しながら、新規開発については Windows Communication Foundation (WCF) を選択できます。 WCF は、どのようなシナリオにおいても .NET Framework を使用してビルドされたアプリケーションとの通信を容易に行うための最適な選択となるように設計されているため、これまで ASP.NET では実現できなかったような方法でソフトウェア通信における多様な問題を解決するための標準ツールとして使用できます。

新しい WCF アプリケーションは、既存の ASP.NET Web サービスがある同じコンピューター上に展開できます。 既存の ASP.NET Web サービスが、バージョン 2.0 より前の .NET Framework を使用している場合は、ASP.NET IIS 登録ツールを使用して、新しい WCF アプリケーションをホストできる IIS アプリケーションに .NET Framework 2.0 を選択的に展開できます。 このツールについてのドキュメントは [ASP.NET IIS Registration Tool (Aspnet_regiis.exe)](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)) にあり、ユーザー インターフェイスは IIS 6.0 管理コンソールに組み込まれています。

WCF を使用して、ASP.NET 互換モードで実行するように構成された WCF サービスを IIS の既存の ASP.NET Web サービス アプリケーションに追加することで、既存の ASP.NET Web サービスに新しい機能を追加できます。 ASP.NET 互換モードにより、新しい WCF サービスのコードは、<xref:System.Web.HttpContext> クラスを使用することで、既存の ASP.NET コードと同じアプリケーション状態情報にアクセスして更新を行うことができます。 また、アプリケーションは同じクラス ライブラリを共有することもできます。

WCF クライアントでは、ASP.NET Web サービスを使用できます。 ASP.NET Web サービス クライアントは、<xref:System.ServiceModel.BasicHttpBinding> で構成された WCF サービスを使用できます。 ASP.NET Web サービスは WCF アプリケーションとの共存が可能であり、WCF を使用して既存の ASP.NET Web サービスに機能を追加することもできます。 このように WCF と ASP.NET Web サービスは同時に使用できるため、ASP.NET Web サービスから WCF への移行が必要になるのは、ASP.NET Web サービスでは提供されず、WCF でのみ提供される機能が必要な場合に限られるかもしれません。

このような移行が必要となるまれな場合においても、あるテクノロジから別のテクノロジへコードを移行するのは、ほとんどの場合、正しいアプローチではありません。 以前のテクノロジでは満たされない新しい要件を満たすために新しいテクノロジを採用する場合、新たに拡張された要件を満たす新しいソリューションを設計することが正しい方法です。 新しい設計では、既存のシステムでの経験とそのシステムを設計して以来、獲得した知識を活用できます。 また、新しい設計は、新しいプラットフォーム上で古い設計を再制作するのではなく、新しいテクノロジの機能を十分に活用することができます。 新しい設計の主要な要素のプロトタイプが完成すると、既存のシステムのコードを新しいシステム内で再使用することが容易になります。

## <a name="see-also"></a>関連項目

- [方法: メタデータの取得および準拠サービスの実装をする](how-to-retrieve-metadata-and-implement-a-compliant-service.md)
