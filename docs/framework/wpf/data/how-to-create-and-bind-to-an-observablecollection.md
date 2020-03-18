---
title: '方法 : ObservableCollection を作成およびバインドする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], ObservableCollection class
- notifications [WPF]
ms.assetid: 6cf7e275-df76-41c6-a611-53b889b8fd5a
ms.openlocfilehash: 596f6ae71e83c5aa3b2b80764f68a8abf08cdb7b
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73453520"
---
# <a name="how-to-create-and-bind-to-an-observablecollection"></a>方法 : ObservableCollection を作成およびバインドする
この例では、<xref:System.Collections.ObjectModel.ObservableCollection%601> クラスから派生したコレクションを作成してバインドする方法を示します。これは、項目が追加または削除されたときに通知を提供するコレクションクラスです。  
  
## <a name="example"></a>例  
 `NameList` コレクションの実装例を次に示します。  
  
```csharp  
public class NameList : ObservableCollection<PersonName>  
{  
    public NameList() : base()  
    {  
        Add(new PersonName("Willa", "Cather"));  
        Add(new PersonName("Isak", "Dinesen"));  
        Add(new PersonName("Victor", "Hugo"));  
        Add(new PersonName("Jules", "Verne"));  
    }  
  }  
  
  public class PersonName  
  {  
      private string firstName;  
      private string lastName;  
  
      public PersonName(string first, string last)  
      {  
          this.firstName = first;  
          this.lastName = last;  
      }  
  
      public string FirstName  
      {  
          get { return firstName; }  
          set { firstName = value; }  
      }  
  
      public string LastName  
      {  
          get { return lastName; }  
          set { lastName = value; }  
      }  
  }  
```  
  
```vb  
Public Class NameList  
    Inherits ObservableCollection(Of PersonName)  
  
    ' Methods  
    Public Sub New()  
        MyBase.Add(New PersonName("Willa", "Cather"))  
        MyBase.Add(New PersonName("Isak", "Dinesen"))  
        MyBase.Add(New PersonName("Victor", "Hugo"))  
        MyBase.Add(New PersonName("Jules", "Verne"))  
    End Sub  
  
End Class  
  
Public Class PersonName  
    ' Methods  
    Public Sub New(ByVal first As String, ByVal last As String)  
        Me._firstName = first  
        Me._lastName = last  
    End Sub  
  
    ' Properties  
    Public Property FirstName() As String  
        Get  
            Return Me._firstName  
        End Get  
        Set(ByVal value As String)  
            Me._firstName = value  
        End Set  
    End Property  
  
    Public Property LastName() As String  
        Get  
            Return Me._lastName  
        End Get  
        Set(ByVal value As String)  
            Me._lastName = value  
        End Set  
    End Property  
  
    ' Fields  
    Private _firstName As String  
    Private _lastName As String  
End Class  
```  
  
 「 [XAML でデータをバインドできるよう](how-to-make-data-available-for-binding-in-xaml.md)にする」で説明されているように、他の共通言語ランタイム (CLR) オブジェクトと同じように、コレクションをバインドできるようにすることができます。 たとえば、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でコレクションをインスタンス化し、次に示すように、そのコレクションをリソースとして指定します。  
  
```xaml  
<Window  
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"  
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
  xmlns:c="clr-namespace:SDKSample"  
  x:Class="SDKSample.Window1"  
  Width="400"  
  Height="280"  
  Title="MultiBinding Sample">  
  
  <Window.Resources>  
    <c:NameList x:Key="NameListData"/>  
  
...  
  
</Window.Resources>  
```  
  
 その後、コレクションにバインドできます。  
  
```xaml  
<ListBox Width="200"  
         ItemsSource="{Binding Source={StaticResource NameListData}}"  
         ItemTemplate="{StaticResource NameItemTemplate}"  
         IsSynchronizedWithCurrentItem="True"/>  
```  
  
 `NameItemTemplate` の定義は、ここには示していません。  
  
> [!NOTE]
> コレクション内のオブジェクトは、「[バインディング ソースの概要](binding-sources-overview.md)」で説明されている要件を満たす必要があります。 特に、<xref:System.Windows.Data.BindingMode.OneWay> または <xref:System.Windows.Data.BindingMode.TwoWay> を使用している場合 (ソースプロパティが動的に変更されたときに [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を更新する場合など) は、<xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスなどの適切なプロパティ変更通知機構を実装する必要があります。  
  
 詳しくは、「[データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」の「コレクションへのバインド」をご覧ください。  
  
## <a name="see-also"></a>関連項目

- [ビュー内のデータの並べ替え](how-to-sort-data-in-a-view.md)
- [ビュー内のデータをフィルター処理する](how-to-filter-data-in-a-view.md)
- [XAML でビューを使用してデータの並べ替えおよびグループ化を行う](how-to-sort-and-group-data-using-a-view-in-xaml.md)
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
