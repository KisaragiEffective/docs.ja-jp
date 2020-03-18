---
title: My.Application.Log による情報の書き込み先の確認
ms.date: 07/20/2015
helpviewer_keywords:
- My.Log object, output location
- output, application log location
- My.Application.Log object, output location
- event logs, determining output location
- application event logs, output location
- applications [Visual Basic], output location
ms.assetid: 5b70143a-7741-45f2-ae1d-03324a3a4189
ms.openlocfilehash: f3fd0ed0388276f1400bf77d0abfb488634a45a5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74353606"
---
# <a name="walkthrough-determining-where-myapplicationlog-writes-information-visual-basic"></a>チュートリアル: My.Application.Log による情報の書き込み先の確認 (Visual Basic)

`My.Application.Log` オブジェクトは、複数のログ リスナーに情報を書き込むことができます。 ログ リスナーは、コンピューターの構成ファイルで設定し、アプリケーションの構成ファイルでオーバーライドできます。 このトピックでは、既定の設定について、およびアプリケーションの設定を確認する方法について説明します。

既定の出力場所について詳しくは、「[アプリケーション ログの使用](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)」をご覧ください。

### <a name="to-determine-the-listeners-for-myapplicationlog"></a>My.Application.Log のリスナーを確認するには

1. アセンブリの構成ファイルを見つけます。 アセンブリを開発中の段階では、**ソリューション エクスプローラー**から Visual Studio の app.config にアクセスできます。 開発が終了すると、構成ファイルはアセンブリの名前に ".config" を付け加えたファイル名で、アセンブリと同じディレクトリに配置されています。

    > [!NOTE]
    > アセンブリによっては、構成ファイルがない場合もあります。

    構成ファイルは XML ファイルです。

2. `<listeners>` セクション内にある、 `<source>` 属性が "DefaultSource" の `name` セクションで、 `<sources>` セクションを見つけます。 `<sources>` セクションは、最上位の `<system.diagnostics>` セクション内の `<configuration>` セクションにあります。

    これらのセクションがない場合には、 `My.Application.Log` のログ リスナーは、コンピューターの構成ファイルで設定されている可能性があります。 以下の手順は、コンピューターの構成ファイルの定義を確認する方法の説明です。

    1. コンピューターの machine.config ファイルを見つけます。 通常は、*SystemRoot\Microsoft.NET\Framework\frameworkVersion\CONFIG* ディレクトリにあります。`SystemRoot` はオペレーティング システム ディレクトリであり、`frameworkVersion` は .NET Framework のバージョンです。

        machine.config の設定は、アプリケーションの構成ファイルでオーバーライドできます。

        以下に示すオプションの要素が存在しない場合には、自分で作成できます。

    2. 最上位の `<listeners>` セクション内にある `<source>` セクション内の `name` セクションで、 `<sources>` 属性が "DefaultSource" の `<system.diagnostics>` セクション内の `<configuration>` セクションを見つけます。

        これらのセクションがない場合、 `My.Application.Log` にあるのは既定のログ リスナーのみです。

3. <`add>` セクションで <`listeners>` 要素を見つけます。

     これらの要素は、名前付きのログ リスナーを `My.Application.Log` のソースに追加します。

4. 最上位の `<add>` セクション内にある `<sharedListeners>` セクション内の `<system.diagnostics>` セクションで、ログ リスナーの名前の `<configuration>` 要素を見つけます。

5. 多くの型の共有リスナーでは、リスナーがデータを書き込む先は、リスナーの初期化データで指定されています。

    - <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener?displayProperty=nameWithType> リスナーは、導入部で説明したように、ファイル ログに書き込みます。

    - <xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType> リスナーは、 `initializeData` パラメーターで指定された、コンピューターのイベント ログに情報を書き込みます。 イベント ログを参照するには、 **サーバー エクスプローラー** または **Windows イベント ビューアー**を使用できます。 詳細については、「 [.NET Framework の ETW イベント](../../../../framework/performance/etw-events.md)」を参照してください。

    - <xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType> リスナーおよび <xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType> リスナーは、 `initializeData` パラメーターで指定されたファイルに書き込みます。

    - <xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType> リスナーは、コマンド ライン コンソールに書き込みます。

    - 他の型のログ リスナーが情報を書き込む先については、その型のドキュメントを参照してください。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.DelimitedListTraceListener>
- <xref:System.Diagnostics.XmlWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics>
- [アプリケーション ログの使用](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)
- [方法 : 例外をログに記録する](../../../../visual-basic/developing-apps/programming/log-info/how-to-log-exceptions.md)
- [方法: ログ メッセージを書き込む](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)
- [チュートリアル : My.Application.Log による情報の書き込み先の変更](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)
- [.NET Framework の ETW イベント](../../../../framework/performance/etw-events.md)
- [トラブルシューティング : ログ リスナー](../../../../visual-basic/developing-apps/programming/log-info/troubleshooting-log-listeners.md)
