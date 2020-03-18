---
title: データ連結に関連するインターフェイス
ms.date: 03/30/2017
helpviewer_keywords:
- data [Windows Forms], data-binding interfaces
- INotifyPropertyChanged interface
- IBindingListView interface
- IList interface [Windows Forms], Windows Forms data binding
- IBindingList interface [Windows Forms], Windows Forms data binding
- interfaces [Windows Forms], Windows Forms data binding
- IEditableObject interface [Windows Forms], Windows Forms data binding
- data binding [Windows Forms], interfaces
- IDataErrorInfo interface [Windows Forms], Windows Forms data binding
ms.assetid: 14e49a2e-3e46-47ca-b491-70d546333277
ms.openlocfilehash: 4e40f7ec1922cdf43e6a0b8f5734acaaeefbc514
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2019
ms.locfileid: "71834588"
---
# <a name="interfaces-related-to-data-binding"></a>データ連結に関連するインターフェイス

ADO.NET を使用すると、アプリケーションのバインドニーズと使用するデータに合わせて、さまざまなデータ構造を作成できます。 Windows フォームでデータを提供または使用するための独自のクラスを作成することもできます。 これらのオブジェクトは、基本的なデータ バインディングから、デザイン時サポートの提供、エラー チェック、変更通知、データ自体に加えられた変更の構造化されたロールバックのサポートに至るまで、さまざまなレベルの機能を提供することができ、複雑さに対応できます。

## <a name="consumers-of-data-binding-interfaces"></a>データ バインディング インターフェイスのコンシューマー

以下のセクションでは、インターフェイスオブジェクトの2つのグループについて説明します。 1 つ目のグループでは、データ ソース作成者がデータ ソースに実装するインターフェイスを示します。 これらのインターフェイスは、データ ソース コンシューマーが使用するように設計されています。ほとんどの場合、データ ソース コンシューマーは、Windows フォーム コントロールまたはコンポーネントです。 2 つ目のグループでは、コンポーネント作成者向けに設計されたインターフェイスを示します。 コンポーネント作成者は、Windows フォーム データ バインディング エンジンが使用する、データ バインディングをサポートするコンポーネントを作成するときにこれらのインターフェイスを使用します。 フォームに関連付けられたクラス内でこれらのインターフェイスを実装することで、データ バインディングを実現できます。各ケースは、データの操作を可能にするインターフェイスを実装するクラスを示しています。 Visual Studio の迅速なアプリケーション開発 (RAD) データデザインエクスペリエンスツールは、既にこの機能を利用しています。

### <a name="interfaces-for-implementation-by-data-source-authors"></a>データ ソース作成者が実装するインターフェイス

以下のインターフェイスは、Windows フォーム コントロールで使用するように設計されています。

- <xref:System.Collections.IList> インターフェイス

  <xref:System.Collections.IList> インターフェイスを実装するクラスは、<xref:System.Array>、<xref:System.Collections.ArrayList>、または <xref:System.Collections.CollectionBase>にすることができます。 これらは、<xref:System.Object>型の項目のインデックス付きリストです。 インデックスの最初の項目によって型が決定されるため、これらのリストには同種の型が含まれている必要があります。 <xref:System.Collections.IList> は、実行時にのみバインドに使用できます。

  > [!NOTE]
  > Windows フォームにバインドするビジネスオブジェクトの一覧を作成する場合は、<xref:System.ComponentModel.BindingList%601>の使用を検討する必要があります。 <xref:System.ComponentModel.BindingList%601> は、双方向の Windows フォームデータバインディングに必要なプライマリインターフェイスを実装する拡張可能なクラスです。

- <xref:System.ComponentModel.IBindingList> インターフェイス

  <xref:System.ComponentModel.IBindingList> インターフェイスを実装するクラスは、はるかに高いレベルのデータバインディング機能を提供します。 この実装では、基本的な並べ替え機能と変更通知を提供します。変更通知では、リスト項目が変更されたとき (たとえば、顧客リストの 3 番目の項目の Address フィールドが変更されたとき) と、リスト自体が変更されたとき (たとえば、リスト内の項目の数が増減したとき) のどちらの場合も変更が通知されます。 複数のコントロールを同じデータにバインドする予定であり、いずれかのコントロールで行われたデータ変更をバインドされた他のコントロールに反映させる必要がある場合に、変更通知が重要になります。

  > [!NOTE]
  > 変更通知は、<xref:System.ComponentModel.IBindingList.SupportsChangeNotification%2A> プロパティを使用して <xref:System.ComponentModel.IBindingList> インターフェイスに対して有効になります。これは `true`すると、リストが変更されたこと、またはリスト内の項目が変更されたことを示す <xref:System.ComponentModel.IBindingList.ListChanged> イベントが発生します。

  変更の種類は、<xref:System.ComponentModel.ListChangedEventArgs> パラメーターの <xref:System.ComponentModel.ListChangedType> プロパティによって記述されます。 したがって、データ モデルが更新されるたびに、依存するビュー (同じデータ ソースにバインドされた他のコントロールなど) も更新されます。 ただし、一覧に含まれるオブジェクトは、リストが <xref:System.ComponentModel.IBindingList.ListChanged> イベントを発生させることができるように、変更時にリストに通知する必要があります。

  > [!NOTE]
  > <xref:System.ComponentModel.BindingList%601> には、<xref:System.ComponentModel.IBindingList> インターフェイスの汎用実装が用意されています。

