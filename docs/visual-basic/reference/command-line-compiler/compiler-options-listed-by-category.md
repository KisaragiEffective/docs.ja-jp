---
title: カテゴリ別のコンパイラ オプション
ms.date: 04/12/2018
helpviewer_keywords:
- Visual Basic compiler, options
ms.assetid: fbe36f7a-7cfa-4f77-a8d4-2be5958568e3
ms.openlocfilehash: 8b6c142041024e672fe42c8c6f2d3ebe7b07cd65
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344805"
---
# <a name="visual-basic-compiler-options-listed-by-category"></a>カテゴリ別のコンパイラオプションの一覧 Visual Basic
Visual Basic コマンドラインコンパイラは、Visual Studio 統合開発環境 (IDE) 内からプログラムをコンパイルするための代替手段として提供されています。 次に示すのは、機能カテゴリ別に並べ替えられた Visual Basic コマンドラインコンパイラオプションの一覧です。  

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]
  
## <a name="compiler-output"></a>コンパイラの出力  
  
|オプション|用途|  
|---|---|  
|[-nologo](../../../visual-basic/reference/command-line-compiler/nologo.md)|コンパイラの著作権情報が表示されないようにします。|  
|[-utf8output](../../../visual-basic/reference/command-line-compiler/utf8output.md)|UTF-8 エンコードを使用してコンパイラ出力を表示します。|  
|[-verbose](../../../visual-basic/reference/command-line-compiler/verbose.md)|コンパイル中に追加の情報を出力します。|  
|`-modulename:<string>`|ソース モジュールの名前を指定します。|  
|[-preferreduilang](../../../csharp/language-reference/compiler-options/preferreduilang-compiler-option.md)|コンパイラ出力用の言語を指定します。|
  
## <a name="optimization"></a>Optimization  
  
|オプション|用途|  
|---|---|  
|[-filealign](../../../visual-basic/reference/command-line-compiler/filealign.md)|出力ファイルでセクションをアラインするサイズを指定します。|  
|[-optimize](../../../visual-basic/reference/command-line-compiler/optimize.md)|最適化を有効または無効にします。|  
  
## <a name="output-files"></a>出力ファイル  
  
|オプション|用途|  
|---|---|  
|[-doc](../../../visual-basic/reference/command-line-compiler/doc.md)|ドキュメント コメントを XML ファイルに出力します。|  
|[-deterministic](../../../visual-basic/reference/command-line-compiler/deterministic.md)|入力が同一である場合、バイナリ コンテンツがコンパイル全体で同一のアセンブリをコンパイラに出力させます。|
|[-netcf](../../../visual-basic/reference/command-line-compiler/netcf.md)|.NET Compact Framework を対象とするようにコンパイラを設定します。|  
|[-out](../../../visual-basic/reference/command-line-compiler/out.md)|出力ファイルを指定します。|  
|[-refonly](refonly-compiler-option.md)|参照アセンブリのみを出力します。|
|[-refout](refout-compiler-option.md)|参照アセンブリの出力パスを指定します。|
|[-target](../../../visual-basic/reference/command-line-compiler/target.md)|出力の形式を指定します。|  
  
## <a name="net-assemblies"></a>.NET アセンブリ  
  
