---
title: XAML 名前空間と名前空間のマッピング
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom classes [WPF], mapping namespaces to
- XAML [WPF], namespaces
- namespace mapping [WPF]
- assemblies [WPF], mapping namespaces to
- mapping namespaces [WPF]
- XAML [WPF], namespace mapping
- classes [WPF], mapping namespaces to
- namespaces [WPF]
ms.assetid: 5c0854e3-7470-435d-9fe2-93eec9d3634e
ms.openlocfilehash: ac9e028d8804c4fdadf4e6be8d7650afa1406969
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124469"
---
# <a name="xaml-namespaces-and-namespace-mapping-for-wpf-xaml"></a>XAML 名前空間および WPF XAML の名前空間の割り当て
このトピックでは、WPF XAML ファイルのルートタグでよく見られる2つの XAML 名前空間マッピングの存在と目的について詳しく説明します。 また、独自のコードで定義されている要素、または別のアセンブリ内に定義されている要素を使用するために、同様のマッピングを生成する方法についても説明します。  

## <a name="what-is-a-xaml-namespace"></a>XAML 名前空間とは何ですか。  
 XAML 名前空間は、実際には XML 名前空間の概念を拡張したものです。 XAML 名前空間を指定する方法は、XML 名前空間の構文、Uri を名前空間の識別子として使用する規則、同じマークアップソースから複数の名前空間を参照する手段を提供するプレフィックスを使用する方法などに依存します。 XML 名前空間の XAML 定義に追加される主な概念は、XAML 名前空間がマークアップ使用の一意性の範囲を示すことです。また、特定の CLR 名前空間および参照によってマークアップエンティティがどのようにサポートされるかにも影響します。アセンブリ. この2つ目の考慮事項は、XAML スキーマコンテキストの概念にも影響を受けます。 ただし、WPF で XAML 名前空間を使用する方法については、一般に、xaml 名前空間、xaml 言語の名前空間、および XAML マークアップによって特定のバッキング CLR に直接マップされる xaml 名前空間に関して、xaml 名前空間について考えることができます。名前空間と参照されるアセンブリ。  
  
<a name="The_WPF_and_XAML_Namespace_Declarations"></a>   
## <a name="the-wpf-and-xaml-namespace-declarations"></a>WPF および XAML 名前空間の宣言  
 多くの XAML ファイルのルートタグ内の名前空間宣言内では、通常、2つの XML 名前空間宣言があることがわかります。 最初の宣言は、WPF クライアント/フレームワークの全体的な XAML 名前空間を既定値としてマップします。  
  
 `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`  
  
 2番目の宣言は、別の XAML 名前空間をマップし、その名前空間 (通常は) を `x:` のプレフィックスに割り当てます。  
  
 `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`  
  
 これらの宣言の間の関係は、`x:` プレフィックスマッピングが XAML 言語定義の一部である組み込みをサポートし、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は xaml を言語として使用し、XAML 用のオブジェクトのボキャブラリを定義する1つの実装であることです。 WPF のボキャブラリの使用は XAML 組み込みの使用法よりはるかに一般的であるため、WPF のボキャブラリは既定値としてマップされます。  
  
 XAML 言語の組み込みサポートをマップするための `x:` プレフィックス規則には、プロジェクトテンプレート、サンプルコード、およびこの SDK 内の言語機能のドキュメントが続きます。 XAML 名前空間は、基本的な WPF アプリケーションでも必要な一般的に使用される多くの機能を定義します。 たとえば、部分クラスを使用して XAML ファイルに分離コードを結合するには、そのクラスに関連する XAML ファイルのルート要素の `x:Class` 属性として名前を付ける必要があります。 または、キー付きリソースとしてアクセスする XAML ページで定義されているすべての要素に、対象の要素に対して `x:Key` 属性が設定されている必要があります。 これらの XAML の詳細については、「 [xaml の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md) 」または「 [Xaml 構文の詳細](xaml-syntax-in-detail.md)」を参照してください。  
  
