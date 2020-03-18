---
title: Installutil.exe (インストーラー ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- uninstalling server resources
- removing server resources
- status information for installation
- installation progress reports
- installing server resources
- Installer tool
- Installutil.exe
- files, Installer tool
- progress information for installation
- reporting installation progress
ms.assetid: 3f9d0533-f895-4897-b4ea-528284e0241d
ms.openlocfilehash: caca946617c681ce6516b7184a9ea506cc67158d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73105074"
---
# <a name="installutilexe-installer-tool"></a>Installutil.exe (インストーラー ツール)

インストーラー ツールは、指定したアセンブリ内のインストーラー コンポーネントを実行することによってサーバー リソースのインストールとアンインストールを実行できるコマンド ライン ユーティリティです。 このツールは、<xref:System.Configuration.Install> 名前空間のクラスと組み合わせることによって動作します。

このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

コマンド プロンプトに次のように入力します。

## <a name="syntax"></a>構文

```console
installutil [/u[ninstall]] [options] assembly [[options] assembly] ...
```

## <a name="parameters"></a>パラメーター

|引数|説明|
|--------------|-----------------|
|`assembly`|インストーラー コンポーネントを実行するアセンブリのファイル名。 `/AssemblyName` オプションを使用してアセンブリの厳密な名前を指定する場合は、このパラメーターを省略します。|

<a name="options"></a>

## <a name="options"></a>オプション

|オプション|説明|
|------------|-----------------|
|`/h[elp]`<br /><br /> または<br /><br /> `/?`|このツールのコマンド構文とオプションを表示します。|
|`/help` *assembly*<br /><br /> または<br /><br /> `/?` *assembly*|InstallUtil.exe のコマンド構文とオプションと共に、指定したアセンブリ内でそれぞれのインストーラーによって認識される追加オプションを表示します。 このオプションを指定すると、各インストーラー コンポーネントの <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> プロパティによって返されるテキストが InstallUtil.exe のヘルプ テキストに追加されます。|
|`/AssemblyName` "*assemblyName*<br /><br /> ,Version=*major.minor.build.revision*<br /><br /> ,Culture=*locale*<br /><br /> ,PublicKeyToken=*publicKeyToken*"|グローバル アセンブリ キャッシュに登録されている必要があるアセンブリの厳密な名前を指定します。 アセンブリ名は、バージョン、カルチャ、およびアセンブリの公開キー トークンによって完全修飾にする必要があります。 完全修飾名は、引用符で囲む必要があります。<br /><br /> たとえば、"myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0" は、完全修飾のアセンブリ名です。|
|`/InstallStateDir=[` *directoryName* `]`|アセンブリをアンインストールするために使用されるデータを格納する .InstallState ファイルのディレクトリを指定します。 既定のディレクトリは、アセンブリを格納しているディレクトリです。|
|`/LogFile=`[*filename*]|インストールの進行状況を記録するログ ファイルの名前を指定します。 既定では、`/LogFile` オプションを省略した場合は、*assemblyname*.InstallLog という名前のログ ファイルが作成されます。 *filename* を省略した場合、ログ ファイルは生成されません。|
|`/LogToConsole`={`true`&#124;`false`}|`true` の場合は出力がコンソールに表示されます。 `false` (既定値) の場合はコンソールへの出力が中止されます。|
|`/ShowCallStack`|インストール中に例外が発生した場合、コール スタックがログ ファイルに出力されます。|
|`/u`[`ninstall`]|指定されたアセンブリをアンインストールします。 他のオプションとは異なり、コマンド ラインのどこにオプションを指定するかとは無関係に、`/u` はすべてのアセンブリに適用されます。|

<a name="cmdline"></a>

## <a name="additional-installer-options"></a>その他のインストーラー オプション

アセンブリ内で使用されるそれぞれのインストーラーでは、「[オプション](#options)」で示したオプション以外のオプションも認識される場合があります。 これらのオプションを確認するには、コマンド ラインで `/?` オプションまたは `/help` オプションと共にアセンブリのパスを指定して InstallUtil.exe を実行します。 これらのオプションを指定するには、InstallUtil.exe で認識されるオプションと共にそれらのオプションをコマンド ラインに含めます。

> [!NOTE]
> 個々のインストーラー コンポーネントでサポートされるオプションのヘルプ テキストは、<xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> プロパティによって返されます。 コマンド ラインで入力された個々のオプションには、<xref:System.Configuration.Install.Installer.Context%2A?displayProperty=nameWithType> プロパティからプログラムでアクセスできます。

オプションとコマンド ライン パラメーターはすべてインストール ログ ファイルに書き込まれます。 ただし、一部のインストーラー コンポーネントで認識される `/Password` パラメーターを使用する場合、パスワード情報は 8 つのアスタリスク (*) に置き換えられ、ログ ファイルに表示されません。

> [!IMPORTANT]
> インストーラーに渡されるパラメーターには機密情報または個人を特定できる情報が含まれる場合があり、既定ではプレーンテキスト ログ ファイルに書き込まれます。 この動作を回避するには、コマンド ラインで Installutil.exe の後に (*filename* 引数を指定せずに) `/LogFile=` を指定して、ログ ファイルが生成されないようにします。

## <a name="remarks"></a>解説

.NET Framework アプリケーションは、従来のプログラム ファイルと関連リソースで構成されます。関連リソースには、メッセージ キュー、イベント ログ、パフォーマンス カウンターなどがあり、アプリケーションを配置するときにこれらのリソースを作成する必要があります。 アセンブリのインストーラー コンポーネントを使用すると、アプリケーションのインストール時にこれらのリソースを作成したり、アプリケーションのアンインストール時に削除したりできます。 Installutil.exe は、これらのインストーラー コンポーネントを検出して実行します。

同じコマンド行に複数のアセンブリを指定できます。 アセンブリ名の前に指定したオプションは、そのアセンブリのインストールに適用されます。 `/u` と `/AssemblyName` 以外のオプションは、累積されますがオーバーライドできます。 つまり、あるアセンブリに指定したオプションは、そのオプションを新しい値と共に指定しない限り、後続のすべてのアセンブリに適用されます。

オプションを指定せずにアセンブリに対して Installutil.exe を実行すると、次の 3 つのファイルがアセンブリのディレクトリ内に作成されます。

- InstallUtil.InstallLog - インストールの進行状況に関する一般的な説明が含まれます。

- *assemblyname*.InstallLog - インストール プロセスのコミット フェーズに固有の情報が含まれます。 コミット フェーズの詳細については、「<xref:System.Configuration.Install.Installer.Commit%2A> メソッド」を参照してください。

- *assemblyname*.InstallState - アセンブリをアンインストールするために使用されるデータが含まれます。

Installutil.exe は、リフレクションを使用して指定されたアセンブリを調査し、<xref:System.Configuration.Install.Installer> 属性が <xref:System.ComponentModel.RunInstallerAttribute?displayProperty=nameWithType> に設定されたすべての `true` タイプを検索します。 次に、<xref:System.Configuration.Install.Installer.Install%2A?displayProperty=nameWithType> タイプの各インスタンスについて、<xref:System.Configuration.Install.Installer.Uninstall%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Configuration.Install.Installer> メソッドを実行します。 Installutil.exe は、トランザクション的な方法でインストールを実行します。つまり、いずれかのアセンブリのインストールに失敗した場合には、他のすべてのアセンブリのインストールをロールバックします。 アンインストールはトランザクション的な方法では実行されません。

Installutil.exe では、遅延署名されたアセンブリのインストールおよびアンインストールはできませんが、厳密な名前付きアセンブリのインストールおよびアンインストールはできます。

.NET Framework Version 2.0 から、32 ビット バージョンの共通言語ランタイム (CLR) は 32 ビット バージョンのインストーラー ツールにのみ付属し、64 ビット バージョンの CLR は 32 ビットおよび 64 ビットの両方のバージョンのインストーラー ツールに付属するようになりました。 64 ビットの CLR を使用しているときは、32 ビットのアセンブリをインストールするには 32 ビットのインストーラーを使用し、64 ビットおよび Microsoft Intermediate Language (MSIL) のアセンブリをインストールするには 64 ビットのインストーラーを使用します。 どちらのバージョンのインストーラー ツールも同様に動作します。

C++ を使用して作成した Windows サービスを Installutil.exe で配置することはできません。Installutil.exe は、C++ コンパイラで作成される埋め込みのネイティブ コードを認識できません。 C++ Windows サービスを Installutil.exe で配置しようとすると、<xref:System.BadImageFormatException> などの例外がスローされます。 これを行うには、サービス コードを C++ モジュールに移行し、インストーラー オブジェクトを C# または Visual Basic で記述します。

## <a name="examples"></a>使用例

InstallUtil.exe のコマンド構文とオプションの説明を表示するコマンドを次に示します。

```console
installutil /?
```

InstallUtil.exe のコマンド構文とオプションの説明を表示するコマンドを次に示します。 インストーラーの <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> プロパティにヘルプ テキストが割り当てられている場合は、`myAssembly.exe` のインストーラー コンポーネントでサポートされるオプションの説明とリストも表示されます。

```console
installutil /? myAssembly.exe
```

アセンブリ `myAssembly.exe` 内のインストーラー コンポーネントを実行するコマンドを次に示します。

```console
installutil myAssembly.exe
```

`/AssemblyName` スイッチと完全修飾名を使用してアセンブリのインストーラー コンポーネントを実行するコマンドを次に示します。

```console
installutil /AssemblyName "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0"
```

ファイル名で指定されたアセンブリと厳密な名前で指定されたアセンブリのインストーラー コンポーネントを実行するコマンドを次に示します。 `/AssemblyName` オプションはオーバーライドできないので、コマンド ラインではファイル名で指定するすべてのアセンブリを厳密な名前で指定するアセンブリよりも前に記述する必要があります。

```console
installutil myAssembly.exe /AssemblyName "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0"
```

アセンブリ `myAssembly.exe` 内のアンインストーラー コンポーネントを実行するコマンドを次に示します。

```console
installutil /u myAssembly.exe
```

`myAssembly1.exe` アセンブリと `myAssembly2.exe` アセンブリ内のアンインストーラー コンポーネントを実行するコマンドを次に示します。

```console
installutil myAssembly1.exe /u myAssembly2.exe
```

コマンド ラインでの `/u` オプションの位置は重要ではないので、これは次のコマンドと同等です。

```console
installutil /u myAssembly1.exe myAssembly2.exe
```

アセンブリ `myAssembly.exe` 内のインストーラーを実行し、進行状況に関する情報を `myLog.InstallLog` に書き込むように指定するコマンドを次に示します。

```console
installutil /LogFile=myLog.InstallLog myAssembly.exe
```

アセンブリ `myAssembly.exe` 内のインストーラーを実行し、進行状況に関する情報を `myLog.InstallLog` に書き込むように指定し、インストーラーのカスタム オプション `/reg` を使用してシステム レジストリを更新するように指定するコマンドを次に示します。

```console
installutil /LogFile=myLog.InstallLog /reg=true myAssembly.exe
```

アセンブリ `myAssembly.exe` 内のインストーラーを実行し、インストーラーのカスタム オプション `/email` を使用してユーザーの電子メール アドレスを指定し、ログ ファイルに出力しないようにするコマンドを次に示します。

```console
installutil /LogFile= /email=admin@mycompany.com myAssembly.exe
```

`myAssembly.exe` に関するインストールの進行状況を `myLog.InstallLog` に書き込み、`myTestAssembly.exe` に関する進行状況を `myTestLog.InstallLog` に書き込むコマンドを次に示します。

```console
installutil /LogFile=myLog.InstallLog myAssembly.exe /LogFile=myTestLog.InstallLog myTestAssembly.exe
```

## <a name="see-also"></a>関連項目

- <xref:System.Configuration.Install>
- [ツール](index.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
