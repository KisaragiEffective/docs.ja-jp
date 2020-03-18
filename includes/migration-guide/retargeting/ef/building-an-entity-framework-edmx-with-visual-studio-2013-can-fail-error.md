---
ms.openlocfilehash: ffafb0c9e3982dd168f907d32a8f59f96e6040d0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804603"
---
### <a name="building-an-entity-framework-edmx-with-visual-studio-2013-can-fail-with-error-msb4062-if-using-the-entitydeploysplit-or-entityclean-tasks"></a>Visual Studio 2013 で Entity Framework edmx をビルドすると、EntityDeploySplit または EntityClean タスクを使用している場合、エラー MSB4062 で失敗することがある

|   |   |
|---|---|
|説明|MSBuild 12.0 ツール (Visual Studio 2013 に含まれる) は、MSBuild ファイルの位置を変更したため、古い Entity Framework のターゲット ファイルは無効になります。 その結果、<code>EntityDeploySplit</code> および <code>EntityClean</code> タスクは、<code>Microsoft.Data.Entity.Build.Tasks.dll</code> を見つけられないために失敗します。 このエラーは、ツールセット (MSBuild/VS) の変更によるものであり、.NET Framework の変更によるものではないことに注意してください。 開発者ツールをアップグレードしたときにのみ発生し、.NET Framework をアップグレード下だけでは発生しません。|
|提案される解決策|Entity Framework のターゲット ファイルは、.NET Framework 4.6 以降の新しい MSBuild レイアウトで機能するように修正されます。 このバージョンの Framework にアップグレードすることで、この問題は修正されます。 または、[この回避策](https://stackoverflow.com/a/24249247/131944)を使用して、ターゲット ファイルに直接パッチを当てることができます。|
|スコープ|Major|
|バージョン|4.5.1|
|[種類]|再ターゲット中|
