---
title: 汎用性のあるクラス ライブラリを使用したプラットフォーム間の開発
ms.date: 09/17/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- Portable Class Library [.NET Framework]
- targeting multiple platforms
- multiple platforms, targeting
ms.assetid: c31e1663-c164-4e65-b66d-d3aa8750a154
ms.openlocfilehash: dd5e5612de15a499c0dce34dc30faa6fd5731c17
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124534"
---
# <a name="cross-platform-development-with-the-portable-class-library"></a>ポータブルクラスライブラリを使用したクロスプラットフォーム開発

Visual Studio のポータブルクラスライブラリのプロジェクトの種類を使用すると、Microsoft プラットフォーム向けのクロスプラットフォームアプリやライブラリをすばやく簡単にビルドできます。

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

ポータブル クラス ライブラリにより、コードの開発とテストにかかる時間とコストを削減できます。 このプロジェクトの種類を使用して、ポータブル .NET Framework アセンブリを作成してビルドし、.NET Framework、iOS、Mac などの複数のプラットフォームを対象とするアプリからそれらのアセンブリを参照します。

Visual Studio でポータブル クラス ライブラリ プロジェクトを作成し、プロジェクトの開発を開始した後でも、ターゲット プラットフォームを変更できます。 Visual Studio では、新しいアセンブリを使用してライブラリをコンパイルします。これにより、コードで行う必要がある変更を特定できます。

## <a name="create-a-portable-class-library-project"></a>ポータブルクラスライブラリプロジェクトを作成する

ポータブルクラスライブラリを作成するには、Visual Studio に用意されているテンプレートを使用します。 新しいプロジェクト (**ファイル** > **新しいプロジェクト**) を作成し、 **[新しいプロジェクト]** ダイアログボックスで、プログラミング言語 (ビジュアルC#または Visual Basic) を選択します。 次に、 **[クラスライブラリ (レガシポータブル)]** テンプレートを選択します。 プロジェクトの名前を入力し、[ **OK]** を選択します。

**[ポータブルクラスライブラリの追加]** ダイアログボックスが表示されます。 2つ以上のターゲットを選択し、[ **OK]** をクリックします。

![Visual Studio でポータブルクラスライブラリターゲットを追加する](media/add-portable-class-library.png)

## <a name="change-targets"></a>ターゲットの変更

ポータブルクラスライブラリプロジェクトを作成するとき、または開発を開始した後で、ターゲットプラットフォームを変更できます。 プロジェクトの作成後にターゲットを変更する場合は、**ソリューションエクスプローラー**で、ポータブルクラスライブラリプロジェクト (ソリューションではない) のショートカットメニューを開き、 **[プロパティ]** を選択します。 プロジェクトのプロパティページの **[ライブラリ]** タブに、プロジェクトが現在対象としているプラットフォームが表示されます。

![Visual Studio のポータブルクラスライブラリのプロジェクトプロパティ](media/pcl-project-properties.png)

ターゲットを追加または削除するには、 **[変更]** ボタンをクリックし、適切なチェックボックスをオンまたはオフにします。

ターゲットを変更すると、変更後のターゲットに合わせて、プロジェクトの開発に使用できる API が変更されます。 Visual Studio は、ターゲットを変更したことで発生する可能性があるエラーと警告を報告します。

Visual Studio で変更を加える前にアセンブリの移植性を評価する場合は、 [.Net 移植性アナライザー](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b)を使用できます。

## <a name="supported-types-and-members"></a>サポートされている型とメンバー

ポータブル クラス ライブラリのプロジェクトで使用できる型とメンバーは、互換性に関するいくつかの要因による制約を受けます。

- 選択したターゲット間で共有される必要があります。

- それらのターゲット間で同様に動作する必要があります。

- 廃止候補であってはなりません。

- 特にサポートしているメンバーがポータブルでない場合は、ポータブルな環境に意味がある必要があります。

メンバーがポータブル クラス ライブラリでサポートされており、選択したターゲットのメンバーである場合、IntelliSense でプロジェクトにこのメンバーが表示されます。 ただし、API がポータブル クラス ライブラリでサポートされている可能性があります。API を使用できるかどうかは、選択したターゲットによって異なります。

## <a name="api-differences-in-the-portable-class-library"></a>ポータブル クラス ライブラリでの API の相違点

サポートされるすべてのプラットフォームでポータブル クラス ライブラリ アセンブリの互換性を確保するために、ポータブル クラス ライブラリでは一部のメンバーが若干変更されています。

## <a name="use-the-portable-class-library"></a>ポータブルクラスライブラリを使用する

ポータブル クラス ライブラリ プロジェクトをビルドしたら、他のプロジェクトからそのプロジェクトを参照します。 プロジェクトを参照することも、アクセスする必要のあるクラスを含む特定のアセンブリを参照することもできます。

ポータブル クラス ライブラリ アセンブリを参照するアプリを実行するには、ターゲット プラットフォームの必要なバージョン (またはそれ以上のバージョン) がコンピューターにインストールされている必要があります。 Visual Studio には必要なすべてのフレームワークが含まれるため、さらに変更を加えることなく、アプリケーションの開発に使用したコンピューターでアプリケーションを実行できます。

### <a name="deploy-a-universal-windows-app"></a>ユニバーサル Windows アプリを展開する

ポータブルクラスライブラリアセンブリを参照するユニバーサル Windows アプリを作成する場合、アプリを展開するために必要なものはすべてアプリパッケージに含まれており、それ以上の手順は必要ありません。

### <a name="deploy-a-net-framework-app"></a>.NET Framework アプリをデプロイする

ポータブル クラス ライブラリ アセンブリを参照する .NET Framework アプリを配置するときは、.NET Framework の正しいバージョンに対する依存関係を指定する必要があります。 この依存関係を指定することで、必要なバージョンがアプリケーションと共に確実にインストールされます。

- ClickOnce 配置を使用して依存関係を作成するには:**ソリューションエクスプローラー**で、発行するプロジェクトのプロジェクトノードを選択します。 (これは、ポータブルクラスライブラリプロジェクトを参照するプロジェクトです)。メニューバーで、[**プロジェクト** > **プロパティ**] を選択し、 **[発行]** タブを選択します。 **[発行]** ページで、 **[前提条件]** を選択します。 必須コンポーネントとして、必要な .NET Framework のバージョンを選択します。

- セットアッププロジェクトとの依存関係を作成するには、**ソリューションエクスプローラー**で、セットアッププロジェクトを選択します。 メニューバーで、[**プロジェクト** > の**プロパティ** > **必須コンポーネント**] を選択します。 必須コンポーネントとして、必要な .NET Framework のバージョンを選択します。

.NET Framework アプリのデプロイの詳細については、「[開発者向け配置ガイド](../../../docs/framework/deployment/deployment-guide-for-developers.md)」を参照してください。

## <a name="see-also"></a>参照

- [MVVM を利用した汎用性のあるクラス ライブラリの使用](../../../docs/standard/cross-platform/using-portable-class-library-with-model-view-view-model.md)
- [複数のプラットフォームを対象とするライブラリのアプリケーション リソース](../../../docs/standard/cross-platform/app-resources-for-libraries-that-target-multiple-platforms.md)
- [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
- [Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../../docs/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)
- [デプロイ](../../../docs/framework/deployment/net-framework-applications.md)
