---
title: アセンブリの読み込みのベスト プラクティス
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies,binding
- LoadFrom method
- default load context
- TypeLoadException class,causes
- assembly loading errors
- load contexts
- assemblies,loading
- LoadWithPartialName method
- load-from context
ms.assetid: 68d1c539-6a47-4614-ab59-4b071c9d4b4c
ms.openlocfilehash: 7575c40edf47e977335bcc34fcd9e49debab0980
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181703"
---
# <a name="best-practices-for-assembly-loading"></a>アセンブリの読み込みのベスト プラクティス
ここでは、<xref:System.InvalidCastException>、<xref:System.MissingMethodException>、およびその他のエラーの原因となることがある型 ID の問題を回避する方法について説明します。 また、次の推奨事項について説明します。  
  
- [読み込みコンテキストのメリットとデメリットについて理解する](#load_contexts)  
  
- [部分的なアセンブリ名をバインドしない](#avoid_partial_names)  
  
- [1 つのアセンブリを複数のコンテキストに読み込まない](#avoid_loading_into_multiple_contexts)  
  
- [複数のバージョンのアセンブリを同じコンテキストに読み込まない](#avoid_loading_multiple_versions)  
  
- [既定の読み込みコンテキストへの切り替えを検討する](#switch_to_default)  
  
 最初の推奨事項である「[読み込みコンテキストのメリットとデメリットについて理解する](#load_contexts)」では、その他の推奨事項の背景情報を提供します。他の推奨事項を理解するには、読み込みコンテキストに関する知識が必要となるためです。  
  
<a name="load_contexts"></a>
## <a name="understand-the-advantages-and-disadvantages-of-load-contexts"></a>読み込みコンテキストの利点と欠点を理解する  
 アプリケーション ドメインでは、アセンブリは、3 つのコンテキストのいずれかに読み込まれるか、コンテキストなしで読み込まれる可能性があります。  
  
- 既定の読み込みコンテキストには、グローバル アセンブリ キャッシュを調査して見つかるアセンブリ、ランタイムが (SQL Server などで) ホストされる場合のホスト アセンブリ ストア、およびアプリケーション ドメインの <xref:System.AppDomainSetup.ApplicationBase%2A> と <xref:System.AppDomainSetup.PrivateBinPath%2A> が含まれます。 <xref:System.Reflection.Assembly.Load%2A> メソッドのほとんどのオーバーロードは、このコンテキストにアセンブリを読み込みます。  
  
- 読み込み元コンテキストには、ローダーによって検索されない場所から読み込まれたアセンブリが含まれます。 たとえば、アドインが、アプリケーション パスではないディレクトリにインストールされることがあります。 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>、<xref:System.AppDomain.CreateInstanceFrom%2A?displayProperty=nameWithType>、<xref:System.AppDomain.ExecuteAssembly%2A?displayProperty=nameWithType> は、パスを使用して読み込むメソッドの例です。  
  
- リフレクション専用コンテキストには、<xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> メソッドと <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> メソッドを使用して読み込まれたアセンブリが含まれます。 このコンテキスト内のコードは実行できないため、ここでは説明しません。 詳細については、「[方法: リフレクションのみのコンテキストにアセンブリを読み込む](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)」を参照してください。  
  
- リフレクション出力を使用して一時動的アセンブリを出力した場合、そのアセンブリはどのコンテキストにも含まれません。 また、<xref:System.Reflection.Assembly.LoadFile%2A> メソッドを使用して読み込まれる大部分のアセンブリは、コンテキストなしで読み込まれます。バイト配列から読み込まれるアセンブリは、(ポリシーが適用された後の) アセンブリの ID がグローバル アセンブリ キャッシュ内に存在するように確立される場合を除き、コンテキストなしで読み込まれます。  
  
 この実行コンテキストには、次のセクションで説明するようなメリットとデメリットがあります。  
  
### <a name="default-load-context"></a>既定の読み込みコンテキスト  
 アセンブリが既定の読み込みコンテキストに読み込まれると、その依存関係が自動的に読み込まれます。 既定の読み込みコンテキストに読み込まれた依存関係は、既定の読み込みコンテキストまたは読み込み元コンテキストにあるアセンブリに対して自動的に検索されます。 アセンブリ ID による読み込みを行うと、バージョンが不明なアセンブリが使用されなくなるため、アプリケーションの安定性が向上します (「[部分的なアセンブリ名をバインドしない](#avoid_partial_names)」を参照してください)。  
  
 既定の読み込みコンテキストを使用すると、次のようなデメリットが生じます。  
  
- 他のコンテキストに読み込まれた依存関係は使用できません。  
  
- プローブ パスの外部の場所にあるアセンブリを、既定の読み込みコンテキストに読み込むことはできません。  
  
### <a name="load-from-context"></a>読み込み元コンテキスト  
 読み込み元コンテキストでは、アプリケーション パスの下にないために調査には含まれないパスから、アセンブリを読み込むことができます。 パス情報はコンテキストによって維持されるため、そのパスから依存関係を見つけて読み込むことができるようになります。 また、このコンテキスト内にあるアセンブリでは、既定の読み込みコンテキストに読み込まれた依存関係を使用できます。  
  
 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> メソッド、またはパスから読み込む他のいずれかのメソッドを使用してアセンブリを読み込むと、次のようなデメリットが生じます。  
  
- 同じ ID を持つアセンブリが既に読み込まれている場合、<xref:System.Reflection.Assembly.LoadFrom%2A> は、別のパスが指定されている場合であっても、読み込み済みのアセンブリを返します。  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A> を使用してアセンブリを読み込んだ後、同じアセンブリを、既定の読み込みコンテキストにあるアセンブリが表示名で読み込もうとすると、読み込みが失敗します。 この問題は、アセンブリが逆シリアル化されるときに発生します。  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A> を使用してアセンブリを読み込み、ID が同じで場所が異なるアセンブリをプローブ パスに含めると、<xref:System.InvalidCastException>、<xref:System.MissingMethodException>、またはその他の予測できない動作が発生します。  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A> には、指定したパスに対する <xref:System.Security.Permissions.FileIOPermissionAccess.Read?displayProperty=nameWithType> および <xref:System.Security.Permissions.FileIOPermissionAccess.PathDiscovery?displayProperty=nameWithType>、または <xref:System.Net.WebPermission> が必要です。  
  
- アセンブリにネイティブ イメージが存在する場合、それは使用されません。  
  
- アセンブリをドメイン中立として読み込むことはできません。  
  
- .NET Framework Version 1.0 および 1.1 では、ポリシーが適用されません。  
  
### <a name="no-context"></a>コンテキストなし  
 コンテキストなしでの読み込みは、リフレクション出力で作成される一時アセンブリに対する唯一のオプションです。 1 つのアプリケーション ドメイン内に同じ ID を持つ複数のアセンブリを読み込むには、コンテキストなしでの読み込みを行うしか方法がありません。 調査のコストは回避されます。  
  
 バイト配列から読み込まれるアセンブリは、ポリシーの適用時に確立されたアセンブリの ID がグローバル アセンブリ キャッシュ内のアセンブリの ID と一致する場合を除き、コンテキストなしで読み込まれます。この場合、アセンブリはグローバル アセンブリ キャッシュから読み込まれます。  
  
 コンテキストなしでアセンブリを読み込むと、次のようなデメリットが生じます。  
  
- <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントを処理しない限り、他のアセンブリが、コンテキストなしで読み込まれたアセンブリにバインドすることはできません。  
  
- 依存関係は自動的に読み込まれません。 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントを処理すると、コンテキストなしでのプリロード、既定の読み込みコンテキストへのプリロード、または読み込みを行うことができます。  
  
- 同じ ID を持つ複数のアセンブリをコンテキストなしで読み込むと、同じ ID を持つアセンブリを複数のコンテキストに読み込む場合と同様の型 ID の問題が発生することがあります。 「[1 つのアセンブリを複数のコンテキストに読み込まない](#avoid_loading_into_multiple_contexts)」を参照してください。  
  
- アセンブリにネイティブ イメージが存在する場合、それは使用されません。  
  
- アセンブリをドメイン中立として読み込むことはできません。  
  
- .NET Framework Version 1.0 および 1.1 では、ポリシーが適用されません。  
  
<a name="avoid_partial_names"></a>
## <a name="avoid-binding-on-partial-assembly-names"></a>部分的なアセンブリ名をバインドしない  
 アセンブリを読み込むときにアセンブリの表示名 (<xref:System.Reflection.Assembly.FullName%2A>) の一部だけを指定すると、部分的な名前のバインディングが行われます。 たとえば、<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドを呼び出すときに、アセンブリのバージョン、カルチャ、および公開キー トークンを省略して、簡易名だけを指定することがあります。 または、<xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> メソッドを呼び出すこともあります。このメソッドは、最初に <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドを呼び出しますが、それでアセンブリが見つからなかった場合、グローバル アセンブリ キャッシュを検索し、使用可能な最新バージョンのアセンブリを読み込みます。  
  
 部分的な名前のバインドを使用すると、次のような多数の問題が発生することがあります。  
  
- <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> メソッドが、同じ簡易名を持つ異なるアセンブリを読み込む可能性があります。 たとえば、2 つのアプリケーションによって、`GraphicsLibrary` という簡易名を持つ 2 つのまったく異なるアセンブリがグローバル アセンブリ キャッシュにインストールされることがあります。  
  
- 実際に読み込まれるアセンブリには、下位互換性がない場合があります。 たとえば、バージョンを指定しないと、プログラムの作成時に使用した元のバージョンよりも、ずっと後にリリースされたバージョンが読み込まれることがあります。 新しいバージョンに加えられている変更が原因で、アプリケーションでエラーが発生することがあります。  
  
- 実際に読み込まれるアセンブリには、上位互換性がない場合があります。 たとえば、最新バージョンのアセンブリを使用してアプリケーションをビルドおよびテストしても、部分的なバインドにより、アプリケーションが使用する機能を備えていない、ずっと以前のバージョンのアセンブリが読み込まれることがあります。  
  
- 新しいアプリケーションをインストールすることで、既存のアプリケーションが破損する可能性があります。 <xref:System.Reflection.Assembly.LoadWithPartialName%2A> メソッドを使用するアプリケーションは、より新しい、互換性のないバージョンの共有アセンブリのインストールによって破損することがあります。  
  
- 予期しない依存関係の読み込みが発生する可能性があります。 依存関係を共有する 2 つのアセンブリを部分的なバインドによって読み込むと、一方のアセンブリが、ビルドやテストで使われたものではないコンポーネントを使用する結果になることがあります。  
  
 このような問題の原因となる可能性から、<xref:System.Reflection.Assembly.LoadWithPartialName%2A> メソッドは、現在は使用されない形式として設定されています。 代わりに <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドを使用して、アセンブリの完全な表示名を指定することをお勧めします。 「[読み込みコンテキストのメリットとデメリットについて理解する](#load_contexts)」および「[既定の読み込みコンテキストへの切り替えを検討する](#switch_to_default)」を参照してください。  
  
 アセンブリの読み込みが簡単になるという理由で <xref:System.Reflection.Assembly.LoadWithPartialName%2A> メソッドを使用する場合は、アセンブリが見つからないことを示すエラー メッセージを表示してアプリケーションを終了する方が、未知のバージョンのアセンブリを自動的に使用するよりも望ましいユーザー エクスペリエンスになる可能性が高い点を考慮してください。未知のバージョンを使用すると、予測できない動作やセキュリティ ホールを引き起こす可能性があります。  
  
<a name="avoid_loading_into_multiple_contexts"></a>
## <a name="avoid-loading-an-assembly-into-multiple-contexts"></a>1 つのアセンブリを複数のコンテキストに読み込まない  
 1 つのアセンブリを複数のコンテキストに読み込むと、型 ID の問題が引き起こされることがあります。 2 つの異なるコンテキストに同じアセンブリから同じ型が読み込まれると、同じ名前を持つ 2 つの異なる型が読み込まれたような状態になります。 一方の型を別の型にキャストしようとした場合、<xref:System.InvalidCastException> がスローされ、型 `MyType` を型 `MyType` にキャストできないという混乱を招くメッセージが表示されます。  
  
 たとえば、`ICommunicate` という名前のアセンブリで `Utility` インターフェイスが宣言されているとします。このアセンブリは、ユーザーのプログラムで参照され、そのプログラムが読み込む他のアセンブリによっても参照されています。 これらの他のアセンブリには、`ICommunicate` インターフェイスを実装する型が含まれ、ユーザーのプログラムで使用できるようになっています。  
  
 では、このプログラムを実行すると何が起きるでしょうか。 プログラムで参照されているアセンブリは、既定の読み込みコンテキストに読み込まれます。 <xref:System.Reflection.Assembly.Load%2A> メソッドを使用し、ID を指定してターゲット アセンブリを読み込んだ場合は、アセンブリが既定の読み込みコンテキストに配置され、その依存関係も同様に扱われます。 プログラムとターゲット アセンブリは、どちらも同じ `Utility` アセンブリを使用することになります。  
  
 ただし、ここでは、<xref:System.Reflection.Assembly.LoadFile%2A> メソッドを使用し、ファイル パスを指定してターゲット アセンブリを読み込む場合を想定します。 アセンブリはコンテキストなしで読み込まれるため、その依存関係は自動的には読み込まれません。 そこで、<xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベント ハンドラーを使用して依存関係を指定し、`Utility` メソッドを使用して、コンテキストなしで <xref:System.Reflection.Assembly.LoadFile%2A> アセンブリを読み込むことにします。 この場合、ターゲット アセンブリに含まれる型のインスタンスを作成し、そのインスタンスを型 `ICommunicate` の変数に割り当てようとすると、<xref:System.InvalidCastException> がスローされます。ランタイムでは、`ICommunicate` アセンブリの 2 つのコピーに含まれている `Utility` インターフェイスが、別々の型であると見なされるためです。  
  
 1 つのアセンブリが複数のコンテキストに読み込まれるシナリオは、これ以外にも数多くあります。 このような競合を回避する最善の方法は、ターゲット アセンブリをアプリケーション パスに再配置し、完全な表示名を指定して <xref:System.Reflection.Assembly.Load%2A> メソッドを使用することです。 これにより、アセンブリが既定の読み込みコンテキストに読み込まれ、どちらのアセンブリも同じ `Utility` アセンブリを使用することになります。  
  
 ターゲット アセンブリをアプリケーション パスの外部に配置する必要がある場合は、<xref:System.Reflection.Assembly.LoadFrom%2A> メソッドを使用して、読み込み元コンテキストに読み込むことができます。 アプリケーションの `Utility` アセンブリへの参照を使用してコンパイルされたターゲット アセンブリでは、アプリケーションが既定の読み込みコンテキストに読み込んだ `Utility` アセンブリが使用されます。 アプリケーション パスの外部に配置された `Utility` アセンブリのコピーに対してターゲット アセンブリが依存関係を持っている場合は、問題が発生する可能性があることに注意が必要です。 アプリケーションが `Utility` アセンブリを読み込む前に、このような依存関係を持つアセンブリが読み込み元コンテキストに読み込まれると、アプリケーションの読み込みは失敗します。  
  
 [ や ](#switch_to_default) などのファイル パスからの読み込みに代わる代替手段については、「<xref:System.Reflection.Assembly.LoadFile%2A>既定の読み込みコンテキストへの切り替えを検討する<xref:System.Reflection.Assembly.LoadFrom%2A>」で説明します。  
  
<a name="avoid_loading_multiple_versions"></a>
## <a name="avoid-loading-multiple-versions-of-an-assembly-into-the-same-context"></a>複数のバージョンのアセンブリを同じコンテキストに読み込まない  
 複数のバージョンのアセンブリを 1 つの読み込みコンテキストに読み込むと、型 ID の問題が引き起こされることがあります。 同じアセンブリの 2 つのバージョンから同じ型が読み込まれると、同じ名前を持つ 2 つの異なる型が読み込まれたような状態になります。 一方の型を別の型にキャストしようとした場合、<xref:System.InvalidCastException> がスローされ、型 `MyType` を型 `MyType` にキャストできないという混乱を招くメッセージが表示されます。  
  
 たとえば、ユーザーのプログラムで、あるバージョンの `Utility` アセンブリを直接読み込み、その後で別のアセンブリを読み込むとします。この別のアセンブリによって、異なるバージョンの `Utility` アセンブリが読み込まれることがあります。 または、コーディング エラーが原因で、アプリケーション内に 2 つの異なるコード パスが生じ、異なるバージョンのアセンブリが読み込まれることも考えられます。  
  
 既定の読み込みコンテキストでは、<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドを使用し、異なるバージョン番号を含むアセンブリの完全な表示名を指定した場合に、この問題が発生することがあります。 コンテキストなしで読み込まれたアセンブリでは、<xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType> メソッドを使用して別々のパスから同じアセンブリを読み込んだ場合に、この問題が発生することがあります。 ランタイムでは、異なるパスから読み込まれた 2 つのアセンブリは、たとえその ID が同一であっても、別々のアセンブリであると見なされます。  
  
 複数のバージョンのアセンブリが存在すると、型 ID の問題だけでなく、<xref:System.MissingMethodException> が発生することもあります。これは、あるバージョンのアセンブリから読み込まれた型が、それとは異なるバージョンの型を必要とするコードに渡された場合に発生します。 たとえば、より新しいバージョンに追加されたメソッドが、コードで必要とされている場合が考えられます。  
  
 型の動作がバージョン間で変更されていると、明確でないエラーが発生することもあります。 たとえば、メソッドから予期しない例外がスローされたり、予期しない値が返されたりすることがあります。  
  
 コードをよく調べ、読み込まれるアセンブリのバージョンが 1 つだけであることを確認してください。 特定の時点で読み込まれているアセンブリを確認するには、<xref:System.AppDomain.GetAssemblies%2A?displayProperty=nameWithType> メソッドを使用できます。  
  
<a name="switch_to_default"></a>
## <a name="consider-switching-to-the-default-load-context"></a>既定の読み込みコンテキストへの切り替えを検討する  
 アプリケーションのアセンブリの読み込みと配置のパターンを調査してください。 バイト配列から読み込まれるアセンブリを取り除くことができるかどうかを検討します。 アセンブリをプローブ パスに移動できるかどうかを検討します。 アセンブリがグローバル アセンブリ キャッシュにある場合、またはアプリケーション ドメインのプローブ パス (つまり、アプリケーション ドメインの <xref:System.AppDomainSetup.ApplicationBase%2A> および <xref:System.AppDomainSetup.PrivateBinPath%2A>) にある場合は、ID を指定してアセンブリを読み込むことができます。  
  
 すべてのアセンブリをプローブ パスに置くことができない場合は、.NET Framework アドイン モデルの使用、アセンブリのグローバル アセンブリ キャッシュ内への配置、アプリケーション ドメインの作成などの代替手段を検討してください。  
  
### <a name="consider-using-the-net-framework-add-in-model"></a>.NET アドイン モデルの使用を検討する  
 読み込み元コンテキストを使用してアドインを実装している場合 (通常、これらはアプリケーション ベースにはインストールされません)、.NET Framework アドイン モデルを使用してください。 このモデルでは、アプリケーション ドメインまたはプロセス レベルでの分離が提供されます。アプリケーション ドメインを管理する必要はありません。 アドイン モデルの詳細については、「[アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))」を参照してください。  
  
### <a name="consider-using-the-global-assembly-cache"></a>グローバル アセンブリ キャッシュの使用を検討する  
 アセンブリをグローバル アセンブリ キャッシュに配置すると、アプリケーション ベースの外部にある共有アセンブリ パスの利点を活用できます。既定の読み込みコンテキストのメリットが損なわれたり、その他のコンテキストのデメリットが生じたりすることはありません。  
  
### <a name="consider-using-application-domains"></a>アプリケーション ドメインの使用を検討する  
 アプリケーションのプローブ パスに配置できないアセンブリがあると判断される場合は、それらのアセンブリに対して新しいアプリケーション ドメインを作成することを検討してください。 <xref:System.AppDomainSetup> を使用して新しいアプリケーション ドメインを作成し、<xref:System.AppDomainSetup.ApplicationBase%2A?displayProperty=nameWithType> プロパティを使用して読み込むアセンブリのあるパスを指定します。 調査するディレクトリが複数ある場合は、<xref:System.AppDomainSetup.ApplicationBase%2A> をルート ディレクトリに設定し、<xref:System.AppDomainSetup.PrivateBinPath%2A?displayProperty=nameWithType> プロパティを使用して調査するサブディレクトリを指定します。 または、複数のアプリケーション ドメインを作成し、各アプリケーション ドメインの <xref:System.AppDomainSetup.ApplicationBase%2A> を、そのアセンブリの適切なパスに設定することもできます。  
  
 これらのアセンブリは、<xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> メソッドを使用して読み込むことができます。 これらはプローブ パスに存在することになるので、読み込み元コンテキストからではなく、既定の読み込みコンテキストに読み込まれるようになります。 ただし、<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドに切り替えて、常に最新のバージョンが使用されるようにアセンブリの完全な表示名を指定することをお勧めします。  
  
## <a name="see-also"></a>参照

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType>
- <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType>
