---
title: ランタイムの変更と再ターゲットの変更 - .NET Framework
ms.date: 10/29/2019
helpviewer_keywords:
- application compatibility
- .NET Framework application compatibility
- .NET Framework changes
ms.assetid: c4ba3ff2-fe59-4c5d-9e0b-86bba3cd865c
ms.openlocfilehash: c46f781d495b87a4f24e77935df7c4814c8567ae
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73196701"
---
# <a name="application-compatibility-in-the-net-framework"></a>.NET Framework のアプリケーションの互換性

.NET の各リリースにおいて、互換性は重要な目標です。 各バージョンが付加的な場合は互換性が確保され、以前のバージョンも引き続き動作します。 一方、以前の機能に変更が生じた場合 (パフォーマンスの向上、セキュリティ上の問題の解決、またはバグの修正などを目的とした)、既存のコードまたは既存のアプリケーションを以降のバージョンで実行すると互換性に問題が発生する可能性があります。

各アプリは、.NET Framework の特定のバージョンをターゲットとします。バージョンを特定する方法は次のとおりです。

- Visual Studio でターゲット フレームワークを定義する。
- プロジェクト ファイルでターゲット フレームワークを指定する。
- <xref:System.Runtime.Versioning.TargetFrameworkAttribute> をソース コードに適用する。

.NET Framework のあるバージョンを別のバージョンに移行する場合、考慮する変更には、次の 2 種類があります。

- [ランタイムの変更点](#runtime-changes)
- [変更の再ターゲット](#retargeting-changes)

## <a name="runtime-changes"></a>ランタイムの変更

ランタイムの問題とは、コンピューターに新しいランタイムを配置し、アプリの動作が変更された場合に発生する問題です。 ターゲットに指定されたバージョンより新しいバージョンでアプリが実行されると、.NET Framework は*後方互換*動作によって、ターゲットに指定されている古いバージョンを模倣します。 アプリは新しいバージョンで実行されますが、古いバージョンで実行される場合と同様に動作します。 .NET Framework のバージョン間の互換性の問題の多くは、この後方互換モデルを通して対応されます。 たとえば、.NET Framework 4.0 向けにコンパイルされたバイナリを、.NET Framework 4.5 以降がインストールされたコンピューターで実行する場合、.NET Framework 4.0 互換モードで実行されます。 つまり、4.5 以降のバージョンの変更の多くは、そのバイナリには影響しません。

アプリケーションがターゲットとする .NET Framework のバージョンは、コードが実行されるアプリケーション ドメインのエントリ アセンブリのターゲット バージョンによって決まります。 そのアプリケーション ドメインに読み込まれたすべての追加アセンブリは、そのバージョンをターゲットとします。 たとえば、実行可能ファイルの場合、実行可能ファイルのターゲットとなるフレームワークは、そのアプリケーション ドメイン内のすべてのアセンブリが実行される互換モードになります。

ご使用の環境に適用されるランタイムの変更の一覧を表示するには、現在ターゲットとしている NET Framework のバージョンを選択した後、移行先のバージョンを選択します。

[!INCLUDE[versionselector](../../../includes/migration-guide/runtime/versionselector.md)]

## <a name="retargeting-changes"></a>変更の再ターゲット

再ターゲットの変更とは、アセンブリを再コンパイルして新しいバージョンをターゲットとする場合に生じる変更です。 新しいバージョンにターゲットするということは、アセンブリで新しい機能が選択されるだけでなく、古い機能との互換性の問題が発生する可能性があることも意味します。

ご使用の環境に適用される再ターゲットの変更の一覧を表示するには、現在ターゲットとしている NET Framework のバージョンを選択した後、移行先のバージョンを選択します。

[!INCLUDE[versionselector](../../../includes/migration-guide/retargeting/versionselector.md)]

## <a name="impact-classification"></a>影響の分類

ランタイムの変更および再ターゲットの変更について説明するトピック (たとえば、「[.NET Framework 4.7.2 から 4.8 への移行に関する再ターゲットの変更](retargeting/4.7.2-4.8.md)」) では、個々の項目を、次のような予想される影響別に分類しています。

**Major**\
多数のアプリに影響するか、またはコードに大幅な変更が必要な、重大な変更。

**Minor**\
少数のアプリに影響するか、コードにわずかな変更を加える必要がある変更。

**エッジ ケース**\
一般的ではない特定のシナリオでアプリに影響を与える変更。

**透明**\
アプリの開発者やユーザーには大きな影響を及ぼさない変更。 アプリはこの変更のために変更を加える必要はありません。

## <a name="see-also"></a>参照

- [バージョンおよび依存関係](versions-and-dependencies.md)
- [新機能](../whats-new/index.md)
- [廃止になった機能](../whats-new/whats-obsolete.md)
