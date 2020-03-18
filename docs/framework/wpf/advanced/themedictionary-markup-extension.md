---
title: ThemeDictionary のマークアップ拡張機能
ms.date: 03/30/2017
f1_keywords:
- ThemeDictionaryExtension
- ThemeDictionary
helpviewer_keywords:
- ThemeDictionary markup extension [WPF]
- XAML [WPF], ThemeDictionary markup extension
ms.assetid: aa75e10b-13dd-4989-972d-51bab63a05e2
ms.openlocfilehash: ab38c2c885e230183852fff895e0a8a8f1d7a666
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459490"
---
# <a name="themedictionary-markup-extension"></a>ThemeDictionary のマークアップ拡張機能
カスタムコントロールの作成者またはサードパーティのコントロールを統合して、コントロールのスタイル設定に使用するテーマ固有のリソースディクショナリを読み込む方法を提供します。  
  
## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法  
  
```xml  
<object property="{ThemeDictionary assemblyUri}" .../>  
```  
  
## <a name="xaml-object-element-usage"></a>XAML オブジェクト要素の使用方法  
  
```xml  
<object>  
  <object.property>  
    <ThemeDictionary AssemblyName="assemblyUri"/>  
  <object.property>  
<object>  
```  
  
## <a name="xaml-values"></a>XAML 値  
  
|||  
|-|-|  
|`assemblyUri`|テーマ情報を格納しているアセンブリの URI (uniform resource identifier)。 通常、これは、大きなパッケージ内のアセンブリを参照するパック URI です。 アセンブリリソースとパック Uri は、デプロイの問題を簡略化します。 詳細については、「 [WPF のパック uri](../app-development/pack-uris-in-wpf.md)」を参照してください。|  
  
## <a name="remarks"></a>Remarks  
 この拡張機能は、1つの特定のプロパティ値のみを格納することを目的としています。 <xref:System.Windows.ResourceDictionary.Source%2A?displayProperty=nameWithType>の値です。  
  
 この拡張機能を使用すると、1つのリソースのみのアセンブリを指定できます。このアセンブリには、Windows Aero テーマがユーザーのシステムに適用されている場合にのみ使用し、他のスタイルは、Luna テーマがアクティブになっている場合にのみ使用します。 この拡張機能を使用すると、必要に応じて、コントロール固有のリソースディクショナリの内容を自動的に無効にして、別のテーマに合わせて再度読み込むことができます。  
  
 `assemblyUri` string (<xref:System.Windows.ThemeDictionaryExtension.AssemblyName%2A> プロパティ値) は、特定のテーマに適用される辞書を識別する名前付け規則の基礎となります。 `ThemeDictionary` の <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> ロジックは、プリコンパイル済みリソースアセンブリに含まれているように、特定のテーマディクショナリバリアントを指す uniform resource identifier (URI) を生成することによって規則を完成させるものです。 ここでは、この規則の説明、一般的なコントロールのスタイル設定とページ/アプリケーションレベルのスタイル設定とのテーマの相互作用については説明しません。 `ThemeDictionary` を使用するための基本的なシナリオは、アプリケーションレベルで宣言された `ResourceDictionary` の <xref:System.Windows.ResourceDictionary.Source%2A> プロパティを指定することです。 ダイレクト URI ではなく `ThemeDictionary` の拡張機能を使用してアセンブリの URI を指定すると、システムテーマが変更されるたびに適用される無効化ロジックが拡張ロジックによって提供されます。  
  
 属性構文は、このマークアップ拡張機能で使用される最も一般的な構文です。 `ThemeDictionary` 識別子文字列の後に設定される文字列トークンは、基になる <xref:System.Windows.ThemeDictionaryExtension.AssemblyName%2A> 拡張クラスの <xref:System.Windows.ThemeDictionaryExtension> 値として割り当てられます。  
  
 `ThemeDictionary` は、オブジェクト要素構文でも使用できます。 この場合は、<xref:System.Windows.ThemeDictionaryExtension.AssemblyName%2A> プロパティの値を指定する必要があります。  
  
 `ThemeDictionary` は、<xref:System.Windows.Markup.StaticExtension.Member%2A> プロパティをプロパティおよび値のペアとして指定する詳細出力属性使用でも使用できます。  
  
```xml  
<object property="{ThemeDictionary AssemblyName=assemblyUri}" .../>  
```  
  
 詳細出力の使用は、複数の設定可能プロパティを持つ拡張機能や、一部のプロパティがオプションである場合に役立ちます。 `ThemeDictionary` には、必須の設定可能プロパティが 1 つしか存在しないため、このような詳細出力の使用は一般的ではありません。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサの実装では、このマークアップ拡張機能の処理は <xref:System.Windows.ThemeDictionaryExtension> クラスによって定義されます。  
  
 `ThemeDictionary` はマークアップ拡張機能です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 内のすべてのマークアップ拡張機能は、属性構文で {および} 文字を使用します。これは、マークアップ拡張機能が属性を処理する必要があることを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサが認識する規則です。 詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [スタイルとテンプレート](../controls/styling-and-templating.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
- [WPF アプリケーションのリソース ファイル、コンテンツ ファイル、およびデータ ファイル](../app-development/wpf-application-resource-content-and-data-files.md)
