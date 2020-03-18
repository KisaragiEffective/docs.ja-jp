---
title: .NET Core にライブラリを移植する
description: ライブラリ プロジェクトを .NET Framework から .NET Core に移植する方法を説明します。
author: cartermp
ms.date: 12/07/2018
ms.openlocfilehash: 68fe36e543d949dc76bdb0c19ef3482936ad9e79
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79397909"
---
# <a name="port-net-framework-libraries-to-net-core"></a>.NET Framework ライブラリを .NET Core に移植する

.NET Framework ライブラリ コードを .NET Core に移植し、そこでクロスプラットフォームを実行して、それを使用するアプリの範囲を広げる方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事では、以下を前提とします。

- Visual Studio 2017 以降を使用している。 (それ以前のバージョンの Visual Studio では、.NET Core がサポートされていません。)
- [推奨の移植プロセス](index.md)を理解している。
- [サード パーティの依存関係](third-party-deps.md)の問題が解決されている。

次の記事の内容を理解している必要もあります。

[.NET Standard](../../standard/net-standard.md)\
この記事では、すべての .NET 実装で使用可能にすることを目的とした、.NET API の正式な仕様について説明します。

[パッケージ、メタパッケージ、フレームワーク](../packages.md)\
この記事では、.NET Core でのパッケージの定義と使用について説明すると共に、複数の .NET 実装で実行されるコードがパッケージでどのようにサポートされるかについて説明します。

[クロス プラットフォーム ツールによるライブラリの作成](../tutorials/libraries.md)\
この記事では、.NET Core CLI を使用してライブラリを作成する方法について説明します。

[.NET Core の *csproj* 形式に追加されたもの](../tools/csproj.md)\
この記事では、*csproj* および MSBuild への移行に伴ってプロジェクト ファイルに追加された変更について説明します。

[.NET Core への移植 - サード パーティの依存関係の分析](third-party-deps.md)\
この記事では、サード パーティの依存関係の移植性と、NuGet パッケージの依存関係が .NET Core で機能しない場合の対処方法について説明します。

## <a name="retarget-to-net-framework-472"></a>.NET Framework 4.7.2 に再ターゲット設定する

コードのターゲットが .NET Framework 4.7.2 でない場合、.NET Framework 4.7.2 に再ターゲット設定することをお勧めします。 ターゲット設定することで、.NET Standard が既存の API をサポートしていない場合に、最新の代替 API を使用できるようになります。

移植するプロジェクトごとに、Visual Studio で次の手順を実行します。

1. プロジェクトを右クリックし、 **[プロパティ]** を選択します。
1. **[対象とする Framework]** ボックスの一覧で、 **[.NET Framework 4.7.2]** を選択します。
1. プロジェクトを再コンパイルします。

プロジェクトのターゲットが .NET Framework 4.7.2 になったため、コード移植のベースとしてその .NET Framework を使用できます。

## <a name="determine-portability"></a>移植性を判別する

次の手順では、API Portability Analyzer (ApiPort) を実行して、分析用の移植性レポートを生成します。

[API Portability Analyzer (ApiPort)](../../standard/analyzers/portability-analyzer.md) を理解し、.NET Core をターゲットとする移植性レポートを生成する方法を理解している必要があります。 その方法は、ニーズと個人の好みによって変わります。 以下のセクションでは、いくつかの異なるアプローチについて詳しく説明します。 コードの構成内容によっては、これらのアプローチの手順を組み合わせて使用していることに気付くかもしれません。

### <a name="deal-primarily-with-the-compiler"></a>主にコンパイラを使用して対処する

このアプローチは、小さなプロジェクト、つまり多くの .NET Framework API を使用しないプロジェクトに適しています。 このアプローチは単純です。

1. 必要に応じてプロジェクトで ApiPort を実行します。 ApiPort を実行すると、レポートから対応が必要となる問題についての情報を得られます。
1. すべてのコードを新しい .NET Core プロジェクトにコピーします。
1. 移植性に関するレポート (生成されている場合) を参照しながら、プロジェクトが完全にコンパイルされるまでコンパイラ エラーを解決します。

このコード中心のアプローチは、構造化されていませんが、多くの場合に問題を迅速に解決します。 特に、データ モデルのみが含まれるプロジェクトにはこのアプローチが最適です。

### <a name="stay-on-the-net-framework-until-portability-issues-are-resolved"></a>移植性の問題が解決されるまで .NET Framework を使い続ける

このアプローチは、すべてのプロセスでコンパイルできるコードを作成したい場合にお勧めです。 アプローチは次のとおりです。

1. プロジェクトで ApiPort を実行します。
1. 移植可能な別の API を使用して問題に対処します。
1. 直接の代替方法を使用できない領域をすべて書き留めます。
1. 新しい .NET Core プロジェクトにコピーしても問題ないことが確認できるまで、移植するすべてのプロジェクトに対して前の手順を繰り返します。
1. コードを新しい .NET Core プロジェクトにコピーします。
1. 直接の代替手段がないと書き留めた問題に対処します。

この慎重なアプローチは、コンパイラ エラーに対処するだけの方法よりも体系的ですが、それでも比較的コード中心であり、コンパイルできるコードが常にあるという利点があります。 別の API を使用するだけでは解決できなかった問題を解決する方法は、大きく異なります。 プロジェクトによっては、次のアプローチのように、より包括的な計画を開発する必要があります。

