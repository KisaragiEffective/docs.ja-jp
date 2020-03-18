---
title: -debug (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /debug
helpviewer_keywords:
- debug compiler option [C#]
- -debug compiler option [C#]
- /debug compiler option [C#]
ms.assetid: e2b48c07-01bc-45cc-a52c-92e9085eb969
ms.openlocfilehash: 8bb2b411dc867b6a43e52058dccf2ac980cf0b1e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69922510"
---
# <a name="-debug-c-compiler-options"></a>-debug (C# コンパイラ オプション)
**-debug** オプションを指定すると、コンパイラによってデバッグ情報が生成され、出力ファイルに格納されます。  
  
## <a name="syntax"></a>構文  
  
```console  
-debug[+ | -]  
-debug:{full | pdbonly}  
```  
  
## <a name="arguments"></a>引数  
 `+` &#124; `-`  
 `+` を指定するか、または単に **-debug** と指定すると、コンパイラによってデバッグ情報が生成され、その情報がプログラム データベース (.pdb ファイル) に出力されます。 `-`-debug **を指定しない場合、** の指定が有効となります。これを指定した場合、デバッグ情報は作成されません。  
  
 `full` &#124; `pdbonly`  
 コンパイラによって生成されるデバッグ情報の種類を指定します。 full 引数を使用すると ( **-debug:pdbonly** を指定しない場合)、実行中のプログラムにデバッガーをアタッチできます。 pdbonly を指定すると、プログラムがデバッガーで開始されたときにはソース コードをデバッグできますが、実行中のプログラムをデバッガーにアタッチしたときはアセンブラーしか表示されません。  
  
## <a name="remarks"></a>解説  
 このオプションを使用してデバッグ ビルドを作成します。 **-debug**、 **-debug+** 、または **-debug:full** のいずれも指定しなかった場合、プログラムの出力ファイルをデバッグすることはできません。  
  
 **-debug:full** を使用する場合は、JIT によって最適化されるコードの速度とサイズに若干影響が生じる点に注意してください。また、 **-debug:full** でデバッグした場合、わずかではありますが、コードの品質にも影響が生じます。 生成されるリリース コードには、 **-debug:pdbonly** を使用するか、PDB を一切使用しないことをお勧めします。  
  
> [!NOTE]
> **-debug:pdbonly** と **-debug:full** の唯一の違いは、 **-debug:full** でコンパイルした場合、デバッグ情報が利用可能であることを JIT コンパイラに通知するための <xref:System.Diagnostics.DebuggableAttribute> が生成される点です。 したがって、<xref:System.Diagnostics.DebuggableAttribute>-debug:full**を使用する場合に、コード内で** が false に設定されていると、エラーが生成されます。  
  
 アプリケーションのデバッグ パフォーマンスを構成する方法については、「[イメージのデバッグの簡略化](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md)」を参照してください。  
  
 .pdb ファイルの場所を変更する方法については、「[-pdb (C# コンパイラ オプション)](./pdb-compiler-option.md)」を参照してください。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ]** ページを開きます。  
  
2. **[ビルド]** プロパティ ページをクリックします。  
  
3. **[詳細設定]** ボタンをクリックします。  
  
4. **[デバッグ情報]** プロパティを変更します。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.DebugSymbols%2A>」をご覧ください。  
  
## <a name="example"></a>例  
 デバッグ情報を出力ファイル `app.pdb` に出力します。  
  
```console  
csc -debug -pdb:app.pdb test.cs  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