- <xref:System.ComponentModel.IBindingListView> インターフェイス

  <xref:System.ComponentModel.IBindingListView> インターフェイスを実装するクラスは、<xref:System.ComponentModel.IBindingList>の実装のすべての機能に加えて、フィルター処理や高度な並べ替え機能を提供します。 この実装では、文字列ベースのフィルター処理と、プロパティ記述子と方向のペアによる複数列の並べ替え機能を提供します。

- <xref:System.ComponentModel.IEditableObject> インターフェイス

  <xref:System.ComponentModel.IEditableObject> インターフェイスを実装するクラスを使用すると、オブジェクトに対する変更が永続的になるタイミングをオブジェクトで制御できます。 この実装により、<xref:System.ComponentModel.IEditableObject.BeginEdit%2A>、<xref:System.ComponentModel.IEditableObject.EndEdit%2A>、および <xref:System.ComponentModel.IEditableObject.CancelEdit%2A> メソッドを使用できるようになります。これにより、オブジェクトに加えられた変更をロールバックできます。 次に、<xref:System.ComponentModel.IEditableObject.BeginEdit%2A>、<xref:System.ComponentModel.IEditableObject.EndEdit%2A>、および <xref:System.ComponentModel.IEditableObject.CancelEdit%2A> の各メソッドの機能の概要と、データに加えられた変更をロールバックできるようにする方法について簡単に説明します。

  - <xref:System.ComponentModel.IEditableObject.BeginEdit%2A> メソッドは、オブジェクトに対する編集の開始を通知します。 このインターフェイスを実装するオブジェクトは、<xref:System.ComponentModel.IEditableObject.CancelEdit%2A> メソッドが呼び出された場合に更新を破棄できるように、<xref:System.ComponentModel.IEditableObject.BeginEdit%2A> メソッド呼び出しの後に更新を格納する必要があります。 データバインディング Windows フォームでは、単一の編集トランザクションのスコープ内で <xref:System.ComponentModel.IEditableObject.BeginEdit%2A> を複数回呼び出すことができます (たとえば、<xref:System.ComponentModel.IEditableObject.BeginEdit%2A>、<xref:System.ComponentModel.IEditableObject.BeginEdit%2A>、<xref:System.ComponentModel.IEditableObject.EndEdit%2A>)。 <xref:System.ComponentModel.IEditableObject> の実装では、<xref:System.ComponentModel.IEditableObject.BeginEdit%2A> が既に呼び出されているかどうかを追跡し、後続の <xref:System.ComponentModel.IEditableObject.BeginEdit%2A>の呼び出しを無視する必要があります。 このメソッドは複数回呼び出すことができるので、それ以降の呼び出しは破壊的であることが重要です。つまり、後続の <xref:System.ComponentModel.IEditableObject.BeginEdit%2A> の呼び出しでは、作成された更新を破棄したり、最初の <xref:System.ComponentModel.IEditableObject.BeginEdit%2A> 呼び出しで保存されたデータを変更したりすることはできません。

  - オブジェクトが現在編集モードである場合、<xref:System.ComponentModel.IEditableObject.EndEdit%2A> メソッドは、基になるオブジェクトに <xref:System.ComponentModel.IEditableObject.BeginEdit%2A> が呼び出された後にすべての変更をプッシュします。

  - <xref:System.ComponentModel.IEditableObject.CancelEdit%2A> メソッドは、オブジェクトに対して行われたすべての変更を破棄します。

  、 <xref:System.ComponentModel.IEditableObject.BeginEdit%2A> 、<xref:System.ComponentModel.IEditableObject.EndEdit%2A>およびの各メソッドの動作の詳細については、「<xref:System.ComponentModel.IEditableObject.CancelEdit%2A>データベースにデータを保存する[」を参照](/visualstudio/data-tools/save-data-back-to-the-database)してください。

  このトランザクションのデータ機能の概念は、<xref:System.Windows.Forms.DataGridView> コントロールによって使用されます。

