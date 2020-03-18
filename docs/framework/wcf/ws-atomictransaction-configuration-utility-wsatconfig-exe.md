---
title: WS-AtomicTransaction 構成ユーティリティ (wsatConfig.exe)
ms.date: 03/30/2017
ms.assetid: 1c56cf98-3963-46d5-a4e1-482deae58c58
ms.openlocfilehash: 009be8d4c25fd9db5b2f2df6e75fb046e92f389a
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094723"
---
# <a name="ws-atomictransaction-configuration-utility-wsatconfigexe"></a>WS-AtomicTransaction 構成ユーティリティ (wsatConfig.exe)

WS-AtomicTransaction 構成ユーティリティは、基本的な WS-AtomicTransaction サポート設定を構成するために使用されます。  
  
## <a name="syntax"></a>構文  
  
```console  
wsatConfig [Options]  
```  
  
## <a name="remarks"></a>解説  
 このコマンドラインツールを使用すると、ローカルコンピューターでのみ基本的な WS-AT 設定を構成できます。 ローカルコンピューターとリモートコンピューターの両方で設定を構成する必要がある場合は、「 [ws-atomictransaction のサポートを構成](./feature-details/configuring-ws-atomic-transaction-support.md)する」の説明に従って、MMC スナップインを使用する必要があります。  
  
 コマンドラインツールは、Windows SDK のインストール場所にあります。具体的には、  
  
 %SystemRoot%\Microsoft.Net\Framework\v3.0\Windows Communication Foundation\wsatConfig.exe  
  
 Windows XP または Windows Server 2003 を実行している場合は、WsatConfig .exe を実行する前に更新プログラムをダウンロードする必要があります。 この更新プログラムの詳細については、「 [update for Windows Communication Foundation」 (KB912817)](https://www.microsoft.com/download/details.aspx?id=21520)を参照してください。  
  
 次の表は、WS-AtomicTransaction 構成ユーティリティ (wsatConfig.exe) で使用できるオプションを示します。  
  
> [!NOTE]
> 選択したポートに SSL 証明書を設定すると、そのポートに関連付けられたオリジナルの SSL 証明書が上書きされます。  
  
|オプション|[説明]|  
|-------------|-----------------|  
|-accounts:\<アカウント、>|WS-AtomicTransaction に追加できるアカウントをコンマで区切って指定します。 これらのアカウントの有効性の確認は行われません。|  
|-accountsCerts:\<thumb >&#124;"Issuer\SubjectName", >|WS-AtomicTransaction に追加できる証明書をコンマで区切って指定します。 証明書は、サムプリントまたは Issuer\SubjectName ペアで示されます。 空の場合は、サブジェクト名に {EMPTY} を使用します。|  
|-endpointCert: < マシン&#124;\<thumb >&#124;"Issuer\SubjectName" >|コンピューターの証明書を使用するか、サムプリントまたは Issuer\SubjectName ペアで指定される別のローカル エンドポイントの証明書を使用します。 空の場合は、サブジェクト名に {EMPTY} を使用します。|  
|-maxTimeout:\<sec >|最大タイムアウトを秒単位で指定します。 有効な値は 0 ~ 3600 です。|  
|-network:\<disable&#124;> を有効にします|WS-AtomicTransaction ネットワーク サポートを有効または無効にします。|  
|-port:\<portNum >|WS-AtomicTransaction の HTTPS ポートを設定します。<br /><br /> このツールを実行する前にファイアウォールが既に有効な場合、ポートは例外の一覧に自動的に登録されます。 このツールを実行する前にファイアウォールが無効な場合は、ファイアウォールに関する追加の構成はありません。<br /><br /> WS-AT の構成後にファイアウォールを有効にする場合は、このツールを再度実行し、このパラメーターを使用してポート番号を指定する必要があります。 WS-AT の構成後にファイアウォールを無効にする場合は、入力を追加しないで WS-AT の動作を続行します。|  
|-timeout:\<sec >|既定のタイムアウトを秒単位で指定します。 有効な値は 1 ～ 3600 の範囲です。|  
|-traceActivity:\<無効&#124;> を有効にします|アクティビティ イベントのトレースを有効または無効にします。|  
|-traceLevel:\<オフ&#124;エラー&#124;重大&#124;警告&#124;情報&#124; &#124;すべて >}|トレース レベルを指定します。|  
|-tracePII:\<disable&#124;> を有効にします|個人を特定できる情報のトレースを有効または無効にします。|  
|-traceProp:\<disable&#124;> を有効にします|伝達イベントのトレースを有効または無効にします。|  
|-restart|MSDTC を再起動して変更を直ちに反映します。 これが指定されていない場合、変更は、MSDTC が再起動されたときに有効になります。|  
|-show|現在の WS-AtomicTransaction プロトコル設定を表示します。|  
|-virtualServer:\<virtualServer >|DTC リソース クラスター名を指定します。|  
  
## <a name="see-also"></a>参照

- [WS-AtomicTransaction の使用](./feature-details/using-ws-atomictransaction.md)
- [WS-AtomicTransaction サポートの構成](./feature-details/configuring-ws-atomic-transaction-support.md)
