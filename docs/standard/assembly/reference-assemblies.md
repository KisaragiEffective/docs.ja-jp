---
title: 参照アセンブリ
description: 参照アセンブリについて説明します。これはライブラリのパブリック API サーフェイスのみを含む .NET の特殊なアセンブリです。
author: MSDN-WhiteKnight
ms.date: 09/12/2019
ms.technology: dotnet-standard
ms.openlocfilehash: 3b85e51a015cca1e53ee2503c7bfa58c504fc718
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78156466"
---
# <a name="reference-assemblies"></a>参照アセンブリ

"*参照アセンブリ*" は、ライブラリのパブリック API サーフェイスを表すために必要最小限のメタデータのみを含む特殊なアセンブリです。 これには、ビルド ツールでアセンブリを参照するときに重要なすべてのメンバーの宣言が含まれます。ただし、すべてのメンバーの実装と、その API コントラクトに影響を与えないプライベート メンバーの宣言は除外されます。 これに対して、通常のアセンブリは "*実装アセンブリ*" と呼ばれます。

参照アセンブリを実行用に読み込むことはできませんが、実装アセンブリと同じ方法でコンパイラの入力として渡すことができます。 参照アセンブリは、通常、特定のプラットフォームまたはライブラリのソフトウェア開発キット (SDK) と共に配布されます。

開発者は、参照アセンブリを使用すると、そのバージョンの完全な実装アセンブリを持たなくても、特定のライブラリ バージョンを対象とするプログラムをビルドできます。 たとえば、マシンに最新バージョンのライブラリのみがインストールされ、ライブラリの以前のバージョンを対象とするプログラムを構築するとします。 実装アセンブリに対して直接コンパイルする場合、以前のバージョンでは使用できない API メンバーを誤って使用する可能性があります。 この誤りは、ターゲット マシンでプログラムをテストするときにのみ見つかります。 以前のバージョンの参照アセンブリに対してコンパイルすると、すぐにコンパイル時エラーが発生します。

参照アセンブリでは、コントラクト、つまり具象実装アセンブリに対応しない API のセットを表すことができます。 "*コントラクト アセンブリ*" と呼ばれるこのような参照アセンブリを使用すると、同じ API のセットをサポートする複数のプラットフォームを対象にすることができます。 たとえば、.NET Standard には、複数の .NET プラットフォーム間で共有される共通 API のセットを表すコントラクト アセンブリ *netstandard.dll* が用意されています。 このような API の実装は、.NET Framework 上の *mscorlib.dll* や .NET Core 上の *System.Private.CoreLib.dll* など、さまざまなプラットフォーム上のさまざまなアセンブリに含まれています。 .NET Standard をターゲットとするライブラリは、.NET Standard をサポートするすべてのプラットフォームで実行できます。

## <a name="using-reference-assemblies"></a>参照アセンブリの使用

プロジェクトから特定の API を使用するには、アセンブリへの参照を追加する必要があります。 参照は、実装アセンブリに追加するか、参照アセンブリに追加できます。 使用可能な場合は常に参照アセンブリを使用することをお勧めします。 これにより、API デザイナーによる使用を意図されている、ターゲット バージョンでサポートされている API メンバーのみが使用されるようになります。 参照アセンブリを使用すると、確実に実装の詳細に依存しないようにすることができます。

