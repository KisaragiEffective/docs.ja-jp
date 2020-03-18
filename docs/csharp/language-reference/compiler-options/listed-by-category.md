---
title: カテゴリ別の C# コンパイラ オプションの一覧
ms.date: 05/15/2018
helpviewer_keywords:
- Visual C# compiler, options listed by category
- compiler options [C#], listed by category
- Visual C#, compiler options listed by category
ms.assetid: 96437ecc-6502-4cd3-b070-e9386a298e83
ms.openlocfilehash: 5cd5607c25dabd8f56ebb58366116666e8e649ea
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73972714"
---
# <a name="c-compiler-options-listed-by-category"></a>カテゴリ別の C# コンパイラ オプションの一覧

次のコンパイラ オプションは、カテゴリ別に並んでいます。 アルファベット順の一覧については、「[アルファベット順の C# コンパイラ オプションの一覧](listed-alphabetically.md)」を参照してください。

## <a name="optimization"></a>Optimization

|オプション|目的|
|------------|-------------|
|[-filealign](filealign-compiler-option.md)|出力ファイル内のセクションのサイズを指定します。|
|[-optimize](optimize-compiler-option.md)|最適化を有効または無効にします。|

## <a name="output-files"></a>出力ファイル

|オプション|目的|
|------------|-------------|
|[-deterministic](deterministic-compiler-option.md)|入力が同一である場合、バイナリ コンテンツがコンパイル全体で同一のアセンブリをコンパイラに出力させます。|
|[-doc](doc-compiler-option.md)|処理されたドキュメントのコメントが書き込まれる XML ファイルを指定します。|
|[-out](out-compiler-option.md)|出力ファイルを指定します。|
|[-pathmap](pathmap-compiler-option.md)|コンパイラによるソース パス名出力のマッピングを指定します|
|[/pdb](pdb-compiler-option.md)|.pdb ファイルの名前と場所を指定します。|
|[-platform](platform-compiler-option.md)|出力プラットフォームを指定します。|
|[/preferreduilang](preferreduilang-compiler-option.md)|コンパイラ出力用の言語を指定します。|
|[/refout](refout-compiler-option.md)|プライマリ アセンブリだけでなく、参照アセンブリを生成します。|
|[/refonly](refonly-compiler-option.md)|プライマリ アセンブリの代わりに、参照アセンブリを生成します。|
|[-target](target-compiler-option.md)|6 つのオプション ([-target:appcontainerexe](target-appcontainerexe-compiler-option.md)、[-target:exe](target-exe-compiler-option.md)、[-target:library](target-library-compiler-option.md)、[-target:module](target-module-compiler-option.md)、[-target:winexe](target-winexe-compiler-option.md)、[-target:winmdobj](target-winmdobj-compiler-option.md)) のいずれかを使用して、出力ファイルの形式を指定します。|
|-modulename:\<string>|ソース モジュールの名前を指定します。|

## <a name="net-framework-assemblies"></a>.NET Framework アセンブリ

|オプション|目的|
|------------|-------------|
|[-addmodule](addmodule-compiler-option.md)|このアセンブリを構成する 1 つ以上のモジュールを指定します。|
|[-delaysign](delaysign-compiler-option.md)|公開キーを追加し、アセンブリには署名しないでおくようコンパイラに指示します。|
|[-keycontainer](keycontainer-compiler-option.md)|暗号化キー コンテナーの名前を指定します。|
|[-keyfile](keyfile-compiler-option.md)|暗号化キーを格納するファイル名を指定します。|
|[/lib](lib-compiler-option.md)|[-reference](reference-compiler-option.md) で参照されるアセンブリの場所を指定します。|
|[-nostdlib](nostdlib-compiler-option.md)|標準ライブラリ (mscorlib.dll) をインポートしないようコンパイラに指示します。|
|[-publicsign](publicsign-compiler-option.md)|アセンブリに署名しないで公開キーを適用しますが、アセンブリが署名されていることを示すアセンブリのビットを設定します。|
|[-reference](reference-compiler-option.md)|アセンブリが格納されているファイルからメタデータをインポートします。|
|-analyzer|このアセンブリからアナライザーを実行します (短縮形: /a)。|
|-additionalfile|コードの生成に直接影響はないが、エラーまたは警告を生成するためにアナライザーが使用できる追加のファイルを指定します。|
|-embed|すべてのソース ファイルを PDB に埋め込みます。|
|-embed:\<file list>|特定のファイルを PDB に埋め込みます。|
## <a name="debuggingerror-checking"></a>デバッグ/エラー チェック

|オプション|目的|
|------------|-------------|
|[-bugreport](bugreport-compiler-option.md)|バグを簡単に報告するための情報を含むファイルを作成します。|
|[/checked](checked-compiler-option.md)|データ型の境界をオーバーフローする整数演算で、実行時に例外を発生させるかどうかを指定します。|
|[-debug](debug-compiler-option.md)|デバッグ情報を生成するようコンパイラに指示します。|
|[-errorreport](errorreport-compiler-option.md)|エラー報告の動作を設定します。|
|[/fullpaths](fullpaths-compiler-option.md)|コンパイラ出力に含まれるファイルの絶対パスを指定します。|
|[-nowarn](nowarn-compiler-option.md)|指定した警告がコンパイラで生成されないようにします。|
|[/warn](warn-compiler-option.md)|警告レベルを設定します。|
|[-warnaserror](warnaserror-compiler-option.md)|警告をエラーに昇格します。|
|-ruleset:\<file>|特定の診断を無効にするルールセット ファイルを指定します。|

## <a name="preprocessor"></a>プリプロセッサ

|オプション|目的|
|------------|-------------|
|[-define](define-compiler-option.md)|プリプロセッサ シンボルを定義します。|

## <a name="resources"></a>リソース

|オプション|目的|
|------------|-------------|
|[-link](link-compiler-option.md)|指定したアセンブリ内の COM 型情報をプロジェクトで使用できるようにします。|
|[-linkresource](linkresource-compiler-option.md)|マネージド リソースへのリンクを作成します。|
|[-resource](resource-compiler-option.md)|.NET Framework のリソースを出力ファイルに埋め込みます。|
|[-win32icon](win32icon-compiler-option.md)|出力ファイルに挿入する .ico ファイルを指定します。|
|[/win32res:](win32res-compiler-option.md)|出力ファイルに挿入する Win32 リソースを指定します。|

## <a name="miscellaneous"></a>その他

|オプション|目的|
|------------|-------------|
|[@](response-file-compiler-option.md)|応答ファイルを指定します。|
|[-?](help-compiler-option.md)|stdout にコンパイラ オプションの一覧を表示します。|
|[-baseaddress](baseaddress-compiler-option.md)|DLL を読み込む位置に推奨されるベース アドレスを指定します。|
|[-codepage](codepage-compiler-option.md)|コンパイルですべてのソース コード ファイルに使用するコード ページを指定します。|
|[-help](help-compiler-option.md)|stdout にコンパイラ オプションの一覧を表示します。|
|[-highentropyva](highentropyva-compiler-option.md)|実行可能ファイルが ASLR (Address Space Layout Randomization) をサポートするように指定します。|
|[-langversion](langversion-compiler-option.md)|言語のバージョンを指定します: Default、ISO-1、ISO-2、3、4、5、6、7、7.1、7.2、7.3、Latest |
|[-main](main-compiler-option.md)|**Main** メソッドの場所を指定します。|
|[-noconfig](noconfig-compiler-option.md)|csc.rsp でコンパイルにしないようコンパイラに指示します。|
|[-nologo](nologo-compiler-option.md)|コンパイラの著作権情報が表示されないようにします。|
|[-recurse](recurse-compiler-option.md)|コンパイルするソース ファイルをサブディレクトリで検索します。|
|[-subsystemversion](subsystemversion-compiler-option.md)|実行可能ファイルが使用できるサブシステムの最低限のバージョンを指定します。|
|[/unsafe](unsafe-compiler-option.md)|[unsafe](../keywords/unsafe.md) キーワードを使用するコードのコンパイルを有効にします。|
|[-utf8output](utf8output-compiler-option.md)|UTF-8 エンコードを使用してコンパイラ出力を表示します。|
|-parallel[+&#124;-]|同時実行ビルドを使用する (+) かどうかを指定します。|
|-checksumalgorithm:\<alg>|PDB に格納されているソース ファイルのチェックサムを計算するためのアルゴリズムを指定します。  サポートされる値は、SHA1 (既定値) または SHA256 です。<br>SHA1 との競合問題のため、Microsoft では SHA256 を推奨しています。|

## <a name="obsolete-options"></a>廃止されたオプション

|オプション|目的|
|---|---|
|-incremental|インクリメンタル コンパイルを有効にします。|

## <a name="see-also"></a>参照

- [C# コンパイラ オプション](index.md)
- [アルファベット順の C# コンパイラ オプションの一覧](listed-alphabetically.md)
- [Visual Studio のコマンドラインのための環境変数を設定する方法](how-to-set-environment-variables-for-the-visual-studio-command-line.md)
