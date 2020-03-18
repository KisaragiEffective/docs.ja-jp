---
title: 厳密な名前付きアセンブリの作成と使用
ms.date: 08/19/2019
helpviewer_keywords:
- strong-name bypass feature
- strong-named assemblies, about strong-named assemblies
- strong-named assemblies
- signing assemblies
- assemblies [.NET Framework], signing
- strong-named assemblies, scenarios
- assemblies [.NET Framework], strong-named
- strong-named assemblies, loading into trusted application domains
- assembly binding, strong-named
ms.assetid: ffbf6d9e-4a88-4a8a-9645-4ce0ee1ee5f9
ms.openlocfilehash: 18a0b7d657290835a34c705513d0d7a4ccbfc61c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75738683"
---
# <a name="create-and-use-strong-named-assemblies"></a>厳密な名前付きアセンブリの作成と使用

厳密な名前は、単純テキスト名、バージョン番号、カルチャ情報 (設定されている場合) から成るアセンブリの識別子と、公開キーおよびデジタル署名で構成されます。 このデジタル署名は、対応する秘密キーを使用してアセンブリ ファイルから生成されます。 (アセンブリ ファイルにはアセンブリ マニフェストが格納されており、そこに、アセンブリを構成するすべてのファイルの名前とハッシュが含まれます。)

> [!WARNING]
> セキュリティに関しては、厳格な名前に依存しないでください。 厳格な名前は、一意の ID を提供するだけです。

厳密な名前のアセンブリは、他の厳密な名前のアセンブリでのタイプだけを使用できます。 それ以外の場合は、厳密な名前のアセンブリの整合性が損なわれます。

