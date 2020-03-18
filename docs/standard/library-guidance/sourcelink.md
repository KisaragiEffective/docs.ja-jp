---
title: ソース リンクと .NET ライブラリ
description: ソース リンクを使用して .NET ライブラリのデバッグ機能を改善するためのベスト プラクティス推奨事項。
ms.date: 01/15/2019
ms.openlocfilehash: 3d768ae6e79efa23a8402ea37bc34cd58cd52c8c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76744540"
---
# <a name="source-link"></a>ソース リンク

ソース リンクは NuGet からの .NET アセンブリのソース コードを開発者がデバッグすることを可能にするテクノロジです。 ソース リンクは NuGet パッケージの作成時に実行され、ソース コントロール メタデータをアセンブリとパッケージの内部に埋め込みます。 パッケージをダウンロードし、Visual Studio でソース リンクを有効にした開発者は、そのソース コードにステップ インできます。 ソース リンクからは、効率的なデバッグを可能にするソース コントロール メタデータが提供されます。

## <a name="source-link-demo"></a>ソース リンクのデモ

> [!VIDEO https://www.youtube.com/embed/gyRGhCQPkB4?start=61]

## <a name="using-source-link"></a>ソース リンクを使用する

ソース リンクの使用方法は、[dotnet/sourcelink](https://github.com/dotnet/sourcelink/blob/master/README.md) GitHub リポジトリにあります。

[NuGet パッケージ エクスプローラー](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)を使用すれば、ソース リンク メタデータがパッケージに正常に埋め込まれたことを確認できます。 `Repository` メタデータがコミット ID と共に存在すること、各ターゲットの .dll と共に .pdb ファイルが見つかることを確認します。

![NuGet パッケージ エクスプローラーのソース リンク](./media/sourcelink/nuget-package-explorer-sourcelink.png "NuGet パッケージ エクスプローラーのソース リンク")

✔️ 検討 ソース リンクを使用して、お使いのアセンブリと NuGet パッケージにソース管理のメタデータを追加する。

> [!TIP]
> デバッガー属性を型に追加することで開発者のデバッグ機能をさらに強化できます。
>
> * <xref:System.Diagnostics.DebuggerDisplayAttribute> では、デバッガーの変数ウィンドウでクラスやフィールドを表示する方法をカスタマイズできます。
> * <xref:System.Diagnostics.DebuggerStepThroughAttribute> では、デバッガーに対してコードのステップ インではなくステップ実行が指示されます。
> * <xref:System.Diagnostics.DebuggerBrowsableAttribute> では、デバッガー変数ウィンドウにメンバーを表示するかどうかが制御されます。

✔️ 検討 シンボル ファイルを発行する (`*.pdb`)。

> デバッグのエクスペリエンスを最善にするには、ライブラリ上でシンボル ファイルを発行してソース リンクを使用する必要があります。 シンボル ファイルとシンボル パッケージの詳細については、「[シンボル パッケージ](./nuget.md#symbol-packages)」を参照してください。

>[!div class="step-by-step"]
>[前へ](dependencies.md)
>[次へ](publish-nuget-package.md)
