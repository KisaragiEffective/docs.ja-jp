---
title: アセンブリを作成する
ms.date: 08/19/2019
helpviewer_keywords:
- assemblies [.NET Framework], multifile
- single-file assemblies
- assemblies [.NET Framework], creating
- multifile assemblies
ms.assetid: 54832ee9-dca8-4c8b-913c-c0b9d265e9a4
ms.openlocfilehash: 81fffb2b2e1d56d6068bf6f663a13fad6968a383
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73740506"
---
# <a name="create-assemblies"></a>アセンブリを作成する

Visual Studio などの IDE や、Windows SDK によって提供されるコンパイラやツールを使って、単一ファイルまたはマルチファイルのアセンブリを作成できます。 最も単純なアセンブリは、単純な名前を持ち、単一のアプリケーション ドメインに読み込まれる単一のファイルです。 このアセンブリはアプリケーション ディレクトリの外部にある他のアセンブリからは参照できず、バージョン チェックが行われません。 アセンブリで構成されるアプリケーションをアンインストールするには、アプリケーションが存在するディレクトリを削除します。 多くの開発者は、このような機能を含むアセンブリがあれば、アプリケーションを展開できます。

マルチファイル アセンブリは、複数のコード モジュールおよびリソース ファイルから作成できます。 複数のアプリケーションで共有できるアセンブリも作成できます。 共有アセンブリは厳密な名前を持つ必要があり、グローバル アセンブリ キャッシュに展開することができます。

コード モジュールとリソースをアセンブリにグループ化する場合、次のような要因に応じていくつかのオプションがあります。

- バージョン管理

     同じバージョン情報が含まれている必要があるグループ モジュール。

- 配置

     展開のモデルをサポートするグループ コードのモジュールおよびリソース。

- 再利用

     いくつかの目的のために論理的に同時に使用できる場合はグループ モジュール。 たとえば、プログラムのメンテナンスで頻繁に使用される型とクラスで構成されるアセンブリは同じアセンブリに配置することができます。 さらに、複数のアプリケーションで共有する予定の型をアセンブリにグループ化し、そのアセンブリを厳密な名前で署名する必要があります。

- セキュリティ

     同じセキュリティ アクセス許可が必要な型を含むグループ モジュール。

- スコープ

     参照可能範囲を同じアセンブリに制限する必要がある型で構成されるグループ モジュール。

アンマネージド COM アプリケーションで使用できる共通言語ランタイム アセンブリを作成するときは、特別な考慮事項があります。 アンマネージド コードの使用の詳細については、「[COM への .NET コンポーネントの公開](../../framework/interop/exposing-dotnet-components-to-com.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [アセンブリのバージョン管理](versioning.md)
- [方法: シングルファイル アセンブリをビルドする](../../framework/app-domains/build-single-file-assembly.md)
- [方法: マルチファイル アセンブリをビルドする](../../framework/app-domains/build-multifile-assembly.md)
- [ランタイムがアセンブリを検索する方法](../../framework/deployment/how-the-runtime-locates-assemblies.md)
- [マルチファイル アセンブリ](../../framework/app-domains/multifile-assemblies.md)
