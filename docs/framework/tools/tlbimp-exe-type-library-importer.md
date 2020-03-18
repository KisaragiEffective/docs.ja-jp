---
title: Tlbimp.exe (タイプ ライブラリ インポーター)
ms.date: 03/30/2017
helpviewer_keywords:
- type libraries [.NET Framework], importing
- importing type library
- Tlbimp.exe
- type definition conversion
- Type Library Importer
- type libraries
- converting type definitions
ms.assetid: ec0a8d63-11b3-4acd-b398-da1e37e97382
ms.openlocfilehash: fcbbc99c06e7c9666d58133bef20b84ef40c98d5
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73104330"
---
# <a name="tlbimpexe-type-library-importer"></a>Tlbimp.exe (タイプ ライブラリ インポーター)
タイプ ライブラリ インポーターは、COM タイプ ライブラリにある型定義を共通言語ランタイム アセンブリで等価な定義に変換します。 Tlbimp.exe の出力は、元のタイプ ライブラリで定義された型のランタイム メタデータを格納するバイナリ ファイル (アセンブリ) です。 このファイルは [ildasm.exe](ildasm-exe-il-disassembler.md) などのツールでチェックできます。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
tlbimp tlbFile [options]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|引数|説明|  
|--------------|-----------------|  
|*tlbFile*|COM タイプ ライブラリを格納する任意のファイルの名前。|  
  