### <a name="develop-a-comprehensive-plan-of-attack"></a>包括的な計画を新たに作成する

このアプローチは、.NET Core をサポートするために、コードの再構築や特定範囲の完全な書き換えが必要になる可能性があるような、大規模で複雑なプロジェクトにお勧めです。 アプローチは次のとおりです。

1. プロジェクトで ApiPort を実行します。
1. 移植できない性質のものが使用されている箇所と、それが移植性全体に与える影響を把握します。
   - 種類の性質を理解します。 数は少なくても使用頻度は高いですか。 数は多くても使用頻度は低いですか。 コードで集中して使用されていますか、それともあちこちで使用されていますか。
   - 移植できないコードの分離は簡単で、効果的に処理できますか。
   - コードをリファクタリングする必要はありますか。
   - 移植可能ではない種類の場合、同じ作業を実行できる代替 API はありますか。 たとえば、<xref:System.Net.WebClient> クラスを使用している場合は、代わりに <xref:System.Net.Http.HttpClient> クラスを使用できます。
   - 一時的な置き換えでない場合でも、作業を完了させるのに使用できる、移植可能な API が別にありますか。 たとえば、<xref:System.Xml.Schema.XmlSchema> を使用して XML を解析しているが、XML スキーマ検出が不要な場合、<xref:System.Xml.Linq> API を使用して API に依存しない解析を独自に実装できます。
1. 移植が困難なアセンブリがある場合、.NET Framework を今すぐ止める価値はあるでしょうか。 次の点を考慮することをお勧めします。
   - .NET Framework または Windows 固有の機能への依存度が高いために、.NET Core との互換性がない機能がライブラリにあるとします。 現時点ではその機能をあきらめ、機能を移植できるリソースが利用できるようになるまで、機能の少ない一時的な .NET Core バージョンのライブラリをリリースするのがよいでしょうか。
   - リファクタリングが有効でしょうか。
1. 使用できない .NET Framework API の代わりになる実装を開発することは理にかなっていますか。
   [.NET Framework の参照ソース](https://github.com/Microsoft/referencesource)のコードのコピー、変更、および使用も検討できます。 この参照ソース コードは [MIT ライセンス](https://github.com/Microsoft/referencesource/blob/master/LICENSE.txt)の下で使用が許可されているため、このソースをコードの基盤として、かなり自由に使用できます。 ただし、コードには Microsoft への帰属を適切に示してください。
1. プロジェクトごとにこのプロセスを繰り返します。

コードベースのサイズによっては、分析フェーズに時間がかかる場合があります。 このフェーズに時間を割き、必要な変更の範囲を完全に把握してから計画を立てることで、長期的には多くの時間を節約できます。特に、コードベースが複雑な場合には有効です。

コードベースの大幅な変更が必要な計画になる可能性がありますが、ターゲット設定は .NET Framework 4.7.2 です。そのため、これは前のアプローチよりも体系化されたバージョンです。 計画の実施方法は、コードベースによって異なります。

### <a name="mixed-approach"></a>混合アプローチ

多くの場合は、プロジェクトごとに、前述のアプローチを混合して利用します。 自分とコードベースにとって最も有用な処理を実行するようにします。

## <a name="port-your-tests"></a>テストを移植する

コードを移植したときにすべての機能が動作することを確認するには、コードを .NET Core に移植してテストすることをお勧めします。 このテストを行うには、.NET Core 用のテストを構築して実行するためのテスト フレームワークを使用する必要があります。 現在のところ、次の 3 つの選択肢があります。

- [xUnit](https://xunit.github.io/)
  - [作業の開始](https://xunit.github.io/docs/getting-started-dotnet-core.html)
  - [MSTest プロジェクトを xUnit に変換するツール](https://github.com/dotnet/codeformatter/tree/master/src/XUnitConverter)
- [NUnit](https://nunit.org/)
  - [作業の開始](https://github.com/nunit/docs/wiki/Installation)
  - [MSTest から NUnit への移行に関するブログ投稿](https://www.florian-rappl.de/News/Page/275/convert-mstest-to-nunit)
- [MSTest](/visualstudio/test/unit-test-basics)

## <a name="recommended-approach"></a>推奨されるアプローチ

最終的に、移植作業は .NET Framework コードの構成内容に大きく左右されます。 コードを移植するのに良い方法は、コードの基本コンポーネントであるライブラリの "*ベース*" から始めることです。 ベースは、その他すべてが直接または間接的に使用するデータ モデル、または他の基本クラスやメソッドの場合があります。

1. 移植対象のライブラリのレイヤーをテストするテスト プロジェクトを移植します。
1. ライブラリのベースを新しい .NET Core プロジェクトにコピーし、サポートする .NET Standard のバージョンを選択します。
1. 必要に応じてコードを変更し、コンパイルします。 多くの場合、NuGet パッケージの依存関係を *csproj* ファイルに追加する必要があります。
1. テストを実行し、必要な調整を行います。
1. 次のコード レイヤーを選択して移植し、前の手順を繰り返します。

ライブラリのベースから開始してベースから外側に向かい、必要に応じて各レイヤーをテストする場合、移植は、問題が一度でコードの 1 レイヤーに分離される体系的なプロセスになります。

## <a name="next-steps"></a>次のステップ

>[!div class="nextstepaction"]
>[.NET Core のプロジェクトを整理する](project-structure.md)