<a name="Mapping_To_Custom_Classes_and_Assemblies"></a>   
## <a name="mapping-to-custom-classes-and-assemblies"></a>カスタムクラスとアセンブリへのマッピング  
 `xmlns` プレフィックス宣言内の一連のトークンを使用して、XML 名前空間をアセンブリにマップできます。これは、標準の WPF および XAML 組み込みの XAML 名前空間をプレフィックスにマップする方法と似ています。  
  
 構文では、次の名前付きトークンと次の値を使用できます。  
  
 要素として公開するパブリック型を含むアセンブリ内で宣言されている CLR 名前空間を `clr-namespace:` します。  
  
 参照されている CLR 名前空間の一部またはすべてを含むアセンブリを `assembly=` します。 通常、この値はパスではなくアセンブリの名前にすぎず、拡張子 (.dll や .exe など) は含まれません。 このアセンブリへのパスは、マップしようとしている XAML を含むプロジェクトファイル内のプロジェクト参照として設定する必要があります。 バージョン管理と厳密な名前の署名を組み込むには、単純な文字列名ではなく、<xref:System.Reflection.AssemblyName>で定義されている文字列を `assembly` 値にすることができます。  
  
 `clr-namespace` トークンを値から区切る文字がコロン (:) であることに注意してください。`assembly` トークンを値から区切る文字は等号 (=) です。 これら2つのトークンの間で使用する文字はセミコロンです。 また、宣言のどこにも空白を入れないでください。  
  
### <a name="a-basic-custom-mapping-example"></a>基本的なカスタムマッピングの例  
 次のコードでは、カスタムクラスの例を定義しています。  
  
```csharp  
namespace SDKSample {  
    public class ExampleClass : ContentControl {  
        public ExampleClass() {  
        ...  
        }  
    }  
}  
```  
  
```vb  
Namespace SDKSample  
    Public Class ExampleClass  
        Inherits ContentControl  
         ...  
        Public Sub New()  
        End Sub  
    End Class  
End Namespace  
```  
  
 このカスタムクラスは、プロジェクト設定 (図に示されていません) ごとに `SDKSampleLibrary`という名前のライブラリにコンパイルされます。  
  
 このカスタムクラスを参照するには、現在のプロジェクトの参照としても含める必要があります。これは通常、Visual Studio のソリューションエクスプローラー UI を使用します。  
  
 クラスが含まれているライブラリと、プロジェクト設定にそのライブラリへの参照があるので、次のプレフィックスマッピングを XAML のルート要素の一部として追加できます。  
  
 `xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary"`  
  
 すべてをまとめて配置するには、次の XAML を使用します。これには、ルートタグ内の一般的な default および x: マッピングと共にカスタムマッピングが含まれています。次に、プレフィックス付き参照を使用して、その UI で `ExampleClass` をインスタンス化します。  
  
```xaml  
<Page x:Class="WPFApplication1.MainPage"  
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"   
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
    xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary">  
  ...  
  <custom:ExampleClass/>  
...  
</Page>  
```  
  
### <a name="mapping-to-current-assemblies"></a>現在のアセンブリへのマッピング  
 参照されている `clr-namespace` が、カスタムクラスを参照しているアプリケーションコードと同じアセンブリ内で定義されている場合は、`assembly` を省略できます。 または、この場合と同等の構文として、等号の後に文字列トークンを指定せずに `assembly=`を指定します。  
  
 カスタムクラスは、同じアセンブリ内で定義されている場合、ページのルート要素として使用することはできません。 部分クラスをマップする必要はありません。XAML の要素として参照する場合は、アプリケーション内のページの部分クラスではないクラスのみをマップする必要があります。  
  
