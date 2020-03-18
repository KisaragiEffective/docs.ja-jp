---
title: -keycontainer
ms.date: 03/10/2018
helpviewer_keywords:
- -keycontainer compiler option [Visual Basic]
- keycontainer compiler option [Visual Basic]
- /keycontainer compiler option [Visual Basic]
ms.assetid: 6a9bc861-1752-4db1-9f64-b5252f0482cc
ms.openlocfilehash: be2ad1416e801398fb513593c7f3828e5488bfaf
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005531"
---
# <a name="-keycontainer"></a>-keycontainer
アセンブリに厳密な名前を付けるキー ペアのキー コンテナー名を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-keycontainer:container  
```  
  
## <a name="arguments"></a>引数  
  
|項目|定義|  
|---|---|  
|`container`|必須。 キーが格納されているコンテナーファイル。 名前にスペースが含まれている場合は、ファイル名を引用符 ("") で囲みます。|  
  
## <a name="remarks"></a>コメント  
 コンパイラは公開キーをアセンブリマニフェストに挿入し、最後のアセンブリに秘密キーで署名することによって、共有可能なコンポーネントを作成します。 キー ファイルを生成するには、コマンド ラインで「`sn -k file`」と入力します。 @No__t-0 オプションを指定すると、キーペアがコンテナーにインストールされます。 詳細については、「[sn.exe (厳密名ツール)](../../../framework/tools/sn-exe-strong-name-tool.md)」を参照してください。  
  
 `-target:module` を指定してコンパイルすると、キーファイルの名前がモジュールに保持され、 [-addmodule](../../../visual-basic/reference/command-line-compiler/addmodule.md)を使用してアセンブリをコンパイルするときに作成されるアセンブリに組み込まれます。  
  
 このオプションは、任意の Microsoft Intermediate Language (MSIL) モジュールのソース コードで、カスタム属性 (<xref:System.Reflection.AssemblyKeyNameAttribute>) として指定することもできます。  
  
 また、暗号化情報を [-keyfile](../../../visual-basic/reference/command-line-compiler/keyfile.md) でコンパイラに渡すことができます。 部分的に署名されたアセンブリを作成する場合は、[-delaysign](../../../visual-basic/reference/command-line-compiler/delaysign.md) を使います。  
  
 アセンブリに署名する方法の詳細については、「[厳密な名前付きアセンブリの作成と使用](../../../standard/assembly/create-use-strong-named.md)」を参照してください。  
  
> [!NOTE]
> @No__t-0 オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="example"></a>例  
 次のコードでは、ソースファイル `Input.vb` でコンパイルし、キーコンテナーを指定しています。  
  
```console  
vbc -keycontainer:key1 input.vb  
```  
  
## <a name="see-also"></a>関連項目

- [.NET のアセンブリ](../../../standard/assembly/index.md)
- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-keyfile](../../../visual-basic/reference/command-line-compiler/keyfile.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
