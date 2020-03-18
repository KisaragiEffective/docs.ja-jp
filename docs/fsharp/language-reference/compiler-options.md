---
title: コンパイラ オプション
description: コンパイラF#のコマンドラインオプションを使用して、 F#アプリとライブラリのコンパイルを制御します。
ms.date: 12/10/2018
ms.openlocfilehash: ecaae538a5db2f5dfefa79cb8e7b8b51d39c440d
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628879"
---
# <a name="compiler-options"></a>コンパイラ オプション

このトピックでは、F# コンパイラ (fsc.exe) のコマンド ライン オプションについて説明します。

コンパイル環境は、プロジェクトのプロパティを設定して制御することもできます。 .NET Core を対象とするプロジェクトの場合、`.fsproj`の "その他のフラグ" `<OtherFlags>...</OtherFlags>` プロパティは、余分なコマンドラインオプションを指定するために使用されます。

## <a name="compiler-options-listed-alphabetically"></a>アルファベット順のコンパイラ オプション

次の表は、コンパイラ オプションのアルファベット順の一覧です。 一部の F# コンパイラ オプションは C# コンパイラ オプションに似ています。 それらについては、C# コンパイラ オプションのトピックへのリンクも用意されています。

|コンパイラ オプション|説明|
|---------------|-----------|
|`-a filename.fs`|指定されたファイルからライブラリを生成します。 このオプションは、`--target:library filename.fs`の短い形式です。|
|`--baseaddress:address`|DLL を読み込む位置に推奨されるベース アドレスを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;baseaddress &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/2fdbz5xd.aspx)」を参照してください。|
|`--codepage:id`|必要なページがシステムの現在の既定のコードページではない場合に、コンパイル時に使用するコードページを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;コードページ&#40;&#35; C&#41;コンパイラオプション](../../csharp/language-reference/compiler-options/codepage-compiler-option.md)」を参照してください。|
|`--consolecolors`|エラーおよび警告がコンソールの色分けされたテキストを使用することを指定します。|
|`--crossoptimize[+|-]`|モジュール間の最適化を有効または無効にします。|
|<code>--delaysign[+&#124;-]</code>|厳密な名前のキーのパブリックな部分のみを使ってアセンブリに遅延署名します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;delaysign &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/ta1sxwy8.aspx)」を参照してください。|
|<code>--checked[+&#124;-]</code>|オーバーフロー チェックの生成を有効または無効にします。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;checked &#40;&#35; C&#41;Compiler Options](https://msdn.microsoft.com/library/h25wtyxf.aspx)」を参照してください。|
|<code>--debug[+&#124;-]</code><br /><br /><code>-g[+&#124;-]</code><br /><br /><code>--debug:[full&#124;pdbonly]</code><br /><br /><code>-g: [full&#124;pdbonly]</code>|デバッグ情報の生成を有効または無効にしたり、生成するデバッグ情報の種類を指定したりします。 既定値は、実行中のプログラムにアタッチできる full です。 Pdb (プログラムデータベース) ファイルに格納されている限られたデバッグ情報を取得するには、 **pdbonly**を選択します。<br /><br />同じ名前の C# コンパイラ オプションに相当します。 詳細については、 を参照してください。<br /><br />[C コンパイラオプション&#41;をデバッグ&#40;します。&#35; &#47;](https://msdn.microsoft.com/library/8cw0bt21.aspx)|
|`--define:symbol`<br /><br />`-d:symbol`|条件付きコンパイルで使用する記号を定義します。|
|<code>--deterministic[+&#124;-]</code>|決定的なアセンブリ (モジュールのバージョン GUID とタイムスタンプを含む) を生成します。 このオプションは、ワイルドカードのバージョン番号と共に使用することはできません。また、埋め込みおよびポータブルデバッグの種類のみをサポートします。|
|`--doc:xmldoc-filename`|指定したファイルに XML ドキュメント コメントを生成するようにコンパイラに指示します。 詳細については、「 [XML Documentation](xml-documentation.md)」を参照してください。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;doc &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/3260k4x7.aspx)」を参照してください。|
|`--fullpaths`|絶対パスを生成するようコンパイラに指示します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;fullpaths &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/d315xc66.aspx)」を参照してください。|
|`--help`<br /><br />`-?`|使用方法を表示します。すべてのコンパイラ オプションの簡単な説明が表示されます。|
|<code>--highentropyva[+&#124;-]</code>|強化されたセキュリティ機能である、高いエントロピの ASLR (Address Space Layout Randomization) を有効または無効にします。 OS は、アプリケーションのインフラストラクチャ (スタックとヒープなど) が読み込まれるメモリの位置をランダム化します。 このオプションを有効にすると、オペレーティング システムではこのランダム化を使用して、64 ビット コンピューターで完全な 64 ビット アドレス空間を使用できます。|
|`--keycontainer:key-container-name`|厳密な名前のキー コンテナーを指定します。|
|`--keyfile:filename`|生成されるアセンブリの署名に使用する公開キー ファイルの名前を指定します。|
|`--lib:folder-name`<br /><br />`-I:folder-name`|参照されるアセンブリを検索するディレクトリを指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;lib &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/s5bac5fx.aspx)」を参照してください。|
|`--linkresource:resource-info`|指定されたリソースをアセンブリにリンクさせます。 リソース情報の形式は <code>filename[name[public&#124;private]]</code><br /><br />このオプションで単一のリソースをリンクすると、`--resource` オプションでリソース ファイル全体を埋め込む代わりの方法として使用できます。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;linkresource &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/xawyf94k.aspx)」を参照してください。|
|`--mlcompatibility`|他のバージョンの ML との互換性維持を目的に設計された機能を使用するときに、表示される警告を無視します。|
|`--noframework`|.NET Framework アセンブリへの既定の参照を無効にします。|
|`--nointerfacedata`|F# 固有のメタデータを含むアセンブリに通常は追加されるリソースを省略するよう、コンパイラに指示します。|
|`--nologo`|コンパイラの起動時にバナー テキストが表示されないようにします。|
|`--nooptimizationdata`|インライン構成要素の実装に必要な場合にのみ最適化を行うよう、コンパイラに指示します。 モジュール間のインライン処理を禁止し、バイナリの互換性を改善してください。|
|`--nowin32manifest`|既定の Win32 マニフェストを省略するようコンパイラに指示します。|
|`--nowarn:warning-number-list`|番号で指定した特定の警告を無効にします。 それぞれの警告番号はコンマで区切ります。 警告の警告番号はコンパイルの出力で確認できます。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;nowarn &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/7f28x9z3.aspx)」を参照してください。|
|<code>--optimize[+&#124;-][optimization-option-list]</code><br /><br /><code>-O[+&#124;-] [optimization-option-list]</code>|最適化を有効または無効にします。 一部の最適化オプションについては、選択して指定したものだけを無効または有効にすることができます。 これには、`nojitoptimize`、`nojittracking`、`nolocaloptimize`、`nocrossoptimize`、`notailcalls` があります。|
|`--out:output-filename`<br /><br />`-o:output-filename`|コンパイルしたアセンブリまたはモジュールの名前を指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;out &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/bw3t50f3.aspx)」を参照してください。|
|`--pdb:pdb-filename`|出力デバッグ PDB (プログラム データベース) ファイルに名前を付けます。 このオプションは、`--debug` も有効にした場合にのみ適用されます。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;pdb &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/ms228625.aspx)」を参照してください。|
|`--platform:platform-name`|生成されたコードが、指定したプラットフォーム (`x86`、`Itanium`、または `x64`) でのみ実行されるように指定します。または、platform-name で `anycpu` が選択されている場合は、生成されたコードをどのプラットフォームでも実行できるように指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;platform &#40;&#35; C&#41;Compiler Options](https://msdn.microsoft.com/library/zekwfyz4.aspx)」を参照してください。|
|`--preferreduilang:lang`| 優先出力言語のカルチャ名を指定します (たとえば、`es-ES`、`ja-JP`)。 |
|`--quotations-debug`|追加のデバッグ情報が F# 引用符リテラルとリフレクション定義から派生した式に対して生成されるように指定します。 デバッグ情報は F# 式ツリー ノードのカスタム属性に追加されます。 「[コード引用符](code-quotations.md)」と「 [Expr. customattributes](https://msdn.microsoft.com/library/eb89943f-5f5b-474e-b125-030ca412edb3)」を参照してください。|
|`--reference:assembly-filename`<br /><br />`-r:assembly-filename`|コンパイルするコードで F# または .NET Framework アセンブリのコードを使用できるようにします。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;リファレンス&#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/yabyz3h4.aspx)」を参照してください。|
|`--resource:resource-filename`|生成されるアセンブリにマネージド リソース ファイルを埋め込みます。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;リソース&#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/c0tyye07.aspx)」を参照してください。|
|`--sig:signature-filename`|生成されるアセンブリに基づいてシグネチャ ファイルを生成します。 署名ファイルの詳細については、「signature」[を参照し](signature-files.md)てください。|
|`--simpleresolution`|MSBuild の解決方法ではなくディレクトリベースの Mono 規則を使用して解決する必要がある、アセンブリ参照を指定します。 既定では、Mono で実行する場合を除き、MSBuild の解決方法が使用されます。|
|`--standalone`|F# ライブラリなどの追加のアセンブリを行わなくても、単独で実行できるように、すべての依存関係を含むアセンブリを生成するように指定します。|
|`--staticlink:assembly-name`|指定したアセンブリと、そのアセンブリに依存するすべての参照 DLL を静的にリンクします。 DLL 名ではなく、アセンブリ名を使用します。|
|`--subsystemversion`|生成された実行可能ファイルが使用できる OS サブシステムのバージョンを指定します。 Windows 8.1 の場合は6.02、windows 7 の場合は6.01、Windows Vista の場合は6.00 を使用します。 このオプションは、DLL ではなく、実行可能ファイルのみに適用され、アプリケーションが OS の特定のバージョンでのみ使用できる特定のセキュリティ機能に依存している場合にのみ使用される必要があります。 このオプションが使用され、低いバージョンの OS でアプリケーションを実行しようとすると、失敗してエラー メッセージが表示されます。|
|<code>--tailcalls[+&#124;-]</code>|tail IL 命令の使用を有効または無効にします。有効にすると、スタック フレームが tail 再帰関数で再利用されます。 このオプションは、既定の設定で有効になります。|
|<code>--target:[exe&#124;winexe&#124;library&#124;module] filename</code>|生成されるコンパイル済みコードの種類とファイル名を指定します。<ul><li>`exe` はコンソール アプリケーションを表します。<br /></li><li>`winexe` は Windows アプリケーションを表します。コンソール アプリケーションとの違いは、標準入出力ストリーム (stdin、stdout、および stderr) が定義されていないことです。<br /></li><li>`library` は、エントリ ポイントのないアセンブリです。<br /></li><li>`module` は .NET Framework モジュール (.netmodule) です。後で他のモジュールと組み合わせて、1 つのアセンブリにまとめることができます。<br /></li><ul/>このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;target &#40;&#35; C&#41;Compiler Options](https://msdn.microsoft.com/library/6h25dztx.aspx)」を参照してください。|
|`--times`|コンパイルのタイミング情報を表示します。|
|`--utf8output`|UTF-8 エンコーディングでのコンパイラ出力を有効にします。|
|`--warn:warning-level`|警告レベル (0 ～ 5) を設定します。 既定のレベルは 3 です。 各警告には、重大度に基づいてレベルが設定されます。 レベル 5 の方が重大度は低くなりますが、レベル 1 より多くの警告が表示されます。<br /><br />レベル 5 の警告は次のとおりです: 21 (実行時にチェックされる再帰的用法)、22 (`let rec` がエラーと評価)、45 (完全な抽象化)、および 52 (防御的コピー)。 他の警告はすべてレベル 2 です。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;警告&#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/13b90fz7.aspx)」を参照してください。|
|`--warnon:warning-number-list`|既定でオフになっている、または別のコマンドラインオプションで無効になっている可能性がある特定の警告を有効にします。 F# 3.0 では、1182 (使用されていない変数) の警告は既定ではオフに設定されています。|
|<code>--warnaserror[+&#124;-] [warning-number-list]</code>|警告をエラーとして報告するオプションを有効または無効にします。 特定の警告番号を指定して無効または有効にすることができます。 後のコマンド ラインのオプションが前のコマンド ラインのオプションをオーバーライドします。 たとえば、エラーとして報告しない警告を指定するには、`--warnaserror+` `--warnaserror-:warning-number-list`を指定します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;warnaserror &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/406xhdz3.aspx)」を参照してください。|
|`--win32manifest:manifest-filename`|コンパイルに Win32 マニフェスト ファイルを追加します。 このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;win32manifest &#40;&#35; C&#41;コンパイラオプション](https://msdn.microsoft.com/library/bb545961.aspx)」を参照してください。|
|`--win32res:resource-filename`|コンパイルに Win32 リソース ファイルを追加します。<br /><br />このコンパイラ オプションは、同じ名前の C# コンパイラ オプションに相当します。 詳細については、「 [ &#47;win32res (&#40;C&#35;)&#41;コンパイラオプション](https://msdn.microsoft.com/library/8f2f5x2e.aspx)」を参照してください。|

## <a name="related-articles"></a>関連トピック

|タイトル|説明|
|-----|-----------|
|[F# Interactive オプション](fsharp-interactive-options.md)|F# インタープリター (fsi.exe) でサポートされるコマンド ライン オプションについて説明します。|
|[プロジェクトのプロパティのリファレンス](/visualstudio/ide/reference/project-properties-reference)|ビルド オプションを提供するプロジェクトのプロパティのページなど、プロジェクトの UI について説明します。|
