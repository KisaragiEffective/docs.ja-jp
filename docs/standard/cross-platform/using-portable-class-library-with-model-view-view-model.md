---
title: Model-View-View Model を利用した汎用性のあるクラス ライブラリの使用
ms.date: 07/18/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Portable Class Library [.NET Framework], and MVVM
- MVVM, and Portable Class Library
ms.assetid: 41a0b9f8-15a2-431a-bc35-e310b2953b03
ms.openlocfilehash: f5312177b9f437d9b5474d38fca80db6fc45245b
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77123676"
---
# <a name="using-portable-class-library-with-model-view-view-model"></a>Model-View-View Model を利用した汎用性のあるクラス ライブラリの使用
.NET Framework[ポータブルクラスライブラリ](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)を使用して、モデルビュービューモデル (MVVM) パターンを実装し、複数のプラットフォーム間でアセンブリを共有できます。

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 MVVM は、基になるビジネスロジックからユーザーインターフェイスを分離するアプリケーションパターンです。 Visual Studio 2012 のポータブルクラスライブラリプロジェクトでモデルクラスとビューモデルクラスを実装し、さまざまなプラットフォーム用にカスタマイズされたビューを作成することができます。 この方法では、次の図に示すように、データモデルとビジネスロジックを1回だけ記述し、.NET Framework、Silverlight、Windows Phone、Windows 8.x ストアアプリからそのコードを使用できます。

 ![プラットフォーム間でアセンブリを共有する MVVM を使用して、ポータブルクラスライブラリを表示します。](./media/using-portable-class-library-with-model-view-view-model/mvvm-share-assemblies-across-platforms.png)

 このトピックでは、MVVM パターンに関する一般的な情報については説明しません。 ここでは、ポータブルクラスライブラリを使用して MVVM を実装する方法についてのみ説明します。 MVVM の詳細については、「 [MVVM クイック5.0 スタート](https://docs.microsoft.com/previous-versions/msp-n-p/gg430857(v=pandp.40))」を参照してください。

## <a name="classes-that-support-mvvm"></a>MVVM をサポートするクラス
 ポータブルクラスライブラリプロジェクトの .NET Framework 4.5、.NET for Windows 8.x ストアアプリ、Silverlight、Windows Phone 7.5 を対象とする場合は、MVVM パターンを実装するために次のクラスを使用できます。

- <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType> クラス

- <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType> クラス

- <xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=nameWithType> クラス

- <xref:System.Collections.Specialized.NotifyCollectionChangedAction?displayProperty=nameWithType> クラス

- <xref:System.Collections.Specialized.NotifyCollectionChangedEventArgs?displayProperty=nameWithType> クラス

- <xref:System.Collections.Specialized.NotifyCollectionChangedEventHandler?displayProperty=nameWithType> クラス

- <xref:System.ComponentModel.DataErrorsChangedEventArgs?displayProperty=nameWithType> クラス

- <xref:System.ComponentModel.INotifyDataErrorInfo?displayProperty=nameWithType> クラス

- <xref:System.ComponentModel.INotifyPropertyChanged?displayProperty=nameWithType> クラス

- <xref:System.Windows.Input.ICommand?displayProperty=nameWithType> クラス

- <xref:System.ComponentModel.DataAnnotations?displayProperty=nameWithType> 名前空間のすべてのクラス

## <a name="implementing-mvvm"></a>MVVM の実装
 ポータブルクラスライブラリプロジェクトはポータブルでないプロジェクトを参照できないため、MVVM を実装するには、通常、ポータブルクラスライブラリプロジェクトでモデルとビューモデルの両方を作成します。 モデルモデルとビューモデルは、同じプロジェクト内にあっても、別のプロジェクトにあってもかまいません。 個別のプロジェクトを使用する場合は、ビューモデルプロジェクトからモデルプロジェクトへの参照を追加します。

 モデルおよびビューモデルのプロジェクトをコンパイルした後、ビューを含むアプリでこれらのアセンブリを参照します。 ビューがビューモデルとのみ対話する場合は、ビューモデルを含むアセンブリを参照するだけで済みます。

### <a name="model"></a>モデル
 次の例は、ポータブルクラスライブラリプロジェクトに存在する可能性がある簡略化されたモデルクラスを示しています。

 [!code-csharp[PortableClassLibraryMVVM#1](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customer.cs#1)]
 [!code-vb[PortableClassLibraryMVVM#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customer.vb#1)]

 次の例は、ポータブルクラスライブラリプロジェクトのデータを設定、取得、および更新する簡単な方法を示しています。 実際のアプリでは、Windows Communication Foundation (WCF) サービスなどのソースからデータを取得します。

 [!code-csharp[PortableClassLibraryMVVM#2](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customerrepository.cs#2)]
 [!code-vb[PortableClassLibraryMVVM#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerrepository.vb#2)]

### <a name="view-model"></a>モデルの表示
 ビューモデルの基本クラスは、MVVM パターンを実装するときに頻繁に追加されます。 基底クラスの例を次に示します。

 [!code-csharp[PortableClassLibraryMVVM#3](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/viewmodelbase.cs#3)]
 [!code-vb[PortableClassLibraryMVVM#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/viewmodelbase.vb#3)]

 <xref:System.Windows.Input.ICommand> インターフェイスの実装は、MVVM パターンでよく使用されます。 <xref:System.Windows.Input.ICommand> インターフェイスを実装する例を次に示します。

 [!code-csharp[PortableClassLibraryMVVM#4](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/relaycommand.cs#4)]
 [!code-vb[PortableClassLibraryMVVM#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/relaycommand.vb#4)]

 次の例は、簡略化されたビューモデルを示しています。

 [!code-csharp[PortableClassLibraryMVVM#5](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainpageviewmodel.cs#5)]
 [!code-vb[PortableClassLibraryMVVM#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerviewmodel.vb#5)]  
  
### <a name="view"></a>表示  
 .NET Framework 4.5 アプリ、Windows 8.x ストアアプリ、Silverlight ベースのアプリ、または Windows Phone 7.5 アプリから、モデルプロジェクトとビューモデルプロジェクトを含むアセンブリを参照できます。  次に、ビューモデルと対話するビューを作成します。 次の例は、ビューモデルからデータを取得して更新する簡素化された Windows Presentation Foundation (WPF) アプリを示しています。 Silverlight、Windows Phone、または Windows 8.x ストアアプリでも同様のビューを作成できます。  
  
 [!code-xaml[PortableClassLibraryMVVM#6](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainwindow.xaml#6)]  
  
## <a name="see-also"></a>参照

- [ポータブル クラス ライブラリ](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)
