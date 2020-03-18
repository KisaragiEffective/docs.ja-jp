---
title: .NET Framework および特別なリリース
ms.date: 10/10/2018
ms.assetid: 721f10fa-3189-4124-a00d-56ddabd889b3
ms.openlocfilehash: a2dcd011548df857a1399c5bbdfe6ed33927672e
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716400"
---
# <a name="the-net-framework-and-out-of-band-releases"></a>.NET Framework および特別なリリース

.NET Framework は、従来のデスクトップ アプリや Web アプリに加えて、Windows Phone や Windows ストア アプリなど、さまざまなプラットフォームに対応し、コードを最大限に再利用できるように発展しています。 通常の .NET Framework のリリースに加えて、クロスプラットフォームでの開発の強化や新機能の導入を目的として、新しい機能をアウトオブバンド (OOB) リリースによって提供しています。 ここでは、.NET Framework および OOB リリースの将来の方向性について説明します。

## <a name="advantages-of-oob-releases"></a>OOB リリースの長所
 新しいコンポーネントやコンポーネントに対する更新プログラムをアウトオブバンドで提供することにより、Microsoft では .NET Framework の更新プログラムをより頻繁に提供できます。 また、カスタマーからのフィードバックにすばやく収集して対応できます。

 独自のアプリで OOB 機能を使用する場合、OOB アセンブリはアプリのパッケージで配置されるため、ユーザーはアプリを実行するために最新バージョンの .NET Framework をインストールする必要はありません。

## <a name="how-oob-packages-are-distributed"></a>OOB パッケージの配布方法
共通言語ランタイム (CLR) のコア コンポーネントの OOB リリースは、.NET 用パッケージ マネージャーである [NuGet](https://www.nuget.org/) を通じて配布されます。 NuGet によって、Visual Studio のソリューション エクスプローラーから簡単に、.NET Framework プロジェクトを参照したり、ライブラリを追加したりすることができます。 NuGet は、Visual Studio 2012 以降のすべてのエディションに付属しています。 NuGet がインストールされているかどうかを確認するには、Visual Studio の **[ツール]** メニューの **[NuGet パッケージ マネージャー]** を検索します。 インストールされていない場合:

1. Visual Studio のメニュー バーで、 **[ツール]** 、 **[拡張機能と更新プログラム]** を選択します (Visual Studio 2010 では、 **[拡張機能マネージャー]** を選択します)。

     **[拡張機能と更新プログラム]** ダイアログ ボックスが表示されます。

2. **[オンライン]** 、 **[NuGet パッケージ マネージャー]** を選択し、 **[ダウンロード]** を選択します。

3. ダウンロードが完了したら、Visual Studio を再起動します。

 詳細なインストール方法については、NuGet Docs Web サイトの「[Installing NuGet](/nuget/install-nuget-client-tools)」(Nuget のインストール) を参照してください。 NuGet の詳細については、[NuGet のドキュメント](/nuget)を参照してください。

## <a name="using-a-nuget-oob-package"></a>NuGet OOB パッケージの使用
 NuGet をインストールした後、Visual Studio のソリューション エクスプローラーを使用して NuGet パッケージの表示や、NuGet パッケージへの参照の追加を行うことができます:

1. Visual Studio でプロジェクトのショートカット メニューを開き、 **[NuGet パッケージの管理]** を選択します。 (このオプションは、 **[プロジェクト]** メニューからも使用できます。)

2. 左ペインで、 **[オンライン]** を選択します。

3. プレリリースのパッケージを使用する場合は、中央のペインのドロップダウン リスト ボックスで **[安定版パッケージのみ]** の代わりに **[リリース前のパッケージを含める]** を選択します。

4. 右ペインで、 **[検索]** ボックスを使用して使用するパッケージを検索します。 Microsoft の一部のパッケージは、Microsoft .NET Framework のロゴで識別され、すべて発行者は Microsoft となっています。

 ![NuGet パッケージ マネージャーを示すスクリーンショット。](./media/the-net-framework-and-out-of-band-releases/nuget-package-manager-dialog.png)

 OOB パッケージを使用するアプリケーションを配置する場合は、前述のとおり、OOB のアセンブリは、アプリのパッケージに付属しています。

## <a name="types-of-oob-releases"></a>OOB のリリースの種類
 通常、OOB パッケージには、1 つ以上のプレリリース バージョンと 1 つの安定したバージョンがあります。 プレリリースに含まれるライセンスでは通常、再配布は許可されませんが、パッケージを試用して、フィードバックを提供することができます。 フィードバックはパッケージに対する更新プログラムに組み込まれます。 最終リリースは NuGet を使って安定したパッケージとして配布され、アプリと共に NuGet パッケージを再配布できるライセンスが含まれます。 安定したパッケージは Microsoft によってサポートされています。 Microsoft では、IntelliSense のサポートや、ブログの投稿、フォーラムでの回答などの別の種類のドキュメントを、すべてのパッケージについて提供します。 また、すべてのパッケージではありませんが、一部のパッケージについてはソース コードも利用できる場合があります。 新しいパッケージや更新パッケージに関するお知らせについては、「[.NET Framework ブログ](https://devblogs.microsoft.com/dotnet/)」を受信登録できます。

 リリース前のパッケージと安定版パッケージの両方を検索するには、NuGet パッケージ マネージャーで **[リリース前のパッケージを含める]** を選択してください。

## <a name="see-also"></a>関連項目

- [はじめに](index.md)
