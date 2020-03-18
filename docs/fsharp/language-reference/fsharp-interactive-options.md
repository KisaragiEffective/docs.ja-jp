---
title: 対話形式のオプション
description: Interactive、fsi.exe でF#サポートされているコマンドラインオプションについて説明します。
ms.date: 05/16/2016
ms.openlocfilehash: cceb8fb50434f3525ebb2ede16e84720d10d320c
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348222"
---
# <a name="f-interactive-options"></a>F# Interactive オプション

> [!NOTE]
> この記事では、現時点の Windows のエクスペリエンスについてのみ説明します。  書き換えられる予定です。

このトピックでは、F# Interactive (`fsi.exe`) でサポートされるコマンド ライン オプションについて説明します。 F# Interactive では、F# コンパイラと同じコマンド ライン オプションを数多く使用できますが、その他にもいくつかのオプションを使用できます。

## <a name="using-f-interactive-for-scripting"></a>F# Interactive を使用したスクリプトの実行

F#対話型、`fsi.exe`は対話形式で起動できます。また、コマンドラインから起動してスクリプトを実行することもできます。 コマンド ラインの構文は次のとおりです。

```console
> fsi.exe [options] [ script-file [arguments] ]
```

F# スクリプト ファイルのファイル拡張子は `.fsx` です。

## <a name="table-of-f-interactive-options"></a>F# Interactive のオプションの表

次の表は、F# Interactive でサポートされるオプションの一覧です。 これらのオプションをコマンド ラインまたは Visual Studio IDE で設定できます。 Visual Studio IDE でこれらのオプションを設定するには、 **[ツール]** メニューを開き、 **[オプション]** を選択し、 **[ F#ツール]** ノードを展開して **[ F#対話型]** を選択します。

F# Interactive オプションの引数でリストを指定する場合は、リストの要素をセミコロン (`;`) で区切ります。

|オプション|説明|
|------|-----------|
|**--**|残りの引数F#をプログラムまたはスクリプトのF#コマンドライン引数として扱うように対話型に指示するために使用されます。これは、 **fsi.exe**リストを使用してコード内でアクセスできます。|
|**--checked**[ **+** &#124; **-** ]|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--codepage:&lt;int&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--consolecolors**[ **+** &#124; **-** ]|警告およびエラーメッセージを色で出力します。|
|**--crossoptimize**[ **+** &#124; **-** ]|モジュール間の最適化を有効または無効にします。|
|**--debug**[ **+** &#124; **-** ]<br /><br />**--debug:** [**full**&#124;**pdbonly**&#124;**portable**&#124;**embedded**]<br /><br />**-g**[ **+** &#124; **-** ]<br /><br />**-g:** [**full**&#124;**pdbonly**&#124;**portable**&#124;**embedded**]|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--define:&lt;string&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--deterministic**[ **+** &#124; **-** ]|決定的なアセンブリ (モジュールのバージョン GUID とタイムスタンプを含む) を生成します。|
|**--exec**|ファイルを読み込んだ後、またはコマンド ラインで指定したスクリプトを実行した後に F# Interactive を終了するように指示します。|
|**--fullpaths**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--gui**[ **+** &#124; **-** ]|Windows フォーム イベントのループを有効または無効にします。 既定ではオンです。|
|**--help**<br /><br />**-?**|各オプションのコマンド ライン構文と簡単な説明を表示するために使用します。|
|**--lib:&lt;folder-list&gt;**<br /><br />**-I:&lt;folder-list&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--load:&lt;ファイル名&gt;**|指定したソース コードを起動時にコンパイルし、コンパイルされた F# の構成要素をセッションに読み込みます。 ターゲットソースに **#use**や **#load**などのスクリプトディレクティブが含まれている場合は、-- **load**または **#load**の代わりに **--use**または **#use**を使用する必要があります。|
|**--mlcompatibility**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--noframework**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラオプション](compiler-options.md)」を参照してください。|
|**--nologo**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--nowarn:&lt;warning-list&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--optimize**[ **+** &#124; **-** ]|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--preferreduilang:&lt;lang&gt;**| 優先する出力言語のカルチャ名を指定します (例: es、ja-jp)。 |
|**--quiet**|Stdout F#ストリームへの対話型の出力を抑制します。|
|**--quotations-debug**|追加のデバッグ情報が F# 引用符リテラルとリフレクション定義から派生した式に対して生成されるように指定します。 デバッグ情報は F# 式ツリー ノードのカスタム属性に追加されます。 「[コード引用符](code-quotations.md)」と「 [Expr. customattributes](https://msdn.microsoft.com/library/eb89943f-5f5b-474e-b125-030ca412edb3)」を参照してください。|
|**--readline**[ **+** &#124; **-** ]|対話モードでのタブ補完を有効または無効にします。|
|**--reference:&lt;ファイル名&gt;**<br /><br />**-r:&lt;ファイル名&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--shadowcopyreferences**[ **+** &#124; **-** ]|対話型プロセスによって参照がF#ロックされるのを防ぎます。|
|**--simpleresolution**|MSBuild の解決ではなく、ディレクトリベースのルールを使用してアセンブリ参照を解決します。|
|**--tailcalls**[ **+** &#124; **-** ]|tail IL 命令の使用を有効または無効にします。有効にすると、スタック フレームが tail 再帰関数で再利用されます。 このオプションは、既定の設定で有効になります。|
|**--targetprofile:&lt;string&gt;**|このアセンブリのターゲットフレームワークプロファイルを指定します。 有効な値は、mscorlib、netcore、または netcore です。  既定値は mscorlib です。|
|**--use:&lt;ファイル名&gt;**|指定したファイルを起動時に最初の入力として使用するよう、インタープリターに指示します。|
|**--utf8output**|fsc.exe コンパイラ オプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--warn:&lt;警告レベル&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--warnaserror**[ **+** &#124; **-** ]|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|
|**--warnaserror**[ **+** &#124; **-** ]: **&lt;int-list&gt;**|**Fsc.exe**コンパイラオプションと同じです。 詳細については、「[コンパイラ オプション](compiler-options.md)」を参照してください。|

## <a name="related-topics"></a>関連トピック

|[タイトル]|説明|
|-----|-----------|
|[コンパイラ オプション](compiler-options.md)|F#コンパイラの**fsc.exe**で使用できるコマンドラインオプションについて説明します。|
