---
title: 'チュートリアル : ハイブリッド アプリケーションのローカライズ'
ms.date: 08/18/2018
helpviewer_keywords:
- localization [WPF interoperability]
- hybrid applications [WPF interoperability]
ms.assetid: fbc0c54e-930a-4c13-8e9c-27b83665010a
ms.openlocfilehash: b406d539f2446824027e9462c8ecbe20c18cfb27
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794126"
---
# <a name="walkthrough-localizing-a-hybrid-application"></a>チュートリアル : ハイブリッド アプリケーションのローカライズ

このチュートリアルでは、Windows フォームベースのハイブリッドアプリケーションで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素をローカライズする方法について説明します。

このチュートリアルでは、以下のタスクを行います。

- Windows フォームホストプロジェクトを作成しています。

- ローカライズ可能なコンテンツの追加

- ローカライズを有効にします。

- リソース識別子を割り当てています。

- LocBaml ツールを使用してサテライトアセンブリを生成します。

このチュートリアルで示すタスクの完全なコード一覧については、「[ハイブリッドアプリケーションのローカライズのサンプル](https://go.microsoft.com/fwlink/?LinkID=160015)」を参照してください。

完了すると、ローカライズされたハイブリッドアプリケーションが完成します。

## <a name="prerequisites"></a>Prerequisites

このチュートリアルを実行するには、次のコンポーネントが必要です。

- Visual Studio 2017

## <a name="creating-the-windows-forms-host-project"></a>Windows フォームホストプロジェクトの作成

最初の手順として、Windows フォームアプリケーションプロジェクトを作成し、ローカライズするコンテンツを含む [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素を追加します。

### <a name="to-create-the-host-project"></a>ホストプロジェクトを作成するには

1. `LocalizingWpfInWf`という名前の**WPF アプリ**プロジェクトを作成します。  (**ファイル** > **新しい** > **プロジェクト** > **Visual C#** または**Visual Basic** > **クラシックデスクトップ** > **WPF アプリケーション**)。

2. `SimpleControl` という名前の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.UserControl> 要素をプロジェクトに追加します。

3. フォームに `SimpleControl` 要素を配置するには、<xref:System.Windows.Forms.Integration.ElementHost> コントロールを使用します。 詳細については、「[チュートリアル: Windows フォームでの 3-D WPF 複合コントロールのホスト](walkthrough-hosting-a-3-d-wpf-composite-control-in-windows-forms.md)」を参照してください。

## <a name="adding-localizable-content"></a>ローカライズ可能なコンテンツの追加

次に、Windows フォーム label コントロールを追加し、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素のコンテンツをローカライズ可能な文字列に設定します。

### <a name="to-add-localizable-content"></a>ローカライズ可能なコンテンツを追加するには

1. **ソリューションエクスプローラー**で、 **[simplecontrol .xaml]** をダブルクリックして、WPF デザイナーで開きます。

2. 次のコードを使用して、<xref:System.Windows.Controls.Button> コントロールの内容を設定します。

     [!code-xaml[LocalizingWpfInWf#10](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizingWpfInWf/CSharp/SimpleControl0.xaml#10)]

3. **ソリューションエクスプローラー**で、 **Form1**をダブルクリックして Windows フォームデザイナーで開きます。

4. **ツールボックス**を開き、 **[ラベル]** をダブルクリックして、フォームにラベルコントロールを追加します。 <xref:System.Windows.Forms.Control.Text%2A> プロパティの値を `"Hello"` に設定します。

5. **F5** キーを押してアプリケーションをビルドし、実行します。

     `SimpleControl` 要素と label コントロールの両方に **"Hello"** というテキストが表示されます。

## <a name="enabling-localization"></a>ローカライズの有効化

Windows フォームデザイナーには、サテライトアセンブリでローカライズを有効にするための設定が用意されています。

### <a name="to-enable-localization"></a>ローカライズを有効にするには

1. **ソリューションエクスプローラー**で、 **[Form1.cs]** をダブルクリックして、Windows フォームデザイナーで開きます。

2. **[プロパティ]** ウィンドウで、フォームの**ローカライズ**可能なプロパティの値を `true`に設定します。

3. **[プロパティ]** ウィンドウで、 **[言語]** プロパティの値を**スペイン語 (スペイン)** に設定します。

4. Windows フォームデザイナーで、ラベル コントロールを選択します。

5. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.Text%2A>] プロパティの値を `"Hola"`に設定します。

     Form1.es という名前の新しいリソースファイルがプロジェクトに追加されます。

6. **ソリューションエクスプローラー**で、 **[Form1.cs]** を右クリックし、 **[コードの表示]** をクリックしてコードエディターで開きます。

7. `InitializeComponent`への呼び出しの前に、次のコードを `Form1` コンストラクターにコピーします。

     [!code-csharp[LocalizingWpfInWf#2](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizingWpfInWf/CSharp/Form1.cs#2)]

8. **ソリューションエクスプローラー**で、 **[Localizingwpfinwf]** を右クリックし、 **[プロジェクトのアンロード]** をクリックします。

     プロジェクト名に " **(利用不可)** " というラベルが付いています。

9. **[Localizingwpfinwf]** を右クリックし、 **[Edit localizingwpfinwf]** をクリックします。

     コードエディターでプロジェクトファイルが開きます。

10. 次の行をプロジェクトファイルの最初の `PropertyGroup` にコピーします。

    ```xml
    <UICulture>en-US</UICulture>
    ```

11. プロジェクトファイルを保存して閉じます。

12. **ソリューションエクスプローラー**で、 **[Localizingwpfinwf]** を右クリックし、 **[プロジェクトの再読み込み]** をクリックします。

## <a name="assigning-resource-identifiers"></a>リソース識別子の割り当て

リソース識別子を使用して、ローカライズ可能なコンテンツをリソースアセンブリにマップできます。 `updateuid` オプションを指定すると、Msbuild.exe アプリケーションによってリソース識別子が自動的に割り当てられます。

### <a name="to-assign-resource-identifiers"></a>リソース識別子を割り当てるには

1. [スタート] メニューから、Visual Studio の開発者コマンドプロンプトを開きます。

2. 次のコマンドを使用して、ローカライズ可能なコンテンツにリソース識別子を割り当てます。

    ```console
    msbuild -t:updateuid LocalizingWpfInWf.csproj
    ```

3. **ソリューションエクスプローラー**で、 **[simplecontrol .xaml]** をダブルクリックしてコードエディターで開きます。 `msbuild` コマンドによって、`Uid` 属性がすべての要素に追加されていることがわかります。 これにより、リソース識別子の割り当てによってローカライズが容易になります。

     [!code-xaml[LocalizingWpfInWf#20](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizingWpfInWf/CSharp/SimpleControl.xaml#20)]

4. **F6**キーを押してソリューションをビルドします。

## <a name="using-locbaml-to-produce-a-satellite-assembly"></a>LocBaml を使用してサテライトアセンブリを生成する

ローカライズされたコンテンツは、リソースのみの*サテライトアセンブリ*に格納されます。 コマンドラインツールの LocBaml を使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コンテンツ用のローカライズされたアセンブリを生成します。

### <a name="to-produce-a-satellite-assembly"></a>サテライトアセンブリを生成するには

1. LocBaml をプロジェクトの obj\Debug フォルダーにコピーします。 詳細については、「[アプリケーションをローカライズする](how-to-localize-an-application.md)」を参照してください。

2. コマンドプロンプトウィンドウで、次のコマンドを使用して、リソース文字列を一時ファイルに抽出します。

    ```console
    LocBaml /parse LocalizingWpfInWf.g.en-US.resources /out:temp.csv
    ```

3. Visual Studio または他のテキストエディターを使用して、一時 .csv ファイルを開きます。 文字列 `"Hello"` をスペイン語翻訳、`"Hola"`に置き換えます。

4. 一時 .csv ファイルを保存します。

5. 次のコマンドを使用して、ローカライズされたリソースファイルを生成します。

    ```console
    LocBaml /generate /trans:temp.csv LocalizingWpfInWf.g.en-US.resources /out:. /cul:es-ES
    ```

     LocalizingWpfInWf.g.es ファイルが obj\Debug フォルダーに作成されます。

6. 次のコマンドを使用して、ローカライズされたサテライトアセンブリをビルドします。

    ```console
    Al.exe /out:LocalizingWpfInWf.resources.dll /culture:es-ES /embed:LocalizingWpfInWf.Form1.es-ES.resources /embed:LocalizingWpfInWf.g.es-ES.resources
    ```

     Obj\Debug フォルダーに、LocalizingWpfInWf .dll ファイルが作成されます。

7. プロジェクトの bin\Debug\es-ES フォルダーに、LocalizingWpfInWf ファイルをコピーします。 既存のファイルを置き換えます。

8. プロジェクトの bin\Debug フォルダーにある LocalizingWpfInWf を実行します。 アプリケーションをリビルドしないでください。または、サテライトアセンブリが上書きされます。

     アプリケーションでは、英語の文字列ではなく、ローカライズされた文字列が表示されます。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [アプリケーションをローカライズする](how-to-localize-an-application.md)
- [チュートリアル: Windows フォームのローカライズ](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/y99d1cd3(v=vs.100))
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
