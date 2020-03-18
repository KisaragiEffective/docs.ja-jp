---
title: マージされたリソース ディクショナリ
ms.date: 03/30/2017
helpviewer_keywords:
- merged resource dictionaries [WPF]
- dictionaries [WPF], merged resources
ms.assetid: d159531f-05d4-49fd-b951-c332de51e5bc
ms.openlocfilehash: 899955a9f3302e67de4efa0ce0cb6baf6bf0ec5d
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559782"
---
# <a name="merged-resource-dictionaries"></a>マージされたリソース ディクショナリ
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] リソースでは、マージされたリソース ディクショナリ機能がサポートされます。 この機能を使用すると、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションのリソース部分を、コンパイル済み [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] アプリケーションの外部で定義できます。 その後、リソースをアプリケーション間で共有できます。また、ローカライズ用に簡単に切り分けることができます。  
  
## <a name="introducing-a-merged-resource-dictionary"></a>マージされたリソース ディクショナリの導入  
 マークアップでは、マージされたリソース ディクショナリをページに導入するために次の構文を使います。  
  
 [!code-xaml[ResourceMergeDictionary#MergedXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourceMergeDictionary/CS/default.xaml#mergedxaml)]  
  
 <xref:System.Windows.ResourceDictionary> 要素には[X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)がないことに注意してください。これは、リソースコレクション内のすべての項目に対して通常必要となります。 ただし、<xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> コレクション内の別の <xref:System.Windows.ResourceDictionary> 参照は特殊なケースで、このマージされたリソースディクショナリのシナリオ用に予約されています。 マージされたリソースディクショナリを導入する <xref:System.Windows.ResourceDictionary> には、 [X:Key ディレクティブ](../../../desktop-wpf/xaml-services/xkey-directive.md)を含めることはできません。 通常、<xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> コレクション内の各 <xref:System.Windows.ResourceDictionary> は <xref:System.Windows.ResourceDictionary.Source%2A> 属性を指定します。 <xref:System.Windows.ResourceDictionary.Source%2A> の値は、マージされるリソースファイルの場所に解決される uniform resource identifier (URI) である必要があります。 その URI の送信先は、ルート要素として <xref:System.Windows.ResourceDictionary> を持つ別の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルである必要があります。  
  
> [!NOTE]
> <xref:System.Windows.ResourceDictionary.Source%2A>を指定する代わりとして、または指定したソースに含まれるすべてのリソースに加えて、マージされたディクショナリとして指定された <xref:System.Windows.ResourceDictionary> 内のリソースを定義することはできます。 ただし、これは一般的なシナリオではありません。マージされたディクショナリの主要なシナリオは、外部ファイルの場所からリソースをマージすることです。 ページのマークアップ内にリソースを指定する場合は、通常、マージされたディクショナリではなく、メイン <xref:System.Windows.ResourceDictionary> で定義する必要があります。  
  
## <a name="merged-dictionary-behavior"></a>マージされたディクショナリの動作  
 マージされたディクショナリ内のリソースは、それらのリソースがマージされる先のメイン リソース ディクショナリのスコープの直後のリソース参照スコープの場所全体を占めます。 リソース キーは個々のディクショナリ内で一意である必要がありますが、マージされたディクショナリのセット内に 1 つのキーが複数回存在してもかまいません。 この場合、返されるリソースは、<xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> コレクションで順番に見つかった最後のディクショナリから取得されます。 <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> コレクションが [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で定義されている場合、コレクション内のマージされたディクショナリの順序は、マークアップで指定された要素の順序になります。 プライマリ ディクショナリ内とマージされたディクショナリ内の両方でキーが定義されている場合、返されるリソースはプライマリ ディクショナリから取られます。 これらのスコープ規則は、静的なリソース参照と動的なリソース参照の両方に等しく適用されます。  
  
### <a name="merged-dictionaries-and-code"></a>マージされたディクショナリとコード  
 マージされたディクショナリは、コードを使って `Resources` ディクショナリに追加できます。 既定では、`Resources` プロパティに存在する既定の空の <xref:System.Windows.ResourceDictionary> には、既定の空の <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> コレクションプロパティもあります。 コードを使用してマージされたディクショナリを追加するには、目的のプライマリ <xref:System.Windows.ResourceDictionary>への参照を取得し、その <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> プロパティ値を取得し、<xref:System.Windows.ResourceDictionary.MergedDictionaries%2A>に含まれる汎用 `Collection` で `Add` を呼び出します。 追加するオブジェクトは、新しい <xref:System.Windows.ResourceDictionary>である必要があります。 コードでは、<xref:System.Windows.ResourceDictionary.Source%2A> プロパティを設定しません。 代わりに、オブジェクトを作成するか、または1つを読み込んで、<xref:System.Windows.ResourceDictionary> オブジェクトを取得する必要があります。 既存の <xref:System.Windows.ResourceDictionary> を読み込み、<xref:System.Windows.ResourceDictionary> ルートを持つ既存の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルストリームで <xref:System.Windows.Markup.XamlReader.Load%2A?displayProperty=nameWithType> を呼び出す方法の1つとして、<xref:System.Windows.Markup.XamlReader.Load%2A?displayProperty=nameWithType> の戻り値を <xref:System.Windows.ResourceDictionary>にキャストする方法があります。  
  
### <a name="merged-resource-dictionary-uris"></a>マージされたリソース ディクショナリの URI  
 マージされたリソースディクショナリを含める方法には、いくつかの方法があります。これは、使用する URI (uniform resource identifier) 形式によって示されます。 大まかに言えば、これらの手法は 2 つのカテゴリに分類できます。プロジェクトの一部としてコンパイルされるリソースと、プロジェクトの一部としてコンパイルされないリソースです。  
  
 プロジェクトの一部としてコンパイルされるリソースの場合は、リソースの場所を参照する相対パスを使うことができます。 相対パスは、コンパイル中に評価されます。 リソースは、リソースのビルド アクションでプロジェクトの一部として定義する必要があります。 リソースの .xaml ファイルをリソースとしてプロジェクトに組み込む場合は、リソース ファイルを出力ディレクトリにコピーする必要がありません。リソースは、コンパイルされたアプリケーション内にあらかじめ組み込まれます。 コンテンツのビルド アクションを使うこともできますが、その場合は、ファイルを出力ディレクトリにコピーして、実行可能ファイルに対して同じパスにリソース ファイルを配置する必要があります。  
  
> [!NOTE]
> 埋め込みリソースのビルド アクションは使用しないでください。 ビルドアクション自体は [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでサポートされていますが、<xref:System.Windows.ResourceDictionary.Source%2A> の解決には <xref:System.Resources.ResourceManager>が組み込まれていないため、個々のリソースをストリームから分離することはできません。 リソースにアクセスするために <xref:System.Resources.ResourceManager> を使用する場合に限り、埋め込みリソースを他の目的に使用することもできます。  
  
 これと関連して、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルに対するパック URI を使い、その URI をソースとして参照する手法もあります。 パック URI を使用すると、参照したアセンブリのコンポーネントを参照したり、その他の手法を利用したりできます。 パック URI の詳細については、「[WPF アプリケーションのリソース ファイル、コンテンツ ファイル、およびデータ ファイル](../app-development/wpf-application-resource-content-and-data-files.md)」を参照してください。  
  
 プロジェクトの一部としてコンパイルされないリソースでは、URI が実行時に評価されます。 リソース ファイルを参照するために、file: や http: などの一般的な URI トランスポートを使うことができます。 コンパイルされないリソースの手法を使うことの欠点は、file: アクセスには追加の配置手順が必要になることと、http: アクセスはインターネット セキュリティ ゾーンを意味することです。  
  
### <a name="reusing-merged-dictionaries"></a>マージされたディクショナリの再利用  
 マージするリソースディクショナリは任意の有効な URI (uniform resource identifier) を介して参照できるため、アプリケーション間でマージされたリソースディクショナリを再利用または共有できます。 これを行うための具体的な方法は、アプリケーションの配置方法と、採用するアプリケーション モデルによって異なります。 前述のパック URI の手法を使うと、開発中にアセンブリ参照を共有することにより、マージされたリソースを複数のプロジェクトから取得する一般的な方法を利用することができます。 このシナリオでは、リソースはまだクライアントによって配布されるため、少なくとも 1 つのアプリケーションで参照先のアセンブリを配置する必要があります。 また、http プロトコルを使う分散 URI によって、マージされたリソースを参照することもできます。  
  
 マージされたディクショナリ/アプリケーションの配置で考えられる別のシナリオは、マージされたディクショナリをローカル アプリケーション ファイルとして、またはローカルの共有記憶域に書き出すことです。  
  
### <a name="localization"></a>ローカリゼーション  
 ローカライズする必要のあるリソースが、プライマリ ディクショナリにマージされたディクショナリに分離されて、Loose [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルとして保持されている場合、これらのファイルを別個にローカライズすることができます。 これは、サテライト リソース アセンブリをローカライズするよりも簡易な手法です。 詳細については、「[WPF のグローバリゼーションおよびローカリゼーションの概要](wpf-globalization-and-localization-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.ResourceDictionary>
- [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [リソースとコード](resources-and-code.md)
- [WPF アプリケーションのリソース ファイル、コンテンツ ファイル、およびデータ ファイル](../app-development/wpf-application-resource-content-and-data-files.md)
