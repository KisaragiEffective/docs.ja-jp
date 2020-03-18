---
title: 例外とパフォーマンス
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- tester-doer pattern
- TryParse pattern
- exceptions, throwing
- exceptions, performance
- throwing exceptions, performance
ms.assetid: 3ad6aad9-08e6-4232-b336-0e301f2493e6
ms.openlocfilehash: afa4e748599781a5979823320d8913ff5357d415
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741646"
---
# <a name="exceptions-and-performance"></a>例外とパフォーマンス
例外に関する一般的な懸念事項の1つとして、定期的に失敗するコードに例外を使用すると、実装のパフォーマンスが許容されなくなることがあります。 これは有効な問題です。 メンバーが例外をスローすると、パフォーマンスが低下することがあります。 ただし、エラーコードの使用を禁止する例外ガイドラインに厳密に準拠しながら、パフォーマンスを向上させることができます。 このセクションで説明する2つのパターンは、この方法を示しています。

 例外がパフォーマンスに悪影響を与える可能性があるため、エラーコードを使用しないように ❌ ます。

 パフォーマンスを向上させるために、次の 2 つのセクションで説明するように、Tester-Doer パターンまたは Try-Parse パターンを使用することができます。

## <a name="tester-doer-pattern"></a>Tester-Doer パターン
 メンバーを2つに分割すると、例外スローメンバーのパフォーマンスを向上させることができます。 <xref:System.Collections.Generic.ICollection%601> インターフェイスの <xref:System.Collections.Generic.ICollection%601.Add%2A> メソッドを見てみましょう。

```csharp
ICollection<int> numbers = ...
numbers.Add(1);
```

 コレクションが読み取り専用の場合、メソッド `Add` がスローされます。 これは、メソッドの呼び出しが頻繁に失敗することが予想されるシナリオでパフォーマンスの問題になる可能性があります。 問題を軽減する方法の 1 つは、値を追加する前に、コレクションが書き込み可能かどうかをテストすることです。

```csharp
ICollection<int> numbers = ...
...
if (!numbers.IsReadOnly)
{
    numbers.Add(1);
}
```

 条件をテストするために使用されるメンバー (この例では `IsReadOnly`プロパティは、テスト担当者と呼ばれます)。 スローされる可能性のある操作を実行するために使用するメンバーは、この例の `Add` メソッドを doer と呼びます。

 ✔️、例外に関連するパフォーマンスの問題を回避するために、一般的なシナリオで例外をスローする可能性のあるメンバーに対してテスト担当者のパターンを検討してください。

## <a name="try-parse-pattern"></a>Try-Parse パターン
 パフォーマンスが非常に重要な API では、前のセクションで説明した Tester-Doer パターンよりも高速なパターンを使用する必要があります。 このパターンでは、メンバー名を調整して、適切に定義されたテスト ケースをメンバー セマンティクスの一部にするための呼び出しを行います。 たとえば、<xref:System.DateTime> は、文字列の解析が失敗した場合に例外をスローする <xref:System.DateTime.Parse%2A> メソッドを定義します。 また、解析を試行する対応する <xref:System.DateTime.TryParse%2A> メソッドも定義しますが、解析が失敗した場合は false を返し、`out` パラメーターを使用した解析の成功の結果を返します。

```csharp
public struct DateTime
{
    public static DateTime Parse(string dateTime)
    {
        ...
    }
    public static bool TryParse(string dateTime, out DateTime result)
    {
        ...
    }
}
```

 このパターンを使用する場合は、厳密な用語で try 機能を定義する必要があります。 明確に定義された try 以外の理由でメンバーが失敗した場合でも、メンバーは対応する例外をスローする必要があります。

 ✔️、例外に関連するパフォーマンスの問題を回避するために、一般的なシナリオで例外をスローする可能性のあるメンバーのために、Try 解析パターンを検討してください。

 このパターンを実装するメソッドには、"Try" というプレフィックスとブール型の戻り値の型を使用✔️ます。

 ✔️、Try 解析パターンを使用して、メンバーごとに例外スローメンバーを提供します。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [例外のデザインのガイドライン](../../../docs/standard/design-guidelines/exceptions.md)
