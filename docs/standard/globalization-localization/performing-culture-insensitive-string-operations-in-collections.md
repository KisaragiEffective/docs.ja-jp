---
title: カルチャの影響を受けないコレクションの操作の実行
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CaseInsensitiveComparer class, using
- CollectionsUtil.CreateCaseInsensitiveHashtable method
- culture-insensitive string operations, collections
- collections [.NET Framework], culture-insensitive string operations
- CaseInsensitiveHashCodeProvider class, using
- ArrayList.Sort method
- SortedList class, culture-insensitive string operations
- culture parameter
ms.assetid: 5cdc9396-a64b-4615-a1cd-b605db4c5983
ms.openlocfilehash: 13a9f4896a37be4297f2a1a11435b85ade381c66
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74353672"
---
# <a name="performing-culture-insensitive-string-operations-in-collections"></a>カルチャの影響を受けないコレクションの操作の実行

既定ではカルチャを認識する動作を提供するクラスとメンバーは <xref:System.Collections> 名前空間にあります。 <xref:System.Collections.CaseInsensitiveComparer> クラスおよび <xref:System.Collections.CaseInsensitiveHashCodeProvider> クラスのパラメーターなしのコンストラクターは、<xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> プロパティを使用して新しいインスタンスを初期化します。 <xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType> メソッドのすべてのオーバーロードは、既定で <xref:System.Collections.Hashtable> プロパティを使用して、`Thread.CurrentCulture` クラスの新しいインスタンスを作成します。 <xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType> メソッドのオーバーロードは、`Thread.CurrentCulture` を使用して既定でカルチャを認識した並べ替えを実行します。 <xref:System.Collections.SortedList> での並べ替えと検索は、文字列がキーとして使用されるときに、`Thread.CurrentCulture` によって影響を受けることがあります。 このセクションで説明する推奨使用方法に従うと、`Collections` 名前空間のこれらのクラスとメソッドでカルチャを認識しない結果が得られます。

> [!NOTE]
> <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> を比較メソッドに渡すと、カルチャを認識しない比較が実行されます。 ただし、これによって、ファイル パス、レジストリ キー、環境変数などで、非言語的な比較が行われることはありません。 また、比較結果に基づいたセキュリティに関する決定もサポートされません。 非言語的な比較や、結果に基づくセキュリティに関する決定については、アプリケーションは <xref:System.StringComparison> 値を受け入れる比較メソッドを使用する必要があります。 アプリケーションは <xref:System.StringComparison> を渡します。

## <a name="using-the-caseinsensitivecomparer-and-caseinsensitivehashcodeprovider-classes"></a>CaseInsensitiveComparer クラスおよび CaseInsensitiveHashCodeProvider クラスの使用

`CaseInsensitiveHashCodeProvider` および `CaseInsensitiveComparer` のパラメーターなしのコンストラクターは、`Thread.CurrentCulture` を使用してクラスの新しいインスタンスを初期化し、その結果としてカルチャを認識した動作が行われます。 次のコード例は `Hashtable` を示します。これがカルチャを認識するのは、`CaseInsensitiveHashCodeProvider` と `CaseInsensitiveComparer` のパラメーターなしのコンストラクターを使用するためです。

```vb
internalHashtable = New Hashtable(CaseInsensitiveHashCodeProvider.Default, CaseInsensitiveComparer.Default)
```

```csharp
internalHashtable = new Hashtable(CaseInsensitiveHashCodeProvider.Default, CaseInsensitiveComparer.Default);
```

`Hashtable` クラスと `CaseInsensitiveComparer` クラスを使用してカルチャを認識しない `CaseInsensitiveHashCodeProvider` を作成したい場合は、`culture` パラメーターを受け取るコンストラクターを使用してこれらのクラスの新しいインスタンスを初期化します。 `culture` パラメーターに <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> を指定してください。 カルチャを認識しない `Hashtable` を次のコード例で示します。

```vb
internalHashtable = New Hashtable(New
    CaseInsensitiveHashCodeProvider(CultureInfo.InvariantCulture),
    New CaseInsensitiveComparer(CultureInfo.InvariantCulture))
```

```csharp
internalHashtable = new Hashtable(new CaseInsensitiveHashCodeProvider
    (CultureInfo.InvariantCulture),
    new CaseInsensitiveComparer(CultureInfo.InvariantCulture));
```

## <a name="using-the-collectionsutilcreatecaseinsensitivehashtable-method"></a>CollectionsUtil.CreateCaseInsensitiveHashTable メソッドの使用

