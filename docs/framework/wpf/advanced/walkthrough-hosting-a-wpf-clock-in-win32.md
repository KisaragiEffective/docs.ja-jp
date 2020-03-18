---
title: 'チュートリアル: Win32 での WPF クロックのホスト'
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- interoperability [WPF], tutorials
- Win32 code [WPF], WPF interoperation
- interoperability [WPF], Win32
ms.assetid: 555e55a7-0851-4ec8-b1c6-0acba7e9b648
ms.openlocfilehash: 0aecde96d182e12ab72b1a6cba129ab1d8a28391
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77123780"
---
# <a name="walkthrough-host-a-wpf-clock-in-win32"></a>チュートリアル: Win32 での WPF クロックのホスト

Win32 アプリケーション内に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を配置するには、<xref:System.Windows.Interop.HwndSource>を使用します。これにより、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツを含む HWND が提供されます。 まず、<xref:System.Windows.Interop.HwndSource>を作成し、CreateWindow のようなパラメーターを指定します。 次に、その内部に必要な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツについて <xref:System.Windows.Interop.HwndSource> を伝えます。 最後に、<xref:System.Windows.Interop.HwndSource>から HWND を取得します。 このチュートリアルでは、[オペレーティングシステムの**日付と時刻のプロパティ**] ダイアログを再実装する Win32 アプリケーション内で混合 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を作成する方法について説明します。

## <a name="prerequisites"></a>前提条件

「 [WPF と Win32 の相互運用」を](wpf-and-win32-interoperation.md)参照してください。

## <a name="how-to-use-this-tutorial"></a>このチュートリアルの使用方法

このチュートリアルでは、相互運用アプリケーションを生成するための重要な手順について説明します。 このチュートリアルは、サンプルの[Win32 クロック相互運用のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/Win32Clock)によって支えられていますが、そのサンプルは終了製品を反映しています。 このチュートリアルでは、独自の既存の Win32 プロジェクト (通常は既存のプロジェクト) を使用して作業を開始し、ホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] をアプリケーションに追加している場合と同様の手順について説明します。 [Win32 クロック相互運用のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/Win32Clock)を使用して、終了製品を比較することができます。

## <a name="a-walkthrough-of-windows-presentation-framework-inside-win32-hwndsource"></a>Win32 内の Windows Presentation Framework のチュートリアル (System.windows.interop.hwndsource>)

次の図は、このチュートリアルの目的の最終製品を示しています。

![[日付と時刻のプロパティ] ダイアログボックスを表示するスクリーンショット。](./media/walkthrough-hosting-a-wpf-clock-in-win32/date-time-properties-dialog.png)

このダイアログを再作成するにはC++ 、Visual Studio で Win32 プロジェクトを作成し、ダイアログエディターを使用して次のものを作成します。

![[再作成された日付と時刻のプロパティ] ダイアログボックス](./media/walkthrough-hosting-a-wpf-clock-in-win32/recreated-date-time-properties-dialog.png)

