---
title: Ilasm.exe (IL アセンブラー)
ms.date: 03/30/2017
helpviewer_keywords:
- MSIL generators
- metadata, MSIL Assembler
- MSIL Assembler
- portable executable files, MSIL Assembler
- PE files, MSIL Assembler
- MSIL
- Ilasm.exe
- verifying MSIL performance
ms.assetid: 4ca3a4f0-4400-47ce-8936-8e219961c76f
ms.openlocfilehash: cb995e78e534048043886070536ef0dd0a45c057
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73105101"
---
# <a name="ilasmexe-il-assembler"></a>Ilasm.exe (IL アセンブラー)

IL アセンブラーは、ポータブル実行可能 (PE) ファイルを IL (Intermediate Language) から生成します (IL の詳細については、「[マネージド実行プロセス](../../standard/managed-execution-process.md)」を参照してください)。IL と必要なメタデータを含む実行可能ファイルを実行すると、IL が予測どおりに動作するかどうかを確認できます。

このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

コマンド プロンプトに次のように入力します。

## <a name="syntax"></a>構文

```console
ilasm [options] filename [[options]filename...]
```

## <a name="parameters"></a>パラメーター

| 引数 | 説明 |
| -------- | ----------- |
|`filename`|.il ソース ファイルの名前。 このファイルは、メタデータ宣言ディレクティブとシンボリック IL 命令で構成されます。 複数のソース ファイル引数を指定すると、*Ilasm.exe* で 1 つの PE ファイルを作成できます。 **注:** .il ソース ファイルのコードの最後の行に、後続の空白または行末文字のいずれかがあることを確認してください。|

