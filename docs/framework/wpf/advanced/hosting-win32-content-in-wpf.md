---
title: Win32 コンテンツのホスト
ms.date: 03/30/2017
helpviewer_keywords:
- interoperability [WPF], tutorials
- Win32 code [WPF], WPF interoperation
- interoperability [WPF], Win32
ms.assetid: 3cc8644a-34f3-4082-9ddc-77623e4df2d8
ms.openlocfilehash: b3113d9f3146e1590363dc9db6f751a429dda74b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747014"
---
# <a name="hosting-win32-content-in-wpf"></a>WPF での Win32 コンテンツのホスト

## <a name="prerequisites"></a>前提条件

「 [WPF と Win32 の相互運用」を](wpf-and-win32-interoperation.md)参照してください。

## <a name="a-walkthrough-of-win32-inside-windows-presentation-framework-hwndhost"></a>Windows Presentation Framework 内部の Win32 のチュートリアル (HwndHost)

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーション内で Win32 コンテンツを再利用するには、<xref:System.Windows.Interop.HwndHost>を使用します。これは、Hwnd を [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツのように表示するコントロールです。 <xref:System.Windows.Interop.HwndSource>と同様に、<xref:System.Windows.Interop.HwndHost> は簡単に使用できます。 <xref:System.Windows.Interop.HwndHost> から派生し、`BuildWindowCore` および `DestroyWindowCore` メソッドを実装し、<xref:System.Windows.Interop.HwndHost> 派生クラスをインスタンス化して [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーション内に配置します。

Win32 ロジックが既にコントロールとしてパッケージ化されている場合、`BuildWindowCore` の実装は、`CreateWindow`の呼び出しよりもわずかです。 たとえば、でC++Win32 LISTBOX コントロールを作成するには、次のようにします。

```cpp
virtual HandleRef BuildWindowCore(HandleRef hwndParent) override {
    HWND handle = CreateWindowEx(0, L"LISTBOX",
    L"this is a Win32 listbox",
    WS_CHILD | WS_VISIBLE | LBS_NOTIFY
    | WS_VSCROLL | WS_BORDER,
    0, 0, // x, y
    30, 70, // height, width
    (HWND) hwndParent.Handle.ToPointer(), // parent hwnd
    0, // hmenu
    0, // hinstance
    0); // lparam

    return HandleRef(this, IntPtr(handle));
}

virtual void DestroyWindowCore(HandleRef hwnd) override {
    // HwndHost will dispose the hwnd for us
}
```

しかし、Win32 コードがそれほど自己完結していないとしたら、 その場合は、Win32 ダイアログボックスを作成し、その内容を大きな [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションに埋め込むことができます。 このサンプルでは、Visual Studio とC++でこれを示していますが、これは別の言語またはコマンドラインでも実行できます。

まず、 C++ DLL プロジェクトにコンパイルされた単純なダイアログを使用します。

次に、大規模な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションにダイアログを導入します。

- DLL をマネージドとしてコンパイルします (`/clr`)

- ダイアログをコントロールに変換する

- `BuildWindowCore` メソッドと `DestroyWindowCore` メソッドを使用して <xref:System.Windows.Interop.HwndHost> の派生クラスを定義する

- `TranslateAccelerator` メソッドをオーバーライドしてダイアログキーを処理します

- `TabInto` メソッドをオーバーライドしてタブ移動をサポートする

- ニーモニックをサポートするための `OnMnemonic` メソッドのオーバーライド

- <xref:System.Windows.Interop.HwndHost> サブクラスをインスタンス化し、右側の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素の下に配置します

### <a name="turn-the-dialog-into-a-control"></a>ダイアログをコントロールに変換する

WS_CHILD と DS_CONTROL スタイルを使用して、ダイアログボックスを子 HWND にすることができます。 ダイアログが定義されているリソースファイル (.rc) に移動し、ダイアログの定義の先頭を見つけます。

```text
IDD_DIALOG1 DIALOGEX 0, 0, 303, 121
STYLE DS_SETFONT | DS_MODALFRAME | DS_FIXEDSYS | WS_POPUP | WS_CAPTION | WS_SYSMENU
```

2番目の行を次のように変更します。

```text
STYLE DS_SETFONT | WS_CHILD | WS_BORDER | DS_CONTROL
```

この操作では、自己完結型のコントロールに完全にパッケージ化されません。Win32 で特定のメッセージを処理できるように、`IsDialogMessage()` を呼び出す必要がありますが、コントロールを変更すると、そのコントロールを別の HWND 内に簡単に配置できます。

## <a name="subclass-hwndhost"></a>サブクラス HwndHost

次の名前空間をインポートします。

```cpp
namespace ManagedCpp
{
    using namespace System;
    using namespace System::Windows;
    using namespace System::Windows::Interop;
    using namespace System::Windows::Input;
    using namespace System::Windows::Media;
    using namespace System::Runtime::InteropServices;
```

次に、<xref:System.Windows.Interop.HwndHost> の派生クラスを作成し、`BuildWindowCore` メソッドと `DestroyWindowCore` メソッドをオーバーライドします。

```cpp
public ref class MyHwndHost : public HwndHost, IKeyboardInputSink {
    private:
        HWND dialog;

    protected:
        virtual HandleRef BuildWindowCore(HandleRef hwndParent) override {
            InitializeGlobals();
            dialog = CreateDialog(hInstance,
                MAKEINTRESOURCE(IDD_DIALOG1),
                (HWND) hwndParent.Handle.ToPointer(),
                (DLGPROC) About);
            return HandleRef(this, IntPtr(dialog));
        }

        virtual void DestroyWindowCore(HandleRef hwnd) override {
            // hwnd will be disposed for us
        }
```

ここでは、`CreateDialog` を使用して、実際にはコントロールであるダイアログボックスを作成します。 これは DLL 内で呼び出された最初のメソッドの1つであるため、後で定義する関数を呼び出すことによって、標準の Win32 初期化も実行する必要があります (`InitializeGlobals()`と呼ばれます)。

```cpp
bool initialized = false;
    void InitializeGlobals() {
        if (initialized) return;
        initialized = true;

        // TODO: Place code here.
        MSG msg;
        HACCEL hAccelTable;

        // Initialize global strings
        LoadString(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
        LoadString(hInstance, IDC_TYPICALWIN32DIALOG, szWindowClass, MAX_LOADSTRING);
        MyRegisterClass(hInstance);
```

### <a name="override-translateaccelerator-method-to-handle-dialog-keys"></a>TranslateAccelerator メソッドをオーバーライドしてダイアログキーを処理します

このサンプルを今実行した場合、ダイアログコントロールが表示されますが、ダイアログボックスを機能させるためのダイアログボックスを表示するすべてのキーボード処理が無視されます。 `TranslateAccelerator` の実装 (<xref:System.Windows.Interop.HwndHost> 実装するインターフェイス `IKeyboardInputSink`から) をオーバーライドする必要があります。 このメソッドは、アプリケーションが WM_KEYDOWN と WM_SYSKEYDOWN を受信したときに呼び出されます。

```cpp
#undef TranslateAccelerator
        virtual bool TranslateAccelerator(System::Windows::Interop::MSG% msg,
            ModifierKeys modifiers) override
        {
            ::MSG m = ConvertMessage(msg);

            // Win32's IsDialogMessage() will handle most of our tabbing, but doesn't know
            // what to do when it reaches the last tab stop
            if (m.message == WM_KEYDOWN && m.wParam == VK_TAB) {
                HWND firstTabStop = GetDlgItem(dialog, IDC_EDIT1);
                HWND lastTabStop = GetDlgItem(dialog, IDCANCEL);
                TraversalRequest^ request = nullptr;

                if (GetKeyState(VK_SHIFT) && GetFocus() == firstTabStop) {
                    // this code should work, but there’s a bug with interop shift-tab in current builds
                    request = gcnew TraversalRequest(FocusNavigationDirection::Last);
                }
                else if (!GetKeyState(VK_SHIFT) && GetFocus() == lastTabStop) {
                    request = gcnew TraversalRequest(FocusNavigationDirection::Next);
                }

                if (request != nullptr)
                    return ((IKeyboardInputSink^) this)->KeyboardInputSite->OnNoMoreTabStops(request);

            }

            // Only call IsDialogMessage for keys it will do something with.
            if (msg.message == WM_SYSKEYDOWN || msg.message == WM_KEYDOWN) {
                switch (m.wParam) {
                    case VK_TAB:
                    case VK_LEFT:
                    case VK_UP:
                    case VK_RIGHT:
                    case VK_DOWN:
                    case VK_EXECUTE:
                    case VK_RETURN:
                    case VK_ESCAPE:
                    case VK_CANCEL:
                        IsDialogMessage(dialog, &m);
                        // IsDialogMessage should be called ProcessDialogMessage --
                        // it processes messages without ever really telling you
                        // if it handled a specific message or not
                        return true;
                }
            }

            return false; // not a key we handled
        }
```

これは、1つの部分に多くのコードがあるため、より詳細な説明を使用できます。 まず、マクロとC++ C++マクロを使用するコードです。`TranslateAccelerator`という名前のマクロが既に存在することに注意する必要があります。これは winuser. h で定義されています。

```cpp
#define TranslateAccelerator  TranslateAcceleratorW
```

したがって、`TranslateAcceleratorW` メソッドではなく、`TranslateAccelerator` メソッドを定義してください。

同様に、アンマネージ winuser .h メッセージとマネージ `Microsoft::Win32::MSG` 構造体の両方があります。 C++ `::` 演算子を使用して、2つを明確に区別できます。

```cpp
virtual bool TranslateAccelerator(System::Windows::Interop::MSG% msg,
    ModifierKeys modifiers) override
{
    ::MSG m = ConvertMessage(msg);
}

Both MSGs have the same data, but sometimes it is easier to work with the unmanaged definition, so in this sample you can define the obvious conversion routine:

```cpp
::MSG ConvertMessage(System::Windows::Interop::MSG% msg) {
    ::MSG m;
    m.hwnd = (HWND) msg.hwnd.ToPointer();
    m.lParam = (LPARAM) msg.lParam.ToPointer();
    m.message = msg.message;
    m.wParam = (WPARAM) msg.wParam.ToPointer();

    m.time = msg.time;

    POINT pt;
    pt.x = msg.pt_x;
    pt.y = msg.pt_y;
    m.pt = pt;

    return m;
}
```

`TranslateAccelerator`に戻ります。 基本的な原則は、Win32 関数 `IsDialogMessage` を呼び出してできるだけ多くの作業を行うことですが、`IsDialogMessage` はダイアログの外部にはアクセスできません。 ダイアログの周囲のユーザータブとして、tab キーを使用してダイアログの最後のコントロールを実行したときに、`IKeyboardInputSite::OnNoMoreStops`を呼び出すことによって、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の部分にフォーカスを設定する必要があります。

```cpp
// Win32's IsDialogMessage() will handle most of the tabbing, but doesn't know
// what to do when it reaches the last tab stop
if (m.message == WM_KEYDOWN && m.wParam == VK_TAB) {
    HWND firstTabStop = GetDlgItem(dialog, IDC_EDIT1);
    HWND lastTabStop = GetDlgItem(dialog, IDCANCEL);
    TraversalRequest^ request = nullptr;

    if (GetKeyState(VK_SHIFT) && GetFocus() == firstTabStop) {
        request = gcnew TraversalRequest(FocusNavigationDirection::Last);
    }
    else if (!GetKeyState(VK_SHIFT) && GetFocus() ==  lastTabStop) { {
        request = gcnew TraversalRequest(FocusNavigationDirection::Next);
    }

    if (request != nullptr)
        return ((IKeyboardInputSink^) this)->KeyboardInputSite->OnNoMoreTabStops(request);
}
```

最後に、`IsDialogMessage` を呼び出します。 しかし、`TranslateAccelerator` メソッドの役割の1つは、キー入力を処理したかどうか [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 指示することです。 処理しなかった場合、入力イベントは、アプリケーションの他の部分をトンネルおよびバブルできます。 ここでは、Win32 のキーボード messange 処理と入力アーキテクチャの性質を明らかにします。 残念ながら、`IsDialogMessage` は、特定のキー入力を処理するかどうかを何も返しません。 さらに悪いことに、キーストロークを処理する必要がない `DispatchMessage()` が呼び出されます。  そのため、`IsDialogMessage`をリバースエンジニアリングし、処理することがわかっているキーに対してのみ呼び出す必要があります。

```cpp
// Only call IsDialogMessage for keys it will do something with.
if (msg.message == WM_SYSKEYDOWN || msg.message == WM_KEYDOWN) {
    switch (m.wParam) {
        case VK_TAB:
        case VK_LEFT:
        case VK_UP:
        case VK_RIGHT:
        case VK_DOWN:
        case VK_EXECUTE:
        case VK_RETURN:
        case VK_ESCAPE:
        case VK_CANCEL:
            IsDialogMessage(dialog, &m);
            // IsDialogMessage should be called ProcessDialogMessage --
            // it processes messages without ever really telling you
            // if it handled a specific message or not
            return true;
    }
```

### <a name="override-tabinto-method-to-support-tabbing"></a>タブ移動をサポートするために TabInto メソッドをオーバーライドする

`TranslateAccelerator`を実装したので、ユーザーはダイアログボックス内を tab キーを使用して、より多くの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションに tab キーを移動できます。 ただし、ユーザーがダイアログボックスに戻ることはできません。 これを解決するには、`TabInto`をオーバーライドします。

```cpp
public:
    virtual bool TabInto(TraversalRequest^ request) override {
        if (request->FocusNavigationDirection == FocusNavigationDirection::Last) {
            HWND lastTabStop = GetDlgItem(dialog, IDCANCEL);
            SetFocus(lastTabStop);
        }
        else {
            HWND firstTabStop = GetDlgItem(dialog, IDC_EDIT1);
            SetFocus(firstTabStop);
        }
        return true;
    }
```

`TraversalRequest` パラメーターを使用すると、タブ操作がタブまたは shift タブのどちらであるかがわかります。

### <a name="override-onmnemonic-method-to-support-mnemonics"></a>OnMnemonic メソッドをオーバーライドしてニーモニックをサポートする

キーボードの処理はほぼ完了していますが、不足しているものがあります。ニーモニックが機能しません。 ユーザーが alt キーを押しながら F キーを押すと、"First name:" という編集ボックスにフォーカスが移動します。 そのため、`OnMnemonic` メソッドをオーバーライドします。

```cpp
virtual bool OnMnemonic(System::Windows::Interop::MSG% msg, ModifierKeys modifiers) override {
    ::MSG m = ConvertMessage(msg);

    // If it's one of our mnemonics, set focus to the appropriate hwnd
    if (msg.message == WM_SYSCHAR && GetKeyState(VK_MENU /*alt*/)) {
        int dialogitem = 9999;
        switch (m.wParam) {
            case 's': dialogitem = IDOK; break;
            case 'c': dialogitem = IDCANCEL; break;
            case 'f': dialogitem = IDC_EDIT1; break;
            case 'l': dialogitem = IDC_EDIT2; break;
            case 'p': dialogitem = IDC_EDIT3; break;
            case 'a': dialogitem = IDC_EDIT4; break;
            case 'i': dialogitem = IDC_EDIT5; break;
            case 't': dialogitem = IDC_EDIT6; break;
            case 'z': dialogitem = IDC_EDIT7; break;
        }
        if (dialogitem != 9999) {
            HWND hwnd = GetDlgItem(dialog, dialogitem);
            SetFocus(hwnd);
            return true;
        }
    }
    return false; // key unhandled
};
```

ここで `IsDialogMessage` を呼び出さないのはなぜですか。  前と同じ問題が発生しています。コードでキー入力が処理されたかどうかをコード [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 通知できる必要があり、`IsDialogMessage` できません。 また、フォーカスされている HWND がダイアログボックス内にない場合、`IsDialogMessage` はニーモニックの処理を拒否するため、2つ目の問題もあります。

### <a name="instantiate-the-hwndhost-derived-class"></a>HwndHost 派生クラスをインスタンス化する

最後に、すべてのキーとタブのサポートが配置されたので、<xref:System.Windows.Interop.HwndHost> を大きな [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションに配置できます。 メインアプリケーションが [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]に記述されている場合、適切な場所に配置する最も簡単な方法は、<xref:System.Windows.Interop.HwndHost>を配置する場所に空の <xref:System.Windows.Controls.Border> 要素を残しておくことです。 ここでは、`insertHwndHostHere`という名前の <xref:System.Windows.Controls.Border> を作成します。

```xaml
<Window x:Class="WPFApplication1.Window1"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="Windows Presentation Framework Application"
    Loaded="Window1_Loaded"
    >
    <StackPanel>
        <Button Content="WPF button"/>
        <Border Name="insertHwndHostHere" Height="200" Width="500"/>
        <Button Content="WPF button"/>
    </StackPanel>
</Window>
```

次に、コードシーケンス内の適切な場所を見つけて、<xref:System.Windows.Interop.HwndHost> をインスタンス化し、<xref:System.Windows.Controls.Border>に接続します。 この例では、<xref:System.Windows.Window> 派生クラスのコンストラクター内に配置します。

```csharp
public partial class Window1 : Window {
    public Window1() {
    }

    void Window1_Loaded(object sender, RoutedEventArgs e) {
        HwndHost host = new ManagedCpp.MyHwndHost();
        insertHwndHostHere.Child = host;
    }
}
```

次のようになります。

![実行中の WPF アプリのスクリーンショット。](./media/hosting-win32-content-in-wpf/windows-presentation-foundation-application.png)

## <a name="see-also"></a>参照

- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
