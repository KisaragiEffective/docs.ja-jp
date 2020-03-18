---
title: 厳密に型指定されたデリゲート
description: デリゲートを必要とする機能を作成するときに、汎用デリゲート型を使用してカスタム型を宣言する方法について説明します。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 564a683d-352b-4e57-8bac-b466529daf6b
ms.openlocfilehash: 798e8b597389bc99d10e587ec417a4e717f28abc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146204"
---
# <a name="strongly-typed-delegates"></a>厳密に型指定されたデリゲート

[[戻る]](delegate-class.md)

前の記事では、`delegate` キーワードを使って実際にデリゲート型を作成する方法を見てきました。

抽象型の Delegate クラスは、疎な結合と呼び出しのための基盤となるものです。 一方、具体的な Delegate 型は、デリゲート オブジェクトの呼び出しリストに追加されるメソッドのタイプ セーフを促し、また強制的に適用することによって、その利便性を大きく高めています。 `delegate` キーワードを使用し、具体的なデリゲート型を定義すると、それらのメソッドがコンパイラによって生成されます。

しかし実際にやってみると、異なるメソッド シグネチャが必要になるたびに新しいデリゲート型を作成することになり、 次第にわずらわしくなってくるものです。 新しい機能を追加するたびに、新しいデリゲート型が必要になるからです。

さいわい、その必要はありません。 .NET Core フレームワークには、デリゲート型が必要になったときにいつでも再利用できる型がいくつか用意されています。 それが[ジェネリック](programming-guide/generics/index.md)定義です。新しいメソッドの宣言が必要になったときは、カスタマイズしたものを宣言することができます。

その筆頭となる型が <xref:System.Action> 型で、いくつかのバリエーションも存在します。

```csharp
public delegate void Action();
public delegate void Action<in T>(T arg);
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
// Other variations removed for brevity.
```

ジェネリック型引数の `in` 修飾子については、共変性についての記事で取り上げます。

`Action` デリゲートには、最大 16 の引数を含むバリエーションが存在します (例: <xref:System.Action%6016>)。
これらの定義では、デリゲート引数のそれぞれに異なるジェネリック引数が使われているという点が重要です。それによってきわめて高い柔軟性が実現されているからです。 メソッドの一連の引数は、必ずしも同じ型である必要はありませんが、同じ型である場合もあります。

戻り値を持たないデリゲート型には、いずれかの `Action` 型を使用します。

フレームワークには、戻り値を持つデリゲート型に使用できる汎用デリゲート型もいくつか用意されています。

```csharp
public delegate TResult Func<out TResult>();
public delegate TResult Func<in T1, out TResult>(T1 arg);
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
// Other variations removed for brevity
```

ジェネリック型引数 result の `out` 修飾子については、共変性についての記事で取り上げます。

`Func` デリゲートには、最大 16 の入力引数を含むバリエーションが存在します (例: <xref:System.Func%6017>)。
慣例上、`Func` に宣言されているすべての型パラメーターの最後に出現する型が常に実行結果の型になります。

値を返すデリゲート型には、いずれかの `Func` 型を使用します。

また、単一の値に対する判定結果を返すデリゲートに特化された <xref:System.Predicate%601>
という型も存在します。

```csharp
public delegate bool Predicate<in T>(T obj);
```

どのような `Predicate` 型でも、構造的に同等な `Func` 型が存在することにお気付きでしょうか。その例を次に示します。

```csharp
Func<string, bool> TestForString;
Predicate<string> AnotherTestForString;
```

これら 2 つの型は等価と思うかもしれません。 しかし、そうではありません。
これら 2 つの変数を相互に置き換えて使用することはできません。 一方の型の変数をもう一方の型に代入することはできないのです。 C# の型システムで使用されるのは、定義されている型の名前であって、構造ではありません。

これらのデリゲート型の定義はすべて .NET Core ライブラリに存在します。つまり、デリゲートを必要とする新しい機能を作成するたびに新たにデリゲート型を定義する必要はないということです。 ほとんどの状況で、皆さんが必要になるデリゲート型はすべて、これらのジェネリック定義でまかなうことができるはずです。 必要な型パラメーターを指定すれば、それらの型のいずれかをインスタンス化することができます。 汎用化できるアルゴリズムの場合、これらのデリゲートをジェネリック型として使用することが可能です。

そうすることで無駄な時間をなくし、デリゲートを使用するうえで新たに作成する必要のある型の数を抑えることができます。

次の記事では、実際にデリゲートを扱うための一般的なパターンをいくつか見ていきます。

[次へ](delegates-patterns.md)
