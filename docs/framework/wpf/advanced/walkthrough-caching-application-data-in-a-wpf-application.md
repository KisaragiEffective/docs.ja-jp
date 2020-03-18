---
title: アプリデータをキャッシュする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- walkthroughs [WPF], caching
- caching [.NET Framework]
- caching [WPF]
ms.assetid: dac2c9ce-042b-4d23-91eb-28f584415cef
ms.openlocfilehash: b7d999f94e2f2ae410a16e537d51c0f890def4e1
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728060"
---
# <a name="walkthrough-caching-application-data-in-a-wpf-application"></a>チュートリアル: WPF アプリケーション内のアプリケーション データのキャッシュ
キャッシュを使用すると、メモリにデータを格納して高速にアクセスできます。 アプリケーションからそのデータに再アクセスするときに、元のソースからではなく、キャッシュからデータを取得できます。 そのため、パフォーマンスとスケーラビリティが向上します。 また、データ ソースが一時的に使用できない場合でも、キャッシュのデータを使用できます。

 .NET Framework には、.NET Framework アプリケーションでキャッシュを使用できるようにするクラスが用意されています。 これらのクラスは <xref:System.Runtime.Caching> 名前空間にあります。

> [!NOTE]
> <xref:System.Runtime.Caching> 名前空間は、.NET Framework 4 で新たに追加されています。 この名前空間により、すべての .NET Framework アプリケーションでキャッシュを使用できるようになります。 以前のバージョンの .NET Framework では、キャッシュは <xref:System.Web> 名前空間でのみ使用できるため、ASP.NET クラスに依存する必要がありました。

 このチュートリアルでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションの一部として .NET Framework で使用できるキャッシュ機能を使用する方法について説明します。 このチュートリアルでは、テキストファイルの内容をキャッシュします。

 このチュートリアルでは、以下のタスクを行います。

- WPF アプリケーションプロジェクトを作成しています。

- .NET Framework 4 への参照を追加しています。

- キャッシュを初期化しています。

- テキストファイルの内容を含むキャッシュエントリを追加します。

- キャッシュエントリの削除ポリシーを指定します。

- キャッシュされたファイルのパスを監視し、監視対象の項目に対する変更についてキャッシュインスタンスに通知します。

## <a name="prerequisites"></a>前提条件
 このチュートリアルを完了するには、以下が必要です。

- Visual Studio 2010。

- 少量のテキストを含むテキストファイル。 (テキストファイルの内容はメッセージボックスに表示されます)。このチュートリアルで示すコードは、次のファイルを操作していることを前提としています。

     `c:\cache\cacheText.txt`

     ただし、このチュートリアルでは、任意のテキストファイルを使用し、コードに小さな変更を加えることができます。

## <a name="creating-a-wpf-application-project"></a>WPF アプリケーションプロジェクトの作成
 まず、WPF アプリケーションプロジェクトを作成します。

#### <a name="to-create-a-wpf-application"></a>WPF アプリケーションを作成するには

1. Visual Studio を起動します。

2. **[ファイル]** メニューの **[新規作成]** をクリックし、 **[新しいプロジェクト]** をクリックします。

     **[新しいプロジェクト]** ダイアログ ボックスが表示されます。

