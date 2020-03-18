---
title: '.NET Framework 4.8、4.7、4.6、4.5 移行ガイド '
ms.custom: updateeachrelease
ms.date: 04/18/2019
helpviewer_keywords:
- .NET Framework, migrating applications to
- migration, .NET Framework
ms.assetid: 02d55147-9b3a-4557-a45f-fa936fadae3b
ms.openlocfilehash: 2fa992e1c0897d360f322581888c51dca8d8a734
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73974987"
---
# <a name="migration-guide-to-the-net-framework-48-47-46-and-45"></a>.NET Framework 4.8、4.7、4.6、4.5 移行ガイド

旧バージョンの .NET Framework を使用してアプリを作成した場合、通常は .NET Framework 4.5 とそのポイント リリース (4.5.1 と 4.5.2)、.NET Framework 4.6 とそのポイント リリース (4.6.1 と 4.6.2)、.NET Framework 4.7 とそのポイント リリース (4.7.1 および 4.7.2)、または .NET Framework 4.8 へ簡単にアップグレードできます。 Visual Studio でプロジェクトを開きます。 プロジェクトが旧バージョンの Visual Studio で作成されている場合は、 **[Project Compatibility]\(プロジェクト互換性\)** ダイアログ ボックスが自動的に開きます。 Visual Studio におけるプロジェクトのアップグレードの詳細については、「[Visual Studio プロジェクトのポート、移行、アップグレード](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects)」と「[Visual Studio 2019 の対象プラットフォームと互換性](/visualstudio/releases/2019/compatibility)」を参照してください。

 ただし、.NET Framework で行われたいくつかの変更により、コードを変更する必要があります。 .NET Framework 4.5 とそのポイント リリース、.NET Framework 4.6 とそのポイント リリース、.NET Framework 4.7 とそのポイント リリース、または .NET Framework 4.8 の新しい機能を利用することもできます。 .NET Framework の新しいバージョンに対応するためのアプリに対するこの種の変更は、一般に*移行*と呼ばれます。 アプリを移行する必要がない場合、アプリは再コンパイルなしで .NET Framework 4.5 またはそれ以降のバージョンで実行できます。

## <a name="migration-resources"></a>移行のためのリソース

アプリを .NET Framework の旧バージョンからバージョン 4.5、4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2、または 4.8 に移行する前に、次のドキュメントを確認してください。

- [バージョンおよび依存関係](versions-and-dependencies.md)に関するページで、.NET Framework の各バージョンの基になる CLR バージョンの詳細と、アプリを正しくターゲットするためのガイドラインを確認してください。

- [アプリケーションの互換性](application-compatibility.md)に関するページで、アプリに影響を与える可能性のあるランタイムの変更と再ターゲットの変更、およびそれらの処理方法について確認してください。

- [クラス ライブラリの互換性のために残されている機能](../whats-new/whats-obsolete.md)に関するページで、コード内の互換性のために残されている型またはメンバーと、推奨される代替の型またはメンバーを確認してください。

- [新機能](../whats-new/index.md)に関するページで、アプリに追加できる可能性のある新機能の説明を確認してください。

## <a name="see-also"></a>関連項目

- [アプリケーションの互換性](application-compatibility.md)
- [.NET Framework 1.1 からの移行](migrating-from-the-net-framework-1-1.md)
- [バージョンの互換性](version-compatibility.md)
- [バージョンおよび依存関係](versions-and-dependencies.md)
- [方法: .NET Framework 4 以降のバージョンをサポートするアプリを構成する](how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
- [新機能](../whats-new/index.md)
- [クラス ライブラリ内にある旧版のもの](../whats-new/whats-obsolete.md)
- [.NET Framework の公式サポート ポリシー](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)
- [.NET Framework 4 の移行の問題](net-framework-4-migration-issues.md)
