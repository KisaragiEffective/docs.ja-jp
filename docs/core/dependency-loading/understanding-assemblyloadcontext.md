---
title: AssemblyLoadContext について - .NET Core
description: .NET Core での AssemblyLoadContext の用途と動作を理解するための主要な概念。
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 8a73a432bf8cc72cced77cf6c62a785b72032913
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72303708"
---
# <a name="understanding-systemruntimeloaderassemblyloadcontext"></a>System.Runtime.Loader.AssemblyLoadContext について

<xref:System.Runtime.Loader.AssemblyLoadContext> クラスは .NET Core に固有です。 この記事では、<xref:System.Runtime.Loader.AssemblyLoadContext> API ドキュメントを概念情報により補完します。

この記事は、動的読み込みを実装する開発者 (特に動的読み込みフレームワークの開発者) に関連しています。

## <a name="what-is-the-assemblyloadcontext"></a>AssemblyLoadContext とは

すべての .NET Core アプリケーションは、暗黙的に <xref:System.Runtime.Loader.AssemblyLoadContext> を使用します。
これは、依存関係を検索して読み込むためのランタイムのプロバイダーです。 依存関係が読み込まれるたびに、<xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスが呼び出されてそれを見つけます。

- マネージド アセンブリやその他の依存関係を検索し、読み込み、キャッシュするサービスを提供します。

- 動的なコードの読み込みとアンロードをサポートするために、独自の <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスにコードとその依存関係を読み込むための分離コンテキストを作成します。

## <a name="when-do-you-need-multiple-assemblyloadcontext-instances"></a>複数の AssemblyLoadContext インスタンスが必要な場合

1 つの <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスは、単純なアセンブリ名 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> につき 1 つのバージョンの <xref:System.Reflection.Assembly> のみを読み込むように制限されています。

この制限は、コード モジュールを動的に読み込むときに問題になることがあります。 各モジュールは個別にコンパイルされ、<xref:System.Reflection.Assembly> の異なるバージョンに依存する場合があります。 この問題は、一般的に使用されるライブラリの異なるバージョンに異なるモジュールが依存している場合によく発生します。

動的なコード読み込みをサポートするために、<xref:System.Runtime.Loader.AssemblyLoadContext> API を使用して、<xref:System.Reflection.Assembly> の競合するバージョンを同じアプリケーション内に読み込むことができます。 各 <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスは、各 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> を特定の <xref:System.Reflection.Assembly> インスタンスにマップする一意のディクショナリを提供します。

また、後でアンロードするために、あるコード モジュールに関連する依存関係をグループ化するための便利なメカニズムも提供します。

## <a name="what-is-special-about-the-assemblyloadcontextdefault-instance"></a>AssemblyLoadContext.Default インスタンスの特別な点

<xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> インスタンスは、起動時にランタイムによって自動的に設定されます。  [既定のプローブ](default-probing.md)を使用して、すべての静的な依存関係を特定し、検索します。

最も一般的な依存関係読み込みのシナリオを解決します。

## <a name="how-does-assemblyloadcontext-support-dynamic-dependencies"></a>AssemblyLoadContext は動的な依存関係をどのようにサポートするか

<xref:System.Runtime.Loader.AssemblyLoadContext> には、オーバーライドできるさまざまなイベントと仮想関数があります。

<xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> インスタンスでは、イベントのオーバーライドのみがサポートされます。

「[マネージド アセンブリの読み込みアルゴリズム](loading-managed.md)」、「[サテライト アセンブリの読み込みアルゴリズム](loading-resources.md)」、「[アンマネージド (ネイティブ) ライブラリの読み込みアルゴリズム](loading-unmanaged.md)」の各記事では、使用可能なすべてのイベントと仮想関数を参照しています。  これらの記事では、読み込みアルゴリズムでの各イベントと関数の相対位置を示しています。 この記事では、その情報は繰り返しません。

ここでは、関連するイベントおよび関数の一般原則について説明します。

- **反復可能にする**。 特定の依存関係に対するクエリは、常に同じ応答になる必要があります。 同じ読み込み済みの依存関係インスタンスを返す必要があります。 この要件は、キャッシュの整合性の基本となります。 特にマネージド アセンブリについては、<xref:System.Reflection.Assembly> キャッシュが作成されます。 キャッシュ キーは、単純なアセンブリ名 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> です。
- **通常はスローしない**。  これらの関数は、要求された依存関係を見つけることができない場合、スローするのではなく `null` を返すことが想定されています。 スローすると、検索が途中で終了し、例外が呼び出し元に伝達されます。 スローは、破損したアセンブリやメモリ不足の状態など、予期しないエラーに限定する必要があります。
- **再帰を回避する**。 これらの関数とハンドラーは、依存関係を検索するための読み込みルールを実装することに注意してください。 実装では、再帰をトリガーする API を呼び出さないでください。 コードでは、通常、特定のパスまたはメモリ参照の引数を必要とする **AssemblyLoadContext**  読み込み関数を呼び出す必要があります。
- **正しい AssemblyLoadContext に読み込む**。 依存関係を読み込む場所の選択は、アプリケーション固有です。  この選択は、これらのイベントと関数によって実装されます。 コードによって **AssemblyLoadContext** のパスによる読み込み関数を呼び出すときは、コードを読み込むインスタンス上でそれらを呼び出します。 `null` を返し、<xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> により読み込みを処理させるのが最も単純なオプションの場合があります。
- **スレッドの競合に注意する**。 読み込みは複数のスレッドによってトリガーできます。 AssemblyLoadContext は、キャッシュにアセンブリをアトミックに追加することで、スレッドの競合を処理します。 競合の敗者のインスタンスは破棄されます。 実装ロジックでは、複数のスレッドを適切に処理しない余分なロジックを追加しないでください。

