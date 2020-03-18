---
title: 型のメンバーの名前
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- events [.NET Framework], names
- methods [.NET Framework], names
- type members
- properties [.NET Framework], names
- fields, names
- field names
- names [.NET Framework], type members
- members [.NET Framework], type
ms.assetid: af5a0903-36af-4c2a-b848-cf959affeaa5
ms.openlocfilehash: 81c837bd045992043208a59f6ee16803c1d6eb3c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744183"
---
# <a name="names-of-type-members"></a>型のメンバーの名前
型は次のメンバーで構成されています: メソッド、プロパティ、イベント、コンストラクター、フィールド。 次のセクションは、型のメンバーに名前を付けるためのガイドラインを示しています。

## <a name="names-of-methods"></a>メソッドの名前
 メソッドはアクションを実行する手段であるため、デザインのガイドラインでは、メソッド名を動詞または動詞句にする必要があります。 また、このガイドラインに従うと、名詞句または形容詞句であるプロパティ名および型名と、メソッド名を区別するためにも機能します。

 ✔️動詞または動詞句であるメソッド名を指定します。

```csharp
public class String {
    public int CompareTo(...);
    public string[] Split(...);
    public string Trim();
}
```

## <a name="names-of-properties"></a>プロパティの名前
 他のメンバーとは異なり、プロパティには名詞句または形容詞の名前を指定する必要があります。 つまり、プロパティはデータを参照するため、プロパティの名前にはデータが反映されます。 プロパティ名には、常に Pascal 形式が使用されます。

 名詞、名詞句、または形容詞を使用してプロパティに名前を指定する✔️ます。

 次の例のように、"Get" メソッドの名前と一致するプロパティがない ❌ ます。

 `public string TextWriter { get {...} set {...} }` `public string GetTextWriter(int value) { ... }`

 通常、このパターンは、プロパティが実際にメソッドであることを示します。

 コレクションのプロパティには、単数形の語句の後に "List" または "Collection" を続けて使用するのではなく、コレクション内の項目を記述する複数形の名前を付けて✔️します。

 ✔️は、ブール型のプロパティに肯定的な語句 (`CantSeek`ではなく`CanSeek`) を付けます。 必要に応じて、ブール型プロパティの前に "Is"、"has"、または "has" を付けることもできますが、値を追加する場所のみを指定できます。

 ✔️プロパティに型と同じ名前を付けることを検討してください。

 たとえば、次のプロパティは、`Color` という名前の列挙値を適切に取得および設定するため、プロパティは `Color` という名前になります。

```csharp
public enum Color {...}
public class Control {
    public Color Color { get {...} set {...} }
}
```

## <a name="names-of-events"></a>イベントの名前
 イベントは常に、発生中のアクションまたは発生したアクションのいずれかのアクションを参照します。 そのため、メソッドと同様、イベントには動詞の名前が付けられ、イベントが発生した時刻を示すために動詞の時制が使用されます。

 ✔️動詞または動詞句を使用してイベントに名前を付けます。

 例として、`Clicked`、`Painting`、`DroppedDown` などがあります。

 ✔️は、現在と過去の時制を使用して、before と after の概念でイベント名を指定します。

 たとえば、ウィンドウを閉じる前に発生するクローズ イベントは `Closing` と呼ばれ、ウィンドウを閉じた後に発生するクローズ イベントは `Closed` と呼ばれます。

 "Before" または "After" プレフィックスまたは事後修正を使用して、イベントの前後を示す ❌ ことはできません。 前述のように、現在形と過去形を使用します。

 次の例に示すように、イベントハンドラー (イベントの種類として使用されるデリゲート) に "EventHandler" サフィックスを付ける✔️ます。

 `public delegate void ClickedEventHandler(object sender, ClickedEventArgs e);`

 ✔️ `sender` という名前の2つのパラメーターと、イベントハンドラーでの `e` を使用します。

 sender パラメーターは、イベントを発生させたオブジェクトを表します。 より具体的な型を使用できる場合も、通常、sender パラメーターの型は `object` になります。

 "EventArgs" サフィックスを使用して、イベント引数クラスの名前を✔️します。

## <a name="names-of-fields"></a>フィールドの名前
 フィールドの名前付けのガイドラインは、静的パブリック フィールドと保護されたフィールドを対象とします。 内部フィールドとプライベート フィールドは、ガイドラインの対象ではありません。また、パブリック インスタンス フィールドや保護されたインスタンス フィールドは、「[メンバーのデザインのガイドライン](../../../docs/standard/design-guidelines/member.md)」で許可されていません。

 フィールド名の大文字と小文字の区別を✔️します。

 ✔️は、名詞、名詞句、または形容詞を使用してフィールドに名前を指定します。

 ❌ フィールド名にプレフィックスを使用しません。

 たとえば、静的フィールドを示すために、"g_" や "s_" を使用しないでください。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [名前付けのガイドライン](../../../docs/standard/design-guidelines/naming-guidelines.md)
