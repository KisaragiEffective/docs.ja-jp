---
title: .NET Framework から .NET Core への移植
description: 移植プロセスを理解し、.NET Framework プロジェクトを .NET Core に移植する際に役立つツールを確認します。
author: cartermp
ms.date: 10/22/2019
ms.openlocfilehash: e483bb6e48dad6c3bf71bfa81e704a137fc02094
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75937313"
---
# <a name="overview-of-porting-from-net-framework-to-net-core"></a>.NET Framework から .NET Core への移植の概要

現在 .NET Framework で実行しているコードの .NET Core への移植を検討する場合があります。 この記事では、次の内容について説明します。

* 移植プロセスの概要。
* コードを .NET Core に移植するときに役立つ場合があるツールの一覧。

## <a name="overview-of-the-porting-process"></a>移植プロセスの概要

プロジェクトを .NET Core に移植する場合は、次の手順を使用することをお勧めします。

1. 移植するすべてのプロジェクトを、.NET Framework 4.7.2 以降をターゲットとするように再ターゲットします。

   この手順により、.NET Core で特定の API がサポートされない場合に、.NET Framework 固有のターゲットに対して API の代替を確実に使用できます。

2. [.NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md) を使ってアセンブリを分析し、それらが .NET Core に移植可能かどうかを確認します。

   API Portability Analyzer ツールによって、コンパイル済みのアセンブリが分析され、レポートが生成されます。 このレポートには、移植性に関する大まかな概要と、使用している API のうち NET Core では利用できないものそれぞれについての内訳が表示されます。

3. [.NET API アナライザー](../../standard/analyzers/api-analyzer.md)をプロジェクトにインストールして、一部のプラットフォームで <xref:System.PlatformNotSupportedException> をスローする API と、発生する可能性のあるその他の互換性の問題を識別します。

   このツールは移植性アナライザーに似ていますが、.NET Core 上にコードをビルドできるかどうかが分析されるのではなく、実行時に <xref:System.PlatformNotSupportedException> をスローするような方法で API を使っているかどうかが分析されます。 .NET Framework 4.7.2 以上から移行する場合、これは一般的ではありませんが、確認することをお勧めします。 .NET Core で例外をスローする API の詳細については、「[.NET Core で常に例外をスローする API](../compatibility/unsupported-apis.md)」を参照してください。

4. `packages.config`Visual Studio の変換ツール[を使用して、すべての ](/nuget/consume-packages/package-references-in-project-files) の依存関係を [PackageReference](/nuget/consume-packages/migrate-packages-config-to-package-reference) 形式に変換します。

   この手順では、依存関係を従来の `packages.config` 形式から変換します。 `packages.config` は .NET Core では機能しないため、パッケージの依存関係がある場合は、この変換が必須です。

5. .NET Core 用の新しいプロジェクトを作成してソース ファイルをコピーするか、ツールを使って既存のプロジェクト ファイルの変換を試みます。

   .NET Core では、.NET Framework よりも簡素化された (異なる) [プロジェクト ファイル形式](../tools/csproj.md)が使用されます。 続行するには、プロジェクト ファイルをこの形式に変換する必要があります。

6. テスト コードを移植します。

   .NET Core への移植はコードベースにとって大きな変更となるため、テスト プロジェクトを移植して、ご自分のコードの移植時にテストを実行できるようにすることを強くお勧めします。 MSTest、xUnit、NUnit はすべて .NET Core で動作します。

さらに、[dotnet try-convert](https://github.com/dotnet/try-convert) ツールを使って、より小規模なソリューションや個人のプロジェクトを、1 つの操作で .NET Core プロジェクトのファイル形式に移植してみることが可能です。 `dotnet try-convert` がすべてのプロジェクトに対して動作する保証はありません。また、これが原因となって、依存していた動作に微妙な変更が生じる可能性があります。 これは、自動化できる基本的なことを自動化するための "_開始点_" としてお使いください。 これは、プロジェクトの移行に対する保証されたソリューションではありません。

## <a name="next-steps"></a>次のステップ

>[!div class="nextstepaction"]
>[依存関係の分析](third-party-deps.md)
