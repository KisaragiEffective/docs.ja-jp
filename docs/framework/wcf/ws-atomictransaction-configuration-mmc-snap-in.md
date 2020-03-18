---
title: WS-AtomicTransaction 構成 MMC スナップイン
ms.date: 03/30/2017
ms.assetid: 23592973-1d51-44cc-b887-bf8b0d801e9e
ms.openlocfilehash: 0bcd08f9a3450c850ead941df6313526d076df2d
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921343"
---
# <a name="ws-atomictransaction-configuration-mmc-snap-in"></a>Ws-atomictransaction 構成 MMC スナップイン

Ws-atomictransaction 構成 MMC スナップインは、ローカルコンピューターとリモートコンピューターの両方で ws-atomictransaction 設定の一部を構成するために使用されます。

## <a name="remarks"></a>Remarks

Windows XP または Windows Server 2003 を実行している場合は、MMC スナップインが表示されます。そのためには、コントロールパネル、管理ツール、**コンポーネントサービス** の順に移動し、**マイコンピューター**を右クリックして **プロパティ** を選択します。 これは MSDTC を構成する場合と同じ場所です。 構成できるオプションは、グループ化されて、 **WS-AT**タブです。

 Windows Vista または Windows Server 2008 を実行している場合は、MMC スナップインを見つけるには、 **[スタート]** ボタンをクリックし、 **[検索]** ボックスに `dcomcnfg.exe` 入力します。 MMC が開いているときに移動、**マイ Computer\Distributed トランザクション コーディネーター DTC**ノードを右クリックし、**プロパティ**です。 構成できるオプションは、グループ化されて、 **WS-AT**タブです。

 前の手順は、ローカル マシンを構成するためのスナップインを起動するために使用します。 リモートコンピューターを構成する場合は、[**コントロールパネル]、[管理ツール]、[コンポーネントサービス**]、の順に移動し、windows XP または windows Server 2003 を実行している場合は同様の手順を実行します。 Windows Vista または windows Server 2008 を実行している場合は、Vista と Windows Server 2008 の前の手順に従いますが、リモートコンピューターのノードの下にある **[分散トランザクション COORDINATOR\LOCAL DTC]** ノードを使用します。

 ツールのユーザー インターフェイスを使用するには、WsatUI.dll ファイルを登録する必要があります。このファイルのパスは次のとおりです。

 **%PROGRAMFILES%\Microsoft SDKs\Windows\v6.0\Bin\WsatUI.dll**

 登録するには、次のコマンドを実行します。

```console
regasm.exe /codebase WsatUI.dll
```

 このツールを使用すると、WS-AtomicTransaction の基本設定を変更できます。 たとえば、WS-AtomicTransaction プロトコル サポートの有効/無効の切り替え、WS-AT で使用する HTTP ポートの構成、SSL 証明書の HTTP ポートへのバインド、証明書のサブジェクト名指定による証明書の構成、トレース モードの選択とタイムアウトの既定および上限の設定を行うことができます。

 ローカルコンピューターでのみ ws-atomictransaction サポートを構成する必要がある場合は、このツールのコマンドラインバージョンを使用できます。 コマンドラインツールの詳細については、「ws-atomictransaction[構成ユーティリティ (wsatConfig .exe)](ws-atomictransaction-configuration-utility-wsatconfig-exe.md) 」トピックを参照してください。

 MMC スナップインとコマンド ライン ツールはいずれも、すべての WS-AT 設定を構成できるわけではありません。 これらの設定は、レジストリを直接変更することによってのみ編集できます。 これらのレジストリ設定の詳細については、「 [ws-atomictransaction のサポートの構成](./feature-details/configuring-ws-atomic-transaction-support.md)」を参照してください。

### <a name="user-interface-description"></a>ユーザー インターフェイスの説明

**Ws-atomictransaction のトランザクションネットワークサポートを有効にする**:

 このチェック ボックスをオンまたはオフに切り替えると、このスナップインのすべての GUI コンポーネントが有効または無効になります。

 このチェック ボックスをオンにする前に、受信か送信の通信、またはその両方で [ネットワーク DTC アクセス] が有効であることを確認する必要があります。 この値は、MSDTC スナップインの **[セキュリティ]** タブで確認できます。

