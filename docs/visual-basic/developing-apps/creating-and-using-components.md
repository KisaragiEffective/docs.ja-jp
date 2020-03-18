---
title: コンポーネントの作成および使用
ms.date: 07/20/2015
helpviewer_keywords:
- components [Visual Basic]
ms.assetid: ee6a4156-73f7-4e9b-8e01-c74c4798b65c
ms.openlocfilehash: 2fefdff9dc27915066e3d92efd8439adffed9fc5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74330294"
---
# <a name="creating-and-using-components-in-visual-basic"></a>Visual Basic でのコンポーネントの作成および使用

*コンポーネント*は、<xref:System.ComponentModel.IComponent?displayProperty=nameWithType> インターフェイスを実装するか、<xref:System.ComponentModel.IComponent> を実装するクラスから直接的または間接的に派生するクラスです。 .NET Framework コンポーネントは、再利用可能なオブジェクトであり、他のオブジェクトと対話したり、外部のリソースやデザイン時のサポートを制御したりすることができます。  
  
 コンポーネントの重要な特徴の 1 つは、コンポーネントがデザイン可能であるということです。つまり、コンポーネントであるクラスは Visual Studio 統合開発環境で使用できます。 コンポーネントは、ツールボックスへの追加、フォームへのドラッグ アンド ドロップ、デザイン サーフェイスでの操作が可能です。 コンポーネントの基本デザイン時サポートが .NET Framework に組み込まれていることに注意してください。基本デザイン時機能を利用するために、コンポーネント開発者は追加の作業を行う必要はありません。  
  
 デザイン可能という点では、"*コントロール*" もコンポーネントに似ています。 ただし、コントロールにはユーザー インターフェイスが用意されているのに対し、コンポーネントには用意されていません。 コントロールは基本コントロール クラスである <xref:System.Windows.Forms.Control> または <xref:System.Web.UI.Control> から派生する必要があります。  
  
## <a name="when-to-create-a-component"></a>コンポーネントを作成する状況  

 デザイン サーフェイス (Windows フォーム デザイナーや Web フォーム デザイナーなど) で使用するクラスにユーザー インターフェイスがない場合、そのクラスは、コンポーネントにして、<xref:System.ComponentModel.IComponent> を実装するか、<xref:System.ComponentModel.IComponent> を直接または間接的に実装するクラスから派生させる必要があります。  
  
 <xref:System.ComponentModel.Component> クラスと <xref:System.ComponentModel.MarshalByValueComponent> クラスは、<xref:System.ComponentModel.IComponent> インターフェイスの基本実装です。 これらのクラスの主な違いは、<xref:System.ComponentModel.Component> クラスは参照渡しでマーシャリングされ、<xref:System.ComponentModel.IComponent> は値渡しでマーシャリングされることにあります。 実装のためのガイドラインを次に示します。  
  
- コンポーネントを参照渡しでマーシャリングする必要がある場合は、<xref:System.ComponentModel.Component> から派生させます。  
  
- コンポーネントを値渡しでマーシャリングする必要がある場合は、<xref:System.ComponentModel.MarshalByValueComponent> から派生させます。  
  
- 単一継承が原因でいずれかの基本実装からコンポーネントを派生できない場合は、<xref:System.ComponentModel.IComponent> を実装します。  
  
## <a name="component-classes"></a>コンポーネントのクラス  

 <xref:System.ComponentModel> 名前空間は、コンポーネントとコントロールの実行時およびデザイン時の動作を実装するために使用できるクラスを提供します。 この名前空間には、属性と型コンバーターの実装、データ ソースへのバインド、コンポーネントのライセンス処理のための基底クラスと基底インターフェイスが含まれています。  
  
 核となるコンポーネント クラスは次のとおりです。  
  
- <xref:System.ComponentModel.Component> で初期化します。 <xref:System.ComponentModel.IComponent> インターフェイスの基本実装。 このクラスにより、アプリケーション間でオブジェクトの共有が可能になります。  
  
- <xref:System.ComponentModel.MarshalByValueComponent> で初期化します。 <xref:System.ComponentModel.IComponent> インターフェイスの基本実装。  
  
- <xref:System.ComponentModel.Container> で初期化します。 <xref:System.ComponentModel.IContainer> インターフェイスの基本実装。 このクラスは、0 個以上のコンポーネントをカプセル化します。  
  
 コンポーネントのライセンス処理に使用するクラスのいくつかを次に示します。  
  
- <xref:System.ComponentModel.License> で初期化します。 すべてのライセンスの抽象基底クラスです。 ライセンスは、コンポーネントの特定のインスタンスに付与されます。  
  
- <xref:System.ComponentModel.LicenseManager> で初期化します。 コンポーネントにライセンスを追加し、<xref:System.ComponentModel.LicenseProvider> を管理するためのプロパティとメソッドを提供します。  
  
- <xref:System.ComponentModel.LicenseProvider> で初期化します。 ライセンス プロバイダーを実装するための抽象基底クラスです。  
  
- <xref:System.ComponentModel.LicenseProviderAttribute> で初期化します。 クラスで使用する <xref:System.ComponentModel.LicenseProvider> クラスを指定します。  
  
 コンポーネントの説明や永続化に一般的に使用するクラスを次に示します。  
  
- <xref:System.ComponentModel.TypeDescriptor> で初期化します。 属性、プロパティ、イベントなど、コンポーネントの特性に関する情報を提供します。  
  
- <xref:System.ComponentModel.EventDescriptor> で初期化します。 イベントに関する情報を提供します。  
  
- <xref:System.ComponentModel.PropertyDescriptor> で初期化します。 プロパティに関する情報を提供します。  
  
## <a name="related-sections"></a>関連セクション  

 [コントロールとコンポーネントの作成時のトラブルシューティング](../../framework/winforms/controls/troubleshooting-control-and-component-authoring.md)  
 一般的な問題に対処する方法について説明します。  
  
## <a name="see-also"></a>参照

- [方法: Windows フォームでデザイン時サポートにアクセスする](../../framework/winforms/controls/developing-windows-forms-controls-at-design-time.md)
