---
title: コンパイル コマンド ラインのサンプル
ms.date: 03/13/2018
helpviewer_keywords:
- command line [Visual Basic], compilers
- compilation [Visual Basic], command-line
- command-line compilers
- compiling source code [Visual Basic], from command line
- Visual Basic compiler, sample command lines
ms.assetid: 5bfbb487-5f47-4267-969a-39dfb917beeb
ms.openlocfilehash: 27a20a5a3525353ffbced729b8ac9c98b3e48fc1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350856"
---
# <a name="sample-compilation-command-lines-visual-basic"></a>コンパイルコマンドラインのサンプル (Visual Basic)

Visual Studio 内から Visual Basic プログラムをコンパイルする代わりに、コマンドラインからコンパイルして、実行可能 (.exe) ファイルまたはダイナミックリンクライブラリ (.dll) ファイルを生成することもできます。

Visual Basic のコマンドラインコンパイラは、入力ファイル、出力ファイル、アセンブリ、およびデバッグオプションとプリプロセッサオプションを制御するオプションの完全なセットをサポートしています。 各オプションは、`-option` と `/option`の2つの交換可能な形式で使用できます。 このドキュメントでは、`-option` フォームのみを示します。

次の表に、自分で使用するために変更できるサンプルコマンドラインを示します。

|目的|新しく使用する機能|
|--------|---------|
|ファイル .vb をコンパイルして、ファイルを作成します。|`vbc -reference:Microsoft.VisualBasic.dll File.vb`|
|ファイル .vb をコンパイルし、ファイル .dll を作成します。|`vbc -target:library File.vb`|
|ファイル .vb をコンパイルして、.exe を作成します。|`vbc -out:My.exe File.vb`|
|ファイル .vb をコンパイルし、ファイル .dll という名前のライブラリと参照アセンブリの両方を作成します。|`vbc -target:library -ref:.\debug\bin\ref\file.dll File.vb`|
|最適化をオンにし、定義した `DEBUG` シンボルを使用して、現在のディレクトリにあるすべての Visual Basic ファイルをコンパイルします。|`vbc -define:DEBUG=1 -optimize -out:File2.exe *.vb`|
|現在のディレクトリにあるすべての Visual Basic ファイルをコンパイルし、ロゴまたは警告を表示せずに、File2 のデバッグバージョンを生成します。|`vbc -target:library -out:File2.dll -nowarn -nologo -debug *.vb`|
|現在のディレクトリにあるすべての Visual Basic ファイルを何らかの .dll にコンパイルします。|`vbc -target:library -out:Something.dll *.vb`|

> [!TIP]
> Visual Studio IDE を使用してプロジェクトをビルドする場合、関連付けられている**vbc.exe**コマンドに関する情報を [出力] ウィンドウのコンパイラオプションと共に表示できます。 この情報を表示するには、[オプション] ダイアログボックス、[プロジェクトとソリューション]、[ビルドと実行](/visualstudio/ide/reference/options-dialog-box-projects-and-solutions-build-and-run) の順に開き、 **MSBuild プロジェクトのビルド出力の詳細**レベルを **[標準]** または 高レベルの詳細 に設定します。

## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [条件付きコンパイル](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)