3. **[インストールされたテンプレート]** で、使用するプログラミング言語 (**Visual Basic**または**ビジュアルC#** ) を選択します。

4. **[新しいプロジェクト]** ダイアログボックスで、 **[WPF アプリケーション]** を選択します。

    > [!NOTE]
    > **Wpf アプリケーション**テンプレートが表示されない場合は、wpf をサポートする .NET Framework のバージョンをターゲットにしていることを確認してください。 **新しいプロジェクト** ダイアログボックスで、一覧から .NET Framework 4 を選択します。

5. **[名前]** テキストボックスに、プロジェクトの名前を入力します。 たとえば、「 **WPFCaching**」と入力できます。

6. **[ソリューションのディレクトリを作成]** チェック ボックスをオンにします。

7. **[OK]** をクリックします。

     WPF デザイナーが**デザイン**ビューで開き、mainwindow.xaml ファイルが表示されます。 Visual Studio によって、 **My プロジェクト**フォルダー、app.xaml ファイル、および mainwindow.xaml ファイルが作成されます。

## <a name="targeting-the-net-framework-and-adding-a-reference-to-the-caching-assemblies"></a>.NET Framework をターゲットにして、キャッシュアセンブリへの参照を追加する
 既定では、WPF アプリケーションは .NET Framework 4 クライアントプロファイルを対象とします。 WPF アプリケーションで <xref:System.Runtime.Caching> 名前空間を使用するには、アプリケーションが .NET Framework 4 (.NET Framework 4 クライアントプロファイルではない) を対象とし、名前空間への参照を含める必要があります。

 したがって、次の手順では、.NET Framework ターゲットを変更し、<xref:System.Runtime.Caching> 名前空間への参照を追加します。

> [!NOTE]
> .NET Framework ターゲットを変更する手順は、Visual Basic プロジェクトとビジュアルC#プロジェクトで異なります。

#### <a name="to-change-the-target-net-framework-in-visual-basic"></a>Visual Basic でターゲット .NET Framework を変更するには

1. **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、 **[プロパティ]** をクリックします。

     アプリケーションの [プロパティ] ウィンドウが表示されます。

2. **[コンパイル]** タブをクリックします。

3. ウィンドウの下部にある **[詳細コンパイルオプション]** をクリックします。

     **[コンパイラの詳細設定**] ダイアログボックスが表示されます。

4. **[ターゲットフレームワーク (すべての構成)]** ボックスの一覧の [.NET Framework 4] を選択します。 (.NET Framework 4 クライアントプロファイルを選択しないでください)。

5. **[OK]** をクリックします。

     **[ターゲット フレームワークの変更]** ダイアログ ボックスが表示されます。

6. **[ターゲットフレームワークの変更]** ダイアログボックスで、 **[はい]** をクリックします。

     プロジェクトが閉じられ、再び開きます。

7. 次の手順に従って、キャッシュアセンブリへの参照を追加します。

    1. **ソリューションエクスプローラー**で、プロジェクトの名前を右クリックし、[参照の**追加**] をクリックします。

    2. **[.Net]** タブを選択し、[`System.Runtime.Caching`] を選択して、 **[OK]** をクリックします。

#### <a name="to-change-the-target-net-framework-in-a-visual-c-project"></a>ビジュアルC#プロジェクトのターゲット .NET Framework を変更するには

1. **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、 **[プロパティ]** をクリックします。

     アプリケーションの [プロパティ] ウィンドウが表示されます。

2. **[アプリケーション]** タブをクリックします。

3. **ターゲットフレームワーク** ボックスの一覧で .NET Framework 4 を選択します。 ( **.NET Framework 4 クライアントプロファイル**を選択しないでください)。

4. 次の手順に従って、キャッシュアセンブリへの参照を追加します。

    1. **[参照]** フォルダーを右クリックし、 **[参照の追加]** をクリックします。

    2. **[.Net]** タブを選択し、[`System.Runtime.Caching`] を選択して、 **[OK]** をクリックします。

## <a name="adding-a-button-to-the-wpf-window"></a>WPF ウィンドウへのボタンの追加
 次に、ボタンコントロールを追加し、ボタンの `Click` イベントのイベントハンドラーを作成します。 その後、ボタンをクリックすると、テキストファイルの内容がキャッシュされて表示されるように、後でコードを追加します。

#### <a name="to-add-a-button-control"></a>ボタンコントロールを追加するには

1. **ソリューションエクスプローラー**で、mainwindow.xaml ファイルをダブルクリックして開きます。

2. **[ツールボックス]** の **[共通の WPF コントロール]** で、`Button` コントロールを `MainWindow` ウィンドウにドラッグします。

3. **[プロパティ]** ウィンドウで、`Button` コントロールの `Content` プロパティを **[キャッシュを取得]** する に設定します。

## <a name="initializing-the-cache-and-caching-an-entry"></a>キャッシュを初期化してエントリをキャッシュする
 次に、次のタスクを実行するためのコードを追加します。

- キャッシュクラスのインスタンスを作成します。つまり、新しい <xref:System.Runtime.Caching.MemoryCache> オブジェクトをインスタンス化します。

- キャッシュが <xref:System.Runtime.Caching.HostFileChangeMonitor> オブジェクトを使用してテキストファイルの変更を監視するように指定します。

- テキストファイルを読み取り、キャッシュエントリとしてその内容をキャッシュします。

- キャッシュされたテキストファイルの内容を表示します。

#### <a name="to-create-the-cache-object"></a>キャッシュオブジェクトを作成するには

1. MainWindow.xaml.cs または Mainwindow.xaml ファイルでイベントハンドラーを作成するために、追加したボタンをダブルクリックします。

2. (クラス宣言の前に) ファイルの先頭に、次の `Imports` (Visual Basic) または `using` (C#) ステートメントを追加します。

    ```csharp
    using System.Runtime.Caching;
    using System.IO;
    ```

    ```vb
    Imports System.Runtime.Caching
    Imports System.IO
    ```

3. イベントハンドラーで、次のコードを追加して、キャッシュオブジェクトをインスタンス化します。

    ```csharp
    ObjectCache cache = MemoryCache.Default;
    ```

    ```vb
    Dim cache As ObjectCache = MemoryCache.Default
    ```

     <xref:System.Runtime.Caching.ObjectCache> クラスは、メモリ内オブジェクトキャッシュを提供する組み込みのクラスです。

4. 次のコードを追加して、`filecontents`という名前のキャッシュエントリの内容を読み取ります。

    ```vb
    Dim fileContents As String = TryCast(cache("filecontents"), String)
    ```

    ```csharp
    string fileContents = cache["filecontents"] as string;
    ```

5. 次のコードを追加して、`filecontents` という名前のキャッシュエントリが存在するかどうかを確認します。

    ```vb
    If fileContents Is Nothing Then

    End If
    ```

    ```csharp
    if (fileContents == null)
    {

    }
    ```

     指定したキャッシュエントリが存在しない場合は、テキストファイルを読み取り、キャッシュエントリとしてキャッシュに追加する必要があります。

6. `if/then` ブロックに次のコードを追加して、10秒後にキャッシュエントリの有効期限が切れることを指定する新しい <xref:System.Runtime.Caching.CacheItemPolicy> オブジェクトを作成します。

    ```vb
    Dim policy As New CacheItemPolicy()
    policy.AbsoluteExpiration = DateTimeOffset.Now.AddSeconds(10.0)
    ```

    ```csharp
    CacheItemPolicy policy = new CacheItemPolicy();
    policy.AbsoluteExpiration = DateTimeOffset.Now.AddSeconds(10.0);
    ```

     削除または有効期限の情報が指定されていない場合、既定値は <xref:System.Runtime.Caching.ObjectCache.InfiniteAbsoluteExpiration>です。これは、キャッシュエントリの有効期限が絶対時刻に限定されないことを意味します。 代わりに、メモリが不足している場合にのみ、キャッシュエントリの有効期限が切れます。 ベストプラクティスとして、絶対有効期限またはスライド式有効期限を常に明示的に指定することをお勧めします。

7. `if/then` ブロック内で、前の手順で追加したコードの後に次のコードを追加して、監視するファイルパスのコレクションを作成し、テキストファイルのパスをコレクションに追加します。

    ```vb
    Dim filePaths As New List(Of String)()
    filePaths.Add("c:\cache\cacheText.txt")
    ```

    ```csharp
    List<string> filePaths = new List<string>();
    filePaths.Add("c:\\cache\\cacheText.txt");
    ```

    > [!NOTE]
    > 使用するテキストファイルが `c:\cache\cacheText.txt`ない場合は、使用するテキストファイルのパスを指定します。

8. 前の手順で追加したコードの後に、次のコードを追加して、キャッシュエントリの変更モニターのコレクションに新しい <xref:System.Runtime.Caching.HostFileChangeMonitor> オブジェクトを追加します。

    ```vb
    policy.ChangeMonitors.Add(New HostFileChangeMonitor(filePaths))
    ```

    ```csharp
    policy.ChangeMonitors.Add(new HostFileChangeMonitor(filePaths));
    ```

     <xref:System.Runtime.Caching.HostFileChangeMonitor> オブジェクトは、テキストファイルのパスを監視し、変更が発生した場合はキャッシュに通知します。 この例では、ファイルの内容が変更されると、キャッシュエントリの有効期限が切れます。

9. 前の手順で追加したコードの後に、テキストファイルの内容を読み取る次のコードを追加します。

    ```vb
    fileContents = File.ReadAllText("c:\cache\cacheText.txt") & vbCrLf & DateTime.Now.ToString()
    ```

    ```csharp
    fileContents = File.ReadAllText("c:\\cache\\cacheText.txt") + "\n" + DateTime.Now;
    ```

     日付と時刻のタイムスタンプが追加されるので、キャッシュエントリの有効期限が切れるタイミングを確認することができます。

10. 前の手順で追加したコードの後に、次のコードを追加して、ファイルの内容を <xref:System.Runtime.Caching.CacheItem> インスタンスとしてキャッシュオブジェクトに挿入します。

    ```vb
    cache.Set("filecontents", fileContents, policy)
    ```

    ```csharp
    cache.Set("filecontents", fileContents, policy);
    ```

     前の手順で作成した <xref:System.Runtime.Caching.CacheItemPolicy> オブジェクトをパラメーターとして渡すことによって、キャッシュエントリを削除する方法に関する情報を指定します。

11. `if/then` ブロックの後に、キャッシュされたファイルの内容をメッセージボックスに表示する次のコードを追加します。

    ```vb
    MessageBox.Show(fileContents)
    ```

    ```csharp
    MessageBox.Show(fileContents);
    ```

12. **[ビルド]** メニューの **[WPFCaching のビルド]** をクリックして、プロジェクトをビルドします。

## <a name="testing-caching-in-the-wpf-application"></a>WPF アプリケーションでのキャッシュのテスト
 これで、アプリケーションをテストすることができます。

#### <a name="to-test-caching-in-the-wpf-application"></a>WPF アプリケーションでキャッシュをテストするには

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

     [`MainWindow`] ウィンドウが表示されます。

2. **[キャッシュの取得]** をクリックします。

     テキストファイルからキャッシュされたコンテンツがメッセージボックスに表示されます。 ファイルのタイムスタンプに注意してください。

3. メッセージボックスを閉じ、 **[キャッシュの取得]** をもう一度クリックします。

     タイムスタンプは変更されません。 これは、キャッシュされたコンテンツが表示されることを示します。

4. 10秒以上待ってから、 **[キャッシュの取得]** をもう一度クリックします。

     今回は、新しいタイムスタンプが表示されます。 これは、ポリシーによってキャッシュエントリの有効期限が切れ、キャッシュされた新しいコンテンツが表示されることを示します。

5. テキストエディターで、作成したテキストファイルを開きます。 まだ変更を加えないでください。

6. メッセージボックスを閉じ、 **[キャッシュの取得]** をもう一度クリックします。

     タイムスタンプに再び注目します。

7. テキストファイルに変更を加え、ファイルを保存します。

8. メッセージボックスを閉じ、 **[キャッシュの取得]** をもう一度クリックします。

     このメッセージボックスには、テキストファイルから更新されたコンテンツと新しいタイムスタンプが含まれています。 これは、絶対タイムアウト期間が経過していないにもかかわらず、ファイルを変更した直後にホストファイルの変更モニターによってキャッシュエントリが削除されたことを示します。

    > [!NOTE]
    > ファイルに変更を加える時間を増やすために、削除時間を20秒以上に増やすことができます。

## <a name="code-example"></a>コード例
 このチュートリアルを完了すると、作成したプロジェクトのコードは次の例のようになります。

 [!code-csharp[CachingWPFApplications#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CachingWPFApplications/CSharp/MainWindow.xaml.cs#1)]
 [!code-vb[CachingWPFApplications#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CachingWPFApplications/VisualBasic/MainWindow.xaml.vb#1)]

## <a name="see-also"></a>参照

- <xref:System.Runtime.Caching.MemoryCache>
- <xref:System.Runtime.Caching.ObjectCache>
- <xref:System.Runtime.Caching>
- [.NET Framework アプリケーションでのキャッシュ](../../performance/caching-in-net-framework-applications.md)
