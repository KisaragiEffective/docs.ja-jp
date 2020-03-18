---
title: Tlbexp.exe (タイプ ライブラリ エクスポーター)
ms.date: 03/30/2017
helpviewer_keywords:
- exporting type library [.NET Framework]
- exporter tool [.NET Framework]
- Tlbexp.exe
- Type Library Exporter
- type libraries [.NET Framework], exporting
ms.assetid: a487d61b-d166-467b-a7ca-d8b52fbff42d
ms.openlocfilehash: f421a9865e457a4e8e08644671efb55c731db28b
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73104346"
---
# <a name="tlbexpexe-type-library-exporter"></a>Tlbexp.exe (タイプ ライブラリ エクスポーター)
タイプ ライブラリ エクスポーターは、共通言語ランタイム アセンブリで定義されている型を記述するタイプ ライブラリを生成します。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
tlbexp assemblyName [options]  
```  
  
## <a name="parameters"></a>パラメーター  
  
|引数|説明|  
|--------------|-----------------|  
|*assemblyName*|タイプ ライブラリをエクスポートする対象のアセンブリ。|  
  
|オプション|説明|  
|------------|-----------------|  
|**/asmpath:** *directory*|アセンブリを検索する場所を指定します。 このオプションを使用する場合は、現在のディレクトリを含め、参照アセンブリの検索場所を明示的に指定する必要があります。<br /><br /> **asmpath** オプションを使用すると、タイプ ライブラリ エクスポーターは、グローバル アセンブリ キャッシュ (GAC) 内のアセンブリを検索しません。|  
|**/help**|このツールのコマンド構文とオプションを表示します。|  
|**/names:** *filename*|タイプ ライブラリにおける大文字の使用スタイルを指定します。 *filename* 引数はテキスト ファイルです。 このファイルには、タイプ ライブラリ内の名前に対する大文字の使用スタイルを 1 行に 1 つずつ指定します。|  
|**/nologo**|Microsoft 著作権情報を表示しません。|  
|**/oldnames**|型名が競合している場合は、Tlbexp.exe は装飾された型名をエクスポートします。 バージョン 2.0 より以前の .NET Framework では、これが既定の動作であることに注意してください。|  
|**/out:** *file*|生成するタイプ ライブラリ ファイルの名前を指定します。 このオプションを省略した場合、アセンブリ名に拡張子 .tlb を付加した名前のタイプ ライブラリが生成されます。このアセンブリ名は実際のアセンブリ名ですが、アセンブリを含むファイルの名前と必ずしも同じである必要はありません。|  
|**/silence:** `warningnumber`|指定された警告を表示しません。 このオプションは、 **/silent** オプションと共に使用することはできません。|  
|**/silent**|成功メッセージを表示しません。 このオプションは、 **/silence** オプションと共に使用することはできません。|  
|**/tlbreference:** *typelibraryname*|Tlbexp.exe はレジストリを参照せずに、タイプ ライブラリの参照を明示的に解決します。 たとえば、アセンブリ B がアセンブリ A を参照する場合は、レジストリで指定されているタイプ ライブラリを参照するのではなく、このオプションを使用してタイプ ライブラリの参照を明示的に指定できます。 Tlbexp.exe はバージョン チェックを実行して、タイプ ライブラリとアセンブリのバージョンが同じであることを確認します。バージョンが異なる場合は、エラーを生成します。<br /><br /> **tlbreference** オプションを指定しても、<xref:System.Runtime.InteropServices.ComImportAttribute> 属性の適用先のインターフェイスが別の型に実装される場合には、レジストリが参照されます。|  
|**/tlbrefpath:** *path*|参照タイプ ライブラリの絶対パス。|  
|**/win32**|このオプションを指定すると、64 ビットのコンピューターでコンパイルする場合、Tlbexp.exe は 32 ビットのタイプ ライブラリを生成します。|  
|**/win64**|このオプションを指定すると、32 ビットのコンピューターでコンパイルする場合、Tlbexp.exe は 64 ビットのタイプ ライブラリを生成します。|  
|**/verbose**|詳細出力モードを指定します。このモードでは、タイプ ライブラリを生成する必要のある参照先アセンブリがある場合は、その一覧を表示します。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
> [!NOTE]
> Tlbexp.exe のコマンド ライン オプションでは、大文字と小文字が区別されません。また、これらのオプションは任意の順序で指定できます。 オプションを一意に識別するために十分である場合は、オプションの一部を指定するだけでもかまいません。 たとえば、 **/n** を指定した場合は **/nologo**、 **/o:** *outfile.tlb* と指定した場合は **/out:** *outfile.tlb* であると見なされます。  
  
## <a name="remarks"></a>解説  
 Tlbexp.exe は、アセンブリで定義されている型の定義を含む、タイプ ライブラリを生成します。 Visual Basic 6.0 などのアプリケーションは、このツールで生成されたタイプ ライブラリを使用して、アセンブリ内で定義されている .NET 型にバインドできます。  
  
> [!IMPORTANT]
> Tlbexp.exe を使用して Windows メタデータ (.winmd) ファイルをエクスポートすることはできません。 Windows ランタイム アセンブリのエクスポートはサポートされていません。  
  
 アセンブリ全体が一度に変換されます。 Tlbexp.exe を使用しても、アセンブリで定義されている型のサブセットに関する型情報は生成できません。  
  
 Tlbexp.exe を使用して、[タイプ ライブラリ インポーター (Tlbimp.exe)](tlbimp-exe-type-library-importer.md) でインポートしたアセンブリからタイプ ライブラリを生成することもできません。 代わりに、Tlbimp.exe でインポートした元のタイプ ライブラリを参照する必要があります。 しかし、Tlbimp.exe でインポートしたアセンブリを参照するアセンブリから、タイプ ライブラリをエクスポートすることはできます。 後の例を参照してください。  
  
 Tlbexp.exe は、生成したタイプ ライブラリを現在のディレクトリ、または出力ファイルとして指定したディレクトリに格納します。 単一のアセンブリから複数のタイプ ライブラリが生成されることもあります。  
  
 Tlbexp.exe はタイプ ライブラリを生成しますが、生成したタイプ ライブラリの登録は行いません。 これは、タイプ ライブラリの生成と登録を両方とも行う[アセンブリ登録ツール (Regasm.exe)](regasm-exe-assembly-registration-tool.md) とは対照的です。 タイプ ライブラリを生成して COM に登録するには、Regasm.exe を使用します。  
  
 `/win32` オプションまたは `/win64` オプションを指定しないと、Tlbexp.exe はコンパイルを実行するコンピューターのタイプ (32 ビット コンピューターまたは 64 ビット コンピューター) に応じて、32 ビットまたは 64 ビットのタイプ ライブラリを生成します。 クロス コンパイルを行う場合は、32 ビット コンピューター上で `/win64` オプションを使用して 64 ビットのタイプ ライブラリを生成したり、64 ビット コンピューター上で `/win32` オプションを使用して 32 ビットのタイプ ライブラリを生成したりできます。 32 ビットのタイプ ライブラリでは、<xref:System.Runtime.InteropServices.ComTypes.SYSKIND> 値は <xref:System.Runtime.InteropServices.ComTypes.SYSKIND.SYS_WIN32> に設定されます。 64 ビットのタイプ ライブラリでは、<xref:System.Runtime.InteropServices.ComTypes.SYSKIND> 値は <xref:System.Runtime.InteropServices.ComTypes.SYSKIND.SYS_WIN64> に設定されます。 すべてのデータ型変換 (たとえば、`IntPtr` や `UIntPtr` などのポインター-サイズ データ型) は、適切に変換されます。  
  
 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性を使用して、<xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType> または `VT_UNKOWN` の `VT_DISPATCH` 値を指定すると、Tlbexp.exe は後続で使用される <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType> フィールドをすべて無視します。 たとえば、次のようなシグネチャがあるとします。  
  
```csharp
[return:MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VarEnum.VT_UNKNOWN, SafeArrayUserDefinedSubType=typeof(ConsoleKeyInfo))] public Array StructUnkSafe(){return null;}  
[return:MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VarEnum.VT_DISPATCH, SafeArrayUserDefinedSubType=typeof(ConsoleKeyInfo))] public Array StructDispSafe(){return null;}  
```  
  
 次のタイプ ライブラリが生成されます。  
  
```cpp 
[id(0x60020004)]  
HRESULT StructUnkSafe([out, retval] SAFEARRAY(IUnknown*)* pRetVal);  
[id(0x60020005)]  
HRESULT StructDispSafe([out, retval] SAFEARRAY(IDispatch*)* pRetVal);  
```  
  
 Tlbexp.exe は <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType> フィールドを無視することに注意してください。  
  
 アセンブリ内にあるすべての情報をタイプ ライブラリに含めることはできないので、エクスポート プロセス中、Tlbexp.exe によって一部のデータが破棄される場合があります。 変換処理、およびタイプ ライブラリに出力される各情報のソースの識別については、「[アセンブリからタイプ ライブラリへの変換の要約](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))」を参照してください。  
  
 タイプ ライブラリ エクスポーターは、<xref:System.TypedReference> オブジェクトがアンマネージ コードで意味を持たない場合でも、`VARIANT` パラメーターが <xref:System.TypedReference> であるメソッドをエクスポートします。 <xref:System.TypedReference> パラメーターを持つメソッドをエクスポートしても、タイプ ライブラリ エクスポーターは警告やエラーを生成しません。作成されたタイプ ライブラリを使用するアンマネージ コードは、正しく動作しません。  
  
 タイプ ライブラリ エクスポーターは、Microsoft Windows 2000 以降でサポートされます。  
  
## <a name="examples"></a>使用例  
 `myTest.dll` 内で見つかったアセンブリと同じ名前を持つタイプ ライブラリを生成するコマンドを次に示します。  
  
```console  
tlbexp myTest.dll  
```  
  
 `clipper.tlb` という名前を持つタイプ ライブラリを生成するコマンドを次に示します。  
  
```console  
tlbexp myTest.dll /out:clipper.tlb  
```  
  
 Tlbexp.exe を使用してインポートしたアセンブリを参照するアセンブリから、Tlbimp.exe を使用してタイプ ライブラリをエクスポートする例を次に示します。  
  
 まず、Tlbimp.exe を使用してタイプ ライブラリ `myLib.tlb` をインポートし、`myLib.dll` として保存します。  
  
```console  
tlbimp myLib.tlb /out:myLib.dll  
```  
  
 上記の例で作成した `Sample.dll,` を参照する `myLib.dll` を C# コンパイラでコンパイルするコマンドを次に示します。  
  
```console  
CSC Sample.cs /reference:myLib.dll /out:Sample.dll  
```  
  
 `Sample.dll` を参照する `myLib.dll` 用のタイプ ライブラリを生成するコマンドを次に示します。  
  
```console  
tlbexp Sample.dll  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.InteropServices.TypeLibExporterFlags>
- [ツール](index.md)
- [Regasm.exe (アセンブリ登録ツール)](regasm-exe-assembly-registration-tool.md)
- [アセンブリからタイプ ライブラリへの変換の要約](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))
- [Tlbimp.exe (タイプ ライブラリ インポーター)](tlbimp-exe-type-library-importer.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