| オプション | 説明 |
| ------ | ----------- |
|**/32bitpreferred**|32 ビット優先イメージ (PE32) を作成します。|
|**/alignment:** `integer`|NT オプション ヘッダーの FileAlignment を `integer` で指定された値に設定します。 このオプションは、ファイル指定されている .alignment IL ディレクティブをオーバーライドします。|
|**/appcontainer**|出力として、Windows アプリ コンテナー内で実行する *.dll* ファイルまたは *.exe* ファイルを作成します。|
|**/arm**|ターゲット プロセッサとして Advanced RISC Machine (ARM) を指定します。<br /><br /> イメージのビット数を指定しない場合、既定は **/32bitpreferred**です。|
|**/base:** `integer`|NT オプション ヘッダーの ImageBase を `integer` で指定された値に設定します。 このオプションは、ファイルに指定されている .imagebase IL ディレクティブをオーバーライドします。|
|**/clock**|指定した .il ソース ファイルのコンパイル時間を計測して報告します。<br /><br /> **Total Run**: 後に続く特定の操作の実行に要した合計時間。<br /><br /> **Startup**: ファイルを読み込み、開く。<br /><br /> **Emitting MD**:メタデータの出力。<br /><br /> **Ref to Def Resolution**: ファイルの定義への参照の解決。<br /><br /> **CEE File Generation**: メモリ内のファイル イメージの生成。<br /><br /> **PE File Writing**: PE へのイメージの書き込み。|
|**/debug**[:**IMPL**&#124;**OPT**]|デバッグ情報 (ローカル変数と引数名、および行番号) を組み込みます。 PDB ファイルを作成します。<br /><br /> **/debug** に値を追加しなければ、JIT の最適化が無効になり、PDB ファイルのシーケンス ポイントが使用されます。<br /><br /> **IMPL** を指定すると、JIT 最適化が無効になり、暗黙のシーケンス ポイントが使用されます。<br /><br /> **OPT** を指定すると、JIT 最適化が有効になり、暗黙のシーケンス ポイントが使用されます。|
|**/dll**|出力として *.dll* ファイルを生成します。|
|**/enc:** `file`|指定されたソース ファイルからエディット コンティニュ デルタを作成します。<br /><br /> この引数は教育機関専用のため、商業目的の使用はサポートされていません。|
|**/exe**|出力として実行可能ファイルを生成します。 既定値です。|
|**/flags:** `integer`|共通言語ランタイム ヘッダーの ImageFlags を `integer` で指定された値に設定します。 このオプションは、ファイルに指定されている .corflags IL ディレクティブをオーバーライドします。 有効な *integer*の値の一覧については、CorHdr.h で COMIMAGE_FLAGS を参照してください。|
|**/fold**|複数の同じメソッド本体を 1 つに折りたたみます。|
|/**highentropyva**|高エントロピ ASLR (Address Space Layout Randomization) をサポートする出力実行可能プログラムを作成します。 (既定では **/appcontainer**)。|
|**/include:** `includePath`|`#include`によってインクルードされるファイルの検索パスを設定します。|
|**/itanium**|ターゲット プロセッサとして Intel Itanium を指定します。<br /><br /> イメージのビット数を指定しない場合、既定は **/pe64**です。|
|**/key:** `keyFile`|`filename` に含まれる秘密キーを使って、厳密な署名を持つ `keyFile` をコンパイルします。|
|**/key:**  @`keySource`|`filename` で生成された秘密キーを使って、厳密な署名を持つ `keySource` をコンパイルします。|
|**/listing**|標準出力にリスティング ファイルを生成します。 このオプションを省略すると、リスティング ファイルは生成されません。<br /><br /> このパラメーターは、.NET Framework 2.0 以降ではサポートされません。|
|**/mdv:** `versionString`|メタデータのバージョン文字列を設定します。|
|**/msv:** `major`.`minor`|メタデータのストリーム バージョンを設定します。ここで、 `major` と `minor` は整数です。|
|**/noautoinherit**|基底クラスが指定されていない場合、 <xref:System.Object> からの既定の継承を無効にします。|
|**/nocorstub**|CORExeMain スタブの生成を抑止します。|
|**/nologo**|Microsoft 著作権情報を表示しません。|
|**/output:** `file.ext`|出力ファイルの名前と拡張子を指定します。 既定では、出力ファイルの名前は最初のソース ファイルの名前と同じです。 既定の拡張子は *.exe* です。 **/dll** オプションを指定した場合の既定の拡張子は *.dll* です。 **注:**  **/output** :myfile.dll と指定しても **/dll** オプションは設定されません。 **/dll**を指定しないと、*myfile.dll* という名前の実行可能ファイルになります。|
|**/optimize**|長いインストラクションを短く最適化します。 たとえば `br` を `br.s`にします。|
|**/pe64**|64 ビットのイメージ (PE32+) を作成します。<br /><br /> ターゲット プロセッサを指定しない場合、既定は `/itanium`です。|
|**/pdb**|デバッグ情報の追跡を有効にせずに PDB ファイルを作成します。|
|**/quiet**|クワイエット モードを指定します。アセンブリの進行状況はレポートされません。|
|**/resource:** `file.res`|指定した \*.res 形式のリソース ファイルを生成される *.exe* ファイルまたは *.dll* ファイルに組み込みます。 **/resource** オプションで指定できる .res ファイルは 1 つだけです。|
|**/ssver:** `int`.`int`|NT オプション ヘッダーのサブシステム バージョン番号を設定します。 **/appcontainer** および **/arm** では、最小バージョン番号は 6.02 です。|
|**/stack:** `stackSize`|NT Optional ヘッダーの SizeOfStackReserve 値を `stackSize`に設定します。|
|**/stripreloc**|ベースの再配置が不要であることを指定します。|
|**/subsystem:** `integer`|NT オプション ヘッダーのサブシステムを `integer` で指定された値に設定します。 このコマンドは、ファイルに指定されている .subsystem IL ディレクティブをオーバーライドします。 有効な `integer` の値の一覧については、winnt.h で IMAGE_SUBSYSTEM を参照してください。|
|**/x64**|ターゲット プロセッサとして 64 ビットの AMD プロセッサを指定します。<br /><br /> イメージのビット数を指定しない場合、既定は **/pe64**です。|
|**/?**|このツールのコマンド構文とオプションを表示します。|