- <xref:System.ComponentModel.ICancelAddNew> インターフェイス

  <xref:System.ComponentModel.ICancelAddNew> インターフェイスを実装するクラスは、通常、<xref:System.ComponentModel.IBindingList> インターフェイスを実装し、<xref:System.ComponentModel.IBindingList.AddNew%2A> メソッドを使用してデータソースに加えられた加算をロールバックできます。 データソースが <xref:System.ComponentModel.IBindingList> インターフェイスを実装している場合は、<xref:System.ComponentModel.ICancelAddNew> インターフェイスも実装する必要があります。

- <xref:System.ComponentModel.IDataErrorInfo> インターフェイス

  <xref:System.ComponentModel.IDataErrorInfo> インターフェイスを実装するクラスを使用すると、オブジェクトは、バインドされたコントロールにカスタムエラー情報を提供できます。

  - <xref:System.ComponentModel.IDataErrorInfo.Error%2A> プロパティは、一般的なエラーメッセージテキスト ("エラーが発生しました" など) を返します。

  - <xref:System.ComponentModel.IDataErrorInfo.Item%2A> プロパティは、列からの特定のエラーメッセージを含む文字列を返します (たとえば、"`State` の列の値が無効です")。

- <xref:System.Collections.IEnumerable> インターフェイス

  <xref:System.Collections.IEnumerable> インターフェイスを実装するクラスは、通常、ASP.NET によって使用されます。 このインターフェイスの Windows フォームサポートは、<xref:System.Windows.Forms.BindingSource> コンポーネントを通じてのみ使用できます。

  > [!NOTE]
  > <xref:System.Windows.Forms.BindingSource> コンポーネントは、すべての <xref:System.Collections.IEnumerable> 項目をバインドのために別のリストにコピーします。

- <xref:System.ComponentModel.ITypedList> インターフェイス

  <xref:System.ComponentModel.ITypedList> インターフェイスを実装するコレクションクラスは、バインドされたコントロールに公開されるプロパティの順序とセットを制御する機能を提供します。

  > [!NOTE]
  > <xref:System.ComponentModel.ITypedList.GetItemProperties%2A> メソッドを実装し、<xref:System.ComponentModel.PropertyDescriptor> 配列が null ではない場合、配列の最後のエントリは、項目の別のリストであるリストプロパティを記述するプロパティ記述子になります。

- <xref:System.ComponentModel.ICustomTypeDescriptor> インターフェイス

  <xref:System.ComponentModel.ICustomTypeDescriptor> インターフェイスを実装するクラスは、それ自体に関する動的な情報を提供します。 このインターフェイスは <xref:System.ComponentModel.ITypedList> に似ていますが、リストではなくオブジェクトに使用されます。 このインターフェイスは、基になる行のスキーマを射影するために <xref:System.Data.DataRowView> によって使用されます。 <xref:System.ComponentModel.ICustomTypeDescriptor> の単純な実装は、<xref:System.ComponentModel.CustomTypeDescriptor> クラスによって提供されます。

  > [!NOTE]
  > <xref:System.ComponentModel.ICustomTypeDescriptor>を実装する型へのデザイン時バインドをサポートするには、型も <xref:System.ComponentModel.IComponent> を実装し、フォームのインスタンスとして存在する必要があります。

- <xref:System.ComponentModel.IListSource> インターフェイス

  <xref:System.ComponentModel.IListSource> インターフェイスを実装するクラスは、リスト以外のオブジェクトでリストベースのバインドを有効にします。 <xref:System.ComponentModel.IListSource> の <xref:System.ComponentModel.IListSource.GetList%2A> メソッドは、<xref:System.Collections.IList>から継承しないオブジェクトからバインド可能なリストを返すために使用されます。 <xref:System.ComponentModel.IListSource> は <xref:System.Data.DataSet> クラスによって使用されます。

- <xref:System.ComponentModel.IRaiseItemChangedEvents> インターフェイス

  <xref:System.ComponentModel.IRaiseItemChangedEvents> インターフェイスを実装するクラスは、<xref:System.ComponentModel.IBindingList> インターフェイスも実装するバインド可能なリストです。 このインターフェイスは、型が <xref:System.ComponentModel.IRaiseItemChangedEvents.RaisesItemChangedEvents%2A> プロパティを使用して <xref:System.ComponentModel.ListChangedType.ItemChanged> 型のイベント <xref:System.ComponentModel.IBindingList.ListChanged> を発生させるかどうかを示すために使用されます。

  > [!NOTE]
  > 前に説明したイベント変換を一覧表示し、<xref:System.Windows.Forms.BindingSource> コンポーネントと対話するプロパティがデータソースに用意されている場合は、<xref:System.ComponentModel.IRaiseItemChangedEvents> を実装する必要があります。 それ以外の場合、<xref:System.Windows.Forms.BindingSource> は、イベント変換を一覧表示してパフォーマンスが低下するプロパティも実行します。

