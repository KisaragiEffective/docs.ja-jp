---
title: .NET のアセンブリ
ms.date: 08/15/2019
ms.assetid: 149f5ca5-5b34-4746-9542-1ae43b2d0256
helpviewer_keywords:
- dynamic assemblies
- security [.NET Framework], boundaries
- boundaries of assemblies
- assemblies [.NET Framework], about
- assemblies [.NET Framework], boundaries
- reference scope boundaries
- assemblies [.NET Framework]
- version boundaries
- type boundaries
ms.openlocfilehash: f85fef37ac952c91ac73570f26d80d8a46f4eedf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156506"
---
# <a name="assemblies-in-net"></a>.NET のアセンブリ

アセンブリは、.NET ベースのアプリケーションの配置、バージョン管理、再利用、アクティベーション スコープ、およびセキュリティ権限の基本単位です。 アセンブリは、相互に連携して 1 つの論理的な機能単位を形成するように構築された型やリソースの集合です。 アセンブリは、実行可能 ( *.exe*) ファイルまたはダイナミック リンク ライブラリ ( *.dll*) ファイルの形を取る、.NET アプリケーションの構成要素です。 それらは、型の実装に関して必要な情報を共通言語ランタイムに提供します。

.NET Core と .NET Framework では、1 つまたは複数のソース コード ファイルからアセンブリをビルドできます。 .NET Framework では、アセンブリに 1 つまたは複数のモジュールを含めることができます。 これを使うと、より大規模なプロジェクトの計画が可能になり、複数の開発者が別々のソース コード ファイルやモジュールで作業し、それらを組み合わせて 1 つのアセンブリを作成することができます。 モジュールについて詳しくは、「[方法:マルチファイル アセンブリをビルドする](../../framework/app-domains/build-multifile-assembly.md)」を参照してください。

アセンブリには、次の特徴があります。

- アセンブリは、 *.exe* または *.dll* ファイルとして実装されます。

- .NET Framework をターゲットにするライブラリの場合、アセンブリを[グローバル アセンブリ キャッシュ (GAC)](../../framework/app-domains/gac.md) に配置することで、それをアプリケーション間で共有することができます。 アセンブリを GAC に含めるには、アセンブリに厳密な名前を付ける必要があります。 詳細については、「[厳密な名前付きアセンブリ](strong-named.md)」を参照してください。

- アセンブリは、必要な場合にのみメモリに読み込まれます。 これらが使用されていない場合は読み込まれません。 これは、アセンブリを使用すると、大規模なプロジェクト内のリソースを効率的に管理できることを意味します。

