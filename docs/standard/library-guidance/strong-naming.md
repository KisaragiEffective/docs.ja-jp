---
title: 厳密な名前付けと .NET ライブラリ
description: .NET ライブラリに対する厳密な名前付けに関するベスト プラクティスの推奨事項。
ms.date: 10/16/2018
ms.openlocfilehash: db268093b07a2ece7cdb8329fd789b52da9c5c32
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76744536"
---
# <a name="strong-naming"></a>厳密な名前付け

厳密な名前付けとは、キーを使用してアセンブリに署名し、[厳格な名前付きのアセンブリ](../assembly/strong-named.md)を生成することです。 アセンブリに厳密な名前が付けられている場合、名前とアセンブリのバージョン番号に基づいて一意の ID が作成され、アセンブリの競合を防ぐのに役立ちます。

厳密な名前付けの欠点は、Windows 上の .NET Framework で、アセンブリに厳密な名前が付けられると、アセンブリの厳密な読み込みが有効になることです。 厳密な名前付きのアセンブリの参照は、アセンブリによって参照されるバージョンと正確に一致する必要があります。その場合、開発者には、アセンブリを使用する際に[バインド リダイレクトの構成](../../framework/configure-apps/redirect-assembly-versions.md)が強制されます。

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
         <dependentAssembly>
            <assemblyIdentity name="myAssembly" publicKeyToken="32ab4ba45e0a69a1" culture="neutral" />
            <bindingRedirect oldVersion="1.0.0.0" newVersion="2.0.0.0"/>
         </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

.NET 開発者が厳密な名前付けについて不満を持つ場合、通常、その内容は厳密なアセンブリの読み込みに関するものです。 さいわい、この問題は .NET Framework に特定されます。 .NET Core、Xamarin、UWP、およびその他のほとんどの .NET 実装では厳密なアセンブリの読み込みは行われず、厳密な名前付けの主な欠点はなくなります。

厳密な名前付けの重要な側面の 1 つはウイルスです。厳密な名前付きのアセンブリで参照できるのは、他の厳密な名前付きのアセンブリのみです。 ご利用のライブラリに厳密な名前が付けられていない場合、厳密な名前付けが必要なアプリケーションやライブラリを構築している開発者はその使用から除外されています。

厳密な名前付けの利点は次のとおりです。

1. アセンブリを他の厳密な名前付きのアセンブリで参照して使用することができます。
2. アセンブリをグローバル アセンブリ キャッシュ (GAC) に格納することができます。
3. アセンブリを、他のバージョンのアセンブリと並行して読み込むことができます。 通常、アセンブリの並行読み込みは、プラグイン アーキテクチャを持つアプリケーションで必要となります。

## <a name="create-strong-named-net-libraries"></a>厳密な名前付き .NET ライブラリを作成する

オープンソースの .NET ライブラリには厳密な名前を付ける必要があります。 アセンブリに厳密な名前を付けることで、ほとんどの人が確実にそれを使用できるようになり、厳密なアセンブリの読み込みは .NET Framework にのみ影響します。

> [!NOTE]
> このガイダンスは、NuGet.org で公開されている .NET ライブラリなど、公的に配布されている .NET ライブラリに固有のものです。厳密な名前付けはほとんどの .NET アプリケーションで必要はなく、既定では行われません。

✔️ 検討すること: ご利用のライブラリのアセンブリに厳密な名前を付ける。

✔️ 検討すること: ご利用のソース管理システムに厳密な名前付けキーを追加する。

> 一般公開されているキーを使用することで、開発者はライブラリ ソース コードを同じキーで変更および再コンパイルすることができます。
>
> [部分信頼シナリオ](../../framework/misc/using-libraries-from-partially-trusted-code.md)において特別なアクセス許可を付与するために、厳密な名前付けキーが過去に使用されていた場合は、そのキーを一般公開しないでください。 そうしないと、既存の環境が損なわれる可能性があります。

> [!IMPORTANT]
> コードの発行元の ID が必要な場合は、[Authenticode](/windows-hardware/drivers/install/authenticode) と [NuGet パッケージ署名](/nuget/create-packages/sign-a-package)をお勧めします。 セキュリティ対策としてコード アクセス セキュリティ (CAS) を使用しないでください。

✔️ 検討すること: ユーザーによるバインド リダイレクトとその更新頻度を減らせるように、アセンブリのバージョンをメジャー バージョン変更時にのみインクリメントする。

> 詳細については、[バージョン管理とアセンブリのバージョン](./versioning.md#assembly-version)に関する記述を参照してください。

❌ 厳密な名前付けキーを追加、削除、または変更しないでください。

> アセンブリの厳密な名前付けキーを変更すると、アセンブリの ID が変更され、それを使用するコンパイル済みコードが中断されます。 詳細については、[バイナリの破壊的変更](./breaking-changes.md#binary-breaking-change)に関する記述を参照してください。

ご利用のライブラリで、厳密な名前が指定されたバージョンおよび厳密でない名前が指定されたバージョンを ❌ 発行しないでください。 たとえば、`Contoso.Api` と`Contoso.Api.StrongNamed` です。

> 2 つのパッケージを発行すると、開発者のエコシステムが分岐されます。 また、両方のパッケージに従ってアプリケーションが停止した場合、型名の競合が発生する可能性があります。 .NET の場合、さまざまなアセンブリにはさまざまな型があります。

>[!div class="step-by-step"]
>[前へ](cross-platform-targeting.md)
>[次へ](nuget.md)
