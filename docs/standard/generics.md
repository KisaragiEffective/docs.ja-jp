---
title: ジェネリック型 (ジェネリック) の概要
description: 実際のデータ型をいじらずにタイプ セーフなデータ構造を定義できるコード テンプレートとしてジェネリックがどのように機能するかを説明します。
author: kuhlenh
ms.author: wiwagn
ms.date: 10/09/2018
ms.openlocfilehash: 3c1181f5be717f328ae906c6009fc8a34b904c89
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "61923852"
---
# <a name="generic-types-overview"></a>ジェネリック型の概要

開発者は、暗黙的か明示的かに関わらず、.NET では常にジェネリックを使用します。 .NET で LINQ を使用していると、<xref:System.Collections.Generic.IEnumerable%601> を操作することがあります。 または、Entity Framework を使用してデータベースと通信するための "汎用リポジトリ" のオンライン サンプルでは、ほとんどのメソッドが IQueryable\<T> を返すことに気付きます。 これらの例の **T** とは何で、なぜそこにあるのでしょうか。

**ジェネリック**は、.NET Framework 2.0 で導入され、本質的に "コード テンプレート" であり、開発者は実際のデータ型をいじらずに[タイプ セーフな](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hbzz1a9a(v=vs.100))データ構造を定義できます。 たとえば、<xref:System.Collections.Generic.List%601> は[ジェネリック コレクション](xref:System.Collections.Generic)であり、`List<int>`、`List<string>`、`List<Person>` などの任意の型で宣言および使用できます。

ジェネリックが便利な理由を理解するために、ジェネリックを追加する前と後の特定のクラス <xref:System.Collections.ArrayList> を見てみましょう。 .NET Framework 1.0 で、`ArrayList` 要素の型は <xref:System.Object> です。 これは、追加されたすべての要素は自動的に `Object` に変換されたことを意味します。 リストから要素を読み取るときも同じことが起こります。 このプロセスは[ボックス化とボックス化解除](../csharp/programming-guide/types/boxing-and-unboxing.md)と呼ばれ、パフォーマンスに影響します。 ただし、それだけでなく、コンパイル時にリスト内のデータの型を判断する方法がありません。 これは脆弱なコードを助長します。 ジェネリックは、リストの各インスタンスに含まれるデータの型を定義することで、この問題を解決します。 たとえば、`List<int>` には整数だけを追加でき、`List<Person>` には Persons だけを追加できます。

ジェネリックは、実行時にも使用できます。 これは、使用されているデータ構造の型をランタイムが認識し、より効率的メモリに格納できることを意味します。

実行時にデータ構造の型がわかるといかに効率的かということを示す小さなプログラムの例を次に示します。

```csharp
  using System;
  using System.Collections;
  using System.Collections.Generic;
  using System.Diagnostics;

  namespace GenericsExample {
    class Program {
      static void Main(string[] args) {
        //generic list
        List<int> ListGeneric = new List<int> { 5, 9, 1, 4 };
        //non-generic list
        ArrayList ListNonGeneric = new ArrayList { 5, 9, 1, 4 };
        // timer for generic list sort
        Stopwatch s = Stopwatch.StartNew();
        ListGeneric.Sort();
        s.Stop();
        Console.WriteLine($"Generic Sort: {ListGeneric}  \n Time taken: {s.Elapsed.TotalMilliseconds}ms");

        //timer for non-generic list sort
        Stopwatch s2 = Stopwatch.StartNew();
        ListNonGeneric.Sort();
        s2.Stop();
        Console.WriteLine($"Non-Generic Sort: {ListNonGeneric}  \n Time taken: {s2.Elapsed.TotalMilliseconds}ms");
        Console.ReadLine();
      }
    }
  }
```

このプログラムで、次のような出力が生成されます。

```console
Generic Sort: System.Collections.Generic.List`1[System.Int32]
 Time taken: 0.0034ms
Non-Generic Sort: System.Collections.ArrayList
 Time taken: 0.2592ms
```

最初にわかるのは、非ジェネリック リストよりジェネリック リストの方が並べ替えがはるかに高速であるということです。 また、ジェネリック リストの型が具体的 ([System.Int32]) であるのに対して、非ジェネリック リストの型は一般的であることもわかります。 ジェネリック `List<int>` の場合はランタイムはそれを <xref:System.Int32> 型と認識してメモリの整数配列にリストの要素を格納できるのに対し、非ジェネリック `ArrayList` の場合はオブジェクトに対して各リスト要素をキャストする必要があります。 この例で示すように、余分なキャストに時間がかかり、リストの並べ替え速度が低下します。

ジェネリックの型をランタイムが認識することのもう 1 つの利点は、デバッグ エクスペリエンスの向上です。 C# でジェネリックをデバッグすると、データ構造内の各要素の型がわかります。 ジェネリックでない場合は、各要素の型を知ることはできません。

## <a name="see-also"></a>参照

- [ジェネリック (C# プログラミング ガイド)](../../docs/csharp/programming-guide/generics/index.md)
