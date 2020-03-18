---
title: .NET Framework 1.1 からの移行
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 4.5, migrating from 1.1
- .NET Framework 1.1, migrating to .NET Framework 4.5
ms.assetid: 7ead0cb3-3b19-414a-8417-a1c1fa198d9e
ms.openlocfilehash: 11fe9ba36d32a4c9fe363b48f76a8bb2b24f073b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73974969"
---
# <a name="migrate-from-the-net-framework-11"></a>.NET Framework 1.1 からの移行

Windows 7 以降のバージョンの Windows オペレーティング システムでは、.NET Framework 1.1 はサポートされません。 このため、.NET Framework 1.1 を対象とするアプリケーションは変更を行わないと、Windows 7 以降のバージョンのオペレーティング システムでは実行できません。 このトピックでは、.NET Framework 1.1 を対象とするアプリケーションを Windows 7 以降のバージョンの Windows オペレーティング システムで実行するために必要な手順について説明します。 .NET Framework 1.1 と Windows 8 の詳細については、[Windows 8 以降のバージョンでの .NET Framework 1.1 アプリの実行](../install/run-net-framework-1-1-apps.md)に関する記事を参照してください。

## <a name="retarget-or-recompile"></a>再ターゲットまたは再コンパイル

.NET Framework 1.1 を使用してコンパイルしたアプリケーションを Windows 7 以降のバージョンの Windows オペレーティング システムで実行するには、次の 2 つの方法があります。

- .NET Framework 4 以降のバージョンで動作するようにアプリケーションのターゲットを変更します。 再ターゲットするには、[\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 要素をアプリケーションの構成ファイルに追加して .NET Framework 4 以降のバージョンで動作できるようにする必要があります。 そのための構成ファイルの形式は次のとおりです。

    ```xml
    <configuration>
       <startup>
          <supportedRuntime version="v4.0"/>
       </startup>
    </configuration>
    ```

- .NET Framework 4 以降のバージョンをターゲットとするコンパイラでアプリケーションを再コンパイルします。 最初に Visual Studio 2003 を使用してソリューションを開発およびコンパイルした場合は、ソリューションを Visual Studio 2010 で (そしておそらく以降のバージョンでも) で開くことができます。また、 **[Project Compatibility]\(プロジェクト互換性\)** ダイアログ ボックスを使用して、ソリューションおよびプロジェクト ファイルを Visual Studio 2003 で使用される形式から Microsoft Build Engine (MSBuild) 形式に変換することができます。

再コンパイルまたは再ターゲットのいずれを選択するかに関係なく、アプリケーションが .NET Framework の新しいバージョンで導入された変更の影響を受けるかどうかを確認する必要があります。 変更には次の 2 種類があります。

- .NET Framework 1.1 とより新しいバージョンの .NET Framework の間で行われた破壊的変更。

- .NET Framework 1.1 とより新しいバージョンの .NET Framework の間で非推奨または旧形式とマークされた型と型のメンバー。

アプリケーションを再ターゲットするか、再コンパイルするかに関係なく、.NET Framework 1.1 より後にリリースされた .NET Framework の各バージョンについては、破壊的変更と旧式の型およびメンバーを確認する必要があります。

## <a name="breaking-changes"></a>互換性に影響する変更

互換性に影響する変更が行われた場合は、変更内容に応じて、アプリケーションの再ターゲットおよび再コンパイル時の回避策が提示される場合があります。 場合によっては、アプリケーションの構成ファイルの [\<runtime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 要素に子要素を追加することで、以前の動作を復元できます。 たとえば、次の構成ファイルでは .NET Framework 1.1 で使用される文字列の並べ替えおよび比較の動作が復元され、アプリケーションの再ターゲットまたは再コンパイルのいずれにも使用できます。

```xml
<configuration>
   <runtime>
      <CompatSortNLSVersion enabled="4096"/>
   </runtime>
</configuration>
```

ただし、ソース コードの変更とアプリケーションの再コンパイルが必要になる場合があります。

互換性に影響する可能性がある変更点がアプリケーションに与える影響を評価するには、次の変更一覧を確認する必要があります。

- 「[Breaking Changes in .NET Framework 2.0](https://docs.microsoft.com/previous-versions/aa570326(v=msdn.10))」 (.NET Framework 2.0 での破壊的変更) に記載されている .NET Framework 1.1 を対象とするアプリケーションに影響する可能性がある .NET Framework 2.0 SP1 の変更点。

- 「[.NET Framework 3.5 SP1 の変更点](https://docs.microsoft.com/previous-versions/dotnet/articles/dd310284(v=msdn.10))」に記載されている .NET Framework 3.5 と .NET Framework 3.5 SP1 間の変更点。

- 「[.NET Framework 4 への移行に関する問題](net-framework-4-migration-issues.md)」に記載されている .NET Framework 3.5 SP1 と .NET Framework 4 間の変更点。

## <a name="obsolete-types-and-members"></a>旧式の型およびメンバー

非推奨の型およびメンバーの影響は、アプリケーションを再ターゲットする場合と再コンパイルする場合とでは若干異なります。 旧式の型およびメンバーを使用しても、その型およびメンバーをアセンブリから物理的に削除しない限り、再ターゲットしたアプリケーションには影響しません。 旧式の型およびメンバーを使用してアプリケーションを再コンパイルすると、通常はコンパイラ エラーではなく、コンパイラの警告が発生します。 ただし、場合によってはコンパイラ エラーが発生し、旧式の型またはメンバーを使用したコードをコンパイルできないことがあります。 その場合は、旧式の型またはメンバーを呼び出すソース コードを変更してからアプリケーションを再コンパイルする必要があります。 旧式の型およびメンバーの詳細については、「[.NET Framework クラス ライブラリの互換性のために残されている機能](../whats-new/whats-obsolete.md)」を参照してください。

.NET Framework 2.0 SP1 のリリース以後に非推奨になった型およびメンバーの影響を評価するには、[クラス ライブラリの互換性のために残されている機能](../whats-new/whats-obsolete.md)に関するページを参照してください。 .NET Framework 2.0 SP1、.NET Framework 3.5、.NET Framework 4 については、旧式の型およびメンバーの一覧を確認してください。
