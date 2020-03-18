---
title: Windows フォーム アプリケーションの基礎
ms.date: 07/20/2015
helpviewer_keywords:
- Windows applications
- Windows Forms, Visual Basic
ms.assetid: 0b919d30-7fd6-42db-85c8-543d15312441
ms.openlocfilehash: 1aa1edf0130e388c6cc87662d83591f41a8e2325
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349160"
---
# <a name="windows-forms-application-basics-visual-basic"></a>Windows フォーム アプリケーションの基礎 (Visual Basic)

Visual Basic の重要な部分は、ユーザーのコンピューター上でローカルに実行される Windows フォームアプリケーションを作成できることです。 Visual Studio を使用して、Windows フォームを使用してアプリケーションとユーザーインターフェイスを作成できます。 Windows フォームアプリケーションは、<xref:System.Windows.Forms> 名前空間のクラスに基づいて構築されています。

## <a name="designing-windows-forms-applications"></a>Windows フォームアプリケーションの設計

Visual Studio で Windows フォームおよび Windows サービスアプリケーションを作成できます。 詳細については、次のトピックを参照してください。

- [Windows フォームではじめに](../../../framework/winforms/getting-started-with-windows-forms.md)します。 Windows フォームを作成およびプログラミングする方法について説明します。

- [コントロールを Windows フォーム](../../../framework/winforms/controls/index.md)します。 Windows フォームコントロールの使用について説明しているトピックのコレクションです。

- [Windows サービスアプリケーション](../../../framework/windows-services/index.md)。 Windows サービスを作成する方法を説明するトピックの一覧を示します。

## <a name="building-rich-interactive-user-interfaces"></a>リッチで対話型のユーザー インターフェイスの構築

Windows フォームは、.NET Framework のスマートクライアントコンポーネントであり、ファイルシステムへの読み取りや書き込みなどの一般的なアプリケーションタスクを可能にするマネージライブラリのセットです。 Visual Studio などの開発環境を使用すると、情報を表示し、ユーザーからの入力を要求し、ネットワーク経由でリモートコンピューターと通信する Windows フォームアプリケーションを作成できます。

Windows フォームでは、フォームはユーザーに情報を表示するビジュアル サーフェイスです。 一般的には、フォームにコントロールを配置し、マウスのクリックやキーの押下などのユーザーアクションへの応答を開発することによって Windows フォームアプリケーションを構築します。 "*コントロール*" は、データを表示したりデータ入力を受け入れたりする独立したユーザー インターフェイス (UI) 要素です。

### <a name="events"></a>イベント

ユーザーがフォームまたはコントロールのいずれかに対して何らかの操作を行うと、イベントが生成されます。 アプリケーションは、コードを使用してこれらのイベントに反応し、イベントが発生したときにそのイベントを処理します。 詳細については、「[Windows フォーム内でのイベント ハンドラーの作成](../../../framework/winforms/creating-event-handlers-in-windows-forms.md)」を参照してください。

### <a name="controls"></a>Controls

Windows フォームには、テキストボックス、ボタン、ドロップダウンボックス、オプションボタン、さらに Web ページを表示するコントロールなど、フォームに配置できるさまざまなコントロールが含まれています。 フォーム上で使用できるすべてのコントロールの一覧については、「[Windows フォームで使用するコントロール](../../../framework/winforms/controls/controls-to-use-on-windows-forms.md)」を参照してください。 既存のコントロールがニーズを満たしていない場合に、Windows フォームは <xref:System.Windows.Forms.UserControl> クラスを使用した独自のカスタム コントロールの作成もサポートしています。

Windows フォームには、Microsoft Office のようなハイエンド アプリケーションの機能をエミュレートする豊富な UI コントロールが用意されています。 <xref:System.Windows.Forms.ToolStrip> と <xref:System.Windows.Forms.MenuStrip> コントロールを使用すると、テキストや画像を含むツールバーやメニューを作成したり、サブメニューを表示したり、テキストボックスやコンボボックスなどの他のコントロールをホストしたりできます。

