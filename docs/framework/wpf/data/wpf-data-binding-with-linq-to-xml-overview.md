---
title: LINQ to XML による WPF のデータ バインディング
ms.date: 10/22/2019
ms.topic: conceptual
ms.openlocfilehash: 65e1524a88f1920c037b2747b0bbe30386951635
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452735"
---
# <a name="overview-of-wpf-data-binding-with-linq-to-xml"></a>LINQ to XML による WPF のデータバインディングの概要

この記事では、<xref:System.Xml.Linq> 名前空間の動的データバインディング機能について説明します。 これらの機能は、Windows Presentation Foundation (WPF) アプリのユーザー インターフェイス (UI) 要素のデータ ソースとして使用できます。 このシナリオは、*および* の特殊な<xref:System.Xml.Linq.XAttribute?displayProperty=fullName>動的プロパティ<xref:System.Xml.Linq.XElement?displayProperty=fullName> に依存しています。

## <a name="xaml-and-linq-to-xml"></a>XAML と LINQ to XML

Extensible Application Markup Language (XAML) は、.NET のテクノロジをサポートするために Microsoft によって作成された XML 言語です。 XAML は、WPF ではユーザー インターフェイス要素やその関連機能 (イベントやデータ バインドなど) を表すために使用され、 Windows Workflow Foundation ではプログラム制御 (*ワークフロー*) などのプログラム構造を表すために使用されます。 XAML を使用すると、テクノロジの宣言型の側面を、プログラムのより個別的な動作を定義する、関連する手続き型のコードから分離することができます。

XAML と LINQ to XML の相互作用には、大きく分けて次の 2 つの方法があります。

- XAML ファイルは整形式の XML であるため、LINQ to XML などの XML テクノロジによるクエリや操作が可能です。

- LINQ to XML のクエリはデータのソースを表すため、WPF UI 要素のデータ バインドのデータ ソースとして使用できます。

このドキュメントでは、2 番目のシナリオについて説明します。

## <a name="data-binding-in-the-windows-presentation-foundation"></a>Windows Presentation Foundation でのデータバインディング

WPF のデータ バインドでは、UI 要素のプロパティをデータ ソースに関連付けることができます。 たとえば、ユーザー定義オブジェクトのパブリック プロパティの値をテキストとして表示する <xref:System.Windows.Controls.Label> はその簡単な例です。 WPF のデータ バインドは次のコンポーネントに依存しています。

|コンポーネント|説明|
|---------------|-----------------|
|バインド ターゲット|データ ソースに関連付ける UI 要素。 WPF のビジュアル要素は <xref:System.Windows.UIElement> クラスから派生します。|
|対象になるプロパティ|バインド ターゲットの*依存プロパティ*。データ バインディング ソースの値が反映されます。 依存プロパティは、<xref:System.Windows.DependencyObject> の派生元である <xref:System.Windows.UIElement> クラスによって直接サポートされます。|
|バインド ソース|表示のために UI 要素に渡される 1 つ以上の値のソース オブジェクト。 WPF で自動的にサポートされるバインド ソースは、CLR オブジェクト、ADO.NET データ オブジェクト、XML データ (XPath または LINQ to XML のクエリからの XML データ)、および他の <xref:System.Windows.DependencyObject> です。|
|ソース パス|バインドされる値または値のセットに解決されるバインド ソースのプロパティ。|

依存プロパティは WPF 固有の概念であり、UI 要素の動的に計算されるプロパティを表します。 たとえば、依存プロパティの既定値は多くの場合、親要素によって提供されます。 これらの特殊なプロパティは、<xref:System.Windows.DependencyProperty> クラスのインスタンスに基づいています (標準のプロパティはフィールドに基づいています)。 依存関係プロパティの詳細については、「[依存関係プロパティの概要](../advanced/dependency-properties-overview.md)」を参照してください。

### <a name="dynamic-data-binding-in-wpf"></a>WPF での動的データバインディング

既定では、対象となる UI 要素が初期化されたときにだけデータ バインディングが行われます。 これを*ワンタイム* バインドと呼びます。 ほとんどの場合、このバインドは十分には役立ちません。一般に、データ バインディングのソリューションでは、次のいずれかの方法を使用して実行時に変更が動的に反映されるようにする必要があります。

