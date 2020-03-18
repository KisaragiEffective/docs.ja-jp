---
title: プラグインがある .NET Core アプリケーションを作成する
description: プラグインをサポートする .NET Core アプリケーションを作成する方法について説明します。
author: jkoritzinsky
ms.author: jekoritz
ms.date: 10/16/2019
ms.openlocfilehash: eae792ddaa6655bfdcd932d3cb695f9dafa68130
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240845"
---
# <a name="create-a-net-core-application-with-plugins"></a>プラグインがある .NET Core アプリケーションを作成する

このチュートリアルでは、カスタムの <xref:System.Runtime.Loader.AssemblyLoadContext> を作成してプラグインを読み込む方法を説明します。 プラグインの依存関係を解決するために <xref:System.Runtime.Loader.AssemblyDependencyResolver> が使用されます。 このチュートリアルでは、ホスト アプリケーションからプラグインの依存関係を正しく分離します。 学習内容は次のとおりです。

- プロジェクトを構造化してプラグインをサポートする。
- カスタム <xref:System.Runtime.Loader.AssemblyLoadContext> を作成して各プラグインを読み込む。
- <xref:System.Runtime.Loader.AssemblyDependencyResolver?displayProperty=fullName> 型を使用して、プラグインが依存関係を持てるようにする。
- ビルド成果物をコピーするだけで簡単にデプロイできるプラグインを作成する。

## <a name="prerequisites"></a>前提条件

