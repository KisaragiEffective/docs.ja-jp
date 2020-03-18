---
title: マネージド アセンブリの読み込みアルゴリズム - .NET Core
description: .NET Core でのマネージド アセンブリ読み込みアルゴリズムの詳細の説明
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 312a320676be6eb453697e0704ab771a6707618b
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73973509"
---
# <a name="managed-assembly-loading-algorithm"></a>マネージド アセンブリの読み込みアルゴリズム

マネージド アセンブリは、さまざまな段階を伴うアルゴリズムを使用して検出され、読み込まれます。

サテライト アセンブリと `WinRT` アセンブリを除くすべてのマネージド アセンブリで同じアルゴリズムが使用されます。

## <a name="when-are-managed-assemblies-loaded"></a>マネージド アセンブリが読み込まれるタイミング

マネージド アセンブリの読み込みをトリガーするための最も一般的なメカニズムは、静的アセンブリ参照です。 これらの参照は、コードが別のアセンブリで定義されている型を使用するときに、コンパイラによって挿入されます。 これらのアセンブリは、ランタイムによって必要に応じて読み込まれます (`load-by-name`)。

特定の API を直接使用によっても、読み込みがトリガーされます。

|API  |説明  |`Active` <xref:System.Runtime.Loader.AssemblyLoadContext> |
|---------|---------|---------|
|<xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromAssemblyName%2A?displayProperty=nameWithType>|`Load-by-name`|[this](../../csharp/language-reference/keywords/this.md) インスタンス|
|<xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromAssemblyPath%2A?displayProperty=nameWithType><p><xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromNativeImagePath%2A?displayProperty=nameWithType>|パスから読み込みます。|[this](../../csharp/language-reference/keywords/this.md) インスタンス|
<xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromStream%2A?displayProperty=nameWithType>|オブジェクトから読み込みます。|[this](../../csharp/language-reference/keywords/this.md) インスタンス|
|<xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType>|新しい <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスにパスから読み込みます。|新しい <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンス。|
<xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>|新しい <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> インスタンスにパスから読み込みます。<p><xref:System.Runtime.Loader.AssemblyLoadContext.Resolving> ハンドラーを <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> に追加します。 ハンドラーは、そのディレクトリからアセンブリの依存関係を読み込みます。|<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> インスタンス。|
|<xref:System.Reflection.Assembly.Load(System.Reflection.AssemblyName)?displayProperty=nameWithType><p><xref:System.Reflection.Assembly.Load(System.String)?displayProperty=nameWithType><p><xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType>|`Load-by-name`。|呼び出し元から推論されます。<p><xref:System.Runtime.Loader.AssemblyLoadContext> メソッドが優先されます。|
|<xref:System.Reflection.Assembly.Load(System.Byte[])?displayProperty=nameWithType><p><xref:System.Reflection.Assembly.Load(System.Byte[],System.Byte[])?displayProperty=nameWithType>|新しい <xref:System.Runtime.Loader.AssemblyLoadContext>インスタンスのオブジェクトから読み込みます。|新しい <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンス。|
<xref:System.Type.GetType(System.String)?displayProperty=nameWithType><p><xref:System.Type.GetType(System.String,System.Boolean)?displayProperty=nameWithType><p><xref:System.Type.GetType(System.String,System.Boolean,System.Boolean)?displayProperty=nameWithType>|`Load-by-name`。|呼び出し元から推論されます。<p>`assemblyResolver` 引数を使用した <xref:System.Type.GetType%2A?displayProperty=nameWithType> メソッドが優先されます。|
<xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType>|型 `name` がアセンブリ修飾ジェネリック型を記述している場合は、`Load-by-name` をトリガーします。|呼び出し元から推論されます。<p>アセンブリ修飾型名を使用する場合は、<xref:System.Type.GetType%2A?displayProperty=nameWithType> が優先されます。|
<xref:System.Activator.CreateInstance(System.String,System.String)?displayProperty=nameWithType><p><xref:System.Activator.CreateInstance(System.String,System.String,System.Object[])?displayProperty=nameWithType><p><xref:System.Activator.CreateInstance(System.String,System.String,System.Boolean,System.Reflection.BindingFlags,System.Reflection.Binder,System.Object[],System.Globalization.CultureInfo,System.Object[])?displayProperty=nameWithType>|`Load-by-name`。|呼び出し元から推論されます。<p><xref:System.Type> 引数を取る <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> メソッドが優先されます。|

## <a name="algorithm"></a>アルゴリズム

次のアルゴリズムでは、ランタイムがマネージド アセンブリを読み込む方法について説明します。

1. `active` <xref:System.Runtime.Loader.AssemblyLoadContext> を決定します。

    - 静的アセンブリ参照の場合、`active` <xref:System.Runtime.Loader.AssemblyLoadContext> は、参照元のアセンブリを読み込んだインスタンスです。
    - 優先 API の場合、`active` <xref:System.Runtime.Loader.AssemblyLoadContext> は明示されます。
    - その他の API の場合、`active` <xref:System.Runtime.Loader.AssemblyLoadContext> は推論されます。 これらの API では、<xref:System.Runtime.Loader.AssemblyLoadContext.CurrentContextualReflectionContext?displayProperty=nameWithType> プロパティが使用されます。 その値が `null` 場合、推論される <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスが使用されます。
    - 上記の表を参照してください。

2. `Load-by-name` メソッドでは、アクティブな <xref:System.Runtime.Loader.AssemblyLoadContext> によってアセンブリが読み込まれます。 優先順位は次のとおりです。
    - `cache-by-name` を検査します。

    - <xref:System.Runtime.Loader.AssemblyLoadContext.Load%2A?displayProperty=nameWithType> 関数を呼び出します。

    - <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> インスタンスのキャッシュを確認し、[マネージド アセンブリの既定のプローブ](default-probing.md#managed-assembly-default-probing) ロジックを実行します。

    - アクティブな AssemblyLoadContext の <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> イベントを発生させます。

    - <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントを発生させます。

3. その他の種類の読み込みでは、`active` <xref:System.Runtime.Loader.AssemblyLoadContext> がアセンブリを読み込みます。 優先順位は次のとおりです。
    - `cache-by-name` を検査します。

    - 指定されたパスまたは未加工のアセンブリ オブジェクトから読み込みます。

4. どちらの場合も、アセンブリが新しく読み込まれた場合は、次のようになります。
   - <xref:System.AppDomain.AssemblyLoad?displayProperty=nameWithType> イベントが発生します。
   - アセンブリの <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスの `cache-by-name` に参照が追加されます。

5. アセンブリが見つかった場合は、必要に応じて参照が `active` <xref:System.Runtime.Loader.AssemblyLoadContext> インスタンスの `cache-by-name` に追加されます。