(<xref:System.Windows.Interop.HwndSource>を使用するために Visual Studio を使用する必要はなく、Win32 プログラムを記述C++するためにを使用する必要はありませんが、これは非常に一般的な方法であり、これについてはチュートリアルの説明にも役立ちます。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の時計をダイアログに入れるには、5つの特定の手順を実行する必要があります。

1. Visual Studio でプロジェクト設定を変更することにより、Win32 プロジェクトでマネージコード ( **/clr**) を呼び出すことができるようにします。

2. 別の DLL に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.Page> を作成します。

3. この [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.Page> <xref:System.Windows.Interop.HwndSource>内に配置します。

4. <xref:System.Windows.Interop.HwndSource.Handle%2A> プロパティを使用して、その <xref:System.Windows.Controls.Page> の HWND を取得します。

5. Win32 を使用して、より大きな Win32 アプリケーション内で HWND を配置する場所を決定する

## <a name="clr"></a>/clr

最初の手順では、アンマネージ Win32 プロジェクトをマネージコードを呼び出すことができるものに変換します。 使用する必要な Dll にリンクする/clr コンパイラオプションを使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で使用するための Main メソッドを調整します。

C++プロジェクト内でマネージコードを使用できるようにするには、win32clock プロジェクトを右クリックし、 **[プロパティ]** を選択します。 **全般** プロパティページ (既定) で、共通言語ランタイムサポート を `/clr`に変更します。

次に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]に必要な Dll への参照を追加します。これには、プレゼンテーションの Uiautomationprovider.dll、Uiautomationtypes.dll、WindowsBase .dll、dll、およびを追加します。 (以下の手順は、オペレーティングシステムが C: ドライブにインストールされていることを前提としています)。

1. Win32clock project を右クリックし、**参照** をクリックして、そのダイアログ内でを選択します。

2. Win32clock プロジェクト を右クリックし、**参照** を選択します。

3. **新しい参照の追加** をクリックし、参照 タブをクリックして、「C:\Program Files\Reference Assemblies\Microsoft\Framework\v3.0\PresentationCore.dll」と入力し、OK をクリックします。

4. プレゼンテーションフレームワーク .dll に対して繰り返します。 C:\Program Files\Reference Assemblies\Microsoft\Framework\v3.0\PresentationFramework.dll.

5. WindowsBase .dll: C:\Program Files\Reference Assemblies\Microsoft\Framework\v3.0\WindowsBase.dll. に対して繰り返します。

6. Uiautomationtypes.dll の繰り返し: C:\Program Files\Reference Assemblies\Microsoft\Framework\v3.0\UIAutomationTypes.dll.

7. Uiautomationprovider.dll の繰り返し: C:\Program Files\Reference Assemblies\Microsoft\Framework\v3.0\UIAutomationProvider.dll.

8. **新しい参照の追加** をクリックし、.dll を選択して、 **OK**をクリックします。

9. **[OK]** をクリックして、参照を追加するための Win32clock プロパティページを終了します。

 最後に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で使用するために `STAThreadAttribute` を `_tWinMain` メソッドに追加します。

```cpp
[System::STAThreadAttribute]
int APIENTRY _tWinMain(HINSTANCE hInstance,
                     HINSTANCE hPrevInstance,
                     LPTSTR    lpCmdLine,
                     int       nCmdShow)
```

この属性は、コンポーネントオブジェクトモデル (COM) を初期化するときに、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] (および Windows フォーム) に必要なシングルスレッドアパートメントモデル (STA) を使用する必要があることを、共通言語ランタイム (CLR) に通知します。

## <a name="create-a-windows-presentation-framework-page"></a>Windows Presentation Framework ページを作成する

次に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.Page>を定義する DLL を作成します。 多くの場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.Page> をスタンドアロンアプリケーションとして作成し、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の部分を記述してデバッグする方が簡単です。 完了したら、プロジェクトを右クリックし、**プロパティ** をクリックして、アプリケーションに移動し、出力の種類 を Windows クラスライブラリ に変更することにより、そのプロジェクトを DLL に変換できます。

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] dll プロジェクトは、Win32 プロジェクト (2 つのプロジェクトを含む1つのソリューション) と組み合わせることができます。ソリューションを右クリックし、 **[Add\Existing プロジェクト]** を選択します。

Win32 プロジェクトからこの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] dll を使用するには、参照を追加する必要があります。

1. Win32clock プロジェクト を右クリックし、**参照** を選択します。

2. **[新しい参照の追加]** をクリックします。

3. **プロジェクト** タブをクリックします。 WPFClock を選択し、OK をクリックします。

4. **[OK]** をクリックして、参照を追加するための Win32clock プロパティページを終了します。

## <a name="hwndsource"></a>System.windows.interop.hwndsource>

次に、<xref:System.Windows.Interop.HwndSource> を使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.Page> HWND のように表示します。 次のコードブロックをC++ファイルに追加します。

```cpp
namespace ManagedCode
{
    using namespace System;
    using namespace System::Windows;
    using namespace System::Windows::Interop;
    using namespace System::Windows::Media;

    HWND GetHwnd(HWND parent, int x, int y, int width, int height) {
        HwndSource^ source = gcnew HwndSource(
            0, // class style
            WS_VISIBLE | WS_CHILD, // style
            0, // exstyle
            x, y, width, height,
            "hi", // NAME
            IntPtr(parent)        // parent window
            );

        UIElement^ page = gcnew WPFClock::Clock();
        source->RootVisual = page;
        return (HWND) source->Handle.ToPointer();
    }
}
}
```

 これは、何らかの説明を使用できる長いコードです。 最初の部分はさまざまな句であり、すべての呼び出しを完全に修飾する必要がありません。

