---
title: Win32 で WPF コンテンツをホストする
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- hosting WPF content in Win32 window [WPF]
ms.assetid: 38ce284a-4303-46dd-b699-c9365b22a7dc
ms.openlocfilehash: 418c5a4708a7842e5e441235738b73a009c9c956
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124548"
---
# <a name="walkthrough-hosting-wpf-content-in-win32"></a>チュートリアル: Win32 での WPF コンテンツのホスト
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、アプリケーションの作成に適した環境を提供します。 ただし、Win32 コードに多大な投資をしている場合は、元のコードを書き換えるのではなく、アプリケーションに [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 機能を追加する方がより効果的な場合があります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、Win32 ウィンドウで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストするための簡単なメカニズムを提供します。  
  
 このチュートリアルでは、win32 ウィンドウで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツをホストするサンプルアプリケーション ( [Win32 ウィンドウで WPF コンテンツ](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/Win32HostingWPFPage)をホストするサンプル) を記述する方法について説明します。 このサンプルを拡張して、任意の Win32 ウィンドウをホストすることができます。 マネージコードとアンマネージコードの混在が必要になるため、 C++アプリケーションは/cli で記述されています。  

<a name="requirements"></a>   
## <a name="requirements"></a>必要条件  
 このチュートリアルでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と Win32 プログラミングの基本的な知識があることを前提としています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プログラミングの基本的な概要については、「[はじめに](../getting-started/index.md)」を参照してください。 Win32 プログラミングの概要については、「チャールズ Petzold 著) による特定の*プログラミングウィンドウ*」で、件名に関する多くの書籍を参照する必要があります。  
  
 このチュートリアルに付属するサンプルは/Cli でC++実装されているので、このチュートリアルでC++は、を使用して Windows API をプログラミングする方法とマネージコードプログラミングについて理解していることを前提としています。 /Cli にC++関する知識は役に立ちますが、必須ではありません。  
  
> [!NOTE]
> このチュートリアルには、関連するサンプルからのコード例が多数含まれています。 しかし、読みやすくするため、完全なサンプル コードは含まれていません。 完全なサンプルコードについては、「 [Win32 ウィンドウでの WPF コンテンツのホスト](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/Win32HostingWPFPage)」のサンプルを参照してください。  
  
