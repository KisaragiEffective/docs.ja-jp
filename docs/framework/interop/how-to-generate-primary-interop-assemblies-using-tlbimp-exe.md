---
title: '方法: Tlbimp.exe を使用してプライマリ相互運用機能アセンブリを生成する'
ms.date: 03/30/2017
helpviewer_keywords:
- primary interop assemblies, generating
- Tlbimp.exe
- Type Library Importer
ms.assetid: 5419011c-6e57-40f6-8c65-386db8f7a651
ms.openlocfilehash: e46295b89b042452cb6e303302a8b88d68d58426
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123911"
---
# <a name="how-to-generate-primary-interop-assemblies-using-tlbimpexe"></a>方法: Tlbimp.exe を使用してプライマリ相互運用機能アセンブリを生成する

プライマリ相互運用機能アセンブリを生成するには、次の 2 つの方法があります。

- Windows SDK によって提供される[タイプ ライブラリ インポーター (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) を使用します。

  プライマリ相互運用機能アセンブリを生成する最も簡単な方法は、[Tlbimp.exe (タイプ ライブラリ インポーター)](../tools/tlbimp-exe-type-library-importer.md) を使用することです。 Tlbimp.exe は、次の保護機能を提供します。

  - 新規の相互運用機能アセンブリを作成するときには、その前に登録されている他のプライマリ相互運用機能アセンブリを調べて、入れ子になったタイプ ライブラリの参照がないかを確認してください。

  - プライマリ相互運用機能アセンブリに厳密な名前が付くようにコンテナー名やファイル名を指定しない場合は、プライマリ相互運用機能アセンブリの生成に失敗します。

  - 依存アセンブリへの参照を省略した場合は、プライマリ相互運用機能アセンブリの生成に失敗します。

  - プライマリ相互運用機能アセンブリではない依存アセンブリへの参照を追加した場合は、プライマリ相互運用機能アセンブリの生成に失敗します。

- C# などの共通言語仕様 (CLS) に準拠した言語を使用して、ソース コードで手動によりプライマリ相互運用機能アセンブリを作成します。 この方法は、タイプ ライブラリが使用できない場合に便利です。

厳密な名前でアセンブリに署名するには、暗号キー ペアが必要です。 詳細については、「[キー ペアを作成する](../../standard/assembly/create-public-private-key-pair.md)」を参照してください。

### <a name="to-generate-a-primary-interop-assembly-using-tlbimpexe"></a>Tlbimp.exe を使用してプライマリ相互運用機能アセンブリを生成する方法

1. コマンド プロンプトで、次のコマンドを入力します。

    **tlbimp** *tlbfile*  **/primary /keyfile:** *filename* **/out:** *assemblyname*

    このコマンドの *tlbfile* は COM タイプ ライブラリを含むファイル、*filename* はキー ペアを含むコンテナーまたはファイルの名前、*assemblyname* は厳密な名前で署名するアセンブリの名前です。

プライマリ相互運用機能アセンブリは、他のプライマリ相互運用機能アセンブリのみを参照できます。 アセンブリがサードパーティの COM タイプ ライブラリからタイプを参照する場合、プライマリ相互運用機能アセンブリを生成する前に、発行者からプライマリ相互運用機能アセンブリを取得する必要があります。 発行者である場合は、参照元のプライマリ相互運用機能アセンブリを生成する前に、依存タイプ ライブラリのプライマリ相互運用機能アセンブリを生成する必要があります。

元のタイプ ライブラリとは異なるバージョン番号の依存プライマリ相互運用機能アセンブリは、現在のディレクトリにインストールされている場合、検出できません。 依存プライマリ相互運用機能アセンブリを Windows レジストリに登録するか、または **/reference** オプションを使用して、Tlbimp.exe が依存 DLL を検出できるようにする必要があります。

複数のバージョンのタイプ ライブラリをラップすることもできます。 その方法については、「[方法: 複数のバージョンのタイプ ライブラリをラップする](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/1565h6hc(v=vs.100))」を参照してください。

## <a name="example"></a>例

次の例は、COM タイプ ライブラリ `LibUtil.tlb` をインポートし、アセンブリ `LibUtil.dll` にキー ファイル `CompanyA.snk` を使用して厳密な名前で署名します。 この例では、特定の名前空間名を省略することにより、既定の名前空間 `LibUtil` を生成します。

```console
tlbimp LibUtil.tlb /primary /keyfile:CompanyA.snk /out:LibUtil.dll
```

名前を (*VendorName*.*LibraryName* の名前付けガイドラインを使用して) より分かりやすくするために、次の例では既定のアセンブリ ファイル名と名前空間名をオーバーライドします。

```console
tlbimp LibUtil.tlb /primary /keyfile:CompanyA.snk /namespace:CompanyA.LibUtil /out:CompanyA.LibUtil.dll
```

次の例では、`CompanyA.LibUtil.dll` を参照する `MyLib.tlb` をインポートし、アセンブリ `CompanyB.MyLib.dll` にキー ファイル `CompanyB.snk` を使用して厳密な名前で署名します。 名前空間 `CompanyB.MyLib` は、既定の名前空間名をオーバーライドします。

```console
tlbimp MyLib.tlb /primary /keyfile:CompanyB.snk /namespace:CompanyB.MyLib /reference:CompanyA.LibUtil.dll /out:CompanyB.MyLib.dll
```

## <a name="see-also"></a>関連項目

- [方法: プライマリ相互運用機能アセンブリを登録する](how-to-register-primary-interop-assemblies.md)