#### <a name="network-group-box"></a>Network グループ ボックス

Network グループでは、HTTPS ポートや追加のセキュリティ設定 (SSL 暗号化など) を指定できます。 DTC ネットワーク トランザクションが有効でない場合、このグループは無効 (灰色表示) です。

 **HTTPS ポート**

 これは WS-AT に使用される HTTPS ポートの値です。 有効なポートを表す値は、1 ～ 65535 の範囲内であることが必要です。 HTTP ポートを変更すると、HTTP サービス構成が変更されます。つまり、前に使用していた WS-AT サービス アドレスが解放され、新しいポートに基づいて新しい WS-AT サービス アドレスが登録されます。 さらに、新しく選択したポートは、現在選択している SSL 証明書で暗号化されます。

> [!NOTE]
> このツールを実行する前にファイアウォールが既に有効な場合、ポートは例外の一覧に自動的に登録されます。 このツールを実行する前にファイアウォールが無効な場合、ファイアウォールに関する追加の構成はありません。

 WS-AT の構成後にファイアウォールを有効にする場合は、このツールを再度実行し、このパラメーターを使用してポート番号を指定する必要があります。 WS-AT の構成後にファイアウォールを無効にする場合は、入力を追加しないで WS-AT の動作を続行します。

 **エンドポイント証明書**

 **[選択**] ボタンをクリックすると、ローカルコンピューターで現在利用可能な証明書が一覧表示され、ユーザーは SSL 暗号化に使用できる証明書を選択できます。 証明書には、秘密キーが必要です。 秘密キーがない場合は、エラー メッセージが表示されます。

> [!NOTE]
> 選択したポートに SSL 証明書を設定すると、そのポートに関連付けられたオリジナルの SSL 証明書が上書きされます。

 **承認されたアカウント**

 **[選択**] ボタンをクリックすると、Windows Access Control リストエディターが呼び出されます。このエディターでは、 **[参加]** 許可 グループの **[許可]** または **[拒否]** ボックスをオンにすることで、ws-atomictransaction トランザクションに参加できるユーザーまたはグループを指定できます。

 **承認済み証明書**

 **[選択**] ボタンをクリックすると、LocalMachine で現在利用可能な証明書の一覧が表示されます。 WS-AtomicTransaction への参加が許可される証明書 ID を選択できます。

#### <a name="timeout-group-box"></a>Timeout グループ ボックス

**[タイムアウト]** グループボックスでは、ws-atomictransaction トランザクションの既定のタイムアウトと最大のタイムアウトを指定できます。 送信のタイムアウトの有効値は 1 ～ 3600 の範囲です。 受信のタイムアウトの有効値は 0 ～ 3600 の範囲です。

#### <a name="tracing-and-logging-group-box"></a>グループ ボックスのトレースとログ記録

**[トレースとログ記録]** グループボックスを使用すると、目的のトレースとログ記録レベルを構成できます。

 **[オプション]** ボタンをクリックすると、追加の設定を指定できるページが表示されます。

 [**トレースレベル**の組み合わせ] ボックスを使用すると、<xref:System.Diagnostics.TraceLevel> 列挙型の任意の有効な値から選択できます。 また、チェック ボックスを使用して、アクティビティ トレースやアクティビティ伝達を実行するかどうかを指定したり、個人を特定する情報を収集するかどうかを指定することもできます。

 **[ログセッション]** グループボックスで、ログセッションを指定することもできます。

> [!NOTE]
> 別のトレース コンシューマーが WS-AT トレース プロバイダーを使用している場合は、トレース イベントの新しいログ セッションを作成できません。 このときにログ記録を構成しようとすると、エラー メッセージ "プロバイダーを有効にできませんでした。 エラー コード: 1" が表示されます。

 トレースとログ記録の詳細については、「[管理と診断](./diagnostics/index.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [WS-AtomicTransaction サポートの構成](./feature-details/configuring-ws-atomic-transaction-support.md)
- [WS-AtomicTransaction 構成ユーティリティ (wsatConfig.exe)](ws-atomictransaction-configuration-utility-wsatconfig-exe.md)
- [管理と診断](./diagnostics/index.md)