|オプション|説明|  
|------------|-----------------|  
|**/asmversion:** *versionnumber*|生成するアセンブリのバージョン番号を指定します。 *versionnumber* は *major.minor.build.revision* の形式で指定します。|  
|**/company:** `companyinformation`|出力アセンブリに会社情報を追加します。|  
|**/copyright:** `copyrightinformation`|出力アセンブリに著作権情報を追加します。 この情報は、アセンブリの **[ファイルのプロパティ]** ダイアログ ボックスで確認できます。|  
|**T:System.Reflection.AssemblyDelaySignAttribute**|Tlbimp.exe が遅延署名を使用して、生成されたアセンブリに厳密な名前で署名するように指定します。 このオプションは、 **/keycontainer:** 、 **/keyfile:** 、または **/publickey:** のいずれかのオプションと共に指定する必要があります。 遅延署名プロセスの詳細については、「[アセンブリへの遅延署名](../../standard/assembly/delay-sign.md)」を参照してください。|  
|**/help**|このツールのコマンド構文とオプションを表示します。|  
|**/keycontainer:** *containername*|*containername* で指定されたキー コンテナーの公開キーと秘密キーのペアを使用して、生成されたアセンブリに厳密な名前で署名します。|  
|**/keyfile:** *filename*|*filename* にある発行者の正式な公開キーと秘密キーのペアを使用して、生成されたアセンブリに厳密な名前で署名します。|  
|**/machine:** `machinetype`|指定したコンピューターの種類 (マイクロプロセッサ) を対象とするアセンブリを作成します。 サポートされているコンピューターの種類は、x86、x64、Itanium、および「不明」です。|  
|**/namespace:** *namespace*|アセンブリの生成先の名前空間を指定します。|  
|**/noclassmembers**|Tlbimp.exe がメンバーをクラスに追加できないようにします。 これにより、<xref:System.TypeLoadException> が発生する可能性がなくなります。|  
|**/nologo**|Microsoft 著作権情報を表示しません。|  
|**/out:** *filename*|メタデータ定義の書き込み先の出力ファイル、アセンブリ、および名前空間の名前を指定します。 アセンブリの名前空間を明示的に制御するインターフェイス定義言語 (IDL: Interface Definition Language) カスタム属性がタイプ ライブラリで指定されている場合、 **/out** オプションはアセンブリの名前空間を制御できません。 このオプションを指定しない場合、Tlbimp.exe は、入力ファイルで定義された実際のタイプ ライブラリと同じ名前のファイルにメタデータを書き込み、そのファイルに .dll 拡張子を割り当てます。 出力ファイルの名前が入力ファイルと同じ場合は、タイプ ライブラリが上書きされるのを防ぐためにエラーが発生します。|  
|**/primary**|指定したタイプ ライブラリのプライマリ相互運用機能アセンブリを生成します。 アセンブリに追加される情報は、アセンブリを生成したタイプ ライブラリの発行者を示します。 プライマリ相互運用機能アセンブリを指定すると、Tlbimp.exe を使用して、ある発行者のアセンブリとタイプ ライブラリから作成された他のアセンブリを区別します。 Tlbimp.exe を使用してインポートするタイプ ライブラリの発行者である場合は、 **/primary** オプションを使用してください。 プライマリ相互運用機能アセンブリには、[厳密な名前](../../standard/assembly/strong-named.md)で署名する必要があります。 詳細については、「[プライマリ相互運用機能アセンブリ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100))」を参照してください。|  
|**/product:** `productinformation`|出力アセンブリに製品情報を追加します。 この情報は、アセンブリの **[ファイルのプロパティ]** ダイアログ ボックスで確認できます。|  
|**/productversion:** `productversioninformation`|出力アセンブリにバージョン情報を追加します。 形式の制限はありません。 この情報は、アセンブリの **[ファイルのプロパティ]** ダイアログ ボックスで確認できます。|  
|**/publickey:** *filename*|生成されたアセンブリに署名するために使用する公開キーを格納するファイルを指定します。 **/publickey:** オプションの代わりに **/keyfile:** オプションまたは **/keycontainer:** オプションを指定した場合、Tlbimp.exe は、 **/keyfile:** または **/keycontainer:** で指定された公開キーと秘密キーのペアから公開キーを生成します。 **/publickey:** オプションを指定すると、テスト キーと遅延署名を使用できます。 ファイルは Sn.exe で生成された形式になります。 詳細については、「[厳密名ツール (Sn.exe)](sn-exe-strong-name-tool.md)」にある Sn.exe の **-p** オプションの説明を参照してください。|  
|**/reference:** *filename*|現在のタイプ ライブラリ以外で定義された型への参照を解決するために使用するアセンブリ ファイルを指定します。 **/reference** オプションを指定しない場合、Tlbimp.exe は、インポートされるタイプ ライブラリが参照しているすべての外部タイプ ライブラリを再帰的に自動インポートします。 **/reference** オプションを指定した場合、このツールは、他のタイプ ライブラリをインポートする前に、参照されたアセンブリ内の外部型を解決しようとします。|  
|**/silence:** `warningnumber`|指定された警告を表示しません。 このオプションは、 **/silent** オプションと共に使用することはできません。|  
|**/silent**|成功メッセージを表示しません。 このオプションは、 **/silence** オプションと共に使用することはできません。|  
|**/strictref**|ツールが現在のアセンブリ内、 **/reference** オプションで指定されたアセンブリ内、または登録されているプライマリ相互運用機能アセンブリ (PIA) 内のすべての参照を解決できない場合は、タイプ ライブラリをインポートしません。|  
|**/strictref:nopia**|**/strictref** と同じですが、PIA を無視します。|  
|**/sysarray**|ツールが COM スタイル SafeArray をマネージド <xref:System.Array> 型としてインポートするように指定します。|  
|**/tlbreference:** *filename*|レジストリを参照せずにタイプ ライブラリ参照を解決する場合に使用するタイプ ライブラリ ファイルを指定します。<br /><br /> このオプションでは、古いタイプ ライブラリの形式の一部が読み込まれないことに注意してください。  ただし、古いタイプ ライブラリの形式は、レジストリまたは現在のディレクトリから暗黙的に読み込むことができます。|  
|**/trademark:** `trademarkinformation`|出力アセンブリに商標情報を追加します。 この情報は、アセンブリの **[ファイルのプロパティ]** ダイアログ ボックスで確認できます。|  
|**/transform:** *transformname*|*transformname* パラメーターで指定されたようにメタデータを変換します。<br /><br /> ディスパッチ専用インターフェイス (dispinterface) のメソッドの [out, retval] パラメーターを戻り値に変換するには、*transformname* パラメーターに **dispret** を指定します。<br /><br /> このオプションの詳細については、このトピックの下記の例を参照してください。|  
|**/unsafe**|.NET Framework セキュリティ チェックなしでインターフェイスを生成します。 この方法で公開されているメソッドを呼び出す場合には、セキュリティ上のリスクが伴います。 このオプションは、上記に該当するコードを公開するリスクを理解した上で使用してください。|  
|**/verbose**|詳細モードを指定します。インポートされたタイプ ライブラリについての詳細情報を表示します。|  
|**/VariantBoolFieldToBool**|構造体の `VARIANT_BOOL` フィールドを <xref:System.Boolean> に変換します。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
> [!NOTE]
> Tlbimp.exe のコマンド ライン オプションでは、大文字と小文字が区別されません。また、これらのオプションは任意の順序で指定できます。 オプションを一意に識別するために十分である場合は、オプションの一部を指定するだけでもかまいません。 したがって、 **/n** と指定した場合は **/nologo**、 **/ou:** *outfile.dll* と指定した場合は **/out:** *outfile.dll* であると見なされます。  
  
