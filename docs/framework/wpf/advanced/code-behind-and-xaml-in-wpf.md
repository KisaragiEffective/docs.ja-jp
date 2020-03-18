---
title: 分離コードと XAML
ms.date: 03/30/2017
helpviewer_keywords:
- XAML [WPF], code-behind
- code-behind files [WPF], XAML
ms.assetid: 9df6d3c9-aed3-471c-af36-6859b19d999f
ms.openlocfilehash: 212a37fb7fbcb7e66a669d96671333be793956df
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738091"
---
# <a name="code-behind-and-xaml-in-wpf"></a>WPF における分離コードと XAML
<a name="introduction"></a>分離コードは、マークアップで定義されたオブジェクトと結合されるコードを記述するために使用される用語です。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページがマークアップコンパイルされている場合です。 このトピックでは、分離コードの要件と、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]のコードの代替インラインコード機構について説明します。  
  
 このトピックには、次のセクションが含まれます。  
  
- [前提条件](#Prerequisites)  
  
- [分離コードと XAML 言語](#codebehind_and_the_xaml_language)  
  
- [WPF の分離コード、イベントハンドラー、および部分クラスの要件](#Code_behind__Event_Handler__and_Partial_Class)  
  
- [x:Code](#x_Code)  
  
- [インラインコードの制限事項](#Inline_Code_Limitations)  
  
<a name="Prerequisites"></a>   
## <a name="prerequisites"></a>前提条件  
 このトピックでは、「XAML の[概要 (WPF)」](../../../desktop-wpf/fundamentals/xaml.md)を読み、CLR とオブジェクト指向プログラミングに関する基本的な知識を持っていることを前提としています。  
  
<a name="codebehind_and_the_xaml_language"></a>   
## <a name="code-behind-and-the-xaml-language"></a>分離コードと XAML 言語  
 XAML 言語には、マークアップファイル側からコードファイルをマークアップファイルに関連付けることができるようにする言語レベルの機能が含まれています。 具体的には、XAML 言語では、言語機能の[X:Class ディレクティブ](../../../desktop-wpf/xaml-services/xclass-directive.md)、 [x:Subclass ディレクティブ](../../../desktop-wpf/xaml-services/xsubclass-directive.md)、および[x:ClassModifier ディレクティブ](../../../desktop-wpf/xaml-services/xclassmodifier-directive.md)が定義されています。 コードをどのように生成するか、およびマークアップとコードを統合する方法は、XAML 言語が指定する内容の一部ではありません。 WPF などのフレームワークによって、コードを統合する方法、アプリケーションとプログラミングモデルで XAML を使用する方法、ビルドアクションやその他のすべてのサポートが必要であるかどうかが決定されます。  
  
<a name="Code_behind__Event_Handler__and_Partial_Class"></a>   
## <a name="code-behind-event-handler-and-partial-class-requirements-in-wpf"></a>WPF の分離コード、イベントハンドラー、および部分クラスの要件  
  
- 部分クラスは、ルート要素をバッキングする型から派生する必要があります。  
  
- マークアップコンパイルビルドアクションの既定の動作では、分離コード側の部分クラス定義で派生を空白のままにすることができます。 コンパイルされた結果は、指定されていない場合でも、ページルートのバッキング型が部分クラスの基礎となることを前提としています。 ただし、この動作に依存することは、ベストプラクティスではありません。  
  
- 分離コードで記述するイベントハンドラーは、インスタンスメソッドである必要があり、静的メソッドにすることはできません。 これらのメソッドは、`x:Class`によって識別される CLR 名前空間内の部分クラスによって定義する必要があります。 イベントハンドラーの名前を修飾して、別のクラススコープのイベントのイベントハンドラーを検索するように [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサに指示することはできません。  
  
- ハンドラーは、バッキング型システム内の適切なイベントのデリゲートと一致する必要があります。  
  
- 特に、Microsoft Visual Basic 言語では、言語固有の `Handles` キーワードを使用して、ハンドラーをハンドラー宣言内のインスタンスおよびイベントに関連付けることができます。これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]の属性を持つハンドラーをアタッチすることではありません。 ただし、この方法にはいくつかの制限があります。 `Handles` キーワードは、特定のルーティングイベントのシナリオや添付イベントなど、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] イベントシステムの特定の機能をすべてサポートすることができないためです。 詳細については、「 [Visual Basic と WPF のイベント処理](visual-basic-and-wpf-event-handling.md)」を参照してください。  
  
<a name="x_Code"></a>   
## <a name="xcode"></a>x:Code  
 [x:Code](../../../desktop-wpf/xaml-services/xcode-intrinsic-xaml-type.md)は [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で定義されたディレクティブ要素です。 `x:Code` ディレクティブ要素には、インラインプログラミングコードを含めることができます。 インラインで定義されているコードは、同じページの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] と対話できます。 次の例は、 C#インラインコードを示しています。 コードが `x:Code` 要素の内側にあり、`<CDATA[`...`]]>` でコードを囲む必要があることに注意してください。これにより、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサ ([!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] スキーマまたは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] スキーマを解釈する) では、コンテンツを XML として解釈しようとしません。  
  
 [!code-xaml[XAMLOvwSupport#ButtonWithInlineCode](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page4.xaml#buttonwithinlinecode)]  
  
<a name="Inline_Code_Limitations"></a>   
## <a name="inline-code-limitations"></a>インラインコードの制限事項  
 インラインコードの使用を回避または制限することを検討してください。 アーキテクチャとコーディングの理念に関して、マークアップと分離コードの分離を維持することで、デザイナーと開発者のロールがより明確になります。 より技術的なレベルでは、インラインコード用に記述するコードを記述するのは困難な場合があります。これは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 生成された部分クラスに常に書き込みを行い、既定の XML 名前空間マッピングのみを使用できるためです。 `using` ステートメントを追加することはできないため、作成する API 呼び出しの多くを完全に修飾する必要があります。 既定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] マッピングには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アセンブリに存在するほとんどの CLR 名前空間が含まれます。他の CLR 名前空間内に含まれる型およびメンバーの呼び出しを完全に修飾する必要があります。 また、インラインコードで部分クラスを超えるものを定義することはできません。また、参照するすべてのユーザーコードエンティティは、生成された部分クラス内のメンバーまたは変数として存在する必要があります。 マクロや、グローバル変数やビルド変数に対する `#ifdef` など、言語固有のプログラミング機能も使用できません。 詳細については、「 [X:Code 組み込み XAML 型](../../../desktop-wpf/xaml-services/xcode-intrinsic-xaml-type.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [x:Code 組み込み XAML 型 ](../../../desktop-wpf/xaml-services/xcode-intrinsic-xaml-type.md)
- [WPF アプリケーションのビルド](../app-development/building-a-wpf-application-wpf.md)
- [XAML 構文の詳細](xaml-syntax-in-detail.md)
