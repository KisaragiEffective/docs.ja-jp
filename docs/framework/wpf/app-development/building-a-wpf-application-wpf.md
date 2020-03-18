---
title: アプリをコンパイルする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WPF application [WPF], building
ms.assetid: a58696fd-bdad-4b55-9759-136dfdf8b91c
ms.openlocfilehash: 00c76dfcdcedc7ceaefaaae785368f8b343457a7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744767"
---
# <a name="compile-a-wpf-application"></a>WPF アプリケーションのコンパイル

Windows Presentation Foundation (WPF) アプリケーションは、.NET Framework の実行可能ファイル (.exe)、ライブラリ (.dll)、または両方の種類のアセンブリの組み合わせとしてビルドできます。 このトピックでは、WPF アプリケーションをビルドする方法を紹介し、ビルドプロセスの主要な手順について説明します。

<a name="Building_a_WPF_Application_using_Command_Line"></a>

## <a name="building-a-wpf-application"></a>WPF アプリケーションのビルド

WPF アプリケーションは、次の方法でコンパイルできます。

- コマンド ライン。 アプリケーションには、コード (XAML ではない) とアプリケーション定義ファイルだけを含める必要があります。 詳細については、「[csc.exe を使用したコマンド ラインからのビルド](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)」または「[コマンド ラインからのビルド (Visual Basic)](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md)」を参照してください。

- Microsoft Build Engine (MSBuild)。 コードと XAML ファイルに加えて、アプリケーションには MSBuild プロジェクト ファイルを含める必要があります。 詳細については、「MSBuild」を参照してください。

- 見ることができます。 Visual Studio は、MSBuild を使用して WPF アプリケーションをコンパイルする統合開発環境であり、UI を作成するためのビジュアル デザイナーを含んでいます。 詳細については、「 [Visual studio を使用したコードの記述と管理](/visualstudio/ide/index-writing-code)」および「 [visual STUDIO での XAML のデザイン](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)」を参照してください。

<a name="The_Windows_Presentation_Foundation_Build_Pipeline"></a>

## <a name="wpf-build-pipeline"></a>WPF ビルド パイプライン

WPF プロジェクトをビルドすると、言語固有のターゲットと WPF 固有のターゲットの組み合わせが呼び出されます。 これらのターゲットを実行するプロセスはビルド パイプラインと呼ばれます。主要な手順を次の図に示します。

![WPF のビルドプロセス](./media/wpfbuildsystem-figure1.png "WPFBuildSystem_Figure1")

<a name="Pre_Build_Initializations"></a>

### <a name="pre-build-initializations"></a>ビルド前の初期化

ビルドする前に、MSBuild は次のような重要なツールとライブラリの場所を決定します。

- .NET Framework。

- Windows SDK ディレクトリ。

- WPF 参照アセンブリの場所。

- アセンブリ検索パスのプロパティ。

MSBuild がアセンブリを検索する最初の場所は、参照アセンブリディレクトリ (%ProgramFiles%\Reference Assemblies\Microsoft\Framework\v3.0\\) です。 この手順では、ビルド プロセスはさまざまなプロパティと項目グループを初期化し、必要なクリーンアップ作業も実行します。

<a name="Resolving_references"></a>

### <a name="resolving-references"></a>参照の解決

ビルド プロセスは、アプリケーション プロジェクトのビルドに必要なアセンブリを探して、バインドします。 このロジックは、`ResolveAssemblyReference` タスクに含まれます。 プロジェクト ファイル内で `Reference` として宣言されたすべてのアセンブリは、検索パスに関する情報と、すでにシステムにインストールされているアセンブリのメタデータと共にタスクに渡されます。 このタスクは、アセンブリを検索し、インストールされているアセンブリのメタデータを使用して、出力マニフェストに表示する必要がないコア WPF アセンブリをフィルターで除外します。 これは、ClickOnce マニフェストに冗長な情報が含まれるのを避けるために行われます。 たとえば、プレゼンテーションフレームワーク .dll は、および WPF 用に構築されたアプリケーションの代表者と見なすことができます。また、.NET Framework がインストールされているすべてのコンピューター上のすべての WPF アセンブリが同じ場所に存在するため、すべてを含める必要はありません。マニフェスト内のすべての .NET Framework 参照アセンブリに関する情報。

<a name="Markup_Compilation___Pass_1"></a>

### <a name="markup-compilationpass-1"></a>マークアップ コンパイル - パス 1

