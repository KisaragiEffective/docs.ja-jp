---
title: リソースとコード
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- keys [WPF], using objects as
- resources [WPF], accessing from procedural code
- procedural code [WPF], creating resources with
- procedural code [WPF], accessing resources from
- resources [WPF], creating with procedural code
ms.assetid: c1cfcddb-e39c-41c8-a7f3-60984914dfae
ms.openlocfilehash: 11903a9bae25b0646d944fb11038e07434996015
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559756"
---
# <a name="resources-and-code"></a>リソースとコード
この概要では、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] 構文ではなく、コードを使用して [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] リソースにアクセスする方法、またはこのリソースを作成する方法に重点を置いて説明します。 一般的なリソースの使用法と [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 構文の観点から見たリソースの詳細については、「[XAML リソース](xaml-resources.md)」を参照してください。  

<a name="accessing"></a>   
## <a name="accessing-resources-from-code"></a>コードからのリソースへのアクセス  
 リソースを識別するキーは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で定義されていれば、コードでリソースを要求する際にも特定のリソースの取得に使用されます。 コードからリソースを取得する最も簡単な方法は、アプリケーションでフレームワークレベルのオブジェクトから <xref:System.Windows.FrameworkElement.FindResource%2A> または <xref:System.Windows.FrameworkElement.TryFindResource%2A> メソッドを呼び出すことです。 これらのメソッドは、要求したキーが見つからなかった場合の動作に違いがあります。 <xref:System.Windows.FrameworkElement.FindResource%2A> は例外を発生させます。<xref:System.Windows.FrameworkElement.TryFindResource%2A> は例外を発生させませんが、`null`を返します。 それぞれのメソッドは、入力パラメーターとしてリソース キーを受け取り、緩く型指定されたオブジェクトを返します。 通常、リソース キーは文字列ですが、文字列以外を使用する場合もあります。詳細については、「[キーとしてのオブジェクトの使用](#objectaskey)」のセクションを参照してください。 通常は、返されたオブジェクトを、リソースの要求時に設定するプロパティで必要な型にキャストします。 コードのリソース解決の検索ロジックは、動的リソース参照の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の場合と同じです。 リソースの検索は、呼び出し元の要素から開始され、論理ツリー内の連続する親要素へと続きます。 その後、必要に応じて、アプリケーション リソース、テーマ、システム リソースへと検索が続きます。 コードによるリソースの要求では、リソース ディクショナリが [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で読み込まれた後に行われた実行時の変更と、リアルタイムのシステム リソースの変更が適切に考慮されます。  
  
 キーによってリソースを検索し、戻り値を使用してプロパティを設定し、<xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントハンドラーとして実装する簡単なコード例を次に示します。  
  
 [!code-csharp[PropertiesOvwSupport#ResourceProceduralGet](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml.cs#resourceproceduralget)]
 [!code-vb[PropertiesOvwSupport#ResourceProceduralGet](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page3.xaml.vb#resourceproceduralget)]  
  
 リソース参照を割り当てる別の方法として、<xref:System.Windows.FrameworkElement.SetResourceReference%2A>ます。 このメソッドは、リソースのキーと、リソース値を割り当てる要素インスタンスに存在する特定の依存関係プロパティの識別子の 2 つのパラメーターを受け取ります。 このメソッドは機能的には前のメソッドと同じですが、戻り値をキャストしなくて済むという利点があります。  
  
 プログラムによってリソースにアクセスするもう1つの方法として、<xref:System.Windows.FrameworkElement.Resources%2A> プロパティの内容に辞書としてアクセスする方法もあります。 このプロパティによって格納されたディクショナリにアクセスする方法は、新しいリソースを既存のコレクションに追加する方法、指定したキー名がコレクション既に取得されたかどうかを確認する方法などのその他のディクショナリ/コレクション操作でもあります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションを完全にコード内で記述する場合は、コードでコレクション全体を作成し、キーを割り当てて、完成したコレクションを確立された要素の <xref:System.Windows.FrameworkElement.Resources%2A> プロパティに割り当てることもできます。 これについては次のセクションで説明します。  
  
 インデックスとして特定のキーを使用して、特定の <xref:System.Windows.FrameworkElement.Resources%2A> コレクション内でインデックスを作成できますが、この方法でリソースにアクセスしても、リソース解決の通常のランタイムルールには従っていないことに注意してください。 特定のコレクションにアクセスしているだけです。 要求したキーで有効なオブジェクトが見つからなかった場合、リソース検索がルートまたはアプリケーションに向かうスコープを走査することはありません。 ただし、この方法では、キーの検索スコープがより限定されるため、特定のケースでパフォーマンス上のメリットが生じる場合があります。 リソースディクショナリを直接操作する方法の詳細については、<xref:System.Windows.ResourceDictionary> クラスを参照してください。  
  
<a name="creating"></a>   
## <a name="creating-resources-with-code"></a>コードによるリソースの作成  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーション全体をコードで作成する場合は、そのアプリケーションのリソースもコードで作成する必要があります。 これを実現するには、新しい <xref:System.Windows.ResourceDictionary> インスタンスを作成し、<xref:System.Windows.ResourceDictionary.Add%2A?displayProperty=nameWithType>への連続した呼び出しを使用して、すべてのリソースをディクショナリに追加します。 次に、ページスコープまたは <xref:System.Windows.Application.Resources%2A?displayProperty=nameWithType>に存在する要素の <xref:System.Windows.FrameworkElement.Resources%2A> プロパティを設定するために作成された <xref:System.Windows.ResourceDictionary> を使用します。 また、<xref:System.Windows.ResourceDictionary> を要素に追加せずに、スタンドアロンオブジェクトとして保持することもできます。 ただし、これを行う場合は、ジェネリック ディクショナリの場合と同様に、項目のキーによって内部のリソースにアクセスする必要があります。 要素 `Resources` プロパティに関連付けられていない <xref:System.Windows.ResourceDictionary> は、要素ツリーの一部として存在しないため、<xref:System.Windows.FrameworkElement.FindResource%2A> および関連するメソッドで使用できる参照シーケンスにスコープがありません。  
  
<a name="objectaskey"></a>   
## <a name="using-objects-as-keys"></a>キーとしてのオブジェクトの使用  
 通常、リソースを使用する場合、リソースのキーは文字列として設定されます。 ただし、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の各種機能の中にはキーを指定する際に意図的に文字列型を使用しないで、このパラメーターにオブジェクトを使用するものもあります。 オブジェクトによってリソースにキーを設定するには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] でのスタイルおよびテーマの適用のサポートを利用します。 スタイルが設定されていないコントロールの既定のスタイルになるテーマのスタイルは、それぞれ、適用するコントロールの <xref:System.Type> によってキー指定されます。 型によってキーが設定されると、各コントロール型の既定のインスタンスで機能する信頼できる検索機構が実現され、型はリフレクションによって検索できるようになり、派生型に既定のスタイルが設定されていない場合でも派生クラスのスタイルを設定する際に使用できます。 [X:Type マークアップ拡張機能](../../../desktop-wpf/xaml-services/xtype-markup-extension.md)を使用して、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で定義されているリソースの <xref:System.Type> キーを指定できます。 [ComponentResourceKey マークアップ拡張機能](componentresourcekey-markup-extension.md)など、文字列以外のキーの使用で [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 機能をサポートする同様の拡張機能があります。  
  
## <a name="see-also"></a>関連項目

- [XAML リソース](xaml-resources.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
