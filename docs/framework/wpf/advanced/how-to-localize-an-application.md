---
title: '方法 : アプリケーションをローカライズする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- localization [WPF], applications
- LocBaml tool [WPF]
- applications [WPF], localizing
ms.assetid: 5001227e-9326-48a4-9dcd-ba1b89ee6653
ms.openlocfilehash: 7e034e92e1ff2b9bec0eaf8e0f3330f7a832a7e5
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095167"
---
# <a name="how-to-localize-an-application"></a>方法 : アプリケーションをローカライズする
このチュートリアルでは、LocBaml ツールを使用して、ローカライズされたアプリケーションを作成する方法について説明します。  
  
> [!NOTE]
> LocBaml ツールは、実稼働可能なアプリケーションではありません。 それはローカリゼーション API の一部を使用するサンプルとして提供されており、ローカリゼーション ツールを記述する方法を例示します。  
  
<a name="Introduction"></a>   
## <a name="overview"></a>概要  
 この説明では、アプリケーションのローカリゼーションの手順を段階を追って示します。 最初に、翻訳されるテキストを抽出できるようにアプリケーションを準備します。 テキストの翻訳後、翻訳されたテキストを元のアプリケーションの新しいコピーにマージします。  
  
<a name="Requirements"></a>   
## <a name="requirements"></a>必要条件  
 この説明では、コマンドラインから実行するコンパイラである Microsoft build engine (MSBuild) を使用します。  
  
 また、プロジェクト ファイルを使用するよう指示されます。 MSBuild とプロジェクトファイルの使用方法については、「[ビルドと配置](../app-development/building-and-deploying-wpf-applications.md)」を参照してください。  
  
 この説明のすべての例では、カルチャとして en-US (英語-米国) を使用します。 別の言語をインストールしなくても、この例の手順全体の作業を行えます。  
  
