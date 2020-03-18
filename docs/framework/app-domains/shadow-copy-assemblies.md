---
title: アセンブリのシャドウ コピー
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], shadow copying
- application domains, shadow copying assemblies
- shadow copying assemblies
ms.assetid: de8b8759-fca7-4260-896b-5a4973157672
ms.openlocfilehash: 9fc8a4aeeeca40f71ed9114a9db40b9a56e5fe6b
ms.sourcegitcommit: 81ad1f09b93f3b3e6706a7f2e4ddf50ef229ea3d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74204569"
---
# <a name="shadow-copying-assemblies"></a>アセンブリのシャドウ コピー

シャドウ コピーにより、アプリケーション ドメインをアンロードしなくても、アプリケーション ドメインで使用されるアセンブリを更新できます。 これは、ASP.NET サイトなど、継続的に使用可能であることが必要なアプリケーションで特に役立ちます。

> [!IMPORTANT]
> シャドウコピーは、Windows 8.x ストアアプリではサポートされていません。

共通言語ランタイムは、アセンブリがアンロードされるまでファイルを更新できないように、アセンブリがロードされるときにアセンブリ ファイルをロックします。 アプリケーション ドメインからアセンブリをアンロードする唯一の方法は、アプリケーション ドメインをアンロードすることなので、通常の状況では、アセンブリを使用しているすべてのアプリケーション ドメインがアンロードされるまで、そのアセンブリをディスク上で更新することはできません。

ファイルをシャドウ コピーするようにアプリケーション ドメインが構成されると、アプリケーション パスからのアセンブリが別の場所にコピーされて、その場所から読み込まれます。 コピーはロックされますが、元のアセンブリ ファイルはロック解除されて、更新可能になります。

> [!IMPORTANT]
> シャドウ コピーが可能な唯一のアセンブリは、アプリケーション ドメインの構成時に <xref:System.AppDomainSetup.ApplicationBase%2A> または <xref:System.AppDomainSetup.PrivateBinPath%2A> プロパティで指定された、アプリケーション ディレクトリやそのサブディレクトリに保存されているものです。 グローバル アセンブリ キャッシュに格納されているアセンブリは、シャドウ コピーされません。

この記事は、次のセクションで構成されています。

