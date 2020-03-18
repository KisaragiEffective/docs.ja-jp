---
title: csc.exe を使用したコマンド ラインからのビルド
ms.date: 04/19/2017
helpviewer_keywords:
- builds [C#]
- command line [C#]
ms.assetid: 66e70056-dd20-453c-a9b3-507e0478b015
ms.openlocfilehash: f692e66672b1804a309c6ac04c158af948a1b1ab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76789871"
---
# <a name="command-line-build-with-cscexe"></a>csc.exe を使用したコマンド ラインからのビルド

C# コンパイラは、その実行可能ファイルの名前 (*csc.exe*) をコマンド プロンプトに入力することによって呼び出します。

**Visual Studio 用開発者コマンド プロンプト** ウィンドウを使用した場合、必要なすべての環境変数が設定されます。 このツールを表示する方法については、「[Visual Studio 用開発者コマンド プロンプト](../../../framework/tools/developer-command-prompt-for-vs.md)」トピックをご覧ください。

標準のコマンド プロンプト ウィンドウを使用する場合は、コンピューター上の任意のサブディレクトリから *csc.exe* を呼び出すことができるようにパスを修正する必要があります。 また、*vsvars32.bat* を実行して、コマンド ライン ビルドをサポートするための適切な環境変数を設定する必要があります。 検索して実行する方法の手順など、*vsvars32.bat* の詳細については、「[Visual Studio のコマンドラインのための環境変数を設定する方法](./how-to-set-environment-variables-for-the-visual-studio-command-line.md)」を参照してください。

Windows Software Development Kit (SDK) のみがインストールされているコンピューターでは、**SDK コマンド プロンプト** ( **[Microsoft .NET Framework SDK]** メニュー オプションから開くことができます) で C# コンパイラを使用できます。

MSBuild を使用して、プログラムによって C# プログラムをビルドすることもできます。 詳細については、「[MSBuild](/visualstudio/msbuild/msbuild)」を参照してください。

通常、実行可能ファイル *csc.exe* は、\\Windows *ディレクトリの Microsoft.NET\Framework\<* *バージョン>* フォルダーに格納されています。 ただし、この格納場所は、特定のコンピューターの構成によって異なる場合があります。 複数のバージョンの .NET Framework がコンピューターにインストールされている場合、このファイルのバージョンが複数見つかります。 このようなインストールの詳細については、「[方法: インストールされている .NET Framework バージョンを確認する](../../../framework/migration-guide/how-to-determine-which-versions-are-installed.md)」を参照してください。

> [!TIP]
> Visual Studio IDE を使用してプロジェクトをビルドすると、**csc** コマンドとその関連するコンパイラ オプションが**出力**ウィンドウに表示されます。 この情報を表示するには、「[方法: ビルド ログ ファイルを表示、保存、および構成する](/visualstudio/ide/how-to-view-save-and-configure-build-log-files#to-change-the-amount-of-information-included-in-the-build-log)」の手順に従って、ログ データの詳細レベルを **[標準]** または **[詳細]** に変更します。 プロジェクトをリビルドした後、C# コンパイラの呼び出しを見つけるために**出力**ウィンドウで **csc** を検索します。

 **このトピックの内容**

- [コマンド ライン構文の規則](#rules-for-command-line-syntax-for-the-c-compiler)

- [サンプル コマンド ライン](#sample-command-lines-for-the-c-compiler)

- [C# コンパイラと C++ コンパイラの出力の相違点](#differences-between-c-compiler-and-c-compiler-output)

## <a name="rules-for-command-line-syntax-for-the-c-compiler"></a>C# コンパイラのコマンド ライン構文の規則

C# コンパイラは、オペレーティング システムのコマンド ラインで指定された引数を次の規則に従って解釈します。

- 引数は、空白 (スペースまたはタブ) で区切ります。

- キャレット (^) は、エスケープ文字やデリミターとしては認識されません。 キャレットは、オペレーティング システムのコマンド ライン パーサーによって処理されてからプログラムの `argv` 配列に渡されます。

- 二重引用符で囲まれた文字列 ("string") は、空白を含む場合でも、単一の引数と見なされます。 二重引用符で囲んだ文字列を引数に埋め込むこともできます。

- 円記号を前に付けた二重引用符 (\\") は、リテラル二重引用符文字 (") として解釈されます。

- 二重引用符の直前にある円記号以外は、円記号 (\\) として解釈されます。

- 二重引用符の直前に円記号が偶数個 (0 個は含まない) あると、円記号のペアごとに 1 個の円記号が `argv` 配列に格納されます。この場合、二重引用符は文字列のデリミターとして解釈されます。

- 二重引用符の直前に円記号が奇数個 (3 個以上) あると、円記号のペアごとに 1 個の円記号が `argv` 配列に格納されます。この場合、二重引用符は残った円記号によりエスケープ シーケンスになります。 これにより、リテラル二重引用符 (") が `argv` に追加されます。

## <a name="sample-command-lines-for-the-c-compiler"></a>C# コンパイラのコマンド ラインのサンプル

- *File.cs* をコンパイルして *File.exe* を作成します。

  ```console
  csc File.cs
  ```

- *File.cs* をコンパイルして *File.dll* を作成します。

  ```console
  csc -target:library File.cs
  ```

- *File.cs* をコンパイルして *My.exe* を作成します。

  ```console
  csc -out:My.exe File.cs
  ```

- 最適化を有効にし、DEBUG シンボルを定義して、現在のディレクトリにあるすべての C# ファイルをコンパイルします。 *File2.exe* が出力されます。

  ```console
  csc -define:DEBUG -optimize -out:File2.exe *.cs
  ```

- 現在のディレクトリにあるすべての C# ファイルをコンパイルして、デバッグ バージョンの *File2.dll* を作成します。 ロゴや警告は表示されません。

  ```console
  csc -target:library -out:File2.dll -warn:0 -nologo -debug *.cs
  ```

- 現在のディレクトリにあるすべての C# ファイルをコンパイルして、*Something.xyz* (DLL) に出力します。

  ```console
  csc -target:library -out:Something.xyz *.cs
  ```

## <a name="differences-between-c-compiler-and-c-compiler-output"></a>C# コンパイラと C++ コンパイラの出力の相違点

C# コンパイラを起動してもオブジェクト ( *.obj*) ファイルは作成されず、出力ファイルが直接作成されます。 このため、C# コンパイラにはリンカーが不要です。

## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [アルファベット順の C# コンパイラ オプションの一覧](./listed-alphabetically.md)
- [カテゴリ別の C# コンパイラ オプションの一覧](./listed-by-category.md)
- [Main() とコマンドライン引数](../../programming-guide/main-and-command-args/index.md)
- [コマンド ライン引数](../../programming-guide/main-and-command-args/command-line-arguments.md)
- [コマンド ライン引数を表示する方法](../../programming-guide/main-and-command-args/how-to-display-command-line-arguments.md)
- [Main() の戻り値](../../programming-guide/main-and-command-args/main-return-values.md)
