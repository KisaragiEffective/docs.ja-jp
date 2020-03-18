---
title: LINQ (統合言語クエリ)
description: LINQ が言語レベルのクエリ機能と、表現力豊かな宣言コードを記述する方法として C# および Visual Basic に API を提供する方法を説明します。
author: cartermp
ms.author: wiwagn
ms.date: 06/20/2016
dev_langs:
- csharp
- vb
ms.technology: dotnet-standard
ms.assetid: c00939e1-59e3-4e61-8fe9-08ad6b3f1295
ms.openlocfilehash: eafd8f78c3d8de1ba064021111f869571d5a570f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160327"
---
# <a name="linq-language-integrated-query"></a>LINQ (統合言語クエリ)

## <a name="what-is-it"></a>紹介

LINQ では、言語レベルのクエリ機能と、表現力豊かな宣言コードを記述する方法として C# および Visual Basic に[高階関数](https://en.wikipedia.org/wiki/Higher-order_function) API が提供されます。

言語レベルのクエリ構文:

```csharp
var linqExperts = from p in programmers
                  where p.IsNewToLINQ
                  select new LINQExpert(p);
```

```vb
Dim linqExperts = From p in programmers
                  Where p.IsNewToLINQ
                  Select New LINQExpert(p)
```

上記を `IEnumerable<T>` API を使用して表した場合の例:

```csharp
var linqExperts = programmers.Where(p => p.IsNewToLINQ)
                             .Select(p => new LINQExpert(p));
```

```vb
Dim linqExperts = programmers.Where(Function(p) p.IsNewToLINQ).
                             Select(Function(p) New LINQExpert(p))
```

## <a name="linq-is-expressive"></a>LINQ は表現力が豊か

たとえば、ペットのリストはあるが、`RFID` 値で直接ペットにアクセスできる辞書に変換する必要があるとします。

従来の命令型コード:

```csharp
var petLookup = new Dictionary<int, Pet>();

foreach (var pet in pets)
{
    petLookup.Add(pet.RFID, pet);
}
```

```vb
Dim petLookup = New Dictionary(Of Integer, Pet)()

For Each pet in pets
    petLookup.Add(pet.RFID, pet)
Next
```

コードの目的は、新しい `Dictionary<int, Pet>` を作成し、ループを使用して追加することではなく、既存のリストを辞書に変換することです。 LINQ ではこの目的が維持されますが、命令型コードでは維持されません。

同等の LINQ 式:

```csharp
var petLookup = pets.ToDictionary(pet => pet.RFID);
```

```vb
Dim petLookup = pets.ToDictionary(Function(pet) pet.RFID)
```

プログラマーとして考えると、目的とコードを同等にするため、LINQ を使用するコードのほうが有益です。 また、コードが簡潔であるという利点があります。 上記のように、コードベースの大部分を 3 分の 1 に減らせることを考えれば、 賢明な選択です。

## <a name="linq-providers-simplify-data-access"></a>LINQ プロバイダーでデータ アクセスが簡単に

実環境の非常に多くのソフトウェアでは、すべてのことが一部のソース (データベース、JSON、XML など) からのデータ処理を中心に展開されます。 多くの場合、これには面倒なデータ ソースごとの新しい API の学習が含まれます。 LINQ は、データ アクセスの一般的な要素を、選択されたデータ ソースに関係なく同じようなクエリ構文に抽象化することで、これを簡略化します。

たとえば、特定の属性値を持つ XML 要素をすべて検索するとします。

```csharp
public static IEnumerable<XElement> FindAllElementsWithAttribute(XElement documentRoot, string elementName,
                                           string attributeName, string value)
{
    return from el in documentRoot.Elements(elementName)
           where (string)el.Element(attributeName) == value
           select el;
}
```

```vb
Public Shared Function FindAllElementsWithAttribute(documentRoot As XElement, elementName As String,
                                           attributeName As String, value As String) As IEnumerable(Of XElement)
    Return From el In documentRoot.Elements(elementName)
           Where el.Element(attributeName).ToString() = value
           Select el
End Function

```

コードを記述して XML ドキュメントを手動でスキャンし、このタスクを実行するのはとても難しいことです。

