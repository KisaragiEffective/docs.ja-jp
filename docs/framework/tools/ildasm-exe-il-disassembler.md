---
title: Ildasm.exe (IL 逆アセンブラー)
ms.date: 03/30/2017
helpviewer_keywords:
- PE files, MSIL Disassembler
- portable executable files, MSIL Disassembler
- Ildasm.exe
- MSIL Disassembler
- text files produced by MSIL Disassembler
- disassembling file for MSIL Assembler input
ms.assetid: db27f6b2-f1ec-499e-be3a-7eecf95ca42b
ms.openlocfilehash: f23f8c48a31dffa7d350c872aed7505da7a36861
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73105055"
---
# <a name="ildasmexe-il-disassembler"></a>Ildasm.exe (IL 逆アセンブラー)

IL 逆アセンブラーは、IL アセンブラー (*Ilasm.exe*) と対をなすツールです。 *Ildasm.exe* は、中間言語 (IL: Intermediate Language) コードを含む、ポータブル実行可能 (PE) ファイルを使用して、*Ilasm.exe* に対する入力として適したテキスト ファイルを作成します。

このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

コマンド プロンプトに次のように入力します。

## <a name="syntax"></a>構文

```console
ildasm [options] [PEfilename] [options]
```

## <a name="parameters"></a>パラメーター

*.exe*、 *.dll*、 *.obj*、 *.lib*、および *.winmd* の各ファイルについて、次のオプションを使用できます。

| オプション | 説明 |
| ------ | ----------- |
|**/out=** `filename`|結果をグラフィカル ユーザー インターフェイスに表示せずに、指定した `filename` を持つ出力ファイルを作成します。|
|**/rtf**|出力をリッチ テキスト形式で生成します。 **/text** オプションと共に使用すると無効になります。|
|**/text**|結果をグラフィカル ユーザー インターフェイスまたは出力ファイルに出力せずに、コンソール ウィンドウに表示します。|
|**/html**|出力を HTML 形式で生成します。 **/output** オプションと共に使用する場合にのみ有効です。|
|**/?**|このツールのコマンド構文とオプションを表示します。|

*.exe* ファイル、 *.dll* ファイル、および *.winmd* ファイルについては、次のオプションも利用できます。

| オプション | 説明 |
| ------ | ----------- |
|**/bytes**|実際のバイトを 16 進形式の命令コメントとして表示します。|
|**/caverbal**|カスタム属性の BLOB を Verbal 形式で生成します。 既定はバイナリ形式です。|
|**/linenum**|元のソース行への参照を組み込みます。|
|**/nobar**|逆アセンブルのプログレス インジケーター ポップアップ ウィンドウの表示を中止します。|
|**/noca**|カスタム属性の出力を抑止します。|
|**/project**|ネイティブ Windows ランタイムに表示される方法ではなく、マネージド コードに表示される方法でメタデータを示します。 `PEfilename` が Windows メタデータ ( *.winmd*) ファイルではない場合、このオプションは無効になります。 「[Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)」を参照してください。|
|**/pubonly**|パブリックな型とメンバーだけを逆アセンブルします。 **/visibility:PUB**と等価です。|
|**/quoteallnames**|すべての名前を単一引用符で囲みます。|
|**/raweh**|例外処理句を生の形式で表示します。|
|**/source**|元のソース行をコメントとして表示します。|
|**/tokens**|クラスとメンバーのメタデータ トークンを表示します。|
|**/visibility:** `vis`[+`vis`...]|指定した参照可能範囲を持つ型またはメンバーだけを逆アセンブルします。 `vis` の有効な値を次に示します。<br /><br /> **PUB** — Public<br /><br /> **PRI** — Private<br /><br /> **FAM** — Family<br /><br /> **ASM** — Assembly<br /><br /> **FAA** — Family および Assembly<br /><br /> **FOA** — Family または Assembly<br /><br /> **PSC** — Private Scope<br /><br /> 以上の可視性修飾子の定義については、「<xref:System.Reflection.MethodAttributes>」と「<xref:System.Reflection.TypeAttributes>」を参照してください。|

次のオプションは、 *.exe* ファイル、 *.dll* ファイル、および *.winmd* ファイルをファイル出力またはコンソール出力する場合にだけ有効です。

