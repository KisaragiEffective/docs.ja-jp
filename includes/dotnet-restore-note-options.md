---
ms.openlocfilehash: 47811d3fab2e4fa531d383dfe818e3cac5613eb3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72179975"
---
> [!NOTE]
> .NET Core 2.0 以降、[`dotnet restore`](~/docs/core/tools/dotnet-restore.md) を実行する必要がなくなりました。`dotnet build` や `dotnet run` のような、復元を必要とするあらゆるコマンドによって暗黙的に実行されるためです。 [Azure DevOps Services の継続的インテグレーション ビルド](/azure/devops/build-release/apps/aspnet/build-aspnet-core)など、明示的な復元が合理的となる一部のシナリオや、復元の時刻を明示的に制御する必要があるビルド システムでは、引き続き有効なコマンドとなります。
>
> このコマンドには `dotnet restore` オプションを指定できますが、`--source` のように長い形式で指定する必要があります。 `-s` のような短い形式のオプションはサポートされていません。