|オプション|用途|  
|---|---|  
|[-addmodule](../../../visual-basic/reference/command-line-compiler/addmodule.md)|指定ファイル内のすべての型情報を現在のコンパイル対象のプロジェクトで使用できるようにします。|  
|[-delaysign](../../../visual-basic/reference/command-line-compiler/delaysign.md)|アセンブリに完全に署名するか、部分的に署名するかを指定します。|  
|[-imports](../../../visual-basic/reference/command-line-compiler/imports.md)|指定したアセンブリから名前空間をインポートします。|  
|[-keycontainer](../../../visual-basic/reference/command-line-compiler/keycontainer.md)|アセンブリに厳密な名前を付けるキー ペアのキー コンテナー名を指定します。|  
|[-keyfile](../../../visual-basic/reference/command-line-compiler/keyfile.md)|アセンブリに厳密な名前を付けるキーまたはキー ペアを含むファイルを指定します。|  
|[-libpath](../../../visual-basic/reference/command-line-compiler/libpath.md)|[-Reference](../../../visual-basic/reference/command-line-compiler/reference.md)オプションによって参照されるアセンブリの場所を指定します。|  
|[-reference](../../../visual-basic/reference/command-line-compiler/reference.md)|アセンブリからメタデータをインポートします。|  
|[-moduleassemblyname](../../../visual-basic/reference/command-line-compiler/moduleassemblyname.md)|モジュールが一部となるアセンブリの名前を指定します。|  
|`-analyzer`|このアセンブリからアナライザーを実行します (短縮形: -a)。|  
|`-additionalfile`|コードの生成に直接影響はないが、エラーまたは警告を生成するためにアナライザーが使用できる追加のファイルを指定します。|  
  
## <a name="debuggingerror-checking"></a>デバッグ/エラーチェック  
  
|オプション|用途|  
|---|---|  
|[-bugreport](../../../visual-basic/reference/command-line-compiler/bugreport.md)|バグを簡単に報告するための情報を含むファイルを作成します。|  
|[-debug](../../../visual-basic/reference/command-line-compiler/debug.md)|デバッグ情報を生成します。|  
|[-nowarn](../../../visual-basic/reference/command-line-compiler/nowarn.md)|警告を生成するコンパイラの機能を無効にします。|  
|[-quiet](../../../visual-basic/reference/command-line-compiler/quiet.md)|コンパイラで構文関連のエラーと警告のコードが表示されないようにします。|  
|[-removeintchecks](../../../visual-basic/reference/command-line-compiler/removeintchecks.md)|整数オーバーフローのチェックを無効にします。|  
|[-warnaserror](../../../visual-basic/reference/command-line-compiler/warnaserror.md)|警告をエラーに昇格します。|  
|`-ruleset:<file>`|特定の診断を無効にするルールセット ファイルを指定します。|  
  
## <a name="help"></a>Help  
  
|オプション|用途|  
|---|---|  
|[-?](../../../visual-basic/reference/command-line-compiler/help.md)|コンパイラ オプションを出力します。 このコマンドは、`-help` オプションの指定と同じです。 コンパイルは発生しません。|  
|[-help](../../../visual-basic/reference/command-line-compiler/help.md)|コンパイラ オプションを出力します。 このコマンドは、`-?` オプションの指定と同じです。 コンパイルは発生しません。|  
  
## <a name="language"></a>[言語]  
  
|オプション|用途|  
|---|---|  
|[-langversion](../../../visual-basic/reference/command-line-compiler/langversion.md)|言語バージョンを指定し&#124;ます&#124;:&#124;9&#124;9.0&#124;10 10.0 11 11.0。|  
|[-optionexplicit](../../../visual-basic/reference/command-line-compiler/optionexplicit.md)|変数の明示的な宣言を強制的に適用します。|  
|[-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)|厳密な型のセマンティクスを強制的に適用します。|  
|[-optioncompare](../../../visual-basic/reference/command-line-compiler/optioncompare.md)|文字列比較をバイナリにするか、ロケール固有のテキストのセマンティクスを使用するかどうかを指定します。|  
|[-optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)|変数宣言でローカル型推論を使用できるようにします。|  
  
## <a name="preprocessor"></a>プリプロセッサ  
  
|オプション|用途|  
|---|---|  
|[-define](../../../visual-basic/reference/command-line-compiler/define.md)|条件付きコンパイルのシンボルを定義します。|  
  
## <a name="resources"></a>リソース  
  
