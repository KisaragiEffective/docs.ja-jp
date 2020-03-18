---
title: 高 DPI のサポート
ms.date: 05/16/2017
helpviewer_keywords:
- High DPI in Windows Forms
- Dynamic rescaling in Windows Forms
- Windows Forms layout
- Windows Forms dynamic resizing
ms.assetid: 075ea4c3-900c-4f8a-9dd2-13ea6804346b
ms.openlocfilehash: a5c3125475c2de2cf83a3d97e356b26c0acdde99
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741889"
---
# <a name="high-dpi-support-in-windows-forms"></a>Windows フォームでの高 DPI サポート

.NET Framework 4.7 以降、Windows フォームには、一般的な高 DPI や動的な DPI の状況に応じた機能強化が含まれています。 チェックの内容は次のとおりです

- <xref:System.Windows.Forms.MonthCalendar> コントロールや <xref:System.Windows.Forms.CheckedListBox> コントロールなど、多数の Windows フォームコントロールのスケーリングとレイアウトの機能強化。

- 単一パスのスケーリング。  .NET Framework 4.6 以前のバージョンでは、スケーリングは複数パスにより実行され、いくつかのコントロールが必要以上にスケーリングされていました。

- Windows フォーム アプリケーション起動後に、ユーザーにより DPI やスケールファクターが変更される、動的 DPI の状況に対応。

.NET Framework 4.7 以降のバージョンでは、高 DPI 対応の強化はオプトインの機能です。 この機能を活用するには、アプリケーションの設定が必要です。

## <a name="configuring-your-windows-forms-app-for-high-dpi-support"></a>高 DPI サポート用の Windows フォームアプリの構成

高 DPI 認識をサポートする新しい Windows フォーム機能は、.NET Framework 4.7 を対象とし、Windows 10 の作成者の更新プログラム以降で Windows オペレーティングシステムで実行されているアプリケーションでのみ使用できます。

さらに、Windows フォームアプリケーションで高 DPI サポートを構成するには、次の操作を行う必要があります。

- Windows 10 との互換性を宣言します。

  これを行うには、マニフェストファイルに次のを追加します。

  ```xml
  <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
    <application>
      <!-- Windows 10 compatibility -->
      <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    </application>
  </compatibility>
  ```

- *App.config*ファイルでモニターごとの DPI 認識を有効にします。

  Windows フォームでは、.NET Framework 4.7 以降に追加された新機能とカスタマイズをサポートする新しい[`<System.Windows.Forms.ApplicationConfigurationSection>`](../configure-apps/file-schema/winforms/index.md)要素が導入されています。 高 DPI をサポートする新機能を利用するには、アプリケーション構成ファイルに次の内容を追加します。

  ```xml
  <System.Windows.Forms.ApplicationConfigurationSection>
    <add key="DpiAwareness" value="PerMonitorV2" />
  </System.Windows.Forms.ApplicationConfigurationSection>
  ```

  > [!IMPORTANT]
  > 以前のバージョンの .NET Framework では、マニフェストを使用して高 DPI サポートを追加していました。 App.config ファイルで定義されている設定を上書きするため、この方法は推奨されなくなりました。

- 静的 <xref:System.Windows.Forms.Application.EnableVisualStyles%2A> メソッドを呼び出します。

  これは、アプリケーションのエントリポイントの最初のメソッド呼び出しである必要があります。 次に例を示します。

  ```csharp
  static void Main()
  {
      Application.EnableVisualStyles();
      Application.SetCompatibleTextRenderingDefault(false);
      Application.Run(new Form2());
  }
  ```

## <a name="opting-out-of-individual-high-dpi-features"></a>個々の高 DPI 機能のオプトアウト

`DpiAwareness` 値を `PerMonitorV2` に設定すると、.NET Framework 4.7 以降のバージョン .NET Framework でサポートされているすべての高 DPI 認識機能が有効になります。 通常、これはほとんどの Windows フォームアプリケーションに適しています。 ただし、1つまたは複数の個別の機能を無効にすることもできます。 これを行う最も重要な理由は、既存のアプリケーションコードで既にその機能が処理されていることです。  たとえば、アプリケーションで自動スケーリングを処理する場合は、次のように自動サイズ変更機能を無効にすることをお勧めします。

