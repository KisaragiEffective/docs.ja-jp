---
title: My.Application.Log の出力をフィルター処理する
ms.date: 07/20/2015
helpviewer_keywords:
- My.Log object, filtering output
- My.Application.Log object, filtering output
- application event logs, output filtering
ms.assetid: 2c0a457a-38a4-49e1-934d-a51320b7b4ca
ms.openlocfilehash: f18556bbe1ca2d77925482319246d403892d31ef
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74353587"
---
# <a name="walkthrough-filtering-myapplicationlog-output-visual-basic"></a>チュートリアル: My.Application.Log の出力のフィルター処理 (Visual Basic)

このチュートリアルでは、`My.Application.Log` オブジェクトの既定のログ フィルター処理を変更して、`Log` オブジェクトからリスナーに渡される情報や、リスナーによって記述される情報を制御する方法について説明します。 構成情報はアプリケーションの構成ファイルに保存されるため、ロギングの動作はアプリケーションをビルドした後でも変更できます。

## <a name="getting-started"></a>作業の開始

`My.Application.Log` が書き込む各メッセージには、重大度レベルが関連付けられます。この情報は、ログ出力を制御するためにフィルター処理メカニズムによって使用されます。 このサンプル アプリケーションでは、`My.Application.Log` メソッドを使用して、重大度レベルの異なる複数のログ メッセージを書き込みます。

#### <a name="to-build-the-sample-application"></a>サンプル アプリケーションをビルドするには

1. 新しい Visual Basic Windows アプリケーション プロジェクトを開きます。

2. Form1 に Button1 という名前のボタンを追加します。