Visual Studio のドラッグアンドドロップフォームデザイナーを使用すると、Windows フォームアプリケーションを簡単に作成できます。カーソルを使用してコントロールを選択し、フォーム上の任意の場所に配置するだけです。 デザイナーには、グリッド線や "スナップ線" などのツールが用意されており、コントロールの配置が煩雑になります。 また、Visual Studio を使用するか、コマンドラインでコンパイルするかにかかわらず、<xref:System.Windows.Forms.FlowLayoutPanel>、<xref:System.Windows.Forms.TableLayoutPanel> および <xref:System.Windows.Forms.SplitContainer> コントロールを使用して、最小限の時間と労力で高度なフォームレイアウトを作成できます。

### <a name="custom-ui-elements"></a>カスタム UI 要素

最後に、独自のカスタム UI 要素を作成する必要がある場合、<xref:System.Drawing> 名前空間には、線、円、およびその他の図形をフォームに直接レンダリングするために必要なすべてのクラスが含まれています。

これらの機能の使用に関する詳細な手順については、次のヘルプトピックを参照してください。

|目的|参照先|
|--------|---------|
|Visual Studio を使用して新しい Windows フォームアプリケーションを作成する|[チュートリアル 1: ピクチャビューアーを作成する](/visualstudio/ide/tutorial-1-create-a-picture-viewer)|
|フォーム上のコントロールを使用する|[方法 : Windows フォームにコントロールを追加する](../../../framework/winforms/controls/how-to-add-controls-to-windows-forms.md)|
|<xref:System.Drawing> を使用してグラフィックスを作成する|[グラフィックス プログラミングについて](../../../framework/winforms/advanced/getting-started-with-graphics-programming.md)|
|カスタムコントロールの作成|[方法: UserControl クラスを継承する](../../../framework/winforms/controls/how-to-inherit-from-the-usercontrol-class.md)|

## <a name="displaying-and-manipulating-data"></a>データの表示と操作

多くのアプリケーションは、データベース、XML ファイル、XML Web サービス、またはその他のデータ ソースからデータを表示する必要があります。 Windows フォームには、このような表形式のデータを従来の行形式や列形式で表示するための <xref:System.Windows.Forms.DataGridView> コントロールと呼ばれる柔軟なコントロールが用意されています。これにより、すべてのデータが独自のセルを占有するようになります。 <xref:System.Windows.Forms.DataGridView> を使用すると、個々のセルの外観をカスタマイズしたり、任意の行や列を固定したり、他の機能の中で複雑なコントロールをセル内に表示したりすることができます。

ネットワーク経由のデータ ソースへの接続は、Windows フォームのスマート クライアントを使用すればシンプルなタスクです。 <xref:System.Windows.Forms.BindingSource> コンポーネント、Visual Studio 2005 の Windows フォームと .NET Framework 2.0 は、データソースへの接続を表し、データをコントロールにバインドしたり、前のレコードと次のレコードに移動したり、レコードを編集したり、変更内容を元のソースに保存したりするためのメソッドを公開します。 <xref:System.Windows.Forms.BindingNavigator> コントロールは、ユーザーがレコード間を移動する <xref:System.Windows.Forms.BindingSource> コンポーネントに対して、シンプルなインターフェイスを提供します。

### <a name="data-bound-controls"></a>データバインドコントロール

[データソース] ウィンドウを使用すると、データバインドコントロールを簡単に作成できます。このウィンドウには、データベース、Web サービス、オブジェクトなどのデータソースがプロジェクト内に表示されます。 このウィンドウからプロジェクトのフォームに項目をドラッグして、データ バインド コントロールを作成できます。 また、[データ ソース] ウィンドウから既存のコントロールにオブジェクトをドラッグして、データに既存のコントロールをバインドすることもできます。

### <a name="settings"></a>設定

Windows フォームで管理できる別の種類のデータ バインディングは設定です。 ほとんどのスマートクライアントアプリケーションは、フォームの最後のサイズなどの実行時の状態に関する情報を保持し、保存されたファイルの既定の場所などのユーザー設定データを保持する必要があります。 アプリケーション設定機能は、クライアントコンピューターに両方の種類の設定を簡単に格納できるようにすることで、これらの要件に対処します。 Visual Studio またはコードエディターを使用して定義すると、これらの設定は XML として永続化され、実行時に自動的にメモリに読み込まれます。

これらの機能の使用に関する詳細な手順については、次のヘルプトピックを参照してください。

