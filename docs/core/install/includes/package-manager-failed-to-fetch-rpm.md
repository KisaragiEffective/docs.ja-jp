---
ms.openlocfilehash: 96f3ef0160144322f77413995c842b745bb5bb1e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920737"
---

.NET Core パッケージのインストール中に、`signature verification failed for file 'repomd.xml' from repository 'packages-microsoft-com-prod'` のようなエラーが表示されることがあります。 一般に、このエラーは、.NET Core のパッケージ フィードが新しいバージョンのパッケージでアップグレード中であり、後でもう一度試す必要があることを意味しています。 アップグレード中は、2 時間以上パッケージ フィードを利用できません。 2 時間以上このエラーが継続的に発生する場合は、<https://github.com/dotnet/core/issues> でイシューを報告してください。