- 「[シャドウ コピーの有効化と使用](#EnablingAndUsing)」では、シャドウ コピーの基本的な使用方法と使用可能なオプションについて説明します。

- 「[起動時のパフォーマンス](#StartupPerformance)」では、起動時のパフォーマンスを改善するために .NET Framework 4 でシャドウ コピーに加えられた変更、および以前のバージョンの動作に戻す方法について説明します。

- 「[廃止メソッド](#ObsoleteMethods)」では、.NET Framework 2.0 でシャドウ コピーを制御するためにプロパティとメソッドに加えられた変更について説明します。

<a name="EnablingAndUsing"></a>

## <a name="enabling-and-using-shadow-copying"></a>シャドウ コピーの有効化と使用

<xref:System.AppDomainSetup> クラスのプロパティを以下のように使用して、シャドウ コピー用のアプリケーション ドメインを構成できます。

- <xref:System.AppDomainSetup.ShadowCopyFiles%2A> プロパティを文字列値に `"true"` に設定して、シャドウ コピーを有効にします。

  既定では、この設定は、アプリケーション パス内のすべてのアセンブリを、それらが読み込まれる前にダウンロード キャッシュにコピーします。 これは、他のコンピューターからダウンロードしたファイルを格納するために共通言語ランタイムによって保守されるものと同じキャッシュで、不要になったときには、共通言語ランタイムが自動的にファイルを削除します。

- 必要に応じて、<xref:System.AppDomainSetup.CachePath%2A> プロパティと <xref:System.AppDomainSetup.ApplicationName%2A> プロパティを使用して、シャドウ コピーしたファイルのカスタムの場所を設定します。

  この場所の基本パスは、<xref:System.AppDomainSetup.ApplicationName%2A> プロパティに <xref:System.AppDomainSetup.CachePath%2A> プロパティをサブディレクトリとして連結することにより形成されます。 アセンブリは、基本パス自体ではなく、このパスのサブディレクトリにシャドウ コピーされます。

  > [!NOTE]
  > <xref:System.AppDomainSetup.ApplicationName%2A> プロパティが設定されていない場合、<xref:System.AppDomainSetup.CachePath%2A> プロパティは無視されて、ダウンロード キャッシュが使用されます。 例外をスローすることはありません。

  カスタムの場所を指定する場合は、不要になったときに、ディレクトリとコピー済みファイルをユーザーがクリーンアップする必要があります。 それらは自動的には削除されません。

  シャドウ コピーされるファイル用にカスタムの場所を設定すると良いいくつかの理由があります。 アプリケーションが生成するコピーの数が多い場合は、シャドウ コピーされるファイル用にカスタムの場所を設定することができます。 ダウンロード キャッシュは有効期間ではなくサイズによって制限されるので、共通言語ランタイムが、まだ使用されているファイルを削除しようとする可能性があります。 カスタムの場所を設定するもう 1 つの状況は、アプリケーションを実行しているユーザーに、共通言語ランタイムがダウンロード キャッシュ用に使用するディレクトリの場所に対する書き込みアクセス権がない場合です。

- 必要に応じて、<xref:System.AppDomainSetup.ShadowCopyDirectories%2A> プロパティを使用して、シャドウ コピーするアセンブリの数を制限します。

  アプリケーション ドメインのシャドウ コピーを有効にすると、既定では、アプリケーション パス内の、つまり <xref:System.AppDomainSetup.ApplicationBase%2A> と <xref:System.AppDomainSetup.PrivateBinPath%2A> プロパティで指定されたディレクトリ内の、すべてのアセンブリがコピーされます。 シャドウ コピーするディレクトリのみを含む文字列を作成して、その文字列を <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> プロパティに割り当てることにより、コピー操作を選択したディレクトリに制限できます。 ディレクトリをセミコロンで区切ります。 シャドウ コピーされるアセンブリは、選択したディレクトリ内のものだけです。

  > [!NOTE]
  > 文字列を <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> プロパティに割り当てない場合、またはこのプロパティを `null` に設定した場合は、<xref:System.AppDomainSetup.ApplicationBase%2A> と <xref:System.AppDomainSetup.PrivateBinPath%2A> プロパティで指定したディレクトリ内のすべてのアセンブリがシャドウ コピーされます。

  > [!IMPORTANT]
  > セミコロンは区切り文字なので、ディレクトリ パスにはセミコロンを含めないでください。 セミコロンのためのエスケープ文字はありません。

<a name="StartupPerformance"></a>

## <a name="startup-performance"></a>起動時のパフォーマンス

シャドウ コピーを使用するアプリケーション ドメインを開始するときには、アプリケーション ディレクトリ内のアセンブリをシャドウ コピー ディレクトリにコピーしたり、既にその場所にある場合には検証したりするので、遅延が生じます。 .NET Framework 4 の前には、すべてのアセンブリを一時ディレクトリにコピーしていました。 各アセンブリを開いて、アセンブリ名を確認し、厳密な名前は検証しました。 各アセンブリを検査して、シャドウ コピーのディレクトリにあるコピーよりも最近に更新されていないかどうかを調べました。 更新されていた場合、それはシャドウ コピーのディレクトリにコピーしました。 最後に、一時コピーを破棄しました。

.NET Framework 4 以降は、起動時の既定の動作として、アプリケーション ディレクトリ内にある各アセンブリのファイルの日時を、シャドウ コピーのディレクトリ内にあるコピーのファイルの日時と直接比較します。 アセンブリが更新されている場合は、.NET Framework の以前のバージョンと同じ手順を使用してそれをコピーし、そうでない場合は、シャドウ コピーのディレクトリ内にあるコピーが読み込まれます。

結果としてのパフォーマンスの改善は、アセンブリの変更頻度が小さく、通常はアセンブリの小さなサブセット内で変更が生じるようなアプリケーションで最大となります。 アプリケーション内のアセンブリの大部分が頻繁に変更される場合は、新しい既定動作によって、パフォーマンスが低下する可能性があります。 [\<shadowCopyVerifyByTimestamp> 要素](../configure-apps/file-schema/runtime/shadowcopyverifybytimestamp-element.md)を構成ファイルに `enabled="false"` と共に追加することによって、.NET Framework の以前のバージョンの起動動作を復元できます。

<a name="ObsoleteMethods"></a>

## <a name="obsolete-methods"></a>廃止メソッド

<xref:System.AppDomain> クラスには、アプリケーション ドメインでシャドウ コピーを制御するために使用できる <xref:System.AppDomain.SetShadowCopyFiles%2A> や <xref:System.AppDomain.ClearShadowCopyPath%2A> などのいくつかのメソッドがありますが、これらは .NET Framework version 2.0 では廃止された機能としてマークされています。 シャドウ コピー用にアプリケーション ドメインを構成するには、<xref:System.AppDomainSetup> クラスのプロパティを使用することをお勧めします。

## <a name="see-also"></a>参照

- <xref:System.AppDomainSetup.ShadowCopyFiles%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.CachePath%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.ApplicationName%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.ShadowCopyDirectories%2A?displayProperty=nameWithType>
- [\<shadowCopyVerifyByTimestamp> 要素](../configure-apps/file-schema/runtime/shadowcopyverifybytimestamp-element.md)