- <xref:System.ComponentModel.ISupportInitialize> インターフェイス

  <xref:System.ComponentModel.ISupportInitialize> インターフェイスを実装するコンポーネントには、プロパティの設定や共同依存プロパティの初期化のためのバッチ最適化の利点があります。 <xref:System.ComponentModel.ISupportInitialize> には、次の2つのメソッドがあります。

  - <xref:System.ComponentModel.ISupportInitialize.BeginInit%2A> は、オブジェクトの初期化が開始されていることを通知します。

  - <xref:System.ComponentModel.ISupportInitialize.EndInit%2A> は、オブジェクトの初期化が完了していることを通知します。

- <xref:System.ComponentModel.ISupportInitializeNotification> インターフェイス

  <xref:System.ComponentModel.ISupportInitializeNotification> インターフェイスを実装するコンポーネントは、<xref:System.ComponentModel.ISupportInitialize> インターフェイスも実装します。 このインターフェイスを使用すると、初期化が完了したことを他の <xref:System.ComponentModel.ISupportInitialize> コンポーネントに通知できます。 <xref:System.ComponentModel.ISupportInitializeNotification> インターフェイスには、次の2つのメンバーが含まれます。

  - <xref:System.ComponentModel.ISupportInitializeNotification.IsInitialized%2A> は、コンポーネントが初期化されているかどうかを示す `boolean` 値を返します。

  - <xref:System.ComponentModel.ISupportInitializeNotification.Initialized> は <xref:System.ComponentModel.ISupportInitialize.EndInit%2A> が呼び出されたときに発生します。

- <xref:System.ComponentModel.INotifyPropertyChanged> インターフェイス

  このインターフェイスを実装するクラスは、プロパティ値のいずれかが変更されたときにイベントを発生させる型です。 このインターフェイスは、コントロールのプロパティごとに変更イベントを持つパターンを置き換えるように設計されています。 <xref:System.ComponentModel.BindingList%601>で使用する場合、ビジネスオブジェクトは <xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスを実装する必要があります。また、BindingList\`1 は <xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged> イベントを <xref:System.ComponentModel.BindingList%601.ListChanged> 型のイベントに変換します。<xref:System.ComponentModel.ListChangedType.ItemChanged>

  > [!NOTE]
  > バインドされたクライアントとデータソースの間のバインディングで変更通知が発生するようにするには、バインドされたデータソースの型で <xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスを実装するか (推奨)、バインドされた型に*propertyName*`Changed` イベントを指定する必要がありますが、両方を実行することはできません。

### <a name="interfaces-for-implementation-by-component-authors"></a>コンポーネント作成者が実装するインターフェイス

以下のインターフェイスは、Windows フォーム データ バインディング エンジンが使用するように設計されています。

- <xref:System.Windows.Forms.IBindableComponent> インターフェイス

  このインターフェイスを実装するクラスは、データ バインディングをサポートするコントロール以外のコンポーネントです。 このクラスは、このインターフェイスの <xref:System.Windows.Forms.IBindableComponent.DataBindings%2A> および <xref:System.Windows.Forms.IBindableComponent.BindingContext%2A> プロパティを使用して、コンポーネントのデータバインディングとバインディングコンテキストを返します。

  > [!NOTE]
  > コンポーネントが <xref:System.Windows.Forms.Control>から継承している場合は、<xref:System.Windows.Forms.IBindableComponent> インターフェイスを実装する必要はありません。

- <xref:System.Windows.Forms.ICurrencyManagerProvider> インターフェイス

  <xref:System.Windows.Forms.ICurrencyManagerProvider> インターフェイスを実装するクラスは、この特定のコンポーネントに関連付けられているバインディングを管理するための独自の <xref:System.Windows.Forms.CurrencyManager> を提供するコンポーネントです。 カスタム <xref:System.Windows.Forms.CurrencyManager> へのアクセスは、<xref:System.Windows.Forms.ICurrencyManagerProvider.CurrencyManager%2A> プロパティによって提供されます。

  > [!NOTE]
  > <xref:System.Windows.Forms.Control> から継承するクラスは、<xref:System.Windows.Forms.Control.BindingContext%2A> プロパティを通じてバインドを自動的に管理します。そのため、<xref:System.Windows.Forms.ICurrencyManagerProvider> を実装する必要がある場合は非常にまれです。

## <a name="see-also"></a>参照

- [データ連結と Windows フォーム](data-binding-and-windows-forms.md)
- [方法: Windows フォームに単純バインド コントロールを作成する](how-to-create-a-simple-bound-control-on-a-windows-form.md)
- [Windows フォームでのデータ バインディング](windows-forms-data-binding.md)