> [!NOTE]
> *Ilasm.exe* に関するすべてのオプションでは大文字と小文字が区別されず、先頭の 3 文字で認識されます。 たとえば、 **/lis** は **/listing** と等価であり、 **/res:** myresfile.res は **/resource:** myresfile.res と等価です。引数を伴うオプションの場合は、オプションと引数の間に区切り記号としてコロン (:) または等号 (=) を挿入できます。 たとえば、 **/output**:*file.ext* は **/output**=*file.ext* と等価です。

## <a name="remarks"></a>解説

IL アセンブラーは、IL ジェネレーターを設計および実装するツールの販売元を支援します。 ツールとコンパイラの開発者は、*Ilasm.exe* を使用することで、PE ファイル形式での IL の出力にかかわることなく、IL とメタデータの生成に集中できます。

C# および Visual Basic など、ランタイムを対象とした他のコンパイラと同様に、*Ilasm.exe* も中間オブジェクト ファイルを生成しません。このため、PE ファイルを形成するためのリンク ステージが必要ありません。

IL アセンブラーは、すべての既存メタデータ、およびランタイムを対象としたプログラミング言語の IL 機能を表現できます。 このため、このようなプログラミング言語で記述されたマネージド コードを IL アセンブラーで適切に表現し、*Ilasm.exe* でコンパイルできます。

> [!NOTE]
> .il ソース ファイルのコードの最後の行に、後続の空白または行末文字がない場合、コンパイルに失敗することがあります。

*Ilasm.exe* と、その対をなすツール [*Ildasm.exe*](ildasm-exe-il-disassembler.md) を併用できます。 *Ildasm.exe* は、IL コードを含む PE ファイルを使用して、*Ilasm.exe* への入力として適したテキスト ファイルを作成します。 これは、必ずしもランタイム メタデータ属性のすべてをサポートしないプログラミング言語で記述されたコードをコンパイルするときなどに便利です。 コードをコンパイルし、その出力を *Ildasm.exe* で実行した後、生成された IL テキスト ファイルを手作業で編集して足りない属性を追加できます。 このテキスト ファイルを *Ilasm.exe* で実行すると、最終的な実行可能ファイルを生成できます。

この方法を使用して、異なるコンパイラによって生成された複数の PE ファイルから 1 つの PE ファイルを生成することもできます。

> [!NOTE]
> 現時点では、埋め込みのネイティブ コード (たとえば Visual C++ で生成された PE ファイル) を含む PE ファイルについては、この手法を使用できません。

この *Ildasm.exe* と *Ilasm.exe* の組み合わせをできる限り正確に使用するため、アセンブラーは、既定では、IL ソース内に記述されている可能性がある (または別のコンパイラによって出力される可能性がある) 長いエンコーディングを短いエンコーディングに置換しません。 可能な場合は、常に **/optimize** オプションを使用して、短いエンコーディングを置換します。

> [!NOTE]
> *Ildasm.exe* はディスク上のファイルについてだけ動作します。 グローバル アセンブリ キャッシュ内にインストールされたファイルについては動作しません。

IL の文法の詳細については、Windows SDK の asmparse.grammar ファイルを参照してください。

## <a name="version-information"></a>バージョン情報

.NET Framework 4.5 以降では、次に類似するコードを使用することで、インターフェイス実装にカスタム属性を追加できます。

