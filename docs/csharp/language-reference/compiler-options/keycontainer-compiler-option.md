---
title: -keycontainer (C# コンパイラ オプション)
ms.date: 05/16/2018
f1_keywords:
- /keycontainer
helpviewer_keywords:
- /keycontainer compiler option [C#]
- keycontainer compiler option [C#]
- -keycontainer compiler option [C#]
ms.assetid: b3982b6d-2382-4f7e-bebd-ce98eaa30763
ms.openlocfilehash: fead2d4296cfa6fb0195cb4b43f6448c0fc7e6a9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70970146"
---
# <a name="-keycontainer-c-compiler-options"></a>-keycontainer (C# コンパイラ オプション)
暗号化キー コンテナーの名前を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-keycontainer:string  
```  
  
## <a name="arguments"></a>引数  
 `string`  
 厳密な名前のキー コンテナーの名前です。  
  
## <a name="remarks"></a>解説  
 **-keycontainer** オプションを使うと、コンパイラは共有可能なコンポーネントを作成します。 コンパイラは、指定されたコンテナーからアセンブリ マニフェストに公開キーを挿入し、最終的なアセンブリに秘密キーで署名します。 キー ファイルを生成するには、コマンド ラインで「`sn -k file`」と入力します。 `sn -i` は、キー ペアをコンテナーにインストールします。 コンパイラが CoreCLR 上で実行するときは、このオプションはサポートされません。 CoreCLR 上でビルドするときにアセンブリに署名するには、[-keyfile](keyfile-compiler-option.md) オプションを使います。
  
 [-target:module](./target-module-compiler-option.md) を指定してコンパイルした場合は、キー ファイルの名前がモジュールに保持され、このモジュールを [-addmodule](./addmodule-compiler-option.md) でアセンブリにコンパイルするときに、アセンブリに組み込まれます。  
  
 このオプションは、任意の Microsoft Intermediate Language (MSIL) モジュールのソース コードで、カスタム属性 (<xref:System.Reflection.AssemblyKeyNameAttribute?displayProperty=nameWithType>) として指定することもできます。  
  
 また、暗号化情報を [-keyfile](./keyfile-compiler-option.md) でコンパイラに渡すことができます。 公開キーをアセンブリ マニフェストに追加してだけおき、アセンブリの署名はテスト後まで待って行いたい場合は、[-delaysign](./delaysign-compiler-option.md) を使います。  
  
 詳細については、「[厳密な名前付きアセンブリの作成と使用](../../../standard/assembly/create-use-strong-named.md)」および「[アセンブリへの遅延署名](../../../standard/assembly/delay-sign.md)」をご覧ください。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. このコンパイラ オプションは、Visual Studio 開発環境では使うことができません。  
  
 このコンパイラ オプションには、プログラムで <xref:VSLangProj.ProjectProperties.AssemblyKeyContainerName%2A> を使ってアクセスできます。  
  
## <a name="see-also"></a>参照

- [C# コンパイラの -keyfile オプション](keyfile-compiler-option.md)
- [C# コンパイラ オプション](index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
