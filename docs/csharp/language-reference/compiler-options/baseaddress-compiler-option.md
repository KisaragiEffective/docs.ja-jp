---
title: -baseaddress (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /dllbase
helpviewer_keywords:
- baseaddress compiler option [C#]
- -baseaddress compiler option [C#]
- /baseaddress compiler option [C#]
ms.assetid: ce13c965-dfe4-4433-94f5-63b476e3a608
ms.openlocfilehash: f138f445b8a335c7505e25b34f560c4da40ab2dd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937208"
---
# <a name="-baseaddress-c-compiler-options"></a>-baseaddress (C# コンパイラ オプション)
**-baseaddress** オプションを使用すると、DLL を読み込む位置に推奨されるベース アドレスを指定できます。 このオプションを使用するタイミングと理由の詳細については、[Larry Osterman のブログ](https://docs.microsoft.com/archive/blogs/larryosterman/why-should-i-even-bother-to-use-dlls-in-my-system)を参照してください。  
  
## <a name="syntax"></a>構文  
  
```console  
-baseaddress:address  
```  
  
## <a name="arguments"></a>引数  
 `address`  
 DLL のベース アドレス。 このアドレスは、10 進数、16 進数、または 8 進数で指定できます。  
  
## <a name="remarks"></a>解説  
 DLL の既定のベース アドレスは、.NET Framework 共通言語ランタイムにより設定されます。  
  
 このアドレスの下位ワードは丸められることに注意してください。 たとえば、0x11110001 と指定すると、丸められて 0x11110000 になります。  
  
 DLL の署名プロセスを完了するには、-R オプションを指定した SN.EXE を使用します。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ]** ページを開きます。  
  
2. **[ビルド]** プロパティ ページをクリックします。  
  
3. **[詳細設定]** ボタンをクリックします。  
  
4. **DLL のベース アドレス** プロパティを変更します。  
  
     このコンパイラ オプションをプログラムによって設定するには、「<xref:VSLangProj80.CSharpProjectConfigurationProperties3.BaseAddress%2A>」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Diagnostics.ProcessModule.BaseAddress%2A?displayProperty=nameWithType>
- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