<a name="basic_procedure"></a>   
## <a name="the-basic-procedure"></a>基本手順  
 ここでは、Win32 ウィンドウで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストするために使用する基本的な手順の概要を説明します。 残りのセクションでは、各手順の詳細について説明します。  
  
 Win32 ウィンドウで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストするためのキーは、<xref:System.Windows.Interop.HwndSource> クラスです。 このクラスは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツを Win32 ウィンドウにラップし、子ウィンドウとして [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] に組み込むことができるようにします。 次の方法では、1つのアプリケーションで Win32 と [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が結合されます。  
  
1. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをマネージクラスとして実装します。  
  
2. /Cli を使用してC++Windows アプリケーションを実装する 既存のアプリケーションおよびアンマネージC++コードから開始する場合は、通常、プロジェクト設定を変更して `/clr` コンパイラフラグを含めることにより、マネージコードを呼び出すことができます。  
  
3. スレッド処理モデルをシングル スレッド アパートメント (STA: Single Threaded Apartment) に設定します。  
  
4. ウィンドウプロシージャで[WM_CREATE](/windows/desktop/winmsg/wm-create)通知を処理し、次の手順を実行します。  
  
    1. 新しい <xref:System.Windows.Interop.HwndSource> オブジェクトを、親ウィンドウがその `parent` パラメーターとなるように指定して作成します。  
  
    2. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツ クラスのインスタンスを作成します。  
  
    3. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツオブジェクトへの参照を <xref:System.Windows.Interop.HwndSource>の <xref:System.Windows.Interop.HwndSource.RootVisual%2A> プロパティに割り当てます。  
  
    4. コンテンツの HWND を取得します。 <xref:System.Windows.Interop.HwndSource.Handle%2A> オブジェクトの <xref:System.Windows.Interop.HwndSource> プロパティにウィンドウ ハンドル (HWND) が格納されます。 アプリケーションのアンマネージ部分で使用できる HWND を取得するには、`Handle.ToPointer()` を HWND にキャストします。  
  
5. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツへの参照を保持する静的フィールドを含むマネージド クラスを実装します。 このクラスを使用すると、Win32 コードから [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツへの参照を取得できます。  
  
6. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツを静的フィールドに割り当てます。  
  
7. 1つ以上の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントにハンドラーをアタッチして、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツから通知を受信します。  
  
8. 静的フィールドに格納した参照を使用して [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツと通信し、プロパティの設定などを行います。  
  
> [!NOTE]
> [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] を使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツを実装することもできます。 ただし、ダイナミックリンクライブラリ (DLL) として個別にコンパイルし、Win32 アプリケーションからその DLL を参照する必要があります。 手順の残りの部分は、前述の手順と同様です。

<a name="implementing_the_application"></a>
## <a name="implementing-the-host-application"></a>ホスト アプリケーションの実装
 ここでは、基本的な Win32 アプリケーションで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストする方法について説明します。 コンテンツ自体は、マネージクラスC++として/cli に実装されます。 ほとんどの部分が、簡単な [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のプログラミングです。 コンテンツの実装の重要な側面については、「 [WPF コンテンツの実装](#implementing_the_wpf_page)」で説明します。

- [基本的なアプリケーション](#the_basic_application)

- [WPF コンテンツのホスティング](#hosting_the_wpf_page)

- [WPF コンテンツへの参照の保持](#holding_a_reference)

- [WPF コンテンツとの通信](#communicating_with_the_page)

<a name="the_basic_application"></a>
### <a name="the-basic-application"></a>基本的なアプリケーション
 ホストアプリケーションの開始点は、Visual Studio 2005 テンプレートを作成することでした。

1. Visual Studio 2005 を開き、 **[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。

2. ビジュアル C++プロジェクトの種類の一覧から [Win32] を選択します。 既定の言語がでないC++場合は、これらのプロジェクトの種類が [**その他の言語] の**下に表示されます。

3. **Win32 プロジェクト**テンプレートを選択し、プロジェクトに名前を割り当てて、 **[OK]** をクリックして、 **win32 アプリケーションウィザード**を起動します。

4. ウィザードの既定の設定をそのまま使用し、 **[完了]** をクリックしてプロジェクトを開始します。

 このテンプレートは、次のような基本的な Win32 アプリケーションを作成します。

- アプリケーションのエントリ ポイント。

- 関連するウィンドウ プロシージャ (WndProc) を含むウィンドウ。

- **ファイル**と**ヘルプ**の見出しを持つメニュー。 **[ファイル]** メニューには、アプリケーションを閉じる **[終了]** 項目があります。 **[ヘルプ]** メニューには、簡単なダイアログボックスを起動する **[About]** 項目があります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツをホストするコードの記述を開始する前に、基本テンプレートに2つの変更を加える必要があります。

 1 つ目は、プロジェクトをマネージド コードとしてコンパイルすることです。 既定では、プロジェクトはアンマネージ コードとしてコンパイルされます。 ただし、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] はマネージド コードで実装されているため、プロジェクトは状況に応じてコンパイルする必要があります。

1. **ソリューションエクスプローラー**でプロジェクト名を右クリックし、コンテキストメニューの **[プロパティ]** をクリックして、 **[プロパティページ]** ダイアログボックスを開きます。

2. 左側のウィンドウのツリービューで、 **[構成プロパティ]** を選択します。

3. 右ペインの **[プロジェクトの既定値]** の一覧から **[共通言語ランタイム]** サポート を選択します。

4. ドロップダウンリストボックスから **[共通言語ランタイムサポート (/clr)]** を選択します。

> [!NOTE]
> このコンパイラ フラグを使用すると、アプリケーションでマネージド コードを使用できますが、アンマネージド コードは以前と同様にコンパイルされます。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、シングル スレッド アパートメント (STA) スレッド処理モデルを使用します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツコードで適切に機能するには、エントリポイントに属性を適用して、アプリケーションのスレッドモデルを STA に設定する必要があります。

 [!code-cpp[Win32HostingWPFPage#WinMain](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/Win32HostingWPFPage.cpp#winmain)]

<a name="hosting_the_wpf_page"></a>
### <a name="hosting-the-wpf-content"></a>WPF コンテンツのホスティング
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは、単純なアドレス入力アプリケーションです。 それは、ユーザー名やアドレスなどを取得する複数の <xref:System.Windows.Controls.TextBox> コントロールで構成されています。 また、 **[OK]** と **[キャンセル**] の2つの <xref:System.Windows.Controls.Button> コントロールもあります。 ユーザーが **[OK]** をクリックすると、ボタンの <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントハンドラーは、<xref:System.Windows.Controls.TextBox> コントロールからデータを収集し、それを対応するプロパティに割り当て、カスタムイベント `OnButtonClicked`を発生させます。 ユーザーが **[キャンセル**] をクリックすると、ハンドラーは単に `OnButtonClicked`を発生させます。 `OnButtonClicked` のイベント引数オブジェクトには、どのボタンをクリックしたかを示すブール型フィールドが含まれています。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストするコードは、ホストウィンドウの[WM_CREATE](/windows/desktop/winmsg/wm-create)通知のハンドラーに実装されます。

 [!code-cpp[Win32HostingWPFPage#WMCreate](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/Win32HostingWPFPage.cpp#wmcreate)]

 `GetHwnd` メソッドは、サイズと位置の情報に加え、親ウィンドウハンドルを取得し、ホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツのウィンドウハンドルを返します。

> [!NOTE]
> `#using` 名前空間に `System::Windows::Interop` ディレクティブを使用することはできません。 使用すると、その名前空間の <xref:System.Windows.Interop.MSG> 構造体と winuser.h で宣言した MSG 構造体の間で名前の競合が発生します。 代わりに、その名前空間のコンテンツにアクセスするための完全修飾名を使用する必要があります。

 [!code-cpp[Win32HostingWPFPage#GetHwnd](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/Win32HostingWPFPage.cpp#gethwnd)]

 アプリケーションウィンドウで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツを直接ホストすることはできません。 代わりに、まず <xref:System.Windows.Interop.HwndSource> コンテンツをラップするための [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] オブジェクトを作成します。 このオブジェクトは、基本的には [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツをホストするように設計されたウィンドウです。 <xref:System.Windows.Interop.HwndSource> オブジェクトは、アプリケーションの一部である Win32 ウィンドウの子として作成することによって、親ウィンドウでホストします。 <xref:System.Windows.Interop.HwndSource> コンストラクターパラメーターには、Win32 子ウィンドウを作成するときに CreateWindow に渡す情報とほぼ同じ情報が含まれています。

 次に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] content オブジェクトのインスタンスを作成します。 この場合、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の内容は、/Cli を使用しC++て別のクラス `WPFPage`として実装されます。 さらに、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で実装することもできます。 ただし、これを行うには、別のプロジェクトを設定し、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツを DLL としてビルドする必要があります。 この DLL への参照をプロジェクトに追加し、その参照を使用して [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツのインスタンスを作成することができます。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツへの参照を <xref:System.Windows.Interop.HwndSource>の <xref:System.Windows.Interop.HwndSource.RootVisual%2A> プロパティに割り当てることにより、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツを子ウィンドウに表示します。

 次のコード行は、イベント ハンドラー `WPFButtonClicked` を [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツの `OnButtonClicked` イベントにアタッチしています。 このハンドラーは、ユーザーが **[OK]** または **[キャンセル**] ボタンをクリックしたときに呼び出されます。 このイベントハンドラーの詳細については、「 [communicating_with_the_WPF コンテンツ](#communicating_with_the_page)」を参照してください。

 示されているコードの最後の行は、<xref:System.Windows.Interop.HwndSource> オブジェクトに関連付けられているウィンドウ ハンドル (HWND) を返します。 このハンドルを Win32 コードから使用すると、ホストされているウィンドウにメッセージを送信できます。ただし、サンプルではこれは実行されません。 <xref:System.Windows.Interop.HwndSource> オブジェクトは、メッセージを受信するたびにイベントを発生させます。 メッセージを処理するには、<xref:System.Windows.Interop.HwndSource.AddHook%2A> メソッドを呼び出してメッセージ ハンドラーをアタッチしてから、そのハンドラーでメッセージを処理します。

<a name="holding_a_reference"></a>
### <a name="holding-a-reference-to-the-wpf-content"></a>WPF コンテンツへの参照の保持
 多くのアプリケーションでは、後で [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツと通信することができます。 たとえば、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツのプロパティを変更したり、場合によっては <xref:System.Windows.Interop.HwndSource> オブジェクトが異なる [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストするようにしたりできます。 そのためには、<xref:System.Windows.Interop.HwndSource> オブジェクトまたは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツへの参照が必要です。 <xref:System.Windows.Interop.HwndSource> オブジェクトと関連する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは、ウィンドウ ハンドルを破棄するまでメモリに残ります。 ただし、<xref:System.Windows.Interop.HwndSource> オブジェクトに割り当てる変数は、ウィンドウ プロシージャから戻ると同時にスコープの外に出ます。 Win32 アプリケーションでこの問題を処理するための一般的な方法は、静的変数またはグローバル変数を使用することです。 残念ながら、このような変数の種類に対してマネージド オブジェクトを割り当てることはできません。 <xref:System.Windows.Interop.HwndSource> オブジェクトに関連付けられているウィンドウ ハンドルを、グローバル変数または静的変数に割り当てることができますが、オブジェクト自体にアクセスすることはできません。

 この問題の最も簡単な解決法は、静的フィールドのセットを含むマネージド クラスを実装して、アクセスが必要なすべてのマネージド オブジェクトへの参照を保持することです。 サンプルでは、`WPFPageHost` クラスを使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツへの参照、および後でユーザーが変更する可能性があるプロパティの数の初期値を保持します。 これは、ヘッダーで定義します。

 [!code-cpp[Win32HostingWPFPage#WPFPageHost](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/Win32HostingWPFPage.h#wpfpagehost)]

 `GetHwnd` 関数の後半部分では、後に、`myPage` がスコープ内にある間に使用するため、対象のフィールドに値を割り当てます。

<a name="communicating_with_the_page"></a>
### <a name="communicating-with-the-wpf-content"></a>WPF コンテンツとの通信
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツとの通信には次の 2 種類があります。 アプリケーションは、ユーザーが **[OK]** ボタンまたは **[キャンセル**] ボタンをクリックしたときに [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツから情報を受け取ります。 アプリケーションには、背景色や既定のフォント サイズなどのさまざまな [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] コンテンツのプロパティをユーザーが変更できるようにする [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] があります。

 前述のように、ユーザーがいずれかのボタンをクリックすると、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは `OnButtonClicked` イベントを発生させます。 アプリケーションは、これらの通知を受信するため、このイベントにハンドラーをアタッチします。 **[OK** ] ボタンがクリックされた場合、ハンドラーは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツからユーザー情報を取得し、一連の静的コントロールに表示します。

 [!code-cpp[Win32HostingWPFPage#WPFButtonClicked](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/Win32HostingWPFPage.cpp#wpfbuttonclicked)]

 ハンドラーは、カスタム イベント引数オブジェクトを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツ、`MyPageEventArgs` から受信します。 オブジェクトの `IsOK` プロパティは、 **[OK** ] ボタンがクリックされた場合は `true` に設定され、 **[キャンセル**] ボタンがクリックされた場合は `false` ます。

 **[OK** ] ボタンがクリックされた場合、ハンドラーはコンテナークラスから [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツへの参照を取得します。 その後、関連する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツ プロパティが保持するユーザー情報を収集し、静的コントロールを使用して親ウィンドウに情報を表示します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツデータはマネージ文字列の形式であるため、Win32 コントロールで使用するためにマーシャリングする必要があります。 **[キャンセル**] ボタンがクリックされた場合、ハンドラーは静的コントロールからデータをクリアします。

 アプリケーション [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] には、ユーザーが [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツの背景色を変更できるようにするラジオ ボタンのセット、および複数のフォント関連のプロパティが用意されています。 次の例は、アプリケーションのウィンドウ プロシージャ (WndProc)、および背景色など、各種のメッセージに対してさまざまなプロパティを設定するそのプロシージャのメッセージ処理からの抜粋です。 その他は類似しているため、示していません。 詳細とコンテキストについては、完全なサンプルを参照してください。

 [!code-cpp[Win32HostingWPFPage#WMCommandToBG](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/Win32HostingWPFPage.cpp#wmcommandtobg)]

 背景色を設定するには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] から `hostedPage` コンテンツ (`WPFPageHost`) への参照を取得して、背景色のプロパティを適切な色に設定します。 サンプルでは、元の色、明るい緑、または明るいサーモン色の 3 つの色のオプションを使用します。 元の背景色は静的フィールドとして `WPFPageHost` クラスに格納されます。 他の 2 つの色を設定するには、新しい <xref:System.Windows.Media.SolidColorBrush> オブジェクトを作成して、<xref:System.Windows.Media.Colors> オブジェクトからコンストラクターに静的な色の値を渡します。

<a name="implementing_the_wpf_page"></a>
## <a name="implementing-the-wpf-page"></a>WPF ページの実装
 実際の実装についての知識がなくても、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツをホストして使用することができます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツが別の DLL にパッケージされている場合は、任意の共通言語ランタイム (CLR) 言語でビルドされている可能性があります。 次に、サンプルで使用さC++れる/cli 実装の簡単なチュートリアルを示します。 このセクションには、次のサブセクションが含まれています。

- [レイアウト](#page_layout)

- [データをホスト ウィンドウに返す](#returning_data_to_window)

- [WPF のプロパティを設定する](#set_page_properties)

<a name="page_layout"></a>
### <a name="layout"></a>[レイアウト]
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツ内の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素は、5つの <xref:System.Windows.Controls.TextBox> コントロールで構成され、<xref:System.Windows.Controls.Label> コントロール (名前、住所、市区町村、都道府県、および Zip) が関連付けられています。 **[OK]** と **[キャンセル**] の2つの <xref:System.Windows.Controls.Button> コントロールもあります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは `WPFPage` クラスに実装されています。 レイアウトは、<xref:System.Windows.Controls.Grid> レイアウト要素で処理されます。 クラスは <xref:System.Windows.Controls.Grid> から継承されます。これにより、クラスは効果的に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツのルート要素になります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツコンストラクターは、必要な幅と高さを取得し、それに応じて <xref:System.Windows.Controls.Grid> サイズを調整します。 次に、一連の <xref:System.Windows.Controls.ColumnDefinition> オブジェクトと <xref:System.Windows.Controls.RowDefinition> オブジェクトを作成し、それぞれ <xref:System.Windows.Controls.Grid> オブジェクトベース <xref:System.Windows.Controls.Grid.ColumnDefinitions%2A> および <xref:System.Windows.Controls.Grid.RowDefinitions%2A> コレクションに追加することによって、基本的なレイアウトを定義します。 これにより、5 つの行と、7 つの列のグリッドが定義され、セルの内容によって大きさが決定します。

 [!code-cpp[Win32HostingWPFPage#WPFPageCtorToGridDef](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagectortogriddef)]

 次に、コンストラクターは [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素を <xref:System.Windows.Controls.Grid> に追加します。 最初の要素はタイトルのテキストです。これは、グリッドの 1 行目の中央に表示される <xref:System.Windows.Controls.Label> コントロールです。

 [!code-cpp[Win32HostingWPFPage#WPFPageCtorTitle](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagectortitle)]

 次の行には、名前の <xref:System.Windows.Controls.Label> コントロールと関連する <xref:System.Windows.Controls.TextBox> コントロールが格納されます。 ラベルとテキスト ボックスの各ペアに同じコードが使用されるため、コードはプライベート メソッドのペアに配置され、5 つのラベルとテキスト ボックスのペアすべてに使用されます。 メソッドは適切な制御を作成し、<xref:System.Windows.Controls.Grid> クラスの静的な <xref:System.Windows.Controls.Grid.SetColumn%2A> および <xref:System.Windows.Controls.Grid.SetRow%2A> メソッドを呼び出して、適切なセルにコントロールを配置します。 コントロールが作成されると、サンプルは <xref:System.Windows.Controls.UIElementCollection.Add%2A> の <xref:System.Windows.Controls.Panel.Children%2A> プロパティの <xref:System.Windows.Controls.Grid> メソッドを呼び出して、グリッドにコントロールを追加します。 残りのラベルとテキスト ボックスのペアを追加するコードは似ています。 詳細については、サンプル コードを参照してください。

 [!code-cpp[Win32HostingWPFPage#WPFPageCtorName](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagectorname)]

 2 つのメソッドの実装は、次のとおりです。

 [!code-cpp[Win32HostingWPFPage#WPFPageCreateHelpers](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagecreatehelpers)]

 最後に、このサンプルでは、 **[OK]** ボタンと **[キャンセル**] ボタンを追加し、イベントハンドラーを <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントにアタッチします。

 [!code-cpp[Win32HostingWPFPage#WPFPageCtorButtonsEvents](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagectorbuttonsevents)]

<a name="returning_data_to_window"></a>
### <a name="returning-the-data-to-the-host-window"></a>データをホスト ウィンドウに返す
 いずれかのボタンをクリックすると、その <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントが発生します。 ホスト ウィンドウはこれらのイベントにハンドラーをアタッチして、<xref:System.Windows.Controls.TextBox> コントロールから直接データを取得します。 サンプルは、いくぶん直接的ではない方法を使用します。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツ内の <xref:System.Windows.Controls.Primitives.ButtonBase.Click> を処理し、カスタムイベント `OnButtonClicked`を発生させて [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツに通知します。 これにより、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツは、ホストに通知する前にパラメーターの検証を行うことができます。 ハンドラーは、<xref:System.Windows.Controls.TextBox> コントロールからテキストを取得し、パブリック プロパティに割り当てます。ここからホストは情報を取得します。

 WPFPage.h でのイベント宣言:

 [!code-cpp[Win32HostingWPFPage#WPFPageEventDecl](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.h#wpfpageeventdecl)]

 WPFPage.cpp での <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベント ハンドラー:

 [!code-cpp[Win32HostingWPFPage#WPFPageButtonClicked](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagebuttonclicked)]

<a name="set_page_properties"></a>
### <a name="setting-the-wpf-properties"></a>WPF のプロパティを設定する
 ユーザーは Win32 ホストを使用して、いくつかの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツのプロパティを変更できます。 Win32 側からは、プロパティを変更するだけです。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のコンテンツ クラスの実装はいくらか複雑になります。これは、全コントロールのフォントを制御する 1 つのグローバル プロパティがないためです。 代わりに、各コントロールの適切なプロパティは、プロパティの set アクセサーで変更されます。 次の例は、`DefaultFontFamily` プロパティのコードを示しています。 プロパティを設定すると、プライベート メソッドが呼び出され、さまざまなコントロールに <xref:System.Windows.Controls.Control.FontFamily%2A> プロパティが設定されます。

 WPFPage.h から:

 [!code-cpp[Win32HostingWPFPage#WPFPageFontFamilyProperty](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.h#wpfpagefontfamilyproperty)]

 WPFPage.cpp から:

 [!code-cpp[Win32HostingWPFPage#WPFPageSetFontFamily](~/samples/snippets/cpp/VS_Snippets_Wpf/Win32HostingWPFPage/CPP/WPFPage.cpp#wpfpagesetfontfamily)]

## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.HwndSource>
- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
