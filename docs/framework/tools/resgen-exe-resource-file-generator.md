---
title: Resgen.exe (リソース ファイル ジェネレーター)
ms.date: 03/30/2017
helpviewer_keywords:
- resource files, .resources files
- resource files, .resx files
- resx files (resource files)
- .resources files
- files, converting
- Resource File Generator
- .resx files
- Resgen.exe
- resource files, converting
- converting files, Resource File Generator
- binary resources files
- embedding files in runtime binary executable
ms.assetid: 8ef159de-b660-4bec-9213-c3fbc4d1c6f4
ms.openlocfilehash: 5fc2bcb03ae6814d69e229ba083c1d5c44ae8ff3
ms.sourcegitcommit: 81ad1f09b93f3b3e6706a7f2e4ddf50ef229ea3d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74204621"
---
# <a name="resgenexe-resource-file-generator"></a>Resgen.exe (リソース ファイル ジェネレーター)
リソース ファイル ジェネレーター (Resgen.exe) は、テキスト (.txt または .restext) ファイルおよび XML ベースのリソース形式 (.resx) ファイルを共通言語ランタイムのバイナリ (.resources) ファイルに変換します。この .resources ファイルは、ランタイム バイナリ実行可能ファイルまたはサテライト アセンブリに埋め込むことができます。 「[リソース ファイルの作成](../resources/creating-resource-files-for-desktop-apps.md)」をご覧ください。  
  
 Resgen.exe は、次のタスクを実行する汎用のリソース変換ユーティリィティです。  
  
- .txt または .restext ファイルから .resources または .resx ファイルへの変換。 (.restext ファイルの形式は .txt ファイルの形式と同じです。 ただし、拡張子 .restext を使用すると、リソース定義を含むテキスト ファイルをより簡単に識別できます)。  
  
- .resources ファイルからテキスト ファイルまたは .resx ファイルへの変換。  
  
- .resx ファイルからテキスト ファイルまたは .resources ファイルへの変換。  
  
- アセンブリからの文字列リソースを Windows 8.x Store アプリケーションでの使用に適した .resw ファイルに抽出します。  
  
- 個々の名前付きリソースと <xref:System.Resources.ResourceManager> インスタンスへのアクセスを提供する厳密に型指定されたクラスを作成します。  
  
 何らかの理由により Resgen.exe が失敗した場合は、戻り値は –1 です。  
  
 Resgen.exe のヘルプを表示する場合は、オプションを指定せずに次のコマンドを使用して Resgen.exe のコマンド構文およびオプションを表示できます。  
  
```console  
resgen  
```  
  
 `/?` スイッチを使用することもできます。  
  
```console  
resgen /?  
```  
  
 Resgen.exe を使用してバイナリ .resources ファイルを生成する場合は、言語コンパイラを使用して実行可能アセンブリにバイナリ ファイルを埋め込むか、[アセンブリ リンカー (Al.exe)](al-exe-assembly-linker.md) を使用してサテライト アセンブリにコンパイルできます。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
