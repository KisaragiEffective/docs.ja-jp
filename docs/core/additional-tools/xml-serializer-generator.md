---
title: Microsoft XML Serializer Generator
description: Microsoft XML Serializer Generator の概要。 XML Serializer Generator を使用して、プロジェクトに格納される型のために XML シリアル化アセンブリを生成します。
author: mlacouture
ms.date: 01/19/2017
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 094dd1227033e167050ad73121b3005a592a0ae4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714521"
---
# <a name="using-microsoft-xml-serializer-generator-on-net-core"></a>.NET Core で Microsoft XML Serializer Generator を使用する

このチュートリアルでは、C# .NET Core アプリケーションで Microsoft XML Serializer Generator を使用する方法について説明します。 このチュートリアルを通して、以下のことを学びます。

> [!div class="checklist"]
>
> - .NET Core アプリの作成方法
> - Microsoft.XmlSerializer.Generator パッケージへの参照を追加する方法
> - MyApp.csproj を編集して依存関係を追加する方法
> - クラスと XmlSerializer を追加する方法
> - アプリケーションをビルドして実行する方法

.NET Framework の [Xml シリアライザー ジェネレーター (sgen.exe)](../../standard/serialization/xml-serializer-generator-tool-sgen-exe.md) と同様に、[Microsoft.XmlSerializer.Generator NuGet パッケージ](https://www.nuget.org/packages/Microsoft.XmlSerializer.Generator)は .NET Core および .NET Standard プロジェクト用の同等のものです。 アセンブリに含まれる型の XML シリアル化アセンブリを作成することで、<xref:System.Xml.Serialization.XmlSerializer> を使用してその型のオブジェクトをシリアル化または逆シリアル化するときの XML シリアル化の起動パフォーマンスを改善します。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、以下が必要です。

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 以降。
- 任意のコード エディター。

> [!TIP]
> コード エディターをインストールする必要がありますか。 [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) をお試しください。

## <a name="use-microsoft-xml-serializer-generator-in-a-net-core-console-application"></a>.NET Core コンソール アプリケーションで Microsoft XML Serializer Generator を使用する

次の手順では、.NET Core コンソール アプリケーションで XML Serializer Generator を使用する方法について説明します。

### <a name="create-a-net-core-console-application"></a>.NET Core コンソール アプリケーションを作成する

コマンド プロンプトを開き、*MyApp* というフォルダーを作成します。 作成したフォルダーに移動し、次のコマンドを入力します。

```dotnetcli
dotnet new console
```

### <a name="add-a-reference-to-the-microsoftxmlserializergenerator-package-in-the-myapp-project"></a>MyApp プロジェクトで Microsoft.XmlSerializer.Generator パッケージへの参照を追加する

[`dotnet add package`](../tools//dotnet-add-package.md) コマンドを使用して、プロジェクトで参照を追加します。

型:

```dotnetcli
dotnet add package Microsoft.XmlSerializer.Generator -v 1.0.0
```

### <a name="verify-changes-to-myappcsproj-after-adding-the-package"></a>パッケージを追加した後に MyApp.csproj の変更を確認する

コード エディターを開き、始めましょう。 引き続き、アプリをビルドした *MyApp* ディレクトリから作業します。

テキスト エディターで *MyApp.csproj* を開きます。

[`dotnet add package`](../tools//dotnet-add-package.md) コマンドを実行すると、以下の行が *MyApp.csproj* プロジェクト ファイルに追加されます。

 ```xml
 <ItemGroup>
    <PackageReference Include="Microsoft.XmlSerializer.Generator" Version="1.0.0" />
 </ItemGroup>
 ```

### <a name="add-another-itemgroup-section-for-net-core-cli-tool-support"></a>.NET Core CLI Tool のサポートのために別の ItemGroup セクションを追加する

検査した `ItemGroup` セクションの後に以下の行を追加します。

 ```xml
 <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.XmlSerializer.Generator" Version="1.0.0" />
 </ItemGroup>
 ```

### <a name="add-a-class-in-the-application"></a>アプリケーションにクラスを追加する

テキスト エディターで *Program.cs* を開きます。 *MyClass* というクラスを *Program.cs* に追加します。

```csharp
public class MyClass
{
   public int Value;
}
```

### <a name="create-an-xmlserializer-for-myclass"></a>MyClass 用の `XmlSerializer` を作成する

*Main* 内に次の行を追加して MyClass 用の `XmlSerializer` を作成します。

```csharp
var serializer = new System.Xml.Serialization.XmlSerializer(typeof(MyClass));
```

### <a name="build-and-run-the-application"></a>アプリケーションの構築と実行

*MyApp* フォルダー内のままで、[`dotnet run`](../tools/dotnet-run.md) を介してアプリケーションを実行すると、事前生成されたシリアライザーが実行時に自動的に読み込まれ、使用されます。

コンソール ウィンドウに次のコマンドを入力します。

```dotnetcli
dotnet run
```

> [!NOTE]
> [`dotnet run`](../tools/dotnet-run.md) は、[`dotnet build`](../tools/dotnet-build.md) を呼び出してビルド ターゲットがビルドされていることを確認した後、`dotnet <assembly.dll>` を呼び出してターゲット アプリケーションを実行します。

> [!IMPORTANT]
> このチュートリアルで紹介した、アプリケーションを実行するコマンドと手順は、開発時にのみ利用されます。 アプリの展開に進むときは、.NET Core アプリの別の[展開方法](../deploying/index.md)や [`dotnet publish`](../tools/dotnet-publish.md) コマンドを利用した方が効果的な場合もあります。

すべて正常に終了すると、*MyApp.XmlSerializers.dll* というアセンブリが生成されます。

おめでとうございます! 今回の成果:
> [!div class="checklist"]
>
> - .NET Core アプリを作成しました。
> - Microsoft.XmlSerializer.Generator パッケージへの参照を追加しました。
> - MyApp.csproj を編集して依存関係を追加しました。
> - クラスと XmlSerializer を追加しました。
> - アプリケーションをビルドして実行しました。

## <a name="related-resources"></a>関連リソース

- [XML シリアル化の概要](../../standard/serialization/introducing-xml-serialization.md)
- [XmlSerializer を使用してシリアル化する方法 (C#)](../../csharp/programming-guide/concepts/linq/how-to-serialize-using-xmlserializer.md)
- [方法: XmlSerializer を使用してシリアル化する (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/how-to-serialize-using-xmlserializer.md)
