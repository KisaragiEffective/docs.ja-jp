---
title: PrintPreviewDialog コントロールの概要
ms.date: 01/08/2018
f1_keywords:
- PrintPreviewDialog
helpviewer_keywords:
- PrintPreviewDialog control (using designer), about PrintPreviewDialog
ms.assetid: efd4ee8d-6edd-47ec-88e4-4a4759bd2384
ms.openlocfilehash: 6fb971493336cda1e04c720dd09147e650918c3a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741416"
---
# <a name="printpreviewdialog-control-overview-windows-forms"></a>Printプレビューダイアログコントロールの概要 (Windows フォーム)

Windows フォーム <xref:System.Windows.Forms.PrintPreviewDialog> コントロールは、印刷時に[PrintDocument](printdocument-component-windows-forms.md)がどのように表示されるかを表示するために事前に構成されたダイアログボックスです。 独自のダイアログボックスを構成するのではなく、単純なソリューションとして Windows ベースのアプリケーション内で使用します。 このコントロールには、印刷を開始するボタン、ズーム イン用のボタン、1 ページまたは複数ページを表示するボタン、およびダイアログ ボックスを閉じるためのボタンが含まれています。

## <a name="key-properties-and-methods"></a>キーのプロパティとメソッド

コントロールのキープロパティは <xref:System.Windows.Forms.PrintPreviewDialog.Document%2A>であり、プレビューするドキュメントを設定します。 ドキュメントは <xref:System.Drawing.Printing.PrintDocument> オブジェクトである必要があります。 ダイアログボックスを表示するには、<xref:System.Windows.Forms.Form.ShowDialog%2A> メソッドを呼び出す必要があります。 アンチエイリアシングを使用すると、テキストを滑らかに表示できますが、表示速度が低下することもあります。これを使用するには、<xref:System.Windows.Forms.PrintPreviewDialog.UseAntiAlias%2A> プロパティを `true`に設定します。

特定のプロパティは、<xref:System.Windows.Forms.PrintPreviewDialog> に含まれる <xref:System.Windows.Forms.PrintPreviewControl> を通じて使用できます。 (フォームにこの <xref:System.Windows.Forms.PrintPreviewControl> を追加する必要はありません。フォームにダイアログを追加すると、<xref:System.Windows.Forms.PrintPreviewDialog> 内に自動的に含まれます)。<xref:System.Windows.Forms.PrintPreviewControl> で使用できるプロパティの例としては、コントロール上で水平方向および垂直方向に表示されるページ数を決定する <xref:System.Windows.Forms.PrintPreviewControl.Columns%2A> と <xref:System.Windows.Forms.PrintPreviewControl.Rows%2A> のプロパティがあります。 <xref:System.Windows.Forms.PrintPreviewControl.Columns%2A> プロパティには、Visual Basic の `PrintPreviewDialog1.PrintPreviewControl.Columns`、ビジュアルC#内の `printPreviewDialog1.PrintPreviewControl.Columns`、またはビジュアルC++での `printPreviewDialog1->PrintPreviewControl->Columns` としてアクセスできます。

## <a name="printpreviewdialog-performance"></a>Printプレビューダイアログのパフォーマンス

次の条件下では、<xref:System.Windows.Forms.PrintPreviewDialog> コントロールの初期化が非常に遅くなります。

- ネットワークプリンターが使用されます。
- このプリンターのユーザー設定 (二重設定など) は変更されます。

.NET Framework 4.5.2 で実行されているアプリでは、構成ファイルの \<appSettings > セクションに次のキーを追加して、<xref:System.Windows.Forms.PrintPreviewDialog> コントロールの初期化のパフォーマンスを向上させることができます。

```xml
<appSettings>
   <add key="EnablePrintPreviewOptimization" value="true" />
</appSettings>
```

`EnablePrintPreviewOptimization` キーが他の値に設定されている場合、またはキーが存在しない場合、最適化は適用されません。

.NET Framework 4.6 以降のバージョンで実行されているアプリでは、アプリ構成ファイルの[\<ランタイム >](../../configure-apps/file-schema/runtime/index.md)セクションの[\<AppContextSwitchOverrides >](../../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)要素に次のスイッチを追加できます。

```xml
<runtime >
   <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false -->
   <AppContextSwitchOverrides value = "Switch.System.Drawing.Printing.OptimizePrintPreview=true" />
</runtime >
```

スイッチが存在しない場合、または他の値に設定されている場合、最適化は適用されません。

<xref:System.Drawing.Printing.PrintDocument.QueryPageSettings> イベントを使用してプリンターの設定を変更した場合、最適化構成スイッチが設定されていても、<xref:System.Windows.Forms.PrintPreviewDialog> コントロールのパフォーマンスは向上しません。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.PrintPreviewDialog>
- [PrintPreviewControl コントロールの概要](printpreviewcontrol-control-overview-windows-forms.md)
- [PrintPreviewDialog コントロール](printpreviewdialog-control-windows-forms.md)
- [ダイアログ ボックス コントロールおよびコンポーネント](dialog-box-controls-and-components-windows-forms.md)