XML との対話が、LINQ プロバイダーでできる唯一のことではありません。 [Linq to SQL](../../docs/framework/data/adonet/sql/linq/index.md) は、MSSQL Server データベースの必要最低限のオブジェクト リレーショナル マッパー (ORM) です。 [JSON.NET](https://www.newtonsoft.com/json/help/html/LINQtoJSON.htm) ライブラリでは、LINQ を使用して効率的に JSON ドキュメントをトラバースできます。 さらに、必要な作業を行うライブラリがない場合は、[独自の LINQ プロバイダーを記述する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2012/bb546158(v=vs.110))こともできます。

## <a name="why-use-the-query-syntax"></a>クエリ構文を使用する理由

これはよくある質問です。 いずれにせよ、

```csharp
var filteredItems = myItems.Where(item => item.Foo);
```

```vb
Dim filteredItems = myItems.Where(Function(item) item.Foo)
```

上の構文は下の構文よりもずっと簡潔です。

```csharp
var filteredItems = from item in myItems
                    where item.Foo
                    select item;
```

```vb
Dim filteredItems = From item In myItems
                    Where item.Foo
                    Select item
```

API 構文は単に、クエリ構文を実行するより簡潔な方法であるだけでしょうか?

いいえ。 クエリ構文では **let** 句を使用できます。したがって、式の後ろの部分でこの句を使用すれば、式のスコープ内で変数を導入してバインドすることができます。 API 構文だけでも同じコードを再現できますが、コードが読み取りにくくなる可能性が高くなります。

そこで、**クエリ構文を使用する必要があるかどうか**ですが、

次のような場合には、使用する必要が**あります**。

* 既存のコードベースで既にクエリ構文を使用している。
* 複雑になるため、クエリ内で変数をスコープする必要がある。
* クエリ構文が好ましく、コードベースから注意がそれることはない。

次のような場合には、使用する必要は**ありません**。

* 既存のコードベースで既に API 構文を使用している。
* クエリ内で変数をスコープする必要はない。
* API 構文が好ましく、コードベースから注意がそれることはない。

## <a name="essential-samples"></a>重要なサンプル

LINQ サンプルの一覧については、「[101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)」 (101 個の LINQ サンプル) を参照してください。

以下に、LINQ の重要な要素をいくつか簡単に示します。 これは決して包括的なものではありません。LINQ ではここで紹介するものよりはるかに多くの機能が提供されます。

* 最も基本的かつ重要な要素 - `Where`、`Select`、および `Aggregate`:

```csharp
// Filtering a list.
var germanShepards = dogs.Where(dog => dog.Breed == DogBreed.GermanShepard);

// Using the query syntax.
var queryGermanShepards = from dog in dogs
                          where dog.Breed == DogBreed.GermanShepard
                          select dog;

// Mapping a list from type A to type B.
var cats = dogs.Select(dog => dog.TurnIntoACat());

// Using the query syntax.
var queryCats = from dog in dogs
                select dog.TurnIntoACat();

// Summing the lengths of a set of strings.
int seed = 0;
int sumOfStrings = strings.Aggregate(seed, (s1, s2) => s1.Length + s2.Length);
```

```vb
' Filtering a list.
Dim germanShepards = dogs.Where(Function(dog) dog.Breed = DogBreed.GermanShepard)

' Using the query syntax.
Dim queryGermanShepards = From dog In dogs
                          Where dog.Breed = DogBreed.GermanShepard
                          Select dog

' Mapping a list from type A to type B.
Dim cats = dogs.Select(Function(dog) dog.TurnIntoACat())

' Using the query syntax.
Dim queryCats = From dog In dogs
                Select dog.TurnIntoACat()

' Summing the lengths of a set of strings.
Dim seed As Integer = 0
Dim sumOfStrings As Integer = strings.Aggregate(seed, Function(s1, s2) s1.Length + s2.Length)
```

* リストをまとめてフラット化する場合:

```csharp
// Transforms the list of kennels into a list of all their dogs.
var allDogsFromKennels = kennels.SelectMany(kennel => kennel.Dogs);
```

```vb
' Transforms the list of kennels into a list of all their dogs.
Dim allDogsFromKennels = kennels.SelectMany(Function(kennel) kennel.Dogs)
```

* 2 つのセットの和集合 (カスタム比較子を含む):

```csharp
public class DogHairLengthComparer : IEqualityComparer<Dog>
{
    public bool Equals(Dog a, Dog b)
    {
        if (a == null && b == null)
        {
            return true;
        }
        else if ((a == null && b != null) ||
                 (a != null && b == null))
        {
            return false;
        }
        else
        {
            return a.HairLengthType == b.HairLengthType;
        }
    }

    public int GetHashCode(Dog d)
    {
        // Default hashcode is enough here, as these are simple objects.
        return d.GetHashCode();
    }
}

...

// Gets all the short-haired dogs between two different kennels.
var allShortHairedDogs = kennel1.Dogs.Union(kennel2.Dogs, new DogHairLengthComparer());
```

```vb
Public Class DogHairLengthComparer
  Inherits IEqualityComparer(Of Dog)

  Public Function Equals(a As Dog,b As Dog) As Boolean
      If a Is Nothing AndAlso b Is Nothing Then
          Return True
      ElseIf (a Is Nothing AndAlso b IsNot Nothing) OrElse (a IsNot Nothing AndAlso b Is Nothing) Then
          Return False
      Else
          Return a.HairLengthType = b.HairLengthType
      End If
  End Function

  Public Function GetHashCode(d As Dog) As Integer
      ' Default hashcode is enough here, as these are simple objects.
      Return d.GetHashCode()
  End Function
End Class

...

' Gets all the short-haired dogs between two different kennels.
Dim allShortHairedDogs = kennel1.Dogs.Union(kennel2.Dogs, New DogHairLengthComparer())
```

* 2 つのセットの積集合:

```csharp
// Gets the volunteers who spend share time with two humane societies.
var volunteers = humaneSociety1.Volunteers.Intersect(humaneSociety2.Volunteers,
                                                     new VolunteerTimeComparer());
```

```vb
' Gets the volunteers who spend share time with two humane societies.
Dim volunteers = humaneSociety1.Volunteers.Intersect(humaneSociety2.Volunteers,
                                                     New VolunteerTimeComparer())
```

* 並べ替え:

```csharp
// Get driving directions, ordering by if it's toll-free before estimated driving time.
var results = DirectionsProcessor.GetDirections(start, end)
              .OrderBy(direction => direction.HasNoTolls)
              .ThenBy(direction => direction.EstimatedTime);
```

```vb
' Get driving directions, ordering by if it's toll-free before estimated driving time.
Dim results = DirectionsProcessor.GetDirections(start, end).
                OrderBy(Function(direction) direction.HasNoTolls).
                ThenBy(Function(direction) direction.EstimatedTime)
```

* 最後に、より高度なサンプルを以下に示します。同じ型の 2 つのインスタンスのプロパティ値が等しいかどうかを判断します ([この StackOverflow の投稿](https://stackoverflow.com/a/844855)から借用し、変更したもの)。

```csharp
public static bool PublicInstancePropertiesEqual<T>(this T self, T to, params string[] ignore) where T : class
{
    if (self == null || to == null)
    {
        return self == to;
    }

    // Selects the properties which have unequal values into a sequence of those properties.
    var unequalProperties = from property in typeof(T).GetProperties(BindingFlags.Public | BindingFlags.Instance)
                            where !ignore.Contains(property.Name)
                            let selfValue = property.GetValue(self, null)
                            let toValue = property.GetValue(to, null)
                            where !Equals(selfValue, toValue)
                            select property;
    return !unequalProperties.Any();
}
```

```vb
<System.Runtime.CompilerServices.Extension()>
Public Function PublicInstancePropertiesEqual(Of T As Class)(self As T, [to] As T, ParamArray ignore As String()) As Boolean
    If self Is Nothing OrElse [to] Is Nothing Then
        Return self Is [to]
    End If

    ' Selects the properties which have unequal values into a sequence of those properties.
    Dim unequalProperties = From [property] In GetType(T).GetProperties(BindingFlags.Public Or BindingFlags.Instance)
                            Where Not ignore.Contains([property].Name)
                            Let selfValue = [property].GetValue(self, Nothing)
                            Let toValue = [property].GetValue([to], Nothing)
                            Where Not Equals(selfValue, toValue) Select [property]
    Return Not unequalProperties.Any()
End Function
```

## <a name="plinq"></a>PLINQ

PLINQ (Parallel LINQ) は、LINQ 式の並列実行エンジンです。 つまり、LINQ の正規表現は、任意の数のスレッドで普通に並列化できます。 これは、式の前に `AsParallel()` を指定して呼び出すことで実行できます。

次の点を考慮します。

```csharp
public static string GetAllFacebookUserLikesMessage(IEnumerable<FacebookUser> facebookUsers)
{
    var seed = default(UInt64);

    Func<UInt64, UInt64, UInt64> threadAccumulator = (t1, t2) => t1 + t2;
    Func<UInt64, UInt64, UInt64> threadResultAccumulator = (t1, t2) => t1 + t2;
    Func<Uint64, string> resultSelector = total => $"Facebook has {total} likes!";

    return facebookUsers.AsParallel()
                        .Aggregate(seed, threadAccumulator, threadResultAccumulator, resultSelector);
}
```

```vb
Public Shared GetAllFacebookUserLikesMessage(facebookUsers As IEnumerable(Of FacebookUser)) As String
{
    Dim seed As UInt64 = 0

    Dim threadAccumulator As Func(Of UInt64, UInt64, UInt64) = Function(t1, t2) t1 + t2
    Dim threadResultAccumulator As Func(Of UInt64, UInt64, UInt64) = Function(t1, t2) t1 + t2
    Dim resultSelector As Func(Of Uint64, string) = Function(total) $"Facebook has {total} likes!"

    Return facebookUsers.AsParallel().
                        Aggregate(seed, threadAccumulator, threadResultAccumulator, resultSelector)
}
```

このコードでは、必要に応じてシステム スレッドにまたがる `facebookUsers` をパーティション分割し、各スレッドの "いいね!" の数を合計し、スレッドごとの計算結果を合計して、その結果を適切な文字列に投影します。

図で表すと次のようになります。

![PLINQ の図](./media/using-linq/plinq-diagram.png)

LINQ で簡単に表すことができる (つまり、純粋関数で副作用のない) 並列化可能な CPU 制約のあるジョブは、PLINQ の候補として最適です。 副作用の_ある_ジョブの場合は、[タスク並列ライブラリ](./parallel-programming/task-parallel-library-tpl.md)の使用を検討してください。

## <a name="further-resources"></a>他のリソース:

* [101 個の LINQ サンプル](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)
* [Linqpad](https://www.linqpad.net/)。プレイグラウンド環境とデータベース クエリ エンジン (C#/F#/Visual Basic 用)
* [EduLinq](https://codeblog.jonskeet.uk/2011/02/23/reimplementing-linq-to-objects-part-45-conclusion-and-list-of-posts/)。LINQ to Objects の実装方法を学習するための電子書籍