.NET Framework ライブラリの参照アセンブリは、ターゲット パックと共に配布されます。 これらを入手するには、スタンドアロン インストーラーをダウンロードするか、Visual Studio インストーラーでコンポーネントを選択します。 詳細については、「[開発者向けの .NET Framework のインストール](../../framework/install/guide-for-developers.md)」を参照してください。 .NET Core と .NET Standard の場合、参照アセンブリは必要に応じて (NuGet 経由で) 自動的にダウンロードされ、参照されます。 .NET Core 3.0 以降では、コア フレームワーク用の参照アセンブリが [Microsoft.NETCore.App.Ref](https://www.nuget.org/packages/Microsoft.NETCore.App.Ref) パッケージに含まれています (3.0 より前のバージョンでは、代わりに [Microsoft.NETCore.App](https://www.nuget.org/packages/Microsoft.NETCore.App) パッケージが使用されます)。 詳細については、.NET Core ガイドの「[パッケージ、メタパッケージ、フレームワーク](../../core/packages.md)」を参照してください。

Visual Studio で **[参照の追加]** ダイアログを使用して .NET Framework アセンブリへの参照を追加する場合、リストからアセンブリを選択すると、Visual Studio ではプロジェクトで選択されたターゲット フレームワーク バージョンに対応する参照アセンブリが自動的に検索されます。 [[参照]](/visualstudio/msbuild/common-msbuild-project-items#reference) プロジェクト項目を使用して MSBuild プロジェクトに参照を直接追加する場合も同様です。完全なファイルのパスではなく、アセンブリ名のみを指定する必要があります。 `-reference` コンパイラ オプション ([C#](../../csharp/language-reference/compiler-options/reference-compiler-option.md) および [Visual Basic](../../visual-basic/reference/command-line-compiler/reference.md)) を使用するか、Roslyn API の <xref:Microsoft.CodeAnalysis.Compilation.AddReferences%2A?displayProperty=nameWithType> メソッドを使用して、コマンド ラインでこれらのアセンブリに参照を追加する場合、正しいターゲット プラットフォーム バージョンの参照アセンブリ ファイルを手動で指定する必要があります。 .NET Framework 参照アセンブリ ファイルは、 *%ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETFramework* ディレクトリにあります。 .NET Core の場合、`PreserveCompilationContext` プロジェクト プロパティを `true` に設定することにより、ターゲット プラットフォームの参照アセンブリを出力ディレクトリの *publish/refs* サブディレクトリにコピーするように発行操作を強制できます。 次に、これらの参照アセンブリ ファイルをコンパイラに渡すことができます。 [Microsoft.Extensions.DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/) パッケージから `DependencyContext` を使用すると、パスを特定するために役立ちます。

実装が含まれていないため、参照アセンブリを実行用に読み込むことはできません。これを行おうとすると、<xref:System.BadImageFormatException?displayProperty=nameWithType> の結果になります。 ただし、内容を調べる必要がある場合は、(<xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> メソッドを使用して) リフレクションのみのコンテキストに読み込むことができます。

## <a name="generating-reference-assemblies"></a>参照アセンブリの生成

ライブラリの参照アセンブリを生成することは、ライブラリ コンシューマーが多数の異なるバージョンのライブラリに対してプログラムをビルドする必要がある場合に役立ちます。 これらのすべてのバージョンに対する実装アセンブリの配布は、サイズが大きいために現実的ではない場合があります。 参照アセンブリのサイズは小さいため、ライブラリの SDK の一部として配布すると、ダウンロード サイズが減り、ディスク領域が節約されます。

また、IDE とビルド ツールでは、参照アセンブリを利用すると、複数のクラス ライブラリで構成される大規模なソリューションの場合にビルド時間を短縮することもできます。 通常、インクリメンタル ビルドのシナリオでは、プロジェクトは、依存するアセンブリを含め、入力ファイルが変更されるとリビルドされます。 実装アセンブリは、プログラマーが任意のメンバーの実装を変更するたびに変わります。 参照アセンブリは、パブリック API が影響を受ける場合にのみ変更されます。 そのため、参照アセンブリを実装アセンブリではなく入力ファイルとして使用すると、場合によっては、依存プロジェクトのビルドがスキップされることがあります。

参照アセンブリは次のように生成できます。

- MSBuild プロジェクトでは、[`ProduceReferenceAssembly` プロジェクト プロパティ](/visualstudio/msbuild/common-msbuild-project-properties)を使用します。
- コマンド ラインからプログラムをコンパイルする場合は、`-refonly` ([C#](../../csharp/language-reference/compiler-options/refonly-compiler-option.md) / [Visual Basic](../../visual-basic/reference/command-line-compiler/refonly-compiler-option.md) ) または `-refout` ([C#](../../csharp/language-reference/compiler-options/refout-compiler-option.md) / [Visual Basic](../../visual-basic/reference/command-line-compiler/refout-compiler-option.md)) コンパイラ オプションを指定します。
- Roslyn API を使用する場合は、<xref:Microsoft.CodeAnalysis.Compilation.Emit%2A?displayProperty=nameWithType> メソッドに渡されるオブジェクトで <xref:Microsoft.CodeAnalysis.Emit.EmitOptions.EmitMetadataOnly?displayProperty=nameWithType> を `true` に、<xref:Microsoft.CodeAnalysis.Emit.EmitOptions.IncludePrivateMembers?displayProperty=nameWithType> を `false` に設定します。

NuGet パッケージと共に参照アセンブリを配布する場合は、実装アセンブリに使用される *lib\\* サブディレクトリではなく、パッケージ ディレクトリ以下の *ref\\* サブディレクトリに含める必要があります。

## <a name="reference-assemblies-structure"></a>参照アセンブリの構造

参照アセンブリは、"*メタデータのみのアセンブリ*" という関連する概念を拡張したものです。 メタデータのみアセンブリには、1 つの `throw null` 本体で置き換えられたメソッド本体がありますが、匿名型を除くすべてのメンバーが含まれます。 (本体なしではなく) `throw null` 本体を使用する理由は、**PEVerify** を実行して渡せるようにするためです (そのためにメタデータの完全性を検証します)。

参照アセンブリは、メタデータのみアセンブリからさらにメタデータ (プライベート メンバー) を削除します。

- 参照アセンブリには、API サーフェスでそれが必要とする参照のみがあります。 実際のアセンブリには、特定の実装に関連するその他の参照がある場合があります。 たとえば、`class C { private void M() { dynamic d = 1; ... } }` の参照アセンブリは、`dynamic` に必要などの型も参照しません。
- プライベート関数のメンバー (メソッド、プロパティ、およびイベント) は、それらの削除がコンパイルに著しい影響を与えない場合に削除されます。 [InternalsVisibleTo](xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute) 属性がない場合は、内部関数メンバーも削除されます。

参照アセンブリ内のメタデータでは、引き続き次の情報が保持されます。

- プライベート型と入れ子にされた型を含むすべての型。
- すべての属性 (内部的なものも含む)。
- すべての仮想メソッド。
- 明示的なインターフェイスの実装。
- プロパティとイベントは、アクセサーが仮想であるため、明示的に実装されます。
- 構造体のすべてのフィールド。

参照アセンブリには、アセンブリレベルの [ReferenceAssembly](xref:System.Runtime.CompilerServices.ReferenceAssemblyAttribute) 属性が含まれます。 この属性は、ソースで指定できます。指定すると、コンパイラではこれを合成する必要がなくなります。 この属性により、ランタイムで実行のための参照アセンブリの読み込みが拒否されます (ただし、リフレクションのみモードでは読み込むことができます)。

正確な参照アセンブリ構造の詳細は、コンパイラのバージョンによって異なります。 新しいバージョンでは、パブリック API のサーフェスに影響がないと判断された場合、より多くのメタデータを除外することを選択できます。

> [!NOTE]
> このセクションの情報は、C# バージョン 7.1 または Visual Basic バージョン 15.3 以降の Roslyn コンパイラによって生成された参照アセンブリにのみ適用されます。 .NET Framework および .NET Core ライブラリの参照アセンブリの構造は、参照アセンブリを生成する独自のメカニズムを使用するため、詳細が多少異なる場合があります。 たとえば、`throw null` の本体ではなく、メソッドの本体が完全に空の場合があります。 ただし、一般的な原則は引き続き適用されます。つまり、使用可能なメソッド実装は含まれず、パブリック API の観点から目に見える影響があるメンバーのメタデータのみが含まれます。

## <a name="see-also"></a>関連項目

- [.NET のアセンブリ](index.md)
- [フレームワーク対象設定機能の概要](/visualstudio/ide/visual-studio-multi-targeting-overview)
- [方法: 参照マネージャーを使用して参照を追加または削除する](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager)
