---
title: キューに置かれたメッセージングのトラブルシューティング
ms.date: 03/30/2017
ms.assetid: a5f2836f-018d-42f5-a571-1e97e64ea5b0
ms.openlocfilehash: 7990d4b9847ee2f35b9fe6269bb211763c4c80b6
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095009"
---
# <a name="troubleshooting-queued-messaging"></a>キューに置かれたメッセージングのトラブルシューティング

ここでは、Windows Communication Foundation (WCF) でキューを使用する際の一般的な質問とトラブルシューティングのヘルプについて説明します。

## <a name="common-questions"></a>一般的な質問

**Q:** WCF Beta 1 を使用し、MSMQ 修正プログラムをインストールしました。 この修正プログラムを削除する必要がありますか。

**A:** はい。 この修正プログラムのサポートは終了しています。 WCF は、修正プログラムの要件を伴わずに MSMQ で動作するようになりました。

**Q:** MSMQ には、<xref:System.ServiceModel.NetMsmqBinding> と <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>の2つのバインドがあります。 それぞれの用途を教えてください。

**A:** 2つの WCF アプリケーション間でキューに置かれた通信のトランスポートとして MSMQ を使用する場合は、<xref:System.ServiceModel.NetMsmqBinding> を使用します。 既存の MSMQ アプリケーションを使用して新しい WCF アプリケーションと通信する場合は、<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> を使用します。

**Q:** <xref:System.ServiceModel.NetMsmqBinding> バインドと `MsmqIntegration` バインドを使用するには、MSMQ をアップグレードする必要がありますか。

**A:** いいえ。 どちらのバインドも、Windows XP と Windows Server 2003 の MSMQ 3.0 で動作します。 Windows Vista で MSMQ 4.0 にアップグレードすると、バインドの特定の機能が使用できるようになります。

**Q:** Msmq 4.0 では、<xref:System.ServiceModel.NetMsmqBinding> バインドと <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> バインドのどの機能を使用できますが、MSMQ 3.0 では使用できませんか。

**A:** Msmq 4.0 では、次の機能は使用できますが、MSMQ 3.0 では使用できません。

- カスタム配信不能キューは、MSMQ 4.0 でのみサポートされます。

- MSMQ 3.0 と 4.0 では、有害メッセージの処理方法が異なります。

- MSMQ 4.0 のみが、リモート トランザクション読み取りをサポートします。

詳細については、「 [Windows Vista、Windows Server 2003、および WINDOWS XP のキュー機能の相違点](../../../../docs/framework/wcf/feature-details/diff-in-queue-in-vista-server-2003-windows-xp.md)」を参照してください。

**Q:** キューに置かれた通信の片側で MSMQ 3.0 を使用し、もう一方の側で MSMQ 4.0 を使用できますか。

**A:** はい。

**Q:** 既存の MSMQ アプリケーションを新しい WCF クライアントまたはサーバーに統合します。 使用している MSMQ インフラストラクチャの両方の側をアップグレードする必要がありますか。

**A:** いいえ。 どちら側も MSMQ 4.0 にアップグレードする必要はありません。

## <a name="troubleshooting"></a>トラブルシューティング

ここでは、一般的なトラブルシューティングの問題に対する解答を示します。 既知の制限である一部の問題は、リリース ノートにも記載されています。

**Q:** プライベートキューを使用しようとしていますが、次の例外が発生しました: `System.InvalidOperationException`: URL が無効です。 キューの URL に '$' 文字を使用することはできません。 net.msmq://machine/private/queueName の構文を使用して、プライベート キューをアドレス指定してください。

**A:** 構成とコードでキュー Uniform Resource Identifier (URI) を確認します。 URI では、"$" 文字を使用できません。 たとえば、OrdersQueue という名前のプライベートキューをアドレス指定するには、URI を `net.msmq://localhost/private/ordersQueue`として指定します。

**Q:** キューに置かれたアプリケーションで `ServiceHost.Open()` を呼び出すと、次の例外がスローされます。 `System.ArgumentException`: ベースアドレスに URI クエリ文字列を含めることはできません。 なぜですか?

**A:** 構成ファイルとコード内のキューの URI を確認します。 MSMQ のキューでは '?' 文字の使用がサポートされていますが、URI はこれを文字列クエリの開始と解釈します。 この問題を回避するには、'?' 文字を含まないキュー名を使用してください。

**Q:** 送信は成功しましたが、受信側でサービス操作が呼び出されていません。 なぜですか?

**A:** 回答を確認するには、次のチェックリストを使用します。

