---
title: .NET Framework のログ記録の制御
ms.date: 03/30/2017
helpviewer_keywords:
- CLR ETW events, logging
ms.assetid: ce13088e-3095-4f0e-9f6b-fad30bbd3d41
ms.openlocfilehash: e7d7d6e60b2f582a579f5811225f4027c37c7876
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77504105"
---
# <a name="controlling-net-framework-logging"></a>.NET Framework のログ記録の制御

Windows イベント トレーシング (ETW: Event Tracing for Windows) を使用して共通言語ランタイム (CLR: Common Language Runtime) イベントを記録できます。 トレースの作成および表示には次のツールを使用します。

- Windows オペレーティング システムに含まれる [Logman](/windows-server/administration/windows-commands/logman) および [Tracerpt](/windows-server/administration/windows-commands/tracerpt_1) コマンド ライン ツール。

- [Windows Performance Toolkit](/windows-hardware/test/wpt/xperf-command-line-reference) の [Xperf](/windows-hardware/test/wpt/) ツール。 Xperf の詳細については、[Windows のパフォーマンスに関するブログ](https://docs.microsoft.com/archive/blogs/pigscanfly/)を参照してください。

CLR イベントの情報をキャプチャするには、コンピューターに CLR プロバイダーがインストールされている必要があります。 プロバイダーがインストールされているかどうかを確認するには、コマンド プロンプトで「`logman query providers`」と入力します。 プロバイダーの一覧が表示されます。 その一覧に、次のような CLR プロバイダーのエントリが含まれている必要があります。

```output
Provider                                 GUID
-------------------------------------------------------------------------------
.NET Common Language Runtime    {E13C0D23-CCBC-4E12-931B-D9CC2EEE27E4}.
```

一覧に CLR プロバイダーが含まれていない場合は、Windows Vista 以降のオペレーティング システムで Windows [Wevtutil](/windows-server/administration/windows-commands/wevtutil) コマンド ライン ツールを使用してインストールできます。 管理者としてコマンド プロンプト ウィンドウを開きます。 Prompt ディレクトリを .NET Framework 4 フォルダー (%WINDIR%\Microsoft.NET\Framework [64] \ v4.\<.NET バージョン > \) に変更します。 このフォルダーに、CLR-ETW.man ファイルが含まれています。 コマンド プロンプトで次のコマンドを入力して CLR プロバイダーをインストールします。

`wevtutil im CLR-ETW.man`

## <a name="capturing-clr-etw-events"></a>CLR ETW イベントのキャプチャ

コマンド ライン ツールの [Logman](/windows-server/administration/windows-commands/logman) および [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) を使用して ETW イベントをキャプチャできます。また、[Tracerpt](/windows-server/administration/windows-commands/tracerpt_1) および [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) ツールを使用してトレース イベントをデコードできます。

ログを有効にするには、次の 3 つの項目を指定する必要があります。

- 通信先のプロバイダー。

- キーワード セットを表す 64 ビットの数値。 各キーワードは、プロバイダーが有効にできる一連のイベントを表します。 この数値は、有効にするキーワードの組み合わせを表します。

- 記録レベル (詳細度) を表す小さな数値。 レベル 1 が最も簡易であり、レベル 5 が最も詳細になります。 レベル 0 は既定値であり、プロバイダー固有であることを意味します。

### <a name="to-capture-clr-etw-events-using-logman"></a>Logman を使用して CLR ETW イベントをキャプチャするには

1. コマンド プロンプトに、次のコマンドを入力します。

     `logman start clrevents -p {e13c0d23-ccbc-4e12-931b-d9cc2eee27e4} 0x1CCBD 0x5 -ets -ct perf`

     各値の説明:

    - `-p` パラメーターはプロバイダーの GUID を識別します。

    - `0x1CCBD` は、発生するイベントのカテゴリを指定しています。

    - `0x5` は、ログ レベルを設定しています (この場合は詳細 (5))。

    - `-ets` パラメーターは、コマンドをイベント トレース セッションに送信するように指定します。

    - `-ct perf` パラメーターは、`QueryPerformanceCounter` 関数を使用して各イベントのタイム スタンプを記録するように指定します。

2. イベントの記録を停止するには、次のように入力します。

     `logman stop clrevents -ets`

     これにより、clrevents.etl という名前のバイナリ トレース ファイルが作成されます。

### <a name="to-capture-clr-etw-events-using-xperf"></a>Xperf を使用して CLR ETW イベントをキャプチャするには

1. コマンド プロンプトに、次のコマンドを入力します。

     `xperf -start clr -on e13c0d23-ccbc-4e12-931b-d9cc2eee27e4:0x1CCBD:5 -f clrevents.etl`

     GUID には CLR ETW プロバイダーの GUID を指定します。`0x1CCBD:5` を指定すると、レベル 5 (詳細) 以下のすべての内容がトレースされます。

2. トレースを停止するには、次のように入力します。

     `Xperf -stop clr`

     これにより、clrevents.etl という名前のトレース ファイルが作成されます。

## <a name="viewing-clr-etw-events"></a>CLR ETW イベントの表示

CLR ETW イベントを表示するには、以下のコマンドを使用します。 イベントの詳細については、「[CLR ETW イベント](clr-etw-events.md)」を参照してください。

### <a name="to-view-clr-etw-events-using-tracerpt"></a>Tracerpt を使用して CLR ETW イベントを表示するには

- コマンド プロンプトに、次のコマンドを入力します。

     `tracerpt clrevents.etl`

     これにより、dumpfile.xml と summary.txt という 2 つのファイルが作成されます。 dumpfile.xml ファイルはすべてのイベントの一覧で、summary.txt はイベントの概要です。

### <a name="to-view-clr-etw-events-using-xperf"></a>Xperf を使用して CLR ETW イベントを表示するには

- コマンド プロンプトに、次のコマンドを入力します。

     `xperf clrevents.etl`

     Xperf ETL ファイル ビューアーが開きます。 このビューアーでは、CLR イベントは、 **[一般的なイベント]** ビューに表示されます。 種類別に分類されたイベントのデータ グリッドを表示するには、このビューで時間帯を選択し、マウスを右クリックして **[概要]** を選択します。

### <a name="to-convert-the-etl-file-to-a-comma-separated-value-file"></a>.etl ファイルをコンマ区切り値ファイルに変換するには

- コマンド プロンプトに、次のコマンドを入力します。

     `xperf -i clrevents.etl -f clrevents.csv`

     このコマンドを使用すると、XPerf によって、表示可能なコンマ区切り値 (CSV) ファイルとしてイベントがダンプされます。 イベントが異なればフィールドも異なるので、この CSV ファイルには、データの前に複数のヘッダー行が含まれます。 すべての行の先頭のフィールドはイベントの種類を表します。このフィールドは、残りのフィールドを判別するために使用されるヘッダーを示します。

## <a name="see-also"></a>参照

- [Windows パフォーマンスツールキット](/windows-hardware/test/wpt/)
- [共通言語ランタイムの ETW イベント](etw-events-in-the-common-language-runtime.md)
