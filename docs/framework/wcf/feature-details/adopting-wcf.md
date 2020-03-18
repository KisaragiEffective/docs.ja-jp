---
title: Windows Communication Foundation の採用
ms.date: 03/30/2017
ms.assetid: 49ba71e2-9468-4082-84c5-cf8daf95e34a
ms.openlocfilehash: e4955d3a1d1c3a7c2afae5b0e1573d383e03bb33
ms.sourcegitcommit: ed3f926b6cdd372037bbcc214dc8f08a70366390
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2020
ms.locfileid: "76116665"
---
# <a name="adopt-windows-communication-foundation"></a>Windows Communication Foundation を採用する

ASP.NET を使用して開発した既存のアプリケーションを引き続き維持しながら、新しい開発に Windows Communication Foundation (WCF) を使用することもできます。 WCF は、任意のシナリオで .NET Framework で構築されたアプリケーションとの通信を容易にするために最適な選択肢となることを目的としています。これは、さまざまなソフトウェア通信の問題を ASP.NET に解決するための標準ツールとして機能します。できません.

新しい WCF アプリケーションは、既存の ASP.NET Web サービスと同じコンピューターにデプロイできます。 既存の ASP.NET Web サービスがバージョン2.0 より前のバージョンの .NET Framework を使用する場合は、ASP.NET IIS 登録ツールを使用して、新しい WCF アプリケーションがホストされる IIS アプリケーションに .NET Framework 2.0 を選択的に展開できます。 このツールは、 [ASP.NET Iis Registration tool (Aspnet_regiis)](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90))に記載されており、iis 6.0 管理コンソールにはユーザーインターフェイスが組み込まれています。

ASP.NET 互換モードで実行するように構成されている WCF サービスを IIS の既存の ASP.NET Web サービスアプリケーションに追加することにより、WCF を使用して、既存の ASP.NET Web サービスに新しい機能を追加できます。 ASP.NET 互換モードのため、新しい WCF サービスのコードは、<xref:System.Web.HttpContext> クラスを使用して、既存の ASP.NET コードと同じアプリケーション状態情報にアクセスし、更新することができます。 また、アプリケーションは同じクラス ライブラリを共有することもできます。

WCF クライアントでは、ASP.NET Web サービスを使用できます。 <xref:System.ServiceModel.BasicHttpBinding> で構成された WCF サービスは、ASP.NET Web サービスクライアントで使用できます。 ASP.NET Web サービスは、WCF アプリケーションと共存できます。また、WCF を使用して、既存の ASP.NET Web サービスに機能を追加することもできます。 これらの方法で WCF と ASP.NET Web サービスを一緒に使用できるようになった場合は、ASP.NET Web サービスではなく、WCF によって提供される機能が必要な場合にのみ、ASP.NET Web サービスを WCF に移行することをお勧めします。

場合によっては、あるテクノロジから別のテクノロジにコードを移行する方法はほとんどありません。 新しいテクノロジを採用する理由は、以前のテクノロジでは満たすことができない新しい要件を満たすことであり、その場合は、新しく展開された一連の要件を満たす新しいソリューションを設計することです。 新しい設計では、既存のシステムでの経験とそのシステムを設計して以来、獲得した知識を活用できます。 また、新しい設計は、新しいプラットフォーム上で古い設計を再制作するのではなく、新しいテクノロジの機能を十分に活用することができます。 新しいデザインの重要な要素をプロトタイプ作成した後は、新しい設計の既存のシステムからコードを再利用しやすくなります。

## <a name="see-also"></a>関連項目

- [方法 : メタデータの取得および準拠サービスの実装をする](../../../../docs/framework/wcf/feature-details/how-to-retrieve-metadata-and-implement-a-compliant-service.md)