- [NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 以降のバージョンをインストールします。

## <a name="create-the-application"></a>アプリケーションを作成する

最初にアプリケーションを作成します。

1. 新しいフォルダーを作成し、そのフォルダーで次のコマンドを実行します。

    ```dotnetcli
    dotnet new console -o AppWithPlugin
    ```

2. プロジェクトのビルドをより容易にするため、同じフォルダー内に Visual Studio のソリューション ファイルを作成します。 次のコマンドを実行します。

    ```dotnetcli
    dotnet new sln
    ```

3. 次のコマンドを実行して、アプリ プロジェクトをソリューションに追加します。

    ```dotnetcli
    dotnet sln add AppWithPlugin/AppWithPlugin.csproj
    ```

これで、アプリケーションのスケルトンに入力できます。 *AppWithPlugin/Program.cs* ファイル内のコードを次のコードに置き換えます。

```csharp
using PluginBase;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Reflection;

namespace AppWithPlugin
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                if (args.Length == 1 && args[0] == "/d")
                {
                    Console.WriteLine("Waiting for any key...");
                    Console.ReadLine();
                }

                // Load commands from plugins.

                if (args.Length == 0)
                {
                    Console.WriteLine("Commands: ");
                    // Output the loaded commands.
                }
                else
                {
                    foreach (string commandName in args)
                    {
                        Console.WriteLine($"-- {commandName} --");

                        // Execute the command with the name passed as an argument.

                        Console.WriteLine();
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex);
            }
        }
    }
}

```

## <a name="create-the-plugin-interfaces"></a>プラグイン インターフェイスを作成する

プラグインがあるアプリをビルドする次の手順は、プラグインで実装する必要があるインターフェイスを定義することです。 アプリとプラグイン間の通信に使用する予定のすべての型を含むクラス ライブラリを作成することをお勧めします。 この分割により、完全なアプリケーションを出荷することなく、プラグイン インターフェイスをパッケージとして公開することができます。

プロジェクトのルート フォルダーで `dotnet new classlib -o PluginBase` を実行します。 また、`dotnet sln add PluginBase/PluginBase.csproj` を実行してプロジェクトをソリューション ファイルに追加します。 `PluginBase/Class1.cs` ファイルを削除して、次のインターフェイス定義を使用して、`PluginBase` フォルダー内に `ICommand.cs` という名前の新しいファイルを作成します。

[!code-csharp[the-plugin-interface](~/samples/snippets/core/tutorials/creating-app-with-plugin-support/csharp/PluginBase/ICommand.cs)]

この `ICommand` インターフェイスは、すべてのプラグインで実装されるインターフェイスです。

これで `ICommand` インターフェイスが定義されたので、アプリケーション プロジェクトをもう少し詳しく入力することができます。 ルート フォルダーから `AppWithPlugin` コマンドを使用して、参照を `PluginBase` プロジェクトから `dotnet add AppWithPlugin\AppWithPlugin.csproj reference PluginBase\PluginBase.csproj` に追加します。

`// Load commands from plugins` コメントを次のコード スニペットに置き換えて、指定したファイル パスからプラグインを読み込めるようにします。

```csharp
string[] pluginPaths = new string[]
{
    // Paths to plugins to load.
};

IEnumerable<ICommand> commands = pluginPaths.SelectMany(pluginPath =>
{
    Assembly pluginAssembly = LoadPlugin(pluginPath);
    return CreateCommands(pluginAssembly);
}).ToList();
```

次に、`// Output the loaded commands` コメントを次のコード スニペットに置き換えます。

```csharp
foreach (ICommand command in commands)
{
    Console.WriteLine($"{command.Name}\t - {command.Description}");
}
```

`// Execute the command with the name passed as an argument` コメントを、次のスニペットに置き換えます。

```csharp
ICommand command = commands.FirstOrDefault(c => c.Name == commandName);
if (command == null)
{
    Console.WriteLine("No such command is known.");
    return;
}

command.Execute();
```

最後に、次に示すように、静的メソッドを `Program` と `LoadPlugin` という名前の `CreateCommands` クラスに追加します。

```csharp
static Assembly LoadPlugin(string relativePath)
{
    throw new NotImplementedException();
}

static IEnumerable<ICommand> CreateCommands(Assembly assembly)
{
    int count = 0;

    foreach (Type type in assembly.GetTypes())
    {
        if (typeof(ICommand).IsAssignableFrom(type))
        {
            ICommand result = Activator.CreateInstance(type) as ICommand;
            if (result != null)
            {
                count++;
                yield return result;
            }
        }
    }

    if (count == 0)
    {
        string availableTypes = string.Join(",", assembly.GetTypes().Select(t => t.FullName));
        throw new ApplicationException(
            $"Can't find any type which implements ICommand in {assembly} from {assembly.Location}.\n" +
            $"Available types: {availableTypes}");
    }
}
```

## <a name="load-plugins"></a>プラグインを読み込む

これでアプリケーションが正しく読み込まれ、読み込んだプラグイン アセンブリからコマンドをインスタンス化できるようになりましたが、まだプラグイン アセンブリを読み込むことはできません。 次のコンテンツを使用して、*AppWithPlugin* フォルダー内に *PluginLoadContext.cs* という名前のファイルを作成します。

[!code-csharp[loading-plugins](~/samples/snippets/core/tutorials/creating-app-with-plugin-support/csharp/AppWithPlugin/PluginLoadContext.cs)]

`PluginLoadContext` 型は <xref:System.Runtime.Loader.AssemblyLoadContext> から派生します。 `AssemblyLoadContext` 型は、アセンブリのバージョンが競合しないように、開発者が読み込んだアセンブリを別のグループに隔離できるようにするランタイムの特殊な型です。 さらに、カスタム `AssemblyLoadContext` は、アセンブリを読み込むためにさまざまなパスから選択して、既定の動作をオーバーライドすることができます。 `PluginLoadContext` は、.NET Core 3.0 で導入された `AssemblyDependencyResolver` 型のインスタンスを使用して、アセンブリ名をパスに解決します。 `AssemblyDependencyResolver` オブジェクトは、.NET クラス ライブラリへのパスを使用して構築されます。 これは、パスが *コンストラクターに渡されたクラス ライブラリの*.deps.json`AssemblyDependencyResolver` ファイルに基づいて、アセンブリとネイティブ ライブラリをその相対パスに解決します。 カスタム `AssemblyLoadContext` は、プラグインが独自の依存関係を持てるようにし、`AssemblyDependencyResolver` は依存関係を正しく読み込むことを容易にします。

これで、`AppWithPlugin` プロジェクトに `PluginLoadContext` 型が追加されたので、次の本文を使用して `Program.LoadPlugin`メソッドを更新します。

```csharp
static Assembly LoadPlugin(string relativePath)
{
    // Navigate up to the solution root
    string root = Path.GetFullPath(Path.Combine(
        Path.GetDirectoryName(
            Path.GetDirectoryName(
                Path.GetDirectoryName(
                    Path.GetDirectoryName(
                        Path.GetDirectoryName(typeof(Program).Assembly.Location)))))));

    string pluginLocation = Path.GetFullPath(Path.Combine(root, relativePath.Replace('\\', Path.DirectorySeparatorChar)));
    Console.WriteLine($"Loading commands from: {pluginLocation}");
    PluginLoadContext loadContext = new PluginLoadContext(pluginLocation);
    return loadContext.LoadFromAssemblyName(new AssemblyName(Path.GetFileNameWithoutExtension(pluginLocation)));
}
```

プラグインごとに異なる `PluginLoadContext` インスタンスを使用することで、プラグインは異なる依存関係、または競合する依存関係であっても問題なく持つことができます。

## <a name="simple-plugin-with-no-dependencies"></a>依存関係のない単純なプラグイン

ルート フォルダーに戻り、次の手順を実行します。

1. 次のコマンドを実行して、`HelloPlugin` という名前の新しいクラス ライブラリ プロジェクトを作成します。

    ```dotnetcli
    dotnet new classlib -o HelloPlugin
    ```

2. 次のコマンドを実行して、そのプロジェクトを `AppWithPlugin` ソリューションに追加します。

    ```dotnetcli
    dotnet sln add HelloPlugin/HelloPlugin.csproj
    ```

3. *HelloPlugin/Class1.cs* ファイルを、次のコンテンツを持つ *HelloCommand.cs* という名前のファイルに置き換えます。

[!code-csharp[the-hello-plugin](~/samples/snippets/core/tutorials/creating-app-with-plugin-support/csharp/HelloPlugin/HelloCommand.cs)]

次に、*HelloPlugin.csproj* ファイルを開きます。 次のようになっているはずです。

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

</Project>

```

`<Project>` タグの間に、次の要素を追加します。

```xml
<ItemGroup>
    <ProjectReference Include="..\PluginBase\PluginBase.csproj">
        <Private>false</Private>
        <ExcludeAssets>runtime</ExcludeAssets>
    </ProjectReference>
</ItemGroup>
```

`<Private>false</Private>` 要素は重要です。 これは MSBuild に *PluginBase.dll* を HelloPlugin の出力ディレクトリにコピーしないように指示します。 *PluginBase.dll* アセンブリが出力ディレクトリに存在すると、`PluginLoadContext` はそこでアセンブリを検索し、*HelloPlugin.dll* アセンブリを読み込むときにそのアセンブリを読み込みます。 この時点で、`HelloPlugin.HelloCommand` 型は、既定の読み込みコンテキストに読み込まれる `ICommand` インターフェイスではなく、*プロジェクトの出力ディレクトリ内の*PluginBase.dll`HelloPlugin` から `ICommand` インターフェイスを実装します。 ランタイムでは、これらの 2 つの型は別のアセンブリからの異なる型として認識されるため、`AppWithPlugin.Program.CreateCommands` メソッドではコマンドが見つかりません。 結果として、プラグイン インターフェイスを含むアセンブリへの参照に `<Private>false</Private>`メタデータが必要になります。

同様に、`<ExcludeAssets>runtime</ExcludeAssets>` が他のパッケージを参照している場合は、`PluginBase` 要素も重要になります。 この設定は `<Private>false</Private>` と同じ効果を持ちますが、`PluginBase` プロジェクトまたはその依存関係のいずれかに含まれている場合があるパッケージ参照に対して機能します。

これで `HelloPlugin` プロジェクトが完了したので、`AppWithPlugin` プロジェクトを更新して、`HelloPlugin` プラグインが配置されている場所を認識できるようにする必要があります。 `// Paths to plugins to load` コメントの後に、`@"HelloPlugin\bin\Debug\netcoreapp3.0\HelloPlugin.dll"` 配列の要素として `pluginPaths` を追加します。

## <a name="plugin-with-library-dependencies"></a>ライブラリ依存関係を持つプラグイン

ほぼすべてのプラグインは単純な "Hello World" よりも複雑で、多くのプラグインには他のライブラリへの依存関係があります。 サンプルの `JsonPlugin` および `OldJson` のプラグイン プロジェクトでは、`Newtonsoft.Json` への NuGet パッケージの依存関係を持つプラグインの 2 つの例が示されています。 プロジェクト ファイル自体には、プロジェクト参照のための特別な情報は含まれていません。また、(`pluginPaths` 配列へのプラグインのパスを追加した後) プラグインは AppWithPlugin アプリの同じ実行で実行する場合でも、完璧に実行されます。 ただし、これらのプロジェクトでは参照されるアセンブリが出力ディレクトリにコピーされないため、プラグインが機能するためには、アセンブリがユーザーのコンピューター上に存在する必要があります。 この問題を回避する方法は 2 つあります。 1 つ目の方法は、`dotnet publish` コマンドを使用してクラス ライブラリを発行することです。 または、プラグインに `dotnet build` の出力を使用できるようにしたい場合は、プラグインのプロジェクト ファイルで `<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>` タグの間に `<PropertyGroup>`プロパティを追加することができます。 例については、`XcopyablePlugin` プラグイン プロジェクトを参照してください。

## <a name="other-examples-in-the-sample"></a>サンプル内のその他の例

このチュートリアルの完全なソース コードは [dotnet/samples リポジトリ](https://github.com/dotnet/samples/tree/master/core/extensions/AppWithPlugin)で確認できます。 完全なサンプルには、他のいくつかの `AssemblyDependencyResolver` の動作例が含まれています。 たとえば、`AssemblyDependencyResolver` オブジェクトも、NuGet パッケージに含まれているローカライズされたサテライト アセンブリと同じようにネイティブ ライブラリを解決できます。 サンプル リポジトリの `UVPlugin` と `FrenchPlugin` で、これらのシナリオが示されています。

## <a name="reference-a-plugin-interface-from-a-nuget-package"></a>NuGet パッケージからプラグイン インターフェイスを参照する

`A.PluginBase` という名前の NuGet パッケージで定義されているプラグイン インターフェイスを持つアプリ A があるとします。 プラグイン プロジェクト内のパッケージを正しく参照するにはどうすればよいでしょうか。 プロジェクト参照の場合、dll が出力にコピーされるのを防止する `<Private>false</Private>` メタデータをプロジェクト ファイル内の `ProjectReference` 要素で使用します。

`A.PluginBase` パッケージを正しく参照するために、プロジェクト ファイル内の `<PackageReference>` 要素を次に変更できます。

```xml
<PackageReference Include="A.PluginBase" Version="1.0.0">
    <ExcludeAssets>runtime</ExcludeAssets>
</PackageReference>
```

これにより、`A.PluginBase` アセンブリがプラグインの出力ディレクトリにコピーされるのを防ぎ、プラグインで `A.PluginBase` の A のバージョンが確実に使用されるようになります。

## <a name="plugin-target-framework-recommendations"></a>プラグインのターゲット フレームワークの推奨事項

プラグインの依存関係の読み込みでは *.deps.json* ファイルが使用されるため、プラグインのターゲット フレームワークに関連する課題があります。 具体的には、プラグインでは .NET Standard のバージョンではなく、.NET Core 3.0 などのランタイムをターゲットとする必要があります。 プロジェクトがターゲットとするフレームワークに基づいて *.deps.json* ファイルが生成されます。また、多くの .NET Standard と互換性のあるパッケージでは .NET Standard に対してビルドするための参照アセンブリと、特定のランタイムのための実装アセンブリが同梱されているため、 *.deps.json* が実装アセンブリを正しく認識できない場合や、想定していた .NET Core バージョンではなく、アセンブリの .NET Standard バージョンが取得される場合があります。

## <a name="plugin-framework-references"></a>プラグイン フレームワークの参照

現時点では、プラグインで新しいフレームワークをプロセスに導入することはできません。 たとえば、ルート `Microsoft.AspNetCore.App` フレームワークのみを使用するアプリケーションに、`Microsoft.NETCore.App` フレームワークを使用するプラグインを読み込むことはできません。 ホスト アプリケーションは、プラグインで必要なすべてのフレームワークへの参照を宣言する必要があります。
