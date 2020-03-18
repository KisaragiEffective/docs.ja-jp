---
title: ServiceModel 登録ツール (ServiceModelReg.exe)
ms.date: 03/30/2017
ms.assetid: 396ec5ae-e34f-4c64-a164-fcf50e86b6ac
ms.openlocfilehash: 2b2580a43270cc221de9cfdf0894a59a040ba307
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837767"
---
# <a name="servicemodel-registration-tool-servicemodelregexe"></a>ServiceModel 登録ツール (ServiceModelReg.exe)
このコマンド ライン ツールは、単一コンピューター上で WCF および WF コンポーネントの登録を管理するための機能を提供します。 WCF および WF コンポーネントはインストール時に構成されるため、通常の状況ではこのツールを使用する必要はありません。 しかし、サービスのアクティブ化に関する問題が発生する場合は、このツールを使用してコンポーネントを登録できます。  
  
## <a name="syntax"></a>構文  
  
```console  
ServiceModelReg.exe[(-ia|-ua|-r)|((-i|-u) -c:<command>)] [-v|-q] [-nologo] [-?]  
```  
  
## <a name="remarks"></a>Remarks  
 ツールは次の場所にあります。  
  
 %SystemRoot%\Microsoft.Net\Framework\v3.0\Windows Communication Foundation\  
  
> [!NOTE]
> ServiceModel 登録ツールが Windows Vista で実行されている場合、 **[windows の機能]** ダイアログが表示されないことがあります。 **Microsoft .NET Framework 3.0**の**Windows Communication Foundation HTTP Activation**オプションが有効になっていることを示します。 **[Windows の機能]** ダイアログボックスにアクセスするには、 **[スタート]** ボタンをクリックし、 **[実行]** をクリックしてから、「」**と入力し**ます。  
  
 次の表は、ServiceModelReg.exe で使用できるオプションを示します。  
  
|オプション|説明|  
|------------|-----------------|  
|`-ia`|WCF および WF のすべてのコンポーネントをインストールします。|  
|`-ua`|WCF および WF のすべてのコンポーネントをアンインストールします。|  
|`-r`|WCF および WF のすべてのコンポーネントを修復します。|  
|`-i`|-c で指定された WCF および WF のコンポーネントをインストールします。|  
|`-u`|-c で指定された WCF および WF のコンポーネントをアンインストールします。|  
|`-c`|コンポーネントをインストールまたはアンインストールします。<br /><br /> -httpnamespace-HTTP 名前空間の予約<br />-tcpportsharing-TCP ポート共有サービス<br />-tcpactivation – TCP activation service (.NET 4 クライアントプロファイルではサポートされていません)<br />-namedpipeactivation –名前付きパイプアクティブ化サービス (.NET 4 クライアントプロファイルではサポートされていません)<br />-msmqactivation – MSMQ アクティブ化サービス (.NET 4 クライアントプロファイルではサポートされていません)<br />-etw – ETW イベントトレースマニフェスト (Windows Vista 以降)|  
|`-q`|Quiet モード (エラー ログのみ表示)|  
|`-v`|詳細モード。|  
|`-nologo`|著作権やバナー メッセージを表示しません。|  
|`-?`|ヘルプ テキストを表示します。|  
  
## <a name="fixing-the-fileloadexception-error"></a>FileLoadException エラーの修正  
 以前のバージョンの WCF をコンピューターにインストールした場合、Servicemodelreg.exe ツールを実行して新しいインストールを登録すると、`FileLoadFoundException` エラーが発生することがあります。 旧バージョンのインストールから手動でファイルを削除しても、machine.config 設定が元のままである限り、このエラーが発生する可能性があります。  
  
 エラー メッセージは、次のようになります。  
  
```console  
Error: System.IO.FileLoadException: Could not load file or assembly 'System.ServiceModel, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference. (Exception from HRESULT: 0x80131040)  
File name: 'System.ServiceModel, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'  
```  
  
 System.ServiceModel Version 2.0.0.0 アセンブリが旧 CTP (Customer Technology Preview) リリースによってインストールされていたというエラー メッセージに注目する必要があります。 最新バージョンの System.ServiceModel アセンブリは、2.0.0.0 ではなく 3.0.0.0 です。 そのため、この問題は、WCF の初期リリースがインストールされているが、完全にはアンインストールされていないコンピューターに、正式な WCF リリースをインストールする場合に発生します。  
  
 ServiceModelReg.exe は、旧バージョンのエントリをクリーンアップすることも、新しいバージョンのエントリを登録することもできません。 唯一の回避策は、machine.config を手動で編集することです。このファイルは次の場所で見つけることができます。  
  
```console  
%windir%\Microsoft.NET\Framework\v2.0.50727\config\machine.config   
```  
  
 WCF を64ビットコンピューターで実行している場合は、この場所で同じファイルを編集する必要もあります。  
  
```console  
%windir%\Microsoft.NET\Framework64\v2.0.50727\config\machine.config   
```  
  
 このファイル内の "System.servicemodel, Version = 2.0.0.0" を参照する XML ノードを検索し、それらのノードを削除します。 ファイルを保存し ServiceModelReg.exe を再実行すると、この問題は解決します。  
  
## <a name="examples"></a>使用例  
 次の例に、ServiceModelReg.exe ツールの最も一般的なオプションの使用方法を示します。  
  
```console  
ServiceModelReg.exe -ia  
  Installs all components  
ServiceModelReg.exe -i -c:httpnamespace -c:etw  
  Installs HTTP namespace reservation and ETW manifests  
ServiceModelReg.exe -u -c:etw  
  Uninstalls ETW manifests  
ServiceModelReg.exe -r  
  Repairs an extended install  
```
