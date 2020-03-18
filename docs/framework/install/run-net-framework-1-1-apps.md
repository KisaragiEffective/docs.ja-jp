---
title: Windows 8、Windows 8.1、または Windows 10 での .NET Framework 1.1 アプリの実行
ms.date: 05/26/2017
helpviewer_keywords:
- Windows 8, running .NET Framework 1.1 apps
- .NET Framework 1.1, running on Windows 8
ms.assetid: fb14e195-fea5-4561-b9a8-60a67283edb9
ms.openlocfilehash: 507de3ca635986d1f4e0e3ba7c607a4af774cdcf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75716274"
---
# <a name="run-net-framework-11-apps-on-windows-8-windows-81-or-windows-10"></a>Windows 8、Windows 8.1、または Windows 10 での .NET Framework 1.1 アプリの実行

.NET Framework 1.1 は、Windows 8、Windows 8.1、Windows Server 2012、Windows Server 2012 R2、または Windows 10 の各オペレーティング システムではサポートされていません。 場合によっては、アプリケーションの実行に .NET Framework 1.1 が特別に指定されています。 このような場合は、.NET Framework 3.5 SP1 以降のバージョンで実行するためにアプリケーションをアップグレードするように、独立ソフトウェア ベンダー (ISV) に連絡する必要があります。 詳細については、「[.NET Framework 1.1 からの移行](../migration-guide/migrating-from-the-net-framework-1-1.md)」を参照してください。

## <a name="install-the-net-framework-11-from-a-cd-or-download-center"></a>CD またはダウンロード センターからの .NET Framework 1.1 のインストール

Windows 8、Windows 8.1、Windows Server 2012、Windows Server 2012 R2、または Windows 10 に .NET Framework 1.1 を手動でインストールすることはできません。 サポート対象から除外されました。 パッケージをインストールしようとすると、「このバージョンの .NET Framework は以前にインストールしたバージョンと互換性がないため、セットアップを続行できません。」というエラー メッセージが表示されます。 この問題を解決するには、[.NET Framework 3.5 SP1](https://www.microsoft.com/download/details.aspx?id=22) をインストールします。 このバージョンには .NET Framework 2.0 (.NET Framework 1.1 の次のリリース) が含まれています。これは、Windows 8、Windows 8.1、および Windows 10 でサポートされています。 .NET Framework の新しいバージョンに自動的に更新されるかどうかを確認するために、常に最初にアプリケーションのインストールを試す必要があります。 更新されない場合は、アプリケーションの更新について ISV に連絡してください。

## <a name="see-also"></a>参照

- [.NET Framework 1.1 からの移行](../migration-guide/migrating-from-the-net-framework-1-1.md)
- [Windows 10、Windows 8.1、および Windows 8 への .NET Framework 3.5 のインストール](dotnet-35-windows-10.md)
