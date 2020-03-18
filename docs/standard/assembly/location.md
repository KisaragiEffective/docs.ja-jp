---
title: アセンブリの場所
ms.date: 08/20/2019
helpviewer_keywords:
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 9f1f41a7-2954-49d3-a2c0-62b6ef4d40ab
ms.openlocfilehash: 0b84aba749625f0f86027cd9d09a5e9a2229a3f2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73733129"
---
# <a name="assembly-location"></a>アセンブリの場所
アセンブリの場所は、参照時に共通言語ランタイムがそれを見つけることができるかどうかを決定します。また、アセンブリをその他のアセンブリと共有できるかどうかも決定できます。 次の場所にアセンブリを展開することができます。

- アプリケーションのディレクトリまたはサブディレクトリ

     これは、アセンブリを展開する最も一般的な場所です。 アプリケーションのルート ディレクトリのサブディレクトリは、言語またはカルチャを基にすることができます。 アセンブリにカルチャ属性の情報がある場合は、アプリケーション ディレクトリの下のサブディレクトリに、そのカルチャの名前で存在する必要があります。

- グローバル アセンブリ キャッシュ

     これは、共通言語ランタイムをインストールしている場所にインストールされている、コンピューター全体で使用できるコード キャッシュです。 ほとんどの場合、あるアセンブリを複数のアプリケーションで共有する場合は、そのアセンブリをグローバル アセンブリ キャッシュ内に展開する必要があります。

- HTTP サーバー上

     HTTP サーバーに展開されているアセンブリは、厳密な名前を持つ必要があります。アプリケーションの構成ファイルのコードベース セクションにあるアセンブリをポイントします。

## <a name="see-also"></a>参照

- [アセンブリを作成する](create.md)
- [グローバル アセンブリ キャッシュ](../../framework/app-domains/gac.md)
- [ランタイムがアセンブリを検索する方法](../../framework/deployment/how-the-runtime-locates-assemblies.md)