> [!NOTE]
> .NET Core では厳密な名前のアセンブリがサポートされており、.NET Core ライブラリ内のすべてのアセンブリは署名されていますが、ほとんどのサードパーティ アセンブリには厳密な名前は必要ありません。 詳細については、GitHub の「[厳密な名前の署名](https://github.com/dotnet/runtime/blob/master/docs/project/strong-name-signing.md)」を参照してください。

## <a name="strong-name-scenario"></a>厳密な名前のシナリオ

次のシナリオは、厳密な名前のアセンブリに署名した後にそれをその名前で参照するプロセスの概要を示しています。

1. アセンブリ A は、次のいずれかの方法を使用して厳密な名前で作成されます。

    - Visual Studio など、厳密な名前の作成をサポートする開発環境を使用する。

    - [厳密名ツール (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) を使用して暗号化キー ペアを作成し、コマンド ライン コンパイラまたは[アセンブリ リンカー (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) のいずれかを使用してそのキー ペアをアセンブリに割り当てる。 Windows SDK には、Sn.exe と Al.exe の両方が用意されています。

2. 開発環境またはツールは、アセンブリのマニフェストを含むファイルのハッシュに、開発者の秘密キーで署名します。 このデジタル署名は、アセンブリ A のマニフェストを含むポータブル実行可能 (PE) ファイルに格納されます。

3. アセンブリ B はアセンブリ A のコンシューマーです。アセンブリ B のマニフェストの参照セクションには、アセンブリ A の公開キーを表すトークンが含まれています。 トークンは完全な公開キーの一部であり、スペースを節約するために、キー自体ではなくこれが使用されます。

4. 共通言語ランタイムは、アセンブリがグローバル アセンブリ キャッシュに入れられる際に、厳密な名前の署名を確認します。 実行時に厳密な名前でバインドするとき、共通言語ランタイムは、アセンブリ B のマニフェストに格納されているキーと、アセンブリ A の厳密な名前を生成するために使用されたキーを比較します。 .NET Framework のセキュリティ チェックに合格してバインドが成功すると、アセンブリ B は、アセンブリ A のビットが改ざんされていないことと、これらのビットが実際にアセンブリ A の開発者からのものであることが保証されます。

> [!NOTE]
> このシナリオは、信頼の問題を扱っていません。 厳密な名前に加えて、完全な Microsoft Authenticode 署名をアセンブリに持たせることができます。 Authenticode 署名には、信頼を確立するための証明書が含まれます。 厳密な名前を使用すると、コードにこのように署名する必要がないということに注意してください。 厳密な名前は、一意の ID を提供するだけです。

## <a name="bypass-signature-verification-of-trusted-assemblies"></a>信頼されたアセンブリの署名検証をバイパスする

.NET Framework 3.5 Service Pack 1 以降、たとえば `MyComputer` ゾーンの既定のアプリケーション ドメインなど、完全に信頼されたアプリケーション ドメインにアセンブリが読み込まれるときに、厳密な名前の署名は検証されなくなりました。 これは厳密な名前のバイパス機能と呼ばれます。 完全に信頼された環境では、<xref:System.Security.Permissions.StrongNameIdentityPermission> に対する完全に信頼された署名済みアセンブリの要求は、その署名に関係なく、常に成功します。 このような場合、厳密な名前のバイパス機能は、完全に信頼されたアセンブリの厳密名署名検証という不要なオーバーヘッドを回避するので、アセンブリを高速で読み込むことができます。

バイ パス機能は、厳密な名前で署名されていて、次の特性を持つアセンブリに適用されます。

- <xref:System.Security.Policy.StrongName> 証拠なしで完全に信頼されている (たとえば、`MyComputer` ゾーン証拠を持っている)。

- 完全に信頼された <xref:System.AppDomain> に読み込まれる。

- その <xref:System.AppDomain> の <xref:System.AppDomainSetup.ApplicationBase%2A> プロパティに基づいた場所から読み込まれる。

- 遅延署名されていない。

この機能は、個々のアプリケーションに対して、またはコンピューターに対して無効にすることができます。 「[方法:厳密な名前のバイパス機能を無効にする](disable-strong-name-bypass-feature.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

|Title|説明|
|-----------|-----------------|
|[方法: 公開キーと秘密キーのキー ペアを作成する](create-public-private-key-pair.md)|署名とアセンブリの暗号化キー ペアを作成する方法について説明します。|
|[方法: 厳密な名前でアセンブリに署名する](sign-strong-name.md)|厳密な名前のアセンブリを作成する方法について説明します。|
|[拡張された厳密な名前付け](enhanced-strong-naming.md)|.NET Framework 4.5 での厳密な名前の拡張について説明します。|
|[方法: 厳密な名前のアセンブリを参照する](reference-strong-named.md)|コンパイル時または実行時に厳密な名前のアセンブリでタイプまたはリソースを参照する方法について説明します。|
|[方法: 厳密な名前のバイパス機能を無効にする](disable-strong-name-bypass-feature.md)|厳密な名前の署名の検証をバイパスする機能を無効にする方法について説明します。 すべてのまたは特定のアプリケーションに対して、この機能を無効にすることができます。|
|[アセンブリを作成する](create.md)|単一ファイル アセンブリおよびマルチファイル アセンブリの概要を示します。|
|[Visual Studio 内でアセンブリに遅延署名する方法](/visualstudio/ide/managing-assembly-and-manifest-signing#how-to-sign-an-assembly-in-visual-studio)|アセンブリを作成した後に厳密な名前でアセンブリに署名する方法について説明します。|
|[Sn.exe (厳密名ツール)](../../framework/tools/sn-exe-strong-name-tool.md)|厳密な名前のアセンブリの作成に役立つ、.NET Framework に付属のツールについて説明します。 このツールには、キーの管理、署名の生成、署名の検査に関する各オプションが用意されています。|
|[Al.exe (アセンブリ リンカー)](../../framework/tools/al-exe-assembly-linker.md)|モジュールまたはリソース ファイルからアセンブリ マニフェストを含むファイルを生成する、.NET Framework に付属のツールについて説明します。|