- リフレクションを使用して、アセンブリに関する情報をプログラムによって取得できます。 詳細については、「[リフレクション (C#)](../../csharp/programming-guide/concepts/reflection.md)」または「[リフレクション (Visual Basic)](../../visual-basic/programming-guide/concepts/reflection.md)」をご覧ください。

- .NET Core の <xref:System.Reflection.MetadataLoadContext> クラスと .NET Core および .NET Framework の <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> または <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A?displayProperty=nameWithType> のメソッドを使用して、アセンブリを検査するためだけに読み込むことができます。

## <a name="assemblies-in-the-common-language-runtime"></a>共通言語ランタイムのアセンブリ

アセンブリは、型の実装に関して必要な情報を共通言語ランタイムに提供します。 共通言語ランタイムにとって、型はアセンブリのコンテキストの外部には存在しません。

アセンブリは、次の情報を定義します。

- 共通言語ランタイムが実行するコード。 各アセンブリが保持できるエントリ ポイントは 1 つだけです (`DllMain`、`WinMain`、または `Main`)。

- セキュリティの境界。 アクセス許可の要求および付与は、アセンブリを単位として行われます。 アセンブリのセキュリティの境界の詳細については、「[アセンブリのセキュリティに関する考慮事項](security-considerations.md)」を参照してください。

- 型の境界。 すべての型の ID には、その型を含んでいるアセンブリの名前が含まれます。 したがって、あるアセンブリのスコープに読み込まれた `MyType` と呼ばれる型は、別のアセンブリのスコープに読み込まれた `MyType` と呼ばれる型とは異なります。

- 参照スコープの境界。 [アセンブリのマニフェスト](#assembly-manifest)には、型を解決したり、リソース要求に応答したりするために使用されるメタデータが含まれています。 マニフェストは、アセンブリの外部に公開する型とリソースを指定し、依存する他のアセンブリを列挙します。 ポータブル実行可能 (PE: Portable Executable) ファイル内の MSIL (Microsoft Intermediate Language) コードに[アセンブリ マニフェスト](#assembly-manifest)が関連付けられていない場合、このコードは実行されません。

- バージョンの境界。 アセンブリは、共通言語ランタイムの最小のバージョン管理可能単位です。 同じアセンブリ内のすべての型とリソースは、1 つの単位としてバージョン管理されます。 [アセンブリ マニフェスト](#assembly-manifest)には、任意の依存アセンブリに対して指定した、バージョンの依存関係が記述されています。 バージョン管理の詳細については、「[アセンブリのバージョン管理](versioning.md)」を参照してください。

- 展開単位。 アプリケーションの起動時には、そのアプリケーションが最初に呼び出すアセンブリだけが必要です。 その他のアセンブリ、たとえばローカリゼーション リソースや、ユーティリティ クラスを格納するアセンブリは必要に応じて取得できます。 これにより、最初にダウンロードされるアプリを単純化し、サイズを縮小できます。 アセンブリの配置の詳細については、「[アプリケーションの配置](../../framework/deployment/index.md)」を参照してください。

- side-by-side 実行単位。 同じアセンブリの複数バージョンの同時実行については、「[アセンブリと side-by-side 実行](side-by-side-execution.md)」を参照してください。

## <a name="create-an-assembly"></a>アセンブリを作成する

アセンブリには、静的アセンブリと動的アセンブリの 2 種類があります。 静的アセンブリは、ディスク上のポータブル実行可能 (PE) ファイルに保存されます。 静的アセンブリには、インターフェイス、クラス、およびビットマップ、JPEG ファイル、その他のリソース ファイルなどのリソースを含めることができます。 動的アセンブリを作成することもできます。動的アセンブリはメモリから直接実行され、実行前にディスクに保存されることはありません。 動的アセンブリは、実行後にディスクに保存できます。

アセンブリを作成するには、いくつかの方法があります。 Visual Studio など、 *.dll* ファイルや *.exe* ファイルを作成できる開発ツールを使用できます。 他の開発環境のモジュールを使ってアセンブリを作成するには、Windows SDK の各種ツールを使用できます。 動的アセンブリの作成には、<xref:System.Reflection.Emit?displayProperty=nameWithType> などの共通言語ランタイム API も使用できます。

アセンブリをコンパイルするには、Visual Studio でビルドするか、.NET Core コマンド ライン インターフェイス ツールでビルドするか、コマンド ライン コンパイラを使用して .NET Framework アセンブリを構築します。 .NET Core CLI を使ったアセンブリのビルドについて詳しくは、「[.NET Core CLI の概要](../../core/tools/index.md)」をご覧ください。 コマンド ライン コンパイラを使ったアセンブリのビルドについては、「[csc.exe を使用したコマンド ラインからのビルド](../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)」(C#)、および[コマンド ラインからのビルド](../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md) (Visual Basic) に関する記事を参照してください。

> [!NOTE]
> Visual Studio でアセンブリをビルドするには、 **[ビルド]** メニューの **[ビルド]** を選択します。

## <a name="assembly-manifest"></a>アセンブリ マニフェスト

すべてのアセンブリの中に "*アセンブリ マニフェスト*" ファイルがあります。 目次と同じように、アセンブリ マニフェストには次が含まれます。

- アセンブリの ID (名前とバージョン)。

- アセンブリを構成するその他すべてのファイルについて記述するファイル テーブル (自分で作成したその他のアセンブリで *.exe* や *.dll* ファイルが依存しているもの、またはビットマップや Readme ファイルなど)。

- *.dll* やその他のファイルなど、すべての外部依存関係の一覧である*アセンブリ参照リスト*。 アセンブリ参照には、グローバルおよびプライベートの両方のオブジェクトへの参照が含まれます。 グローバル オブジェクトは、その他のすべてのアプリケーションで利用できます。 .NET Core では、グローバル オブジェクトは特定の .NET Core ランタイムと組み合わされます。 .NET Framework では、グローバル オブジェクトはグローバル アセンブリ キャッシュ (GAC) 内にあります。 *System.IO.dll* は、GAC 内のアセンブリの一例です。 プライベート オブジェクトは、アプリがインストールされているディレクトリと同じレベルまたはその下のディレクトリ内に存在する必要があります。

アセンブリにはコンテンツ、バージョン管理、および依存関係に関する情報が含まれているため、それらを使用するアプリケーションは、Windows システムのレジストリのような外部ソースに依存しなくても正常に機能することができます。 アセンブリは、 *.dll* の競合を減らし、アプリケーションの信頼性を高め、配置を容易にします。 多くの場合、.NET ベースのアプリケーションは、ターゲット コンピューターにそのファイルをコピーするだけでインストールすることができます。 詳細については、「[アセンブリ マニフェスト](manifest.md)」を参照してください。

## <a name="add-a-reference-to-an-assembly"></a>アセンブリに参照を追加する

アプリケーションでアセンブリを使用するには、アセンブリへの参照を追加する必要があります。 アセンブリが参照された後は、その名前空間のアクセス可能なすべての型、プロパティ、メソッド、およびその他のメンバーを、そのコードがご自分のソース ファイルの一部であるかのようにアプリケーションで使用することができます。

> [!NOTE]
> .NET クラス ライブラリのほとんどのアセンブリは自動的に参照されます。 システム アセンブリが自動的に参照されない場合、.NET Core では、アセンブリを含む NuGet パッケージへの参照を追加できます。 Visual Studio で NuGet パッケージ マネージャーを使用するか、またはアセンブリの [\<PackageReference>](../../core/tools/dependencies.md#the-packagereference-element) 要素を *.csproj* または *.vbproj* プロジェクトに追加します。 .NET Framework では、アセンブリへの参照を追加するには、Visual Studio の **[参照の追加]** ダイアログを使用するか、[C#](../../csharp/language-reference/compiler-options/reference-compiler-option.md) または [Visual Basic](../../visual-basic/reference/command-line-compiler/reference.md) コンパイラに向けてコマンド ライン オプション `-reference` を使用します。

C# では、1 つのアプリケーションで同じアセンブリの 2 つのバージョンを使用することができます。 詳細については、「[extern エイリアス](../../csharp/language-reference/keywords/extern-alias.md)」を参照してください。

## <a name="related-content"></a>関連コンテンツ

|Title|説明|
|-----------|-----------------|
|[アセンブリの内容](contents.md)|アセンブリを構成する要素。|
|[アセンブリ マニフェスト](manifest.md)|アセンブリ マニフェストのデータと、データをアセンブリに格納する方法。|
|[グローバル アセンブリ キャッシュ](../../framework/app-domains/gac.md)|GAC がアセンブリを格納および使用する方法。|
|[厳密な名前付きアセンブリ](strong-named.md)|厳密な名前付きアセンブリの特性。|
|[アセンブリのセキュリティに関する考慮事項](security-considerations.md)|セキュリティがアセンブリに対してどのように作用するか。|
|[アセンブリのバージョン管理](versioning.md)|.NET Framework のバージョン管理ポリシーの概要。|
|[アセンブリの配置](../../framework/app-domains/assembly-placement.md)|アセンブリの配置場所。|
|[アセンブリと side-by-side 実行](side-by-side-execution.md)|複数のバージョンのランタイムやアセンブリの同時使用。|
|[動的メソッドおよびアセンブリの出力](../../../docs/framework/reflection-and-codedom/emitting-dynamic-methods-and-assemblies.md)|動的アセンブリの作成方法。|
|[ランタイムがアセンブリを検索する方法](../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md)|実行時に .NET Framework でアセンブリ参照がどのように解決されるか。|

## <a name="reference"></a>関連項目

<xref:System.Reflection.Assembly?displayProperty=nameWithType>

## <a name="see-also"></a>関連項目

- [.NET アセンブリ ファイルの形式](file-format.md)
- [フレンド アセンブリ](friend.md)
- [参照アセンブリ](reference-assemblies.md)
- [方法: アセンブリを読み込み、アンロードする](load-unload.md)
- [方法: .NET Core でアセンブリのアンローダビリティを使用およびデバッグする](unloadability.md)
- [方法: ファイルがアセンブリであるかどうかを確認する](identify.md)