<a name="Mapping_CLR_Namespaces_to_XML_Namespaces_in_an"></a>   
## <a name="mapping-clr-namespaces-to-xml-namespaces-in-an-assembly"></a>アセンブリ内の XML 名前空間への CLR 名前空間のマッピング  
 WPF では、複数の CLR 名前空間を1つの XAML 名前空間にマップするために XAML プロセッサによって使用される CLR 属性を定義します。 この属性 <xref:System.Windows.Markup.XmlnsDefinitionAttribute>は、アセンブリを生成するソースコードのアセンブリレベルに配置されます。 WPF アセンブリソースコードでは、この属性を使用して、<xref:System.Windows> や <xref:System.Windows.Controls>などのさまざまな共通名前空間を `http://schemas.microsoft.com/winfx/2006/xaml/presentation` 名前空間にマップします。  
  
 <xref:System.Windows.Markup.XmlnsDefinitionAttribute> は、XML/XAML 名前空間名と CLR 名前空間名の2つのパラメーターを受け取ります。 複数の CLR 名前空間を同じ XML 名前空間にマップするために、複数の <xref:System.Windows.Markup.XmlnsDefinitionAttribute> を存在させることができます。 これらの名前空間のメンバーは、マップされた後、部分クラスの分離コードページで適切な `using` ステートメントを指定することにより、必要に応じて完全修飾なしで参照することもできます。 詳細については、「<xref:System.Windows.Markup.XmlnsDefinitionAttribute>」を参照してください。  
  
## <a name="designer-namespaces-and-other-prefixes-from-xaml-templates"></a>デザイナーの名前空間と XAML テンプレートからのその他のプレフィックス  
 WPF XAML 用の開発環境やデザインツールで作業している場合は、xaml マークアップ内に他の定義済みの XAML 名前空間/プレフィックスがあることがわかります。  
  
 Visual Studio の WPF デザイナーでは、通常、プレフィックス `d:`にマップされるデザイナーの名前空間を使用します。 WPF の新しいプロジェクトテンプレートでは、この XAML 名前空間を事前にマップして、Visual Studio の WPF デザイナーとその他のデザイン環境間で XAML のインターチェンジをサポートすることができます。 このデザインの XAML 名前空間は、デザイナーの XAML ベースの UI を roundtripping しながら、デザイン状態を perpetuate するために使用されます。 また、デザイナーで実行時データソースを有効にする `d:IsDataSource`などの機能にも使用されます。  
  
 マップされていると思われるもう1つのプレフィックスは `mc:`です。 `mc:` は、マークアップ互換性のためのものであり、必ずしも XAML 固有ではないマークアップ互換性パターンを利用しています。 また、マークアップ互換性機能を使用して、フレームワーク間、またはバッキング実装の他の境界間で XAML を交換したり、XAML スキーマコンテキスト間で作業したり、デザイナーで制限されたモードの互換性を提供したりすることができます。 マークアップ互換性の概念と WPF との関係の詳細については、「[マークアップの互換性 (mc:)」を参照してください。言語機能](markup-compatibility-mc-language-features.md)。  
  
## <a name="wpf-and-assembly-loading"></a>WPF とアセンブリの読み込み  
 WPF の XAML スキーマコンテキストは WPF アプリケーションモデルと統合され、さらに、CLR によって定義された <xref:System.AppDomain>の概念が使用されます。 次に、XAML スキーマコンテキストで、WPF での <xref:System.AppDomain> とその他の要素の使用に基づいて、実行時またはデザイン時にアセンブリを読み込む方法または型を検索する方法について説明します。  
  
1. 最後に読み込まれたアセンブリから開始して、名前のすべての側面に一致する、既に読み込まれているアセンブリを検索して、<xref:System.AppDomain>を反復処理します。  
  
2. 名前が修飾されている場合は、修飾名に対して <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=nameWithType> を呼び出します。  
  
3. 修飾名の短い名前と公開キートークンが、マークアップが読み込まれたアセンブリと一致する場合は、そのアセンブリを返します。  
  
4. <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=nameWithType>を呼び出すには、短い名前と公開キートークンを使用します。  
  
5. 名前が修飾されていない場合は、<xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType>を呼び出します。  
  
 ルース XAML では、手順 3; が使用されません。アセンブリが読み込まれていません。  
  
 WPF 用にコンパイルされた XAML (XamlBuildTask を使用して生成) では、<xref:System.AppDomain> (手順 1) で既に読み込まれているアセンブリは使用しません。 また、XamlBuildTask の出力から名前を修飾しないでください。そのため、手順5は適用されません。  
  
 (プレゼンテーション Buildtask によって生成される) コンパイル済みの BAML はすべての手順を使用しますが、BAML には修飾されていないアセンブリ名を含めることはできません。  
  
## <a name="see-also"></a>参照

- [XML 名前空間について](https://docs.microsoft.com/previous-versions/aa468565(v=msdn.10))
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
