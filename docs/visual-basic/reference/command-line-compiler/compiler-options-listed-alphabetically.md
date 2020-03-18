---
title: アルファベット順のコンパイラ オプション
ms.date: 04/12/2018
helpviewer_keywords:
- Visual Basic compiler, options
ms.assetid: e67febba-bacf-4e1f-a143-c141e063f90e
ms.openlocfilehash: 85fb07f46c2d885491db7358f24b3b50836c2ca8
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937758"
---
# <a name="visual-basic-compiler-options-listed-alphabetically"></a>Visual Basic コンパイラ オプション一覧 (アルファベット順)
Visual Basic コマンドラインコンパイラは、Visual Studio 統合開発環境 (IDE) からプログラムをコンパイルするための代替手段として提供されています。 アルファベット順に並べ替えられた Visual Basic のコマンドラインコンパイラオプションの一覧を次に示します。  

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]
  
|オプション|目的|  
|------------|-------------|  
|[@ (応答ファイルの指定)](../../../visual-basic/reference/command-line-compiler/specify-response-file.md)|応答ファイルを指定します。|  
|[-?](../../../visual-basic/reference/command-line-compiler/help.md)|コンパイラ オプションを出力します。 このコマンドは、`-help` オプションの指定と同じです。 コンパイルは発生しません。|  
|`-additionalfile`|コードの生成に直接影響はないが、エラーまたは警告を生成するためにアナライザーが使用できる追加のファイルを指定します。|  
|[-addmodule](../../../visual-basic/reference/command-line-compiler/addmodule.md)|指定ファイル内のすべての型情報を現在のコンパイル対象のプロジェクトで使用できるようにします。|  
|`-analyzer`|このアセンブリからアナライザーを実行します (短縮形: -a)。|  
|[-baseaddress](../../../visual-basic/reference/command-line-compiler/baseaddress.md)|DLL のベース アドレスを指定します。|  
|[-bugreport](../../../visual-basic/reference/command-line-compiler/bugreport.md)|バグを簡単に報告するための情報を含むファイルを作成します。|  
|`-checksumalgorithm:<alg>`|PDB に格納されているソース ファイルのチェックサムを計算するためのアルゴリズムを指定します。  サポートされる値は、SHA1 (既定値) または SHA256 です。 <br>SHA1 の衝突の問題のため、SHA256 以上をお勧めします。|  
|[-codepage](../../../visual-basic/reference/command-line-compiler/codepage.md)|コンパイルですべてのソース コード ファイルに使用するコード ページを指定します。|  
|[-debug](../../../visual-basic/reference/command-line-compiler/debug.md)|デバッグ情報を生成します。|  
|[-define](../../../visual-basic/reference/command-line-compiler/define.md)|条件付きコンパイルのシンボルを定義します。|  
|[-delaysign](../../../visual-basic/reference/command-line-compiler/delaysign.md)|アセンブリに完全に署名するか、部分的に署名するかを指定します。|  
|[-deterministic](../../../visual-basic/reference/command-line-compiler/deterministic.md)|入力が同一である場合、バイナリ コンテンツがコンパイル全体で同一のアセンブリをコンパイラに出力させます。|
|[-doc](../../../visual-basic/reference/command-line-compiler/doc.md)|ドキュメント コメントを XML ファイルに出力します。|  
|[-errorreport](../../../visual-basic/reference/command-line-compiler/errorreport.md)|Visual Basic コンパイラが内部コンパイラエラーを報告する方法を指定します。|  
|[-filealign](../../../visual-basic/reference/command-line-compiler/filealign.md)|出力ファイルでセクションをアラインするサイズを指定します。|  
|[-help](../../../visual-basic/reference/command-line-compiler/help.md)|コンパイラ オプションを出力します。 このコマンドは、`-?` オプションの指定と同じです。 コンパイルは発生しません。|  
|[-highentropyva](../../../visual-basic/reference/command-line-compiler/highentropyva.md)|特定の実行可能ファイルで高エントロピ ASLR (Address Space Layout Randomization) をサポートするかどうかを示します。|  
|[-imports](../../../visual-basic/reference/command-line-compiler/imports.md)|指定したアセンブリから名前空間をインポートします。|  
|[-keycontainer](../../../visual-basic/reference/command-line-compiler/keycontainer.md)|アセンブリに厳密な名前を付けるキー ペアのキー コンテナー名を指定します。|  
|[-keyfile](../../../visual-basic/reference/command-line-compiler/keyfile.md)|アセンブリに厳密な名前を付けるキーまたはキー ペアを含むファイルを指定します。|  
|[-langversion](../../../visual-basic/reference/command-line-compiler/langversion.md)|言語バージョンを指定し&#124;ます&#124;:&#124;9&#124;9.0&#124;10 10.0 11 11.0。|  
|[-libpath](../../../visual-basic/reference/command-line-compiler/libpath.md)|[-Reference](../../../visual-basic/reference/command-line-compiler/reference.md)オプションによって参照されるアセンブリの場所を指定します。|  
|[-linkresource](../../../visual-basic/reference/command-line-compiler/linkresource.md)|マネージド リソースへのリンクを作成します。|  
|[-main](../../../visual-basic/reference/command-line-compiler/main.md)|起動時に使用する `Sub Main` プロシージャを含むクラスを指定します。|  
|[-moduleassemblyname](../../../visual-basic/reference/command-line-compiler/moduleassemblyname.md)|モジュールが一部となるアセンブリの名前を指定します。|  
|`-modulename:<string>`|ソース モジュールの名前を指定します。|  
|[-netcf](../../../visual-basic/reference/command-line-compiler/netcf.md)|.NET Compact Framework を対象とするようにコンパイラを設定します。|  
|[-noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)|Vbc.rsp でコンパイルしないでください。|  
|[-nologo](../../../visual-basic/reference/command-line-compiler/nologo.md)|コンパイラの著作権情報が表示されないようにします。|  
|[-nostdlib](../../../visual-basic/reference/command-line-compiler/nostdlib.md)|コンパイラが標準ライブラリを参照しないようにします。|  
|[-nowarn](../../../visual-basic/reference/command-line-compiler/nowarn.md)|警告を生成するコンパイラの機能を無効にします。|  
|[-nowin32manifest](../../../visual-basic/reference/command-line-compiler/nowin32manifest.md)|アプリケーション マニフェストを実行可能ファイルに埋め込まないようコンパイラに指定します。|  
|[-optimize](../../../visual-basic/reference/command-line-compiler/optimize.md)|コードの最適化を有効または無効にします。|  
|[-optioncompare](../../../visual-basic/reference/command-line-compiler/optioncompare.md)|文字列比較をバイナリにするか、ロケール固有のテキストのセマンティクスを使用するかどうかを指定します。|  
|[-optionexplicit](../../../visual-basic/reference/command-line-compiler/optionexplicit.md)|変数の明示的な宣言を強制的に適用します。|  
|[-optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)|変数宣言でローカル型推論を使用できるようにします。|  
|[-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)|厳密な言語セマンティクスを適用します。|  
|[-out](../../../visual-basic/reference/command-line-compiler/out.md)|出力ファイルを指定します。|  
<code>-parallel[+&#124;-]</code> 

|[-platform](../../../visual-basic/reference/command-line-compiler/platform.md)|コンパイラによる出力ファイルの対象となるプロセッサ プラットフォームを指定します。|  
|`-preferreduilang`|出力用の言語名を指定します。|  
|[-quiet](../../../visual-basic/reference/command-line-compiler/quiet.md)|コンパイラで構文関連のエラーと警告のコードが表示されないようにします。|  
|[-recurse](../../../visual-basic/reference/command-line-compiler/recurse.md)|コンパイルするソース ファイルをサブディレクトリで検索します。|  
|[-reference](../../../visual-basic/reference/command-line-compiler/reference.md)|アセンブリからメタデータをインポートします。|  
|[/refonly](refonly-compiler-option.md)|参照アセンブリのみを出力します。|
|[/refout](refout-compiler-option.md)|参照アセンブリの出力パスを指定します。|
|[-removeintchecks](../../../visual-basic/reference/command-line-compiler/removeintchecks.md)|整数オーバーフローのチェックを無効にします。|  
|[-resource](../../../visual-basic/reference/command-line-compiler/resource.md)|マネージド リソースをアセンブリに埋め込みます。|  
|[-rootnamespace](../../../visual-basic/reference/command-line-compiler/rootnamespace.md)|すべての型宣言に対して名前空間を指定します。|  
|`-ruleset:<file>`|特定の診断を無効にするルールセット ファイルを指定します。|  
|[-sdkpath](../../../visual-basic/reference/command-line-compiler/sdkpath.md)|Mscorlib.dll および Microsoft.VisualBasic.dll の位置を指定します。|  
|[-subsystemversion](../../../visual-basic/reference/command-line-compiler/subsystemversion.md)|生成された実行可能ファイルが使用できるサブシステムの最低限のバージョンを指定します。|  
|[-target](../../../visual-basic/reference/command-line-compiler/target.md)|出力ファイルの形式を指定します。|  
|[-utf8output](../../../visual-basic/reference/command-line-compiler/utf8output.md)|UTF-8 エンコードを使用してコンパイラ出力を表示します。|  
|[-vbruntime](../../../visual-basic/reference/command-line-compiler/vbruntime.md)|コンパイラが Visual Basic Runtime Library を参照せずにコンパイルするか、特定のランタイム ライブラリを参照してコンパイルするかを指定します。|  
|[-verbose](../../../visual-basic/reference/command-line-compiler/verbose.md)|コンパイル中に追加の情報を出力します。|  
|[-warnaserror](../../../visual-basic/reference/command-line-compiler/warnaserror.md)|警告をエラーに昇格します。|  
|[-win32icon](../../../visual-basic/reference/command-line-compiler/win32icon.md)|.ico ファイルを出力ファイルに挿入します。|  
|[-win32manifest](../../../visual-basic/reference/command-line-compiler/win32manifest.md)|プロジェクトのポータブル実行可能 (PE) ファイルに埋め込まれる、ユーザー定義の Win32 アプリケーション マニフェスト ファイルを識別します。|  
|[-win32resource](../../../visual-basic/reference/command-line-compiler/win32resource.md)|Win32 リソースを出力ファイルに挿入します。|  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コンパイラ オプション一覧 (カテゴリ別)](../../../visual-basic/reference/command-line-compiler/compiler-options-listed-by-category.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