- *一方向*のバインド。一方に対する変更が自動的に反映されます。 ソースに対する変更がターゲットに反映されるのが最も一般的ですが、その逆のパターンが役に立つ場合もあります。

- *双方向*のバインド。ソースに対する変更がターゲットに、ターゲットに対する変更がソースに、それぞれ自動的に反映されます。

一方向または双方向のバインドが行われるようにするには、変更通知のメカニズムをソースに実装する必要があります。たとえば、<xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスを実装するか、サポートする各プロパティに対して *PropertyNameChanged* パターンを使用します。

WPF のデータ バインディングの詳細については、「[データ バインディングの概要](/dotnet/framework/wpf/data/data-binding-wpf)」を参照してください。

## <a name="dynamic-properties-in-linq-to-xml-classes"></a>LINQ to XML クラスの動的プロパティ

ほとんどの LINQ to XML クラスは、WPF の動的データ ソースとして適切ではありません。 最も有用な情報の一部は、プロパティではなく、メソッドを経由した場合にのみ取得でき、これらのクラスのプロパティでは変更通知が実装されません。 LINQ to XML では、WPF のデータ バインディングをサポートするために一連の*動的プロパティ*が公開されます。

これらの動的プロパティは、<xref:System.Xml.Linq.XAttribute> クラスと <xref:System.Xml.Linq.XElement> クラスの既存のメソッドやプロパティと同じ機能を持つ特殊な実行時プロパティであり、 これらのクラスを WPF の動的データ ソースとして使用することのみを目的として追加されています。 この目的を満たすため、これらすべての動的プロパティに変更通知が実装されています。 これらの動的プロパティの詳細については、次の「[LINQ to XML の動的プロパティ](linq-to-xml-dynamic-properties.md)」を参照してください。

> [!NOTE]
> <xref:System.Xml.Linq> 名前空間のさまざまなクラスに含まれている標準のパブリック プロパティの多くは、一方向のデータ バインドには使用できます。 ただし、既に説明したように、一方向のデータ バインドではソースもターゲットも動的に更新されません。

### <a name="access-dynamic-properties"></a>動的プロパティへのアクセス

<xref:System.Xml.Linq.XAttribute> クラスと <xref:System.Xml.Linq.XElement> クラスの動的プロパティには、標準のプロパティのようにアクセスすることはできません。 たとえば、C# などの CLR 準拠の言語で次のような操作を行うことはできません。

- コンパイル時に直接アクセスする。 動的プロパティは、コンパイラや Visual Studio の IntelliSense からは見えません。

- .NET リフレクションを使用して実行時に検出またはアクセスする。 動的プロパティは実行時においても、CLR の基本的な意味でのプロパティではありません。

C# で動的プロパティへアクセスするには、実行時に <xref:System.ComponentModel> 名前空間の機能を使用する必要があります。

一方、XML ソースでは、次のような形式の単純な表記法を使用して動的プロパティにアクセスできます。

```xml
<object>.<dynamic-property>
```

この 2 つのクラスの動的プロパティは、1 つの値またはインデクサーのいずれかに解決されます。1 つの値の場合は直接使用できますが、インデクサーの場合は、結果の値または値のコレクションを取得するにはインデックスを渡す必要があります。 インデクサーの場合の構文は次のようになります。

```xml
<object>.<dynamic-property>[<index-value>]
```

詳細については、「[LINQ to XML の動的プロパティ](linq-to-xml-dynamic-properties.md)」を参照してください。

WPF の動的バインドを実装するには、動的プロパティを <xref:System.Windows.Data> 名前空間 (特に <xref:System.Windows.Data.Binding> クラス) の機能と共に使用します。

## <a name="see-also"></a>参照

- [LINQ to XML による WPF のデータ バインディング](wpf-data-binding-with-linq-to-xml-overview.md)
- [LINQ to XML の動的プロパティ](linq-to-xml-dynamic-properties.md)
- [WPF の XAML](../advanced/xaml-in-wpf.md)
- [データ バインド (WPF)](/dotnet/framework/wpf/data/data-binding-wpf)
- [ワークフロー マークアップの使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms735921(v=vs.90))
