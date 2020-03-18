---
title: -keyfile (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /keyfile
helpviewer_keywords:
- /keyfile compiler option [C#]
- -keyfile compiler option [C#]
- keyfile compiler option [C#]
ms.assetid: 0815f9de-ace4-4e98-b4c6-13c55dea40c2
ms.openlocfilehash: bf271cc6b6887e930911071d4603b51daed55e61
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70970262"
---
# <a name="-keyfile-c-compiler-options"></a>-keyfile (C# コンパイラ オプション)
暗号化キーを格納するファイル名を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-keyfile:file  
```  
  
## <a name="arguments"></a>引数  
  
|用語|定義|  
|----------|----------------|  
|`file`|厳密な名前のキーを格納するファイルの名前。|  
  
## <a name="remarks"></a>解説  
 このオプションを指定すると、コンパイラは、指定したファイルからアセンブリ マニフェストに公開キーを挿入し、最終的なアセンブリに秘密キーで署名します。 キー ファイルを生成するには、コマンド ラインで「sn -k `file`」と入力します。  
  
 **-target:module** を指定してコンパイルした場合は、キー ファイルの名前がモジュールに保持され、[-addmodule](./addmodule-compiler-option.md) でアセンブリをコンパイルすると作成されるアセンブリに組み込まれます。  
  
 また、暗号化情報を [-keycontainer](./keycontainer-compiler-option.md) でコンパイラに渡すことができます。 部分的に署名されたアセンブリを作成する場合は、[-delaysign](./delaysign-compiler-option.md) を使います。  
  
 同じコンパイルで (コマンド ライン オプションまたはカスタム属性によって) -keyfile と -keycontainer を両方とも指定すると、コンパイラは最初にキー コンテナーを試します。 それが成功すると、アセンブリはキー コンテナーの情報で署名されます。 キー コンテナーが見つからない場合、コンパイラは -keyfile で指定されたファイルを検索します。 ファイルが検出された場合、アセンブリはキー ファイルの情報で署名され、キー情報はキー コンテナーにインストールされるため (sn -i と同様)、次のコンパイル時にはキー コンテナーが有効になります。  
  
 キー ファイルには公開キーだけが含まれる場合があることに注意してください。  
  
 詳細については、「[厳密な名前付きアセンブリの作成と使用](../../../standard/assembly/create-use-strong-named.md)」および「[アセンブリへの遅延署名](../../../standard/assembly/delay-sign.md)」をご覧ください。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ]** ページを開きます。  
  
2. **[署名]** プロパティ ページをクリックします。  
  
3. **[厳密な名前のキー ファイルを選択してください]** プロパティを変更します。  
  
 このコンパイラ オプションには、プログラムで <xref:VSLangProj.ProjectProperties.AssemblyOriginatorKeyFile%2A> を使ってアクセスできます。  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
