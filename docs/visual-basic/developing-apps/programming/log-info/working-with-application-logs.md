---
title: アプリケーション ログの使用
ms.date: 07/20/2015
helpviewer_keywords:
- logs, application
- application event logs, Visual Basic
- application event logs
ms.assetid: 2581afd1-5791-4bc4-86b2-46244e9fe468
ms.openlocfilehash: 617b940d2cf15779ae3c10e4663b63c9771d44b6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345897"
---
# <a name="working-with-application-logs-in-visual-basic"></a>Visual Basic でのアプリケーション ログの使用

`My.Application.Log` オブジェクトと `My.Log` オブジェクトを使用すると、ログおよびトレース情報をログに簡単に書き込むことができます。

## <a name="how-messages-are-logged"></a>メッセージをログに記録する方法

最初に、ログの <xref:System.Diagnostics.TraceSource.Switch%2A> プロパティの <xref:Microsoft.VisualBasic.Logging.Log.TraceSource%2A> プロパティを使用してメッセージの重大度がチェックされます。 既定では、重大度が "情報" 以上のメッセージのみ、ログの `TraceListener` コレクションで指定されたトレース リスナーに渡されます。 次に各リスナーは、メッセージの重大度をリスナーの <xref:System.Diagnostics.TraceSource.Switch%2A> プロパティと比較します。 メッセージの重大度が十分に高い場合に、リスナーはメッセージを書き込みます。

次の図は、 `WriteEntry` メソッドに書き込まれたメッセージが、ログのトレース リスナーの `WriteLine` メソッドにどのように渡されるかを示しています。

![My ログの呼び出しを示す図。](./media/working-with-application-logs/my-log-call-messages.png)

アプリケーションの構成ファイルを変更すると、ログおよびトレース リスナーの動作を変更できます。 次の図は、ログと構成ファイルの各部分の対応を示します。

![My ログの構成を示す図。](./media/working-with-application-logs/my-log-configuration.png)

## <a name="where-messages-are-logged"></a>メッセージがログに記録される場所

アセンブリに構成ファイルがない場合、 `My.Application.Log` オブジェクトと `My.Log` オブジェクトは <xref:System.Diagnostics.DefaultTraceListener> クラスを介してアプリケーションのデバッグ出力への書き込みを行います。 また、`My.Application.Log` オブジェクトは <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> クラスを介してアセンブリのログ ファイルへの書き込みを行い、`My.Log` オブジェクトは、<xref:System.Web.WebPageTraceListener> クラスを介して ASP.NET Web ページの出力への書き込みを行います。

デバッグ出力は、アプリケーションをデバッグ モードで実行中のときに Visual Studio **[出力]** ウィンドウで参照できます。 **[出力]** ウィンドウを開くには、 **[デバッグ]** メニュー項目をクリックし、 **[ウィンドウ]** をポイントして、 **[出力]** をクリックします。 **[出力]** ウィンドウで **[出力元の表示]** ボックスの **[デバッグ]** を選択します。

既定では、 `My.Application.Log` がログ ファイルを書き込む先は、ユーザーのアプリケーション データ用のパスです。 このパスは、 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.FullLogFileName%2A> オブジェクトの <xref:Microsoft.VisualBasic.Logging.Log.DefaultFileLogWriter%2A> プロパティから取得できます。 このパスの形式は次のとおりです。

`BasePath`\\`CompanyName`\\`ProductName`\\`ProductVersion`

`BasePath` の一般的な値は次のとおりです。

C:\Documents and Settings\\`username`\Application Data

`CompanyName`、 `ProductName`、 `ProductVersion` の値は、アプリケーションのアセンブリ情報から取得されます。 ログ ファイル名の形式は *AssemblyName*.log です。 *AssemblyName* は、アセンブリのファイル名から拡張子を取り除いたものです。 複数のログ ファイルが必要な場合 (たとえば、アプリケーションがログに書き込もうとしているときで、元のログが利用できない場合) には、ログ ファイル名の形式は *AssemblyName*-*iteration*.log です。 `iteration` は正の `Integer`をクリックします。