```il
.class interface public abstract auto ansi IMyInterface
{
  .method public hidebysig newslot abstract virtual
    instance int32 method1() cil managed
  {
  } // end of method IMyInterface::method1
} // end of class IMyInterface
.class public auto ansi beforefieldinit MyClass
  extends [mscorlib]System.Object
  implements IMyInterface
  {
    .interfaceimpl type IMyInterface
    .custom instance void
      [mscorlib]System.Diagnostics.DebuggerNonUserCodeAttribute::.ctor() = ( 01 00 00 00 )
      …
```

.NET Framework 4.5 以降では、次のコードに示すように、未処理のバイナリ表現を使用することで、任意のマーシャリング BLOB (バイナリ ラージ オブジェクト) を指定できます。

```il
.method public hidebysig abstract virtual
        instance void
        marshal({ 38 01 02 FF })
        Test(object A_1) cil managed
```

IL の文法の詳細については、Windows SDK の asmparse.grammar ファイルを参照してください。

## <a name="examples"></a>使用例

IL ファイル *myTestFile.il* をアセンブルして実行可能ファイル *myTestFile.exe* を生成するコマンドを次に示します。

```console
ilasm myTestFile
```

IL ファイル *myTestFile.il* をアセンブルして *.dll* ファイル *myTestFile.dll* を生成するコマンドを次に示します。

```console
ilasm myTestFile /dll
```

IL ファイル *myTestFile.il* をアセンブルして *.dll* ファイル *myNewTestFile.dll* を生成するコマンドを次に示します。

```console
ilasm myTestFile /dll /output:myNewTestFile.dll
```

コンソールに "Hello World!" 記述するだけです。 このコードのコンパイル後に [*Ildasm.exe*](ildasm-exe-il-disassembler.md) ツールを使用して、IL ファイルを生成できます。

```csharp
using System;

public class Hello
{
    public static void Main(String[] args)
    {
        Console.WriteLine("Hello World!");
    }
}
```

次の IL コードの例は、前の C# のコード例に対応しています。 IL アセンブラー ツールを使うと、このコードをアセンブリにコンパイルできます。 IL コードと C# コードの例は、どちらもコンソールに "Hello World!" 記述するだけです。

```il
// Metadata version: v2.0.50215
.assembly extern mscorlib
{
  .publickeytoken = (B7 7A 5C 56 19 34 E0 89 )                         // .z\V.4..
  .ver 2:0:0:0
}
.assembly sample
{
  .custom instance void [mscorlib]System.Runtime.CompilerServices.CompilationRelaxationsAttribute::.ctor(int32) = ( 01 00 08 00 00 00 00 00 )
  .hash algorithm 0x00008004
  .ver 0:0:0:0
}
.module sample.exe
// MVID: {A224F460-A049-4A03-9E71-80A36DBBBCD3}
.imagebase 0x00400000
.file alignment 0x00000200
.stackreserve 0x00100000
.subsystem 0x0003       // WINDOWS_CUI
.corflags 0x00000001    //  ILONLY
// Image base: 0x02F20000

// =============== CLASS MEMBERS DECLARATION ===================

.class public auto ansi beforefieldinit Hello
       extends [mscorlib]System.Object
{
  .method public hidebysig static void  Main(string[] args) cil managed
  {
    .entrypoint
    // Code size       13 (0xd)
    .maxstack  8
    IL_0000:  nop
    IL_0001:  ldstr      "Hello World!"
    IL_0006:  call       void [mscorlib]System.Console::WriteLine(string)
    IL_000b:  nop
    IL_000c:  ret
  } // end of method Hello::Main

  .method public hidebysig specialname rtspecialname
          instance void  .ctor() cil managed
  {
    // Code size       7 (0x7)
    .maxstack  8
    IL_0000:  ldarg.0
    IL_0001:  call       instance void [mscorlib]System.Object::.ctor()
    IL_0006:  ret
  } // end of method Hello::.ctor

} // end of class Hello
```

## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [*Ildasm.exe* (IL 逆アセンブラー)](ildasm-exe-il-disassembler.md)
- [マネージド実行プロセス](../../standard/managed-execution-process.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
