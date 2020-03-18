---
title: コレクション型依存関係プロパティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties [WPF], dependency
- properties [WPF], collection-type
- dependency properties [WPF]
- collection-type properties [WPF]
ms.assetid: 99f96a42-3ab7-4f64-a16b-2e10d654e97c
ms.openlocfilehash: 039ae0cb314eba2f1bb3e5b39f2127a5e694f334
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73974154"
---
# <a name="collection-type-dependency-properties"></a>コレクション型依存関係プロパティ
ここでは、プロパティの型がコレクション型である場合に依存関係プロパティを実装する方法についての、ガイダンスと推奨されるパターンを示します。  

<a name="implementing"></a>   
## <a name="implementing-a-collection-type-dependency-property"></a>コレクション型依存関係プロパティの実装  
 一般に、依存関係プロパティの場合は、CLR プロパティラッパーを定義します。このラッパーでは、フィールドやその他のコンストラクトではなく <xref:System.Windows.DependencyProperty> 識別子によってプロパティがサポートされます。 コレクション型プロパティを実装するときは、これと同じパターンに従います。 ただし、コレクション型のプロパティは、コレクション内に含まれる型自体が <xref:System.Windows.DependencyObject> または <xref:System.Windows.Freezable> 派生クラスである場合に、パターンの複雑さをいくつか生み出します。  
  
<a name="initializing"></a>   
## <a name="initializing-the-collection-beyond-the-default-value"></a>既定値を上回るコレクションの初期化  
 依存関係プロパティを作成するときは、初期フィールド値としてプロパティの既定値を指定しません。 代わりに、依存関係プロパティのメタデータを使用して既定値を指定します。 プロパティが参照型の場合、依存関係プロパティのメタデータで指定する既定値はインスタンスごとの既定値ではありません。その型のすべてのインスタンスに適用される既定値です。 したがって、コレクション プロパティ メタデータによって定義される単一の静的コレクションを、その型の新しく作成されるインスタンスの作業既定値として使用しないように注意してください。 代わりに、クラス コンストラクターのロジックの一部として、コレクションの値に一意 (インスタンス) のコレクションを意図的に設定する必要があります。 それ以外の場合は、意図しないシングルトン クラスを作成することになります。  
  
 例を次に示します。 この例の次のセクションでは、`Aquarium`クラスの定義を示します。これには、既定値を持つ欠陥が含まれています。 クラスは、コレクション型の依存関係プロパティ `AquariumObjects`を定義します。このプロパティは、ジェネリック <xref:System.Collections.Generic.List%601> 型を <xref:System.Windows.FrameworkElement> 型制約と共に使用します。 依存関係プロパティの <xref:System.Windows.DependencyProperty.Register%28System.String%2CSystem.Type%2CSystem.Type%2CSystem.Windows.PropertyMetadata%29> の呼び出しでは、メタデータによって既定値が新しいジェネリック <xref:System.Collections.Generic.List%601>に設定されます。

> [!WARNING]
> 次のコードは正しく動作しません。

 [!code-csharp[PropertiesOvwSupport2#CollectionProblemDefinition](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport2/CSharp/page.xaml.cs#collectionproblemdefinition)]
 [!code-vb[PropertiesOvwSupport2#CollectionProblemDefinition](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport2/visualbasic/page.xaml.vb#collectionproblemdefinition)]  
  
 ただし、コードをこのままにすると、単一のリストの既定値が `Aquarium` のすべてのインスタンスで共有されます。 次のテスト コードは、2 つの異なる `Aquarium` インスタンスをインスタンス化し、それぞれに単一の異なる `Fish` を追加する方法を示していますが、これを実行すると、想定外の結果になります。  
  
 [!code-csharp[PropertiesOvwSupport#CollectionProblemTestCode](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page4.xaml.cs#collectionproblemtestcode)]
 [!code-vb[PropertiesOvwSupport#CollectionProblemTestCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page4.xaml.vb#collectionproblemtestcode)]  
  
 各コレクションのカウントが 1 になるのではなく、いずれのコレクションもカウントが 2 になります。 これは、各 `Aquarium` で `Fish` が既定値のコレクションに追加されますが、その起因となっているのがメタデータでの単一のコンストラクター呼び出しであり、したがって、すべてのインスタンス間で共有されるためです。 これは望ましい状況ではありません。  
  
 この問題を解決するには、クラス コンストラクターの呼び出しの一部として、コレクションの依存関係プロパティの値を一意のインスタンスにリセットする必要があります。 プロパティは読み取り専用の依存関係プロパティであるため、クラス内でのみアクセス可能な <xref:System.Windows.DependencyPropertyKey> を使用して、<xref:System.Windows.DependencyObject.SetValue%28System.Windows.DependencyPropertyKey%2CSystem.Object%29> メソッドを使用して設定します。  
  
 [!code-csharp[PropertiesOvwSupport#CollectionProblemCtor](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page4.xaml.cs#collectionproblemctor)]
 [!code-vb[PropertiesOvwSupport#CollectionProblemCtor](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page4.xaml.vb#collectionproblemctor)]  
  
 再びテスト コードを実行すると、結果は期待したものに近くなり、各 `Aquarium` が独自の一意のコレクションをサポートします。  
  
 コレクション プロパティを読み取り/書き込みにすると、このパターンには若干のバリエーションが発生します。 その場合は、コンストラクターからパブリック set アクセサーを呼び出して初期化を行うことができます。この場合も、パブリック <xref:System.Windows.DependencyProperty> 識別子を使用して、set ラッパー内の <xref:System.Windows.DependencyObject.SetValue%28System.Windows.DependencyProperty%2CSystem.Object%29> の非キー署名を呼び出します。  
  
## <a name="reporting-binding-value-changes-from-collection-properties"></a>コレクション プロパティからのバインディング値変更の報告  
 それ自体が依存関係プロパティであるコレクション プロパティは、変更をサブプロパティに自動的には報告しません。 コレクションにバインディングを作成している場合は、これによってバインディングが変更を報告しないことがあり、一部のデータ バインディング シナリオが無効になります。 ただし、コレクション型 <xref:System.Windows.FreezableCollection%601> をコレクション型として使用すると、コレクション内の含まれている要素に対するサブプロパティの変更が適切に報告され、バインディングは正常に機能します。  
  
 依存関係オブジェクトコレクションでサブプロパティバインドを有効にするには、コレクションプロパティを型 <xref:System.Windows.FreezableCollection%601>として作成し、そのコレクションの型制約を任意の <xref:System.Windows.DependencyObject> 派生クラスに設定します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.FreezableCollection%601>
- [WPF における XAML とカスタム クラス](xaml-and-custom-classes-for-wpf.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
- [依存関係プロパティのメタデータ](dependency-property-metadata.md)