| オプション | 説明 |
| ------ | ----------- |
|**/all**|**/header**、 **/bytes**、 **/stats**、 **/classlist**、 **/tokens** の各オプションの組み合わせを指定します。|
|**/classlist**|モジュールで定義されているクラスの一覧を含めます。|
|**/forward**|事前のクラス宣言を使用します。|
|**/headers**|出力にファイル ヘッダー情報を組み込みます。|
|**/item:** `class`[ **::** `member`[`(sig`]]|指定した引数に応じて、次の要素を逆アセンブルします。<br /><br /> -   指定した `class` を逆アセンブルします。<br />-   指定した `class` の `member` を逆アセンブルします。<br />-   指定したシグネチャ `sig` を持つ `class` の `member` を逆アセンブルします。 `sig` の形式は次のとおりです。<br />     [`instance`] `returnType`(`parameterType1`, `parameterType2`, …, `parameterTypeN`)<br />     **注** .NET Framework Version 1.0 および 1.1 では、`(sig)` のように、`sig` の後に閉じかっこを付ける必要があります。 .NET Framework 2.0 以降では、閉じかっこを省略して `(sig` とする必要があります。|
|**/noil**|IL アセンブリ コードが出力されなくなります。|
|**/stats**|イメージの統計情報を含めます。|
|**/typelist**|ラウンド トリップの型の順序を保存するために、型の完全な一覧を生成します。|
|**/unicode**|出力に Unicode エンコードを使用します。|
|**/utf8**|出力に UTF-8 エンコードを使用します。 既定値は ANSI です。|

次のオプションは、 *.exe*、 *.dll*、 *.obj*、 *.lib*、および *.winmd* の各ファイルをファイル出力またはコンソール出力する場合にだけ有効です。

| オプション | 説明 |
| ------ | ----------- |
|**/metadata**[=`specifier`]|メタデータを表示します。ここで、`specifier` は次のとおりです。<br /><br /> **MDHEADER** — メタデータのヘッダー情報とサイズを表示します。<br /><br /> **HEX** — ワードおよび 16 進で情報を表示します。<br /><br /> **CSV** — レコード数およびヒープ サイズを表示します。<br /><br /> **UNREX** — 未解決の外部項目を表示します。<br /><br /> **SCHEMA** — メタデータ ヘッダーおよびスキーマ情報を表示します。<br /><br /> **RAW** — 未処理のメタデータ テーブルを表示します。<br /><br /> **HEAPS** — 未処理のヒープを表示します。<br /><br /> **VALIDATE** — メタデータの一貫性を検証します。<br /><br /> **/metadata** を複数回指定し、`specifier`に異なる値を指定できます。|

次のオプションは、 *.lib* ファイルをファイル出力またはコンソール出力する場合にだけ有効です。

| オプション | 説明 |
| ------ | ----------- |
|**/objectfile**=`filename`|指定したライブラリ内の単一のオブジェクト ファイルのメタデータを表示します。|

> [!NOTE]
> *Ildasm.exe* に関するすべてのオプションでは大文字と小文字が区別されず、先頭の 3 文字で認識されます。 たとえば、 **/quo** は **/quoteallnames** と等価です。 引数を伴うオプションの場合は、オプションと引数の間に区切り記号としてコロン (:) または等号 (=) を挿入できます。 たとえば、 **/output:** *filename* は **/output=** *filename*と等価です。

## <a name="remarks"></a>解説

*Ildasm.exe* はディスク上のファイルについてだけ動作します。 グローバル アセンブリ キャッシュ内にインストールされたファイルについては動作しません。

*Ildasm.exe* で生成されるテキスト ファイルを IL アセンブラー (*Ilasm.exe*) に対する入力として使用できます。 これは、必ずしもランタイム メタデータ属性のすべてをサポートしないプログラミング言語で記述されたコードをコンパイルするときなどに便利です。 コードをコンパイルし、その出力を *Ildasm.exe* で実行した後、生成された IL テキスト ファイルを手作業で編集して足りない属性を追加できます。 このテキスト ファイルを IL アセンブラーで実行すると、最終的な実行可能ファイルを生成できます。

> [!NOTE]
> 現時点では、埋め込みのネイティブ コード (たとえば Visual C++ で生成された PE ファイル) を含む PE ファイルについては、この手法を使用できません。  

IL 逆アセンブラーで既定の GUI を使用すると、既存のどの PE ファイルのメタデータおよび逆アセンブルしたコードでも、階層ツリー ビューで表示できます。 GUI を使用するには、引数 *PEfilename* またはその他のオプションを指定せずに、コマンド行で「**ildasm**」と入力します。 **[ファイル]** メニューで、*Ildasm.exe* に読み込む PE ファイルまで移動できます。 選択した PE ファイルについて表示されたメタデータおよび逆アセンブルしたコードを保存するには、 **[ファイル]** メニューの **[ダンプ]** をクリックします。 階層ツリー ビューだけを保存するには、 **[ファイル]** メニューの **[ツリービューをダンプ]** をクリックします。 *Ildasm.exe* へのファイルの読み込みとその出力の解釈について詳しくは、Windows SDK に付属している Samples フォルダー内にある、*Ildasm.exe* のチュートリアルを参照してください。

*Ildasm.exe* に対して、埋め込みリソースを含む引数 *PEfilename* を指定した場合は、複数の出力ファイルが生成されます。生成されるファイルは、IL コードを含む 1 つのテキスト ファイルと、埋め込みマネージド リソースごとにリソース名を使用してメタデータから生成した .resources ファイルです。 アンマネージ リソースが *PEfilename* の中に埋め込まれている場合は、IL 出力に対して **/output** オプションで指定されたファイル名を使用して、.res ファイルが生成されます。

