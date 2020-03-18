---
title: クラス、構造体、およびインターフェイスの名前
ms.date: 10/22/2008
helpviewer_keywords:
- type names, guidelines
- classes [.NET Framework], names
- enumerations [.NET Framework], names
- names [.NET Framework], interfaces
- common type names
- names [.NET Framework], type names
- names [.NET Framework], classes
- interfaces [.NET Framework], names
- generic type parameters
ms.assetid: 87a4b0da-ed64-43b1-ac43-968576c444ce
ms.openlocfilehash: 2c528348c0e84037a80df9797c56f03b51c73adc
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727788"
---
# <a name="names-of-classes-structs-and-interfaces"></a>クラス、構造体、およびインターフェイスの名前
次に示す名前付けのガイドラインは、一般的な型の名前付けに適用されます。

 ✔️は、文字の大文字と小文字の区別を使用して、名詞または名詞句でクラスと構造体に名前を付けます。

 これにより、動詞句を使用して名前が付けられたメソッドの型名が区別されます。

 ✔️には、形容詞のフレーズを使用するか、名詞や名詞句を使用することもあります。

 名詞と名詞句を使用することはほとんどなく、型がインターフェイスではなく抽象クラスであることを示している可能性があります。

 ❌ は、クラス名にプレフィックスを付けません (例: "C")。

 ✔️派生クラスの名前を基底クラスの名前で終了することを検討してください。

 これは非常に読みやすく、リレーションシップについて明確に説明しています。 このコードの例としては、`ArgumentOutOfRangeException`があります。これは `Exception`の一種であり、`SerializableAttribute`であり、`Attribute`です。 ただし、このガイドラインを適用するには、適切な判断を使用することが重要です。たとえば、`Button` クラスは `Control` イベントの一種ですが、名前に `Control` は表示されません。

 型がインターフェイスであることを示すには、インターフェイス名の前に I という文字を付ける✔️ます。

 たとえば、`IComponent` (わかりやすい名詞)、`ICustomAttributeProvider` (名詞句)、および `IPersistable` (形容詞) は、適切なインターフェイス名です。 他の型名と同様に、省略形を避けます。

 クラスがインターフェイスの標準実装であるクラスインターフェイスのペアを定義する場合は、名前がインターフェイス名の "I" プレフィックスによってのみ異なることを✔️してください。

## <a name="names-of-generic-type-parameters"></a>ジェネリック型パラメーターの名前
 ジェネリックが .NET Framework 2.0 に追加されました。 この機能では、*型パラメーター*と呼ばれる新しい種類の識別子が導入されました。

 1文字の名前が完全に記述されており、わかりやすい名前で値が追加されない場合を除き、ジェネリック型パラメーターにわかりやすい名前を付ける✔️ます。

 ✔️1つの1文字の型パラメーターを持つ型の型パラメーター名として `T` を使用することを検討してください。

```csharp
public int IComparer<T> { ... }
public delegate bool Predicate<T>(T item);
public struct Nullable<T> where T:struct { ... }
```

 ✔️は、わかりやすい型パラメーター名を `T`でプレフィックスします。

```csharp
public interface ISessionChannel<TSession> where TSession : ISession {
    TSession Session { get; }
}
```

 パラメーターの名前の型パラメーターに設定されている制約を示す✔️ます。

 たとえば、`ISession` に制約されたパラメーターは `TSession`と呼ばれる場合があります。

## <a name="names-of-common-types"></a>共通型の名前
 ✔️は、次の表に記載されているガイドラインに従って、特定の .NET Framework 型から派生した型に名前を付けることができます。

|Base Type|派生/実装型のガイドライン|
|---------------|------------------------------------------|
|`System.Attribute`|カスタム属性クラスの名前にサフィックス "Attribute" を追加✔️ます。|
|`System.Delegate`|✔️は、イベントで使用されるデリゲートの名前に "EventHandler" というサフィックスを追加します。<br /><br /> イベントハンドラーとして使用されていないデリゲートの名前に "Callback" というサフィックスを追加✔️ます。<br /><br /> ❌、デリゲートにサフィックス "Delegate" を追加しないでください。|
|`System.EventArgs`|サフィックス "EventArgs" を追加✔️ます。|
|`System.Enum`|❌ は、このクラスから派生しません。代わりに、お使いの言語でサポートされているキーワードを使用してください。たとえば、でC#は、`enum` キーワードを使用します。<br /><br /> ❌ サフィックス "Enum" または "Flag" は追加しないでください。|
|`System.Exception`|サフィックス "Exception" を追加✔️ます。|
|`IDictionary` <br /> `IDictionary<TKey,TValue>`|サフィックス "Dictionary" を追加✔️ます。 `IDictionary` は特定の種類のコレクションであることに注意してくださいが、このガイドラインは、次に示す一般的なコレクションのガイドラインよりも優先されます。|
|`IEnumerable` <br /> `ICollection` <br /> `IList` <br /> `IEnumerable<T>` <br /> `ICollection<T>` <br /> `IList<T>`|サフィックス "Collection" を追加✔️ます。|
|`System.IO.Stream`|サフィックス "Stream" を追加✔️ます。|
|`CodeAccessPermission IPermission`|✔️サフィックス "Permission" を追加します。|

## <a name="naming-enumerations"></a>列挙型の名前付け
 一般的に、列挙型の名前 (列挙型とも呼ばれます) は、標準的な型の名前付け規則に従う必要があります。 ただし、列挙型に特に適用される追加のガイドラインがあります。

 ✔️値がビットフィールドでない限り、列挙体には単数型名を使用します。

 ✔️は、値としてビットフィールドを持つ列挙体に対して複数形の型名を使用します (flags enum とも呼ばれます)。

 列挙型の名前に "Enum" サフィックスを使用 ❌ ことはできません。

 enum 型の名前に "Flag" または "Flags" サフィックスを使用しない ❌。

 列挙値の名前にプレフィックスを使用しないでください (たとえば、ADO 列挙型の場合は "ad"、リッチテキスト列挙型の場合は "rtf" など)。 ❌

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [名前付けのガイドライン](../../../docs/standard/design-guidelines/naming-guidelines.md)
