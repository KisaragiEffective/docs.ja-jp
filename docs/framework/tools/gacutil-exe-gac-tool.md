---
title: Gacutil.exe (グローバル アセンブリ キャッシュ ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, viewing contents
- viewing assemblies in global assembly cache
- global assembly cache, manipulating contents
- GAC (global assembly cache), Gacutil.exe
- Gacutil.exe
- GAC (global assembly cache), viewing contents
- installing assemblies into global assembly cache
- removing assemblies from global assembly cache
- list of assemblies in global assembly cache
- cache [.NET Framework], global assembly cache
- GAC (global assembly cache), manipulating contents
- global assembly cache, Gacutil.exe
- Global Assembly Cache tool
ms.assetid: 4c7be9c8-72ae-481f-a01c-1a4716806e99
ms.openlocfilehash: 87f3cb799ba4e406906759e1facd19d00c8bdace
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107501"
---
# <a name="gacutilexe-global-assembly-cache-tool"></a>Gacutil.exe (グローバル アセンブリ キャッシュ ツール)

グローバル アセンブリ キャッシュ ツールを使用すると、グローバル アセンブリ キャッシュとダウンロード キャッシュの内容を表示および操作できます。

このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

コマンド プロンプトに次のように入力します。

## <a name="syntax"></a>構文

```console
gacutil [options] [assemblyName | assemblyPath | assemblyListFile]
```

## <a name="parameters"></a>パラメーター

|引数|説明|
|--------------|-----------------|
|*assemblyName*|アセンブリの名前。 `myAssembly` などの部分的に指定したアセンブリ名、または `myAssembly, Version=2.0.0.0, Culture=neutral, PublicKeyToken=0038abc9deabfle5` などの完全に指定したアセンブリ名を指定できます。|
|*assemblyPath*|アセンブリ マニフェストを含むファイルの名前。|
|*assemblyListFile*|インストールまたはアンインストールするアセンブリを一覧表示する ANSI テキスト ファイルへのパス。 テキスト ファイルを使用してアセンブリをインストールするには、ファイルの行ごとに各アセンブリへのパスを指定します。 このツールは、*assemblyListFile* の位置を基準にして相対パスを解釈します。 テキスト ファイルを使用してアセンブリをアンインストールするには、ファイルの行ごとに各アセンブリの完全限定アセンブリ名を指定します。 このトピックの後半にある *assemblyListFile* の内容の例を参照してください。|

|オプション|説明|
|------------|-----------------|
|**/cdl**|ダウンロード キャッシュの内容を削除します。|
|**/f**|このオプションを **/i** オプションまたは **/il** オプションと共に使用して、アセンブリを強制的に再インストールします。 同じ名前のアセンブリがグローバル アセンブリ キャッシュに既に存在する場合、既存のアセンブリは上書きされます。|
|**/h** **[elp]**|このツールのコマンド構文とオプションを表示します。|
|**/i** *assemblyPath*|アセンブリをグローバル アセンブリ キャッシュにインストールします。|
|**/if**  *assemblyPath*|アセンブリをグローバル アセンブリ キャッシュにインストールします。 同じ名前のアセンブリがグローバル アセンブリ キャッシュに既に存在する場合、既存のアセンブリは上書きされます。<br /><br /> このオプションを指定するのは、 **/i** オプションと **/f** オプションを一緒に指定するのと同じです。|
|**/il** *assemblyListFile*|*assemblyListFile* で指定された 1 つ以上のアセンブリをグローバル アセンブリ キャッシュにインストールします。|
|**/ir**  *assemblyPath*<br /><br /> *scheme*<br /><br /> *ID*<br /><br /> *description*|アセンブリをグローバル アセンブリ キャッシュにインストールし、そのアセンブリをカウントするための参照を追加します。 このオプションと共に *assemblyPath*、*scheme*、*id*、および *description* の各パラメーターを指定する必要があります。 これらのパラメーターとして指定できる有効な値については、 **/r** オプションを参照してください。<br /><br /> このオプションを指定するのは、 **/i** オプションと **/r** オプションを一緒に指定するのと同じです。|
|**/l** [*assemblyName*]|グローバル アセンブリ キャッシュの内容を一覧表示します。 *assemblyName* パラメーターを指定した場合は、その名前と一致するアセンブリだけが一覧表示されます。|
|**/ldl**|ダウンロードされたファイルのキャッシュの内容を一覧表示します。|
|**/l** [*assemblyName*]|すべてのアセンブリとそれらに該当する参照カウントを一覧表示します。 *assemblyName* パラメーターを指定した場合は、その名前と一致するアセンブリと、それらに該当する参照カウントだけが一覧表示されます。|
|**/nologo**|Microsoft 著作権情報を表示しません。|
|**/r** [*assemblyName &#124; assemblyPath*]<br /><br /> *scheme*<br /><br /> *ID*<br /><br /> *description*|インストールまたはアンインストールするアセンブリへのトレースされた参照を指定します。 このオプションは、 **/i**、 **/il**、 **/u**、または **/ul** の各オプションと共に指定します。<br /><br /> アセンブリをインストールするには、このオプションと共に *assemblyPath*、*scheme*、*id*、および *description* の各パラメーターを指定します。 アセンブリをアンインストールするには、*assemblyName*、*scheme*、*id*、および *description* の各パラメーターを指定します。<br /><br /> アセンブリへの参照を削除するには、アセンブリをインストールしたときに **/i** オプションおよび **/r** (または **/ir**) オプションと共に指定したのと同じ *scheme*、*id* および *description* の各パラメーターを指定する必要があります。 アセンブリをアンインストールする場合、それが削除する最後の参照で、Windows インストーラーにアセンブリへの未解決の参照がまったくないときには、アセンブリもグローバル アセンブリ キャッシュから削除されます。<br /><br /> *scheme* パラメーターはインストール スキームのタイプを指定します。 次のいずれかの値を指定できます。<br /><br /> - UNINSTALL_KEY: インストーラーがアプリケーションを Microsoft Windows の [アプリケーションの追加と削除] に追加する場合は、この値を指定します。 アプリケーションは、レジストリ キーを HKLM\Software\Microsoft\Windows\CurrentVersion に追加して、そのアプリケーション自体を [アプリケーションの追加と削除] に追加します。<br />- FILEPATH: インストーラーがアプリケーションを [アプリケーションの追加と削除] に追加しない場合は、この値を指定します。<br />- OPAQUE: レジストリ キーまたはファイル パスの指定をインストール シナリオで行わない場合は、この値を指定します。 この値を指定した場合には、*id* パラメーターとしてカスタム情報を指定できます。<br /><br /> *id* パラメーターとして指定する値は、*scheme* パラメーターに指定した値によって決まります。<br /><br /> - *scheme* パラメーターとして UNINSTALL_KEY を指定した場合は、HKLM\Software\Microsoft\Windows\CurrentVersion レジストリ キーに設定したアプリケーションの名前を指定します。 たとえば、レジストリ キーが HKLM\Software\Microsoft\Windows\CurrentVersion\MyApp の場合は、*id* パラメーターとして MyApp を指定します。<br />- *scheme* パラメーターとして FILEPATH を指定した場合は、*id* パラメーターとして、アセンブリをインストールする実行可能ファイルへの完全パスを指定します。<br />- *scheme* パラメーターとして OPAQUE を指定した場合は、*id* パラメーターとして任意のデータを指定できます。 指定するデータは二重引用符 ("") で囲む必要があります。<br /><br /> *description* パラメーターを使用して、インストールするアプリケーションに関する説明を指定できます。 この情報は、参照を列挙した場合に表示されます。|
|**/silent**|すべての出力を表示しません。|
|**/u**  *assemblyName*|アセンブリをグローバル アセンブリ キャッシュからアンインストールします。|
|**/uf**  *assemblyName*|指定したアセンブリへのすべての参照を削除して、アセンブリを強制的にアンインストールします。<br /><br /> このオプションを指定するのは、 **/u** オプションと **/f** オプションを一緒に指定するのと同じです。 **注:** このオプションを使用しても、Microsoft Windows インストーラーを使用してインストールされたアセンブリを削除することはできません。 削除しようとすると、エラー メッセージが表示されます。|
|**/ul** *assemblyListFile*|*assemblyListFile* で指定された 1 つ以上のアセンブリをグローバル アセンブリ キャッシュからアンインストールします。|
|**/u** **[ngen]** *assemblyName*|指定したアセンブリをグローバル アセンブリ キャッシュからアンインストールします。 指定したアセンブリが既存の参照カウントを持っている場合、参照カウントは表示されますが、アセンブリはグローバル アセンブリ キャッシュから削除されません。 **注:** .NET Framework Version 2.0 では、`/ungen` はサポートされていません。 代わりに、[Ngen.exe (ネイティブ イメージ ジェネレーター)](ngen-exe-native-image-generator.md)の `uninstall` コマンドを使用します。 <br /><br /> .NET Framework Version 1.0 と 1.1 で **/ungen** を指定すると、Gacutil.exe はネイティブ イメージ キャッシュからアセンブリを削除します。 このキャッシュは、[Ngen.exe (ネイティブ イメージ ジェネレーター)](ngen-exe-native-image-generator.md) を使用して作成したアセンブリのネイティブ イメージを格納します。|
|**/ur**  *assemblyName*<br /><br /> *scheme*<br /><br /> *ID*<br /><br /> *description*|指定したアセンブリへの参照をグローバル アセンブリ キャッシュからアンインストールします。 アセンブリへの参照を削除するには、アセンブリをインストールしたときに **/i** オプションおよび **/r** (または **/ir**) オプションと共に指定したのと同じ *scheme*、*id* および *description* の各パラメーターを指定する必要があります。 これらのパラメーターとして指定できる有効な値については、 **/r** オプションを参照してください。<br /><br /> このオプションを指定するのは、 **/u** オプションと **/r** オプションを一緒に指定するのと同じです。|
|**/?**|このツールのコマンド構文とオプションを表示します。|

## <a name="remarks"></a>解説

> [!NOTE]
> Gacutil.exe を使用するには管理者権限が必要です。

Gacutil.exe を使用すると、アセンブリのキャッシュへのインストールやキャッシュからの削除、およびキャッシュ内容の一覧表示を実行できます。

Gacutil.exe には、Windows インストーラーでサポートされる参照カウント スキームに類似した参照カウントをサポートするオプションが用意されています。 Gacutil.exe を使用して、同じアセンブリをインストールする 2 つのアプリケーションをインストールできます。このツールは、アセンブリへの参照の数を追跡します。 その結果、両方のアプリケーションがアンインストールされるまで、アセンブリはコンピューター上にとどまります。 実際の製品のインストールに Gacutil.exe を使用する場合は、参照カウントをサポートするオプションを使用してください。 アセンブリをインストールし、それをカウントするための参照を追加するには、 **/i** オプションと **/r** オプションを一緒に使用します。 アセンブリの参照カウントを削除するには、 **/u** オプションと **/r** オプションを一緒に使用します。 **/i** オプションと **/u** オプションを単独で使用しても、参照カウントはサポートされないので注意してください。 これらのオプションは、製品開発中に使用するのには適していますが、実際の製品のインストールには適していません。

ANSI テキスト ファイルに格納されているアセンブリの一覧をインストールまたはアンインストールするには、それぞれ **/il** オプションまたは **/ul** オプションを使用します。 テキスト ファイルの内容は正しく書式設定されている必要があります。 テキスト ファイルを使用してアセンブリをインストールするには、ファイルの行ごとに各アセンブリへのパスを指定します。 インストールするアセンブリを含むファイルの内容を表示する例を次に示します。

```text
myAssembly1.dll
myAssembly2.dll
myAssembly3.dll
```

テキスト ファイルを使用してアセンブリをアンインストールするには、ファイルの行ごとに各アセンブリの完全限定アセンブリ名を指定します。 アンインストールするアセンブリを含むファイルの内容を表示する例を次に示します。

```text
myAssembly1,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
myAssembly2,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
myAssembly3,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
```

> [!NOTE]
> (ファイル名拡張子を除して) 79 文字から 91 文字よりも長いファイル名のアセンブリをインストールしようとすると、次のエラーが発生する可能性があります。
>
> ```output
> Failure adding assembly to the cache:   The file name is too long.
> ```
>
> これは、Gacutil.exe によって、内部的に以下の要素で構成される MAX_PATH 文字までのパスが構築されるためです。
>
> - GAC ルート - 34 文字 (つまり `C:\Windows\Microsoft.NET\assembly\`)
> - Architecture - 7 文字または 9 文字 (つまり `GAC_32\`、`GAC_64\`、`GAC_MSIL`)
> - AssemblyName - 最大 91 文字。ただし、他の要素のサイズによって変わります (例: `System.Xml.Linq\`)
> - AssemblyInfo - 次から構成される 31 文字から 48 文字以上。
>   - Framework - 5 文字 (例: `v4.0_`)
>   - AssemblyVersion - 8 文字 24 文字 (例: `9.0.1000.0_`)
>   - AssemblyLanguage - 1 文字から 8 文字 (例: `de_`、`sr-Cyrl_`)
>   - PublicKey - 17 文字 (例: `31bf3856ad364e35\`)
> - DllFileName - 最大 91 文字 + 4 文字 (つまり `<AssemblyName>.dll`)

## <a name="examples"></a>使用例

アセンブリ `mydll.dll` をグローバル アセンブリ キャッシュにインストールするコマンドを次に示します。

```console
gacutil /i mydll.dll
```

アセンブリ `hello` への参照カウントがない場合に、そのアセンブリをグローバル アセンブリ キャッシュから削除するコマンドを次に示します。

```console
gacutil /u hello
```

上のコマンドでは、アセンブリ名が完全に指定されていないため、複数のアセンブリがアセンブリ キャッシュから削除される可能性もあります。 たとえば、キャッシュ内に `hello` のバージョン 1.0.0.0 と 3.2.2.1 が両方ともインストールされている場合にコマンド `gacutil /u hello` を実行すると、アセンブリは両方とも削除されます。

複数のアセンブリを削除しないようにするには、次の例のようなコードを使用します。 このコマンドでは、完全に指定したバージョン番号、カルチャ、および公開キーと一致するアセンブリ `hello` だけが削除されます。

```console
gacutil /u hello, Version=1.0.0.1, Culture="de",PublicKeyToken=45e343aae32233ca
```

ファイル `assemblyList.txt` で指定されたアセンブリをグローバル アセンブリ キャッシュにインストールするコマンドを次に示します。

```console
gacutil /il assemblyList.txt
```

ファイル `assemblyList.txt` で指定されたアセンブリをグローバル アセンブリ キャッシュから削除するコマンドを次に示します。

```console
gacutil /ul assemblyList.txt
```

次のコマンドにより `myDll.dll` がグローバル アセンブリ キャッシュにインストールされ、myDll.dll をカウントするための参照が追加されます。 アセンブリ `myDll.dll` はアプリケーション `MyApp` によって使用されます。 `UNINSTALL_KEY MyApp` パラメーターは、Windows の [アプリケーションの追加と削除] に `MyApp` を追加するレジストリ キーを指定します。 description パラメーターは `My Application Description` と指定されています。

```console
gacutil /i /r myDll.dll UNINSTALL_KEY MyApp "My Application Description"
```

次のコマンドにより `myDll.dll` がグローバル アセンブリ キャッシュにインストールされ、myDll.dll をカウントするための参照が追加されます。 scheme パラメーター `FILEPATH` と id パラメーター `c:\applications\myApp\myApp.exe` は、`myDll.dll.` をインストールするアプリケーションへのパスを指定します。description パラメーターは `MyApp` として指定されています。

```console
gacutil /i /r myDll.dll FILEPATH c:\applications\myApp\myApp.exe MyApp
```

次のコマンドにより `myDll.dll` がグローバル アセンブリ キャッシュにインストールされ、myDll.dll をカウントするための参照が追加されます。 scheme パラメーター `OPAQUE` が指定されているので、id パラメーターと description パラメーターをカスタマイズできます。

```console
gacutil /i /r mydll.dll OPAQUE "Insert custom application details here" "Insert Custom description information here"
```

アプリケーション `myDll.dll` による `myApp` への参照を削除するコマンドを次に示します。 これがアセンブリへの最後の参照の場合は、アセンブリもグローバル アセンブリ キャッシュから削除されます。

```console
gacutil /u /r myDll.dll FILEPATH c:\applications\myApp\myApp.exe MyApp
```

グローバル アセンブリ キャッシュの内容を一覧表示するコマンドを次に示します。

```console
gacutil /l
```

## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [グローバル アセンブリ キャッシュ](../app-domains/gac.md)
- [Regasm.exe (アセンブリ登録ツール)](regasm-exe-assembly-registration-tool.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
