---
title: F# Interactive (fsi.exe) のリファレンス
description: 対話型 ( F# fsi.exe) を使用して、コンソールでコードF#を対話形式で実行したり、 F#スクリプトを実行したりする方法について説明します。
ms.date: 05/16/2016
ms.openlocfilehash: 9f4b5c0e7527d29e375265bb31a5de2df098f8e1
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73419901"
---
# <a name="interactive-programming-with-f"></a>F\# を使用した対話型プログラミング

> [!NOTE]
> この記事では、現時点の Windows のエクスペリエンスについてのみ説明します。  書き換えられる予定です。

> [!NOTE]
> API リファレンスのリンクをクリックすると MSDN に移動します。  docs.microsoft.com API リファレンスは完全ではありません。

F# Interactive (fsi.exe) は、コンソールで F# コードを対話形式で実行したり、F# スクリプトを実行したりするために使用します。 つまり、F# Interactive は、F# 言語の REPL (Read、Evaluate、Print Loop) を実行します。

コンソールから F# Interactive を実行するには、fsi.exe を実行します。  Fsi.exe は次の場所にあります。

```console
C:\Program Files (x86)\Microsoft Visual Studio\2019\<sku>\Common7\IDE\CommonExtensions\Microsoft\FSharp
```

ここで `sku` は `Community`、`Professional`、`Enterprise`のいずれかです。

使用できるコマンド ライン オプションについては、「[F# Interactive Options](../../language-reference/fsharp-interactive-options.md)」 (F# Interactive オプション) を参照してください。

Visual Studio で F# Interactive を実行するには、ツール バーの **[F# Interactive]** というボタンをクリックするか、**Ctrl + Alt + F** キーを使用します。 この操作により、対話形式のウィンドウが開きます。このウィンドウは、F# Interactive セッションを実行するツール ウィンドウです 対話形式のウィンドウで実行するコードを選択し、**Alt + Enter** キーの組み合わせを押す方法もあります。 F# インタラクティブが **[F# Interactive]** というツール ウィンドウで開始されます。 このショートカット キーを使用するときは、エディター ウィンドウにフォーカスがあることを確認します。

コンソールと Visual Studio のどちらを使用している場合でも、コマンド プロンプトが表示され、入力待ちの状態になります。 コード ファイルと同じようにコードを入力できます。 コードをコンパイルして実行するには、2 つのセミコロン ( **;;** ) を入力して、入力行を終了します。

F# Interactive によってコードがコンパイルされ、成功すると、コードが実行され、コンパイルされた型のシグネチャと値が出力されます。 エラーが発生した場合は、エラー メッセージが出力されます。

同じセッションで入力したコードからは、前に入力したどの構成要素にもアクセスできるため、プログラムの構築が可能です。 ツール ウィンドウには十分なバッファーが用意されており、必要に応じてコードをファイルにコピーできます。

Visual Studio で実行する場合、F# Interactive はプロジェクトとは独立して動作します。このため、たとえば、プロジェクトで定義された構成要素を F# Interactive で使用することはできません。使用するには、関数のコードを対話形式のウィンドウにコピーする必要があります。

あるライブラリを参照するプロジェクトを開くと、**ソリューション エクスプローラー**で、F# Interactive のライブラリを参照できます。 F# Interactive のライブラリを参照するには、 **[参照]** ノードを展開し、ライブラリのショートカット メニューを開き、 **[F# Interactive に送信]** をクリックします。

設定を調整することで、F# Interactive コマンド ライン引数 (オプション) を制御できます。 **[ツール]** メニューの **[オプション]** をクリックし、 **[F# ツール]** を展開します。 変更できる 2 つの設定は、F# Interactive オプションおよび 64 ビット コンピューターで F# Interactive を実行する場合にのみ関連する **64 ビット F# Interactive** 設定です。 この設定によって、32 ビットまたは 64 ビット プロセスとして実行するかどうかを決定するためにコンピューター アーキテクチャを使用する、fsi.exe または fsianycpu.exe の専用の 64 ビット バージョンを実行するかどうかが決定されます。

## <a name="scripting-with-f"></a>F\# でのスクリプト作成

スクリプトで使用されるファイル拡張子は **.fsx** または **.fsscript** です。 ソース コードをコンパイルし、後でそのコンパイル済みのアセンブリを実行する代わりに、**fsi.exe** を実行し、F# ソース コードのスクリプトのファイル名を指定するだけで、F# Interactive によってコードを読み取り、リアルタイムで実行することができます。

## <a name="differences-between-the-interactive-scripting-and-compiled-environments"></a>対話型、スクリプト、およびコンパイル型の環境の相違

F# Interactive でのコードのコンパイル時には、対話形式で実行しているか、スクリプトを実行しているかにかかわらず、シンボル **INTERACTIVE** が定義されます。 コンパイラでのコードのコンパイル時には、シンボル **COMPILED** が定義されます。 したがって、コンパイル モードと対話モードでコードを別にする必要がある場合は、条件付きコンパイルを行うプリプロセッサ ディレクティブを使用して、どちらを有効にするかを決定できます。

F# Interactive でスクリプトを実行するときに使用できるディレクティブの中には、コンパイラを実行するときには使用できないものがあります。 次の表に、F# Interactive で使用できるディレクティブの概要を示します。

|ディレクティブ|説明|
|---------|-----------|
|**#help**|使用できるディレクティブに関する情報を表示します。|
|**#I**|アセンブリ検索パスを引用符で囲んで指定します。|
|**#load**|ソース ファイルを読み取り、コンパイルして実行します。|
|**#quit**|F# Interactive セッションを終了します。|
|**#r**|アセンブリを参照します。|
|**#time ["on"&#124;"off"]**|**#time** 単独で、パフォーマンス情報を表示するかどうかを切り替えます。 有効にすると、解釈および実行されるコードのセクションごとに、実時間、CPU 時間、およびガベージ コレクションの情報が F# Interactive によって計測されます。|

F# Interactive でファイルまたはパスを指定するときは、リテラル文字列が想定されます。 したがって、ファイルとパスは引用符で囲む必要があり、通常のエスケープ文字が適用されます。 また、@ 文字を使用して、パスを含む文字列が F# Internactive で逐語的文字列として解釈されるように指示することもできます。 この場合、F# Interactive はエスケープ文字を無視するようになります。

コンパイル モードと対話モードの違いの 1 つは、コマンド ライン引数にアクセスする方法です。 コンパイル モードでは **System.Environment.GetCommandLineArgs** を使用します。 スクリプトでは、**fsi.CommandLineArgs** を使用します。

次のコードは、スクリプトでコマンド ライン引数を読み取る関数を作成する方法と、スクリプトから別のアセンブリを参照する方法を示しています。 最初のコード ファイル **MyAssembly.fs** は、参照先となるアセンブリのコードです。 このファイルをコマンド ラインでコンパイルし (**fsc -a MyAssembly.fs**)、2 番目のファイルをスクリプトとしてコマンド ラインで実行します (**fsi --exec file1.fsx** test)。

```fsharp
// MyAssembly.fs
module MyAssembly
let myFunction x y = x + 2 * y
```

```fsharp
// file1.fsx
#r "MyAssembly.dll"

printfn "Command line arguments: "

for arg in fsi.CommandLineArgs do
    printfn "%s" arg

printfn "%A" (MyAssembly.myFunction 10 40)
```

出力は次のとおりです。

```console
Command line arguments:
file1.fsx
test
90
```

## <a name="related-topics"></a>関連トピック

|Title|説明|
|-----|-----------|
|[F# Interactive オプション](../../language-reference/fsharp-interactive-options.md)|F#対話型の fsi.exe のコマンドライン構文とオプションについて説明します。|
|[F# Interactive ライブラリ リファレンス](https://msdn.microsoft.com/visualfsharpdocs/conceptual/fsharp-interactive-library-reference)|F# Interactive でコードを実行するときに使用できるライブラリ機能について説明します。|