<a name="create_sample_app"></a>   
## <a name="create-a-sample-application"></a>サンプルのアプリケーションの作成  
 このステップでは、ローカリゼーション用のアプリケーションを準備します。 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] のサンプルでは、この説明のコード サンプルで使用される HelloApp のサンプルが提供されています。 このサンプルを使用する場合は、 [LocBaml ツールサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Tools/LocBaml)から [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイルをダウンロードしてください。  
  
1. ローカリゼーションを開始するポイントまで、アプリケーションを開発します。  
  
2. プロジェクトファイルで開発言語を指定して、MSBuild がニュートラル言語リソースを格納するメインアセンブリとサテライトアセンブリ (.resources 拡張子を持つファイル) を生成するようにします。 HelloApp サンプルのプロジェクト ファイルは HelloApp.csproj です。 このファイルに、以下のように特定される開発言語があります。  
  
     `<UICulture>en-US</UICulture>`  
  
3. [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルに UID を追加します。 UID は、ファイルへの変更を追跡して、翻訳する必要がある項目を識別するために使用されます。 ファイルに Uid を追加するには、プロジェクトファイルで**updateuid**を実行します。  
  
     **msbuild-t:updateuid helloapp.resources.dll**  
  
     不足している Uid または重複する Uid がないことを確認するには、 **checkuid**を実行します。  
  
     **msbuild-helloapp.resources.dll checkuid**  
  
     **Updateuid**を実行した後、ファイルには uid が含まれている必要があります。 たとえば、HelloApp の Pane1.xaml ファイルに、以下の内容があるはずです。  
  
     `<StackPanel x:Uid="StackPanel_1">`  
  
     `<TextBlock x:Uid="TextBlock_1">Hello World</TextBlock>`  
  
     `<TextBlock x:Uid="TextBlock_2">Goodbye World</TextBlock>`  
  
     `</StackPanel>`  
  
<a name="create_dll"></a>   
## <a name="create-the-neutral-language-resources-satellite-assembly"></a>ニュートラル言語リソースのサテライト アセンブリを作成する  
 ニュートラル言語リソースのサテライト アセンブリを生成するようにアプリケーションを構成した後、アプリケーションをビルドします。 これにより、メイン アプリケーション アセンブリだけでなく、ローカリゼーションで LocBaml が必要とするニュートラル言語リソースのサテライト アセンブリが生成されます。 アプリケーションをビルドするには:  
  
1. Helloapp.resources.dll をコンパイルして、ダイナミックリンクライブラリ (DLL) を作成します。  
  
     **msbuild helloapp.csproj**  
  
2. 新規作成されたメインのアプリケーション アセンブリ HelloApp.exe は、次のフォルダーに作成されます。  
  
     `C:\HelloApp\Bin\Debug\`  
  
3. 新規作成されたニュートラル言語リソースのサテライト アセンブリ HelloApp.resources.dll は次のフォルダーに作成されます。  
  
     `C:\HelloApp\Bin\Debug\en-US\`  
  
<a name="build_locbaml"></a>   
## <a name="build-the-locbaml-tool"></a>LocBaml ツールをビルドする  
  
1. LocBaml のビルドに必要なすべてのファイルは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] サンプルに配置されています。 C# [LocBaml ツールサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Tools/LocBaml)からファイルをダウンロードします。  
  
2. ツールをビルドするには、コマンド ラインでプロジェクト ファイル (locbaml.csproj) を実行します。  
  
     **msbuild locbaml.csproj**  
  
3. 新しく作成された実行可能ファイル (locbaml.exe) を検索するには、bin \release ディレクトリに移動します。 例: C:\LocBaml\Bin\Release\locbaml.exe  
  
4. LocBaml を実行するときに指定できるオプションは、次のとおりです。  
  
    - **parse**または **-p:** Baml、リソース、または DLL ファイルを解析して、.csv または .txt ファイルを生成します。  
  
    - **generate**または **-g:** 翻訳されたファイルを使用して、ローカライズされたバイナリファイルを生成します。  
  
    - **out**または **-o** {*filedirectory*] **:** 出力ファイル名。  
  
    - **culture**または **-cul** {*culture*] **:** 出力アセンブリのロケール。  
  
    - **translation**または **-trans** {*translation. .csv*] **:** 翻訳またはローカライズされたファイル。  
  
    - **asmpath**または **-asmpath:** {*filedirectory*] **:** [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] コードにカスタムコントロールが含まれている場合は、カスタムコントロールアセンブリに**asmpath**を指定する必要があります。  
  
    - **nologo:** ロゴまたは著作権情報は表示されません。  
  
    - **verbose:** 詳細モードの情報が表示されます。  
  
    > [!NOTE]
    > ツールの実行時にオプションの一覧が必要な場合は、「 **LocBaml** 」と入力し、enter キーを押します。  
  
<a name="parse_dll"></a>   
## <a name="use-locbaml-to-parse-a-file"></a>LocBaml を使用してファイルを解析する  
 LocBaml ツールが作成されたので、これを使用して HelloApp.resources.dll を解析し、ローカライズするテキスト コンテンツを抽出できる状態になりました。  
  
1. LocBaml.exe を、メインのアプリケーション アセンブリが作成された、アプリケーションの bin \debug フォルダーにコピーします。  
  
2. サテライト アセンブリ ファイルを解析して、.csv ファイルとして出力を格納するには、次のコマンドを使用します。  
  
     **LocBaml.exe /parse HelloApp.resources.dll /out:Hello.csv**  
  
    > [!NOTE]
    > 入力ファイル HelloApp.resources.dll が LocBaml.exe と同じディレクトリに存在しない場合は、いずれかのファイルを移動して両方のファイルが同じディレクトリにあるようにします。  
  
3. LocBaml を実行してファイルを解析する際、出力はコンマ区切り (.csv ファイル) またはタブ区切り (.txt ファイル) された 7 つのフィールドで構成されます。 HelloApp.resources.dll の解析済みの .csv ファイルを次に示します。

   | |
   |-|
   |HelloApp.g.en-US.resources:window1.baml,Stack1:System.Windows.Controls.StackPanel.$Content,Ignore,FALSE, FALSE,,#Text1;#Text2;|
   |HelloApp.g.en-US.resources:window1.baml,Text1:System.Windows.Controls.TextBlock.$Content,None,TRUE, TRUE,,Hello World|
   |HelloApp.g.en-US.resources:window1.baml,Text2:System.Windows.Controls.TextBlock.$Content,None,TRUE, TRUE,,Goodbye World|

   7 つのフィールドは、次のとおりです。  
  
   1. **BAML 名**。 ソース言語のサテライト アセンブリに関する BAML リソースの名前。  
  
   2. **リソース キー**。 ローカライズされたリソースの識別子。  
  
   3. **[カテゴリ]** 。 値の型です。 「[ローカリゼーション属性とコメント」を](localization-attributes-and-comments.md)参照してください。  
  
   4. **読みやすさ**。 ローカライザーによって値が読み取れるかどうか。 「[ローカリゼーション属性とコメント」を](localization-attributes-and-comments.md)参照してください。  
  
   5. **変更可能性**。 ローカライザーによって値が変更できるかどうか。 「[ローカリゼーション属性とコメント」を](localization-attributes-and-comments.md)参照してください。  
  
   6. **コメント**。 値をローカライズする方法を決定するための値の追加の説明。 「[ローカリゼーション属性とコメント」を](localization-attributes-and-comments.md)参照してください。  
  
   7. **値**。 目的のカルチャに翻訳するテキストの値。  
  
   次の表は、.csv ファイルの区切り記号付きの値にこれらのフィールドをマップする方法を示しています。  
  
   |BAML 名|リソース キー|カテゴリ|読みやすさ|変更可能性|説明|値|  
   |---------------|------------------|--------------|-----------------|-------------------|--------------|-----------|
   |HelloApp.g.en-US.resources:window1.baml|Stack1:System.Windows.Controls.StackPanel.$Content|Ignore|FALSE|FALSE||#Text1;#Text2|
   |HelloApp.g.en-US.resources:window1.baml|Stack1:System.Windows.Controls.StackPanel.$Content|なし|TRUE|TRUE||Hello World|
   |HelloApp.g.en-US.resources:window1.baml|Stack1:System.Windows.Controls.StackPanel.$Content|なし|TRUE|TRUE||Goodbye World|
  
   **[コメント]** フィールドのすべての値に値が含まれていないことに注意してください。フィールドに値がない場合は、空になります。 また、最初の行の項目は読み取りも変更もできず、**カテゴリ**値として "Ignore" が含まれていることにも注意してください。これらはすべて、値がローカライズ可能ではないことを示します。  
  
4. 解析されたファイル内のローカライズ可能な項目の検出を容易にするため (特に大規模なファイルの場合)、**カテゴリ**、**読みやすさ**、**変更可能性**で項目を並べ替えたりフィルター処理したりすることができます。 たとえば、読み取りも変更もできない値をフィルター処理することができます。  
  
<a name="translate_loc_content"></a>   
## <a name="translate-the-localizable-content"></a>ローカライズ可能なコンテンツを翻訳する  
 抽出されたコンテンツを翻訳するために使用可能な任意のツールを使用します。 これを行うには、リソースを .csv ファイルに書き込み、Microsoft Excel で表示して、最後の列 (値) に変更を加える方法が適しています。  
  
<a name="merge_translations"></a>   
## <a name="use-locbaml-to-generate-a-new-resourcesdll-file"></a>LocBaml を使用して新しい .resources.dll ファイルを生成する  
 LocBaml で HelloApp.resources.dll を解析して識別されたコンテンツは翻訳済みであり、元のアプリケーションにマージする必要があります。 **Generate**または **-g**オプションを使用して、新しい .resources .dll ファイルを生成します。  
  
1. 新しい HelloApp.resources.dll ファイルを生成するには、次の構文を使用します。 カルチャを en-US (/cul:en-US) としてマークします。  
  
     **LocBaml/generate Helloapp.resources.dll/trans: Hello. .csv/out: c:\/cul: en-US.**  
  
    > [!NOTE]
    > 入力ファイル Hello.csv が実行可能ファイル LocBaml.exe と同じディレクトリに存在しない場合は、いずれかのファイルを移動して、両方のファイルが同じディレクトリにあるようにします。  
  
2. C:\HelloApp\Bin\Debug\en-US\HelloApp.resources.dll ディレクトリにある古い HelloApp.resources.dll ファイルを新規作成した HelloApp.resources.dll ファイルに置き換えます。  
  
3. ここで "Hello World" と "Goodbye World" をアプリケーション内で翻訳する必要があります。  
  
4. 別のカルチャに翻訳するには、翻訳先の言語のカルチャを使用します。 フランス語 (カナダ) に翻訳する方法の例を次に示します。  
  
     **LocBaml.exe /generate HelloApp.resources.dll /trans:Hellofr-CA.csv /out:c:\ /cul:fr-CA**  
  
5. メイン アプリケーション アセンブリと同じアセンブリで、新しいサテライト アセンブリを格納するために、新しいカルチャ固有のフォルダーを作成します。 フランス語 (カナダ) のフォルダーは "fr-CA" となります。  
  
6. 新しいフォルダーに生成されたサテライト アセンブリをコピーします。  
  
7. 新しいサテライト アセンブリをテストするには、アプリケーションが実行するカルチャを変更する必要があります。 これは、次の 2 つの方法のいずれかで行うことができます。  
  
    - オペレーティングシステムの地域設定を変更します ([**スタート** &#124; **] コントロールパネル** &#124;の **[地域と言語] オプション**)。  
  
    - アプリケーションで、次のコードを App.xaml.cs に追加します。  
  
   [!code-xaml[LocBamlChangeCultureSnippets#LocBamlChangeCultureMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/LocBamlChangeCultureSnippets/CSharp/App.xaml#locbamlchangeculturemarkup)]
   [!code-csharp[LocBamlChangeCultureSnippets#LocBamlChangeCultureCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/LocBamlChangeCultureSnippets/CSharp/App.xaml.cs#locbamlchangeculturecodebehind)]
   [!code-vb[LocBamlChangeCultureSnippets#LocBamlChangeCultureCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/LocBamlChangeCultureSnippets/VisualBasic/Application.xaml.vb#locbamlchangeculturecodebehind)]  
  
<a name="Some_Tips_for_Using_LocBaml"></a>   
## <a name="some-tips-for-using-locbaml"></a>LocBaml を使用するためのヒント  
  
- カスタム コントロールを定義するすべての依存アセンブリを LocBaml のローカル ディレクトリにコピーするか、GAC にインストールする必要があります。 これは、ローカライズ API がバイナリ XAML (BAML) を読み取るときに、依存アセンブリにアクセスできる必要があるためです。  
  
- メインのアセンブリが署名済みの場合は、生成されたリソース DLL も読み込むために署名されている必要があります。  
  
- ローカライズされたリソースの DLL は、メインのアセンブリと同期する必要があります。  
  
<a name="Whats_Next"></a>   
## <a name="whats-next"></a>参照トピック  
 これで、LocBaml ツールの使用方法に関する基本的な知識が得られました。  UID を含むファイルを作成できるようになりました。 LocBaml ツールを使用することで、ローカライズ可能なコンテンツを抽出するファイルを解析できます。コンテンツを翻訳すると、翻訳済みのコンテンツをマージする .resources.dll ファイルを生成できます。 このトピックには、可能性のあるすべての詳細情報は含まれていませんが、LocBaml を使用してアプリケーションをローカライズするために必要な知識は得られました。  
  
## <a name="see-also"></a>参照

- [WPF のグローバリゼーション](globalization-for-wpf.md)
- [自動レイアウトの使用の概要](use-automatic-layout-overview.md)
