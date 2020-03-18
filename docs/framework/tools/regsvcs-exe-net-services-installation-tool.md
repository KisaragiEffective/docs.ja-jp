---
title: Regsvcs.exe (.NET サービス インストール ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- Regsvcs.exe
- .NET Services Installation tool
- loading assemblies
- assemblies [.NET Framework], registering
- type libraries
- registering assemblies
ms.assetid: 5220fe58-5aaf-4e8e-8bc3-b78c63025804
ms.openlocfilehash: 1af93ae89d027bccdd52b9cd9c49f8e620303677
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73104942"
---
# <a name="regsvcsexe-net-services-installation-tool"></a>Regsvcs.exe (.NET サービス インストール ツール)
.NET サービス インストール ツールは、次のアクションを実行します。  
  
- アセンブリの読み込みと登録。  
  
- タイプ ライブラリの生成、登録、および指定された COM+ アプリケーションへのインストール。  
  
- プログラムでクラスに追加したサービスの設定。  
  
 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
      regsvcs [/c | /fc | /u] [/tlb:typeLibraryFile] [/extlb]  
[/reconfig] [/componly] [/appname:applicationName]  
[/nologo] [/quiet]assemblyFile.dll   
```  
  
## <a name="parameters"></a>パラメーター  
  
|引数|説明|  
|--------------|-----------------|  
|*assemblyFile.dll*|ソース アセンブリ ファイル。 アセンブリは、厳密な名前で署名する必要があります。 詳しくは、「[厳密な名前でのアセンブリへの署名](../../standard/assembly/sign-strong-name.md)」をご覧ください。|  
  
|オプション|説明|  
|------------|-----------------|  
|**/appdir:** *path*|アプリケーションのルート ディレクトリを示します。|  
|**/appname:** *applicationName*|検索または作成する COM+ アプリケーションの名前を指定します。|  
|**/c**|ターゲット アプリケーションを作成します。|  
|**/componly**|コンポーネントだけを設定します。メソッドとインターフェイスは無視されます。|  
|**/exapp**|このツールが既存のアプリケーションを要求するように指定します。|  
|**/extlb**|既存のタイプ ライブラリを使用します。|  
|**/fc**|対象アプリケーションを検索または作成します。|  
|**/help**|このツールのコマンド構文とオプションを表示します。|  
|**/noreconfig**|既存の対象アプリケーションを再設定しません。|  
|**/nologo**|Microsoft 著作権情報を表示しません。|  
|**/parname:** *name*|検索または作成する COM+ アプリケーションの名前または ID を指定します。|  
|**/reconfig**|既存の対象アプリケーションを再設定します。 既定値です。|  
|**/tlb:** *typelibraryfile*|インストールするタイプ ライブラリ ファイルを指定します。|  
|**/u**|対象アプリケーションをアンインストールします。|  
|**/quiet**|クワイエット モードを指定します。ロゴと成功メッセージは表示されません。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
## <a name="remarks"></a>解説  
 Regsvcs.exe には、*assemblyFile.dll* で指定されているソース アセンブリ ファイルが必要です。 このアセンブリは、厳密な名前で署名する必要があります。 厳密な名前での署名について詳しくは、「[厳密な名前でのアセンブリへの署名](../../standard/assembly/sign-strong-name.md)」をご覧ください。 対象アプリケーションおよびタイプ ライブラリの名前は、オプションです。 引数 *applicationName* はソース アセンブリ ファイルから生成できます。存在しない場合は、Regsvcs.exe によって作成されます。 引数 *typelibraryfile* にはタイプ ライブラリ名を指定できます。 タイプ ライブラリ名を指定しない場合、Regsvcs.exe は既定値としてアセンブリ名を使用します。  
  
 Regsvcs.exe がコンポーネントのメソッドを登録する場合は、それらのメソッドに対する[確認要求](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/9kc0c6st(v=vs.100))と[リンク確認要求](../misc/link-demands.md)の影響を受けます。 このツールは完全に信頼された環境で実行されるため、アクセス許可に対するほとんどの確認要求は成功します。 ただし、Regsvcs.exe では、<xref:System.Security.Permissions.StrongNameIdentityPermission> または <xref:System.Security.Permissions.PublisherIdentityPermission> に対する確認要求やリンク確認要求によって保護されたメソッドを含むコンポーネントを登録することはできません。  
  
 Regsvcs.exe を使用するには、ローカル コンピューターの管理特権が必要です。  
  
 上記のアクションの実行中に Regsvcs.exe が異常終了した場合は、対応するエラー メッセージが表示されます。  
  
## <a name="examples"></a>使用例  
 `myTest.dll` に含まれるすべてのパブリック クラスを `myTargetApp` (既存の COM+ アプリケーション) に追加し、タイプ ライブラリ `myTest.tlb` を生成するコマンドを次に示します。  
  
```console  
regsvcs /appname:myTargetApp myTest.dll  
```  
  
 `myTest.dll` に含まれるすべてのパブリック クラスを `myTargetApp` (既存の COM+ アプリケーション) に追加し、タイプ ライブラリ `newTest.tlb` を生成するコマンドを次に示します。  
  
```console  
regsvcs /appname:myTargetApp /tlb:newTest.tlb myTest.dll  
```  
  
## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [方法: 厳密な名前でアセンブリに署名する](../../standard/assembly/sign-strong-name.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
