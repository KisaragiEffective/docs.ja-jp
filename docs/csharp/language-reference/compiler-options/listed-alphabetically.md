---
title: アルファベット順の C# コンパイラ オプションの一覧
ms.date: 05/15/2018
helpviewer_keywords:
- compiler options [C#], listed alphabetically
- C# language, compiler options listed alphabetically
- Visual C# compiler, options listed alphabetically
- Visual C#, compiler options listed alphabetically
ms.assetid: 43535ea0-ca47-4a15-b528-615087a86092
ms.openlocfilehash: d6d471cd27f35de6325a130e6c909d13cb1dcc85
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73972739"
---
# <a name="c-compiler-options-listed-alphabetically"></a>アルファベット順の C# コンパイラ オプションの一覧

次のコンパイラ オプションは、アルファベット順に並んでいます。 カテゴリ別の一覧については、「[カテゴリ別の C# コンパイラ オプションの一覧](listed-by-category.md)」をご覧ください。

|オプション|目的|
|------------|-------------|
|[@](response-file-compiler-option.md)|応答ファイルを読み込んで、オプションを追加します。|
|[-?](help-compiler-option.md)|使用法に関する説明を標準出力に表示します。|
|-additionalfile|コードの生成に直接影響はないが、エラーまたは警告を生成するためにアナライザーが使用できる追加のファイルを指定します。|
|[-addmodule](addmodule-compiler-option.md)|指定されたモジュールをこのアセンブリにリンクします。|
|-analyzer|このアセンブリからアナライザーを実行します (短縮形: -a)。|
|[/appconfig](appconfig-compiler-option.md)|アセンブリのバインド時に app.config の場所を指定します。|
|[-baseaddress](baseaddress-compiler-option.md)|ビルドするライブラリのベース アドレスを指定します。|
|[-bugreport](bugreport-compiler-option.md)|"障害報告" ファイルを作成します。 -errorreport:prompt または -errorreport:send と組み合わせて使用した場合、このファイルがクラッシュ情報と共に送信されます。|
|[/checked](checked-compiler-option.md)|オーバーフロー チェックを生成するようコンパイラに指示します。|
|-checksumalgorithm:\<alg>|PDB に格納されているソース ファイルのチェックサムを計算するためのアルゴリズムを指定します。  サポートされる値は SHA256 (既定) または SHA1 です。<br>SHA1 との競合問題のため、Microsoft では SHA256 を推奨しています。 |
|[-codepage](codepage-compiler-option.md)|ソース ファイルを開くときに使用するコードページを指定します。|
|[-debug](debug-compiler-option.md)|デバッグ情報を生成します。|
|[-define](define-compiler-option.md)|条件付きコンパイル シンボルを定義します。|
|[-delaysign](delaysign-compiler-option.md)|厳密名キーのパブリックな部分のみを使ってアセンブリに遅延署名します。|
|[-deterministic](deterministic-compiler-option.md)|入力が同一である場合、バイナリ コンテンツがコンパイル全体で同一のアセンブリをコンパイラに出力させます。|
|[-doc](doc-compiler-option.md)|生成する XML ドキュメント ファイルを指定します。|
|-embed|すべてのソース ファイルを PDB に埋め込みます。|
|-embed:\<file list>|特定のファイルを PDB に埋め込みます。|
|-errorendlocation|各エラーの終了位置の出力行と出力列。|
|-errorlog:\<file>|コンパイラとアナライザーのすべての診断情報を記録するファイルを指定します。|
|[-errorreport](errorreport-compiler-option.md)|内部コンパイラ エラーの処理方法 (prompt、send、none) を指定します。 既定値は none です。|
|[-filealign](filealign-compiler-option.md)|出力ファイルのセクションで使用する配置を指定します。|
|[/fullpaths](fullpaths-compiler-option.md)|完全修飾パスを生成するようコンパイラに指示します。|
|[-help](help-compiler-option.md)|使用法に関する説明を標準出力に表示します。|
|[-highentropyva](highentropyva-compiler-option.md)|高エントロピ ASLR がサポートされていることを指定します。|
|-incremental|インクリメンタル コンパイルを有効にします [廃止]。|
|[-keycontainer](keycontainer-compiler-option.md)|厳密な名前のキー コンテナーを指定します。|
|[-keyfile](keyfile-compiler-option.md)|厳密な名前のキー ファイルを指定します。|
|[-langversion:\<string>](langversion-compiler-option.md)|言語バージョンとして、既定、ISO-1、ISO-2、3、4、5、6、7、7.1、7.2、7.3、または最新を指定します。 |
|[/lib](lib-compiler-option.md)|参照を検索する追加のディレクトリを指定します。|
|[-link](link-compiler-option.md)|指定したアセンブリ内の COM 型情報をプロジェクトで使用できるようにします。|
|[-linkresource](linkresource-compiler-option.md)|このアセンブリに指定されたリソースをリンクします。|
|[-main](main-compiler-option.md)|エントリ ポイントを含む型を指定します (他のエントリ ポイントはすべて無視します)。|
|[-moduleassemblyname](moduleassemblyname-compiler-option.md)|.netmodule がアクセスできる非パブリック型のアセンブリを指定します。|
|-modulename:\<string>|ソース モジュールの名前を指定します。|
|[-noconfig](noconfig-compiler-option.md)|CSC.RSP ファイルを自動で含めないようコンパイラに指示します。|
|[-nologo](nologo-compiler-option.md)|コンパイラの著作権メッセージが表示されないようにします。|
|[-nostdlib](nostdlib-compiler-option.md)|標準ライブラリ (mscorlib.dll) を参照しないようコンパイラに指示します。|
|[-nowarn](nowarn-compiler-option.md)|特定の警告メッセージを無効にします。|
|[-nowin32manifest](nowin32manifest-compiler-option.md)|アプリケーション マニフェストを実行可能ファイルに埋め込まないようコンパイラに指定します。|
|[-optimize](optimize-compiler-option.md)|最適化を有効または無効にします。|
|[-out](out-compiler-option.md)|出力ファイル名を指定します (既定では、メイン クラスまたは最初のファイルの基本名)。|
|-parallel[+&#124;-]|同時実行ビルドを使用する (+) かどうかを指定します。|
|[-pathmap](pathmap-compiler-option.md)|コンパイラによるソース パス名出力のマッピングを指定します。|
|[/pdb](pdb-compiler-option.md)|.pdb ファイルの名前と場所を指定します。|
|[-platform](platform-compiler-option.md)|このコードを実行できるプラットフォームを制限します (x86、Itanium、x64、anycpu、anycpu32bitpreferred)。 既定は anycpu です。|
|[/preferreduilang](preferreduilang-compiler-option.md)|コンパイラの出力に使用する言語を指定します。|
|[-publicsign](publicsign-compiler-option.md)|アセンブリに署名しないで公開キーを適用しますが、アセンブリが署名されていることを示すアセンブリのビットを設定します。|
|[-recurse](recurse-compiler-option.md)|ワイルドカードの指定に従い、現在のディレクトリとサブディレクトリ内のすべてのファイルをインクルードします。|
|[-reference](reference-compiler-option.md)|指定されたアセンブリ ファイルからメタデータを参照します。|
|[/refout](refout-compiler-option.md)|プライマリ アセンブリだけでなく、参照アセンブリを生成します。|
|[/refonly](refonly-compiler-option.md)|プライマリ アセンブリの代わりに、参照アセンブリを生成します。|
|-reportanalyzer|実行時間など、アナライザーの追加情報を報告します。|
|[-resource](resource-compiler-option.md)|指定したリソースを埋め込みます。|
|-ruleset:\<file>|特定の診断を無効にするルールセット ファイルを指定します。|
|[-subsystemversion](subsystemversion-compiler-option.md)|実行可能ファイルが使用できるサブシステムの最低限のバージョンを指定します。|
|[-target](target-compiler-option.md)|6 つのオプション ([-target:appcontainerexe](target-appcontainerexe-compiler-option.md)、[-target:exe](target-exe-compiler-option.md)、[-target:library](target-library-compiler-option.md)、[-target:module](target-module-compiler-option.md)、[-target:winexe](target-winexe-compiler-option.md)、[-target:winmdobj](target-winmdobj-compiler-option.md)) のいずれかを使用して、出力ファイルの形式を指定します。|
|[/unsafe](unsafe-compiler-option.md)|[アンセーフ](../keywords/unsafe.md) コードを許可します。|
|[-utf8output](utf8output-compiler-option.md)|UTF-8 エンコードでコンパイラのメッセージを出力します。|
|-version|コンパイラのバージョン番号を表示して終了します。|
|[/warn](warn-compiler-option.md)|警告レベル (0 ～ 4) を設定します。|
|[-warnaserror](warnaserror-compiler-option.md)|特定の警告をエラーとして報告します。|
|[-win32icon](win32icon-compiler-option.md)|出力に使用するアイコンを指定します。|
|[-win32manifest](win32manifest-compiler-option.md)|カスタム win32 マニフェスト ファイルを指定します。|
|[/win32res:](win32res-compiler-option.md)|win32 リソース ファイル (.res) を指定します。|

## <a name="see-also"></a>関連項目

- [C# コンパイラ オプション](index.md)
- [カテゴリ別の C# コンパイラ オプションの一覧](listed-by-category.md)
- [Visual Studio のコマンドラインのための環境変数を設定する方法](how-to-set-environment-variables-for-the-visual-studio-command-line.md)
- [\<compiler> 要素](../../../framework/configure-apps/file-schema/compiler/compiler-element.md)
