---
title: -optimize (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /optimize
helpviewer_keywords:
- /optimize compiler option [C#]
- -o compiler option [C#]
- optimize compiler option [C#]
- /o compiler option [C#]
- -optimize compiler option [C#]
- compiler optimization [C#]
- o compiler option [C#]
ms.assetid: 6dd5b6f2-cd1d-4593-a9f4-1c2ed9404ca0
ms.openlocfilehash: bec99ca582070a99fd8b734ef8a7b9e71d945488
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69606600"
---
# <a name="-optimize-c-compiler-options"></a>-optimize (C# コンパイラ オプション)
**-optimize** オプションは、コンパイラで実行する最適化を有効または無効にします。最適化を実行すると、出力ファイルのサイズが小さくなり、速度と効率が向上します。  
  
## <a name="syntax"></a>構文  
  
```console  
-optimize[+ | -]  
```  
  
## <a name="remarks"></a>解説  
 **-optimize** は実行時にコードを最適化するように共通言語ランタイムに指示します。  
  
 既定では、最適化が無効になります。 **-optimize+** を指定して最適化を有効にします。  
  
 モジュールをアセンブリで使用するように作成する場合は、アセンブリと同じ **-optimize** 設定を使用します。  
  
 **-o** は **-optimize** の省略形です。  
  
 **-optimize** オプションと [-debug](./debug-compiler-option.md) オプションを結合することができます。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ]** ページを開きます。  
  
2. **[ビルド]** プロパティ ページをクリックします。  
  
3. **コードの最適化**プロパティを変更します。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.Optimize%2A>」をご覧ください。  
  
## <a name="example"></a>例  
 `t2.cs` をコンパイルしてコンパイラの最適化を有効にします。  
  
```console  
csc t2.cs -optimize  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
