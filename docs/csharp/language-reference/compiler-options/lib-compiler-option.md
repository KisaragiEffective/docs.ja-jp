---
title: -lib (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /lib
helpviewer_keywords:
- lib compiler option [C#]
- -lib compiler option [C#]
- /lib compiler option [C#]
ms.assetid: b0efcc88-e8aa-4df4-a00b-8bdef70b7673
ms.openlocfilehash: 0c230147be055170ca015f27bd42bb096399405d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69606826"
---
# <a name="-lib-c-compiler-options"></a>-lib (C# コンパイラ オプション)
**-lib** オプションは、[-reference (C# コンパイラ オプション)](./reference-compiler-option.md) オプションによって参照されるアセンブリの場所を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-lib:dir1[,dir2]  
```  
  
## <a name="arguments"></a>引数  
 `dir1`  
 参照されているアセンブリが現在の作業ディレクトリ (コンパイラを起動したディレクトリ) または共通言語ランタイムのシステム ディレクトリに見つからない場合にコンパイラが検索するディレクトリです。  
  
 `dir2`  
 アセンブリ参照を検索する 1 つまたは複数の追加ディレクトリです。 複数のディレクトリはコンマで区切り、それらの間に空白文字は入れません。  
  
## <a name="remarks"></a>解説  
 コンパイラは、完全に修飾されていないアセンブリ参照を次の順序で検索します。  
  
1. 現在の作業ディレクトリ。 これは、コンパイラを起動したディレクトリです。  
  
2. 共通言語ランタイムのシステム ディレクトリ。  
  
3. **-lib** によって指定されているディレクトリ。  
  
4. LIB 環境変数によって指定されているディレクトリ。  
  
 アセンブリ参照を指定するには **-reference** を使います。  
  
 **-lib** は追加です。複数回指定すると、前の値に追加されます。  
  
 **-lib** を使う代わりに、必要なアセンブリを作業ディレクトリにコピーしてもかまいません。このようにすると、アセンブリ名を **-reference** に渡すだけで済みます。 後で、作業ディレクトリからアセンブリを削除できます。 依存アセンブリへのパスはアセンブリ マニフェストで指定されていないため、ターゲット コンピューターで開始されたアプリケーションは、グローバル アセンブリ キャッシュでアセンブリを探して使います。  
  
 コンパイラがアセンブリを参照できるということは、共通言語ランタイムが実行時にアセンブリを検索して読み込むことができるという意味ではありません。 ランタイムが参照されているアセンブリを検索する方法について詳しくは、「[ランタイムがアセンブリを検索する方法](../../../framework/deployment/how-the-runtime-locates-assemblies.md)」をご覧ください。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1. プロジェクトの **[プロパティ ページ]** ダイアログ ボックスを開きます。  
  
2. **[参照パス]** プロパティ ページをクリックします。  
  
3. リスト ボックスの内容を変更します。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「 <xref:VSLangProj80.ProjectProperties3.ReferencePath%2A>」をご覧ください。  
  
## <a name="example"></a>例  
 t2.cs をコンパイルして .exe ファイルを作成します。 コンパイラは、作業ディレクトリと C ドライブのルート ディレクトリで、アセンブリ参照を探します。  
  
```console  
csc -lib:c:\ -reference:t2.dll t2.cs  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
