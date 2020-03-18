---
title: .NET Framework および .NET Core のプロジェクトを整理する
description: プロジェクト所有者が横並びの .NET Framework と .NET Core に対してソリューションをコンパイルするときに役立ちます。
author: conniey
ms.date: 12/07/2018
ms.openlocfilehash: d71cc3102846c08f4e35831921b8cc4ca82f9e1b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75777330"
---
# <a name="organize-your-project-to-support-both-net-framework-and-net-core"></a>.NET Framework と .NET Core の両方をサポートするようにプロジェクトを整理する

.NET Framework と .NET Core 両方のコンパイルに同時に対応するソリューションを作成できます。 この記事では、この目標を達成するために役立つ、いくつかのプロジェクトの整理のオプションについて説明します。 ここでは、.NET Core でプロジェクト レイアウトを設定する方法について決定するときに考慮する典型的なシナリオを紹介します。 この一覧はプロジェクトのニーズに基づいて選択されており、すべてを網羅しるわけではりません。

- [**既存のプロジェクトと .NET Core プロジェクトを結合し、シングル プロジェクトを作成する**](#replace-existing-projects-with-a-multi-targeted-net-core-project)

  *効果:*
  - 複数のプロジェクトではなくシングル プロジェクトをコンパイルすることで、ビルド プロセスをシンプルにします。それぞれ、異なる .NET Framework バージョンまたはプラットフォームをターゲットにします。
  - ターゲットが複数のプロジェクトのソース ファイル管理をシンプルにします。シングル プロジェクト ファイルを管理することになります。 ソース ファイルの追加または削除時に、代替方法では、これらを他のプロジェクトと手動で同期する必要があります。
  - 使用する NuGet パッケージを簡単に生成します。
  - コンパイラ ディレクティブを利用することで、ライブラリの特定の .NET Framework バージョンに対してコードを記述できます。

  *サポートされていないシナリオ:*
  - 既存のプロジェクトを開くには、開発者が Visual Studio 2017 以降を使用している必要があります。 旧バージョンの Visual Studio をサポートするには、[プロジェクト ファイルを異なるフォルダーで保持する](#support-vs)ことが推奨されます。

- <a name="support-vs"></a>[**既存のプロジェクトと新しい .NET Core プロジェクトを別々に保存する**](#keep-existing-projects-and-create-a-net-core-project)

  *効果:*
  - Visual Studio 2017 以降を持っていない場合がある開発者と共同作成者に対して、既存のプロジェクトの開発をサポートします。
  - 既存のプロジェクトで新しいバグが発生する可能性が減ります。既存のプロジェクトではコード チャーンが要求されないためです。

## <a name="example"></a>例

次のリポジトリを考慮してください。

![既存のプロジェクト](./media/project-structure/existing-project-structure.png)

[**ソース コード**](https://github.com/dotnet/samples/tree/master/framework/libraries/migrate-library/)

下に説明する既存のプロジェクトの制約や複雑性にもよりますが、このリポジトリのために .NET Core のサポートを追加する方法がいくつかあります。

## <a name="replace-existing-projects-with-a-multi-targeted-net-core-project"></a>既存のプロジェクトとターゲットが複数ある .NET Core Project を置換します

リポジトリを再整理できます。既存の *\*.csproj* ファイルが削除され、複数のフレームワークをターゲットにするシングル *\*.csproj* ファイルが作成されます。 異なるフレームワークに対してシングル プロジェクトでコンパイルできるので、この方法が推奨されます。 さまざまなコンパイル オプション、ターゲットにするフレームワークごとの依存関係などを処理することもできます。

![複数のフレームワークをターゲットにする csproj の作成](./media/project-structure/multi-targeted-project.png)

[**ソース コード**](https://github.com/dotnet/samples/tree/master/framework/libraries/migrate-library-csproj/)

注目するべき変更点:

- *packages.config* と *\*.csproj* が新しい [.NET Core *\*.csproj*](https://github.com/dotnet/samples/tree/master/framework/libraries/migrate-library-csproj/src/Car/Car.csproj) に置き換わりました。 NuGet パッケージが `<PackageReference> ItemGroup` で指定されています。

## <a name="keep-existing-projects-and-create-a-net-core-project"></a>既存のプロジェクトを保持し、.NET Core プロジェクトを作成する

古いフレームワークをターゲットにする既存のプロジェクトがあるとき、そのようなプロジェクトをそのまま残し、.NET Core プロジェクトを利用して今後のフレームワークをターゲットにすると効率的な場合があります。

![別のフォルダーに既存のプロジェクトがある .NET Core プロジェクト](./media/project-structure/separate-projects-same-source.png)

[**ソース コード**](https://github.com/dotnet/samples/tree/master/framework/libraries/migrate-library-csproj-keep-existing/)

.NET Core と既存のプロジェクトを別々のフォルダーに保存します。 プロジェクトを別々のフォルダーに保存すれば、Visual Studio 2017 以降のバージョンを所有する必要がありません。 古いプロジェクトだけを開く別個のソリューションを作成できます。

## <a name="see-also"></a>参照

- [.NET Core の移植に関するドキュメント](index.md)