- トランザクション キューの要件と指定済みの保証が適合することを確認します。 次の原則に注意してください。

  - 永続的なメッセージ (データグラムとセッション) は、トランザクションキューにのみ "厳密に1回" 保証 (<xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> = `true`) を使用して送信できます。

  - "1 回限りの" 配信の保証付きのセッションのみを送信できます。

  - セッションでトランザクション キューからメッセージを受け取るには、1 つのトランザクションが必要です。

  - 非トランザクションキューに対してのみ、(<xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> = `false`) 保証なしで、揮発性または永続的なメッセージ (データグラムのみ) を送信または受信できます。

- 配信不能キューを確認します。 このキューにメッセージが置かれている場合は、メッセージが配信されなかった理由を特定してください。

- 送信キューを確認して、接続性またはアドレス指定の問題を特定します。

**Q:** カスタム配信不能キューを指定しましたが、送信側アプリケーションを開始すると、配信不能キューが見つからないか、送信元アプリケーションに配信不能キューへのアクセス許可がないという例外が発生します。 パフォーマンスが低下しているのはなぜですか。

**A:** カスタム配信不能キューの URI には、最初のセグメント (例://localhost/private/myAppdead-letter queue) に "localhost" またはコンピューター名が含まれている必要があります。

**Q:** カスタム配信不能キューを定義する必要があるか、既定の配信不能キューがあるか。

**A:** 保証が "厳密に1回" (<xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> = `true`) であり、カスタム配信不能キューを指定しない場合、既定値はシステム全体のトランザクション配信不能キューになります。

保証が none (<xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> = `false`) の場合、既定では配信不能キュー機能はありません。

**Q:** サービスが Svchost.exe でスローします。 "EndpointListener の要件を ListenerFactory が満たすことができません" というメッセージが表示されます。 なぜですか?

A. サービス コントラクトを確認してください。 すべてのサービス操作に "IsOneWay =`true`" を配置し忘れた可能性があります。 キューは、一方向のサービス操作しかサポートしません。

**Q:** キューにメッセージがありますが、サービス操作が呼び出されていません。 問題は何ですか?

**A:** サービスホストに障害が発生していないかどうかを確認します。 確認するには、トレースを調べるか、`IErrorHandler` を実装します。 既定では、有害メッセージが検出された場合、サービス ホストはエラーになります。

**Q:** キューにメッセージがありますが、Web でホストされているキューに登録されたサービスがアクティブ化されていません。 なぜですか?

**A:** 最も一般的な理由はアクセス許可です。

1. `NetMsmqActivator` プロセスが実行され、そのキューで、`NetMsmqActivator` の ID に読み取りおよびシーク アクセス許可が割り当てられていることを確認してください。

2. `NetMsmqActivator` がリモート コンピューター上のキューを監視している場合は、`NetMsmqActivator` が制限付きトークンの下で実行されていないことを確認してください。 無制限のトークンを使用して `NetMsmqActivator` を実行するには、以下を実行します。

    ```console
    sc sidtype NetMsmqActivator unrestricted
    ```

セキュリティに関連しない Web ホストの問題については、「キューに登録され[たアプリケーションを web でホスト](../../../../docs/framework/wcf/feature-details/web-hosting-a-queued-application.md)する」を参照してください。

**Q:** セッションにアクセスする最も簡単な方法は何ですか。

**A:** セッションの最後のメッセージに対応する操作に対して AutoComplete =`true` を設定し、残りのすべてのサービス操作に対して AutoComplete =`false` を設定します。

**Q:** キューに登録されたセッションメッセージとキューに置かれたデータグラムメッセージの両方を含むキューから読み取るときに、サービスが `ProtocolException` をスローするのはなぜですか。

**A:** キューに登録されたセッションメッセージとキューに置かれたデータグラムメッセージの構成方法には、基本的な違いがあります。 このため、キューを使用するセッション メッセージを読み取ろうとするサービスは、キューを使用するデータグラム メッセージを受信できず、キューを使用するデータグラム メッセージを読み取ろうとするサービスは、セッション メッセージを受信できません。 これら両方の種類のメッセージを同じキューから読み取ろうとすると、次の例外がスローされます。

```console
System.ServiceModel.MsmqPoisonMessageException: The transport channel detected a poison message. This occurred because the message exceeded the maximum number of delivery attempts or because the channel detected a fundamental problem with the message. The inner exception may contain additional information.
---> System.ServiceModel.ProtocolException: An incoming MSMQ message contained invalid or unexpected .NET Message Framing information in its body. The message cannot be received. Ensure that the sender is using a compatible service contract with a matching SessionMode.
```

アプリケーションが同じコンピューターからキューを使用するセッション メッセージとキューを使用するデータグラム メッセージの両方を送信する場合は、任意のカスタム配信不能キューだけでなくシステム配信不能キューで特にこの問題が発生する可能性があります。 メッセージを正常に送信できない場合、メッセージは配信不能キューに移されます。 このような場合は、セッション メッセージとデータグラム メッセージの両方が配信不能キューに置かれる可能性があります。 実行時にキューから読み取るときに両方の種類のメッセージを分離することはできません。そのため、アプリケーションでは、キューを使用するセッション メッセージとキューを使用するデータグラム メッセージの両方を同じコンピューターから送信しないでください。

### <a name="msmq-integration-specific-troubleshooting"></a>MSMQ 統合 : 固有のトラブルシューティング

**Q:** メッセージを送信したとき、またはサービスホストを開いたときに、スキームが間違っていることを示すエラーが表示されます。 なぜですか?

**A:** MSMQ 統合バインドを使用する場合は、msmq-mqseries スキームを使用する必要があります。 たとえば、msmq.formatname:DIRECT=OS:.\private$\OrdersQueue などです。 ただし、カスタム配信不能キューを指定するときは、net.msmq スキームを使用する必要があります。

**Q:** パブリックまたはプライベート形式名を使用して Windows Vista でサービスホストを開くと、エラーが表示されます。 なぜですか?

**A:** Windows Vista の WCF 統合チャネルは、有害なメッセージを処理するためにメインアプリケーションキューのサブキューを開くことができるかどうかを確認します。 サブキュー名は、リスナーに渡された、formatname URI から派生します。 MSMQ のサブキュー名に使用できるのは、直接形式名のみです。 そのためにエラーが発生します。 キュー URI を直接形式名に変更してください。

**Q:** MSMQ アプリケーションからメッセージを受信すると、メッセージはキューに置かれ、受信側の WCF アプリケーションによって読み取られることはありません。 なぜですか?

**A:** メッセージに本文があるかどうかを確認します。 メッセージに本文がない場合、MSMQ 統合チャネルはメッセージを無視します。 例外を通知する `IErrorHandler` を実装し、トレースを確認してください。

### <a name="security-related-troubleshooting"></a>セキュリティ関連のトラブルシューティング

**Q:** ワークグループモードで既定のバインドを使用するサンプルを実行すると、メッセージは送信されたように見えますが、受信側で受信されることはありません。

**A:** 既定では、メッセージは、Active Directory Directory サービスを必要とする MSMQ 内部証明書を使用して署名されます。 ワークグループ モードでは、Active Directory が使用できないため、メッセージに署名できません。 このため、メッセージは配信不能キューにあり、"署名が正しくありません" などのエラーの原因が示されます。

この問題を回避するには、セキュリティを無効にします。 これを行うには <xref:System.ServiceModel.NetMsmqSecurity.Mode%2A> = <xref:System.ServiceModel.NetMsmqSecurityMode.None> を設定して、ワークグループモードで動作するようにします。

また、<xref:System.ServiceModel.MsmqTransportSecurity> プロパティから <xref:System.ServiceModel.NetMsmqSecurity.Transport%2A> を取得し、それを <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> に設定して、クライアント証明書を設定する方法もあります。

さらに、MSMQ と Active Directory 統合をインストールして回避することもできます。

**Q:** Active Directory で既定のバインド (トランスポートセキュリティが有効) を使用してメッセージをキューに送信すると、"内部証明書が見つかりません" というメッセージが表示されます。 これをどのように修正すればよいですか?

**A:** これは、送信側の Active Directory の証明書を更新する必要があることを意味します。 これを行うには、 **[コントロールパネル]** 、 **[管理ツール]** 、 **[コンピューターの管理]** の順に開き、 **[MSMQ]** を右クリックして、 **[プロパティ]** を選択します。 **[ユーザー証明書]** タブを選択し、 **[更新]** ボタンをクリックします。

**Q:** <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> を使用してメッセージを送信し、使用する証明書を指定すると、"証明書が無効です" というメッセージが表示されます。 これをどのように修正すればよいですか?

**A:** 証明書モードでローカルコンピューターの証明書ストアを使用することはできません。 証明書スナップインを使用して、コンピューターの証明書ストアから現在のユーザー ストアに証明書をコピーする必要があります。 証明書スナップインを開くには、以下を実行します。

1. **[スタート]** をクリックし、 **[実行]** を選択して `mmc`を入力し、[ **OK]** をクリックします。

2. **Microsoft 管理コンソール**で、 **[ファイル]** メニューを開き、 **[スナップインの追加と削除]** を選択します。

3. スナップインの **[追加と削除]** ダイアログボックスで、 **[追加]** ボタンをクリックします。

4. **[スタンドアロンスナップ]** インの追加 ダイアログボックスで、証明書 を選択し、 **[追加]** をクリックします。

5. **[証明書]** スナップイン ダイアログボックスで、 **[ユーザーアカウント]** を選択し、 **[完了]** をクリックします。

6. 次に、前の手順を使用して2番目の証明書スナップインを追加しますが、今度は **[コンピューターアカウント]** を選択し、 **[次へ]** をクリックします。

7. **[ローカル コンピューター]** を選択して **[完了]** をクリックします。 これで、コンピューターの証明書ストアから現在のユーザー ストアに証明書をドラッグ アンド ドロップできます。

**Q:** サービスがワークグループモードの別のコンピューターのキューから読み取りを行うと、"アクセスが拒否されました" という例外が発生します。

**A:** ワークグループモードでは、リモートアプリケーションがキューにアクセスできるようにするには、そのキューにアクセスする権限が必要です。 キューのアクセス制御リスト (ACL) に "Anonymous login" を追加し、読み取りアクセス許可を付与します。

**Q:** ネットワークサービスクライアント (またはドメインアカウントを持たないクライアント) がキューに置かれたメッセージを送信する場合、送信は無効な証明書で失敗します。 これをどのように修正すればよいですか?

**A:** バインドの構成を確認します。 既定のバインディングでは、メッセージに署名するために MSMQ トランスポート セキュリティを有効にしています。 これを無効にしてください。

### <a name="remote-transacted-receives"></a>リモート トランザクション受信

**Q:** コンピューター A にキューがあり、コンピューター B のキューからメッセージを読み取る WCF サービス (リモートトランザクション受信シナリオ) がある場合、メッセージはキューから読み取られません。 トレース情報は、"トランザクションをインポートできません" というメッセージで受信が失敗したことを示します。 修正するにはどうすればよいですか。

**A:** これには、次の3つの原因が考えられます。

- ドメイン モードの場合、リモート トランザクション受信には、Microsoft 分散トランザクション コーディネーター (MSDTC) ネットワーク アクセスが必要です。 これは、 **[コンポーネントの追加と削除]** を使用して有効にすることができます。

  ![ネットワーク DTC アクセスの有効化を示すスクリーンショット。](./media/troubleshooting-queued-messaging/enable-distributed-transaction-coordinator-access.jpg)

- トランザクション マネージャーと通信するための認証モードを確認します。 ワークグループモードの場合は、[認証は必要ありません] を選択する必要があります。 ドメインモードの場合は、[相互認証が必要] を選択する必要があります。

  ![XA トランザクションの有効化](../../../../docs/framework/wcf/feature-details/media/4f3695e0-fb0b-4c5b-afac-75f8860d2bb0.jpg "4f3695e0-fb0b-4c5b-afac-75f8860d2bb0")

- **[インターネット接続ファイアウォール]** 設定で、MSDTC が例外の一覧に含まれていることを確認します。

- Windows Vista を使用していることを確認します。 Windows Vista の MSMQ は、リモートトランザクション読み取りをサポートしています。 以前の Windows リリース上の MSMQ は、リモート トランザクション読み取りをサポートしません。

**Q:** キューから読み取るサービスがネットワークサービス (たとえば、Web ホスト) である場合、キューからの読み取り時にアクセス拒否例外が発生するのはなぜですか。

**A:** ネットワークサービスがキューから読み取ることができるようにするには、ネットワークサービスの読み取りアクセスをキュー ACL に追加する必要があります。

**Q:** MSMQ アクティブ化サービスを使用して、リモートコンピューター上のキュー内のメッセージに基づいてアプリケーションをアクティブ化することはできますか。

**A:** はい。 これには、ネットワーク サービスとして動作するように MSMQ アクティベーション サービスを構成し、リモート コンピューター上のキューへのネットワーク サービス アクセスを追加する必要があります。

## <a name="using-custom-msmq-bindings-with-receivecontext-enabled"></a>ReceiveContext を有効にしたカスタム MSMQ バインディングの使用

<xref:System.ServiceModel.Channels.ReceiveContext> が有効になっているカスタム MSMQ バインディングを使用する場合、ネイティブ MSMQ では非同期 <xref:System.ServiceModel.Channels.ReceiveContext> 受信の i/o 完了がサポートされないため、受信メッセージの処理でスレッドプールスレッドが使用されます。 これは、このようなメッセージの処理で <xref:System.ServiceModel.Channels.ReceiveContext> の内部トランザクションが使用され、MSMQ が非同期処理をサポートしていないためです。 この問題を回避するには、エンドポイントに <xref:System.ServiceModel.Description.SynchronousReceiveBehavior> を追加して、同期処理を強制するか、<xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.MaxPendingReceives%2A> を1に設定します。