resgen  [-define:symbol1[,symbol2,...]] [/useSourcePath] filename.extension  | /compile filename.extension... [outputFilename.extension] [/r:assembly] [/str:lang[,namespace[,class[,file]]] [/publicclass]]   
```  
  
```console  
resgen filename.extension [outputDirectory]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|パラメーターまたはスイッチ|説明|  
|-------------------------|-----------------|  
|`/define:` *symbol1*[, *symbol2*,...]|.NET Framework 4.5 以降、テキスト ベース (.txt または .restext) リソース ファイル内の条件付きコンパイルがサポートされています。 *symbol* が `#ifdef` コンストラクト内の入力テキスト ファイルに含まれるシンボルに対応している場合は、関連の文字列リソースが .resources ファイルに含まれます。 `#if !` スイッチによって定義されていないシンボルと共に `/define` ステートメントが入力テキスト ファイルに含まれている場合、関連の文字列リソースがリソース ファイルに含まれます。<br /><br /> テキスト ファイル以外で使用される場合、`/define` は無視されます。 シンボルでは、大文字と小文字が区別されます。<br /><br /> このオプションについて詳しくは、このトピックの「[リソースの条件付きコンパイル](#Conditional)」をご覧ください。|  
|`useSourcePath`|入力ファイルの現在のディレクトリを使用して相対ファイル パスを解決することを指定します。|  
|`/compile`|複数の .resx ファイルまたはテキスト ファイルを指定して、一括した操作で複数の .resources ファイルに変換できるようにします。 このオプションを指定しない場合、指定できる入力ファイル引数は 1 つだけです。 出力ファイルには、*filename*.resources という名前が付けられます。<br /><br /> このオプションは、`/str:` オプションと一緒に使用することはできません。<br /><br /> このオプションについて詳しくは、このトピックの「[複数のファイルのコンパイルまたは変換](#Multiple)」をご覧ください。|  
|`/r:` `assembly`|指定されたアセンブリからメタデータを参照します。 これは、.resx ファイルを変換するときに使用され、Resgen.exe がオブジェクト リソースをシリアル化または非シリアル化できるようにします。 C# および Visual Basic コンパイラの `/reference:` や `/r:` オプションに似ています。|  
|`filename.extension`|変換対象の入力ファイルの名前を指定します。 この表の前に示した 1 番目の長いコマンド ライン構文を使用する場合は、`extension` が以下のいずれかであることが必要です。<br /><br /> .txt または .restext<br /> .resources ファイルまたは .resx ファイルに変換するテキスト ファイル。 テキスト ファイルには、文字列リソースだけを含めることができます。 ファイル形式については、「[リソース ファイルの作成](../resources/creating-resource-files-for-desktop-apps.md)」の「テキスト ファイル内のリソース」をご覧ください。<br /><br /> .resx<br /> .resources ファイルまたはテキスト (.txt または .restext) ファイルに変換する、XML ベースのリソース ファイル。<br /><br /> .resources<br /> .resx ファイルまたはテキスト (.txt または .restext) ファイルに変換するバイナリ リソース ファイル。<br /><br /> この表の前に示した 2 番目の短いコマンド ライン構文を使用する場合は、`extension` が以下のいずれかであることが必要です。<br /><br /> .exe または .dll<br /> 文字列リソースが Windows 8.x Store アプリケーションの開発で使用するための .resw ファイルに抽出される .NET Framework アセンブリ (実行可能ファイルまたはライブラリ)。|  
|`outputFilename.extension`|作成するリソース ファイルの名前および種類を指定します。<br /><br /> .txt ファイル、.restext ファイル、または .resx ファイルから .resources ファイルへ変換する場合、この引数は省略できます。 `outputFilename` を指定しないと、入力 `filename` ファイルに拡張子 .resources が追加され、そのファイルが `filename,extension` を含むディレクトリに書き込まれます。<br /><br /> .resources ファイルから変換する場合、引数 `outputFilename.extension` は必ず指定する必要があります。 .resources ファイルを XML ベースのリソース ファイルに変換する場合は、拡張子が .resx のファイル名を指定します。 .resources ファイルをテキスト ファイルに変換する場合は、拡張子が .txt または .restext のファイル名を指定します。 .resources ファイルを .txt ファイルに変換するのは、.resource ファイルに文字列値だけが含まれている場合に限ります。|  
|`outputDirectory`|Windows 8.x Store アプリケーションの場合は、`filename.extension` に文字列リソースを含む .resw ファイルが書き込まれるディレクトリを指定します。 `outputDirectory` は既に存在している必要があります。|  
|`/str:` `language[,namespace[,classname[,filename]]]`|`language` オプションで指定されたプログラミング言語で、厳密に型指定されたリソース クラス ファイルを作成します。 `language` は、次のリテラルの 1 つで構成できます:<br /><br /> -   C# の場合: `c#`、`cs`、または `csharp`。<br />-   Visual Basic の場合: `vb` または `visualbasic`。<br />-   VBScript の場合: `vbs` または `vbscript`。<br />-   C++ の場合: `c++`、`mc`、または `cpp`。<br />-   JavaScript の場合: `js`、`jscript`、または `javascript`。<br /><br /> `namespace` オプションではプロジェクトの既定の名前空間を指定し、`classname` オプションでは生成されるクラスの名前を指定し、`filename` オプションではクラス ファイルの名前を指定します。<br /><br /> `/str:` オプションは 1 つの入力ファイルにのみ対応しているため、`/compile` オプションと一緒に使用することはできません。<br /><br /> `namespace` を指定して、`classname` を指定しない場合、出力ファイル名からクラス名が派生します (たとえば、ピリオドがアンダースコアに置き換えられます)。 このため、厳密に型指定されたリソースが正常に機能しないことがあります。 この問題を回避するには、クラス名と出力ファイル名の両方を指定します。<br /><br /> このオプションについて詳しくは、このトピックの「[厳密に型指定されたリソース クラスの生成](#Strong)」をご覧ください。|  
|`/publicClass`|厳密に型指定されたリソース クラスをパブリック クラスとして作成します。 既定では、リソース クラスは、C# の場合 `internal`、Visual Basic の場合 `Friend` です。<br /><br /> `/str:` オプションを使用しない場合、このオプションは無視されます。|  
  
## <a name="resgenexe-and-resource-file-types"></a>Resgen.exe とリソース ファイルの種類  
 Resgen.exe で正常にリソースを変換するには、テキストおよび .resx ファイルが正しい形式になっている必要があります。  
  
### <a name="text-txt-and-restext-files"></a>テキスト (.txt および .restext) ファイル  
 テキスト (.txt または .restext) ファイルには、文字列リソースのみを含めることができます。 文字列リソースは、文字列を複数の言語に翻訳する必要があるアプリケーションを記述する場合に便利です。 たとえば、適切な文字列リソースを使用することで、メニュー文字列を簡単に地域固有の文字列に変更できます。 Resgen.exe は名前と値のペアを含むテキスト ファイルを読み取ります。名前はリソースを説明する文字列で、値はリソース文字列そのものです。  
  
> [!NOTE]
> .txt ファイルと .restext ファイルの形式については、「[リソース ファイルの作成](../resources/creating-resource-files-for-desktop-apps.md)」の「テキスト ファイル内のリソース」をご覧ください。  
  
 リソースを格納するテキスト ファイルは、基本ラテンの範囲の文字だけ (U+007F まで) が含まれていない限り、UTF-8 エンコーディングまたは Unicode (UTF-16) エンコーディングで格納する必要があります。 Resgen.exe は、ANSI エンコーディングを使用して保存されたテキスト ファイルを処理するときに拡張 ANSI 文字を削除します。  
  
 Resgen.exe は、テキスト ファイルの中でリソース名が重複していないかどうかを確認します。 テキスト ファイルの中でリソース名が重複している場合は、警告メッセージが出され、2 つ目の値は無視されます。  
  
### <a name="resx-files"></a>.resx ファイル  
 .resx リソース ファイル形式は、XML エントリから構成されます。 テキスト ファイルの場合と同様に、これらの XML エントリの中に文字列リソースを指定できます。 .resx ファイルがテキスト ファイルよりも優れている点は、オブジェクトの指定や埋め込みもできることです。 .resx ファイルを表示してみると、埋め込みオブジェクト (画像など) のバイナリ形式を参照できます (このバイナリ情報がリソース マニフェストの一部に含まれている場合)。 テキスト ファイルと同様に、.resx ファイルはテキスト エディター (メモ帳や Microsoft Word など) で開き、内容を書き込み、解析、操作できます。 そのためには、XML のタグや .resx ファイルの構造を十分に知っておく必要があります。 .resx ファイル形式について詳しくは、「[リソース ファイルの作成](../resources/creating-resource-files-for-desktop-apps.md)」の「.resx ファイル内のリソース」セクションをご覧ください。  
  
 文字列以外の埋め込みオブジェクトを含む .resources ファイルを作成するには、それらのオブジェクトを含む .resx ファイルを Resgen.exe で変換するか、<xref:System.Resources.ResourceWriter> クラスで提供されているメソッドを呼び出して、オブジェクト リソースをコードから直接ファイルに追加する必要があります。  
  
 .resx または .resources ファイルにオブジェクトが含まれ、Resgen.exe を使用してそれをテキスト ファイルに変換する場合、文字列リソースはすべて正しく変換されますが、非文字列オブジェクトのデータ型も文字列としてファイルに書き込まれることになります。 この変換処理では埋め込みオブジェクトが失われるため、リソースの取得時にエラーが発生したと報告されます。  
  
### <a name="converting-between-resources-file-types"></a>リソース ファイルの種類間の変換  
 異なる種類のリソース ファイル間で変換を行うと、変換元と変換先のファイルの種類によっては、Resgen.exe が変換を実行できないか、特定のリソースに関する情報が失われる場合があります。 次の表は、あるリソース ファイルの種類から別の種類に変換するとき、正常な変換の種類を指定しています。  
  
|変換元|テキスト ファイルへ|.resx ファイルへ|.resw ファイルへ|.resources ファイルへ|  
|------------------|------------------|-------------------|-------------------|------------------------|  
|テキスト (.txt または .restext) ファイル|--|問題なし|サポートなし|問題なし|  
|.resx ファイル|変換は、ファイルに文字列以外のリソース (ファイル リンクを含む) が含まれている場合は失敗します|--|サポートなし|問題なし|  
|.resources ファイル|変換は、ファイルに文字列以外のリソース (ファイル リンクを含む) が含まれている場合は失敗します|問題なし|サポートなし|--|  
|.exe または .dll アセンブリ|サポートなし|サポートなし|文字列リソース (パス名を含む) だけがリソースとして認識されます。|サポートなし|  
  
## <a name="performing-specific-resgenexe-tasks"></a>特定の Resgen.exe タスクの実行  
 さまざまな方法で Resgen.exe を使用できます。テキストベースまたは XML ベースのリソース ファイルをバイナリ ファイルにコンパイルすることや、リソース ファイル形式を別のリソース ファイル形式に変換することや、<xref:System.Resources.ResourceManager> 機能をラップしてリソースへのアクセスを提供するクラスを生成することができます。 このセクションには、各タスクに関する詳細な情報があります。  
  
- [リソースのバイナリ ファイルへのコンパイル](resgen-exe-resource-file-generator.md#Compiling)  
  
- [リソース ファイルの種類間の変換](resgen-exe-resource-file-generator.md#Convert)  
  
- [複数のファイルのコンパイルまたは変換](resgen-exe-resource-file-generator.md#Multiple)  
  
- [.resw ファイルへのリソースのエクスポート](resgen-exe-resource-file-generator.md#Exporting)  
  
- [リソースの条件付きコンパイル](resgen-exe-resource-file-generator.md#Conditional)  
  
- [厳密に型指定されたリソース クラスの生成](resgen-exe-resource-file-generator.md#Strong)  
  
<a name="Compiling"></a>   
### <a name="compiling-resources-into-a-binary-file"></a>リソースのバイナリ ファイルへのコンパイル  
 Resgen.exe の最も一般的な用途は、テキスト ベースのリソース ファイル (.txt または .restext ファイル) または XML ベースのリソース ファイル (.resx ファイル) をバイナリ .resources ファイルにコンパイルすることです。 次に、出力ファイルは、言語コンパイラを使用してメインのアセンブリに、または[アセンブリ リンカー (AL.exe)](al-exe-assembly-linker.md) を使用してサテライト アセンブリに埋め込むことができます。  
  
 リソース ファイルをコンパイルする構文は次のとおりです。  
  
```console  
resgen inputFilename [outputFilename]   
```  
  
 パラメーターは次のとおりです。  
  
 `inputFilename`  
 コンパイルするリソース ファイルの拡張子を含むファイル名。 Resgen.exe は、拡張子 .txt、.restext、または .resx を持つファイルだけをコンパイルします。  
  
 `outputFilename`  
 出力ファイルの名前。 `outputFilename` を省略した場合は、Resgen.exe が `inputFilename` と同じディレクトリに `inputFilename` のルート ファイル名の .resources ファイルを作成します。 `outputFilename` でディレクトリ パスが示されている場合、ディレクトリが存在する必要があります。  
  
 ファイル名を指定し、それをピリオドでルート ファイル名から区切ることで、.resources ファイルの完全修飾名前空間を指定します。 たとえば、`outputFilename` が `MyCompany.Libraries.Strings.resources` の場合、その名前空間は MyCompany.Libraries になります。  
  
 Resources.txt に含まれる名前と値のペアを読み取り、Resources.resources という名前のバイナリ .resources ファイルを書き込むコマンドを次に示します。 出力ファイル名は明示的に指定されていないため、入力ファイルと同じ名前を既定で受け取ります。  
  
```console  
resgen Resources.txt   
```  
  
 Resources.restext に含まれる名前と値のペアを読み取り、StringResources.resources という名前のバイナリ リソース ファイルを書き込むコマンドを次に示します。  
  
```console  
resgen Resources.restext StringResources.resources  
```  
  
 XML ベースの入力ファイル Resources.resx を読み取り、Resources.resources という名前のバイナリ .resources ファイルを書き込むコマンドを次に示します。  
  
```console  
resgen Resources.resx Resources.resources  
```  
  
<a name="Convert"></a>   
### <a name="converting-between-resource-file-types"></a>リソース ファイルの種類間の変換  
 テキストベースか XML ベースのリソース ファイルをバイナリ .resources ファイルにコンパイルすることに加えて、Resgen.exe は、任意のサポートされるファイルの種類を他のサポートされるファイルの種類に変換できます。 これによって、次の変換を実行できます。  
  
- .txt および .restext ファイルから .resx ファイル。  
  
- .resx ファイルから .txt および .restext ファイル。  
  
- .resources ファイルから .txt および .restext ファイル。  
  
- .resources ファイルから .resx ファイル。  
  
 構文は、前のセクションの構文と同じです。  
  
 また、Resgen.exe を使用して、.NET Framework アセンブリへの埋め込みリソースを Windows 8.x Store アプリケーション対応 .resw ファイルに変換できます。  
  
 バイナリ リソース ファイル Resources.resources を読み取り、Resources.resx という名前の XML ベースの出力ファイルを書き込むコマンドを次に示します。  
  
```console  
resgen Resources.resources Resources.resx  
```  
  
 次のコマンドは、StringResources.txt という名前のテキスト ベースのリソース ファイルを読み取り、LibraryResources.resx という名前の XML ベースのリソース ファイルに書き込みます。 文字列リソースを含むことに加えて、.resx ファイルは、文字列以外のリソースを保存するのにも使用できます。  
  
```console  
resgen StringResources.txt LibraryResources.resx  
```  
  
 次の 2 つのコマンドは、Resources.resx という名前の XML ベースのリソース ファイルを読み取り、Resources.txt および Resources.restext という名前のテキスト ファイルに書き込みます。 .resx ファイルに埋め込みオブジェクトが含まれている場合は、テキスト ファイルに正しく変換されません。  
  
```console  
resgen Resources.resx Resources.txt  
resgen Resources.resx Resources.restext  
```  
  
<a name="Multiple"></a>   
### <a name="compiling-or-converting-multiple-files"></a>複数のファイルのコンパイルまたは変換  
 `/compile` スイッチを使用すると、1 回の操作でリソース ファイルの一覧を別の形式に変換できます。 構文は次のとおりです。  
  
```console  
resgen /compile filename.extension [filename.extension...]  
```  
  
 次のコマンドは、3 つのファイル、StringResources.resources、TableResources.resources、ImageResources.resources を StringResources.txt、TableResources.resw、ImageResources.resw という名前の別の .resources ファイルにコンパイルします。  
  
```console  
resgen /compile StringResources.txt TableResources.resx ImageResources.resx  
```  
  
<a name="Exporting"></a>   
### <a name="exporting-resources-to-a-resw-file"></a>.resw ファイルへのリソースのエクスポート  
 Windows 8.x Store アプリケーションを開発する場合は、既存のデスクトップ アプリケーションのリソースを使用することもできます。 ただし、2 つの種類のアプリケーションは、異なるファイル形式をサポートします。 デスクトップ アプリケーションでは、テキストのリソース (.txt または .restext) または .resx ファイルはバイナリ .resources ファイルにコンパイルされます。 Windows 8.x Store アプリケーションでは、.resw ファイルはバイナリ パッケージ リソース インデックス (PRI) ファイルにコンパイルされます。 Resgen.exe を使用して、このギャップを埋めることができます。これを行うには、実行可能ファイルまたはサテライト アセンブリからリソースを抽出して、Windows 8.x Store アプリケーションの開発時に使用できる 1 つ以上の .resw ファイルを記述します。  
  
> [!IMPORTANT]
> Visual Studio は、ポータブル ライブラリ内のリソースを Windows 8.x Store アプリに組み込むために必要なすべての変換を自動的に処理します。 アセンブリ内でリソースを .resw ファイル形式に変換するために Resgen.exe を直接使用する方法は、Visual Studio 外で Windows 8.x Store アプリを開発する開発者にのみ関連します。  
  
 アセンブリから .resw ファイルを生成する構文は次のとおりです。  
  
```console  
resgen filename.extension  [outputDirectory]  
```  
  
 パラメーターは次のとおりです。  
  
 `filename.extension`  
 .NET Framework アセンブリ (実行可能ファイルまたは .DLL) の名前。 ファイルにリソースが含まれていない場合、Resgen.exe でファイルは作成されません。  
  
 `outputDirectory`  
 .resw ファイルを記述する既存のディレクトリ。 `outputDirectory` を省略すると、.resw ファイルは現在のディレクトリに書き込まれます。 Resgen.exe は、アセンブリ内の .resources ファイルごとに 1 つの .resw ファイルを作成します。 .resw ファイルのルート ファイル名は .resources ファイルのルート名と同じです。  
  
 次のコマンドは、MyApp.exe に埋め込まれた各 .resources ファイルに対し、Win8Resources ディレクトリに .resw ファイルを作成します。  
  
```console  
resgen MyApp.exe Win8Resources  
```  
  
<a name="Conditional"></a>   
### <a name="conditionally-compiling-resources"></a>リソースの条件付きコンパイル  
 .NET Framework 4.5 以降、Resgen.exe では、テキスト (.txt および .restext) ファイル内の文字列リソースの条件付きコンパイルがサポートされています。 これによって、複数のビルド構成で 1 つのテキスト ベースのリソース ファイルを使用できます。  
  
 .txt または .restext ファイルで、シンボルが定義されている場合は、`#ifdef` … `#endif` コンストラクトを使って、バイナリ .resources ファイルにリソースを含めます。また、シンボルが定義されていない場合は、`#if !` ... `#endif` コンストラクトを使ってリソースを含めます。 コンパイル時に、シンボルのコンマ区切りリストが続く `/define:` オプションを使用して、シンボルを定義します。 比較では、大文字と小文字が区別されます。つまり、`/define` によって定義されたシンボルとコンパイルされるテキスト ファイルのシンボルの大文字と小文字が一致する必要があります。  
  
 たとえば、UIResources.rext という名前の次のファイルには、`AppTitle`、`PRODUCTION`、または `CONSULT` のどの名前のシンボルが定義されているかに応じて 3 つの値からいずれか 1 つの値を取得できる `RETAIL` という名前の文字列リソースが含まれます。  
  
```text
#ifdef PRODUCTION  
AppTitle=My Software Company Project Manager   
#endif  
#ifdef CONSULT  
AppTitle=My Consulting Company Project Manager  
#endif  
#ifdef RETAIL  
AppTitle=My Retail Store Project Manager  
#endif  
FileMenuName=File  
```  
  
 ファイルは、次のコマンドを使用してバイナリ .resources ファイルにコンパイルできます。  
  
```console  
resgen /define:CONSULT UIResources.restext  
```  
  
 これによって、2 つの文字列リソースを含む .resources ファイルが生成されます。 `AppTitle` リソースの値は "My Consulting Company Project Manager" です。  
  
<a name="Strong"></a>   
### <a name="generating-a-strongly-typed-resource-class"></a>厳密に型指定されたリソース クラスを生成しています  
 Resgen.exe は厳密に型指定されたリソースをサポートします。このサポートでは、静的な読み取り専用プロパティを持つクラスを作成して、リソースへのアクセスをカプセル化します。 これによって、リソースを取得するために <xref:System.Resources.ResourceManager> クラスのメソッドを直接呼び出す方法の代替手段が可能になります。 `/str` クラスの機能をラップする、Resgen.exe の <xref:System.Resources.Tools.StronglyTypedResourceBuilder> オプションを使用して、厳密に型指定されたリソース サポートを有効にすることができます。 `/str` オプションを指定した場合の Resgen.exe の出力は、入力パラメーターで参照されるリソースと一致する厳密に型指定されたプロパティを含むクラスです。 このクラスは、処理されたファイルで使用できるリソースに対する、厳密に型指定された読み取り専用アクセスを提供します。  
  
 厳密に型指定されたリソースを作成する構文は次のとおりです。  
  
```console  
resgen inputFilename [outputFilename] /str:language[,namespace,[classname[,filename]]] [/publicClass]  
```  
  
 パラメーターとスイッチ:  
  
 `inputFilename`  
 厳密に型指定されたリソース クラスを生成するためのリソース ファイルのファイル名 (拡張子を含む)。 ファイルは、テキスト ベース、XML ベース、または .resources バイナリ ファイルです。 .txt、.restext、.resw、または .resources 拡張子を持つことができます。  
  
 `outputFilename`  
 出力ファイルの名前。 `outputFilename` でディレクトリ パスが示されている場合、ディレクトリが存在する必要があります。 `outputFilename` を省略した場合は、Resgen.exe が `inputFilename` と同じディレクトリに `inputFilename` のルート ファイル名の .resources ファイルを作成します。  
  
 `outputFilename` は、テキスト ベース、XML ベース、またはバイナリ .resources ファイルです。 `outputFilename` の拡張子が `inputFilename` のファイル拡張子と異なる場合は、Resgen.exe でファイル変換が実行されます。  
  
 `inputFilename` が .resources ファイルで、`outputFilename` も .resources ファイルである場合は、Resgen.exe で .resources ファイルがコピーされます。 `outputFilename` を省略すると、Resgen.exe で `inputFilename` が同一の .resources ファイルで上書きされます。  
  
 *language*  
 厳密な型のリソース クラスのソース コードの生成に使用する言語。 指定できる値は、C# コードの場合は `cs`、`C#`、`csharp`、Visual Basic コードの場合は `vb`、`visualbasic`、VBScript コードの場合は `vbs`、`vbscript`、C++ コードの場合は `c++`、`mc`、`cpp` です。  
  
 *namespace*  
 厳密に型指定したクラスを含む名前空間。 .resources ファイルおよびリソース クラスは、同じ名前空間を持つ必要があります。 `outputFilename` で名前空間を指定する場合については、「[リソースのバイナリ ファイルへのコンパイル](resgen-exe-resource-file-generator.md#Compiling)」をご覧ください。 *namespace* を省略すると、リソース クラスは、名前空間に含まれなくなります。  
  
 *classname*  
 厳密な型のリソース クラスの名前。 これは、.resources ファイルのルート名に対応する必要があります。 たとえば、Resgen.exe で MyCompany.Libraries.Strings.resources という名前の .resources ファイルが生成される場合、厳密に型指定されたリソース クラスの名前は Strings になります。 If *classname* を省略すると、生成されるクラスは `outputFilename` のルート名から派生します。 If `outputFilename` を省略すると、生成されるクラスは `inputFilename` のルート名から派生します。  
  
 *classname* には、埋め込まれた空白などの無効な文字を含めることはできません。 *classname* に空白が埋め込まれている場合、または *classname* が *inputFilename* から既定で生成されて、*inputFilename* に空白が埋め込まれている場合、Resgen.exe によって無効な文字がすべてアンダースコア (\_) に置き換えられます。  
  
 *ファイル名*  
 クラス ファイルの名前。  
  
 `/publicclass`  
 厳密に型指定されたリソース クラスを `internal` (C# の場合) または `Friend` (Visual Basic) ではなくパブリックにします。 これにより、リソースには、それ自体が埋め込まれているアセンブリの外部からアクセスできます。  
  
> [!IMPORTANT]
> 厳密に型指定されたリソース クラスを作成するときは、生成されたコードの名前空間とクラス名に .resources ファイルの名前を合わせる必要があります。 ただし、Resgen.exe では、互換性のない名前の .resources ファイルを生成するオプションを指定することもできます。 この動作を回避するには、出力ファイルの生成後にその名前を変更します。  
  
 厳密に型指定されたリソース クラスには、次のメンバーがあります。  
  
- 厳密に入力されたリソース クラスをインスタンス化するために使用できる、パラメーターなしのコンストラクター。  
  
- `static` (C#) または `Shared` (Visual Basic)、および厳密に入力されたリソースを管理する `ResourceManager` のインスタンスを返す読み取り専用 <xref:System.Resources.ResourceManager> プロパティ。  
  
- リソース検索に使用するカルチャを設定する `Culture` 静的プロパティ。 既定値は `null` です。したがって、現在の UI カルチャが使用されることを意味します。  
  
- 1 つの `static` (C#) または `Shared` (Visual Basic)、および .resources ファイルのリソースごとの読み取り専用プロパティ。 プロパティの名前はリソースの名前です。  
  
 たとえば、次のコマンドでは、StringResources.txt という名前のリソース ファイルを StringResources.resources にコンパイルし、リソース マネージャーへのアクセスに使用できる StringResources.vb という名前の Visual Basic ソース コード ファイルに `StringResources` という名前のクラスを生成します。  
  
```console  
resgen StringResources.txt /str:vb,,StringResources   
```  
  
## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [デスクトップ アプリケーションのリソース](../resources/index.md)
- [リソース ファイルの作成](../resources/creating-resource-files-for-desktop-apps.md)
- [Al.exe (アセンブリ リンカー)](al-exe-assembly-linker.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