3. Button1 の <xref:System.Windows.Forms.Control.Click> イベント ハンドラーで、次のコードを追加します。

     [!code-vb[VbVbcnMyApplicationLogFiltering#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyApplicationLogFiltering/VB/Form1.vb#1)]

4. デバッガーでアプリケーションを実行します。

5. **[Button1]** を押します。

     アプリケーションは、アプリケーションのデバッグ出力とログ ファイルに次の情報を書き込みます。

     `DefaultSource Information: 0 : In Button1_Click`

     `DefaultSource Error: 2 : Error in the application.`

6. アプリケーションを終了します。

     アプリケーションのデバッグ出力ウィンドウを表示する方法について詳しくは、「[出力ウィンドウ](/visualstudio/ide/reference/output-window)」をご覧ください。 アプリケーションのログ ファイルの場所について詳しくは、「[チュートリアル: My.Application.Log による情報の書き込み先の確認](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)」をご覧ください。

    > [!NOTE]
    > 既定では、アプリケーションはアプリケーションの終了時にログ ファイルの出力をフラッシュします。

     上の例では、<xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A> メソッドの 2 回目の呼び出しと <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A> メソッドの呼び出しでログが出力されます。`WriteEntry` メソッドの最初と最後の呼び出しでは出力されません。 これは、`WriteEntry` と `WriteException` の重大度レベルが "Information" と "Error" であるためです。これらはどちらも、`My.Application.Log` オブジェクトの既定のログ フィルター処理で許可されます。 これに対し、重大度レベルが "Start" および "Stop" のイベントについては、ログ出力が生成されません。

## <a name="filtering-for-all-myapplicationlog-listeners"></a>すべての My.Application.Log リスナーのフィルター処理

`My.Application.Log` オブジェクトは、<xref:System.Diagnostics.SourceSwitch> という名前の `DefaultSwitch` を使用し、`WriteEntry` および `WriteException` メソッドからログ リスナーに渡すメッセージを制御します。 アプリケーションの構成ファイル内にある `DefaultSwitch` は、その値を <xref:System.Diagnostics.SourceLevels> 列挙値のいずれかに設定することで構成できます。 既定では、この値は "Information" です。

次の表は、Log がリスナーにメッセージを書き込むために必要な重大度レベルを、`DefaultSwitch` の設定ごとに示したものです。

|DefaultSwitch の値|出力に必要なメッセージの重大度|
|---|---|
|`Critical`|`Critical`|
|`Error`|`Critical` または `Error`|
|`Warning`|`Critical`、`Error`、または `Warning`|
|`Information`|`Critical`、`Error`、`Warning`、または `Information`|
|`Verbose`|`Critical`、`Error`、`Warning`、`Information`、または `Verbose`|
|`ActivityTracing`|`Start`、`Stop`、`Suspend`、`Resume`、または `Transfer`|
|`All`|すべてのメッセージが許可されます。|
|`Off`|すべてのメッセージがブロックされます。|

> [!NOTE]
> `WriteEntry` メソッドと `WriteException` メソッドにはそれぞれ、重大度レベルを指定しないオーバー ロードがあります。 `WriteEntry` オーバー ロードの暗黙的な重大度レベルは "Information" で、`WriteException` オーバー ロードの暗黙的な重大度レベルは "Error" です。

次の表は、前の例に示したログ出力について説明したものです。 既定の `DefaultSwitch` 設定 (Information) では、`WriteEntry` メソッドに対する 2 番目の呼び出しと、`WriteException` メソッドに対する呼び出しでのみ、ログ出力が生成されます。

#### <a name="to-log-only-activity-tracing-events"></a>アクティビティ トレース イベントだけを記録するには

1. **ソリューション エクスプローラー**で app.config を右クリックし、 **[開く]** を選択します。

     または

     app.config ファイルがない場合は、次の操作を行います。

    1. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。

    2. **[新しい項目の追加]** ダイアログ ボックスで、 **[アプリケーション構成ファイル]** を選択します。

    3. **[追加]** をクリックします。

2. 最上位の `<switches>` セクション内の `<system.diagnostics>` セクションで、`<configuration>` セクションを見つけます。

3. スイッチのコレクションに `DefaultSwitch` を追加する要素を見つけます。 これは次のような要素です。

     `<add name="DefaultSwitch" value="Information" />`

4. `value` 属性の値を "ActivityTracing" に変更します。

5. app.config ファイルの内容は次の XML のようになります。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <system.diagnostics>
        <sources>
          <!-- This section configures My.Application.Log -->
          <source name="DefaultSource" switchName="DefaultSwitch">
            <listeners>
              <add name="FileLog"/>
            </listeners>
          </source>
        </sources>
        <switches>
          <add name="DefaultSwitch" value="ActivityTracing" />
        </switches>
        <sharedListeners>
          <add name="FileLog"
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
                     Microsoft.VisualBasic, Version=8.0.0.0,
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,
                     processorArchitecture=MSIL"
               initializeData="FileLogWriter"/>
        </sharedListeners>
      </system.diagnostics>
    </configuration>
    ```

6. デバッガーでアプリケーションを実行します。

7. **[Button1]** を押します。

     アプリケーションは、アプリケーションのデバッグ出力とログ ファイルに次の情報を書き込みます。

     `DefaultSource Start: 4 : Entering Button1_Click`

     `DefaultSource Stop: 5 : Leaving Button1_Click`

8. アプリケーションを終了します。

9. `value` 属性の値を "Information" に戻します。

    > [!NOTE]
    > `DefaultSwitch` スイッチの設定では、`My.Application.Log` のみが制御されます。 .NET Framework の <xref:System.Diagnostics.Trace?displayProperty=nameWithType> クラスと <xref:System.Diagnostics.Debug?displayProperty=nameWithType> クラスの動作が変えられることはありません。

## <a name="individual-filtering-for-myapplicationlog-listeners"></a>My.Application.Log リスナーの個別のフィルター処理

前の例では、すべての `My.Application.Log` 出力のフィルター処理を変更する方法について説明しました。 この例では、個別のログ リスナーをフィルター処理する方法について説明します。 既定では、アプリケーションには、アプリケーションのデバッグ出力とログ ファイルに情報を書き込む 2 つのリスナーがあります。

構成ファイルは、`My.Application.Log` のスイッチのように、各リスナーにフィルターを適用することで、ログ リスナーの動作を制御します。 ログ リスナーは、メッセージの重大度がログの `DefaultSwitch` とログ リスナーのフィルターの両方によって許可された場合にのみ、メッセージを出力します。

この例では、新しいデバッグ リスナーのフィルター処理を構成し、それを `Log` オブジェクトに追加する方法について説明します。 デバッグ メッセージが新しいデバッグ リスナーから送られるようにするには、既定のデバッグ リスナーを `Log` オブジェクトから削除する必要があります。

#### <a name="to-log-only-activity-tracing-events"></a>アクティビティ トレース イベントだけを記録するには

1. **ソリューション エクスプローラー**で app.config を右クリックし、 **[開く]** を選択します。

     \- または -

     app.config ファイルがない場合は、次の操作を行います。

    1. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。

    2. **[新しい項目の追加]** ダイアログ ボックスで、 **[アプリケーション構成ファイル]** を選択します。

    3. **[追加]** をクリックします。

2. **ソリューション エクスプローラー**で app.config を右クリックします。 **[開く]** をクリックします。

3. `<listeners>` セクション内にある、`<source>` 属性が "DefaultSource" の `name` セクションで、`<sources>` セクションを見つけます。 `<sources>` セクションは、最上位の `<system.diagnostics>` セクション内の `<configuration>` セクションにあります。

4. `<listeners>` セクションに次の要素を追加します。

    ```xml
    <!-- Remove the default debug listener. -->
    <remove name="Default"/>
    <!-- Add a filterable debug listener. -->
    <add name="NewDefault"/>
    ```

5. 最上位の `<sharedListeners>` セクション内の `<system.diagnostics>` セクションで、 `<configuration>` セクションを見つけます。

6. その `<sharedListeners>` セクションに次の要素を追加します。

    ```xml
    <add name="NewDefault"
         type="System.Diagnostics.DefaultTraceListener,
               System, Version=2.0.0.0, Culture=neutral,
               PublicKeyToken=b77a5c561934e089,
               processorArchitecture=MSIL">
        <filter type="System.Diagnostics.EventTypeFilter"
                initializeData="Error" />
    </add>
    ```

     <xref:System.Diagnostics.EventTypeFilter> フィルターは <xref:System.Diagnostics.SourceLevels> 列挙値の 1 つをその `initializeData` 属性として取ります。

7. app.config ファイルの内容は次の XML のようになります。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <system.diagnostics>
        <sources>
          <!-- This section configures My.Application.Log -->
          <source name="DefaultSource" switchName="DefaultSwitch">
            <listeners>
              <add name="FileLog"/>
              <!-- Remove the default debug listener. -->
              <remove name="Default"/>
              <!-- Add a filterable debug listener. -->
              <add name="NewDefault"/>
            </listeners>
          </source>
        </sources>
        <switches>
          <add name="DefaultSwitch" value="Information" />
        </switches>
        <sharedListeners>
          <add name="FileLog"
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
                     Microsoft.VisualBasic, Version=8.0.0.0,
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,
                     processorArchitecture=MSIL"
               initializeData="FileLogWriter"/>
          <add name="NewDefault"
               type="System.Diagnostics.DefaultTraceListener,
                     System, Version=2.0.0.0, Culture=neutral,
                     PublicKeyToken=b77a5c561934e089,
                     processorArchitecture=MSIL">
            <filter type="System.Diagnostics.EventTypeFilter"
                    initializeData="Error" />
          </add>
        </sharedListeners>
      </system.diagnostics>
    </configuration>
    ```

8. デバッガーでアプリケーションを実行します。

9. **[Button1]** を押します。

     アプリケーションは、アプリケーションのログ ファイルに次の情報を書き込みます。

     `Default Information: 0 : In Button1_Click`

     `Default Error: 2 : Error in the application.`

     フィルター処理の基準がより厳しくなったので、アプリケーションのデバッグ出力に書き込まれる情報は少なくなります。

     `Default Error   2   Error`

10. アプリケーションを終了します。

配置後にログの設定を変更する方法については、「[アプリケーション ログの使用](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)」をご覧ください。

## <a name="see-also"></a>参照

- [チュートリアル : My.Application.Log による情報の書き込み先の確認](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)
- [チュートリアル : My.Application.Log による情報の書き込み先の変更](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)
- [チュートリアル : カスタム ログ リスナーの作成](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-creating-custom-log-listeners.md)
- [方法: ログ メッセージを書き込む](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)
- [トレース スイッチ](../../../../framework/debug-trace-profile/trace-switches.md)
- [アプリケーションからの情報のログ記録](../../../../visual-basic/developing-apps/programming/log-info/index.md)
