---
title: データ バインディング
ms.date: 03/30/2017
helpviewer_keywords:
- data [Windows Forms]
- Windows Forms, data binding
- data [Windows Forms], architecture
- Windows Forms controls, data binding
ms.assetid: c3826d8e-ea25-4ad4-a669-45bfb19192aa
ms.openlocfilehash: 68871db848ab46b88865e668f27f09972e8debcf
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76734617"
---
# <a name="windows-forms-data-binding"></a>Windows フォームのデータ バインディング
Windows フォームでのデータ バインディングは、データ ソースの情報をフォーム上のコントロールで表示したり変更したりする手段を提供します。 従来のデータ ソースに対してだけでなく、データを格納したほとんどすべての構造に対してバインドできます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [データ連結と Windows フォーム](data-binding-and-windows-forms.md)  
 Windows フォームのデータ バインディングの概要を説明します。  
  
 [Windows フォームがサポートするデータ ソース](data-sources-supported-by-windows-forms.md)  
 Windows フォームで使用できるデータ ソースについて説明します。  
  
 [データ連結に関連するインターフェイス](interfaces-related-to-data-binding.md)  
 Windows フォームのデータ バインドで使用されるいくつかのインターフェイスについて説明します。  
  
 [方法: Windows フォームでデータ間を移動する](how-to-navigate-data-in-windows-forms.md)  
 データ ソースの項目間を移動する方法を示します。  
  
 [Windows フォーム データ バインドの変更通知](change-notification-in-windows-forms-data-binding.md)  
 Windows フォーム データ バインドの異なる種類の変更通知について説明します。  
  
 [方法: INotifyPropertyChanged インターフェイスを実装する](how-to-implement-the-inotifypropertychanged-interface.md)  
 <xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスを実装する方法について説明します。 インターフェイスは、バインドしたコントロールを通してビジネス オブジェクトのプロパティの変更内容を通信します。  
  
 [方法: PropertyNameChanged パターンを適用する](how-to-apply-the-propertynamechanged-pattern.md)  
 *PropertyName*Changed パターンを Windows フォームユーザーコントロールのプロパティに適用する方法を示します。  
  
 [方法: ITypedList インターフェイスを実装する](how-to-implement-the-itypedlist-interface.md)  
 <xref:System.ComponentModel.ITypedList> インターフェイスを実装して、バインドできるリストのスキーマを検出できるようにする方法について説明します。  
  
 [方法: IListSource インターフェイスを実装する](how-to-implement-the-ilistsource-interface.md)  
 <xref:System.ComponentModel.IListSource> インターフェイスを実装して、<xref:System.Collections.IList> を実装する代わりに別の場所からリストを提供する、バインドできるクラスを作成する方法について説明します。  
  
 [方法: 複数のコントロールを 1 つのデータ ソースにバインドして同期状態を保つ](multiple-controls-bound-to-data-source-synchronized.md)  
 <xref:System.Windows.Forms.BindingSource.BindingComplete> イベントを処理して、データ ソースにバインドされているすべてのコントロールの同期を保つ方法について説明します。  
  
 [方法: 子テーブルの選択行が現在位置を保持することを保証する](ensure-the-selected-row-in-a-child-table-correct.md)  
 親テーブルのフィールドが変更された場合に、子テーブルの選択行が変更されないことを保証する方法について説明しています。  
  
 「[データバインディングに関連するインターフェイス](interfaces-related-to-data-binding.md)」、「[方法: Windows フォームでデータを移動](how-to-navigate-data-in-windows-forms.md)する」、および「[方法: Windows フォームに単純バインドコントロールを作成する](how-to-create-a-simple-bound-control-on-a-windows-form.md)」も参照してください。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.Binding?displayProperty=nameWithType>  
 バインドできるコンポーネントとデータ ソースの間のバインディングを表すクラスについて説明します。  
  
 <xref:System.Windows.Forms.BindingSource?displayProperty=nameWithType>  
 コントロールにバインドするためにデータ ソースをカプセル化するクラスについて説明します。  
  
## <a name="related-sections"></a>関連項目  
 [BindingSource コンポーネント](./controls/bindingsource-component.md)  
 <xref:System.Windows.Forms.BindingSource> コンポーネントの使用方法の例を示すトピックの一覧を示します。  
  
 [DataGridView コントロール](./controls/datagridview-control-windows-forms.md)  
 バインドできるデータ グリッド コントロールの使用方法の例を示すトピックの一覧を示します。  
  
 「 [Visual Studio でのデータへのアクセス](/visualstudio/data-tools/accessing-data-in-visual-studio)」も参照してください。