> [!NOTE]
> *Ildasm.exe* では、入力ファイルの *.obj* と *.lib* についてはメタデータの説明だけが表示されます。 これらの種類のファイルの場合、IL コードは逆アセンブルされません。

*Ildasm.exe* を .exe ファイルまたは *.dll* ファイルに対して実行し、ファイルが管理されているかどうかを確認できます。 ファイルが管理されていない場合は、そのファイルに有効な共通言語ランタイム ヘッダーがないため、逆アセンブルできないことを示すメッセージが表示されます。 ファイルが管理されている場合は、処理が正常に行われます。

## <a name="version-information"></a>バージョン情報

.NET Framework 4.5 以降、未処理のバイナリ コンテンツを表示することで、認識できないマーシャリング BLOB (バイナリ ラージ オブジェクト) が *Ildasm.exe* によって処理されます。 たとえば、C# プログラムで生成されたマーシャル BLOB の表示方法を次のコードに示します。

```csharp
public void Test([MarshalAs((short)70)] int test) { }
```

```il
// IL from Ildasm.exe output
.method public hidebysig instance void Test(int32  marshal({ 46 }) test) cil managed
```

次の *Ildasm.exe* の出力からの抜粋に示すように、.NET Framework 4.5 以降、*Ildasm.exe* ではインターフェイス実装に適用される属性が表示されます。

```il
.class public auto ansi beforefieldinit MyClass
  extends [mscorlib]System.Object
  implements IMyInterface
  {
    .interfaceimpl type IMyInterface
    .custom instance void
      [mscorlib]System.Diagnostics.DebuggerNonUserCodeAttribute::.ctor() = ( 01 00 00 00 )
      …
```

## <a name="examples"></a>使用例

PE ファイル `MyHello.exe` のメタデータと逆アセンブルしたコードを、*Ildasm.exe* の既定の GUI で表示するコマンドを次に示します。

```console
ildasm myHello.exe
```

ファイル `MyFile.exe` を逆アセンブルし、生成される IL アセンブラー テキストをファイル *MyFile.il* に格納するコマンドを次に示します。

```console
ildasm MyFile.exe /output:MyFile.il
```

ファイル `MyFile.exe` を逆アセンブルし、生成される IL アセンブラーをコンソール ウィンドウに表示するコマンドを次に示します。

```console
ildasm MyFile.exe /text
```

`MyApp.exe` ファイルに埋め込みのマネージド リソースとアンマネージド リソースが含まれる場合、次のコマンドを実行すると 4 つのファイル (*MyApp.il*、*MyApp.res*、*Icons.resources*、*Message.resources*) が生成されます。

```console
ildasm MyApp.exe /output:MyApp.il
```

`MyFile.exe` 内のクラス `MyClass` に属するメソッド `MyMethod` を逆アセンブルし、その出力をコンソール ウィンドウに表示するコマンドを次に示します。

```console
ildasm /item:MyClass::MyMethod MyFile.exe /text
```

上の例では、異なるシグネチャを持つ `MyMethod` という名前のメソッドが複数存在する可能性があります。 戻り値の型 **void**、およびパラメーターの型 **int32** と **string** を指定してインスタンス メソッド `MyMethod` を逆アセンブルするコマンドを次に示します。

```console
ildasm /item:"MyClass::MyMethod(instance void(int32,string)" MyFile.exe /text
```

> [!NOTE]
> .NET Framework Version 1.0 および 1.1 では、メソッド名の後にある左かっことシグネチャの後にある右かっこの数が一致するように `MyMethod(instance void(int32))` とする必要があります。 .NET Framework Version 2.0 以降では、閉じかっこを省略して `MyMethod(instance void(int32)` とする必要があります。

`static` メソッド (Visual Basic の `Shared` メソッド) を取得するには、キーワード `instance` を省略します。 `int32` や `string` のようなプリミティブ型でないクラス型には名前空間を含め、キーワード `class` を前に付ける必要があります。 外部の型には、角かっこで囲んだライブラリ名を前に付ける必要があります。 `MyMethod` 型の 1 つのパラメーターと <xref:System.AppDomain> 型の戻り値を持つ、<xref:System.AppDomain> という名前の静的メソッドを逆アセンブルするコマンドを次に示します。

```console
ildasm /item:"MyClass::MyMethod(class [mscorlib]System.AppDomain(class [mscorlib]System.AppDomain)" MyFile.exe /text
```

入れ子になった型は、クラスを前に付けてスラッシュで区切る必要があります。 たとえば、`MyNamespace.MyClass` クラスに `NestedClass` という名前の入れ子になったクラスが含まれている場合は、`class MyNamespace.MyClass/NestedClass` のように指定します。

## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [Ilasm.exe (IL アセンブラー)](ilasm-exe-il-assembler.md)
- [マネージド実行プロセス](../../standard/managed-execution-process.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
