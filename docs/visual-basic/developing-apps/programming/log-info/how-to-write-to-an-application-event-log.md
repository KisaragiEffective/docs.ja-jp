---
title: '方法 : アプリケーション イベント ログに書き込む'
ms.date: 07/20/2015
helpviewer_keywords:
- Computer.EventLog element
- WriteEntry method [Visual Basic]
- My.Computer.EventLog element
- event logs, writing to
ms.assetid: cadbc8c1-87af-4746-934e-55b79a4f6e2b
ms.openlocfilehash: 511bb8fb16851872c1a16ae7627ed0fc6594337c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74352046"
---
# <a name="how-to-write-to-an-application-event-log-visual-basic"></a>方法: アプリケーション イベント ログに書き込む (Visual Basic)

`My.Application.Log` オブジェクトおよび `My.Log` オブジェクトを使用すると、アプリケーション内で発生したイベントに関する情報を書き込めます。 この例では、 `My.Application.Log` がアプリケーション イベント ログにトレース情報を書き込むようにイベント ログ リスナーを構成する方法を示します。

セキュリティ ログに書き込むことはできません。 システム ログに書き込むためには、LocalSystem または Administrator アカウントのメンバーであることが必要です。

イベント ログを参照するには、 **サーバー エクスプローラー** または **Windows イベント ビューアー**を使用できます。 詳細については、「 [.NET Framework の ETW イベント](../../../../framework/performance/etw-events.md)」を参照してください。

## <a name="to-add-and-configure-the-event-log-listener"></a>イベント ログ リスナーを追加および構成するには

1. **ソリューション エクスプローラー** で app.config を右クリックし、 **[開く]** を選択します。

    \- または

    app.config ファイルがない場合は、次の操作を行います。

    1. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。

    2. **[新しい項目の追加]** ダイアログ ボックスで、 **[アプリケーション構成ファイル]** を選択します。

    3. **[追加]** をクリックします。

2. アプリケーション構成ファイルで `<listeners>` セクションを見つけます。

    `<listeners>` セクションは、最上位の `<source>` セクションに入れ子になった `<system.diagnostics>` セクションにさらに入れ子になっている、名前属性が "DefaultSource" の `<configuration>` セクションにあります。

3. その `<listeners>` セクションに次の要素を追加します。

    ```xml
    <add name="EventLog"/>
    ```

4. 最上位の `<sharedListeners>` セクション内の `<system.diagnostics>` セクションで、 `<configuration>` セクションを見つけます。

5. その `<sharedListeners>` セクションに次の要素を追加します。

    ```xml
    <add name="EventLog"
        type="System.Diagnostics.EventLogTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
         initializeData="APPLICATION_NAME"/>
    ```

    `APPLICATION_NAME` をアプリケーションの名前に置き換えます。

    > [!NOTE]
    > 通常、アプリケーションがイベント ログに書き込むのはエラーのみです。 ログ出力のフィルター処理の詳細については、「 [Walkthrough: Filtering My.Application.Log Output](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-filtering-my-application-log-output.md)」を参照してください。

## <a name="to-write-event-information-to-the-event-log"></a>イベント情報をイベント ログに書き込むには

`My.Application.Log.WriteEntry` メソッドまたは `My.Application.Log.WriteException` メソッドを使用して、イベント ログに情報を書き込みます。 詳しくは、「[方法: ログ メッセージを書き込む](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)」および「[方法: 例外をログに記録する](../../../../visual-basic/developing-apps/programming/log-info/how-to-log-exceptions.md)」をご覧ください。

アセンブリに対してイベント ログ リスナーを設定すると、そのアセンブリで `My.Application.Log` が書き込んだすべてのメッセージを受け取ります。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A>
- [アプリケーション ログの使用](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)
- [方法 : 例外をログに記録する](../../../../visual-basic/developing-apps/programming/log-info/how-to-log-exceptions.md)
- [チュートリアル : My.Application.Log による情報の書き込み先の確認](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)