|目的|参照先|
|--------|---------|
|<xref:System.Windows.Forms.BindingSource> コンポーネントの使用|[方法 : デザイナーを使用して Windows フォーム コントロールを BindingSource コンポーネントにバインドする](../../../framework/winforms/controls/bind-wf-controls-with-the-bindingsource.md)|
|ADO.NET データソースを操作する|[方法 : Windows フォーム BindingSource コンポーネントで ADO.NET データを並べ替える/フィルター処理する](../../../framework/winforms/controls/sort-and-filter-ado-net-data-with-wf-bindingsource-component.md)|
|[データソース] ウィンドウを使用する|[チュートリアル: Windows フォームでのデータの表示](/visualstudio/data-tools/accessing-data-in-visual-studio)|

## <a name="deploying-applications-to-client-computers"></a>クライアント コンピューターにアプリケーションを配置する

アプリケーションを作成したら、それをユーザーに送信して、ユーザーが自分のクライアントコンピューターにインストールして実行できるようにする必要があります。 ClickOnce テクノロジを使用すると、わずか数回のクリックで、Visual Studio 内からアプリケーションを配置し、Web 上のアプリケーションを指す URL をユーザーに提供できます。 ClickOnce は、アプリケーション内のすべての要素と依存関係を管理し、クライアントコンピューターにアプリケーションが正しくインストールされていることを確認します。

ClickOnce アプリケーションは、ユーザーがネットワークに接続されている場合にのみ実行するように構成することも、オンラインとオフラインの両方で実行するように構成することもできます。 アプリケーションでオフライン操作をサポートするように指定すると、ClickOnce によって、ユーザーが URL を使用せずにアプリケーションを開くことができるように、ユーザーの **[スタート]** メニューにアプリケーションへのリンクが追加されます。

アプリケーションを更新するときに、新しい配置マニフェストとアプリケーションの新しいコピーを Web サーバーに発行します。 ClickOnce は、使用可能な更新プログラムがあることを検出し、ユーザーのインストールをアップグレードします。古いアセンブリを更新するために、カスタムプログラミングは必要ありません。

ClickOnce の完全な概要については、「 [clickonce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)」を参照してください。 これらの機能の使用に関する詳細な手順については、次のヘルプトピックを参照してください。

|目的|参照先|
|--------|---------|
|ClickOnce を使用してアプリケーションを配置する|[方法: 発行ウィザードを使用して ClickOnce アプリケーションを発行する](/visualstudio/deployment/how-to-publish-a-clickonce-application-using-the-publish-wizard)<br /><br /> [チュートリアル : ClickOnce アプリケーションを手動で配置する](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)|
|ClickOnce 配置を更新する|[方法 : ClickOnce アプリケーションの更新プログラムを管理する](/visualstudio/deployment/how-to-manage-updates-for-a-clickonce-application)|
|ClickOnce を使用してセキュリティを管理する|[方法 : ClickOnce のセキュリティ設定を有効にする](/visualstudio/deployment/how-to-enable-clickonce-security-settings)|

## <a name="other-controls-and-features"></a>その他のコントロールおよび機能

Windows フォームには、ダイアログ ボックスの作成、ヘルプやドキュメントの印刷や追加、アプリケーションの複数言語へのローカライズのサポートなど、一般的なタスクを高速で簡単に実装できる機能が他にも多数あります。 さらに、Windows フォームは .NET Framework の堅牢なセキュリティシステムに依存しているため、より安全なアプリケーションを顧客にリリースできます。

これらの機能の使用に関する詳細な手順については、次のヘルプトピックを参照してください。

|目的|参照先|
|--------|---------|
|フォームの内容を印刷する|[方法 : Windows フォームでグラフィックスを印刷する](../../../framework/winforms/advanced/how-to-print-graphics-in-windows-forms.md)<br /><br /> [方法 : Windows フォームで複数ページのテキスト ファイルを印刷する](../../../framework/winforms/advanced/how-to-print-a-multi-page-text-file-in-windows-forms.md)|
|Windows フォームのセキュリティについての詳細|[Windows フォームのセキュリティの概要](../../../framework/winforms/security-in-windows-forms-overview.md)|

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- [Windows フォームの概要](../../../framework/winforms/windows-forms-overview.md)
- [My.Forms オブジェクト](../../../visual-basic/language-reference/objects/my-forms-object.md)