この手順では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルを解析してコンパイルし、ランタイムが XML の解析とプロパティ値の検証に時間を費やすことがないようにします。 コンパイルされた [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルを事前にトークン化するため、実行時の読み込みは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルを読み込むよりもはるかに短時間で終わります。

この手順では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ビルド項目である `Page` ファイルごとに、次のアクティビティが実行されます。

1. [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルがマークアップ コンパイラによって解析されます。

2. [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 用にコンパイル済みの表現が作成されて、obj\Release フォルダーにコピーされます。

3. 新しい部分クラスの CodeDOM 表現が作成され、obj\Release フォルダーにコピーされます。

さらに、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルごとに、言語固有のコード ファイルが生成されます。 たとえば、Visual Basic プロジェクトの Page1 ページの場合は、Page1 が生成されます。C#プロジェクトの Page1 ページの場合、Page1.g.cs が生成されます。 ファイル名の ".g" は、ファイルが生成されたコードであり、マークアップ ファイルのトップレベルの要素 (`Page` や `Window` など) に対する部分クラス宣言を持つことを示しています。 クラスは、(Visual Basic で`Extends`) のC# `partial` 修飾子を使用して宣言し、他の場所 (通常は分離コードファイル Page1.xaml.cs) に別の宣言があることを示します。

部分クラスは、適切な基本クラス (ページの <xref:System.Windows.Controls.Page> など) から拡張され、<xref:System.Windows.Markup.IComponentConnector?displayProperty=nameWithType> インターフェイスを実装します。 <xref:System.Windows.Markup.IComponentConnector> インターフェイスには、コンポーネントを初期化し、そのコンテンツ内の要素の名前とイベントを接続するメソッドがあります。 その結果、生成されたコード ファイルには、次のようなメソッドの実装が含まれます。

```csharp
public void InitializeComponent() {
    if (_contentLoaded) {
        return;
    }
    _contentLoaded = true;
    System.Uri resourceLocater =
        new System.Uri(
            "window1.xaml",
            System.UriKind.RelativeOrAbsolute);
    System.Windows.Application.LoadComponent(this, resourceLocater);
}
```

```vb
Public Sub InitializeComponent() _

    If _contentLoaded Then
        Return
    End If

    _contentLoaded = True
    Dim resourceLocater As System.Uri = _
        New System.Uri("mainwindow.xaml", System.UriKind.Relative)

    System.Windows.Application.LoadComponent(Me, resourceLocater)

End Sub
```

既定では、マークアップコンパイルは MSBuild エンジンと同じ <xref:System.AppDomain> で実行されます。 これにより、パフォーマンスが大幅に向上します。 この動作は、`AlwaysCompileMarkupFilesInSeparateDomain` プロパティで切り替えることができます。 これには、個別の <xref:System.AppDomain>をアンロードすることによって、すべての参照アセンブリをアンロードするという利点があります。

<a name="Pass_2_of_Markup_Compilation"></a>

### <a name="markup-compilationpass-2"></a>マークアップ コンパイル - パス 2

マークアップ コンパイルのパス 1 ですべての [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページがコンパイルされるわけではありません。 ローカルで定義された型参照 (同じプロジェクト内の他のコードで定義された型の参照) を含む [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルは、この時点ではコンパイルから除外されます。 これは、ローカルで定義された型は、ソース内にのみ存在し、まだコンパイルされていないためです。 これを判別するために、パーサーは、マークアップ ファイル内で `x:Name` などの項目を検索するヒューリスティックを使用します。 このようなインスタンスが見つかると、そのマークアップ ファイルのコンパイルは、コード ファイルがコンパイルされるまで延期され、その後、2 階目のマークアップ コンパイル パスで、これらのファイルが処理されます。

<a name="File_Classification"></a>

### <a name="file-classification"></a>ファイルの分類

ビルド プロセスは、配置されるアプリケーション アセンブリに基づいて、出力ファイルを異なるリソース グループに分けます。 一般的なローカライズされないアプリケーションでは、`Resource` としてマークされたすべてのデータ ファイルは、メイン アセンブリ (実行可能ファイルまたはライブラリ) に配置されます。 プロジェクトで `UICulture` が設定されると、コンパイル済みのすべての [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルと、言語固有として明示的にマークされたリソースは、サテライト リソース アセンブリに配置されます。 さらに、言語に依存しないすべてのリソースは、メイン アセンブリに配置されます。 ビルド プロセスのこの手順で、この決定が行われます。

プロジェクト ファイル内の `ApplicationDefinition`、`Page`、および `Resource` ビルト アクションは、`Localizable` メタデータで拡張でき (受け入れられる値は `true` と `false`)、これによって、ファイルが言語固有か、言語に依存しないかを指定します。

<a name="Core_Compilation"></a>

### <a name="core-compilation"></a>コア コンパイル

コア コンパイル手順では、コード ファイルのコンパイルが行われます。 これは Microsoft.CSharp.targets や Microsoft.VisualBasic.targets など、言語固有のターゲット ファイル内のロジックによって調整されます。 ヒューリスティックによってマークアップ コンパイラの 1 回のパスでは不十分であると判断されると、メイン アセンブリが生成されます。 ただし、プロジェクト内の 1 つ以上の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルにローカルで定義された型の参照が含まれている場合、一時的な .dll ファイルが生成され、これによってマークアップ コンパイルの 2 回目のパスが完了すると、最終的なアプリケーション アセンブリが作成されます。

<a name="Manifest_generation"></a>

### <a name="manifest-generation"></a>マニフェストの生成

ビルドプロセスの最後に、すべてのアプリケーションアセンブリとコンテンツファイルの準備が整ったら、アプリケーションの ClickOnce マニフェストが生成されます。

配置マニフェスト ファイルは、配置モデル (現在のバージョン、更新動作、およびパブリッシャーの ID とデジタル署名) を記述します。 このマニフェストは、配置を処理する管理者が作成します。 ファイル拡張子は、xbap (XAML ブラウザーアプリケーション (Xbap) 用) と、インストールされているアプリケーションのアプリケーションです。 前者は `HostInBrowser` プロジェクト プロパティによって指定されるため、マニフェストはアプリケーションがブラウザーによってホストされることを識別します。

アプリケーション マニフェスト (.exe.manifest ファイル) は、アプリケーション アセンブリと依存ライブラリを記述し、アプリケーションに必要なアクセス許可をリストします。 このファイルは、アプリケーション開発者が作成します。 ClickOnce アプリケーションを起動するために、ユーザーはアプリケーションの配置マニフェストファイルを開きます。

これらのマニフェストファイルは、常に Xbap 用に作成されます。 インストール型アプリケーションの場合、プロジェクト ファイル内で `GenerateManifests` プロパティの値が `true` に指定されない限り、作成されません。

Xbap は、一般的なインターネットゾーンのアプリケーションに割り当てられているアクセス許可に対して、<xref:System.Security.Permissions.WebBrowserPermission> と <xref:System.Security.Permissions.MediaPermission>の2つの追加のアクセス許可を取得します。 WPF ビルドシステムは、アプリケーションマニフェストでこれらのアクセス許可を宣言します。

<a name="Incremental_Build_Support"></a>

## <a name="incremental-build-support"></a>インクリメンタル ビルドのサポート

WPF ビルドシステムでは、インクリメンタルビルドがサポートされています。 これは、マークアップやコードに加えられた変更を検出し、変更の影響を受けるアイテムだけをコンパイルするという高度な機能です。 インクリメンタル ビルド メカニズムは、次のファイルを使用します。

- $(*AssemblyName*)_MarkupCompiler.Cache ファイル。コンパイラの現在の状態を保持します。

- $(*AssemblyName*)_MarkupCompiler.lref ファイル。ローカルで定義された型への参照を持つ [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルをキャッシュします。

インクリメンタル ビルドを制御する規則のセットを次に示します。

- ビルド システムが変更を検出する最小単位は、ファイルです。 そのため、コード ファイルの場合、ビルド システムは、型が変更されたかどうか、またはコードが追加されたかどうかを検出できません。 プロジェクト ファイルの場合も同様です。

- インクリメンタル ビルドのメカニズムは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページがクラスを定義するのか、他のクラスを使用するのかを認識できなければなりません。

- `Reference` エントリが変更された場合は、すべてのページを再コンパイルします。

- コード ファイルが変更された場合は、ローカルで定義された型の参照を含むすべてのページを再コンパイルします。

- [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルが変更された場合:

  - [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] がプロジェクトで `Page` として宣言されている場合: [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] にローカルで定義された型の参照が含まれていない場合は、その [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] とローカル参照を含むすべての [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページを再コンパイルします。[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] にローカル参照が含まれる場合は、ローカル参照が含まれているすべての [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページを再コンパイルします。

  - [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] がプロジェクトの `ApplicationDefinition` として宣言されている場合: すべての [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページを再コンパイルします (理由: 各 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] に、変更された可能性のある <xref:System.Windows.Application> 型への参照が含まれています)。

- プロジェクト ファイルがコード ファイルを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルではなくアプリケーション定義として宣言している場合:

  - プロジェクト ファイル内の `ApplicationClassName` 値が変更されたかどうか (新しいアプリケーションの種類があるかどうか) を確認します。 変更されていた場合は、アプリケーション全体を再コンパイルします。

  - 変更されていなかった場合は、ローカル参照を含んでいるすべての [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページを再コンパイルします。

- プロジェクト ファイルが変更された場合、上記のすべての規則を適用し、再コンパイルする必要があるものを確認します。 `AssemblyName`、`IntermediateOutputPath`、`RootNamespace`、および `HostInBrowser` プロパティが変更された場合は、再コンパイルが必要です。

次のような再コンパイルのシナリオが考えられます。

- アプリケーション全体が再コンパイルされる。

- ローカルで定義された方参照を含んでいる [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルのみが再コンパイルされる。

- 何も再コンパイルされない (プロジェクトに何も変更が加えられていない場合)。

## <a name="see-also"></a>参照

- [WPF アプリケーションの配置](deploying-a-wpf-application-wpf.md)
- [WPF MSBuild のリファレンス](/visualstudio/msbuild/wpf-msbuild-reference)
- [WPF におけるパッケージの URI](pack-uris-in-wpf.md)
- [WPF アプリケーションのリソース ファイル、コンテンツ ファイル、およびデータ ファイル](wpf-application-resource-content-and-data-files.md)