`CollectionsUtil.CreateCaseInsensitiveHashTable` メソッドは、文字列の大文字と小文字を無視する `Hashtable` クラスの新しいインスタンスを手早く作成する便利な方法です。 ただし、`CollectionsUtil.CreateCaseInsensitiveHashTable` メソッドのすべてのオーバーロードは、`Thread.CurrentCulture` プロパティを使用するためカルチャを認識します。 このメソッドを使用して、カルチャを認識しない `Hashtable` を作成することはできません。 カルチャを認識しない `Hashtable` を作成するには、`Hashtable`パラメーターを受け取る `culture` コンストラクターを使用します。 `culture` パラメーターに `CultureInfo.InvariantCulture` を指定してください。 カルチャを認識しない `Hashtable` を次のコード例で示します。

```vb
internalHashtable = New Hashtable(New
    CaseInsensitiveHashCodeProvider(CultureInfo.InvariantCulture),
    New CaseInsensitiveComparer(CultureInfo.InvariantCulture))
```

```csharp
internalHashtable = new Hashtable(new CaseInsensitiveHashCodeProvider
    (CultureInfo.InvariantCulture),
    new CaseInsensitiveComparer(CultureInfo.InvariantCulture));
```

<a name="cpconperformingculture-insensitivestringoperationsincollectionsanchor1"></a>

## <a name="using-the-sortedlist-class"></a>SortedList クラスの使用

`SortedList`は、キーによって並べ替えられ、キーとインデックスを使ってアクセスできる、キーと値のペアのコレクションを表します。 文字列がキーであるときに `SortedList` を使用すると、並べ替えと検索が `Thread.CurrentCulture` プロパティの影響を受けることがあります。 `SortedList` でカルチャを認識しない動作を実行するには、`SortedList` パラメーターを受け取るコンストラクターの 1 つを使用して `comparer` を作成します。 `comparer` パラメーターは、キーの比較に使用される <xref:System.Collections.IComparer> の実装を指定します。 このパラメーターには、キーを比較するために `CultureInfo.InvariantCulture` を使用するカスタム comparer クラスを指定してください。 次の例は、カルチャを認識しないカスタム comparer クラスです。これは `comparer`コンストラクターの `SortedList` パラメーターとして指定できます。

```vb
Imports System.Collections
Imports System.Globalization

Friend Class InvariantComparer
    Implements IComparer
    Private m_compareInfo As CompareInfo
    Friend Shared [Default] As New InvariantComparer()

    Friend Sub New()
        m_compareInfo = CultureInfo.InvariantCulture.CompareInfo
    End Sub

    Public Function Compare(a As Object, b As Object) As Integer _
            Implements IComparer.Compare
        Dim sa As String = CType(a, String)
        Dim sb As String = CType(b, String)
        If Not (sa Is Nothing) And Not (sb Is Nothing) Then
            Return m_compareInfo.Compare(sa, sb)
        Else
            Return Comparer.Default.Compare(a, b)
        End If
    End Function
End Class
```

```csharp
using System;
using System.Collections;
using System.Globalization;

internal class InvariantComparer : IComparer
{
    private CompareInfo m_compareInfo;
    internal static readonly InvariantComparer Default = new
        InvariantComparer();

    internal InvariantComparer()
    {
        m_compareInfo = CultureInfo.InvariantCulture.CompareInfo;
    }

    public int Compare(Object a, Object b)
    {
        String sa = a as String;
        String sb = b as String;
        if (sa != null && sb != null)
            return m_compareInfo.Compare(sa, sb);
        else
            return Comparer.Default.Compare(a,b);
    }
}
```

一般に、`SortedList` を文字列に対して使用する際にカスタム invariant comparer を指定しないと、リストにデータが設定された後で、`Thread.CurrentCulture` に対する変更によってリストが無効になることがあります。

## <a name="using-the-arraylistsort-method"></a>ArrayList.Sort メソッドの使用

`ArrayList.Sort` メソッドのオーバーロードは、`Thread.CurrentCulture`プロパティを使用して、カルチャを認識する並べ替えを既定で実行します。 結果は、さまざまな並べ替え順序のためカルチャによって変わることがあります。 カルチャを認識した動作を回避するには、`IComparer`実装を受け入れるこのメソッドのオーバーロードを使用します。 `comparer` パラメーターには、`CultureInfo.InvariantCulture` を使用するカスタム invariant comparer クラスを指定してください。 カスタム invariant comparer クラスの例は、「[SortedList クラスの使用](#cpconperformingculture-insensitivestringoperationsincollectionsanchor1)」のトピックをご覧ください。

## <a name="see-also"></a>参照

- <xref:System.Collections.CaseInsensitiveComparer>
- <xref:System.Collections.CaseInsensitiveHashCodeProvider>
- <xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType>
- <xref:System.Collections.SortedList>
- <xref:System.Collections.Hashtable>
- <xref:System.Collections.IComparer>
- [カルチャを認識しない文字列操作の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations.md)
- <xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType>
