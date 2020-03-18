---
title: .NET API アナライザー
description: 非推奨の API およびプラットフォームの互換性の問題を検出するのに .NET API アナライザーがどのように役立つかについて説明します。
author: oliag
ms.date: 02/20/2020
ms.technology: dotnet-standard
ms.openlocfilehash: e214c91f2beebc7f3b3324f4879deba9a5623f86
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156135"
---
# <a name="net-api-analyzer"></a>.NET API アナライザー

.NET API アナライザーは、さまざまなプラットフォームでの C# API の互換性リスクの可能性および非推奨の API の呼び出しを検出する Roslyn アナライザーです。 開発のすべての段階ですべての C# 開発者に役立ちます。

API アナライザーは、NuGet パッケージ [Microsoft.DotNet.Analyzers.Compatibility](https://www.nuget.org/packages/Microsoft.DotNet.Analyzers.Compatibility/) として提供されています。 プロジェクトでこれを参照すると、コードが自動的に監視されて、問題のある API 使用が示されます。 また、電球アイコンをクリックすると、考えられる修正方法の提案を得ることもできます。 ドロップダウン メニューには、警告を抑制するオプションが含まれます。

> [!NOTE]
> .NET API アナライザーは、まだプレリリース バージョンです。

## <a name="prerequisites"></a>必須コンポーネント

- Visual Studio 2017 以降のバージョン、または Visual Studio for Mac (すべてのバージョン)。

## <a name="discover-deprecated-apis"></a>非推奨の API を検出する

### <a name="what-are-deprecated-apis"></a>非推奨の API とは

.NET ファミリは大規模な製品のセットであり、より適切に顧客のニーズに対応するため頻繁にアップグレードされています。 当然、API が廃止されたり新しいものに置き換えられたりすることがあります。 より優れた代替 API が存在する API は、非推奨と見なされます。 API が非推奨であり使ってはならないことを通知する手段の 1 つは、<xref:System.ObsoleteAttribute> 属性で API をマークすることです。 この方法の欠点は、すべての非推奨 API に対して診断 ID が 1 つしかないことです (C# の場合、[CS0612](../../csharp/misc/cs0612.md))。 これによって、次のことが起こります。

- ケースごとに専用のドキュメントを作成できません。
- 特定のカテゴリの警告を抑制できません。 すべてを抑制するか、まったく抑制しないかです。
- 新しい非推奨 API をユーザーに通知するには、参照されているアセンブリまたは対象のパッケージを更新する必要があります。

API アナライザーは、DE (Deprecation Error の略) で始まる API 固有のエラー コードを使うので、個々の警告の表示を制御できます。 アナライザーによって識別された非推奨の API は、[dotnet/platform-compat](https://github.com/dotnet/platform-compat) リポジトリで定義されています。

### <a name="add-the-api-analyzer-to-your-project"></a>API アナライザーをプロジェクトに追加する

1. Visual Studio を開きます。
2. アナライザーを実行するプロジェクトを開きます。
3. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 (このオプションは、 **[プロジェクト]** メニューからも使用できます。)
4. [NuGet パッケージ マネージャー] タブ上で、次を実行します。
   1. [パッケージ ソース] として [nuget.org] を選択します。
   2. **[参照]** タブに移動します。
   3. **[プレリリースを含める]** を選択します。
   4. **[Microsoft.DotNet.Analyzers.Compatibility]** を検索します。
   5. 一覧にある該当のパッケージを選択します。
   6. **[インストール]** ボタンを選択します。
   7. **[変更のプレビュー]** ダイアログの **[OK]** を選択します。表示されているパッケージのライセンス条項に同意する場合は、 **[ライセンスの同意]** ダイアログの **[同意する]** を選択します。

### <a name="use-the-api-analyzer"></a>API アナライザーを使用する

非推奨の API (<xref:System.Net.WebClient> など) がコードで使われていると、API アナライザーは緑の波線でそれを強調します。 API 呼び出しをポイントすると、次の例のように、電球アイコンと API の非推奨に関する情報が表示されます。

!["緑の波線が表示された WebClient API と左側の電球アイコンのスクリーンショット"](media/api-analyzer/green-squiggle.jpg)

**[エラー一覧]** ウィンドウには、非推奨の API ごとに一意の ID を含む警告が表示されます。次に示すのは `DE004` の例です。

!["警告の ID と説明が表示されている [エラー一覧] ウィンドウのスクリーンショット"](media/api-analyzer/warnings-id-and-descriptions.jpg "警告を含むエラー一覧ウィンドウ。")

ID をクリックすると、API が非推奨になった理由に関する詳細情報と、使用できる代替 API に関する提案が表示される Web ページに移動します。

強調表示されたメンバーを右クリックして **[\<診断 ID> の非表示]** を選ぶと、警告を抑制できます。 警告を抑制するには 2 つの方法があります。

- [ローカル (ソース内)](#suppress-warnings-locally)
- [グローバル (抑制ファイル内)](#suppress-warnings-globally) - 推奨

### <a name="suppress-warnings-locally"></a>警告をローカルに抑制する

警告をローカルに抑制するには、警告を抑制するメンバーを右クリックして、 **[クイック アクションとリファクタリング]**  >  **[\<*診断 ID*> の非表示]**  >  **[ソース内]** の順に選びます。 [#pragma](../../csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning.md) 警告プリプロセッサ ディレクティブが、定義されているスコープ内のソース コードに追加されます。!["#pragma warning disable で囲まれたコードのスクリーンショット"](media/api-analyzer/suppress-in-source.jpg)

### <a name="suppress-warnings-globally"></a>警告をグローバルに抑制する

警告をグローバルに抑制するには、警告を抑制するメンバーを右クリックして、 **[クイック アクションとリファクタリング]**  >  **[\<*診断 ID*> の非表示]**  >  **[抑制ファイル内]** の順に選びます。

!["緑の波線が表示された WebClient API と左側の電球アイコンのスクリーンショット"](media/api-analyzer/suppress-in-sup-file.jpg)

最初の抑制後に、*GlobalSuppressions.cs* ファイルがプロジェクトに追加されます。 新しいグローバル抑制は、このファイルに追加されます。

!["緑の波線が表示された WebClient API と左側の電球アイコンのスクリーンショット"](media/api-analyzer/suppression-file.jpg)

グローバル抑制は、プロジェクト間で API 使用の一貫性を確保するのに推奨される方法です。

## <a name="discover-cross-platform-issues"></a>クロスプラットフォームの問題を検出する

非推奨の API と同様に、アナライザーはクロスプラットフォームではないすべての API を識別します。 たとえば、<xref:System.Console.WindowWidth?displayProperty=nameWithType> は Windows では動作しますが、Linux や macOS では動作しません。 診断 ID は、 **[エラー一覧]** ウィンドウに表示されます。 右クリックして **[クイック アクションとリファクタリング]** を選ぶことで、その警告を抑制することができます。 2 つのオプション (非推奨のメンバーを使い続けて警告を抑制するか、まったく使わない) がある非推奨の場合とは異なり、特定のプラットフォーム用にのみコードを開発している場合は、コードを実行する予定のない他のすべてのプラットフォームですべての警告を抑制できます。 そのために必要なことは、プロジェクト ファイルを編集し、無視するすべてのプラットフォームを列記した `PlatformCompatIgnore` プロパティを追加するだけです。 指定できる値は、`Linux`、`macOS`、`Windows` です。

```xml
<PropertyGroup>
    <PlatformCompatIgnore>Linux;macOS</PlatformCompatIgnore>
</PropertyGroup>
```

コードが複数のプラットフォームを対象にしていて、一部のプラットフォームでサポートされていない API を利用したい場合は、`if` ステートメントを使ってコードのその部分を保護できます。

```csharp
if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
{
     var w = Console.WindowWidth;
     // More code
}
```

また、対象のフレームワーク/オペレーティング システムごとに条件付きでコンパイルすることもできますが、現在は[手動で](../frameworks.md#how-to-specify-target-frameworks)行う必要があります。

## <a name="supported-diagnostics"></a>サポートされている診断

現時点では、アナライザーは次のケースを処理します。

- <xref:System.PlatformNotSupportedException> (PC001) をスローする .NET Standard API の使用。
- .NET Framework 4.6.1 (PC002) では利用できない .NET Standard API の使用。
- UWP (PC003) に存在しないネイティブ API の使用。
- Delegate.BeginInvoke および EndInvoke API (PC004) の使用。
- 非推奨 (DEXXXX) としてマークされている API の使用。

## <a name="ci-machine"></a>CI マシン

これらの診断はすべて、IDE で利用できるだけでなく、プロジェクトのビルドの一部としてコマンド ラインでも利用でき、これには CI サーバーが含まれます。

## <a name="configuration"></a>構成

ユーザーは、診断の処理方法を決定します (警告、エラー、提案、オフ)。 たとえば、設計者は、互換性の問題をエラーとして扱い、一部の非推奨の API の呼び出しでは警告を生成し、それ以外については提案を生成するだけにする、といったことを決定できます。 これを、診断 ID 別およびプロジェクト別に構成できます。 そのためには、**ソリューション エクスプローラー**で、プロジェクトの **[依存関係]** ノードに移動します。 ノード **[依存関係]**  >  **[アナライザー]**  >  **[Microsoft.DotNet.Analyzers.Compatibility]** を展開します。 診断 ID を右クリックし、 **[ルール セットの重要度を設定]** を選んで、目的のオプションを選びます。

!["診断とルール セットの重要度のポップアップ ダイアログが表示されているソリューション エクスプローラーのスクリーンショット"](media/api-analyzer/disable-notifications.jpg)

## <a name="see-also"></a>関連項目

- 「[Introducing API Analyzer](https://devblogs.microsoft.com/dotnet/introducing-api-analyzer/)」(API アナライザーの概要) のブログ投稿。
- YouTube の「[API Analyzer](https://youtu.be/eeBEahYXGd0)」(API アナライザー) デモ ビデオ。
