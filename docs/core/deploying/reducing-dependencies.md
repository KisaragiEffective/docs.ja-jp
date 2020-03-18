---
title: project.json によるパッケージ依存関係の縮小
description: project.json ベースのライブラリ作成時にパッケージの依存関係を減らします。
author: cartermp
ms.date: 06/20/2016
ms.openlocfilehash: 48ba3ef578388fd98fe7cb830df313512d359483
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75740827"
---
# <a name="reducing-package-dependencies-with-projectjson"></a>project.json によるパッケージ依存関係の縮小

この記事では、`project.json` ライブラリの作成時にパッケージ依存関係を減らすために知っておくべきことについて紹介します。 この記事を最後までお読みいただくと、必要な依存関係だけが使用されるようにライブラリを構築する方法を理解できます。

## <a name="why-its-important"></a>これが重要な理由

.NET Core は NuGet パッケージで構成される製品です。  必要不可欠なパッケージが [.NETStandard.Library メタパッケージ](https://www.nuget.org/packages/NETStandard.Library)です。これは他のパッケージから構成される NuGet パッケージです。 一連のパッケージが提供されますが、それらは .NET Framework、.NET Core、Xamarin/Mono など、複数の .NET 実装で動作することが確認されています。

しかしながら、お使いのライブラリではすべてのパッケージが使用されるとは限りません。  ライブラリを作成し、NuGet 経由で配信するときは、実際に使用するパッケージにのみ依存関係を "減らす" ことが推奨されます。  結果的に、NuGet パッケージの全体的フットプリントが少なくなります。

## <a name="how-to-do-it"></a>方法

現在のところ、パッケージ参照を減らす正式な `dotnet` コマンドはありません。  代わりに、手動で行う必要があります。  一般的なプロセスは次のようになります。

1. お使いの `NETStandard.Library` の `1.6.0` セクションにある `dependencies` バージョン `project.json` を参照します。
2. コマンド ラインから `dotnet restore` でパッケージを復元します ([注記参照](#dotnet-restore-note))。
3. `project.lock.json` ファイルを調べ、`NETStandard.Library` というセクションを探します。  ファイルの始まりの近くにあります。
4. `dependencies` の下にあるパッケージをすべてコピーします。
5. `.NETStandard.Library` 参照を削除し、コピーしたパッケージで置き換えます。
6. 不要なパッケージ参照を削除します。

不要なパッケージは次の方法で確認できます。

1. 試用とエラー。 パッケージを削除したり、復元したり、ライブラリがまだコンパイルするか確認したり、このプロセスを繰り返したりなどの操作が含まれます。
2. [ILSpy](https://github.com/icsharpcode/ILSpy#ilspy-------) や [.NET Reflector](https://www.red-gate.com/products/dotnet-development/reflector) などのツールを利用し、コードで実際に利用されている参照を確認します。 その後、利用している種類に該当しないパッケージを削除できます。

## <a name="example"></a>例

追加機能を汎用コレクション タイプに提供するライブラリを記述したとします。 そのようなライブラリは `System.Collections` のようなパッケージに依存しなければなりませんが、`System.Net.Http` のようなパッケージにはまったく依存しないことがあります。 そのため、このライブラリが必要とするものだけにパッケージ依存関係を減らすと効果的です。

このライブラリから余計なものを減らすには、最初に `project.json` ファイルから始め、`NETStandard.Library` バージョン `1.6.0` の参照を追加します。

```json
{
    "version":"1.0.0",
    "dependencies":{
        "NETStandard.Library":"1.6.0"
    },
    "frameworks": {
        "netstandard1.0": {}
     }
}
```

次に、`dotnet restore` でパッケージを復元し ([注記参照](#dotnet-restore-note))、`project.lock.json` ファイルを調べ、`NETStandard.Library` に対して復元されたすべてのパッケージを探します。

`project.lock.json` をターゲットにするとき、`netstandard1.0` ファイルの関連セクションは次のようになります。

```json
"NETStandard.Library/1.6.0":{
    "type": "package",
    "dependencies": {
        "Microsoft.NETCore.Platforms": "1.0.1",
        "Microsoft.NETCore.Runtime": "1.0.2",
        "System.Collections": "4.0.11",
        "System.Diagnostics.Debug": "4.0.11",
        "System.Diagnostics.Tools": "4.0.1",
        "System.Globalization": "4.0.11",
        "System.IO": "4.1.0",
        "System.Linq": "4.1.0",
        "System.Net.Primitives": "4.0.11",
        "System.ObjectModel": "4.0.12",
        "System.Reflection": "4.1.0",
        "System.Reflection.Extensions": "4.0.1",
        "System.Reflection.Primitives": "4.0.1",
        "System.Resources.ResourceManager": "4.0.1",
        "System.Runtime": "4.1.0",
        "System.Runtime.Extensions": "4.1.0",
        "System.Text.Encoding": "4.0.11",
        "System.Text.Encoding.Extensions": "4.0.11",
        "System.Text.RegularExpressions": "4.1.0",
        "System.Threading": "4.0.11",
        "System.Threading.Tasks": "4.0.11",
        "System.Xml.ReaderWriter": "4.0.11",
        "System.Xml.XDocument": "4.0.11"
    }
}
```

次に、ライブラリの `dependencies` ファイルの `project.json` セクションにパッケージ参照をコピーします。`NETStandard.Library` 参照を置き換えます。

```json
{
    "version":"1.0.0",
    "dependencies":{
        "Microsoft.NETCore.Platforms": "1.0.1",
        "Microsoft.NETCore.Runtime": "1.0.2",
        "System.Collections": "4.0.11",
        "System.Diagnostics.Debug": "4.0.11",
        "System.Diagnostics.Tools": "4.0.1",
        "System.Globalization": "4.0.11",
        "System.IO": "4.1.0",
        "System.Linq": "4.1.0",
        "System.Net.Primitives": "4.0.11",
        "System.ObjectModel": "4.0.12",
        "System.Reflection": "4.1.0",
        "System.Reflection.Extensions": "4.0.1",
        "System.Reflection.Primitives": "4.0.1",
        "System.Resources.ResourceManager": "4.0.1",
        "System.Runtime": "4.1.0",
        "System.Runtime.Extensions": "4.1.0",
        "System.Text.Encoding": "4.0.11",
        "System.Text.Encoding.Extensions": "4.0.11",
        "System.Text.RegularExpressions": "4.1.0",
        "System.Threading": "4.0.11",
        "System.Threading.Tasks": "4.0.11",
        "System.Xml.ReaderWriter": "4.0.11",
        "System.Xml.XDocument": "4.0.11"
    },
    "frameworks":{
        "netstandard1.0": {}
    }
}
```

パッケージがたくさんあります。コレクション タイプを拡張するとき、この多くが実際には必要ありません。  手動でパッケージを削除するか、[ILSpy](https://github.com/icsharpcode/ILSpy#ilspy-------) や [.NET Reflector](https://www.red-gate.com/products/dotnet-development/reflector/) のようなツールを利用し、コードで実際に使用されるパッケージを特定できます。

余計なものを減らしたパッケージは次のようになります。

```json
{
    "dependencies":{
        "Microsoft.NETCore.Platforms": "1.0.1",
        "Microsoft.NETCore.Runtime": "1.0.2",
        "System.Collections": "4.0.11",
        "System.Linq": "4.1.0",
        "System.Runtime": "4.1.0",
        "System.Runtime.Extensions": "4.1.0",
        "System.Runtime.Handles": "4.0.1",
        "System.Runtime.InteropServices": "4.1.0",
        "System.Runtime.InteropServices.RuntimeInformation": "4.0.0",
        "System.Threading.Tasks": "4.0.11"
    },
    "frameworks":{
        "netstandard1.0": {}
     }
}
```

これで、`NETStandard.Library` メタパッケージに依存する場合より、フットプリントが少なくなります。

<a name="dotnet-restore-note"></a>
[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