## <a name="how-are-dynamic-dependencies-isolated"></a>動的な依存関係はどのように分離されるか

各 <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスは、<xref:System.Reflection.Assembly> インスタンスと <xref:System.Type> 定義の一意のスコープを表します。

これらの依存関係の間にバイナリの分離はありません。 名前によって互いを検索しないことによってのみ分離されています。

各 <xref:System.Runtime.Loader.AssemblyLoadContext> は次のようになります。

- <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> が別の <xref:System.Reflection.Assembly> インスタンスを参照している可能性があります。
- <xref:System.Type.GetType%2A?displayProperty=nameWithType> が、同じ型の `name` に対して異なる型のインスタンスを返す場合があります。

## <a name="how-are-dependencies-shared"></a>依存関係はどのように共有されるか

依存関係は、<xref:System.Runtime.Loader.AssemblyLoadContext> インスタンス間で簡単に共有できます。 一般的なモデルは、1 つの <xref:System.Runtime.Loader.AssemblyLoadContext> が依存関係を読み込むことです。  もう 1 つは、読み込まれたアセンブリへの参照を使用して依存関係を共有します。

この共有は、ランタイム アセンブリに必要です。 これらのアセンブリは、<xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> にのみ読み込むことができます。 `ASP.NET`、`WPF`、`WinForms` などのフレームワークでも同じことが必要です。

共有の依存関係は <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> に読み込むことをお勧めします。 この共有は、一般的な設計パターンです。

共有は、カスタム <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスのコーディングで実装されます。 <xref:System.Runtime.Loader.AssemblyLoadContext> には、オーバーライドできるさまざまなイベントと仮想関数があります。 これらの関数のいずれかが、別の <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスに読み込まれた <xref:System.Reflection.Assembly> インスタンスへの参照を返すと、<xref:System.Reflection.Assembly> インスタンスが共有されます。 標準読み込みアルゴリズムは、一般的な共有パターンを簡略化するために、読み込みを <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> に任せます。  「[マネージド アセンブリの読み込みアルゴリズム](loading-managed.md)」を参照してください。

## <a name="complications"></a>問題点

### <a name="type-conversion-issues"></a>型変換の問題

2 つの <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスに同じ `name` の型定義が含まれている場合、それらは同じ型ではありません。 それらが同じ型になるのは、それらが同じ <xref:System.Reflection.Assembly> インスタンスからのものである場合に限ります。

さらに問題が複雑なのは、これらの一致しない型に関する例外メッセージが分かりにくい場合があることです。 型は、例外メッセージの中で単純型の名前によって参照されます。 この場合の一般的な例外メッセージは、次の形式になります。

> 型 'IsolatedType' のオブジェクトを型 'IsolatedType' に変換できません。

### <a name="debugging-type-conversion-issues"></a>型変換の問題のデバッグ

一致しない型のペアが存在する場合、次の点にも注意する必要があります。

- 各型の <xref:System.Type.Assembly?displayProperty=nameWithType>
- 各型の <xref:System.Runtime.Loader.AssemblyLoadContext>。これは <xref:System.Runtime.Loader.AssemblyLoadContext.GetLoadContext(System.Reflection.Assembly)?displayProperty=nameWithType> 関数を使用して取得できます。

`a` と `b` の 2 つのオブジェクトがある場合、デバッガーで次のものを評価すると便利です。

```csharp
// In debugger look at each assembly's instance, Location, and FullName
a.GetType().Assembly
b.GetType().Assembly
// In debugger look at each AssemblyLoadContext's instance and name
System.Runtime.AssemblyLoadContext.GetLoadContext(a.GetType().Assembly)
System.Runtime.AssemblyLoadContext.GetLoadContext(b.GetType().Assembly)
```

### <a name="resolving-type-conversion-issues"></a>型変換の問題の解決

このような型変換の問題を解決するには、2 つの設計パターンがあります。

1. 一般的な共有型を使用します。 この共有型は、プリミティブなランタイム型にすることも、共有アセンブリに新しい共有型を作成することもできます。  多くの場合、共有型はアプリケーション アセンブリで定義された [interface ](../../csharp/language-reference/keywords/interface.md) です。 参照:[依存関係はどのように共有されるか](#how-are-dependencies-shared)。

2. マーシャリング技法を使用して、ある型から別の型に変換します。