コンピューターおよびアプリケーションの構成ファイルを追加または変更すると、既定の動作をオーバーライドできます。 詳細については、「[チュートリアル:My.Application.Log による情報の書き込み先の変更](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)」を参照してください。

## <a name="configuring-log-settings"></a>ログ設定を構成する

`Log` オブジェクトの既定の実装では、アプリケーション構成ファイル app.config がなくても動作するようになっています。既定の動作を変更するには、新しい設定を記述した構成ファイルを追加する必要があります。 詳細については、「[チュートリアル:My.Application.Log の出力をフィルター処理する](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-filtering-my-application-log-output.md)」を参照してください。

app.config ファイルでは、ログの構成セクションは、メインの `<system.diagnostics>` ノードの `<configuration>` ノードにあります。 ログ情報は、以下のノードに定義されています。

- `Log` オブジェクトのリスナーは、DefaultSource という名前の `<sources>` ノードで定義されています。

- `Log` オブジェクトの重大度フィルターは、DefaultSwitch という名前の `<switches>` ノードで定義されています。

- ログ リスナーは `<sharedListeners>` ノードで定義されています。

 次のコードに、 `<sources>`、 `<switches>`、 `<sharedListeners>` の各ノードの例を示します。

```xml
<configuration>
  <system.diagnostics>
    <sources>
      <source name="DefaultSource" switchName="DefaultSwitch">
        <listeners>
          <add name="FileLog"/>
        </listeners>
      </source>
    </sources>
    <switches>
      <add name="DefaultSwitch" value="Information" />
    </switches>
    <sharedListeners>
      <add name="FileLog"
        type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
          Microsoft.VisualBasic, Version=8.0.0.0, Culture=neutral,
          PublicKeyToken=b03f5f7f11d50a3a, processorArchitecture=MSIL"
        initializeData="FileLogWriter"
      />
    </sharedListeners>
  </system.diagnostics>
</configuration>
```

## <a name="changing-log-settings-after-deployment"></a>配置後のログ設定の変更

アプリケーションを開発すると、その構成の設定は、上記の例のように、app.config ファイルに格納されます。 アプリケーションを配置した後でも、構成ファイルを編集することで、ログを構成できます。 Windows ベースのアプリケーションでは、このファイルの名前は *applicationName*.exe.config で、実行可能ファイルと同じフォルダーに存在する必要があります。 Web アプリケーションでは、これは、プロジェクトに関連付けられた Web.config ファイルです。

アプリケーションは、クラスのインスタンスを作成するコードを初めて実行するときに、そのオブジェクトについての情報を構成ファイルでチェックします。 `Log` オブジェクトの場合は、 `Log` オブジェクトに初めてアクセスするときにそれを行います。 各オブジェクトについて、システムが構成ファイルをチェックするのは、アプリケーションがそのオブジェクトを最初に作成するときの 1 回だけです。 したがって、変更を有効にするためには、アプリケーションの再起動が必要な場合があります。

配置されたアプリケーションでは、アプリケーションの起動前にスイッチ オブジェクトを再設定することで、トレース コードを有効にできます。 通常この作業では、スイッチ オブジェクトのオンとオフを切り替えるか、またはトレース レベルを変更し、その後アプリケーションを再起動します。

## <a name="security-considerations"></a>セキュリティの考慮事項

ログにデータを書き込むときには、次の点を考慮する必要があります。

- **ユーザー情報を漏らさないようにします。** アプリケーションでは、認められた情報のみをログに書き込むようにします。 たとえば、アプリケーション ログにユーザー名を書き込んでも問題ないとしても、ユーザーのパスワードを書き込むのは問題です。

- **ログは安全な場所に配置します。** 機密性の高い情報を含む可能性のあるログは、安全な場所に格納する必要があります。

- **不正な情報は避けます。** 一般にアプリケーションでは、ユーザーが入力したデータは、使用する前にすべて検証する必要があります。 これは、アプリケーション ログにデータを書き込むときも同じです。

- **サービス拒否を避けます。** アプリケーションがログに書き込む情報が多すぎると、ログが満杯になったり、重要な情報を見つけにくくなったりする可能性があります。

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- [アプリケーションからの情報のログ記録](../../../../visual-basic/developing-apps/programming/log-info/index.md)
