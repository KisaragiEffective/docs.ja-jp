---
title: メンバーのオーバーロード
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- default arguments
- members [.NET Framework], overloaded
- member design guidelines [.NET Framework], overloading
- overloaded members
- signatures, members
ms.assetid: 964ba19e-8b94-4b5b-b1e3-5a0b531a0bb1
ms.openlocfilehash: c9cb178e838aab99c22089b527a6bd2e86b325de
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727840"
---
# <a name="member-overloading"></a>メンバーのオーバーロード
メンバーのオーバーロードとは、同じ型に複数のメンバーを作成し、パラメーターの数または型だけではなく、同じ名前を持つことを意味します。 たとえば、次の例では、`WriteLine` メソッドがオーバーロードされています。

```csharp
public static class Console {
    public void WriteLine();
    public void WriteLine(string value);
    public void WriteLine(bool value);
    ...
}
```

 パラメーターを持つことができるのはメソッド、コンストラクター、およびインデックス付きプロパティだけなので、これらのメンバーのみをオーバーロードできます。

 オーバーロードは、再利用可能なライブラリのユーザビリティ、生産性、および読みやすさを向上させるための最も重要な手法の1つです。 パラメーターの数をオーバーロードすることで、コンストラクターとメソッドのより単純なバージョンを提供できるようになります。 パラメーターの型をオーバーロードすると、選択した異なる型のセットに対して同じ操作を実行するメンバーに対して同じメンバー名を使用できるようになります。

 ✔️は、記述的なパラメーター名を使用して、短いオーバーロードによって使用される既定値を示すようにします。

 ❌ オーバーロード内のパラメーター名を変更しないようにします。 1つのオーバーロード内のパラメーターが、別のオーバーロード内のパラメーターと同じ入力を表している場合、パラメーターの名前は同じである必要があります。

 ❌、オーバーロードされたメンバーのパラメーターの順序に不整合が生じないようにします。 同じ名前のパラメーターは、すべてのオーバーロードで同じ位置に出現します。

 ✔️は、最大のオーバーロード (拡張が必要な場合) のみにしてください。 より短いオーバーロードでは、より長いオーバーロードに対してを呼び出すだけで済みます。

 ❌ は、`ref` または `out` 修飾子を使用してメンバーをオーバーロードしないでください。

 一部の言語では、このようなオーバーロードの呼び出しを解決できません。 また、通常、このようなオーバーロードには完全に異なるセマンティクスがあり、オーバーロードは使用できませんが、2つの異なるメソッドを使用することをお勧めします。

 ❌ には、同じ位置にあるパラメーターを持つオーバーロードと、異なるセマンティクスを持つ類似した型があります。

 ✔️オプションの引数に対して `null` を渡すことを許可します。

 ✔️は、既定の引数を持つメンバーを定義するのではなく、メンバーのオーバーロードを使用します。

 既定の引数は CLS に準拠していません。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [メンバーのデザインのガイドライン](../../../docs/standard/design-guidelines/member.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