|オプション|用途|  
|---|---|  
|[-linkresource](../../../visual-basic/reference/command-line-compiler/linkresource.md)|マネージド リソースへのリンクを作成します。|  
|[-resource](../../../visual-basic/reference/command-line-compiler/resource.md)|マネージド リソースをアセンブリに埋め込みます。|  
|[-win32icon](../../../visual-basic/reference/command-line-compiler/win32icon.md)|.ico ファイルを出力ファイルに挿入します。|  
|[-win32resource](../../../visual-basic/reference/command-line-compiler/win32resource.md)|Win32 リソースを出力ファイルに挿入します。|  
  
## <a name="miscellaneous"></a>その他  
  
|オプション|用途|  
|---|---|  
|[@ (応答ファイルの指定)](../../../visual-basic/reference/command-line-compiler/specify-response-file.md)|応答ファイルを指定します。|  
|[-baseaddress](../../../visual-basic/reference/command-line-compiler/baseaddress.md)|DLL のベース アドレスを指定します。|  
|[-codepage](../../../visual-basic/reference/command-line-compiler/codepage.md)|コンパイルですべてのソース コード ファイルに使用するコード ページを指定します。|  
|[-errorreport](../../../visual-basic/reference/command-line-compiler/errorreport.md)|Visual Basic コンパイラが内部コンパイラエラーを報告する方法を指定します。|  
|[-highentropyva](../../../visual-basic/reference/command-line-compiler/highentropyva.md)|特定の実行可能ファイルで高エントロピ ASLR (Address Space Layout Randomization) をサポートするかどうかを Windows カーネルに示します。|  
|[-main](../../../visual-basic/reference/command-line-compiler/main.md)|起動時に使用する `Sub Main` プロシージャを含むクラスを指定します。|  
|[-noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)|コンパイルに Vbc.rsp が使用されないようにします。|  
|[-nostdlib](../../../visual-basic/reference/command-line-compiler/nostdlib.md)|コンパイラが標準ライブラリを参照しないようにします。|  
|[-nowin32manifest](../../../visual-basic/reference/command-line-compiler/nowin32manifest.md)|アプリケーション マニフェストを実行可能ファイルに埋め込まないようコンパイラに指定します。|  
|[-platform](../../../visual-basic/reference/command-line-compiler/platform.md)|コンパイラによる出力ファイルの対象となるプロセッサ プラットフォームを指定します。|  
|[-recurse](../../../visual-basic/reference/command-line-compiler/recurse.md)|コンパイルするソース ファイルをサブディレクトリで検索します。|  
|[-rootnamespace](../../../visual-basic/reference/command-line-compiler/rootnamespace.md)|すべての型宣言に対して名前空間を指定します。|  
|[-sdkpath](../../../visual-basic/reference/command-line-compiler/sdkpath.md)|Mscorlib.dll および Microsoft.VisualBasic.dll の位置を指定します。|  
|[-vbruntime](../../../visual-basic/reference/command-line-compiler/vbruntime.md)|コンパイラが Visual Basic Runtime Library を参照せずにコンパイルするか、特定のランタイム ライブラリを参照してコンパイルするかを指定します。|  
|[-win32manifest](../../../visual-basic/reference/command-line-compiler/win32manifest.md)|プロジェクトのポータブル実行可能 (PE) ファイルに埋め込まれる、ユーザー定義の Win32 アプリケーション マニフェスト ファイルを識別します。|  
|`-parallel[+&#124;-]`|同時実行ビルドを使用する (+) かどうかを指定します。|  
|`-checksumalgorithm:<alg>`|PDB に格納されているソース ファイルのチェックサムを計算するためのアルゴリズムを指定します。  サポートされる値は、SHA1 (既定値) または SHA256 です。 <br>SHA1 の衝突の問題のため、SHA256 以上をお勧めします。|  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コンパイラ オプション一覧 (アルファベット順)](../../../visual-basic/reference/command-line-compiler/compiler-options-listed-alphabetically.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