## <a name="remarks"></a>解説  
 Tlbimp.exe は、タイプ ライブラリ全体の変換を一括して実行します。 このツールを使用しても、単一のタイプ ライブラリで定義されている型のサブセットに関する型情報は生成できません。  
  
 アセンブリへの[厳密な名前](../../standard/assembly/strong-named.md)の割り当てを許可しておくと、さまざまな場合に役立ちます。この割り当てが必須であることもあります。 このため、Tlbimp.exe には、厳密な名前を持つアセンブリを生成するために必要な情報を提供するオプションが用意されています。 **/keyfile:** オプションと **/keycontainer:** オプションは、両方ともアセンブリに厳密な名前で署名します。 したがって、これらのオプションを両方同時に指定しないでください。  
  
 **/reference** オプションを複数回使用すると、複数の参照アセンブリを指定できます。
 
 Tlbimp.exe によるアセンブリの生成方法のため、アセンブリのターゲットを `mscorlib` の別のバージョンに変更することはできません。 たとえば、.NET Framework 2.0 をターゲットとするアセンブリを生成したい場合は、.NET Framework 2.0/3.0/3.5 SDK に付属する Tlbimp.exe を使用する必要があります。 .NET Framework 4.x をターゲットにするには、.NET Framework 4.x SDK に付属する Tlbimp.exe を使用する必要があります。
 
 複数のタイプ ライブラリを格納するモジュールから 1 つのタイプ ライブラリをインポートするときに、タイプ ライブラリ ファイルにリソース ID を追加することもできます。 Tlbimp.exe は、このファイルが現在のディレクトリにあるか、ユーザーが完全パスを指定した場合にだけこのファイルを認識できます。 このトピックの下記の例を参照してください。  
  
## <a name="examples"></a>使用例  
 `myTest.tlb` 内で見つかったタイプ ライブラリと同じ名前で、.dll 拡張子を持つアセンブリを生成するコマンドを次に示します。  
  
```console  
tlbimp myTest.tlb   
```  
  
 `myTest.dll` という名前を持つアセンブリを生成するコマンドを次に示します。  
  
```console  
tlbimp  myTest.tlb  /out:myTest.dll  
```  
  
 `MyModule.dll\1` で指定されているタイプ ライブラリと同じ名前で、.dll 拡張子を持つアセンブリを生成するコマンドを次に示します。 `MyModule.dll\1` は現在のディレクトリ内に置かれている必要があります。  
  
```console  
tlbimp MyModule.dll\1  
```  
  
 タイプ ライブラリ `myTestLib.dll` に対して、`TestLib.dll` という名前のアセンブリを生成するコマンドを次に示します。 **/transform:dispret** オプションは、タイプ ライブラリのディスパッチ インターフェイスに対するメソッドの [out, retval] パラメーターをマネージド ライブラリの戻り値に変換します。  
  
```console  
tlbimp TestLib.dll /transform:dispret /out:myTestLib.dll  
```  
  
 上記の例のタイプ ライブラリ `TestLib.dll` には、void 型を返し、[out, retval] パラメーターを持つ、`SomeMethod` という名前のディスパッチ インターフェイス メソッドが含まれています。 `SomeMethod` の `TestLib.dll` に対する入力タイプ ライブラリ メソッド シグネチャのコードを次に示します。  
  
```cpp  
void SomeMethod([out, retval] VARIANT_BOOL*);  
```  
  
 **/transform:dispret** オプションを指定すると、Tlbimp.exe は、`SomeMethod` の `[out, retval]` パラメーターを `bool` 型の戻り値に変換します。 **/transform:dispret** オプションを指定した場合に、Tlbimp.exe が、マネージド ライブラリ `myTestLib.dll` で `SomeMethod` に対して生成するメソッド シグネチャを次に示します。  
  
```csharp  
bool SomeMethod();  
```  
  
 Tlbimp.exe を使用して、 **/transform:dispret** を指定せずに `TestLib.dll` のマネージド ライブラリを生成すると、マネージド ライブラリ `myTestLib.dll` には `SomeMethod` の次のメソッド シグネチャが生成されます。  
  
```csharp  
void SomeMethod(out bool x);  
```  
  
## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [Tlbexp.exe (タイプ ライブラリ エクスポーター)](tlbexp-exe-type-library-exporter.md)
- [タイプ ライブラリのアセンブリとしてのインポート](../interop/importing-a-type-library-as-an-assembly.md)
- [タイプ ライブラリからアセンブリへの変換の要約](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Ildasm.exe (IL 逆アセンブラー)](ildasm-exe-il-disassembler.md)
- [Sn.exe (厳密名ツール)](sn-exe-strong-name-tool.md)
- [厳密な名前付きアセンブリ](../../standard/assembly/strong-named.md)
- [タイプ ライブラリを相互運用機能アセンブリにインポートするための属性](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/y6a7ak23(v=vs.100))
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