```xml
<System.Windows.Forms.ApplicationConfigurationSection>
  <add key="DpiAwareness" value="PerMonitorV2" />
  <add key="EnableWindowsFormsHighDpiAutoResizing" value="false" />
</System.Windows.Forms.ApplicationConfigurationSection>
```

個々のキーとその値の一覧については、「 [Windows フォーム Add Configuration 要素](../configure-apps/file-schema/winforms/windows-forms-add-configuration-element.md)」を参照してください。

## <a name="new-dpi-change-events"></a>新しい DPI 変更イベント

.NET Framework 4.7 以降では、次の3つの新しいイベントを使用して、動的な DPI 変更をプログラムで処理できます。

- <xref:System.Windows.Forms.Control.DpiChangedAfterParent>。親コントロールまたはフォームの DPI 変更イベントが発生した後に、コントロールの DPI 設定がプログラムによって変更されたときに発生します。
- <xref:System.Windows.Forms.Control.DpiChangedBeforeParent>。親コントロールまたはフォームの DPI 変更イベントが発生する前に、コントロールの DPI 設定がプログラムによって変更されたときに発生します。
- <xref:System.Windows.Forms.Form.DpiChanged>。フォームが現在表示されているディスプレイデバイスで DPI 設定が変更されたときに発生します。

## <a name="new-helper-methods-and-properties"></a>新しいヘルパーメソッドとプロパティ

また、.NET Framework 4.7 では、DPI スケーリングに関する情報を提供し、DPI スケーリングを実行できる新しいヘルパーメソッドとプロパティが多数追加されています。 チェックの内容は次のとおりです

- <xref:System.Windows.Forms.Control.LogicalToDeviceUnits%2A>。値を論理ピクセルからデバイスピクセルに変換します。

- <xref:System.Windows.Forms.Control.ScaleBitmapLogicalToDevice%2A>。デバイスの論理 DPI にビットマップイメージをスケーリングします。

- <xref:System.Windows.Forms.Control.DeviceDpi%2A>。現在のデバイスの DPI を返します。

## <a name="versioning-considerations"></a>バージョン管理に関する考慮事項

.NET Framework 4.7 と Windows 10 の作成者の更新プログラムで実行するだけでなく、アプリケーションも高 DPI の機能強化と互換性のない環境で実行される場合があります。 この場合、アプリケーションのフォールバックを開発する必要があります。 これを行うと、スケーリングを処理する[カスタム描画](./controls/user-drawn-controls.md)を実行できます。

これを行うには、アプリが実行されているオペレーティングシステムを特定する必要もあります。 そのためには、次のようなコードを使用します。

```csharp
// Create a reference to the OS version of Windows 10 Creators Update.
Version OsMinVersion = new Version(10, 0, 15063, 0);

// Access the platform/version of the current OS.
Console.WriteLine(Environment.OSVersion.Platform.ToString());
Console.WriteLine(Environment.OSVersion.VersionString);

// Compare the current version to the minimum required version.
Console.WriteLine(Environment.OSVersion.Version.CompareTo(OsMinVersion));
```

アプリケーションマニフェストでサポートされているオペレーティングシステムとして表示されていない場合、アプリケーションが Windows 10 を正常に検出できないことに注意してください。

また、アプリケーションがビルドされた .NET Framework のバージョンを確認することもできます。

```csharp
Console.WriteLine(AppDomain.CurrentDomain.SetupInformation.TargetFrameworkName);
```

## <a name="see-also"></a>参照

- [構成要素の追加 Windows フォーム](../configure-apps/file-schema/winforms/windows-forms-add-configuration-element.md)
- [Windows フォームのサイズとスケールを調整する](adjusting-the-size-and-scale-of-windows-forms.md)