```cpp
namespace ManagedCode
{
    using namespace System;
    using namespace System::Windows;
    using namespace System::Windows::Interop;
    using namespace System::Windows::Media;
```

 次に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツを作成し、その周囲に <xref:System.Windows.Interop.HwndSource> を配置し、HWND を返す関数を定義します。

```cpp
HWND GetHwnd(HWND parent, int x, int y, int width, int height) {
```

まず、CreateWindow と同様のパラメーターを持つ <xref:System.Windows.Interop.HwndSource>を作成します。

```cpp
HwndSource^ source = gcnew HwndSource(
    0, // class style
    WS_VISIBLE | WS_CHILD, // style
    0, // exstyle
    x, y, width, height,
    "hi", // NAME
    IntPtr(parent) // parent window
);
```

次に、コンストラクターを呼び出して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] content クラスを作成します。

```cpp
UIElement^ page = gcnew WPFClock::Clock();
```

次に、ページを <xref:System.Windows.Interop.HwndSource>に接続します。

```cpp
source->RootVisual = page;
```

 最後の行で、<xref:System.Windows.Interop.HwndSource>の HWND を返します。

```cpp
return (HWND) source->Handle.ToPointer();
```

## <a name="positioning-the-hwnd"></a>Hwnd の配置

これで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クロックを含む HWND が作成されたので、この HWND を Win32 ダイアログ内に配置する必要があります。 HWND をどこに配置するかがわかっている場合は、先ほど定義した `GetHwnd` 関数にそのサイズと場所を渡します。 ただし、リソースファイルを使用してダイアログを定義したので、Hwnd がどこに配置されているかを正確に確認することはできません。 Visual Studio のダイアログエディターを使用して、時計の移動先 ("ここに時計を挿入する") に Win32 スタティックコントロールを配置し、それを使用して [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クロックを配置することができます。

WM_INITDIALOG を処理する場合は、`GetDlgItem` を使用して、プレースホルダー STATIC の HWND を取得します。

```cpp
HWND placeholder = GetDlgItem(hDlg, IDC_CLOCK);
```

次に、そのプレースホルダーのサイズと位置を計算し、その場所に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クロックを配置できるようにします。

RECT 四角形;

```cpp
GetWindowRect(placeholder, &rectangle);
int width = rectangle.right - rectangle.left;
int height = rectangle.bottom - rectangle.top;
POINT point;
point.x = rectangle.left;
point.y = rectangle.top;
result = MapWindowPoints(NULL, hDlg, &point, 1);
```

次に、プレースホルダーを静的に非表示にします。

```cpp
ShowWindow(placeholder, SW_HIDE);
```

次のように、その場所に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] clock HWND を作成します。

```cpp
HWND clock = ManagedCode::GetHwnd(hDlg, point.x, point.y, width, height);
```

このチュートリアルをおもしろいものにし、実際の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クロックを作成するには、この時点で [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 時計コントロールを作成する必要があります。 多くの場合、コードビハインドでいくつかのイベントハンドラーを使用するだけで、マークアップでこれを行うことができます。 このチュートリアルは相互運用に関するものであり、コントロールの設計に関するものではないため、ここでは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] クロックの完全なコードをコードブロックとして提供しています。これは、ビルドのための個別の指示も、各部分が意味するものでもありません。 このコードを自由に試して、コントロールの外観や外観を変更してください。

マークアップは次のとおりです。

[!code-xaml[Win32Clock#AllClockXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/Win32Clock/CS/Clock.xaml#allclockxaml)]

次に、関連する分離コードを示します。

[!code-csharp[Win32Clock#AllClockCS](~/samples/snippets/csharp/VS_Snippets_Wpf/Win32Clock/CS/Clock.xaml.cs#allclockcs)]

最終的な結果は次のようになります。

![[最終結果の日付と時刻のプロパティ] ダイアログボックス](./media/walkthrough-hosting-a-wpf-clock-in-win32/final-result-date-time-properties-dialog.png)

このスクリーンショットを生成したコードと結果を比較するには、「 [Win32 Clock 相互運用のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/Win32Clock)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.HwndSource>
- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
- [Win32 クロック相互運用のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/Win32Clock)
